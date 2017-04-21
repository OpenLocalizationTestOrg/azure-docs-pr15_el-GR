Μας διακριτικού cache θα πρέπει να λειτουργεί σε μια απλή υπόθεση, αλλά, τι συμβαίνει όταν το διακριτικό έχει λήξει ή έχει ανακληθεί; Το διακριτικό θα μπορούσε να λήξει, όταν η εφαρμογή δεν εκτελείται. Αυτό σημαίνει το διακριτικό cache δεν είναι έγκυρο. Το διακριτικό επίσης μπορεί να λήξει ενώ εκτελείται η εφαρμογή. Το αποτέλεσμα είναι μια κατάσταση HTTP κωδικός 401 "χωρίς εξουσιοδότηση". 

Πρέπει να είναι σε θέση να εντοπίσει ένα διακριτικό έχει λήξει και να την ανανεώσετε. Για να το κάνετε αυτό χρησιμοποιούμε μια [ServiceFilter](http://dl.windowsazure.com/androiddocs/com/microsoft/windowsazure/mobileservices/ServiceFilter.html) από τη [βιβλιοθήκη Android προγράμματος-πελάτη](http://dl.windowsazure.com/androiddocs/).

Σε αυτήν την ενότητα ορίζετε μια ServiceFilter που θα εντοπίσει μια απόκριση 401 κωδικός κατάστασης HTTP και ενεργοποιεί μια ανανέωση των το διακριτικό και του cache του διακριτικού. Επιπλέον, αυτό ServiceFilter αποκλείει άλλες αιτήσεις εξαγωγής κατά τον έλεγχο ταυτότητας, έτσι ώστε αυτές τις αιτήσεις μπορούν να χρησιμοποιήσουν το διακριτικό ανανεωμένα.

1. Ανοίξτε το αρχείο ToDoActivity.java και προσθέστε τις ακόλουθες δηλώσεις εισαγωγής:
 
        import java.util.concurrent.atomic.AtomicBoolean;
        import java.util.concurrent.ExecutionException;

        import com.microsoft.windowsazure.mobileservices.MobileServiceException;
 
2. Τα ακόλουθα μέλη για να προσθέσετε το `ToDoActivity` τάξης. 

        public boolean bAuthenticating = false;
        public final Object mAuthenticationLock = new Object();

    Αυτά θα χρησιμοποιηθεί για να συγχρονίσετε τον έλεγχο ταυτότητας του χρήστη. Θέλουμε μόνο για τον έλεγχο ταυτότητας μόνο μία φορά. Τυχόν κλήσεις στη διάρκεια ενός ελέγχου ταυτότητας θα πρέπει να περιμένετε και χρησιμοποιήστε το νέο κωδικό από τον έλεγχο ταυτότητας σε εξέλιξη.

3. Στο αρχείο ToDoActivity.java, προσθέστε την ακόλουθη μέθοδο για την τάξη ToDoActivity που θα χρησιμοποιηθεί για να αποκλείσετε εξερχόμενες κλήσεις σε άλλα νήματα κατά τον έλεγχο ταυτότητας βρίσκεται σε εξέλιξη.

        /**
         * Detects if authentication is in progress and waits for it to complete. 
         * Returns true if authentication was detected as in progress. False otherwise.
         */
        public boolean detectAndWaitForAuthentication()
        {
            boolean detected = false;
            synchronized(mAuthenticationLock)
            {
                do
                {
                    if (bAuthenticating == true)
                        detected = true;
                    try
                    {
                        mAuthenticationLock.wait(1000);
                    }
                    catch(InterruptedException e)
                    {}
                }
                while(bAuthenticating == true);
            }
            if (bAuthenticating == true)
                return true;
            
            return detected;
        }
        

4. Στο αρχείο ToDoActivity.java, προσθέστε την ακόλουθη μέθοδο για να την κλάση ToDoActivity. Αυτή η μέθοδος ενεργοποιεί την αναμονή και, στη συνέχεια, ενημερώστε το διακριτικό σε αιτήσεις εξαγωγής όταν ολοκληρωθεί ο έλεγχος ταυτότητας. 

        
        /**
         * Waits for authentication to complete then adds or updates the token 
         * in the X-ZUMO-AUTH request header.
         * 
         * @param request
         *            The request that receives the updated token.
         */
        private void waitAndUpdateRequestToken(ServiceFilterRequest request)
        {
            MobileServiceUser user = null;
            if (detectAndWaitForAuthentication())
            {
                user = mClient.getCurrentUser();
                if (user != null)
                {
                    request.removeHeader("X-ZUMO-AUTH");
                    request.addHeader("X-ZUMO-AUTH", user.getAuthenticationToken());
                }
            }
        }


5. Στο αρχείο ToDoActivity.java, ενημερώστε το `authenticate` μέθοδο ToDoActivity την κλάση ώστε να δέχεται δυαδικής τιμής παραμέτρου για να επιτρέψετε να επιβάλετε την ανανέωση της μνήμης cache διακριτικού και διακριτικού. Πρέπει επίσης να ειδοποιήσετε οποιαδήποτε αποκλεισμένων νήματα όταν ολοκληρωθεί ο έλεγχος ταυτότητας, ώστε να τους να σηκώστε το νέο κωδικό.

        /**
         * Authenticates with the desired login provider. Also caches the token. 
         * 
         * If a local token cache is detected, the token cache is used instead of an actual 
         * login unless bRefresh is set to true forcing a refresh.
         * 
         * @param bRefreshCache
         *            Indicates whether to force a token refresh. 
         */
        private void authenticate(boolean bRefreshCache) {
            
            bAuthenticating = true;
            
            if (bRefreshCache || !loadUserTokenCache(mClient))
            {
                // New login using the provider and update the token cache.
                mClient.login(MobileServiceAuthenticationProvider.MicrosoftAccount,
                        new UserAuthenticationCallback() {
                            @Override
                            public void onCompleted(MobileServiceUser user,
                                    Exception exception, ServiceFilterResponse response) {
    
                                synchronized(mAuthenticationLock)
                                {
                                    if (exception == null) {
                                        cacheUserToken(mClient.getCurrentUser());
                                        createTable();
                                    } else {
                                        createAndShowDialog(exception.getMessage(), "Login Error");
                                    }
                                    bAuthenticating = false;
                                    mAuthenticationLock.notifyAll();
                                }
                            }
                        });
            }
            else
            {
                // Other threads may be blocked waiting to be notified when 
                // authentication is complete.
                synchronized(mAuthenticationLock)
                {
                    bAuthenticating = false;
                    mAuthenticationLock.notifyAll();
                }
                createTable();
            }
        }   



6. Στο αρχείο ToDoActivity.java, προσθέστε αυτόν τον κωδικό για ένα νέο `RefreshTokenCacheFilter` κλάσης μέσα την κλάση ToDoActivity:

        /**
        * The RefreshTokenCacheFilter class filters responses for HTTP status code 401. 
         * When 401 is encountered, the filter calls the authenticate method on the 
         * UI thread. Out going requests and retries are blocked during authentication. 
         * Once authentication is complete, the token cache is updated and 
         * any blocked request will receive the X-ZUMO-AUTH header added or updated to 
         * that request.   
         */
        private class RefreshTokenCacheFilter implements ServiceFilter {
         
            AtomicBoolean mAtomicAuthenticatingFlag = new AtomicBoolean();                     
            
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(
                    final ServiceFilterRequest request, 
                    final NextServiceFilterCallback nextServiceFilterCallback
                    )
            {
                // In this example, if authentication is already in progress we block the request
                // until authentication is complete to avoid unnecessary authentications as 
                // a result of HTTP status code 401. 
                // If authentication was detected, add the token to the request.
                waitAndUpdateRequestToken(request);
     
                // Send the request down the filter chain
                // retrying up to 5 times on 401 response codes.
                ListenableFuture<ServiceFilterResponse> future = null;
                ServiceFilterResponse response = null;
                int responseCode = 401;
                for (int i = 0; (i < 5 ) && (responseCode == 401); i++)
                {
                    future = nextServiceFilterCallback.onNext(request);
                    try {
                        response = future.get();
                        responseCode = response.getStatus().getStatusCode();
                    } catch (InterruptedException e) {
                       e.printStackTrace();
                    } catch (ExecutionException e) {
                        if (e.getCause().getClass() == MobileServiceException.class)
                        {
                            MobileServiceException mEx = (MobileServiceException) e.getCause();
                            responseCode = mEx.getResponse().getStatus().getStatusCode();
                            if (responseCode == 401)
                            {
                                // Two simultaneous requests from independent threads could get HTTP status 401. 
                                // Protecting against that right here so multiple authentication requests are
                                // not setup to run on the UI thread.
                                // We only want to authenticate once. Requests should just wait and retry 
                                // with the new token.
                                if (mAtomicAuthenticatingFlag.compareAndSet(false, true))                                                                                                      
                                {
                                    // Authenticate on UI thread
                                    runOnUiThread(new Runnable() {
                                        @Override
                                        public void run() {
                                            // Force a token refresh during authentication.
                                            authenticate(true);
                                        }
                                    });
                                }
    
                                // Wait for authentication to complete then update the token in the request. 
                                waitAndUpdateRequestToken(request);
                                mAtomicAuthenticatingFlag.set(false);                                                  
                            }
                        }
                    }
                }
                return future;
            }
        }


    Αυτό το φίλτρο υπηρεσία θα ελέγξει κάθε απόκριση για κωδικός κατάστασης HTTP 401 "χωρίς εξουσιοδότηση". Εάν μια 401 σφάλμα, μια νέα πρόσκληση σε σύνδεσης για να αποκτήσετε έναν νέο κωδικό θα ρύθμισης στο νήμα περιβάλλοντος εργασίας Χρήστη. Άλλες κλήσεις θα αποκλειστούν μέχρι να ολοκληρωθεί η σύνδεση ή μέχρι να απέτυχε 5 προσπάθειες. Εάν το νέο διακριτικό λαμβάνεται, την αίτηση που την ενεργοποίησε τη 401 θα επαναληφθεί με το νέο κωδικό και οποιαδήποτε αποκλεισμένων κλήσεις θα επαναληφθεί με το νέο κωδικό. 

7. Στο αρχείο ToDoActivity.java, προσθέστε αυτόν τον κωδικό για ένα νέο `ProgressFilter` κλάσης μέσα την κλάση ToDoActivity:
        
        /**
        * The ProgressFilter class renders a progress bar on the screen during the time the App is waiting for the response of a previous request.
        * the filter shows the progress bar on the beginning of the request, and hides it when the response arrived.
        */
        private class ProgressFilter implements ServiceFilter {
            @Override
            public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback nextServiceFilterCallback) {

                final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();

                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
                    }
                });

                ListenableFuture<ServiceFilterResponse> future = nextServiceFilterCallback.onNext(request);

                Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
                    @Override
                    public void onFailure(Throwable e) {
                        resultFuture.setException(e);
                    }

                    @Override
                    public void onSuccess(ServiceFilterResponse response) {
                        runOnUiThread(new Runnable() {

                            @Override
                            public void run() {
                                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.GONE);
                            }
                        });

                        resultFuture.set(response);
                    }
                });

                return resultFuture;
            }
        }
        
    Αυτό το φίλτρο θα εμφανιστεί η γραμμή προόδου στην αρχή της αίτησης και θα αποκρύψετε όταν έρθει η απόκριση.

8. Στο αρχείο ToDoActivity.java, ενημερώστε το `onCreate` μέθοδο ως εξής:

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.activity_to_do);
            mProgressBar = (ProgressBar) findViewById(R.id.loadingProgressBar);
        
            // Initialize the progress bar
            mProgressBar.setVisibility(ProgressBar.GONE);
        
            try {
                // Create the Mobile Service Client instance, using the provided
                // Mobile Service URL and key
                mClient = new MobileServiceClient(
                        "https://<YOUR MOBILE SERVICE>.azure-mobile.net/",
                        "<YOUR MOBILE SERVICE KEY>", this)
                           .withFilter(new ProgressFilter())
                           .withFilter(new RefreshTokenCacheFilter());
            
                // Authenticate passing false to load the current token cache if available.
                authenticate(false);
                    
            } catch (MalformedURLException e) {
                createAndShowDialog(new Exception("Error creating the Mobile Service. " +
                    "Verify the URL"), "Error");
            }
        }


       In this code, `RefreshTokenCacheFilter` is used in addition to `ProgressFilter`. Also during `onCreate` we want to load the token cache. So `false` is passed in to the `authenticate` method.



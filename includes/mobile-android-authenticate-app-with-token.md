
Το προηγούμενο παράδειγμα εμφάνιζε μια τυπική εισόδου, γεγονός που απαιτεί το πρόγραμμα-πελάτη να επικοινωνήσετε με την υπηρεσία παροχής ταυτότητας και υπόβαθρο Azure service κάθε φορά που ξεκινά η εφαρμογή. Όχι μόνο αυτή η μέθοδος είναι αποτελεσματική, μπορεί να αντιμετωπίσετε θέματα που σχετίζονται με τη χρήση πρέπει να πολλοί πελάτες προσπαθήσετε να εκκινήσετε εφαρμογή την ίδια στιγμή. Μια καλύτερη προσέγγιση είναι να cache το διακριτικό εξουσιοδότησης που επιστράφηκε από την υπηρεσία Azure και προσπαθήστε να χρησιμοποιήσετε αυτό το πρώτο πριν από τη χρήση μιας υπηρεσίας παροχής βάσει εισόδου. 

>[AZURE.NOTE]Μπορείτε να αποθηκεύσετε προσωρινά το διακριτικό εκδοθεί από υπολογιστή στο παρασκήνιο Azure service ανεξάρτητα από το εάν χρησιμοποιείτε τον έλεγχο ταυτότητας διαχειριζόμενο από το πρόγραμμα-πελάτη ή διαχείριση υπηρεσίας. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί διαχείριση υπηρεσίας ελέγχου ταυτότητας.


1. Ανοίξτε το αρχείο ToDoActivity.java και προσθέστε τις ακόλουθες δηλώσεις εισαγωγής:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

2. Προσθέστε το τα ακόλουθα μέλη για να το `ToDoActivity` τάξης.

        public static final String SHAREDPREFFILE = "temp"; 
        public static final String USERIDPREF = "uid";  
        public static final String TOKENPREF = "tkn";   


3. Στο αρχείο ToDoActivity.java, προσθέστε το τον ακόλουθο ορισμό για το `cacheUserToken` μέθοδος.
 
        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }   
  
    Αυτή η μέθοδος αποθηκεύει το αναγνωριστικό χρήστη και το διακριτικό σε ένα αρχείο προτίμηση που έχει επισημανθεί ως ιδιωτικό. Αυτό θα πρέπει να προστατεύσετε την πρόσβαση στο cache, έτσι ώστε οι άλλες εφαρμογές στη συσκευή δεν έχουν πρόσβαση στο το διακριτικό, επειδή η προτίμηση είναι φιλτραρισμένο για την εφαρμογή. Ωστόσο, εάν κάποιος αποκτήσει πρόσβαση στη συσκευή, είναι δυνατό ώστε αυτές να αποκτήσει πρόσβαση στο cache διακριτικού με άλλο τρόπο. 

    >[AZURE.NOTE]Μπορείτε να προστατεύσετε το διακριτικό με κρυπτογράφηση περαιτέρω εάν θεωρείται ιδιαίτερα ευαίσθητα διακριτικού πρόσβαση στα δεδομένα σας και κάποιος μπορεί να αποκτήσει πρόσβαση στη συσκευή. Ωστόσο, μια πλήρως ασφαλής λύση είναι πέρα από το πεδίο αυτού του προγράμματος εκμάθησης και εξαρτώνται από τις απαιτήσεις ασφαλείας.


4. Στο αρχείο ToDoActivity.java, προσθέστε το τον ακόλουθο ορισμό για το `loadUserTokenCache` μέθοδο.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null); 
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null); 
            if (token == null)
                return false;
                
            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);
                
            return true;
        }



5. Στο αρχείο *ToDoActivity.java* , αντικαταστήστε το `authenticate` μέθοδο με την ακόλουθη μέθοδο που χρησιμοποιεί ένα διακριτικού cache. Αλλάξτε την υπηρεσία παροχής σύνδεσης εάν θέλετε να χρησιμοποιήσετε ένα λογαριασμό διαφορετικό από το Google.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
        
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();  
                    }
                });
            }
        }

6. Δημιουργήστε την εφαρμογή και έλεγχος ελέγχου ταυτότητας χρησιμοποιώντας έναν έγκυρο λογαριασμό. Εκτελέστε τουλάχιστον δύο φορές. Κατά την πρώτη εκτέλεση του, θα πρέπει να λαμβάνετε ένα μήνυμα για να συνδεθείτε και να δημιουργήσετε το διακριτικό cache. Μετά από αυτό, κάθε εκτέλεση θα επιχειρήσει να φορτώσετε το διακριτικό cache για τον έλεγχο ταυτότητας και δεν θα πρέπει να απαιτείται για τη σύνδεση.




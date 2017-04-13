
1. Στην **Εξερεύνηση έργου** στο Android Studio, ανοίξτε το αρχείο ToDoActivity.java και προσθέστε τις παρακάτω προτάσεις εισαγωγή.

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. Προσθέστε την ακόλουθη μέθοδο για την τάξη **ToDoActivity** : 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    Αυτό δημιουργεί μια νέα μέθοδο χειρισμού της διαδικασίας ελέγχου ταυτότητας. Έλεγχος ταυτότητας του χρήστη, χρησιμοποιώντας μια σύνδεση Google. Εμφανίζεται ένα παράθυρο διαλόγου που εμφανίζει το Αναγνωριστικό του χρήστη με έλεγχο ταυτότητας. Δεν μπορείτε να συνεχίσετε χωρίς ένα θετικό έλεγχο ταυτότητας.

    > [AZURE.NOTE] Εάν χρησιμοποιείτε μια υπηρεσία παροχής ταυτότητας εκτός από το Google, αλλάξτε την τιμή που του μεταβιβάστηκε την παραπάνω μέθοδο **σύνδεσης** σε ένα από τα εξής: _διαθέσιμος MicrosoftAccount_, _Facebook_, _Twitter_ή _windowsazureactivedirectory_.

3. Στη μέθοδο **onCreate** , προσθέστε την ακόλουθη γραμμή του κώδικα μετά τον κωδικό που εμφανίζει το `MobileServiceClient` αντικειμένου.

        authenticate();

    Αυτή η κλήση ξεκινά τη διεργασία ελέγχου ταυτότητας.

4. Μετακινήστε το υπόλοιπο κωδικό μετά `authenticate();` στη μέθοδο **onCreate** σε μια νέα μέθοδο **createTable** , που έχει την παρακάτω μορφή:

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9. Από το μενού **Εκτέλεση** , στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση εφαρμογής** για να ξεκινήσετε την εφαρμογή και συνδεθείτε με την υπηρεσία παροχής ταυτότητας επιλογής. 

    Όταν είστε με επιτυχία συνδεδεμένοι στο, η εφαρμογή θα πρέπει να εκτελούνται χωρίς σφάλματα και θα πρέπει να ερωτήματος στην υπηρεσία υποστήριξης και να κάνετε ενημερώσεις στα δεδομένα.
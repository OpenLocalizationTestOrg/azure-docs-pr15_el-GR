
1. Στη λύση προβολή (ή **Εξερεύνηση λύσεων** στο Visual Studio), κάντε δεξί κλικ στο φάκελο **στοιχεία** , κάντε κλικ στην επιλογή **Λήψη περισσότερα στοιχεία...**, αναζητήστε το στοιχείο **Πελάτη ανταλλαγής μηνυμάτων Google Cloud** και προσθέστε το στο έργο.

2. Ανοίξτε το αρχείο έργου ToDoActivity.cs και προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση για την τάξη:

        using Gcm.Client;

3. Στην τάξη **ToDoActivity** , προσθέστε τον ακόλουθο κώδικα νέο: 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    Αυτό σας επιτρέπει να αποκτήσετε πρόσβαση την παρουσία φορητός υπολογιστής-πελάτης από τη διεργασία push πρόγραμμα χειρισμού της υπηρεσίας.

4.  Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **OnCreate** , αφού δημιουργηθεί το **MobileServiceClient** :

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

Το **ToDoActivity** είναι τώρα έτοιμη για να προσθέσετε τις ειδοποιήσεις προώθησης.
4. Δημιουργία μιας νέας κλάσης του έργου που ονομάζεται `ToDoBroadcastReceiver`.

5. Προσθέστε τα ακόλουθα χρήση προτάσεων για την τάξη **ToDoBroadcastReceiver** :

        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

6. Προσθέστε τις ακόλουθες αιτήσεις δικαιωμάτων μεταξύ των καταστάσεων **χρησιμοποιώντας** και τη δήλωση **χώρου ονομάτων** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

7. Αντικαταστήστε το υπάρχον ορισμό κλάσης **ToDoBroadcastReceiver** με τα εξής:
 
        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, 
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, 
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, 
        Categories = new string[] { "@PACKAGE_NAME@" })]
        public class ToDoBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            // Set the Google app ID.
            public static string[] senderIDs = new string[] { "<PROJECT_NUMBER>" };
        }

    Στο παραπάνω κώδικα, πρέπει να αντικαταστήσετε _`<PROJECT_NUMBER>`_ με τον αριθμό έργου που έχουν εκχωρηθεί από το Google κατά την παροχή της υπηρεσίας την εφαρμογή σας στην πύλη για προγραμματιστές του Google. 

8. Στο αρχείο έργου ToDoBroadcastReceiver.cs, προσθέστε τον ακόλουθο κώδικα που καθορίζει την κλάση **PushHandlerService** :
 
        // The ServiceAttribute must be applied to the class.
        [Service] 
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
 
            public PushHandlerService() : base(ToDoBroadcastReceiver.senderIDs) { }
        }

    Σημειώστε ότι αυτή η κλάση προέρχεται από **GcmServiceBase** και ότι το χαρακτηριστικό **υπηρεσίας** πρέπει να εφαρμοστεί σε αυτήν την κλάση.

    >[AZURE.NOTE]Η κλάση **GcmServiceBase** εφαρμόζει το **OnRegistered()**, **OnUnRegistered()**, **OnMessage()** και **OnError()** μεθόδους. Πρέπει να παρακάμψετε τις παρακάτω μεθόδους στην τάξη **PushHandlerService** .

5. Προσθέστε τον ακόλουθο κώδικα για την τάξη **PushHandlerService** που παρακάμπτει το πρόγραμμα χειρισμού συμβάντων **OnRegistered** . 

        protected override void OnRegistered(Context context, string registrationId)
        {
            System.Diagnostics.Debug.WriteLine("The device has been registered with GCM.", "Success!");

            // Get the MobileServiceClient from the current activity instance.
            MobileServiceClient client = ToDoActivity.CurrentActivity.CurrentClient;
            var push = client.GetPush();

            // Define a message body for GCM.
            const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

            // Define the template registration as JSON.
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
              {"body", templateBodyGCM }
            };

            try
            {
                // Make sure we run the registration on the same thread as the activity, 
                // to avoid threading errors.
                ToDoActivity.CurrentActivity.RunOnUiThread(

                    // Register the template with Notification Hubs.
                    async () => await push.RegisterAsync(registrationId, templates));
                
                System.Diagnostics.Debug.WriteLine(
                    string.Format("Push Installation Id", push.InstallationId.ToString()));
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(
                    string.Format("Error with Azure push registration: {0}", ex.Message));
            }
        }

    Αυτή η μέθοδος χρησιμοποιεί το επιστρεφόμενο Αναγνωριστικό δήλωσης GCM για να καταχωρήσετε με Azure για τις ειδοποιήσεις προώθησης. Οι ετικέτες μπορούν να προστεθούν μόνο για την καταχώρηση, αφού δημιουργηθεί. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ΔΙΑΔΙΚΑΣΙΕΣ: Προσθήκη ετικετών σε μια συσκευή εγκατάσταση για να ενεργοποιήσετε την push-ετικέτες](../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags).

10. Παράκαμψη της μεθόδου **OnMessage** στο **PushHandlerService** με τον ακόλουθο κώδικα:

        protected override void OnMessage(Context context, Intent intent)
        {          
            string message = string.Empty;

            // Extract the push notification message from the intent.
            if (intent.Extras.ContainsKey("message"))
            {
                message = intent.Extras.Get("message").ToString();
                var title = "New item added:";

                // Create a notification manager to send the notification.
                var notificationManager = 
                    GetSystemService(Context.NotificationService) as NotificationManager;

                // Create a new intent to show the notification in the UI. 
                PendingIntent contentIntent = 
                    PendingIntent.GetActivity(context, 0, 
                    new Intent(this, typeof(ToDoActivity)), 0);           

                // Create the notification using the builder.
                var builder = new Notification.Builder(context);
                builder.SetAutoCancel(true);
                builder.SetContentTitle(title);
                builder.SetContentText(message);
                builder.SetSmallIcon(Resource.Drawable.ic_launcher);
                builder.SetContentIntent(contentIntent);
                var notification = builder.Build();

                // Display the notification in the Notifications Area.
                notificationManager.Notify(1, notification);

            }
        }

12. Παρακάμπτουν τις μεθόδους **OnUnRegistered()** και **OnError()** με τον ακόλουθο κώδικα.

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            throw new NotImplementedException();
        }

        protected override void OnError(Context context, string errorId)
        {
            System.Diagnostics.Debug.WriteLine(
                string.Format("Error occurred in the notification: {0}.", errorId));
        }
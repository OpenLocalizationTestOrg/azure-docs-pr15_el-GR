<properties
    pageTitle="Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Forms | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε τις υπηρεσίες Azure για την αποστολή ειδοποιήσεων push πολλών πλατφόρμα στις εφαρμογές Xamarin.Forms."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Προσθέστε τις ειδοποιήσεις push σας εφαρμογή Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Επισκόπηση

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε push ειδοποιήσεων για όλα τα έργα προέκυψε από [Xamarin.Forms Γρήγορη εκκίνηση](app-service-mobile-xamarin-forms-get-started.md) , ώστε να αποστέλλεται μια ειδοποίηση push για όλους τους πελάτες πλατφόρμες κάθε φορά που έχει εισαχθεί μια εγγραφή.

Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, θα χρειαστεί το πακέτο επέκτασης ειδοποιήσεων push. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Για iOS, θα χρειαστείτε μια [ιδιότητα μέλους Apple προγραμματιστής πρόγραμμα](https://developer.apple.com/programs/ios/) και μια συσκευή iOS φυσικής επειδή το [iOS simulator δεν υποστηρίζει τις ειδοποιήσεις προώθησης](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Ρύθμιση παραμέτρων διανομέα ειδοποίησης

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Ενημέρωση του project server για την αποστολή ειδοποιήσεων push

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και εκτέλεση του Android έργου

Ολοκληρώστε αυτήν την ενότητα για να ενεργοποιήσετε τις ειδοποιήσεις push για το έργο Xamarin.Forms Droid για Android.


###<a name="enable-firebase-cloud-messaging-fcm"></a>Ενεργοποίηση Firebase Cloud ανταλλαγής μηνυμάτων (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Ρύθμιση παραμέτρων υπόβαθρο εφαρμογή Mobile για να στείλετε προσκλήσεις push χρησιμοποιώντας FCM

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Προσθέστε τις ειδοποιήσεις push στο Android έργο

Με το παρασκηνίου έχουν ρυθμιστεί με FCM, μπορεί να προσθέσουμε στοιχεία και τους κωδικούς στον υπολογιστή-πελάτη για την καταχώρηση με FCM, εγγραφείτε για τις ειδοποιήσεις push με διανομείς ειδοποίηση Azure μέσω υπολογιστή στο παρασκήνιο εφαρμογής για κινητές συσκευές και λάβετε ειδοποιήσεις.

1. Στο έργο **Droid** , κάντε δεξί κλικ στο φάκελο **στοιχεία** , κάντε κλικ στην επιλογή **Λήψη περισσότερα στοιχεία...**, αναζητήστε το στοιχείο **Πελάτη ανταλλαγής μηνυμάτων Google Cloud** και προσθέστε το στο έργο. Αυτό το στοιχείο υποστηρίζει τις ειδοποιήσεις push για ένα έργο Xamarin Android.


2. Ανοίξτε το αρχείο έργου MainActivity.cs και προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση στο επάνω μέρος του αρχείου:

        using Gcm.Client;

3. Προσθέστε τον ακόλουθο κώδικα στη μέθοδο **OnCreate** μετά την κλήση σε **LoadApplication**:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Προσθέστε μια νέα μέθοδο Βοήθειας **CreateAndShowDialog** , ως εξής:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Προσθέστε τον ακόλουθο κώδικα για την τάξη **MainActivity** :

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Αυτό εμφανίζει την τρέχουσα παρουσία **MainActivity** , ώστε να σας μπορεί να εκτελέσει στο κύριο νήμα περιβάλλοντος εργασίας Χρήστη.

6. Προετοιμασία του `instance`, μεταβλητά στην αρχή της μεθόδου **OnCreate** , ως εξής.

        // Set the current instance of MainActivity.
        instance = this;

2. Προσθέστε ένα νέο αρχείο κλάσης στο έργο **Droid** με το όνομα `GcmService.cs`, και βεβαιωθείτε ότι υπάρχουν οι παρακάτω προτάσεις **Χρήση** στο επάνω μέρος του αρχείου:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Προσθέστε τις ακόλουθες αιτήσεις δικαιωμάτων στο επάνω μέρος του αρχείου, μετά τις προτάσεις **χρησιμοποιώντας** και πριν από τη δήλωση **χώρου ονομάτων** .

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Προσθέστε τον ακόλουθο ορισμό κλάσης χώρου ονομάτων. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Αντικαταστήστε **< PROJECT_NUMBER >** με τον αριθμό του έργου σας σημειώσατε προηγουμένως.   

11. Αντικαταστήστε το κενό τάξη **GcmService** με τον ακόλουθο κώδικα, το οποίο χρησιμοποιεί το νέο δέκτη εκπομπής:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Προσθέστε τον ακόλουθο κώδικα για την τάξη **GcmService** που παρακάμπτει το πρόγραμμα χειρισμού συμβάντων **OnRegistered** και υλοποιεί μια μέθοδο **καταχώρηση** .

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Προσθέστε τον ακόλουθο κώδικα που υλοποιεί **OnMessage**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Αυτό χειρίζεται τις εισερχόμενες ειδοποιήσεις και στείλτε τους για τη Διαχείριση ειδοποιήσεων για να εμφανίζονται.

14. **GcmServiceBase** απαιτεί επίσης να υλοποιήσετε το μεθόδους πρόγραμμα χειρισμού **OnUnRegistered** και **OnError** , οι οποίες μπορείτε να κάνετε ως εξής:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Τώρα, είστε έτοιμοι δοκιμής τις ειδοποιήσεις push στην εφαρμογή εκτελείται σε μια συσκευή Android ή το προσομοιωτή.

###<a name="test-push-notifications-in-your-android-app"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή Android

Τα πρώτα δύο βήματα απαιτούνται μόνο κατά τη δοκιμή σε ένα προσομοίωσης.

1. Βεβαιωθείτε ότι για την ανάπτυξη στον ή τον εντοπισμό σφαλμάτων σε μια εικονική συσκευή που έχει APIs Google Ορισμός ως προορισμού, όπως φαίνεται παρακάτω στη Διαχείριση Android εικονική συσκευή (AVD).

2. Προσθέστε ένα λογαριασμό Google στη συσκευή Android, κάνοντας κλικ στην επιλογή **εφαρμογές** > **Ρυθμίσεις** > **Προσθήκη λογαριασμού**, στη συνέχεια, ακολουθήστε τις οδηγίες για να χρησιμοποιήσετε προσθέσετε έναν υπάρχοντα λογαριασμό Google στη συσκευή για να δημιουργήσετε ένα νέο.

1. Στο Visual Studio ή Xamarin Studio, κάντε δεξί κλικ στο έργο **Droid** και κάντε κλικ στην επιλογή **Ορισμός ως εκκίνησης έργου**.

2. Πατήστε το κουμπί **Εκτέλεση** για να δημιουργήσετε το έργο και ξεκινήστε την εφαρμογή σε συσκευή Android ή προσομοίωσης.

3. Στην εφαρμογή, πληκτρολογήστε μια εργασία και, στη συνέχεια, κάντε κλικ στο κουμπί συν (**+**) εικονίδιο.

4. Βεβαιωθείτε ότι έχει ληφθεί μια ειδοποίηση όταν προστίθεται ένα στοιχείο.


##<a name="optional-configure-and-run-the-ios-project"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και να εκτελέσετε το έργο iOS

Αυτή η ενότητα είναι για την εκτέλεση του έργου iOS Xamarin για συσκευές iOS. Μπορείτε να παραλείψετε αυτή την ενότητα, εάν δεν εργάζεστε με συσκευές iOS.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Ρύθμιση παραμέτρων ενότητα ειδοποίησης για APNS

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Στη συνέχεια θα ορίσετε τη ρύθμιση του project iOS Xamarin Studio ή Visual Studio.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Προσθέστε τις ειδοποιήσεις push σας εφαρμογή iOS

1. Στο έργο **iOS** , ανοίξτε το AppDelegate.cs προσθέστε την ακόλουθη πρόταση **Χρήση** στο επάνω μέρος του αρχείου κώδικα.

        using Newtonsoft.Json.Linq;

4. Στην τάξη **AppDelegate** , προσθέστε παράκαμψη για το συμβάν **RegisteredForRemoteNotifications** για την καταχώρηση για τις ειδοποιήσεις:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. Στο **AppDelegate**, πρέπει επίσης να προσθέσετε το παρακάτω παράκαμψη για το πρόγραμμα χειρισμού συμβάντων **DidReceivedRemoteNotification** :

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Αυτή η μέθοδος χειρίζεται εισερχόμενες ειδοποιήσεις ενώ εκτελείται η εφαρμογή.

2. Στην τάξη **AppDelegate** , προσθέστε τον ακόλουθο κώδικα στη μέθοδο **FinishedLaunching** : 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Αυτή η δυνατότητα επιτρέπει υποστήριξη για τις ειδοποιήσεις της απομακρυσμένης και προσκλήσεις push εγγραφής.

Εφαρμογή σας έχει ενημερωθεί για την υποστήριξη ειδοποιήσεων push.

####<a name="test-push-notifications-in-your-ios-app"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή iOS

1. Κάντε δεξί κλικ στο έργο iOS και κάντε κλικ στην επιλογή **Ορισμός ως StartPp έργου**.

2. Πατήστε το κουμπί **εκτέλεσης** ή **F5** στο Visual Studio για τη δημιουργία του έργου και να ξεκινήσετε την εφαρμογή σε συσκευή iOS και, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να αποδεχτείτε τις ειδοποιήσεις push.

    > [AZURE.NOTE] Πρέπει να αποδεχτείτε τις ειδοποιήσεις push ρητά από την εφαρμογή. Αυτή η αίτηση προκύπτει μόνο την πρώτη φορά που θα εκτελείται η εφαρμογή.

3. Στην εφαρμογή, πληκτρολογήστε μια εργασία και, στη συνέχεια, κάντε κλικ στο κουμπί συν (**+**) εικονίδιο.

4. Βεβαιωθείτε ότι μια ειδοποίηση λήψη, στη συνέχεια, κάντε κλικ στο **κουμπί OK** για να κλείσετε την ειδοποίηση.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Προαιρετικό) Ρύθμιση παραμέτρων και να εκτελέσετε το Windows έργα

Αυτή η ενότητα είναι για την εκτέλεση του Xamarin.Forms WinApp και WinPhone81 έργα για συσκευές με Windows. Αυτά τα βήματα υποστηρίζουν επίσης έργα καθολικής πλατφόρμας των Windows (UWP). Εάν δεν εργάζεστε με συσκευές με Windows, μπορείτε να παραλείψετε αυτή την ενότητα.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Καταχώρηση της εφαρμογής Windows για τις ειδοποιήσεις push WNS

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Ρύθμιση παραμέτρων ενότητα ειδοποίησης για WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Προσθέστε τις ειδοποιήσεις push για την εφαρμογή των Windows

1. Στο Visual Studio, ανοίξτε **App.xaml.cs** σε ένα έργο Windows και προσθέστε τις παρακάτω προτάσεις **Χρήση** .

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Αντικατάσταση `<your_TodoItemManager_portable_class_namespace>` με χώρο ονομάτων του έργου σας φορητή που περιέχει το `TodoItemManager` τάξης.
 

2. Στο App.xaml.cs, προσθέστε την ακόλουθη μέθοδο **InitNotificationsAsync** : 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Αυτή η μέθοδος λαμβάνει το κανάλι ειδοποιήσεων push και καταχωρεί ένα πρότυπο για να λαμβάνετε ειδοποιήσεις προτύπου από το Κέντρο ειδοποίηση σας. Μια ειδοποίηση πρότυπο που υποστηρίζει *messageParam* παραδίδονται σε αυτό το πρόγραμμα-πελάτη.

3. Στο App.xaml.cs, ενημερώστε τον ορισμό μέθοδο πρόγραμμα χειρισμού συμβάντων **OnLaunched** , προσθέτοντας την `async` πλήκτρο τροποποίησης, στη συνέχεια, προσθέστε την ακόλουθη γραμμή κώδικα στο τέλος της μεθόδου: 

        await InitNotificationsAsync();

    Αυτό διασφαλίζει ότι η καταχώρηση ειδοποιήσεων push δημιουργείται ή ανανεώνονται κάθε φορά που γίνεται εκκίνηση της εφαρμογής. Είναι σημαντικό να το κάνετε αυτό για να εξασφαλίσετε ότι το κανάλι push WNS πάντα είναι ενεργό.  

4. Στην Εξερεύνηση λύσεων για το Visual Studio, ανοίξτε το αρχείο **Package.appxmanifest** και ορίστε **Με δυνατότητα αναδυόμενη** σε **Ναι** στην περιοχή **ειδοποιήσεων**.

5. Δημιουργία της εφαρμογής και βεβαιωθείτε ότι έχετε χωρίς σφάλματα.  Εφαρμογή προγράμματος-πελάτη που τώρα θα πρέπει να εγγραφείτε για το πρότυπο ειδοποιήσεων από την εφαρμογή Mobile παρασκηνίου. Επαναλάβετε αυτήν την ενότητα για κάθε έργο Windows στη λύση σας.


####<a name="test-push-notifications-in-your-windows-app"></a>Ειδοποιήσεις push δοκιμής στην εφαρμογή Windows

1. Στο Visual Studio, κάντε δεξί κλικ σε ένα έργο των Windows και κάντε κλικ στην επιλογή **Ορισμός ως εκκίνησης έργου**.

2. Πατήστε το κουμπί **Εκτέλεση** για να δημιουργήσετε το έργο και ξεκινήστε την εφαρμογή.

3. Στην εφαρμογή, πληκτρολογήστε ένα όνομα για μια νέα todoitem και, στη συνέχεια, κάντε κλικ στο κουμπί συν (**+**) εικονίδιο για να την προσθέσετε.

4. Βεβαιωθείτε ότι είναι λάβει μια ειδοποίηση όταν προστίθεται το στοιχείο.

##<a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τις ειδοποιήσεις push:

* [Διάγνωση θεμάτων ειδοποιήσεων push](../notification-hubs/notification-hubs-push-notification-fixer.md)  
Υπάρχουν διάφορες αιτίες γιατί ειδοποιήσεις ενδέχεται να λάβετε αποτεθεί ή δεν τελειώνει σε συσκευές. Αυτό το θέμα δείχνει πώς μπορείτε να αναλύσετε και να υπολογίσετε τη ρίζα αποτυχιών ειδοποιήσεων push. 

Μπορείτε να συνεχίσετε με ένα από τα παρακάτω προγράμματα εκμάθησης:

* [Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας](app-service-mobile-xamarin-forms-get-started-users.md)  
Μάθετε πώς να ελέγχουν την ταυτότητα χρηστών από την εφαρμογή σας με μια υπηρεσία παροχής ταυτότητας.

* [Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή σας](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Μάθετε πώς μπορείτε να προσθέσετε την εφαρμογή σας χρησιμοποιώντας μια εφαρμογή Mobile παρασκηνίου υποστήριξη χωρίς σύνδεση. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333


<properties
    pageTitle="Γρήγορα αποτελέσματα με διανομείς ειδοποίησης για εφαρμογές Xamarin.Android | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή Xamarin Android."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Γρήγορα αποτελέσματα με διανομείς ειδοποίηση με Xamarin για Android

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων push σε μια εφαρμογή Xamarin.Android.
Θα μπορείτε να δημιουργήσετε μια κενή εφαρμογή Xamarin.Android που λαμβάνει τις ειδοποιήσεις push με χρήση του Google Cloud ανταλλαγής μηνυμάτων (GCM). Όταν ολοκληρώσετε την εργασία, θα έχετε τη δυνατότητα να χρησιμοποιήσετε το Κέντρο ειδοποίηση σας μετάδοση ειδοποιήσεων push σε όλες τις συσκευές που εκτελεί την εφαρμογή σας. Ο κώδικας ολοκληρωθεί είναι διαθέσιμη στην [εφαρμογή NotificationHubs] [ GitHub] δείγμα.

Αυτό το πρόγραμμα εκμάθησης δείχνει το απλό σενάριο εκπομπής χρησιμοποιώντας την ειδοποίηση διανομείς.


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Μπορείτε να βρείτε την ολοκληρωμένη κώδικα για αυτό το πρόγραμμα εκμάθησης GitHub [εδώ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Visual Studio με Xamarin στα Windows ή Xamarin Studio στο Mac OS χ. πλήρεις οδηγίες εγκατάστασης είναι σε [εγκατάστασης και εγκατάσταση για Visual Studio και Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Ενεργό λογαριασμό Google
+ [Azure μηνυμάτων στοιχείου]
+ [Στοιχείο προγράμματος-πελάτη ανταλλαγής μηνυμάτων Google Cloud]

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης διανομείς ειδοποίησης για τις εφαρμογές Xamarin.Android.

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Ενεργοποίηση Google Cloud ανταλλαγή μηνυμάτων

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Ρύθμιση των παραμέτρων σας διανομέα ειδοποίησης

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p>Κάντε κλικ στην καρτέλα <b>Ρύθμιση παραμέτρων</b> στην κορυφή, πληκτρολογήστε την τιμή του <b>Αριθμού-κλειδιού API</b> αποκτήσατε στην προηγούμενη ενότητα και, στη συνέχεια, κάντε κλικ στην επιλογή <b>Αποθήκευση</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Το Κέντρο ειδοποίηση σας τώρα έχει ρυθμιστεί ώστε να λειτουργεί με GCM και έχετε τις συμβολοσειρές σύνδεσης για να καταχωρήσετε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις και για την αποστολή ειδοποιήσεων push.

##<a name="connect-your-app-to-the-notification-hub"></a>Σύνδεση της εφαρμογής σας με την ενότητα "Ειδοποίηση"

###<a name="create-a-new-project"></a>Δημιουργία νέου έργου

1. Στο Xamarin Studio, κάντε κλικ στην επιλογή **Νέα λύση**, κάντε κλικ στην επιλογή **Εφαρμογή Android**και κάντε κλικ στο κουμπί **Επόμενο**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Πληκτρολογήστε το **Όνομα εφαρμογής** και **αναγνωριστικό**. Κάντε κλικ στην επιλογή που θέλετε να υποστηρίζουν και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο** και **Δημιουργία** **Plaforms προορισμού** .

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Αυτό δημιουργεί ένα νέο έργο Android.

2. Ανοίξτε τις ιδιότητες του έργου, κάνοντας δεξί κλικ σε νέο έργο στην προβολή λύσης και επιλέξτε **Επιλογές**. Επιλέξτε το στοιχείο **Εφαρμογή του Android** στην ενότητα **Δημιουργία** .

    Βεβαιωθείτε ότι το πρώτο γράμμα του το **όνομα του πακέτου** είναι πεζά.

    > [AZURE.IMPORTANT] Το πρώτο γράμμα του ονόματος του πακέτου πρέπει να είναι πεζά. Διαφορετικά, θα λάβετε δήλωσης σφάλματα εφαρμογών όταν καταχωρείτε τις **BroadcastReceiver** και **IntentFilter** για τις ειδοποιήσεις push παρακάτω.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Προαιρετικά, ορίστε την **Ελάχιστη Android έκδοση** σε ένα άλλο επίπεδο API.

4. Προαιρετικά, ορίστε την **προορισμού Android έκδοση** στην άλλη έκδοση API που θέλετε να στοχεύσετε (πρέπει να API επίπεδο 8 ή νεότερη έκδοση).

Κάντε κλικ στο **κουμπί OK** και κλείστε το παράθυρο διαλόγου Επιλογές του Project.


###<a name="add-the-required-components-to-your-project"></a>Προσθέστε τα απαιτούμενα στοιχεία στο έργο σας

Το Google Cloud μηνυμάτων πρόγραμμα-πελάτη διαθέσιμη στο χώρο αποθήκευσης στοιχείου Xamarin απλοποιεί τη διαδικασία της υποστήριξης ειδοποιήσεων push σε Xamarin.Android.

1. Κάντε δεξί κλικ στο φάκελο τα στοιχεία στην εφαρμογή Xamarin.Android και επιλέξτε **Λάβετε περισσότερα στοιχεία**.

2. Αναζήτηση για το στοιχείο **Azure ανταλλαγής μηνυμάτων** και προσθέστε το στο έργο.

3. Αναζήτηση για το στοιχείο **Πελάτη ανταλλαγής μηνυμάτων Google Cloud** και προσθέστε το στο έργο.


###<a name="set-up-notification-hubs-in-your-project"></a>Ρύθμιση ειδοποιήσεων διανομείς στο έργο σας

1. Συγκεντρώστε τις ακόλουθες πληροφορίες για το Κέντρο σας Android εφαρμογή και ειδοποιήσεων:

    - **GoogleProjectNumber**: λήψη αυτής της τιμής αριθμός έργου από την επισκόπηση της εφαρμογής στην πύλη για προγραμματιστές του Google. Κάνατε μια σημείωση παλαιότερη έκδοση αυτής της τιμής όταν δημιουργήσατε την εφαρμογή στην πύλη.
    - **Ακούστε συμβολοσειρά σύνδεσης**: στον πίνακα εργαλείων στην [Πύλη κλασική Azure], κάντε κλικ στην επιλογή **Προβολή συμβολοσειρές σύνδεσης**. Αντιγράψτε τη συμβολοσειρά σύνδεσης *DefaultListenSharedAccessSignature* για αυτήν την τιμή.
    - **Όνομα διανομέας**: αυτό είναι το όνομα του Κέντρο σας από την [Πύλη κλασική Azure]. Για παράδειγμα, *mynotificationhub2*.

    Δημιουργία μιας κλάσης **Constants.cs** για το έργο σας Xamarin και να ορίσετε τις ακόλουθες σταθερές τιμές στην τάξη. Αντικαταστήστε τα σύμβολα κράτησης θέσης με τις τιμές.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Προσθέστε τα ακόλουθα χρήση προτάσεων για **MainActivity.cs**:

        using Android.Util;
        using Gcm.Client;

3. Μεταβλητή παρουσίας για να προσθέσετε το `MainActivity` τάξης που θα χρησιμοποιηθεί για να εμφανιστεί ένα παράθυρο διαλόγου προειδοποίησης, όταν η εφαρμογή εκτελείται:

        public static MainActivity instance;


3. Δημιουργήστε την ακόλουθη μέθοδο στην τάξη **MainActivity** :

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. Στο το `OnCreate` μέθοδο **MainActivity.cs**, η προετοιμασία του `instance` μεταβλητή και να προσθέσετε μια κλήση σε `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Δημιουργήστε μια νέα κλάση, **MyBroadcastReceiver**.

    > [AZURE.NOTE] Θα σας θα περιηγηθείτε μέσω μιας κλάσης **BroadcastReceiver** από την αρχή παρακάτω. Ωστόσο, μια γρήγορη εναλλακτική λύση με μη αυτόματο τρόπο τη δημιουργία **MyBroadcastReceiver.cs** είναι να αναφέρονται στο αρχείο **GcmService.cs** που βρέθηκε στο έργο Xamarin.Android δείγμα περιλαμβάνεται με τα [δείγματα NotificationHubs][GitHub]. Δημιουργία διπλότυπου **GcmService.cs** και την αλλαγή ονόματα κλάσης μπορεί να είναι ένα καλό σημείο για να ξεκινήσετε με φυσική παρουσία.

5. Προσθέστε τα ακόλουθα χρήση προτάσεων για **MyBroadcastReceiver.cs** (που αναφέρονται στο στοιχείο και συγκρότησης που προσθέσατε προηγουμένως):

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. Στο **MyBroadcastReceiver.cs**, προσθέστε τις ακόλουθες αιτήσεις δικαιωμάτων μεταξύ των καταστάσεων **χρησιμοποιώντας** και τη δήλωση **χώρου ονομάτων** :

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. Στο **MyBroadcastReceiver.cs**, αλλάξτε την κλάση **MyBroadcastReceiver** ώστε να συμφωνεί με τα εξής:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Προσθέστε ένα άλλο εκπαιδευτικό στο **MyBroadcastReceiver.cs** με το όνομα **PushHandlerService**, που προέρχεται από **GcmServiceBase**. Βεβαιωθείτε ότι για να εφαρμόσετε το χαρακτηριστικό **υπηρεσίας** την κλάση:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** υλοποιεί μεθόδους **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**και **OnError()**. Μας κλάσης υλοποίησης **PushHandlerService** πρέπει να παρακάμψετε τις παρακάτω μεθόδους και τις παρακάτω μεθόδους θα εφαρμόζεται σε απόκριση αλληλεπίδραση με την ενότητα "Ειδοποίηση".


9. Παράκαμψη της μεθόδου **OnRegistered()** στο **PushHandlerService** , χρησιμοποιώντας τον ακόλουθο κώδικα:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] Τον κώδικα **OnRegistered()** παραπάνω, σημειώστε τη δυνατότητα να καθορίσετε ετικέτες για να εγγραφείτε για το συγκεκριμένο κανάλια ανταλλαγής μηνυμάτων.

10. Παράκαμψη της μεθόδου **OnMessage** στο **PushHandlerService** , χρησιμοποιώντας τον ακόλουθο κώδικα:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Προσθέστε τις ακόλουθες μεθόδους **createNotification** και **dialogNotify** σε **PushHandlerService** για ειδοποιώντας τους χρήστες όταν έχει ληφθεί μια ειδοποίηση.

    >[AZURE.NOTE] Ειδοποίηση σχεδίασης στο Android έκδοση 5.0 και νεότερη αντιπροσωπεύει μια σημαντική αναχώρησης από προηγούμενες εκδόσεις. Εάν δοκιμάσετε αυτό σε Android 5.0 ή νεότερη έκδοση, η εφαρμογή θα πρέπει να να εκτελείται για να λάβετε την ειδοποίηση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Android ειδοποιήσεις](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Παράκαμψη αποσπάσματος μέλη **OnUnRegistered()**, **OnRecoverableError()**και **OnError()** ώστε να μεταγλώττιση του κώδικα:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Εκτέλεση της εφαρμογής σας στο το προσομοίωσης

Εάν μπορείτε να εκτελέσετε αυτήν την εφαρμογή στο το προσομοίωσης, βεβαιωθείτε ότι χρησιμοποιείτε ένα Android εικονική συσκευή (AVD) που υποστηρίζει Google APIs.

> [AZURE.IMPORTANT] Προκειμένου να λαμβάνουν ειδοποιήσεις προώθησης, πρέπει να ρυθμίσετε ένα λογαριασμό Google στη συσκευή σας Android εικονικού. (Στο το προσομοίωσης, μεταβείτε στις **Ρυθμίσεις** και κάντε κλικ στην επιλογή **Προσθήκη λογαριασμού**). Επίσης, βεβαιωθείτε ότι το προσομοίωσης είναι συνδεδεμένος στο Internet.

>[AZURE.NOTE] Ειδοποίηση σχεδίασης στο Android έκδοση 5.0 και νεότερη αντιπροσωπεύει μια σημαντική αναχώρησης από προηγούμενες εκδόσεις. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Android ειδοποιήσεις](http://go.microsoft.com/fwlink/?LinkId=615880).


1. Από τα **Εργαλεία**, κάντε κλικ στην επιλογή **Άνοιγμα της διαχείρισης προσομοίωσης Android**, επιλέξτε τη συσκευή σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![][18]

2. Επιλέξτε **Google APIs** στο **προορισμού**και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

    ![][19]

3. Στην επάνω γραμμή εργαλείων, κάντε κλικ στην επιλογή **Εκτέλεση**και, στη συνέχεια, επιλέξτε την εφαρμογή σας. Αυτό ξεκινά το προσομοίωσης και εκτελεί την εφαρμογή.

  Η εφαρμογή ανακτά την *registrationId* από GCM και καταγράφει με την ενότητα "Ειδοποίηση".

##<a name="send-notifications-from-your-backend"></a>Αποστολή ειδοποιήσεων από τον υπολογιστή στο παρασκήνιο


Μπορείτε να δοκιμάσετε να λαμβάνω ειδοποιήσεις στην εφαρμογή σας με την αποστολή ειδοποιήσεων στην [Πύλη κλασική Azure] μέσω στην καρτέλα εντοπισμού σφαλμάτων στην ενότητα "Ειδοποίηση", όπως φαίνεται στην παρακάτω οθόνη.

![][30]


Ειδοποιήσεις Push κανονικά αποστέλλονται σε μια υπηρεσία υποστήριξης όπως υπηρεσίες Mobile ή ASP.NET μέσω μιας βιβλιοθήκης συμβατά. Μπορείτε επίσης να χρησιμοποιήσετε το REST API απευθείας, για να στείλετε μηνύματα ειδοποιήσεων εάν μια βιβλιοθήκη δεν είναι διαθέσιμη για τον υπολογιστή στο παρασκήνιο.

Ακολουθεί μια λίστα με ορισμένα άλλα προγράμματα εκμάθησης που μπορεί να θέλετε να αναθεωρήσετε για την αποστολή ειδοποιήσεων:

- ASP.NET: Ανατρέξτε στο θέμα [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες].
- Azure Java διανομείς ειδοποίηση SDK: Δείτε [πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Java](notification-hubs-java-push-notification-tutorial.md) για την αποστολή ειδοποιήσεων από Java. Αυτό έχει ελεγχθεί με έκλειψη για την ανάπτυξη Android.
- PHP: Δείτε [πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από PHP](notification-hubs-php-push-notification-tutorial.md).


Στην επόμενη υποενότητες του προγράμματος εκμάθησης, αποστολή ειδοποιήσεων χρησιμοποιώντας μια εφαρμογή κονσόλας .NET και χρησιμοποιώντας μια κινητή υπηρεσία μέσω μιας δέσμης ενεργειών κόμβο.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων, χρησιμοποιώντας μια εφαρμογή .NET

Σε αυτήν την ενότητα, θα στείλουμε ειδοποιήσεων, χρησιμοποιώντας μια εφαρμογή κονσόλας .NET

1. Δημιουργία νέας εφαρμογής κονσόλας Visual C#:

    ![][20]

2. Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία**, κάντε κλικ στην επιλογή **Διαχείριση πακέτου NuGet**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**.

    Εμφανίζει την Κονσόλα διαχείρισης πακέτου στο Visual Studio.

3. Στο παράθυρο Κονσόλα διαχείρισης πακέτου, ορίστε την **προεπιλεγμένη έργου** στο έργο σας νέα εφαρμογή κονσόλας και, στη συνέχεια, στο παράθυρο της κονσόλας, εκτελέστε την ακόλουθη εντολή:

        Install-Package Microsoft.Azure.NotificationHubs

    Αυτό προσθέτει μια αναφορά στο SDK διανομείς ειδοποίηση Azure χρησιμοποιώντας το <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">πακέτο Microsoft.Azure.Notification NuGet διανομείς</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Ανοίξτε το αρχείο Program.cs και προσθέστε το εξής `using` δήλωση:

        using Microsoft.Azure.NotificationHubs;

5. Στο σας `Program` κλάση, προσθέστε την ακόλουθη μέθοδο. Ενημερώστε το κείμενο κράτησης θέσης με *DefaultFullSharedAccessSignature* σύνδεσης συμβολοσειράς και διανομέα το όνομά σας από την [Πύλη κλασική Azure].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Προσθέστε τις ακόλουθες γραμμές στη μέθοδο **κύριες** σας:

         SendNotificationAsync();
         Console.ReadLine();

7. Πατήστε το πλήκτρο F5 για να εκτελέσετε την εφαρμογή. Θα πρέπει να λαμβάνετε μια ειδοποίηση στην εφαρμογή.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων, χρησιμοποιώντας μια υπηρεσία κινητών τηλεφώνων

1. Ακολουθήστε [Γρήγορα αποτελέσματα με τις υπηρεσίες του Mobile].

1. Είσοδος στην [Πύλη κλασική Azure]και επιλέξτε την υπηρεσία κινητών τηλεφώνων.

2. Επιλέξτε την καρτέλα **Χρονοδιάγραμμα** στο επάνω μέρος.

    ![][22]

3. Δημιουργήστε μια νέα προγραμματισμένη εργασία, εισαγάγετε ένα όνομα και επιλέξτε **ζήτηση**.

    ![][23]

4. Όταν δημιουργείται η εργασία, κάντε κλικ στο όνομα του έργου. Στη συνέχεια, κάντε κλικ στην καρτέλα **δέσμης ενεργειών** στην επάνω γραμμή.

5. Εισαγάγετε την ακόλουθη δέσμη ενεργειών μέσα στη συνάρτηση του χρονοδιαγράμματος. Βεβαιωθείτε ότι για να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultFullSharedAccessSignature* που λάβατε νωρίτερα. Κάντε κλικ στην επιλογή **Αποθήκευση**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Κάντε κλικ στην επιλογή **Εκτέλεση μία φορά** στην κάτω γραμμή. Πρέπει να λάβετε μια αναδυόμενη ειδοποίηση.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το απλό παράδειγμα μετάδοση ειδοποιήσεις για όλες τις συσκευές σας Android. Για να στοχεύετε σε συγκεκριμένους χρήστες, ανατρέξτε στο το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]. Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, μπορείτε να διαβάσετε [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις]. Μάθετε περισσότερα σχετικά με τη χρήση διανομείς ειδοποίηση στο [Ειδοποίηση διανομείς οδηγίες] και την [Ειδοποίηση διανομείς οδηγίες για Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Γρήγορα αποτελέσματα με τις υπηρεσίες του Mobile]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure κλασική πύλη]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Ειδοποίηση διανομείς καθοδήγηση]: http://msdn.microsoft.com/library/jj927170.aspx
[Ειδοποίηση διανομείς οδηγιών για Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]: /manage/services/notification-hubs/notify-users-aspnet
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Στοιχείο προγράμματος-πελάτη ανταλλαγής μηνυμάτων Google Cloud]: http://components.xamarin.com/view/GCMClient/
[Azure μηνυμάτων στοιχείου]: http://components.xamarin.com/view/azure-messaging

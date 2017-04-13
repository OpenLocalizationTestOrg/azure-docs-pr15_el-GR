<properties
    pageTitle="Αποστολή ειδοποιήσεων push σε Android με το Azure ειδοποίηση διανομείς και μηνυμάτων Cloud Firebase | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να χρησιμοποιείτε διανομείς ειδοποίηση Azure και την ανταλλαγή μηνυμάτων Cloud Firebase για τις ειδοποιήσεις push για συσκευές Android."
    services="notification-hubs"
    documentationCenter="android"
    keywords="ειδοποιήσεις Push ειδοποιήσεις push κοινοποίηση, ειδοποιήσεων android push, fcm, firebase στο cloud ανταλλαγή μηνυμάτων"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Αποστολή ειδοποιήσεων push σε Android με διανομείς ειδοποίηση Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

> [AZURE.IMPORTANT] Αυτό το θέμα περιγράφει τις ειδοποιήσεις push με Google Firebase Cloud ανταλλαγής μηνυμάτων (FCM). Εάν εξακολουθείτε να χρησιμοποιείτε το Google Cloud ανταλλαγής μηνυμάτων (GCM), ανατρέξτε στο θέμα [Αποστολή ειδοποιήσεων push για Android με το Azure ειδοποίηση διανομείς και GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure ειδοποίηση διανομείς και Firebase Cloud μηνυμάτων για την αποστολή ειδοποιήσεων push σε μια εφαρμογή του Android.
Θα μπορείτε να δημιουργήσετε μια κενή εφαρμογή Android που λαμβάνει τις ειδοποιήσεις push, χρησιμοποιώντας Firebase Cloud ανταλλαγής μηνυμάτων (FCM).



[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Το ολοκληρωμένο κώδικα για αυτό το πρόγραμμα εκμάθησης μπορούν να ληφθούν από GitHub [εδώ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

- Εκτός από τον ενεργό Azure λογαριασμού που αναφέρονται παραπάνω, αυτό το πρόγραμμα εκμάθησης απαιτεί την πιο πρόσφατη έκδοση του [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).
- Android 2.3 ή νεότερη έκδοση για Firebase Cloud μηνυμάτων.
- Αναθεώρηση αποθετήριο Google 27 ή νεότερη έκδοση είναι απαραίτητη για το Firebase Cloud μηνυμάτων.
- Υπηρεσίες Google Play 9.0.2 ή νεότερη έκδοση για Firebase Cloud μηνυμάτων.
- Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης διανομείς ειδοποίησης για τις εφαρμογές του Android.


##<a name="create-a-new-android-studio-project"></a>Δημιουργήστε ένα νέο έργο Studio Android

1. Στο Android Studio, ξεκινήστε ένα νέο έργο Android Studio.

    ![Android Studio - νέου έργου](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)

2. Επιλέξτε το διαστάσεων **τηλεφώνων και Tablet** και το **Ελάχιστο SDK** που θέλετε για την υποστήριξη. Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

    ![Android Studio - ροή εργασίας για τη δημιουργία έργου](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)

3. Επιλέξτε **Κενή δραστηριότητας** για τα κύρια δραστηριότητα, κάντε κλικ στο κουμπί **Επόμενο**και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**.


##<a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Δημιουργήστε ένα έργο που υποστηρίζει Firebase Cloud μηνυμάτων

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Ρύθμιση παραμέτρων διανομέα νέα ειδοποίηση

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. στο το blade **Ρυθμίσεις** από το Κέντρο ειδοποίηση σας, επιλέξτε **Υπηρεσίες ειδοποιήσεων** και, στη συνέχεια, **Google (GCM)**. Πληκτρολογήστε το κλειδί διακομιστή FCM που αντιγράψατε προηγουμένως από την [Κονσόλα Firebase](https://firebase.google.com/console/) και κάντε κλικ στην επιλογή **Αποθήκευση**.

&emsp;&emsp;![Ειδοποίηση Azure διανομείς - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Το Κέντρο ειδοποίηση σας τώρα έχει ρυθμιστεί να λειτουργεί με Firebase Cloud Messagin και έχετε τις συμβολοσειρές σύνδεσης για να καταχωρήσετε την εφαρμογή σας να λαμβάνει και να στείλετε τις ειδοποιήσεις push.

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας με την ενότητα "Ειδοποίηση"

###<a name="add-google-play-services-to-the-project"></a>Προσθέστε τις υπηρεσίες Google Play στο έργο

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Προσθήκη βιβλιοθηκών διανομείς ειδοποίηση Azure


1. Στο το `Build.Gradle` αρχείων για την **εφαρμογή**, προσθέστε τις ακόλουθες γραμμές στην ενότητα **εξαρτήσεις** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Προσθέστε το παρακάτω αποθετήριο μετά την ενότητα **εξαρτήσεις** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Ενημέρωση του AndroidManifest.xml.


1. Για την υποστήριξη FCM, θα σας πρέπει να υλοποιήσετε ένα Αναγνωριστικό εμφάνισης υπηρεσία ακρόασης μας κώδικα που χρησιμοποιείται για να [αποκτήσετε διακριτικά εγγραφής](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) με χρήση [Του Google FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). Σε αυτό το πρόγραμμα εκμάθησης θα σας θα ονομάσετε την κλάση `MyInstanceIDService`. 
 
    Προσθέστε τον παρακάτω ορισμό υπηρεσίας μέσα στο αρχείο AndroidManifest.xml, το `<application>` ετικέτα. 

        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>



2. Μόλις έχουμε μας διακριτικό δήλωσης FCM με το API FirebaseInstanceId, θα χρησιμοποιήσουμε το για να [καταχωρήσετε με την ενότητα ειδοποίηση Azure](notification-hubs-push-notification-registration-management.md). Θα υποστηρίζουμε αυτή η καταχώρηση του φόντου χρησιμοποιώντας μια `IntentService` με το όνομα `RegistrationIntentService`. Αυτή η υπηρεσία θα είναι επίσης υπεύθυνοι για την ανανέωση μας διακριτικό δήλωσης FCM.
 
    Προσθέστε τον παρακάτω ορισμό υπηρεσίας μέσα στο αρχείο AndroidManifest.xml, το `<application>` ετικέτα. 

        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>



3. Θα, επίσης, να ορίσετε μια ακουστικό να λαμβάνω ειδοποιήσεις. Προσθέστε τον ακόλουθο ορισμό ακουστικό μέσα στο αρχείο AndroidManifest.xml, το `<application>` ετικέτα. Αντικατάσταση το `<your package>` σύμβολο κράτησης θέσης με το όνομά σας πραγματική πακέτου εμφανίζεται στο επάνω μέρος το `AndroidManifest.xml` αρχείου.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Προσθέστε τα ακόλουθα απαραίτητες FCM που σχετίζονται με δικαιώματα παρακάτω το `</application>` ετικέτα. Βεβαιωθείτε ότι για να αντικαταστήσετε `<your package>` με το όνομα του πακέτου εμφανίζεται στο επάνω μέρος του `AndroidManifest.xml` αρχείου.

    Για περισσότερες πληροφορίες σχετικά με αυτά τα δικαιώματα, ανατρέξτε στο θέμα [Ρύθμιση μια εφαρμογή προγράμματος-πελάτη GCM για Android](https://developers.google.com/cloud-messaging/android/client#manifest) και [μετεγκατάσταση μια εφαρμογή προγράμματος-πελάτη GCM για Android για να Firebase Cloud μηνυμάτων](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />


### <a name="adding-code"></a>Προσθήκη κώδικα


1. Στην προβολή έργου, αναπτύξτε το στοιχείο **εφαρμογή** > **src** > **κύριο** > **java**. Κάντε δεξιό κλικ στο φάκελο του πακέτου στην περιοχή **java**, κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κλάσης Java**. Προσθέσετε μια νέα κατηγορία με το όνομα `NotificationSettings`. 

    ![Android Studio - νέας κλάσης Java](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Βεβαιωθείτε ότι για να ενημερώσετε αυτά τα τρία πλαίσια κράτησης θέσης στο τον ακόλουθο κώδικα για την `NotificationSettings` κλάσης:
    * **Αναγνωριστικό SenderId πρέπει**: το αναγνωριστικό αποστολέα τον οποίο αποκτήσατε προηγουμένως στην καρτέλα **Ανταλλαγή μηνυμάτων Cloud** από τις ρυθμίσεις του έργου σας στην [Κονσόλα Firebase](https://firebase.google.com/console/).
    * **HubListenConnectionString**: τη συμβολοσειρά σύνδεσης **DefaultListenAccessSignature** για το Κέντρο σας. Μπορείτε να αντιγράψετε τη συμβολοσειρά σύνδεσης, κάνοντας κλικ στο κουμπί **Πολιτικές πρόσβασης** του blade **Ρυθμίσεις** από το Κέντρο σας στην [Πύλη του Azure].
    * **HubName**: Χρησιμοποιήστε το όνομα του Κέντρο σας ειδοποίηση που εμφανίζεται στο το blade διανομέα στην [Πύλη του Azure].

    `NotificationSettings`κώδικας:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }

2. Χρησιμοποιώντας τα βήματα που περιγράφονται παραπάνω, προσθέστε μια άλλη νέα κλάση με το όνομα `MyInstanceIDService`. Αυτό θα μας υλοποίηση της υπηρεσίας ακρόασης Αναγνωριστικό εμφάνισης.

    Ο κωδικός για αυτήν την κλάση θα καλούν μας `IntentService` για να [ανανεώσετε το διακριτικό FCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) στο παρασκήνιο.

        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;
        
        
        public class MyInstanceIDService extends FirebaseInstanceIdService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.d(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Προσθήκη άλλου νέα κλάση στο έργο σας με το όνομα, `RegistrationIntentService`. Αυτό θα είναι η εφαρμογή για το `IntentService` που θα χειρίζεται [το διακριτικό FCM ανανέωση](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) και την [εγγραφή με την ενότητα "Ειδοποίηση"](notification-hubs-push-notification-registration-management.md).

    Χρησιμοποιήστε τον ακόλουθο κώδικα για αυτήν την κλάση.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {
        
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
        
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. Στο σας `MainActivity` κλάση, προσθέστε τα ακόλουθα `import` δηλώσεις επάνω από τη δήλωση τάξης.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Προσθέστε τα ακόλουθα μέλη ιδιωτική στο επάνω μέρος της κατηγορίας. Θα χρησιμοποιήσουμε αυτές τις [ελέγχει τη διαθεσιμότητα των αναπαραγωγή τις υπηρεσίες Google όπως προτείνεται από το Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Στο σας `MainActivity` κλάση, προσθέστε την ακόλουθη μέθοδο για να τη διαθεσιμότητα των υπηρεσιών αναπαραγωγή Google. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. Στο σας `MainActivity` κλάση, προσθέστε τον ακόλουθο κώδικα που θα ελέγξετε για τις υπηρεσίες αναπαραγωγή Google πριν από την κλήση του `IntentService` για να λάβετε την εγγραφή σας FCM διακριτικού και καταχώρηση με το Κέντρο ειδοποίηση σας.

        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. Στο το `OnCreate` μέθοδο το `MainActivity` κλάση, προσθέστε τον ακόλουθο κώδικα για να ξεκινήσετε τη διαδικασία εγγραφής όταν δημιουργείται δραστηριότητα.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Προσθέστε αυτές τις πρόσθετες μεθόδους για την `MainActivity` για να επαληθεύσετε εφαρμογή κατάσταση και αναφορά κατάστασης στην εφαρμογή.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. Το `ToastNotify` μέθοδος χρησιμοποιεί *"Γεια"* `TextView` στοιχείο ελέγχου για να αναφέρετε την κατάσταση και τις ειδοποιήσεις μόνιμα στην εφαρμογή. Στη διάταξή σας activity_main.xml, προσθέστε το ακόλουθο αναγνωριστικό για αυτό το στοιχείο ελέγχου.

        android:id="@+id/text_hello"

11. Στη συνέχεια θα προσθέσουμε μια υποκατηγορία για μας ακουστικό ορίσαμε στο το AndroidManifest.xml. Προσθήκη άλλου νέα κλάση στο έργο σας με το όνομα `MyHandler`.

12. Προσθέστε τις ακόλουθες δηλώσεις εισαγωγής στο επάνω μέρος `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Προσθέστε τον ακόλουθο κώδικα για την `MyHandler` κλάσης καθιστώντας μια υποκατηγορία `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Αυτός ο κωδικός παρακάμπτει το `OnReceive` μέθοδο, ώστε το πρόγραμμα χειρισμού θα αναφέρει ειδοποιήσεις που λαμβάνονται. Το πρόγραμμα χειρισμού στέλνει επίσης την ειδοποίηση push για τη Διαχείριση ειδοποιήσεων Android, χρησιμοποιώντας το `sendNotification()` μέθοδο. Το `sendNotification()` μέθοδος πρέπει να εκτελεστεί όταν η εφαρμογή δεν εκτελείται και να λάβει μια ειδοποίηση.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Στο Android Studio στη γραμμή μενού, κάντε κλικ στην επιλογή **Δημιουργία** > **Εκ νέου δημιουργία έργου** για να βεβαιωθείτε ότι υπάρχουν χωρίς σφάλματα στον κώδικά σας.
15. Εκτελέστε την εφαρμογή στη συσκευή σας και να επαληθεύσετε με επιτυχία καταχωρεί με την ενότητα "Ειδοποίηση". 

    > [AZURE.NOTE] Ενδέχεται να αποτύχει εγγραφής από την αρχική εκκίνηση μέχρι το `onTokenRefresh()` ονομάζεται μέθοδο παρουσίας αναγνωριστικό υπηρεσίας. Η ανανέωση θα πρέπει να προετοιμασία μια επιτυχής εγγραφή με την ενότητα "Ειδοποίηση".

##<a name="sending-push-notifications"></a>Αποστολή ειδοποιήσεων push

Μπορείτε να δοκιμάσετε να λαμβάνω ειδοποιήσεις push στην εφαρμογή, στέλνοντάς τους μέσω του [Azure πύλη] - εμφάνισης για την ενότητα **Αντιμετώπιση προβλημάτων** στο το blade διανομέα, όπως φαίνεται παρακάτω.

![Αποστολή ειδοποίησης Azure διανομείς - έλεγχος](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων push απευθείας από την εφαρμογή

>[AZURE.IMPORTANT] Αυτό το παράδειγμα αποστολής ειδοποιήσεων από την εφαρμογή υπολογιστή-πελάτη παρέχονται για την εκμάθηση μόνο. Επειδή αυτό θα απαιτήσει την `DefaultFullSharedAccessSignature` για να παρουσιάσετε στην εφαρμογή του προγράμματος-πελάτη, εκθέτει το Κέντρο ειδοποίηση σας για να τον κίνδυνο ότι ένας χρήστης μπορεί να αποκτήσει πρόσβαση για την αποστολή ειδοποιήσεων μη εξουσιοδοτημένη για τους πελάτες σας.

Κανονικά, μπορείτε να στείλετε ειδοποιήσεων με χρήση ενός διακομιστή παρασκηνίου. Για ορισμένες περιπτώσεις, ίσως θέλετε να έχετε τη δυνατότητα για την αποστολή ειδοποιήσεων push απευθείας από την εφαρμογή-πελάτη. Αυτή η ενότητα εξηγεί τον τρόπο για την αποστολή ειδοποιήσεων από το πρόγραμμα-πελάτη με χρήση του [Azure ειδοποίηση διανομέα REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Στην προβολή έργου Android Studio, αναπτύξτε το στοιχείο **εφαρμογή** > **src** > **κύριο** > **πόροι** > **διάταξη**. Άνοιγμα του `activity_main.xml` διάταξη αρχείου και κάντε κλικ στην επιλογή το **κείμενο** με το tab για να ενημερώσετε τα περιεχόμενα κειμένου του αρχείου. Ενημερώστε την με τον κώδικα που ακολουθεί, τα οποία προσθέτει νέες `Button` και `EditText` στοιχεία ελέγχου για την αποστολή μηνύματα ειδοποιήσεων push για την ενότητα "Ειδοποίηση". Μόλις πριν προσθέσετε αυτόν τον κώδικα στο κάτω μέρος, `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Στην προβολή έργου Android Studio, αναπτύξτε το στοιχείο **εφαρμογή** > **src** > **κύριο** > **πόροι** > **τιμές**. Άνοιγμα του `strings.xml` αρχείων και προσθέστε τις τιμές συμβολοσειράς που αναφέρονται από το νέο `Button` και `EditText` στοιχεία ελέγχου. Προσθέστε αυτές τις στο κάτω μέρος του αρχείου, απλώς πριν `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Στο σας `NotificationSetting.java` αρχείου, προσθέστε την ακόλουθη ρύθμιση για να το `NotificationSettings` τάξης.

    Ενημέρωση `HubFullAccess` με τη συμβολοσειρά σύνδεσης **DefaultFullSharedAccessSignature** για το Κέντρο σας. Μπορούν να αντιγραφούν αυτήν τη συμβολοσειρά σύνδεσης από την [Πύλη Azure] κάνοντας κλικ στο κουμπί **Πολιτικές πρόσβασης** του blade **Ρυθμίσεις** για το Κέντρο ειδοποίηση σας.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";

4. Στο σας `MainActivity.java` αρχείων, προσθέστε τα ακόλουθα `import` παραπάνω προτάσεις το `MainActivity` τάξης.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. Στο σας `MainActivity.java` αρχείο, προσθέστε τα ακόλουθα μέλη στο επάνω μέρος της το `MainActivity` τάξης.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Πρέπει να δημιουργήσετε μια υπογραφή Access λογισμικού (συσχετισμών ασφαλείας) το διακριτικό για τον έλεγχο ταυτότητας μιας αίτησης POST για την αποστολή μηνυμάτων με την ειδοποίηση διανομέα. Αυτό γίνεται κατά την ανάλυση των βασικών δεδομένων από τη συμβολοσειρά σύνδεσης και, στη συνέχεια, δημιουργώντας το διακριτικό συσχετισμών ασφαλείας, όπως αναφέρεται στην περιοχή αναφοράς REST API [Κοινές έννοιες](http://msdn.microsoft.com/library/azure/dn495627.aspx) . Ο ακόλουθος κώδικας είναι ένα παράδειγμα υλοποίηση.

    Στο `MainActivity.java`, προσθέστε την ακόλουθη μέθοδο για να το `MainActivity` κλάσης ανάλυση συμβολοσειρά σύνδεσης.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. Στο `MainActivity.java`, προσθέστε την ακόλουθη μέθοδο για να το `MainActivity` κλάσης για τη δημιουργία ενός διακριτικού ελέγχου ταυτότητας των συσχετισμών ασφαλείας.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. Στο `MainActivity.java`, προσθέστε την ακόλουθη μέθοδο για να το `MainActivity` τάξης για να χειριστείτε το κουμπί **Αποστολή ειδοποίησης** , κάντε κλικ στην επιλογή και στείλτε το μήνυμα ειδοποίησης push την ενότητα, χρησιμοποιώντας το ενσωματωμένο REST API.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Δοκιμή της εφαρμογής σας

####<a name="push-notifications-in-the-emulator"></a>Ειδοποιήσεις Push στο το προσομοίωσης

Εάν θέλετε να ελέγξετε τις ειδοποιήσεις push μέσα σε μια προσομοίωσης, βεβαιωθείτε ότι η εικόνα σας προσομοίωσης υποστηρίζει το επίπεδο Google API που επιλέξατε για την εφαρμογή σας. Εάν την εικόνα σας δεν υποστηρίζει φυσικά API Google, θα καταλήξετε με το **ΥΠΗΡΕΣΊΑ\_δεν\_ΔΙΑΘΈΣΙΜΕΣ** εξαίρεση.

Εκτός από τα παραπάνω, βεβαιωθείτε ότι έχετε προσθέσει το λογαριασμό σας Google στο σας εκτελείται προσομοίωσης στην περιοχή **Ρυθμίσεις** > **Λογαριασμοί**. Διαφορετικά, τις προσπάθειες για την καταχώρηση με GCM ενδέχεται να έχει ως αποτέλεσμα το **ελέγχου ΤΑΥΤΌΤΗΤΑΣ\_ΑΠΈΤΥΧΕ** εξαίρεση.

####<a name="running-the-application"></a>Εκτέλεση της εφαρμογής

1. Εκτελέστε την εφαρμογή και παρατηρήστε ότι το Αναγνωριστικό καταχώρησης αναφερθεί για μια επιτυχημένη καταχώρηση.

    ![Δοκιμές σε Android - καταχώρησης καναλιού](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)

2. Πληκτρολογήστε ένα μήνυμα ειδοποίησης που θα σταλεί σε όλες τις συσκευές Android που έχουν εγγραφεί με την ενότητα.

    ![Δοκιμές σε Android - αποστολή ενός μηνύματος](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)

3. Πατήστε **Αποστολή ειδοποίησης**. Θα εμφανίσει όλες τις συσκευές που έχετε την εφαρμογή που εκτελεί μια `AlertDialog` παρουσία με το μήνυμα ειδοποίησης push. Συσκευές που δεν έχετε την εφαρμογή εκτελείται, αλλά έχουν καταχωρηθεί προηγουμένως για τις ειδοποιήσεις push θα λάβετε μια ειδοποίηση στη Διαχείριση ειδοποιήσεων Android. Αυτές μπορούν να προβληθούν με κίνηση σάρωσης προς τα κάτω από την επάνω αριστερή γωνία.

    ![Δοκιμές σε Android - ειδοποιήσεων](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

##<a name="next-steps"></a>Επόμενα βήματα

Συνιστάται να το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push σε χρήστες] με το επόμενο βήμα. Αυτό θα σας δείξουμε πώς για την αποστολή ειδοποιήσεων από μια παρασκηνίου ASP.NET Χρησιμοποιήστε ετικέτες για να στοχεύετε σε συγκεκριμένους χρήστες.

Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, δείτε το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] .

Για να μάθετε περισσότερες γενικές πληροφορίες σχετικά με την ειδοποίηση διανομείς, ανατρέξτε στο θέμα μας [Καθοδήγηση διανομείς ειδοποίησης].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Ειδοποίηση διανομείς καθοδήγηση]: notification-hubs-push-notification-overview.md
[Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push στους χρήστες]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Πύλη του Azure]: https://portal.azure.com

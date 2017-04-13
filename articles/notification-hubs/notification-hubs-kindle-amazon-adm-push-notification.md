<properties
    pageTitle="Γρήγορα αποτελέσματα με διανομείς ειδοποίηση Azure για εφαρμογές Kindle | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή Kindle."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Γρήγορα αποτελέσματα με διανομείς ειδοποίησης για εφαρμογές Kindle

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων push σε μια εφαρμογή Kindle.
Θα μπορείτε να δημιουργήσετε μια κενή εφαρμογή Kindle που λαμβάνει τις ειδοποιήσεις push, χρησιμοποιώντας την ανταλλαγή μηνυμάτων συσκευή Amazon (ADM).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Λήψη του Android SDK (υποθέσουμε ότι θα χρησιμοποιήσετε Έκλειψη) από την <a href="http://go.microsoft.com/fwlink/?LinkId=389797">τοποθεσία Android</a>.
+ Ακολουθήστε τα βήματα στο θέμα <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Ρύθμιση του ανάπτυξης περιβάλλον σας</a> για να ρυθμίσετε το περιβάλλον ανάπτυξης για Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Προσθέστε μια νέα εφαρμογή για την πύλη για προγραμματιστές

1. Πρώτα, δημιουργήστε μια εφαρμογή στην [πύλη για προγραμματιστές του Amazon].

    ![][0]

2. Αντιγράψτε το **πλήκτρο εφαρμογής**.

    ![][1]

3. Στην πύλη, κάντε κλικ στο όνομα της εφαρμογής και, στη συνέχεια, κάντε κλικ στην καρτέλα **Συσκευή μηνυμάτων** .

    ![][2]

4. Κάντε κλικ στην επιλογή **Δημιουργία νέου προφίλ ασφαλείας**και, στη συνέχεια, δημιουργήστε ένα νέο προφίλ ασφαλείας (για παράδειγμα, **το προφίλ ασφαλείας TestAdm**). Στη συνέχεια, κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![][3]

5. Κάντε κλικ στην επιλογή **Προφίλ ασφαλείας** για να προβάλετε το προφίλ ασφαλείας που μόλις δημιουργήσατε. Αντιγράψτε τις τιμές **Αναγνωριστικό υπολογιστή-πελάτη** και **Μυστικό προγράμματος-πελάτη** για μελλοντική χρήση.

    ![][4]

## <a name="create-an-api-key"></a>Δημιουργήστε ένα κλειδί API

1. Ανοίξτε μια γραμμή εντολών με δικαιώματα διαχειριστή.
2. Μεταβείτε στο φάκελο Android SDK.
3. Εισαγάγετε την ακόλουθη εντολή:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  Για τον κωδικό πρόσβασης **keystore** , πληκτρολογήστε **android**.

5.  Αντιγράψτε το αποτύπωμα **MD5** .
6.  Επιστροφή στην πύλη του προγραμματιστής, στην καρτέλα **ανταλλαγή μηνυμάτων** , κάντε κλικ στην επιλογή **Android/Kindle** και πληκτρολογήστε το όνομα του πακέτου για την εφαρμογή σας (για παράδειγμα, **com.sample.notificationhubtest**) και την τιμή **MD5** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία κλειδιού API**.

## <a name="add-credentials-to-the-hub"></a>Προσθήκη διαπιστευτηρίων για την ενότητα

Στην πύλη, προσθέστε το μυστικό προγράμματος-πελάτη και το Αναγνωριστικό υπολογιστή-πελάτη στην καρτέλα **Ρύθμιση παραμέτρων** του Κέντρο σας ειδοποίηση.

## <a name="set-up-your-application"></a>Ρυθμίστε την εφαρμογή σας

> [AZURE.NOTE] Όταν θέλετε να δημιουργήσετε μια εφαρμογή, χρησιμοποιήστε τουλάχιστον 17 επίπεδο API.

Προσθέστε τις βιβλιοθήκες ADM στο έργο σας Έκλειψη:

1. Για να αποκτήσετε τη βιβλιοθήκη ADM, [κάντε λήψη του SDK]. Εξαγάγετε το αρχείο zip SDK.
2. Στο Έκλειψη, κάντε δεξιό κλικ στο έργο σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Ιδιότητες**. Επιλέξτε **Διαδρομή Δόμηση Java** στα αριστερά και, στη συνέχεια, επιλέξτε την καρτέλα **βιβλιοθήκες **στο επάνω μέρος. Κάντε κλικ στην επιλογή **Προσθήκη εξωτερικών βάζων**και επιλέξτε το αρχείο `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` από τον κατάλογο στο οποίο εξαγάγατε το Amazon SDK.
3. Κάντε λήψη του SDK Android NotificationHubs (σύνδεση).
4. Αποσυμπιέστε το πακέτο και, στη συνέχεια, σύρετε το αρχείο `notification-hubs-sdk.jar` σε το `libs` φακέλου σε Έκλειψη.

Επεξεργαστείτε το δηλωτικό εφαρμογής για την υποστήριξη ADM:

1. Προσθέστε το πεδίο ονόματος Amazon στο ριζικό στοιχείο δήλωσης:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Προσθήκη δικαιωμάτων ως το πρώτο στοιχείο στην περιοχή δήλωσης στοιχείου. Αντικαταστήστε **[Το ΌΝΟΜΆ σας ΠΑΚΈΤΟ]** με το πακέτο που χρησιμοποιήσατε για να δημιουργήσετε την εφαρμογή σας.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Εισαγάγετε το ακόλουθο στοιχείο ως το πρώτο θυγατρικό του στοιχείου εφαρμογής. Μην ξεχάσετε να αντικαταστήσετε το **[ΌΝΟΜΑ ΣΑΣ ΥΠΗΡΕΣΊΑΣ]** με το όνομα του προγράμματος χειρισμού μήνυμα ADM που δημιουργείτε στην επόμενη ενότητα (συμπεριλαμβανομένου του πακέτου) και αντικαταστήστε **[Το ΌΝΟΜΆ σας ΠΑΚΈΤΟ]** με το όνομα του πακέτου με την οποία δημιουργήσατε την εφαρμογή σας.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Δημιουργία του προγράμματος χειρισμού ADM μηνύματος

1. Δημιουργία μιας νέας κλάσης που μεταβιβάζονται από `com.amazon.device.messaging.ADMMessageHandlerBase` και ονομάστε το `MyADMMessageHandler`, όπως φαίνεται στην παρακάτω εικόνα:

    ![][6]

2. Προσθέστε τα ακόλουθα `import` προτάσεις:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Προσθέστε τον ακόλουθο κώδικα στην τάξη που δημιουργήσατε. Να θυμάστε να αντικαταστήσετε την ενότητα όνομα και η συμβολοσειρά σύνδεσης (ακρόαση):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Προσθέστε τον ακόλουθο κώδικα για την `OnMessage()` μέθοδο:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Προσθέστε τον ακόλουθο κώδικα για την `OnRegistered` μέθοδο:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Προσθέστε τον ακόλουθο κώδικα για την `OnUnregistered` μέθοδο:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. Στο το `MainActivity` μέθοδο, προσθέστε την ακόλουθη δήλωση εισαγωγής:

        import com.amazon.device.messaging.ADM;

8. Προσθέστε τον ακόλουθο κώδικα στο τέλος του `OnCreate` μέθοδο:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Προσθήκη αριθμού-κλειδιού API για την εφαρμογή σας

1. Στο Έκλειψη, δημιουργήστε ένα νέο αρχείο με το όνομα **api_key.txt** των περιουσιακών στοιχείων καταλόγου του έργου σας.
2. Ανοίξτε το αρχείο και αντιγράψτε τον αριθμό-κλειδί API που δημιουργήσατε στην πύλη για προγραμματιστές του Amazon.

## <a name="run-the-app"></a>Εκτελέστε την εφαρμογή

1. Ξεκινήστε το προσομοίωσης.
2. Στο το προσομοίωσης, κάντε μια κίνηση σάρωσης από το επάνω μέρος και κάντε κλικ στην επιλογή **Ρυθμίσεις**, και, στη συνέχεια, κάντε κλικ στην επιλογή **ο λογαριασμός μου** και καταχώρηση με έναν έγκυρο λογαριασμό Amazon.
3. Στο Έκλειψη, εκτελέστε την εφαρμογή.

> [AZURE.NOTE] Εάν παρουσιαστεί κάποιο πρόβλημα, ελέγξτε την ώρα της προσομοίωσης (ή η συσκευή). Η τιμή ώρας πρέπει να είναι ακριβείς. Για να αλλάξετε την ώρα της προσομοίωσης το Kindle, μπορείτε να εκτελέσετε την ακόλουθη εντολή από τον κατάλογο εργαλεία πλατφόρμας Android SDK:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Αποστολή ενός μηνύματος

Για να στείλετε ένα μήνυμα με τη χρήση .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon πύλη για προγραμματιστές]: https://developer.amazon.com/home.html
[Κάντε λήψη του SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png

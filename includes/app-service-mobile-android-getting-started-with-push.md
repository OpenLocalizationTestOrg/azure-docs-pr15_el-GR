1. Στο έργο σας **εφαρμογή** , ανοίξτε το αρχείο `AndroidManifest.xml`. Στον κώδικα στα επόμενα δύο βήματα, αντικαταστήστε _`**my_app_package**`_ με το όνομα του πακέτου εφαρμογής για το έργο σας, που είναι η τιμή του το `package` χαρακτηριστικό το `manifest` ετικέτα.

2. Προσθέστε τα ακόλουθα δικαιώματα νέα μετά την υπάρχουσα `uses-permission` στοιχείο:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Προσθέστε τον ακόλουθο κώδικα μετά το `application` tag ανοίγματος:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Ανοίξτε το αρχείο *ToDoActivity.java*και προσθέστε την ακόλουθη πρόταση εισαγωγής:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Προσθέστε την ακόλουθη μεταβλητή ιδιωτικό στην κατηγορία: αντικατάσταση _`<PROJECT_NUMBER>`_ με τον αριθμό έργου που έχουν εκχωρηθεί από το Google για την εφαρμογή σας στην προηγούμενη διαδικασία:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Αλλάξετε τον ορισμό του το *MobileServiceClient* από **ιδιωτική** σε **δημόσια στατικά**, ώστε να τώρα μοιάζει κάπως έτσι:

        public static MobileServiceClient mClient;

7. Στη συνέχεια θα χρειαστεί να προσθέσετε μια νέα κλάση να χειρίζεται τις ειδοποιήσεις. Στην Εξερεύνηση έργου, ανοίξτε το **src** => **κύριο** => κόμβοι**java** και κάντε δεξί κλικ στον κόμβο όνομα πακέτου: κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **Κλάσης Java**.

8. Στο **όνομα του** τύπου `MyHandler`, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. Στο πλαίσιο MyHandler αρχείου, αντικαταστήστε τη δήλωση τάξης με

        public class MyHandler extends NotificationsHandler {


10. Προσθέστε τις παρακάτω προτάσεις εισαγωγή για τη `MyHandler` κλάσης:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Στη συνέχεια, προσθέστε αυτό το μέλος για την `MyHandler` κλάσης:

        public static final int NOTIFICATION_ID = 1;


12. Στο το `MyHandler` κλάση, προσθέστε τον ακόλουθο κώδικα για να παρακάμψετε τη μέθοδο **onRegistered** , η οποία καταχωρεί τη συσκευή σας με την υπηρεσία κινητών τηλεφώνων διανομέα ειδοποίησης.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. Στο το `MyHandler` κλάση, προσθέστε τον ακόλουθο κώδικα για να παρακάμψετε τη μέθοδο **onReceive** , η οποία έχει ως αποτέλεσμα την ειδοποίηση για να εμφανίσετε μετά τη λήψη.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. Επιστροφή στο αρχείο TodoActivity.java, ενημερώστε τη μέθοδο **onCreate** της κλάσης *ToDoActivity* για να καταχωρήσετε την κλάση πρόγραμμα χειρισμού ειδοποίησης. Βεβαιωθείτε ότι για να προσθέσετε αυτόν τον κωδικό, αφού δημιουργηθεί το *MobileServiceClient* .


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Εφαρμογή σας έχει ενημερωθεί για την υποστήριξη ειδοποιήσεων push.

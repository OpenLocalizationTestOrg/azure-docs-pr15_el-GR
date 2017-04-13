<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure ειδοποίηση διανομείς χρησιμοποιώντας Baidu | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να χρησιμοποιείτε Azure διανομείς ειδοποίησης για τις ειδοποιήσεις push για συσκευές Android χρησιμοποιώντας Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Γρήγορα αποτελέσματα με χρήση Baidu διανομείς ειδοποίησης

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

Baidu cloud push είναι μια υπηρεσία cloud κινεζικών που μπορείτε να χρησιμοποιήσετε για την αποστολή ειδοποιήσεων push για κινητές συσκευές. Αυτή η υπηρεσία είναι ιδιαίτερα χρήσιμη στην Κίνα, όπου την παροχή ειδοποιήσεων push σε Android είναι περίπλοκη λόγω της παρουσίας αποθηκεύει διαφορετικό εφαρμογών και υπηρεσιών push, εκτός από τη διαθεσιμότητα των συσκευές Android που δεν είναι συνήθως συνδεδεμένοι στο GCM (Google Cloud μηνυμάτων).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ Android SDK (υποθέσουμε ότι θα χρησιμοποιήσετε Έκλειψη), το οποίο μπορείτε να κάνετε λήψη από την <a href="http://go.microsoft.com/fwlink/?LinkId=389797">τοποθεσία Android</a>
+ [Υπηρεσίες κινητές συσκευές Android SDK]
+ [Android Baidu Push SDK]

>[AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Δημιουργία λογαριασμού Baidu

Για να χρησιμοποιήσετε Baidu, πρέπει να έχετε ένα λογαριασμό Baidu. Εάν έχετε ήδη ένα, συνδεθείτε [πύλη Baidu] και μεταβείτε στο επόμενο βήμα. Διαφορετικά, δείτε τις παρακάτω οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα λογαριασμό Baidu.  

1. Μεταβείτε στην [πύλη Baidu] και κάντε κλικ στη σύνδεση**登录**(**σύνδεση**). Κάντε κλικ στην επιλογή**立即注册**για να ξεκινήσετε τη διαδικασία εγγραφής λογαριασμού.

    ![][1]

2. Πληκτρολογήστε τις απαιτούμενες λεπτομέρειες — τηλεφώνου/ηλεκτρονικού ταχυδρομείου διεύθυνση, τον κωδικό πρόσβασης και επαλήθευση κώδικα — και κάντε κλικ στην επιλογή **εγγραφή στο**.

    ![][2]

3. Που θα σταλεί ένα μήνυμα ηλεκτρονικού ταχυδρομείου στη διεύθυνση ηλεκτρονικού ταχυδρομείου που έχετε εισαγάγει με μια σύνδεση για να ενεργοποιήσετε το λογαριασμό σας Baidu.

    ![][3]

4. Συνδεθείτε στο λογαριασμό ηλεκτρονικού ταχυδρομείου σας, ανοίξτε την αλληλογραφία ενεργοποίησης Baidu και κάντε κλικ στη σύνδεση ενεργοποίησης για να ενεργοποιήσετε το λογαριασμό σας Baidu.

    ![][4]

Όταν έχετε ένα λογαριασμό Baidu ενεργοποιημένη, συνδεθείτε στην [πύλη Baidu].

##<a name="register-as-a-baidu-developer"></a>Καταχώρηση ως προγραμματιστής Baidu

1. Αφού έχετε συνδεθεί [Baidu πύλη], κάντε κλικ στην επιλογή**更多 >>** (**περισσότερα**).

    ![][5]

2. Κύλιση προς τα κάτω στην ενότητα**站长与开发者服务 (υπεύθυνου τοποθεσίας Web και των υπηρεσιών για προγραμματιστές)** και κάντε κλικ στο**百度开放云平台**(**Baidu Άνοιγμα cloud πλατφόρμα**).

    ![][6]

3. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή**开发者服务**(**Developer Services**) στην επάνω δεξιά γωνία.

    ![][7]

4. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή**注册开发者**(**Registered προγραμματιστές**) από το μενού στην επάνω δεξιά γωνία.

    ![][8]

5. Πληκτρολογήστε το όνομα, περιγραφή και έναν αριθμό κινητού τηλεφώνου για τη λήψη ενός μηνύματος κειμένου επαλήθευση και, στη συνέχεια, κάντε κλικ στην επιλογή**送验证码**(**Αποστολή κωδικό επαλήθευσης**). Σημειώστε ότι για διεθνών αριθμών τηλεφώνου, πρέπει να περικλείσετε τον κωδικό χώρας σε παρενθέσεις. Για παράδειγμα, για έναν αριθμό Ηνωμένες Πολιτείες, θα είναι **(1) 1234567890**.

    ![][9]

6. Στη συνέχεια, θα πρέπει να λάβετε ένα μήνυμα κειμένου με έναν κωδικό επαλήθευσης, όπως φαίνεται στο παρακάτω παράδειγμα:

    ![][10]

7. Εισαγάγετε τον κωδικό επαλήθευσης από το μήνυμα στο**验证码**(**Κωδικός επιβεβαίωσης**).

8. Τέλος, ολοκληρώστε την καταχώρηση για προγραμματιστές, αποδεχτείτε τη συμφωνία Baidu και κάνοντας κλικ στην επιλογή**提交**(**Υποβολή**). Θα δείτε την ακόλουθη σελίδα μετά την επιτυχή ολοκλήρωση των εγγραφής:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Δημιουργία έργου Baidu cloud push

Όταν δημιουργείτε ένα έργο Baidu cloud push, λαμβάνετε το Αναγνωριστικό εφαρμογής, API κλειδί και μυστικό κλειδί.

1. Αφού έχετε συνδεθεί [Baidu πύλη], κάντε κλικ στην επιλογή**更多 >>** (**περισσότερα**).

    ![][5]

2. Κύλιση προς τα κάτω στην ενότητα**站长与开发者服务**(**υπεύθυνου τοποθεσίας Web και των υπηρεσιών για προγραμματιστές**) και κάντε κλικ στο**百度开放云平台**(**Baidu Άνοιγμα cloud πλατφόρμα**).

    ![][6]

3. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή**开发者服务**(**Developer Services**) στην επάνω δεξιά γωνία.

    ![][7]

4. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή**云推送**(**Cloud Push**) από την ενότητα**云服务**(**Cloud Services**).

    ![][12]

5. Μόλις συνδεθείτε καταχωρημένες προγραμματιστής, θα δείτε**管理控制台**(**Management Console**) στην επάνω γραμμή μενού. Κάντε κλικ στην επιλογή**开发者服务管理**(**προγραμματιστές υπηρεσίας διαχείρισης**).

    ![][13]

6. Στην επόμενη σελίδα, κάντε κλικ στην επιλογή**创建工程**(**Δημιουργία έργου**).

    ![][14]

7. Πληκτρολογήστε ένα όνομα εφαρμογής και κάντε κλικ στην επιλογή**创建**(**Δημιουργία**).

    ![][15]

8. Κατά την επιτυχή δημιουργία ενός έργου Baidu cloud push, θα δείτε μια σελίδα με το **αναγνωριστικό εφαρμογής**, **API κλειδί**και **Μυστικό κλειδί**. Σημειώστε το κλειδί API και μυστικό κλειδί, το οποίο θα χρησιμοποιήσουμε αργότερα.

    ![][16]

9. Ρυθμίστε τις παραμέτρους του έργου για τις ειδοποιήσεις push κάνοντας κλικ στην επιλογή**云推送**(**Cloud Push**) στο αριστερό τμήμα του παραθύρου.

    ![][31]

10. Στην επόμενη σελίδα, κάντε κλικ στο κουμπί**推送设置**(**Push ρυθμίσεις**).

    ![][32]  

11. Στη σελίδα Ρύθμιση παραμέτρων, προσθέστε το όνομα του πακέτου που θα είναι χρησιμοποιώντας στο έργο σας Android στο πεδίο**应用包名**(**πακέτο εφαρμογών**), και, στη συνέχεια, κάντε κλικ στην επιλογή**保存设置**(**Αποθήκευση**).  

    ![][33]

Θα δείτε το**保存成功!** (**με επιτυχία αποθηκευμένο!**) μηνύματος.

##<a name="configure-your-notification-hub"></a>Ρύθμιση των παραμέτρων σας διανομέα ειδοποίησης

1. Πραγματοποιήστε είσοδο στην [Πύλη κλασική Azure], και, στη συνέχεια, κάντε κλικ στο κουμπί **+ ΔΗΜΙΟΥΡΓΊΑ** στο κάτω μέρος της οθόνης.

2. Κάντε κλικ στην επιλογή **Εφαρμογή υπηρεσιών**, κάντε κλικ στην επιλογή **Bus υπηρεσίας**, κάντε κλικ στην επιλογή **Ειδοποίηση διανομέας**και, στη συνέχεια, κάντε κλικ στην επιλογή **Γρήγορη δημιουργία**.

3. Δώστε ένα όνομα για την **Ειδοποίηση διανομέα**, επιλέξτε την **περιοχή** και **Namespace** όπου θα δημιουργηθεί αυτό διανομέα ειδοποίηση, και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία διανομέα νέα ειδοποίηση**.  

    ![][17]

4. Κάντε κλικ στην επιλογή ο χώρος ονομάτων στο οποίο δημιουργήσατε το Κέντρο ειδοποίηση σας και, στη συνέχεια, κάντε κλικ στην επιλογή **Ειδοποίηση διανομείς** στο επάνω μέρος.

    ![][18]

5. Επιλέξτε την ενότητα ειδοποίησης που δημιουργήσατε και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων** από το επάνω μενού.

    ![][19]

6. Κάντε κύλιση προς τα κάτω στην ενότητα **ρυθμίσεις ειδοποιήσεων baidu** και πληκτρολογήστε το κλειδί API και μυστικό κλειδί που λάβατε από την κονσόλα Baidu προηγουμένως για το έργο σας Baidu cloud push. Κάντε κλικ στην επιλογή **Αποθήκευση**.

    ![][20]

7. Κάντε κλικ στην καρτέλα **πίνακα εργαλείων** στο επάνω μέρος για την ενότητα "Ειδοποίηση" και, στη συνέχεια, κάντε κλικ στην επιλογή **Προβολή συμβολοσειρά σύνδεσης**.

    ![][21]

8. Σημειώστε την **DefaultListenSharedAccessSignature** και **DefaultFullSharedAccessSignature** από το παράθυρο **πληροφορίες σύνδεσης της Access** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Σύνδεση της εφαρμογής σας με την ενότητα "Ειδοποίηση"

1. Στο ADT Έκλειψη, δημιουργήστε ένα νέο έργο Android (**αρχείο** > **Δημιουργία** > **Android έργο εφαρμογών**).

    ![][23]

2. Πληκτρολογήστε ένα **Όνομα εφαρμογής** και βεβαιωθείτε ότι η έκδοση **SDK απαιτείται ελάχιστη** έχει οριστεί σε **API 16: Android 4.1**.

    ![][24]

3. Κάντε κλικ στο κουμπί **Επόμενο** και συνεχίστε παρακάτω του οδηγού, μέχρι να εμφανιστεί το παράθυρο **Δημιουργία δραστηριότητας** . Βεβαιωθείτε ότι είναι επιλεγμένο το **Κενό δραστηριότητας** και τέλος επιλέξτε **Τέλος** για να δημιουργήσετε μια νέα εφαρμογή Android.

    ![][25]

4. Βεβαιωθείτε ότι **Προορισμού Δημιουργία έργου** έχει ρυθμιστεί σωστά.

    ![][26]

5. Κάντε λήψη του αρχείου ειδοποίηση-διανομείς-0.4.jar από την καρτέλα **αρχεία** του [Ειδοποίηση-διανομείς-Android-SDK στην Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Προσθέστε το αρχείο στο φάκελο **βιβλιοθηκών** του έργου σας Έκλειψη και ανανεώστε το φάκελο *βιβλιοθηκών* .

6. Λήψη και αποσυμπιέστε το [Baidu Push Android SDK], ανοίξτε το φάκελο, **βιβλιοθηκών** και, στη συνέχεια, αντιγράψτε το αρχείο βάζο **pushservice-x.y.z και** και το **armeabi** & **mips** φακέλους στο φάκελο **βιβλιοθηκών** της εφαρμογής σας Android.

7. Ανοίξτε το αρχείο **AndroidManifest.xml** του έργου σας Android και προσθέστε τα δικαιώματα που απαιτούνται από το SDK Baidu.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Προσθέστε την ιδιότητα **android: όνομα** για το στοιχείο **εφαρμογή** σε **AndroidManifest.xml**, αντικαθιστώντας *yourprojectname* (π.χ., **com.example.BaiduTest**). Βεβαιωθείτε ότι αυτό το όνομα έργου ταιριάζει με αυτό που έχει ρυθμιστεί στην κονσόλα Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Προσθέστε τις ακόλουθες ρυθμίσεις παραμέτρων μέσα στο στοιχείο εφαρμογή μετά το **. MainActivity** στοιχείο δραστηριότητας, αντικαθιστώντας *yourprojectname* (π.χ., **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Προσθέστε μια νέα κλάση που ονομάζεται **ConfigurationSettings.java** στο έργο.

    ![][28]

    ![][29]

10. Προσθέστε τον ακόλουθο κώδικα σε αυτήν:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Ορίστε την τιμή της **API_KEY** με που ανακτήθηκαν από το Baidu cloud έργο νωρίτερα, **NotificationHubName** με το όνομα διανομέας σας ειδοποίησης από την πύλη κλασική Azure και **NotificationHubConnectionString** με DefaultListenSharedAccessSignature από την πύλη κλασική Azure.

11. Προσθέσετε μια νέα κατηγορία που ονομάζεται **DemoApplication.java**και προσθέστε τον ακόλουθο κώδικα σε αυτήν:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Προσθήκη άλλου νέα κλάση που ονομάζεται **MyPushMessageReceiver.java**και προσθήκη του κώδικα κάτω από αυτό. Αυτή είναι η κλάση που χειρίζεται τις ειδοποιήσεις push που λαμβάνονται από το διακομιστή push Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Ανοίξτε **MainActivity.java**και προσθέστε τη μέθοδο **onCreate** τα εξής:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Ανοίξτε τις ακόλουθες δηλώσεις εισαγωγής στο επάνω μέρος:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Αποστολή ειδοποιήσεων για την εφαρμογή σας


Μπορείτε γρήγορα να δοκιμάσετε να λαμβάνω ειδοποιήσεις στην εφαρμογή σας με την αποστολή ειδοποιήσεων στην [Πύλη του Azure](https://portal.azure.com/) χρησιμοποιώντας το κουμπί **Δοκιμή αποστολή** στην ενότητα "Ειδοποίηση", όπως φαίνεται στην παρακάτω οθόνη.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Ειδοποιήσεις Push κανονικά αποστέλλονται σε μια υπηρεσία παρασκηνίου, όπως υπηρεσίες Mobile ή ASP.NET χρησιμοποιώντας μια συμβατή βιβλιοθήκη. Μπορείτε επίσης να χρησιμοποιήσετε το REST API απευθείας, για να στείλετε μηνύματα ειδοποιήσεων εάν μια βιβλιοθήκη δεν είναι διαθέσιμη για το παρασκηνίου.

Σε αυτό το πρόγραμμα εκμάθησης, θα Διατηρήστε το απλό και μόνο δείχνουν δοκιμή της εφαρμογής υπολογιστή-πελάτη σας με την αποστολή ειδοποιήσεων με χρήση του SDK .NET για διανομείς ειδοποίηση σε μια εφαρμογή κονσόλας αντί για μια υπηρεσία υποστήριξης. Συνιστάται να το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για τις ειδοποιήσεις push σε χρήστες](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) με το επόμενο βήμα για την αποστολή ειδοποιήσεων από μια παρασκηνίου ASP.NET. Ωστόσο, οι ακόλουθες προσεγγίσεις μπορεί να χρησιμοποιηθεί για την αποστολή ειδοποιήσεων:

* **Περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ**: που μπορεί να υποστηρίξει ειδοποίηση σε οποιαδήποτε πλατφόρμα παρασκηνίου χρησιμοποιώντας το [ΥΠΌΛΟΙΠΟ περιβάλλοντος εργασίας](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure ειδοποίηση διανομείς .NET SDK**: στο το Nuget διαχείρισης πακέτου για το Visual Studio, εκτελέστε [Microsoft.Azure.NotificationHubs πακέτου εγκατάστασης](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Εφαρμογές του Mobile**: για παράδειγμα του τρόπου για την αποστολή ειδοποιήσεων από μια παρασκηνίου Azure εφαρμογής υπηρεσίας κινητών εφαρμογών που είναι ενσωματωμένο στο διανομείς ειδοποίησης, ανατρέξτε στο θέμα [Προσθήκη ειδοποιήσεων push για την εφαρμογή για κινητές συσκευές](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: για ένα παράδειγμα του τρόπου για την αποστολή ειδοποιήσεων, χρησιμοποιώντας τα ΥΠΌΛΟΙΠΑ API, ανατρέξτε στην ενότητα "Πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση από Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων από μια εφαρμογή κονσόλας .NET.

Σε αυτήν την ενότητα, θα σας δείξουμε στέλνει μια ειδοποίηση χρήση εφαρμογής κονσόλας .NET.

1. Δημιουργία νέας εφαρμογής κονσόλας Visual C#:

    ![][30]

2. Στο παράθυρο Κονσόλα διαχείρισης πακέτου, ορίστε την **προεπιλεγμένη έργου** στο έργο σας νέα εφαρμογή κονσόλας και, στη συνέχεια, στο παράθυρο της κονσόλας, εκτελέστε την ακόλουθη εντολή:

        Install-Package Microsoft.Azure.NotificationHubs

    Αυτό προσθέτει μια αναφορά στο SDK διανομείς ειδοποίηση Azure χρησιμοποιώντας το <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">πακέτο Microsoft.Azure.Notification NuGet διανομείς</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Ανοίξτε το αρχείο **Program.cs** και προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using Microsoft.Azure.NotificationHubs;

4. Στο σας `Program` κλάση, προσθέστε την ακόλουθη μέθοδο και αντικατάσταση *DefaultFullSharedAccessSignatureSASConnectionString* και *NotificationHubName* με τις τιμές που έχετε.

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Προσθέστε τις ακόλουθες γραμμές στη μέθοδο **κύριες** σας:

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Δοκιμή της εφαρμογής σας

Για να δοκιμάσετε αυτήν την εφαρμογή με μια πραγματική τηλέφωνο, απλώς συνδέστε το τηλέφωνο στον υπολογιστή σας με χρήση καλωδίου USB. Αυτό φορτώνει την εφαρμογή σας σε τηλέφωνο συνημμένων.

Για να δοκιμάσετε αυτήν την εφαρμογή με το προσομοίωσης, στη γραμμή εργαλείων επάνω Έκλειψη, κάντε κλικ στην επιλογή **Εκτέλεση**και, στη συνέχεια, επιλέξτε την εφαρμογή σας. Αυτό ξεκινά το προσομοίωσης, και, στη συνέχεια, φορτώνει και εκτελεί την εφαρμογή.

Η εφαρμογή ανακτά το 'αναγνωριστικό χρήστη' και 'channelId' από την υπηρεσία ειδοποιήσεων Baidu Push και καταγράφει με την ενότητα "Ειδοποίηση".

Για να στείλετε μια ειδοποίηση δοκιμής που μπορείτε να χρησιμοποιήσετε την καρτέλα εντοπισμού σφαλμάτων της πύλης κλασική Azure. Εάν δημιουργηθεί η εφαρμογή κονσόλας .NET για το Visual Studio, απλώς πατήστε το πλήκτρο F5 στο Visual Studio για να εκτελέσετε την εφαρμογή. Η εφαρμογή θα σας στέλνει μια ειδοποίηση που θα εμφανίζεται στην περιοχή ειδοποιήσεων επάνω από τη συσκευή ή προσομοίωσης.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Υπηρεσίες κινητές συσκευές Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Android Baidu Push SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure κλασική πύλη]: https://manage.windowsazure.com/
[Πύλη Baidu]: http://www.baidu.com/

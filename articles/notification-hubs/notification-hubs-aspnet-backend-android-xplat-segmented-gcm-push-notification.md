<properties
    pageTitle="Ειδοποίηση διανομείς διακοπή ειδήσεις προγραμμάτων εκμάθησης - Android"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Service Bus ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων πρόσφατες ειδήσεις για συσκευές Android."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση Azure μετάδοση πρόσφατες ειδήσεις ειδοποιήσεων για την εφαρμογή της Android. Όταν ολοκληρωθεί η εγκατάσταση, θα έχετε τη δυνατότητα να εγγραφείτε για διακοπή κατηγορίες συζητήσεων που σας ενδιαφέρει και λήψη μόνο ειδοποιήσεων push για αυτές τις κατηγορίες. Αυτό το σενάριο είναι ένα κοινό μοτίβο για πολλές εφαρμογές όπου έχουν ειδοποιήσεις θα αποσταλούν σε ομάδες χρηστών που έχουν προηγουμένως δηλωθεί ως ενδιαφέροντος σε αυτά, π.χ., πρόγραμμα ανάγνωσης RSS, εφαρμογές για το ανεμιστήρες μουσικής, κ.λπ.

Εκπομπή σενάρια είναι ενεργοποιημένα, συμπεριλαμβάνοντας μία ή περισσότερες _ετικέτες_ κατά τη δημιουργία μιας εγγραφής στην ενότητα ειδοποίησης. Όταν μια ετικέτα αποστέλλονται ειδοποιήσεις, όλες τις συσκευές που έχετε καταχωρήσει για την ετικέτα θα λάβετε την ειδοποίηση. Επειδή οι ετικέτες είναι απλώς συμβολοσειρές, δεν χρειάζεται να γίνει παροχή εκ των προτέρων. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε [ειδοποίηση διανομείς δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το θέμα δημιουργεί στην εφαρμογή του που δημιουργήσατε [γρήγορα]αποτελέσματα με το διανομείς ειδοποίηση[get-started]. Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ήδη ολοκληρώσει [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση][get-started].

##<a name="add-category-selection-to-the-app"></a>Προσθήκη επιλογή κατηγορίας στην εφαρμογή

Το πρώτο βήμα είναι να προσθέσετε τα στοιχεία του περιβάλλοντος εργασίας Χρήστη στις υπάρχουσες κύριο τη δραστηριότητά σας που επιτρέπει στο χρήστη να επιλέξει κατηγορίες για την καταχώρηση. Οι κατηγορίες που επιλέγονται από ένα χρήστη είναι αποθηκευμένα στη συσκευή. Κατά την εκκίνηση της εφαρμογής, μια καταχώρηση της συσκευής έχει δημιουργηθεί στο κέντρο σας ειδοποίηση με επιλεγμένες κατηγορίες ως ετικέτες.

1. Ανοίξτε το αρχείο σας res/layout/activity_main.xml και αντικαταστήστε το περιεχόμενο με τα εξής:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Ανοίξτε το αρχείο res/values/strings.xml και προσθέστε τις ακόλουθες γραμμές:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Διάταξη γραφικών σας main_activity.xml πρέπει τώρα να μοιάζει ως εξής:

    ![][A1]

3. Τώρα μπορείτε να δημιουργήσετε ένα εκπαιδευτικό **ειδοποιήσεις** στο ίδιο πακέτο ως τάξη σας **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Αυτή η κλάση χρησιμοποιεί την τοπική αποθήκευση για να αποθηκεύσετε τις κατηγορίες συζητήσεων που έχει αυτήν τη συσκευή για να λάβετε. Περιέχει επίσης μεθόδους για να εγγραφείτε για αυτές τις κατηγορίες.


4. Στην τάξη σας **MainActivity** κατάργηση ιδιωτικών πεδία σας για **NotificationHub** και **GoogleCloudMessaging**και να προσθέσετε ένα πεδίο για τις **ειδοποιήσεις**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Στη συνέχεια, στη μέθοδο **onCreate** , καταργήστε την προετοιμασία του πεδίου **διανομέας** και τη μέθοδο **registerWithNotificationHubs** . Στη συνέχεια, προσθέστε τις ακόλουθες γραμμές που προετοιμασία της παρουσίας της κλάσης **ειδοποιήσεις** . 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`και `HubListenConnectionString` ήδη θα πρέπει να οριστεί με το `<hub name>` και `<connection string with listen access>` σύμβολα κράτησης θέσης με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultListenSharedAccessSignature* που λάβατε νωρίτερα.

    > [AZURE.NOTE] Επειδή τα διαπιστευτήρια που διανέμονται με μια εφαρμογή προγράμματος-πελάτη δεν είναι γενικά ασφαλής, πρέπει να διανείμετε μόνο το κλειδί για ακρόαση πρόσβαση με την εφαρμογή υπολογιστή-πελάτη σας. Ακούστε access παρέχει τη δυνατότητα να εγγραφείτε για τις ειδοποιήσεις, αλλά υπάρχουσες καταχωρήσεις εφαρμογή σας δεν μπορεί να τροποποιηθεί και δεν είναι δυνατό να αποστέλλονται ειδοποιήσεις. Χρησιμοποιείται το κλειδί πλήρη πρόσβαση σε μια υπηρεσία ασφαλούς υποστήριξης για την αποστολή ειδοποιήσεων και αλλαγή υπάρχουσες καταχωρήσεις.


6. Στη συνέχεια, προσθέστε τις ακόλουθες εισαγωγές και `subscribe` μέθοδο για το χειρισμό στο κουμπί εγγραφή, κάντε κλικ στην επιλογή συμβάν:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Αυτή η μέθοδος δημιουργεί μια λίστα κατηγοριών και χρησιμοποιεί την κλάση **ειδοποιήσεις** για την αποθήκευση της λίστας στο χώρο αποθήκευσης της τοπικής και καταχώρηση τις αντίστοιχες ετικέτες με το Κέντρο ειδοποίηση σας. Όταν αλλάζουν οι κατηγορίες, η καταχώρηση αναδημιουργείται με τις νέες κατηγορίες.

Εφαρμογή σας είναι τώρα μπορείτε να αποθηκεύσετε ένα σύνολο κατηγοριών στον τοπικό χώρο αποθήκευσης στη συσκευή και καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ο χρήστης αλλάζει την επιλογή των κατηγοριών.

##<a name="register-for-notifications"></a>Εγγραφή για ειδοποιήσεις

Αυτά τα βήματα καταχωρήστε την ενότητα "Ειδοποίηση" κατά την εκκίνηση χρησιμοποιώντας τις κατηγορίες που έχουν αποθηκευτεί στον τοπικό χώρο αποθήκευσης.

> [AZURE.NOTE] Επειδή το registrationId που έχουν εκχωρηθεί από Google Cloud ανταλλαγής μηνυμάτων (GCM) μπορεί να αλλάξουν οποιαδήποτε στιγμή, πρέπει να δηλώσετε για τις ειδοποιήσεις συχνά για να αποφύγετε την ειδοποίηση αποτυχίες. Αυτό το παράδειγμα καταχωρεί για ειδοποίηση κάθε φορά που ξεκινά η εφαρμογή. Για τις εφαρμογές που εκτελούνται συχνά, περισσότερες από μία φορά την ημέρα, μπορείτε να πιθανώς παραλείψετε εγγραφής για να διατηρήσετε το εύρος ζώνης, εάν έχει περάσει μικρότερη από την ημέρα από την προηγούμενη εγγραφή.


1. Προσθέστε τον ακόλουθο κώδικα στο τέλος της μεθόδου **onCreate** στην τάξη **MainActivity** :

        notifications.subscribeToCategories(notifications.retrieveCategories());

    Αυτό διασφαλίζει ότι κάθε φορά που ξεκινά η εφαρμογή ανακτά τις κατηγορίες από τοπικού χώρου αποθήκευσης και ζητά ένα registeration για αυτές τις κατηγορίες. 

2. Στη συνέχεια, ενημερώστε το `onStart()` μέθοδο το `MainActivity` κλάση ως εξής:

    @Overrideπροστατευμένο void onStart() {super.onStart();      isVisible = true.

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    Αυτό ενημερώνει το κύριο δραστηριότητας με βάση την κατάσταση των κατηγοριών είχε αποθηκευτεί προηγουμένως.

Η εφαρμογή είναι τώρα ολοκληρωμένο και να αποθηκεύσετε ένα σύνολο κατηγορίες σε συσκευή τοπικό χώρο αποθήκευσης χρησιμοποιούνται για την καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ο χρήστης αλλάζει την επιλογή των κατηγοριών. Στη συνέχεια, θα σας θα ορίσετε έναν υπολογιστή στο παρασκήνιο που μπορεί να σας στείλει ειδοποιήσεις κατηγορία αυτής της εφαρμογής.

##<a name="sending-tagged-notifications"></a>Αποστολή ειδοποιήσεων με ετικέτες

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Εκτελέστε την εφαρμογή και Δημιουργία ειδοποιήσεων

1. Στο Android Studio, δημιουργήστε την εφαρμογή και ξεκινήστε το σε μια συσκευή ή προσομοίωσης.

    Σημειώστε ότι η εφαρμογή περιβάλλοντος εργασίας Χρήστη παρέχει ένα σύνολο εναλλαγή που σας επιτρέπει να επιλέξετε τις κατηγορίες για να εγγραφείτε για.

2. Ενεργοποίηση ενός ή περισσότερων εναλλαγή κατηγορίες και, στη συνέχεια, κάντε κλικ στην επιλογή **εγγραφή**.

    Η εφαρμογή μετατρέπει επιλεγμένες κατηγορίες σε ετικέτες και τις αιτήσεις μια νέα καταχώρηση της συσκευής για το επιλεγμένο ετικέτες από την ενότητα "Ειδοποίηση". Οι καταχωρημένες κατηγορίες είναι επιστρέφεται και εμφανίζεται σε μια αναδυόμενη ειδοποίηση.

4. Στείλτε μια ειδοποίηση για νέα, εκτελώντας την εφαρμογή κονσόλας .NET.  Εναλλακτικά, μπορείτε να στείλετε ειδοποιήσεις προτύπου με ετικέτες χρησιμοποιώντας την καρτέλα εντοπισμού σφαλμάτων από το Κέντρο ειδοποίηση σας στην [Πύλη κλασική Azure].

    Ειδοποιήσεις για επιλεγμένες κατηγορίες εμφανίζονται ως αναδυόμενη ειδοποιήσεις.

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, θα σας μάθατε τον τρόπο εκπομπής πρόσφατες ειδήσεις ανά κατηγορία. Εξετάστε την ολοκλήρωση ένα από τα παρακάτω προγράμματα εκμάθησης που επισημαίνουν άλλα σενάρια διανομείς ειδοποίησης για προχωρημένους:

+ [Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]

    Μάθετε πώς μπορείτε να αναπτύξετε την εφαρμογή πρόσφατες ειδήσεις για να ενεργοποιήσετε την αποστολή ειδοποιήσεων μεταφρασμένα.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Azure κλασική πύλη]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591

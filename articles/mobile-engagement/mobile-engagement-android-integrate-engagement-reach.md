<properties
    pageTitle="Ενοποίηση Android SDK Azure δέσμευση κινητές συσκευές"
    description="Πιο πρόσφατες ενημερώσεις και διαδικασίες για Android SDK για δέσμευση Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Πώς μπορείτε να ενοποιήσετε Reach δέσμευση σε Android

> [AZURE.IMPORTANT] Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφονται στο το πώς μπορείτε να ενοποιήσετε δέσμευση σε Android εγγράφου πριν να ακολουθήσετε αυτόν τον οδηγό.

##<a name="standard-integration"></a>Τυπική ενοποίησης

Το SDK επίτευξη απαιτεί τη **βιβλιοθήκη Android υποστήριξης (v4)**.

Είναι ο πιο γρήγορος τρόπος για να προσθέσετε στη βιβλιοθήκη του έργου σας στο **Έκλειψη** `Right click on your project -> Android Tools -> Add Support Library...`.

Εάν δεν χρησιμοποιείτε το Έκλειψη, μπορείτε να διαβάσετε τις οδηγίες [εδώ].

Αντιγραφή αρχείων πόρων Reach από το SDK στο έργο σας:

-   Αντιγράψτε τα αρχεία από το `res/layout` φακέλου που παρέχεται με το SDK σε το `res/layout` φάκελο της εφαρμογής σας.
-   Αντιγράψτε τα αρχεία από το `res/drawable` φακέλου που παρέχεται με το SDK σε το `res/drawable` φάκελο της εφαρμογής σας.

Επεξεργασία του `AndroidManifest.xml` αρχείο:

-   Προσθέστε την παρακάτω ενότητα (μεταξύ του `<application>` και `</application>` ετικέτες):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Χρειάζεστε αυτό το δικαίωμα για επανάληψη αναπαραγωγής της ειδοποιήσεις συστήματος που δεν έχουν ενεργοποιηθεί κατά την εκκίνηση (διαφορετικά θα διατηρηθούν στο δίσκο, αλλά δεν θα εμφανίζονται πλέον, έχετε πραγματικά να συμπεριλάβετε αυτό).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Καθορίστε ένα εικονίδιο που χρησιμοποιείται για τις ειδοποιήσεις (και τα δύο στην εφαρμογή και στο σύστημα αυτά), αντιγράφετε και επεξεργάζεστε την παρακάτω ενότητα (μεταξύ του `<application>` και `</application>` ετικέτες):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Αυτή η ενότητα είναι **υποχρεωτική** εάν σκοπεύετε σχετικά με τη χρήση ειδοποιήσεις συστήματος κατά τη δημιουργία Reach εκστρατείες. Android αποτρέπει την εμφάνισή ειδοποιήσεις συστήματος χωρίς εικονίδια. Επομένως, εάν παραλείψετε αυτή την ενότητα, τους τελικούς χρήστες σας δεν θα μπορείτε να λαμβάνετε.

-   Εάν δημιουργήσετε εκστρατείες με χρήση γενική εικόνα ειδοποιήσεις συστήματος, πρέπει να προσθέσετε τα ακόλουθα δικαιώματα (μετά το `</application>` ετικέτα) Εάν λείπουν:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Σε Android M και εάν η εφαρμογή σας ως προορισμό επίπεδο Android API 23 ή μεγαλύτερο, ``WRITE_EXTERNAL_STORAGE`` δικαιωμάτων απαιτεί έγκριση χρήστη. Διαβάστε [αυτήν την ενότητα](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Για τις ειδοποιήσεις συστήματος μπορείτε επίσης να καθορίσετε στην εκστρατεία Reach εάν η συσκευή θα πρέπει να καλείται ή/και τη δόνηση. Για να λειτουργήσει, πρέπει να βεβαιωθείτε ότι δηλωθεί τα ακόλουθα δικαιώματα (μετά το `</application>` ετικέτα):

            <uses-permission android:name="android.permission.VIBRATE" />

    Χωρίς αυτό το δικαίωμα, Android αποτρέπει ειδοποιήσεων συστήματος που φαίνεται εάν που επιλέξατε την κλήση ή στην επιλογή vibrate στη Διαχείριση επίτευξη εκστρατείας.

-   Εάν δημιουργήσετε την εφαρμογή σας χρησιμοποιώντας **ProGuard** και έχετε σφαλμάτων που σχετίζονται με τη βιβλιοθήκη Android υποστήριξης ή τη δέσμευση βάζο, προσθέστε τις ακόλουθες γραμμές για να σας `proguard.cfg` αρχείο:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Εγγενής Push

Τώρα που έχετε ρυθμίσει τις παραμέτρους Reach λειτουργικής μονάδας, πρέπει να ρυθμίσετε τις παραμέτρους εγγενούς push να μπορούν να λαμβάνουν τις καμπάνιες στη συσκευή.

Υποστηρίζουμε δύο υπηρεσίες σε Android:

  - Συσκευές Google Play: χρήση [Ανταλλαγής μηνυμάτων για το Google Cloud] , ακολουθώντας το [πώς μπορείτε να ενοποιήσετε GCM με τον Οδηγό δέσμευση](mobile-engagement-android-gcm-integrate.md) οδηγός.
  - Συσκευές Amazon: χρήση [Ανταλλαγής μηνυμάτων για το Amazon συσκευή] , ακολουθώντας το [πώς μπορείτε να ενοποιήσετε ADM με τον Οδηγό δέσμευση](mobile-engagement-android-adm-integrate.md) οδηγός.

Εάν θέλετε να στοχεύσετε Amazon και Google Play συσκευές, τις πιθανές να έχουν όλα τα στοιχεία μέσα σε 1 AndroidManifest.xml/APK για ανάπτυξη. Ωστόσο, κατά την υποβολή για το Amazon, ενδέχεται να απορρίπτει την εφαρμογή σας εάν βρει GCM κώδικα.

Θα πρέπει να χρησιμοποιήσετε πολλά APKs σε αυτήν την περίπτωση.

**Η εφαρμογή σας είναι τώρα έτοιμο για λήψη και να εμφανίσετε εκστρατείες reach!**

##<a name="how-to-handle-data-push"></a>Τον τρόπο χειρισμού των δεδομένων push

### <a name="integration"></a>Ενοποίηση

Εάν θέλετε την εφαρμογή σας να μπορούν να λαμβάνουν Reach ωθεί δεδομένων, πρέπει να δημιουργήσετε μια δευτερεύουσα κλάση του `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` και αναφοράς με την `AndroidManifest.xml` αρχείου (μεταξύ του `<application>` ή/και `</application>` ετικέτες):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Στη συνέχεια, μπορείτε να παρακάμψετε τη `onDataPushStringReceived` και `onDataPushBase64Received` επιστροφές κλήσης. Ακολουθεί ένα παράδειγμα:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Κατηγορία

Η παράμετρος κατηγορία είναι προαιρετική κατά τη δημιουργία μιας εκστρατείας Push δεδομένων και σας επιτρέπει να φιλτράρετε δεδομένα προωθεί. Αυτό είναι χρήσιμο εάν έχετε πολλές εκπομπής δέκτες χειρισμός διαφορετικούς τύπους δεδομένων ωθεί, ή εάν θέλετε να push διαφορετικά είδη των `Base64` δεδομένων και θέλετε να προσδιορίσετε τον τύπο τους πριν από την ανάλυση τους.

### <a name="callbacks-return-parameter"></a>Παράμετρος επιστροφής των επιστροφές κλήσης

Εδώ θα βρείτε ορισμένες οδηγίες για το χειρισμό σωστά η παράμετρος επιστροφής της `onDataPushStringReceived` και `onDataPushBase64Received`:

-   Θα πρέπει να επιστρέψει μια ακουστικό εκπομπής `null` στην επιστροφή κλήσης εάν δεν γνωρίζει τον τρόπο χειρισμού των πάτημα δεδομένων. Θα πρέπει να χρησιμοποιείτε την κατηγορία για να προσδιορίσετε εάν το ακουστικό εκπομπής θα χειρίζεται το πάτημα δεδομένων ή όχι.
-   Ένα από το ακουστικό εκπομπής πρέπει να επιστρέψει `true` στην επιστροφή κλήσης εάν να δέχεται το πάτημα δεδομένων.
-   Ένα από το ακουστικό εκπομπής πρέπει να επιστρέψει `false` στην επιστροφή κλήσης εάν αναγνωρίζει το πάτημα δεδομένων, αλλά απορρίπτει το για οποιονδήποτε λόγο. Για παράδειγμα, επιστρέψει `false` όταν τα ληφθέντα δεδομένα δεν είναι έγκυρα.
-   Εάν μία εκπομπή επιστρέφει ακουστικό `true` κατά την άλλη μία επιστρέφει `false` για την ίδια push δεδομένων, η συμπεριφορά δεν έχει οριστεί, που πρέπει να το κάνετε ποτέ.

Ο επιστρεφόμενος τύπος χρησιμοποιείται μόνο για τα στατιστικά στοιχεία Reach:

-   `Replied`αυξάνεται εάν ένα από τα εκπομπής δέκτες επιστραφεί είτε `true` ή `false`.
-   `Actioned`αυξάνεται μόνο εάν ένα από τα εκπομπής δέκτες επιστρέφονται `true`.

##<a name="how-to-customize-campaigns"></a>Πώς μπορείτε να προσαρμόσετε εκστρατείες

Για να προσαρμόσετε εκστρατείες, μπορείτε να τροποποιήσετε τις διατάξεις που παρέχονται στο SDK επίτευξη.

Πρέπει να διατηρήσετε όλα τα αναγνωριστικά που χρησιμοποιούνται σε διατάξεις και να διατηρήσετε τους τύπους των προβολών που χρησιμοποιούν ένα αναγνωριστικό, ειδικά για κείμενο και εικόνα προβολές. Ορισμένες προβολές χρησιμοποιούνται μόνο για να αποκρύψετε ή να εμφανίσετε περιοχές, ώστε να μπορεί να αλλάξει τον τύπο τους. Ελέγξτε τον κωδικό προέλευσης, εάν πρόκειται να αλλάξετε τον τύπο της προβολής σε παρεχόμενη διατάξεις.

### <a name="notifications"></a>Ειδοποιήσεις

Υπάρχουν δύο τύποι των ειδοποιήσεων: ειδοποιήσεις συστήματος και της εφαρμογής που χρησιμοποιούν διαφορετική διάταξη αρχεία.

#### <a name="system-notifications"></a>Ειδοποιήσεις συστήματος

Για να προσαρμόσετε τις ειδοποιήσεις συστήματος πρέπει να χρησιμοποιήσετε τις **κατηγορίες**. Μπορείτε να μεταβείτε σε [κατηγορίες](#categories).

#### <a name="in-app-notifications"></a>Ειδοποιήσεις της εφαρμογής

Από προεπιλογή, μια ειδοποίηση της εφαρμογής είναι μια προβολή που προστίθεται δυναμικά το τρέχον περιβάλλον εργασίας χρήστη δραστηριότητας ευχαριστήριο τη μέθοδο του Android `addContentView()`. Αυτή η διαδικασία ονομάζεται μια επικάλυψη ειδοποίησης. Ειδοποίηση επικαλύψεων είναι ιδανικά για μια γρήγορη ενοποίησης επειδή δεν απαιτούν να τροποποιήσετε οποιαδήποτε διάταξη στην εφαρμογή σας.

Για να τροποποιήσετε την εμφάνιση του επικαλύψεων ειδοποίηση, μπορείτε απλώς να τροποποιήσετε το αρχείο `engagement_notification_area.xml` για τις ανάγκες σας.

> [AZURE.NOTE] Το αρχείο `engagement_notification_overlay.xml` είναι εκείνο που χρησιμοποιείται για να δημιουργήσετε μια επικάλυψη ειδοποίηση, περιλαμβάνει το αρχείο `engagement_notification_area.xml`. Μπορείτε επίσης να το προσαρμόσετε στις ανάγκες σας (όπως στην περίπτωση τοποθέτησης στην περιοχή ειδοποιήσεων εντός της επικάλυψης).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Συμπεριλάβετε διάταξη ειδοποίηση ως μέρος μιας διάταξης δραστηριότητας

Επικαλύψεων είναι ιδανικά για μια γρήγορη ενοποίησης, αλλά μπορεί να είναι ενοχλητικό ή να παρουσιάσει τα αποτελέσματα σε ειδικές περιπτώσεις. Το σύστημα επικάλυψης μπορούν να προσαρμοστούν σε ένα επίπεδο δραστηριότητας, καθιστώντας εύκολη για να αποτρέψετε την πλευρά του εφέ για ειδική δραστηριότητες.

Μπορείτε να αποφασίσετε να συμπεριλάβετε μας διάταξη ειδοποίηση σε την υπάρχουσα διάταξη ευχαριστήριο τη δήλωση **περιλαμβάνουν** Android. Ακολουθεί ένα παράδειγμα ενός τροποποιημένου `ListActivity` διάταξη που περιέχει μόνο ένα `ListView`.

**Πριν από την ενοποίηση δέσμευση:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Μετά την ολοκλήρωση της δέσμευσης:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

Σε αυτό το παράδειγμα έχουμε προσθέσει ένα γονικό κοντέινερ από την αρχική διάταξη που χρησιμοποιήσατε μια προβολή λίστας με το στοιχείο ανώτατου επιπέδου. Έχουμε προσθέσει επίσης `android:layout_weight="1"` για να μπορέσετε να προσθέσετε μια προβολή κάτω από μια προβολή λίστας έχει ρυθμιστεί με `android:layout_height="fill_parent"`.

Το SDK επίτευξη δέσμευση εντοπίζει αυτόματα ότι τη διάταξη ειδοποίηση περιλαμβάνεται σε αυτή τη δραστηριότητα και δεν θα προσθέσει μια επικάλυψη για αυτήν τη δραστηριότητα.

> [AZURE.TIP] Εάν χρησιμοποιείτε μια ListActivity στην εφαρμογή σας, μια ορατή επικάλυψη Reach θα σας εμποδίσει αντιδρούν κλικ πλέον τα στοιχεία στην προβολή λίστας. Αυτό είναι ένα γνωστό θέμα. Για να επιλύσετε αυτό το πρόβλημα προτείνουμε να μπορείτε να ενσωματώσετε στη διάταξη ειδοποίησης στη δική σας διάταξη δραστηριότητας λίστα όπως στο προηγούμενο δείγμα.

##### <a name="disabling-application-notification-per-activity"></a>Απενεργοποίηση ειδοποιήσεων εφαρμογής ανά δραστηριότητα

Εάν δεν θέλετε το επικάλυψη για να προστεθεί τη δραστηριότητά σας και, εάν δεν μπορείτε να συμπεριλάβετε τη διάταξη ειδοποίηση σε τη δική σας διάταξη, μπορείτε να απενεργοποιήσετε την επικάλυψη για αυτήν τη δραστηριότητα στο το `AndroidManifest.xml` , προσθέτοντας μια `meta-data` ενότητας, όπως στο ακόλουθο παράδειγμα:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Κατηγορίες

Όταν τροποποιείτε τις διατάξεις που παρέχεται, μπορείτε να τροποποιήσετε την εμφάνιση των όλες οι ειδοποιήσεις σας. Κατηγορίες σάς επιτρέπουν να ορίσετε διάφορες εμφανίσεις προορισμού (πιθανώς συμπεριφορές) για τις ειδοποιήσεις. Μια κατηγορία μπορεί να καθοριστεί κατά τη δημιουργία μιας εκστρατείας Reach. Λάβετε υπόψη ότι οι κατηγορίες σάς επιτρέπουν επίσης να προσαρμόσετε ανακοινώσεις και ψηφοφορίες, που περιγράφεται παρακάτω σε αυτό το έγγραφο.

Για να καταχωρήσετε ένα πρόγραμμα χειρισμού κατηγορίας για τις ειδοποιήσεις, πρέπει να προσθέσετε μια κλήση όταν η εφαρμογή έχει προετοιμαστεί.

> [AZURE.IMPORTANT] Διαβάστε την προειδοποίηση σχετικά με το χαρακτηριστικό android: διαδικασία \<android sdk-δέσμευση-διεργασίας\> του άρθρου πώς να ενσωματώσετε δέσμευση σε Android θέμα πριν να συνεχίσετε.

Το παρακάτω παράδειγμα προϋποθέτει μπορείτε αναγνωρίζεται την προηγούμενη προειδοποίηση και να χρησιμοποιήσετε μια δευτερεύουσα κλάση του `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Το `MyNotifier` αντικείμενο είναι η εφαρμογή από το πρόγραμμα χειρισμού κατηγορία ειδοποίησης. Είναι είτε μια εφαρμογή της το `EngagementNotifier` περιβάλλοντος εργασίας ή μιας κλάσης sub από την προεπιλεγμένη εφαρμογή: `EngagementDefaultNotifier`.

Σημειώστε ότι το ίδιο ειδοποίηση μπορεί να χειρίζεται πολλές κατηγορίες, μπορείτε να τους καταχωρήσετε ως εξής:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Για να αντικαταστήσετε την προεπιλεγμένη εφαρμογή κατηγορία, μπορείτε να καταχωρήσετε την υλοποίηση της όπως στο ακόλουθο παράδειγμα:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Η τρέχουσα κατηγορία που χρησιμοποιείται σε ένα πρόγραμμα χειρισμού μεταβιβάζεται ως παράμετρος στο περισσότερες μεθόδους που μπορείτε να παρακάμψετε στο `EngagementDefaultNotifier`.

Μεταβιβάζεται είτε ως μια `String` παραμέτρου ή έμμεσα στο ένα `EngagementReachContent` αντικείμενο το οποίο περιλαμβάνει ένα `getCategory()` μέθοδο.

Μπορείτε να αλλάξετε το μεγαλύτερο μέρος της διαδικασίας Δημιουργία ειδοποίησης, επαναπροσδιορισμός μεθόδους στην `EngagementDefaultNotifier`, για πιο σύνθετη προσαρμογή, μπορείτε να ρίξετε μια ματιά στην τεκμηρίωση της τεχνικής και τον κωδικό προέλευσης.

##### <a name="in-app-notifications"></a>Ειδοποιήσεις της εφαρμογής

Εάν θέλετε απλώς να χρησιμοποιήσετε εναλλακτικές διατάξεις για μια συγκεκριμένη κατηγορία, μπορείτε να το εφαρμόσετε όπως στο ακόλουθο παράδειγμα:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Παράδειγμα `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Όπως μπορείτε να δείτε, το αναγνωριστικό προβολή επικάλυψης είναι διαφορετική από την τυπική. Είναι σημαντικό ότι κάθε διάταξη χρησιμοποιεί ένα μοναδικό αναγνωριστικό για επικαλύψεων.

**Παράδειγμα `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Όπως μπορείτε να δείτε, το αναγνωριστικό Προβολή περιοχής ειδοποιήσεων είναι διαφορετική από την τυπική. Είναι σημαντικό ότι κάθε διάταξη χρησιμοποιεί ένα μοναδικό αναγνωριστικό για περιοχές ειδοποιήσεων.

Αυτό το απλό παράδειγμα της κατηγορίας κάνει εφαρμογής (ή της εφαρμογής) ειδοποιήσεις που εμφανίζονται στο επάνω μέρος της οθόνης. Δεν αλλάξουμε τυπική αναγνωριστικά χρησιμοποιείται στην ίδια περιοχή ειδοποιήσεων.

Εάν θέλετε να το αλλάξετε, πρέπει να καθορίσετε εκ νέου το `EngagementDefaultNotifier.prepareInAppArea` μέθοδο. Συνιστάται να δείτε την τεχνική τεκμηρίωση και τον πηγαίο κώδικα της `EngagementNotifier` και `EngagementDefaultNotifier` εάν θέλετε επιπέδου της προσαρμογής για προχωρημένους.

##### <a name="system-notifications"></a>Ειδοποιήσεις συστήματος

Παρατείνοντας `EngagementDefaultNotifier`, μπορείτε να παρακάμψετε `onNotificationPrepared` για να αλλάξετε την ειδοποίηση που είχε προετοιμαστεί από την προεπιλεγμένη εφαρμογή.

Για παράδειγμα:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

Αυτό το παράδειγμα δημιουργεί μια ειδοποίηση συστήματος για ένα περιεχόμενο που εμφανίζεται ως συμβάντος σε εξέλιξη όταν χρησιμοποιείται στην κατηγορία "σε εξέλιξη".

Εάν θέλετε να δημιουργήσετε το `Notification` αντικείμενο από την αρχή, μπορείτε να επιστρέψετε `false` με τη μέθοδο και κλήση `notify` τον εαυτό σας σε το `NotificationManager`. Σε αυτήν την περίπτωση είναι σημαντικό να διατηρείτε ένα `contentIntent`, έναν `deleteIntent` και το αναγνωριστικό ειδοποίησης που χρησιμοποιείται από `EngagementReachReceiver`.

Ακολουθεί ένα παράδειγμα σωστή όπως μια εφαρμογή:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Ειδοποίηση μόνο ανακοινώσεις

Η διαχείριση του, κάντε κλικ σε μια ειδοποίηση που μπορεί να προσαρμοστεί μόνο ανακοίνωση, παρακάμπτοντας `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` για να τροποποιήσετε το προετοιμασμένο `Intent`. Χρήση αυτής της μεθόδου σάς επιτρέπει να εύκολα με ακρίβεια οι σημαίες.

Για παράδειγμα, για να προσθέσετε το `SINGLE_TOP` σημαία:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Για τους χρήστες παλαιού τύπου δέσμευση, έχετε υπόψη ότι ειδοποιήσεις συστήματος χωρίς ενέργεια διεύθυνση URL τώρα ξεκινά η εφαρμογή εάν ήταν στο παρασκήνιο, ώστε να μπορεί να είναι η κλήση αυτής της μεθόδου με μια ανακοίνωση χωρίς η διεύθυνση URL ενέργειας. Λάβετε υπόψη που κατά την προσαρμογή πρόθεση.

Μπορείτε επίσης να εφαρμόσετε `EngagementNotifier.executeNotifAnnouncementAction` από την αρχή.

##### <a name="notification-life-cycle"></a>Κύκλος ζωής ειδοποίησης

Όταν χρησιμοποιείτε την προεπιλεγμένη κατηγορία, ορισμένες μεθόδους κύκλου ζωής ονομάζονται στην το `EngagementReachInteractiveContent` αντικειμένου για να αναφέρουν στατιστικά στοιχεία και να ενημερώσει την κατάσταση εκστρατείας:

-   Όταν η ειδοποίηση εμφανίζεται στην εφαρμογή ή τοποθέτηση στη γραμμή κατάστασης, η `displayNotification` μέθοδος καλείται (το οποίο αναφέρει στατιστικά στοιχεία) με `EngagementReachAgent` εάν `handleNotification` επιστρέφει `true`.
-   Εάν κλείσετε την ειδοποίηση, την `exitNotification` καλείται μέθοδος, αναφέρεται στατιστικής και τώρα είναι δυνατή η επεξεργασία επόμενη εκστρατείες.
-   Εάν κάνετε κλικ σε την ειδοποίηση, `actionNotification` είναι που ονομάζεται, αναφέρεται η στατιστική τιμή και ξεκινά η συσχετισμένη πρόθεση.

Εάν την εφαρμογή του, `EngagementNotifier` παρακάμπτει την προεπιλεγμένη συμπεριφορά, πρέπει να καλέσετε τις παρακάτω μεθόδους κύκλου ζωής μόνοι σας. Τα παρακάτω παραδείγματα εξηγούν ορισμένες περιπτώσεις όπου παρακαμφθούν την προεπιλεγμένη συμπεριφορά:

-   Δεν μπορείτε να επεκτείνετε `EngagementDefaultNotifier`, π.χ. υλοποιηθεί χειρισμού κατηγορία από την αρχή.
-   Για τις ειδοποιήσεις συστήματος, overrode το `onNotificationPrepared` και που τροποποιήσατε `contentIntent` ή `deleteIntent` στο το `Notification` αντικειμένου.
-   Για τις ειδοποιήσεις της εφαρμογής, overrode `prepareInAppArea`, φροντίστε να αντιστοιχίσετε τουλάχιστον `actionNotification` σε μία από τις U.I στοιχεία ελέγχου.

> [AZURE.NOTE] Εάν `handleNotification` παρουσιάζει μια εξαίρεση, το περιεχόμενο έχει διαγραφεί και `dropContent` ονομάζεται. Αυτό αναφέρεται σε στατιστικά στοιχεία και τώρα είναι δυνατή η επεξεργασία επόμενη εκστρατείες.

### <a name="announcements-and-polls"></a>Ανακοινώσεις και ψηφοφορίες

#### <a name="layouts"></a>Διατάξεις

Μπορείτε να τροποποιήσετε το `engagement_text_announcement.xml`, `engagement_web_announcement.xml` και `engagement_poll.xml` αρχείων για την προσαρμογή κειμένου ανακοινώσεις, ανακοινώσεις web και ψηφοφορίες.

Αυτά τα αρχεία, κοινή χρήση δύο κοινές διατάξεις για την περιοχή τίτλου και την περιοχή του κουμπιού. Η διάταξη για τον τίτλο του είναι `engagement_content_title.xml` και χρησιμοποιεί το ομώνυμη drawable αρχείο για το φόντο. Η διάταξη για τα κουμπιά ενεργειών και εξόδου είναι `engagement_button_bar.xml` και χρησιμοποιεί το ομώνυμη drawable αρχείο για το φόντο.

Σε μια ψηφοφορία, τη διάταξη ερώτηση και επιλογές τους είναι δυναμικά έχουν πολλές φορές χρησιμοποιώντας το `engagement_question.xml` αρχείο διάταξης για τις ερωτήσεις και το `engagement_choice.xml` αρχείο για τις επιλογές.

#### <a name="categories"></a>Κατηγορίες

##### <a name="alternate-layouts"></a>Εναλλακτικές διατάξεις

Όπως ειδοποιήσεις, κατηγορία της εκστρατείας μπορεί να χρησιμοποιηθεί για να έχουν εναλλακτικές διατάξεις για τις ανακοινώσεις και ψηφοφορίες.

Για παράδειγμα, για να δημιουργήσετε μια κατηγορία για μια ανακοίνωση για ένα κείμενο, μπορείτε να επεκτείνετε `EngagementTextAnnouncementActivity` και αναφέρετε το `AndroidManifest.xml` αρχείο:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Λάβετε υπόψη ότι η κατηγορία στο φίλτρο πρόθεση χρησιμοποιείται για να κάνετε τη διαφορά με τη δραστηριότητα ανακοίνωση προεπιλογή.

Το SDK επίτευξη χρησιμοποιεί το σύστημα πρόθεση να επιλύσετε τη σωστή δραστηριότητα για μια συγκεκριμένη κατηγορία και να εμπίπτει ξανά την κατηγορία προεπιλογή εάν αποτύχει η ανάλυση.

Στη συνέχεια, θα πρέπει να υλοποιήσετε `MyCustomTextAnnouncementActivity`, εάν θέλετε απλώς να αλλάξετε τη διάταξη (αλλά να διατηρήσετε τα ίδια προβολή αναγνωριστικά), έχετε απλώς για να ορίσετε την κλάση όπως στο ακόλουθο παράδειγμα:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Για να αντικαταστήσετε την προεπιλεγμένη κατηγορία ανακοινώσεων κειμένου, απλώς να την αντικαταστήσετε `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` από την εφαρμογή.

Ανακοινώσεις Web και ψηφοφορίες μπορεί να προσαρμοστεί με παρόμοιο τρόπο.

Για ανακοινώσεις web μπορείτε να επεκτείνετε `EngagementWebAnnouncementActivity` και δηλώνουν τη δραστηριότητά σας σε το `AndroidManifest.xml` όπως στο ακόλουθο παράδειγμα:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Για ψηφοφορίες μπορείτε να επεκτείνετε `EngagementPollActivity` και δηλώνουν σας στο το `AndroidManifest.xml` όπως στο ακόλουθο παράδειγμα:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Εφαρμογή από την αρχή

Μπορείτε να υλοποιήσετε κατηγορίες για τις δραστηριότητες ανακοίνωση (και ψηφοφορία) χωρίς ανάπτυξη ένα από τα `Engagement*Activity` κλάσεις που παρέχεται από την επίτευξη SDK. Αυτό είναι χρήσιμο για παράδειγμα εάν θέλετε να ορίσετε μια διάταξη που δεν χρησιμοποιεί το ίδιο προβολές ως τις τυπικές διατάξεις.

Όπως για Προσαρμογή ειδοποιήσεων για προχωρημένους, καλό είναι να εξετάσετε τον πηγαίο κώδικα της τυπική υλοποίηση.

Ακολουθούν ορισμένα πράγματα που πρέπει να λάβετε υπόψη: Reach θα ξεκινήσει τη δραστηριότητα με ένα συγκεκριμένο σκοπό σας (που αντιστοιχεί στο φίλτρο πρόθεση) συν μια επιπλέον παράμετρο που θα είναι το αναγνωριστικό του περιεχομένου.

Για να ανακτήσετε το αντικείμενο περιεχομένου που περιέχουν τα πεδία που καθορίσατε κατά τη δημιουργία της εκστρατείας στην τοποθεσία web, μπορείτε να κάνετε αυτό:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Στατιστικά στοιχεία, θα πρέπει να αναφέρετε το περιεχόμενο εμφανίζεται στο το `onResume` συμβάν:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Στη συνέχεια, μην ξεχάσετε να καλέσετε είτε `actionContent(this)` ή `exitContent(this)` στο αντικείμενο περιεχομένου πριν από τη δραστηριότητα τίθεται σε φόντο.

Εάν δεν μπορείτε να καλέσετε είτε `actionContent` ή `exitContent`, στατιστικά στοιχεία δεν θα αποσταλεί (δηλαδή χωρίς ανάλυση σε εκστρατείας) και το σημαντικότερο, το επόμενο εκστρατείες δεν θα ειδοποιηθούν μέχρι να γίνει επανεκκίνηση της διαδικασίας εφαρμογών.

Προσανατολισμός ή άλλες αλλαγές ρύθμισης παραμέτρων μπορεί να κάνει ο κωδικός περίπλοκη για να προσδιορίσετε εάν η δραστηριότητα τίθεται σε φόντου ή όχι, τυπική υλοποίηση εξασφαλίζει ότι το περιεχόμενο που είναι αναφέρθηκε ως εξέλθουν εάν ο χρήστης αφήσει τη δραστηριότητα (είτε πατώντας το συνδυασμό πλήκτρων `HOME` ή `BACK`), αλλά όχι εάν ο προσανατολισμός αλλάζει.

Ακολουθεί το ενδιαφέρον τμήμα της εφαρμογής:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Όπως μπορείτε να δείτε, αν έχετε καλέσει `actionContent(this)` , στη συνέχεια, ολοκληρώσει τη δραστηριότητα, `exitContent(this)` μπορεί να ονομάζεται με ασφάλεια χωρίς να χρειάζεται καμία επίδραση.

[Εδώ]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud ανταλλαγή μηνυμάτων]:http://developer.android.com/guide/google/gcm/index.html
[Συσκευή Amazon την ανταλλαγή μηνυμάτων]:https://developer.amazon.com/sdk/adm.html

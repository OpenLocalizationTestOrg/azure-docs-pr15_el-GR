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

#<a name="how-to-integrate-engagement-on-android"></a>Πώς μπορείτε να ενοποιήσετε δέσμευση σε Android

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Αυτή η διαδικασία περιγράφει ο πιο απλός τρόπος για να ενεργοποιήσετε την ανάλυση της δέσμευσης και παρακολούθηση συναρτήσεις στην εφαρμογή σας Android.

> [AZURE.IMPORTANT] Το ελάχιστο επίπεδο Android SDK API πρέπει να είναι 10 ή νεότερη έκδοση (Android 2.3.3 ή νεότερη έκδοση).

Τα ακόλουθα βήματα είναι αρκετά ενεργοποιεί την αναφορά των αρχείων καταγραφής χρειάζεται, για να υπολογίσετε όλα τα στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και Technicals. Η έκθεση της καταγραφής που είναι απαραίτητα για τον υπολογισμό άλλων στατιστικών στοιχείων όπως συμβάντα, σφάλματα και οι εργασίες πρέπει να γίνουν με μη αυτόματο τρόπο χρησιμοποιώντας το API δέσμευση (ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στη συσκευή Android](mobile-engagement-android-use-engagement-api.md) εφόσον αυτά τα στατιστικά στοιχεία έχουν εξαρτάται από την εφαρμογή.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Ενσωμάτωση του SDK δέσμευση και η υπηρεσία στο έργο σας Android

Κάντε λήψη του Android SDK από [εδώ](https://aka.ms/vq9mfn) λήψη `mobile-engagement-VERSION.jar` και να τα τοποθετήσετε στο το `libs` φάκελο του έργου σας Android (να δημιουργήσετε το φάκελο βιβλιοθηκών Εάν δεν υπάρχει ακόμα).

> [AZURE.IMPORTANT]
> Εάν δημιουργείτε το πακέτο εφαρμογών με ProGuard, πρέπει να διατηρήσετε ορισμένες κλάσεις. Μπορείτε να χρησιμοποιήσετε το παρακάτω τμήμα κώδικα ρύθμισης παραμέτρων:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Καθορίστε τη συμβολοσειρά σύνδεσης δέσμευση καλώντας την ακόλουθη μέθοδο στη δραστηριότητα εκκίνησης:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Εμφανίζεται η συμβολοσειρά σύνδεσης για την εφαρμογή σας στην πύλη Azure.

-   Εάν υπάρχει, προσθέστε τα ακόλουθα δικαιώματα Android (πριν από την `<application>` ετικέτα):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Προσθέστε την παρακάτω ενότητα (μεταξύ του `<application>` και `</application>` ετικέτες):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Αλλαγή `<Your application name>` από το όνομα της εφαρμογής σας.

> [AZURE.TIP] Το `android:label` χαρακτηριστικό σας επιτρέπει να επιλέξετε το όνομα της υπηρεσίας δέσμευση όπως θα εμφανίζεται σε οι τελικοί χρήστες στην οθόνη "Εκτελεί τις υπηρεσίες" από το τηλέφωνο. Συνιστάται να ορίσετε αυτήν την ιδιότητα για να `"<Your application name>Service"` (π.χ. `"AcmeFunGameService"`).

Καθορισμός του `android:process` χαρακτηριστικό εξασφαλίζει ότι θα εκτελείται η υπηρεσία δέσμευση στη δική του διεργασία (εκτελούνται δέσμευση στην ίδια διεργασία, όπως η εφαρμογή σας θα κάνει σας νήματος κύριες/περιβάλλον εργασίας Χρήστη ενδεχομένως λιγότερο αποκρίνεται).

> [AZURE.NOTE] Οποιονδήποτε κωδικό τοποθετήσετε σε `Application.onCreate()` και άλλα επιστροφές κλήσης εφαρμογή θα εκτελεστεί για την εφαρμογή διεργασίες, συμπεριλαμβανομένης της υπηρεσίας δέσμευση. Μπορεί να έχει ανεπιθύμητες επιπτώσεις (όπως εκχωρήσεις περιττών μνήμης και νήματα την πρόσληψη διεργασίας, διπλότυπων εκπομπής δέκτες ή υπηρεσίες).

Εάν παρακάμψετε `Application.onCreate()`, συνιστάται να προσθέσετε το τμήμα κώδικα παρακάτω στην αρχή του `Application.onCreate()` συνάρτηση:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Μπορείτε να κάνετε το ίδιο για `Application.onTerminate()`, `Application.onLowMemory()` και `Application.onConfigurationChanged(...)`.

Μπορείτε επίσης να επεκτείνετε `EngagementApplication` αντί για επέκταση `Application`: η επιστροφή κλήσης `Application.onCreate()` κάνει τη διαδικασία ελέγχου κλήσεων και `Application.onApplicationProcessCreate()` μόνο εάν η τρέχουσα διεργασία δεν είναι αυτό που φιλοξενεί την υπηρεσία δέσμευση, τους ίδιους κανόνες ισχύουν για τα άλλα επιστροφές κλήσης.

##<a name="basic-reporting"></a>Βασική αναφορά

### <a name="recommended-method-overload-your-activity-classes"></a>Προτεινόμενη μέθοδος: υπερφόρτωσης σας `Activity` κλάσεις

Για να ενεργοποιήσετε την αναφορά της όλα τα αρχεία καταγραφής που απαιτούνται από δέσμευση για τον υπολογισμό χρηστών, οι περίοδοι λειτουργίας, δραστηριότητες, παρουσιάζει σφάλμα και τεχνικές στατιστικά στοιχεία, θα πρέπει απλώς να κάνετε όλων σας `*Activity` δευτερεύουσες κλάσεις μεταβιβάζονται από το αντίστοιχο `Engagement*Activity` κλάσεις (π.χ. Εάν επεκτείνει τη δραστηριότητά σας παλαιού τύπου `ListActivity`, πραγματοποίηση να εκτείνεται `EngagementListActivity`).

**Χωρίς δέσμευση:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Με δέσμευση:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Κατά τη χρήση `EngagementListActivity` ή `EngagementExpandableListActivity`, βεβαιωθείτε ότι οποιαδήποτε κλήση `requestWindowFeature(...);` πραγματοποιείται πριν από την κλήση σε `super.onCreate(...);`, αλλιώς θα παρουσιαστεί σφάλμα.

Μπορείτε να βρείτε αυτές τις κατηγορίες στην το `src` φακέλου, και να τα αντιγράψετε στο έργο σας. Οι κλάσεις είναι επίσης στο το **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Εναλλακτική μέθοδο: κλήση `startActivity()` και `endActivity()` με μη αυτόματο τρόπο

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `Activity` κλάσεις, αντί για αυτό μπορείτε να ξεκινήσετε και να τερματίσετε σας δραστηριότητες, καλώντας `EngagementAgent`του μεθόδους απευθείας.

> [AZURE.IMPORTANT] Στο Android SDK ποτέ καλεί το `endActivity()` μέθοδο, ακόμα και όταν η εφαρμογή είναι κλειστό (σε Android, οι εφαρμογές είναι στην πραγματικότητα ποτέ κλειστές). Έτσι, είναι *ΙΔΙΑΊΤΕΡΑ* συνιστάται να καλέσετε το `startActivity()` μέθοδο σε το `onResume` επιστροφής κλήσης από *όλα* σας δραστηριότητες, και το `endActivity()` μέθοδο σε το `onPause()` επιστροφής κλήσης από *ΌΛΕΣ* τις δραστηριότητές σας. Αυτό είναι ο μόνος τρόπος για να βεβαιωθείτε ότι οι περίοδοι λειτουργίας δεν θα διαρροής. Εάν διαρροής μια περίοδο λειτουργίας, η υπηρεσία δέσμευση ποτέ θα αποσυνδεθείτε από υπολογιστή στο παρασκήνιο δέσμευση (από την υπηρεσία παραμένει συνδεδεμένη με την προϋπόθεση ότι η περίοδος λειτουργίας είναι σε εκκρεμότητα).

Ακολουθεί ένα παράδειγμα:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Αυτό το παράδειγμα παρόμοια με την το `EngagementActivity` τάξης και τις παραλλαγές, των οποίων πηγαίου κώδικα παρέχεται με το `src` φακέλου.

##<a name="test"></a>Έλεγχος

Τώρα Επιβεβαιώστε ότι το ενοποίησης, εκτελώντας την εφαρμογή για κινητές συσκευές σε προσομοίωσης ή τη συσκευή και επαλήθευση ότι το καταχωρεί μια περίοδο λειτουργίας στην καρτέλα οθόνη.

Οι επόμενες ενότητες είναι προαιρετικό.

##<a name="location-reporting"></a>Θέση αναφοράς

Εάν θέλετε θέσεις, για να να αναφερθεί, πρέπει να προσθέσετε μερικές γραμμές της ρύθμισης παραμέτρων (μεταξύ του `<application>` και `</application>` ετικέτες).

### <a name="lazy-area-location-reporting"></a>Περιοχή τεμπέλη θέση αναφοράς

Τεμπέλη περιοχή αναφοράς θέση σας επιτρέπει να αναφέρετε τη χώρα, περιοχή και τοποθεσία που έχει συσχετιστεί με συσκευές. Αυτός ο τύπος αναφοράς θέση χρησιμοποιεί μόνο θέσεις δικτύου (με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi). Στην περιοχή συσκευή αναφέρεται πολύ μία φορά σε κάθε περίοδο λειτουργίας. GPS το έχει χρησιμοποιήσει ποτέ και, επομένως, αυτός ο τύπος της θέσης αναφοράς έχει πολύ λίγα (όχι να πείτε χωρίς) επίδραση στο το μπαταριών.

Αναφερόμενου περιοχές που χρησιμοποιούνται για τον υπολογισμό γεωγραφικά στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, συμβάντα και σφάλματα. Μπορεί επίσης να χρησιμοποιηθεί ως κριτηρίου στο Reach εκστρατείες.

Για να ενεργοποιήσετε την αναφορά θέση τεμπέλη περιοχή, μπορείτε να το κάνετε χρησιμοποιώντας τις ρυθμίσεις που αναφέρθηκε προηγουμένως σε αυτήν τη διαδικασία:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα, εάν υπάρχει:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ή μπορείτε να συνεχίσετε να χρησιμοποιείτε ``ACCESS_FINE_LOCATION`` αν χρησιμοποιείτε ήδη στην εφαρμογή σας.

### <a name="real-time-location-reporting"></a>Πραγματικό χρόνο θέσης αναφοράς

Αναφορά θέση πραγματικό χρόνο σας επιτρέπει να αναφέρετε το γεωγραφικού πλάτους και μήκους που σχετίζονται με τις συσκευές. Από προεπιλογή, αυτός ο τύπος της θέσης αναφοράς χρησιμοποιεί μόνο θέσεις δικτύου (με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi) και η αναφορά είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (δηλαδή κατά τη διάρκεια μιας περιόδου λειτουργίας).

Θέσεις πραγματικό χρόνο είναι *δεν* χρησιμοποιείται για τον υπολογισμό στατιστικών στοιχείων. Μόνο ο σκοπός τους είναι να επιτρέψετε τη χρήση του πραγματικού χρόνου παν-περίφραξη \<Reach-ακροατηρίου-geofencing\> κριτηρίου στο Reach εκστρατείες.

Για να ενεργοποιήσετε την αναφορά θέση πραγματικό χρόνο, μπορείτε να το κάνετε χρησιμοποιώντας τις ρυθμίσεις που αναφέρθηκε προηγουμένως σε αυτήν τη διαδικασία:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα, εάν υπάρχει:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Ή μπορείτε να συνεχίσετε να χρησιμοποιείτε ``ACCESS_FINE_LOCATION`` αν χρησιμοποιείτε ήδη στην εφαρμογή σας.

#### <a name="gps-based-reporting"></a>GPS βάσει αναφοράς

Από προεπιλογή, αναφοράς θέση πραγματικού χρόνου χρησιμοποιεί μόνο θέσεις δικτύου με βάση. Για να ενεργοποιήσετε τη χρήση θέσεις GPS βάσει (που είναι πολύ πιο ακριβείς), χρησιμοποιήστε το αντικείμενο ρύθμισης παραμέτρων:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα, εάν υπάρχει:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Δημιουργία αναφορών φόντου

Από προεπιλογή, αναφοράς θέση πραγματικό χρόνο είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (δηλαδή κατά τη διάρκεια μιας περιόδου λειτουργίας). Για να ενεργοποιήσετε το αναφοράς επίσης στο παρασκήνιο, χρησιμοποιήστε το αντικείμενο ρύθμισης παραμέτρων:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Όταν η εφαρμογή θα εκτελείται στο παρασκήνιο, μόνο δικτύου βάσει θέσεις αναφερθούν, ακόμα και αν έχετε ενεργοποιήσει το GPS.

Η αναφορά θέση φόντου θα πρέπει να διακοπεί, αν ο χρήστης επανεκκίνηση τη συσκευή, μπορείτε να προσθέσετε αυτό για να κάνετε επανεκκίνηση αυτόματα κατά την εκκίνηση:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα, εάν υπάρχει:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M δικαιώματα

Ξεκινώντας με Android M, ορισμένα δικαιώματα πραγματοποιείται κατά το χρόνο εκτέλεσης και χρειάζεται την έγκριση του χρήστη.

Τα δικαιώματα χρόνου εκτέλεσης θα απενεργοποιηθούν από προεπιλογή για νέες εγκαταστάσεις εφαρμογής εάν στόχου Android API επίπεδο 23. Διαφορετικά αυτό θα είναι ενεργοποιημένη από προεπιλογή.

Ο χρήστης μπορούν να ενεργοποιούν/απενεργοποιούν αυτά τα δικαιώματα από το μενού Ρυθμίσεις συσκευής. Η απενεργοποίηση δικαιωμάτων από το μενού συστήματος τερματίζει διεργασίες παρασκηνίου της εφαρμογής, αυτή είναι μια συμπεριφορά του συστήματος και δεν έχει καμία επίδραση στη δυνατότητα λήψης push στο φόντο.

Στο πλαίσιο της δέσμευσης Mobile, τα δικαιώματα που απαιτούν έγκριση κατά το χρόνο εκτέλεσης είναι:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(μόνο όταν στόχευσης Android API επιπέδου 23 για αυτήν)

Το εξωτερικό χώρο αποθήκευσης χρησιμοποιείται μόνο για τη δυνατότητα γενική εικόνα Reach. Εάν βρείτε ζητήσετε από τους χρήστες να είναι ενοχλητικούς αυτό το δικαίωμα, μπορείτε να την καταργήσετε εάν την είχατε χρησιμοποιήσει μόνο για δέσμευση Mobile αλλά εις βάρος της απενεργοποιώντας τη δυνατότητα γενική εικόνα.

Για τις δυνατότητες θέση, θα πρέπει να ζητήσετε δικαιώματα χρήστη που χρησιμοποιεί ένα παράθυρο διαλόγου τυπική συστήματος. Εάν ο χρήστης εγκρίνει, πρέπει να πείτε ``EngagementAgent`` να ακολουθήσετε αυτήν την αλλαγή στο λογαριασμό σε πραγματικό χρόνο (αλλιώς η αλλαγή θα γίνει επεξεργασία την επόμενη φορά που ο χρήστης ενεργοποιεί την εφαρμογή).

Ακολουθεί ένα δείγμα κώδικα για χρήση σε μια δραστηριότητα της εφαρμογής σας για να ζητήσετε δικαιώματα και να προωθήσετε το αποτέλεσμα, εάν είναι θετικό ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Σύνθετες αναφοράς

Προαιρετικά, εάν θέλετε να αναφέρετε εφαρμογή συγκεκριμένα συμβάντα, σφάλματα και εργασιών, πρέπει να χρησιμοποιήσετε το API δέσμευση μέσω των μεθόδων το `EngagementAgent` τάξης. Αντικείμενο αυτής της κλάσης μπορεί να είναι retreived καλώντας την `EngagementAgent.getInstance()` στατική μέθοδο.

Το API δέσμευση σας επιτρέπει να χρησιμοποιήσετε όλες οι δυνατότητες για προχωρημένους του δέσμευση και είναι λεπτομερές του άρθρου πώς να χρησιμοποιήσετε το API δέσμευση σε Android (καθώς και όπως στην τεχνική τεκμηρίωση του το `EngagementAgent` κλάσης).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Ρύθμιση παραμέτρων για προχωρημένους (στο AndroidManifest.xml)

### <a name="wake-locks"></a>Ενεργοποίηση κλειδωμάτων

Εάν θέλετε να βεβαιωθείτε ότι τα στατιστικά στοιχεία θα στέλνονται σε πραγματικό χρόνο κατά τη χρήση μέσω Wi-Fi ή όταν η οθόνη είναι απενεργοποιημένη, προσθέστε τα εξής δικαιώματα Προαιρετικά:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Αναφορά σφαλμάτων

Εάν θέλετε να απενεργοποιήσετε σφάλμα αναφορών, προσθέστε αυτό (μεταξύ του `<application>` και `</application>` ετικέτες):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Όριο καταιγισμού

Από προεπιλογή, οι αναφορές εξυπηρέτησης δέσμευση καταγράφει σε πραγματικό χρόνο. Εάν η εφαρμογή σας αναφέρει αρχεία καταγραφής πολύ συχνά, είναι καλύτερα να buffer τα αρχεία καταγραφής και την αναφορά τους όλα ταυτόχρονα σε μια κανονική ώρα base (αυτό ονομάζεται "λειτουργία καταιγισμού"). Για να κάνετε αυτό, προσθέστε αυτό (μεταξύ του `<application>` και `</application>` ετικέτες):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Η κατάσταση λειτουργίας καταιγισμού ελαφρώς αυξήσετε τη διάρκεια ζωής μπαταριών αλλά έχει επιπτώσεις στην οθόνη δέσμευση: όλες οι περίοδοι λειτουργίας και εργασιών διάρκεια στρογγυλοποιείται σε το όριο καταιγισμού (έτσι, περιόδους λειτουργίας και εργασίες που είναι μικρότερες από το όριο καταιγισμού ενδέχεται να μην είναι ορατά). Συνιστάται να χρησιμοποιήσετε ένα όριο καταιγισμού δεν είναι πλέον από 30000 (30s).

### <a name="session-timeout"></a>Χρονικό όριο περιόδου λειτουργίας

Από προεπιλογή, η περίοδος λειτουργίας είναι δεκάδες λήξη μετά το τέλος του τελευταίου δραστηριότητας (το οποίο συνήθως συμβαίνει πατώντας το πλήκτρο για οικιακή χρήση ή πίσω, με τη ρύθμιση του τηλεφώνου σε αδράνεια ή κατά τη μετάβαση σε άλλη εφαρμογή). Πρόκειται για να αποφύγετε τη διαίρεση περιόδου λειτουργίας κάθε φορά που το χρήστη Έξοδος και επιστροφή στην εφαρμογή πολύ γρήγορα (το οποίο μπορεί να συμβεί όταν εσείς σηκώστε μια εικόνα, επιλέξτε μια ειδοποίηση, κ.λπ.). Ενδέχεται να θέλετε να τροποποιήσετε αυτήν την παράμετρο. Για να κάνετε αυτό, προσθέστε αυτό (μεταξύ του `<application>` και `</application>` ετικέτες):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Απενεργοποίηση της αναφοράς αρχείου καταγραφής

### <a name="using-a-method-call"></a>Χρησιμοποιώντας τη μέθοδο κλήσης

Εάν θέλετε δέσμευση να διακόψετε την αποστολή αρχείων καταγραφής, μπορείτε να καλέσετε:

            EngagementAgent.getInstance(context).setEnabled(false);

Αυτή η κλήση είναι μόνιμη: χρησιμοποιεί ένα αρχείο κοινόχρηστο προτιμήσεων.

Εάν δέσμευση είναι ενεργό, όταν καλείτε αυτήν τη συνάρτηση, ενδέχεται να χρειαστούν 1 λεπτό για διακοπή της υπηρεσίας. Ωστόσο, αυτό δεν θα εκκίνηση της υπηρεσίας καθόλου την επόμενη φορά που θα ξεκινήσετε την εφαρμογή.

Μπορείτε να ενεργοποιήσετε ξανά αναφοράς καλώντας την ίδια λειτουργία με το αρχείο καταγραφής `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Ενοποίηση του δικού σας`PreferenceActivity`

Αντί για κλήση αυτής της συνάρτησης, μπορείτε επίσης να ενσωματώσετε αυτήν τη ρύθμιση απευθείας στο του υπάρχοντος `PreferenceActivity`.

Μπορείτε να ρυθμίσετε δέσμευση για να χρησιμοποιήσετε το αρχείο σας προτιμήσεις (με τη λειτουργία επιθυμητές) με το `AndroidManifest.xml` αρχείων με `application meta-data`:

-   Το `engagement:agent:settings:name` κλειδί χρησιμοποιείται για να καθορίσετε το όνομα του αρχείου κοινόχρηστο προτιμήσεις.
-   Το `engagement:agent:settings:mode` κλειδί χρησιμοποιείται για να ορίσετε τη λειτουργία του αρχείου κοινόχρηστο προτιμήσεις, θα πρέπει να χρησιμοποιήσετε την ίδια λειτουργία ως στο σας `PreferenceActivity`. Η λειτουργία πρέπει να διαβιβαστεί ως αριθμό: Εάν χρησιμοποιείτε ένα συνδυασμό σταθερό σημαίες στον κώδικά σας, ελέγξτε τη συνολική τιμή.

Χρήση δέσμευση πάντα το `engagement:key` δυαδική κλειδί μέσα στο αρχείο προτιμήσεων για τη Διαχείριση αυτήν τη ρύθμιση.

Το παρακάτω παράδειγμα `AndroidManifest.xml` εμφανίζει τις προεπιλεγμένες τιμές:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Στη συνέχεια, μπορείτε να προσθέσετε μια `CheckBoxPreference` στη διάταξή σας προτίμηση όπως το εξής:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094

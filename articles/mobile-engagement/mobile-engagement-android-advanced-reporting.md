<properties
    pageTitle="Επιλογές για προχωρημένους αναφοράς για Azure Mobile δέσμευση Android SDK"
    description="Περιγράφει πώς μπορείτε να κάνετε σύνθετες αναφοράς για να καταγράψετε ανάλυσης για Azure Mobile δέσμευση Android SDK"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Σύνθετες αναφοράς με δέσμευση σε Android

> [AZURE.SELECTOR]
- [Καθολική των Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Αυτό το θέμα περιγράφει πρόσθετα σενάρια αναφοράς στην εφαρμογή σας Android. Μπορείτε να εφαρμόσετε αυτές τις επιλογές για την εφαρμογή που έχουν δημιουργηθεί με το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα](mobile-engagement-android-get-started.md) .

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Το πρόγραμμα εκμάθησης, θα ολοκληρώσει ήταν σκοπίμως άμεσο και απλή, αλλά μπορείτε να επιλέξετε επιλογές για προχωρημένους.

## <a name="modifying-your-activity-classes"></a>Τροποποίηση του `Activity` κλάσεις

Στην [ξεκινήσατε το πρόγραμμα εκμάθησης](mobile-engagement-android-get-started.md), όλα έπρεπε να κάνετε ήταν για να κάνετε το `*Activity` υποκατηγορίες μεταβιβάζονται από το αντίστοιχο `Engagement*Activity` τάξεις τους. Για παράδειγμα, εάν επεκταθεί παλαιού τύπου δραστηριότητά σας `ListActivity`, μπορείτε να την κάνετε επέκταση `EngagementListActivity`.

> [AZURE.IMPORTANT] Κατά τη χρήση `EngagementListActivity` ή `EngagementExpandableListActivity`, βεβαιωθείτε ότι οποιαδήποτε κλήση `requestWindowFeature(...);` πραγματοποιείται πριν από την κλήση σε `super.onCreate(...);`, διαφορετικά παρουσιάζεται σφάλμα.

Μπορείτε να βρείτε αυτές τις κατηγορίες στην το `src` φακέλου, και να τα αντιγράψετε στο έργο σας. Οι κλάσεις είναι επίσης στο το **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Εναλλακτική μέθοδο: κλήση `startActivity()` και `endActivity()` με μη αυτόματο τρόπο

Εάν δεν μπορείτε ή δεν θέλετε να υπερφόρτωσης σας `Activity` κλάσεις, αντί για αυτό μπορείτε να ξεκινήσετε και να τερματίσετε σας δραστηριότητες, καλώντας την `EngagementAgent`του μεθόδους απευθείας.

> [AZURE.IMPORTANT] Στο Android SDK ποτέ καλεί το `endActivity()` μέθοδο, ακόμα και όταν η εφαρμογή είναι κλειστό (σε Android, οι εφαρμογές είναι ποτέ κλειστές). Έτσι, είναι *ΙΔΙΑΊΤΕΡΑ* συνιστάται να καλέσετε το `startActivity()` μέθοδο σε το `onResume` επιστροφής κλήσης από *όλα* σας δραστηριότητες, και το `endActivity()` μέθοδο σε το `onPause()` επιστροφής κλήσης από *ΌΛΕΣ* τις δραστηριότητές σας. Αυτό είναι ο μόνος τρόπος για να βεβαιωθείτε ότι οι περίοδοι λειτουργίας δεν είναι διαρροής. Εάν διαρροής μια περίοδο λειτουργίας, η υπηρεσία δέσμευση ποτέ πραγματοποιεί αποσύνδεση από υπολογιστή στο παρασκήνιο δέσμευση (από την υπηρεσία παραμένει συνδεδεμένη με την προϋπόθεση ότι η περίοδος λειτουργίας είναι σε εκκρεμότητα).

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

Αυτό το παράδειγμα είναι παρόμοια με την το `EngagementActivity` τάξης και τις παραλλαγές, των οποίων πηγαίου κώδικα παρέχεται με το `src` φακέλου.

## <a name="using-applicationoncreate"></a>Χρήση Application.onCreate()

Οποιονδήποτε κωδικό τοποθετήσετε σε `Application.onCreate()` και σε άλλη εφαρμογή εκτελείται επιστροφές κλήσης για την εφαρμογή διεργασίες, συμπεριλαμβανομένης της υπηρεσίας δέσμευση. Μπορεί να έχει ανεπιθύμητες επιπτώσεις, όπως εκχωρήσεις περιττών μνήμης και νήματα σε διαδικασία την πρόσληψη, ή διπλότυπων εκπομπής δέκτες ή υπηρεσίες.

Εάν παρακάμψετε `Application.onCreate()`, συνιστάται να προσθέσετε το τμήμα κώδικα παρακάτω στην αρχή της σας `Application.onCreate()` συνάρτηση:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Μπορείτε να κάνετε το ίδιο για `Application.onTerminate()`, `Application.onLowMemory()`, και `Application.onConfigurationChanged(...)`.

Μπορείτε επίσης να επεκτείνετε `EngagementApplication` αντί για επέκταση `Application`: η επιστροφή κλήσης `Application.onCreate()` κάνει τη διαδικασία ελέγχου κλήσεων και `Application.onApplicationProcessCreate()` μόνο εάν η τρέχουσα διεργασία δεν είναι αυτό που φιλοξενεί την υπηρεσία δέσμευση, τους ίδιους κανόνες ισχύουν για τα άλλα επιστροφές κλήσης.

## <a name="tags-in-the-androidmanifestxml-file"></a>Οι ετικέτες στο αρχείο AndroidManifest.xml

Στην υπηρεσία ετικέτα στο αρχείο AndroidManifest.xml, το `android:label` χαρακτηριστικό σας επιτρέπει να επιλέξετε το όνομα της υπηρεσίας δέσμευση όπως εμφανίζεται στους τελικούς χρήστες στην οθόνη "Εκτελεί τις υπηρεσίες" από το τηλέφωνο. Συνιστάται η ρύθμιση αυτού του χαρακτηριστικού για να `"<Your application name>Service"` (για παράδειγμα, `"AcmeFunGameService"`).

Καθορισμός του `android:process` χαρακτηριστικό εξασφαλίζει ότι εκτελείται η υπηρεσία δέσμευση στη δική του διεργασία (εκτελείται δέσμευση σε την ίδια διαδικασία, καθώς η εφαρμογή σας κάνει σας νήματος κύριες/περιβάλλον εργασίας Χρήστη ενδεχομένως λιγότερο αποκρίνεται).

## <a name="building-with-proguard"></a>Δημιουργία με ProGuard

Εάν δημιουργείτε το πακέτο εφαρμογών με ProGuard, πρέπει να διατηρήσετε ορισμένες κλάσεις. Μπορείτε να χρησιμοποιήσετε το παρακάτω τμήμα κώδικα ρύθμισης παραμέτρων:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }

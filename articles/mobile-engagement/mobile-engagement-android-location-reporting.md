<properties
    pageTitle="Θέση αναφοράς για Android SDK Azure δέσμευση κινητές συσκευές"
    description="Περιγράφει πώς μπορείτε να ρυθμίσετε τις παραμέτρους θέσης αναφοράς για το Azure Mobile δέσμευση Android SDK"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Θέση αναφοράς για Android SDK Azure δέσμευση κινητές συσκευές

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Αυτό το θέμα περιγράφει πώς μπορείτε να κάνετε θέση αναφοράς για την εφαρμογή σας Android.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Θέση αναφοράς

Εάν θέλετε θέσεις, για να να αναφερθεί, πρέπει να προσθέσετε μερικές γραμμές της ρύθμισης παραμέτρων (μεταξύ του `<application>` και `</application>` ετικέτες).

### <a name="lazy-area-location-reporting"></a>Περιοχή τεμπέλη θέση αναφοράς

Αναφορά θέση τεμπέλη περιοχή δίνει τη δυνατότητα αναφοράς τη χώρα, περιοχή και τοποθεσία που σχετίζονται με τις συσκευές. Αυτός ο τύπος αναφοράς θέση χρησιμοποιεί μόνο θέσεις δικτύου (με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi). Στην περιοχή συσκευή αναφέρεται πολύ μία φορά σε κάθε περίοδο λειτουργίας. GPS το έχει χρησιμοποιήσει ποτέ και, επομένως, αυτός ο τύπος της θέσης αναφοράς έχει χαμηλή επίδραση στις το μπαταριών.

Αναφερόμενου περιοχές που χρησιμοποιούνται για τον υπολογισμό γεωγραφικά στατιστικά στοιχεία σχετικά με τους χρήστες, οι περίοδοι λειτουργίας, συμβάντα και σφάλματα. Μπορεί επίσης να χρησιμοποιηθεί ως κριτηρίου στο Reach εκστρατείες.

Ενεργοποίηση θέση τεμπέλη περιοχής αναφοράς με χρήση της ρύθμισης παραμέτρων προηγουμένως που αναφέρονται σε αυτήν τη διαδικασία:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Πρέπει επίσης να καθορίσετε μια θέση δικαιωμάτων. Χρησιμοποιεί αυτόν τον κωδικό ``COARSE`` δικαιωμάτων:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Εάν η εφαρμογή σας απαιτεί, μπορείτε να χρησιμοποιήσετε ``ACCESS_FINE_LOCATION`` αντί για αυτό.

### <a name="real-time-location-reporting"></a>Δημιουργία αναφορών σε πραγματικό χρόνο θέση

Δημιουργία αναφορών σε πραγματικό χρόνο θέση δίνει τη δυνατότητα αναφοράς του γεωγραφικού πλάτους και μήκους που σχετίζονται με τις συσκευές. Από προεπιλογή, αυτός ο τύπος αναφοράς θέση χρησιμοποιεί μόνο τις θέσεις δικτύου, με βάση το Αναγνωριστικό κελιού ή μέσω Wi-Fi. Η αναφορά είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (για παράδειγμα, κατά τη διάρκεια μιας περιόδου λειτουργίας).

Θέσεις σε πραγματικό χρόνο είναι *δεν* χρησιμοποιείται για τον υπολογισμό στατιστικών στοιχείων. Μόνο ο σκοπός τους είναι να επιτρέψετε τη χρήση του σε πραγματικό χρόνο παν χωριστή παρουσίαση και διαχείριση \<Reach-ακροατηρίου-geofencing\> κριτηρίου στο Reach εκστρατείες.

Για να ενεργοποιήσετε σε πραγματικό χρόνο θέση αναφοράς, προσθέστε μια γραμμή κώδικα όπου μπορείτε να ορίσετε τη συμβολοσειρά σύνδεσης δέσμευση στη δραστηριότητα εκκίνηση. Το αποτέλεσμα μοιάζει με τα εξής:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS βάσει αναφοράς

Από προεπιλογή, δημιουργία αναφορών σε πραγματικό χρόνο θέση χρησιμοποιεί μόνο θέσεις δικτύου. Για να ενεργοποιήσετε τη χρήση βάσει GPS θέσεις, που είναι πολύ πιο ακριβείς, χρησιμοποιήστε το αντικείμενο ρύθμισης παραμέτρων:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα, εάν υπάρχει:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Δημιουργία αναφορών φόντου

Από προεπιλογή, δημιουργία αναφορών σε πραγματικό χρόνο θέση είναι ενεργή μόνο όταν η εφαρμογή θα εκτελείται σε πρώτο πλάνο (για παράδειγμα, κατά τη διάρκεια μιας περιόδου λειτουργίας). Για να ενεργοποιήσετε το αναφοράς επίσης στο παρασκήνιο, χρησιμοποιήστε αυτό το αντικείμενο ρύθμισης παραμέτρων:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Όταν η εφαρμογή θα εκτελείται στο παρασκήνιο, αναφερθούν μόνο θέσεις που βασίζονται σε δίκτυο, ακόμα και αν έχετε ενεργοποιήσει το GPS.

Εάν ο χρήστης επανεκκίνηση τη συσκευή, έχει διακοπεί η αναφορά θέση φόντου. Για να επανεκκινήσετε αυτόματα κατά την εκκίνηση, προσθέστε αυτόν τον κωδικό.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Πρέπει επίσης να προσθέσετε τα ακόλουθα δικαιώματα εάν λείπουν:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M δικαιώματα

Ξεκινώντας με Android M, ορισμένα δικαιώματα πραγματοποιείται κατά το χρόνο εκτέλεσης και χρειάζεται έγκριση χρήστη.

Εάν το που στοχεύουν σε επίπεδο Android API 23, τα δικαιώματα χρόνου εκτέλεσης είναι απενεργοποιημένες από προεπιλογή για νέες εγκαταστάσεις εφαρμογής. Διαφορετικά αυτές είναι ενεργοποιημένες από προεπιλογή.

Μπορείτε να ενεργοποιούν/απενεργοποιούν αυτά τα δικαιώματα από το μενού Ρυθμίσεις συσκευής. Η απενεργοποίηση δικαιωμάτων από το μενού συστήματος τερματίζει την διεργασίες παρασκηνίου της εφαρμογής, που είναι μια συμπεριφορά συστήματος, και δεν έχει καμία επίδραση στη δυνατότητα λήψης push στο φόντο.

Στο πλαίσιο θέση δέσμευση Mobile αναφοράς, τα δικαιώματα που απαιτούν έγκριση κατά το χρόνο εκτέλεσης είναι:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Ζητήστε δικαιώματα από το χρήστη που χρησιμοποιεί ένα παράθυρο διαλόγου τυπική συστήματος. Εάν ο χρήστης εγκρίνει, πείτε ``EngagementAgent`` να ακολουθήσετε αυτήν την αλλαγή στο λογαριασμό στο σε πραγματικό χρόνο. Διαφορετικά η αλλαγή επεξεργάζεται την επόμενη φορά που ο χρήστης ενεργοποιεί την εφαρμογή.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

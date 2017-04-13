<properties
    pageTitle="Ρύθμιση παραμέτρων για προχωρημένους για Azure Mobile δέσμευση Android SDK"
    description="Περιγράφει τις επιλογές ρύθμιση παραμέτρων για προχωρημένους, όπως το Android δήλωσης με Azure Mobile δέσμευση Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Ρύθμιση παραμέτρων για προχωρημένους για Azure Mobile δέσμευση Android SDK

> [AZURE.SELECTOR]
- [Καθολική των Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Αυτή η διαδικασία περιγράφει πώς μπορείτε να ρυθμίσετε τις παραμέτρους διάφορες επιλογές ρύθμισης παραμέτρων για τις εφαρμογές του Azure Mobile δέσμευση Android.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Απαιτήσεις για δικαιώματα
Ορισμένες επιλογές απαιτούν συγκεκριμένα δικαιώματα, τα οποία παρατίθενται εδώ για αναφορά και, στη γραμμή στο τη συγκεκριμένη δυνατότητα. Προσθέστε αυτά τα δικαιώματα για το AndroidManifest.xml του έργου σας αμέσως πριν ή μετά την `<application>` ετικέτα.

Ο κώδικας δικαιωμάτων πρέπει να μοιάζει με τα εξής, όπου μπορείτε να συμπληρώσετε τα κατάλληλα δικαιώματα από τον πίνακα που ακολουθεί.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Δικαιωμάτων | Όταν χρησιμοποιείται |
| ---------- | --------- |
| ΣΤΟ INTERNET | Απαιτείται. Για βασικές αναφορές |
| ACCESS_NETWORK_STATE | Απαιτείται. Για βασικές αναφορές |
| RECEIVE_BOOT_COMPLETED | Απαιτείται. Για να εμφανίζονται στο κέντρο ειδοποιήσεις μετά την επανεκκίνηση της συσκευής |
| WAKE_LOCK | Συνιστάται η. Ενεργοποιεί τη συλλογή δεδομένων όταν χρησιμοποιείτε μέσω Wi-Fi ή όταν η οθόνη είναι απενεργοποιημένη |
| ΔΌΝΗΣΗ | Προαιρετικό. Σας δίνει τη δυνατότητα δόνησης όταν γίνεται λήψη των ειδοποιήσεων |
| DOWNLOAD_WITHOUT_NOTIFICATION | Προαιρετικό. Επιτρέπει την ειδοποίηση Android γενικής εικόνας |
| WRITE_EXTERNAL_STORAGE | Προαιρετικό. Επιτρέπει την ειδοποίηση Android γενικής εικόνας |
| ACCESS_COARSE_LOCATION | Προαιρετικό. Επιτρέπει σε πραγματικό χρόνο θέσης αναφοράς |
| ACCESS_FINE_LOCATION | Προαιρετικό. Ενεργοποιεί τη θέση που βασίζεται σε GPS αναφοράς |

Ξεκινώντας με Android M, [ορισμένα δικαιώματα πραγματοποιείται κατά το χρόνο εκτέλεσης](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Εάν χρησιμοποιείτε ήδη ``ACCESS_FINE_LOCATION``, στη συνέχεια, δεν χρειάζεται να χρησιμοποιήσετε τα ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Επιλογές Android δήλωσης ρύθμισης παραμέτρων

### <a name="crash-report"></a>Αναφορά σφαλμάτων

Για να απενεργοποιήσετε σφάλμα αναφορών, προσθέστε αυτόν τον κωδικό μεταξύ του `<application>` και `</application>` ετικέτες:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Όριο καταιγισμού

Από προεπιλογή, οι αναφορές εξυπηρέτησης δέσμευση καταγράφει σε πραγματικό χρόνο. Εάν τα αρχεία καταγραφής εφαρμογών αναφοράς ποικίλλουν συχνά, είναι καλύτερα να buffer τα αρχεία καταγραφής και να αναφέρετε όλα ταυτόχρονα σε τακτική βάση φορά (που ονομάζεται "κατάσταση λειτουργίας καταιγισμού"). Για να το κάνετε αυτό, προσθέστε αυτόν τον κωδικό μεταξύ του `<application>` και `</application>` ετικέτες:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Κατάσταση λειτουργίας καταιγισμού ελαφρώς αυξάνεται ο χρόνος ζωής μπαταριών αλλά έχει επιπτώσεις στην οθόνη δέσμευση: όλες οι περίοδοι λειτουργίας και εργασιών διάρκεια στρογγυλοποιούνται με το όριο καταιγισμού (έτσι, περιόδους λειτουργίας και εργασίες που είναι μικρότερες από το όριο καταιγισμού ενδέχεται να μην είναι ορατά). Το όριο καταιγισμού πρέπει να έχει περισσότερους από 30000 (30s).

### <a name="session-timeout"></a>Χρονικό όριο περιόδου λειτουργίας

 Μπορείτε να τερματίσετε μια δραστηριότητας, πατώντας το πλήκτρο **για οικιακή χρήση** ή **προς τα πίσω** , ρυθμίζοντας το τηλέφωνο που είναι σε αδράνεια ή από μεταπήδηση σε άλλη εφαρμογή. Από προεπιλογή, μια περίοδο λειτουργίας τερματίζεται δέκα δευτερόλεπτα μετά το τέλος του τελευταίου δραστηριότητας. Αυτό, αποφεύγεται η περίοδος λειτουργίας διαίρεσης κάθε φορά ο χρήστης πραγματοποιήσει έξοδο και επιστρέφει στην εφαρμογή γρήγορα, που μπορεί να συμβεί όταν ο χρήστης λαμβάνει μια εικόνα, ελέγχει μια ειδοποίηση, κ.λπ. Ενδέχεται να θέλετε να τροποποιήσετε αυτήν την παράμετρο. Για να το κάνετε αυτό, προσθέστε αυτόν τον κωδικό μεταξύ του `<application>` και `</application>` ετικέτες:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Απενεργοποίηση της αναφοράς αρχείου καταγραφής

### <a name="using-a-method-call"></a>Χρησιμοποιώντας τη μέθοδο κλήσης

Εάν θέλετε δέσμευση να διακόψετε την αποστολή αρχείων καταγραφής, μπορείτε να καλέσετε:

    EngagementAgent.getInstance(context).setEnabled(false);

Αυτή η κλήση είναι μόνιμη: χρησιμοποιεί ένα αρχείο κοινόχρηστο προτιμήσεων.

Εάν δέσμευση είναι ενεργό, όταν καλείτε αυτήν τη συνάρτηση, ενδέχεται να χρειαστούν ένα λεπτό για διακοπή της υπηρεσίας. Ωστόσο, αυτό δεν θα εκκίνηση της υπηρεσίας καθόλου την επόμενη φορά που θα ξεκινήσετε την εφαρμογή.

Μπορείτε να ενεργοποιήσετε ξανά αναφοράς καλώντας την ίδια λειτουργία με το αρχείο καταγραφής `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Ενοποίηση του δικού σας`PreferenceActivity`

Αντί για κλήση αυτής της συνάρτησης, μπορείτε επίσης να ενσωματώσετε αυτήν τη ρύθμιση απευθείας στο του υπάρχοντος `PreferenceActivity`.

Μπορείτε να ρυθμίσετε δέσμευση για να χρησιμοποιήσετε το αρχείο σας προτιμήσεις (με τη λειτουργία επιθυμητές) με το `AndroidManifest.xml` αρχείων με `application meta-data`:

-   Το `engagement:agent:settings:name` κλειδί χρησιμοποιείται για να καθορίσετε το όνομα του αρχείου κοινόχρηστο προτιμήσεις.
-   Το `engagement:agent:settings:mode` κλειδί χρησιμοποιείται για να ορίσετε τη λειτουργία του αρχείου κοινόχρηστο προτιμήσεις. Χρησιμοποιήστε την ίδια λειτουργία ως στο σας `PreferenceActivity`. Η λειτουργία πρέπει να διαβιβαστεί ως αριθμό: Εάν χρησιμοποιείτε ένα συνδυασμό σταθερό σημαίες στον κώδικά σας, ελέγξτε τη συνολική τιμή.

Δέσμευση πάντα χρησιμοποιεί το `engagement:key` δυαδική κλειδί μέσα στο αρχείο προτιμήσεων για τη Διαχείριση αυτήν τη ρύθμιση.

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

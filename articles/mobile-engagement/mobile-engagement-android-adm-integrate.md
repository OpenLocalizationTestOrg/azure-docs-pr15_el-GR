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


#<a name="how-to-integrate-adm-with-engagement"></a>Με τον τρόπο ενσωμάτωσης ADM δέσμευση

> [AZURE.IMPORTANT] Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφονται στο το πώς μπορείτε να ενοποιήσετε δέσμευση σε Android εγγράφου πριν να ακολουθήσετε αυτόν τον οδηγό.
>
> Αυτό το έγγραφο είναι χρήσιμο μόνο εάν έχετε ήδη ενσωματωμένο τη λειτουργική μονάδα Reach και σχεδιασμός για να προωθήσετε Amazon συσκευές. Για να ενσωματώσετε Reach εκστρατείες στην εφαρμογή, διαβάστε πρώτα πώς μπορείτε να ενοποιήσετε δέσμευση φτάσετε στο Android.

##<a name="introduction"></a>Εισαγωγή

Ενοποίηση ADM επιτρέπει την εφαρμογή σας για να είναι δυνατή η προώθηση όταν στόχευσης συσκευές Amazon Android.

Περιέχουν ωφέλιμα φορτία ADM προωθηθεί στο SDK πάντα το `azme` κλειδιού στο αντικείμενο δεδομένων. Επομένως, εάν χρησιμοποιείτε ADM για κάποιον άλλο σκοπό στην εφαρμογή σας, μπορείτε να φιλτράρετε με βάση αυτό το πλήκτρο ωθεί.

> [AZURE.IMPORTANT] Μόνο Amazon Kindle συσκευές με Android 4.0.3 ή πιο πάνω υποστηρίζονται από το Amazon ανταλλαγή μηνυμάτων συσκευή. Ωστόσο, μπορείτε να ενοποιήσετε αυτόν τον κωδικό με ασφάλεια σε άλλες συσκευές.

##<a name="sign-up-to-adm"></a>Εγγραφείτε για να ADM

Εάν δεν είναι ήδη, πρέπει να ενεργοποιήσετε ADM στο λογαριασμό σας στο Amazon.

Η διαδικασία περιγράφεται στο: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Αφού ολοκληρώσετε τη διαδικασία, λάβετε:

-   Διακριτικό διαπιστευτήρια (ένα Αναγνωριστικό υπολογιστή-πελάτη και έναν μυστικό προγράμματος-πελάτη) για δέσμευση για να μπορέσετε να push τις συσκευές σας.
-   Ένα κλειδί API που πρέπει να είναι ενσωματωμένο στην εφαρμογή σας.

##<a name="sdk-integration"></a>Ενοποίηση του SDK

### <a name="managing-device-registrations"></a>Διαχείριση καταχωρήσεων συσκευή

Κάθε συσκευή πρέπει να στείλετε μια εντολή εγγραφής με τους διακομιστές ADM, διαφορετικά δεν είναι δυνατό να φτάσει.

Εάν χρησιμοποιείτε ήδη στη [βιβλιοθήκη προγράμματος-πελάτη ADM]και έχετε ήδη [ενσωματωμένη ADM] μπορείτε να μεταβείτε απευθείας στο android-sdk-adm-λήψη.

Αν δεν έχετε συνδέσει ακόμη ADM, δέσμευση περιλαμβάνει ένα απλούστερο τρόπος για να την ενεργοποιήσετε στην εφαρμογή σας:

Επεξεργασία του `AndroidManifest.xml` αρχείο:

-   Προσθέστε το πεδίο Amazon ονομάτων, πρέπει να ξεκινήσετε το αρχείο ως εξής:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Εντός του `<application/>` ετικέτα, προσθέστε αυτήν την ενότητα:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Μετά την προσθήκη της ετικέτας amazon, ενδέχεται να έχετε ένα σφάλμα Δόμηση αν το έργο δημιουργία προορισμού είναι μικρότερη από Android 2.1. Θα πρέπει να χρησιμοποιήσετε ένα προορισμού Δόμηση **Android 2.1 +** (μην ανησυχείτε, μπορείτε να εξακολουθείτε να έχετε μια `minSdkVersion` οριστεί στην τιμή 4).
-   Ενοποιήστε το κλειδί API ADM ως ενός περιουσιακού στοιχείου από [αυτήν τη διαδικασία]που ακολουθεί.

Στη συνέχεια, ακολουθήστε τις οδηγίες της στις επόμενες ενότητες.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Επικοινωνία αναγνωριστικό δήλωσης για την υπηρεσία δέσμευση Push και να λαμβάνετε ειδοποιήσεις

Προκειμένου να επικοινωνούν το αναγνωριστικό καταχώρησης της συσκευής στην υπηρεσία δέσμευση Push και να λαμβάνουν τις ειδοποιήσεις, προσθέστε τα εξής για να σας `AndroidManifest.xml` αρχείο, εσωτερικά το `<application/>` ετικέτα (ακόμα και αν χρησιμοποιείτε ADM χωρίς δέσμευση):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Βεβαιωθείτε ότι έχετε τα ακόλουθα δικαιώματα στο σας `AndroidManifest.xml` (πριν από την `</application>` ετικέτας).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Εκχώρηση δέσμευση διακριτικό διαπιστευτήρια

Υποβολή OAuth τα διαπιστευτήριά σας (Αναγνωριστικό υπολογιστή-πελάτη και μυστικό προγράμματος-πελάτη) στην πύλη δέσμευση.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[Βιβλιοθήκη ADM προγράμματος-πελάτη]:https://developer.amazon.com/sdk/adm/setup.html
[ενσωματωμένη ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Αυτή η διαδικασία]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset

<properties
    pageTitle="Ενοποίηση Android SDK Azure δέσμευση κινητές συσκευές"
    description="Πιο πρόσφατες ενημερώσεις και διαδικασίες για Android SDK για δέσμευση Mobile Azure"
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
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Με τον τρόπο ενσωμάτωσης GCM δέσμευση κινητές συσκευές

> [AZURE.IMPORTANT] Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφονται στο το πώς μπορείτε να ενοποιήσετε δέσμευση σε Android εγγράφου πριν να ακολουθήσετε αυτόν τον οδηγό.
>
> Αυτό το έγγραφο είναι χρήσιμο μόνο εάν έχετε ήδη ενσωματωμένο τη λειτουργική μονάδα Reach και σχεδιασμός για να προωθήσετε Google Play συσκευές. Για να ενσωματώσετε Reach εκστρατείες στην εφαρμογή, διαβάστε πρώτα πώς μπορείτε να ενοποιήσετε δέσμευση φτάσετε στο Android.

##<a name="introduction"></a>Εισαγωγή

Ενοποίηση GCM επιτρέπει την εφαρμογή σας για να είναι δυνατή η προώθηση.

Περιέχουν ωφέλιμα φορτία GCM προωθηθεί στο SDK πάντα το `azme` κλειδιού στο αντικείμενο δεδομένων. Επομένως, εάν χρησιμοποιείτε GCM για κάποιον άλλο σκοπό στην εφαρμογή σας, μπορείτε να φιλτράρετε με βάση αυτό το πλήκτρο ωθεί.

> [AZURE.IMPORTANT] Μόνο οι συσκευές που εκτελούν Android έκδοση 2.2 ή άνω, έχοντας Google Play εγκατεστημένη και αντιμετωπίζετε Google φόντου σύνδεση ενεργοποιημένη μπορεί να είναι δυνατή η προώθηση χρησιμοποιώντας GCM; Ωστόσο, μπορείτε να ενοποιήσετε αυτόν τον κωδικό με ασφάλεια σε μη υποστηριζόμενες συσκευές (χρησιμοποιεί απλώς στόχοι).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Δημιουργία έργου Google Cloud μηνυμάτων με αριθμό-κλειδί API

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>Ενοποίηση του SDK

### <a name="managing-device-registrations"></a>Διαχείριση καταχωρήσεων συσκευή

Κάθε συσκευή πρέπει να στείλετε μια εντολή εγγραφής με τους διακομιστές Google, διαφορετικά δεν είναι δυνατό να φτάσει.

Μια συσκευή επίσης να unregister από τις ειδοποιήσεις GCM (η συσκευή είναι μη καταχωρημένες αυτόματα, εάν έχει καταργηθεί η εγκατάσταση της εφαρμογής).

Εάν δεν χρησιμοποιείτε το [Google Play SDK] ή δεν ήδη στέλνετε την πρόθεση εγγραφής στον εαυτό σας, μπορείτε να κάνετε δέσμευση καταχώρηση στη συσκευή αυτόματα για εσάς.

Για να ενεργοποιήσετε αυτή, προσθέστε τα εξής για να σας `AndroidManifest.xml` αρχείο, εσωτερικά το `<application/>` ετικέτα:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Επικοινωνία αναγνωριστικό δήλωσης για την υπηρεσία δέσμευση Push και να λαμβάνετε ειδοποιήσεις

Προκειμένου να επικοινωνούν το αναγνωριστικό καταχώρησης της συσκευής στην υπηρεσία δέσμευση Push και να λαμβάνουν τις ειδοποιήσεις, προσθέστε τα εξής για να σας `AndroidManifest.xml` αρχείο, εσωτερικά το `<application/>` ετικέτα (ακόμη και αν διαχειρίζεστε εγγραφές συσκευή εσείς οι ίδιοι):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Βεβαιωθείτε ότι έχετε τα ακόλουθα δικαιώματα στο σας `AndroidManifest.xml` (μετά το `</application>` ετικέτας).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Εκχώρηση πρόσβασης κινητές συσκευές δέσμευση για τον αριθμό-κλειδί API GCM

Ακολουθήστε [αυτόν τον Οδηγό](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) για την εκχώρηση πρόσβασης κινητών δέσμευση για τον αριθμό-κλειδί API GCM.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start

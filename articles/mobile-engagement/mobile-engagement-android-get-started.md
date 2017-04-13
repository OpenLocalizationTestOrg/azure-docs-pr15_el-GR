<properties
    pageTitle="Γρήγορα αποτελέσματα με Android εφαρμογές Azure Mobile δέσμευση"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και push ειδοποιήσεις για τις εφαρμογές του Android."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για εφαρμογές του Android

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα σας δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε τις χρήσης εφαρμογής και πώς μπορείτε να στείλετε τις ειδοποιήσεις push σε τμηματική χρήστες από μια εφαρμογή του Android.
Αυτό το πρόγραμμα εκμάθησης δείχνει το απλό σενάριο εκπομπής χρησιμοποιώντας δέσμευση Mobile. Σε αυτό, μπορείτε να δημιουργήσετε μια κενή εφαρμογή Android που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας Google Cloud ανταλλαγής μηνυμάτων (GCM).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Ολοκλήρωση αυτού του προγράμματος εκμάθησης απαιτεί το [Android εργαλεία για προγραμματιστές](https://developer.android.com/sdk/index.html), η οποία περιλαμβάνει το Android Studio ενσωματωμένο περιβάλλον ανάπτυξης και την πιο πρόσφατη πλατφόρμα Android.

Απαιτεί επίσης στο [Android SDK δέσμευση Mobile](https://aka.ms/vq9mfn).

> [AZURE.IMPORTANT] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Ρύθμιση Mobile δέσμευση για την εφαρμογή σας Android

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ολοκλήρωσης", που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. Μπορείτε να δημιουργήσετε μια βασική εφαρμογή με Android Studio για μια επίδειξη την ενοποίηση.

Μπορείτε να βρείτε την τεκμηρίωση πλήρη ενοποίηση στο την [ενοποίηση Android SDK δέσμευση Mobile](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Δημιουργήστε ένα έργο Android

1. Ξεκινήστε **Android Studio**και στο αναδυόμενο παράθυρο, επιλέξτε **Έναρξη νέου έργου Android Studio**.

    ![][1]

2. Δώστε έναν τομέα εφαρμογής όνομα και την εταιρεία. Σημειώστε τι κάνετε τη συμπλήρωση, επειδή που το χρειαστείτε αργότερα. Κάντε κλικ στο κουμπί **Επόμενο**.

    ![][2]

3. Επιλέξτε το επίπεδο API και διαστάσεων προορισμού και κάντε κλικ στο κουμπί **Επόμενο**.

    >[AZURE.NOTE] Δέσμευση Mobile απαιτεί επίπεδο API 10 ελάχιστη (Android 2.3.3).

    ![][3]

4. Επιλέξτε **Κενό δραστηριότητας** εδώ, που είναι η οθόνη που μόνο για αυτήν την εφαρμογή και κάντε κλικ στο κουμπί **Επόμενο**.

    ![][4]

5. Τέλος, αφήστε τις προεπιλογές ως έχει και κάντε κλικ στο κουμπί **Τέλος**.

    ![][5]

Android Studio δημιουργεί τώρα στην εφαρμογή επίδειξη στο οποίο θα σας ενοποίηση δέσμευση Mobile.

### <a name="include-the-sdk-library-in-your-project"></a>Συμπεριλάβετε τη βιβλιοθήκη SDK στο έργο σας

1. Κάντε λήψη του [Android SDK δέσμευση κινητές συσκευές](https://aka.ms/vq9mfn).
2. Εξαγάγετε το αρχείο αρχειοθέτησης σε ένα φάκελο στον υπολογιστή σας.
3. Προσδιορισμός στη βιβλιοθήκη .jar για την τρέχουσα έκδοση του αυτό το SDK και αντιγραφή του στο Πρόχειρο.

      ![][6]

4. Μεταβείτε στην ενότητα **έργου** (1) και επικολλήστε το .jar στο φάκελο βιβλιοθηκών (2).

      ![][7]

5. Για να φορτώσετε στη βιβλιοθήκη, συγχρονίστε το έργο.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου με τη συμβολοσειρά σύνδεσης

1. Αντιγράψτε τις ακόλουθες γραμμές κώδικα της δημιουργίας δραστηριότητας (πρέπει να εκτελεστούν μόνο σε ένα σημείο της εφαρμογής σας, συνήθως η κύρια δραστηριότητα). Για αυτήν την εφαρμογή δείγματος, ανοίξτε γρήγορα την MainActivity στην περιοχή src, -> κεντρική -> java φάκελο και προσθέστε το εξής:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Επίλυση τις αναφορές, πατώντας το συνδυασμό πλήκτρων Alt + Enter ή προσθέτοντας τις ακόλουθες δηλώσεις εισαγωγής:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Επιστρέψτε στην πύλη του Azure κλασική στη σελίδα **Πληροφορίες σύνδεσης** της εφαρμογής σας και αντιγράψτε τη **Συμβολοσειρά σύνδεσης**.

      ![][9]

4. Επικολλήστε τα σε το `setConnectionString` παράμετρο, αντικαθιστώντας ολόκληρη η συμβολοσειρά φαίνεται στον ακόλουθο κώδικα:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Προσθήκη δικαιωμάτων και μια δήλωση υπηρεσίας

1. Προσθέστε αυτά τα δικαιώματα για το Manifest.xml του έργου σας αμέσως πριν ή μετά την `<application>` ετικέτα:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Για να δηλώσετε την υπηρεσία παράγοντα, προσθέστε αυτόν τον κωδικό μεταξύ του `<application>` και `</application>` ετικέτες:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Στον κώδικα που επικολλήσατε, αντικαταστήστε `"<Your application name>"` στην ετικέτα, που εμφανίζεται στο μενού " **Ρυθμίσεις** ", όπου μπορείτε να δείτε τις υπηρεσίες που εκτελούνται στη συσκευή. Μπορείτε να προσθέσετε τη λέξη "Υπηρεσίας" σε αυτήν την ετικέτα για παράδειγμα.

### <a name="send-a-screen-to-mobile-engagement"></a>Αποστολή σε οθόνη στο κινητό δέσμευση

Για να ξεκινήσετε την αποστολή δεδομένων και να διασφαλίσετε ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη (δραστηριότητα) στον υπολογιστή στο παρασκήνιο δέσμευση Mobile.

Μεταβείτε στο **MainActivity.java** και προσθέστε τα εξής για να αντικαταστήσετε τη βασική κλάση της **MainActivity** να **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Εάν σας βασικής κλάσης δεν είναι *δραστηριότητα*, συμβουλευτείτε [Για προχωρημένους Android αναφοράς](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) για το πώς μπορείτε να λαμβάνουν στοιχεία από διαφορετικές κατηγορίες.


Σχολιάσετε την ακόλουθη γραμμή για αυτό το σενάριο απλού δείγματος:

    // setSupportActionBar(toolbar);

Εάν θέλετε να διατηρήσετε το `ActionBar` στην εφαρμογή σας, ανατρέξτε στην ενότητα [Advanced Android αναφοράς](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes).

## <a name="connect-app-with-real-time-monitoring"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Κατά τη διάρκεια μιας εκστρατείας, δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν και ΕΠΊΤΕΥΞΗ τους χρήστες σας με τις ειδοποιήσεις push και ανταλλαγή μηνυμάτων της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Στην παρακάτω ενότητα ρυθμίζει την εφαρμογή σας για τη λήψη τους.

### <a name="copy-sdk-resources-in-your-project"></a>Αντιγραφή SDK πόρων στο έργο σας

1. Μεταβείτε ξανά το περιεχόμενό σας SDK λήψης και αντιγράψτε το φάκελο **πόροι** .

    ![][10]

2. Μεταβείτε ξανά για να Android Studio, επιλέξτε τον **κύριο** κατάλογο του αρχεία του έργου σας και, στη συνέχεια, επικολλήστε, για να προσθέσετε τους πόρους στο έργο σας.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Μεταβείτε στο [Android SDK](mobile-engagement-android-sdk-overview.md) για να λάβετε λεπτομερείς γνώσεων σχετικά με την ενοποίηση SDK.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png

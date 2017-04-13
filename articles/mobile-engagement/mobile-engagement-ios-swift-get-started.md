<properties
    pageTitle="Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για iOS στο Σουίφτ | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και τις ειδοποιήσεις Push για iOS εφαρμογές."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για τις εφαρμογές στον Σουίφτ iOS

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε την χρήση εφαρμογών και αποστολή ειδοποιήσεων push σε τμηματική χρήστες σε μια εφαρμογή του iOS.
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια εφαρμογή κενό iOS που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας σύστημα ειδοποιήσεων Push της Apple (APNS).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ 8 XCode, η οποία μπορείτε να εγκαταστήσετε από το MAC App Store
+ η [Δέσμευση Mobile iOS SDK]
+ Πιστοποιητικό ειδοποιήσεων Push (.p12) που μπορείτε να λάβετε σε το Κέντρο ανάπτυξης Apple

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Σουίφτ έκδοση 3.0. 

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης δέσμευση Mobile για τις εφαρμογές του iOS.

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>Ρύθμιση Mobile δέσμευση για την εφαρμογή σας iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ολοκλήρωσης", που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. Μπορείτε να βρείτε την τεκμηρίωση ολοκλήρωσης ενοποίησης στο την [ενοποίηση SDK iOS Mobile δέσμευση](mobile-engagement-ios-sdk-overview.md)

Θα δημιουργήσουμε μια βασική εφαρμογή με XCode για μια επίδειξη την ενοποίηση:

###<a name="create-a-new-ios-project"></a>Δημιουργήστε ένα νέο έργο iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στο κινητό δέσμευση παρασκηνίου

1. Κάντε λήψη του [iOS δέσμευση Mobile SDK]
2. Εξαγάγετε το. tar.gz αρχείο σε ένα φάκελο στον υπολογιστή σας
3. Κάντε δεξί κλικ στο έργο και επιλέξτε "Προσθήκη αρχείων σε..."

    ![][1]

4. Μεταβείτε στο φάκελο όπου έχετε κάνει εξαγωγή του SDK και επιλέξτε το `EngagementSDK` φακέλου, στη συνέχεια, πατήστε το κουμπί OK.

    ![][2]

5. Άνοιγμα του `Build Phases` καρτέλα και το `Link Binary With Libraries` μενού προσθέσετε τα πλαίσια, όπως φαίνεται παρακάτω:

    ![][3]

8. Δημιουργήστε μια επικεφαλίδα γεφύρωση για να μπορέσετε να χρησιμοποιήσετε το SDK στόχο C APIs επιλέγοντας Αρχείο > Δημιουργία > Αρχείο > iOS > προέλευση > αρχείο κεφαλίδας.

    ![][4]

9. Επεξεργαστείτε το αρχείο γεφύρωσης κεφαλίδας για να εκθέσετε κώδικα Mobile δέσμευση στόχο-C ταχείας κώδικα, προσθέστε τις ακόλουθες εισαγωγές:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Στην περιοχή Δημιουργία ρυθμίσεις, βεβαιωθείτε ότι η Δόμηση κεφαλίδα γεφύρωση στόχο-C τη ρύθμιση στην περιοχή προγράμματος μεταγλώττισης Σουίφτ - δημιουργία κώδικα έχει μια διαδρομή για αυτήν την κεφαλίδα. Ακολουθεί ένα παράδειγμα διαδρομής: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (ανάλογα με τη διαδρομή)**

    ![][6]

11. Επιστρέψτε στην πύλη του Azure στη σελίδα *Πληροφορίες σύνδεσης* της εφαρμογής σας και αντιγράψτε τη συμβολοσειρά σύνδεσης

    ![][5]

12. Τώρα, επικολλήστε τη συμβολοσειρά σύνδεσης στο το `didFinishLaunchingWithOptions` πληρεξουσίου

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

Για να ξεκινήσετε την αποστολή δεδομένων και την εξασφάλιση ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη (δραστηριότητα) στον υπολογιστή στο παρασκήνιο δέσμευση Mobile.

1. Ανοίξτε το αρχείο **ViewController.swift** και να αντικαταστήσετε τη βασική κλάση της **ViewController** να **EngagementViewController**:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων Push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν και να ΕΠΙΚΟΙΝΩΝΉΣΕΤΕ με τους χρήστες σας με τις ειδοποιήσεις Push και μηνύματα της εφαρμογής στο περιβάλλον της εκστρατείες. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες θα εγκατάστασης την εφαρμογή σας για τη λήψη τους.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push χωρίς μηνύματα

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Προσθήκη στη βιβλιοθήκη Reach στο έργο σας

1. Κάντε δεξί κλικ στην επιλογή το έργο σας
2. Επιλέξτε`Add file to ...`
3. Μεταβείτε στο φάκελο όπου έχετε κάνει εξαγωγή του SDK
4. Επιλέξτε το `EngagementReach` φακέλου
5. Κάντε κλικ στην επιλογή Προσθήκη
6. Επεξεργαστείτε το αρχείο γεφύρωσης κεφαλίδας για να εκθέσετε επίτευξη στόχο-C δέσμευση Mobile κεφαλίδες και προσθέστε τις ακόλουθες εισαγωγές:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Τροποποίηση εφαρμογής πληρεξούσιο

1. Εντός του `didFinishLaunchingWithOptions` - Δημιουργήστε μια λειτουργική μονάδα reach και μεταβιβάζουν την υπάρχουσα γραμμή προετοιμασίας δέσμευση:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push APNS
1. Προσθέστε την ακόλουθη γραμμή για να το `didFinishLaunchingWithOptions` μέθοδο:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Προσθήκη του `didRegisterForRemoteNotificationsWithDeviceToken` μέθοδο ως εξής:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Προσθήκη του `didReceiveRemoteNotification:fetchCompletionHandler:` μέθοδο ως εξής:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Δέσμευση Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png

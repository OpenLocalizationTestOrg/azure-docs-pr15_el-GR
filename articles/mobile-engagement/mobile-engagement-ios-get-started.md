<properties
    pageTitle="Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για iOS στο στόχο C | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Mobile δέσμευση με ανάλυση και push ειδοποιήσεις για τις εφαρμογές του iOS."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>Γρήγορα αποτελέσματα με δέσμευση Mobile Azure για εφαρμογές iOS στο στόχο C

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure Mobile δέσμευση για να κατανοήσετε την χρήση εφαρμογών και αποστολή ειδοποιήσεων push σε τμηματική χρήστες σε μια εφαρμογή του iOS.
Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια εφαρμογή κενό iOS που συλλέγει βασικές δεδομένα και να λαμβάνει τις ειδοποιήσεις push χρησιμοποιώντας σύστημα ειδοποιήσεων Push της Apple (APNS).

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ 8 XCode, η οποία μπορείτε να εγκαταστήσετε από το MAC App Store
+ η [Δέσμευση Mobile iOS SDK]

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης δέσμευση Mobile για τις εφαρμογές του iOS.

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>Ρύθμιση Mobile δέσμευση για την εφαρμογή σας iOS

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει μια "βασικές ολοκλήρωσης", που είναι το ελάχιστο σύνολο που απαιτείται για τη συλλογή δεδομένων και να στείλετε μια ειδοποίηση push. Μπορείτε να βρείτε την τεκμηρίωση ολοκλήρωσης ενοποίησης στο την [ενοποίηση SDK iOS Mobile δέσμευση](mobile-engagement-ios-sdk-overview.md)

Θα δημιουργήσουμε μια βασική εφαρμογή με XCode για μια επίδειξη την ενοποίηση.

###<a name="create-a-new-ios-project"></a>Δημιουργήστε ένα νέο έργο iOS

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Σύνδεση της εφαρμογής σας στον υπολογιστή στο παρασκήνιο Mobile δέσμευση

1. Κάντε λήψη του [iOS δέσμευση Mobile SDK].
2. Εξαγάγετε το. tar.gz αρχείο σε ένα φάκελο στον υπολογιστή σας.
3. Κάντε δεξί κλικ στο έργο και, στη συνέχεια, επιλέξτε **Προσθήκη αρχείων σε**.

    ![][1]

4. Μεταβείτε στο φάκελο όπου έχετε κάνει εξαγωγή του SDK, επιλέξτε το `EngagementSDK` φακέλου και, στη συνέχεια, πατήστε **OK**.

    ![][2]

5. Ανοίξτε την καρτέλα **Δημιουργία φάσεις** και στο μενού **Δυαδικό με βιβλιοθήκες σύνδεσης** , προσθέστε τα πλαίσια, όπως φαίνεται παρακάτω:

    ![][3]

6. Επιστρέψτε στην πύλη του Azure στη σελίδα **Πληροφορίες σύνδεσης** της εφαρμογής σας και αντιγράψτε τη συμβολοσειρά σύνδεσης.

    ![][4]

7. Προσθέστε την ακόλουθη γραμμή κώδικα στο αρχείο σας **AppDelegate.m** .

        #import "EngagementAgent.h"

8. Τώρα, επικολλήστε τη συμβολοσειρά σύνδεσης στο το `didFinishLaunchingWithOptions` πληρεξουσίου.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`είναι μια προαιρετική πρόταση που σας επιτρέπει SDK αρχεία καταγραφής για να εντοπίσετε ζητήματα. 

##<a id="monitor"></a>Ενεργοποίηση παρακολούθησης σε πραγματικό χρόνο

Για να ξεκινήσετε την αποστολή δεδομένων και την εξασφάλιση ότι οι χρήστες είναι ενεργό, πρέπει να στείλετε τουλάχιστον μία οθόνη (δραστηριότητα) στον υπολογιστή στο παρασκήνιο δέσμευση Mobile.

1. Ανοίξτε το αρχείο **ViewController.h** και εισαγωγή **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Τώρα αντικατάσταση της υπερ-κλάσης του περιβάλλοντος εργασίας **ViewController** με `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Σύνδεση εφαρμογής με παρακολούθηση σε πραγματικό χρόνο

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ενεργοποίηση ειδοποιήσεων push και της εφαρμογής ανταλλαγής μηνυμάτων

Δέσμευση Mobile σάς επιτρέπει να αλληλεπιδρούν οι χρήστες σας και να ΕΠΙΚΟΙΝΩΝΉΣΕΤΕ με τις ειδοποιήσεις push και την ανταλλαγή μηνυμάτων στο περιβάλλον της εκστρατείες της εφαρμογής. Η ενότητα αυτή ονομάζεται REACH στην πύλη του δέσμευση Mobile.
Οι παρακάτω ενότητες ρύθμιση της εφαρμογής σας για τη λήψη τους.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push χωρίς μηνύματα

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Προσθήκη στη βιβλιοθήκη Reach στο έργο σας

1. Κάντε δεξιό κλικ στο έργο σας.
2. Επιλέξτε **Προσθήκη αρχείο**.
3. Μεταβείτε στο φάκελο όπου έχετε κάνει εξαγωγή του SDK.
4. Επιλέξτε το `EngagementReach` φακέλου.
5. Κάντε κλικ στην επιλογή **Προσθήκη**.

### <a name="modify-your-application-delegate"></a>Τροποποίηση εφαρμογής πληρεξούσιο

1. Επιστροφή στο αρχείο **AppDeletegate.m** , εισαγάγετε τη λειτουργική μονάδα επίτευξη δέσμευση.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Εντός του `application:didFinishLaunchingWithOptions` μέθοδο, δημιουργήστε μια λειτουργική μονάδα Reach και μεταβιβάζουν το στη γραμμή του υπάρχοντος προετοιμασίας δέσμευση:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push APNS

1. Προσθέστε την ακόλουθη γραμμή για να το `application:didFinishLaunchingWithOptions` μέθοδο:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Προσθήκη του `application:didRegisterForRemoteNotificationsWithDeviceToken` μέθοδο ως εξής:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Προσθήκη του `didFailToRegisterForRemoteNotificationsWithError` μέθοδο ως εξής:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Προσθήκη του `didReceiveRemoteNotification:fetchCompletionHandler` μέθοδο ως εξής:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Δέσμευση Mobile iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png


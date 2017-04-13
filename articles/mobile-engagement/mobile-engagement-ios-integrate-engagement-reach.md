<properties
    pageTitle="Azure δέσμευση Mobile iOS ενοποίησης επίτευξη SDK | Microsoft Azure"
    description="Πιο πρόσφατες ενημερώσεις και διαδικασίες για iOS SDK για δέσμευση Mobile Azure"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Πώς μπορείτε να ενοποιήσετε επίτευξη δέσμευση στο iOS

Πρέπει να ακολουθήσετε τη διαδικασία ενοποίησης που περιγράφεται το [πώς μπορείτε να ενοποιήσετε δέσμευση στο έγγραφο iOS](mobile-engagement-ios-integrate-engagement.md) πριν να ακολουθήσετε αυτόν τον οδηγό.

Παρούσα τεκμηρίωση απαιτεί XCode 8. Εάν κάνετε πραγματικά εξαρτώνται από XCode 7, στη συνέχεια, μπορείτε να χρησιμοποιήσετε το [iOS v3.2.4 SDK δέσμευση](https://aka.ms/r6oouh). Υπάρχει ένα σφάλμα γνωστά σε αυτήν την προηγούμενη έκδοση ενώ εκτελείται σε συσκευές iOS 10: οι ειδοποιήσεις συστήματος δεν είναι actioned. Για να διορθώσετε αυτό, θα πρέπει να υλοποιήσετε το API που έχουν καταργηθεί `application:didReceiveRemoteNotification:` στην εφαρμογή πληρεξουσίου ως εξής:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Αυτό δεν συνιστάται αυτή η λύση** ως αυτήν τη συμπεριφορά να αλλάξετε σε οποιαδήποτε έκδοση αναβάθμισης επερχόμενες iOS (ακόμη και δευτερευουσών) επειδή αυτή iOS API έχει καταργηθεί. Θα πρέπει να μεταβαίνετε σε XCode 8 όσο το δυνατόν πιο σύντομα.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ενεργοποιήστε την εφαρμογή σας για να λαμβάνετε ειδοποιήσεις Push χωρίς μηνύματα

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Ενοποίηση βήματα

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Ενσωμάτωση του SDK επίτευξη δέσμευση στο έργο σας iOS

-   Προσθέστε το sdk Reach στο έργο σας Xcode. Στο Xcode, μεταβείτε στις επιλογές **έργου \> Προσθήκη έργο** και επιλέξτε το `EngagementReach` φακέλου.

### <a name="modify-your-application-delegate"></a>Τροποποίηση εφαρμογής πληρεξούσιο

-   Στο επάνω μέρος του αρχείου σας υλοποίηση, εισαγάγετε τη λειτουργική μονάδα επίτευξη δέσμευση:

        [...]
        #import "AEReachModule.h"

-   Μέσα σε μέθοδο `applicationDidFinishLaunching:` ή `application:didFinishLaunchingWithOptions:`, να δημιουργήσετε μια λειτουργική μονάδα reach και μεταβιβάζουν την υπάρχουσα γραμμή προετοιμασίας δέσμευση:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Τροποποίηση συμβολοσειρά **'icon.png'** με το όνομα της εικόνας που θέλετε να ορίσετε ως το εικονίδιο ειδοποίησης.
-   Εάν θέλετε να χρησιμοποιήσετε την επιλογή *τιμή σήματος ενημέρωσης* στο Reach εκστρατείες ή εάν θέλετε να χρησιμοποιήσετε εγγενούς push \<ΑΔΑ/Reach API/εκστρατείας μορφή/εγγενής Push\> εκστρατείες, θα πρέπει να αφήσετε το Reach λειτουργική μονάδα Διαχείριση το σήμα εικονίδιο ίδια (θα αυτόματα καταργήστε την επιλογή από το σήμα εφαρμογών και επίσης να επαναφέρετε την τιμή που είναι αποθηκευμένα με δέσμευση κάθε φορά που η εφαρμογή είναι αποτελέσματα ή foregrounded). Αυτό γίνεται, προσθέτοντας την ακόλουθη γραμμή μετά την προετοιμασία Reach λειτουργικής μονάδας:

        [reach setAutoBadgeEnabled:YES];

-   Εάν θέλετε να χειριστείτε Reach push δεδομένων, πρέπει να επιτρέψετε την εφαρμογή πληρεξούσιο συμφωνούν με την `AEReachDataPushDelegate` πρωτόκολλο. Προσθέστε την ακόλουθη γραμμή μετά την προετοιμασία Reach λειτουργικής μονάδας:

        [reach setDataPushDelegate:self];

-   Στη συνέχεια, μπορείτε να εφαρμόσετε τις μεθόδους `onDataPushStringReceived:` και `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` στον πληρεξούσιο εφαρμογής:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Κατηγορία

Η παράμετρος κατηγορία είναι προαιρετική κατά τη δημιουργία μιας εκστρατείας Push δεδομένων και σας επιτρέπει να φιλτράρετε δεδομένα προωθεί. Αυτό είναι χρήσιμο εάν θέλετε να push διαφορετικά είδη των `Base64` δεδομένων και θέλετε να προσδιορίσετε τον τύπο τους πριν από την ανάλυση τους.

**Η εφαρμογή σας είναι τώρα έτοιμο για λήψη και να εμφανίσετε περιεχόμενο reach!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Πώς μπορείτε να λαμβάνετε ανακοινώσεις και ψηφοφορίες ανά πάσα στιγμή

Δέσμευση μπορεί να σας στείλει Reach ειδοποιήσεις στους τελικούς χρήστες σας οποιαδήποτε στιγμή, χρησιμοποιώντας την υπηρεσία ειδοποιήσεων Push Apple.

Για να ενεργοποιήσετε αυτήν τη λειτουργία, θα πρέπει να προετοιμάσετε την εφαρμογή για τις ειδοποιήσεις push της Apple και τροποποίηση πληρεξούσιο εφαρμογής.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Προετοιμασία της εφαρμογής σας για τις ειδοποιήσεις push της Apple

Ακολουθήστε τον οδηγό: [Πώς να προετοιμάσετε την εφαρμογή για τις ειδοποιήσεις Push της Apple](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Προσθήκη του κώδικα είναι απαραίτητο προγράμματος-πελάτη

*Σε αυτό το σημείο την εφαρμογή σας θα πρέπει να έχετε ένα καταχωρημένο πιστοποιητικό push της Apple στο το frontend δέσμευση.*

Εάν δεν γίνεται ήδη, πρέπει να καταχωρήσετε την εφαρμογή σας να λαμβάνουν ειδοποιήσεις προώθησης.

* Εισαγωγή του `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Προσθέστε την ακόλουθη γραμμή κατά την εκκίνηση της εφαρμογής σας (συνήθως σε `application:didFinishLaunchingWithOptions:`):

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

Στη συνέχεια, πρέπει να παράσχετε στους δέσμευση το διακριτικό συσκευή που επιστρέφονται από τους διακομιστές Apple. Αυτό γίνεται στη μέθοδο με το όνομα `application:didRegisterForRemoteNotificationsWithDeviceToken:` στον πληρεξούσιο εφαρμογής:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Τέλος, θα πρέπει να σας ενημερώσει για το SDK δέσμευση όταν η εφαρμογή σας λαμβάνει μια ειδοποίηση απομακρυσμένης. Για να το κάνετε αυτό, η κλήση της μεθόδου `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` στον πληρεξούσιο εφαρμογής:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Την παραπάνω μέθοδο εισάγεται στο iOS 7. Εάν στοχεύετε σε iOS < 7, βεβαιωθείτε ότι για την υλοποίηση της μεθόδου `application:didReceiveRemoteNotification:` στην εφαρμογή πληρεξουσίων ή κλήση `applicationDidReceiveRemoteNotification` στην το EngagementAgent, μεταφέροντας υπόψη αντί για το `handler` όρισμα:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Από προεπιλογή, δέσμευση επίτευξη ελέγχει το completionHandler. Εάν θέλετε να απαντήσετε με μη αυτόματο τρόπο το `handler` αποκλείσετε στον κώδικά σας, μπορείτε να περάσετε υπόψη το `handler` το όρισμα και το στοιχείο ελέγχου την ολοκλήρωση αποκλεισμός μόνοι σας. Ανατρέξτε στο θέμα το `UIBackgroundFetchResult` τύπο για μια λίστα των πιθανών τιμών.


### <a name="full-example"></a>Παράδειγμα πλήρους

Ακολουθεί ένα παράδειγμα πλήρη ενοποίηση:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Εάν έχετε τη δική σας υλοποίηση UNUserNotificationCenterDelegate

Το SDK έχει επίσης τη δική του υλοποίηση του πρωτοκόλλου UNUserNotificationCenterDelegate. Χρησιμοποιείται από το SDK για την παρακολούθηση του κύκλου ζωής δέσμευσης ειδοποιήσεις σε συσκευές που εκτελείται σε iOS 10 ή μεγαλύτερη. Εάν το SDK εντοπίζει ο πληρεξούσιος δεν θα χρησιμοποιήσει τη δική του υλοποίηση επειδή μπορεί να είναι μόνο ένας πληρεξούσιος UNUserNotificationCenter ανά εφαρμογή. Αυτό σημαίνει ότι θα πρέπει να προσθέσετε τη λογική δέσμευση πληρεξούσιο το δικό σας.

Υπάρχουν δύο τρόποι για να επιτύχετε το εξής.

Απλώς μέσω της προώθησης πληρεξούσιο κλήσεις στο SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Ή με τη μεταβίβαση από το `AEUserNotificationHandler` τάξης

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Μπορείτε να προσδιορίσετε αν μια ειδοποίηση προέρχεται από δέσμευση ή δεν διέρχονται τα `userInfo` λεξικό στον παράγοντα `isEngagementPushPayload:` κλάση μέθοδο.

##<a name="how-to-customize-campaigns"></a>Πώς μπορείτε να προσαρμόσετε εκστρατείες

### <a name="notifications"></a>Ειδοποιήσεις

Υπάρχουν δύο τύποι των ειδοποιήσεων: ειδοποιήσεις συστήματος και της εφαρμογής.

Σύστημα ειδοποιήσεις αντιμετωπίζονται από iOS και δεν μπορούν να προσαρμοστούν.

Ειδοποιήσεις της εφαρμογής έχουν κατασκευαστεί μιας προβολής που προστίθεται δυναμικά στο τρέχον παράθυρο εφαρμογής. Αυτή η διαδικασία ονομάζεται μια επικάλυψη ειδοποίησης. Ειδοποίηση επικαλύψεων είναι ιδανικά για μια γρήγορη ενοποίησης επειδή δεν απαιτούν να τροποποιήσετε οποιαδήποτε προβολή στην εφαρμογή σας.

#### <a name="layout"></a>Διάταξη

Για να τροποποιήσετε την εμφάνιση των τις ειδοποιήσεις της εφαρμογής, μπορείτε απλώς να τροποποιήσετε το αρχείο `AENotificationView.xib` για τις ανάγκες σας, αρκεί να διατηρείτε την ετικέτα τιμές και τύπους την υπάρχουσα δευτερευουσών προβολών.

Από προεπιλογή, οι ειδοποιήσεις της εφαρμογής παρουσιάζονται στο κάτω μέρος της οθόνης. Εάν προτιμάτε να τα εμφανίσετε στο επάνω μέρος της οθόνης, επεξεργαστείτε το παρεχόμενο `AENotificationView.xib` και αλλάξτε το `AutoSizing` ιδιότητα από την κύρια προβολή, ώστε να διατηρηθούν στο επάνω μέρος του superview.

#### <a name="categories"></a>Κατηγορίες

Όταν τροποποιήσετε τη διάταξη που παρέχεται, μπορείτε να τροποποιήσετε την εμφάνιση των όλες οι ειδοποιήσεις σας. Κατηγορίες σάς επιτρέπουν να ορίσετε διάφορες εμφανίσεις προορισμού (πιθανώς συμπεριφορές) για τις ειδοποιήσεις. Μια κατηγορία μπορεί να καθοριστεί κατά τη δημιουργία μιας εκστρατείας Reach. Λάβετε υπόψη ότι οι κατηγορίες σάς επιτρέπουν επίσης να προσαρμόσετε ανακοινώσεις και ψηφοφορίες, που περιγράφεται παρακάτω σε αυτό το έγγραφο.

Για να καταχωρήσετε ένα πρόγραμμα χειρισμού κατηγορίας για τις ειδοποιήσεις, πρέπει να προσθέσετε μια κλήση αφού τη λειτουργική μονάδα reach έχει προετοιμαστεί.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`πρέπει να είναι μια παρουσία του αντικειμένου που συμφωνεί με το πρωτόκολλο `AENotifier`.

Από τον εαυτό σας, μπορείτε να εφαρμόσετε τις μεθόδους πρωτόκολλο ή μπορείτε να επιλέξετε να reimplement την υπάρχουσα κλάση `AEDefaultNotifier` που εκτελεί ήδη το μεγαλύτερο μέρος της εργασίας.

Για παράδειγμα, εάν θέλετε να επανακαθορίσετε την Προβολή ειδοποίησης για μια συγκεκριμένη κατηγορία, μπορείτε να ακολουθήσετε αυτό το παράδειγμα:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Αυτό το απλό παράδειγμα της κατηγορίας, ας υποθέσουμε ότι έχετε ένα αρχείο που ονομάζεται `MyNotificationView.xib` σε σας δέσμη κύρια εφαρμογή. Εάν δεν μπορείτε να βρείτε ένα αντίστοιχο τη μέθοδο `.xib`, την ειδοποίηση δεν θα εμφανίζεται και δέσμευση θα εξόδου ένα μήνυμα στην κονσόλα.

Το αρχείο που παρέχεται πένας πρέπει να τηρεί τους παρακάτω κανόνες:

-   Θα πρέπει να περιέχει μόνο μία προβολή.
-   Δευτερευουσών προβολών πρέπει να είναι από τους ίδιους τύπους με αυτά μέσα στο αρχείο πένας που παρέχεται με το όνομα`AENotificationView.xib`
-   Δευτερευουσών προβολών θα πρέπει να έχετε τις ίδιες ετικέτες ως αυτά μέσα στην παρεχόμενη πένας αρχείο με το όνομα`AENotificationView.xib`

> [AZURE.TIP] Απλώς αντιγράψτε το αρχείο που παρέχεται πένας, με το όνομα `AENotificationView.xib`, και να ξεκινήσετε να εργάζεστε από εκεί. Αλλά πρέπει να είστε προσεκτικοί, η προβολή μέσα σε αυτό το αρχείο πένας είναι συσχετισμένη με την κλάση `AENotificationView`. Αυτή η κλάση επαναπροσδιορισμός τη μέθοδο `layoutSubViews` για να μετακινήσετε και να αλλάξετε το μέγεθος του δευτερευουσών προβολών ανάλογα με το περιβάλλον. Ίσως θελήσετε να την αντικαταστήσετε με μια `UIView` ή προσαρμοσμένη προβολή κλάσης.

Εάν χρειάζεστε περαιτέρω προσαρμογή των ειδοποιήσεων σας (Εάν θέλετε για παράδειγμα για τη φόρτωση της προβολής σας απευθείας από τον κωδικό), συνιστάται να Ρίξτε μια ματιά στην τεκμηρίωση κωδικό και κλάσης παρεχόμενη προέλευσης του `Protocol ReferencesDefaultNotifier` και `AENotifier`.

Σημειώστε ότι μπορείτε να χρησιμοποιήσετε την ίδια ειδοποίηση για πολλές κατηγορίες.

Μπορείτε επίσης να επαναπροσδιορισμός Αναγγελία του προεπιλεγμένου ως εξής:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Διαχείριση ειδοποιήσεων

Όταν χρησιμοποιείτε την προεπιλεγμένη κατηγορία, ορισμένες μεθόδους κύκλου ζωής ονομάζονται στην το `AEReachContent` αντικειμένου για να αναφέρουν στατιστικά στοιχεία και να ενημερώσει την κατάσταση εκστρατείας:

-   Όταν εμφανίζεται η ειδοποίηση στην εφαρμογή, η `displayNotification` μέθοδος καλείται (το οποίο αναφέρει στατιστικά στοιχεία) με `AEReachModule` εάν `handleNotification:` επιστρέφει `YES`.
-   Εάν κλείσετε την ειδοποίηση, την `exitNotification` καλείται μέθοδος, αναφέρεται στατιστικής και τώρα είναι δυνατή η επεξεργασία επόμενη εκστρατείες.
-   Εάν κάνετε κλικ σε την ειδοποίηση, `actionNotification` είναι που ονομάζεται, αναφέρεται η στατιστική τιμή και εκτελείται η συσχετισμένη ενέργεια.

Εάν την εφαρμογή του, `AENotifier` παρακάμπτει την προεπιλεγμένη συμπεριφορά, πρέπει να καλέσετε τις παρακάτω μεθόδους κύκλου ζωής μόνοι σας. Τα παρακάτω παραδείγματα εξηγούν ορισμένες περιπτώσεις όπου παρακαμφθούν την προεπιλεγμένη συμπεριφορά:

-   Δεν μπορείτε να επεκτείνετε `AEDefaultNotifier`, π.χ. υλοποιηθεί χειρισμού κατηγορία από την αρχή.
-   Που overrode `prepareNotificationView:forContent:`, φροντίστε να αντιστοιχίσετε τουλάχιστον `onNotificationActioned` ή `onNotificationExited` σε μία από τις U.I στοιχεία ελέγχου.

> [AZURE.WARNING] Εάν `handleNotification:` παρουσιάζει μια εξαίρεση, το περιεχόμενο έχει διαγραφεί και `drop` είναι ονομάζεται, αυτό αναφέρεται σε στατιστικά στοιχεία και τώρα είναι δυνατή η επεξεργασία επόμενη εκστρατείες.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Συμπεριλάβετε ειδοποίηση ως μέρος μια υπάρχουσα προβολή

Επικαλύψεων είναι ιδανικά για μια γρήγορη ενοποίησης, αλλά μπορεί να είναι μερικές φορές δεν εύχρηστο ή μπορεί να παρουσιάσει ανεπιθύμητα αποτελέσματα.

Εάν δεν είστε ικανοποιημένοι με το σύστημα επικάλυψης σε ορισμένες από τις προβολές, μπορείτε να την προσαρμόσετε για αυτές τις προβολές.

Μπορείτε να αποφασίσετε να συμπεριλάβετε μας διάταξη ειδοποίηση σε υπάρχουσες προβολές σας. Για να το κάνετε, υπάρχει δύο εφαρμογή στυλ:

1.  Προσθήκη της προβολής ειδοποίηση χρησιμοποιώντας τη Δόμηση περιβάλλοντος εργασίας

    -   Άνοιγμα *περιβάλλοντος εργασίας του εργαλείου δόμησης*
    -   Τοποθετήστε ένα 320 x 60 (ή 768 x 60 Εάν είστε σε iPad) `UIView` όπου θέλετε την ειδοποίηση για να εμφανίζονται
    -   Ορίστε την τιμή ετικέτας για αυτήν την προβολή σε: **36822491**

2.  Προσθέστε την Προβολή ειδοποίησης μέσω προγραμματισμού. Προσθέστε τον ακόλουθο κώδικα μόνο όταν έχει γίνει προετοιμασία της προβολής σας:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`Μπορείτε να βρείτε μακροεντολής στο `AEDefaultNotifier.h`.

> [AZURE.NOTE] Αναγγελία του προεπιλεγμένου εντοπίζει αυτόματα ότι τη διάταξη ειδοποίηση περιλαμβάνεται σε αυτήν την προβολή και δεν θα προσθέσει μια επικάλυψη για αυτήν.

### <a name="announcements-and-polls"></a>Ανακοινώσεις και ψηφοφορίες

#### <a name="layouts"></a>Διατάξεις

Μπορείτε να τροποποιήσετε τα αρχεία `AEDefaultAnnouncementView.xib` και `AEDefaultPollView.xib` με την προϋπόθεση ότι μπορείτε να διατηρήσετε την ετικέτα τιμές και τύπους την υπάρχουσα δευτερευουσών προβολών.

#### <a name="categories"></a>Κατηγορίες

##### <a name="alternate-layouts"></a>Εναλλακτικές διατάξεις

Όπως ειδοποιήσεις, κατηγορία της εκστρατείας μπορεί να χρησιμοποιηθεί για να έχουν εναλλακτικές διατάξεις για τις ανακοινώσεις και ψηφοφορίες.

Για να δημιουργήσετε μια κατηγορία για μια ανακοίνωση, πρέπει να επεκτείνετε **AEAnnouncementViewController** και να καταχωρήσετε όταν έχει γίνει προετοιμασία της λειτουργικής μονάδας reach:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Κάθε φορά που ένας χρήστης θα κάντε κλικ σε μια ειδοποίηση για μια ανακοίνωση με την κατηγορία "μου\_κατηγορία", ελεγκτή σας καταχωρημένες προβολή (σε αυτήν την περίπτωση `MyCustomAnnouncementViewController`) θα γίνει προετοιμασία καλώντας τη μέθοδο `initWithAnnouncement:` και η προβολή θα προστεθεί στο τρέχον παράθυρο εφαρμογής.

Κατά την εκτέλεση του το `AEAnnouncementViewController` κλάσης θα πρέπει να διαβάσετε την ιδιότητα `announcement` προετοιμασία σας δευτερευουσών προβολών. Εξετάστε το ενδεχόμενο να το παρακάτω παράδειγμα, όπου δύο ετικέτες είναι προετοιμασία με χρήση `title` και `body` ιδιοτήτων του `AEReachAnnouncement` κλάσης:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Εάν δεν θέλετε να φορτώσετε σας προβολές του με εσάς, αλλά απλώς θέλετε να χρησιμοποιήσετε ξανά την προεπιλεγμένη διάταξη προβολή ανακοίνωση, μπορείτε απλώς να κάνετε ελεγκτή σας προσαρμοσμένη προβολή επεκτείνει την κλάση παρεχόμενη `AEDefaultAnnouncementViewController`. Αναπαραγωγή σε αυτή την περίπτωση, το αρχείο πένας `AEDefaultAnnouncementView.xib` και μετονομάστε την, ώστε να μπορεί να μεταφερθεί από τον ελεγκτή προσαρμοσμένη προβολή (για έναν ελεγκτή με το όνομα `CustomAnnouncementViewController`, θα πρέπει να καλέσετε το αρχείο πένας `CustomAnnouncementView.xib`).

Για να αντικαταστήσετε την προεπιλεγμένη κατηγορία ανακοινώσεων, απλώς καταχώρηση του ελεγκτή σας προσαρμοσμένη προβολή για την κατηγορία που ορίζονται στο `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Ψηφοφορίες μπορεί να προσαρμοστεί με τον ίδιο τρόπο:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Αυτήν τη στιγμή, στην παρεχόμενη `MyCustomPollViewController` πρέπει να επεκτείνετε `AEPollViewController`. Ή μπορείτε να επιλέξετε για να επεκτείνετε από τον προεπιλεγμένο ελεγκτή: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Μην ξεχάσετε να καλέσετε είτε `action` (`submitAnswers:` για ελεγκτές προβολή προσαρμοσμένης ψηφοφορίας) ή `exit` μέθοδο πριν κλείσετε την προβολή ελεγκτή. Διαφορετικά, στατιστικά στοιχεία δεν θα αποσταλεί (δηλαδή χωρίς ανάλυση σε εκστρατείας) και περισσότερες σημαντικό επόμενη εκστρατείες δεν θα ειδοποιηθούν μέχρι να γίνει επανεκκίνηση της διαδικασίας εφαρμογών.

##### <a name="implementation-example"></a>Παράδειγμα εφαρμογής

Σε αυτήν την εφαρμογή στην προβολή προσαρμοσμένης ανακοίνωση φορτώνεται από ένα αρχείο εξωτερικών xib.

Όπως για Προσαρμογή ειδοποιήσεων για προχωρημένους, καλό είναι να εξετάσετε τον πηγαίο κώδικα της τυπική υλοποίηση.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end

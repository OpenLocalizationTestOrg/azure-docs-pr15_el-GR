<properties
    pageTitle="Azure δέσμευση Mobile iOS τη διαδικασία αναβάθμισης SDK | Microsoft Azure"
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

#<a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του δέσμευση στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Για κάθε νέα έκδοση του SDK πρέπει να πρώτα να αντικαταστήσετε (Κατάργηση και εκ νέου εισαγωγή στο xcode) τους φακέλους EngagementSDK και EngagementReach.

##<a name="from-300-to-400"></a>Από το 3.0.0 να 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 είναι υποχρεωτική ξεκινώντας από την έκδοση 4.0.0 του SDK.

> [AZURE.NOTE] Εάν κάνετε πραγματικά εξαρτώνται από XCode 7, στη συνέχεια, μπορείτε να χρησιμοποιήσετε το [iOS v3.2.4 SDK δέσμευση](https://aka.ms/r6oouh). Υπάρχει ένα σφάλμα γνωστά στον τη λειτουργική μονάδα reach αυτής της προηγούμενης έκδοσης ενώ εκτελείται σε συσκευές iOS 10: οι ειδοποιήσεις συστήματος δεν είναι actioned. Για να διορθώσετε αυτό, θα πρέπει να υλοποιήσετε το API που έχουν καταργηθεί `application:didReceiveRemoteNotification:` στην εφαρμογή πληρεξουσίου ως εξής:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Αυτό δεν συνιστάται αυτή η λύση** ως αυτήν τη συμπεριφορά να αλλάξετε σε οποιαδήποτε έκδοση αναβάθμισης επερχόμενες iOS (ακόμη και δευτερευουσών) επειδή αυτή iOS API έχει καταργηθεί. Θα πρέπει να μεταβαίνετε σε XCode 8 όσο το δυνατόν πιο σύντομα.

### <a name="usernotifications-framework"></a>Πλαίσιο UserNotifications
Πρέπει να προσθέσετε το `UserNotifications` framework σε σας φάσεις που δημιουργείτε.

στην Εξερεύνηση έργου, ανοίξτε το παράθυρο έργου και επιλέξτε τη σωστή προορισμού. Στη συνέχεια, ανοίξτε την καρτέλα **"Δημιουργία φάσεις"** και στο μενού **"δυαδικό με βιβλιοθήκες σύνδεσης"** , προσθέστε framework `UserNotifications.framework` -Ορισμός τη σύνδεση ως`Optional`

### <a name="application-push-capability"></a>Η δυνατότητα push εφαρμογής
XCode 8 μπορεί να επαναφέρετε την εφαρμογή σας push τη δυνατότητα, ανατρέξτε διπλή μεταβίβαση ελέγχου του `capability` καρτέλα του την επιλεγμένη προορισμού.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Προσθήκη του κώδικα εγγραφής της νέας ειδοποίησης iOS 10
Το παλαιότερο τμήμα κώδικα για την καταχώρηση της εφαρμογής σε ειδοποιήσεις εξακολουθεί να λειτουργεί αλλά χρησιμοποιεί που έχουν καταργηθεί APIs ενώ εκτελείται σε iOS 10.

Εισαγωγή του `User Notification` framework:

        #import <UserNotifications/UserNotifications.h> 

Στην εφαρμογή πληρεξούσιο `application:didFinishLaunchingWithOptions` αντικατάσταση μέθοδο:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

από:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Εάν έχετε ήδη τη δική σας υλοποίηση UNUserNotificationCenterDelegate

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

##<a name="from-200-to-300"></a>Από το 2.0.0 να 3.0.0
Υποστήριξη για iOS αποτεθεί 4.X. Ξεκινώντας από αυτήν την έκδοση προορισμού ανάπτυξη της εφαρμογής σας πρέπει να είναι τουλάχιστον iOS 6.

Εάν χρησιμοποιείτε Reach στην εφαρμογή σας, θα πρέπει να προσθέσετε `remote-notification` τιμή για το `UIBackgroundModes` πίνακα στο αρχείο σας Info.plist προκειμένου να λαμβάνουν ειδοποιήσεις απομακρυσμένης.

Η μέθοδος `application:didReceiveRemoteNotification:` πρέπει να αντικατασταθεί με `application:didReceiveRemoteNotification:fetchCompletionHandler:` στον πληρεξούσιο εφαρμογής.

"AEPushDelegate.h" έχει καταργηθεί περιβάλλον εργασίας και πρέπει να καταργήσετε όλες τις αναφορές. Αυτό περιλαμβάνει την κατάργηση `[[EngagementAgent shared] setPushDelegate:self]` και οι μέθοδοι πληρεξουσίου από πληρεξούσιο εφαρμογής:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Από το 1.16.0 να 2.0.0
Παρακάτω περιγράφεται ο τρόπος για να μετεγκαταστήσετε μια ενοποίηση SDK από την υπηρεσία Capptain που παρέχεται από Capptain συσχετισμούς Ασφαλείας σε μια εφαρμογή που υποστηρίζεται από Azure Mobile δέσμευση.
Εάν κάνετε μετεγκατάσταση από μια προηγούμενη έκδοση, επικοινωνήστε με την τοποθεσία web Capptain για μετεγκατάσταση σε 1.16 πρώτα και, στη συνέχεια, εφαρμόστε την ακόλουθη διαδικασία.

>[AZURE.IMPORTANT] Capptain και δέσμευση Mobile δεν είναι τις ίδιες υπηρεσίες και τη διαδικασία που ακολουθεί επισημαίνει μόνο με τη μετεγκατάσταση στην εφαρμογή υπολογιστή-πελάτη. Μετεγκατάσταση στο SDK στην εφαρμογή θα δεν μετεγκατάσταση των δεδομένων σας από τους διακομιστές Capptain με τους διακομιστές Mobile δέσμευση

### <a name="agent"></a>Παράγοντας

Η μέθοδος `registerApp:` έχει αντικατασταθεί από τη νέα μέθοδο `init:`. Ο πληρεξούσιος εφαρμογής πρέπει να ενημερωθούν αναλόγως και χρησιμοποιήστε συμβολοσειρά σύνδεσης:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Παρακολούθηση SmartAd έχει καταργηθεί από SDK πρέπει απλώς να καταργήσετε όλες τις εμφανίσεις της `AETrackModule` τάξης

### <a name="class-name-changes"></a>Αλλαγές ονομάτων τάξης

Ως μέρος του rebranding, υπάρχουν μερικά ονόματα κλάσης/αρχείων που πρέπει να αλλάξουν.

Όλες τις κατηγορίες το πρόθεμα "CP" έχουν μετονομαστεί με πρόθεμα "Πσ".

Παράδειγμα:

-   `CPModule.h`έχει μετονομαστεί σε `AEModule.h`.

Όλες τις κατηγορίες το πρόθεμα "Capptain" έχουν μετονομαστεί με πρόθεμα "Δέσμευση".

Παραδείγματα:

-   Τάξη `CapptainAgent` έχει μετονομαστεί σε `EngagementAgent`.
-   Τάξη `CapptainTableViewController` έχει μετονομαστεί σε `EngagementTableViewController`.
-   Τάξη `CapptainUtils` έχει μετονομαστεί σε `EngagementUtils`.
-   Τάξη `CapptainViewController` έχει μετονομαστεί σε `EngagementViewController`.

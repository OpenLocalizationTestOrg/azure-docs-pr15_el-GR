<properties
    pageTitle="Azure δέσμευση Mobile iOS Επισκόπηση SDK | Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK για δέσμευση Mobile Azure

Ξεκινήστε εδώ για να λάβετε όλες τις λεπτομέρειες σχετικά με τον τρόπο για να ενσωματώσετε Azure Mobile δέσμευση σε μια εφαρμογή iOS. Εάν θέλετε να του δώσετε μια, δοκιμάστε πρώτα, βεβαιωθείτε ότι μπορείτε να κάνετε το [πρόγραμμα εκμάθησης 15 λεπτά](mobile-engagement-ios-get-started.md).

Κάντε κλικ στο κουμπί για να δείτε το [Περιεχόμενο SDK](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Διαδικασίες ολοκλήρωσης
1. Ξεκινήστε εδώ: [πώς μπορείτε να ενοποιήσετε δέσμευση Mobile στην εφαρμογή iOS](mobile-engagement-ios-integrate-engagement.md)

2. Για τις ειδοποιήσεις: [πώς μπορείτε να ενοποιήσετε Reach (ειδοποιήσεις) στην εφαρμογή iOS](mobile-engagement-ios-integrate-engagement-reach.md)

3. Προσθήκη ετικετών σε εφαρμογή πρόγραμμα: [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή iOS](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Σημειώσεις έκδοσης

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Σταθερή ειδοποίηση δεν actioned σε συσκευές iOS 10.
-   Υποβάθμιση XCode 7.

Για την προηγούμενη έκδοση, ανατρέξτε στις [Σημειώσεις έκδοσης ολοκλήρωσης](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του δέσμευση στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Ίσως χρειαστεί να ακολουθήσετε αρκετές διαδικασίες Εάν χάσατε πολλές εκδόσεις SDK δείτε την πλήρη [Αναβάθμιση διαδικασίες](mobile-engagement-ios-upgrade-procedure.md).

Για κάθε νέα έκδοση του SDK πρέπει να πρώτα να αντικαταστήσετε (Κατάργηση και εκ νέου εισαγωγή στο xcode) τους φακέλους EngagementSDK και EngagementReach.

###<a name="from-300-to-400"></a>Από το 3.0.0 να 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 είναι υποχρεωτική ξεκινώντας από την έκδοση 4.0.0 του SDK.

> [AZURE.NOTE] Εάν κάνετε πραγματικά εξαρτώνται από XCode 7, στη συνέχεια, μπορείτε να χρησιμοποιήσετε το [iOS v3.2.4 SDK δέσμευση](https://aka.ms/r6oouh). Υπάρχει ένα σφάλμα γνωστά στον τη λειτουργική μονάδα reach αυτής της προηγούμενης έκδοσης ενώ εκτελείται σε συσκευές iOS 10: οι ειδοποιήσεις συστήματος δεν είναι actioned. Για να διορθώσετε αυτό, θα πρέπει να υλοποιήσετε το API που έχουν καταργηθεί `application:didReceiveRemoteNotification:` στην εφαρμογή πληρεξουσίου ως εξής:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] **Αυτό δεν συνιστάται αυτή η λύση** ως αυτήν τη συμπεριφορά να αλλάξετε σε οποιαδήποτε έκδοση αναβάθμισης επερχόμενες iOS (ακόμη και δευτερευουσών) επειδή αυτή iOS API έχει καταργηθεί. Θα πρέπει να μεταβαίνετε σε XCode 8 όσο το δυνατόν πιο σύντομα.

#### <a name="usernotifications-framework"></a>Πλαίσιο UserNotifications
Πρέπει να προσθέσετε το `UserNotifications` framework σε σας φάσεις που δημιουργείτε.

στην Εξερεύνηση έργου, ανοίξτε το παράθυρο έργου και επιλέξτε τη σωστή προορισμού. Στη συνέχεια, ανοίξτε την καρτέλα **"Δημιουργία φάσεις"** και στο μενού **"δυαδικό με βιβλιοθήκες σύνδεσης"** , προσθέστε framework `UserNotifications.framework` -Ορισμός τη σύνδεση ως`Optional`

#### <a name="application-push-capability"></a>Η δυνατότητα push εφαρμογής
XCode 8 μπορεί να επαναφέρετε την εφαρμογή σας push τη δυνατότητα, ανατρέξτε διπλή μεταβίβαση ελέγχου του `capability` καρτέλα του την επιλεγμένη προορισμού.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Προσθήκη του κώδικα εγγραφής της νέας ειδοποίησης iOS 10
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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Εάν έχετε ήδη τη δική σας υλοποίηση UNUserNotificationCenterDelegate

Το SDK έχει τη δική του υλοποίηση του πρωτοκόλλου UNUserNotificationCenterDelegate. Χρησιμοποιείται από το SDK για την παρακολούθηση του κύκλου ζωής δέσμευσης ειδοποιήσεις σε συσκευές που εκτελείται σε iOS 10 ή μεγαλύτερη. Εάν το SDK εντοπίζει ο πληρεξούσιος δεν θα χρησιμοποιήσει τη δική του υλοποίηση επειδή μπορεί να είναι μόνο ένας πληρεξούσιος UNUserNotificationCenter ανά εφαρμογή. Αυτό σημαίνει ότι θα πρέπει να προσθέσετε τη λογική δέσμευση πληρεξούσιο το δικό σας.

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
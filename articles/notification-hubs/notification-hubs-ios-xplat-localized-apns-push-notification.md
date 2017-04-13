<properties
    pageTitle="Ειδοποίηση διανομείς μεταφραστεί διακοπή ειδήσεις το πρόγραμμα εκμάθησης για iOS"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Service Bus ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων μεταφρασμένα πρόσφατες ειδήσεις (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Χρήση διανομείς ειδοποίηση για την αποστολή μεταφρασμένα πρόσφατες ειδήσεις για συσκευές iOS

> [AZURE.SELECTOR]
- [C# του Windows Store](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md) του Azure ειδοποίηση διανομείς μετάδοση πρόσφατες ειδήσεις ειδοποιήσεις που έχουν προσαρμοστεί, γλώσσα και τη συσκευή. Σε αυτό το πρόγραμμα εκμάθησης ξεκινάτε με την εφαρμογή iOS που έχουν δημιουργηθεί με [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις]. Όταν ολοκληρωθεί η εγκατάσταση, θα μπορείτε να εγγραφείτε για κατηγορίες που σας ενδιαφέρει, καθορίστε μια γλώσσα στην οποία θέλετε να λαμβάνετε τις ειδοποιήσεις και λαμβάνετε μόνο τις ειδοποιήσεις push για επιλεγμένες κατηγορίες στη συγκεκριμένη γλώσσα.


Υπάρχουν δύο μέρη σε αυτό το σενάριο:

- εφαρμογή iOS επιτρέπει προγράμματος-πελάτη συσκευών, για να καθορίσετε μια γλώσσα, καθώς και για να εγγραφείτε για διαφορετικές πρόσφατες ειδήσεις κατηγορίες.

- το παρασκηνίου εκπέμπει τις ειδοποιήσεις, χρησιμοποιώντας την **ετικέτα** και **πρότυπο** feautres του Azure ειδοποίηση διανομείς.



##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πρέπει να έχετε ήδη ολοκληρώσει το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] και έχετε τον κώδικα που είναι διαθέσιμη, επειδή αυτό το πρόγραμμα εκμάθησης βασίζεται απευθείας σε αυτόν τον κώδικα.

Visual Studio 2012 ή νεότερη έκδοση είναι προαιρετικό.



##<a name="template-concepts"></a>Πρότυπο έννοιες

Σε [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] δημιουργηθεί μια εφαρμογή που χρησιμοποιούνται **ετικέτες** για να εγγραφείτε σε ειδοποιήσεις για ειδήσεις διαφορετικές κατηγορίες.
Πολλές εφαρμογές, ωστόσο, στοχεύστε πολλών αγορές και απαιτούν τοπικής προσαρμογής. Αυτό σημαίνει ότι το περιεχόμενο των ειδοποιήσεων τον εαυτό τους πρέπει να μεταφραστεί και παραδόθηκε στο σωστό σύνολο των συσκευών.
Σε αυτό το θέμα θα δείξουμε πώς μπορείτε να χρησιμοποιήσετε τη δυνατότητα **πρότυπο** της ειδοποίησης διανομείς για παράδοση εύκολα τις μεταφρασμένα πρόσφατες ειδήσεις ειδοποιήσεις.

Σημείωση: είναι ένας τρόπος για να στείλετε τις μεταφρασμένα ειδοποιήσεις για να δημιουργήσετε πολλές εκδόσεις του κάθε ετικέτα. Για παράδειγμα, για την υποστήριξη Αγγλικά, γαλλικά και Mandarin, θα πρέπει τρεις διαφορετικές ετικέτες για διεθνείς ειδήσεις: "world_en", "world_fr" και "world_ch". Θα σας, στη συνέχεια, θα πρέπει να στείλετε μια μεταφρασμένη έκδοση του τα νέα κόσμο σε κάθε μία από αυτές τις ετικέτες. Σε αυτό το θέμα, μπορούμε να χρησιμοποιήσουμε πρότυπα για να αποφύγετε τη διάδοση ετικέτες και την απαίτηση αποστολής πολλά μηνύματα.

Σε υψηλό επίπεδο, πρότυπα είναι ένας τρόπος για να καθορίσετε πώς μια συγκεκριμένη συσκευή θα λαμβάνουν μια ειδοποίηση. Το πρότυπο καθορίζει τη μορφή ακριβή φορτίο με αναφορά σε ιδιότητες που αποτελούν μέρος του μηνύματος που αποστέλλονται από το app υποστήριξης. Σε περίπτωση μας, θα στείλουμε ένα μήνυμα agnostic τοπικές ρυθμίσεις που περιέχει όλες τις υποστηριζόμενες γλώσσες:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Στη συνέχεια, θα σας εξασφαλίζει ότι οι συσκευές καταχώρηση με ένα πρότυπο που αναφέρεται στη σωστή ιδιότητα. Για παράδειγμα, μια εφαρμογή iOS που θέλει να καταχωρήσετε για Γαλλικά συζητήσεων θα καταχωρήσετε τα εξής:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Τα πρότυπα είναι μια πολύ ισχυρά δυνατότητα που μπορείτε να μάθετε περισσότερα σχετικά με την στο άρθρο μας [πρότυπα](notification-hubs-templates-cross-platform-push-messages.md) .

##<a name="the-app-user-interface"></a>Το περιβάλλον εργασίας χρήστη της εφαρμογής

Τώρα θα σας θα τροποποιήσετε την εφαρμογή διακοπή συζητήσεων που δημιουργήσατε στο θέμα [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] για να στείλετε μεταφρασμένα πρόσφατες ειδήσεις με τη χρήση προτύπων.


Στο MainStoryboard_iPhone.storyboard σας, προσθέστε ένα στοιχείο ελέγχου τμηματική με τις τρεις γλώσσες που θα υποστηρίζουμε: Αγγλικά, γαλλικά και Mandarin.

![][13]

Στη συνέχεια, βεβαιωθείτε ότι για να προσθέσετε μια IBOutlet στο ViewController.h σας, όπως φαίνεται παρακάτω:

![][14]

##<a name="building-the-ios-app"></a>Δημιουργία της εφαρμογής iOS


1. Στο σας Notification.h προσθέστε τη μέθοδο *retrieveLocale* , και να τροποποιήσετε το χώρο αποθήκευσης και εγγραφή μεθόδους, όπως φαίνεται παρακάτω:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Στο Notification.m σας, τροποποιήστε τη μέθοδο *storeCategoriesAndSubscribe* , προσθέτοντας την παράμετρο τοπικές ρυθμίσεις και την αποθήκευση σε τις προεπιλογές χρήστη:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Στη συνέχεια, να τροποποιήσετε τη μέθοδο *εγγραφή* για να συμπεριλάβετε τις τοπικές ρυθμίσεις:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Παρατηρήστε πώς τώρα χρησιμοποιούμε τη μέθοδο *registerTemplateWithDeviceToken*, αντί για *registerNativeWithDeviceToken*. Όταν θα σας εγγραφείτε για ένα πρότυπο που έχετε για την παροχή του προτύπου json, καθώς και ένα όνομα για το πρότυπο (όπως μας εφαρμογή μπορεί να θέλετε να καταχωρήσετε διαφορετικά πρότυπα). Βεβαιωθείτε ότι για να καταχωρήσετε τις κατηγορίες ως ετικετών, όπως για να βεβαιωθείτε ότι λαμβάνετε τα notifciations για αυτές τις πληροφορίες.

    Προσθέστε μια μέθοδο για να ανακτήσετε τις τοπικές ρυθμίσεις από τις προεπιλεγμένες ρυθμίσεις χρήστη:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Τώρα που θα σας τροποποίησης μας κλάσης ειδοποιήσεις, έχουμε για να βεβαιωθείτε ότι το ViewController χρησιμοποιεί τη νέα UISegmentControl. Προσθέστε την ακόλουθη γραμμή στη μέθοδο *viewDidLoad* για να βεβαιωθείτε ότι για να εμφανίσετε τις τοπικές ρυθμίσεις που είναι επιλεγμένο τη συγκεκριμένη στιγμή:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    Στη συνέχεια, στο μεθόδου *εγγραφή* , αλλάξτε την κλήση σε το *storeCategoriesAndSubscribe* με το εξής:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Τέλος, πρέπει να ενημερώσετε τη μέθοδο *didRegisterForRemoteNotificationsWithDeviceToken* στο AppDelegate.m σας, ώστε να μπορείτε να ανανεώσετε την εγγραφή σας σωστά κατά την εκκίνηση της εφαρμογής σας. Αλλαγή της κλήσης για τη μέθοδο *εγγραφή* των ειδοποιήσεων με τα εξής:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(προαιρετικό) Αποστολή ειδοποιήσεων μεταφρασμένα προτύπου από το app κονσόλας .NET.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(προαιρετικό) Αποστολή ειδοποιήσεων μεταφρασμένα πρότυπο από τη συσκευή

Εάν δεν έχετε πρόσβαση στο Visual Studio, ή απλώς θέλετε να δοκιμάσετε να στέλνει τις ειδοποιήσεις μεταφρασμένα προτύπου απευθείας από την εφαρμογή στη συσκευή.  Μπορείτε να κάνετε απλή τις μεταφρασμένα πρότυπο παραμέτρους για να προσθέσετε το `SendNotificationRESTAPI` μέθοδο που έχετε ορίσει στην προηγούμενη εκμάθηση.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τη χρήση προτύπων, ανατρέξτε στο θέμα:

- [Ειδοποιήστε τους χρήστες με διανομείς ειδοποίηση: ASP.NET]
- [Ειδοποιήστε τους χρήστες με διανομείς ειδοποίηση: υπηρεσίες Mobile]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Ειδοποιήστε τους χρήστες με διανομείς ειδοποίηση: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Ειδοποιήστε τους χρήστες με διανομείς ειδοποίηση: υπηρεσίες Mobile]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx

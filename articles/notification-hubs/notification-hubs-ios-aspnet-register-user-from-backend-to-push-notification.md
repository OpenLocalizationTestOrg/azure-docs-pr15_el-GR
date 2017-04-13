<properties
    pageTitle="Να καταχωρήσει τον τρέχοντα χρήστη για τις ειδοποιήσεις push χρησιμοποιώντας το API Web | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ζητήσετε δήλωσης ειδοποιήσεων push σε μια εφαρμογή iOS με διανομείς Azure ειδοποίηση όταν registeration πραγματοποιείται από το API Web ASP.NET."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Να καταχωρήσει τον τρέχοντα χρήστη για τις ειδοποιήσεις push χρησιμοποιώντας ASP.NET

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να ζητήσετε δήλωσης ειδοποιήσεων push με διανομείς Azure ειδοποίησης κατά την καταχώρηση πραγματοποιείται από το API Web ASP.NET. Αυτό το θέμα επεκτείνει το πρόγραμμα εκμάθησης [ειδοποίηση οι χρήστες με ειδοποίηση διανομείς]. Πρέπει να έχετε ήδη ολοκληρώσει τα απαραίτητα βήματα σε αυτό το πρόγραμμα εκμάθησης για να δημιουργήσετε το με έλεγχο ταυτότητας υπηρεσία κινητών τηλεφώνων. Για περισσότερες πληροφορίες σχετικά με την ειδοποίηση σενάριο χρήστες, ανατρέξτε στο θέμα [ειδοποίηση οι χρήστες με ειδοποίηση διανομείς].

##<a name="update-your-app"></a>Ενημέρωση της εφαρμογής σας  

1. Στο MainStoryboard_iPhone.storyboard σας, προσθέστε τα ακόλουθα στοιχεία από τη βιβλιοθήκη του αντικειμένου:

    + **Ετικέτα**: "Push χρήστη με διανομείς ειδοποίησης"
    + **Ετικέτα**: "InstallationId"
    + **Ετικέτα**: "Χρήστης"
    + **Πεδίο κειμένου**: "Χρήστης"
    + **Ετικέτα**: "Κωδικός πρόσβασης"
    + **Πεδίο κειμένου**: "Κωδικός πρόσβασης"
    + **Κουμπί**: "Σύνδεση"

    Σε αυτό το σημείο του πίνακα διάταξης μοιάζει με τα εξής:

    ![][0]

2. Στο πρόγραμμα επεξεργασίας Βοηθού, δημιουργία πριζών για όλα τα στοιχεία ελέγχου μεταγωγής και τους καλέσει, συνδεθείτε τα πεδία κειμένου με την προβολή ελεγκτή (πληρεξούσιος), και να δημιουργήσετε μια **ενέργεια** για το κουμπί **σύνδεσης** .

    ![][1]

    Το αρχείο BreakingNewsViewController.h τώρα θα πρέπει να περιέχει τον ακόλουθο κώδικα:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Δημιουργία μιας κλάσης με το όνομα **DeviceInfo**και αντιγράψτε τον παρακάτω κώδικα στην ενότητα περιβάλλον εργασίας του αρχείου DeviceInfo.h:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Αντιγράψτε τον παρακάτω κώδικα στην ενότητα υλοποίησης του αρχείου DeviceInfo.m:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. Στο PushToUserAppDelegate.h, προσθέστε τα εξής singleton ιδιότητα:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. Στη μέθοδο **didFinishLaunchingWithOptions** στο PushToUserAppDelegate.m, προσθέστε τον ακόλουθο κώδικα:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Στην πρώτη γραμμή προετοιμάζει το singleton **DeviceInfo** . Η δεύτερη γραμμή ξεκινά η καταχώρηση για push ειδοποιήσεις, η οποία είναι ήδη παρουσίαση είναι ήδη έχετε ολοκληρώσει το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς] .

9. Στο PushToUserAppDelegate.m, εφαρμόζετε τη μέθοδο **didRegisterForRemoteNotificationsWithDeviceToken** στον AppDelegate σας και προσθέστε τον ακόλουθο κώδικα:

        self.deviceInfo.deviceToken = deviceToken;

    Καθορίζει το διακριτικό συσκευή για την αίτηση.

    > [AZURE.NOTE] Σε αυτό το σημείο, θα πρέπει να μην υπάρχει οποιοδήποτε άλλο κώδικα σε αυτήν τη μέθοδο. Εάν έχετε ήδη μια κλήση προς τη μέθοδο **registerNativeWithDeviceToken** που προστέθηκε όταν ολοκληρωθεί το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με διανομείς ειδοποίησης](/manage/services/notification-hubs/get-started-notification-hubs-ios/) , πρέπει σχόλιο ανάληψης ή να καταργήσετε αυτήν την κλήση.

10. Στο αρχείο PushToUserAppDelegate.m, προσθέστε την ακόλουθη μέθοδο πρόγραμμα χειρισμού:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Αυτή η μέθοδος εμφανίζει μια ειδοποίηση το περιβάλλον εργασίας χρήστη κατά την εφαρμογή σας λαμβάνει ειδοποιήσεις ενώ εκτελείται.

9. Ανοίξτε το αρχείο PushToUserViewController.m, και να επιστρέψετε το πληκτρολόγιο κατά την εφαρμογή παρακάτω:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. Στη μέθοδο **viewDidLoad** στο αρχείο PushToUserViewController.m, προετοιμασία της ετικέτας installationId ως εξής:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Προσθέστε τις ακόλουθες ιδιότητες στο περιβάλλον εργασίας στο PushToUserViewController.m:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Στη συνέχεια, προσθέστε την ακόλουθη εφαρμογή:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Αντιγράψτε τον παρακάτω κώδικα της μεθόδου πρόγραμμα χειρισμού **σύνδεσης** που δημιουργήθηκε από XCode:

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Αυτή η μέθοδος λαμβάνει τόσο ένα Αναγνωριστικό εγκατάστασης και ένα κανάλι για τις ειδοποιήσεις push και σας αποστέλλει, μαζί με τον τύπο της συσκευής, στη μέθοδο Web API με έλεγχο ταυτότητας που δημιουργεί μια καταχώρηση στην ειδοποίηση διανομείς. Αυτό το API Web έχει οριστεί στην [ειδοποίηση οι χρήστες με ειδοποίηση διανομείς].

Τώρα που η εφαρμογή προγράμματος-πελάτη έχει ενημερωθεί, επιστρέψτε στο [ειδοποίηση οι χρήστες με ειδοποίηση διανομείς] και ενημερώστε την υπηρεσία κινητών τηλεφώνων για την αποστολή ειδοποιήσεων με χρήση ειδοποιήσεων διανομείς.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Ειδοποιήστε τους χρήστες με διανομείς ειδοποίησης]: /manage/services/notification-hubs/notify-users-aspnet

[Γρήγορα αποτελέσματα με διανομείς ειδοποίησης]: /manage/services/notification-hubs/get-started-notification-hubs-ios

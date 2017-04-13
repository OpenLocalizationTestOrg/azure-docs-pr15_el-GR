<properties
    pageTitle="Azure ειδοποιήσεων Push εμπλουτισμένου διανομείς"
    description="Μάθετε πώς μπορείτε να στείλετε τις ειδοποιήσεις push εμπλουτισμένου σε μια εφαρμογή iOS από το Azure. Δείγματα κώδικα γραμμένο σε στόχο-C και C#."
    documentationCenter="ios"
    services="notification-hubs"
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

#<a name="azure-notification-hubs-rich-push"></a>Azure ειδοποιήσεων Push εμπλουτισμένου διανομείς


##<a name="overview"></a>Επισκόπηση

Για να την προσοχή των χρηστών με άμεσα εμπλουτισμένο περιεχόμενο, μια εφαρμογή μπορεί να θέλετε να push εκτός από απλό κείμενο. Αυτές οι ειδοποιήσεις Προβιβασμός αλληλεπιδράσεις των χρηστών και παρουσίαση περιεχομένου όπως διευθύνσεις URL, ήχων, εικόνες/κουπόνια και πολλά άλλα. Αυτό το πρόγραμμα εκμάθησης δημιουργεί το θέμα [Σας ειδοποιεί τους χρήστες](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) και δείχνει πώς μπορείτε να στείλετε τις ειδοποιήσεις push που ενσωματώνουν ωφέλιμα φορτία (για παράδειγμα, εικόνα).


Αυτό το πρόγραμμα εκμάθησης είναι συμβατή με iOS 7 και 8.

  ![][IOS1]

Σε υψηλό επίπεδο:

1. Η εφαρμογή υποστήριξης:
    - Αποθηκεύει το Εμπλουτισμένο φορτίο (σε αυτήν την περίπτωση, εικόνα) στο χώρο αποθήκευσης της βάσης δεδομένων/τοπικό υπολογιστή στο παρασκήνιο
    - Στέλνει το Αναγνωριστικό του αυτή η ειδοποίηση εμπλουτισμένου στη συσκευή
2. Εφαρμογή στη συσκευή:
    - Επαφών που ζητά το Εμπλουτισμένο φορτίο με το Αναγνωριστικό που λαμβάνει υπόβαθρο
    - Αποστέλλει ειδοποιήσεις χρήστες στη συσκευή κατά την ανάκτηση δεδομένων έχει ολοκληρωθεί και εμφανίζει το φορτίο αμέσως μόλις χρήστες πατήστε για να μάθετε περισσότερα


## <a name="webapi-project"></a>WebAPI έργου

1. Στο Visual Studio, ανοίξτε το έργο **AppBackend** που δημιουργήσατε στην εκμάθηση [Ειδοποιήστε τους χρήστες](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .
2. Αποκτήστε μια εικόνα που θέλετε να ειδοποιήσετε τους χρήστες με και να την τοποθετήσετε σε ένα φάκελο **img** στον κατάλογο του έργου σας.
3. Κάντε κλικ στην επιλογή **Εμφάνιση όλων των αρχείων** στην Εξερεύνηση λύσεων και κάντε δεξί κλικ στο φάκελο για να **Συμπεριλάβετε στο έργο**.
4. Έχοντας επιλέξει την εικόνα, αλλάξτε την ενέργεια Δημιουργία στο παράθυρο Ιδιότητες **Ενσωματωμένου**πόρο.

    ![][IOS2]

5. Στο **Notifications.cs**, προσθέστε τα ακόλουθα χρησιμοποιώντας πρόταση:

        using System.Reflection;

6. Ενημερώστε την κλάση ολόκληρη **ειδοποιήσεις** με τον ακόλουθο κώδικα. Φροντίστε να αντικαταστήσετε τους χαρακτήρες κράτησης θέσης με το διαπιστευτήρια διανομέα ειδοποίησης και το όνομα αρχείου εικόνας.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (προαιρετικό) Ανατρέξτε [Πώς να ενσωματώσετε και πρόσβαση σε πόρους με χρήση του Visual C#](http://support.microsoft.com/kb/319292) για περισσότερες πληροφορίες σχετικά με τον τρόπο για να προσθέσετε και να αποκτήσετε πόρων έργου.

7. Στο **NotificationsController.cs**, επαναπροσδιορισμός **NotificationsController** με τα εξής τμήματα. Αυτό αποστέλλει ένα αναγνωριστικό αρχικό σιωπηρή εμπλουτισμένου ειδοποίηση συσκευή και σας επιτρέπει ανάκτησης πλευρά του προγράμματος-πελάτη της εικόνας:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Τώρα θα σας θα αναπτύξετε εκ νέου αυτής της εφαρμογής σε μια τοποθεσία Web Azure προκειμένου να κάνουν το λογισμικό προσβάσιμο από όλες τις συσκευές. Κάντε δεξί κλικ στο έργο **AppBackend** και επιλέξτε **Δημοσίευση**.

9. Επιλέξτε Azure τοποθεσίας Web ως προορισμό σας δημοσίευση. Συνδεθείτε με το λογαριασμό σας Azure και επιλέξτε μια υπάρχουσα ή μια νέα τοποθεσία Web και σημειώστε την ιδιότητα **διεύθυνση URL προορισμού** στην καρτέλα **σύνδεση** . Θα αναφέρουμε αυτή η διεύθυνση URL ως το *τελικό σημείο υποστήριξης* παρακάτω σε αυτό το πρόγραμμα εκμάθησης. Κάντε κλικ στο κουμπί **Δημοσίευση**.

## <a name="modify-the-ios-project"></a>Τροποποίηση του έργου iOS

Τώρα που έχετε τροποποιήσει σας παρασκηνίου εφαρμογής για να στείλετε απλώς το *αναγνωριστικό* της ειδοποίησης, θα μπορείτε να αλλάξετε την εφαρμογή iOS χειρίζεται αυτό το αναγνωριστικό και να ανακτήσετε το εμπλουτισμένο μήνυμα από τον υπολογιστή στο παρασκήνιο.

1. Ανοίξτε το έργο σας iOS και ενεργοποιήσετε τις ειδοποιήσεις απομακρυσμένης μεταβαίνοντας στο σας κύριο εφαρμογής προορισμού στην ενότητα **προορισμών** .

2. Κάντε κλικ στην επιλογή **δυνατότητες**, ενεργοποιήστε την επιλογή **Λειτουργίες φόντου**και επιλέξτε το πλαίσιο ελέγχου **Απομακρυσμένο ειδοποιήσεις** .

    ![][IOS3]

3. Μεταβείτε στο **Main.storyboard**και βεβαιωθείτε ότι έχετε επιλέξει μια προβολή ελεγκτή (που προβλέπονται ως κεντρική προβολή ελεγκτή σε αυτό το πρόγραμμα εκμάθησης) από το πρόγραμμα εκμάθησης [Ειδοποιήστε χρήστη](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .

4. Προσθέστε έναν **Ελεγκτή περιήγησης** για να σας πίνακα διάταξης και στοιχείο ελέγχου και σύρετε για οικιακή χρήση και προβολή ελεγκτή ώστε να είναι η **Προβολή ριζικό κατάλογο** της περιήγησης. Βεβαιωθείτε ότι είναι επιλεγμένο το **Είναι ελεγκτή αρχική προβολή** στην επιθεώρηση χαρακτηριστικά του ελεγκτή περιήγησης μόνο.

5. Προσθήκη ενός **Ελεγκτή προβολή** σε πίνακα διάταξης και να προσθέσετε μια **Εικόνα προβολής**. Αυτή είναι η σελίδα θα βλέπουν οι χρήστες όταν έχουν επιλέξει για να μάθετε περισσότερα, κάνοντας κλικ στην το notifiication. Σας πίνακα διάταξης πρέπει να μοιάζει ως εξής:

    ![][IOS4]

6. Κάντε κλικ στην επιλογή στον **Κεντρική προβολή ελεγκτή** σε πίνακα διάταξης και βεβαιωθείτε ότι έχει **homeViewController** ως την **Προσαρμοσμένη τάξης** και **Αναγνωριστικό πίνακα διάταξης** κάτω από την επιθεώρηση ταυτότητα.

7. Κάντε το ίδιο για ελεγκτή Προβολή εικόνας ως **imageViewController**.

8. Στη συνέχεια, δημιουργήστε μια νέα προβολή ελεγκτή κλάση με τίτλο **imageViewController** να χειρίζεται το περιβάλλον εργασίας Χρήστη που μόλις δημιουργήσατε.

9. Στο **imageViewController.h**, προσθέστε τα ακόλουθα δηλώσεις περιβάλλοντος εργασίας του ελεγκτή. Βεβαιωθείτε ότι στο στοιχείο ελέγχου και σύρετε από την προβολή της εικόνας πίνακα διάταξης σε αυτές τις ιδιότητες για να συνδέσετε δύο:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Στο **imageViewController.m**, προσθέστε τα ακόλουθα στο τέλος της **viewDidload**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. Στο **AppDelegate.m**, εισαγάγετε τον ελεγκτή εικόνας που δημιουργήσατε:

        #import "imageViewController.h"

12. Προσθέστε μια ενότητα περιβάλλοντος εργασίας με την ακόλουθη δήλωση:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. Στο **AppDelegate**, βεβαιωθείτε ότι την εφαρμογή σας καταχωρήσει για σιωπηρή ειδοποιήσεις στο **εφαρμογής: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Αντικαταστήστε την παρακάτω εφαρμογή για **εφαρμογή: didRegisterForRemoteNotificationsWithDeviceToken** να πίνακα διάταξης περιβάλλοντος εργασίας Χρήστη αλλάζει σε λογαριασμό του:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Στη συνέχεια, προσθέστε τις ακόλουθες μεθόδους για να **AppDelegate.m** για να ανακτήσετε την εικόνα από το τελικό σημείο και να στείλετε μια τοπική ειδοποίηση όταν ολοκληρωθεί η ανάκτηση. Βεβαιωθείτε ότι για να αντικαταστήσετε το σύμβολο κράτησης θέσης `{backend endpoint}` με το τελικό σημείο υποστήριξης:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Χειρισμός την τοπική ειδοποίηση παραπάνω με το άνοιγμα προς τα επάνω στον ελεγκτή Προβολή εικόνας στο **AppDelegate.m** με τις παρακάτω μεθόδους:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

1. Στο XCode, εκτελέστε την εφαρμογή σε μια συσκευή φυσικής iOS (πάτημα ειδοποιήσεις δεν θα λειτουργούν σε το simulator).

2. Στην εφαρμογή iOS περιβάλλοντος εργασίας Χρήστη, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης για την ίδια τιμή για τον έλεγχο ταυτότητας και κάντε κλικ στην επιλογή **Log In**.

3. Κάντε κλικ στην επιλογή **Αποστολή push** και θα πρέπει να δείτε μια ειδοποίηση της εφαρμογής. Εάν κάνετε κλικ σε **περισσότερες**, θα εισαγάγετε την εικόνα που επιλέξατε να συμπεριλάβετε στο σας εφαρμογή παρασκηνίου.

4. Μπορείτε επίσης να επιλέξετε **Αποστολή push** και αμέσως, πατήστε το κουμπί αρχικής σελίδας της συσκευής σας. Σε μερικά λεπτά, θα λάβετε μια ειδοποίηση push. Εάν το πατήστε ή κάντε κλικ στην επιλογή περισσότερα, θα εισαγάγετε για την εφαρμογή σας και το περιεχόμενο εμπλουτισμένου εικόνας.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png

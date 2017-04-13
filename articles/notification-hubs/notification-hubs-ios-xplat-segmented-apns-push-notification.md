<properties
    pageTitle="Ειδοποίηση διανομείς πρόσφατες ειδήσεις πρόγραμμα εκμάθησης - iOS"
    description="Μάθετε πώς να χρησιμοποιείτε Azure Service Bus ειδοποίηση διανομείς για την αποστολή ειδοποιήσεων πρόσφατες ειδήσεις για συσκευές iOS."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε διανομείς ειδοποίηση Azure μετάδοση πρόσφατες ειδήσεις ειδοποιήσεων για μια εφαρμογή iOS. Όταν ολοκληρωθεί η εγκατάσταση, θα έχετε τη δυνατότητα να εγγραφείτε για διακοπή κατηγορίες συζητήσεων που σας ενδιαφέρει και λήψη μόνο ειδοποιήσεων push για αυτές τις κατηγορίες. Αυτό το σενάριο είναι ένα κοινό μοτίβο για πολλές εφαρμογές όπου έχουν ειδοποιήσεις θα αποσταλούν σε ομάδες χρηστών που έχουν προηγουμένως δηλωθεί ως ενδιαφέροντος σε αυτά, π.χ., πρόγραμμα ανάγνωσης RSS, εφαρμογές για το ανεμιστήρες μουσικής, κ.λπ.

Εκπομπή σενάρια είναι ενεργοποιημένα, συμπεριλαμβάνοντας μία ή περισσότερες _ετικέτες_ κατά τη δημιουργία μιας εγγραφής στην ενότητα ειδοποίησης. Όταν μια ετικέτα αποστέλλονται ειδοποιήσεις, όλες τις συσκευές που έχετε καταχωρήσει για την ετικέτα θα λάβετε την ειδοποίηση. Επειδή οι ετικέτες είναι απλώς συμβολοσειρές, δεν χρειάζεται να γίνει παροχή εκ των προτέρων. Για περισσότερες πληροφορίες σχετικά με τις ετικέτες, ανατρέξτε [ειδοποίηση διανομείς δρομολόγηση και παραστάσεις ετικέτα](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το θέμα δημιουργεί στην εφαρμογή του που δημιουργήσατε [γρήγορα]αποτελέσματα με το διανομείς ειδοποίηση[get-started]. Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ήδη ολοκληρώσει [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση][get-started].

##<a name="add-category-selection-to-the-app"></a>Προσθήκη επιλογή κατηγορίας στην εφαρμογή

Το πρώτο βήμα είναι να προσθέσετε τα στοιχεία του περιβάλλοντος εργασίας Χρήστη για να τον υπάρχοντα πίνακα διάταξης που επιτρέπει στο χρήστη να επιλέξει κατηγορίες για την καταχώρηση. Οι κατηγορίες που επιλέγονται από ένα χρήστη είναι αποθηκευμένα στη συσκευή. Κατά την εκκίνηση της εφαρμογής, μια καταχώρηση της συσκευής έχει δημιουργηθεί στο κέντρο σας ειδοποίηση με επιλεγμένες κατηγορίες ως ετικέτες.

1. Στο MainStoryboard_iPhone.storyboard σας, προσθέστε τα ακόλουθα στοιχεία από τη βιβλιοθήκη του αντικειμένου:
    + Μια ετικέτα με το κείμενο "Διακοπή ειδήσεων",
    + Ετικέτες με κειμένων κατηγορία "Διεθνείς", "Πολιτική", "Επιχειρήσεις", "Τεχνολογίας", "Φυσικής", "Sports",
    + Έξι διακόπτες, μία ανά κατηγορία, ορίστε το διακόπτη κάθε **κατάσταση** ώστε να είναι **απενεργοποιημένη** από προεπιλογή.
    + Ένα κουμπί με την ετικέτα "Εγγραφή"

    Σας πίνακα διάταξης πρέπει να μοιάζει ως εξής:

    ![][3]

2. Στο πρόγραμμα επεξεργασίας Βοηθού, δημιουργία πριζών για όλους τους διακόπτες και τους καλέσει "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Δημιουργήστε μια ενέργεια για το κουμπί που ονομάζεται "Εγγραφή". ViewController.h σας θα πρέπει να περιλαμβάνουν τα εξής:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Δημιουργία νέας **Κατηγορίας αφής κακάο** που ονομάζεται `Notifications`. Αντιγράψτε τον παρακάτω κώδικα στην ενότητα περιβάλλον εργασίας του αρχείου Notifications.h:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Προσθέστε την ακόλουθη οδηγία εισαγωγής Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Αντιγράψτε τον παρακάτω κώδικα στην ενότητα υλοποίησης του αρχείου Notifications.m.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Αυτή η κλάση χρησιμοποιεί τοπική αποθήκευση για αποθήκευση και ανάκτηση κατηγορίες των συζητήσεων που θα λάβετε αυτήν τη συσκευή. Επίσης, περιέχει μια μέθοδο για να εγγραφείτε για αυτές τις κατηγορίες, χρησιμοποιώντας ένα [πρότυπο](notification-hubs-templates-cross-platform-push-messages.md) εγγραφής.

7. Στο αρχείο AppDelegate.h, προσθέστε μια εντολή εισαγωγής για Notifications.h και προσθέστε μια ιδιότητα για μια παρουσία της κλάσης ειδοποιήσεις:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. Στη μέθοδο **didFinishLaunchingWithOptions** στο AppDelegate.m, προσθέστε τον κώδικα για την προετοιμασία την παρουσία ειδοποιήσεις στην αρχή της μεθόδου.  
 
    `HUBNAME`και `HUBLISTENACCESS` (ορίζονται στο hubinfo.h) θα πρέπει να έχετε ήδη το `<hub name>` και `<connection string with listen access>` σύμβολα κράτησης θέσης αντικαθίσταται με το όνομα διανομέας ειδοποίησης και τη συμβολοσειρά σύνδεσης για *DefaultListenSharedAccessSignature* που λάβατε νωρίτερα

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Επειδή τα διαπιστευτήρια που διανέμονται με μια εφαρμογή προγράμματος-πελάτη δεν είναι γενικά ασφαλής, πρέπει να διανείμετε μόνο το κλειδί για ακρόαση πρόσβαση με την εφαρμογή υπολογιστή-πελάτη σας. Ακούστε access παρέχει τη δυνατότητα να εγγραφείτε για τις ειδοποιήσεις, αλλά υπάρχουσες καταχωρήσεις εφαρμογή σας δεν μπορεί να τροποποιηθεί και δεν είναι δυνατό να αποστέλλονται ειδοποιήσεις. Χρησιμοποιείται το κλειδί πλήρη πρόσβαση σε μια υπηρεσία ασφαλούς υποστήριξης για την αποστολή ειδοποιήσεων και αλλαγή υπάρχουσες καταχωρήσεις.


9. Στη μέθοδο **didRegisterForRemoteNotificationsWithDeviceToken** στο AppDelegate.m, αντικαταστήστε τον κώδικα στη μέθοδο με τον παρακάτω κώδικα για τη μεταβίβαση το διακριτικό συσκευή για να την κλάση ειδοποιήσεις. Τάξη ειδοποιήσεις θα εκτελέσει την καταχώρηση για ειδοποιήσεις με τις κατηγορίες. Εάν ο χρήστης αλλάζει τις επιλογές κατηγορία, ονομάζουμε το `subscribeWithCategories` μέθοδο απάντηση στο κουμπί **εγγραφή** για να ενημερώσετε τους.

    > [AZURE.NOTE] Επειδή το διακριτικό συσκευή που έχουν εκχωρηθεί από το Apple Push ειδοποίηση υπηρεσίας (APNS) μπορεί να ευκαιρία ανά πάσα στιγμή, πρέπει να δηλώσετε για τις ειδοποιήσεις συχνά για να αποφύγετε την ειδοποίηση αποτυχίες. Αυτό το παράδειγμα καταχωρεί για ειδοποίηση κάθε φορά που ξεκινά η εφαρμογή. Για τις εφαρμογές που εκτελούνται συχνά, περισσότερες από μία φορά την ημέρα, μπορείτε να πιθανώς παραλείψετε εγγραφής για να διατηρήσετε το εύρος ζώνης, εάν έχει περάσει μικρότερη από την ημέρα από την προηγούμενη εγγραφή.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Σημειώστε ότι σε αυτό το σημείο θα πρέπει να υπάρχει κανένα άλλο κώδικα στη μέθοδο **didRegisterForRemoteNotificationsWithDeviceToken** .

10. Τις παρακάτω μεθόδους πρέπει ήδη να υπάρχουν σε AppDelegate.m από την ολοκλήρωση το παράθυρο [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση] [ get-started] το πρόγραμμα εκμάθησης.  Εάν δεν είναι, προσθέστε τους.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Αυτή η μέθοδος χειρισμού των ειδοποιήσεων που ελήφθησαν κατά την εκτέλεση της εφαρμογής, εμφανίζοντας μια απλή **UIAlert**.

11. Στο ViewController.m, προσθέστε μια δήλωση εισαγωγής για AppDelegate.h και αντιγράψτε τον παρακάτω κώδικα τη μέθοδο που δημιουργούνται από το XCode **εγγραφή** . Αυτός ο κώδικας θα ενημερωθεί η εγγραφή της ειδοποίησης για να χρησιμοποιήσετε τα νέα κατηγορία tag ο χρήστης έχει επιλέξει στο περιβάλλον εργασίας χρήστη.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Αυτή η μέθοδος δημιουργεί μια **NSMutableArray** κατηγοριών και χρησιμοποιεί την κλάση **ειδοποιήσεις** για την αποθήκευση της λίστας στο χώρο αποθήκευσης της τοπικής και καταγράφει τις αντίστοιχες ετικέτες με το Κέντρο ειδοποίηση σας. Όταν αλλάζουν οι κατηγορίες, η καταχώρηση αναδημιουργείται με τις νέες κατηγορίες.

12. Στο ViewController.m, προσθέστε τον ακόλουθο κώδικα στη μέθοδο **viewDidLoad** για να ορίσετε το περιβάλλον εργασίας χρήστη, με βάση τις κατηγορίες που είχε αποθηκευτεί προηγουμένως.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Η εφαρμογή τώρα να αποθηκεύσετε ένα σύνολο κατηγορίες σε την τοπική αποθήκευση συσκευή χρησιμοποιείται για την καταχώρηση με την ενότητα "Ειδοποίηση" κάθε φορά που ξεκινά η εφαρμογή.  Ο χρήστης μπορεί να αλλάξετε την επιλογή των κατηγοριών κατά το χρόνο εκτέλεσης και κάντε κλικ στη μέθοδο **εγγραφή** για να ενημερώσετε την εγγραφή για τη συσκευή. Στη συνέχεια, θα μπορείτε να ενημερώσετε την εφαρμογή για να στείλετε τις ειδοποιήσεις πρόσφατες ειδήσεις απευθείας στην ίδια την εφαρμογή.


##<a name="optional-sending-tagged-notifications"></a>(προαιρετικό) Αποστολή ειδοποιήσεων με ετικέτες

Εάν δεν έχετε πρόσβαση στο Visual Studio, μπορείτε να προχωρήσετε στην επόμενη ενότητα και αποστολή ειδοποιήσεων από την ίδια την εφαρμογή. Μπορείτε επίσης να στείλετε την ειδοποίηση proper πρότυπο από την [Πύλη κλασική Azure] χρησιμοποιώντας την καρτέλα εντοπισμού σφαλμάτων για το Κέντρο ειδοποίηση σας. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(προαιρετικό) Αποστολή ειδοποιήσεων από τη συσκευή

Κανονικά ειδοποιήσεις θα αποσταλούν από μια υπηρεσία υποστήριξης, αλλά, μπορείτε να στείλετε πρόσφατες ειδήσεις ειδοποιήσεις απευθείας από την εφαρμογή. Για να το κάνετε αυτό θα ενημερώσουμε το `SendNotificationRESTAPI` μέθοδο που ορίσαμε τα [Γρήγορα αποτελέσματα με διανομείς ειδοποίηση] [ get-started] το πρόγραμμα εκμάθησης.


1. Στο ViewController.m update το `SendNotificationRESTAPI` μέθοδο ως ακολουθεί ώστε να αποδέχεται μια παράμετρο για την ετικέτα κατηγορίας και στέλνει την ειδοποίηση proper [πρότυπο](notification-hubs-templates-cross-platform-push-messages.md) .

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

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



2. Σε ViewController.m ενημερώσετε την ενέργεια **Αποστολή ειδοποίησης** , όπως φαίνεται στον κώδικα που ακολουθεί. Έτσι ώστε να θα στείλετε τις ειδοποιήσεις χρησιμοποιώντας μεμονωμένα κάθε ετικέτα και να στείλετε σε πολλές πλατφόρμες.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Δημιουργήστε ξανά το έργο σας και βεβαιωθείτε ότι έχετε επιλέξει χωρίς σφάλματα έκδοσης.


##<a name="run-the-app-and-generate-notifications"></a>Εκτελέστε την εφαρμογή και Δημιουργία ειδοποιήσεων

1. Πατήστε το κουμπί "Εκτέλεση" για τη δημιουργία του έργου και να ξεκινήσετε την εφαρμογή. Επιλέξτε ορισμένες επιλογές πρόσφατες ειδήσεις για να εγγραφείτε σε και, στη συνέχεια, πατήστε το κουμπί **εγγραφή** . Θα πρέπει να βλέπετε ένα παράθυρο διαλόγου που υποδεικνύει ότι έχουν εγγραφεί οι ειδοποιήσεις.

    ![][1]

    Όταν επιλέγετε **εγγραφή**, η εφαρμογή μετατρέπει επιλεγμένες κατηγορίες σε ετικέτες και τις αιτήσεις μια νέα καταχώρηση της συσκευής για το επιλεγμένο ετικέτες από την ενότητα "Ειδοποίηση".

2. Πληκτρολογήστε ένα μήνυμα που θα σταλεί ως πρόσφατες ειδήσεις και, στη συνέχεια, πατήστε το κουμπί **Αποστολή ειδοποίησης** . Εναλλακτικά, εκτελέστε την εφαρμογή κονσόλας .NET για τη δημιουργία ειδοποιήσεων.

    ![][2]


3. Κάθε συσκευή έχετε εγγραφεί στο διακοπή συζητήσεων θα λάβετε τις ειδοποιήσεις ειδήσεις πρόσφατες που στείλατε.



## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το πρόγραμμα εκμάθησης, θα σας μάθατε τον τρόπο εκπομπής πρόσφατες ειδήσεις ανά κατηγορία. Εξετάστε την ολοκλήρωση ένα από τα παρακάτω προγράμματα εκμάθησης που επισημαίνουν άλλα σενάρια διανομείς ειδοποίησης για προχωρημένους:

+ **[Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]**

    Μάθετε πώς μπορείτε να αναπτύξετε την εφαρμογή πρόσφατες ειδήσεις για να ενεργοποιήσετε την αποστολή ειδοποιήσεων μεταφρασμένα.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Χρήση ειδοποιήσεων διανομείς μετάδοση μεταφρασμένα πρόσφατες ειδήσεις]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure κλασική πύλη]: https://manage.windowsazure.com

<properties
    pageTitle="Αποστολή ειδοποιήσεων push για iOS με διανομείς ειδοποίηση Azure | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να μάθετε πώς να χρησιμοποιείτε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή του iOS."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="ειδοποίηση, τις ειδοποιήσεις push, Push ios ειδοποιήσεις push"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Αποστολή ειδοποιήσεων push για iOS με διανομείς ειδοποίηση Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Επισκόπηση

> [AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε ένα λογαριασμό Azure active. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να χρησιμοποιήσετε Azure διανομείς ειδοποίηση για την αποστολή ειδοποιήσεων push σε μια εφαρμογή του iOS. Θα μπορείτε να δημιουργήσετε μια εφαρμογή κενό iOS που λαμβάνει τις ειδοποιήσεις push, χρησιμοποιώντας την [υπηρεσία ειδοποιήσεων Push Apple (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Όταν ολοκληρώσετε την εργασία, θα έχετε τη δυνατότητα να χρησιμοποιήσετε το Κέντρο ειδοποίηση σας μετάδοση ειδοποιήσεων push σε όλες τις συσκευές που εκτελεί την εφαρμογή σας.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Μπορείτε να βρείτε την ολοκληρωμένη κώδικα για αυτό το πρόγραμμα εκμάθησης [στην GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτό το πρόγραμμα εκμάθησης απαιτεί τα εξής:

+ [Υπηρεσίες Mobile iOS έκδοση 1.2.4 SDK]
+ Πιο πρόσφατη έκδοση του [Xcode]
+ Μια iOS 8 (ή νεότερη έκδοση)-συσκευή με δυνατότητα
+ Ιδιότητα μέλους [Apple προγραμματιστής πρόγραμμα](https://developer.apple.com/programs/) .

   > [AZURE.NOTE] Λόγω απαιτήσεις ρύθμισης παραμέτρων για τις ειδοποιήσεις push, μπορείτε πρέπει να αναπτύξετε και να ελέγξετε τις ειδοποιήσεις push σε μια συσκευή φυσικής iOS (iPhone ή iPad) αντί για το iOS Simulator.

Ολοκλήρωση αυτού του προγράμματος εκμάθησης αποτελεί προϋπόθεση για όλους άλλα προγράμματα εκμάθησης διανομείς ειδοποίησης για τις εφαρμογές του iOS.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Ρύθμιση παραμέτρων του διανομέα ειδοποίησης για iOS ειδοποιήσεις push

Αυτή η ενότητα σας καθοδηγεί στις τη δημιουργία διανομέα νέα ειδοποίηση και ρύθμιση παραμέτρων ελέγχου ταυτότητας με APNS χρησιμοποιώντας το πιστοποιητικό push **.p12** που δημιουργήσατε. Εάν θέλετε να χρησιμοποιήσετε ένα διανομέα ειδοποίησης που έχετε ήδη δημιουργήσει, μπορείτε να μεταβείτε στο βήμα 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p>Κάντε κλικ στο κουμπί <b>Ειδοποίησης υπηρεσίες</b> στο το blade <b>Ρυθμίσεις</b> και, στη συνέχεια, επιλέξτε <b>Apple (APNS)</b>. Κάντε κλικ στην εντολή <b>Αποστολή πιστοποιητικό</b> και επιλέξτε το αρχείο <b>.p12</b> που εξαγάγατε νωρίτερα. Βεβαιωθείτε ότι μπορείτε επίσης να καθορίσετε τον σωστό κωδικό πρόσβασης.</p>
<p>Βεβαιωθείτε ότι έχετε επιλέξει τη λειτουργία <b>Sandbox</b> , επειδή πρόκειται για ανάπτυξη. Χρησιμοποιήστε την <b>παραγωγή</b> μόνο εάν θέλετε να στείλετε τις ειδοποιήσεις push στους χρήστες που έχουν αγοράσει την εφαρμογή από το χώρο αποθήκευσης.</p>
</li>
</ol>
&emsp;&emsp;![Ρύθμιση παραμέτρων APNS στην πύλη του Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Ρύθμιση παραμέτρων έκδοσης πιστοποιητικών APNS στην πύλη Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Το Κέντρο ειδοποίηση σας τώρα έχει ρυθμιστεί ώστε να λειτουργεί με APNS και έχετε τις συμβολοσειρές σύνδεσης για να καταχωρήσετε την εφαρμογή και αποστολή ειδοποιήσεων push.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Σύνδεση σας εφαρμογή iOS με διανομείς ειδοποίησης

1. Στο Xcode, δημιουργήστε ένα νέο έργο iOS και επιλέξτε το πρότυπο **Μία μόνο εφαρμογή προβολής** .

    ![Xcode - μία και μόνο Προβολή εφαρμογής][8]

2. Κατά τη ρύθμιση των επιλογών για το νέο έργο σας, βεβαιωθείτε ότι χρησιμοποιείτε το ίδιο **Όνομα προϊόντος** και **Αναγνωριστικό εταιρείας** που χρησιμοποιήσατε κατά τη ρύθμιση προηγουμένως το Αναγνωριστικό πακέτου στην πύλη Apple προγραμματιστής.

    ![Xcode - επιλογές του project][11]

3. Στην περιοχή **των προορισμών**, κάντε κλικ στο όνομα του έργου σας, κάντε κλικ στην καρτέλα **Δημιουργία ρυθμίσεις** και ανάπτυξη **Κώδικα είσοδο ταυτότητα**και, στη συνέχεια, στην περιοχή **Εντοπισμός σφαλμάτων**, ορίστε την ταυτότητά σας υπογραφής κώδικα. Εναλλαγή **επίπεδα** από το **βασικό** **όλων**και ορίστε **Προμήθεια προφίλ** στο προφίλ παροχής που δημιουργήσατε προηγουμένως.

    Εάν δεν βλέπετε το νέο προφίλ παροχής που δημιουργήσατε στο Xcode, δοκιμάστε να ανανεώσετε τα προφίλ για την ταυτότητά σας υπογραφής. Κάντε κλικ στην επιλογή **Xcode** στη γραμμή μενού, κάντε κλικ στην επιλογή **Προτιμήσεις**, κάντε κλικ στην καρτέλα **λογαριασμός** , κάντε κλικ στο κουμπί **Προβολή λεπτομερειών** , κάντε κλικ στην επιλογή υπογραφής την ταυτότητά σας και, στη συνέχεια, κάντε κλικ στο κουμπί Ανανέωση στην κάτω δεξιά γωνία.

    ![Xcode - παροχής προφίλ][9]

4. Κάντε λήψη του [iOS υπηρεσίες Mobile SDK έκδοση 1.2.4] και αποσυμπιέστε το αρχείο. Στο Xcode, κάντε δεξιό κλικ στο έργο σας και κάντε κλικ στην επιλογή **Προσθήκη αρχείων για** να προσθέσετε το φάκελο **WindowsAzureMessaging.framework** στο έργο σας Xcode. Επιλέξτε **Αντιγραφή στοιχείων, εάν είναι απαραίτητο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη**.

    >[AZURE.NOTE] Οι διανομείς ειδοποίηση SDK δεν υποστηρίζει αυτήν τη στιγμή bitcode σε Xcode 7.  Πρέπει να ορίσετε **Ενεργοποίηση Bitcode** σε **όχι** στο παράθυρο **Δημιουργία επιλογές** για το έργο σας.

    ![Αποσυμπίεση του Azure SDK][10]

5. Προσθέστε ένα νέο αρχείο κεφαλίδας στο έργο σας με το όνομα `HubInfo.h`. Αυτό το αρχείο θα περιέχει τις σταθερές για το Κέντρο ειδοποίηση σας.  Προσθέστε τους ακόλουθους ορισμούς και αντικαταστήστε τα σύμβολα κράτησης θέσης λεκτική σταθερά συμβολοσειράς με το *όνομα διανομέας* και *DefaultListenSharedAccessSignature* που σημειώσατε προηγουμένως.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Άνοιγμα του `AppDelegate.h` αρχείου, προσθέστε τις ακόλουθες οδηγίες εισαγωγής:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Στο σας `AppDelegate.m file`, προσθέστε τον ακόλουθο κώδικα στο το `didFinishLaunchingWithOptions` μέθοδο ανάλογα με την έκδοση του iOS. Αυτός ο κωδικός καταχωρεί σας λαβή συσκευή με APNs:

    Για iOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    Για τις εκδόσεις iOS πριν από 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Στο ίδιο αρχείο, προσθέστε τις ακόλουθες μεθόδους. Αυτός ο κωδικός συνδέεται με το διανομέα ειδοποιήσεων, χρησιμοποιώντας τις πληροφορίες σύνδεσης που καθορίσατε στο HubInfo.h. Στη συνέχεια, σας δίνει το διακριτικό συσκευή για την ενότητα "Ειδοποίηση", έτσι ώστε να την ενότητα "Ειδοποίηση" μπορεί να στείλει ειδοποιήσεις:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Στο ίδιο αρχείο, προσθέστε την ακόλουθη μέθοδο για να εμφανίσετε μια **UIAlert** εάν έχει λάβει την ειδοποίηση ενώ η εφαρμογή είναι ενεργή:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Δημιουργήστε και εκτελέστε την εφαρμογή στη συσκευή σας για να βεβαιωθείτε ότι δεν υπάρχει αποτυχίες.

## <a name="send-test-push-notifications"></a>Αποστολή ειδοποιήσεων push δοκιμής


Μπορείτε να δοκιμάσετε να λαμβάνω ειδοποιήσεις στην εφαρμογή σας με την αποστολή ειδοποιήσεων push στην [Πύλη του Azure] μέσω στην ενότητα **Αντιμετώπιση προβλημάτων** σε το blade διανομέα (χρησιμοποιήστε την επιλογή *Δοκιμή αποστολή* ).

![Azure πύλης - αποστολή δοκιμής][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Προαιρετικό) Αποστολή ειδοποιήσεων push από την εφαρμογή

>[AZURE.IMPORTANT] Αυτό το παράδειγμα αποστολής ειδοποιήσεων από την εφαρμογή υπολογιστή-πελάτη παρέχονται για την εκμάθηση μόνο. Επειδή αυτό θα απαιτήσει την `DefaultFullSharedAccessSignature` για να παρουσιάσετε στην εφαρμογή του προγράμματος-πελάτη, εκθέτει το Κέντρο ειδοποίηση σας για να τον κίνδυνο ότι ένας χρήστης μπορεί να αποκτήσει πρόσβαση για την αποστολή ειδοποιήσεων μη εξουσιοδοτημένη για τους πελάτες σας.

Εάν θέλετε να στείλετε τις ειδοποιήσεις push από μέσα σε μια εφαρμογή, αυτή η ενότητα παρέχει ένα παράδειγμα του τρόπου να το κάνετε αυτό χρησιμοποιώντας το περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ.

1. Στο Xcode, ανοίξτε `Main.storyboard` και προσθέστε τα εξής στοιχεία περιβάλλοντος εργασίας Χρήστη από τη βιβλιοθήκη του αντικειμένου για να επιτρέψετε στο χρήστη για την αποστολή ειδοποιήσεων push στην εφαρμογή:

    - Μια ετικέτα με χωρίς κείμενο της ετικέτας. Θα χρησιμοποιηθεί για να αναφέρετε σφάλματα κατά την αποστολή ειδοποιήσεων. Η ιδιότητα **γραμμές** θα πρέπει να έχει οριστεί σε **0** έτσι ώστε το θα αυτόματα μέγεθος περιορίζεται σε στα δεξιά και αριστερά περιθώρια και το επάνω μέρος της προβολής.
    - Ορίστε ένα πεδίο κειμένου με κείμενο **κράτησης θέσης** για να **Πληκτρολογήστε το μήνυμα ειδοποίησης**. Περιορίσετε το πεδίο ακριβώς κάτω από την ετικέτα, όπως φαίνεται παρακάτω. Ορισμός του ελεγκτή προβολή ως πληρεξουσίου ρεύματος.
    - Ένα κουμπί με τίτλο **Αποστολή ειδοποίησης** περιορισμό ακριβώς κάτω από το πεδίο κειμένου και στο κέντρο οριζόντια.

    Η προβολή θα πρέπει να μοιάζει ως εξής:

    ![Xcode σχεδίασης][32]


2. [Προσθήκη πριζών](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) για το πεδίο ετικέτας και κείμενο συνδεδεμένοι προβολή σας, και να ενημερώσετε το `interface` ορισμό για την υποστήριξη `UITextFieldDelegate` και `NSXMLParserDelegate`. Προσθέστε τις τρεις ιδιότητα δηλώσεις φαίνεται παρακάτω για να υποστηρίξει καλεί το REST API και ανάλυση της απόκρισης.

    Το αρχείο ViewController.h πρέπει να μοιάζει ως εξής:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Άνοιγμα `HubInfo.h` και προσθέστε τις ακόλουθες σταθερές που θα χρησιμοποιηθεί για την αποστολή ειδοποιήσεων για το Κέντρο σας. Αντικαταστήστε το σύμβολο κράτησης θέσης συμβολοσειρά κειμένου με την πραγματική συμβολοσειρά σύνδεσης *DefaultFullSharedAccessSignature* .

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Προσθέστε τα ακόλουθα `#import` προτάσεις για να σας `ViewController.h` αρχείου.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. Στο `ViewController.m` προσθέστε τον ακόλουθο κώδικα για την εφαρμογή του περιβάλλοντος εργασίας. Αυτός ο κώδικας θα ανάλυση *DefaultFullSharedAccessSignature* συμβολοσειρά σύνδεσης. Όπως αναφέρθηκε στην [αναφορά REST API](http://msdn.microsoft.com/library/azure/dn495627.aspx), αυτές οι πληροφορίες αναλυμένη θα χρησιμοποιηθεί για τη δημιουργία ενός διακριτικού συσχετισμών ασφαλείας για την κεφαλίδα αίτησης **εξουσιοδότησης** .

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. Στο `ViewController.m`, ενημερώστε το `viewDidLoad` μέθοδο για την ανάλυση της συμβολοσειράς σύνδεσης κατά τη φόρτωση της προβολής. Επίσης, προσθέστε τις μεθόδους βοηθητικού προγράμματος, φαίνεται παρακάτω, για να την εφαρμογή περιβάλλοντος εργασίας.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. Στο `ViewController.m`, προσθέστε τον ακόλουθο κώδικα για την εφαρμογή περιβάλλοντος εργασίας για να δημιουργήσετε το διακριτικό εξουσιοδότησης συσχετισμών ασφαλείας που παρέχεται στην κεφαλίδα της **εξουσιοδότησης** , όπως αναφέρεται στην περιοχή [REST API αναφοράς](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + σύρετε από την **Αποστολή ειδοποιήσεων** κουμπί για να `ViewController.m` για να προσθέσετε μια ενέργεια με το όνομα **SendNotificationMessage** για το συμβάν **Αφής προς τα κάτω** . Ενημερώστε τη μέθοδο με τον ακόλουθο κώδικα για να στείλετε την ειδοποίηση χρησιμοποιώντας το REST API.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. Στο `ViewController.m`, προσθέστε την ακόλουθη μέθοδο πληρεξούσιο για την υποστήριξη Κλείσιμο του πληκτρολογίου για το πεδίο κειμένου. CTRL + σύρετε από το πεδίο κειμένου στο εικονίδιο ελεγκτή προβολής στη σχεδίαση περιβάλλοντος εργασίας για να ορίσετε τον ελεγκτή προβολή ως πληρεξουσίου ρεύματος.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. Στο `ViewController.m`, προσθέστε τις ακόλουθες μεθόδους πληρεξούσιο για την υποστήριξη κατά την ανάλυση της απόκρισης χρησιμοποιώντας `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Δημιουργήστε το έργο και βεβαιωθείτε ότι δεν υπάρχουν σφάλματα.


> [AZURE.NOTE] Εάν αντιμετωπίσετε ένα σφάλμα Δόμηση στην Xcode7 σχετικά με την υποστήριξη bitcode, θα πρέπει να αλλάξετε τις **Ρυθμίσεις Δόμηση** > **Ενεργοποίηση Bitcode (ENABLE_BITCODE)** σε **ΌΧΙ** στο Xcode. Το SDK διανομείς ειδοποίηση δεν υποστηρίζει αυτήν τη στιγμή bitcode. 

Μπορείτε να βρείτε όλων των πιθανών ειδοποίηση φορτίων στο Apple [τοπικών και Οδηγό προγραμματισμού ειδοποιήσεων Push].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Έλεγχος εάν η εφαρμογή σας να λαμβάνουν ειδοποιήσεις push

Για να ελέγξετε τις ειδοποιήσεις push σε iOS, πρέπει να αναπτύξετε την εφαρμογή σε μια συσκευή φυσικής iOS. Δεν μπορείτε να στείλετε τις ειδοποιήσεις push Apple με τη χρήση του iOS Simulator.

1. Εκτελέστε την εφαρμογή και να επαληθεύσετε ότι η δήλωση ολοκληρωθεί με επιτυχία, και, στη συνέχεια, πατήστε το κουμπί **OK**.

    ![iOS δοκιμής δήλωσης ειδοποιήσεων Push εφαρμογής][33]

2. Μπορείτε να στείλετε μια ειδοποίηση push δοκιμής από την [Πύλη Azure], όπως περιγράφεται παραπάνω. Αν έχετε προσθέσει κώδικα για την αποστολή ειδοποιήσεων push στην εφαρμογή, πατήστε μέσα στο πεδίο κειμένου για να εισαγάγετε ένα μήνυμα ειδοποίησης. Στη συνέχεια, πατήστε το κουμπί **Αποστολή** στο πληκτρολόγιο ή το κουμπί **Αποστολή ειδοποίησης** στην προβολή για να στείλετε το μήνυμα ειδοποίησης.

    ![iOS εφαρμογή Push αποστολή δοκιμή ειδοποίησης][34]

3. Η κοινοποίηση push αποστέλλεται σε όλες τις συσκευές που έχουν καταχωρηθεί να λαμβάνει τις ειδοποιήσεις από το συγκεκριμένο διανομέα ειδοποίησης.

    ![iOS εφαρμογή Push λήψη δοκιμή ειδοποίησης][35]


##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το απλό παράδειγμα μετάδοση ειδοποιήσεων push για όλες τις συσκευές σας καταχωρημένες iOS. Προτείνουμε ως επόμενο βήμα σας μάθετε που μπορείτε να προχωρήσετε στο το πρόγραμμα εκμάθησης [Azure ειδοποίηση διανομείς ειδοποιήστε τους χρήστες για iOS με το .NET παρασκηνίου] , που θα σας καθοδηγήσει τη δημιουργία ενός στοιχείου διακομιστή για την αποστολή ειδοποιήσεων push χρησιμοποιώντας ετικέτες. 

Εάν θέλετε να χωρίσετε τους χρήστες σας από τις ομάδες που σας ενδιαφέρουν, μπορείτε να μετακινήσετε επιπλέον για το πρόγραμμα εκμάθησης [Χρήση διανομείς ειδοποίησης για να στείλετε πρόσφατες ειδήσεις] . 

Για γενικές πληροφορίες σχετικά με την ειδοποίηση διανομείς, ανατρέξτε στο θέμα [Οδηγίες διανομείς ειδοποίησης].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Υπηρεσίες Mobile iOS έκδοση 1.2.4 SDK]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Ειδοποίηση διανομείς καθοδήγηση]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure ειδοποίηση διανομείς ειδοποιήστε τους χρήστες για iOS με το .NET παρασκηνίου]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Χρήση διανομείς ειδοποίηση για την αποστολή πρόσφατες ειδήσεις]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Τοπική και Οδηγός προγραμματισμού ειδοποιήσεων Push]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Πύλη του Azure]: https://portal.azure.com
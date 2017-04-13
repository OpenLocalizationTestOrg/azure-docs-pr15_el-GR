<properties
    pageTitle="Ειδοποίηση Azure διανομείς ασφαλούς Push"
    description="Μάθετε πώς μπορείτε να στείλετε τις ειδοποιήσεις push ασφαλούς σε μια εφαρμογή iOS από το Azure. Δείγματα κώδικα γραμμένο σε στόχο-C και C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Ειδοποίηση Azure διανομείς ασφαλούς Push

> [AZURE.SELECTOR]
- [Παγκόσμια των Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Επισκόπηση

Υποστήριξη ειδοποιήσεων Push στο Microsoft Azure επιτρέπει να αποκτήσετε πρόσβαση σε μια υποδομής εύκολο για χρήση, multiplatform, κλιμάκωση ανάληψης push που απλοποιεί σε μεγάλο βαθμό την εφαρμογή των ειδοποιήσεων push για καταναλωτές και για μεγάλες επιχειρήσεις στις εφαρμογές για κινητές συσκευές πλατφόρμες.

Λόγω ρυθμιστικές ή τους περιορισμούς ασφαλείας, μερικές φορές μια εφαρμογή μπορεί να θέλετε να συμπεριλάβετε κάτι στην ειδοποίηση που δεν είναι δυνατό να μεταδοθούν μέσω της υποδομής κοινοποίηση τυπική push. Αυτό το πρόγραμμα εκμάθησης περιγράφει τον τρόπο για να επιτύχετε το ίδιο εμπειρία με την αποστολή ευαίσθητες πληροφορίες μέσω ασφαλή, με έλεγχο ταυτότητας σύνδεσης μεταξύ της συσκευής προγράμματος-πελάτη και την εφαρμογή παρασκηνίου.

Σε υψηλό επίπεδο, η ροή είναι ως εξής:

1. Η εφαρμογή παρασκηνίου:
    - Ασφαλής φορτίο αποθηκεύονται στη βάση δεδομένων παρασκηνίου.
    - Στέλνει το Αναγνωριστικό του αυτή η ειδοποίηση στη συσκευή (καμία ασφαλούς πληροφορία δεν αποστέλλεται).
2. Η εφαρμογή από τη συσκευή, όταν λάβετε την ειδοποίηση:
    - Η συσκευή επαφών του παρασκηνίου που ζητά το φορτίο ασφαλή.
    - Η εφαρμογή μπορεί να εμφανίσει το φορτίο ως μια ειδοποίηση στη συσκευή.

Είναι σημαντικό να λάβετε υπόψη ότι το προηγούμενο ροής (και σε αυτό το πρόγραμμα εκμάθησης), θα σας λαμβάνεται ως δεδομένο ότι η συσκευή αποθηκεύει ένα διακριτικό ελέγχου ταυτότητας στον τοπικό χώρο αποθήκευσης, αφού ο χρήστης συνδέεται. Αυτό εγγυάται μια εντελώς απρόσκοπτη εμπειρία, όπως η συσκευή να ανακτήσετε την ειδοποίηση ασφαλούς φορτίο χρησιμοποιώντας αυτό το διακριτικό. Εάν η εφαρμογή σας δεν αποθηκεύει κωδικοί ελέγχου ταυτότητας στη συσκευή ή εάν αυτά τα διακριτικά μπορεί να έχει λήξει, την εφαρμογή συσκευή, μόλις λάβετε την ειδοποίηση πρέπει να εμφανίζει μια γενική ειδοποίηση ερώτηση στο χρήστη για να ξεκινήσετε την εφαρμογή. Η εφαρμογή πραγματοποιεί έλεγχο ταυτότητας χρήστη, στη συνέχεια, και εμφανίζει το φορτίο ειδοποίησης.

Αυτό το πρόγραμμα εκμάθησης ασφαλούς Push δείχνει πώς μπορείτε να στείλετε μια ειδοποίηση push με ασφάλεια. Το πρόγραμμα εκμάθησης δημιουργεί στην το πρόγραμμα εκμάθησης [Ειδοποιήστε τους χρήστες](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , έτσι θα πρέπει να ολοκληρώσετε τα βήματα που περιγράφονται σε αυτό το πρόγραμμα εκμάθησης πρώτα.

> [AZURE.NOTE] Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε δημιουργήσει και ρυθμιστεί το Κέντρο ειδοποίηση σας, όπως περιγράφεται σε [Γρήγορα αποτελέσματα με ειδοποίηση διανομείς (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Τροποποίηση του έργου iOS

Τώρα που τροποποιήσατε σας εφαρμογή παρασκηνίου για να στείλετε απλώς το *αναγνωριστικό* της ειδοποίησης, πρέπει να αλλάξετε την εφαρμογή iOS σας για να χειριστείτε αυτήν την ειδοποίηση και επιστροφή κλήσης σας παρασκηνίου για να ανακτήσετε το ασφαλές μήνυμα να εμφανίζεται.

Για την επίτευξη αυτού του στόχου, έχουμε για να γράψετε της λογικής για να ανακτήσετε τη διασφάλιση περιεχομένου από το app υποστήριξης.

1. Στο **AppDelegate.m**, βεβαιωθείτε ότι τα μητρώα εφαρμογής για σιωπηρή ειδοποιήσεις, ώστε να επεξεργάζεται το αναγνωριστικό ειδοποίηση που αποστέλλεται από υπολογιστή στο παρασκήνιο. Προσθέστε την επιλογή **UIRemoteNotificationTypeNewsstandContentAvailability** στο didFinishLaunchingWithOptions:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Στο σας **AppDelegate.m** , προσθέστε μια ενότητα εφαρμογής στο επάνω μέρος με την ακόλουθη δήλωση:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Προσθέστε, στη συνέχεια, στην ενότητα εφαρμογή τον ακόλουθο κώδικα, αντικαθιστώντας το σύμβολο κράτησης θέσης `{back-end endpoint}` με το τελικό σημείο για σας υποστήριξης που λαμβάνονται προηγουμένως:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Τώρα που έχετε να χειριστείτε την εισερχόμενη ειδοποίηση και να χρησιμοποιήσετε την παραπάνω μέθοδο για την ανάκτηση του περιεχομένου για να εμφανίσετε. Πρώτα, έχουμε για να ενεργοποιήσετε την εφαρμογή iOS να εκτελείται στο παρασκήνιο όταν λαμβάνετε μια ειδοποίηση push. Στο **XCode**, επιλέξτε το έργο σας εφαρμογή στο αριστερό τμήμα του παραθύρου και, στη συνέχεια, κάντε κλικ στην επιλογή σας κύριο εφαρμογής προορισμού στην ενότητα **προορισμών** στο κεντρικό παράθυρο.

5. Στη συνέχεια, κάντε κλικ την καρτέλα **δυνατότητες** στο επάνω μέρος στο κεντρικό παράθυρο και επιλέξτε το πλαίσιο ελέγχου **Απομακρυσμένο ειδοποιήσεις** .

    ![][IOS1]


6. Στο **AppDelegate.m** , προσθέστε την ακόλουθη μέθοδο για να χειριστείτε τις ειδοποιήσεις push:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Σημειώστε ότι είναι προτιμότερο να χειρίζεται τις περιπτώσεις δεν υπάρχει ιδιότητα κεφαλίδα ελέγχου ταυτότητας ή απόρριψη από το παρασκηνίου. Το συγκεκριμένο χειρισμό από αυτές τις περιπτώσεις εξαρτώνται από κυρίως την εμπειρία χρήστη προορισμού. Μία επιλογή είναι να εμφανίζεται μια ειδοποίηση με ένα γενικό μήνυμα για το χρήστη για τον έλεγχο ταυτότητας για να ανακτήσετε την πραγματική ειδοποίηση.

## <a name="run-the-application"></a>Εκτελέστε την εφαρμογή

Για να εκτελέσετε την εφαρμογή, κάντε τα εξής:

1. Στο XCode, εκτελέστε την εφαρμογή σε μια συσκευή φυσικής iOS (πάτημα ειδοποιήσεις δεν θα λειτουργούν σε το simulator).

2. Στην εφαρμογή iOS περιβάλλοντος εργασίας Χρήστη, πληκτρολογήστε ένα όνομα χρήστη και τον κωδικό πρόσβασης. Αυτά μπορεί να είναι οποιαδήποτε συμβολοσειρά, αλλά ο χρήστης πρέπει να έχει την ίδια τιμή.

3. Στην εφαρμογή iOS περιβάλλοντος εργασίας Χρήστη, κάντε κλικ στην επιλογή **σύνδεση στο**. Στη συνέχεια, κάντε κλικ στην επιλογή **Αποστολή push**. Θα πρέπει να δείτε την ασφαλή ειδοποίηση που εμφανίζεται στο κέντρο ειδοποίησης.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png

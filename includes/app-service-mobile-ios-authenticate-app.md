**Στόχος-C**: 

1. Στο Mac σας, ανοίξτε _QSTodoListViewController.m_ σε Xcode και προσθέστε την ακόλουθη μέθοδο. Αλλαγή _google_ σε _διαθέσιμος microsoftaccount_, _twitter_, _facebook_ή _windowsazureactivedirectory_ , εάν δεν χρησιμοποιείτε το Google ως υπηρεσία παροχής ταυτότητας. Εάν χρησιμοποιείτε το Facebook, [θα πρέπει να whitelist Facebook τομείς στην εφαρμογή](https://developers.facebook.com/docs/ios/ios9#whitelist).

            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
            
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }


2. Αντικατάσταση `[self refresh]` στο `viewDidLoad` σε _QSTodoListViewController.m_ με τα εξής:

            [self loginAndGetData];

3. Πατήστε το πλήκτρο _Εκτέλεση_ για να ξεκινήσετε την εφαρμογή και, στη συνέχεια, συνδεθείτε. Όταν είστε συνδεδεμένοι, θα πρέπει να μπορείτε να προβάλετε τη λίστα Todo και να κάνετε ενημερώσεις.

**Σουίφτ**:

1. Στο Mac σας, ανοίξτε _ToDoTableViewController.swift_ σε Xcode και προσθέστε την ακόλουθη μέθοδο. Αλλαγή _google_ σε _διαθέσιμος microsoftaccount_, _twitter_, _facebook_ή _windowsazureactivedirectory_ , εάν δεν χρησιμοποιείτε το Google ως υπηρεσία παροχής ταυτότητας. Εάν χρησιμοποιείτε το Facebook, [θα πρέπει να whitelist Facebook τομείς στην εφαρμογή](https://developers.facebook.com/docs/ios/ios9#whitelist).
        
            func loginAndGetData() {
                
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
                
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }


2. Καταργήστε τις γραμμές `self.refreshControl?.beginRefreshing()` και `self.onRefresh(self.refreshControl)` στο τέλος της `viewDidLoad()` στο _ToDoTableViewController.swift_. Προσθήκη μιας κλήσης για να `loginAndGetData()` στη θέση τους:

            loginAndGetData()

3. Πατήστε το πλήκτρο _Εκτέλεση_ για να ξεκινήσετε την εφαρμογή και, στη συνέχεια, συνδεθείτε. Όταν είστε συνδεδεμένοι, θα πρέπει να μπορείτε να προβάλετε τη λίστα Todo και να κάνετε ενημερώσεις.

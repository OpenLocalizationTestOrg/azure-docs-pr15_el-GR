<properties
    pageTitle="Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή Mobile Azure (iOS)"
    description="Μάθετε πώς να χρησιμοποιείτε εφαρμογών υπηρεσίας Mobile σε cache και συγχρονισμός δεδομένων χωρίς σύνδεση στην εφαρμογή iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή για κινητές συσκευές iOS

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης καλύπτει τη δυνατότητα συγχρονισμού χωρίς σύνδεση του Azure Mobile Apps για iOS. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου. Οι αλλαγές αποθηκεύονται σε μια τοπική βάση δεδομένων. Όταν η συσκευή είναι πάλι σε σύνδεση, αυτές οι αλλαγές συγχρονίζονται με το απομακρυσμένο υπολογιστή στο παρασκήνιο.

Εάν πρόκειται για την πρώτη εμπειρία με εφαρμογές του Mobile Azure, θα πρέπει να ολοκληρώσετε πρώτα το πρόγραμμα εκμάθησης, [Δημιουργήστε μια εφαρμογή iOS]. Εάν δεν χρησιμοποιείτε το έργο διακομιστή ληφθέντων γρήγορης εκκίνησης, πρέπει να προσθέσετε τα πακέτα επέκτασης πρόσβασης δεδομένων στο έργο σας. Για περισσότερες πληροφορίες σχετικά με τα πακέτα επέκτασης server, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Για να μάθετε περισσότερα σχετικά με τη δυνατότητα συγχρονισμού χωρίς σύνδεση, ανατρέξτε στο θέμα [Συγχρονισμού χωρίς σύνδεση δεδομένων σε εφαρμογές του Mobile Azure].

## <a name="review-sync"></a>Αναθεώρηση του κώδικα συγχρονισμού προγράμματος-πελάτη

Το έργο προγράμματος-πελάτη που έχετε λάβει ήδη για το πρόγραμμα εκμάθησης, [Δημιουργήστε μια εφαρμογή iOS] περιέχει κώδικα υποστήριξης, χρησιμοποιώντας μια τοπική βάση δεδομένων που βασίζεται σε βασικά δεδομένα συγχρονισμού εκτός σύνδεσης. Αυτή η ενότητα είναι μια σύνοψη των τι περιλαμβάνεται ήδη τον κώδικα προγραμμάτων εκμάθησης. Για μια επισκόπηση της δυνατότητας, ανατρέξτε στο θέμα [Δεδομένων συγχρονισμού χωρίς σύνδεση σε εφαρμογές του Mobile Azure].

Τη δυνατότητα συγχρονισμού συγχρονισμού χωρίς σύνδεση δεδομένων από εφαρμογές του Mobile Azure επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια τοπική βάση δεδομένων όταν δεν είναι δυνατή η πρόσβαση στο δίκτυο. Για να χρησιμοποιήσετε αυτές τις δυνατότητες της εφαρμογής σας, θα προετοιμάσετε το περιβάλλον συγχρονισμού της `MSClient` και αναφοράς ενός τοπικού χώρου αποθήκευσης. Στη συνέχεια, αναφορά πίνακα μέσω του `MSSyncTable` περιβάλλοντος εργασίας.

1. Στο **QSTodoService.m** (στόχο-C) ή **ToDoTableViewController.swift** (Σουίφτ), σημειώστε τον τύπο του μέλους `syncTable` είναι `MSSyncTable`. Αυτή η διασύνδεση πίνακα συγχρονισμού αντί χρησιμοποιεί συγχρονισμού χωρίς σύνδεση `MSTable`. Όταν χρησιμοποιείται ένας πίνακας συγχρονισμού, όλες οι λειτουργίες μεταβείτε στον τοπικό χώρο αποθήκευσης και συγχρονίζονται μόνο με το απομακρυσμένο υπολογιστή στο παρασκήνιο με ρητή λειτουργίες Ώθηση και έλξη.

    Για να λάβετε μια αναφορά σε έναν πίνακα συγχρονισμού, χρησιμοποιήστε τη μέθοδο `syncTableWithName` σε `MSClient`. Για να καταργήσετε λειτουργίες συγχρονισμού χωρίς σύνδεση, χρησιμοποιήστε `tableWithName` αντί για αυτό.

2. Πριν από τις λειτουργίες πίνακα μπορούν να πραγματοποιηθούν, πρέπει να προετοιμαστεί του τοπικού χώρου αποθήκευσης. Παρακάτω θα δείτε τον σχετικό κωδικό. 
    
    **Στόχος C**:
    
    Στο το `QSTodoService.init` μέθοδο:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Σουίφτ**:
    
    Στο το `ToDoTableViewController.viewDidLoad` μέθοδο:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Τον τρόπο αυτό δημιουργείται ένα τοπικό χώρο αποθήκευσης χρησιμοποιώντας το περιβάλλον εργασίας `MSCoreDataStore`, που παρέχονται στο SDK εφαρμογές του Mobile. Μπορείτε να κάνετε αντί για αυτό ένα παροχή μια διαφορετική τοπικού χώρου αποθήκευσης με την εφαρμογή του `MSSyncContextDataSource` πρωτόκολλο. 
    
    Επίσης, η πρώτη παράμετρος της `MSSyncContext` χρησιμοποιείται για να καθορίσετε ένα πρόγραμμα χειρισμού της διένεξης. Δεδομένου ότι σας έχουν περάσει `nil`, θα σας θα λάβουν το προεπιλεγμένο διένεξης πρόγραμμα χειρισμού, που αποτυγχάνει σε οποιαδήποτε διένεξη.
    
3. Τώρα, ας εκτέλεση της λειτουργίας πραγματική συγχρονισμού και λήψη δεδομένων από το απομακρυσμένο υπολογιστή στο παρασκήνιο.

    **Στόχος-C**:
    
    `syncData`πρώτα προωθεί νέες αλλαγές, στη συνέχεια, καλεί `pullData` για τη λήψη δεδομένων από το απομακρυσμένο υπολογιστή στο παρασκήνιο. Με τη σειρά, η μέθοδος `pullData` λαμβάνει νέα δεδομένα που ταιριάζουν με ένα ερώτημα:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Σουίφτ**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    Στην έκδοση στόχο-C, στο `syncData`, ονομάζουμε πρώτα `pushWithCompletion` σε περιβάλλον συγχρονισμού. Αυτή η μέθοδος είναι μέλος της `MSSyncContext` (και όχι τον συγχρονισμό πίνακα ίδια) επειδή αυτό θα push αλλαγές σε όλους τους πίνακες. Μόνο οι εγγραφές που έχουν τροποποιηθεί με κάποιον τρόπο τοπικά (μέσω λειτουργίες CUD) θα αποστέλλονται στο διακομιστή. Στη συνέχεια, ο Βοηθός `pullData` ονομάζεται, ποια κλήσεις `MSSyncTable.pullWithQuery` να ανακτήσετε απομακρυσμένα δεδομένα και να αποθηκεύσετε σε τοπική βάση δεδομένων.
    
    Στην έκδοση ταχείας, δεν υπάρχει καμία κλήση για να `pushWithCompletion`. Αυτό συμβαίνει επειδή η λειτουργία push δεν ήταν απολύτως απαραίτητο. Εάν υπάρχουν αλλαγές σε εκκρεμότητα στο περιβάλλον συγχρονισμού για τον πίνακα που κάνει μια λειτουργία push, ελκυστική πάντα θέματα πάτημα πρώτα. Ωστόσο, εάν έχετε περισσότερους από έναν πίνακες συγχρονισμού, είναι καλύτερα να καλέσετε ρητά push για να βεβαιωθείτε ότι όλα είναι συνεπείς σε σχετικούς πίνακες.
    
    Σε εκδόσεις τόσο το στόχο-C και Σουίφτ, η μέθοδος `pullWithQuery` σάς επιτρέπει να καθορίσετε ένα ερώτημα για να φιλτράρετε τις εγγραφές που θέλετε να ανακτήσετε. Σε αυτό το παράδειγμα, το ερώτημα ανακτά απλώς όλες τις εγγραφές ο απομακρυσμένος `TodoItem` πίνακα.
    
    Η δεύτερη παράμετρος να `pullWithQuery` είναι το Αναγνωριστικό ερωτήματος που χρησιμοποιείται για το *συγχρονισμό με αυξάνονται*. Τμηματική συγχρονισμού ανακτά μόνο τις καρτέλες που τροποποιήθηκαν από τον τελευταίο συγχρονισμό, χρησιμοποιώντας την καρτέλα `UpdatedAt` χρονικής σήμανσης (που ονομάζεται `updatedAt` στον τοπικό χώρο αποθήκευσης.) Το Αναγνωριστικό ερωτήματος πρέπει να είναι μια περιγραφική συμβολοσειρά που είναι μοναδικός για κάθε λογική ερώτημα στην εφαρμογή σας. Για να εξαίρεση αυξάνονται συγχρονισμού, μεταβίβαση `nil` με το αναγνωριστικό ερωτήματος. Σημειώστε ότι αυτό μπορεί να είναι ενδεχομένως αποτελεσματική, επειδή θα ανακτήσει όλες τις εγγραφές σε κάθε εργασία ελκυστική.

5. Το στόχο-C εφαρμογή συγχρονίζει όταν θα σας τροποποίηση ή προσθήκη δεδομένων, ένας χρήστης πραγματοποιεί την κίνηση ανανέωσης, και στην εκκίνηση. Η εφαρμογή ταχείας συγχρονίζει όταν ένας χρήστης πραγματοποιεί την κίνηση ανανέωση και στην εκκίνηση. 

Επειδή η εφαρμογή συγχρονίζεται κάθε φορά που τα δεδομένα είναι τροποποιηθεί (στόχο-C) ή κάθε φορά που ξεκινά η εφαρμογή (στόχο-C & Σουίφτ), η εφαρμογή προϋποθέτει ότι ο χρήστης είναι συνδεδεμένος. Σε μια άλλη ενότητα, θα ενημερώσουμε την εφαρμογή, έτσι ώστε οι χρήστες μπορούν να επεξεργαστούν ακόμα και όταν είναι χωρίς σύνδεση.

## <a name="review-core-data"></a>Αναθεώρηση του μοντέλου δεδομένων πυρήνα

Όταν χρησιμοποιείτε το χώρο αποθήκευσης βασικά δεδομένα χωρίς σύνδεση, πρέπει να ορίσετε συγκεκριμένο πινάκων και πεδίων στο μοντέλο δεδομένων. Η εφαρμογή δείγμα περιλαμβάνει ήδη ένα μοντέλο δεδομένων με τη σωστή μορφή. Σε αυτήν την ενότητα σας θα καθοδηγήσουμε μέσω σε αυτούς τους πίνακες και τον τρόπο που χρησιμοποιούνται.

- Άνοιγμα **QSDataModel.xcdatamodeld**. Υπάρχουν τέσσερις πίνακες που ορίζονται από το--τρία που χρησιμοποιούνται από το SDK, και έναν πίνακα για την todo στοιχεία οι ίδιοι:     *MS_TableOperations: για την παρακολούθηση των στοιχείων που πρέπει να συγχρονιστούν με το διακομιστή    * MS_TableOperationErrors: για παρακολούθηση της τυχόν σφάλματα που πραγματοποιούνται κατά το συγχρονισμό εκτός σύνδεσης     *MS_TableConfig: για παρακολούθηση την τελευταία ενημέρωση χρόνου για την τελευταία λειτουργία συγχρονισμού για όλες τις εργασίες ελκυστική    * TodoItem : Για την αποθήκευση των στοιχείων todo. Το σύστημα στήλες **createdAt** **updatedAt**και **έκδοση** είναι προαιρετικό σύστημα ιδιότητες.

>[AZURE.NOTE] Το SDK εφαρμογές του Mobile Azure διατηρεί τα ονόματα στηλών που γίνεται με "**``**". Δεν πρέπει να χρησιμοποιείτε αυτό το πρόθεμα σε όλα τα στοιχεία εκτός από το σύστημα στηλών, διαφορετικά επιστρέφει τα ονόματα στηλών θα τροποποιηθούν κατά τη χρήση του απομακρυσμένου υπολογιστή στο παρασκήνιο.

- Όταν χρησιμοποιείτε τη δυνατότητα συγχρονισμού χωρίς σύνδεση, πρέπει να ορίσετε τους πίνακες συστήματος όπως φαίνεται παρακάτω.

    ### <a name="system-tables"></a>Πίνακες συστήματος

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Χαρακτηριστικό  |    Τύπος     |
  	|----------- |   ------    |
  	| αναγνωριστικό         | Ακέραιο αριθμό 64  |
  	| αναγνωριστικό στοιχείου     | Συμβολοσειρά      |
  	| Ιδιότητες | Δυαδικά δεδομένα |
  	| Πίνακας      | Συμβολοσειρά      |
  	| tableKind  | Ακέραιος 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Χαρακτηριστικό  |    Τύπος     |
  	|----------- |   ------    |
  	| αναγνωριστικό         | Συμβολοσειρά      |
  	| operationId | Ακέραιο αριθμό 64 |
  	| Ιδιότητες | Δυαδικά δεδομένα |
  	| tableKind  | Ακέραιος 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Χαρακτηριστικό  |    Τύπος     |
  	|----------- |   ------    |
  	| αναγνωριστικό         | Συμβολοσειρά      |
  	| πλήκτρο        | Συμβολοσειρά      |
  	| τύπο κλειδιού    | Ακέραιο αριθμό 64  |
  	| Πίνακας      | Συμβολοσειρά      |
  	| τιμή      | Συμβολοσειρά      |

    ### <a name="data-table"></a>Πίνακας δεδομένων

    **TodoItem**

  	| Χαρακτηριστικό    |  Τύπος   | Σημείωση                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| αναγνωριστικό           | Συμβολοσειρά, επισημανθεί ως απαιτούμενη  | πρωτεύον κλειδί στον απομακρυσμένο χώρο αποθήκευσης                            |
  	| Ολοκλήρωση     | Δυαδική τιμή | πεδίο στοιχείου Todo                                        |
  	| κείμενο         | Συμβολοσειρά  | πεδίο στοιχείου Todo                                        |
  	| createdAt | Ημερομηνία    | (προαιρετικό) χάρτες createdAt ιδιότητα συστήματος         |
  	| updatedAt | Ημερομηνία    | (προαιρετικό) χάρτες updatedAt ιδιότητα συστήματος         |
  	| έκδοση   | Συμβολοσειρά  | (προαιρετικό) χρησιμοποιείται για τον εντοπισμό διενέξεις, χάρτες έκδοση |


## <a name="setup-sync"></a>Αλλαγή της συμπεριφοράς συγχρονισμού της εφαρμογής

Σε αυτήν την ενότητα, θα μπορείτε να τροποποιήσετε την εφαρμογή, έτσι ώστε να μην συγχρονιστούν κατά την έναρξη της εφαρμογής ή κατά την εισαγωγή και ενημέρωση των στοιχείων, αλλά μόνο όταν εκτελείται το κουμπί Ανανέωση χειρονομία.

**Στόχος C**:

1. Στο **QSTodoListViewController.m**, αλλάξτε τη μέθοδο **viewDidLoad** για να καταργήσετε την κλήση σε `[self refresh]` στο τέλος της μεθόδου. Τώρα, τα δεδομένα δεν θα συγχρονίζονται με το διακομιστή κατά την εκκίνηση εφαρμογών, αλλά αντί για αυτό, θα είναι τα περιεχόμενα του τοπικού χώρου αποθήκευσης.

2. Στο **QSTodoService.m**, τροποποιήστε τον ορισμό των `addItem` , έτσι ώστε το δεν συγχρονίζεται μετά την εισαγωγή του στοιχείου. Κατάργηση του `self syncData` Αποκλεισμός και να τον αντικαταστήσετε με τα εξής:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Τροποποιήστε τον ορισμό των `completeItem` όπως παραπάνω; Κατάργηση του αποκλεισμού για `self syncData` και να τον αντικαταστήσετε με τα εξής:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Σουίφτ**:

1. Στο `viewDidLoad` στο **ToDoTableViewController.swift**, σχολιάσετε αυτές τις δύο γραμμές, για να διακόψετε το συγχρονισμό κατά την έναρξη της εφαρμογής. Τη στιγμή της γραφής σε αυτό το άρθρο, η εφαρμογή Todo Σουίφτ δεν ενημερώνει την υπηρεσία όταν κάποιος προσθέτει ή ολοκληρωθεί ένα στοιχείο, μόνο κατά την εκκίνηση εφαρμογών.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Δοκιμή της εφαρμογής

Σε αυτήν την ενότητα, θα συνδεθείτε σε μια μη έγκυρη διεύθυνση URL για να προσομοιώσετε ένα σενάριο χωρίς σύνδεση. Όταν προσθέτετε στοιχεία δεδομένων, αυτά θα που θα διατηρούνται στα του τοπικού χώρου αποθήκευσης βασικά δεδομένα, αλλά δεν συγχρονίζονται στον φορητό υπολογιστή στο παρασκήνιο.

1. Αλλάξτε τη διεύθυνση URL App Mobile στο **QSTodoService.m** σε μη έγκυρη διεύθυνση URL και εκτελέστε ξανά την εφαρμογή:

    **Στόχος-C** στο QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Σουίφτ** στο ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Προσθήκη ορισμένα στοιχεία todo. Έξοδος από το simulator (ή υποχρεωτικά κλείσετε την εφαρμογή) και κάντε επανεκκίνηση. Βεβαιωθείτε ότι έχετε έχει διατηρηθεί τις αλλαγές σας.

3. Προβάλετε τα περιεχόμενα του πίνακα απομακρυσμένης TodoItem:

    + Για έναν υπολογιστή στο παρασκήνιο Node.js, μεταβείτε στην [πύλη του Azure](https://portal.azure.com/)και στην εφαρμογή Mobile παρασκηνίου κάντε κλικ στην επιλογή **Εύκολο πίνακες** > **TodoItem** για να προβάλετε τα περιεχόμενα του `TodoItem` πίνακα.
    + Για έναν υπολογιστή στο παρασκήνιο .NET, προβάλετε τα περιεχόμενα του πίνακα είτε με ένα εργαλείο SQL όπως SQL Server Management Studio, ή με ένα πρόγραμμα-πελάτη ΥΠΌΛΟΙΠΑ όπως Fiddler ή Postman.

    Βεβαιωθείτε ότι τα νέα στοιχεία *δεν* έχουν συγχρονιστεί με το διακομιστή:

4. Αλλαγή της διεύθυνσης URL για τη σωστή στην στο **QSTodoService.m** και εκτελέστε ξανά την εφαρμογή. Εκτελέστε την κίνηση ανανέωσης με έλξη προς τα κάτω στη λίστα των στοιχείων. Θα δείτε ένα κουμπί αυξομείωσης προόδου.

5. Προβάλετε τα δεδομένα TodoItem ξανά. Η νέα και τροποποιημένα TodoItems πρέπει τώρα να εμφανίζεται.

## <a name="summary"></a>Σύνοψη

Για να υποστηρίζει τη δυνατότητα συγχρονισμού χωρίς σύνδεση, χρησιμοποιήσαμε το `MSSyncTable` το περιβάλλον εργασίας και προετοιμασία `MSClient.syncContext` με ένα τοπικό χώρο αποθήκευσης. Σε αυτήν την περίπτωση του τοπικού χώρου αποθήκευσης ήταν μια βάση δεδομένων που βασίζεται σε βασικά δεδομένα.

Όταν χρησιμοποιείτε ένα τοπικό χώρο αποθήκευσης δεδομένων πυρήνα, πρέπει να ορίσετε πολλούς πίνακες με τις [Ιδιότητες συστήματος σωστά](#review-core-data).

Η κανονική λειτουργίες CRUD για τις εφαρμογές του Mobile Azure λειτουργούν σαν να εξακολουθεί να είναι συνδεδεμένος στην εφαρμογή, αλλά όλες οι εργασίες που προκύπτουν έναντι του τοπικού χώρου αποθήκευσης.

Όταν θέλουμε να συγχρονίσετε του τοπικού χώρου αποθήκευσης με το διακομιστή, χρησιμοποιήσαμε το `MSSyncTable.pullWithQuery`μέθοδο.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]

* [Εξώφυλλο στο cloud: συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές] \(Σημείωση: το βίντεο σχετικά με τις υπηρεσίες Mobile, αλλά συγχρονισμού χωρίς σύνδεση λειτουργεί με παρόμοιο τρόπο στις εφαρμογές του Mobile Azure\)

<!-- URLs. -->


[Δημιουργήστε μια εφαρμογή για iOS]: app-service-mobile-ios-get-started.md
[Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Εξώφυλλο του cloud: Συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές συσκευές]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

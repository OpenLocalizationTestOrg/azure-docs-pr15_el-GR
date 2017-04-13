<properties
    pageTitle="Πώς μπορείτε να χρησιμοποιήστε iOS SDK για εφαρμογές του Mobile Azure"
    description="Πώς μπορείτε να χρησιμοποιήστε iOS SDK για εφαρμογές του Mobile Azure"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Πώς μπορείτε να χρησιμοποιήστε iOS βιβλιοθήκη προγράμματος-πελάτη για εφαρμογές του Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Αυτός ο οδηγός σας μαθαίνει για να εκτελέσετε κοινά σενάρια χρησιμοποιώντας την πιο πρόσφατη [εφαρμογές του Mobile Azure iOS SDK][1]. Εάν είστε εξοικειωμένοι με εφαρμογές του Mobile Azure, πρώτη ολοκλήρωσης [Azure Mobile Apps γρήγορης εκκίνησης] για να δημιουργήσετε έναν υπολογιστή στο παρασκήνιο, δημιουργήστε έναν πίνακα και κάντε λήψη ενός έργου Xcode προ-δομημένες iOS. Σε αυτόν τον οδηγό, θα σας εστιάσετε την πλευρά του προγράμματος-πελάτη iOS SDK. Για να μάθετε περισσότερα σχετικά με την πλευρά του διακομιστή SDK για υπολογιστή στο παρασκήνιο, ανατρέξτε στο θέμα το HOWTOs SDK διακομιστή.

## <a name="reference-documentation"></a>Τεκμηρίωση αναφοράς

Η τεκμηρίωση αναφοράς για το πρόγραμμα-πελάτης iOS SDK βρίσκεται εδώ: [εφαρμογές του Mobile Azure iOS αναφοράς υπολογιστή-πελάτη][2].

## <a name="supported-platforms"></a>Υποστηριζόμενες πλατφόρμες

Το iOS SDK υποστηρίζει στόχο-C έργα, 2.2 Σουίφτ έργα και έργα 2.3 Σουίφτ για iOS εκδόσεις 8.0 ή νεότερη έκδοση.

Ο έλεγχος ταυτότητας "διακομιστή ροής" χρησιμοποιεί μια προβολής Web για το περιβάλλον εργασίας Χρήστη που παρουσιάστηκε.  Εάν η συσκευή δεν είναι σε θέση να παρουσιάσετε ένα περιβάλλον εργασίας Χρήστη προβολής Web, στη συνέχεια, απαιτείται μια άλλη μέθοδος ελέγχου ταυτότητας που είναι εκτός του αντικειμένου του προϊόντος.  
Αυτό το SDK δεν είναι κατάλληλη για παρακολούθησης ή τύπου ομοίως περιορισμένα συσκευές, επομένως.

##<a name="Setup"></a>Το πρόγραμμα εγκατάστασης και τις προϋποθέσεις

Αυτός ο οδηγός προϋποθέτει ότι έχετε δημιουργήσει έναν υπολογιστή στο παρασκήνιο με έναν πίνακα. Αυτός ο οδηγός προϋποθέτει ότι ο πίνακας έχει το ίδιο σχήμα ως τους πίνακες σε αυτά τα προγράμματα εκμάθησης. Αυτός ο οδηγός επίσης προϋποθέτει ότι στον κώδικά σας, μπορείτε να κάνετε αναφορά `MicrosoftAzureMobile.framework` και εισαγωγή `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Πώς μπορείτε να: δημιουργία προγράμματος-πελάτη

Για να αποκτήσετε πρόσβαση σε μια παρασκηνίου εφαρμογές του Mobile Azure στο έργο σας, δημιουργήστε μια `MSClient`. Αντικατάσταση `AppUrl` με τη διεύθυνση URL της εφαρμογής. Μπορείτε να αφήσετε `gatewayURLString` και `applicationKey` κενό. Εάν ρυθμίσετε μια πύλη για έλεγχο ταυτότητας, τη συμπλήρωση `gatewayURLString` με τη διεύθυνση URL της πύλης.

**Στόχος-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Σουίφτ**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Πώς μπορείτε να: Δημιουργία πίνακα αναφοράς

Για να δεδομένων της access ή ενημέρωση, δημιουργήστε μια αναφορά για τον πίνακα παρασκηνίου. Αντικατάσταση `TodoItem` με το όνομα του πίνακα

**Στόχος C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Σουίφτ**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Πώς μπορείτε να: ερωτήματος δεδομένων

Για να δημιουργήσετε ένα ερώτημα βάσης δεδομένων, ερωτήματος το `MSTable` αντικειμένου. Το παρακάτω ερώτημα λαμβάνει όλα τα στοιχεία στην `TodoItem` και καταγράφει το κείμενο κάθε στοιχείου.

**Στόχος-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Σουίφτ**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Πώς μπορείτε να: φίλτρο επέστρεψε δεδομένα

Για να φιλτράρετε τα αποτελέσματα, υπάρχουν πολλές διαθέσιμες επιλογές.

Για να φιλτράρετε χρησιμοποιώντας ένα κατηγόρημα, χρησιμοποιήστε μια `NSPredicate` και `readWithPredicate`. Τα ακόλουθα φίλτρα επέστρεψε δεδομένα για την εύρεση μόνο ελλιπή Todo στοιχεία.

**Στόχος-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Σουίφτ**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Πώς μπορείτε να: χρήση MSQuery

Για να εκτελέσετε ένα σύνθετο ερώτημα (συμπεριλαμβανομένης της ταξινόμησης και σελιδοποίησης), δημιουργήστε μια `MSQuery` αντικείμενο, απευθείας ή με τη χρήση κατηγόρημα:

**Στόχος-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Σουίφτ**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`σας επιτρέπει να ελέγχετε πολλές συμπεριφορές ερωτήματος.

* Καθορίστε τη σειρά των αποτελεσμάτων
* Περιορίστε τα πεδία που θα επιστρέψει
* Περιορίστε τον αριθμό των εγγραφών για να επιστρέψετε
* Καθορίστε το συνολικό πλήθος απάντηση
* Καθορισμός προσαρμοσμένου ερωτήματος παραμέτρων συμβολοσειράς στην πρόσκληση σε
* Εφαρμογή πρόσθετες λειτουργίες

Εκτέλεση μιας `MSQuery` ερωτήματος καλώντας `readWithCompletion` στο αντικείμενο.

## <a name="sorting"></a>Πώς μπορείτε να: ταξινόμηση δεδομένων με MSQuery

Για να ταξινομήσετε τα αποτελέσματα, ας δούμε ένα παράδειγμα. Για να ταξινομήσετε κατά αύξουσα σειρά το πεδίο "κείμενο", στη συνέχεια, κατά φθίνουσα 'ολοκλήρωση', κλήση `MSQuery` ως εξής:

**Στόχος-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Σουίφτ**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Πώς μπορείτε να: Περιορίστε τα πεδία και αναπτύξτε το στοιχείο παραμέτρους συμβολοσειράς ερωτήματος με MSQuery

Για να περιορίσετε τα πεδία που θα επιστραφούν σε ένα ερώτημα, καθορίστε τα ονόματα των πεδίων στην ιδιότητα **selectFields** . Αυτό το παράδειγμα επιστρέφει μόνο το κείμενο και τις ολοκληρωμένες πεδία:

**Στόχος-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Σουίφτ**:

```
query.selectFields = ["text", "complete"]
```

Για να συμπεριλάβετε παραμέτρους συμβολοσειράς ερωτήματος επιπλέον στην πρόσκληση σε διακομιστή (για παράδειγμα, επειδή χρησιμοποιεί μια προσαρμοσμένη δέσμη ενεργειών του διακομιστή τους), τη συμπλήρωση `query.parameters` ως εξής:

**Στόχος-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Σουίφτ**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Πώς μπορείτε να: ρύθμιση παραμέτρων του μεγέθους σελίδας

Με εφαρμογές του Mobile Azure, το μέγεθος της σελίδας ελέγχει τον αριθμό των εγγραφών που είναι τα μηνύματα μεταφέρονται ταυτόχρονα από τους πίνακες παρασκηνίου. Μια κλήση σε `pull` δεδομένων, στη συνέχεια, θα μαζική ασφαλείας δεδομένων, με βάση αυτό το μέγεθος σελίδας, μέχρι να υπάρχουν περισσότερες εγγραφές για να αποσπάσετε.

Είναι δυνατή η ρύθμιση παραμέτρων του μεγέθους σελίδας με χρήση **MSPullSettings** , όπως φαίνεται παρακάτω. Το προεπιλεγμένο μέγεθος σελίδας είναι 50 και το παρακάτω παράδειγμα αλλάζει σε 3.

Μπορούσατε να ρυθμίσετε ένα διαφορετικό μέγεθος σελίδας για λόγους απόδοσης. Εάν έχετε πολλές εγγραφές μικρές δεδομένων, το μέγεθος του υψηλή μειώνει τον αριθμό των απαιτείται επικοινωνία με το διακομιστή. 

Αυτή η ρύθμιση ελέγχει μόνο το μέγεθος της σελίδας στην πλευρά του προγράμματος-πελάτη. Εάν ο υπολογιστής-πελάτης ζητά από ένα μεγαλύτερο μέγεθος σελίδας από όσες υποστηρίζει υπόβαθρο εφαρμογές του Mobile, το μέγεθος της σελίδας έχει φτάσει το μέγιστο όριο υπόβαθρο έχει ρυθμιστεί ώστε να υποστηρίζει το. 

Αυτή η ρύθμιση είναι επίσης τον _αριθμό_ των εγγραφών δεδομένων, όχι το _μέγεθος σε byte_.

Αν αυξήσετε το μέγεθος της σελίδας προγράμματος-πελάτη, [μπορείτε, επίσης, θα πρέπει να αυξήσετε το μέγεθος της σελίδας στο διακομιστή](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Στόχος-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Σουίφτ**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Πώς μπορείτε να: εισαγωγή δεδομένων

Για να εισαγάγετε μια νέα γραμμή πίνακα, δημιουργήστε μια `NSDictionary` και ενεργοποιούν `table insert`. Εάν είναι ενεργοποιημένη η [Δυναμική σχήματος] , Azure εφαρμογής υπηρεσίας κινητών υπόβαθρο δημιουργεί αυτόματα νέες στήλες με βάση το `NSDictionary`.

Εάν `id` είναι δεν παρέχονται υπόβαθρο δημιουργεί αυτόματα ένα νέο μοναδικό αναγνωριστικό. Δώστε τις δικές σας `id` για να χρησιμοποιήσετε ηλεκτρονικού ταχυδρομείου διευθύνσεων, τα ονόματα των χρηστών, ή το δικό σας προσαρμοσμένο τις τιμές ως αναγνωριστικό. Παροχή τη δική σας Ταυτότητα μπορεί να διευκολύνει τη συνδέσμων και λογική επαγγελματικά βάσης δεδομένων.

Το `result` περιέχει το νέο στοιχείο που έχει εισαχθεί. Ανάλογα με τη λογική του διακομιστή σας, μπορεί να έχει πρόσθετα ή τροποποιημένα δεδομένα σε σύγκριση με το τι μεταβιβάστηκε στο διακομιστή.

**Στόχος-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Σουίφτ**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Πώς μπορείτε να: τροποποίηση των δεδομένων

Για να ενημερώσετε μια υπάρχουσα γραμμή, τροποποιήστε ένα στοιχείο και κλήση `update`:

**Στόχος C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Σουίφτ**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Εναλλακτικά, πληκτρολογήστε το Αναγνωριστικό γραμμής και το ενημερωμένο πεδίο:

**Στόχος-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Σουίφτ**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Τουλάχιστον, το `id` χαρακτηριστικού πρέπει να οριστεί όταν κάνετε ενημερώσεις.

##<a name="deleting"></a>Πώς μπορείτε να: διαγραφή δεδομένων

Για να διαγράψετε ένα στοιχείο, κλήση `delete` με το στοιχείο:

**Στόχος C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Σουίφτ**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Εναλλακτικά, διαγραφή, παρέχοντας ένα Αναγνωριστικό γραμμής:

**Στόχος C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Σουίφτ**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Τουλάχιστον, το `id` χαρακτηριστικού πρέπει να οριστεί κατά την πραγματοποίηση διαγράφει.

##<a name="customapi"></a>Πώς μπορείτε να: κλήση προσαρμοσμένου API

Με ένα προσαρμοσμένο API, να μπορείτε να εμφανίσετε τις λειτουργίες παρασκηνίου. Δεν είναι απαραίτητο να αντιστοιχίσετε σε μια λειτουργία πίνακα. Κάντε μπορείτε να αποκτήσετε περισσότερο έλεγχο στην ανταλλαγή μηνυμάτων, μπορείτε ακόμα και όχι μόνο ανάγνωση/σύνολο κεφαλίδες και αλλαγή της μορφής σώματος απόκρισης. Για να μάθετε πώς μπορείτε να δημιουργήσετε ένα προσαρμοσμένο API στον υπολογιστή στο παρασκήνιο, διαβάστε [Προσαρμοσμένο APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Για να καλέσετε μια προσαρμοσμένη API, καλέστε `MSClient.invokeAPI`. Της αίτησης και της απόκρισης περιεχομένου θεωρούνται JSON. Για να χρησιμοποιήσετε άλλους τύπους πολυμέσων, [Χρησιμοποιήστε τα άλλα υπερφόρτωσης του `invokeAPI` ] [ 5].  Για να κάνετε μια `GET` αίτηση αντί για ένα `POST` ζητήσετε, ρύθμιση παραμέτρων `HTTPMethod` για να `"GET"` και παράμετρο `body` να `nil` (εφόσον αιτήσεις ΛΉΨΗΣ δεν έχουν των μηνυμάτων που.) Εάν το προσαρμοσμένο API υποστηρίζει άλλα ρήματα HTTP, αλλάξτε `HTTPMethod` σωστά.

**Στόχος C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Σουίφτ**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Πώς μπορείτε να: Register push πρότυπα για την αποστολή ειδοποιήσεων πλατφόρμες

Για να καταχωρήσετε τα πρότυπα, μεταβιβάζουν πρότυπα με τη μέθοδο **client.push registerDeviceToken** στην εφαρμογή υπολογιστή-πελάτη.

**Στόχος C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Σουίφτ**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Τα πρότυπα είναι του τύπου NSDictionary και μπορούν να περιέχουν πολλά πρότυπα με την εξής μορφή:

**Στόχος C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Σουίφτ**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Όλες οι ετικέτες αφαιρούνται από την αίτηση για την ασφάλεια.  Για να προσθέσετε ετικέτες σε εγκαταστάσεις ή πρότυπα στο εσωτερικό των εγκαταστάσεων, ανατρέξτε στο θέμα [εργασία με το διακομιστή υποστήριξης .NET SDK για εφαρμογές του Mobile Azure][4].  Για την αποστολή ειδοποιήσεων με χρήση αυτών των καταχωρημένων προτύπων, εργασία με [Ειδοποίηση διανομείς API][3].

##<a name="errors"></a>Πώς μπορείτε να: χειρισμού σφαλμάτων

Όταν καλείτε μια κινητή παρασκηνίου Azure εφαρμογής υπηρεσίας, ο αποκλεισμός ολοκλήρωσης περιέχει ένα `NSError` παραμέτρου. Όταν παρουσιάζεται ένα σφάλμα, αυτή η παράμετρος είναι μη υπόψη. Στον κώδικα, πρέπει να ελέγξετε αυτή η παράμετρος και χειρίζεται το σφάλμα, εφόσον χρειάζεται, όπως φαίνεται στο το προηγούμενο τμήματα κώδικα.

Το αρχείο [`<WindowsAzureMobileServices/MSError.h>`] [6] ορίζει τις σταθερές `MSErrorResponseKey`, `MSErrorRequestKey`, και `MSErrorServerItemKey`. Για να δείτε περισσότερα δεδομένα που σχετίζονται με το σφάλμα:

**Στόχος C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Σουίφτ**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Επιπλέον, το αρχείο ορίζει σταθερές για κάθε κωδικό σφάλματος:

**Στόχος C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Σουίφτ**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Πώς μπορείτε να: ελέγχουν την ταυτότητα χρηστών με τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory

Μπορείτε να χρησιμοποιήσετε το ενεργό βιβλιοθήκη ελέγχου ταυτότητας καταλόγου (ADAL) για να πραγματοποιήσετε τους χρήστες στην εφαρμογή σας με χρήση Azure Active Directory. Ο έλεγχος ταυτότητας ροής προγράμματος-πελάτη χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας SDK είναι προτιμότερο να χρησιμοποιείτε το `loginWithProvider:completion:` μέθοδο.  Έλεγχος ταυτότητας ροής προγράμματος-πελάτη παρέχει ένα πιο εγγενή αίσθηση UX και επιτρέπει επιπλέον προσαρμογής.

1. Ρύθμιση παραμέτρων του στοιχείου διακομιστή εφαρμογής για κινητές συσκευές για AAD εισόδου, ακολουθώντας το [πώς μπορείτε να ρυθμίσετε τις παραμέτρους της εφαρμογής υπηρεσίας για τη σύνδεση υπηρεσίας καταλόγου Active Directory] [ 7] το πρόγραμμα εκμάθησης. Βεβαιωθείτε ότι για να ολοκληρώσετε το προαιρετικό βήμα την καταχώρηση μιας εφαρμογής εγγενές πρόγραμμα-πελάτη. Για iOS, συνιστάται η ανακατεύθυνση URI είναι μέρος της φόρμας `<app-scheme>://<bundle-id>`. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα η [γρήγορη έναρξη ADAL iOS][8].

2. Εγκαταστήστε το ADAL χρησιμοποιώντας Cocoapods. Επεξεργαστείτε το Podfile για να συμπεριλάβετε τον ακόλουθο ορισμό, αντικαθιστώντας **Στο ΈΡΓΟ ΣΑΣ** με το όνομα του έργου σας Xcode:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   και το Pod:

        pod 'ADALiOS'

3. Χρησιμοποιώντας το Terminal, εκτελέστε `pod install` από τον κατάλογο που περιέχει το έργο σας και, στη συνέχεια, ανοίξτε που δημιουργήθηκε Xcode χώρο εργασίας (όχι του έργου).

4. Προσθέστε τον ακόλουθο κώδικα στην εφαρμογή σας, σύμφωνα με τη γλώσσα που χρησιμοποιείτε. Σε κάθε, κάντε τα παρακάτω αντικατάστασης ελέγχου:

    * Αντικαταστήστε **Εισαγωγή-ΑΡΧΉ-ΕΔΏ** με το όνομα του μισθωτή με την οποία παρασχεθεί την εφαρμογή σας. Η μορφή πρέπει να είναι https://login.windows.net/contoso.onmicrosoft.com. Αυτή η τιμή μπορεί να αντιγραφεί από την καρτέλα τομέα στο σας Azure Active Directory στο [Azure κλασική πύλη].
    * Αντικαταστήστε **Εισαγωγή-ΠΌΡΩΝ-ID-ΕΔΏ** με το Αναγνωριστικό υπολογιστή-πελάτη για την εφαρμογή για κινητές συσκευές παρασκηνίου. Μπορείτε να αποκτήσετε το Αναγνωριστικό υπολογιστή-πελάτη από την καρτέλα **για προχωρημένους** στην περιοχή **Azure Active Directory ρυθμίσεις** στην πύλη.
    * Αντικαταστήστε **Εισαγωγή-πρόγραμμα-ΠΕΛΆΤΗ-ID-ΕΔΏ** με το Αναγνωριστικό πελάτη που αντιγράψατε από την εφαρμογή εγγενές πρόγραμμα-πελάτη.
    * Αντικαταστήστε **Εισαγωγή-REDIRECT-URI-ΕΔΏ** με το τελικό σημείο _/.auth/login/done_ της τοποθεσίας σας, χρησιμοποιώντας το σχήμα HTTPS. Αυτή η τιμή πρέπει να είναι παρόμοια με _https://contoso.azurewebsites.net/.auth/login/done_.

**Στόχος C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Σουίφτ**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Πώς μπορείτε να: έλεγχο ταυτότητας χρηστών με το Facebook SDK για iOS

Μπορείτε να χρησιμοποιήσετε το SDK Facebook για iOS για να συνδεθείτε χρηστών στο Facebook χρησιμοποιώντας την εφαρμογή σας.  Χρήση του ελέγχου ταυτότητας ροής ένα πρόγραμμα-πελάτη είναι προτιμότερο να χρησιμοποιείτε το `loginWithProvider:completion:` μέθοδο.  Ο έλεγχος ταυτότητας ροής προγράμματος-πελάτη παρέχει ένα πιο εγγενή αίσθηση UX και επιτρέπει επιπλέον προσαρμογής.

1. Ρύθμιση παραμέτρων του στοιχείου διακομιστή εφαρμογής για κινητές συσκευές για εισόδου στο Facebook, ακολουθώντας το [πώς μπορείτε να ρυθμίσετε τις παραμέτρους της εφαρμογής υπηρεσίας για τη σύνδεση Facebook] [ 9] το πρόγραμμα εκμάθησης.

2. Εγκατάσταση του SDK Facebook για iOS, ακολουθώντας το [Facebook SDK για iOS - γρήγορα αποτελέσματα] [ 10] τεκμηρίωση. Αντί να δημιουργήσετε μια εφαρμογή, μπορείτε να προσθέσετε την πλατφόρμα iOS σε υπάρχουσα εγγραφή σας. 

3. Τεκμηρίωση του Facebook περιλαμβάνει ορισμένες κώδικα στόχο-C σε εφαρμογή πληρεξουσίου. Εάν χρησιμοποιείτε **Σουίφτ**, μπορείτε να χρησιμοποιήσετε τις παρακάτω μεταφράσεις για AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Εκτός από την προσθήκη `FBSDKCoreKit.framework` στο έργο σας, να προσθέσετε επίσης μια αναφορά σε `FBSDKLoginKit.framework` με τον ίδιο τρόπο. 

4. Προσθέστε τον ακόλουθο κώδικα στην εφαρμογή σας, σύμφωνα με τη γλώσσα που χρησιμοποιείτε. 

**Στόχος C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Σουίφτ**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Πώς μπορείτε να: έλεγχο ταυτότητας χρηστών με ύφασμα Twitter για iOS

Μπορείτε να χρησιμοποιήσετε ύφασμα για iOS για να συνδεθείτε χρήστες στο Twitter χρησιμοποιώντας την εφαρμογή σας. Έλεγχος ταυτότητας ροής προγράμματος-πελάτη είναι προτιμότερο να χρησιμοποιείτε το `loginWithProvider:completion:` μέθοδο, όπως παρέχει μια πιο εγγενή αίσθηση UX και επιτρέπει την επιπλέον προσαρμογής.

1. Ρύθμιση παραμέτρων σας παρασκηνίου εφαρμογής για κινητές συσκευές για εισόδου στο Twitter, ακολουθώντας το πρόγραμμα εκμάθησης [τον τρόπο ρύθμισης παραμέτρων εφαρμογής υπηρεσίας για το Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) .

2. Προσθήκη ύφασμα στο έργο σας ακολουθώντας την τεκμηρίωση [ύφασμα για iOS - γρήγορα αποτελέσματα] και τη ρύθμιση του TwitterKit.

    > [AZURE.NOTE] Από προεπιλογή, ύφασμα δημιουργεί μια εφαρμογή Twitter για εσάς. Μπορείτε να αποφύγετε τη δημιουργία μιας εφαρμογής με την καταχώρηση το κλειδί καταναλωτή και μυστικό καταναλωτή που έχετε δημιουργήσει προηγουμένως χρησιμοποιώντας τα ακόλουθα τμήματα κώδικα.  Εναλλακτικά, μπορείτε να αντικαταστήσετε τις τιμές καταναλωτή κλειδί και μυστικό καταναλωτή που δίνετε για εφαρμογής υπηρεσίας με τις τιμές που βλέπετε στον πίνακα [Εργαλείων ύφασμα]. Εάν ενεργοποιήσετε αυτήν την επιλογή, βεβαιωθείτε ότι για να ορίσετε τη διεύθυνση URL επιστροφής κλήσης σε μια τιμή κράτησης θέσης, όπως είναι οι `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Εάν επιλέξετε να χρησιμοποιήσετε το απόρρητο που δημιουργήσατε νωρίτερα, προσθέστε τον ακόλουθο κώδικα τον πληρεξούσιο εφαρμογής:
    
    **Στόχος C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Σουίφτ**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Προσθέστε τον ακόλουθο κώδικα στην εφαρμογή σας, σύμφωνα με τη γλώσσα που χρησιμοποιείτε. 

**Στόχος C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Σουίφτ**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Πώς μπορείτε να: έλεγχο ταυτότητας χρηστών με το Google εισόδου SDK για iOS

Μπορείτε να χρησιμοποιήσετε το Google εισόδου SDK για iOS για να πραγματοποιήσετε τους χρήστες στην εφαρμογή σας χρησιμοποιώντας ένα λογαριασμό Google.  Google ανακοινώσει πρόσφατα αλλαγές για να τις πολιτικές ασφαλείας OAuth.  Αυτές οι αλλαγές πολιτικής απαιτεί τη χρήση του Google SDK στο μέλλον.

1. Ρύθμιση παραμέτρων σας παρασκηνίου εφαρμογής για κινητές συσκευές για είσοδος στο Google, ακολουθώντας το πρόγραμμα εκμάθησης [πώς μπορείτε να ρυθμίσετε τις παραμέτρους της εφαρμογής υπηρεσίας για τη σύνδεση Google](app-service-mobile-how-to-configure-google-authentication.md) .

2. Εγκαταστήστε το Google SDK για iOS ακολουθώντας την τεκμηρίωση [Google εισόδου για iOS - Έναρξη ενοποίηση](https://developers.google.com/identity/sign-in/ios/start-integrating) . Ενδέχεται να μπορείτε να μεταβείτε στην ενότητα "Τον έλεγχο ταυτότητας με ένα διακομιστή παρασκηνίου".

3. Προσθέστε τα ακόλουθα ο πληρεξούσιος `signIn:didSignInForUser:withError:` μέθοδο, σύμφωνα με τη γλώσσα που χρησιμοποιείτε.

**Στόχος C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Σουίφτ**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Βεβαιωθείτε ότι μπορείτε επίσης να προσθέσετε τα εξής για να `application:didFinishLaunchingWithOptions:` στην εφαρμογή πληρεξούσιο αντικαθιστώντας "SERVER_CLIENT_ID" με το ίδιο Αναγνωριστικό που χρησιμοποιήσατε για να ρυθμίσετε τις παραμέτρους της εφαρμογής υπηρεσίας στο βήμα 1.

**Στόχος C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Σουίφτ**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Προσθέστε τον ακόλουθο κώδικα στην εφαρμογή σας με ένα UIViewController που υλοποιεί το `GIDSignInUIDelegate` πρωτόκολλο, σύμφωνα με τη γλώσσα που χρησιμοποιείτε.  Έχετε πραγματοποιήσει έξοδο πριν να εισέλθετε ξανά και, αν και δεν χρειάζεται να εισαγάγετε ξανά τα διαπιστευτήριά σας, βλέπετε ένα παράθυρο διαλόγου συγκατάθεση.  Καλέστε αυτήν τη μέθοδο μόνο όταν το διακριτικό περιόδου λειτουργίας έχει λήξει.
 
 **Στόχος C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Σουίφτ**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure κινητή εφαρμογές γρήγορης εκκίνησης]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Δυναμική σχήματος]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Δομή του πίνακα εργαλείων]: https://www.fabric.io/home
[Ύφασμα για iOS - γρήγορα αποτελέσματα]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started

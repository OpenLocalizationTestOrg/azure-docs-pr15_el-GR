##<a name="create-client"></a>Δημιουργία σύνδεσης προγράμματος-πελάτη

Δημιουργία σύνδεσης προγράμματος-πελάτη, δημιουργώντας μια `WindowsAzure.MobileServiceClient` αντικειμένου.  Αντικατάσταση `appUrl` με τη διεύθυνση URL για την εφαρμογή για κινητές συσκευές.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Εργασία με πίνακες

Για να δεδομένων της access ή ενημέρωση, δημιουργήστε μια αναφορά για τον πίνακα παρασκηνίου. Αντικατάσταση `tableName` με το όνομα του πίνακα

```
var table = client.getTable(tableName);
```

Όταν έχετε μια αναφορά πίνακα, μπορείτε να εργαστείτε περαιτέρω με τον πίνακά σας:

* [Ερώτημα σε πίνακα](#querying)
  * [Φιλτράρισμα δεδομένων](#table-filter)
  * [Σελιδοποίησης στα δεδομένα](#table-paging)
  * [Ταξινόμηση δεδομένων](#sorting-data)
* [Εισαγωγή δεδομένων](#inserting)
* [Τροποποίηση των δεδομένων](#modifying)
* [Διαγραφή δεδομένων](#deleting)

###<a name="querying"></a>Πώς μπορείτε να: υποβολή ερωτήματος σε μια αναφορά πίνακα

Όταν έχετε μια αναφορά πίνακα, μπορείτε να το χρησιμοποιήσετε για να υποβάλετε ερώτημα για τα δεδομένα στο διακομιστή.  Ερωτήματα γίνονται σε γλώσσα "LINQ-μου αρέσει".
Για να επαναφέρετε όλα τα δεδομένα από τον πίνακα, χρησιμοποιήστε τα εξής:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Η συνάρτηση επιτυχίας ονομάζεται με τα αποτελέσματα.   Μην χρησιμοποιείτε `for (var i in results)` στο την επιτυχία λειτουργούν όπως που θα διαδοχικές προσεγγίσεις επάνω από τις πληροφορίες που περιλαμβάνονται στα αποτελέσματα όταν άλλες συναρτήσεις ερωτήματος (όπως είναι οι `.includeTotalCount()`) που χρησιμοποιούνται.

Για περισσότερες πληροφορίες σχετικά με τη σύνταξη ερωτήματος, ανατρέξτε στην [τεκμηρίωση αντικείμενο ερωτήματος].

####<a name="table-filter"></a>Φιλτράρισμα των δεδομένων στο διακομιστή

Μπορείτε να χρησιμοποιήσετε ένα `where` όρος στην αναφορά του πίνακα:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Μπορείτε επίσης να χρησιμοποιήσετε μια συνάρτηση που φιλτράρει το αντικείμενο.  Σε αυτήν την περίπτωση το `this` μεταβλητή έχει ανατεθεί στο τρέχον αντικείμενο που φιλτράρονται.  Τα ακόλουθα λειτουργικά είναι ισοδύναμη με εκ των προτέρων παράδειγμα:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Σελιδοποίησης στα δεδομένα

Χρησιμοποιεί τις μεθόδους take() και skip().  Για παράδειγμα, εάν θέλετε να διαιρέσετε τον πίνακα σε εγγραφές 100 γραμμής:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Το `.includeTotalCount()` μέθοδος χρησιμοποιείται για να προσθέσετε ένα πεδίο totalCount το αντικείμενο αποτελεσμάτων.  Το πεδίο totalCount συμπληρώνεται με τον συνολικό αριθμό των εγγραφών που θα επιστραφεί εάν χρησιμοποιείται χωρίς σελιδοποίησης.

Μπορείτε να χρησιμοποιήσετε τη μεταβλητή σελίδες και ορισμένα κουμπιά περιβάλλοντος εργασίας Χρήστη για την παροχή μια σελίδα λίστας. Χρησιμοποιήστε loadPage() για να φορτώσετε τις νέες εγγραφές για κάθε σελίδα.  Θα πρέπει να μπορείτε να εφαρμόσετε κάποια λειτουργία της εγγραφής στο cache για ταχύτητα πρόσβασης με εγγραφές που έχουν φορτωθεί ήδη.


####<a name="sorting-data"></a>Πώς μπορείτε να: επιστροφής ταξινομημένων δεδομένων

Χρησιμοποιήστε τις μεθόδους ερωτήματος .orderBy() ή .orderByDescending():

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Για περισσότερες πληροφορίες σχετικά με το αντικείμενο ερωτήματος, ανατρέξτε στην [τεκμηρίωση αντικείμενο ερωτήματος].

###<a name="inserting"></a>Πώς μπορείτε να: εισαγωγή δεδομένων

Δημιουργήστε ένα αντικείμενο JavaScript με την κατάλληλη ημερομηνία και καλέστε table.insert() ασύγχρονα:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Για επιτυχή εισαγωγής, επιστρέφεται το στοιχείο που έχει εισαχθεί με τα πρόσθετα πεδία που απαιτούνται για λειτουργίες συγχρονισμού.  Θα πρέπει να ενημερώσετε το δικό σας cache με αυτές τις πληροφορίες για τις νεότερες ενημερώσεις.

Σημειώστε ότι το SDK Server Azure Mobile εφαρμογές Node.js υποστηρίζει δυναμικές σχήματος για σκοπούς ανάπτυξης.
Στην περίπτωση δυναμική σχήμα, το σχήμα του πίνακα ενημερώνεται δυναμική, επιτρέποντάς σας την προσθήκη στηλών σε πίνακα απλώς, καθορίζοντας τους σε μια εισαγωγή ή ενημέρωση λειτουργία.  Συνιστάται να απενεργοποιήσετε δυναμικής σχήματος πριν περάσετε την εφαρμογή με το παραγωγής.

###<a name="modifying"></a>Πώς μπορείτε να: τροποποίηση των δεδομένων

Παρόμοια με τη μέθοδο .insert(), πρέπει να δημιουργήσετε το αντικείμενο ενημέρωση και, στη συνέχεια, καλέστε .update().  Το αντικείμενο ενημερωμένη έκδοση πρέπει να περιέχει το Αναγνωριστικό της εγγραφής για να ενημερωθούν - είναι αυτό που λαμβάνεται κατά την ανάγνωση της εγγραφής ή κατά την κλήση .insert().

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Πώς μπορείτε να: διαγραφή δεδομένων

Κλήση της μεθόδου .del() για να διαγράψετε μια εγγραφή.  Μεταβίβαση το Αναγνωριστικό στην αναφορά του αντικειμένου:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```

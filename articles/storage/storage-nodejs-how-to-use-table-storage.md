<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων του Azure από Node.js | Microsoft Azure"
    description="Αποθηκεύστε δομημένων δεδομένων στο cloud χρησιμοποιώντας χώρος αποθήκευσης πινάκων του Azure, ένα χώρο αποθήκευσης δεδομένων NoSQL."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων του Azure από Node.js

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια με την υπηρεσία Azure πίνακα σε μια εφαρμογή Node.js.

Τα παραδείγματα κώδικα σε αυτό το θέμα, θεωρείται ότι έχετε ήδη μια εφαρμογή Node.js. Για πληροφορίες σχετικά με τη δημιουργία μιας εφαρμογής Node.js στο Azure, ανατρέξτε στο θέμα οποιαδήποτε από αυτά τα θέματα:

- [Δημιουργία εφαρμογής web Node.js στο Azure εφαρμογής υπηρεσίας](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js στον Azure χρησιμοποιώντας WebMatrix](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (χρησιμοποιώντας το Windows PowerShell)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για πρόσβαση στο χώρο αποθήκευσης Azure

Για να χρησιμοποιήσετε το χώρο αποθήκευσης Azure, χρειάζεστε το Azure SDK χώρου αποθήκευσης για Node.js, η οποία περιλαμβάνει ένα σύνολο βιβλιοθηκών ευχρηστία που επικοινωνία με τις υπηρεσίες ΥΠΌΛΟΙΠΟ του χώρου αποθήκευσης.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Χρήση διαχείρισης πακέτου κόμβου (NPM) για την εγκατάσταση του πακέτου

1.  Χρησιμοποιήστε ένα περιβάλλον γραμμής εντολών όπως **PowerShell** (Windows), το **Terminal** (Mac) ή το **πάρτι** (Unix) και μεταβείτε στο φάκελο όπου δημιουργήσατε την εφαρμογή σας.

2.  Πληκτρολογήστε **npm εγκατάσταση αποθήκευσης azure** στο παράθυρο εντολών. Έξοδος από την εντολή είναι παρόμοιο με το παρακάτω παράδειγμα.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Μπορείτε να εκτελέσετε με μη αυτόματο τρόπο την εντολή **ls** για να επιβεβαιώσετε ότι ένα **κόμβου\_λειτουργικές μονάδες** που δημιουργήθηκε το φάκελο. Μέσα σε αυτόν το φάκελο θα βρείτε το πακέτο **αποθήκευσης azure** , η οποία περιέχει τις βιβλιοθήκες που πρέπει να έχουν πρόσβαση χώρου αποθήκευσης.

### <a name="import-the-package"></a>Εισαγωγή του πακέτου

Προσθέστε τον ακόλουθο κώδικα στο επάνω μέρος του αρχείου **server.js** στην εφαρμογή σας:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Ρύθμιση σύνδεσης αποθήκευσης Azure

Τη λειτουργική μονάδα azure θα διαβάσει τις μεταβλητές περιβάλλοντος AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΛΟΓΑΡΙΑΣΜΟΎ και AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ACCESS\_ΚΛΕΙΔΊ ή AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΣΎΝΔΕΣΗΣ\_ΣΥΜΒΟΛΟΣΕΙΡΆΣ για τις πληροφορίες που απαιτούνται για να συνδεθείτε στο λογαριασμό σας Azure χώρου αποθήκευσης. Εάν δεν έχουν οριστεί αυτές τις μεταβλητές περιβάλλοντος, πρέπει να καθορίσετε τις πληροφορίες λογαριασμού κατά την κλήση **TableService**.

Ένα παράδειγμα για τη ρύθμιση των μεταβλητών περιβάλλοντος στην [Πύλη του Azure](https://portal.azure.com) για μια τοποθεσία Web του Azure, ανατρέξτε στο θέμα [Node.js εφαρμογής web με την υπηρεσία πίνακα Azure].

## <a name="create-a-table"></a>Δημιουργία πίνακα

Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **TableService** και χρησιμοποιεί για να δημιουργήσετε έναν νέο πίνακα. Προσθέστε την ακόλουθη κοντά στο επάνω μέρος **server.js**.

    var tableSvc = azure.createTableService();

Την κλήση σε **createTableIfNotExists** θα δημιουργήσει έναν νέο πίνακα με το καθορισμένο όνομα, εάν δεν υπάρχει ήδη. Το παρακάτω παράδειγμα δημιουργεί ένα νέο πίνακα με το όνομα 'Πίνακας' Εάν δεν υπάρχει ήδη:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

Το `result.created` θα είναι `true` Εάν δημιουργηθεί ένα νέο πίνακα, και `false` εάν ο πίνακας υπάρχει ήδη. Το `response` θα περιέχει πληροφορίες σχετικά με την αίτηση.

### <a name="filters"></a>Φίλτρα

Προαιρετικό λειτουργίες φιλτραρίσματος μπορούν να εφαρμοστούν σε διάφορες λειτουργίες που εκτελούνται με χρήση **TableService**. Φιλτράρισμα λειτουργίες μπορούν να περιλαμβάνουν καταγραφής, αυτόματη επανάληψη, κ.λπ. Φίλτρα είναι αντικείμενα τα οποία υλοποίηση της μεθόδου με την υπογραφή:

    function handle (requestOptions, next)

Μετά την εκτέλεση τις προ-επεξεργασία αρχείου σχετικά με τις επιλογές αίτηση, τη μέθοδο πρέπει να καλέσετε "Επόμενο", που περνά μέσα επιστροφή κλήσης με την εξής υπογραφή:

    function (returnObject, finalCallback, next)

Με αυτήν την επιστροφή κλήσης, και μετά την επεξεργασία του returnObject (απάντηση από την αίτηση στο διακομιστή), η επιστροφή κλήσης πρέπει για κλήση Επόμενο, εάν υπάρχει να συνεχίσετε την επεξεργασία τα άλλα φίλτρα ή απλώς κλήση διαφορετικά finalCallback για να τερματίσετε την κλήση υπηρεσίας.

Με το Azure SDK για Node.js, **ExponentialRetryPolicyFilter** και **LinearRetryPolicyFilter**περιλαμβάνονται δύο φίλτρα που υλοποίηση της λογικής "Επανάληψη". Το ακόλουθο παράδειγμα δημιουργεί ένα αντικείμενο **TableService** που χρησιμοποιεί το **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα

Για να προσθέσετε μια οντότητα, πρέπει πρώτα να δημιουργήσετε ένα αντικείμενο το οποίο ορίζει ιδιότητες σας οντότητα. Όλα οντοτήτων πρέπει να περιέχει ένα **PartitionKey** και **RowKey**, που είναι μοναδικά αναγνωριστικά για την οντότητα.

* **PartitionKey** - καθορίζει τα διαμερίσματα που είναι αποθηκευμένη η οντότητα στο

* **RowKey** - προσδιορίζει με μοναδικό τρόπο η οντότητα εντός τα διαμερίσματα

**PartitionKey** και **RowKey** πρέπει να είναι τιμές συμβολοσειρών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση του μοντέλου δεδομένων υπηρεσίας του πίνακα](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Ακολουθεί ένα παράδειγμα καθορίζει μια οντότητα. Σημείωση η συγκεκριμένη **ημερομηνία λήξης** έχει οριστεί ως τύπος **Edm.DateTime**. Καθορίζει τον τύπο είναι προαιρετικό και τύπους θα είναι προκύπτουν αν δεν καθορίσατε.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] Υπάρχει επίσης ένα πεδίο **χρονικής σήμανσης** για κάθε εγγραφή, το οποίο έχει ρυθμιστεί Azure όταν μια οντότητα που έχει εισαχθεί ή ενημερώνονται.

Μπορείτε επίσης να χρησιμοποιήσετε το **entityGenerator** για τη δημιουργία οντοτήτων. Το παρακάτω παράδειγμα δημιουργεί την ίδια οντότητα εργασιών χρησιμοποιώντας το **entityGenerator**.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Για να προσθέσετε μια οντότητα στον πίνακά σας, μεταβιβάζουν το αντικείμενο οντότητα για τη μέθοδο **insertEntity** .

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Εάν η λειτουργία είναι επιτυχής, `result` θα περιέχει το [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) της εγγραφής που έχει εισαχθεί και `response` θα περιέχει πληροφορίες σχετικά με τη λειτουργία.

Παράδειγμα απόκριση:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Από προεπιλογή, **insertEntity** δεν επιστρέφει την οντότητα που έχει εισαχθεί ως μέρος του `response` πληροφορίες. Εάν σκοπεύετε να εκτελέσετε άλλες λειτουργίες σε αυτήν την οντότητα, ή θέλετε να προσωρινή αποθήκευση των πληροφοριών, μπορεί να είναι χρήσιμο για να την επιστρέφεται ως μέρος του `result`. Μπορείτε να το κάνετε ενεργοποιώντας **echoContent** ως εξής:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Ενημέρωση μιας οντότητας

Υπάρχουν πολλές μεθόδους που είναι διαθέσιμες για να ενημερώσετε μια υπάρχουσα οντότητα:

* **replaceEntity** - ενημερώνει μια υπάρχουσα οντότητα, αντικαθιστώντας το

* **mergeEntity** - ενημερώνει μια υπάρχουσα οντότητα κατά τη συγχώνευση νέες τιμές ιδιοτήτων σε την υπάρχουσα οντότητα

* **insertOrReplaceEntity** - ενημερώνει μια υπάρχουσα οντότητα, αντικαθιστώντας αυτό. Εάν υπάρχει καμία οντότητα, θα εισαχθεί ένα νέο

* **insertOrMergeEntity** - ενημερώνει μια υπάρχουσα οντότητα συγχώνευση νέες τιμές ιδιοτήτων στο την υπάρχουσα. Εάν υπάρχει καμία οντότητα, θα εισαχθεί ένα νέο

Το παρακάτω παράδειγμα παρουσιάζει ενημέρωση μια οντότητα χρησιμοποιώντας **replaceEntity**:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Από προεπιλογή, μια οντότητα ενημέρωση δεν ελέγχει για να δείτε αν τα δεδομένα που ενημερώνονται προηγουμένως έχει τροποποιηθεί από άλλη διεργασία. Για την υποστήριξη ταυτόχρονες ενημερώσεις:
>
> 1. Λάβετε το ETag του αντικειμένου που ενημερώνεται. Αυτό επιστρέφεται ως μέρος του `response` για κάθε οντότητα που σχετίζονται με τη λειτουργία και να μπορούν να ανακτηθούν έως `response['.metadata'].etag`.
>
> 2. Κατά την εκτέλεση μιας λειτουργίας ενημέρωσης σε μια οντότητα, προσθέστε τις πληροφορίες ETag προηγουμένως ανάκτηση για τη νέα οντότητα. Για παράδειγμα:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Εκτέλεση της λειτουργίας ενημέρωσης. Εάν η οντότητα έχει τροποποιηθεί μετά που ανακτήσατε την τιμή ETag, όπως μια άλλη παρουσία της εφαρμογής σας, μια `error` θα επιστραφούν που δηλώνει ότι η συνθήκη ενημέρωση που καθορίζεται στην αίτηση ήταν δεν πληρούνται.

Με **replaceEntity** και **mergeEntity**, εάν δεν υπάρχει η οντότητα που ενημερώνεται, στη συνέχεια, η λειτουργία update θα αποτύχει. Επομένως, εάν θέλετε να αποθηκεύσετε μια οντότητα ανεξάρτητα από αν υπάρχει ήδη, χρησιμοποιήστε **insertOrReplaceEntity** ή **insertOrMergeEntity**.

Το `result` για ενημέρωση με επιτυχία λειτουργίες θα περιέχει το **Etag** της ενημερωμένης οντότητας.

## <a name="work-with-groups-of-entities"></a>Εργασία με ομάδες οντοτήτων

Μερικές φορές έχει νόημα να υποβάλετε πολλές λειτουργίες μαζί σε μια δέσμη για να βεβαιωθείτε ότι ατομικής επεξεργασία από το διακομιστή. Για να πραγματοποιήσετε που, χρησιμοποιήστε την κλάση **TableBatch** για να δημιουργήσετε μια δέσμη και, στη συνέχεια, χρησιμοποιήστε τη μέθοδο **executeBatch** του **TableService** για να εκτελέσετε τις λειτουργίες μαζικής.

 Το παρακάτω παράδειγμα παρουσιάζει δύο οντοτήτων σε μια δέσμη υποβολή:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Για λειτουργίες επιτυχημένη μαζικής επεξεργασίας, `result` θα περιέχει πληροφορίες για κάθε εργασία στη δέσμη.

### <a name="work-with-batched-operations"></a>Εργασία με λειτουργίες δέσμης

Λειτουργίες που έχουν προστεθεί σε μια δέσμη μπορούν να ελέγχονται από την προβολή του `operations` την ιδιότητα. Μπορείτε επίσης να χρησιμοποιήσετε τις παρακάτω μεθόδους για να εργαστείτε με τις λειτουργίες:

* **καταργήστε την επιλογή** - καταργεί όλες οι λειτουργίες από μια δέσμη

* **getOperations** - λαμβάνει μια πράξη από τη δέσμη

* **hasOperations** - επιστρέφει true εάν η δέσμη περιέχει λειτουργίες

* **removeOperations** - καταργεί μια λειτουργία

* **μέγεθος** - επιστρέφει τον αριθμό των λειτουργιών στη δέσμη

## <a name="retrieve-an-entity-by-key"></a>Ανάκτηση μιας οντότητας με αριθμό-κλειδί

Για να επιστρέψετε μια συγκεκριμένη οντότητα με βάση τις **PartitionKey** και **RowKey**, χρησιμοποιήστε τη μέθοδο **retrieveEntity** .

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Όταν ολοκληρωθεί, αυτή η λειτουργία `result` θα περιέχει η οντότητα.

## <a name="query-a-set-of-entities"></a>Ένα σύνολο οντοτήτων ερωτήματος

Για να το ερώτημα σε έναν πίνακα, χρησιμοποιήστε το αντικείμενο **TableQuery** για να δημιουργήσετε μια παράσταση ερωτήματος με τους ακόλουθους όρους:

* **Επιλέξτε** - τα πεδία που θα επιστραφεί από το ερώτημα

* **όπου** - το σημείο όπου όρος

    * **και** - μια `and` συνθήκη where

    * **ή** - μια `or` συνθήκη where

* **επάνω** - τον αριθμό των στοιχείων για τη λήψη


Το παρακάτω παράδειγμα δημιουργεί ένα ερώτημα που θα επιστρέψει τα πρώτα πέντε στοιχεία με μια PartitionKey της 'hometasks'.

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

Εφόσον δεν χρησιμοποιείται η **επιλογή** , όλα τα πεδία θα επιστραφεί. Για να εκτελέσετε το ερώτημα σε σχέση με έναν πίνακα, χρησιμοποιήστε **queryEntities**. Το παρακάτω παράδειγμα χρησιμοποιεί αυτό το ερώτημα για να επιστρέψει οντοτήτων από 'Πίνακας'.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Εάν είναι επιτυχής, `result.entries` θα περιέχει έναν πίνακα οντοτήτων που ταιριάζουν με το ερώτημα. Εάν το ερώτημα δεν ήταν δυνατό να επιστρέψει όλες οντοτήτων, `result.continuationToken` θα είναι μη*null* και μπορούν να χρησιμοποιηθούν ως η τρίτη παράμετρος της **queryEntities** για να ανακτήσετε περισσότερα αποτελέσματα. Για το αρχικό ερώτημα, χρησιμοποιήστε τη συνάρτηση *null* για την τρίτη παράμετρο.

### <a name="query-a-subset-of-entity-properties"></a>Ένα υποσύνολο οντότητα ιδιοτήτων ερωτήματος

Ένα ερώτημα σε έναν πίνακα μπορεί να ανακτήσει λίγα πεδία από μια οντότητα.
Αυτό μειώνει το εύρος ζώνης και να βελτιώσετε τις επιδόσεις του ερωτήματος, ειδικά για μεγάλη οντοτήτων. Χρησιμοποιήστε τον όρο **Επιλέξτε** και μεταβιβάζουν τα ονόματα των πεδίων που θα επιστραφεί. Για παράδειγμα, το ακόλουθο ερώτημα θα επιστρέψει μόνο τα πεδία **Περιγραφή** και **ημερομηνία λήξης** .

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Διαγραφή οντότητας

Μπορείτε να διαγράψετε μια οντότητα χρησιμοποιώντας τα πλήκτρα διαμερίσματα και της γραμμής. Σε αυτό το παράδειγμα, το αντικείμενο **task1** περιέχει τις τιμές **RowKey** και **PartitionKey** της οντότητας θα διαγραφεί. Στη συνέχεια, το αντικείμενο που του μεταβιβάστηκε η μέθοδος **deleteEntity** .

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Εξετάστε το ενδεχόμενο χρήσης ETags κατά τη διαγραφή στοιχείων, για να βεβαιωθείτε ότι το στοιχείο δεν έχει τροποποιηθεί από άλλη διεργασία. Για πληροφορίες σχετικά με τη χρήση ETags, ανατρέξτε στο θέμα [Ενημέρωση μια οντότητα](#update-an-entity) .

## <a name="delete-a-table"></a>Διαγραφή ενός πίνακα

Ο ακόλουθος κώδικας Διαγράφει έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Εάν δεν είστε βέβαιοι εάν ο πίνακας υπάρχει, χρησιμοποιήστε **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Χρησιμοποιήστε τα διακριτικά συνέχισης

Όταν ερωτήματός σας πίνακες για μεγάλες ποσότητες των αποτελεσμάτων, αναζητήστε τα διακριτικά συνέχισης. Μπορεί να υπάρχουν μεγάλες ποσότητες δεδομένων διαθέσιμη για το ερώτημα που ενδέχεται να μην συνειδητοποιήσετε ότι εάν δεν δημιουργείτε την αναγνώριση όταν υπάρχει ένα διακριτικό συνέχισης.

Αντικείμενο τα αποτελέσματα επιστρέφονται κατά την υποβολή ερωτημάτων σύνολα οντοτήτων μια `continuationToken` ιδιότητα όταν υπάρχει όπως ενός διακριτικού. Μπορείτε να χρησιμοποιήσετε αυτό κατά την εκτέλεση ενός ερωτήματος για να συνεχίσετε τη μετακίνηση μεταξύ των οντοτήτων διαμερίσματα και πίνακα.

Κατά την υποβολή ερωτήματος, μπορεί να παρέχεται μια παράμετρος continuationToken μεταξύ την παρουσία του αντικειμένου ερωτήματος και η συνάρτηση επιστροφής κλήσης:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Εάν μπορείτε να ελέγξετε το `continuationToken` αντικείμενο, θα βρείτε ιδιότητες όπως `nextPartitionKey`, `nextRowKey` και `targetLocation`, που μπορούν να χρησιμοποιηθούν για να επαναλάβετε σε όλα τα αποτελέσματα.

Υπάρχει επίσης ένα δείγμα συνέχισης εντός του Azure αποθήκευσης Node.js repo σε GitHub. Αναζητήστε `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Εργασία με κοινόχρηστη πρόσβαση υπογραφών

Κοινόχρηστη πρόσβαση υπογραφές (συσχετισμών Ασφαλείας) είναι ένας ασφαλής τρόπος για την παροχή λεπτομερούς πρόσβαση στους πίνακες χωρίς να παρέχετε το όνομα του λογαριασμού χώρου αποθήκευσης ή πλήκτρα. Συσχετίσεις Ασφαλείας συχνά χρησιμοποιούνται για την παροχή περιορισμένη πρόσβαση στα δεδομένα σας, όπως παρέχει μια εφαρμογή για κινητές συσκευές με εγγραφές ερωτήματος.

Μια αξιόπιστη εφαρμογή, όπως μια υπηρεσία που βασίζεται στο cloud δημιουργεί μια συσχετισμών Ασφαλείας χρησιμοποιώντας το **generateSharedAccessSignature** από το **TableService**και παρέχει ένα μη αξιόπιστα ή ημι-αξιόπιστη μια εφαρμογή όπως εφαρμογής για κινητές συσκευές. Τις συσχετίσεις Ασφαλείας δημιουργούνται χρησιμοποιώντας μια πολιτική, η οποία περιγράφει τις ημερομηνίες έναρξης και λήξης κατά την οποία είναι έγκυρη τις συσχετίσεις Ασφαλείας, καθώς και το επίπεδο πρόσβασης που έχει εκχωρηθεί στον κάτοχο συσχετισμών Ασφαλείας.

Το παρακάτω παράδειγμα δημιουργεί μια νέα πολιτική κοινόχρηστο πρόσβασης που θα σας επιτρέψει ο κάτοχος συσχετισμών Ασφαλείας ερώτημα ("r") στον πίνακα και λήξη 100 λεπτά μετά την ώρα που έχει δημιουργηθεί.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Σημειώστε ότι οι πληροφορίες του κεντρικού υπολογιστή πρέπει να δοθεί επίσης, όπως απαιτείται όταν ο κάτοχος συσχετισμών Ασφαλείας επιχειρεί να αποκτήσετε πρόσβαση στον πίνακα.

Στη συνέχεια, την εφαρμογή-πελάτη χρησιμοποιεί τις συσχετίσεις Ασφαλείας με **TableServiceWithSAS** για την εκτέλεση λειτουργιών σε σχέση με τον πίνακα. Το παρακάτω παράδειγμα συνδέεται με τον πίνακα και εκτελεί ένα ερώτημα.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Επειδή το συσχετισμών Ασφαλείας δημιουργήθηκε με το ερώτημα access, εάν η προσπάθεια πραγματοποιήθηκαν για εισαγωγή, ενημέρωση ή διαγραφή οντοτήτων, θα επιστραφεί ένα σφάλμα.

### <a name="access-control-lists"></a>Λίστες ελέγχου πρόσβασης

Μπορείτε επίσης να χρησιμοποιήσετε μια λίστα ελέγχου πρόσβασης (ACL) για να ορίσετε την πολιτική πρόσβασης για μια συσχετισμούς Ασφαλείας. Αυτό είναι χρήσιμο εάν θέλετε για να επιτρέψετε σε πολλούς υπολογιστές-πελάτες για να αποκτήσετε πρόσβαση στον πίνακα, αλλά παρέχει πολιτικές πρόσβασης διαφορετικό για κάθε πελάτη.

Ένα ACL έχει υλοποιηθεί με χρήση ενός πίνακα της access πολιτικές, με το Αναγνωριστικό που σχετίζονται με κάθε πολιτικής. Το παρακάτω παράδειγμα καθορίζει δύο πολιτικές, μία για το 'Χρήστη1' και μία για 'user2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Το παρακάτω παράδειγμα λαμβάνει την τρέχουσα ACL για τον πίνακα **hometasks** και, στη συνέχεια, προσθέτει τις νέες πολιτικές που χρησιμοποιεί **setTableAcl**. Αυτή η προσέγγιση σας επιτρέπει:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Όταν έχει οριστεί η ACL, μπορείτε να δημιουργήσετε μια συσχετισμών Ασφαλείας με βάση το Αναγνωριστικό για μια πολιτική. Το παρακάτω παράδειγμα δημιουργεί μια νέα συσχετισμών Ασφαλείας για 'user2':

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους.

-   [Ιστολόγιο ομάδας azure χώρου αποθήκευσης][].
-   Αρχείο φύλαξης [Azure SDK χώρου αποθήκευσης για κόμβο][] στο GitHub.
-   [Κέντρο για προγραμματιστές του node.js](/develop/nodejs/)

  [Azure αποθήκευσης SDK για κόμβου]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js εφαρμογής web με την υπηρεσία πίνακα Azure]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md

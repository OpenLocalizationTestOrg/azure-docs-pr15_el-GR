<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από Node.js | Microsoft Azure"
    description="Αποθήκευση μη δομημένα δεδομένα στο cloud με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων)."
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



# <a name="how-to-use-blob-storage-from-nodejs"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από Node.js

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το άρθρο σάς δείχνει πώς να εκτελείτε συνηθισμένα σενάρια χρησιμοποιώντας χώρο αποθήκευσης αντικειμένων Blob. Τα δείγματα εγγράφονται μέσω του API Node.js. Τα σενάρια καλύπτεται περιλαμβάνουν πώς μπορείτε να αποστείλετε, λίστα, κάντε λήψη και διαγραφή αντικειμένων blob.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Δημιουργία μιας εφαρμογής Node.js

Για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε μια εφαρμογή Node.js, ανατρέξτε στο θέμα [Δημιουργία μια εφαρμογή web Node.js στο Azure εφαρμογής υπηρεσίας], [Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure] --με τη χρήση του Windows PowerShell ή [Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js να Azure χρήση πίνακα Web].

## <a name="configure-your-application-to-access-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να αποκτήσετε πρόσβαση χώρου αποθήκευσης

Για να χρησιμοποιήσετε Azure χώρου αποθήκευσης, χρειάζεστε το Azure SDK χώρου αποθήκευσης για Node.js, η οποία περιλαμβάνει ένα σύνολο βιβλιοθηκών ευκολία που επικοινωνία με τις υπηρεσίες ΥΠΌΛΟΙΠΟ του χώρου αποθήκευσης.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Χρήση διαχείρισης πακέτου κόμβου (NPM) για να αποκτήσετε το πακέτο

1.  Χρησιμοποιήστε ένα περιβάλλον γραμμής εντολών όπως **PowerShell** (Windows), το **Terminal** (Mac) ή το **πάρτι** (Unix), για να μεταβείτε στο φάκελο όπου δημιουργήσατε την εφαρμογή του δείγματος.

2.  Πληκτρολογήστε **npm εγκατάσταση αποθήκευσης azure** στο παράθυρο εντολών. Έξοδος από την εντολή είναι παρόμοιο με το ακόλουθο παράδειγμα κώδικα.

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

3.  Μπορείτε να εκτελέσετε με μη αυτόματο τρόπο την εντολή **ls** για να επιβεβαιώσετε ότι ένα **κόμβου\_λειτουργικές μονάδες** που δημιουργήθηκε το φάκελο. Μέσα σε αυτόν το φάκελο, βρείτε το πακέτο **αποθήκευσης azure** , η οποία περιέχει τις βιβλιοθήκες που χρειάζεστε για να αποκτήσετε πρόσβαση χώρου αποθήκευσης.

### <a name="import-the-package"></a>Εισαγωγή του πακέτου

Χρησιμοποιείτε το Σημειωματάριο ή κάποιο άλλο πρόγραμμα επεξεργασίας κειμένου, προσθέστε τα ακόλουθα στο επάνω μέρος του αρχείου **server.js** της εφαρμογής όπου σκοπεύετε να χρησιμοποιήσετε αποθήκευσης:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Ρύθμιση σύνδεσης αποθήκευσης Azure

Η λειτουργική μονάδα Azure θα διαβάσει τις μεταβλητές περιβάλλοντος `AZURE_STORAGE_ACCOUNT` και `AZURE_STORAGE_ACCESS_KEY`, ή `AZURE_STORAGE_CONNECTION_STRING`, για τις πληροφορίες που απαιτούνται για να συνδεθείτε στο λογαριασμό σας Azure χώρου αποθήκευσης. Εάν δεν έχουν οριστεί αυτές τις μεταβλητές περιβάλλοντος, πρέπει να καθορίσετε τις πληροφορίες λογαριασμού κατά την κλήση **createBlobService**.

Ένα παράδειγμα για τη ρύθμιση των μεταβλητών περιβάλλοντος στην [Πύλη του Azure](https://portal.azure.com) για μια εφαρμογή Azure web, ανατρέξτε στο θέμα [Node.js εφαρμογής web με την υπηρεσία πίνακα Azure].

## <a name="create-a-container"></a>Δημιουργία κοντέινερ

Το αντικείμενο **BlobService** σάς επιτρέπει να εργάζεστε με το κοντέινερ και αντικείμενα blob. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **BlobService** . Προσθέστε τα ακόλουθα κοντά στο επάνω μέρος **server.js**:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Μπορείτε να αποκτήσετε πρόσβαση σε ένα αντικείμενο blob ανώνυμα χρησιμοποιώντας **createBlobServiceAnonymous** και παρέχει τη διεύθυνση κεντρικού υπολογιστή. Για παράδειγμα, χρησιμοποιήστε `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Για να δημιουργήσετε ένα νέο κοντέινερ, χρησιμοποιήστε **createContainerIfNotExists**. Το ακόλουθο παράδειγμα κώδικα δημιουργεί ένα νέο κοντέινερ που ονομάζεται 'mycontainer':

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Εάν το κοντέινερ που μόλις δημιουργήθηκε, `result.created` είναι αληθής. Εάν υπάρχει ήδη στο κοντέινερ, `result.created` είναι ψευδής. `response`περιέχει πληροφορίες σχετικά με τη λειτουργία, συμπεριλαμβανομένων των πληροφοριών ETag για το κοντέινερ.

### <a name="container-security"></a>Κοντέινερ ασφαλείας

Από προεπιλογή, το νέο κοντέινερ είναι ιδιωτικά και δεν είναι δυνατή η πρόσβαση ανώνυμα. Για να κάνετε το κοντέινερ δημόσια, έτσι ώστε να μπορούν να έχουν πρόσβαση ανώνυμα, μπορείτε να ορίσετε το επίπεδο πρόσβασης στο κοντέινερ **αντικειμένων blob** ή **κοντέινερ**.

* **blob** - επιτρέπει την ανώνυμη πρόσβαση για ανάγνωση με το περιεχόμενο blob και μετα-δεδομένων μέσα σε αυτό το κοντέινερ, αλλά όχι σε κοντέινερ μετα-δεδομένων όπως η καταχώρηση όλων των BLOB μέσα σε ένα κοντέινερ

* **κοντέινερ** - επιτρέπει την ανώνυμη πρόσβαση ανάγνωση blob περιεχόμενο και μετα-δεδομένα, καθώς και κοντέινερ μετα-δεδομένων

Το ακόλουθο παράδειγμα κώδικα παρουσιάζει ορίζοντας το επίπεδο πρόσβασης σε **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Εναλλακτικά, μπορείτε να τροποποιήσετε το επίπεδο πρόσβασης ένα κοντέινερ, χρησιμοποιώντας **setContainerAcl** για να καθορίσετε το επίπεδο πρόσβασης. Το ακόλουθο παράδειγμα κώδικα αλλάζει το επίπεδο πρόσβασης σε κοντέινερ:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Το αποτέλεσμα περιέχει πληροφορίες σχετικά με τη λειτουργία, όπως η τρέχουσα **ETag** για το κοντέινερ.

### <a name="filters"></a>Φίλτρα

Μπορείτε να εφαρμόσετε προαιρετικό λειτουργίες φιλτραρίσματος σε λειτουργίες που εκτελούνται με **BlobService**. Φιλτράρισμα λειτουργίες μπορούν να περιλαμβάνουν καταγραφής, αυτόματη επανάληψη, κ.λπ. Φίλτρα είναι αντικείμενα τα οποία υλοποίηση της μεθόδου με την υπογραφή:

    function handle (requestOptions, next)

Μετά την εκτέλεση τις προ-επεξεργασία αρχείου σχετικά με τις επιλογές αίτηση, τη μέθοδο πρέπει να καλέσετε "Επόμενο", που περνά μέσα επιστροφή κλήσης με την εξής υπογραφή:

    function (returnObject, finalCallback, next)

Με αυτήν την επιστροφή κλήσης, και μετά την επεξεργασία του returnObject (απάντηση από την αίτηση στο διακομιστή), η επιστροφή κλήσης πρέπει για κλήση Επόμενο, εάν υπάρχει να συνεχίσετε την επεξεργασία τα άλλα φίλτρα ή απλώς κλήση finalCallback για να τερματίσετε την κλήση υπηρεσίας.

Με το Azure SDK για Node.js, **ExponentialRetryPolicyFilter** και **LinearRetryPolicyFilter**περιλαμβάνονται δύο φίλτρα που υλοποίηση της λογικής "Επανάληψη". Το ακόλουθο παράδειγμα δημιουργεί ένα αντικείμενο **BlobService** που χρησιμοποιεί το **ExponentialRetryPolicyFilter**:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ

Υπάρχουν τρεις τύποι αντικειμένων blob: Αποκλεισμός αντικείμενα blob, σελίδα αντικείμενα BLOB και προσάρτηση αντικείμενα blob. Αντικείμενα BLOB μπλοκ επιτρέπουν με πιο αποτελεσματικό τρόπο αποστολή μεγάλου όγκου δεδομένων. Προσάρτηση αντικείμενα BLOB είναι βελτιστοποιημένη για λειτουργία προσάρτησης. Αντικείμενα BLOB σελίδας είναι βελτιστοποιημένη για λειτουργίες ανάγνωσης/εγγραφής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατανόηση των αντικειμένων blob μπλοκ, αντικείμενα BLOB Προσάρτηση, και αντικείμενα BLOB σελίδας](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Αντικείμενα BLOB μπλοκ

Για να αποστείλετε δεδομένα σε ένα μπλοκ blob, χρησιμοποιήστε τα εξής:

* **createBlockBlobFromLocalFile** - δημιουργεί ένα νέο blob μπλοκ και αποστέλλει τα περιεχόμενα του αρχείου

* **createBlockBlobFromStream** - δημιουργεί ένα νέο blob μπλοκ και αποστέλλει τα περιεχόμενα μιας ροής

* **createBlockBlobFromText** - δημιουργεί ένα νέο blob μπλοκ και κάνει αποστολή των περιεχομένων μιας συμβολοσειράς

* **createWriteStreamToBlockBlob** - παρέχει μια ροή εγγραφής σε ένα μπλοκ blob

Το ακόλουθο παράδειγμα κώδικα αποστέλλει τα περιεχόμενα του αρχείου **test.txt** σε **myblob**.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Το `result` επιστρέφεται από τις παρακάτω μεθόδους περιέχει πληροφορίες σχετικά με τη λειτουργία, όπως το **ETag** από το αντικείμενο blob.

### <a name="append-blobs"></a>Προσάρτηση αντικείμενα BLOB

Για να αποστείλετε δεδομένα σε ένα νέο blob προσάρτησης, χρησιμοποιήστε τα εξής:

* **createAppendBlobFromLocalFile** - δημιουργεί ένα νέο blob προσάρτησης και αποστέλλει τα περιεχόμενα του αρχείου

* **createAppendBlobFromStream** - δημιουργεί ένα νέο blob προσάρτησης και αποστέλλει τα περιεχόμενα μιας ροής

* **createAppendBlobFromText** - δημιουργεί ένα νέο blob προσάρτησης και κάνει αποστολή των περιεχομένων μιας συμβολοσειράς

* **createWriteStreamToNewAppendBlob** - δημιουργεί ένα νέο blob προσάρτησης και, στη συνέχεια, παρέχει μια ροή για την εγγραφή σε αυτήν

Το ακόλουθο παράδειγμα κώδικα αποστέλλει τα περιεχόμενα του αρχείου **test.txt** σε **myappendblob**.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Για να προσαρτήσετε ένα μπλοκ σε μια υπάρχουσα blob προσάρτησης, χρησιμοποιήστε τα εξής:

* **appendFromLocalFile** - Προσάρτηση τα περιεχόμενα του αρχείου σε μια υπάρχουσα blob προσάρτησης

* **appendFromStream** - προσάρτηση των περιεχομένων μιας ροής σε μια υπάρχουσα blob προσάρτησης

* **appendFromText** - προσάρτηση των περιεχομένων μιας ακολουθίας χαρακτήρων σε μια υπάρχουσα blob προσάρτησης

* **appendBlockFromStream** - προσάρτηση των περιεχομένων μιας ροής σε μια υπάρχουσα blob προσάρτησης

* **appendBlockFromText** - προσάρτηση των περιεχομένων μιας ακολουθίας χαρακτήρων σε μια υπάρχουσα blob προσάρτησης

> [AZURE.NOTE] appendFromXXX APIs θα κάνετε ορισμένες επικύρωσης πλευρά του προγράμματος-πελάτη αποτυχία γρήγορα για να αποφύγετε την κλήση διακομιστή unncessary. δεν θα appendBlockFromXXX.

Το ακόλουθο παράδειγμα κώδικα αποστέλλει τα περιεχόμενα του αρχείου **test.txt** σε **myappendblob**.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Αντικείμενα BLOB σελίδας

Για την αποστολή δεδομένων σε μια σελίδα blob, χρησιμοποιήστε τα εξής:

* **createPageBlob** - δημιουργεί ένα νέο blob σελίδας του συγκεκριμένου μήκους

* **createPageBlobFromLocalFile** - δημιουργεί μια νέα σελίδα blob και αποστέλλει τα περιεχόμενα του αρχείου

* **createPageBlobFromStream** - δημιουργεί μια νέα σελίδα blob και αποστέλλει τα περιεχόμενα μιας ροής

* **createWriteStreamToExistingPageBlob** - παρέχει μια ροή εγγραφής σε μια υπάρχουσα blob σελίδας

* **createWriteStreamToNewPageBlob** - δημιουργεί ένα νέο blob σελίδας και, στη συνέχεια, παρέχει μια ροή για την εγγραφή σε αυτήν

Το ακόλουθο παράδειγμα κώδικα αποστέλλει τα περιεχόμενα του αρχείου **test.txt** σε **mypageblob**.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Σελίδα αντικείμενα BLOB αποτελείται από 512-byte 'σελίδες'. Θα λάβετε ένα μήνυμα σφάλματος κατά την αποστολή δεδομένων με μέγεθος που δεν είναι πολλαπλάσιο του 512.

## <a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ

Για να παραθέσετε τα αντικείμενα BLOB σε ένα κοντέινερ, χρησιμοποιήστε τη μέθοδο **listBlobsSegmented** . Εάν θέλετε να επιστρέψετε αντικείμενα blob με ένα συγκεκριμένο πρόθεμα, χρησιμοποιήστε **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

Το `result` περιέχει ένα `entries` συλλογή, η οποία είναι ένας πίνακας με αντικείμενα που περιγράφουν κάθε blob. Εάν δεν μπορεί να επιστραφεί όλων των BLOB, το `result` παρέχει επίσης ένα `continuationToken`, που μπορείτε να χρησιμοποιήσετε ως η δεύτερη παράμετρος για την ανάκτηση πρόσθετες καταχωρήσεις.

## <a name="download-blobs"></a>Λήψη αντικείμενα BLOB

Για να κάνετε λήψη δεδομένων από ένα blob, χρησιμοποιήστε τα εξής:

* **getBlobToLocalFile** - γράφει τα περιεχόμενα blob στο αρχείο

* **getBlobToStream** - συντάσσει τα περιεχόμενα blob μια ροή

* **getBlobToText** - γράφει τα περιεχόμενα blob σε μια συμβολοσειρά

* **createReadStream** - παρέχει μια ροή ανάγνωσης από το αντικείμενο blob

Το ακόλουθο παράδειγμα κώδικα παρουσιάζει τη χρήση **getBlobToStream** να κάνετε λήψη των περιεχομένων του blob **myblob** και να αποθηκεύσετε το αρχείο **output.txt** χρησιμοποιώντας μια ροή:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

Το `result` περιέχει πληροφορίες σχετικά με το αντικείμενο blob, συμπεριλαμβανομένων πληροφοριών **ETag** .

## <a name="delete-a-blob"></a>Διαγράψτε ένα blob

Τέλος, για να διαγράψετε ένα blob, καλέστε **deleteBlob**. Το ακόλουθο παράδειγμα κώδικα διαγράφει το αντικείμενο blob με το όνομα **myblob**.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Ταυτόχρονη πρόσβαση

Για την υποστήριξη ταυτόχρονη πρόσβαση σε ένα αντικείμενο blob από πολλά προγράμματα-πελάτες ή πολλές παρουσίες διαδικασία, μπορείτε να χρησιμοποιήσετε **ETags** ή **μισθώσεις**.

* **Etag** - παρέχει έναν τρόπο για να εντοπίσετε που το αντικείμενο blob ή κοντέινερ έχει τροποποιηθεί από άλλη διεργασία

* **Εκμίσθωση** - παρέχει ένας τρόπος για να αποκτήσετε αποκλειστική, ανανεωθεί, εγγραφή ή διαγραφή πρόσβαση σε ένα αντικείμενο blob για μια χρονική περίοδο

### <a name="etag"></a>ETag

Χρήση ETags Εάν χρειάζεστε για να επιτρέψετε σε πολλούς υπολογιστές-πελάτες ή παρουσίες για να γράψετε στο μπλοκ Blob ή μια σελίδα Blob ταυτόχρονα. Το ETag σάς επιτρέπει να προσδιορίσετε αν το κοντέινερ ή blob τροποποιήθηκε από αρχικά ανάγνωση ή την δημιούργησε, που σας επιτρέπει να αποφύγετε την αντικατάσταση των αλλαγών δεσμευμένου από άλλο πρόγραμμα-πελάτη ή διεργασία.

Μπορείτε να ορίσετε ETag συνθήκες, χρησιμοποιώντας την προαιρετική `options.accessConditions` παραμέτρου. Το ακόλουθο παράδειγμα κώδικα αποστέλλει μόνο το αρχείο **test.txt** εάν υπάρχει ήδη το αντικείμενο blob και έχει την τιμή ETag που περιέχονται `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Όταν χρησιμοποιείτε το ETags, το μοτίβο γενικά είναι:

1. Αποκτήστε το ETag ως αποτέλεσμα ένα δημιουργία, τη λίστα ή λειτουργία λήψης.

2. Εκτελεί μια ενέργεια, τον έλεγχο ότι η τιμή ETag δεν έχει τροποποιηθεί.

Εάν η τιμή έχει τροποποιηθεί, αυτό υποδεικνύει ότι ένα άλλο πρόγραμμα-πελάτη ή παρουσία τροποποιηθεί την blob ή το κοντέινερ εφόσον έχετε λάβει την τιμή ETag.

### <a name="lease"></a>Εκμίσθωση

Μπορείτε να αποκτήσετε μια νέα μίσθωση, χρησιμοποιώντας τη μέθοδο **acquireLease** , καθορίζοντας την blob ή το κοντέινερ που θέλετε να αποκτήσετε μια μίσθωση σε. Για παράδειγμα, ο ακόλουθος κώδικας αποκτά μια εκμίσθωση **myblob**.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Πρέπει να παρέχουν οι επόμενες λειτουργίες σε **myblob** το `options.leaseId` παραμέτρου. Η μίσθωση Αναγνωριστικό επιστρέφεται ως `result.id` από **acquireLease**.

> [AZURE.NOTE] Από προεπιλογή, η διάρκεια μίσθωσης είναι απεριόριστο. Μπορείτε να καθορίσετε μια διάρκεια μη άπειρο (από 15 έως 60 δευτερόλεπτα), παρέχοντας την `options.leaseDuration` παραμέτρου.

Για να καταργήσετε μια μίσθωση, χρησιμοποιήστε **releaseLease**. Για να διακόψετε μια μίσθωση, αλλά εμποδίζετε άλλους χρήστες από το να αποκτήσει μια νέα μίσθωση μέχρι να έχει λήξει η αρχική διάρκεια, χρησιμοποιήστε **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Εργασία με κοινόχρηστη πρόσβαση υπογραφών

Κοινόχρηστη πρόσβαση υπογραφές (συσχετισμών Ασφαλείας) είναι ένας ασφαλής τρόπος για την παροχή λεπτομερούς πρόσβαση σε κοντέινερ και αντικείμενα BLOB χωρίς να παρέχετε το όνομα του λογαριασμού χώρου αποθήκευσης ή πλήκτρα. Κοινόχρηστη πρόσβαση υπογραφές συχνά χρησιμοποιούνται για την παροχή περιορισμένη πρόσβαση στα δεδομένα σας, όπως παρέχει μια εφαρμογή για κινητές συσκευές για να αποκτήσετε πρόσβαση σε αντικείμενα blob.

> [AZURE.NOTE] Ενώ μπορείτε επίσης να επιτρέψετε ανώνυμη πρόσβαση σε αντικείμενα blob, υπογραφές κοινόχρηστη πρόσβαση σάς επιτρέπουν να παρέχουν περισσότερες ελεγχόμενη πρόσβαση, όπως χρειάζεται να δημιουργήσετε τις συσχετίσεις Ασφαλείας.

Μια αξιόπιστη εφαρμογή, όπως μια υπηρεσία που βασίζεται στο cloud δημιουργεί υπογραφών κοινόχρηστη πρόσβαση με τη χρήση του **generateSharedAccessSignature** από το **BlobService**και παρέχει σε ένα μη αξιόπιστα ή ημι-αξιόπιστη μια εφαρμογή όπως εφαρμογής για κινητές συσκευές. Κοινή χρήση πρόσβασης υπογραφές δημιουργούνται χρησιμοποιώντας μια πολιτική, η οποία περιγράφει την έναρξη και τερματισμός ημερομηνίες κατά τις οποίες οι υπογραφές κοινόχρηστη πρόσβαση είναι έγκυρη, καθώς και τα εκχωρηθεί το επίπεδο πρόσβασης στον κάτοχο υπογραφές κοινόχρηστη πρόσβαση.

Το ακόλουθο παράδειγμα κώδικα δημιουργεί μια νέα πολιτική κοινόχρηστο πρόσβασης που επιτρέπει την υποδοχή υπογραφές κοινόχρηστη πρόσβαση για την εκτέλεση λειτουργιών ανάγνωσης στο το blob **myblob** και λήξη 100 λεπτά μετά την ώρα που έχει δημιουργηθεί.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Σημειώστε ότι οι πληροφορίες του κεντρικού υπολογιστή πρέπει να δοθεί επίσης, όπως απαιτείται όταν ο κάτοχος υπογραφές κοινόχρηστη πρόσβαση προσπαθεί να αποκτήσει πρόσβαση στο κοντέινερ.

Στη συνέχεια, την εφαρμογή-πελάτη χρησιμοποιεί υπογραφές κοινόχρηστη πρόσβαση με **BlobServiceWithSAS** για την εκτέλεση λειτουργιών σε σχέση με το αντικείμενο blob. Το παρακάτω λαμβάνει πληροφορίες σχετικά με την **myblob**.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Επειδή οι υπογραφές κοινόχρηστη πρόσβαση δημιουργήθηκαν με την πρόσβαση μόνο για ανάγνωση, εάν γίνει προσπάθεια για να τροποποιήσετε το αντικείμενο blob, θα επιστραφεί ένα σφάλμα.

### <a name="access-control-lists"></a>Λίστες ελέγχου πρόσβασης

Μπορείτε επίσης να χρησιμοποιήσετε μια λίστα ελέγχου πρόσβασης (ACL) για να ορίσετε την πολιτική πρόσβασης για συσχετισμούς Ασφαλείας. Αυτό είναι χρήσιμο εάν θέλετε για να επιτρέψετε σε πολλούς υπολογιστές-πελάτες για να αποκτήσετε πρόσβαση σε ένα κοντέινερ, αλλά παρέχουν πολιτικές πρόσβασης διαφορετικό για κάθε πελάτη.

Ένα ACL έχει υλοποιηθεί με χρήση ενός πίνακα της access πολιτικές, με το Αναγνωριστικό που σχετίζονται με κάθε πολιτικής. Το ακόλουθο παράδειγμα κώδικα ορίζει δύο πολιτικές, μία για το 'Χρήστη1' και μία για 'user2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Το ακόλουθο παράδειγμα κώδικα λαμβάνει την τρέχουσα ACL για **mycontainer**και, στη συνέχεια, προσθέτει τις νέες πολιτικές που χρησιμοποιεί **setBlobAcl**. Αυτή η προσέγγιση σας επιτρέπει:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Αφού ορίσετε το ACL, στη συνέχεια, μπορείτε να δημιουργήσετε υπογραφές κοινόχρηστη πρόσβαση με βάση το Αναγνωριστικό για μια πολιτική. Το ακόλουθο παράδειγμα κώδικα δημιουργεί νέα κοινόχρηστη πρόσβαση υπογραφές για 'user2':

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους.

-   [Azure αποθήκευσης SDK για αναφορά API κόμβου][]
-   [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης][]
-   Αρχείο φύλαξης [Azure SDK χώρου αποθήκευσης για κόμβο][] στο GitHub
-   [Κέντρο για προγραμματιστές του node.js](/develop/nodejs/)
-   [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)

[Azure αποθήκευσης SDK για κόμβου]: https://github.com/Azure/azure-storage-node

[Δημιουργία εφαρμογής web Node.js στο Azure εφαρμογής υπηρεσίας]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Node.js εφαρμογής web με την υπηρεσία πίνακα Azure]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Δημιουργήστε και αναπτύξτε μια εφαρμογή web Node.js στον Azure χρήση πίνακα Web]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Δημιουργήστε και αναπτύξτε μια εφαρμογή Node.js σε μια υπηρεσία Cloud Azure]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure αποθήκευσης SDK για αναφορά API κόμβου]: http://dl.windowsazure.com/nodestoragedocs/index.html

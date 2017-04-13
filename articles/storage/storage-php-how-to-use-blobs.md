<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob (χώρος αποθήκευσης αντικειμένων) από PHP | Microsoft Azure"
    description="Αποθήκευση μη δομημένα δεδομένα στο cloud με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob από PHP

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία που αποθηκεύει μη δομημένα δεδομένα στο cloud ως αντικείμενα/αντικείμενα blob. Χώρος αποθήκευσης αντικειμένων blob μπορούν να αποθηκεύσουν οποιονδήποτε τύπο κειμένου ή δυαδικά δεδομένα, όπως ένα έγγραφο, του αρχείου πολυμέσων ή εφαρμογή του προγράμματος εγκατάστασης. Χώρος αποθήκευσης αντικειμένων blob επίσης αναφέρεται ως αντικείμενο χώρου αποθήκευσης.

Αυτός ο οδηγός δείχνει πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια με την υπηρεσία αντικειμένων blob του Azure. Τα δείγματα είναι γραμμένες σε PHP και χρησιμοποιήστε το [Azure SDK για PHP] [download]. Τα σενάρια καλύπτεται περιλαμβάνουν **Αποστολή**, **καταχώρηση**, **τη λήψη**και **τη διαγραφή** αντικειμένων blob. Για περισσότερες πληροφορίες σχετικά με αντικείμενα blob, ανατρέξτε στην ενότητα [επόμενα βήματα](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Δημιουργία μιας εφαρμογής PHP

Η μόνη απαίτηση για τη δημιουργία μιας εφαρμογής PHP που αποκτά πρόσβαση στην υπηρεσία αντικειμένων blob του Azure είναι η αναφορά σε κλάσεων στο Azure SDK για PHP από τον κωδικό. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εργαλεία ανάπτυξης για να δημιουργήσετε την εφαρμογή σας, όπως το Σημειωματάριο.

Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες service, το οποίο μπορεί να ονομάζεται μέσα σε μια εφαρμογή PHP τοπικά ή στον κώδικα που εκτελείται μέσα σε ένα ρόλο Azure web, ρόλο εργαζόμενου ή τοποθεσία Web.

## <a name="get-the-azure-client-libraries"></a>Λάβετε τις βιβλιοθήκες Azure προγράμματος-πελάτη

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για να αποκτήσετε πρόσβαση στην υπηρεσία blob

Για να χρησιμοποιήσετε την υπηρεσία αντικειμένων blob του Azure APIs, πρέπει να:

1. Αναφορά στο αρχείο συσκευή αυτόματης φόρτωσης χρησιμοποιώντας την εντολή [require_once] , και
2. Αναφορά οποιαδήποτε κλάσεις που μπορείτε να χρησιμοποιήσετε.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να συμπεριλάβετε το αρχείο συσκευή αυτόματης φόρτωσης και αναφορά την κλάση **ServicesBuilder** .

> [AZURE.NOTE] Αυτό το παράδειγμα (και άλλα παραδείγματα σε αυτό το άρθρο) θεωρείται ότι έχετε εγκαταστήσει τις βιβλιοθήκες PHP προγράμματος-πελάτη για Azure μέσω σύνθεσης. Εάν έχετε εγκαταστήσει τις βιβλιοθήκες με μη αυτόματο τρόπο, πρέπει να αναφέρονται τα `WindowsAzure.php` συσκευή αυτόματης φόρτωσης αρχείου.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Στα παρακάτω παραδείγματα, η `require_once` δήλωση θα εμφανίζονται πάντα, αλλά μόνο τις κατηγορίες που είναι απαραίτητο για να εκτελέσει το παράδειγμα αναφέρονται.

## <a name="set-up-an-azure-storage-connection"></a>Ρύθμιση σύνδεσης Azure χώρου αποθήκευσης

Για να ξεκινήσει ένα πρόγραμμα-πελάτης υπηρεσίας αντικειμένων blob του Azure, πρέπει πρώτα να έχετε μια έγκυρη συμβολοσειρά σύνδεσης. Η μορφή για τη συμβολοσειρά σύνδεσης υπηρεσίας blob είναι:

Για να αποκτήσετε πρόσβαση σε μια ζωντανή υπηρεσία:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Για να αποκτήσετε πρόσβαση σε προσομοίωσης του χώρου αποθήκευσης:

    UseDevelopmentStorage=true


Για να δημιουργήσετε οποιονδήποτε υπολογιστή-πελάτη Azure υπηρεσία, πρέπει να χρησιμοποιήσετε την κλάση **ServicesBuilder** . Μπορείς:

* Μεταβίβαση τη συμβολοσειρά σύνδεσης απευθείας σε αυτήν ή
* Χρησιμοποιήστε το **CloudConfigurationManager (ΚΦΜ)** για να ελέγξετε πολλές εξωτερικές προελεύσεις για τη συμβολοσειρά σύνδεσης:
    * Από προεπιλογή, θα με την υποστήριξη για ένα εξωτερικό αρχείο προέλευσης - μεταβλητές περιβάλλοντος.
    * Μπορείτε να προσθέσετε νέες προελεύσεις επεκτείνοντας την κλάση **ConnectionStringSource** .

Για παραδείγματα που περιγράφονται εδώ, τη συμβολοσειρά σύνδεσης θα περάσουν απευθείας.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Δημιουργία κοντέινερ

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Ένα αντικείμενο **BlobRestProxy** σάς επιτρέπει να δημιουργήσετε ένα κοντέινερ αντικειμένων blob με τη μέθοδο **createContainer** . Όταν δημιουργείτε ένα κοντέινερ, μπορείτε να ορίσετε επιλογές στο κοντέινερ, αλλά κάνοντας επομένως δεν είναι απαραίτητο. (Το παρακάτω παράδειγμα εμφανίζει τον τρόπο ρύθμισης του κοντέινερ στη λίστα ελέγχου πρόσβασης (ACL) και κοντέινερ μετα-δεδομένα).

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Κλήση **setPublicAccess (PublicAccessType::CONTAINER\_και\_αντικείμενα BLOB)** κάνει τα δεδομένα κοντέινερ και αντικειμένων blob προσβάσιμο μέσω ανώνυμου αιτήσεις. Κλήση **setPublicAccess(PublicAccessType::BLOBS_ONLY)** , μόνο τα δεδομένα blob κάνει προσβάσιμα μέσω ανώνυμου αιτήσεις. Για περισσότερες πληροφορίες σχετικά με το κοντέινερ ACL, ανατρέξτε στο θέμα [Ορισμός κοντέινερ ACL (REST API)][container-acl].

Για περισσότερες πληροφορίες σχετικά με τους κωδικούς σφάλματος υπηρεσίας Blob, ανατρέξτε στο θέμα [Κωδικοί σφάλματος υπηρεσίας Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ

Για να αποστείλετε ένα αρχείο ως blob, χρησιμοποιήστε τη μέθοδο **BlobRestProxy -> createBlockBlob** . Αυτή η λειτουργία δημιουργεί το αντικείμενο blob, εάν δεν υπάρχει ή αντικαθιστά την περίπτωση που κάνει. Το παρακάτω παράδειγμα κώδικα προϋποθέτει ότι το κοντέινερ έχει ήδη δημιουργηθεί και χρησιμοποιεί [fopen] [ fopen] για να ανοίξετε το αρχείο ως ροή.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Σημειώστε ότι το προηγούμενο παράδειγμα αποστέλλει ένα blob ως ροή. Ωστόσο, ένα blob μπορεί επίσης να αποσταλούν ως ένα χρησιμοποιώντας συμβολοσειρά, για παράδειγμα, το [αρχείου\_λήψη\_περιεχόμενα] [ file_get_contents] συνάρτηση. Για να το κάνετε χρησιμοποιώντας το προηγούμενο παράδειγμα, αλλάξτε `$content = fopen("c:\myfile.txt", "r");` να `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ

Για να παραθέσετε τα αντικείμενα BLOB σε ένα κοντέινερ, χρησιμοποιήστε τη μέθοδο **BlobRestProxy -> listBlobs** με μια **foreach** βρόχος σε βρόχο έως το αποτέλεσμα. Ο ακόλουθος κώδικας εμφανίζει το όνομα του κάθε αντικειμένων blob ως Έξοδος σε ένα κοντέινερ και εμφανίζει το URI στο πρόγραμμα περιήγησης.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Λήψη ένα blob

Για να κάνετε λήψη μιας blob, η κλήση της μεθόδου **BlobRestProxy -> getBlob** και, στη συνέχεια, η κλήση της μεθόδου **getContentStream** στο αντικείμενο που προκύπτει **GetBlobResult** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Σημειώστε ότι το παραπάνω παράδειγμα λαμβάνει ένα blob ως πόρο ροής (η προεπιλεγμένη συμπεριφορά). Ωστόσο, μπορείτε να χρησιμοποιήσετε το [ροή\_λήψη\_περιεχόμενα] [ stream-get-contents] συνάρτηση για να μετατρέψετε το επιστρεφόμενο ροής σε μια συμβολοσειρά.

## <a name="delete-a-blob"></a>Διαγράψτε ένα blob

Για να διαγράψετε ένα blob, μεταβιβάζουν το όνομα κοντέινερ και αντικειμένων blob **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Διαγραφή ενός κοντέινερ αντικειμένων blob

Τέλος, για να διαγράψετε ένα κοντέινερ αντικειμένων blob, μεταβιβάζουν το όνομα του κοντέινερ **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία της υπηρεσίας αντικειμένων blob του Azure, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

- Επισκεφθείτε το [Ιστολόγιο ομάδας αποθήκευσης Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Δείτε το [παράδειγμα blob μπλοκ PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Δείτε το [παράδειγμα blob PHP σελίδας](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)
 
Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές PHP](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents

<properties
    pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων) χρησιμοποιώντας .NET | Microsoft Azure"
    description="Αποθήκευση μη δομημένα δεδομένα στο cloud με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων)."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία που αποθηκεύει μη δομημένα δεδομένα στο cloud ως αντικείμενα/αντικείμενα blob. Χώρος αποθήκευσης αντικειμένων blob μπορούν να αποθηκεύσουν οποιονδήποτε τύπο κειμένου ή δυαδικά δεδομένα, όπως ένα έγγραφο, αρχείο πολυμέσων ή εφαρμογή του προγράμματος εγκατάστασης. Χώρος αποθήκευσης αντικειμένων blob επίσης αναφέρεται ως αντικείμενο αποθήκευσης.

### <a name="about-this-tutorial"></a>Σχετικά με αυτό το πρόγραμμα εκμάθησης

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς να συντάξετε κώδικα .NET για ορισμένα σενάρια κοινή χρήση του χώρου αποθήκευσης αντικειμένων Blob του Azure. Σενάρια που καλύπτονται περιλαμβάνουν αποστολή, την καταχώρηση, τη λήψη και τη διαγραφή αντικειμένων blob.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 45 λεπτά

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Βιβλιοθήκη προγράμματος-πελάτη Azure χώρου αποθήκευσης για .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Διαχείριση ομάδας παραμέτρων για .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Λογαριασμός Azure χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Περισσότερα δείγματα

Για περισσότερα παραδείγματα χρησιμοποιώντας χώρο αποθήκευσης αντικειμένων Blob, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob Azure στο .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Μπορείτε να κάνετε λήψη του δείγματος εφαρμογής και εκτελέστε το ή αναζητήστε τον κωδικό στη GitHub.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Προσθήκη δηλώσεις χώρου ονομάτων

Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του `program.cs` αρχείο:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Ανάλυση τη συμβολοσειρά σύνδεσης

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Δημιουργήστε το πρόγραμμα-πελάτη υπηρεσίας Blob

Η κλάση **CloudBlobClient** σάς επιτρέπει να ανακτήσετε κοντέινερ και αντικείμενα BLOB που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob. Ακολουθεί ένας τρόπος για να δημιουργήσετε το πρόγραμμα-πελάτη υπηρεσίας:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Τώρα είστε έτοιμοι να γράψετε κώδικα που διαβάζει δεδομένα από και καταγράφει δεδομένα με το χώρο αποθήκευσης αντικειμένων Blob.

## <a name="create-a-container"></a>Δημιουργία κοντέινερ

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Αυτό το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα κοντέινερ, εάν δεν υπάρχει ήδη:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Από προεπιλογή, το νέο κοντέινερ είναι ιδιωτική, γεγονός που σημαίνει ότι πρέπει να καθορίσετε τον αριθμό-κλειδί πρόσβασης χώρου αποθήκευσης για τη λήψη αντικείμενα BLOB από αυτό το κοντέινερ. Εάν θέλετε να καταστήσετε διαθέσιμα τα αρχεία μέσα στο κοντέινερ για όλους τους χρήστες, μπορείτε να ρυθμίσετε το κοντέινερ για να είναι δημόσιες χρησιμοποιώντας τον ακόλουθο κώδικα:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Οποιοσδήποτε στο Internet μπορεί να δει αντικείμενα BLOB δημόσια κοντέινερ, αλλά μπορείτε να τροποποιήσετε ή να διαγράψετε μόνο εάν έχετε το πλήκτρο πρόσβασης κατάλληλο λογαριασμό ή μια υπογραφή κοινόχρηστη πρόσβαση.

## <a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ

Χώρο αποθήκευσης Blob του Azure υποστηρίζει αντικείμενα BLOB μπλοκ και αντικείμενα BLOB σελίδας.  Στις περισσότερες περιπτώσεις, blob μπλοκ είναι ο τύπος συνιστάται να χρησιμοποιήσετε.

Για να αποστείλετε ένα αρχείο σε ένα μπλοκ blob, λάβετε μια αναφορά κοντέινερ και χρησιμοποιήστε το για να λάβετε μια αναφορά blob μπλοκ. Όταν έχετε μια αναφορά blob, μπορείτε να αποστείλετε οποιαδήποτε ροή δεδομένων σε αυτήν, καλώντας τη μέθοδο **UploadFromStream** . Αυτή η λειτουργία θα δημιουργήσει το αντικείμενο blob Εάν δεν έχετε προηγουμένως υπάρχουν ή να αντικαταστήσετε την περίπτωση που υπάρχει.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα blob σε ένα κοντέινερ και προϋποθέτει ότι έχει ήδη δημιουργήσει το κοντέινερ.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ

Για να παραθέσετε τα αντικείμενα BLOB κοντέινερ, λάβετε πρώτα μια αναφορά κοντέινερ. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τη μέθοδο **ListBlobs** στο κοντέινερ για να ανακτήσετε τα αντικείμενα BLOB ή/και σε καταλόγους μέσα σε αυτό. Για να αποκτήσετε πρόσβαση το εμπλουτισμένο σύνολο ιδιοτήτων και μεθόδων για ένα επιστρεφόμενο **IListBlobItem**, πρέπει να το μετατραπεί σε ένα αντικείμενο **CloudBlockBlob**, **CloudPageBlob**ή **CloudBlobDirectory** .  Εάν ο τύπος είναι άγνωστη, μπορείτε να χρησιμοποιήσετε έναν τύπο ελέγχου για να προσδιορίσετε την οποία θέλετε να μετατραπεί για να.  Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να ανακτήσετε και εξόδου το URI της κάθε στοιχείο του `photos` κοντέινερ:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Όπως φαίνεται παραπάνω, μπορείτε να ονομάσετε αντικείμενα blob με πληροφορίες διαδρομής στα ονόματά τους. Αυτό δημιουργεί μια δομή εικονικού καταλόγου που μπορείτε να οργανώσετε και να διέλευσης όπως θα κάνατε με ένα σύστημα παραδοσιακή αρχείων. Σημειώστε ότι η δομή καταλόγου είναι εικονικού μόνο - οι διαθέσιμες στο χώρο αποθήκευσης αντικειμένων Blob μόνο πόροι είναι κοντέινερ και αντικείμενα blob. Ωστόσο, τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης παρέχει ένα αντικείμενο **CloudBlobDirectory** για να αναφέρονται σε έναν εικονικό κατάλογο και να απλοποιήσετε τη διαδικασία της εργασίας με αντικείμενα BLOB που είναι οργανωμένα σε αυτόν τον τρόπο.

Για παράδειγμα, εξετάστε το ακόλουθο σύνολο αντικείμενα BLOB μπλοκ σε ένα κοντέινερ που ονομάζεται `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Όταν καλείτε **ListBlobs** στο κοντέινερ 'φωτογραφίες' (όπως στο παραπάνω δείγμα), επιστρέφεται μια ιεραρχική λίστα. Περιέχει αντικείμενα **CloudBlobDirectory** και **CloudBlockBlob** , που αντιπροσωπεύει την σε καταλόγους και αντικείμενα blob στο κοντέινερ, αντίστοιχα. Το αποτέλεσμα έχει την παρακάτω:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Προαιρετικά, μπορείτε να ορίσετε την παράμετρο **UseFlatBlobListing** της της μεθόδου **ListBlobs** στην **τιμή true**. Σε αυτήν την περίπτωση, κάθε blob στο κοντέινερ επιστρέφεται ως ένα αντικείμενο **CloudBlockBlob** . Την κλήση σε **ListBlobs** για να λάβετε μια λίστα επίπεδο μοιάζει κάπως έτσι:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

και τα αποτελέσματα είναι ως εξής:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Λήψη αντικείμενα BLOB

Για να κάνετε λήψη αντικείμενα blob, πρώτα να ανακτήσετε μια αναφορά blob και, στη συνέχεια, να καλέσετε τη μέθοδο **DownloadToStream** . Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο **DownloadToStream** για να μεταφέρετε τα περιεχόμενα blob σε ένα αντικείμενο ροής που, στη συνέχεια, μπορείτε να παραμένει σε ένα τοπικό αρχείο.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο **DownloadToStream** για να κάνετε λήψη των περιεχομένων μιας blob ως συμβολοσειρά κειμένου.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Διαγραφή αντικειμένων blob

Για να διαγράψετε ένα blob, πρώτα λήψη μιας αναφοράς blob και, στη συνέχεια, να καλέσετε τη μέθοδο **Διαγράψτε** το.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Λίστα ασύγχρονα αντικείμενα BLOB στις σελίδες

Εάν καταχώρηση μεγάλου αριθμού αντικείμενα BLOB ή που θέλετε να ελέγξετε τον αριθμό των αποτελεσμάτων, μπορείτε να επιστρέψετε σε μια λίστα λειτουργία, μπορείτε να παραθέσετε αντικείμενα BLOB στις σελίδες των αποτελεσμάτων. Αυτό το παράδειγμα δείχνει πώς μπορείτε να επιστρέφει αποτελέσματα σε σελίδες του ασύγχρονα, έτσι ώστε να εκτέλεσης δεν αποκλείονται κατά την αναμονή για να λάβετε ένα μεγάλο σύνολο αποτελεσμάτων.

Αυτό το παράδειγμα εμφανίζει ένα επίπεδο blob καταχώρηση, αλλά μπορείτε επίσης να εκτελέσετε μια ιεραρχική λίστα, ορίζοντας την `useFlatBlobListing` παράμετρος της μεθόδου **ListBlobsSegmentedAsync** για να `false`.

Επειδή η μέθοδος δείγμα κλήσεις μεθόδου ασύγχρονης, πρέπει να είναι πριν από την `async` λέξη-κλειδί και θα πρέπει να επιστρέφει ένα αντικείμενο **εργασίας** . Η λέξη-κλειδί await που έχουν καθοριστεί για τη μέθοδο **ListBlobsSegmentedAsync** διακόπτει προσωρινά την εκτέλεση της μεθόδου δείγμα μέχρι να ολοκληρωθεί η εργασία καταχώρηση.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Εγγραφή σε μια blob προσάρτησης

Ένα blob προσάρτησης είναι ένας νέος τύπος blob, εισάγονται με την έκδοση 5.x της βιβλιοθήκης του προγράμματος-πελάτη Azure χώρου αποθήκευσης για .NET. Ένα blob προσάρτησης είναι βελτιστοποιημένη για λειτουργίες προσάρτησης, όπως η καταγραφή. Όπως ένα μπλοκ blob, ένα blob προσάρτησης αποτελείται από τα μπλοκ, αλλά όταν προσθέτετε ένα νέο μπλοκ ένα blob προσάρτησης, είναι πάντα προσαρτημένο στο τέλος της το αντικείμενο blob. Δεν μπορείτε να ενημερώσετε ή να διαγράψετε μια υπάρχουσα μπλοκ σε ένα blob προσάρτησης. Το μπλοκ αναγνωριστικών για ένα blob προσάρτησης δεν εκτίθενται ως έχουν για ένα μπλοκ blob.

Κάθε μπλοκ σε ένα blob προσάρτησης μπορεί να είναι διαφορετικό μέγεθος, έως και 4 MB, και ένα blob προσάρτησης μπορούν να περιλαμβάνουν έως 50.000 μπλοκ. Το μέγιστο μέγεθος των ένα blob προσάρτησης, επομένως, είναι λίγο περισσότερο από 195 GB (4 MB X 50000 μπλοκ).

Το παρακάτω παράδειγμα δημιουργεί μια νέα blob προσάρτησης και τοποθετεί ορισμένα δεδομένα, προσομοίωση μιας λειτουργίας απλής καταγραφής.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Ανατρέξτε στο θέμα [Κατανόηση των αντικειμένων blob μπλοκ, αντικείμενα BLOB σελίδας, και προσάρτηση αντικείμενα BLOB](https://msdn.microsoft.com/library/azure/ee691964.aspx) για περισσότερες πληροφορίες σχετικά με τις διαφορές μεταξύ τους τρεις τύπους αντικείμενα blob.

## <a name="managing-security-for-blobs"></a>Διαχείριση της ασφάλειας για αντικείμενα BLOB

Από προεπιλογή, αποθήκευσης Azure διατηρεί τα δεδομένα σας ασφαλή με περιορισμό πρόσβασης στον κάτοχο του λογαριασμού, που έχει στη διάθεσή του τα πλήκτρα πρόσβασης λογαριασμού. Όταν πρέπει να κάνετε κοινή χρήση αντικειμένων blob δεδομένων στο λογαριασμό σας στο χώρο αποθήκευσης, είναι σημαντικό να το κάνετε χωρίς επιπτώσεις της ασφάλειας του πλήκτρων πρόσβασης του λογαριασμού. Επιπλέον, μπορείτε να κρυπτογραφήσετε blob δεδομένα για να βεβαιωθείτε ότι είναι ασφαλής μεταβαίνοντας σε δίκτυο και στο χώρο αποθήκευσης Azure.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Έλεγχος της πρόσβασης σε δεδομένα blob

Από προεπιλογή, τα δεδομένα blob στο λογαριασμό σας στο χώρο αποθήκευσης είναι προσβάσιμα μόνο στον κάτοχο λογαριασμού χώρου αποθήκευσης. Πραγματοποίηση ελέγχου ταυτότητας προσκλήσεις σε σχέση με το χώρο αποθήκευσης αντικειμένων Blob απαιτεί το πλήκτρο πρόσβασης λογαριασμού από προεπιλογή. Ωστόσο, ίσως θελήσετε να είναι ορισμένα δεδομένα blob διαθέσιμες σε άλλους χρήστες. Έχετε δύο επιλογές:

- **Ανώνυμης πρόσβασης:** Μπορείτε να κάνετε ένα κοντέινερ ή τα αντικείμενα BLOB διαθέσιμη στο κοινό για ανώνυμη πρόσβαση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση ανώνυμη πρόσβαση για ανάγνωση σε κοντέινερ και αντικείμενα BLOB](storage-manage-access-to-resources.md) .
- **Κοινή χρήση υπογραφών πρόσβασης:** Μπορείτε να παράσχετε προγράμματα-πελάτες με μια κοινόχρηστη πρόσβαση υπογραφή (συσχετισμών Ασφαλείας), το οποίο παρέχει ανάθεση πρόσβαση σε έναν πόρο στο λογαριασμό σας στο χώρο αποθήκευσης, με τα δικαιώματα που καθορίζετε και ένα διάστημα που καθορίζετε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Η κρυπτογράφηση δεδομένων blob

Azure αποθήκευσης υποστηρίζει κρυπτογράφηση δεδομένων blob τόσο στο πρόγραμμα-πελάτη και στο διακομιστή:

- **Κρυπτογράφησης πλευρά του προγράμματος-πελάτη:** Η βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης για .NET υποστηρίζει κρυπτογράφηση δεδομένων μέσα σε εφαρμογές προγράμματος-πελάτη πριν από την αποστολή στο χώρο αποθήκευσης Azure και αποκρυπτογράφηση δεδομένων κατά τη λήψη του στον υπολογιστή-πελάτη. Στη βιβλιοθήκη υποστηρίζει επίσης ενοποίηση με το Azure θάλαμο αριθμού-κλειδιού για τη Διαχείριση βασικών λογαριασμού χώρου αποθήκευσης. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Κρυπτογράφηση πλευρά του προγράμματος-πελάτη με το .NET για το Microsoft Azure αποθήκευσης](storage-client-side-encryption.md) . Δείτε επίσης [πρόγραμμα εκμάθησης: κρυπτογράφηση και αποκρυπτογράφηση αντικείμενα blob στο χώρο αποθήκευσης Microsoft Azure χρησιμοποιώντας Azure κλειδί θάλαμο](storage-encrypt-decrypt-blobs-key-vault.md).
- **Κρυπτογράφηση διακομιστή**: αποθήκευσης Azure υποστηρίζει τώρα κρυπτογράφηση πλευρά του διακομιστή. Ανατρέξτε στην ενότητα [κρυπτογράφηση υπηρεσίας Azure χώρου αποθήκευσης για τα δεδομένα στο υπόλοιπο (έκδοση Preview)](storage-service-encryption.md).

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρο αποθήκευσης αντικειμένων Blob, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα.

### <a name="microsoft-azure-storage-explorer"></a>Εξερεύνηση χώρου αποθήκευσης του Windows Azure
- [Εξερεύνηση χώρου αποθήκευσης Azure Microsoft (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) είναι δωρεάν, μεμονωμένη εφαρμογή από τη Microsoft που σας επιτρέπει να εργαστείτε οπτικά με αποθήκευσης Azure δεδομένα σε Windows, OS X και Linux.

### <a name="blob-storage-samples"></a>Δείγματα χώρο αποθήκευσης αντικειμένων blob

- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure στο .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Αναφορά χώρο αποθήκευσης αντικειμένων blob

- [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Αναφορά REST API](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Εννοιολογική οδηγοί

- [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)
- [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αρχείων για .NET](storage-dotnet-how-to-use-files.md)
- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355

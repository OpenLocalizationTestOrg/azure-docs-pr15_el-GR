<properties
    pageTitle="Γρήγορα αποτελέσματα με το blob χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες (υπηρεσίες cloud) | Microsoft Azure"
    description="Πώς να γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure σε ένα έργο υπηρεσία cloud στο Visual Studio μετά τη σύνδεση με ένα λογαριασμό χώρου αποθήκευσης χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης Blob του Azure και του Visual Studio συνδεδεμένες υπηρεσίες (cloud services έργα)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ξεκινήσετε με το χώρο αποθήκευσης Blob του Azure αφού έχετε δημιουργήσει ή στα οποία γίνεται αναφορά αποθήκευσης Azure λογαριασμού, χρησιμοποιώντας το παράθυρο διαλόγου **Προσθήκη συνδεδεμένες υπηρεσίες** του Visual Studio σε ένα έργο Visual Studio cloud υπηρεσιών. Θα σας δείξουμε πώς μπορείτε να αποκτήσετε πρόσβαση και να δημιουργήσετε blob κοντέινερ και πώς να εκτελείτε συνήθεις εργασίες όπως αποστολή, την καταχώρηση και τη λήψη αντικείμενα blob. Τα δείγματα που είναι γραμμένες σε C\# και να χρησιμοποιήσετε τη [Βιβλιοθήκη προγράμματος-πελάτη του Microsoft Azure χώρου αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία για την αποθήκευση μεγάλου όγκου μη δομημένα δεδομένα που είναι δυνατή η πρόσβαση από οπουδήποτε στον κόσμο μέσω HTTP ή HTTPS. Ένα μεμονωμένο blob μπορεί να είναι οποιουδήποτε μεγέθους. Αντικείμενα BLOB μπορεί να είναι στοιχεία, όπως εικόνες, αρχεία ήχου και βίντεο, ανεπεξέργαστα δεδομένα και αρχεία εγγράφων.

Όπως ακριβώς αρχείων live σε φακέλους, χώρος αποθήκευσης αντικειμένων blob βρίσκονται στο κοντέινερ. Αφού δημιουργήσετε ένα χώρο αποθήκευσης, μπορείτε να δημιουργήσετε μία ή περισσότερες κοντέινερ στο χώρο αποθήκευσης της. Για παράδειγμα, σε ένα χώρο αποθήκευσης που ονομάζεται "Λευκώματος", μπορείτε να δημιουργήσετε κοντέινερ στο χώρο αποθήκευσης της που ονομάζεται "εικόνες" για να αποθηκεύσετε εικόνες και άλλο που ονομάζεται "ήχου" για να αποθηκεύσετε αρχεία ήχου. Αφού δημιουργήσετε το κοντέινερ, μπορείτε να αποστείλετε αρχεία μεμονωμένα blob σε αυτά.

- Για περισσότερες πληροφορίες σχετικά με προγραμματισμό χειρισμό αντικείμενα blob, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md).
- Για γενικές πληροφορίες σχετικά με το χώρο αποθήκευσης Azure, ανατρέξτε [στην τεκμηρίωση χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/).
- Για γενικές πληροφορίες σχετικά με τις υπηρεσίες Cloud Azure, ανατρέξτε στο θέμα [τεκμηρίωση των υπηρεσιών Cloud](https://azure.microsoft.com/documentation/services/cloud-services/).
- Για περισσότερες πληροφορίες σχετικά με τον προγραμματισμό εφαρμογών ASP.NET, ανατρέξτε στο θέμα [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Κοντέινερ αντικειμένων blob πρόσβαση στον κώδικα

Για την πρόσβαση μέσω προγραμματισμού αντικείμενα BLOB έργα υπηρεσία cloud, πρέπει να προσθέσετε τα ακόλουθα στοιχεία, εάν δεν είναι ήδη υπάρχει.

1. Προσθέστε τις ακόλουθες δηλώσεις χώρος ονομάτων κώδικα στο επάνω μέρος της οποιοδήποτε αρχείο C# στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού αποθήκευσης Azure.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Λάβετε ένα αντικείμενο **CloudStorageAccount** που αντιπροσωπεύει τις πληροφορίες λογαριασμού χώρου αποθήκευσης. Χρησιμοποιήστε τον ακόλουθο κώδικα για να λάβετε το τις συμβολοσειρά σύνδεσης χώρου αποθήκευσης και τις πληροφορίες λογαριασμού χώρου αποθήκευσης από τη ρύθμιση παραμέτρων της υπηρεσίας Azure.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Λάβετε ένα αντικείμενο **CloudBlobClient** για να αναφέρονται σε μια υπάρχουσα κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Λάβετε ένα αντικείμενο **CloudBlobContainer** για να αναφέρονται σε ένα συγκεκριμένο blob κοντέινερ.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Χρησιμοποιήστε όλο τον κώδικα που φαίνεται στην προηγούμενη διαδικασία μπροστά από τον κωδικό που εμφανίζονται στις ενότητες που ακολουθούν.

## <a name="create-a-container-in-code"></a>Δημιουργία κοντέινερ στον κώδικα

> [AZURE.NOTE] Ορισμένα API που εκτελούν κλήσεων ανάληψη στο χώρο αποθήκευσης Azure στο ASP.NET είναι ασύγχρονης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ασύγχρονη προγραμματισμού με ασύγχρονης και Await](http://msdn.microsoft.com/library/hh191443.aspx) . Ο κώδικας στο παρακάτω παράδειγμα προϋποθέτει ότι χρησιμοποιείτε ασύγχρονης μεθόδους προγραμματισμού.

Για να δημιουργήσετε ένα κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης, πρέπει να κάνετε είναι να προσθέσετε μια κλήση σε **CreateIfNotExistsAsync** όπως τον ακόλουθο κώδικα:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Για να διαθέσετε τα αρχεία μέσα στο κοντέινερ για όλους τους χρήστες, μπορείτε να ορίσετε το κοντέινερ για να είναι δημόσια, χρησιμοποιώντας τον παρακάτω κώδικα.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Οποιοσδήποτε στο Internet μπορεί να δει αντικείμενα BLOB δημόσια κοντέινερ, αλλά μπορείτε να τροποποιήσετε ή να διαγράψετε μόνο εάν έχετε τον αριθμό-κλειδί κατάλληλα δικαιώματα πρόσβασης.

## <a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ

Azure αποθήκευσης υποστηρίζει αντικείμενα BLOB μπλοκ και αντικείμενα BLOB σελίδας. Στις περισσότερες περιπτώσεις, blob μπλοκ είναι ο τύπος συνιστάται να χρησιμοποιήσετε.

Για να αποστείλετε ένα αρχείο σε ένα μπλοκ blob, λάβετε μια αναφορά κοντέινερ και χρησιμοποιήστε το για να λάβετε μια αναφορά blob μπλοκ. Όταν έχετε μια αναφορά blob, μπορείτε να αποστείλετε οποιαδήποτε ροή δεδομένων σε αυτήν, καλώντας τη μέθοδο **UploadFromStream** . Αυτή η λειτουργία δημιουργεί το αντικείμενο blob αν δεν υπάρχει ήδη, ή αντικαθιστά το, εάν υπάρχει. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα blob σε ένα κοντέινερ και προϋποθέτει ότι έχει ήδη δημιουργήσει το κοντέινερ.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ

Για να παραθέσετε τα αντικείμενα BLOB σε ένα κοντέινερ, λάβετε πρώτα μια αναφορά κοντέινερ. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τη μέθοδο **ListBlobs** στο κοντέινερ για να ανακτήσετε τα αντικείμενα BLOB ή/και σε καταλόγους μέσα σε αυτό. Για να αποκτήσετε πρόσβαση το εμπλουτισμένο σύνολο ιδιοτήτων και μεθόδων για ένα επιστρεφόμενο **IListBlobItem**, πρέπει να το μετατραπεί σε ένα αντικείμενο **CloudBlockBlob**, **CloudPageBlob**ή **CloudBlobDirectory** . Εάν ο τύπος είναι άγνωστη, μπορείτε να χρησιμοποιήσετε έναν τύπο ελέγχου για να προσδιορίσετε την οποία θέλετε να μετατραπεί για να. Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να ανακτήσετε και εξόδου το URI κάθε στοιχείου στο κοντέινερ **φωτογραφίες** :

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

Όπως φαίνεται στο προηγούμενο παράδειγμα κώδικα, η υπηρεσία blob έχει την έννοια της σε καταλόγους μέσα σε κοντέινερ, καθώς και. Αυτό είναι έτσι ώστε να μπορείτε να οργανώσετε το αντικείμενα BLOB σε μια πιο φάκελο δομή. Για παράδειγμα, εξετάστε το ακόλουθο σύνολο αντικείμενα BLOB μπλοκ σε ένα κοντέινερ που ονομάζεται **φωτογραφίες**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Όταν καλείτε **ListBlobs** στο κοντέινερ (όπως το προηγούμενο παράδειγμα), η συλλογή επιστρέφονται περιέχει αντικείμενα **CloudBlobDirectory** και **CloudBlockBlob** που αντιπροσωπεύει την σε καταλόγους και αντικείμενα BLOB που περιέχονται στο ανώτερο επίπεδο. Ακολουθεί το αποτέλεσμα:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Προαιρετικά, μπορείτε να ορίσετε την παράμετρο **UseFlatBlobListing** της της μεθόδου **ListBlobs** στην **τιμή true**. Το αποτέλεσμα κάθε blob επιστρέφεται ως μια **CloudBlockBlob**, ανεξάρτητα από το καταλόγου. Εδώ θα βρείτε την κλήση σε **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

και εδώ θα βρείτε τα αποτελέσματα:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Λήψη αντικείμενα BLOB

Για να κάνετε λήψη αντικείμενα blob, πρώτα να ανακτήσετε μια αναφορά blob και, στη συνέχεια, να καλέσετε τη μέθοδο **DownloadToStream** . Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο **DownloadToStream** για να μεταφέρετε τα περιεχόμενα blob σε ένα αντικείμενο ροής που, στη συνέχεια, μπορείτε να παραμένει σε ένα τοπικό αρχείο.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο **DownloadToStream** για να κάνετε λήψη των περιεχομένων μιας blob ως συμβολοσειρά κειμένου.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Διαγραφή αντικειμένων blob

Για να διαγράψετε ένα blob, πρώτα λήψη μιας αναφοράς blob και, στη συνέχεια, καλέστε τη μέθοδο **Delete** .

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Λίστα ασύγχρονα αντικείμενα BLOB σε σελίδες

Εάν καταχώρηση μεγάλου αριθμού αντικείμενα BLOB ή που θέλετε να ελέγξετε τον αριθμό των αποτελεσμάτων, μπορείτε να επιστρέψετε σε μια λίστα λειτουργία, μπορείτε να παραθέσετε αντικείμενα BLOB στις σελίδες των αποτελεσμάτων. Αυτό το παράδειγμα δείχνει πώς μπορείτε να επιστρέφει αποτελέσματα σε σελίδες του ασύγχρονα, έτσι ώστε να εκτέλεσης δεν αποκλείονται κατά την αναμονή για να λάβετε ένα μεγάλο σύνολο αποτελεσμάτων.

Αυτό το παράδειγμα εμφανίζει ένα επίπεδο blob καταχώρηση, αλλά μπορείτε επίσης να εκτελέσετε μια ιεραρχική λίστα, ορίζοντας την παράμετρο **useFlatBlobListing** της μεθόδου **ListBlobsSegmentedAsync** σε **false**.

Επειδή η μέθοδος δείγμα κλήσεις μεθόδου ασύγχρονης, αυτό πρέπει να είναι πριν από τη λέξη-κλειδί **ασύγχρονης** και πρέπει να επιστραφεί ένα αντικείμενο **εργασίας** . Η λέξη-κλειδί await που έχουν καθοριστεί για τη μέθοδο **ListBlobsSegmentedAsync** διακόπτει προσωρινά την εκτέλεση της μεθόδου δείγμα μέχρι να ολοκληρωθεί η εργασία καταχώρηση.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

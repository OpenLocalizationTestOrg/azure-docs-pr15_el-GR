<properties
    pageTitle="Γρήγορα αποτελέσματα με το blob χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες (ASP.NET 5) | Microsoft Azure"
    description="Πώς μπορείτε να ξεκινήσετε με χώρο αποθήκευσης αντικειμένων Blob του Azure σε ένα έργο Visual Studio ASP.NET 5 αφού έχετε δημιουργήσει ένα λογαριασμό χώρου αποθήκευσης χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Γρήγορα αποτελέσματα με το Azure Blob χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες (ASP.NET 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ξεκινήσετε με χώρο αποθήκευσης αντικειμένων Blob του Azure στο Visual Studio, αφού έχετε δημιουργήσει ή στα οποία γίνεται αναφορά λογαριασμού Azure χώρου αποθήκευσης σε ένα έργο ASP.NET 5, χρησιμοποιώντας το παράθυρο διαλόγου του Visual Studio προσθέσετε συνδεδεμένες υπηρεσίες.

Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία για την αποθήκευση μεγάλου όγκου μη δομημένα δεδομένα που είναι δυνατή η πρόσβαση από οπουδήποτε στον κόσμο μέσω HTTP ή HTTPS. Ένα μεμονωμένο blob μπορεί να είναι οποιουδήποτε μεγέθους. Αντικείμενα BLOB μπορεί να είναι στοιχεία, όπως εικόνες, αρχεία ήχου και βίντεο, ανεπεξέργαστα δεδομένα και αρχεία εγγράφων. Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ξεκινήσετε με το χώρο αποθήκευσης αντικειμένων blob αφού δημιουργήσετε ένα λογαριασμό Azure χώρου αποθήκευσης, χρησιμοποιώντας το παράθυρο διαλόγου **Προσθήκη συνδεδεμένες υπηρεσίες** του Visual Studio σε ένα έργο ASP.NET 5.

Όπως ακριβώς αρχείων live σε φακέλους, χώρος αποθήκευσης αντικειμένων blob βρίσκονται στο κοντέινερ. Αφού δημιουργήσετε ένα χώρο αποθήκευσης, μπορείτε να δημιουργήσετε μία ή περισσότερες κοντέινερ στο χώρο αποθήκευσης της. Για παράδειγμα, σε ένα χώρο αποθήκευσης που ονομάζεται "Λευκώματος", μπορείτε να δημιουργήσετε κοντέινερ στο χώρο αποθήκευσης της που ονομάζεται "εικόνες" για να αποθηκεύσετε εικόνες και άλλο που ονομάζεται "ήχου" για να αποθηκεύσετε αρχεία ήχου. Αφού δημιουργήσετε το κοντέινερ, μπορείτε να αποστείλετε αρχεία μεμονωμένα blob σε αυτά. Για περισσότερες πληροφορίες σχετικά με προγραμματισμό χειρισμό αντικείμενα blob, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Κοντέινερ αντικειμένων blob πρόσβαση στον κώδικα

Για την πρόσβαση μέσω προγραμματισμού αντικείμενα BLOB ASP.NET 5 έργα, πρέπει να προσθέσετε τα παρακάτω στοιχεία, εάν δεν είναι ήδη υπάρχει.

1. Προσθέστε τις ακόλουθες δηλώσεις χώρος ονομάτων κώδικα στο επάνω μέρος της οποιοδήποτε αρχείο C# στην οποία θέλετε να πρόσβαση μέσω προγραμματισμού Azure χώρου αποθήκευσης.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Λάβετε ένα αντικείμενο **CloudStorageAccount** που αντιπροσωπεύει τις πληροφορίες λογαριασμού χώρου αποθήκευσης. Χρησιμοποιήστε τον ακόλουθο κώδικα για να λάβετε το τις συμβολοσειρά σύνδεσης χώρου αποθήκευσης και τις πληροφορίες λογαριασμού χώρου αποθήκευσης από τη ρύθμιση παραμέτρων της υπηρεσίας Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **ΣΗΜΕΊΩΣΗ:** Χρησιμοποιήστε όλο τον παραπάνω κώδικα μπροστά από τον κωδικό στις ενότητες που ακολουθούν.


3. Χρησιμοποιήστε ένα αντικείμενο **CloudBlobClient** για να λάβετε μια αναφορά **CloudBlobContainer** σε μια υπάρχουσα κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Δημιουργία κοντέινερ στον κώδικα

Μπορείτε επίσης να χρησιμοποιήσετε το **CloudBlobClient** για να δημιουργήσετε ένα κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης. Το μόνο που χρειάζεται να κάνετε είναι να προσθέσετε μια κλήση σε **CreateIfNotExistsAsync** όπως τον ακόλουθο κώδικα:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**ΣΗΜΕΊΩΣΗ:** Τα API που εκτελούν κλήσεις με το χώρο αποθήκευσης Azure ASP.NET 5 είναι ασύγχρονης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ασύγχρονη προγραμματισμού με ασύγχρονης και Await](http://msdn.microsoft.com/library/hh191443.aspx) . Ο παρακάτω κώδικας προϋποθέτει ασύγχρονης μεθόδους προγραμματισμού που χρησιμοποιούνται.

Για να διαθέσετε τα αρχεία μέσα στο κοντέινερ για όλους τους χρήστες, μπορείτε να ορίσετε το κοντέινερ για να είναι δημόσια, χρησιμοποιώντας τον παρακάτω κώδικα.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Αποστείλετε ένα blob σε ένα κοντέινερ

Για να αποστείλετε ένα αρχείο blob σε ένα κοντέινερ, λάβετε μια αναφορά κοντέινερ και χρησιμοποιήστε το για να λάβετε μια αναφορά blob. Αφού έχετε μια αναφορά blob, μπορείτε να αποστείλετε οποιαδήποτε ροή δεδομένων σε αυτήν, καλώντας τη μέθοδο **UploadFromStreamAsync** . Αυτή η λειτουργία δημιουργεί το αντικείμενο blob, αν δεν υπάρχει ήδη, ή αντικαθιστά το, εάν υπάρχει. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα blob σε ένα κοντέινερ και προϋποθέτει ότι έχει ήδη δημιουργήσει το κοντέινερ.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Λίστα με τα αντικείμενα BLOB σε ένα κοντέινερ
Για να παραθέσετε τα αντικείμενα BLOB σε ένα κοντέινερ, λάβετε πρώτα μια αναφορά κοντέινερ. Στη συνέχεια, μπορείτε να καλέσετε τη μέθοδο **ListBlobsSegmentedAsync** του κοντέινερ για να ανακτήσετε τα αντικείμενα BLOB ή/και σε καταλόγους μέσα σε αυτό. Για να αποκτήσετε πρόσβαση το εμπλουτισμένο σύνολο ιδιοτήτων και μεθόδων για ένα επιστρεφόμενο **IListBlobItem**, πρέπει να το μετατραπεί σε ένα αντικείμενο **CloudBlockBlob**, **CloudPageBlob**ή **CloudBlobDirectory** . Εάν δεν γνωρίζετε τον τύπο blob, μπορείτε να χρησιμοποιήσετε έναν τύπο ελέγχου για να προσδιορίσετε την οποία θέλετε να μετατραπεί για να. Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να ανακτήσετε και εξόδου το URI κάθε στοιχείου σε ένα κοντέινερ.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Υπάρχουν άλλοι τρόποι για να παραθέσετε τα περιεχόμενα ενός κοντέινερ αντικειμένων blob. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Λήψη ένα blob
Για να κάνετε λήψη μιας blob, πρώτα λήψη μιας αναφοράς για το αντικείμενο blob και, στη συνέχεια, να καλέσετε τη μέθοδο **DownloadToStreamAsync** . Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο **DownloadToStreamAsync** για να μεταφέρετε τα περιεχόμενα blob σε ένα αντικείμενο ροής που, στη συνέχεια, μπορείτε να αποθηκεύσετε ως ένα τοπικό αρχείο.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Υπάρχουν άλλοι τρόποι για να αποθηκεύσετε αντικείμενα BLOB ως αρχεία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Διαγράψτε ένα blob
Για να διαγράψετε ένα blob, πρώτα λήψη μιας αναφοράς για το αντικείμενο blob και, στη συνέχεια, να καλέσετε τη μέθοδο **DeleteAsync** σε αυτό.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

<properties
    pageTitle="Ορισμός και ανάκτηση ιδιότητες και μετα-δεδομένων για τα αντικείμενα στο χώρο αποθήκευσης Azure | Microsoft Azure"
    description="Αποθήκευση προσαρμοσμένου μετα-δεδομένων σε αντικείμενα στο χώρο αποθήκευσης Azure, και ορίστε και ανάκτηση Ιδιότητες συστήματος."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Ορισμός και ανάκτηση ιδιότητες και μετα-δεδομένων #

## <a name="overview"></a>Επισκόπηση

Αντικείμενα στο χώρο αποθήκευσης Azure ιδιότητες υποστήριξης συστήματος και χρήστη μετα-δεδομένα, εκτός από τα δεδομένα που περιέχουν:

*   **Ιδιότητες συστήματος.** Ιδιότητες συστήματος που υπάρχουν σε κάθε πόρο χώρου αποθήκευσης. Ορισμένες από αυτές μπορούν να ανάγνωση ή ορίσετε, ενώ άλλοι είναι μόνο για ανάγνωση. Στην περιοχή στο παρασκήνιο, ορισμένες ιδιότητες συστήματος αντιστοιχούν σε ορισμένες τυπικές κεφαλίδες HTTP. Η βιβλιοθήκη προγράμματος-πελάτη Azure αποθήκευσης διατηρεί τα εξής για εσάς.  

*   **Που ορίζονται από το χρήστη μετα-δεδομένων.** Μετα-δεδομένων που ορίζονται από το χρήστη είναι μετα-δεδομένων που καθορίζετε σε έναν πόρο που δίνεται, με τη μορφή ένα ζεύγος ονόματος-τιμής. Μπορείτε να χρησιμοποιήσετε μετα-δεδομένων για την αποθήκευση πρόσθετες τιμές με έναν πόρο χώρου αποθήκευσης. Αυτές οι τιμές είναι το δικό σας μόνο για λόγους και δεν επηρεάζουν τον τρόπο συμπεριφοράς του πόρου.  

Ανάκτηση τιμές ιδιοτήτων και μετα-δεδομένων για έναν πόρο χώρου αποθήκευσης είναι μια διαδικασία δύο βημάτων. Πριν να διαβάζετε αυτές τις τιμές, θα πρέπει να τους λήψη ρητά καλώντας τη μέθοδο **FetchAttributes** .

> [AZURE.IMPORTANT] Τιμές ιδιοτήτων και μετα-δεδομένων για έναν πόρο χώρου αποθήκευσης δεν συμπληρώνονται, εκτός εάν μία από τις μεθόδους **FetchAttributes** καλέσετε. 

## <a name="setting-and-retrieving-properties"></a>Ρύθμιση και την ανάκτηση ιδιοτήτων

Για να ανακτήσετε τιμές ιδιοτήτων, καλέστε τη μέθοδο **FetchAttributes** σε blob ή κοντέινερ για να συμπληρώσετε τις ιδιότητες και, στη συνέχεια, διαβάστε τις τιμές.

Για να ορίσετε ιδιότητες σε ένα αντικείμενο, καθορίστε την τιμή της ιδιότητας και, στη συνέχεια, η κλήση της μεθόδου **SetProperties** .

Το ακόλουθο παράδειγμα κώδικα δημιουργεί ένα κοντέινερ και εγγράφει κάποιες από τις τιμές ιδιοτήτων σε ένα παράθυρο κονσόλας:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Ρύθμιση και την ανάκτηση μετα-δεδομένων

Μπορείτε να καθορίσετε μετα-δεδομένων ως ένα ή περισσότερα ζεύγη ονόματος-τιμής σε έναν πόρο blob ή κοντέινερ. Για να ορίσετε μετα-δεδομένων, προσθήκη ζεύγη ονόματος-τιμής στη συλλογή **μετα-δεδομένων** στον πόρο και, στη συνέχεια, η κλήση της μεθόδου **SetMetadata** για να αποθηκεύσετε τις τιμές για την υπηρεσία.

> [AZURE.NOTE] Το όνομα του μετα-δεδομένων του πρέπει να συμφωνεί με τις συνθήκες ονοματοθεσίας για τα αναγνωριστικά C#.
 
Το ακόλουθο παράδειγμα κώδικα ορίζει μετα-δεδομένων σε ένα κοντέινερ. Μια τιμή έχει οριστεί χρησιμοποιώντας τη συλλογή μέθοδο **Add** . Η άλλη τιμή έχει οριστεί χρησιμοποιώντας τη σύνταξη έμμεσα κλειδιού/τιμής. Και τα δύο είναι έγκυρες.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Για να ανακτήσετε μετα-δεδομένων, καλέστε τη μέθοδο **FetchAttributes** σε blob ή κοντέινερ για να συμπληρώσετε τη συλλογή **μετα-δεδομένων** και, στη συνέχεια, διαβάστε τις τιμές, όπως φαίνεται στο παρακάτω παράδειγμα.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Δείτε επίσης  

- [Βιβλιοθήκη προγράμματος-πελάτη Azure χώρου αποθήκευσης για αναφορά .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης για το πακέτο .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) 

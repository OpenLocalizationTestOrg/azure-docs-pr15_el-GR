<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob (χώρος αποθήκευσης αντικειμένων) από C++ | Microsoft Azure"
    description="Αποθήκευση μη δομημένα δεδομένα στο cloud με το χώρο αποθήκευσης αντικειμένων Blob του Azure (χώρος αποθήκευσης αντικειμένων)."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-blob-storage-from-c"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από C++  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Χώρο αποθήκευσης Blob του Azure είναι μια υπηρεσία που αποθηκεύει μη δομημένα δεδομένα στο cloud ως αντικείμενα/αντικείμενα blob. Χώρος αποθήκευσης αντικειμένων blob μπορούν να αποθηκεύσουν οποιονδήποτε τύπο κειμένου ή δυαδικά δεδομένα, όπως ένα έγγραφο, αρχείο πολυμέσων ή εφαρμογή του προγράμματος εγκατάστασης. Χώρος αποθήκευσης αντικειμένων blob επίσης αναφέρεται ως αντικείμενο χώρου αποθήκευσης.

Αυτός ο οδηγός θα δείχνουν πώς να εκτελείτε συνηθισμένα σενάρια με την υπηρεσία αποθήκευσης αντικειμένων Blob του Azure. Τα δείγματα είναι γραμμένες σε C++ και να χρησιμοποιήσετε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Τα σενάρια καλύπτεται περιλαμβάνουν **Αποστολή**, **καταχώρηση**, **τη λήψη**και **τη διαγραφή** αντικειμένων blob.  

>[AZURE.NOTE] Αυτός ο οδηγός ως προορισμό στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ έκδοση 1.0.0 και παραπάνω. Συνιστάται η έκδοση είναι βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης 2.2.0, που είναι διαθέσιμα μέσω [NuGet](http://www.nuget.org/packages/wastorage) ή [GitHub](https://github.com/Azure/azure-storage-cpp).

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Δημιουργία μιας εφαρμογής C++
Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες του χώρου αποθήκευσης που μπορεί να εκτελεστεί μέσα σε μια εφαρμογή C++.  

Για να το κάνετε αυτό, θα πρέπει να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ και δημιουργήστε ένα λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας στο Azure.   

Για να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++, μπορείτε να χρησιμοποιήσετε τις παρακάτω μεθόδους:

-   **Linux:** Ακολουθήστε τις οδηγίες που παρέχονται στη σελίδα [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για το αρχείο README C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Των Windows:** Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία > Διαχείριση πακέτου NuGet > Κονσόλα διαχείρισης πακέτου**. Πληκτρολογήστε την παρακάτω εντολή στην [Κονσόλα διαχείρισης πακέτου NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) και πατήστε το πλήκτρο **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Ρύθμιση παραμέτρων της εφαρμογής σας για πρόσβαση στο χώρο αποθήκευσης αντικειμένων Blob  
Προσθέστε τα εξής καταστάσεις στο επάνω μέρος του αρχείου C++ όπου θέλετε να χρησιμοποιήσετε το Azure αποθήκευσης APIs για να αποκτήσετε πρόσβαση σε αντικείμενα BLOB περιλαμβάνουν:  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Ρύθμιση μιας συμβολοσειράς σύνδεσης Azure χώρου αποθήκευσης
Ένα πρόγραμμα-πελάτη Azure αποθήκευσης χρησιμοποιεί μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης για να αποθηκεύσετε τα τελικά σημεία και τα διαπιστευτήρια για την πρόσβαση σε υπηρεσίες διαχείρισης δεδομένων. Όταν εκτελείται σε μια εφαρμογή προγράμματος-πελάτη, πρέπει να δώσετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης με την εξής μορφή, χρησιμοποιώντας το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης χώρου αποθήκευσης για το λογαριασμό χώρου αποθήκευσης που αναφέρονται στην [Πύλη του Azure](https://portal.azure.com) για το *όνομα λογαριασμού* και *AccountKey* τιμές. Για πληροφορίες σχετικά με τους λογαριασμούς χώρου αποθήκευσης και πλήκτρα πρόσβασης, ανατρέξτε στο θέμα [Σχετικά με τους λογαριασμούς Azure χώρου αποθήκευσης](storage-create-storage-account.md). Αυτό το παράδειγμα δείχνει πώς μπορείτε να δηλώσετε ένα στατικό πεδίο για τη διατήρηση τη συμβολοσειρά σύνδεσης:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Για να ελέγξετε την εφαρμογή σας στον τοπικό υπολογιστή με Windows, μπορείτε να χρησιμοποιήσετε το Microsoft Azure [προσομοίωσης χώρου αποθήκευσης](storage-use-emulator.md) που έχει εγκατασταθεί με το [Azure SDK](https://azure.microsoft.com/downloads/). Το προσομοίωσης χώρου αποθήκευσης είναι ένα βοηθητικό πρόγραμμα που προσομοιώνει τις υπηρεσίες Blob, ουρά και πίνακα που είναι διαθέσιμες στο Azure στον υπολογιστή σας τοπικής ανάπτυξης. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δηλώσετε ένα στατικό πεδίο για να κρατήστε τη συμβολοσειρά σύνδεσης για να σας προσομοίωσης τοπική αποθήκευση:

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Για να ξεκινήσετε το προσομοίωσης Azure χώρου αποθήκευσης, επιλέξτε το κουμπί " **Έναρξη** " ή πατήστε το πλήκτρο των **Windows** . Αρχίστε να πληκτρολογείτε **Azure προσομοίωσης χώρου αποθήκευσης**και επιλέξτε **Microsoft Azure αποθήκευσης προσομοίωσης** από τη λίστα των εφαρμογών.  

Στα παρακάτω παραδείγματα προϋποθέτουν ότι έχετε χρησιμοποιήσει μία από αυτές τις δύο μεθόδους για να λάβετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.  

## <a name="retrieve-your-connection-string"></a>Ανάκτηση συμβολοσειρά σύνδεσης
Μπορείτε να χρησιμοποιήσετε την κλάση **cloud_storage_account** για την αναπαράσταση των πληροφοριών λογαριασμού χώρου αποθήκευσης. Για να ανακτήσετε πληροφορίες του λογαριασμού σας χώρο αποθήκευσης από τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης, μπορείτε να χρησιμοποιήσετε τη μέθοδο **ανάλυση** .  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Στη συνέχεια, μπορείτε να λάβετε μια αναφορά σε ένα εκπαιδευτικό **cloud_blob_client** όπως σάς επιτρέπει να ανακτήσετε αντικείμενα που αντιπροσωπεύουν κοντέινερ και αντικείμενα BLOB αποθηκευτούν σε την υπηρεσία αποθήκευσης αντικειμένων Blob. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **cloud_blob_client** χρησιμοποιώντας το αντικείμενο λογαριασμού χώρου αποθήκευσης που θα σας ανάκτηση επάνω από:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Πώς μπορείτε να: δημιουργία κοντέινερ

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Αυτό το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα κοντέινερ, εάν δεν υπάρχει ήδη:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Από προεπιλογή, το νέο κοντέινερ είναι ιδιωτική και πρέπει να καθορίσετε τον αριθμό-κλειδί πρόσβασης χώρου αποθήκευσης για τη λήψη αντικείμενα BLOB από αυτό το κοντέινερ. Εάν θέλετε να καταστήσετε διαθέσιμα τα αρχεία (αντικείμενα BLOB) μέσα στο κοντέινερ για όλους τους χρήστες, μπορείτε να ρυθμίσετε το κοντέινερ για να είναι δημόσιες χρησιμοποιώντας τον ακόλουθο κώδικα:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Οποιοσδήποτε στο Internet μπορεί να δει αντικείμενα BLOB δημόσια κοντέινερ, αλλά μπορείτε να τροποποιήσετε ή να διαγράψετε μόνο εάν έχετε τον αριθμό-κλειδί κατάλληλα δικαιώματα πρόσβασης.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Πώς μπορείτε να: αποστείλετε μια blob σε ένα κοντέινερ
Χώρο αποθήκευσης Blob του Azure υποστηρίζει αντικείμενα BLOB μπλοκ και αντικείμενα BLOB σελίδας. Στις περισσότερες περιπτώσεις, blob μπλοκ είναι ο τύπος συνιστάται να χρησιμοποιήσετε.  

Για να αποστείλετε ένα αρχείο σε ένα μπλοκ blob, λάβετε μια αναφορά κοντέινερ και χρησιμοποιήστε το για να λάβετε μια αναφορά blob μπλοκ. Όταν έχετε μια αναφορά blob, μπορείτε να αποστείλετε οποιαδήποτε ροή δεδομένων σε αυτήν, καλώντας τη μέθοδο **upload_from_stream** . Αυτή η λειτουργία θα δημιουργήσει το αντικείμενο blob, εάν δεν έχετε προηγουμένως υπάρχουν ή να αντικαταστήσετε την περίπτωση που υπάρχει. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα blob σε ένα κοντέινερ και προϋποθέτει ότι έχει ήδη δημιουργήσει το κοντέινερ.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε τη μέθοδο **upload_from_file** για να αποστείλετε ένα αρχείο σε ένα μπλοκ blob.

## <a name="how-to-list-the-blobs-in-a-container"></a>Πώς μπορείτε να: λίστα τα αντικείμενα BLOB σε ένα κοντέινερ
Για να παραθέσετε τα αντικείμενα BLOB σε ένα κοντέινερ, λάβετε πρώτα μια αναφορά κοντέινερ. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε τη μέθοδο **list_blobs** στο κοντέινερ για να ανακτήσετε τα αντικείμενα BLOB ή/και σε καταλόγους μέσα σε αυτό. Για να αποκτήσετε πρόσβαση το εμπλουτισμένο σύνολο ιδιοτήτων και μεθόδων για μια επιστρέφεται **list_blob_item**, πρέπει να καλέσετε τη μέθοδο **list_blob_item.as_blob** για να λάβετε ένα αντικείμενο **cloud_blob** ή τη μέθοδο **list_blob.as_directory** για να λάβετε ένα αντικείμενο cloud_blob_directory. Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να ανακτήσετε και εξόδου το URI κάθε στοιχείου στο κοντέινερ **μου δείγμα κοντέινερ** :

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Για περισσότερες λεπτομέρειες σε καταχώρηση λειτουργίες, ανατρέξτε στο θέμα [Πόροι αποθήκευσης Azure λίστας σε C++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Πώς μπορείτε να: λήψη αντικείμενα BLOB
Για να κάνετε λήψη αντικείμενα blob, πρώτα να ανακτήσετε μια αναφορά blob και, στη συνέχεια, να καλέσετε τη μέθοδο **download_to_stream** . Το παρακάτω παράδειγμα χρησιμοποιεί τη μέθοδο **download_to_stream** για να μεταφέρετε τα περιεχόμενα blob σε ένα αντικείμενο ροής που, στη συνέχεια, μπορείτε να παραμένει σε ένα τοπικό αρχείο.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε τη μέθοδο **download_to_file** για να κάνετε λήψη των περιεχομένων μιας blob σε ένα αρχείο.
Επιπλέον, μπορείτε επίσης να χρησιμοποιήσετε τη μέθοδο **download_text** για να κάνετε λήψη των περιεχομένων μιας blob ως συμβολοσειρά κειμένου.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Πώς μπορείτε να: διαγραφή αντικείμενα BLOB
Για να διαγράψετε ένα blob, πρώτα λήψη μιας αναφοράς blob και, στη συνέχεια, να καλέσετε τη μέθοδο **delete_blob** σε αυτό.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που μάθατε τα βασικά στοιχεία του χώρο αποθήκευσης αντικειμένων blob, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure.  

-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Λίστα πόρων Azure αποθήκευσης σε C++](storage-c-plus-plus-enumeration.md)
-   [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά C++](http://azure.github.io/azure-storage-cpp)
-   [Τεκμηρίωση Azure χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/)
- [Μεταφορά δεδομένων με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](storage-use-azcopy.md)

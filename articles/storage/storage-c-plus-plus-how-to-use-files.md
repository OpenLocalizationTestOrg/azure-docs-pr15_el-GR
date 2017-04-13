<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων από C++ | Microsoft Azure"
    description="Αποθηκεύστε το αρχείο δεδομένων στο cloud με το χώρο αποθήκευσης αρχείων Azure."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αρχείων από C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Σχετικά με αυτό το πρόγραμμα εκμάθησης

Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να εκτελείτε βασικές λειτουργίες στην υπηρεσία Microsoft Azure αρχείου χώρου αποθήκευσης. Έως δείγματα γραμμένο σε C++, θα μάθετε πώς μπορείτε να δημιουργήσετε κοινόχρηστα στοιχεία και σε καταλόγους, αποστολή, λίστας και διαγραφή αρχείων. Εάν είστε εξοικειωμένοι με την υπηρεσία αποθήκευσης αρχείων στο Microsoft Azure, διέρχονται από τις έννοιες στις ενότητες που ακολουθούν θα είναι πολύ χρήσιμο στην κατανόηση των δειγμάτων.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Δημιουργία μιας εφαρμογής C++

Για να δημιουργήσετε τα δείγματα, θα πρέπει να εγκαταστήσετε τη βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης Azure 2.4.0 για C++. Θα πρέπει επίσης έχετε δημιουργήσει ένα λογαριασμό Azure χώρου αποθήκευσης.

Για να εγκαταστήσετε το πρόγραμμα-πελάτη αποθήκευσης Azure 2.4.0 για C++, μπορείτε να χρησιμοποιήσετε μία από τις παρακάτω μεθόδους:

-   **Linux:** Ακολουθήστε τις οδηγίες που παρέχονται στη σελίδα [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για το αρχείο README C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Των Windows:** Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία &gt; διαχείρισης πακέτου NuGet &gt; Κονσόλα διαχείρισης πακέτου**. Πληκτρολογήστε την παρακάτω εντολή στην [Κονσόλα διαχείρισης πακέτου NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) και πατήστε το πλήκτρο **ENTER**.

    Εγκατάσταση του πακέτου wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε το χώρο αποθήκευσης αρχείων

Προσθέστε τα εξής περιλαμβάνουν δηλώσεις στο επάνω μέρος του αρχείου C++ όπου θέλετε να χρησιμοποιήσετε το Azure αποθήκευσης APIs για να αποκτήσετε πρόσβαση σε αρχεία:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Ρυθμίσετε μια συμβολοσειρά σύνδεσης Azure χώρου αποθήκευσης

Για να χρησιμοποιήσετε το χώρο αποθήκευσης αρχείων, πρέπει να συνδεθείτε στο λογαριασμό σας Azure χώρου αποθήκευσης. Το πρώτο βήμα θα ήταν να ρυθμίσετε τις παραμέτρους μια συμβολοσειρά σύνδεσης, η οποία θα χρησιμοποιήσουμε για να συνδεθείτε στο λογαριασμό σας χώρου αποθήκευσης. Ας Καθορίστε μια στατική μεταβλητή για να το κάνετε.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Σύνδεση με ένα λογαριασμό Azure χώρου αποθήκευσης

Μπορείτε να χρησιμοποιήσετε την κλάση **cloud_storage_account** για την αναπαράσταση των πληροφοριών λογαριασμού χώρου αποθήκευσης. Για να ανακτήσετε πληροφορίες του λογαριασμού σας χώρο αποθήκευσης από τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης, μπορείτε να χρησιμοποιήσετε τη μέθοδο **ανάλυση** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Πώς μπορείτε να: Δημιουργήστε ένα κοινόχρηστο στοιχείο

Όλα τα αρχεία και καταλόγους στο χώρο αποθήκευσης αρχείων που βρίσκονται σε ένα κοντέινερ που ονομάζεται ένα **κοινή χρήση**. Το λογαριασμό χώρου αποθήκευσης μπορεί να έχει όσες μετοχών που επιτρέπει τη χωρητικότητα λογαριασμού. Για να αποκτήσετε πρόσβαση σε ένα κοινόχρηστο στοιχείο και τα περιεχόμενά της, πρέπει να χρησιμοποιήσετε ένα πρόγραμμα-πελάτη χώρο αποθήκευσης αρχείων.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Χρησιμοποιώντας το πρόγραμμα-πελάτη χώρου αποθήκευσης αρχείων, μπορείτε να αποκτήσετε, στη συνέχεια, μια αναφορά σε ένα κοινόχρηστο στοιχείο.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Για να δημιουργήσετε το κοινόχρηστο στοιχείο, χρησιμοποιήστε τη μέθοδο **create_if_not_exists** του αντικειμένου **cloud_file_share** .

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Σε αυτό το σημείο, η **κοινή χρήση** περιέχει μια αναφορά σε ένα κοινόχρηστο στοιχείο με το όνομα **my-δείγμα-κοινή χρήση**.

## <a name="how-to-upload-a-file"></a>Πώς μπορείτε να: Αποστολή αρχείου

Τουλάχιστον, μια κοινή χρήση χώρου αποθήκευσης του αρχείου Azure περιέχει έναν ριζικό κατάλογο όπου μπορεί να βρίσκονται τα αρχεία. Σε αυτήν την ενότητα, θα μάθετε πώς μπορείτε να αποστείλετε ένα αρχείο από τον τοπικό χώρο αποθήκευσης στον ριζικό κατάλογο της ένα κοινόχρηστο στοιχείο.

Είναι το πρώτο βήμα για την αποστολή ενός αρχείου για να αποκτήσετε μια αναφορά στον κατάλογο όπου θα πρέπει να βρίσκονται. Μπορείτε να το κάνετε αυτό, καλώντας τη μέθοδο **get_root_directory_reference** του αντικειμένου κοινή χρήση.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Τώρα που έχετε μια αναφορά στον ριζικό κατάλογο του κοινόχρηστου στοιχείου, μπορείτε να αποστείλετε ένα αρχείο σε αυτό. Αυτό το παράδειγμα αποστολές από ένα αρχείο από το κείμενο και από μια ροή.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Πώς μπορείτε να: δημιουργία καταλόγου

Μπορείτε επίσης να οργανώσετε χώρου αποθήκευσης με την τοποθέτηση αρχείων μέσα υποκατάλογοι αντί όλες τις στον ριζικό κατάλογο. Η υπηρεσία αποθήκευσης αρχείων Azure σας επιτρέπει να δημιουργήσετε όσο σε καταλόγους, καθώς επιτρέπει το λογαριασμό σας. Ο παρακάτω κώδικας θα δημιουργήσει έναν κατάλογο με όνομα **μου δείγμα καταλόγου** κάτω από τον ριζικό κατάλογο, καθώς και σε έναν κατάλογο με το όνομα **my-δείγμα-δευτερεύοντα κατάλογο**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Πώς μπορείτε να: λίστα αρχείων και καταλόγων σε ένα κοινόχρηστο στοιχείο

Απόκτηση μια λίστα αρχείων και καταλόγων μέσα σε ένα κοινόχρηστο στοιχείο γίνεται εύκολα με κλήση **list_files_and_directories** σε μια αναφορά **cloud_file_directory** . Για να αποκτήσετε πρόσβαση το εμπλουτισμένο σύνολο ιδιοτήτων και μεθόδων για ένα επιστρεφόμενο **list_file_and_directory_item**, πρέπει να καλέσετε τη μέθοδο **list_file_and_directory_item.as_file** για να λάβετε ένα αντικείμενο **cloud_file** ή τη μέθοδο **list_file_and_directory_item.as_directory** για να λάβετε ένα αντικείμενο **cloud_file_directory** .

Ο ακόλουθος κώδικας περιγράφει πώς μπορείτε να ανακτήσετε και εξόδου το URI κάθε στοιχείου στον ριζικό κατάλογο του κοινόχρηστου στοιχείου.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Πώς μπορείτε να: λήψη ενός αρχείου

Για να κάνετε λήψη αρχείων, πρώτα να ανακτήσετε μια αναφορά σε αρχείο και, στη συνέχεια, να καλέσετε τη μέθοδο **download_to_stream** για να μεταφέρετε τα περιεχόμενα του αρχείου σε ένα αντικείμενο ροής, το οποίο, στη συνέχεια, μπορείτε να παραμένει σε ένα τοπικό αρχείο. Εναλλακτικά, μπορείτε να χρησιμοποιήσετε τη μέθοδο **download_to_file** για να κάνετε λήψη των περιεχομένων ενός αρχείου σε ένα τοπικό αρχείο. Μπορείτε να χρησιμοποιήσετε τη μέθοδο **download_text** για να κάνετε λήψη τα περιεχόμενα του αρχείου ως συμβολοσειρά κειμένου.

Το παρακάτω παράδειγμα χρησιμοποιεί τις μεθόδους **download_to_stream** και **download_text** για μια επίδειξη τη λήψη των αρχείων που έχουν δημιουργηθεί σε προηγούμενες ενότητες.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Πώς μπορείτε να: διαγραφή αρχείου

Μια άλλη κοινές λειτουργία αποθήκευσης αρχείων είναι διαγραφή του αρχείου. Ο ακόλουθος κώδικας διαγράφει ένα αρχείο με το όνομα my-δείγμα-αρχείο-3 αποθηκευμένο κάτω από το ριζικό κατάλογο.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Πώς μπορείτε να: διαγράψετε έναν κατάλογο

Η διαγραφή ενός καταλόγου είναι μια απλή εργασία, παρόλο που θα πρέπει να σημειωθεί ότι δεν μπορείτε να διαγράψετε έναν κατάλογο που εξακολουθεί να περιέχει αρχεία ή άλλους καταλόγους.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Πώς μπορείτε να: διαγράψετε ένα κοινόχρηστο στοιχείο

Διαγραφή ενός κοινόχρηστου στοιχείου γίνεται καλώντας τη μέθοδο **delete_if_exists** σε ένα αντικείμενο cloud_file_share. Ακολουθεί δείγμα κώδικα που κάνει αυτό.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Ορίστε το μέγιστο μέγεθος για έναν κοινόχρηστο πόρο αρχείων

Μπορείτε να ορίσετε το όριο (ή μέγιστο μέγεθος) για έναν κοινόχρηστο πόρο αρχείων, σε gigabyte. Μπορείτε επίσης να ελέγξετε για να δείτε πόσο δεδομένα είναι αποθηκευμένα σε κοινή χρήση.

Με τη ρύθμιση του ορίου για ένα κοινόχρηστο στοιχείο, μπορείτε να περιορίσετε το συνολικό μέγεθος των αρχείων που είναι αποθηκευμένα σε κοινή χρήση. Εάν το συνολικό μέγεθος των αρχείων σε κοινή χρήση υπερβαίνει το όριο που ορίστηκε στην κοινόχρηστη θέση, στη συνέχεια, προγράμματα-πελάτες θα δεν είναι δυνατή η για να αυξήσετε το μέγεθος των υπαρχόντων αρχείων ή να δημιουργήσετε νέα αρχεία, εάν αυτά τα αρχεία είναι κενά.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για να ελέγξετε την τρέχουσα χρήση για έναν κοινόχρηστο πόρο και τον τρόπο ρύθμισης του ορίου για κοινή χρήση.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Δημιουργία υπογραφής κοινόχρηστη πρόσβαση για ένα αρχείο ή έναν κοινόχρηστο πόρο αρχείων

Μπορείτε να δημιουργήσετε μια κοινόχρηστη πρόσβαση υπογραφή (συσχετισμών Ασφαλείας) για ένα κοινόχρηστο αρχείο ή για ένα μεμονωμένο αρχείο. Μπορείτε επίσης να δημιουργήσετε μια πολιτική κοινόχρηστη πρόσβαση σε ένα κοινόχρηστο αρχείο για να διαχειριστείτε κοινόχρηστη πρόσβαση υπογραφές. Δημιουργία πολιτικής κοινόχρηστη πρόσβαση συνιστάται, όπως παρέχει ένα μέσο ανάκληση τις συσχετίσεις Ασφαλείας, εάν πρέπει να έχει παραβιαστεί.

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική κοινόχρηστη πρόσβαση σε ένα κοινόχρηστο στοιχείο και, στη συνέχεια, χρησιμοποιεί αυτήν την πολιτική για την παροχή τους περιορισμούς μια συσχετισμών Ασφαλείας σε ένα αρχείο στην κοινή χρήση.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία και χρήση υπογραφών κοινόχρηστη πρόσβαση, ανατρέξτε στο θέμα [Χρήση κοινόχρηστων Access υπογραφές (συσχετισμών Ασφαλείας)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure, εξερευνήστε αυτούς τους πόρους:

-   [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για C++](https://github.com/Azure/azure-storage-cpp)

-   [Εξερεύνηση Azure χώρου αποθήκευσης](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Τεκμηρίωση Azure χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/)

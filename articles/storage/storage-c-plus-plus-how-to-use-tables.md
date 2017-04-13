<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων (C++) | Microsoft Azure"
    description="Αποθηκεύστε δομημένων δεδομένων στο cloud χρησιμοποιώντας χώρος αποθήκευσης πινάκων του Azure, ένα χώρο αποθήκευσης δεδομένων NoSQL."
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

# <a name="how-to-use-table-storage-from-c"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από C++

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση  
Αυτός ο οδηγός θα σας δείξουν πώς μπορείτε να εκτελέσετε συνηθισμένα σενάρια, χρησιμοποιώντας την υπηρεσία αποθήκευσης Azure πίνακα. Τα δείγματα είναι γραμμένες σε C++ και να χρησιμοποιήσετε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία και τη διαγραφή ενός πίνακα** και την **εργασία με πίνακα οντοτήτων**.

>[AZURE.NOTE] Αυτός ο οδηγός ως προορισμό στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ έκδοση 1.0.0 και παραπάνω. Συνιστάται η έκδοση είναι βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης 2.2.0, που είναι διαθέσιμα μέσω [NuGet](http://www.nuget.org/packages/wastorage) ή [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Δημιουργία μιας εφαρμογής C++  
Σε αυτόν τον οδηγό, θα χρησιμοποιήσετε δυνατότητες του χώρου αποθήκευσης που μπορεί να εκτελεστεί μέσα σε μια εφαρμογή C++. Για να το κάνετε αυτό, θα πρέπει να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ και δημιουργήστε ένα λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας στο Azure.  

Για να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++, μπορείτε να χρησιμοποιήσετε τις παρακάτω μεθόδους:

-   **Linux:** Ακολουθήστε τις οδηγίες που παρέχονται στη σελίδα της [Βιβλιοθήκης προγράμματος-πελάτη του Azure χώρου αποθήκευσης για το αρχείο README C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Των Windows:** Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία > Διαχείριση πακέτου NuGet > Κονσόλα διαχείρισης πακέτου**. Πληκτρολογήστε την παρακάτω εντολή στην [Κονσόλα διαχείρισης πακέτου NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) και πατήστε το πλήκτρο Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για πρόσβαση στο χώρο αποθήκευσης πινάκων  
Προσθέστε τα εξής περιλαμβάνουν δηλώσεις στο επάνω μέρος του αρχείου C++ όπου θέλετε να χρησιμοποιήσετε το Azure αποθήκευσης APIs για να αποκτήσετε πρόσβαση σε πίνακες:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Ρυθμίσετε μια συμβολοσειρά σύνδεσης Azure χώρου αποθήκευσης  
Ένα πρόγραμμα-πελάτη Azure αποθήκευσης χρησιμοποιεί μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης για να αποθηκεύσετε τα τελικά σημεία και τα διαπιστευτήρια για την πρόσβαση σε υπηρεσίες διαχείρισης δεδομένων. Όταν εκτελείται μια εφαρμογή προγράμματος-πελάτη, πρέπει να δώσετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης με την εξής μορφή. Χρησιμοποιήστε το όνομα του λογαριασμού χώρου αποθήκευσης και το πλήκτρο πρόσβασης χώρου αποθήκευσης για το λογαριασμό χώρου αποθήκευσης που αναφέρονται στην [Πύλη του Azure](https://portal.azure.com) για το *όνομα λογαριασμού* και *AccountKey* τιμές. Για πληροφορίες σχετικά με τους λογαριασμούς χώρου αποθήκευσης και πλήκτρα πρόσβασης, ανατρέξτε στο θέμα [σχετικά με το Azure λογαριασμούς χώρου αποθήκευσης](storage-create-storage-account.md). Αυτό το παράδειγμα δείχνει πώς μπορείτε να δηλώσετε ένα στατικό πεδίο για τη διατήρηση τη συμβολοσειρά σύνδεσης:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Για να ελέγξετε την εφαρμογή σας στον τοπικό σας υπολογιστή με Windows, μπορείτε να χρησιμοποιήσετε το Azure [προσομοίωσης χώρου αποθήκευσης](storage-use-emulator.md) που έχει εγκατασταθεί με το [Azure SDK](https://azure.microsoft.com/downloads/). Το προσομοίωσης χώρου αποθήκευσης είναι ένα βοηθητικό πρόγραμμα που προσομοιώνει τις υπηρεσίες αντικειμένων Blob του Azure, ουρά και πίνακα που είναι διαθέσιμες στον υπολογιστή σας τοπικής ανάπτυξης. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δηλώσετε ένα στατικό πεδίο για να κρατήστε τη συμβολοσειρά σύνδεσης για να σας προσομοίωσης τοπική αποθήκευση:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Για να ξεκινήσετε το προσομοίωσης Azure χώρου αποθήκευσης, κάντε κλικ στο κουμπί **Έναρξη** ή πατήστε το πλήκτρο των Windows. Αρχίστε να πληκτρολογείτε **Azure προσομοίωσης χώρου αποθήκευσης**και κατόπιν επιλέξτε **Microsoft Azure αποθήκευσης προσομοίωσης** από τη λίστα των εφαρμογών.  

Στα παρακάτω παραδείγματα προϋποθέτουν ότι έχετε χρησιμοποιήσει μία από αυτές τις δύο μεθόδους για να λάβετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.  

## <a name="retrieve-your-connection-string"></a>Ανάκτηση συμβολοσειρά σύνδεσης  
Μπορείτε να χρησιμοποιήσετε την κλάση **cloud_storage_account** για την αναπαράσταση των πληροφοριών λογαριασμού χώρου αποθήκευσης. Για να ανακτήσετε πληροφορίες του λογαριασμού σας χώρο αποθήκευσης από τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης, μπορείτε να χρησιμοποιήσετε τη μέθοδο ανάλυσης.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Στη συνέχεια, μπορείτε να λάβετε μια αναφορά σε μια τάξη **cloud_table_client** , καθώς σας επιτρέπει να κάνετε λήψη αναφοράς αντικειμένων για πίνακες και οντοτήτων αποθηκευτούν σε την υπηρεσία αποθήκευσης πίνακα. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **cloud_table_client** χρησιμοποιώντας το αντικείμενο λογαριασμού χώρου αποθήκευσης που θα σας ανάκτηση επάνω από:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Δημιουργία πίνακα
Ένα αντικείμενο **cloud_table_client** σάς επιτρέπει να λάβετε αντικείμενα αναφοράς για τους πίνακες και οντοτήτων. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **cloud_table_client** και χρησιμοποιεί για να δημιουργήσετε έναν νέο πίνακα.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα
Για να προσθέσετε μια οντότητα σε έναν πίνακα, δημιουργήστε ένα νέο αντικείμενο **table_entity** και μεταβιβάζουν **table_operation::insert_entity**. Τον παρακάτω κώδικα χρησιμοποιεί ονόματος του πελάτη ως το κλειδί γραμμής και το επώνυμο ως το κλειδί διαμερίσματα. Μαζί, μια οντότητα διαμερίσματα και κλειδί γραμμή προσδιορίζει η οντότητα στον πίνακα. Οντοτήτων με το ίδιο κλειδί διαμερίσματα μπορούν να αναζητηθούν ταχύτερα από αυτές με αριθμούς-κλειδιά διαμερισμάτων διαφορετικά, αλλά με τη χρήση διάφορων τύπων partition κλειδιών επιτρέπει μεγαλύτερη δυνατότητα κλιμάκωσης παράλληλη λειτουργία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Microsoft Azure αποθήκευσης επιδόσεις και κλιμάκωση λίστα ελέγχου](storage-performance-checklist.md).

Ο ακόλουθος κώδικας δημιουργεί μια νέα παρουσία του **table_entity** με ορισμένα δεδομένα πελατών για να αποθηκευτεί. Ο κώδικας καλεί **table_operation::insert_entity** για να δημιουργήσετε ένα αντικείμενο **table_operation** για να εισαγάγετε μια οντότητα σε έναν πίνακα και συσχετίζει τη νέα οντότητα πίνακα με το. Τέλος, ο κώδικας καλεί τη μέθοδο execute στο αντικείμενο **cloud_table** . Και το νέο **table_operation** στέλνει την αίτηση για την υπηρεσία πίνακα για να εισαγάγετε τη νέα οντότητα πελάτη στον πίνακα "άτομα".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Εισαγάγετε μια δέσμη οντοτήτων
Μπορείτε να εισαγάγετε μια δέσμη οντοτήτων με την υπηρεσία του πίνακα σε μια λειτουργία εγγραφής. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **table_batch_operation** και, στη συνέχεια, προσθέτει τρία εισαγωγή λειτουργίες σε αυτήν. Κάθε λειτουργία εισαγωγής προστίθεται από τη δημιουργία ενός νέου αντικειμένου οντότητα, τη ρύθμιση τις τιμές και, στη συνέχεια, καλώντας τη μέθοδο εισαγωγής στο αντικείμενο **table_batch_operation** για να συσχετίσετε την οντότητα με μια νέα λειτουργία εισαγωγής. Στη συνέχεια, **cloud_table.execute** ονομάζεται για την εκτέλεση της λειτουργίας.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Ορισμένα πράγματα που πρέπει να λάβετε υπόψη σε λειτουργίες δέσμης:  

-   Μπορείτε να εκτελέσετε έως 100 εισαγωγή, διαγραφή, τη συγχώνευση, αντικατάσταση, εισαγωγή ή συγχώνευση και εισαγωγή ή αντικατάσταση των λειτουργιών σε οποιονδήποτε συνδυασμό σε μια μεμονωμένη δέσμη.  
-   Μια λειτουργία δέσμης μπορεί να έχει μια λειτουργία ανάκτηση, εάν είναι η μόνη λειτουργία στη δέσμη.  
-   Όλα οντοτήτων σε μια λειτουργία μαζικά πρέπει να έχει το ίδιο κλειδί διαμερίσματα.  
-   Μια λειτουργία δέσμης περιορίζεται σε ένα φορτίο δεδομένων 4-MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Ανάκτηση όλα οντοτήτων ένα διαμερίσματα
Ερώτημα για έναν πίνακα για όλους οντοτήτων ένα διαμερίσματα, χρησιμοποιήστε ένα αντικείμενο **table_query** . Το ακόλουθο παράδειγμα κώδικα καθορίζει ένα φίλτρο για οντοτήτων όπου 'Smith' είναι το πλήκτρο διαμερίσματα. Αυτό το παράδειγμα εκτυπώνει τα πεδία από κάθε οντότητα στα αποτελέσματα του ερωτήματος στην κονσόλα.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Το ερώτημα σε αυτό το παράδειγμα εμφανίζει όλα τα οντοτήτων που ικανοποιούν τα κριτήρια φίλτρου. Εάν έχετε μεγάλους πίνακες και πρέπει να κάνετε λήψη των οντοτήτων πίνακα συχνά, συνιστάται να αποθηκεύετε τα δεδομένα σας σε Azure χώρο αποθήκευσης αντικειμένων blob αντί για αυτό.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Ανάκτηση μιας περιοχής των οντοτήτων ένα διαμερίσματα
Εάν δεν θέλετε ερώτημα για όλα τα οντοτήτων ένα διαμερίσματα, μπορείτε να καθορίσετε μια περιοχή συνδυάζοντας το φίλτρο κλειδιού διαμερίσματα με φίλτρο βασικοί γραμμής. Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί δύο φίλτρα για όλους οντοτήτων σε διαμερίσματα 'Smith' όπου το κλειδί γραμμής (όνομα) ξεκινά νωρίτερα από "E" στο αλφάβητο με γράμμα και, στη συνέχεια, να εκτυπώνει τα αποτελέσματα του ερωτήματος.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Ανάκτηση μία οντότητα
Μπορείτε να συντάξετε ένα ερώτημα για να ανακτήσετε μια συγκεκριμένη οντότητα. Ο ακόλουθος κώδικας χρησιμοποιεί **table_operation::retrieve_entity** για να καθορίσετε τον πελάτη 'Jeff Smith'. Αυτή η μέθοδος επιστρέφει μόνο μία οντότητα, αντί για μια συλλογή και την επιστρεφόμενη τιμή είναι στο **table_result**. Καθορισμός κλειδιών διαμερίσματα και γραμμών σε ένα ερώτημα είναι ο πιο γρήγορος τρόπος για να ανακτήσετε μια μοναδική οντότητα από την υπηρεσία του πίνακα.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Αντικατάσταση μιας οντότητας
Για να αντικαταστήσετε μια οντότητα, να το ανακτήσετε από την υπηρεσία του πίνακα, η τροποποίηση του αντικειμένου οντότητα και, στη συνέχεια, να αποθηκεύσετε τις αλλαγές ξανά με την υπηρεσία του πίνακα. Ο ακόλουθος κώδικας αλλάζει έναν υπάρχοντα πελάτη τηλέφωνο αριθμός τη διεύθυνση ηλεκτρονικού ταχυδρομείου. Αντί για κλήση **table_operation::insert_entity**, αυτός ο κωδικός χρησιμοποιεί **table_operation::replace_entity**. Αυτό θα κάνει η οντότητα να αντικατασταθούν πλήρως στο διακομιστή, εκτός εάν η οντότητα στο διακομιστή έχει αλλάξει μετά την ανάκτησή, οπότε η λειτουργία θα αποτύχει. Αυτό το σφάλμα είναι για να αποτρέψετε την εφαρμογή σας από αντικαταστήσει κατά λάθος μια αλλαγή που έγιναν μεταξύ της ανάκτησης και ενημέρωση από ένα άλλο στοιχείο της εφαρμογής σας. Ο χειρισμός των αυτό το σφάλμα είναι να ανακτήσετε η οντότητα ξανά, κάντε τις αλλαγές σας (Εάν εξακολουθεί να ισχύει) και, στη συνέχεια, εκτελέστε μια άλλη λειτουργία **table_operation::replace_entity** . Στην επόμενη ενότητα θα σας δείξουν πώς μπορείτε να παρακάμψετε αυτήν τη συμπεριφορά.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Εισαγωγή-ή-αντικατάσταση μια οντότητα
λειτουργίες **table_operation::replace_entity** θα αποτύχει, εάν η οντότητα έχει αλλάξει μετά την ανάκτησή από το διακομιστή. Επιπλέον, πρέπει να ανακτήσετε την οντότητα από το διακομιστή πρώτα στη σειρά για **table_operation::replace_entity** να ολοκληρωθεί με επιτυχία. Ορισμένες φορές, ωστόσο, δεν γνωρίζετε εάν υπάρχει η οντότητα στο διακομιστή και τις τρέχουσες τιμές που είναι αποθηκευμένα σε αυτό είναι σημασία — ενημέρωση σας θα πρέπει να αντικαταστήσετε τις όλες. Για να γίνει αυτό, μπορείτε να χρησιμοποιήσετε μια λειτουργία **table_operation::insert_or_replace_entity** . Αυτή η λειτουργία εισάγει την οντότητα, αν δεν υπάρχει, ή αντικαθιστά το, εάν υπάρχει, ανεξάρτητα από το πότε έγινε η τελευταία ενημέρωση. Στο παρακάτω παράδειγμα κώδικα, η οντότητα πελατών για Jeff Smith εξακολουθεί να ανάκτηση, αλλά, στη συνέχεια, αποθηκεύεται στο διακομιστή μέσω **table_operation::insert_or_replace_entity**. Οι ενημερώσεις που κάνατε στην οντότητα μεταξύ της ανάκτησης και λειτουργία ενημέρωσης θα αντικατασταθούν.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Ένα υποσύνολο οντότητα ιδιοτήτων ερωτήματος  
Ένα ερώτημα σε έναν πίνακα μπορεί να ανακτήσει ιδιότητες μερικά μόνο από μια οντότητα. Το ερώτημα στο ακόλουθο κώδικα χρησιμοποιεί τη μέθοδο **table_query::set_select_columns** για να επιστρέψετε μόνο τις διευθύνσεις ηλεκτρονικού ταχυδρομείου των οντοτήτων στον πίνακα.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Υποβολή ερωτημάτων μερικές ιδιότητες από μια οντότητα είναι μια πιο αποτελεσματική λειτουργία από την ανάκτηση όλων των ιδιοτήτων.

## <a name="delete-an-entity"></a>Διαγραφή οντότητας
Μπορείτε εύκολα να διαγράψετε μια οντότητα μετά την ανάκτηση του. Όταν ανακτηθούν η οντότητα, καλέστε **table_operation::delete_entity** με την οντότητα για να διαγράψετε. Στη συνέχεια, καλέστε τη μέθοδο **cloud_table.execute** . Ο ακόλουθος κώδικας ανακτά και διαγράφει μια οντότητα με έναν αριθμό-κλειδί διαμερίσματα της "Κοίτα" και ένα κλειδί γραμμής "Jeff".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Διαγραφή ενός πίνακα
Τέλος, το ακόλουθο παράδειγμα κώδικα Διαγράφει έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης. Θα είναι διαθέσιμη για να δημιουργηθεί εκ νέου για κάποιο χρονικό διάστημα μετά τη διαγραφή ενός πίνακα που έχει διαγραφεί.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που μάθατε τα βασικά στοιχεία του πίνακα χώρου αποθήκευσης, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure:  

-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Λίστα πόρων Azure αποθήκευσης σε C++](storage-c-plus-plus-enumeration.md)
-   [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά C++](http://azure.github.io/azure-storage-cpp)
-   [Azure τεκμηρίωση χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/)

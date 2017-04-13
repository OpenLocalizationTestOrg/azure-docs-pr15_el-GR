<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από Java | Microsoft Azure"
    description="Αποθηκεύστε δομημένων δεδομένων στο cloud χρησιμοποιώντας χώρος αποθήκευσης πινάκων του Azure, ένα χώρο αποθήκευσης δεδομένων NoSQL."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός θα σας δείξουν πώς να εκτελείτε συνηθισμένα σενάρια με την υπηρεσία αποθήκευσης Azure πίνακα. Τα δείγματα είναι γραμμένες σε Java και χρησιμοποιήστε το [Azure SDK χώρου αποθήκευσης για Java][]. Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία**, **καταχώρηση**και **Διαγραφή** πινάκων, καθώς και **Εισαγωγή**, **Υποβολή ερωτημάτων**, **Τροποποίηση**και **Διαγραφή** οντοτήτων σε έναν πίνακα. Για περισσότερες πληροφορίες σχετικά με τους πίνακες, ανατρέξτε στην ενότητα [επόμενα βήματα](#Next-Steps) .

Σημείωση: Μια SDK είναι διαθέσιμη για τους προγραμματιστές που χρησιμοποιούν το χώρο αποθήκευσης Azure σε συσκευές Android. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το [Azure SDK χώρου αποθήκευσης για Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Δημιουργία μιας εφαρμογής Java

Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες του χώρου αποθήκευσης που μπορεί να εκτελεστεί μέσα σε μια εφαρμογή Java τοπικά ή σε κώδικα που εκτελείται μέσα σε ένα ρόλο web ή εργαζόμενου ρόλος στο Azure.

Για να το κάνετε αυτό, θα πρέπει να εγκαταστήσετε το Java Development Kit (JDK) και να δημιουργήσετε ένα λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας στο Azure. Αφού το έχετε κάνει, θα πρέπει να βεβαιωθείτε ότι το σύστημα ανάπτυξης πληροί τις ελάχιστες απαιτήσεις και τις εξαρτήσεις, κάτι που παρατίθενται στο αποθετήριο [Azure SDK χώρου αποθήκευσης για Java][] σε GitHub. Εάν το σύστημά σας πληροί αυτές τις απαιτήσεις, μπορείτε να ακολουθήσετε τις οδηγίες για τη λήψη και εγκατάσταση των βιβλιοθηκών Azure χώρου αποθήκευσης για Java στο σύστημά σας από συγκεκριμένο χώρο αποθήκευσης. Αφού ολοκληρώσετε αυτές τις εργασίες, θα μπορείτε να δημιουργήσετε μια εφαρμογή Java που χρησιμοποιεί τα παραδείγματα σε αυτό το άρθρο.

## <a name="configure-your-application-to-access-table-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για πρόσβαση στο χώρο αποθήκευσης πινάκων

Προσθέστε τις ακόλουθες δηλώσεις εισαγωγής στο επάνω μέρος του αρχείου Java όπου θέλετε να χρησιμοποιήσετε αποθήκευσης Azure APIs για να αποκτήσετε πρόσβαση σε πίνακες:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Ρύθμιση μιας συμβολοσειράς σύνδεσης Azure χώρου αποθήκευσης

Ένα πρόγραμμα-πελάτη Azure αποθήκευσης χρησιμοποιεί μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης για να αποθηκεύσετε τα τελικά σημεία και τα διαπιστευτήρια για την πρόσβαση σε υπηρεσίες διαχείρισης δεδομένων. Όταν εκτελείται σε μια εφαρμογή προγράμματος-πελάτη, πρέπει να δώσετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης με την εξής μορφή, χρησιμοποιώντας το όνομα του λογαριασμού χώρου αποθήκευσης και τον αριθμό-κλειδί πρωτεύοντος πρόσβασης για το λογαριασμό χώρου αποθήκευσης που αναφέρονται στην [Πύλη του Azure](https://portal.azure.com) για το *όνομα λογαριασμού* και *AccountKey* τιμές. Αυτό το παράδειγμα δείχνει πώς μπορείτε να δηλώσετε ένα στατικό πεδίο για τη διατήρηση τη συμβολοσειρά σύνδεσης:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Σε μια εφαρμογή που εκτελούνται μέσα σε ένα ρόλο στο Microsoft Azure, αυτή η συμβολοσειρά μπορούν να αποθηκευτούν στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας, *ServiceConfiguration.cscfg*, και είναι δυνατή η πρόσβαση με μια κλήση προς τη μέθοδο **RoleEnvironment.getConfigurationSettings** . Ακολουθεί ένα παράδειγμα γρήγορα τη συμβολοσειρά σύνδεσης από ένα στοιχείο **τη ρύθμιση** που ονομάζεται *StorageConnectionString* στο αρχείο ρύθμισης παραμέτρων της υπηρεσίας:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Στα παρακάτω παραδείγματα προϋποθέτουν ότι έχετε χρησιμοποιήσει μία από αυτές τις δύο μεθόδους για να λάβετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης.

## <a name="how-to-create-a-table"></a>Πώς μπορείτε να: Δημιουργία πίνακα

Ένα αντικείμενο **CloudTableClient** σάς επιτρέπει να λάβετε αντικείμενα αναφοράς για τους πίνακες και οντοτήτων. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **CloudTableClient** και χρησιμοποιεί για να δημιουργήσετε ένα νέο αντικείμενο **CloudTable** που αντιπροσωπεύει έναν πίνακα που ονομάζεται "άτομα". (Σημείωση: υπάρχουν επιπλέον τρόποι για να δημιουργήσετε αντικείμενα **CloudStorageAccount** ; για περισσότερες πληροφορίες, ανατρέξτε στο θέμα **CloudStorageAccount** στην περιοχή [Αναφοράς SDK Azure αποθήκευσης υπολογιστή-πελάτη].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Πώς μπορείτε να: λίστα των πινάκων

Για να λάβετε μια λίστα με τους πίνακες, καλέστε τη μέθοδο **CloudTableClient.listTables()** για να ανακτήσετε μια iterable λίστα με τα ονόματα πινάκων.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Πώς μπορείτε να: Προσθέστε μια οντότητα σε έναν πίνακα

Αντιστοιχίστε αντικείμενα Java χρησιμοποιώντας μια προσαρμοσμένη κλάση εφαρμογής **TableEntity**οντοτήτων. Για τη διευκόλυνσή σας, την κλάση **TableServiceEntity** υλοποιεί **TableEntity** και χρησιμοποιεί αντανάκλαση για την αντιστοίχιση ιδιοτήτων, με αποτέλεσμα την και setter μεθόδους με το όνομα για τις ιδιότητες. Για να προσθέσετε μια οντότητα σε έναν πίνακα, πρέπει πρώτα να δημιουργήσετε μιας κλάσης που ορίζει τις ιδιότητες του σας οντότητα. Ο ακόλουθος κώδικας καθορίζει μια κλάση οντότητα που χρησιμοποιεί το όνομά του πελάτη ως γραμμή κλειδί και επώνυμο ως το κλειδί διαμερίσματα. Μαζί, μια οντότητα διαμερίσματα και κλειδί γραμμή προσδιορίζει η οντότητα στον πίνακα. Μπορούν να αναζητηθούν ταχύτερα από αυτές με διαφορετικά διαμερίσματα πλήκτρα οντοτήτων με το ίδιο κλειδί διαμερίσματα.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Λειτουργίες πίνακα που αφορούν οντοτήτων απαιτούν ένα αντικείμενο **TableOperation** . Αυτό το αντικείμενο Καθορίζει τη λειτουργία να εκτελεστεί σε μια οντότητα, η οποία μπορεί να εκτελεστεί με ένα αντικείμενο **CloudTable** . Ο ακόλουθος κώδικας δημιουργεί μια νέα παρουσία της κλάσης **CustomerEntity** με ορισμένα δεδομένα πελατών για να αποθηκευτεί. Ο κώδικας καλεί **TableOperation.insertOrReplace** για να δημιουργήσετε ένα αντικείμενο **TableOperation** για να εισαγάγετε μια οντότητα σε έναν πίνακα και συσχετίζει το νέο **CustomerEntity** με το. Τέλος, ο κώδικας καλεί τη μέθοδο **Εκτέλεση** στο αντικείμενο **CloudTable** , που καθορίζει τον πίνακα "άτομα" και το νέο **TableOperation**, που στέλνει την αίτηση, στη συνέχεια, με την υπηρεσία αποθήκευσης για να εισαγάγετε τη νέα οντότητα πελάτη στον πίνακα "άτομα" ή αντικαταστήστε την οντότητα, εάν υπάρχει ήδη.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Πώς μπορείτε να: Εισαγάγετε μια δέσμη οντοτήτων

Μπορείτε να εισαγάγετε μια δέσμη οντοτήτων με την υπηρεσία του πίνακα σε μια λειτουργία εγγραφής. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **TableBatchOperation** και, στη συνέχεια, προσθέτει τρία εισαγωγή λειτουργίες σε αυτήν. Κάθε λειτουργία εισαγωγής προστίθεται από τη δημιουργία ενός νέου αντικειμένου οντότητα, τη ρύθμιση τις τιμές και, στη συνέχεια, καλώντας τη μέθοδο **Εισαγωγή** του αντικειμένου **TableBatchOperation** για να συσχετίσετε την οντότητα με μια νέα λειτουργία εισαγωγής. Στη συνέχεια, τον κωδικό κλήσεις που **εκτελεί** στον το αντικείμενο **CloudTable** , που καθορίζει τον πίνακα "άτομα" και το αντικείμενο **TableBatchOperation** , το οποίο αποστέλλει τη δέσμη λειτουργιών πίνακα με την υπηρεσία αποθήκευσης σε μία μόνο αίτηση.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Ορισμένα πράγματα που πρέπει να λάβετε υπόψη σε λειτουργίες δέσμης:

- Μπορείτε να εκτελέσετε έως 100 εισαγωγή, διαγραφή, συγχώνευση, αντικατάσταση, εισαγωγή ή συγχώνευση, και εισαγάγετε ή αντικαταστήστε τις εργασίες σε οποιονδήποτε συνδυασμό σε μια μεμονωμένη δέσμη.
- Μια λειτουργία δέσμης μπορεί να έχει μια λειτουργία ανάκτηση, εάν είναι η μόνη λειτουργία στη δέσμη.
- Όλα οντοτήτων σε μια λειτουργία μαζικά πρέπει να έχει το ίδιο κλειδί διαμερίσματα.
- Μια λειτουργία δέσμης περιορίζεται σε ένα φορτίο δεδομένων 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Πώς μπορείτε να: ανάκτηση όλα οντοτήτων ένα διαμερίσματα

Ερώτημα για έναν πίνακα για οντοτήτων ένα διαμερίσματα, μπορείτε να χρησιμοποιήσετε ένα **TableQuery**. Κλήση **TableQuery.from** για να δημιουργήσετε ένα ερώτημα σε ένα συγκεκριμένο πίνακα που επιστρέφει έναν τύπο αποτελεσμάτων καθορισμένο. Ο ακόλουθος κώδικας καθορίζει ένα φίλτρο για οντοτήτων όπου 'Smith' είναι το πλήκτρο διαμερίσματα. **TableQuery.generateFilterCondition** είναι μια μέθοδος Βοήθειας για να δημιουργήσετε φίλτρα για ερωτήματα. Κλήση **όπου** από την αναφορά που επιστρέφονται από τη μέθοδο **TableQuery.from** για να εφαρμόσετε το φίλτρο στο ερώτημα. Όταν το ερώτημα εκτελείται με μια κλήση για την **Εκτέλεση** του αντικειμένου **CloudTable** , η συνάρτηση επιστρέφει έναν **επαναλήπτη** με τον τύπο αποτελεσμάτων **CustomerEntity** που καθορίζεται. Μπορείτε να χρησιμοποιήσετε το **επαναλήπτη** επιστρέφονται στο ένα για κάθε βρόχου για την εκμετάλλευση των αποτελεσμάτων. Αυτός ο κωδικός εκτυπώνει τα πεδία από κάθε οντότητα στα αποτελέσματα του ερωτήματος στην κονσόλα.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Πώς μπορείτε να: ανάκτηση μιας περιοχής των οντοτήτων ένα διαμερίσματα

Εάν δεν θέλετε ερώτημα για όλα τα οντοτήτων ένα διαμερίσματα, μπορείτε να καθορίσετε μια περιοχή χρησιμοποιώντας τελεστές σύγκρισης σε ένα φίλτρο. Ο ακόλουθος κώδικας συνδυάζει δύο φίλτρα για να λάβετε όλα οντοτήτων partition "Κοίτα", όπου το πλήκτρο γραμμής (όνομα) ξεκινά με ένα γράμμα έως και "E" της αλφαβήτου. Στη συνέχεια, αυτό εκτυπώνεται τα αποτελέσματα του ερωτήματος. Εάν χρησιμοποιείτε το οντοτήτων προστεθεί στον πίνακα στη δέσμη εισαγωγή ενότητα αυτού του οδηγού, μόνο δύο οντοτήτων επιστρέφονται αυτήν τη στιγμή (Βασίλη και Γιώργος) Jeff Smith δεν περιλαμβάνεται.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Πώς μπορείτε να: ανάκτηση μία οντότητα

Μπορείτε να συντάξετε ένα ερώτημα για να ανακτήσετε μια συγκεκριμένη οντότητα. Ο ακόλουθος κώδικας κλήσεις **TableOperation.retrieve** με αριθμό-κλειδί διαμερίσματα και γραμμή παραμέτρων κλειδιού για να καθορίσετε τον πελάτη "Jeff κοίτα", αντί για τη δημιουργία ενός **TableQuery** και χρησιμοποιώντας φίλτρα για να κάνετε το ίδιο. Όταν εκτελείται, η λειτουργία ανάκτηση επιστρέφει μόνο μία οντότητα, αντί για μια συλλογή. Η μέθοδος **getResultAsType** μετατρέπει ρητά το αποτέλεσμα με τον τύπο του στόχου της ανάθεσης, ένα αντικείμενο **CustomerEntity** . Εάν αυτός ο τύπος δεν είναι συμβατή με τον τύπο που καθορίζεται για το ερώτημα, μια θα είναι εξαίρεση. Τιμή null επιστρέφεται εάν δεν υπάρχει η οντότητα έχει μια ακριβή διαμερίσματα και συμφωνούν με αριθμό-κλειδί γραμμής. Καθορισμός κλειδιών διαμερίσματα και γραμμών σε ένα ερώτημα είναι ο πιο γρήγορος τρόπος για να ανακτήσετε μια μοναδική οντότητα από την υπηρεσία του πίνακα.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Πώς μπορείτε να: τροποποίηση μιας οντότητας

Για να τροποποιήσετε μια οντότητα, να το ανακτήσετε από την υπηρεσία του πίνακα, κάντε αλλαγές στο αντικείμενο οντότητα και αποθηκεύσετε τις αλλαγές ξανά με την υπηρεσία του πίνακα με τη λειτουργία αντικατάστασης ή συγχώνευσης. Ο ακόλουθος κώδικας αλλάζει αριθμού τηλεφώνου έναν υπάρχοντα πελάτη. Αντί για κλήση **TableOperation.insert** όπως κάναμε για να εισαγάγετε, αυτός ο κωδικός κλήσεις **TableOperation.replace**. Η μέθοδος **CloudTable.execute** κλήσεις στην υπηρεσία πίνακα και έχει αντικατασταθεί η οντότητα, εκτός εάν κάποια άλλη εφαρμογή αλλάξει το στην ώρα από την ανάκτηση αυτής της εφαρμογής του. Όταν συμβεί αυτό, μια εξαίρεση και η οντότητα πρέπει να είναι ανάκτηση τροποποιήσατε και αποθηκεύσατε ξανά. Σε αυτό το μοτίβο "Επανάληψη" βέλτιστου είναι κοινά σε ένα σύστημα κατανέμεται αποθήκευσης.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Πώς μπορείτε να: ένα υποσύνολο οντότητα ιδιοτήτων ερωτήματος

Ένα ερώτημα σε έναν πίνακα μπορεί να ανακτήσει ιδιότητες μερικά μόνο από μια οντότητα. Αυτήν την τεχνική, που ονομάζεται προβολή, μειώνει το εύρος ζώνης και να βελτιώσετε τις επιδόσεις του ερωτήματος, ειδικά για μεγάλη οντοτήτων. Το ερώτημα στο ακόλουθο κώδικα χρησιμοποιεί τη μέθοδο **Επιλέξτε** για να επιστρέψετε μόνο τις διευθύνσεις ηλεκτρονικού ταχυδρομείου των οντοτήτων στον πίνακα. Τα αποτελέσματα είναι προβλεπόμενου σε μια συλλογή της **συμβολοσειράς** με τη βοήθεια ενός **EntityResolver**, που λειτουργεί η μετατροπή του τύπου σε των οντοτήτων που επιστρέφονται από το διακομιστή. Μπορείτε να μάθετε περισσότερα σχετικά με την προβολή, σε [Azure πίνακες: εισαγωγή Upsert και προβολής ερωτήματος][]. Σημειώστε ότι προβολής δεν υποστηρίζεται σε προσομοίωσης του τοπικού χώρου αποθήκευσης, ώστε να εκτελείται αυτόν τον κωδικό μόνο όταν χρησιμοποιείτε ένα λογαριασμό την υπηρεσία του πίνακα.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Πώς μπορείτε να: εισαγωγή ή αντικατάσταση μιας οντότητας

Συχνά θέλετε να προσθέσετε μια οντότητα σε έναν πίνακα χωρίς να γνωρίζετε εάν υπάρχει ήδη στον πίνακα. Μια λειτουργία Εισαγωγή ή αντικατάσταση σάς επιτρέπει να κάνετε μια μεμονωμένη αίτηση που θα εισαγάγει η οντότητα, εάν δεν υπάρχει ή αντικαταστήστε το υπάρχον εάν κάνει. Δημιουργία στην προηγούμενη παραδείγματα, ο ακόλουθος κώδικας εισάγει ή αντικαθιστά η οντότητα για "Walter Harp". Αφού δημιουργήσετε μια νέα οντότητα, αυτός ο κώδικας καλεί τη μέθοδο **TableOperation.insertOrReplace** . Αυτός ο κωδικός, στη συνέχεια, καλεί **Εκτέλεση** στο αντικείμενο **CloudTable** με τον πίνακα και την εισαγωγή ή αντικατάσταση λειτουργία πίνακα ως τις παραμέτρους. Για να ενημερώσετε μόνο ένα μέρος μιας οντότητας, η μέθοδος **TableOperation.insertOrMerge** μπορεί να χρησιμοποιηθεί αντί για αυτό. Σημείωση που εισαγωγή-ή-αντικατάσταση δεν υποστηρίζεται σε προσομοίωσης του τοπικού χώρου αποθήκευσης, ώστε να εκτελείται αυτόν τον κωδικό μόνο όταν χρησιμοποιείτε ένα λογαριασμό την υπηρεσία του πίνακα. Μπορείτε να μάθετε περισσότερα σχετικά με εισαγωγή ή αντικατάσταση και εισαγωγή ή συγχώνευση στο εξής [Azure πίνακες: εισαγωγή Upsert και προβολής ερωτήματος][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Πώς μπορείτε να: Διαγραφή οντότητας

Μπορείτε εύκολα να διαγράψετε μια οντότητα μετά την ανάκτηση του. Όταν ανακτηθούν η οντότητα, καλέστε **TableOperation.delete** με την οντότητα για να διαγράψετε. Στη συνέχεια, καλέστε την **Εκτέλεση** του αντικειμένου **CloudTable** . Ο ακόλουθος κώδικας ανακτά και διαγράφει ένα οντότητα του πελάτη.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Πώς μπορείτε να: διαγραφή ενός πίνακα

Τέλος, ο ακόλουθος κώδικας Διαγράφει έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης. Έναν πίνακα που έχει διαγραφεί θα είναι διαθέσιμο για να δημιουργήσετε ξανά για κάποιο χρονικό διάστημα μετά τη διαγραφή, συνήθως λιγότερο από 40 δευτερόλεπτα.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του πίνακα χώρου αποθήκευσης, ακολουθήστε αυτές τις συνδέσεις για να μάθετε πώς μπορείτε να κάνετε πιο σύνθετες εργασίες χώρου αποθήκευσης.

- [Azure αποθήκευσης SDK για Java][]
- [Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη][]
- [Azure αποθήκευσης REST API][]
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης][]

Για περισσότερες πληροφορίες, ανατρέξτε επίσης στο [Κέντρο για προγραμματιστές Java](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure αποθήκευσης SDK για Java]: https://github.com/azure/azure-storage-java
[Azure αποθήκευσης SDK για Android]: https://github.com/azure/azure-storage-android
[Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη]: http://dl.windowsazure.com/storage/javadoc/
[Azure αποθήκευσης REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure πίνακες: Εισαγωγή στις Upsert και προβολής ερωτήματος]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

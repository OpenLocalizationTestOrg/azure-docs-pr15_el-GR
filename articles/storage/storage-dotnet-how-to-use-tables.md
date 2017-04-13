<properties
    pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET | Microsoft Azure"
    description="Αποθηκεύστε δομημένων δεδομένων στο cloud χρησιμοποιώντας χώρος αποθήκευσης πινάκων του Azure, ένα χώρο αποθήκευσης δεδομένων NoSQL."
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


# <a name="get-started-with-azure-table-storage-using-net"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Επισκόπηση

Χώρος αποθήκευσης πινάκων του Azure είναι μια υπηρεσία που αποθηκεύει δομημένα δεδομένα NoSQL στο cloud. Χώρος αποθήκευσης πινάκων είναι ένα πλήκτρο/χαρακτηριστικό χώρο αποθήκευσης με schemaless σχεδίαση. Επειδή το χώρο αποθήκευσης πινάκων είναι schemaless, είναι εύκολο να προσαρμόσετε τα δεδομένα σας με τις ανάγκες σας evolve εφαρμογής. Πρόσβαση σε δεδομένα είναι γρήγορη και οικονομικός για όλα τα είδη των εφαρμογών. Χώρος αποθήκευσης πινάκων είναι συνήθως σημαντικά χαμηλότερη στο κόστος από παραδοσιακά SQL για παρόμοια όγκους δεδομένων.

Μπορείτε να χρησιμοποιήσετε το χώρο αποθήκευσης πινάκων για την αποθήκευση ευέλικτη σύνολα δεδομένων, όπως τα δεδομένα των χρηστών για τις εφαρμογές web, βιβλία διευθύνσεων, πληροφορίες για τη συσκευή και οποιονδήποτε άλλο τύπο μετα-δεδομένων που απαιτεί την υπηρεσία. Μπορείτε να αποθηκεύσετε οποιονδήποτε αριθμό οντοτήτων σε έναν πίνακα και ένα λογαριασμό του χώρου αποθήκευσης ενδέχεται να περιέχουν οποιονδήποτε αριθμό πίνακες, μέχρι το όριο δυναμικότητας του λογαριασμού χώρου αποθήκευσης.

### <a name="about-this-tutorial"></a>Σχετικά με αυτό το πρόγραμμα εκμάθησης

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς να συντάξετε κώδικα .NET για ορισμένα σενάρια κοινή χρήση πίνακα Azure αποθήκευσης, όπως τη δημιουργία και τη διαγραφή ενός πίνακα και εισαγωγή, ενημέρωση, διαγραφή και υποβολή ερωτημάτων στα δεδομένα του πίνακα.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 45 λεπτά

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Βιβλιοθήκη προγράμματος-πελάτη Azure χώρου αποθήκευσης για .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Διαχείριση ομάδας παραμέτρων για .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Λογαριασμός Azure χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Περισσότερα δείγματα

Για περισσότερα παραδείγματα χρήσης χώρος αποθήκευσης πινάκων, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure στο .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Μπορείτε να κάνετε λήψη του δείγματος εφαρμογής και εκτελέστε το ή αναζητήστε τον κωδικό στη GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Προσθήκη δηλώσεις χώρου ονομάτων

Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του `program.cs` αρχείο:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Ανάλυση τη συμβολοσειρά σύνδεσης

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Δημιουργήστε το πρόγραμμα-πελάτη υπηρεσίας πίνακα

Η κλάση **CloudTableClient** σάς επιτρέπει να ανακτήσετε πίνακες και οντοτήτων που είναι αποθηκευμένα στο χώρο αποθήκευσης πινάκων. Ακολουθεί ένας τρόπος για να δημιουργήσετε το πρόγραμμα-πελάτη υπηρεσίας:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Τώρα είστε έτοιμοι να γράψετε κώδικα που διαβάζει δεδομένα από και καταγράφει δεδομένα χώρος αποθήκευσης πινάκων.

## <a name="create-a-table"></a>Δημιουργία πίνακα

Αυτό το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε έναν πίνακα, εάν δεν υπάρχει ήδη:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα

Αντιστοίχιση οντοτήτων με C\# αντικείμενα, χρησιμοποιώντας μια προσαρμοσμένη κλάση που προέρχονται από **TableEntity**. Για να προσθέσετε μια οντότητα σε έναν πίνακα, δημιουργήστε μια κλάσης που ορίζει τις ιδιότητες του σας οντότητα. Ο ακόλουθος κώδικας καθορίζει μια κλάση οντότητα που χρησιμοποιεί το όνομα του πελάτη ως το κλειδί γραμμής και το επώνυμο ως το κλειδί διαμερίσματα. Μαζί, μια οντότητα διαμερίσματα και κλειδί γραμμή προσδιορίζει η οντότητα στον πίνακα. Οντοτήτων με το ίδιο κλειδί διαμερίσματα μπορούν να αναζητηθούν ταχύτερα από αυτές με αριθμούς-κλειδιά διαμερισμάτων διαφορετικά, αλλά με τη χρήση διάφορων τύπων partition κλειδιών επιτρέπει μεγαλύτερη δυνατότητα κλιμάκωσης παράλληλες λειτουργιών.  Για οποιαδήποτε ιδιότητα που θα πρέπει να είναι αποθηκευμένα στην υπηρεσία πίνακα, η ιδιότητα πρέπει να είναι μια δημόσια ιδιότητα υποστηριζόμενο τύπο που εκθέτει και τα δύο `get` και `set`.
Επίσης, τον τύπο οντότητα *πρέπει να* εμφανίζουν μια κατασκευή χωρίς παραμέτρους.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Λειτουργίες πίνακα που αφορούν οντοτήτων εκτελούνται μέσω του αντικειμένου **CloudTable** που δημιουργήσατε προηγουμένως στην ενότητα "Δημιουργία πίνακα". Η λειτουργία να εκτελεστεί αντιπροσωπεύεται από ένα αντικείμενο **TableOperation** .  Το ακόλουθο παράδειγμα κώδικα δείχνει τη δημιουργία του αντικειμένου **CloudTable** και, στη συνέχεια, ένα αντικείμενο **CustomerEntity** .  Για να προετοιμάσετε τη λειτουργία, δημιουργείται ένα αντικείμενο **TableOperation** για να εισαγάγετε την οντότητα του πελάτη στον πίνακα.  Τέλος, εκτελείται η λειτουργία καλώντας **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Εισαγάγετε μια δέσμη οντοτήτων

Μπορείτε να εισαγάγετε μια δέσμη οντοτήτων σε έναν πίνακα σε μια λειτουργία εγγραφής. Ορισμένες άλλες σημειώσεις στη δέσμη λειτουργίες:

-  Μπορείτε να εκτελέσετε ενημερώσεις, διαγράφει και εισάγει με την ίδια λειτουργία μαζικά.
-  Μια λειτουργία μαζικά μπορούν να περιλαμβάνουν έως 100 οντοτήτων.
-  Όλα οντοτήτων σε μια λειτουργία μαζικά πρέπει να έχει το ίδιο κλειδί διαμερίσματα.
-  Ενώ είναι δυνατή η εκτέλεση ενός ερωτήματος ως λειτουργία δέσμης, πρέπει να είναι η μόνη λειτουργία στη δέσμη.

<!-- -->
Το ακόλουθο παράδειγμα κώδικα δημιουργεί δύο αντικείμενα οντότητα και προσθέτει το καθένα σε **TableBatchOperation** , χρησιμοποιώντας τη μέθοδο **Εισαγωγή** . Στη συνέχεια, **CloudTable.Execute** ονομάζεται για την εκτέλεση της λειτουργίας.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Ανάκτηση όλα οντοτήτων ένα διαμερίσματα

Ερώτημα για έναν πίνακα για όλους οντοτήτων ένα διαμερίσματα, χρησιμοποιήστε ένα αντικείμενο **TableQuery** .
Το ακόλουθο παράδειγμα κώδικα καθορίζει ένα φίλτρο για οντοτήτων όπου 'Smith' είναι το πλήκτρο διαμερίσματα. Αυτό το παράδειγμα εκτυπώνει τα πεδία από κάθε οντότητα στα αποτελέσματα του ερωτήματος στην κονσόλα.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Ανάκτηση μιας περιοχής των οντοτήτων ένα διαμερίσματα

Εάν δεν θέλετε ερώτημα για όλα τα οντοτήτων ένα διαμερίσματα, μπορείτε να καθορίσετε μια περιοχή συνδυάζοντας το φίλτρο κλειδιού διαμερίσματα με φίλτρο βασικοί γραμμής. Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί δύο φίλτρα για όλους οντοτήτων σε διαμερίσματα 'Smith' όπου το κλειδί γραμμής (όνομα) ξεκινά νωρίτερα από "E" στο αλφάβητο με γράμμα και, στη συνέχεια, να εκτυπώνει τα αποτελέσματα του ερωτήματος.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Ανάκτηση μία οντότητα

Μπορείτε να συντάξετε ένα ερώτημα για να ανακτήσετε μια συγκεκριμένη οντότητα. Ο ακόλουθος κώδικας χρησιμοποιεί **TableOperation** για να καθορίσετε τον πελάτη 'Βασίλη Smith'.
Αυτή η μέθοδος επιστρέφει μόνο μία οντότητα αντί για μια συλλογή και την επιστρεφόμενη τιμή σε **TableResult.Result** είναι ένα αντικείμενο **CustomerEntity** .
Καθορισμός κλειδιών διαμερίσματα και γραμμών σε ένα ερώτημα είναι ο πιο γρήγορος τρόπος για να ανακτήσετε μια μοναδική οντότητα από την υπηρεσία του πίνακα.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Αντικατάσταση μιας οντότητας

Για να ενημερώσετε μια οντότητα, να το ανακτήσετε από την υπηρεσία του πίνακα, η τροποποίηση του αντικειμένου οντότητα και, στη συνέχεια, να αποθηκεύσετε τις αλλαγές ξανά με την υπηρεσία του πίνακα. Ο ακόλουθος κώδικας αλλάζει αριθμού τηλεφώνου έναν υπάρχοντα πελάτη. Αντί για κλήση **Εισαγωγή**, αυτός ο κωδικός χρησιμοποιεί **Αντικατάσταση**. Αυτό θα κάνει η οντότητα να αντικατασταθούν πλήρως στο διακομιστή, εκτός εάν η οντότητα στο διακομιστή έχει αλλάξει μετά την ανάκτησή, οπότε η λειτουργία θα αποτύχει.  Αυτό το σφάλμα είναι για να αποτρέψετε την εφαρμογή σας από αντικαταστήσει κατά λάθος μια αλλαγή που έγιναν μεταξύ της ανάκτησης και ενημέρωση από ένα άλλο στοιχείο της εφαρμογής σας.  Ο χειρισμός των αυτό το σφάλμα είναι να ανακτήσετε η οντότητα ξανά, κάντε τις αλλαγές σας (Εάν εξακολουθεί να ισχύει) και, στη συνέχεια, εκτελέστε μια άλλη λειτουργία **αντικατάστασης** .  Στην επόμενη ενότητα θα σας δείξουν πώς μπορείτε να παρακάμψετε αυτήν τη συμπεριφορά.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Εισαγωγή-ή-αντικατάσταση μια οντότητα

**Αντικατάσταση** λειτουργίες θα αποτύχει, εάν η οντότητα έχει αλλάξει μετά την ανάκτησή από το διακομιστή.  Επιπλέον, πρέπει να ανακτήσετε την οντότητα από το διακομιστή πρώτα στη σειρά για τη λειτουργία **αντικατάστασης** να ολοκληρωθεί με επιτυχία.
Ορισμένες φορές, ωστόσο, δεν γνωρίζετε εάν υπάρχει η οντότητα στο διακομιστή και τις τρέχουσες τιμές που είναι αποθηκευμένα σε αυτό είναι σημασία. Ενημέρωση σας θα πρέπει να αντικαθιστούν τις όλες.  Για να γίνει αυτό, μπορείτε να χρησιμοποιήσετε μια λειτουργία **InsertOrReplace** .  Αυτή η λειτουργία εισάγει την οντότητα, αν δεν υπάρχει, ή αντικαθιστά το, εάν υπάρχει, ανεξάρτητα από το πότε έγινε η τελευταία ενημέρωση.  Στο παρακάτω παράδειγμα κώδικα, η οντότητα πελατών για Smith Βασίλη εξακολουθεί να ανάκτηση, αλλά, στη συνέχεια, αποθηκεύεται στο διακομιστή μέσω **InsertOrReplace**.  Οι ενημερώσεις που κάνατε στην οντότητα μεταξύ τις λειτουργίες ανάκτηση και ενημέρωση θα αντικατασταθούν.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Ένα υποσύνολο οντότητα ιδιοτήτων ερωτήματος

Ένα ερώτημα πίνακα να ανακτήσετε μόνο μερικές ιδιότητες από μια οντότητα αντί για όλες τις ιδιότητες οντότητα. Αυτήν την τεχνική, που ονομάζεται προβολή, μειώνει το εύρος ζώνης και να βελτιώσετε τις επιδόσεις του ερωτήματος, ειδικά για μεγάλη οντοτήτων. Το ερώτημα στο ακόλουθο κώδικα επιστρέφει μόνο τις διευθύνσεις ηλεκτρονικού ταχυδρομείου των οντοτήτων στον πίνακα. Αυτό γίνεται με χρήση ενός ερωτήματος **DynamicTableEntity** και επίσης **EntityResolver**. Μπορείτε να μάθετε περισσότερα σχετικά με την προβολή σε την [παρουσίαση Upsert και προβολής ερωτήματος ιστολόγιο δημοσίευση][]. Σημειώστε ότι προβολής δεν υποστηρίζεται σε προσομοίωσης του τοπικού χώρου αποθήκευσης, ώστε να εκτελείται αυτόν τον κωδικό μόνο όταν χρησιμοποιείτε ένα λογαριασμό την υπηρεσία του πίνακα.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Διαγραφή οντότητας

Μπορείτε να διαγράψετε μια οντότητα εύκολα μετά την ανάκτηση, χρησιμοποιώντας το ίδιο μοτίβο που εμφανίζεται για την ενημέρωση μιας οντότητας.  Ο ακόλουθος κώδικας ανακτά και διαγράφει ένα οντότητα του πελάτη.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Διαγραφή ενός πίνακα

Τέλος, το ακόλουθο παράδειγμα κώδικα Διαγράφει έναν πίνακα από ένα λογαριασμό του χώρου αποθήκευσης. Θα είναι διαθέσιμη για να δημιουργηθεί εκ νέου για κάποιο χρονικό διάστημα μετά τη διαγραφή ενός πίνακα που έχει διαγραφεί.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Ασύγχρονη ανάκτηση οντοτήτων σε σελίδες

Αν διαβάζετε μεγάλου αριθμού οντοτήτων και θέλετε να διαδικασία/εμφάνισης οντοτήτων όπως ανάκτησή τους και όχι σε αναμονή για αυτές όλων για να επιστρέψετε, μπορείτε να ανακτήσετε οντοτήτων με χρήση ενός ερωτήματος τμηματική. Αυτό το παράδειγμα δείχνει πώς μπορείτε να επιστρέφει αποτελέσματα σε σελίδες, χρησιμοποιώντας το μοτίβο αναμένει ασύγχρονης ώστε να εκτέλεσης δεν αποκλείονται ενώ περιμένετε για ένα μεγάλο σύνολο αποτελεσμάτων για να επιστρέψετε. Για περισσότερες λεπτομέρειες σχετικά με τη χρήση του μοτίβου ασύγχρονης Await στο .NET, ανατρέξτε στο θέμα [ασύγχρονης προγραμματισμού με ασύγχρονης και Await (C# και Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του πίνακα χώρου αποθήκευσης, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης:

- Ανατρέξτε στο θέμα περισσότερα δείγματα αποθήκευσης πίνακα [γρήγορα](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure στο .NET
- Προβάλετε την τεκμηρίωση πίνακα υπηρεσιών αναφοράς για πλήρεις λεπτομέρειες σχετικά με την διαθέσιμων API:
    - [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Αναφορά REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Μάθετε πώς μπορείτε να απλοποιήσετε τον κώδικα που συντάσσετε ώστε να λειτουργεί με το χώρο αποθήκευσης Azure, χρησιμοποιώντας το [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Προβολή περισσότερων δυνατοτήτων οδηγοί για να μάθετε σχετικά με τις πρόσθετες επιλογές για την αποθήκευση δεδομένων στο Azure.
    - [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md) για να αποθηκεύσετε μη δομημένα δεδομένα.
    - [Σύνδεση με βάση δεδομένων SQL με χρήση του .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) για την αποθήκευση σχεσιακών δεδομένων.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Δημοσίευση ιστολογίου θέσπιση Upsert και προβολής ερωτήματος]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage

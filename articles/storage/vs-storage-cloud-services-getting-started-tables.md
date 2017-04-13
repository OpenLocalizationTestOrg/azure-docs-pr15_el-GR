<properties
    pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων και του Visual Studio συνδεδεμένες υπηρεσίες (υπηρεσίες cloud) | Microsoft Azure"
    description="Πώς να γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure σε ένα έργο υπηρεσία cloud στο Visual Studio μετά τη σύνδεση με ένα λογαριασμό χώρου αποθήκευσης χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
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

# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure και του Visual Studio συνδεδεμένες υπηρεσίες (cloud services έργα)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

##<a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να ξεκινήσετε με χώρο αποθήκευσης πινάκων του Azure στο Visual Studio, αφού έχετε δημιουργήσει ή στα οποία γίνεται αναφορά λογαριασμού Azure χώρου αποθήκευσης στο cloud services έργου χρησιμοποιώντας το παράθυρο διαλόγου του Visual Studio **Προσθέσετε συνδεδεμένες υπηρεσίες** . Η λειτουργία **Προσθέσετε συνδεδεμένες υπηρεσίες** εγκαθιστά τα κατάλληλα πακέτα NuGet για να αποκτήσετε πρόσβαση Azure χώρου αποθήκευσης στο έργο σας και προσθέτει τη συμβολοσειρά σύνδεσης για το λογαριασμό χώρου αποθήκευσης σε αρχεία ρύθμισης παραμέτρων του έργου σας.

Η υπηρεσία αποθήκευσης πίνακα Azure σάς επιτρέπει να αποθηκεύσετε μεγάλες ποσότητες δομημένα δεδομένα. Η υπηρεσία είναι μια αποθήκευσης δεδομένων NoSQL που δέχεται κλήσεις με έλεγχο ταυτότητας από εντός και εκτός του Azure cloud. Azure πίνακες είναι ιδανικά για την αποθήκευση δομημένες, μη σχεσιακών δεδομένων.

Για να ξεκινήσετε, πρέπει πρώτα να δημιουργήσετε έναν πίνακα στο λογαριασμό σας στο χώρο αποθήκευσης. Θα σας δείξουμε πώς μπορείτε να δημιουργήσετε έναν πίνακα του Azure στον κώδικα, καθώς και πώς να εκτελείτε βασικές πίνακα και οντότητα διαδικασιών, όπως η προσθήκη, τροποποίηση, ανάγνωση και ανάγνωση οντοτήτων πίνακα. Τα δείγματα που είναι γραμμένες σε C\# κώδικα και να χρησιμοποιήσετε τη [βιβλιοθήκη προγράμματος-πελάτη Microsoft Azure αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**ΣΗΜΕΊΩΣΗ:** Ορισμένα από τα API που εκτελούν κλήσεις εκτός Azure χώρου αποθήκευσης είναι ασύγχρονης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ασύγχρονη προγραμματισμού με ασύγχρονης και Await](http://msdn.microsoft.com/library/hh191443.aspx) . Ο παρακάτω κώδικας προϋποθέτει ασύγχρονης μεθόδους προγραμματισμού που χρησιμοποιούνται.

- Για περισσότερες πληροφορίες σχετικά με προγραμματισμό Διαχείριση πινάκων, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET](storage-dotnet-how-to-use-tables.md) .
- Ανατρέξτε [στην τεκμηρίωση χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/) για γενικές πληροφορίες σχετικά με το χώρο αποθήκευσης Azure.
- Για γενικές πληροφορίες σχετικά με τις υπηρεσίες Azure cloud, ανατρέξτε στο θέμα [τεκμηρίωση των υπηρεσιών Cloud](https://azure.microsoft.com/documentation/services/cloud-services/) .
- Για περισσότερες πληροφορίες σχετικά με τον προγραμματισμό εφαρμογών ASP.NET, ανατρέξτε στο θέμα [ASP.NET](http://www.asp.net) .

## <a name="access-tables-in-code"></a>Πίνακες της Access σε κώδικα

Για να αποκτήσετε πρόσβαση σε πίνακες σε έργα υπηρεσία cloud, πρέπει να περιλαμβάνουν τα εξής στοιχεία σε οποιαδήποτε C# αρχεία προέλευσης που πρόσβαση στο χώρο αποθήκευσης πινάκων του Azure.

1. Βεβαιωθείτε ότι οι δηλώσεις χώρου ονομάτων στο επάνω μέρος του αρχείου C# περιλαμβάνει αυτές τις προτάσεις που **χρησιμοποιείτε** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Λάβετε ένα αντικείμενο **CloudStorageAccount** που αντιπροσωπεύει τις πληροφορίες λογαριασμού χώρου αποθήκευσης. Χρησιμοποιήστε τον παρακάτω κώδικα για να λάβετε τη συμβολοσειρά σύνδεσης του χώρου αποθήκευσης και των πληροφοριών λογαριασμού χώρου αποθήκευσης από τη ρύθμιση παραμέτρων της υπηρεσίας Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
> [AZURE.NOTE]  Χρησιμοποιήστε όλο τον παραπάνω κώδικα μπροστά από τον κώδικα στον στα παρακάτω παραδείγματα.

3. Λάβετε ένα αντικείμενο **CloudTableClient** για αναφορά τα αντικείμενα πίνακα στο λογαριασμό σας στο χώρο αποθήκευσης.

         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Λάβετε ένα αντικείμενο αναφοράς **CloudTable** για να αναφέρονται σε ένα συγκεκριμένο πίνακα και οντοτήτων.

        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Δημιουργία πίνακα σε κώδικα

Για να δημιουργήσετε τον πίνακα Azure, απλώς προσθέστε μια κλήση σε **CreateIfNotExistsAsync** για να το μετά τη λήψη ενός αντικειμένου **CloudTable** όπως περιγράφεται στην ενότητα "Πρόσβαση από πίνακες στο κώδικα".

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Προσθέστε μια οντότητα σε έναν πίνακα

Για να προσθέσετε μια οντότητα σε έναν πίνακα, δημιουργήστε μιας κλάσης που ορίζει τις ιδιότητες του σας οντότητα. Ο ακόλουθος κώδικας καθορίζει μια κλάση οντότητα που ονομάζεται **CustomerEntity** που χρησιμοποιεί το όνομα του πελάτη ως το κλειδί γραμμής και το επώνυμο ως το κλειδί διαμερίσματα.

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

Λειτουργίες πίνακα που αφορούν οντοτήτων γίνονται χρησιμοποιώντας το αντικείμενο **CloudTable** που δημιουργήσατε νωρίτερα σε "πρόσβαση από πίνακες στο κώδικα." Το αντικείμενο **TableOperation** αντιπροσωπεύει τη λειτουργία πρέπει να πραγματοποιηθεί. Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να δημιουργήσετε ένα αντικείμενο **CloudTable** και ένα αντικείμενο **CustomerEntity** . Για να προετοιμάσετε τη λειτουργία, δημιουργείται ένα **TableOperation** για να εισαγάγετε την οντότητα του πελάτη στον πίνακα. Τέλος, εκτελείται η λειτουργία καλώντας **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Εισαγάγετε μια δέσμη οντοτήτων

Μπορείτε να εισαγάγετε πολλές οντοτήτων σε έναν πίνακα σε μια λειτουργία μόνο εγγραφής. Το ακόλουθο παράδειγμα κώδικα δημιουργεί δύο αντικείμενα οντότητα ("Jeff κοίτα" και "Κοίτα Βασίλη"), τα οποία προσθέτει ένα αντικείμενο **TableBatchOperation** χρησιμοποιώντας τη μέθοδο εισαγωγής και, στη συνέχεια, να ξεκινήσει η λειτουργία καλώντας **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Λήψη όλων των οντοτήτων στο ένα διαμερίσματα

Ερώτημα για έναν πίνακα για όλων των οντοτήτων ένα διαμερίσματα, χρησιμοποιήστε ένα αντικείμενο **TableQuery** . Το ακόλουθο παράδειγμα κώδικα καθορίζει ένα φίλτρο για οντοτήτων όπου 'Smith' είναι το πλήκτρο διαμερίσματα. Αυτό το παράδειγμα εκτυπώνει τα πεδία από κάθε οντότητα στα αποτελέσματα του ερωτήματος στην κονσόλα.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## <a name="get-a-single-entity"></a>Λάβετε μια μοναδική οντότητα

Μπορείτε να συντάξετε ένα ερώτημα για να λάβετε μια συγκεκριμένη οντότητα. Ο ακόλουθος κώδικας χρησιμοποιεί ένα αντικείμενο **TableOperation** για να καθορίσετε έναν πελάτη που ονομάζεται 'Βασίλη Smith'. Αυτή η μέθοδος επιστρέφει μόνο μία οντότητα, αντί για μια συλλογή και την τιμή που επιστρέφεται στο **TableResult.Result** είναι ένα αντικείμενο **CustomerEntity** . Καθορισμός κλειδιών διαμερίσματα και γραμμών σε ένα ερώτημα είναι ο πιο γρήγορος τρόπος για να ανακτήσετε μια μοναδική οντότητα από την υπηρεσία του **πίνακα** .

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Διαγραφή οντότητας
Μπορείτε να διαγράψετε μια οντότητα μόλις το βρείτε. Ο ακόλουθος κώδικας αναζητά μια οντότητα πελάτη με το όνομα "Βασίλη κοίτα" και, εάν το εντοπίσει, διαγράφει το.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

<properties
    pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης ουρά Azure μέσω του .NET | Microsoft Azure"
    description="Azure ουρές παρέχουν αξιόπιστη, ασύγχρονης μηνυμάτων μεταξύ των στοιχείων της εφαρμογής. Στο cloud ανταλλαγής μηνυμάτων ενεργοποιεί την εφαρμογή στοιχεία για να κλιμακωθεί ανεξάρτητα."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Γρήγορα αποτελέσματα με το χώρο αποθήκευσης ουρά Azure μέσω .NET

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση

Χώρος αποθήκευσης Azure ουρά παρέχει cloud μηνυμάτων μεταξύ των στοιχείων της εφαρμογής. Κατά τη σχεδίαση εφαρμογών για την κλίμακα, στοιχεία εφαρμογής συχνά είναι αποσυνδεδεμένη, έτσι ώστε να κλιμακωθεί ανεξάρτητα. Αποθήκευση ουράς προσφέρει ασύγχρονης ανταλλαγής μηνυμάτων για την επικοινωνία μεταξύ των στοιχείων της εφαρμογής, εάν εκτελούνται στο cloud, στην επιφάνεια εργασίας, σε ένα διακομιστή εσωτερικής εγκατάστασης ή σε μια κινητή συσκευή. Αποθήκευση ουρά υποστηρίζει επίσης τη Διαχείριση εργασιών ασύγχρονης και τη δημιουργία ροών εργασίας διαδικασία.

### <a name="about-this-tutorial"></a>Σχετικά με αυτό το πρόγραμμα εκμάθησης

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς να συντάξετε κώδικα .NET για ορισμένα σενάρια κοινή χρήση του χώρου αποθήκευσης ουρά Azure. Σενάρια που καλύπτονται περιλαμβάνουν τη δημιουργία και διαγραφή ουρές και προσθήκη ανάγνωσης και τη διαγραφή μηνυμάτων ουράς.

**Εκτιμώμενη χρόνο για να ολοκληρωθεί:** 45 λεπτά

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Βιβλιοθήκη προγράμματος-πελάτη Azure χώρου αποθήκευσης για .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Διαχείριση ομάδας παραμέτρων για .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Λογαριασμός Azure χώρου αποθήκευσης](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Προσθήκη δηλώσεις χώρου ονομάτων

Προσθέστε τα ακόλουθα `using` δηλώσεις στο επάνω μέρος του `program.cs` αρχείο:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Ανάλυση τη συμβολοσειρά σύνδεσης

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Δημιουργήστε το πρόγραμμα-πελάτη υπηρεσίας Ουράς

Η κλάση **CloudQueueClient** σάς επιτρέπει να ανακτήσετε ουρές που έχουν αποθηκευτεί στο χώρο αποθήκευσης ουρά. Ακολουθεί ένας τρόπος για να δημιουργήσετε το πρόγραμμα-πελάτη υπηρεσίας:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Τώρα είστε έτοιμοι να γράψετε κώδικα που διαβάζει δεδομένα από και καταγράφει δεδομένα με το χώρο αποθήκευσης ουρά.

## <a name="create-a-queue"></a>Δημιουργία ουράς

Αυτό το παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια ουρά, εάν δεν υπάρχει ήδη:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Εισαγάγετε ένα μήνυμα σε μια ουρά

Για να εισαγάγετε ένα μήνυμα σε μια ήδη υπάρχουσα ουρά, πρέπει πρώτα να δημιουργήσετε μια νέα **CloudQueueMessage**. Στη συνέχεια, καλέστε τη μέθοδο **AddMessage** . Μια **CloudQueueMessage** μπορεί να δημιουργηθεί από μια συμβολοσειρά (σε μορφή UTF-8) ή έναν πίνακα **byte** . Ακολουθεί κωδικό ο οποίος δημιουργεί μια ουρά (Εάν δεν υπάρχει) και την εισάγει το μήνυμα 'Καλώς όρισες κόσμο':

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Ρίξτε μια ματιά σε στο επόμενο μήνυμα

Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας τη μέθοδο **PeekMessage** .

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Αλλαγή των περιεχομένων ένα μήνυμα σε ουρά

Μπορείτε να αλλάξετε τα περιεχόμενα ενός μηνύματος στην ίδια θέση στην ουρά. Εάν το μήνυμα αντιπροσωπεύει μια εργασία, μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα να ενημερώσετε την κατάσταση της εργασίας εργασίας. Ο ακόλουθος κώδικας ενημερώνει το μήνυμα ουρά με νέο περιεχόμενο και ορίζει το χρονικό όριο αναγνωσιμότητα για να επεκτείνετε μια άλλη 60 δευτερόλεπτα. Αυτό αποθηκεύει την κατάσταση της εργασίας που σχετίζεται με το μήνυμα και παρέχει το πρόγραμμα-πελάτη άλλο λεπτά για να συνεχίσετε να εργάζεστε με το μήνυμα. Μπορείτε να χρησιμοποιήσετε αυτήν την τεχνική για την παρακολούθηση πολλών βήμα ροές εργασιών στην ουρά μηνυμάτων, χωρίς να χρειάζεται να ξεκινήσετε από την αρχή, εάν ένα βήμα επεξεργασίας αποτυγχάνει λόγω αποτυχία υλικού ή λογισμικού. Συνήθως, θα κρατήσετε καθώς και μια καταμέτρηση "Επανάληψη" και, εάν το μήνυμα έχει επαναληφθεί περισσότερες από *n* φορές, μπορείτε να το διαγράψετε. Αυτό προστατεύει από ένα μήνυμα που ενεργοποιεί ένα σφάλμα στην εφαρμογή κάθε φορά που υποβάλλεται σε επεξεργασία.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Καταργήστε την ουρά του επόμενου μηνύματος

Καταργήστε την τον κωδικό ουρές σε ένα μήνυμα από μια ουρά σε δύο βήματα. Όταν καλείτε **GetMessage**, λαμβάνετε στο επόμενο μήνυμα σε μια ουρά. Ένα μήνυμα που επιστρέφονται από **GetMessage** δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Από προεπιλογή, αυτό το μήνυμα παραμένει αόρατο για 30 δευτερόλεπτα. Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, μπορείτε, επίσης, πρέπει να καλέσετε **DeleteMessage**. Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι εάν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Τον κωδικό κλήσεις **DeleteMessage** δεξιά μετά το μήνυμα έχει υποστεί επεξεργασία.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Χρήση αναμένει ασύγχρονης μοτίβου με το κοινό χώρο αποθήκευσης ουρά APIs

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε το μοτίβο αναμένει ασύγχρονης με κοινό χώρο αποθήκευσης ουρά APIs. Το δείγμα κλήσεις της ασύγχρονης έκδοσης του κάθε μία από τις μεθόδους που δίνεται, όπως υποδεικνύεται από το επίθημα *ασύγχρονης* κάθε μέθοδο. Πότε χρησιμοποιείται ένα ασύγχρονη μέθοδο, το ασύγχρονης-αναμένει μοτίβο διακόπτει προσωρινά την τοπική εκτέλεση μέχρι να ολοκληρωθεί η κλήση. Αυτή η συμπεριφορά επιτρέπει το τρέχον νήμα να κάνετε άλλες εργασίες, το οποίο σας βοηθά να αποφύγετε συνωστισμών επιδόσεων και βελτιώνει την συνολική ανταπόκριση της εφαρμογής σας. Για περισσότερες λεπτομέρειες σχετικά με τη χρήση του μοτίβου ασύγχρονης Await στο .NET ανατρέξτε [ασύγχρονης και Await (C# και Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Αξιοποιήσετε πρόσθετες επιλογές, καταργήστε την υπηρεσία Ουράς μηνυμάτων

Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά.
Πρώτα, μπορείτε να λάβετε μια δέσμη μηνυμάτων (έως 32). Δεύτερον, μπορείτε να ορίσετε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη, επιτρέποντάς σας κώδικα περισσότερες ή λιγότερο χρόνο για την επεξεργασία πλήρως κάθε μήνυμα. Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί τη μέθοδο **GetMessages** για 20 μηνύματα σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μήνυμα χρησιμοποιώντας ένα βρόχο **foreach** . Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά για κάθε μήνυμα. Σημειώστε ότι το ξεκινά 5 λεπτά για όλα τα μηνύματα την ίδια στιγμή, επομένως, αφού έχετε 5 λεπτά περάσει από την κλήση σε **GetMessages**, τυχόν μηνύματα που δεν έχουν διαγραφεί θα γίνει ορατό ξανά.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Λάβετε το μήκος ουράς

Μπορείτε να λάβετε μια εκτίμηση της τον αριθμό των μηνυμάτων σε μια ουρά. Η μέθοδος **FetchAttributes** ζητά από την υπηρεσία ουράς για να ανακτήσετε τα χαρακτηριστικά ουρά, όπως το πλήθος μήνυμα. Η ιδιότητα **ApproximateMessageCount** επιστρέφει την τελευταία τιμή που έχει ανακτηθεί με τη μέθοδο **FetchAttributes** , χωρίς να καλούν την υπηρεσία ουράς.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Διαγραφή μιας ουράς

Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, κλήση της μεθόδου **Διαγράψτε** το αντικείμενο ουράς.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης ουρά, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

- Ανατρέξτε στην τεκμηρίωση αναφοράς υπηρεσίας ουράς για πλήρεις λεπτομέρειες σχετικά με την διαθέσιμων API:
    - [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Αναφορά REST API](http://msdn.microsoft.com/library/azure/dd179355)
- Μάθετε πώς μπορείτε να απλοποιήσετε τον κώδικα που συντάσσετε ώστε να λειτουργεί με το χώρο αποθήκευσης Azure, χρησιμοποιώντας το [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).
- Προβολή περισσότερων δυνατοτήτων οδηγοί για να μάθετε σχετικά με τις πρόσθετες επιλογές για την αποθήκευση δεδομένων στο Azure.
    - [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης πινάκων του Azure μέσω .NET](storage-dotnet-how-to-use-tables.md) για την αποθήκευση δομημένα δεδομένα.
    - [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob του Azure χρησιμοποιώντας .NET](storage-dotnet-how-to-use-blobs.md) για να αποθηκεύσετε μη δομημένα δεδομένα.
    - [Σύνδεση με βάση δεδομένων SQL με χρήση του .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) για την αποθήκευση σχεσιακών δεδομένων.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2

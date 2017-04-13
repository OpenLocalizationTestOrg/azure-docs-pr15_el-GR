<properties
    pageTitle="Γρήγορα αποτελέσματα με το χώρο αποθήκευσης ουρά και του Visual Studio συνδεδεμένες υπηρεσίες (ASP.NET) | Microsoft Azure"
    description="Πώς μπορείτε να ξεκινήσετε με χρήση του χώρου αποθήκευσης Azure ουρά σε ένα έργο ASP.NET στο Visual Studio μετά τη σύνδεση με ένα λογαριασμό χώρου αποθήκευσης χρησιμοποιώντας το Visual Studio συνδεδεμένες υπηρεσίες"
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
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Γρήγορα αποτελέσματα με το Azure ουρά χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση

Αυτό το άρθρο περιγράφει πώς να ξεκινήσετε να χρησιμοποιείτε ουρά Azure χώρου αποθήκευσης στο Visual Studio, αφού έχετε δημιουργήσει ή στα οποία γίνεται αναφορά λογαριασμού Azure χώρου αποθήκευσης σε ένα έργο ASP.NET χρησιμοποιώντας το παράθυρο διαλόγου του Visual Studio **Προσθέσετε συνδεδεμένες υπηρεσίες** .

Θα σας δείξουμε πώς μπορείτε να δημιουργήσετε και να αποκτήσετε πρόσβαση σε μια ουρά Azure στο λογαριασμό σας στο χώρο αποθήκευσης. Θα επίσης σας δείξουμε πώς να εκτελείτε βασικές ουρά διαδικασιών, όπως η προσθήκη, τροποποίηση, ανάγνωσης και κατάργηση ουρά μηνυμάτων. Τα δείγματα είναι γραμμένες σε κώδικα C# και χρησιμοποιήστε τη [Βιβλιοθήκη προγράμματος-πελάτη του Microsoft Azure χώρου αποθήκευσης για .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Για περισσότερες πληροφορίες σχετικά με το ASP.NET, ανατρέξτε στο θέμα [ASP.NET](http://www.asp.net).

Ο χώρος αποθήκευσης Azure ουρά είναι μια υπηρεσία για την αποθήκευση μεγάλου αριθμού των μηνυμάτων που μπορεί να είναι προσβάσιμα από οπουδήποτε στον κόσμο μέσω με έλεγχο ταυτότητας κλήσεων με HTTP ή HTTPS. Ένα μήνυμα μόνο ουρά μπορεί να είναι έως 64 KB μέγεθος και μια ουρά μπορούν να περιέχουν εκατομμύρια μηνυμάτων, μέχρι το όριο συνολική χωρητικότητα ενός λογαριασμού χώρου αποθήκευσης.

## <a name="access-queues-in-code"></a>Ουρές πρόσβαση στον κώδικα

Για να αποκτήσετε πρόσβαση σε ουρές ASP.NET έργα, πρέπει να περιλαμβάνουν τα ακόλουθα στοιχεία σε οποιοδήποτε αρχείο προέλευσης C# που πρόσβαση ουρά Azure χώρου αποθήκευσης.

1. Βεβαιωθείτε ότι οι δηλώσεις χώρου ονομάτων στο επάνω μέρος του αρχείου C# περιλαμβάνει αυτές τις προτάσεις που **χρησιμοποιείτε** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Λάβετε ένα αντικείμενο **CloudStorageAccount** που αντιπροσωπεύει τις πληροφορίες λογαριασμού χώρου αποθήκευσης. Χρησιμοποιήστε τον ακόλουθο κώδικα για να λάβετε το τις συμβολοσειρά σύνδεσης χώρου αποθήκευσης και τις πληροφορίες λογαριασμού χώρου αποθήκευσης από τη ρύθμιση παραμέτρων της υπηρεσίας Azure.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Λάβετε ένα αντικείμενο **CloudQueueClient** αναφοράς στα αντικείμενα ουράς στο λογαριασμό σας στο χώρο αποθήκευσης.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Λάβετε ένα αντικείμενο **CloudQueue** για να αναφέρονται σε μια συγκεκριμένη ουρά.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**ΣΗΜΕΊΩΣΗ** Χρησιμοποιήστε όλο τον παραπάνω κώδικα μπροστά από τον κώδικα στον στα παρακάτω παραδείγματα.

## <a name="create-a-queue-in-code"></a>Δημιουργία ουράς στον κώδικα

Για να δημιουργήσετε μια Azure ουρά στον κώδικα, απλώς προσθέστε μια κλήση για να **CreateIfNotExists** στον κώδικα παραπάνω.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Προσθέστε ένα μήνυμα σε ουρά

Για να εισαγάγετε ένα μήνυμα σε μια ήδη υπάρχουσα ουρά, δημιουργήστε ένα αντικείμενο **CloudQueueMessage** και, στη συνέχεια, η κλήση της μεθόδου **AddMessage** .

Ένα αντικείμενο **CloudQueueMessage** μπορούν να δημιουργηθούν από μια συμβολοσειρά (σε μορφή UTF-8) ή από έναν πίνακα byte.

Ακολουθεί ένα παράδειγμα το οποίο εισάγει το μήνυμα 'Καλώς όρισες κόσμο'.

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Ανάγνωση ενός μηνύματος σε μια ουρά

Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας τη μέθοδο PeekMessage().

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Ανάγνωση και να καταργήσετε ένα μήνυμα σε μια ουρά

Να καταργήσετε τον κωδικό (καταργήστε την ουρά) ένα μήνυμα από μια ουρά σε δύο βήματα.
1. Κλήση GetMessage() για να μεταβείτε στο επόμενο μήνυμα σε μια ουρά. Ένα μήνυμα που επιστρέφονται από GetMessage() δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Από προεπιλογή, αυτό το μήνυμα παραμένει αόρατο για 30 δευτερόλεπτα.
2.  Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, καλέστε **DeleteMessage**.

Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι εάν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Ο ακόλουθος κώδικας κλήσεις **DeleteMessage** δεξιά μετά το μήνυμα έχει υποστεί επεξεργασία.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Χρήση πρόσθετες επιλογές, καταργήστε την υπηρεσία Ουράς μηνυμάτων

Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά.
Πρώτα, μπορείτε να λάβετε μια δέσμη μηνυμάτων (έως 32). Δεύτερον, μπορείτε να ορίσετε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη, επιτρέποντάς σας κώδικα περισσότερες ή λιγότερο χρόνο για την επεξεργασία πλήρως κάθε μήνυμα. Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί τη μέθοδο **GetMessages** για 20 μηνύματα σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μήνυμα χρησιμοποιώντας ένα βρόχο **foreach** . Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά για κάθε μήνυμα. Σημειώστε ότι το ξεκινά 5 λεπτά για όλα τα μηνύματα την ίδια στιγμή, επομένως, αφού έχετε 5 λεπτά περάσει από την κλήση σε **GetMessages**, τυχόν μηνύματα που δεν έχουν διαγραφεί θα γίνει ορατό ξανά.

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

Μπορείτε να λάβετε μια εκτίμηση της τον αριθμό των μηνυμάτων σε μια ουρά. Η μέθοδος **FetchAttributes** ζητά από το queueservice για να ανακτήσετε τα χαρακτηριστικά ουρά, όπως το πλήθος μήνυμα. Η ιδιότητα **ApproximateMethodCount** επιστρέφει την τελευταία τιμή που έχει ανακτηθεί με τη μέθοδο **FetchAttributes** , χωρίς την κλήση του queueservice.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Χρήση αναμένει ασύγχρονης μοτίβου με κοινές queueAPIs

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε το μοτίβο αναμένει ασύγχρονης με κοινές queueAPIs. Το δείγμα καλεί την έκδοση ασύγχρονης κάθε μία από τις μεθόδους που δίνεται, αυτό μπορεί να είναι ορατό από την επιδιόρθωση μετά τη ασύγχρονης κάθε μεθόδου. Πότε χρησιμοποιείται ένα ασύγχρονη μέθοδο το ασύγχρονης-αναμένει μοτίβο διακόπτει προσωρινά την τοπική εκτέλεση μέχρι να ολοκληρωθεί η κλήση. Αυτή η συμπεριφορά επιτρέπει το τρέχον νήμα να κάνετε άλλες εργασίες που σας βοηθά να αποφύγετε συνωστισμών επιδόσεων και βελτιώνει την συνολική ανταπόκριση της εφαρμογής σας. Για περισσότερες λεπτομέρειες σχετικά με τη χρήση του μοτίβου ασύγχρονης Await στο .NET ανατρέξτε στο θέμα [ασύγχρονης και Await (C# και Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Διαγραφή μιας ουράς

Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, κλήση της μεθόδου **Διαγράψτε** το αντικείμενο ουράς.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]
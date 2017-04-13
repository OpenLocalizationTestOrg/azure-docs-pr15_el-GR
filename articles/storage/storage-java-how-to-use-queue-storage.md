<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Java | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς Azure για να δημιουργήσετε και να διαγράψετε ουρές, εισαγωγή, λήψη και να διαγράψετε μηνύματα. Δείγματα γραμμένο σε Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από Java

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός θα σας δείξουν πώς να εκτελείτε συνηθισμένα σενάρια με την υπηρεσία ουράς Azure χώρου αποθήκευσης. Τα δείγματα είναι γραμμένες σε Java και χρησιμοποιήστε το [Azure SDK χώρου αποθήκευσης για Java][]. Τα σενάρια καλύπτεται περιλαμβάνουν ουρά **εισαγωγής**, **παρατήρηση**, **λήψη**και **τη διαγραφή** μηνυμάτων, καθώς και **τη δημιουργία** και **Διαγραφή** ουρές. Για περισσότερες πληροφορίες σχετικά με ουρές, ανατρέξτε στην ενότητα [επόμενα βήματα](#Next-Steps) .

Σημείωση: Μια SDK είναι διαθέσιμη για τους προγραμματιστές που χρησιμοποιούν το χώρο αποθήκευσης Azure σε συσκευές Android. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το [Azure SDK χώρου αποθήκευσης για Android][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Δημιουργία μιας εφαρμογής Java

Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες του χώρου αποθήκευσης που μπορεί να εκτελεστεί μέσα σε μια εφαρμογή Java τοπικά ή σε κώδικα που εκτελείται μέσα σε ένα ρόλο web ή εργαζόμενου ρόλος στο Azure.

Για να το κάνετε αυτό, θα πρέπει να εγκαταστήσετε το Java Development Kit (JDK) και να δημιουργήσετε ένα λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας στο Azure. Αφού το έχετε κάνει, θα πρέπει να βεβαιωθείτε ότι το σύστημα ανάπτυξης πληροί τις ελάχιστες απαιτήσεις και τις εξαρτήσεις, κάτι που παρατίθενται στο αποθετήριο [Azure SDK χώρου αποθήκευσης για Java][] σε GitHub. Εάν το σύστημά σας πληροί αυτές τις απαιτήσεις, μπορείτε να ακολουθήσετε τις οδηγίες για τη λήψη και εγκατάσταση των βιβλιοθηκών Azure χώρου αποθήκευσης για Java στο σύστημά σας από συγκεκριμένο χώρο αποθήκευσης. Αφού ολοκληρώσετε αυτές τις εργασίες, θα μπορείτε να δημιουργήσετε μια εφαρμογή Java που χρησιμοποιεί τα παραδείγματα σε αυτό το άρθρο.

## <a name="configure-your-application-to-access-queue-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να αποκτήσετε πρόσβαση σε ουρά χώρου αποθήκευσης

Προσθέστε τις ακόλουθες δηλώσεις εισαγωγής στο επάνω μέρος του αρχείου Java όπου θέλετε να χρησιμοποιήσετε Azure αποθήκευσης APIs για να αποκτήσετε πρόσβαση ουρές:

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

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

## <a name="how-to-create-a-queue"></a>Πώς μπορείτε να: δημιουργία ουράς

Ένα αντικείμενο **CloudQueueClient** σάς επιτρέπει να λάβετε αναφορά αντικειμένων για ουρές. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **CloudQueueClient** . (Σημείωση: υπάρχουν επιπλέον τρόποι για να δημιουργήσετε αντικείμενα **CloudStorageAccount** ; για περισσότερες πληροφορίες, ανατρέξτε στο θέμα **CloudStorageAccount** στην περιοχή [Αναφοράς SDK Azure αποθήκευσης υπολογιστή-πελάτη].)

Χρησιμοποιήστε το αντικείμενο **CloudQueueClient** για να λάβετε μια αναφορά στην ουρά που θέλετε να χρησιμοποιήσετε. Μπορείτε να δημιουργήσετε ουρά, εάν δεν υπάρχει.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Πώς μπορείτε να: Προσθέστε ένα μήνυμα σε ουρά

Για να εισαγάγετε ένα μήνυμα σε μια ήδη υπάρχουσα ουρά, πρέπει πρώτα να δημιουργήσετε μια νέα **CloudQueueMessage**. Στη συνέχεια, καλέστε τη μέθοδο **addMessage** . Μια **CloudQueueMessage** μπορεί να δημιουργηθεί από μια συμβολοσειρά (σε μορφή UTF-8) ή έναν πίνακα byte. Ακολουθεί κωδικό ο οποίος δημιουργεί μια ουρά (Εάν δεν υπάρχει) και την εισάγει το μήνυμα "Hello, World".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Πώς μπορείτε να: Ρίξτε μια ματιά σε στο επόμενο μήνυμα

Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Πώς μπορείτε να: αλλαγή των περιεχομένων ένα μήνυμα σε ουρά

Μπορείτε να αλλάξετε τα περιεχόμενα ενός μηνύματος στην ίδια θέση στην ουρά. Εάν το μήνυμα αντιπροσωπεύει μια εργασία, μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα να ενημερώσετε την κατάσταση της εργασίας εργασίας. Ο ακόλουθος κώδικας ενημερώνει το μήνυμα ουρά με νέο περιεχόμενο και ορίζει το χρονικό όριο ορατότητα για να επεκτείνετε μια άλλη 60 δευτερόλεπτα. Αυτό αποθηκεύει την κατάσταση της εργασίας που σχετίζεται με το μήνυμα και παρέχει το πρόγραμμα-πελάτη άλλο λεπτά για να συνεχίσετε να εργάζεστε με το μήνυμα. Μπορείτε να χρησιμοποιήσετε αυτήν την τεχνική για την παρακολούθηση πολλών βήμα ροές εργασιών στην ουρά μηνυμάτων, χωρίς να χρειάζεται να ξεκινήσετε από την αρχή, εάν ένα βήμα επεξεργασίας αποτυγχάνει λόγω αποτυχία υλικού ή λογισμικού. Συνήθως, θα κρατήσετε καθώς και μια καταμέτρηση "Επανάληψη" και, εάν το μήνυμα έχει επαναληφθεί περισσότερες από *n* φορές, μπορείτε να το διαγράψετε. Αυτό προστατεύει από ένα μήνυμα που ενεργοποιεί ένα σφάλμα στην εφαρμογή κάθε φορά που υποβάλλεται σε επεξεργασία.

Το παρακάτω αναζητήσεις δείγματα κώδικα μέσω ουρά μηνυμάτων, εντοπίζει το πρώτο μήνυμα που αντιστοιχεί "Hello, World" για το περιεχόμενο, στη συνέχεια, τροποποιεί το μήνυμα περιεχομένου και εξέρχεται από.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Εναλλακτικά, το παρακάτω δείγμα κώδικα ενημερώνει μόνο τα ορατά πρώτου μηνύματος στην ουρά.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Πώς μπορείτε να: λήψη το μήκος ουράς

Μπορείτε να λάβετε μια εκτίμηση της τον αριθμό των μηνυμάτων σε μια ουρά. Η μέθοδος **downloadAttributes** ζητά από την υπηρεσία ουράς για διάφορες τρέχουσες τιμές, καθώς και το πλήθος των τον αριθμό των μηνυμάτων που βρίσκονται σε μια ουρά. Το πλήθος είναι μόνο κατά προσέγγιση, επειδή τα μηνύματα μπορούν να προστεθούν ή να καταργηθούν μετά την υπηρεσία ουράς αποκρίνεται σε την αίτησή σας. Η μέθοδος **getApproximateMessageCount** επιστρέφει την τελευταία τιμή που ανακτήθηκαν από την κλήση σε **downloadAttributes**, χωρίς να καλούν την υπηρεσία ουράς.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Πώς μπορείτε να: απομάκρυνση από την ουρά του επόμενου μηνύματος

Τον κωδικό dequeues ένα μήνυμα από μια ουρά σε δύο βήματα. Όταν καλείτε **retrieveMessage**, λαμβάνετε στο επόμενο μήνυμα σε μια ουρά. Ένα μήνυμα που επιστρέφονται από **retrieveMessage** δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Από προεπιλογή, αυτό το μήνυμα παραμένει αόρατο για 30 δευτερόλεπτα. Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, μπορείτε, επίσης, πρέπει να καλέσετε **deleteMessage**. Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι εάν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Τον κωδικό κλήσεις **deleteMessage** δεξιά μετά το μήνυμα έχει υποστεί επεξεργασία.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Πρόσθετες επιλογές για τα μηνύματα κατάργησης

Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά. Πρώτα, μπορείτε να λάβετε μια δέσμη μηνυμάτων (έως 32). Δεύτερον, μπορείτε να ορίσετε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη, επιτρέποντάς σας κώδικα περισσότερες ή λιγότερο χρόνο για την επεξεργασία πλήρως κάθε μήνυμα.

Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί τη μέθοδο **retrieveMessages** για 20 μηνύματα σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μήνυμα με χρήση ενός βρόχου **για** . Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά (300 δευτερόλεπτα) για κάθε μήνυμα. Σημειώστε ότι τα πέντε λεπτά ξεκινά για όλα τα μηνύματα την ίδια στιγμή, επομένως, όταν πέντε λεπτά έχουν περάσει από την κλήση σε **retrieveMessages**, τυχόν μηνύματα που δεν έχουν διαγραφεί θα γίνει ορατό ξανά.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Πώς μπορείτε να: λίστα τις ουρές

Για να αποκτήσετε μια λίστα με τις τρέχουσες ουρές, καλέστε τη μέθοδο **CloudQueueClient.listQueues()** , η οποία θα επιστρέψει μια συλλογή **CloudQueue** αντικειμένων.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Πώς μπορείτε να: διαγραφή ουράς

Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, καλέστε τη μέθοδο **deleteIfExists** στο αντικείμενο **CloudQueue** .

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης ουρά, ακολουθήστε αυτές τις συνδέσεις για να μάθετε σχετικά με τις πιο σύνθετες εργασίες χώρου αποθήκευσης.

- [Azure αποθήκευσης SDK για Java][]
- [Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη][]
- [Υπηρεσίες Azure αποθήκευσης REST API][]
- [Ιστολόγιο ομάδας Azure χώρου αποθήκευσης][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure αποθήκευσης SDK για Java]: https://github.com/azure/azure-storage-java
[Azure αποθήκευσης SDK για Android]: https://github.com/azure/azure-storage-android
[Αναφορά SDK Azure αποθήκευσης προγράμματος-πελάτη]: http://dl.windowsazure.com/storage/javadoc/
[Υπηρεσίες Azure αποθήκευσης REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Ιστολόγιο ομάδας Azure χώρου αποθήκευσης]: http://blogs.msdn.com/b/windowsazurestorage/

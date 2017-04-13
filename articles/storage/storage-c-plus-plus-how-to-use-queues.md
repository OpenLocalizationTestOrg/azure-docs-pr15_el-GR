<properties
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά (C++) | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς χώρου αποθήκευσης στο Azure. Δείγματα είναι γραμμένες σε C++."
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

# <a name="how-to-use-queue-storage-from-c"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης ουρά από C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Επισκόπηση
Αυτός ο οδηγός θα σας δείξουν πώς να εκτελείτε συνηθισμένα σενάρια με την υπηρεσία ουράς Azure χώρου αποθήκευσης. Τα δείγματα είναι γραμμένες σε C++ και να χρησιμοποιήσετε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Τα σενάρια καλύπτεται περιλαμβάνουν **εισαγωγής**, **παρατήρηση**, **λήψη**και **Διαγραφή** ουρά μηνυμάτων, καθώς και **τη δημιουργία και διαγραφή ουρές**.

>[AZURE.NOTE] Αυτός ο οδηγός ως προορισμό στη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ έκδοση 1.0.0 και παραπάνω. Συνιστάται η έκδοση είναι βιβλιοθήκη προγράμματος-πελάτη χώρου αποθήκευσης 2.2.0, που είναι διαθέσιμα μέσω [NuGet](http://www.nuget.org/packages/wastorage) ή [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Δημιουργία μιας εφαρμογής C++  
Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε δυνατότητες του χώρου αποθήκευσης που μπορεί να εκτελεστεί μέσα σε μια εφαρμογή C++.

Για να το κάνετε αυτό, θα πρέπει να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++ και δημιουργήστε ένα λογαριασμό Azure χώρο αποθήκευσης στη συνδρομή σας στο Azure.

Για να εγκαταστήσετε τη βιβλιοθήκη Azure αποθήκευσης προγράμματος-πελάτη για C++, μπορείτε να χρησιμοποιήσετε τις παρακάτω μεθόδους:

-   **Linux:** Ακολουθήστε τις οδηγίες που παρέχονται στη σελίδα [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για το αρχείο README C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Των Windows:** Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία > Διαχείριση πακέτου NuGet > Κονσόλα διαχείρισης πακέτου**. Πληκτρολογήστε την παρακάτω εντολή στην [Κονσόλα διαχείρισης πακέτου NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) και πατήστε το πλήκτρο **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να αποκτήσετε πρόσβαση σε ουρά χώρου αποθήκευσης
Προσθέστε τα εξής καταστάσεις στο επάνω μέρος του αρχείου C++ όπου θέλετε να χρησιμοποιήσετε το Azure αποθήκευσης APIs για να αποκτήσετε πρόσβαση ουρές περιλαμβάνουν:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Ρυθμίσετε μια συμβολοσειρά σύνδεσης Azure χώρου αποθήκευσης

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

## <a name="how-to-create-a-queue"></a>Πώς μπορείτε να: δημιουργία ουράς
Ένα αντικείμενο **cloud_queue_client** σάς επιτρέπει να λάβετε αναφορά αντικειμένων για ουρές. Ο ακόλουθος κώδικας δημιουργεί ένα αντικείμενο **cloud_queue_client** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Χρησιμοποιήστε το αντικείμενο **cloud_queue_client** για να λάβετε μια αναφορά στην ουρά που θέλετε να χρησιμοποιήσετε. Μπορείτε να δημιουργήσετε ουρά, εάν δεν υπάρχει.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Πώς μπορείτε να: Εισαγάγετε ένα μήνυμα σε μια ουρά
Για να εισαγάγετε ένα μήνυμα σε μια ήδη υπάρχουσα ουρά, πρέπει πρώτα να δημιουργήσετε μια νέα **cloud_queue_message**. Στη συνέχεια, καλέστε τη μέθοδο **add_message** . Μια **cloud_queue_message** μπορεί να δημιουργηθεί από μια συμβολοσειρά ή έναν πίνακα **byte** . Ακολουθεί κωδικό ο οποίος δημιουργεί μια ουρά (Εάν δεν υπάρχει) και την εισάγει το μήνυμα 'Καλώς όρισες κόσμο':

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Πώς μπορείτε να: Ρίξτε μια ματιά σε στο επόμενο μήνυμα
Μπορείτε να Ρίξτε μια ματιά σε το μήνυμα στο μπροστινό μέρος μια ουρά χωρίς να την καταργήσετε από την ουρά καλώντας τη μέθοδο **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Πώς μπορείτε να: αλλαγή των περιεχομένων ένα μήνυμα σε ουρά
Μπορείτε να αλλάξετε τα περιεχόμενα ενός μηνύματος στην ίδια θέση στην ουρά. Εάν το μήνυμα αντιπροσωπεύει μια εργασία, μπορείτε να χρησιμοποιήσετε αυτήν τη δυνατότητα να ενημερώσετε την κατάσταση της εργασίας εργασίας. Ο ακόλουθος κώδικας ενημερώνει το μήνυμα ουρά με νέο περιεχόμενο και ορίζει το χρονικό όριο αναγνωσιμότητα για να επεκτείνετε μια άλλη 60 δευτερόλεπτα. Αυτό αποθηκεύει την κατάσταση της εργασίας που σχετίζεται με το μήνυμα και παρέχει το πρόγραμμα-πελάτη άλλο λεπτά για να συνεχίσετε να εργάζεστε με το μήνυμα. Μπορείτε να χρησιμοποιήσετε αυτήν την τεχνική για την παρακολούθηση πολλών βήμα ροές εργασιών στην ουρά μηνυμάτων, χωρίς να χρειάζεται να ξεκινήσετε από την αρχή, εάν ένα βήμα επεξεργασίας αποτυγχάνει λόγω αποτυχία υλικού ή λογισμικού. Συνήθως, θα κρατήσετε καθώς και μια καταμέτρηση "Επανάληψη" και, εάν το μήνυμα έχει επαναληφθεί περισσότερες από n φορές, μπορείτε να το διαγράψετε. Αυτό προστατεύει από ένα μήνυμα που ενεργοποιεί ένα σφάλμα στην εφαρμογή κάθε φορά που υποβάλλεται σε επεξεργασία.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Πώς μπορείτε να: καταργήστε την ουρά του επόμενου μηνύματος
Καταργήστε την τον κωδικό ουρές σε ένα μήνυμα από μια ουρά σε δύο βήματα. Όταν καλείτε **get_message**, λαμβάνετε στο επόμενο μήνυμα σε μια ουρά. Ένα μήνυμα που επιστρέφονται από **get_message** δεν είναι ορατό σε οποιονδήποτε άλλο κωδικό ανάγνωση μηνυμάτων από αυτήν την ουρά. Για να ολοκληρώσετε την κατάργηση του μηνύματος από την ουρά, μπορείτε, επίσης, πρέπει να καλέσετε **delete_message**. Αυτή η διαδικασία δύο βημάτων από την κατάργηση ενός μηνύματος διασφαλίζει ότι εάν αποτύχει η επεξεργασία ενός μηνύματος λόγω αποτυχία υλικού ή λογισμικού σας κώδικα, μια άλλη παρουσία του κωδικού σας μπορεί να λάβετε το ίδιο μήνυμα και να προσπαθήστε ξανά. Τον κωδικό κλήσεις **delete_message** δεξιά μετά το μήνυμα έχει υποστεί επεξεργασία.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Πώς μπορείτε να: αξιοποιήσετε πρόσθετες επιλογές, καταργήστε την υπηρεσία Ουράς μηνυμάτων
Υπάρχουν δύο τρόποι για να προσαρμόσετε ανάκτηση μηνύματος από μια ουρά. Πρώτα, μπορείτε να λάβετε μια δέσμη μηνυμάτων (έως 32). Δεύτερον, μπορείτε να ορίσετε ένα χρονικό όριο invisibility μεγαλύτερη ή μικρότερη, επιτρέποντάς σας κώδικα περισσότερες ή λιγότερο χρόνο για την επεξεργασία πλήρως κάθε μήνυμα. Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί τη μέθοδο **get_messages** για 20 μηνύματα σε μία κλήση. Στη συνέχεια, επεξεργάζεται κάθε μήνυμα με χρήση ενός βρόχου **για** . Επίσης, ορίζει το χρονικό όριο invisibility έως πέντε λεπτά για κάθε μήνυμα. Σημειώστε ότι το ξεκινά 5 λεπτά για όλα τα μηνύματα την ίδια στιγμή, επομένως, αφού έχετε 5 λεπτά περάσει από την κλήση σε **get_messages**, τυχόν μηνύματα που δεν έχουν διαγραφεί θα γίνει ορατό ξανά.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Πώς μπορείτε να: λήψη το μήκος ουράς
Μπορείτε να λάβετε μια εκτίμηση της τον αριθμό των μηνυμάτων σε μια ουρά. Η μέθοδος **download_attributes** ζητά από την υπηρεσία ουράς για να ανακτήσετε τα χαρακτηριστικά ουρά, όπως το πλήθος μήνυμα. Η μέθοδος **approximate_message_count** λαμβάνει τον κατά προσέγγιση αριθμό των μηνυμάτων στην ουρά.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Πώς μπορείτε να: διαγραφή ουράς
Για να διαγράψετε μια ουρά και όλα τα μηνύματα που περιέχονται σε αυτό, καλέστε τη μέθοδο **delete_queue_if_exists** του αντικειμένου ουράς.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του χώρου αποθήκευσης ουρά, ακολουθήστε αυτές τις συνδέσεις για να μάθετε περισσότερα σχετικά με το χώρο αποθήκευσης Azure.

-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων από C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Λίστα πόρων Azure αποθήκευσης σε C++](storage-c-plus-plus-enumeration.md)
-   [Βιβλιοθήκη προγράμματος-πελάτη του χώρου αποθήκευσης για αναφορά C++](http://azure.github.io/azure-storage-cpp)
-   [Τεκμηρίωση Azure χώρου αποθήκευσης](https://azure.microsoft.com/documentation/services/storage/)


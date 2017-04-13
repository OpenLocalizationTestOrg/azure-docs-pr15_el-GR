<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας με PHP | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας στο Azure. Δείγματα κώδικα γραμμένο σε PHP." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Αυτός ο οδηγός δείχνει πώς μπορείτε να χρησιμοποιήσετε ουρές Bus υπηρεσίας. Τα δείγματα είναι γραμμένες σε PHP και χρησιμοποιήστε το [Azure SDK για PHP](../php-download-sdk.md). Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία ουρές**, **Αποστολή και λήψη μηνυμάτων**και **τη διαγραφή ουρές**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>Δημιουργία μιας εφαρμογής PHP

Η μόνη απαίτηση για τη δημιουργία μιας εφαρμογής PHP που αποκτά πρόσβαση στην υπηρεσία αντικειμένων Blob του Azure είναι η αναφορά κλάσεων στο [Azure SDK για PHP](../php-download-sdk.md) από τον κωδικό. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εργαλεία ανάπτυξης για να δημιουργήσετε την εφαρμογή σας ή το Σημειωματάριο.

> [AZURE.NOTE] Την εγκατάσταση του PHP πρέπει να έχουν την [επέκταση OpenSSL](http://php.net/openssl) εγκατασταθεί και ενεργοποιηθεί.

Σε αυτόν τον οδηγό, θα χρησιμοποιήσετε δυνατότητες υπηρεσίας που μπορεί να ονομάζεται από μέσα σε μια εφαρμογή PHP τοπικά ή σε κώδικα που εκτελείται μέσα σε ένα ρόλο Azure web, ρόλο εργαζόμενου ή τοποθεσία Web.

## <a name="get-the-azure-client-libraries"></a>Λήψη του προγράμματος-πελάτη Azure βιβλιοθήκες

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Για να χρησιμοποιήσετε την υπηρεσία Bus ουρά APIs, κάντε τα εξής:

1. Αναφορά στο αρχείο συσκευή αυτόματης φόρτωσης χρησιμοποιώντας το [require_once] [ require_once] πρόταση.
2. Αναφορά οποιαδήποτε κλάσεις που μπορείτε να χρησιμοποιήσετε.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να συμπεριλάβετε το αρχείο συσκευή αυτόματης φόρτωσης και αναφορά την κλάση **ServicesBuilder** .

> [AZURE.NOTE] Αυτό το παράδειγμα (και άλλα παραδείγματα σε αυτό το άρθρο) προϋποθέτει ότι έχετε εγκαταστήσει τις βιβλιοθήκες PHP προγράμματος-πελάτη για Azure μέσω σύνθεσης. Εάν έχετε εγκαταστήσει τις βιβλιοθήκες με μη αυτόματο τρόπο ή ως ένα πακέτο τις ΑΧΛΑΔΙΈΣ, πρέπει να αναφέρεται το αρχείο συσκευή αυτόματης φόρτωσης **WindowsAzure.php** .

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Στα παρακάτω παραδείγματα, η `require_once` δήλωση θα εμφανίζονται πάντα, αλλά μόνο τις κατηγορίες που είναι απαραίτητο για να εκτελέσει το παράδειγμα αναφέρονται.

## <a name="set-up-a-service-bus-connection"></a>Ρύθμιση σύνδεσης Bus υπηρεσίας

Για να ξεκινήσει ένα πρόγραμμα-πελάτη Bus υπηρεσία, πρέπει πρώτα να έχετε μια έγκυρη συμβολοσειρά σύνδεσης σε αυτήν τη μορφή:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Όπου **το τελικό σημείο** είναι συνήθως της μορφής `[yourNamespace].servicebus.windows.net`.

Για να δημιουργήσετε οποιοδήποτε πρόγραμμα-πελάτη Azure service πρέπει να χρησιμοποιήσετε την κλάση **ServicesBuilder** . Μπορείς:

* Μεταβίβαση τη συμβολοσειρά σύνδεσης απευθείας σε αυτήν.
* Χρησιμοποιήστε το **CloudConfigurationManager (ΚΦΜ)** για να ελέγξετε πολλές εξωτερικές προελεύσεις για τη συμβολοσειρά σύνδεσης:
    * Από προεπιλογή διατίθεται με την υποστήριξη για ένα εξωτερικό αρχείο προέλευσης - μεταβλητές περιβάλλοντος
    * Μπορείτε να προσθέσετε νέες προελεύσεις επεκτείνοντας την κλάση **ConnectionStringSource**

Για παραδείγματα που περιγράφονται εδώ, τη συμβολοσειρά σύνδεσης θα περάσουν απευθείας.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Πώς μπορείτε να: δημιουργία ουράς

Μπορείτε να εκτελέσετε λειτουργίες διαχείρισης για ουρές Bus υπηρεσίας μέσω της κλάσης **ServiceBusRestProxy** . Ένα αντικείμενο **ServiceBusRestProxy** έχει συνταχθεί μέσω της μεθόδου εργοστασίου **ServicesBuilder::createServiceBusService** με μια κατάλληλη σύνδεση συμβολοσειρά που ενσωματώνει το διακριτικό δικαιωμάτων για τη διαχείριση του.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για να ξεκινήσει μια **ServiceBusRestProxy** και καλέστε **ServiceBusRestProxy -> createQueue** για να δημιουργήσετε μια ουρά με το όνομα `myqueue` μέσα σε μια `MySBNamespace` χώρος ονομάτων υπηρεσίας:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε το `listQueues` μέθοδος σε `ServiceBusRestProxy` αντικειμένων για να ελέγξετε εάν υπάρχει ήδη μια ουρά με ένα καθορισμένο όνομα μέσα σε ένα χώρο ονομάτων.

## <a name="how-to-send-messages-to-a-queue"></a>Πώς μπορείτε να: αποστολή μηνυμάτων σε μια ουρά

Για να στείλετε ένα μήνυμα σε ουρά Bus υπηρεσίας, η εφαρμογή σας καλεί τη μέθοδο **ServiceBusRestProxy -> sendQueueMessage** . Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα για να το `myqueue` ουρά προηγουμένως δημιουργήσει εντός του `MySBNamespace` χώρος ονομάτων υπηρεσίας.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Μηνύματα ουρές Bus υπηρεσία αποστολής για να (και λήψης από) είναι παρουσίες της κλάσης **BrokeredMessage** . Τα αντικείμενα **BrokeredMessage** έχουν ένα σύνολο τυπικές μεθόδους (όπως **getLabel**, **getTimeToLive**, **setLabel**και **setTimeToLive**) και τις ιδιότητες που χρησιμοποιούνται για τη διατήρηση προσαρμοσμένες ιδιότητες συγκεκριμένη εφαρμογή και ένας οργανισμός των δεδομένων αυθαίρετο εφαρμογής.

Υπηρεσία Bus ουρές υποστηρίζουν μέγιστο μέγεθος αρχείου των 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ουρά, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει μια ουρά. Αυτό το όριο επάνω στο μέγεθος της ουράς είναι 5 GB.

## <a name="how-to-receive-messages-from-a-queue"></a>Πώς μπορείτε να λαμβάνετε μηνύματα από μια ουρά

Ο καλύτερος τρόπος για να λάβετε μηνύματα από μια ουρά είναι να χρησιμοποιήσετε μια μέθοδο **ServiceBusRestProxy -> receiveQueueMessage** . Είναι δυνατή η λήψη μηνυμάτων σε δύο διαφορετικές καταστάσεις λειτουργίας: **ReceiveAndDelete** (προεπιλογή) και **PeekLock**.

Όταν λαμβάνετε χρησιμοποιώντας τη λειτουργία **ReceiveAndDelete** , είναι μια λειτουργία μίας στιγμιότυπο - δηλαδή, όταν Bus υπηρεσία λαμβάνει ένα αποδεικτικό ανάγνωσης για ένα μήνυμα σε μια ουρά, το επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** , λαμβάνετε ένα μήνυμα μετατρέπεται σε μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλες τους καταναλωτές να πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής κατά την προώθηση του ληφθέντος μηνύματος στον **ServiceBusRestProxy -> deleteMessage**. Όταν Bus υπηρεσίας βλέπει **deleteMessage** κλήσεων, θα επισημάνετε το μήνυμα ως που καταναλώνεται και να την καταργήσετε από την ουρά.

Το παρακάτω παράδειγμα εμφανίζει πώς μπορούν να ληφθούν ένα μήνυμα και υποβάλλονται σε επεξεργασία με χρήση της κατάστασης λειτουργίας **PeekLock** (όχι η προεπιλεγμένη κατάσταση λειτουργίας).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς μπορείτε να: χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **unlockMessage** του ληφθέντος μηνύματος (αντί για τη μέθοδο **deleteMessage** ). Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα μέσα στην ουρά και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο μέσα στην ουρά και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus θα ξεκλειδώσετε αυτόματα το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης **deleteMessage** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον μία φορά επεξεργασία**; Αυτό σημαίνει ότι κάθε μήνυμα υποβάλλεται σε επεξεργασία τουλάχιστον μία φορά, αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προσθήκη επιπλέον λογικής για τις εφαρμογές του χειρισμού διπλότυπων μήνυμα παράδοσης συνιστάται. Συχνά, αυτό είναι δυνατό με τη μέθοδο **getMessageId** του μηνύματος, η οποία παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του ουρές Bus υπηρεσίας, ανατρέξτε στο θέμα [ουρές, θέματα, και συνδρομές][] για περισσότερες πληροφορίες.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το [Κέντρο για προγραμματιστές PHP](/develop/php/).

[Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 

<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε τα θέματα υπηρεσίας Bus με PHP | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τα θέματα υπηρεσίας Bus με PHP στο Azure." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Πώς μπορείτε να χρησιμοποιήσετε θέματα Bus υπηρεσίας και συνδρομών

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Αυτό το άρθρο σας δείχνει πώς μπορείτε να χρησιμοποιήσετε θέματα Bus υπηρεσίας και συνδρομών. Τα δείγματα είναι γραμμένες σε PHP και χρησιμοποιήστε το [Azure SDK για PHP](../php-download-sdk.md). Τα σενάρια καλύπτεται περιλαμβάνουν **τη δημιουργία θέματα και συνδρομές**, **τη δημιουργία συνδρομής φίλτρα**, **Αποστολή μηνυμάτων σε ένα θέμα**, **λαμβάνετε μηνύματα από μια συνδρομή**και **Διαγραφή θέματα και συνδρομών**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>Δημιουργία μιας εφαρμογής PHP

Είναι η μόνη απαίτηση για τη δημιουργία μιας εφαρμογής PHP που αποκτά πρόσβαση στην υπηρεσία αντικειμένων Blob του Azure για αναφορά κλάσεις στο [Azure SDK για PHP](../php-download-sdk.md) από τον κωδικό. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εργαλεία ανάπτυξης για να δημιουργήσετε την εφαρμογή σας ή το Σημειωματάριο.

> [AZURE.NOTE] Την εγκατάσταση του PHP πρέπει να έχουν την [επέκταση OpenSSL](http://php.net/openssl) εγκατασταθεί και ενεργοποιηθεί.

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της υπηρεσίας δυνατότητες που μπορεί να ονομάζεται μέσα σε μια εφαρμογή PHP τοπικά ή σε κώδικα που εκτελείται μέσα σε ένα ρόλο Azure web, ρόλο εργαζόμενου ή τοποθεσία Web.

## <a name="get-the-azure-client-libraries"></a>Λήψη του προγράμματος-πελάτη Azure βιβλιοθήκες

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Ρύθμιση των παραμέτρων σας εφαρμογής για να χρησιμοποιήσετε Bus υπηρεσίας

Για να χρησιμοποιήσετε τα API Bus υπηρεσίας:

1. Αναφορά στο αρχείο συσκευή αυτόματης φόρτωσης χρησιμοποιώντας το [require_once] [ require-once] πρόταση.
2. Αναφορά οποιαδήποτε κλάσεις που μπορείτε να χρησιμοποιήσετε.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να συμπεριλάβετε το αρχείο συσκευή αυτόματης φόρτωσης και αναφορά την κλάση **ServiceBusService** .

> [AZURE.NOTE] Αυτό το παράδειγμα (και άλλα παραδείγματα σε αυτό το άρθρο) προϋποθέτει ότι έχετε εγκαταστήσει τις βιβλιοθήκες PHP προγράμματος-πελάτη για Azure μέσω σύνθεσης. Εάν έχετε εγκαταστήσει τις βιβλιοθήκες με μη αυτόματο τρόπο ή ως ένα πακέτο τις ΑΧΛΑΔΙΈΣ, πρέπει να αναφέρεται το αρχείο συσκευή αυτόματης φόρτωσης **WindowsAzure.php** .

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Στα παρακάτω παραδείγματα, η `require_once` δήλωση θα εμφανίζονται πάντα, αλλά μόνο τις κατηγορίες που είναι απαραίτητο για να εκτελέσει το παράδειγμα αναφέρονται.

## <a name="set-up-a-service-bus-connection"></a>Ρύθμιση σύνδεσης Bus υπηρεσίας

Για να ξεκινήσει ένα πρόγραμμα-πελάτη υπηρεσίας Bus πρέπει πρώτα να έχετε μια έγκυρη συμβολοσειρά σύνδεσης σε αυτήν τη μορφή:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Όπου `Endpoint` είναι συνήθως της μορφής `https://[yourNamespace].servicebus.windows.net`.

Για να δημιουργήσετε οποιοδήποτε πρόγραμμα-πελάτη Azure service πρέπει να χρησιμοποιήσετε την κλάση **ServicesBuilder** . Μπορείς:

* Μεταβίβαση τη συμβολοσειρά σύνδεσης απευθείας σε αυτήν.
* Χρησιμοποιήστε το **CloudConfigurationManager (ΚΦΜ)** για να ελέγξετε πολλές εξωτερικές προελεύσεις για τη συμβολοσειρά σύνδεσης:
    * Από προεπιλογή θα με την υποστήριξη για ένα εξωτερικό αρχείο προέλευσης - μεταβλητές περιβάλλοντος.
    * Μπορείτε να προσθέσετε νέες προελεύσεις επεκτείνοντας την κλάση **ConnectionStringSource** .

Για παραδείγματα που περιγράφονται εδώ, μεταβιβάζεται απευθείας τη συμβολοσειρά σύνδεσης.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Δημιουργία θέματος

Μπορείτε να εκτελέσετε λειτουργίες διαχείρισης για θέματα Bus υπηρεσίας μέσω της κλάσης **ServiceBusRestProxy** . Ένα αντικείμενο **ServiceBusRestProxy** έχει συνταχθεί μέσω της μεθόδου εργοστασίου **ServicesBuilder::createServiceBusService** με μια κατάλληλη σύνδεση συμβολοσειρά που ενσωματώνει το διακριτικό δικαιωμάτων για τη διαχείριση του.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για να ξεκινήσει μια **ServiceBusRestProxy** και καλέστε **ServiceBusRestProxy -> createTopic** για να δημιουργήσετε ένα θέμα με το όνομα `mytopic` μέσα σε μια `MySBNamespace` χώρος ονομάτων:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε το `listTopics` μέθοδος σε `ServiceBusRestProxy` αντικειμένων για να ελέγξετε εάν υπάρχει ήδη ένα θέμα με ένα καθορισμένο όνομα μέσα σε ένα χώρο ονομάτων υπηρεσίας.

## <a name="create-a-subscription"></a>Δημιουργία εγγραφής

Το θέμα συνδρομές δημιουργούνται επίσης με τη μέθοδο **ServiceBusRestProxy -> createSubscription** . Συνδρομές ονομάζονται και μπορούν να έχουν ένα προαιρετικό φίλτρο που περιορίζει το σύνολο των μηνυμάτων που του μεταβιβάστηκε εικονικού ουρά τη συνδρομή.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Δημιουργήστε μια συνδρομή με το φίλτρο προεπιλεγμένη (MatchAll)

Το φίλτρο **MatchAll** είναι το προεπιλεγμένο φίλτρο που χρησιμοποιείται, αν υπάρχει φίλτρο έχει καθοριστεί όταν δημιουργείται μια νέα συνδρομή. Όταν χρησιμοποιείται το φίλτρο **MatchAll** , όλα τα μηνύματα που δημοσιεύονται στο θέμα τοποθετούνται στην ουρά εικονικού τη συνδρομή. Το παρακάτω παράδειγμα δημιουργεί μια συνδρομή με το όνομα 'mysubscription' και χρησιμοποιεί το προεπιλεγμένο φίλτρο **MatchAll** .

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Δημιουργία συνδρομές με τα φίλτρα

Μπορείτε επίσης να ορίσετε φίλτρα που σας επιτρέπουν να καθορίσετε ποια μηνύματα που αποστέλλονται σε ένα θέμα, θα πρέπει να εμφανίζονται μέσα σε μια συνδρομή στο συγκεκριμένο θέμα. Η πιο ευέλικτη τύπο φίλτρου που υποστηρίζονται από συνδρομές είναι το **SqlFilter**, που εφαρμόζει ένα υποσύνολο SQL92. Φίλτρα SQL λειτουργούν σχετικά με τις ιδιότητες των μηνυμάτων που δημοσιεύονται στο θέμα. Για περισσότερες πληροφορίες σχετικά με το SqlFilters, ανατρέξτε στο θέμα [Ιδιότητα SqlFilter.SqlExpression][sqlfilter].

> [AZURE.NOTE] Κάθε κανόνας σε μια συνδρομή επεξεργάζεται εισερχόμενα μηνύματα ανεξάρτητα από αυτό, προσθήκη τους αποτέλεσμα μηνυμάτων με τη συνδρομή. Επιπλέον, κάθε νέα συνδρομή έχει ένα προεπιλεγμένο αντικείμενο **κανόνα** με φίλτρο που προσθέτει όλα τα μηνύματα από το θέμα με τη συνδρομή. Για να εμφανιστούν μόνο τα μηνύματα που ταιριάζουν με το φίλτρο, πρέπει να καταργήσετε τον προεπιλεγμένο κανόνα. Μπορείτε να καταργήσετε τον προεπιλεγμένο κανόνα, χρησιμοποιώντας το `ServiceBusRestProxy->deleteRule` μέθοδο.

Το παρακάτω παράδειγμα δημιουργεί μια εγγραφή με όνομα **HighMessages** με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια προσαρμοσμένη ιδιότητα **αριθμού μηνύματος** που είναι μεγαλύτερη από 3 (ανατρέξτε στο θέμα [Αποστολή μηνυμάτων σε ένα θέμα](#send-messages-to-a-topic) για πληροφορίες σχετικά με την προσθήκη προσαρμοσμένες ιδιότητες σε μηνύματα):

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Σημειώστε ότι αυτός ο κωδικός απαιτεί τη χρήση ενός επιπλέον χώρο ονομάτων: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Ομοίως, το ακόλουθο παράδειγμα δημιουργεί μια εγγραφή με όνομα **LowMessages** με **SqlFilter** που επιλέγει μόνο τα μηνύματα που έχουν μια **αριθμού μηνύματος** ιδιότητα μικρότερη ή ίση με 3:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Τώρα, κατά την αποστολή ενός μηνύματος για να το `mytopic` θέμα, πάντα παραδοθεί σε μια συνδρομή στο δέκτες το `mysubscription` συνδρομή, και να παραδοθεί επιλεκτικής δέκτες μια συνδρομή στο το `HighMessages` και `LowMessages` συνδρομές (ανάλογα με το περιεχόμενο του μηνύματος).

## <a name="send-messages-to-a-topic"></a>Αποστολή μηνυμάτων σε ένα θέμα

Για να στείλετε ένα μήνυμα σε ένα θέμα Bus υπηρεσίας, η εφαρμογή σας καλεί τη μέθοδο **ServiceBusRestProxy -> sendTopicMessage** . Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να στείλετε ένα μήνυμα για να το `mytopic` θέμα προηγουμένως δημιουργήσει εντός του `MySBNamespace` χώρος ονομάτων υπηρεσίας.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Τα μηνύματα που αποστέλλονται σε θέματα υπηρεσίας Bus είναι παρουσίες της κλάσης **BrokeredMessage** . Τα αντικείμενα **BrokeredMessage** έχουν ένα σύνολο τυπικές ιδιότητες και μεθόδους (όπως **getLabel** **getTimeToLive**, **setLabel**και **setTimeToLive**), καθώς και τις ιδιότητες που μπορούν να χρησιμοποιηθούν για τη διατήρηση προσαρμοσμένες ιδιότητες συγκεκριμένη εφαρμογή. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να στείλετε 5 δοκιμαστικών μηνυμάτων για να το `mytopic` προηγουμένως δημιουργήσει το θέμα. Τη μέθοδο **Ορισμός ιδιότητας** χρησιμοποιείται για να προσθέσετε μια προσαρμοσμένη ιδιότητα (`MessageNumber`) σε κάθε μήνυμα. Λάβετε υπόψη ότι η `MessageNumber` τιμή της ιδιότητας διαφέρει σε κάθε μήνυμα (μπορείτε να χρησιμοποιήσετε αυτή την τιμή για να καθορίσετε ποιες συνδρομές λαμβάνουν, όπως φαίνεται στην ενότητα [Δημιουργία συνδρομής](#create-a-subscription) ):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Θέματα υπηρεσίας Bus υποστηρίζει ένα μέγιστο μέγεθος μηνύματος 256 KB η [τυπική σειρά](service-bus-premium-messaging.md) και 1 MB στο [επίπεδο Premium](service-bus-premium-messaging.md). Την επικεφαλίδα, η οποία περιλαμβάνει τις ιδιότητες τυπικών και προσαρμοσμένων εφαρμογών, μπορεί να έχει ένα μέγιστο μέγεθος των 64 KB. Υπάρχει όριο στον αριθμό των μηνυμάτων που θα διατηρούνται σε ένα θέμα, αλλά υπάρχει μια καπέλο το συνολικό μέγεθος των μηνυμάτων που διαθέτει ένα θέμα. Αυτό το όριο επάνω στο θέμα μέγεθος είναι 5 GB. Για περισσότερες πληροφορίες σχετικά με τα όρια, ανατρέξτε στο θέμα [όρια Bus υπηρεσίας][].

## <a name="receive-messages-from-a-subscription"></a>Λήψη μηνυμάτων από μια συνδρομή

Ο καλύτερος τρόπος για να λάβετε μηνύματα από μια συνδρομή είναι να χρησιμοποιήσετε μια μέθοδο **ServiceBusRestProxy -> receiveSubscriptionMessage** . Μηνύματα που λαμβάνετε να εργαστείτε σε δύο διαφορετικές καταστάσεις λειτουργίας: **ReceiveAndDelete** (προεπιλογή) και **PeekLock**.

Όταν λαμβάνετε χρησιμοποιώντας τη λειτουργία **ReceiveAndDelete** , είναι μια λειτουργία μίας στιγμιότυπο. Αυτό σημαίνει ότι όταν Bus υπηρεσία λαμβάνει ένα αποδεικτικό ανάγνωσης για ένα μήνυμα σε μια συνδρομή, το επισημαίνει το μήνυμα ως που καταναλώνεται και επιστρέφει στην εφαρμογή. Η λειτουργία **ReceiveAndDelete** είναι το μοντέλο απλούστερος και λειτουργεί καλύτερα για σενάρια στις οποίες μια εφαρμογή μπορεί να αποδεχτεί τις δεν επεξεργασία ενός μηνύματος σε περίπτωση αποτυχίας. Για να κατανοήσετε αυτήν, εξετάστε το ενδεχόμενο να ένα σενάριο που ζητήματα της αίτησης λήψης του καταναλωτή και, στη συνέχεια, παρουσιάζει σφάλμα πριν από την επεξεργασία του. Επειδή η υπηρεσία Bus θα έχουν σήμανση του μηνύματος ως που καταναλώνεται, στη συνέχεια, όταν η εφαρμογή επανεκκίνηση του και αρχίζει κατανάλωση ξανά τα μηνύματα, το θα έχετε παραβλέψει το μήνυμα που καταναλώθηκε πριν από το σφάλμα.

Στη λειτουργία **PeekLock** , λαμβάνετε ένα μήνυμα μετατρέπεται σε μια λειτουργία δύο στάδιο, γεγονός που επιτρέπει σε εφαρμογές υποστήριξης που δεν θέλετε να γίνει μηνύματα που λείπουν. Όταν Bus υπηρεσία λαμβάνει μια αίτηση, βρίσκει στο επόμενο μήνυμα να καταναλωθεί, κλειδώνει για να αποτρέψετε άλλους καταναλωτές πραγματοποιεί λήψη του και, στη συνέχεια, επιστρέφει το στην εφαρμογή. Μετά την εφαρμογή ολοκληρώσει την επεξεργασία του μηνύματος (ή αποθηκεύει αξιόπιστα για μελλοντική επεξεργασία), ολοκληρώνει δεύτερου σταδίου της διαδικασίας παραλαβής κατά την προώθηση του ληφθέντος μηνύματος στον **ServiceBusRestProxy -> deleteMessage**. Όταν Bus υπηρεσίας βλέπει **deleteMessage** κλήσεων, θα επισημάνετε το μήνυμα ως που καταναλώνεται και να την καταργήσετε από την ουρά.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να λαμβάνετε και επεξεργασία ενός μηνύματος με λειτουργία **PeekLock** (όχι η προεπιλεγμένη κατάσταση λειτουργίας). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Πώς μπορείτε να: χειριστείτε σφάλματα εφαρμογών και αναγνώσιμα μηνύματα

Υπηρεσία Bus παρέχει λειτουργικότητα που θα σας βοηθήσουν να ανακτήσετε ομαλά από σφάλματα στο εφαρμογής ή δυσκολίες επεξεργασία ενός μηνύματος. Εάν μια εφαρμογή ακουστικό είναι δυνατή η επεξεργασία του μηνύματος για κάποιο λόγο, στη συνέχεια, το να καλέσετε τη μέθοδο **unlockMessage** του ληφθέντος μηνύματος (αντί για τη μέθοδο **deleteMessage** ). Αυτό θα προκαλέσει Bus υπηρεσίας για να ξεκλειδώσετε το μήνυμα μέσα στην ουρά και να την κάνετε διαθέσιμη θα ληφθεί ξανά, με την ίδια εφαρμογή καταναλώνει ή από άλλη εφαρμογή καταναλώνει.

Υπάρχει επίσης ένα χρονικό όριο που σχετίζεται με ένα μήνυμα κλειδωμένο μέσα στην ουρά και, εάν η εφαρμογή δεν μπορεί να επεξεργαστεί το μήνυμα πριν από την κλείδωμα λήξει το χρονικό όριο (για παράδειγμα, εάν η εφαρμογή παρουσιάζει σφάλμα), στη συνέχεια, υπηρεσία Bus θα ξεκλειδώσετε αυτόματα το μήνυμα και να την κάνετε διαθέσιμη θα ληφθεί ξανά.

Σε περίπτωση που η εφαρμογή παρουσιάζει σφάλμα μετά την επεξεργασία του μηνύματος, αλλά πριν από την έκδοση της αίτησης **deleteMessage** , στη συνέχεια, το μήνυμα θα είναι redelivered στην εφαρμογή κατά την επανεκκίνηση. Συχνά αυτό ονομάζεται **Τουλάχιστον μία φορά επεξεργασία**; Αυτό σημαίνει ότι κάθε μήνυμα υποβάλλεται σε επεξεργασία τουλάχιστον μία φορά, αλλά σε ορισμένες περιπτώσεις ίσως redelivered το ίδιο μήνυμα. Εάν το σενάριο δεν θέλετε να γίνει διπλότυπων επεξεργασίας, στη συνέχεια, προγραμματιστές εφαρμογών θα πρέπει να προσθέσετε επιπλέον λογική για τις εφαρμογές του χειρισμού διπλότυπων μήνυμα παράδοσης. Συχνά, αυτό είναι δυνατό με τη μέθοδο **getMessageId** του μηνύματος, η οποία παραμένει σταθερή κατά μήκος προσπαθειών παράδοσης.

## <a name="delete-topics-and-subscriptions"></a>Διαγράψτε τα θέματα και συνδρομών

Για να διαγράψετε ένα θέμα ή μια συνδρομή, χρησιμοποιήστε το **ServiceBusRestProxy -> deleteTopic** ή τις μεθόδους **ServiceBusRestProxy -> deleteSubscripton** , αντίστοιχα. Σημειώστε ότι η διαγραφή ενός θέματος διαγράφει επίσης τις συνδρομές που έχουν καταχωρηθεί με το θέμα.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε ένα θέμα με το όνομα `mytopic` και τις συνδρομές καταχωρημένες.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Χρησιμοποιώντας τη μέθοδο **deleteSubscription** , μπορείτε να διαγράψετε μια συνδρομή ανεξάρτητη:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε τα βασικά στοιχεία του ουρές Bus υπηρεσίας, ανατρέξτε στο θέμα [ουρές, θέματα, και συνδρομές][] για περισσότερες πληροφορίες.

[Ουρές, θέματα και συνδρομών]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Υπηρεσία Bus ορίων]: service-bus-quotas.md

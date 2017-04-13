<properties 
    pageTitle="Υποστήριξη AMQP 1.0 για Bus υπηρεσίας διαμερίσματα ουρές και θέματα | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε τις ουρές για προχωρημένους μήνυμα ουράς Protocol (AMQP) 1.0 με υπηρεσία Bus διαμερίσματα και θέματα." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>Υποστήριξη AMQP 1.0 Bus υπηρεσίας διαμερίσματα ουρές και θέματα 

Azure Service Bus τώρα υποστηρίζει το Advanced μήνυμα ουράς πρωτόκολλο (**AMQP**) 1.0 για υπηρεσία Bus **διαμερίσματα ουρές και θέματα.**

**AMQP** είναι ένα πρωτόκολλο ουράς Άνοιγμα τυπικό μήνυμα που σας επιτρέπει να αναπτύξετε εφαρμογές πλατφόρμες με διαφορετικές γλώσσες προγραμματισμού. Για γενικές πληροφορίες σχετικά με το AMQP υποστηρίζει σε Bus υπηρεσίας, ανατρέξτε στο θέμα [AMQP 1.0 υποστηρίζει σε Bus υπηρεσίας](service-bus-amqp-overview.md).

**Partitioned ουρές και θέματα**, γνωστές και ως *διαμερίσματα οντοτήτων*, προσφέρουν μεγαλύτερη διαθεσιμότητα, την αξιοπιστία και μετάδοσης από συμβατικός ουρές δεν έχουν δημιουργηθεί διαμερίσματα και θέματα. Για περισσότερες πληροφορίες σχετικά με τα διαμερίσματα οντοτήτων, ανατρέξτε στο θέμα [Διαμερίσματα οντοτήτων μηνυμάτων](service-bus-partitioning.md).

Η προσθήκη της AMQP 1.0 ως πρωτόκολλο για την επικοινωνία με διαμερίσματα ουρές και θέματα σάς επιτρέπει να δημιουργήσετε διαλειτουργικού εφαρμογές που μπορούν να επωφεληθούν από τη μεγαλύτερη διαθεσιμότητα, αξιοπιστία, και σε όλο το που παρέχεται από την υπηρεσία Bus διαμερίσματα οντοτήτων.

Για λεπτομερείς σύρματος επιπέδου AMQP 1.0 πρωτόκολλο οδηγίες, που εξηγεί τον τρόπο Bus υπηρεσία υλοποιεί και δημιουργεί τις προδιαγραφές της τεχνικής OASIS AMQP, ανατρέξτε στο θέμα το [AMQP 1.0 στο Azure Service Bus και διανομείς συμβάν πρωτόκολλο Οδηγός](service-bus-amqp-protocol-guide.md).    

## <a name="use-amqp-with-partitioned-queues"></a>Χρήση AMQP με διαμερίσματα ουρές

Ουρές είναι χρήσιμη για σενάρια που απαιτούν χρονικό αποσύνδεση, φόρτωση ισοστάθμιση, εξισορρόπηση φόρτου και ελεύθερη ζεύξης. Με μια ουρά, εκδότες αποστολή μηνυμάτων στην ουρά και οι καταναλωτές λήψη μηνυμάτων από την ουρά, στις περιπτώσεις όπου ένα μήνυμα μόνο είναι δυνατή η λήψη μόνο μία φορά. Ένα καλό παράδειγμα αυτό είναι ένα σύστημα απογραφής στο οποίο διατάξεων σταθμούς δημοσίευση δεδομένων σε μια ουρά αντί απευθείας στο σύστημα διαχείρισης αποθέματος. Το σύστημα διαχείρισης αποθέματος καταναλώνει, στη συνέχεια, τα δεδομένα ανά πάσα στιγμή για τη Διαχείριση μετοχών αναπλήρωσης. Αυτό έχει πολλά πλεονεκτήματα, όπως το σύστημα διαχείρισης αποθέματος δεν χρειάζεται να είναι σε σύνδεση πάντα. Για περισσότερες λεπτομέρειες σχετικά με την υπηρεσία Bus ουρές, ανατρέξτε στο θέμα [Δημιουργία εφαρμογές που χρησιμοποιούν ουρές Bus υπηρεσίας](service-bus-create-queues.md). 

Ένα διαμερίσματα ουρά περαιτέρω αυξάνει τη διαθεσιμότητα, την αξιοπιστία και μετάδοσης για εφαρμογές, όπως αυτές οι ουρές δημιουργούνται διαμερίσματα σε πολλές μεσίτες μήνυμα και αποθηκεύει ανταλλαγής μηνυμάτων.     

### <a name="create-partitioned-queues"></a>Δημιουργία διαμερίσματα ουρές

Μπορείτε να δημιουργήσετε ένα διαμερίσματα ουρά με χρήση του [Azure κλασική πύλη][] και το SDK Bus υπηρεσίας. Για να δημιουργήσετε ένα διαμερίσματα ουρά, ορίστε την ιδιότητα [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) **true** στην παρουσία [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) . Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε μια ουρά διαμερίσματα χρησιμοποιώντας το SDK Bus υπηρεσίας. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Αποστολή και λήψη μηνυμάτων με χρήση AMQP

Να στέλνετε μηνύματα, και να λαμβάνετε μηνύματα από ένα διαμερίσματα ουρά χρησιμοποιώντας AMQP ως πρωτόκολλο, ορίζοντας την ιδιότητα [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) όπως φαίνεται στο ακόλουθο κώδικα.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Χρήση AMQP με διαμερίσματα θέματα

Τα θέματα είναι εννοιολογικά παρόμοια με ουρές, αλλά τα θέματα να δρομολογήσετε ένα αντίγραφο του ίδιου μηνύματος σε πολλές *συνδρομές*. Σε ένα θέμα, εκδότες αποστολή μηνυμάτων στο θέμα και οι καταναλωτές λήψη μηνυμάτων από συνδρομές. Στο διατάξεων σενάριο συστήματος απόθεμα, τερματικούς σταθμούς δημοσίευση δεδομένων στο θέμα. Το σύστημα διαχείρισης αποθέματος, στη συνέχεια, λαμβάνει μηνύματα από μια συνδρομή. Επιπλέον, ένα σύστημα παρακολούθησης μπορεί να λάβετε το ίδιο μήνυμα από μια διαφορετική συνδρομή. Για περισσότερες λεπτομέρειες σχετικά με θέματα Bus υπηρεσίας και συνδρομές, ανατρέξτε στο θέμα [Δημιουργία εφαρμογές που χρησιμοποιούν θέματα Bus υπηρεσίας και συνδρομών](service-bus-create-topics-subscriptions.md). 

Όπως με ουρές, διαμερίσματα θέματα περαιτέρω αύξηση του τη διαθεσιμότητα, την αξιοπιστία και μετάδοσης για εφαρμογές, όπως αυτά τα θέματα και τις συνδρομές δημιουργούνται διαμερίσματα σε πολλές μεσίτες μήνυμα και αποθηκεύει ανταλλαγής μηνυμάτων. 

### <a name="create-partitioned-topics"></a>Δημιουργία διαμερίσματα θέματα

Μπορείτε να δημιουργήσετε ένα διαμερίσματα στο θέμα χρήση του [Azure κλασική πύλη][] και το SDK Bus υπηρεσίας. Για να δημιουργήσετε ένα διαμερίσματα θέμα, ορίστε την ιδιότητα [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) **true** στην παρουσία [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να δημιουργήσετε ένα διαμερίσματα θέμα χρησιμοποιώντας το SDK Bus υπηρεσίας.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Αποστολή και λήψη μηνυμάτων με χρήση AMQP

Μπορείτε να στέλνετε μηνύματα και λήψη μηνυμάτων από μια συνδρομή διαμερίσματα το θέμα χρησιμοποιώντας AMQP ως ένα πρωτόκολλο, ορίζοντας την ιδιότητα [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), όπως φαίνεται στο ακόλουθο κώδικα.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Επόμενα βήματα

Δείτε τις ακόλουθες πρόσθετες πληροφορίες για να μάθετε περισσότερα σχετικά με τα διαμερίσματα οντοτήτων ανταλλαγής μηνυμάτων, καθώς και AMQP.

*    [Διαμερίσματα οντοτήτων ανταλλαγής μηνυμάτων](service-bus-partitioning.md)
*    [ΟΡΓΑΝΙΣΜΌΣ μήνυμα ουράς Protocol (AMQP) έκδοση 1.0 για προχωρημένους](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Υποστήριξη AMQP 1.0 στο Bus υπηρεσίας](service-bus-amqp-overview.md)
*    [1.0 AMQP του οδηγού πρωτόκολλο Bus υπηρεσίας Azure και διανομείς συμβάντος](service-bus-amqp-protocol-guide.md)
*    [Πώς μπορείτε να χρησιμοποιήσετε σας το Java μήνυμα υπηρεσίας (JMS) API με υπηρεσία Bus και AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Πώς μπορείτε να χρησιμοποιήσετε AMQP 1.0 με το API υπηρεσίας Bus .NET](service-bus-dotnet-advanced-message-queuing.md)

[Azure κλασική πύλη]: http://manage.windowsazure.com

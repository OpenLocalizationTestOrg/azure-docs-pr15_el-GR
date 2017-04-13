<properties 
    pageTitle="Πώς μπορείτε να χρησιμοποιήσετε Azure Service Bus με το SDK WebJobs" 
    description="Μάθετε πώς να χρησιμοποιείτε ουρές Bus υπηρεσίας Azure και θέματα με το SDK WebJobs." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Πώς μπορείτε να χρησιμοποιήσετε Azure Service Bus με το SDK WebJobs

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός παρέχει C# δείγματα κώδικα που δείχνουν τον τρόπο ενεργοποίησης μιας διεργασίας κατά τη λήψη ενός μηνύματος Bus υπηρεσίας Azure. Τα δείγματα κώδικα Χρησιμοποιήστε [WebJobs SDK](websites-dotnet-webjobs-sdk.md) έκδοση 1.x.

Στον Οδηγό προϋποθέτει ότι γνωρίζετε [πώς μπορείτε να δημιουργήσετε ένα έργο WebJob στο Visual Studio με συμβολοσειρές σύνδεσης που οδηγούν στο λογαριασμό σας χώρου αποθήκευσης](websites-dotnet-webjobs-sdk-get-started.md).

Τα τμήματα κώδικα Εμφάνιση μόνο των συναρτήσεων, δεν τον κώδικα που δημιουργεί το `JobHost` αντικείμενο όπως αυτό το παράδειγμα:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Είναι ένα [παράδειγμα κώδικα ολοκλήρωσης Bus υπηρεσίας](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) στο αποθετήριο azure-webjobs-sdk-δείγματα στην GitHub.com.

## <a id="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να εργαστείτε με υπηρεσία Bus πρέπει να εγκαταστήσετε το πακέτο NuGet [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) εκτός από τα άλλα πακέτα WebJobs SDK. 

Πρέπει επίσης να ορίσετε τη συμβολοσειρά σύνδεσης AzureWebJobsServiceBus εκτός από το χώρο αποθήκευσης συμβολοσειρές σύνδεσης.  Μπορείτε να το κάνετε το `connectionStrings` ενότητα του αρχείου App.config, όπως φαίνεται στο ακόλουθο παράδειγμα:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Για ένα έργο δείγμα που περιλαμβάνει τη ρύθμιση συμβολοσειρά σύνδεσης υπηρεσίας Bus στο αρχείο App.config, ανατρέξτε στο θέμα [παράδειγμα Bus υπηρεσίας](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Οι συμβολοσειρές σύνδεσης μπορεί επίσης να οριστεί στο περιβάλλον του Azure χρόνου εκτέλεσης, το οποίο, στη συνέχεια, αντικαθιστά τις ρυθμίσεις App.config όταν εκτελείται το WebJob στο Azure; Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το SDK WebJobs](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Τον τρόπο ενεργοποίησης μιας συνάρτησης όταν λαμβάνετε ένα μήνυμα ουρά Bus υπηρεσίας

Για να γράψετε μια συνάρτηση που καλεί το SDK WebJobs όταν λαμβάνετε ένα μήνυμα ουρά, χρησιμοποιήστε το `ServiceBusTrigger` χαρακτηριστικό. Η κατασκευή χαρακτηριστικού παίρνει μια παράμετρο που καθορίζει το όνομα της ουράς προς απάντηση.

### <a name="how-servicebustrigger-works"></a>Πώς λειτουργεί η ServiceBusTrigger

Το SDK λαμβάνει ένα μήνυμα στο `PeekLock` λειτουργία και κλήσεις `Complete` στο μήνυμα, εάν η συνάρτηση ολοκληρωθεί με επιτυχία ή κλήσεις `Abandon` εάν αποτύχει η συνάρτηση. Εάν η συνάρτηση εκτελείται για περισσότερο από το `PeekLock` χρονικό όριο, το κλείδωμα ανανεώνεται αυτόματα.

Υπηρεσία Bus κάνει δικό του χειρισμού αλλοίωσης ουρά που δεν είναι δυνατό να ελέγχεται ή να έχει ρυθμιστεί από το SDK WebJobs. 

### <a name="string-queue-message"></a>Συμβολοσειρά ουρά μηνύματος

Το παρακάτω δείγμα κώδικα διαβάζει ένα μήνυμα ουρά που περιέχει μια συμβολοσειρά και συντάσσει τη συμβολοσειρά στον πίνακα εργαλείων WebJobs SDK.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Σημείωση:** Εάν θέλετε να δημιουργήσετε την ουρά μηνυμάτων σε μια εφαρμογή που δεν χρησιμοποιεί το SDK WebJobs, βεβαιωθείτε ότι για να ορίσετε [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) σε "κείμενο/απλό".

### <a name="poco-queue-message"></a>Μήνυμα ουρά POCO

Το SDK θα αποσειριοποίηση αυτόματα ένα μήνυμα ουρά που περιέχει JSON για μια POCO [(απλό παλιό αντικείμενο CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) τύπου. Το παρακάτω δείγμα κώδικα διαβάζει ένα μήνυμα ουρά που περιέχει ένα `BlobInformation` αντικείμενο το οποίο περιλαμβάνει ένα `BlobName` ιδιότητα:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Για δείγματα κώδικα που εμφανίζει τον τρόπο χρήσης ιδιοτήτων του POCO για να εργαστείτε με αντικείμενα BLOB και πίνακες στο την ίδια συνάρτηση, ανατρέξτε στο θέμα της [αποθήκευσης ουρές έκδοση αυτού του άρθρου](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Εάν σας κώδικα που δημιουργεί το μήνυμα ουρά δεν χρησιμοποιούν το SDK WebJobs, χρησιμοποιήστε κώδικα παρόμοιο με το ακόλουθο παράδειγμα:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Τύποι ServiceBusTrigger λειτουργεί με

Εκτός από `string` και POCO τύπους, μπορείτε να χρησιμοποιήσετε το `ServiceBusTrigger` χαρακτηριστικό με έναν πίνακα byte ή ένα `BrokeredMessage` αντικειμένου.

## <a id="create"></a>Πώς μπορείτε να δημιουργήσετε Bus υπηρεσίας Ουράς μηνυμάτων

Για να γράψετε μια συνάρτηση που δημιουργεί ένα νέο μήνυμα ουρά, χρησιμοποιήστε το `ServiceBus` χαρακτηριστικών και μεταβιβάζουν το όνομα ουρά την κατασκευή χαρακτηριστικού. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Δημιουργήστε ένα μήνυμα μόνο ουρά σε μια συνάρτηση μη ασύγχρονης

Το παρακάτω δείγμα κώδικα χρησιμοποιεί μια παράμετρο εξόδου για να δημιουργήσετε ένα νέο μήνυμα στην ουρά με το όνομα "outputqueue" με το ίδιο περιεχόμενο με το μήνυμα που λαμβάνεται στην ουρά με το όνομα "inputqueue".

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Η παράμετρος εξόδου για τη δημιουργία ενός μηνύματος μόνο ουρά μπορεί να είναι οτιδήποτε από τους παρακάτω τύπους:

* `string`
* `byte[]`
* `BrokeredMessage`
* Δυνατότητα σειριοποίησης τύπος POCO που έχετε καθορίσει. Η σειριοποίηση αυτόματα ως JSON.

Για τις παραμέτρους του τύπου POCO, ένα μήνυμα ουρά δημιουργείται πάντα όταν λήξει η συνάρτηση; Εάν η παράμετρος είναι null, το SDK δημιουργεί ένα μήνυμα ουρά που θα επιστρέψει τιμή null όταν το μήνυμα έχει λάβει και αποσειριοποιηθούν. Για τους άλλους τύπους, εάν η παράμετρος είναι null δημιουργείται κανένα μήνυμα ουρά.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Δημιουργία πολλών μηνυμάτων ουρά ή σε ασύγχρονων συναρτήσεων

Για να δημιουργήσετε πολλά μηνύματα, χρησιμοποιήστε το `ServiceBus` χαρακτηριστικών με `ICollector<T>` ή `IAsyncCollector<T>`, όπως φαίνεται στο ακόλουθο δείγμα κώδικα:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Κάθε μήνυμα ουρά δημιουργείται αμέσως όταν το `Add` μέθοδος καλείται.

## <a id="topics"></a>Πώς μπορείτε να εργαστείτε με τα θέματα Bus υπηρεσίας

Για να γράψετε μια συνάρτηση που καλεί το SDK όταν λαμβάνετε ένα μήνυμα σχετικά με ένα θέμα Bus υπηρεσίας, χρησιμοποιήστε το `ServiceBusTrigger` χαρακτηριστικό με την κατασκευή η οποία λαμβάνει όνομα θέματος και το όνομα της συνδρομής, όπως φαίνεται στο ακόλουθο δείγμα κώδικα:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Για να δημιουργήσετε ένα μήνυμα σχετικά με ένα θέμα, χρησιμοποιήστε το `ServiceBus` χαρακτηριστικό με τον ίδιο τρόπο που χρησιμοποιείτε με ένα όνομα ουρά όνομα θέματος.

## <a name="features-added-in-release-11"></a>Δυνατότητες που έχουν προστεθεί στην έκδοση 1.1

Οι παρακάτω δυνατότητες έχουν προστεθεί στην έκδοση 1.1:

* Δυνατότητα βαθύ προσαρμογής της επεξεργασίας μέσω μηνυμάτων `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`υποστηρίζει προσαρμογής από την υπηρεσία Bus `MessagingFactory` και `NamespaceManager`.
* A `MessageProcessor` μοτίβο στρατηγική σας επιτρέπει να καθορίσετε ένα πρόγραμμα επεξεργασίας ανά ουρά/το θέμα.
* Μήνυμα ταυτόχρονης εκτέλεσης επεξεργασίας υποστηρίζεται από προεπιλογή. 
* Εύκολη προσαρμογή των `OnMessageOptions` μέσω `ServiceBusConfiguration.MessageOptions`.
* Να επιτρέπεται να έχει καθοριστεί σε [Δικαιώματα_προσπέλασης](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (για σενάρια όπου μπορεί να μην έχετε Διαχείριση δικαιωμάτων). 

## <a id="queues"></a>Σχετικά θέματα που καλύπτονται από το άρθρο διαδικασιών ουρές χώρου αποθήκευσης

Για πληροφορίες σχετικά με WebJobs SDK δεν συγκεκριμένα σενάρια για Bus υπηρεσίας, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Θέματα που καλύπτονται σε αυτό το άρθρο περιλαμβάνουν τα εξής:

* Ασύγχρονων συναρτήσεων
* Πολλές παρουσίες
* Φυσιολογική τερματισμού
* Χρήση του WebJobs SDK χαρακτηριστικά στο σώμα μιας συνάρτησης
* Ορίστε τις συμβολοσειρές σύνδεσης SDK στον κώδικα
* Ορισμός τιμών για WebJobs SDK παραμέτρους κατασκευή στον κώδικα
* Ενεργοποίηση μιας συνάρτησης με μη αυτόματο τρόπο
* Γράψτε αρχείων καταγραφής

## <a id="nextsteps"></a>Επόμενα βήματα

Αυτός ο οδηγός έχει παράσχει δείγματα κώδικα που δείχνουν τον τρόπο χειρισμού των συνηθισμένα σενάρια για την εργασία με Bus υπηρεσίας Azure. Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs, ανατρέξτε στο θέμα [Πόροι προτεινόμενα WebJobs Azure](http://go.microsoft.com/fwlink/?linkid=390226).
 

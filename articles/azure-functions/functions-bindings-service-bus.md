<properties
    pageTitle="Azure συναρτήσεις υπηρεσίας Bus εναύσματα και συνδέσεις | Microsoft Azure"
    description="Κατανόηση πώς μπορείτε να χρησιμοποιήσετε Azure Service Bus εναύσματα και συνδέσεις σε συναρτήσεις Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, συμβάν επεξεργασία, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure συναρτήσεις υπηρεσίας Bus εναύσματα και συνδέσεις για ουρές και θέματα

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και κώδικα Azure Service Bus εναύσματα και συνδέσεις σε συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Έναυσμα Bus υπηρεσίας ουράς ή το θέμα

#### <a name="functionjson"></a>Function.JSON

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για την ουρά ή το θέμα ή το μήνυμα ουρά ή το θέμα. 
- `queueName`: Για ουρά έναυσμα μόνο, το όνομα της ουράς προς απάντηση.
- `topicName`: Για το θέμα έναυσμα μόνο, το όνομα του θέματος σε ψηφοφορία.
- `subscriptionName`: Για το θέμα έναυσμα μόνο, το όνομα της συνδρομής.
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης Bus υπηρεσίας. Η συμβολοσειρά σύνδεσης πρέπει να είναι για ένα χώρο ονομάτων Bus υπηρεσίας, δεν περιορίζεται σε μια συγκεκριμένη ουρά ή ένα θέμα. Εάν η συμβολοσειρά σύνδεσης δεν έχουν Διαχείριση δικαιωμάτων, ορίστε το `accessRights` την ιδιότητα. Εάν αφήσετε `connection` κενή, στο έναυσμα ή σύνδεσης θα λειτουργεί με την προεπιλεγμένη υπηρεσία Bus συμβολοσειρά σύνδεσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsServiceBus.
- `accessRights`: Καθορίζει τα δικαιώματα πρόσβασης που είναι διαθέσιμες για τη συμβολοσειρά σύνδεσης. Η προεπιλεγμένη τιμή είναι `manage`. Για να ορίσετε `listen` Εάν χρησιμοποιείτε μια συμβολοσειρά σύνδεσης που δεν παρέχει διαχείριση δικαιωμάτων. Διαφορετικά οι συναρτήσεις χρόνου εκτέλεσης ενδέχεται να δοκιμάσετε και αποτυχίας για να εκτελέσετε λειτουργίες που απαιτούν διαχείριση δικαιωμάτων.
- `type`: Πρέπει να οριστεί στην *serviceBusTrigger*.
- `direction`: Πρέπει να οριστούν *σε*. 

Παράδειγμα *function.json* για ένα έναυσμα ουρά Bus υπηρεσίας:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# παράδειγμα κώδικα που επεξεργάζεται Bus υπηρεσίας ουράς μηνύματος

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # παράδειγμα κώδικα που επεξεργάζεται Bus υπηρεσίας ουράς μηνύματος

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Παράδειγμα κώδικα node.js που επεξεργάζεται Bus υπηρεσίας ουράς μηνύματος

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Υποστηριζόμενοι τύποι

Το μήνυμα Bus υπηρεσίας ουράς μπορεί να αποσειριοποιούνται σε οποιονδήποτε από τους παρακάτω τύπους:

* Αντικείμενο (από JSON)
* συμβολοσειρά
* ο πίνακας byte 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>Συμπεριφορά PeekLock

Το περιβάλλον εκτέλεσης συναρτήσεις λαμβάνει ένα μήνυμα στο `PeekLock` λειτουργία και κλήσεις `Complete` στο μήνυμα, εάν η συνάρτηση ολοκληρωθεί με επιτυχία ή κλήσεις `Abandon` εάν αποτύχει η συνάρτηση. Εάν η συνάρτηση εκτελείται για περισσότερο από το `PeekLock` χρονικό όριο, το κλείδωμα ανανεώνεται αυτόματα.

#### <a id="sbpoison"></a>Χειρισμός μηνυμάτων αλλοίωσης

Υπηρεσία Bus κάνει το δικό της αλλοίωσης χειρισμό που δεν είναι δυνατό να ελέγχεται ή να διαμορφωθεί με ρύθμιση παραμέτρων συναρτήσεις Azure ή τον κωδικό μηνυμάτων. 

#### <a id="sbsinglethread"></a>Τεχνολογία μονό

Από προεπιλογή οι συναρτήσεις χρόνου εκτέλεσης επεξεργάζεται πολλών μηνυμάτων ουρά ταυτόχρονα. Για να κατευθύνετε το περιβάλλον εκτέλεσης για την επεξεργασία μόνο μία ουρά ή το θέμα μήνυμα κάθε φορά, ορίστε `serviceBus.maxConcurrrentCalls` σε 1 στο αρχείο *host.json* . Για πληροφορίες σχετικά με το αρχείο *host.json* , ανατρέξτε στο θέμα [Δομής των φακέλων](functions-reference.md#folder-structure) στο άρθρο αναφοράς προγραμματιστή και [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) στο wiki αποθετήριο WebJobs.Script.

## <a id="sboutput"></a>Υπηρεσία Bus ουρά ή το θέμα εξόδου σύνδεσης

#### <a name="functionjson"></a>Function.JSON

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για την ουρά ή το μήνυμα ουρά. 
- `queueName`: Για ουρά έναυσμα μόνο, το όνομα της ουράς προς απάντηση.
- `topicName`: Για το θέμα έναυσμα μόνο, το όνομα του θέματος σε ψηφοφορία.
- `subscriptionName`: Για το θέμα έναυσμα μόνο, το όνομα της συνδρομής.
- `connection`: Έναυσμα ίδια όπως για Bus υπηρεσίας.
- `accessRights`: Καθορίζει τα δικαιώματα πρόσβασης που είναι διαθέσιμες για τη συμβολοσειρά σύνδεσης. Η προεπιλεγμένη τιμή είναι `manage`. Για να ορίσετε `send` Εάν χρησιμοποιείτε μια συμβολοσειρά σύνδεσης που δεν παρέχει διαχείριση δικαιωμάτων. Διαφορετικά οι συναρτήσεις χρόνου εκτέλεσης ενδέχεται να δοκιμάσετε και αποτυχίας για να εκτελέσετε λειτουργίες που απαιτούν διαχείριση δικαιωμάτων, όπως τη δημιουργία ουρές.
- `type`: Πρέπει να οριστεί στην *serviceBus*.
- `direction`: Πρέπει να οριστεί στην *ανάληψη*. 

Παράδειγμα *function.json* για χρησιμοποιώντας ένα έναυσμα χρονόμετρο για να γράψετε Bus υπηρεσίας Ουράς μηνυμάτων:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Υποστηριζόμενοι τύποι

Συναρτήσεις Azure να δημιουργήσετε ένα μήνυμα ουρά Bus υπηρεσίας από οποιονδήποτε από τους παρακάτω τύπους.

* Αντικείμενο (πάντα δημιουργεί ένα μήνυμα JSON, δημιουργεί το μήνυμα με ένα αντικείμενο null εάν η τιμή είναι μηδέν όταν λήξει η συνάρτηση)
* συμβολοσειρά (δημιουργεί ένα μήνυμα, εάν η τιμή είναι δεν είναι null όταν λήξει η συνάρτηση)
* ο πίνακας byte (λειτουργεί ως συμβολοσειρά) 
* `BrokeredMessage`(C#, λειτουργεί ως συμβολοσειρά)

Για τη δημιουργία πολλών μηνυμάτων σε μια συνάρτηση C#, μπορείτε να χρησιμοποιήσετε `ICollector<T>` ή `IAsyncCollector<T>`. Δημιουργείται ένα μήνυμα όταν καλείτε το `Add` μέθοδο.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# παραδείγματα κώδικα που δημιουργούν Bus υπηρεσίας Ουράς μηνυμάτων

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # παράδειγμα κώδικα που δημιουργεί ένα μήνυμα ουρά Bus υπηρεσίας

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Παράδειγμα κώδικα node.js που δημιουργεί ένα μήνυμα ουρά Bus υπηρεσίας

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

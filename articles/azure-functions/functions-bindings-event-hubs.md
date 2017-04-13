<properties
    pageTitle="Azure συνδέσεις διανομέα συμβάν συναρτήσεις | Microsoft Azure"
    description="Κατανόηση πώς μπορείτε να χρησιμοποιήσετε διανομέα συμβάν Azure συνδέσεις σε συναρτήσεις Azure."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure συνδέσεις διανομέα συμβάν συναρτήσεις

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και συνδέσεις [Διανομέα συμβάν Azure](../event-hubs/event-hubs-overview.md) κώδικα για τις συναρτήσεις Azure. Συναρτήσεις Azure υποστήριξης έναυσμα και εξόδου συνδέσεις για διανομείς συμβάν Azure.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Εάν είστε νέος χρήστης του Azure διανομείς συμβάντος, ανατρέξτε στο θέμα η [Επισκόπηση Azure συμβάν διανομέα](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure διανομέα συμβάν έναυσμα σύνδεσης

Ένα έναυσμα Azure συμβάν διανομέα μπορεί να χρησιμοποιηθεί για να απαντήσετε σε ένα συμβάν που αποστέλλονται σε μια ροή συμβάν διανομέα συμβάν. Πρέπει να έχετε πρόσβαση για ανάγνωση στο διανομέα συμβάντος για να ρυθμίσετε μια σύνδεση ενεργοποίησης.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Function.JSON για το συμβάν διανομέα έναυσμα σύνδεσης

Το αρχείο *function.json* για ένα έναυσμα διανομέα συμβάν Azure καθορίζει τις ακόλουθες ιδιότητες:

- `type`: Πρέπει να οριστεί στην *eventHubTrigger*.
- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το μήνυμα διανομέα συμβάντος. 
- `direction`: Πρέπει να οριστούν *σε*. 
- `path`: Το όνομα του συμβάντος διανομέα.
- `consumerGroup`: Αυτή είναι μια προαιρετική ιδιότητα χρησιμοποιείται για να ορίσετε την [ομάδα καταναλωτή](../event-hubs-overview.md#consumer-groups) χρησιμοποιείται για την εγγραφή σε συμβάντα στην ενότητα. Εάν παραλειφθεί το όρισμα, η `$Default` καταναλωτή της ομάδας. 
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει τη συμβολοσειρά σύνδεσης στο χώρο ονομάτων που βρίσκονται στο διανομέα συμβάντος σε. Αντιγράψτε αυτήν τη συμβολοσειρά σύνδεσης, κάνοντας κλικ στο κουμπί **Πληροφορίες σύνδεσης** για το χώρο ονομάτων, όχι στο διανομέα συμβάν ίδια.  Αυτή η συμβολοσειρά σύνδεσης πρέπει να έχετε τουλάχιστον δικαιώματα ανάγνωσης για την ενεργοποίηση της εργασίας.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Παράδειγμα έναυσμα C# Azure διανομέα συμβάντος
 
Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, στο σώμα του μηνύματος συμβάντος θα καταγραφεί με τη χρήση του κώδικα C# συνάρτηση παρακάτω:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Παράδειγμα έναυσμα F # Azure διανομέα συμβάντος

Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, στο σώμα του μηνύματος συμβάντος θα καταγραφεί με τη χρήση του F # συνάρτηση κώδικα παρακάτω:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure διανομέα συμβάν ενεργοποίησης παράδειγμα Node.js
 
Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, στο σώμα του μηνύματος συμβάντος θα συνδεθείτε χρησιμοποιώντας τον κωδικό συνάρτηση Node.js παρακάτω:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure διανομέα συμβάν εξόδου σύνδεσης

Ένα διανομέα συμβάν Azure σύνδεσης εξόδου χρησιμοποιείται για την εγγραφή συμβάντα μια ροή συμβάν διανομέα συμβάν. Πρέπει να έχετε δικαιώματα αποστολής με ένα διανομέα συμβάντος για να γράψετε συμβάντα σε αυτήν. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Function.JSON για το συμβάν διανομέα εξόδου σύνδεσης

Το αρχείο *function.json* για ένα διανομέα συμβάν Azure εξόδου σύνδεσης καθορίζει τις ακόλουθες ιδιότητες:

- `type`: Πρέπει να οριστεί στην *eventHub*.
- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το μήνυμα διανομέα συμβάντος. 
- `path`: Το όνομα του συμβάντος διανομέα.
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει τη συμβολοσειρά σύνδεσης στο χώρο ονομάτων που βρίσκονται στο διανομέα συμβάντος σε. Αντιγράψτε αυτήν τη συμβολοσειρά σύνδεσης, κάνοντας κλικ στο κουμπί **Πληροφορίες σύνδεσης** για το χώρο ονομάτων, όχι στο διανομέα συμβάν ίδια.  Αυτή η συμβολοσειρά σύνδεσης πρέπει να έχετε δικαιώματα αποστολής για να στείλετε το μήνυμα στη ροή διανομέα συμβάν.
- `direction`: Πρέπει να οριστεί στην *ανάληψη*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure συμβάν διανομέα C# παράδειγμα κώδικα για σύνδεση εξόδου
 
Ο ακόλουθος C# παράδειγμα συνάρτησης κώδικας περιγράφει εγγραφή συμβάντος σε ροή ενός συμβάντος διανομέα συμβάν. Σε αυτό το παράδειγμα αναπαριστά το αποτέλεσμα του συμβάντος διανομέα σύνδεσης φαίνεται παραπάνω εφαρμόζονται σε ένα έναυσμα χρονομέτρησης C#.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure συμβάν διανομέα F # παράδειγμα κώδικα για σύνδεση εξόδου

Ο ακόλουθος F # παράδειγμα συνάρτησης κώδικας περιγράφει εγγραφή συμβάντος σε ροή ενός συμβάντος διανομέα συμβάν. Σε αυτό το παράδειγμα αναπαριστά το αποτέλεσμα του συμβάντος διανομέα σύνδεσης φαίνεται παραπάνω εφαρμόζονται σε ένα έναυσμα χρονόμετρο C#.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure παράδειγμα κώδικα Node.js διανομέα συμβάντων για σύνδεση εξόδου
 
Ο ακόλουθος κώδικας συνάρτηση παράδειγμα Node.js περιγράφει εγγραφή συμβάντος σε ροή ενός συμβάντος διανομέα συμβάν. Σε αυτό το παράδειγμα αναπαριστά το αποτέλεσμα του συμβάντος διανομέα σύνδεσης φαίνεται παραπάνω εφαρμόζονται σε ένα έναυσμα χρονομέτρου Node.js.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

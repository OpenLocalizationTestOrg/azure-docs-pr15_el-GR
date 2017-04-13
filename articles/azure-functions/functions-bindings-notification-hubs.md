<properties
    pageTitle="Azure συναρτήσεις ειδοποίηση διανομέα σύνδεσης | Microsoft Azure"
    description="Να κατανοήσετε τον τρόπο χρήσης του Azure ειδοποίηση διανομέα σύνδεσης σε συναρτήσεις Azure."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure διανομέα ειδοποίηση συναρτήσεις εξόδου σύνδεσης

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και συνδέσεις διανομέα ειδοποίηση Azure κώδικα σε συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Συναρτήσεις σας μπορεί να στείλει τις ειδοποιήσεις push χρησιμοποιείτε διανομέα ρυθμισμένο ειδοποίηση Azure με μερικές γραμμές κώδικα. Ωστόσο, την ενότητα ειδοποίηση Azure πρέπει να ρυθμιστεί για την πλατφόρμα ειδοποιήσεις υπηρεσιών (Γραμματίων) που θέλετε να χρησιμοποιήσετε. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενός διανομέα ειδοποίηση Azure και την ανάπτυξη μιας εφαρμογές προγράμματος-πελάτη που καταχώρηση για να λαμβάνετε ειδοποιήσεις, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με την ειδοποίηση διανομείς](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) και κάντε κλικ την πλατφόρμα προγράμματος-πελάτη σας προορισμού στο επάνω μέρος.

Τις ειδοποιήσεις που στέλνετε μπορεί να είναι εγγενής ειδοποιήσεις ή πρότυπο ειδοποιήσεις. Εγγενής ειδοποιήσεις προορισμού μια συγκεκριμένη γνωστοποίηση πλατφόρμα όπως έχει ρυθμιστεί στο το `platform` ιδιότητα της σύνδεσης εξόδου. Μια ειδοποίηση πρότυπο μπορεί να χρησιμοποιηθεί για τη στόχευση πολλές πλατφόρμες.   

## <a name="notification-hub-output-binding-properties"></a>Ιδιότητες σύνδεσης εξόδου διανομέα ειδοποίησης

Το αρχείο function.json παρέχει τις ακόλουθες ιδιότητες:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το μήνυμα ειδοποίησης διανομέα.
- `type`: πρέπει να οριστεί σε *"notificationHub"*.
- `tagExpression`: Παραστάσεις ετικέτα σάς επιτρέπουν να καθορίσετε ότι οι ειδοποιήσεις θα παραδίδονται σε ένα σύνολο από συσκευές που έχετε καταχωρήσει για να λαμβάνετε ειδοποιήσεις που ταιριάζουν με την έκφραση ετικέτας.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δρομολόγηση και ετικέτα παραστάσεις](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Το όνομα του πόρου διανομέα ειδοποίησης στην πύλη του Azure.
- `connection`: Αυτήν τη συμβολοσειρά σύνδεσης πρέπει να είναι μια συμβολοσειρά σύνδεσης **Ρύθμιση εφαρμογής** οριστεί στην τιμή *DefaultFullSharedAccessSignature* για το Κέντρο ειδοποίηση σας.
- `direction`: πρέπει να οριστεί σε *"θέμα"*. 
- `platform`: Η ιδιότητα πλατφόρμα υποδεικνύει την πλατφόρμα ειδοποίηση σας προορισμών ειδοποίησης. Πρέπει να είναι μία από τις παρακάτω τιμές:
    - `template`: Αυτή είναι η προεπιλεγμένη πλατφόρμα εάν η ιδιότητα πλατφόρμα παραλειφθεί από τη σύνδεση εξόδου. Ειδοποιήσεις πρότυπο μπορεί να χρησιμοποιηθεί για προορισμού οποιαδήποτε πλατφόρμα έχει ρυθμιστεί στην ενότητα ειδοποίηση Azure. Για περισσότερες πληροφορίες σχετικά με τη χρήση προτύπων γενικά για την αποστολή ειδοποιήσεων διασταύρωσης πλατφόρμα με ένα διανομέα ειδοποίηση Azure, ανατρέξτε στο θέμα [πρότυπα](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Υπηρεσία ειδοποιήσεων Push Apple. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενότητα ειδοποίησης για APNS και λαμβάνει την ειδοποίηση σε μια εφαρμογή προγράμματος-πελάτη, ανατρέξτε στο θέμα [Αποστολή ειδοποιήσεων push για iOS με διανομείς ειδοποίηση Azure](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Συσκευή Amazon μηνυμάτων](https://developer.amazon.com/device-messaging). Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενότητα ειδοποίησης για ADM και λαμβάνει την ειδοποίηση σε μια εφαρμογή Kindle, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με διανομείς ειδοποίησης για τις εφαρμογές Kindle](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud μηνυμάτων](https://developers.google.com/cloud-messaging/). Firebase Cloud μηνυμάτων, που είναι η νέα έκδοση του GCM, υποστηρίζεται επίσης. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση ενότητα ειδοποίησης για GCM/FCM και λαμβάνει την ειδοποίηση σε μια εφαρμογή προγράμματος-πελάτη Android, ανατρέξτε στην ενότητα [Αποστολή ειδοποιήσεων push για Android με διανομείς ειδοποίηση Azure](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Υπηρεσίες ειδοποιήσεων Push Windows](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) στόχευσης πλατφόρμες των Windows. Windows Phone 8.1 και νεότερες εκδόσεις υποστηρίζεται επίσης WNS. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενότητα ειδοποίησης για WNS και λαμβάνει την ειδοποίηση σε μια εφαρμογή για καθολική πλατφόρμας των Windows (UWP), ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με την ειδοποίηση διανομείς για Windows καθολικής πλατφόρμα εφαρμογές](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Υπηρεσία ειδοποιήσεων Push της Microsoft](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Αυτή η πλατφόρμα υποστηρίζει το Windows Phone 8 και παλαιότερες πλατφόρμες Windows Phone. Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενότητα ειδοποίησης για MPNS και λαμβάνει την ειδοποίηση σε μια εφαρμογή για Windows Phone, ανατρέξτε στο θέμα [Αποστολή ειδοποιήσεων push με διανομείς ειδοποίηση Azure σε Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Παράδειγμα function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Ρύθμιση συμβολοσειρά σύνδεσης διανομέα ειδοποίησης

Για να χρησιμοποιήσετε ένα αποτέλεσμα διανομέα ειδοποίηση βιβλιοδεσία, πρέπει να ρυθμίσετε τη συμβολοσειρά σύνδεσης για την ενότητα. Αυτό μπορεί να γίνει στην καρτέλα *ενσωματώσουν* , επιλέγοντας το Κέντρο ειδοποίηση σας ή τη δημιουργία ενός νέου. 

Μπορείτε να προσθέσετε επίσης με μη αυτόματο τρόπο μια συμβολοσειρά σύνδεσης για μια υπάρχουσα ενότητα, προσθέτοντας μια συμβολοσειρά σύνδεσης για το *DefaultFullSharedAccessSignature* στο κέντρο σας ειδοποίηση. Αυτή η συμβολοσειρά σύνδεσης παρέχει το δικαίωμα πρόσβασης συνάρτηση για να στείλετε μηνύματα ειδοποιήσεων. Η τιμή της συμβολοσειράς σύνδεσης *DefaultFullSharedAccessSignature* είναι δυνατή η πρόσβαση από το κουμπί **πλήκτρα** στο το κύριο blade του πόρου διανομέα ειδοποίησης στην πύλη του Azure. Για να προσθέσετε με μη αυτόματο τρόπο μια συμβολοσειρά σύνδεσης για το Κέντρο σας, χρησιμοποιήστε τα ακόλουθα βήματα: 

1. Στην την **εφαρμογή συνάρτηση** blade της πύλης Azure, κάντε κλικ στην επιλογή **των ρυθμίσεων της εφαρμογής συνάρτηση > Μετάβαση στις ρυθμίσεις εφαρμογής υπηρεσίας**.

2. Στο το blade **Ρυθμίσεις** , κάντε κλικ στην επιλογή **Ρυθμίσεις εφαρμογής**.

3. Κάντε κύλιση προς τα κάτω στην ενότητα **συμβολοσειρές σύνδεσης** και προσθέστε μια καθορισμένη καταχώρηση για το *DefaultFullSharedAccessSignature* τιμή για το Κέντρο ειδοποίηση σας. Αλλαγή του τύπου σε **προσαρμοσμένη**.
4. Αναφορά ονόματος συμβολοσειρά σύνδεσης στο τις συνδέσεις εξόδου. Παρόμοια με **MyHubConnectionString** χρησιμοποιούνται στο παραπάνω παράδειγμα.

## <a name="native-notification-examples"></a>Παραδείγματα εγγενών ειδοποίησης

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>Ειδοποιήσεις εγγενούς APNS με εναύσματα ουρά C#

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τύπους που ορίζονται από το [Microsoft Azure ειδοποίηση διανομείς βιβλιοθήκη](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) για να στείλετε μια ειδοποίηση εγγενούς APNS. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>Ειδοποιήσεις εγγενής GCM με εναύσματα ουρά C#

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τύπους που ορίζονται από το [Microsoft Azure ειδοποίηση διανομείς βιβλιοθήκη](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) για να στείλετε μια ειδοποίηση GCM εγγενή. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>Ειδοποιήσεις εγγενούς WNS με εναύσματα ουρά C#

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τύπους που ορίζονται από το [Microsoft Azure ειδοποίηση διανομείς βιβλιοθήκη](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) για να στείλετε μια εγγενή WNS αναδυόμενη ειδοποίηση. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Παραδείγματα ειδοποίηση προτύπου

#### <a name="template-example-for-nodejs-timer-triggers"></a>Παράδειγμα προτύπου για Node.js χρονόμετρο εναύσματα 

Αυτό το παράδειγμα στέλνει μια ειδοποίηση για ένα [πρότυπο δήλωσης](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) που περιέχει `location` και `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>Παράδειγμα προτύπου για εναύσματα χρονόμετρο F #

Αυτό το παράδειγμα στέλνει μια ειδοποίηση για ένα [πρότυπο δήλωσης](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) που περιέχει `location` και `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Παράδειγμα προτύπου, χρησιμοποιώντας μια παράμετρος out 

Αυτό το παράδειγμα στέλνει μια ειδοποίηση για ένα [πρότυπο δήλωσης](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) που περιέχει ένα `message` κράτησης θέσης στο πρότυπο.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Παράδειγμα προτύπου με ασύγχρονης συνάρτηση

Εάν χρησιμοποιείτε ασύγχρονης κώδικα, παράμετροι εξόδου δεν επιτρέπεται. Σε αυτήν την περίπτωση χρησιμοποιείτε `IAsyncCollector` για να επιστρέψει την ειδοποίηση πρότυπο. Ο κώδικας που ακολουθεί είναι ένα παράδειγμα ασύγχρονης του κώδικα παραπάνω. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>Παράδειγμα προτύπου χρήση JSON 

Αυτό το παράδειγμα στέλνει μια ειδοποίηση για ένα [πρότυπο δήλωσης](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) που περιέχει ένα `message` κράτησης θέσης στο πρότυπο χρησιμοποιώντας μια έγκυρη συμβολοσειρά JSON.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Παράδειγμα προτύπου χρήση ειδοποιήσεων διανομείς βιβλιοθήκης τύπων

Αυτό το παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε τύπους που ορίζονται από το [Microsoft Azure ειδοποίηση διανομείς βιβλιοθήκη](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

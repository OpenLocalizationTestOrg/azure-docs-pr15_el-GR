<properties
    pageTitle="Σύνδεση Azure συναρτήσεις Twilio | Microsoft Azure"
    description="Για να κατανοήσετε τον τρόπο χρήσης του Twilio συνδέσεων με συναρτήσεις Azure."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Twilio συναρτήσεις εξόδου σύνδεσης

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να χρησιμοποιήσετε Twilio συνδέσεων με συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure υποστηρίζει συναρτήσεις Twilio εξόδου συνδέσεις για να ενεργοποιήσετε τις συναρτήσεις για την αποστολή μηνυμάτων κειμένου SMS με μερικές γραμμές κώδικα και ένα λογαριασμό [Twilio](https://www.twilio.com/) . 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Function.JSON για διανομέα ειδοποίηση Azure εξόδου σύνδεσης

Το αρχείο function.json παρέχει τις ακόλουθες ιδιότητες:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το μήνυμα κειμένου Twilio SMS.
- `type`: πρέπει να οριστεί σε *"twilioSms"*.
- `accountSid`: Αυτή η τιμή πρέπει να οριστεί στο όνομα του μια ρύθμιση εφαρμογής που περιέχει το αναγνωριστικό ασφαλείας λογαριασμού Twilio.
- `authToken`: Αυτή η τιμή πρέπει να οριστεί στο όνομα του μια ρύθμιση εφαρμογής που διατηρεί το διακριτικό Twilio ελέγχου ταυτότητας.
- `to`: Αυτή η τιμή έχει οριστεί σε τον αριθμό τηλεφώνου που αποστέλλεται κειμένου SMS.
- `from`: Αυτή η τιμή έχει οριστεί σε τον αριθμό τηλεφώνου που αποστέλλεται από το κείμενο SMS.
- `direction`: πρέπει να οριστεί σε *"θέμα"*.
- `body`: Αυτή η τιμή μπορεί να χρησιμοποιηθεί σε κώδικα οριστική το μήνυμα κειμένου SMS, εάν δεν χρειάζεται να ορίσετε δυναμικά στον κώδικα της συνάρτησης σας. 

 
Παράδειγμα function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Παράδειγμα C# ουρά έναυσμα με Twilio εξόδου σύνδεσης

#### <a name="synchronous"></a>Σύγχρονη

Αυτό σύγχρονη παράδειγμα κώδικα για ένα έναυσμα ουρά αποθήκευσης Azure χρησιμοποιεί μια παράμετρος out για να στείλετε ένα μήνυμα κειμένου σε πελάτη που παραγγελίας.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Ασύγχρονη

Αυτό ασύγχρονης παράδειγμα κώδικα για ένα έναυσμα ουρά αποθήκευσης Azure στέλνει ένα μήνυμα κειμένου σε πελάτη που παραγγελίας.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Παράδειγμα Node.js ουρά έναυσμα με Twilio εξόδου σύνδεσης

Αυτό το παράδειγμα Node.js στέλνει ένα μήνυμα κειμένου σε πελάτη που παραγγελίας.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

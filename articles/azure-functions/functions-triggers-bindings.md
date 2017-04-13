<properties
    pageTitle="Azure συναρτήσεις εναύσματα και συνδέσεις | Microsoft Azure"
    description="Κατανόηση πώς μπορείτε να χρησιμοποιήσετε εναύσματα και συνδέσεις σε συναρτήσεις Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, Επεξεργασία συμβάντος, webhooks, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure συναρτήσεις εναύσματα και συνδέσεις αναφορά προγραμματιστών


Αυτό το θέμα παρέχει γενικές αναφοράς για εναύσματα και συνδέσεις. Περιλαμβάνει ορισμένες από τις δυνατότητες για προχωρημένους σύνδεσης και σύνταξη που υποστηρίζονται από όλους τους τύπους σύνδεσης.  

Εάν αναζητάτε για λεπτομερείς πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων και κωδικοποίησης ένα συγκεκριμένο τύπο έναυσμα ή σύνδεσης, ίσως θελήσετε να κάνετε κλικ σε μία από τις έναυσμα ή συνδέσεις που παρατίθενται παρακάτω:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Αυτά τα άρθρα λαμβάνεται ως δεδομένο ότι έχετε διαβάσει την [αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md)και τα άρθρα αναφοράς προγραμματιστή [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)ή [Node.js](functions-reference-node.md) .



## <a name="overview"></a>Επισκόπηση

Τα εναύσματα είναι συμβάν απαντήσεων που χρησιμοποιείται για την ενεργοποίηση του προσαρμοσμένου κώδικα. Σας επιτρέπουν να αποκρίνεται σε συμβάντα κατά μήκος της πλατφόρμας Azure ή στην εσωτερική εγκατάσταση. Συνδέσεις αντιπροσωπεύουν τα απαραίτητα μετα-δεδομένα που χρησιμοποιούνται για τη σύνδεση του κώδικα στην επιθυμητή έναυσμα ή σχετικές εισαγωγής ή δεδομένα εξόδου.

Για να λάβετε μια καλύτερη ιδέα για τις διαφορετικές συνδέσεις που μπορείτε να ενοποιήσετε με την εφαρμογή σας Azure συνάρτηση, ανατρέξτε στον παρακάτω πίνακα.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Για να κατανοήσετε καλύτερα ενεργοποιεί και συνδέσεις σε γενικές γραμμές, ας υποθέσουμε ότι θέλετε να εκτελέσετε ορισμένες κώδικα στη διαδικασία αποτεθεί ένα νέο στοιχείο σε ένα χώρο αποθήκευσης Azure ουρά. Συναρτήσεις Azure παρέχει ένα έναυσμα ουρά Azure για να υποστηρίξει αυτήν. Θα πρέπει, οι ακόλουθες πληροφορίες για την παρακολούθηση της ουρά:
 
- Ο λογαριασμός χώρου αποθήκευσης όπου υπάρχει η ουρά.
- Το όνομα της ουράς.
- Ένα όνομα μεταβλητής που θα χρησιμοποιούν τον κωδικό να αναφέρεται το νέο στοιχείο που απορρίφθηκε στην ουρά.  
 
Ένα έναυσμα ουρά σύνδεση περιέχει αυτές τις πληροφορίες για μια συνάρτηση Azure. Το αρχείο *function.json* για κάθε συνάρτηση περιέχει όλες τις σχετικές συνδέσεις.  Ακολουθεί ένα παράδειγμα *function.json* που περιέχει μια ουρά έναυσμα σύνδεσης. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Ο κώδικας μπορεί να στείλει διαφορετικούς τύπους εξόδου ανάλογα με τον τρόπο επεξεργασίας το νέο στοιχείο ουρά. Για παράδειγμα, μπορεί να θέλετε να συντάξετε μια νέα εγγραφή σε έναν πίνακα αποθήκευσης Azure.  Για να γίνει αυτό, μπορείτε να ρυθμίσετε μια σύνδεση εξόδου σε έναν πίνακα αποθήκευσης Azure. Ακολουθεί ένα παράδειγμα *function.json* που περιλαμβάνει μια σύνδεση εξόδου πίνακα χώρου αποθήκευσης που θα μπορούσαν να χρησιμοποιηθούν με ένα έναυσμα ουρά. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Η ακόλουθη συνάρτηση C# αποκρίνεται σε ένα νέο στοιχείο απόρριψης στην ουρά και εγγράφει μια νέα καταχώρηση χρήστη σε έναν πίνακα αποθήκευσης Azure.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Για περισσότερα παραδείγματα κώδικα και πιο συγκεκριμένες πληροφορίες σχετικά με τους τύπους Azure χώρου αποθήκευσης που υποστηρίζονται, ανατρέξτε στο θέμα [συναρτήσεις Azure εναύσματα και συνδέσεις για το χώρο αποθήκευσης Azure](functions-bindings-storage.md).


Για να χρησιμοποιήσετε τις πιο σύνθετες δυνατότητες σύνδεσης στην πύλη του Azure, κάντε κλικ στην επιλογή **πρόγραμμα επεξεργασίας για προχωρημένους** στην καρτέλα **ενσωματώσουν** της συνάρτησης σας. Πρόγραμμα επεξεργασίας για προχωρημένους σάς επιτρέπει να επεξεργαστείτε το *function.json* απευθείας στην πύλη του.

## <a name="dynamic-parameter-binding"></a>Παράμετρος δυναμικής σύνδεσης 

Αντί για ρύθμιση παραμέτρων για στατική τη ρύθμιση για τις ιδιότητες σύνδεσης εξόδου, μπορείτε να ρυθμίσετε τις παραμέτρους δυναμικά να συνδεθεί με τα δεδομένα που είναι μέρος του εναύσματος σας σύνδεση εισόδου. Εξετάστε το ενδεχόμενο να ένα σενάριο όπου νέες παραγγελίες σε αυτές χρησιμοποιώντας μια ουρά αποθήκευσης Azure. Κάθε νέο στοιχείο ουρά είναι μια συμβολοσειρά JSON που περιέχει τουλάχιστον τις ακόλουθες ιδιότητες:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Ενδέχεται να θέλετε να στείλετε ένα μήνυμα κειμένου SMS χρησιμοποιώντας το λογαριασμό σας Twilio ως ενημερωμένη έκδοση που παραλήφθηκε τη σειρά στον πελάτη.  Μπορείτε να ρυθμίσετε το `body` και `to` πεδίο σας Twilio εξόδου σύνδεσης σε δυναμικό είναι συνδεδεμένο με το `name` και `mobileNumber` που έχουν ως εξής τμήμα της εισόδου.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Τώρα τον κωδικό συνάρτηση έχει μόνο για την προετοιμασία η παράμετρος εξόδου ως εξής. Κατά την εκτέλεση των ιδιοτήτων εξόδου θα είναι συνδεδεμένο με τα δεδομένα εισαγωγής που θέλετε.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Τυχαίες GUID

Συναρτήσεις Azure παρέχει σύνταξη για τη δημιουργία τυχαίων GUID με τις συνδέσεις. Την ακόλουθη σύνταξη σύνδεσης θα εγγραφή εξόδου σε ένα νέο BLOB με ένα μοναδικό όνομα σε ένα χώρο αποθήκευσης Azure κοντέινερ: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Επιστρέφει ένα μεμονωμένο αποτέλεσμα

Στις περιπτώσεις όπου τον κωδικό συνάρτηση επιστρέφει ένα μεμονωμένο αποτέλεσμα, μπορείτε να χρησιμοποιήσετε μια σύνδεση εξόδου με όνομα `$return` για να διατηρήσετε μια πιο φυσική υπογραφή συνάρτησης στον κώδικά σας. Αυτό μπορεί να χρησιμοποιηθεί μόνο με τις γλώσσες που υποστηρίζουν τιμή επιστροφής (C#, Node.js, F #). Η σύνδεση είναι παρόμοια με την εξής σύνδεση εξόδου blob που χρησιμοποιείται με ένα έναυσμα ουρά.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Ο ακόλουθος κώδικας C# επιστρέφει το αποτέλεσμα του πιο φυσικά χωρίς τη χρήση μιας `out` παράμετρος στη συνάρτηση υπογραφής.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Αυτή η προσέγγιση είναι επίδειξη κάτω από το στοιχείο με Node.js.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # που παρέχονται παρακάτω παράδειγμα.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Αυτό μπορεί επίσης να χρησιμοποιηθεί με πολλές παραμέτρους εξόδου ορίζοντας ένα μεμονωμένο αποτέλεσμα με `$return`.


## <a name="route-support"></a>Δρομολόγηση υποστήριξης

Από προεπιλογή όταν δημιουργείτε μια συνάρτηση για ένα έναυσμα HTTP ή WebHook, η συνάρτηση είναι μπορούν να χρησιμοποιηθούν με μια διαδρομή της φόρμας:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Μπορείτε να προσαρμόσετε αυτήν τη μέθοδο χρησιμοποιώντας την προαιρετική `route` ιδιότητας το έναυσμα HTTP εισόδου της σύνδεσης. Ως παράδειγμα, το παρακάτω αρχείο *function.json* ορίζει ένα `route` ιδιότητας για ένα έναυσμα HTTP:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Αυτή η ρύθμιση παραμέτρων, η συνάρτηση είναι τώρα μπορούν να χρησιμοποιηθούν με την εξής δρομολόγηση αντί για την αρχική δρομολόγηση.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Αυτό σας επιτρέπει τη συνάρτηση κώδικα για την υποστήριξη δύο παραμέτρους στη διεύθυνση `category` και `id`. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε [Περιορισμού δρομολόγηση API Web](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) με τις παραμέτρους. Ο ακόλουθος κώδικας συνάρτηση C# κάνει χρήση και τις δύο παραμέτρους.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Παρακάτω θα δείτε Node.js συνάρτηση κώδικα ώστε να χρησιμοποιεί τις ίδιες παραμέτρους δρομολόγησης.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Από προεπιλογή, όλες οι διαδρομές συνάρτηση είναι το πρόθεμα *api*. Μπορείτε επίσης να προσαρμόσετε ή να καταργήσετε το πρόθεμα χρησιμοποιώντας το `http.routePrefix` ιδιότητα στο αρχείο σας *host.json* . Το παρακάτω παράδειγμα καταργεί το πρόθεμα δρομολόγησης *api* , χρησιμοποιώντας μια κενή συμβολοσειρά για το πρόθεμα στο αρχείο *host.json* .

    {
      "http": {
        "routePrefix": ""
      }
    }

Για λεπτομερείς πληροφορίες σχετικά με τον τρόπο για να ενημερώσετε το αρχείο *host.json* για σας συνάρτηση, ανατρέξτε στο θέμα [Τρόπος για να ενημερώσετε συνάρτηση εφαρμογή αρχείων](functions-reference.md#fileupdate). 

Με την προσθήκη αυτής της ρύθμισης παραμέτρων, η συνάρτηση είναι τώρα μπορούν να χρησιμοποιηθούν με τη δρομολόγηση παρακάτω:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Για πληροφορίες σχετικά με άλλες ιδιότητες που μπορείτε να ρυθμίσετε στο αρχείο σας *host.json* , ανατρέξτε στο θέμα [αναφορά host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Δοκιμή μια συνάρτηση](functions-test-a-function.md)
* [Κλίμακα μια συνάρτηση](functions-scale.md)

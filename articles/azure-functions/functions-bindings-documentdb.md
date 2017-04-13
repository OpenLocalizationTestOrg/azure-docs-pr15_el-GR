<properties
    pageTitle="Azure συνδέσεις συναρτήσεις DocumentDB | Microsoft Azure"
    description="Να κατανοήσετε τον τρόπο χρήσης του Azure DocumentDB συνδέσεις σε συναρτήσεις Azure."
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

# <a name="azure-functions-documentdb-bindings"></a>Azure συνδέσεις DocumentDB συναρτήσεις

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και συνδέσεις Azure DocumentDB κώδικα σε συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB εισαγωγής σύνδεσης

Συνδέσεις εισόδου μπορεί να φορτώσετε ένα έγγραφο από μια συλλογή DocumentDB και μεταβιβάζουν απευθείας σύνδεσή σας. Το αναγνωριστικό εγγράφου μπορεί να καθοριστεί με βάση το έναυσμα που κλήση της συνάρτησης. Σε μια συνάρτηση C#, τυχόν αλλαγές που κάνατε στην εγγραφή θα αυτόματα αποστέλλονται πίσω στη συλλογή όταν η συνάρτηση έξοδο με επιτυχία.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON για σύνδεση εισόδου DocumentDB

Το αρχείο *function.json* παρέχει τις ακόλουθες ιδιότητες:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το έγγραφο.
- `type`: πρέπει να οριστεί σε "documentdb".
- `databaseName`: Η βάση δεδομένων που περιέχει το έγγραφο.
- `collectionName`: Η συλλογή που περιέχει το έγγραφο.
- `id`: Το αναγνωριστικό του εγγράφου για να ανακτήσετε. Αυτή η ιδιότητα υποστηρίζει συνδέσεις παρόμοιο με το "{queueTrigger}", που θα χρησιμοποιήσει την τιμή συμβολοσειράς του μηνύματος ουρά με το έγγραφο αναγνωριστικό.
- `connection`: Αυτή η συμβολοσειρά πρέπει να είναι ένα σύνολο ρύθμιση εφαρμογής για το τελικό σημείο για το λογαριασμό σας DocumentDB. Εάν επιλέξετε το λογαριασμό σας από την καρτέλα ενσωματώσουν, μια νέα εφαρμογή ρύθμιση θα δημιουργηθεί για εσάς με όνομα που έχει την παρακάτω μορφή, yourAccount_DOCUMENTDB. Εάν χρειάζεστε για να δημιουργήσετε με μη αυτόματο τρόπο τη ρύθμιση της εφαρμογής, τη συμβολοσειρά σύνδεσης πραγματική πρέπει να έχει την ακόλουθη μορφή, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- ' κατεύθυνση: πρέπει να οριστεί σε *"σε"*.

Παράδειγμα *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure παράδειγμα εισαγωγής κώδικα DocumentDB για ένα έναυσμα ουρά C#
 
Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, τη σύνδεση εισόδου DocumentDB θα ανακτήσει το έγγραφο με το αναγνωριστικό που ταιριάζει με τη συμβολοσειρά μήνυμα ουρά και να μεταβιβάζουν την παράμετρο 'εγγράφου'. Εάν δεν εντοπιστεί αυτό το έγγραφο, η παράμετρος 'εγγράφου' θα είναι null. Το έγγραφο έχει ενημερωθεί με τη νέα τιμή κειμένου, στη συνέχεια, όταν βγει από τη συνάρτηση.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure παράδειγμα εισαγωγής κώδικα DocumentDB για ένα έναυσμα ουρά F #

Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, τη σύνδεση εισόδου DocumentDB θα ανακτήσετε το έγγραφο με το αναγνωριστικό που ταιριάζει με τη συμβολοσειρά μήνυμα ουρά και να μεταβιβάζουν την παράμετρο 'εγγράφου'. Εάν δεν εντοπιστεί αυτό το έγγραφο, η παράμετρος 'εγγράφου' θα είναι null. Το έγγραφο έχει ενημερωθεί με τη νέα τιμή κειμένου, στη συνέχεια, όταν βγει από τη συνάρτηση.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Θα χρειαστείτε μια `project.json` αρχείο που χρησιμοποιεί NuGet για να καθορίσετε το `FSharp.Interop.Dynamic` και `Dynamitey` πακέτων ως πακέτο εξαρτήσεις, ως εξής:

    {
      "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
          }
        }
      }
    }

Αυτό θα χρησιμοποιήσετε NuGet για τη λήψη του εξαρτήσεις και θα τους αναφορά στη δέσμη ενεργειών σας.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure παράδειγμα εισαγωγής κώδικα DocumentDB για ένα έναυσμα ουρά Node.js
 
Χρησιμοποιώντας το παραπάνω παράδειγμα function.json, τη σύνδεση εισόδου DocumentDB θα ανακτήσει το έγγραφο με το αναγνωριστικό που ταιριάζει με τη συμβολοσειρά μήνυμα ουρά και μεταβιβάζουν για να το `documentIn` ιδιότητα σύνδεσης. Στις συναρτήσεις Node.js, ενημερωμένα έγγραφα δεν αποστέλλονται πίσω στη συλλογή. Ωστόσο, μπορείτε να περάσετε τη σύνδεση εισόδου απευθείας σε ένα αποτέλεσμα DocumentDB σύνδεση με όνομα `documentOut` για την υποστήριξη ενημερώσεις. Αυτό το παράδειγμα κώδικα ενημερώνει την ιδιότητα κείμενο του εγγράφου εισαγωγής και ορίζει με το έγγραφο εξόδου.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB εξόδου συνδέσεις

Συναρτήσεις σας να γράψετε JSON έγγραφα σε μια βάση δεδομένων Azure DocumentDB χρησιμοποιώντας το **Έγγραφο DocumentDB Azure** εξόδου σύνδεσης. Για περισσότερες πληροφορίες σχετικά με Azure DocumentDB Αναθεωρήστε την [Εισαγωγή στις DocumentDB](../documentdb/documentdb-introduction.md) και το [πρόγραμμα εκμάθησης που ξεκινήσατε](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON για DocumentDB εξόδου σύνδεσης

Το αρχείο function.json παρέχει τις ακόλουθες ιδιότητες:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το νέο έγγραφο.
- `type`: πρέπει να οριστεί σε *"documentdb"*.
- `databaseName`: Η βάση δεδομένων που περιέχει τη συλλογή όπου θα δημιουργηθεί το νέο έγγραφο.
- `collectionName`: Η συλλογή όπου θα δημιουργηθεί το νέο έγγραφο.
- `createIfNotExists`: Αυτή είναι μια τιμή boolean για να υποδείξετε αν θα δημιουργηθεί η συλλογή, αν δεν υπάρχει. Η προεπιλογή είναι *false*. Ο λόγος για αυτό το νέο συλλογές δημιουργούνται με δεσμευμένες μεταγωγή, που έχει τις τιμές συνέπειες. Για περισσότερες λεπτομέρειες, επισκεφθείτε την [Τιμολόγηση σελίδας](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Αυτή η συμβολοσειρά πρέπει να ορίσετε μια **Ρύθμιση εφαρμογής** για το τελικό σημείο για το λογαριασμό σας DocumentDB. Εάν επιλέξετε το λογαριασμό σας από την καρτέλα **ενσωματώσουν** , μια νέα εφαρμογή ρύθμιση θα δημιουργηθεί για εσάς με όνομα που λαμβάνει την ακόλουθη φόρμα, `yourAccount_DOCUMENTDB`. Εάν χρειάζεστε για να δημιουργήσετε με μη αυτόματο τρόπο τη ρύθμιση της εφαρμογής, τη συμβολοσειρά σύνδεσης πραγματική πρέπει να έχει την ακόλουθη μορφή, `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: πρέπει να οριστεί σε *"θέμα"*. 
 
Παράδειγμα function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Παράδειγμα κώδικα εξόδου Azure DocumentDB για ένα έναυσμα ουρά Node.js

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Το έγγραφο εξόδου:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Παράδειγμα κώδικα εξόδου Azure DocumentDB για ένα έναυσμα ουρά F #

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Παράδειγμα κώδικα εξόδου Azure DocumentDB για ένα έναυσμα ουρά C#


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB εξόδου κώδικα παράδειγμα ρύθμιση ονόματος αρχείου

Εάν θέλετε να ορίσετε το όνομα του εγγράφου στη συνάρτηση, απλώς ορίστε το `id` τιμή.  Για παράδειγμα, εάν JSON περιεχομένου για έναν εργαζόμενο καταργήθηκε στην ουρά παρόμοιο με το εξής:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Μπορείτε να χρησιμοποιήσετε τον παρακάτω κώδικα C# σε μια συνάρτηση έναυσμα ουρά: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Ή η ισοδύναμη κώδικα F #:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Παράδειγμα εξόδου:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

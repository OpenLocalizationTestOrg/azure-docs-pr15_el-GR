<properties
    pageTitle="Αναφορά προγραμματιστών Azure συναρτήσεις NodeJS | Microsoft Azure"
    description="Για να κατανοήσετε τον τρόπο ανάπτυξης συναρτήσεις Azure χρησιμοποιώντας NodeJS."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, Επεξεργασία συμβάντος, webhooks, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Αναφορά προγραμματιστών Azure NodeJS συναρτήσεις

> [AZURE.SELECTOR]
- [C# δέσμης ενεργειών](../articles/azure-functions/functions-reference-csharp.md)
- [F # δέσμης ενεργειών](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Η εμπειρία κόμβο/JavaScript για τις συναρτήσεις Azure διευκολύνει την εξαγωγή μιας συνάρτησης που εκτελείται από έναν `context` αντικειμένου για την επικοινωνία με το περιβάλλον εκτέλεσης, καθώς και για τη λήψη και την αποστολή δεδομένων μέσω συνδέσεων.

Σε αυτό το άρθρο προϋποθέτει ότι έχετε διαβάσει ήδη την [αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md).

## <a name="exporting-a-function"></a>Εξαγωγή μιας συνάρτησης

Όλες τις συναρτήσεις JavaScript πρέπει να εξαγάγετε ένα μόνο `function` μέσω `module.exports` για το περιβάλλον εκτέλεσης για να βρείτε τη συνάρτηση και εκτελέστε το. Αυτή η συνάρτηση πάντα πρέπει να συμπεριλάβετε μια `context` αντικειμένου.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Συνδέσεις των `direction === "in"` μεταβιβάζονται κατά μήκος ως ορίσματα συνάρτησης, γεγονός που σημαίνει ότι μπορείτε να χρησιμοποιήσετε [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) δυναμικά χειρισμού νέα δεδομένα εισόδου (για παράδειγμα, με τη χρήση `arguments.length` διαδοχικές προσεγγίσεις μέσω όλα τα δεδομένα εισόδου σας). Αυτή η λειτουργία είναι πολύ χρήσιμο, εάν έχετε μόνο ένα έναυσμα με χωρίς επιπλέον εισροές, όπως που προβλέψιμα να πρόσβαση στα δεδομένα σας έναυσμα χωρίς αναφορά σας `context` αντικειμένου.

Τα ορίσματα πάντα μεταβιβάζονται κατά μήκος της συνάρτησης με τη σειρά που προκύπτουν στο *function.json*, ακόμα και αν δεν μπορείτε να τα καθορίσετε στην εντολή εξαγωγή. Για παράδειγμα, εάν έχετε `function(context, a, b)` και να την αλλάξετε σε `function(context, a)`, μπορείτε να λάβετε την τιμή της `b` συνάρτηση κώδικα με αναφορά σε `arguments[3]`.

Όλες τις συνδέσεις, ανεξάρτητα από την κατεύθυνση, είναι επίσης αντανάκλαση κατά μήκος του `context` αντικειμένου (δείτε παρακάτω). 

## <a name="context-object"></a>αντικείμενο περιβάλλοντος

Χρησιμοποιεί το περιβάλλον εκτέλεσης μιας `context` για τη μεταβίβαση δεδομένων από και προς την συνάρτηση και για να σας επιτρέπουν να επικοινωνήσετε με το περιβάλλον εκτέλεσης του αντικειμένου.

Το αντικείμενο περιβάλλοντος είναι πάντα η πρώτη παράμετρος σε μια συνάρτηση και πρέπει να είναι πάντοτε περιλαμβάνονται επειδή έχει μεθόδους όπως `context.done` και `context.log` που απαιτούνται για να χρησιμοποιήσετε σωστά το περιβάλλον εκτέλεσης. Μπορείτε να ονομάσετε το αντικείμενο ό, τι θέλετε (δηλαδή `ctx` ή `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>Context.Bindings

Το `context.bindings` αντικείμενο συλλέγει όλα τα δεδομένα εισόδου και εξόδου. Τα δεδομένα προστίθεται σε το `context.bindings` αντικείμενο μέσω του `name` ιδιότητα της σύνδεσης. Για παράδειγμα, λόγω τον ακόλουθο ορισμό σύνδεσης *function.json*, μπορείτε να αποκτήσετε πρόσβαση τα περιεχόμενα της ουράς μέσω `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

Το `context.done` συνάρτηση σας ενημερώνει για το περιβάλλον εκτέλεσης ότι είστε έτοιμοι εκτελείται. Αυτό είναι σημαντικό για να καλέσετε όταν ολοκληρώσετε την εργασία με τη συνάρτηση. Εάν δεν το χρησιμοποιείτε, το περιβάλλον εκτέλεσης θα εξακολουθούν να ποτέ γνωρίζετε ότι σας συνάρτηση ολοκληρώθηκε. 

Το `context.done` συνάρτηση σάς επιτρέπει να επιστρέψει ένα σφάλμα που ορίζονται από το χρήστη για το περιβάλλον εκτέλεσης, καθώς και μια ιδιότητα τσάντα ιδιοτήτων που θα αντικαταστήσει τις ιδιότητες της το `context.bindings` αντικειμένου.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>Context.log(Message)

Το `context.log` μέθοδος σας επιτρέπει να εξόδου καταγραφής προτάσεις που συσχετίζονται μαζί για σκοπούς καταγραφής. Εάν χρησιμοποιείτε το `console.log`, τα μηνύματά σας θα εμφανίσει μόνο για το επίπεδο καταγραφής διαδικασία, που δεν είναι χρήσιμη ως.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

Το `context.log` μέθοδος υποστηρίζει την ίδια μορφοποίηση παράμετρο που υποστηρίζει την κόμβο [util.format μέθοδο](https://nodejs.org/api/util.html#util_util_format_format) . Επομένως, για παράδειγμα, κώδικας ως εξής:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

μπορούν να εγγραφούν ως εξής:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP εναύσματα: context.req και context.res

Στην περίπτωση εναύσματα HTTP, επειδή είναι όπως ένα κοινό μοτίβο για να χρησιμοποιήσετε `req` και `res` για τα HTTP αίτησης και απόκρισης αντικείμενα, αποφασίσει ώστε να μπορείτε εύκολα να αποκτήσετε πρόσβαση σε αυτά στο αντικείμενο περιβάλλοντος, αντί να επιβάλετε μπορείτε να χρησιμοποιήσετε το πλήρες `context.bindings.name` μοτίβο.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Έκδοση κόμβο & πακέτο διαχείρισης

Η έκδοση κόμβου είναι κλειδωμένος τη συγκεκριμένη στιγμή στο `5.9.1`. Προσπαθούμε να Διερεύνηση προσθήκη υποστήριξης για τις περισσότερες εκδόσεις και καθιστώντας με δυνατότητα ρύθμισης παραμέτρων.

Μπορείτε να συμπεριλάβετε πακέτων στη συνάρτηση σας με την αποστολή ενός αρχείου *package.json* της συνάρτησης σας φάκελο στο σύστημα αρχείων στην εφαρμογή της συνάρτησης. Για το αρχείο αποστολή οδηγίες, ανατρέξτε στην ενότητα **πώς μπορείτε να ενημερώσετε τα αρχεία εφαρμογής συνάρτηση** στο [θέμα αναφοράς προγραμματιστή Azure συναρτήσεις](functions-reference.md#fileupdate). 

Μπορείτε επίσης να χρησιμοποιήσετε `npm install` στο περιβάλλον γραμμής εντολών της εφαρμογής της συνάρτησης SCM (Kudu):

1. Μεταβείτε στο: `https://<function_app_name>.scm.azurewebsites.net`.

2. Κάντε κλικ στην επιλογή **Εντοπισμός σφαλμάτων κονσόλας > CMD**.

3. Μεταβείτε στο `D:\home\site\wwwroot\<function_name>`.

4. Εκτέλεση `npm install`.

Αφού έχετε εγκαταστήσει και τα πακέτα που χρειάζεστε, εισαγωγή τους συνάρτηση σας με τους τρόπους συνήθη (δηλαδή μέσω `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Μεταβλητές περιβάλλοντος

Για να λάβετε μια μεταβλητή περιβάλλοντος ή εφαρμογής ρύθμιση τιμής, χρησιμοποιήστε `process.env`, όπως φαίνεται στο παρακάτω παράδειγμα κώδικα:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Υποστήριξη γραφομηχανή/CoffeeScript

Δεν υπάρχει ακόμη, οποιαδήποτε απευθείας υποστήριξη για αυτόματη-μεταγλώττιση γραφομηχανή/CoffeeScript μέσω του χρόνου εκτέλεσης, ώστε να που θα όλα πρέπει να γίνεται εκτός του χρόνου εκτέλεσης, κατά το χρόνο ανάπτυξης. 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md)
* [Azure συναρτήσεις C# αναφορά προγραμματιστών](functions-reference-csharp.md)
* [Azure συναρτήσεις F # αναφορά προγραμματιστών](functions-reference-fsharp.md)
* [Azure συναρτήσεις εναύσματα και συνδέσεις](functions-triggers-bindings.md)

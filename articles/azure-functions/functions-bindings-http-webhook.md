<properties
    pageTitle="Azure συνδέσεις HTTP συναρτήσεις και webhook | Microsoft Azure"
    description="Να κατανοήσετε τον τρόπο χρήσης του HTTP και webhook εναύσματα και συνδέσεις σε συναρτήσεις Azure."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure συνδέσεις HTTP συναρτήσεις και webhook

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και κώδικα HTTP και webhook εναύσματα και συνδέσεις σε συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>Function.JSON για συνδέσεις HTTP και webhook

Το αρχείο *function.json* παρέχει ιδιότητες που σχετίζονται με τόσο το αίτησης και απόκρισης.

Ιδιότητες για την αίτηση HTTP:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το αντικείμενο αίτησης (ή το σώμα της αίτησης στην περίπτωση συναρτήσεις Node.js).
- `type`: Πρέπει να οριστεί στην *httpTrigger*.
- `direction`: Πρέπει να οριστούν *σε*. 
- `webHookType`: Για WebHook εναύσματα, έγκυρες τιμές είναι *github*, *αδράνεια*και *genericJson*. Για ένα έναυσμα HTTP που δεν είναι μια WebHook, ορίστε αυτήν την ιδιότητα σε μια κενή συμβολοσειρά. Για περισσότερες πληροφορίες σχετικά με WebHooks, ανατρέξτε στην παρακάτω ενότητα [WebHook εναύσματα](#webhook-triggers) .
- `authLevel`: Δεν ισχύει για εναύσματα WebHook. Ορισμός σε "συνάρτηση" για να απαιτείται το κλειδί API, "ανώνυμος" αποθέστε το API κλειδιού απαίτηση ή "Διαχειριστής" για να απαιτείται από το πρωτεύον κλειδί API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κλειδιά API](#apikeys) κάτω από το στοιχείο.

Ιδιότητες για την απόκριση HTTP:

- `name`: Όνομα μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το αντικείμενο απόκρισης.
- `type`: Πρέπει να οριστεί σε *http*.
- `direction`: Πρέπει να οριστεί στην *ανάληψη*. 
 
Παράδειγμα *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook εναυσμάτων

Ενεργοποίηση WebHook είναι ένα έναυσμα HTTP που έχει τις ακόλουθες δυνατότητες που έχουν σχεδιαστεί για WebHooks:

* Για ορισμένες WebHook υπηρεσίες παροχής Internet (προς το παρόν GitHub και αδράνεια υποστηρίζονται), ο χρόνος εκτέλεσης συναρτήσεις επικυρώσει υπογραφής της υπηρεσίας παροχής.
* Για τις συναρτήσεις Node.js, ο χρόνος εκτέλεσης συναρτήσεις παρέχει σώμα της αίτησης αντί για το αντικείμενο αίτησης. Δεν υπάρχει κανένας χειρισμός ειδικά για το C# συναρτήσεις, επειδή μπορείτε να ελέγχετε τι παρέχεται, καθορίζοντας τον τύπο παραμέτρου. Εάν καθορίσετε `HttpRequestMessage` λάβετε το αντικείμενο request. Εάν καθορίσετε έναν τύπο POCO, ο χρόνος εκτέλεσης συναρτήσεις προσπαθεί να ανάλυση ενός αντικειμένου JSON στο κυρίως κείμενο της αίτησης για να συμπληρώσετε τις ιδιότητες του αντικειμένου.
* Για να ενεργοποιήσετε μια WebHook συνάρτηση αίτηση HTTP πρέπει να συμπεριλάβετε ένα κλειδί API. Για μη WebHook HTTP εναύσματα, αυτή η απαίτηση είναι προαιρετικό.

Για πληροφορίες σχετικά με το πώς μπορείτε να ρυθμίσετε μια GitHub WebHook, ανατρέξτε στο θέμα [GitHub προγραμματιστής - δημιουργία WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>Διεύθυνση URL για να ενεργοποιήσετε τη συνάρτηση

Για να ενεργοποιήσετε μια συνάρτηση, μπορείτε να στείλετε μια αίτηση HTTP σε μια διεύθυνση URL που είναι ένας συνδυασμός από τη διεύθυνση URL εφαρμογής συνάρτηση και το όνομα συνάρτησης:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>Κλειδιά API

Από προεπιλογή, ένα κλειδί API πρέπει να συμπεριλαμβάνονται με μια αίτηση HTTP για να ενεργοποιήσετε μια συνάρτηση HTTP ή WebHook. Το κλειδί μπορεί να συμπεριληφθεί σε μεταβλητή συμβολοσειράς ερωτήματος με το όνομα `code`, ή να συμπεριληφθεί σε μια `x-functions-key` κεφαλίδα HTTP. Για τις συναρτήσεις μη WebHook, μπορείτε να υποδείξετε ότι ένας αριθμός-κλειδί API δεν απαιτείται, ορίζοντας την `authLevel` ιδιότητα που θα "ανώνυμος" στο αρχείο *function.json* .

Μπορείτε να βρείτε τιμές κλειδιού API στο φάκελο *D:\home\data\Functions\secrets* στο σύστημα αρχείων της εφαρμογής συνάρτηση.  Το πρωτεύον κλειδί και πλήκτρο λειτουργιών έχουν οριστεί στο αρχείο *host.json* , όπως φαίνεται σε αυτό το παράδειγμα. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Το πλήκτρο λειτουργίας από *host.json* μπορεί να χρησιμοποιηθεί για την ενεργοποίηση οποιαδήποτε συνάρτηση αλλά δεν ενεργοποιούν μια συνάρτηση απενεργοποιημένο. Το πρωτεύον κλειδί μπορεί να χρησιμοποιηθεί για την ενεργοποίηση οποιαδήποτε συνάρτηση και θα ενεργοποιεί μια συνάρτηση, ακόμα και αν είναι απενεργοποιημένη. Μπορείτε να ρυθμίσετε μια συνάρτηση για να απαιτείται από το πρωτεύον κλειδί, ορίζοντας την `authLevel` ιδιότητα "Διαχειριστής". 

Εάν ο φάκελος *απόρρητο* περιέχει ένα αρχείο JSON με το ίδιο όνομα με μια συνάρτηση, η `key` ιδιότητα σε αυτό το αρχείο μπορεί επίσης να χρησιμοποιηθεί για να ενεργοποιήσετε τη συνάρτηση και αυτό το κλειδί θα λειτουργούν μόνο με τη συνάρτηση αναφέρεται. Για παράδειγμα, το πλήκτρο API για μια συνάρτηση που ονομάζεται `HttpTrigger` καθορίζεται στο *HttpTrigger.json* στο φάκελο *απορρήτου* . Ακολουθεί ένα παράδειγμα:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Όταν ρυθμίζετε ένα έναυσμα WebHook, μην κάνετε κοινή χρήση του πρωτεύοντος κλειδιού με την υπηρεσία παροχής WebHook. Χρησιμοποιήστε έναν αριθμό-κλειδί που λειτουργούν μόνο με τη συνάρτηση που επεξεργάζεται το WebHook.  Το πρωτεύον κλειδί μπορεί να χρησιμοποιηθεί για την ενεργοποίηση οποιαδήποτε συνάρτηση, ακόμα και να απενεργοποιηθεί συναρτήσεις.

## <a name="example-c-code-for-an-http-trigger-function"></a>Παράδειγμα C# κώδικα για μια συνάρτηση έναυσμα HTTP 

Το παράδειγμα κώδικα αναζητά μια `name` παραμέτρου είτε στη συμβολοσειρά ερωτήματος ή στο σώμα της αίτησης HTTP.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>Παράδειγμα F # κώδικα για μια συνάρτηση έναυσμα HTTP

Το παράδειγμα κώδικα αναζητά μια `name` παραμέτρου είτε στη συμβολοσειρά ερωτήματος ή στο σώμα της αίτησης HTTP.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Θα χρειαστείτε μια `project.json` αρχείου που χρησιμοποιεί NuGet για την αναφορά του `FSharp.Interop.Dynamic` και `Dynamitey` συγκροτήσεις, ως εξής:

```json
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
```

Αυτό θα χρησιμοποιήσετε NuGet για τη λήψη του εξαρτήσεις και θα τους αναφορά στη δέσμη ενεργειών σας.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Παράδειγμα Node.js κώδικα για μια συνάρτηση έναυσμα HTTP 

Σε αυτό το παράδειγμα κώδικα αναζητά μια `name` παραμέτρου είτε στη συμβολοσειρά ερωτήματος ή στο σώμα της αίτησης HTTP.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Παράδειγμα C# κώδικα για μια συνάρτηση GitHub WebHook 

Σε αυτό το παράδειγμα κώδικα καταγράφει GitHub ζήτημα σχόλια.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Παράδειγμα F # κώδικα για μια συνάρτηση GitHub WebHook

Σε αυτό το παράδειγμα κώδικα καταγράφει GitHub ζήτημα σχόλια.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Παράδειγμα Node.js κώδικα για μια συνάρτηση GitHub WebHook 

Σε αυτό το παράδειγμα κώδικα καταγράφει GitHub ζήτημα σχόλια.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

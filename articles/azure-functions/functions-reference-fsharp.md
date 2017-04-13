<properties
    pageTitle="Azure συναρτήσεις F # αναφορά προγραμματιστών | Microsoft Azure"
    description="Για να κατανοήσετε τον τρόπο ανάπτυξης συναρτήσεις Azure χρησιμοποιώντας F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, επεξεργασία, webhooks, δυναμική υπολογισμού, χωρίς αρχιτεκτονική, F συμβάντος#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Αναφορά προγραμματιστών F # Azure συναρτήσεις

> [AZURE.SELECTOR]
- [C# δέσμης ενεργειών](../articles/azure-functions/functions-reference-csharp.md)
- [F # δέσμης ενεργειών](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # για τις συναρτήσεις Azure είναι μια λύση για την εκτέλεση εύκολα μικρά τμήματα κώδικα ή "συναρτήσεις", στο cloud. Δεδομένα ρέει στο σας συνάρτηση F # μέσω ορίσματα συνάρτησης. Ονόματα των ορισμάτων που καθορίζονται στην `function.json`, και υπάρχουν προκαθορισμένα ονόματα για να αποκτήσετε πρόσβαση σε στοιχεία, όπως η συνάρτηση διακριτικά καταγραφής και ακύρωσης.

Σε αυτό το άρθρο προϋποθέτει ότι έχετε διαβάσει ήδη την [αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md).

## <a name="how-fsx-works"></a>Πώς λειτουργεί η .fsx

Μια `.fsx` αρχείο είναι μια δέσμη ενεργειών F #. Αυτό μπορεί να θεωρηθεί ως F # έργο που περιέχεται σε ένα αρχείο. Το αρχείο περιέχει και τα δύο τον κωδικό για το πρόγραμμά σας (στη συγκεκριμένη περίπτωση, η συνάρτηση σας Azure), καθώς και οδηγίες για τη διαχείριση των εξαρτήσεων.

Όταν χρησιμοποιείτε ένα `.fsx` για μια συνάρτηση Azure, συνήθως απαιτείται συγκροτήσεις συμπεριλαμβάνονται αυτόματα για εσάς, επιτρέποντάς σας να εστιάσετε σε τον κώδικα συνάρτηση αντί για "στερεότυπο".

## <a name="binding-to-arguments"></a>Σύνδεση σε ορίσματα

Κάθε σύνδεση υποστηρίζει ορισμένα σύνολα ορισμάτων, όπως περιγράφεται στις [συναρτήσεις Azure εναύσματα και αναφορά προγραμματιστών συνδέσεις](functions-triggers-bindings.md). Για παράδειγμα, μία από τις συνδέσεις όρισμα υποστηρίζει ένα έναυσμα blob είναι μια POCO, η οποία μπορεί να εκφράζονται με μια εγγραφή F #. Για παράδειγμα:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

Συνάρτηση σας Azure F # θα διαρκέσει ένα ή περισσότερα ορίσματα. Όταν, αναφερθούμε ορίσματα Azure συναρτήσεις, ανατρέξτε σε ορίσματα _εισόδου_ και _εξόδου_ ορίσματα. Ένα όρισμα εισόδου είναι ακριβώς τι φαίνεται κάπως: εισόδου σας συνάρτηση Azure F #. Το όρισμα _εξόδου_ είναι μεταβλητά δεδομένων ή ένα `byref<>` όρισμα που χρησιμοποιείται ως ένας τρόπος για τη μεταβίβαση δεδομένων πίσω _εκτός_ της συνάρτησης σας.

Στο παράδειγμα παραπάνω, `blob` είναι ένα όρισμα εισόδου, και `output` είναι ένα όρισμα εξόδου. Παρατηρήστε ότι χρησιμοποιήσαμε `byref<>` για `output` (χωρίς να χρειάζεται να προσθέσετε το `[<Out>]` σχόλιο). Χρήση μιας `byref<>` τύπος επιτρέπει σας συνάρτηση για να αλλάξετε ποια εγγραφή ή το αντικείμενο που αναφέρεται το όρισμα.

Όταν μια εγγραφή F # χρησιμοποιείται ως τύπος εισαγωγής, τον ορισμό εγγραφή πρέπει να επισημανθούν με `[<CLIMutable>]` προκειμένου να επιτρέψετε πλαίσιο Azure συναρτήσεις για να ορίσετε τα πεδία σωστά πριν διέρχεται η εγγραφή σας συνάρτηση. Εμφάνιση σύνθετων ρυθμίσεων, `[<CLIMutable>]` δημιουργεί setters για τις ιδιότητες εγγραφής. Για παράδειγμα:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Μια κλάση F # μπορεί επίσης να χρησιμοποιηθεί για τα δύο στο και ανάληψη ορίσματα. Για μια κατηγορία, ιδιότητες θα χρειαστείτε συνήθως στοιχεία επικέντρωσης και setters. Για παράδειγμα:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Καταγραφή

Για να συνδεθείτε εξόδου σας [ροής αρχεία καταγραφής](../app-service-web/web-sites-streaming-logs-and-console.md) στο F #, συνάρτηση σας πρέπει να εκτελεί το όρισμα του τύπου `TraceWriter`. Για συνέπειας, συνιστάται να ονομάζεται αυτό το όρισμα `log`. Για παράδειγμα:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Ασύγχρονη

Το `async` μπορεί να χρησιμοποιηθεί η ροή εργασίας, αλλά το αποτέλεσμα πρέπει να επιστρέψει μια `Task`. Αυτό μπορεί να γίνει με `Async.StartAsTask`, για παράδειγμα:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Ακύρωση διακριτικού

Εάν σας συνάρτηση πρέπει να χειριστείτε τερματισμού ομαλά, μπορείτε να της δώσετε ένα [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) όρισμα. Αυτό μπορεί να συνδυαστεί με `async`, για παράδειγμα:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Εισαγωγή πεδία ονομάτων

Πεδία ονομάτων μπορεί να ανοίξει με τον συνήθη τρόπο:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Τα παρακάτω πεδία ονομάτων ανοίγει αυτόματα:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Αναφορά σε εξωτερική συγκροτήσεων

Ομοίως, πλαίσια συγκρότησης οι αναφορές να προστεθούν με το `#r "AssemblyName"` οδηγία.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Οι παρακάτω συγκροτήσεις προστίθενται αυτόματα με τις συναρτήσεις Azure περιβάλλον φιλοξενίας:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Επιπλέον, οι παρακάτω συγκροτήσεις έχουν ειδική περίπτωση και μπορεί να γίνει παραπομπή από simplename (π.χ. `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Εάν πρέπει να αναφέρονται σε μια ιδιωτική συγκρότησης, μπορείτε να αποστείλετε το αρχείο συναρμολόγησης σε μια `bin` φακέλου σε σχέση με το συνάρτηση και αναφοράς, χρησιμοποιώντας το αρχείο όνομα (π.χ.  `#r "MyAssembly.dll"`). Για πληροφορίες σχετικά με τον τρόπο για να αποστείλετε αρχεία στο φάκελο συνάρτηση, ανατρέξτε στην παρακάτω ενότητα στο πακέτο διαχείρισης.

## <a name="editor-prelude"></a>Πρόγραμμα επεξεργασίας Prelude

Ένα πρόγραμμα επεξεργασίας που υποστηρίζει τις υπηρεσίες προγράμματος μεταγλώττισης F # δεν θα γνωρίζουν οι χώροι ονομάτων και συγκροτήσεις που περιλαμβάνει Azure συναρτήσεις. Ως εκ τούτου, μπορεί να είναι χρήσιμο για να συμπεριλάβετε μια prelude που σας βοηθά το πρόγραμμα επεξεργασίας Εύρεση οι συγκροτήσεις που χρησιμοποιείτε και για να ανοίξετε ρητά πεδία ονομάτων. Για παράδειγμα:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Όταν Azure συναρτήσεις για εκτέλεση του κώδικα, επεξεργάζεται το αρχείο προέλευσης με `COMPILED` που ορίζονται από το, ώστε το πρόγραμμα επεξεργασίας prelude θα αγνοούνται.

## <a name="package-management"></a>Πακέτο διαχείρισης

Για να χρησιμοποιήσετε τα πακέτα NuGet σε μια συνάρτηση F #, προσθέστε μια `project.json` αρχείων για να το φάκελο της συνάρτησης στο σύστημα αρχείων στην εφαρμογή της συνάρτησης. Ακολουθεί ένα παράδειγμα `project.json` αρχείου που προσθέτει την αναφορά πακέτου NuGet `Microsoft.ProjectOxford.Face` έκδοση 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Υποστηρίζεται μόνο το .NET Framework 4.6, επομένως, βεβαιωθείτε ότι το `project.json` αρχείο Καθορίζει `net46` όπως φαίνεται εδώ.

Όταν κάνετε αποστολή μιας `project.json` αρχείο, το περιβάλλον εκτέλεσης λαμβάνει τα πακέτα και προσθέτει αυτόματα οι αναφορές σε συγκροτήσεις το πακέτο. Δεν χρειάζεται να προσθέσετε `#r "AssemblyName"` οδηγίες. Μόλις προσθέσετε τις απαιτούμενες `open` προτάσεις για να σας `.fsx` αρχείου.

Ίσως θελήσετε να τοποθετήσετε αυτόματα συγκροτήσεις αναφορές σας prelude επεξεργασίας, για να βελτιώσετε το πρόγραμμα επεξεργασίας αλληλεπίδραση με F # μεταγλώττιση τις υπηρεσίες του.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Πώς μπορείτε να προσθέσετε μια `project.json` αρχείο σας συνάρτηση Azure

1. Ξεκινήστε διασφαλίζοντας εφαρμογή της συνάρτησης εκτελείται, όπου μπορείτε να κάνετε με το άνοιγμα λειτουργία σας στην πύλη του Azure. Έτσι μπορείτε να έχετε πρόσβαση στα αρχεία καταγραφής ροής όπου θα εμφανίζονται εξόδου του πακέτου εγκατάστασης.

2. Για να αποστείλετε μια `project.json` αρχείων, χρησιμοποιήστε μία από τις μεθόδους που περιγράφονται στο [πώς μπορείτε να ενημερώσετε τα αρχεία εφαρμογής συνάρτηση](functions-reference.md#fileupdate). Εάν χρησιμοποιείτε [Συνεχούς ανάπτυξης για τις συναρτήσεις Azure](functions-continuous-deployment.md), μπορείτε να προσθέσετε μια `project.json` αρχείο για να σας ενδιάμεσου σταδίου κλάδο προκειμένου να πειραματιστείτε με την πριν από την προσθήκη του σας κλάδο ανάπτυξης.

3. Μετά την `project.json` αρχείο προστίθεται, θα δείτε παρόμοιο με το ακόλουθο παράδειγμα στη συνάρτηση το αποτέλεσμα ροής καταγραφής:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Μεταβλητές περιβάλλοντος

Για να λάβετε μια μεταβλητή περιβάλλοντος ή εφαρμογής ρύθμιση τιμής, χρησιμοποιήστε `System.Environment.GetEnvironmentVariable`, για παράδειγμα:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Επαναχρησιμοποίηση .fsx κώδικα

Μπορείτε να χρησιμοποιήσετε κωδικό από άλλο `.fsx` αρχεία χρησιμοποιώντας ένα `#load` οδηγία. Για παράδειγμα:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Παρέχει διαδρομές για το `#load` οδηγία είναι σε σχέση με τη θέση του σας `.fsx` αρχείου.

* `#load "logger.fsx"`φορτώνει ένα αρχείο που βρίσκεται στο φάκελο συνάρτηση.

* `#load "package\logger.fsx"`φορτώνει ένα αρχείο που βρίσκεται στο το `package` του φακέλου συνάρτηση.

* `#load "..\shared\mylogger.fsx"`φορτώνει ένα αρχείο που βρίσκεται στο το `shared` φακέλων στο ίδιο επίπεδο με το φάκελο συνάρτηση, δηλαδή, απευθείας στην περιοχή `wwwroot`.

Το `#load` οδηγία λειτουργεί μόνο με `.fsx` αρχεία (F # δέσμη ενεργειών), και όχι με `.fs` αρχεία.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Οδηγός F #](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md)
* [Azure συναρτήσεις C# αναφορά προγραμματιστών](functions-reference-csharp.md)
* [Αναφορά προγραμματιστών Azure NodeJS συναρτήσεις](functions-reference-node.md)
* [Azure συναρτήσεις εναύσματα και συνδέσεις](functions-triggers-bindings.md)
* [Azure συναρτήσεις δοκιμές](functions-test-a-function.md)
* [Azure συναρτήσεις κλιμάκωση](functions-scale.md)

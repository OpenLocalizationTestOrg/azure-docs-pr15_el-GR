<properties
    pageTitle="Αναφορά προγραμματιστών Azure συναρτήσεις | Microsoft Azure"
    description="Για να κατανοήσετε τον τρόπο ανάπτυξης συναρτήσεις Azure χρησιμοποιώντας C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, Επεξεργασία συμβάντος, webhooks, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure συναρτήσεις C# αναφορά προγραμματιστών

> [AZURE.SELECTOR]
- [C# δέσμης ενεργειών](../articles/azure-functions/functions-reference-csharp.md)
- [F # δέσμης ενεργειών](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Η εμπειρία C# για τις συναρτήσεις Azure βασίζεται σε στο SDK WebJobs Azure. Δεδομένα ρέει στο C# λειτουργία σας μέσω ορίσματα της μεθόδου. Ονόματα των ορισμάτων που καθορίζονται στην `function.json`, και υπάρχουν προκαθορισμένα ονόματα για να αποκτήσετε πρόσβαση σε στοιχεία, όπως η συνάρτηση διακριτικά καταγραφής και ακύρωσης.

Σε αυτό το άρθρο προϋποθέτει ότι έχετε διαβάσει ήδη την [αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md).

## <a name="how-csx-works"></a>Πώς λειτουργεί η .csx

Το `.csx` μορφή σας επιτρέπει να γράψετε λιγότερο στερεότυπο "κείμενο" και να εστιάσετε σχετικά με τη σύνταξη απλώς C# συνάρτησης. Για τις συναρτήσεις Azure, απλώς περιλαμβάνουν τυχόν αναφορές συγκρότησης και πεδία ονομάτων που χρειάζεστε επάνω, όπως συνήθως, και να αντί για αναδίπλωσης όλα τα στοιχεία σε ένα χώρο ονομάτων και τάξης, μπορείτε να καθορίσετε ακριβώς σας `Run` μέθοδο. Εάν θέλετε να συμπεριλάβετε οποιοδήποτε τις κατηγορίες, για παράδειγμα για να ορίσετε POCO αντικείμενα, μπορείτε να συμπεριλάβετε μια κλάση μέσα στο ίδιο αρχείο.

## <a name="binding-to-arguments"></a>Σύνδεση σε ορίσματα

Τις διάφορες συνδέσεις που είναι συνδεδεμένα με μια συνάρτηση C# μέσω του `name` ιδιότητα στη ρύθμιση παραμέτρων *function.json* . Κάθε σύνδεση έχει το δικό της υποστηριζόμενοι τύποι που τεκμηριώνονται ανά σύνδεση; Για παράδειγμα, ένα έναυσμα blob μπορούν να υποστηρίξουν μια συμβολοσειρά, μια POCO ή πολλοί άλλοι τύποι. Μπορείτε να χρησιμοποιήσετε τον τύπο που ταιριάζει καλύτερα στις ανάγκες σας. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Καταγραφή

Για να συνδεθείτε εξόδου σας ροή αρχείων καταγραφής της C#, μπορείτε να συμπεριλάβετε μια `TraceWriter` πληκτρολογήσατε όρισμα. Σας συνιστούμε να ονομάστε το `log`. Συνιστάται να αποφύγετε `Console.Write` σε συναρτήσεις Azure.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Ασύγχρονη

Για να κάνετε μια συνάρτηση ασύγχρονης, χρησιμοποιήστε το `async` λέξεων-κλειδιών και επιστρέφει μια `Task` αντικειμένου.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Ακύρωση διακριτικού

Σε ορισμένες περιπτώσεις, ενδέχεται να έχετε εργασίες που είναι ευαίσθητα για να την τερματίσετε. Ενώ πάντα είναι καλύτερα να σύνταξη κώδικα που μπορεί να διαχειριστεί παρουσιάζει σφάλμα, στις περιπτώσεις όπου θέλετε να χειριστείτε φυσιολογική τερματισμού αιτήσεις, μπορείτε να καθορίσετε μια [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) πληκτρολογήσατε όρισμα.  A `CancellationToken` θα παρέχονται Εάν ενεργοποιηθεί ένας Τερματισμός κεντρικού υπολογιστή. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Εισαγωγή πεδία ονομάτων

Εάν θέλετε να εισαγάγετε πεδία ονομάτων, μπορείτε να κάνετε επομένως ως συνήθως, με το `using` όρος.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Τα παρακάτω πεδία ονομάτων εισάγονται αυτόματα και, επομένως, είναι προαιρετικά:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Αναφορά σε εξωτερική συγκροτήσεων

Για συγκροτήσεις framework, προσθέστε αναφορές, χρησιμοποιώντας το `#r "AssemblyName"` οδηγία.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Εάν πρέπει να αναφέρονται σε μια ιδιωτική συγκρότησης, μπορείτε να αποστείλετε το αρχείο συναρμολόγησης σε μια `bin` φακέλου σε σχέση με το συνάρτηση και αναφοράς, χρησιμοποιώντας το αρχείο όνομα (π.χ.  `#r "MyAssembly.dll"`). Για πληροφορίες σχετικά με τον τρόπο για να αποστείλετε αρχεία στο φάκελο συνάρτηση, ανατρέξτε στην παρακάτω ενότητα στο πακέτο διαχείρισης.

## <a name="package-management"></a>Πακέτο διαχείρισης

Για να χρησιμοποιήσετε πακέτα NuGet σε μια συνάρτηση C#, αποστείλετε ένα αρχείο *project.json* για το φάκελο της συνάρτησης στο σύστημα αρχείων στην εφαρμογή της συνάρτησης. Ακολουθεί ένα παράδειγμα αρχείου *project.json* που προσθέτει μια αναφορά σε Microsoft.ProjectOxford.Face έκδοση 1.1.0:

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

Υποστηρίζεται μόνο το .NET Framework 4.6, επομένως, βεβαιωθείτε ότι το αρχείο *project.json* καθορίζει `net46` όπως φαίνεται εδώ.

Όταν κάνετε αποστολή ενός αρχείου *project.json* , το περιβάλλον εκτέλεσης λαμβάνει τα πακέτα και προσθέτει αυτόματα οι αναφορές σε συγκροτήσεις το πακέτο. Δεν χρειάζεται να προσθέσετε `#r "AssemblyName"` οδηγίες. Μόλις προσθέσετε τις απαιτούμενες `using` προτάσεων για το αρχείο *run.csx* για να χρησιμοποιήσετε τους τύπους που προσδιορίζονται στα πακέτα NuGet.


### <a name="how-to-upload-a-projectjson-file"></a>Πώς μπορείτε να αποστείλετε ένα αρχείο project.json

1. Ξεκινήστε διασφαλίζοντας εφαρμογή της συνάρτησης εκτελείται, όπου μπορείτε να κάνετε με το άνοιγμα λειτουργία σας στην πύλη του Azure. 

    Έτσι μπορείτε να έχετε πρόσβαση στα αρχεία καταγραφής ροής όπου θα εμφανίζονται εξόδου του πακέτου εγκατάστασης. 

2. Για να αποστείλετε ένα αρχείο project.json, χρησιμοποιήστε μία από τις μεθόδους που περιγράφονται στην ενότητα **πώς μπορείτε να ενημερώσετε τα αρχεία εφαρμογής συνάρτηση** του [Azure συναρτήσεις θέμα αναφοράς προγραμματιστή](functions-reference.md#fileupdate). 

3. Μετά την αποστολή του αρχείου *project.json* , θα δείτε εξόδου όπως το παρακάτω παράδειγμα στη συνάρτηση σας ροή καταγραφής:

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

Για να λάβετε μια μεταβλητή περιβάλλοντος ή εφαρμογής ρύθμιση τιμής, χρησιμοποιήστε `System.Environment.GetEnvironmentVariable`, όπως φαίνεται στο παρακάτω παράδειγμα κώδικα:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Επαναχρησιμοποίηση .csx κώδικα

Μπορείτε να χρησιμοποιήσετε κλάσεις και μεθόδους που έχουν οριστεί σε άλλα αρχεία *.csx* στο αρχείο σας *run.csx* . Για να το κάνετε αυτό, χρησιμοποιήστε `#load` οδηγιών στο αρχείο σας *run.csx* , όπως φαίνεται στο παρακάτω παράδειγμα.

Παράδειγμα *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Παράδειγμα *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Μπορείτε να χρησιμοποιήσετε μια σχετική διαδρομή με το `#load` οδηγία:

* `#load "mylogger.csx"`φορτώνει ένα αρχείο που βρίσκεται στο φάκελο συνάρτηση.

* `#load "loadedfiles\mylogger.csx"`φορτώνει ένα αρχείο που βρίσκεται σε ένα φάκελο στο φάκελο συνάρτηση.

* `#load "..\shared\mylogger.csx"`φορτώνει ένα αρχείο που βρίσκεται σε ένα φάκελο στο ίδιο επίπεδο με το φάκελο συνάρτηση, δηλαδή, απευθείας στην περιοχή *wwwroot*.
 
Το `#load` οδηγία λειτουργεί μόνο με τα αρχεία *.csx* (C# δέσμη ενεργειών) και όχι με τα αρχεία *.cs* . 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στους ακόλουθους πόρους:

* [Αναφορά προγραμματιστών Azure συναρτήσεις](functions-reference.md)
* [Azure συναρτήσεις F # αναφορά προγραμματιστών](functions-reference-fsharp.md)
* [Αναφορά προγραμματιστών Azure NodeJS συναρτήσεις](functions-reference-node.md)
* [Azure συναρτήσεις εναύσματα και συνδέσεις](functions-triggers-bindings.md)


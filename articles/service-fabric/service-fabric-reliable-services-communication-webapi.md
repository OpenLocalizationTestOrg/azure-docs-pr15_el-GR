<properties
   pageTitle="Επικοινωνία με το API Web ASP.NET υπηρεσίας | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να υλοποιήσετε επικοινωνιών βήμα προς βήμα της υπηρεσίας, χρησιμοποιώντας το API Web ASP.NET με OWIN αυτο-φιλοξενίας στην αξιόπιστη API υπηρεσίες."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Γρήγορα αποτελέσματα: υπηρεσίες API Web της υπηρεσίας υφάσματος με OWIN αυτο-φιλοξενίας

Azure Service ύφασμα τοποθετεί στη δύναμη στα χέρια σας όταν πρόκειται να αποφασίσετε πώς θέλετε τις υπηρεσίες σας να επικοινωνούν με τους χρήστες και μεταξύ τους. Αυτό το πρόγραμμα εκμάθησης εστιάζει στην εφαρμογή υπηρεσίας επικοινωνίας με χρήση του ASP.NET Web API με ανοιχτό διασύνδεση Web για τη αυτο-φιλοξενία .NET (OWIN) σε ύφασμα της υπηρεσίας αξιόπιστη API των υπηρεσιών. Θα σας θα delve βάθος σε περιβάλλον επικοινωνίας αξιόπιστων υπηρεσιών API. Θα επίσης χρησιμοποιήσουμε το API Web σε ένα παράδειγμα βήμα προς βήμα για να σας δείξουν πώς μπορείτε να ρυθμίσετε μια προσαρμοσμένη επικοινωνίας παρακολούθηση.


## <a name="introduction-to-web-api-in-service-fabric"></a>Εισαγωγή στις API Web στην υπηρεσία ύφασμα

ASP.NET Web API είναι ένα δημοφιλές και ισχυρά πλαίσιο για τη δημιουργία APIs HTTP επάνω από το .NET Framework. Εάν δεν είστε ήδη εξοικειωμένοι με το πλαίσιο, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το ASP.NET Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) για να μάθετε περισσότερα.

API Web στην υπηρεσία ύφασμα είναι το ίδιο API Web ASP.NET που γνωρίζετε και αγαπάτε. Η διαφορά είναι στον τρόπο που *host* μιας εφαρμογής Web API. Δεν θα χρησιμοποιείτε Microsoft Internet Information Services (IIS). Για να κατανοήσετε καλύτερα τη διαφορά, ας τη χωρίσετε σε δύο μέρη:

 1. Η εφαρμογή Web API (όπως ελεγκτές και τα μοντέλα)
 2. Στον κεντρικό υπολογιστή (το διακομιστή web, συνήθως των υπηρεσιών IIS)

Μια εφαρμογή Web API ίδια δεν αλλάζει. Είναι δεν διαφέρει από τις εφαρμογές Web API που μπορεί να έχει εγγραφεί στο παρελθόν και θα πρέπει να μπορείτε να μετακινήσετε απλώς πάνω από το μεγαλύτερο τμήμα του κώδικα της εφαρμογής σας. Αλλά, εάν έχετε διατηρείτε στις υπηρεσίες IIS, όπου μπορεί να είναι λίγο διαφορετικό από το τι έχετε συνηθίσει να host της εφαρμογής. Πριν από την λαμβάνουμε στο τμήμα φιλοξενίας, ας ξεκινήσουμε με κάτι περισσότερο οικεία: η εφαρμογή Web API.


## <a name="create-the-application"></a>Δημιουργία της εφαρμογής

Ξεκινήστε με τη δημιουργία νέας εφαρμογής υπηρεσίας υφάσματος με μία μόνο υπηρεσία χωρίς κατάσταση στο Visual Studio 2015:

![Δημιουργία νέας εφαρμογής υπηρεσίας ύφασμα](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Ένα πρότυπο Visual Studio για χρήση του Web API χωρίς κατάσταση υπηρεσίας είναι διαθέσιμες σε εσάς. Σε αυτό το πρόγραμμα εκμάθησης, θα μπορέσουμε να δημιουργήσουμε ένα έργο Web API από την αρχή που καταλήγει σε τι θα λαμβάνατε εάν έχετε επιλέξει αυτό το πρότυπο.

Επιλέξτε ένα κενό έργο χωρίς κατάσταση υπηρεσίας για να μάθετε πώς μπορείτε να δημιουργήσετε ένα έργο Web API από την αρχή ή μπορείτε να ξεκινήσετε με το πρότυπο Web API χωρίς κατάσταση υπηρεσίας και ακολουθήστε απλώς κατά μήκος.  

![Δημιουργήστε ένα μεμονωμένο χωρίς κατάσταση υπηρεσίας](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Το πρώτο βήμα είναι να έλκει ορισμένα πακέτα NuGet για το API Web. Το πακέτο θέλουμε να χρησιμοποιήσετε είναι Microsoft.AspNet.WebApi.OwinSelfHost. Αυτό το πακέτο περιλαμβάνει όλα τα απαραίτητα πακέτα Web API και των πακέτων του *κεντρικού υπολογιστή* . Αυτό θα είναι σημαντικές αργότερα.

![Δημιουργία Web API με χρήση της διαχείρισης πακέτου NuGet](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Αφού έχουν εγκατασταθεί τα πακέτα, μπορείτε να ξεκινήσετε δόμησης ανάληψη τη βασική δομή έργου Web API. Εάν έχετε χρησιμοποιήσει το API Web, θα πρέπει να είναι πολύ οικείο τη δομή του έργου. Ξεκινήστε προσθέτοντας ένα `Controllers` καταλόγου και ελεγκτή απλό τιμές:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Στη συνέχεια, προσθέστε ένα εκπαιδευτικό εκκίνησης στο ριζικό κατάλογο έργου για να καταχωρήσετε τη δρομολόγηση, formatters και οποιοδήποτε άλλο πρόγραμμα εγκατάστασης ρύθμισης παραμέτρων. Αυτό είναι επίσης όπου το API Web συνδέεται *κεντρικό υπολογιστή*, που θα να επανεξετάζονται ξανά αργότερα. 

**Startup.CS**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Που είναι για το τμήμα εφαρμογής. Σε αυτό το σημείο, έχετε μπορούμε να εγκαταστήσουμε απλώς τη βασική διάταξη έργου Web API. Μέχρι στιγμής, δεν θα πρέπει είναι πολύ διαφορετικό από το API Web έργα που ενδέχεται να έχετε εγγραφεί στο παρελθόν ή από το βασικό πρότυπο Web API. Επιχειρηματικής λογικής μεταβαίνει στην ελεγκτές και τα μοντέλα ως συνήθως.

Τώρα τι θα σας κάνω πληροφορίες για τη φιλοξενία ώστε να σας μπορεί να το εκτελέσει στην πραγματικότητα;

## <a name="service-hosting"></a>Υπηρεσία φιλοξενίας

Σε ύφασμα υπηρεσίας, την υπηρεσία εκτελείται σε μια *υπηρεσία διεργασία κεντρικού υπολογιστή*, ένα εκτελέσιμο αρχείο που εκτελεί την υπηρεσία κώδικα. Όταν γράφετε μια υπηρεσία, χρησιμοποιώντας την αξιόπιστη API των υπηρεσιών, το έργο σας υπηρεσία συγκεντρώνει μόνο σε εκτελέσιμο αρχείο που καταχωρεί τον τύπο της υπηρεσίας και εκτελεί τον κώδικα. Αυτό ισχύει στις περισσότερες περιπτώσεις, κατά την εγγραφή μιας υπηρεσίας στην υπηρεσία ύφασμα στο .NET. Όταν ανοίγετε Program.cs του έργου χωρίς κατάσταση υπηρεσίας, θα πρέπει να βλέπετε:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Εάν που φαίνεται ύποπτα όπως το σημείο εισόδου σε μια εφαρμογή κονσόλας, αυτό συμβαίνει επειδή είναι.

Περισσότερες λεπτομέρειες σχετικά με την υπηρεσία διεργασία κεντρικού υπολογιστή και την καταγραφή της υπηρεσίας είναι πέρα από το πεδίο αυτού του άρθρου. Αλλά είναι σημαντικό να γνωρίζετε για τώρα αυτόν *τον κωδικό υπηρεσία εκτελείται στη δική του διεργασία*.

## <a name="self-host-web-api-with-an-owin-host"></a>Αυτο-φιλοξενήσετε Web API σε μια υπηρεσία παροχής φιλοξενίας OWIN

Δεδομένου ότι κώδικα της εφαρμογής σας Web API φιλοξενείται στη δική του διεργασία, πώς που συνδέσετε με την παροχή σε διακομιστή web; Πληκτρολογήστε [OWIN](http://owin.org/). OWIN είναι απλώς ένα συμβόλαιο μεταξύ των εφαρμογών web .NET και διακομιστές web. Παραδοσιακά όταν χρησιμοποιείται ASP.NET (έως και MVC 5), η εφαρμογή web στενά συνδεδεμένων στις υπηρεσίες IIS μέσω System.Web. Ωστόσο, το API Web υλοποιεί OWIN, ώστε να μπορείτε να συντάξετε μια εφαρμογή web που είναι αποσυνδεδεμένη από το διακομιστή web που φιλοξενεί το. Αυτόν το λόγο, μπορείτε να χρησιμοποιήσετε ένα *αυτο-φιλοξενούνται* OWIN διακομιστή web που μπορείτε να ξεκινήσετε τη δική σας διεργασίας. Αυτό προσαρμόζεται απόλυτα με το μοντέλο φιλοξενίας ύφασμα υπηρεσιών μας περιγράφεται.

Σε αυτό το άρθρο, θα χρησιμοποιήσουμε Katana με τον κεντρικό υπολογιστή OWIN για την εφαρμογή Web API. Katana είναι μια υλοποίηση host OWIN ανοιχτού κώδικα ενσωματωμένη σε [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) και το Windows [HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).

> [AZURE.NOTE] Για να μάθετε περισσότερα σχετικά με το Katana, μεταβείτε στην [τοποθεσία Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Για μια γρήγορη επισκόπηση του τρόπου χρήσης Katana για τη αυτο-φιλοξενία Web API, ανατρέξτε στο θέμα [Χρήση OWIN σε Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Ρυθμίστε το διακομιστή web

Το API των υπηρεσιών αξιόπιστη παρέχει ένα σημείο εισόδου επικοινωνίας, όπου μπορείτε να συνδέσετε σε στοίβες επικοινωνίας που επιτρέπει στους χρήστες και προγράμματα-πελάτες για να συνδεθείτε με την υπηρεσία:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Ο διακομιστής web (και τυχόν άλλες στοίβας επικοινωνίας που χρησιμοποιείται στο μέλλον, όπως WebSockets) πρέπει να χρησιμοποιήσετε το περιβάλλον εργασίας ICommunicationListener για να ενσωματώσετε σωστά με το σύστημα. Τους λόγους για αυτό θα μετατραπεί σε πιο εμφανή στα παρακάτω βήματα.

Πρώτα, δημιουργήστε κλάσης που ονομάζεται OwinCommunicationListener που υλοποιεί ICommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

Το περιβάλλον εργασίας ICommunicationListener παρέχει τρεις μεθόδους για τη διαχείριση μιας υπηρεσίας ακρόασης επικοινωνίας για την υπηρεσία σας:

 - *OpenAsync*. Έναρξη ακρόαση για αιτήσεις.
 - *CloseAsync*. Διακοπή ακρόασης για αιτήσεις, λήξη οποιαδήποτε εν πτήσει αιτήσεις και τερματισμός ομαλά.
 - *Ματαίωση*. Ακύρωση όλα τα στοιχεία "και" Διακοπή αμέσως.

Για να ξεκινήσετε, προσθέστε μέλη ιδιωτικό κλάσης για το ακροατήριο θα πρέπει να λειτουργούν πράγματα. Αυτά θα προετοιμαστεί μέσω της κατασκευής και θα χρησιμοποιηθεί αργότερα κατά τη ρύθμιση του ακρόασης διεύθυνσης URL.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Υλοποίηση της OpenAsync

Για να ρυθμίσετε το διακομιστή web, χρειάζεστε δύο στοιχεία των πληροφοριών:

 - *Πρόθεμα διαδρομή A URL*. Παρόλο που αυτή είναι προαιρετική, καλό είναι να το ορίσετε τώρα, έτσι ώστε να μπορείτε να φιλοξενήσετε ασφαλής πολλές υπηρεσίες web στην εφαρμογή σας.
 - *Μια θύρα*.

Πριν να λάβετε μια θύρα για το διακομιστή web, είναι σημαντικό να κατανοήσετε ότι ύφασμα υπηρεσία παρέχει ένα επίπεδο εφαρμογής που λειτουργεί ως buffer μεταξύ της εφαρμογής σας και το υποκείμενο λειτουργικό σύστημα που εκτελείται στον. Ως εκ τούτου, ύφασμα υπηρεσία παρέχει ένας τρόπος για να ρυθμίσετε τις παραμέτρους *τελικά σημεία* για τις υπηρεσίες σας. Υπηρεσία ύφασμα εξασφαλίζει ότι τα τελικά σημεία είναι διαθέσιμοι για την υπηρεσία για να χρησιμοποιήσετε. Με αυτόν τον τρόπο, δεν χρειάζεται να ρυθμίσετε τις παραμέτρους τους στον εαυτό σας στο περιβάλλον των υποκείμενων OS. Μπορείτε εύκολα να φιλοξενήσετε την εφαρμογή υπηρεσίας ύφασμα σε διαφορετικά περιβάλλοντα χωρίς να χρειάζεται να κάνετε αλλαγές στην εφαρμογή σας. (Για παράδειγμα, μπορείτε να φιλοξενήσετε το ίδιο εφαρμογής στο Azure ή σε δικό του κέντρου δεδομένων.)

Ρύθμιση παραμέτρων ορίου HTTP στο PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Αυτό το βήμα είναι σημαντική, επειδή η διεργασία κεντρικού υπολογιστή υπηρεσίας εκτελείται με περιορισμένα διαπιστευτήρια (υπηρεσία δικτύου στα Windows). Αυτό σημαίνει ότι η υπηρεσία σας δεν θα έχει πρόσβαση για να ρυθμίσετε ένα τελικό σημείο HTTP σε δικό του. Με τη ρύθμιση παραμέτρων του τελικού σημείου, υπηρεσία ύφασμα γνωρίζει για να ρυθμίσετε τη λίστα ελέγχου proper πρόσβασης (ACL) για τη διεύθυνση URL που θα ακρόαση της υπηρεσίας. Υπηρεσία ύφασμα παρέχει επίσης μια τυπική θέση για ρύθμιση παραμέτρων τελικά σημεία.


Πίσω στο OwinCommunicationListener.cs, μπορείτε να ξεκινήσετε την εφαρμογή OpenAsync. Αυτό είναι όπου μπορείτε να ξεκινήσετε το διακομιστή web. Πρώτα, λάβετε τις πληροφορίες τελικού σημείου και δημιουργήστε τη διεύθυνση URL που θα ακρόαση της υπηρεσίας. Η διεύθυνση URL θα διαφέρει ανάλογα με το εάν το ακροατήριο χρησιμοποιείται σε μια υπηρεσία χωρίς κατάσταση ή κατάσταση υπηρεσίας. Για μια υπηρεσία κατάστασης, το ακροατήριο πρέπει να δημιουργήσει μια μοναδική διεύθυνση για κάθε ρεπλίκα κατάστασης υπηρεσίας κάνει ακρόαση σε. Για τις υπηρεσίες χωρίς κατάσταση, τη διεύθυνση μπορεί να είναι πολύ πιο απλά. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Σημειώστε ότι η "http://+" χρησιμοποιείται εδώ. Πρόκειται για να βεβαιωθείτε ότι ο διακομιστής web ακρόαση σε όλες τις διαθέσιμες διευθύνσεις, συμπεριλαμβάνοντας τοπικού κεντρικού υπολογιστή, το FQDN και IP του υπολογιστή.

Η εφαρμογή OpenAsync είναι μία από τις πιο σημαντικές αιτίες γιατί ο διακομιστής web (ή οποιαδήποτε στοίβα επικοινωνίας) έχει υλοποιηθεί ως μια ICommunicationListener και όχι απλώς να το ανοίξετε απευθείας από `RunAsync()` στην υπηρεσία. Η τιμή που επιστρέφεται από OpenAsync είναι η διεύθυνση που εκτελεί ακρόαση το διακομιστή web σε. Όταν αυτή η διεύθυνση επιστρέφεται στο σύστημα, καταχωρεί τη διεύθυνση με την υπηρεσία. Υπηρεσία ύφασμα παρέχει ένα API που επιτρέπει στους υπολογιστές-πελάτες και άλλες υπηρεσίες, στη συνέχεια, για να ζητήσετε αυτήν τη διεύθυνση, το όνομα της υπηρεσίας. Αυτό είναι σημαντικό, επειδή η διεύθυνση της υπηρεσίας δεν είναι στατική. Υπηρεσίες μετακινούνται μέσα στο σύμπλεγμα για σκοπούς εξισορρόπηση και διαθεσιμότητα πόρων. Αυτό είναι το μηχανισμό που επιτρέπει στους υπολογιστές-πελάτες για να επιλύσετε τη διεύθυνση ακρόασης για μια υπηρεσία.

Αυτό υπόψη, OpenAsync ξεκινά το διακομιστή web και επιστρέφει τη διεύθυνση που το κάνει ακρόαση στη. Σημείωση κάνει ακρόαση σε "http://+", αλλά πριν από την OpenAsync επιστρέφει τη διεύθυνση, το "+" αντικαθίσταται με το IP ή το FQDN του κόμβου είναι αυτήν τη στιγμή σε. Η διεύθυνση που επιστρέφεται από αυτήν τη μέθοδο είναι τι έχει καταχωρηθεί με το σύστημα. Επίσης, είναι τι προγράμματα-πελάτες και άλλες υπηρεσίες δείτε όταν ζητείται τη διεύθυνση της υπηρεσίας. Για προγράμματα-πελάτες να συνδεθούν σωστά σε αυτό, χρειάζονται μια πραγματική IP ή FQDN στη διεύθυνση.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Σημειώστε ότι αυτό αναφέρει την κλάση εκκίνησης που μεταβιβάστηκε για το OwinCommunicationListener στην κατασκευή. Αυτή η παρουσία εκκίνησης χρησιμοποιείται από το διακομιστή web γίνεται εκκίνηση της εφαρμογής Web API.

Το `ServiceEventSource.Current.Message()` γραμμής εμφανίζεται στο παράθυρο διαγνωστικών συμβάντα αργότερα, όταν εκτελείτε την εφαρμογή για να επιβεβαιώσετε ότι ο διακομιστής web έχει ξεκινήσει με επιτυχία.

## <a name="implement-closeasync-and-abort"></a>Υλοποίηση CloseAsync και ματαίωση

Τέλος, υλοποιήστε CloseAsync και ματαίωση για να διακόψετε το διακομιστή web. Ο διακομιστής web μπορεί να διακοπεί με τη διάθεση τη λαβή διακομιστή που δημιουργήθηκε κατά τη διάρκεια της OpenAsync.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

Σε αυτό το παράδειγμα υλοποίηση CloseAsync και ματαίωση απλώς διακόπτετε το διακομιστή web. Ενδέχεται να μπορείτε να επιλέξετε να εκτελέσετε μια πιο ομαλά συντονισμένη τερματισμού του διακομιστή web σε CloseAsync. Για παράδειγμα, ο τερματισμός θα μπορούσε να περιμένετε έως ότου εν πτήσει αιτήσεις για να ολοκληρωθούν πριν από την επιστροφή.

## <a name="start-the-web-server"></a>Ξεκινήστε το διακομιστή web

Τώρα είστε έτοιμοι να δημιουργήσετε και να επιστρέψετε μια παρουσία του OwinCommunicationListener για να ξεκινήσετε το διακομιστή web. Επιστροφή στην τάξη υπηρεσίας (WebService.cs), παρακάμπτουν το `CreateServiceInstanceListeners()` μέθοδο:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Αυτό είναι όπου το Web API *εφαρμογής* και του OWIN *host* τέλος ικανοποιούν. Στον κεντρικό υπολογιστή (OwinCommunicationListener) λαμβάνει μια παρουσία της *εφαρμογής* (Web API) μέσω την κλάση εκκίνησης. Υπηρεσία ύφασμα διαχειρίζεται, στη συνέχεια, τη διάρκεια του κύκλου ζωής. Αυτό το ίδιο μοτίβο μπορείτε να ακολουθήσετε συνήθως με οποιαδήποτε στοίβα επικοινωνίας.

## <a name="put-it-all-together"></a>Να τα οργανώσετε όλων

Σε αυτό το παράδειγμα, δεν χρειάζεται να κάνετε τίποτα το `RunAsync()` μέθοδο, ώστε να που παράκαμψη απλώς μπορούν να καταργηθούν.

Την υλοποίηση της υπηρεσίας τελικό πρέπει να είναι πολύ απλής. Μόνο πρέπει να δημιουργήσετε την παρακολούθηση της επικοινωνίας:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Η πλήρης `OwinCommunicationListener` κλάσης:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Τώρα που έχετε τοποθετείτε όλα τα τμήματα στη θέση, το έργο σας θα πρέπει να μοιάζει με μια τυπική εφαρμογή Web API με σημεία εισόδου αξιόπιστο το API των υπηρεσιών και κεντρικός υπολογιστής OWIN:


![API Web με σημεία εισόδου αξιόπιστο το API των υπηρεσιών και OWIN κεντρικού υπολογιστή](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Εκτέλεση και σύνδεση μέσω ενός προγράμματος περιήγησης web

Εάν δεν το έχετε κάνει, [Ρυθμίστε το περιβάλλον ανάπτυξης](service-fabric-get-started.md).


Τώρα, μπορείτε να δημιουργήσετε και να υλοποιήσετε την υπηρεσία. Πατήστε το πλήκτρο **F5** στο Visual Studio για τη δημιουργία και ανάπτυξη της εφαρμογής. Στο παράθυρο διαγνωστικών συμβάντα, θα πρέπει να βλέπετε ένα μήνυμα που δηλώνει ότι ο διακομιστής web ανοίξει σε http://localhost:8281 /.


![Παράθυρο Visual Studio διαγνωστικών συμβάντα](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Εάν η θύρα έχει ήδη ανοίξει από άλλη διεργασία στον υπολογιστή σας, μπορείτε να δείτε εδώ ένα σφάλμα. Αυτό υποδεικνύει ότι δεν ήταν δυνατό να ανοίξει το ακροατήριο. Εάν πρόκειται για την περίπτωση, δοκιμάστε να χρησιμοποιήσετε μια διαφορετική θύρα για τη ρύθμιση παραμέτρων ορίου σε ServiceManifest.xml.


Όταν εκτελείται η υπηρεσία, ανοίξτε ένα πρόγραμμα περιήγησης και μεταβείτε σε [api/http://localhost:8281/τιμών](http://localhost:8281/api/values) για να την δοκιμάσετε.

## <a name="scale-it-out"></a>Κλιμάκωση την προβολή

Κλιμάκωση ανάληψη εφαρμογές web χωρίς κατάσταση συνήθως σημαίνει ότι η προσθήκη περισσότερων μηχανές και περιστροφής τα web apps σε αυτά. Μηχανισμός ενορχήστρωσης της υπηρεσίας ύφασμα μπορεί να κάνει για εσάς κάθε φορά που προστίθενται νέα κόμβους σε ένα σύμπλεγμα. Όταν δημιουργείτε παρουσίες μιας χωρίς κατάσταση υπηρεσίας, μπορείτε να καθορίσετε τον αριθμό των παρουσιών που θέλετε να δημιουργήσετε. Υπηρεσία ύφασμα τοποθετεί αυτόν τον αριθμό των παρουσιών σε κόμβους του συμπλέγματος. Και ότι δεν θέλετε να δημιουργήσετε περισσότερες από μία παρουσία σε οποιαδήποτε έναν κόμβο. Μπορείτε επίσης να υποδείξετε ύφασμα υπηρεσίας για να δημιουργήσετε πάντα μια παρουσία σε κάθε κόμβο, καθορίζοντας **-1** για την καταμέτρηση παρουσία. Αυτό εγγυάται ότι κάθε φορά που προσθέτετε κόμβους για να κλιμακωθεί ανάληψη το σύμπλεγμά σας, μια παρουσία σας χωρίς κατάσταση υπηρεσίας θα δημιουργηθούν στον νέο κόμβους. Αυτή η τιμή είναι μια ιδιότητα της παρουσίας υπηρεσίας, ώστε να έχει οριστεί όταν δημιουργείτε μια παρουσία της υπηρεσίας. Μπορείτε να το κάνετε μέσω του PowerShell:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Μπορείτε επίσης να κάνετε αυτό όταν ορίζετε μια προεπιλεγμένη υπηρεσία σε ένα έργο Visual Studio χωρίς κατάσταση υπηρεσίας:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία εφαρμογής και παρουσιών της υπηρεσίας, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Επόμενα βήματα

[Εντοπισμός σφαλμάτων εφαρμογής υπηρεσίας ύφασμα σας με χρήση του Visual Studio](service-fabric-debugging-your-application.md)

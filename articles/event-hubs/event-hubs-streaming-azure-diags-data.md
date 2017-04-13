<properties
    pageTitle="Διαγνωστικά Azure δεδομένων στη διαδρομή συντόμευσης χρησιμοποιώντας διανομείς συμβάν ροής | Microsoft Azure"
    description="Παρουσιάζει τον τρόπο ρύθμισης των παραμέτρων Διαγνωστικά Azure με διανομείς συμβάν από λήξη σε λήξη, όπως οδηγίες για συνηθισμένα σενάρια."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/14/2016"
    ms.author="sethm" />

# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Διαγνωστικά Azure δεδομένων στη διαδρομή συντόμευσης ροής με τη χρήση διανομείς συμβάντος

Διαγνωστικά του Azure παρέχει ευέλικτες τρόπους για να συλλέξετε μετρήσεις και τα αρχεία καταγραφής από τις υπηρεσίες cloud εικονικές μηχανές (ΣΠΣ) και μεταφορά αποτελέσματα με το χώρο αποθήκευσης Azure. Ξεκινώντας από το χρονικό πλαίσιο Μαρτίου 2016 (SDK 2.9), μπορείτε να δέκτη Διαγνωστικά σε προελεύσεις δεδομένων πλήρως προσαρμοσμένες και να μεταφέρετε δεδομένα συντόμευσης διαδρομή σε δευτερόλεπτα χρησιμοποιώντας [Azure συμβάν διανομείς](https://azure.microsoft.com/services/event-hubs/).

Υποστηριζόμενοι τύποι δεδομένων περιλαμβάνουν τα εξής:

- Συμβάν ανίχνευσης για Windows (ETW) συμβάντα
- Μετρητές επιδόσεων
- Αρχεία καταγραφής συμβάντων των Windows
- Αρχεία καταγραφής εφαρμογών
- Azure αρχεία καταγραφής από την υποδομή Διαγνωστικά

Σε αυτό το άρθρο δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους Διαγνωστικά Azure με διανομείς συμβάν από λήξη σε λήξη. Καθοδήγηση παρέχεται επίσης για τα ακόλουθα συνηθισμένα σενάρια:

- Πώς μπορείτε να προσαρμόσετε τα αρχεία καταγραφής και τις μετρήσεις που λαμβάνει sinked με διανομείς συμβάντος
- Πώς μπορείτε να αλλάξετε τις ρυθμίσεις παραμέτρων σε κάθε περιβάλλον
- Πώς μπορείτε να προβάλετε διανομείς συμβάν ροής δεδομένων
- Τρόπος αντιμετώπισης προβλημάτων της σύνδεσης  

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Συμβάν διανομείς βυθιστεί στο Azure Διαγνωστικά υποστηρίζεται στο Cloud Services, ΣΠΣ, εικονική μηχανή κλίμακα σύνολα και υπηρεσία ύφασμα ξεκινώντας από το Azure 2.9 SDK και τα αντίστοιχα εργαλεία Azure για το Visual Studio.

- Επέκταση του Azure Διαγνωστικά 1.6 ([SDK Azure για .NET 2.9 ή νεότερη έκδοση](https://azure.microsoft.com/downloads/) ως προορισμό αυτό από προεπιλογή)
- [Visual Studio 2013 ή νεότερη έκδοση](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- Υπάρχουσες ρυθμίσεις παραμέτρων του Azure Διαγνωστικά σε μια εφαρμογή, χρησιμοποιώντας ένα αρχείο *.wadcfgx* και μία από τις παρακάτω μεθόδους:
    - [Ρύθμιση παραμέτρων διαγνωστικών για τις υπηρεσίες Azure Cloud και εικονικές μηχανές](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) του Visual Studio:
    - Windows PowerShell: [Ενεργοποίηση Διαγνωστικά στις υπηρεσίες Cloud Azure χρησιμοποιώντας το PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
- Χώρος ονομάτων διανομείς συμβάν παρασχεθεί ανά στο άρθρο, [Γρήγορα αποτελέσματα με το συμβάν διανομείς](./event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Σύνδεση Διαγνωστικά Azure με δέκτη διανομείς συμβάντος

Διαγνωστικά του Azure δέκτες πάντα τα αρχεία καταγραφής και μετρήσεις, από προεπιλογή, σε ένα λογαριασμό αποθήκευσης Azure. Μια εφαρμογή μπορεί να επιπλέον δέκτη με διανομείς συμβάν, προσθέτοντας μια νέα ενότητα **δέκτες** στο στοιχείο **WadCfg** στην ενότητα **PublicConfig** του αρχείου *.wadcfgx* . Στο Visual Studio, είναι αποθηκευμένο το αρχείο *.wadcfgx* στην ακόλουθη διαδρομή: **Έργου υπηρεσία Cloud** > **ρόλους** >  **(όνομα ρόλου)** > **diagnostics.wadcfgx** αρχείου.

```
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```

Σε αυτό το παράδειγμα, τη διεύθυνση URL διανομέα συμβάντος έχει οριστεί σε το πλήρως προσδιορισμένο χώρο ονομάτων από την ενότητα συμβάν: χώρος ονομάτων διανομείς συμβάν + "/" + όνομα διανομέας συμβάν.  

Διεύθυνση URL του διανομέα συμβάν εμφανίζεται στην [πύλη του Azure](http://go.microsoft.com/fwlink/?LinkID=213885) στον πίνακα εργαλείων του συμβάντος διανομείς.  

Το όνομα **δέκτη** μπορεί να οριστεί σε οποιαδήποτε έγκυρη συμβολοσειρά, με την προϋπόθεση ότι χρησιμοποιείται η ίδια τιμή, με συνέπεια σε όλο το αρχείο ρύθμισης παραμέτρων.

> [AZURE.NOTE]  Μπορεί να υπάρχουν επιπλέον δέκτες, όπως *applicationInsights* έχει ρυθμιστεί σε αυτήν την ενότητα. Διαγνωστικά του Azure επιτρέπει στους μίας ή περισσότερων δέκτες που καθορίζονται εάν κάθε δέκτη δηλώνεται επίσης στην ενότητα **PrivateConfig** .  

Επίσης πρέπει να έχουν δηλωθεί και να οριστεί στην ενότητα **PrivateConfig** του αρχείου ρύθμισης παραμέτρων *.wadcfgx* του δέκτη συμβάντος διανομείς.

```
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="" key="" endpoint="" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="9B3SwghJOGEUvXigc6zHPInl2iYxrgsKHZoy4nm9CUI=" />
</PrivateConfig>
```

Το `SharedAccessKeyName` τιμή πρέπει να συμφωνεί με ένα κλειδί θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας) και πολιτική που έχει οριστεί στο χώρο ονομάτων **Διανομείς συμβάν** . Μεταβείτε στον πίνακα εργαλείων διανομείς συμβάντων στην [πύλη του Azure](https://manage.windowsazure.com), κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** και ορίστε μια καθορισμένη πολιτική (για παράδειγμα, "SendRule") που έχει δικαιώματα *Αποστολή* . Το **StorageAccount** δηλώνεται επίσης στο **PrivateConfig**. Δεν χρειάζεται να αλλάξετε τιμές εδώ εάν εργάζονται. Σε αυτό το παράδειγμα, θα σας αφήστε τις τιμές κενό, που είναι ένα σύμβολο ότι ένα ροής περιουσιακών στοιχείων θα ορίσετε τις τιμές. Για παράδειγμα, το αρχείο ρύθμισης παραμέτρων του περιβάλλοντος *ServiceConfiguration.Cloud.cscfg* ορίζει τα κατάλληλα περιβάλλον ονόματα και αριθμούς-κλειδιά.  

> [AZURE.WARNING] Το κλειδί συσχετισμών Ασφαλείας διανομείς συμβάν είναι αποθηκευμένο σε μορφή απλού κειμένου στο αρχείο *.wadcfgx* . Συχνά, αυτό το κλειδί έχει γίνει ανάληψη ελέγχου στοιχείο ελέγχου πηγαίου κώδικα ή είναι διαθέσιμη ως ενός περιουσιακού στοιχείου στο διακομιστή σας δόμηση, επομένως, θα πρέπει να προστατεύσετε το ανάλογα με την περίπτωση. Συνιστάται να χρησιμοποιήσετε έναν αριθμό-κλειδί συσχετισμών Ασφαλείας εδώ με *Αποστολή μόνο* δικαιώματα ώστε να κακόβουλο χρήστη να γράψετε για την ενότητα συμβάντων, αλλά δεν κάνουν ακρόαση σε αυτό ή να διαχειριστείτε το.

## <a name="configure-azure-diagnostics-logs-and-metrics-to-sink-with-event-hubs"></a>Ρύθμιση παραμέτρων των αρχείων καταγραφής διαγνωστικών Azure και των μετρήσεων για δέκτη με διανομείς συμβάντος

Όπως περιγράφεται παραπάνω, όλα προεπιλογή και τα Διαγνωστικά του προσαρμοσμένου δεδομένα, δηλαδή, μετρήσεις και τα αρχεία καταγραφής, είναι sinked αυτόματα με το χώρο αποθήκευσης Azure στο τα χρονικά διαστήματα που έχει ρυθμιστεί. Με διανομείς συμβάντων και τυχόν πρόσθετες δέκτη, μπορείτε να καθορίσετε οποιαδήποτε κόμβο ρίζας ή φύλλα στην ιεραρχία να είναι sinked με την ενότητα συμβάντων. Αυτό περιλαμβάνει τα συμβάντα ETW, μετρητές επιδόσεων, αρχεία καταγραφής συμβάντων των Windows και αρχεία καταγραφής εφαρμογών.   

Είναι σημαντικό να λάβετε υπόψη το πλήθος των σημείων δεδομένων πρέπει να μεταφερθούν στην πραγματικότητα με διανομείς συμβάν. Συνήθως, οι προγραμματιστές μεταφέρετε δεδομένα ζεστού διαδρομή χαμηλής αδράνειας που πρέπει να που καταναλώθηκε και ερμηνεύεται γρήγορα. Τα συστήματα που παρακολουθεί τις ειδοποιήσεις ή autoscale κανόνων είναι παραδείγματα. Προγραμματιστής επίσης μπορεί να ρυθμίσετε τις παραμέτρους ενός χώρου αποθήκευσης εναλλακτική ανάλυση ή αναζήτηση store--για παράδειγμα, Azure ροή ανάλυση, Elasticsearch, ένα προσαρμοσμένο σύστημα παρακολούθησης ή μια αγαπημένη σύστημα παρακολούθησης από άλλους χρήστες.

Ακολουθούν ορισμένες ρυθμίσεις παραμέτρων παράδειγμα.

```
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
</PerformanceCounters>
```

Στο παρακάτω παράδειγμα, το δέκτη εφαρμόζεται ο γονικός κόμβος **PerformanceCounters** στην ιεραρχία, που σημαίνει όλων των θυγατρικών **PerformanceCounters** θα αποσταλούν συμβάν διανομείς.  

```
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```

Στο προηγούμενο παράδειγμα, το δέκτη εφαρμόζεται μόνο τρεις μετρητές: **Αιτήσεις σε ουρά**, **Οι αιτήσεις απορρίπτονται**και **% χρόνος επεξεργασίας**.  

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο προγραμματιστής να περιορίσετε την ποσότητα των απεσταλμένων δεδομένων για να την κρίσιμη μετρήσεις που χρησιμοποιούνται για αυτήν την υπηρεσία υγείας.  

```
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```

Σε αυτό το παράδειγμα, το δέκτη έχει εφαρμοστεί σε αρχεία καταγραφής και φιλτράρεται μόνο σε επίπεδο ανίχνευση σφάλματος.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Ανάπτυξη και να ενημερώσετε μια ρύθμισης παραμέτρων εφαρμογής και τα Διαγνωστικά υπηρεσίες Cloud

Visual Studio παρέχει τη διαδρομή ευκολότερος για την ανάπτυξη της εφαρμογής και ρύθμισης παραμέτρων δέκτη συμβάντος διανομείς. Για να προβάλετε και να επεξεργαστείτε το αρχείο, ανοίξτε το αρχείο *.wadcfgx* στο Visual Studio, επεξεργαστείτε και αποθηκεύστε το. Η διαδρομή είναι **Έργο υπηρεσία Cloud** > **ρόλους** > **(όνομα ρόλου)** > **diagnostics.wadcfgx**.  

Σε αυτό το σημείο, όλα ανάπτυξης και ανάπτυξη της ενημερωμένης έκδοσης ενέργειες στο Visual Studio, Visual Studio Team System, και όλες οι εντολές ή δέσμες ενεργειών που βασίζονται σε MSBuild και χρήση του **/t: δημοσίευση** προορισμού, συμπεριλάβετε το *.wadcfgx* στη διαδικασία συσκευασία. Επιπλέον, αναπτύξεις και ενημερώσεις αναπτύξετε το αρχείο σε Azure χρησιμοποιώντας την κατάλληλη επέκταση παράγοντας Διαγνωστικά Azure ΣΠΣ σας.

Μετά την ανάπτυξη της εφαρμογής και διαγνωστικά του Azure ρύθμισης παραμέτρων, θα δείτε αμέσως δραστηριότητας στον πίνακα εργαλείων του διανομέα συμβάν. Αυτό υποδεικνύει ότι είστε έτοιμοι για να μετακινηθείτε προβολή των δεδομένων ζεστού διαδρομή σε το εργαλείο προγράμματος-πελάτη ή ανάλυσης ακρόασης της επιλογής σας.  

Στην παρακάτω εικόνα, στον πίνακα εργαλείων διανομείς συμβάν εμφανίζεται άρτιο αποστολή δεδομένων Διαγνωστικά στο συμβάν διανομέα ξεκινώντας κάποτε μετά 11 Μ.Μ. Που είναι όταν η εφαρμογή έχει αναπτυχθεί με ένα αρχείο ενημερωμένων *.wadcfgx* και το δέκτη έχει ρυθμιστεί σωστά.

![][0]  

> [AZURE.NOTE] Όταν κάνετε ενημερώσεις για το αρχείο ρύθμισης παραμέτρων Διαγνωστικά Azure (.wadcfgx), συνιστάται να push τις ενημερωμένες εκδόσεις για ολόκληρη την εφαρμογή, καθώς και τη ρύθμιση παραμέτρων, χρησιμοποιώντας το Visual Studio δημοσίευσης ή μια δέσμη ενεργειών του Windows PowerShell.  

## <a name="view-hot-path-data"></a>Προβολή δεδομένων ζεστού διαδρομή

Όπως περιγράφεται παραπάνω, υπάρχουν πολλές περιπτώσεις χρήσης για ακρόαση και επεξεργασία δεδομένων συμβάν διανομείς.

Ένα απλό προσέγγιση είναι να δημιουργήσετε μια εφαρμογή κονσόλας μικρές δοκιμή για να ακούσετε την ενότητα συμβάντων και εκτύπωση στη ροή εξόδου. Μπορείτε να τοποθετήσετε τον παρακάτω κώδικα, η οποία εξηγείται με περισσότερες λεπτομέρειες σε [Γρήγορα αποτελέσματα με το συμβάν διανομείς](./event-hubs-csharp-ephcs-getstarted.md)), σε μια εφαρμογή κονσόλας.  

Σημειώστε ότι η εφαρμογή κονσόλας πρέπει να περιλαμβάνει το [πακέτο Nuget Host επεξεργαστής συμβάν](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Να θυμάστε ότι για να αντικαταστήσετε τις τιμές σε αγκύλες στη συνάρτηση **κύριες** με τιμές για τους πόρους σας.   

```
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event Hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sink"></a>Αντιμετώπιση προβλημάτων δέκτη διανομείς συμβάντος

- Ο διανομέας συμβάν δεν εμφανίζει Εισερχόμενα ή εξερχόμενα εκδήλωσης δραστηριότητας όπως αναμένεται.

    Ελέγξτε ότι το Κέντρο συμβάν σας παρέχεται με επιτυχία. Όλες οι πληροφορίες σύνδεσης στην ενότητα **PrivateConfig** του *.wadcfgx* πρέπει να συμφωνούν με τις τιμές του πόρου όπως φαίνεται στην πύλη. Βεβαιωθείτε ότι έχετε μια πολιτική συσχετισμών Ασφαλείας που ορίζονται από το ("SendRule" στο παράδειγμα) στην πύλη και ότι έχει εκχωρηθεί δικαίωμα *Αποστολή* .  

- Μετά από μια ενημέρωση, την ενότητα συμβάντων εμφανίζει δεν είναι πλέον Εισερχόμενα ή εξερχόμενα εκδήλωσης δραστηριότητας.

    Πρώτα, βεβαιωθείτε ότι τις πληροφορίες συμβάντων διανομέας και τη ρύθμιση παραμέτρων είναι σωστή, όπως εξηγείται προηγουμένως. Ορισμένες φορές το **PrivateConfig** γίνεται επαναφορά σε μια ενημερωμένη έκδοση ανάπτυξης. Το προτεινόμενο fix είναι να κάνετε όλων των αλλαγών στο *.wadcfgx* του έργου και, στη συνέχεια, push μια ενημερωμένη έκδοση πλήρους αίτησης. Εάν αυτό δεν είναι δυνατό, βεβαιωθείτε ότι η ενημέρωση Διαγνωστικά προωθεί μια ολοκληρωμένη **PrivateConfig** που περιλαμβάνει το πλήκτρο συσχετισμών Ασφαλείας.  

- Προσπάθησα τις προτάσεις και την ενότητα συμβάντων εξακολουθεί να μην λειτουργεί.

    Ανατρέξτε στον πίνακα αποθήκευσης Azure που περιέχει τα αρχεία καταγραφής και σφαλμάτων για τα Διαγνωστικά του Azure ίδια: **WADDiagnosticInfrastructureLogsTable**. Μία επιλογή είναι να χρησιμοποιήσετε ένα εργαλείο όπως [Azure Εξερεύνηση χώρου αποθήκευσης](http://www.storageexplorer.com) για να συνδεθείτε σε αυτόν το λογαριασμό χώρου αποθήκευσης, προβάλλετε αυτόν τον πίνακα και προσθήκη ενός ερωτήματος για χρονικής σήμανσης τις τελευταίες 24 ώρες. Μπορείτε να χρησιμοποιήσετε το εργαλείο για να εξαγάγετε ένα αρχείο .csv και να το ανοίξετε σε μια εφαρμογή όπως το Microsoft Excel. Excel διευκολύνει την αναζήτηση για τις συμβολοσειρές τηλεφωνικής πιστωτικής κάρτας, όπως **EventHubs**, για να δείτε ποιες σφάλμα αναφέρεται.  

## <a name="next-steps"></a>Επόμενα βήματα

• [Μάθετε περισσότερα σχετικά με το συμβάν διανομείς](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Προσάρτημα: Ολοκλήρωση παράδειγμα αρχείου (.wadcfgx) ρύθμισης παραμέτρων Διαγνωστικά του Azure

```
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount />
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="" key="" endpoint="" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Συμπληρωματική *ServiceConfiguration.Cloud.cscfg* για αυτό το παράδειγμα έχει τα εξής.

```
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

<!-- Images. -->
[0]: ./media/event-hubs-streaming-azure-diags-data/dashboard.png

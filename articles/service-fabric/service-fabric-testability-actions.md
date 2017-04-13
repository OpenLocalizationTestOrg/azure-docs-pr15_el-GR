<properties
   pageTitle="Η ενέργεια με δυνατότητα δοκιμής | Microsoft Azure"
   description="Σε αυτό το άρθρο ακρόασης σχετικά με τις ενέργειες με δυνατότητα δοκιμής που βρέθηκε στο Microsoft Azure Service ύφασμα."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="timlt"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/03/2016"
   ms.author="motanv;heeldin"/>

# <a name="testability-actions"></a>Ενέργειες με δυνατότητα δοκιμής
Για να προσομοιώσετε μια αναξιόπιστες υποδομή, ύφασμα υπηρεσίας Azure σάς παρέχει, τον προγραμματιστή, με τρόπους για να προσομοιώσετε διάφορες αποτυχίες ρεαλιστικό και μεταβάσεις κατάστασης. Εκτίθενται ως ενέργειες με δυνατότητα δοκιμής. Οι ενέργειες είναι τα API χαμηλού επιπέδου που προκαλούν ένα συγκεκριμένο σφάλμα εισαγωγής, μετάβαση κατάστασης ή επικύρωσης. Με το συνδυασμό αυτές τις ενέργειες, μπορείτε να συντάξετε σενάρια πλήρη έλεγχο για τις υπηρεσίες σας.

Υπηρεσία ύφασμα παρέχει ορισμένα κοινά σενάρια δοκιμής που αποτελείται από αυτές τις ενέργειες. Προτείνουμε να που χρησιμοποιούν αυτές τις ενσωματωμένες σενάρια, την οποία επιλέγονται προσεκτικά για να ελέγξετε κοινές μεταβάσεις κατάστασης και τα κουτιά αποτυχία. Ωστόσο, οι ενέργειες μπορεί να χρησιμοποιηθεί για τη δημιουργία σενάρια προσαρμοσμένου δοκιμών κατά την οποία θέλετε να προσθέσετε κάλυψης για σενάρια που δεν καλύπτεται από τα ενσωματωμένα σενάρια ακόμη ή που είναι προσαρμοσμένη προσαρμοσμένες για την εφαρμογή σας.

C# υλοποιήσεις από τις ενέργειες που βρίσκονται σε συγκρότησης System.Fabric.dll. Της λειτουργικής μονάδας PowerShell ύφασμα συστήματος βρίσκεται στο συγκρότησης Microsoft.ServiceFabric.Powershell.dll. Ως μέρος της εγκατάστασης του χρόνου εκτέλεσης, εγκαθίσταται της λειτουργικής μονάδας ServiceFabric PowerShell για να επιτρέψετε για μεγαλύτερη ευκολία στη χρήση.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Φυσιολογική έναντι ενέργειες ungraceful σφαλμάτων
Ενέργειες με δυνατότητα δοκιμής ταξινομούνται σε δύο κύριες λίστες παλαιών αρχείων:

* Ungraceful σφάλματα: αυτά τα σφάλματα προσομοίωση αποτυχίες όπως επανεκκίνηση του υπολογιστή και την επεξεργασία παρουσιάσει σφάλμα. Σε αυτές τις περιπτώσεις αποτυχιών, το περιβάλλον εκτέλεσης της διαδικασίας τερματίζεται απότομα. Αυτό σημαίνει ότι δεν υπάρχει εκκαθάριση του μέλους είναι δυνατό να εκτελεστεί πριν από την εκκίνηση της εφαρμογής ξανά.

* Φυσιολογική σφάλματα: αυτά τα σφάλματα προσομοίωση φυσιολογική ενέργειες όπως ρεπλίκα μετακινεί και πτώσεις που ενεργοποιείται κάνοντας εξισορρόπηση φόρτου. Σε αυτές τις περιπτώσεις, η υπηρεσία λαμβάνει μια ειδοποίηση από το κλείσιμο και να εκκαθαρίσετε το μέλος πριν από την έξοδο.

Για καλύτερη ποιότητα επικύρωσης, εκτελέστε την υπηρεσία και τα επαγγελματικά φόρτο εργασίας κατά την πρόκληση διάφορες φυσιολογική και ungraceful σφάλματα. Σφάλματα ungraceful να σενάρια όπου η διαδικασία της υπηρεσίας τερματίζεται απότομα στο μέσο ορισμένες ροής εργασίας. Αυτό ελέγχει τη διαδρομή αποκατάστασης όταν η υπηρεσία ρεπλίκα γίνεται επαναφορά από ύφασμα υπηρεσίας. Αυτό θα σας βοηθήσει δοκιμή συνέπεια δεδομένων και εάν η κατάσταση υπηρεσίας διατηρείται σωστά μετά από αποτυχίες. Το άλλο σύνολο αποτυχιών (φυσιολογική αποτυχίες) Ελέγξτε ότι η υπηρεσία απαντά σωστά αντίγραφα μετακινούνται από ύφασμα υπηρεσίας. Αυτό ελέγχει χειρισμού της ακύρωσης στη μέθοδο RunAsync. Η υπηρεσία πρέπει να ελέγξετε για το διακριτικό ακύρωσης τη ρύθμιση, αποθήκευση σωστά στην κατάσταση και έξοδος από τη μέθοδο RunAsync.

## <a name="testability-actions-list"></a>Λίστα ενεργειών με δυνατότητα δοκιμής

| Ενέργεια | Περιγραφή | Διαχειριζόμενο API | Cmdlet του PowerShell | Φυσιολογική ungraceful σφάλματα |
|---------|-------------|-------------|-------------------|------------------------------|
|CleanTestState| Καταργεί όλα τα κατάσταση δοκιμής από το σύμπλεγμα σε περίπτωση εσφαλμένο τερματισμό του προγράμματος οδήγησης της δοκιμής. | CleanTestStateAsync | Κατάργηση ServiceFabricTestState | Δεν ισχύει |
| InvokeDataLoss | Υποδείξει απώλεια δεδομένων σε μια υπηρεσία διαμερίσματα. | InvokeDataLossAsync | Κλήση ServiceFabricPartitionDataLoss | Φυσιολογική |
| InvokeQuorumLoss | Εισάγει ένα διαμερίσματα δεδομένη κατάστασης υπηρεσίας στην απώλεια απαρτίας. | InvokeQuorumLossAsync | Κλήση ServiceFabricQuorumLoss | Φυσιολογική |
| Μετακίνηση κύρια | Μετακινεί το καθορισμένο κύρια ρεπλίκα της κατάστασης υπηρεσίας ο καθορισμένος κόμβος συμπλέγματος. | MovePrimaryAsync | Μετακίνηση ServiceFabricPrimaryReplica | Φυσιολογική |
| Μετακίνηση δευτερεύοντος | Μετακινεί την τρέχουσα δευτερεύοντα ρεπλίκα της κατάστασης υπηρεσίας σε έναν κόμβο διαφορετικό συμπλέγματος. | MoveSecondaryAsync | Μετακίνηση ServiceFabricSecondaryReplica | Φυσιολογική |
| RemoveReplica | Προσομοιώνει αποτυχία ρεπλίκα με την κατάργηση ενός αντιγράφου από ένα σύμπλεγμα. Αυτό θα κλείσει η ρεπλίκα και θα μετάβασης σε ρόλο "Καμία", η κατάργηση της κατάστασής από το σύμπλεγμα. | RemoveReplicaAsync | Κατάργηση ServiceFabricReplica | Φυσιολογική |
| RestartDeployedCodePackage | Προσομοιώνει αποτυχία διαδικασία πακέτου κώδικα με την επανεκκίνηση ένα πακέτο κώδικα που έχει αναπτυχθεί σε έναν κόμβο σε ένα σύμπλεγμα. Αυτό ματαίωση διαδικασίας πακέτου κώδικα, η οποία θα γίνει επανεκκίνηση όλα τα αντίγραφα του χρήστη υπηρεσίας φιλοξενούνται σε αυτήν τη διαδικασία. | RestartDeployedCodePackageAsync | Επανεκκίνηση ServiceFabricDeployedCodePackage | Ungraceful |
| RestartNode | Προσομοιώνει αποτυχία ύφασμα υπηρεσιών συμπλέγματος κόμβο με την επανεκκίνηση έναν κόμβο. | RestartNodeAsync | Επανεκκίνηση ServiceFabricNode | Ungraceful |
| RestartPartition | Προσομοιώνει ένα κέντρο δεδομένων σενάριο blackout blackout ή σύμπλεγμα με την επανεκκίνηση αντίγραφα ορισμένων ή όλων των ένα διαμερίσματα. | RestartPartitionAsync | Επανεκκίνηση ServiceFabricPartition | Φυσιολογική |
| RestartReplica | Προσομοιώνει αποτυχία ρεπλίκα επανεκκίνηση μόνιμων ρεπλίκα σε ένα σύμπλεγμα, κλείνοντας στη ρεπλίκα και, στη συνέχεια, να το ανοίξετε. | RestartReplicaAsync | ServiceFabricReplica επανεκκίνηση | Φυσιολογική |
| StartNode | Ξεκινά έναν κόμβο σε ένα σύμπλεγμα που ήδη έχει διακοπεί. | StartNodeAsync | Έναρξη-ServiceFabricNode | Δεν ισχύει |
| StopNode | Προσομοιώνει αποτυχία κόμβο με διακοπή έναν κόμβο σε ένα σύμπλεγμα. Ο κόμβος θα παραμείνει προς τα κάτω, μέχρι να ονομάζεται StartNode. | StopNodeAsync | Διακοπή ServiceFabricNode | Ungraceful |
| ValidateApplication | Επαληθεύει τη διαθεσιμότητα και την εύρυθμη λειτουργία της όλες τις υπηρεσίες ύφασμα υπηρεσίας μέσα σε μια εφαρμογή, συνήθως μετά την πρόκληση ορισμένες σφαλμάτων στο σύστημα. | ValidateApplicationAsync | Δοκιμή ServiceFabricApplication | Δεν ισχύει |
| ValidateService | Επαληθεύει τη διαθεσιμότητα και την εύρυθμη λειτουργία της υπηρεσίας εξυπηρέτησης ύφασμα, συνήθως μετά την πρόκληση ορισμένες σφαλμάτων στο σύστημα. | ValidateServiceAsync | Δοκιμή ServiceFabricService | Δεν ισχύει |

## <a name="running-a-testability-action-using-powershell"></a>Εκτελεί μια ενέργεια με δυνατότητα δοκιμής με χρήση του PowerShell

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να εκτελέσετε μια ενέργεια με δυνατότητα δοκιμής με χρήση του PowerShell. Θα μάθετε πώς μπορείτε να εκτελέσετε μια ενέργεια με δυνατότητα δοκιμής σε ένα τοπικό σύμπλεγμα (ένα πλαίσιο) ή σε ένα σύμπλεγμα Azure. Microsoft.Fabric.Powershell.dll--τη λειτουργική μονάδα υπηρεσίας ύφασμα PowerShell--εγκαθίσταται αυτόματα κατά την εγκατάσταση του Microsoft υπηρεσίας ύφασμα MSI. Η λειτουργική μονάδα φορτώνεται αυτόματα όταν ανοίγετε μια γραμμή εντολών του PowerShell.

Εκμάθηση τμήματα:

- [Εκτέλεση μιας ενέργειας σε ένα σύμπλεγμα ένα πλαίσιο](#run-an-action-against-a-one-box-cluster)
- [Εκτέλεση μιας ενέργειας σε ένα σύμπλεγμα Azure](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Εκτέλεση μιας ενέργειας σε ένα σύμπλεγμα ένα πλαίσιο

Για να εκτελέσετε μια ενέργεια με δυνατότητα δοκιμής σε σχέση με ένα τοπικό σύμπλεγμα, συνδεθείτε με το σύμπλεγμα και ανοίξτε τη γραμμή εντολών του PowerShell σε κατάσταση λειτουργίας διαχειριστή. Ας εξετάσουμε την ενέργεια **ServiceFabricNode επανεκκίνηση** .

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Δείτε την ενέργεια **Επανεκκίνηση ServiceFabricNode** εκτελείται σε έναν κόμβο με το όνομα "Node1". Η λειτουργία ολοκλήρωσης καθορίζει ότι το θα πρέπει να δεν επιβεβαιώσει αν η ενέργεια επανεκκίνηση κόμβου στην πραγματικότητα ολοκληρώθηκε με επιτυχία. Καθορίζοντας τη λειτουργία ολοκλήρωσης, όπως "Verify" θα προκαλέσει την για να επιβεβαιώσετε αν η ενέργεια επανεκκίνηση στην πραγματικότητα ολοκληρώθηκε με επιτυχία. Αντί να προσδιορίζεται απευθείας στον κόμβο με το όνομά του, μπορείτε να το καθορίσετε μέσω έναν αριθμό-κλειδί διαμερίσματα και το είδος του ρεπλίκα, ως εξής:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Επανεκκίνηση ServiceFabricNode** πρέπει να χρησιμοποιείται για να επανεκκινήσετε έναν κόμβο ύφασμα υπηρεσίας σε ένα σύμπλεγμα. Αυτό θα διακόψει τη διαδικασία Fabric.exe, η οποία θα γίνει επανεκκίνηση όλων των το σύστημα υπηρεσίας και χρήστη υπηρεσίας αντίγραφα φιλοξενούνται σε αυτόν τον κόμβο. Χρησιμοποιώντας αυτό το API για να ελέγξετε την υπηρεσία βοηθά να αποκαλύψετε σφάλματα κατά μήκος των διαδρομών αποκατάστασης ανακατεύθυνσης. Σας βοηθά να προσομοιώσετε αποτυχίες κόμβο του συμπλέγματος.

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει την εντολή **Επανεκκίνηση ServiceFabricNode** με δυνατότητα δοκιμής στην πράξη.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Το αποτέλεσμα από την πρώτη **Get-ServiceFabricNode** (ένα cmdlet από τη λειτουργική μονάδα υπηρεσίας ύφασμα PowerShell) δείχνει ότι το τοπικό σύμπλεγμα έχει πέντε κόμβοι: Node.1 να Node.5. Μετά την εκτέλεση της ενέργειας με δυνατότητα δοκιμής (cmdlet) **Επανεκκίνηση ServiceFabricNode** στον κόμβο, με το όνομα Node.4, θα δούμε ότι έχει γίνει επαναφορά συνεχούς τον κόμβο.

### <a name="run-an-action-against-an-azure-cluster"></a>Εκτέλεση μιας ενέργειας σε ένα σύμπλεγμα Azure

Εκτελεί μια ενέργεια με δυνατότητα δοκιμής (με χρήση του PowerShell) σε σχέση με ένα σύμπλεγμα Azure είναι παρόμοια με την εκτέλεση της ενέργειας σε σχέση με ένα τοπικό σύμπλεγμα. Η μόνη διαφορά είναι ότι μπορέσετε να εκτελέσετε την ενέργεια, αντί για τη σύνδεση με το τοπικό σύμπλεγμα, πρέπει πρώτα να συνδεθείτε με το Azure σύμπλεγμα.

## <a name="running-a-testability-action-using-c35"></a>Εκτελεί μια ενέργεια με δυνατότητα δοκιμής με C & #35;

Για να εκτελέσετε μια ενέργεια με δυνατότητα δοκιμής χρησιμοποιώντας C#, πρέπει πρώτα να συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας FabricClient. Στη συνέχεια, αποκτήστε τις παραμέτρους που απαιτούνται για την εκτέλεση της ενέργειας. Διαφορετικές παραμέτρους μπορεί να χρησιμοποιηθεί για να εκτελέσετε την ίδια ενέργεια.
Εξετάζοντας την ενέργεια RestartServiceFabricNode, ένας τρόπος για να το εκτελέσετε είναι χρησιμοποιώντας τις πληροφορίες κόμβου (όνομα κόμβου και Αναγνωριστικό παρουσίας κόμβου) στο σύμπλεγμα.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Εξήγηση παραμέτρου:

- **CompleteMode** Καθορίζει ότι η κατάσταση λειτουργίας πρέπει να επιβεβαιώσει δεν εάν η ενέργεια επανεκκίνηση στην πραγματικότητα ολοκληρώθηκε με επιτυχία. Καθορίζοντας τη λειτουργία ολοκλήρωσης, όπως "Verify" θα προκαλέσει την για να επιβεβαιώσετε αν η ενέργεια επανεκκίνηση στην πραγματικότητα ολοκληρώθηκε με επιτυχία.  
- **OperationTimeout** ορίζει το χρονικό διάστημα για τη λειτουργία για να ολοκληρωθεί πριν από μια εξαίρεση TimeoutException.
- **CancellationToken** επιτρέπει σε εκκρεμότητα κλήσης για να την ακυρώσετε.

Αντί να προσδιορίζεται απευθείας στον κόμβο με το όνομά του, μπορείτε να το καθορίσετε μέσω έναν αριθμό-κλειδί διαμερίσματα και το είδος του αντιγράφου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [PartitionSelector και ReplicaSelector](#partition_replica_selector).


```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector και ReplicaSelector

### <a name="partitionselector"></a>PartitionSelector
PartitionSelector είναι ένα πρόγραμμα βοήθειας που εκτίθενται στο με δυνατότητα δοκιμής και χρησιμοποιείται για να επιλέξετε ένα συγκεκριμένο διαμερίσματα στο οποίο θέλετε να εκτελέσετε οποιαδήποτε από τις ενέργειες με δυνατότητα δοκιμής. Μπορεί να χρησιμοποιηθεί για να επιλέξετε ένα συγκεκριμένο διαμερίσματα εάν το Αναγνωριστικό διαμερίσματα είναι γνωστό εκ των προτέρων. Ή, μπορείτε να παρέχετε το κλειδί διαμερίσματα και τη λειτουργία θα επιλύσει το Αναγνωριστικό partition εσωτερικά. Έχετε επίσης τη δυνατότητα επιλογής έναν τυχαίο διαμερίσματα.

Για να χρησιμοποιήσετε αυτό το βοηθητικό πρόγραμμα, δημιουργήστε το αντικείμενο PartitionSelector και επιλέξτε τα διαμερίσματα χρησιμοποιώντας μία από τις μεθόδους επιλέξτε *. Στη συνέχεια, Πέρασμα στο αντικείμενο PartitionSelector για το API που απαιτεί το. Εάν η επιλογή δεν είναι ενεργοποιημένη, από προεπιλογή επιλέγεται μια τυχαία διαμερίσματα.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector είναι ένα πρόγραμμα βοήθειας που εκτίθενται στο με δυνατότητα δοκιμής και χρησιμοποιείται για να σας βοηθήσει να επιλέξετε μια ρεπλίκα στην οποία θέλετε να εκτελέσετε οποιαδήποτε από τις ενέργειες με δυνατότητα δοκιμής. Μπορεί να χρησιμοποιηθεί για να επιλέξετε μια συγκεκριμένη ρεπλίκα εάν το Αναγνωριστικό ρεπλίκα είναι γνωστό εκ των προτέρων. Επιπλέον, έχετε τη δυνατότητα επιλογής μια κύρια ρεπλίκα ή ένα δευτερεύον τυχαία. ReplicaSelector προέρχεται από PartitionSelector, ώστε να πρέπει να επιλέξετε τη ρεπλίκα και τα διαμερίσματα στην οποία θέλετε να εκτελέσετε τη λειτουργία με δυνατότητα δοκιμής.

Για να χρησιμοποιήσετε αυτό το βοηθητικό πρόγραμμα, δημιουργήστε ένα αντικείμενο ReplicaSelector και ορίστε τον τρόπο που θέλετε να επιλέξετε τη ρεπλίκα και τα διαμερίσματα. Στη συνέχεια, μπορείτε να το περάσετε σε το API που απαιτεί το. Εάν η επιλογή δεν είναι ενεργοποιημένη, από προεπιλογή σε τυχαία ρεπλίκα και τυχαία διαμερίσματα.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Επόμενα βήματα

- [Σενάρια με δυνατότητα δοκιμής](service-fabric-testability-scenarios.md)
- Πώς μπορείτε να δοκιμάσετε την υπηρεσία
   - [Προσομοίωση αποτυχίες κατά τη διάρκεια της υπηρεσίας φόρτους εργασίας](service-fabric-testability-workload-tests.md)
   - [Επικοινωνία με υπηρεσία αποτυχίες](service-fabric-testability-scenarios-service-communication.md)

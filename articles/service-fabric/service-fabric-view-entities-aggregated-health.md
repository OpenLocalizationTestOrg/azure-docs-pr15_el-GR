<properties
   pageTitle="Πώς μπορείτε να προβάλετε Azure Service ύφασμα οντοτήτων συναθροιστεί εύρυθμης λειτουργίας | Microsoft Azure"
   description="Περιγράφει τον τρόπο ερωτήματος, προβολή και αξιολόγηση Azure Service ύφασμα οντοτήτων συγκεντρωτική εύρυθμης λειτουργίας, μέσω εύρυθμης λειτουργίας ερωτημάτων και γενικά ερωτημάτων."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Προβολή αναφορών εύρυθμης λειτουργίας υπηρεσίας ύφασμα
Azure Service ύφασμα παρουσιάζει ένα [μοντέλο εύρυθμης λειτουργίας](service-fabric-health-introduction.md) που αποτελείται από το σύστημα στοιχεία και watchdogs μπορούν να αναφέρουν τοπικό συνθήκες που τους παρακολούθηση οντοτήτων εύρυθμης λειτουργίας. Η [Αποθήκευση εύρυθμης λειτουργίας](service-fabric-health-introduction.md#health-store) συγκεντρώνει όλα τα δεδομένα εύρυθμης λειτουργίας για να προσδιορίσετε αν οντοτήτων είναι σε καλή κατάσταση.

Έξοδος από το πλαίσιο, το σύμπλεγμα συμπληρώνεται με αναφορές εύρυθμης λειτουργίας που αποστέλλονται από τα στοιχεία του συστήματος. Διαβάστε περισσότερα στο [Χρήση αναφορών εύρυθμης λειτουργίας συστήματος για την αντιμετώπιση προβλημάτων](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Υπηρεσία ύφασμα παρέχει πολλούς τρόπους για να λάβετε το πλήθος των συγκεντρωτικών αποτελεσμάτων εύρυθμη λειτουργία των οντοτήτων:

- [Εξερεύνηση ύφασμα υπηρεσίας](service-fabric-visualizing-your-cluster.md) ή άλλα εργαλεία απεικόνισης

- Εύρυθμης λειτουργίας ερωτημάτων (μέσω του PowerShell, το API ή REST)

- Γενικά ερωτήματα που επιστρέφεται μια λίστα οντοτήτων που έχουν εύρυθμης λειτουργίας με μία από τις ιδιότητες (μέσω του PowerShell, το API ή REST)

Για μια επίδειξη αυτές τις επιλογές, ας χρησιμοποιήσουμε ένα τοπικό σύμπλεγμα με πέντε κόμβους. Δίπλα στο το **ύφασμα: / σύστημα** εφαρμογή (το οποίο υπάρχει έξοδος από το πλαίσιο), αναπτύσσονται κάποιες άλλες εφαρμογές. Μία από αυτές τις εφαρμογές είναι **ύφασμα: / WordCount**. Αυτή η εφαρμογή περιέχει κατάστασης υπηρεσίας έχουν ρυθμιστεί με επτά αντίγραφα. Επειδή υπάρχουν μόνο πέντε κόμβους, τα στοιχεία του συστήματος εμφανίζουν μια προειδοποίηση που τα διαμερίσματα βρίσκεται κάτω από το πλήθος προορισμού.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Εύρυθμη λειτουργία στην Εξερεύνηση ύφασμα υπηρεσίας
Υπηρεσία ύφασμα Explorer παρέχει μια οπτική προβολή του συμπλέγματος. Στην παρακάτω εικόνα, μπορείτε να δείτε που:

- Η εφαρμογή **ύφασμα: / WordCount** είναι κόκκινο (στο σφάλμα), επειδή έχει ένα συμβάν σφάλματος αναφέρεται από **MyWatchdog** για την ιδιότητα **διαθεσιμότητας**.

- Μία από τις υπηρεσίες, **ύφασμα: / WordCount/WordCountService** είναι κίτρινο (σε προειδοποίηση). Επειδή η υπηρεσία έχει ρυθμιστεί με επτά αντίγραφα, αυτές δεν είναι δυνατό να τοποθετηθούν, καθώς υπάρχουν μόνο πέντε κόμβους. Παρόλο που δεν εμφανίζεται εδώ, τα διαμερίσματα υπηρεσίας είναι κίτρινο λόγω της αναφοράς συστήματος. Το κίτρινο partition ενεργοποιεί την υπηρεσία κίτρινο.

- Το σύμπλεγμα είναι κόκκινο λόγω της εφαρμογής κόκκινο.

Η αξιολόγηση χρησιμοποιεί προεπιλεγμένες πολιτικές από το σύμπλεγμα δηλωτικό και δήλωση εφαρμογής. Είναι αντικειμενική πολιτικές και όχι να αποδεχτεί τις σε περίπτωση αποτυχίας.

Προβολή του συμπλέγματος με την Εξερεύνηση ύφασμα υπηρεσίας:

![Προβολή του συμπλέγματος με την Εξερεύνηση ύφασμα υπηρεσίας.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Διαβάστε περισσότερα σχετικά με την [Υπηρεσία ύφασμα Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Εύρυθμης λειτουργίας ερωτημάτων
Υπηρεσία ύφασμα εκθέτει εύρυθμης λειτουργίας ερωτημάτων για κάθε έναν από τους υποστηριζόμενοι [τύποι οντότητα](service-fabric-health-introduction.md#health-entities-and-hierarchy). Αυτές είναι δυνατή η πρόσβαση μέσω του API (τις μεθόδους μπορεί να βρεθεί στην **FabricClient.HealthManager**), cmdlet του PowerShell και ΥΠΌΛΟΙΠΟ. Αυτά τα ερωτήματα επιστρέφουν ολοκλήρωσης εύρυθμης λειτουργίας πληροφορίες σχετικά με την οντότητα: το συγκεντρωτική εύρυθμης λειτουργίας, συμβάντα εύρυθμης λειτουργίας οντότητα, μέλη εύρυθμη λειτουργία των θυγατρικών (όταν υπάρχει) και κατεστραμμένες αξιολογήσεις όταν η οντότητα δεν είναι σε καλή κατάσταση.

> [AZURE.NOTE] Μια οντότητα εύρυθμης λειτουργίας επιστρέφεται όταν συμπληρώνεται πλήρως στο χώρο αποθήκευσης εύρυθμης λειτουργίας. Η οντότητα πρέπει να είναι ενεργό (χωρίς να διαγράψετε) και να έχετε μια αναφορά του συστήματος. Το γονικό στοιχείο οντοτήτων για την ιεραρχία αλυσίδα πρέπει επίσης να περιέχουν αναφορές του συστήματος. Εάν οποιαδήποτε από αυτές τις συνθήκες δεν πληρούνται, τα ερωτήματα εύρυθμης λειτουργίας επιστρέφουν μια εξαίρεση που εμφανίζει για ποιο λόγο δεν επιστρέφεται η οντότητα.

Τα ερωτήματα εύρυθμης λειτουργίας πρέπει να περάσει στο το αναγνωριστικό οντότητα, η οποία εξαρτάται από τον τύπο οντότητας. Τα ερωτήματα δέχονται παραμέτρους πολιτικής προαιρετικό εύρυθμης λειτουργίας. Εάν έχουν καθοριστεί χωρίς πολιτικές εύρυθμης λειτουργίας, τις [πολιτικές εύρυθμης λειτουργίας](service-fabric-health-introduction.md#health-policies) από το δηλωτικό σύμπλεγμα ή την εφαρμογή που χρησιμοποιούνται για την αξιολόγηση. Τα ερωτήματα δέχονται επίσης φίλτρων για την επιστροφή μόνο μερικές θυγατρικά στοιχεία ή συμβάντα--αυτά που ακολουθούν τα καθορισμένα φίλτρα.

> [AZURE.NOTE] Τα φίλτρα εξόδου εφαρμόζονται από την πλευρά του διακομιστή, ώστε να μειωθεί το μέγεθος του μηνύματος απάντησης. Συνιστάται η ότι μπορείτε χρησιμοποιήσετε τα φίλτρα εξόδου για να περιορίσετε τα δεδομένα που επιστρέφονται, αντί να εφαρμόσετε φίλτρα στην πλευρά του προγράμματος-πελάτη.

Περιέχει μια οντότητα εύρυθμης λειτουργίας:

- Η κατάσταση συγκεντρωτική εύρυθμης λειτουργίας της οντότητας. Υπολογιστεί από το χώρο αποθήκευσης εύρυθμης λειτουργίας που βασίζονται σε αναφορές εύρυθμης λειτουργίας οντότητα, μέλη εύρυθμη λειτουργία των θυγατρικών (όταν υπάρχει) και πολιτικές εύρυθμης λειτουργίας. Διαβάστε περισσότερα σχετικά με την [αξιολόγηση εύρυθμης λειτουργίας οντότητα](service-fabric-health-introduction.md#entity-health-evaluation).  

- Τα συμβάντα εύρυθμης λειτουργίας στην οντότητα.

- Η συλλογή των μελών εύρυθμης λειτουργίας για όλα τα θυγατρικά στοιχεία για τα πρόσωπα που μπορούν να έχουν θυγατρικά στοιχεία. Τα μέλη εύρυθμης λειτουργίας περιέχει αναγνωριστικά οντότητα και την κατάσταση εύρυθμης λειτουργίας συγκέντρωσης. Για να ολοκληρωθεί εύρυθμης λειτουργίας για θυγατρικό, κλήση της εύρυθμης λειτουργίας ερωτήματος για τον τύπο οντότητας θυγατρικό και μεταβιβάζουν στο θυγατρικό αναγνωριστικό.

- Κατεστραμμένη αξιολόγησης που οδηγούν στην έκθεση που την ενεργοποίησε την κατάσταση της οντότητας, εάν η οντότητα δεν είναι σε καλή κατάσταση.

## <a name="get-cluster-health"></a>Λήψη εύρυθμης λειτουργίας συμπλέγματος
Επιστρέφει την εύρυθμη λειτουργία της οντότητας σύμπλεγμα και περιέχει τα μέλη εύρυθμη λειτουργία των εφαρμογών και κόμβους (θυγατρικά στοιχεία του συμπλέγματος). Εισαγωγή δεδομένων:

- [Προαιρετικό] Η πολιτική εύρυθμης λειτουργίας συμπλέγματος που χρησιμοποιείται για την αξιολόγηση τους κόμβους και τα συμβάντα σύμπλεγμα.

- [Προαιρετικό] Η εφαρμογή εύρυθμης λειτουργίας πολιτικής χάρτη, με τις πολιτικές εύρυθμης λειτουργίας που χρησιμοποιείται για να παρακάμψετε τις πολιτικές δήλωσης εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα, κόμβους και εφαρμογές που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα, κόμβους και εφαρμογές που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε σύμπλεγμα εύρυθμης λειτουργίας, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) το **HealthManager**.

Η ακόλουθη κλήση λαμβάνει την εύρυθμη λειτουργία συμπλέγματος:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία συμπλέγματος, χρησιμοποιώντας μια πολιτική εύρυθμης λειτουργίας προσαρμοσμένο σύμπλεγμα και φίλτρα για κόμβους και εφαρμογές. Δημιουργεί [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), που περιέχει τις πληροφορίες εισόδου.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία συμπλέγματος είναι [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Η κατάσταση του συμπλέγματος είναι πέντε κόμβους, εφαρμογή συστήματος και ύφασμα: / WordCount ρυθμιστεί όπως περιγράφεται.

Το ακόλουθο cmdlet λαμβάνει εύρυθμης λειτουργίας σύμπλεγμα χρησιμοποιώντας προεπιλεγμένες πολιτικές εύρυθμης λειτουργίας. Την κατάσταση εύρυθμης λειτουργίας συγκεντρωτική βρίσκεται σε προειδοποίηση, επειδή υφάσματος: / WordCount εφαρμογή είναι σε προειδοποίηση. Παρατηρήστε πώς το κατεστραμμένες αξιολογήσεις παρέχονται λεπτομέρειες σχετικά με τις συνθήκες που την ενεργοποίησε την εύρυθμη λειτουργία της συγκέντρωσης.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Το ακόλουθο cmdlet του PowerShell λαμβάνει την εύρυθμη λειτουργία του συμπλέγματος, χρησιμοποιώντας μια προσαρμοσμένη εφαρμογή πολιτικής. Φιλτράρισμα αποτελεσμάτων για να λάβετε μόνο σφάλμα ή προειδοποίηση εφαρμογές και κόμβους. Ως αποτέλεσμα, δεν υπάρχει κόμβους επιστρέφονται, όπως είναι όλα σε καλή κατάσταση. Μόνο υφάσματος: / WordCount εφαρμογής ακολουθεί το φίλτρο εφαρμογές. Επειδή η προσαρμοσμένη πολιτική καθορίζει πρέπει να λάβετε υπόψη τις προειδοποιήσεις ως σφάλματα για υφάσματος: / WordCount εφαρμογής, η εφαρμογή αξιολογείται όπως στο σφάλμα και, επομένως, είναι το σύμπλεγμα.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας συμπλέγματος με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707669.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707696.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-node-health"></a>Λήψη εύρυθμης λειτουργίας κόμβου
Επιστρέφει την εύρυθμη λειτουργία μιας οντότητας κόμβο και περιέχει τα συμβάντα εύρυθμης λειτουργίας που αναφέρεται στον κόμβο. Εισαγωγή δεδομένων:

- [Απαιτείται] Το όνομα κόμβου που προσδιορίζει τον κόμβο.

- [Προαιρετικό] Το σύμπλεγμα εύρυθμης λειτουργίας ρυθμίσεις πολιτικής χρησιμοποιούνται για την αξιολόγηση της υγείας.

- [Προαιρετικό] Φίλτρα για συμβάντα που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε εύρυθμης λειτουργίας κόμβου μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) το HealthManager.

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία κόμβου για το καθορισμένο κόμβο όνομα:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία κόμβου για το καθορισμένο κόμβο όνομα και μεταφέρει στο φίλτρο συμβάντων και προσαρμοσμένης πολιτικής μέσω [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx):

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία κόμβου είναι [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .
Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία κόμβου χρησιμοποιώντας προεπιλεγμένες πολιτικές εύρυθμης λειτουργίας:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία της όλους τους κόμβους του συμπλέγματος:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας κόμβο με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707650.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707665.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-application-health"></a>Λήψη εφαρμογής εύρυθμης λειτουργίας
Επιστρέφει την εύρυθμη λειτουργία της εφαρμογής οντότητα. Περιέχει τα μέλη εύρυθμης λειτουργίας της εφαρμογής και θυγατρικά στοιχεία της υπηρεσίας. Εισαγωγή δεδομένων:

- [Απαιτείται] Το όνομα της εφαρμογής (URI) που προσδιορίζει την εφαρμογή.

- [Προαιρετικό] Η πολιτική εύρυθμης λειτουργίας εφαρμογής που χρησιμοποιείται για να παρακάμψετε τις πολιτικές δήλωσης εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα, τις υπηρεσίες και ανάπτυξη εφαρμογών που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα, τις υπηρεσίες και ανάπτυξη εφαρμογών που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε την εύρυθμη λειτουργία της εφαρμογής, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) το HealthManager.

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία της εφαρμογής για το καθορισμένο όνομα εφαρμογής (URI):

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία της εφαρμογής για το καθορισμένο όνομα εφαρμογής (URI), με τα φίλτρα και προσαρμοσμένες πολιτικές που καθορίζεται μέσω [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx).

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία της εφαρμογής είναι [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Το ακόλουθο cmdlet επιστρέφει την εύρυθμη λειτουργία της το **ύφασμα: / WordCount** εφαρμογής:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Το ακόλουθο cmdlet του PowerShell μεταφέρει στις προσαρμοσμένες πολιτικές. Φιλτράρει επίσης θυγατρικά στοιχεία και συμβάντα.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας εφαρμογής με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707681.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707643.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-service-health"></a>Λήψη εύρυθμη λειτουργία υπηρεσιών
Επιστρέφει την εύρυθμη λειτουργία υπηρεσίας οντότητας. Περιέχει τα διαμερίσματα μέλη εύρυθμης λειτουργίας. Εισαγωγή δεδομένων:

- [Απαιτείται] Το όνομα της υπηρεσίας (URI) που προσδιορίζει την υπηρεσία.

- [Προαιρετικό] Την πολιτική εύρυθμης λειτουργίας εφαρμογής που χρησιμοποιούνται για την παράκαμψη της δήλωσης πολιτικής εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα και τα διαμερίσματα που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα και τα διαμερίσματα που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε την εύρυθμη λειτουργία των υπηρεσιών μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) το HealthManager.

Το παρακάτω παράδειγμα λαμβάνει την εύρυθμη λειτουργία της υπηρεσίας με το καθορισμένο όνομα της υπηρεσίας (URI):

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Ο ακόλουθος κώδικας λαμβάνει την εύρυθμη λειτουργία υπηρεσίας για το καθορισμένο όνομα της υπηρεσίας (URI), καθορίζοντας φίλτρα και την προσαρμοσμένη πολιτική μέσω [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx):

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία των υπηρεσιών είναι [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία υπηρεσίας, χρησιμοποιώντας προεπιλεγμένες πολιτικές εύρυθμης λειτουργίας:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμη λειτουργία υπηρεσιών με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707609.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707646.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-partition-health"></a>Λήψη partition εύρυθμης λειτουργίας
Επιστρέφει την εύρυθμη λειτουργία μιας οντότητας διαμερίσματα. Περιέχει τα μέλη εύρυθμης λειτουργίας αντιγράφου. Εισαγωγή δεδομένων:

- [Απαιτείται] Τα διαμερίσματα Αναγνωριστικό (GUID) που προσδιορίζει τα διαμερίσματα.

- [Προαιρετικό] Την πολιτική εύρυθμης λειτουργίας εφαρμογής που χρησιμοποιούνται για την παράκαμψη της δήλωσης πολιτικής εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα και αντίγραφα που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα και αντίγραφα που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε partition εύρυθμης λειτουργίας μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) το HealthManager. Για να καθορίσετε προαιρετικών παραμέτρων, δημιουργήστε [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία διαμερίσματα είναι [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία για όλα τα διαμερίσματα της το **ύφασμα: / WordCount/WordCountService** υπηρεσίας:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας διαμερίσματα με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707683.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707680.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-replica-health"></a>Λήψη αντιγράφου εύρυθμης λειτουργίας
Επιστρέφει την εύρυθμη λειτουργία της κατάστασης υπηρεσίας ρεπλίκα ή μια παρουσία χωρίς κατάσταση υπηρεσίας. Εισαγωγή δεδομένων:

- [Απαιτείται] Τα διαμερίσματα Αναγνωριστικό (GUID) και ρεπλίκα το Αναγνωριστικό που προσδιορίζει τη ρεπλίκα.

- [Προαιρετικό] Η εφαρμογή εύρυθμης λειτουργίας πολιτικής παράμετροι που χρησιμοποιούνται για να παρακάμψετε τις πολιτικές δήλωσης εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε την εύρυθμη λειτουργία ρεπλίκα μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) το HealthManager. Για να καθορίσετε τις παραμέτρους για προχωρημένους, χρησιμοποιήστε [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία ρεπλίκα είναι [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία της πρωτεύον αντίγραφο για όλα τα διαμερίσματα της υπηρεσίας:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας ρεπλίκα με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707673.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707641.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-deployed-application-health"></a>Λήψη εφαρμογής εύρυθμης λειτουργίας
Επιστρέφει την εύρυθμη λειτουργία μιας εφαρμογής αναπτυχθεί σε μια οντότητα κόμβο. Περιέχει τα μέλη ανεπτυγμένος υπηρεσίας πακέτου εύρυθμης λειτουργίας. Εισαγωγή δεδομένων:

- [Απαιτείται] Το όνομα της εφαρμογής (URI) και όνομα κόμβου (συμβολοσειρά) που προσδιορίζουν εφαρμογής.

- [Προαιρετικό] Η πολιτική εύρυθμης λειτουργίας εφαρμογής που χρησιμοποιείται για να παρακάμψετε τις πολιτικές δήλωσης εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα και πακέτα ανεπτυγμένος υπηρεσιών που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα και πακέτα ανεπτυγμένος υπηρεσιών που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε την εύρυθμη λειτουργία μιας εφαρμογής αναπτυχθεί σε έναν κόμβο μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) το HealthManager. Για να καθορίσετε προαιρετικών παραμέτρων, χρησιμοποιήστε [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία της εφαρμογής είναι [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) . Για να βρείτε όπου έχει αναπτυχθεί μια εφαρμογή, εκτελέστε [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) και εξετάστε τα θυγατρικά στοιχεία της εφαρμογής.

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία της το **ύφασμα: / WordCount** εφαρμογή αναπτυχθεί σε **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμης λειτουργίας εφαρμογής με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707644.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707688.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="get-deployed-service-package-health"></a>Λήψη εύρυθμη λειτουργία των υπηρεσιών ανεπτυγμένος πακέτου
Επιστρέφει την εύρυθμη λειτουργία μιας οντότητας πακέτου ανεπτυγμένος υπηρεσίας. Εισαγωγή δεδομένων:

- [Απαιτείται] Το όνομα της εφαρμογής (URI), όνομα κόμβου (συμβολοσειρά) και υπηρεσία δήλωσης όνομα (συμβολοσειρά) που προσδιορίζουν το πακέτο ανεπτυγμένος υπηρεσίας.

- [Προαιρετικό] Την πολιτική εύρυθμης λειτουργίας εφαρμογής που χρησιμοποιούνται για την παράκαμψη της δήλωσης πολιτικής εφαρμογής.

- [Προαιρετικό] Φίλτρα για συμβάντα που καθορίζουν ποιες εγγραφές βρίσκονται που σας ενδιαφέρουν και πρέπει να επιστραφεί στο αποτέλεσμα (για παράδειγμα, μόνο, σφάλματα ή προειδοποιήσεις και σφάλματα). Όλα τα συμβάντα που χρησιμοποιούνται για την αξιολόγηση της υγείας οντότητα συναθροιστεί, ανεξάρτητα από το φίλτρο.

### <a name="api"></a>API
Για να λάβετε την εύρυθμη λειτουργία της ένα πακέτο ανεπτυγμένος υπηρεσίας μέσω του API, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) το HealthManager. Για να καθορίσετε προαιρετικών παραμέτρων, χρησιμοποιήστε [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία υπηρεσίας ανεπτυγμένος πακέτου είναι [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) . Για να δείτε όπου έχει αναπτυχθεί μια εφαρμογή, εκτελέστε [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) και κοιτάξτε την ανάπτυξη εφαρμογών. Για να δείτε ποια πακέτα είναι σε μια εφαρμογή υπηρεσίας, δείτε τα θυγατρικά στοιχεία πακέτου ανεπτυγμένος υπηρεσίας στο αποτέλεσμα [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) .

Το ακόλουθο cmdlet λαμβάνει την εύρυθμη λειτουργία του πακέτου υπηρεσίας **WordCountServicePkg** από το **ύφασμα: / WordCount** εφαρμογή αναπτυχθεί σε **_Node_2**. Η οντότητα έχει **System.Hosting** αναφορών για επιτυχή πακέτου service και ενεργοποίηση του σημείου εισόδου και επιτυχής υπηρεσία τύπου εγγραφής.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε εύρυθμη λειτουργία των υπηρεσιών ανεπτυγμένος πακέτου με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/dn707677.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/dn707689.aspx) που περιλαμβάνει πολιτικές εύρυθμης λειτουργίας που περιγράφονται στο σώμα.

## <a name="health-chunk-queries"></a>Ερωτήματα μπλοκ εύρυθμης λειτουργίας
Τα ερωτήματα μπλοκ εύρυθμης λειτουργίας να επιστρέψετε θυγατρικά στοιχεία πολλών επιπέδων σύμπλεγμα (σταδιακά), ανά φίλτρα εισόδου. Υποστηρίζει σύνθετα φίλτρα που επιτρέπουν όγκο ευελιξία για να εκφράσετε ποια συγκεκριμένα τα παιδιά να επιστραφούν, που προσδιορίζεται από το μοναδικό αναγνωριστικό ή άλλες αναγνωριστικό ομάδας ή/και κατάσταση εύρυθμης λειτουργίας. Από προεπιλογή, δεν υπάρχει θυγατρικά στοιχεία περιλαμβάνονται, σε σύγκριση με την εύρυθμη λειτουργία εντολές που περιλαμβάνουν πάντα θυγατρικά στοιχεία πρώτου επιπέδου.

Τα [ερωτήματα εύρυθμης λειτουργίας](service-fabric-view-entities-aggregated-health.md#health-queries) επιστρέφουν μόνο πρώτου επιπέδου θυγατρικά στοιχεία της καθορισμένης οντότητας ανά απαιτούμενα φίλτρα. Για να λάβετε τα θυγατρικά στοιχεία του τα θυγατρικά στοιχεία, πρέπει να καλέσετε APIs επιπλέον εύρυθμης λειτουργίας για κάθε οντότητα που σας ενδιαφέρουν. Ομοίως, για να λάβετε την εύρυθμη λειτουργία των συγκεκριμένων οντοτήτων, πρέπει να καλέσετε έναν εύρυθμης λειτουργίας API για κάθε οντότητα που θέλετε. Το ερώτημα μπλοκ για προχωρημένους φιλτράρισμα σάς επιτρέπει να ζητήσετε πολλαπλών στοιχείων ενδιαφέροντος σε ένα ερώτημα, την ελαχιστοποίηση το μέγεθος του μηνύματος και τον αριθμό των μηνυμάτων.

Η τιμή του ερωτήματος μπλοκ είναι ότι μπορείτε να λάβετε κατάσταση εύρυθμης λειτουργίας για περισσότερες σύμπλεγμα οντοτήτων (ενδεχομένως όλα σύμπλεγμα οντοτήτων ξεκινώντας από το ριζικό απαιτείται) σε μία κλήση. Να μπορείτε να εκφράσετε όπως είναι οι σύνθετες εύρυθμης λειτουργίας ερωτήματος:

- Επιστροφή μόνο τις εφαρμογές στο σφάλμα και για αυτές τις εφαρμογές περιλαμβάνουν όλες τις υπηρεσίες σε προειδοποίηση | σφάλματος. Για τις υπηρεσίες επιστρέφεται, συμπεριλάβετε όλα τα διαμερίσματα.

- Επιστροφή μόνο την εύρυθμη λειτουργία των εφαρμογών 4, που καθορίζεται από τα ονόματά τους.

- Επιστροφή μόνο την εύρυθμη λειτουργία των εφαρμογών του τύπου μια εφαρμογή που θέλετε.

- Επιστροφή όλα ανεπτυγμένος οντοτήτων σε έναν κόμβο. Επιστρέφει όλες τις εφαρμογές, όλα αναπτύξει εφαρμογές στον καθορισμένο κόμβο και όλα τα πακέτα ανεπτυγμένος υπηρεσιών σε αυτόν τον κόμβο.

- Επιστροφή όλα τα αντίγραφα στο σφάλμα. Επιστρέφει όλες τις εφαρμογές, τις υπηρεσίες, τα διαμερίσματα και μόνο αντίγραφα στο σφάλμα.

- Επιστρέφει όλες τις εφαρμογές. Για μια συγκεκριμένη υπηρεσία, συμπεριλάβετε όλα τα διαμερίσματα.

Προς το παρόν, το ερώτημα μπλοκ εύρυθμης λειτουργίας εκτίθεται μόνο για το σύμπλεγμα οντότητα. Η συνάρτηση επιστρέφει ένα μπλοκ εύρυθμης λειτουργίας συμπλέγματος, το οποίο περιέχει:

- Η κατάσταση εύρυθμης λειτουργίας συμπλέγματος συναθροιστεί.

- Η λίστα μπλοκ κατάσταση εύρυθμης λειτουργίας της κόμβους που ακολουθούν φίλτρα εισόδου.

- Η λίστα μπλοκ κατάσταση εύρυθμης λειτουργίας των εφαρμογών που ακολουθούν φίλτρα εισόδου. Κάθε μπλοκ κατάσταση εύρυθμης λειτουργίας εφαρμογής περιέχει μια λίστα μπλοκ με όλες τις υπηρεσίες που ακολουθούν φίλτρα εισόδου και μια λίστα μπλοκ με όλες τις εφαρμογές ανεπτυγμένος που ακολουθούν τα φίλτρα. Ίδια για τα θυγατρικά στοιχεία των υπηρεσιών και την ανάπτυξη εφαρμογών. Με αυτόν τον τρόπο, όλα οντοτήτων στο σύμπλεγμα μπορεί να ενδεχομένως επιστραφεί εάν ζητήθηκε, με ιεραρχικό τρόπο.

### <a name="cluster-health-chunk-query"></a>Σύμπλεγμα εύρυθμης λειτουργίας μπλοκ ερωτήματος
Επιστρέφει την εύρυθμη λειτουργία της οντότητας σύμπλεγμα και περιέχει το μπλοκ κατάστασης ιεραρχικά εύρυθμης λειτουργίας απαιτείται θυγατρικά στοιχεία. Εισαγωγή δεδομένων:

- [Προαιρετικό] Η πολιτική εύρυθμης λειτουργίας συμπλέγματος που χρησιμοποιείται για την αξιολόγηση τους κόμβους και τα συμβάντα σύμπλεγμα.

- [Προαιρετικό] Η εφαρμογή εύρυθμης λειτουργίας πολιτικής χάρτη, με τις πολιτικές εύρυθμης λειτουργίας που χρησιμοποιείται για να παρακάμψετε τις πολιτικές δήλωσης εφαρμογής.

- [Προαιρετικό] Φίλτρα για κόμβους και εφαρμογές που καθορίζουν ποιες εγγραφές έχουν ενδιαφέρον και πρέπει να επιστραφεί στο αποτέλεσμα. Τα φίλτρα είναι συγκεκριμένες για μια ομάδα/οντότητα οντοτήτων ή μπορούν να εφαρμοστούν όλες οντοτήτων σε αυτό το επίπεδο. Η λίστα φίλτρων μπορεί να περιέχει ένα γενικό φίλτρο ή/και φίλτρα για συγκεκριμένες αναγνωριστικά λεπτομερές ως πρόσωπα που επιστρέφονται από το ερώτημα. Εάν είναι κενό, δεν επιστρέφουν τα θυγατρικά στοιχεία από προεπιλογή.
Διαβάστε περισσότερα σχετικά με τα φίλτρα στην [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) και [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx). Η εφαρμογή φίλτρων μπορούν να σταδιακά Καθορίστε σύνθετων φίλτρων για θυγατρικά στοιχεία.

Το αποτέλεσμα του μπλοκ περιλαμβάνει τα θυγατρικά στοιχεία που ακολουθούν τα φίλτρα.

Προς το παρόν, το ερώτημα μπλοκ δεν επιστρέφει κατεστραμμένες αξιολογήσεις ή οντότητα συμβάντα. Επιπλέον πληροφορίες που μπορεί να ληφθεί χρησιμοποιώντας το υπάρχον ερώτημα εύρυθμης λειτουργίας συμπλέγματος.

### <a name="api"></a>API
Για να λάβετε σύμπλεγμα εύρυθμης λειτουργίας μπλοκ, δημιουργήστε μια `FabricClient` και κλήση της μεθόδου [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) το **HealthManager**. Μπορείτε να περάσετε στο [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) για να περιγράψετε πολιτικές εύρυθμης λειτουργίας και σύνθετα φίλτρα.

Ο ακόλουθος κώδικας λαμβάνει σύμπλεγμα εύρυθμης λειτουργίας μπλοκ με σύνθετα φίλτρα.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShell
Το cmdlet για να λάβετε την εύρυθμη λειτουργία συμπλέγματος είναι [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Πρώτα, συνδεθείτε με το σύμπλεγμα, χρησιμοποιώντας το cmdlet [ServiceFabricCluster σύνδεση](https://msdn.microsoft.com/library/mt125938.aspx) .

Ο ακόλουθος κώδικας λαμβάνει κόμβους μόνο εάν έχουν κατά λάθος, με εξαίρεση ενός συγκεκριμένου κόμβου, η οποία θα πρέπει πάντα να επιστραφούν.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Το ακόλουθο cmdlet λαμβάνει σύμπλεγμα μπλοκ με εφαρμογή φίλτρων.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Το ακόλουθο cmdlet επιστρέφει όλα ανεπτυγμένος οντοτήτων σε έναν κόμβο.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>ΥΠΌΛΟΙΠΟ
Μπορείτε να λάβετε σύμπλεγμα εύρυθμης λειτουργίας μπλοκ με μια [αίτηση ΓΡΉΓΟΡΑ](https://msdn.microsoft.com/library/azure/mt656722.aspx) ή μια [πρόσκληση σε ΔΗΜΟΣΊΕΥΣΗ](https://msdn.microsoft.com/library/azure/mt656721.aspx) που περιλαμβάνει τις πολιτικές εύρυθμης λειτουργίας και σύνθετων φίλτρων που περιγράφονται στο σώμα.

## <a name="general-queries"></a>Γενικές ερωτημάτων
Γενικές ερωτήματα επιστρέφουν μια λίστα οντοτήτων ύφασμα υπηρεσία ενός καθορισμένου τύπου. Εκτίθενται μέσω του API (χρησιμοποιώντας τις μεθόδους σε **FabricClient.QueryManager**), cmdlet του PowerShell και ΥΠΌΛΟΙΠΟ. Αυτά τα ερωτήματα συγκεντρωτικά αποτελέσματα δευτερεύοντα ερωτήματα από πολλά στοιχεία. Ένα από αυτά είναι τα [αποθηκεύσετε εύρυθμης λειτουργίας](service-fabric-health-introduction.md#health-store), που συμπληρώνει την κατάσταση συγκεντρωτική εύρυθμης λειτουργίας για κάθε αποτέλεσμα ερωτήματος.  

> [AZURE.NOTE] Γενικές ερωτήματα επιστρέφουν την κατάσταση εύρυθμης λειτουργίας συγκεντρωτική η οντότητα και δεν περιέχουν δεδομένα εμπλουτισμένου εύρυθμης λειτουργίας. Εάν μια οντότητα δεν είναι καλή, μπορείτε να τις παρακολουθήσετε με εύρυθμης λειτουργίας ερωτημάτων για όλες τις πληροφορίες εύρυθμης λειτουργίας, όπως συμβάντα, μέλη εύρυθμη λειτουργία των θυγατρικών και κατεστραμμένες αξιολογήσεις.

Εάν γενικά ερωτήματα επιστρέφουν μια κατάσταση εύρυθμης λειτουργίας άγνωστη για μια οντότητα, είναι πιθανό ότι ο χώρος αποθήκευσης εύρυθμης λειτουργίας δεν έχει ολοκληρωθεί δεδομένα σχετικά με την οντότητα. Επίσης, είναι πιθανό ότι ένα δευτερεύον ερώτημα στο χώρο αποθήκευσης εύρυθμης λειτουργίας δεν ήταν επιτυχής (για παράδειγμα, Παρουσιάστηκε σφάλμα επικοινωνίας, ή το χώρο αποθήκευσης εύρυθμης λειτουργίας έχει επιβραδύνει). Παρακολούθηση θέματος με εύρυθμης λειτουργίας ερωτήματος για την οντότητα. Εάν το δευτερεύον ερώτημα αντιμετώπισε μεταβατικές σφάλματα, όπως θέματα δικτύου, ενδέχεται να επιτύχει αυτό το ερώτημα παρακολούθησης. Μπορεί να επίσης σας περισσότερες λεπτομέρειες από το χώρο αποθήκευσης εύρυθμης λειτουργίας σχετικά με το γιατί δεν εκτίθεται η οντότητα.

Τα ερωτήματα που περιέχουν **HealthState** για οντοτήτων είναι:

- Λίστα κόμβου: επιστρέφει τους κόμβους λίστας στο σύμπλεγμα (σελίδων).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShell: Get-ServiceFabricNode
- Λίστα εφαρμογών: επιστρέφει τη λίστα των εφαρμογών του συμπλέγματος (σελίδων).
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricApplication
- Λίστα υπηρεσιών: επιστρέφει τη λίστα των υπηρεσιών σε μια εφαρμογή (σελίδων).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShell: Get-ServiceFabricService
- Λίστα διαμερισμάτων: επιστρέφει τη λίστα των διαμερισμάτων σε μια υπηρεσία (σελίδων).
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShell: Get-ServiceFabricPartition
- Λίστα ρεπλίκα: επιστρέφει τη λίστα των αντίγραφα σε ένα διαμερίσματα (σελίδων).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShell: Get-ServiceFabricReplica
- Λίστα εφαρμογών αναπτυχθεί: επιστρέφει τη λίστα ανάπτυξη εφαρμογών σε έναν κόμβο.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication
- Αναπτυχθεί λίστα πακέτου υπηρεσιών: επιστρέφει τη λίστα των πακέτων υπηρεσίας σε μια εφαρμογή ανεπτυγμένος.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShell: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Ορισμένα από τα ερωτήματα επιστρέφουν σελιδοποιημένος αποτελέσματα. Η απόδοση αυτών των ερωτημάτων είναι μια λίστα που προέρχονται από [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Εάν τα αποτελέσματα δεν εμπίπτουν ένα μήνυμα, επιστρέφεται η τιμή μόνο μια σελίδα και ένα ContinuationToken που παρακολουθεί όπου απαρίθμηση διακοπεί. Πρέπει να συνεχίσετε να καλέσετε το ίδιο ερώτημα και μεταβιβάζουν στο διακριτικό συνέχισης από το προηγούμενο ερώτημα για να λάβετε επόμενη αποτελέσματα.

### <a name="examples"></a>Παραδείγματα

Ο ακόλουθος κώδικας λαμβάνει τις κατεστραμμένες εφαρμογές στο σύμπλεγμα:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Το ακόλουθο cmdlet λαμβάνει τις λεπτομέρειες της εφαρμογής για το ύφασμα: / WordCount εφαρμογής. Παρατηρήστε ότι η κατάσταση εύρυθμης λειτουργίας είναι προειδοποίησης.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Το ακόλουθο cmdlet λαμβάνει τις υπηρεσίες με κατάσταση εύρυθμης λειτουργίας της προειδοποίησης:

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Σύμπλεγμα και εφαρμογή αναβαθμίσεις
Κατά τη διάρκεια μιας εποπτευόμενη αναβάθμισης του συμπλέγματος και εφαρμογή, υπηρεσία ύφασμα έλεγχοι εύρυθμης λειτουργίας για να βεβαιωθείτε ότι όλα τα στοιχεία παραμένει σε καλή κατάσταση. Εάν μια οντότητα είναι κατεστραμμένη όπως αξιολογούνται χρησιμοποιώντας πολιτικές ρυθμισμένο εύρυθμης λειτουργίας, η αναβάθμιση ισχύει πολιτικές αναβάθμιση ειδικά για να προσδιορίσετε την επόμενη ενέργεια. Ενδέχεται να είναι δυνατή η παύση της αναβάθμισης για να επιτρέψετε την παρέμβαση του χρήστη (όπως διόρθωση συνθήκες σφάλματος ή αλλαγή πολιτικών) ή αυτόματα μπορεί να κάνετε επαναφορά στην προηγούμενη έκδοση καλή.

Κατά την αναβάθμιση *σύμπλεγμα* , μπορείτε να λάβετε την κατάσταση αναβάθμισης συμπλέγματος. Την κατάσταση αναβάθμισης περιλαμβάνει κατεστραμμένες αξιολογήσεις, οι οποίες οδηγούν σε τι είναι κατεστραμμένος στο σύμπλεγμα. Εάν η αναβάθμιση επανέρχεται λόγω ζητημάτων εύρυθμης λειτουργίας, την κατάσταση αναβάθμισης θυμάται την τελευταία κατεστραμμένες λόγους. Αυτές οι πληροφορίες μπορεί να βοηθήσει τους διαχειριστές διερευνήσουμε τι δεν πήγε καλά μετά την αναβάθμιση, επαναφορά ή διακοπή.

Ομοίως, κατά την αναβάθμιση μιας *εφαρμογής* , τυχόν κατεστραμμένες αξιολογήσεις περιέχονται σε την κατάσταση αναβάθμισης εφαρμογής.

Τα παρακάτω εμφανίζει την κατάσταση αναβάθμισης εφαρμογής για μια τροποποιημένη ύφασμα: / WordCount εφαρμογής. Μια φύλαξης αναφέρεται σφάλμα σε ένα από τα αντίγραφα. Η αναβάθμιση σάρωση επειδή δεν τηρούνται οι έλεγχοι εύρυθμης λειτουργίας.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Διαβάστε περισσότερα σχετικά με την [αναβάθμιση υπηρεσίας ύφασμα εφαρμογής](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Χρήση αξιολογήσεις εύρυθμης λειτουργίας για την αντιμετώπιση προβλημάτων
Κάθε φορά που υπάρχει κάποιο πρόβλημα με το σύμπλεγμα ή μια εφαρμογή, εξετάστε την εύρυθμη λειτουργία συμπλέγματος ή εφαρμογή για να επισημάνετε ποιο είναι το πρόβλημα. Κατεστραμμένη αξιολόγησης παρέχουν λεπτομέρειες σχετικά με το τι ενεργοποίησε την τρέχουσα εύρυθμης λειτουργίας. Εάν χρειάζεται, μπορείτε να κάνετε Διερεύνηση σε κατεστραμμένη θυγατρικών οντοτήτων για τον προσδιορισμό της αρχικής αιτίας.

> [AZURE.NOTE] Κατεστραμμένη αξιολόγησης εμφανίζουν την πρώτη αιτία η οντότητα αξιολογείται τρέχουσα κατάσταση εύρυθμης λειτουργίας. Μπορεί να υπάρχουν πολλά άλλα συμβάντα που ενεργοποιούν αυτήν την κατάσταση, αλλά δεν είναι θα εφαρμόζονται στις αξιολογήσεις. Για περισσότερες πληροφορίες, Διερεύνηση σε των οντοτήτων εύρυθμης λειτουργίας να υπολογίσετε όλες οι κατεστραμμένες αναφορές στο σύμπλεγμα.

## <a name="next-steps"></a>Επόμενα βήματα
[Χρήση αναφορών εύρυθμης λειτουργίας συστήματος για την αντιμετώπιση προβλημάτων](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Προσθήκη προσαρμοσμένων αναφορών εύρυθμης λειτουργίας υπηρεσίας ύφασμα](service-fabric-report-health.md)

[Πώς μπορείτε να αναφέρετε και έλεγχος εύρυθμης λειτουργίας υπηρεσίας](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Παρακολούθηση και διάγνωση υπηρεσίες τοπικά](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Αναβάθμιση εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-upgrade.md)

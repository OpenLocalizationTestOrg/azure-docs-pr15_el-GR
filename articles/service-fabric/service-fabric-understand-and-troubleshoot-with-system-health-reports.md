<properties
   pageTitle="Αντιμετώπιση προβλημάτων με τις αναφορές εύρυθμης λειτουργίας συστήματος | Microsoft Azure"
   description="Περιγράφει τις αναφορές εύρυθμης λειτουργίας που αποστέλλονται από στοιχεία Azure Service ύφασμα και τη χρήση τους για το σύμπλεγμα αντιμετώπισης προβλημάτων ή θέματα εφαρμογών."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Χρήση αναφορών εύρυθμης λειτουργίας συστήματος για την αντιμετώπιση προβλημάτων

Azure Service ύφασμα στοιχεία αναφοράς από το πλαίσιο σε όλα οντοτήτων στο σύμπλεγμα. Η [Αποθήκευση εύρυθμης λειτουργίας](service-fabric-health-introduction.md#health-store) δημιουργεί και διαγράφει οντοτήτων με βάση τις αναφορές του συστήματος. Το επίσης οργανώνουν τους σε μια ιεραρχία που καταγράφει τις αλληλεπιδράσεις οντότητα.

> [AZURE.NOTE] Για να κατανοήσετε τις έννοιες που σχετίζονται με την εύρυθμη λειτουργία, διαβάστε περισσότερα στο [μοντέλο εύρυθμης λειτουργίας υπηρεσίας ύφασμα](service-fabric-health-introduction.md).

Αναφορές εύρυθμης λειτουργίας συστήματος παρέχουν ορατότητα σε σύμπλεγμα και λειτουργία της εφαρμογής και ζητήματα σημαία μέσω εύρυθμης λειτουργίας. Για εφαρμογές και υπηρεσίες, αναφορών εύρυθμης λειτουργίας συστήματος Βεβαιωθείτε ότι οντοτήτων σχετικά με την εφαρμογή και συμπεριφέρεται σωστά από την προοπτική ύφασμα υπηρεσίας. Οι αναφορές δεν παρέχουν οποιαδήποτε παρακολούθηση εύρυθμης λειτουργίας της εταιρικής λογικής της υπηρεσίας ή ανίχνευση παραμονή διεργασίες. Υπηρεσίες χρήστη να εμπλουτίσετε τα δεδομένα εύρυθμης λειτουργίας με συγκεκριμένες πληροφορίες για τους λογικής.

> [AZURE.NOTE] Αναφορές εύρυθμης λειτουργίας watchdogs είναι ορατές μόνο *Αφού* τα στοιχεία του συστήματος, δημιουργήστε μια οντότητα. Όταν διαγράφεται μια οντότητα, ο χώρος αποθήκευσης εύρυθμης λειτουργίας διαγράφει αυτόματα όλες τις αναφορές εύρυθμης λειτουργίας που έχει συσχετιστεί με αυτό. Το ίδιο ισχύει όταν δημιουργείται μια νέα παρουσία της οντότητας (για παράδειγμα, δημιουργείται μια νέα παρουσία ρεπλίκα υπηρεσίας). Όλες τις αναφορές που σχετίζονται με την παλιά παρουσία έχουν διαγραφεί και τακτοποίησης από το χώρο αποθήκευσης.

Το στοιχείο του συστήματος αναφορές προσδιορίζονται από την προέλευση δεδομένων, που ξεκινά με το "**συστήματος.**" πρόθεμα. Watchdogs δεν είναι δυνατό να χρησιμοποιήσετε το ίδιο πρόθεμα για τις προελεύσεις, όπως αναφορές με μη έγκυρες παράμετροι απορρίπτονται.
Ας δούμε ορισμένες αναφορές συστήματος για να κατανοήσετε τι ενεργοποιεί τους και πώς μπορείτε να διορθώσετε τα πιθανά ζητήματα που αντιπροσωπεύουν.

> [AZURE.NOTE] Υπηρεσία ύφασμα εξακολουθεί να προσθέσετε αναφορές σε συνθήκες που σας ενδιαφέρουν που βελτιώσετε τις πιθανότητες εμφάνισής σε τι συμβαίνει στη σύμπλεγμα και την εφαρμογή.

## <a name="cluster-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος συμπλέγματος
Η οντότητα εύρυθμης λειτουργίας συμπλέγματος δημιουργείται αυτόματα στο χώρο αποθήκευσης εύρυθμης λειτουργίας. Εάν όλα λειτουργούν σωστά, δεν διαθέτει μια αναφορά του συστήματος.

### <a name="neighborhood-loss"></a>Απώλεια περιοχή
**System.Federation** αναφέρει ένα μήνυμα σφάλματος όταν εντοπίσει μια περιοχή απώλεια. Η αναφορά είναι από μεμονωμένους κόμβους και το Αναγνωριστικό κόμβου περιλαμβάνει το όνομα της ιδιότητας. Εάν μία περιοχή θα χαθεί στο πεδίο ολόκληρο κλήση ύφασμα υπηρεσίας, συνήθως μπορείτε να περιμένετε δύο συμβάντα (δύο πλευρές της αναφοράς διάκενου). Αν χαθούν περισσότερες γειτονιές, υπάρχουν περισσότερα συμβάντα.

Η αναφορά Καθορίζει το χρονικό όριο καθολικού εκμίσθωση ως στην διάρκεια ζωής. Η αναφορά στέλνεται κάθε μισό της διάρκειας TTL για με την προϋπόθεση ότι η συνθήκη παραμένει ενεργή. Το συμβάν καταργείται αυτόματα κατά τη λήξη. Κατάργηση όταν έχει λήξει συμπεριφορά εξασφαλίζει ότι η αναφορά είναι τακτοποίησης από το χώρο αποθήκευσης εύρυθμης λειτουργίας σωστά, ακόμα και αν ο κόμβος αναφοράς είναι προς τα κάτω.

- **Αναγνωριστικού προέλευσης**: System.Federation
- **Ιδιότητα**: ξεκινά με **περιοχή** και περιλαμβάνει πληροφορίες κόμβου
- **Επόμενα βήματα**: Διερεύνηση γιατί χάνεται στην περιοχή (για παράδειγμα, ελέγξτε την επικοινωνία μεταξύ του συμπλέγματος κόμβοι).

## <a name="node-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος κόμβου
**System.FM**, που αντιπροσωπεύει την υπηρεσία Διαχείριση ανακατεύθυνσης, είναι η αρχή έκδοσης πιστοποιητικών που διαχειρίζεται πληροφορίες σχετικά με τους κόμβους συμπλέγματος. Κάθε κόμβος θα πρέπει να έχετε μία αναφορά από System.FM που εμφανίζει την κατάσταση. Το οντοτήτων κόμβο καταργούνται κατά την κατάσταση κόμβο καταργείται (ανατρέξτε στο θέμα [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Κόμβος επάνω/κάτω
System.FM αναφορές ως OK, όταν ο κόμβος συμμετέχει η κλήση (είναι εγκατάσταση και λειτουργία). Να αναφέρει ένα μήνυμα σφάλματος, όταν ο κόμβος αναχωρεί κουδουνίσματος (είναι προς τα κάτω, είτε για την αναβάθμιση ή απλώς επειδή έχει απέτυχε). Την ιεραρχία εύρυθμης λειτουργίας που δημιουργήθηκε από το χώρο αποθήκευσης εύρυθμης λειτουργίας αποκτά ενέργεια ανεπτυγμένος οντοτήτων συσχέτισης με αναφορές κόμβο System.FM. Θεωρεί ότι ο κόμβος ένα γονικό στοιχείο εικονικού όλα ανεπτυγμένος οντοτήτων. Το ανεπτυγμένος οντοτήτων σε αυτόν τον κόμβο είναι εκτίθεται μέσω ερωτήματα, εάν ο κόμβος αναφερθεί ως προς τα επάνω κατά System.FM, με την ίδια παρουσία ως την παρουσία που σχετίζεται με το οντοτήτων. Όταν System.FM αναφέρει ότι ο κόμβος είναι προς τα κάτω ή επανεκκίνηση του (μια νέα παρουσία), ο χώρος αποθήκευσης εύρυθμης λειτουργίας εκκαθαρίζει αυτόματα το ανεπτυγμένος οντοτήτων που μπορεί να υπάρχει μόνο στον κόμβο προς τα κάτω ή στην προηγούμενη παρουσία του κόμβου.

- **Αναγνωριστικού προέλευσης**: System.FM
- **Ιδιότητα**: κατάσταση
- **Επόμενα βήματα**: Εάν ο κόμβος είναι προς τα κάτω για αναβάθμιση, αυτό θα πρέπει να επιστρέψει προς τα επάνω όταν έχει αναβαθμιστεί. Σε αυτήν την περίπτωση, θα πρέπει να αλλάξει την κατάσταση εύρυθμης λειτουργίας επιστρέψτε στο κουμπί OK. Εάν ο κόμβος δεν επιστρέψει ή αποτύχει, το πρόβλημα πρέπει περισσότερες έρευνα.

Το παρακάτω παράδειγμα εμφανίζει το συμβάν System.FM με μια κατάσταση εύρυθμης λειτουργίας OK για κόμβο:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Πιστοποιητικό λήξης
**System.FabricNode** αναφορές μια προειδοποίηση όταν τα πιστοποιητικά που χρησιμοποιούνται από τον κόμβο είναι κοντά λήξης. Υπάρχουν τρεις πιστοποιητικά ανά κόμβο: **Certificate_cluster**, **Certificate_server**και **Certificate_default_client**. Όταν η λήξη είναι τουλάχιστον δύο εβδομάδες, την κατάσταση εύρυθμης λειτουργίας αναφορά είναι OK. Κατά τη λήξη είναι μέσα σε δύο εβδομάδες, ο τύπος αναφορά είναι μια προειδοποίηση. TTL από αυτά τα συμβάντα είναι άπειρο και να καταργηθούν κατά έναν κόμβο αφήνει το σύμπλεγμα.

- **Αναγνωριστικού προέλευσης**: System.FabricNode
- **Ιδιότητα**: ξεκινά με το **πιστοποιητικό** και περιέχει περισσότερες πληροφορίες σχετικά με τον τύπο πιστοποιητικού
- **Επόμενα βήματα**: Ενημερώστε τα πιστοποιητικά αν βρίσκονται κοντά λήξης.

### <a name="load-capacity-violation"></a>Φόρτωση χωρητικότητα παραβίασης
Η υπηρεσία ύφασμα εξισορρόπηση φόρτου αναφορές μια προειδοποίηση αν εντοπίσει παραβίασης χωρητικότητα κόμβο.

 - **Αναγνωριστικού προέλευσης**: System.PLB
 - **Ιδιότητα**: ξεκινά με τη **δυνατότητα**
 - **Επόμενα βήματα**: έλεγχος που παρέχονται μετρικά και να προβάλετε την τρέχουσα χωρητικότητα στον κόμβο.

## <a name="application-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος εφαρμογής
**System.CM**, που αντιπροσωπεύει την υπηρεσία διαχείρισης συμπλέγματος, είναι η αρχή έκδοσης πιστοποιητικών που διαχειρίζεται πληροφορίες σχετικά με την εφαρμογή.

### <a name="state"></a>Κατάσταση
System.CM αναφορές ως OK όταν η εφαρμογή έχει δημιουργηθεί ή ενημερωθεί. Σας ενημερώνει για το χώρο αποθήκευσης εύρυθμης λειτουργίας όταν η εφαρμογή έχει διαγραφεί, έτσι ώστε να μπορεί να καταργηθεί από το χώρο αποθήκευσης.

- **Αναγνωριστικού προέλευσης**: System.CM
- **Ιδιότητα**: κατάσταση
- **Επόμενα βήματα**: Εάν έχει δημιουργηθεί η εφαρμογή, θα πρέπει να περιλαμβάνει την αναφορά εύρυθμης λειτουργίας συμπλέγματος Manager. Διαφορετικά, ελέγξτε την κατάσταση της εφαρμογής με την έκδοση ενός ερωτήματος (για παράδειγμα, το PowerShell cmdlet * *Get-ServiceFabricApplication ApplicationName *applicationName***).

Το παρακάτω παράδειγμα εμφανίζει το συμβάν κατάσταση στην το **ύφασμα: / WordCount** εφαρμογής:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος υπηρεσίας
**System.FM**, που αντιπροσωπεύει την υπηρεσία Διαχείριση ανακατεύθυνσης, είναι η αρχή έκδοσης πιστοποιητικών που διαχειρίζεται πληροφορίες σχετικά με τις υπηρεσίες.

### <a name="state"></a>Κατάσταση
System.FM αναφορές ως OK όταν η υπηρεσία έχει δημιουργηθεί. Διαγράφει την οντότητα από το χώρο αποθήκευσης εύρυθμης λειτουργίας όταν έχει διαγραφεί της υπηρεσίας.

- **Αναγνωριστικού προέλευσης**: System.FM
- **Ιδιότητα**: κατάσταση

Το παρακάτω παράδειγμα εμφανίζει το συμβάν κατάστασης στην υπηρεσία **ύφασμα: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Δεν έχει τοποθετηθεί αντίγραφα παραβίασης
**System.PLB** αναφέρει μια προειδοποίηση, εάν δεν μπορεί να εντοπίσει μια θέση για ένα ή περισσότερα αντίγραφα υπηρεσίας. Η αναφορά έχει καταργηθεί κατά τη λήξη.

- **Αναγνωριστικού προέλευσης**: System.FM
- **Ιδιότητα**: κατάσταση
- **Επόμενα βήματα**: Ελέγξτε τους περιορισμούς υπηρεσίας και την τρέχουσα κατάσταση του τη θέση.

Το παρακάτω παράδειγμα εμφανίζει παραβίασης για μια υπηρεσία που έχουν ρυθμιστεί με 7 αντίγραφα προορισμού σε ένα σύμπλεγμα με 5 κόμβους:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος partition
**System.FM**, που αντιπροσωπεύει την υπηρεσία Διαχείριση ανακατεύθυνσης, είναι η αρχή έκδοσης πιστοποιητικών που διαχειρίζεται πληροφορίες σχετικά με τα διαμερίσματα υπηρεσίας.

### <a name="state"></a>Κατάσταση
System.FM αναφορές ως OK όταν τα διαμερίσματα έχει δημιουργηθεί και είναι σε καλή κατάσταση. Διαγράφει την οντότητα από το χώρο αποθήκευσης εύρυθμης λειτουργίας όταν διαγράφεται τα διαμερίσματα.

Εάν τα διαμερίσματα είναι κάτω από το ελάχιστο ρεπλίκα πλήθος, αναφέρει κάποιο σφάλμα. Εάν τα διαμερίσματα δεν είναι κάτω από το ελάχιστο ρεπλίκα πλήθος, αλλά είναι κάτω από το πλήθος ρεπλίκα προορισμού, αναφορές μια προειδοποίηση. Εάν τα διαμερίσματα είναι απώλεια απαρτίας, System.FM αναφέρει κάποιο σφάλμα.

Άλλα σημαντικά συμβάντα περιλαμβάνουν μια προειδοποίηση κατά την αλλαγή των ρυθμίσεων παραμέτρων διαρκεί περισσότερο από το αναμενόμενο και όταν η Δόμηση διαρκεί περισσότερο από το αναμενόμενο. Την αναμενόμενη ώρες για τη δημιουργία και αλλαγή των ρυθμίσεων παραμέτρων είναι με δυνατότητα ρύθμισης παραμέτρων που βασίζονται σε σενάρια υπηρεσιών. Για παράδειγμα, εάν μια υπηρεσία έχει ένα terabyte κατάστασης, όπως η βάση δεδομένων SQL, δημιουργία διαρκεί περισσότερο από μια υπηρεσία με μικρό κατάστασης.

- **Αναγνωριστικού προέλευσης**: System.FM
- **Ιδιότητα**: κατάσταση
- **Επόμενα βήματα**: Εάν η κατάσταση εύρυθμης λειτουργίας δεν είναι OK, είναι πιθανό ότι ορισμένα αντίγραφα δεν έχουν δημιουργήσει, ανοίξει, ή προωθούνται πρωτεύον ή δευτερεύον σωστά. Σε πολλές περιπτώσεις, η αρχική αιτία είναι μια υπηρεσία σφάλμα κατά το άνοιγμα ή αλλαγή ρόλων εφαρμογή.

Το παρακάτω παράδειγμα εμφανίζει ένα άρτιο διαμερίσματα:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Το παρακάτω παράδειγμα εμφανίζει την κατάσταση της ένα διαμερίσματα που βρίσκεται κάτω από την καταμέτρηση ρεπλίκα προορισμού. Το επόμενο βήμα είναι να την περιγραφή διαμερίσματα, το οποίο εμφανίζει πώς έχει ρυθμιστεί: **MinReplicaSetSize** είναι δύο και **TargetReplicaSetSize** επτά. Στη συνέχεια, βρείτε τον αριθμό των κόμβοι στο σύμπλεγμα: πέντε. Επομένως, σε αυτήν την περίπτωση, δύο αντίγραφα δεν είναι δυνατό να τοποθετηθεί.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Παραβίαση περιορισμού αντιγράφου
**System.PLB** αναφέρει μια προειδοποίηση, εάν εντοπίζει παραβίασης περιορισμού ρεπλίκα και δεν μπορούν να τοποθετήσουν αντίγραφα των τα διαμερίσματα.

- **Αναγνωριστικού προέλευσης**: System.PLB
- **Ιδιότητα**: ξεκινά με **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος αντιγράφου
**System.RA**, που αντιπροσωπεύει το στοιχείο παράγοντα αλλαγή των ρυθμίσεων παραμέτρων, είναι η αρχή για την κατάσταση αντιγράφου.

### <a name="state"></a>Κατάσταση
**System.RA** αναφορές ως OK όταν δημιουργηθεί η ρεπλίκα.

- **Αναγνωριστικού προέλευσης**: System.RA
- **Ιδιότητα**: κατάσταση

Το παρακάτω παράδειγμα εμφανίζει μια καλή ρεπλίκα:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Άνοιγμα αντιγράφου κατάστασης
Περιγραφή της αυτήν την αναφορά εύρυθμης λειτουργίας περιέχει την ώρα έναρξης (Συντονισμένη παγκόσμια ώρα) όταν έχει ενεργοποιηθεί η κλήση API.

**System.RA** αναφέρει μια προειδοποίηση, εάν το άνοιγμα αντιγράφου διαρκεί περισσότερο από την περίοδο που έχετε διαμορφώσει (προεπιλογή: 30 λεπτά). Εάν το API επηρεάζει διαθεσιμότητα υπηρεσίας, η αναφορά εκδίδεται πολύ ταχύτερα (ένα με δυνατότητα ρύθμισης παραμέτρων διάστημα, με έναν προεπιλεγμένο 30 δευτερόλεπτα). Ο χρόνος που μετράται περιλαμβάνει την ώρα που λαμβάνονται για το άνοιγμα του προγράμματος αναπαραγωγής και το άνοιγμα της υπηρεσίας. Η ιδιότητα αλλάζει σε OK εάν ολοκληρώσει το άνοιγμα.

- **Αναγνωριστικού προέλευσης**: System.RA
- **Ιδιότητα**: **ReplicaOpenStatus**
- **Επόμενα βήματα**: Εάν η κατάσταση εύρυθμης λειτουργίας δεν είναι OK, εξετάστε γιατί το άνοιγμα αντιγράφου διαρκεί περισσότερο από το αναμενόμενο.

### <a name="slow-service-api-call"></a>Κλήση API αργή υπηρεσίας
Αναφορά **System.RAP** και **System.Replicator** μια προειδοποίηση εάν μια κλήση για να τον κωδικό χρήστη διαρκεί περισσότερο από την ώρα που έχει ρυθμιστεί. Η προειδοποίηση είναι απενεργοποιημένο, όταν ολοκληρωθεί η κλήση.

- **Αναγνωριστικού προέλευσης**: System.RAP ή System.Replicator
- **Ιδιότητα**: το όνομα του API αργή. Η περιγραφή παρέχει περισσότερες λεπτομέρειες σχετικά με την ώρα που το API έχει τεθεί σε εκκρεμότητα.
- **Επόμενα βήματα**: Διερεύνηση γιατί η κλήση διαρκεί περισσότερο από το αναμενόμενο.

Το παρακάτω παράδειγμα εμφανίζει ένα διαμερίσματα στο απώλεια απαρτίας και τα βήματα έρευνα τέλος για να υπολογίσετε γιατί. Ένα από τα αντίγραφα έχει μια κατάσταση εύρυθμης λειτουργίας προειδοποίηση, ώστε να λαμβάνετε την εύρυθμη λειτουργία. Εμφανίζει ότι στη λειτουργία της υπηρεσίας διαρκεί περισσότερο από το αναμενόμενο, ένα συμβάν αναφέρεται από System.RAP. Μετά τη λήψη αυτών των πληροφοριών, το επόμενο βήμα είναι να δείτε τον κώδικα υπηρεσίας και Διερεύνηση εκεί. Για αυτήν την περίπτωση, η εφαρμογή **RunAsync** της κατάστασης υπηρεσίας δημιουργεί μια ανεπίλυτη εξαίρεση. Τα αντίγραφα Ανακύκλωσης, ώστε να μην βλέπετε οποιαδήποτε αντίγραφα σε κατάσταση προειδοποίησης. Μπορείτε να επαναλάβετε γρήγορα την κατάσταση εύρυθμης λειτουργίας και αναζητήστε τις διαφορές στις το αναγνωριστικό του αντιγράφου. Σε ορισμένες περιπτώσεις, η επαναλήψεις μπορεί να σας δώσει ενδείξεις.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Όταν ξεκινάτε την εφαρμογή ελαττωματική κάτω από το πρόγραμμα εντοπισμού σφαλμάτων, τα windows διαγνωστικών συμβάντα εμφάνιση εξαίρεση από RunAsync:

![Visual Studio 2015 διαγνωστικών συμβάντα: RunAsync αποτυχία στο ύφασμα: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 διαγνωστικών συμβάντα: Αποτυχία RunAsync στο **ύφασμα: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Πλήρης ουράς αναπαραγωγής
**System.Replicator** αναφέρει μια προειδοποίηση, εάν η ουρά αναπαραγωγής είναι πλήρης. Στην κύρια, αυτό συνήθως συμβαίνει επειδή μία ή περισσότερες δευτερεύουσες ρεπλίκα είναι αργή για να επιβεβαιώσετε λειτουργίες. Στην τη δευτερεύουσα, αυτό συνήθως συμβαίνει όταν η υπηρεσία είναι αργή για να εφαρμόσετε τις λειτουργίες. Η προειδοποίηση είναι απενεργοποιημένο, όταν η ουρά δεν είναι πλέον είναι πλήρης.

- **Αναγνωριστικού προέλευσης**: System.Replicator
- **Ιδιότητα**: **PrimaryReplicationQueueStatus** ή **SecondaryReplicationQueueStatus**, ανάλογα με το ρόλο αντιγράφου

### <a name="slow-naming-operations"></a>Ονομασία αργή λειτουργίες

**System.NamingService** αναφορές εύρυθμης λειτουργίας στο την κύρια ρεπλίκα όταν μια λειτουργία ονομάτων διαρκεί περισσότερο από αποδεκτή. Παραδείγματα ονομάτων λειτουργίες είναι [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) ή [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Περισσότερες μεθόδους μπορεί να βρεθεί στην περιοχή FabricClient, για παράδειγμα στην περιοχή [υπηρεσία διαχείρισης μεθόδων](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) ή [μεθόδων διαχείρισης ιδιότητα](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Η υπηρεσία ονομάτων επιλύει τα ονόματα υπηρεσιών σε μια θέση στο σύμπλεγμα και σας δίνει τη δυνατότητα στους χρήστες να διαχειρίζονται τα ονόματα υπηρεσιών και τις ιδιότητες. Πρόκειται για μια υπηρεσία ύφασμα διαμερίσματα διατηρηθεί υπηρεσίας. Ένα από τα διαμερίσματα αντιπροσωπεύει τον κάτοχο της αρχής, που περιέχει μετα-δεδομένα σχετικά με όλα τα ονόματα ύφασμα υπηρεσίας και τις υπηρεσίες. Τα ονόματα υπηρεσιών ύφασμα αντιστοιχίζονται σε διαφορετικά διαμερίσματα, που ονομάζεται διαμερίσματα όνομα κατόχου, ώστε η υπηρεσία είναι επεκτάσιμη. Διαβάστε περισσότερα σχετικά με την [υπηρεσία ονομάτων](service-fabric-architecture.md).

Όταν μια λειτουργία ονομάτων διαρκεί περισσότερο από το αναμενόμενο, η λειτουργία έχει σημαία έκθεση προειδοποίηση σχετικά με την *κύρια ρεπλίκα της τα διαμερίσματα υπηρεσίας ονομάτων που χρησιμοποιείται τη λειτουργία*. Εάν η λειτουργία ολοκληρωθεί με επιτυχία, η προειδοποίηση είναι απενεργοποιημένο. Εάν η λειτουργία ολοκληρωθεί με το μήνυμα σφάλματος, η αναφορά εύρυθμης λειτουργίας περιλαμβάνει λεπτομέρειες σχετικά με το σφάλμα.

- **Αναγνωριστικού προέλευσης**: System.NamingService
- **Ιδιότητα**: ξεκινά με πρόθεμα **Duration_** και προσδιορίζει τη λειτουργία αργή και το όνομα ύφασμα υπηρεσία στην οποία έχει εφαρμοστεί η λειτουργία. Για παράδειγμα, εάν η δημιουργία της υπηρεσίας στο όνομα ύφασμα: / MyApp/MyService διαρκεί πολύ χρόνο, η ιδιότητα είναι Duration_AOCreateService.fabric:/MyApp/MyService. AO σημεία του ρόλου των τα διαμερίσματα ονομάτων για αυτό το όνομα και τη λειτουργία.
- **Επόμενα βήματα**: ελέγχου γιατί η λειτουργία ονομάτων αποτυγχάνει. Κάθε λειτουργίας μπορούν να έχουν διαφορετικές αρχικές αιτίες. Για παράδειγμα, διαγραφή υπηρεσία ενδέχεται να έχει κολλήσει σε έναν κόμβο, επειδή ο κεντρικός υπολογιστής εφαρμογής διατηρεί παρουσιάζει σφάλμα σε έναν κόμβο λόγω σφάλματος χρήστη στον κώδικα υπηρεσίας.

Το παρακάτω παράδειγμα εμφανίζει μια λειτουργία δημιουργίας υπηρεσίας. Η λειτουργία διαρκεί περισσότερο από τη διάρκεια που έχει ρυθμιστεί. AO επαναλήψεις και στέλνει την εργασία σε όχι. ΔΕΝ ολοκληρώθηκε η τελευταία λειτουργία με το χρονικό όριο. Σε αυτήν την περίπτωση, η ίδια ρεπλίκα είναι πρωτεύον για το AO και δεν ΥΠΆΡΧΟΥΝ ρόλοι.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος DeployedApplication
**System.Hosting** είναι η αρχή στην ανάπτυξη οντοτήτων.

### <a name="activation"></a>Ενεργοποίηση
System.Hosting αναφορές ως OK όταν μια εφαρμογή έχει ενεργοποιηθεί με επιτυχία στον κόμβο. Διαφορετικά, αναφέρει κάποιο σφάλμα.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: ενεργοποίηση, συμπεριλαμβανομένης της έκδοσης της ενοποιημένης επικοινωνίας
- **Επόμενα βήματα**: Εάν η εφαρμογή είναι κατεστραμμένη, εξετάστε γιατί απέτυχε η ενεργοποίηση.

Το παρακάτω παράδειγμα εμφανίζει επιτυχή ενεργοποίηση:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Λήψη
**System.Hosting** αναφέρει ένα σφάλμα εάν αποτύχει η λήψη του πακέτου εφαρμογών.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: * *λήψη:*RolloutVersion***
- **Επόμενα βήματα**: Διερεύνηση γιατί απέτυχε η λήψη του στον κόμβο.

## <a name="deployedservicepackage-system-health-reports"></a>Αναφορές εύρυθμης λειτουργίας συστήματος DeployedServicePackage
**System.Hosting** είναι η αρχή στην ανάπτυξη οντοτήτων.

### <a name="service-package-activation"></a>Ενεργοποίηση του πακέτου υπηρεσίας
System.Hosting αναφορές ως OK εάν η υπηρεσία Ενεργοποίηση πακέτου στον κόμβο είναι επιτυχής. Διαφορετικά, αναφέρει κάποιο σφάλμα.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: ενεργοποίησης
- **Επόμενα βήματα**: Διερεύνηση γιατί απέτυχε η ενεργοποίηση.

### <a name="code-package-activation"></a>Ενεργοποίηση του πακέτου κώδικα
**System.Hosting** αναφορές ως OK για κάθε πακέτο κώδικα εάν η ενεργοποίηση είναι επιτυχής. Εάν αποτύχει η ενεργοποίηση, αναφορές μια προειδοποίηση, όπως έχει ρυθμιστεί. Εάν **CodePackage** αποτυγχάνει για να ενεργοποιήσετε ή να τερματίζει με το μήνυμα σφάλματος που είναι μεγαλύτερη από τη ρύθμιση παραμέτρων **CodePackageHealthErrorThreshold**, φιλοξενίας αναφέρει κάποιο σφάλμα. Εάν ένα πακέτο υπηρεσίας περιέχει πολλά πακέτα κώδικα, δημιουργείται μια αναφορά ενεργοποίησης για κάθε μία.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: χρησιμοποιεί το πρόθεμα **CodePackageActivation** και περιέχει το όνομα του πακέτου κώδικα και το σημείο εισόδου ως * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint* ** (για παράδειγμα, **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Καταχώρηση τύπου υπηρεσίας
**System.Hosting** αναφορές ως OK εάν ο τύπος υπηρεσίας έχει καταχωρηθεί με επιτυχία. Αναφορές ένα σφάλμα εάν η καταχώρηση δεν έτοιμοι σε χρόνο (όπως έχει ρυθμιστεί με τη χρήση **ServiceTypeRegistrationTimeout**). Εάν ο τύπος υπηρεσίας έχει καταχωρηθεί από τον κόμβο, αυτό συμβαίνει επειδή το χρόνο εκτέλεσης έχει κλείσει. Φιλοξενεί τις αναφορές μια προειδοποίηση.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: χρησιμοποιεί το πρόθεμα **ServiceTypeRegistration** και περιέχει το όνομα του τύπου υπηρεσίας (για παράδειγμα, **ServiceTypeRegistration:FileStoreServiceType**)

Το παρακάτω παράδειγμα εμφανίζει ένα πακέτο άρτιο ανεπτυγμένος υπηρεσίας:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Λήψη
**System.Hosting** αναφέρει ένα σφάλμα εάν αποτύχει η λήψη του πακέτου υπηρεσίας.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: * *λήψη:*RolloutVersion***
- **Επόμενα βήματα**: Διερεύνηση γιατί απέτυχε η λήψη του στον κόμβο.

### <a name="upgrade-validation"></a>Αναβάθμιση επικύρωσης
**System.Hosting** αναφέρει ένα σφάλμα εάν αποτυχίες επικύρωσης κατά την αναβάθμιση ή εάν αποτύχει η αναβάθμιση στον κόμβο.

- **Αναγνωριστικού προέλευσης**: System.Hosting
- **Ιδιότητα**: χρησιμοποιεί το πρόθεμα **FabricUpgradeValidation** και περιέχει την έκδοση αναβάθμισης
- **Περιγραφή**: σημεία το σφάλμα που παρουσιάστηκε

## <a name="next-steps"></a>Επόμενα βήματα
[Προβολή αναφορών εύρυθμης λειτουργίας υπηρεσίας ύφασμα](service-fabric-view-entities-aggregated-health.md)

[Πώς μπορείτε να αναφέρετε και έλεγχος εύρυθμης λειτουργίας υπηρεσίας](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Παρακολούθηση και διάγνωση υπηρεσίες τοπικά](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Αναβάθμιση εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-upgrade.md)

<properties
    pageTitle="Αυτοματοποίηση Διαχείριση εφαρμογών υπηρεσίας υφάσματος με χρήση του PowerShell | Microsoft Azure"
    description="Ανάπτυξη, αναβάθμιση, δοκιμή και κατάργηση εφαρμογών υπηρεσίας υφάσματος με χρήση του PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Αυτοματοποίηση του κύκλου ζωής εφαρμογών με χρήση του PowerShell

Μπορεί να είναι αυτοματοποιημένη πολλές πτυχές της του [κύκλου ζωής εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-lifecycle.md) .  Σε αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του PowerShell για την αυτοματοποίηση κοινών εργασιών για την ανάπτυξη, την αναβάθμιση, κατάργηση και δοκιμή Azure Service ύφασμα εφαρμογών.  Διαχείριση και HTTP API για Διαχείριση εφαρμογών είναι επίσης διαθέσιμες. Ανατρέξτε στο θέμα [εφαρμογή κύκλου ζωής](service-fabric-application-lifecycle.md) για περισσότερες πληροφορίες.  

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πριν από τη μετακίνηση στις εργασίες στο άρθρο, βεβαιωθείτε ότι:

+ Εξοικείωση με τις έννοιες ύφασμα υπηρεσίας που περιγράφεται στην [Τεχνική επισκόπηση της υπηρεσίας ύφασμα](service-fabric-technical-overview.md).
+ [Εγκαταστήστε το περιβάλλον εκτέλεσης, SDK, και εργαλεία](service-fabric-get-started.md), η οποία εγκαθιστά τη λειτουργική μονάδα PowerShell **ServiceFabric** .
+ [Εκτέλεση της δέσμης ενεργειών του PowerShell Ενεργοποίηση](service-fabric-get-started.md#enable-powershell-script-execution).
+ Ξεκινήστε ένα τοπικό σύμπλεγμα.  Ξεκινήστε ένα νέο παράθυρο του PowerShell ως διαχειριστής και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών ρύθμισης συμπλέγματος από το φάκελο SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Πριν να εκτελέσετε τις εντολές του PowerShell σε αυτό το άρθρο, πρώτα να συνδεθείτε με το τοπικό σύμπλεγμα ύφασμα υπηρεσία, χρησιμοποιώντας [**Σύνδεση ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx):`Connect-ServiceFabricCluster localhost:19000`
+ Τις ακόλουθες εργασίες απαιτούν ένα πακέτο εφαρμογών v1 για την ανάπτυξη και ένα πακέτο εφαρμογών v2 για αναβάθμιση. Κάντε λήψη του [ **WordCount** δείγμα εφαρμογής](http://aka.ms/servicefabricsamples) (που βρίσκεται στα δείγματα γρήγορα αποτελέσματα). Δημιουργήστε και συσκευασία της εφαρμογής στο Visual Studio (κάντε δεξί κλικ σε **WordCount** στην Εξερεύνηση λύσεων και επιλέξτε **το πακέτο**). Αντιγράψτε το πακέτο v1 στο `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` να `C:\Temp\WordCount`. Αντιγραφή `C:\Temp\WordCount` σε `C:\Temp\WordCountV2`, τη δημιουργία του πακέτου εφαρμογής v2 για αναβάθμιση. Άνοιγμα `C:\Temp\WordCountV2\ApplicationManifest.xml` σε ένα πρόγραμμα επεξεργασίας κειμένου. Στο στοιχείο **ApplicationManifest** , αλλάξτε το χαρακτηριστικό **ApplicationTypeVersion** από "1.0.0" σε "2.0.0" για να ενημερώσετε τον αριθμό έκδοσης της εφαρμογής. Αποθηκεύστε το τροποποιημένο αρχείο ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Εργασία: Ανάπτυξη μιας εφαρμογής υπηρεσίας ύφασμα

Αφού δημιουργηθεί και συσκευαστούν της εφαρμογής (ή λήψη του πακέτου εφαρμογών), μπορείτε να αναπτύξετε την εφαρμογή σε ένα τοπικό σύμπλεγμα ύφασμα υπηρεσίας. Ανάπτυξη περιλαμβάνει αποστολή του πακέτου εφαρμογών, την καταχώρηση ο τύπος της εφαρμογής και τη δημιουργία την παρουσία της εφαρμογής. Χρησιμοποιήστε τις οδηγίες που εμφανίζονται σε αυτήν την ενότητα για να αναπτύξετε μια νέα εφαρμογή σε ένα σύμπλεγμα.

### <a name="step-1-upload-the-application-package"></a>Βήμα 1: Αποστολή του πακέτου εφαρμογών
Αποστολή του πακέτου εφαρμογών στο χώρο αποθήκευσης ειδώλου τίθεται σε μια θέση προσβάσιμος σε εσωτερικά στοιχεία ύφασμα υπηρεσίας.  Το πακέτο εφαρμογών περιέχει τα απαραίτητα δήλωση εφαρμογής υπηρεσίας δηλώσεων και τον κωδικό, ρύθμιση παραμέτρων και πακέτα δεδομένων για τη δημιουργία της εφαρμογής και παρουσιών της υπηρεσίας.  Η εντολή [**Αντιγραφή ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) αποστέλλει το πακέτο. Για παράδειγμα:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Βήμα 2: Καταχωρήστε τον τύπο εφαρμογής
Εγγραφή για το πακέτο εφαρμογών κάνει τον τύπο εφαρμογής και την έκδοση που έχουν δηλωθεί στο δηλωτικό εφαρμογή διαθέσιμη για χρήση. Το σύστημα διαβάζει το πακέτο που έχουν αποσταλεί στο βήμα 1, επιβεβαιώστε το πακέτο (ισοδύναμο με την εκτέλεση της [**Δοκιμής ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) τοπικά), η επεξεργασία των περιεχομένων του πακέτου και αντιγράψτε το πακέτο επεξεργασία σε μια θέση εσωτερικής συστήματος.  Εκτελέστε το cmdlet [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCount
```
Για να δείτε όλους τους τύπους εφαρμογών που έχουν καταχωρηθεί στο σύμπλεγμα, εκτελέστε το cmdlet [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Βήμα 3: Δημιουργία την παρουσία της εφαρμογής
Μια εφαρμογή μπορεί να χρησιμοποιηθεί με οποιαδήποτε έκδοση τύπο εφαρμογής που έχει καταχωρηθεί με επιτυχία, χρησιμοποιώντας την εντολή [**Δημιουργία ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) . Το όνομα της εφαρμογής κάθε δηλώνεται στην ανάπτυξη του χρόνου και πρέπει να ξεκινήσετε με το **ύφασμα:** συνδυασμού και είναι μοναδικός για κάθε παρουσία της εφαρμογής. Η εφαρμογή πληκτρολογήστε το όνομα και τύπος έκδοση της εφαρμογής δηλώνονται στο αρχείο **ApplicationManifest.xml** το πακέτο εφαρμογών. Εάν οποιαδήποτε προεπιλεγμένες υπηρεσίες καθορίστηκαν στο δηλωτικό εφαρμογής από τον τύπο εφαρμογής-στόχου, στη συνέχεια, αυτές δημιουργούνται αυτήν τη στιγμή.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Η εντολή [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) παραθέτει όλες τις εμφανίσεις εφαρμογής που δημιουργήθηκαν με επιτυχία, μαζί με τις συνολικές κατάστασης. Η εντολή [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) παραθέτει όλες τις παρουσίες υπηρεσίας που έχουν δημιουργηθεί με επιτυχία μέσα σε μια παρουσία της δεδομένης εφαρμογής. Παρατίθενται προεπιλεγμένες υπηρεσίες (εάν υπάρχουν).

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Εργασία: Αναβάθμιση μιας εφαρμογής υπηρεσίας ύφασμα
Μπορείτε να αναβαθμίσετε μια προηγουμένως ανεπτυγμένος εφαρμογή υπηρεσίας υφάσματος με ένα πακέτο εφαρμογών ενημερωμένες. Αυτή η εργασία αναβαθμίζει την εφαρμογή WordCount που έχει αναπτυχθεί σε "εργασίας: ανάπτυξη μιας εφαρμογής υπηρεσίας ύφασμα." Διαβάστε μέσω [εφαρμογής υπηρεσίας ύφασμα αναβάθμισης](service-fabric-application-upgrade.md) για περισσότερες πληροφορίες.

Για να διατηρήσετε πράγματα απλό για αυτό το παράδειγμα, μόνο ο αριθμός έκδοσης εφαρμογής ενημερώθηκε στο πακέτο εφαρμογών WordCountV2 έχουν δημιουργηθεί με τις προϋποθέσεις. Ένα σενάριο πιο ρεαλιστική συνεπάγεται ενημέρωση αρχείων κώδικα, ρύθμιση παραμέτρων ή των δεδομένων σας υπηρεσίας και, στη συνέχεια, φτιάχνετε και συσκευασία της εφαρμογής με αριθμούς ενημερωμένη έκδοση.  

### <a name="step-1-upload-the-updated-application-package"></a>Βήμα 1: Αποστολή του πακέτου εφαρμογών ενημερωμένες
Η εφαρμογή v1 WordCount είναι έτοιμη για αναβάθμιση. Εάν ανοίξετε ένα παράθυρο του PowerShell ως διαχειριστής και πληκτρολογήστε [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx), θα δείτε αυτήν την έκδοση 1.0.0 του τον τύπο εφαρμογής WordCount έχει αναπτυχθεί.  

Τώρα μπορείτε να αντιγράψετε το πακέτο εφαρμογών ενημερωμένες στο χώρο αποθήκευσης ειδώλου ύφασμα υπηρεσίας (όπου είναι αποθηκευμένες στα πακέτα εφαρμογών από ύφασμα υπηρεσίας). Η παράμετρος **ApplicationPackagePathInImageStore** ενημερώνει ύφασμα υπηρεσία όπου είναι δυνατό να εντοπίσει το πακέτο εφαρμογών. Την παρακάτω εντολή αντιγράφει το πακέτο εφαρμογών για να **WordCountV2** στο χώρο αποθήκευσης εικόνα:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Βήμα 2: Καταχωρήστε τον τύπο της ενημερωμένης εφαρμογής
Το επόμενο βήμα είναι να καταχωρήσετε τη νέα έκδοση της εφαρμογής με ύφασμα υπηρεσίας, το οποίο μπορεί να εκτελεστεί χρησιμοποιώντας το cmdlet [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) :

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Βήμα 3: Έναρξη της αναβάθμισης
Διάφορες αναβάθμισης παράμετροι χρονικών ορίων και κριτήρια εύρυθμης λειτουργίας μπορούν να εφαρμοστούν σε εφαρμογή αναβαθμίσεις του στοιχείου. Διαβάστε την [εφαρμογή αναβάθμισης παραμέτρους](service-fabric-application-upgrade-parameters.md) και [διαδικασία αναβάθμισης](service-fabric-application-upgrade.md) εγγράφων για να μάθετε περισσότερα. Όλες οι υπηρεσίες και τις εμφανίσεις πρέπει να είναι _σε καλή κατάσταση_ μετά την αναβάθμιση.  Ορίστε το **HealthCheckStableDuration** σε 60 δευτερόλεπτα (έτσι ώστε να είναι σε καλή κατάσταση των υπηρεσιών για τουλάχιστον 20 δευτερόλεπτα πριν από την αναβάθμιση προχωρά στο επόμενο αναβάθμισης τομέα).  Ορίσετε την **UpgradeDomainTimeout** 1200 δευτερόλεπτα και το **UpgradeTimeout** 3000 δευτερόλεπτα. Τέλος, μπορείτε να ρυθμίσετε το **UpgradeFailureAction** να **Επαναφορά**, η οποία ζητά ότι ύφασμα υπηρεσίας θα γίνει επαναφορά της εφαρμογής στην προηγούμενη έκδοση εάν παρουσιαστούν αποτυχίες κατά την αναβάθμιση.

Τώρα, μπορείτε να ξεκινήσετε την αναβάθμιση της εφαρμογής, χρησιμοποιώντας το cmdlet [**ServiceFabricApplicationUpgrade έναρξης**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Σημειώστε ότι το όνομα της εφαρμογής είναι το ίδιο με το όνομα της εφαρμογής προηγουμένως ανεπτυγμένος v1.0.0 (ύφασμα: / WordCount). Υπηρεσία ύφασμα χρησιμοποιεί αυτό το όνομα για να προσδιορίσετε ποια εφαρμογή γρήγορα αναβαθμίζεται. Εάν ορίσετε τη λήξη περιόδων για να είναι πολύ μικρό, ενδέχεται να συναντήσετε ένα μήνυμα αποτυχία λήξης χρονικού ορίου που αναφέρει το πρόβλημα. Αναφορά σε [Αντιμετώπιση προβλημάτων εφαρμογής αναβαθμίσεις](service-fabric-application-upgrade-troubleshooting.md)ή να αυξήσετε τη λήξη περιόδων.

### <a name="step-4-check-upgrade-progress"></a>Βήμα 4: Αναβάθμισης ελέγχου προόδου
Μπορείτε να παρακολουθείτε την πρόοδο αναβάθμισης εφαρμογής, χρησιμοποιώντας την [Εξερεύνηση ύφασμα υπηρεσίας](service-fabric-visualizing-your-cluster.md)ή χρησιμοποιώντας το cmdlet [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) :

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Μετά από μερικά λεπτά, το cmdlet [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) εμφανίζει ότι έχουν αναβαθμιστεί σε όλους τους τομείς αναβάθμισης (ολοκληρωθεί).

## <a name="task-test-a-service-fabric-application"></a>Εργασία: Δοκιμή μιας εφαρμογής υπηρεσίας ύφασμα

Για να γράψετε υπηρεσίες υψηλής ποιότητας, οι προγραμματιστές πρέπει να μπορούν να προκαλέσουν σφάλματα αναξιόπιστες υποδομή για να ελέγξετε τη σταθερότητα των υπηρεσιών τους. Υπηρεσία ύφασμα δίνει στους προγραμματιστές τη δυνατότητα να προκαλέσουν σφάλμα ενέργειες και να ελέγξετε τις υπηρεσίες παρουσία αποτυχίες, χρησιμοποιώντας τα σενάρια δοκιμής χάος και ανακατεύθυνσης.  Διαβάστε μέσω [Επισκόπηση με δυνατότητα δοκιμής](service-fabric-testability-overview.md) για πρόσθετες πληροφορίες.

### <a name="step-1-run-the-chaos-test-scenario"></a>Βήμα 1: Εκτέλεση το σενάριο χάος δοκιμής
Το σενάριο δοκιμής χάος δημιουργεί σφάλματα σε ολόκληρο το σύμπλεγμα ύφασμα υπηρεσίας. Το σενάριο συμπιέζει σφάλματα γενικά προβληθούν μέσω μήνες ή έτη για μερικές ώρες. Ο συνδυασμός διεμπλεκόμενο σφαλμάτων με υψηλή σφαλμάτων χρέωσης εντοπίζει τη γωνία περιπτώσεις που αλλιώς μπορεί να λείπουν. Το παρακάτω παράδειγμα εκτελείται το σενάριο δοκιμής χάος για 60 λεπτά.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Βήμα 2: Εκτέλεση το σενάριο δοκιμής ανακατεύθυνσης
Δοκιμή του εφεδρικού προορισμών σενάριο μια συγκεκριμένη υπηρεσία διαμερίσματα αντί για ολόκληρο το σύμπλεγμα και αφήνει επηρεάζονται άλλες υπηρεσίες. Το σενάριο επαναλαμβάνεται μέσω μιας ακολουθίας προσομοιωμένη σφαλμάτων στην υπηρεσία επικύρωση ενώ εκτελείται το επιχειρηματικής λογικής. Αποτυχία επικύρωσης υπηρεσίας υποδεικνύει ένα θέμα που χρειάζεται περαιτέρω έρευνα. Η δοκιμή ανακατεύθυνσης υποδείξει σφαλμάτων μόνο μία κάθε φορά, σε αντίθεση με το σενάριο δοκιμής χάος, που μπορεί να προκαλέσει πολλά σφάλματα. Το παρακάτω παράδειγμα εκτελείται η δοκιμή ανακατεύθυνσης για 60 λεπτά υφάσματος: / WordCount/WordCountService υπηρεσίας.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Εργασία: Κατάργηση μιας εφαρμογής υπηρεσίας ύφασμα
Μπορείτε να διαγράψετε μια παρουσία μιας εφαρμογής, να καταργήσετε τον τύπο εφαρμογής προμήθεια του φακέλου από το σύμπλεγμα και να καταργήσετε το πακέτο εφαρμογών από το ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Βήμα 1: Κατάργηση μια παρουσία της εφαρμογής
Όταν μια παρουσία της εφαρμογής δεν είναι πλέον απαραίτητη, μπορείτε να την καταργήσετε οριστικά χρησιμοποιώντας το cmdlet [**Κατάργηση ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) . Αυτό καταργεί αυτόματα όλες τις υπηρεσίες που ανήκουν στην εφαρμογή, οριστική κατάργηση όλη την κατάσταση υπηρεσίας. Αυτή η λειτουργία δεν μπορεί να αντιστραφεί και είναι δυνατή η ανάκτησή κατάσταση εφαρμογής.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Βήμα 2: Unregister ο τύπος της εφαρμογής
Όταν δεν χρειάζεστε πλέον μια συγκεκριμένη έκδοση του έναν τύπο εφαρμογής, καταργήστε το, χρησιμοποιώντας το cmdlet [**Unregister ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) . Κατάργηση καταχώρησης τύπους που δεν χρησιμοποιούνται εκδόσεις χώρου αποθήκευσης που χρησιμοποιείται από το πακέτο εφαρμογών στο χώρο αποθήκευσης της εικόνας. Ένας τύπος εφαρμογής μπορεί να είναι μη καταχωρημένες με την προϋπόθεση ότι δεν υπάρχουν εφαρμογές δημιουργία παρουσίας του σε σχέση με το ή σε εκκρεμότητα αναβαθμίσεις εφαρμογής σε αυτήν.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Βήμα 3: Κατάργηση του πακέτου εφαρμογών
Μετά την κατάργηση της καταχώρησης ο τύπος της εφαρμογής, το πακέτο εφαρμογών μπορούν να καταργηθούν από το χώρο αποθήκευσης της εικόνας χρησιμοποιώντας το cmdlet [**Κατάργηση ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) .

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Επόμενα βήματα
[Κύκλος ζωής εφαρμογής υπηρεσίας ύφασμα](service-fabric-application-lifecycle.md)

[Ανάπτυξη μιας εφαρμογής](service-fabric-deploy-remove-applications.md)

[Αναβάθμιση εφαρμογής](service-fabric-application-upgrade.md)

[Cmdlet του Azure ύφασμα υπηρεσίας](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Cmdlet με δυνατότητα δοκιμής του Azure ύφασμα υπηρεσίας](https://msdn.microsoft.com/library/azure/mt125844.aspx)

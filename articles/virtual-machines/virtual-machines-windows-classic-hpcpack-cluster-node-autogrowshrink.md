<properties
 pageTitle="Κόμβοι συμπλέγματος πακέτο HPC Autoscale | Microsoft Azure"
 description="Αυτόματη μεγέθυνση και σμίκρυνση του αριθμού των κόμβους υπολογιστικών σύμπλεγμα HPC πακέτο στο Azure"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a>Αυτόματη μεγέθυνση και σμίκρυνση τους πόρους σύμπλεγμα HPC πακέτο στο Azure σύμφωνα με το φόρτο εργασίας συμπλέγματος




Εάν αναπτύσσετε κόμβους Azure "καταιγισμού" στο πακέτο HPC συμπλέγματος ή μπορείτε να δημιουργήσετε ένα σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure, ενδέχεται να θέλετε ένας τρόπος για την αυτόματη μεγέθυνση ή σμίκρυνση τον αριθμό των πόρων Azure υπολογισμού όπως κόμβους ή πυρήνων σύμφωνα με το τρέχον φόρτο εργασίας στο σύμπλεγμα. Αυτό σας επιτρέπει να χρησιμοποιήσετε Azure τους πόρους σας πιο αποτελεσματικά και να ελέγξετε το κόστος τους.
Για να το κάνετε αυτό, ρυθμίστε την ιδιότητα σύμπλεγμα HPC πακέτο **AutoGrowShrink**. Εναλλακτικά, μπορείτε να εκτελέσετε τη δέσμη ενεργειών HPC PowerShell **AzureAutoGrowShrink.ps1** που έχει εγκατασταθεί με το πακέτο HPC.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Επίσης, αυτήν τη στιγμή που μπορεί να μόνο αυτόματα μεγέθυνση και σμίκρυνση κόμβους υπολογιστικών HPC πακέτο που χρησιμοποιούν ένα λειτουργικό σύστημα Windows Server.

## <a name="set-the-autogrowshrink-cluster-property"></a>Ορίστε την ιδιότητα AutoGrowShrink συμπλέγματος

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* **HPC Pack 2012 R2 ενημερωμένη έκδοση 2 ή νεότερες εκδόσεις σύμπλεγμα** - μπορεί να είναι ο κόμβος κεφαλής συμπλέγματος αναπτυχθεί είτε στην εσωτερική εγκατάσταση ή σε μια Εικονική Azure. Ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα υβριδική με HPC πακέτο](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) για να ξεκινήσετε με έναν κόμβο κεφαλής εσωτερικής εγκατάστασης και τους κόμβους Azure "καταιγισμού". Ανατρέξτε στο θέμα η [δέσμη ενεργειών ανάπτυξης IaaS πακέτο HPC](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) να αναπτύσσουν γρήγορα ένα σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure.


* **Για ένα σύμπλεγμα με μια κεφαλή κόμβο στο Azure** - Εάν χρησιμοποιείτε τη δέσμη ενεργειών ανάπτυξης HPC πακέτο IaaS για να δημιουργήσετε το σύμπλεγμα, ενεργοποιήστε την ιδιότητα σύμπλεγμα **AutoGrowShrink** , ορίζοντας την επιλογή AutoGrowShrink στο αρχείο ρύθμισης παραμέτρων συμπλέγματος. Για λεπτομέρειες, ανατρέξτε στην τεκμηρίωση που συνοδεύει τη [λήψη δεσμών ενεργειών](https://www.microsoft.com/download/details.aspx?id=44949). 

    Εναλλακτικά, μπορείτε να ενεργοποιήσετε την ιδιότητα σύμπλεγμα **AutoGrowShrink** μετά την ανάπτυξη του συμπλέγματος, χρησιμοποιώντας τις εντολές του HPC PowerShell που περιγράφονται στην επόμενη ενότητα. Για να προετοιμαστείτε για αυτό, ολοκληρώστε πρώτα τα παρακάτω βήματα:
    1. Ρύθμιση παραμέτρων ενός πιστοποιητικού Azure διαχείρισης στον κόμβο κεφαλής και στην Azure συνδρομής. Για μια ανάπτυξη δοκιμής μπορείτε να χρησιμοποιήσετε το πιστοποιητικό αυτόματης υπογραφής προεπιλεγμένη Microsoft Azure HPC που HPC πακέτο εγκαθίσταται στον κόμβο κεφαλής, και απλώς να αποστείλετε αυτό το πιστοποιητικό για τη συνδρομή σας στο Azure. Για επιλογές και βήματα, ανατρέξτε στο θέμα η [βιβλιοθήκη TechNet οδηγίες](https://technet.microsoft.com/library/gg481759.aspx).
    2. Εκτελέστε την εντολή **regedit** στον κόμβο κεφαλής, μεταβείτε στο HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo και προσθέστε μια νέα τιμή συμβολοσειράς. Ορίστε το όνομα της τιμής για "Αποτύπωση" και την τιμή δεδομένων για την αποτύπωση του πιστοποιητικού στο βήμα 1.


### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a>Για να ορίσετε την ιδιότητα AutoGrowShrink τις εντολές του HPC PowerShell

Παρακάτω είναι δείγματος τις εντολές του HPC PowerShell για να ορίσετε **AutoGrowShrink** και για να ρυθμίσετε τη συμπεριφορά με πρόσθετες παραμέτρους. Ανατρέξτε στο θέμα [AutoGrowShrink παράμετροι](#AutoGrowShrink-parameters) αργότερα σε αυτό το άρθρο για την πλήρη λίστα των ρυθμίσεων. 

Για να εκτελέσετε αυτές τις εντολές, ξεκινήστε HPC PowerShell στον κόμβο κεφαλής συμπλέγματος ως διαχειριστής.

**Για να ενεργοποιήσετε την ιδιότητα AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 1

**Για να απενεργοποιήσετε την ιδιότητα AutoGrowShrink**

    Set-HpcClusterProperty –EnableGrowShrink 0

**Για να αλλάξετε το διάστημα μεγέθυνση σε λεπτά**

    Set-HpcClusterProperty –GrowInterval <interval>

**Για να αλλάξετε το διάστημα σμίκρυνση σε λεπτά**

    Set-HpcClusterProperty –ShrinkInterval <interval>

**Για να προβάλετε την τρέχουσα ρύθμιση παραμέτρων του AutoGrowShrink**

    Get-HpcClusterProperty –AutoGrowShrink

### <a name="autogrowshrink-parameters"></a>Παράμετροι AutoGrowShrink

Οι παρακάτω είναι AutoGrowShrink παράμετροι που μπορείτε να τροποποιήσετε, χρησιμοποιώντας την εντολή **Ορισμός HpcClusterProperty** .

* **EnableGrowShrink** - αλλαγής για να ενεργοποιήσετε ή να απενεργοποιήσετε την ιδιότητα **AutoGrowShrink** .
* **ParamSweepTasksPerCore** - αριθμός των εργασιών parametric εκκαθάριση αύξηση μία πυρήνα. Η προεπιλογή είναι αύξηση μία πυρήνα ανά εργασία. 
 
    >[AZURE.NOTE] HPC πακέτο QFE KB3134307 αλλάζει **ParamSweepTasksPerCore** σε **TasksPerResourceUnit**. Αυτό είναι με βάση τον τύπο πόρου εργασία και μπορούν να κόμβου, socket ή το μέγεθος των κύριων.
    
* **GrowThreshold** - όριο σε ουρά εργασιών για να ενεργοποιήσετε την αυτόματη γεωμετρική πρόοδος. Η προεπιλογή είναι 1, γεγονός που σημαίνει ότι που εάν υπάρχουν 1 ή περισσότερες εργασίες σε κατάσταση σε ουρά, αυτόματη μεγέθυνση κόμβους.
* **GrowInterval** - χρονικό διάστημα σε λεπτά για να ενεργοποιήσετε την αυτόματη γεωμετρική πρόοδος. Το προεπιλεγμένο χρονικό διάστημα είναι 5 λεπτά.
* **ShrinkInterval** - χρονικό διάστημα σε λεπτά για να ενεργοποιήσετε την αυτόματη συρρίκνωση. Το προεπιλεγμένο χρονικό διάστημα είναι 5 λεπτά. |
* **ShrinkIdleTimes** - αριθμός συνεχής ελέγχων για να σμικρύνετε για να υποδείξετε ότι οι κόμβοι είναι σε αδράνεια. Η προεπιλογή είναι 3 φορές. Για παράδειγμα, εάν το **ShrinkInterval** είναι 5 λεπτά, πακέτο HPC ελέγχει εάν ο κόμβος είναι σε αδράνεια κάθε 5 λεπτά. Εάν οι κόμβοι είναι στην κατάσταση αδράνειας αφού 3 συνεχής ελέγχει (15 λεπτά), στη συνέχεια, πακέτο HPC συρρικνώνεται αυτόν τον κόμβο.
* **ExtraNodesGrowRatio** - επιπλέον ποσοστό του κόμβους αύξηση για εργασίες μηνύματος που περνά μέσα περιβάλλοντος εργασίας (MPI). Η προεπιλεγμένη τιμή είναι 1, γεγονός που σημαίνει ότι πακέτο HPC μεγαλώνει κόμβους 1% για εργασίες MPI. 
* **GrowByMin** - Μετάβαση στο υποδεικνύουν αν η πολιτική διπλάσιο μέγεθος βασίζεται σε τους ελάχιστους πόρους που απαιτούνται για το έργο. Η προεπιλογή είναι false, γεγονός που σημαίνει ότι πακέτο HPC μεγαλώνει κόμβους για τις εργασίες με βάση το μέγιστο πόρους που απαιτούνται για τις εργασίες.
* **SoaJobGrowThreshold** - όριο των εισερχόμενων SOA αιτήσεων για να ενεργοποιήσετε την αυτόματη μεγέθυνση διαδικασία. Η προεπιλεγμένη τιμή είναι 50000.  
    
    >[AZURE.NOTE] Αυτή η παράμετρος υποστηρίζεται εκκίνηση στο HPC Pack 2012 R2 Update 3.
    
* **SoaRequestsPerCore** -αριθμός των εισερχόμενων SOA αιτήσεων για ανάπτυξη μία πυρήνα. Η προεπιλεγμένη τιμή είναι 20000.  

    >[AZURE.NOTE] Αυτή η παράμετρος υποστηρίζεται εκκίνηση στο HPC Pack 2012 R2 ενημερωμένη έκδοση 3.

### <a name="mpi-example"></a>Παράδειγμα MPI

Από προεπιλογή πακέτο HPC μεγαλώνει 1% επιπλέον κόμβους για εργασίες MPI (**ExtraNodesGrowRatio** έχει οριστεί σε 1). Η αιτία είναι ότι MPI μπορεί να απαιτεί πολλούς κόμβους και η εργασία μπορεί να εκτελεστεί μόνο όταν είστε έτοιμοι όλους τους κόμβους. Όταν Azure ξεκινά κόμβους, μερικές φορές έναν κόμβο ίσως χρειαστεί περισσότερο χρόνο για να ξεκινήσετε από άλλα άτομα, προκαλεί άλλους κόμβους να είναι σε αδράνεια κατά την αναμονή για αυτόν τον κόμβο να είστε έτοιμοι. Με τη μεγέθυνση επιπλέον κόμβους, πακέτο HPC αυτήν τη στιγμή σε αναμονή πόρων μειώνει και ενδεχομένως αποθηκεύει κόστους. Για να αυξήσετε το ποσοστό της επιπλέον κόμβους για εργασίες MPI (για παράδειγμα, για να 10%), εκτελέστε την εντολή παρόμοια με

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>Παράδειγμα SOA

Από προεπιλογή, **SoaJobGrowThreshold** έχει οριστεί σε 50000 και **SoaRequestsPerCore** έχει οριστεί σε 200000. Εάν υποβάλετε μια SOA έργου με 70000 αιτήσεις, θα υπάρχουν μία εργασία σε ουρά και εισερχόμενες αιτήσεις είναι 70000. Σε αυτήν την περίπτωση μεγαλώνει πακέτο HPC 1 πυρήνα για αυτήν την εργασία, καθώς και για τις εισερχόμενες αιτήσεις, θα αυξηθεί (70000-50000) / βασικών 20000 = 1, έτσι θα αυξηθεί 2 πυρήνων για αυτό το έργο SOA στο σύνολο.

## <a name="run-the-azureautogrowshrinkps1-script"></a>Εκτελέστε τη δέσμη ενεργειών AzureAutoGrowShrink.ps1

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* **HPC πακέτο 2012 R2 ενημέρωση 1 ή νεότερη έκδοση σύμπλεγμα** - η δέσμη ενεργειών **AzureAutoGrowShrink.ps1** εγκαθίσταται στο φάκελο % CCP_HOME % Ανακύκλωσης. Μπορεί να είναι ο κόμβος κεφαλής συμπλέγματος αναπτυχθεί είτε στην εσωτερική εγκατάσταση ή σε μια Εικονική Azure. Ανατρέξτε στο θέμα [Ρύθμιση ένα σύμπλεγμα υβριδική με HPC πακέτο](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) για να ξεκινήσετε με έναν κόμβο κεφαλής εσωτερικής εγκατάστασης και τους κόμβους Azure "καταιγισμού". Δείτε τη [δέσμη ενεργειών ανάπτυξης IaaS πακέτο HPC](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) να αναπτύσσουν γρήγορα ένα σύμπλεγμα HPC πακέτο στο ΣΠΣ Azure ή χρησιμοποιήστε ένα [πρότυπο Azure γρήγορη έναρξη](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).

* **Azure PowerShell 0.8.12** - αυτήν τη στιγμή η δέσμη ενεργειών εξαρτάται από τη συγκεκριμένη έκδοση του Azure PowerShell. Εάν εκτελείτε μια νεότερη έκδοση στον κόμβο κεφαλής, ίσως χρειαστεί να υποβιβάσετε Azure PowerShell [έκδοση 0.8.12](http://az412849.vo.msecnd.net/downloads03/azure-powershell.0.8.12.msi) για να εκτελέσετε τη δέσμη ενεργειών. 

* **Για ένα σύμπλεγμα με κόμβους Azure καταιγισμού** - εκτελέστε τη δέσμη ενεργειών σε έναν υπολογιστή-πελάτη όπου είναι εγκατεστημένο το πακέτο HPC ή στον κόμβο κεφαλής. Εάν εκτελείται σε έναν υπολογιστή-πελάτη, βεβαιωθείτε ότι να ορίσετε τη μεταβλητή $env: CCP_SCHEDULER σωστά, ώστε να οδηγούν σε κεφαλής κόμβο. Οι κόμβοι Azure "καταιγισμού" πρέπει να προστεθεί ήδη στο σύμπλεγμα, αλλά μπορούν να σε κατάσταση δεν αναπτύσσεται.


* **Για ένα σύμπλεγμα αναπτυχθεί στο ΣΠΣ Azure** - εκτελέστε τη δέσμη ενεργειών στον κόμβο κεφαλής Εικονική, επειδή εξαρτάται από το **HpcIaaSNode.ps1 Έναρξη** και **Διακοπή HpcIaaSNode.ps1** δέσμες ενεργειών που είναι εγκατεστημένα εκεί. Αυτές τις δέσμες ενεργειών επιπλέον απαιτούν ένα πιστοποιητικό διαχείρισης Azure ή αρχείο ρυθμίσεων δημοσίευση (ανατρέξτε στο θέμα [Διαχείριση τον υπολογισμό κόμβους σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)). Βεβαιωθείτε ότι όλα τα κόμβος ΣΠΣ πρέπει έχουν ήδη προστεθεί στο σύμπλεγμα. Μπορεί να σε κατάσταση διακοπή.

### <a name="syntax"></a>Σύνταξη

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]

```
### <a name="parameters"></a>Παράμετροι

 * **NodeTemplates** - ονόματα των προτύπων κόμβο για να ορίσετε την εμβέλεια για τους κόμβους για μεγέθυνση και σμίκρυνση. Εάν δεν καθορίζονται (η προεπιλεγμένη τιμή είναι @()), όλους τους κόμβους στην ομάδα κόμβο **AzureNodes** είναι στο εύρος όταν **NodeType** έχει την τιμή AzureNodes και όλους τους κόμβους στην ομάδα κόμβο **ComputeNodes** έχουν εμβέλεια όταν **NodeType** έχει την τιμή ComputeNodes.

 * **JobTemplates** - ονόματα από τα πρότυπα έργων, για να ορίσετε την εμβέλεια για τους κόμβους για ανάπτυξη.

 * **NodeType** - τον τύπο του κόμβου για μεγέθυνση και σμίκρυνση. Υποστηριζόμενες τιμές είναι οι εξής:

     * **AzureNodes** – για τους κόμβους Azure PaaS (καταιγισμού) σε μια εσωτερική ή σύμπλεγμα Azure IaaS.

     * **ComputeNodes** - για κόμβος ΣΠΣ μόνο σε ένα σύμπλεγμα Azure IaaS.

* **NumOfQueuedJobsPerNodeToGrow** - αριθμός σε ουρά εργασιών που απαιτούνται για την ανάπτυξη έναν κόμβο.

* **NumOfQueuedJobsToGrowThreshold** - ο οριακός αριθμός σε ουρά εργασίες για να ξεκινήσετε τη διαδικασία μεγέθυνση.

* **NumOfActiveQueuedTasksPerNodeToGrow** - ο αριθμός των ενεργών εργασιών σε ουρά που απαιτούνται για την ανάπτυξη έναν κόμβο. Εάν έχει καθοριστεί **NumOfQueuedJobsPerNodeToGrow** με τιμή μεγαλύτερη του 0, η παράμετρος αυτή παραβλέπεται.

* **NumOfActiveQueuedTasksToGrowThreshold** - ο οριακός αριθμός ενεργών εργασιών σε ουρά για να ξεκινήσετε τη διαδικασία μεγέθυνση.

* **NumOfInitialNodesToGrow** - τον αρχικό αριθμό ελάχιστη κόμβους αύξηση εάν όλους τους κόμβους εύρος έχουν **Αναπτυχθεί δεν** ή **Διακοπή (Deallocated)**.

* **GrowCheckIntervalMins** - το χρονικό διάστημα σε λεπτά μεταξύ των ελέγχων για ανάπτυξη.

* **ShrinkCheckIntervalMins** - το χρονικό διάστημα σε λεπτά μεταξύ των ελέγχων για σμίκρυνση.

* **ShrinkCheckIdleTimes** - τον αριθμό των συνεχής σμίκρυνση ελέγχων (διαχωρισμένα με **ShrinkCheckIntervalMins**) για να υποδείξετε ότι οι κόμβοι είναι σε αδράνεια.

* **UseLastConfigurations** - τις προηγούμενες ρυθμίσεις παραμέτρων που αποθηκεύονται στο αρχείο όρισμα.

* **ArgFile**- στο όνομα του αρχείου όρισμα που χρησιμοποιείται για να αποθηκεύσετε και να ενημερώσετε τις ρυθμίσεις παραμέτρων για να εκτελέσετε τη δέσμη ενεργειών.

* **LogFilePrefix** - το πρόθεμα όνομα του αρχείου καταγραφής. Μπορείτε να καθορίσετε μια διαδρομή. Από προεπιλογή, το αρχείο καταγραφής εγγράφεται του τρέχοντος καταλόγου εργασίας.

### <a name="example-1"></a>Παράδειγμα 1

Το παρακάτω παράδειγμα ρυθμίζει τις παραμέτρους τους κόμβους Azure καταιγισμού αναπτυχθεί με το πρότυπο AzureNode προεπιλεγμένη για μεγέθυνση και σμίκρυνση αυτόματα. Εάν όλες οι κόμβοι είναι αρχικά σε κατάσταση **Δεν αναπτύσσεται** , τουλάχιστον 3 κόμβοι είναι αποτελέσματα. Εάν ο αριθμός των εργασιών σε ουρά υπερβαίνει 8, η δέσμη ενεργειών ξεκινά κόμβους μέχρι η αναλογία σε ουρά εργασιών για να **NumOfQueuedJobsPerNodeToGrow**υπερβαίνει τον αριθμό τους. Εάν βρεθεί έναν κόμβο να είναι σε αδράνεια σε 3 φορές διαδοχικές αδράνειας, έχει διακοπεί.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Παράδειγμα 2

Το παρακάτω παράδειγμα ρυθμίζει τις παραμέτρους του Azure κόμβος ΣΠΣ αναπτυχθεί με το πρότυπο ComputeNode προεπιλεγμένη για μεγέθυνση και σμίκρυνση αυτόματα.
Οι εργασίες που έχει ρυθμιστεί από το προεπιλεγμένο πρότυπο έργου ορίζουν το εύρος της το φόρτο εργασίας στο σύμπλεγμα. Εάν όλες οι κόμβοι αρχικά έχουν διακοπεί, τουλάχιστον 5 κόμβοι είναι αποτελέσματα. Εάν ο αριθμός των ενεργών εργασιών σε ουρά υπερβαίνει 15, η δέσμη ενεργειών ξεκινά κόμβους μέχρι ο λόγος των ενεργών εργασιών σε ουρά να **NumOfActiveQueuedTasksPerNodeToGrow**υπερβαίνει τον αριθμό τους. Εάν βρεθεί έναν κόμβο να είναι σε αδράνεια σε 10 φορές διαδοχικές αδράνειας, έχει διακοπεί.

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
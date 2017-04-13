<properties
 pageTitle="Εκτέλεση ΑΣΤΈΡΙ-ΚΦΜ + με Pack HPC σε VM Linux | Microsoft Azure"
 description="Αναπτύξετε ένα σύμπλεγμα πακέτο HPC Microsoft στην Azure και να εκτελέσετε ένα ΑΣΤΈΡΙ-ΚΦΜ + εργασίας με βάση πολλές Linux τον υπολογισμό κόμβους σε ένα δίκτυο RDMA."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Εκτέλεση ΑΣΤΈΡΙ-ΚΦΜ + με το Microsoft HPC πακέτο σε μια RDMA Linux συμπλέγματος στο Azure
Αυτό το άρθρο σάς δείχνει πώς να αναπτύξετε ένα σύμπλεγμα πακέτο HPC Microsoft Azure και εκτέλεση ενός [ΑΣΤΈΡΙ CD-adapco-ΚΦΜ +](http://www.cd-adapco.com/products/star-ccm%C2%AE) εργασίας με βάση πολλές κόμβους υπολογιστικών Linux που συνδέονται μεταξύ τους με InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Πακέτο HPC Microsoft παρέχει δυνατότητες για να εκτελέσετε μια ποικιλία ευρείας κλίμακας HPC και παράλληλες εφαρμογές, συμπεριλαμβανομένων των εφαρμογών MPI, σε συμπλεγμάτων εικονικές μηχανές Windows Azure. Πακέτο HPC υποστηρίζει επίσης Linux HPC εφαρμογές που εκτελούνται σε VM κόμβος Linux που έχουν αναπτυχθεί σε ένα σύμπλεγμα HPC πακέτο. Για μια εισαγωγή στη χρήση Linux τον υπολογισμό κόμβους με HPC Pack, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Linux κόμβους υπολογιστικών σε ένα σύμπλεγμα HPC πακέτο στο Azure](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Ρυθμίστε ένα σύμπλεγμα HPC πακέτο
Κάντε λήψη των δεσμών ενεργειών ανάπτυξης IaaS πακέτο HPC από το [Κέντρο λήψης](https://www.microsoft.com/en-us/download/details.aspx?id=44949) και εξαγάγετε τα τοπικά.

Azure PowerShell αποτελεί προϋπόθεση. Εάν δεν έχει ρυθμιστεί PowerShell στον τοπικό υπολογιστή σας, διαβάστε το άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

Προς το παρόν, οι εικόνες Linux από το Azure Marketplace (το οποίο περιέχει τα προγράμματα οδήγησης InfiniBand για Azure) είναι για SLES 12, CentOS 6.5 και CentOS 7.1. Σε αυτό το άρθρο είναι με βάση τη χρήση των SLES 12. Για να ανακτήσετε το όνομα του όλες τις εικόνες Linux που υποστηρίζουν HPC στην αγορά, μπορείτε να εκτελέσετε την ακόλουθη εντολή PowerShell:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Το αποτέλεσμα εμφανίζει τη θέση στην οποία είναι διαθέσιμες αυτές τις εικόνες και το όνομα της εικόνας (**όνομα εικόνας**) να χρησιμοποιηθεί με το πρότυπο ανάπτυξης αργότερα.

Πριν να αναπτύξετε το σύμπλεγμα, πρέπει να δημιουργήσετε ένα αρχείο πακέτου HPC ανάπτυξη προτύπου. Επειδή θα σας στόχευσης ένα μικρό σύμπλεγμα, ο κόμβος κεφαλής θα ελεγκτή τομέα και φιλοξενείτε μια τοπική βάση δεδομένων SQL.

Το ακόλουθο πρότυπο θα ανάπτυξη κεφαλής κόμβου, δημιουργήστε ένα αρχείο XML που ονομάζεται **MyCluster.xml**και αντικαταστήστε τις τιμές του **SubscriptionId**, **StorageAccount**, **θέση**, **VMName**και **όνομα_υπηρεσίας** με το δικό σας.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Ξεκινήστε τη δημιουργία κεφαλή κόμβου, εκτελέστε την εντολή PowerShell σε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

Μετά από 20 έως 30 λεπτά, ο κόμβος κεφαλής πρέπει να είστε έτοιμοι. Μπορείτε να συνδεθείτε σε αυτήν από την πύλη του Azure κάνοντας κλικ στο εικονίδιο **σύνδεση** του υπολογιστή εικονική.

Μπορεί να εμφανίσει έχετε για να διορθώσετε την προώθηση DNS. Για να κάνετε αυτό, ξεκινήστε Διαχείριση DNS.

1.  Κάντε δεξί κλικ στο όνομα του διακομιστή στο DNS Manager, επιλέξτε **Ιδιότητες**και, στη συνέχεια, κάντε κλικ στην καρτέλα **προωθήσεων** .

2.  Κάντε κλικ στο κουμπί **Επεξεργασία** για να καταργήσετε οποιαδήποτε προωθήσεις και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

3.  Βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο ελέγχου **Χρήση υποδείξεις ρίζας εάν προωθήσεων δεν είναι διαθέσιμες** και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**.

## <a name="set-up-linux-compute-nodes"></a>Ρύθμιση κόμβους υπολογιστικών Linux
Μπορείτε να αναπτύξετε κόμβους υπολογιστικών Linux, χρησιμοποιώντας το ίδιο πρότυπο ανάπτυξης που χρησιμοποιήσατε για να δημιουργήσετε τον κεφαλής κόμβο.

Αντιγράψτε το αρχείο **MyCluster.xml** από τον τοπικό υπολογιστή στον κόμβο κεφαλής και ενημερώστε την ετικέτα **NodeCount** με τον αριθμό των κόμβους που θέλετε για την ανάπτυξη (< = 20). Να είστε προσεκτικοί να υπάρχει αρκετός διαθέσιμος πυρήνων στο Azure ορίου σας, επειδή κάθε παρουσία A9 θα εκμετάλλευση 16 πυρήνων σε τη συνδρομή σας. Μπορείτε να χρησιμοποιήσετε A8 παρουσίες (8 πυρήνων) αντί για A9, εάν θέλετε να χρησιμοποιήσετε περισσότερες ΣΠΣ στον ίδιο προϋπολογισμό.

Στον κόμβο κεφαλής, αντιγράψτε τις δέσμες ενεργειών ανάπτυξης HPC πακέτο IaaS.

Εκτελέστε τις ακόλουθες εντολές Azure PowerShell σε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα:

1.  Εκτελέστε το **Πρόσθετο AzureAccount** για να συνδεθείτε με τη συνδρομή σας στο Azure.

2.  Εάν έχετε πολλές συνδρομές, εκτελέστε **Get-AzureSubscription** για να τους εμφανίσετε.

3.  Ορισμός προεπιλεγμένης συνδρομή, εκτελώντας το **xxxx επιλέξτε AzureSubscription - SubscriptionName-προεπιλεγμένη** εντολή.

4.  Εκτέλεση **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** για να ξεκινήσετε την ανάπτυξη Linux υπολογιστική κόμβους.

    ![Ανάπτυξη κεφαλή κόμβου σε δράση][hndeploy]

Ανοίξτε το εργαλείο διαχείρισης σύμπλεγμα HPC πακέτο. Μετά από μερικά λεπτά, κόμβους υπολογιστικών Linux τακτικά θα εμφανίζεται στη λίστα με τους κόμβους σύμπλεγμα υπολογισμού. Με τη λειτουργία κλασική ανάπτυξης, IaaS ΣΠΣ δημιουργούνται διαδοχικά. Επομένως, εάν ο αριθμός των κόμβοι είναι σημαντικό, η γρήγορα όλα αναπτυχθεί μπορεί να διαρκέσει ένα σημαντικό χρονικό διάστημα.

![Οι κόμβοι Linux στο HPC πακέτο σύμπλεγμα Manager][clustermanager]

Τώρα που όλους τους κόμβους είναι εγκατάσταση και λειτουργία του συμπλέγματος, υπάρχουν ρυθμίσεις πρόσθετη υποδομή του για να κάνετε.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Ρυθμίστε ένα κοινόχρηστο αρχείο Azure για Windows και Linux κόμβους
Μπορείτε να χρησιμοποιήσετε την υπηρεσία Azure αρχείων για την αποθήκευση των δεσμών ενεργειών, πακέτων εφαρμογών και αρχείων δεδομένων. Αρχείο Azure παρέχει δυνατότητες CIFS επάνω από το χώρο αποθήκευσης αντικειμένων Blob του Azure ως μόνιμη χώρος αποθήκευσης. Έχετε υπόψη ότι αυτή δεν είναι η πιο μεταβλητού μεγέθους λύση, αλλά είναι ο πιο απλός και δεν απαιτεί αποκλειστικό ΣΠΣ.

Δημιουργήστε ένα κοινόχρηστο αρχείο Azure, ακολουθώντας τις οδηγίες στο άρθρο [Γρήγορα αποτελέσματα με το χώρο αποθήκευσης αρχείων Azure στα Windows](..\storage\storage-dotnet-how-to-use-files.md).

Διατηρήστε το όνομα του λογαριασμού σας χώρο αποθήκευσης ως **saname**, το όνομα του αρχείου κοινή χρήση ως **όνομα κοινόχρηστου στοιχείου**και το κλειδί λογαριασμού αποθήκευσης ως **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Ενεργοποίησης κοινή χρήση αρχείου Azure στον κόμβο κεφαλής
Ανοίξτε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα και εκτελέστε την ακόλουθη εντολή για να αποθηκεύσετε τα διαπιστευτήρια σε θάλαμο το τοπικό υπολογιστή:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Στη συνέχεια, για να ενεργοποίησης κοινή χρήση αρχείου Azure, εκτελέστε:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Τοποθετήσετε κοινή χρήση αρχείου Azure σε κόμβους υπολογιστικών Linux
Ένα χρήσιμο εργαλείο που παρέχεται με το πακέτο HPC είναι το εργαλείο clusrun. Μπορείτε να χρησιμοποιήσετε αυτό το εργαλείο γραμμής εντολών για να εκτελέσετε την ίδια εντολή ταυτόχρονα σε ένα σύνολο κόμβους υπολογιστικών. Σε περίπτωση μας, χρησιμοποιούνται για να ενεργοποιήσουν τον κοινόχρηστο πόρο αρχείων Azure και παραμένει το επιβίωσης επανεκκίνηση.
Σε μια αναβαθμισμένη γραμμή εντολών στον κόμβο κεφαλής, εκτελέστε τις ακόλουθες εντολές.

Για να δημιουργήσετε τον κατάλογο ενεργοποίησης:

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Για να ενεργοποίησης κοινή χρήση αρχείου Azure:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Για να διατηρηθεί η κοινή χρήση ενεργοποίησης:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Εγκατάσταση ΑΣΤΈΡΙ-ΚΦΜ +
Azure παρουσίες Εικονική A8 και A9 παρέχουν υποστήριξη InfiniBand και τις δυνατότητες RDMA. Τα προγράμματα οδήγησης πυρήνα που αυτές τις δυνατότητες είναι διαθέσιμες για το Windows Server 2012 R2, SUSE 12, CentOS 6.5 και CentOS 7.1 εικόνες από το Azure Marketplace. Microsoft MPI και Intel MPI (έκδοση 5.x) είναι οι δύο βιβλιοθήκες MPI που υποστηρίζουν αυτά τα προγράμματα οδήγησης στο Azure.

ΑΣΤΈΡΙ CD adapco-ΚΦΜ + αφήστε 11.x και αργότερα συνοδεύει Intel MPI έκδοση 5.x, ώστε να περιλαμβάνεται InfiniBand υποστήριξη για Azure.

Λήψη του Linux64 ΑΣΤΈΡΙ-πακέτο ΚΦΜ + από την [πύλη CD-adapco](https://steve.cd-adapco.com). Σε περίπτωση μας, χρησιμοποιήσαμε έκδοση 11.02.010 στο μεικτές ακρίβεια.

Στον κόμβο κεφαλής, στο παράθυρο κοινή χρήση αρχείου Azure **/hpcdata** , δημιουργήστε μια δέσμη ενεργειών κελύφους με το όνομα **setupstarccm.sh** με το παρακάτω περιεχόμενο. Αυτή η δέσμη ενεργειών θα εκτελείται κάθε κόμβος για να ρυθμίσετε ΑΣΤΈΡΙ-ΚΦΜ + τοπικά.

#### <a name="sample-setupstarcmsh-script"></a>Δείγμα δέσμης ενεργειών setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Τώρα, για να ρυθμίσετε ΑΣΤΈΡΙ-ΚΦΜ + σε όλες τις Linux τον υπολογισμό κόμβους, ανοίξτε μια γραμμή εντολών με αναβαθμισμένα δικαιώματα και εκτελέστε την ακόλουθη εντολή:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Κατά την εκτέλεση της εντολής, μπορείτε να παρακολουθείτε τη χρήση της CPU με τη χρήση του χάρτη θερμότητας της διαχείρισης συμπλέγματος. Μετά από μερικά λεπτά, όλους τους κόμβους πρέπει να είναι σωστά ρύθμιση.

## <a name="run-star-ccm-jobs"></a>Εκτέλεση ΑΣΤΈΡΙ-ΚΦΜ + εργασίες
Πακέτο HPC χρησιμοποιείται για τις δυνατότητές του χρονοδιαγράμματος έργου για να εκτελέσετε ΑΣΤΈΡΙ-ΚΦΜ + εργασίες. Για να το κάνετε, χρειαζόμαστε την υποστήριξη του μερικά δεσμών ενεργειών που χρησιμοποιούνται για να ξεκινήσει η εργασία και εκτελέστε ΑΣΤΈΡΙ-ΚΦΜ +. Τα δεδομένα εισόδου παραμένουν στο κοινόχρηστο αρχείο Azure πρώτη για λόγους ευκολίας.

Χρησιμοποιείται η ακόλουθη δέσμη ενεργειών PowerShell σε ουρά ένα ΑΣΤΈΡΙ-ΚΦΜ + εργασία. Χρειάζονται τρία ορίσματα:

*  Το όνομα του μοντέλου

*  Ο αριθμός των κόμβους που θα χρησιμοποιηθεί

*  Ο αριθμός πυρήνων σε κάθε κόμβο που θα χρησιμοποιηθεί

Επειδή το ΑΣΤΈΡΙ-ΚΦΜ + να συμπληρώσετε το εύρος ζώνης μνήμης, συνήθως είναι καλύτερα να χρησιμοποιήσετε λιγότερες πυρήνων ανά κόμβους υπολογιστικών και να προσθέσετε νέα κόμβους. Ο ακριβής αριθμός πυρήνων ανά κόμβο θα εξαρτώνται από την οικογένεια επεξεργαστή και της ταχύτητας τοπικό.

Οι κόμβοι εκχωρούνται αποκλειστικά για το έργο και δεν μπορεί να είναι κοινόχρηστη με άλλες εργασίες. Η εργασία δεν έχει ξεκινήσει ως μια εργασία MPI απευθείας. Η δέσμη ενεργειών κελύφους **runstarccm.sh** θα ξεκινήσει η εκκίνηση του MPI.

Το μοντέλο εισαγωγής και της δέσμης ενεργειών **runstarccm.sh** αποθηκεύονται στην κοινή χρήση **/hpcdata** που έχει ήδη ενεργοποιηθεί.

Τα αρχεία καταγραφής ονομάζονται με το Αναγνωριστικό εργασίας και είναι αποθηκευμένες σε την **κοινή χρήση /hpcdata**, μαζί με το ΑΣΤΈΡΙ-ΚΦΜ + αρχεία εξόδου.


#### <a name="sample-submitstarccmjobps1-script"></a>Δείγμα δέσμης ενεργειών SubmitStarccmJob.ps1
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Αντικαταστήστε το **runner.java** με την προτιμώμενη ΑΣΤΈΡΙ-εκκίνηση μοντέλο ΚΦΜ + Java και καταγραφή κώδικα.

#### <a name="sample-runstarccmsh-script"></a>Δείγμα δέσμης ενεργειών runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

Σε δοκιμή μας, χρησιμοποιήσαμε ενός διακριτικού Power On Demand άδεια χρήσης. Για αυτό το διακριτικό, πρέπει να ορίσετε τη μεταβλητή περιβάλλοντος **$CDLMD_LICENSE_FILE** **1999@flex.cd-adapco.com** και το κλειδί στην επιλογή **- podkey** της γραμμής εντολών.

Μετά την προετοιμασία ορισμένες, η δέσμη ενεργειών εξάγει--από τις **$CCP_NODES_CORES** μεταβλητές περιβάλλοντος που πακέτο HPC Ορισμός--τη λίστα των κόμβους για να δημιουργήσετε ένα hostfile που χρησιμοποιεί την εκκίνηση MPI. Σε αυτό το hostfile θα περιέχει τη λίστα των ονομάτων κόμβο υπολογισμού που χρησιμοποιούνται για την εργασία, ένα όνομα σε κάθε γραμμή.

Η μορφή **$CCP_NODES_CORES** ακολουθεί αυτό το μοτίβο:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Όπου:

* `<Number of nodes>`είναι ο αριθμός των κόμβους που έχει εκχωρηθεί σε αυτήν την εργασία.

* `<Name of node_n_...>`είναι το όνομα κάθε κόμβου έχει εκχωρηθεί σε αυτήν την εργασία.

* `<Cores of node_n_...>`είναι ο αριθμός πυρήνων στον κόμβο έχει εκχωρηθεί σε αυτήν την εργασία.

Ο αριθμός πυρήνων (**$NBCORES**) επίσης υπολογίζεται με βάση τον αριθμό των κόμβους (**$NBNODES**) και ο αριθμός πυρήνων ανά κόμβο (που παρέχεται ως παράμετρο **$NBCORESPERNODE**).

Για τις επιλογές MPI, αυτά που χρησιμοποιούνται με Intel MPI στην Azure είναι οι εξής:

*   `-mpi intel`Για να καθορίσετε Intel MPI.

*   `-fabric UDAPL`Για να χρησιμοποιήσετε ρήματα Azure InfiniBand.

*   `-cpubind bandwidth,v`Για να βελτιστοποιήσετε το εύρος ζώνης για MPI με ΑΣΤΈΡΙ-ΚΦΜ +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Για να κάνετε Intel MPI εργασία με Azure InfiniBand και για να ορίσετε τον απαιτούμενο αριθμό πυρήνων ανά κόμβο.

*   `-batch`Για να ξεκινήσετε ΑΣΤΈΡΙ-ΚΦΜ + σε κατάσταση λειτουργίας δέσμης με χωρίς περιβάλλον εργασίας Χρήστη.


Τέλος, για να ξεκινήσετε μια εργασία, βεβαιωθείτε ότι το κόμβοι είναι εγκατάσταση και λειτουργία και είναι σε σύνδεση στη Διαχείριση συμπλέγματος. Στη συνέχεια, από τη γραμμή εντολών του PowerShell, εκτελέστε αυτό:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Διακοπή κόμβοι
Νεότερη έκδοση σε, αφού ολοκληρώσετε την εργασία με τις δοκιμές σας, μπορείτε να χρησιμοποιήσετε τις παρακάτω εντολές HPC πακέτο PowerShell για να διακόψετε και να ξεκινήσετε κόμβους:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Επόμενα βήματα
Προσπαθήστε να εκτελέσετε άλλες φόρτους εργασίας του Linux. Για παράδειγμα, ανατρέξτε στα θέματα:

* [Εκτέλεση NAMD με το πακέτο του Microsoft HPC σε Linux υπολογισμού τους κόμβους Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Εκτέλεση OpenFOAM με το Microsoft HPC πακέτο σε ένα σύμπλεγμα Linux RDMA στο Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png

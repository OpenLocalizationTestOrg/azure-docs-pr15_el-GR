<properties
    pageTitle="Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM χρησιμοποιώντας Επαναφορά τοποθεσίας Azure και PowerShell (Διαχείριση πόρων) | Microsoft Azure"
    description="Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM χρησιμοποιώντας Επαναφορά τοποθεσίας Azure και PowerShell"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM σε Azure με χρήση του PowerShell και διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](site-recovery-vmm-to-azure.md)
- [PowerShell - διαχείριση πόρων](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Κλασική πύλη](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - κλασικό](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Επισκόπηση

Azure Επαναφορά τοποθεσίας συμβάλλει της στρατηγικής ανάκτησης (BCDR) επιχειρήσεις συνέχειας και καταστροφή από orchestrating αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των εικονικές μηχανές έναν αριθμό σεναρίων ανάπτυξης. Για μια πλήρη λίστα των σενάρια ανάπτυξης, δείτε την [Επισκόπηση Azure τοποθεσίας αποκατάστασης](site-recovery-overview.md).

Αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του PowerShell για να αυτοματοποιήσετε κοινές εργασίες που πρέπει να εκτελέσετε κατά τη ρύθμιση του Azure Επαναφορά τοποθεσίας να αναπαραγάγετε το Hyper-V εικονικές μηχανές στο σύστημα κέντρου VMM σύννεφων για Azure χώρου αποθήκευσης.

Το άρθρο περιλαμβάνει τις προϋποθέσεις για το σενάριο, και σας δείχνει

- Πώς μπορείτε να ρυθμίσετε ένα θάλαμο υπηρεσίες ανάκτησης
- Εγκαταστήστε την υπηρεσία παροχής αποκατάστασης τοποθεσίας Azure στο διακομιστή VMM προέλευσης
- Καταχώρηση στο διακομιστή στο το θάλαμο, προσθέστε ένα λογαριασμό Azure χώρου αποθήκευσης
- Εγκαταστήσει τον παράγοντα υπηρεσίες ανάκτησης Azure σε διακομιστές host Hyper-V
- Ρύθμιση παραμέτρων προστασίας για VMM σύννεφων, που θα εφαρμοστεί σε όλα τα προστατευμένα εικονικές μηχανές
- Ενεργοποίηση προστασίας για αυτές τις εικονικές μηχανές.
- Ελέγξτε την ανακατεύθυνση για να βεβαιωθείτε ότι όλα λειτουργούν όπως αναμένεται.

Εάν αντιμετωπίσετε προβλήματα με τη ρύθμιση του αυτό το σενάριο, δημοσιεύστε τις ερωτήσεις σας στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων και κλασική](../resource-manager-deployment-model.md). Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων.

## <a name="before-you-start"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε αυτές τις προϋποθέσεις στη θέση:

### <a name="azure-prerequisites"></a>Προαπαιτούμενα Azure

- Χρειάζεστε ένα λογαριασμό [Microsoft Azure](https://azure.microsoft.com/) . Εάν δεν έχετε, ξεκινήστε με ένα [δωρεάν λογαριασμό](https://azure.microsoft.com/free). Επιπλέον, μπορείτε να διαβάσετε σχετικά με το [διαχειριστή της ανάκτησης τοποθεσίας Azure τις τιμές](https://azure.microsoft.com/pricing/details/site-recovery/).
- Θα χρειαστείτε μια υπηρεσία παροχής Κρυπτογράφησης συνδρομή εάν δοκιμάζετε την αναπαραγωγή σε ένα σενάριο συνδρομή υπηρεσία παροχής Κρυπτογράφησης. Μάθετε περισσότερα σχετικά με την υπηρεσία παροχής Κρυπτογράφησης πρόγραμμα πώς [για εγγραφή στο πρόγραμμα υπηρεσία παροχής Κρυπτογράφησης](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Χρειάζεστε ένα λογαριασμό Azure v2 χώρου αποθήκευσης (Διαχείριση πόρων) για την αποθήκευση δεδομένων από αναπαραγωγή σε Azure. Ο λογαριασμός πρέπει με δυνατότητα αναπαραγωγής παν. Αυτό θα πρέπει να είναι στην ίδια περιοχή ως της υπηρεσίας ανάκτησης τοποθεσίας Azure και να σχετίζονται με την ίδια συνδρομή ή τη συνδρομή στην υπηρεσία παροχής Κρυπτογράφησης. Για να μάθετε περισσότερα σχετικά με τη ρύθμιση του Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Εισαγωγή στο Microsoft Azure αποθήκευσης](../storage/storage-introduction.md) για αναφορά.
- Θα πρέπει να βεβαιωθείτε ότι εικονικές μηχανές που επιθυμείτε να προστατεύσετε πληρούν τις [προϋποθέσεις Azure εικονική μηχανή](site-recovery-best-practices.md#azure-virtual-machine-requirements).

> [AZURE.NOTE] Προς το παρόν, μόνο Εικονική επιπέδου οι λειτουργίες είναι δυνατή μέσω του Powershell. Υποστήριξη για πρόγραμμα επιπέδου αξιοποίηση θα γίνει σύντομα.  Προς το παρόν, που περιορίζονται σε εκτέλεση ΑΠΟΤΥΧΙΑ-overs μόνο μια υποδιαίρεση 'προστατευμένη Εικονική' και όχι ένα επίπεδο Σχεδιασμός αποκατάστασης.

### <a name="vmm-prerequisites"></a>Προαπαιτούμενα στοιχεία VMM
- Θα χρειαστεί VMM διακομιστή που εκτελείται σε σύστημα κέντρο 2012 R2.
- Οποιαδήποτε VMM server που περιέχει εικονικές μηχανές που επιθυμείτε να προστατεύσετε πρέπει να εκτελεί την υπηρεσία παροχής Azure τοποθεσίας αποκατάστασης. Αυτό είναι εγκατεστημένη κατά την ανάπτυξη Azure τοποθεσίας αποκατάστασης.
- Χρειάζεστε τουλάχιστον μία cloud στο διακομιστή VMM που επιθυμείτε να προστατεύσετε. Πρέπει να περιέχει στο cloud:
    - Ένα ή περισσότερα VMM ομάδες κεντρικών υπολογιστών.
    - Ένα ή περισσότερα Hyper-V κεντρικούς διακομιστές ή συμπλεγμάτων σε κάθε ομάδα κεντρικού υπολογιστή.
    - Ένα ή περισσότερα εικονικές μηχανές στο διακομιστή προέλευσης Hyper-V.
- Μάθετε περισσότερα σχετικά με τη ρύθμιση σύννεφων VMM:
    - Διαβάστε περισσότερα σχετικά με την ιδιωτική σύννεφων VMM στο [Τι νέο υπάρχει στο Cloud ιδιωτική με σύστημα κέντρο 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) και [VMM 2012 και το σύννεφων](http://go.microsoft.com/fwlink/?LinkId=324956).
    - Μάθετε περισσότερα σχετικά με [τη ρύθμιση των παραμέτρων υφάσματος cloud VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - Αφού τα στοιχεία του cloud ύφασμα είναι στη θέση μάθετε περισσότερα σχετικά με τη δημιουργία ιδιωτικού σύννεφων στη [Δημιουργία ενός ιδιωτικού cloud στο VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) και σε [αναλυτικές οδηγίες: δημιουργία ιδιωτικού σύννεφων με σύστημα κέντρο 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Προαπαιτούμενα στοιχεία Hyper-V

- Οι διακομιστές host Hyper-V πρέπει να εκτελεί τουλάχιστον **Windows Server 2012** με το ρόλο Hyper-V ή **Microsoft Hyper-V Server 2012** και έχετε εγκαταστήσει τις πιο πρόσφατες ενημερώσεις.
- Εάν χρησιμοποιείτε το Hyper-V σε μια σημείωση σύμπλεγμα που broker σύμπλεγμα δεν δημιουργείται αυτόματα εάν έχετε ένα στατικό σύμπλεγμα με βάση τη διεύθυνση IP. Θα χρειαστεί να ρυθμίσετε τις παραμέτρους broker σύμπλεγμα με μη αυτόματο τρόπο. Για
- Για οδηγίες, δείτε [πώς μπορείτε να ρυθμίσετε τις παραμέτρους Broker ρεπλίκα Hyper-V](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Τυχόν Hyper-V κεντρικού υπολογιστή διακομιστή ή σύμπλεγμα για την οποία θέλετε να διαχειριστείτε προστασίας πρέπει να συμπεριληφθεί σε ένα σύννεφο VMM.

### <a name="network-mapping-prerequisites"></a>Προαπαιτούμενα στοιχεία αντιστοίχισης δικτύου
Κατά την προστασία εικονικές μηχανές στο Azure, οι χάρτες αντιστοίχισης δικτύου η εικονική μηχανή δίκτυα στο διακομιστή VMM προέλευσης και προορισμού Azure δίκτυα για να ενεργοποιήσετε τα εξής:

- Όλους τους υπολογιστές που ανακατευθύνει στο ίδιο δίκτυο να συνδεθείτε σε άλλο, ανεξάρτητα από το ποιο πρόγραμμα αποκατάστασης βρίσκονται στην.
- Εάν μια πύλη δικτύου είναι η εγκατάσταση στον προορισμό Azure δικτύου, εικονικές μηχανές να συνδεθείτε με άλλες εικονικές μηχανές εσωτερικής εγκατάστασης.
- Εάν δεν μπορείτε να ρυθμίσετε τις παραμέτρους αντιστοίχιση δικτύου, θα μπορούν να συνδεθούν μεταξύ τους μετά την ανακατεύθυνση για Azure μόνο τις εικονικές μηχανές που ανακατευθύνει στο ίδιο πρόγραμμα αποκατάστασης.

Εάν θέλετε να αναπτύξετε αντιστοίχιση δικτύου θα χρειαστείτε τα εξής:

- Τις εικονικές μηχανές που επιθυμείτε να προστατεύσετε στο διακομιστή προέλευσης VMM θα πρέπει να είστε συνδεδεμένοι σε δίκτυο Εικονική. Αυτό το δίκτυο θα πρέπει να είναι συνδεδεμένη με ένα λογικό δίκτυο που σχετίζεται με το cloud.
- Azure δίκτυο στο οποίο μπορούν να συνδεθούν από αναπαραγωγή εικονικές μηχανές μετά την ανακατεύθυνση. Θα μπορείτε να επιλέξετε αυτό το δίκτυο τη στιγμή της ανακατεύθυνση. Το δίκτυο πρέπει να είναι στην ίδια περιοχή ως συνδρομή σας Azure Επαναφορά τοποθεσίας.

Μάθετε περισσότερα σχετικά με την αντιστοίχιση δικτύου στο

- [Τρόπος ρύθμισης παραμέτρων λογικά δίκτυα στα VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Τρόπος ρύθμισης παραμέτρων Εικονική δίκτυα και πύλες στα VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Πώς μπορείτε να ρυθμίσετε και να παρακολουθείτε εικονικών δικτύων στο Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>Προαπαιτούμενα στοιχεία PowerShell
Βεβαιωθείτε ότι έχετε επιλέξει Azure PowerShell είστε έτοιμοι να στείλετε. Εάν χρησιμοποιείτε ήδη PowerShell, θα πρέπει να την αναβάθμιση στην έκδοση 0.8.10 ή νεότερη έκδοση. Για πληροφορίες σχετικά με τη ρύθμιση του PowerShell, ανατρέξτε στον [Οδηγό εγκατάστασης και ρύθμισης παραμέτρων Azure PowerShell](../powershell-install-configure.md). Αφού έχετε ορίσει και να ρυθμίσει τις παραμέτρους του PowerShell, μπορείτε να προβάλετε όλα τα διαθέσιμα cmdlet για την υπηρεσία [εδώ](https://msdn.microsoft.com/library/dn850420.aspx).

Για να μάθετε σχετικά με τις συμβουλές που μπορούν να σας βοηθήσουν μπορείτε να χρησιμοποιήσετε τα cmdlet, όπως τιμές παραμέτρων εισροές και εκροές συνήθως χειρισμό στο Azure PowerShell, ανατρέξτε στο θέμα [Οδηγός για γρήγορα αποτελέσματα με το cmdlet του Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Βήμα 1: Ορισμός της συνδρομής

1. Από το Azure powershell, συνδεθείτε στο λογαριασμό σας Azure: χρησιμοποιώντας τα ακόλουθα cmdlet

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Λάβετε μια λίστα με όλες τις συνδρομές σας. Επίσης, αυτό θα εμφανιστούν τα subscriptionIDs για κάθε μία από τις συνδρομές. Σημείωση προς τα κάτω το subscriptionID από τη συνδρομή στην οποία θέλετε να δημιουργήσετε το θάλαμο υπηρεσίες ανάκτησης

        Get-AzureRmSubscription

3. Ορίστε τη συνδρομή στην οποία το θάλαμο υπηρεσίες ανάκτησης είναι να δημιουργηθεί, η αναφορά το Αναγνωριστικό συνδρομής

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Βήμα 2: Δημιουργία ενός θάλαμο υπηρεσίες ανάκτησης

1. Δημιουργία ομάδας πόρων στη Διαχείριση Azure πόρων, εάν δεν έχετε ήδη

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Δημιουργήστε μια νέα θάλαμο υπηρεσίες ανάκτησης και αποθηκεύστε το αντικείμενο θάλαμο ASR που δημιουργήθηκε σε μια μεταβλητή (θα χρησιμοποιηθεί αργότερα). Μπορείτε, επίσης, να ανακτήσετε τη ASR θάλαμο αντικείμενο δημοσίευση δημιουργία χρησιμοποιώντας το cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Βήμα 3: Ρύθμιση του περιβάλλοντος θάλαμο υπηρεσίες ανάκτησης

1.  Ρύθμιση του περιβάλλοντος θάλαμο, εκτελώντας την παρακάτω εντολή.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Βήμα 4: Εγκατάσταση της υπηρεσίας παροχής αποκατάστασης Azure τοποθεσίας

1.  Στον υπολογιστή VMM, δημιουργήστε έναν κατάλογο, εκτελώντας την ακόλουθη εντολή:

        New-Item c:\ASR -type directory

2.  Εξαγάγετε τα αρχεία χρησιμοποιώντας την υπηρεσία παροχής που έχετε λάβει, εκτελέστε την παρακάτω εντολή

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Εγκατάσταση της υπηρεσίας παροχής χρησιμοποιώντας τις παρακάτω εντολές:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Περιμένετε να ολοκληρωθεί η εγκατάσταση.

4.  Καταχώρηση στο διακομιστή στο το θάλαμο χρησιμοποιώντας την ακόλουθη εντολή:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Βήμα 5: Δημιουργία λογαριασμού Azure χώρου αποθήκευσης

1. Εάν δεν έχετε ένα λογαριασμό Azure χώρου αποθήκευσης, δημιουργία λογαριασμού με δυνατότητα αναπαραγωγής παν στο το ίδιο παν ως το θάλαμο, εκτελώντας την ακόλουθη εντολή:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Σημειώστε ότι πρέπει να είναι στην ίδια περιοχή ως της υπηρεσίας ανάκτησης τοποθεσίας Azure το λογαριασμό χώρου αποθήκευσης και να συσχετιστούν με την ίδια συνδρομή.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Βήμα 6: Εγκαταστήστε τον παράγοντα υπηρεσιών Azure αποκατάστασης

1. Ο παράγοντας υπηρεσίες ανάκτησης Azure στο [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) λήψη και εγκαταστήστε την σε κάθε διακομιστή host Hyper-V που βρίσκεται σε το σύννεφων VMM που επιθυμείτε να προστατεύσετε.

2. Εκτελέστε την ακόλουθη εντολή σε όλους τους κεντρικούς υπολογιστές VMM:

    /nu/q marsagentinstaller.exe

## <a name="step-7-configure-cloud-protection-settings"></a>Βήμα 7: Ρύθμιση παραμέτρων cloud ρυθμίσεις προστασίας

1.  Δημιουργία πολιτικής αναπαραγωγής για να Azure, εκτελώντας την ακόλουθη εντολή:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Λάβετε ένα κοντέινερ προστασίας, εκτελώντας τις παρακάτω εντολές:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Λάβετε τα στοιχεία της πολιτικής σε μεταβλητή χρησιμοποιώντας την εργασία που έχει δημιουργηθεί και η αναφορά το όνομα της πολιτικής φιλικό:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Ξεκινήστε τη συσχέτιση του κοντέινερ προστασία με την πολιτική αναπαραγωγής:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Αφού ολοκληρωθεί η εργασία, εκτελέστε την ακόλουθη εντολή:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Αφού ολοκληρωθεί η εργασία επεξεργασίας, εκτελέστε την ακόλουθη εντολή:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).

## <a name="step-8-configure-network-mapping"></a>Βήμα 8: Ρύθμιση παραμέτρων αντιστοίχιση δικτύου

Πριν ξεκινήσετε αντιστοίχιση δικτύου Επαληθεύστε ότι εικονικές μηχανές στο διακομιστή προέλευσης VMM είναι συνδεδεμένοι σε δίκτυο Εικονική. Επιπλέον, μπορείτε να δημιουργήσετε μία ή περισσότερες Azure εικονικού δίκτυα.

Μάθετε περισσότερα σχετικά με τον τρόπο για να δημιουργήσετε μια εικονική δίκτυο χρήση της διαχείρισης πόρων Azure και PowerShell, σε [Δημιουργία εικονικού δικτύου με χρήση της διαχείρισης πόρων Azure και PowerShell σύνδεση VPN τοποθεσίας σε τοποθεσία](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Σημειώστε ότι πολλών δικτύων εικονική μηχανή μπορεί να αντιστοιχιστεί σε ένα μεμονωμένο Azure δίκτυο. Εάν το δίκτυο προορισμού έχει πολλά δευτερεύοντα δίκτυα και ένα από αυτά τα δευτερεύοντα δίκτυα έχει το ίδιο όνομα με υποδικτύου στον οποίο βρίσκεται η εικονική μηχανή προέλευσης, στη συνέχεια, η εικονική μηχανή ρεπλίκα θα συνδεθεί με αυτό το δευτερεύον δίκτυο προορισμού μετά την ανακατεύθυνση. Εάν δεν υπάρχει κανένα υποδίκτυο προορισμού με ένα όνομα που ταιριάζει, η εικονική μηχανή θα συνδεθεί με το πρώτο υποδίκτυο στο δίκτυο.

1. Η πρώτη εντολή λαμβάνει διακομιστές για την τρέχουσα θάλαμο Azure τοποθεσίας αποκατάστασης. Η εντολή αποθηκεύει τους διακομιστές αποκατάσταση τοποθεσία του Microsoft Azure στη μεταβλητή $Servers πίνακα.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Η δεύτερη εντολή λαμβάνει την τοποθεσία δίκτυο ανάκτησης για τον πρώτο διακομιστή στον πίνακα $Servers. Η εντολή αποθηκεύει τα δίκτυα στη μεταβλητή $Networks.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Η τρίτη εντολή λαμβάνει Azure εικονικών δικτύων και, στη συνέχεια, αυτήν την τιμή στη μεταβλητή $AzureVmNetworks.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Το τελικό cmdlet δημιουργεί μια αντιστοίχιση μεταξύ του δικτύου Azure εικονική μηχανή και το κύριο δίκτυο. Το cmdlet Καθορίζει το κύριο δίκτυο ως το πρώτο στοιχείο της $Networks. Το cmdlet καθορίζει μια εικονική μηχανή δικτύου ως το πρώτο στοιχείο της $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Βήμα 9: Ενεργοποίηση προστασίας για εικονικές μηχανές

Αφού τους διακομιστές, σύννεφων και δίκτυα έχουν ρυθμιστεί σωστά, μπορείτε να ενεργοποιήσετε προστασίας για εικονικές μηχανές στο cloud.

 Λάβετε υπόψη τα εξής:

 - Εικονικές μηχανές πρέπει να πληροί τις απαιτήσεις Azure. Ελέγξτε τα εξής στην [υποστήριξη και τις προϋποθέσεις](site-recovery-best-practices.md) του Οδηγού σχεδιασμού.

 - Για να ενεργοποιήσετε την προστασία, το λειτουργικό σύστημα και το λειτουργικό σύστημα πρέπει να οριστεί ιδιότητες του δίσκου για την εικονική μηχανή. Όταν δημιουργείτε μια εικονική μηχανή στο VMM χρησιμοποιώντας ένα πρότυπο εικονική μηχανή μπορείτε να ορίσετε την ιδιότητα. Μπορείτε επίσης να ορίσετε αυτές τις ιδιότητες για υπάρχοντα εικονικές μηχανές στις καρτέλες **Γενικά** και **Ρύθμιση παραμέτρων υλικού** οι ιδιότητες εικονική μηχανή. Εάν δεν μπορείτε να ορίσετε αυτές τις ιδιότητες στο VMM θα έχετε τη δυνατότητα να ρυθμίσετε τις παραμέτρους στην πύλη του Azure τοποθεσίας αποκατάστασης.


  1. Για να ενεργοποιήσετε την προστασία, εκτελέστε την παρακάτω εντολή για να λάβετε το κοντέινερ προστασίας:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Λάβετε την προστασία οντότητα (Εικονική), εκτελώντας την ακόλουθη εντολή:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Ενεργοποίηση του DR για το Εικονική, εκτελώντας την ακόλουθη εντολή:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Δοκιμή της ανάπτυξης

Για να ελέγξετε την ανάπτυξη, μπορείτε να εκτελέσετε δοκιμής ανακατεύθυνση για έναν εικονικό υπολογιστή, ή να δημιουργήσετε ένα σχέδιο αποκατάστασης που αποτελείται από πολλά εικονικές μηχανές και να εκτέλεση δοκιμής ανακατεύθυνση για το πρόγραμμα. Ανακατεύθυνση δοκιμής Προσομοιώνει το μηχανισμό ανακατεύθυνση και αποκατάστασης σε ένα δίκτυο απομόνωσης. Να σημειωθεί ότι:

- Εάν θέλετε να συνδεθείτε με την εικονική μηχανή στο Azure χρησιμοποιώντας απομακρυσμένη επιφάνεια εργασίας μετά την ανακατεύθυνση, ενεργοποίηση σύνδεση απομακρυσμένης επιφάνειας εργασίας στον υπολογιστή εικονικές πριν από την εκτέλεση του εφεδρικού δοκιμής.
- Μετά την ανακατεύθυνση, θα χρησιμοποιήσετε μια δημόσια διεύθυνση IP για να συνδεθείτε με την εικονική μηχανή στο Azure χρησιμοποιώντας απομακρυσμένης επιφάνειας εργασίας. Εάν θέλετε να το κάνετε αυτό, βεβαιωθείτε ότι δεν έχετε τις πολιτικές τομέα που θα εμποδίσει να μια εικονική μηχανή χρησιμοποιώντας μια δημόσια διεύθυνση.

Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).


### <a name="run-a-test-failover"></a>Εκτέλεση δοκιμής ανακατεύθυνσης

1.  Ξεκινήστε την ανακατεύθυνση δοκιμής, εκτελώντας την ακόλουθη εντολή:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Εκτελέστε μια προγραμματισμένη ανακατεύθυνσης

1. Ξεκινήστε την προγραμματισμένη ανακατεύθυνσης, εκτελώντας την ακόλουθη εντολή:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης

1. Έναρξη του προγραμματισμένου εφεδρικού, εκτελώντας την ακόλουθη εντολή:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


## <a name=monitor></a>Εποπτεία δραστηριότητας

Χρησιμοποιήστε τις παρακάτω εντολές για την παρακολούθηση της δραστηριότητας. Σημειώστε ότι θα πρέπει να περιμένετε ανάμεσα στα εργασίες για την επεξεργασία για να ολοκληρώσετε.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Επόμενα βήματα

[Διαβάστε περισσότερα](https://msdn.microsoft.com/library/azure/mt637930.aspx) σχετικά με την Επαναφορά τοποθεσίας Azure με το cmdlet του PowerShell για τη διαχείριση πόρων Azure.

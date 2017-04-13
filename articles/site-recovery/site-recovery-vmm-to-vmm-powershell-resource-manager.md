<properties
    pageTitle="Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM σε μια δευτερεύουσα τοποθεσία VMM χρήση του PowerShell (Διαχείριση πόρων) | Microsoft Azure"
    description="Περιγράφει τον τρόπο ανάπτυξης Azure Επαναφορά τοποθεσίας για να οργανώσετε αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των ΣΠΣ Hyper-V στο σύννεφων VMM σε μια δευτερεύουσα τοποθεσία VMM χρήση του PowerShell (Διαχείριση πόρων)"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM σε μια δευτερεύουσα τοποθεσία VMM χρήση του PowerShell (Διαχείριση πόρων)

> [AZURE.SELECTOR]
- [Πύλη του Azure](site-recovery-vmm-to-vmm.md)
- [Κλασική πύλη](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - διαχείριση πόρων](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Καλώς ορίσατε στο Επαναφορά τοποθεσίας Azure! Χρησιμοποιήστε αυτό το άρθρο εάν θέλετε να αναπαραγάγετε εσωτερικής Hyper-V εικονικές μηχανές διαχείρισης στο σύννεφων Manager εικονική μηχανή κέντρο σύστημα (VMM) σε μια δευτερεύουσα τοποθεσία. 

Σε αυτό το άρθρο σάς δείχνει τον τρόπο χρήσης του PowerShell για την αυτοματοποίηση κοινών εργασιών που πρέπει να εκτελέσετε κατά τη ρύθμιση του Azure Επαναφορά τοποθεσίας να αναπαραγάγετε το Hyper-V εικονικές μηχανές στο σύστημα κέντρου VMM σύννεφων σε σύστημα κέντρο VMM σύννεφων σε δευτερεύουσα τοποθεσία.

Το άρθρο περιλαμβάνει τις προϋποθέσεις για το σενάριο, και σας δείχνει 

- Πώς μπορείτε να ρυθμίσετε ένα θάλαμο υπηρεσίες ανάκτησης
- Εγκατάσταση του Azure την υπηρεσία παροχής αποκατάστασης τοποθεσίας σε διακομιστή VMM προέλευσης και προορισμού VMM διακομιστή
- Οι διακομιστές VMM σε αυτήν την θάλαμο REGISTER
- Ρύθμιση παραμέτρων πολιτική αναπαραγωγής για το Cloud VMM. Οι ρυθμίσεις αναπαραγωγής στην πολιτική θα εφαρμοστεί σε όλα τα προστατευμένα εικονικές μηχανές 
- Ενεργοποίηση προστασίας για τις εικονικές μηχανές. 
- Δοκιμή του εφεδρικού του ΣΠΣ μεμονωμένα ή ως τμήμα του σχεδίου αποκατάστασης για να βεβαιωθείτε ότι όλα λειτουργούν όπως αναμένεται.
- Εκτελέστε μια προγραμματισμένη ή μια μη προγραμματισμένη ανακατεύθυνσης του ΣΠΣ μεμονωμένα ή ως τμήμα του σχεδίου αποκατάστασης για να βεβαιωθείτε ότι όλα λειτουργούν όπως αναμένεται.

Εάν αντιμετωπίσετε προβλήματα με τη ρύθμιση του αυτό το σενάριο, δημοσιεύστε τις ερωτήσεις σας στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure περιλαμβάνει δύο διαφορετικά [μοντέλα ανάπτυξης](../resource-manager-deployment-model.md) για τη δημιουργία και εργασία με πόρους: Διαχείριση πόρων Azure και κλασική. Azure έχει επίσης δύο πύλες – Azure κλασική πύλης που υποστηρίζει το μοντέλο κλασική ανάπτυξης και το Azure πύλης με την υποστήριξη για δύο μοντέλα ανάπτυξης. Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων.



## <a name="on-premises-prerequisites"></a>Προαπαιτούμενα στοιχεία εσωτερικής εγκατάστασης

Ακολουθεί τι θα χρειαστείτε στις τοποθεσίες κύριας και δευτερεύουσας εσωτερικής εγκατάστασης για να αναπτύξετε αυτό το σενάριο:

**Προαπαιτούμενα στοιχεία** | **Λεπτομέρειες** 
--- | ---
**VMM** | Σας συνιστούμε να αναπτύξετε ένα διακομιστή VMM στην κύρια τοποθεσία και ένα διακομιστή VMM σε δευτερεύουσα τοποθεσία.<br/><br/> Μπορείτε επίσης να [αναπαραγάγετε μεταξύ σύννεφων σε ένα διακομιστή VMM](site-recovery-single-vmm.md). Για να το κάνετε αυτό θα χρειαστείτε τουλάχιστον δύο σύννεφων ρυθμιστεί στο διακομιστή VMM.<br/><br/> Οι διακομιστές VMM πρέπει να εκτελεί τουλάχιστον συστήματος κέντρο 2012 SP1 με τις πιο πρόσφατες ενημερώσεις.<br/><br/> Κάθε διακομιστή VMM πρέπει να έχετε σε μία ή περισσότερες σύννεφων έχει ρυθμιστεί και όλα σύννεφων πρέπει να έχουν το προφίλ χωρητικότητα Hyper-V ρύθμιση. <br/><br/>Σύννεφων πρέπει να περιέχει μία ή περισσότερες ομάδες VMM κεντρικού υπολογιστή.<br/><br/>Μάθετε περισσότερα σχετικά με τη ρύθμιση σύννεφων VMM στη [ρύθμιση των παραμέτρων υφάσματος cloud VMM](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), και [αναλυτικές οδηγίες: δημιουργία ιδιωτικού σύννεφων με σύστημα κέντρο 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> Οι διακομιστές VMM θα πρέπει να έχετε πρόσβαση στο internet. 
**Hyper-V** | Διακομιστές Hyper-V πρέπει να εκτελεί τουλάχιστον Windows Server 2012, με το ρόλο Hyper-V και έχετε εγκαταστήσει τις πιο πρόσφατες ενημερώσεις.<br/><br/> Διακομιστής Hyper-V πρέπει να περιέχει μία ή περισσότερες ΣΠΣ.<br/><br/>  Το Hyper-V κεντρικούς διακομιστές πρέπει να βρίσκεται σε ομάδες κεντρικών υπολογιστών σε το σύννεφων VMM κύριας και δευτερεύουσας.<br/><br/> Εάν χρησιμοποιείτε το Hyper-V σε ένα σύμπλεγμα σε Windows Server 2012 R2 θα πρέπει να εγκαταστήσετε την [Ενημέρωση 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Εάν χρησιμοποιείτε το Hyper-V σε ένα σύμπλεγμα σε Windows Server 2012 σημείωση που broker σύμπλεγμα δεν δημιουργούνται αυτόματα εάν έχετε ένα στατικό σύμπλεγμα με βάση τη διεύθυνση IP. Θα χρειαστεί να ρυθμίσετε τις παραμέτρους broker σύμπλεγμα με μη αυτόματο τρόπο. [Διαβάστε περισσότερα](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Υπηρεσία παροχής** | Κατά την ανάπτυξη Επαναφορά τοποθεσίας, μπορείτε να εγκαταστήσετε την υπηρεσία παροχής Azure τοποθεσίας αποκατάστασης σε διακομιστές VMM. Η υπηρεσία παροχής επικοινωνεί με Επαναφορά τοποθεσίας μέσω HTTPS 443 για να οργανώσετε αναπαραγωγής. Αναπαραγωγή δεδομένων πραγματοποιείται μεταξύ των διακομιστών Hyper-V κύριας και δευτερεύουσας πάνω από το τοπικό ΔΊΚΤΥΟ ή μια σύνδεση VPN.<br/><br/> Η υπηρεσία παροχής που εκτελείται στο διακομιστή VMM χρειάζονται πρόσβαση σε αυτές τις διευθύνσεις URL: *. hypervrecoverymanager.windowsazure.com- *. accesscontrol.Windows.NET; *. backup.windowsazure.com- *. blob.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Επιπλέον επιτρέπεται η επικοινωνία τείχος προστασίας από τους διακομιστές VMM για τις [περιοχές διευθύνσεων IP του κέντρου δεδομένων του Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) και να επιτρέπεται το πρωτόκολλο HTTPS (443).

### <a name="network-mapping-prerequisites"></a>Προαπαιτούμενα στοιχεία αντιστοίχισης δικτύου
Χάρτες αντιστοίχισης δικτύου μεταξύ δικτύων Εικονική VMM στους διακομιστές VMM κύριας και δευτερεύουσας για να:

- Βέλτιστη τοποθετήστε ΣΠΣ ρεπλίκα στη δευτερεύουσα hosts Hyper-V μετά την ανακατεύθυνση.
- Σύνδεση ΣΠΣ ρεπλίκα σε κατάλληλη Εικονική δίκτυα.
- Εάν δεν μπορείτε να ρυθμίσετε τις παραμέτρους δικτύου αντιστοίχιση ρεπλίκα ΣΠΣ δεν θα είναι συνδεδεμένη με οποιοδήποτε δίκτυο μετά την ανακατεύθυνση.
- Εάν θέλετε να εγκαταστήσετε το δίκτυο αντιστοίχιση κατά την ανάκτηση τοποθεσία ανάπτυξης εδώ είναι τι θα χρειαστείτε:

    - Βεβαιωθείτε ότι ΣΠΣ στο διακομιστή προέλευσης Hyper-V κεντρικός υπολογιστής είναι συνδεδεμένοι σε δίκτυο VMM Εικονική. Αυτό το δίκτυο θα πρέπει να είναι συνδεδεμένη με ένα λογικό δίκτυο που σχετίζεται με το cloud.
    - Βεβαιωθείτε ότι το δευτερεύον cloud που θα χρησιμοποιήσετε για την ανάκτηση έχει αντίστοιχο Εικονική δίκτυο έχει ρυθμιστεί. Αυτό το δίκτυο Εικονική θα πρέπει να είναι συνδεδεμένη με ένα λογικό δίκτυο που είναι συσχετισμένη με το δευτερεύον cloud.


Μάθετε περισσότερα σχετικά με τη ρύθμιση των παραμέτρων δικτύων VMM τα παρακάτω άρθρα

- [Τρόπος ρύθμισης παραμέτρων λογικά δίκτυα στα VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Τρόπος ρύθμισης παραμέτρων Εικονική δίκτυα και πύλες στα VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Μάθετε περισσότερα](site-recovery-network-mapping.md) σχετικά με το πώς λειτουργεί η αντιστοίχιση δικτύου.

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

1. Δημιουργία ομάδας του πόρου από διαχειριστή πόρων Azure, εάν δεν έχετε ήδη

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Δημιουργήστε μια νέα θάλαμο υπηρεσίες ανάκτησης και αποθηκεύστε το αντικείμενο θάλαμο ASR που δημιουργήθηκε σε μια μεταβλητή (θα χρησιμοποιηθεί αργότερα). Μπορείτε, επίσης, να ανακτήσετε τη ASR θάλαμο αντικείμενο δημοσίευση δημιουργία χρησιμοποιώντας το cmdlet Get-AzureRMRecoveryServicesVault:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Βήμα 3: Ρύθμιση του περιβάλλοντος θάλαμο υπηρεσίες ανάκτησης

1.  Εάν έχετε ένα θάλαμο που έχετε ήδη δημιουργήσει, εκτελέστε την παρακάτω εντολή για να λάβετε το θάλαμο.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Ρύθμιση του περιβάλλοντος θάλαμο, εκτελώντας την παρακάτω εντολή.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Βήμα 5: Δημιουργία και να συσχετίσετε μια πολιτική αναπαραγωγής

1.  Δημιουργία πολιτικής αναπαραγωγής Hyper-V 2012 R2, εκτελώντας την ακόλουθη εντολή:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] Στο cloud VMM μπορεί να περιέχει το Hyper-V κεντρικούς υπολογιστές που εκτελούν διαφορετικές εκδόσεις του Windows Server (όπως που αναφέρεται στο τις προϋποθέσεις Hyper-V), αλλά η πολιτική αναπαραγωγής είναι συγκεκριμένη έκδοση λειτουργικού Συστήματος. Εάν έχετε διάφορες υπηρεσίες παροχής φιλοξενίας εκτελείται σε εκδόσεις διαφορετικό λειτουργικό σύστημα, στη συνέχεια, δημιουργία πολιτικών ξεχωριστή αναπαραγωγής για κάθε τύπο έκδοση λειτουργικού Συστήματος. Για π.χ: Εάν έχετε πέντε hosts εκτελείται σε Windows διακομιστές 2012 και τριών σε Windows Server 2012 R2, η δημιουργία δύο αναπαραγωγής πολιτικές – ένα για κάθε τύπο εκδόσεις λειτουργικού συστήματος.

2.  Λήψη του πρωτεύοντος προστασίας κοντέινερ (πρωτεύον VMM Cloud) και αποκατάστασης προστασίας κοντέινερ (αποκατάστασης VMM Cloud), εκτελώντας τις παρακάτω εντολές:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Ανακτήστε την πολιτική που δημιουργήσατε στο βήμα 1, χρησιμοποιώντας το φιλικό όνομα της πολιτικής

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Ξεκινήστε τη συσχέτιση του κοντέινερ προστασίας (VMM Cloud) με την πολιτική αναπαραγωγής:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Περιμένετε για την πολιτική συσχετισμού να ολοκληρωθεί η εργασία. Μπορείτε να ελέγξετε εάν η εργασία έχει ολοκληρωθεί χρησιμοποιώντας το παρακάτω τμήμα κώδικα του PowerShell.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Αφού ολοκληρωθεί η εργασία επεξεργασίας, εκτελέστε την ακόλουθη εντολή:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).

## <a name="step-5-configure-network-mapping"></a>Βήμα 5: Ρυθμίστε τις παραμέτρους δικτύου αντιστοίχισης

1. Η πρώτη εντολή λαμβάνει διακομιστές για την τρέχουσα θάλαμο Azure τοποθεσίας αποκατάστασης. Η εντολή αποθηκεύει τους διακομιστές αποκατάσταση τοποθεσία του Microsoft Azure στη μεταβλητή $Servers πίνακα.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Το παρακάτω εντολές λήψη στο δίκτυο αποκατάστασης τοποθεσίας για το διακομιστή VMM προέλευσης και προορισμού VMM διακομιστή.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Ο διακομιστής VMM προέλευσης μπορεί να είναι το πρώτο ή για το δεύτερο στον πίνακα των διακομιστών. Επιλέξτε τα ονόματα των διακομιστών VMM και λάβετε τα δίκτυα σωστά


4. Το τελικό cmdlet δημιουργεί μια αντιστοίχιση μεταξύ το κύριο δίκτυο και το δίκτυο αποκατάστασης. Το cmdlet Καθορίζει το κύριο δίκτυο ως το πρώτο στοιχείο της $PrimaryNetworks και του δικτύου αποκατάστασης ως το πρώτο στοιχείο της $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Βήμα 6: Ρύθμιση παραμέτρων αντιστοίχισης χώρου αποθήκευσης

1. Η παρακάτω εντολή λαμβάνει τη λίστα των ταξινομήσεις χώρο αποθήκευσης στη μεταβλητή $storageclassifications.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Το παρακάτω εντολές γρήγορα την ταξινόμηση προέλευσης σε $SourceClassificaion μεταβλητή και προορισμού ταξινόμηση σε μεταβλητή $TargetClassification. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Το ταξινομήσεις προέλευσης και προορισμού μπορεί να είναι οποιοδήποτε στοιχείο του πίνακα. Αναφέρονται στο αποτέλεσμα του την παρακάτω εντολή για την εικόνα του ευρετηρίου της προέλευσης και προορισμού ταξινομήσεις στον πίνακα $storageclassifications. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Επιλογή αντικειμένου - ιδιότητα FriendlyName, αναγνωριστικό | Μορφοποίηση πίνακα


3. Το παρακάτω cmdlet δημιουργεί μια αντιστοίχιση μεταξύ την κατάταξη προέλευσης και την ταξινόμηση προορισμού. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Βήμα 7: Ενεργοποίηση προστασίας για εικονικές μηχανές

Αφού τους διακομιστές, σύννεφων και δίκτυα έχουν ρυθμιστεί σωστά, μπορείτε να ενεργοποιήσετε προστασίας για εικονικές μηχανές στο cloud. 


  1. Για να ενεργοποιήσετε την προστασία, εκτελέστε την παρακάτω εντολή για να λάβετε το κοντέινερ προστασίας:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Λάβετε την προστασία οντότητα (Εικονική), εκτελώντας την ακόλουθη εντολή:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Ενεργοποίηση αναπαραγωγής για την εικονική Μηχανή, εκτελώντας την ακόλουθη εντολή:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Δοκιμή της ανάπτυξης

Για να ελέγξετε την ανάπτυξη, μπορείτε να εκτελέσετε ανακατεύθυνσης δοκιμή για μια εικονική μηχανή, ή να δημιουργήσετε ένα σχέδιο αποκατάστασης που αποτελείται από πολλά εικονικές μηχανές και να εκτέλεση δοκιμής ανακατεύθυνσης για το πρόγραμμα. Δοκιμή ανακατεύθυνσης Προσομοιώνει το μηχανισμό ανακατεύθυνσης και αποκατάστασης σε ένα δίκτυο απομόνωσης. 

> [AZURE.NOTE] Μπορείτε να δημιουργήσετε ένα σχέδιο ανάκτησης για την εφαρμογή σας στην πύλη Azure.

Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).


### <a name="run-a-test-failover"></a>Εκτέλεση δοκιμής ανακατεύθυνσης

1.  Εκτελέστε την παρακάτω cmdlet του για να λάβετε το Εικονική δίκτυο στο οποίο θέλετε να ελέγξετε ανακατεύθυνσης σας ΣΠΣ να.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Εκτέλεση δοκιμής ανακατεύθυνσης από μια Εικονική, κάνοντας τα εξής:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Εκτέλεση δοκιμής ανακατεύθυνσης προγράμματος ανάκτησης, κάνοντας τα εξής:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Εκτελέστε μια προγραμματισμένη ανακατεύθυνσης

1. Εκτελέστε μια προγραμματισμένη ανακατεύθυνση από μια Εικονική, κάνοντας τα εξής:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Εκτελέστε μια προγραμματισμένη ανακατεύθυνση προγράμματος ανάκτησης, κάνοντας τα εξής:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης

1. Εκτελέστε μια μη προγραμματισμένη ανακατεύθυνση από μια Εικονική, κάνοντας τα εξής:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2 εκτελέστε μια μη προγραμματισμένη ανακατεύθυνση προγράμματος ανάκτησης, κάνοντας τα εξής:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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


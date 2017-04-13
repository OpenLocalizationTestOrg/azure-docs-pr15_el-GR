<properties
    pageTitle="Αναπαραγωγή Hyper-V εικονικές μηχανές στο σύννεφων VMM χρησιμοποιώντας Επαναφορά τοποθεσίας Azure και PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να αυτοματοποιήσετε την αναπαραγωγή του Hyper-V εικονικές μηχανές στο σύννεφων VMM χρησιμοποιώντας Επαναφορά τοποθεσίας και του PowerShell."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Αναπαραγάγετε Hyper-V εικονικές μηχανές στο σύννεφων VMM για χρήση του Powershell - κλασική Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](site-recovery-vmm-to-azure.md)
- [PowerShell - διαχείριση πόρων](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Κλασική πύλη](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - κλασικό](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Επισκόπηση

Azure Επαναφορά τοποθεσίας συμβάλλει της στρατηγικής ανάκτησης (BCDR) επιχειρήσεις συνέχειας και καταστροφή από orchestrating αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των εικονικές μηχανές έναν αριθμό σεναρίων ανάπτυξης. Για μια πλήρη λίστα των σενάρια ανάπτυξης, δείτε την [Επισκόπηση Azure τοποθεσίας αποκατάστασης](site-recovery-overview.md).

Αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του PowerShell για να αυτοματοποιήσετε κοινές εργασίες που πρέπει να εκτελέσετε κατά τη ρύθμιση του Azure Επαναφορά τοποθεσίας να αναπαραγάγετε το Hyper-V εικονικές μηχανές στο σύστημα κέντρου VMM σύννεφων για Azure χώρου αποθήκευσης.

Το άρθρο περιλαμβάνει τις προϋποθέσεις για το σενάριο, και θα μάθετε πώς να ρυθμίσετε ένα θάλαμο Επαναφορά τοποθεσίας, εγκαταστήστε την υπηρεσία παροχής αποκατάστασης τοποθεσίας Azure στο διακομιστή προέλευσης VMM, καταχώρηση διακομιστή στο το θάλαμο, Προσθήκη λογαριασμού Azure χώρου αποθήκευσης, εγκαταστήσει τον παράγοντα υπηρεσίες ανάκτησης Azure σε διακομιστές host Hyper-V, ρύθμιση παραμέτρων προστασίας για σύννεφων VMM που θα εφαρμοστεί σε όλα τα προστατευμένα εικονικές μηχανές , και, στη συνέχεια, ενεργοποίηση προστασίας για αυτές τις εικονικές μηχανές. Λήξη προς τα επάνω, δοκιμάζοντας την ανακατεύθυνση για να βεβαιωθείτε ότι όλα λειτουργούν όπως αναμένεται.

Εάν αντιμετωπίσετε προβλήματα με τη ρύθμιση του αυτό το σενάριο, δημοσιεύστε τις ερωτήσεις σας στο [Φόρουμ υπηρεσίες ανάκτησης Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων και κλασική](../resource-manager-deployment-model.md). Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο ανάπτυξης κλασική.



## <a name="before-you-start"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε αυτές τις προϋποθέσεις στη θέση:

### <a name="azure-prerequisites"></a>Προαπαιτούμενα Azure

- Χρειάζεστε ένα λογαριασμό [Microsoft Azure](https://azure.microsoft.com/) . Μπορείτε να ξεκινήσετε με μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).
- Χρειάζεστε ένα λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση δεδομένων από αναπαραγωγή. Ο λογαριασμός πρέπει με δυνατότητα αναπαραγωγής παν. Το θα πρέπει να είναι στην ίδια περιοχή ως το θάλαμο Azure Επαναφορά τοποθεσίας και να συσχετιστούν με την ίδια συνδρομή. [Μάθετε περισσότερα σχετικά με το Azure χώρου αποθήκευσης](../storage/storage-introduction.md).
- Θα πρέπει να βεβαιωθείτε ότι εικονικές μηχανές που επιθυμείτε να προστατεύσετε συμμορφώνονται με [τις προϋποθέσεις Azure εικονική μηχανή](site-recovery-best-practices.md#virtual-machines).

### <a name="vmm-prerequisites"></a>Προαπαιτούμενα στοιχεία VMM
- Θα χρειαστεί VMM διακομιστή που εκτελείται σε σύστημα κέντρο 2012 R2.
- Χρειάζεστε τουλάχιστον μία cloud στο διακομιστή VMM που επιθυμείτε να προστατεύσετε. Πρέπει να περιέχει στο cloud:
    - Ένα ή περισσότερα VMM ομάδες κεντρικών υπολογιστών.
    - Ένα ή περισσότερα Hyper-V κεντρικούς διακομιστές ή συμπλεγμάτων σε κάθε ομάδα κεντρικού υπολογιστή.
    - Ένα ή περισσότερα εικονικές μηχανές στο διακομιστή προέλευσης Hyper-V.

### <a name="hyper-v-prerequisites"></a>Προαπαιτούμενα στοιχεία Hyper-V

- Οι διακομιστές host Hyper-V πρέπει να εκτελεί τουλάχιστον **Windows Server 2012** με το ρόλο Hyper-V ή **Microsoft Hyper-V Server 2012** και έχετε εγκαταστήσει τις πιο πρόσφατες ενημερώσεις.
- Εάν χρησιμοποιείτε το Hyper-V σε μια σημείωση σύμπλεγμα που broker σύμπλεγμα δεν δημιουργείται αυτόματα εάν έχετε ένα στατικό σύμπλεγμα με βάση τη διεύθυνση IP. Θα χρειαστεί να ρυθμίσετε τις παραμέτρους broker σύμπλεγμα με μη αυτόματο τρόπο. Για να το κάνετε αυτό, στη Διαχείριση διακομιστών > Διαχείριση ανακατεύθυνσης συμπλεγμάτων, συνδεθείτε με το σύμπλεγμα, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων ρόλο** και επιλέξτε **Broker ρεπλίκα Hyper-V** στην οθόνη **Επιλέξτε το ρόλο** του οδηγού υψηλής διαθεσιμότητας.
- Τυχόν Hyper-V κεντρικού υπολογιστή διακομιστή ή σύμπλεγμα για την οποία θέλετε να διαχειριστείτε προστασίας πρέπει να συμπεριληφθεί σε ένα σύννεφο VMM.

### <a name="network-mapping-prerequisites"></a>Προαπαιτούμενα στοιχεία αντιστοίχισης δικτύου
Κατά την προστασία εικονικές μηχανές σε χάρτες αντιστοίχισης Azure δικτύου μεταξύ δικτύων Εικονική στο διακομιστή VMM προέλευσης και προορισμού Azure δίκτυα για να ενεργοποιήσετε τα εξής:

- Όλους τους υπολογιστές που ανακατευθύνει στο ίδιο δίκτυο να συνδεθείτε σε άλλο, ανεξάρτητα από το ποιο πρόγραμμα αποκατάστασης βρίσκονται στην.
- Εάν μια πύλη δικτύου είναι η εγκατάσταση στον προορισμό Azure δικτύου, εικονικές μηχανές να συνδεθείτε με άλλες εικονικές μηχανές εσωτερικής εγκατάστασης.
- Εάν δεν μπορείτε να ρυθμίσετε τις παραμέτρους δικτύου αντιστοίχισης μόνο εικονικές μηχανές οι οποίες ανακατευθύνει στο ίδιο πρόγραμμα αποκατάστασης θα μπορούν να συνδεθούν μεταξύ τους μετά την ανακατεύθυνση Azure.

Εάν θέλετε να αναπτύξετε αντιστοίχιση δικτύου θα χρειαστείτε τα εξής:

- Τις εικονικές μηχανές που επιθυμείτε να προστατεύσετε στο διακομιστή προέλευσης VMM θα πρέπει να είστε συνδεδεμένοι σε δίκτυο Εικονική. Αυτό το δίκτυο θα πρέπει να είναι συνδεδεμένη με ένα λογικό δίκτυο που σχετίζεται με το cloud.
- Azure δίκτυο στο οποίο μπορούν να συνδεθούν από αναπαραγωγή εικονικές μηχανές μετά την ανακατεύθυνση. Θα μπορείτε να επιλέξετε αυτό το δίκτυο τη στιγμή της ανακατεύθυνσης. Το δίκτυο πρέπει να είναι στην ίδια περιοχή ως συνδρομή σας Azure Επαναφορά τοποθεσίας.
- [Μάθετε περισσότερα](site-recovery-network-mapping.md) σχετικά με την αντιστοίχιση δικτύου:

###<a name="powershell-prerequisites"></a>Προαπαιτούμενα στοιχεία PowerShell
Βεβαιωθείτε ότι έχετε επιλέξει Azure PowerShell είστε έτοιμοι να στείλετε. Εάν χρησιμοποιείτε ήδη PowerShell, θα πρέπει να την αναβάθμιση στην έκδοση 0.8.10 ή νεότερη έκδοση. Για πληροφορίες σχετικά με τη ρύθμιση του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md). Αφού έχετε ορίσει και να ρυθμίσει τις παραμέτρους του PowerShell, μπορείτε να προβάλετε όλα τα διαθέσιμα cmdlet για την υπηρεσία [εδώ](https://msdn.microsoft.com/library/dn850420.aspx).

Για να μάθετε σχετικά με τις συμβουλές που μπορούν να σας βοηθήσουν μπορείτε να χρησιμοποιήσετε τα cmdlet, όπως τιμές παραμέτρων εισροές και εκροές συνήθως χειρισμό στο Azure PowerShell, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το cmdlet του Azure](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Βήμα 1: Ορισμός της συνδρομής

Στο PowerShell, εκτελέστε τα παρακάτω cmdlet:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Αντικαταστήστε τα στοιχεία μέσα σε "< >" με τις συγκεκριμένες πληροφορίες.

## <a name="step-2-create-a-site-recovery-vault"></a>Βήμα 2: Δημιουργία ενός θάλαμο Επαναφορά τοποθεσίας

Στο PowerShell, αντικαταστήστε τα στοιχεία μέσα σε "< >" με τις συγκεκριμένες πληροφορίες και εκτέλεση αυτών των εντολών:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Βήμα 3: Δημιουργία ένα κλειδί θάλαμο εγγραφής

Δημιουργία ένα κλειδί εγγραφής σε το θάλαμο. Αφού κάνετε λήψη Azure την υπηρεσία παροχής αποκατάστασης τοποθεσίας και να το εγκαταστήσετε στο διακομιστή VMM, θα χρησιμοποιήσετε αυτό το κλειδί για να καταχωρήσετε το διακομιστή VMM στο το θάλαμο.

1.  Λήψη του αρχείου ρύθμιση θάλαμο και να ορίσετε το περιβάλλον:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Ρύθμιση του περιβάλλοντος θάλαμο, εκτελώντας τις παρακάτω εντολές:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Βήμα 4: Εγκατάσταση της υπηρεσίας παροχής αποκατάστασης Azure τοποθεσίας

1.  Στον υπολογιστή VMM, δημιουργήστε έναν κατάλογο, εκτελώντας την ακόλουθη εντολή:

    ```

    pushd C:\ASR\

    ```

2.  Εξαγάγετε τα αρχεία χρησιμοποιώντας την υπηρεσία παροχής που έχετε λάβει, εκτελέστε την παρακάτω εντολή

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Εγκατάσταση της υπηρεσίας παροχής χρησιμοποιώντας τις παρακάτω εντολές:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Περιμένετε να ολοκληρωθεί η εγκατάσταση.

4.  Καταχώρηση στο διακομιστή στο το θάλαμο χρησιμοποιώντας την ακόλουθη εντολή:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Βήμα 5: Δημιουργία λογαριασμού Azure χώρου αποθήκευσης

Εάν δεν έχετε ένα λογαριασμό Azure χώρου αποθήκευσης, δημιουργία λογαριασμού με δυνατότητα αναπαραγωγής παν, εκτελώντας την ακόλουθη εντολή:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Σημειώστε ότι πρέπει να είναι στην ίδια περιοχή ως της υπηρεσίας ανάκτησης τοποθεσίας Azure το λογαριασμό χώρου αποθήκευσης και να συσχετιστούν με την ίδια συνδρομή.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Βήμα 6: Εγκαταστήστε τον παράγοντα υπηρεσιών Azure αποκατάστασης

Από την πύλη Azure, εγκαταστήστε τον παράγοντα υπηρεσίες ανάκτησης Azure σε κάθε διακομιστή host Hyper-V που βρίσκονται σε το σύννεφων VMM που επιθυμείτε να προστατεύσετε.

Εκτελέστε την ακόλουθη εντολή σε όλους τους κεντρικούς υπολογιστές VMM:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Βήμα 7: Ρύθμιση παραμέτρων cloud ρυθμίσεις προστασίας

1.  Δημιουργήστε ένα προφίλ προστασίας cloud για να Azure, εκτελώντας την ακόλουθη εντολή:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Λάβετε ένα κοντέινερ προστασίας, εκτελώντας τις παρακάτω εντολές:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Ξεκινήστε τη συσχέτιση του κοντέινερ προστασία με το cloud:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Αφού ολοκληρωθεί η εργασία, εκτελέστε την ακόλουθη εντολή:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Αφού ολοκληρωθεί η εργασία επεξεργασίας, εκτελέστε την ακόλουθη εντολή:


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



Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).

## <a name="step-8-configure-network-mapping"></a>Βήμα 8: Ρύθμιση παραμέτρων αντιστοίχιση δικτύου
Πριν ξεκινήσετε αντιστοίχιση δικτύου Επαληθεύστε ότι εικονικές μηχανές στο διακομιστή προέλευσης VMM είναι συνδεδεμένοι σε δίκτυο Εικονική. Επιπλέον, μπορείτε να δημιουργήσετε μία ή περισσότερες Azure εικονικού δίκτυα. Σημειώστε ότι πολλών δικτύων Εικονική μπορεί να αντιστοιχιστεί σε ένα μεμονωμένο Azure δίκτυο.

Λάβετε υπόψη ότι, εάν το δίκτυο προορισμού έχει πολλά δευτερεύοντα δίκτυα και ένα από αυτά τα δευτερεύοντα δίκτυα έχει το ίδιο όνομα με υποδικτύου στην οποία βρίσκεται η εικονική μηχανή προέλευσης, στη συνέχεια, η εικονική μηχανή ρεπλίκα θα συνδεθεί με αυτό το δευτερεύον δίκτυο προορισμού μετά την ανακατεύθυνση. Εάν δεν υπάρχει ένα υποδίκτυο προορισμού με ένα όνομα που ταιριάζει, η εικονική μηχανή θα συνδεθεί με το πρώτο υποδίκτυο στο δίκτυο.

Η πρώτη εντολή λαμβάνει διακομιστές για την τρέχουσα θάλαμο Azure τοποθεσίας αποκατάστασης. Η εντολή αποθηκεύει τους διακομιστές αποκατάσταση τοποθεσία του Microsoft Azure στη μεταβλητή $Servers πίνακα.

    $Servers = Get-AzureSiteRecoveryServer


Η δεύτερη εντολή λαμβάνει την τοποθεσία δίκτυο ανάκτησης για τον πρώτο διακομιστή στον πίνακα $Servers. Η εντολή αποθηκεύει τα δίκτυα στη μεταβλητή $Networks.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Τρίτη εντολή λαμβάνει όλες τις συνδρομές σας Azure χρησιμοποιώντας το cmdlet Get-AzureSubscription και, στη συνέχεια, αυτή η τιμή αποθηκεύεται στη μεταβλητή $Subscriptions.

    $Subscriptions = Get-AzureSubscription



Η τέταρτη εντολή λαμβάνει Azure εικονικών δικτύων, χρησιμοποιώντας το cmdlet Get-AzureVNetSite και, στη συνέχεια, αυτήν την τιμή στη μεταβλητή $AzureVmNetworks.


    $AzureVmNetworks = Get-AzureVNetSite



Το τελικό cmdlet δημιουργεί μια αντιστοίχιση μεταξύ του δικτύου Azure εικονική μηχανή και το κύριο δίκτυο. Το cmdlet Καθορίζει το κύριο δίκτυο ως το πρώτο στοιχείο της $Networks. Το cmdlet καθορίζει μια εικονική μηχανή δικτύου ως το πρώτο στοιχείο της $AzureVmNetworks χρησιμοποιώντας το αναγνωριστικό. Η εντολή περιλαμβάνει το αναγνωριστικό Azure συνδρομής.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Βήμα 9: Ενεργοποίηση προστασίας για εικονικές μηχανές

Αφού διακομιστές, σύννεφων και δίκτυα έχουν ρυθμιστεί σωστά, μπορείτε να ενεργοποιήσετε προστασίας για εικονικές μηχανές στο cloud. Λάβετε υπόψη τα εξής:

Εικονικές μηχανές πρέπει να πληρούν [τις προϋποθέσεις Azure εικονική μηχανή](site-recovery-best-practices.md#virtual-machines).

Για να ενεργοποιήσετε την προστασία με το λειτουργικό σύστημα και το λειτουργικό σύστημα πρέπει να οριστεί ιδιότητες του δίσκου για την εικονική μηχανή. Όταν δημιουργείτε μια εικονική μηχανή στο VMM χρησιμοποιώντας ένα πρότυπο εικονική μηχανή μπορείτε να ορίσετε την ιδιότητα. Μπορείτε επίσης να ορίσετε αυτές τις ιδιότητες για υπάρχοντα εικονικές μηχανές στις καρτέλες **Γενικά** και **Ρύθμιση παραμέτρων υλικού** οι ιδιότητες εικονική μηχανή. Εάν δεν μπορείτε να ορίσετε αυτές τις ιδιότητες στο VMM θα έχετε τη δυνατότητα να ρυθμίσετε τις παραμέτρους στην πύλη του Azure τοποθεσίας αποκατάστασης.



1.  Για να ενεργοποιήσετε την προστασία, εκτελέστε την παρακάτω εντολή για να λάβετε το κοντέινερ προστασίας:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Λάβετε την προστασία οντότητα (Εικονική), εκτελώντας την ακόλουθη εντολή:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Ενεργοποίηση του DR για το Εικονική, εκτελώντας την ακόλουθη εντολή:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Δοκιμή της ανάπτυξης

Για να ελέγξετε την ανάπτυξη, μπορείτε να εκτελέσετε ανακατεύθυνσης δοκιμή για μια εικονική μηχανή, ή να δημιουργήσετε ένα σχέδιο αποκατάστασης που αποτελείται από πολλά εικονικές μηχανές και να εκτέλεση δοκιμής ανακατεύθυνσης για το πρόγραμμα. Δοκιμή ανακατεύθυνσης Προσομοιώνει το μηχανισμό ανακατεύθυνσης και αποκατάστασης σε ένα δίκτυο απομόνωσης. Να σημειωθεί ότι:

- Εάν θέλετε να συνδεθείτε με την εικονική μηχανή στο Azure χρησιμοποιώντας απομακρυσμένη επιφάνεια εργασίας μετά την ανακατεύθυνση, ενεργοποίηση σύνδεση απομακρυσμένης επιφάνειας εργασίας στον υπολογιστή εικονικές πριν από την εκτέλεση του εφεδρικού δοκιμής.
- Μετά την ανακατεύθυνση, θα χρησιμοποιήσετε μια δημόσια διεύθυνση IP για να συνδεθείτε με την εικονική μηχανή στο Azure χρησιμοποιώντας απομακρυσμένης επιφάνειας εργασίας. Εάν θέλετε να το κάνετε αυτό, βεβαιωθείτε ότι δεν έχετε τις πολιτικές τομέα που θα εμποδίσει να μια εικονική μηχανή χρησιμοποιώντας μια δημόσια διεύθυνση.

Για να ελέγξετε την ολοκλήρωση της λειτουργίας, ακολουθήστε τα βήματα στην [Εποπτεία δραστηριότητας](#monitor).

### <a name="create-a-recovery-plan"></a>Δημιουργία σχεδίου ανάκτησης

1. Δημιουργήστε ένα αρχείο .xml ως πρότυπο για το πρόγραμμά σας αποκατάστασης χρησιμοποιώντας τα παρακάτω δεδομένα και, στη συνέχεια, αποθηκεύστε το ως "C:\RPTemplatePath.xml".
2. Αλλάξτε το RecoveryPlan κόμβου αναγνωριστικό, όνομα, PrimaryServerId και SecondaryServerId.
3. Αλλάξτε τον κόμβο ProtectionEntity PrimaryProtectionEntityId (vmid από VMM).
4. Μπορείτε να προσθέσετε περισσότερες ΣΠΣ, προσθέτοντας περισσότερα ProtectionEntity κόμβους.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Συμπληρώστε τα δεδομένα στο πρότυπο:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Δημιουργήστε το RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Εκτέλεση δοκιμής ανακατεύθυνσης

1.  Λήψη του αντικειμένου RecoveryPlan, εκτελώντας την ακόλουθη εντολή:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Ξεκινήστε την ανακατεύθυνση δοκιμής, εκτελώντας την ακόλουθη εντολή:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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

[Διαβάστε περισσότερα](https://msdn.microsoft.com/library/dn850420.aspx) σχετικά με τα cmdlet του PowerShell αποκατάστασης Azure τοποθεσίας. </a>.

<properties
    pageTitle="Προστασία διακομιστές Azure με χρήση του Azure PowerShell με τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Αυτοματοποίηση προστασία των διακομιστές Azure με Επαναφορά τοποθεσίας Azure με τη χρήση του PowerShell και διαχείριση πόρων Azure."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Αναπαραγάγετε μεταξύ της εσωτερικής Hyper-V εικονικές μηχανές και Azure χρησιμοποιώντας το PowerShell και διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](site-recovery-hyper-v-site-to-azure.md)
- [PowerShell - διαχείριση πόρων](site-recovery-deploy-with-powershell-resource-manager.md)
- [Κλασική πύλη](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Επισκόπηση

Azure Επαναφορά τοποθεσίας συμβάλλει της στρατηγικής ανάκτησης επιχειρήσεις συνέχειας και καταστροφή από orchestrating αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των εικονικές μηχανές έναν αριθμό σεναρίων ανάπτυξης. Για μια πλήρη λίστα των σενάρια ανάπτυξης, ανατρέξτε στο θέμα η [Επισκόπηση Azure τοποθεσίας αποκατάστασης](site-recovery-overview.md).

Azure PowerShell είναι μια λειτουργική μονάδα που παρέχει το cmdlet για τη Διαχείριση Azure μέσω του Windows PowerShell. Μπορεί να λειτουργήσει με δύο τύπους λειτουργικών μονάδων: λειτουργική μονάδα Azure προφίλ ή τη λειτουργική μονάδα Azure διαχείριση πόρων.

Τοποθεσία αποκατάστασης cmdlet του PowerShell, διαθέσιμη με το PowerShell Azure για Azure διαχείριση πόρων, θα σας βοηθήσουν να προστατεύσετε και να ανακτήσετε τους διακομιστές με Azure.

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης του Windows PowerShell, μαζί με το Azure διαχείριση πόρων, για να αναπτύξετε Επαναφορά τοποθεσίας για να ρυθμίσετε τις παραμέτρους και να οργανώσετε προστασία server Azure. Το παράδειγμα που χρησιμοποιείται σε αυτό το άρθρο θα μάθετε πώς να προστατεύσετε και ανακατευθύνει ανάκτηση εικονικές μηχανές σε ένα κεντρικό υπολογιστή Hyper-V για να Azure, με χρήση του Azure PowerShell με τη διαχείριση πόρων Azure.

> [AZURE.NOTE] Τα cmdlet του PowerShell αποκατάστασης τοποθεσίας αυτήν τη στιγμή σάς επιτρέπουν να ρυθμίσετε τα εξής: μία εικονική μηχανή Διαχείριση τοποθεσίας σε μια άλλη, μια τοποθεσία εικονική μηχανή Manager για να Azure και μια τοποθεσία Hyper-V ώστε να Azure.

Δεν χρειάζεται να είναι ένα ειδικό PowerShell για να χρησιμοποιήσετε αυτό το άρθρο, αλλά πρέπει να κατανοήσετε τις βασικές έννοιες, όπως λειτουργικές μονάδες, των cmdlet και οι περίοδοι λειτουργίας. Για περισσότερες πληροφορίες σχετικά με το Windows PowerShell, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Μπορείτε επίσης να διαβάσετε περισσότερα σχετικά με τη [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md).

> [AZURE.NOTE] Οι συνεργάτες της Microsoft που αποτελούν μέρος του προγράμματος παροχής λύσεων Cloud (CSP) να ρυθμίσετε τις παραμέτρους και να διαχειριστείτε προστασία των πελατών τους διακομιστές τους πελάτες αντίστοιχα υπηρεσία παροχής Κρυπτογράφησης συνδρομές (μισθωτή συνδρομές).

## <a name="before-you-start"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε αυτές τις προϋποθέσεις στη θέση:

- Ένας λογαριασμός [Microsoft Azure](https://azure.microsoft.com/) . Μπορείτε να ξεκινήσετε με μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/). Επιπλέον, μπορείτε να διαβάσετε σχετικά με [τις τιμές διαχείριση αποκατάστασης τοποθεσίας Azure](https://azure.microsoft.com/pricing/details/site-recovery/).
- Azure PowerShell 1.0. Για πληροφορίες σχετικά με αυτήν την έκδοση και πώς μπορείτε να το εγκαταστήσετε, ανατρέξτε στο θέμα [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- Οι λειτουργικές μονάδες [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) και [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) . Μπορείτε να λάβετε τις πιο πρόσφατες εκδόσεις των αυτές οι λειτουργικές μονάδες από τη [συλλογή του PowerShell](https://www.powershellgallery.com/)

Σε αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης του Powershell Azure με τη Διαχείριση Azure πόρων για να ρυθμίσετε και να διαχειριστείτε την προστασία των διακομιστών. Το παράδειγμα που χρησιμοποιείται σε αυτό το άρθρο σας δείχνει πώς να προστατεύσετε μια εικονική μηχανή, εκτελείται σε μια υπηρεσία παροχής φιλοξενίας Hyper-V, για να Azure. Οι ακόλουθες προϋποθέσεις είναι συγκεκριμένες για αυτό το παράδειγμα. Για μια πιο ολοκληρωμένη σύνολο απαιτήσεις για τα διάφορα σενάρια αποκατάστασης τοποθεσίας, ανατρέξτε στην τεκμηρίωση που σχετίζονται με αυτό το σενάριο.

- Μια Hyper-V κεντρικό υπολογιστή που εκτελεί Windows Server 2012 R2 ή Microsoft Hyper-V Server 2012 R2 που περιέχει μία ή περισσότερες εικονικές μηχανές.
- Διακομιστές Hyper-V συνδεδεμένοι στο Internet, είτε άμεσα είτε μέσω ενός διακομιστή μεσολάβησης.
- Θέλετε να προστατεύσετε το εικονικές μηχανές θα πρέπει να συμφωνούν με [τις προϋποθέσεις εικονική μηχανή](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Βήμα 1: Πραγματοποιήστε είσοδο στο λογαριασμό σας στο Azure


1. Ανοίξτε μια κονσόλα PowerShell και να εκτελέσετε αυτήν την εντολή για να πραγματοποιήσετε είσοδο στο λογαριασμό σας Azure. Το cmdlet εμφανίζει μια ιστοσελίδα που θα σας ειδοποιήσει για τα διαπιστευτήριά σας λογαριασμό.

        Login-AzureRmAccount

    Εναλλακτικά, μπορείτε επίσης να συμπεριλάβετε τα διαπιστευτήριά σας λογαριασμό ως παράμετρο, για να το `Login-AzureRmAccount` cmdlet, χρησιμοποιώντας το `-Credential` παραμέτρου.

    Εάν είστε συνεργάτης υπηρεσία παροχής Κρυπτογράφησης εργασία εκ μέρους μισθωτή, καθορίστε τον πελάτη ως ένα μισθωτή, χρησιμοποιώντας τους tenantID ή μισθωτή κύριο όνομα τομέα.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Ένας λογαριασμός μπορεί να έχει πολλές συνδρομές, έτσι θα πρέπει να μπορείτε να συσχετίσετε τη συνδρομή που θέλετε να χρησιμοποιήσετε με το λογαριασμό.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Βεβαιωθείτε ότι έχει καταχωρηθεί η συνδρομή σας για να χρησιμοποιήσετε τις υπηρεσίες παροχής Azure για υπηρεσίες ανάκτησης και Επαναφορά τοποθεσίας, χρησιμοποιώντας τις παρακάτω εντολές:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    Στη λίστα εξόδου από αυτές τις εντολές, εάν η **RegistrationState** έχει οριστεί σε **καταχωρημένο**, μπορείτε να προχωρήσετε στο βήμα 2. Εάν όχι, θα πρέπει να καταχωρείτε την υπηρεσία παροχής που λείπουν από τη συνδρομή σας.

    Για να καταχωρήσετε την υπηρεσία παροχής του Azure για επαναφορά τοποθεσίας και υπηρεσίες ανάκτησης, εκτελέστε τις εξής εντολές:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Βεβαιωθείτε ότι οι υπηρεσίες παροχής που έχουν καταχωρηθεί με επιτυχία χρησιμοποιώντας τις παρακάτω εντολές: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` και `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Βήμα 2: Ρύθμιση του θάλαμο υπηρεσίες ανάκτησης

1. Δημιουργία ομάδας του πόρου από διαχειριστή πόρων Azure, στο οποίο θα δημιουργήσετε το θάλαμο ή χρησιμοποιήστε μια υπάρχουσα ομάδα πόρων. Μπορείτε να δημιουργήσετε μια νέα ομάδα πόρων, χρησιμοποιώντας την ακόλουθη εντολή:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    όπου η μεταβλητή $ResourceGroupName περιέχει το όνομα της ομάδας πόρων που θέλετε να δημιουργήσετε και η μεταβλητή $Geo περιέχει την Azure περιοχή στην οποία θέλετε να δημιουργήσετε την ομάδα πόρων (για παράδειγμα, "Νότια Βραζιλίας").

    Μπορείτε να αποκτήσετε μια λίστα με τις ομάδες πόρων στη συνδρομή σας, χρησιμοποιώντας το `Get-AzureRmResourceGroup` cmdlet.

2. Δημιουργήστε μια νέα θάλαμο υπηρεσίες ανάκτησης Azure ως εξής:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Μπορείτε να ανακτήσετε μια λίστα με τις υπάρχουσες χώροι φύλαξης, χρησιμοποιώντας το `Get-AzureRmRecoveryServicesVault` cmdlet.

> [AZURE.NOTE] Εάν θέλετε να εκτελούν λειτουργίες σε χώροι φύλαξης Επαναφορά τοποθεσίας που δημιουργήθηκε με την πύλη κλασική ή τη λειτουργική μονάδα Azure υπηρεσίας διαχείρισης PowerShell, μπορείτε να ανακτήσετε μια λίστα των εν λόγω χώροι φύλαξης, χρησιμοποιώντας το `Get-AzureRmSiteRecoveryVault` cmdlet. Θα πρέπει να δημιουργήσετε μια νέα θάλαμο υπηρεσίες ανάκτησης για όλες τις νέες εργασίες. Επαναφορά τοποθεσίας θαλάμους που δημιουργήσατε νωρίτερα υποστηρίζονται, αλλά δεν έχετε τις πιο πρόσφατες δυνατότητες.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Βήμα 3: Ρύθμιση του περιβάλλοντος θάλαμο υπηρεσίες ανάκτησης

1.  Ρύθμιση του περιβάλλοντος θάλαμο, εκτελώντας την ακόλουθη εντολή:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Βήμα 4: Δημιουργία τοποθεσίας Hyper-V και δημιουργήστε ένα νέο κλειδί εγγραφής θάλαμο για την τοποθεσία.

1. Δημιουργήστε μια νέα τοποθεσία Hyper-V ως εξής:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Αυτό το cmdlet ξεκινά μια εργασία Επαναφορά τοποθεσίας για να δημιουργήσετε την τοποθεσία και επιστρέφει ένα αντικείμενο εργασίας Επαναφορά τοποθεσίας. Περιμένετε για το έργο για να ολοκληρώσετε και επιβεβαιώστε ότι η εργασία ολοκληρώθηκε με επιτυχία.

    Μπορείτε να ανακτήσετε το αντικείμενο εργασίας και συνεπώς να ελέγξετε την τρέχουσα κατάσταση της εργασίας, χρησιμοποιώντας το cmdlet Get-AzureRmSiteRecoveryJob.
2. Δημιουργία και να κάνετε λήψη μιας εγγραφής κλειδί για την τοποθεσία, ως εξής:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Αντιγράψτε το κλειδί που έχετε λάβει στον κεντρικό υπολογιστή Hyper-V. Χρειάζεστε το κλειδί για να καταχωρήσετε τον κεντρικό υπολογιστή του Hyper-V στην τοποθεσία.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Βήμα 5: Εγκατάσταση η υπηρεσία παροχής Επαναφορά τοποθεσίας Azure και παράγοντα υπηρεσίες ανάκτησης Azure στην υπηρεσία παροχής φιλοξενίας Hyper-V

1. Κάντε λήψη του προγράμματος εγκατάστασης για την πιο πρόσφατη έκδοση της υπηρεσίας παροχής από τη [Microsoft](https://aka.ms/downloaddra).

2. Εκτέλεση του προγράμματος εγκατάστασης στην υπηρεσία παροχής φιλοξενίας Hyper-V και στο τέλος της εγκατάστασης, συνεχίστε με το βήμα εγγραφής.

3. Όταν σας ζητηθεί, δώστε την τοποθεσία λήψης εγγραφής κλειδί, και την καταγραφή ολοκλήρωσης του κεντρικού υπολογιστή Hyper-V με την τοποθεσία.

4. Βεβαιωθείτε ότι ο κεντρικός υπολογιστής Hyper-V έχει καταχωρηθεί στην τοποθεσία, χρησιμοποιώντας την ακόλουθη εντολή:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Βήμα 6: Δημιουργία πολιτικής αναπαραγωγής και να συσχετίσετε με το κοντέινερ προστασίας

1. Δημιουργία πολιτικής αναπαραγωγής ως εξής:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Ελέγξτε το επιστρεφόμενο εργασία για να βεβαιωθείτε ότι η δημιουργία πολιτικής αναπαραγωγής ολοκληρωθεί με επιτυχία.

    >[AZURE.IMPORTANT] Το λογαριασμό χώρου αποθήκευσης που καθορίζεται πρέπει να είναι στην ίδια περιοχή Azure ως σας θάλαμο υπηρεσίες ανάκτησης και πρέπει να έχει ενεργοποιηθεί παν-αναπαραγωγής.
    >
    > - Εάν ο καθορισμένος λογαριασμός αποκατάστασης χώρου αποθήκευσης είναι τύπου αποθήκευσης Azure (κλασικό), ανακατεύθυνσης από το προστατευμένο μηχανές ανάκτηση του υπολογιστή για να Azure IaaS (κλασικό).
    > - Εάν η καθορισμένη αποκατάστασης λογαριασμού χώρου αποθήκευσης είναι τύπου Azure χώρου αποθήκευσης (Διαχείριση πόρων Azure), ανακατεύθυνσης από το προστατευμένο μηχανές ανάκτηση του υπολογιστή για να Azure IaaS (Διαχείριση πόρων Azure).

2. Λήψη του κοντέινερ προστασίας που αντιστοιχεί στην τοποθεσία, ως εξής:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Ξεκινήστε τη συσχέτιση του κοντέινερ προστασία με την πολιτική αναπαραγωγής, ως εξής:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Περιμένετε για τη δουλειά συσχετισμού για να ολοκληρώσετε και βεβαιωθείτε ότι ολοκληρώθηκε με επιτυχία.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Βήμα 7: Ενεργοποίηση προστασίας για εικονικές μηχανές

1. Εμφανίζεται η οντότητα προστασίας αντιστοιχεί την εικονική Μηχανή που θέλετε να προστατεύσετε, ως εξής:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Ξεκινήστε προστασία το εικονικό υπολογιστή, ως εξής:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Το λογαριασμό χώρου αποθήκευσης που καθορίζεται πρέπει να είναι στην ίδια περιοχή Azure ως σας θάλαμο υπηρεσίες ανάκτησης και πρέπει να έχει ενεργοποιηθεί παν-αναπαραγωγής.
    >
    > - Εάν ο καθορισμένος λογαριασμός αποκατάστασης χώρου αποθήκευσης είναι τύπου αποθήκευσης Azure (κλασικό), ανακατεύθυνσης από το προστατευμένο μηχανές ανάκτηση του υπολογιστή για να Azure IaaS (κλασικό).
    > - Εάν η καθορισμένη αποκατάστασης λογαριασμού χώρου αποθήκευσης είναι τύπου Azure χώρου αποθήκευσης (Διαχείριση πόρων Azure), ανακατεύθυνσης από το προστατευμένο μηχανές ανάκτηση του υπολογιστή για να Azure IaaS (Διαχείριση πόρων Azure).

    > Εάν η Εικονική προστατεύετε έχει περισσότερα από ένα δίσκο συνημμένη, καθορίστε το δίσκο του λειτουργικού συστήματος, χρησιμοποιώντας την παράμετρο *OSDiskName* .

3. Περιμένετε για τις εικονικές μηχανές για την επίτευξη κατάσταση προστατευμένο μετά την αρχική αναπαραγωγής. Αυτό μπορεί να διαρκέσει λίγο, ανάλογα με παράγοντες όπως την ποσότητα των δεδομένων να αναπαραχθεί και το διαθέσιμο εύρος ζώνης νέα να Azure. Η κατάσταση εργασίας και StateDescription ενημερώνονται ως εξής, κατά την εικονική Μηχανή επικοινωνούν μαζί σε ένα προστατευμένο μέλος.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Ενημέρωση αποκατάστασης ιδιότητες, όπως το μέγεθος του ρόλου Εικονική και του Azure δικτύου για να επισυνάψετε την εικονική μηχανή περιβάλλοντος εργασίας κάρτες δικτύου για κατά την ανακατεύθυνση.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Βήμα 8: Εκτέλεση δοκιμής ανακατεύθυνσης

1. Εκτελέστε μια εργασία ανακατεύθυνσης δοκιμής, ως εξής:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Βεβαιωθείτε ότι η δοκιμή Εικονική δημιουργείται στο Azure. (Η εργασία ανακατεύθυνσης δοκιμής έχει ανασταλεί, μετά τη δημιουργία της δοκιμής Εικονική στο Azure. Η εργασία ολοκληρώνεται με εκκαθάριση του που έχουν δημιουργηθεί artefacts κατά την επαναφορά της εργασίας, όπως φαίνεται στο επόμενο βήμα.)

3. Ολοκληρώστε την ανακατεύθυνση δοκιμής, ως εξής:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Επόμενα βήματα

[Διαβάστε περισσότερα](https://msdn.microsoft.com/library/azure/mt637930.aspx) σχετικά με την Επαναφορά τοποθεσίας Azure με το cmdlet του PowerShell για τη διαχείριση πόρων Azure.

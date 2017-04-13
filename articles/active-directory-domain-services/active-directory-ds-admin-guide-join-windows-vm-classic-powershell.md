<properties
    pageTitle="Υπηρεσίες τομέα της υπηρεσίας καταλόγου Azure Active Directory: Οδηγός διαχείρισης | Microsoft Azure"
    description="Συμμετοχή σε μια εικονική μηχανή των Windows σε διαχειριζόμενες τομέα με χρήση του Azure PowerShell και το μοντέλο κλασική ανάπτυξης."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Συμμετοχή σε μια εικονική μηχανή Windows Server με χρήση του PowerShell διαχειριζόμενων τομέα

> [AZURE.SELECTOR]
- [Azure κλασική πύλης - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα για τη δημιουργία και εργασία με πόρους: [Διαχείριση πόρων και κλασική](../resource-manager-deployment-model.md). Σε αυτό το άρθρο καλύπτει χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης. Azure υπηρεσίες τομέα AD δεν υποστηρίζει τη συγκεκριμένη στιγμή στο μοντέλο διαχείρισης πόρων.

Αυτά τα βήματα σας δείχνουν πώς μπορείτε να προσαρμόσετε ένα σύνολο των εντολών του PowerShell Azure που δημιουργείτε και να ρυθμίσουν εκ των προτέρων βασίζεται σε Windows Azure εικονικό μηχάνημα, χρησιμοποιώντας μια προσέγγιση μπλοκ δόμησης. Αυτά τα βήματα θα σας βοηθήσει να δημιουργήσετε έναν υπολογιστή με Windows Azure εικονικές και τη συμμετοχή του σε μια διαχειριζόμενη τομέα υπηρεσίες τομέα AD Azure.

Μια προσέγγιση συμπλήρωσης-σε-το-κενά για τη δημιουργία σύνολα εντολών του PowerShell Azure, ακολουθήστε τα παρακάτω βήματα. Αυτή η προσέγγιση μπορεί να είναι χρήσιμο εάν είστε εξοικειωμένοι με το PowerShell ή εάν θέλετε να γνωρίζετε ποιες τιμές για να καθορίσετε για επιτυχή ρύθμισης παραμέτρων. Οι προχωρημένοι PowerShell να κάνετε τις εντολές και να αντικαταστήσετε τις δικές τους τιμές για τις μεταβλητές (τις γραμμές που αρχίζουν από "$").

Εάν δεν το έχετε κάνει ήδη, χρησιμοποιήστε τις οδηγίες στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md) για να εγκαταστήσετε το Azure PowerShell στον τοπικό σας υπολογιστή. Στη συνέχεια, ανοίξτε μια γραμμή εντολών του Windows PowerShell.

## <a name="step-1-add-your-account"></a>Βήμα 1: Προσθήκη λογαριασμού

1. Στη γραμμή εντολών του PowerShell, πληκτρολογήστε **Πρόσθετο AzureAccount** και πιέστε το πλήκτρο **Enter**.
2. Πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου που σχετίζονται με τη συνδρομή σας στο Azure και κάντε κλικ στην επιλογή **Continue**.
3. Πληκτρολογήστε τον κωδικό πρόσβασης για το λογαριασμό σας.
4. Κάντε κλικ στην επιλογή **Είσοδος**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Βήμα 2: Ορίστε τη συνδρομή σας και το λογαριασμό χώρου αποθήκευσης

Ορίστε τις Azure συνδρομή σας και το λογαριασμό χώρου αποθήκευσης με την εκτέλεση αυτών των εντολών στη γραμμή εντολών του Windows PowerShell. Αντικαταστήστε τα πάντα μέσα σε τις προσφορές, συμπεριλαμβανομένων του < και > χαρακτήρες, με τα σωστά ονόματα.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Μπορείτε να λάβετε το όνομα της συνδρομής σωστή από την ιδιότητα SubscriptionName από την έξοδο από την εντολή **Get-AzureSubscription** . Μπορείτε να λάβετε το όνομα του λογαριασμού σωστή χώρου αποθήκευσης από την ιδιότητα ετικέτας από την έξοδο από την εντολή **Get-AzureStorageAccount** μετά την εκτέλεση της εντολής **Επιλέξτε AzureSubscription** .


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Βήμα 3: Αναλυτικές οδηγίες βήμα προς βήμα - προμήθεια η εικονική μηχανή και τη συμμετοχή του στον τομέα διαχειριζόμενων
Παρακάτω θα δείτε την αντίστοιχη εντολή Azure PowerShell ρύθμιση για να δημιουργήσετε το εικονικό υπολογιστή, με κενές γραμμές ανάμεσα σε κάθε μπλοκ για εύκολη ανάγνωση.

Καθορίστε πληροφορίες σχετικά με το Windows εικονική μηχανή προμήθεια.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Για τις τιμές InstanceSize για D, DS ή σειρά G εικονικές μηχανές, ανατρέξτε στο θέμα [εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Παρέχει πληροφορίες σχετικά με τον τομέα διαχειριζόμενων.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Καθορίστε το όνομα της υπηρεσίας cloud.

    $svcname="Contoso100-test"

Καθορίστε το όνομα του εικονικού δικτύου στο οποίο πρέπει να είναι συνδεδεμένα τα Εικονική. Βεβαιωθείτε ότι ο τομέας διαχειριζόμενων AAD DS είναι διαθέσιμες σε αυτό το εικονικό δίκτυο.

    $vnetname="MyPreviewVnet"

Επιλέξτε την εικόνα Εικονική για να χρησιμοποιηθούν για την παροχή του Εικονική.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Ρυθμίστε τις παραμέτρους του Εικονική - όνομα Εικονική ρύθμιση, παρουσία μέγεθος & εικόνα που θα χρησιμοποιηθεί.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Προμηθευτείτε πιστοποιήσεις τοπικού διαχειριστή για την εικονική Μηχανή. Επιλέξτε έναν κωδικό πρόσβασης ισχυρό τοπικού διαχειριστή.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Αποκτήστε τα διαπιστευτήρια για ένα λογαριασμό χρήστη που ανήκουν σε ομάδα 'Διαχειριστές ελεγκτή Τομέα AAD' για να συμμετάσχετε σε Εικονική διαχειριζόμενων στον τομέα. Δεν καθορίσετε το όνομα τομέα - για παράδειγμα, στο παράδειγμά μας, θα σας Καθορίστε 'ο Μπάμπης' ως το όνομα χρήστη.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Ρύθμιση παραμέτρων του Εικονική - Καθορισμός απαίτησης συμμετοχή τομέα & απαιτούμενα διαπιστευτήρια.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Ορίστε ένα υποδίκτυο για την εικονική Μηχανή.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Προαιρετικό: Τοποθετήστε το δείκτη στη διεύθυνση IP του τομέα. Εάν ορίσετε τις διευθύνσεις IP του υπηρεσίες τομέα AD Azure διαχειριζόμενων τομέα για να τους διακομιστές DNS για το εικονικό δίκτυο, αυτό το βήμα δεν απαιτείται.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Τώρα, παροχή η Εικονική Windows τομέα.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Δέσμη ενεργειών για την παροχή μια Εικονική Windows και αυτόματα τον συνδέσετε μια διαχειριζόμενη AAD τομέα υπηρεσιών τομέα
Αυτό το σύνολο εντολών του PowerShell δημιουργεί μια εικονική μηχανή για ένα διακομιστή γραμμής εταιρικά που:

- Χρησιμοποιεί την εικόνα του Windows Server 2012 R2 κέντρου δεδομένων.
- Είναι ένα πολύ μικρό εικονική μηχανή.
- Περιλαμβάνει το όνομα contoso-δοκιμής.
- Αυτόματη τομέα συνδέεται με το contoso100 διαχειριζόμενων τομέα.
- Προστίθεται στο ίδιο δίκτυο εικονικού ως διαχειριζόμενη τομέα.

Παρακάτω θα δείτε την πλήρη δέσμη ενεργειών για να δημιουργήσετε την εικονική μηχανή Windows και αυτόματα τον συνδέσετε στον τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Σχετικό περιεχόμενο
- [Υπηρεσίες Azure τομέα AD - Οδηγός γρήγορων αποτελεσμάτων](./active-directory-ds-getting-started.md)

- [Διαχείριση ενός τομέα διαχειριζόμενων υπηρεσίες τομέα AD Azure](./active-directory-ds-admin-guide-administer-domain.md)

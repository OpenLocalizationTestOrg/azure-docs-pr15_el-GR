<properties
    pageTitle="Δημιουργήστε μια εικονική μηχανή SQL Server στο Azure PowerShell (κλασικό) | Microsoft Azure"
    description="Παρέχει τα βήματα και δεσμών ενεργειών του PowerShell για να δημιουργήσετε μια Εικονική Azure με εικόνες συλλογή εικονική μηχανή SQL Server. Αυτό το θέμα χρησιμοποιεί τη λειτουργία κλασική ανάπτυξης."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/07/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Προμήθεια μια εικονική μηχανή SQL Server με χρήση του PowerShell Azure (κλασικό)

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο παρέχει τα βήματα για το πώς μπορείτε να δημιουργήσετε μια εικονική μηχανή SQL Server στο Azure χρησιμοποιώντας τα cmdlet του PowerShell.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Για τη διαχείριση πόρων έκδοση αυτού του θέματος, ανατρέξτε στο θέμα [παροχή μια εικονική μηχανή SQL Server με χρήση του PowerShell Azure για τη διαχείριση πόρων](virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell:

1. Εάν δεν έχετε λογαριασμό Azure, επισκεφθείτε [Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

2. [Κάντε λήψη και εγκαταστήστε την πιο πρόσφατη εντολές του Azure PowerShell](../powershell-install-configure.md).

3. Εκκινήστε το Windows PowerShell και συνδέστε τη με τη συνδρομή σας Azure με την εντολή **Προσθήκη AzureAccount** .

        Add-AzureAccount

## <a name="determine-your-target-azure-region"></a>Προσδιορισμός του Azure περιοχή προορισμού

Το SQL Server εικονική μηχανή θα να φιλοξενούνται σε μια υπηρεσία cloud που βρίσκεται μια συγκεκριμένη περιοχή Azure. Τα παρακάτω βήματα σας βοηθούν να προσδιορίσετε το χώρο αποθήκευσης στο λογαριασμό σας περιοχής, και στο cloud υπηρεσιών που θα χρησιμοποιηθεί για τα υπόλοιπα του προγράμματος εκμάθησης.

1. Προσδιορίστε το κέντρο δεδομένων που θέλετε να χρησιμοποιήσετε για τη φιλοξενία του Εικονική SQL Server. Τις παρακάτω εντολές του PowerShell θα εμφανίζει τις διαθέσιμες περιοχές με λεπτομέρειες με μια λίστα σύνοψης στο τέλος.

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  Αφού έχετε προσδιορίσει προτιμώμενη θέση σας, ορίστε μια μεταβλητή με όνομα **$dcLocation** σε αυτήν την περιοχή.

        $dcLocation = "<region name>"

## <a name="set-your-subscription-and-storage-account"></a>Ορίστε το λογαριασμό σας συνδρομή και χώρου αποθήκευσης

1. Προσδιορίστε το Azure συνδρομή που θα χρησιμοποιήσετε για τη νέα εικονική μηχανή.

        (Get-AzureSubscription).SubscriptionName

1. Εκχωρήστε το Azure συνδρομή προορισμού στη μεταβλητή **$subscr** . Στη συνέχεια, ορίστε αυτή ως την τρέχουσα συνδρομή σας Azure.

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. Στη συνέχεια, ελέγξτε για υπαρχόντων λογαριασμών χώρου αποθήκευσης. Η ακόλουθη δέσμη ενεργειών εμφανίζει όλους τους λογαριασμούς χώρου αποθήκευσης που υπάρχουν στην επιλεγμένη περιοχή σας:

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] Εάν χρειάζεστε νέο λογαριασμό του χώρου αποθήκευσης, πρέπει πρώτα να δημιουργήσετε ένα όνομα λογαριασμού όλα πεζά χώρου αποθήκευσης με την εντολή Δημιουργία AzureStorageAccount όπως στο ακόλουθο παράδειγμα: **Δημιουργία AzureStorageAccount - StorageAccountName "<storage account name>"-θέση $dcLocation**

1. Αντιστοιχίστε το όνομα λογαριασμού του χώρου αποθήκευσης προορισμού για να το **$staccount**. Στη συνέχεια, χρησιμοποιήστε το **Σύνολο AzureSubscription** για να ορίσετε τη συνδρομή και τρέχοντα λογαριασμό χώρου αποθήκευσης.

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## <a name="select-a-sql-server-virtual-machine-image"></a>Επιλέξτε μια εικόνα εικονική μηχανή SQL Server

1. Μάθετε τη λίστα των διαθέσιμων εικόνων εικονικές μηχανές SQL Server από τη συλλογή. Αυτές οι εικόνες που έχουν μια ιδιότητα **ImageFamily** που ξεκινά με "SQL". Το ακόλουθο ερώτημα εμφανίζει της οικογένειας εικόνα διαθέσιμες σε εσάς που έχετε προεγκατεστημένα τον SQL Server.

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. Όταν βρείτε την οικογένεια εικόνα εικονική μηχανή, ενδέχεται να υπάρχουν πολλά δημοσιευμένες εικόνες σε αυτήν την οικογένεια. Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για να βρείτε την πιο πρόσφατη όνομα εικόνας δημοσιευμένο εικονική μηχανή για την οικογένειά σας επιλεγμένης εικόνας (όπως **SQL Server 2016 RTM για μεγάλες επιχειρήσεις σε Windows Server 2012 R2**):

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## <a name="create-the-virtual-machine"></a>Δημιουργήστε την εικονική μηχανή

Τέλος, δημιουργήστε την εικονική μηχανή με το PowerShell:

1. Δημιουργήστε μια υπηρεσία cloud να φιλοξενήσετε τη νέα Εικονική. Σημειώστε ότι είναι επίσης πιθανό να χρησιμοποιήσετε μια υπάρχουσα υπηρεσία cloud αντί για αυτό. Δημιουργήστε μια νέα μεταβλητή **$svcname** με το σύντομο όνομα της υπηρεσίας cloud.

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. Καθορίστε το όνομα του υπολογιστή εικονικές και μέγεθος. Για περισσότερες πληροφορίες σχετικά με τα μεγέθη εικονική μηχανή, ανατρέξτε στο θέμα [Μεγέθη εικονική μηχανή για Azure](virtual-machines-windows-sizes.md).

        $vmname="<machine name>"
        $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. Καθορίστε το λογαριασμό του τοπικού διαχειριστή και τον κωδικό πρόσβασης.

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. Εκτελέστε την ακόλουθη δέσμη ενεργειών για να δημιουργήσετε την εικονική μηχανή.

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] Για πρόσθετες επεξήγηση και επιλογές ρύθμισης παραμέτρων, ανατρέξτε στην ενότητα **Δόμηση του συνόλου εντολών** στο [Azure χρήση του PowerShell για να δημιουργήσετε και να ρυθμίσουν εκ των προτέρων εικονικές μηχανές που βασίζεται στα Windows](virtual-machines-windows-classic-create-powershell.md).

## <a name="example-powershell-script"></a>Παράδειγμα δέσμης ενεργειών του PowerShell

Παρέχει την ακόλουθη δέσμη ενεργειών και παράδειγμα ολοκλήρωσης δέσμης ενεργειών που δημιουργεί μια εικονική μηχανή **SQL Server 2016 RTM για μεγάλες επιχειρήσεις σε Windows Server 2012 R2** . Εάν χρησιμοποιείτε αυτήν τη δέσμη ενεργειών, πρέπει να προσαρμόσετε το αρχικό μεταβλητές με βάση τα προηγούμενα βήματα σε αυτό το θέμα.

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5"

    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1


## <a name="connect-with-remote-desktop"></a>Σύνδεση με απομακρυσμένη επιφάνεια εργασίας

1. Δημιουργία του. Αρχεία RDP στο φάκελο εγγράφων του τρέχοντος χρήστη για να εμφανίσετε αυτές τις εικονικές μηχανές για να ολοκληρώσετε τη ρύθμιση:

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. Στον κατάλογο έγγραφα, ξεκινήστε το αρχείο RDP. Σύνδεση με το όνομα χρήστη του διαχειριστή και τον κωδικό πρόσβασης που παρέχεται παραπάνω (για παράδειγμα, εάν το όνομα χρήστη ήταν VMAdmin, καθορίστε "\VMAdmin" με το χρήστη και εισαγάγετε τον κωδικό πρόσβασης).

        cd $documentspath
        .\vm1.rdp

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Ολοκληρώστε τη ρύθμιση παραμέτρων του SQL Server υπολογιστή για απομακρυσμένη πρόσβαση

Μετά τη σύνδεση σε υπολογιστή με σύνδεση απομακρυσμένης επιφάνειας εργασίας, ρύθμιση παραμέτρων του SQL Server με βάση τις οδηγίες που εμφανίζονται στα [βήματα για τη ρύθμιση παραμέτρων σύνδεσης του SQL Server σε μια Εικονική Azure](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Επόμενα βήματα

Μπορείτε να βρείτε πρόσθετες οδηγίες για την προμήθεια εικονικές μηχανές με το PowerShell στην [τεκμηρίωση εικονικές μηχανές](virtual-machines-windows-classic-create-powershell.md). Για πρόσθετες δέσμες ενεργειών που σχετίζονται με SQL Server και Premium χώρου αποθήκευσης, ανατρέξτε στο θέμα [Χρήση του χώρου αποθήκευσης Premium Azure με τον SQL Server σε εικονικές μηχανές](virtual-machines-windows-classic-sql-server-premium-storage.md).

Σε πολλές περιπτώσεις, το επόμενο βήμα είναι να μετεγκαταστήσετε τις βάσεις δεδομένων σε αυτό νέα Εικονική SQL Server. Για οδηγίες μετεγκατάσταση βάσης δεδομένων, ανατρέξτε στο θέμα [Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](virtual-machines-windows-migrate-sql.md).

Εάν σας ενδιαφέρει επίσης με την πύλη Azure για να δημιουργήσετε SQL εικονικές μηχανές, ανατρέξτε στο θέμα [προμήθεια του SQL Server εικονικό μηχάνημα σε Azure](virtual-machines-windows-portal-sql-server-provision.md). Σημειώστε ότι το πρόγραμμα εκμάθησης που σας καθοδηγεί μέσω της πύλης δημιουργεί ΣΠΣ χρησιμοποιώντας το προτεινόμενο μοντέλο διαχείρισης πόρων και όχι το μοντέλο κλασική που χρησιμοποιούνται σε αυτό το θέμα του PowerShell.

Εκτός από αυτούς τους πόρους, προτείνουμε να αναθεωρείτε [άλλα θέματα που σχετίζονται με εκτελεί τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-server-iaas-overview.md).

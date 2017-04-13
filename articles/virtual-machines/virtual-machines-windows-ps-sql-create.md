<properties
    pageTitle="Δημιουργήστε μια εικονική μηχανή SQL Server στο Azure PowerShell (Διαχείριση πόρων) | Microsoft Azure"
    description="Παρέχει τα βήματα και δεσμών ενεργειών του PowerShell για να δημιουργήσετε μια Εικονική Azure με εικόνες συλλογή εικονική μηχανή SQL Server."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Προμήθεια μια εικονική μηχανή SQL Server με χρήση του PowerShell Azure (Διαχείριση πόρων)

> [AZURE.SELECTOR]
- [Πύλη](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

## <a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια μεμονωμένη Azure εικονική μηχανή χρησιμοποιώντας το μοντέλο ανάπτυξης **Διαχείρισης πόρων Azure** με χρήση των cmdlet του Azure PowerShell. Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσουμε μια μεμονωμένη εικονική μηχανή χρησιμοποιώντας μια μεμονωμένη μονάδα δίσκου από μια εικόνα στη συλλογή SQL. Θα δημιουργήσουμε νέες υπηρεσίες παροχής για το χώρο αποθήκευσης, του δικτύου και πόροι υπολογισμού που θα χρησιμοποιηθεί από την εικονική μηχανή. Εάν έχετε υπάρχουσες υπηρεσίες παροχής για οποιονδήποτε από αυτούς τους πόρους, μπορείτε να χρησιμοποιήσετε αυτές τις υπηρεσίες παροχής.

Εάν χρειάζεστε την κλασική έκδοση αυτού του θέματος, ανατρέξτε στο θέμα [προμήθεια μια εικονική μηχανή SQL Server με χρήση του PowerShell Azure κλασική](virtual-machines-windows-classic-ps-sql-create.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για αυτό το πρόγραμμα εκμάθησης θα πρέπει:

- Μια λογαριασμός Azure και τη συνδρομή πριν να ξεκινήσετε. Εάν δεν έχετε ένα, εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).
- [Azure PowerShell)](../powershell-install-configure.md), ελάχιστη έκδοση του 1.4.0 ή νεότερη έκδοση (αυτό το πρόγραμμα εκμάθησης γραμμένο με έκδοση 1.5.0).
    - Για να ανακτήσετε τη δική σας έκδοση, πληκτρολογήστε **Get-λειτουργική μονάδα Azure ListAvailable**.

## <a name="configure-your-subscription"></a>Ρύθμιση παραμέτρων τη συνδρομή σας

Ανοίξτε το Windows PowerShell και δημιουργήστε πρόσβαση στο λογαριασμό σας Azure, εκτελώντας το ακόλουθο cmdlet. Θα εμφανιστεί με ένα σύμβολο στην οθόνη για να εισαγάγετε τα διαπιστευτήριά σας. Χρησιμοποιήστε το ίδιο μήνυμα ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.

    Add-AzureRmAccount

Αφού πραγματοποιήσετε είσοδο με επιτυχία στο θα δείτε ορισμένες πληροφορίες στην οθόνη που περιλαμβάνει το SubscriptionId με τα οποία έχετε εισέλθει στο. Αυτή είναι η συνδρομή στην οποία θα δημιουργηθεί τους πόρους για αυτό το πρόγραμμα εκμάθησης, εκτός εάν αλλάξετε σε μια διαφορετική συνδρομή. Εάν έχετε πολλές SubscriptionIds, εκτελέστε το ακόλουθο cmdlet για να επιστρέψει μια λίστα όλων των SubscriptionIds σας:

    Get-AzureRmSubscription

Για να αλλάξετε μια άλλη SubscriptionID, εκτελέστε το ακόλουθο cmdlet με την επιθυμητή SubscriptionId.

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Ορισμός μεταβλητών εικόνας

Για να απλοποιήσετε χρηστικότητα και κατανόηση της ολοκληρωμένων δέσμης ενεργειών από αυτό το πρόγραμμα εκμάθησης, θα σας θα ξεκινήσει με τον ορισμό ενός αριθμού μεταβλητών. Αλλάξτε τις τιμές παραμέτρων, ανατρέξτε στο θέμα Προσαρμογή, ωστόσο προσέχετε τους περιορισμούς που σχετίζονται με όνομα μήκη και ειδικών χαρακτήρων, όταν τροποποιείτε τις τιμές που παρέχονται ονομασίας.

### <a name="location-and-resource-group"></a>Θέση και ομάδα πόρων
Χρησιμοποιήστε δύο μεταβλητές, για να ορίσετε μια περιοχή δεδομένων και την ομάδα των πόρων στο οποίο θα δημιουργήσετε το άλλους πόρους για την εικονική μηχανή.

Τροποποίηση που θέλετε και, στη συνέχεια, εκτελέστε τα ακόλουθα cmdlet για την προετοιμασία αυτών των μεταβλητών.

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Ιδιότητες χώρου αποθήκευσης

Χρησιμοποιήστε τις παρακάτω μεταβλητές για να ορίσετε το λογαριασμό χώρου αποθήκευσης και τον τύπο του χώρου αποθήκευσης που θα χρησιμοποιηθεί από την εικονική μηχανή.

Τροποποίηση που θέλετε και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet για την προετοιμασία αυτών των μεταβλητών. Σημειώστε ότι σε αυτό το παράδειγμα, χρησιμοποιούμε [Premium χώρου αποθήκευσης](../storage/storage-premium-storage.md), που συνιστώνται για παραγωγής φόρτους εργασίας. Για λεπτομέρειες σχετικά με αυτές τις οδηγίες και άλλες συστάσεις, ανατρέξτε στο θέμα [επιδόσεις βέλτιστες πρακτικές για τον SQL Server σε εικονικές μηχανές Windows Azure](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Ιδιότητες δικτύου

Χρησιμοποιήστε τις παρακάτω μεταβλητές για να ορίσετε το περιβάλλον εργασίας δικτύου, τη μέθοδο εκχώρησης TCP/IP, το όνομα εικονικού δικτύου, το όνομα εικονικού υποδίκτυο, την περιοχή διευθύνσεων IP για το εικονικό δίκτυο, την περιοχή διευθύνσεων IP για το υποδίκτυο, και την ετικέτα ονόματος τομέα δημόσια θα χρησιμοποιηθεί από το δίκτυο για την εικονική μηχανή.

Τροποποίηση που θέλετε και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet για την προετοιμασία αυτών των μεταβλητών.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"   

### <a name="virtual-machine-properties"></a>Ιδιότητες εικονική μηχανή

Χρησιμοποιήστε τις παρακάτω μεταβλητές, για να καθορίσετε το όνομα του υπολογιστή εικονικές, το όνομα του υπολογιστή, το μέγεθος εικονική μηχανή και το όνομα του δίσκου λειτουργικό σύστημα για την εικονική μηχανή.

Τροποποίηση που θέλετε και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet για την προετοιμασία αυτών των μεταβλητών.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Ιδιότητες εικόνας

Χρησιμοποιήστε τις παρακάτω μεταβλητές για να ορίσετε την εικόνα για να χρησιμοποιήσετε για την εικονική μηχανή. Σε αυτό το παράδειγμα, χρησιμοποιείται η εικόνα SQL Server 2016 Enterprise.

Τροποποίηση που θέλετε και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet για την προετοιμασία αυτών των μεταβλητών.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Σημειώστε ότι μπορείτε να λάβετε μια πλήρη λίστα των προσφορών εικόνα SQL Server με την εντολή Get-AzureRmVMImageOffer:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Και μπορείτε να δείτε τις διαθέσιμες για μια προσφορά με την εντολή Get-AzureRmVMImageSku SKU. Την ακόλουθη εντολή εμφανίζει όλες τις SKU διαθέσιμη για το **SQL2014SP1 WS2012R2** προσφορά.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Με το μοντέλο ανάπτυξης για τη διαχείριση πόρων, στο πρώτο αντικείμενο που δημιουργείτε είναι η ομάδα πόρων. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx) για να δημιουργήσετε μια ομάδα Azure πόρων και τους πόρους με το όνομα της ομάδας πόρων και τη θέση που καθορίζεται από τις μεταβλητές που ήδη προετοιμασία.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε τη νέα ομάδα πόρων.

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Η εικονική μηχανή απαιτεί πόρων αποθήκευσης για το λειτουργικό σύστημα δίσκο και για τα αρχεία δεδομένων και καταγραφής του SQL Server. Για λόγους ευκολίας, θα μπορούμε να δημιουργήσουμε έναν μόνο δίσκο και για τα δύο. Μπορείτε να επισυνάψετε επιπλέον δίσκων αργότερα χρησιμοποιώντας το cmdlet [Προσθήκη Azure δίσκου](https://msdn.microsoft.com/library/azure/dn495252.aspx) για να τοποθετήσετε τα δεδομένα του SQL Server και αρχείων σε αποκλειστική δίσκων καταγραφής. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) για να δημιουργήσετε ένα λογαριασμό τυπική χώρου αποθήκευσης στη νέα ομάδα πόρων και με το όνομα του λογαριασμού χώρου αποθήκευσης, όνομα Sku του χώρου αποθήκευσης και θέση που ορίζονται από το χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε νέο λογαριασμό σας στο χώρο αποθήκευσης.  

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Δημιουργία πόρων δικτύου

Η εικονική μηχανή απαιτεί έναν αριθμό πόρων δικτύου για συνδεσιμότητα του δικτύου.

- Κάθε εικονική μηχανή απαιτεί ένα εικονικό δίκτυο.
- Ένα εικονικό δίκτυο πρέπει να έχετε τουλάχιστον μία υποδικτύου που ορίζονται από το.
- Περιβάλλον εργασίας δικτύου πρέπει να οριστεί με μια δημόσια ή μια ιδιωτική διεύθυνση IP.

### <a name="create-a-virtual-network-subnet-configuration"></a>Δημιουργία μιας ρύθμισης παραμέτρων υποδικτύου εικονικού δικτύου

Θα ξεκινήσουμε δημιουργώντας μια ρύθμιση παραμέτρων υποδικτύου για μας εικονικού δικτύου. Για το πρόγραμμα εκμάθησης, θα δημιουργήσουμε ένα προεπιλεγμένο υποδίκτυο χρησιμοποιώντας το cmdlet [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) . Θα δημιουργήσουμε μας παραμέτρους υποδικτύου εικονικού δικτύου με το όνομα και τη διεύθυνση πρόθεμα υποδικτύου που ορίζονται από το χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία.

>[AZURE.NOTE] Μπορείτε να ορίσετε πρόσθετες ιδιότητες της ρύθμισης παραμέτρων υποδικτύου εικονικού δικτύου χρησιμοποιώντας αυτό το cmdlet, αλλά αυτό είναι πέρα από το πεδίο αυτού του προγράμματος εκμάθησης.

Εκτελέστε το ακόλουθο cmdlet για τη δημιουργία της ρύθμισης παραμέτρων εικονικού υποδικτύου.

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Δημιουργήστε ένα εικονικό δίκτυο

Στη συνέχεια, θα δημιουργήσουμε μας εικονικού δικτύου, χρησιμοποιώντας το cmdlet [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) . Θα δημιουργήσουμε μας εικονικού δικτύου στη νέα ομάδα πόρων, με το όνομα, θέση και διεύθυνση οριστεί πρόθεμα χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία και χρησιμοποιώντας τη ρύθμιση παραμέτρων υποδικτύου που έχετε ορίσει στο προηγούμενο βήμα.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε το εικονικό δίκτυο.

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Δημιουργία στη δημόσια διεύθυνση IP

Τώρα που έχουμε μας εικονικού δικτύου που ορίζονται από το, πρέπει να ρυθμίσετε τις παραμέτρους μια διεύθυνση IP για σύνδεση με την εικονική μηχανή. Για αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσουμε μια δημόσια διεύθυνση IP με δυναμική IP διευθύνσεων για την υποστήριξη σύνδεσης στο Internet. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) για να δημιουργήσετε στη δημόσια διεύθυνση IP στην ομάδα πόρων δημιουργήθηκε prevously και με το όνομα, θέση, μέθοδο εκχώρησης και ετικέτα ονόματος τομέα DNS που ορίζονται από το χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία.

>[AZURE.NOTE] Μπορείτε να ορίσετε πρόσθετες ιδιότητες του στη δημόσια διεύθυνση IP χρησιμοποιώντας αυτό το cmdlet, αλλά αυτό είναι πέρα από το πεδίο αυτού του προγράμματος εκμάθησης αρχική. Μπορείτε επίσης να δημιουργήσετε μια ιδιωτική διεύθυνση ή μια διεύθυνση με μια στατική διεύθυνση, αλλά αυτό είναι επίσης πέρα από το πεδίο αυτού του προγράμματος εκμάθησης.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε στη δημόσια διεύθυνση IP σας.

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Δημιουργία του περιβάλλοντος εργασίας δικτύου

Συνεχίζουμε να τώρα έτοιμοι να δημιουργήσετε το περιβάλλον εργασίας δικτύου που θα χρησιμοποιήσετε μας εικονική μηχανή. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) για να δημιουργήσετε το περιβάλλον εργασίας δικτύου στην ομάδα πόρων δημιούργησε προηγούμενες και με το όνομα, θέση, υποδικτύου και δημόσια διεύθυνση IP προηγουμένως καθοριστεί.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε το περιβάλλον εργασίας του δικτύου.

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Ρύθμιση παραμέτρων ενός αντικειμένου Εικονική

Τώρα που έχουμε που ορίζονται από το χώρο αποθήκευσης και δικτύου τους πόρους, θα σας είναι έτοιμοι για να ορίσετε πόρους υπολογισμού για την εικονική μηχανή. Για το πρόγραμμα εκμάθησης, θα σας θα προσδιορίσετε το μέγεθος εικονική μηχανή και διάφορες ιδιότητες λειτουργικό σύστημα, να καθορίσετε το περιβάλλον εργασίας δικτύου που ήταν προηγουμένως δημιουργήσαμε, ορισμός χώρος αποθήκευσης αντικειμένων blob και, στη συνέχεια, καθορίστε το δίσκο του λειτουργικού συστήματος.

### <a name="create-the-vm-object"></a>Δημιουργία του αντικειμένου Εικονική

Θα σας θα ξεκινάτε ορίζοντας το μέγεθος εικονική μηχανή. Για αυτό το πρόγραμμα εκμάθησης, θα σας καθορίζοντας ένα DS13. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) για να δημιουργήσετε ένα αντικείμενο με δυνατότητα ρύθμισης παραμέτρων εικονική μηχανή με το όνομα και το μέγεθος που ορίζονται από το χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε το αντικείμενο εικονική μηχανή.

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Δημιουργία ενός αντικειμένου διαπιστευτηρίων σε κρατήστε το όνομα και τον κωδικό πρόσβασης για τα διαπιστευτήρια τοπικού διαχειριστή

Πριν να μπορούμε να εγκαταστήσουμε το λειτουργικό σύστημα ιδιότητες για την εικονική μηχανή, πρέπει να παρέχετε τα διαπιστευτήρια του λογαριασμού τοπικού διαχειριστή ως ασφαλή συμβολοσειρά. Για να γίνει αυτό, θα χρησιμοποιήσουμε το cmdlet [Get-διαπιστευτηρίων](https://technet.microsoft.com/library/hh849815.aspx) .

Εκτελέστε το ακόλουθο cmdlet και, στο παράθυρο αίτησης διαπιστευτηρίων του Windows PowerShell, πληκτρολογήστε το όνομα και τον κωδικό πρόσβασης για το λογαριασμό τοπικού διαχειριστή στον υπολογιστή Windows εικονική.

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Ορισμός των ιδιοτήτων λειτουργικό σύστημα για την εικονική μηχανή

Τώρα θα σας είναι έτοιμη για να ορίσετε ιδιότητες λειτουργικό σύστημα η εικονική μηχανή. Θα χρησιμοποιήσουμε το cmdlet [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) για να ορίσετε τον τύπο του λειτουργικού συστήματος ως των Windows, απαιτούν από τον [παράγοντα εικονική μηχανή](virtual-machines-windows-classic-agents-and-extensions.md) να έχει εγκατασταθεί, να καθορίσετε ότι το cmdlet, σας δίνει τη δυνατότητα αυτόματης ενημέρωσης και να ορίσετε το όνομα εικονική μηχανή, το όνομα του υπολογιστή και τα διαπιστευτήρια χρησιμοποιώντας τις μεταβλητές που ήδη προετοιμασία.

Εκτελέστε το ακόλουθο cmdlet για να ορίσετε τις ιδιότητες λειτουργικό σύστημα για την εικονική μηχανή σας.

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Προσθέστε το περιβάλλον εργασίας δικτύου για να την εικονική μηχανή

Στη συνέχεια, θα προσθέσουμε τη διασύνδεση δικτύου που δημιουργήσαμε προηγουμένως στον υπολογιστή εικονική. Θα χρησιμοποιήσουμε το cmdlet [AzureRmVMNetworkInterface Προσθήκη](https://msdn.microsoft.com/library/mt619351.aspx) για να προσθέσετε το περιβάλλον εργασίας δικτύου, χρησιμοποιώντας τη μεταβλητή περιβάλλοντος εργασίας δικτύου που ορίσατε προηγουμένως.

Εκτελέστε το ακόλουθο cmdlet για να ρυθμίσετε το περιβάλλον εργασίας δικτύου για την εικονική μηχανή σας.

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Ορίστε τη θέση αποθήκευσης αντικειμένων blob του δίσκου που θα χρησιμοποιηθεί από την εικονική μηχανή

Στη συνέχεια, θα σας θα ορίσετε τη θέση αποθήκευσης αντικειμένων blob του δίσκου που θα χρησιμοποιηθεί από την εικονική μηχανή χρησιμοποιώντας τις μεταβλητές που ορίσατε προηγουμένως.

Εκτελέστε το ακόλουθο cmdlet για να ορίσετε τη θέση αποθήκευσης αντικειμένων blob.

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Ρυθμίστε το λειτουργικό σύστημα δίσκου ιδιότητες για την εικονική μηχανή

Στη συνέχεια, θα σας θα ορίσετε το λειτουργικό σύστημα δίσκου ιδιότητες για την εικονική μηχανή. Θα χρησιμοποιήσουμε το cmdlet [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) για να καθορίσετε ότι το λειτουργικό σύστημα για την εικονική μηχανή θα προέρχονται από μια εικόνα, για να ορίσετε σε cache για ανάγνωση μόνο (επειδή SQL Server εγκαθίσταται στον ίδιο δίσκο) και τον ορισμό του ονόματος εικονική μηχανή και ο δίσκος λειτουργικού συστήματος που ορίζονται από το χρησιμοποιώντας τις μεταβλητές που ορίσαμε νωρίτερα.

Εκτελέστε το ακόλουθο cmdlet για να ορίσετε το λειτουργικό σύστημα δίσκου ιδιότητες για την εικονική μηχανή σας.

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Καθορίστε την εικόνα πλατφόρμα για την εικονική μηχανή

Είναι το τελευταίο βήμα ρύθμισης παραμέτρων για να καθορίσετε την εικόνα πλατφόρμα για μας εικονική μηχανή. Για το πρόγραμμα εκμάθησης, χρησιμοποιούμε την πιο πρόσφατη εικόνα SQL Server 2016 CTP. Θα χρησιμοποιήσουμε το cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) για να χρησιμοποιήσετε αυτήν την εικόνα, όπως ορίζεται από τις μεταβλητές που ορίσατε προηγουμένως.

Εκτελέστε το ακόλουθο cmdlet για να καθορίσετε την εικόνα πλατφόρμα για την εικονική μηχανή σας.

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Δημιουργήστε την Εικονική SQL

Τώρα που έχετε ολοκληρώσει τα βήματα ρύθμισης παραμέτρων, είστε έτοιμοι να δημιουργήσετε την εικονική μηχανή. Θα χρησιμοποιήσουμε το cmdlet [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) για να δημιουργήσετε την εικονική μηχανή χρησιμοποιώντας τις μεταβλητές που έχουν ορίσαμε.

Εκτελέστε το ακόλουθο cmdlet για να δημιουργήσετε την εικονική μηχανή σας.

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Η εικονική μηχανή έχει δημιουργηθεί. Παρατηρήστε ότι τυπική αποθήκευσης δημιουργείται ένας λογαριασμός για τα Διαγνωστικά εκκίνησης επειδή ο λογαριασμός καθορισμένο χώρο αποθήκευσης για δίσκο η εικονική μηχανή είναι ένα άρτιο χώρου αποθήκευσης.

Τώρα, μπορείτε να προβάλετε αυτόν τον υπολογιστή στην πύλη του Azure για να δείτε [τη δημόσια διεύθυνση IP και το πλήρως προσδιορισμένο όνομα τομέα](virtual-machines-windows-portal-sql-server-provision.md#Connect).  

## <a name="example-script"></a>Παράδειγμα δέσμης ενεργειών

Η ακόλουθη δέσμη ενεργειών περιέχει την πλήρη δέσμη ενεργειών του PowerShell για αυτό το πρόγραμμα εκμάθησης. Προϋποθέτει ότι έχετε ήδη το πρόγραμμα εγκατάστασης του Azure συνδρομή για χρήση με τις εντολές **AzureRmAccount Προσθήκη** και **Επιλέξτε AzureRmSubscription** .


    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Επόμενα βήματα
Αφού δημιουργηθεί η εικονική μηχανή, είστε έτοιμοι να συνδεθείτε με την εικονική μηχανή χρησιμοποιώντας συνδεσιμότητας RDP και ρύθμιση του. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σύνδεση σε ένα SQL Server εικονικό μηχάνημα σε Azure (Διαχείριση πόρων)](virtual-machines-windows-sql-connect.md).

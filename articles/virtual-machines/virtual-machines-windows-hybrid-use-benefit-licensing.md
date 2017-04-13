<properties
   pageTitle="Azure υβριδική όφελος χρήση παραθύρου διακομιστής | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να μεγιστοποιήσετε σας πλεονεκτήματα εξασφάλισης αναβάθμισης λογισμικού διακομιστή των Windows για να φέρετε άδειες χρήσης στην εσωτερική εγκατάσταση Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Οφέλη χρήσης Azure υβριδική για τον Windows Server

Για πελάτες που χρησιμοποιούν Windows Server με εξασφάλισης αναβάθμισης λογισμικού, να φέρετε σας στην εσωτερική εγκατάσταση αδειών χρήσης του Windows Server Azure και να εκτελέσετε Windows Server ΣΠΣ στο Azure με μειωμένο κόστος. Το πλεονέκτημα της χρήσης υβριδική Azure σάς επιτρέπει να εκτελέσετε ΣΠΣ διακομιστή των Windows στο Azure και λάβετε χρεώνονται μόνο για το συντελεστή βάση υπολογισμού. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το [όφελος χρήση υβριδική Azure παραχώρησης πολλαπλών αδειών χρήσης σελίδας](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Σε αυτό το άρθρο εξηγεί τον τρόπο για την ανάπτυξη του Windows Server ΣΠΣ στο Azure αυτού του οφέλους άδεια χρήσης.

> [AZURE.NOTE] Δεν μπορείτε να χρησιμοποιήσετε εικόνες Azure Marketplace για την ανάπτυξη του Windows Server ΣΠΣ χρησιμοποιώντας το όφελος Χρησιμοποιήστε υβριδική Azure. Πρέπει να αναπτύξετε το ΣΠΣ με τη χρήση του PowerShell ή διαχείριση πόρων προτύπων για να καταχωρήσετε σωστά το ΣΠΣ ως επιλέξιμα προεξοφλητικό επιτόκιο βάση υπολογισμού.

## <a name="pre-requisites"></a>Προαπαιτούμενα
Υπάρχουν μερικοί προαπαιτούμενα προκειμένου να χρησιμοποιούν Azure υβριδική χρήση όφελος για Windows Server ΣΠΣ στο Azure:

- Έχετε εγκατεστημένη τη λειτουργική μονάδα Azure PowerShell
- Έχετε τον VHD διακομιστή των Windows που έχουν αποσταλεί με το χώρο αποθήκευσης Azure

### <a name="install-azure-powershell"></a>Εγκατάσταση του Azure PowerShell
Βεβαιωθείτε ότι έχετε [εγκατεστημένη και ρυθμισμένη την πιο πρόσφατη Azure PowerShell](../powershell-install-configure.md). Ακόμα και αν πρόκειται να αναπτύξετε το ΣΠΣ με τη χρήση προτύπων για τη διαχείριση πόρων, εξακολουθείτε να χρειάζεστε Azure PowerShell εγκατεστημένο για να αποστείλετε το VHD διακομιστή των Windows (ανατρέξτε στο θέμα το παρακάτω βήμα).

### <a name="upload-a-windows-server-vhd"></a>Αποστολή σε Windows Server VHD

Για να αναπτύξετε μια Εικονική διακομιστή των Windows στα Azure, πρέπει πρώτα να δημιουργήσετε έναν VHD που περιέχει το βασικό Δόμηση Windows Server. Αυτό VHD πρέπει να σωστά προετοιμαστεί μέσω Sysprep πριν από την αποστολή Azure. Μπορείτε να [Διαβάστε περισσότερα σχετικά με τις απαιτήσεις VHD και διαδικασία Sysprep](./virtual-machines-windows-upload-image.md) και [Υποστήριξη Sysprep για τους ρόλους διακομιστή](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Δημιουργία αντιγράφου ασφαλείας του Εικονική πριν από την εκτέλεση του Sysprep. Μόλις έχουν προετοιμαστεί σας VHD, αποστολή VHD σας χρησιμοποιώντας το λογαριασμό χώρου αποθήκευσης Azure το `Add-AzureRmVhd` cmdlet ως εξής:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server, του SharePoint Server και Dynamics επίσης μπορούν να χρησιμοποιήσουν το εξασφάλισης αναβάθμισης λογισμικού παραχώρησης πολλαπλών αδειών χρήσης. Εξακολουθείτε να χρειάζεστε για να προετοιμάσετε την εικόνα του Windows Server κατά την εγκατάσταση την εφαρμογή στοιχεία και παρέχοντας αντίστοιχα κλειδιών άδειας χρήσης και, στη συνέχεια, αποστολή της εικόνας δίσκου σε Azure. Εξετάστε την κατάλληλη τεκμηρίωση για την εκτέλεση του Sysprep με την εφαρμογή σας, όπως [ζητήματα σχετικά με την εγκατάσταση του SQL Server με χρήση Sysprep](https://msdn.microsoft.com/library/ee210754.aspx) ή [Δημιουργία ενός του SharePoint Server 2016 ειδώλου αναφοράς (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx).

Μπορείτε επίσης να διαβάσετε περισσότερα σχετικά με την [Αποστολή του VHD Azure στη διαδικασία](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account).

> [AZURE.TIP] Σε αυτό το άρθρο εστιάζει στην ανάπτυξη ΣΠΣ διακομιστή των Windows. Μπορείτε επίσης να αναπτύξετε ΣΠΣ προγράμματος-πελάτη των Windows με τον ίδιο τρόπο. Στα παρακάτω παραδείγματα, αντικαθιστάτε `Server` με `Client` σωστά.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>Ανάπτυξη μια Εικονική μέσω του PowerShell γρήγορης εκκίνησης
Κατά την ανάπτυξη του Εικονική διακομιστή των Windows μέσω του PowerShell, που έχουν μια πρόσθετη παράμετρο για `-LicenseType`. Όταν έχετε ολοκληρώσει την αποστολή του σε Azure VHD, δημιουργείτε ένα νέο χρησιμοποιώντας Εικονική `New-AzureRmVM` και να καθορίσετε τον τύπο άδειας χρήσης ως εξής:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Μπορείτε να [διαβάσετε μια πιο λεπτομερείς οδηγίες σχετικά με την ανάπτυξη μια Εικονική στα Azure μέσω του PowerShell](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) παρακάτω ή διαβάστε ένα περισσότερο περιγραφικό Οδηγό στα διαφορετικά βήματα για να [δημιουργήσετε μια Εικονική Windows χρήση της διαχείρισης πόρων και PowerShell](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>Ανάπτυξη μια Εικονική μέσω από διαχειριστή πόρων
Εντός των προτύπων για τη διαχείριση πόρων, μια πρόσθετη παράμετρο για `licenseType` μπορεί να έχει καθοριστεί. Μπορείτε να διαβάσετε περισσότερα σχετικά με τη [σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα](../resource-group-authoring-templates.md). Όταν έχετε ολοκληρώσει την αποστολή του σε Azure VHD, επεξεργαστείτε πρότυπο διαχείρισης πόρων για να συμπεριλάβετε τον τύπο άδειας χρήσης ως μέρος της υπηρεσίας παροχής υπολογισμού και ανάπτυξη του προτύπου κανονικά:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Επαληθεύστε την Εικονική χρησιμοποιώντας το όφελος άδειας χρήσης
Αφού έχετε αναπτύξει σας Εικονική έως και οι δύο μέθοδοι ανάπτυξης του PowerShell ή διαχείριση πόρων, επιβεβαιώστε τον τύπο άδειας χρήσης με `Get-AzureRmVM` ως εξής:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Το αποτέλεσμα θα είναι παρόμοιο με το εξής:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Αυτό παρουσιάζει με την εξής Εικονική αναπτυχθεί χωρίς οφέλη χρήσης του Azure υβριδική παραχώρησης πολλαπλών αδειών χρήσης, όπως μια Εικονική αναπτυχθεί απευθείας από τη συλλογή Azure:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>Λεπτομερή ανάλυση του PowerShell

Τα παρακάτω λεπτομερή βήματα PowerShell εμφανίζουν μια πλήρη ανάπτυξη των μια Εικονική. Μπορείτε να διαβάσετε περισσότερα περιβάλλοντος ως προς την πραγματική cmdlet και διαφορετικά στοιχεία που δημιουργούνται στον [δημιουργήσετε μια Εικονική Windows χρήση της διαχείρισης πόρων και PowerShell](./virtual-machines-windows-ps-create.md). Μπορείτε να περιηγηθείτε στις δημιουργία σας ομάδα πόρων, το λογαριασμό χώρου αποθήκευσης και το εικονικό δίκτυο, στη συνέχεια, ορισμός σας Εικονική και τέλος δημιουργία Εικονική σας.
 
Πρώτα, με ασφαλή τρόπο λήψης διαπιστευτηρίων, ορίστε μια θέση και ένα όνομα ομάδας πόρων:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Δημιουργία μιας δημόσιας IP:

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Ορισμός σας υποδίκτυο, NIC και VNET:

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Ονομάστε το Εικονική και δημιουργήστε μια Εικονική ρύθμισης παραμέτρων:

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Προσδιορισμός του λειτουργικού Συστήματος:

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Προσθέστε το NIC η Εικονική:

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Ορισμός του λογαριασμού χώρου αποθήκευσης για να χρησιμοποιήσετε:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Αποστολή του VHD, κατάλληλα προετοιμασμένοι, και να επισυνάψετε το Εικονική για χρήση:

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Τέλος, δημιουργήσετε Εικονική σας και να ορίσετε τον τύπο άδειας χρήσης για να χρησιμοποιούν Azure υβριδική χρήση όφελος:

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Επόμενα βήματα

Διαβάστε περισσότερα σχετικά με τις [άδειες Azure υβριδική χρήση όφελος](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

Μάθετε περισσότερα σχετικά με τη [Χρήση προτύπων από διαχειριστή πόρων](../azure-resource-manager/resource-group-overview.md).

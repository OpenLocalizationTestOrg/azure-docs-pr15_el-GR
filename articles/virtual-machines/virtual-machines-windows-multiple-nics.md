<properties
   pageTitle="Δημιουργήστε μια Εικονική Windows με πολλά NIC | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια Εικονική Windows με πολλά NIC συνημμένη με τη χρήση προτύπων Azure PowerShell ή διαχείριση πόρων."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Δημιουργία μια Εικονική Windows με πολλά NIC
Μπορείτε να δημιουργήσετε μια εικονική μηχανή (Εικονική) στο Azure που έχει πολλές εικονικού διασυνδέσεις δικτύου (NIC) που έχουν επισυναφθεί σε αυτό. Ένα συνηθισμένο σενάριο που θα ήταν να έχει διαφορετικά δευτερεύοντα δίκτυα προσκηνίου και παρασκηνίου συνδεσιμότητας ή ένα δίκτυο αφοσιωμένη στο να μια λύση παρακολούθησης ή δημιουργίας αντιγράφων ασφαλείας. Σε αυτό το άρθρο παρέχει γρήγορη εντολές για να δημιουργήσετε μια Εικονική με πολλά NIC συνημμένη. Για λεπτομερείς πληροφορίες, συμπεριλαμβανομένου του τρόπου για τη δημιουργία πολλών NIC μέσα τη δική σας δεσμών ενεργειών του PowerShell, διαβάστε περισσότερα σχετικά με [την ανάπτυξη πολλών NIC ΣΠΣ](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Διαφορετικά [μεγέθη Εικονική](virtual-machines-windows-sizes.md) υποστήριξη διαφορετικό αριθμό NIC, επομένως το μέγεθος σας Εικονική αντίστοιχα.

>[AZURE.WARNING] Πρέπει να επισυνάψετε πολλά NIC όταν μπορείτε να δημιουργήσετε μια Εικονική - δεν μπορείτε να προσθέσετε NIC σε μια υπάρχουσα Εικονική. Μπορείτε να [δημιουργήσετε μια Εικονική με βάση το αρχικό εικονικού δίσκους](virtual-machines-windows-vhd-copy.md) και δημιουργήσετε πολλά NIC, όπως να αναπτύξετε την εικονική Μηχανή.

## <a name="create-core-resources"></a>Δημιουργία πόρων πυρήνα
Βεβαιωθείτε ότι έχετε τις [πιο πρόσφατες Azure PowerShell εγκατασταθεί και ρυθμιστεί](../powershell-install-configure.md). Συνδεθείτε στο λογαριασμό σας στο Azure:

```powershell
Login-AzureRmAccount
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `mystorageaccount`, και `myVM`.

Πρώτα, δημιουργήστε μια ομάδα πόρων. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUs` θέση:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Δημιουργία λογαριασμού χώρου αποθήκευσης για τη διατήρηση του ΣΠΣ. Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Δημιουργία εικονικού δικτύου και δευτερεύοντα δίκτυα
Ορισμός δύο δευτερευόντων δικτύων εικονικού δικτύου - μία για την κυκλοφορία προσκηνίου και μία για την κίνηση παρασκηνίου. Το παρακάτω παράδειγμα καθορίζει δύο δευτερευόντων δικτύων, με το όνομα `mySubnetFrontEnd` και `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Δημιουργήστε το εικονικό δίκτυο και δευτερεύοντα δίκτυα. Το ακόλουθο παράδειγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Δημιουργία πολλών NIC
Δημιουργήστε δύο NIC, επισύναψη μία NIC στο υποδίκτυο προσκηνίου και μία NIC στο υποδίκτυο παρασκηνίου. Το ακόλουθο παράδειγμα δημιουργεί δύο NIC, με το όνομα `myNic1` και `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Συνήθως δημιουργείτε επίσης μια [ομάδα ασφαλείας δικτύου](../virtual-network/virtual-networks-nsg.md) ή [μονάδα εξισορρόπησης φόρτου](../load-balancer/load-balancer-overview.md) για να διαχειρίζεστε και να διανέμετε κίνηση κατά μήκος του ΣΠΣ. Το άρθρο [πιο λεπτομερείς Εικονική πολλούς NIC](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) καθοδήγηση μέσω τη δημιουργία μιας ομάδας ασφαλείας δικτύου και εκχώρηση NIC.


## <a name="create-the-virtual-machine"></a>Δημιουργήστε την εικονική μηχανή
Ξεκινήστε τώρα για τη δημιουργία της ρύθμισης παραμέτρων Εικονική. Κάθε μεγέθους Εικονική έχει όριο για τον συνολικό αριθμό των NIC που μπορείτε να προσθέσετε μια Εικονική. Διαβάστε περισσότερα σχετικά με τα [μεγέθη των Windows Εικονική](virtual-machines-windows-sizes.md). 

Πρώτα, ορίστε τα διαπιστευτήριά σας Εικονική σε το `$cred` μεταβλητή ως εξής:

```powershell
$cred = Get-Credential
```

Το παρακάτω παράδειγμα καθορίζει μια Εικονική με το όνομα `myVM` και χρησιμοποιεί μια Εικονική μέγεθος που υποστηρίζει έως και δύο NIC (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Δημιουργήστε τα υπόλοιπα σας config Εικονική. Το παρακάτω παράδειγμα δημιουργεί μια Εικονική Windows Server 2012 R2:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Επισύναψη τα δύο NIC που δημιουργήσατε προηγουμένως:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Ρύθμιση παραμέτρων του χώρου αποθήκευσης και εικονικού δίσκου για το νέο Εικονική:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Τέλος, δημιουργήστε μια Εικονική:

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Δημιουργία πολλών NIC με τη χρήση προτύπων από διαχειριστή πόρων
Azure πρότυπα διαχείρισης πόρων χρησιμοποιούν δηλωτικό JSON αρχείων για να ορίσετε το περιβάλλον σας. Μπορείτε να διαβάσετε μια [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md). Πρότυπα διαχείρισης πόρων παρέχει κάποιο τρόπο για να δημιουργήσετε πολλές παρουσίες ενός πόρου κατά την ανάπτυξη, όπως τη δημιουργία πολλών NIC. Μπορείτε να χρησιμοποιήσετε *Αντιγραφή* για να καθορίσετε τον αριθμό των εμφανίσεων για να δημιουργήσετε:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Διαβάστε περισσότερα σχετικά με [τη δημιουργία πολλών παρουσιών με χρήση *αντιγραφής*](../resource-group-create-multiple.md). 

Μπορείτε επίσης να χρησιμοποιήσετε ένα `copyIndex()` για να προσαρτήσετε, στη συνέχεια, έναν αριθμό σε ένα όνομα πόρου, το οποίο σας επιτρέπει να δημιουργήσετε `myNic1`, `MyNic2`κ.λπ. Ακολουθεί ένα παράδειγμα προσαρτώντας την τιμή ευρετήριο:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Μπορείτε να διαβάσετε μια ολοκληρωμένη παράδειγμα τη δημιουργία [πολλών NIC με τη χρήση προτύπων από διαχειριστή πόρων](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Επόμενα βήματα
Βεβαιωθείτε ότι για να δείτε [τα μεγέθη των Windows Εικονική](virtual-machines-windows-sizes.md) όταν προσπαθείτε να δημιουργήσετε μια Εικονική με πολλά NIC. Προσέξτε να τον μέγιστο αριθμό NIC υποστηρίζει κάθε μεγέθους Εικονική. 

Να θυμάστε ότι δεν μπορείτε να προσθέσετε επιπλέον NIC σε μια υπάρχουσα Εικονική, πρέπει να δημιουργήσετε όλα τα NIC κατά την ανάπτυξη του Εικονική. Να χειριστείτε κατά το σχεδιασμό αναπτύξεις σας για να βεβαιωθείτε ότι έχετε όλα τη συνδεσιμότητα δικτύου απαιτείται από την αρχή.
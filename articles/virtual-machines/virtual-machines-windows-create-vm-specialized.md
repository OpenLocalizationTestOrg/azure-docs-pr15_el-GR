<properties
    pageTitle="Δημιουργήστε ένα αντίγραφο του σας Εικονική Windows | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του σας εξειδικευμένες Εικονική Azure με Windows, στο μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Δημιουργήστε μια Εικονική από έναν εξειδικευμένο VHD

Δημιουργήστε μια νέα Εικονική επισυνάπτοντας VHD εξειδικευμένες ως ο δίσκος λειτουργικού Συστήματος με χρήση του Powershell. VHD εξειδικευμένες διατηρεί τους λογαριασμούς χρηστών, εφαρμογών και άλλων δεδομένων κατάσταση από το αρχικό Εικονική. 

Εάν θέλετε να δημιουργήσετε μια Εικονική από έναν γενικευμένη VHD, ανατρέξτε στο θέμα [Δημιουργία μια Εικονική από μια εικόνα γενικευμένη VHD](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Δημιουργία των υποδικτύου και vNet

Δημιουργία της vNet και υποδικτύου [εικονικού δικτύου](../virtual-network/virtual-networks-overview.md).

1. Δημιουργήστε το υποδίκτυο. Αυτό το παράδειγμα δημιουργεί ένα υποδίκτυο με το όνομα **mySubNet**, στο την ομάδα πόρων **myResourceGroup**, και ορίζει το πρόθεμα υποδικτύου διευθύνσεων **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Δημιουργήστε το vNet. Αυτό το παράδειγμα ορίζει το όνομα εικονικού δικτύου για να **myVnetName**, τη θέση στην οποία **Δυτική ΗΠΑ**και το πρόθεμα διεύθυνσης για την εικονικού δικτύου **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Δημιουργήστε μια δημόσια διεύθυνση IP και NIC

Για να ενεργοποιήσετε την επικοινωνία με την εικονική μηχανή στο εικονικό δίκτυο, χρειάζεστε μια [δημόσια διεύθυνση IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) και μια διασύνδεση δικτύου.

1. Δημιουργήστε τη δημόσια διευθύνσεων IP. Σε αυτό το παράδειγμα, τη δημόσια διεύθυνση IP, το όνομα έχει οριστεί σε **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Δημιουργία της σύνδεσης NIC. Σε αυτό το παράδειγμα, το όνομα του NIC έχει οριστεί σε **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Δημιουργήστε την ομάδα ασφαλείας δικτύου και έναν κανόνα RDP

Για να μπορέσετε να συνδεθείτε με το Εικονική χρησιμοποιώντας RDP, πρέπει να έχετε έναν κανόνα ασφάλειας που επιτρέπει την πρόσβαση RDP στη θύρα 3389. Επειδή VHD για τη νέα Εικονική που δημιουργήθηκε από μια υπάρχουσα εξειδικευμένες Εικονική, αφού δημιουργηθεί η Εικονική μπορείτε να χρησιμοποιήσετε έναν υπάρχοντα λογαριασμό από την εικονική μηχανή προέλευσης που είχαν δικαιωμάτων για να συνδεθείτε χρησιμοποιώντας RDP.

Αυτό το παράδειγμα ορίζει το όνομα NSG **myNsg** και το όνομα του κανόνα RDP να **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Για περισσότερες πληροφορίες σχετικά με τα τελικά σημεία και NSG κανόνες, ανατρέξτε στο θέμα [Άνοιγμα θυρών σε μια Εικονική στα Azure χρησιμοποιώντας το PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Δημιουργία της ρύθμισης παραμέτρων Εικονική

Ρυθμίστε τις παραμέτρους Εικονική για να επισυνάψετε το αντιγραμμένο VHD ως OS VHD.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Εάν έχετε δίσκων δεδομένων που πρέπει να για να το επισυνάψετε η Εικονική, επίσης θα πρέπει να προσθέσετε τα εξής: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Τα δεδομένα και τις διευθύνσεις URL δίσκου λειτουργικό σύστημα είναι κάπως έτσι: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Μπορείτε να τη βρείτε στην πύλη, μεταβαίνοντας στο κοντέινερ χώρου αποθήκευσης προορισμού, κάνοντας κλικ στην επιλογή το λειτουργικό σύστημα ή τα δεδομένα VHD που έχει αντιγραφεί και, στη συνέχεια, να αντιγράψετε τα περιεχόμενα της διεύθυνσης URL.


## <a name="create-the-vm"></a>Δημιουργήστε την εικονική Μηχανή

Δημιουργήστε την εικονική Μηχανή χρησιμοποιώντας τις ρυθμίσεις παραμέτρων που θα σας μόλις δημιουργήσατε.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Εάν αυτή η εντολή ολοκληρώθηκε με επιτυχία, θα δείτε εξόδου ως εξής:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Βεβαιωθείτε ότι η Εικονική δημιουργήθηκε 
 
Θα πρέπει να δείτε το πρόσφατα δημιουργημένο Εικονική είτε στην [πύλη του Azure](https://portal.azure.com), στην ενότητα **Αναζήτηση** > **εικονικές μηχανές**, ή χρησιμοποιώντας τις παρακάτω εντολές του PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να συνδεθείτε με το νέο εικονική μηχανή, αναζητήστε την εικονική Μηχανή στην [πύλη](https://portal.azure.com), κάντε κλικ στην επιλογή **σύνδεση**και ανοίξτε το αρχείο απομακρυσμένης επιφάνειας εργασίας RDP. Χρησιμοποιήστε τα διαπιστευτήρια λογαριασμού από την αρχική εικονική μηχανή σας για να συνδεθείτε στον υπολογιστή σας νέα εικονική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να συνδεθείτε και να συνδεθείτε με μια Azure εικονική μηχανή με Windows](virtual-machines-windows-connect-logon.md).








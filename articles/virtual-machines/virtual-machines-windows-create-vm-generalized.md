<properties
    pageTitle="Δημιουργία Εικονική από VHD γενικευμένη | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια εικονική μηχανή Windows από μια γενική εικόνα VHD χρησιμοποιώντας το Azure PowerShell, στο μοντέλο ανάπτυξης διαχείρισης πόρων."
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
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Δημιουργήστε μια Εικονική από μια εικόνα γενικευμένη VHD

Μια εικόνα γενικευμένη VHD είχε όλων των πληροφοριών σας προσωπικό λογαριασμό καταργηθεί χρησιμοποιώντας [Sysprep](virtual-machines-windows-generalize-vhd.md). Μπορείτε να δημιουργήσετε έναν γενικευμένη VHD εκτελώντας το Sysprep σε μια Εικονική εσωτερικής εγκατάστασης, στη συνέχεια, [Αποστολή VHD να Azure](virtual-machines-windows-upload-image.md), ή με την εκτέλεση του Sysprep σε μια υπάρχουσα Εικονική Azure και, στη συνέχεια, [αντιγράφοντας τον VHD](virtual-machines-windows-vhd-copy.md).

Εάν θέλετε να δημιουργήσετε μια Εικονική από έναν εξειδικευμένο VHD, ανατρέξτε στο θέμα [Δημιουργία μια Εικονική από έναν εξειδικευμένο VHD](virtual-machines-windows-create-vm-specialized.md).

Ο πιο γρήγορος τρόπος για να δημιουργήσετε μια Εικονική από VHD γενικευμένη είναι να χρησιμοποιήσετε [γρήγορης εκκίνησης προτύπου] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εάν σκοπεύετε να χρησιμοποιήσετε έναν VHD που έχουν αποσταλεί από μια Εικονική εσωτερικής εγκατάστασης, όπως μια που έχει δημιουργηθεί χρησιμοποιώντας το Hyper-V, θα πρέπει να βεβαιωθείτε ότι ακολουθήσατε τις οδηγίες στο θέμα [Προετοιμασία VHD των Windows για να αποστείλετε στο Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 

Το απεσταλμένο VHD και υπάρχοντα VHD Εικονική Azure πρέπει να είναι γενίκευση πριν μπορέσετε να δημιουργήσετε μια Εικονική χρησιμοποιώντας αυτήν τη μέθοδο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Generalize μια εικονική μηχανή Windows χρησιμοποιώντας το Sysprep](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>Ορίστε το URI του VHD

Το URI για τον VHD για να χρησιμοποιήσετε έχει τη μορφή: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Σε αυτό το παράδειγμα VHD με το όνομα **myVHD** βρίσκεται στο λογαριασμό του χώρου αποθήκευσης **mystorageaccount** στο το κοντέινερ **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Δημιουργήστε ένα εικονικό δίκτυο

Δημιουργία της vNet και υποδικτύου [εικονικού δικτύου](../virtual-network/virtual-networks-overview.md).


1. Δημιουργήστε το υποδίκτυο. Το παρακάτω δείγμα δημιουργεί ένα δευτερεύον δίκτυο με το όνομα **mySubnet** στο την ομάδα πόρων **myResourceGroup** με το πρόθεμα διεύθυνση **10.0.0.0/24**.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Δημιουργήστε το εικονικό δίκτυο. Το παρακάτω δείγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα **myVnet** στη θέση **Δυτική ΗΠΑ** με το πρόθεμα τη διεύθυνση της **10.0.0.0/16**.  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Δημιουργήστε μια δημόσια διασύνδεση δικτύου και τη διεύθυνση IP

Για να ενεργοποιήσετε την επικοινωνία με την εικονική μηχανή στο εικονικό δίκτυο, χρειάζεστε μια [δημόσια διεύθυνση IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) και μια διασύνδεση δικτύου.

1. Δημιουργήστε μια δημόσια διεύθυνση IP. Αυτό το παράδειγμα δημιουργεί μια δημόσια διεύθυνση IP με το όνομα **myPip**. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. Δημιουργία της σύνδεσης NIC. Αυτό το παράδειγμα δημιουργεί μια NIC με το όνομα **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Δημιουργήστε την ομάδα ασφαλείας δικτύου και έναν κανόνα RDP

Για να μπορέσετε να συνδεθείτε με το Εικονική χρησιμοποιώντας RDP, πρέπει να υπάρχει ένας κανόνας ασφαλείας που επιτρέπει την πρόσβαση RDP στη θύρα 3389. 

Αυτό το παράδειγμα δημιουργεί μια NSG με το όνομα **myNsg** που περιέχει έναν κανόνα που ονομάζεται **myRdpRule** που επιτρέπει την κυκλοφορία RDP μέσω της θύρας 3389. Για περισσότερες πληροφορίες σχετικά με το NSGs, ανατρέξτε στο θέμα [Άνοιγμα θυρών σε μια Εικονική στα Azure χρησιμοποιώντας το PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Δημιουργήστε μια μεταβλητή για το εικονικό δίκτυο

Δημιουργήστε μια μεταβλητή για την ολοκληρωμένη εικονικού δικτύου. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>Δημιουργήστε την εικονική Μηχανή

Η ακόλουθη δέσμη ενεργειών PowerShell δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους εικονική μηχανή και να χρησιμοποιήσετε την εικόνα Εικονική αποσταλεί ως προέλευση για τη νέα εγκατάσταση.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Βεβαιωθείτε ότι η Εικονική δημιουργήθηκε 

Όταν ολοκληρωθεί η εγκατάσταση, θα πρέπει να βλέπετε το που έχουν δημιουργηθεί πρόσφατα Εικονική στην [πύλη του Azure](https://portal.azure.com) στην ενότητα **Αναζήτηση** > **εικονικές μηχανές**, ή χρησιμοποιώντας τις παρακάτω εντολές του PowerShell:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να διαχειριστείτε το νέο εικονική μηχανή με Azure PowerShell, ανατρέξτε στο θέμα [Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell](virtual-machines-windows-ps-manage.md).



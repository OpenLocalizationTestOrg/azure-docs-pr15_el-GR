<properties
    pageTitle="Δημιουργήστε μια Εικονική Azure χρησιμοποιώντας το PowerShell | Microsoft Azure"
    description="Χρήση του PowerShell Azure και διαχείριση πόρων Azure για να δημιουργήσετε εύκολα μια νέα Εικονική εκτελούν τον Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Δημιουργήστε μια Εικονική Windows χρήση της διαχείρισης πόρων και PowerShell

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να δημιουργήσετε γρήγορα μια εικονική μηχανή Azure που εκτελούν Windows Server και τους πόρους που χρειάζεται η χρήση της [Διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md) και PowerShell. 

Απαιτούνται όλα τα βήματα σε αυτό το άρθρο για να δημιουργήσετε μια εικονική μηχανή και θα πρέπει να μπορεί να χρειαστεί περίπου 30 λεπτά για να κάνετε τα εξής βήματα. Αντικαταστήστε το παράδειγμα τιμές παραμέτρων στις εντολές με ονόματα που έχει νόημα για το περιβάλλον σας.

## <a name="step-1-install-azure-powershell"></a>Βήμα 1: Εγκατάσταση του Azure PowerShell

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.
        
## <a name="step-2-create-a-resource-group"></a>Βήμα 2: Δημιουργία μιας ομάδας πόρων

Όλοι οι πόροι πρέπει να περιέχονται σε μια ομάδα πόρων, επομένως, σας επιτρέπει να δημιουργήσετε πρώτα αυτήν την ενέργεια.  

1. Λάβετε μια λίστα με τις διαθέσιμες θέσεις όπου μπορεί να δημιουργηθεί πόρους.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Ορίστε τη θέση για τους πόρους. Αυτή η εντολή ορίζει τη θέση για να **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Δημιουργία μιας ομάδας πόρων. Αυτή η εντολή δημιουργεί την ομάδα των πόρων με το όνομα **myResourceGroup** στη θέση που καθορίζετε.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Βήμα 3: Δημιουργία λογαριασμού χώρου αποθήκευσης

Ένα [λογαριασμό χώρου αποθήκευσης](../storage/storage-introduction.md) είναι απαραίτητη για την αποθήκευση του εικονικού σκληρού δίσκου που χρησιμοποιείται από την εικονική μηχανή που δημιουργείτε. Ονόματα λογαριασμών χώρου αποθήκευσης πρέπει να είναι μεταξύ 3 και 24 χαρακτήρες και μπορεί να περιέχει αριθμούς και πεζών γραμμάτων.

1. Ελέγξτε το όνομα λογαριασμού χώρου αποθήκευσης για τη μοναδικότητα. Αυτή η εντολή ελέγχει το όνομα **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Εάν αυτή η εντολή επιστρέφει την **τιμή True**, το προτεινόμενο όνομα είναι μοναδικό μέσα σε Azure. 
    
2. Τώρα, δημιουργήστε το λογαριασμό χώρου αποθήκευσης.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Βήμα 4: Δημιουργία ενός εικονικού δικτύου

Όλες οι εικονικές μηχανές είναι μέρος ενός [εικονικού δικτύου](../virtual-network/virtual-networks-overview.md).

1. Δημιουργήστε ένα υποδίκτυο για το εικονικό δίκτυο. Αυτή η εντολή δημιουργεί ένα δευτερεύον δίκτυο με το όνομα **mySubnet** με μια διεύθυνση πρόθεμα 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Τώρα, δημιουργήστε το εικονικό δίκτυο. Αυτή η εντολή δημιουργεί ένα εικονικό δίκτυο με το όνομα **myVnet** χρησιμοποιώντας το υποδίκτυο που δημιουργήσατε και ένα πρόθεμα διεύθυνσης της **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Βήμα 5: Δημιουργία μιας δημόσιας διασύνδεση δικτύου και τη διεύθυνση IP

Για να ενεργοποιήσετε την επικοινωνία με την εικονική μηχανή στο εικονικό δίκτυο, χρειάζεστε μια [δημόσια διεύθυνση IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) και μια διασύνδεση δικτύου.

1. Δημιουργία στη δημόσια διεύθυνση IP. Αυτή η εντολή δημιουργεί μια δημόσια διεύθυνση IP με το όνομα **myPublicIp** με μια μέθοδο εκχώρησης **δυναμικής**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Δημιουργήστε το περιβάλλον εργασίας δικτύου. Αυτή η εντολή δημιουργεί μια διασύνδεση δικτύου με το όνομα **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Βήμα 6: Δημιουργήστε μια εικονική μηχανή

Τώρα που έχετε όλα τα τμήματα στη θέση, είναι ώρα για να δημιουργήσετε την εικονική μηχανή.

1. Εκτέλεση αυτής της εντολής για να ορίσετε το όνομα λογαριασμού του διαχειριστή και τον κωδικό πρόσβασης για την εικονική μηχανή.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Ο κωδικός πρόσβασης πρέπει να στο 12-123 χαρακτήρες και να έχετε τουλάχιστον μία πεζούς χαρακτήρες, ένα χαρακτήρα με κεφαλαία γράμματα, έναν αριθμό και ένα ειδικό χαρακτήρα. 
        
2. Δημιουργήστε το αντικείμενο ρύθμισης παραμέτρων για την εικονική μηχανή. Αυτή η εντολή δημιουργεί ένα αντικείμενο ρύθμισης παραμέτρων με το όνομα **myVmConfig** που καθορίζει το όνομα του η Εικονική και το μέγεθος του η Εικονική.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές στο Azure](virtual-machines-windows-sizes.md) για μια λίστα με τις διαθέσιμες μεγέθη για μια εικονική μηχανή.
    
3. Ρύθμιση παραμέτρων λειτουργικού συστήματος για την εικονική Μηχανή. Αυτή η εντολή ορίζει το όνομα του υπολογιστή, τύπος λειτουργικό σύστημα και διαπιστευτήρια λογαριασμού για την εικονική Μηχανή.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Καθορίστε την εικόνα που θα χρησιμοποιήσετε για την προμήθεια του Εικονική. Αυτή η εντολή ορίζει την εικόνα Windows Server για να χρησιμοποιήσετε για την εικονική Μηχανή. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Για περισσότερες πληροφορίες σχετικά με την επιλογή εικόνες που θα χρησιμοποιήσετε, ανατρέξτε στο θέμα [Περιήγηση και επιλογή Windows εικονική μηχανή εικόνων στο Azure με το PowerShell ή το CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. Προσθέστε το περιβάλλον εργασίας δικτύου που δημιουργήσατε στη ρύθμιση παραμέτρων.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Ορίστε το όνομα και τη θέση του σκληρού δίσκου Εικονική. Σε ένα κοντέινερ είναι αποθηκευμένο το αρχείο εικονικού σκληρού δίσκου. Αυτή η εντολή δημιουργεί το δίσκο σε ένα κοντέινερ που ονομάζεται **vhds/WindowsVMosDisk.vhd** στο λογαριασμό χώρου αποθήκευσης που έχετε δημιουργήσει.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Προσθέστε πληροφορίες δίσκου του λειτουργικού συστήματος με τη ρύθμιση παραμέτρων Εικονική. Αντικαταστήστε την τιμή του **$diskName** με ένα όνομα για το λειτουργικό σύστημα δίσκο. Δημιουργήστε τη μεταβλητή και προσθέστε τις πληροφορίες δίσκου στη ρύθμιση παραμέτρων.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Τέλος, δημιουργήστε την εικονική μηχανή.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Επόμενα βήματα

- Εάν υπάρχουν προβλήματα με την ανάπτυξη, ένα επόμενο βήμα σας είναι να κοιτάξετε [αναπτύξεις ομάδα πόρων αντιμετώπιση προβλημάτων με την πύλη Azure](../resource-manager-troubleshoot-deployments-portal.md)
- Μάθετε πώς μπορείτε να διαχειριστείτε την εικονική μηχανή που δημιουργήσατε ανατρέχοντας στο θέμα [Διαχείριση εικονικές μηχανές χρήση της διαχείρισης πόρων Azure και PowerShell](virtual-machines-windows-ps-manage.md).
- Εκμεταλλευτείτε χρησιμοποιώντας ένα πρότυπο για να δημιουργήσετε μια εικονική μηχανή χρησιμοποιώντας τις πληροφορίες στο θέμα [Δημιουργία μια εικονική μηχανή Windows με ένα πρότυπο από διαχειριστή πόρων](virtual-machines-windows-ps-template.md)

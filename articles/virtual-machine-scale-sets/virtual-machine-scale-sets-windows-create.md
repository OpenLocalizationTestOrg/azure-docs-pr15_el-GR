<properties
    pageTitle="Δημιουργία ενός συνόλου κλίμακα εικονική μηχανή χρησιμοποιώντας το PowerShell | Microsoft Azure"
    description="Δημιουργία ενός συνόλου κλίμακα εικονική μηχανή με χρήση του PowerShell"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Δημιουργήστε μια κλίμακα εικονική μηχανή Windows συνόλου με χρήση του PowerShell Azure

Μια προσέγγιση συμπλήρωσης-σε-το-κενά για να δημιουργήσετε ένα σύνολο κλίμακα Azure εικονική μηχανή, ακολουθήστε τα παρακάτω βήματα. Ανατρέξτε στο θέμα [Επισκόπηση ορίζει κλίμακα εικονική μηχανή](virtual-machine-scale-sets-overview.md) για να μάθετε περισσότερα σχετικά με τα σύνολα κλίμακα.

Θα χρειαστεί περίπου 30 λεπτά για να εκτελέσετε τα βήματα σε αυτό το άρθρο.

## <a name="step-1-install-azure-powershell"></a>Βήμα 1: Εγκατάσταση του Azure PowerShell

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="step-2-create-resources"></a>Βήμα 2: Δημιουργία πόρων

Δημιουργήστε τους πόρους που απαιτούνται για το νέο σύνολο κλίμακα.

### <a name="resource-group"></a>Ομάδα πόρων

Ένα σύνολο κλίμακα εικονική μηχανή πρέπει να περιέχονται σε μια ομάδα πόρων.

1. Λάβετε μια λίστα με τις διαθέσιμες θέσεις όπου μπορείτε να τοποθετήσετε τους πόρους:

        Get-AzureLocation | Sort Name | Select Name

2. Επιλέξτε μια θέση που είναι καλύτερη για εσάς, αντικαταστήστε την τιμή του **$locName** με αυτό το όνομα θέσης και, στη συνέχεια, δημιουργήστε τη μεταβλητή:

        $locName = "location name from the list, such as Central US"

3. Αντικαταστήστε την τιμή του **$rgName** με το όνομα που θέλετε να χρησιμοποιήσετε για τη νέα ομάδα πόρων και, στη συνέχεια, δημιουργήστε τη μεταβλητή: 

        $rgName = "resource group name"
        
4. Δημιουργήστε την ομάδα των πόρων:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Το λογαριασμό χώρου αποθήκευσης

Ένα λογαριασμό του χώρου αποθήκευσης χρησιμοποιείται από μια εικονική μηχανή για να αποθηκεύσετε το δίσκο του λειτουργικού συστήματος και διαγνωστικών δεδομένα που χρησιμοποιούνται για την κλιμάκωση. Όταν είναι δυνατό, είναι βέλτιστη πρακτική να έχετε ένα λογαριασμό χώρου αποθήκευσης για κάθε εικονική μηχανή που δημιουργήσατε σε ένα σύνολο κλίμακα. Εάν δεν είναι δυνατό, πρόγραμμα για λιγότερα από 20 ΣΠΣ ανά λογαριασμού χώρου αποθήκευσης. Το παράδειγμα σε αυτό το άρθρο εμφανίζει τρεις λογαριασμούς χώρου αποθήκευσης που δημιουργούνται για τρεις εικονικές μηχανές.

1. Αντικαταστήστε την τιμή του **$saName** με ένα όνομα για το λογαριασμό χώρου αποθήκευσης. Ελέγξτε το όνομα για τη μοναδικότητα. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Εάν η απάντηση είναι **Αληθής**, το προτεινόμενο όνομα είναι μοναδικό.

3. Αντικαταστήστε την τιμή του **$saType** με τον τύπο του λογαριασμού χώρου αποθήκευσης και, στη συνέχεια, δημιουργήστε τη μεταβλητή:  

        $saType = "storage account type"
        
    Οι πιθανές τιμές είναι: Standard_LRS, Standard_GRS, Standard_RAGRS ή Premium_LRS.
        
4. Δημιουργήστε το λογαριασμό:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Επαναλάβετε τα βήματα 1 έως 4 για να δημιουργήσετε τρεις λογαριασμούς χώρου αποθήκευσης, για παράδειγμα myst1, myst2 και myst3.

### <a name="virtual-network"></a>Εικονικό δίκτυο

Ένα εικονικό δίκτυο είναι απαραίτητη για τις εικονικές μηχανές του συνόλου κλίμακα.

1. Αντικαταστήστε την τιμή του **$subnetName** με το όνομα που θέλετε να χρησιμοποιήσετε για το υποδίκτυο στο εικονικό δίκτυο και, στη συνέχεια, δημιουργήστε τη μεταβλητή: 

        $subnetName = "subnet name"
        
2. Δημιουργήστε παραμέτρους υποδικτύου του:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Το πρόθεμα διεύθυνσης μπορεί να διαφέρουν στο δίκτυο εικονικού.

3. Αντικαταστήστε την τιμή του **$netName** με το όνομα που θέλετε να χρησιμοποιήσετε για το εικονικό δίκτυο και, στη συνέχεια, δημιουργήστε τη μεταβλητή: 

        $netName = "virtual network name"
        
4. Δημιουργήστε το εικονικό δίκτυο:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Ρύθμιση παραμέτρων για το σύνολο κλίμακας

Έχετε όλους τους πόρους που χρειάζεστε για την κλίμακα ρύθμιση των παραμέτρων, οπότε ας δημιουργήσουμε το.  

1. Αντικαταστήστε την τιμή του **$ipName** με το όνομα που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων IP και, στη συνέχεια, δημιουργήστε τη μεταβλητή: 

        $ipName = "IP configuration name"
        
2. Δημιουργήστε τη ρύθμιση παραμέτρων IP:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Αντικαταστήστε την τιμή του **$vmssConfig** με το όνομα που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων σύνολο κλίμακα και, στη συνέχεια, δημιουργήστε τη μεταβλητή:   

        $vmssConfig = "Scale set configuration name"
        
3. Δημιουργήστε τη ρύθμιση παραμέτρων για το σύνολο κλίμακα:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Αυτό το παράδειγμα εμφανίζει μια κλίμακα ορίστε που δημιουργείται με τρεις εικονικές μηχανές. Για περισσότερες πληροφορίες σχετικά με την η δυναμικότητα των συνόλων κλίμακας, ανατρέξτε στο θέμα [Επισκόπηση ορίζει εικονική μηχανή κλίμακα](virtual-machine-scale-sets-overview.md) . Αυτό το βήμα περιλαμβάνει επίσης τη ρύθμιση του μεγέθους (αναφέρεται ως SkuName) τις εικονικές μηχανές του συνόλου. Για να βρείτε ένα μέγεθος που ικανοποιεί τις ανάγκες σας, εξετάστε [μεγέθη για εικονικές μηχανές](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Προσθέστε τη ρύθμιση παραμέτρων δικτύου του περιβάλλοντος εργασίας στη ρύθμιση παραμέτρων σύνολο κλίμακα:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Λειτουργικό σύστημα προφίλ

1. Αντικαταστήστε την τιμή του **$computerName** με το πρόθεμα όνομα υπολογιστή που θέλετε να χρησιμοποιήσετε και, στη συνέχεια, δημιουργήστε τη μεταβλητή: 

        $computerName = "computer name prefix"
        
2. Αντικαταστήστε την τιμή του **$adminName** το όνομα του λογαριασμού του διαχειριστή στο τις εικονικές μηχανές και, στη συνέχεια, δημιουργήστε τη μεταβλητή:

        $adminName = "administrator account name"
        
3. Αντικαταστήστε την τιμή του **$adminPassword** με τον κωδικό πρόσβασης λογαριασμού και, στη συνέχεια, δημιουργήστε τη μεταβλητή:

        $adminPassword = "password for administrator accounts"
        
4. Δημιουργήστε το προφίλ το λειτουργικό σύστημα:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Χώρος αποθήκευσης προφίλ

1. Αντικαταστήστε την τιμή του **$storageProfile** με το όνομα που θέλετε να χρησιμοποιήσετε για το χώρο αποθήκευσης προφίλ και, στη συνέχεια, δημιουργήστε τη μεταβλητή:  

        $storageProfile = "storage profile name"
        
2. Δημιουργήστε τις μεταβλητές που ορίζουν την εικόνα για να χρησιμοποιήσετε:  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Για να βρείτε τις πληροφορίες σχετικά με άλλες εικόνες που θα χρησιμοποιήσετε, εξετάστε [Περιήγηση και επιλογή Azure εικονική μηχανή εικόνων με το Windows PowerShell και το Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Αντικαταστήστε την τιμή του **$vhdContainers** με μια λίστα που περιέχει τις διαδρομές όπου είναι αποθηκευμένο το εικονικών δίσκων σκληρό, όπως "https://mystorage.blob.core.windows.net/vhds", και, στη συνέχεια, δημιουργήστε τη μεταβλητή:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Δημιουργήστε το χώρο αποθήκευσης προφίλ:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Ορισμός κλίμακα εικονική μηχανή

Τέλος, μπορείτε να δημιουργήσετε το σύνολο κλίμακα.

1. Αντικαταστήστε την τιμή του **$vmssName** με το όνομα του συνόλου εικονική μηχανή κλίμακα και, στη συνέχεια, δημιουργήστε τη μεταβλητή:

        $vmssName = "scale set name"
        
2. Δημιουργία συνόλου κλίμακα:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Θα πρέπει να δείτε κάτι παρόμοιο με αυτό το παράδειγμα που εμφανίζει μια επιτυχημένη ανάπτυξη:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Βήμα 3: Εξερευνήστε τους πόρους

Χρησιμοποιήστε αυτούς τους πόρους για να εξερευνήσετε το σύνολο κλίμακα εικονικό μηχάνημα που δημιουργήσατε:

- Azure - ένα περιορισμένο χρονικό πληροφορίες είναι διαθέσιμη η πύλη με την πύλη.
- [Εξερεύνηση των πόρων azure](https://resources.azure.com/) - αυτό το εργαλείο είναι η καλύτερη για την Εξερεύνηση την τρέχουσα κατάσταση του συνόλου κλίμακα.
- Azure PowerShell - Χρησιμοποιήστε αυτή την εντολή για να λάβετε πληροφορίες:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Επόμενα βήματα

- Διαχείριση του συνόλου κλίμακας που μόλις δημιουργήσατε χρησιμοποιώντας τις πληροφορίες σε [Διαχείριση εικονικές μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή](virtual-machine-scale-sets-windows-manage.md)
- Εξετάστε το ενδεχόμενο ρύθμισης αυτόματης κλίμακας σας κλίμακας ρύθμιση χρησιμοποιώντας πληροφορίες στα [σύνολα αυτόματης κλιμάκωσης και εικονική μηχανή κλίμακα](virtual-machine-scale-sets-autoscale-overview.md)
- Μάθετε περισσότερα σχετικά με κατακόρυφη κλιμάκωση ανατρέχοντας [Κατακόρυφη autoscale με σύνολα κλίμακα εικονική μηχανή](virtual-machine-scale-sets-vertical-scale-reprovision.md)

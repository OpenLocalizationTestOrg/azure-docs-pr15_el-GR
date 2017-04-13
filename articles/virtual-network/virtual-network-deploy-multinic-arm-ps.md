<properties
   pageTitle="Ανάπτυξη πολλούς NIC ΣΠΣ χρησιμοποιώντας το PowerShell στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ NIC πολλούς χρησιμοποιώντας το PowerShell στη Διαχείριση πόρων"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-using-powershell"></a>Ανάπτυξη πολλούς NIC ΣΠΣ χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Προς το παρόν, δεν μπορείτε να έχετε ΣΠΣ με ένα μεμονωμένο NIC και ΣΠΣ με πολλά NIC στο ίδιο σύνολο διαθεσιμότητα. Επομένως, πρέπει να υλοποιήσετε τους διακομιστές παρασκηνίου σε μια ομάδα πόρων διαφορετικό από όλα τα άλλα στοιχεία. Τα παρακάτω βήματα Χρησιμοποιήστε μια ομάδα πόρων με το όνομα *IaaSStory* για την ομάδα κύριο πόρων και *IaaSStory παρασκηνίου* για τους διακομιστές παρασκηνίου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναπτύξετε τους διακομιστές παρασκηνίου, πρέπει να αναπτύξετε την ομάδα των πόρων κύριο με όλους τους πόρους που είναι απαραίτητο για αυτό το σενάριο. Για να αναπτύξετε αυτούς τους πόρους, ακολουθήστε τα παρακάτω βήματα.

1. Μεταβείτε [στη](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)σελίδα προτύπου.
2. Στη σελίδα του προτύπου, στα δεξιά της **γονικής ομάδας πόρων**, κάντε κλικ στην επιλογή **Ανάπτυξη να Azure**.
3. Εάν είναι απαραίτητο, αλλάξτε τις τιμές παραμέτρων και, στη συνέχεια, ακολουθήστε τα βήματα στην πύλη του Azure προεπισκόπηση για να αναπτύξετε την ομάδα των πόρων.

> [AZURE.IMPORTANT] Βεβαιωθείτε ότι τα ονόματα λογαριασμού χώρου αποθήκευσης είναι μοναδικό. Δεν μπορείτε να έχετε ονόματα λογαριασμών διπλότυπων χώρου αποθήκευσης στο Azure.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Ανάπτυξη του παρασκηνίου ΣΠΣ

Το ΣΠΣ παρασκηνίου εξαρτώνται από τη δημιουργία των πόρων που παρατίθενται παρακάτω.

- **Λογαριασμό του χώρου αποθήκευσης για δίσκων δεδομένων**. Για καλύτερες επιδόσεις, των δίσκων δεδομένων στους διακομιστές βάσης δεδομένων θα χρησιμοποιήσει τεχνολογία συμπαγές κατάσταση μονάδα δίσκου (SSD), η οποία απαιτεί ένα λογαριασμό του χώρου αποθήκευσης premium. Βεβαιωθείτε ότι η θέση Azure αναπτύξετε για την υποστήριξη του χώρου αποθήκευσης premium.
- **NIC**. Κάθε Εικονική θα έχει δύο NIC, μία για πρόσβαση στη βάση δεδομένων, και μία για τη διαχείριση.
- **Ορισμός διαθεσιμότητας**. Θα προστεθούν όλους τους διακομιστές βάσης δεδομένων σε ένα μεμονωμένο διαθεσιμότητα Ορισμός, για να βεβαιωθείτε ότι τουλάχιστον ένα από τα ΣΠΣ είναι προς τα επάνω και να εκτελείται κατά τη συντήρηση.  

### <a name="step-1---start-your-script"></a>Βήμα 1 - Έναρξη της δέσμης ενεργειών

Μπορείτε να κάνετε λήψη του πλήρους δέσμη ενεργειών του PowerShell χρησιμοποιείται [εδώ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/arm/virtual-network-deploy-multinic-arm-ps.ps1). Ακολουθήστε τα παρακάτω βήματα για να αλλάξετε τη δέσμη ενεργειών για να εργαστείτε στο περιβάλλον σας.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση την υπάρχουσα ομάδα πόρων αναπτυχθεί παραπάνω στο [τις προϋποθέσεις](#Prerequisites).

        $existingRGName        = "IaaSStory"
        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"
        $remoteAccessNSGName   = "NSG-RemoteAccess"
        $stdStorageAccountName = "wtestvnetstoragestd"

2. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση τις τιμές που θέλετε να χρησιμοποιήσετε για την ανάπτυξη παρασκηνίου.

        $backendRGName         = "IaaSStory-Backend"
        $prmStorageAccountName = "wtestvnetstorageprm"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $publisher             = "MicrosoftSQLServer"
        $offer                 = "SQL2014SP1-WS2012R2"
        $sku                   = "Standard"
        $version               = "latest"
        $vmNamePrefix          = "DB"
        $osDiskPrefix          = "osdiskdb"
        $dataDiskPrefix        = "datadisk"
        $diskSize              = "120"  
        $nicNamePrefix         = "NICDB"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

3. Ανακτήστε την υπάρχουσα τους πόρους που απαιτούνται για την ανάπτυξη.

        $vnet                  = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $existingRGName
        $backendSubnet         = $vnet.Subnets|?{$_.Name -eq $backendSubnetName}
        $remoteAccessNSG       = Get-AzureRmNetworkSecurityGroup -Name $remoteAccessNSGName -ResourceGroupName $existingRGName
        $stdStorageAccount     = Get-AzureRmStorageAccount -Name $stdStorageAccountName -ResourceGroupName $existingRGName

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Βήμα 2 - Δημιουργία απαραίτητων πόρων για το ΣΠΣ

Πρέπει να δημιουργήσετε μια νέα ομάδα πόρων, ένα λογαριασμό χώρου αποθήκευσης για το δίσκων δεδομένων και διαθεσιμότητα για όλους ΣΠΣ. Alos χρειάζεστε τα διαπιστευτήρια του λογαριασμού τοπικού διαχειριστή για κάθε Εικονική. Για να δημιουργήσετε αυτούς τους πόρους, εκτελέστε τα ακόλουθα βήματα.

1. Δημιουργία νέας ομάδας πόρων.

        New-AzureRmResourceGroup -Name $backendRGName -Location $location

2. Δημιουργία νέου λογαριασμού χώρου αποθήκευσης premium στην ομάδα πόρων δημιουργήθηκε παραπάνω.

        $prmStorageAccount = New-AzureRmStorageAccount -Name $prmStorageAccountName `
            -ResourceGroupName $backendRGName -Type Premium_LRS -Location $location

3. Δημιουργία ενός νέου συνόλου διαθεσιμότητα.

        $avSet = New-AzureRmAvailabilitySet -Name $avSetName -ResourceGroupName $backendRGName -Location $location

4. Λήψη του τοπικού διαχειριστή διαπιστευτήρια λογαριασμού που θα χρησιμοποιηθεί για κάθε Εικονική.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

### <a name="step-3---create-the-nics-and-backend-vms"></a>Βήμα 3 - Δημιουργία το NIC και παρασκηνίου ΣΠΣ

Πρέπει να χρησιμοποιήσετε ένα βρόχο για να δημιουργήσετε όσες ΣΠΣ θέλετε, και να δημιουργήσετε τις απαραίτητες NIC και ΣΠΣ μέσα στο βρόχο. Για να δημιουργήσετε το NIC και ΣΠΣ, εκτελέστε τα ακόλουθα βήματα.

1. Έναρξη μιας `for` επανάληψη για να επαναλάβετε τις εντολές για να δημιουργήσετε μια Εικονική και δύο NIC όσες φορές είναι απαραίτητο, με βάση την τιμή από το `$numberOfVMs` μεταβλητή.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Δημιουργήστε το NIC που χρησιμοποιούνται για την πρόσβαση στη βάση δεδομένων.

            $nic1Name = $nicNamePrefix + $suffixNumber + "-DA"
            $ipAddress1 = $ipAddressPrefix + ($suffixNumber + 3)
            $nic1 = New-AzureRmNetworkInterface -Name $nic1Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress1

3. Δημιουργήστε το NIC χρησιμοποιείται για απομακρυσμένη πρόσβαση. Παρατηρήστε πώς αυτό το NIC περιλαμβάνει μια NSG που σχετίζονται με αυτό.

            $nic2Name = $nicNamePrefix + $suffixNumber + "-RA"
            $ipAddress2 = $ipAddressPrefix + (53 + $suffixNumber)
            $nic2 = New-AzureRmNetworkInterface -Name $nic2Name -ResourceGroupName $backendRGName `
                -Location $location -SubnetId $backendSubnet.Id -PrivateIpAddress $ipAddress2 `
                -NetworkSecurityGroupId $remoteAccessNSG.Id

4. Δημιουργία `vmConfig` αντικειμένου.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avSet.Id

5. Δημιουργήστε δύο δίσκων δεδομένων ανά Εικονική. Παρατηρήστε ότι είναι δίσκων δεδομένων στο λογαριασμό premium χώρου αποθήκευσης που δημιουργήσατε νωρίτερα.

            $dataDisk1Name = $vmName + "-" + $osDiskPrefix + "-1"    
            $data1VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk1Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk1Name -DiskSizeInGB $diskSize `
                -VhdUri $data1VhdUri -CreateOption empty -Lun 0

            $dataDisk2Name = $vmName + "-" + $dataDiskPrefix + "-2"    
            $data2VhdUri = $prmStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $dataDisk2Name + ".vhd"
            Add-AzureRmVMDataDisk -VM $vmConfig -Name $dataDisk2Name -DiskSizeInGB $diskSize `
                -VhdUri $data2VhdUri -CreateOption empty -Lun 1

6. Ρύθμιση παραμέτρων του λειτουργικού συστήματος και εικόνα που θα χρησιμοποιηθεί για την εικονική Μηχανή.

            $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
            $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher -Offer $offer -Skus $sku -Version $version

7. Τα δύο NIC δημιουργήθηκε παραπάνω για να προσθέσετε το `vmConfig` αντικειμένου.

            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic1.Id -Primary
            $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic2.Id

8. Δημιουργήστε το λειτουργικό σύστημα δίσκου και δημιουργήστε την εικονική Μηχανή. Ειδοποίηση του `}` που τελειώνει το `for` βρόχο.

            $osDiskName = $vmName + "-" + $osDiskSuffix
            $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
            $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
            New-AzureRmVM -VM $vmConfig -ResourceGroupName $backendRGName -Location $location
        }

### <a name="step-4---run-the-script"></a>Βήμα 4 - εκτελέστε τη δέσμη ενεργειών

Τώρα που έχετε κάνει λήψη και να αλλάξει τη δέσμη ενεργειών με βάση τις ανάγκες σας, σφάλμα χρόνου εκτέλεσης αυτός δέσμη ενεργειών για να δημιουργήσετε ΣΠΣ βάσης δεδομένων παρασκηνίου με πολλές NIC.

1. Αποθηκεύστε τη δέσμη ενεργειών και εκτελέστε το από τη γραμμή εντολών **του PowerShell** ή το **PowerShell ISE**. Θα δείτε το αρχικό εξόδου, όπως φαίνεται παρακάτω.

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        ResourceId        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/IaaSStory-Backend

2. Μετά από μερικά λεπτά, συμπληρώστε τα διαπιστευτήρια ερώτηση και κάντε κλικ στο κουμπί **OK**. Το παρακάτω αποτέλεσμα αντιπροσωπεύει ένα μεμονωμένο Εικονική. Ειδοποίηση ολόκληρη τη διαδικασία εκτελέσατε 8 λεπτά για να ολοκληρωθεί.

        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        ResourceGroupName            :
        Id                           :
        Name                         : DB2
        Type                         :
        Location                     :
        Tags                         :
        TagsText                     : null
        AvailabilitySetReference     : Microsoft.Azure.Management.Compute.Models.AvailabilitySetReference
        AvailabilitySetReferenceText : {
                                         "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend/providers/
                                       Microsoft.Compute/availabilitySets/ASDB"
                                       }
        Extensions                   :
        ExtensionsText               : null
        HardwareProfile              : Microsoft.Azure.Management.Compute.Models.HardwareProfile
        HardwareProfileText          : {
                                         "VirtualMachineSize": "Standard_DS3"
                                       }
        InstanceView                 :
        InstanceViewText             : null
        NetworkProfile               :
        NetworkProfileText           : null
        OSProfile                    :
        OSProfileText                : null
        Plan                         :
        PlanText                     : null
        ProvisioningState            :
        StorageProfile               : Microsoft.Azure.Management.Compute.Models.StorageProfile
        StorageProfileText           : {
                                         "DataDisks": [
                                           {
                                             "Lun": 0,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-1",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-1.vhd"
                                             }
                                           },
                                           {
                                             "Lun": 1,
                                             "Caching": null,
                                             "CreateOption": "empty",
                                             "DiskSizeGB": 127,
                                             "Name": "DB2-datadisk-2",
                                             "SourceImage": null,
                                             "VirtualHardDisk": {
                                               "Uri": "https://wtestvnetstorageprm.blob.core.windows.net/vhds/DB2-datadisk-2.vhd"
                                             }
                                           }
                                         ],
                                         "ImageReference": null,
                                         "OSDisk": null
                                       }
        DataDiskNames                : {DB2-datadisk-1, DB2-datadisk-2}
        NetworkInterfaceIDs          :
        RequestId                    :
        StatusCode                   : 0


        EndTime             : 10/30/2015 9:30:03 AM -08:00
        Error               :
        Output              :
        StartTime           : 10/30/2015 9:22:54 AM -08:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK

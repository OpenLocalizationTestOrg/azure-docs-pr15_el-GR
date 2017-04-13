<properties 
   pageTitle="Ανάπτυξη μια Εικονική με στατική δημόσια IP χρησιμοποιώντας το PowerShell στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αναπτύξετε το ΣΠΣ με στατική δημόσια IP χρησιμοποιώντας το PowerShell στη Διαχείριση πόρων"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-powershell"></a>Ανάπτυξη μια Εικονική με στατική δημόσια IP με χρήση του PowerShell

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="step-1---start-your-script"></a>Βήμα 1 - Έναρξη της δέσμης ενεργειών

Μπορείτε να κάνετε λήψη του πλήρους δέσμη ενεργειών του PowerShell χρησιμοποιείται [εδώ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1). Ακολουθήστε τα παρακάτω βήματα για να αλλάξετε τη δέσμη ενεργειών για να εργαστείτε στο περιβάλλον σας.

1. Αλλάξτε τις τιμές από τις παρακάτω μεταβλητές με βάση τις τιμές που θέλετε να χρησιμοποιήσετε για την ανάπτυξη. Οι τιμές κάτω από το χάρτη για να το σενάριο που χρησιμοποιείται σε αυτό το έγγραφο.

        # Set variables resource group
        $rgName                = "IaaSStory"
        $location              = "West US"
        
        # Set variables for VNet
        $vnetName              = "WTestVNet"
        $vnetPrefix            = "192.168.0.0/16"
        $subnetName            = "FrontEnd"
        $subnetPrefix          = "192.168.1.0/24"
        
        # Set variables for storage
        $stdStorageAccountName = "iaasstorystorage"
        
        # Set variables for VM
        $vmSize                = "Standard_A1"
        $diskSize              = 127
        $publisher             = "MicrosoftWindowsServer"
        $offer                 = "WindowsServer"
        $sku                   = "2012-R2-Datacenter"
        $version               = "latest"
        $vmName                = "WEB1"
        $osDiskName            = "osdisk"
        $nicName               = "NICWEB1"
        $privateIPAddress      = "192.168.1.101"
        $pipName               = "PIPWEB1"
        $dnsName               = "iaasstoryws1"

## <a name="step-2---create-the-necessary-resources-for-your-vm"></a>Βήμα 2 - Δημιουργία τους πόρους που είναι απαραίτητο για την εικονική Μηχανή

Πριν να δημιουργήσετε μια Εικονική, χρειάζεστε μια ομάδα πόρων, VNet, δημόσια IP και NIC για να χρησιμοποιηθεί από την εικονική Μηχανή.

1. Δημιουργία νέας ομάδας πόρων.

        New-AzureRmResourceGroup -Name $rgName -Location $location
        
2. Δημιουργία της VNet και υποδικτύου.

        $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
            -AddressPrefix $vnetPrefix -Location $location   
        
        Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
            -VirtualNetwork $vnet -AddressPrefix $subnetPrefix
        
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet 

3. Δημιουργία δημόσιας πόρου διευθύνσεων IP. 

        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
            -AllocationMethod Static -DomainNameLabel $dnsName -Location $location

4. Δημιουργήστε το περιβάλλον εργασίας δικτύου (NIC) για την εικονική Μηχανή στο δευτερεύον δημιουργήθηκε παραπάνω, με τη δημόσια διευθύνσεων IP. Παρατηρήστε το πρώτο cmdlet ανακτώντας τα VNet από το Azure, αυτό είναι απαραίτητο, επειδή ένα **Σύνολο AzureRmVirtualNetwork** εκτελέστηκε για να αλλάξετε την υπάρχουσα VNet.

        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
            -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
            -PublicIpAddress $pip

5. Δημιουργία λογαριασμού χώρου αποθήκευσης για τη φιλοξενία του OS Εικονική μονάδα δίσκου.

        $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
            -ResourceGroupName $rgName -Type Standard_LRS -Location $location

## <a name="step-3---create-the-vm"></a>Βήμα 3 - Δημιουργία την εικονική Μηχανή 

Τώρα που όλοι οι πόροι είναι απαραίτητο είναι στη θέση, μπορείτε να δημιουργήσετε μια νέα Εικονική.

1. Δημιουργήστε το αντικείμενο ρύθμισης παραμέτρων για την εικονική Μηχανή.

        $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize 

1. Λάβετε τα διαπιστευτήρια του λογαριασμού τοπικού διαχειριστή Εικονική.

        $cred = Get-Credential -Message "Type the name and password for the local administrator account."

2. Δημιουργήστε ένα αντικείμενο ρύθμισης παραμέτρων Εικονική.

        $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
            -Credential $cred -ProvisionVMAgent -EnableAutoUpdate

3. Ορίστε την εικόνα του λειτουργικού συστήματος για την εικονική Μηχανή.

        $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
            -Offer $offer -Skus $sku -Version $version

4. Ρυθμίστε τις παραμέτρους του δίσκου OS.

        $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
        $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage

5. Προσθέστε το NIC η Εικονική.

        $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary

6. Δημιουργήστε την εικονική Μηχανή.

        New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location

7. Αποθηκεύστε το αρχείο δέσμης ενεργειών.

## <a name="step-4---run-the-script"></a>Βήμα 4 - εκτελέστε τη δέσμη ενεργειών

Αφού κάνετε τις απαραίτητες αλλαγές και κατανόηση της δέσμης ενεργειών εμφανίζεται παραπάνω, εκτελέστε τη δέσμη ενεργειών. 

1. Από κονσόλας PowerShell ή PowerShell ISE, εκτελέστε τη δέσμη ενεργειών παραπάνω.
2. Το παρακάτω αποτέλεσμα πρέπει να εμφανίζεται μετά από μερικά λεπτά.

        ResourceGroupName : IaaSStory
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory
                
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {}
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "AddressPrefix": "192.168.1.0/24"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
        
        AddressSpace      : Microsoft.Azure.Commands.Network.Models.PSAddressSpace
        DhcpOptions       : Microsoft.Azure.Commands.Network.Models.PSDhcpOptions
        Subnets           : {FrontEnd}
        ProvisioningState : Succeeded
        AddressSpaceText  : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptionsText   : {
                              "DnsServers": []
                            }
        SubnetsText       : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "ProvisioningState": "Succeeded"
                              }
                            ]
        ResourceGroupName : IaaSStory
        Location          : westus
        ResourceGuid      : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Tag               : {}
        TagsTable         : 
        Name              : WTestVNet
        Etag              : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory/providers/Microsoft.Network/virtualNetworks/WTestVNet
                
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        Status              : Succeeded
        StatusCode          : OK
        Output              : 
        StartTime           : 1/12/2016 12:57:56 PM -08:00
        EndTime             : 1/12/2016 12:59:13 PM -08:00
        Error               : 
        ErrorText           : 

   
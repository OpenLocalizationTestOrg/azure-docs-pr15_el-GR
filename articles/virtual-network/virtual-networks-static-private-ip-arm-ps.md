<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό στη Διαχείριση Azure πόρων με χρήση του PowerShell | Microsoft Azure"
   description="Κατανόηση των στατική ιδιωτικές διευθύνσεις IP και πώς μπορείτε να διαχειριστείτε τους στο διαχειριστή πόρων Azure με χρήση του PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-in-resource-manager-by-using-powershell"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό στη Διαχείριση πόρων με χρήση του PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [διαχειριστείτε στατική διεύθυνση IP ιδιωτικό στο μοντέλο κλασική ανάπτυξης](virtual-networks-static-private-ip-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Το δείγμα PowerShell παρακάτω εντολές περιμένετε ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει με βάση το παραπάνω σενάριο. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής περιγράφεται στη [Δημιουργία ενός vnet](virtual-networks-create-vnet-arm-ps.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική διεύθυνση IP ιδιωτικά, κατά τη δημιουργία μια εικονική Μηχανή
Για να δημιουργήσετε μια Εικονική με το όνομα *DNS01* στο δευτερεύον *FrontEnd* από ένα VNet με το όνομα *TestVNet* με στατική ιδιωτικό IP του *192.168.1.101*, ακολουθήστε τα παρακάτω βήματα:

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Ορισμός μεταβλητές για το λογαριασμό χώρου αποθήκευσης, θέση, ομάδα πόρων και τα διαπιστευτήρια που θα χρησιμοποιηθεί. Θα πρέπει να εισαγάγετε ένα όνομα χρήστη και τον κωδικό πρόσβασης για την εικονική Μηχανή. Πρέπει να υπάρχει ήδη, την ομάδα αποθήκευσης για το λογαριασμό και πόρων.

        $stName = "vnetstorage"
        $locName = "Central US"
        $rgName = "TestRG"
        $cred = Get-Credential -Message "Type the name and password of the local administrator account."

3. Ανακτήστε την εικονικού δικτύου και το δευτερεύον που θέλετε να δημιουργήσετε την Εικονική στα.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet  
        $subnet = $vnet.Subnets[0].Id

4. Εάν είναι απαραίτητο, δημιουργήστε μια δημόσια διεύθυνση IP για να αποκτήσετε πρόσβαση η Εικονική από το Internet.

        $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

5. Δημιουργήστε ένα NIC χρησιμοποιώντας τη στατική διεύθυνση IP ιδιωτικό που θέλετε να αντιστοιχίσετε την εικονική Μηχανή. Βεβαιωθείτε ότι είναι το IP από την περιοχή υποδικτύου που προσθέτετε την εικονική Μηχανή να. Αυτό είναι το κύριο βήμα για αυτό το άρθρο, όπου μπορείτε να ορίσετε τη διεύθυνση IP ιδιωτική να είναι στατική.

        $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.1.101

6. Δημιουργήστε την εικονική Μηχανή χρησιμοποιώντας το NIC δημιουργήθηκε παραπάνω.

        $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
        $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01  -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
        $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
        $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
        $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri -CreateOption fromImage
        New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 

    Αναμενόμενο αποτέλεσμα:

        EndTime             : 9/8/2015 2:32:09 PM -07:00
        Error               : 
        Output              : 
        StartTime           : 9/8/2015 2:27:42 PM -07:00
        Status              : Succeeded
        TrackingOperationId : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        RequestId           : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
        StatusCode          : OK 


## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική
Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή PowerShell και παρατηρήστε τις τιμές για *PrivateIpAddress* και *PrivateIpAllocationMethod*:

    Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG

Αναμενόμενο αποτέλεσμα:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/Te
                           stNIC
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMach
                           ines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkIn
                           terfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtual
                           Networks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicI
                           PAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Για να καταργήσετε τη στατική διεύθυνση IP ιδιωτικό προστεθεί η Εικονική στη δέσμη ενεργειών παραπάνω, εκτελέστε τις ακόλουθες εντολές του PowerShell:
    
    $nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
    $nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
    Set-AzureRmNetworkInterface -NetworkInterface $nic

Αναμενόμενο αποτέλεσμα:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/Te
                           stNIC
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMach
                           ines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkIn
                           terfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtual
                           Networks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/publicI
                           PAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική διεύθυνση IP ιδιωτικό σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική ιδιωτική διεύθυνση IP για την εικονική Μηχανή που δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, σφάλμα χρόνου εκτέλεσης ακόλουθο εντολή:

    $nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
    $nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
    $nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
    Set-AzureRmNetworkInterface -NetworkInterface $nic

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).
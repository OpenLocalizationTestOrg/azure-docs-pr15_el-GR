<properties 
    pageTitle="Προσομοιωμένη υβριδικό περιβάλλον δοκιμής cloud | Microsoft Azure" 
    description="Δημιουργία προσομοιωμένη υβριδικού περιβάλλοντος cloud για IT pro ή δοκιμών ανάπτυξης, με χρήση δύο Azure εικονικών δικτύων και μια σύνδεση VNet-VNet." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Ρύθμιση του cloud προσομοιωμένη υβριδικού περιβάλλοντος για σκοπούς δοκιμής

Σε αυτό το άρθρο σας καθοδηγεί στη δημιουργία προσομοιωμένη υβριδικού περιβάλλοντος cloud με το Microsoft Azure χρησιμοποιώντας δύο Azure εικονικού δίκτυα. Ακολουθεί η ρύθμιση παραμέτρων που προκύπτει.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Με αυτόν τον προσομοιώνει ένα υβριδικό περιβάλλον παραγωγής cloud και αποτελείται από:

- Ένα δίκτυο προσομοιωμένη και απλοποιημένη εσωτερικής εγκατάστασης που φιλοξενούνται σε μια Azure εικονικού δικτύου (το εικονικό δίκτυο TestLab).
- Ένα δίκτυο προσομοιωμένη σταυρό εσωτερικής εγκατάστασης εικονικού φιλοξενείται στο Azure (TestVNET).
- Μια σύνδεση VNet-VNet μεταξύ των δύο εικονικού δικτύων.
- Έναν ελεγκτή δευτερεύοντα τομέα στο δίκτυο εικονικού TestVNET.

Αυτή η δυνατότητα παρέχει μια βάση και κοινές ξεκινώντας σημείο από το οποίο μπορείτε να:

- Ανάπτυξη και δοκιμή εφαρμογές σε μια πρόχειρη υβριδικό περιβάλλον cloud.
- Δημιουργία δοκιμαστική παραμέτρων υπολογιστών, ορισμένες εντός του TestLab εικονικού δικτύου και ορισμένα εντός του TestVNET εικονικού δικτύου, να προσομοιώσετε υβριδική βασίζεται στο cloud IT φόρτους εργασίας του.

Υπάρχουν τέσσερις κύριες φάσεις για τη ρύθμιση των αυτό το υβριδικό περιβάλλον δοκιμής cloud:

1.  Ρύθμιση παραμέτρων το εικονικό δίκτυο TestLab.
2.  Δημιουργήστε το εικονικό δίκτυο σταυρό εσωτερικής εγκατάστασης.
3.  Δημιουργία της σύνδεσης VPN VNet-VNet.
4.  Ρύθμιση παραμέτρων του ελεγκτή τομέα DC2. 

Αυτή η ρύθμιση παραμέτρων απαιτεί μια συνδρομή του Azure. Εάν έχετε μια συνδρομή MSDN ή Visual Studio, ανατρέξτε στο θέμα [πιστωτικής μηνιαία Azure για τους συνδρομητές του Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Εικονικές μηχανές και των πυλών εικονικού δικτύου στο Azure αναλαμβάνει ένα κόστος σε εξέλιξη νομισματικά όταν εκτελούνται. Αυτό το κόστος είναι χρεώνονται σε σχέση με το MSDN ή που καταβλήθηκε συνδρομής. Μια πύλη Azure VPN έχει υλοποιηθεί ως σύνολο δύο Azure εικονικές μηχανές. Για να ελαχιστοποιήσετε το κόστος, δημιουργήστε το περιβάλλον δοκιμής και εκτελέστε τις απαραίτητες δοκιμές και επίδειξης συντομότερο δυνατό.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Φάση 1: Ρύθμιση παραμέτρων του δικτύου εικονικού TestLab

Χρησιμοποιήστε τις οδηγίες στο θέμα [Ρύθμιση παραμέτρων βάσης δοκιμαστικού περιβάλλοντος](https://technet.microsoft.com/library/mt771177.aspx) για να ρυθμίσετε το DC1, APP1 και CLIENT1 υπολογιστές στο Azure εικονικό δίκτυο με το όνομα TestLab. 

Στη συνέχεια, ξεκινήστε μια γραμμή εντολών του Azure PowerShell.

> [AZURE.NOTE] Η παρακάτω εντολή ορίζει χρήση Azure PowerShell 1.0 και νεότερες εκδόσεις.

Πραγματοποιήστε είσοδο στο λογαριασμό σας.

    Login-AzureRMAccount

Λάβετε το όνομα της συνδρομής σας χρησιμοποιώντας την ακόλουθη εντολή.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Ορίστε τη συνδρομή σας στο Azure. Χρησιμοποιήστε την ίδια συνδρομή που χρησιμοποιήσατε για να δημιουργήσετε το βασικό σας παραμέτρων σε φάσεις 1. Αντικαταστήστε τα πάντα μέσα σε τις προσφορές, συμπεριλαμβανομένων του < και > χαρακτήρες, με το σωστό όνομα.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Στη συνέχεια, προσθέστε ένα υποδίκτυο πύλης στο δίκτυο εικονικού TestLab με βάση τις ρυθμίσεις σας, που θα χρησιμοποιηθεί για τη φιλοξενία του Azure πύλης.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Στη συνέχεια, να ζητήσετε μια δημόσια διεύθυνση IP για να εκχωρήσετε στην πύλη για το εικονικό δίκτυο TestLab.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Στη συνέχεια, δημιουργήστε την πύλη.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Έχετε υπόψη ότι νέα πύλες μπορεί να διαρκέσει 20 λεπτά ή περισσότερο για να δημιουργήσετε.

Από την πύλη Azure στον τοπικό υπολογιστή σας, συνδεθείτε με DC1 με τα διαπιστευτήρια CORP\User1. Για να ρυθμίσετε τον τομέα CORP ώστε υπολογιστές και οι χρήστες χρησιμοποιούν το τοπικό ελεγκτή τομέα για τον έλεγχο ταυτότητας, εκτελέστε αυτές οι εντολές από μια γραμμή εντολών του Windows PowerShell επιπέδου διαχειριστή στην DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Φάση 2: Δημιουργήστε το εικονικό δίκτυο TestVNET

Πρώτα, δημιουργήστε το εικονικό δίκτυο TestVNET και προστασία του με μια ομάδα ασφαλείας δικτύου.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Στη συνέχεια, να ζητήσετε μια δημόσια διεύθυνση IP που θα εκχωρηθεί στην πύλη για το εικονικό δίκτυο TestVNET και τη δημιουργία της πύλης.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Φάση 3: Δημιουργία της σύνδεσης VNet-VNet

Πρώτα, μπορείτε να αποκτήσετε ένα ήδη κοινόχρηστο κλειδί τυχαία, με κρυπτογράφηση ισχυρό, 32 χαρακτήρων από το διαχειριστή δικτύου ή την ασφάλεια. Εναλλακτικά, χρησιμοποιήστε τις πληροφορίες στη [Δημιουργία συμβολοσειράς τυχαία για ένα ήδη κοινόχρηστο κλειδί ασφαλείας IP](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) για να αποκτήσετε ένα ήδη κοινόχρηστο κλειδί.

Στη συνέχεια, χρησιμοποιήστε αυτές τις εντολές για να δημιουργήσετε τη σύνδεση VPN VNet-VNet, που μπορεί να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Μετά από μερικά λεπτά, θα πρέπει η δημιουργία της σύνδεσης.

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Φάση 4: Ρύθμιση παραμέτρων του ελεγκτή τομέα DC2

Πρώτα, δημιουργήστε μια εικονική μηχανή για ελεγκτή τομέα DC2. Εκτέλεση αυτών των εντολών στη γραμμή εντολών του PowerShell Azure στον τοπικό σας υπολογιστή.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Στη συνέχεια, συνδεθείτε με τον νέο υπολογιστή εικονικές ελεγκτή τομέα DC2 από την πύλη του Azure.

Στη συνέχεια, ρυθμίστε τις παραμέτρους ενός κανόνα τείχους προστασίας των Windows για να επιτρέψετε την κυκλοφορία για σκοπούς δοκιμής βασική σύνδεση. Από μια γραμμή εντολών του Windows PowerShell επιπέδου διαχειριστή στον ελεγκτή τομέα DC2, εκτελέστε αυτές οι εντολές.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Η εντολή ping θα πρέπει να έχει ως αποτέλεσμα τέσσερις επιτυχής απαντήσεις από τη διεύθυνση IP 10.0.0.4. Αυτή είναι μια δοκιμαστική κίνησης κατά μήκος της σύνδεσης VNet-VNet.

Στη συνέχεια, προσθέστε το δίσκο επιπλέον δεδομένα στον ελεγκτή τομέα DC2 ως ένα νέο ένταση με το γράμμα στ:.

1.  Στο αριστερό τμήμα του παραθύρου της διαχείρισης διακομιστών, κάντε κλικ στην επιλογή **αρχείο και τις υπηρεσίες αποθήκευσης**και, στη συνέχεια, κάντε κλικ στην επιλογή **δίσκων**.
2.  Στο παράθυρο περιεχόμενο, στην ομάδα **δίσκων** , κάντε κλικ στην επιλογή **δίσκο 2** (με τα **διαμερίσματα** οριστεί σε **Άγνωστη**).
3.  Κάντε κλικ στην επιλογή **εργασίες**και, στη συνέχεια, κάντε κλικ στην επιλογή **Νέα ένταση**.
4.  Στην πριν ξεκινήσετε σελίδα του οδηγού ένταση, κάντε κλικ στο κουμπί **Επόμενο**.
5.  Στη σελίδα επιλέξτε τον διακομιστή και του δίσκου, κάντε κλικ στην επιλογή **2 δίσκου**και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**. Όταν σας ζητηθεί, κάντε κλικ στο **κουμπί OK**.
6.  Στη Καθορισμός το μέγεθος της σελίδας ένταση, κάντε κλικ στο κουμπί **Επόμενο**.
7.  Στη εκχώρηση σε μια σελίδα γράμμα ή το φάκελο μονάδα δίσκου, κάντε κλικ στο κουμπί **Επόμενο**.
8.  Στη σελίδα ρυθμίσεις Επιλέξτε αρχείο συστήματος, κάντε κλικ στο κουμπί **Επόμενο**.
9.  Στη σελίδα Επιλογές επιβεβαίωση, κάντε κλικ στην επιλογή **Δημιουργία**.
10. Όταν τελειώσετε, κάντε κλικ στο κουμπί **Κλείσιμο**.

Στη συνέχεια, ρύθμιση παραμέτρων του ελεγκτή τομέα DC2 ως μια ρεπλίκα ελεγκτή τομέα για τον τομέα corp.contoso.com. Εκτέλεση αυτών των εντολών από τη γραμμή εντολών του Windows PowerShell στον ελεγκτή τομέα DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Σημειώστε ότι θα σας ζητηθεί να δώσετε τον κωδικό πρόσβασης CORP\User1 και έναν κωδικό πρόσβασης λειτουργία επαναφοράς υπηρεσιών καταλόγου (DSRM) και να κάνετε επανεκκίνηση του ελεγκτή τομέα DC2.

Τώρα που το εικονικό δίκτυο TestVNET διαθέτει δικό του διακομιστή DNS (ελεγκτή τομέα DC2), πρέπει να ρυθμίσετε το εικονικό δίκτυο TestVNET για να χρησιμοποιήσετε αυτόν το διακομιστή DNS.

1.  Στο αριστερό τμήμα του Azure πύλη, κάντε κλικ στο εικονίδιο εικονικών δικτύων και, στη συνέχεια, κάντε κλικ στην επιλογή **TestVNET**.
2.  Στην καρτέλα **Ρυθμίσεις** , κάντε κλικ στην επιλογή **διακομιστές DNS**.
3.  Στο **κύριο διακομιστή DNS**, πληκτρολογήστε **192.168.0.4** για να αντικαταστήσετε 10.0.0.4.
4.  Κάντε κλικ στην επιλογή **Αποθήκευση**.

Αυτή είναι η τρέχουσα ρύθμιση παραμέτρων. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Το cloud προσομοιωμένη υβριδικό περιβάλλον είναι τώρα έτοιμο για σκοπούς δοκιμής.

## <a name="next-step"></a>Επόμενο βήμα

- Ορίστε μια [γραμμή επιχειρηματικών εφαρμογών που βασίζεται στο web,](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) σε αυτό το περιβάλλον.

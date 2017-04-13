<properties
    pageTitle="Δημιουργία Internet αντικριστές εξισορρόπηση φόρτου με IPv6 με χρήση του PowerShell για τη διαχείριση πόρων | Microsoft Azure"
    description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου με IPv6 με χρήση του PowerShell για τη διαχείριση πόρων"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="το IPv6, azure εξισορρόπηση φόρτου, διπλή στοίβα, δημόσια ip, εγγενούς ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου με IPv6 με χρήση του PowerShell για τη διαχείριση πόρων

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Πρότυπο](./load-balancer-ipv6-internet-template.md)

Μια μονάδα εξισορρόπησης φόρτου Azure είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-4 (TCP, UDP). Η μονάδα εξισορρόπησης φόρτου παρέχει υψηλή διαθεσιμότητα από τη διανομή εισερχόμενη κίνηση μεταξύ των παρουσιών σε καλή κατάσταση υπηρεσίας στις υπηρεσίες cloud ή εικονικές μηχανές σε ένα σύνολο εξισορρόπησης φόρτου. Azure εξισορρόπηση φόρτου επίσης να εμφανίσετε αυτές τις υπηρεσίες σε πολλαπλές θύρες, πολλές διευθύνσεις IP ή και τα δύο.

## <a name="example-deployment-scenario"></a>Παράδειγμα ανάπτυξης σεναρίου

Το παρακάτω διάγραμμα παρουσιάζει το λύση αναπτύσσεται σε αυτό το άρθρο εξισορρόπησης φόρτου.

![Σενάριο εξισορρόπησης φόρτου](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Σε αυτό το σενάριο θα δημιουργήσετε στους παρακάτω πόρους Azure:

- μια μονάδα εξισορρόπησης φόρτου μέσω Internet με ένα IPv4 και μια IPv6 δημόσια διεύθυνση IP
- δύο φόρτωση εξισορρόπησης κανόνες για να αντιστοιχίσετε τη δημόσια VIPs για τα τελικά σημεία ιδιωτική
- μια ρύθμιση διαθεσιμότητα που περιέχει τα δύο ΣΠΣ
- δύο εικονικές μηχανές (ΣΠΣ)
- περιβάλλον εικονικού δικτύου για κάθε Εικονική με διευθύνσεις IPv4 και IPv6 στους οποίους έχουν ανατεθεί

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Ανάπτυξη της λύσης χρησιμοποιώντας το PowerShell Azure

Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να δημιουργήσετε ένα Internet αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας τη διαχείριση πόρων Azure με το PowerShell. Με το Azure διαχείριση πόρων, κάθε πόρο έχει δημιουργηθεί και έχει ρυθμιστεί ξεχωριστά, στη συνέχεια, τοποθέτηση για να δημιουργήσετε έναν πόρο.

Για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου, δημιουργία και ρύθμιση παραμέτρων τα ακόλουθα αντικείμενα:

- Ρύθμιση παραμέτρων προσκηνίου IP - περιέχει δημόσιες διευθύνσεις IP για την εισερχόμενη κίνηση δικτύου.
- Χώρος συγκέντρωσης παρασκηνίου διεύθυνση - περιέχει διασυνδέσεις δικτύου (NIC) για τις εικονικές μηχανές για να λαμβάνετε κίνηση του δικτύου από τη μονάδα εξισορρόπησης φόρτου.
- Εξισορρόπηση φόρτου κανόνες - περιέχει κανόνες αντιστοίχισης μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου στη θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- NAT κανόνες εισερχομένων - περιέχει κανόνες αντιστοίχιση μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- Probes - περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιείται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονικές μηχανές στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Ρύθμιση του PowerShell για να χρησιμοποιήσετε για τη διαχείριση πόρων

Βεβαιωθείτε ότι έχετε την πιο πρόσφατη έκδοση παραγωγής της λειτουργικής μονάδας από διαχειριστή πόρων Azure για το PowerShell.

1. Πραγματοποιήστε είσοδο στο Azure

        Login-AzureRmAccount

    Εισαγάγετε τα διαπιστευτήριά σας όταν σας ζητηθεί.

2. Ελέγξτε τις συνδρομές για το λογαριασμό

        Get-AzureRmSubscription

3. Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Δημιουργήστε μια ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Δημιουργήστε ένα εικονικό δίκτυο και μια δημόσια διεύθυνση IP για το χώρο συγκέντρωσης προσκηνίου IP

1. Δημιουργήστε ένα εικονικό δίκτυο με ένα δευτερεύον δίκτυο.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Δημιουργία Azure δημόσια διεύθυνση IP (PIP) πόρων για την προσκηνίου ομάδα διευθύνσεων IP.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Η μονάδα εξισορρόπησης φόρτου χρησιμοποιεί την ετικέτα τομέα από τη δημόσια IP ως πρόθεμα για το FQDN. Σε αυτό το παράδειγμα, το FQDN είναι *lbnrpipv4.westus.cloudapp.azure.com* και *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Δημιουργήστε μια ρυθμίσεων περιβάλλοντος IP και ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου

1. Δημιουργία ρύθμισης παραμέτρων προσκηνίου διεύθυνση που χρησιμοποιεί τις διευθύνσεις IP δημόσια που δημιουργήσατε.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Δημιουργήστε σύνολα διεύθυνση παρασκηνίου.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Δημιουργία κανόνων Λίβρες, κανόνες NAT, μια δοκιμή του και μια μονάδα εξισορρόπησης φόρτου

Αυτό το παράδειγμα δημιουργεί τα ακόλουθα στοιχεία:

- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 443 στη θύρα 4443
- Ένας κανόνας εξισορρόπησης φόρτου να εξισορροπήσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 80 στη θύρα 80 στις διευθύνσεις στο χώρο συγκέντρωσης παρασκηνίου.
- Ένας κανόνας εξισορρόπησης φόρτου που επιτρέπει RDP σύνδεσης του ΣΠΣ στη θύρα 3389.
- δοκιμή του κανόνα για να ελέγξετε την κατάσταση εύρυθμης λειτουργίας σε μια σελίδα με όνομα *HealthProbe.aspx* ή μια υπηρεσία στη θύρα 8080
- μια μονάδα εξισορρόπησης φόρτου που χρησιμοποιεί όλα αυτά τα αντικείμενα

1. Δημιουργήστε τους κανόνες NAT.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Δημιουργήστε μια δοκιμή εύρυθμης λειτουργίας του. Υπάρχουν δύο τρόποι για να ρυθμίσετε τις παραμέτρους ενός δοκιμή του:

    Δοκιμή του HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    ή δοκιμή του TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    Για αυτό το παράδειγμα, θα χρησιμοποιήσετε το καθετήρες TCP.

3. Δημιουργήστε έναν κανόνα εξισορρόπησης φόρτου.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Δημιουργήστε τη μονάδα εξισορρόπησης φόρτου χρησιμοποιώντας τα αντικείμενα που δημιουργήσατε προηγουμένως.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Δημιουργία NIC για του ΣΠΣ παρασκηνίου

1. Λήψη του εικονικού δικτύου και εικονικό δίκτυο υποδίκτυο, όπου το NIC πρέπει να δημιουργηθούν.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Δημιουργήστε τις ρυθμίσεις παραμέτρων IP και NIC για του ΣΠΣ.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Δημιουργία εικονικές μηχανές και εκχωρήστε το NIC που έχουν δημιουργηθεί πρόσφατα

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία μια Εικονική, ανατρέξτε στο θέμα [Δημιουργία και ρύθμιση παραμέτρων μια εικονική μηχανή Windows με το διαχειριστή πόρων και Azure PowerShell](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Δημιουργήστε ένα λογαριασμό διαθεσιμότητα Ορισμός και αποθήκευση

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Δημιουργία κάθε Εικονική και αντιστοίχιση προηγούμενου δημιουργήθηκε NIC

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)

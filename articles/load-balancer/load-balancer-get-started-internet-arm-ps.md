<properties
   pageTitle="Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου μέσω Internet στη Διαχείριση πόρων με χρήση του PowerShell | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου μέσω Internet στη Διαχείριση πόρων με χρήση του PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
   ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="get-started"></a>Δημιουργία μια μονάδα εξισορρόπησης φόρτου μέσω Internet στη Διαχείριση πόρων με χρήση του PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο ανάπτυξης διαχείρισης πόρων. Μπορείτε επίσης να [Μάθετε πώς μπορείτε να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου μέσω Internet, χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Ανάπτυξη της λύσης με χρήση του Azure PowerShell

Οι ακόλουθες διαδικασίες εξηγούν τον τρόπο για να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου μέσω Internet χρησιμοποιώντας τη διαχείριση πόρων Azure με το PowerShell. Με το Azure διαχείριση πόρων, κάθε πόρο δημιουργείται και έχει ρυθμιστεί ξεχωριστά, και στη συνέχεια, τοποθετήστε για να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου.

Πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους των παρακάτω αντικειμένων για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου:

- Ρύθμιση παραμέτρων IP του προσκηνίου: περιέχει δημόσιες διευθύνσεις IP (PIP) για την εισερχόμενη κίνηση δικτύου.
- Σύνολο παρασκηνίου διευθύνσεων: περιέχει διασυνδέσεις δικτύου (NIC) για τις εικονικές μηχανές για να λαμβάνετε κίνηση του δικτύου από τη μονάδα εξισορρόπησης φόρτου.
- Εξισορρόπηση φόρτου κανόνες: περιέχει κανόνες που αντιστοίχισης μιας δημόσιας θύρας στην τη μονάδα εξισορρόπησης φόρτου σε μια θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- NAT κανόνες εισερχομένων: περιέχει κανόνες που να αντιστοιχούν μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- Καθετήρες: περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιείται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονική μηχανή στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Ρύθμιση του PowerShell για να χρησιμοποιήσετε για τη διαχείριση πόρων

Βεβαιωθείτε ότι έχετε την πιο πρόσφατη έκδοση παραγωγής της λειτουργικής μονάδας από διαχειριστή πόρων Azure για το PowerShell:

1. Πραγματοποιήστε είσοδο στο Azure.

        Login-AzureRmAccount

    Εισαγάγετε τα διαπιστευτήριά σας όταν σας ζητηθεί.

2. Ελέγξτε τις συνδρομές για το λογαριασμό.

        Get-AzureRmSubscription

3. Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Δημιουργία μιας ομάδας πόρων. (Παραλείψετε αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Δημιουργήστε ένα εικονικό δίκτυο και μια δημόσια διεύθυνση IP για το χώρο συγκέντρωσης προσκηνίου IP

1. Δημιουργήστε ένα υποδίκτυο και ένα εικονικό δίκτυο.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Δημιουργία ενός Azure δημόσια πόρου διεύθυνσης IP, που ονομάζεται **PublicIP**, θα χρησιμοποιηθεί από ένα σύνολο προσκηνίου IP με το όνομα DNS **loadbalancernrp.westus.cloudapp.azure.com**. Η παρακάτω εντολή χρησιμοποιεί τον τύπο στατική εκχώρηση.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Τη μονάδα εξισορρόπησης φόρτου χρησιμοποιεί την ετικέτα τομέα από τη δημόσια IP ως ένα πρόθεμα για το FQDN. Αυτό είναι διαφορετικό από το μοντέλο κλασική ανάπτυξης, που χρησιμοποιείται από την υπηρεσία cloud ως τη μονάδα εξισορρόπησης φόρτου FQDN.
    >Σε αυτό το παράδειγμα, το FQDN είναι **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Δημιουργία χώρου συγκέντρωσης προσκηνίου IP και ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου

1. Δημιουργία ενός προσκηνίου χώρου συγκέντρωσης διευθύνσεων IP που ονομάζεται **Προσκήνιο Λίβρες** που χρησιμοποιεί τον πόρο **PublicIp** .

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Δημιουργία χώρου συγκέντρωσης παρασκηνίου διευθύνσεων με όνομα **Λίβρες παρασκηνίου**.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Δημιουργία κανόνων NAT, ένας κανόνας εξισορρόπησης φόρτου, μια δοκιμή του και μια μονάδα εξισορρόπησης φόρτου

Αυτό το παράδειγμα δημιουργεί τα ακόλουθα στοιχεία:

- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 3441 στη θύρα 3389
- Ένας κανόνας NAT για να μεταφράσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 3442 στη θύρα 3389
- Δοκιμή του κανόνα για να ελέγξετε την κατάσταση εύρυθμης λειτουργίας σε μια σελίδα με το όνομα **HealthProbe.aspx**
- Ένας κανόνας εξισορρόπησης φόρτου να εξισορροπήσετε όλη την εισερχόμενη κυκλοφορία στη θύρα 80 στη θύρα 80 στις διευθύνσεις στο χώρο συγκέντρωσης παρασκηνίου
- Μια μονάδα εξισορρόπησης φόρτου που χρησιμοποιεί όλα αυτά τα αντικείμενα

Ακολουθήστε τα παρακάτω βήματα:

1. Δημιουργήστε τους κανόνες NAT.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Δημιουργήστε μια δοκιμή εύρυθμης λειτουργίας του. Υπάρχουν δύο τρόποι για να ρυθμίσετε τις παραμέτρους ενός δοκιμή του:

    Δοκιμή του HTTP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    Δοκιμή του TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Δημιουργήστε έναν κανόνα εξισορρόπησης φόρτου.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Δημιουργήστε τη μονάδα εξισορρόπησης φόρτου χρησιμοποιώντας τα αντικείμενα που δημιουργήσατε προηγουμένως.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Δημιουργία NIC

Δημιουργία διασυνδέσεων δικτύου (ή να τροποποιήσετε υπάρχουσες) και, στη συνέχεια, να τα συσχετίζετε για κανόνες NAT, κανόνες εξισορρόπησης φόρτου και καθετήρες:

1. Λάβετε το εικονικό δίκτυο και ένα υποδίκτυο εικονικού δικτύου, όπου το NIC πρέπει να δημιουργηθούν.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Δημιουργία ενός NIC με το όνομα **είναι nic1 λίβρες**, και να συσχετίσετε με τον πρώτο κανόνα NAT και το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου πρώτη (και μόνο).

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Δημιουργία ενός NIC με το όνομα **είναι nic2 λίβρες**, και να συσχετίσετε με τον δεύτερο κανόνα NAT και το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου πρώτη (και μόνο).

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Ελέγξτε το NIC.

        $backendnic1

    Αναμενόμενο αποτέλεσμα:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Χρησιμοποιήστε το `Add-AzureRmVMNetworkInterface` cmdlet για να αντιστοιχίσετε το NIC σε διαφορετική ΣΠΣ.

## <a name="create-a-virtual-machine"></a>Δημιουργήστε μια εικονική μηχανή

Για οδηγίες σχετικά με τη δημιουργία μια εικονική μηχανή και εκχώρηση NIC, ανατρέξτε στο θέμα [Δημιουργία Εικονική μηχανή Azure χρήση του PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Προσθέστε το περιβάλλον εργασίας δικτύου για να τη μονάδα εξισορρόπησης φόρτου

1. Ανακτήστε την εξισορρόπηση φόρτου από το Azure.

    Φόρτωση του πόρου εξισορρόπησης φόρτου σε μια μεταβλητή (Εάν δεν το έχετε κάνει που ακόμα). Η μεταβλητή ονομάζεται **$lb**. Χρησιμοποιήστε τα ίδια ονόματα από τον πόρο εξισορρόπησης φόρτου που δημιουργήσατε νωρίτερα.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Τοποθετήστε τη ρύθμιση παραμέτρων υποστήριξης σε μεταβλητή.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Τοποθετήστε το περιβάλλον εργασίας που έχουν δημιουργηθεί ήδη δικτύου σε μεταβλητή. Το όνομα της μεταβλητής είναι **$nic**. Το όνομα του περιβάλλοντος εργασίας δικτύου είναι το ίδιο με αυτό από το παραπάνω παράδειγμα.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Αλλάξτε τη ρύθμιση παραμέτρων υποστήριξης στο περιβάλλον εργασίας δικτύου.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Αποθηκεύστε το αντικείμενο περιβάλλοντος εργασίας δικτύου.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Μετά από ένα περιβάλλον εργασίας δικτύου προστίθεται στο χώρο συγκέντρωσης παρασκηνίου εξισορρόπησης φόρτου, ξεκινά λαμβάνει κίνηση του δικτύου με βάση τους κανόνες εξισορρόπησης φόρτου για το συγκεκριμένο πόρο εξισορρόπησης φόρτου.

## <a name="update-an-existing-load-balancer"></a>Ενημέρωση μιας υπάρχουσας εξισορρόπηση φόρτου

1. Χρησιμοποιώντας τη μονάδα εξισορρόπησης φόρτου από το παραπάνω παράδειγμα, αντιστοιχίστε ένα αντικείμενο εξισορρόπησης φόρτου τη μεταβλητή **$slb** , χρησιμοποιώντας `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Στο παρακάτω παράδειγμα, μπορείτε να προσθέσετε έναν κανόνα εισερχομένων NAT--χρησιμοποιώντας 81 στο χώρο συγκέντρωσης προσκηνίου θύρες και 8181 για το χώρο συγκέντρωσης παρασκηνίου--σε μια υπάρχουσα εξισορρόπηση φόρτου.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Αποθηκεύστε τη νέα ρύθμιση παραμέτρων χρησιμοποιώντας `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Καταργήστε μια μονάδα εξισορρόπησης φόρτου

Χρησιμοποιήστε την εντολή `Remove-AzureLoadBalancer` για να διαγράψετε μια μονάδα εξισορρόπησης φόρτου που δημιουργήσατε προηγουμένως που ονομάζεται **NRP Λίβρες** σε μια ομάδα πόρων που ονομάζεται **NRP RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε το προαιρετικός διακόπτης **-ισχύ** για να αποφύγετε την ερώτηση για διαγραφή.

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)

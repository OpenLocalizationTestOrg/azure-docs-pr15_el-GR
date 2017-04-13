<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell στη Διαχείριση πόρων"
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

# <a name="create-an-internal-load-balancer-using-powershell"></a>Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Ακολουθήστε τα παρακάτω βήματα εξηγούν τον τρόπο για να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου χρήση της διαχείρισης πόρων Azure με PowerShell. Με Azure διαχείριση πόρων, τα στοιχεία για να δημιουργήσετε μια εσωτερική εξισορρόπηση φόρτου έχουν ρυθμιστεί μεμονωμένα και, στη συνέχεια, συνδυάζονται για να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου.

Πρέπει να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους για να αναπτύξετε μια μονάδα εξισορρόπησης φόρτου τα ακόλουθα αντικείμενα:

- Πρώτο πλάνο Τέλος ρύθμισης παραμέτρων IP - θα ρυθμίσετε τις παραμέτρους του ιδιωτική διεύθυνση IP για την εισερχόμενη κίνηση δικτύου
- Χώρος συγκέντρωσης διεύθυνση παρασκηνίου - θα ρυθμίσετε τις παραμέτρους των διασυνδέσεων δικτύου με θα λάβουν την κυκλοφορία εξισορρόπησης φόρτου που προέρχονται από το χώρο συγκέντρωσης IP υπολογιστή-πελάτη
- Η φόρτωση εξισορρόπησης κανόνων - προέλευσης και ρύθμιση παραμέτρων τοπική θύρα για τη μονάδα εξισορρόπησης φόρτου.
- Probes - ρυθμίζει τις παραμέτρους τη δοκιμή του κατάσταση εύρυθμης λειτουργίας για τις παρουσίες εικονική μηχανή.
- NAT κανόνες εισερχομένων - ρυθμίζει τις παραμέτρους των κανόνων θύρας για απευθείας πρόσβαση σε μία από τις παρουσίες εικονική μηχανή.

Μπορείτε να λάβετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης στοιχεία με τη Διαχείριση Azure πόρων στο [διαχειριστή πόρων Azure υποστήριξης για εξισορρόπηση φόρτου](load-balancer-arm.md).

Ακολουθήστε τα παρακάτω βήματα εξηγούν τον τρόπο για να ρυθμίσετε μια μονάδα εξισορρόπησης φόρτου μεταξύ των δύο εικονικές μηχανές.


## <a name="setup-powershell-to-use-resource-manager"></a>Ρύθμιση PowerShell για τη χρήση της διαχείρισης πόρων

Βεβαιωθείτε ότι έχετε την πιο πρόσφατη έκδοση της παραγωγής από τη λειτουργική μονάδα Azure για το PowerShell και έχετε σωστά ρύθμιση του PowerShell για να αποκτήσετε πρόσβαση Azure τη συνδρομή σας.

### <a name="step-1"></a>Βήμα 1

        Login-AzureRmAccount

### <a name="step-2"></a>Βήμα 2

Ελέγξτε τις συνδρομές για το λογαριασμό

        Get-AzureRmSubscription

Θα σας ζητηθεί να έλεγχος ταυτότητας με τα διαπιστευτήριά σας.<BR>

### <a name="step-3"></a>Βήμα 3

Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Δημιουργία ομάδας πόρων για εξισορρόπηση φόρτου

### <a name="step-4"></a>Βήμα 4

Δημιουργήστε μια νέα ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια μονάδα εξισορρόπησης φόρτου θα χρησιμοποιούν την ίδια ομάδα πόρων.

Στο παραπάνω παράδειγμα που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "NRP-RG" και τη θέση "Δυτική ΜΑΣ".

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Δημιουργία εικονικού δικτύου και μια ιδιωτική διεύθυνση IP για το χώρο συγκέντρωσης IP υπολογιστή-πελάτη


### <a name="step-1"></a>Βήμα 1

Δημιουργεί ένα υποδίκτυο για το εικονικό δίκτυο και αντιστοιχίζει στη μεταβλητή $backendSubnet

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Δημιουργήστε ένα εικονικό δίκτυο:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Δημιουργεί το εικονικό δίκτυο και προσθέτει το υποδίκτυο είναι υποδικτύου λίβρες το εικονικό δίκτυο NRPVNet και αντιστοιχίζει στη μεταβλητή $vnet



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Δημιουργία εμπρός IP προσκηνίου και παρασκηνίου χώρο συγκέντρωσης διευθύνσεων

Ρύθμιση του χώρου συγκέντρωσης IP υπολογιστή-πελάτη για το εισερχόμενες φόρτωσης εξισορρόπησης δικτύου κυκλοφορία και παρασκηνίου διεύθυνση χώρο συγκέντρωσης για να εμφανιστεί η φόρτωση εξισορρόπηση κίνηση.

### <a name="step-1"></a>Βήμα 1

Δημιουργία χώρου συγκέντρωσης IP προσκηνίου χρησιμοποιώντας την ιδιωτική διεύθυνση IP 10.0.2.5 για το 10.0.2.0/24 υποδικτύου που θα είναι το εισερχόμενες τελικό σημείο κίνηση του δικτύου.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>βήμα 2

Ρύθμιση του χώρου συγκέντρωσης διεύθυνση παρασκηνίου που χρησιμοποιείται για τη λήψη εισερχόμενα δεδομένα από το χώρο συγκέντρωσης IP υπολογιστή-πελάτη:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Δημιουργία κανόνων Λίβρες, κανόνες NAT, δοκιμή του και μονάδα εξισορρόπησης φόρτου

Αφού δημιουργήσετε το χώρο συγκέντρωσης IP υπολογιστή-πελάτη και το σύνολο των διευθύνσεων παρασκηνίου, θα πρέπει να δημιουργήσετε τους κανόνες που ανήκει στον πόρο εξισορρόπησης φόρτου:

### <a name="step-1"></a>Βήμα 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Το παραπάνω παράδειγμα δημιουργεί τα ακόλουθα στοιχεία:

- Κανόνας NAT που όλη την εισερχόμενη κυκλοφορία στη θύρα 3441 να μεταβαίνουν στη θύρα 3389.
- ένα δεύτερο κανόνα NAT που όλη την εισερχόμενη κυκλοφορία στη θύρα 3442 να μεταβαίνουν στη θύρα 3389.
- Ένας κανόνας εξισορρόπησης φόρτου τη φόρτωση υπόλοιπο όλα τα εισερχόμενα κίνηση στη δημόσια 80 σε τοπική θύρα 80 στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
- δοκιμή του κανόνα που θα ελέγξετε την κατάσταση εύρυθμης λειτουργίας για τη διαδρομή "HealthProbe.aspx"



### <a name="step-2"></a>Βήμα 2

Δημιουργήστε τη μονάδα εξισορρόπησης φόρτου πρόσθεση όλων των αντικειμένων (NAT κανόνες, κανόνες εξισορρόπησης φόρτου, ρυθμίσεις παραμέτρων δοκιμή του):

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Δημιουργία διασυνδέσεις δικτύου

Αφού δημιουργήσετε το εσωτερικό εξισορρόπηση φόρτου, χρειάζεστε μπορείτε να καθορίσετε ποια διασυνδέσεις δικτύου θα λαμβάνετε εισερχόμενες κίνηση του δικτύου εξισορρόπησης φόρτου, κανόνες NAT και δοκιμή του. Το περιβάλλον εργασίας δικτύου σε αυτήν την περίπτωση έχει ρυθμιστεί μεμονωμένα και μπορεί να αντιστοιχιστεί σε μια εικονική μηχανή αργότερα.


### <a name="step-1"></a>Βήμα 1


Λήψη του πόρου εικονικού δικτύου και το δευτερεύον για να δημιουργήσετε διασυνδέσεις δικτύου:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Αυτό το βήμα δημιουργεί ένα περιβάλλον εργασίας δικτύου, η οποία θα ανήκει στο χώρο συγκέντρωσης παρασκηνίου εξισορρόπησης φόρτου και να συσχετίσετε τον πρώτο κανόνα NAT για RDP για αυτήν τη διασύνδεση δικτύου:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Βήμα 2

Δημιουργήστε μια δεύτερη διασύνδεση δικτύου που ονομάζεται Λίβρες-Nic2-ΕΊΝΑΙ:

Αυτό το βήμα δημιουργεί μια δεύτερη διασύνδεση δικτύου, εκχώρηση για τον ίδιο χώρο συγκέντρωσης παρασκηνίου εξισορρόπησης φόρτου και συσχετίζοντας τον δεύτερο κανόνα NAT που έχει δημιουργηθεί για RDP:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Το τελικό αποτέλεσμα θα εμφανίσει τα εξής:

    $backendnic1

Αναμενόμενο αποτέλεσμα:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Βήμα 3

Χρησιμοποιήστε την εντολή Προσθήκη AzureRmVMNetworkInterface για να αντιστοιχίσετε το NIC σε μια εικονική μηχανή.

Μπορείτε να βρείτε τις οδηγίες βήμα προς βήμα για να δημιουργήσετε μια εικονική μηχανή και εκχωρήστε NIC ακολουθώντας την τεκμηρίωση: [Δημιουργία Εικονική μηχανή Azure χρησιμοποιώντας PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Εάν έχετε ήδη μια εικονική μηχανή δημιουργήθηκε, μπορείτε να προσθέσετε το περιβάλλον εργασίας δικτύου με τα παρακάτω βήματα:

#### <a name="step-1"></a>Βήμα 1

Φόρτωση του πόρου εξισορρόπησης φόρτου σε μια μεταβλητή (Εάν δεν το έχετε κάνει που ακόμα). Η μεταβλητή χρησιμοποιείται ονομάζεται $lb και χρησιμοποιήστε τα ίδια ονόματα από τον πόρο εξισορρόπησης φόρτου που δημιουργήθηκε παραπάνω.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Βήμα 2

Τοποθετήστε τη ρύθμιση παραμέτρων υποστήριξης σε μεταβλητή.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Βήμα 3

Τοποθετήστε το περιβάλλον εργασίας που έχουν δημιουργηθεί ήδη δικτύου σε μεταβλητή. το όνομα της μεταβλητής χρησιμοποιούνται είναι $nic. Το όνομα του περιβάλλοντος εργασίας δικτύου χρησιμοποιούνται είναι η ίδια από το παραπάνω παράδειγμα.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Βήμα 4

Αλλάξτε τη ρύθμιση παραμέτρων υποστήριξης στο περιβάλλον εργασίας δικτύου.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Βήμα 5

Αποθηκεύστε το αντικείμενο περιβάλλοντος εργασίας δικτύου.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Μετά από ένα περιβάλλον εργασίας δικτύου προστίθεται στο χώρο συγκέντρωσης παρασκηνίου εξισορρόπησης φόρτου, ξεκινά λαμβάνει κίνηση του δικτύου με βάση το κανόνες για το συγκεκριμένο πόρο εξισορρόπησης φόρτου εξισορρόπησης φόρτου.

## <a name="update-an-existing-load-balancer"></a>Ενημέρωση μιας υπάρχουσας εξισορρόπηση φόρτου


### <a name="step-1"></a>Βήμα 1

Χρησιμοποιώντας τη μονάδα εξισορρόπησης φόρτου από το παραπάνω παράδειγμα, αντιστοιχίστε αντικείμενο εξισορρόπησης φόρτου σε μεταβλητή $slb χρησιμοποιώντας Get-AzureRmLoadBalancer

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Βήμα 2

Στο παρακάτω παράδειγμα, θα μπορείτε να προσθέσετε έναν νέο κανόνα εισερχομένων NAT χρησιμοποιώντας θύρα 81 στο προσκήνιο και τη θύρα 8181 για το χώρο συγκέντρωσης παρασκηνίου σε μια υπάρχουσα εξισορρόπηση φόρτου

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Βήμα 3

Αποθηκεύστε τη νέα ρύθμιση παραμέτρων χρησιμοποιώντας σύνολο AzureLoadBalancer

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Καταργήστε μια μονάδα εξισορρόπησης φόρτου

Χρησιμοποιήστε την εντολή Κατάργηση AzureRmLoadBalancer για να διαγράψετε μια μονάδα εξισορρόπησης φόρτου που δημιουργήσατε προηγουμένως που ονομάζεται "NRP-Λίβρες" σε μια ομάδα πόρων που ονομάζεται "NRP-RG"

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε την προαιρετική εναλλαγή - ισχύει για να αποφύγετε την ερώτηση για διαγραφή.



## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
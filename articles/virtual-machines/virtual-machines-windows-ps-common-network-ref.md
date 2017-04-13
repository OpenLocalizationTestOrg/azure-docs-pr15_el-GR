<properties
    pageTitle="Εντολές κοινό δίκτυο του PowerShell για ΣΠΣ | Microsoft Azure"
    description="Συνήθεις εντολές του PowerShell για να ξεκινήσετε τη δημιουργία ένα εικονικό δίκτυο και τις συναφείς πόρους για ΣΠΣ."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Συνήθεις εντολές του Azure PowerShell δικτύου για ΣΠΣ

Εάν θέλετε να δημιουργήσετε μια εικονική μηχανή, πρέπει να δημιουργήσετε [εικονικού δικτύου](../virtual-network/virtual-networks-overview.md) ή να γνωρίζετε σχετικά με ένα υπάρχον εικονικό δίκτυο στο οποίο μπορείτε να προσθέσετε την εικονική Μηχανή. Συνήθως, όταν δημιουργείτε μια Εικονική, πρέπει επίσης να λάβετε υπόψη τη δημιουργία τους πόρους που περιγράφονται σε αυτό το άρθρο.

Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) για πληροφορίες σχετικά με την εγκατάσταση την πιο πρόσφατη έκδοση του Azure PowerShell, επιλέγοντας τη συνδρομή σας και είσοδος στο λογαριασμό σας.

## <a name="create-network-resources"></a>Δημιουργία πόρων δικτύου

Εργασία | Εντολή 
-------------- | -------------------------
Δημιουργία υποδικτύου παραμέτρων | $subnet1 = [Δημιουργία AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -όνομα "subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = δημιουργία AzureRmVirtualNetworkSubnetConfig-όνομα "subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Ένα τυπικό δικτύου μπορεί να έχει ένα υποδίκτυο για μια [μονάδα εξισορρόπησης φόρτου αντικριστές internet](../load-balancer/load-balancer-internet-overview.md) και ένα ξεχωριστό υποδίκτυο για μια [εσωτερική εξισορρόπηση φόρτου](../load-balancer/load-balancer-internal-overview.md). |
Δημιουργήστε ένα εικονικό δίκτυο | $vnet = [Δημιουργία AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -resource_group_name"όνομα"virtual_network_name"- ResourceGroupName"-θέση "location_name" - AddressPrefix XX. X.X.X/XX-υποδικτύου $subnet1, $subnet2
Έλεγχος για ένα όνομα τομέα μοναδικών τιμών | [Δοκιμή AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "όνομα_τομέα"-θέση "location_name"<BR><BR>Μπορείτε να καθορίσετε ένα όνομα τομέα DNS για μια [δημόσια IP πόρων](../virtual-network/virtual-network-ip-addresses-overview-arm.md), ο οποίος δημιουργεί μια αντιστοίχιση για domainname.location.cloudapp.azure.com στη δημόσια διεύθυνση IP στους διακομιστές Azure Διαχείριση DNS. Το όνομα μπορεί να περιέχει μόνο γράμματα, αριθμούς και παύλες. Το πρώτο και το τελευταίο χαρακτήρα πρέπει να είναι ένα γράμμα ή αριθμό και το όνομα τομέα πρέπει να είναι μοναδικό μέσα σε τη θέση του Azure. Εάν επιστρέφεται **τιμή True** , το προτεινόμενο όνομα είναι καθολικά μοναδικό.
Δημιουργήστε μια δημόσια διεύθυνση IP | $pip = [Δημιουργία AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -όνομα "ip_address_name" - ResourceGroupName "resource_group_name" - DomainNameLabel "όνομα_τομέα"-θέση "location_name" - AllocationMethod δυναμικής<BR><BR>Στη δημόσια διεύθυνση IP χρησιμοποιεί το όνομα τομέα που ελεγχθεί προηγουμένως και χρησιμοποιείται από τη ρύθμιση παραμέτρων frontend τη μονάδα εξισορρόπησης φόρτου.
Δημιουργία μιας ρύθμισης παραμέτρων IP frontend | $frontendIP = [Δημιουργία AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -όνομα "frontend_ip_name" - PublicIpAddress $pip<BR><BR>Η ρύθμιση παραμέτρων frontend περιλαμβάνει στη δημόσια διεύθυνση IP που δημιουργήσατε προηγουμένως για εισερχόμενη κίνηση δικτύου.
Δημιουργία χώρου συγκέντρωσης διεύθυνση παρασκηνίου | $beAddressPool = [Δημιουργία AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -όνομα "backend_pool_name"<BR><BR>Παρέχει εσωτερικές διευθύνσεις για υπολογιστή στο παρασκήνιο της τη μονάδα εξισορρόπησης φόρτου που είναι προσβάσιμες μέσω ενός περιβάλλοντος εργασίας δικτύου.
Δημιουργήστε μια δοκιμή του | $healthProbe = [Δημιουργία AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -όνομα "probe_name" - RequestPath 'HealthProbe.aspx'-πρωτοκόλλου http-θύρα 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Περιέχει καθετήρες εύρυθμης λειτουργίας που χρησιμοποιείται για να ελέγξετε τη διαθεσιμότητα των παρουσιών εικονικές μηχανές στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
Δημιουργήστε μια φόρτωση εξισορρόπηση κανόνα | $lbRule = [Δημιουργία AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -όνομα HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-Διερεύνηση $healthProbe-πρωτόκολλο Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Περιέχει κανόνες που εκχώρηση δημόσια θύρας στην τη μονάδα εξισορρόπησης φόρτου σε μια θύρα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
Δημιουργία κανόνα εισερχομένων NAT | $inboundNATRule = [Δημιουργία AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -όνομα "rule_name" - FrontendIpConfiguration $frontendIP-πρωτόκολλο TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Περιέχει κανόνες αντιστοίχιση μια δημόσια θύρα τη μονάδα εξισορρόπησης φόρτου σε μια θύρα για ένα συγκεκριμένο εικονικό μηχάνημα στο χώρο συγκέντρωσης διεύθυνση παρασκηνίου.
Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου | $loadBalancer = "resource_group_name" [Δημιουργία AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-όνομα "load_balancer_name"-θέση "location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Διερεύνηση $healthProbe
Δημιουργήστε ένα περιβάλλον εργασίας δικτύου | $nic1 = "resource_group_name" [Δημιουργία AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-όνομα "network_interface_name"-θέση "location_name" - PrivateIpAddress XX. X.X.X-$loadBalancer.BackendAddressPools[0 - LoadBalancerBackendAddressPool subnet2 υποδίκτυο] $loadBalancer.InboundNatRules[0 - LoadBalancerInboundNatRule]<BR><BR>Δημιουργήστε ένα περιβάλλον εργασίας δικτύου, χρησιμοποιώντας τη δημόσια διεύθυνση IP και υποδικτύου εικονικού δικτύου που δημιουργήσατε προηγουμένως.
    
## <a name="get-information-about-network-resources"></a>Λήψη πληροφοριών σχετικά με τους πόρους του δικτύου

Εργασία | Εντολή 
-------------- | -------------------------
Λίστα εικονικών δικτύων | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Παραθέτει σε λίστα όλα τα δίκτυα εικονικού στην ομάδα πόρων.
Λήψη πληροφοριών σχετικά με ένα εικονικό δίκτυο | Get-AzureRmVirtualNetwork-resource_group_name"όνομα"virtual_network_name"- ResourceGroupName"
Λίστα δευτερεύοντα δίκτυα σε ένα εικονικό δίκτυο | Get-AzureRmVirtualNetwork-ονομασία "virtual_network_name" - ResourceGroupName "resource_group_name" & #124- Επιλέξτε δευτερεύοντα δίκτυα
Λήψη πληροφοριών σχετικά με ένα υποδίκτυο | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -όνομα "subnet_name" - VirtualNetwork $vnet<BR><BR>Λαμβάνει πληροφορίες σχετικά με το υποδίκτυο στο καθορισμένο εικονικό δίκτυο. Η τιμή $vnet αντιπροσωπεύει το αντικείμενο που επιστρέφονται από Get-AzureRmVirtualNetwork.
Λίστα διευθύνσεων IP | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Εμφανίζει τις δημόσιες διευθύνσεις IP, στην ομάδα πόρων.
Λίστα φόρτωσης balancers | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Παραθέτει σε λίστα όλα τα balancers φόρτωσης στην ομάδα πόρων.
Λίστα διασυνδέσεων δικτύου | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "resource_group_name"<BR><BR>Παραθέτει σε λίστα όλες τις διασυνδέσεις δικτύου στην ομάδα πόρων.
Λήψη πληροφοριών σχετικά με ένα περιβάλλον εργασίας δικτύου | Get-AzureRmNetworkInterface-resource_group_name"όνομα"network_interface_name"- ResourceGroupName"<BR><BR>Λαμβάνει πληροφορίες σχετικά με ένα συγκεκριμένο δίκτυο περιβάλλον εργασίας.
Λάβετε τη ρύθμιση παραμέτρων IP από ένα περιβάλλον εργασίας δικτύου | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -όνομα "ipconfiguration_name" - NetworkInterface $nic<BR><BR>Λαμβάνει πληροφορίες σχετικά με τη ρύθμιση παραμέτρων IP του περιβάλλοντος εργασίας καθορισμένο δικτύου. Η τιμή $nic αντιπροσωπεύει το αντικείμενο που επιστρέφονται από Get-AzureRmNetworkInterface.

## <a name="manage-network-resources"></a>Διαχείριση πόρων δικτύου

Εργασία | Εντολή 
-------------- | -------------------------
Προσθέστε ένα δευτερεύον σε εικονικό δίκτυο | [Προσθήκη AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-όνομα "subnet_name" - VirtualNetwork $vnet<BR><BR>Προσθέτει ένα υποδίκτυο σε ένα υπάρχον εικονικό δίκτυο. Η τιμή $vnet αντιπροσωπεύει το αντικείμενο που επιστρέφονται από Get-AzureRmVirtualNetwork.
Διαγραφή ενός εικονικού δικτύου | [Κατάργηση AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -resource_group_name"όνομα"virtual_network_name"- ResourceGroupName"<BR><BR>Καταργεί το καθορισμένο εικονικό δίκτυο από την ομάδα πόρων.
Διαγράψτε ένα περιβάλλον εργασίας δικτύου | [Κατάργηση AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -resource_group_name"όνομα"network_interface_name"- ResourceGroupName"<BR><BR>Καταργεί το περιβάλλον εργασίας δικτύου καθορισμένο από την ομάδα πόρων.
Διαγράψτε μια μονάδα εξισορρόπησης φόρτου | [Κατάργηση AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -resource_group_name"όνομα"load_balancer_name"- ResourceGroupName"<BR><BR>Καταργεί το καθορισμένο εξισορρόπηση φόρτου από την ομάδα πόρων.
Διαγράψτε μια δημόσια διεύθυνση IP | [Κατάργηση AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-resource_group_name"όνομα"ip_address_name"- ResourceGroupName"<BR><BR>Καταργεί την καθορισμένη δημόσια διεύθυνση IP από την ομάδα πόρων.

## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε το περιβάλλον εργασίας δικτύου που μόλις δημιουργήσατε πότε μπορείτε να [δημιουργήσετε μια Εικονική](virtual-machines-windows-ps-create.md).
- Μάθετε πώς μπορείτε να [δημιουργήσετε μια Εικονική με πολλές διασυνδέσεις δικτύου](../virtual-network/virtual-networks-multiple-nics.md).

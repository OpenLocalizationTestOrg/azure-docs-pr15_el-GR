<properties 
   pageTitle="Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση για συνδέσεις τοποθεσίας σε τοποθεσία με χρήση του μοντέλου ανάπτυξης διαχείρισης πόρων | Microsoft Azure"
   description="Πώς μπορείτε να κάνετε ανακατεύθυνση ή 'επιβάλετε' όλη την κυκλοφορία Internet συνδεδεμένων στην τοποθεσία σας εσωτερικής εγκατάστασης."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων Azure

> [AZURE.SELECTOR]
- [PowerShell - κλασικό](vpn-gateway-about-forced-tunneling.md)
- [PowerShell - διαχείριση πόρων](vpn-gateway-forced-tunneling-rm.md)

Εξαναγκασμένη διοχέτευση σάς επιτρέπει να ανακατεύθυνση ή "ισχύει" όλη την κυκλοφορία Internet συνδεδεμένων πίσω στην τοποθεσία σας στην εσωτερική εγκατάσταση μέσω διοχέτευσης VPN τοποθεσίας σε τοποθεσία για επιθεώρηση και έλεγχος. Αυτή είναι μια απαίτηση κρίσιμες ασφαλείας για τα περισσότερα για μεγάλες επιχειρήσεις IT πολιτικές.

Χωρίς εξαναγκασμένη διοχέτευση κυκλοφορία Internet συνδεδεμένων από το ΣΠΣ στο Azure θα πάντα διέλευση από το Azure την υποδομή δικτύου απευθείας ανάληψη στο Internet, χωρίς την επιλογή για να σας επιτρέπουν να ελέγξετε ή να ελέγχετε την κυκλοφορία. Ενδέχεται να οδηγήσουν σε κοινοποίηση πληροφοριών ή άλλους τύπους παραβιάσεις ασφαλείας μη εξουσιοδοτημένη πρόσβαση στο Internet

Σε αυτό το άρθρο σάς καθοδηγεί σε τη ρύθμιση των παραμέτρων υποχρεωτική διοχέτευση για εικονικού δίκτυα που δημιουργούνται με χρήση του μοντέλου ανάπτυξης διαχείρισης πόρων.

**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Ανάπτυξη μοντέλων και εργαλεία για την εξαναγκασμένη διοχέτευση**

Εξαναγκασμένη διοχέτευσης σύνδεσης μπορεί να ρυθμιστεί για το μοντέλο κλασική ανάπτυξης και το μοντέλο ανάπτυξης διαχείρισης πόρων. Ανατρέξτε στον παρακάτω πίνακα για περισσότερες πληροφορίες. Ενημερώνουμε αυτόν τον πίνακα ως νέα άρθρα, νέα μοντέλα ανάπτυξης και πρόσθετα εργαλεία γίνονται διαθέσιμοι για αυτήν τη ρύθμιση παραμέτρων. Όταν ένα άρθρο είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Υποχρεωτική περίπου διοχέτευση


Το παρακάτω διάγραμμα παρουσιάζει πώς εξαναγκασμένη διοχέτευσης λειτουργεί. 

![Υποχρεωτική διοχέτευση](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Στο παραπάνω παράδειγμα, το Frontend υποδικτύου δεν είναι υποχρεωτική γίνει διοχέτευση. Το φόρτο εργασίας στο δευτερεύον Frontend να συνεχίσετε να αποδεχτείτε και απάντηση σε αιτήματα πελατών από το Internet απευθείας. Είστε υποχρεωμένοι των δευτερευόντων δικτύων ενδιάμεσο επίπεδο και παρασκηνίου διοχέτευσης. Οποιαδήποτε εξερχόμενες συνδέσεις από αυτές τις δύο δευτερευόντων δικτύων στο Internet θα υποχρεωτική ή θα ανακατευθύνεται ξανά με μια τοποθεσία εσωτερικής μέσω ένα από τα σήραγγες S2S VPN.

Αυτό σας επιτρέπει να περιορισμός και έλεγχος πρόσβασης στο Internet από το εικονικές μηχανές ή στο cloud services σε Azure, ενώ συνεχίζετε να ενεργοποιήσετε την αρχιτεκτονική πολλαπλών επιπέδων υπηρεσίας απαιτείται. Μπορείτε επίσης να εφαρμόσετε εξαναγκασμένη διοχέτευση με τα δίκτυα ολόκληρο εικονικό εάν υπάρχουν χωρίς φόρτους εργασίας μέσω Internet στο εικονικού δίκτυά σας.

## <a name="requirements-and-considerations"></a>Απαιτήσεις και τα θέματα

Εξαναγκασμένη διοχέτευση στο Azure έχει ρυθμιστεί μέσω δρομολογεί εικονικού δικτύου που ορίζονται από το χρήστη. Ανακατεύθυνση κίνηση σε μια εσωτερική τοποθεσία εκφράζεται ως μια προεπιλεγμένη δρομολόγηση για την πύλη Azure VPN. Για περισσότερες πληροφορίες σχετικά με τη δρομολόγηση ορίζονται από το χρήστη και εικονικών δικτύων, ανατρέξτε στο θέμα [ορίζονται από το χρήστη δρομολογεί και προώθηση IP](../virtual-network/virtual-networks-udr-overview.md).

- Κάθε υποδίκτυο εικονικού δικτύου έχει ένα ενσωματωμένο, πίνακας δρομολόγησης συστήματος. Ο πίνακας δρομολόγησης συστήματος περιλαμβάνει τις ακόλουθες τρεις ομάδες διαδρομών:

    - **Δρομολογεί την τοπική VNet:** Απευθείας στον προορισμό της ΣΠΣ στο ίδιο δίκτυο εικονικού
    
    - **Εσωτερικής δρομολογεί:** Για την πύλη Azure VPN
    
    - **Προεπιλεγμένη δρομολόγηση:** Απευθείας με το Internet. Πακέτα που προορίζονται για να το ιδιωτικών διευθύνσεων IP δεν καλύπτεται από την προηγούμενη δύο δρομολογεί θα καταργηθεί.

-  Αυτή η διαδικασία χρησιμοποιεί δρομολογεί ορίζονται από το χρήστη (UDR) για να δημιουργήσετε έναν πίνακα δρομολόγησης για να προσθέσετε μια προεπιλεγμένη δρομολόγηση και, στη συνέχεια, να συσχετίσετε τον πίνακα δρομολόγησης για να σας subnet(s) VNet για να ενεργοποιήσετε την εξαναγκασμένη διοχέτευση σε αυτά τα δευτερεύοντα δίκτυα.

- Εξαναγκασμένη διοχέτευση πρέπει να σχετίζονται με ένα VNet που έχει μια πύλη VPN βάσει δρομολόγηση. Πρέπει να ορίσετε μια "προεπιλεγμένη τοποθεσία" μεταξύ των τοποθεσιών τοπικού σταυρό εσωτερικής εγκατάστασης συνδεδεμένοι με το εικονικό δίκτυο.

- ExpressRoute υποχρεωτική διοχέτευση δεν έχει ρυθμιστεί μέσω αυτού του μηχανισμού, αλλά αντί για αυτό, είναι ενεργοποιημένη από διαφήμιση μια προεπιλεγμένη δρομολόγηση μέσω του peering περιόδους λειτουργίας το πρωτόκολλο BGP ExpressRoute. Ανατρέξτε στην [Τεκμηρίωση ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) για περισσότερες πληροφορίες.

## <a name="configuration-overview"></a>Επισκόπηση της ρύθμισης παραμέτρων

Η παρακάτω διαδικασία σάς βοηθά να δημιουργήσετε μια ομάδα πόρων και ένα VNet. Μπορείτε, στη συνέχεια, Δημιουργία πύλης VPN και να ρυθμίσετε τις παραμέτρους εξαναγκασμένη διοχέτευση. Σε αυτήν τη διαδικασία, το εικονικό δίκτυο "MultiTier-VNet" έχει 3 δευτερεύοντα δίκτυα: *Frontend*, *Midtier*και *παρασκηνίου*, με 4 σταυρό εσωτερική συνδέσεις: *DefaultSiteHQ*και 3 *διακλαδώσεις*.

Τα βήματα της διαδικασίας ορίστε το *DefaultSiteHQ* ως την προεπιλεγμένη σύνδεση τοποθεσίας για υποχρεωτική διοχέτευση και ρυθμίσετε τις παραμέτρους του Midtier και δευτερεύοντα δίκτυα παρασκηνίου για να χρησιμοποιήσετε υποχρεωτική διοχέτευση.

    
## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν από την έναρξη της ρύθμισης παραμέτρων.

- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).

- Θα πρέπει να εγκαταστήσετε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure (1,0 ή νεότερη έκδοση). Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .


## <a name="configure-forced-tunneling"></a>Ρύθμιση παραμέτρων εξαναγκασμένη διοχέτευση

1. Στην κονσόλα του PowerShell, συνδεθείτε στο λογαριασμό σας στο Azure. Αυτό το cmdlet ζητά για τα διαπιστευτήρια σύνδεσης στο λογαριασμό σας στο Azure. Μετά την καταγραφή στο, να κάνει λήψη ρυθμίσεις του λογαριασμού σας, ώστε να είναι διαθέσιμες για Azure PowerShell.

        Login-AzureRmAccount 

2. Λάβετε μια λίστα με όλες τις συνδρομές σας Azure.

        Get-AzureRmSubscription

2. Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Δημιουργία μιας ομάδας πόρων.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Δημιουργήστε ένα εικονικό δίκτυο και καθορίστε δευτερεύοντα δίκτυα. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Δημιουργία τοπικού δικτύου πυλών.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Δημιουργία πίνακα δρομολόγηση και δρομολόγηση κανόνα.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Συσχέτιση του πίνακα δρομολόγηση για να τα δευτερεύοντα δίκτυα Midtier και παρασκηνίου.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Δημιουργία της πύλης με μια προεπιλεγμένη τοποθεσία. Αυτό το βήμα απαιτείται χρόνος για να ολοκληρώσετε, μερικές φορές 45 λεπτά ή περισσότερο, επειδή δημιουργείτε και ρύθμιση παραμέτρων της πύλης.<br> Η `-GatewayDefaultSite` είναι η παράμετρος cmdlet που επιτρέπει την εξαναγκασμένη παραμέτρους δρομολόγησης για να εργαστείτε, ώστε να χειριστείτε για να ρυθμίσετε τις παραμέτρους σωστά αυτήν τη ρύθμιση. Αυτή η παράμετρος είναι διαθέσιμη στο PowerShell 1.0 ή νεότερη έκδοση.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Δημιουργήστε τις συνδέσεις VPN-τοποθεσίας.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        




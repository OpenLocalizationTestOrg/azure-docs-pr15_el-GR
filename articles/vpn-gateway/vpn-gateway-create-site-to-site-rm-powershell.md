<properties
   pageTitle="Δημιουργήστε ένα εικονικό δίκτυο με μια σύνδεση VPN-τοποθεσίας χρήση της διαχείρισης πόρων Azure και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε τη δημιουργία ενός VNet χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και τη σύνδεση του με το δίκτυό σας τοπικό εσωτερικής εγκατάστασης χρησιμοποιώντας μια σύνδεση S2S VPN πύλης."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>Δημιουργήστε μια VNet με χρήση του PowerShell σύνδεσης τοποθεσίας σε τοποθεσία

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Κλασικό - κλασική πύλη](vpn-gateway-site-to-site-create.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε δημιουργώντας ένα εικονικό δίκτυο και σύνδεση VPN-τοποθεσίας πύλης στο δίκτυό σας εσωτερικής εγκατάστασης χρησιμοποιώντας το μοντέλο ανάπτυξης Azure διαχείριση πόρων. Συνδέσεις τοποθεσίας σε τοποθεσία μπορούν να χρησιμοποιηθούν για διασταυρούμενο εσωτερικής εγκατάστασης και υβριδικό ρυθμίσεις παραμέτρων.

![Διάγραμμα τοποθεσίας σε τοποθεσία] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "τοποθεσία σε τοποθεσία") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Μοντέλα ανάπτυξης και οι μέθοδοι για συνδέσεις τοποθεσίας σε τοποθεσία

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Ο παρακάτω πίνακας εμφανίζει την ανάπτυξη του αυτήν τη στιγμή διαθέσιμα μοντέλα και μεθόδων για ρυθμίσεις παραμέτρων τοποθεσίας σε τοποθεσία. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Πρόσθετες ρυθμίσεις

Εάν θέλετε να συνδεθείτε μαζί VNets, αλλά δεν θα δημιουργήσετε μια σύνδεση προς μια θέση εσωτερικής εγκατάστασης, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet](vpn-gateway-vnet-vnet-rm-ps.md). Εάν θέλετε να προσθέσετε μια σύνδεση-τοποθεσίας σε μια VNet που διαθέτει ήδη μια σύνδεση, ανατρέξτε στο θέμα [Προσθήκη σύνδεσης S2S σε μια VNet με μια υπάρχουσα σύνδεση VPN πύλης](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Βεβαιωθείτε ότι έχετε τα ακόλουθα στοιχεία πριν να ξεκινήσετε τη ρύθμιση των παραμέτρων.

- Μια συμβατή συσκευή VPN και κάποιο άτομο που είναι σε θέση να ρυθμίσετε τις παραμέτρους της. Ανατρέξτε στο θέμα [σχετικά με τις συσκευές VPN](vpn-gateway-about-vpn-devices.md). Εάν δεν είστε εξοικειωμένοι με τη ρύθμιση των παραμέτρων σας συσκευή VPN, ή να είστε εξοικειωμένοι με τη διεύθυνση IP περιοχές που βρίσκεται στο ρυθμίσεις παραμέτρων δικτύου σας εσωτερικής εγκατάστασης, θα πρέπει να επικοινωνήσετε με κάποιον που μπορούν να παρέχουν αυτές τις λεπτομέρειες για εσάς.

- Μια εξωτερική αντικριστές δημόσια διεύθυνση IP για τη συσκευή σας VPN. Αυτή η διεύθυνση IP δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT.
    
- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).
    
- Η πιο πρόσφατη έκδοση του τα cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .


## <a name="Login"></a>1. συνδεθείτε με τη συνδρομή σας 

Βεβαιωθείτε ότι μπορείτε Μετάβαση σε κατάσταση λειτουργίας PowerShell για να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

    Login-AzureRmAccount

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription 

Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2. Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο πύλης

Τα παραδείγματα χρησιμοποιούν ένα υποδίκτυο πύλης του /28. Ενώ είναι δυνατή η δημιουργία μιας πύλης υποδικτύου όσο /29, συνιστάται να δημιουργήσετε ένα μεγαλύτερο υποδίκτυο που περιλαμβάνει περισσότερες διευθύνσεις, επιλέγοντας τουλάχιστον /28 ή /27. Αυτό θα σας επιτρέψει για τις διευθύνσεις αρκετό ώστε να περιλαμβάνει δυνατές πρόσθετες ρυθμίσεις που ίσως θέλετε στο μέλλον.

Εάν έχετε ήδη ένα εικονικό δίκτυο με ένα υποδίκτυο πύλη που είναι/29 ή μεγαλύτερο, μπορείτε να προχωρήσετε στο μέλλον να [προσθέσετε την πύλη τοπικού δικτύου](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Για να δημιουργήσετε ένα εικονικό δίκτυο και ένα υποδίκτυο πύλης

Χρησιμοποιήστε το παρακάτω παράδειγμα για να δημιουργήσετε ένα εικονικό δίκτυο και ένα υποδίκτυο πύλης. Αντικαταστήστε τις τιμές για τη δική σας. 

Πρώτα, δημιουργήστε μια ομάδα πόρων:
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Στη συνέχεια, δημιουργήστε το εικονικό δίκτυο. Βεβαιωθείτε ότι η διεύθυνση διαστήματα που καθορίζετε δεν επικαλύπτονται οποιονδήποτε από τους χώρους διευθύνσεων που διαθέτετε στο δίκτυό σας εσωτερικής εγκατάστασης.

Το παρακάτω δείγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα *testvnet* και δύο δευτερευόντων δικτύων, μία που ονομάζεται *GatewaySubnet* και το άλλο ονομάζεται *Subnet1*. Είναι σημαντικό για να δημιουργήσετε ένα δευτερεύον δίκτυο με το όνομα συγκεκριμένα *GatewaySubnet*. Εάν έχετε ονομάστε το κάτι άλλο, της ρύθμισης παραμέτρων σύνδεσης θα αποτύχει. 

Ορίστε τις μεταβλητές.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Δημιουργήστε το VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Για να προσθέσετε ένα υποδίκτυο πύλης σε ένα εικονικό δίκτυο που έχετε ήδη δημιουργήσει

Αυτό το βήμα απαιτείται μόνο εάν χρειάζεστε για να προσθέσετε ένα υποδίκτυο πύλη σε μια VNet που δημιουργήσατε προηγουμένως.

Μπορείτε να δημιουργήσετε το δικό σας υποδίκτυο πύλης χρησιμοποιώντας το παρακάτω παράδειγμα. Φροντίστε να ονομάσετε το υποδίκτυο πύλης 'GatewaySubnet'. Εάν έχετε ονομάστε το κάτι άλλο, μπορείτε να δημιουργήσετε ένα υποδίκτυο, αλλά Azure δεν θα χειριστείτε την ως υποδίκτυο πύλης.

Ορίστε τις μεταβλητές.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Δημιουργήστε το υποδίκτυο πύλης.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Ρύθμιση των παραμέτρων. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>προσθήκη της πύλης τοπικού δικτύου

Ένα εικονικό δίκτυο, η πύλη τοπικό δίκτυο συνήθως αναφέρεται στη θέση σας εσωτερικής εγκατάστασης. Δώστε στην τοποθεσία ένα όνομα με την οποία Azure μπορούν να αναφέρονται σε αυτό και επίσης να καθορίσετε το πρόθεμα χώρου διεύθυνσης για την πύλη τοπικού δικτύου. 

Azure χρησιμοποιεί το πρόθεμα διεύθυνση IP που καθορίζετε για να προσδιορίσετε ποια κυκλοφορία για αποστολή στη θέση σας εσωτερικής εγκατάστασης. Αυτό σημαίνει ότι πρέπει να καθορίσετε κάθε πρόθεμα διεύθυνσης που θέλετε να συσχετίζονται με την πύλη τοπικού δικτύου. Μπορείτε εύκολα να ενημερώσετε αυτά τα προθέματα εάν αλλάξει το δίκτυο εσωτερικής εγκατάστασης. 

Όταν χρησιμοποιείτε τα παραδείγματα PowerShell, λάβετε υπόψη τα εξής:
    
- Το *GatewayIPAddress* είναι η διεύθυνση IP της συσκευής σας VPN εσωτερικής εγκατάστασης. Συσκευή VPN δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT. 
- Το *AddressPrefix* είναι ο χώρος διεύθυνση εσωτερικής εγκατάστασης.

Για να προσθέσετε μια πύλη τοπικό δίκτυο με πρόθεμα μία διεύθυνση:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Για να προσθέσετε μια πύλη τοπικού δικτύου με πολλαπλές προθέματα διεύθυνση:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>Για να τροποποιήσετε προθέματα διεύθυνση IP για την πύλη τοπικού δικτύου

Μερικές φορές αλλάξει το τοπικό δίκτυο προθέματα πύλης. Τα βήματα που κάνετε για να τροποποιήσετε το προθέματα διεύθυνση IP εξαρτώνται από το εάν έχετε δημιουργήσει μια σύνδεση VPN πύλης. Ανατρέξτε στην ενότητα [προθέματα διεύθυνση IP τροποποίηση μιας πύλης τοπικό δίκτυο](#modify) αυτού του άρθρου.


## <a name="PublicIP"></a>4. αίτηση για μια δημόσια διεύθυνση IP για την πύλη VPN

Στη συνέχεια, να ζητήσετε μια δημόσια διεύθυνση IP που θα εκχωρηθεί για την πύλη Azure VNet VPN. Δεν είναι η ίδια διεύθυνση IP που εκχωρείται στη συσκευή σας VPN; προτιμάτε εκχωρείται για την πύλη Azure VPN ίδια. Δεν μπορείτε να καθορίσετε τη διεύθυνση IP που θέλετε να χρησιμοποιήσετε. Εκχωρείται δυναμικά για την πύλη. Χρησιμοποιήστε αυτήν τη διεύθυνση IP κατά τη ρύθμιση των παραμέτρων σας συσκευή VPN εσωτερικής εγκατάστασης για να συνδεθείτε στην πύλη.

Η πύλη Azure VPN για το μοντέλο ανάπτυξης διαχείρισης πόρων προς το παρόν υποστηρίζει μόνο δημόσιες διευθύνσεις IP, χρησιμοποιώντας τη μέθοδο δυναμική εκχώρηση. Ωστόσο, αυτό δεν σημαίνει ότι θα αλλάξει τη διεύθυνση IP. Ο χρόνος μόνο την αλλαγή της διεύθυνσης IP πύλης Azure VPN είναι όταν έχει διαγραφεί και εκ νέου δημιουργία της πύλης. Δημόσια διεύθυνση IP της πύλης δεν θα αλλάξει σε αλλαγή μεγέθους, επαναφορά ή άλλες εσωτερικές συντήρηση/αναβαθμίσεις από την πύλη Azure VPN.

Χρησιμοποιήστε το παρακάτω δείγμα PowerShell:

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. Δημιουργήστε τη ρύθμιση διευθύνσεων IP πύλης

Η ρύθμιση παραμέτρων της πύλης ορίζει το υποδίκτυο και στη δημόσια διεύθυνση IP για να χρησιμοποιήσετε. Χρησιμοποιήστε το παρακάτω παράδειγμα για τη δημιουργία της ρύθμισης παραμέτρων της πύλης.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. Δημιουργία της πύλης εικονικού δικτύου

Σε αυτό το βήμα, δημιουργείτε την πύλη εικονικού δικτύου. Δημιουργία μιας πύλης μπορεί να διαρκέσει μεγάλο χρονικό διάστημα για να ολοκληρωθεί. Συχνά 45 λεπτά ή περισσότερα. 

Χρησιμοποιήστε τις ακόλουθες τιμές:

- Το *-GatewayType* για μια ρύθμιση παραμέτρων-τοποθεσίας είναι *Vpn*. Ο τύπος πύλης είναι πάντα ειδικά για τη ρύθμιση παραμέτρων που εφαρμόζετε. Για παράδειγμα, άλλες ρυθμίσεις παραμέτρων πύλης μπορεί να απαιτεί - GatewayType ExpressRoute. 

- Το *-VpnType* μπορεί να είναι *RouteBased* (αναφέρεται ως μια δυναμική πύλη σε ορισμένα έγγραφα) ή *PolicyBased* (αναφέρεται ως στατική πύλη σε ορισμένες τεκμηρίωση). Για περισσότερες πληροφορίες σχετικά με τους τύπους πύλης VPN, ανατρέξτε στο θέμα [Σχετικά με τις πύλες VPN](vpn-gateway-about-vpngateways.md#vpntype).
- Το *-GatewaySku* μπορεί να είναι *βασικές*, *τυπικό*ή *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. ρυθμίσετε τη συσκευή σας VPN

Σε αυτό το σημείο, θα χρειαστείτε στη δημόσια διεύθυνση IP της πύλης εικονικού δικτύου για τη ρύθμιση των παραμέτρων σας συσκευή VPN εσωτερικής εγκατάστασης. Εργασία με τον κατασκευαστή της συσκευής για συγκεκριμένες πληροφορίες. Μπορείτε να ανατρέξετε στις [Συσκευές VPN](vpn-gateway-about-vpn-devices.md) για περισσότερες πληροφορίες.

Για να βρείτε στη δημόσια διεύθυνση IP σας πύλης εικονικού δικτύου, χρησιμοποιήστε το παρακάτω παράδειγμα:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. δημιουργήσετε τη σύνδεση VPN

Στη συνέχεια, να δημιουργήσετε τη σύνδεση VPN τοποθεσίας σε τοποθεσία μεταξύ της πύλης εικονικού δικτύου και τη συσκευή σας VPN. Φροντίστε να αντικαταστήσετε τις τιμές με το δικό σας. Το κοινόχρηστο κλειδί πρέπει να συμφωνεί με την τιμή που χρησιμοποιήσατε για τη ρύθμιση παραμέτρων συσκευή VPN. Σημειώστε ότι η `-ConnectionType` για--τοποθεσίας είναι *ασφαλείας IP*. 

Ορίστε τις μεταβλητές.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Δημιουργία της σύνδεσης.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Πρόκειται να εγκατασταθούν μετά από μια σύντομη κατά τη σύνδεση. 

## <a name="toverify"></a>Για να επιβεβαιώσετε μια σύνδεση VPN

Υπάρχουν μερικά διαφορετικούς τρόπους για να επιβεβαιώσετε τη σύνδεση VPN.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>Για να τροποποιήσετε προθέματα διεύθυνση IP για μια πύλη τοπικού δικτύου

Εάν θέλετε να αλλάξετε τα προθέματα για την πύλη τοπικό δίκτυο, χρησιμοποιήστε τις παρακάτω οδηγίες. Παρέχονται δύο σύνολα οδηγίες. Τις οδηγίες που επιλέγετε εξαρτώνται από το εάν έχετε ήδη δημιουργήσει τη σύνδεση της πύλης. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Για να τροποποιήσετε τη διεύθυνση IP πύλης για μια πύλη τοπικού δικτύου

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Επόμενα βήματα

- Μπορείτε να προσθέσετε εικονικές μηχανές εικονικού δίκτυά σας. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- Για πληροφορίες σχετικά με το πρωτόκολλο BGP, ανατρέξτε στο θέμα το [Πρωτόκολλο BGP Επισκόπηση](vpn-gateway-bgp-overview.md) και [πώς μπορείτε να ρυθμίσετε τις παραμέτρους πρωτόκολλο BGP](vpn-gateway-bgp-resource-manager-ps.md).


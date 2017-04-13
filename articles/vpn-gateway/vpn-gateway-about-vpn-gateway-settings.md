<properties 
   pageTitle="Σχετικά με τις ρυθμίσεις πύλης VPN για πύλες εικονικού δικτύου | Microsoft Azure"
   description="Μάθετε σχετικά με τις ρυθμίσεις πύλης VPN Azure εικονικού δικτύου."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>Σχετικά με τις ρυθμίσεις πύλης VPN

Μια λύση σύνδεσης πύλης VPN εξαρτάται από τη ρύθμιση παραμέτρων του πολλούς πόρους για να στείλετε την κίνηση του δικτύου μεταξύ εικονικών δικτύων και εσωτερικής θέσεις. Κάθε πόρο περιέχει ρυθμίσεις. Ο συνδυασμός των πόρων και των ρυθμίσεων Καθορίζει το αποτέλεσμα σύνδεσης.

Οι ενότητες σε αυτό το άρθρο περιγράφουν τις ρυθμίσεις που σχετίζονται με μια πύλη VPN στο μοντέλο ανάπτυξης **Διαχείρισης πόρων** και τους πόρους. Ίσως σας φανεί χρήσιμο να προβάλετε τις διαθέσιμες παραμέτρους χρησιμοποιώντας διαγράμματα τοπολογία σύνδεσης. Μπορείτε να βρείτε το περιγραφές και τοπολογίας διαγράμματα για κάθε λύση σύνδεσης στο άρθρο [Σχετικά με την πύλη VPN](vpn-gateway-about-vpngateways.md) . 

## <a name="gwtype"></a>Τύποι πύλης

Κάθε εικονικού δικτύου μπορεί να έχει μόνο μία πύλη εικονικού δικτύου κάθε τύπο. Όταν δημιουργείτε μια πύλη εικονικού δικτύου, πρέπει να βεβαιωθείτε ότι ο τύπος πύλης είναι σωστές για τη ρύθμιση παραμέτρων.

Οι διαθέσιμες τιμές για - GatewayType είναι: 

- VPN
- ExpressRoute

Μια πύλη VPN απαιτεί το `-GatewayType` *Vpn*.  

Παράδειγμα:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>SKU πύλης


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Ρύθμιση παραμέτρων της πύλης SKU

**Καθορισμός της πύλης SKU στην πύλη του Azure**

Εάν χρησιμοποιείτε την πύλη του Azure για να δημιουργήσετε μια πύλη εικονικού δικτύου για τη διαχείριση πόρων, μπορείτε να επιλέξετε την πύλη SKU, χρησιμοποιώντας την αναπτυσσόμενη λίστα. Οι επιλογές σας δίνονται αντιστοιχούν τον τύπο πύλη και τον τύπο VPN που επιλέγετε.

Για παράδειγμα, εάν επιλέξετε τον τύπο πύλη 'VPN' και το VPN τύπος 'βασίζεται σε πολιτική', βλέπετε μόνο το 'Βασική' SKU καθώς αυτό είναι το μόνο διαθέσιμο για PolicyBased VPN SKU. Εάν επιλέξετε 'Δρομολόγηση βάσει', μπορείτε να επιλέξετε από Basic, τυπική και HighPerformance SKU. 


**Καθορισμός της πύλης SKU χρήση του PowerShell**


Το παρακάτω παράδειγμα PowerShell Καθορίζει το `-GatewaySku` ως *πρότυπο*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Αλλαγή μιας πύλης SKU**

Εάν θέλετε να κάνετε αναβάθμιση της πύλης SKU σε μια πιο ισχυρή SKU (από τη βασική/πρότυπο για να HighPerformance) μπορείτε να χρησιμοποιήσετε το `Resize-AzureRmVirtualNetworkGateway` cmdlet του PowerShell. Μπορείτε επίσης να υποβιβάσετε της πύλης χρησιμοποιώντας αυτό το cmdlet μέγεθος SKU.

Το παρακάτω παράδειγμα PowerShell εμφανίζει μια πύλη SKU αλλαγμένο μέγεθος για να HighPerformance.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Εκτιμώμενη συγκεντρωτικών αποτελεσμάτων μετάδοσης από πύλη SKU και πληκτρολογήστε

Ο παρακάτω πίνακας εμφανίζει τους τύπους πύλης και την εκτιμώμενη απόδοση συγκεντρωτικών αποτελεσμάτων. Αυτός ο πίνακας ισχύει για το τόσο τη διαχείριση πόρων και τα μοντέλα κλασική ανάπτυξης.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Τύποι συνδέσεων

Στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, κάθε ρύθμιση παραμέτρων απαιτεί έναν τύπο σύνδεσης πύλης συγκεκριμένες εικονικού δικτύου. Οι τιμές για το διαθέσιμο PowerShell για τη διαχείριση πόρων `-ConnectionType` είναι:

- Ασφαλείας IP
- Vnet2Vnet
- ExpressRoute
- VPNClient

Στο παρακάτω παράδειγμα PowerShell, μπορούμε να δημιουργήσουμε μια σύνδεση S2S που απαιτεί ο τύπος σύνδεσης *ασφαλείας IP*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>Τύποι VPN

Κατά τη δημιουργία της πύλης εικονικού δικτύου για μια ρύθμιση παραμέτρων της πύλης VPN, πρέπει να καθορίσετε έναν τύπο VPN. Ο τύπος VPN που επιλέγετε εξαρτάται από την τοπολογία σύνδεσης που θέλετε να δημιουργήσετε. Για παράδειγμα, μια σύνδεση P2S απαιτεί έναν τύπο RouteBased VPN. Ένας τύπος VPN επίσης να εξαρτώνται από το υλικό που θα χρησιμοποιήσετε. Ρυθμίσεις παραμέτρων S2S απαιτούν μια συσκευή VPN. Ορισμένες συσκευές VPN υποστηρίζει μόνο ένα συγκεκριμένο τύπο VPN.

Τον τύπο VPN που επιλέγετε πρέπει να πληροί όλες τις σύνδεσης απαιτήσεις για τη λύση που θέλετε να δημιουργήσετε. Για παράδειγμα, εάν θέλετε να δημιουργήσετε μια σύνδεση S2S VPN πύλης και μια σύνδεση P2S VPN πύλης για το ίδιο εικονικό δίκτυο, θα χρησιμοποιούσατε VPN τύπος *RouteBased* επειδή P2S απαιτεί RouteBased VPN τύπο. Θα πρέπει επίσης να επαληθεύσετε ότι η συσκευή σας VPN υποστηρίζεται μια σύνδεση RouteBased VPN. 

Μετά τη δημιουργία μιας πύλης εικονικού δικτύου, δεν μπορείτε να αλλάξετε τον τύπο VPN. Πρέπει να διαγράψετε την πύλη εικονικού δικτύου και να δημιουργήσετε ένα νέο. Υπάρχουν δύο τύποι VPN:

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Το παρακάτω παράδειγμα PowerShell Καθορίζει το `-VpnType` ως *RouteBased*. Όταν δημιουργείτε μια πύλη, πρέπει να βεβαιωθείτε ότι το VpnType - είναι σωστές για τη ρύθμιση παραμέτρων. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Απαιτήσεις πύλης

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Υποδίκτυο πύλης

Για να ρυθμίσετε τις παραμέτρους μιας πύλης εικονικού δικτύου, πρέπει πρώτα να δημιουργήσετε ένα υποδίκτυο πύλης για το VNet. Το υποδίκτυο πύλης πρέπει να ονομάζεται *GatewaySubnet* για να λειτουργήσει σωστά. Αυτό το όνομα σας επιτρέπει να γνωρίζετε ότι αυτό το υποδίκτυο πρέπει να χρησιμοποιηθεί για την πύλη Azure.

Το ελάχιστο μέγεθος του σας υποδίκτυο πύλης εξαρτάται πλήρως από τη ρύθμιση παραμέτρων που θέλετε να δημιουργήσετε. Αν και είναι δυνατή η δημιουργία μιας πύλης υποδικτύου όσο /29, συνιστάται να δημιουργήσετε ένα υποδίκτυο πύλης /28 ή μεγαλύτερο (/ 28, /27, /26, κ.λπ.). 

Δημιουργία σε μεγαλύτερο μέγεθος πύλης δεν σας επιτρέπει να από την εκτέλεση σε σχέση με περιορισμοί μεγέθους πύλης. Για παράδειγμα, ίσως έχετε δημιουργήσει μια πύλη εικονικού δικτύου με μέγεθος υποδικτύου πύλης /29 για μια σύνδεση S2S. Μπορείτε τώρα θέλετε να ρυθμίσετε τις παραμέτρους μιας S2S/ExpressRoute συνυπάρχουν ρύθμισης παραμέτρων. Ότι η ρύθμιση παραμέτρων απαιτεί πύλης υποδικτύου ελάχιστο μέγεθος /28. Για να δημιουργήσετε τη ρύθμιση των παραμέτρων σας, θα πρέπει να τροποποιήσετε το υποδίκτυο πύλης για να χωρέσει η ελάχιστη απαίτηση για τη σύνδεση, το οποίο είναι /28.

Το παρακάτω παράδειγμα του PowerShell για τη διαχείριση πόρων εμφανίζει ένα υποδίκτυο πύλης με την ονομασία GatewaySubnet. Μπορείτε να δείτε τη σημειογραφία CIDR καθορίζει μια /27, που επιτρέπει την αρκετές διευθύνσεις IP για τις περισσότερες ρυθμίσεις που υπάρχει αυτήν τη στιγμή.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Τοπικό δίκτυο πυλών

Κατά τη δημιουργία μιας ρύθμισης παραμέτρων πύλης VPN, της πύλης τοπικού δικτύου συχνά αντιπροσωπεύει τη θέση εσωτερικής εγκατάστασης. Στο μοντέλο κλασική ανάπτυξης, η πύλη τοπικό δίκτυο έχει αναφέρονται ως μια τοπική τοποθεσία. 

Δώστε την πύλη τοπικό δίκτυο ένα όνομα, τη δημόσια διεύθυνση IP της συσκευής VPN εσωτερικής εγκατάστασης, και καθορίστε τα προθέματα διεύθυνση που βρίσκονται στη θέση του εσωτερικής εγκατάστασης. Azure εξετάζει τα προθέματα διεύθυνσης προορισμού για την κίνηση δικτύου, χρήστη ελέγχει τη ρύθμιση παραμέτρων που έχετε καθορίσει για την πύλη τοπικό δίκτυο και δρομολογεί πακέτα αντίστοιχα. Μπορείτε επίσης να καθορίσετε πύλες τοπικού δικτύου για ρυθμίσεις παραμέτρων VNet-VNet που χρησιμοποιούν μια σύνδεση VPN πύλης.

Το παρακάτω παράδειγμα PowerShell δημιουργεί μια νέα πύλη τοπικού δικτύου:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Μερικές φορές πρέπει να τροποποιήσετε τις ρυθμίσεις πύλης τοπικού δικτύου. Για παράδειγμα, όταν προσθέτετε ή να τροποποιείτε περιοχή διευθύνσεων, ή εάν η διεύθυνση IP της συσκευής VPN αλλάζει. Για μια κλασική VNet, μπορείτε να αλλάξετε αυτές τις ρυθμίσεις στην πύλη του κλασική στη σελίδα τοπικά δίκτυα. Για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Τροποποίηση ρυθμίσεων πύλης τοπικό δίκτυο με χρήση του PowerShell](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>Cmdlet του REST API και PowerShell

Για επιπλέον πόρους τεχνική και σύνταξη συγκεκριμένες απαιτήσεις κατά τη χρήση REST API και cmdlet του PowerShell για ρυθμίσεις παραμέτρων πύλης VPN, ανατρέξτε στις ακόλουθες σελίδες:

|**Κλασικό** | **Διαχείριση πόρων**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τις ρυθμίσεις παραμέτρων διαθέσιμη σύνδεση, ανατρέξτε στο θέμα [Σχετικά με την πύλη VPN](vpn-gateway-about-vpngateways.md) . 







 

<properties
   pageTitle="Πώς μπορείτε να ρυθμίσετε τις παραμέτρους των συνδέσεων VPN S2S ενεργούς με Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε ρύθμιση παραμέτρων συνδέσεων ενεργούς με Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Ρύθμιση παραμέτρων συνδέσεων S2S VPN ενεργούς με Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε ενεργούς σταυρό εσωτερικής εγκατάστασης και συνδέσεις VNet-VNet χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και PowerShell.


**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Σχετικά με τις συνδέσεις ιδιαίτερα διαθέσιμη σταυρό εσωτερικής εγκατάστασης

Για να επιτύχετε υψηλής διαθεσιμότητας για διασταυρούμενο εσωτερικής εγκατάστασης και συνδεσιμότητας VNet-VNet, πρέπει να αναπτύξετε πολλές πύλες VPN και να δημιουργήσετε πολλαπλές παράλληλες συνδέσεις μεταξύ σας δίκτυα και του Azure. Ανατρέξτε στο θέμα [ιδιαίτερα διαθέσιμη σταυρό εσωτερικής εγκατάστασης και συνδεσιμότητας VNet-VNet](./vpn-gateway-highlyavailable.md) για μια επισκόπηση της επιλογές συνδεσιμότητας και τοπολογίας.

Σε αυτό το άρθρο παρέχει τις οδηγίες για να ρυθμίσετε μια σύνδεση VPN ενεργούς σταυρό εσωτερικής εγκατάστασης και ενεργούς σύνδεσης μεταξύ δύο εικονικού δικτύων:

- [Τμήμα 1 - Δημιουργία και ρύθμιση παραμέτρων της πύλης Azure VPN στη λειτουργία ενεργούς](#aagateway)

- [Μέρος 2 - Δημιουργία συνδέσεων μεταξύ ενεργούς εσωτερικής εγκατάστασης](#aacrossprem)

- [Μέρος 3 - Δημιουργία ενεργούς VNet-VNet συνδέσεων](#aav2v)

- [Μέρος 4 - Ενημέρωση υπάρχουσας πύλης μεταξύ ενεργούς και ενεργό-αναμονή](#aaupdate)

Μπορείτε να συνδυάσετε τα εξής για να δημιουργήσετε μια τοπολογία πιο σύνθετες, ιδιαίτερα διαθέσιμο δίκτυο που ανταποκρίνεται στις ανάγκες σας.

>[AZURE.IMPORTANT] Έχετε υπόψη ότι η κατάσταση ενεργούς λειτουργεί μόνο σε HighPerformance SKU


## <a name ="aagateway"></a>Τμήμα 1 - Δημιουργία και ρύθμιση παραμέτρων ενεργούς VPN πυλών

Ακολουθήστε τα παρακάτω βήματα θα ρυθμίσετε τις παραμέτρους της πύλης Azure VPN σε ενεργούς λειτουργίες. Οι βασικές διαφορές μεταξύ των πυλών ενεργούς και ενεργό αναμονής:

- Πρέπει να δημιουργήσετε δύο ρυθμίσεις παραμέτρων IP πύλης με δύο δημόσιων διευθύνσεων IP
- Χρειάζεστε μπορείτε να ορίσετε τη σημαία EnableActiveActiveFeature
- Η πύλη SKU πρέπει να είναι HighPerformance

Οι άλλες ιδιότητες είναι ίδια όπως και οι πύλες ενεργή ενεργή. 

### <a name="before-you-begin"></a>Πριν ξεκινήσετε

- Βεβαιωθείτε ότι έχετε μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).
    
- Θα χρειαστεί να εγκαταστήσετε το cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Βήμα 1 - Δημιουργία και ρύθμιση παραμέτρων VNet1

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Για αυτήν την άσκηση, ας ξεκινήσουμε με δήλωση μας μεταβλητές. Το παρακάτω παράδειγμα δηλώνει τις μεταβλητές χρησιμοποιώντας τις τιμές για αυτήν την άσκηση. Φροντίστε να αντικαταστήσετε τις τιμές με το δικό σας όταν ρυθμίζετε τις παραμέτρους για την παραγωγή. Μπορείτε να χρησιμοποιήσετε αυτές τις μεταβλητές εάν εκτελείτε τα βήματα για να εξοικειωθείτε με αυτόν τον τύπο της ρύθμισης παραμέτρων. Τροποποιήστε τις μεταβλητές, και, στη συνέχεια, αντιγράψτε και επικολλήστε στην κονσόλα του PowerShell.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. συνδεθείτε με τη συνδρομή σας και να δημιουργήσετε μια νέα ομάδα πόρων

Βεβαιωθείτε ότι μπορείτε Μετάβαση σε κατάσταση λειτουργίας PowerShell για να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. Δημιουργία TestVNet1

Το παρακάτω δείγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα TestVNet1 και τρία δευτερεύοντα δίκτυα, μία που ονομάζεται GatewaySubnet, μία που ονομάζεται προσκήνιο και έναν που ονομάζεται παρασκηνίου. Κατά την αντικατάσταση τιμών, είναι σημαντικό θα ονομάσετε πάντα σας υποδίκτυο πύλης συγκεκριμένα GatewaySubnet. Εάν έχετε ονομάστε το κάτι άλλο, Δημιουργία πύλης σας θα αποτύχει. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Βήμα 2 - Δημιουργία της πύλης VPN για TestVNet1 με ενεργούς λειτουργία

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. Δημιουργία δημόσιας διευθύνσεις IP και πύλης ρυθμίσεων IP

Αίτηση δύο δημόσιες διευθύνσεις IP που θα εκχωρηθεί για την πύλη που θα δημιουργήσετε για το VNet. Μπορείτε, επίσης, θα ορίσετε το υποδίκτυο και ρυθμίσεις παραμέτρων IP απαιτείται. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. Δημιουργία της πύλης VPN με ενεργούς ρύθμισης παραμέτρων

Δημιουργία της πύλης εικονικού δικτύου για το TestVNet1. Σημειώστε ότι υπάρχουν δύο εγγραφές GatewayIpConfig και έχει οριστεί η σημαία EnableActiveActiveFeature. Η λειτουργία ενεργούς απαιτεί μια πύλη VPN δρομολόγηση βάσει των HighPerformance SKU. Δημιουργία μιας πύλης μπορεί να διαρκέσει λίγο (30 λεπτά ή περισσότερο για να ολοκληρώσετε).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. αποκτήσετε την πύλη δημόσιες διευθύνσεις IP και τη διεύθυνση IP το πρωτόκολλο BGP ομότιμης σύνδεσης

Όταν δημιουργηθεί η πύλη, θα πρέπει να αποκτήσετε τη διεύθυνση IP ομότιμης σύνδεσης το πρωτόκολλο BGP της πύλης VPN Azure. Αυτή η διεύθυνση είναι απαραίτητη για τη ρύθμιση παραμέτρων της πύλης VPN Azure ως ένα πρωτόκολλο BGP ομότιμης σύνδεσης για τις συσκευές VPN εσωτερικής εγκατάστασης.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Χρησιμοποιήστε τα ακόλουθα cmdlet για να εμφανίσετε τις δύο δημόσια διευθύνσεις IP που έχουν εκχωρηθεί για την πύλη VPN και διευθύνσεις IP ομότιμης σύνδεσης το πρωτόκολλο BGP αντίστοιχο για κάθε παρουσία πύλης:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Τη σειρά των τη δημόσια IP διευθύνσεις για τις παρουσίες πύλης και οι αντίστοιχες διευθύνσεις διεισδύουν το πρωτόκολλο BGP είναι η ίδια. Σε αυτό το παράδειγμα, η πύλη Εικονική με δημόσια IP της 40.112.190.5 θα χρησιμοποιήσει 10.12.255.4 ως το πρωτόκολλο BGP διεισδύουν διεύθυνση και της πύλης με 138.91.156.129 θα χρησιμοποιήσει 10.12.255.5. Αυτές οι πληροφορίες είναι απαραίτητος κατά τη ρύθμιση του στην εσωτερική εγκατάσταση VPN τις συσκευές σας τη σύνδεση με την πύλη ενεργούς. Η πύλη εμφανίζεται στο παρακάτω διάγραμμα με όλες τις διευθύνσεις:

![η πύλη ενεργούς](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Όταν δημιουργηθεί η πύλη, μπορείτε να χρησιμοποιήσετε αυτήν την πύλη για να δημιουργήσετε σύνδεση VNet-VNet ή ενεργούς σταυρό εσωτερικής εγκατάστασης. Οι παρακάτω ενότητες θα δείτε τα βήματα για να ολοκληρώσετε την άσκηση.


## <a name ="aacrossprem"></a>Μέρος 2 - να δημιουργήσετε μια σύνδεση μεταξύ ενεργούς εσωτερικής εγκατάστασης

Για να δημιουργήσετε μια σύνδεση μεταξύ εσωτερικής εγκατάστασης, πρέπει να δημιουργήσετε μια τοπική πύλη δικτύου για να αναπαραστήσετε τη συσκευή σας στην εσωτερική εγκατάσταση VPN και μια σύνδεση για να συνδεθείτε στην πύλη Azure VPN με την πύλη του τοπικού δικτύου. Σε αυτό το παράδειγμα, η πύλη Azure VPN είναι σε λειτουργία ενεργούς. Ως αποτέλεσμα, ακόμα κι αν υπάρχει μόνο μία εσωτερικής εγκατάστασης και πόρων μία σύνδεση VPN συσκευής (πύλη τοπικό δίκτυο), και οι δύο παρουσίες πύλης Azure VPN θα δημιουργήσουν σήραγγες S2S VPN με τη συσκευή εσωτερικής εγκατάστασης.

Πριν να συνεχίσετε, βεβαιωθείτε ότι έχετε ολοκληρώσει [μέρος 1](#aagateway) αυτής της άσκησης.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Βήμα 1 - Δημιουργία και ρύθμιση παραμέτρων της πύλης τοπικού δικτύου

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Άσκηση αυτό θα εξακολουθήσει να δημιουργήσετε τη ρύθμιση παραμέτρων που εμφανίζεται στο διάγραμμα. Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Μερικά πράγματα που πρέπει να λάβετε υπόψη σχετικά με τις παραμέτρους της πύλης τοπικού δικτύου:

- Η πύλη τοπικού δικτύου μπορεί να είναι στο ίδιο ή σε διαφορετική θέση και ομάδα πόρων ως πύλη VPN. Αυτό το παράδειγμα εμφανίζει τα σε διαφορετικές ομάδες πόρων, αλλά στην ίδια θέση Azure.

- Εάν υπάρχει μόνο μία συσκευή VPN εσωτερικής εγκατάστασης, όπως φαίνεται παραπάνω, η σύνδεση ενεργούς να εργαστείτε με ή χωρίς το πρωτόκολλο BGP πρωτόκολλο. Αυτό το παράδειγμα χρησιμοποιεί το πρωτόκολλο BGP για τη σύνδεση μεταξύ εσωτερικής εγκατάστασης.

- Εάν το πρωτόκολλο BGP είναι ενεργοποιημένη, το πρόθεμα πρέπει να δηλώσετε για την πύλη τοπικού δικτύου είναι η διεύθυνση κεντρικού υπολογιστή από τη διεύθυνση IP ομότιμης σύνδεσης το πρωτόκολλο BGP στη συσκευή σας VPN. Σε αυτήν την περίπτωση, είναι μια /32 πρόθεμα "10.52.255.253/32".

- Ως υπενθύμιση, πρέπει να χρησιμοποιήσετε διαφορετικό πρωτόκολλο BGP ASNs μεταξύ σας δίκτυα εσωτερικής εγκατάστασης και του Azure VNet. Εάν είναι η ίδια, πρέπει να αλλάξετε το ASN VNet, εάν η συσκευή VPN εσωτερικής εγκατάστασης χρησιμοποιεί ήδη το ASN σύνδεση με άλλους γείτονες το πρωτόκολλο BGP.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Δημιουργία πύλης τοπικού δικτύου για το Site5
    
Πριν να συνεχίσετε, βεβαιωθείτε ότι είστε ακόμα συνδεδεμένοι με τη συνδρομή 1. Δημιουργήστε την ομάδα των πόρων, εάν δεν έχει δημιουργήσει ακόμα.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Βήμα 2 - συνδέστε την πύλη VNet και τοπικό δίκτυο πύλης

#### <a name="1-get-the-two-gateways"></a>1. λάβετε δύο πυλών

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Δημιουργήστε το TestVNet1 Site5 σύνδεση

Σε αυτό το βήμα, θα δημιουργήσετε τη σύνδεση από TestVNet1 σε Site5_1 με "EnableBGP" ρύθμιση για $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN και το πρωτόκολλο BGP παραμέτρους για τη συσκευή σας στην εσωτερική εγκατάσταση VPN

Το παρακάτω παράδειγμα παραθέτει τις παραμέτρους θα εισαγάγετε στην ενότητα Ρύθμιση παραμέτρων το πρωτόκολλο BGP στη συσκευή σας VPN εσωτερικής εγκατάστασης για αυτήν την άσκηση:

    - Site5 ASN: 65050
    - Site5 το πρωτόκολλο BGP IP: 10.52.255.253
    - Προθέματα ανακοινώνουμε: (για παράδειγμα) 10.51.0.0/16 και 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure IP το πρωτόκολλο BGP VNet 1: 10.12.255.4 για σωλήνα για να 40.112.190.5
    - Azure IP το πρωτόκολλο BGP VNet 2: 10.12.255.5 για σωλήνα για να 138.91.156.129
    - Στατικές διαδρομές: 10.12.255.4/32 προορισμού, nexthop της διοχέτευσης VPN περιβάλλοντος εργασίας για να 40.112.190.5 10.12.255.5/32 προορισμού, nexthop της διοχέτευσης VPN περιβάλλοντος εργασίας για να 138.91.156.129
    - eBGP Multihop: Βεβαιωθείτε ότι η επιλογή "multihop" για eBGP είναι ενεργοποιημένη στη συσκευή σας, εάν είναι απαραίτητο

Θα πρέπει να δυνατή η σύνδεση μετά από μερικά λεπτά, και την περίοδο λειτουργίας peering το πρωτόκολλο BGP θα ξεκινήσει μόλις πραγματοποιηθεί η σύνδεση ασφαλείας IP. Αυτό το παράδειγμα μέχρι στιγμής έχει ρυθμίσει τις παραμέτρους μία μόνο συσκευή VPN εσωτερικής εγκατάστασης, που προκύπτουν στο διάγραμμα φαίνεται παρακάτω:

![ενεργό-ενεργό-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Βήμα 3 - σύνδεση δύο συσκευές VPN εσωτερικής εγκατάστασης για την πύλη VPN ενεργούς

Εάν έχετε δύο συσκευές VPN στο ίδιο δίκτυο εσωτερικής εγκατάστασης, μπορείτε να επιτύχετε διπλή πλεονασμού κατά τη σύνδεση της πύλης Azure VPN στη δεύτερη συσκευή VPN.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Δημιουργία της δεύτερης πύλης τοπικού δικτύου για το Site5

Σημειώστε ότι η διεύθυνση IP πύλης πρόθημα διεύθυνσης και το πρωτόκολλο BGP διεισδύουν διεύθυνση για το δεύτερο πύλη τοπικού δικτύου δεν πρέπει να επικαλύπτονται με την προηγούμενη πύλη τοπικού δικτύου για το ίδιο δίκτυο εσωτερικής εγκατάστασης. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. Συνδέστε την πύλη VNet και της δεύτερης πύλης τοπικού δικτύου

Δημιουργία της σύνδεσης από TestVNet1 Site5_2 με "EnableBGP" ρύθμιση για $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN και το πρωτόκολλο BGP παραμέτρους για τη δεύτερη συσκευή VPN εσωτερικής εγκατάστασης

Ομοίως, κάτω από λίστες τις παραμέτρους θα εισαγάγετε στη δεύτερη συσκευή VPN:

    - Site5 ASN: 65050
    - Site5 το πρωτόκολλο BGP IP: 10.52.255.254
    - Προθέματα ανακοινώνουμε: (για παράδειγμα) 10.51.0.0/16 και 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure IP το πρωτόκολλο BGP VNet 1: 10.12.255.4 για σωλήνα για να 40.112.190.5
    - Azure IP το πρωτόκολλο BGP VNet 2: 10.12.255.5 για σωλήνα για να 138.91.156.129
    - Στατικές διαδρομές: 10.12.255.4/32 προορισμού, nexthop της διοχέτευσης VPN περιβάλλοντος εργασίας για να 40.112.190.5 10.12.255.5/32 προορισμού, nexthop της διοχέτευσης VPN περιβάλλοντος εργασίας για να 138.91.156.129
    - eBGP Multihop: Βεβαιωθείτε ότι η επιλογή "multihop" για eBGP είναι ενεργοποιημένη στη συσκευή σας, εάν είναι απαραίτητο

Όταν έχουν δημιουργηθεί η σύνδεση (μήκους), μπορείτε να χρησιμοποιείτε διπλή πλεονάζοντα συσκευές VPN και σήραγγες τη σύνδεση το δίκτυο εσωτερικής εγκατάστασης και Azure:

![διπλής-πλεονασμού-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Μέρος 3 - να δημιουργήσετε μια σύνδεση VNet-VNet ενεργούς

Αυτή η ενότητα δημιουργεί μια σύνδεση VNet-VNet ενεργούς με το πρωτόκολλο BGP. 

Συνεχίστε τις παρακάτω οδηγίες από τα προηγούμενα βήματα που αναφέρονται παραπάνω. Πρέπει να ολοκληρώσετε [μέρος 1](#aagateway) για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους TestVNet1 και την πύλη VPN με το πρωτόκολλο BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Βήμα 1 - Δημιουργία TestVNet2 και την πύλη VPN

Είναι σημαντικό για να βεβαιωθείτε ότι το χώρο διευθύνσεων IP του νέου εικονικού δικτύου, TestVNet2, δεν συμπίπτει με οποιαδήποτε από τις περιοχές VNet.

Σε αυτό το παράδειγμα, το εικονικό δίκτυα ανήκουν την ίδια συνδρομή. Μπορείτε να ρυθμίσετε τις συνδέσεις VNet-VNet μεταξύ διάφορες συνδρομές; ανατρέξτε στην [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet](./vpn-gateway-vnet-vnet-rm-ps.md) για να μάθετε περισσότερες λεπτομέρειες. Βεβαιωθείτε ότι μπορείτε να προσθέσετε το "-EnableBgp $True" κατά τη δημιουργία τις συνδέσεις για να ενεργοποιήσετε το πρωτόκολλο BGP.

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Δημιουργήστε TestVNet2 στη νέα ομάδα πόρων

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. Δημιουργία της πύλης VPN ενεργούς για TestVNet2

Αίτηση δύο δημόσιων διευθύνσεων IP που θα εκχωρηθεί για την πύλη που θα δημιουργήσετε για το VNet. Μπορείτε, επίσης, θα ορίσετε το υποδίκτυο και ρυθμίσεις παραμέτρων IP απαιτείται. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Δημιουργία της πύλης VPN με τον αριθμό AS και τη σημαία "EnableActiveActiveFeature". Σημειώστε ότι πρέπει να μπορείτε να αντικαταστήσετε την προεπιλεγμένη ASN στην σας Azure VPN πύλες. Το ASNs για το συνδεδεμένο VNets πρέπει να είναι διαφορετικές για να ενεργοποιήσετε το πρωτόκολλο BGP και δρομολόγηση τη μεταφορά.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Βήμα 2 - σύνδεση TestVNet1 και TestVNet2 πυλών

Σε αυτό το παράδειγμα, και οι δύο πύλες είναι στην ίδια συνδρομή. Μπορείτε να ολοκληρώσετε αυτό το βήμα κατά την ίδια περίοδο λειτουργίας PowerShell.

#### <a name="1-get-both-gateways"></a>1. λάβετε και τις δύο πυλών

Βεβαιωθείτε ότι συνδέεστε και συνδεθείτε με τη συνδρομή 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Δημιουργία δύο συνδέσεων

Σε αυτό το βήμα, θα δημιουργήσετε τη σύνδεση από TestVNet1 σε TestVNet2 καθώς και τη σύνδεση από TestVNet2 σε TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Φροντίστε να ενεργοποιήσετε το πρωτόκολλο BGP για τις ΔΎΟ συνδέσεις.

Αφού ολοκληρώσετε αυτά τα βήματα, θα να δημιουργήσετε τη σύνδεση σε λίγα λεπτά και το πρωτόκολλο BGP διεισδύουν περιόδου λειτουργίας θα είναι προς τα επάνω, μόλις ολοκληρωθεί η σύνδεση VNet-VNet με διπλή πλεονασμού:

![ενεργό-ενεργό-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Μέρος 4 - Ενημέρωση υπάρχουσας πύλης μεταξύ ενεργούς και ενεργό-αναμονή

Η τελευταία ενότητα θα περιγράφει πώς μπορείτε να ρυθμίσετε μια υπάρχουσα πύλη Azure VPN από το ενεργό αναμονής ενεργούς λειτουργία ή το αντίστροφο.

>[AZURE.IMPORTANT] Έχετε υπόψη ότι η κατάσταση ενεργούς λειτουργεί μόνο σε HighPerformance SKU

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Ρύθμιση παραμέτρων μιας πύλης αναμονής ενεργός στην πύλη ενεργούς

#### <a name="1-gateway-parameters"></a>1. παράμετροι πύλης

Το παρακάτω παράδειγμα μετατρέπει μια πύλη ενεργό αναμονής σε μια πύλη ενεργούς. Πρέπει να δημιουργήσετε μια άλλη δημόσια διεύθυνση IP και, στη συνέχεια, προσθέστε μια δεύτερη ρύθμιση IP πύλης. Κάτω από το εμφανίζει τις παραμέτρους που χρησιμοποιούνται:

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2. Δημιουργήστε στη δημόσια διεύθυνση IP και, στη συνέχεια, προσθέστε τη δεύτερη ρύθμιση παραμέτρων IP πύλης

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. ενεργοποιήστε τη λειτουργία ενεργούς και ενημερώστε την πύλη

Πρέπει να ορίσετε το αντικείμενο πύλης σε PowerShell για να ενεργοποιήσετε την πραγματική ενημερωμένη έκδοση. Το SKU του αντικειμένου πύλης πρέπει να αλλάξουν επίσης HighPerformance από τότε που δημιουργήθηκε προηγουμένως ως πρότυπο.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Αυτή η ενημέρωση μπορεί να διαρκέσει 30 έως 45 λεπτά.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Ρύθμιση παραμέτρων μιας πύλης ενεργούς στην πύλη ενεργό την αναμονή

#### <a name="1-gateway-parameters"></a>1. παράμετροι πύλης

Χρησιμοποιήστε τις ίδιες παραμέτρους όπως παραπάνω, λάβετε το όνομα της ρύθμισης παραμέτρων IP που θέλετε να καταργήσετε.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. Καταργήστε τη ρύθμιση παραμέτρων IP πύλης και να απενεργοποιήσετε τη λειτουργία ενεργούς

Ομοίως, πρέπει να ορίσετε το αντικείμενο πύλης σε PowerShell για να ενεργοποιήσετε την πραγματική ενημερωμένη έκδοση.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Αυτή η ενημερωμένη έκδοση μπορεί να χρειαστεί έως 30 45 λεπτά.


## <a name="next-steps"></a>Επόμενα βήματα

Μόλις ολοκληρωθεί η σύνδεσή σας, μπορείτε να προσθέσετε εικονικές μηχανές στα δίκτυά σας εικονικού. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .


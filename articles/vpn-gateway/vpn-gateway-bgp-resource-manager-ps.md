<properties
   pageTitle="Πώς μπορείτε να ρυθμίσετε το πρωτόκολλο BGP σε Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε ρύθμιση παραμέτρων το πρωτόκολλο BGP με Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Πώς μπορείτε να ρυθμίσετε το πρωτόκολλο BGP σε Azure πύλες VPN χρήση της διαχείρισης πόρων Azure και PowerShell

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να ενεργοποιήσετε το πρωτόκολλο BGP σε μια σύνδεση VPN-τοποθεσίας (S2S) σταυρό εσωτερικής εγκατάστασης και μια σύνδεση VNet-VNet χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και PowerShell.


**Σχετικά με τα μοντέλα Azure ανάπτυξης**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>Σχετικά με το πρωτόκολλο BGP

Το πρωτόκολλο BGP είναι το τυπικό πρωτόκολλο δρομολόγησης που χρησιμοποιούνται συνήθως στο Internet για την ανταλλαγή πληροφοριών δρομολόγησης και της δυνατότητας πρόσβασης μεταξύ των δύο ή περισσότερα δίκτυα. Το πρωτόκολλο BGP ενεργοποιεί το Azure πύλες VPN και εσωτερικής VPN τις συσκευές σας, που ονομάζεται τους συνεργάτες το πρωτόκολλο BGP ή γείτονες, σε "δρομολογεί" του exchange που θα σας ενημερώσει για δύο πύλες στη διαθεσιμότητα και τη δυνατότητα πρόσβασης σε αυτά τα προθέματα να μεταβείτε μέσω των πυλών ή δρομολογητές που εμπλέκονται. Το πρωτόκολλο BGP επίσης να ενεργοποιήσετε τη μεταφορά δρομολόγηση μεταξύ πολλών δικτύων μετάδοση δρομολογεί μια πύλη το πρωτόκολλο BGP μαθαίνει από ένα πρωτόκολλο BGP ομότιμης σύνδεσης σε όλες τις άλλες τους το πρωτόκολλο BGP συνεργάτες.

Ανατρέξτε στο θέμα [Επισκόπηση των το πρωτόκολλο BGP με Azure πύλες VPN](./vpn-gateway-bgp-overview.md) για περαιτέρω ανάλυση οφέλη από το πρωτόκολλο BGP και για να κατανοήσετε τις τεχνικές απαιτήσεις και τα θέματα του χρησιμοποιώντας το πρωτόκολλο BGP.

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Γρήγορα αποτελέσματα με το πρωτόκολλο BGP σε Azure VPN πύλες

Σε αυτό το άρθρο θα σας καθοδηγήσει τα βήματα για να πραγματοποιήσετε τις ακόλουθες εργασίες:

- [Τμήμα 1 - ενεργοποίηση το πρωτόκολλο BGP σας πύλης Azure VPN](#enablebgp)

- [Μέρος 2 - δημιουργούν μια σύνδεση μεταξύ εσωτερικής εγκατάστασης με το πρωτόκολλο BGP](#crossprembgp)

- [Μέρος 3 - δημιουργούν μια σύνδεση VNet-VNet με το πρωτόκολλο BGP](#v2vbgp)

Κάθε τμήμα του παραθύρου οδηγιών αποτελεί βασικό μπλοκ δόμησης για ενεργοποίηση το πρωτόκολλο BGP στην η συνδεσιμότητα του δικτύου σας. Εάν δεν μπορείτε να ολοκληρώσετε τα τρία τμήματα, θα μπορείτε να δημιουργήσετε την τοπολογία, όπως φαίνεται στο ακόλουθο διάγραμμα:

![Το πρωτόκολλο BGP τοπολογίας](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Μπορείτε να συνδυάσετε τα εξής για να δημιουργήσετε ένα δίκτυο πιο σύνθετες, πολλών Ελπίζουμε, τη μεταφορά που ανταποκρίνεται στις ανάγκες σας.

## <a name ="enablebgp"></a>Μέρος 1 - ρύθμιση παραμέτρων το πρωτόκολλο BGP της πύλης Azure VPN

Ακολουθήστε τα παρακάτω βήματα ρύθμισης παραμέτρων θα το πρωτόκολλο BGP παράμετροι εγκατάστασης της πύλης Azure VPN όπως φαίνεται στο ακόλουθο διάγραμμα:

![Το πρωτόκολλο BGP πύλης](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Πριν ξεκινήσετε

- Βεβαιωθείτε ότι έχετε μια συνδρομή του Azure. Εάν δεν έχετε ήδη μια συνδρομή του Azure, μπορείτε να ενεργοποιήσετε το [MSDN συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ή το σύμβολο του για έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/).
    
- Θα χρειαστεί να εγκαταστήσετε το cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

### <a name="step-1---create-and-configure-vnet1"></a>Βήμα 1 - Δημιουργία και ρύθμιση παραμέτρων VNet1 

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Για αυτήν την άσκηση, ας ξεκινήσουμε με δήλωση μας μεταβλητές. Το παρακάτω παράδειγμα δηλώνει τις μεταβλητές χρησιμοποιώντας τις τιμές για αυτήν την άσκηση. Φροντίστε να αντικαταστήσετε τις τιμές με το δικό σας όταν ρυθμίζετε τις παραμέτρους για την παραγωγή. Μπορείτε να χρησιμοποιήσετε αυτές τις μεταβλητές εάν εκτελείτε τα βήματα για να εξοικειωθείτε με αυτόν τον τύπο της ρύθμισης παραμέτρων. Τροποποιήστε τις μεταβλητές, και, στη συνέχεια, αντιγράψτε και επικολλήστε στην κονσόλα του PowerShell.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. συνδεθείτε με τη συνδρομή σας και να δημιουργήσετε μια νέα ομάδα πόρων

Βεβαιωθείτε ότι κάνετε Μετάβαση σε κατάσταση λειτουργίας PowerShell για να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

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

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Βήμα 2 - Δημιουργία της πύλης VPN για TestVNet1 με το πρωτόκολλο BGP παραμέτρους

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. Δημιουργήστε τις ρυθμίσεις παραμέτρων IP και δευτερεύον

Αίτηση για μια δημόσια διεύθυνση IP που θα εκχωρηθεί για την πύλη που θα δημιουργήσετε για το VNet. Μπορείτε, επίσης, θα ορίσετε το υποδίκτυο και ρυθμίσεις παραμέτρων IP απαιτείται. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2. Δημιουργία της πύλης VPN με τον αριθμό AS

Δημιουργία της πύλης εικονικού δικτύου για το TestVNet1. Σημειώστε ότι το πρωτόκολλο BGP απαιτεί μια πύλη βάσει δρομολόγηση VPN, καθώς και την παράμετρο πρόσθεση, - Asn, για να ορίσετε το ASN (αριθμός AS) για TestVNet1. Δημιουργία μιας πύλης μπορεί να διαρκέσει λίγο (30 λεπτά ή περισσότερο για να ολοκληρώσετε).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. λήψη της διεύθυνσης IP του Azure το πρωτόκολλο BGP ομότιμης σύνδεσης

Όταν δημιουργηθεί η πύλη, θα πρέπει να αποκτήσετε τη διεύθυνση IP ομότιμης σύνδεσης το πρωτόκολλο BGP της πύλης VPN Azure. Αυτή η διεύθυνση είναι απαραίτητη για τη ρύθμιση παραμέτρων της πύλης VPN Azure ως ένα πρωτόκολλο BGP ομότιμης σύνδεσης για τις συσκευές VPN εσωτερικής εγκατάστασης.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Την τελευταία εντολή θα εμφανίσει την αντίστοιχη διαμόρφωση το πρωτόκολλο BGP της πύλης VPN Azure; Για παράδειγμα:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Όταν δημιουργηθεί η πύλη, μπορείτε να χρησιμοποιήσετε αυτήν την πύλη για να δημιουργήσετε σύνδεση VNet-VNet με το πρωτόκολλο BGP ή σταυρό εσωτερικής εγκατάστασης. Οι παρακάτω ενότητες θα δείτε τα βήματα για να ολοκληρώσετε την άσκηση.

## <a name ="crossprembbgp"></a>Μέρος 2 - δημιουργούν μια σύνδεση μεταξύ εσωτερικής εγκατάστασης με το πρωτόκολλο BGP

Για να δημιουργήσετε μια σύνδεση μεταξύ εσωτερικής εγκατάστασης, πρέπει να δημιουργήσετε μια τοπική πύλη δικτύου για να αναπαραστήσετε τη συσκευή σας στην εσωτερική εγκατάσταση VPN και μια σύνδεση για να συνδεθείτε στην πύλη Azure VPN με την πύλη του τοπικού δικτύου. Η διαφορά μεταξύ τις οδηγίες σε αυτό το άρθρο είναι οι πρόσθετες ιδιότητες που απαιτούνται για να καθορίσετε τη ρύθμιση παραμέτρων το πρωτόκολλο BGP.

![Το πρωτόκολλο BGP για διασταυρούμενο εσωτερικής εγκατάστασης](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Πριν να συνεχίσετε, βεβαιωθείτε ότι έχετε ολοκληρώσει [μέρος 1](#enablebgp) αυτής της άσκησης.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Βήμα 1 - Δημιουργία και ρύθμιση παραμέτρων της πύλης τοπικού δικτύου

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Άσκηση αυτό θα εξακολουθήσει να δημιουργήσετε τη ρύθμιση παραμέτρων που εμφανίζεται στο διάγραμμα. Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Μερικά πράγματα που πρέπει να λάβετε υπόψη σχετικά με τις παραμέτρους της πύλης τοπικού δικτύου:

- Η πύλη τοπικού δικτύου μπορεί να είναι στο ίδιο ή σε διαφορετική θέση και ομάδα πόρων ως πύλη VPN. Αυτό το παράδειγμα εμφανίζει τα σε διαφορετικές ομάδες πόρων σε διαφορετικές θέσεις.

- Το ελάχιστο πρόθεμα πρέπει να δηλώσετε για την πύλη τοπικού δικτύου είναι η διεύθυνση κεντρικού υπολογιστή από τη διεύθυνση IP ομότιμης σύνδεσης το πρωτόκολλο BGP στη συσκευή σας VPN. Σε αυτήν την περίπτωση, είναι μια /32 πρόθεμα "10.52.255.254/32".

- Ως υπενθύμιση, πρέπει να χρησιμοποιήσετε διαφορετικό πρωτόκολλο BGP ASNs μεταξύ σας δίκτυα εσωτερικής εγκατάστασης και του Azure VNet. Εάν είναι η ίδια, πρέπει να αλλάξετε το ASN VNet εάν συσκευή VPN εσωτερικής εγκατάστασης χρησιμοποιούν ήδη το ASN σύνδεση με άλλους γείτονες το πρωτόκολλο BGP.
    
Πριν να συνεχίσετε, βεβαιωθείτε ότι είστε ακόμα συνδεδεμένοι με τη συνδρομή 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Δημιουργία πύλης τοπικού δικτύου για το Site5

Φροντίστε να δημιουργήσετε την ομάδα πόρων, εάν αυτό δεν έχει δημιουργηθεί, πριν από τη δημιουργία της πύλης του τοπικού δικτύου. Παρατηρήστε τις δύο πρόσθετες παραμέτρους για την πύλη τοπικού δικτύου: Asn και BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Βήμα 2 - συνδέστε την πύλη VNet και τοπικό δίκτυο πύλης

#### <a name="1-get-the-two-gateways"></a>1. λάβετε δύο πυλών

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2. Δημιουργήστε το TestVNet1 Site5 σύνδεση

Σε αυτό το βήμα, θα δημιουργήσετε τη σύνδεση από TestVNet1 σε Site5. Πρέπει να καθορίσετε "-EnableBGP $True" για να ενεργοποιήσετε το πρωτόκολλο BGP για αυτήν τη σύνδεση. Όπως περιγράφεται παραπάνω, είναι δυνατό να έχετε τόσο το πρωτόκολλο BGP και μη πρωτόκολλο BGP συνδέσεις για την ίδια πύλη VPN Azure. Εκτός εάν το πρωτόκολλο BGP είναι ενεργοποιημένη στην ιδιότητα σύνδεσης, Azure δεν θα ενεργοποιήσει το πρωτόκολλο BGP για αυτήν τη σύνδεση Παρόλο που το πρωτόκολλο BGP παράμετροι έχουν ήδη ρυθμιστεί σε δύο πυλών.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Το παρακάτω παράδειγμα παραθέτει τις παραμέτρους θα εισαγάγετε στην ενότητα Ρύθμιση παραμέτρων το πρωτόκολλο BGP στη συσκευή σας VPN εσωτερικής εγκατάστασης για αυτήν την άσκηση:

    - Site5 ASN: 65050
    - Site5 το πρωτόκολλο BGP IP: 10.52.255.254
    - Προθέματα ανακοινώνουμε: (για παράδειγμα) 10.51.0.0/16 και 10.52.0.0/16
    - Azure VNet ASN: 65010
    - Azure VNet το πρωτόκολλο BGP IP: 10.12.255.30
    - Στατική διαδρομή: Προσθέστε μια διαδρομή για 10.12.255.30/32, με nexthop που στο περιβάλλον διοχέτευσης VPN στη συσκευή σας
    - eBGP Multihop: Βεβαιωθείτε ότι η επιλογή "multihop" για eBGP είναι ενεργοποιημένη στη συσκευή σας, εάν είναι απαραίτητο

Θα πρέπει να δυνατή η σύνδεση μετά από μερικά λεπτά, και την περίοδο λειτουργίας peering το πρωτόκολλο BGP θα ξεκινήσει μόλις πραγματοποιηθεί η σύνδεση ασφαλείας IP.
 
## <a name ="v2vbgp"></a>Μέρος 3 - δημιουργούν μια σύνδεση VNet-VNet με το πρωτόκολλο BGP

Αυτή η ενότητα προσθέτει μια σύνδεση VNet-VNet με το πρωτόκολλο BGP, όπως φαίνεται στο παρακάτω διάγραμμα. 

![Το πρωτόκολλο BGP για VNet-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Συνεχίστε τις παρακάτω οδηγίες από τα προηγούμενα βήματα που αναφέρονται παραπάνω. Πρέπει να ολοκληρώσετε [μέρος ι](#enablebgp) για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους TestVNet1 και την πύλη VPN με το πρωτόκολλο BGP. 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Βήμα 1 - Δημιουργία TestVNet2 και την πύλη VPN

Είναι σημαντικό για να βεβαιωθείτε ότι το χώρο διευθύνσεων IP του νέου εικονικού δικτύου, TestVNet2, δεν συμπίπτει με οποιαδήποτε από τις περιοχές VNet.

Σε αυτό το παράδειγμα, το εικονικό δίκτυα ανήκουν την ίδια συνδρομή. Μπορείτε να ρυθμίσετε VNet-VNet συνδέσεων μεταξύ διάφορες συνδρομές; ανατρέξτε στην [Ρύθμιση παραμέτρων σύνδεσης VNet-VNet](./vpn-gateway-vnet-vnet-rm-ps.md) για να μάθετε περισσότερες λεπτομέρειες. Βεβαιωθείτε ότι μπορείτε να προσθέσετε το "-EnableBgp $True" κατά τη δημιουργία τις συνδέσεις για να ενεργοποιήσετε το πρωτόκολλο BGP.

#### <a name="1-declare-your-variables"></a>1. να δηλώσετε τις μεταβλητές

Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2. Δημιουργήστε TestVNet2 στη νέα ομάδα πόρων

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. Δημιουργία της πύλης VPN για TestVNet2 με το πρωτόκολλο BGP παραμέτρους

Αίτηση για μια δημόσια διεύθυνση IP που θα εκχωρηθεί για την πύλη που θα δημιουργήσετε για το VNet. Μπορείτε, επίσης, θα ορίσετε το υποδίκτυο και ρυθμίσεις παραμέτρων IP απαιτείται. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Δημιουργία της πύλης VPN με τον αριθμό AS. Σημειώστε ότι πρέπει να μπορείτε να αντικαταστήσετε την προεπιλεγμένη ASN στην σας Azure VPN πύλες. Το ASNs για το συνδεδεμένο VNets πρέπει να είναι διαφορετικές για να ενεργοποιήσετε το πρωτόκολλο BGP και δρομολόγηση τη μεταφορά.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Βήμα 2 - σύνδεση TestVNet1 και TestVNet2 πυλών

Σε αυτό το παράδειγμα, και οι δύο πύλες είναι στην ίδια συνδρομή. Μπορείτε να ολοκληρώσετε αυτό το βήμα κατά την ίδια περίοδο λειτουργίας PowerShell.

#### <a name="1-get-both-gateways"></a>1. λάβετε και τις δύο πυλών

Βεβαιωθείτε ότι η σύνδεση και συνδεθείτε με τη συνδρομή 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. Δημιουργία δύο συνδέσεων

Σε αυτό το βήμα, θα δημιουργήσετε τη σύνδεση από TestVNet1 σε TestVNet2 καθώς και τη σύνδεση από TestVNet2 σε TestVNet1.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Φροντίστε να ενεργοποιήσετε το πρωτόκολλο BGP για τις ΔΎΟ συνδέσεις.

Αφού ολοκληρώσετε αυτά τα βήματα, η σύνδεση θα δημιουργήσουν σε λίγα λεπτά και θα την περίοδο λειτουργίας peering το πρωτόκολλο BGP του μία φορά στη σύνδεση VNet-VNet έχει ολοκληρωθεί.

Εάν έχετε ολοκληρώσει τα τρία τμήματα του αυτήν την άσκηση, θα έχετε καθορίσει μια τοπολογία του δικτύου όπως φαίνεται παρακάτω:

![Το πρωτόκολλο BGP για VNet-VNet](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Επόμενα βήματα

Μόλις ολοκληρωθεί η σύνδεσή σας, μπορείτε να προσθέσετε εικονικές μηχανές στα δίκτυά σας εικονικού. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .


<properties
   pageTitle="Σύνδεση Azure VNets με πύλης VPN και PowerShell | Microsoft Azure"
   description="Σε αυτό το άρθρο σάς καθοδηγεί σε σύνδεση εικονικού δικτύων μαζί με τη χρήση διαχείριση πόρων Azure και PowerShell."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Ρύθμιση παραμέτρων σύνδεσης VNet-VNet για διαχείριση πόρων με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Διαχείριση πόρων - Azure πύλη](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Διαχείριση πόρων - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Κλασικό - κλασική πύλη](virtual-networks-configure-vnet-to-vnet-connection.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε μια σύνδεση μεταξύ VNets στο μοντέλο ανάπτυξης διαχείρισης πόρων με τη χρήση πύλη VPN. Το εικονικό δίκτυα μπορεί να είναι στις ίδιες ή σε διαφορετικές περιοχές και από τις ίδιες ή σε διαφορετικές συνδρομές.


![διάγραμμα v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Μοντέλα ανάπτυξης και οι μέθοδοι για τις συνδέσεις VNet-VNet

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Ο παρακάτω πίνακας εμφανίζει τα μοντέλα αυτήν τη στιγμή διαθέσιμα ανάπτυξης και οι μέθοδοι για διαμορφώσεις VNet-VNet. Όταν ένα άρθρο με βήματα ρύθμισης παραμέτρων είναι διαθέσιμη, θα σας σύνδεση απευθείας σε αυτήν από αυτόν τον πίνακα.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>Διεισδύουν VNet

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>Σχετικά με τις συνδέσεις VNet-VNet

Σύνδεση ένα εικονικό δίκτυο με κάποιο άλλο εικονικό δίκτυο (VNet-VNet) είναι παρόμοια με τη σύνδεση ενός VNet σε μια θέση τοποθεσία εσωτερικής εγκατάστασης. Και οι δύο τύποι συνδεσιμότητας Χρησιμοποιήστε μια πύλη Azure VPN για να παρέχουν μια ασφαλής διοχέτευση χρησιμοποιώντας ασφαλείας IP/IKE. Μπορεί να είναι η VNets μπορείτε να συνδεθείτε σε διαφορετικές περιοχές. Ή στις διάφορες συνδρομές. Μπορείτε να συνδυάσετε ακόμα VNet-VNet επικοινωνία με ρυθμίσεις παραμέτρων πολλών τοποθεσιών. Σας επιτρέπει να δημιουργήσετε τοπολογίες δικτύου που συνδυάζουν σταυρό εσωτερικής εγκατάστασης συνδεσιμότητα με συνδεσιμότητας εξαρτήσεις μεταξύ εικονικού δικτύου, όπως φαίνεται στο ακόλουθο διάγραμμα:


![Σχετικά με τις συνδέσεις](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Γιατί να συνδεθώ εικονικών δικτύων;

Ενδέχεται να θέλετε να συνδεθείτε εικονικών δικτύων για τους ακόλουθους λόγους:

- **Περιοχή διασταύρωσης παν-πλεονασμού και παν παρουσίας**
    - Μπορείτε να ρυθμίσετε τη δική σας παν αναπαραγωγής ή ο συγχρονισμός με ασφαλή σύνδεση χωρίς να μεταβείτε επάνω από τα τελικά σημεία μέσω Internet.
    - Με το Azure κίνηση Manager και εξισορρόπηση φόρτου, μπορείτε να ρυθμίσετε ιδιαίτερα διαθέσιμη φόρτο εργασίας με παν πλεονασμού σε πολλές περιοχές Azure. Ένα παράδειγμα σημαντικό είναι να ρυθμίσετε SQL πάντα σε με διαθεσιμότητα ομάδες που σε πολλές περιοχές Azure.

- **Τοπικές εφαρμογές πολλαπλών επιπέδων με απομόνωσης ή διαχείρισης όριο**
    - Μέσα στην ίδια περιοχή, μπορείτε να ρυθμίσετε εφαρμογές πολλαπλών επιπέδων με πολλά εικονικού δίκτυα συνδεδεμένα μεταξύ τους λόγω απομόνωσης ή τις απαιτήσεις διαχείρισης.


### <a name="vnet-to-vnet-faq"></a>Συνήθεις Ερωτήσεις VNet-VNet

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Ποιο σύνολο βημάτων πρέπει να χρησιμοποιήσω;

Σε αυτό το άρθρο, μπορείτε να δείτε δύο διαφορετικά σύνολα βημάτων. Ένα σύνολο βημάτων για [VNets που βρίσκονται στην ίδια συνδρομή](#samesub)και ένα άλλο για [VNets που βρίσκονται σε διάφορες συνδρομές](#difsub). Η βασική διαφορά μεταξύ των συνόλων είναι εάν μπορείτε να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους όλα εικονικού δικτύου και τους πόρους πύλης μέσα στην ίδια περίοδο λειτουργίας PowerShell.

Τα βήματα σε αυτό το άρθρο χρησιμοποιούν μεταβλητές που έχουν δηλωθεί στην αρχή κάθε ενότητας. Εάν ήδη εργάζεστε με υπάρχουσα VNets, τροποποιήστε τις μεταβλητές ώστε να αντικατοπτρίζει τις ρυθμίσεις στο δικό σας περιβάλλον. 

![Και οι δύο συνδέσεις](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Πώς να συνδεθείτε VNets που βρίσκονται στην ίδια συνδρομή

![διάγραμμα v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Πριν ξεκινήσετε
    
Προτού ξεκινήσετε, πρέπει να εγκαταστήσετε το cmdlet του PowerShell για τη διαχείριση πόρων Azure. Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση τα cmdlet του PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md) .

### <a name="Step1"></a>Βήμα 1 - Σχεδιασμός τις περιοχές διευθύνσεων IP


Στα παρακάτω βήματα, μπορούμε να δημιουργήσουμε δύο εικονικών δικτύων μαζί με τις αντίστοιχες πύλης δευτερεύοντα δίκτυα και ρυθμίσεις παραμέτρων. Στη συνέχεια, μπορούμε να δημιουργήσουμε μια σύνδεση VPN μεταξύ του δύο VNets. Είναι σημαντικό να προγραμματίζετε τις περιοχές διευθύνσεων IP για τις παραμέτρους δικτύου. Έχετε υπόψη ότι πρέπει να βεβαιωθείτε ότι καμία από τις VNet περιοχές ή περιοχές τοπικό δίκτυο επικαλύπτονται με οποιονδήποτε τρόπο.

Χρησιμοποιούμε τις παρακάτω τιμές στα παραδείγματα:

**Τιμές για TestVNet1:**

- Όνομα VNet: TestVNet1
- Ομάδα πόρων: TestRG1
- Θέση: Ανατολικής η.π.α.
- TestVNet1: 10.11.0.0/16 & 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Παρασκηνίου: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- Διακομιστή DNS: 8.8.8.8
- GatewayName: VNet1GW
- Δημόσια IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- Τύπος σύνδεσης: VNet2VNet


**Τιμές για TestVNet4:**

- Όνομα VNet: TestVNet4
- TestVNet2: 10.41.0.0/16 & 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Παρασκηνίου: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Ομάδα πόρων: TestRG4
- Θέση: Δυτική η.π.α.
- Διακομιστή DNS: 8.8.8.8
- GatewayName: VNet4GW
- Δημόσια IP: VNet4GWIP
- VPNType: RouteBased
- Σύνδεση: VNet4toVNet1
- Τύπος σύνδεσης: VNet2VNet



### <a name="Step2"></a>Βήμα 2 - Δημιουργία και ρύθμιση παραμέτρων TestVNet1

1. Να δηλώσετε τις μεταβλητές

    Ξεκινήστε από δηλώνεται μεταβλητές. Αυτό το παράδειγμα δηλώνει τις μεταβλητές χρησιμοποιώντας τις τιμές για αυτήν την άσκηση. Στις περισσότερες περιπτώσεις, θα πρέπει να αντικαταστήσετε τις τιμές με το δικό σας. Ωστόσο, μπορείτε να χρησιμοποιήσετε αυτές τις μεταβλητές εάν εκτελείτε τα βήματα για να εξοικειωθείτε με αυτόν τον τύπο της ρύθμισης παραμέτρων. Τροποποιήστε τις μεταβλητές, εάν είναι απαραίτητο, στη συνέχεια, αντιγράψτε και επικολλήστε τα στο κονσόλα σας PowerShell.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Σύνδεση με τη συνδρομή σας

    Μετάβαση σε κατάσταση λειτουργίας PowerShell για να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων. Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

        Login-AzureRmAccount

    Ελέγξτε τις συνδρομές για το λογαριασμό.

        Get-AzureRmSubscription 

    Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Δημιουργία νέας ομάδας πόρων

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Δημιουργήστε τις ρυθμίσεις παραμέτρων υποδικτύου για TestVNet1

    Αυτό το παράδειγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα TestVNet1 και τρία δευτερεύοντα δίκτυα, μία που ονομάζεται GatewaySubnet, μία που ονομάζεται προσκήνιο και έναν που ονομάζεται παρασκηνίου. Κατά την αντικατάσταση τιμών, είναι σημαντικό θα ονομάσετε πάντα σας υποδίκτυο πύλης συγκεκριμένα GatewaySubnet. Εάν έχετε ονομάστε το κάτι άλλο, Δημιουργία πύλης σας θα αποτύχει. 

    Το παρακάτω παράδειγμα χρησιμοποιεί τις μεταβλητές που ορίσατε προηγουμένως. Σε αυτό το παράδειγμα, το υποδίκτυο πύλης χρησιμοποιεί μια /27. Ενώ είναι δυνατή η δημιουργία μιας πύλης υποδικτύου όσο /29, συνιστάται να δημιουργήσετε ένα μεγαλύτερο υποδίκτυο που περιλαμβάνει περισσότερες διευθύνσεις, επιλέγοντας τουλάχιστον /28 ή /27. Αυτό θα σας επιτρέψει για τις διευθύνσεις αρκετό ώστε να περιλαμβάνει δυνατές πρόσθετες ρυθμίσεις που ίσως θέλετε στο μέλλον. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. Δημιουργία TestVNet1

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Πρόσκληση σε μια δημόσια διεύθυνση IP

    Αίτηση για μια δημόσια διεύθυνση IP που θα εκχωρηθεί για την πύλη που θα δημιουργήσετε για το VNet. Παρατηρήστε ότι το AllocationMethod είναι δυναμική. Δεν μπορείτε να καθορίσετε τη διεύθυνση IP που θέλετε να χρησιμοποιήσετε. Εκχωρείται δυναμικά για την πύλη. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Δημιουργήστε τη ρύθμιση παραμέτρων της πύλης

    Η ρύθμιση παραμέτρων της πύλης ορίζει το υποδίκτυο και στη δημόσια διεύθυνση IP για να χρησιμοποιήσετε. Χρησιμοποιήστε το παράδειγμα για τη δημιουργία της ρύθμισης παραμέτρων της πύλης. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Δημιουργία της πύλης για TestVNet1

    Σε αυτό το βήμα, δημιουργείτε την πύλη εικονικού δικτύου για το TestVNet1. Ρυθμίσεις παραμέτρων VNet-VNet απαιτούν ένα RouteBased VpnType. Δημιουργία μιας πύλης μπορεί να διαρκέσει λίγο (45 λεπτά ή περισσότερα για την ολοκλήρωση).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Βήμα 3 - Δημιουργία και ρύθμιση παραμέτρων TestVNet4

Αφού έχετε ρυθμίσει TestVNet1, δημιουργήσετε TestVNet4. Ακολουθήστε τα βήματα που περιγράφονται παρακάτω, αντικαθιστώντας τις τιμές με το δικό όταν είναι απαραίτητο. Αυτό το βήμα μπορεί να γίνει μέσα στην ίδια περίοδο λειτουργίας PowerShell, επειδή είναι στην ίδια συνδρομή.

1. Να δηλώσετε τις μεταβλητές

    Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Πριν να συνεχίσετε, βεβαιωθείτε ότι είστε ακόμα συνδεδεμένοι με τη συνδρομή 1.

2. Δημιουργία νέας ομάδας πόρων

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Δημιουργήστε τις ρυθμίσεις παραμέτρων υποδικτύου για TestVNet4

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. Δημιουργία TestVNet4

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Πρόσκληση σε μια δημόσια διεύθυνση IP

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Δημιουργήστε τη ρύθμιση παραμέτρων της πύλης

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. Δημιουργία της πύλης TestVNet4

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Βήμα 4 - συνδεθείτε πυλών

1. Λήψη και οι δύο πύλες εικονικού δικτύου

    Σε αυτό το παράδειγμα, επειδή είναι και τα δύο πύλες στην ίδια συνδρομή, αυτό το βήμα μπορεί να ολοκληρωθεί κατά την ίδια περίοδο λειτουργίας PowerShell.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. Δημιουργία του TestVNet1 TestVNet4 σύνδεση

    Σε αυτό το βήμα, δημιουργείτε τη σύνδεση από TestVNet1 σε TestVNet4. Θα δείτε ένα κοινόχρηστο κλειδί στα οποία γίνεται αναφορά στα παραδείγματα. Μπορείτε να χρησιμοποιήσετε τις δικές σας τιμές για το κοινόχρηστο κλειδί. Σημαντικό είναι ότι πρέπει να συμφωνεί με το κοινόχρηστο κλειδί για τις δύο συνδέσεις. Δημιουργία σύνδεσης μπορεί να διαρκέσει λίγο, για να ολοκληρωθεί.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. Δημιουργία του TestVNet4 TestVNet1 σύνδεση

    Αυτό το βήμα είναι παρόμοιο με αυτό που φαίνεται παραπάνω, εκτός από τη δημιουργία της σύνδεσης από TestVNet4 σε TestVNet1. Βεβαιωθείτε ότι τα κοινόχρηστα κλειδιά ταιριάζουν.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Η σύνδεση θα πρέπει να μετά από μερικά λεπτά.

4. Επαληθεύστε τη σύνδεση. Ανατρέξτε στην ενότητα [πώς μπορείτε να επαληθεύσετε τη σύνδεσή σας](#verify).


## <a name="difsub"></a>Πώς να συνδεθείτε VNets που βρίσκονται σε διάφορες συνδρομές


![διάγραμμα v2v](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

Σε αυτό το σενάριο, θα σας σύνδεση TestVNet1 και TestVNet5. TestVNet1 και TestVNet5 βρίσκονται σε μια διαφορετική συνδρομή. Τα βήματα για αυτήν τη ρύθμιση παραμέτρων προσθέστε μια πρόσθετη σύνδεση VNet-VNet προκειμένου να συνδέσετε TestVNet1 στο TestVNet5. 

Η διαφορά εδώ είναι ότι ορισμένα από τα βήματα ρύθμισης παραμέτρων πρέπει να εκτελεστούν σε ξεχωριστή περίοδο λειτουργίας PowerShell στο περιβάλλον της δεύτερης εγγραφής. Ειδικά όταν τις δύο συνδρομές ανήκουν σε διαφορετικές εταιρείες. 

Συνεχίστε τις οδηγίες από τα προηγούμενα βήματα που αναφέρονται παραπάνω. Πρέπει να ολοκληρώσετε [το βήμα 1](#Step1) και [2 βήμα](#Step2) για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους TestVNet1 και την πύλη VPN για TestVNet1. Αφού ολοκληρώσετε το βήμα 1 και το βήμα 2, συνεχίστε με το βήμα 5 για να δημιουργήσετε TestVNet5.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Βήμα 5 - επαλήθευση τα επιπλέον περιοχές διευθύνσεων IP

Είναι σημαντικό για να βεβαιωθείτε ότι το χώρο διευθύνσεων IP του νέου εικονικού δικτύου, TestVNet5, δεν συμπίπτει με οποιαδήποτε VNet περιοχές ή περιοχές πύλης τοπικού δικτύου. 

Σε αυτό το παράδειγμα, το εικονικό δίκτυα μπορεί να ανήκουν σε διαφορετικές εταιρείες. Για αυτήν την άσκηση, μπορείτε να χρησιμοποιήσετε τις παρακάτω τιμές για το TestVNet5:

**Τιμές για TestVNet5:**

- Όνομα VNet: TestVNet5
- Ομάδα πόρων: TestRG5
- Θέση: Ανατολική Ιαπωνία
- TestVNet5: 10.51.0.0/16 & 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Παρασκηνίου: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- Διακομιστή DNS: 8.8.8.8
- GatewayName: VNet5GW
- Δημόσια IP: VNet5GWIP
- VPNType: RouteBased
- Σύνδεση: VNet5toVNet1
- Τύπος σύνδεσης: VNet2VNet

**Πρόσθετες τιμές για TestVNet1:**

- Σύνδεση: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Βήμα 6 - Δημιουργία και ρύθμιση παραμέτρων TestVNet5

Αυτό το βήμα, πρέπει να γίνουν στο περιβάλλον της στη νέα συνδρομή. Αυτό το τμήμα μπορεί να πραγματοποιηθεί από το διαχειριστή σε μια διαφορετική εταιρεία που είναι ο κάτοχος της συνδρομής.

1. Να δηλώσετε τις μεταβλητές

    Φροντίστε να αντικαταστήσετε τις τιμές με αυτά που θέλετε να χρησιμοποιήσετε για τη ρύθμιση παραμέτρων.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Σύνδεση με τη συνδρομή 5

    Ανοίξτε την κονσόλα του PowerShell και συνδεθείτε στο λογαριασμό σας. Χρησιμοποιήστε το παρακάτω παράδειγμα, για να σας βοηθήσει να συνδεθείτε:

        Login-AzureRmAccount

    Ελέγξτε τις συνδρομές για το λογαριασμό.

        Get-AzureRmSubscription 

    Καθορίστε τη συνδρομή στην οποία θέλετε να χρησιμοποιήσετε.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Δημιουργία νέας ομάδας πόρων

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Δημιουργήστε τις ρυθμίσεις παραμέτρων υποδικτύου για TestVNet4
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. Δημιουργία TestVNet5

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Πρόσκληση σε μια δημόσια διεύθυνση IP

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Δημιουργήστε τη ρύθμιση παραμέτρων της πύλης

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. Δημιουργία της πύλης TestVNet5

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Βήμα 7 - σύνδεση πυλών

Σε αυτό το παράδειγμα, επειδή οι πύλες είναι σε διαφορετική τις συνδρομές, θα σας έχετε διαιρέστε αυτού του βήματος σε δύο περιόδους λειτουργίας PowerShell που έχει επισημανθεί ως [συνδρομή 1] και [συνδρομή 5].

1. **[Συνδρομή 1]** Λήψη της πύλης εικονικού δικτύου για τη συνδρομή 1

    Βεβαιωθείτε ότι συνδέεστε και συνδεθείτε με τη συνδρομή 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Αντιγράψτε το αποτέλεσμα από τα ακόλουθα στοιχεία και αποστολή αυτών στο διαχειριστή του 5 συνδρομής μέσω ηλεκτρονικού ταχυδρομείου ή κάποια άλλη μέθοδο.

        $vnet1gw.Name
        $vnet1gw.Id

    Αυτά τα δύο στοιχεία θα έχουν τιμές που είναι παρόμοιο με το ακόλουθο παράδειγμα αποτέλεσμα:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Συνδρομή 5]** Λήψη της πύλης εικονικού δικτύου για τη συνδρομή 5

    Βεβαιωθείτε ότι έχετε σύνδεση στο και συνδεθείτε με τη συνδρομή 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Αντιγράψτε το αποτέλεσμα από τα ακόλουθα στοιχεία και αποστολή αυτών στο διαχειριστή της συνδρομής 1 μέσω ηλεκτρονικού ταχυδρομείου ή κάποια άλλη μέθοδο.

        $vnet5gw.Name
        $vnet5gw.Id

    Αυτά τα δύο στοιχεία θα έχουν τιμές που είναι παρόμοιο με το ακόλουθο παράδειγμα αποτέλεσμα:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Συνδρομή 1]** Δημιουργία του TestVNet1 TestVNet5 σύνδεση

    Σε αυτό το βήμα, δημιουργείτε τη σύνδεση από TestVNet1 σε TestVNet5. Η διαφορά εδώ είναι που vnet5gw $ δεν είναι δυνατή η λήψη απευθείας επειδή είναι σε μια διαφορετική συνδρομή. Θα πρέπει να δημιουργήσετε ένα νέο αντικείμενο PowerShell μαζί με τις τιμές κοινοποιούνται από τη συνδρομή 1 στα παραπάνω βήματα. Χρησιμοποιήστε το παρακάτω παράδειγμα. Αντικαταστήστε το όνομα, το αναγνωριστικό και κλειδί κοινής χρήσης με τις δικές σας τιμές. Σημαντικό είναι ότι πρέπει να συμφωνεί με το κοινόχρηστο κλειδί για τις δύο συνδέσεις. Δημιουργία σύνδεσης μπορεί να διαρκέσει λίγο, για να ολοκληρωθεί.

    Βεβαιωθείτε ότι μπορείτε να συνδεθείτε με τη συνδρομή 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Συνδρομή 5]** Δημιουργία του TestVNet5 TestVNet1 σύνδεση

    Αυτό το βήμα είναι παρόμοιο με αυτό που φαίνεται παραπάνω, εκτός από τη δημιουργία της σύνδεσης από TestVNet5 σε TestVNet1. Η ίδια διαδικασία δημιουργίας ενός αντικειμένου PowerShell με βάση τις τιμές που λαμβάνονται από τη συνδρομή 1 εδώ ισχύει επίσης. Σε αυτό το βήμα, βεβαιωθείτε ότι τα κοινόχρηστα κλειδιά ταιριάζουν.

    Βεβαιωθείτε ότι συνδέεστε με τη συνδρομή 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Τρόπος επιβεβαίωσης σύνδεσης


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Επόμενα βήματα

- Μόλις ολοκληρωθεί η σύνδεσή σας, μπορείτε να προσθέσετε εικονικές μηχανές στα δίκτυά σας εικονικού. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- Για πληροφορίες σχετικά με το πρωτόκολλο BGP, ανατρέξτε στο θέμα το [Πρωτόκολλο BGP Επισκόπηση](vpn-gateway-bgp-overview.md) και [πώς μπορείτε να ρυθμίσετε τις παραμέτρους πρωτόκολλο BGP](vpn-gateway-bgp-resource-manager-ps.md). 


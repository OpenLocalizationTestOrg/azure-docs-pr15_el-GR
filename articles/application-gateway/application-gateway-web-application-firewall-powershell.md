<properties
   pageTitle="Ρύθμιση παραμέτρων του τείχους προστασίας εφαρμογών Web στο νέο ή υπάρχον πύλη εφαρμογής | Microsoft Azure"
   description="Σε αυτό το άρθρο παρέχει οδηγίες σχετικά με τον τρόπο για να ξεκινήσετε να χρησιμοποιείτε το τείχος προστασίας εφαρμογής web σε μια υπάρχουσα ή μια νέα πύλη εφαρμογής."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Ρύθμιση παραμέτρων του τείχους προστασίας εφαρμογής Web σε μια νέα ή υπάρχουσα πύλη εφαρμογής

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-web-application-firewall-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-web-application-firewall-powershell.md)

Το web εφαρμογής τείχος προστασίας (WAF) στην πύλη εφαρμογής Azure προστατεύει εφαρμογές web από κοινές επιθέσεις βασίζεται στο web όπως την εισαγωγή SQL, επιθέσεις δέσμης ενεργειών διατοποθεσιακή και hijacks περιόδου λειτουργίας.

Azure πύλη εφαρμογής είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-7. Παρέχει ανακατεύθυνση, δρομολόγησης επιδόσεων αιτήσεις HTTP ανάμεσα σε διαφορετικούς διακομιστές, είτε είναι στο cloud ή εσωτερικής εγκατάστασης. Εφαρμογή παρέχει πολλές ελεγκτή παράδοσης εφαρμογής (ADC) δυνατότητες όπως εξισορρόπηση φόρτου HTTP συσχέτισης βασίζεται σε cookie περιόδου λειτουργίας, Secure Sockets Layer (SSL) μείωση φόρτου, καθετήρες προσαρμοσμένο εύρυθμης λειτουργίας υποστήριξης για πολλούς τοποθεσίας και πολλά άλλα. Για να βρείτε μια πλήρη λίστα των υποστηριζόμενες δυνατότητες, επισκεφθείτε την επισκόπηση πύλης εφαρμογής

Στο ακόλουθο άρθρο παρουσιάζει πώς μπορείτε να [προσθέσετε τείχος προστασίας εφαρμογής web σε μια υπάρχουσα εφαρμογή πύλη](#add-web-application-firewall-to-an-existing-application-gateway) και να [δημιουργήσετε μια πύλη εφαρμογής που χρησιμοποιεί τείχος προστασίας εφαρμογής web](#create-an-application-gateway-with-web-application-firewall).

![σενάριο εικόνας][scenario]

## <a name="waf-configuration-differences"></a>Διαφορές ρύθμισης παραμέτρων WAF

Εάν έχετε διαβάσει [Δημιουργία μια πύλη εφαρμογής με το PowerShell](application-gateway-create-gateway-arm.md), μπορείτε να κατανοήσετε τις ρυθμίσεις SKU για να ρυθμίσετε τις παραμέτρους κατά τη δημιουργία μια πύλη εφαρμογής. WAF παρέχει πρόσθετες ρυθμίσεις για να ορίσετε κατά τη ρύθμιση παραμέτρων την SKU σε μια πύλη εφαρμογής. Υπάρχουν επιπλέον αλλαγές που κάνετε στην πύλη εφαρμογής ίδια.

**SKU** - μια πύλη κανονική εφαρμογή χωρίς να υποστηρίζει WAF **τυπική\_μικρές**, **τυπική\_Μεσαίο**, και **τυπική\_Large** μεγέθη. Με την εισαγωγή του WAF, υπάρχουν δύο πρόσθετες SKU, **WAF\_Μεσαίο** και **WAF\_Large**. WAF δεν υποστηρίζεται στη μικρή εφαρμογή πυλών.

**Επίπεδο** - οι διαθέσιμες τιμές είναι **Τυπικά** ή **WAF**. Όταν χρησιμοποιείτε το τείχος προστασίας των εφαρμογών Web, πρέπει να επιλέξετε **WAF** .

**Κατάσταση λειτουργίας** - αυτή η ρύθμιση είναι η λειτουργία του WAF. επιτρεπόμενη τιμή είναι **εντοπισμού** και **αποτροπής**. Όταν WAF έχει οριστεί σε λειτουργία εντοπισμού, όλες οι απειλές αποθηκεύονται σε ένα αρχείο καταγραφής. Στη λειτουργία αποτροπής, συμβάντα είστε ακόμα συνδεδεμένοι, αλλά ο εισβολέας λαμβάνει 403 μη εξουσιοδοτημένη απάντηση από την πύλη εφαρμογής.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Προσθήκη τείχος προστασίας εφαρμογής web σε μια υπάρχουσα πύλη εφαρμογής

Βεβαιωθείτε ότι χρησιμοποιείτε την πιο πρόσφατη έκδοση του Azure PowerShell. Περισσότερες πληροφορίες είναι διαθέσιμες στη [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Βήμα 1

Συνδεθείτε στο λογαριασμό σας στο Azure.

    Login-AzureRmAccount

### <a name="step-2"></a>Βήμα 2

Επιλέξτε τη συνδρομή για να χρησιμοποιήσετε για αυτό το σενάριο.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Βήμα 3

Ανακτήστε την πύλη που θέλετε να προσθέσετε το τείχος προστασίας εφαρμογής Web για να.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Βήμα 4

Ρύθμιση παραμέτρων την sku τείχος προστασίας εφαρμογής web. Είναι τα διαθέσιμα μεγέθη **WAF\_Large** και **WAF\_Μεσαίο**. Όταν χρησιμοποιείται τείχος προστασίας εφαρμογής web η σειρά πρέπει να **WAF**, πρέπει να επιβεβαιωθεί η δυναμικότητα όταν ορίζετε την sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Βήμα 5

Ρυθμίστε τις παραμέτρους WAF όπως ορίζεται στο ακόλουθο παράδειγμα:

Για τη ρύθμιση **WafMode** , οι διαθέσιμες τιμές είναι αποτροπής και ανίχνευσης.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Βήμα 6

Ενημερώστε την πύλη εφαρμογής με τις ρυθμίσεις που ορίζονται στο προηγούμενο βήμα.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Αυτή η εντολή ενημερώνει την πύλη εφαρμογής με το τείχος προστασίας εφαρμογής Web. Συνιστάται να προβάλετε [Διαγνωστικά πύλης εφαρμογής](application-gateway-diagnostics.md) για να κατανοήσετε τον τρόπο προβολής των αρχείων καταγραφής για την πύλη εφαρμογής. Λόγω της φύσης ασφαλείας WAF, αρχεία καταγραφής πρέπει να αναθεωρηθούν τακτικά για να κατανοήσετε το στη στάση ασφαλείας από τις εφαρμογές web.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Δημιουργήστε μια πύλη εφαρμογής με το τείχος προστασίας εφαρμογής Web

Ακολουθήστε τα παρακάτω βήματα σας καθοδηγήσει ολόκληρη τη διαδικασία από την αρχή ως το τέλος για να δημιουργήσετε μια πύλη εφαρμογής με το τείχος προστασίας εφαρμογής Web.

Βεβαιωθείτε ότι χρησιμοποιείτε την πιο πρόσφατη έκδοση του Azure PowerShell. Περισσότερες πληροφορίες είναι διαθέσιμες στη [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Βήμα 1

Συνδεθείτε στο Azure

    Login-AzureRmAccount

Θα σας ζητηθεί για τον έλεγχο ταυτότητας με τα διαπιστευτήριά σας.

### <a name="step-2"></a>Βήμα 2

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription

### <a name="step-3"></a>Βήμα 3

Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Βήμα 4

Δημιουργήστε μια ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Αυτή η θέση χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια πύλη εφαρμογής χρησιμοποιεί την ίδια ομάδα πόρων.

Στο προηγούμενο παράδειγμα, που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "appgw-RG" και τη θέση "Δυτική ΜΑΣ".

>[AZURE.NOTE] Εάν θέλετε να ρυθμίσετε τις παραμέτρους ενός προσαρμοσμένου δοκιμή του για την πύλη εφαρμογή, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με προσαρμοσμένο καθετήρες με χρήση του PowerShell](application-gateway-create-probe-ps.md). Ανατρέξτε στο θέμα [προσαρμοσμένες καθετήρες και τον έλεγχο εύρυθμης λειτουργίας](application-gateway-probe-overview.md) για περισσότερες πληροφορίες.

### <a name="step-5"></a>Βήμα 5

Εκχωρήστε μια περιοχή διευθύνσεων για το υποδίκτυο να χρησιμοποιηθεί για την πύλη εφαρμογής ίδια.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Ένα υποδίκτυο για μια εφαρμογή θα πρέπει να έχετε τουλάχιστον των 28 μάσκας bit. Αυτή η τιμή αφήνει 10 διαθέσιμες διευθύνσεις στο δευτερεύον για παρουσίες πυλών εφαρμογής. Με ένα μικρότερο υποδίκτυο, ενδέχεται να μην μπορείτε να προσθέσετε περισσότερες παρουσία σας πύλη εφαρμογής στο μέλλον.

### <a name="step-6"></a>Βήμα 6

Εκχωρήστε μια περιοχή διευθύνσεων που θα χρησιμοποιηθεί για το χώρο συγκέντρωσης διευθύνσεων παρασκηνίου.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Βήμα 7

Δημιουργία ένα εικονικό δίκτυο με το προηγούμενο δευτερεύοντα δίκτυα στην ομάδα πόρων που δημιουργήσατε στο βήμα: [Δημιουργία της ομάδας πόρων](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Βήμα 8

Ανάκτηση του πόρου εικονικού δικτύου και υποδικτύου πόρους που θα χρησιμοποιηθεί με τα παρακάτω βήματα:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Βήμα 9

Δημιουργία δημόσιας πόρου IP που θα χρησιμοποιηθεί για την πύλη εφαρμογής. Αυτή η δημόσια διεύθυνση IP χρησιμοποιείται σε ένα από τα παρακάτω βήματα:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Πύλη εφαρμογής δεν υποστηρίζει τη χρήση του μια δημόσια διεύθυνση IP που δημιουργήθηκε με μια ετικέτα τομέα που καθορίζεται. Υποστηρίζεται μόνο μια δημόσια διεύθυνση IP με μια ετικέτα που έχουν δημιουργηθεί δυναμικά τομέα. Εάν χρειάζεστε ένα όνομα φιλικό dns για την πύλη εφαρμογής, συνιστάται να χρησιμοποιήσετε μια εγγραφή cname ως ψευδώνυμο.

### <a name="step-10"></a>Βήμα 10

Πρέπει να ρυθμίσετε όλα τα στοιχεία ρύθμισης παραμέτρων πριν από τη δημιουργία της εφαρμογής πύλης. Ακολουθήστε τα παρακάτω βήματα Δημιουργήστε τα στοιχεία ρύθμισης παραμέτρων που απαιτούνται για έναν πόρο πύλης εφαρμογής.

Δημιουργήστε μια ρύθμιση εφαρμογής πύλης IP, αυτό ρυθμίζει τις παραμέτρους τι υποδικτύου χρησιμοποιεί η πύλη εφαρμογής. Κατά την εκκίνηση εφαρμογών πύλης, παίρνει μια διεύθυνση IP από το υποδίκτυο έχει ρυθμιστεί και δρομολογεί την κίνηση δικτύου στις διευθύνσεις IP στο χώρο συγκέντρωσης IP παρασκηνίου. Λάβετε υπόψη ότι κάθε παρουσία λαμβάνει μια διεύθυνση IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Βήμα 11

Ρύθμιση παραμέτρων του χώρου συγκέντρωσης διευθύνσεων IP παρασκηνίου με τις διευθύνσεις IP τους διακομιστές web παρασκηνίου. Αυτές οι διευθύνσεις IP είναι οι διευθύνσεις IP που λαμβάνουν την κίνηση δικτύου που προέρχεται από το τελικό σημείο προσκηνίου διευθύνσεων IP. Μπορείτε να αντικαταστήσετε τις ακόλουθες διευθύνσεις IP για να προσθέσετε τη δική σας διεύθυνση IP εφαρμογής τα τελικά σημεία.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Βήμα 12

Αποστείλετε το πιστοποιητικό που θα χρησιμοποιηθεί σχετικά με τους πόρους με δυνατότητα ssl παρασκηνίου χώρου συγκέντρωσης.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Βήμα 13

Ρυθμίστε τις παραμέτρους των ρυθμίσεων της εφαρμογής πύλης http παρασκηνίου. Εκχωρήστε το πιστοποιητικό που έχουν αποσταλεί στο προηγούμενο βήμα για τις ρυθμίσεις http.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Βήμα 14

Ρυθμίστε τις παραμέτρους της προσκηνίου θύρας IP για το δημόσιο τελικό σημείο διευθύνσεων IP. Αυτήν τη θύρα είναι η θύρα που συνδέονται στους τελικούς χρήστες.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Βήμα 15

Δημιουργήστε μια ρύθμιση προσκηνίου IP, αυτή η ρύθμιση αντιστοιχίζει μια διεύθυνση ip ιδιωτικό ή δημόσιο του προσκηνίου της εφαρμογής πύλης. Το παρακάτω βήμα συσχετίζει στη δημόσια διεύθυνση IP στο προηγούμενο βήμα με τη ρύθμιση παραμέτρων προσκηνίου IP.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Βήμα 16

Ρυθμίστε τις παραμέτρους του πιστοποιητικού για την πύλη εφαρμογής. Αυτό το πιστοποιητικό χρησιμοποιείται για να αποκρυπτογραφήσετε και να κρυπτογραφήσετε ξανά την κυκλοφορία της πύλης εφαρμογής.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Βήμα 17

Δημιουργία της ακρόασης HTTP για την πύλη εφαρμογής. Εκχωρήστε το πιστοποιητικό ρύθμισης παραμέτρων, θύρας και ssl προσκηνίου ip για να χρησιμοποιήσετε.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Βήμα 18

Δημιουργήστε έναν κανόνα δρομολόγησης εξισορρόπησης φόρτου που ρυθμίζει τις παραμέτρους της συμπεριφοράς εξισορρόπησης φόρτου. Σε αυτό το παράδειγμα, δημιουργείται ένας κανόνας βασικές round robin.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Βήμα 19

Ρυθμίστε τις παραμέτρους του μεγέθους παρουσία της εφαρμογής πύλης.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Μπορείτε να επιλέξετε μεταξύ **WAF\_Μεσαίο** και **WAF\_Large**, η σειρά όταν χρησιμοποιείτε WAF είναι πάντα **WAF**. Δυναμικότητα είναι οποιοσδήποτε αριθμός μεταξύ 1 και 10.

### <a name="step-20"></a>Βήμα 20

Ρύθμιση παραμέτρων της λειτουργίας για WAF, αποδεκτές τιμές είναι **αποτροπής** και **ανίχνευσης**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Βήμα 21

Δημιουργήστε μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα προηγούμενα βήματα. Σε αυτό το παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

## <a name="get-application-gateway-dns-name"></a>Λήψη εφαρμογής όνομα DNS πύλης

Όταν δημιουργηθεί η πύλη, το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους του υπολογιστή-πελάτη για την επικοινωνία. Όταν χρησιμοποιείτε μια δημόσια IP, πύλη εφαρμογής απαιτεί μια δυναμική αντιστοίχιση όνομα DNS, το οποίο δεν είναι φιλική. Για να τοποθετήστε το δείκτη στο δημόσιο τελικό σημείο της εφαρμογής πύλης μπορεί να χρησιμοποιηθεί για να βεβαιωθείτε ότι οι τελικοί χρήστες μπορούν να πατήσετε την πύλη εφαρμογής μια εγγραφή CNAME. [Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για στο Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Για να κάνετε αυτό, ανακτήσετε λεπτομέρειες σχετικά με την πύλη εφαρμογής και το σχετικό όνομα IP/DNS χρησιμοποιώντας το στοιχείο PublicIPAddress που συνδέονται με την πύλη εφαρμογής. Το όνομα της πύλης εφαρμογή DNS πρέπει να χρησιμοποιείται για να δημιουργήσετε μια εγγραφή CNAME, το οποίο δείχνει τις εφαρμογές web δύο σε αυτό το όνομα DNS. Η χρήση των εγγραφών A δεν συνιστάται, επειδή το VIP ενδέχεται να αλλάξουν κατά την επανεκκίνηση της εφαρμογής πύλης.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους καταγραφή διαγνωστικών, για να συνδεθείτε τα συμβάντα που έχουν εντοπιστεί ή εμπόδισε με τείχος προστασίας εφαρμογής Web με την επίσκεψή σας [Διαγνωστικά πύλης εφαρμογής](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png
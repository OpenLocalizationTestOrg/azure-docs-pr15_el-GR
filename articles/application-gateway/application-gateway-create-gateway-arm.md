<properties
   pageTitle="Δημιουργία, ξεκινήστε ή διαγράψτε μια πύλη εφαρμογής χρησιμοποιώντας τη διαχείριση πόρων Azure | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να δημιουργήσετε, ρύθμιση παραμέτρων, Έναρξη και διαγράψτε μια πύλη Azure εφαρμογής χρησιμοποιώντας τη διαχείριση πόρων Azure"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Δημιουργία, ξεκινήστε ή διαγράψτε μια πύλη εφαρμογής χρησιμοποιώντας τη διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-create-gateway-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-create-gateway-arm.md)
- [Azure κλασική PowerShell](application-gateway-create-gateway.md)
- [Azure προτύπου για τη διαχείριση πόρων](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure πύλη εφαρμογής είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-7. Παρέχει ανακατεύθυνση, δρομολόγησης επιδόσεων αιτήσεις HTTP ανάμεσα σε διαφορετικούς διακομιστές, είτε είναι στο cloud ή εσωτερικής εγκατάστασης. Πύλη εφαρμογής παρέχει πολλές δυνατότητες εφαρμογή παράδοσης ελεγκτή (ADC) συμπεριλαμβανομένων εξισορρόπηση φόρτου HTTP συσχέτισης βασίζεται σε cookie περιόδου λειτουργίας, Secure Sockets Layer (SSL) μείωση φόρτου, καθετήρες προσαρμοσμένο εύρυθμης λειτουργίας υποστήριξης για πολλούς τοποθεσίας και πολλά άλλα. Για να βρείτε μια πλήρη λίστα των υποστηριζόμενες δυνατότητες, επισκεφθείτε την [Επισκόπηση πύλης εφαρμογής](application-gateway-introduction.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε, ρύθμιση παραμέτρων, Έναρξη και να διαγράψετε μια πύλη εφαρμογής.

>[AZURE.IMPORTANT] Για να εργαστείτε με Azure πόρων, είναι σημαντικό να κατανοήσετε ότι Azure έχει αυτήν τη στιγμή δύο μοντέλων ανάπτυξης: Διαχείριση πόρων και κλασική. Βεβαιωθείτε ότι έχετε κατανοήσει [μοντέλων ανάπτυξης και εργαλεία](../azure-classic-rm.md) πριν να εργαστείτε με οποιαδήποτε Azure πόρων. Μπορείτε να προβάλετε την τεκμηρίωση για διάφορα εργαλεία, κάνοντας κλικ στις καρτέλες στο επάνω μέρος αυτού του άρθρου. Αυτό το έγγραφο περιγράφει τη δημιουργία μια πύλη εφαρμογής χρησιμοποιώντας τη διαχείριση πόρων Azure. Για να χρησιμοποιήσετε την κλασική έκδοση, ανατρέξτε στο θέμα [Δημιουργία μιας εφαρμογής ανάπτυξης κλασική πύλης με χρήση του PowerShell](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Εάν έχετε ένα υπάρχον εικονικό δίκτυο, επιλέξτε μια υπάρχουσα κενή υποδικτύου ή δημιουργήστε ένα υποδίκτυο στο δίκτυό σας υπάρχοντα εικονικό αποκλειστικά για χρήση από την πύλη εφαρμογής. Δεν μπορείτε να αναπτύξετε την πύλη εφαρμογής σε διαφορετικό δίκτυο εικονικού από τους πόρους που σκοπεύετε να αναπτύξετε πίσω από την πύλη εφαρμογής.
3. Πρέπει να υπάρχει τους διακομιστές που ρυθμίζετε τις παραμέτρους για να χρησιμοποιήσετε την πύλη εφαρμογής ή έχετε αντιστοιχίσει τα τελικά σημεία που δημιουργήθηκε στο το εικονικό δίκτυο ή με μια δημόσια IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Τι θα πρέπει να δημιουργήσει μια πύλη εφαρμογής;

- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο εικονικού δικτύου ή πρέπει να είναι μια δημόσια IP/VIP.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτές οι τιμές διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου).
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο, το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης.

## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Η διαφορά μεταξύ της χρήσης κλασική Azure και διαχείριση πόρων Azure είναι η σειρά με την οποία δημιουργείτε την πύλη εφαρμογής και τα στοιχεία που πρέπει να ρυθμιστούν.

Με τη διαχείριση πόρων, όλα τα στοιχεία που κάνετε μια πύλη εφαρμογής ρυθμίζονται μεμονωμένα και, στη συνέχεια, τοποθετήστε για να δημιουργήσετε τον πόρο πύλης εφαρμογής.

Ακολουθούν τα βήματα που απαιτούνται για να δημιουργήσετε μια πύλη εφαρμογής.

## <a name="create-a-resource-group-for-resource-manager"></a>Δημιουργία μιας ομάδας πόρων για διαχείριση πόρων

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

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Βήμα 4

Δημιουργήστε μια ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Αυτή η θέση χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια πύλη εφαρμογής χρησιμοποιεί την ίδια ομάδα πόρων.

Στο παραπάνω παράδειγμα, που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "appgw-RG" και τη θέση "Δυτική ΜΑΣ".

>[AZURE.NOTE] Εάν θέλετε να ρυθμίσετε τις παραμέτρους ενός προσαρμοσμένου δοκιμή του για την πύλη εφαρμογή, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με προσαρμοσμένο καθετήρες με χρήση του PowerShell](application-gateway-create-probe-ps.md). Ανατρέξτε στο θέμα [προσαρμοσμένες καθετήρες και τον έλεγχο εύρυθμης λειτουργίας](application-gateway-probe-overview.md) για περισσότερες πληροφορίες.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο χρησιμοποιώντας τη διαχείριση πόρων.

### <a name="step-1"></a>Βήμα 1

Εκχωρήστε το 10.0.0.0/24 περιοχής διευθύνσεων στη μεταβλητή υποδικτύου που θα χρησιμοποιηθεί για να δημιουργήσετε ένα εικονικό δίκτυο.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Βήμα 2

Δημιουργήστε ένα εικονικό δίκτυο με το όνομα "appgwvnet" στο πόρου ομάδας "appgw-rg" για την περιοχή Δυτική η.π.α., χρησιμοποιώντας το πρόθεμα 10.0.0.0/16 με 10.0.0.0/24 υποδικτύου.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Βήμα 3

Εκχωρήστε μια μεταβλητή υποδικτύου για τα επόμενα βήματα, δημιουργώντας μια πύλη εφαρμογής.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Δημιουργήστε μια δημόσια διεύθυνση IP για τη ρύθμιση παραμέτρων του περιβάλλοντος χρήστη

Δημιουργία δημόσιας IP πόρου "publicIP01" πόρου ομάδας "appgw-rg" για την περιοχή Δυτική ΗΠΑ.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Δημιουργήστε ένα αντικείμενο ρύθμισης παραμέτρων εφαρμογής πύλης

Πρέπει να ρυθμίσετε όλα τα στοιχεία ρύθμισης παραμέτρων πριν από τη δημιουργία της εφαρμογής πύλης. Ακολουθήστε τα παρακάτω βήματα Δημιουργήστε τα στοιχεία ρύθμισης παραμέτρων που απαιτούνται για έναν πόρο πύλης εφαρμογής.

### <a name="step-1"></a>Βήμα 1

Δημιουργήστε μια ρύθμιση IP πύλης εφαρμογή με το όνομα "gatewayIP01". Κατά την εκκίνηση εφαρμογών πύλης, παίρνει μια διεύθυνση IP από το υποδίκτυο έχει ρυθμιστεί και δρομολογεί την κίνηση δικτύου στις διευθύνσεις IP στο χώρο συγκέντρωσης IP παρασκηνίου. Λάβετε υπόψη ότι κάθε παρουσία λαμβάνει μια διεύθυνση IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Βήμα 2

Ρύθμιση παραμέτρων το σύνολο διευθύνσεων IP παρασκηνίου με το όνομα "pool01" με διευθύνσεις IP "134.170.185.46, 134.170.188.221,134.170.185.50". Αυτές οι διευθύνσεις IP είναι οι διευθύνσεις IP που λαμβάνουν την κίνηση δικτύου που προέρχεται από το τελικό σημείο προσκηνίου διευθύνσεων IP. Μπορείτε να αντικαταστήσετε το προηγούμενο διευθύνσεις IP για να προσθέσετε τη δική σας διεύθυνση IP εφαρμογής τα τελικά σημεία.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Βήμα 3

Ρύθμιση παραμέτρων εφαρμογής πύλης τη ρύθμιση "poolsetting01" για την κίνηση δικτύου εξισορρόπηση φόρτου στο χώρο συγκέντρωσης παρασκηνίου.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Βήμα 4

Ρυθμίστε τις παραμέτρους του προσκηνίου θύρα IP με το όνομα "frontendport01" για τη δημόσια τελικού σημείου διευθύνσεων IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Βήμα 5

Δημιουργία της προσκηνίου ρύθμισης παραμέτρων IP με το όνομα "fipconfig01" και να συσχετίσετε στη δημόσια διεύθυνση IP με τη ρύθμιση παραμέτρων προσκηνίου IP.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Βήμα 6

Δημιουργία ακρόασης το όνομα "listener01" και να συσχετίσετε τη θύρα προσκηνίου στη προσκηνίου ρύθμιση παραμέτρων διευθύνσεων IP.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Βήμα 7

Δημιουργήστε τη φόρτωση εξισορρόπησης δρομολόγησης κανόνα που ονομάζεται "rule01", το οποίο ρυθμίζει τις παραμέτρους της συμπεριφοράς εξισορρόπησης φόρτου.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Βήμα 8

Ρυθμίστε τις παραμέτρους του μεγέθους παρουσία της εφαρμογής πύλης.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ Standard_Small, Standard_Medium και Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Δημιουργήστε μια πύλη εφαρμογής χρησιμοποιώντας το νέο AzureRmApplicationGateway

Δημιουργήστε μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα προηγούμενα βήματα. Σε αυτό το παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Βήμα 9

Ανακτήστε DNS και VIP λεπτομέρειες της πύλης εφαρμογής από το δημόσιο πόρου IP που συνδέονται με την πύλη εφαρμογής.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Διαγράψτε μια πύλη εφαρμογής

Για να διαγράψετε μια πύλη εφαρμογής, ακολουθήστε τα παρακάτω βήματα:

### <a name="step-1"></a>Βήμα 1

Λήψη του αντικειμένου πύλης εφαρμογή και να τη συσχετίσετε σε μεταβλητή "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Βήμα 2

Χρησιμοποιήστε **Διακοπή AzureRmApplicationGateway** για να διακόψετε την πύλη εφαρμογής.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Όταν η εφαρμογή πύλη είναι σε κατάσταση διακοπής, χρησιμοποιήστε το cmdlet **AzureRmApplicationGateway κατάργηση** για να καταργήσετε την υπηρεσία.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] Το **-επιβάλετε** διακόπτης μπορεί να χρησιμοποιηθεί για να κάνετε απόκρυψη του μηνύματος επιβεβαίωσης κατάργηση.

Για να επαληθεύσετε ότι η υπηρεσία έχει καταργηθεί, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureRmApplicationGateway** . Αυτό το βήμα δεν είναι απαραίτητο.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μείωση φόρτου SSL, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια πύλη εφαρμογής για το SSL μείωση φόρτου](application-gateway-ssl.md).

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής για χρήση με μια εσωτερική εξισορρόπηση φόρτου, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)](application-gateway-ilb.md).

Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)

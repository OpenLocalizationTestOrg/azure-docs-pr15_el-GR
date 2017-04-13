<properties
   pageTitle="Ρύθμιση παραμέτρων μια πύλη εφαρμογής για μείωση φόρτου SSL χρησιμοποιώντας τη διαχείριση πόρων Azure | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να δημιουργήσετε μια πύλη εφαρμογής με SSL μείωση φόρτου χρησιμοποιώντας τη διαχείριση πόρων Azure"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Ρύθμιση παραμέτρων μια πύλη εφαρμογής για μείωση φόρτου SSL χρησιμοποιώντας τη διαχείριση πόρων Azure

> [AZURE.SELECTOR]
-[Πύλη του Azure](application-gateway-ssl-portal.md)
-[Azure του PowerShell για τη διαχείριση πόρων](application-gateway-ssl-arm.md)
-[Azure κλασική PowerShell](application-gateway-ssl.md)

 Azure πύλη εφαρμογής μπορεί να ρυθμιστεί για να τερματίσετε την περίοδο λειτουργίας Secure Sockets Layer (SSL) στην πύλη για να αποφύγετε κοστίζουν εργασίες αποκρυπτογράφησης SSL να συμβεί κατά τη συστοιχία web. Μείωση φόρτου SSL απλοποιεί επίσης τη διαμόρφωση διακομιστή περιβάλλοντος χρήστη και τη διαχείριση της εφαρμογής web.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής. Βεβαιωθείτε ότι δεν υπάρχουν εικονικές μηχανές ή αναπτύξεις cloud χρησιμοποιούν το υποδίκτυο. Πύλη εφαρμογής πρέπει να είναι από μόνο σε ένα υποδίκτυο εικονικού δικτύου.
3. Πρέπει να υπάρχει τους διακομιστές που ρυθμίζετε τις παραμέτρους για να χρησιμοποιήσετε την πύλη εφαρμογής ή έχετε αντιστοιχίσει τα τελικά σημεία που δημιουργήθηκε στο το εικονικό δίκτυο ή με μια δημόσια IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Τι θα πρέπει να δημιουργήσει μια πύλη εφαρμογής;


- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο εικονικού δικτύου ή πρέπει να είναι μια δημόσια IP/VIP.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτές οι ρυθμίσεις είναι διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου).
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο και το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης. Προς το παρόν, υποστηρίζεται μόνο η *Βασική* κανόνα. Ο κανόνας *βασικές* είναι φόρτωσης round robin διανομής.

**Πρόσθετες ρυθμίσεις παραμέτρων σημειώσεων**

Για ρύθμιση παραμέτρων πιστοποιητικά SSL, το πρωτόκολλο στο **HttpListener** πρέπει να αλλάξετε σε *Https* (με διάκριση πεζών-κεφαλαίων). Το στοιχείο **SslCertificate** προστίθεται **HttpListener** με την τιμή μεταβλητής που έχει ρυθμιστεί για το πιστοποιητικό SSL. Θα πρέπει να ενημερωθούν προσκηνίου τη θύρα 443.

**Για να ενεργοποιήσετε την συσχέτισης που βασίζεται σε cookie**: μια πύλη εφαρμογής μπορεί να ρυθμιστεί για να βεβαιωθείτε ότι μια αίτηση από μια περίοδο λειτουργίας του προγράμματος-πελάτη είναι κατευθυνθεί πάντα την ίδια εικονική Μηχανή στη συστοιχία web. Αυτό το σενάριο γίνεται με την εισαγωγή ενός cookie περιόδου λειτουργίας που επιτρέπει την πύλη για να κατευθύνουν κυκλοφορία σωστά. Για να ενεργοποιήσετε βασίζεται σε cookie συσχέτισης, ορίστε **CookieBasedAffinity** *ενεργοποιημένη* στο στοιχείο **BackendHttpSettings** .


## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Η διαφορά μεταξύ της χρήσης του Azure κλασική μοντέλο ανάπτυξης και διαχείρισης πόρων Azure είναι η σειρά που δημιουργείτε μια πύλη εφαρμογής και τα στοιχεία που πρέπει να ρυθμιστούν.

Με τη διαχείριση πόρων, όλα τα στοιχεία από μια πύλη εφαρμογής ρυθμίζονται μεμονωμένα και, στη συνέχεια, τοποθετήστε για να δημιουργήσετε μια εφαρμογή του πόρου πύλης.


Ακολουθούν τα βήματα που απαιτούνται για να δημιουργήσετε μια πύλη εφαρμογής:

1. Δημιουργία μιας ομάδας πόρων για διαχείριση πόρων
2. Δημιουργία εικονικού δικτύου, υποδικτύου και δημόσια IP για την πύλη εφαρμογής
3. Δημιουργήστε ένα αντικείμενο ρύθμισης παραμέτρων εφαρμογής πύλης
4. Δημιουργία ενός πόρου πύλης εφαρμογής


## <a name="create-a-resource-group-for-resource-manager"></a>Δημιουργία μιας ομάδας πόρων για διαχείριση πόρων

Βεβαιωθείτε ότι αλλάζετε λειτουργία του PowerShell για να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων Azure. Περισσότερες πληροφορίες είναι διαθέσιμες στη [Χρήση του Windows PowerShell με το διαχειριστή πόρων](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Βήμα 1

    Login-AzureRmAccount

### <a name="step-2"></a>Βήμα 2

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription

Θα σας ζητηθεί για τον έλεγχο ταυτότητας με τα διαπιστευτήριά σας.<BR>

### <a name="step-3"></a>Βήμα 3

Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Βήμα 4

Δημιουργήστε μια ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Αυτή η ρύθμιση χρησιμοποιείται ως η προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια πύλη εφαρμογής χρησιμοποιεί την ίδια ομάδα πόρων.

Στο παραπάνω παράδειγμα, που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "appgw RG" και τη θέση "Δυτική ΜΑΣ".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο χρησιμοποιώντας τη διαχείριση πόρων:

### <a name="step-1"></a>Βήμα 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Αυτό το δείγμα εκχωρεί την 10.0.0.0/24 περιοχή διεύθυνση σε μεταβλητή υποδικτύου που θα χρησιμοποιηθεί για να δημιουργήσετε ένα εικονικό δίκτυο.

### <a name="step-2"></a>Βήμα 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Αυτό το δείγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα "appgwvnet" στο πόρου ομάδας "appgw-rg" για την περιοχή Δυτική η.π.α., χρησιμοποιώντας το πρόθεμα 10.0.0.0/16 με 10.0.0.0/24 υποδικτύου.

### <a name="step-3"></a>Βήμα 3

    $subnet = $vnet.Subnets[0]

Αυτό το δείγμα αντιστοιχίζει αντικειμένου υποδικτύου μεταβλητή $subnet για τα επόμενα βήματα.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Δημιουργήστε μια δημόσια διεύθυνση IP για τη ρύθμιση παραμέτρων του περιβάλλοντος χρήστη

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Αυτό το δείγμα δημιουργεί μια δημόσια πόρων IP "publicIP01" στο πόρου ομάδας "appgw-rg" για την περιοχή Δυτική η.π.α..


## <a name="create-an-application-gateway-configuration-object"></a>Δημιουργήστε ένα αντικείμενο ρύθμισης παραμέτρων εφαρμογής πύλης

### <a name="step-1"></a>Βήμα 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Αυτό το δείγμα δημιουργεί μια ρύθμιση IP πύλης εφαρμογή με το όνομα "gatewayIP01". Κατά την εκκίνηση εφαρμογών πύλης, παίρνει μια διεύθυνση IP από το υποδίκτυο έχει ρυθμιστεί και δρομολογείτε την κίνηση δικτύου στις διευθύνσεις IP στο χώρο συγκέντρωσης IP παρασκηνίου. Λάβετε υπόψη ότι κάθε παρουσία λαμβάνει μια διεύθυνση IP.

### <a name="step-2"></a>Βήμα 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Αυτό το δείγμα ρυθμίζει το σύνολο διευθύνσεων IP παρασκηνίου με το όνομα "pool01" με διευθύνσεις IP "134.170.185.46, 134.170.188.221,134.170.185.50." Αυτές οι τιμές είναι οι διευθύνσεις IP που λαμβάνουν την κίνηση δικτύου που προέρχεται από το τελικό σημείο προσκηνίου διευθύνσεων IP. Αντικαταστήστε τις διευθύνσεις IP από το προηγούμενο παράδειγμα με τις διευθύνσεις IP του σας τελικά σημεία της εφαρμογής web.

### <a name="step-3"></a>Βήμα 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Αυτό το δείγμα ρυθμίζει τις παραμέτρους ρύθμισης πύλης εφαρμογής "poolsetting01" για να εξισορρόπηση φόρτου κίνηση του δικτύου στο χώρο συγκέντρωσης παρασκηνίου.

### <a name="step-4"></a>Βήμα 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Αυτό το δείγμα ρυθμίζει τις παραμέτρους του προσκηνίου θύρα IP με το όνομα "frontendport01" για το δημόσιο τελικό σημείο διευθύνσεων IP.

### <a name="step-5"></a>Βήμα 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Αυτό το δείγμα ρυθμίζει τις παραμέτρους του πιστοποιητικού που χρησιμοποιείται για τη σύνδεση SSL. Το πιστοποιητικό πρέπει να είναι σε μορφή .pfx και τον κωδικό πρόσβασης, πρέπει να είναι μεταξύ των χαρακτήρων 4 έως 12.

### <a name="step-6"></a>Βήμα 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Αυτό το δείγμα δημιουργεί τη προσκηνίου ρύθμιση παραμέτρων IP με το όνομα "fipconfig01" και συσχετίζει στη δημόσια διεύθυνση IP με τη ρύθμιση παραμέτρων προσκηνίου IP.

### <a name="step-7"></a>Βήμα 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Αυτό το δείγμα δημιουργεί το όνομα ακρόασης "listener01" και συσχετίζει τη θύρα προσκηνίου στη ρύθμιση παραμέτρων IP του προσκηνίου και πιστοποιητικού.

### <a name="step-8"></a>Βήμα 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Αυτό το δείγμα δημιουργεί τη φόρτωση εξισορρόπησης δρομολόγησης κανόνα που ονομάζεται "rule01", το οποίο ρυθμίζει τις παραμέτρους της συμπεριφοράς εξισορρόπησης φόρτου.

### <a name="step-9"></a>Βήμα 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Αυτό το δείγμα ρυθμίζει το μέγεθος παρουσία της εφαρμογής πύλης.

>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ Standard_Small, Standard_Medium και Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Δημιουργήστε μια πύλη εφαρμογής χρησιμοποιώντας το νέο AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Αυτό το δείγμα δημιουργεί μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα προηγούμενα βήματα. Στο παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".

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

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής για χρήση με μια εσωτερική εξισορρόπηση φόρτου (ILB), ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)](application-gateway-ilb.md).

Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)

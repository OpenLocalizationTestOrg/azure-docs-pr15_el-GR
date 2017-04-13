<properties
   pageTitle="Δημιουργία ενός προσαρμοσμένου δοκιμή του για την πύλη εφαρμογής με χρήση του PowerShell στη Διαχείριση πόρων | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη δοκιμή του εφαρμογής πύλης με χρήση του PowerShell στη Διαχείριση πόρων"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Δημιουργία ενός προσαρμοσμένου δοκιμή του Azure εφαρμογής πύλης με χρήση του PowerShell για τη διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-create-probe-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-create-probe-ps.md)
- [Azure κλασική PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][μοντέλο κλασική ανάπτυξης](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Βήμα 1

Χρησιμοποιήστε AzureRmAccount σύνδεσης για τον έλεγχο ταυτότητας.

    Login-AzureRmAccount

### <a name="step-2"></a>Βήμα 2

Ελέγξτε τις συνδρομές για το λογαριασμό.

    Get-AzureRmSubscription

### <a name="step-3"></a>Βήμα 3

Επιλέξτε από το Azure όλες τις συνδρομές σας για να χρησιμοποιήσετε. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Βήμα 4

Δημιουργήστε μια ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Αυτή η θέση χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια πύλη εφαρμογής χρησιμοποιούν την ίδια ομάδα πόρων.

Στο παραπάνω παράδειγμα, που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "appgw RG" και τη θέση "Δυτική ΜΑΣ".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής

Ακολουθήστε τα παρακάτω βήματα να δημιουργήσετε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής.

### <a name="step-1"></a>Βήμα 1


Εκχωρήστε το 10.0.0.0/24 περιοχή διεύθυνση σε μεταβλητή υποδικτύου που θα χρησιμοποιηθεί για να δημιουργήσετε ένα εικονικό δίκτυο.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Βήμα 2

Δημιουργήστε ένα εικονικό δίκτυο με το όνομα "appgwvnet" στο πόρου ομάδας "appgw-rg" για την περιοχή Δυτική η.π.α., χρησιμοποιώντας το πρόθεμα 10.0.0.0/16 με 10.0.0.0/24 υποδικτύου.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Βήμα 3

Εκχωρήστε μια μεταβλητή υποδικτύου για τα επόμενα βήματα, δημιουργώντας μια πύλη εφαρμογής.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Δημιουργήστε μια δημόσια διεύθυνση IP για τη ρύθμιση παραμέτρων του περιβάλλοντος χρήστη


Δημιουργία δημόσιας IP πόρου "publicIP01" πόρου ομάδας "appgw-rg" για την περιοχή Δυτική ΗΠΑ.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Δημιουργία ενός αντικειμένου ρύθμισης παραμέτρων πύλης εφαρμογή με ένα προσαρμοσμένο δοκιμή του

Μπορείτε να ορίσετε όλα τα στοιχεία ρύθμισης παραμέτρων πριν από τη δημιουργία της εφαρμογής πύλης. Ακολουθήστε τα παρακάτω βήματα Δημιουργήστε τα στοιχεία ρύθμισης παραμέτρων που απαιτούνται για έναν πόρο πύλης εφαρμογής.

### <a name="step-1"></a>Βήμα 1

Δημιουργήστε μια ρύθμιση IP πύλης εφαρμογή με το όνομα "gatewayIP01". Κατά την εκκίνηση εφαρμογών πύλης, παίρνει μια διεύθυνση IP από το υποδίκτυο έχει ρυθμιστεί και δρομολογείτε την κίνηση δικτύου στις διευθύνσεις IP στο χώρο συγκέντρωσης IP παρασκηνίου. Λάβετε υπόψη ότι κάθε παρουσία λαμβάνει μια διεύθυνση IP.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Βήμα 2


Ρύθμιση παραμέτρων το σύνολο διευθύνσεων IP παρασκηνίου με το όνομα "pool01" με διευθύνσεις IP "134.170.185.46, 134.170.188.221,134.170.185.50". Αυτές οι τιμές είναι οι διευθύνσεις IP που λαμβάνουν την κίνηση δικτύου που προέρχεται από το τελικό σημείο προσκηνίου διευθύνσεων IP. Μπορείτε να αντικαταστήσετε τις παραπάνω για να προσθέσετε τη δική σας διεύθυνση IP εφαρμογής τα τελικά σημεία διευθύνσεις IP.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Βήμα 3


Το προσαρμοσμένο δοκιμή του έχει ρυθμιστεί σε αυτό το βήμα.

Οι παράμετροι χρησιμοποιούνται είναι:

- **Διάστημα** - ρυθμίζει τις παραμέτρους των ελέγχων δοκιμή του χρονικού διαστήματος σε δευτερόλεπτα.
- **Λήξη χρονικού ορίου** - Καθορίζει το χρονικό όριο δοκιμή του για έλεγχο απόκρισης HTTP.
- **Όνομα κεντρικού υπολογιστή - και τη διαδρομή** - διαδρομή πλήρη διεύθυνση URL που ενεργοποιείται από πύλη εφαρμογής για να προσδιορίσετε την εύρυθμη λειτουργία της παρουσίας. Για παράδειγμα, εάν έχετε μια τοποθεσία Web http://contoso.com/, στη συνέχεια, το προσαρμοσμένο δοκιμή του μπορούν να ρυθμιστούν για "http://contoso.com/path/custompath.htm" για δοκιμή του ελέγχους για να έχετε μια επιτυχημένη απόκριση HTTP.
- **UnhealthyThreshold** - ο αριθμός των αποτυχημένων αποκρίσεις HTTP χρειάζεται να επισημαίνει την παρουσία παρασκηνίου ως *κατεστραμμένες*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Βήμα 4

Ρύθμιση παραμέτρων εφαρμογής πύλης τη ρύθμιση "poolsetting01" για την κίνηση στο χώρο συγκέντρωσης παρασκηνίου. Αυτό το βήμα έχει επίσης μια ρύθμιση παραμέτρων λήξης χρονικού ορίου που είναι για το χώρο συγκέντρωσης παρασκηνίου απάντηση μιας αίτησης πύλης. Όταν παρασκηνίου απόκριση επισκέψεις χρονικό όριο, πύλη εφαρμογής ακυρώνει την αίτηση. Αυτή η τιμή είναι διαφορετικό από ένα χρονικό όριο δοκιμή του που είναι μόνο για την απάντηση παρασκηνίου για τη διερεύνηση ελέγχων.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Βήμα 5

Ρυθμίστε τις παραμέτρους του προσκηνίου θύρα IP με το όνομα "frontendport01" για τη δημόσια τελικού σημείου διευθύνσεων IP.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Βήμα 6

Δημιουργία της προσκηνίου ρύθμισης παραμέτρων IP με το όνομα "fipconfig01" και να συσχετίσετε στη δημόσια διεύθυνση IP με τη ρύθμιση παραμέτρων προσκηνίου IP.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Βήμα 7

Δημιουργία ακρόασης το όνομα "listener01" και να συσχετίσετε τη θύρα προσκηνίου στη προσκηνίου ρύθμιση παραμέτρων διευθύνσεων IP.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Βήμα 8

Δημιουργήστε τη φόρτωση εξισορρόπησης δρομολόγησης κανόνα που ονομάζεται "rule01", το οποίο ρυθμίζει τις παραμέτρους της συμπεριφοράς εξισορρόπησης φόρτου.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Βήμα 9

Ρυθμίστε τις παραμέτρους του μεγέθους παρουσία της εφαρμογής πύλης.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ Standard_Small, Standard_Medium και Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Δημιουργήστε μια πύλη εφαρμογής χρησιμοποιώντας το νέο AzureRmApplicationGateway

Δημιουργήστε μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα παραπάνω βήματα. Σε αυτό το παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Προσθήκη μιας δοκιμή του σε μια υπάρχουσα πύλη εφαρμογής

Έχετε τέσσερα βήματα για να προσθέσετε ένα προσαρμοσμένο δοκιμή του σε μια υπάρχουσα πύλη εφαρμογής.

### <a name="step-1"></a>Βήμα 1

Φόρτωση του πόρου πύλης εφαρμογής σε μεταβλητή PowerShell χρησιμοποιώντας **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Βήμα 2

Προσθέστε μια δοκιμή του τις υπάρχουσες ρυθμίσεις παραμέτρων της πύλης.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


Στο παράδειγμα, το προσαρμοσμένο δοκιμή του έχει ρυθμιστεί για να ελέγξετε για contoso.com/path/custompath.htm διαδρομή διεύθυνσης URL κάθε 30 δευτερόλεπτα. Μια οριακή τιμή λήξης χρονικού ορίου 120 δευτερόλεπτα έχει ρυθμιστεί με έναν μέγιστο αριθμό 8 αιτήσεις αποτυχίας αναζήτησης.

### <a name="step-3"></a>Βήμα 3

Προσθέστε τη δοκιμή του στο χώρο συγκέντρωσης παρασκηνίου τη ρύθμιση παραμέτρων και λήξης χρονικού ορίου χρησιμοποιώντας **-Ορισμός-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Βήμα 4

Αποθηκεύστε τη ρύθμιση παραμέτρων για την πύλη εφαρμογής χρησιμοποιώντας **Σύνολο AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Κατάργηση ενός δοκιμή του από μια υπάρχουσα πύλη εφαρμογής

Ακολουθούν τα βήματα για να καταργήσετε ένα προσαρμοσμένο δοκιμή του από μια υπάρχουσα πύλη εφαρμογής.

### <a name="step-1"></a>Βήμα 1

Φόρτωση του πόρου πύλης εφαρμογής σε μεταβλητή PowerShell χρησιμοποιώντας **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Βήμα 2

Καταργήστε τη ρύθμιση παραμέτρων δοκιμή του από την πύλη εφαρμογής με τη χρήση **AzureRmApplicationGatewayProbeConfig κατάργηση**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Βήμα 3

Ενημέρωση του χώρου συγκέντρωσης παρασκηνίου τη ρύθμιση για να καταργήσετε τη διερεύνηση και ρύθμιση χρονικού ορίου χρησιμοποιώντας **-Ορισμός-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Βήμα 4

Αποθηκεύστε τη ρύθμιση παραμέτρων για την πύλη εφαρμογής χρησιμοποιώντας **Σύνολο AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="get-application-gateway-dns-name"></a>Λήψη εφαρμογής όνομα DNS πύλης

Όταν δημιουργηθεί η πύλη, το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους του υπολογιστή-πελάτη για την επικοινωνία. Όταν χρησιμοποιείτε μια δημόσια IP, πύλη εφαρμογής απαιτεί μια δυναμική αντιστοίχιση όνομα DNS, το οποίο δεν είναι φιλική. Για να τοποθετήστε το δείκτη στο δημόσιο τελικό σημείο της εφαρμογής πύλης μπορεί να χρησιμοποιηθεί για να βεβαιωθείτε ότι οι τελικοί χρήστες μπορούν να πατήσετε την πύλη εφαρμογής μια εγγραφή CNAME. [Ρύθμιση των παραμέτρων ενός προσαρμοσμένου ονόματος τομέα για στο Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). Για να το κάνετε αυτό, η ανάκτηση λεπτομέρειες σχετικά με την πύλη εφαρμογής και το σχετικό όνομα IP/DNS χρησιμοποιώντας το στοιχείο PublicIPAddress που συνδέονται με την πύλη εφαρμογής. Το όνομα της πύλης εφαρμογή DNS πρέπει να χρησιμοποιείται για να δημιουργήσετε μια εγγραφή CNAME, το οποίο δείχνει τις εφαρμογές web δύο σε αυτό το όνομα DNS. Η χρήση των εγγραφών A δεν συνιστάται, επειδή το VIP ενδέχεται να αλλάξουν κατά την επανεκκίνηση της εφαρμογής πύλης.
    
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

Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους SSL μείωση φόρτου μεταβαίνοντας [Ρύθμιση παραμέτρων μείωση φόρτου SSL](application-gateway-ssl-arm.md)


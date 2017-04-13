<properties
   pageTitle="Δημιουργία και ρύθμιση παραμέτρων μιας εφαρμογής πύλης με μια εσωτερική εξισορρόπηση φόρτου (ILB) χρησιμοποιώντας τη διαχείριση πόρων Azure | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να δημιουργήσετε, ρύθμιση παραμέτρων, Έναρξη και να διαγράψετε μια πύλη Azure εφαρμογής με εσωτερική εξισορρόπηση φόρτου (ILB) για τη διαχείριση πόρων Azure"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Δημιουργήστε μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB) χρησιμοποιώντας τη διαχείριση πόρων Azure

> [AZURE.SELECTOR]
- [Azure κλασική βήματα](application-gateway-ilb.md)
- [Τα βήματα του PowerShell για τη διαχείριση πόρων](application-gateway-ilb-arm.md)

Azure πύλη εφαρμογής μπορούν να ρυθμιστούν με μια VIP μέσω Internet ή με μια εσωτερική τελικό σημείο που δεν εκτίθεται στο Internet, γνωστές και ως ένα τελικό σημείο του εσωτερικού φόρτωσης εξισορρόπησης (ILB). Ρύθμιση παραμέτρων της πύλης με μια ILB είναι χρήσιμη για εσωτερικές εφαρμογές γραμμής εταιρικά που δεν είναι εκτεθειμένη στο Internet. Είναι επίσης χρήσιμο για τις υπηρεσίες και φορτώστε βαθμίδες μέσα σε μια εφαρμογή πολλαπλών επιπέδων που καθίσετε σε ένα όριο ασφαλείας που δεν είναι εκτεθειμένη στο Internet, αλλά εξακολουθούν να απαιτούν round robin κατανομή, την περίοδο λειτουργίας υποστεί ή τερματισμού Secure Sockets Layer (SSL).

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής με μια ILB.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο και ένα υποδίκτυο για πύλη εφαρμογής. Βεβαιωθείτε ότι δεν υπάρχουν εικονικές μηχανές ή αναπτύξεις cloud χρησιμοποιούν το υποδίκτυο. Πύλη εφαρμογής πρέπει να είναι από μόνο σε ένα υποδίκτυο εικονικού δικτύου.
3. Πρέπει να υπάρχει τους διακομιστές που ρυθμίζετε τις παραμέτρους για να χρησιμοποιήσετε την πύλη εφαρμογής ή έχετε αντιστοιχίσει τα τελικά σημεία που δημιουργήθηκε στο το εικονικό δίκτυο ή με μια δημόσια IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Τι θα πρέπει να δημιουργήσει μια πύλη εφαρμογής;


- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο δίκτυο εικονικού αλλά σε διαφορετικό υποδίκτυο για την πύλη εφαρμογής ή πρέπει να είναι μια δημόσια IP/VIP.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτοί είναι διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου).
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο και το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης. Προς το παρόν, υποστηρίζεται μόνο η *Βασική* κανόνα. Ο κανόνας *βασικές* είναι φόρτωσης round robin διανομής.



## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Η διαφορά μεταξύ της χρήσης κλασική Azure και διαχείριση πόρων Azure είναι η σειρά με την οποία δημιουργείτε την πύλη εφαρμογής και τα στοιχεία που πρέπει να ρυθμιστούν.
Με τη διαχείριση πόρων, όλα τα στοιχεία που κάνετε μια πύλη εφαρμογής ρυθμιστούν ξεχωριστά και, στη συνέχεια, τοποθετήστε για να δημιουργήσετε τον πόρο πύλης εφαρμογής.


Ακολουθούν τα βήματα που απαιτούνται για να δημιουργήσετε μια πύλη εφαρμογής:

1. Δημιουργία μιας ομάδας πόρων για διαχείριση πόρων
2. Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής
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

Δημιουργήστε μια νέα ομάδα πόρων (παράλειψη αυτό το βήμα εάν χρησιμοποιείτε μια υπάρχουσα ομάδα πόρων).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure διαχείριση πόρων απαιτεί όλων των ομάδων πόρων καθορίσετε μια θέση. Χρησιμοποιείται ως την προεπιλεγμένη θέση για τους πόρους σε αυτήν την ομάδα πόρων. Βεβαιωθείτε ότι όλες οι εντολές για να δημιουργήσετε μια πύλη εφαρμογής χρησιμοποιεί την ίδια ομάδα πόρων.

Στο παραπάνω παράδειγμα, που δημιουργήσαμε μια ομάδα πόρων που ονομάζεται "appgw-rg" και τη θέση "Δυτική ΜΑΣ".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Δημιουργήστε ένα εικονικό δίκτυο και ένα υποδίκτυο για την πύλη εφαρμογής

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο χρησιμοποιώντας τη διαχείριση πόρων:

### <a name="step-1"></a>Βήμα 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Αυτό εκχωρεί την 10.0.0.0/24 περιοχή διεύθυνση σε μεταβλητή υποδικτύου που θα χρησιμοποιηθεί για να δημιουργήσετε ένα εικονικό δίκτυο.

### <a name="step-2"></a>Βήμα 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Αυτό δημιουργεί ένα εικονικό δίκτυο με το όνομα "appgwvnet" στο πόρου ομάδας "appgw-rg" για την περιοχή Δυτική η.π.α., χρησιμοποιώντας το πρόθεμα 10.0.0.0/16 με 10.0.0.0/24 υποδικτύου.

### <a name="step-3"></a>Βήμα 3

    $subnet = $vnet.subnets[0]

Αυτό εκχωρεί αντικειμένου υποδικτύου μεταβλητή $subnet για τα επόμενα βήματα.

## <a name="create-an-application-gateway-configuration-object"></a>Δημιουργήστε ένα αντικείμενο ρύθμισης παραμέτρων εφαρμογής πύλης

### <a name="step-1"></a>Βήμα 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Αυτό δημιουργεί μια ρύθμιση IP πύλης εφαρμογή με το όνομα "gatewayIP01". Κατά την εκκίνηση εφαρμογών πύλης, παίρνει μια διεύθυνση IP από το υποδίκτυο έχει ρυθμιστεί και δρομολογείτε την κίνηση δικτύου στις διευθύνσεις IP στο χώρο συγκέντρωσης IP παρασκηνίου. Λάβετε υπόψη ότι κάθε παρουσία λαμβάνει μια διεύθυνση IP.


### <a name="step-2"></a>Βήμα 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Αυτό το σύνολο διευθύνσεων IP παρασκηνίου με το όνομα "pool01" με διευθύνσεις IP ρυθμίζει τις παραμέτρους "134.170.185.46, 134.170.188.221,134.170.185.50". Αυτές είναι οι διευθύνσεις IP που λαμβάνουν την κίνηση δικτύου που προέρχεται από το τελικό σημείο προσκηνίου IP. Μπορείτε να αντικαταστήσετε τις παραπάνω για να προσθέσετε τη δική σας διεύθυνση IP εφαρμογής τα τελικά σημεία διευθύνσεις IP.

### <a name="step-3"></a>Βήμα 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Αυτό ρυθμίζει τις παραμέτρους κίνηση του δικτύου πύλης τη ρύθμιση "poolsetting01" για τη φόρτωση εξισορρόπηση εφαρμογή στο χώρο συγκέντρωσης παρασκηνίου.

### <a name="step-4"></a>Βήμα 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Αυτό ρυθμίζει τις παραμέτρους του προσκηνίου θύρα IP με το όνομα "frontendport01" για το ILB.

### <a name="step-5"></a>Βήμα 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Αυτό δημιουργεί τη προσκηνίου ρύθμιση παραμέτρων IP που ονομάζεται "fipconfig01" και συσχετίζει με μια ιδιωτική IP από το τρέχον υποδίκτυο εικονικού δικτύου.

### <a name="step-6"></a>Βήμα 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Αυτό δημιουργεί το ακροατήριο που ονομάζεται "listener01" και συσχετίζει τη θύρα προσκηνίου στη προσκηνίου ρύθμιση παραμέτρων διευθύνσεων IP.

### <a name="step-7"></a>Βήμα 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Αυτό δημιουργεί τη φόρτωση κανόνα δρομολόγησης εξισορρόπησης που ονομάζεται "rule01", το οποίο ρυθμίζει τις παραμέτρους της συμπεριφοράς εξισορρόπησης φόρτου.

### <a name="step-8"></a>Βήμα 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Αυτό ρυθμίζει το μέγεθος παρουσία της εφαρμογής πύλης.

>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ Standard_Small, Standard_Medium και Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Δημιουργήστε μια πύλη εφαρμογής χρησιμοποιώντας το νέο AzureApplicationGateway

Δημιουργεί μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα παραπάνω βήματα. Σε αυτό το παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Με τον τρόπο αυτό δημιουργείται μια πύλη εφαρμογής με όλα τα στοιχεία ρύθμισης παραμέτρων από τα παραπάνω βήματα. Στο παράδειγμα, η πύλη εφαρμογή ονομάζεται "appgwtest".


## <a name="delete-an-application-gateway"></a>Διαγράψτε μια πύλη εφαρμογής

Για να διαγράψετε μια πύλη εφαρμογής, θα πρέπει να κάνετε τα εξής κατά σειρά:

1. Χρησιμοποιήστε το cmdlet **Διακοπή AzureRmApplicationGateway** για να διακόψετε την πύλη.
2. Χρησιμοποιήστε το cmdlet **AzureRmApplicationGateway κατάργηση** για να καταργήσετε την πύλη.
3. Βεβαιωθείτε ότι η πύλη έχει καταργηθεί, χρησιμοποιώντας το cmdlet **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Βήμα 1

Λήψη του αντικειμένου πύλης εφαρμογή και να τη συσχετίσετε σε μεταβλητή "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Βήμα 2

Χρησιμοποιήστε **Διακοπή AzureRmApplicationGateway** για να διακόψετε την πύλη εφαρμογής. Αυτό το δείγμα δείχνει το cmdlet **Διακοπή AzureRmApplicationGateway** στην πρώτη γραμμή, ακολουθούμενο από το αποτέλεσμα.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Όταν η εφαρμογή πύλη είναι σε κατάσταση διακοπής, χρησιμοποιήστε το cmdlet **AzureRmApplicationGateway κατάργηση** για να καταργήσετε την υπηρεσία.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] Το **-επιβάλετε** διακόπτης μπορεί να χρησιμοποιηθεί για να κάνετε απόκρυψη του μηνύματος επιβεβαίωσης κατάργηση.


Για να επαληθεύσετε ότι η υπηρεσία έχει καταργηθεί, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureRmApplicationGateway** . Αυτό το βήμα δεν είναι απαραίτητο.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μείωση φόρτου SSL, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια πύλη εφαρμογής για το SSL μείωση φόρτου](application-gateway-ssl.md).

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής για χρήση με ένα ILB, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)](application-gateway-ilb.md).

Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)

<properties
   pageTitle="Δημιουργία, ξεκινήστε ή να διαγράψετε μια πύλη εφαρμογής | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να δημιουργήσετε, ρύθμιση παραμέτρων, Έναρξη και να διαγράψετε μια πύλη Azure εφαρμογής"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Δημιουργία, ξεκινήστε ή να διαγράψετε μια πύλη εφαρμογής

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-create-gateway-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-create-gateway-arm.md)
- [Azure κλασική PowerShell](application-gateway-create-gateway.md)
- [Azure προτύπου για τη διαχείριση πόρων](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure πύλη εφαρμογής είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-7. Παρέχει ανακατεύθυνση, δρομολόγησης επιδόσεων αιτήσεις HTTP ανάμεσα σε διαφορετικούς διακομιστές, είτε είναι στο cloud ή εσωτερικής εγκατάστασης. Πύλη εφαρμογής παρέχει πολλές δυνατότητες εφαρμογή παράδοσης ελεγκτή (ADC) συμπεριλαμβανομένων εξισορρόπηση φόρτου HTTP συσχέτισης βασίζεται σε cookie περιόδου λειτουργίας, Secure Sockets Layer (SSL) μείωση φόρτου, καθετήρες προσαρμοσμένο εύρυθμης λειτουργίας υποστήριξης για πολλούς τοποθεσίας και πολλά άλλα. Για να βρείτε μια πλήρη λίστα των υποστηριζόμενες δυνατότητες, επισκεφθείτε την [Επισκόπηση πύλης εφαρμογής](application-gateway-introduction.md)

Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να δημιουργήσετε, ρύθμιση παραμέτρων, Έναρξη και να διαγράψετε μια πύλη εφαρμογής.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Εάν έχετε ένα υπάρχον εικονικό δίκτυο, επιλέξτε μια υπάρχουσα κενή υποδικτύου ή δημιουργήστε ένα νέο υποδίκτυο στο δίκτυό σας υπάρχοντα εικονικό αποκλειστικά για χρήση με την πύλη εφαρμογής. Δεν μπορείτε να αναπτύξετε την πύλη εφαρμογής σε διαφορετικό δίκτυο εικονικού από τους πόρους που σκοπεύετε να αναπτύξετε πίσω από την πύλη εφαρμογής, εκτός εάν διεισδύουν vnet χρησιμοποιείται. Για να μάθετε περισσότερα, επισκεφθείτε [Διεισδύουν Vnet](../virtual-network/virtual-network-peering-overview.md)
3. Βεβαιωθείτε ότι έχετε ένα εικονικό δίκτυο εργάζεστε με ένα έγκυρο υποδίκτυο. Βεβαιωθείτε ότι δεν υπάρχουν εικονικές μηχανές ή αναπτύξεις cloud χρησιμοποιούν το υποδίκτυο. Η πύλη εφαρμογής πρέπει να είναι μόνη ένα υποδίκτυο εικονικού δικτύου.
3. Πρέπει να υπάρχει τους διακομιστές που ρυθμίζετε τις παραμέτρους για να χρησιμοποιήσετε την πύλη εφαρμογής ή έχετε αντιστοιχίσει τα τελικά σημεία που δημιουργήθηκε στο το εικονικό δίκτυο ή με μια δημόσια IP/VIP.

## <a name="what-is-required-to-create-an-application-gateway"></a>Τι θα πρέπει να δημιουργήσει μια πύλη εφαρμογής;

Όταν χρησιμοποιείτε την εντολή **Δημιουργία AzureApplicationGateway** για να δημιουργήσετε την πύλη εφαρμογής, ρύθμιση των παραμέτρων δεν έχει οριστεί σε αυτό το σημείο και ο πόρος που έχουν δημιουργηθεί πρόσφατα έχουν ρυθμιστεί οι παράμετροι είτε με τη χρήση XML ή ένα αντικείμενο ρύθμισης παραμέτρων.

Οι τιμές είναι:

- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο εικονικού δικτύου ή πρέπει να είναι μια δημόσια IP/VIP.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτές οι τιμές διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου).
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο και το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης.

## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Για να δημιουργήσετε μια πύλη εφαρμογής:

1. Δημιουργήστε έναν πόρο πύλης εφαρμογής.
2. Δημιουργήστε ένα αρχείο XML ρύθμισης παραμέτρων ή ένα αντικείμενο ρύθμισης παραμέτρων.
3. Ολοκλήρωση τη ρύθμιση παραμέτρων για τον πόρο πύλης που έχουν δημιουργηθεί πρόσφατα εφαρμογής.

>[AZURE.NOTE] Εάν θέλετε να ρυθμίσετε τις παραμέτρους ενός προσαρμοσμένου δοκιμή του για την πύλη εφαρμογή, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με προσαρμοσμένο καθετήρες με χρήση του PowerShell](application-gateway-create-probe-classic-ps.md). Ανατρέξτε στο θέμα [προσαρμοσμένες καθετήρες και τον έλεγχο εύρυθμης λειτουργίας](application-gateway-probe-overview.md) για περισσότερες πληροφορίες.

![Παράδειγμα σεναρίου][scenario]

### <a name="create-an-application-gateway-resource"></a>Δημιουργία ενός πόρου πύλης εφαρμογής

Για να δημιουργήσετε την πύλη, χρησιμοποιήστε το cmdlet **New-AzureApplicationGateway** , αντικαθιστώντας τις τιμές με το δικό σας. Χρέωση για την πύλη δεν ξεκινά σε αυτό το σημείο. Χρέωση αρχίζει αργότερα, όταν ξεκινήσει με επιτυχία με την πύλη.

Το παρακάτω παράδειγμα δημιουργεί μια πύλη εφαρμογής, χρησιμοποιώντας ένα εικονικό δίκτυο που ονομάζεται "testvnet1" και ένα δευτερεύον που ονομάζεται "υποδικτύου-1".


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Περιγραφή*, *InstanceCount*και *GatewaySize* είναι προαιρετικές παραμέτρους.

Για να επικυρώσετε ότι η πύλη που δημιουργήθηκε, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ μικρό, μεσαίο και μεγάλο.

*VirtualIPs* και *DnsName* εμφανίζονται ως κενά επειδή η πύλη δεν έχει αρχίσει ακόμα. Αυτά δημιουργούνται αφού η πύλη είναι σε κατάσταση λειτουργίας.

## <a name="configure-the-application-gateway"></a>Ρύθμιση παραμέτρων της πύλης εφαρμογής

Μπορείτε να ρυθμίσετε την πύλη εφαρμογής με τη χρήση XML ή ένα αντικείμενο ρύθμισης παραμέτρων.

## <a name="configure-the-application-gateway-by-using-xml"></a>Ρύθμιση παραμέτρων της πύλης εφαρμογών με χρήση XML

Στο παρακάτω παράδειγμα, μπορείτε να χρησιμοποιήσετε ένα αρχείο XML για να ρυθμίσετε όλες τις ρυθμίσεις πύλης εφαρμογής και ολοκλήρωση τους στον πόρο πύλης εφαρμογής.  

### <a name="step-1"></a>Βήμα 1  

Αντιγράψτε το ακόλουθο κείμενο στο Σημειωματάριο.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Επεξεργαστείτε τις τιμές μεταξύ των παρενθέσεων για τα στοιχεία ρύθμισης παραμέτρων. Αποθηκεύστε το αρχείο με επέκταση .xml.

>[AZURE.IMPORTANT] Το στοιχείο πρωτοκόλλου Http ή Https διάκριση πεζών-κεφαλαίων.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα αρχείο ρύθμισης παραμέτρων για να ρυθμίσετε την πύλη εφαρμογής. Η φόρτωση παράδειγμα υπολοίπων κυκλοφορία HTTP στη δημόσια 80 και στέλνει κίνηση του δικτύου παρασκηνίου θύρα 80 μεταξύ δύο διευθύνσεων IP.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Βήμα 2

Στη συνέχεια, ορίστε την πύλη εφαρμογής. Χρησιμοποιήστε το cmdlet **Set-AzureApplicationGatewayConfig** με ένα αρχείο XML ρύθμισης παραμέτρων.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Ρύθμιση παραμέτρων της πύλης εφαρμογής, χρησιμοποιώντας ένα αντικείμενο ρύθμισης παραμέτρων

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους της εφαρμογής πύλης χρησιμοποιώντας αντικείμενα ρύθμισης παραμέτρων. Όλα τα στοιχεία ρύθμισης παραμέτρων πρέπει να ρυθμιστεί μεμονωμένα και στη συνέχεια προστίθεται σε ένα αντικείμενο ρύθμισης παραμέτρων εφαρμογής πύλης. Αφού δημιουργήσετε το αντικείμενο ρύθμισης παραμέτρων, μπορείτε να χρησιμοποιήσετε την εντολή **Set-AzureApplicationGateway** για να ολοκληρώσετε τη ρύθμιση παραμέτρων για τον πόρο που δημιουργήσατε προηγουμένως εφαρμογής πύλης.

>[AZURE.NOTE] Πριν να εκχωρήσετε μια τιμή σε κάθε αντικείμενο ρύθμισης παραμέτρων, πρέπει να δηλώσετε το είδος του αντικειμένου που χρησιμοποιεί PowerShell για χώρο αποθήκευσης. Στην πρώτη γραμμή για να δημιουργήσετε τα μεμονωμένα στοιχεία Καθορίζει ποιες Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (όνομα αντικειμένου) που χρησιμοποιούνται.

### <a name="step-1"></a>Βήμα 1

Δημιουργήστε όλα τα στοιχεία ρύθμισης παραμέτρων μεμονωμένα.

Δημιουργία του προσκηνίου IP, όπως φαίνεται στο παρακάτω παράδειγμα.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Δημιουργήστε τη θύρα προσκηνίου, όπως φαίνεται στο παρακάτω παράδειγμα.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Δημιουργήστε το χώρο συγκέντρωσης server παρασκηνίου.

 Ορισμός στις διευθύνσεις IP που προστίθενται στο χώρο συγκέντρωσης διακομιστή παρασκηνίου, όπως φαίνεται στο επόμενο παράδειγμα.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Χρησιμοποιήστε το αντικείμενο $server για να προσθέσετε τις τιμές του αντικειμένου χώρου συγκέντρωσης παρασκηνίου ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Δημιουργήστε τη ρύθμιση χώρου συγκέντρωσης διακομιστή παρασκηνίου.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Δημιουργήστε το ακροατήριο.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Δημιουργήστε τον κανόνα.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Βήμα 2

Αντιστοίχιση όλα τα στοιχεία μεμονωμένα ρύθμισης παραμέτρων σε ένα αντικείμενο ρύθμισης παραμέτρων πύλης εφαρμογής ($appgwconfig).

Προσθήκη του προσκηνίου IP στη ρύθμιση παραμέτρων.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Προσθέστε τη θύρα προσκηνίου στη ρύθμιση παραμέτρων.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Προσθέστε το χώρο συγκέντρωσης παρασκηνίου server στη ρύθμιση παραμέτρων.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Προσθέστε τη ρύθμιση χώρου συγκέντρωσης παρασκηνίου στη ρύθμιση παραμέτρων.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Προσθέστε το ακροατήριο στη ρύθμιση παραμέτρων.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Προσθέστε τον κανόνα στη ρύθμιση παραμέτρων.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Βήμα 3

Ολοκλήρωση του αντικειμένου ρύθμισης παραμέτρων στον πόρο εφαρμογής πύλης χρησιμοποιώντας το **Σύνολο AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Έναρξη της πύλης

Όταν η πύλη έχει ρυθμιστεί, χρησιμοποιήστε το cmdlet **AzureApplicationGateway έναρξης** για να ξεκινήσετε την πύλη. Χρέωση για μια πύλη εφαρμογής ξεκινά μόλις η πύλη έχει ξεκινήσει με επιτυχία.

> [AZURE.NOTE] Το cmdlet **Έναρξη AzureApplicationGateway** μπορεί να διαρκέσει έως και 15-20 λεπτά για να ολοκληρώσετε.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Επαληθεύστε την κατάσταση πύλης

Χρησιμοποιήστε το cmdlet **Get-AzureApplicationGateway** για να ελέγξετε την κατάσταση της πύλης. Εάν **Έναρξη AzureApplicationGateway** ολοκληρώθηκε με επιτυχία στο προηγούμενο βήμα, θα πρέπει να εκτελεί *κατάσταση* και *Vip* και *DnsName* θα πρέπει να έχετε έγκυρων καταχωρήσεων.

Το παρακάτω παράδειγμα εμφανίζει μια πύλη εφαρμογής που βρίσκεται επάνω, χρησιμοποιείτε, και είστε έτοιμοι να λαμβάνουν την κυκλοφορία που προορίζονται για `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Διαγράψτε μια πύλη εφαρμογής

Για να διαγράψετε μια πύλη εφαρμογής:

1. Χρησιμοποιήστε το cmdlet **Διακοπή AzureApplicationGateway** για να διακόψετε την πύλη.
2. Χρησιμοποιήστε το cmdlet **AzureApplicationGateway κατάργηση** για να καταργήσετε την πύλη.
3. Βεβαιωθείτε ότι η πύλη έχει καταργηθεί, χρησιμοποιώντας το cmdlet **Get-AzureApplicationGateway** .

Το παρακάτω παράδειγμα εμφανίζει το cmdlet **Διακοπή AzureApplicationGateway** στην πρώτη γραμμή, ακολουθούμενο από το αποτέλεσμα.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Όταν η εφαρμογή πύλη είναι σε κατάσταση διακοπής, χρησιμοποιήστε το cmdlet **AzureApplicationGateway κατάργηση** για να καταργήσετε την υπηρεσία.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Για να επαληθεύσετε ότι η υπηρεσία έχει καταργηθεί, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureApplicationGateway** . Αυτό το βήμα δεν είναι απαραίτητο.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μείωση φόρτου SSL, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια πύλη εφαρμογής για το SSL μείωση φόρτου](application-gateway-ssl.md).

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής για χρήση με μια εσωτερική εξισορρόπηση φόρτου, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)](application-gateway-ilb.md).

Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png
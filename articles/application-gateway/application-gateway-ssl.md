<properties
   pageTitle="Ρύθμιση παραμέτρων μιας εφαρμογής πύλης για μείωση φόρτου SSL με χρήση κλασική ανάπτυξης | Microsoft Azure"
   description="Σε αυτό το άρθρο παρέχει οδηγίες για να δημιουργήσετε μια πύλη εφαρμογής με SSL μείωση φόρτου χρησιμοποιώντας το μοντέλο Azure κλασική ανάπτυξης."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Ρύθμιση παραμέτρων μια πύλη εφαρμογής για μείωση φόρτου SSL με χρήση του μοντέλου κλασική ανάπτυξης

> [AZURE.SELECTOR]
-[Πύλη του Azure](application-gateway-ssl-portal.md)
-[Του PowerShell για τη διαχείριση πόρων Azure](application-gateway-ssl-arm.md)
-[Azure κλασική PowerShell](application-gateway-ssl.md)

Azure πύλη εφαρμογής μπορεί να ρυθμιστεί για να τερματίσετε την περίοδο λειτουργίας Secure Sockets Layer (SSL) στην πύλη για να αποφύγετε κοστίζουν εργασίες αποκρυπτογράφησης SSL να συμβεί κατά τη συστοιχία web. Μείωση φόρτου SSL απλοποιεί επίσης τη διαμόρφωση διακομιστή περιβάλλοντος χρήστη και τη διαχείριση της εφαρμογής web.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του Azure PowerShell χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Βεβαιωθείτε ότι έχετε ένα εικονικό δίκτυο εργάζεστε με ένα έγκυρο υποδίκτυο. Βεβαιωθείτε ότι δεν υπάρχουν εικονικές μηχανές ή αναπτύξεις cloud χρησιμοποιούν το υποδίκτυο. Η πύλη εφαρμογής πρέπει να είναι μόνη ένα υποδίκτυο εικονικού δικτύου.
3. Πρέπει να υπάρχει τους διακομιστές που ρυθμίζετε τις παραμέτρους για να χρησιμοποιήσετε την πύλη εφαρμογής ή έχετε αντιστοιχίσει τα τελικά σημεία που δημιουργήθηκε στο το εικονικό δίκτυο ή με μια δημόσια IP/VIP.

Για να ρυθμίσετε τις παραμέτρους SSL μείωση φόρτου σε μια πύλη εφαρμογής, κάντε τα ακόλουθα βήματα με τη σειρά που παρατίθενται:

1. [Δημιουργήστε μια πύλη εφαρμογής](#create-an-application-gateway)
2. [Αποστολή πιστοποιητικά SSL](#upload-ssl-certificates)
3. [Ρύθμιση παραμέτρων της πύλης](#configure-the-gateway)
4. [Ρύθμιση των παραμέτρων της πύλης](#set-the-gateway-configuration)
5. [Έναρξη της πύλης](#start-the-gateway)
6. [Επαληθεύστε την κατάσταση πύλης](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Για να δημιουργήσετε την πύλη, χρησιμοποιήστε το cmdlet **New-AzureApplicationGateway** , αντικαθιστώντας τις τιμές με το δικό σας. Χρέωση για την πύλη δεν ξεκινά σε αυτό το σημείο. Χρέωση αρχίζει αργότερα, όταν ξεκινήσει με επιτυχία με την πύλη.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Για να επικυρώσετε ότι η πύλη που δημιουργήθηκε, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureApplicationGateway** .

Στο το δείγμα, *Περιγραφή*, *InstanceCount*και *GatewaySize* είναι προαιρετικές παραμέτρους. Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μικρές και μεγάλες είναι άλλες διαθέσιμες τιμές. *VirtualIPs* και *DnsName* εμφανίζονται ως κενά επειδή η πύλη δεν έχει αρχίσει ακόμα. Αυτές οι τιμές δημιουργούνται αφού η πύλη είναι σε κατάσταση λειτουργίας.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>Αποστολή πιστοποιητικά SSL

Χρησιμοποιήστε **AzureApplicationGatewaySslCertificate Προσθήκη** για να αποστείλετε το πιστοποιητικό διακομιστή σε μορφή *pfx* για την πύλη εφαρμογής. Το όνομα του πιστοποιητικού είναι το όνομα του επιλεγεί από το χρήστη και πρέπει να είναι μοναδικό μέσα στην πύλη εφαρμογής. Αυτό το πιστοποιητικό αναφέρεται με αυτό το όνομα σε όλες τις εργασίες διαχείρισης πιστοποιητικού της εφαρμογής πύλης.

Το παρακάτω παράδειγμα εμφανίζει το cmdlet, αντικαταστήστε τις τιμές στο δείγμα με το δικό σας.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Στη συνέχεια, επικυρώστε η αποστολή πιστοποιητικού. Χρησιμοποιήστε το cmdlet **Get-AzureApplicationGatewayCertificate** .

Αυτό το δείγμα δείχνει το cmdlet στην πρώτη γραμμή, ακολουθούμενο από το αποτέλεσμα.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Ο κωδικός πρόσβασης πιστοποιητικό πρέπει να είναι μεταξύ 4 έως 12 χαρακτήρες, γράμματα ή αριθμούς. Δεν γίνονται δεκτές ειδικούς χαρακτήρες.

## <a name="configure-the-gateway"></a>Ρύθμιση παραμέτρων της πύλης

Ρύθμιση παραμέτρων πύλης μιας εφαρμογής αποτελείται από πολλές τιμές. Οι τιμές μπορεί να είναι συνδεδεμένη μαζί για να δημιουργήσετε τη ρύθμιση παραμέτρων.

Οι τιμές είναι:

- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο εικονικού δικτύου ή πρέπει να είναι μια δημόσια IP/VIP.
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Προσκηνίου θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα που είναι δυνατό το άνοιγμα της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το ακροατήριο διαθέτει προσκηνίου θύρα, ένα πρωτόκολλο (Http ή Https, αυτές οι τιμές διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου).
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο και το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης. Προς το παρόν, υποστηρίζεται μόνο η *Βασική* κανόνα. Ο κανόνας *βασικές* είναι φόρτωσης round robin διανομής.

**Πρόσθετες ρυθμίσεις παραμέτρων σημειώσεων**

Για ρύθμιση παραμέτρων πιστοποιητικά SSL, το πρωτόκολλο στο **HttpListener** πρέπει να αλλάξετε σε *Https* (με διάκριση πεζών-κεφαλαίων). Το στοιχείο **SslCert** προστίθεται **HttpListener** με την τιμή που έχει οριστεί για το ίδιο όνομα που χρησιμοποιούνται στο η αποστολή του πριν από την ενότητα πιστοποιητικά SSL. Θα πρέπει να ενημερωθούν προσκηνίου τη θύρα 443.

**Για να ενεργοποιήσετε την συσχέτισης που βασίζεται σε cookie**: μια πύλη εφαρμογής μπορεί να ρυθμιστεί για να βεβαιωθείτε ότι μια αίτηση από μια περίοδο λειτουργίας του προγράμματος-πελάτη είναι κατευθυνθεί πάντα την ίδια εικονική Μηχανή στη συστοιχία web. Αυτό το σενάριο γίνεται με την εισαγωγή ενός cookie περιόδου λειτουργίας που επιτρέπει την πύλη για να κατευθύνουν κυκλοφορία σωστά. Για να ενεργοποιήσετε βασίζεται σε cookie συσχέτισης, ορίστε **CookieBasedAffinity** *ενεργοποιημένη* στο στοιχείο **BackendHttpSettings** .



Μπορείτε να δημιουργήσετε τη ρύθμιση των παραμέτρων σας είτε με τη δημιουργία ενός αντικειμένου ρύθμισης παραμέτρων ή με τη χρήση ενός αρχείου XML ρύθμισης παραμέτρων.
Για να δημιουργήσετε ρύθμιση των παραμέτρων σας χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων XML, χρησιμοποιήστε το παρακάτω παράδειγμα:

**Δείγμα XML ρύθμισης παραμέτρων**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Ρύθμιση των παραμέτρων της πύλης

Στη συνέχεια, μπορείτε να ορίσετε την πύλη εφαρμογής. Μπορείτε να χρησιμοποιήσετε το cmdlet **Set-AzureApplicationGatewayConfig** με αντικείμενο ρύθμισης παραμέτρων ή με ένα αρχείο XML ρύθμισης παραμέτρων.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Έναρξη της πύλης

Όταν η πύλη έχει ρυθμιστεί, χρησιμοποιήστε το cmdlet **AzureApplicationGateway έναρξης** για να ξεκινήσετε την πύλη. Χρέωση για μια πύλη εφαρμογής ξεκινά μόλις η πύλη έχει ξεκινήσει με επιτυχία.


**Σημείωση:** Το cmdlet **Έναρξη AzureApplicationGateway** μπορεί να διαρκέσει έως και 15-20 λεπτά για να ολοκληρώσετε.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Επαληθεύστε την κατάσταση πύλης

Χρησιμοποιήστε το cmdlet **Get-AzureApplicationGateway** για να ελέγξετε την κατάσταση της πύλης. Εάν **Έναρξη AzureApplicationGateway** ολοκληρώθηκε με επιτυχία στο προηγούμενο βήμα, θα πρέπει να εκτελεί *κατάσταση* και *VirtualIPs* και *DnsName* θα πρέπει να έχετε έγκυρων καταχωρήσεων.

Αυτό το δείγμα εμφανίζει μια πύλη εφαρμογής που είναι προς τα επάνω, εκτελείται, και είστε έτοιμοι να κρατήσετε κίνηση.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Επόμενα βήματα


Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)
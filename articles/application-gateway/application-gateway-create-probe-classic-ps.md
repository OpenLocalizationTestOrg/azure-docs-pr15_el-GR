<properties
   pageTitle="Δημιουργία ενός προσαρμοσμένου δοκιμή του για εφαρμογή πύλης με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη δοκιμή του εφαρμογής πύλης με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Δημιουργία ενός προσαρμοσμένου δοκιμή του Azure εφαρμογής πύλης (κλασική) με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Πύλη του Azure](application-gateway-create-probe-portal.md)
- [Azure του PowerShell για τη διαχείριση πόρων](application-gateway-create-probe-ps.md)
- [Azure κλασική PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής

Για να δημιουργήσετε μια πύλη εφαρμογής:

1. Δημιουργήστε έναν πόρο πύλης εφαρμογής.
2. Δημιουργήστε ένα αρχείο XML ρύθμισης παραμέτρων ή ένα αντικείμενο ρύθμισης παραμέτρων.
3. Ολοκλήρωση τη ρύθμιση παραμέτρων για τον πόρο πύλης που έχουν δημιουργηθεί πρόσφατα εφαρμογής.

### <a name="create-an-application-gateway-resource"></a>Δημιουργία ενός πόρου πύλης εφαρμογής

Για να δημιουργήσετε την πύλη, χρησιμοποιήστε το cmdlet **New-AzureApplicationGateway** , αντικαθιστώντας τις τιμές με το δικό σας. Χρέωση για την πύλη δεν ξεκινά σε αυτό το σημείο. Χρέωση αρχίζει αργότερα, όταν ξεκινήσει με επιτυχία με την πύλη.

Το παρακάτω παράδειγμα δημιουργεί μια πύλη εφαρμογής, χρησιμοποιώντας ένα εικονικό δίκτυο που ονομάζεται "testvnet1" και ένα δευτερεύον που ονομάζεται "υποδικτύου-1".

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Για να επικυρώσετε ότι η πύλη που δημιουργήθηκε, μπορείτε να χρησιμοποιήσετε το cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μπορείτε να επιλέξετε μεταξύ μικρό, μεσαίο και μεγάλο.

 *VirtualIPs* και *DnsName* εμφανίζονται ως κενά επειδή η πύλη δεν έχει αρχίσει ακόμα. Αυτά δημιουργούνται αφού η πύλη είναι σε κατάσταση λειτουργίας.

## <a name="configure-an-application-gateway"></a>Ρύθμιση παραμέτρων μια πύλη εφαρμογής

Μπορείτε να ρυθμίσετε την πύλη εφαρμογής με τη χρήση XML ή ένα αντικείμενο ρύθμισης παραμέτρων.

## <a name="configure-an-application-gateway-by-using-xml"></a>Ρύθμιση παραμέτρων μιας εφαρμογής πύλης με χρήση XML

Στο παρακάτω παράδειγμα, μπορείτε να χρησιμοποιήσετε ένα αρχείο XML για να ρυθμίσετε όλες τις ρυθμίσεις πύλης εφαρμογής και ολοκλήρωση τους στον πόρο πύλης εφαρμογής.  

### <a name="step-1"></a>Βήμα 1

Αντιγράψτε το ακόλουθο κείμενο στο Σημειωματάριο.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Επεξεργαστείτε τις τιμές μεταξύ των παρενθέσεων για τα στοιχεία ρύθμισης παραμέτρων. Αποθηκεύστε το αρχείο με επέκταση .xml.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε ένα αρχείο ρύθμισης παραμέτρων για να ρυθμίσετε την πύλη εφαρμογής, για να φορτώσετε υπολοίπου κυκλοφορία HTTP στη δημόσια 80 και στείλτε κίνηση του δικτύου παρασκηνίου θύρα 80 μεταξύ δύο διευθύνσεων IP, χρησιμοποιώντας μια προσαρμοσμένη δοκιμή του.

>[AZURE.IMPORTANT] Το στοιχείο πρωτοκόλλου Http ή Https διάκριση πεζών-κεφαλαίων.

Ένα νέο στοιχείο ρύθμισης παραμέτρων \<Διερεύνηση\> προστίθεται για ρύθμιση παραμέτρων του προσαρμοσμένου καθετήρες.

Οι παράμετροι είναι οι εξής:

- **Όνομα** - όνομα αναφοράς για το προσαρμοσμένο δοκιμή του.
- **Protocol** - πρωτόκολλο που χρησιμοποιείται (πιθανών τιμών είναι HTTP ή HTTPS).
- **Κεντρικός υπολογιστής** και **διαδρομή** - διαδρομή πλήρη διεύθυνση URL που ενεργοποιείται από την πύλη εφαρμογής για να προσδιορίσετε την εύρυθμη λειτουργία της παρουσίας. Για παράδειγμα, εάν έχετε μια τοποθεσία Web http://contoso.com/, στη συνέχεια, το προσαρμοσμένο δοκιμή του μπορούν να ρυθμιστούν για "http://contoso.com/path/custompath.htm" για δοκιμή του ελέγχους για να έχετε μια επιτυχημένη απόκριση HTTP.
- **Διάστημα** - ρυθμίζει τις παραμέτρους των ελέγχων δοκιμή του χρονικού διαστήματος σε δευτερόλεπτα.
- **Λήξη χρονικού ορίου** - Καθορίζει το χρονικό όριο δοκιμή του για έλεγχο απόκρισης HTTP.
- **UnhealthyThreshold** - ο αριθμός των αποτυχημένων αποκρίσεις HTTP χρειάζεται να επισημαίνει την παρουσία παρασκηνίου ως *κατεστραμμένες*.

Το όνομα δοκιμή του γίνεται αναφορά σε το <BackendHttpSettings> ρύθμιση παραμέτρων για να αντιστοιχίσετε ποια χώρου συγκέντρωσης παρασκηνίου χρησιμοποιεί δοκιμή του προσαρμοσμένες ρυθμίσεις.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Προσθέστε μια ρύθμιση προσαρμοσμένης δοκιμή του σε μια υπάρχουσα πύλη εφαρμογής

Αλλαγή της τρέχουσας ρύθμισης παραμέτρων από μια πύλη εφαρμογής απαιτεί τρία βήματα: λήψη του τρέχοντος αρχείου ρύθμισης παραμέτρων XML, τροποποίηση να έχετε ένα προσαρμοσμένο δοκιμή του και ρύθμιση παραμέτρων της πύλης εφαρμογής με τις νέες ρυθμίσεις XML.

### <a name="step-1"></a>Βήμα 1

Λήψη του αρχείου XML με τη χρήση get-AzureApplicationGatewayConfig. Κάτι τέτοιο εξαγάγει τη ρύθμιση παραμέτρων XML για να τροποποιηθούν για να προσθέσετε μια ρύθμιση δοκιμή του.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Βήμα 2

Ανοίξτε το αρχείο XML σε ένα πρόγραμμα επεξεργασίας κειμένου. Προσθήκη μιας `<probe>` ενότητα μετά `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Στην ενότητα backendHttpSettings του XML, προσθέστε το όνομα δοκιμή του, όπως φαίνεται στο ακόλουθο παράδειγμα:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Αποθηκεύστε το αρχείο XML.

### <a name="step-3"></a>Βήμα 3

Ενημέρωση η ρύθμιση παραμέτρων πύλης με το νέο αρχείο XML χρησιμοποιώντας το **Σύνολο AzureApplicationGatewayConfig**. Αυτό ενημερώνει την πύλη εφαρμογής με τη νέα ρύθμιση παραμέτρων.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Επόμενα βήματα

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μείωση φόρτου Secure Sockets Layer (SSL), ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μια πύλη εφαρμογής για το SSL μείωση φόρτου](application-gateway-ssl.md).

Εάν θέλετε να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής για χρήση με μια εσωτερική εξισορρόπηση φόρτου, ανατρέξτε στο θέμα [Δημιουργία μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)](application-gateway-ilb.md).

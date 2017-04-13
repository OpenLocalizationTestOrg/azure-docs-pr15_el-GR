<properties 
   pageTitle="Δημιουργία και ρύθμιση παραμέτρων μια πύλη εφαρμογής με εσωτερική εξισορρόπηση φόρτου (ILB) σε ένα εικονικό δίκτυο | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει οδηγίες για να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής Azure με εσωτερική εξισορρόπηση φόρτου τελικού σημείου"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Δημιουργήστε μια πύλη εφαρμογής με μια εσωτερική εξισορρόπηση φόρτου (ILB)

> [AZURE.SELECTOR]
- [Azure κλασική βήματα](application-gateway-ilb.md)
- [Τα βήματα του Powershell για τη διαχείριση πόρων](application-gateway-ilb-arm.md)

Πύλη εφαρμογής μπορούν να ρυθμιστούν με internet αντικριστές εικονικό IP ή με εσωτερική τελικό σημείο δεν εκτίθεται στο Internet, γνωστή και ως τελικό σημείο του εσωτερικού εξισορρόπησης φόρτου (ILB). Ρύθμιση παραμέτρων της πύλης με μια ILB είναι χρήσιμη για εσωτερικές γραμμής επιχειρηματικές εφαρμογές δεν εκτίθεται στο internet. Είναι επίσης χρήσιμο για υπηρεσίες/βαθμίδες μέσα σε μια εφαρμογή πολλαπλών επιπέδων, το οποίο βρίσκεται σε ένα όριο ασφαλείας δεν εκτίθεται στο internet, αλλά εξακολουθούν να απαιτούν round robin φόρτωσης διανομής, υποστεί περιόδου λειτουργίας ή τερματισμού SSL. Σε αυτό το άρθρο σάς καθοδηγεί σε τα βήματα για να ρυθμίσετε τις παραμέτρους μια πύλη εφαρμογής με μια ILB.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

1. Εγκαταστήστε την πιο πρόσφατη έκδοση του τα cmdlet του PowerShell Azure χρησιμοποιώντας το πρόγραμμα εγκατάστασης πλατφόρμας Web. Μπορείτε να κάνετε λήψη και εγκατάσταση της πιο πρόσφατης έκδοσης από την ενότητα **Του Windows PowerShell** από τη [σελίδα λήψης](https://azure.microsoft.com/downloads/).
2. Βεβαιωθείτε ότι έχετε ένα εικονικό δίκτυο εργασίας με έγκυρη υποδικτύου.
3. Βεβαιωθείτε ότι έχετε διακομιστές παρασκηνίου είτε στο δίκτυο εικονικού είτε με μια δημόσια στους οποίους έχουν ανατεθεί IP/VIP.


Για να δημιουργήσετε μια πύλη εφαρμογής, εκτελέστε τα ακόλουθα βήματα με τη σειρά που παρατίθενται. 

1. [Δημιουργήστε μια πύλη εφαρμογής](#create-a-new-application-gateway)
2. [Ρύθμιση παραμέτρων της πύλης](#configure-the-gateway)
3. [Ρύθμιση των παραμέτρων της πύλης](#set-the-gateway-configuration)
4. [Έναρξη της πύλης](#start-the-gateway)
4. [Επαλήθευση της πύλης](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Δημιουργήστε μια πύλη εφαρμογής:

**Για να δημιουργήσετε την πύλη**, χρησιμοποιήστε το `New-AzureApplicationGateway` cmdlet, αντικαθιστώντας τις τιμές με το δικό σας. Σημειώστε ότι χρεώσεις για την πύλη δεν θα ξεκινήσει αυτήν τη στιγμή. Χρέωση αρχίζει αργότερα, όταν ξεκινήσει με επιτυχία με την πύλη.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Για να επικυρώσετε** ότι η πύλη που δημιουργήθηκε, μπορείτε να χρησιμοποιήσετε το `Get-AzureApplicationGateway` cmdlet. 

Στο το δείγμα, *Περιγραφή*, *InstanceCount*και *GatewaySize* είναι προαιρετικές παραμέτρους. Η προεπιλεγμένη τιμή για το *InstanceCount* είναι 2, με μια μέγιστη τιμή 10. Η προεπιλεγμένη τιμή για *GatewaySize* είναι το μεσαίο. Μικρές και μεγάλες είναι άλλες διαθέσιμες τιμές. *VIP* και *DnsName* εμφανίζονται ως κενά επειδή η πύλη δεν έχει αρχίσει ακόμα. Αυτά δημιουργούνται αφού η πύλη είναι σε κατάσταση λειτουργίας. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Ρύθμιση παραμέτρων της πύλης

Ρύθμιση παραμέτρων πύλης μιας εφαρμογής αποτελείται από πολλές τιμές. Οι τιμές μπορεί να είναι συνδεδεμένη μαζί για να δημιουργήσετε τη ρύθμιση παραμέτρων.
 
Οι τιμές είναι:

- **Σύνολο διακομιστή παρασκηνίου:** Η λίστα διευθύνσεων IP των διακομιστών παρασκηνίου. Οι διευθύνσεις IP που παρατίθενται θα πρέπει να ανήκουν είτε στο υποδίκτυο VNet ή πρέπει να είναι μια δημόσια IP/VIP. 
- **Ρυθμίσεων χώρου συγκέντρωσης διακομιστή παρασκηνίου:** Κάθε χώρος συγκέντρωσης έχει ρυθμίσεις όπως θύρα, το πρωτόκολλο και βασίζονται σε cookie συσχέτισης. Αυτές οι ρυθμίσεις είναι συνδεδεμένη με ένα χώρο συγκέντρωσης και εφαρμόζονται σε όλους τους διακομιστές εντός του χώρου συγκέντρωσης.
- **Frontend θύρας:** Αυτήν τη θύρα είναι η δημόσια θύρα ανοίξει της εφαρμογής πύλης. Κίνηση επισκέψεις αυτήν τη θύρα και, στη συνέχεια, γίνεται ανακατεύθυνση σε έναν από τους διακομιστές παρασκηνίου.
- **Ακρόασης:** Το πρόγραμμα ακρόασης έχει μια θύρα frontend, ένα πρωτόκολλο (Http ή Https, αυτοί είναι διάκριση πεζών-κεφαλαίων), και το όνομα του πιστοποιητικού SSL (εάν τη ρύθμιση των παραμέτρων SSL μείωση φόρτου). 
- **Κανόνα:** Ο κανόνας συνδέει το ακροατήριο και το χώρο συγκέντρωσης server παρασκηνίου και καθορίζει ποιο σύνολο διακομιστή παρασκηνίου την κυκλοφορία πρέπει να απευθύνονται όταν το επισκέψεις μιας συγκεκριμένης υπηρεσίας ακρόασης. Προς το παρόν, υποστηρίζεται μόνο η *Βασική* κανόνα. Ο κανόνας *βασικές* είναι φόρτωσης round robin διανομής.

Μπορείτε να δημιουργήσετε τη ρύθμιση των παραμέτρων σας είτε με τη δημιουργία ενός αντικειμένου ρύθμισης παραμέτρων, ή με τη χρήση ενός αρχείου XML ρύθμισης παραμέτρων. Για να δημιουργήσετε ρύθμιση των παραμέτρων σας χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων XML, χρησιμοποιήστε το παρακάτω δείγμα.



Λάβετε υπόψη τα εξής:


- Το στοιχείο *FrontendIPConfigurations* περιγράφει τις σχετικές για τη ρύθμιση παραμέτρων πύλη εφαρμογής με ένα ILB ILB λεπτομέρειες. 

- Το Frontend IP, *Τύπος* πρέπει να οριστεί σε "Ιδιωτικό"

- Το *StaticIPAddress* πρέπει να οριστεί ως το επιθυμητό εσωτερική IP στην οποία η πύλη λαμβάνει την κυκλοφορία. Σημειώστε ότι το στοιχείο *StaticIPAddress* είναι προαιρετικό. Εάν δεν σύνολο, επιλέγεται διαθέσιμη εσωτερική IP από το ανεπτυγμένος υποδίκτυο. 

- Η τιμή του στοιχείου *όνομα* που καθορίζεται στο *FrontendIPConfiguration* πρέπει να χρησιμοποιείται στο στοιχείο *FrontendIP* το HTTPListener να αναφέρεται το FrontendIPConfiguration.

 **Δείγμα XML ρύθμισης παραμέτρων**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Ρύθμιση των παραμέτρων της πύλης

Στη συνέχεια, θα μπορείτε να ορίσετε την πύλη εφαρμογής. Μπορείτε να χρησιμοποιήσετε το `Set-AzureApplicationGatewayConfig` cmdlet με ένα αντικείμενο ρύθμισης παραμέτρων ή με ένα αρχείο XML ρύθμισης παραμέτρων. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Έναρξη της πύλης

Μόλις η πύλη έχει ρυθμιστεί, χρησιμοποιήστε το `Start-AzureApplicationGateway` cmdlet για να ξεκινήσετε την πύλη. Χρέωση για μια πύλη εφαρμογής ξεκινά μόλις η πύλη έχει ξεκινήσει με επιτυχία. 


> [AZURE.NOTE] Το `Start-AzureApplicationGateway` cmdlet ενδέχεται να χρειαστούν έως 15-20 λεπτά για να ολοκληρωθεί. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Επαληθεύστε την κατάσταση πύλης

Χρησιμοποιήστε το `Get-AzureApplicationGateway` cmdlet για να ελέγξετε την κατάσταση της πύλης. Εάν *Έναρξη AzureApplicationGateway* ολοκληρώθηκε με επιτυχία στο προηγούμενο βήμα, την κατάσταση πρέπει να *εκτελούνται*και η Vip και DnsName θα πρέπει να έχετε έγκυρων καταχωρήσεων. Αυτό το δείγμα δείχνει το cmdlet στην πρώτη γραμμή, ακολουθούμενο από το αποτέλεσμα. Σε αυτό το δείγμα, η πύλη εκτελείται και είστε έτοιμοι να την κυκλοφορία. 

> [AZURE.NOTE] Η πύλη εφαρμογής έχει ρυθμιστεί για να αποδεχτείτε την κίνηση στο ρυθμισμένο τελικού σημείου ILB του 10.0.0.10 σε αυτό το παράδειγμα.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Επόμενα βήματα


Εάν θέλετε περισσότερες πληροφορίες σχετικά με τη φόρτωση εξισορρόπησης επιλογές σε γενικές γραμμές, ανατρέξτε στα θέματα:

- [Azure εξισορρόπηση φόρτου](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Διαχείριση Azure κίνηση](https://azure.microsoft.com/documentation/services/traffic-manager/)

<properties 
    pageTitle="Αρχιτεκτονική ασφαλείας σε επίπεδα με περιβάλλοντα εφαρμογής υπηρεσίας" 
    description="Εφαρμογή μια αρχιτεκτονική σε επίπεδα ασφαλείας με εφαρμογή υπηρεσίας περιβάλλοντα." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Εφαρμογή μια αρχιτεκτονική σε επίπεδα ασφαλείας με περιβάλλοντα εφαρμογής υπηρεσίας

## <a name="overview"></a>Επισκόπηση ##
 
Εφόσον εφαρμογής υπηρεσίας περιβάλλοντα παρέχουν ένα περιβάλλον απομόνωσης χρόνου εκτέλεσης αναπτυχθεί σε ένα εικονικό δίκτυο, οι προγραμματιστές να δημιουργήσετε μια αρχιτεκτονική σε επίπεδα ασφαλείας, παρέχοντας διαφορετικά επίπεδα πρόσβασης δικτύου για κάθε επίπεδο φυσικής εφαρμογής.

Ένα κοινό ενδιαφέρον είναι να αποκρύψετε τελειώνει API πίσω από τη γενική πρόσβαση στο Internet και να επιτρέπονται μόνο API για να καλείται από εφαρμογές web νέα.  [Δίκτυο ομάδες ασφαλείας (NSGs)] [ NetworkSecurityGroups] μπορεί να χρησιμοποιηθεί σε δευτερεύοντα δίκτυα που περιέχει περιβάλλοντα εφαρμογής υπηρεσίας για να περιορίσετε την πρόσβαση του κοινού για τις εφαρμογές του API.

Το παρακάτω διάγραμμα εμφανίζει ένα παράδειγμα αρχιτεκτονική με μια εφαρμογή WebAPI βάσει αναπτυχθεί σε ένα περιβάλλον εφαρμογής υπηρεσίας.  Τρεις παρουσίες εφαρμογής web ξεχωριστά, αναπτυχθεί σε τρεις ξεχωριστές εφαρμογής υπηρεσίας περιβάλλοντα, πραγματοποίηση κλήσεων παρασκηνίου στην ίδια εφαρμογή WebAPI.

![Εννοιολογική αρχιτεκτονικής][ConceptualArchitecture] 

Στο πράσινο σύμβολο συν υποδεικνύουν ότι η ομάδα ασφαλείας δικτύου στο υποδίκτυο που περιέχει "apiase" επιτρέπει εισερχόμενες κλήσεις από την επόμενη web apps, ως στις κλήσεις από τον εαυτό.  Ωστόσο την ίδια ομάδα ασφαλείας δικτύου ρητά αποτρέπει την πρόσβαση σε γενικές εισερχόμενη κυκλοφορία από το Internet. 

Το υπόλοιπο της αυτό το θέμα παρουσιάζει τα βήματα που απαιτούνται για να ρυθμίσετε την ομάδα ασφαλείας δικτύου στο υποδίκτυο που περιέχει "apiase".

## <a name="determining-the-network-behavior"></a>Καθορισμός της συμπεριφοράς δικτύου ##
Για να γνωρίζετε ποιες κανόνες ασφαλείας δικτύου είναι απαραίτητες, πρέπει να προσδιορίσετε ποια προγράμματα-πελάτες δικτύου θα επιτρέπεται να φτάσει στο περιβάλλον εφαρμογής υπηρεσίας που περιέχει την εφαρμογή API και θα αποκλειστούν ποια προγράμματα-πελάτες.

Από [τις ομάδες ασφαλείας δικτύου (NSGs)] [ NetworkSecurityGroups] εφαρμόζονται σε δευτερεύοντα δίκτυα, και εφαρμογή υπηρεσίας περιβάλλοντα έχουν αναπτυχθεί σε δευτερεύοντα δίκτυα, εφαρμόζονται οι κανόνες που περιέχονται σε μια NSG για **όλες** τις εφαρμογές που εκτελείται σε ένα περιβάλλον εφαρμογής υπηρεσίας.  Χρησιμοποιώντας το δείγμα αρχιτεκτονική για αυτό το άρθρο, όταν μια ομάδα ασφαλείας δικτύου έχει εφαρμοστεί στο υποδίκτυο που περιέχει "apiase", όλες οι εφαρμογές που εκτελούνται στο περιβάλλον εφαρμογής υπηρεσίας "apiase" θα προστατεύονται με το ίδιο σύνολο των κανόνων ασφαλείας. 

- **Προσδιορίσετε τη διεύθυνση IP εξερχομένων νέα καλούντες:**  Τι είναι η διεύθυνση IP ή τις διευθύνσεις την επόμενη καλούντες;  Αυτές οι διευθύνσεις θα πρέπει να ρητά επιτρέπεται η πρόσβαση στο το NSG.  Επειδή το κλήσεις μεταξύ της εφαρμογής υπηρεσίας περιβάλλοντα θεωρούνται κλήσεις "Internet", αυτό σημαίνει ότι η διεύθυνση IP εξερχομένων που έχουν εκχωρηθεί σε καθένα από τα τρία περιβάλλοντα υπηρεσίας νέα εφαρμογή πρέπει να επιτρέπεται η πρόσβαση στο το NSG για το υποδίκτυο "apiase".   Για περισσότερες λεπτομέρειες σχετικά με τον καθορισμό της διεύθυνσης IP εξερχομένων για εφαρμογές που εκτελούνται σε ένα περιβάλλον εφαρμογής υπηρεσίας ανατρέξτε στο θέμα η [Αρχιτεκτονική δικτύου] [ NetworkArchitecture] άρθρο Επισκόπηση.
- **Η εφαρμογή API παρασκηνίου χρειάζονται κλήση στον εαυτό;**  Ένα σημείο ορισμένες φορές παραβλέπεται και διακριτική είναι το σενάριο όπου η εφαρμογή παρασκηνίου πρέπει να καλέσετε ίδια.  Εάν μια εφαρμογή API παρασκηνίου σε ένα περιβάλλον εφαρμογής υπηρεσίας πρέπει να καλέσετε ίδια, αυτό επίσης αντιμετωπίζεται ως μια κλήση "Internet".  Στην αρχιτεκτονική δείγμα αυτό απαιτεί επιτρέπει την πρόσβαση από τη διεύθυνση IP εξερχομένων από το "apiase" εφαρμογή υπηρεσίας περιβάλλον καθώς και.

## <a name="setting-up-the-network-security-group"></a>Ρύθμιση του δικτύου ομάδας ασφαλείας ##
Μόλις το σύνολο των εξερχόμενων διευθύνσεων IP είναι γνωστό, το επόμενο βήμα είναι να δημιουργήσετε μια ομάδα ασφαλείας δικτύου.  Δίκτυο ομάδες ασφαλείας μπορούν να δημιουργηθούν για δύο διαχείριση πόρων βάσει εικονικών δικτύων, καθώς και κλασική εικονικού δίκτυα.  Στα παρακάτω παραδείγματα δείχνουν τη δημιουργία και τη ρύθμιση των παραμέτρων ενός NSG κλασική εικονικού δικτύου με χρήση του Powershell.

Για την αρχιτεκτονική δείγμα, τα περιβάλλοντα βρίσκονται στη Νότια κεντρικής ΜΑΣ, ώστε να δημιουργηθεί μια κενή NSG στη συγκεκριμένη περιοχή:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Πρώτα ρητή επιτρέπουν κανόνας προστίθεται για την υποδομή Azure διαχείρισης όπως σημειώνεται στο άρθρο στην [εισερχόμενη κυκλοφορία] [ InboundTraffic] για εφαρμογή υπηρεσίας περιβάλλοντα.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Στη συνέχεια, προστίθενται δύο κανόνες, για να επιτρέψετε HTTP και HTTPS κλήσεις από την πρώτη νέα εφαρμογή υπηρεσίας περιβάλλον ("fe1ase").

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Ξεπλένουμε και επαναλάβετε για το δεύτερο και τρίτο νέα εφαρμογή υπηρεσίας περιβάλλοντα ("fe2ase" και "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Τέλος, μπορείτε να εκχωρήσετε πρόσβαση σε τη διεύθυνση IP εξερχομένων περιβάλλοντος υπηρεσιών εφαρμογής το API του παρασκηνίου, έτσι ώστε το να καλέσετε στον εαυτό.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Δεν υπάρχει άλλους κανόνες ασφάλειας δικτύου πρέπει να ρυθμιστούν επειδή κάθε NSG έχει ένα σύνολο προεπιλεγμένους κανόνες που αποκλείει εισερχόμενης πρόσβασης από το Internet από προεπιλογή.

Την πλήρη λίστα των κανόνων, στην ομάδα ασφαλείας δικτύου εμφανίζονται παρακάτω.  Σημειώστε τον τρόπο που το τελευταίο κανόνα που έχει επισημανθεί, εμποδίζει εισερχόμενης πρόσβασης από όλους τους καλούντες διαφορετικό από εκείνους που έχουν εκχωρηθεί πρόσβαση.

![Ρύθμιση παραμέτρων NSG][NSGConfiguration] 

Το τελικό βήμα είναι να εφαρμόσετε το NSG στο υποδίκτυο που περιέχει την εφαρμογή υπηρεσίας περιβάλλον "apiase".  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Με το NSG εφαρμόζονται στο υποδίκτυο, μόνο τα τρία νέα εφαρμογή υπηρεσίας περιβάλλοντα επιτρέπονται, και το περιβάλλον υπηρεσίας εφαρμογής που περιέχει το API παρασκηνίου, για να καλέσετε το περιβάλλον "apiase".


## <a name="additional-links-and-information"></a>Πρόσθετες συνδέσεις και πληροφορίες ##
Όλα τα άρθρα και με ποιον τρόπο-για να του για εφαρμογή υπηρεσίας περιβάλλοντα είναι διαθέσιμες στο το [αρχείο README για περιβάλλοντα εφαρμογών υπηρεσίας](../app-service/app-service-app-service-environments-readme.md).

Πληροφορίες σχετικά με [τις ομάδες ασφαλείας δικτύου](../virtual-network/virtual-networks-nsg.md). 

Κατανόηση των [εξερχόμενων διευθύνσεων IP] [ NetworkArchitecture] και εφαρμογή υπηρεσίας περιβάλλοντα.

[Θύρες δικτύου] [ InboundTraffic] χρησιμοποιούνται από την εφαρμογή υπηρεσίας περιβάλλοντα.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png

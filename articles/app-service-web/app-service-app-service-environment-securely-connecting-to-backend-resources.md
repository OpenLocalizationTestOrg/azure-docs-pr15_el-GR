<properties 
    pageTitle="Σύνδεση με ασφάλεια σε πόρους υποστήριξης από ένα περιβάλλον εφαρμογής υπηρεσίας" 
    description="Μάθετε σχετικά με τον τρόπο για την ασφαλή σύνδεση με πόρους υποστήριξης από ένα περιβάλλον εφαρμογής υπηρεσίας." 
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
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Σύνδεση με ασφάλεια σε πόρους υποστήριξης από ένα περιβάλλον εφαρμογής υπηρεσίας #

## <a name="overview"></a>Επισκόπηση ##
Επειδή το περιβάλλον εφαρμογής υπηρεσίας δημιουργείται πάντα στο **είτε** ένα διαχειριστή πόρων Azure εικονικού δικτύου, **ή** μια κλασική ανάπτυξη του μοντέλου [εικονικού δικτύου][virtualnetwork], εξερχόμενες συνδέσεις από ένα περιβάλλον εφαρμογής υπηρεσίας με άλλους πόρους υποστήριξης να ροής αποκλειστικά μέσω του εικονικού δικτύου.  Με μια πρόσφατη αλλαγή που κάνατε στο 2016 Ιουνίου, ASEs μπορεί επίσης να αναπτυχθεί σε εικονικό δίκτυα που χρησιμοποιούν είτε δημόσια διεύθυνση περιοχές ή κενά διαστήματα διεύθυνση RFC1918 (π.χ. Οι ιδιωτικές διευθύνσεις).  

Για παράδειγμα, μπορεί να υπάρχουν SQL Server εκτελείται σε ένα σύμπλεγμα εικονικές μηχανές με θύρα 1433 κλειδωμένο προς τα κάτω.  Το τελικό σημείο μπορεί να είναι ACLd να μόνο να επιτρέπεται η πρόσβαση από άλλους πόρους στο ίδιο δίκτυο εικονικού.  

Ένα άλλο παράδειγμα, διάκριση πεζών-κεφαλαίων τελικά σημεία μπορεί να εκτελεστεί εσωτερικής εγκατάστασης και να συνδεθεί με Azure μέσω δύο [Τοποθεσίας σε τοποθεσία] [ SiteToSite] ή [Azure ExpressRoute] [ ExpressRoute] συνδέσεις.  Ως αποτέλεσμα, μόνο τους πόρους εικονικού δίκτυα, συνδεθείτε με την τοποθεσία σε τοποθεσία ή σήραγγες ExpressRoute θα μπορούν να έχουν πρόσβαση στην εσωτερική εγκατάσταση τελικά σημεία.

Όλα αυτά τα σενάρια, εφαρμογές που εκτελείται σε ένα περιβάλλον υπηρεσίας εφαρμογή θα έχει τη δυνατότητα για την ασφαλή σύνδεση για τα διάφορα διακομιστές και τους πόρους.  Εξερχόμενη κυκλοφορία από τις εφαρμογές που εκτελούνται σε ένα περιβάλλον εφαρμογής υπηρεσίας σε ιδιωτική τελικά σημεία στο ίδιο δίκτυο εικονικού (ή συνδεδεμένοι στο ίδιο δίκτυο εικονικού), θα μόνο ροής μέσω του εικονικού δικτύου.  Εξερχόμενη κυκλοφορία για ιδιωτικό τελικά σημεία δεν θα ροής μέσω του δημόσιου Internet.

Ότι ισχύει για την εξερχόμενη κυκλοφορία από ένα περιβάλλον εφαρμογής υπηρεσίας για να τα τελικά σημεία μέσα σε ένα εικονικό δίκτυο.  Εφαρμογή υπηρεσίας περιβάλλοντα δεν μπορούν να συνδεθούν τελικά σημεία της εικονικές μηχανές που βρίσκεται στο **ίδιο** υποδίκτυο με το περιβάλλον εφαρμογής υπηρεσίας.  Αυτό συνήθως δεν πρέπει να κάποιο πρόβλημα με την προϋπόθεση ότι εφαρμογής υπηρεσίας περιβάλλοντα έχουν αναπτυχθεί σε ένα υποδίκτυο δεσμευμένη για αποκλειστική χρήση από μόνο του περιβάλλοντος εφαρμογής υπηρεσίας.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Απαιτήσεις DNS και σύνδεσης εξερχομένων ##
Για ένα περιβάλλον υπηρεσίας εφαρμογής για να λειτουργήσει σωστά, απαιτεί εξερχομένων πρόσβαση σε διάφορες τελικά σημεία. Μια πλήρη λίστα των τα τελικά σημεία εξωτερικών που χρησιμοποιούνται από ένα ASE βρίσκεται στην ενότητα "Απαιτείται η συνδεσιμότητα του δικτύου" αυτού του άρθρου [Παραμέτρους δικτύου για ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

Εφαρμογή υπηρεσίας περιβάλλοντα απαιτούν έγκυρη υποδομή DNS έχουν ρυθμιστεί για το εικονικό δίκτυο.  Εάν για κάποιο λόγο τη ρύθμιση παραμέτρων DNS αλλάξει αφού δημιουργηθεί ένα περιβάλλον εφαρμογής υπηρεσίας, οι προγραμματιστές να επιβάλετε ένα περιβάλλον υπηρεσίας εφαρμογή, μπορείτε να ορίσετε τη νέα ρύθμιση παραμέτρων του DNS.  Ενεργοποίηση συνάθροισης επανεκκίνηση περιβάλλον χρησιμοποιώντας το εικονίδιο "Επανεκκίνηση" που βρίσκεται στο επάνω μέρος του blade διαχείρισης εφαρμογής υπηρεσίας περιβάλλον στην πύλη του θα προκαλέσει το περιβάλλον μπορείτε να ορίσετε τη νέα ρύθμιση παραμέτρων του DNS.

Συνιστάται επίσης ότι τυχόν προσαρμοσμένες τους διακομιστές DNS στο το vnet είναι ρύθμισης εκ των προτέρων πριν από τη δημιουργία εφαρμογής υπηρεσίας περιβάλλοντος.  Εάν ένα εικονικό δίκτυο ρύθμισης παραμέτρων DNS αλλάξουν ενώ δημιουργείται ένα περιβάλλον εφαρμογής υπηρεσίας, που θα προκαλέσει την αποτυχία διαδικασία δημιουργίας εφαρμογής υπηρεσίας περιβάλλον.  Σε μια παρόμοια ραβδώσεων, εάν υπάρχει ένα προσαρμοσμένο διακομιστή DNS από την άλλη πλευρά μιας πύλης VPN και το διακομιστή DNS είναι δυνατή η πρόσβαση ή δεν είναι διαθέσιμη, η διαδικασία δημιουργίας εφαρμογής υπηρεσίας περιβάλλον θα επίσης αποτύχει.

## <a name="connecting-to-a-sql-server"></a>Σύνδεση με SQL Server
Μια κοινή ρύθμιση παραμέτρων του SQL Server περιλαμβάνει ένα τελικό σημείο ακρόαση στη θύρα 1433:

![Τελικό σημείο του SQL Server][SqlServerEndpoint]

Υπάρχουν δύο προσεγγίσεις για τον περιορισμό κίνηση σε αυτό το τελικό σημείο:


- [Λίστες ελέγχου πρόσβασης δικτύου] [ NetworkAccessControlLists] (δικτύου ACL)

- [Ομάδες ασφαλείας δικτύου][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Περιορισμός πρόσβασης με ένα δίκτυο ACL

Θύρα 1433 μπορεί να είναι ασφαλείς χρησιμοποιώντας μια λίστα ελέγχου πρόσβασης δικτύου.  Το παράδειγμα κάτω από το πρόγραμμα-πελάτη whitelists διευθύνσεις που προέρχονται από μέσα σε ένα εικονικό δίκτυο και εμποδίζει την πρόσβαση σε όλα τα άλλα προγράμματα-πελάτες.

![Παράδειγμα λίστας ελέγχου πρόσβασης δικτύου][NetworkAccessControlListExample]

Τυχόν εφαρμογές που εκτελούνται σε περιβάλλον εφαρμογής υπηρεσίας στο ίδιο δίκτυο εικονικού με τον SQL Server θα μπορούν να συνδεθείτε με την παρουσία του SQL Server με τη διεύθυνση IP **VNet εσωτερικού** για την εικονική μηχανή SQL Server.  

Η συμβολοσειρά σύνδεσης παράδειγμα παρακάτω αναφορές του SQL Server, χρησιμοποιώντας την ιδιωτική διεύθυνση IP.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Παρόλο που η εικονική μηχανή έχει καθώς και μια δημόσια τελικό σημείο, θα απορρίπτονται λόγω του δικτύου ACL προσπάθειες σύνδεσης με τη δημόσια διεύθυνση IP. 

## <a name="restricting-access-with-a-network-security-group"></a>Περιορισμός πρόσβασης με μια ομάδα ασφαλείας δικτύου
Εναλλακτική προσέγγιση για την ασφάλεια των πρόσβασης είναι με μια ομάδα ασφαλείας δικτύου.  Δίκτυο ομάδες ασφαλείας μπορούν να εφαρμοστούν μεμονωμένα εικονικές μηχανές ή σε ένα δευτερεύον που περιέχει εικονικές μηχανές.

Μια ομάδα ασφαλείας δικτύου πρέπει πρώτα να δημιουργηθεί:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Περιορισμός πρόσβασης σε μόνο εσωτερική κυκλοφορία VNet είναι πολύ απλής με μια ομάδα ασφαλείας δικτύου.  Μόνο τους προεπιλεγμένους κανόνες σε μια ομάδα ασφαλείας δικτύου επιτρέπεται η πρόσβαση από άλλα προγράμματα-πελάτες δικτύου στο ίδιο δίκτυο εικονικού.

Κλείδωμα του ως αποτέλεσμα πρόσβαση σε SQL Server είναι τόσο απλή όσο η εφαρμογή μιας ομάδας ασφαλείας δικτύου με τους κανόνες του προεπιλεγμένου σε είτε τις εικονικές μηχανές εκτελεί SQL Server ή το υποδίκτυο που περιέχει τις εικονικές μηχανές.

Το παρακάτω δείγμα ισχύει μια ομάδα ασφαλείας δικτύου για το οποίο περιέχει υποδίκτυο:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Το τελικό αποτέλεσμα είναι ένα σύνολο κανόνων ασφαλείας που αποκλείει εξωτερικής πρόσβασης, επιτρέποντας εσωτερική πρόσβαση VNet:

![Προεπιλεγμένους κανόνες ασφαλείας δικτύου][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Γρήγορα αποτελέσματα
Όλα τα άρθρα και με ποιον τρόπο-για να του για εφαρμογή υπηρεσίας περιβάλλοντα είναι διαθέσιμες στο το [αρχείο README για περιβάλλοντα εφαρμογών υπηρεσίας](../app-service/app-service-app-service-environments-readme.md).

Για να ξεκινήσετε με εφαρμογή υπηρεσίας περιβάλλοντα, ανατρέξτε στο θέμα [Εισαγωγή στο περιβάλλον εφαρμογής υπηρεσίας][IntroToAppServiceEnvironment]

Για λεπτομέρειες γύρω από τον έλεγχο εισερχόμενη κυκλοφορία για το περιβάλλον σας εφαρμογής υπηρεσίας, δείτε [τον έλεγχο εισερχόμενη κυκλοφορία σε ένα περιβάλλον εφαρμογής υπηρεσίας][ControlInboundASE]

Για περισσότερες πληροφορίες σχετικά με την πλατφόρμα Azure εφαρμογής υπηρεσίας, ανατρέξτε στο θέμα [Azure εφαρμογής υπηρεσίας][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 

<properties 
    pageTitle="Ρύθμιση παραμέτρων τείχος προστασίας εφαρμογών Web (WAF) για το περιβάλλον εφαρμογής υπηρεσίας" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους τείχος προστασίας εφαρμογής web μπροστά από το περιβάλλον εφαρμογής υπηρεσίας." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Ρύθμιση παραμέτρων τείχος προστασίας εφαρμογών Web (WAF) για το περιβάλλον εφαρμογής υπηρεσίας

## <a name="overview"></a>Επισκόπηση ##
Web τείχη προστασίας εφαρμογή όπως το [WAF Barracuda για Azure](https://www.barracuda.com/programs/azure) που είναι διαθέσιμες στο το ασφαλούς τις εφαρμογές web με έλεγχο εισερχόμενη κυκλοφορία web για να αποκλείσετε εγχύσεις SQL, διατοποθεσιακή δημιουργία δέσμης ενεργειών, λογισμικό κακόβουλης λειτουργίας αποστολές & εφαρμογή DDoS και άλλες επιθέσεις βοηθά [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) . Το επίσης ελέγχει τις απαντήσεις από τους διακομιστές web παρασκηνίου για την αποτροπή απώλειας δεδομένων (DLP). Σε συνδυασμό με την απομόνωση και πρόσθετες κλίμακας που παρέχεται από την εφαρμογή υπηρεσίας περιβάλλοντα, παρέχει περιβάλλον ιδανικό για host επαγγελματικές εφαρμογές web κρίσιμες που πρέπει να δέχονται κακόβουλο αιτήσεις και μεγάλο όγκο κίνηση.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Το πρόγραμμα εγκατάστασης ##
Για αυτό το έγγραφο θα σας θα ρυθμίσετε τις παραμέτρους μας περιβάλλον εφαρμογής υπηρεσίας πίσω από πολλούς φόρτωσης εξισορρόπηση παρουσίες Barracuda WAF, έτσι ώστε μόνο με την κυκλοφορία από το WAF να επικοινωνήσετε μαζί του περιβάλλοντος εφαρμογής υπηρεσίας και δεν θα είναι προσβάσιμα από το DMZ. Έχουμε επίσης Manager κίνηση Azure μπροστά από μας παρουσίες Barracuda WAF για εξισορρόπηση του φόρτου σε κέντρα δεδομένων Azure και περιοχές. Ένα διάγραμμα υψηλού επιπέδου της ρύθμισης θα είναι παρόμοιο τι εμφανίζεται κάτω από το στοιχείο.

![Αρχιτεκτονική][Architecture] 

> Σημείωση: Με την εισαγωγή του [ILB υποστήριξης για το περιβάλλον εφαρμογής υπηρεσίας](app-service-environment-with-internal-load-balancer.md), μπορείτε να ρυθμίσετε το ASE για να μην είναι προσβάσιμα από το DMZ και είναι διαθέσιμες μόνο σε ιδιωτικό δίκτυο. 

## <a name="configuring-your-app-service-environment"></a>Ρύθμιση παραμέτρων του περιβάλλοντος εφαρμογής υπηρεσίας ##
Για να ρυθμίσετε ένα περιβάλλον εφαρμογής υπηρεσίας ανατρέξτε [την τεκμηρίωση](app-service-web-how-to-create-an-app-service-environment.md) το θέμα. Όταν έχετε ένα περιβάλλον υπηρεσίας εφαρμογής που δημιουργήσατε, μπορείτε να δημιουργήσετε [Εφαρμογές Web](app-service-web-overview.md), [API εφαρμογές](../app-service-api/app-service-api-apps-why-best-platform.md) και [Εφαρμογές του Mobile](../app-service-mobile/app-service-mobile-value-prop.md) σε αυτό το περιβάλλον που θα προστατεύονται πίσω από το WAF μας ρύθμιση παραμέτρων στην επόμενη ενότητα.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Ρύθμιση παραμέτρων της υπηρεσίας WAF Cloud Barracuda ##
Barracuda έχει μια [λεπτομερή άρθρο](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) σχετικά με την ανάπτυξη του WAF σε μια εικονική μηχανή στο Azure. Αλλά επειδή θέλετε πλεονασμού και Παρουσιάστε δεν ένα μοναδικό σημείο αποτυχίας, που θέλετε να αναπτύξετε τουλάχιστον 2 ΣΠΣ παρουσία WAF στην ίδια υπηρεσία Cloud, όταν ακολουθήσετε αυτές τις οδηγίες.

### <a name="adding-endpoints-to-cloud-service"></a>Προσθήκη τελικά σημεία για την υπηρεσία στο Cloud ###
Όταν έχετε 2 ή περισσότερες παρουσίες Εικονική WAF στην υπηρεσία Cloud μπορείτε να χρησιμοποιήσετε την [πύλη του Azure](https://portal.azure.com/) για να προσθέσετε HTTP και HTTPS τελικά σημεία που χρησιμοποιούνται από την εφαρμογή σας, όπως φαίνεται στην παρακάτω εικόνα.

![Ρύθμιση παραμέτρων του τελικού σημείου][ConfigureEndpoint]

Εάν οι εφαρμογές σας χρησιμοποιούν άλλες τελικά σημεία, βεβαιωθείτε ότι για να προσθέσετε αυτές σε αυτήν τη λίστα καθώς και. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Ρύθμιση παραμέτρων Barracuda WAF έως την πύλη διαχείρισης ###
Barracuda WAF χρησιμοποιεί 8000 θύρες TCP για ρύθμιση παραμέτρων έως την πύλη διαχείρισης. Επειδή έχουμε πολλές παρουσίες του ΣΠΣ WAF θα πρέπει να επαναλάβετε τα βήματα που περιγράφονται εδώ για κάθε παρουσία Εικονική. 


> Σημείωση: Όταν τελειώσετε με τη ρύθμιση παραμέτρων WAF, καταργήστε το τελικό σημείο TCP/8000 από όλες τις ΣΠΣ WAF για να διατηρήσει ασφαλείς τις WAF.

Προσθέστε το τελικό σημείο διαχείρισης, όπως φαίνεται στην παρακάτω εικόνα για να ρυθμίσετε τις παραμέτρους του WAF Barracuda.

![Προσθήκη τελικού σημείου διαχείρισης][AddManagementEndpoint]
 
Χρησιμοποιήστε ένα πρόγραμμα περιήγησης για να αναζητήσετε το τελικό σημείο διαχείρισης την υπηρεσία Cloud. Εάν την υπηρεσία Cloud ονομάζεται test.cloudapp.net, μπορείτε να αποκτήσετε πρόσβαση σε αυτό το τελικό σημείο κάνοντας αναζήτηση για http://test.cloudapp.net:8000. Θα πρέπει να δείτε μια σελίδα σύνδεσης όπως κάτω από ότι μπορείτε να συνδεθείτε χρησιμοποιώντας τα διαπιστευτήρια που καθορίσατε κατά τη φάση ρύθμισης Εικονική WAF.

![Σελίδα "Διαχείριση σύνδεσης"][ManagementLoginPage]

Μία φορά σύνδεσης που θα πρέπει να βλέπετε έναν πίνακα εργαλείων με αυτή που θα παρουσιάζουν βασικές στατιστικά στοιχεία σχετικά με την προστασία WAF στην παρακάτω εικόνα.

![Πίνακας εργαλείων διαχείρισης][ManagementDashboard]

Κάνοντας κλικ στην καρτέλα υπηρεσίες θα σάς επιτρέπουν να ρυθμίσετε τις παραμέτρους του WAF για την προστασία των υπηρεσιών. Για περισσότερες λεπτομέρειες σχετικά με τη ρύθμιση των παραμέτρων σας WAF Barracuda μπορείτε να ανατρέξετε [στην τεκμηρίωση τους](https://techlib.barracuda.com/waf/getstarted1). Στο παράδειγμα κάτω από μια εφαρμογή Web της Azure που εξυπηρετούν κίνηση σε HTTP και HTTPS έχει ρυθμιστεί.

![Διαχείριση προσθέσετε υπηρεσίες][ManagementAddServices]

> Σημείωση: Ανάλογα με τον τρόπο έχουν ρυθμιστεί οι εφαρμογές σας και τις δυνατότητες που χρησιμοποιούνται στο περιβάλλον σας εφαρμογής υπηρεσίας, θα πρέπει να προωθήσετε την κυκλοφορία για TCP θύρες εκτός από 80 και 443, π.χ. Εάν έχετε IP SSL εγκατάστασης για μια εφαρμογή Web. Για μια λίστα των θύρες δικτύου που χρησιμοποιούνται στα περιβάλλοντα εφαρμογής υπηρεσίας, ανατρέξτε [στην τεκμηρίωση του στοιχείου ελέγχου εισερχόμενη κυκλοφορία](app-service-app-service-environment-control-inbound-traffic.md) ενότητα θύρες δικτύου.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Ρύθμιση παραμέτρων διαχείρισης κίνηση του Microsoft Azure (ΠΡΟΑΙΡΕΤΙΚΆ) ##
Εάν η εφαρμογή σας είναι διαθέσιμη σε πολλές περιοχές, στη συνέχεια, θα θέλετε να φορτώσετε υπόλοιπο τους πίσω από το [Azure κίνηση Manager](../traffic-manager/traffic-manager-overview.md). Για να το κάνετε, ώστε να μπορείτε να προσθέσετε ένα τελικό σημείο στην [πύλη του Azure κλασική](https://manage.azure.com) χρησιμοποιώντας το όνομα υπηρεσία Cloud για το WAF στο προφίλ Manager κίνηση, όπως φαίνεται στην παρακάτω εικόνα. 

![Διαχείριση κίνηση τελικού σημείου][TrafficManagerEndpoint]

Εάν η εφαρμογή σας απαιτεί έλεγχο ταυτότητας, βεβαιωθείτε ότι έχετε ορισμένες πόρο που δεν απαιτούν κάθε ελέγχου ταυτότητας για τη Διαχείριση κίνηση για να κάνετε ping για τη διαθεσιμότητα της εφαρμογής σας. Μπορείτε να ρυθμίσετε τη διεύθυνση URL στην ενότητα Ρύθμιση παραμέτρων στην [πύλη του Azure κλασική](https://manage.azure.com) , όπως φαίνεται παρακάτω.

![Ρύθμιση παραμέτρων διαχείρισης κίνηση][ConfigureTrafficManager]

Για να προωθήσετε την ping που κίνηση Manager από το WAF στην εφαρμογή σας, πρέπει να ρύθμισης μεταφράσεων τοποθεσίας Web σε σας WAF Barracuda για την προώθηση κίνηση στην εφαρμογή σας, όπως φαίνεται στο παρακάτω παράδειγμα.

![Τοποθεσία Web μεταφράσεων][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Διασφάλιση της κυκλοφορίας περιβάλλον εφαρμογής υπηρεσίας, χρησιμοποιώντας τις ομάδες ασφαλείας δικτύου (NSG)##
Ακολουθήστε την [τεκμηρίωση του στοιχείου ελέγχου εισερχόμενη κυκλοφορία](app-service-app-service-environment-control-inbound-traffic.md) για λεπτομέρειες σε περιορισμός κυκλοφορία για το περιβάλλον σας εφαρμογή υπηρεσίας από το WAF μόνο, χρησιμοποιώντας τη διεύθυνση VIP της υπηρεσίας Cloud. Ακολουθεί ένα δείγμα εντολών του Powershell για την εκτέλεση αυτής της εργασίας για τη θύρα TCP 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Αντικαταστήστε το SourceAddressPrefix με την εικονική διεύθυνση IP (VIP) από μια υπηρεσία Cloud της WAF σας.

> Σημείωση: Το VIP της υπηρεσίας Cloud σας θα αλλάξει όταν διαγράφετε και να δημιουργήσετε ξανά την υπηρεσία Cloud. Βεβαιωθείτε ότι για να ενημερώσετε τη διεύθυνση IP στην ομάδα πόρων δικτύου, αφού ολοκληρώσετε τη διαδικασία. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png

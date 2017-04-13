<properties 
    pageTitle="Εισαγωγή στο περιβάλλον εφαρμογής υπηρεσίας" 
    description="Μάθετε περισσότερα σχετικά με τη δυνατότητα εφαρμογής υπηρεσίας περιβάλλον που παρέχει μονάδες κλίμακας ασφαλείς, VNet συνδεθεί, αποκλειστικό για την εκτέλεση όλων από τις εφαρμογές σας." 
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

# <a name="introduction-to-app-service-environment"></a>Εισαγωγή στο περιβάλλον εφαρμογής υπηρεσίας

## <a name="overview"></a>Επισκόπηση ##
Περιβάλλον εφαρμογής υπηρεσίας είναι [Premium] [ PremiumTier] υπηρεσίας πρόγραμμα επιλογή Azure εφαρμογής υπηρεσίας που παρέχει μια πλήρως απομόνωσης και αποκλειστικό περιβάλλον για την ασφαλή εκτέλεση Azure εφαρμογής υπηρεσίας εφαρμογών σε υψηλή κλίμακα, συμπεριλαμβανομένων των [Εφαρμογών Web][WebApps], [Οι εφαρμογές του Mobile][MobileApps], και [Εφαρμογές API][APIApps].  

Περιβάλλοντα εφαρμογής υπηρεσίας είναι ιδανική για φόρτους εργασίας εφαρμογών που απαιτεί:

- Πολύ υψηλή κλίμακας
- Απομόνωση και ασφαλή πρόσβαση στο δίκτυο

Οι πελάτες να δημιουργήσετε πολλά περιβάλλοντα υπηρεσίας εφαρμογής μέσα σε μία μόνο περιοχή Azure, καθώς και σε πολλές περιοχές Azure.  Με αυτόν τον τρόπο εφαρμογής υπηρεσίας περιβάλλοντα ιδανικό για την οριζόντια κλιμάκωση βαθμίδες κατάσταση λιγότερο εφαρμογής που υποστηρίζουν υψηλή RPS φόρτους εργασίας.

Περιβάλλοντα εφαρμογής υπηρεσίας είναι απομονωμένες για να εκτελείτε μόνο ένα μεμονωμένο πελάτη εφαρμογές και αναπτύσσονται πάντα σε ένα εικονικό δίκτυο.  Οι πελάτες έχουν λεπτομερή έλεγχο και τα δύο κίνηση του δικτύου εισερχόμενων και εξερχόμενων εφαρμογής και εφαρμογές να δημιουργήσετε συνδέσεις υψηλής ταχύτητας ασφαλούς μέσω εικονικών δικτύων για εταιρικούς πόρους εσωτερικής εγκατάστασης.

Όλα τα άρθρα και με ποιον τρόπο-για να του σχετικά με την εφαρμογή υπηρεσίας περιβάλλοντα είναι διαθέσιμες στο το [αρχείο README για περιβάλλοντα εφαρμογών υπηρεσίας](../app-service/app-service-app-service-environments-readme.md).

Για μια επισκόπηση του τον τρόπο εφαρμογής υπηρεσίας περιβάλλοντα ενεργοποίησης υψηλή κλίμακα και εξασφάλισης της πρόσβαση στο δίκτυο, ανατρέξτε στο θέμα την [Κατάρρευση βαθύ AzureCon] [ AzureConDeepDive] στην εφαρμογή υπηρεσίας περιβάλλοντα!

Για βάθος-κατάρρευση σχετικά με την οριζόντια κλιμάκωση χρησιμοποιώντας πολλά περιβάλλοντα υπηρεσίας εφαρμογών, ανατρέξτε στο άρθρο σχετικά με τον τρόπο ρύθμισης ενός [αποτύπωμα παν κατανεμημένης εφαρμογής][GeodistributedAppFootprint].

Για να δείτε πώς έχει ρυθμιστεί η αρχιτεκτονική ασφαλείας εμφανίζονται στο την κατάρρευση βαθύ AzureCon, ανατρέξτε στο άρθρο σχετικά με την εφαρμογή ενός [επίπεδα αρχιτεκτονική ασφαλείας](app-service-app-service-environment-layered-security.md) με εφαρμογή υπηρεσίας περιβάλλοντα.

Εφαρμογές που εκτελείται σε εφαρμογή υπηρεσίας περιβάλλοντα μπορεί να έχει την πρόσβασή τους διαχειρίζονται μέσω πύλης από νέα συσκευές όπως τα τείχη προστασίας εφαρμογών web (WAF).  Το άρθρο σχετικά με [τη ρύθμιση ενός WAF για εφαρμογή υπηρεσίας περιβάλλοντα](app-service-app-service-environment-web-application-firewall.md) καλύπτει αυτό το σενάριο. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Πόροι αποκλειστικό υπολογισμού ##
Όλοι οι πόροι υπολογισμού σε ένα περιβάλλον εφαρμογής υπηρεσίας είναι αφοσιωμένη αποκλειστικά σε μια μεμονωμένη συνδρομή και ένα περιβάλλον εφαρμογής υπηρεσίας μπορούν να ρυθμιστούν με έως πενήντα (50) υπολογισμού πόρους για αποκλειστική χρήση μόνο από μια εφαρμογή.

Περιβάλλον εφαρμογής υπηρεσίας αποτελείται από ένα χώρο συγκέντρωσης πόρων προσκηνίου υπολογισμού, καθώς και χώρους συγκέντρωσης πόρων υπολογισμού ένα έως τρία εργασίας. 

Ο χώρος συγκέντρωσης προσκηνίου περιέχει πόρους υπολογισμού υπεύθυνοι για τερματισμό SSL ως εξισορρόπηση φόρτου και αυτόματη των αιτήσεων εφαρμογών μέσα σε ένα περιβάλλον εφαρμογής υπηρεσίας. 

Κάθε ομάδα εργασίας περιέχει πόρους υπολογισμού εκχωρηθεί σε [Εφαρμογή υπηρεσίας προγράμματος][AppServicePlan], που περιέχουν μία ή περισσότερες εφαρμογές Azure εφαρμογής υπηρεσίας.  Επειδή μπορεί να είναι έως και τρεις διαφορετικές εργαζόμενου χώρους συγκέντρωσης σε ένα περιβάλλον εφαρμογής υπηρεσίας, έχετε την ευελιξία να επιλέξετε διαφορετικό υπολογισμού πόρους για κάθε ομάδα εργασίας.  

Για παράδειγμα, αυτό σας επιτρέπει για να δημιουργήσετε μία ομάδα εργασίας με λιγότερο ισχυρά υπολογισμού πόρους για εφαρμογή υπηρεσίας προγράμματος που προορίζονται για εφαρμογές ανάπτυξης ή ελέγχου.  Μια ομάδα εργασίας δεύτερη (ή ακόμη και τρίτο) μπορούν να χρησιμοποιήσουν πιο ισχυρή υπολογισμού πόρους που προορίζονται για την εφαρμογή υπηρεσίας προγράμματος εκτέλεση παραγωγής εφαρμογών.

Για περισσότερες λεπτομέρειες σχετικά με την ποσότητα του υπολογισμού διαθέσιμοι πόροι για τους χώρους συγκέντρωσης προσκηνίου και εργαζόμενου, δείτε [πώς μπορείτε να ρύθμιση παραμέτρων περιβάλλον εφαρμογής υπηρεσίας][HowToConfigureanAppServiceEnvironment].  

Για λεπτομέρειες σχετικά με τα διαθέσιμα τον υπολογισμό μεγέθη πόρων που υποστηρίζονται σε ένα περιβάλλον εφαρμογής υπηρεσίας, επικοινωνήστε με την [Εφαρμογή υπηρεσίας τιμολόγησης] [ AppServicePricing] σελίδα και να αναθεωρήσετε τις διαθέσιμες επιλογές για την εφαρμογή υπηρεσίας περιβάλλοντα στο το Premium τις τιμές σε επίπεδο.

## <a name="virtual-network-support"></a>Υποστήριξη εικονικού δικτύου ##
Μπορεί να δημιουργηθεί ένα περιβάλλον εφαρμογής υπηρεσίας στο **είτε** ένα διαχειριστή πόρων Azure εικονικού δικτύου, **ή** μια κλασική ανάπτυξη του μοντέλου εικονικού δικτύου ([περισσότερες πληροφορίες για εικονικών δικτύων][MoreInfoOnVirtualNetworks]).  Επειδή το περιβάλλον εφαρμογής υπηρεσίας υπάρχει πάντα σε ένα εικονικό δίκτυο και μεγαλύτερη ακρίβεια μέσα σε ένα δευτερεύον ένα εικονικό δίκτυο, μπορείτε να αξιοποιήσετε τις δυνατότητες ασφαλείας των εικονικών δικτύων για να ελέγξετε και οι δύο δικτύου εισερχόμενες και εξερχόμενες επικοινωνίες.  

Περιβάλλον εφαρμογής υπηρεσίας μπορεί να είναι είτε στο Internet αντικριστές με μια δημόσια διεύθυνση IP, ή εσωτερικό αντικριστές με μόνο μια διεύθυνση εσωτερικού εξισορρόπησης φόρτου Azure (ILB).

Μπορείτε να χρησιμοποιήσετε [τις ομάδες ασφαλείας δικτύου] [ NetworkSecurityGroups] για να περιορίσετε τις επικοινωνίες δικτύου εισερχόμενη στο υποδίκτυο όπου βρίσκεται το περιβάλλον εφαρμογής υπηρεσίας.  Αυτό σας επιτρέπει να εκτελέσετε εφαρμογές πίσω από το νέα συσκευές και υπηρεσίες όπως τείχη προστασίας εφαρμογής web και οι υπηρεσίες παροχής ΑΔΑ δικτύου.

Εφαρμογές πρέπει επίσης συχνά για πρόσβαση σε εταιρικούς πόρους, όπως εσωτερικές βάσεις δεδομένων και υπηρεσίες web.  Μια κοινή προσέγγιση είναι να κάνετε αυτά τα τελικά σημεία διαθέσιμο μόνο σε εσωτερικές κίνηση του δικτύου ροή μέσα σε μια Azure εικονικού δικτύου.  Όταν ένα περιβάλλον εφαρμογής υπηρεσίας είναι συνδεδεμένος στο ίδιο δίκτυο εικονικού ως τις υπηρεσίες του εσωτερικού, εφαρμογές που εκτελούνται στο περιβάλλον να αποκτήσετε πρόσβαση τους, συμπεριλαμβανομένων των τελικά σημεία προσβάσιμος μέσω [Τοποθεσίας σε τοποθεσία] [ SiteToSite] και [Azure ExpressRoute] [ ExpressRoute] συνδέσεις.

Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο εφαρμογής υπηρεσίας περιβάλλοντα εργασίας με εικονικού δίκτυα και δίκτυα εσωτερικής εγκατάστασης, ανατρέξτε στα ακόλουθα άρθρα στην [Αρχιτεκτονική δικτύου][NetworkArchitectureOverview], [Έλεγχος εισερχομένων κίνηση][ControllingInboundTraffic], και [Με ασφάλεια τη σύνδεση στο παρασκήνιο][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Γρήγορα αποτελέσματα

Για να ξεκινήσετε με εφαρμογή υπηρεσίας περιβάλλοντα, ανατρέξτε στο θέμα [Πώς να δημιουργήσετε μια εφαρμογή υπηρεσίας περιβάλλον][HowToCreateAnAppServiceEnvironment]

Όλα τα άρθρα και με ποιον τρόπο-για να του για εφαρμογή υπηρεσίας περιβάλλοντα είναι διαθέσιμες στο το [αρχείο README για περιβάλλοντα εφαρμογών υπηρεσίας](../app-service/app-service-app-service-environments-readme.md).

Για περισσότερες πληροφορίες σχετικά με την πλατφόρμα Azure εφαρμογής υπηρεσίας, ανατρέξτε στο θέμα [Azure εφαρμογής υπηρεσίας][AzureAppService].

Για μια επισκόπηση της εφαρμογής υπηρεσίας περιβάλλον αρχιτεκτονικής δικτύου, δείτε την [Επισκόπηση αρχιτεκτονική δικτύου] [ NetworkArchitectureOverview] το άρθρο.

Για λεπτομέρειες σχετικά με ένα περιβάλλον εφαρμογής υπηρεσίας με ExpressRoute, ανατρέξτε στο ακόλουθο άρθρο στην [εφαρμογή υπηρεσίας περιβάλλοντα και δρομολόγηση Express][NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 

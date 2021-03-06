<properties 
    pageTitle="Επισκόπηση αρχιτεκτονικής δικτύου της εφαρμογής υπηρεσίας περιβάλλοντα" 
    description="Επισκόπηση της αρχιτεκτονικής του δικτύου τοπολογία ofApp περιβάλλοντα τεχνικής υποστήριξης." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Επισκόπηση αρχιτεκτονικής δικτύου της εφαρμογής υπηρεσίας περιβάλλοντα

## <a name="introduction"></a>Εισαγωγή ##
Εφαρμογή υπηρεσίας περιβάλλοντα δημιουργούνται πάντα μέσα σε ένα δευτερεύον [εικονικού δικτύου] [ virtualnetwork] -εφαρμογές που εκτελείται σε ένα περιβάλλον εφαρμογής υπηρεσίας μπορούν να επικοινωνούν με ιδιωτική τελικά σημεία που βρίσκεται μέσα στην ίδια τοπολογία εικονικού δικτύου.  Επειδή οι πελάτες μπορεί να κλειδώσετε τμήματα της υποδομής εικονικού δικτύου τους, είναι σημαντικό να κατανοήσετε τους τύπους των ροών επικοινωνίας δικτύου που προκύπτουν με εφαρμογή υπηρεσίας περιβάλλον.

## <a name="general-network-flow"></a>Γενική ροή δικτύου ##
 
Όταν μια εφαρμογή υπηρεσίας περιβάλλον (ASE) χρησιμοποιεί μια δημόσια εικονική διεύθυνση IP (VIP) για τις εφαρμογές, όλη την εισερχόμενη κυκλοφορία φτάνει στη συγκεκριμένη δημόσια VIP.  Περιλαμβάνονται και οι HTTP και HTTPS κίνηση για εφαρμογές, καθώς και άλλες κίνηση για FTP, λειτουργία απομακρυσμένου εντοπισμού σφαλμάτων και λειτουργίες Azure διαχείρισης.  Για μια πλήρη λίστα των τις συγκεκριμένες θύρες (υποχρεωτική και προαιρετική) που είναι διαθέσιμες σε τη δημόσια VIP, ανατρέξτε στο άρθρο σχετικά με [τον έλεγχο της εισερχόμενης κυκλοφορίας] [ controllinginboundtraffic] σε ένα περιβάλλον εφαρμογής υπηρεσίας. 

Εφαρμογή υπηρεσίας περιβάλλοντα υποστηρίζουν επίσης εκτελείται εφαρμογές που έχουν συνδεθεί μόνο σε μια εσωτερική διεύθυνση εικονικού δικτύου, επίσης γνωστή ως μια διεύθυνση ILB (εσωτερική εξισορρόπηση φόρτου).  Σε ένα ILB με δυνατότητα ASE, HTTP και HTTPS κίνηση για εφαρμογές, καθώς και απομακρυσμένο εντοπισμού σφαλμάτων κλήσεις, να φτάσει στη διεύθυνση ILB.  Για τις πιο συνηθισμένες ρυθμίσεις ILB ASE, FTP/FTPS κίνηση θα φτάνουν επίσης στη διεύθυνση ILB.  Λειτουργίες διαχείρισης Azure θα εξακολουθούν να ροής θύρες 454/455 τη δημόσια VIP από μια ILB ενεργοποιημένη ωστόσο ASE.

Στο παρακάτω διάγραμμα δείχνει μια επισκόπηση των διαφόρων ροών εισερχόμενων και εξερχόμενων δικτύου για ένα περιβάλλον υπηρεσίας εφαρμογής όπου οι εφαρμογές είναι συνδεδεμένα με μια εικονική δημόσια διεύθυνση IP:

![Γενικά ροών δικτύου][GeneralNetworkFlows]

Περιβάλλον εφαρμογής υπηρεσίας μπορούν να επικοινωνούν με μια ποικιλία τελικά σημεία ιδιωτικό πελατών.  Για παράδειγμα, εφαρμογές που εκτελούνται στο περιβάλλον εφαρμογής υπηρεσίας να συνδεθείτε με διακομιστές βάσης δεδομένων που εκτελείται σε εικονικές μηχανές IaaS στην ίδια τοπολογία εικονικού δικτύου.

>[AZURE.IMPORTANT] Εξετάζοντας διάγραμμα δικτύου, το "άλλες τον υπολογισμό πόρων" έχουν αναπτυχθεί σε διαφορετικό υποδίκτυο από το περιβάλλον της εφαρμογής υπηρεσίας. Ανάπτυξη πόρων στο ίδιο υποδίκτυο με το ASE θα αποκλεισμός συνδεσιμότητας από ASE σε αυτούς τους πόρους (με εξαίρεση το συγκεκριμένο εντός-ASE δρομολόγηση). Ανάπτυξη σε ένα διαφορετικό υποδίκτυο αντί για αυτό (σε το ίδιο VNET). Το περιβάλλον υπηρεσίας εφαρμογής, στη συνέχεια, θα μπορείτε να συνδεθείτε. Χωρίς πρόσθετες ρυθμίσεις παραμέτρων είναι απαραίτητο.

Εφαρμογή υπηρεσίας περιβάλλοντα επικοινωνία επίσης με Sql DB και αποθήκευσης Azure πόρους που απαιτούνται για τη διαχείριση και λειτουργικό περιβάλλον εφαρμογής υπηρεσίας.  Ορισμένους από τους πόρους Sql και του χώρου αποθήκευσης που σας ενημερώνει για ένα περιβάλλον εφαρμογής υπηρεσίας με βρίσκονται στην ίδια περιοχή ως περιβάλλον εφαρμογής υπηρεσίας, ενώ άλλοι βρίσκονται σε απομακρυσμένο Azure περιφέρειες.  Ως αποτέλεσμα, εξερχομένων τη σύνδεση στο Internet είναι πάντα απαιτείται για ένα περιβάλλον εφαρμογής υπηρεσίας για να λειτουργήσει σωστά. 

Επειδή το περιβάλλον εφαρμογής υπηρεσίας έχει αναπτυχθεί σε ένα υποδίκτυο, ομάδες ασφαλείας δικτύου μπορεί να χρησιμοποιηθεί για τον έλεγχο της εισερχόμενης κυκλοφορίας στο υποδίκτυο.  Για λεπτομέρειες σχετικά με τον τρόπο για να ελέγξετε την εισερχόμενη κυκλοφορία σε ένα περιβάλλον υπηρεσίας εφαρμογών, ανατρέξτε στο ακόλουθο [άρθρο][controllinginboundtraffic].

Για λεπτομέρειες σχετικά με τον τρόπο για να επιτρέψετε εξερχομένων σύνδεσης στο Internet από ένα περιβάλλον υπηρεσίας εφαρμογών, ανατρέξτε στο ακόλουθο άρθρο σχετικά με την εργασία με [Δρομολόγηση Express][ExpressRoute].  Η ίδια διαδικασία που περιγράφεται στο άρθρο ισχύει όταν εργάζεστε με σύνδεση τοποθεσίας σε τοποθεσία και χρήση υποχρεωτική διοχέτευση.

## <a name="outbound-network-addresses"></a>Διευθύνσεις εξερχομένων δικτύου ##
Όταν ένα περιβάλλον εφαρμογής υπηρεσίας κάνει εξερχόμενες κλήσεις, μια διεύθυνση IP πάντα είναι συσχετισμένη με το εξερχόμενες κλήσεις.  Η συγκεκριμένη διεύθυνση IP που χρησιμοποιείται εξαρτάται από το εάν βρίσκεται το τελικό σημείο που ονομάζεται εντός της τοπολογίας εικονικού δικτύου ή εκτός της τοπολογίας εικονικού δικτύου.

Εάν το τελικό σημείο που ονομάζεται είναι **εκτός** των της τοπολογίας εικονικού δικτύου, στη συνέχεια, τη διεύθυνση εξερχομένων (ή η εξερχόμενη διεύθυνση NAT) που χρησιμοποιείται είναι η δημόσια VIP του περιβάλλοντος εφαρμογής υπηρεσίας.  Μπορείτε να βρείτε αυτήν τη διεύθυνση στο περιβάλλον εργασίας χρήστη της πύλης για το περιβάλλον εφαρμογής υπηρεσίας στο blade ιδιότητες.
 
![Διεύθυνση IP εξερχομένων][OutboundIPAddress]

Αυτή η διεύθυνση μπορεί επίσης να προσδιοριστεί για ASEs που έχουν μόνο μια δημόσια VIP, δημιουργώντας μια εφαρμογή στο περιβάλλον εφαρμογής υπηρεσίας και, στη συνέχεια, εκτελεί ένα *nslookup* στη διεύθυνση της εφαρμογής. Η διεύθυνση IP που προκύπτει είναι τόσο τη δημόσια VIP, καθώς και την εφαρμογή υπηρεσίας περιβάλλον εξερχομένων NAT διεύθυνση.

Εάν το τελικό σημείο που ονομάζεται είναι **μέσα** στην τοπολογία εικονικού δικτύου, τη διεύθυνση εξερχομένων της κλήσης εφαρμογής θα την εσωτερική διεύθυνση IP του πόρου μεμονωμένα υπολογισμού με την εφαρμογή.  Ωστόσο δεν υπάρχει μια μόνιμη αντιστοίχιση εικονικού δικτύου εσωτερικές διευθύνσεις IP εφαρμογές.  Εφαρμογές μπορούν να μετακινηθείτε σε διαφορετικές υπολογισμού πόρων και το χώρο συγκέντρωσης διαθέσιμη υπολογισμού πόρων σε ένα περιβάλλον εφαρμογής υπηρεσίας μπορούν να αλλάζουν λόγω κλίμακας λειτουργίες.

Ωστόσο, επειδή το περιβάλλον εφαρμογής υπηρεσίας πάντα βρίσκεται μέσα σε ένα υποδίκτυο, που είναι εγγυημένη ότι την εσωτερική διεύθυνση IP του πόρου υπολογισμού εκτελείται μια εφαρμογή πάντα θα βρίσκονται μέσα στην περιοχή CIDR του υποδικτύου.  Ως αποτέλεσμα, όταν λεπτομερή ACL ή τις ομάδες ασφαλείας δικτύου που χρησιμοποιούνται για την ασφαλή πρόσβαση σε άλλα τελικά σημεία στο εσωτερικό του εικονικού δικτύου, η περιοχή υποδικτύου που περιέχει το περιβάλλον εφαρμογής υπηρεσίας πρέπει να επιτρέπεται η πρόσβαση.

Το παρακάτω διάγραμμα παρουσιάζει αυτά τα θέματα με περισσότερες λεπτομέρειες:

![Διευθύνσεις εξερχομένων δικτύου][OutboundNetworkAddresses]

Στο παραπάνω διάγραμμα:

- Εφόσον το δημόσιο VIP του περιβάλλοντος εφαρμογής υπηρεσίας είναι 192.23.1.2, που είναι η εξερχομένων διεύθυνση IP που χρησιμοποιήθηκε κατά την πραγματοποίηση κλήσεων για τα τελικά σημεία "Internet".
- Η περιοχή CIDR του περιέχει υποδικτύου για το περιβάλλον εφαρμογής υπηρεσίας είναι 10.0.1.0/26.  Άλλα τελικά σημεία μέσα την ίδια υποδομή εικονικού δικτύου θα δείτε τις κλήσεις από τις εφαρμογές ως που προέρχονται από κάπου αλλού μέσα σε αυτή η περιοχή διευθύνσεων.

## <a name="calls-between-app-service-environments"></a>Κλήσεις μεταξύ της εφαρμογής υπηρεσίας περιβάλλοντα ##
Μια πιο σύνθετη περίπτωση μπορεί να προκύψει εάν αναπτύξετε πολλά περιβάλλοντα υπηρεσίας εφαρμογής στο ίδιο δίκτυο εικονικού και πραγματοποίηση εξερχόμενες κλήσεις από μία εφαρμογή υπηρεσίας περιβάλλον άλλη εφαρμογή υπηρεσίας περιβάλλον.  Αυτοί οι τύποι cross εφαρμογής υπηρεσίας περιβάλλον κλήσεις θα θεωρούνται επίσης ως κλήσεις "Internet".

Το παρακάτω διάγραμμα παρουσιάζει ένα παράδειγμα μιας σε επίπεδα αρχιτεκτονικής με εφαρμογές σε ένα περιβάλλον υπηρεσίας εφαρμογής (π.χ. "Εμπρός πόρτας" web apps) κλήση εφαρμογές σε μια δεύτερη εφαρμογή υπηρεσίας περιβάλλον (π.χ. εσωτερικού παρασκηνίου API εφαρμογές δεν προορίζεται για να είναι δυνατή η πρόσβαση από το Internet). 

![Κλήσεις μεταξύ της εφαρμογής υπηρεσίας περιβάλλοντα][CallsBetweenAppServiceEnvironments] 

Στο παραπάνω παράδειγμα το περιβάλλον υπηρεσίας εφαρμογής "ASE ένα" έχει εξερχομένων τη διεύθυνση IP του 192.23.1.2.  Εάν μια εφαρμογή που εκτελείται σε αυτό εφαρμογής υπηρεσίας περιβάλλον κάνει μία εξερχόμενη κλήση σε μια εφαρμογή που εκτελούνται στο δεύτερο εφαρμογής υπηρεσίας περιβάλλον ("ASE δύο") που βρίσκεται στο ίδιο δίκτυο εικονικού, την εξερχόμενη κλήση θα αντιμετωπίζεται ως μια κλήση "Internet".  Έχει ως αποτέλεσμα την κίνηση δικτύου φτάνει στο δεύτερο εφαρμογής υπηρεσίας περιβάλλον θα εμφανίζονται ως που προέρχονται από 192.23.1.2 (δηλαδή δεν υποδικτύου διευθύνσεων περιοχή του πρώτου περιβάλλοντος εφαρμογής υπηρεσίας).

Παρόλο που τα κλήσεις μεταξύ διαφορετικά περιβάλλοντα υπηρεσίας εφαρμογής θεωρούνται κλήσεις "Internet", όταν δύο περιβάλλοντα εφαρμογής υπηρεσίας βρίσκονται στην ίδια περιοχή Azure την κίνηση δικτύου θα παραμείνει στο τοπικές Azure δίκτυο και δεν θα φυσικά ροής μέσω του δημόσιου Internet.  Έχει ως αποτέλεσμα μπορείτε να χρησιμοποιήσετε μια ομάδα ασφαλείας δικτύου στο υποδίκτυο του δεύτερου περιβάλλοντος εφαρμογής υπηρεσίας για να επιτρέπονται μόνο εισερχόμενες κλήσεις από την πρώτη εφαρμογή υπηρεσίας περιβάλλον (του οποίου η διεύθυνση IP εξερχομένων είναι 192.23.1.2), διασφαλίζοντας την ασφαλή επικοινωνία μεταξύ των περιβαλλόντων εφαρμογής υπηρεσίας.

## <a name="additional-links-and-information"></a>Πρόσθετες συνδέσεις και πληροφορίες ##
Όλα τα άρθρα και με ποιον τρόπο-για να του για εφαρμογή υπηρεσίας περιβάλλοντα είναι διαθέσιμες στο το [αρχείο README για περιβάλλοντα εφαρμογών υπηρεσίας](../app-service/app-service-app-service-environments-readme.md).

Λεπτομέρειες σχετικά με εισερχομένων θύρες που χρησιμοποιούνται από την εφαρμογή υπηρεσίας περιβάλλοντα και χρησιμοποιώντας τις ομάδες ασφαλείας δικτύου για να ελέγξετε την εισερχόμενη κυκλοφορία είναι διαθέσιμη [εδώ][controllinginboundtraffic].

Λεπτομέρειες σχετικά με τη χρήση του χρήστη που ορίζονται από το δρομολογεί για να εκχωρήσετε εξερχομένων πρόσβαση στο Internet για περιβάλλοντα υπηρεσίας εφαρμογής είναι διαθέσιμη σε αυτό το [άρθρο][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png


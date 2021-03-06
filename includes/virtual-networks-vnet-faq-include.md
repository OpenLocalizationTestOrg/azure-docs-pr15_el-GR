## <a name="virtual-network-basics"></a>Βασικά στοιχεία εικονικού δικτύου

### <a name="what-is-an-azure-virtual-network-vnet"></a>Τι είναι μια εικονική Azure δίκτυο (VNet);

Μπορείτε να χρησιμοποιήσετε VNets για την παροχή των και διαχείριση εικονικών ιδιωτικών δικτύων (VPN) στο Azure και, προαιρετικά, σύνδεση το VNets με άλλες VNets στο Azure ή με τον εσωτερικής την υποδομή των τμημάτων για να δημιουργήσετε λύσεις υβριδική ή σταυρό εσωτερικής εγκατάστασης. Κάθε VNet δημιουργείτε διαθέτει δικό του μπλοκ CIDR και μπορούν να συνδεθούν με άλλα δίκτυα VNets και εσωτερικής εγκατάστασης με την προϋπόθεση ότι τα μπλοκ CIDR έρχονται σε διένεξη. Έχετε επίσης τα στοιχεία ελέγχου των ρυθμίσεων διακομιστή DNS για VNets και αγοράς από το VNet σε δευτερεύοντα δίκτυα.

Χρησιμοποιήστε το VNets για να:

- Δημιουργήστε μια αποκλειστική ιδιωτικό μόνο στο cloud εικονικού δικτύου

    Μερικές φορές που δεν απαιτούν μια ρύθμιση παραμέτρων σταυρό εσωτερικής εγκατάστασης για τη λύση. Όταν δημιουργείτε μια VNet, τις υπηρεσίες και τα ΣΠΣ μέσα σε VNet σας μπορούν να επικοινωνούν απευθείας και με ασφάλεια μεταξύ τους στο cloud. Αυτό διατηρεί την κυκλοφορία ασφαλή εντός του VNet, αλλά εξακολουθείτε να σάς επιτρέπει να ρύθμιση παραμέτρων συνδέσεων τελικό σημείο για τα ΣΠΣ και τις υπηρεσίες που απαιτούν επικοινωνίας με το Internet ως μέρος της λύσης.

- Επέκταση με ασφάλεια κέντρο δεδομένων σας

    Με VNets, μπορείτε να δημιουργήσετε παραδοσιακή VPN (S2S)--τοποθεσίας για να κλιμακωθεί με ασφάλεια σας χωρητικότητα κέντρου δεδομένων. S2S VPN χρησιμοποιούν ασφαλείας IP για την παροχή ασφαλούς σύνδεσης μεταξύ του εταιρικού πύλης VPN και Azure.

- Ενεργοποίηση της υβριδικής cloud σενάρια

    VNets να έχετε την ευελιξία να υποστηρίζει μια περιοχή υβριδική cloud σενάρια. Μπορείτε να συνδεθείτε με ασφάλεια εφαρμογές σε οποιονδήποτε τύπο συστήματος εσωτερικής όπως τους κεντρικούς υπολογιστές και συστήματα Unix που βασίζεται στο cloud.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Πώς μπορώ να γνωρίζω εάν χρειάζομαι ένα εικονικό δίκτυο;

Επισκεφθείτε την [Επισκόπηση εικονικού δικτύου](../articles/virtual-network/virtual-networks-overview.md) για να δείτε έναν πίνακα αποφάσεων που θα σας βοηθήσουν να αποφασίσετε η καλύτερη επιλογή σχεδίασης δικτύου για εσάς.

### <a name="how-do-i-get-started"></a>Πώς μπορώ να ξεκινήσω;

Επισκεφθείτε [την τεκμηρίωση εικονικού δικτύου](https://azure.microsoft.com/documentation/services/virtual-network/) για να ξεκινήσετε. Αυτή η σελίδα έχει συνδέσεις σε κοινές τις βήματα ρύθμισης παραμέτρων, καθώς και τις πληροφορίες που θα σας βοηθήσουν να κατανοήσετε τις ενέργειες που θα πρέπει να λάβετε υπόψη κατά τη σχεδίαση του εικονικού δικτύου.

### <a name="what-services-can-i-use-with-vnets"></a>Σε ποιες υπηρεσίες μπορώ να χρησιμοποιήσω με το VNets;

VNets μπορεί να χρησιμοποιηθεί με μια ποικιλία διαφορετικές υπηρεσίες του Azure, όπως τις υπηρεσίες Cloud (PaaS), εικονικές μηχανές και εφαρμογές Web. Ωστόσο, υπάρχουν μερικά υπηρεσίες που δεν υποστηρίζονται σε ένα VNet. Ελέγξτε τη συγκεκριμένη υπηρεσία που θέλετε να χρησιμοποιήσετε και βεβαιωθείτε ότι είναι συμβατό.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Μπορώ να χρησιμοποιήσω VNets χωρίς συνδεσιμότητας σταυρό εσωτερικής εγκατάστασης;

Ναι. Μπορείτε να χρησιμοποιήσετε ένα VNet χωρίς τη χρήση συνδεσιμότητας-τοποθεσίας. Αυτό είναι ιδιαίτερα χρήσιμο εάν θέλετε να εκτελέσετε ελεγκτές τομέα και συστοιχίες SharePoint στο Azure.

## <a name="virtual-network-configuration"></a>Ρύθμιση παραμέτρων εικονικού δικτύου

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Ποια εργαλεία μπορώ να χρησιμοποιήσω για να δημιουργήσετε ένα VNet;

Μπορείτε να χρησιμοποιήσετε τα παρακάτω εργαλεία για να δημιουργήσετε ή να ρυθμίσετε τις παραμέτρους ενός εικονικού δικτύου:

- Azure πύλη (για κλασική και διαχείριση πόρων VNets).

- Ένα αρχείο ρύθμισης παραμέτρων δικτύου (netcfg - για κλασική VNets μόνο). Ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός εικονικού δικτύου, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων δικτύου](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (για κλασική και διαχείριση πόρων VNets).

- Azure CLI (για κλασική και διαχείριση πόρων VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Ποιες περιοχές διευθύνσεων μπορώ να χρησιμοποιήσω στο VNets μου;

Μπορείτε να χρησιμοποιήσετε δημόσια περιοχές διευθύνσεων IP και οποιαδήποτε διεύθυνση IP που ορίζονται στο [RFC 1918](http://tools.ietf.org/html/rfc1918).

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Μπορώ να έχω δημόσιες διευθύνσεις IP στο VNets μου;

Ναι. Για περισσότερες πληροφορίες σχετικά με τη δημόσια περιοχές διευθύνσεων IP, ανατρέξτε στο θέμα [χώρου διευθύνσεων IP δημόσια σε ένα εικονικό δίκτυο (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Λάβετε υπόψη ότι σας δημόσια διευθύνσεις IP δεν θα είναι άμεσα προσβάσιμη από το Internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Υπάρχει όριο στον αριθμό των δευτερευόντων δικτύων στο εικονικού δίκτυό μου;

Υπάρχει όριο στον αριθμό των δευτερευόντων δικτύων χρησιμοποιείτε μέσα σε ένα VNet. Όλα τα δευτερεύοντα δίκτυα πρέπει να περιέχεται πλήρως στο χώρο διευθύνσεων εικονικού δικτύου και δεν θα πρέπει να επικαλύπτονται μεταξύ.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Υπάρχουν περιορισμοί σχετικά με τη χρήση διευθύνσεων IP μέσα σε αυτά τα δευτερεύοντα δίκτυα;

Azure διατηρεί ορισμένες διευθύνσεις IP μέσα σε κάθε υποδίκτυο. Το πρώτο και το τελευταίο διευθύνσεις IP του των δευτερευόντων δικτύων είναι δεσμευμένα για συμβατότητα με πρωτόκολλο, μαζί με 3 περισσότερες διευθύνσεις που χρησιμοποιούνται για τις υπηρεσίες του Azure.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Τρόπος μικρές και μεγάλες πώς μπορείτε να VNets και δευτερευόντων δικτύων;

Η μικρότερη υποστηρίζουμε είναι μια /29 και τη μεγαλύτερη τιμή είναι μια /8 (με χρήση των ορισμών υποδικτύου CIDR).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Μπορώ να μεταφέρω VLAN μου να Azure χρησιμοποιώντας VNets;

Όχι. VNets είναι επικαλύψεων Layer 3. Azure δεν υποστηρίζει οποιοδήποτε σημασιολογία επιπέδου 2.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Μπορώ να καθορίσω προσαρμοσμένες πολιτικές δρομολόγησης στην VNets και δευτερεύοντα δίκτυα μου;

Ναι. Μπορείτε να χρησιμοποιήσετε χρήστη που καθορίζονται από τη δρομολόγηση (UDR). Για περισσότερες πληροφορίες σχετικά με την UDR, επισκεφθείτε [δρομολογεί που ορίζονται από το χρήστη και την προώθηση IP](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNets υποστήριξη πολλαπλής ή ευρείας διανομής;

Όχι. Δεν υποστηρίζουμε πολλαπλής ή ευρείας διανομής.

### <a name="what-protocols-can-i-use-within-vnets"></a>Ποια πρωτόκολλα μπορώ να χρησιμοποιήσω μέσα σε VNets;

Μπορείτε να χρησιμοποιήσετε τις τυπικές τα πρωτόκολλα που βασίζεται σε IP μέσα σε VNets. Ωστόσο, πολλαπλής διανομής, εκπομπής, IP-σε-IP συμπυκνωμένα πακέτα και πακέτα γενικής χρήσης συμπύκνωση δρομολόγησης (GRE) αποκλείονται μέσα σε VNets. Τυπικά πρωτόκολλα που λειτουργούν περιλαμβάνουν τα εξής:

- TCP
- UDP
- ΠΑΚΈΤΑ ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Μπορώ να ping μου δρομολογητές προεπιλεγμένη μέσα σε μια VNet;

Όχι.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Μπορώ να χρησιμοποιήσω tracert για τη διάγνωση συνδεσιμότητας;

Όχι.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Μπορώ να προσθέσω δευτερεύοντα δίκτυα αφού δημιουργηθεί το VNet;

Ναι. Δευτερεύοντα δίκτυα μπορούν να προστεθούν σε VNets ανά πάσα στιγμή, με την προϋπόθεση ότι τη διεύθυνση δεν αποτελεί μέρος του άλλου υποδικτύου στο το VNet.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Μπορώ να αλλάξω το μέγεθος του μου υποδικτύου μετά τη δημιουργία του;

Μπορείτε να προσθέσετε, να καταργήσετε, να αναπτύξετε ή να σμικρύνετε ένα υποδίκτυο, εάν δεν υπάρχουν ΣΠΣ ή υπηρεσίες αναπτυχθεί μέσα σε αυτό με τη χρήση των cmdlet του PowerShell ή το αρχείο NETCFG. Επίσης, μπορείτε να προσθέσετε, να καταργήσετε, να ανάπτυξη ή να σμικρύνετε το πρόθεμα με την προϋπόθεση ότι το δευτερεύοντα δίκτυα που περιέχουν ΣΠΣ ή υπηρεσίες δεν επηρεάζονται από την αλλαγή.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Μπορούν να τροποποιήσουν δευτερεύοντα δίκτυα μετά να δημιουργίας τους;

Ναι. Μπορείτε να προσθέσετε, να καταργήσετε και να τροποποιήσετε τα μπλοκ CIDR χρησιμοποιείται από ένα VNet.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Μπορώ να συνδεθώ στο Internet αν χρησιμοποιώ υπηρεσιών μου σε μια VNet;

Ναι. Όλες οι υπηρεσίες αναπτυχθεί μέσα σε μια VNet να συνδεθείτε στο Internet. Κάθε υπηρεσία cloud αναπτυχθεί στο Azure περιλαμβάνει μια δημόσια μπορούν να χρησιμοποιηθούν VIP που έχει αντιστοιχιστεί σε αυτήν. Θα πρέπει να ορίσετε εισόδου τελικά σημεία για PaaS ρόλους και τα τελικά σημεία για εικονικές μηχανές για να ενεργοποιήσετε αυτές τις υπηρεσίες ώστε να δέχεται συνδέσεις από το internet.

### <a name="do-vnets-support-ipv6"></a>VNets υποστηρίζει IPv6;

Όχι. Δεν μπορείτε να χρησιμοποιήσετε το IPv6 με VNets αυτήν τη στιγμή.

### <a name="can-a-vnet-span-regions"></a>Μπορεί να μια VNet εκτείνεται σε περιοχές;

Όχι. Μια VNet περιορίζεται σε μία μόνο περιοχή.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Μπορώ να συνδεθώ VNet μια άλλη VNet στο Azure;

Ναι. Μπορείτε να δημιουργήσετε VNet σε VNet επικοινωνία με τη χρήση REST API ή του Windows PowerShell. Μπορείτε επίσης να συνδέσετε VNets μέσω διεισδύουν VNet. Δείτε περισσότερες λεπτομέρειες σχετικά με διεισδύουν [εδώ.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Επίλυση ονόματος (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Τι είναι οι επιλογές μου DNS για VNets;

Χρησιμοποιήστε τον πίνακα απόφασης στη σελίδα [Επίλυση ονομάτων για ΣΠΣ και παρουσίες ρόλο](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) για να σας καθοδηγήσει σε όλες τις διαθέσιμες επιλογές DNS.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Μπορώ να καθορίσετε διακομιστές DNS για μια VNet;

Ναι. Μπορείτε να καθορίσετε διευθύνσεις IP του διακομιστή DNS στις ρυθμίσεις VNet. Αυτό θα εφαρμοστεί ως την προεπιλεγμένη διακομιστές DNS για όλες ΣΠΣ στο το VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Πόσους διακομιστές DNS να καθορίσετε;

Μπορείτε να καθορίσετε έως και 12 διακομιστές DNS.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Μπορώ να αλλάξω μου οι διακομιστές DNS μετά που έχετε δημιουργήσει του δικτύου;

Ναι. Μπορείτε να αλλάξετε τη λίστα διακομιστή DNS για τον VNet οποιαδήποτε στιγμή. Εάν αλλάξετε τη λίστα των διακομιστών DNS, θα πρέπει να κάνετε επανεκκίνηση κάθε ένα από τα ΣΠΣ σε σας VNet για να επιλέξετε τον νέο διακομιστή DNS προς τα επάνω.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Τι είναι το DNS που παρέχονται από Azure και με VNets λειτουργεί;

Δόθηκε Azure DNS είναι μια υπηρεσία DNS πολλών μισθωτών που παρέχεται από τη Microsoft. Azure καταχωρεί όλες τις ΣΠΣ και παρουσίες ρόλο σε αυτήν την υπηρεσία. Αυτή η υπηρεσία παρέχει επίλυση ονομάτων με όνομα κεντρικού υπολογιστή για ΣΠΣ και παρουσίες ρόλο που περιέχονται σε την ίδια υπηρεσία cloud και με FQDN για ΣΠΣ και παρουσίες ρόλος στο το ίδιο VNet.

> [AZURE.NOTE] Υπάρχει περιορισμός αυτήν τη στιγμή με τις υπηρεσίες του 100 cloud πρώτη στο εικονικό δίκτυο για την επίλυση ονομάτων μισθωτή σταυρό χρησιμοποιώντας DNS που δόθηκε Azure. Εάν χρησιμοποιείτε το δικό σας διακομιστή DNS, ο περιορισμός αυτός δεν ισχύει.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Μπορεί να παρακάμπτουν τις ρυθμίσεις DNS μου σε μια ανά Εικονική / εξυπηρέτησης βάση;

Ναι. Μπορείτε να ρυθμίσετε τους διακομιστές DNS με βάση υπηρεσίας ανά cloud για να παρακάμψετε τις προεπιλεγμένες ρυθμίσεις δικτύου. Ωστόσο, συνιστάται να χρησιμοποιήσετε όλο το δίκτυο DNS όσο είναι δυνατό.

### <a name="can-i-bring-my-own-dns-suffix"></a>Μπορώ να μεταφέρω το δικό μου επίθημα DNS;

Όχι. Δεν μπορείτε να καθορίσετε ένα προσαρμοσμένο επίθημα DNS για τον VNets.

## <a name="vnets-and-vms"></a>VNets και ΣΠΣ

### <a name="can-i-deploy-vms-to-a-vnet"></a>Να μπορούν να αναπτύξω ΣΠΣ σε μια VNet;

Ναι.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Να μπορούν να αναπτύξω ΣΠΣ Linux σε μια VNet;

Ναι. Μπορείτε να αναπτύξετε τις distro του Linux που υποστηρίζονται από το Azure.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Ποια είναι η διαφορά ανάμεσα σε μια δημόσια VIP και μια εσωτερική διεύθυνση IP;

- Μια εσωτερική διεύθυνση IP είναι μια διεύθυνση IP που έχει εκχωρηθεί σε κάθε Εικονική μέσα σε μια VNet από το DHCP. Δεν είναι δημόσια τοποθεσία. Εάν έχετε δημιουργήσει μια VNet, την εσωτερική διεύθυνση IP εκχωρείται από την περιοχή που καθορίσατε στο των ρυθμίσεων υποδικτύου VNet σας. Εάν δεν έχετε ένα VNet, θα εξακολουθείτε να αντιστοιχιστεί μια εσωτερική διεύθυνση IP. Την εσωτερική διεύθυνση IP θα παραμείνουν με την εικονική Μηχανή για τη διάρκεια ζωής του, εκτός αν που κατάργηση εκχώρησης Εικονική.

- Μια δημόσια VIP είναι η δημόσια διεύθυνση IP στην οποία έχει ανατεθεί σας cloud εξισορρόπησης υπηρεσία ή τη φόρτωση. Δεν έχει αντιστοιχιστεί απευθείας σε σας Εικονική NIC. Το VIP παραμένει με την υπηρεσία cloud που έχει αντιστοιχιστεί σε μέχρι όλα τα ΣΠΣ σε που υπηρεσία cloud είναι κατάργηση εκχώρησης ή διαγραφεί. Σε αυτό το σημείο, την κυκλοφορία.

### <a name="what-ip-address-will-my-vm-receive"></a>Ποια διεύθυνση IP θα λάβουν Εικονική μου;

- **Διεύθυνση IP εσωτερικού-** Εάν αναπτύξετε μια Εικονική σε ένα VNet, η Εικονική λαμβάνει μια εσωτερική διεύθυνση IP από ένα σύνολο εσωτερικές διευθύνσεις IP που καθορίζετε. ΣΠΣ επικοινωνία εντός του VNets με τη χρήση εσωτερικές διευθύνσεις IP. Παρόλο που Azure εκχωρεί μια δυναμική εσωτερική διεύθυνση IP, μπορείτε να ζητήσετε μια στατική διεύθυνση για την Εικονική. Για να μάθετε περισσότερα σχετικά με τη στατική εσωτερικές διευθύνσεις IP, επισκεφθείτε [πώς μπορείτε να ορίσετε μια στατική εσωτερική IP](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Εικονική σας επίσης είναι συσχετισμένη με μια VIP, παρόλο που ένα VIP ποτέ εκχωρείται η Εικονική απευθείας. Μια VIP είναι μια δημόσια διεύθυνση IP που μπορούν να αντιστοιχιστούν στην υπηρεσία cloud σας. Προαιρετικά, μπορείτε να, δεσμεύσετε ένα VIP της υπηρεσίας cloud.

- **ILPIP-** Μπορείτε επίσης να ρυθμίσετε μια παρουσία επιπέδου δημόσια διεύθυνση IP (ILPIP). ILPIPs είναι απευθείας που σχετίζεται με την εικονική Μηχανή, αντί για την υπηρεσία cloud. Για να μάθετε περισσότερα σχετικά με το ILPIPs, επισκεφθείτε [Επιπέδου παρουσίας δημόσια Επισκόπηση διευθύνσεων IP](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Μπορώ να δεσμεύσετε μια εσωτερική διεύθυνση IP για μια Εικονική που που θα δημιουργήσετε αργότερα;

Όχι. Δεν μπορείτε να δεσμεύσετε μια εσωτερική διεύθυνση IP. Εάν είναι διαθέσιμη μια εσωτερική διεύθυνση IP θα αντιστοιχιστεί σε ένα ρόλο ή Εικονική παρουσία από το διακομιστή DHCP. Εικονική που μπορεί να ή μπορεί να μην είναι αυτό που θέλετε για την εσωτερική διεύθυνση IP θα εκχωρηθούν. Ωστόσο, μπορείτε να, αλλάξετε την εσωτερική διεύθυνση IP του ήδη δημιουργηθεί Εικονική μηχανή σε οποιαδήποτε διαθέσιμη εσωτερική διεύθυνση IP.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Κάνετε εσωτερική αλλαγή διευθύνσεις IP για ΣΠΣ σε μια VNet;

Ναι. Εσωτερικές διευθύνσεις IP παραμένουν με την εικονική Μηχανή για τη διάρκεια ζωής του, εκτός εάν η Εικονική κατάργηση εκχώρησης. Όταν μια Εικονική κατάργηση εκχώρησης, την εσωτερική διεύθυνση IP έχει κυκλοφορήσει εκτός και εάν έχετε ορίσει μια στατική διεύθυνση IP εσωτερικού για σας Εικονική. Εάν η Εικονική απλώς διακοπεί (και να τοποθετήσετε στον την κατάσταση **Διακοπή (Deallocated)**) θα παραμείνει που του έχουν ανατεθεί σε η Εικονική τη διεύθυνση IP.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Μπορώ να με μη αυτόματο τρόπο εκχωρήσω διευθύνσεις IP στο NIC σε ΣΠΣ;

Όχι. Δεν πρέπει να αλλάξετε τις ιδιότητες περιβάλλοντος εργασίας του ΣΠΣ. Οι αλλαγές ενδέχεται να προκαλέσουν ενδεχομένως απώλεια της σύνδεσης για να την εικονική Μηχανή.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Τι συμβαίνει με μου διευθύνσεις IP, εάν να τερματίσετε μια Εικονική;

Δεν υπάρχει τίποτα. Οι διευθύνσεις IP (δημόσια VIP και εσωτερική διεύθυνση IP) θα παραμείνει με την υπηρεσία cloud ή Εικονική.

> [AZURE.NOTE] Εάν θέλετε να απλώς να τερματίσετε την εικονική Μηχανή, μην χρησιμοποιείτε την πύλη διαχείρισης, για να το κάνετε. Προς το παρόν, το κουμπί τερματισμού θα καταργήσετε την εκχώρηση η εικονική μηχανή.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Μπορώ να μετακινήσω ΣΠΣ από μία υποδίκτυο στο άλλο υποδικτύου σε μια VNet χωρίς εκ νέου την ανάπτυξη;

Ναι. Μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Να να ρυθμίσετε τις παραμέτρους μια στατική διεύθυνση MAC για Εικονική μου;

Όχι. Μια διεύθυνση MAC δεν μπορεί να ρυθμιστεί στατικά.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Η διεύθυνση MAC παραμένει ίδια για μου Εικονική αφού έχει δημιουργηθεί;

Ναι, τη διεύθυνση MAC θα παραμείνουν τα ίδια για μια Εικονική Παρόλο που η Εικονική έχει διακοπεί (deallocated) και relaunched.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Μπορώ να συνδεθώ στο Internet από μια Εικονική στα μια VNet;

Ναι. Όλες οι υπηρεσίες αναπτυχθεί μέσα σε μια VNet να συνδεθείτε στο Internet. Επιπλέον, κάθε υπηρεσία cloud αναπτυχθεί στο Azure περιλαμβάνει μια δημόσια μπορούν να χρησιμοποιηθούν VIP που έχει αντιστοιχιστεί σε αυτήν. Πρέπει να ορίσετε εισαγωγής τελικά σημεία για PaaS ρόλους και τα τελικά σημεία για ΣΠΣ για να ενεργοποιήσετε αυτές τις υπηρεσίες ώστε να δέχεται συνδέσεις από το Internet.

## <a name="vnets-and-services"></a>VNets και υπηρεσίες

### <a name="what-services-can-i-use-with-vnets"></a>Σε ποιες υπηρεσίες μπορώ να χρησιμοποιήσω με το VNets;

Μπορείτε να χρησιμοποιήσετε μόνο τις υπηρεσίες υπολογισμού μέσα σε VNets. Υπηρεσίες υπολογισμού περιορίζονται σε υπηρεσίες Cloud (ρόλοι web και εργαζόμενου) και ΣΠΣ.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Μπορώ να χρησιμοποιήσω Web Apps με εικονικού δικτύου;

Ναι. Μπορείτε να αναπτύξετε εφαρμογές Web μέσα σε μια VNet χρησιμοποιώντας ASE (εφαρμογή υπηρεσίας περιβάλλον). Προσθήκη που, εφαρμογές Web μπορεί να ασφαλή σύνδεση και αποκτήστε πρόσβαση τους πόρους σας VNet Azure εάν έχετε ρυθμίσει τις παραμέτρους για το VNet σημείο σε τοποθεσία. Για περισσότερες πληροφορίες, ανατρέξτε στα παρακάτω:


- [Δημιουργία εφαρμογών Web σε ένα περιβάλλον εφαρμογής υπηρεσίας](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Ενοποίηση με το Web Apps εικονικού δικτύου](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Χρήση συνδέσεων υβριδική και VNet ενοποίηση με τα Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Ενοποίηση μια εφαρμογή web με ένα δίκτυο εικονικού Azure](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Μπορώ να αναπτύξω το τις υπηρεσίες cloud με το web και εργαζόμενου ρόλους (PaaS) σε μια VNet;

Ναι. Μπορείτε να αναπτύξετε τις υπηρεσίες PaaS μέσα σε VNets.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Πώς μπορώ να να αναπτύξω PaaS ρόλους για ένα VNet;

Μπορείτε να κάνετε αυτό, καθορίζοντας το όνομα VNet και οι αντιστοιχίσεις /subnet ρόλο στην ενότητα Ρύθμιση παραμέτρων δικτύου του στη ρύθμιση παραμέτρων υπηρεσίας. Δεν χρειάζεται να ενημερώσετε οποιαδήποτε από τις δυαδικά δεδομένα.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Μπορώ να μετακινήσω υπηρεσιών μου και έξοδος από VNets;

Όχι. Δεν μπορείτε να μετακινήσετε τις υπηρεσίες και έξοδος από VNets. Θα πρέπει να διαγράψετε και να αναπτύξετε ξανά την υπηρεσία για να το μετακινήσετε σε ένα άλλο VNet.

## <a name="vnets-and-security"></a>VNets και την ασφάλεια

### <a name="what-is-the-security-model-for-vnets"></a>Τι είναι το μοντέλο ασφαλείας για VNets;

VNets είναι πλήρως απομόνωσης από το ένα το άλλο και άλλες υπηρεσίες που φιλοξενούνται στην Azure υποδομής. Ένα VNet είναι ένα όριο αξιοπιστίας.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Μπορώ να Καθορισμός ACL ή NSGs σε VNets μου;

Όχι. Δεν μπορείτε να συσχετίσετε ACL ή NSGs να VNets. Ωστόσο, ACL μπορεί να οριστεί σε τελικά σημεία εισόδου για ΣΠΣ που έχουν αναπτυχθεί σε μια VNets και NSGs μπορεί να είναι συσχετισμένη με δευτερεύοντα δίκτυα ή NIC.

### <a name="is-there-a-vnet-security-whitepaper"></a>Υπάρχει μια λευκή βίβλο ασφαλείας VNet;

Ναι. Μπορείτε να κάνετε λήψη του [εδώ](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>APIs, σχήματα και εργαλεία

### <a name="can-i-manage-vnets-from-code"></a>Μπορώ να διαχειριστώ VNets από τον κώδικα;

Ναι. Μπορείτε να χρησιμοποιήσετε το ΥΠΌΛΟΙΠΟ APIs για τη Διαχείριση συνδεσιμότητας VNets και διασταυρούμενο εσωτερικής εγκατάστασης. Μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Υπάρχει υποστήριξη tooling για VNets;

Ναι. Μπορείτε να χρησιμοποιήσετε εργαλεία PowerShell και τη γραμμή εντολών για μια ποικιλία πλατφόρμες. Μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ](http://go.microsoft.com/fwlink/?LinkId=317721).

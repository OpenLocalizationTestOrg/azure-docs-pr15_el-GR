<properties
   pageTitle="Εφαρμογή μια υβριδική αρχιτεκτονική δικτύου με Azure και εσωτερικής VPN | Microsoft Azure"
   description="Πώς μπορείτε να υλοποιήσετε μια αρχιτεκτονική ασφαλούς τοποθεσίας σε τοποθεσία δικτύου που εκτείνεται σε μια Azure εικονικού δικτύου και ένα δίκτυο εσωτερικής εγκατάστασης συνδεδεμένοι, χρησιμοποιώντας ένα VPN."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Εφαρμογή ενός υβριδικού αρχιτεκτονική δικτύου με Azure και VPN εσωτερικής εγκατάστασης

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Σε αυτό το άρθρο περιγράφει ένα σύνολο πρακτικές για την επέκταση ένα δίκτυο εσωτερικής εγκατάστασης σε Azure χρησιμοποιώντας μια τοποθεσία σε τοποθεσία εικονικού ιδιωτικού δικτύου (VPN). Η κίνηση ρέει ανάμεσα στις ενός δικτύου εικονικού Azure (VNet) μέσω μιας διοχέτευσης ασφαλείας IP VPN και το δίκτυο εσωτερικής εγκατάστασης. Αυτή η αρχιτεκτονική είναι κατάλληλη για εφαρμογές υβριδική με τα εξής χαρακτηριστικά:

- Τμήματα της εφαρμογής εκτελέσετε εσωτερικής εγκατάστασης, ενώ άλλοι εκτέλεση στο Azure.

- Η κίνηση μεταξύ της εσωτερικής υλικού και στο cloud είναι πιθανό να είναι ανοιχτό ή είναι προτιμότερο να συναλλαγές ελαφρώς εκτεταμένο λανθάνων χρόνος για την ευελιξία και ισχύος επεξεργασίας του στο cloud.

- Το εκτεταμένο δίκτυο αποτελεί ένα κλειστό σύστημα. Δεν υπάρχει καμία απευθείας διαδρομή από το Internet για το VNet Azure.

- Οι χρήστες συνδέονται με το δίκτυο εσωτερικής εγκατάστασης για να χρησιμοποιήσετε τις υπηρεσίες που φιλοξενούνται στο Azure. Η γέφυρα μεταξύ το δίκτυο εσωτερικής εγκατάστασης και τις υπηρεσίες που εκτελούνται στο Azure είναι διαφανή στους χρήστες.

Σενάρια που χωρά αυτό το προφίλ παραδείγματα:

- Γραμμή επιχειρηματικές εφαρμογές που χρησιμοποιούνται μέσα σε μια εταιρεία, όπου έχει γίνει μετεγκατάσταση ενός τμήματος της λειτουργικότητας στο cloud.

- Ανάπτυξη/δοκιμής εργαστήριο φόρτους εργασίας.

> [AZURE.NOTE] Azure έχει δύο διαφορετικές ανάπτυξης μοντέλα: [Διαχείριση πόρων Azure] [ resource-manager-overview] και κλασική. Σε αυτό το σχέδιο χρησιμοποιεί διαχείριση πόρων, η οποία η Microsoft συνιστά για νέες αναπτύξεις.

## <a name="architecture-diagram"></a>Διάγραμμα αρχιτεκτονικής

Το παρακάτω διάγραμμα επισημαίνει τα στοιχεία σε αυτήν την αρχιτεκτονική:

> Ένα έγγραφο του Visio που περιλαμβάνει αυτό το διάγραμμα αρχιτεκτονική είναι διαθέσιμο για λήψη στο [Κέντρο λήψης αρχείων της Microsoft]το[visio-download]. Σε αυτό το διάγραμμα είναι στο "υβριδική δίκτυο - VPN" σελίδα.

![[0]][0]

- **Δίκτυο εσωτερικής εγκατάστασης.** Αυτό είναι ένα δίκτυο υπολογιστές και συσκευές, συνδεδεμένοι μέσω ενός ιδιωτικού δικτύου τοπικό εκτελούνται μέσα σε μια εταιρεία.

- **Συσκευή VPN.** Αυτή είναι μια συσκευή ή μια υπηρεσία που παρέχει εξωτερική σύνδεση με το δίκτυο εσωτερικής εγκατάστασης. Η συσκευή VPN μπορεί να είναι μια συσκευή υλικού ή μπορεί να είναι μια λύση λογισμικού όπως το δρομολόγησης και απομακρυσμένης πρόσβασης υπηρεσίας (RRAS) σε Windows Server 2012.

> [AZURE.NOTE] Για μια λίστα με τις υποστηριζόμενες συσκευές VPN και πληροφορίες σχετικά με τη ρύθμιση παραμέτρων των επιλεγμένων VPN συσκευές για τη σύνδεση με μια πύλη VPN Azure, ανατρέξτε στις οδηγίες για την κατάλληλη συσκευή από τη [λίστα των συσκευών VPN που υποστηρίζονται από το Azure][vpn-appliance].

- **Ν σειρών cloud της εφαρμογής.** Αυτή είναι η εφαρμογή που φιλοξενούνται στο Azure. Αυτό μπορεί να περιλαμβάνει πολλά επίπεδα, με πολλά δευτερεύοντα δίκτυα συνδεδεμένοι μέσω balancers Azure φόρτωση. Η κίνηση σε κάθε υποδίκτυο ενδέχεται να υπόκεινται σε κανόνες που έχουν οριστεί με τη χρήση [Ομάδων ασφαλείας δικτύου Azure (NSGs)][azure-network-security-group]. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Microsoft Azure ασφαλείας][getting-started-with-azure-security].

> [AZURE.NOTE] Σε αυτό το άρθρο περιγράφει την εφαρμογή cloud ως μία οντότητα. Ανατρέξτε στο θέμα [εκτελείται μια αρχιτεκτονική N-επιπέδων σε Azure] [ implementing-a-multi-tier-architecture-on-Azure] για λεπτομερείς πληροφορίες.

- **[Εικονικό δίκτυο (VNet)][azure-virtual-network].** Η εφαρμογή cloud και τα στοιχεία για την πύλη VPN Azure βρίσκονται στο ίδιο το VNet.

- **[Πύλη Azure VPN][azure-vpn-gateway].** Η υπηρεσία πύλης VPN σάς επιτρέπει να σύνδεση το VNet μέσω μιας συσκευής VPN στο δίκτυο εσωτερικής εγκατάστασης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [σύνδεση ένα δίκτυο εσωτερικής εγκατάστασης σε δίκτυο εικονικού Microsoft Azure][connect-to-an-Azure-vnet]. Η πύλη VPN αποτελείται από τα εξής στοιχεία:

  - **Πύλη εικονικού δικτύου.** Πρόκειται για έναν πόρο που παρέχει μια εικονική συσκευή VPN για το VNet. Είναι υπεύθυνος για την κίνηση δρομολόγησης από το δίκτυο εσωτερικής εγκατάστασης για το VNet.

  - **Πύλη τοπικού δικτύου.** Αυτή είναι μια αφαίρεση της συσκευής VPN εσωτερικής εγκατάστασης. Κίνηση του δικτύου από την εφαρμογή cloud με το δίκτυο εσωτερικής εγκατάστασης δρομολογείται μέσω της πύλης σε αυτό.

  - **Σύνδεση.** Η σύνδεση έχει ιδιότητες που καθορίζουν τον τύπο σύνδεσης (ασφαλείας IP) και τον αριθμό-κλειδί από κοινού με τη συσκευή VPN εσωτερικής εγκατάστασης για να κρυπτογραφήσετε την κυκλοφορία.

  - **Η πύλη υποδικτύου.** Η πύλη εικονικού δικτύου παραμένει στο δικό του υποδίκτυο, η οποία είναι υπόκεινται σε διάφορες απαιτήσεις, όπως περιγράφεται στην παρακάτω ενότητα συστάσεις.

- **Εσωτερική εξισορρόπηση φόρτου.** Κίνηση του δικτύου από την πύλη VPN δρομολογείται στην εφαρμογή cloud μέσω μιας εσωτερικής εξισορρόπηση φόρτου. Η μονάδα εξισορρόπησης φόρτου βρίσκεται στο δευτερεύον προσκηνίου της εφαρμογής.

## <a name="recommendations"></a>Συστάσεις

Azure προσφέρει πολλές διαφορετικές πόρους και τύπους πόρων, ώστε να μπορεί να είναι αυτή η αναφορά αρχιτεκτονική παρασχεθεί πολλούς διαφορετικούς τρόπους. Παρέχουμε ένα πρότυπο από διαχειριστή πόρων Azure για να εγκαταστήσετε την αρχιτεκτονική αναφοράς που ακολουθεί αυτές τις συστάσεις. Εάν επιλέξετε να δημιουργήσετε το δικό σας αρχιτεκτονική αναφορά θα πρέπει να ακολουθήσετε αυτές τις συστάσεις, εκτός αν έχετε συγκεκριμένες απαιτήσεις που σύσταση υπερβαίνει το μέγεθος.

### <a name="vnet-and-gateway-subnet"></a>Υποδικτύου VNet και πύλης

Δημιουργήστε μια VNet Azure για τη διατήρηση των στοιχείων στο cloud. Το χώρο διευθύνσεων από το Azure VNet πρέπει να είναι αρκετά μεγάλος, ώστε να περιλαμβάνει τις διευθύνσεις που χρησιμοποιείται από το ΣΠΣ και στα δευτερεύοντα δίκτυα του το VNet. Βεβαιωθείτε ότι το χώρο διευθύνσεων VNet διαθέτει αρκετό χώρο για ανάπτυξη εάν επιπλέον ΣΠΣ είναι πιθανό να φανούν χρήσιμες στο μέλλον. Ο χώρος διευθύνσεων από το VNet δεν πρέπει να επικαλύπτονται με το δίκτυο εσωτερικής εγκατάστασης. Για παράδειγμα, το παραπάνω διάγραμμα χρησιμοποιεί τη διεύθυνση 10.20.0.0/16 χώρο για την VNet.

Δημιουργήστε ένα δευτερεύον δίκτυο με το όνομα _GatewaySubnet_, με μια περιοχή διευθύνσεων της /27. Αυτό το υποδίκτυο απαιτείται από την πύλη εικονικού δικτύου και εκχώρηση 32 διευθύνσεις σε αυτό το υποδίκτυο βοηθούν στην αποτροπή που εκτελούνται με πιθανές πύλης περιορισμοί μεγέθους στο μέλλον. Αποφύγετε την τοποθέτηση αυτό το υποδίκτυο στη μέση του χώρου διευθύνσεων. Μια καλή πρακτική είναι να ορίσετε το χώρο διευθύνσεων για το υποδίκτυο πύλης στο επάνω άκρο του χώρου διευθύνσεων VNet. Παράδειγμα που εμφανίζεται στο διάγραμμα χρησιμοποιεί 10.20.255.224/27.  Μια γρήγορη διαδικασία για να υπολογίσετε το CIDR είναι ως εξής:

1. Ορίστε τα bit μεταβλητής στο χώρο διευθύνσεων από το VNet σε 1, ώστε να τα bit που χρησιμοποιούνται από το υποδίκτυο πύλης, στη συνέχεια, ορίστε τα υπόλοιπα bit σε 0.

2. Μετατρέψτε τα bit που προκύπτει σε δεκαδικό και express το ως ένα χώρο διευθύνσεων με το σύνολο μήκος πρόθεμα το μέγεθος του υποδικτύου πύλης.

Για παράδειγμα, για ένα VNet με μια περιοχή διευθύνσεων IP του 10.20.0.0/16, εφαρμόζοντας βήμα #1 παραπάνω γίνεται 10.20.0b11111111.0b11100000.  Μετατροπή που σε δεκαδικό και εκφράζουν την, όπως ένα χώρο διευθύνσεων αποδόσεις 10.20.255.224/27

> [AZURE.WARNING] Μην αναπτύξετε άλλες εικονικές μηχανές ή παρουσίες ρόλος στο υποδίκτυο πύλης. Επίσης, μην εκχωρείτε μια NSG σε αυτό το υποδίκτυο, όπως θα προκαλέσει την πύλη για να διακόψετε τη λειτουργία.

### <a name="virtual-network-gateway"></a>Η πύλη εικονικού δικτύου

Εκχώρηση μια δημόσια διεύθυνση IP για την πύλη εικονικού δικτύου.

Δημιουργία της πύλης εικονικού δικτύου στο δευτερεύον πύλης και να την εκχωρήσετε που μόλις εκχωρημένων δημόσια διεύθυνση IP. Χρησιμοποιήστε τον τύπο πύλη που ταιριάζει περισσότερο με τις απαιτήσεις σας και την οποία είναι ενεργοποιημένη από σας συσκευής VPN:

Δημιουργία μιας [πύλης που βασίζεται σε πολιτική] [ policy-based-routing] εάν θέλετε να ελέγχετε στενά πώς δρομολογούνται τα αιτήματα. με βάση κριτήρια πολιτικής όπως προθέματα διεύθυνση. Βασίζεται σε πολιτική πύλες Χρησιμοποιήστε στατική δρομολόγηση και λειτουργούν μόνο με συνδέσεις τοποθεσίας σε τοποθεσία.

Δημιουργία [πύλης βάσει δρομολόγηση] [ route-based-routing] εάν μπορείτε να συνδεθείτε με το δίκτυο εσωτερικής εγκατάστασης με χρήση RRAS, υποστηρίζει πολλές τοποθεσίες ή η περιοχή σταυρό συνδέσεις ή υλοποίηση συνδέσεις VNet-VNet (συμπεριλαμβανομένων των διαδρομών που διέρχονται από πολλές VNets). Δρομολόγηση βάσει πύλες χρησιμοποιούν δυναμική δρομολόγηση στην απευθείας κυκλοφορία μεταξύ δίκτυα. Αυτές μπορεί να να αποδεχτεί τις αποτυχίες στη διαδρομή δικτύου καλύτερη από στατικές διαδρομές, επειδή μπορεί να προσπαθούν εναλλακτικές διαδρομές. Δρομολόγηση βάσει πύλες επίσης να μειώσετε το επιβάρυνσης διαχείρισης, επειδή δρομολογεί ίσως να μην χρειάζεται να ενημερωθούν με μη αυτόματο τρόπο, όταν αλλάζουν οι διευθύνσεις δικτύου.

Για μια λίστα με τις υποστηριζόμενες συσκευές VPN, ανατρέξτε στο θέμα [σχετικά με το VPN συσκευές για συνδέσεις τοποθεσίας σε τοποθεσία πύλης VPN][vpn-appliances].

> [AZURE.NOTE] Μετά τη δημιουργία της πύλης, δεν μπορείτε να αλλάξετε μεταξύ τύπων πύλης χωρίς τη διαγραφή και εκ νέου τη δημιουργία της πύλης.

Επιλέξτε την SKU πύλης VPN Azure που ταιριάζει περισσότερο με τις απαιτήσεις σας μεταγωγή. Azure πύλης VPN είναι διαθέσιμο σε τρεις SKU εμφανίζονται στον παρακάτω πίνακα. Κάθε SKU παρέχει διάφοροι μετάδοσης, [επιβάλλονται τέλη] [ azure-gateway-charges] με βάση το χρονικό διάστημα που παρέχεται στην πύλη και είναι διαθέσιμη.

| SKU | Μετάδοσης VPN | Σήραγγες Max ασφαλείας IP|
|-----|----------------|------------------|
| Βασικές | 100 Mbps | 10 |
| Τυπική | 100 Mbps | 10 |
| Υψηλές επιδόσεις | 200 Mbps | 30 |

> [AZURE.NOTE] Το βασικό SKU δεν είναι συμβατή με Azure ExpressRoute. Μπορείτε να [αλλάξετε την SKU] [ changing-SKUs] μετά τη δημιουργία της πύλης.

Δημιουργία κανόνων δρομολόγησης για το υποδίκτυο πύλης που απευθείας εισερχόμενη κυκλοφορία εφαρμογή από την πύλη για το εσωτερικό εξισορρόπηση φόρτου και όχι επιτρέποντάς αιτήσεις για τη μεταβίβαση απευθείας στο του ΣΠΣ που υλοποίηση της εφαρμογής.

### <a name="on-premises-network-connection"></a>Σύνδεση δικτύου εσωτερικής εγκατάστασης

Δημιουργία πύλης τοπικού δικτύου. Καθορίστε τη δημόσια διεύθυνση IP της συσκευής VPN εσωτερικής εγκατάστασης, καθώς και το χώρο διευθύνσεων δίκτυο εσωτερικής εγκατάστασης. Σημειώστε ότι η συσκευή VPN εσωτερικής εγκατάστασης πρέπει να έχετε μια δημόσια διεύθυνση IP που είναι δυνατή η πρόσβαση από την πύλη τοπικό δίκτυο στην πύλη VPN Azure. Η συσκευή VPN δεν μπορεί να βρίσκεται πίσω από μια συσκευή NAT.

Δημιουργήστε μια σύνδεση τοποθεσίας σε τοποθεσία για την πύλη εικονικού δικτύου και την πύλη του τοπικού δικτύου. Επιλέξτε τον τύπο σύνδεσης τοποθεσίας σε τοποθεσία (ασφαλείας IP) και καθορίστε το κοινόχρηστο κλειδί. Κρυπτογράφηση τοποθεσίας σε τοποθεσία με την πύλη VPN Azure βασίζεται σε πρωτόκολλο ασφαλείας IP, χρήση προ-κοινόχρηστα κλειδιά για έλεγχο ταυτότητας. Μπορείτε να καθορίσετε τον αριθμό-κλειδί κατά τη δημιουργία της πύλης VPN Azure. Πρέπει να ρυθμίζετε τις παραμέτρους της συσκευής VPN εκτέλεση εσωτερικής εγκατάστασης με το ίδιο κλειδί. Υπάρχουν άλλοι μηχανισμοί ελέγχου ταυτότητας δεν υποστηρίζονται αυτήν τη στιγμή.

Βεβαιωθείτε ότι το εσωτερικής υποδομή δρομολόγησης έχει ρυθμιστεί ώστε να προωθεί αιτήσεις προορίζονται για διευθύνσεις με τη VNet Azure στη συσκευή VPN.

Ανοίξτε τις θύρες που απαιτούνται από την εφαρμογή cloud στο δίκτυο εσωτερικής εγκατάστασης.

Ελέγξτε τη σύνδεση για να επιβεβαιώσετε ότι:

- Η συσκευή VPN εσωτερικής εγκατάστασης σωστά δρομολογεί την κίνηση στην εφαρμογή cloud μέσω της πύλης VPN Azure.

- Το VNet σωστά δρομολογεί την κίνηση ξανά με το δίκτυο εσωτερικής εγκατάστασης.

- Απαγορευμένη κίνηση και στις δύο κατευθύνσεις έχει αποκλειστεί σωστά.

## <a name="scalability-considerations"></a>Ζητήματα κλιμάκωση

Μπορείτε να επιτύχετε περιορισμένη κατακόρυφη κλιμάκωση μετακινώντας από τη βασική ή τυπική SKU πύλης VPN σε την υψηλή SKU VPN επιδόσεων.

Για VNets που αναμένετε μεγάλο όγκο κίνησης VPN, εξετάστε το ενδεχόμενο διανομή το διαφορετικό φόρτους εργασίας σε ξεχωριστή μικρότερο VNets και τη ρύθμιση των παραμέτρων μιας πύλης VPN για κάθε μία από αυτές.

Μπορείτε να δημιουργήσετε διαμερίσματα του VNet, είτε οριζόντια είτε κατακόρυφα. Για να δημιουργήσετε διαμερίσματα οριζόντια, μετακίνηση κάποιες περιπτώσεις Εικονική από κάθε επίπεδο σε δευτερεύοντα δίκτυα τη νέα VNet. Το αποτέλεσμα είναι ότι κάθε VNet έχει την ίδια δομή και λειτουργικότητα. Για να δημιουργήσετε διαμερίσματα κατακόρυφα, σχεδιάσετε εκ νέου κάθε επίπεδο να χωρίσετε τη λειτουργικότητα σε διαφορετικές περιοχές λογική (όπως χειρισμός παραγγελίες, Τιμολόγηση, Διαχείριση λογαριασμού πελάτη και ούτω καθεξής). Στη συνέχεια, μπορεί να τοποθετηθεί κάθε λειτουργική περιοχή στο δικό του VNet.

Αναπαραγωγή έναν ελεγκτή τομέα της υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης στο το VNet και εφαρμογή DNS στο το VNet, μπορεί να σας βοηθήσει να μειώσετε ορισμένα από την κυκλοφορία που σχετίζονται με την ασφάλεια και διαχείρισης ροή από εσωτερικής εγκατάστασης στο cloud.

## <a name="availability-considerations"></a>Ζητήματα διαθεσιμότητας

Εάν χρειάζεστε για να βεβαιωθείτε ότι το δίκτυο εσωτερικής εγκατάστασης παραμένει διαθέσιμη για την πύλη VPN Azure, υλοποιήσετε ένα σύμπλεγμα ανακατεύθυνσης για την πύλη VPN εσωτερικής εγκατάστασης.

Εάν η εταιρεία σας έχει πολλές τοποθεσίες στην εσωτερική εγκατάσταση, δημιουργία [τοποθεσίας πολλών συνδέσεων] [ vpn-gateway-multi-site] σε μία ή περισσότερες VNets Azure. Αυτή η προσέγγιση απαιτεί δυναμική δρομολόγηση (δρομολόγηση βάσει), επομένως, βεβαιωθείτε ότι η πύλη VPN εσωτερικής υποστηρίζει αυτήν τη δυνατότητα.

Ανατρέξτε στο θέμα [SLA πύλης VPN] [ sla-for-vpn-gateway] για τις λεπτομέρειες σχετικά με το SLA πύλης VPN.

## <a name="manageability-considerations"></a>Ζητήματα διαχειρισιμότητα

Εποπτεία διαγνωστικών πληροφοριών από συσκευές VPN εσωτερικής εγκατάστασης. Αυτή η διαδικασία εξαρτάται από τις δυνατότητες που παρέχονται από τη συσκευή VPN. Για παράδειγμα, εάν χρησιμοποιείτε την υπηρεσία δρομολόγησης και απομακρυσμένης πρόσβασης σε Windows Server 2012, που θα πρέπει να ενεργοποιήσετε την [Καταγραφή RRAS][rras-logging].

Χρησιμοποιήστε τα [Διαγνωστικά πύλης VPN Azure] [ gateway-diagnostic-logs] για να καταγράψετε πληροφορίες σχετικά με ζητήματα συνδεσιμότητας. Αυτά τα αρχεία καταγραφής μπορεί να χρησιμοποιηθεί για την παρακολούθηση πληροφοριών, όπως η προέλευση και τους προορισμούς σύνδεσης αιτήσεις, το πρωτόκολλο που χρησιμοποιήθηκε, και τον τρόπο πραγματοποιήθηκε η σύνδεση (ή γιατί απέτυχε η προσπάθεια).

Παρακολουθήστε τα αρχεία καταγραφής λειτουργικές της πύλης VPN Azure με τη χρήση των αρχείων καταγραφής ελέγχου που είναι διαθέσιμες στην πύλη του Azure. Ξεχωριστά αρχεία καταγραφής είναι διαθέσιμα για την πύλη του τοπικού δικτύου, η πύλη Azure δικτύου και τη σύνδεση. Αυτές οι πληροφορίες μπορούν να χρησιμοποιηθούν για να παρακολουθείτε τις αλλαγές που κάνατε στην πύλη και μπορεί να είναι χρήσιμο εάν μια πύλη προηγουμένως λειτουργικό σταματά να λειτουργεί για κάποιο λόγο.

![[2]][2]

Παρακολούθηση της συνδεσιμότητας, και να παρακολουθείτε συμβάντα αποτυχίας σύνδεσης. Μπορείτε να χρησιμοποιήσετε ένα πακέτο παρακολούθησης όπως [Nagios] [ nagios] για να καταγράψετε και να αναφέρετε τις πληροφορίες αυτές.

## <a name="security-considerations"></a>Ζητήματα ασφαλείας

Δημιουργήστε ένα διαφορετικό κοινόχρηστο κλειδί για κάθε πύλη VPN. Χρησιμοποιήστε ένα ισχυρό κοινόχρηστο κλειδί για να αποφύγετε επιθέσεις βίαιες ενέργειες ισχύ.

> [AZURE.NOTE] Προς το παρόν, δεν μπορείτε να χρησιμοποιήσετε θάλαμο κλειδί Azure στην πύλη Azure VPN ήδη κοινόχρηστα κλειδιά.

Βεβαιωθείτε ότι η συσκευή VPN εσωτερικής εγκατάστασης χρησιμοποιεί μια μέθοδος κρυπτογράφησης που είναι [συμβατή με την πύλη VPN Azure][vpn-appliance-ipsec]. Για βασίζεται σε πολιτική δρομολόγηση, της πύλης VPN Azure υποστηρίζει τους αλγόριθμους κρυπτογράφησης AES256, AES128 και 3DES. Δρομολόγηση βάσει πύλες υποστηρίζουν AES256 και 3DES.

Εάν σας συσκευής VPN εσωτερικής εγκατάστασης είναι περίμετρο δικτύου που έχει ένα τείχος προστασίας μεταξύ του δικτύου περίμετρο και του Internet, ίσως χρειαστεί να ρυθμίσετε τις παραμέτρους [κανόνες τείχους προστασίας επιπλέον] [ additional-firewall-rules] για να επιτρέπεται η σύνδεση VPN--τοποθεσίας.

Εάν την εφαρμογή στο το VNet στέλνει δεδομένα στο Internet, εξετάστε την [εφαρμογή εξαναγκασμένη διοχέτευση] [ forced-tunneling] για να δρομολογείτε όλη την κυκλοφορία Internet συνδεδεμένων μέσα από το δίκτυο εσωτερικής εγκατάστασης. Αυτή η προσέγγιση σάς επιτρέπει να υποβληθούν εξερχόμενες αιτήσεις που γίνονται από την εφαρμογή από την υποδομή εσωτερικής εγκατάστασης.

> [AZURE.NOTE] Εξαναγκασμένη διοχέτευση μπορεί να επηρεάσει τη σύνδεση με τις υπηρεσίες Azure (την υπηρεσία αποθήκευσης, για παράδειγμα) και τη Διαχείριση αδειών χρήσης των Windows.

## <a name="troubleshooting-considerations"></a>Αντιμετώπιση προβλημάτων σε θέματα

> [AZURE.NOTE] Επισκεφθείτε τη δρομολόγηση και απομακρυσμένη πρόσβαση ιστολογίου για γενικές πληροφορίες σχετικά με την [αντιμετώπιση κοινών σφαλμάτων που σχετίζονται με το VPN][troubleshooting-vpn-errors].

Εάν δεν μπορεί να αλλάξουν τη σύνδεση VPN κίνηση, ελέγξτε τα εξής:

### <a name="on-premises-vpn-appliance"></a>Συσκευή VPN εσωτερικής εγκατάστασης:

**Η συσκευή VPN εσωτερικής λειτουργεί σωστά;**

Οποιαδήποτε αρχεία που δημιουργούνται από τη συσκευή VPN για σφάλματα και αποτυχίες καταγραφής ελέγχου. Τη θέση του αυτές τις πληροφορίες διαφέρουν ανάλογα με τη συσκευή σας. Για παράδειγμα, εάν χρησιμοποιείτε RRAS σε Windows Server 2012, μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή PowerShell για να εμφανίσετε πληροφορίες συμβάντων σφάλματος για την υπηρεσία RRAS:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Η ιδιότητα _μηνύματος_ από κάθε καταχώρηση παρέχει μια περιγραφή του σφάλματος. Μερικά συνηθισμένα παραδείγματα είναι οι εξής:

- Αδυναμία σύνδεσης, είναι δυνατόν να οφείλεται σε μια εσφαλμένη διεύθυνση IP που έχουν καθοριστεί για την πύλη VPN Azure στις παραμέτρους RRAS VPN δικτύου περιβάλλοντος εργασίας:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Το πρόβλημα κοινόχρηστο κλειδί που καθορίζεται στη ρύθμιση παραμέτρων RRAS VPN δικτύου περιβάλλοντος εργασίας:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Μπορείτε επίσης να αποκτήσετε το αρχείο καταγραφής συμβάντων πληροφορίες σχετικά με τις προσπάθειες σύνδεσης μέσω της υπηρεσίας RRAS, χρησιμοποιώντας την ακόλουθη εντολή PowerShell:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Σε περίπτωση αποτυχίας σύνδεσης, αυτό το αρχείο καταγραφής θα περιέχει σφάλματα που μοιάζει με το εξής:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Είναι η κυκλοφορία σωστά δρομολόγησης συσκευής VPN μέσω της πύλης VPN Azure;**

Χρησιμοποιήστε ένα εργαλείο όπως [PsPing] [ psping] για να επιβεβαιώσετε τη σύνδεση και τη δρομολόγηση σε την πύλη VPN. Για παράδειγμα, για να ελέγξετε τη σύνδεση από έναν υπολογιστή εσωτερικής εγκατάστασης σε διακομιστή web που βρίσκεται στο το VNet, εκτελέστε την ακόλουθη εντολή (αντικαταστήστε `<<web-server-address>>` με τη διεύθυνση του διακομιστή web):

```
PsPing -t <<web-server-address>>:80
```

Εάν η μηχανή εσωτερικής εγκατάστασης να δρομολογήσετε την κίνηση στο διακομιστή web, θα πρέπει να βλέπετε εξόδου παρόμοια με τα εξής:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Εάν η μηχανή εσωτερικής εγκατάστασης δεν μπορεί να επικοινωνήσει με τον καθορισμένο προορισμό, θα δείτε μηνύματα ως εξής:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Το τείχος προστασίας στην εσωτερική εγκατάσταση επιτρέπει την κυκλοφορία VPN διέλευση; Έχετε ανοίξει τις σωστές θύρες;**

Επιβεβαίωση ότι η συσκευή VPN εσωτερικής εγκατάστασης χρησιμοποιεί μια μέθοδος κρυπτογράφησης που είναι [συμβατή με την πύλη VPN Azure][vpn-appliance].

Για βασίζεται σε πολιτική δρομολόγηση, της πύλης VPN Azure υποστηρίζει τους αλγόριθμους κρυπτογράφησης AES256, AES128 και 3DES. Δρομολόγηση βάσει πύλες υποστηρίζουν AES256 και 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet πύλη:

Εξετάστε τα [αρχεία καταγραφής διαγνωστικών πύλης VPN Azure] [ gateway-diagnostic-logs] για πιθανά ζητήματα.

**Είναι η πύλη VPN Azure και συσκευής VPN εσωτερικής εγκατάστασης έχει ρυθμιστεί με το ίδιο κλειδί ελέγχου ταυτότητας κοινόχρηστου;**

Μπορείτε να προβάλετε το κοινόχρηστο κλειδί που είναι αποθηκευμένα με την πύλη VPN Azure χρησιμοποιώντας την ακόλουθη εντολή Azure CLI:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Χρησιμοποιήστε την εντολή κατάλληλη για σας συσκευής VPN εσωτερικής εγκατάστασης για να εμφανίσετε το κοινόχρηστο κλειδί που έχει ρυθμιστεί για συγκεκριμένη συσκευή.

Επιβεβαιώστε ότι το υποδίκτυο _GatewaySubnet_ κρατώντας πατημένο το της πύλης VPN Azure δεν έχει συσχετιστεί με ένα NSG.

Μπορείτε να προβάλετε τις λεπτομέρειες υποδίκτυο, χρησιμοποιώντας την ακόλουθη εντολή Azure CLI:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Βεβαιωθείτε ότι δεν υπάρχει κανένα πεδίο δεδομένων ονομάζεται _αναγνωριστικό ομάδας ασφαλείας δικτύου_. Το παρακάτω παράδειγμα εμφανίζει τα αποτελέσματα για παράδειγμα το _GatewaySubnet_ που έχει μια εκχωρημένη NSG (_Ομάδα πυλών VPN_). Αυτό μπορεί να προκαλέσει αποτρέψετε πύλης από λειτουργεί σωστά εάν έχουν οριστεί για αυτό NSG οποιαδήποτε κανόνες:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Είναι τις εικονικές μηχανές του Azure VNet έχει ρυθμιστεί ώστε να επιτρέψετε την κυκλοφορία που προέρχονται από έξω από το VNet; Επιλέξτε οποιοδήποτε NSG κανόνες που σχετίζονται με δευτερεύοντα δίκτυα που περιέχει αυτές τις εικονικές μηχανές.**

Μπορείτε να προβάλετε όλους τους κανόνες NSG, χρησιμοποιώντας την ακόλουθη εντολή Azure CLI:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Έχει αποσυνδεθεί της πύλης VPN Azure;**

Μπορείτε να χρησιμοποιήσετε την παρακάτω εντολή Azure PowerShell για να ελέγξετε την τρέχουσα κατάσταση η σύνδεση Azure VPN. Το `<<connection-name>>` παραμέτρου είναι το όνομα του στη σύνδεση Azure VPN που συνδέει την πύλη εικονικού δικτύου και την τοπική πύλη.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Τα παρακάτω τμήματα κώδικα επισημάνετε το αποτέλεσμα που δημιουργούνται από το εάν η πύλη είναι συνδεδεμένη (το πρώτο παράδειγμα) και αποσυνδεθεί (το δεύτερο παράδειγμα):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Ρύθμιση παραμέτρων κεντρικού υπολογιστή Εικονική χρήσης εύρους ζώνης δικτύου και επιδόσεων εφαρμογής:

Βεβαιωθείτε ότι το τείχος προστασίας των επισκέπτη λειτουργικού συστήματος που εκτελείται στο του ΣΠΣ Azure στο δευτερεύον έχουν ρυθμιστεί σωστά για να επιτρέψετε την κυκλοφορία ρητά από τις περιοχές διευθύνσεων IP εσωτερικής εγκατάστασης.

**Είναι διαθέσιμη για την πύλη VPN Azure την ένταση ήχου της κυκλοφορίας κοντά το όριο του εύρους ζώνης;**

Tooling εξαρτάται από τις υπηρεσίες είναι διαθέσιμες σε σας συσκευή VPN εκτελείται εσωτερικής εγκατάστασης. Για παράδειγμα, εάν χρησιμοποιείτε RRAS σε Windows Server 2012, μπορείτε να χρησιμοποιήσετε Εποπτεία επιδόσεων για να παρακολουθείτε τον όγκο δεδομένων που λάβατε και μεταδίδονται μέσω της σύνδεσης VPN; χρησιμοποιώντας το _Συνολικό RAS_ αντικείμενο, επιλέξτε τους μετρητές _Byte λήψης/δευτερόλεπτο_ και _Byte μετάδοσης/δευτερόλεπτο_ :

![[3]][3]

Θα πρέπει να συγκρίνετε τα αποτελέσματα με το εύρος ζώνης είναι διαθέσιμο για την πύλη VPN (100 Mbps για το Basic και τυπική SKU και 200 Mbps για την υψηλή SKU επιδόσεων):

![[4]][4]

**Οποιαδήποτε από τις εικονικές μηχανές του Azure VNet εκτελούνται αργά; Είναι αυτά φορτωμένη υπερβολικά, είναι αρκετά από αυτά για το χειρισμό η φόρτωση, υπάρχουν όλα φόρτωσης balancers έχει ρυθμιστεί σωστά;**

Απαιτείται [καταγραφή και ανάλυση πληροφοριών διαγνωστικών][azure-vm-diagnostics]. Μπορείτε να εξετάσετε τα αποτελέσματα με την πύλη Azure, αλλά πολλά εργαλεία τρίτων κατασκευαστών είναι επίσης διαθέσιμες που μπορούν να παρέχουν λεπτομερείς ιδέες τα δεδομένα επιδόσεων.

**Είναι η εφαρμογή πραγματοποίηση αποτελεσματική χρήση πόρων cloud;**

Κωδικός εφαρμογή Instrument εκτελείται σε κάθε Εικονική για να προσδιορίσετε αν εφαρμογές κάνουν τη βέλτιστη χρήση των πόρων. Μπορείτε να χρησιμοποιήσετε εργαλεία όπως η [Εφαρμογή ιδέες] [ application-insights] για να εκτελέσετε αυτές τις εργασίες.

## <a name="solution-deployment"></a>Ανάπτυξη της λύσης

Εάν έχετε μια υπάρχουσα υποδομή εσωτερικής εγκατάστασης που έχει ήδη ρυθμιστεί με μια συσκευή VPN, μπορείτε να αναπτύξετε την αρχιτεκτονική αναφοράς, ακολουθώντας τα παρακάτω βήματα:

1. Κάντε δεξί κλικ στο κουμπί παρακάτω και επιλέξτε είτε "Άνοιγμα σύνδεσης σε νέα καρτέλα" ή "Άνοιγμα σύνδεσης σε νέο παράθυρο":  
[![Ανάπτυξη Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Αναμονή για τη σύνδεση για να ανοίξετε στην πύλη του Azure και, στη συνέχεια, ακολουθήστε τα παρακάτω βήματα: 
    - Το όνομα της **ομάδας πόρων** ορίζεται ήδη στο αρχείο παραμέτρων, επομένως, επιλέξτε **Δημιουργία νέου** και πληκτρολογήστε `ra-hybrid-vpn-rg` στο πλαίσιο κειμένου.
    - Επιλέξτε την περιοχή από τη **θέση** αναπτυσσόμενο πλαίσιο.
    - Επεξεργαστείτε το **Πρότυπο ρίζας Uri** ή πλαίσια κειμένου **Uri ριζικού παραμέτρων** .
    - Εξετάστε τους όρους και τις προϋποθέσεις και, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου **αποδέχομαι τους όρους και προϋποθέσεις που αναφέρονται παραπάνω** .
    - Κάντε κλικ στο κουμπί **αγοράς** .

3. Περιμένετε για την ανάπτυξη για να ολοκληρωθεί.

## <a name="next-steps"></a>Επόμενα βήματα

- [Κατά την εγκατάσταση ελεγκτές επιπλέον τομέα για έναν τομέα Active Directory εσωτερικής εγκατάστασης με χρήση ΣΠΣ σε μια VNet Azure][installing-ad].

- [Οδηγίες για την ανάπτυξη του Windows Server υπηρεσίας καταλόγου Active Directory στο ΣΠΣ Azure][deploying-ad].

- [Δημιουργία ενός διακομιστή DNS σε μια VNet][creating-dns].

- [Ρύθμιση παραμέτρων και τη Διαχείριση DNS σε μια VNet][configuring-dns].

- [Χρήση συσκευές ασφαλείας δικτύου Stormshield εσωτερικής εγκατάστασης σε ένα VPN][stormshield].

- [Εφαρμογή ενός υβριδικού αρχιτεκτονική δικτύου με Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Δομή του υβριδική δικτύου που εκτείνονται σε το εσωτερικής εγκατάστασης και cloud υποδομών"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Δημιουργία διαμερισμάτων μια VNet για να βελτιώσω την κλιμάκωση"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Αρχεία καταγραφής στην πύλη του Azure ελέγχου"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Μετρητές επιδόσεων για την παρακολούθηση κίνηση του δικτύου VPN"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Γράφημα επιδόσεων δικτύου VPN παράδειγμα"
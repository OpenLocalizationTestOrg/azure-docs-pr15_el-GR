<properties
   pageTitle="Ασφάλεια των δεδομένων και την κρυπτογράφηση βέλτιστες πρακτικές | Microsoft Azure"
   description="Σε αυτό το άρθρο παρέχει ένα σύνολο βέλτιστες πρακτικές για την ασφάλεια των δεδομένων και χρήση κρυπτογράφησης ενσωματωμένη Azure δυνατότητες."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yuridio"/>

#<a name="azure-data-security-and-encryption-best-practices"></a>Βέλτιστες πρακτικές ασφαλείας Azure δεδομένων και κρυπτογράφησης

Ένα από τα πλήκτρα για την προστασία των δεδομένων στο cloud accounting για τις πιθανές καταστάσεις στις οποίες μπορεί να προκύψουν τα δεδομένα σας και ποιοι έλεγχοι είναι διαθέσιμοι για το συγκεκριμένο μέλος. Για την Azure δεδομένων ασφαλείας κρυπτογράφησης βέλτιστες πρακτικές και τις συστάσεις θα γύρω από τα εξής δεδομένα Πολιτείες:

- Σε αδράνεια: Περιλαμβάνει όλες τις πληροφορίες χώρο αποθήκευσης αντικειμένων, κοντέινερ και τύπους που υπάρχουν στατικά φυσικού μέσου, να έλεγχος ή να το οπτικό δίσκο.

- Σε μεταφορά: Όταν μεταφέρονται τα δεδομένα μεταξύ των στοιχείων, θέσεις ή προγράμματα, όπως μέσω του δικτύου, σε μια υπηρεσία bus (από εσωτερικής εγκατάστασης στο cloud και το αντίστροφο, συμπεριλαμβανομένων των συνδέσεων υβριδική όπως ExpressRoute), ή κατά τη διάρκεια μιας διαδικασίας εισόδου/εξόδου, αυτό είναι θεωρηθεί ως που έχει τεθεί σε κίνηση.

Σε αυτό το άρθρο αναλύονται μια συλλογή από Azure δεδομένων ασφαλείας και κρυπτογράφησης βέλτιστες πρακτικές. Αυτές τις βέλτιστες πρακτικές προέρχονται από μας εμπειρία με ασφάλεια Azure δεδομένων και κρυπτογράφησης και τις εμπειρίες των πελατών, όπως τον εαυτό σας.

Για κάθε βέλτιστη πρακτική, θα σας θα εξηγούν:

- Τι είναι η βέλτιστη πρακτική
- Γιατί που θέλετε να ενεργοποιήσετε αυτήν τη βέλτιστη πρακτική
- Τι μπορεί να είναι το αποτέλεσμα, εάν δεν μπορέσετε να ενεργοποιήσετε τη βέλτιστη πρακτική
- Πιθανές εναλλακτικές λύσεις για τη βέλτιστη πρακτική
- Πώς μπορείτε να μάθετε για να ενεργοποιήσετε τη βέλτιστη πρακτική

Σε αυτό το άρθρο ασφάλεια δεδομένων Azure και βέλτιστες πρακτικές κρυπτογράφησης βασίζεται σε μια γνώμη συναίνεσης, και δυνατότητες Azure πλατφόρμα και σύνολα δυνατοτήτων, καθώς υπάρχουν την ώρα που έχει συνταχθεί σε αυτό το άρθρο. Απόψεις και τεχνολογιών αλλάζουν μέσα στο χρόνο και σε αυτό το άρθρο θα ενημερωθεί σε τακτική βάση ώστε να αντικατοπτρίζει αυτές τις αλλαγές.

Azure δεδομένων ασφαλείας και κρυπτογράφησης βέλτιστες πρακτικές αναφέρονται σε αυτό το άρθρο περιλαμβάνουν τα εξής:

- Έλεγχο ταυτότητας πολλών παραγόντων
- Έλεγχος πρόσβασης (RBAC) βάσει ρόλων χρήση
- Κρυπτογράφηση Azure εικονικές μηχανές
- Χρησιμοποιήστε μοντέλα ασφάλειας υλικού
- Διαχείριση με ασφαλή σταθμούς εργασίας
- Ενεργοποίηση κρυπτογράφηση δεδομένων SQL
- Προστατεύστε τα δεδομένα κατά τη μεταφορά
- Επιβολή κρυπτογράφησης δεδομένων σε επίπεδο αρχείο


## <a name="enforce-multi-factor-authentication"></a>Έλεγχο ταυτότητας πολλών παραγόντων

Το πρώτο βήμα για πρόσβαση σε δεδομένα και στοιχείο ελέγχου στο Microsoft Azure είναι για τον έλεγχο ταυτότητας χρήστη. [Azure έλεγχο ταυτότητας πολλών παραγόντων (MFA)](../multi-factor-authentication/multi-factor-authentication.md) είναι μια μέθοδο επαλήθευση ταυτότητας του χρήστη χρησιμοποιώντας κάποια άλλη μέθοδο από απλώς ένα όνομα χρήστη και τον κωδικό πρόσβασης. Σε αυτό τον έλεγχο ταυτότητας μέθοδο σάς βοηθά στη διαφύλαξη πρόσβαση σε δεδομένα και εφαρμογές κατά τη σύσκεψη ζήτηση χρήστη για μια απλή διαδικασία εισόδου.

Με την ενεργοποίηση Azure MFA για τους χρήστες σας, προσθέτετε ένα δεύτερο επίπεδο ασφάλειας πρόσθετα εισόδου χρήστη και οι συναλλαγές. Σε αυτήν την περίπτωση, μια συναλλαγή ενδέχεται να έχουν πρόσβαση σε ένα έγγραφο που βρίσκεται σε ένα διακομιστή αρχείων ή σε σας του SharePoint Online. Azure MFA βοηθά, επίσης, IT για να μειώσετε την πιθανότητα ότι ένα έχει παραβιαστεί διαπιστευτηρίων θα έχουν πρόσβαση στα δεδομένα της εταιρείας.

Για παράδειγμα: Εάν επιβολή Azure MFA για τους χρήστες και ρυθμίστε τις παραμέτρους του για να χρησιμοποιήσετε μια τηλεφωνική κλήση ή μήνυμα κειμένου ως επαλήθευσης, εάν έχει παραβιαστεί διαπιστευτηρίων του χρήστη, ο εισβολέας δεν θα μπορούν να έχουν πρόσβαση σε οποιονδήποτε πόρο δεδομένου ότι εσείς δεν θα έχουν πρόσβαση στο τηλέφωνό του χρήστη. Εταιρείες που δεν προσθέσετε αυτό το επιπλέον επίπεδο προστασίας ταυτότητας είναι πιο ευαίσθητα για διαπιστευτηρίων κλοπή επίθεση, η οποία μπορεί να οδηγήσει σε παραβίαση δεδομένων.

Μια εναλλακτική λύση για εταιρείες που θέλετε να διατηρήσετε το ελέγχου ταυτότητας ελέγχου εσωτερικής εγκατάστασης είναι να χρησιμοποιήσετε [Διακομιστή ελέγχου ταυτότητας πολλαπλών παραγόντων Azure](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), που ονομάζεται επίσης MFA εσωτερικής εγκατάστασης. Χρησιμοποιώντας αυτήν τη μέθοδο που θα εξακολουθούν να μπορέσετε να έλεγχο ταυτότητας πολλών παραγόντων, διατηρώντας το MFA διακομιστή εσωτερικής εγκατάστασης.

Για περισσότερες πληροφορίες σχετικά με Azure MFA, διαβάστε το άρθρο [Γρήγορα αποτελέσματα με έλεγχο ταυτότητας πολλών παραγόντων Azure στο cloud](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Έλεγχος πρόσβασης (RBAC) βάσει ρόλων χρήση
Περιορισμός πρόσβασης βάσει αρχών ασφαλείας που [πρέπει να γνωρίζετε](https://en.wikipedia.org/wiki/Need_to_know) και [ελάχιστων δικαιωμάτων](https://en.wikipedia.org/wiki/Principle_of_least_privilege) . Αυτό είναι απαραίτητο για εταιρείες που θέλουν να επιβάλετε πολιτικές ασφαλείας για πρόσβαση σε δεδομένα. Azure βάσει ρόλων πρόσβασης ελέγχου (RBAC) μπορεί να χρησιμοποιηθεί για την εκχώρηση δικαιωμάτων σε χρήστες, ομάδες και εφαρμογές σε ένα συγκεκριμένο εύρος. Το εύρος της μια εκχώρηση ρόλων μπορεί να είναι μια συνδρομή, μια ομάδα πόρων ή έναν πόρο.

Μπορείτε να αξιοποιήσετε [ενσωματωμένη RBAC ρόλων](../active-directory/role-based-access-built-in-roles.md) στο Azure για να εκχωρήσετε δικαιώματα για τους χρήστες. Εξετάστε το ενδεχόμενο χρήσης *Συμβολής λογαριασμού χώρου αποθήκευσης* για το cloud τους τελεστές που χρειάζεστε για να διαχειριστείτε τους λογαριασμούς χώρου αποθήκευσης και ρόλων *Κλασική συμβολής λογαριασμού χώρου αποθήκευσης* για τη Διαχείριση λογαριασμών κλασική χώρου αποθήκευσης. Για τελεστές cloud που πρέπει να διαχειριστείτε ΣΠΣ και το λογαριασμό χώρου αποθήκευσης, εξετάστε το ενδεχόμενο προσθέτοντάς τις *Συμβολής εικονική μηχανή* ρόλο.

Εταιρείες που επιβάλλει τον έλεγχο πρόσβασης δεδομένων από αξιοποίηση δυνατότητες, όπως RBAC ενδέχεται εκχωρεί δικαιώματα περισσότερες από αυτές που χρειάζονται για τους χρήστες. Αυτό μπορεί να οδηγήσει σε παραβίαση δεδομένων δίνοντας ορισμένοι χρήστες έχουν πρόσβαση σε δεδομένα τα οποία δεν πρέπει να έχει αρχικά.

Μπορείτε να μάθετε περισσότερα σχετικά με το Azure RBAC διαβάζοντας το άρθρο [Έλεγχος πρόσβασης Azure Role-Based](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Κρυπτογράφηση Azure εικονικές μηχανές
Για πολλές εταιρείες, [κρυπτογράφηση δεδομένων στο υπόλοιπο](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) είναι μια υποχρεωτική βήμα προς δεδομένα προστασίας προσωπικών δεδομένων, συμμόρφωση και ακεραιότητα δεδομένων. Azure δίσκου κρυπτογράφησης επιτρέπει στους διαχειριστές IT για την κρυπτογράφηση των Windows και Linux IaaS εικονική μηχανή (Εικονική) δίσκων. Azure κρυπτογράφηση δίσκου αξιοποιεί το κλάδο τυπική BitLocker η δυνατότητα των Windows και τη δυνατότητα DM κρυπτογραφημένου του Linux για την παροχή κρυπτογράφηση τόμου για το λειτουργικό σύστημα και των δίσκων δεδομένων.

Μπορείτε να αξιοποιήσετε Azure δίσκου κρυπτογράφησης για την προστασία και να προστατεύσετε τα δεδομένα σας, ώστε να ανταποκρίνεται σας ασφάλειας του οργανισμού και τις απαιτήσεις συμμόρφωσης. Οργανισμοί πρέπει επίσης να χρησιμοποιήσετε κρυπτογράφηση για τη μείωση κινδύνων δεδομένα σχετικά με τη μη εξουσιοδοτημένη πρόσβαση. Επίσης, συνιστάται να κρυπτογραφήσετε μονάδες πριν από την εγγραφή ευαίσθητα δεδομένα σε αυτά.

Βεβαιωθείτε ότι για την κρυπτογράφηση όγκους δεδομένων σας Εικονική και τόμου εκκίνησης προκειμένου να προστατέψετε τα δεδομένα στο υπόλοιπο στο λογαριασμό σας στο Azure χώρου αποθήκευσης. Προστασία των κλειδιών κρυπτογράφησης και απορρήτου από αξιοποίηση [Azure κλειδί θάλαμο](../key-vault/key-vault-whatis.md).

Για τους διακομιστές σας εσωτερικής εγκατάστασης των Windows, μπορείτε να την παρακάτω κρυπτογράφηση βέλτιστες πρακτικές:

- Χρησιμοποιήσετε το [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) για την κρυπτογράφηση δεδομένων
- Αποθήκευση πληροφοριών ανάκτησης στις υπηρεσίες τομέα AD DS.
- Εάν υπάρχει οποιοδήποτε πρόβλημα που κλειδιών BitLocker έχει παραβιαστεί, συνιστάται να μορφοποιείτε είτε η μονάδα δίσκου για να καταργήσετε όλες τις εμφανίσεις της τα μετα-δεδομένα BitLocker από τη μονάδα δίσκου ή που αποκρυπτογράφηση και να κρυπτογραφήσετε ξανά ολόκληρη τη μονάδα δίσκου.

Εταιρείες που επιβάλει κρυπτογράφηση δεδομένων είναι πιο πιθανό να είναι εκτεθειμένη προβλημάτων ακεραιότητας δεδομένων, όπως κακόβουλος ή επικίνδυνο χρήστες κλοπή δεδομένων και έχει παραβιαστεί λογαριασμούς μη εξουσιοδοτημένη πρόσβαση σε δεδομένα σε Απαλοιφή μορφοποίησης. Εκτός από αυτούς τους κινδύνους, εταιρείες που πρέπει να συμμορφώνονται με κανονισμούς κλάδο, πρέπει να αποδείξετε ότι είστε επιμελής και χρησιμοποιούν τα στοιχεία ελέγχου σωστή ασφάλεια για να βελτιώσετε την ασφάλεια των δεδομένων.

Μπορείτε να μάθετε περισσότερα σχετικά με την κρυπτογράφηση δίσκου Azure διαβάζοντας το άρθρο [Azure δίσκου κρυπτογράφησης για Windows και Linux IaaS ΣΠΣ](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Χρήση λειτουργικές μονάδες ασφαλείας υλικού

Οι λύσεις κρυπτογράφησης κλάδο χρησιμοποιούν μυστικού πλήκτρα για την κρυπτογράφηση των δεδομένων. Επομένως, είναι σημαντικό τα πλήκτρα αποθηκεύονται με ασφάλεια. Διαχείριση κλειδιών γίνεται αναπόσπαστο μέρος της προστασίας των δεδομένων, καθώς αυτό θα να χρησιμοποιηθούν για την αποθήκευση μυστικού κλειδιών που χρησιμοποιούνται για την κρυπτογράφηση των δεδομένων.

Κρυπτογράφηση Azure δίσκου χρησιμοποιεί [Azure θάλαμο αριθμού-κλειδιού](https://azure.microsoft.com/services/key-vault/) για να σας βοηθήσει να ελέγξετε και να διαχειριστείτε κλειδιών κρυπτογράφησης δίσκου και απορρήτου στη συνδρομή σας κλειδιού θάλαμο, εξασφαλίζοντας ότι όλα τα δεδομένα των δίσκων εικονική μηχανή είναι κρυπτογραφημένα στο υπόλοιπο στο χώρο αποθήκευσης του Azure. Θα πρέπει να μπορείτε να χρησιμοποιήσετε Azure θάλαμο αριθμού-κλειδιού για να ελέγξετε πλήκτρα και χρήσης πολιτικής.

Υπάρχουν πολλά κινδύνους που σχετίζονται με χωρίς στοιχεία ελέγχου ασφαλείας στη θέση για να προστατεύσετε τα κρυφά κλειδιά που χρησιμοποιήθηκαν για την κρυπτογράφηση των δεδομένων σας. Εάν εισβολείς έχουν πρόσβαση στα μυστικού κλειδιά, θα μπορεί να αποκρυπτογραφήσει τα δεδομένα και ενδεχομένως να έχουν πρόσβαση σε εμπιστευτικές πληροφορίες.

Μπορείτε να μάθετε περισσότερα σχετικά με τις γενικές συστάσεις για τη διαχείριση πιστοποιητικών στο Azure διαβάζοντας το άρθρο [Διαχείριση πιστοποιητικών στο Azure: οδηγίες και όχι](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Για περισσότερες πληροφορίες σχετικά με το Azure κλειδί θάλαμο, διαβάστε [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Διαχείριση με ασφαλή σταθμούς εργασίας

Επειδή το τεράστιες μεγαλύτερο μέρος των επιθέσεις προορισμού του τελικού χρήστη, το τελικό σημείο μετατρέπεται σε ένα από τα κύρια σημεία της επίθεση. Εάν ένας εισβολέας απευθείας το τελικό σημείο, αυτός να αξιοποιήσετε τα διαπιστευτήρια του χρήστη για να αποκτήσετε πρόσβαση στα δεδομένα της εταιρείας. Οι περισσότερες επιθέσεις τελικού σημείου έχουν τη δυνατότητα για να επωφεληθείτε από το γεγονός ότι οι τελικοί χρήστες είναι διαχειριστές σε τους τοπικό σταθμούς εργασίας.

Μπορείτε να μειώσετε αυτών των κινδύνων, χρησιμοποιώντας μια σταθμούς εργασίας ασφαλούς διαχείρισης. Συνιστάται να χρησιμοποιήσετε ένα [Πόδι Προνομιούχους Access σταθμούς εργασίας (ΖΏΟΥ)](https://technet.microsoft.com/library/mt634654.aspx) για να μειώσετε την επιφάνεια επίθεση σε σταθμούς εργασίας. Αυτές οι σταθμούς εργασίας ασφαλούς διαχείρισης μπορούν να σας βοηθήσουν μετριασμός ορισμένες από αυτές τις επιθέσεις να εξασφαλίζεται ότι τα δεδομένα σας είναι πιο ασφαλές. Βεβαιωθείτε ότι χρησιμοποιείτε πόδι ΖΏΟΥ να σταθεροποίηση και να κλειδώσετε σας σταθμούς εργασίας. Αυτό είναι ένα σημαντικό βήμα για να παρέχουν διασφαλίσεις υψηλό επίπεδο ασφάλειας για ευαίσθητες λογαριασμούς, εργασίες και προστασία των δεδομένων.

Έλλειψη τελικού σημείου προστασίας μπορεί να τοποθετήσει τα δεδομένα σας σε κίνδυνο, βεβαιωθείτε ότι για να επιβάλετε πολιτικές ασφαλείας από όλες τις συσκευές που χρησιμοποιούνται για την εκμετάλλευση δεδομένων, ανεξάρτητα από τη θέση δεδομένων (cloud ή εσωτερικής εγκατάστασης).

Μπορείτε να μάθετε περισσότερα σχετικά με τα δικαιώματα πρόσβασης σταθμούς εργασίας διαβάζοντας το άρθρο [Ασφάλιση δικαιώματα πρόσβασης](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>Ενεργοποίηση κρυπτογράφηση δεδομένων SQL

[Κρυπτογράφηση βάσης δεδομένων SQL Azure διαφανή δεδομένων](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) συμβάλλει στην προστασία υπό την απειλή κακόβουλης δραστηριότητας, εκτελώντας σε πραγματικό χρόνο κρυπτογράφηση και αποκρυπτογράφηση τη βάση δεδομένων, συσχετισμένη αντίγραφα ασφαλείας και αρχεία καταγραφής συναλλαγών στο υπόλοιπο χωρίς να απαιτείται αλλαγές στην εφαρμογή.  TDE κρυπτογραφεί την αποθήκευση μιας ολόκληρης βάσης δεδομένων με τη χρήση συμμετρική κλειδιού που ονομάζεται του κλειδιού κρυπτογράφησης βάσης δεδομένων.

Ακόμα και όταν το ολόκληρου χώρου αποθήκευσης είναι κρυπτογραφημένα, είναι πολύ σημαντικό για να κρυπτογραφήσετε επίσης την ίδια σας βάση δεδομένων. Αυτή είναι μια εφαρμογή της το άμυνας σε βάθος προσέγγιση για την προστασία των δεδομένων. Εάν χρησιμοποιείτε [Βάση δεδομένων SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) και θέλετε να προστατεύσετε ευαίσθητα δεδομένα όπως πιστωτική κάρτα ή αριθμοί μητρώου κοινωνικής ασφάλισης, μπορείτε να κρυπτογραφήσετε βάσεις δεδομένων με FIPS κρυπτογράφηση AES 140-2 επικυρωθεί 256 bit που πληροί τις απαιτήσεις της πολλά βιομηχανικά πρότυπα (π.χ., HIPAA, PCI).

Είναι σημαντικό να κατανοήσετε ότι τα αρχεία που σχετίζονται με [την επέκταση χώρου συγκέντρωσης buffer](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) δεν είναι κρυπτογραφημένα όταν μια βάση δεδομένων είναι κρυπτογραφημένη χρησιμοποιώντας TDE. Πρέπει να χρησιμοποιήσετε εργαλεία κρυπτογράφηση σε επίπεδο αρχείο συστήματος, όπως το BitLocker ή το [Σύστημα αρχείων κρυπτογράφησης](https://technet.microsoft.com/library/cc700811.aspx) (EFS) για BPE που σχετίζονται με τα αρχεία.

Από έναν εξουσιοδοτημένο χρήστη, όπως ένα διαχειριστή ασφαλείας ή διαχειριστή βάσης δεδομένων μπορεί να πρόσβαση στα δεδομένα, ακόμα και αν η βάση δεδομένων είναι κρυπτογραφημένα με TDE, θα πρέπει να επίσης ακολουθήσετε τις παρακάτω συστάσεις:

- Έλεγχος ταυτότητας του SQL στο επίπεδο της βάσης δεδομένων
- Azure AD ελέγχου ταυτότητας με τους ρόλους RBAC
- Χρήστες και εφαρμογές πρέπει να χρησιμοποιήσετε ξεχωριστούς λογαριασμούς για τον έλεγχο ταυτότητας. Με αυτόν τον τρόπο, μπορείτε να περιορίσετε τα δικαιώματα που έχουν εκχωρηθεί σε χρήστες και εφαρμογές και να μειώσετε τους κινδύνους της κακόβουλης δραστηριότητας
- Υλοποίηση ασφάλειας σε επίπεδο βάσης δεδομένων με τη χρήση τους ρόλους σταθερό βάσης δεδομένων (όπως db_datareader ή db_datawriter), ή μπορείτε να δημιουργήσετε προσαρμοσμένες τους ρόλους για την εφαρμογή σας για να εκχωρήσετε ρητά δικαιώματα σε επιλεγμένα αντικείμενα βάσης δεδομένων

Εταιρείες που δεν χρησιμοποιούν το επίπεδο κρυπτογράφηση βάσης δεδομένων μπορεί να είναι πιο ευαίσθητο για επιθέσεις που ενδέχεται να μειώσει δεδομένα που βρίσκονται σε βάσεις δεδομένων SQL.

Μπορείτε να μάθετε περισσότερα σχετικά με την κρυπτογράφηση SQL TDE διαβάζοντας το άρθρο [Διαφανή κρυπτογράφηση δεδομένων με βάση δεδομένων SQL Azure](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Προστατεύστε τα δεδομένα κατά τη μεταφορά

Προστασία δεδομένων κατά τη μεταφορά πρέπει να είναι βασικές μέρος της στρατηγικής προστασίας δεδομένων. Εφόσον δεδομένων θα Μετακίνηση εμπρός και πίσω από πολλές θέσεις, τη γενική σύσταση είναι να χρησιμοποιείτε πάντα SSL/TLS πρωτόκολλα για την ανταλλαγή δεδομένων σε διαφορετικές θέσεις. Σε ορισμένες περιπτώσεις, ίσως θέλετε να απομονώσετε το κανάλι ολόκληρη την επικοινωνία μεταξύ της εσωτερικής εγκατάστασης και cloud υποδομή, χρησιμοποιώντας μια εικονικού ιδιωτικού δικτύου (VPN).

Για μετακίνηση μεταξύ σας υποδομή εσωτερικής εγκατάστασης και του Azure τα δεδομένα, πρέπει να λάβετε υπόψη κατάλληλες εγγυήσεις όπως HTTPS ή VPN.

Για εταιρείες που πρέπει να εξασφάλισης της πρόσβασης από πολλούς σταθμούς εργασίας βρίσκεται στην εσωτερική εγκατάσταση για να Azure, χρησιμοποιήστε [Azure VPN-τοποθεσίας](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Για εταιρείες που πρέπει να ασφαλή πρόσβαση από μία σταθμούς εργασίας που βρίσκεται στην εσωτερική εγκατάσταση σε Azure, χρησιμοποιήστε [σημείο σε τοποθεσία VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Μεγαλύτερο δεδομένων σύνολα μπορούν να μετακινηθούν επάνω από ένα αποκλειστικό WAN υψηλής ταχύτητας σύνδεση όπως [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Εάν επιλέξετε να χρησιμοποιήσετε ExpressRoute, μπορείτε επίσης να κρυπτογραφήσετε τα δεδομένα στο επίπεδο της εφαρμογής με τη χρήση [SSL/TLS](https://support.microsoft.com/kb/257591) ή άλλα πρωτόκολλα για προσθέσει προστασία.

Εάν αλληλεπίδραση με το χώρο αποθήκευσης Azure μέσω της πύλης Azure, όλες οι συναλλαγές προκύψουν μέσω HTTPS. [Χώρος αποθήκευσης REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx) μέσω HTTPS επίσης μπορεί να χρησιμοποιηθεί για να αλληλεπιδράσετε με το [Χώρο αποθήκευσης Azure](https://azure.microsoft.com/services/storage/) και [Βάση δεδομένων SQL Azure](https://azure.microsoft.com/services/sql-database/).

Εταιρείες που να προστατέψετε τα δεδομένα κατά τη μεταφορά αποτύχει είναι πιο ευαίσθητα για [επιθέσεις στο μέσο](https://technet.microsoft.com/library/gg195821.aspx), [μη εξουσιοδοτημένη ανάγνωση μηνυμάτων](https://technet.microsoft.com/library/gg195641.aspx) και πειρατεία περιόδου λειτουργίας. Αυτές οι επιθέσεις μπορεί να είναι το πρώτο βήμα για να αποκτήσετε πρόσβαση σε εμπιστευτικά δεδομένα.

Μπορείτε να μάθετε περισσότερα σχετικά με την επιλογή Azure VPN διαβάζοντας το άρθρο [Προγραμματισμός και σχεδίαση για την πύλη VPN](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Επιβολή κρυπτογράφησης δεδομένων σε επίπεδο αρχείο

Ένα άλλο επίπεδο προστασίας που μπορεί να αυξήσουν το επίπεδο ασφαλείας για τα δεδομένα σας η κρυπτογράφηση το ίδιο το αρχείο, ανεξάρτητα από τη θέση του αρχείου.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) χρησιμοποιεί κρυπτογράφηση, ταυτότητας και εξουσιοδότηση πολιτικές για να ασφαλίσετε τα αρχεία και ηλεκτρονικού ταχυδρομείου. Azure RMS λειτουργεί σε πολλές συσκευές, τηλέφωνα, Tablet και υπολογιστές με προστασία τόσο εντός της εταιρείας σας και εκτός της εταιρείας σας. Αυτή η δυνατότητα δεν είναι επειδή Azure RMS προσθέτει ένα επίπεδο προστασίας που απομένει με τα δεδομένα του, ακόμα και όταν το αφήνει τα όρια της εταιρείας σας.

Όταν χρησιμοποιείτε Azure RMS για να προστατεύσετε τα αρχεία σας, χρησιμοποιείτε κρυπτογράφησης βιομηχανικών προτύπων με πλήρη υποστήριξη για [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Όταν μπορείτε να αξιοποιήσετε Azure RMS για προστασία των δεδομένων, έχετε την εξασφάλιση ότι η προστασία παραμένει με το αρχείο, ακόμα και αν έχει αντιγραφεί χώρου αποθήκευσης που δεν είναι κάτω από τον έλεγχο IT, όπως μια υπηρεσία αποθήκευσης στο cloud. Το ίδιο συμβαίνει για αρχεία σε κοινή χρήση μέσω ηλεκτρονικού ταχυδρομείου, το αρχείο είναι προστατευμένο ως συνημμένο σε μήνυμα ηλεκτρονικού ταχυδρομείου με οδηγίες πώς μπορείτε να ανοίξετε το συνημμένο προστατευμένο.

Κατά το σχεδιασμό για υιοθέτησης Azure RMS προτείνουμε να τα εξής:

- Εγκαταστήστε το [RMS κοινής χρήσης εφαρμογής](https://technet.microsoft.com/library/dn339006.aspx). Αυτή η εφαρμογή ενοποιείται με το Office εφαρμογές κατά την εγκατάσταση του Office ενός προσθέτου, έτσι ώστε οι χρήστες να προστατέψετε εύκολα αρχεία απευθείας.
- Ρύθμιση παραμέτρων των εφαρμογών και υπηρεσιών για την υποστήριξη Azure RMS
- Δημιουργήστε [προσαρμοσμένα πρότυπα](https://technet.microsoft.com/library/dn642472.aspx) που αντικατοπτρίζουν τις ανάγκες της επιχείρησής σας. Για παράδειγμα: ένα πρότυπο για επάνω μυστικού δεδομένα τα οποία θα πρέπει να εφαρμόζεται σε όλα επάνω μυστικό που σχετίζονται με μηνύματα ηλεκτρονικού ταχυδρομείου.

Εταιρείες που έχουν αδύναμους στην προστασία [δεδομένων ταξινόμηση](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) και το αρχείο μπορεί να είναι περισσότερο ευαίσθητα στη διαρροής δεδομένων. Χωρίς προστασία proper αρχείων, δεν θα μπορείτε να αποκτήσετε ιδέες επιχειρήσεις, να παρακολουθείτε για κατάχρηση και να αποτρέψετε κακόβουλο πρόσβαση στα αρχεία εταιρείες.

Μπορείτε να μάθετε περισσότερα σχετικά με το Azure RMS διαβάζοντας το άρθρο [Γρήγορα αποτελέσματα με τη Διαχείριση δικαιωμάτων Azure](https://technet.microsoft.com/library/jj585016.aspx).

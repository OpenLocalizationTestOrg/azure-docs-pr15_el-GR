<properties
    pageTitle="Επισκόπηση συγκριτικών Azure βάσης δεδομένων SQL"
    description="Αυτό το θέμα περιγράφει τα συγκριτικών βάσης δεδομένων SQL Azure χρησιμοποιούνται μετρούν τις επιδόσεις της βάσης δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="06/21/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-benchmark-overview"></a>Επισκόπηση συγκριτικών Azure βάση δεδομένων SQL

## <a name="overview"></a>Επισκόπηση
Βάση δεδομένων SQL Microsoft Azure προσφέρει τρεις [επίπεδα υπηρεσίας](sql-database-service-tiers.md) με πολλά επίπεδα επιδόσεων. Κάθε επίπεδο επιδόσεων παρέχει ένα σύνολο αυξανόμενος πόρους ή 'ενέργειας', που έχει σχεδιαστεί για να παρέχουν αυξανόμενη υψηλότερη μετάδοσης.

Είναι σημαντικό να μπορούν να προσδιορίστε ποσοτικά πώς αυξανόμενος power κάθε επίπεδο επιδόσεων μεταφράζει σε απόδοσης αυξημένη μιας βάσης δεδομένων. Για να το κάνετε αυτό Microsoft έχει αναπτύξει το συγκριτικών βάση δεδομένων SQL Azure (ASDB). Το σημείο αναφοράς εκτελεί συνδυασμό βρέθηκαν σε όλους τους φόρτους εργασίας Συναλλαγής βασικές λειτουργίες. Θα σας μετρήστε την ταχύτητα μεταγωγής πραγματοποιείται για τις βάσεις δεδομένων εκτελούνται σε κάθε επίπεδο επιδόσεων.

Οι πόροι και power κάθε επίπεδο επίπεδο και επιδόσεων υπηρεσίας εκφράζονται όσον αφορά τις [Μονάδες συναλλαγή βάσης δεδομένων (DTUs)](sql-database-technical-overview.md#understand-dtus). DTUs παρέχει κάποιο τρόπο για να περιγράψετε τη σχετική χωρητικότητα του επιπέδου απόδοσης που βασίζεται σε ανάμειξη μέτρηση της CPU, μνήμη, ανάγνωση και εγγραφή χρεώσεις που παρέχεται από κάθε επίπεδο επιδόσεων. Διπλασιασμού το DTU χαρακτηρισμό μιας βάσης δεδομένων ισούται με διπλασιασμού power βάσης δεδομένων. Το σημείο αναφοράς σας επιτρέπει να αξιολογήσετε τις επιπτώσεις στην απόδοση της βάσης δεδομένων της ισχύος του αυξανόμενου που παρέχεται από κάθε επίπεδο επιδόσεων, ασκώντας λειτουργιών πραγματική βάσης δεδομένων, ενώ κλίμακας μέγεθος της βάσης δεδομένων, αριθμός των χρηστών και χρεώσεις συναλλαγής ανάλογα με τους πόρους που παρέχονται στη βάση δεδομένων.

Εκφράζοντας την ταχύτητα μεταγωγής από το επίπεδο βασικές υπηρεσιών χρησιμοποιώντας συναλλαγές ανά ώρα, η σειρά τυπική υπηρεσία χρησιμοποιώντας συναλλαγές ανά λεπτό και το επίπεδο υπηρεσιών Premium χρησιμοποιώντας συναλλαγές ανά δευτερόλεπτο, το σας διευκολύνει να συσχετίσετε γρήγορα τις επιδόσεις πιθανά από κάθε επίπεδο υπηρεσιών με τις απαιτήσεις μιας εφαρμογής του.

## <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Συσχέτιση αποτελεσμάτων συγκριτικών σε πραγματικό κόσμο απόδοση της βάσης δεδομένων
Είναι σημαντικό να κατανοήσετε ότι ASDB, όπως όλα σημεία αναφοράς, είναι αντιπρόσωπος και ενδεικτική μόνο. Οι συντελεστές συναλλαγής επίτευξη με την εφαρμογή συγκριτικών δεν θα είναι τα ίδια με αυτά που μπορεί να είναι με άλλες εφαρμογές. Το σημείο αναφοράς αποτελείται από μια συλλογή από διαφορετικές συναλλαγής τύποι εκτελεστεί σε ένα σχήμα που περιέχει μια περιοχή με πίνακες και τους τύπους δεδομένων. Ενώ το σημείο αναφοράς εκτελεί τις ίδιες βασικές λειτουργίες που είναι κοινά και για όλους τους φόρτους εργασίας Συναλλαγής, δεν αντιπροσωπεύει οποιαδήποτε συγκεκριμένη κλάση της βάσης δεδομένων ή εφαρμογή. Ο στόχος από το σημείο αναφοράς είναι να δώσετε ένα λογικό οδηγό με οδηγίες για τη σχετική επιδόσεων μιας βάσης δεδομένων που μπορεί να αναμένεται ότι κατά την κλιμάκωση προς τα επάνω ή προς τα κάτω μεταξύ των επιπέδων επιδόσεων. Στην πραγματικότητα, βάσεις δεδομένων είναι διαφορετικά μεγέθη και την πολυπλοκότητα αντιμετωπίσετε διαφορετικούς συνδυασμούς φόρτους εργασίας και θα ανταποκριθεί με διαφορετικούς τρόπους. Για παράδειγμα, μια εφαρμογή εισόδου/ΕΞΌΔΟΥ στενής μπορεί να πατήσετε όρια εισόδου/ΕΞΌΔΟΥ αργά ή μια εφαρμογή υπολογιστική μπορεί να πατήσετε όρια CPU αργά. Δεν υπάρχει εγγυημένη που θα κλιμακωθεί οποιαδήποτε συγκεκριμένη βάση δεδομένων με τον ίδιο τρόπο αναφοράς στην περιοχή αύξηση φορτίου.

Το σημείο αναφοράς και την μεθοδολογία περιγράφονται με περισσότερες λεπτομέρειες παρακάτω.

## <a name="benchmark-summary"></a>Σύνοψη συγκριτικών
ASDB υπολογίζει την απόδοση του συνδυασμό βασικές λειτουργιών της βάσης δεδομένων που προκύπτουν συχνότερα στην ηλεκτρονική συναλλαγή επεξεργασίας φόρτους εργασίας (Συναλλαγής). Παρόλο που το σημείο αναφοράς έχει σχεδιαστεί με το cloud υπολογιστική σε γνώμη, το σχήμα της βάσης δεδομένων, δεδομένα πληθυσμού και συναλλαγές έχουν σχεδιαστεί ώστε να είναι ευρέως αντιπρόσωπος από τα βασικά στοιχεία που χρησιμοποιείται πιο συχνά σε Συναλλαγής φόρτους εργασίας.

## <a name="schema"></a>Σχήμα
Το σχήμα έχει σχεδιαστεί για να έχετε αρκετό ποικιλία και την πολυπλοκότητα για την υποστήριξη ένα ευρύ φάσμα των λειτουργιών. Το σημείο αναφοράς εκτελείται σε σχέση με μια βάση δεδομένων που αποτελείται από τα έξι πίνακες. Οι πίνακες υπάρχουν τρεις κατηγορίες: σταθερού μεγέθους, κλιμάκωση και την ανάπτυξη. Υπάρχουν δύο πίνακες σταθερού μεγέθους. τρεις κλίμακας πίνακες. και μία αυξανόμενη πίνακα. Οι πίνακες σταθερού μεγέθους έχουν έναν σταθερό αριθμό γραμμών. Κλίμακας πίνακες έχουν μια προτεραιότητα που είναι ανάλογο με απόδοση της βάσης δεδομένων, αλλά δεν αλλάζει κατά το σημείο αναφοράς. Μέγεθος του πίνακα αυξανόμενη όπως κλίμακας πίνακα σε αρχικής φόρτωσης, αλλά οι αλλαγές προτεραιότητα κατά τη διάρκεια εκτέλεσης του συγκριτικών ως σειρές που έχουν εισαχθεί και διαγραφεί.

Το σχήμα περιλαμβάνει συνδυασμό των τύπων δεδομένων, συμπεριλαμβανομένων των ακέραιου αριθμού αριθμητικό χαρακτήρα και ημερομηνίας/ώρας. Το σχήμα περιλαμβάνει πρωτεύοντα και δευτερεύοντα κλειδιά, αλλά δεν οποιαδήποτε ξένα κλειδιά – αυτό σημαίνει ότι υπάρχουν χωρίς περιορισμούς της ακεραιότητας αναφορών μεταξύ πινάκων.

Ένα πρόγραμμα γενιάς δεδομένων δημιουργεί τα δεδομένα για την αρχική βάση δεδομένων. Ακέραιος και αριθμητικών δεδομένων δημιουργείται με διάφορες στρατηγικές. Σε ορισμένες περιπτώσεις, διανέμονται τυχαία τιμές σε μια περιοχή. Σε άλλες περιπτώσεις, ένα σύνολο τιμών είναι τυχαία της σκίασης για να βεβαιωθείτε ότι διατηρούνται μια συγκεκριμένη κατανομή. Πεδία κειμένου δημιουργούνται από μια λίστα των λέξεων για την παραγωγή ρεαλιστική εμφάνιση δεδομένων.

Μέγεθος της βάσης δεδομένων που βασίζεται σε μια "συντελεστής κλίμακας". Ο παράγοντας κλίμακας (συντομογραφία Ελ) καθορίζει την προτεραιότητα της την κλίμακα και την ανάπτυξη της πίνακες. Όπως περιγράφεται παρακάτω στην ενότητα, οι χρήστες και Pacing, το μέγεθος της βάσης δεδομένων, αριθμός των χρηστών και μέγιστη απόδοση όλα κλιμάκωση ανάλογα με το μεταξύ τους.

## <a name="transactions"></a>Συναλλαγές
Το φόρτο εργασίας αποτελείται από εννέα τύπους κίνησης, όπως φαίνεται στον παρακάτω πίνακα. Κάθε συναλλαγή έχει σχεδιαστεί για να επισημάνετε ένα συγκεκριμένο σύνολο χαρακτηριστικών συστήματος στο βάσης δεδομένων μηχανισμός και σύστημα υλικό, με υψηλή αντίθεση από τις άλλες συναλλαγές. Αυτή η προσέγγιση διευκολύνει την πρόσβαση στο αποτέλεσμα διάφορα στοιχεία για τη συνολική απόδοση. Για παράδειγμα, η συναλλαγή "Ανάγνωση χοντρό" παράγει ένα σημαντικό αριθμό των λειτουργιών ανάγνωσης από δίσκο.

| Τύπος κίνησης | Περιγραφή |
|---|---|
| Διαβάστε Lite | ΕΠΙΛΈΞΤΕ; στη μνήμη; μόνο για ανάγνωση |
| Ανάγνωση Μεσαίο | ΕΠΙΛΈΞΤΕ; κυρίως στη μνήμη; μόνο για ανάγνωση |
| Ανάγνωση χοντρό | ΕΠΙΛΈΞΤΕ; κυρίως δεν στη μνήμη; μόνο για ανάγνωση |
| Ενημέρωση Lite | Η ΕΝΗΜΈΡΩΣΗ. στη μνήμη; ανάγνωση και εγγραφή |
| Ενημέρωση χοντρό | Η ΕΝΗΜΈΡΩΣΗ. κυρίως δεν στη μνήμη; ανάγνωση και εγγραφή |
| Εισαγωγή Lite | ΕΙΣΑΓΩΓΉ; στη μνήμη; ανάγνωση και εγγραφή |
| Εισαγωγή χοντρό | ΕΙΣΑΓΩΓΉ; κυρίως δεν στη μνήμη; ανάγνωση και εγγραφή |
| Διαγραφή | ΔΙΑΓΡΑΦΉ; συνδυασμό στη μνήμη και όχι στη μνήμη; ανάγνωση και εγγραφή |
| Χοντρό CPU | ΕΠΙΛΈΞΤΕ; στη μνήμη; σχετικά φόρτο εργασίας CPU; μόνο για ανάγνωση |

## <a name="workload-mix"></a>Συνδυασμός φόρτο εργασίας
Συναλλαγές επιλέγονται τυχαία από μια σταθμισμένη κατανομή με τη συνολική mix παρακάτω. Το συνολικό mix έχει μια αναλογία ανάγνωσης/εγγραφής περίπου 2:1.

| Τύπος κίνησης | % του Mix |
|---|---|
| Διαβάστε Lite | 35 |
| Ανάγνωση Μεσαίο | 20 |
| Ανάγνωση χοντρό | 5 |
| Ενημέρωση Lite | 20 |
| Ενημέρωση χοντρό | 3 |
| Εισαγωγή Lite | 3 |
| Εισαγωγή χοντρό | 2 |
| Διαγραφή | 2 |
| Χοντρό CPU | 10 |

## <a name="users-and-pacing"></a>Χρήστες και pacing
Το φόρτο εργασίας συγκριτικών κινείται από ένα εργαλείο που υποβάλλει συναλλαγές σε ένα σύνολο συνδέσεων για να προσομοιώσετε τη συμπεριφορά ενός αριθμού ταυτόχρονες χρηστών. Παρόλο που όλες τις συνδέσεις και συναλλαγές που δημιουργούνται από τον υπολογιστή, για λόγους ευκολίας αναφέρουμε αυτές τις συνδέσεις ως "χρήστες". Παρόλο που κάθε χρήστης λειτουργεί ανεξάρτητα από όλους τους άλλους χρήστες, όλοι οι χρήστες εκτελούν τον ίδιο κύκλο βήματα που φαίνεται παρακάτω:

1. Δημιουργήστε μια σύνδεση βάσης δεδομένων.
2. Επαναλάβετε μέχρι να ειδοποιήσει για να εξέλθετε από:
    - Επιλέξτε μια συναλλαγή τυχαία (από μια κατανομή σταθμισμένου).
    - Εκτέλεση της επιλεγμένης συναλλαγής και Μετρήστε το χρόνο απόκρισης.
    - Περιμένετε έως ότου pacing καθυστέρηση.
3. Κλείστε τη σύνδεση βάσης δεδομένων.
4. Έξοδος.

Την καθυστέρηση pacing (στο βήμα 2c) είναι επιλεγμένο τυχαία, αλλά με κατανομή που περιλαμβάνει ο μέσος όρος 1,0 δευτερόλεπτα. Επομένως, κάθε χρήστης μπορεί να, κατά μέσο όρο, να δημιουργήσουν πολύ μία συναλλαγή ανά δευτερόλεπτο.

## <a name="scaling-rules"></a>Κλιμάκωση κανόνων
Ο αριθμός των χρηστών προσδιορίζεται από το μέγεθος της βάσης δεδομένων (σε μονάδες κλίμακας παραγόντων). Υπάρχει ένα χρήστη για κάθε πέντε μονάδες κλίμακας παραγόντων. Λόγω του pacing καθυστέρηση, ένας χρήστης μπορεί να δημιουργήσει πολύ μία συναλλαγή ανά δευτερόλεπτο, κατά μέσο όρο.

Για παράδειγμα, μια κλίμακα παραγόντων 500 (Ελ = 500) βάση δεδομένων θα έχει 100 χρήστες και να επιτύχετε μέγιστη ταχύτητα 100 TPS. Για να καθοδηγήσετε μια υψηλότερη TPS επιτόκιο απαιτεί περισσότερους χρήστες και μια μεγαλύτερη βάση δεδομένων.

Ο παρακάτω πίνακας εμφανίζει τον αριθμό των χρηστών στην πραγματικότητα υποστεί για κάθε επίπεδο επίπεδο και επιδόσεων υπηρεσίας.

| Επίπεδο υπηρεσιών (επίπεδο επιδόσεων) | Οι χρήστες | Μέγεθος βάσης δεδομένων |
|---|---|---|
| Βασική | 5 | 720 MB |
| Τυπική (S0) | 10 | 1 GB |
| Τυπική (S1) | 20 | 2.1 GB |
| Τυπική (S2) | 50 | 7.1 GB |
| Premium (P1) | 100 | 14 GB |
| Premium (P2) | 200 | 28 GB |
| Premium (P6 P3) | 800 | 114 GB |

## <a name="measurement-duration"></a>Διάρκεια μέτρησης
Μια έγκυρη συγκριτικών εκτέλεση απαιτεί μια σταθερή κατάσταση μέτρησης διάρκεια τουλάχιστον μία ώρα.

## <a name="metrics"></a>Μετρήσεις
Το πλήκτρο μετρικά στο το σημείο αναφοράς είναι μετάδοσης και ο χρόνος απόκρισης.

- Απόδοση είναι το μέτρο βασικές απόδοσης με το σημείο αναφοράς. Μετάδοσης αναφέρεται σε συναλλαγές ανά μονάδα-του-χρόνου, μετρώντας όλους τους τύπους κίνησης.
- Χρόνος απόκρισης είναι μια μέτρηση της απόδοσης προβλεψιμότητα. Τον περιορισμό του χρόνου απόκρισης ποικίλλει, ανάλογα με την κλάση της υπηρεσίας, με υψηλότερη κλάσεις της υπηρεσίας χρειάζεται μια πιο αυστηρά απαίτηση χρόνο απόκρισης, όπως φαίνεται παρακάτω.

| Εφαρμογή της υπηρεσίας  | Μέτρηση μετάδοσης | Απαίτησης για το χρόνο απόκρισης |
|---|---|---|
| Premium | Συναλλαγές ανά δευτερόλεπτο | 95th εκατοστημορίου στο 0,5 δευτερόλεπτα |
| Τυπική | Συναλλαγές ανά λεπτό | 90 εκατοστημορίου στο 1,0 δευτερόλεπτα |
| Βασική | Συναλλαγές ανά ώρα | 80th εκατοστημορίου στο 2.0 δευτερόλεπτα |

## <a name="conclusion"></a>Ολοκλήρωση
Το σημείο αναφοράς της βάσης δεδομένων SQL Azure υπολογίζει την σχετική απόδοση της βάσης δεδομένων SQL Azure εκτελείται κατά μήκος της περιοχής βαθμίδες διαθέσιμη υπηρεσία και επιπέδων επιδόσεων. Το σημείο αναφοράς εκτελεί συνδυασμό βασικές λειτουργιών της βάσης δεδομένων που προκύπτουν συχνότερα στην ηλεκτρονική συναλλαγή επεξεργασίας φόρτους εργασίας (Συναλλαγής). Με τη μέτρηση πραγματική απόδοση, το σημείο αναφοράς παρέχει ένα πιο χαρακτηριστικό αξιολόγηση των επιπτώσεων μετάδοσης της αλλαγής στο επίπεδο επιδόσεων από αυτήν που επιτρέπεται από την καταχώρηση μόνο τους πόρους που παρέχεται από κάθε επίπεδο όπως ταχύτητα CPU, το μέγεθος της μνήμης και IOP Προέλευσης. Στο μέλλον, θα συνεχίσουμε να εξελίσσονται το σημείο αναφοράς για να διευρύνετε το πεδίο εφαρμογής και να αναπτύξετε τα δεδομένα που παρέχονται.

## <a name="resources"></a>Πόροι
[Εισαγωγή σε βάση δεδομένων SQL](sql-database-technical-overview.md)

[Επίπεδα υπηρεσίας και επιδόσεων](sql-database-service-tiers.md)

[Καθοδήγηση επιδόσεων για μία μόνο βάσεις δεδομένων](sql-database-performance-guidance.md)

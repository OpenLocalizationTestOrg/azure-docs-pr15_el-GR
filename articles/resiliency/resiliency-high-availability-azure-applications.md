<properties
   pageTitle="Υψηλή διαθεσιμότητα για τις εφαρμογές του Azure | Microsoft Azure"
   description="Τεχνική επισκόπηση και λεπτομερείς πληροφορίες σχετικά με τη σχεδίαση και δημιουργία εφαρμογών για υψηλή διαθεσιμότητα στο Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-for-applications-built-on-microsoft-azure"></a>Υψηλή διαθεσιμότητα για εφαρμογές που δημιουργήθηκαν στο Microsoft Azure

Μια εφαρμογή ιδιαίτερα διαθέσιμη ότι απορροφούν διακυμάνσεις διαθεσιμότητα, φόρτωση και προσωρινό αποτυχίες κατά το υλικό και εξαρτημένες υπηρεσίες. Η εφαρμογή εξακολουθεί να λειτουργεί σε μια αποδεκτή χρήστη και η απάντηση επίπεδο, όπως ορίζονται από επιχειρησιακές απαιτήσεις ή τις συμβάσεις επιπέδου υπηρεσιών εφαρμογής (SLA).

##<a name="azure-high-availability-features"></a>Azure δυνατότητες υψηλής διαθεσιμότητας

Azure έχει πολλές δυνατότητες ενσωματωμένη πλατφόρμα που υποστηρίζουν ιδιαίτερα διαθέσιμες εφαρμογές. Αυτή η ενότητα περιγράφει ορισμένες από αυτές τις βασικές δυνατότητες. Για μια πιο ολοκληρωμένη ανάλυση της πλατφόρμας, ανατρέξτε στο θέμα [τεχνική καθοδήγηση Azure υποστηρίζεται](./resiliency-technical-guidance.md).

###<a name="fabric-controller"></a>Ύφασμα ελεγκτή

Στον ελεγκτή Azure ύφασμα διατάξεις και παρακολουθεί τη συνθήκη των παρουσιών Azure υπολογισμού. Στον ελεγκτή ύφασμα ελέγχει την κατάσταση του υλικού και λογισμικού των παρουσιών μηχανικής κεντρικός υπολογιστής και επισκέπτη. Όταν εντοπίσει αποτυχία, επιβάλλει SLA με αυτόματη μετακίνηση των εμφανίσεων Εικονική. Η έννοια της περαιτέρω τομείς σφαλμάτων και αναβάθμιση υποστηρίζει το SLA υπολογισμού.

Όταν έχουν αναπτυχθεί πολλές παρουσίες ρόλο υπηρεσία Cloud, Azure ανάπτυξη αυτές τις περιπτώσεις σε διαφορετική σφαλμάτων τομείς. Ένα όριο τομέα σφαλμάτων στην ουσία είναι ένα διαφορετικό υλικό rack στην ίδια περιοχή. Σφάλμα τομείς μειώστε την πιθανότητα ότι αποτυχία μεταφρασμένα υλικού θα προκαλέσει τη διακοπή της υπηρεσίας μιας εφαρμογής του. Δεν μπορείτε να διαχειριστείτε τον αριθμό των τομέων σφαλμάτων που έχουν εκχωρηθεί σε εργαζόμενου ή ρόλους web σας. Στον ελεγκτή ύφασμα χρησιμοποιεί αποκλειστικό πόρους που είναι ξεχωριστό από τις εφαρμογές που φιλοξενείται στο Azure. Έχει 100 τοις εκατό συνεχούς επειδή αυτό χρησιμοποιείται ως το πυρήνας του Azure συστήματος. Παρακολουθεί και διαχειρίζεται παρουσίες ρόλο σε άλλους τομείς σφαλμάτων.

Το παρακάτω διάγραμμα παρουσιάζει Azure κοινόχρηστους πόρους ότι στον ελεγκτή ύφασμα αναπτύσσεται και διαχειρίζεται σε διαφορετική σφαλμάτων τομείς.

![Απλοποιημένη προβολή του απομόνωση τομέα σφαλμάτων](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Αναβάθμιση τομείς είναι παρόμοια με τομείς σφάλμα στη συνάρτηση, αλλά που υποστηρίζουν αναβαθμίσεις και όχι αποτυχίες. Έναν τομέα της αναβάθμισης είναι μια λογική μονάδα διαχωρισμού παρουσία που καθορίζει ποιες παρουσίες σε μια συγκεκριμένη υπηρεσία θα αναβαθμιστούν σε ένα σημείο στο χρόνο. Από προεπιλογή, για την ανάπτυξη υπηρεσία, ορίζονται πέντε αναβάθμισης τομείς. Ωστόσο, μπορείτε να αλλάξετε αυτήν την τιμή στο αρχείο ορισμού της υπηρεσίας. Για παράδειγμα, ας υποθέσουμε ότι έχετε οκτώ παρουσίες του ρόλου σας web. Θα υπάρχουν δύο παρουσίες σε τρεις τομείς αναβάθμισης και δύο παρουσίες σε έναν τομέα αναβάθμισης. Azure ορίζει την ακολουθία ενημέρωση, αλλά αυτό είναι με βάση τον αριθμό των τομέων αναβάθμισης. Για περισσότερες πληροφορίες σχετικά με την αναβάθμιση τομέων, ανατρέξτε στο θέμα [Ενημέρωση μια υπηρεσία στο cloud](../cloud-services/cloud-services-update-azure-service.md).

###<a name="features-in-other-services"></a>Δυνατότητες σε άλλες υπηρεσίες

Εκτός από τις δυνατότητες πλατφόρμα που υποστηρίζουν διαθεσιμότητα υψηλή υπολογισμού, Azure ενσωματώνει χαρακτηριστικά υψηλής διαθεσιμότητας σε τις άλλες υπηρεσίες. Για παράδειγμα, το χώρο αποθήκευσης Azure διατηρεί τρία αντίγραφα των όλα blob πίνακα και δεδομένα ουράς. Επίσης, επιτρέπει την επιλογή παν αναπαραγωγής για να αποθηκεύσετε αντίγραφα ασφαλείας των αντικειμένων blob και πίνακες σε μια δευτερεύουσα περιοχή. Το δίκτυο Azure περιεχομένου παράδοσης επιτρέπει αντικείμενα BLOB προσωρινή αποθήκευση από όλο τον κόσμο πλεονασμού και κλιμάκωση. Βάση δεδομένων SQL του Azure διατηρεί επίσης πολλά αντίγραφα.

Εκτός από την [τεχνική καθοδήγηση ανοχή](https://aka.ms/bctechguide) σειρά άρθρων, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές για τη σχεδίαση της μεγάλης κλίμακας υπηρεσίες σχετικά με τις υπηρεσίες Cloud Azure](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) χαρτιού. Τα έγγραφα αυτά παρέχουν βαθύτερη συζήτησης από τις δυνατότητες διαθεσιμότητα Azure πλατφόρμας.

Παρόλο που Azure παρέχει πολλές δυνατότητες που υποστηρίζουν υψηλής διαθεσιμότητας, είναι σημαντικό να έχετε κατανοήσει τους περιορισμούς:

- Για υπολογισμού, Azure εγγυάται ότι σας τους ρόλους που είναι διαθέσιμα και εκτελείται, αλλά δεν γνωρίζω εάν η εφαρμογή σας είναι ενσωματωμένη ή υπερφορτωμένο.
- Για βάση δεδομένων SQL Azure, αναπαράγεται σύγχρονη δεδομένων μέσα στην περιοχή. Μπορείτε να επιλέξετε την ενεργή παν-αναπαραγωγή, που επιτρέπει σε έως και τέσσερα αντίγραφα επιπλέον βάσης δεδομένων στην ίδια (ή τις διαφορετικές περιοχές). Αυτά τα αντίγραφα της βάσης δεδομένων δεν είναι αντίγραφα ασφαλείας σε δεδομένη χρονική στιγμή. Βάσεις δεδομένων SQL παρέχουν δυνατότητες δημιουργίας αντιγράφων ασφαλείας στη δεδομένη χρονική στιγμή. Για να μάθετε περισσότερα σχετικά με τις δυνατότητες SQL βάσης δεδομένων σε δεδομένη χρονική στιγμή, διαβάστε [Σημείο βάσης δεδομένων SQL Azure στο χρόνο επαναφορά](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/).
- Για το χώρο αποθήκευσης Azure, δεδομένα πίνακα και αντικειμένων blob αναπαράγεται από προεπιλογή μια εναλλακτική περιοχή. Ωστόσο, δεν έχετε πρόσβαση τα αντίγραφα μέχρι να επιλέγει Microsoft να ανακατευθύνει στην εναλλακτική τοποθεσία. Ανακατεύθυνσης περιοχή πραγματοποιείται μόνο στην περίπτωση διαταραχή μακρά ολόκληρη την περιοχή υπηρεσία και δεν υπάρχει καμία SLA για χρόνο παν ανακατεύθυνσης. Είναι επίσης σημαντικό να θυμάστε ότι οποιαδήποτε καταστροφή δεδομένων δισέλιδα γρήγορα να τα αντίγραφα.

Για αυτούς τους λόγους, πρέπει να συμπληρώνουν οι δυνατότητες διαθεσιμότητα πλατφόρμα με δυνατότητες διαθεσιμότητα συγκεκριμένη εφαρμογή. Διαθεσιμότητα συγκεκριμένη εφαρμογή δυνατότητες περιλαμβάνουν τη δυνατότητα στιγμιότυπο blob για να δημιουργήσετε αντίγραφα ασφαλείας σε δεδομένη χρονική στιγμή blob δεδομένων.

###<a name="availability-sets-for-azure-virtual-machines"></a>Διαθεσιμότητα σύνολα για εικονικές μηχανές Windows Azure

Το μεγαλύτερο μέρος των σε αυτό το άρθρο εστιάζει σε υπηρεσίες cloud, οι οποίες χρησιμοποιούν μια πλατφόρμα ως ένα μοντέλο υπηρεσίας (PaaS). Ωστόσο, υπάρχουν επίσης διαθεσιμότητα συγκεκριμένες δυνατότητες για εικονικές μηχανές Windows Azure, που χρησιμοποιεί μια υποδομή ως ένα μοντέλο υπηρεσίας (IaaS). Για να επιτύχετε υψηλής διαθεσιμότητας με εικονικές μηχανές, πρέπει να χρησιμοποιήσετε τα σύνολα διαθεσιμότητα. Ένα σύνολο διαθεσιμότητα λειτουργεί μια παρόμοια λειτουργία σφαλμάτων και αναβάθμιση τομείς. Μέσα σε ένα σύνολο διαθεσιμότητα, Azure τοποθετεί τις εικονικές μηχανές με τον τρόπο που αποτρέπει την μεταφέρετε προς τα κάτω όλες οι υπολογιστές σε αυτήν την ομάδα σφαλμάτων μεταφρασμένα υλικού και δραστηριότητες συντήρησης. Διαθεσιμότητα σύνολα απαιτούνται για να επιτύχετε το SLA Azure για τη διαθεσιμότητα των εικονικές μηχανές.

Το παρακάτω διάγραμμα παρέχει μια αναπαράσταση των δύο σύνολα διαθεσιμότητα που web ομάδας και SQL Server εικονικές μηχανές, αντίστοιχα.

![Διαθεσιμότητα σύνολα για εικονικές μηχανές Windows Azure](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Στο παραπάνω διάγραμμα, SQL Server είναι εγκατεστημένη και λειτουργεί σε εικονικές μηχανές. Αυτό είναι διαφορετικό από την προηγούμενη συζήτηση της βάσης δεδομένων SQL Azure, η οποία παρέχει μια βάση δεδομένων ως διαχειριζόμενη υπηρεσία.

##<a name="application-strategies-for-high-availability"></a>Στρατηγικές εφαρμογών για υψηλή διαθεσιμότητα

Οι περισσότερες στρατηγικές εφαρμογών για υψηλή διαθεσιμότητα αφορούν πλεονασμού ή την κατάργηση του σκληρού εξαρτήσεις μεταξύ των στοιχείων της εφαρμογής. Σχεδίαση εφαρμογής θα πρέπει να υποστηρίζει ανοχή σφαλμάτων κατά τη διάρκεια σποραδική χρόνου εκτός λειτουργίας του Azure ή υπηρεσίες τρίτων κατασκευαστών. Οι παρακάτω ενότητες περιγράφουν εφαρμογή μοτίβα για την αύξηση της τη διαθεσιμότητα των υπηρεσιών σας cloud.

###<a name="asynchronous-communication-and-durable-queues"></a>Ασύγχρονη επικοινωνία και διαρκή ουρές

Εξετάστε το ενδεχόμενο ασύγχρονη επικοινωνία μεταξύ ευελιξία τα συνδεδεμένα υπηρεσιών για να αυξήσετε διαθεσιμότητα σε Azure εφαρμογές. Σε αυτό το μοτίβο, σύνταξη μηνυμάτων ουρές αποθήκευσης ή ουρές Azure Service Bus για μετέπειτα επεξεργασία. Όταν γράφετε το μήνυμα στην ουρά, ο έλεγχος επιστρέφει αμέσως στον αποστολέα του μηνύματος. Ένα άλλο επίπεδο της εφαρμογής χειρίζεται το μήνυμα επεξεργασία, συνήθως υλοποιηθεί ως ρόλος εργασίας. Εάν ο ρόλος εργαζόμενου μεταβαίνει προς τα κάτω, τα μηνύματα συσσωρεύονται σε ουρά μέχρι να γίνεται επαναφορά της υπηρεσίας επεξεργασίας. Με την προϋπόθεση ότι η ουρά είναι διαθέσιμη, δεν υπάρχει καμία απευθείας εξάρτηση μεταξύ του προσκηνίου αποστολέα και τον επεξεργαστή μηνύματος. Αυτό καταργεί την απαίτηση για κλήσεις σύγχρονη υπηρεσίας που μπορεί να είναι συμφόρηση μετάδοσης σε κατανεμημένες εφαρμογές.

Μια παραλλαγή αυτό χρησιμοποιεί χώρο αποθήκευσης Azure (αντικείμενα blob, πίνακες, ουρές) ή υπηρεσίας Bus ουρές ως θέση ανακατεύθυνσης για κλήσεις αποτυχίας βάσης δεδομένων. Για παράδειγμα, μια σύγχρονη κλήση μέσα σε μια εφαρμογή σε άλλη υπηρεσία (όπως η βάση δεδομένων SQL Azure) αποτυγχάνει επανειλημμένα. Μπορείτε ίσως να σειριοποίηση αυτά τα δεδομένα σε διαρκή χώρου αποθήκευσης. Κάποια στιγμή αργότερα όταν η υπηρεσία ή η βάση δεδομένων είναι πάλι σε σύνδεση, η εφαρμογή να υποβάλετε εκ νέου την αίτηση από το χώρο αποθήκευσης. Η διαφορά σε αυτό το μοντέλο είναι ότι η ενδιάμεση θέση δεν είναι ένα σταθερό τμήμα της ροής εργασίας της εφαρμογής. Χρησιμοποιείται μόνο σε σενάρια αποτυχία.

Και στα δύο σενάρια, ασύγχρονης επικοινωνίας και ενδιάμεση αποθήκευση εμποδίζουν downed υπηρεσίας παρασκηνίου μεταφέρετε προς τα κάτω ολόκληρη την εφαρμογή. Ουρές εξυπηρέτηση ως μια λογική ενδιάμεσου. Για περισσότερες οδηγίες σχετικά με την επιλογή τη σωστή υπηρεσία ουράς, ανατρέξτε στο θέμα [Azure ουρές και και Azure Service Bus--σε σύγκριση και υπενθύμισης και χρωματικές](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###<a name="fault-detection-and-retry-logic"></a>Εντοπισμός και προσπαθήστε ξανά λογικής σφαλμάτων

Ένα σημαντικό σημείο στη σχεδίαση ιδιαίτερα διαθέσιμη εφαρμογή είναι να χρησιμοποιήσετε "Επανάληψη" λογικής σε κώδικα ομαλά χειρισμού μια υπηρεσία που είναι προσωρινά προς τα κάτω. Το [Μεταβατικές σφαλμάτων χειρισμός μπλοκ εφαρμογής](https://msdn.microsoft.com/library/hh680934.aspx), που αναπτύχθηκε από την ομάδα Microsoft μοτίβων και πρακτικές, βοηθά τους προγραμματιστές εφαρμογών σε αυτήν τη διαδικασία. Η λέξη "μεταβατική" σημαίνει ότι ένα προσωρινό συνθήκη που διαρκεί μόνο για μια σχετικά σύντομο χρονικό διάστημα. Στο πλαίσιο αυτού του άρθρου, χειρισμός μεταβατικές αποτυχίες αποτελεί τμήμα ανάπτυξη μιας εφαρμογής ιδιαίτερα διαθέσιμη. Παραδείγματα μεταβατικές συνθήκες σφάλματα περιστασιακό δικτύου και χαθούν συνδέσεις βάσεων δεδομένων.

Το μεταβατικές σφαλμάτων χειρισμός εφαρμογή μπλοκ είναι μια απλοποιημένη τρόπο με τον οποίο μπορείτε να χειριστείτε αποτυχίες στον κωδικό σας σε φυσιολογική. Μπορείτε να το χρησιμοποιήσετε για να βελτιώσετε τη διαθεσιμότητα των εφαρμογών σας, προσθέτοντας έναν ισχυρό μεταβατικές λογικής χειρισμού σφαλμάτων. Στις περισσότερες περιπτώσεις, "Επανάληψη" λογική χειρίζεται τη σύντομη διακοπή και συνδέεται ξανά τον αποστολέα και παραλήπτη μετά από ένα ή περισσότερα αποτυχημένων προσπαθειών. Μια προσπάθεια επιτυχής "Επανάληψη" συνήθως Μετάβαση απαρατήρητη τους χρήστες της εφαρμογής.

Προγραμματιστές έχετε τρεις επιλογές για τη Διαχείριση τους λογικής "Επανάληψη": αυξανόμενες, σταθερό διάστημα, και εκθετική. Τμηματική αναμονή πλέον πριν από κάθε επανάληψης με αυξανόμενη γραμμικής τρόπο (για παράδειγμα, 1, 2, 3 και 4 δευτερόλεπτα). Σταθερό διάστημα χρειάζεται να περιμένει το ίδιο χρονικό διάστημα μεταξύ κάθε "Επανάληψη" (για παράδειγμα, 2 δευτερόλεπτα). Για μια πιο τυχαία επιλογή, η εκθετική πίσω-απενεργοποίηση χρειάζεται να περιμένει πλέον μεταξύ των επαναλήψεων. Ωστόσο, αυτό χρησιμοποιεί εκθετική συμπεριφορά (για παράδειγμα, 2, 4, 8 και 16 δευτερόλεπτα).

Η υψηλού επιπέδου στρατηγική στον κωδικό σας είναι:

1. Ορισμός του στρατηγική "Επανάληψη" και την πολιτική.
1. Δοκιμάστε τη λειτουργία που μπορεί να έχει ως αποτέλεσμα ένα σφάλμα μεταβατικές.
1. Εάν παρουσιαστεί σφάλμα μεταβατικές, καλέστε την πολιτική "Επανάληψη".
1. Εάν αποτύχει όλες τις επαναλήψεις, αντιμετωπίσει μια τελική εξαίρεση.

Δοκιμή λογικής "Επανάληψη" σε προσομοιωμένη αποτυχίες για να βεβαιωθείτε ότι επαναλήψεις σε διαδοχικές λειτουργίες δεν έχει ως αποτέλεσμα ένα απροσδόκητες μακρά καθυστέρηση. Κάντε αυτό πριν να αποφασίσετε να αποτύχει η συνολική εργασία.

###<a name="reference-data-pattern-for-high-availability"></a>Μοτίβο δεδομένων αναφοράς για υψηλή διαθεσιμότητα

Δεδομένα αναφοράς είναι τα δεδομένα μόνο για ανάγνωση μιας εφαρμογής του. Αυτά τα δεδομένα παρέχει το περιβάλλον επιχειρήσεις εντός των οποίων η εφαρμογή παράγει συναλλαγών δεδομένων κατά τη διάρκεια μιας λειτουργίας επιχειρήσεις. Συναλλαγές δεδομένων είναι μια συνάρτηση σε δεδομένη χρονική στιγμή τα δεδομένα αναφοράς. Επομένως, την ακεραιότητα εξαρτάται από το στιγμιότυπο των δεδομένων αναφοράς την ώρα της κίνησης. Αυτός είναι ένας ορισμός κάπως ελεύθερη, αλλά αυτό θα πρέπει να αρκεί για το σκοπό εδώ.

Δεδομένα αναφοράς στο περιβάλλον μιας εφαρμογής του είναι απαραίτητο για τη λειτουργία της εφαρμογής. Οι αντίστοιχες εφαρμογές δημιουργούν και συντηρούν δεδομένων αναφοράς. κύρια δεδομένα συστημάτων διαχείρισης (MDM) εκτέλεση συχνά αυτήν τη συνάρτηση. Αυτά τα συστήματα είναι υπεύθυνα για τον κύκλο ζωής των δεδομένων αναφοράς. Δεδομένα αναφοράς παραδείγματα κατάλογο προϊόντων, υπόδειγμα υπάλληλο, τμήματα υπόδειγμα και υπόδειγμα εξοπλισμού. Δεδομένα αναφοράς να προέρχονται από εκτός του οργανισμού, για παράδειγμα, ταχυδρομικούς κώδικες ή φόρο χρεώσεων. Στρατηγικές για την αύξηση της τη διαθεσιμότητα των δεδομένων αναφοράς είναι συνήθως λιγότερο δύσκολη από αυτά για τα δεδομένα συναλλαγών. Τα δεδομένα αναφοράς περιέχει το πλεονέκτημα κυρίως ότι αμετάβλητες.

Μπορείτε να κάνετε Azure web και εργαζόμενου τους ρόλους που εκμετάλλευση αυτόνομα κατά το χρόνο εκτέλεσης δεδομένων αναφοράς με την ανάπτυξη τα δεδομένα αναφοράς μαζί με την εφαρμογή. Εάν το μέγεθος του τοπικού χώρου αποθήκευσης επιτρέπει μια τέτοια ανάπτυξη, αυτή είναι μια ιδανική κατάσταση. Ενσωματωμένο βάσεων δεδομένων (SQL, NoSQL) ή αρχεία XML αναπτυχθεί σε ένα τοπικό σύστημα αρχείων θα σας βοηθήσει με την αυτονομία μονάδες κλίμακας Azure υπολογισμού. Ωστόσο, θα πρέπει να έχετε ένα μηχανισμό για την ενημέρωση των δεδομένων σε κάθε ρόλο χωρίς να απαιτείται επανάληψη ανάπτυξης. Για να το κάνετε αυτό, τοποθετήστε τις ενημερωμένες εκδόσεις για τα δεδομένα αναφοράς σε ένα τελικό σημείο χώρο αποθήκευσης στο cloud (για παράδειγμα, χώρος αποθήκευσης αντικειμένων Blob του Azure ή βάση δεδομένων SQL). Προσθήκη κώδικα σε κάθε ρόλο που κάνουν λήψη των ενημερώσεων δεδομένων σε κόμβους υπολογιστικών κατά την εκκίνηση του ρόλου. Εναλλακτικά, μπορείτε να προσθέσετε κώδικα που επιτρέπει στο διαχειριστή για να εκτελέσετε μια εξαναγκασμένη λήψης σε παρουσίες του ρόλου.

Για να αυξήσετε τη διαθεσιμότητα, τους ρόλους θα πρέπει να περιέχει ένα σύνολο δεδομένων αναφοράς σε περίπτωση που ο χώρος αποθήκευσης είναι προς τα κάτω. Αυτή η δυνατότητα επιτρέπει τους ρόλους για να ξεκινήσετε με ένα βασικό σύνολο δεδομένων αναφοράς μέχρι ο πόρος χώρου αποθήκευσης θα είναι διαθέσιμο για τις ενημερώσεις.

![Εφαρμογή υψηλή διαθεσιμότητα έως κόμβους αυτόνομα υπολογιστικών](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Μία εξέταση για αυτό το μοτίβο είναι της ανάπτυξης και της ταχύτητας εκκίνησης για ρόλους σας. Εάν για την ανάπτυξη ή τη λήψη μεγάλες ποσότητες δεδομένων αναφοράς κατά την εκκίνηση, αυτό να αυξήσετε το χρονικό διάστημα που χρειάζεται για την αυξομείωσης του νέου αναπτύξεις ή παρουσίες ρόλο. Αυτό μπορεί να είναι μια αποδεκτή ανταλλαγή για την αυτονομία αντιμετωπίζετε τα δεδομένα αναφοράς αμέσως διαθέσιμες σε κάθε ρόλο και όχι ανάλογα με τις υπηρεσίες εξωτερικού χώρου αποθήκευσης.

###<a name="transactional-data-pattern-for-high-availability"></a>Μοτίβο συναλλαγών δεδομένων για υψηλή διαθεσιμότητα

Συναλλαγές δεδομένων είναι τα δεδομένα που δημιουργείται από την εφαρμογή σε ένα περιβάλλον επιχειρήσεις. Συναλλαγές δεδομένων είναι ένας συνδυασμός από το σύνολο των επιχειρηματικών διαδικασιών που υλοποιεί την εφαρμογή και τα δεδομένα αναφοράς που υποστηρίζει αυτές τις διαδικασίες. Παραδείγματα συναλλαγών δεδομένων μπορεί να περιλαμβάνουν παραγγελίες, ειδοποιήσεις για προχωρημένους αποστολής, τιμολόγια και ευκαιρίες Διαχείριση (CRM) σχέση πελάτη. Τα δεδομένα συναλλαγών, επομένως, που δημιουργούνται θα χορηγηθεί στα εξωτερικά συστήματα για διατήρηση εγγραφών ή για περαιτέρω επεξεργασία.

Να θυμάστε ότι αυτή η αναφορά να αλλάξετε δεδομένων μέσα σε τα συστήματα που είναι υπεύθυνος για αυτά τα δεδομένα. Για αυτόν το λόγο, συναλλαγών δεδομένων πρέπει να αποθηκεύσετε το περιβάλλον δεδομένων αναφοράς σε δεδομένη χρονική στιγμή έτσι ώστε να έχει το ελάχιστο εξωτερικές εξαρτήσεις για τη συνέπεια σημασιολογίας. Για παράδειγμα, μπορείτε να την κατάργηση ενός προϊόντος από τον κατάλογο μερικά μηνών μετά μια σειρά. Η βέλτιστη πρακτική είναι να ενσωματώσετε όσο περιβάλλον δεδομένων αναφοράς ως είναι εφικτό σε συναλλαγή. Έτσι διατηρείται η σημασιολογία που σχετίζεται με τη συναλλαγή, ακόμα και αν αλλάξουν τα δεδομένα αναφοράς μετά καταγράφεται η συναλλαγή.

Όπως προαναφέρθηκε, αρχιτεκτονικές που χρησιμοποιούν ελεύθερη ζεύξης και ασύγχρονη επικοινωνία ο δανεισμός τον εαυτό τους σε υψηλότερα επίπεδα διαθεσιμότητας. Αυτό ισχύει για συναλλαγών καθώς και τα δεδομένα, αλλά η εφαρμογή είναι πιο σύνθετες. Παραδοσιακή συναλλαγών ιδέες συνήθως εξαρτώνται από τη βάση δεδομένων για τη διασφάλιση της συναλλαγής. Όταν Παρουσιάστε επίπεδα ενδιάμεση, τον κώδικα της εφαρμογής πρέπει να χειρίζεται σωστά τα δεδομένα σε διάφορα επίπεδα για να βεβαιωθείτε ότι επαρκή συνέπειας και διάρκεια ζωής.

Η παρακάτω ακολουθία περιγράφει μια ροή εργασίας που διαχωρίζει την καταγραφή συναλλαγών δεδομένων από την επεξεργασία:

1. Κόμβος Web: παρουσίαση αναφέρονται σε δεδομένα.
1. Εξωτερική αποθήκευση: Αποθήκευση ενδιάμεσου συναλλαγών δεδομένων.
1. Κόμβος Web: Ολοκληρώστε τη συναλλαγή τελικού χρήστη.
1. Κόμβος Web: αποστολή των ολοκληρωμένων συναλλαγών δεδομένων, μαζί με το περιβάλλον δεδομένων αναφοράς, σε προσωρινή αποθήκευση διαρκή που είναι εγγυημένη για να δώσετε μια απόκριση προβλέψιμα.
1. Κόμβος Web: σήματος την ολοκλήρωση τελικού χρήστη της κίνησης.
1. Φόντο, τον υπολογισμό κόμβος: εξαγωγή των συναλλαγών δεδομένων, των επεξεργασίας του εγγράφου εάν είναι απαραίτητο και στείλτε το στη θέση αποθήκευσής τελικό στο τρέχον σύστημα.

Το παρακάτω διάγραμμα παρουσιάζει μία πιθανή εφαρμογή της αυτήν τη σχεδίαση σε μια υπηρεσία cloud που φιλοξενείται στο Azure.

![Υψηλή διαθεσιμότητα έως ελεύθερη ζεύξης](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Τα βέλη διακεκομμένη στο παραπάνω διάγραμμα υποδεικνύουν ασύγχρονης επεξεργασίας. Ο ρόλος web προσκηνίου δεν γνωρίζει αυτό ασύγχρονης επεξεργασίας. Αυτό οδηγεί στο χώρο αποθήκευσης της κίνησης στον τελικό προορισμό σε σχέση με το τρέχον σύστημα. Λόγω του λανθάνων χρόνος που παρουσιάζει αυτό το μοντέλο ασύγχρονης, των συναλλαγών δεδομένων δεν είναι αμέσως διαθέσιμα για το ερώτημα. Επομένως, κάθε μονάδας των συναλλαγών δεδομένων πρέπει να αποθηκευτεί στο cache ή χρειάζεται μια περίοδο λειτουργίας χρήστη ώστε να ανταποκρίνεται την άμεση περιβάλλοντος εργασίας Χρήστη.

Ο ρόλος web είναι αυτόνομα από το υπόλοιπο της υποδομής του. Στο προφίλ του διαθεσιμότητα είναι ένας συνδυασμός από το ρόλο web και Azure ουρά και όχι ολόκληρο υποδομής. Εκτός από την υψηλή διαθεσιμότητα, αυτή η προσέγγιση επιτρέπει ο ρόλος web για να κλιμακωθεί οριζόντια, ανεξάρτητα από το χώρο αποθήκευσης παρασκηνίου. Αυτό το μοντέλο υψηλής διαθεσιμότητας μπορεί να επηρεάσουν στην τα οικονομικά δεδομένα των λειτουργιών. Πρόσθετα στοιχεία όπως Azure ουρές και τους ρόλους εργασίας μπορεί να επηρεάσει μηνιαίο κόστος χρήσης.

Σημειώστε ότι το προηγούμενο διάγραμμα εμφανίζει μία εφαρμογή αυτής της προσέγγισης οι αποσυνδεδεμένοι συναλλαγών δεδομένα. Υπάρχουν πολλές άλλες πιθανές υλοποιήσεις. Η παρακάτω λίστα παρέχει μερικές εναλλακτικές λύσεις:

 * Ένα ρόλο εργαζόμενου μπορεί να τοποθετηθεί μεταξύ του ρόλου web και η ουρά χώρου αποθήκευσης.
 * Μια υπηρεσία Bus ουρά μπορεί να χρησιμοποιηθεί αντί για ένα χώρο αποθήκευσης Azure ουρά.
 * Ο τελικός προορισμός μπορεί να είναι αποθήκευσης Azure ή μια υπηρεσία παροχής διαφορετική βάση δεδομένων.
 * Azure Cache μπορεί να χρησιμοποιηθεί στο επίπεδο του web για την παροχή τις απαιτήσεις άμεση σε cache μετά τη συναλλαγή.

###<a name="scalability-patterns"></a>Μοτίβα κλιμάκωση

Είναι σημαντικό να λάβετε υπόψη ότι η κλιμάκωση της υπηρεσίας cloud απευθείας επηρεάζει τη διαθεσιμότητα. Εάν αυξημένη φόρτωσης έχει ως αποτέλεσμα την υπηρεσία για να μην ανταποκρίνεται, την εντύπωση χρήστη είναι ότι η εφαρμογή είναι προς τα κάτω. Ακολουθήστε τις βέλτιστες πρακτικές για την κλιμάκωση με βάση το αναμενόμενο εφαρμογή φόρτωση και μελλοντικές προσδοκίες. Η υψηλότερη κλίμακα περιλαμβάνει πολλά ζητήματα, όπως τη χρήση μόνο έναντι πολλούς λογαριασμούς χώρου αποθήκευσης, κοινή χρήση σε πολλές βάσεις δεδομένων και σε cache στρατηγικές. Για μια λεπτομερή μέτρα σε αυτά τα μοτίβα, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές για τη σχεδίαση της μεγάλης κλίμακας υπηρεσίες στο Azure τις υπηρεσίες Cloud](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/).

##<a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο αποτελεί μέρος μιας σειράς από άρθρα εστιασμένη σε [Αποκατάσταση και υψηλής διαθεσιμότητας για εφαρμογές που δημιουργήθηκαν στο Microsoft Azure](./resiliency-disaster-recovery-high-availability-azure-applications.md). Το επόμενο άρθρο σε αυτήν τη σειρά είναι [Αποκατάσταση για εφαρμογές που δημιουργήθηκαν στο Microsoft Azure](./resiliency-disaster-recovery-azure-applications.md).
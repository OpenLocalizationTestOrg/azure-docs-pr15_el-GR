<properties
   pageTitle="Στο cloud λύσεων αποκατάστασης από καταστροφή - SQL αναπαραγωγή βάσης δεδομένων Active παν-| Microsoft Azure"
   description="Μάθετε πώς μπορείτε να σχεδιάσετε λύσεων αποκατάστασης από καταστροφή cloud για επιχειρήσεις συνέχειας σχεδιασμού χρησιμοποιώντας παν αναπαραγωγής για εφαρμογή δημιουργίας αντιγράφων ασφαλείας δεδομένων με βάση δεδομένων SQL Azure."
   keywords="στο cloud αποκατάσταση, λύσεων αποκατάστασης από καταστροφή, εφαρμογή δημιουργίας αντιγράφων ασφαλείας δεδομένων, η παν-αναπαραγωγή, σχεδιασμό συνέχειας επιχειρήσεις"
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="07/20/2016"
   ms.author="sashan"/>

# <a name="design-an-application-for-cloud-disaster-recovery-using-active-geo-replication-in-sql-database"></a>Σχεδιάστε μια εφαρμογή για αποκατάσταση cloud χρησιμοποιείται η ενεργή παν-αναπαραγωγή σε βάση δεδομένων SQL


> [AZURE.NOTE] [Ενεργό παν-αναπαραγωγής](sql-database-geo-replication-overview.md) είναι τώρα διαθέσιμη για όλες τις βάσεις δεδομένων σε όλα τα επίπεδα.



Μάθετε πώς να χρησιμοποιείτε [Ενεργό παν-αναπαραγωγή](sql-database-geo-replication-overview.md) σε βάση δεδομένων SQL για σχεδίαση εφαρμογές βάσεων δεδομένων που είναι ανθεκτικά Τοπικές αποτυχίες και καταστροφική διακοπές. Για επιχειρήσεις συνέχειας σχεδιασμό, εξετάστε το ενδεχόμενο να την τοπολογία εφαρμογής ανάπτυξης, επιπέδου σύμβαση παροχής υπηρεσιών στοχεύετε, κίνηση λανθάνων χρόνος και έξοδα. Σε αυτό το άρθρο θα σας εξετάσουμε τα κοινά μοτίβα εφαρμογής και συζητήσετε σχετικά με τα πλεονεκτήματα και τα ανταλλάγματα κάθε επιλογής. Για πληροφορίες σχετικά με το ενεργό παν-αλληλεπίδραση με τους χώρους συγκέντρωσης ελαστικά, ανατρέξτε στο θέμα [στρατηγικές αποκατάστασης από καταστροφή ελαστικότητας χώρου συγκέντρωσης](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Σχεδίαση μοτίβο 1: ενεργό παθητικές ανάπτυξης για αποκατάσταση cloud με μια βάση δεδομένων βρίσκεται από κοινού

Αυτή η επιλογή είναι καταλληλότερο για τις εφαρμογές με τα εξής χαρακτηριστικά:

+ Ενεργή παρουσία σε μία μόνο περιοχή Azure
+ Ισχυρός εξάρτηση στην ανάγνωση και εγγραφή (RW) πρόσβαση στα δεδομένα
+ Περιοχή σταυρό συνδεσιμότητα μεταξύ της λογικής της εφαρμογής και τη βάση δεδομένων δεν είναι αποδεκτή λόγω λανθάνοντα χρόνο και την κυκλοφορία του κόστους    

Σε αυτήν την περίπτωση, η τοπολογία εφαρμογής ανάπτυξης είναι βελτιστοποιημένη για το χειρισμό τοπικές καταστροφές όταν όλα τα στοιχεία εφαρμογής επηρεάζονται και πρέπει να ανακατεύθυνσης ως μονάδα. Για γεωγραφική πλεονασμού, η λογική της εφαρμογής και τη βάση δεδομένων αναπαράγονται σε μια άλλη περιοχή αλλά δεν χρησιμοποιούνται για την εφαρμογή φόρτο εργασίας την κανονική συνθήκες. Η εφαρμογή στη δευτερεύουσα περιοχή πρέπει να ρυθμιστεί για να χρησιμοποιήσετε μια συμβολοσειρά σύνδεσης SQL στη δευτερεύουσα βάση δεδομένων. Κίνηση manager έχει ρυθμιστεί ώστε να χρησιμοποιήσετε [τη μέθοδο δρομολόγησης ανακατεύθυνσης](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [AZURE.NOTE] [Διαχείριση Azure κίνηση](../traffic-manager/traffic-manager-overview.md) χρησιμοποιείται σε αυτό το άρθρο για εικόνα μόνο. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε λύση εξισορρόπησης φόρτου που υποστηρίζει τη μέθοδο δρομολόγησης ανακατεύθυνσης.    

Εκτός από τις παρουσίες κύρια εφαρμογή, πρέπει να λάβετε υπόψη μια μικρή [εφαρμογή ρόλο εργαζόμενου](cloud-services-choose-me.md#tellmecs) που παρακολουθεί την κύρια βάση δεδομένων με την έκδοση περιοδικό T-μόνο για ανάγνωση (RO) εντολών SQL για την ανάπτυξη. Μπορείτε να το χρησιμοποιήσετε για την αυτόματη ενεργοποίηση ανακατεύθυνσης, για να δημιουργήσετε μια ειδοποίηση στην κονσόλα διαχείρισης της εφαρμογής σας ή για να κάνετε και τα δύο. Για να εξασφαλίσετε ότι παρακολούθησης δεν επηρεάζεται από ολόκληρη την περιοχή ΡΕΥΜΑΤΟΣ, πρέπει να αναπτύξετε το παρουσιών της εφαρμογής παρακολούθησης σε κάθε περιοχή και έχετε κάθε παρουσία σύνδεση με την κύρια βάση δεδομένων στην περιοχή κύρια και δευτερεύουσα βάση δεδομένων στην περιοχή τοπική. 

> [AZURE.NOTE] Τόσο παρακολούθηση εφαρμογές θα πρέπει να είναι ενεργό και Διερεύνηση κύριων και δευτερευουσών βάσεις δεδομένων. Η τελευταία μπορεί να χρησιμοποιηθεί για τον εντοπισμό αποτυχία στη δευτερεύουσα περιοχή και ειδοποίηση όταν η εφαρμογή δεν είναι προστατευμένο.     

Το παρακάτω διάγραμμα παρουσιάζει αυτήν τη ρύθμιση παραμέτρων πριν από μια διακοπή ρεύματος.

![Ρύθμιση παραμέτρων παν-αναπαραγωγή βάσης δεδομένων SQL. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Μετά από μια μη διαθεσιμότητα στην κύρια περιοχή, η εφαρμογή παρακολούθησης εντοπίζει ότι την κύρια βάση δεδομένων δεν είναι δυνατή η πρόσβαση και καταχωρεί ειδοποίησης. Ανάλογα με την εφαρμογή SLA, μπορείτε να αποφασίσετε πόσες διαδοχικές παρακολούθησης καθετήρες πρέπει να αποτύχει πριν δηλώνει μια μη διαθεσιμότητα βάσης δεδομένων. Για να επιτύχετε συντονισμένη ανακατεύθυνσης από το τελικό σημείο της εφαρμογής και τη βάση δεδομένων, θα πρέπει να έχετε την εφαρμογή παρακολούθησης, ακολουθήστε τα παρακάτω βήματα:

1. [Ενημερώστε την κατάσταση του πρωτεύοντος τελικό σημείο](https://msdn.microsoft.com/library/hh758250.aspx) για να ενεργοποιήσετε ανακατεύθυνσης τελικό σημείο.
2. Καλέστε το δευτερεύον βάσης δεδομένων για να [ξεκινήσετε ανακατεύθυνσης βάσης δεδομένων](sql-database-geo-replication-portal.md).

Μετά την ανακατεύθυνση, η εφαρμογή θα επεξεργάζεται τις αιτήσεις χρήστη στη δευτερεύουσα περιοχή αλλά θα παραμείνει βρίσκονται από κοινού με τη βάση δεδομένων, επειδή η κύρια βάση δεδομένων θα είναι πλέον στη δευτερεύουσα περιοχή. Αυτό το σενάριο απεικονίζεται από το επόμενο διάγραμμα. Σε όλα τα διαγράμματα, συμπαγείς γραμμές υποδεικνύουν ενεργές συνδέσεις, διακεκομμένες γραμμές υποδεικνύουν συνδέσεις σε αναστολή και σύμβολα διακοπή υποδεικνύουν εναύσματα ενέργεια.


![Παν-αναπαραγωγή: Ανακατεύθυνση δευτερεύοντα βάση δεδομένων. Δημιουργία αντιγράφων ασφαλείας δεδομένων εφαρμογής.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Εάν μια μη διαθεσιμότητα συμβαίνει στη δευτερεύουσα περιοχή, έχει ανασταλεί τη σύνδεση αλληλεπίδραση μεταξύ των κύριων σελίδων και δευτερεύοντα βάσης δεδομένων και της εφαρμογής παρακολούθησης καταχωρεί μια ειδοποίηση ότι εκτίθεται την κύρια βάση δεδομένων. Δεν επηρεάζεται η απόδοση της εφαρμογής, αλλά η εφαρμογή λειτουργεί εκτεθειμένης και επομένως στο μεγαλύτερο κίνδυνο σε περίπτωση δύο περιοχές αποτύχει διαδοχικά.

> [AZURE.NOTE] Συνιστάται μόνο ρυθμίσεις παραμέτρων ανάπτυξης με μία μόνο περιοχή DR. Αυτό συμβαίνει επειδή οι περισσότεροι από το Azure geographies έχουν δύο περιοχές. Αυτές οι ρυθμίσεις παραμέτρων δεν θα Προστατεύστε την εφαρμογή σας καταστροφική αστοχία του δύο περιοχές. Σε ένα πιθανό συμβάν όπως αποτυχία, μπορείτε να ανακτήσετε τις βάσεις δεδομένων σε μια περιοχή τρίτο χρησιμοποιώντας [λειτουργία παν επαναφοράς](sql-database-disaster-recovery.md#recovery-using-geo-restore).

Μόλις μετριάζεται τη μη διαθεσιμότητα, τη δευτερεύουσα βάση δεδομένων συγχρονίζεται αυτόματα με το πρωτεύον. Κατά το συγχρονισμό, οι επιδόσεις του των κύριων θα μπορούσε να είναι επηρεάζονται λίγο ανάλογα με την ποσότητα των δεδομένων που πρέπει να είναι δυνατός ο συγχρονισμός. Το παρακάτω διάγραμμα παρουσιάζει μια μη διαθεσιμότητα στη δευτερεύουσα περιοχή.

![Δευτερεύουσα βάση δεδομένων συγχρονιστεί με κύρια. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)


Τα βασικά **πλεονεκτήματα** των αυτό το μοτίβο σχεδίασης είναι οι εξής:

+ Η συμβολοσειρά σύνδεσης SQL έχει οριστεί κατά την ανάπτυξη εφαρμογών σε κάθε περιοχή και δεν αλλάζει μετά την ανακατεύθυνση.
+ Απόδοση της εφαρμογής δεν επηρεάζεται από ανακατεύθυνσης ως την εφαρμογή και τη βάση δεδομένων βρίσκονται πάντοτε από κοινού.

Το κύριο **ανταλλαγή** είναι ότι την παρουσία της εφαρμογής πλεονάζοντα στη δευτερεύουσα περιοχή χρησιμοποιείται μόνο για αποκατάσταση.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Σχεδίαση μοτίβο 2: ανάπτυξη ενεργούς για την εξισορρόπηση φόρτου εφαρμογής
Αυτή η επιλογή αποκατάστασης από καταστροφή cloud είναι καταλληλότερο για τις εφαρμογές με τα εξής χαρακτηριστικά:

+ Διαβάζει υψηλή αναλογία της βάσης δεδομένων σε εγγραφές
+ Λανθάνων χρόνος εγγραφής βάσης δεδομένων δεν επηρεάζει την εμπειρία τελικού χρήστη  
+ Μόνο για ανάγνωση λογικής μπορεί να διαχωριστεί από λογικής ανάγνωση και εγγραφή χρησιμοποιώντας μια συμβολοσειρά σύνδεσης διαφορετικό
+ Μόνο για ανάγνωση λογικής δεν εξαρτάται από δεδομένα που συγχρονίζονται πλήρως με τις πιο πρόσφατες ενημερώσεις  

Εάν οι εφαρμογές σας έχουν αυτά τα χαρακτηριστικά, φόρτωση εξισορρόπηση του τελικού χρήστη συνδέσεις μεταξύ πολλών παρουσιών της εφαρμογής σε διαφορετικές περιοχές να βελτιώσετε επιδόσεις και την εμπειρία τελικού χρήστη. Για να υλοποιήσετε εξισορρόπησης φόρτου, κάθε περιοχή θα πρέπει να έχετε μια ενεργή παρουσία της εφαρμογής με τη λογική (RW) ανάγνωση και εγγραφή συνδεδεμένο με την κύρια βάση δεδομένων στην κύρια περιοχή. Η λογική (RO) μόνο για ανάγνωση πρέπει να είναι συνδεδεμένες με δευτερεύοντα βάση δεδομένων στην ίδια περιοχή ως την παρουσία της εφαρμογής. Κίνηση manager θα πρέπει να ρυθμιστεί για να χρησιμοποιήσετε [τη δρομολόγηση round robin](../traffic-manager/traffic-manager-configure-round-robin-routing-method.md) ή [δρομολόγησης επιδόσεων](../traffic-manager/traffic-manager-configure-performance-routing-method.md) με [Παρακολούθηση τελικό σημείο](../traffic-manager/traffic-manager-monitoring.md) ενεργοποιηθεί για κάθε παρουσία της εφαρμογής.

Όπως στο μοτίβο #1, θα πρέπει να την ανάπτυξη μιας εφαρμογής παρόμοια παρακολούθησης. Αλλά, σε αντίθεση με μοτίβο #1, η εφαρμογή παρακολούθησης δεν θα είναι υπεύθυνος για την ενεργοποίηση του εφεδρικού τελικό σημείο.

> [AZURE.NOTE] Ενώ αυτό το μοτίβο χρησιμοποιεί περισσότερες από μία δευτερεύουσα βάσης δεδομένων, μόνο ένα από τα secondaries θα χρησιμοποιηθούν για ανακατεύθυνσης για τους λόγους σημειωθεί νωρίτερα. Επειδή αυτό το μοτίβο απαιτεί πρόσβαση μόνο για ανάγνωση στο δευτερεύον, απαιτεί ενεργό παν-αναπαραγωγής.

Διαχείριση κυκλοφορίας πρέπει να ρυθμιστεί για απόδοση δρομολόγησης για να κατευθύνετε τις συνδέσεις χρηστών με την παρουσία της εφαρμογής που βρίσκεται πιο κοντά γεωγραφική θέση του χρήστη. Το παρακάτω διάγραμμα παρουσιάζει αυτήν τη ρύθμιση παραμέτρων πριν από μια διακοπή ρεύματος.
![Χωρίς διακοπή ρεύματος: επιδόσεων δρομολόγηση σε πλησιέστερο εφαρμογής. Παν-αναπαραγωγής.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Σε περίπτωση που εντοπιστεί μια μη διαθεσιμότητα βάσης δεδομένων στην κύρια περιοχή, πραγματοποιείτε ανακατεύθυνσης από την κύρια βάση δεδομένων σε μία από τις δευτερεύουσες περιοχές, αλλάζοντας τη θέση από την κύρια βάση δεδομένων. Ο διευθυντής κίνηση αυτόματα αποκλείει το τελικό σημείο για εργασία χωρίς σύνδεση από τον πίνακα δρομολόγησης, αλλά συνεχίζει δρομολόγηση την κυκλοφορία του τελικού χρήστη στις υπόλοιπες παρουσίες online. Επειδή η κύρια βάση δεδομένων είναι τώρα σε διαφορετική περιοχή, όλες τις εμφανίσεις online πρέπει να αλλάξετε τους συμβολοσειρά σύνδεσης του SQL για ανάγνωση-εγγραφή για να συνδεθείτε με τη νέα κύρια. Είναι σημαντικό να πάρετε αυτή η αλλαγή πριν από την έναρξη του εφεδρικού βάσης δεδομένων. Συμβολοσειρές σύνδεσης SQL μόνο για ανάγνωση θα πρέπει να διατηρήσετε αναλλοίωτο όπως οδηγεί πάντα στη βάση δεδομένων στην ίδια περιοχή. Τα βήματα ανακατεύθυνσης είναι:  

1. Αλλαγή συμβολοσειρές σύνδεσης SQL ανάγνωση και εγγραφή στην οποία θα οδηγεί η νέα κύρια.
2. Κλήση της βάσης δεδομένων που έχει οριστεί ως δευτερεύουσα για την [Προετοιμασία βάσης δεδομένων ανακατεύθυνσης](sql-database-geo-replication-portal.md).

Το παρακάτω διάγραμμα παρουσιάζει τη νέα ρύθμιση παραμέτρων μετά την ανακατεύθυνση.
![Ρύθμιση παραμέτρων μετά την ανακατεύθυνση. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Σε περίπτωση μια μη διαθεσιμότητα σε μία από τις δευτερεύουσες περιοχές, τη Διαχείριση κίνηση καταργεί αυτόματα το τελικό σημείο για εργασία χωρίς σύνδεση σε αυτήν την περιοχή από τον πίνακα δρομολόγησης. Το κανάλι αναπαραγωγής στη δευτερεύουσα βάση δεδομένων σε αυτήν την περιοχή έχει ανασταλεί. Επειδή στις υπόλοιπες περιοχές λάβετε επιπλέον χρήστη κίνηση σε αυτό το σενάριο, η εφαρμογή επιδόσεις επηρεάζονται κατά τη διακοπή ρεύματος. Μόλις μετριάζεται τη μη διαθεσιμότητα, τη δευτερεύουσα βάση δεδομένων στην επηρεαζόμενη περιοχή αμέσως είναι συγχρονισμένο με το πρωτεύον. Κατά το συγχρονισμό απόδοσης της κύριας ενδέχεται να ελαφρώς αντιμετωπίσουν προβλήματα ανάλογα με την ποσότητα των δεδομένων που πρέπει να είναι δυνατός ο συγχρονισμός. Το παρακάτω διάγραμμα παρουσιάζει μια μη διαθεσιμότητα σε μία από τις δευτερεύουσες περιοχές.

![Μη διαθεσιμότητα στη δευτερεύουσα περιοχή. Στο cloud αποκατάσταση - παν αναπαραγωγής.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

Το βασικό **πλεονέκτημα** της αυτό το μοτίβο σχεδίασης είναι ότι μπορείτε να κλιμάκωση το φόρτο εργασίας εφαρμογής σε πολλές secondaries για να επιτύχετε την απόδοση βέλτιστη τελικού χρήστη. Των **ισορροπιών** αυτής της επιλογής είναι οι εξής:

+ Ανάγνωση και εγγραφή συνδέσεων μεταξύ των παρουσιών της εφαρμογής και τη βάση δεδομένων είναι διαφορετικές λανθάνων χρόνος και κόστος
+ Εφαρμογή επιδόσεις επηρεάζονται κατά τη διάρκεια του μη διαθεσιμότητα
+ Απαιτούνται παρουσιών της εφαρμογής για να αλλάξετε δυναμικά τη συμβολοσειρά σύνδεσης SQL μετά την ανακατεύθυνση βάσης δεδομένων.  

> [AZURE.NOTE] Μια παρόμοια προσέγγιση μπορεί να χρησιμοποιηθεί για μείωση φόρτου εξειδικευμένες φόρτους εργασίας όπως αναφοράς εργασίες, εργαλεία επιχειρηματικής ευφυΐας ή δημιουργία αντιγράφων ασφαλείας. Συνήθως αυτά τα φόρτους εργασίας εκμετάλλευση πόρους σημαντική βάσεων δεδομένων, επομένως, συνιστάται να που ορίζετε ένα από τα δευτερεύοντα βάσεις δεδομένων τους με το επίπεδο απόδοσης που ταιριάζουν με τα αναμενόμενα φόρτο εργασίας.

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Σχεδίαση μοτίβο 3: ενεργό παθητικές ανάπτυξης για διατήρηση δεδομένων  
Αυτή η επιλογή είναι καταλληλότερο για τις εφαρμογές με τα εξής χαρακτηριστικά:

+ Οποιεσδήποτε απώλειες δεδομένων είναι επιχειρήσεις υψηλό κίνδυνο. Η ανακατεύθυνση βάσης δεδομένων μπορεί να χρησιμοποιηθεί μόνο ως τελευταία λύση, εάν η μη διαθεσιμότητα είναι μόνιμο.
+ Η εφαρμογή μπορεί να λειτουργήσει σε "λειτουργία μόνο για ανάγνωση" για μια χρονική περίοδο.

Σε αυτό το μοτίβο, η εφαρμογή αλλάζει σε λειτουργία μόνο για ανάγνωση όταν είστε συνδεδεμένοι στη βάση δεδομένων του δευτερεύοντα. Η λογική εφαρμογής στην κύρια περιοχή βρίσκεται από κοινού με την κύρια βάση δεδομένων και λειτουργεί σε κατάσταση λειτουργίας ανάγνωσης / εγγραφής (RW). Η λογική εφαρμογών στη δευτερεύουσα περιοχή βρίσκεται από κοινού με τη δευτερεύουσα βάση δεδομένων και είστε έτοιμοι να λειτουργεί σε λειτουργία μόνο για ανάγνωση (RO).  Κίνηση manager θα πρέπει να ρυθμιστεί για να χρησιμοποιεί [δρομολόγηση ανακατεύθυνσης](../traffic-manager/traffic-manager-configure-failover-routing-method.md) με [Παρακολούθηση τελικό σημείο](../traffic-manager/traffic-manager-monitoring.md) ενεργοποιηθεί για δύο παρουσιών της εφαρμογής.

Το παρακάτω διάγραμμα παρουσιάζει αυτήν τη ρύθμιση παραμέτρων πριν από μια διακοπή ρεύματος.
![Ανάπτυξη ενεργό παθητικές πριν από την ανακατεύθυνση. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Όταν ο διευθυντής κίνηση εντοπίσει αποτυχία σύνδεσης στην κύρια περιοχή, μεταβαίνει αυτόματα την κυκλοφορία χρήστη για να την παρουσία της εφαρμογής στη δευτερεύουσα περιοχή. Με αυτό το μοτίβο, είναι σημαντικό που μπορείτε να **κάνετε δεν** προετοιμασία βάσης δεδομένων ανακατεύθυνσης μετά τη μη διαθεσιμότητα εντοπίζεται. Η εφαρμογή στη δευτερεύουσα περιοχή έχει ενεργοποιηθεί και λειτουργεί σε λειτουργία μόνο για ανάγνωση, χρησιμοποιώντας τη δευτερεύουσα βάση δεδομένων. Απεικονίζεται με το ακόλουθο διάγραμμα.

![Μη διαθεσιμότητα: Εφαρμογή σε λειτουργία μόνο για ανάγνωση. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Μόλις μετριάζεται τη μη διαθεσιμότητα στην κύρια περιοχή, Διαχείριση κίνηση εντοπίσει την επαναφορά της συνδεσιμότητας στην κύρια περιοχή και αλλάζει την κυκλοφορία χρήστη ξανά για να την παρουσία της εφαρμογής στην κύρια περιοχή. Παρουσία της εφαρμογής που ισχύει και λειτουργεί σε κατάσταση λειτουργίας ανάγνωσης-εγγραφής που χρησιμοποιεί την κύρια βάση δεδομένων.

> [AZURE.NOTE] Επειδή αυτό το μοτίβο απαιτεί πρόσβαση μόνο για ανάγνωση στο δευτερεύον απαιτεί ενεργό παν-αναπαραγωγής.

Σε περίπτωση μια μη διαθεσιμότητα στη δευτερεύουσα περιοχή, η διαχείριση κίνηση επισημαίνει το τελικό σημείο εφαρμογής στην κύρια περιοχή ως υποβαθμισμένο και το κανάλι αναπαραγωγής έχει ανασταλεί. Ωστόσο, αυτή μη διαθεσιμότητα δεν επηρεάζει την εφαρμογή απόδοσης κατά τη διάρκεια του μη διαθεσιμότητα. Μόλις μετριάζεται τη μη διαθεσιμότητα, τη δευτερεύουσα βάση δεδομένων αμέσως είναι συγχρονισμένο με το πρωτεύον. Κατά το συγχρονισμό απόδοσης της κύριας ενδέχεται να ελαφρώς αντιμετωπίσουν προβλήματα ανάλογα με την ποσότητα των δεδομένων που πρέπει να είναι δυνατός ο συγχρονισμός.

![Μη διαθεσιμότητα: Δευτερεύοντα βάση δεδομένων. Αποκατάσταση cloud.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Αυτό το μοτίβο σχεδίαση έχει αρκετά **πλεονεκτήματα**:

+ Το αποφεύγεται η απώλεια δεδομένων κατά το προσωρινό διακοπές.
+ Δεν απαιτεί να αναπτύξετε μια εφαρμογή παρακολούθησης κατά την ανάκτηση ενεργοποιείται από την κυκλοφορία manager.
+ Χρόνος εκτός λειτουργίας εξαρτάται μόνο πόσο γρήγορα την κυκλοφορία manager εντοπίσει την αποτυχία συνδεσιμότητας, το οποίο είναι δυνατό να ρυθμιστεί.

Των **ισορροπιών** είναι οι εξής:

+ Εφαρμογή πρέπει να είναι σε θέση να λειτουργούν στη λειτουργία μόνο για ανάγνωση.
+ Απαιτείται για την εναλλαγή δυναμικά σε αυτό, όταν είναι συνδεδεμένο σε μια βάση δεδομένων μόνο για ανάγνωση.
+ Η επανάληψη των λειτουργιών ανάγνωση και εγγραφή απαιτεί αποκατάστασης στην κύρια περιοχή, γεγονός που σημαίνει ότι η πρόσβαση σε δεδομένα πλήρους μπορεί να απενεργοποιηθεί για ώρες ή ακόμα και ημέρες.

> [AZURE.NOTE] Σε περίπτωση μια μη διαθεσιμότητα μόνιμο υπηρεσίας στην περιοχή, πρέπει να με μη αυτόματο τρόπο ενεργοποίηση ανακατεύθυνσης βάσης δεδομένων και αποδεχτείτε την απώλεια δεδομένων. Η εφαρμογή θα λειτουργούν στη δευτερεύουσα περιοχή με πρόσβαση ανάγνωσης / εγγραφής στη βάση δεδομένων.

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Σχεδιασμός συνέχειας επιχειρήσεις: Επιλέξτε τη σχεδίαση εφαρμογών για το cloud αποκατάσταση

Της στρατηγικής αποκατάστασης από καταστροφή συγκεκριμένο σύννεφο μπορεί να συνδυάσετε ή επέκταση αυτά τα μοτίβα σχεδίαση ώστε να ανταποκρίνονται καλύτερα στις ανάγκες της εφαρμογής σας.  Όπως προαναφέρθηκε, τη στρατηγική επιλέξετε βασίζεται σε το SLA που θέλετε για την προσφορά για τους πελάτες σας και την τοπολογία εφαρμογής ανάπτυξης. Για να σας βοηθήσουν να λάβετε απόφασή σας, ο ακόλουθος πίνακας συγκρίνει τις επιλογές με βάση την απώλεια δεδομένων εκτιμώμενη ή ανάκτησης, τοποθετήστε το δείκτη στόχο (RPO) και εκτιμώμενη χρόνου ανάκτησης (εισαγωγή).

| Μοτίβο | RPO | ΕΙΣΑΓΩΓΉ
| :--- |:--- | :---
| Ενεργό παθητικές ανάπτυξης για αποκατάσταση πρόσβαση από κοινού βρίσκεται στη βάση δεδομένων | Ανάγνωση και εγγραφή πρόσβασης < 5 sec | Χρόνος ανίχνευσης αποτυχίας + API ανακατεύθυνση κλήσεων + δοκιμή επαλήθευσης εφαρμογής
| Ανάπτυξη ενεργούς για την εξισορρόπηση φόρτου εφαρμογής | Ανάγνωση και εγγραφή πρόσβασης < 5 sec | Χρόνος ανίχνευσης αποτυχίας + κλήση ανακατεύθυνσης API + συμβολοσειρά σύνδεσης SQL αλλαγή + δοκιμή επαλήθευσης εφαρμογής
| Ενεργό παθητικές ανάπτυξης για διατήρηση δεδομένων | Πρόσβαση μόνο για ανάγνωση < 5 sec πρόσβαση για ανάγνωση-εγγραφή = μηδέν | Πρόσβαση μόνο για ανάγνωση = χρόνος ανίχνευσης συνδεσιμότητας αποτυχίας + δοκιμή επαλήθευσης εφαρμογής <br>Ανάγνωση και εγγραφή access = χρόνος για τον περιορισμό του μη διαθεσιμότητα

## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)
- Για πληροφορίες σχετικά με το ενεργό παν-αλληλεπίδραση με τους χώρους συγκέντρωσης ελαστικά, ανατρέξτε στο θέμα [στρατηγικές αποκατάστασης από καταστροφή ελαστικότητας χώρου συγκέντρωσης](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
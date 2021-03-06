<properties 
   pageTitle="Βάση δεδομένων SQL Azure συνήθεις Ερωτήσεις" 
   description="Ρωτήστε απαντήσεις σε συνήθεις ερωτήσεις πελατών σχετικά με τις βάσεις δεδομένων cloud βάση δεδομένων SQL Azure, σύστημα διαχείρισης σχεσιακή βάση δεδομένων της Microsoft (RDBMS) και βάση δεδομένων ως υπηρεσία στο cloud." 
   services="sql-database" 
   documentationCenter="" 
   authors="CarlRabeler" 
   manager="jhubbard" 
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>Βάση δεδομένων SQL συνήθεις Ερωτήσεις

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Πώς η χρήση της βάσης δεδομένων SQL εμφανίζεται στο λογαριασμό μου; 
Λογαριασμοί βάση δεδομένων SQL σε ένα προβλέψιμα Ωριαία χρέωση που βασίζεται σε δύο το επίπεδο υπηρεσιών + επίπεδο επιδόσεων για μία μόνο βάσεις δεδομένων ή eDTUs ανά χώρου συγκέντρωσης ελαστικότητας βάση δεδομένων. Πραγματική χρήση έχει υπολογιστεί και αναλογική ανά ώρα, ώστε να τη χρέωσή σας ενδέχεται να εμφανίζουν κλάσματα της ώρας. Για παράδειγμα, εάν υπάρχει μια βάση δεδομένων για 12 ώρες σε έναν μήνα, τη χρέωσή σας δείχνει τη χρήση του 0,5 ημέρες. Επιπλέον, επίπεδα υπηρεσίας + επίπεδο επιδόσεων και eDTUs ανά χώρου συγκέντρωσης είναι ανάλυση στο τιμολόγιο ώστε να είναι πιο εύκολο να δείτε τον αριθμό των ημερών βάσης δεδομένων που χρησιμοποιούνται για κάθε σε ένα μεμονωμένο μήνα.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Τι γίνεται εάν μία βάση δεδομένων είναι ενεργή για λιγότερο από μία ώρα ή χρησιμοποιεί ένα ανώτερο επίπεδο υπηρεσιών για λιγότερο από μία ώρα;
Χρέωσης για κάθε ώρα υπάρχει μια βάση δεδομένων χρησιμοποιώντας το υψηλότερο επίπεδο υπηρεσιών + επίπεδο απόδοσης που εφαρμόζονται κατά τη συγκεκριμένη ώρα, ανεξάρτητα από τη χρήση ή αν η βάση δεδομένων ήταν ενεργό για λιγότερο από μία ώρα. Για παράδειγμα, αν δημιουργήσετε μία βάση δεδομένων και διαγράψτε το πέντε λεπτά αργότερα τη χρέωσή σας απεικονίζει μια χρέωση για την ώρα μία βάση δεδομένων. 

Παραδείγματα
    
- Εάν δημιουργήσετε μια βασική βάση δεδομένων και, στη συνέχεια, αμέσως την αναβάθμιση του σε τυπική S1, που θα χρεωθείτε με το S1 τυπική χρέωση για την πρώτη ώρα.

- Αν κάνετε αναβάθμιση μιας βάσης δεδομένων από Basic Premium στις 10:00 μ.μ. και να ολοκληρωθεί η αναβάθμιση στις 1:35 π.μ. την επόμενη ημέρα, θα χρεωθείτε με το ρυθμό Premium ξεκινώντας από το 1:00 π.μ. 

- Εάν μπορείτε να υποβιβάσετε μια βάση δεδομένων από Premium για βασικές 11:00 π.μ. και ότι ολοκληρώνεται στις 2:15 μ.μ. και, στη συνέχεια, η βάση δεδομένων θα χρεωθεί με το επιτόκιο Premium μέχρι 3:00 μμ, μετά από την οποία θα χρεωθεί με τις βασικές τιμές.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Πώς χρήση χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων εμφανίζεται στις χρέωσής μου και τι συμβαίνει όταν αλλάζω eDTUs ανά χώρο συγκέντρωσης;
Χρεώσεις χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων εμφανίζεται στο τιμολόγιο χρέωσης ως ελαστικά DTUs (eDTUs) σε τα διαστήματα εμφανίζεται στην περιοχή eDTUs ανά χώρου συγκέντρωσης στη [σελίδα τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/sql-database/). Δεν υπάρχει χωρίς χρέωση ανά βάση για χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων. Χρέωσης για κάθε ώρα υπάρχει ένα χώρο συγκέντρωσης κατά την υψηλότερη eDTU, ανεξάρτητα από τη χρήση ή εάν το χώρο συγκέντρωσης ήταν ενεργό για λιγότερο από μία ώρα. 

Παραδείγματα

- Εάν δημιουργήσετε ένα χώρο συγκέντρωσης τυπική ελαστικότητας βάσης δεδομένων με 200 eDTUs 11:18 π.μ., προσθέτοντας πέντε βάσεις δεδομένων στο χώρο συγκέντρωσης, που θα χρεωθείτε για 200 eDTUs για ολόκληρη την ώρα, ξεκινώντας από το 11 π.μ. έως το υπόλοιπο της ημέρας.
- Στην ημέρα 2, στις 5:05 π.μ., βάση δεδομένων 1 αρχίζει κατανάλωση 50 eDTUs και διατηρεί σταθερή έως την ημέρα. Βάσεις δεδομένων 2-5 παρουσιάζει διακυμάνσεις μεταξύ 0 και 80 eDTUs. Στη διάρκεια της ημέρας, μπορείτε να προσθέσετε πέντε άλλες βάσεις δεδομένων που εκμετάλλευση διαφορετικές eDTUs όλη την ημέρα. 2 ημέρα είναι μια πλήρη ημέρα χρεώνονται στο 200 eDTU. 
- Ημέρα 3, στις 5 π.μ. Μπορείτε να προσθέσετε ένα άλλο 15 βάσεις δεδομένων. Χρήση της βάσης δεδομένων αυξάνεται όλη την ημέρα στο σημείο όπου αποφασίσετε να αυξήσετε eDTUs για το χώρο συγκέντρωσης από 200 400 στις 8:05 μ.μ. Χρεώσεις στο επίπεδο 200 eDTU ήταν σε ισχύ μέχρι 8 μμ και αυξάνεται 400 eDTUs για τις υπόλοιπες τέσσερις ώρες. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Πώς η χρήση της ενεργό παν-αναπαραγωγή σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων εμφανίζεται στις χρέωσής μου;
Σε αντίθεση με μία μόνο βάσεις δεδομένων, χρησιμοποιώντας [Ενεργό παν-αλληλεπίδραση](sql-database-geo-replication-overview.md) με ελαστικά βάσεις δεδομένων δεν έχουν άμεση επίδραση χρέωσης.  Που θα χρεωθείτε μόνο για την παροχή της υπηρεσίας για κάθε έναν από τους χώρους συγκέντρωσης (πρωτεύοντα χώρο συγκέντρωσης και δευτερεύοντα χώρο συγκέντρωσης) eDTUs

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Πώς επηρεάζει η χρήση της δυνατότητας ελέγχου χρέωσής μου; 
Έλεγχος είναι ενσωματωμένη στην υπηρεσία βάση δεδομένων SQL χωρίς πρόσθετη κόστους και είναι διαθέσιμο σε βάσεις δεδομένων Basic, τυπική και Premium. Ωστόσο, για να αποθηκεύσετε τα αρχεία καταγραφής ελέγχου, το ελέγχου δυνατότητα χρησιμοποιεί ένα λογαριασμό αποθήκευσης Azure, και οι χρεώσεις για πίνακες και ουρές στο χώρο αποθήκευσης Azure με βάση το μέγεθος του αρχείου καταγραφής ελέγχου.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Πώς μπορώ να βρω το επίπεδο δεξιά service επίπεδο και επιδόσεων για μία μόνο βάσεις δεδομένων και τα σύνολα ελαστικότητας βάσης δεδομένων; 
Υπάρχουν μερικά εργαλεία που είναι διαθέσιμες σε εσάς. 

- Για βάσεις δεδομένων εσωτερικής εγκατάστασης, χρησιμοποιήστε το [σύμβουλο DTU αλλαγής μεγέθους](http://dtucalculator.azurewebsites.net/) για να συνιστάται η βάσεις δεδομένων και DTUs απαιτείται και Υπολογισμός πολλών βάσεις δεδομένων για χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων.
- Εάν μία βάση δεδομένων θα το πλεονέκτημα σε ένα χώρο συγκέντρωσης, έξυπνη μηχανισμός του Azure συνιστά ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων, εάν αυτό που βλέπει ένα μοτίβο ιστορικού χρήση που αξίζει το. Ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με την πύλη του Azure](sql-database-elastic-pool-manage-portal.md). Για λεπτομέρειες σχετικά με το πώς μπορείτε να κάνετε μαθηματικές πράξεις στον εαυτό σας, ανατρέξτε στο θέμα [θέματα τιμής και απόδοσης για ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-guidance.md)
- Για να δείτε εάν πρέπει να καλέσετε μια βάση δεδομένων μία μόνο προς τα επάνω ή προς τα κάτω, ανατρέξτε στο θέμα [επιδόσεις καθοδήγηση για μία μόνο βάσεις δεδομένων](sql-database-performance-guidance.md).

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Πόσο συχνά μπορώ να αλλάξω το επίπεδο σειρά ή επιδόσεων υπηρεσίας από μια βάση δεδομένων μία μόνο; 
Με V12 βάσεις δεδομένων, μπορείτε να αλλάξετε το επίπεδο υπηρεσιών (μεταξύ Basic τυπική και Premium) ή το επίπεδο απόδοσης μέσα σε ένα επίπεδο υπηρεσιών (για παράδειγμα, S1 έως S2) όσο συχνά θέλετε. Για βάσεις δεδομένων παλαιότερη έκδοση, μπορείτε να αλλάξετε το επίπεδο σειρά ή επιδόσεων service συνολικά τέσσερις φορές σε διάστημα 24 ωρών.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Πόσο συχνά μπορώ να προσαρμόσω το eDTUs ανά χώρο συγκέντρωσης; 
Όσο συχνά θέλετε.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Πόσος χρόνος χρειάζεται να αλλάξετε το επίπεδο σειρά ή επιδόσεων υπηρεσίας από μία βάση δεδομένων ή να μετακινήσετε μια βάση δεδομένων και έξοδος από ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων; 
Να αλλάξετε το επίπεδο υπηρεσιών μιας βάσης δεδομένων και μετακίνηση και έξοδος από ένα χώρο συγκέντρωσης απαιτεί τη βάση δεδομένων που θα αντιγραφούν στην πλατφόρμα ως φόντο λειτουργία. Αλλάζοντας το επίπεδο υπηρεσιών μπορεί να διαρκέσει από μερικά λεπτά για να μερικές ώρες ανάλογα με το μέγεθος των βάσεων δεδομένων. Και στις δύο περιπτώσεις, τις βάσεις δεδομένων παραμένουν συνδεδεμένος και διαθέσιμος στη διάρκεια της μετακίνησης. Για λεπτομέρειες σχετικά με την αλλαγή μόνο βάσεις δεδομένων, ανατρέξτε στο θέμα [Αλλαγή το επίπεδο υπηρεσιών μιας βάσης δεδομένων](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Πότε πρέπει να χρησιμοποιώ μια βάση δεδομένων μία μόνο έναντι ελαστικότητας βάσεις δεδομένων; 
Σε γενικές γραμμές, χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων έχουν σχεδιαστεί για ένα τυπικό [μοτίβο εφαρμογή λογισμικού-ως-a-service (ΑΠΑ)](sql-database-design-patterns-multi-tenancy-saas-applications.md), όπου δεν υπάρχει μία βάση δεδομένων ανά μισθωτή ή πελάτη. Αγορά μεμονωμένων βάσεων δεδομένων και overprovisioning να καλύπτει τη μεταβλητή και κορυφαίο απαιτήσεων για κάθε βάση δεδομένων είναι συχνά δεν κόστους αποτελεσματική. Με τους χώρους συγκέντρωσης, μπορείτε να διαχειριστείτε τις συλλογικές επιδόσεις του χώρου συγκέντρωσης και των βάσεων δεδομένων προς τα επάνω ή προς τα κάτω αυτόματη κλιμάκωση. 

Έξυπνη μηχανισμός του Azure συνιστά χώρου συγκέντρωσης για βάσεις δεδομένων, όταν ένα μοτίβο χρήση αξίζει το. Για λεπτομέρειες, ανατρέξτε στο θέμα [βάση δεδομένων SQL συστάσεις σειρά τις τιμές](sql-database-service-tier-advisor.md). Για λεπτομερείς οδηγίες σχετικά με την επιλογή μεταξύ μεμονωμένων και ελαστικά βάσεων δεδομένων, ανατρέξτε στο θέμα [τιμής και απόδοσης ζητήματα για χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Τι σημαίνει ο όρος να έχει έως 200% του χώρου αποθήκευσής σας μέγιστο προμήθεια του φακέλου βάσης δεδομένων αποθήκευσης αντιγράφων ασφαλείας; 
Ο χώρος αποθήκευσης αντιγράφων ασφαλείας είναι το χώρο αποθήκευσης που σχετίζεται με τα αντίγραφα ασφαλείας αυτοματοποιημένη βάσης δεδομένων που χρησιμοποιούνται για [σημείου-σε-χρόνου-επαναφορά](sql-database-recovery-using-backups.md#-point-in-time-restore) και [Παν-επαναφορά](sql-database-recovery-using-backups.md#geo-restore). Βάση δεδομένων SQL Microsoft Azure παρέχει έως 200% του χώρου αποθήκευσής σας μέγιστο προμήθεια του φακέλου βάσης δεδομένων της αποθήκευσης αντιγράφων ασφαλείας χωρίς πρόσθετο κόστος. Για παράδειγμα, εάν έχετε μια τυπική DB παρουσία με μέγεθος DB προμήθεια του φακέλου από 250 GB, που παρέχονται με 500 GB αποθήκευσης αντιγράφων ασφαλείας χωρίς πρόσθετη χρέωση. Εάν η βάση δεδομένων υπερβαίνει το παρεχόμενο αποθήκευσης αντιγράφων ασφαλείας, μπορείτε να επιλέξετε για να μειώσετε την περίοδο διατήρησης με επικοινωνία με την υποστήριξη του Azure ή να πληρώσετε για την αποθήκευση αντιγράφων ασφαλείας επιπλέον χρεωθεί με τυπική χρέωση πρόσβαση για ανάγνωση γεωγραφικά πλεονάζοντα χώρο αποθήκευσης (RA-Εξοπλισμό). Για περισσότερες πληροφορίες σχετικά με χρέωση RA Εξοπλισμό, ανατρέξτε στο θέμα λεπτομέρειες τιμολόγησης χώρου αποθήκευσης.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Να είμαι Μετακίνηση από το Web/επιχειρήσεις για τα νέα επίπεδα υπηρεσίας, τι πρέπει να γνωρίζω;
Τώρα είναι απόσυρση Azure βάσεις δεδομένων SQL Web και επιχειρήσεις. Τα επίπεδα Basic, τυπική, Premium και ελαστικές αντικαταστήστε τις retiring βάσεις δεδομένων Web και επιχειρήσεις. Θα σας έχετε επιπλέον συνήθεις Ερωτήσεις που θα πρέπει να σας βοηθήσει να σε αυτήν την περίοδο μετάβασης. [Συνήθεις Ερωτήσεις Ηλιοβασίλεμα Web και Business Edition](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Τι είναι μια καθυστέρηση αναμενόμενο αναπαραγωγής όταν αναπαράγονται παν μια βάση δεδομένων μεταξύ των δύο περιοχές εντός του ίδιου Γεωγραφία Azure;  
Θα σας υποστηρίζετε αυτήν τη στιγμή μια RPO πέντε δευτερόλεπτα και έχει την καθυστέρηση αναπαραγωγής μικρότερο από ότι κατά τη δευτερεύουσα παν φιλοξενείται στο το Azure προτεινόμενα ζεύγη περιοχής και κατά την ίδια σειρά υπηρεσίας.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Τι είναι μια καθυστέρηση αναμενόμενο αναπαραγωγής όταν παν-δευτερεύουσα δημιουργείται στην ίδια περιοχή ως την κύρια βάση δεδομένων;  
Που βασίζονται σε δεδομένα εμπειρική, δεν υπάρχει πάρα πολύ διαφορά μεταξύ εντός περιοχής και απόκρισης αλληλεπίδραση μεταξύ περιοχή όταν το Azure προτεινόμενα χρησιμοποιείται ζεύγη περιοχής. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Εάν υπάρχει μια αποτυχία δικτύου ανάμεσα σε δύο περιοχές, πώς λειτουργεί η λογική "Επανάληψη" εργασίας όταν έχει οριστεί παν αναπαραγωγής;  
Εάν υπάρχει μια αποσύνδεση, θα σας Επανάληψη κάθε 10 δευτερόλεπτα για να αποκαταστήσει συνδέσεις.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Τι μπορώ να κάνω για να εξασφαλίσετε ότι γίνεται αναπαραγωγή κρίσιμες αλλαγής κύρια βάση δεδομένων;
Το δευτερεύον παν είναι μια ρεπλίκα ασύγχρονης και δεν προσπαθήσουμε να τον διατηρήσετε στον πλήρη συγχρονισμό με το πρωτεύον. Αλλά παρέχουμε μια μέθοδο για να επιβάλετε συγχρονισμού για να βεβαιωθείτε ότι η αναπαραγωγή των κρίσιμων αλλαγών (για παράδειγμα, ενημερώσεις του κωδικού πρόσβασης). Εξαναγκασμένη συγχρονισμού επηρεάζει επιδόσεων επειδή αποκλείει συνομιλίας μέχρι να αναπαράγονται όλες οι συναλλαγές δεσμευμένη. Για λεπτομέρειες, ανατρέξτε στο θέμα [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Ποια εργαλεία είναι διαθέσιμες για την παρακολούθηση του απόκρισης αλληλεπίδραση μεταξύ της κύριας βάσης δεδομένων και παν-δευτερεύουσα;
Θα σας εκθέτει το απόκρισης σε πραγματικό χρόνο αλληλεπίδραση μεταξύ της κύριας βάσης δεδομένων και παν-δευτερεύουσα μέσω ενός DMV. Για λεπτομέρειες, ανατρέξτε στο θέμα [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

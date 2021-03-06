<properties 
   pageTitle="Τοπικά StorSimple καρφιτσωθεί όγκους συνήθεις Ερωτήσεις | Microsoft Azure"
   description="Παρέχει απαντήσεις σε συνήθεις ερωτήσεις σχετικά με όγκους StorSimple καρφιτσωθεί τοπικά."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Τοπικά StorSimple καρφιτσωθεί όγκους: συνήθεις ερωτήσεις

## <a name="overview"></a>Επισκόπηση

Οι παρακάτω είναι ερωτήσεων και απαντήσεων που ενδέχεται να έχετε όταν δημιουργείτε μια ένταση StorSimple καρφιτσωθεί τοπικά, μετατροπή ομότιμου τόμου σε τοπικά καρφιτσωμένη όγκο (και αντίστροφα), ή δημιουργία αντιγράφων ασφαλείας και επαναφορά ενός τοπικά καρφιτσωμένη όγκου.

Ερωτήσεις και απαντήσεις έχουν τακτοποιηθεί στις ακόλουθες κατηγορίες

- Δημιουργία ενός τοπικά καρφιτσωμένη τόμου
- Δημιουργία αντιγράφων ασφαλείας τοπικά καρφιτσωμένη
- Μετατροπή ενός ομότιμου όγκου τοπικά καρφιτσωμένη τόμου
- Επαναφορά ενός τοπικά καρφιτσωμένη τόμου
- Αποτυχία πάνω από ένα τοπικά καρφιτσωμένη έντασης ήχου

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Ερωτήσεις σχετικά με τη δημιουργία ενός τοπικά καρφιτσωμένη τόμου

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Τι είναι το μέγιστο μέγεθος ενός τοπικά καρφιτσωμένη τόμου που να μπορεί να δημιουργήσει στις συσκευές 8000 σειρά;

**A** που μπορούν να προμηθεύσουν τοπικά καρφιτσωμένη όγκους έως 8.5 TB ή ομότιμου όγκους έως 200 TB στη συσκευή 8100. Στη συσκευή μεγαλύτερο 8600, μπορούν να προμηθεύσουν τοπικά καρφιτσωμένη όγκους έως 22,5 TB ή ομότιμου όγκους έως 500 TB.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Αναβάθμιση πρόσφατα συσκευή μου 8100 σε ενημερωμένη έκδοση 2 και όταν προσπαθώ να δημιουργήσω ένα τοπικά καρφιτσωμένη ένταση, το μέγιστο μέγεθος διαθέσιμα είναι μόνο 6 TB και δεν 8,5 TB. Γιατί δεν είναι δυνατή η δημιουργία ενός τόμου 8,5 TB;

**A** μπορούν να προμηθεύσουν τοπικά καρφιτσωμένη όγκους έως 8,5 TB ή επιπέδων όγκους έως 200 TB στη συσκευή 8100. Εάν η συσκευή σας ήδη έχει επιπέδων όγκους, στη συνέχεια, το διαθέσιμο χώρο για τη δημιουργία ενός τοπικά καρφιτσωμένη τόμου θα αναλογικά κάτω από αυτό το μέγιστο όριο. Για παράδειγμα, εάν ήδη διαθέτετε έχει αποδοθεί 100 TB ομότιμου όγκους στη συσκευή σας 8100 (το οποίο είναι το ήμισυ της δυναμικότητας ομότιμου), στη συνέχεια, το μέγιστο μέγεθος των ενός τοπικού τόμου που μπορείτε να δημιουργήσετε στη συσκευή 8100 θα αντίστοιχα μειωθεί έως 4 TB (περίπου ήμισυ της μέγιστης τοπικά καρφιτσωμένα χωρητικότητα ένταση ήχου).

Επειδή το τοπικό χώρο στη συσκευή χρησιμοποιείται για τη φιλοξενία το σύνολο εργασίας ομότιμου όγκους, το διαθέσιμο χώρο για τη δημιουργία ενός τοπικά καρφιτσωμένη τόμου μειώνεται εάν η συσκευή διαθέτει επιπέδων όγκους. Αντίθετα, τη δημιουργία ενός τοπικά καρφιτσωμένη τόμου αναλογικά μειώνει το διαθέσιμο χώρο για ομότιμου όγκους. Ο παρακάτω πίνακας συνοψίζει τη διαθέσιμη ομότιμου δυναμικότητα στις συσκευές 8100 και 8600 όταν δημιουργούνται τοπικά καρφιτσωμένη όγκους.

|Προμήθεια του φακέλου τοπικά καρφιτσωμένη όγκους χωρητικότητα|Διαθέσιμη δυναμικότητα προμήθεια για ομότιμου όγκους - 8100|Διαθέσιμη δυναμικότητα προμήθεια για ομότιμου όγκους - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | NA | 277.8 TB |
|15 TB | NA | 166.7 TB |
|22.5 TB | NA | 0 TB |


**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Γιατί είναι η δημιουργία τοπικά καρφιτσωμένη τόμου μια λειτουργία μεγάλης διάρκειας; 

**ΑΠΑΝΤΉΣΕΙΣ.** Τοπικά καρφιτσωμένη όγκους thickly έχουν παρασχεθεί. Για να δημιουργήσετε χώρο του τοπικού βαθμίδες της συσκευής, ορισμένα δεδομένα από υπάρχουσα ομότιμου όγκους ενδέχεται να είναι δυνατή η προώθηση στο cloud κατά τη διάρκεια της διαδικασίας προετοιμασίας. Και καθώς αυτό εξαρτάται από το μέγεθος της έντασης ήχου την παροχή της υπηρεσίας, τα υπάρχοντα δεδομένα στη συσκευή σας και το διαθέσιμο εύρος ζώνης στο cloud, την ώρα που λαμβάνονται για τη δημιουργία ενός τοπικού τόμου μπορεί να είναι μερικές ώρες.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Πόσος χρόνος χρειάζεται για να δημιουργήσετε μια τοπικά καρφιτσωμένη ένταση ήχου;

**ΑΠΑΝΤΉΣΕΙΣ.** Επειδή τοπικά καρφιτσωμένη όγκους thickly έχουν παρασχεθεί, ορισμένα υπάρχοντα δεδομένα από ομότιμου όγκους ενδέχεται να είναι δυνατή η προώθηση στο cloud κατά τη διάρκεια της διαδικασίας προετοιμασίας. Επομένως, την ώρα που λαμβάνονται για να δημιουργήσετε μια τοπικά καρφιτσωμένη ένταση εξαρτάται από πολλούς παράγοντες, συμπεριλαμβανομένου του μεγέθους του τόμου, τα δεδομένα για τη συσκευή σας και το διαθέσιμο εύρος ζώνης. Σε μια συσκευή πρόσφατα εγκατεστημένων που έχει χωρίς όγκους, ο χρόνος για να δημιουργήσετε μια τοπικά καρφιτσωμένη ένταση είναι περίπου 10 λεπτά ανά terabyte των δεδομένων. Ωστόσο, η δημιουργία τοπικού όγκους μπορεί να διαρκέσει μερικές ώρες με βάση τους παράγοντες επεξήγηση επάνω σε μια συσκευή που χρησιμοποιείται.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Θέλω να δημιουργήσω μια τοπικά καρφιτσωμένη έντασης ήχου. Υπάρχουν τις βέλτιστες πρακτικές που πρέπει να γνωρίζετε;

**ΑΠΑΝΤΉΣΕΙΣ.** Τοπικά καρφιτσωμένη όγκους είναι κατάλληλες για φόρτους εργασίας που απαιτούν τοπικό εγγυήσεις δεδομένων ανά πάσα και διάκριση πεζών-κεφαλαίων των αδρανειών στο cloud. Εξετάζοντας παράλληλα χρήση της τοπικής όγκους για οποιονδήποτε από φόρτους εργασίας σας, θα πρέπει να γνωρίζετε από τα εξής:

- Τοπικά καρφιτσωμένη όγκους thickly παροχή της υπηρεσίας και τη δημιουργία τοπικού όγκους επηρεάζει τον διαθέσιμο χώρο για ομότιμου όγκους. Γι ' αυτό, προτείνουμε να ξεκινήσετε με μικρότερο μέγεθος όγκους και κλιμάκωσης ως σας αυξάνει την απαίτηση χώρου αποθήκευσης.

- Προμήθεια του τοπικού όγκους είναι μια λειτουργία μεγάλης διάρκειας που ενδέχεται να περιλαμβάνουν Ώθηση υπάρχοντα δεδομένα από ομότιμου όγκους στο cloud. Ως αποτέλεσμα, ενδέχεται να αντιμετωπίσετε μειωμένη επιδόσεων σε αυτές τις όγκους.

- Προμήθεια του τοπικού όγκους είναι μια χρονοβόρα λειτουργία. Το χρόνο που εμπλέκονται εξαρτάται από πολλούς παράγοντες: το μέγεθος της έντασης ήχου την παροχή της υπηρεσίας δεδομένων στη συσκευή σας και διαθέσιμο εύρος ζώνης. Εάν δεν έχετε δημιουργήσει το υπάρχον όγκους στο cloud, η δημιουργία τόμου είναι πιο αργά. Προτείνουμε να κάνετε λήψη στιγμιοτύπων cloud από το υπάρχον όγκους πριν από την προμήθεια ενός τοπικού τόμου.
 
- Μπορείτε να μετατρέψετε υπάρχουσες ομότιμου όγκους τοπικά καρφιτσωμένη όγκους και αυτή η μετατροπή περιλαμβάνει την προμήθεια του χώρου στη συσκευή για την ένταση που προκύπτει καρφιτσωμένη τοπικά (εκτός από τα μεταφέρετε προς τα κάτω ομότιμου δεδομένων [εάν οποιοδήποτε] από το cloud). Ξανά, αυτή είναι μια λειτουργία μεγάλης διάρκειας που εξαρτάται από παράγοντες που έχουμε εξετάσει παραπάνω. Προτείνουμε να δημιουργείτε αντίγραφα ασφαλείας του υπάρχοντος όγκους πριν από τη μετατροπή κατά τη διαδικασία θα είναι ακόμα πιο αργά εάν υπάρχοντα όγκους δεν δημιουργούνται αντίγραφα ασφαλείας. Συσκευή σας ενδέχεται να συναντήσετε επίσης μειωμένη απόδοσης κατά τη διάρκεια αυτής της διαδικασίας.
    
Περισσότερες πληροφορίες σχετικά με τον τρόπο δημιουργίας [ενός τοπικά καρφιτσωμένη τόμου](storsimple-manage-volumes-u2.md#add-a-volume)

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να δημιουργήσω πολλές τοπικά καρφιτσωμένη όγκους ταυτόχρονα;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, αλλά οποιαδήποτε τοπικά καρφιτσωμένη ένταση εργασίες δημιουργίας και επέκτασης υποβάλλονται σε επεξεργασία διαδοχικά.

Τοπικά καρφιτσωμένη όγκους thickly έχουν παρασχεθεί και αυτή η ενέργεια απαιτεί τη δημιουργία του τοπικού χώρου στη συσκευή (το οποίο μπορεί να οδηγήσει σε υπάρχοντα δεδομένα από ομότιμου όγκους για να είναι δυνατή η προώθηση στο cloud κατά τη διάρκεια της διαδικασίας προετοιμασίας). Επομένως, εάν μια εργασία προετοιμασίας βρίσκεται σε εξέλιξη, άλλες εργασίες δημιουργίας τοπικού τόμου θα τοποθετηθούν στην ουρά μέχρι να ολοκληρωθεί αυτή η εργασία.

Ομοίως, εάν μια υπάρχουσα τοπικού τόμου αναπτύσσεται ή ενός ομότιμου τόμου μετατρέπεται σε τοπικά καρφιτσωμένη όγκο, στη συνέχεια, τη δημιουργία ενός νέου τοπικά καρφιτσωμένη τόμου είναι στην ουρά μέχρι να ολοκληρωθεί η προηγούμενη εργασία. Επέκταση του μεγέθους ενός τοπικά καρφιτσωμένη τόμου περιλαμβάνει την επέκταση του υπάρχοντος τοπικού χώρου για αυτήν την ένταση ήχου. Μετατροπή από μια ομότιμου τοπικά καρφιτσωμένα ένταση περιλαμβάνει επίσης τη δημιουργία του τοπικού χώρου για την προκύπτουσα τοπικά καρφιτσωμένα έντασης ήχου. Σε δύο αυτές τις λειτουργίες, δημιουργία ή επέκταση του τοπικού χώρου πολύ εκτελείται εργασία.

Μπορείτε να προβάλετε αυτές τις εργασίες στη σελίδα " **εργασίες** " της υπηρεσίας διαχείρισης StorSimple Azure. Η εργασία που είναι ενεργά σε επεξεργασία συνεχή ενημερώνεται ώστε να αντικατοπτρίζει την πρόοδο της προμήθειας χώρου. Τις υπόλοιπες εργασίες τοπικά καρφιτσωμένη ένταση έχει επισημανθεί ως εκτελείται, αλλά σταματήσει την πρόοδό τους και τη συλλογή με τη σειρά που έχουν στην ουρά.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Διέγραψα μια τοπικά καρφιτσωμένη έντασης ήχου. Γιατί δεν βλέπω το διάστημα ποιοτική αποκατάσταση αντανακλώνται στο το διαθέσιμο χώρο, όταν προσπαθώ να δημιουργήσω ένα νέο ένταση ήχου; 

**ΑΠΑΝΤΉΣΕΙΣ.** Εάν διαγράψετε μια τοπικά καρφιτσωμένη ένταση, το διαθέσιμο χώρο για νέα όγκους ενδέχεται να μην ενημερώνεται αμέσως. Η υπηρεσία διαχείρισης StorSimple ενημερώνει το διαθέσιμο χώρο τοπικό σχεδόν κάθε ώρα. Προτείνουμε να περιμένετε για μια ώρα πριν επιχειρήσετε να δημιουργήσετε το νέο έντασης ήχου.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Υποστηρίζονται τοπικά καρφιτσωμένη όγκους της συσκευής cloud;

**ΑΠΑΝΤΉΣΕΙΣ.** Τοπικά καρφιτσωμένη όγκους δεν υποστηρίζονται στο cloud συσκευής (8010 και 8020 συσκευές παλαιότερα γνωστές ως τη συσκευή εικονικού StorSimple).

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να χρησιμοποιήσω το cmdlet του Azure PowerShell για να δημιουργήσετε και να διαχειριστείτε τοπικά καρφιτσωμένη όγκους; 

**ΑΠΑΝΤΉΣΕΙΣ.** Όχι, δεν μπορείτε να δημιουργήσετε τοπικά καρφιτσωμένη όγκους μέσω των cmdlet του Azure PowerShell (επιπέδων οποιαδήποτε ένταση δημιουργείτε μέσω του Azure PowerShell). Προτείνουμε επίσης ότι δεν χρησιμοποιείτε το cmdlet του Azure PowerShell για να τροποποιήσετε τις ιδιότητες ενός τοπικά καρφιτσωμένη τόμου, καθώς αυτό θα έχει ως αποτέλεσμα ανεπιθύμητα τροποποίηση τον τύπο του τόμου για ομότιμου.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Ερωτήσεις σχετικά με τη δημιουργία αντιγράφων ασφαλείας ενός τοπικά καρφιτσωμένη τόμου

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Είναι τοπικό στιγμιότυπων των τοπικά καρφιτσωμένη όγκους υποστηρίζονται;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, μπορείτε να κρατήσετε τοπικό στιγμιότυπων των σας τοπικά καρφιτσωμένη όγκους. Ωστόσο, προτείνουμε ιδιαίτερα ότι δημιουργείτε αντίγραφα ασφαλείας σας τοπικά καρφιτσωμένη όγκους με τα στιγμιότυπα cloud για να βεβαιωθείτε ότι τα δεδομένα σας είναι προστατευμένο σχετικές με την περίπτωση από μια καταστροφή.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Υπάρχουν τυχόν οδηγίες για τη διαχείριση του τοπικού στιγμιότυπα για τοπικά καρφιτσωμένη όγκους;

**ΑΠΑΝΤΉΣΕΙΣ.** Συχνές τοπικό στιγμιότυπα μαζί με μεγάλη ταχύτητα churn δεδομένων του όγκου τοπικά καρφιτσωμένη ενδέχεται να προκαλέσει τοπικό χώρο στη συσκευή για να καταναλωθεί γρήγορα και να έχει ως αποτέλεσμα δεδομένα από ομότιμου όγκους που προωθηθεί στο cloud. Επομένως, προτείνουμε να ελαχιστοποιήσετε τον αριθμό των τοπικό στιγμιότυπα.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Έλαβα μια προειδοποίηση που αναφέρει ότι ενδέχεται να ακυρωθούν μου τοπικό στιγμιότυπων των τοπικά καρφιτσωμένη όγκους. Πότε να κάτι τέτοιο;

**ΑΠΑΝΤΉΣΕΙΣ.** Συχνές τοπικό στιγμιότυπα μαζί με μεγάλη ταχύτητα churn δεδομένων του όγκου τοπικά καρφιτσωμένη ενδέχεται να προκαλέσει τοπικό χώρο στη συσκευή να καταναλωθεί γρήγορα. Εάν το τοπικό βαθμίδες της συσκευής που χρησιμοποιούνται σε μεγάλο βαθμό, μια μη διαθεσιμότητα εκτεταμένο cloud ενδέχεται να έχει ως αποτέλεσμα η συσκευή πώς να γίνετε πλήρους και εισερχόμενες εγγραφές την ένταση ήχου μπορεί να έχει ως αποτέλεσμα ακύρωση της τα στιγμιότυπα (όπως κανένα διάστημα υπάρχει για να ενημερώσετε τα στιγμιότυπα παραπέμπει τα παλαιότερα μπλοκ δεδομένων που αντικαταστάθηκε). Σε αυτή την περίπτωση θα συνεχίσουν οι εγγραφές την ένταση ήχου πράξης, αλλά τα τοπικά στιγμιότυπα μπορεί να είναι έγκυρα. Δεν υπάρχει καμία επίδραση για τις υπάρχουσες στιγμιότυπα cloud.

Η ειδοποίηση προειδοποίηση είναι να σας ειδοποιεί ότι η κατάσταση μπορεί να προκύψουν και βεβαιωθείτε διεύθυνση ίδια έγκαιρα, είτε ελέγχοντας σας χρονοδιαγράμματα τοπικό στιγμιότυπα στιγμιότυπα λιγότερο συχνά τοπική ή τη διαγραφή παλαιότερων τοπικό στιγμιότυπα που απαιτούνται δεν είναι πλέον.

Εάν τα στιγμιότυπα τοπικό ακυρώνονται, θα λάβετε μια ειδοποίηση πληροφορίες που σας ειδοποιεί ότι το τοπικό στιγμιότυπα για το συγκεκριμένο πολιτική αντιγράφων ασφαλείας δεν είναι πλέον έγκυροι μαζί με τη λίστα των χρονικές σημάνσεις την τοπική στιγμιότυπων, τα οποία έχουν ακυρώνονται. Αυτά τα στιγμιότυπα θα διαγραφεί αυτόματα και δεν είναι πλέον θα μπορείτε να τα προβάλετε στη σελίδα **Δημιουργία αντιγράφων ασφαλείας κατάλογοι** στην πύλη του Azure κλασική.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Ερωτήσεις σχετικά με τη μετατροπή ενός ομότιμου τόμου σε τοπικά καρφιτσωμένη όγκο

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Να παρατηρείτε ορισμένες βάρους στη συσκευή κατά τη μετατροπή ενός ομότιμου τόμου σε τοπικά καρφιτσωμένη όγκο. Γιατί συμβαίνει αυτό; 

**ΑΠΑΝΤΉΣΕΙΣ.** Η διαδικασία μετατροπής περιλαμβάνει δύο βήματα: 

  1. Προμήθεια του χώρου της συσκευής για το συντομότερο-σε--μετατρέπεται τοπικά καρφιτσωμένη έντασης ήχου.
  2. Λήψη ομότιμου δεδομένα από το cloud για να βεβαιωθείτε ότι τοπικό εγγυάται.

Και τα δύο από τα παρακάτω βήματα είναι μεγάλες εκτέλεση λειτουργιών που εξαρτώνται από το μέγεθος του όγκου τη μετατροπή δεδομένων στη συσκευή και το διαθέσιμο εύρος ζώνης. Καθώς ορισμένα δεδομένα από υπάρχουσα ομότιμου όγκους μπορεί να γίνει υπερχείλιση του στο cloud ως μέρος της διαδικασίας προετοιμασίας, τη συσκευή σας ενδέχεται να συναντήσετε μειωμένη απόδοσης κατά τη διάρκεια αυτήν τη στιγμή. Επιπλέον, μπορεί να είναι πιο αργά τη διαδικασία μετατροπής εάν:

- Υπάρχουσα όγκους δεν έχουν δημιουργηθεί αντίγραφα ασφαλείας στο cloud; Επομένως συνιστάται η δημιουργία αντιγράφου ασφαλείας σας όγκους πριν από την προετοιμασία μετατροπή.

- Το εύρος ζώνης επιτάχυνσης πολιτικές έχουν εφαρμοστεί, που μπορεί να περιορίσετε το εύρος ζώνης διαθέσιμη στο cloud; Επομένως, συνιστάται να έχετε ένα αποκλειστικό 40 Mbps ή περισσότερες σύνδεση στο cloud.

- Η διαδικασία μετατροπής μπορεί να διαρκέσει μερικές ώρες λόγω τους παράγοντες πολλαπλάσιο επεξήγηση παραπάνω; γι ' αυτό, προτείνουμε να εκτελέσετε αυτήν τη λειτουργία κατά τη διάρκεια μη κορυφών ώρες ή σε ένα Σαββατοκύριακο για να αποφύγετε την επίδραση στο τέλος των καταναλωτών.

Περισσότερες πληροφορίες σχετικά με τη [Μετατροπή ενός ομότιμου τόμου σε μια τοπικά καρφιτσωμένη έντασης ήχου](storsimple-manage-volumes-u2.md#change-the-volume-type)

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να ακυρώσετε τη λειτουργία μετατροπής ένταση ήχου;

**ΑΠΑΝΤΉΣΕΙΣ.** Όχι, δεν είναι δυνατή η ακύρωση της λειτουργίας μετατροπής μία φορά που ξεκινούν από. Όπως περιγράφεται στην προηγούμενη ερώτηση, λάβετε υπόψη τα πιθανά ζητήματα επιδόσεων που ενδέχεται να συναντήσετε κατά τη διαδικασία, και ακολουθήστε τις βέλτιστες πρακτικές που αναφέρονται παραπάνω κατά το σχεδιασμό σας μετατροπής.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Τι συμβαίνει με μου ένταση εάν αποτύχει η λειτουργία μετατροπής;

**ΑΠΑΝΤΉΣΕΙΣ.** Μετατροπή τόμου μπορεί να αποτύχει λόγω ζητημάτων συνδεσιμότητας του cloud. Η συσκευή μπορεί να εμφανίσει να διακόψετε τη διαδικασία μετατροπής μετά από μια σειρά των αποτυχημένων προσπαθειών για να μεταφέρετε προς τα κάτω ομότιμου δεδομένων από το cloud. Σε ένα τέτοιο σενάριο, τον τύπο του τόμου θα συνεχίσουν να είναι ο τύπος ένταση προέλευσης πριν από τη μετατροπή, και:

- Μια ειδοποίηση κρίσιμης στρογγυλοποιείται να ειδοποιήσετε σχετικά με την αποτυχία μετατροπής έντασης ήχου. Περισσότερες πληροφορίες σχετικά με [τις ειδοποιήσεις που σχετίζονται με τοπικά καρφιτσωμένη όγκους](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Εάν θέλετε να μετατρέψετε ένα ομότιμου σε τοπικά καρφιτσωμένη όγκο, την ένταση ήχου θα συνεχίσουν να παρουσιάζουν ιδιοτήτων ομότιμου τόμου όπως δεδομένων μπορεί να εξακολουθεί να βρίσκονται στο cloud. Προτείνουμε να που επιλύσετε τα προβλήματα σύνδεσης και, στη συνέχεια, επαναλάβετε τη λειτουργία μετατροπής.
 
- Ομοίως, όταν μετατροπής από μια τοπικά καρφιτσωμένη σε ομότιμου όγκο αποτύχει, παρόλο που η ένταση θα επισημανθούν ως ένα τοπικά καρφιτσωμένη ένταση, αυτό θα λειτουργεί ως ενός ομότιμου τόμου (επειδή θα μπορούσε να έχει έχουν δεδομένα στο cloud). Ωστόσο, θα εξακολουθήσει να καταλάβει χώρο στην τοπική τις βαθμίδες της συσκευής. Αυτός ο χώρος δεν θα είναι διαθέσιμες για άλλες τοπικά καρφιτσωμένη όγκους. Προτείνουμε να επαναλάβετε τη λειτουργία για να βεβαιωθείτε ότι η ένταση μετατροπή ολοκληρώνεται και μπορεί να ανακτηθεί τον τοπικό χώρο στη συσκευή.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Ερωτήσεις σχετικά με την επαναφορά ενός τοπικά καρφιτσωμένη τόμου

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Είναι τοπικά καρφιτσωμένη όγκους ανακτηθεί αμέσως;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, τοπικά καρφιτσωμένη όγκους επαναφέρονται αμέσως. Μόλις οι πληροφορίες μετα-δεδομένων για την ένταση ήχου τα μηνύματα μεταφέρονται από το cloud ως μέρος της λειτουργίας επαναφοράς, η ένταση σύνδεση και είναι δυνατή η πρόσβαση από τον κεντρικό υπολογιστή. Ωστόσο, τοπικό εγγυήσεις για την ένταση ήχου δεδομένων δεν θα εμφανίζεται μέχρι να όλα τα δεδομένα έχουν ληφθεί από το cloud και ενδέχεται να αντιμετωπίσετε μειωθεί επιδόσεων σε αυτές τις όγκους στη διάρκεια της επαναφοράς.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Πόσος χρόνος χρειάζεται για να επαναφέρετε μια τοπικά καρφιτσωμένη ένταση ήχου;

**ΑΠΑΝΤΉΣΕΙΣ.** Τοπικά καρφιτσωμένη όγκους είναι επαναφορά αμέσως και να τεθούν σε σύνδεση με την ένταση μετα-δεδομένων είναι η ανάκτηση των πληροφοριών από το cloud, ενώ τα δεδομένα ένταση εξακολουθεί να γίνει λήψη στο παρασκήνιο. Αυτό το τελευταίο τμήμα της λειτουργίας επαναφοράς--γρήγορα ξανά την τοπική εγγυήσεις για τα δεδομένα ένταση--είναι μια λειτουργία μεγάλης διάρκειας και μπορεί να διαρκέσει μερικές ώρες για όλα τα δεδομένα πρέπει να γίνουν τοπικό ξανά. Ο χρόνος που λαμβάνονται για να ολοκληρώσετε το ίδιο εξαρτάται από πολλούς παράγοντες, όπως το μέγεθος του όγκου πραγματοποιείται επαναφορά και το διαθέσιμο εύρος ζώνης. Εάν η αρχική ένταση που πραγματοποιείται επαναφορά έχει διαγραφεί, θα ληφθούν επιπλέον χρόνος για να δημιουργήσετε τον τοπικό χώρο στη συσκευή ως μέρος της λειτουργίας επαναφοράς.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Χρειάζομαι για να επαναφέρετε μου υπάρχοντα τοπικά καρφιτσωμένη έντασης ήχου σε μια παλαιότερη στιγμιότυπο (που λαμβάνονται όταν η ένταση έχει επιπέδων). Θα την ένταση ήχου να γίνει επαναφορά όπως επιπέδων σε αυτήν την περίπτωση;

**ΑΠΑΝΤΉΣΕΙΣ.** Όχι, θα γίνει επαναφορά της έντασης ως ένα τοπικά καρφιτσωμένη έντασης ήχου. Παρόλο που οι ημερομηνίες στιγμιότυπου στο χρόνο όταν η ένταση έχει επιπέδων, κατά την επαναφορά υπάρχοντος όγκους, StorSimple πάντοτε χρησιμοποιεί τον τύπο της έντασης ήχου στο δίσκο καθώς υπάρχει αυτήν τη στιγμή.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Να επεκταθεί μου τοπικά καρφιτσωμένη ένταση πρόσφατα, αλλά τώρα πρέπει να επαναφέρετε τα δεδομένα σε μια ώρα κατά την ένταση ήχου μικρότερο μέγεθος. Θα επαναφορά αλλάξετε το μέγεθος του τρέχοντος τόμου και θα πρέπει να επεκτείνετε το μέγεθος του όγκου μόλις ολοκληρωθεί η επαναφορά;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, η επαναφορά θα αλλάξει μέγεθος την ένταση ήχου και θα πρέπει να επεκτείνετε το μέγεθος του όγκου μετά την ολοκλήρωση της επαναφοράς.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να αλλάξω τον τύπο ενός τόμου κατά τη διάρκεια της επαναφοράς;

**A.** Όχι, δεν μπορείτε να αλλάξετε τον τύπο του τόμου κατά τη διάρκεια της επαναφοράς.

- Όγκους που έχουν διαγραφεί επαναφέρονται ως τον τύπο που είναι αποθηκευμένα στο στιγμιότυπο.

- Υπάρχουσα όγκους επαναφέρονται βάσει του τρέχοντος τύπου, ανεξάρτητα από τον τύπο που είναι αποθηκευμένα στο στιγμιότυπο (ανατρέξτε στις προηγούμενες δύο ερωτήσεις).
 
**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Πρέπει να επαναφέρετε μου τοπικά καρφιτσωμένη ένταση, αλλά να επιλέξει ένα λάθος σημείο στο στιγμιότυπο ώρα. Μπορώ να ακυρώσω την τρέχουσα λειτουργία επαναφοράς;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, μπορείτε να ακυρώσετε μια λειτουργία επαναφοράς συνεχή. Η κατάσταση του τόμου θα γίνει επαναφορά στην κατάσταση κατά την έναρξη της επαναφοράς. Ωστόσο, όλες οι εγγραφές που έχουν πραγματοποιηθεί για την ένταση ήχου, ενώ η επαναφορά ήταν σε εξέλιξη θα χαθούν.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Να ξεκινήσετε μια λειτουργία επαναφοράς σε μία από μου τοπικά καρφιτσωμένη όγκους και τώρα μπορώ να δω ένα στιγμιότυπο μου κατάλογο λίστας εκκρεμοτήτων που να μην recollect τη δημιουργία. Τι είναι αυτό χρησιμοποιείται για;

**ΑΠΑΝΤΉΣΕΙΣ.** Αυτό είναι το προσωρινό στιγμιότυπο που έχει δημιουργηθεί πριν από τη λειτουργία επαναφοράς και χρησιμοποιείται για επαναφορά, σε περίπτωση που η επαναφορά ακυρώνεται ή αποτυγχάνει. Μην διαγράψετε αυτό το στιγμιότυπο; Αυτό θα διαγραφούν αυτόματα όταν ολοκληρωθεί η επαναφορά. Αυτή η συμπεριφορά μπορεί να προκύψει εάν σας εργασίας επαναφοράς περιλαμβάνει μόνο τοπικά καρφιτσωμένο όγκους ή συνδυασμό και των τοπικά καρφιτσωμένη και ομότιμου όγκους. Εάν η εργασία επαναφοράς περιλαμβάνει μόνο ομότιμου όγκους, στη συνέχεια, αυτή η συμπεριφορά δεν θα συμβεί.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Να η κλωνοποίηση μια τοπικά καρφιτσωμένη ένταση ήχου;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, μπορείτε να κάνετε. Ωστόσο, η ένταση τοπικά καρφιτσωμένη θα κλωνοποιηθεί ως ενός ομότιμου τόμου από προεπιλογή. Περισσότερες πληροφορίες σχετικά με τον τρόπο [αντιγραφής ενός τοπικά καρφιτσωμένη τόμου](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Ερωτήσεις σχετικά με την ανακατεύθυνση ενός τοπικά καρφιτσωμένη τόμου

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Πρέπει να αποτύχει πάνω από τη συσκευή μου σε άλλη συσκευή φυσικής. Θα μου τοπικά καρφιτσωμένη όγκους αποτύχει πάνω από καθώς τοπικά καρφιτσωμένα ή επιπέδων;

**ΑΠΑΝΤΉΣΕΙΣ.** Ανάλογα με την έκδοση του λογισμικού της συσκευής προορισμού, τοπικά καρφιτσωμένη όγκους θα αποτύχει μέσω ως:

- Τοπικά καρφιτσωμένα εάν η συσκευή προορισμού διαθέτει StorSimple 8000 σειρά ενημέρωση 2
- Επιπέδων εάν η συσκευή προορισμού διαθέτει StorSimple 8000 σειρά ενημέρωση 1.x
- Επιπέδων εάν η συσκευή προορισμού είναι η συσκευή cloud (έκδοση ενημερωμένη έκδοση 2 λογισμικού ή ενημέρωση 1.x)

Περισσότερες πληροφορίες σχετικά με την [ανακατεύθυνση και DR της τοπικά καρφιτσωμένα όγκους μεταξύ εκδόσεων](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Τοπικά καρφιτσωμένη όγκους αμέσως επαναφέρονται στη διάρκεια αποκατάσταση (DR);

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, τοπικά καρφιτσωμένη όγκους επαναφέρονται αμέσως κατά την ανακατεύθυνση. Μόλις οι πληροφορίες μετα-δεδομένων για την ένταση ήχου τα μηνύματα μεταφέρονται από το cloud ως μέρος της λειτουργίας ανακατεύθυνσης, η ένταση είναι έφερε online στη συσκευή προορισμού και είναι δυνατή η πρόσβαση από τον κεντρικό υπολογιστή. Στο μεταξύ, τα δεδομένα της έντασης ήχου θα συνεχίσουν να κάνετε λήψη στο παρασκήνιο και μπορεί να αντιμετωπίσετε μειωμένη επιδόσεων σε αυτές τις όγκους στη διάρκεια του εφεδρικού.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να δω την εργασία ανακατεύθυνσης ολοκληρωθεί, πώς μπορώ να παρακολουθώ την πρόοδο του τοπικά καρφιτσωμένη τόμου που πραγματοποιείται επαναφορά στη συσκευή προορισμού;

**ΑΠΑΝΤΉΣΕΙΣ.** Κατά τη διάρκεια μιας λειτουργίας ανακατεύθυνσης, η εργασία ανακατεύθυνσης έχει επισημανθεί ως μόνο μία φορά Συμπληρώστε όλα τα όγκους του συνόλου ανακατεύθυνσης έχουν αμέσως επαναφορά και να συνδεθεί στη συσκευή προορισμού. Αυτό περιλαμβάνει οποιαδήποτε τοπικά καρφιτσωμένη όγκους που ενδέχεται να απέτυχε μέσω; Ωστόσο, τοπικό εγγυήσεις των δεδομένων θα είναι διαθέσιμη μόνο όταν έχει γίνει λήψη όλων των δεδομένων για την ένταση ήχου. Μπορείτε να παρακολουθείτε αυτό προόδου για κάθε καρφιτσωμένη τοπικά τόμου που απέτυχε από την αρχή παρακολούθηση τις αντίστοιχες επαναφορά εργασίες που έχουν δημιουργηθεί ως μέρος του εφεδρικού. Αυτές οι μεμονωμένων εργασιών επαναφοράς θα δημιουργηθεί μόνο για τοπικά καρφιτσωμένη όγκους.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορώ να αλλάξω τον τύπο ενός τόμου κατά την ανακατεύθυνση;

**ΑΠΑΝΤΉΣΕΙΣ.** Όχι, δεν μπορείτε να αλλάξετε τον τύπο του τόμου κατά τη διάρκεια της ανακατεύθυνσης. Εάν έχετε αποτυγχάνουν μέσω σε άλλη συσκευή φυσικής που εκτελεί StorSimple 8000 σειρά ενημερωμένη έκδοση 2, το όγκους θα αποτύχει πάνω από την με βάση τον τύπο τόμου που είναι αποθηκευμένα στο στιγμιότυπο. Όταν αποτυγχάνει επάνω σε οποιαδήποτε άλλη συσκευή έκδοση, ανατρέξτε στην ερώτηση επάνω από τον τύπο του τόμου μετά ανακατεύθυνσης.

**ΟΙ ΕΡΩΤΉΣΕΙΣ.** Μπορεί να αποτύχει επάνω σε ένα κοντέινερ ένταση με τοπικά καρφιτσωμένη όγκους στη συσκευή cloud;

**ΑΠΑΝΤΉΣΕΙΣ.** Ναι, μπορείτε να κάνετε. Τα τοπικά καρφιτσωμένη όγκους θα αποτύχει μέσω ως ομότιμου όγκους. Περισσότερες πληροφορίες σχετικά με την [ανακατεύθυνση και DR της τοπικά καρφιτσωμένα όγκους μεταξύ εκδόσεων](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)



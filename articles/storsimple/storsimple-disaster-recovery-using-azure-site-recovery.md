<properties 
   pageTitle="Αυτοματοποίηση DR για κοινόχρηστα στοιχεία αρχείων σε StorSimple χρησιμοποιώντας την αποκατάσταση τοποθεσίας Azure | Microsoft Azure"
   description="Περιγράφει τα βήματα και τις βέλτιστες πρακτικές για τη δημιουργία μιας λύσης αποκατάστασης από καταστροφή για κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται στο χώρο αποθήκευσης StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="vidarmsft"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/16/2016"
   ms.author="vidarmsft" />

# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>Αυτόματη αποκατάσταση λύση χρησιμοποιώντας την αποκατάσταση τοποθεσίας Azure για κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται σε StorSimple

## <a name="overview"></a>Επισκόπηση

Microsoft Azure StorSimple είναι μια υβριδική λύση χώρο αποθήκευσης στο cloud που αντιμετωπίζει το τις πολυπλοκότητες μη δομημένα δεδομένα συνήθως που σχετίζονται με κοινόχρηστα στοιχεία αρχείων. StorSimple χρησιμοποιεί χώρο αποθήκευσης στο cloud ως επέκταση της λύσης εσωτερικής εγκατάστασης και αυτόματα τις σειρές δεδομένων κατά μήκος της εσωτερικής αποθήκευσης και το χώρο αποθήκευσης στο cloud. Ενσωματωμένο προστασία δεδομένων, με τοπική και cloud στιγμιότυπα, καταργεί την ανάγκη για μια sprawling υποδομή χώρου αποθήκευσης.

[Επαναφορά τοποθεσίας Azure](../site-recovery/site-recovery-overview.md) είναι μια υπηρεσία που βασίζονται στο Azure που παρέχει από καταστροφή δυνατότητες για ανάκτηση (DR) από orchestrating αναπαραγωγή, ανακατεύθυνσης και αξιοποίηση των εικονικές μηχανές. Azure Επαναφορά τοποθεσίας υποστηρίζει έναν αριθμό τεχνολογίες αναπαραγωγής να αναπαραγάγετε με συνέπεια, προστασία και ανακατευθύνει απρόσκοπτα εικονικές μηχανές και εφαρμογές σε δημόσια/ιδιωτική ή φιλοξενούμενη σύννεφων.

Χρήση Επαναφορά τοποθεσίας Azure, εικονική μηχανή αναπαραγωγής και δυνατότητες στιγμιοτύπων cloud StorSimple, μπορείτε να προστατεύσετε το περιβάλλον διακομιστή ολόκληρο αρχείο. Σε περίπτωση διαταραχή, μπορείτε να χρησιμοποιήσετε ένα μόνο κλικ για να εμφανίσετε το κοινόχρηστα στοιχεία αρχείων online στο Azure σε λίγα λεπτά.

Αυτό το έγγραφο εξηγεί με λεπτομέρειες πώς μπορείτε να δημιουργήσετε μια λύση αποκατάστασης από καταστροφή για σας κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται στο χώρο αποθήκευσης StorSimple, και εκτελέστε προγραμματισμένες, μη προγραμματισμένη, και δοκιμή ανακατευθύνσεις χρησιμοποιώντας ένα σχέδιο αποκατάστασης ένα κλικ. Στην πραγματικότητα, σας δείχνει πώς μπορείτε να τροποποιήσετε την το πεδίο σχέδιο αποκατάστασης στο σας θάλαμο Azure Επαναφορά τοποθεσίας για να ενεργοποιήσετε την ανακατευθύνσεις StorSimple κατά τη διάρκεια της καταστροφής σενάρια. Επιπλέον, περιγράφει υποστηριζόμενες ρυθμίσεις παραμέτρων και τις προϋποθέσεις. Αυτό το έγγραφο προϋποθέτει ότι είστε εξοικειωμένοι με τα βασικά στοιχεία για επαναφορά τοποθεσίας Azure και StorSimple αρχιτεκτονικές.

## <a name="supported-azure-site-recovery-deployment-options"></a>Υποστηριζόμενες επιλογές ανάπτυξης Azure Επαναφορά τοποθεσίας

Οι πελάτες να ανάπτυξης διακομιστές αρχείων ως φυσικής διακομιστών ή εικονικές μηχανές (ΣΠΣ) εκτελείται σε Hyper-V ή VMware και, στη συνέχεια, να δημιουργήσετε κοινόχρηστα στοιχεία αρχείων από όγκους σκαλισμένη ο χώρος αποθήκευσης StorSimple. Azure Επαναφορά τοποθεσίας για να προστατεύσετε φυσικών και εικονικού αναπτύξεις είτε σε δευτερεύουσα τοποθεσία ή να Azure. Αυτό το έγγραφο καλύπτει λεπτομέρειες μιας λύσης DR με Azure ως στην τοποθεσία ανάκτησης για διακομιστή αρχείων Εικονική φιλοξενούνται σε Hyper-V και κοινόχρηστων αρχείων σε StorSimple χώρου αποθήκευσης. Άλλα σενάρια με την οποία ο διακομιστής αρχείο Εικονική βρίσκεται σε μια Εικονική VMware ή φυσικής μηχανή μπορεί να υλοποιηθεί κατά τον ίδιο τρόπο.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εφαρμογή μιας λύσης αποκατάστασης από καταστροφή ένα κλικ που χρησιμοποιεί Επαναφορά τοποθεσίας Azure για κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται στο χώρο αποθήκευσης StorSimple περιλαμβάνει τις ακόλουθες προϋποθέσεις:

-   Εσωτερικής εγκατάστασης διακομιστή Windows Server 2012 R2 αρχείο Εικονική φιλοξενούνται σε Hyper-V ή VMware ή φυσικής μηχανή

-   StorSimple χώρου αποθήκευσης συσκευή εσωτερικής εγκατάστασης που έχουν καταχωρηθεί manager Azure StorSimple

-   StorSimple συσκευής Cloud που έχουν δημιουργηθεί με τη Διαχείριση Azure StorSimple (αυτό μπορεί να διατηρούνται σε κατάσταση τερματισμού)

-   Κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται από τη ρύθμιση των παραμέτρων της συσκευής αποθήκευσης StorSimple όγκους

-   [Επαναφορά τοποθεσίας azure υπηρεσίες θάλαμο](../site-recovery/site-recovery-vmm-to-vmm.md) δημιουργήθηκε σε μια συνδρομή στο Microsoft Azure

Επιπλέον, εάν το Azure είναι τοποθεσία ανάκτησης, εκτελέστε το [εργαλείο αξιολόγηση ετοιμότητας εικονική μηχανή Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) σε VM για να βεβαιωθείτε ότι είναι συμβατό με τις υπηρεσίες ΣΠΣ Azure και Επαναφορά τοποθεσίας Azure.

Για να αποφύγετε λανθάνων χρόνος θέματα (το οποίο μπορεί να οδηγήσει σε υψηλότερες κόστους), βεβαιωθείτε ότι δημιουργείτε σας συσκευής Cloud StorSimple αυτοματισμού λογαριασμού και χώρου αποθήκευσης λογαριασμών στην ίδια περιοχή.

## <a name="enable-dr-for-storsimple-file-shares"></a>Ενεργοποίηση DR για StorSimple κοινόχρηστα στοιχεία αρχείων  

Κάθε στοιχείο του περιβάλλοντος εσωτερικής εγκατάστασης πρέπει να είναι προστατευμένο για να ενεργοποιήσετε την πλήρη αναπαραγωγή και αποκατάστασης. Αυτή η ενότητα περιγράφει τον τρόπο:

-   Ρύθμιση του αναπαραγωγής της υπηρεσίας καταλόγου Active Directory και το DNS (προαιρετικά)

-   Χρήση του Azure Επαναφορά τοποθεσίας για να ενεργοποιήσετε την προστασία του διακομιστή αρχείων Εικονική

-   Ενεργοποίηση προστασίας του όγκους StorSimple

-   Ρύθμιση παραμέτρων του δικτύου

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Ρύθμιση του αναπαραγωγής της υπηρεσίας καταλόγου Active Directory και το DNS (προαιρετικά)

Εάν θέλετε να προστατεύσετε το υπολογιστές που εκτελούν υπηρεσίας καταλόγου Active Directory και το DNS, ώστε να είναι διαθέσιμες στην τοποθεσία DR, πρέπει να προστατεύσετε ρητά (έτσι ώστε οι διακομιστές αρχείου είναι προσβάσιμη μετά ανακατεύθυνσης με έλεγχο ταυτότητας). Υπάρχουν δύο Συνιστώμενες επιλογές με βάση την πολυπλοκότητα του περιβάλλοντος εσωτερικής εγκατάστασης του πελάτη.

#### <a name="option-1"></a>Επιλογή 1

Εάν ο πελάτης έχει ένα μικρό αριθμό των εφαρμογών, έναν μεμονωμένο ελεγκτή τομέα για ολόκληρο το εσωτερικής τοποθεσίας και θα αποτυγχάνει επάνω σε ολόκληρη την τοποθεσία, στη συνέχεια, συνιστάται να χρησιμοποιείτε Επαναφορά τοποθεσίας Azure αναπαραγωγής για την αναπαραγωγή του υπολογιστή του ελεγκτή τομέα σε μια δευτερεύουσα τοποθεσία (αυτό είναι που ισχύουν για--τοποθεσίας και τοποθεσιών να Azure).

#### <a name="option-2"></a>Επιλογή 2

Εάν ο πελάτης έχει μεγάλο αριθμό με τις εφαρμογές του, εκτελεί ένα σύμπλεγμα δομών της υπηρεσίας καταλόγου Active Directory και θα αποτυγχάνει πάνω από μερικές εφαρμογές κάθε φορά, στη συνέχεια, σας συνιστούμε να τη ρύθμιση του ενός πρόσθετου ελεγκτή τομέα στην τοποθεσία DR (είτε μια δευτερεύουσα τοποθεσία ή στο Azure).

Ανατρέξτε [DR αυτόματης λύση για την υπηρεσία καταλόγου Active Directory και το DNS χρησιμοποιώντας την αποκατάσταση τοποθεσίας Azure](../site-recovery/site-recovery-active-directory.md) για οδηγίες όταν διάθεση ελεγκτή τομέα στην τοποθεσία DR. Για το υπόλοιπο αυτού του εγγράφου, θα υποθέσουμε ότι ελεγκτή τομέα είναι διαθέσιμη στην τοποθεσία DR.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Χρήση του Azure Επαναφορά τοποθεσίας για να ενεργοποιήσετε την προστασία του διακομιστή αρχείων Εικονική

Αυτό το βήμα απαιτεί ότι μπορείτε να προετοιμάσετε το περιβάλλον εσωτερικής εγκατάστασης αρχείο διακομιστή, δημιουργία και προετοιμασία ένα θάλαμο Επαναφορά τοποθεσίας Azure και ενεργοποίηση προστασίας αρχείων των η Εικονική.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Για να προετοιμάσετε το περιβάλλον εσωτερικής εγκατάστασης αρχείο διακομιστή

1.  Ορίστε το στοιχείο **Έλεγχος λογαριασμού χρήστη** να **ειδοποιήσετε ποτέ**. Αυτό είναι απαραίτητο, ώστε να μπορείτε να χρησιμοποιήσετε Azure αυτοματισμού δέσμες ενεργειών για τη σύνδεση των στόχων iSCSI μετά την αποτυχία από την αρχή Azure τοποθεσίας αποκατάστασης.

    1.  Πατήστε το πλήκτρο των Windows + Q και κάντε αναζήτηση για **έλεγχο λογαριασμού ΧΡΉΣΤΗ**.

    2.  Επιλέξτε **ρυθμίσεις αλλαγή έλεγχος λογαριασμού χρήστη**.

    3.  Σύρετε τη γραμμή προς τα κάτω προς **Ειδοποιούμαι ποτέ**.

    4.  Κάντε κλικ στο κουμπί **OK** και, στη συνέχεια, επιλέξτε **Ναι** όταν σας ζητηθεί.

        ![](./media/storsimple-dr-using-asr/image1.png)

1.  Εγκαταστήστε τον παράγοντα Εικονική σε κάθε μία από το διακομιστή αρχείων ΣΠΣ. Αυτό είναι απαραίτητο, ώστε να μπορείτε να εκτελέσετε δέσμες ενεργειών Azure αυτοματισμού στα το αποτυχίας μέσω ΣΠΣ.

    1.  [Λήψη τον παράγοντα](http://aka.ms/vmagentwin) `C:\\Users\\<username>\\Downloads`.

    2.  Ανοίξτε το Windows PowerShell σε κατάσταση λειτουργίας διαχειριστή (Εκτέλεση ως διαχειριστής) και, στη συνέχεια, πληκτρολογήστε την παρακάτω εντολή για να μεταβείτε στη θέση λήψης:

        `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

        > [AZURE.NOTE] Το όνομα του αρχείου μπορεί να αλλάξει, ανάλογα με την έκδοση.

1.  Κάντε κλικ στο κουμπί **Επόμενο**.

2.  Αποδεχτείτε **Τους όρους της συμφωνίας** και, στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.

3.  Κάντε κλικ στο κουμπί **Τέλος**.


1.  Δημιουργία κοινόχρηστων αρχείων με χρήση όγκους σκαλισμένη ο χώρος αποθήκευσης StorSimple. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [χρήση της υπηρεσίας διαχείρισης StorSimple για τη Διαχείριση όγκους](storsimple-manage-volumes.md).

    1.  Στο σας ΣΠΣ εσωτερικής εγκατάστασης, πατήστε το πλήκτρο των Windows + Q και να αναζητήσετε **iSCSI**.

    2.  Επιλέξτε **iSCSI προετοιμασίας**.

    3.  Επιλέξτε την καρτέλα **ρύθμισης παραμέτρων** και αντιγράψτε το όνομα του δημιουργού.

    4.  Συνδεθείτε στο [Azure κλασική πύλη](https://manage.windowsazure.com/).

    5.  Επιλέξτε την καρτέλα **StorSimple** και, στη συνέχεια, επιλέξτε την υπηρεσία διαχείρισης StorSimple που περιέχει τη φυσική συσκευή.

    6.  Δημιουργία container(s) έντασης ήχου και, στη συνέχεια, δημιουργήστε τόμων. (Αυτές οι όγκους είναι για το αρχείο share(s) στο διακομιστή αρχείων ΣΠΣ). Αντιγράψτε το όνομα του δημιουργού και δώστε ένα κατάλληλο όνομα για τις εγγραφές ελέγχου πρόσβασης κατά τη δημιουργία του όγκους.

    7.  Επιλέξτε την καρτέλα **Ρύθμιση παραμέτρων** και τη Σημείωση προς τα κάτω τη διεύθυνση IP της συσκευής.

    8.  Σε σας ΣΠΣ εσωτερικής εγκατάστασης, μεταβείτε ξανά το **πρόγραμμα προετοιμασίας iSCSI** και εισαγάγετε το IP στην ενότητα γρήγορη σύνδεση. Κάντε κλικ στην επιλογή **Γρήγορη σύνδεση** (στη συσκευή θα πρέπει τώρα να είναι συνδεδεμένη).

    9.  Ανοίξτε την πύλη διαχείρισης του Azure και επιλέξτε την καρτέλα **όγκους και συσκευές** . Κάντε κλικ στην επιλογή **Αυτόματη ρύθμιση παραμέτρων**. Θα εμφανιστούν την ένταση ήχου που μόλις δημιουργήσατε.

    10. Στην πύλη του, επιλέξτε την καρτέλα **συσκευές** και, στη συνέχεια, επιλέξτε **δημιουργήσετε μια νέα συσκευή εικονικού.** (Αυτή η εικονική συσκευή θα χρησιμοποιηθεί εάν παρουσιαστεί ανακατεύθυνσης). Αυτής της νέας συσκευής εικονικού μπορεί να διατηρούνται σε κατάσταση χωρίς σύνδεση για να αποφύγετε την επιπλέον κόστους. Για να κρατήσετε το εικονικό συσκευής για εργασία χωρίς σύνδεση, μεταβείτε στην ενότητα **εικονικές μηχανές** στην πύλη και τερματισμός του.

    11. Επιστρέψτε στο του ΣΠΣ εσωτερικής εγκατάστασης και ανοίξτε τη Διαχείριση δίσκων (πατήστε το πλήκτρο των Windows + X και επιλέξτε **Διαχείριση δίσκων**).

    12. Θα παρατηρήσετε ορισμένες επιπλέον δίσκων (ανάλογα με τον αριθμό των όγκους που έχετε δημιουργήσει). Κάντε δεξί κλικ το πρώτο, επιλέξτε **Προετοιμασία δίσκου**και επιλέξτε **OK**. Κάντε δεξί κλικ στην ενότητα **Unallocated** , επιλέξτε **Νέα απλή ένταση ήχου**, εκχωρήσετε ένα γράμμα μονάδας δίσκου και ολοκλήρωση του οδηγού.

    13. Επαναλάβετε το βήμα l για όλων των δίσκων. Τώρα, μπορείτε να δείτε όλων των δίσκων σε **Αυτόν τον Υπολογιστή** στην Εξερεύνηση των Windows.

    14. Χρησιμοποιήστε το ρόλο υπηρεσίες αποθήκευσης αρχείων και να δημιουργήσετε κοινόχρηστα στοιχεία αρχείων σε αυτές τις όγκους.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Για να δημιουργήσετε και να προετοιμάσετε ένα θάλαμο Azure Επαναφορά τοποθεσίας

Ανατρέξτε στην [τεκμηρίωση Azure Επαναφορά τοποθεσίας](../site-recovery/site-recovery-hyper-v-site-to-azure.md) για να ξεκινήσετε με Επαναφορά τοποθεσίας Azure πριν από την προστασία του διακομιστή αρχείων Εικονική.

#### <a name="to-enable-protection"></a>Για να ενεργοποιήσετε την προστασία

1.  Αποσυνδέστε το target(s) iSCSI από του ΣΠΣ εσωτερικής εγκατάστασης που θέλετε να προστατεύσετε μέσω Azure Επαναφορά τοποθεσίας:

    1.  Πατήστε το πλήκτρο των Windows + Q και να αναζητήσετε **iSCSI**.

    2.  Επιλέξτε **Ρύθμιση προετοιμασίας iSCSI**.

    3.  Αποσυνδέστε τη συσκευή StorSimple που έχετε συνδέσει προηγουμένως. Εναλλακτικά, μπορείτε να μεταβείτε από το διακομιστή αρχείων για λίγα λεπτά όταν η ενεργοποίηση προστασίας.

    > [AZURE.NOTE] Αυτό θα προκαλέσει κοινόχρηστα στοιχεία αρχείων που να είναι προσωρινά μη διαθέσιμος

1.  [Ενεργοποίηση προστασίας εικονική μηχανή](../site-recovery/site-recovery-hyper-v-site-to-azure.md##step-6-enable-replication) του διακομιστή αρχείων Εικονική από την πύλη Επαναφορά τοποθεσίας Azure.

2.  Όταν ξεκινά το αρχικό συγχρονισμό, μπορείτε να συνδεθείτε ξανά προορισμού ξανά. Επιλέξτε το πρόγραμμα προετοιμασίας iSCSI, επιλέξτε τη συσκευή StorSimple και κάντε κλικ στην επιλογή **σύνδεση**.

3.  Όταν ολοκληρωθεί το συγχρονισμό και την κατάσταση της η Εικονική είναι **προστατευμένο**, επιλέξτε την εικονική Μηχανή, επιλέξτε στην καρτέλα **Ρύθμιση παραμέτρων** και ενημερώστε το δίκτυο του Εικονική αντίστοιχα (αυτό είναι το δίκτυο στο οποίο αποτυχίας μέσω VM(s) θα είναι μέρος). Εάν δεν εμφανίζεται στο δίκτυο, αυτό σημαίνει ότι ο συγχρονισμός συνεχίζεται.

### <a name="enable-protection-of-storsimple-volumes"></a>Ενεργοποίηση προστασίας του όγκους StorSimple

Εάν δεν έχετε επιλέξει την επιλογή **Ενεργοποίηση ένα αντίγραφο ασφαλείας προεπιλογή για αυτήν την ένταση ήχου** για το όγκους StorSimple, μεταβείτε στις **Πολιτικές ασφαλείας** στην υπηρεσία StorSimple Manager και δημιουργία αντιγράφων ασφαλείας κατάλληλου πολιτικής για όλα τα όγκους. Συνιστάται να ορίσετε τη συχνότητα δημιουργίας αντιγράφων ασφαλείας για το στόχο σημείου αποκατάστασης (RPO) που θέλετε να δείτε για την εφαρμογή.

### <a name="configure-the-network"></a>Ρύθμιση παραμέτρων του δικτύου

Για το αρχείο διακομιστή Εικονική, ρύθμιση παραμέτρων δικτύου στο Azure τοποθεσίας ανάκτησης, έτσι ώστε τα δίκτυα Εικονική είναι συνδεδεμένος στο δίκτυο σωστή DR μετά την ανακατεύθυνση.

Μπορείτε να επιλέξετε την εικονική Μηχανή στο **VMM Cloud** ή την **Ομάδα "Προστασία"** για να ρυθμίσετε τις παραμέτρους δικτύου, όπως φαίνεται στην παρακάτω εικόνα.

![](./media/storsimple-dr-using-asr/image2.png)

## <a name="create-a-recovery-plan"></a>Δημιουργία σχεδίου ανάκτησης

Μπορείτε να δημιουργήσετε ένα σχέδιο αποκατάστασης στο ASR για να αυτοματοποιήσετε τη διαδικασία ανακατεύθυνσης κοινόχρηστα στοιχεία αρχείων. Εάν παρουσιαστεί διαταραχή, μπορείτε να μεταφέρετε τα κοινόχρηστα στοιχεία αρχείων προς τα επάνω σε λίγα λεπτά με ένα μόνο κλικ. Για να ενεργοποιήσετε αυτήν αυτοματισμού, θα χρειαστεί ένα λογαριασμό Azure αυτοματισμού.

#### <a name="to-create-the-account"></a>Για να δημιουργήσετε το λογαριασμό

1.  Μεταβείτε στην πύλη του Azure κλασική και μεταβείτε στην ενότητα **αυτοματισμού** .

1.  Δημιουργία νέου λογαριασμού αυτοματισμού. Διατηρήστε την επιλογή στην ίδια παν/περιοχή στην οποία έχουν δημιουργηθεί τους λογαριασμούς χώρου αποθήκευσης και StorSimple Cloud συσκευής.

2.  Κάντε κλικ στην επιλογή **Δημιουργία** &gt; **Εφαρμογή υπηρεσιών** &gt; **αυτοματισμού** &gt; **Runbook** &gt; **Από τη συλλογή** για να εισαγάγετε όλα τα απαιτούμενα runbooks στο λογαριασμό αυτοματισμού.

    ![](./media/storsimple-dr-using-asr/image3.png)

1.  Προσθέστε την ακόλουθη runbooks από το παράθυρο **Αποκατάσταση** στη συλλογή:

    -   Ανακατεύθυνση StorSimple ένταση κοντέινερ

    -   Εκκαθάριση του StorSimple όγκους μετά τη δοκιμή ανακατεύθυνσης (TFO)

    -   Όγκους ενεργοποίησης στη συσκευή StorSimple μετά την ανακατεύθυνση

    -   Έναρξη StorSimple εικονικού συσκευής

    -   Κατάργηση της εγκατάστασης επέκταση προσαρμοσμένων δεσμών ενεργειών σε Εικονική Azure

        ![](./media/storsimple-dr-using-asr/image4.png)


1.  Δημοσίευση όλες τις δέσμες ενεργειών επιλέγοντας runbook στο λογαριασμό αυτοματισμού και μεταβαίνοντας στην καρτέλα **συντάκτη** . Μετά από αυτό το βήμα, στην καρτέλα **Runbooks** θα εμφανίζονται ως εξής:

     ![](./media/storsimple-dr-using-asr/image5.png)

1.  Στο λογαριασμό αυτοματισμού, μεταβείτε στην καρτέλα **στοιχεία** , κάντε κλικ στην επιλογή **Προσθήκη ρύθμιση** &gt; **Προσθήκη διαπιστευτηρίων**, και προσθέστε τα διαπιστευτήριά σας Azure – όνομα παγίου AzureCredential.

    Χρησιμοποιήστε τα διαπιστευτήρια των Windows PowerShell. Αυτό πρέπει να είναι μια διαπιστευτηρίων που περιέχει ένα Αναγνωριστικό εταιρείας όνομα χρήστη και τον κωδικό πρόσβασης με πρόσβαση σε αυτήν τη συνδρομή Azure και έλεγχος ταυτότητας πολλών παραγόντων είναι απενεργοποιημένος. Αυτό είναι απαραίτητο για τον έλεγχο ταυτότητας εκ μέρους του χρήστη κατά τη διάρκεια του ανακατευθύνσεις και για να μεταφέρετε το όγκους διακομιστή αρχείων στην τοποθεσία DR.

1.  Στο λογαριασμό αυτοματισμού, επιλέξτε την καρτέλα **στοιχεία** και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη ρύθμιση** &gt; **μεταβλητή Προσθήκη** και προσθέστε τις ακόλουθες μεταβλητές. Μπορείτε να επιλέξετε για να κρυπτογραφήσετε αυτά τα στοιχεία. Αυτές οι μεταβλητές είναι αποκατάστασης πρόγραμμα – ειδικά. Εάν σας αποκατάστασης Σχεδιασμός (το οποίο θα δημιουργήσετε με το επόμενο βήμα) όνομα είναι TestPlan και, στη συνέχεια, θα πρέπει να σας μεταβλητές TestPlan StorSimRegKey, TestPlan AzureSubscriptionName και ούτω καθεξής.

    -   *RecoveryPlanName* **-StorSimRegKey**: το κλειδί εγγραφής για την υπηρεσία διαχείρισης StorSimple.

    -   *RecoveryPlanName* **-AzureSubscriptionName**: το όνομα της συνδρομής Azure.

    -   *RecoveryPlanName* **-ResourceName**: το όνομα του πόρου StorSimple που διαθέτει τη συσκευή StorSimple.

    -   *RecoveryPlanName* **-ΌνομαΣυσκευής**: τη συσκευή που πρέπει να είναι απέτυχε πάνω από.

    -   *RecoveryPlanName* **-TargetDeviceName**: το StorSimple Cloud συσκευής στην οποία είναι που αποτυγχάνουν επάνω από το κοντέινερ.

    -   *RecoveryPlanName* **-VolumeContainers**: μια συμβολοσειρά διαχωρισμένες με κόμματα ένταση κοντέινερ παρουσίαση από τη συσκευή που πρέπει να είναι απέτυχε μέσω; Για παράδειγμα, volcon1, volcon2, volcon3.

    -   RecoveryPlanName**-TargetDeviceDnsName**: το όνομα της υπηρεσίας της συσκευής προορισμού (αυτό βρίσκεται στην ενότητα **εικονική μηχανή** : το όνομα της υπηρεσίας είναι το ίδιο με το όνομα DNS).

    -   *RecoveryPlanName* **-StorageAccountName**: το όνομα του λογαριασμού χώρου αποθήκευσης με την οποία θα αποθηκευτεί η δέσμη ενεργειών (το οποίο πρέπει να εκτελείται σε το αποτυχίας μέσω Εικονική). Αυτό μπορεί να είναι οποιονδήποτε λογαριασμό χώρου αποθήκευσης που έχει κάποιο χώρο για να αποθηκεύσετε προσωρινά τη δέσμη ενεργειών.

    -   *RecoveryPlanName* **-StorageAccountKey**: το πλήκτρο πρόσβασης για το λογαριασμό χώρου αποθήκευσης παραπάνω.

    -   *RecoveryPlanName* **-ScriptContainer**: το όνομα του κοντέινερ στο οποίο θα αποθηκευτεί η δέσμη ενεργειών στο cloud. Εάν δεν υπάρχει το κοντέινερ, θα δημιουργηθεί.

    -   *RecoveryPlanName* **-VMGUIDS**: κατά την προστασία μια Εικονική, Επαναφορά τοποθεσίας Azure αντιστοιχίζει κάθε Εικονική ένα μοναδικό Αναγνωριστικό που θα σας δώσει τις λεπτομέρειες της το αποτυχίας μέσω Εικονική. Για να αποκτήσετε το VMGUID, επιλέξτε την καρτέλα **Υπηρεσίες ανάκτησης** και, στη συνέχεια, κάντε κλικ στο **Στοιχείο προστασία** &gt; **Ομάδες προστασίας** &gt; **μηχανές** &gt; **Ιδιότητες**. Εάν έχετε πολλές ΣΠΣ, στη συνέχεια, προσθέστε τα GUID ως συμβολοσειρά διαχωρισμένες με κόμματα.

    -   *RecoveryPlanName* **-AutomationAccountName** -το όνομα του λογαριασμού αυτοματισμού στην οποία έχετε προσθέσει το runbooks και τα στοιχεία.

    Για παράδειγμα, το όνομα του σχεδίου αποκατάστασης είναι fileServerpredayRP, στη συνέχεια, την καρτέλα **στοιχεία** πρέπει να εμφανίζεται ως εξής μετά την προσθήκη όλων των περιουσιακών στοιχείων.

    ![](./media/storsimple-dr-using-asr/image6.png)


1.  Μεταβείτε στην ενότητα **Υπηρεσίες ανάκτησης** και επιλέξτε το θάλαμο Επαναφορά τοποθεσίας Azure που δημιουργήσατε νωρίτερα.

2.  Επιλέξτε την καρτέλα **Προγράμματος αποκατάστασης** και δημιουργήστε ένα νέο πρόγραμμα αποκατάστασης ως εξής:

    μια.  Καθορίστε ένα όνομα και επιλέξτε την κατάλληλη **Ομάδα "Προστασία"**.

    β.  Επιλέξτε το ΣΠΣ από την ομάδα προστασίας που θέλετε να συμπεριλάβετε στο πρόγραμμα αποκατάστασης.

    c.  Μετά την ανάκτηση δημιουργείται πρόγραμμα, επιλέξτε το για να ανοίξετε την προβολή προσαρμογής πρόγραμμα αποκατάστασης.

    d.  Επιλέξτε **όλα τερματισμού ομάδες**, κάντε κλικ στην επιλογή **δέσμης ενεργειών**και επιλέξτε **Προσθήκη μιας δέσμης ενεργειών του πρωτεύοντος πριν από τον τερματισμό όλων ομάδας**.

    ε.  Επιλέξτε το λογαριασμό αυτοματισμού (στο οποίο προσθέσατε το runbooks) και, στη συνέχεια, επιλέξτε runbook **αποτύχει επάνω-StorSimple-ένταση-κοντέινερ** .

    ε.  Κάντε κλικ στην επιλογή **ομάδα 1: Έναρξη**, επιλέξτε **εικονικές μηχανές**και η προσθήκη του ΣΠΣ που είναι να προστατεύονται στο πρόγραμμα αποκατάστασης.

    γ.  Κάντε κλικ στην επιλογή **ομάδα 1: Έναρξη**, επιλέξτε **δέσμης ενεργειών**και να προσθέσετε όλες τις παρακάτω δέσμες ενεργειών σε σειρά ως βήματα **μετά την ομάδα 1** .

    - Έναρξη-StorSimple-εικονικού-συσκευής runbook
    - Αποτυχία runbook επάνω-StorSimple-ένταση-κοντέινερ
    - Runbook ενεργοποίησης όγκους μετά την ανακατεύθυνση
    - Κατάργηση της εγκατάστασης-προσαρμοσμένη-δέσμης ενεργειών-επέκταση runbook

1.  Προσθέστε μια μη αυτόματη ενέργεια μετά από τις παραπάνω 4 δέσμες ενεργειών στο ίδιο **ομάδα 1: βήματα μετά τη** ενότητας. Αυτή η ενέργεια είναι το σημείο στο οποίο μπορείτε να επαληθεύσετε ότι όλα λειτουργούν σωστά. Αυτή η ενέργεια πρέπει να προστεθεί μόνο ως μέρος της δοκιμής ανακατεύθυνσης (επομένως, επιλέξτε μόνο το **Δοκιμή ανακατεύθυνσης** πλαίσιο ελέγχου).

2.  Μετά τη μη αυτόματη ενέργεια, προσθέστε τη δέσμη ενεργειών εκκαθάριση χρησιμοποιώντας την ίδια διαδικασία που χρησιμοποιήσατε για τα άλλα runbooks. Αποθήκευση του σχεδίου αποκατάστασης.

    > [AZURE.NOTE] Κατά την εκτέλεση δοκιμής ανακατεύθυνσης, πρέπει να επαληθεύσετε όλα τα στοιχεία στο βήμα μη αυτόματη ενέργεια, επειδή το όγκους StorSimple που είχαν έχει κλωνοποιηθεί στη συσκευή προορισμού θα διαγραφεί ως μέρος της την εκκαθάριση μετά την ολοκλήρωση της μη αυτόματης ενέργειας.

    ![](./media/storsimple-dr-using-asr/image7.png)

## <a name="perform-a-test-failover"></a>Εκτέλεση δοκιμής ανακατεύθυνσης

Ανατρέξτε στον Οδηγό συνοδευτικό [Active Directory DR λύση](../site-recovery/site-recovery-active-directory.md) για το συγκεκριμένο υποδείξεις σχετικά με την υπηρεσία καταλόγου Active Directory κατά τη διάρκεια του εφεδρικού δοκιμής. Το εσωτερικής εγκατάστασης δεν υπάρχει διαταραχή καθόλου όταν πραγματοποιείται η δοκιμή ανακατεύθυνσης. Ο όγκος StorSimple που έχουν επισυναφθεί σε η Εικονική εσωτερικής εγκατάστασης είναι κλωνοποιηθεί στη συσκευή Cloud StorSimple στην Azure. Μια Εικονική για σκοπούς δοκιμής ενημερώνεται στο Azure και το κλωνοποιημένο όγκους επισυνάπτονται σε η Εικονική.

#### <a name="to-perform-the-test-failover"></a>Για να εκτελέσετε την ανακατεύθυνση δοκιμής

1.  Στην κλασική Azure πύλη, επιλέξτε σας θάλαμο αποκατάστασης τοποθεσίας.

1.  Κάντε κλικ στην επιλογή το σχέδιο αποκατάστασης που δημιουργήσατε για το διακομιστή αρχείων Εικονική.

2.  Κάντε κλικ στην επιλογή **Δοκιμή ανακατεύθυνσης**.

3.  Επιλέξτε το εικονικό δίκτυο για να ξεκινήσετε τη διαδικασία ανακατεύθυνσης δοκιμής.

    ![](./media/storsimple-dr-using-asr/image8.png)

1.  Όταν το δευτερεύον περιβάλλον προς τα επάνω, μπορείτε να εκτελέσετε επικυρώσεων σας.

2.  Μετά την ολοκλήρωση του επικυρώσεων, κάντε κλικ στην επιλογή **Ολοκλήρωση επικυρώσεις**. Θα είναι η εκκαθάριση του περιβάλλοντος ανακατεύθυνσης δοκιμής και θα ολοκληρωθεί η λειτουργία TFO.

## <a name="perform-an-unplanned-failover"></a>Εκτελέστε μια μη προγραμματισμένη ανακατεύθυνσης

Κατά την μια μη προγραμματισμένη ανακατεύθυνση, το όγκους StorSimple είναι απέτυχε πάνω από εικονική συσκευή, μια Εικονική ρεπλίκα γνωστοποιείται προς τα επάνω στο Azure και την όγκους επισυνάπτονται σε η Εικονική.

#### <a name="to-perform-an-unplanned-failover"></a>Για να εκτελέσετε μια μη προγραμματισμένη ανακατεύθυνσης

1.  Στην κλασική Azure πύλη, επιλέξτε σας θάλαμο αποκατάστασης τοποθεσίας.

1.  Κάντε κλικ στο πρόγραμμα ανάκτησης που έχει δημιουργηθεί για διακομιστή αρχείων Εικονική.

2.  Κάντε κλικ στην επιλογή **ανακατεύθυνση** και, στη συνέχεια, επιλέξτε **Μη προγραμματισμένη ανακατεύθυνσης**.

    ![](./media/storsimple-dr-using-asr/image9.png)

1.  Επιλέξτε το δίκτυο προορισμού και, στη συνέχεια, κάντε κλικ στο εικονίδιο ελέγχου ✓ για να ξεκινήσετε τη διαδικασία ανακατεύθυνσης.

## <a name="perform-a-planned-failover"></a>Εκτελέστε μια προγραμματισμένη ανακατεύθυνσης

Κατά τη διάρκεια της προγραμματισμένης ανακατεύθυνσης, το εσωτερικής εγκατάστασης αρχείων διακομιστή Εικονική τερματίζεται ομαλά και ένα σύννεφο λαμβάνεται αντιγράφου ασφαλείας στιγμιότυπο των το όγκους στη συσκευή StorSimple. Ο όγκος StorSimple είναι απέτυχε πάνω από εικονική συσκευή, μια Εικονική ρεπλίκα ενημερώνεται στην Azure και το όγκους επισυνάπτονται σε η Εικονική.

#### <a name="to-perform-a-planned-failover"></a>Για να εκτελέσετε μια προγραμματισμένη ανακατεύθυνσης

1.  Στην κλασική Azure πύλη, επιλέξτε σας θάλαμο αποκατάστασης τοποθεσίας.

1.  Κάντε κλικ στην επιλογή το σχέδιο αποκατάστασης που δημιουργήσατε για το διακομιστή αρχείων Εικονική.

2.  Κάντε κλικ στην επιλογή **ανακατεύθυνση** και, στη συνέχεια, επιλέξτε **Προγραμματισμένη ανακατεύθυνσης**.

3.  Επιλέξτε το δίκτυο προορισμού και, στη συνέχεια, κάντε κλικ στο εικονίδιο ελέγχου ✓ για να ξεκινήσετε τη διαδικασία ανακατεύθυνσης.

## <a name="perform-a-failback"></a>Εκτελέστε μια αποκατάσταση μετά από

Κατά τη διάρκεια μιας αποκατάστασης μετά από αποτυχία, StorSimple ένταση κοντέινερ είναι απέτυχε μέσω ξανά η φυσική συσκευή μετά που έχετε δημιουργήσει ένα αντίγραφο ασφαλείας.

#### <a name="to-perform-a-failback"></a>Για να εκτελέσετε μια αποκατάσταση μετά από

1.  Στην κλασική Azure πύλη, επιλέξτε σας θάλαμο αποκατάστασης τοποθεσίας.

1.  Κάντε κλικ στην επιλογή το σχέδιο αποκατάστασης που δημιουργήσατε για το διακομιστή αρχείων Εικονική.

2.  Κάντε κλικ στην επιλογή **ανακατεύθυνση** και επιλέξτε **ανακατεύθυνσης προγραμματισμένη** ή **μη προγραμματισμένη ανακατεύθυνσης**.

3.  Κάντε κλικ στην επιλογή **Αλλαγή κατεύθυνσης**.

4.  Επιλέξτε το κατάλληλο δεδομένων συγχρονισμού και επιλογές για τη δημιουργία Εικονική.

5.  Κάντε κλικ στο εικονίδιο ελέγχου ✓ για να ξεκινήσετε τη διαδικασία αποκατάστασης μετά από αποτυχία.

    ![](./media/storsimple-dr-using-asr/image10.png)

## <a name="best-practices"></a>Βέλτιστες πρακτικές

### <a name="capacity-planning-and-readiness-assessment"></a>Αξιολόγηση χωρητικότητας σχεδιασμού και ετοιμότητας


#### <a name="hyper-v-site"></a>Τοποθεσία Hyper-V

Χρησιμοποιήστε το [εργαλείο οργάνωση χωρητικότητα χρήστη](http://www.microsoft.com/download/details.aspx?id=39057) για να σχεδιάσετε το διακομιστή, αποθήκευση και την υποδομή δικτύου σας για το περιβάλλον ρεπλίκα Hyper-V.

#### <a name="azure"></a>Azure

Μπορείτε να εκτελέσετε το [εργαλείο αξιολόγηση ετοιμότητας εικονική μηχανή Azure](http://azure.microsoft.com/downloads/vm-readiness-assessment/) σε VM για να βεβαιωθείτε ότι είναι συμβατά με το Azure ΣΠΣ και υπηρεσίες ανάκτησης Azure τοποθεσίας. Το εργαλείο αξιολόγηση ετοιμότητας ελέγχει ρυθμίσεις παραμέτρων Εικονική και προειδοποιεί όταν οι ρυθμίσεις παραμέτρων είναι συμβατή με Azure. Για παράδειγμα, εμφανίζει μια προειδοποίηση εάν μια μονάδα δίσκου C: είναι μεγαλύτερο από 127 GB.


Σχεδιασμός δυνατοτήτων αποτελείται από τουλάχιστον δύο σημαντικά διεργασίες:

-   Αντιστοίχιση εσωτερικής ΣΠΣ Hyper-V για να μεγέθη Εικονική Azure (όπως A6, A7, A8 και A9).

-   Καθορισμός το εύρος ζώνης Internet που απαιτείται.

## <a name="limitations"></a>Περιορισμοί

- Προς το παρόν, μπορεί να αποτύχουν μόνο 1 StorSimple συσκευή μέσω (σε μια μεμονωμένη συσκευή Cloud StorSimple). Το σενάριο από ένα διακομιστή αρχείων που εκτείνεται σε πολλές συσκευές StorSimple δεν υποστηρίζεται ακόμη.

- Εάν λάβετε ένα μήνυμα σφάλματος κατά την ενεργοποίηση προστασίας για μια Εικονική, βεβαιωθείτε ότι έχετε αποσυνδεθεί τους στόχους iSCSI.

- Όλα τα δοχεία τόμου που έχουν ομαδοποιηθεί μαζί λόγω δημιουργίας αντιγράφων ασφαλείας πολιτικές διεύρυνση σε κοντέινερ ένταση θα αποτύχει μέσω μαζί.

- Όλα τα όγκους στα δοχεία τόμου που έχετε επιλέξει θα αποτύχει πάνω από.

- Όγκους που προστίθενται περισσότερες από 64 TB δεν μπορεί να αποτύχει πάνω από επειδή τη μέγιστη χωρητικότητα μία συσκευής Cloud StorSimple είναι 64 TB.

- Εάν αποτύχει η προγραμματισμένη/μη προγραμματισμένη ανακατεύθυνσης και του ΣΠΣ δημιουργούνται στο Azure, στη συνέχεια, εκκαθαρίζει του ΣΠΣ. Αντί για αυτό, κάντε μια αποκατάσταση μετά από. Εάν διαγράψετε το ΣΠΣ, στη συνέχεια, του ΣΠΣ εσωτερικής εγκατάστασης δεν είναι δυνατό να ενεργοποιηθεί ξανά.

- Μετά ανακατεύθυνσης, εάν δεν μπορείτε να δείτε το όγκους, μεταβείτε στο του ΣΠΣ, ανοίξτε τη Διαχείριση δίσκων, επαναλάβετε τη σάρωση των δίσκων, και, στη συνέχεια, να τα μεταφέρετε online.

- Σε ορισμένες περιπτώσεις, τα γράμματα μονάδας δίσκου στην τοποθεσία DR μπορεί να είναι διαφορετικά από τα γράμματα εσωτερικής εγκατάστασης. Αν συμβεί αυτό, θα πρέπει να διορθώσετε με μη αυτόματο τρόπο το πρόβλημα μετά την ολοκλήρωση του εφεδρικού.

- Έλεγχος ταυτότητας πολλών παραγόντων θα πρέπει να έχει απενεργοποιηθεί για τα διαπιστευτήρια Azure που έχει εισαχθεί στο λογαριασμό αυτοματισμού ως ενός περιουσιακού στοιχείου. Εάν αυτός ο έλεγχος ταυτότητας δεν είναι απενεργοποιημένη, δεσμών ενεργειών δεν θα επιτρέπεται να εκτελείται αυτόματα και το πρόγραμμα αποκατάστασης θα αποτύχει.

- Χρονικό όριο εκτύπωσης ανακατεύθυνσης: το StorSimple δέσμης ενεργειών θα λήξει το χρονικό όριο εάν την ανακατεύθυνση της έντασης ήχου κοντέινερ διαρκεί περισσότερο από το όριο Επαναφορά τοποθεσίας Azure ανά δέσμης ενεργειών (προς το παρόν 120 λεπτά).

- Το χρονικό όριο εργασίας δημιουργίας αντιγράφων ασφαλείας: το StorSimple δέσμης ενεργειών λήγει το χρονικό όριο εάν το αντίγραφο ασφαλείας της όγκους διαρκεί περισσότερο από το όριο Επαναφορά τοποθεσίας Azure ανά δέσμης ενεργειών (προς το παρόν 120 λεπτά).
 
    > [AZURE.IMPORTANT] Εκτελέστε το αντίγραφο ασφαλείας με μη αυτόματο τρόπο από την πύλη του Azure και, στη συνέχεια, εκτελέστε ξανά το πρόγραμμα αποκατάστασης.

- Δημιουργία διπλότυπου χρονικό όριο εκτύπωσης: το StorSimple δέσμης ενεργειών λήγει το χρονικό όριο εάν η κλωνοποίηση όγκους διαρκεί περισσότερο από το όριο Επαναφορά τοποθεσίας Azure ανά δέσμης ενεργειών (προς το παρόν 120 λεπτά).

- Χρόνος σφάλμα συγχρονισμού: το StorSimple δέσμες ενεργειών σφάλματα ανάληψη πληροφορεί ότι τα αντίγραφα ασφαλείας ήταν επιτυχής, παρόλο που το αντίγραφο ασφαλείας είναι επιτυχής στην πύλη του. Μια πιθανή αιτία για αυτό μπορεί να είναι ότι η ώρα της συσκευής StorSimple μπορεί να είναι εκτός συγχρονισμού με την τρέχουσα ώρα στη ζώνη ώρας.
 
    > [AZURE.IMPORTANT] Συγχρονισμός την ώρα της συσκευής με την τρέχουσα ώρα στη ζώνη ώρας.

- Σφάλμα ανακατεύθυνσης συσκευής: το StorSimple δέσμης ενεργειών μπορεί να αποτύχει, εάν υπάρχει μια ανακατεύθυνσης συσκευής όταν εκτελείται το πρόγραμμα αποκατάστασης.
    
    > [AZURE.IMPORTANT] Εκτελέστε ξανά το πρόγραμμα αποκατάστασης μετά την ολοκλήρωση του εφεδρικού συσκευής.

## <a name="summary"></a>Σύνοψη

Χρησιμοποιώντας την αποκατάσταση Azure τοποθεσίας, μπορείτε να δημιουργήσετε ένα σχέδιο αποκατάστασης ολοκλήρωσης αυτοματοποιημένη καταστροφής για διακομιστή αρχείων Εικονική αντιμετωπίζετε κοινόχρηστα στοιχεία αρχείων που φιλοξενούνται στο χώρο αποθήκευσης StorSimple. Μπορείτε να ξεκινήσετε την ανακατεύθυνση μέσα σε δευτερόλεπτα από οπουδήποτε σε περίπτωση μια διακοπή και να λάβετε την εφαρμογή λειτουργία μετά από μερικά λεπτά.
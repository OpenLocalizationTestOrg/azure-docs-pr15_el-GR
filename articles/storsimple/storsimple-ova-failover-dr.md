<properties
   pageTitle="Καταστροφή αποκατάστασης και συσκευή ανακατεύθυνσης για τον πίνακα εικονικού StorSimple"
   description="Μάθετε περισσότερα σχετικά με τον τρόπο ανακατεύθυνσης σας StorSimple εικονικό πίνακα."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array"></a>Καταστροφή αποκατάστασης και συσκευή ανακατεύθυνσης για τον πίνακα εικονικού StorSimple


## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει την αποκατάσταση για το Microsoft Azure StorSimple εικονικού πίνακα (γνωστό και ως το StorSimple εσωτερικής εικονική συσκευή) όπως τα λεπτομερή βήματα που απαιτούνται για την ανακατεύθυνση σε άλλη συσκευή εικονικού στην περίπτωση μιας καταστροφής. Ανακατεύθυνσης σάς επιτρέπει να μετεγκαταστήσετε τα δεδομένα σας από μια συσκευή *προέλευσης* στο κέντρο δεδομένων σε άλλη συσκευή *προορισμού* που βρίσκεται στο ίδιο ή σε μια διαφορετική γεωγραφική θέση. Είναι η ανακατεύθυνση συσκευής για ολόκληρη τη συσκευή. Κατά την ανακατεύθυνση, κατοχή αλλάζουν τα δεδομένα cloud για τη συσκευή προέλευσης που της συσκευής προορισμού.

Ανακατεύθυνση συσκευή είναι orchestrated μέσω της δυνατότητας αποκατάστασης (DR) καταστροφής και ξεκινά από τη σελίδα **συσκευές** . Αυτή η σελίδα Διαχείριση όλες τις συσκευές StorSimple συνδεδεμένο με την υπηρεσία StorSimple Manager. Για κάθε συσκευή, εμφανίζονται τα φιλικό όνομα, κατάσταση, χωρητικότητα προμήθεια του φακέλου και το μέγιστο, τύπος και μοντέλο.

![](./media/storsimple-ova-failover-dr/image15.png)

Σε αυτό το άρθρο ισχύει για πίνακες εικονικού StorSimple μόνο. Αποτυχία επάνω σε μια συσκευή 8000 σειράς, μεταβείτε στο [ανακατεύθυνσης και αποκατάσταση της συσκευής σας StorSimple](storsimple-device-failover-disaster-recovery.md).


## <a name="what-is-disaster-recovery"></a>Τι είναι η αποκατάσταση;

Σε ένα σενάριο αποκατάστασης (DR) από καταστροφή, την κύρια συσκευή σταματά να λειτουργεί. Σε αυτήν την περίπτωση, μπορείτε να μετακινήσετε τα δεδομένα cloud που σχετίζονται με τη συσκευή απέτυχε σε άλλη συσκευή, χρήση την κύρια συσκευή σας ως *αρχείο προέλευσης* και καθορίζοντας μια άλλη συσκευή ως *προορισμού*. Αυτή η διαδικασία αναφέρεται ως του *ανακατεύθυνσης*. Κατά την ανακατεύθυνση, όλα τα όγκους ή τα κοινόχρηστα στοιχεία από τη συσκευή προέλευσης αλλαγή της κατοχής και μεταβιβάζονται στη συσκευή προορισμού. Επιτρέπεται χωρίς φιλτράρισμα των δεδομένων.

DR διαμορφώνεται ως επαναφορά πλήρους συσκευής χρησιμοποιώντας το θερμότητας που βασίζονται σε χάρτη δημιουργία στοιβάδων και παρακολούθηση. Ένα χάρτη θερμότητας ορίζεται αντιστοιχίζοντας μια τιμή θερμότητας με τα δεδομένα που βασίζονται σε ανάγνωση και εγγραφή μοτίβα. Αυτό θερμότητας αντιστοίχιση, στη συνέχεια, βαθμίδες τη χαμηλότερη μπλοκ δεδομένων θερμότητας στο cloud πρώτα διατηρώντας το υψηλό θερμότητας (πιο χρησιμοποιείται) μπλοκ δεδομένων στο τοπικό επίπεδο. Κατά τη διάρκεια μιας DR, χάρτη θερμότητας χρησιμοποιείται για να επαναφέρετε και rehydrate τα δεδομένα από το cloud. Η συσκευή λαμβάνει όλων των όγκους/μετοχών στο τελευταίο αντίγραφο ασφαλείας πρόσφατα (όπως καθορίζεται εσωτερικά) και εκτελεί μια επαναφορά από αυτό το αντίγραφο ασφαλείας. Ολόκληρη τη διαδικασία DR είναι orchestrated από τη συσκευή.


## <a name="prerequisites-for-device-failover"></a>Προαπαιτούμενα στοιχεία για τη συσκευή ανακατεύθυνσης


### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για ανακατεύθυνση οποιαδήποτε συσκευή, θα πρέπει να πληρούνται οι ακόλουθες προϋποθέσεις:

- Η συσκευή προέλευσης πρέπει να βρίσκεται σε κατάσταση **Deactivated** .

- Η συσκευή προορισμού πρέπει να εμφανίζεται ως **ενεργός** στην πύλη του Azure κλασική. Θα πρέπει να παρέχετε εικονική συσκευή προορισμού της δυναμικότητας ίδια ή νεότερη έκδοση. Μπορείτε, στη συνέχεια, πρέπει να χρησιμοποιήσετε το τοπικό web περιβάλλοντος εργασίας Χρήστη για να ρυθμίσετε τις παραμέτρους και καταχώρηση με επιτυχία την εικονική συσκευή.

    > [AZURE.IMPORTANT] Μην επιχειρήσετε για να ρυθμίσετε το καταχωρημένες εικονική συσκευή μέσω της υπηρεσίας κάνοντας κλικ στην επιλογή **εγκατάσταση ολοκλήρωσης συσκευής**. Ρύθμιση των παραμέτρων δεν συσκευή πρέπει να εκτελεστούν μέσω της υπηρεσίας.

- Η συσκευή προέλευσης και προορισμού πρέπει να είναι του ίδιου τύπου. Μπορείτε μόνο μπορεί να αποτύχει επάνω από μια εικονική συσκευή έχει ρυθμιστεί ως διακομιστής αρχείων σε άλλον διακομιστή του αρχείου. Το ίδιο ισχύει για έναν διακομιστή iSCSI.

- Για διακομιστή αρχείων DR, συνιστάται να συμμετέχετε στη συσκευή προορισμού στον ίδιο τομέα ως που της προέλευσης, έτσι ώστε τα δικαιώματα κοινής χρήσης επιλύονται αυτόματα. Σε αυτήν την έκδοση υποστηρίζεται μόνο η ανακατεύθυνση σε μια συσκευή προορισμού στον ίδιο τομέα.

### <a name="other-considerations"></a>Άλλα θέματα

- Συνιστάται να λαμβάνουν όλες τις όγκους ή κοινόχρηστα στοιχεία στη συσκευή προέλευσης για εργασία χωρίς σύνδεση.

- Εάν είναι προγραμματισμένη ανακατεύθυνσης, συνιστάται να που λαμβάνουν ένα αντίγραφο ασφαλείας της συσκευής και, στη συνέχεια, συνεχίστε με την ανακατεύθυνση για να ελαχιστοποιήσετε την απώλεια δεδομένων. Εάν πρόκειται για μια μη προγραμματισμένη ανακατεύθυνσης, θα χρησιμοποιηθεί το πιο πρόσφατο αντίγραφο ασφαλείας για να επαναφέρετε τη συσκευή.

- Οι συσκευές διαθέσιμες προορισμού για DR είναι συσκευές που έχει τη δυνατότητα ίδια ή μεγαλύτερο σε σύγκριση με τη συσκευή προέλευσης. Οι συσκευές που συνδέονται με την υπηρεσία, αλλά δεν πληροί τα κριτήρια του αρκετό χώρο δεν θα είναι διαθέσιμο ως συσκευές προορισμού.

### <a name="dr-prechecks"></a>DR prechecks

Πριν αρχίσει η DR, prechecks εκτελούνται στη συσκευή. Αυτοί οι έλεγχοι να εξασφαλίσετε ότι δεν υπάρχουν σφάλματα θα παρουσιαστεί κατά αρχίζει DR. Το prechecks περιλαμβάνουν τα εξής:

- Επικύρωση το λογαριασμό χώρου αποθήκευσης

- Έλεγχος τη συνδεσιμότητα cloud για να Azure

- Έλεγχος διαθέσιμο χώρο στη συσκευή προορισμού

- Έλεγχος εάν μια συσκευή iSCSI διακομιστή προέλευσης έχει έγκυρα ονόματα ACR, IQN (δεν υπερβαίνει 220 χαρακτήρες) και τον κωδικό πρόσβασης CHAP (12 έως 16 χαρακτήρες) που σχετίζεται με το όγκους

Εάν οποιοδήποτε από τα παραπάνω prechecks αποτύχει, δεν μπορείτε να συνεχίσετε με το DR. Πρέπει να επιλύσετε αυτά τα θέματα και, στη συνέχεια, προσπαθήστε ξανά να DR.

Αφού το DR ολοκληρώθηκε με επιτυχία, η κατοχή των δεδομένων cloud στη συσκευή προέλευσης μεταφέρεται στη συσκευή προορισμού. Η συσκευή προέλευσης, στη συνέχεια, δεν είναι πλέον διαθέσιμη στην πύλη του. Πρόσβαση σε όλα τα όγκους/κοινόχρηστα στοιχεία στη συσκευή προέλευσης έχει αποκλειστεί και τη συσκευή προορισμού γίνεται ενεργή.

> [AZURE.IMPORTANT]
> 
> Αν η συσκευή δεν είναι πλέον διαθέσιμο, η εικονική μηχανή που παροχή της υπηρεσίας στο σύστημα υποδοχής εξακολουθεί να καταναλώνει πόρους. Όταν ολοκληρωθεί με επιτυχία το DR, μπορείτε να διαγράψετε αυτόν τον υπολογιστή εικονικές από το σύστημα κεντρικού υπολογιστή.

## <a name="fail-over-to-a-virtual-array"></a>Αποτυχία πάνω από έναν πίνακα, εικονικό

Συνιστάται να έχετε ένα άλλο πίνακα StorSimple εικονικού παροχή της υπηρεσίας και ρύθμιση των παραμέτρων μέσω του τοπικού web περιβάλλοντος εργασίας Χρήστη που έχουν καταχωρηθεί της υπηρεσίας διαχείρισης StorSimple πριν από την εκτέλεση αυτής της διαδικασίας.


> [AZURE.IMPORTANT]
> 
> - Δεν επιτρέπεται να ανακατευθύνει από μια συσκευή σειράς StorSimple 8000 σε μια συσκευή εικονικού 1200.
> - Μπορεί να αποτύχει επάνω από μια συσκευή εικονικού Federal πληροφορίες επεξεργασίας τυπική (FIPS) με δυνατότητα αναπτυχθεί στην πύλη για δημόσιους οργανισμούς σε μια εικονική συσκευή στην πύλη Azure κλασική. Ισχύει επίσης το αντίστροφο.

Ακολουθήστε τα παρακάτω βήματα για να επαναφέρετε τη συσκευή σε μια συσκευή εικονικού StorSimple προορισμού.

1. Λαμβάνουν όγκους/κοινόχρηστα στοιχεία χωρίς σύνδεση στον κεντρικό υπολογιστή. Ανατρέξτε στις οδηγίες λειτουργικό σύστημα – ειδικά στον κεντρικό υπολογιστή για να μεταφέρετε όγκους μετοχών εκτός σύνδεσης. Εάν δεν ήδη για εργασία χωρίς σύνδεση, θα χρειαστεί να ακολουθήσετε όλων των όγκους/μετοχών χωρίς σύνδεση στη συσκευή, μεταβαίνοντας **συσκευές > μετοχές** (ή **συσκευή > όγκους**). Επιλέξτε μια κοινή χρήση/όγκο και κάντε κλικ στην επιλογή **λήψη για εργασία χωρίς σύνδεση** στο κάτω μέρος της σελίδας. Όταν σας ζητηθεί επιβεβαίωση, κάντε κλικ στο κουμπί **Ναι**. Επαναλάβετε αυτήν τη διαδικασία για όλα τα κοινόχρηστα στοιχεία/όγκους στη συσκευή.

2. Στη σελίδα **συσκευές** , επιλέξτε τη συσκευή προέλευσης για ανακατεύθυνση και κάντε κλικ στην επιλογή **Απενεργοποίηση**. 
    ![](./media/storsimple-ova-failover-dr/image16.png)

3. Θα σας ζητηθεί επιβεβαίωση. Απενεργοποίηση συσκευή είναι μια διαδικασία μόνιμο που δεν είναι δυνατή η αναίρεση. Θα γίνει υπενθύμιση επίσης να ακολουθήσετε τις μετοχές/όγκους χωρίς σύνδεση στον κεντρικό υπολογιστή.

    ![](./media/storsimple-ova-failover-dr/image18.png)

3. Κατά την επιβεβαίωση, θα ξεκινήσει η απενεργοποίηση. Μετά την απενεργοποίηση ολοκληρώθηκε με επιτυχία, θα ειδοποιηθείτε.

    ![](./media/storsimple-ova-failover-dr/image19.png)

4. Στη σελίδα **συσκευές** , την κατάσταση της συσκευής τώρα θα αλλάξει **Deactivated**.

    ![](./media/storsimple-ova-failover-dr/image20.png)

5. Επιλέξτε τη συσκευή απενεργοποιημένη και στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **ανακατεύθυνσης**.

6. Στον Οδηγό ανακατεύθυνσης επιβεβαίωση που ανοίγει, κάντε τα εξής:

    1. Από την αναπτυσσόμενη λίστα με τις διαθέσιμες συσκευές, επιλέξτε μια **συσκευή προορισμού.** Μόνο τις συσκευές που έχετε επαρκή δυναμικότητα εμφανίζονται στην αναπτυσσόμενη λίστα.

    2. Εξετάστε τις λεπτομέρειες που σχετίζονται με τη συσκευή προέλευσης, όπως το όνομα της συσκευής, η συνολική χωρητικότητα και τα ονόματα των τα κοινόχρηστα στοιχεία που θα αποτύχει πάνω από.

        ![](./media/storsimple-ova-failover-dr/image21.png)

7. Επιλέξτε **να συμφωνούν ότι ανακατεύθυνσης είναι μια λειτουργία μόνιμο και αφού του εφεδρικού ολοκληρώθηκε με επιτυχία, η συσκευή προέλευσης θα διαγραφεί**.

8. Κάντε κλικ στο εικονίδιο ελέγχου ![](./media/storsimple-ova-failover-dr/image1.png).


9. Κινείται ένα έργο ανακατεύθυνσης και θα ειδοποιηθείτε. Κάντε κλικ στην **προβολή έργου** για την παρακολούθηση του εφεδρικού.

    ![](./media/storsimple-ova-failover-dr/image22.png)

10. Στη σελίδα **εργασίες** , θα δείτε μια εργασία ανακατεύθυνσης που έχει δημιουργηθεί για τη συσκευή προέλευσης. Αυτή η εργασία εκτελεί την prechecks DR.

    ![](./media/storsimple-ova-failover-dr/image23.png)

    Μετά την prechecks DR είναι επιτυχής, η εργασία ανακατεύθυνσης θα δημιουργούν εργασιών επαναφοράς για κάθε κοινή χρήση/τόμου που υπάρχει στη συσκευή σας προέλευσης.

    ![](./media/storsimple-ova-failover-dr/image24.png)

11. Μετά την ολοκλήρωση του εφεδρικού, μεταβείτε στη σελίδα **συσκευές** .

    μια. Επιλέξτε τη συσκευή εικονικού StorSimple που χρησιμοποιήθηκε ως τη συσκευή προορισμού για τη διαδικασία ανακατεύθυνσης.

    β. Μεταβείτε στη σελίδα **κοινόχρηστα στοιχεία** (ή **όγκους** εάν iSCSI server). Τώρα θα πρέπει να παρατίθενται όλα τα κοινόχρηστα στοιχεία (όγκους) από το παλιό συσκευή.
    
    ![](./media/storsimple-ova-failover-dr/image25.png)

![](./media/storsimple-ova-failover-dr/video_icon.png)**Το βίντεο**

Αυτό το βίντεο δείχνει πώς μπορείτε να ανακατευθύνει StorSimple εσωτερικής εικονική συσκευή σε άλλη συσκευή εικονικού.

> [AZURE.VIDEO storsimple-virtual-array-disaster-recovery]

## <a name="business-continuity-disaster-recovery-bcdr"></a>Επιχειρήσεις συνέχειας αποκατάσταση (BCDR)

Ένα σενάριο αποκατάστασης (BCDR) επιχειρήσεις συνέχειας καταστροφή παρουσιάζεται όταν το κέντρο δεδομένων ολόκληρο Azure σταματά να λειτουργεί. Αυτό μπορεί να επηρεάσει την υπηρεσία διαχείρισης StorSimple και τις σχετικές συσκευές StorSimple.

Εάν υπάρχουν StorSimple συσκευές που έχουν καταχωρηθεί ακριβώς πριν από την εμφάνιση μιας καταστροφής, στη συνέχεια, αυτές τις συσκευές StorSimple ίσως χρειαστεί να διαγραφεί. Μετά την καταστροφή, μπορείτε να δημιουργήσετε ξανά και να ρυθμίσετε αυτές τις συσκευές.

## <a name="errors-during-dr"></a>Σφάλματα κατά τη διάρκεια DR

**Μη διαθεσιμότητα συνδεσιμότητας cloud κατά τη διάρκεια DR**

Εάν η σύνδεση cloud διακοπεί αφού έχει ξεκινήσει DR και πριν να ολοκληρωθεί η επαναφορά συσκευή, θα αποτύχει η DR και θα ειδοποιηθείτε. Η συσκευή προορισμού που χρησιμοποιήθηκε για DR, στη συνέχεια, έχει επισημανθεί ως *χωρίς δυνατότητα χρήσης.* Στην ίδια συσκευή προορισμού δεν μπορούν να χρησιμοποιηθούν, στη συνέχεια, για μελλοντική DRs.

**Δεν υπάρχουν συσκευές συμβατά προορισμού**

Εάν δεν έχετε αρκετό χώρο τις συσκευές διαθέσιμες προορισμού, θα δείτε ένα σφάλμα στο εφέ που υπάρχουν προορισμού συμβατές συσκευές.

**Precheck αποτυχίες**

Εάν ένα από τα prechecks δεν πληρούνται, στη συνέχεια, θα βλέπετε αποτυχίες precheck.

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τον τρόπο για να [διαχειριστείτε το εικονικό πίνακα StorSimple χρήση του τοπικού web περιβάλλοντος εργασίας Χρήστη](storsimple-ova-web-ui-admin.md).
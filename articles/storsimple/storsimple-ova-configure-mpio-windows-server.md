<properties 
   pageTitle="Ρύθμιση παραμέτρων MPIO στην υπηρεσία παροχής φιλοξενίας εικονικού πίνακα StorSimple | Microsoft Azure"
   description="Περιγράφει τον τρόπο ρύθμισης των παραμέτρων Multipath εισόδου/εξόδου (MPIO) για το StorSimple εικονικού πίνακα συνδεδεμένο με ένα κεντρικό υπολογιστή που εκτελεί Windows Server 2012 R2."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="alkohli" />

# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Ρύθμιση παραμέτρων εισόδου/εξόδου Multipath στον κεντρικό υπολογιστή Windows Server για τον πίνακα εικονικού StorSimple

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να εγκαταστήσετε τη δυνατότητα Multipath εισόδου/εξόδου (MPIO) στην υπηρεσία παροχής φιλοξενίας Windows Server, να εφαρμόσετε ρυθμίσεις παραμέτρων συγκεκριμένων για μόνο StorSimple όγκους και, στη συνέχεια, επιβεβαιώστε MPIO για StorSimple όγκους. Η διαδικασία προϋποθέτει ότι σας StorSimple 1200 εικονικού πίνακα με δύο διασυνδέσεις δικτύου είναι συνδεδεμένος με μια υπηρεσία παροχής φιλοξενίας Windows Server με δύο διασυνδέσεις δικτύου. Οι πληροφορίες που περιέχονται σε αυτό το άρθρο ισχύει μόνο για το εικονικό πίνακα. Για πληροφορίες σχετικά με τις συσκευές σειρά StorSimple 8000, μεταβείτε στη [Ρύθμιση παραμέτρων MPIO για τον κεντρικό υπολογιστή StorSimple](storsimple-configure-mpio-windows-server.md). 

Η δυνατότητα MPIO στο Windows Server σάς βοηθά Δόμηση ιδιαίτερα διαθέσιμη, ανοχή χώρου αποθήκευσης διαμορφώσεις. MPIO χρησιμοποιεί στοιχεία πλεονάζοντα φυσική διαδρομή — προσαρμογέων, καλώδια και διακόπτες — για να δημιουργήσετε λογικές διαδρομές μεταξύ του διακομιστή και τη συσκευή αποθήκευσης. Εάν υπάρχει μια αποτυχία στοιχείου, προκαλεί μια λογική διαδρομή αποτυχία, multipathing λογικής χρησιμοποιεί εναλλακτικής διαδρομής εισόδου/εξόδου, ώστε να εφαρμογές εξακολουθούν να έχουν πρόσβαση στα δεδομένα τους. Επιπλέον ανάλογα με τις ρυθμίσεις σας, MPIO επίσης να βελτιώσετε απόδοσης με εξισορρόπηση εκ νέου η φόρτωση σε όλες αυτές τις διαδρομές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση MPIO](https://technet.microsoft.com/library/cc725907.aspx "MPIO επισκόπηση και δυνατότητες").  

Για την υψηλή διαθεσιμότητα της λύσης σας StorSimple, ρύθμιση παραμέτρων MPIO στο Windows Server κεντρικούς υπολογιστές συνδεδεμένο με τον πίνακα εικονικού StorSimple 1200 (γνωστό και ως το εσωτερικής εικονική συσκευή). Οι διακομιστές κεντρικού υπολογιστή μπορούν να, στη συνέχεια, να αποδεχτεί τις μια σύνδεση, το δίκτυο ή την αποτυχία περιβάλλοντος εργασίας. 

Θα πρέπει να ακολουθήσετε αυτά τα βήματα για να ρυθμίσετε τις παραμέτρους MPIO: 

- Προαπαιτούμενα στοιχεία ρύθμισης παραμέτρων

- Βήμα 1: Εγκατάσταση MPIO στον κεντρικό υπολογιστή Windows Server

- Βήμα 2: Ρύθμιση παραμέτρων MPIO για όγκους StorSimple

- Βήμα 3: Όγκους StorSimple ενεργοποίησης του κεντρικού υπολογιστή

Κάθε ένα από τα παραπάνω βήματα περιγράφεται στις παρακάτω ενότητες.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Αυτή η ενότητα λεπτομέρειες για τις προϋποθέσεις ρύθμισης παραμέτρων για τον κεντρικό υπολογιστή του Windows Server και το εικονικό πίνακα.

### <a name="on-windows-server-host"></a>Στον κεντρικό υπολογιστή Windows Server

-  Βεβαιωθείτε ότι η υπηρεσία παροχής φιλοξενίας Windows Server έχει 2 διασυνδέσεις δικτύου με δυνατότητα.


### <a name="on-storsimple-virtual-array"></a>Στον πίνακα εικονικού StorSimple

- Το εικονικό πίνακα πρέπει να ρυθμιστεί ως διακομιστής iSCSI. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση εικονικού πίνακα ως διακομιστής iSCSI](storsimple-ova-deploy3-iscsi-setup.md). Μία ή περισσότερες διασυνδέσεις δικτύου θα πρέπει να είναι ενεργοποιημένη σε έναν πίνακα.   

- Το διασυνδέσεων δικτύου σε αυτόν τον πίνακα εικονικού πρέπει να είναι προσβάσιμος από τον κεντρικό υπολογιστή του Windows Server.

- Ένα ή περισσότερα όγκους πρέπει να δημιουργηθεί σε σας StorSimple εικονικό πίνακα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσθήκη ενός τόμου](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume) σε σας StorSimple 1200 εικονικό πίνακα. Σε αυτήν τη διαδικασία, που δημιουργήσαμε 3 όγκους (2 τοπικά καρφιτσωμένη και 1 ομότιμου όγκος όπως φαίνεται παρακάτω) στον εικονικό έναν πίνακα.
    
    ![mpio0](./media/storsimple-ova-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Ρύθμιση παραμέτρων υλικού για StorSimple εικονικού πίνακα

Η παρακάτω εικόνα δείχνει τη ρύθμιση παραμέτρων υλικού για υψηλή διαθεσιμότητα και εξισορρόπησης φόρτου multipathing κεντρικού υπολογιστή Windows Server και StorSimple εικονικού πίνακα που χρησιμοποιείται σε αυτήν τη διαδικασία.  

![ρύθμιση παραμέτρων υλικού MPIO](./media/storsimple-ova-configure-mpio-windows-server/1200hardwareconfig.png)

Όπως φαίνεται στην προηγούμενη εικόνα:

- Το εικονικό πίνακα StorSimple παρασχεθεί στο Hyper-V είναι μια συσκευή active μεμονωμένο κόμβο έχει ρυθμιστεί ως διακομιστής iSCSI.

- Δύο διασυνδέσεις εικονικού δικτύου είναι ενεργοποιημένες στο σας πίνακα. Στο τοπικό web περιβάλλοντος εργασίας Χρήστη του πίνακα εικονικού 1200, βεβαιωθείτε ότι οι δύο διασυνδέσεις δικτύου είναι ενεργοποιημένες, μεταβαίνοντας στις **Ρυθμίσεις δικτύου** , όπως φαίνεται παρακάτω:

    ![Ενεργοποιημένη σε 1200 διασυνδέσεις δικτύου](./media/storsimple-ova-configure-mpio-windows-server/mpio9.png)
    
    Σημειώστε τις διευθύνσεις IPv4 από τις διασυνδέσεις με δυνατότητα δικτύου (Ethernet, Ethernet 2 από προεπιλογή) και να αποθηκεύσετε για μελλοντική χρήση στον κεντρικό υπολογιστή.

- Δύο διασυνδέσεις δικτύου είναι ενεργοποιημένες στην υπηρεσία παροχής φιλοξενίας Windows Server. Εάν οι συνδεδεμένοι διασυνδέσεις για κεντρικού υπολογιστή και του πίνακα είναι στο ίδιο υποδίκτυο, στη συνέχεια, θα 4 διαδρομές διαθέσιμη. Αυτό ήταν τα γράμματα σε αυτήν τη διαδικασία. Ωστόσο, εάν κάθε διασύνδεση δικτύου στο περιβάλλον εργασίας του πίνακα και κεντρικού υπολογιστή είναι σε διαφορετικό υποδίκτυο IP (και όχι δυνατότητα δρομολόγησης), στη συνέχεια, 2 μόνο διαδρομές θα είναι διαθέσιμες.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Βήμα 1: Εγκατάσταση MPIO στον κεντρικό υπολογιστή Windows Server

MPIO είναι μια προαιρετική δυνατότητα σε Windows Server και δεν είναι εγκατεστημένο από προεπιλογή. Θα πρέπει να εγκατασταθούν ως δυνατότητα μέσω της διαχείρισης διακομιστή. Για να εγκαταστήσετε αυτήν τη δυνατότητα στην υπηρεσία παροχής φιλοξενίας Windows Server, ολοκληρώστε την ακόλουθη διαδικασία.

[AZURE.INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]


## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Βήμα 2: Ρύθμιση παραμέτρων MPIO για όγκους StorSimple

MPIO πρέπει να είναι διαμορφωμένη για τον προσδιορισμό StorSimple όγκους. Για να ρυθμίσετε τις παραμέτρους MPIO την αναγνώριση όγκους StorSimple, ακολουθήστε τα παρακάτω βήματα.

[AZURE.INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Βήμα 3: Όγκους StorSimple ενεργοποίησης του κεντρικού υπολογιστή

Αφού MPIO έχει ρυθμιστεί σε Windows Server, τόμων που δημιουργήθηκε σε έναν πίνακα StorSimple μπορεί να ενεργοποιηθεί και, στη συνέχεια, μπορούν να επωφεληθούν από MPIO για πλεονασμού. Για να ενεργοποίησης ενός τόμου, εκτελέστε τα ακόλουθα βήματα.

#### <a name="to-mount-volumes-on-the-host"></a>Για να ενεργοποίησης όγκους στον κεντρικό υπολογιστή

1. Ανοίξτε το παράθυρο **iSCSI ιδιότητες προετοιμασίας** στον κεντρικό υπολογιστή Windows Server. Κάντε κλικ στην επιλογή **Διαχείριση διακομιστών > Πίνακας εργαλείων > Εργαλεία > iSCSI προετοιμασίας**.
2. Στο παράθυρο διαλόγου **ιδιότητες προετοιμασίας iSCSI** , κάντε κλικ στην καρτέλα εντοπισμού και, στη συνέχεια, κάντε κλικ στην επιλογή **Ανακαλύψτε πύλη προορισμού**.
3. Στο παράθυρο διαλόγου **Ανακαλύψτε πύλη προορισμού** , κάντε τα εξής:
    
    - Πληκτρολογήστε τη διεύθυνση IP του πρώτου περιβάλλοντος εργασίας με δυνατότητα δικτύου σε σας StorSimple εικονικό πίνακα. Από προεπιλογή, αυτό θα ήταν **Ethernet**. 
    - Κάντε κλικ στο **κουμπί OK** για να επιστρέψετε στο παράθυρο διαλόγου **ιδιότητες προετοιμασίας iSCSI** .

    >[AZURE.IMPORTANT] **Εάν χρησιμοποιείτε ένα ιδιωτικό δίκτυο για τις συνδέσεις iSCSI, εισαγάγετε τη διεύθυνση IP της θύρας ΔΕΔΟΜΈΝΩΝ που είναι συνδεδεμένο στο ιδιωτικό δίκτυο.**

4. Επαναλάβετε τα βήματα 2-3 για μια δεύτερη διασύνδεση δικτύου (για παράδειγμα, Ethernet 2) στον πίνακά σας. 

5. Επιλέξτε την καρτέλα **προορισμών** στο παράθυρο διαλόγου **ιδιότητες προετοιμασίας iSCSI** . Για το εικονικό πίνακα, θα πρέπει να βλέπετε κάθε επιφάνεια ένταση ως προορισμό στην περιοχή **Των προορισμών που εντόπισε**. Σε αυτήν την περίπτωση, θα εντοπιστούν τρεις στόχους (αντιστοιχεί στο τρεις όγκους).

    ![mpio1](./media/storsimple-ova-configure-mpio-windows-server/mpio1.png)

6. Κάντε κλικ στην επιλογή **σύνδεση** για να δημιουργήσετε μια περίοδο λειτουργίας iSCSI με τον πίνακα StorSimple. Θα εμφανιστεί ένα παράθυρο διαλόγου **σύνδεση με προορισμού** . Επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση πολλαπλών διαδρομών** . Κάντε κλικ στην επιλογή **για προχωρημένους**.

    ![mpio2](./media/storsimple-ova-configure-mpio-windows-server/mpio2.png)

8. Στο παράθυρο διαλόγου **Ρυθμίσεις για προχωρημένους** , κάντε τα εξής:                                       
    -    Στην αναπτυσσόμενη λίστα **Τοπικού προσαρμογέα** , επιλέξτε **Microsoft iSCSI προετοιμασίας**.
    -    Στην αναπτυσσόμενη λίστα του **Δημιουργού IP** , επιλέξτε τη διεύθυνση IP του κεντρικού υπολογιστή.
    -    Στην **Πύλη προορισμού** IP αναπτυσσόμενη λίστα, επιλέξτε το IP του περιβάλλοντος εργασίας του πίνακα.
    -    Κάντε κλικ στο **κουμπί OK** για να επιστρέψετε στο παράθυρο διαλόγου **ιδιότητες προετοιμασίας iSCSI** .

    ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

9. Κάντε κλικ στην επιλογή **Ιδιότητες**. 

    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)
10. Στο παράθυρο διαλόγου **Ιδιότητες** , κάντε κλικ στην επιλογή **Προσθήκη περιόδου λειτουργίας**.

    ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)

10. Στο παράθυρο διαλόγου **σύνδεση με προορισμού** , επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση πολλαπλών διαδρομών** . Κάντε κλικ στην επιλογή **για προχωρημένους**.
11. Στο παράθυρο διαλόγου **Ρυθμίσεις για προχωρημένους** :                                        
    -  Στην αναπτυσσόμενη λίστα **τοπικού προσαρμογέα** , επιλέξτε Microsoft iSCSI προετοιμασίας.
    -  Στην αναπτυσσόμενη λίστα του **Δημιουργού IP** , επιλέξτε τη διεύθυνση IP που αντιστοιχεί στον κεντρικό υπολογιστή. Σε αυτήν την περίπτωση, συνδέεστε δύο διασυνδέσεων δικτύου σε έναν πίνακα σε ένα περιβάλλον εργασίας ενιαίου δικτύου στον κεντρικό υπολογιστή. Επομένως, αυτή η διασύνδεση είναι το ίδιο με αυτό που παρέχεται για την πρώτη περίοδο λειτουργίας.
    -  Στην αναπτυσσόμενη λίστα **IP πύλη προορισμού** , επιλέξτε τη διεύθυνση IP για το δεύτερο περιβάλλον δεδομένων ενεργοποιημένη σε έναν πίνακα.
    -  Κάντε κλικ στο **κουμπί OK** για να επιστρέψετε στο παράθυρο διαλόγου Ιδιότητες προετοιμασίας iSCSI. Έχετε προσθέσει μια δεύτερη περίοδο λειτουργίας στον προορισμό.

        ![mpio11](./media/storsimple-ova-configure-mpio-windows-server/mpio11.png)

    - Αφού προσθέσετε την επιθυμητή περιόδους λειτουργίας (οι διαδρομές), στο παράθυρο διαλόγου **ιδιότητες προετοιμασίας iSCSI** , επιλέξτε τον προορισμό και κάντε κλικ στην επιλογή **Ιδιότητες**. Στην καρτέλα οι περίοδοι λειτουργίας του παραθύρου διαλόγου **Ιδιότητες** , σημειώστε την περίοδο λειτουργίας τέσσερις αναγνωριστικά που αντιστοιχούν στα τη διαδρομή πιθανών διατάξεων. Για να ακυρώσετε μια περίοδο λειτουργίας, επιλέξτε το πλαίσιο ελέγχου δίπλα σε ένα αναγνωριστικό περιόδου λειτουργίας και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποσύνδεση**.
 
    - Για να προβάλετε τις συσκευές που βρίσκονται εντός περιόδους λειτουργίας, επιλέξτε την καρτέλα **συσκευές** . Για να ρυθμίσετε τις παραμέτρους της πολιτικής MPIO για μια επιλεγμένη συσκευή, κάντε κλικ στην επιλογή **MPIO**. Το **
    -  Λεπτομέρειες** θα εμφανιστεί το παράθυρο διαλόγου. Στην το **MPIO** καρτέλα, μπορείτε να επιλέξετε την κατάλληλη **πολιτικής εξισορρόπησης φόρτου** ρυθμίσεις. Μπορείτε επίσης να προβάλετε το **ενεργό** ή **αναμονής ** διαδρομή τύπο.

10. Επαναλάβετε τα βήματα 8-11 για να προσθέσετε επιπλέον περιόδους λειτουργίας (οι διαδρομές) προορισμού. Με δύο διασυνδέσεις στον κεντρικό υπολογιστή και δύο στο εικονικού έναν πίνακα, μπορείτε να προσθέσετε ένα σύνολο τέσσερις περιόδους λειτουργίας για κάθε προορισμού. 

    ![mpio14](./media/storsimple-ova-configure-mpio-windows-server/mpio14.png)

11. Θα πρέπει να επαναλάβετε αυτά τα βήματα για κάθε ένταση (επιφανειών ως προορισμό).

    ![mpio15](./media/storsimple-ova-configure-mpio-windows-server/mpio15.png)

12. Ανοίξτε τη **Διαχείριση υπολογιστή** , μεταβαίνοντας **Διαχείριση διακομιστών > Πίνακας εργαλείων > Διαχείριση υπολογιστή**. Στο αριστερό παράθυρο, κάντε κλικ στην επιλογή **χώρου αποθήκευσης > Διαχείριση δίσκων**. Το τόμων που δημιουργήθηκε σε έναν πίνακα εικονικού StorSimple που είναι ορατές σε αυτόν τον κεντρικό υπολογιστή θα εμφανίζεται στην περιοχή **Διαχείριση δίσκων** ως νέα δίσκους.

13. Προετοιμάστε το δίσκο και δημιουργήστε μια νέα έντασης ήχου. Κατά τη διαδικασία μορφή, επιλέξτε ένα μέγεθος μονάδας εκχώρησης (Αυστραλία) από 64 KB. Επαναλάβετε τη διαδικασία για όλα τα διαθέσιμα όγκους.

    ![Διαχείριση δίσκων](./media/storsimple-ova-configure-mpio-windows-server/mpio20.png)

14. Στην περιοχή **Διαχείριση δίσκων**, κάντε δεξί κλικ στο **δίσκο** και επιλέξτε **Ιδιότητες**.

15. Στο παράθυρο διαλόγου **Ιδιότητες των συσκευών δίσκου πολλαπλών διαδρομών** , κάντε κλικ στην καρτέλα **MPIO** .

    ![Ιδιότητες του δίσκου](./media/storsimple-ova-configure-mpio-windows-server/mpio21.png)

16. Στην ενότητα **DSM όνομα** , κάντε κλικ στην επιλογή **Λεπτομέρειες** και βεβαιωθείτε ότι οι παράμετροι έχουν ρυθμιστεί οι παράμετροι προεπιλογή. Οι παράμετροι προεπιλογή είναι:

    - Διαδρομή επαλήθευση περίοδο = 30
    - Επανάληψη Count = 3
    - ΠΟΠ κατάργηση περίοδο = 20
    - Χρονικό διάστημα επανάληψης = 1
    - Επαλήθευση διαδρομή με δυνατότητα = απενεργοποιημένο.

    >[AZURE.NOTE] **Μην τροποποιείτε τις προεπιλεγμένες παραμέτρους.**


## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τη [χρήση της υπηρεσίας διαχείρισης StorSimple για να διαχειριστείτε το εικονικό πίνακα StorSimple](storsimple-ova-manager-service-administration.md).
 

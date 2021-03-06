<properties 
   pageTitle="Σημειώσεις έκδοσης StorSimple εικονικού ενημερώσεις πίνακα | Microsoft Azure"
   description="Περιγράφει κρίσιμες Άνοιγμα προβλήματα και λύσεις για το εικονικό πίνακα StorSimple εκτελείται ενημέρωση 0,2 και 0,1."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Σημειώσεις έκδοσης ενημέρωση πίνακα εικονικές 0,2 και 0,1 StorSimple

## <a name="overview"></a>Επισκόπηση

Το παρακάτω σημειώσεις έκδοσης προσδιορίστε την κρίσιμη ανοιχτά θέματα και τα θέματα επιλυθεί για ενημερώσεις του Microsoft Azure StorSimple εικονικό πίνακα. (Microsoft Azure StorSimple εικονικού πίνακα είναι επίσης γνωστή ως το εικονικό συσκευή εσωτερικής StorSimple ή τη συσκευή εικονικού StorSimple.) 

Σημειώσεις έκδοσης ενημερώνονται συνεχώς και όπως εντοπιστούν κρίσιμα ζητήματα που απαιτεί μια λύση, προστίθενται. Πριν από την ανάπτυξή σας συσκευή εικονικού StorSimple, προσεκτικά τις πληροφορίες που περιέχονται στις σημειώσεις έκδοσης.

Ενημερωμένη έκδοση 0,2 αντιστοιχεί την έκδοση λογισμικού **10.0.10280.0**; Ενημερωμένη έκδοση 0.1 είναι έκδοση **10.0.10279.0**. Οι παρακάτω ενότητες παρουσιάζουν τις αλλαγές για κάθε ενημερωμένη έκδοση. 

> [AZURE.NOTE] Ενημερώσεις είναι ενοχλητικούς και θα επανεκκινήστε τη συσκευή σας. Εάν οι εισόδου/εξόδου είναι σε εξέλιξη, η συσκευή θα υπάρξουν χρόνου εκτός λειτουργίας.

## <a name="issues-fixed-in-the-update-02"></a>Θέματα που επιδιορθώνονται στην ενημερωμένη έκδοση 0,2
Ενημερωμένη έκδοση 0,2 περιλαμβάνει όλες τις αλλαγές από την ενημερωμένη έκδοση 0.1 εκτός από την ενημέρωση κώδικα που περιγράφεται στον παρακάτω πίνακα:

Η δυνατότητα                              | Το ζήτημα                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Ενημερώσεις                                 | Στην τελευταία έκδοση, ενημερωμένες εκδόσεις δεν ανιχνεύονται αυτόματα στην Azure κλασική πύλη, έτσι θα έπρεπε να χρησιμοποιήσετε περιβάλλον εργασίας Χρήστη του τοπικού Web για την εγκατάσταση των ενημερώσεων. Αυτό το πρόβλημα διορθώνεται σε αυτήν την έκδοση. Μετά την εγκατάσταση της ενημερωμένης έκδοσης 0,2, μπορείτε να εγκαταστήσετε μελλοντικές ενημερώσεις με την πύλη Azure κλασική.                       

## <a name="whats-new-in-the-update-01"></a>Τι νέο υπάρχει στο την ενημερωμένη έκδοση 0.1

Ενημερωμένη έκδοση 0.1 περιέχει τις ακόλουθες διορθώσεις σφαλμάτων και βελτιώσεις. 

- **Βελτιωμένο υποστηρίζεται για διακοπές του cloud**: Αυτή η έκδοση περιλαμβάνει πολλές διορθώσεις σφαλμάτων γύρω από αποκατάσταση, δημιουργία αντιγράφων ασφαλείας, επαναφορά και δημιουργία στοιβάδων σε περίπτωση διαταραχή συνδεσιμότητας cloud. 

- **Βελτιωμένο επαναφορά επιδόσεων**: αυτήν την έκδοση έχει διορθώσεις σφαλμάτων που έχουν σημαντικά περιορίσω την ώρα ολοκλήρωση των εργασιών επαναφοράς.

- **Αυτόματο διάστημα ανάκτηση βελτιστοποίησης**: όταν διαγράφονται δεδομένα σε όγκους αραιά προμήθεια του φακέλου, τα μπλοκ χώρου αποθήκευσης που δεν χρησιμοποιούνται πρέπει να να ανακτηθεί. Σε αυτήν την έκδοση βελτιώθηκε τη διαδικασία ανάκτηση χώρου από το cloud που προκύπτουν στο χώρο που δεν χρησιμοποιούνται, πώς να γίνετε διαθέσιμο πιο γρήγορα σε σύγκριση με τις προηγούμενες εκδόσεις.

- **Νέες εικόνες εικονικού δίσκου**: νέο VHD, VHDX και VMDK είναι πλέον διαθέσιμες μέσω Azure κλασική πύλη. Μπορείτε να λάβετε αυτές τις εικόνες για την προμήθεια νέου ενημερωμένη έκδοση 0.1 συσκευές.

- **Βελτίωση της ακρίβειας της κατάστασης εργασιών στην πύλη**: στην προηγούμενη έκδοση του λογισμικού, δεν λεπτομερούς κατάσταση εργασίας αναφοράς στην πύλη. Αυτό το ζήτημα έχει επιλυθεί σε αυτήν την έκδοση.

- **Συμμετοχή σε τομέα εμπειρία**: διορθώσεις σφαλμάτων που σχετίζονται με τη συμμετοχή σε τομέα και μετονομασία της συσκευής.


## <a name="issues-fixed-in-the-update-01"></a>Θέματα που επιδιορθώνονται στην ενημερωμένη έκδοση 0.1

Ο παρακάτω πίνακας παρέχει μια σύνοψη των θεμάτων που επιδιορθώνονται σε αυτήν την έκδοση.

| Όχι.  | Η δυνατότητα                              | Το ζήτημα                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | Σε ορισμένες εκδόσεις VMware, ο δίσκος λειτουργικό σύστημα ήταν θεωρούνται ως κατακερματισμένο προκαλούν ειδοποιήσεις και διακόψετε συνήθεις εργασίες. Αυτό ήταν επιδιορθωθεί σε αυτήν την έκδοση.                                                                                                                                                                                    |
| 2    | iSCSI διακομιστή                         | Στην τελευταία έκδοση, ο χρήστης έχει χρειάζεται για να καθορίσετε μια πύλη για κάθε διασύνδεση με δυνατότητα δικτύου της συσκευής σας εικονικού StorSimple. Αυτή η συμπεριφορά έχει αλλάξει σε αυτήν την έκδοση, ώστε ο χρήστης πρέπει να ρυθμίσετε τις παραμέτρους τουλάχιστον μία πύλη για όλες τις διασυνδέσεις με δυνατότητα δικτύου.                                                                              |
| 3    | Πακέτο υποστήριξης                      | Στην προηγούμενη έκδοση του λογισμικού, η συλλογή πακέτο υποστήριξης απέτυχε κατά τις διαστάσεις του πακέτου έχουν μεγαλύτερα από 1 GB. Αυτό το πρόβλημα διορθώνεται σε αυτήν την έκδοση.                                                                                                                                                                               |
| 4    | Πρόσβαση στο cloud                         |  Στην τελευταία έκδοση, εάν ο πίνακας εικονικού StorSimple δεν είχαν συνδεσιμότητα του δικτύου και έγινε επανεκκίνηση, το τοπικό περιβάλλον εργασίας Χρήστη να έχουν αντιμετωπίζετε προβλήματα συνδεσιμότητας. Σε αυτό το πρόβλημα διορθώνεται σε αυτήν την έκδοση.                                                                                                                            |
| 5    | Παρακολούθηση γραφήματα                    | Στην προηγούμενη έκδοση, ακολουθώντας μια συσκευή ανακατεύθυνσης, τα γραφήματα cloud δυνατότητα χρήσης εμφανίζονται λανθασμένες τιμές στην πύλη του Azure κλασική. Αυτό είναι σταθερή στην τρέχουσα έκδοση.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Γνωστά θέματα στην ενημερωμένη έκδοση 0.1

Ο παρακάτω πίνακας παρέχει μια σύνοψη των γνωστών θεμάτων για έναν πίνακα εικονικού StorSimple και περιλαμβάνει τα προβλήματα κυκλοφορίας σημειωθεί από τις προηγούμενες εκδόσεις. **Την έκδοση θέματα που αναφέρονται σε αυτήν την έκδοση είναι σημειωμένα με αστερίσκο. Σχεδόν όλα τα θέματα σε αυτήν τη λίστα έχουν μεταφέρεται από την έκδοση GA StorSimple εικονικό πίνακα.**


| Όχι. | Η δυνατότητα | Το ζήτημα | Λύση: / σχολίων |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Ενημερώσεις | Εικονικό συσκευές που δημιουργήσατε στην έκδοση preview δεν μπορεί να ενημερωθεί σε μια υποστηριζόμενη έκδοση γενικής διαθεσιμότητας. | Αυτές τις συσκευές εικονικού πρέπει να είναι απέτυχε επάνω από την έκδοση γενικής διαθεσιμότητας με χρήση μιας ροής εργασίας από καταστροφή αποκατάστασης (DR). |
| **2.** | Προμήθεια του φακέλου δεδομένων δίσκου | Μία φορά που έχουν παρασχεθεί ένα δίσκο δεδομένων από ένα συγκεκριμένο καθορισμένο μέγεθος και δημιουργήσει την αντίστοιχη StorSimple εικονική συσκευή, πρέπει να δεν ανάπτυξη ή να σμικρύνετε το δίσκο δεδομένων. Προσπαθείτε να κάνετε θα έχει ως αποτέλεσμα την απώλεια όλων των δεδομένων σε την τοπική βαθμίδες της συσκευής. |   |
| **3.** | Πολιτική ομάδας | Όταν μια συσκευή τομέα, η εφαρμογή της πολιτικής ομάδας μπορεί να επηρεάσει αρνητικά τη λειτουργία της συσκευής. | Βεβαιωθείτε ότι το εικονικό πίνακα είναι στο δικό του οργανική μονάδα (OU) για την υπηρεσία καταλόγου Active Directory και χωρίς αντικείμενα πολιτικής ομάδας (αντικείμενο πολιτικής ΟΜΆΔΑΣ) έχουν εφαρμοστεί σε αυτό.|
| **4.** | Τοπικό web περιβάλλοντος εργασίας Χρήστη | Εάν είναι ενεργοποιημένες οι βελτιωμένες δυνατότητες ασφαλείας του Internet Explorer (IE ESC), ορισμένες σελίδες περιβάλλοντος εργασίας Χρήστη τοπικό web όπως η αντιμετώπιση προβλημάτων ή συντήρηση ενδέχεται να μην λειτουργεί σωστά. Κουμπιά σε αυτές τις σελίδες επίσης ενδέχεται να μην λειτουργούν. | Απενεργοποιήστε τις βελτιωμένες δυνατότητες ασφαλείας στον Internet Explorer.|
| **5.** | Τοπικό web περιβάλλοντος εργασίας Χρήστη | Σε μια εικονική μηχανή Hyper-V, τις διασυνδέσεις δικτύου στο περιβάλλον εργασίας Χρήστη εμφανίζονται ως 10 web Gbps περιβάλλοντα εργασίας. | Αυτή η συμπεριφορά είναι αντανάκλαση των Hyper-V. Το Hyper-V εμφανίζει πάντα 10 Gbps για προσαρμογέων εικονικού δικτύου. |
| **6.** | Ομότιμου όγκους ή κοινόχρηστα στοιχεία | Περιοχή byte κλειδώματος για τις εφαρμογές που λειτουργούν με το StorSimple ομότιμου όγκους δεν υποστηρίζεται. Εάν είναι ενεργοποιημένη η byte περιοχή κλείδωμα, δημιουργία στοιβάδων StorSimple δεν θα λειτουργούν. | Προτεινόμενα μέτρα περιλαμβάνουν: <br></br>Απενεργοποιήστε την επιλογή περιοχής byte κλειδώματος λογικής της εφαρμογής σας.<br></br>Επιλέξτε για να τοποθετήσετε τα δεδομένα για αυτήν την εφαρμογή στο τοπικά καρφιτσωμένη όγκους αντί για ομότιμου όγκους.<br></br>*Ότι*: αν χρησιμοποιώντας τοπικά καρφιτσωμένα όγκους και byte περιοχή κλείδωμα ενεργοποιείται, έχετε υπόψη ότι η ένταση τοπικά καρφιτσωμένη μπορεί να είναι online ακόμα και πριν να ολοκληρωθεί η επαναφορά. Στην περίπτωση αυτή, εάν μια επαναφορά είναι σε εξέλιξη, στη συνέχεια, πρέπει να περιμένετε για την επαναφορά για να ολοκληρωθεί. |
| **7.** | Ομότιμου μετοχών | Εργασία με μεγάλα αρχεία θα μπορούσε να έχει ως αποτέλεσμα αργή επίπεδο ανάληψη. | Όταν εργάζεστε με μεγάλα αρχεία, συνιστάται να ότι το μεγαλύτερο αρχείο είναι μικρότερη από 3% του μεγέθους κοινή χρήση. |
| **8.** | Χρησιμοποιείται χωρητικότητα για τα κοινόχρηστα στοιχεία | Μπορείτε να δείτε κοινή χρήση κατανάλωση κατά την απουσία των δεδομένων σε κοινή χρήση. Αυτό συμβαίνει επειδή η δυναμικότητα χρησιμοποιείται για τα κοινόχρηστα στοιχεία περιλαμβάνει μετα-δεδομένων. |   |
| **9.** | Αποκατάσταση | Μπορείτε να εκτελέσετε μόνο την αποκατάσταση ενός διακομιστή αρχείων με τον ίδιο τομέα με αυτό της συσκευής προέλευσης. Αποκατάσταση σε μια συσκευή προορισμού σε άλλον τομέα δεν υποστηρίζεται σε αυτήν την έκδοση. | Αυτό θα υλοποιηθεί σε νεότερη έκδοση. |
| **10.** | Azure PowerShell | Δεν είναι δυνατό να γίνεται η Διαχείριση εικονικού συσκευές StorSimple κατά το PowerShell Azure σε αυτήν την έκδοση. | Πρέπει να γίνει όλες τη διαχείριση των συσκευών εικονικού μέσω Azure κλασική πύλη και τοπικό web περιβάλλοντος εργασίας Χρήστη. |
| **11.** | Αλλαγή του κωδικού πρόσβασης | Κονσόλα συσκευή εικονικού πίνακα δέχεται μόνο εισαγωγής σε μορφή πληκτρολογίου en-US. |   |
| **12.** | CHAP | Δεν είναι δυνατό να καταργηθούν CHAP διαπιστευτήρια που μόλις δημιουργήσατε. Επιπλέον, εάν τροποποιήσετε τα διαπιστευτήρια CHAP, θα πρέπει να μεταφέρετε το όγκους εκτός σύνδεσης και, στη συνέχεια, μεταφέρετέ τα online για να τεθούν σε ισχύ οι αλλαγές. | Αυτά θα αντιμετωπιστεί σε μια μελλοντική έκδοση. |
| **13.** | iSCSI διακομιστή  | Η 'χρησιμοποιούνται χώρου αποθήκευσης' για μια ένταση iSCSI εμφανίζονται μπορεί να διαφέρουν σε της υπηρεσίας διαχείρισης StorSimple και ο κεντρικός υπολογιστής iSCSI. | Ο κεντρικός υπολογιστής iSCSI έχει την προβολή του συστήματος αρχείων.<br></br>Η συσκευή βλέπει τα μπλοκ έχει εκχωρηθεί όταν ο όγκος ήταν στο μέγιστο μέγεθος.|
| **14.** | Αρχείο διακομιστή *  | Εάν ένα αρχείο σε ένα φάκελο έχει μια εναλλακτική δεδομένων ροής (ΔΙΑΦΗΜΊΣΕΙΣ) που σχετίζεται με αυτό, τις ΔΙΑΦΗΜΊΣΕΙΣ δεν είναι αντίγραφα ασφαλείας ή την επαναφορά μέσω αποκατάσταση, αντιγραφής και αποκατάσταση επίπεδο στοιχείου.| |


## <a name="next-step"></a>Επόμενο βήμα

[Εγκατάσταση ενημερώσεων](storsimple-ova-install-update-01.md) σε σας StorSimple εικονικό πίνακα.

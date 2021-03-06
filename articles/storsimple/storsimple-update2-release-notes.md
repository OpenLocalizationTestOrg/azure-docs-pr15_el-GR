<properties 
   pageTitle="Σημειώσεις έκδοσης StorSimple 8000 σειρά ενημερωμένη έκδοση 2 | Microsoft Azure"
   description="Περιγράφει τα νέα χαρακτηριστικά, προβλήματα και λύσεις για StorSimple 8000 σειρά ενημερωμένη έκδοση 2."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Σημειώσεις έκδοσης StorSimple 8000 σειρά ενημερωμένη έκδοση 2  

## <a name="overview"></a>Επισκόπηση

Το παρακάτω σημειώσεις έκδοσης περιγράφουν τις νέες δυνατότητες και προσδιορίστε το κρίσιμα ζητήματα ανοιχτό για StorSimple 8000 σειρά ενημερωμένη έκδοση 2. Περιλαμβάνουν επίσης μια λίστα με το λογισμικό, το πρόγραμμα οδήγησης και ενημερώσεις υλικολογισμικού δίσκου που περιλαμβάνονται σε αυτήν την έκδοση StorSimple. 

Ενημέρωση 2 μπορούν να εφαρμοστούν σε οποιαδήποτε συσκευή StorSimple εξαντλήσετε κυκλοφορίας (GA) ή ενημερωμένη έκδοση 0.1 ενημερωμένη έκδοση 1.2. Η έκδοση συσκευή που σχετίζεται με την ενημερωμένη έκδοση 2 είναι 6.3.9600.17673.

Διαβάστε τις πληροφορίες που περιέχονται στις σημειώσεις έκδοσης πριν από την ανάπτυξη της ενημερωμένης έκδοσης στη λύση σας StorSimple.

>[AZURE.IMPORTANT]
> 
- Χρειάζονται ώρες περίπου 4-7 για την εγκατάσταση αυτής της ενημερωμένης έκδοσης (συμπεριλαμβανομένων των τις ενημερώσεις των Windows). 
- Ενημερωμένη έκδοση 2 έχει λογισμικό, το πρόγραμμα οδήγησης LSI και ενημερώσεις υλικολογισμικού SSD.
- Για νέες εκδόσεις, δεν μπορεί να δείτε ενημερώσεις αμέσως επειδή κάνουμε μια φάσεις της ενοποιημένης επικοινωνίας από τις ενημερώσεις. Περιμένετε μερικές ημέρες και, στη συνέχεια, σάρωση για ενημερώσεις ξανά ως αυτά θα γίνουν διαθέσιμες σύντομα.


## <a name="whats-new-in-update-2"></a>Τι νέο υπάρχει στην έκδοση 2

Ενημερωμένη έκδοση 2 παρουσιάζει τις ακόλουθες νέες δυνατότητες.

- **Τοπικά καρφιτσωμένα όγκους** – σε προηγούμενες εκδόσεις της σειράς StorSimple 8000, μπλοκ δεδομένων έχουν επιπέδων στο cloud με βάση τη χρήση. Δεν υπήρχε τρόπος για να εξασφαλίσετε ότι μπλοκ θα παραμείνει στην τοπική. Στο ενημερωμένη έκδοση 2, κατά τη δημιουργία ενός τόμου, μπορείτε να ορίσετε μια ένταση όπως τοπικά καρφιτσωμένη και κύρια δεδομένα από αυτήν την ένταση ήχου δεν θα είναι επιπέδων στο cloud. Στιγμιότυπα από τοπικά καρφιτσωμένη όγκους θα εξακολουθούν να αντιγραφούν στο cloud για δημιουργία αντιγράφων ασφαλείας, έτσι ώστε να μπορούν να χρησιμοποιηθούν στο cloud για σκοπούς αποκατάστασης φορητότητα και καταστροφή δεδομένων. Επιπλέον, μπορείτε να αλλάξετε τον τύπο του τόμου (δηλαδή, μετατροπή επιπέδων όγκους να τοπικά καρφιτσωμένη όγκους και επιπέδων όγκους μετατροπή τοπικά καρφιτσωμένα να). 

- **Βελτιώσεις εικονική συσκευή StorSimple** -προηγουμένως, τη σειρά StorSimple 8000 έχει τοποθετηθεί η εικονική συσκευή ως λύση καταστροφής ανάκτησης ή ανάπτυξη/έλεγχο. Παρουσιάστηκε ένα μόνο μοντέλο της συσκευής εικονικού (μοντέλο 1100). Ενημέρωση 2 παρουσιάζει δύο μοντέλων εικονική συσκευή: 

     - 8010 (πρώην ονομάζεται το 1100) – καμία αλλαγή; χωρητικότητα 30 TB και χρησιμοποιεί Azure τυπική χώρο αποθήκευσης.
     - 8020 – διαθέτει χωρητικότητα 64 TB και χρησιμοποιεί χώρο αποθήκευσης Azure Premium για βελτιωμένη απόδοση.

    Υπάρχει ένα μεμονωμένο VHD για δύο μοντέλα εικονική συσκευή (8010 8020). Κατά την πρώτη εκκίνηση της συσκευής εικονικού, εντοπίζει τις παραμέτρους πλατφόρμα και εφαρμόζεται η σωστή μοντέλο έκδοση.

- **Βελτιώσεις δικτύου** -η ενημερωμένη έκδοση 2 περιλαμβάνει τις ακόλουθες βελτιώσεις δικτύου:

     - Πολλά NIC μπορεί να ενεργοποιηθεί για το cloud, έτσι ώστε να ανακατεύθυνσης μπορεί να προκύψει εάν αποτύχει η NIC.
     - Δρομολόγηση βελτιώσεις, με σταθερό μετρήσεις για cloud ενεργοποιημένη μπλοκ.
     - Online "Επανάληψη" αποτυχίας πόρων πριν ανακατεύθυνσης.
     - Ειδοποιήσεις για νέα για αποτυχίες υπηρεσίας.

- **Ενημέρωση βελτιώσεις** – σε ενημερωμένη έκδοση 1.2 και παλαιότερες εκδόσεις, η σειρά StorSimple 8000 ενημερώθηκε μέσω δύο κανάλια: ενημέρωση των Windows για το σύμπλεγμα, iSCSI, και ούτω καθεξής και το Microsoft Update για δυαδικών αρχείων και υλικολογισμικό.
    Ενημέρωση 2 χρησιμοποιεί το Microsoft Update για όλα τα πακέτα ενημερωμένων εκδόσεων. Αυτό θα πρέπει να οδηγήσει σε λιγότερο χρόνο ενημερωμένων ή κάνοντας ανακατευθύνσεις. 

- **Ενημερώσεις υλικολογισμικού** – τα εξής ενημερωμένες εκδόσεις υλικολογισμικού περιλαμβάνονται:
    - LSI: lsi_sas2.sys 2.00.72.10 την έκδοση προϊόντος
    - Μόνο SSD (δεν υπάρχουν ενημερωμένες εκδόσεις σκληρού ΔΊΣΚΟΥ): XMGG, XGEG, KZ50, F6C2 και VR08

- **Πριν από την ενεργοποίηση υποστήριξης** – ενημερωμένη έκδοση 2 επιτρέπει στη Microsoft να αποσπάσετε πρόσθετες πληροφορίες διαγνωστικών από τη συσκευή. Όταν ομάδα μας λειτουργίες προσδιορίζει συσκευές που αντιμετωπίζετε προβλήματα, Ζητούμε καλύτερα δυνατότητα να συλλέξετε πληροφορίες από τη συσκευή και διάγνωση θεμάτων. **Με την αποδοχή ενημερωμένη έκδοση 2, επιτρέπετε μας για την παροχή αυτό πριν από την ενεργοποίηση υποστήριξης**.    
 

## <a name="issues-fixed-in-update-2"></a>Ζητήματα που διορθώνονται στο ενημερωμένη έκδοση 2

Στους πίνακες που ακολουθούν παρέχει μια σύνοψη των θεμάτων που είχαν διορθωθεί στο 2 ενημερώσεις.    

| Όχι. | Η δυνατότητα | Το ζήτημα | Ισχύει για φυσική συσκευή | Ισχύει για το εικονικό συσκευή |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Διασυνδέσεις δικτύου | Μετά την αναβάθμιση σε ενημέρωση 1, η υπηρεσία διαχείρισης StorSimple αναφέρεται ότι οι θύρες σύνολο δεδομένων 2 και Data3 απέτυχε σε μία ελεγκτή. Αυτό το ζήτημα έχει διορθωθεί. | Ναι | Όχι |
| 2 | Ενημερώσεις | Μετά την αναβάθμιση σε 1 ενημέρωση, ειδοποίηση ηχητικές ειδοποιήσεις Παρουσιάστηκε στην πύλη του Azure κλασική σε πολλές συσκευές. Αυτό το ζήτημα έχει διορθωθεί. | Ναι | Όχι |
| 3 | Έλεγχος ταυτότητας Openstack | Όταν χρησιμοποιείτε Openstack ως υπηρεσία παροχής cloud, ενδέχεται να λάβετε ένα μήνυμα σφάλματος ότι η συμβολοσειρά ελέγχου ταυτότητας cloud ήταν πολύ χρόνο. Αυτό έχει καθοριστεί. | Ναι | Όχι |


## <a name="known-issues-in-update-2"></a>Γνωστά θέματα στο ενημερωμένη έκδοση 2

Ο παρακάτω πίνακας παρέχει μια σύνοψη των γνωστών θεμάτων σε αυτήν την έκδοση.

| Όχι. | Η δυνατότητα | Το ζήτημα | Σχόλια / λύση | Ισχύει για φυσική συσκευή | Ισχύει για το εικονικό συσκευή |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Δίσκου απαρτίας | Σπάνια, εάν το μεγαλύτερο μέρος των δίσκων στο EBOD περίβλημα της συσκευής 8600 αποσυνδέονται αποτέλεσμα απαρτία δίσκου, στη συνέχεια, το χώρο συγκέντρωσης χώρου αποθήκευσης θα μεταβείτε χωρίς σύνδεση. Θα παραμείνει χωρίς σύνδεση ακόμα και αν είναι επανασύνδεση των δίσκων. | Θα πρέπει να κάνετε επανεκκίνηση της συσκευής. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με υποστήριξης της Microsoft για επόμενα βήματα. | Ναι | Όχι |
| 2 | Αναγνωριστικό εσφαλμένες ελεγκτή | Όταν πραγματοποιείται αντικαθιστά ελεγκτή, ελεγκτή 0 μπορεί να εμφανίζεται ως ελεγκτή 1. Κατά τη διάρκεια της αντικατάστασης ελεγκτή, όταν η εικόνα έχει φορτωθεί από τον κόμβο ομότιμης σύνδεσης, το Αναγνωριστικό ελεγκτή μπορεί να εμφανίζεται αρχικά ως αναγνωριστικό του ελεγκτή ομότιμης σύνδεσης. Σπάνια, αυτή η συμπεριφορά ενδέχεται επίσης να εμφανίζεται μετά την επανεκκίνηση του συστήματος. | Απαιτείται καμία ενέργεια χρήστη. Αυτή η κατάσταση επιλύει ίδια μετά την ολοκλήρωση της αντικατάστασης ελεγκτή. | Ναι | Όχι |
| 3 | Λογαριασμοί χώρου αποθήκευσης | Χρήση της υπηρεσίας χώρου αποθήκευσης για να διαγράψετε το λογαριασμό χώρου αποθήκευσης είναι ένα σενάριο που δεν υποστηρίζονται. Αυτό θα οδηγήσει σε μια κατάσταση στην οποία δεν είναι δυνατή η ανάκτηση των δεδομένων χρήστη.|  | Ναι | Ναι |
| 4 | Συσκευή ανακατεύθυνσης | Πολλές ανακατευθύνσεις ενός κοντέινερ ένταση από την ίδια συσκευή προέλευσης σε διαφορετική προορισμού συσκευές δεν υποστηρίζεται. Ανακατεύθυνση από μια μεμονωμένη συσκευή νεκρού σε πολλές συσκευές θα κάνουν την ένταση κοντέινερ στην πρώτη απέτυχε πάνω από τη συσκευή χάσετε κατοχή δεδομένων. Μετά την ανακατεύθυνση, αυτά τα κοντέινερ ένταση θα εμφανίζονται ή να συμπεριφέρονται διαφορετικά όταν επιλέγετε την προβολή τους στην πύλη του Azure κλασική. | | Ναι | Όχι |
| 5 | Εγκατάσταση | Κατά τη διάρκεια StorSimple προσαρμογέα για την εγκατάσταση του SharePoint, πρέπει να δώσετε μια συσκευή IP για την εγκατάσταση, για να ολοκληρωθεί με επιτυχία.    | | Ναι | Όχι |
| 6 | Διακομιστής μεσολάβησης Web | Εάν σας ρύθμισης παραμέτρων διακομιστή μεσολάβησης web έχει HTTPS ως το καθορισμένο πρωτόκολλο, στη συνέχεια, θα επηρεαστεί η επικοινωνία σας υπηρεσία συσκευής και στη συσκευή θα μεταβείτε εκτός σύνδεσης. Υποστήριξη πακέτων θα δημιουργηθούν επίσης κατά τη διαδικασία, καταναλώνει πόρους σημαντική στη συσκευή σας. | Βεβαιωθείτε ότι η διεύθυνση URL του διακομιστή μεσολάβησης web έχει HTTP ως το καθορισμένο πρωτόκολλο. Για περισσότερες πληροφορίες, μεταβείτε στη [Ρύθμιση παραμέτρων διακομιστή μεσολάβησης web για τη συσκευή σας](storsimple-configure-web-proxy.md). | Ναι | Όχι |
| 7 | Διακομιστής μεσολάβησης Web | Εάν ρυθμίσετε τις παραμέτρους και ενεργοποίηση διακομιστή μεσολάβησης web σε μια συσκευή καταχωρημένες, στη συνέχεια, θα πρέπει να επανεκκινήσετε τον ενεργό ελεγκτή στη συσκευή σας. | | Ναι | Όχι |
| 8 | Λανθάνων χρόνος υψηλή cloud και υψηλό φόρτο εργασίας εισόδου/εξόδου | Όταν συσκευή StorSimple συναντά ένα συνδυασμό των αδρανειών πολύ υψηλή cloud (σειρά δευτερόλεπτα) και υψηλό φόρτο εργασίας εισόδου/εξόδου, η συσκευή όγκους Μετάβαση σε κατάσταση υποβαθμίζονται και ενδέχεται να αποτύχει η εισόδων/εξόδων με ένα σφάλμα "δεν είναι έτοιμο συσκευής". | Θα πρέπει να επανεκκινήστε τη συσκευή ελεγκτές με μη αυτόματο τρόπο ή να εκτελέσετε μια συσκευή ανακατεύθυνσης για την ανάκτηση από αυτήν την περίπτωση. | Ναι | Όχι |
| 9 | Azure PowerShell | Όταν χρησιμοποιείτε το cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124- Επιλογή αντικειμένου - πρώτα 1 - αναμονή** για να επιλέξετε το πρώτο αντικείμενο έτσι ώστε να μπορείτε να δημιουργήσετε ένα νέο αντικείμενο **VolumeContainer** , το cmdlet επιστρέφει όλα τα αντικείμενα. | Αναδίπλωση το cmdlet σε παρενθέσεις ως εξής: **(Get-Azure-StorSimpleStorageAccountCredential) & #124- Επιλογή αντικειμένου - πρώτα 1 - αναμονή** | Ναι | Ναι |
| 10| Μετεγκατάσταση | Όταν πολλών ένταση κοντέινερ μεταβιβάζονται για μετεγκατάσταση, η ΄ΉΤΑ για δημιουργία αντιγράφων ασφαλείας πιο πρόσφατη είναι ακριβείς μόνο για το πρώτο κοντέινερ ένταση. Επιπλέον, παράλληλες μετεγκατάστασης θα ξεκινήσει μετά τα πρώτα 4 αντίγραφα ασφαλείας στο πρώτο κοντέινερ ένταση μετεγκαθίστανται. | Συνιστάται να κάνετε μετεγκατάσταση ένα κοντέινερ ένταση κάθε φορά. | Ναι | Όχι |
| 11| Μετεγκατάσταση | Μετά την επαναφορά, όγκους δεν προστίθεται η πολιτική ασφαλείας ή την ομάδα εικονικού δίσκου. | Θα πρέπει να προσθέσετε αυτές τις όγκους σε μια πολιτική ασφαλείας για να δημιουργήσετε αντίγραφα ασφαλείας. | Ναι | Ναι |
| 12| Μετεγκατάσταση | Μετά την ολοκλήρωση της μετεγκατάστασης, η συσκευή 5000/7000 σειράς δεν πρέπει να πρόσβαση τα κοντέινερ μετεγκαταστάθηκαν δεδομένων. | Συνιστάται να διαγράψετε το κοντέινερ μετεγκαταστάθηκαν δεδομένων μετά τη μετεγκατάσταση ολοκλήρωσης και δεσμευμένη. | Ναι | Όχι |
| 13| Κλωνοποίηση και DR | Μια συσκευή StorSimple εκτελεί ενημέρωση 1 δεν είναι δυνατό να κλωνοποίηση ή την εκτέλεση αποκατάστασης από καταστροφή σε μια συσκευή η εκτέλεση λογισμικού 1 πριν από την ενημέρωση. | Θα πρέπει να ενημερώσετε τη συσκευή προορισμού για να ενημερώσετε 1 για να επιτρέψετε σε αυτές τις λειτουργίες | Ναι | Ναι |
| 14 | Μετεγκατάσταση | Ρύθμιση παραμέτρων δημιουργίας αντιγράφων ασφαλείας για μετεγκατάσταση ενδέχεται να αποτύχει σε μια συσκευή σειράς 5000 7000 όταν υπάρχουν ένταση ομάδες με χωρίς συσχετισμένη όγκους. | Διαγράψτε όλες τις ομάδες κενή ένταση με χωρίς συσχετισμένη όγκους και, στη συνέχεια, προσπαθήστε ξανά το αντίγραφο ασφαλείας της ρύθμισης παραμέτρων.| Ναι | Όχι |
| 15 | Azure cmdlet του PowerShell και τοπικά καρφιτσωμένη όγκους | Δεν μπορείτε να δημιουργήσετε μια τοπικά καρφιτσωμένη ένταση μέσω των cmdlet του Azure PowerShell. (Οποιαδήποτε ένταση δημιουργείτε μέσω του Azure PowerShell θα είναι επιπέδων.) |Χρησιμοποιείτε πάντα την υπηρεσία διαχείρισης StorSimple για ρύθμιση παραμέτρων τοπικά καρφιτσωμένη όγκους.| Ναι | Όχι |
| 16 |Διαθέσιμο για τοπικά καρφιτσωμένη όγκους χώρο | Εάν διαγράψετε μια τοπικά καρφιτσωμένη ένταση, το διαθέσιμο χώρο για νέα όγκους ενδέχεται να μην ενημερώνεται αμέσως. Η υπηρεσία διαχείρισης StorSimple ενημερώνει το διαθέσιμο χώρο τοπικό περίπου κάθε ώρα.| Περιμένετε μία ώρα πριν επιχειρήσετε να δημιουργήσετε το νέο έντασης ήχου. | Ναι | Όχι |
| 17 | Τοπικά καρφιτσωμένη όγκους | Εργαστείτε επαναφορά εκθέτει το αντίγραφο ασφαλείας προσωρινά στιγμιότυπο στον κατάλογο αντιγράφου ασφαλείας, αλλά μόνο για τη διάρκεια της εργασίας επαναφοράς. Επιπλέον, εκθέτει μια ομάδα εικονικού δίσκου με πρόθεμα **tmpCollection** στη σελίδα **Πολιτικές δημιουργίας αντιγράφων ασφαλείας** , αλλά μόνο για τη διάρκεια της εργασίας επαναφοράς. | Αυτή η συμπεριφορά μπορεί να προκύψει εάν σας εργασίας επαναφοράς περιλαμβάνει μόνο τοπικά καρφιτσωμένο όγκους ή συνδυασμό και των τοπικά καρφιτσωμένη και ομότιμου όγκους. Εάν η εργασία επαναφοράς περιλαμβάνει μόνο ομότιμου όγκους, στη συνέχεια, αυτή η συμπεριφορά δεν θα συμβεί. Απαιτείται χωρίς παρέμβαση του χρήστη. | Ναι | Όχι |
| 18 | Τοπικά καρφιτσωμένη όγκους | Εάν κάνετε ακύρωση μιας εργασίας επαναφοράς και ανακατεύθυνσης ελεγκτή παρουσιάζεται αμέσως στη συνέχεια, η εργασία επαναφοράς θα εμφανίσει **απέτυχε** αντί **ακυρώθηκε**. Εάν μια εργασία επαναφορά αποτύχει και ανακατεύθυνσης ελεγκτή πραγματοποιείται αμέσως στη συνέχεια, η εργασία επαναφοράς θα εμφανίσει **ακυρώθηκε** αντί **απέτυχε**. | Αυτή η συμπεριφορά μπορεί να προκύψει εάν σας εργασίας επαναφοράς περιλαμβάνει μόνο τοπικά καρφιτσωμένο όγκους ή συνδυασμό και των τοπικά καρφιτσωμένη και ομότιμου όγκους. Εάν η εργασία επαναφοράς περιλαμβάνει μόνο ομότιμου όγκους, στη συνέχεια, αυτή η συμπεριφορά δεν θα συμβεί. Απαιτείται χωρίς παρέμβαση του χρήστη. | Ναι | Όχι |
| 19 |Τοπικά καρφιτσωμένη όγκους | Εάν κάνετε ακύρωση μιας εργασίας επαναφοράς ή εάν αποτύχει η επαναφορά και, στη συνέχεια, παρουσιάζεται ανακατεύθυνσης ελεγκτή, μια εργασία επιπλέον επαναφοράς εμφανίζεται στη σελίδα " **εργασίες** ". | Αυτή η συμπεριφορά μπορεί να προκύψει εάν σας εργασίας επαναφοράς περιλαμβάνει μόνο τοπικά καρφιτσωμένο όγκους ή συνδυασμό και των τοπικά καρφιτσωμένη και ομότιμου όγκους. Εάν η εργασία επαναφοράς περιλαμβάνει μόνο ομότιμου όγκους, στη συνέχεια, αυτή η συμπεριφορά δεν θα συμβεί. Απαιτείται χωρίς παρέμβαση του χρήστη. | Ναι | Όχι |
| 20 |Τοπικά καρφιτσωμένη όγκους | Εάν προσπαθήσετε να μετατρέψετε μια ομότιμου ένταση (που έχουν δημιουργηθεί και κλωνοποιημένο με ενημερωμένη έκδοση 1.2 ή προηγούμενες εκδόσεις) σε τοπικά καρφιτσωμένη όγκο και τη συσκευή σας επαρκεί ο χώρος ή υπάρχει μια μη διαθεσιμότητα της cloud, στη συνέχεια, το clone(s) μπορεί να έχει καταστραφεί.| Αυτό το πρόβλημα παρουσιάζεται μόνο με όγκους που περιείχε το λογισμικό που έχουν δημιουργηθεί και κλωνοποιημένο με πριν από την ενημέρωση 2. Αυτό πρέπει να είναι ένα σενάριο σπάνια.|
| 21 | Μετατροπή έντασης ήχου | Δεν ενημερώνουν το ACRs που έχουν επισυναφθεί σε μια ένταση ενώ η μετατροπή τόμου βρίσκεται σε εξέλιξη (επιπέδων σε τοπικά καρφιτσωμένα ή αντίστροφα). Ενημέρωση του ACRs θα μπορούσε να έχει ως αποτέλεσμα την καταστροφή δεδομένων. | Εάν είναι απαραίτητο, ενημερώστε το ACRs πριν από τη μετατροπή της έντασης ήχου και μην κάνετε περαιτέρω ενημερώσεις ACR ενώ η μετατροπή βρίσκεται σε εξέλιξη. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Ενημερώσεις του ελεγκτή και υλικολογισμικού στο ενημερωμένη έκδοση 2

Σε αυτήν την έκδοση ενημερώνει το πρόγραμμα οδήγησης και το υλικολογισμικό δίσκο στη συσκευή σας.
 
- Για περισσότερες πληροφορίες σχετικά με το υλικολογισμικό LSI ενημερωμένη έκδοση, ανατρέξτε στο άρθρο της Γνωσιακής βάσης 3121900. 
- Για περισσότερες πληροφορίες σχετικά με το υλικολογισμικό δίσκου ενημερωμένη έκδοση, ανατρέξτε στο άρθρο της Γνωσιακής βάσης 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Εικονική συσκευή ενημερώσεις στο ενημερωμένη έκδοση 2

Αυτή η ενημερωμένη έκδοση δεν μπορεί να εφαρμοστεί η εικονική συσκευή. Νέες συσκευές εικονικού θα πρέπει να δημιουργηθεί. 

## <a name="next-step"></a>Επόμενο βήμα

Μάθετε πώς να [εγκαταστήσετε την ενημερωμένη έκδοση 2](storsimple-install-update-2.md) στη συσκευή σας StorSimple.

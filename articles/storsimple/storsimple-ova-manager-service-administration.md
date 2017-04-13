<properties 
   pageTitle="Διαχείριση StorSimple Manager εικονικές πίνακα | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να διαχειριστείτε το StorSimple πίνακα εικονικού εσωτερικής εγκατάστασης με χρήση της υπηρεσίας διαχείρισης StorSimple στην πύλη του Azure κλασική."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>Χρησιμοποιήστε την υπηρεσία διαχείρισης StorSimple για να διαχειριστείτε το εικονικό πίνακα StorSimple

![ροή διεργασιών εγκατάστασης](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο περιγράφει το περιβάλλον υπηρεσίας διαχείρισης StorSimple, συμπεριλαμβανομένου του τρόπου για τη σύνδεση σε αυτό και τις διάφορες επιλογές που είναι διαθέσιμες, και παρέχει συνδέσεις με τις συγκεκριμένες ροές εργασίας που μπορούν να εκτελεστούν μέσω αυτό περιβάλλοντος εργασίας Χρήστη. 

Μετά την ανάγνωση αυτό το άρθρο, θα γνωρίζετε πώς μπορείτε να:

- Σύνδεση με την υπηρεσία διαχείρισης StorSimple
- Το περιβάλλον εργασίας Χρήστη Διαχείριση StorSimple περιήγηση
- Διαχείριση του πίνακα εικονικού StorSimple μέσω της υπηρεσίας διαχείρισης StorSimple

> [AZURE.NOTE] Για να προβάλετε τις επιλογές διαχείρισης που είναι διαθέσιμες για τη συσκευή σειράς StorSimple 8000, μεταβείτε στο θέμα [χρήση της υπηρεσίας διαχείρισης StorSimple για να διαχειριστείτε τη συσκευή σας StorSimple](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>Σύνδεση με την υπηρεσία διαχείρισης StorSimple

Η υπηρεσία διαχείρισης StorSimple εκτελείται στο Microsoft Azure και συνδέεται με πολλούς πίνακες εικονικού StorSimple. Μπορείτε να χρησιμοποιήσετε μια κεντρική Microsoft Azure κλασική πύλη εκτελείται σε ένα πρόγραμμα περιήγησης για να διαχειριστείτε αυτές τις συσκευές. Για να συνδεθείτε στην υπηρεσία StorSimple Manager, κάντε τα εξής.

#### <a name="to-connect-to-the-service"></a>Για να συνδεθείτε με την υπηρεσία

1. Μεταβείτε στο [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Χρησιμοποιώντας τα διαπιστευτήρια λογαριασμού Microsoft, συνδεθείτε πύλη κλασική Microsoft Azure (που βρίσκεται στην επάνω δεξιά πλευρά του παραθύρου).

3. Κάντε κύλιση προς τα κάτω αριστερό παράθυρο περιήγησης για να αποκτήσετε πρόσβαση στην υπηρεσία StorSimple Manager.

    ![Κάντε κύλιση στην υπηρεσία](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Περιήγηση της υπηρεσίας διαχείρισης StorSimple περιβάλλοντος εργασίας Χρήστη

Ιεραρχία περιήγησης για την υπηρεσία διαχείρισης StorSimple περιβάλλοντος εργασίας Χρήστη εμφανίζεται στον παρακάτω πίνακα.

- Στη σελίδα προορισμού **Διαχείρισης StorSimple** σας μεταφέρει στις σελίδες επιπέδου υπηρεσία περιβάλλοντος εργασίας Χρήστη που ισχύουν για όλα τα εικονικού πίνακες μέσα σε μια υπηρεσία.

- Στη σελίδα **συσκευές** σας μεταφέρει στις σελίδες περιβάλλοντος εργασίας Χρήστη – επίπεδο συσκευής που ισχύουν για ένα συγκεκριμένο εικονικό πίνακα.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>Ιεραρχία περιήγησης service StorSimple Manager

|Σελίδα προορισμού|Σελίδες υπηρεσίας επιπέδου|Σελίδες συσκευών επιπέδου|
|---|---|---|
|Υπηρεσία διαχείρισης StorSimple|Πίνακας εργαλείων (υπηρεσία)|Πίνακας εργαλείων (συσκευή)|
||Συσκευές →|Οθόνη|
||Κατάλογος αντιγράφων ασφαλείας|Κοινόχρηστα στοιχεία (αρχείο server) ή </br>Όγκους (iSCSI server)|
||Ρύθμιση παραμέτρων (υπηρεσίας)|Ρύθμιση παραμέτρων (συσκευή)|
||Εργασίες|Συντήρηση|
||Ειδοποιήσεις|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Χρήση της υπηρεσίας διαχείρισης StorSimple για την εκτέλεση εργασιών διαχείρισης

Ο παρακάτω πίνακας εμφανίζει μια σύνοψη όλων των κοινών εργασιών διαχείρισης και σύνθετες ροές εργασίας που μπορούν να εκτελεστούν εντός της υπηρεσίας διαχείρισης StorSimple περιβάλλοντος εργασίας Χρήστη. Αυτές οι εργασίες είναι οργανωμένα με βάση τις σελίδες περιβάλλοντος εργασίας Χρήστη στον οποίο ξεκίνησε.

Για περισσότερες πληροφορίες σχετικά με κάθε ροή εργασίας, επιλέξτε την κατάλληλη διαδικασία στον πίνακα.

#### <a name="storsimple-manager-workflows"></a>Διαχείριση StorSimple ροές εργασίας

|Εάν θέλετε να κάνετε το εξής...|Μετάβαση σε αυτήν τη σελίδα περιβάλλοντος εργασίας Χρήστη...|Χρησιμοποιήστε αυτήν τη διαδικασία|
|---|---|---|
|Δημιουργία μιας υπηρεσίας</br>Διαγραφή μιας υπηρεσίας</br>Λάβετε το κλειδί εγγραφής υπηρεσίας</br>Αναδημιουργήσετε το κλειδί εγγραφής υπηρεσίας|Υπηρεσία διαχείρισης StorSimple|[Ανάπτυξη της υπηρεσίας διαχείρισης StorSimple](storsimple-ova-manage-service.md)|
|Αλλαγή του κλειδιού κρυπτογράφησης δεδομένων υπηρεσιών</br>Προβάλετε τα αρχεία καταγραφής λειτουργίες|Πίνακας εργαλείων StorSimple Manager service →|[Χρησιμοποιήστε τον πίνακα εργαλείων της υπηρεσίας StorSimple](storsimple-ova-service-dashboard.md)|
|Απενεργοποιήστε μια εικονική πίνακα</br>Διαγραφή ενός εικονικού πίνακα|Διαχείριση StorSimple υπηρεσία → συσκευές|[Απενεργοποίηση ή διαγραφή ενός εικονικού πίνακα](storsimple-ova-deactivate-and-delete-device.md)|
|Ανακατεύθυνση από καταστροφή αποκατάστασης και συσκευής</br>Προαπαιτούμενα στοιχεία ανακατεύθυνσης</br>Ανακατεύθυνση σε μια εικονική συσκευή</br>Επιχειρήσεις συνέχειας αποκατάσταση (BCDR)</br>Σφάλματα κατά την αποκατάσταση|Διαχείριση StorSimple υπηρεσία → συσκευές|[Καταστροφή αποκατάστασης και συσκευή ανακατεύθυνσης για τον πίνακα εικονικού StorSimple](storsimple-ova-failover-dr.md)|
|Δημιουργία αντιγράφου ασφαλείας των κοινόχρηστων στοιχείων και όγκους</br>Μη αυτόματη δημιουργία αντιγράφων ασφαλείας</br>Αλλάξτε το χρονοδιάγραμμα αντιγράφων ασφαλείας</br>Προβάλετε υπάρχοντα αντίγραφα ασφαλείας|Διαχείριση StorSimple υπηρεσία → αντίγραφο ασφαλείας καταλόγου|[Δημιουργία αντιγράφου ασφαλείας σας πίνακα εικονικού StorSimple](storsimple-ova-backup.md)|
|Επαναφορά κοινόχρηστα στοιχεία από ένα σύνολο αντιγράφων ασφαλείας</br>Επαναφορά όγκους από ένα σύνολο αντιγράφων ασφαλείας</br>Ανάκτηση επιπέδου στοιχείου (μόνο για διακομιστή αρχείων)|Κατάλογος StorSimple Manager service → δημιουργίας αντιγράφων ασφαλείας|[Επαναφορά από ένα αντίγραφο ασφαλείας του πίνακα εικονικού StorSimple](storsimple-ova-restore.md)|
|Σχετικά με τους λογαριασμούς χώρου αποθήκευσης</br>Προσθήκη λογαριασμού χώρου αποθήκευσης</br>Επεξεργασία λογαριασμού χώρου αποθήκευσης</br>Διαγραφή ενός λογαριασμού χώρου αποθήκευσης|Ρύθμιση παραμέτρων → service StorSimple Manager|[Διαχείριση λογαριασμών χώρου αποθήκευσης για έναν πίνακα εικονικού StorSimple](storsimple-ova-manage-storage-accounts.md)|
|Πληροφορίες για τις εγγραφές ελέγχου πρόσβασης</br>Προσθήκη ή τροποποίηση μιας εγγραφής ελέγχου πρόσβασης </br>Διαγραφή μιας εγγραφής ελέγχου πρόσβασης|Ρύθμιση παραμέτρων → service StorSimple Manager|[Διαχείριση εγγραφών ελέγχου πρόσβασης για τον πίνακα εικονικού StorSimple](storsimple-ova-manage-acrs.md)|
|Προβολή λεπτομερειών έργου|Διαχείριση StorSimple υπηρεσία → εργασιών| [Διαχείριση εργασιών StorSimple εικονικού πίνακα](storsimple-ova-manage-jobs.md)|
|Ρύθμιση παραμέτρων ειδοποίησης</br>Λήψη ειδοποιήσεων υπενθύμισης</br>Διαχείριση ειδοποιήσεων</br>Εξετάστε τις ειδοποιήσεις|Διαχείριση StorSimple υπηρεσία → ειδοποιήσεων|[Προβολή και διαχείριση ειδοποιήσεων για τον πίνακα εικονικού StorSimple](storsimple-ova-manage-alerts.md)|
|Τροποποιήστε τον κωδικό πρόσβασης διαχειριστή συσκευής|Διαχείριση StorSimple υπηρεσία → συσκευές ρύθμιση παραμέτρων →|[Αλλαγή του κωδικού πρόσβασης διαχειριστή συσκευής StorSimple εικονικού πίνακα](storsimple-ova-change-device-admin-password.md)|
|Εγκατάσταση των ενημερώσεων του λογισμικού|Διαχείριση StorSimple υπηρεσία → συσκευές → συντήρηση|[Ενημερώστε το εικονικό πίνακα](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] Πρέπει να χρησιμοποιήσετε το [τοπικό web περιβάλλοντος εργασίας Χρήστη](storsimple-ova-web-ui-admin.md) για τις ακόλουθες εργασίες:
>
>- [Ανάκτηση του κλειδιού κρυπτογράφησης δεδομένων υπηρεσιών](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Δημιουργία πακέτου υποστήριξης](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Διακοπή και επανεκκίνηση μια εικονική πίνακα](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Επόμενα βήματα
Για πληροφορίες σχετικά με το περιβάλλον εργασίας Χρήστη web και πώς να τη χρησιμοποιείτε, μεταβείτε για [χρήση στο web StorSimple περιβάλλοντος εργασίας Χρήστη για να διαχειριστείτε το εικονικό πίνακα StorSimple](storsimple-ova-web-ui-admin.md).
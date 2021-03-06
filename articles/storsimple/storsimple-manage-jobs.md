<properties 
   pageTitle="Προβολή και διαχείριση εργασιών StorSimple | Microsoft Azure"
   description="Περιγράφει τη σελίδα εργασίες service StorSimple Manager και πώς μπορείτε να το χρησιμοποιήσετε για να παρακολουθείτε πρόσφατες τρέχουσα και προγραμματισμένες εργασίες δημιουργίας αντιγράφων ασφαλείας."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs"></a>Χρήση της υπηρεσίας διαχείρισης StorSimple για προβολή και διαχείριση εργασιών StorSimple

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Επισκόπηση

Στη σελίδα **εργασίες** παρέχει ένα μεμονωμένο κεντρικής πύλης για προβολή και τη διαχείριση των εργασιών που ξεκίνησαν σε συσκευές συνδεδεμένο με την υπηρεσία StorSimple Manager. Μπορείτε να προβάλετε τις εργασίες προγραμματισμένη, εκτελείται, ολοκληρωμένη και αποτυχίας για πολλές συσκευές. Αποτελέσματα παρουσιάζονται σε μορφή πίνακα. 

![Σελίδα "εργασίες"](./media/storsimple-manage-jobs/HCS_JobsPage.png)

Μπορείτε να βρείτε γρήγορα τις εργασίες που θα θέλατε να φιλτράροντας σε πεδία όπως:

- **Κατάσταση** – εργασίες είναι δυνατό να εκτελείται, προγραμματισμένη, αποτυχίας, ολοκληρωμένες, ακύρωση ή ακυρωθεί.

- **Τύπος** – εργασίες μπορούν να δημιουργηθούν ως αποτέλεσμα μια προγραμματισμένη ή ένα αντίγραφο ασφαλείας σε ζήτηση (**Λήψη δημιουργίας αντιγράφων ασφαλείας**), κλωνοποιώντας, μια συσκευή επαναφορά ή μια λειτουργία ενημέρωσης.

- **Συσκευές** – εργασίες έχουν ξεκινήσει σε μια συγκεκριμένη συσκευή συνδεδεμένο με την υπηρεσία.

- **Από και έως** – εργασίες μπορούν να φιλτραριστούν με βάση το εύρος ημερομηνίας και ώρας.

Στη συνέχεια, είναι πινακοποιημένη τις φιλτραρισμένες εργασίες με βάση τα παρακάτω χαρακτηριστικά:

- **Τύπος** – δημιουργία αντιγράφων ασφαλείας, αντιγραφής, επαναφορά, ανακατεύθυνση ή ενημέρωση.

- **Κατάσταση** – εκτελείται, προγραμματισμένη, αποτυχίας, ολοκληρωμένων, ακύρωση ή ακυρωθεί.

- **Οντότητα** – τις εργασίες μπορούν να συσχετιστούν με ένα ένταση, μια πολιτική ασφαλείας ή μια συσκευή. Κλωνοποίηση εργασίας είναι συσχετισμένη με μια όγκος, ότι μια προγραμματισμένη εργασία αντιγράφου ασφαλείας είναι συσχετισμένη με μια πολιτική ασφαλείας. Δημιουργείται μια συσκευή έργου ως αποτέλεσμα αποκατάσταση (DR) ή μια λειτουργία επαναφοράς.

- **Συσκευή** -στο όνομα της συσκευής στην οποία ξεκίνησε η εργασία.

- **Αποτελέσματα στην** – την ώρα έναρξης της εργασίας.

- **Πρόοδος** – το ποσοστό ολοκλήρωσης μιας εργασίας που εκτελείται. Για ένα ολοκληρωμένο έργο, αυτό θα πρέπει να είναι πάντα 100%.

Λίστα εργασιών ανανεώνονται κάθε 30 δευτερόλεπτα.

Μπορείτε να εκτελέσετε τις ακόλουθες ενέργειες που σχετίζονται με την εργασία σε αυτήν τη σελίδα:

- Προβολή λεπτομερειών έργου

- Ακύρωση μιας εργασίας

## <a name="view-job-details"></a>Προβολή λεπτομερειών έργου

Ακολουθήστε τα παρακάτω βήματα για να προβάλετε τις λεπτομέρειες της οποιαδήποτε εργασία.

#### <a name="to-view-job-details"></a>Για να προβάλετε λεπτομέρειες εργασίας

1. Στη σελίδα " **εργασίες** ", εμφάνιση Υπόλ που θα θέλατε να με την εκτέλεση ενός ερωτήματος με τα κατάλληλα φίλτρα. Μπορείτε να κάνετε αναζήτηση για την ολοκλήρωση, εκτελεί ή ακυρώθηκε εργασίες.

2. Επιλέξτε μια εργασία.

3. Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Λεπτομέρειες**.

4. Στο παράθυρο διαλόγου **Λεπτομέρειες εργασίας δημιουργίας αντιγράφων ασφαλείας** , μπορείτε να προβάλετε την κατάσταση, λεπτομέρειες, Στατιστικά χρόνου και στατιστικά στοιχεία δεδομένων.

## <a name="cancel-a-job"></a>Ακύρωση μιας εργασίας

Ακολουθήστε τα παρακάτω βήματα για να ακυρώσετε μια εργασία που εκτελείται.

### <a name="to-cancel-a-job"></a>Για να ακυρώσετε μια εργασία

1. Στη σελίδα " **εργασίες** ", εμφάνιση εκτελείται Υπόλ που θέλετε να ακυρώσετε με την εκτέλεση ενός ερωτήματος με τα κατάλληλα φίλτρα.

1. Επιλέξτε την εργασία.

1. Στο κάτω μέρος της σελίδας, κάντε κλικ στην επιλογή **Άκυρο**.

1. Όταν σας ζητηθεί επιβεβαίωση, κάντε κλικ στο κουμπί **Ναι**.

Αυτή η εργασία είναι τώρα ακυρώθηκε.

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε πώς μπορείτε να [διαχειριστείτε τις πολιτικές αντιγράφου ασφαλείας σας StorSimple](storsimple-manage-backup-policies.md).

- Μάθετε πώς μπορείτε να [χρησιμοποιήσετε την υπηρεσία διαχείρισης StorSimple για να διαχειριστείτε τη συσκευή σας StorSimple](storsimple-manager-service-administration.md).

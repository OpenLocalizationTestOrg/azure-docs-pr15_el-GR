<properties
   pageTitle="Υπηρεσίες ανάκτησης φύλαξης συνήθεις Ερωτήσεις | Microsoft Azure"
   description="Αυτή η έκδοση του στις συνήθεις Ερωτήσεις υποστηρίζει έκδοση Preview δημόσια της υπηρεσίας Azure δημιουργίας αντιγράφων ασφαλείας. Απαντήσεις σε συνήθεις ερωτήσεις σχετικά με τον παράγοντα αντιγράφων ασφαλείας, δημιουργία αντιγράφων ασφαλείας και διατήρησης, ανάκτησης, ασφάλεια και άλλες συνήθεις ερωτήσεις σχετικά με τη λύση Azure δημιουργίας αντιγράφων ασφαλείας."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="λύση δημιουργίας αντιγράφων ασφαλείας. υπηρεσίας δημιουργίας αντιγράφων ασφαλείας"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Θάλαμο υπηρεσίες ανάκτησης - συνήθεις Ερωτήσεις


Σε αυτό το άρθρο παρέχει πληροφορίες σχετικά με τις υπηρεσίες ανάκτησης θάλαμο και το συμπληρώνουν [Azure συνήθεις ερωτήσεις για τη δημιουργία αντιγράφων ασφαλείας](backup-azure-backup-faq.md). Συνήθεις Ερωτήσεις για δημιουργία αντιγράφων ασφαλείας του Azure παρέχει το πλήρες σύνολο των ερωτήσεων και απαντήσεων σχετικά με την υπηρεσία Azure δημιουργίας αντιγράφων ασφαλείας.  

Μπορείτε να υποβάλετε ερωτήσεις σχετικά με τη δημιουργία αντιγράφων ασφαλείας Azure στην ενότητα Disqus σε αυτό το άρθρο ή ένα σχετικό άρθρο. Μπορείτε επίσης να δημοσιεύσετε ερωτήσεις σχετικά με την υπηρεσία Azure δημιουργίας αντιγράφων ασφαλείας στο [φόρουμ συζήτησης](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Χώροι φύλαξης υπηρεσίες ανάκτησης είναι διαχείριση πόρων με βάση. Δημιουργία αντιγράφων ασφαλείας χώροι φύλαξης (Κλασική λειτουργία) εξακολουθεί να υποστηρίζονται; <br/>
Ναι, εξακολουθεί να υποστηρίζονται χώροι φύλαξης δημιουργίας αντιγράφων ασφαλείας. Δημιουργήστε αντίγραφα ασφαλείας χώροι φύλαξης στην [πύλη κλασική](https://manage.windowsazure.com). Δημιουργία χώροι φύλαξης υπηρεσίες ανάκτησης στην [πύλη του Azure](https://portal.azure.com). Ωστόσο, συνιστάται ιδιαίτερα να να δημιουργήσετε θάλαμο υπηρεσίες ανάκτησης ως όλες οι μελλοντικές βελτιώσεις θα είναι διαθέσιμο μόνο σε υπηρεσίες ανάκτησης θάλαμο.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Μπορώ να μετεγκαταστήσω θάλαμο ένα αντίγραφο ασφαλείας σε ένα θάλαμο υπηρεσίες ανάκτησης; <br/>
Δυστυχώς δεν, αυτήν τη στιγμή που δεν είναι δυνατό να μετεγκαταστήσετε τα περιεχόμενα των θάλαμο ένα αντίγραφο ασφαλείας σε ένα θάλαμο υπηρεσίες ανάκτησης. Εργαζόμαστε για την προσθήκη αυτήν τη λειτουργικότητα, αλλά δεν είναι διαθέσιμη ως μέρος του Public Preview.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Υπηρεσίες ανάκτησης χώροι φύλαξης υποστηρίζουν κλασική ΣΠΣ ή διαχείριση πόρων βάσει ΣΠΣ; <br/>
Χώροι φύλαξης υπηρεσίες ανάκτησης υποστηρίζουν και τα δύο μοντέλων.  Μπορείτε να δημιουργήσετε αντίγραφα ασφαλείας μια Εικονική που έχουν δημιουργηθεί με την πύλη κλασική (το οποίο είναι ΣΠΣ Κλασική λειτουργία) ή μια Εικονική που δημιουργήσατε στην πύλη του Azure (το οποίο είναι διαχείριση πόρων βάσει) σε ένα θάλαμο υπηρεσίες ανάκτησης.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Να έχετε δημιουργήσει αντίγραφα ασφαλείας μου κλασική ΣΠΣ στο θάλαμο δημιουργίας αντιγράφων ασφαλείας. Τώρα θέλω να μετεγκαταστήσετε μου ΣΠΣ από Κλασική λειτουργία λειτουργία διαχείρισης πόρων.  Πώς μπορώ να δημιουργία αντιγράφων ασφαλείας τους στο θάλαμο υπηρεσίες ανάκτησης;
Αντίγραφα ασφαλείας των κλασική ΣΠΣ στο αντίγραφο ασφαλείας θάλαμο δεν θα μετεγκατασταθούν αυτόματα σε θάλαμο υπηρεσίες ανάκτησης κατά τη μετεγκατάσταση του ΣΠΣ από Κλασική λειτουργία διαχείρισης πόρων. Ακολουθήστε τα παρακάτω βήματα για τη μετεγκατάσταση των αντιγράφων ασφαλείας Εικονική:

1. Στο αντίγραφο ασφαλείας θάλαμο, μεταβείτε στην καρτέλα **Προστασία στοιχεία** και επιλέξτε η Εικονική. Κάντε κλικ στην εντολή [Διακοπή προστασίας](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Μην *διαγράψετε δεδομένα αντιγράφου ασφαλείας που σχετίζονται* επιλογή **απενεργοποιημένο**.
2. Μετεγκατάσταση η εικονική μηχανή από Κλασική λειτουργία λειτουργία διαχείρισης πόρων. Βεβαιωθείτε ότι χώρου αποθήκευσης και δικτύου που αντιστοιχεί στο εικονική μηχανή επίσης μετεγκαθίστανται λειτουργία διαχείρισης πόρων.
3. Δημιουργήστε ένα θάλαμο υπηρεσίες ανάκτησης και ρύθμιση παραμέτρων δημιουργίας αντιγράφων ασφαλείας στο την μετεγκατάσταση εικονική μηχανή χρησιμοποιώντας **αντιγράφου ασφαλείας** ενέργεια επάνω από θάλαμο πίνακα εργαλείων. Μάθετε περισσότερα σχετικά με την [Ενεργοποίηση δημιουργίας αντιγράφων ασφαλείας στο θάλαμο υπηρεσίες ανάκτησης](backup-azure-vms-first-look-arm.md)
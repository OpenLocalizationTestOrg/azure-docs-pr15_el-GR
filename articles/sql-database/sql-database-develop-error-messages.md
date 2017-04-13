<properties
    pageTitle="Κωδικοί σφάλματος SQL - σφάλμα σύνδεσης βάσης δεδομένων | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL, όπως συνηθισμένων σφαλμάτων σύνδεσης βάσης δεδομένων, ζητήματα αντίγραφο της βάσης δεδομένων και γενικά σφάλματα. "
    keywords="Κωδικός σφάλματος SQL, sql της access, σφάλμα σύνδεσης βάσης δεδομένων, τους κωδικούς σφάλματος sql"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Κωδικοί σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL: σφάλμα σύνδεσης και άλλα θέματα της βάσης δεδομένων


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Σε αυτό το άρθρο παραθέτει τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL, συμπεριλαμβανομένων σφαλμάτων σύνδεσης βάσης δεδομένων, μεταβατικές σφάλματα (ονομάζεται επίσης μεταβατικές σφάλματα), σφάλματα διακυβέρνησης πόρων, ζητήματα αντίγραφο της βάσης δεδομένων, ελαστικά χώρο συγκέντρωσης και άλλα σφάλματα. Οι περισσότερες κατηγορίες έχουν συγκεκριμένη βάση δεδομένων SQL Azure και δεν εφαρμόζονται στο Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Βάση δεδομένων σφαλμάτων σύνδεσης, μεταβατικές σφάλματα και άλλα σφάλματα προσωρινά

Ο παρακάτω πίνακας περιγράφει τους κωδικούς σφάλματος SQL για σφάλματα απώλεια σύνδεσης, καθώς και άλλα μεταβατικές σφάλματα που μπορεί να αντιμετωπίσετε κατά την εφαρμογή προσπαθεί να αποκτήσει πρόσβαση σε βάση δεδομένων SQL. Για γρήγορα αποτελέσματα προγράμματα εκμάθησης σχετικά με τον τρόπο σύνδεσης με βάση δεδομένων SQL Azure, ανατρέξτε στο θέμα [σύνδεση με βάση δεδομένων SQL Azure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Πιο συνηθισμένα σφάλματα σύνδεσης βάσης δεδομένων και μεταβατικές σφαλμάτων

Η υποδομή Azure έχει τη δυνατότητα να δυναμικά ρυθμίσει ξανά τις παραμέτρους των διακομιστών, όταν προκύπτουν μεγάλο φόρτο εργασίας στην υπηρεσία βάσης δεδομένων SQL.  Αυτή η συμπεριφορά δυναμικής ενδέχεται να προκαλέσει το πρόγραμμα-πελάτης να χάσετε τη σύνδεση με βάση δεδομένων SQL. Αυτό το είδος συνθήκη σφάλματος ονομάζεται μια *μεταβατικές σφαλμάτων*.

Εάν το πρόγραμμα-πελάτη σας έχει λογικής "Επανάληψη", το να δοκιμάσετε να αποκαταστήσει τη σύνδεση αφού καταχωρίσετε την ώρα μεταβατικές σφαλμάτων για να διορθωθεί.  Συνιστάται να ότι μπορείτε καθυστερήσετε για 5 δευτερόλεπτα πριν από την πρώτη "Επανάληψη". Επανάληψη μετά από μια καθυστέρηση μικρότερη από 5 δευτερόλεπτα τους κινδύνους overwhelming την υπηρεσία cloud. Για κάθε οι επόμενες επανάληψη προσπάθειας την καθυστέρηση θα πρέπει να αυξηθεί εκθετικά, έως και 60 δευτερόλεπτα.

Μεταβατικές σφαλμάτων δήλωσης συνήθως ως ένα από τα ακόλουθα μηνύματα σφάλματος από τα προγράμματα-πελάτες:

- Βάση δεδομένων < db_name > σε διακομιστή < Azure_instance > δεν είναι διαθέσιμη αυτήν τη στιγμή. Προσπαθήστε ξανά αργότερα τη σύνδεση. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη πελατών και δώστε τους το Αναγνωριστικό περιόδου λειτουργίας ανίχνευσης < session_id >

- Βάση δεδομένων < db_name > σε διακομιστή < Azure_instance > δεν είναι διαθέσιμη αυτήν τη στιγμή. Προσπαθήστε ξανά αργότερα τη σύνδεση. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη πελατών και δώστε τους το Αναγνωριστικό περιόδου λειτουργίας ανίχνευσης < session_id >. (Microsoft SQL Server, σφάλματος: 40613)

- Μια υπάρχουσα σύνδεση διακόπηκε υποχρεωτικά από τον απομακρυσμένο υπολογιστή.

- System.Data.Entity.Core.EntityCommandExecutionException: Παρουσιάστηκε σφάλμα κατά την εκτέλεση τον ορισμό της εντολής. Ανατρέξτε στην εσωτερική εξαίρεση για λεπτομέρειες. ---> System.Data.SqlClient.SqlException: Παρουσιάστηκε ένα σφάλμα επιπέδου μεταφοράς κατά τη λήψη αποτελεσμάτων από το διακομιστή. (υπηρεσία παροχής: υπηρεσία παροχής περιόδου λειτουργίας, σφάλματος: 19 - φυσική σύνδεση δεν μπορεί να χρησιμοποιηθεί)

Για παραδείγματα κώδικα της λογικής "Επανάληψη", ανατρέξτε στα θέματα:

- [Βιβλιοθήκες σύνδεσης για τη βάση δεδομένων SQL και του SQL Server](sql-database-libraries.md) 
- [Ενέργειες για να διορθώσετε σφάλματα σύνδεσης και μεταβατικές σφάλματα στη βάση δεδομένων SQL](sql-database-connectivity-issues.md)

Μια συζήτηση σχετικά με τον *αποκλεισμό περίοδο* για προγράμματα-πελάτες που χρησιμοποιούν το ADO.NET είναι διαθέσιμη στο [SQL Server σύνδεσης συγκέντρωση (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Κωδικοί σφάλματος μεταβατικές σφαλμάτων

Τα ακόλουθα σφάλματα είναι μεταβατικές και πρέπει να επαναληφθεί στο λογική εφαρμογής 

| Κωδικός σφάλματος | Σοβαρότητας | Περιγραφή |
| ---: | ---: | :--- |
| 4060 | 16 | Δεν είναι δυνατό να Άνοιγμα βάσης δεδομένων "% & #x2a, ls" ζητήθηκε από τη σύνδεση. Απέτυχε η σύνδεση. |
|40197|17|Η υπηρεσία αντιμετώπισε σφάλμα κατά την επεξεργασία της αίτησης. Προσπαθήστε ξανά. Κωδικός σφάλματος %d.<br/><br/>Θα λάβετε αυτό το σφάλμα, όταν η υπηρεσία είναι προς τα κάτω, λόγω λογισμικού ή αναβαθμίσεις υλικού, αποτυχίες υλικού ή άλλα προβλήματα ανακατεύθυνσης. Ο κωδικός σφάλματος (%d) ενσωματωθεί μέσα στο μήνυμα σφάλματος 40197 παρέχει πρόσθετες πληροφορίες σχετικά με το είδος θα αποτύχει ή ανακατεύθυνσης που έγιναν. Ορισμένα παραδείγματα του σφάλματος είναι ενσωματωμένες κώδικες μέσα στο μήνυμα σφάλματος 40197 είναι 40020, 40143, 40166 και 40540.<br/><br/>Επανασύνδεση στο διακομιστή βάσης δεδομένων SQL θα σας συνδέει αυτόματα σε ένα άρτιο αντίγραφο της βάσης δεδομένων σας. Η εφαρμογή σας πρέπει να ενημερωθείτε 40197, αρχείο καταγραφής σφαλμάτων τον κωδικό σφάλματος ενσωματωμένο (%d) μέσα στο μήνυμα για την αντιμετώπιση προβλημάτων και προσπαθήστε να συνδεθείτε με βάση δεδομένων SQL μέχρι οι πόροι είναι διαθέσιμοι και τη σύνδεσή σας είναι εγκατεστημένος ξανά.|
|40501|20|Η υπηρεσία είναι απασχολημένη αυτήν τη στιγμή. Προσπαθήστε ξανά μετά από 10 δευτερόλεπτα. Το περιστατικό Αναγνωριστικό: %ls. Κωδικός: %d.<br/><br/>*Σημείωση:* Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:<br/>• [Όρια πόρων βάση δεδομένων SQL azure](sql-database-resource-limits.md).
|40613|17|Βάση δεδομένων '% & #x2a; ls' στο διακομιστή '% & #x2a; ls' δεν είναι διαθέσιμη αυτήν τη στιγμή. Προσπαθήστε ξανά αργότερα τη σύνδεση. Εάν το πρόβλημα δεν επιλυθεί, επικοινωνήστε με την υποστήριξη πελατών και δώστε τους το Αναγνωριστικό ανίχνευση περίοδο λειτουργίας '% & #x2a; ls'.|
|49918|16|Δεν είναι δυνατή η επεξεργασία της αίτησης. Οι πόροι δεν επαρκούν για την επεξεργασία αίτησης.<br/><br/>Η υπηρεσία είναι απασχολημένη αυτήν τη στιγμή. Προσπαθήστε ξανά αργότερα την αίτηση. |
|49919|16|Δεν είναι δυνατό να διαδικασίας Δημιουργία ή ενημέρωση αίτησης. Πάρα πολλές Δημιουργία ή ενημέρωση λειτουργιών σε εξέλιξη για τη συνδρομή "% ld".<br/><br/>Η υπηρεσία είναι απασχολημένη επεξεργασία πολλών Δημιουργία ή ενημέρωση αιτήσεων για τη συνδρομή σας ή το διακομιστή. Προσκλήσεις σε αυτήν τη στιγμή που αποκλείονται για βελτιστοποίηση των πόρων. Ερώτημα [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) για εκκρεμείς εργασίες. Περιμένετε έως ότου σε εκκρεμότητα Δημιουργία ή ενημέρωση αιτήσεις έχουν ολοκληρωθεί ή διαγράψετε μία από τις αιτήσεις σε εκκρεμότητα και προσπαθήστε ξανά την αίτησή σας αργότερα. |
|49920|16|Δεν είναι δυνατή η επεξεργασία της αίτησης. Πάρα πολλές εργασίες σε εξέλιξη για τη συνδρομή "% ld".<br/><br/>Η υπηρεσία είναι απασχολημένη με την επεξεργασία πολλών αιτήσεων για αυτήν τη συνδρομή. Προσκλήσεις σε αυτήν τη στιγμή που αποκλείονται για βελτιστοποίηση των πόρων. Ερώτημα [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) για την κατάσταση λειτουργίας. Περιμένετε έως ότου εκκρεμείς αιτήσεις έχουν ολοκληρωθεί ή διαγραφή μία από τις αιτήσεις σε αναμονή και προσπαθήστε ξανά την αίτησή σας αργότερα. |

## <a name="database-copy-errors"></a>Σφάλματα αντίγραφο της βάσης δεδομένων

Τα ακόλουθα σφάλματα μπορεί να εμφανίζεται κατά την αντιγραφή μιας βάσης δεδομένων σε βάση δεδομένων SQL Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων SQL Azure](sql-database-copy.md).


|Κωδικός σφάλματος|Σοβαρότητας|Περιγραφή|
|---:|---:|:---|
|40635|16|Πρόγραμμα-πελάτη με διεύθυνση IP '% & #x2a; ls' έχει απενεργοποιηθεί προσωρινά.|
|40637|16|Δημιουργία αντιγράφου της βάσης δεδομένων είναι απενεργοποιημένη.|
|40561|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Βάση δεδομένων προέλευσης ή προορισμού δεν υπάρχει.|
|40562|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Έχει διακοπεί η βάση δεδομένων προέλευσης.|
|40563|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Έχει διακοπεί η βάση δεδομένων προορισμού.|
|40564|16|Αντίγραφο της βάσης δεδομένων απέτυχε λόγω εσωτερικό σφάλμα. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά.|
|40565|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Επιτρέπεται λιγότερα από 1 αντίγραφο ταυτόχρονες βάσης δεδομένων από την ίδια προέλευση. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά αργότερα.|
|40566|16|Αντίγραφο της βάσης δεδομένων απέτυχε λόγω εσωτερικό σφάλμα. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά.|
|40567|16|Αντίγραφο της βάσης δεδομένων απέτυχε λόγω εσωτερικό σφάλμα. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά.|
|40568|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Βάση δεδομένων προέλευσης έχει γίνει διαθέσιμος. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά.|
|40569|16|Απέτυχε η αντιγραφή της βάσης δεδομένων. Βάση δεδομένων προορισμού έχει γίνει διαθέσιμος. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά.|
|40570|16|Αντίγραφο της βάσης δεδομένων απέτυχε λόγω εσωτερικό σφάλμα. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά αργότερα.|
|40571|16|Αντίγραφο της βάσης δεδομένων απέτυχε λόγω εσωτερικό σφάλμα. Πληκτρολογήστε Απόρριψη βάσης δεδομένων προορισμού και προσπαθήστε ξανά αργότερα.|

## <a name="resource-governance-errors"></a>Σφάλματα διακυβέρνησης πόρων

Τα ακόλουθα σφάλματα προκαλούνται από υπερβολική χρήση πόρων κατά την εργασία με βάση δεδομένων SQL Azure. Για παράδειγμα:

- Μια συναλλαγή έχει ανοιχτό για μεγάλο χρονικό διάστημα.
- Μια συναλλαγή κρατώντας πατημένο το πάρα πολλά κλειδωμάτων.
- Μια εφαρμογή καταναλώνει πάρα πολύ μνήμης.
- Μια εφαρμογή καταναλώνει πάρα πολύ `TempDb` χώρο.

Σχετικά θέματα:

* Πιο λεπτομερείς πληροφορίες είναι διαθέσιμες εδώ: [βάση δεδομένων SQL Azure όρια πόρων](sql-database-resource-limits.md)

|Κωδικός σφάλματος|Σοβαρότητας|Περιγραφή|
|---:|---:|:---|
|10928|20|Αναγνωριστικό πόρου: %d. Το όριο %s για τη βάση δεδομένων είναι %d και συμπληρώθηκε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Το Αναγνωριστικό πόρου υποδεικνύει τον πόρο που έχει φτάσει το όριο. Για νήματα εργασίας, το Αναγνωριστικό πόρου = 1. Για περιόδους λειτουργίας, το Αναγνωριστικό πόρου = 2.<br/><br/>*Σημείωση:* Για περισσότερες πληροφορίες σχετικά με αυτό το σφάλμα και πώς μπορείτε να επιλύσετε, ανατρέξτε στα θέματα:<br/>• [Όρια πόρων βάση δεδομένων SQL azure](sql-database-resource-limits.md). |
|10929|20|Αναγνωριστικό πόρου: %d. Η ελάχιστη εγγύηση %s είναι %d, μέγιστο όριο είναι %d και την τρέχουσα χρήση για τη βάση δεδομένων είναι %d. Ωστόσο, ο διακομιστής αυτήν τη στιγμή είναι πολύ απασχολημένοι για την υποστήριξη μεγαλύτερο από %d αιτήσεις για αυτήν τη βάση δεδομένων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Διαφορετικά, προσπαθήστε ξανά αργότερα.<br/><br/>Το Αναγνωριστικό πόρου υποδεικνύει τον πόρο που έχει φτάσει το όριο. Για νήματα εργασίας, το Αναγνωριστικό πόρου = 1. Για περιόδους λειτουργίας, το Αναγνωριστικό πόρου = 2.<br/><br/>*Σημείωση:* Για περισσότερες πληροφορίες σχετικά με αυτό το σφάλμα και πώς μπορείτε να επιλύσετε, ανατρέξτε στα θέματα:<br/>• [Όρια πόρων βάση δεδομένων SQL azure](sql-database-resource-limits.md).|
|40544|20|Η βάση δεδομένων έχει φτάσει το όριο μεγέθους της. Διαμερίσματα ή διαγραφή δεδομένων, αποθέστε ευρετήρια ή ανατρέξτε στην τεκμηρίωση για πιθανές λύσεις.|
|40549|16|Λήξη της περιόδου λειτουργίας επειδή έχετε μια μεγάλη διάρκεια εκτέλεσης συναλλαγή. Δοκιμάστε συντόμευση συναλλαγής σας.|
|40550|16|Η περίοδος λειτουργίας τερματίστηκε επειδή αποκτήσει πάρα πολλά κλειδωμάτων. Δοκιμή ανάγνωσης ή τροποποίηση λιγότερες γραμμές σε μία συναλλαγή.|
|40551|16|Η περίοδος λειτουργίας τερματίστηκε λόγω πλεονάζουσα `TEMPDB` χρήση. Προσπαθήστε να τροποποιήσετε το ερώτημα για να μειώσετε τη χρήση του χώρου προσωρινό πίνακα.<br/><br/>*Συμβουλή:* Εάν χρησιμοποιείτε προσωρινά αντικείμενα, να εξοικονομήσετε χώρο στο το `TEMPDB` βάσης δεδομένων αποθέτοντας προσωρινά αντικείμενα, αφού δεν είναι πλέον απαραίτητες από την περίοδο λειτουργίας.|
|40552|16|Η περίοδος λειτουργίας τερματίστηκε λόγω χρήση χώρου καταγραφής πλεονάζουσα συναλλαγή. Προσπαθήστε να τροποποιήσετε λιγότερες γραμμές σε μία συναλλαγή.<br/><br/>*Συμβουλή:* Εάν εκτελείτε μαζικές εισάγει χρησιμοποιώντας το `bcp.exe` βοηθητικού προγράμματος ή το `System.Data.SqlClient.SqlBulkCopy` κλάση, δοκιμάστε να χρησιμοποιήσετε το `-b batchsize` ή `BatchSize` επιλογές για να περιορίσετε τον αριθμό των γραμμών που αντιγράψατε στο διακομιστή σε κάθε συναλλαγή. Εάν φτιάχνετε ένα ευρετήριο με τα `ALTER INDEX` πρόταση, δοκιμάστε να χρησιμοποιήσετε το `REBUILD WITH ONLINE = ON` την επιλογή.|
|40553|16|Η περίοδος λειτουργίας τερματίστηκε λόγω υπερβολικής χρήσης μνήμης. Προσπαθήστε να τροποποιήσετε το ερώτημα για την επεξεργασία λιγότερες γραμμές.<br/><br/>*Συμβουλή:* Μείωση του αριθμού των `ORDER BY` και `GROUP BY` λειτουργίες στον κώδικα Transact-SQL μειώνει τις απαιτήσεις μνήμης του ερωτήματός σας.|

## <a name="elastic-pool-errors"></a>Σφάλματα ελαστικότητας χώρου συγκέντρωσης

Τα ακόλουθα σφάλματα σχετίζονται με τη δημιουργία και χρήση Elastics χώρους συγκέντρωσης.

| Αριθμός_σφάλματος | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Ο χώρος συγκέντρωσης ελαστικότητας έχει φτάσει στο όριο χώρου αποθήκευσης. Η χρήση του χώρου αποθήκευσης για το χώρο συγκέντρωσης ελαστικά δεν πρέπει να υπερβαίνει MB (%d). | Όριο χώρου συγκέντρωσης ελαστικότητας σε MB. | Εγγραφή δεδομένων σε μια βάση δεδομένων, όταν το όριο χώρου αποθήκευσης του χώρου συγκέντρωσης ελαστικότητας συμπληρώθηκε προσπάθεια. | Πληκτρολογήστε ίσως χρειάζεται να αυξήσετε το DTUs του ελαστικότητας χώρου συγκέντρωσης εάν είναι δυνατόν για να αυξήσετε το όριο χώρου αποθήκευσης, να μειώσετε το χώρο αποθήκευσης που χρησιμοποιούνται από τις μεμονωμένες βάσεις δεδομένων εντός του χώρου συγκέντρωσης ελαστικά ή κατάργηση βάσεις δεδομένων από το χώρο συγκέντρωσης ελαστικότητας. |
| 10929 | EX_USER | Η ελάχιστη εγγύηση %s είναι %d, μέγιστο όριο είναι %d και την τρέχουσα χρήση για τη βάση δεδομένων είναι %d. Ωστόσο, ο διακομιστής αυτήν τη στιγμή είναι πολύ απασχολημένοι για την υποστήριξη μεγαλύτερο από %d αιτήσεις για αυτήν τη βάση δεδομένων. Ανατρέξτε στο θέμα [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) για βοήθεια. Διαφορετικά, προσπαθήστε ξανά αργότερα. | Min DTU ανά βάση δεδομένων. Max DTU ανά βάση δεδομένων | Ο συνολικός αριθμός των εργαζομένων ταυτόχρονες (αιτήσεις) σε όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης ελαστικότητας επιχειρήσατε να υπερβαίνει το όριο του χώρου συγκέντρωσης. | Πληκτρολογήστε ίσως χρειάζεται να αυξήσετε το DTUs του ελαστικότητας χώρου συγκέντρωσης εάν είναι δυνατόν για να αυξήσετε το χρονικό όριο εργαζόμενου ή κατάργηση βάσεις δεδομένων από το χώρο συγκέντρωσης ελαστικότητας. |
| 40844 | EX_USER | Βάση δεδομένων '%ls' στο διακομιστή '%ls' είναι μια βάση δεδομένων edition "%ls" σε ένα σύνολο ελαστικά και δεν μπορεί να έχει μια σχέση συνεχής αντίγραφο. | όνομα βάσης δεδομένων, edition βάσης δεδομένων, το όνομα του διακομιστή | Μια εντολή StartDatabaseCopy εκδίδεται για μια μη premium db σε ένα σύνολο ελαστικότητας. | Έρχομαι σύντομα |
| 40857 | EX_USER | Ελαστικά χώρου συγκέντρωσης δεν βρέθηκε για διακομιστή: '%ls', το όνομα του χώρου συγκέντρωσης ελαστικότητας: '%ls'. | όνομα του διακομιστή. όνομα ελαστικότητας χώρου συγκέντρωσης | Καθορισμένο ελαστικότητας χώρου συγκέντρωσης δεν υπάρχει στο καθορισμένο διακομιστή. | Δώστε ένα όνομα έγκυρη ελαστικότητας χώρου συγκέντρωσης. |
| 40858 | EX_USER | Ελαστικά χώρος συγκέντρωσης '%ls' υπάρχει ήδη στο διακομιστή: '%ls' | όνομα χώρου συγκέντρωσης ελαστικά, το όνομα του διακομιστή | Καθορισμένο ελαστικότητας χώρου συγκέντρωσης υπάρχει ήδη στο καθορισμένο λογική διακομιστή. | Δώστε νέο όνομα ελαστικότητας χώρο συγκέντρωσης. |
| 40859 | EX_USER | Ελαστικά χώρου συγκέντρωσης δεν υποστηρίζει το επίπεδο υπηρεσιών '%ls'. | επίπεδο υπηρεσιών ελαστικότητας χώρο συγκέντρωσης | Επίπεδο καθορισμένο υπηρεσιών δεν υποστηρίζεται για παροχή ελαστικότητας χώρου συγκέντρωσης. | Δώστε τη σωστή έκδοση ή αφήστε κενό για να χρησιμοποιήσετε το προεπιλεγμένο επίπεδο υπηρεσιών επίπεδο υπηρεσιών. |
| 40860 | EX_USER | Συνδυασμός ελαστικότητας χώρου συγκέντρωσης '%ls' και της υπηρεσίας στόχος '%ls' δεν είναι έγκυρος. | όνομα χώρου συγκέντρωσης ελαστικότητας; στόχου ονόματος στο επίπεδο υπηρεσίας | Ελαστικά χώρο συγκέντρωσης και της υπηρεσίας στόχος μπορεί να καθοριστεί μαζί μόνο εάν ο στόχος υπηρεσίας καθορίζεται ως 'ElasticPool'. | Καθορίστε σωστό συνδυασμό ελαστικότητας χώρο συγκέντρωσης και στόχος υπηρεσίας. |
| 40861 | EX_USER | Η έκδοση βάσης δεδομένων ' %. *των ls δεν μπορεί να είναι διαφορετικά από το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης που είναι ' %.* ls'. | βάση δεδομένων edition, επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης | Η έκδοση βάσης δεδομένων είναι διαφορετικό από το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Καθορίζει μια έκδοση βάσης δεδομένων που είναι διαφορετικό από το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης.  Σημειώστε ότι η έκδοση βάσης δεδομένων δεν χρειάζεται να έχει καθοριστεί. |
| 40862 | EX_USER | Όνομα ελαστικότητας χώρου συγκέντρωσης πρέπει να καθορίσει εάν το στόχο υπηρεσίας ελαστικότητας χώρος συγκέντρωσης έχει καθοριστεί. | Κανένας | Στόχος υπηρεσίας ελαστικότητας χώρου συγκέντρωσης προσδιορίζει με ένα χώρο συγκέντρωσης ελαστικότητας. | Καθορίστε το όνομα χώρου συγκέντρωσης ελαστικότητας Εάν χρησιμοποιείτε το στόχο υπηρεσίας ελαστικότητας χώρο συγκέντρωσης. |
| 40864 | EX_USER | Το DTUs για το χώρο συγκέντρωσης ελαστικά πρέπει να είναι τουλάχιστον (%d) σε DTUs για το επίπεδο υπηρεσιών ' %. * των ls. | DTUs για το χώρο συγκέντρωσης ελαστικότητας; επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Προσπαθείτε να ορίσετε το DTUs για το χώρο συγκέντρωσης ελαστικότητας κάτω από το ελάχιστο όριο. | Δοκιμάστε ξανά τη ρύθμιση του DTUs για το ελαστικές χώρο συγκέντρωσης τουλάχιστον το ελάχιστο όριο. |
| 40865 | EX_USER | Το DTUs για το χώρο συγκέντρωσης ελαστικά δεν πρέπει να υπερβαίνει (%d) σε DTUs για το επίπεδο υπηρεσιών ' %. * των ls. | DTUs για το χώρο συγκέντρωσης ελαστικότητας; επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Προσπαθείτε να ορίσετε το DTUs για το χώρο συγκέντρωσης ελαστικότητας επάνω από το μέγιστο όριο. | Δοκιμάστε ξανά τη ρύθμιση του DTUs για το χώρο συγκέντρωσης ελαστικότητας να δεν είναι μεγαλύτερο από το μέγιστο όριο. |
| 40867 | EX_USER | Πρέπει να είναι το μέγιστο ανά βάση δεδομένων DTU τουλάχιστον (%d) για το επίπεδο υπηρεσιών ' %. * των ls. | Max DTU ανά βάση δεδομένων. επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης | Προσπαθείτε να ορίσετε το μέγιστο DTU ανά βάση δεδομένων κάτω από το όριο που υποστηρίζεται. | Εξετάστε το ενδεχόμενο χρήσης το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης που υποστηρίζει τη ρύθμιση που θέλετε. |
| 40868 | EX_USER | Το μέγιστο ανά βάση δεδομένων DTU δεν πρέπει να υπερβαίνει (%d) για το επίπεδο υπηρεσιών ' %. * των ls. | Max DTU ανά βάση δεδομένων. επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Προσπαθείτε να ορίσετε το μέγιστο DTU ανά βάση δεδομένων πέρα από το όριο που υποστηρίζεται. | Εξετάστε το ενδεχόμενο χρήσης το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης που υποστηρίζει τη ρύθμιση που θέλετε. |
| 40870 | EX_USER | Το ελάχιστο DTU ανά βάση δεδομένων δεν πρέπει να υπερβαίνει (%d) για το επίπεδο υπηρεσιών ' %. * των ls. | Min DTU ανά βάση δεδομένων. επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Προσπαθείτε να ορίσετε το ελάχιστο DTU ανά βάση δεδομένων πέρα από το όριο που υποστηρίζεται. | Εξετάστε το ενδεχόμενο χρήσης το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης που υποστηρίζει τη ρύθμιση που θέλετε. |
| 40873 | EX_USER | Ο αριθμός των βάσεων δεδομένων (%d) και DTU min ανά βάση δεδομένων (%d) δεν πρέπει να υπερβαίνει τα DTUs του ελαστικότητας χώρου συγκέντρωσης (%d). | Ο αριθμός βάσεις δεδομένων στην ελαστικότητας χώρου συγκέντρωσης; Min DTU ανά βάση δεδομένων. DTUs ελαστικά χώρου συγκέντρωσης. | Για να καθορίσετε DTU min για βάσεις δεδομένων στο χώρο συγκέντρωσης ελαστικότητας που υπερβαίνει το DTUs του χώρου συγκέντρωσης ελαστικότητας προσπάθεια. | Ίσως χρειάζεται να αυξήσετε το DTUs του χώρου συγκέντρωσης ελαστικά, ή μειώστε το ελάχιστο DTU ανά βάση δεδομένων ή μειώστε τον αριθμό των βάσεων δεδομένων στο χώρο συγκέντρωσης ελαστικότητας. |
| 40877 | EX_USER | Ένα χώρο συγκέντρωσης ελαστικά δεν μπορούν να διαγραφούν, εκτός εάν δεν περιέχει όλες τις βάσεις δεδομένων. | Κανένας | Το χώρο συγκέντρωσης ελαστικότητας περιέχει μία ή περισσότερες βάσεις δεδομένων και, επομένως, δεν μπορούν να διαγραφούν. | Καταργήστε βάσεις δεδομένων από το χώρο συγκέντρωσης ελαστικότητας για να το διαγράψετε. |
| 40881 | EX_USER | Ο χώρος συγκέντρωσης ελαστικότητας ' %. * των ls έχει φτάσει στο όριο count βάσης δεδομένων.  Το όριο count βάσης δεδομένων για το χώρο συγκέντρωσης ελαστικά δεν πρέπει να υπερβαίνει (%d) για το χώρο συγκέντρωσης ελαστικότητας με (%d) DTUs. | Όνομα χώρου συγκέντρωσης ελαστικότητας; όριο καταμέτρηση της βάσης δεδομένων του χώρου συγκέντρωσης ελαστικά, e DTUs για το χώρο συγκέντρωσης πόρων. | Προσπαθείτε να δημιουργήσετε ή να προσθέσετε βάσης δεδομένων στο ελαστικότητας συγκέντρωσης όταν η βάση δεδομένων καταμέτρηση του χώρου συγκέντρωσης ελαστικότητας όριο. | Επικοινωνήστε ίσως χρειάζεται να αυξήσετε το DTUs του ελαστικότητας χώρου συγκέντρωσης εάν είναι δυνατό, για να αυξήσετε το όριο της βάσης δεδομένων ή να διαγράψετε βάσεις δεδομένων από το χώρο συγκέντρωσης ελαστικότητας. |
| 40889 | EX_USER | Το DTUs ή το όριο χώρου αποθήκευσης για το χώρο συγκέντρωσης ελαστικότητας ' %. * των ls δεν είναι δυνατό να μειωθεί δεδομένου ότι που δεν παρέχουν αρκετό χώρο αποθήκευσης για τις βάσεις δεδομένων. | Το όνομα του χώρου συγκέντρωσης ελαστικότητας. | Προσπαθεί να μειώσετε το όριο χώρου αποθήκευσης του χώρου συγκέντρωσης ελαστικότητας κάτω από τη χρήση του χώρου αποθήκευσης. | Πληκτρολογήστε λάβετε υπόψη τη μείωση της χρήσης χώρου αποθήκευσης των μεμονωμένων βάσεων δεδομένων στο χώρο συγκέντρωσης ελαστικά ή καταργήσετε βάσεις δεδομένων από το χώρο συγκέντρωσης για να μειώσετε το DTUs ή το όριο χώρου αποθήκευσης. |
| 40891 | EX_USER | Το ελάχιστο DTU ανά βάση δεδομένων (%d) δεν πρέπει να υπερβαίνει το μέγιστο DTU ανά βάση δεδομένων (%d). | Min DTU ανά βάση δεδομένων. DTU max ανά βάση δεδομένων. | Προσπαθείτε να ορίσετε το ελάχιστο DTU ανά βάση δεδομένων που είναι υψηλότερα από το μέγιστο DTU ανά βάση δεδομένων. | Βεβαιωθείτε ότι το ελάχιστο DTU ανά βάσεις δεδομένων δεν υπερβαίνει το μέγιστο DTU ανά βάση δεδομένων. |
| TBD | EX_USER | Το μέγεθος του χώρου αποθήκευσης για μια μεμονωμένη βάση δεδομένων σε ένα χώρο συγκέντρωσης ελαστικά δεν πρέπει να υπερβαίνει το μέγιστο μέγεθος που επιτρέπεται από ' %. * των ls υπηρεσίας επίπεδο ελαστικότητας χώρου συγκέντρωσης. | επίπεδο υπηρεσιών ελαστικότητας χώρο συγκέντρωσης | Το μέγιστο μέγεθος για τη βάση δεδομένων υπερβαίνει το μέγιστο μέγεθος που επιτρέπεται από το επίπεδο υπηρεσιών ελαστικότητας χώρου συγκέντρωσης. | Ορίστε το μέγιστο μέγεθος της βάσης δεδομένων εντός των ορίων του το μέγιστο μέγεθος που επιτρέπεται από το επίπεδο υπηρεσιών ελαστικότητας χώρο συγκέντρωσης. |

Σχετικά θέματα:

* [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Οθόνη και να διαχειριστείτε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Γενικά σφάλματα

Τα ακόλουθα σφάλματα δεν εμπίπτουν σε οποιαδήποτε προηγούμενη κατηγορίες.

|Κωδικός σφάλματος|Σοβαρότητας|Περιγραφή|
|---:|---:|:---|
|15006|16|<AdministratorLogin>δεν είναι έγκυρο όνομα επειδή περιέχει μη έγκυρους χαρακτήρες.|
|18452|14|Απέτυχε η σύνδεση. Η σύνδεση προέρχεται από αξιόπιστη τομέα και δεν μπορεί να χρησιμοποιηθεί με το Windows authentication.%. & #x2a; ls (συνδέσεις Windows δεν υποστηρίζονται σε αυτή την έκδοση του SQL Server.)|
|18456|14|Απέτυχε η σύνδεση για το χρήστη ' %. #x2a;ls'.%. & #x2a; % ls. & #x2a; ls (login απέτυχε για το χρήστη "% & #x2a, ls". Απέτυχε η αλλαγή κωδικού πρόσβασης. Αλλαγή του κωδικού πρόσβασης κατά τη σύνδεση δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.)|
|18470|14|Απέτυχε η σύνδεση για το χρήστη '% & #x2a; ls'. Αιτία: Ο λογαριασμός είναι disabled.% & #x2a; ls|
|40014|16|Πολλές βάσεις δεδομένων δεν μπορεί να χρησιμοποιηθεί στην ίδια συναλλαγή.|
|40054|16|Πίνακες χωρίς ένα ευρετήριο συμπλέγματος δεν υποστηρίζονται σε αυτή την έκδοση του SQL Server. Δημιουργία ευρετηρίου ομαδοποιημένων και προσπαθήστε ξανά.|
|40133|15|Αυτή η λειτουργία δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40506|16|Καθορισμένο αναγνωριστικό ΑΣΦΑΛΕΊΑΣ δεν είναι έγκυρο για αυτήν την έκδοση του SQL Server.|
|40507|16|' %. & #x2a, των ls δεν είναι δυνατή με παραμέτρους σε αυτήν την έκδοση του SQL Server.|
|40508|16|ΧΡΉΣΗ δήλωση δεν υποστηρίζεται για εναλλαγή μεταξύ των βάσεων δεδομένων. Χρησιμοποιήστε μια νέα σύνδεση για να συνδεθείτε με μια διαφορετική βάση δεδομένων.|
|40510|16|Δήλωση '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server|
|40511|16|Ενσωματωμένη συνάρτηση '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40512|16|Η δυνατότητα που έχουν καταργηθεί '%ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40513|16|Διακομιστής μεταβλητής '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40514|16|'%ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40515|16|Αναφορά σε βάση δεδομένων ή/και του διακομιστή ονομάτων στο '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40516|16|Καθολικό temp αντικειμένων δεν υποστηρίζονται σε αυτή την έκδοση του SQL Server.|
|40517|16|Επιλογή λέξης-κλειδιού ή δήλωση '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40518|16|Εντολή DBCC '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40520|16|Δυνατότητα ασφάλισης κλάσης "% S_MSG" δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40521|16|Δυνατότητα ασφάλισης κλάσης "% S_MSG" δεν υποστηρίζεται στην εμβέλεια διακομιστή σε αυτήν την έκδοση του SQL Server.|
|40522|16|Τύπος κεφαλαίου '% & #x2a; ls' βάσης δεδομένων δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40523|16|Δημιουργία '% & #x2a; ls' έμμεσα χρήστη δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server. Δημιουργία ρητά ο χρήστης πριν να τη χρησιμοποιείτε.|
|40524|16|Τύπος δεδομένων '% & #x2a; ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40525|16|ΜΕ το '%.ls' δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40526|16|' %. & #x2a, των ls σύνολο γραμμών υπηρεσία παροχής που χρησιμοποιείτε δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40527|16|Συνδεδεμένοι διακομιστές δεν υποστηρίζονται σε αυτή την έκδοση του SQL Server.|
|40528|16|Οι χρήστες δεν είναι δυνατό να αντιστοιχιστεί σε πιστοποιητικά, ασύμμετρη πλήκτρα ή συνδέσεις Windows σε αυτή την έκδοση του SQL Server.|
|40529|16|Ενσωματωμένη συνάρτηση '% & #x2a; ls' σε απομίμησης περιβάλλοντος δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40532|11|Δεν μπορεί να ανοίξει διακομιστή "% & #x2a, ls" ζητήθηκε από τη σύνδεση. Απέτυχε η σύνδεση.|
|40553|16|Η περίοδος λειτουργίας τερματίστηκε λόγω υπερβολικής χρήσης μνήμης. Προσπαθήστε να τροποποιήσετε το ερώτημα για την επεξεργασία λιγότερες γραμμές.<br/><br/>*Σημείωση:* Μείωση του αριθμού των `ORDER BY` και `GROUP BY` λειτουργίες στον κώδικα Transact-SQL βοηθά στη μείωση της τις απαιτήσεις μνήμης του ερωτήματός σας.|
|40604|16|Θα μπορούσε να μην ΔΗΜΙΟΥΡΓΊΑ/πρόταση ALTER βάσης ΔΕΔΟΜΈΝΩΝ, επειδή θα υπερβεί το όριο του διακομιστή.|
|40606|16|Επισύναψη βάσεων δεδομένων δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40607|16|Συνδέσεις Windows δεν υποστηρίζονται σε αυτή την έκδοση του SQL Server.|
|40611|16|Οι διακομιστές μπορεί να έχει οριστεί πολύ 128 κανόνες τείχους προστασίας.|
|40614|16|Έναρξη της διεύθυνσης IP του κανόνα τείχους προστασίας δεν πρέπει να υπερβαίνει διεύθυνση IP λήξης.|
|40615|16|Δεν μπορεί να ανοίξει διακομιστή '{0}' ζητήθηκε από τη σύνδεση. Πρόγραμμα-πελάτη με διεύθυνση IP '{1}' δεν επιτρέπεται η πρόσβαση στο διακομιστή.  Για να ενεργοποιήσετε την πρόσβαση, χρησιμοποιήστε την πύλη βάσης δεδομένων SQL ή εκτελέστε sp_set_firewall_rule στην κύρια βάση δεδομένων για να δημιουργήσετε έναν κανόνα τείχους προστασίας για αυτήν τη διεύθυνση IP ή περιοχή διευθύνσεων.  Ενδέχεται να χρειαστούν έως και πέντε λεπτά για να τεθούν σε ισχύ αυτή η αλλαγή.|
|40617|16|Το όνομα του κανόνα τείχους προστασίας που ξεκινά με <rule name> είναι πολύ μεγάλη. Μέγιστο μήκος είναι 128.|
|40618|16|Το όνομα του κανόνα τείχος προστασίας δεν είναι δυνατό να είναι κενή.|
|40620|16|Απέτυχε η σύνδεση για το χρήστη "% & #x2a, ls". Απέτυχε η αλλαγή κωδικού πρόσβασης. Αλλαγή του κωδικού πρόσβασης κατά τη σύνδεση δεν υποστηρίζεται σε αυτήν την έκδοση του SQL Server.|
|40627|20|Λειτουργία σε διακομιστή '{0}' και βάση δεδομένων '{1}' βρίσκεται σε εξέλιξη. Περιμένετε λίγα λεπτά πριν να δοκιμάσετε ξανά.|
|40630|16|Αποτυχία επικύρωσης τον κωδικό πρόσβασης. Ο κωδικός πρόσβασης δεν πληροί απαιτήσεις πολιτικής επειδή είναι πολύ μικρό.|
|40631|16|Ο κωδικός πρόσβασης που καθορίσατε είναι πολύ μεγάλη. Ο κωδικός πρόσβασης θα πρέπει να έχετε λιγότερα από 128 χαρακτήρες.|
|40632|16|Αποτυχία επικύρωσης τον κωδικό πρόσβασης. Ο κωδικός πρόσβασης δεν πληροί απαιτήσεις πολιτικής επειδή δεν είναι αρκετά σύνθετη.|
|40636|16|Δεν μπορείτε να χρησιμοποιήσετε ένα όνομα βάσης δεδομένων δεσμευμένη '% & #x2a; ls' σε αυτήν τη λειτουργία.|
|40638|16|Αναγνωριστικό συνδρομής δεν είναι έγκυρη < αναγνωριστικό συνδρομής >. Δεν υπάρχει συνδρομής.|
|40639|16|Αίτηση συμμορφώνεται με σχήμα: <schema error>.|
|40640|20|Ο διακομιστής αντιμετώπισε μη αναμενόμενη εξαίρεση.|
|40641|16|Στη θέση που καθορίζεται δεν είναι έγκυρη.|
|40642|17|Ο διακομιστής αυτήν τη στιγμή είναι πολύ απασχολημένοι. Δοκιμάστε ξανά αργότερα.|
|40643|16|Η τιμή καθορισμένο κεφαλίδας ms-έκδοση x δεν είναι έγκυρη.|
|40644|14|Απέτυχε η επιτρέπουν την πρόσβαση σε καθορισμένη εγγραφή.|
|40645|16|Όνομα_διακομιστή <servername> δεν μπορεί να είναι κενό ή null. Το μπορούν να γίνουν μόνο προς τα επάνω από πεζούς χαρακτήρες 'a'-'z', τους αριθμούς 0-9 και της παύλας. Το ενωτικό δεν υποψήφιου πελάτη ή να ίχνη στο όνομά της.|
|40646|16|Αναγνωριστικό συνδρομής δεν μπορεί να είναι κενή.|
|40647|16|Συνδρομή < αναγνωριστικό συνδρομής δεν διαθέτει όνομα_διακομιστή διακομιστή.|
|40648|17|Πάρα πολλές αιτήσεις έχουν εκτελεστεί. Προσπαθήστε ξανά αργότερα.|
|40649|16|Μη έγκυροι τύπου περιεχομένου έχει καθοριστεί. Υποστηρίζεται μόνο εφαρμογή/xml.|
|40650|16|Συνδρομή < αναγνωριστικό συνδρομής > δεν υπάρχει ή δεν είναι έτοιμη για τη λειτουργία.|
|40651|16|Απέτυχε η δημιουργία διακομιστή, επειδή η συνδρομή < αναγνωριστικό συνδρομής > είναι απενεργοποιημένη.|
|40652|16|Δεν είναι δυνατό να μετακινήσετε ή να δημιουργήσετε ένα διακομιστή. Συνδρομή < αναγνωριστικό συνδρομής > θα υπέρβαση ορίου του διακομιστή.|
|40671|17|Αποτυχία επικοινωνίας μεταξύ της πύλης και τη διαχείριση της υπηρεσίας. Προσπαθήστε ξανά αργότερα.|
|40852|16|Δεν είναι δυνατό να Άνοιγμα βάσης δεδομένων ' %. *ls' στο διακομιστή ' %.* των ls ζητήθηκε από τη σύνδεση. Πρόσβαση στη βάση δεδομένων είναι μόνο για επιτρεπόμενους χρησιμοποιώντας μια συμβολοσειρά σύνδεσης με ενεργοποιημένη ασφάλεια. Για να αποκτήσετε πρόσβαση σε αυτήν τη βάση δεδομένων, τροποποιήστε τις συμβολοσειρές σύνδεσης ώστε να περιέχει 'ασφαλούς' στο διακομιστή FQDN -.database.windows 'όνομα διακομιστή'.net θα πρέπει να τροποποιηθούν ώστε να .database "όνομα διακομιστή". `secure`. των windows.net.|
|45168|16|Το σύστημα SQL Azure είναι φόρτος και τοποθετείται ένα ανώτερο όριο σε ταυτόχρονες λειτουργίες DB CRUD για ένα διακομιστή (π.χ., δημιουργία βάσης δεδομένων). Ο διακομιστής που καθορίζεται στο μήνυμα σφάλματος έχει υπερβεί τον μέγιστο αριθμό ταυτόχρονες συνδέσεις. Δοκιμάστε ξανά αργότερα.|
|45169|16|Το σύστημα SQL azure είναι φόρτος και τοποθετώντας ένα ανώτερο όριο στον αριθμό των λειτουργίες CRUD ταυτόχρονες διακομιστή για μια μεμονωμένη συνδρομή (π.χ., δημιουργία server). Η εγγραφή που καθορίζεται στο μήνυμα σφάλματος έχει υπερβεί τον μέγιστο αριθμό ταυτόχρονες συνδέσεις και η αίτηση απορρίφθηκε. Δοκιμάστε ξανά αργότερα.|

## <a name="related-links"></a>Σχετικές συνδέσεις

- [Περιορισμοί γενικά βάσης δεδομένων Azure SQL και κατευθυντήριες γραμμές](sql-database-general-limitations.md)
- [Όρια πόρων Azure βάσης δεδομένων SQL](sql-database-resource-limits.md)
<properties
    pageTitle="Αντιμετώπιση κοινών ζητημάτων σύνδεσης με βάση δεδομένων SQL Azure"
    description="Βήματα για να προσδιορισμός και επίλυση συνηθισμένων σφαλμάτων σύνδεσης για βάση δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Αντιμετώπιση προβλημάτων σύνδεσης με βάση δεδομένων SQL Azure

Όταν αποτύχει η σύνδεση με βάση δεδομένων SQL Azure, λαμβάνετε [μηνύματα σφάλματος](sql-database-develop-error-messages.md). Σε αυτό το άρθρο είναι μια κεντρική θέμα που σας βοηθά να αντιμετώπιση προβλημάτων συνδεσιμότητας βάση δεδομένων SQL Azure. Θα παρουσιάζει [έχει ως αποτέλεσμα το κοινό](#cause) ζητήματα σύνδεσης, συνιστά [ένα εργαλείο αντιμετώπισης προβλημάτων](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) που σας βοηθά ταυτότητας το πρόβλημα και παρέχει βήματα αντιμετώπισης προβλημάτων για την επίλυση [σφαλμάτων μεταβατικές](#troubleshoot-transient-errors) και [μόνιμη ή μη-μεταβατικές σφάλματα](#troubleshoot-the-persistent-errors). Τέλος, παραθέτει [όλα τα σχετικά άρθρα για ζητήματα συνδεσιμότητας βάση δεδομένων SQL Azure](#all-topics-for-azure-sql-database-connection-problems).

Εάν αντιμετωπίσετε τα προβλήματα σύνδεσης, δοκιμάστε τα βήματα αντιμετώπιση προβλημάτων που περιγράφονται σε αυτό το άρθρο.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Αιτία

Προβλήματα σύνδεσης μπορεί να οφείλεται σε οποιοδήποτε από τα εξής:

- Αποτυχία για να εφαρμόσετε βέλτιστες πρακτικές και οδηγίες σχεδίασης κατά τη διαδικασία σχεδίασης της εφαρμογής.  Ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md) για να ξεκινήσετε.
- Αλλαγή των ρυθμίσεων Azure SQL βάσης δεδομένων παραμέτρων
- Ρυθμίσεις του τείχους προστασίας
- Λήξη χρονικού ορίου σύνδεσης
- Πληροφορίες σύνδεσης λάθος
- Μέγιστο όριο φτάσει σε ορισμένους πόρους βάση δεδομένων SQL Azure

Γενικά, ζητήματα σύνδεσης με βάση δεδομένων SQL Azure ταξινομούνται ως εξής:

- [Μεταβατικές σφάλματα (μικρής διάρκειας ή περιστασιακό)](#troubleshoot-transient-errors)
- [Μόνιμη ή μη-μεταβατικές σφάλματα (σφάλματα που επαναλαμβάνεται τακτικά)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Δοκιμάστε την αντιμετώπιση προβλημάτων για ζητήματα συνδεσιμότητας βάση δεδομένων SQL Azure

Εάν αντιμετωπίσετε ένα σφάλμα τη συγκεκριμένη σύνδεση, δοκιμάστε [αυτό το εργαλείο](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database), το οποίο θα σας βοηθήσει να γρήγορα ταυτότητας και επιλύσει το πρόβλημά σας.

## <a name="troubleshoot-transient-errors"></a>Αντιμετώπιση σφαλμάτων μεταβατικές
Εάν η εφαρμογή σας αντιμετωπίζει μεταβατικές σφάλματα, εξετάστε τα παρακάτω θέματα για συμβουλές σχετικά με τον τρόπο αντιμετώπισης και να μειώσετε τη συχνότητα αυτών των σφαλμάτων:

- [Αντιμετώπιση προβλημάτων βάσης δεδομένων &lt;x&gt; στο διακομιστή &lt;y&gt; δεν είναι διαθέσιμη (σφάλμα: 40613)](sql-database-troubleshoot-connection.md)
- [Αντιμετώπιση προβλημάτων, διάγνωση και αποτροπή σφαλμάτων σύνδεσης SQL και μεταβατικές σφαλμάτων για βάση δεδομένων SQL](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Αντιμετώπιση προβλημάτων μόνιμη σφάλματα (σφάλματα μη μεταβατική)

Εάν η εφαρμογή αποτύχει μόνιμα για σύνδεση με βάση δεδομένων SQL Azure, αυτό συνήθως δείχνει ότι υπάρχει κάποιο πρόβλημα με ένα από τα εξής:

- Ρύθμιση παραμέτρων του τείχους προστασίας. Το τείχος προστασίας βάση δεδομένων ή πλευρά του προγράμματος-πελάτη Azure SQL αποκλείει συνδέσεις με βάση δεδομένων SQL Azure.
- Αλλαγή των ρυθμίσεων παραμέτρων στην πλευρά του προγράμματος-πελάτη δικτύου: για παράδειγμα, μια νέα διεύθυνση IP ή διακομιστή μεσολάβησης.
- Σφάλμα χρήστη: για παράδειγμα, λάθος παραμέτρους σύνδεσης, όπως το όνομα του διακομιστή στη συμβολοσειρά σύνδεσης.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Βήματα για να επιλύσετε προβλήματα συνδεσιμότητας μόνιμη

1.  Ρύθμιση του [κανόνες τείχους προστασίας](sql-database-configure-firewall-settings.md) για να επιτρέψετε στη διεύθυνση IP του προγράμματος-πελάτη.
2.  Σε όλα τα τείχη προστασίας μεταξύ του προγράμματος-πελάτη και του Internet, βεβαιωθείτε ότι είναι ανοιχτό για εξερχόμενες συνδέσεις θύρα 1433. Διαβάστε τη [Ρύθμιση παραμέτρων του τείχους προστασίας των Windows για να επιτρέπεται η πρόσβαση SQL Server](https://msdn.microsoft.com/library/cc646023.aspx) για επιπλέον πληροφορίες.
3.  Επαληθεύστε τη συμβολοσειρά σύνδεσης και άλλες ρυθμίσεις σύνδεσης. Ανατρέξτε στην ενότητα συμβολοσειρά σύνδεσης στο [θέμα ζητήματα συνδεσιμότητας](sql-database-connectivity-issues.md#connections-to-azure-sql-database).
4.  Έλεγχος εύρυθμης λειτουργίας υπηρεσιών στον πίνακα εργαλείων. Εάν πιστεύετε ότι υπάρχει ένα τοπικές μη διαθεσιμότητα, ανατρέξτε στο θέμα [ανάκτηση από μια μη διαθεσιμότητα](sql-database-disaster-recovery.md) για τα βήματα για να ανακτήσετε μια νέα περιοχή.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Όλα τα θέματα για προβλήματα σύνδεσης βάσης δεδομένων SQL Azure

Ο παρακάτω πίνακας παραθέτει κάθε θέμα πρόβλημα σύνδεσης που ισχύει απευθείας με την υπηρεσία βάσης δεδομένων SQL Azure.


| &nbsp; | Τίτλος | Περιγραφή |
| --: | :-- | :-- |
| 1 | [Αντιμετώπιση προβλημάτων σύνδεσης με βάση δεδομένων SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Αυτή είναι η σελίδα προορισμού για την αντιμετώπιση προβλημάτων συνδεσιμότητας σε βάση δεδομένων SQL Azure. Περιγράφει τον τρόπο προσδιορισμός και επίλυση μεταβατικές σφάλματα και μόνιμη ή μη-μεταβατικές σφάλματα στη βάση δεδομένων SQL Azure. |
| 2 | [Αντιμετώπιση προβλημάτων, διάγνωση και αποτροπή σφαλμάτων σύνδεσης SQL και μεταβατικές σφαλμάτων για βάση δεδομένων SQL](sql-database-connectivity-issues.md) | Μάθετε πώς μπορείτε να αντιμετωπίσετε, διάγνωση και να αποτρέψετε ένα σφάλμα σύνδεσης SQL ή μεταβατικές σφάλμα στη βάση δεδομένων SQL Azure. |
| 3 | [Γενικές οδηγίες για μεταβατικές χειρισμού σφαλμάτων](best-practices-retry-general.md) | Παρέχει γενικές οδηγίες για το χειρισμό μεταβατικές σφάλμα κατά τη σύνδεση με βάση δεδομένων SQL Azure. |
| 4 | [Αντιμετώπιση προβλημάτων συνδεσιμότητας με βάση δεδομένων SQL Microsoft Azure](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) | Αυτό το εργαλείο βοηθά την ταυτότητά σας πρόβλημα επίλυση σφαλμάτων σύνδεσης. |
| 5 | [Αντιμετώπιση προβλημάτων "βάση δεδομένων &lt;x&gt; στο διακομιστή &lt;y&gt; δεν είναι διαθέσιμη αυτήν τη στιγμή. Σφάλμα προσπαθήστε ξανά αργότερα τη σύνδεση"](sql-database-troubleshoot-connection.md) | Περιγράφει τον τρόπο προσδιορισμός και επίλυση ενός σφάλματος 40613: "βάση δεδομένων &lt;x&gt; στο διακομιστή &lt;y&gt; δεν είναι διαθέσιμη αυτήν τη στιγμή. Προσπαθήστε ξανά αργότερα τη σύνδεση." |
| 6 | [Κωδικοί σφάλματος SQL για εφαρμογές προγράμματος-πελάτη της βάσης δεδομένων SQL: σφάλμα σύνδεσης και άλλα θέματα της βάσης δεδομένων](sql-database-develop-error-messages.md) | Παρέχει πληροφορίες σχετικά με τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL, όπως συνηθισμένων σφαλμάτων σύνδεσης βάσης δεδομένων, ζητήματα αντίγραφο της βάσης δεδομένων και γενικά σφάλματα. |
| 7 | [Azure καθοδήγηση επιδόσεων βάσης δεδομένων SQL για μία μόνο βάσεις δεδομένων](sql-database-performance-guidance.md) | Παρέχει οδηγίες για να καθορίσετε ποια επίπεδο υπηρεσιών είναι κατάλληλη για την εφαρμογή σας. Επίσης, παρέχει συστάσεις για την εφαρμογή σας για να αξιοποιήσετε στο έπακρο τη βάση δεδομένων SQL Azure της ρύθμισης. |
| 8 | [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md) | Παρέχει συνδέσεις σε δείγματα κώδικα για διάφορες τεχνολογίες που μπορείτε να χρησιμοποιήσετε για να συνδεθείτε και να αλληλεπιδράσετε με βάση δεδομένων SQL Azure. |
| 9 | Αναβάθμιση σε βάση δεδομένων SQL Azure v12 σελίδα ([Azure πύλη](sql-database-upgrade-server-portal.md), [PowerShell](sql-database-upgrade-server-powershell.md)) | Παρέχει οδηγίες για την αναβάθμιση υπάρχοντος διακομιστές V11: βάση δεδομένων SQL Azure και των βάσεων δεδομένων σε V12 βάση δεδομένων SQL Azure χρησιμοποιώντας την πύλη του Azure ή PowerShell. |


## <a name="next-steps"></a>Επόμενα βήματα

- [Αντιμετώπιση προβλημάτων επιδόσεων βάση δεδομένων SQL Azure](sql-database-troubleshoot-performance.md)
- [Αντιμετώπιση προβλημάτων δικαιώματα βάση δεδομένων SQL Azure](sql-database-troubleshoot-permissions.md)
- [Δείτε όλα τα θέματα για την υπηρεσία βάσης δεδομένων SQL Azure](sql-database-index-all-articles.md)
- [Αναζήτηση στην τεκμηρίωση των Windows Azure](http://azure.microsoft.com/search/documentation/)
- [Προβολή των πιο πρόσφατων ενημερώσεων για την υπηρεσία βάσης δεδομένων SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
- [Γενικές οδηγίες για μεταβατικές χειρισμού σφαλμάτων](../best-practices-retry-general.md)
- [Βιβλιοθήκες σύνδεσης για τη βάση δεδομένων SQL και του SQL Server](sql-database-libraries.md)
- [Η διαδρομή εκμάθησης για τη χρήση βάσης δεδομένων SQL Azure](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database)
- [Η διαδρομή εκμάθησης για τη χρήση δυνατοτήτων ελαστικότητας βάσης δεδομένων και εργαλεία](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale) 

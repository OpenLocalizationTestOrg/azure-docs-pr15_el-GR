<properties
    pageTitle="Δημιουργία και διαχείριση υπό κλίμακα ανάληψη βάσεις δεδομένων SQL Azure χρησιμοποιώντας ελαστικότητας εργασίες | Micosoft Azure"
    description="Θα καθοδηγήσουν για τη δημιουργία και τη διαχείριση μιας εργασίας ελαστικότητας βάσης δεδομένων."
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Δημιουργία και διαχείριση υπό κλίμακα ανάληψη βάσεις δεδομένων SQL Azure χρησιμοποιώντας ελαστικότητας εργασίες (έκδοση preview)

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)


**Εργασίες ελαστικότητας βάσης δεδομένων** απλοποίηση της διαχείρισης των ομάδων των βάσεων δεδομένων κατά την εκτέλεση λειτουργιών διαχείρισης όπως αλλαγές σχήματος, Διαχείριση διαπιστευτηρίων, ενημερώσεις δεδομένων αναφοράς, η συλλογή δεδομένων επιδόσεων ή τη συλλογή τηλεμετρίας μισθωτή (πελάτη). Ελαστικά εργασίες βάσης δεδομένων είναι αυτήν τη στιγμή διαθέσιμα μέσω του Azure πύλη και τα cmdlet του PowerShell. Ωστόσο, το Azure πύλης επιφανειών περιορισμένες λειτουργίες περιορίζεται σε εκτέλεσης σε όλες τις βάσεις δεδομένων σε ένα [χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων (έκδοση preview)](sql-database-elastic-pool.md). Για να αποκτήσετε πρόσβαση σε πρόσθετες δυνατότητες και την εκτέλεση των δεσμών ενεργειών σε μια ομάδα βάσεων δεδομένων, όπως μια συλλογή που ορίζονται από το προσαρμοσμένο ή ένα σύνολο shard (που δημιουργήθηκε χρησιμοποιώντας [βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-scale-introduction.md)), ανατρέξτε στο θέμα [Δημιουργία και διαχείριση εργασιών με χρήση του PowerShell](sql-database-elastic-jobs-powershell.md). Για περισσότερες πληροφορίες σχετικά με τις εργασίες, ανατρέξτε στο θέμα [Επισκόπηση εργασιών ελαστικότητας βάσης δεδομένων](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Μια συνδρομή του Azure. Για μια δωρεάν δοκιμαστική έκδοση, ανατρέξτε στο θέμα [δωρεάν δοκιμαστική έκδοση ενός μήνα](https://azure.microsoft.com/pricing/free-trial/).
* Ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων. Ανατρέξτε στο θέμα [σχετικά με ελαστικά χώρους συγκέντρωσης βάσης δεδομένων](sql-database-elastic-pool.md)
* Εγκατάσταση των στοιχείων υπηρεσίας εργασία ελαστικότητας βάσης δεδομένων. Ανατρέξτε στο θέμα [κατά την εγκατάσταση της υπηρεσίας εργασία ελαστικότητας βάσης δεδομένων](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Δημιουργία εργασιών

1. Με την [πύλη του Azure](https://portal.azure.com), από ένα υπάρχον σύνολο εργασίας ελαστικότητας βάσης δεδομένων, κάντε κλικ στην επιλογή **Δημιουργία εργασίας**.
2. Πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης από το διαχειριστή της βάσης δεδομένων (που δημιουργήθηκε κατά την εγκατάσταση των εργασιών) για τη βάση δεδομένων ελέγχου εργασιών (χώρος αποθήκευσης μετα-δεδομένων για τις εργασίες).

    ![Όνομα της εργασίας, πληκτρολογήστε ή επικολλήστε κώδικα και κάντε κλικ στην επιλογή Εκτέλεση][1]
2. Στο το blade **Δημιουργία εργασίας** , πληκτρολογήστε ένα όνομα για το έργο.
3. Πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης για να συνδεθείτε με τις βάσεις δεδομένων προορισμού με επαρκή δικαιώματα για την εκτέλεση της δέσμης ενεργειών με επιτυχία.
4. Επικολλήστε ή πληκτρολογήστε στη δέσμη ενεργειών T-SQL.
5. Κάντε κλικ στην επιλογή **Αποθήκευση** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εκτέλεση**.

    ![Δημιουργία εργασιών και εκτέλεση][5]

## <a name="run-idempotent-jobs"></a>Εκτέλεση idempotent εργασιών

Όταν εκτελείτε μια δέσμη ενεργειών σε ένα σύνολο των βάσεων δεδομένων, πρέπει να βεβαιωθείτε ότι η δέσμη ενεργειών είναι idempotent. Αυτό σημαίνει ότι τη δέσμη ενεργειών πρέπει να μπορούν να εκτελούν πολλές φορές, ακόμα και αν απέτυχε πριν σε κατάσταση μη ολοκλήρωσης. Για παράδειγμα, όταν αποτύχει μια δέσμη ενεργειών, η εργασία θα αυτόματα επαναληφθεί μέχρι να επιτύχει (εντός ορίων, ως η "Επανάληψη" λογική θα εμφανίσει διακόψει την επανάληψη). Ο τρόπος να το κάνετε αυτό είναι να χρησιμοποιήσετε το ενός όρου "ΕΆΝ ΥΠΆΡΧΕΙ" και να διαγράψετε οποιοδήποτε βρέθηκαν παρουσία πριν από τη δημιουργία ενός νέου αντικειμένου. Παράδειγμα εμφανίζεται εδώ:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε έναν όρο "ΕΆΝ δεν ΥΠΆΡΧΕΙ" πριν από τη δημιουργία μιας νέας περιόδου λειτουργίας:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Αυτή η δέσμη ενεργειών ενημερώνει, στη συνέχεια, τον πίνακα που δημιουργήσατε προηγουμένως.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Έλεγχος κατάστασης εργασίας

Αφού έχει ξεκινήσει μια εργασία, μπορείτε να ελέγξετε για την πρόοδο του έργου.

1. Από τη σελίδα χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων, κάντε κλικ στην επιλογή **Διαχείριση εργασιών**.

    ![Κάντε κλικ στην επιλογή "Διαχείριση εργασιών"][2]

2. Κάντε κλικ στο όνομα του (α) μιας εργασίας. Η **ΚΑΤΆΣΤΑΣΗ** μπορεί να είναι "Ολοκληρώθηκε" ή "Αποτυχία". Λεπτομέρειες της εργασίας εμφανίζονται (b) με την ημερομηνία και την ώρα δημιουργίας και χρήσης του. Η λίστα (c) κάτω από το οποίο εμφανίζει την πρόοδο της δέσμης ενεργειών σε μια βάση δεδομένων κάθε στο χώρο συγκέντρωσης, παρέχοντας τις λεπτομέρειές ημερομηνίας και ώρας.

    ![Έλεγχος μια ολοκληρωμένη εργασία][3]


## <a name="checking-failed-jobs"></a>Ο έλεγχος απέτυχε εργασίες

Εάν μια εργασία αποτύχει, μπορεί να βρεθεί ένα αρχείο καταγραφής για την εκτέλεση. Κάντε κλικ στο όνομα της αποτυχίας εργασίας για να δείτε τις λεπτομέρειές του.

![Έλεγχος αποτυχίας εργασίας][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 

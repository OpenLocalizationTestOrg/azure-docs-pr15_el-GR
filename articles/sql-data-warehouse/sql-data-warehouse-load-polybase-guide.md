<properties
   pageTitle="Οδηγός για τη χρήση PolyBase στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Γενικές οδηγίες και συστάσεις για τη χρήση PolyBase σε σενάρια αποθήκη δεδομένων του SQL."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Οδηγός για τη χρήση PolyBase στο αποθήκη δεδομένων του SQL

Αυτός ο οδηγός παρέχει πρακτικές πληροφορίες για τη χρήση PolyBase στο αποθήκη δεδομένων του SQL.

Για να ξεκινήσετε, ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [Φόρτωση δεδομένων με PolyBase][] .


## <a name="rotating-storage-keys"></a>Περιστροφή πλήκτρα χώρου αποθήκευσης

Κατά καιρούς που θα θέλετε να αλλάξετε τον αριθμό-κλειδί πρόσβασης με το χώρο αποθήκευσης αντικειμένων blob σας για λόγους ασφαλείας.

Ο πιο κομψό τρόπος για να εκτελέσετε αυτήν την εργασία είναι να ακολουθήστε μια διαδικασία γνωστή ως "Περιστροφή τα πλήκτρα". Ίσως έχετε παρατηρήσει ότι έχετε δύο κλειδιά χώρου αποθήκευσης για το λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob. Αυτό είναι έτσι, ώστε να είναι δυνατή η μετάβαση

Περιστροφή των αριθμών-κλειδιών λογαριασμός Azure χώρου αποθήκευσης είναι μια διαδικασία απλό τριών βημάτων

1. Δημιουργία δεύτερο διαπιστευτηρίων εύρος βάσης δεδομένων με βάση το χώρο αποθήκευσης στο δευτερεύον κλειδί πρόσβασης
2. Δημιουργία δεύτερο εξωτερικό αρχείο προέλευσης δεδομένων που βασίζεται σε αυτό νέα πιστοποίηση
3. Απόθεση και δημιουργήστε την εξωτερική πινάκων που δείχνει προς το νέο εξωτερικό αρχείο προέλευσης δεδομένων

Όταν ολοκληρώσετε τη μετεγκατάσταση όλες τις εξωτερικές πίνακες για το νέο εξωτερικό αρχείο προέλευσης δεδομένων, στη συνέχεια, μπορείτε να εκτελέσετε την καθαρή εργασίες:

1. Απόθεση πρώτο εξωτερικό αρχείο προέλευσης δεδομένων
2. Απόθεση πρώτης βάσης δεδομένων εύρος με βάση το κλειδί πρόσβασης πρωτεύοντα χώρο αποθήκευσης διαπιστευτηρίων
3. Συνδεθείτε στο Azure και να δημιουργήσετε ξανά το πλήκτρο πρωτεύοντος πρόσβασης είστε έτοιμοι για την επόμενη φορά

## <a name="query-azure-blob-storage-data"></a>Δεδομένα χώρο αποθήκευσης αντικειμένων blob του Azure ερωτήματος
Ερωτήματα εξωτερικοί πίνακες Χρησιμοποιήστε απλώς το όνομα του πίνακα, σαν να ήταν ένας σχεσιακός πίνακας.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Ένα ερώτημα σε έναν εξωτερικό πίνακα μπορεί να αποτύχει με το σφάλμα *"Ερώτημα ματαιώθηκε--το μέγιστο όριο απόρριψη όριο συμπληρώθηκε κατά την ανάγνωση από μια εξωτερική προέλευση"*. Αυτό υποδεικνύει ότι εξωτερικών δεδομένων περιέχει *βρώμικος* εγγραφές. Μια εγγραφή δεδομένων θεωρείται 'βρώμικος' Εάν πραγματικά δεδομένα τύποι/αριθμός στηλών δεν ταιριάζουν με τους ορισμούς των στηλών του εξωτερικού πίνακα ή εάν τα δεδομένα δεν συμφωνούν με την καθορισμένη εξωτερικών μορφή αρχείου. Για να διορθώσετε αυτό το πρόβλημα, βεβαιωθείτε ότι το εξωτερικό πίνακα και των ορισμών μορφή εξωτερικό αρχείο είναι σωστά και εξωτερικών δεδομένων συμμορφώνεται με αυτούς τους ορισμούς. Σε περίπτωση που ένα υποσύνολο εγγραφών εξωτερικά δεδομένα είναι βρώμικος, μπορείτε να επιλέξετε για να απορρίψετε αυτές τις εγγραφές για τα ερωτήματά σας, χρησιμοποιώντας τις επιλογές απόρριψη στη ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΏΝ DDL ΠΊΝΑΚΑ.


## <a name="load-data-from-azure-blob-storage"></a>Φόρτωση δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure
Αυτό το παράδειγμα φορτώνει δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure στη βάση δεδομένων SQL αποθήκη δεδομένων.

Αποθήκευση δεδομένων απευθείας καταργεί την ώρα μεταφορά δεδομένων για ερωτήματα. Την αποθήκευση των δεδομένων με ευρετήριο columnstore βελτιώνει τις επιδόσεις ερωτημάτων για ερωτήματα ανάλυσης με έως και 10 x.

Αυτό το παράδειγμα χρησιμοποιεί τη δήλωση ΔΗΜΙΟΥΡΓΊΑ ΕΠΙΛΈΞΤΕ ΩΣ ΠΊΝΑΚΑ για τη φόρτωση των δεδομένων. Το νέο πίνακα μεταβιβάζονται τις στήλες με το όνομα του ερωτήματος. Το μεταβιβάζονται τους τύπους δεδομένων από αυτές τις στήλες από τον ορισμό του εξωτερικού πίνακα.

ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΈΞΤΕ είναι μια ιδιαίτερα performant πρόταση Transact-SQL που φορτώνει τα δεδομένα παράλληλα για όλους τους κόμβους υπολογισμού του σας αποθήκη δεδομένων του SQL.  Το αναπτύχθηκε αρχικά για το μηχανισμό μαζικά παράλληλες επεξεργασίας (MPP) στο σύστημα αναλύσεων πλατφόρμα και βρίσκεται τώρα στο αποθήκη δεδομένων του SQL.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Ανατρέξτε στο θέμα [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Δημιουργήσετε στατιστικά στοιχεία που μόλις έχει φορτωθεί δεδομένων

Αποθήκη δεδομένων του SQL Azure κάνει δεν έχουν ακόμα υποστήριξη αυτόματη δημιουργία ή την αυτόματη ενημέρωση στατιστικών.  Για να λάβετε τις καλύτερες επιδόσεις από τα ερωτήματά σας, είναι σημαντικό στατιστικά στοιχεία να δημιουργηθούν σε όλες τις στήλες όλων των πινάκων μετά την πρώτη φόρτωσης ή προκύπτουν σημαντικές αλλαγές στα δεδομένα.  Για μια αναλυτική εξήγηση των στατιστικών στοιχείων, ανατρέξτε στο θέμα [Στατιστικά στοιχεία][] στην ομάδα ανάπτυξη των θεμάτων.  Ακολουθεί ένα γρήγορο παράδειγμα του τρόπου δημιουργίας στατιστικά στοιχεία σχετικά με το υποβάλλεται φόρτωση σε αυτό το παράδειγμα.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Εξαγωγή δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure
Αυτή η ενότητα δείχνει πώς μπορείτε να εξαγάγετε δεδομένα από αποθήκη δεδομένων του SQL στο χώρο αποθήκευσης αντικειμένων blob του Azure. Αυτό το παράδειγμα χρησιμοποιεί τη ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΏΝ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΈΞΤΕ που είναι μια ιδιαίτερα performant πρόταση Transact-SQL για να εξαγάγετε τα δεδομένα παράλληλα από όλους τους κόμβους υπολογισμού.

Το παρακάτω παράδειγμα δημιουργεί έναν εξωτερικό πίνακα Weblogs2014 χρήση ορισμούς στηλών και των δεδομένων από dbo. Πίνακας ιστολόγια. Τον ορισμό του εξωτερικού πίνακα είναι αποθηκευμένο σε αποθήκη δεδομένων του SQL και τα αποτελέσματα της πρότασης SELECT εξάγονται στον κατάλογο "/ Αρχειοθέτηση/log2014 /" κάτω από το κοντέινερ αντικειμένων blob που καθορίζεται από το αρχείο προέλευσης δεδομένων. Εξαγωγή των δεδομένων σε μορφή αρχείου καθορισμένο κείμενο.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Εργασία γύρω από την απαίτηση PolyBase UTF-8
Στην παρουσίαση PolyBase υποστηρίζει τη φόρτωση αρχείων δεδομένων που έχουν κωδικοποίηση UTF-8. Όπως UTF-8 χρησιμοποιεί την ίδια κωδικοποίηση χαρακτήρων ως ASCII PolyBase θα επίσης υποστήριξη φόρτωσης δεδομένα που είναι ASCII κωδικοποιημένη. Ωστόσο, PolyBase δεν υποστηρίζει άλλες κωδικοποίηση χαρακτήρων όπως UTF-16 / Unicode ή επεκταθεί χαρακτήρων ASCII. Εκτεταμένη ASCII περιλαμβάνει τους χαρακτήρες με τόνους όπως που είναι κοινά γερμανικά διαλυτικά.

Για να επιλύσετε αυτήν την απαίτηση την καλύτερη απάντηση είναι να κάνετε εκ νέου εγγραφή σε κωδικοποίηση UTF-8.

Υπάρχουν πολλοί τρόποι για να το κάνετε αυτό. Ακολουθούν δύο προσεγγίσεις με χρήση του Powershell:

### <a name="simple-example-for-small-files"></a>Απλό παράδειγμα για μικρές αρχεία

Ακολουθεί μια απλή μία γραμμή δέσμη ενεργειών του Powershell που δημιουργεί το αρχείο.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Ωστόσο, ενώ αυτό είναι ένα απλό τρόπο για την εκ νέου κωδικοποίηση των δεδομένων με means δεν είναι η πιο αποτελεσματική. Είσοδος/έξοδος ροής παρακάτω παράδειγμα ιδιαίτερα, είναι πολύ πιο γρήγορα και επιτυγχάνει το ίδιο αποτέλεσμα.

### <a name="io-streaming-example-for-larger-files"></a>Παράδειγμα ροής εισόδου/ΕΞΌΔΟΥ για μεγαλύτερα αρχεία

Το παρακάτω δείγμα κώδικα είναι πιο σύνθετη, αλλά, όπως το ροές τις γραμμές δεδομένων από αρχείο προέλευσης για να το ενεργοποιήσετε είναι πολύ πιο αποτελεσματικές. Χρησιμοποιήστε αυτήν την προσέγγιση για μεγαλύτερα αρχεία.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με τη μετακίνηση δεδομένων σε αποθήκη δεδομένων του SQL, ανατρέξτε στο θέμα η [Επισκόπηση μετεγκατάστασης δεδομένων][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Φόρτωση των δεδομένων με PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[Επισκόπηση μετεγκατάστασης δεδομένων]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΜΟΡΦΉ ΕΞΩΤΕΡΙΚΌ ΑΡΧΕΊΟ (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [ΕΞΩΤΕΡΙΚΌΣ ΠΊΝΑΚΑΣ ΔΗΜΙΟΥΡΓΊΑ (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->

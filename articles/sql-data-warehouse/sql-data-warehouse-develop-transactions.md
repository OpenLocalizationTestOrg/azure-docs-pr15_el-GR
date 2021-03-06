<properties
   pageTitle="Συναλλαγών σε αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για την υλοποίηση συναλλαγές σε αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Συναλλαγών σε αποθήκη δεδομένων του SQL

Όπως θα περιμένατε, αποθήκη δεδομένων του SQL υποστηρίζει συναλλαγές ως μέρος του φόρτου εργασίας αποθήκη δεδομένων. Ωστόσο, για να βεβαιωθείτε ότι διατηρούνται οι επιδόσεις του αποθήκη δεδομένων του SQL σε κλίμακα ορισμένες δυνατότητες περιορίζονται σε σύγκριση με τον SQL Server. Σε αυτό το άρθρο επισημαίνει τις διαφορές και παραθέτει τα άλλα άτομα. 

## <a name="transaction-isolation-levels"></a>Επίπεδα απομόνωσης συναλλαγής
Αποθήκη δεδομένων του SQL υλοποιεί ΟΞΈΟΣ συναλλαγές. Ωστόσο, απομόνωση των συναλλαγών υποστήριξης της περιορίζεται στα `READ UNCOMMITTED` και αυτό δεν μπορεί να αλλάξει. Μπορείτε να εφαρμόσετε έναν αριθμό κωδικοποίησης μεθόδους για να αποτρέψετε βρώμικος διαβάζει δεδομένα, εάν πρόκειται για ένα πρόβλημα για εσάς. Τα πιο δημοφιλή μεθόδους αξιοποιήσετε CTAS και πίνακα partition μετάβασης (συχνά γνωστό ως την προεπιλεγμένη ρύθμιση μοτίβο παραθύρου) για να αποτρέψετε την υποβολή ερωτημάτων δεδομένων που εξακολουθεί να είναι έχει προετοιμαστεί χρηστών. Οι προβολές που προ-φιλτράρισμα των δεδομένων είναι επίσης μια δημοφιλή προσέγγιση.  

## <a name="transaction-size"></a>Μέγεθος συναλλαγής
Μέγεθος είναι περιορισμένο συναλλαγή την τροποποίηση δεδομένων μονής ακρίβειας. Το όριο σήμερα εφαρμόζεται "ανά διανομής". Γι ' αυτό, η συνολική κατανομή μπορεί να υπολογιστεί πολλαπλασιάζοντας το όριο με την καταμέτρηση διανομής. Για να κατά προσέγγιση τον μέγιστο αριθμό γραμμών στη συναλλαγή διαιρέστε το καπέλο κατανομή με το συνολικό μέγεθος του κάθε γραμμή. Για στήλες μεταβλητού μήκους εξετάστε το ενδεχόμενο να διαρκεί ένα χρονικό average στήλης αντί να χρησιμοποιήσετε το μέγιστο μέγεθος.

Στον πίνακα κάτω από τις εξής υποθέσεις έχουν γίνει:

* Παρουσιάστηκε μια κατανομή των δεδομένων 
* Το μήκος της γραμμής μέσου είναι 250 byte

| [DWU][]    | Κεφαλαίων ανά διανομής (απομείωση) | Αριθμός κατανομές | ΜΈΓΙΣΤΟ μέγεθος συναλλαγών (απομείωση) | # Γραμμές ανά διανομής | Ο μέγιστος αριθμός γραμμών ανά συναλλαγή |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2.25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  διαίρεσης 4,5                       | 60                      |  270                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  ως 60.000.000             |  3,600,000,000           |
| DW3000 | 22.5                       | 60                      |  1,350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2,700                     | 180,000,000             | 10,800,000,000           |

Το όριο μεγέθους συναλλαγής εφαρμόζεται ανά συναλλαγή ή μια λειτουργία. Δεν εφαρμόζεται σε όλες τις ταυτόχρονες συναλλαγές. Επομένως, κάθε συναλλαγή επιτρέπεται να συντάξετε το ποσό των δεδομένων για το αρχείο καταγραφής. 

Βελτιστοποίηση και να ελαχιστοποιήσετε την ποσότητα των δεδομένων στο αρχείο καταγραφής, ανατρέξτε στο άρθρο [συναλλαγές βέλτιστες πρακτικές][] .

> [AZURE.WARNING] Το μέγεθος του μέγιστου συναλλαγής να μόνο θεωρηθεί ΚΑΤΑΚΕΡΜΑΤΙΣΜΌΣ ή ακόμα και πίνακες ROUND_ROBIN κατανεμημένης πού βρίσκεται η μετάδοση των δεδομένων. Εάν η συναλλαγή είναι εγγραφή δεδομένων με ασύμμετρη τρόπο για τη διανομή, στη συνέχεια, το όριο είναι πιθανό να σας καλούν πριν από το μέγεθος του μέγιστου συναλλαγή.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Κατάσταση συναλλαγής
Αποθήκη δεδομένων του SQL χρησιμοποιεί τη συνάρτηση XACT_STATE() για την αναφορά των αποτυχημένων συναλλαγή χρησιμοποιώντας την τιμή του -2. Αυτό σημαίνει ότι η συναλλαγή απέτυχε και έχει επισημανθεί για επαναφορά μόνο

> [AZURE.NOTE] Τη χρήση του -2 από τη συνάρτηση XACT_STATE για να καταδείξετε μια συναλλαγή που απέτυχε αντιπροσωπεύει διαφορετική συμπεριφορά με τον SQL Server. SQL Server χρησιμοποιεί την τιμή -1 για να αντιπροσωπεύει μια μη committable συναλλαγή. SQL Server μπορεί να αποδεχτεί τις ορισμένα σφάλματα μέσα σε μια συναλλαγή χωρίς να χρειάζεται να επισημανθεί ως μη committable. Για παράδειγμα `SELECT 1/0` θα έχει ως αποτέλεσμα σφάλμα αλλά δεν επιβάλετε συναλλαγή σε μια μη committable κατάσταση. SQL Server επιτρέπει επίσης διαβάζει στη κατάργησης committable συναλλαγή. Ωστόσο, αποθήκη δεδομένων του SQL δεν σας επιτρέπουν να κάνετε το εξής. Εάν προκύψει σφάλμα στο εσωτερικό μιας συναλλαγής αποθήκη δεδομένων του SQL θα εισαγάγει αυτόματα την κατάσταση-2 και δεν θα μπορείτε να κάνετε οποιαδήποτε επιλογή περαιτέρω προτάσεις μέχρι την πρόταση πραγματοποιήθηκε επαναφορά. Επομένως, είναι σημαντικό να ελέγξετε ότι κώδικα της εφαρμογής σας για να δείτε εάν χρησιμοποιεί XACT_STATE() ως που ίσως χρειαστεί να κάνετε τροποποιήσεις κώδικα.

Για παράδειγμα, στο SQL Server ενδέχεται να μπορείτε να δείτε μια συναλλαγή που μοιάζει κάπως έτσι:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Εάν αφήσετε τον κωδικό ως έχει παραπάνω, στη συνέχεια, θα λάβετε το ακόλουθο μήνυμα σφάλματος:

Μήνυμα λάθους 111233, 16 επίπεδο, κατάσταση 1, γραμμή 1 111233; Η τρέχουσα συναλλαγή ματαιώθηκε και τις αλλαγές σε εκκρεμότητα έχουν επαναφορά. Αιτία: Συναλλαγή σε κατάσταση μόνο για επαναφορά ήταν δεν ρητά επανέρχεται πριν από μια DDL, ΟΘΔ ή πρόταση SELECT.

Επίσης δεν θα εμφανιστεί το αποτέλεσμα των συναρτήσεων ERROR_ *.

Στο αποθήκη δεδομένων του SQL του κώδικα πρέπει να είναι λίγο τροποποιηθεί:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Η αναμενόμενη συμπεριφορά παρατηρείται τώρα. Θα γίνεται η διαχείριση του σφάλματος στη συναλλαγή και τις συναρτήσεις ERROR_ * παρέχουν τιμές όπως αναμένεται.

Το μόνο που έχει αλλάξει που είναι το `ROLLBACK` της κίνησης έπρεπε να συμβεί πριν από την ανάγνωση των πληροφοριών σφάλματος που περιλαμβάνει το `CATCH` μπλοκ.

## <a name="errorline-function"></a>Συνάρτηση Error_Line()
Επίσης, είναι αξίζει να αναφερθούν ότι αποθήκη δεδομένων του SQL δεν υλοποίηση ή δεν υποστηρίζει αυτήν τη συνάρτηση ERROR_LINE(). Εάν έχετε αυτό στον κώδικά σας θα πρέπει να καταργήσετε, για να είναι συμβατή με SQL αποθήκη δεδομένων. Χρησιμοποιήστε ετικέτες ερωτήματος στον κώδικα για την υλοποίηση ισοδύναμη λειτουργικότητα. Ανατρέξτε στο άρθρο της [ΕΤΙΚΈΤΑΣ][] για περισσότερες λεπτομέρειες σχετικά με αυτήν τη δυνατότητα.

## <a name="using-throw-and-raiserror"></a>Χρήση ΠΕΤΏ και RAISERROR
ΠΕΤΏ είναι η πιο σύγχρονο υλοποίηση για ανύψωση εξαιρέσεις σε αποθήκη δεδομένων του SQL, αλλά υποστηρίζεται επίσης RAISERROR. Υπάρχουν μερικές διαφορές που αξίζει να πληρώνετε ωστόσο την προσοχή.

- Μηνύματα σφάλματος που δεν μπορεί να είναι αριθμοί στην περιοχή 100,000 150.000 για ΠΕΤΏ ορίζονται από το χρήστη
- Μηνύματα σφάλματος RAISERROR επιδιορθώνονται στο 50000
- Χρήση των sys.messages δεν υποστηρίζεται

## <a name="limitiations"></a>Limitiations
Αποθήκη δεδομένων του SQL διαθέτει μερικά άλλους περιορισμούς που σχετίζονται με συναλλαγές.

Αυτές είναι οι εξής:

- Δεν υπάρχει κατανεμημένες συναλλαγές
- Δεν υπάρχει ένθετες συναλλαγές επιτρέπεται
- Χωρίς αποθήκευση σημεία επιτρέπονται
- Δεν υπάρχει καθορισμένη συναλλαγές
- Σήμανση συναλλαγές
- Δεν υπάρχει υποστήριξη για DDL όπως `CREATE TABLE` μέσα σε ένα χρήστη που ορίζονται από το συναλλαγής

## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με τη βελτιστοποίηση συναλλαγές, ανατρέξτε στο θέμα [συναλλαγές βέλτιστες πρακτικές][].  Για να μάθετε περισσότερα σχετικά με άλλες βέλτιστες πρακτικές αποθήκη δεδομένων του SQL, ανατρέξτε στο θέμα [βέλτιστες πρακτικές αποθήκη δεδομένων του SQL][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Συναλλαγές βέλτιστες πρακτικές]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-best-practices.md
[ΕΤΙΚΈΤΑ]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->

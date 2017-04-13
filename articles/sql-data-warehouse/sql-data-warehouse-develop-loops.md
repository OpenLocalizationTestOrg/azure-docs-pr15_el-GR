<properties
   pageTitle="Επαναλήψεις στην αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για βρόχοι Transact-SQL και αντικατάσταση δείκτες σε αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="loops-in-sql-data-warehouse"></a>Επαναλήψεις στην αποθήκη δεδομένων του SQL
Αποθήκη δεδομένων του SQL υποστηρίζει το βρόχο [ΕΝΏ][] για την εκτέλεση επανειλημμένα μπλοκ πρόταση. Αυτό θα συνεχιστεί για με την προϋπόθεση ότι οι καθορισμένες συνθήκες είναι αληθείς ή μέχρι τον κώδικα συγκεκριμένα τερματίζει την επανάληψη χρησιμοποιώντας το `BREAK` λέξεων-κλειδιών. Επαναλήψεις είναι ιδιαίτερα χρήσιμη για την αντικατάσταση δρομείς που ορίζονται στον κώδικα SQL. Σχεδόν όλα δρομείς που είναι γραμμένες σε κώδικα SQL είναι η γρήγορη προώθηση, διαβάστε Ευτυχώς, μόνο ποικιλία. Επομένως, [ΕΝΏ] επαναλήψεις είναι μια εξαιρετική εναλλακτική λύση εάν διαπιστώσετε ότι χρειάζεται να αντικαταστήσετε μία.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Αξιοποίηση επαναλήψεις και αντικατάστασης δείκτες σε αποθήκη δεδομένων του SQL
Ωστόσο, πριν από την καταδύσεις στην κεφαλή πρώτα πρέπει να ζητήσετε από τον εαυτό σας το ακόλουθο ερώτημα: "Ήταν αυτού του δρομέα δυνατή εκ νέου η εγγραφή για να χρησιμοποιήσετε λειτουργίες συνόλου βάσει;". Σε πολλές περιπτώσεις, η απάντηση θα Ναι και συχνά είναι η καλύτερη προσέγγιση. Μια λειτουργία συνόλου με βάση συχνά εκτελεί σημαντικά ταχύτερα από προσέγγισης επαναληπτικού, γραμμή προς γραμμή.

Γρήγορη προώθηση δρομείς μόνο για ανάγνωση μπορεί να αντικατασταθεί εύκολα με μια βρόχου δομή. Ακολουθεί ένα απλό παράδειγμα. Αυτό το παράδειγμα κώδικα ενημερώνει τα στατιστικά στοιχεία για κάθε πίνακα στη βάση δεδομένων. Με τις διαδοχικές πάνω από τους πίνακες στο βρόχο είναι δυνατή η εκτέλεση κάθε εντολή στην ακολουθία.

Πρώτα, δημιουργήστε ένα προσωρινό πίνακα που περιέχει έναν αριθμό μοναδικών γραμμών που χρησιμοποιούνται για τον προσδιορισμό του μεμονωμένες προτάσεις:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Δεύτερον, προετοιμασία τις μεταβλητές που απαιτούνται για την εκτέλεση του βρόχου:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Επανάληψη τώρα μέσω δηλώσεις εκτέλεση μία κάθε φορά:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Τέλος αποθέστε τον προσωρινό πίνακα που έχουν δημιουργηθεί με το πρώτο βήμα

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[ΧΡΌΝΟΣ]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->

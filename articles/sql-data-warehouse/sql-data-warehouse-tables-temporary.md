<properties
   pageTitle="Προσωρινό πίνακες στην αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Γρήγορα αποτελέσματα με το προσωρινό πίνακες στην αποθήκη δεδομένων του SQL Azure."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Προσωρινό πίνακες στην αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Τύποι δεδομένων][]
- [Διανομή][]
- [Ευρετήριο][]
- [Partition][]
- [Στατιστικά στοιχεία][]
- [Προσωρινό][]

Προσωρινό οι πίνακες είναι ιδιαίτερα χρήσιμη κατά την επεξεργασία δεδομένων - ιδίως κατά τη διάρκεια μετασχηματισμού όπου τα αποτελέσματα ενδιάμεσου είναι μεταβατικές. Στο αποθήκη δεδομένων του SQL προσωρινό πίνακες υπάρχουν στο επίπεδο της περιόδου λειτουργίας.  Αυτές είναι ορατές μόνο στην περίοδο λειτουργίας στην οποία έχουν δημιουργηθεί και χάνονται αυτόματα κατά την αποσύνδεση αυτήν την περίοδο λειτουργίας.  Προσωρινό πίνακες παρέχουν ένα πλεονέκτημα επιδόσεων επειδή τα αποτελέσματά τους είναι γραμμένες σε τοπική και όχι απομακρυσμένης αποθήκευσης.  Προσωρινό οι πίνακες είναι λίγο διαφορετικά στην αποθήκη δεδομένων του SQL Azure από βάση δεδομένων SQL Azure, όπως αυτές είναι δυνατή η πρόσβαση από οπουδήποτε μέσα την περίοδο λειτουργίας, συμπεριλαμβανομένων των τόσο εντός και εκτός της μια αποθηκευμένη διαδικασία.

Σε αυτό το άρθρο περιέχει βασικές οδηγίες για τη χρήση προσωρινό πίνακες και επισημαίνει τις αρχές της περιόδου λειτουργίας επιπέδου προσωρινό πίνακες. Χρησιμοποιώντας τις πληροφορίες σε αυτό το άρθρο σάς βοηθούν να modularize τον κωδικό, βελτίωση της δυνατότητα επαναχρησιμοποίησης και διευκόλυνση της συντήρησης του κώδικα.

## <a name="create-a-temporary-table"></a>Δημιουργήστε έναν προσωρινό πίνακα

Προσωρινό πίνακες δημιουργούνται από την τοποθέτηση απλώς το όνομα του πίνακα με μια `#`.  Για παράδειγμα:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Προσωρινό πίνακες μπορούν επίσης να δημιουργηθούν με ένα `CTAS` χρησιμοποιώντας ακριβώς την ίδια προσέγγιση:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`είναι πολύ ισχυρή εντολής και έχει το πλεονέκτημα ότι πολύ αποτελεσματική σε τη χρήση του χώρου καταγραφής συναλλαγών. 


## <a name="dropping-temporary-tables"></a>Απόθεση προσωρινό πίνακες

Όταν δημιουργείται μια νέα περίοδο λειτουργίας, θα πρέπει να υπάρχει προσωρινό πίνακες.  Ωστόσο, εάν καλείτε την ίδια διαδικασία αποθηκευμένες, η οποία δημιουργεί ένα προσωρινό με το ίδιο όνομα, για να βεβαιωθείτε ότι το `CREATE TABLE` προτάσεις είναι επιτυχής ένα απλό ελέγχου προ-ύπαρξης με μια `DROP` μπορεί να χρησιμοποιηθεί ως σε το παρακάτω παράδειγμα:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Για την κωδικοποίηση συνέπειας, είναι καλό να χρησιμοποιήσετε αυτό το μοτίβο για πίνακες και προσωρινό πίνακες.  Είναι επίσης καλή ιδέα να χρησιμοποιήσετε `DROP TABLE` για να καταργήσετε προσωρινά πίνακες όταν ολοκληρώσετε μαζί τους στον κώδικά σας.  Σε αποθηκευμένη διαδικασία ανάπτυξης είναι αρκετά συνηθισμένο για να δείτε τις εντολές απόθεσης συνδυασμένα στο τέλος της μια διαδικασία για να βεβαιωθείτε ότι αυτά τα αντικείμενα διαγράφονται.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing κώδικα

Επειδή το προσωρινό πίνακες μπορούν να προβληθούν σε οποιοδήποτε σημείο σε μια περίοδο λειτουργίας χρήστη, αυτό μπορεί να είναι εκμετάλλευση θα σας βοηθήσουν να modularize κώδικα της εφαρμογής σας.  Για παράδειγμα, η παρακάτω αποθηκευμένη διαδικασία συγκεντρώνει τις προτεινόμενες πρακτικές από το παραπάνω για τη δημιουργία DDL που θα ενημερώνει όλα τα στατιστικά στοιχεία της βάσης δεδομένων κατά όνομα στατιστικής.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Σε αυτό το στάδιο την ενέργεια μόνο που παρουσιάστηκε είναι η δημιουργία μιας αποθηκευμένης διαδικασίας που θα απλώς που δημιουργούνται από ένα προσωρινό πίνακα, stats_ddl #, με προτάσεις DDL.  Αυτή η αποθηκευμένη διαδικασία θα απόθεση #stats_ddl εάν υπάρχει ήδη για να βεβαιωθείτε ότι δεν αποτυγχάνει εάν εκτελέσετε περισσότερες από μία φορές μέσα σε μια περίοδο λειτουργίας.  Ωστόσο, εφόσον δεν υπάρχει καμία `DROP TABLE` στο τέλος της αποθηκευμένης διαδικασίας, όταν ολοκληρωθεί η αποθηκευμένη διαδικασία, αυτό θα αποχώρηση από τον πίνακα που έχουν δημιουργηθεί, έτσι ώστε να μπορεί να διαβαστεί εκτός της αποθηκευμένης διαδικασίας.  Στην αποθήκη δεδομένων του SQL, σε αντίθεση με άλλες βάσεις δεδομένων SQL Server, είναι δυνατό να χρησιμοποιήσετε τον προσωρινό πίνακα εκτός τη διαδικασία που το δημιούργησε.  Προσωρινό πίνακες αποθήκη δεδομένων του SQL μπορεί να χρησιμοποιείται **σε οποιοδήποτε σημείο** μέσα στην περίοδο λειτουργίας. Αυτό μπορεί να οδηγήσει σε περισσότερες λειτουργική και διαχειρίσιμα κωδικό ως στο το παρακάτω παράδειγμα:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Περιορισμοί προσωρινό πίνακα

Αποθήκη δεδομένων του SQL επιβάλλουν ορισμένοι περιορισμοί κατά την εφαρμογή προσωρινό πίνακες.  Προς το παρόν, μόνο την περίοδο λειτουργίας εμβέλειας προσωρινό πίνακες υποστηρίζονται.  Καθολικό προσωρινό πινάκων δεν υποστηρίζονται.  Επιπλέον, δεν είναι δυνατό να δημιουργήσετε μοντέλα σε προσωρινό πίνακες.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στα άρθρα [Επισκόπηση πίνακα][Επισκόπηση], [Τύπους δεδομένων πίνακα][Τύπους δεδομένων], [διανομή πίνακα][κατανομή], [δημιουργίας ευρετηρίου για έναν πίνακα][ευρετηρίου], [διαμερισμάτων πίνακα][διαμερισμάτων] και [Στατιστικά στοιχεία πίνακα για τη διατήρηση][Στατιστικά στοιχεία].  Για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές αποθήκη δεδομένων SQL][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση]: ./sql-data-warehouse-tables-overview.md
[Τύποι δεδομένων]: ./sql-data-warehouse-tables-data-types.md
[Διανομή]: ./sql-data-warehouse-tables-distribute.md
[Ευρετήριο]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[Προσωρινό]: ./sql-data-warehouse-tables-temporary.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->

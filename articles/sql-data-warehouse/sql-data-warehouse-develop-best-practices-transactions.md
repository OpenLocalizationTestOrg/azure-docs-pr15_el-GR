<properties
   pageTitle="Βελτιστοποίηση συναλλαγές για αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Βέλτιστη πρακτική εξάσκηση οδηγίες για τη σύνταξη ενημερώσεις αποτελεσματική συναλλαγών σε αποθήκη δεδομένων του SQL Azure"
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
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Βελτιστοποίηση συναλλαγές για αποθήκη δεδομένων του SQL

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να βελτιστοποιήσετε την απόδοση των συναλλαγών κώδικά σας κατά την ελαχιστοποίηση κινδύνου για μεγάλη επαναφορές.

## <a name="transactions-and-logging"></a>Συναλλαγές και καταγραφής

Οι συναλλαγές είναι ένα σημαντικό στοιχείο του μηχανισμού μια σχεσιακή βάση δεδομένων. Αποθήκη δεδομένων του SQL χρησιμοποιεί συναλλαγές κατά την τροποποίηση δεδομένων. Αυτές οι συναλλαγές μπορεί να είναι ρητή ή σιωπηρή. Μία `INSERT`, `UPDATE` και `DELETE` δηλώσεις είναι παραδείγματα έμμεσα συναλλαγές. Ρητή οι συναλλαγές καταγράφονται ρητά από έναν προγραμματιστή χρησιμοποιώντας `BEGIN TRAN`, `COMMIT TRAN` ή `ROLLBACK TRAN` και χρησιμοποιούνται συνήθως όταν πολλαπλές δηλώσεις τροποποίηση πρέπει να είναι συνδεδεμένη μαζί σε μία μονάδα ατομικής. 

Αποθήκη δεδομένων του SQL Azure δεσμεύεται αλλαγές στη βάση δεδομένων με χρήση των αρχείων καταγραφής συναλλαγών. Κάθε κατανομή έχει το δικό της αρχείο καταγραφής συναλλαγών. Εγγραφών στο αρχείο καταγραφής συναλλαγών είναι αυτόματες. Δεν υπάρχει καμία ρύθμιση παραμέτρων απαιτείται. Ωστόσο, ενώ αυτή η διαδικασία εγγυάται την εγγραφή να προκαλέσει μια επιβάρυνσης του συστήματος. Μπορείτε να ελαχιστοποιήσετε αυτό επίδραση με τη σύνταξη κώδικα ενημερώσετε αποτελεσματική. Αποτελεσματική ενημερώσετε κώδικα ευρέως εμπίπτουν σε δύο κατηγορίες.

- Αξιοποιήσετε δομές ελάχιστη καταγραφή όπου είναι δυνατό
- Διαδικασία δεδομένα χρησιμοποιώντας το εύρος δέσμες για να αποφύγετε στον ενικό συναλλαγές μεγάλης διάρκειας
- Εγκρίνει ένα διαμερίσματα εναλλαγή μοτίβο για μεγάλο τροποποιήσεις σε μια δεδομένη partition

## <a name="minimal-vs-full-logging"></a>Ελάχιστο έναντι πλήρους καταγραφής

Σε αντίθεση με πλήρως συνδεδεμένος λειτουργίες, που χρησιμοποιούν το αρχείο καταγραφής συναλλαγών για να παρακολουθείτε κάθε αλλαγή γραμμής, ελάχιστες συνδεδεμένος λειτουργίες παρακολούθηση των αναθέσεων βαθμό και μετα-δεδομένα μόνο τις αλλαγές. Επομένως, ελάχιστη καταγραφή περιλαμβάνει την καταγραφή μόνο τις πληροφορίες που απαιτείται για την επαναφορά της συναλλαγής σε περίπτωση αποτυχίας ή μια πρόσκληση ρητή (`ROLLBACK TRAN`). Καθώς λιγότερο πληροφορίες παρακολουθούνται στο αρχείο καταγραφής συναλλαγών, μια λειτουργία ελάχιστες συνδεδεμένος εκτελεί καλύτερα από μια λειτουργία παρόμοιο μέγεθος πλήρως συνδεδεμένος. Επιπλέον, επειδή λιγότερες εγγραφές μεταβείτε το αρχείο καταγραφής συναλλαγών, δημιουργείται ένα πολύ μικρότερο ποσό δεδομένα του αρχείου καταγραφής και επομένως είναι αποτελεσματικό περισσότερες εισόδου/εξόδου.

Τα όρια ασφαλείας συναλλαγής εφαρμόζονται μόνο σε πλήρως συνδεδεμένος λειτουργίες.

>[AZURE.NOTE] Ελάχιστες συνδεδεμένος λειτουργίες να συμμετάσχετε σε ρητή συναλλαγές. Καθώς παρακολουθούνται όλων των αλλαγών στο εκχώρησης δομές, είναι δυνατό να επαναφέρετε ελάχιστες συνδεδεμένος λειτουργίες. Είναι σημαντικό να κατανοήσετε ότι η αλλαγή έχει συνδεθεί "ελάχιστες" δεν είναι συνδεδεμένος κατάργησης.

## <a name="minimally-logged-operations"></a>Ελάχιστες συνδεδεμένος λειτουργίες

Έχουν τη δυνατότητα να ελάχιστες καταγραφεί τις ακόλουθες λειτουργίες:

- ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ ([CTAS][])
- ΕΙΣΑΓΩΓΉ... ΕΠΙΛΈΞΤΕ
- ΔΗΜΙΟΥΡΓΊΑ ΕΥΡΕΤΗΡΊΟΥ
- Η ΠΡΌΤΑΣΗ ALTER ΕΚ ΝΈΟΥ ΔΗΜΙΟΥΡΓΊΑ ΕΥΡΕΤΗΡΊΟΥ
- ΑΠΌΘΕΣΗ ΕΥΡΕΤΗΡΊΟΥ
- ΠΕΡΙΚΟΠΉ ΠΊΝΑΚΑ
- ΑΠΌΘΕΣΗ ΠΊΝΑΚΑ
- Η ΠΡΌΤΑΣΗ ALTER ΠΊΝΑΚΑ ΔΙΑΚΌΠΤΗ PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Λειτουργίες κίνηση εσωτερικό δεδομένων (όπως είναι οι `BROADCAST` και `SHUFFLE`) δεν επηρεάζονται από το όριο ασφάλεια συναλλαγή.

## <a name="minimal-logging-with-bulk-load"></a>Ελάχιστη καταγραφή με μαζικής φόρτωσης

`CTAS`και `INSERT...SELECT` είναι και τα δύο μαζική λειτουργίες φόρτωσης. Ωστόσο, και τα δύο επηρεάζονται από τον ορισμό του πίνακα προορισμού και εξαρτώνται από το σενάριο φόρτωση. Τα παρακάτω είναι ένας πίνακας που εξηγεί εάν η λειτουργία μαζικής σας θα πλήρως ή ελάχιστες καταγραφεί:  

| Πρωτεύον ευρετήριο               | Σενάριο φόρτωσης                                            | Λειτουργία σύνδεσης |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Σωρού                        | Οποιαδήποτε                                                      | **Ελάχιστο**  |
| Ευρετήριο συμπλέγματος             | Κενή προορισμού πίνακα                                       | **Ελάχιστο**  |
| Ευρετήριο συμπλέγματος             | Φόρτωση γραμμές δεν επικαλύπτονται με υπάρχουσες σελίδες προορισμού | **Ελάχιστο**  |
| Ευρετήριο συμπλέγματος             | Επικάλυψη φορτωμένου γραμμών με υπάρχουσες σελίδες προορισμού        | Πλήρης         |
| Ευρετήριο Columnstore συμπλέγματος | Μαζική μέγεθος > = 102,400 ανά partition στοιχισμένο διανομής | **Ελάχιστο**  |
| Ευρετήριο Columnstore συμπλέγματος | Μαζική μέγεθος < 102,400 ανά partition στοιχισμένο διανομής  | Πλήρης         |

Είναι αξίζει να αναφερθούν ότι όλες οι εγγραφές για να ενημερώσετε δευτερεύοντα ή μη ομαδοποιημένη ευρετήρια θα είναι πάντα πλήρως συνδεδεμένος λειτουργίες.

> [AZURE.IMPORTANT] Αποθήκη δεδομένων του SQL έχει 60 κατανομές. Επομένως, υπό την προϋπόθεση ότι όλες οι γραμμές είναι ομοιόμορφα κατανεμημένα και προορισμού σε ένα μεμονωμένο διαμερίσματα, δέσμη σας θα πρέπει να περιέχει 6,144,000 γραμμές ή μεγαλύτερο να καταγράφονται ελάχιστες κατά την εγγραφή Columnstore συγκεντρωτικού ευρετηρίου. Εάν ο πίνακας έχει διαμερίσματα και των γραμμών που θα εισαγάγετε καλύπτουν όρια διαμερίσματα, στη συνέχεια, θα χρειαστεί 6,144,000 γραμμές ανά όριο partition υπό την προϋπόθεση ότι ακόμα και δεδομένα κατανομής. Κάθε διαμερίσματα σε κάθε κατανομή ανεξάρτητη πρέπει να υπερβαίνει το όριο 102,400 γραμμή για την εισαγωγή ελάχιστες να συνδεθείτε στον την κατανομή.

Γίνεται φόρτωση των δεδομένων σε έναν πίνακα μη κενή με ένα συγκεντρωτικό ευρετήριο συχνά μπορούν να περιέχουν διάφορες γραμμές πλήρως συνδεδεμένος και ελάχιστες συνδεδεμένος. Συγκεντρωτικό ευρετήριο είναι ένα δέντρο ισορροπημένες (b-δέντρου) των σελίδων. Εάν η σελίδα που εγγράφονται περιέχει ήδη γραμμές από μια άλλη συναλλαγή, στη συνέχεια, αυτές τις εγγραφές θα πλήρως καταγραφεί. Ωστόσο, εάν η σελίδα είναι κενή, στη συνέχεια, την εγγραφή σε αυτήν τη σελίδα θα ελάχιστες καταγραφεί.

## <a name="optimizing-deletes"></a>Βελτιστοποίηση διαγράφει

`DELETE`είναι μια πλήρως συνδεδεμένος λειτουργία.  Εάν χρειάζεστε για να διαγράψετε ένα μεγάλο όγκο δεδομένων σε έναν πίνακα ή ένα διαμερίσματα, είναι συχνά πιο λογικό να `SELECT` τα δεδομένα που θέλετε να διατηρήσετε, τα οποία μπορεί να εκτελεστεί ως ελάχιστες συνδεδεμένος λειτουργία.  Για να γίνει αυτό, δημιουργήστε έναν νέο πίνακα με [CTAS][].  Μετά τη δημιουργία, χρησιμοποιήστε [ΜΕΤΟΝΟΜΑΣΊΑ][] να κάνετε εναλλαγή από το παλιό πίνακα με τον πίνακα που μόλις δημιουργήθηκε.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Βελτιστοποίηση των ενημερώσεων

`UPDATE`είναι μια πλήρως συνδεδεμένος λειτουργία.  Εάν πρέπει να ενημερώσετε ένα μεγάλο αριθμό γραμμών σε έναν πίνακα ή ένα διαμερίσματα μπορεί να είναι συχνά πολύ πιο αποτελεσματική να χρησιμοποιήσετε μια λειτουργία ελάχιστες συνδεδεμένος όπως [CTAS][] για να το κάνετε.

Στο παράδειγμα κάτω από έναν πλήρη πίνακα ενημέρωσης έχει μετατραπεί σε ένα `CTAS` ώστε να είναι πιθανό ελάχιστη καταγραφή.

Σε αυτήν την περίπτωση θα σας εκ των υστέρων προσθέτετε ένα ποσό έκπτωσης με τις πωλήσεις του πίνακα:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Εκ νέου δημιουργία μεγάλους πίνακες μπορούν να επωφεληθούν από τη χρήση των δυνατοτήτων διαχείρισης φόρτο εργασίας αποθήκη δεδομένων του SQL. Για περισσότερες λεπτομέρειες, ανατρέξτε στην ενότητα Διαχείριση φόρτο εργασίας στο άρθρο [ταυτόχρονης εκτέλεσης][] .

## <a name="optimizing-with-partition-switching"></a>Βελτιστοποίηση με εναλλαγή partition

Όταν αντιμετωπίζουν με μεγάλη κλίμακα τροποποιήσεις στο εσωτερικό ενός [πίνακα][], ένα διαμερίσματα εναλλαγή μοτίβο καθιστά πολύ νόημα. Εάν η τροποποίηση δεδομένων είναι σημαντική και επεκτείνεται σε περισσότερα από ένα διαμερίσματα, απλώς διαδοχικές προσεγγίσεις τα διαμερίσματα επιτυγχάνει, στη συνέχεια, το ίδιο αποτέλεσμα.

Τα βήματα για να εκτελέσετε ένα διακόπτη διαμερίσματα είναι ως εξής:
1. Δημιουργήστε ένα κενό ανάληψη partition
2. Εκτελέστε το "Ενημέρωση" ως μια CTAS
3. Μετάβαση από τα υπάρχοντα δεδομένα στον πίνακα out
4. Μετάβαση στα νέα δεδομένα
5. Εκκαθάριση των δεδομένων

Ωστόσο, για να εντοπίσετε τα διαμερίσματα για να μεταβείτε πρώτα θα πρέπει να δημιουργήσετε μια διαδικασία Βοήθειας όπως αυτό που παρακάτω. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Αυτή η διαδικασία πραγματοποιεί Μεγιστοποίηση εκ νέου χρήση κώδικα και διατηρεί τα διαμερίσματα εναλλαγή παράδειγμα πιο συμπυκνωμένος.

Ο παρακάτω κώδικας παρουσιάζει τα πέντε βήματα που αναφέρονται παραπάνω για την επίτευξη μιας πλήρους partition εναλλαγή ρουτίνας.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Ελαχιστοποίηση της καταγραφής με μικρές δέσμες

Για την τροποποίηση λειτουργίες μεγάλου όγκου δεδομένων, μπορεί να είναι νόημα να χωρίσετε τη λειτουργία σε μπλοκ ή δέσμες για την εμβέλεια της μονάδας της εργασίας.

Ένα παράδειγμα στην πράξη παρέχονται παρακάτω. Το μέγεθος της δέσμης έχει οριστεί σε trivial αριθμό για να επισημάνετε την τεχνική. Στην πραγματικότητα το μέγεθος δέσμης θα ήταν πολύ μεγαλύτερο. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Παύση και κλίμακας καθοδήγηση

Αποθήκη δεδομένων του SQL Azure σάς επιτρέπει να παύση, συνέχιση και κλιμάκωση σας αποθήκη δεδομένων ζήτηση. Όταν κάνετε παύση ή την κλίμακα σας αποθήκη δεδομένων του SQL είναι σημαντικό να κατανοήσετε ότι οι συναλλαγές εν πτήσει τερματίζονται αμέσως; προκαλεί τυχόν ανοιχτά συναλλαγές για να γίνει επαναφορά. Εάν το φόρτο εργασίας είχε εκδοθεί ένα χρόνο εκτελείται και μη ολοκληρωμένες τροποποίηση δεδομένων πριν από τη λειτουργία παύση ή την κλίμακα, στη συνέχεια αυτής της εργασίας θα πρέπει να είναι δυνατή η αναίρεση. Αυτό μπορεί να επηρεάσουν το χρόνο που χρειάζεται για να διακόψετε ή να κλιμακωθεί βάσης δεδομένων σας αποθήκη δεδομένων του SQL Azure. 

> [AZURE.IMPORTANT] Και τα δύο `UPDATE` και `DELETE` είναι πλήρως συνδεδεμένος λειτουργίες και ώστε αυτές Αναίρεση/Ακύρωση αναίρεσης λειτουργίες μπορεί να διαρκέσει πολύ περισσότερο από το ισοδύναμο συνδεδεμένοι ελάχιστες λειτουργίες. 

Το σενάριο καλύτερης είναι για να επιτρέψετε στο συναλλαγές τροποποίησης δεδομένων πτήσεων ολοκληρωθεί πριν από την παύση ή κλιμάκωση αποθήκη δεδομένων του SQL. Ωστόσο, αυτό μπορεί να μην πάντα είναι πρακτικό. Για να μετριασμός του κινδύνου από μια μεγάλη επαναφορά, εξετάστε το ενδεχόμενο να μία από τις ακόλουθες επιλογές:

- Γράψτε εκ νέου χρήση [CTAS][] χρόνο εκτέλεση λειτουργιών
- Διασπάστε τη λειτουργία σε μπλοκ; λειτουργικό σε ένα υποσύνολο των γραμμών

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα [συναλλαγών σε αποθήκη δεδομένων του SQL][] για να μάθετε περισσότερα σχετικά με τα επίπεδα απομόνωσης και συναλλαγών όρια.  Για μια επισκόπηση των άλλων βέλτιστων πρακτικών, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές αποθήκη δεδομένων SQL][].

<!--Image references-->

<!--Article references-->
[Συναλλαγών σε αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-develop-transactions.md
[διαμερίσματα πίνακα]: ./sql-data-warehouse-tables-partition.md
[Ταυτόχρονης εκτέλεσης]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[ΜΕΤΟΝΟΜΑΣΊΑ]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->


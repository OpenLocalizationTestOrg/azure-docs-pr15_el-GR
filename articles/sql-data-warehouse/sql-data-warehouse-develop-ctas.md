<properties
   pageTitle="Δημιουργία πίνακα κατά την επιλογή (CTAS) στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για την κωδικοποίηση με τη δημιουργία πίνακα ως πρόταση select (CTAS) στο αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
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

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Δημιουργία πίνακα ως επιλογή (CTAS) σε αποθήκη δεδομένων του SQL
Δημιουργία πίνακα ως επιλογή ή `CTAS` είναι διαθέσιμη μία από τις πιο σημαντικές δυνατότητες T-SQL. Πρόκειται για ένα πλήρως parallelized λειτουργία που δημιουργεί ένα νέο πίνακα με βάση το αποτέλεσμα μιας πρότασης SELECT. `CTAS`είναι ο απλούστερος και ταχύτερος τρόπος για να δημιουργήσετε ένα αντίγραφο του πίνακα. Μπορείτε να να είναι μια υπερτροφοδότηση έκδοση του `SELECT..INTO` εάν θέλετε. Αυτό το έγγραφο παρέχει παραδείγματα και βέλτιστες πρακτικές για `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Χρήση CTAS για να αντιγράψετε έναν πίνακα

Ίσως ένα από τα πιο συνηθισμένες χρήσεις των `CTAS` είναι να δημιουργήσετε ένα αντίγραφο του πίνακα, έτσι ώστε να μπορείτε να αλλάξετε τον κώδικα DDL. Εάν για παράδειγμα που το δημιούργησε αρχικά πίνακα ως `ROUND_ROBIN` και τώρα θέλετε να το αλλάξετε σε έναν πίνακα κατανεμημένο σε μια στήλη, `CTAS` είναι πώς μπορείτε να αλλάξετε τη στήλη διανομής. `CTAS`μπορεί επίσης να χρησιμοποιηθεί για να αλλάξετε τους τύπους διαμερισμάτων, η δημιουργία ευρετηρίου ή στήλης.

Ας υποθέσουμε ότι δημιουργήσατε αυτόν τον πίνακα με χρήση του προεπιλεγμένου τύπου κατανομή των `ROUND_ROBIN` κατανεμημένης εφόσον δεν υπάρχει στήλη διανομής έχει καθοριστεί σε το `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Τώρα που θέλετε να δημιουργήσετε ένα νέο αντίγραφο της αυτόν τον πίνακα με ευρετήριο Columnstore ομαδοποιημένων, έτσι ώστε να μπορείτε να επωφεληθείτε από τις επιδόσεις της ομαδοποιημένη Columnstore πίνακες. Επίσης που θέλετε να διανείμετε αυτόν τον πίνακα στην ProductKey εφόσον πρόβλεψη σύνδεσμοι σε αυτήν τη στήλη και θέλετε να αποφύγετε την κίνηση δεδομένων κατά τη διάρκεια σύνδεσμοι σε ProductKey. Τέλος θέλετε επίσης να προσθέσετε διαμερισμάτων σε OrderDateKey, ώστε να μπορείτε να διαγράψετε γρήγορα παλιά δεδομένα αποθέτοντας το παλιό τα διαμερίσματα. Παρακάτω θα δείτε τη δήλωση CTAS, το οποίο θα αντιγράψετε τον παλιό πίνακα σε ένα νέο πίνακα.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Τέλος, μπορείτε να μετονομάσετε τους πίνακές σας για να εναλλαγή στο νέο σας πίνακα και, στη συνέχεια, αποθέστε το παλιό πίνακα.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] Αποθήκη δεδομένων του SQL Azure κάνει δεν έχουν ακόμα υποστήριξη αυτόματη δημιουργία ή την αυτόματη ενημέρωση στατιστικά στοιχεία.  Για να λάβετε τις καλύτερες επιδόσεις από τα ερωτήματά σας, είναι σημαντικό στατιστικά στοιχεία να δημιουργηθούν σε όλες τις στήλες όλων των πινάκων μετά την πρώτη φόρτωσης ή προκύπτουν σημαντικές αλλαγές στα δεδομένα.  Για μια αναλυτική εξήγηση των στατιστικών στοιχείων, ανατρέξτε στο θέμα [Στατιστικά στοιχεία][] στην ομάδα ανάπτυξη των θεμάτων.

## <a name="using-ctas-to-work-around-unsupported-features"></a>Χρήση CTAS για να αντιμετωπίσετε μη υποστηριζόμενες δυνατότητες

`CTAS`μπορεί επίσης να χρησιμοποιηθεί για να επιλύσετε έναν αριθμό από τις μη υποστηριζόμενες δυνατότητες που παρατίθενται παρακάτω. Αυτό μπορεί να αποδείξετε συχνά να είναι μια κατάσταση win/win όπως όχι μόνο τον κωδικό θα συμβατή αλλά συχνά θα εκτελεστεί πιο γρήγορα σε αποθήκη δεδομένων του SQL. Αυτό είναι ως αποτέλεσμα το πλήρως parallelized σχεδίασης. Σενάρια που μπορεί να εργαστεί γύρω από με CTAS περιλαμβάνουν τα εξής:

- ΕΠΙΛΈΞΤΕ... ΣΕ
- ΣΎΝΔΕΣΜΟΙ ANSI ενημερώσεις
- Σύνδεσμοι ANSI στα διαγράφει
- ΣΥΓΧΏΝΕΥΣΗ δήλωση

> [AZURE.NOTE] Προσπαθήστε να πιστεύετε ότι "CTAS πρώτη". Εάν πιστεύετε ότι μπορείτε να επιλύσετε ένα πρόβλημα με `CTAS` , στη συνέχεια, που είναι συνήθως ο καλύτερος τρόπος για να προσεγγίσετε το - ακόμα και αν συντάσσετε περισσότερα δεδομένα ως αποτέλεσμα.
>

## <a name="selectinto"></a>ΕΠΙΛΈΞΤΕ... ΣΕ
Μπορεί να βρείτε `SELECT..INTO` εμφανίζεται σε έναν αριθμό ψηφίων στη λύση σας.

Ακολουθεί ένα παράδειγμα ενός `SELECT..INTO` δήλωση:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Για να μετατρέψετε τα παραπάνω για να `CTAS` είναι αρκετά απλή:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] CTAS απαιτεί αυτήν τη στιγμή έχει καθοριστεί μια στήλη διανομής.  Εάν δεν σκόπιμα προσπαθείτε να αλλάξετε τη στήλη διανομής, σας `CTAS` θα εκτελέσει το ταχύτερα, εάν επιλέξετε μια στήλη διανομής που είναι ίδια με τον υποκείμενο πίνακα, όπως αυτή η στρατηγική αποφεύγεται η κίνηση δεδομένων.  Εάν θέλετε να δημιουργήσετε έναν μικρό πίνακα όπου επιδόσεις δεν αποτελεί παράγοντα, στη συνέχεια, μπορείτε να καθορίσετε `ROUND_ROBIN` να αποφύγετε την αποφασίστε σε μια στήλη διανομής.

## <a name="ansi-join-replacement-for-update-statements"></a>Αντικατάσταση συμμετοχή ANSI για δηλώσεις ενημέρωσης

Μπορεί να διαπιστώσετε ότι έχετε μια σύνθετη ενημερωμένη έκδοση που συνδέει περισσότερες από δύο πίνακες μαζί με τη συμμετοχή σε σύνταξη ANSI για να εκτελέσετε την ενημερωμένη ΈΚΔΟΣΗ ή ΔΙΑΓΡΑΦΉ.

Φανταστείτε έπρεπε να ενημερώσετε αυτόν τον πίνακα:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Το αρχικό ερώτημα ενδέχεται να έχετε αναζητήσει κάπως έτσι:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Επειδή το SQL Data Warehouse δεν υποστηρίζει ANSI σύνδεσμοι στο το `FROM` από τον όρο FROM μια `UPDATE` πρόταση, δεν μπορείτε να αντιγράψετε αυτόν τον κωδικό μέσω χωρίς να το αλλάξετε ελαφρώς.

Μπορείτε να χρησιμοποιήσετε ένα συνδυασμό μια `CTAS` και μια έμμεση συμμετοχή για να αντικαταστήσετε αυτόν τον κωδικό:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>Αντικατάσταση συμμετοχή ANSI για διαγραφή δηλώσεις
Ορισμένες φορές είναι ο καλύτερος τρόπος για τη διαγραφή δεδομένων για να χρησιμοποιήσετε `CTAS`. Αντί να διαγράψετε τα δεδομένα απλώς επιλέξτε τα δεδομένα που θέλετε να διατηρήσετε. Αυτό ειδικά true για `DELETE` δηλώσεις που χρησιμοποιούν ansi Ένωσης σύνταξη εφόσον SQL Data Warehouse δεν υποστηρίζει ANSI σύνδεσμοι στο το `FROM` από τον όρο FROM μια `DELETE` πρόταση.

Παράδειγμα μια πρόταση DELETE έχει μετατραπεί είναι διαθέσιμη παρακάτω:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Αντικατάσταση δηλώσεις συγχώνευσης
Συγχώνευση δηλώσεις μπορούν να αντικατασταθούν, τουλάχιστον στο τμήμα, με τη χρήση `CTAS`. Μπορείτε να ενοποιήσετε το `INSERT` και το `UPDATE` σε μια μεμονωμένη πρόταση. Τυχόν εγγραφές που έχουν διαγραφεί θα πρέπει να είναι κλειστά Απενεργοποίηση σε μια δεύτερη πρόταση.

Παράδειγμα μια `UPSERT` είναι διαθέσιμη παρακάτω:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>Σύσταση CTAS: ρητά να δηλώνουν τύπο δεδομένων και δυνατότητα αποδοχής τιμών null εξόδου

Όταν κάνετε μετεγκατάσταση κώδικα μπορεί να σας φανούν εκτελείτε σε αυτόν τον τύπο του μοτίβου κωδικοποίησης:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinctively μπορεί να πιστεύετε ότι θα πρέπει να μπορείτε να μετεγκαταστήσετε αυτόν τον κωδικό ενός CTAS και που θα είναι σωστή. Ωστόσο, υπάρχει ένα κρυφό ζήτημα εδώ.

Ο ακόλουθος κώδικας δεν αποδίδει το ίδιο αποτέλεσμα:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Παρατηρήστε ότι η στήλη "αποτέλεσμα" προς τα εμπρός μεταφέρει τις τιμές δυνατότητα αποδοχής τιμών null και τύπων δεδομένων της παράστασης. Αυτό μπορεί να οδηγήσει σε διακριτική διακυμάνσεις στις τιμές, αν δεν είστε προσεκτικοί.

Δοκιμάστε τα ακόλουθα ως παράδειγμα:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Η τιμή που αποθηκεύεται για το αποτέλεσμα είναι διαφορετικό. Καθώς χρησιμοποιείται η μόνιμων τιμή στη στήλη αποτέλεσμα σε άλλες εκφράσεις το σφάλμα γίνεται ακόμα πιο σημαντική.

![][1]

Αυτό είναι ιδιαίτερα σημαντικό για μετεγκαταστάσεις δεδομένων. Παρόλο που το δεύτερο ερώτημα είναι αναμφισβήτητα πιο ακριβή υπάρχει πρόβλημα. Τα δεδομένα θα είναι διαφορετικό σε σύγκριση με το σύστημα προέλευσης και που οδηγεί σε ερωτήσεις της ακεραιότητας στο την μετεγκατάσταση. Αυτό είναι ένα από αυτά τα σπάνιων περιπτώσεις όπου η απάντηση "λάθος" είναι στην πραγματικότητα τον σωστό τομέα!

Ο λόγος μπορούμε να δούμε ότι αυτό διαφορές μεταξύ των δύο αποτελεσμάτων είναι προς τα κάτω, χύτευσης έμμεσα τύπου. Στο πρώτο παράδειγμα ο πίνακας καθορίζει τον ορισμό της στήλης. Όταν η γραμμή έχει εισαχθεί παρουσιάζεται μια μετατροπή έμμεσα τύπων. Στο δεύτερο παράδειγμα υπάρχει τύπος μη ρητή μετατροπή όπως η παράσταση καθορίζει τύπο δεδομένων της στήλης. Παρατηρήστε επίσης ότι η στήλη στο δεύτερο παράδειγμα έχει οριστεί ως τύπο NULLable στήλη ότι στο πρώτο παράδειγμα δεν έχει. Όταν ο πίνακας που δημιουργήθηκε στην την πρώτη στήλη παράδειγμα, δυνατότητα αποδοχής τιμών null έχει οριστεί ρητά. Στη δεύτερη το ήταν απλώς προς τα αριστερά για να την παράσταση και από προεπιλογή αυτό το παράδειγμα θα έχει ως αποτέλεσμα σε έναν ορισμό NULL.  

Για να επιλύσετε αυτά τα θέματα που πρέπει να ορίσετε ρητά η μετατροπή του τύπου και η δυνατότητα αποδοχής τιμών null σε το `SELECT` τμήμα του `CTAS` πρόταση. Δεν μπορείτε να ορίσετε αυτές τις ιδιότητες στο τμήμα Δημιουργία πίνακα.

Το παρακάτω παράδειγμα δείχνει τον τρόπο για να διορθώσετε τον κώδικα:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Λάβετε υπόψη τα εξής:
- CAST ή ΜΕΤΑΤΡΟΠΉ μπορεί να έχουν χρησιμοποιηθεί
- ISNULL χρησιμοποιείται για να επιβάλετε δυνατότητα αποδοχής τιμών null δεν ΣΥΝΈΝΩΣΗΣ
- ISNULL είναι η συνάρτηση εξωτερικά
- Το δεύτερο τμήμα της ISNULL του είναι μια σταθερά δηλαδή 0

> [AZURE.NOTE] Είναι απαραίτητο να χρησιμοποιήσετε για τη δυνατότητα αποδοχής τιμών null σωστά θα οριστεί `ISNULL` και δεν `COALESCE`. `COALESCE`δεν είναι μια συνάρτηση ντετερμινιστική και επομένως το αποτέλεσμα της παράστασης θα είναι πάντα μηδενικές τιμές. `ISNULL`είναι διαφορετικό. Είναι ντετερμινιστική. Επομένως, όταν το δεύτερο τμήμα της το `ISNULL` συνάρτηση είναι μια σταθερά ή μια καθορισμένη τιμή και, στη συνέχεια, θα είναι η τιμή που προκύπτει NOT NULL.

Αυτή η συμβουλή δεν είναι απλώς χρήσιμο για τη διασφάλιση της ακεραιότητας των τους υπολογισμούς σας. Είναι επίσης σημαντικό μετάβασης διαμερίσματα πίνακα. Φανταστείτε ότι έχετε ορίσει ως το γεγονός αυτόν τον πίνακα:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Ωστόσο, το πεδίο τιμή είναι μια υπολογισμένη παράσταση δεν είναι μέρος των δεδομένων προέλευσης.

Για να δημιουργήσετε διαμερίσματα στο σύνολο δεδομένων σας ενδέχεται να θέλετε να κάνετε το εξής:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Το ερώτημα θα εκτελεστεί τέλεια λεπτομερές. Το πρόβλημα παρουσιάζεται όταν προσπαθείτε να εκτελέσετε το διακόπτη διαμερίσματα. Οι ορισμοί πίνακα δεν συμφωνούν. Για να κάνετε οι ορισμοί πίνακα που ταιριάζει με το CTAS πρέπει να τροποποιηθεί.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Μπορείτε να δείτε, επομένως, ότι συνέπειας τύπο και τη διατήρηση ιδιότητες δυνατότητα αποδοχής τιμών null σε μια CTAS είναι μια καλή πρακτική μηχανικής καλύτερα. Αυτό σας βοηθά να διατηρείται η ακεραιότητα στους υπολογισμούς σας και επίσης εξασφαλίζει ότι εναλλαγή διαμερίσματα είναι δυνατό.

Ανατρέξτε στο MSDN για περισσότερες πληροφορίες σχετικά με τη χρήση [CTAS][]. Είναι μία από τις πιο σημαντικές δηλώσεις στο αποθήκη δεδομένων του SQL Azure. Βεβαιωθείτε ότι έχετε κατανοήσει πλήρως το.

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->

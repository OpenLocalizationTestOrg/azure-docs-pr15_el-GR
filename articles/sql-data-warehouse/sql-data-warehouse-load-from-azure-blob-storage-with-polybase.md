<properties
   pageTitle="Φόρτωση δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure σε αποθήκη δεδομένων του SQL (PolyBase) | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε PolyBase για να φορτώσετε δεδομένα από το χώρο αποθήκευσης αντικειμένων blob του Azure σε αποθήκη δεδομένων του SQL. Φόρτωση μερικούς πίνακες από δημόσιων δεδομένων σε σχήμα αποθήκη δεδομένων Contoso λιανικής πώλησης."
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
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Φόρτωση δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure σε αποθήκη δεδομένων του SQL (PolyBase)

> [AZURE.SELECTOR]
- [Προέλευση δεδομένων](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Χρησιμοποιήστε PolyBase και T-SQL εντολές για τη φόρτωση των δεδομένων από το χώρο αποθήκευσης αντικειμένων blob του Azure στο αποθήκη δεδομένων του SQL Azure. 

Για να διατηρήσετε απλή, αυτό το πρόγραμμα εκμάθησης φορτώνει δύο πινάκων από μια δημόσια αντικειμένων Blob του Azure χώρο αποθήκευσης στη το σχήμα αποθήκη δεδομένων Contoso λιανικής πώλησης. Για να φορτώσετε το πλήρες σύνολο δεδομένων, εκτελέστε το παράδειγμα [Φόρτωση πλήρους αποθήκη δεδομένων Contoso λιανικής πώλησης][] από το χώρο αποθήκευσης δείγματα του Microsoft SQL Server.

Σε αυτό το πρόγραμμα εκμάθησης θα πρέπει:

1. Ρύθμιση παραμέτρων PolyBase για τη φόρτωση από χώρο αποθήκευσης αντικειμένων blob του Azure
2. Φόρτωση δημόσιων δεδομένων στη βάση δεδομένων σας
3. Οι βελτιστοποιήσεις αφού ολοκληρωθεί η φόρτωση.


## <a name="before-you-begin"></a>Πριν ξεκινήσετε
Για να εκτελέσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure που διαθέτει ήδη μια βάση δεδομένων SQL αποθήκη δεδομένων. Εάν δεν έχετε ήδη αυτό, ανατρέξτε στο θέμα [Δημιουργία μιας αποθήκη δεδομένων του SQL][].

## <a name="1-configure-the-data-source"></a>1. ρύθμιση παραμέτρων της προέλευσης δεδομένων

PolyBase χρησιμοποιεί εξωτερικά αντικείμενα T SQL για να καθορίσετε τη θέση και τα χαρακτηριστικά των εξωτερικών δεδομένων. Οι ορισμοί εξωτερικό αντικείμενο είναι αποθηκευμένα σε αποθήκη δεδομένων του SQL. Τα ίδια τα δεδομένα αποθηκεύονται εξωτερικά.

### <a name="11-create-a-credential"></a>1.1. Δημιουργία μιας διαπιστευτηρίων

**Παράλειψη αυτού του βήματος** εάν γίνεται φόρτωση των δεδομένων δημόσια Contoso. Δεν χρειάζεται ασφαλή πρόσβαση στα δεδομένα δημόσια εφόσον είναι ήδη προσβάσιμο από οποιονδήποτε.

**Μην παραλείψετε αυτό το βήμα** Εάν χρησιμοποιείτε αυτό το πρόγραμμα εκμάθησης ως πρότυπο για τη φόρτωση τα δικά σας δεδομένα. Για να αποκτήσετε πρόσβαση σε δεδομένα μέσω μιας διαπιστευτηρίων, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για να δημιουργήσετε μια πιστοποίηση εμβέλεια βάσης δεδομένων και, στη συνέχεια, να το χρησιμοποιήσετε όταν καθορίζει τη θέση του αρχείου προέλευσης δεδομένων.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

Μεταβείτε στο βήμα 2.

### <a name="12-create-the-external-data-source"></a>1.2. Δημιουργία της εξωτερικής προέλευσης δεδομένων

Χρησιμοποιήστε αυτή την εντολή [ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌ αρχείο ΠΡΟΈΛΕΥΣΗΣ ΔΕΔΟΜΈΝΩΝ][] για να αποθηκεύσετε τη θέση των δεδομένων, καθώς και τον τύπο των δεδομένων. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Εάν επιλέξετε να κάνετε το αντικειμένων blob του azure χώρους αποθήκευσης δημόσια, να θυμάστε ότι ως κάτοχος δεδομένων που θα χρεωθεί για τα δεδομένα εξόδου χρεώσεις όταν δεδομένων αφήνει το κέντρο δεδομένων. 

## <a name="2-configure-data-format"></a>2. ρύθμιση παραμέτρων μορφοποίηση δεδομένων

Τα δεδομένα αποθηκεύονται σε αρχεία κειμένου στο χώρο αποθήκευσης αντικειμένων blob του Azure και κάθε πεδίο διαχωρίζονται με οριοθέτη. Εκτελέστε αυτή την εντολή [ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΏΝ ΜΟΡΦΉ ΑΡΧΕΊΟΥ][] για να καθορίσετε τη μορφοποίηση των δεδομένων με τα αρχεία κειμένου. Τα δεδομένα Contoso είναι αρχεία μη συμπιεσμένων και κατακόρυφη γραμμή με κόμματα.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. Δημιουργία εξωτερικών πινάκων

Τώρα που έχετε καθορίσει τα δεδομένα προέλευσης και τη μορφή αρχείου, είστε έτοιμοι να δημιουργήσετε οι εξωτερικοί πίνακες. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Δημιουργήστε ένα σχήμα για τα δεδομένα. 

Για να δημιουργήσετε μια θέση για την αποθήκευση των δεδομένων Contoso στη βάση δεδομένων σας, δημιουργήστε ένα σχήμα.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. Δημιουργία εξωτερικής πινάκων. 

Εκτελέστε αυτήν τη δέσμη ενεργειών για τη δημιουργία πινάκων εξωτερικών DimProduct και FactOnlineSales. Όλα κάνουμε εδώ καθορίζει τα ονόματα στηλών και τους τύπους δεδομένων και τους η σύνδεση με τη θέση και μορφή για τα αρχεία αποθήκευσης αντικειμένων blob του Azure. Τον ορισμό είναι αποθηκευμένο σε αποθήκη δεδομένων του SQL και τα δεδομένα είναι ακόμα σε το αντικειμένων Blob του Azure χώρου αποθήκευσης.

Η παράμετρος **ΘΈΣΗ** είναι ο φάκελος κάτω από τον ριζικό φάκελο στο αντικείμενο Azure χώρο αποθήκευσης Blob. Κάθε πίνακας βρίσκεται σε διαφορετικό φάκελο.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. η φόρτωση των δεδομένων
Υπάρχει διαφορετικούς τρόπους για να αποκτήσετε πρόσβαση σε εξωτερικά δεδομένα.  Μπορείτε να ερωτήματος δεδομένων απευθείας από το εξωτερικό πίνακα, φόρτωση των δεδομένων σε νέους πίνακες βάσης δεδομένων ή προσθήκη εξωτερικών δεδομένων σε υπάρχοντες πίνακες βάσης δεδομένων.  


### <a name="41-create-a-new-schema"></a>4.1. Δημιουργήστε μια νέα διάταξη

CTAS δημιουργεί ένα νέο πίνακα που περιέχει τα δεδομένα.  Πρώτα, δημιουργήστε ένα σχήμα για τα δεδομένα contoso.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Φόρτωση των δεδομένων σε νέους πίνακες

Για να φορτώσετε δεδομένα από το χώρο αποθήκευσης αντικειμένων blob του Azure και αποθηκεύστε το σε έναν πίνακα μέσα σε βάση δεδομένων σας, χρησιμοποιήστε την πρόταση [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΈΞΤΕ (Transact-SQL)][] . Φόρτωση με CTAS αξιοποιεί το ισχυρό τύπο εξωτερικοί πίνακες που μόλις δημιουργήσατε. Για να φορτώσετε τα δεδομένα σε νέους πίνακες, χρησιμοποιήστε μία πρόταση [CTAS][] ανά πίνακα. 

CTAS δημιουργεί ένα νέο πίνακα και συμπληρώνει με τα αποτελέσματα μιας πρότασης select. CTAS καθορίζει τον νέο πίνακα για να έχουν τις ίδιες στήλες και τύπους δεδομένων με τα αποτελέσματα της πρότασης select. Εάν επιλέξετε όλες τις στήλες από έναν εξωτερικό πίνακα, το νέο πίνακα θα ρεπλίκα των στηλών και των τύπων δεδομένων στον εξωτερικό πίνακα.

Σε αυτό το παράδειγμα, μπορούμε να δημιουργήσουμε τη διάσταση και τον πίνακα δεδομένων, όπως ο κατακερματισμός κατανέμεται πίνακες. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 παρακολούθηση της προόδου φόρτωσης

Μπορείτε να παρακολουθήσετε την πρόοδο της σας φόρτωσης χρήση δυναμική Διαχείριση προβολών (DMVs). 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. βελτιστοποίηση συμπίεσης columnstore

Από προεπιλογή, αποθήκη δεδομένων του SQL αποθηκεύει τον πίνακα ως ένα ευρετήριο ομαδοποιημένων columnstore. Αφού ολοκληρωθεί η φόρτωση, ορισμένες από τις γραμμές δεδομένων δεν μπορεί να συμπιέζονται σε το columnstore.  Υπάρχει μια ποικιλία λόγους γιατί αυτό μπορεί να συμβεί. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση columnstore ευρετήρια][].

Για να βελτιστοποιήσετε την απόδοση του ερωτήματος και τη συμπίεση columnstore μετά από μια φόρτωση, εκ νέου δημιουργία του πίνακα για να επιβάλετε το ευρετήριο columnstore για να συμπιέσετε όλες τις γραμμές. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Για περισσότερες πληροφορίες σχετικά με τη διατήρηση columnstore ευρετήρια, ανατρέξτε στο άρθρο [Διαχείριση columnstore ευρετήρια][] .

## <a name="6-optimize-statistics"></a>6. βελτιστοποίηση στατιστικά στοιχεία

Είναι προτιμότερο να δημιουργήσετε στατιστικά στοιχεία μίας στήλης αμέσως μετά μια φόρτωση. Υπάρχουν ορισμένες επιλογές για στατιστικά στοιχεία. Για παράδειγμα, εάν δημιουργήσετε μίας στήλης στατιστικά στοιχεία σε κάθε στήλη, ενδέχεται να χρειαστεί μεγάλο χρονικό διάστημα για να δημιουργήσετε ξανά όλα τα στατιστικά στοιχεία. Εάν γνωρίζετε ότι ορισμένες στήλες δεν πρόκειται να βρίσκονται σε κατηγορήματα ερωτήματος, μπορείτε να παραλείψετε τη δημιουργία στατιστικά στοιχεία σε αυτές τις στήλες.

Εάν αποφασίσετε να δημιουργήσετε μίας στήλης στατιστικά στοιχεία σε κάθε στήλη μέρος κάθε πίνακα, μπορείτε να χρησιμοποιήσετε το δείγμα κώδικα αποθηκευμένη διαδικασία `prc_sqldw_create_stats` στο άρθρο [Στατιστικά στοιχεία][] .

Το παρακάτω παράδειγμα είναι ένα καλό σημείο εκκίνησης για τη δημιουργία στατιστικών στοιχείων. Δημιουργεί μίας στήλης στατιστικά στοιχεία σε κάθε στήλη του πίνακα Διάσταση και, σε κάθε στήλη Ένωσης στους πίνακες δεδομένων. Μπορείτε να πάντα προσθέσετε στατιστικά στοιχεία για μία ή πολλές στήλες σε άλλες στήλες πίνακα fact αργότερα.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Μη κλειδωμένα επιτευγμάτων!

Που έχουν μεταφερθεί με επιτυχία δημόσιων δεδομένων σε αποθήκη δεδομένων του SQL Azure. Τα κατάφερες πολύ καλά!

Τώρα, μπορείτε να ξεκινήσετε την υποβολή ερωτημάτων τους πίνακες με ερωτήματα όπως το εξής:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Επόμενα βήματα
Για να φορτώσετε τα πλήρη δεδομένα αποθήκη δεδομένων Contoso λιανικής πώλησης, χρησιμοποιήστε τη δέσμη ενεργειών σε για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL][].

<!--Image references-->

<!--Article references-->
[Δημιουργήστε μια αποθήκη δεδομένων SQL]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL]: sql-data-warehouse-overview-develop.md
[Διαχείριση columnstore ευρετηρίων]: sql-data-warehouse-tables-index.md
[Στατιστικά στοιχεία]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΉΣ ΠΡΟΈΛΕΥΣΗΣ ΔΕΔΟΜΈΝΩΝ]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌ ΑΡΧΕΊΟ ΜΟΡΦΉΣ]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Φόρτωση του πλήρους αποθήκη δεδομένων Contoso λιανικής πώλησης]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md

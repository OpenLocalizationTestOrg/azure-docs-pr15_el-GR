<properties
   pageTitle="Φόρτωση δεδομένων από SQL Server σε αποθήκη δεδομένων SQL Azure (PolyBase) | Microsoft Azure"
   description="Χρησιμοποιεί bcp για να εξαγάγετε δεδομένα από SQL Server σε επίπεδο αρχείων, AZCopy για την εισαγωγή δεδομένων με το χώρο αποθήκευσης αντικειμένων blob του Azure και PolyBase να ingest τα δεδομένα σε αποθήκη δεδομένων του SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Φόρτωση των δεδομένων με PolyBase στο αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να φορτώσετε δεδομένα σε αποθήκη δεδομένων του SQL με τη χρήση AzCopy και PolyBase. Όταν ολοκληρώσετε τη διαδικασία, θα γνωρίζετε πώς μπορείτε να:

- Χρήση AzCopy για να αντιγράψετε δεδομένα με το χώρο αποθήκευσης αντικειμένων blob του Azure
- Δημιουργία αντικειμένων βάσης δεδομένων για να προσδιορίσετε τα δεδομένα
- Εκτέλεση ενός ερωτήματος T SQL για να φορτώσετε τα δεδομένα

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να περιηγηθείτε στις αυτό το πρόγραμμα εκμάθησης, χρειάζεστε

- Μια βάση δεδομένων SQL αποθήκη δεδομένων.
- Ένας λογαριασμός Azure χώρου αποθήκευσης του τύπου τυπική τοπικά πλεονάζοντα χώρο αποθήκευσης (τυπική-LRS), τυπική παν πλεοναζουσών αποθήκευσης (τυπική-Εξοπλισμό) ή τυπική πρόσβαση για ανάγνωση παν πλεοναζουσών χώρου αποθήκευσης (τυπική-RAGRS).
- Βοηθητικό πρόγραμμα γραμμής εντολών AzCopy. Κάντε λήψη και εγκαταστήστε την [πιο πρόσφατη έκδοση του AzCopy][] η οποία έχει εγκατασταθεί με τα εργαλεία του Microsoft Azure χώρου αποθήκευσης.

    ![Εργαλεία Azure χώρου αποθήκευσης](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Βήμα 1: Προσθήκη δειγμάτων δεδομένων με το χώρο αποθήκευσης αντικειμένων blob του Azure

Για να φορτώσετε δεδομένων, πρέπει να τοποθετήσετε μερικά δείγματα δεδομένων στο χώρο αποθήκευσης αντικειμένων blob του Azure. Σε αυτό το βήμα θα σας να συμπληρώσετε μια Azure χώρο αποθήκευσης αντικειμένων blob με δείγμα δεδομένων. Αργότερα, θα χρησιμοποιήσουμε PolyBase για να φορτώσετε αυτό το δείγμα δεδομένων στη βάση δεδομένων σας αποθήκη δεδομένων του SQL.

### <a name="a-prepare-a-sample-text-file"></a>ΑΠΑΝΤΉΣΕΙΣ. Προετοιμασία ενός δείγματος αρχείου κειμένου

Για να προετοιμάσετε ένα δείγμα αρχείου κειμένου:

1. Ανοίξτε το Σημειωματάριο και αντιγράψτε τις ακόλουθες γραμμές δεδομένων σε ένα νέο αρχείο. Αποθήκευση αυτό στον τοπικό προσωρινό κατάλογο ως % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B. Εύρεση του τελικού σημείου υπηρεσίας blob

Για να βρείτε το blob τελικού σημείου υπηρεσίας:

1. Από την πύλη του Azure, επιλέξτε **Αναζήτηση** > **Λογαριασμούς χώρου αποθήκευσης**.
2. Κάντε κλικ στο λογαριασμό χώρου αποθήκευσης που θέλετε να χρησιμοποιήσετε.
3. Στο blade το λογαριασμό χώρου αποθήκευσης, κάντε κλικ στην επιλογή αντικείμενα BLOB

    ![Κάντε κλικ στην επιλογή αντικείμενα BLOB](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. Αποθηκεύστε τη διεύθυνση URL τελικού σημείου υπηρεσίας blob για αργότερα.

    ![BLOB τελικού σημείου υπηρεσίας](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>Γ. Εύρεση αριθμού-κλειδιού Azure χώρου αποθήκευσης

Για να βρείτε τον αριθμό-κλειδί Azure χώρου αποθήκευσης:

1. Από την πύλη Azure, επιλέξτε **Αναζήτηση** > **Λογαριασμούς χώρου αποθήκευσης**.
2. Κάντε κλικ στο λογαριασμό του χώρου αποθήκευσης που θέλετε να χρησιμοποιήσετε.
3. Επιλέξτε **όλες τις ρυθμίσεις** > **πλήκτρων πρόσβασης**.
4. Κάντε κλικ στο πλαίσιο Αντιγραφή για να αντιγράψετε ένα από τα πλήκτρα πρόσβασης του στο Πρόχειρο.

    ![Αντιγραφή κλειδί Azure αποθήκευσης](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>Δ. Αντιγράψτε το δείγμα αρχείου με το χώρο αποθήκευσης αντικειμένων blob του Azure

Για να αντιγράψετε τα δεδομένα σας με το χώρο αποθήκευσης αντικειμένων blob του Azure:

1. Ανοίξτε μια γραμμή εντολών και αλλάξτε τον κατάλογο στον κατάλογο εγκατάστασης AzCopy. Αυτή η εντολή αλλάζει για να τον προεπιλεγμένο κατάλογο εγκατάστασης σε ένα πρόγραμμα-πελάτη Windows 64 bit.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Εκτελέστε την παρακάτω εντολή για να αποστείλετε το αρχείο. Καθορίστε τη διεύθυνση URL τελικού σημείου υπηρεσίας blob για <blob service endpoint URL> και το κλειδί λογαριασμού Azure χώρου αποθήκευσης για < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Ανατρέξτε επίσης στο θέμα [Γρήγορα αποτελέσματα με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy]την[πιο πρόσφατη έκδοση του AzCopy].

### <a name="e-explore-your-blob-storage-container"></a>Ε. Εξερεύνηση του κοντέινερ χώρου αποθήκευσης αντικειμένων blob

Για να δείτε το αρχείο έχετε αποστείλει σε χώρο αποθήκευσης αντικειμένων blob:

1. Επιστρέψτε στο σας blade υπηρεσίας Blob.
2. Κοντέινερ, κάντε διπλό κλικ στο **datacontainer**.
3. Για να εξερευνήσετε τη διαδρομή στα δεδομένα σας, κάντε κλικ στην επιλογή το φάκελο **datedimension** και θα δείτε το απεσταλμένο αρχείο **DimDate2.txt**.
4. Για να προβάλετε τις ιδιότητες, κάντε κλικ στην επιλογή **DimDate2.txt**.
5. Σημειώστε ότι στο το blade ιδιότητες αντικειμένων Blob, μπορείτε να λάβετε ή διαγράψτε το αρχείο.

    ![Προβολή Azure χώρο αποθήκευσης blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Βήμα 2: Δημιουργία έναν εξωτερικό πίνακα για το δείγμα δεδομένων

Σε αυτήν την ενότητα μπορούμε να δημιουργήσουμε έναν εξωτερικό πίνακα που καθορίζει το δείγμα δεδομένων.

PolyBase χρησιμοποιεί εξωτερικό πίνακες για να αποκτήσετε πρόσβαση σε δεδομένα στο χώρο αποθήκευσης αντικειμένων blob του Azure. Επειδή τα δεδομένα δεν αποθηκεύονται σε αποθήκη δεδομένων του SQL, PolyBase χειρίζεται τον έλεγχο ταυτότητας με εξωτερικά δεδομένα, χρησιμοποιώντας μια πιστοποίηση εμβέλεια βάσης δεδομένων.

Το παράδειγμα σε αυτό το βήμα χρησιμοποιεί αυτές τις προτάσεις Transact-SQL για να δημιουργήσετε έναν εξωτερικό πίνακα.

- [Δημιουργία πρωτεύοντος κλειδιού (Transact-SQL)][] για να κρυπτογραφήσετε το μυστικό της βάσης δεδομένων σας εύρος διαπιστευτηρίων.
- [Δημιουργία βάσης δεδομένων εύρος διαπιστευτηρίων (Transact-SQL)][] για να καθορίσετε τις πληροφορίες ελέγχου ταυτότητας για το λογαριασμό σας Azure χώρου αποθήκευσης.
- [Δημιουργία εξωτερικής προέλευσης δεδομένων (Transact-SQL)][] για να καθορίσετε τη θέση του χώρου αποθήκευσης αντικειμένων blob του Azure.
- [Δημιουργία εξωτερικής μορφή αρχείου (Transact-SQL)][] για να καθορίσετε τη μορφή των δεδομένων σας.
- [Δημιουργία εξωτερικής πίνακα (Transact-SQL)][] για να καθορίσετε τον ορισμό πίνακα και τη θέση των δεδομένων.

Σε αυτό το ερώτημα θα εκτελεστεί σε βάση δεδομένων σας αποθήκη δεδομένων του SQL. Θα δημιουργήσει έναν εξωτερικό πίνακα με το όνομα DimDate2External στο σχήμα dbo που οδηγεί στα δεδομένα δείγματος DimDate2.txt το χώρο αποθήκευσης αντικειμένων blob του Azure.


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


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


Στον SQL Server αντικείμενο Explorer στο Visual Studio, μπορείτε να δείτε τη μορφή εξωτερικό αρχείο, εξωτερική προέλευση δεδομένων και τον πίνακα DimDate2External.

![Προβολή εξωτερικού πίνακα](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Βήμα 3: Φόρτωση δεδομένων σε αποθήκη δεδομένων του SQL

Αφού δημιουργηθεί το εξωτερικό πίνακα, μπορείτε να φορτώσετε τα δεδομένα σε έναν νέο πίνακα ή να το εισαγάγετε σε έναν υπάρχοντα πίνακα.

- Για να φορτώσετε τα δεδομένα σε έναν νέο πίνακα, εκτελέστε την [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ (Transact-SQL)][] πρόταση SELECT. Το νέο πίνακα θα έχει τις στήλες με το όνομα του ερωτήματος. Οι τύποι δεδομένων των στηλών θα ταιριάζουν με τους τύπους δεδομένων στον ορισμό εξωτερικού πίνακα.
- Για να φορτώσετε τα δεδομένα σε έναν υπάρχοντα πίνακα, χρησιμοποιήστε το [Εισαγωγή... ΕΠΙΛΟΓΉ (Transact-SQL)][] πρόταση.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Βήμα 4: Δημιουργία στατιστικά στοιχεία για τα δεδομένα που μόλις έχει φορτωθεί σας

Αποθήκη δεδομένων του SQL μη αυτόματης δημιουργίας ή αυτόματης ενημέρωσης στατιστικά στοιχεία. Γι ' αυτό, για να επιτύχετε επιδόσεις ερωτημάτων υψηλή, είναι σημαντικό να δημιουργήσετε στατιστικά στοιχεία σε κάθε στήλη του κάθε πίνακα μετά την πρώτη φόρτωση. Είναι επίσης σημαντικό για να ενημερώσετε τα στατιστικά στοιχεία μετά τις σημαντικές αλλαγές στα δεδομένα.

Αυτό το παράδειγμα δημιουργεί στατιστικά στοιχεία μίας στήλης στον πίνακα DimDate2.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Στατιστικά στοιχεία][].  


## <a name="next-steps"></a>Επόμενα βήματα
Ανατρέξτε στον [Οδηγό PolyBase][] για περαιτέρω πληροφορίες που πρέπει να γνωρίζετε καθώς σχεδιάζετε μια λύση που χρησιμοποιεί PolyBase.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[Οδηγός PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[πιο πρόσφατη έκδοση του AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΉΣ ΠΡΟΈΛΕΥΣΗΣ ΔΕΔΟΜΈΝΩΝ (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌ ΑΡΧΕΊΟ ΜΟΡΦΉΣ (Transact-SQL)]:https://msdn.microsoft.com/library/dn935026.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΉΣ ΠΊΝΑΚΑ (Transact-SQL)]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΩΣ ΕΠΙΛΟΓΉ (Transact-SQL)]:https://msdn.microsoft.com/library/mt204041.aspx
[ΕΙΣΑΓΩΓΉ... ΕΠΙΛΟΓΉ (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΠΡΩΤΕΎΟΝΤΟΣ ΚΛΕΙΔΙΟΎ (Transact-SQL)]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[ΔΗΜΙΟΥΡΓΊΑ βάσης ΔΕΔΟΜΈΝΩΝ ΕΎΡΟΣ ΔΙΑΠΙΣΤΕΥΤΗΡΊΩΝ (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx

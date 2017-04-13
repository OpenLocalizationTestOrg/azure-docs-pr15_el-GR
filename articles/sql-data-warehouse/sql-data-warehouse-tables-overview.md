<properties
   pageTitle="Επισκόπηση των πινάκων στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Γρήγορα αποτελέσματα με τους πίνακες αποθήκη δεδομένων SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Επισκόπηση των πινάκων στο αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Τύποι δεδομένων][]
- [Διανομή][]
- [Ευρετήριο][]
- [Partition][]
- [Στατιστικά στοιχεία][]
- [Προσωρινό][]

Γρήγορα αποτελέσματα με τη δημιουργία πινάκων στην αποθήκη δεδομένων του SQL είναι απλή.  Η βασική σύνταξη [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ][] ακολουθεί τα κοινά σύνταξη που είναι πιο πιθανό ήδη εξοικειωμένοι από την εργασία με άλλες βάσεις δεδομένων.  Για να δημιουργήσετε έναν πίνακα, πρέπει απλώς να ονομάστε τον πίνακα, δώστε ένα όνομα των στηλών και τον ορισμό τους τύπους δεδομένων για κάθε στήλη.  Εάν δημιουργείτε πίνακες σε άλλες βάσεις δεδομένων, αυτό θα πρέπει να είναι πολύ οικείο για εσάς.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Το παραπάνω παράδειγμα δημιουργεί έναν πίνακα με το όνομα Customers με δύο στήλες, όνομα και επώνυμο.  Κάθε στήλη έχει οριστεί με τύπο δεδομένων VARCHAR(25), το οποίο περιορίζει τα δεδομένα σε 25 χαρακτήρες.  Αυτά τα θεμελιώδη χαρακτηριστικά του πίνακα, καθώς και άλλα άτομα, κυρίως είναι ίδια όπως και άλλες βάσεις δεδομένων.  Τύποι δεδομένων που έχουν οριστεί για κάθε στήλη και διασφάλιση της ακεραιότητας των δεδομένων σας.  Τα ευρετήρια μπορούν να προστεθούν για να βελτιώσετε την απόδοση, μειώνοντας εισόδου/εξόδου.  Δημιουργία διαμερισμάτων μπορούν να προστεθούν για βελτίωση της απόδοσης όταν πρέπει να τροποποιήσετε δεδομένα.

[Μετονομασία] [ RENAME] πίνακα αποθήκη δεδομένων του SQL μοιάζει κάπως έτσι:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Κατανεμημένη πίνακες

Ένα νέο χαρακτηριστικό θεμελιώδεις που έχουν εισαχθεί από κατανεμημένων συστημάτων όπως αποθήκη δεδομένων του SQL είναι η **στήλη διανομής**.  Η στήλη κατανομής είναι πολύ πολύ τι φαίνεται παρακάτω.  Είναι η στήλη η οποία καθορίζει τον τρόπο για να διανείμετε ή διαίρεση, τα δεδομένα σας στο παρασκήνιο.  Όταν δημιουργείτε έναν πίνακα χωρίς να καθορίσετε τη στήλη διανομής, ο πίνακας κατανέμεται αυτόματα με τη χρήση **round robin**.  Ενώ round robin πίνακες μπορούν να είναι αρκετό σε ορισμένα σενάρια, τον ορισμό στηλών διανομής μπορεί να μειώσει κίνηση δεδομένων κατά τη διάρκεια ερωτημάτων, επομένως βελτιστοποίηση των επιδόσεων.  Ανατρέξτε στο θέμα [διανομή πίνακα][κατανομή] για να μάθετε περισσότερα σχετικά με τον τρόπο για να επιλέξετε μια στήλη διανομής.

## <a name="indexing-and-partitioning-tables"></a>Δημιουργία ευρετηρίου και δημιουργία διαμερισμάτων πίνακες

Καθώς γίνονται πιο προχωρημένους χρησιμοποιώντας την αποθήκη δεδομένων του SQL και θέλετε να βελτιστοποιήσετε την απόδοση, που θα θέλετε να μάθετε περισσότερα σχετικά με τη σχεδίαση πίνακα.  Για περισσότερες πληροφορίες, ανατρέξτε στα άρθρα σε [Τύπους δεδομένων πίνακα][Τύπους δεδομένων], [τη διανομή πίνακα][κατανομή], [δημιουργίας ευρετηρίου για έναν πίνακα][ευρετηρίου] και [διαμερισμάτων πίνακα][διαμερίσματα].

## <a name="table-statistics"></a>Στατιστικά στοιχεία πίνακα

Στατιστικά στοιχεία είναι μια εξαιρετική σημασία για να έχετε τις καλύτερες επιδόσεις από την αποθήκη δεδομένων του SQL.  Επειδή το SQL Data Warehouse δεν έχουν ακόμα δημιουργεί αυτόματα και στατιστικά στοιχεία ενημέρωσης για εσάς, όπως που μπορεί να έχουν πρέπει να περιμένουν σε βάση δεδομένων SQL Azure, ανάγνωσης μας άρθρο [στατιστικών][] μπορεί να είναι ένα από τα πιο σημαντικά άρθρα Διαβάστε για να βεβαιωθείτε ότι έχετε τις καλύτερες επιδόσεις από τα ερωτήματά σας.

## <a name="temporary-tables"></a>Προσωρινό πίνακες

Προσωρινό πίνακες είναι πίνακες που μόνο υπάρχει στη διάρκεια της τη σύνδεσή σας και δεν είναι δυνατό να προβληθούν από άλλους χρήστες.  Προσωρινό πίνακες μπορεί να είναι ένας καλός τρόπος για να εμποδίσετε άλλους να βλέπουν προσωρινό αποτελεσμάτων και να μειώσετε επίσης την ανάγκη για την εκκαθάριση.  Επειδή το προσωρινό πίνακες χρησιμοποιούν επίσης τοπικού χώρου αποθήκευσης, μπορούν να παρέχουν ταχύτερη απόδοση για κάποιες εργασίες.  Ανατρέξτε στα άρθρα[προσωρινό] [Προσωρινό πίνακα]για περισσότερες λεπτομέρειες σχετικά με τους πίνακες προσωρινό.

## <a name="external-tables"></a>Εξωτερικοί πίνακες

Εξωτερικοί πίνακες, γνωστές και ως Polybase πίνακες, είναι πίνακες που μπορούν να αναζητηθούν από αποθήκη δεδομένων του SQL, αλλά σημείου σε εξωτερικά δεδομένα από αποθήκη δεδομένων του SQL.  Για παράδειγμα, μπορείτε να δημιουργήσετε έναν εξωτερικό πίνακα ποια σημεία αρχείων στο χώρο αποθήκευσης Blob του Azure.  Για περισσότερες λεπτομέρειες σχετικά με τη δημιουργία και υποβολή ερωτημάτων έναν εξωτερικό πίνακα, ανατρέξτε στο θέμα [Φόρτωση δεδομένων με Polybase][].  

## <a name="unsupported-table-features"></a>Μη υποστηριζόμενες δυνατότητες πίνακα

Ενώ αποθήκη δεδομένων του SQL περιέχει πολλές από τις ίδιες δυνατότητες πίνακα που παρέχεται από άλλες βάσεις δεδομένων, υπάρχουν ορισμένες δυνατότητες οι οποίες δεν υποστηρίζονται ακόμη.  Ακολουθεί μια λίστα με κάποιες από τις δυνατότητες των πινάκων που δεν υποστηρίζονται ακόμη.

| Μη υποστηριζόμενες δυνατότητες |
| --- |
|[Ιδιότητα ταυτότητας][] (ανατρέξτε στο θέμα [Εκχώρηση λύση αριθμό-κλειδί αντικατάστασης][])|
|Πρωτεύον κλειδί, ξένα κλειδιά, ατομικών και ελέγχου [Περιορισμών του πίνακα][]|
|[Μοναδικά ευρετήρια][]|
|[Υπολογιζόμενες στήλες][]|
|[Κατακερματισμένο στηλών][]|
|[Τύποι που ορίζονται από το χρήστη][]|
|[Ακολουθία][]|
|[Εναυσμάτων][]|
|[Οι προβολές με ευρετήριο][]|
|[Συνωνύμων][]|

## <a name="table-size-queries"></a>Τα ερωτήματα μεγέθους πίνακα

Ένα απλό τρόπο για να προσδιορίσετε χώρο και των γραμμών που καταναλώνεται από έναν πίνακα σε κάθε ένα από τα 60 κατανομές, είναι να χρησιμοποιήσετε [DBCC PDW_SHOWSPACEUSED][].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ωστόσο, χρησιμοποιώντας εντολές DBCC μπορεί να αρκετά τον περιορισμό.  Διαχείριση δυναμικών προβολών (DMVs) σάς επιτρέπει να βλέπετε πολύ περισσότερες λεπτομέρειες, καθώς και να σας δώσει πολύ μεγαλύτερο έλεγχο τα αποτελέσματα του ερωτήματος.  Ξεκινήστε με τη δημιουργία αυτής της προβολής, που θα αναφέρεται από πολλά από τα παραδείγματα σε αυτό και άλλα αντικείμενα.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Χώρος πίνακα σύνοψης

Αυτό το ερώτημα επιστρέφει τις γραμμές και τις χώρου με πίνακα.  Είναι μια εξαιρετική ερώτημα για να δείτε τους πίνακες που είναι μεγαλύτερη τους πίνακές σας και αν έχουν round robin ή κατακερματισμός κατανεμημένο.  Για πίνακες κατακερματισμός κατανεμημένο εμφανίζει επίσης τη στήλη διανομής.  Στις περισσότερες περιπτώσεις μεγαλύτερη τους πίνακές σας θα πρέπει να κατακερματισμός διανέμεται με ένα ομαδοποιημένο columnstore ευρετήριο.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Χώρος πίνακα με βάση τον τύπο της κατανομής

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Χώρος πίνακα με βάση τον τύπο ευρετηρίου

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Κατανομή χώρου σύνοψης

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στα άρθρα σε [Τύπους δεδομένων πίνακα][Τύπους δεδομένων], [τη διανομή πίνακα][κατανομή], [δημιουργίας ευρετηρίου για έναν πίνακα][ευρετηρίου], [διαμερισμάτων πίνακα][διαμερισμάτων], [Λαμβάνοντας υπόψη στατιστικά στοιχεία πίνακα][στατιστικών στοιχείων] και [Προσωρινό πίνακες][προσωρινό].  Για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές αποθήκη δεδομένων SQL][].

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
[Φόρτωση των δεδομένων με Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Ιδιότητα ταυτότητας]: https://msdn.microsoft.com/library/ms186775.aspx
[Εκχώρηση λύση κλειδιών υποκατάστασης]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Περιορισμοί πίνακα]: https://msdn.microsoft.com/library/ms188066.aspx
[Υπολογιζόμενες στήλες]: https://msdn.microsoft.com/library/ms186241.aspx
[Κατακερματισμένο στηλών]: https://msdn.microsoft.com/library/cc280604.aspx
[Τύποι που ορίζονται από το χρήστη]: https://msdn.microsoft.com/library/ms131694.aspx
[Ακολουθία]: https://msdn.microsoft.com/library/ff878091.aspx
[Εναυσμάτων]: https://msdn.microsoft.com/library/ms189799.aspx
[Οι προβολές με ευρετήριο]: https://msdn.microsoft.com/library/ms191432.aspx
[Συνωνύμων]: https://msdn.microsoft.com/library/ms177544.aspx
[Μοναδικά ευρετήρια]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->

<properties
   pageTitle="Δημιουργία διαμερισμάτων πίνακες στην αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Γρήγορα αποτελέσματα με πίνακα διαμερισμάτων στο αποθήκη δεδομένων του SQL Azure."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Δημιουργία διαμερισμάτων πίνακες στην αποθήκη δεδομένων του SQL

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Τύποι δεδομένων][]
- [Διανομή][]
- [Ευρετήριο][]
- [Partition][]
- [Στατιστικά στοιχεία][]
- [Προσωρινό][]

Δημιουργία διαμερισμάτων υποστηρίζεται σε όλους τους τύπους πίνακα αποθήκη δεδομένων SQL; συμπεριλαμβανομένων των ομαδοποιημένων columnstore, συγκεντρωτικό ευρετήριο και σωρού.  Δημιουργία διαμερισμάτων υποστηρίζεται επίσης σε όλους τους τύπους διανομής, συμπεριλαμβανομένης της κατακερματισμός ή round robin κατανεμημένο.  Δημιουργία διαμερισμάτων παρέχει τη δυνατότητα να διαιρέσετε τα δεδομένα σε μικρότερες ομάδες δεδομένων και στις περισσότερες περιπτώσεις, διαμερισμάτων μπορεί να γίνει σε μια στήλη ημερομηνιών.

## <a name="benefits-of-partitioning"></a>Πλεονεκτήματα της διαμερισμάτων

Δημιουργία διαμερισμάτων μπορούν να επωφεληθούν απόδοσης συντήρησης και ερωτήματος δεδομένων.  Εάν οφέλη και τα δύο ή απλώς ένα εξαρτάται από τον τρόπο φόρτωση των δεδομένων και εάν της ίδιας στήλης μπορούν να χρησιμοποιηθούν για σκοπούς και τα δύο, επειδή το διαμερισμάτων μπορεί να γίνει μόνο σε μία στήλη.

### <a name="benefits-to-loads"></a>Οφέλη από τη φόρτωση

Το βασικό πλεονέκτημα της διαμερισμάτων στο αποθήκη δεδομένων του SQL είναι βελτιώσει την αποτελεσματικότητα και την απόδοση της φόρτωσης δεδομένων με χρήση των διαμερισμάτων διαγραφής, αλλαγή και τη συγχώνευση.  Στις περισσότερες περιπτώσεις έχει διαμερίσματα δεδομένων σε μια στήλη ημερομηνιών που στενά είναι συνδεδεμένη με τη σειρά που την φόρτωση των δεδομένων στη βάση δεδομένων.  Ένα από τα μεγαλύτερη πλεονεκτήματα της χρήσης διαμερίσματα για να διατηρήσετε τα δεδομένα του την αποφυγή της καταγραφής συναλλαγών.  Ενώ απλώς εισαγωγής, ενημέρωση ή διαγραφή δεδομένων μπορεί να είναι η πιο απλές προσέγγιση, με μια μικρή σκέψης και προσπάθειας, με χρήση διαμερισμάτων κατά τη διαδικασία φόρτωσης να ουσιαστικά βελτιώνει την απόδοση.

Αλλαγή διαμερίσματα μπορεί να χρησιμοποιηθεί για γρήγορη κατάργηση ή αντικατάσταση μιας ενότητας του πίνακα.  Για παράδειγμα, ενός πίνακα πωλήσεων δεδομένων ενδέχεται να περιέχουν μόνο σε δεδομένα για τους τελευταίους 36 μήνες.  Στο τέλος κάθε μήνα, το παλαιότερο μήνα με δεδομένα πωλήσεων, διαγράφονται από τον πίνακα.  Αυτά τα δεδομένα θα μπορούσε να διαγραφεί, χρησιμοποιώντας μια πρόταση delete για να διαγράψετε τα δεδομένα για το παλαιότερο μήνα.  Ωστόσο, η διαγραφή μεγάλης ποσότητας δεδομένων γραμμή προς γραμμή με μια πρόταση delete μπορεί να απαιτεί πολύ χρόνο, καθώς και δημιουργήστε τον κίνδυνο μεγάλο πράξεις, οι οποίες μπορεί να χρειαστεί αρκετό χρόνο για επαναφορά εάν προκύψει κάποιο πρόβλημα.  Μια καλύτερη προσέγγιση είναι να απλώς αποθέστε το παλαιότερο διαμερίσματα δεδομένων.  Όπου η διαγραφή των μεμονωμένων γραμμών μπορεί να διαρκέσει ώρες, τη διαγραφή ενός ολόκληρου διαμερίσματα μπορεί να διαρκέσει δευτερόλεπτα.

### <a name="benefits-to-queries"></a>Πλεονεκτήματα στα ερωτήματα

Διαμερισμάτων μπορεί επίσης να χρησιμοποιηθεί για βελτίωση της απόδοσης του ερωτήματος.  Εάν ένα ερώτημα εφαρμόζει φίλτρο σε μια στήλη διαμερίσματα, αυτό να περιορίσετε τη σάρωση για να μόνο τα κατάλληλα διαμερίσματα που μπορεί να είναι ένα πολύ μικρότερο υποσύνολο των δεδομένων, αποφυγή σάρωσης του πλήρους πίνακα.  Με την εισαγωγή των ευρετηρίων ομαδοποιημένων columnstore, τα οφέλη επιδόσεων κατηγορήματος εξάλειψη είναι λιγότερο χρήσιμα, αλλά σε ορισμένες περιπτώσεις, μπορεί να υπάρχει ένα πλεονέκτημα στα ερωτήματα.  Για παράδειγμα, εάν ο πίνακας πωλήσεων fact έχει διαμερίσματα σε 36 μηνών χρησιμοποιώντας το πεδίο ημερομηνία πώλησης, στη συνέχεια, τα ερωτήματα αυτό το φίλτρο κατά την ημερομηνία πώλησης να παραλείψετε αναζήτηση σε διαμερίσματα που δεν συμφωνούν με το φίλτρο.

## <a name="partition-sizing-guidance"></a>Καθοδήγηση partition αλλαγής μεγέθους

Ενώ διαμερισμάτων μπορούν να χρησιμοποιηθούν για βελτίωση της απόδοσης ορισμένα σενάρια, δημιουργείται ένας πίνακας με **πάρα πολλά** διαμερίσματα μπορεί να προκαλέσει προβλήματα επιδόσεων υπό ορισμένες συνθήκες.  Αυτά τα θέματα είναι ιδιαίτερα true για πίνακες ομαδοποιημένων columnstore.  Για τη δημιουργία διαμερισμάτων για να είναι χρήσιμες, είναι σημαντικό να κατανοήσετε πότε να χρησιμοποιήσετε διαμερισμάτων και τον αριθμό των διαμερισμάτων που θα δημιουργήσετε.  Υπάρχει σκληρό fast κανόνας ως προς πόσες διαμερίσματα είναι υπερβολικά πολλά, εξαρτάται από τα δεδομένα σας και τον αριθμό των διαμερισμάτων που φορτώνονται σε ταυτόχρονα.  Αλλά ως μια γενική πρακτικός κανόνας, πιστεύετε ότι της προσθήκης δεκάδες σε εκατοντάδες των διαμερισμάτων, δεν χιλιάδες.

Κατά τη δημιουργία διαμερισμάτων σε πίνακες **ομαδοποιημένων columnstore** , είναι σημαντικό να λάβετε υπόψη σας τον αριθμό των γραμμών που θα ξεκινάτε με κάθε διαμερίσματα.  Για βέλτιστη συμπίεση και απόδοση των ομαδοποιημένων columnstore πινάκων, απαιτείται τουλάχιστον 1 εκατομμύριο γραμμές ανά διανομής και διαμερίσματα.  Πριν από τα διαμερίσματα δημιουργούνται, αποθήκη δεδομένων του SQL διαιρεί ήδη κάθε πίνακα σε 60 κατανέμεται βάσεις δεδομένων.  Κάθε δημιουργία διαμερισμάτων που προσθέσατε σε έναν πίνακα είναι εκτός από το κατανομές που δημιουργήθηκε στο παρασκήνιο.  Χρησιμοποιώντας αυτό το παράδειγμα, εάν ο πίνακας πωλήσεων fact περιέχεται 36 μηνιαία διαμερίσματα και δεδομένου ότι αποθήκη δεδομένων του SQL 60 κατανομές, στη συνέχεια, στον πίνακα πωλήσεων fact πρέπει να περιέχει 60 εκατομμύρια γραμμές ανά μήνα ή γραμμών δισεκατομμυρίων 2.1 όταν συμπληρώνονται όλους τους μήνες.  Εάν ένας πίνακας περιέχει σημαντικά λιγότερες γραμμές από τον προτεινόμενο ελάχιστο αριθμό γραμμών ανά διαμερίσματα, μπορείτε να χρησιμοποιήσετε τα διαμερίσματα λιγότερες προκειμένου να κάνετε αυξήστε τον αριθμό των γραμμών ανά διαμερίσματα.  Επίσης ανατρέξτε στο άρθρο[ευρετηρίου] [ευρετηρίου]που περιλαμβάνει τα ερωτήματα που μπορούν να εκτελεστούν σε αποθήκη δεδομένων του SQL για την αξιολόγηση της ποιότητας των ευρετηρίων columnstore σύμπλεγμα.

## <a name="syntax-difference-from-sql-server"></a>Σύνταξη διαφορά από SQL Server

Αποθήκη δεδομένων του SQL παρουσιάζει μια απλοποιημένη ορισμό των διαμερισμάτων που είναι λίγο διαφορετική από SQL Server.  Διαμερισμού συναρτήσεις και ηλεκτρονικού "ψαρέματος" δεν χρησιμοποιούνται σε αποθήκη δεδομένων του SQL ως έχουν στον SQL Server.  Αντί για αυτό, μόνο που πρέπει να κάνετε είναι να τον προσδιορισμό διαμερίσματα στηλών και των σημείων όριο.  Κατά τη σύνταξη των διαμερισμάτων μπορεί να είναι λίγο διαφορετική από SQL Server, τις βασικές έννοιες είναι ίδιες.  SQL Server και αποθήκη δεδομένων του SQL υποστηρίζουν μία στήλη διαμερίσματα ανά πίνακα, ο οποίος μπορεί να είναι κυμαινόταν διαμερίσματα.  Για να μάθετε περισσότερα σχετικά με τη δημιουργία διαμερισμάτων, ανατρέξτε στο θέμα [Δημιουργία διαμερισμάτων πίνακες και ευρετήρια][].

Το παρακάτω παράδειγμα μιας πρότασης SQL Data Warehouse διαμερίσματα [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ][] , διαμερίσματα στον πίνακα FactInternetSales στη στήλη OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Μετεγκατάσταση διαμερισμάτων από SQL Server

Για να μετεγκαταστήσετε απλώς ορισμών partition SQL Server στο αποθήκη δεδομένων του SQL:

- Εξάλειψη του SQL Server [συνδυασμού διαμερίσματα][].
- Προσθέστε τον ορισμό [συνάρτηση partition][] στον πίνακα "ΔΗΜΙΟΥΡΓΊΑ".

Εάν κάνετε μετεγκατάσταση διαμερίσματα πίνακα από μια παρουσία του SQL Server την κάτω από το SQL μπορεί να σας βοηθήσει να ερωτημάτων τον αριθμό των γραμμών που βρίσκονται σε κάθε διαμερίσματα.  Λάβετε υπόψη ότι εάν χρησιμοποιείται το ίδιο επίπεδο λεπτομερειών διαμερισμού σε αποθήκη δεδομένων του SQL, τον αριθμό των γραμμών ανά διαμερίσματα θα μείωση κατά έναν παράγοντα 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Διαχείριση φόρτου εργασίας

Ένα ολοκληρωμένο τμήμα εξέταση για να υπολογίσετε της απόφασης διαμερίσματα πίνακα είναι η [Διαχείριση φόρτου εργασίας][].  Διαχείριση φόρτου εργασίας σε αποθήκη δεδομένων του SQL είναι κυρίως τη διαχείριση μνήμης και ταυτόχρονης εκτέλεσης.  Σε αποθήκη δεδομένων του SQL τη μέγιστη μνήμη εκχωρηθεί σε κάθε κατανομή κατά την εκτέλεση ενός ερωτήματος είναι κλάσεις διέπονται πόρων.  Ιδανικά διαμερισμάτων σας θα έχουν μέγεθος λαμβάνοντας υπόψη άλλοι παράγοντες, όπως τις ανάγκες μνήμης της δόμησης ομαδοποιημένων columnstore ευρετήρια.  Ομαδοποιημένη όφελος ευρετήρια columnstore σημαντικά όταν έχουν εκχωρηθεί περισσότερη μνήμη.  Γι ' αυτό, θα θέλετε να εξασφαλίσετε ότι ένα αναδόμηση διαμερίσματα ευρετηρίου δεν είναι starved μνήμης. Κατά τη μετάβαση από τον προεπιλεγμένο ρόλο, smallrc, σε έναν από τους άλλους ρόλους όπως largerc μπορείτε να επιτύχετε αυξάνοντας το ποσό της μνήμης που είναι διαθέσιμη στο ερώτημά σας.

Πληροφορίες σχετικά με την εκχώρηση μνήμης ανά κατανομής είναι διαθέσιμη κατά την υποβολή ερωτημάτων στις προβολές πόρων ρυθμιστής δυναμική διαχείριση. Στην πραγματικότητα σας εκχώρηση μνήμης θα είναι μικρότερη από τα παρακάτω στοιχεία. Ωστόσο, αυτό παρέχει οδηγίες που μπορείτε να χρησιμοποιήσετε κατά την αλλαγή μεγέθους των διαμερισμάτων σας για λειτουργίες διαχείρισης δεδομένων ενός επιπέδου.  Προσπαθήστε να αποφύγετε αλλαγής μεγέθους σας διαμερίσματα πέρα από την εκχώρηση μνήμης που παρέχεται από την κλάση πολύ μεγάλο πόρων. Εάν σας τα διαμερίσματα αυξηθεί πέρα από αυτό το σχήμα που ο κίνδυνος πίεση μνήμης που με τη σειρά οδηγεί σε λιγότερο βέλτιστη συμπίεση.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Αλλαγή partition

Αποθήκη δεδομένων του SQL υποστηρίζει διαμερίσματα διαίρεση, τη συγχώνευση και η αλλαγή. Κάθε μία από αυτές τις συναρτήσεις είναι excuted χρησιμοποιώντας την πρόταση [ALTER TABLE][] .

Για να αλλάξετε τα διαμερίσματα ανάμεσα σε δύο πίνακες πρέπει να βεβαιωθείτε ότι τα διαμερίσματα στοίχιση στην τους αντίστοιχους όρια και ότι οι ορισμοί πίνακα ταιριάζουν. Με τους περιορισμούς ελέγχου δεν είναι διαθέσιμες για την επιβολή της περιοχής των τιμών σε έναν πίνακα στον πίνακα προέλευσης πρέπει να περιέχει τα ίδια όρια διαμερίσματα ως τον πίνακα προορισμού. Εάν αυτό δεν είναι πεζών και κεφαλαίων γραμμάτων, στη συνέχεια, μετάβαση διαμερίσματα θα αποτύχει, τα μετα-δεδομένα διαμερίσματα δεν θα συγχρονιστούν.

### <a name="how-to-split-a-partition-that-contains-data"></a>Πώς μπορείτε να διαιρέσετε ένα διαμερίσματα που περιέχει δεδομένα

Ο πιο αποτελεσματικός τρόπος για να διαιρέσετε ένα διαμερίσματα που περιέχει ήδη δεδομένα είναι να χρησιμοποιήσετε ένα `CTAS` πρόταση. Εάν ο πίνακας διαμερίσματα είναι μια ομαδοποιημένη columnstore, στη συνέχεια, τα διαμερίσματα πίνακα πρέπει να είναι κενές για μπορεί να διαιρεθεί.

Ακολουθεί ένας πίνακας διαμερίσματα columnstore δείγμα που περιέχει μία γραμμή σε κάθε διαμερίσματα:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Με τη δημιουργία του αντικειμένου στατιστική τιμή, θα σας διασφαλίζει ότι η συγκεκριμένη μετα-δεδομένα του πίνακα είναι πιο ακριβείς. Εάν θα σας παραλείψετε τη δημιουργία στατιστικά στοιχεία, στη συνέχεια, αποθήκη δεδομένων του SQL θα χρησιμοποιήσετε τις προεπιλεγμένες τιμές. Για λεπτομέρειες σχετικά με στατιστικά στοιχεία, διαβάστε [Στατιστικά στοιχεία][].

Θα σας, στη συνέχεια, να ερωτήματος για την καταμέτρηση γραμμής χρησιμοποιώντας το `sys.partitions` καταλόγου προβολή:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Εάν Προσπαθούμε να διαιρέσετε αυτόν τον πίνακα, θα σας θα λάβετε ένα μήνυμα σφάλματος:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Μήνυμα λάθους 35346, 15 επίπεδο, κατάσταση 1, γραμμή ΔΙΑΊΡΕΣΗΣ 44 όρος της πρότασης ΤΡΟΠΟΠΟΙΉΣΟΥΝ PARTITION απέτυχε επειδή τα διαμερίσματα δεν είναι κενό.  Μόνο τα διαμερίσματα κενή μπορεί να διαιρεθεί σε όταν υπάρχει ένα ευρετήριο columnstore στον πίνακα. Εξετάστε το ενδεχόμενο απενεργοποίησης του ευρετηρίου columnstore πριν από την έκδοση της δήλωσης PARTITION ΤΡΟΠΟΠΟΙΉΣΕΤΕ και, στη συνέχεια, αναδόμηση ευρετηρίου columnstore μετά την ολοκλήρωση ΤΡΟΠΟΠΟΙΉΣΟΥΝ ΔΙΑΜΕΡΊΣΜΑΤΑ.

Ωστόσο, μπορούμε να χρησιμοποιήσουμε `CTAS` για να δημιουργήσετε έναν νέο πίνακα για τη διατήρηση δεδομένα μας.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Καθώς τα όρια partition στοιχίζονται επιτρέπεται διακόπτη. Αυτό θα αποχώρηση από τον πίνακα προέλευσης με μια κενή διαμερισμάτων που θα σας, στη συνέχεια, να διαιρέσετε.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Το μόνο που απομένει να κάνετε είναι να στοιχίσετε τα δεδομένα για τα νέα όρια διαμερίσματα με χρήση `CTAS` και διακόπτη ξανά τα δεδομένα με τον κύριο πίνακα

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Αφού ολοκληρώσετε τη μετακίνηση των δεδομένων είναι μια καλή ιδέα να ανανεώσετε τα στατιστικά στοιχεία του πίνακα προορισμού για να βεβαιωθείτε ότι εκφράζουν με ακρίβεια η νέα κατανομή των δεδομένων σε τους αντίστοιχους διαμερίσματα:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Πίνακας διαμερισμάτων ελέγχου προέλευσης

Για να αποφύγετε τον ορισμό του πίνακα από **rusting** στο σύστημα προέλευσης στοιχείου ελέγχου μπορεί να θέλετε να λάβετε υπόψη τα παρακάτω προσέγγιση:

1. Δημιουργία πίνακα με διαμερίσματα πίνακα αλλά χωρίς τιμές partition

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`ο πίνακας ως μέρος της διαδικασίας ανάπτυξης:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Με αυτήν την προσέγγιση του κώδικα στο στοιχείο ελέγχου πηγαίου παραμένει στατικό και τις τιμές του διαμερισμού όριο μπορεί να είναι δυναμική; σε εξέλιξη με την αποθήκη μέσα στο χρόνο.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στα άρθρα σε [Πίνακα Επισκόπηση][Επισκόπηση], [Τύπους δεδομένων πίνακα][Τύπους δεδομένων], [τη διανομή πίνακα][κατανομή], [δημιουργίας ευρετηρίου για έναν πίνακα][ευρετηρίου], [Λαμβάνοντας υπόψη στατιστικά στοιχεία πίνακα][Στατιστικά στοιχεία] και [Προσωρινό πίνακες][προσωρινό].  Για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές, ανατρέξτε στο θέμα [Βέλτιστες πρακτικές αποθήκη δεδομένων SQL][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση]: ./sql-data-warehouse-tables-overview.md
[Τύποι δεδομένων]: ./sql-data-warehouse-tables-data-types.md
[Διανομή]: ./sql-data-warehouse-tables-distribute.md
[Ευρετήριο]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[Προσωρινό]: ./sql-data-warehouse-tables-temporary.md
[Διαχείριση φόρτου εργασίας]: ./sql-data-warehouse-develop-concurrency.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων SQL]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Διαμερίσματα πίνακες και ευρετήρια]: https://msdn.microsoft.com/library/ms190787.aspx
[ΤΡΟΠΟΠΟΊΗΣΗ ΤΟΥ ΠΊΝΑΚΑ]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ]: https://msdn.microsoft.com/library/mt203953.aspx
[συνάρτηση partition]: https://msdn.microsoft.com/library/ms187802.aspx
[διαμερίσματα συνδυασμού]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->

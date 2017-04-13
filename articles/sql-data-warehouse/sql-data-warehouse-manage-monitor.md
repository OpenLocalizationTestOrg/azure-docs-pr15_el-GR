<properties
   pageTitle="Παρακολούθηση του φόρτου εργασίας χρησιμοποιείτε DMVs | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να παρακολουθείτε τον φόρτο εργασίας με χρήση DMVs."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Παρακολούθηση του φόρτου εργασίας σας με χρήση DMVs

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης των δυναμικών προβολών διαχείρισης (DMVs) για την παρακολούθηση του φόρτου εργασίας σας και να διερευνήσουμε εκτέλεση ερωτήματος στο αποθήκη δεδομένων του SQL Azure.

## <a name="permissions"></a>Δικαιώματα

Ερώτημα για το DMVs σε αυτό το άρθρο, χρειάζεστε δικαιώματα ΚΑΤΆΣΤΑΣΗ ΠΡΟΒΟΛΉΣ βάσης ΔΕΔΟΜΈΝΩΝ ή στοιχείο ΕΛΈΓΧΟΥ. Συνήθως χορήγηση ΚΑΤΆΣΤΑΣΗ βάσης ΔΕΔΟΜΈΝΩΝ ΠΡΟΒΟΛΉΣ είναι την προτιμώμενη δικαιωμάτων, όπως είναι πιο περιοριστικό.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Οθόνη συνδέσεις

Είστε συνδεδεμένοι [sys.dm_pdw_exec_sessions][]όλες τις συνδέσεις για να αποθήκη δεδομένων του SQL.  Αυτό DMV περιέχει το τελευταίο 10.000 συνδέσεις.  Το session_id είναι το πρωτεύον κλειδί και έχουν ανατεθεί σε διαδοχικά για κάθε νέα σύνδεση.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Εκτέλεση ερωτήματος οθόνη

Όλα τα ερωτήματα που εκτελούνται σε αποθήκη δεδομένων του SQL καταγράφονται [sys.dm_pdw_exec_requests][].  Αυτό DMV περιέχει τα τελευταία 10.000 ερωτήματα που εκτελούνται.  Η request_id προσδιορίζει κάθε ερώτημα και είναι το πρωτεύον κλειδί για αυτό DMV.  Το request_id έχει εκχωρηθεί διαδοχικά για κάθε νέο ερώτημα και είναι το πρόθεμα QID, που σημαίνει αναγνωριστικό ερωτήματος.  Υποβολή ερωτημάτων αυτό DMV για μια δεδομένη session_id εμφανίζει όλα τα ερωτήματα για μια δεδομένη σύνδεσης.

>[AZURE.NOTE] Αποθηκευμένες διαδικασίες Χρησιμοποιήστε πολλαπλές ταυτότητες αίτηση.  Αίτηση αναγνωριστικά εκχωρούνται με διαδοχική σειρά. 

Εδώ θα βρείτε βήματα για να ακολουθήσετε για να διερευνήσουμε προγράμματα εκτέλεσης του ερωτήματος και ώρες για ένα συγκεκριμένο ερώτημα.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>ΒΉΜΑ 1: Προσδιορίστε το ερώτημα που θέλετε να ερευνήσετε

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Από τα αποτελέσματα του ερωτήματος προηγούμενα, **Σημειώστε το Αναγνωριστικό αίτηση** του ερωτήματος που θέλετε να ερευνήσετε.

Ερωτήματα σε κατάσταση **αναστολής** είναι που έχει τεθεί σε ουρά λόγω ταυτόχρονης εκτέλεσης όρια. Αυτά τα ερωτήματα εμφανίζονται επίσης στο ερώτημα αναμονή sys.dm_pdw_waits με τύπο UserConcurrencyResourceType. Για περισσότερες λεπτομέρειες σχετικά με τα όρια συγχρονισμού, ανατρέξτε στο θέμα [Διαχείριση ταυτόχρονης εκτέλεσης και φόρτο εργασίας][] . Ερωτήματα επίσης να περιμένετε για άλλους λόγους όπως για το κλείδωμα του αντικειμένου.  Εάν το ερώτημά σας είναι σε αναμονή για έναν πόρο, ανατρέξτε στο θέμα [Investigating ερωτήματα αναμονή για πόρους][] περαιτέρω προς τα κάτω σε αυτό το άρθρο.

Για να απλοποιήσετε την αναζήτηση ενός ερωτήματος στον πίνακα sys.dm_pdw_exec_requests, χρησιμοποιήστε την [ΕΤΙΚΈΤΑ][] για να εκχωρήσετε ένα σχόλιο για το ερώτημα που μπορεί να αναζητηθεί στην προβολή sys.dm_pdw_exec_requests.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>ΒΉΜΑ 2: Διερεύνηση στο πρόγραμμα ερωτήματος

Χρησιμοποιήστε το Αναγνωριστικό αίτηση για την ανάκτηση του ερωτήματος κατανέμεται πρόγραμμα SQL (DSQL) από [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Όταν ένα πρόγραμμα DSQL διαρκεί περισσότερο από το αναμενόμενο, η αιτία μπορεί να είναι μια σύνθετη πρόγραμμα με πολλά βήματα DSQL ή ένα μόνο βήμα διαρκεί πολύ χρόνο.  Εάν το πρόγραμμα είναι πολλά βήματα με τις διάφορες λειτουργίες μετακίνησης, εξετάστε το ενδεχόμενο βελτιστοποιώντας τον πίνακα κατανομές για να μειώσετε την κίνηση δεδομένων. Το άρθρο [πίνακα διανομής][] εξηγεί γιατί πρέπει να μετακινηθούν για την επίλυση ενός ερωτήματος δεδομένων και εξηγεί ορισμένες στρατηγικών διανομής για να ελαχιστοποιήσετε την κίνηση δεδομένων.

Για να διερευνήσουμε περαιτέρω λεπτομέρειες σχετικά με ένα βήμα προς βήμα, η στήλη *operation_type* του βήματος ερωτήματος μεγάλη διάρκεια εκτέλεσης και σημειώστε το **Ευρετήριο βήμα**:

- Συνεχίστε με το βήμα 3a για **Λειτουργίες SQL**: OnOperation, RemoteOperation, ReturnOperation.
- Συνεχίστε με το βήμα 3 β για **Λειτουργίες κίνηση δεδομένων**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>ΒΉΜΑ 3a: Διερεύνηση SQL σε την κατανεμημένη βάσεις δεδομένων

Χρησιμοποιήστε το Αναγνωριστικό αίτηση και το ευρετήριο βήμα για την ανάκτηση λεπτομερειών από [sys.dm_pdw_sql_requests][], που περιέχει πληροφορίες εκτέλεσης του βήματος ερωτήματος σε όλες τις βάσεις δεδομένων κατανέμεται.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Όταν εκτελείται το βήμα ερωτήματος, [DBCC PDW_SHOWEXECUTIONPLAN][] μπορεί να χρησιμοποιηθεί για την ανάκτηση του σχεδίου εκτιμώμενη SQL Server από το cache πρόγραμμα του SQL Server για το βήμα που εκτελείται σε μια συγκεκριμένη κατανομή.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>ΒΉΜΑ 3 β: Διερεύνηση κίνηση δεδομένων σε βάσεις δεδομένων με την κατανεμημένη

Χρησιμοποιήστε το Αναγνωριστικό αίτηση και το ευρετήριο βήμα για να ανακτήσετε πληροφορίες σχετικά με ένα βήμα κίνηση δεδομένων που εκτελείται σε κάθε διανομής από [sys.dm_pdw_dms_workers][].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Ελέγξτε τη στήλη *total_elapsed_time* για να δείτε εάν μια συγκεκριμένη κατανομή διαρκεί περισσότερο χρόνο από άλλους για τη μεταφορά δεδομένων.
- Για την κατανομή μεγάλη διάρκεια εκτέλεσης, ελέγξτε τη στήλη *rows_processed* για να δείτε εάν τον αριθμό των γραμμών που μετακινείται από διανομής που είναι πολύ μεγαλύτερο από άλλους χρήστες. Εάν Ναι, αυτό μπορεί να δηλώνει skew τα υποκείμενα δεδομένα.

Εάν το ερώτημα εκτελείται, [DBCC PDW_SHOWEXECUTIONPLAN][] μπορεί να χρησιμοποιηθεί για την ανάκτηση του σχεδίου εκτιμώμενη SQL Server από το cache πρόγραμμα του SQL Server για το βήμα εκτελούνται τη συγκεκριμένη στιγμή SQL μέσα σε μια συγκεκριμένη κατανομή.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Οθόνη αναμονή ερωτημάτων

Εάν ανακαλύψετε ότι το ερώτημά σας δεν πραγματοποιεί προόδου επειδή αυτό είναι σε αναμονή για έναν πόρο, ακολουθεί ένα ερώτημα που εμφανίζει όλους τους πόρους που είναι σε αναμονή για ένα ερώτημα.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Εάν το ερώτημα είναι ενεργά σε αναμονή για τους πόρους από ένα άλλο ερώτημα, στη συνέχεια, η κατάσταση θα είναι **AcquireResources**.  Εάν το ερώτημα περιέχει όλους τους απαραίτητους πόρους, στη συνέχεια, η κατάσταση θα είναι **Granted**.

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες σχετικά με την DMVs, ανατρέξτε στο θέμα [προβολές συστήματος][] .
Ανατρέξτε στο θέμα [βέλτιστες πρακτικές αποθήκη δεδομένων του SQL][] για περισσότερες πληροφορίες σχετικά με τις βέλτιστες πρακτικές

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Βέλτιστες πρακτικές αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-best-practices.md
[Προβολές συστήματος]: ./sql-data-warehouse-reference-tsql-system-views.md
[Πίνακας διανομής]: ./sql-data-warehouse-tables-distribute.md
[Διαχείριση συγχρονισμού και φόρτο εργασίας]: ./sql-data-warehouse-develop-concurrency.md
[Διερεύνηση ερωτήματα αναμονή για πόρους]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[ΕΤΙΚΈΤΑ]: https://msdn.microsoft.com/library/ms190322.aspx

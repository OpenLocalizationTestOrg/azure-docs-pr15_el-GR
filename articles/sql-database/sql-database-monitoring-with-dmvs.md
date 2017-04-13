<properties
   pageTitle="Παρακολούθηση βάση δεδομένων SQL Azure με δυναμική Διαχείριση προβολών | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να εντοπίσετε και να διάγνωση κοινά προβλήματα επιδόσεων χρησιμοποιώντας δυναμική Διαχείριση προβολών για την παρακολούθηση της βάσης δεδομένων SQL Microsoft Azure."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Παρακολούθηση βάση δεδομένων SQL Azure χρησιμοποιώντας δυναμική Διαχείριση προβολών

Βάση δεδομένων SQL Microsoft Azure ενεργοποιεί ένα υποσύνολο δυναμική Διαχείριση προβολών για τη διάγνωση προβλημάτων επιδόσεων, που μπορεί να προκληθούν από ανενεργή ή μεγάλη διάρκεια εκτέλεσης ερωτημάτων, συνωστισμό, σχέδια κακή ερωτήματος και ούτω καθεξής. Αυτό το θέμα παρέχει πληροφορίες σχετικά με τον τρόπο για να εντοπίζουν κοινά προβλήματα επιδόσεων χρησιμοποιώντας δυναμική Διαχείριση προβολών.

Βάση δεδομένων SQL μερικώς υποστηρίζει τρεις κατηγορίες δυναμική Διαχείριση προβολών:

- Βάση δεδομένων που σχετίζονται με δυναμική Διαχείριση προβολών.
- Οι προβολές που σχετίζονται με την εκτέλεση δυναμική διαχείριση.
- Οι προβολές που σχετίζονται με συναλλαγή δυναμική διαχείριση.

Για λεπτομερείς πληροφορίες σχετικά με δυναμική Διαχείριση προβολών, ανατρέξτε στο θέμα [δυναμικών προβολών διαχείρισης και συναρτήσεις (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) στο ηλεκτρονικά βιβλία του SQL Server.

## <a name="permissions"></a>Δικαιώματα

Σε βάση δεδομένων SQL, υποβολή ερωτημάτων σε μια προβολή δυναμική διαχείριση απαιτεί δικαιώματα **ΚΑΤΆΣΤΑΣΗ ΠΡΟΒΟΛΉΣ βάσης ΔΕΔΟΜΈΝΩΝ** . Το δικαίωμα **ΠΡΟΒΟΛΉΣ βάσης ΔΕΔΟΜΈΝΩΝ ΚΑΤΆΣΤΑΣΗ** επιστρέφει πληροφορίες σχετικά με όλα τα αντικείμενα μέσα στην τρέχουσα βάση δεδομένων.
Για να εκχωρήσετε το δικαίωμα **ΠΡΟΒΟΛΉΣ ΚΑΤΆΣΤΑΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** σε μια συγκεκριμένη βάση δεδομένων χρήστη, εκτελέστε το ακόλουθο ερώτημα:

```GRANT VIEW DATABASE STATE TO database_user; ```

Σε μια παρουσία του SQL Server εσωτερικής εγκατάστασης, Διαχείριση δυναμικών προβολών επιστρέψει πληροφορίες κατάστασης διακομιστή. Σε βάση δεδομένων SQL, μπορούν να επιστρέψουν πληροφορίες σχετικά με την τρέχουσα λογική βάση δεδομένων μόνο.

## <a name="calculating-database-size"></a>Τον υπολογισμό του μεγέθους της βάσης δεδομένων

Το παρακάτω ερώτημα επιστρέφει το μέγεθος της βάσης δεδομένων σας (σε megabyte):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Το παρακάτω ερώτημα επιστρέφει το μέγεθος των μεμονωμένων αντικειμένων (σε megabyte) στη βάση δεδομένων σας:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Παρακολούθηση συνδέσεων

Μπορείτε να χρησιμοποιήσετε την προβολή [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) για να ανακτήσετε πληροφορίες σχετικά με τις συνδέσεις που έχει δημιουργηθεί σε ένα συγκεκριμένο διακομιστή βάσης δεδομένων SQL Azure και τις λεπτομέρειες της κάθε σύνδεση. Επιπλέον, η προβολή [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) είναι χρήσιμη κατά την ανάκτηση πληροφορίες σχετικά με όλες τις συνδέσεις του ενεργού χρήστη και εσωτερικές εργασίες.
Το παρακάτω ερώτημα ανακτά πληροφορίες σχετικά με την τρέχουσα σύνδεση:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Κατά την εκτέλεση του **sys.dm_exec_requests** και **sys.dm_exec_sessions προβολές**, εάν έχετε δικαίωμα **ΠΡΟΒΟΛΉΣ ΚΑΤΆΣΤΑΣΗ βάσης ΔΕΔΟΜΈΝΩΝ** στη βάση δεδομένων, μπορείτε να δείτε όλα εκτέλεση περίοδοι λειτουργίας στη βάση δεδομένων. Διαφορετικά, μπορείτε να δείτε μόνο την τρέχουσα περίοδο λειτουργίας.

## <a name="monitoring-query-performance"></a>Παρακολούθηση των επιδόσεων του ερωτήματος

Αργή ή χρόνο εκτέλεσης ερωτημάτων μπορούν να εκμετάλλευση πόρους του συστήματος σημαντική. Αυτή η ενότητα παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε προβολές για τη δυναμική διαχείριση για να εντοπίσετε μερικά συνηθισμένα προβλήματα επιδόσεων ερωτήματος. Μια αναφορά παλαιότερων αλλά εξακολουθούν να είναι χρήσιμες για την αντιμετώπιση προβλημάτων, είναι στο άρθρο [Αντιμετώπιση προβλημάτων επιδόσεων στον SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) στο Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Μπορείτε να βρείτε επάνω Ν ερωτημάτων

Το παρακάτω παράδειγμα επιστρέφει πληροφορίες σχετικά με τα πιο δημοφιλή ερωτήματα πέντε κατάταξης με μέσος χρόνος CPU. Αυτό το παράδειγμα συγκεντρώνει τα ερωτήματα σύμφωνα με τους κατακερματισμός ερωτήματος, ώστε να λογικά ισοδύναμη ερωτήματα είναι ομαδοποιημένα κατά την κατανάλωση αθροιστική πόρων.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Παρακολούθηση αποκλεισμένων ερωτημάτων

Αργή ή μεγάλη διάρκεια εκτέλεσης ερωτημάτων μπορούν να συνεισφέρουν σε κατανάλωση πλεονάζουσα πόρων και να είναι η συνέπεια αποκλεισμένων ερωτημάτων. Η αιτία από τον αποκλεισμό μπορεί να είναι κακή εφαρμογή σχεδίασης, εσφαλμένες ερωτήματος προγράμματος, η έλλειψη χρήσιμες ευρετηρίων και ούτω καθεξής. Μπορείτε να χρησιμοποιήσετε την προβολή sys.dm_tran_locks για πληροφορίες σχετικά με την τρέχουσα δραστηριότητά κλειδώματος στη βάση δεδομένων SQL Azure. Για παράδειγμα κώδικα, ανατρέξτε στο θέμα [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) στο ηλεκτρονικά βιβλία του SQL Server.

### <a name="monitoring-query-plans"></a>Παρακολούθηση προγράμματος ερωτήματος

Επίσης, ένα σχέδιο αποτελεσματική ερωτήματος μπορεί να αυξήσουν κατανάλωση CPU. Το παρακάτω παράδειγμα χρησιμοποιεί την προβολή [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) για να προσδιορίσετε ποια query χρησιμοποιεί την πιο αθροιστική CPU.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Δείτε επίσης

[Εισαγωγή σε βάση δεδομένων SQL](sql-database-technical-overview.md)

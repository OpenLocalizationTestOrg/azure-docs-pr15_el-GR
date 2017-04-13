<properties 
    pageTitle="Δημιουργία ή να μετακινήσετε μια βάση δεδομένων Azure SQL σε ένα σύνολο ελαστικότητας χρησιμοποιώντας T-SQL | Microsoft Azure" 
    description="Χρησιμοποιήστε T-SQL για να δημιουργήσετε μια βάση δεδομένων Azure SQL σε ένα χώρο συγκέντρωσης ελαστικότητας. Ή χρησιμοποιήστε το για να μετακινήσετε το datbase και έξοδος από χώρους συγκέντρωσης Τ-SQL." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Παρακολούθηση και διαχείριση χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με Transact-SQL  

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Χρησιμοποιήστε τις εντολές [Δημιουργία βάσης δεδομένων (βάση δεδομένων SQL Azure)](https://msdn.microsoft.com/library/dn268335.aspx) και να [Τροποποιήσουν τις Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) για να δημιουργήσετε και να μετακινήσετε βάσεις δεδομένων σε και έξοδος από το ελαστικότητας χώρους συγκέντρωσης. Το χώρο συγκέντρωσης ελαστικά πρέπει να υπάρχει να μπορέσετε να χρησιμοποιήσετε αυτές τις εντολές. Οι εντολές αυτές επηρεάζουν μόνο βάσεις δεδομένων. Δημιουργία νέου συνόλων και τη ρύθμιση των ιδιοτήτων του χώρου συγκέντρωσης (όπως eDTUs min και max) δεν μπορεί να αλλάξει με εντολές T-SQL.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Δημιουργία μιας νέας βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας
Χρησιμοποιήστε την εντολή ΔΗΜΙΟΥΡΓΊΑ βάσης ΔΕΔΟΜΈΝΩΝ με την επιλογή SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Όλες τις βάσεις δεδομένων σε ένα σύνολο ελαστικότητας μεταβιβάζεται το επίπεδο υπηρεσιών του χώρου συγκέντρωσης ελαστικά (Basic, τυπική, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Μετακίνηση μιας βάσης δεδομένων μεταξύ ελαστικότητας συνόλων
Χρησιμοποιήστε την εντολή ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ με την ΤΡΟΠΟΠΟΊΗΣΗ και ρύθμιση ΥΠΗΡΕΣΊΑΣ\_ΣΤΌΧΟΥ επιλογή ως ΕΛΑΣΤΙΚΈΣ\_χώρο ΣΥΓΚΈΝΤΡΩΣΗΣ; Ορισμός του ονόματος στο όνομα του χώρου συγκέντρωσης προορισμού.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Μετακίνηση μιας βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας 
Χρησιμοποιήστε την εντολή ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ με την ΤΡΟΠΟΠΟΊΗΣΗ και ρύθμιση ΥΠΗΡΕΣΊΑΣ\_ΣΤΌΧΟΥ επιλογή ως ELASTIC_POOL; Ορισμός του ονόματος στο όνομα του χώρου συγκέντρωσης προορισμού.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Μετακίνηση μιας βάσης δεδομένων από ένα χώρο συγκέντρωσης ελαστικότητας
Χρησιμοποιήστε την εντολή ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ και ορίστε την SERVICE_OBJECTIVE σε ένα από τα επίπεδα επιδόσεων (S0 S1, κλπ).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Λίστα βάσεις δεδομένων σε ένα σύνολο ελαστικότητας
Χρησιμοποιήστε το [sys.database\_υπηρεσία \_προβολή των στόχων](https://msdn.microsoft.com/library/mt712619) για μια λίστα όλων των βάσεων δεδομένων σε ένα σύνολο ελαστικότητας. Συνδεθείτε στο την κύρια βάση δεδομένων για την προβολή του ερωτήματος.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Λήψη δεδομένων χρήσης πόρων για ένα χώρο συγκέντρωσης

Χρησιμοποιήστε το [sys.elastic\_χώρου συγκέντρωσης \_πόρων \_προβολή στατιστικές](https://msdn.microsoft.com/library/mt280062.aspx) να εξετάσετε τα στατιστικά στοιχεία χρήσης πόρων ενός χώρου συγκέντρωσης ελαστικότητας σε λογικές διακομιστή. Συνδεθείτε στο την κύρια βάση δεδομένων για την προβολή του ερωτήματος.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Λήψη "Χρήση πόρων" για μια βάση δεδομένων της ελαστικότητας

Χρησιμοποιήστε το [sys.dm\_ db\_ πόρων\_στατιστικές προβολή](https://msdn.microsoft.com/library/dn800981.aspx) ή [sys.resource \_προβολή στατιστικές](https://msdn.microsoft.com/library/dn269979.aspx) να εξετάσετε τα στατιστικά στοιχεία χρήσης πόρων μιας βάσης δεδομένων σε ένα σύνολο ελαστικότητας. Αυτή η διαδικασία είναι παρόμοια με την υποβολή ερωτημάτων χρήση πόρων για κάθε μία βάση δεδομένων.

## <a name="next-steps"></a>Επόμενα βήματα

Μετά τη δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων, μπορείτε να διαχειριστείτε ελαστικότητας βάσεων δεδομένων στο χώρο συγκέντρωσης, δημιουργώντας ελαστικότητας εργασίες. Ελαστικά εργασίες διευκολύνουν την εκτέλεση δεσμών ενεργειών Τ-SQL σε σχέση με οποιονδήποτε αριθμό βάσεων δεδομένων στο χώρο συγκέντρωσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση εργασιών ελαστικότητας βάσης δεδομένων](sql-database-elastic-jobs-overview.md). 

Ανατρέξτε στο θέμα [Κλιμάκωση ανάληψη με βάση δεδομένων SQL Azure](sql-database-elastic-scale-introduction.md): Χρησιμοποιήστε εργαλεία ελαστικότητας βάσης δεδομένων για να κλιμάκωσης, μετακίνηση δεδομένων, ερώτημα ή δημιουργήστε συναλλαγές.

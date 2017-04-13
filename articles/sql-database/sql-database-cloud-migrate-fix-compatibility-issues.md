<properties
   pageTitle="Επιδιόρθωση ζητημάτων συμβατότητας βάσης δεδομένων SQL Server πριν από την μετεγκατάσταση σε βάση δεδομένων SQL | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, συμβατότητα, Οδηγός μετεγκατάστασης του SQL Azure"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>Χρήση οδηγού μετεγκατάστασης Azure SQL για ζητήματα συμβατότητας του επιδιόρθωση SQL Server βάση δεδομένων πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure

> [AZURE.SELECTOR]
- Χρήση του [Οδηγού μετεγκατάστασης Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Χρήση [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Χρήση [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Σε αυτό το άρθρο, θα μάθετε για να εντοπίσετε και να διορθώσετε προβλήματα συμβατότητας βάσης δεδομένων SQL Server με χρήση του οδηγού μετεγκατάστασης Azure SQL πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure.

## <a name="using-sql-azure-migration-wizard"></a>Χρήση του οδηγού μετεγκατάστασης Azure SQL

Χρησιμοποιήστε το εργαλείο CodePlex [Οδηγός μετεγκατάστασης Azure SQL](http://sqlazuremw.codeplex.com/) για να δημιουργήσετε μια δέσμη ενεργειών T-SQL από μια βάση δεδομένων συμβατή προέλευσης. Αυτή η δέσμη ενεργειών μετατρέπεται, στη συνέχεια, από τον οδηγό για να είναι συμβατή με τη βάση δεδομένων SQL. Μπορείτε, στη συνέχεια, συνδεθείτε με βάση δεδομένων SQL Azure να εκτελέσει τη δέσμη ενεργειών. Αυτό το εργαλείο αναλύει επίσης αρχεία παρακολούθησης για να προσδιορίσετε ζητήματα συμβατότητας. Η δέσμη ενεργειών είναι δυνατό να δημιουργηθούν με σχήμα μόνο ή μπορεί να περιλαμβάνουν δεδομένα σε μορφή BCP. Επιπλέον τεκμηρίωση, συμπεριλαμβανομένων των αναλυτικές οδηγίες είναι διαθέσιμη στο CodePlex στο [Οδηγός μετεγκατάστασης του SQL Azure](http://sqlazuremw.codeplex.com/).  

 ![Διάγραμμα SAMW μετεγκατάστασης](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Δεν είναι όλες συμβατό σχήματος που εντοπίζονται από τον Οδηγό μπορούν να διορθωθούν από τα ενσωματωμένα μετασχηματισμοί. Συμβατό δέσμη ενεργειών που δεν είναι δυνατό να απευθύνεται αναφέρονται ως σφάλματα, με τα σχόλια που έχουν εισαχθεί σε τη δέσμη ενεργειών που δημιουργήθηκε. Εάν εντοπιστούν πολλά σφάλματα, χρησιμοποιήστε το Visual Studio ή SQL Server Management Studio για να βήμα προς βήμα και να διορθώσετε κάθε λάθος που δεν ήταν δυνατό να διορθωθεί χρησιμοποιώντας τον Οδηγό μετεγκατάστασης του SQL Server.

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Μετεγκατάσταση συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

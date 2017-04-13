<properties
   pageTitle="Επιδιόρθωση ζητημάτων συμβατότητας βάσης δεδομένων SQL Server με χρήση του SQL Server διαχείρισης Studio πριν από την μετεγκατάσταση σε βάση δεδομένων SQL | Microsoft Azure"
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

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>Επιδιόρθωση ζητημάτων συμβατότητας βάσης δεδομένων SQL Server με χρήση SQL Server Management Studio πριν από την μετεγκατάσταση σε βάση δεδομένων SQL

> [AZURE.SELECTOR]
- Χρήση του [Οδηγού μετεγκατάστασης Azure SQL](sql-database-cloud-migrate-fix-compatibility-issues.md)
- Χρήση [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Χρήση [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Έμπειρους χρήστες να διορθώσετε προβλήματα συμβατότητας βάσης δεδομένων SQL Server με χρήση του SQL Server Management Studio πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure.


> [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>Χρήση του SQL Server Management Studio

Χρήση του SQL Server Management Studio για να διορθώσετε προβλήματα συμβατότητας, χρησιμοποιώντας διάφορες εντολές Transact-SQL, όπως **ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ**. Αυτή η μέθοδος είναι κυρίως για προχωρημένους χρήστες που είναι εξοικειωμένοι εργάζεστε Transact-SQL ζωντανή βάσης δεδομένων. Διαφορετικά, συνιστάται να χρησιμοποιήσετε SSDT. 



## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Μετεγκατάσταση συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

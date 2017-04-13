<properties
   pageTitle="Καθορισμός συμβατότητας της βάσης δεδομένων SQL χρησιμοποιώντας SqlPackage.exe | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, βάση δεδομένων SQL συμβατότητα, SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>Καθορισμός συμβατότητας της βάσης δεδομένων SQL χρησιμοποιώντας SqlPackage.exe

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Σύμβουλος αναβάθμισης](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Σε αυτό το άρθρο, θα μάθετε για να προσδιορίσετε εάν μια βάση δεδομένων SQL Server είναι συμβατά για τη μετεγκατάσταση με βάση δεδομένων SQL χρησιμοποιώντας το βοηθητικό πρόγραμμα εντολών [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) .

## <a name="using-sqlpackageexe"></a>Χρήση SqlPackage.exe

1. Ανοίξτε μια γραμμή εντολών και αλλάξτε έναν κατάλογο που περιέχει την πιο πρόσφατη έκδοση του sqlpackage.exe. Αυτό το βοηθητικό πρόγραμμα διατίθεται με τις πιο πρόσφατες εκδόσεις του [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) και [SQL Server Data Tools για το Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), ή μπορείτε να κάνετε λήψη την πιο πρόσφατη έκδοση του [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) απευθείας από το Κέντρο λήψης της Microsoft.
2. Εκτελέστε την ακόλουθη εντολή SqlPackage με τα ακόλουθα ορίσματα για το περιβάλλον σας:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < όνομα_διακομιστή >: < όνομα_βάσης_δεδομένων > /tf: /p:TableData < target_file > = < schema_name.table_name >>< Αρχείο_εξόδου > 2 > & 1'

  	| Το όρισμα  | Περιγραφή  |
  	|---|---|
  	| < όνομα_διακομιστή >  | όνομα διακομιστή προέλευσης  |
  	| < όνομα_βάσης_δεδομένων >  | όνομα βάσης δεδομένων προέλευσης  |
  	| < target_file >  | όνομα αρχείου και θέση για το αρχείο BACPAC  |
  	| < schema_name.table_name >  | Οι πίνακες για το οποίο υπάρχουν στο αρχείο προορισμού  |
  	| < Αρχείο_εξόδου >  | το όνομα του αρχείου και τη θέση για το αρχείο εξόδου με σφάλματα, εάν υπάρχει  |

    Ο λόγος για το όρισμα /p:TableName είναι ότι θέλουμε μόνο για να ελέγξετε για συμβατότητα βάσης δεδομένων για εξαγωγή σε Azure SQL DB V12, αντί να εξαγάγετε τα δεδομένα από όλους τους πίνακες. Δυστυχώς, το όρισμα εξαγωγής για sqlpackage.exe δεν υποστηρίζει την εξαγωγή μηδέν πίνακες. Πρέπει να καθορίσετε τουλάχιστον έναν πίνακα, όπως έναν μικρό πίνακα. < Αρχείο_εξόδου > περιέχει την αναφορά των σφαλμάτων. Το "> 2 > & 1" συμβολοσειρά σωλήνες στην τυπική έξοδο και το τυπικό σφάλμα που προκύπτει από την εκτέλεση της εντολής για να το καθορισμένο αρχείο εξόδου.

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Ανοίξτε το αρχείο εξόδου και δείτε τα σφάλματα συμβατότητας, εάν υπάρχει. 

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Επιδιόρθωση ζητημάτων συμβατότητας μετεγκατάσταση της βάσης δεδομένων](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Μετεγκατάσταση συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

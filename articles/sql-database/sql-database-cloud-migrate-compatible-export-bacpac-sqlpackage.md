<properties
   pageTitle="Εξαγωγή μιας βάσης δεδομένων SQL Server σε ένα αρχείο BACPAC χρησιμοποιώντας SqlPackage | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, εξαγωγή βάσης δεδομένων, εξαγωγή αρχείου BACPAC, sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>Εξαγωγή μιας βάσης δεδομένων SQL Server σε ένα αρχείο BACPAC χρησιμοποιώντας SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Αυτό το άρθρο παρουσιάζει τον τρόπο εξαγωγής μια βάση δεδομένων SQL Server σε ένα αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) χρησιμοποιώντας το βοηθητικό πρόγραμμα γραμμής εντολών [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Αυτό το βοηθητικό πρόγραμμα διατίθεται με τις πιο πρόσφατες εκδόσεις του [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) και [SQL Server Data Tools για το Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), ή μπορείτε να κάνετε λήψη την πιο πρόσφατη έκδοση του [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) απευθείας από το Κέντρο λήψης της Microsoft.

1. Ανοίξτε μια γραμμή εντολών και να αλλάξετε έναν κατάλογο που περιέχει το βοηθητικό πρόγραμμα γραμμής εντολών sqlpackage.exe - αυτό το βοηθητικό πρόγραμμα διατίθεται με το Visual Studio και SQL Server. Χρησιμοποιήστε την αναζήτηση στον υπολογιστή σας για να βρείτε τη διαδρομή στο περιβάλλον σας.
2. Εκτελέστε την ακόλουθη εντολή sqlpackage.exe με τα ακόλουθα ορίσματα για το περιβάλλον σας:

    ' sqlpackage.exe /Action:Export /ssn: /sdn < όνομα_διακομιστή >: < όνομα_βάσης_δεδομένων > /tf: < target_file >

  	| Το όρισμα  | Περιγραφή  |
  	|---|---|
  	| < όνομα_διακομιστή >  | όνομα διακομιστή προέλευσης  |
  	| < όνομα_βάσης_δεδομένων >  | όνομα βάσης δεδομένων προέλευσης  |
  	| < target_file >  | όνομα αρχείου και θέση για το αρχείο BACPAC  |

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Εισαγωγή ενός BACPAC σε βάση δεδομένων SQL Azure χρησιμοποιώντας SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Εισαγωγή ενός BACPAC στο SqlPackage βάσης δεδομένων Azure SQL](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Εισαγωγή ενός BACPAC πύλη Azure βάσης δεδομένων SQL Azure](sql-database-import.md)
- [Εισαγωγή ενός BACPAC με το PowerShell βάσης δεδομένων Azure SQL](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

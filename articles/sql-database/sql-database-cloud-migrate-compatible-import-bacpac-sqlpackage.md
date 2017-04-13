<properties
   pageTitle="Εισαγωγή σε βάση δεδομένων SQL από ένα αρχείο BACPAC χρησιμοποιώντας SqlPackage"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, Εισαγωγή βάσης δεδομένων, εισαγάγετε το αρχείο BACPAC, sqlpackage"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>Εισαγωγή σε βάση δεδομένων SQL από ένα αρχείο BACPAC χρησιμοποιώντας SqlPackage

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Πύλη του Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

Σε αυτό το άρθρο παρουσιάζει τον τρόπο εισαγωγής με βάση δεδομένων SQL από ένα αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) χρησιμοποιώντας το βοηθητικό πρόγραμμα γραμμής εντολών [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) . Αυτό το βοηθητικό πρόγραμμα διατίθεται με τις πιο πρόσφατες εκδόσεις του [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) και [SQL Server Data Tools για το Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx), ή μπορείτε να κάνετε λήψη την πιο πρόσφατη έκδοση του [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) απευθείας από το Κέντρο λήψης της Microsoft.


> [AZURE.NOTE] Τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχουν παρασχεθεί ήδη μια βάση δεδομένων SQL server, έχετε στη διάθεσή σας τις πληροφορίες σύνδεσης και έχετε επαληθεύσει ότι τη βάση δεδομένων προέλευσης είναι συμβατή.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Εισαγωγή από αρχείο BACPAC σε βάση δεδομένων SQL Azure χρησιμοποιώντας SqlPackage

Χρησιμοποιήστε τα ακόλουθα βήματα για να χρησιμοποιήσετε το βοηθητικό πρόγραμμα γραμμής εντολών [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) για να εισαγάγετε ένα συμβατό βάση δεδομένων SQL Server (ή βάση δεδομένων Azure SQL) από ένα αρχείο BACPAC.

> [AZURE.NOTE] Ακολουθήστε τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε ήδη παρασχεθεί ένα διακομιστή βάσης δεδομένων SQL Azure και έχετε στη διάθεσή σας τις πληροφορίες σύνδεσης.

1. Ανοίξτε μια γραμμή εντολών και να αλλάξετε έναν κατάλογο που περιέχει το βοηθητικό πρόγραμμα γραμμής εντολών sqlpackage.exe - αυτό το βοηθητικό πρόγραμμα διατίθεται με το Visual Studio και SQL Server.
2. Εκτελέστε την ακόλουθη εντολή sqlpackage.exe με τα ακόλουθα ορίσματα για το περιβάλλον σας:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Το όρισμα  | Περιγραφή  |
  	|---|---|
  	| < όνομα_διακομιστή >  | όνομα διακομιστή προορισμού  |
  	| < όνομα_βάσης_δεδομένων >  | όνομα βάσης δεδομένων προορισμού  |
  	| < όνομα_χρήστη >  | το όνομα χρήστη στο διακομιστή προορισμού |
  	| < τον κωδικό πρόσβασης >  | τον κωδικό πρόσβασης του χρήστη  |
  	| < source_file >  | το όνομα αρχείου και θέση για το αρχείο BACPAC που εισάγεται  |

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

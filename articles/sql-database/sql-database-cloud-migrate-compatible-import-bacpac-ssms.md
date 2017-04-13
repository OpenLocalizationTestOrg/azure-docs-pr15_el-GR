<properties
   pageTitle="Μετεγκατάσταση μιας βάσης δεδομένων SQL Server σε βάση δεδομένων SQL Azure | Microsoft Azure"
   description="Βάση δεδομένων SQL του Microsoft Azure, ανάπτυξη βάσης δεδομένων, μετεγκατάσταση της βάσης δεδομένων, Εισαγωγή βάσης δεδομένων, εξαγωγή βάσης δεδομένων, Οδηγός μετεγκατάστασης"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Εισαγωγή από BACPAC στη βάση δεδομένων SQL χρησιμοποιώντας SSMS

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Πύλη του Azure](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)

Σε αυτό το άρθρο δείχνει πώς μπορείτε να εισαγάγετε από ένα αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) στη βάση δεδομένων SQL χρησιμοποιώντας την εξαγωγή επίπεδο εφαρμογής Οδηγό δεδομένων στο SQL Server Management Studio.

> [AZURE.NOTE] Ακολουθήστε τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε ήδη παρασχεθεί σας λογική παρουσία του Azure SQL και έχετε στη διάθεσή σας τις πληροφορίες σύνδεσης.

1. Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του SQL Server Management Studio. Οι νέες εκδόσεις Management Studio ενημερώνονται μηνιαία να παραμένουν συγχρονισμένοι με ενημερώσεις για την πύλη του Azure.

     > [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Σύνδεση με το διακομιστή βάσης δεδομένων SQL Azure, κάντε δεξί κλικ στο φάκελο **βάσεων δεδομένων** και κάντε κλικ στην επιλογή **Εισαγωγή δεδομένων επιπέδων εφαρμογή...**

    ![Εισαγωγή στοιχείου μενού εφαρμογής επιπέδων δεδομένων](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Για να δημιουργήσετε τη βάση δεδομένων σε βάση δεδομένων SQL Azure, εισαγάγετε ένα αρχείο BACPAC από τον τοπικό δίσκο ή επιλέξτε ο λογαριασμός Azure χώρου αποθήκευσης και κοντέινερ στο οποίο αποστείλατε το αρχείο BACPAC.

    ![Εισαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Κατά την εισαγωγή ενός BACPAC από χώρο αποθήκευσης αντικειμένων blob του Azure, χρησιμοποιήστε τυπική χώρου αποθήκευσης. Εισαγωγή ενός BACPAC από χώρο αποθήκευσης premium δεν υποστηρίζεται.

4.  Δώστε στο **νέο όνομα βάσης δεδομένων** για τη βάση δεδομένων σε Azure SQL DB, ορίστε την **Έκδοση του Microsoft βάση δεδομένων SQL Azure** (επίπεδο υπηρεσιών) **μέγιστο μέγεθος της βάσης δεδομένων**και **Υπηρεσία στόχο** (επίπεδο επιδόσεων).

    ![Ρυθμίσεις της βάσης δεδομένων](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Κάντε κλικ στο κουμπί **Επόμενο** και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος** για να εισαγάγετε το αρχείο BACPAC σε μια νέα βάση δεδομένων σε διακομιστή βάσης δεδομένων SQL Azure.

6. Με την Εξερεύνηση των αντικειμένων, συνδεθείτε στη βάση δεδομένων σας μετεγκαταστάθηκαν στο διακομιστή βάσης δεδομένων SQL Azure.

6.  Με την πύλη Azure, προβάλετε τη βάση δεδομένων και τις ιδιότητές του.

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

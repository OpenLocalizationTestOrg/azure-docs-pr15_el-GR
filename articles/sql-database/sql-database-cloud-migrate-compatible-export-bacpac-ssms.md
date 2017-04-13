
<properties
   pageTitle="Εξαγωγή μιας βάσης δεδομένων SQL Server σε ένα αρχείο BACPAC χρησιμοποιώντας το SQL Server Management Studio | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, εξαγωγή βάσης δεδομένων, εξαγωγή αρχείου BACPAC, Οδηγός εξαγωγής εφαρμογών σειρά δεδομένων"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>Εξαγωγή μιας βάσης δεδομένων SQL Server σε ένα αρχείο BACPAC χρησιμοποιώντας το SQL Server Management Studio

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Σε αυτό το άρθρο παρουσιάζει τον τρόπο εξαγωγής μια βάση δεδομένων SQL Server σε ένα αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) χρησιμοποιώντας τα δεδομένα επίπεδο εφαρμογής Οδηγός εξαγωγής στο SQL Server Management Studio. 

1. Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του SQL Server Management Studio. Οι νέες εκδόσεις Management Studio ενημερώνονται μηνιαία να παραμένουν συγχρονισμένοι με ενημερώσεις για την πύλη του Azure.

     > [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Ανοίξτε το Management Studio και συνδεθείτε με τη βάση δεδομένων προέλευσης στην Εξερεύνηση των αντικειμένων.

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Κάντε δεξί κλικ τη βάση δεδομένων προέλευσης στην Εξερεύνηση του αντικειμένου, τοποθετήστε το δείκτη σε **εργασίες**και κάντε κλικ στην επιλογή **Εξαγωγή δεδομένων επιπέδων εφαρμογή...**

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Στον Οδηγό εξαγωγής, ρυθμίστε τις παραμέτρους της εξαγωγή για να αποθηκεύσετε το αρχείο BACPAC είτε σε μια θέση στον τοπικό δίσκο ή σε μια Azure blob. Το εξαγόμενο BACPAC περιλαμβάνει πάντα του σχήματος ολοκλήρωσης βάσης δεδομένων και, από προεπιλογή, τα δεδομένα από όλους τους πίνακες. Χρησιμοποιήστε την καρτέλα για προχωρημένους, εάν θέλετε να εξαιρέσετε δεδομένων από ορισμένων ή όλων των πινάκων. Για παράδειγμα, μπορείτε να εξαγάγετε μόνο τα δεδομένα για αναφορά πίνακες και όχι από όλους τους πίνακες.

***Σημαντικό*** Κατά την εξαγωγή ενός BACPAC με το χώρο αποθήκευσης αντικειμένων blob του Azure, χρησιμοποιήστε τυπική χώρου αποθήκευσης. Εισαγωγή ενός BACPAC από χώρο αποθήκευσης premium δεν υποστηρίζεται.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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

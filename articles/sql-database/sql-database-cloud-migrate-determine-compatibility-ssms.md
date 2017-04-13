<properties
   pageTitle="Χρήση SQL Server Management Studio για τον καθορισμό της βάσης δεδομένων SQL συμβατότητας πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, βάση δεδομένων SQL συμβατότητα, εξαγωγή δεδομένων επίπεδο Οδηγός εφαρμογών"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>Χρήση SQL Server Management Studio για τον καθορισμό της βάσης δεδομένων SQL συμβατότητας πριν από την μετεγκατάσταση σε βάση δεδομένων SQL Azure

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Σύμβουλος αναβάθμισης](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
Σε αυτό το άρθρο θα μάθετε για να προσδιορίσετε εάν μια βάση δεδομένων SQL Server είναι συμβατά για τη μετεγκατάσταση με βάση δεδομένων SQL χρησιμοποιώντας την εξαγωγή επίπεδο εφαρμογής Οδηγό δεδομένων στο SQL Server Management Studio.

## <a name="using-sql-server-management-studio"></a>Χρήση του SQL Server Management Studio

1. Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του SQL Server Management Studio. Οι νέες εκδόσεις Management Studio ενημερώνονται μηνιαία να παραμείνει σε συγχρονισμό με ενημερώσεις στην πύλη του Azure.

     > [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Ανοίξτε το Management Studio και συνδεθείτε με τη βάση δεδομένων προέλευσης στην Εξερεύνηση των αντικειμένων.
3. Κάντε δεξί κλικ τη βάση δεδομένων προέλευσης στην Εξερεύνηση του αντικειμένου, τοποθετήστε το δείκτη σε **εργασίες**και κάντε κλικ στην επιλογή **Εξαγωγή δεδομένων επιπέδων εφαρμογή...**

    ![Εξαγωγή μιας εφαρμογής επιπέδων δεδομένων από το μενού "εργασίες"](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Στον Οδηγό εξαγωγής, κάντε κλικ στο κουμπί **Επόμενο**και, στη συνέχεια, στην καρτέλα **Ρυθμίσεις** , ρύθμιση παραμέτρων την εξαγωγή για να αποθηκεύσετε το αρχείο BACPAC από μια θέση στον τοπικό δίσκο ή μια αντικειμένων blob του Azure. Αποθήκευση ενός αρχείου BACPAC εάν έχετε ζητήματα συμβατότητας βάσης δεδομένων. Εάν υπάρχουν προβλήματα συμβατότητας, να εμφανίζονται στην κονσόλα.

    ![Εξαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Για να παραλείψετε εξαγωγή δεδομένων, κάντε κλικ στην **καρτέλα για προχωρημένους** και καταργήστε την επιλογή από το πλαίσιο ελέγχου **Επιλογή όλων** . Σε αυτό το σημείο, στόχος μας είναι μόνο για να ελέγξετε για συμβατότητα.

    ![Εξαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Κάντε κλικ στο κουμπί **Επόμενο** και, στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**. Ζητήματα συμβατότητας βάσης δεδομένων, αν υπάρχουν, εμφανίζονται αφού ο οδηγός επικυρώσει το σχήμα.

    ![Εξαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Εάν εμφανίζεται χωρίς σφάλματα, τη βάση δεδομένων είναι συμβατή και είστε έτοιμοι για τη μετεγκατάσταση. Εάν έχετε σφάλματα, πρέπει να τα διορθώσετε. Για να δείτε τα σφάλματα, κάντε κλικ στο κουμπί **σφάλματος** για **Validating σχήμα**. 
    ![Εξαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Εάν το *. Αρχείο BACPAC δημιουργείται με επιτυχία, στη συνέχεια, τη βάση δεδομένων είναι συμβατή με βάση δεδομένων SQL και είστε έτοιμοι για τη μετεγκατάσταση.

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Επιδιόρθωση ζητημάτων συμβατότητας μετεγκατάσταση της βάσης δεδομένων](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Μετεγκατάσταση συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

<properties
   pageTitle="Μετεγκατάσταση βάσης δεδομένων SQL Server με βάση δεδομένων SQL που χρησιμοποιεί ανάπτυξη βάση δεδομένων για να Οδηγός βάσεων δεδομένων Microsoft Azure | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, Οδηγός βάσεων δεδομένων Microsoft Azure"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>Μετεγκατάσταση βάσης δεδομένων SQL Server με βάση δεδομένων SQL που χρησιμοποιεί ανάπτυξη βάση δεδομένων για να Οδηγός βάσεων δεδομένων Microsoft Azure


> [AZURE.SELECTOR]
- [Οδηγός μετεγκατάστασης SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Εξαγωγή σε αρχείο BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Εισαγωγή από αρχείο BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Αναπαραγωγή συναλλαγών](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Η βάση δεδομένων ανάπτυξη Οδηγό βάση δεδομένων Microsoft Azure στο SQL Server Management Studio μετεγκαθιστά ένα [συμβατό βάση δεδομένων SQL Server](sql-database-cloud-migrate.md) απευθείας στο διακομιστή βάσης δεδομένων SQL Azure.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Χρήση της βάσης δεδομένων ανάπτυξη Οδηγός βάσεων δεδομένων Microsoft Azure

> [AZURE.NOTE] Ακολουθήστε τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε μια [βάση δεδομένων SQL server παρασχεθεί](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Βεβαιωθείτε ότι έχετε εγκαταστήσει την πιο πρόσφατη έκδοση του SQL Server Management Studio. Οι νέες εκδόσεις Management Studio ενημερώνονται μηνιαία να παραμείνει σε συγχρονισμό με ενημερώσεις στην πύλη του Azure.

    > [AZURE.IMPORTANT] Συνιστάται να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση του Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

2. Ανοίξτε το Management Studio και συνδεθείτε με τη βάση δεδομένων SQL Server για να μετεγκαταστήσετε στην Εξερεύνηση των αντικειμένων.
3. Κάντε δεξί κλικ της βάσης δεδομένων στην Εξερεύνηση του αντικειμένου, τοποθετήστε το δείκτη σε **εργασίες**και κάντε κλικ στην επιλογή **Ανάπτυξη βάση δεδομένων Microsoft Azure βάση δεδομένων SQL...**

    ![Ανάπτυξη Azure από το μενού "εργασίες"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  Στον Οδηγό ανάπτυξης, κάντε κλικ στο κουμπί **Επόμενο**και, στη συνέχεια, κάντε κλικ στην επιλογή **σύνδεση** για να ρυθμίσετε τη σύνδεση με το διακομιστή βάσης δεδομένων SQL.

    ![Ανάπτυξη Azure από το μενού "εργασίες"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Στη σύνδεση στο παράθυρο διαλόγου Server, πληκτρολογήστε τις πληροφορίες σύνδεσης για να συνδεθείτε με το διακομιστή βάσης δεδομένων SQL.

    ![Ανάπτυξη Azure από το μενού "εργασίες"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Δώστε τα εξής για το αρχείο [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) που αυτός ο οδηγός δημιουργεί κατά τη διάρκεια της διαδικασίας μετεγκατάστασης:

 - Το **νέο όνομα βάσης δεδομένων** 
 - Η **Έκδοση του Microsoft βάση δεδομένων SQL Azure** ([επίπεδο υπηρεσιών](sql-database-service-tiers.md))
 - Το **μέγιστο μέγεθος της βάσης δεδομένων**
 - Ο **Στόχος υπηρεσίας** (επίπεδο επιδόσεων)
 - Το **προσωρινό όνομα αρχείου**  

    ![Εξαγωγή ρυθμίσεων](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Ολοκλήρωση του οδηγού. Ανάλογα με το μέγεθος και την πολυπλοκότητα της βάσης δεδομένων, ενδέχεται να χρειαστούν ανάπτυξης από μερικά λεπτά σε πολλές ώρες. Εάν αυτός ο οδηγός εντοπίσει ζητήματα συμβατότητας, τα σφάλματα εμφανίζονται στην οθόνη και συνεχίσετε τη μετεγκατάσταση. Για οδηγίες σχετικά με τον τρόπο για να διορθώσετε προβλήματα συμβατότητας βάσης δεδομένων, μεταβείτε στην [επιδιόρθωση ζητημάτων συμβατότητας βάσης δεδομένων](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Με την Εξερεύνηση των αντικειμένων, συνδεθείτε στη βάση δεδομένων σας μετεγκαταστάθηκαν στο διακομιστή βάσης δεδομένων SQL Azure.
8.  Με την πύλη Azure, προβάλετε τη βάση δεδομένων και τις ιδιότητές του.

## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

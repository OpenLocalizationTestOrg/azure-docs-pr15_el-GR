<properties
   pageTitle="Μετεγκατάσταση σε βάση δεδομένων SQL που χρησιμοποιείται η αναπαραγωγή συναλλαγών | Microsoft Azure"
   description="Azure βάση δεδομένων Microsoft SQL, μετεγκατάσταση της βάσης δεδομένων, Εισαγωγή βάσης δεδομένων, αναπαραγωγή συναλλαγών"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>Μετεγκατάσταση βάσης δεδομένων SQL Server με βάση δεδομένων SQL Azure χρησιμοποιείται η αναπαραγωγή συναλλαγών

> [AZURE.SELECTOR]
- [Οδηγός μετεγκατάστασης SSMS](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [Εξαγωγή σε αρχείο BACPAC](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [Εισαγωγή από αρχείο BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Αναπαραγωγή συναλλαγών](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Σε αυτό το άρθρο, θα μάθετε για τη μετεγκατάσταση μιας συμβατά βάσης δεδομένων SQL Server με βάση δεδομένων SQL Azure με ελάχιστο χρόνο εκτός λειτουργίας χρησιμοποιείται η αναπαραγωγή συναλλαγών του SQL Server.

## <a name="understanding-the-transactional-replication-architecture"></a>Κατανόηση της αρχιτεκτονικής αναπαραγωγή συναλλαγών

Όταν δεν πρέπει να καταργήσετε τη βάση δεδομένων SQL Server από παραγωγής ενώ εκτελείται η μετεγκατάσταση, μπορείτε να χρησιμοποιήσετε αναπαραγωγή συναλλαγών του SQL Server ως λύση σας μετεγκατάστασης. Για να χρησιμοποιήσετε αυτήν τη λύση, μπορείτε να ρυθμίσετε τη βάση δεδομένων SQL Azure ως συνδρομητής για να την παρουσία του SQL Server εσωτερικής εγκατάστασης που θέλετε να μετεγκαταστήσετε. Το εσωτερικής διανομέας αναπαραγωγή συναλλαγών συγχρονίζει δεδομένα από τη βάση δεδομένων εσωτερικής εγκατάστασης να συγχρονιστούν (τον εκδότη) ενώ νέες συναλλαγές συνεχίζουν προκύψει. 

Μπορείτε επίσης να χρησιμοποιήσετε αναπαραγωγή συναλλαγών για να μετεγκαταστήσετε ένα υποσύνολο της βάσης δεδομένων εσωτερικής εγκατάστασης. Τη δημοσίευση που κάνετε αναπαραγωγή σε βάση δεδομένων SQL Azure μπορεί να περιορίζεται σε ένα υποσύνολο των πινάκων της βάσης δεδομένων γίνεται αναπαραγωγή. Για κάθε πίνακα γίνεται αναπαραγωγή, μπορείτε να περιορίσετε τα δεδομένα σε ένα υποσύνολο των γραμμών ή/και ένα υποσύνολο των στηλών.

Με αναπαραγωγή συναλλαγών, όλες οι αλλαγές στα δεδομένα ή σχήματος εμφανίζονται στη βάση δεδομένων SQL Azure. Όταν ολοκληρωθεί ο συγχρονισμός και είστε έτοιμοι για τη μετεγκατάσταση, αλλάξτε τη συμβολοσειρά σύνδεσης με τις εφαρμογές σας ώστε να οδηγεί στη βάση δεδομένων SQL Azure τους. Μόλις αναπαραγωγή συναλλαγών εξέρχεται αλλαγές προς τα αριστερά στη βάση δεδομένων εσωτερικής εγκατάστασης και όλες τις εφαρμογές σας οδηγεί Azure DB, μπορείτε να την καταργήσετε αναπαραγωγή συναλλαγών. Βάση δεδομένων SQL Azure είναι τώρα το σύστημα παραγωγής σας.

 ![Διάγραμμα SeedCloudTR](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Απαιτήσεις συναλλαγών αναπαραγωγής

Αναπαραγωγή συναλλαγών είναι μια τεχνολογία ενσωματωμένα και ενσωματωμένη με τον SQL Server από SQL Server 6.5. Είναι μια τεχνολογία ώριμη και αποδεδειγμένη ότι περισσότερα DBAs γνωρίζουν με τα οποία έχουν εμπειρία. Με το [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server), είναι τώρα δυνατή για να ρυθμίσετε τη βάση δεδομένων SQL Azure ως [συνδρομητών αναπαραγωγή συναλλαγών](https://msdn.microsoft.com/library/mt589530.aspx) στη δημοσίευσή σας εσωτερικής εγκατάστασης. Η εμπειρία που μπορείτε να την εγκατάσταση από Management Studio είναι η ίδια όπως εάν ρυθμίσετε συνδρομητής αναπαραγωγή συναλλαγών σε διακομιστή εσωτερικής εγκατάστασης. Υποστήριξη για αυτό το σενάριο υποστηρίζεται όταν τον εκδότη και ο διανομέας είναι τουλάχιστον μία από τις παρακάτω εκδόσεις του SQL Server:

 - SQL Server 2016 και παραπάνω 
 - SQL Server 2014 SP1 CU3 και παραπάνω
 - SQL Server 2014 RTM CU10 και παραπάνω
 - SQL Server 2012 SP2 CU8 και παραπάνω
 - SQL Server 2012 SP3 και παραπάνω


> [AZURE.IMPORTANT] Χρησιμοποιήστε την πιο πρόσφατη έκδοση του SQL Server Management Studio για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL. Παλαιότερες εκδόσεις του SQL Server Management Studio δεν μπορεί να ρυθμίσει βάσης δεδομένων SQL ως συνδρομητή. [Ενημέρωση SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Επόμενα βήματα

- [Νεότερη έκδοση του SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)
- [Νεότερη έκδοση του SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Αναπαραγωγή συναλλαγών](https://msdn.microsoft.com/library/mt589530.aspx)
- [V12 βάση δεδομένων SQL](sql-database-v12-whats-new.md)
- [Transact-SQL μερικώς ή δεν υποστηρίζεται συναρτήσεις](sql-database-transact-sql-information.md)
- [Μετεγκατάσταση βάσεων δεδομένων SQL Server με χρήση Βοηθό μετεγκατάστασης του SQL Server](http://blogs.msdn.com/b/ssma/)

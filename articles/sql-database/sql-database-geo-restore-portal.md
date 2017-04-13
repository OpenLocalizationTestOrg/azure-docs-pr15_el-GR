<properties
    pageTitle="Επαναφέρετε μια βάση δεδομένων Azure SQL από μια αυτόματη δημιουργία αντιγράφων ασφαλείας (Azure πύλη) | Microsoft Azure"
    description="Επαναφέρετε μια βάση δεδομένων Azure SQL από μια αυτόματη δημιουργία αντιγράφων ασφαλείας (Azure πύλη)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Επαναφέρετε μια βάση δεδομένων Azure SQL από μια αυτόματη δημιουργία αντιγράφων ασφαλείας με την πύλη Azure


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md#geo-restore)
- [Επαναφορά παν: PowerShell](sql-database-geo-restore-powershell.md)

Αυτό το άρθρο παρουσιάζει τον τρόπο για να επαναφέρετε τη βάση δεδομένων από μια [Αυτόματη δημιουργία αντιγράφων ασφαλείας](sql-database-automated-backups.md) σε ένα νέο διακομιστή με [Παν-επαναφορά](sql-database-recovery-using-backups/.md#geo-restore) με την πύλη Azure.

## <a name="select-a-database-to-restore"></a>Επιλέξτε μια βάση δεδομένων για να επαναφέρετε

Για να επαναφέρετε μια βάση δεδομένων στην πύλη του Azure, κάντε τα εξής βήματα:

1.  Μεταβείτε στην [πύλη του Azure](https://portal.azure.com).
2.  Στην αριστερή πλευρά της οθόνης, επιλέξτε **+ νέο** > **βάσεις δεδομένων** > **Βάσης δεδομένων SQL**:

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Επιλέξτε **αντιγράφου ασφαλείας** ως την προέλευση και, στη συνέχεια, επιλέξτε το αντίγραφο ασφαλείας που θέλετε να επαναφέρετε. Καθορίστε ένα όνομα βάσης δεδομένων, ένα διακομιστή που θέλετε να επαναφέρετε τη βάση δεδομένων, και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**:
  
    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-geo-restore-portal/geo-restore.png)

Παρακολούθηση της κατάστασης της λειτουργίας επαναφοράς κάνοντας κλικ στο εικονίδιο ειδοποίησης στην επάνω δεξιά πλευρά της σελίδας. 


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)

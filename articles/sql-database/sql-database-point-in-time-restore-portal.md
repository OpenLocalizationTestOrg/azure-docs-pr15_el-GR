<properties
    pageTitle="Επαναφέρετε μια βάση δεδομένων Azure SQL σε ένα προηγούμενο σημείο χρόνου (Azure πύλη) | Microsoft Azure"
    description="Επαναφέρετε μια βάση δεδομένων Azure SQL προηγούμενο σημείο στο χρόνο."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Επαναφέρετε μια βάση δεδομένων Azure SQL σε ένα προηγούμενο σημείο χρόνο με την πύλη του Azure


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md)
- [Επαναφορά σε δεδομένη χρονική στιγμή: PowerShell](sql-database-point-in-time-restore-powershell.md)

Αυτό το άρθρο παρουσιάζει τον τρόπο για να επαναφέρετε τη βάση δεδομένων σε μια προγενέστερη χρονική στιγμή από [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md) με την πύλη Azure.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Επαναφορά βάσης δεδομένων SQL σε ένα προηγούμενο σημείο στο χρόνο

Επιλέξτε μια βάση δεδομένων για να επαναφέρετε στην πύλη του Azure:

1.  Ανοίξτε την [πύλη του Azure](https://portal.azure.com).
2.  Στην αριστερή πλευρά της οθόνης, επιλέξτε **περισσότερες υπηρεσίες** > **βάσεις δεδομένων SQL**.
3.  Επιλέξτε τη βάση δεδομένων που θέλετε να επαναφέρετε.
4.  Στο επάνω μέρος της σελίδας της βάσης δεδομένων σας, επιλέξτε **Επαναφορά**:

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Στη σελίδα " **Επαναφορά** ", επιλέξτε την ημερομηνία και ώρα (σε ώρα UTC) για να επαναφέρετε τη βάση δεδομένων και, στη συνέχεια, κάντε κλικ στο κουμπί **OK**:

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Οθόνη τη λειτουργία επαναφοράς

1. Αφού κάνετε κλικ στο **κουμπί OK** στο προηγούμενο βήμα, κάντε κλικ στο εικονίδιο ειδοποίησης στην επάνω δεξιά γωνία της σελίδας και κάντε κλικ στην ειδοποίηση **βάσης δεδομένων SQL επαναφορά** για λεπτομέρειες.

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Ανοίγει η σελίδα βάσης δεδομένων SQL επαναφορά με πληροφορίες σχετικά με την κατάσταση της επαναφοράς. Μπορείτε να κάνετε κλικ το στοιχείο γραμμής για περισσότερες λεπτομέρειες:

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)

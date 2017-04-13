<properties
    pageTitle="Επαναφορά διαγραμμένων βάσης δεδομένων Azure SQL (Azure πύλη) | Microsoft Azure"
    description="Επαναφέρετε μια διαγραμμένη βάση δεδομένων Azure SQL (Azure πύλη)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Επαναφορά διαγραμμένων βάσης δεδομένων Azure SQL με την πύλη Azure

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md)
- [**Επαναφορά διαγραμμένων DB: πύλη**](sql-database-restore-deleted-database-portal.md)
- [Επαναφορά διαγραμμένων DB: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Επιλέξτε τη βάση δεδομένων για να επαναφέρετε 

Για να επαναφέρετε μια διαγραμμένη βάση δεδομένων στην πύλη του Azure:

1.  Στην [πύλη του Azure](https://portal.azure.com), κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** > **διακομιστές SQL**.
3.  Επιλέξτε το διακομιστή που περιείχε η βάση δεδομένων που θέλετε να επαναφέρετε.
4.  Κάντε κύλιση προς τα κάτω στην ενότητα **Λειτουργίες** του σας blade server και επιλέξτε **Διαγραμμένα βάσεις δεδομένων**: ![επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Επιλέξτε τη βάση δεδομένων που θέλετε να επαναφέρετε.
6.  Καθορίστε ένα όνομα βάσης δεδομένων και κάντε κλικ στο κουμπί **OK**:

    ![Επαναφέρετε μια βάση δεδομένων Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)

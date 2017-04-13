<properties
    pageTitle="Επαναφέρετε μια βάση δεδομένων SQL Azure από ένα αντίγραφο ασφαλείας παν πλεονάζοντα (PowerShell) | Microsoft Azure"
    description="Επαναφέρετε μια βάση δεδομένων SQL Azure σε ένα νέο διακομιστή από ένα αντίγραφο ασφαλείας παν πλεονάζοντα"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Επαναφέρετε μια βάση δεδομένων SQL Azure από ένα αντίγραφο ασφαλείας παν πλεονάζοντα με χρήση του PowerShell


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md)
- [Παν-επαναφορά: Πύλη Azure](sql-database-geo-restore-portal.md)

Αυτό το άρθρο σας δείχνει πώς μπορείτε να επαναφέρετε τη βάση δεδομένων σε ένα νέο διακομιστή χρησιμοποιώντας παν-επαναφοράς. Αυτό μπορεί να γίνει μέσω του PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Παν-Επαναφορά βάσης δεδομένων σε μια μεμονωμένη βάση δεδομένων

1. Λάβετε τα πλεονάζοντα παν αντίγραφο ασφαλείας της βάσης δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Έναρξη Επαναφορά από το παν πλεονάζοντα αντίγραφο ασφαλείας με τη χρήση του [επαναφοράς-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Παν-Επαναφορά βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων

1. Λάβετε το παν πλεονάζοντα αντίγραφο ασφαλείας της βάσης δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Έναρξη Επαναφορά από το παν πλεονάζοντα αντίγραφο ασφαλείας με τη χρήση του [επαναφοράς-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet. Καθορίστε το όνομα του χώρου συγκέντρωσης που θέλετε να επαναφέρετε τη βάση δεδομένων σε.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md).
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md).
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md).
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md).  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md).

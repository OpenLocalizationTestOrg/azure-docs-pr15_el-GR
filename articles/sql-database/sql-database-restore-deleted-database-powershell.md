<properties
    pageTitle="Επαναφέρετε μια διαγραμμένη βάση δεδομένων SQL Azure (PowerShell) | Microsoft Azure"
    description="Επαναφέρετε μια διαγραμμένη βάση δεδομένων SQL Azure (PowerShell)."
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


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>Επαναφορά διαγραμμένων βάσης δεδομένων SQL Azure με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md)
- [Επαναφορά διαγραμμένων DB: πύλη](sql-database-restore-deleted-database-portal.md)
- [**Επαναφορά διαγραμμένων DB: PowerShell**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>Λάβετε μια λίστα των διαγραμμένων βάσεων δεδομένων

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>Επαναφορά διαγραμμένων βάση δεδομένων σας σε μια μεμονωμένη βάση δεδομένων

Λάβετε το αντίγραφο ασφαλείας διαγραμμένη βάση δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Επαναφορά από το αντίγραφο ασφαλείας της βάσης δεδομένων διαγραμμένων ξεκινήσετε, στη συνέχεια, χρησιμοποιώντας το cmdlet [Επαναφορά AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) .

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>Επαναφορά διαγραμμένων βάση δεδομένων σας σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων

Λάβετε το αντίγραφο ασφαλείας διαγραμμένη βάση δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το cmdlet [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) . Επαναφορά από το αντίγραφο ασφαλείας της βάσης δεδομένων διαγραμμένων ξεκινήσετε, στη συνέχεια, χρησιμοποιώντας το cmdlet [Επαναφορά AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) .

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)

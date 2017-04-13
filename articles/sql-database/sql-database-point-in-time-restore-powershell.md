<properties
    pageTitle="Επαναφέρετε μια βάση δεδομένων SQL Azure σε ένα προηγούμενο σημείο χρόνου (PowerShell) | Microsoft Azure"
    description="Επαναφέρετε μια βάση δεδομένων SQL Azure σε ένα προηγούμενο σημείο χρόνου"
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

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Επαναφέρετε μια βάση δεδομένων SQL Azure σε ένα προηγούμενο σημείο ώρα με το PowerShell

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-recovery-using-backups.md)
- [Επαναφορά σε δεδομένη χρονική στιγμή: Πύλη Azure](sql-database-point-in-time-restore-portal.md)

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να ανακτήσετε τη βάση δεδομένων σε μια προγενέστερη χρονική στιγμή από [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md). Μπορείτε να το κάνετε αυτό με χρήση του PowerShell.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Επαναφέρετε τη βάση δεδομένων σε ένα σημείο χρονικό ως αυτόνομης υπηρεσίας βάσης δεδομένων

1. Λήψη της βάσης δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Επαναφορά της βάσης δεδομένων σε ένα σημείο στο χρόνο χρησιμοποιώντας το [επαναφοράς-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Επαναφορά βάσης δεδομένων σας σε ένα σημείο στο χρόνο σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων

1. Λήψη της βάσης δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας το [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Επαναφορά της βάσης δεδομένων σε ένα σημείο στο χρόνο χρησιμοποιώντας το [επαναφοράς-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- Για να μάθετε σχετικά με τη βάση δεδομένων SQL Azure αυτόματης δημιουργίας αντιγράφων ασφαλείας, ανατρέξτε στο θέμα [βάση δεδομένων SQL αυτόματης δημιουργίας αντιγράφων ασφαλείας](sql-database-automated-backups.md)
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την ανάκτηση, ανατρέξτε στο θέμα [Επαναφορά βάσης δεδομένων από τα αντίγραφα ασφαλείας που ξεκινούν από την υπηρεσία](sql-database-recovery-using-backups.md)
- Για να μάθετε σχετικά με τις επιλογές αποκατάστασης του ταχύτερος, ανατρέξτε στο θέμα [Ενεργό-παν-αναπαραγωγής](sql-database-geo-replication-overview.md)  
- Για να μάθετε πώς να χρησιμοποιείτε την αυτόματη δημιουργία αντιγράφων ασφαλείας για την αρχειοθέτηση, ανατρέξτε στο θέμα [Αντιγραφή βάσης δεδομένων](sql-database-copy.md)

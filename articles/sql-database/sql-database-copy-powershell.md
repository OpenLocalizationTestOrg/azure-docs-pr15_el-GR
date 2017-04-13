<properties 
    pageTitle="Αντιγράψτε μια βάση δεδομένων Azure SQL με χρήση του PowerShell | Microsoft Azure" 
    description="Δημιουργία αντιγράφου μιας βάσης δεδομένων Azure SQL με χρήση του PowerShell" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Αντιγράψτε μια βάση δεδομένων Azure SQL με χρήση του PowerShell


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-copy.md)
- [Πύλη του Azure](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Σε αυτό το άρθρο δείχνει πώς μπορείτε να αντιγράψετε μια βάση δεδομένων SQL με το PowerShell στον ίδιο διακομιστή, σε διαφορετικό διακομιστή, ή να αντιγράψετε μια βάση δεδομένων σε ένα [χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md). Λειτουργία αντίγραφο της βάσης δεδομένων χρησιμοποιεί το cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Για να ολοκληρώσετε αυτό το άρθρο, χρειάζεστε τα εξής:

- Μια βάση δεδομένων Azure SQL (μια βάση δεδομένων για να αντιγράψετε). Εάν δεν έχετε μια βάση δεδομένων SQL, να δημιουργήσετε μία ακολουθώντας τα βήματα σε αυτό το άρθρο: [Δημιουργία του πρώτου βάση δεδομένων SQL Azure](sql-database-get-started.md).
- Η πιο πρόσφατη έκδοση του Azure PowerShell. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md).


Πολλές νέες δυνατότητες της βάσης δεδομένων SQL υποστηρίζονται μόνο όταν χρησιμοποιείτε το [μοντέλο ανάπτυξης διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md), ώστε να παραδείγματα χρησιμοποιούν τα [cmdlet του PowerShell βάσης δεδομένων SQL Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx) για διαχείριση πόρων. Το υπάρχον μοντέλο κλασική ανάπτυξης [(κλασική) cmdlet για βάση δεδομένων SQL Azure](https://msdn.microsoft.com/library/azure/dn546723.aspx) που υποστηρίζονται για συμβατότητα με προηγούμενες εκδόσεις, αλλά συνιστάται να χρησιμοποιήσετε τα cmdlet διαχείρισης πόρων.


>[AZURE.NOTE] Ανάλογα με το μέγεθος της βάσης δεδομένων σας, η λειτουργία αντιγραφής ενδέχεται να χρειαστεί κάποιος χρόνος για να ολοκληρωθεί.


## <a name="copy-a-sql-database-to-the-same-server"></a>Αντιγράψτε μια βάση δεδομένων SQL στον ίδιο διακομιστή

Για να δημιουργήσετε το αντίγραφο στον ίδιο διακομιστή, παραλείψτε την `-CopyServerName` παραμέτρου (ή να τη ρυθμίσετε στον ίδιο διακομιστή).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Αντιγράψτε μια βάση δεδομένων SQL σε διαφορετικό διακομιστή

Για να δημιουργήσετε το αντίγραφο σε διαφορετικό διακομιστή, συμπεριλάβετε το `-CopyServerName` παραμέτρου και ορίστε την τιμή σε διαφορετικό διακομιστή. Το *αντίγραφο* διακομιστή πρέπει να υπάρχει ήδη. Εάν το έγγραφο βρίσκεται σε μια ομάδα διαφορετικό πόρο και, στη συνέχεια, μπορείτε, επίσης, πρέπει να συμπεριλάβετε το `-CopyResourceGroupName` παραμέτρου.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Αντιγράψτε μια βάση δεδομένων SQL σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων

Για να δημιουργήσετε ένα αντίγραφο μιας βάσης δεδομένων SQL σε ένα χώρο συγκέντρωσης, ορίστε το `-ElasticPoolName` παραμέτρου σε έναν υπάρχοντα χώρο συγκέντρωσης.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Επίλυση συνδέσεις

Για να επιλύσετε συνδέσεις μετά την ολοκλήρωση της λειτουργίας αντιγραφής, ανατρέξτε στο θέμα [επίλυση συνδέσεις](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Παράδειγμα δέσμης ενεργειών του PowerShell

Η ακόλουθη δέσμη ενεργειών προϋποθέτει όλων των ομάδων πόρων, διακομιστές, και το χώρο συγκέντρωσης υπάρχουν ήδη (αντικαταστήστε τις τιμές μεταβλητών με υπάρχουσα τους πόρους σας). Όλα όσα πρέπει να υπάρχει, με εξαίρεση το αντίγραφο της βάσης δεδομένων.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Επόμενα βήματα

- Για μια επισκόπηση του αντιγράψετε μια βάση δεδομένων SQL Azure, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων Azure SQL](sql-database-copy.md) .
- Ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων Azure SQL με την πύλη Azure](sql-database-copy-portal.md) για να αντιγράψετε μια βάση δεδομένων με την πύλη Azure.
- Ανατρέξτε στο θέμα [Αντιγραφή μια βάση δεδομένων Azure SQL χρησιμοποιώντας T-SQL](sql-database-copy-transact-sql.md) για να αντιγράψετε μια βάση δεδομένων με χρήση Transact-SQL.
- Δείτε [πώς μπορείτε να διαχειριστείτε ασφαλείας βάσης δεδομένων Azure SQL μετά την αποκατάσταση](sql-database-geo-replication-security-config.md) για να μάθετε περισσότερα σχετικά με τη Διαχείριση χρηστών και συνδέσεις κατά την αντιγραφή μιας βάσης δεδομένων σε διαφορετικό διακομιστή λογική.


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Νέα AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Διαχείριση συνδέσεων](sql-database-manage-logins.md)
- [Σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγματος](sql-database-connect-query-ssms.md)
- [Εξαγωγή της βάσης δεδομένων σε ένα BACPAC](sql-database-export.md)
- [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)
- [Τεκμηρίωση βάσης δεδομένων SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Αναφορές για τα Cmdlet του PowerShell βάσης δεδομένων Azure SQL](https://msdn.microsoft.com/library/mt574084.aspx)

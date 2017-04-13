<properties
    pageTitle="Διαχείριση της βάσης δεδομένων Azure SQL με το PowerShell | Microsoft Azure"
    description="Τη διαχείριση της βάσης δεδομένων SQL Azure με το PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="sstein"/>

# <a name="manage-azure-sql-database-with-powershell"></a>Διαχείριση της βάσης δεδομένων Azure SQL με το PowerShell


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

Αυτό το θέμα δείχνει τα cmdlet του PowerShell που χρησιμοποιούνται για να εκτελούν πολλές εργασίες βάση δεδομένων SQL Azure. Για μια πλήρη λίστα, ανατρέξτε στο θέμα [Azure SQL βάση δεδομένων cmdlet] (https://msdn.microsoft.com/library/mt574084(v=azure.300\).aspx).


## <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων

Δημιουργήστε μια ομάδα πόρων του βάση δεδομένων SQL και των σχετικών Azure πόρων με το [δημιουργία-AzureRmResourceGroup] (https://msdn.microsoft.com/library/azure/mt759837(v=azure.300\).aspx) cmdlet.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του Azure PowerShell με το διαχειριστή πόρων Azure](../powershell-azure-resource-manager.md).
Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL δέσμη ενεργειών του PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).

## <a name="create-a-sql-database-server"></a>Δημιουργήστε μια βάση δεδομένων SQL server

Δημιουργήστε μια βάση δεδομένων SQL server με το [δημιουργία-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) cmdlet. Αντικαταστήστε *server1* με το όνομα του διακομιστή σας. Ονόματα διακομιστών πρέπει να είναι μοναδικό σε όλους τους διακομιστές βάση δεδομένων SQL Azure. Εάν το όνομα του διακομιστή είναι ήδη ληφθεί, λαμβάνετε ένα σφάλμα. Αυτή η εντολή ενδέχεται να χρειαστούν αρκετά λεπτά για να ολοκληρωθεί. Η ομάδα πόρων πρέπει να υπάρχει ήδη στο τη συνδρομή σας.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    

$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι η βάση δεδομένων SQL](sql-database-technical-overview.md). Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL δέσμη ενεργειών του PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-server-firewall-rule"></a>Δημιουργία κανόνα τείχους προστασίας του διακομιστή βάσης δεδομένων SQL

Δημιουργία κανόνα τείχους προστασίας για να αποκτήσετε πρόσβαση στο διακομιστή με το [δημιουργία-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet. Εκτελέστε την ακόλουθη εντολή, αντικαθιστώντας τις διευθύνσεις IP έναρξης και λήξης με έγκυρες τιμές για το πρόγραμμα-πελάτη. Η ομάδα πόρων και διακομιστή πρέπει να υπάρχει ήδη στο τη συνδρομή σας.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Για να επιτρέψετε σε άλλες υπηρεσίες του Azure πρόσβαση στο διακομιστή σας, δημιουργήστε έναν κανόνα τείχος προστασίας και ορίστε και τα δύο το `-StartIpAddress` και `-EndIpAddress` **0.0.0.0**. Αυτός ο κανόνας ειδική τείχος προστασίας επιτρέπει όλη την κυκλοφορία Azure για να αποκτήσετε πρόσβαση στο διακομιστή.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τείχος προστασίας βάσης δεδομένων SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx). Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL δέσμη ενεργειών του PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="create-a-sql-database-blank"></a>Δημιουργία βάσης δεδομένων SQL (κενή)

Δημιουργία βάσης δεδομένων με το [δημιουργία-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) cmdlet. Η ομάδα πόρων και διακομιστή πρέπει να υπάρχει ήδη στο τη συνδρομή σας. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι η βάση δεδομένων SQL](sql-database-technical-overview.md). Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL δέσμη ενεργειών του PowerShell](sql-database-get-started-powershell.md#create-a-sql-database-powershell-script).


## <a name="change-the-performance-level-of-a-sql-database"></a>Αλλάξτε το επίπεδο επιδόσεων μια βάση δεδομένων SQL

Κλιμάκωση τη βάση δεδομένων προς τα επάνω ή προς τα κάτω με το [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet. Η ομάδα πόρων διακομιστή και βάσης δεδομένων πρέπει να υπάρχει ήδη στο τη συνδρομή σας. Ορισμός του `-RequestedServiceObjectiveName` σε ένα μονό διάστημα (όπως το παρακάτω τμήμα κώδικα) για βασική σειρά. Ρυθμίσετε να *S0*, *S1*, *P1*, *P6*, κ.λπ., όπως το προηγούμενο παράδειγμα για άλλα επίπεδα.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επιλογές βάσης δεδομένων SQL και επιδόσεων: Κατανόηση στοιχεία που είναι διαθέσιμα σε κάθε επίπεδο υπηρεσιών](sql-database-service-tiers.md). Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [δέσμη ενεργειών του PowerShell δείγμα για να αλλάξετε το επίπεδο επίπεδο και επιδόσεων υπηρεσίας της βάσης δεδομένων SQL](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="copy-a-sql-database-to-the-same-server"></a>Αντιγράψτε μια βάση δεδομένων SQL στον ίδιο διακομιστή

Αντιγράψτε μια βάση δεδομένων SQL στον ίδιο διακομιστή με το [δημιουργία-AzureRmSqlDatabaseCopy] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx) cmdlet. Ορίστε το `-CopyServerName` και `-CopyResourceGroupName` για τις ίδιες τιμές με σας ομάδα διακομιστή και πόρων της βάσης δεδομένων προέλευσης.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αντιγραφή μιας βάσης δεδομένων SQL Azure](sql-database-copy.md). Για ένα δείγμα δέσμης ενεργειών, ανατρέξτε στο θέμα [Αντιγραφή μια βάση δεδομένων SQL δέσμη ενεργειών του PowerShell](sql-database-copy-powershell.md#example-powershell-script).


## <a name="delete-a-sql-database"></a>Διαγραφή βάσης δεδομένων SQL

Διαγραφή βάσης δεδομένων SQL με το [κατάργηση-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619368(v=azure.300\).aspx) cmdlet. Η ομάδα πόρων διακομιστή και βάσης δεδομένων πρέπει να υπάρχει ήδη στο τη συνδρομή σας.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="delete-a-sql-database-server"></a>Διαγραφή μιας βάσης δεδομένων SQL server

Διαγραφή ενός διακομιστή με το [κατάργηση-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603488(v=azure.300\).aspx) cmdlet.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="create-and-manage-elastic-database-pools-using-powershell"></a>Δημιουργία και διαχείριση συνόλων ελαστικότητας βάσης δεδομένων με χρήση του PowerShell

Για λεπτομέρειες σχετικά με τη δημιουργία χώρους συγκέντρωσης ελαστικότητας βάσης δεδομένων με χρήση του PowerShell, ανατρέξτε στο θέμα [Δημιουργία νέου χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell](sql-database-elastic-pool-create-powershell.md).

Για λεπτομέρειες σχετικά με τη Διαχείριση συνόλων ελαστικότητας βάση δεδομένων με χρήση του PowerShell, ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell](sql-database-elastic-pool-manage-powershell.md).



## <a name="related-information"></a>Σχετικές πληροφορίες

- [Cmdlet για βάση δεδομένων azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Αναφορά azure Cmdlet] (https://msdn.microsoft.com/library/azure/dn708514(v=azure.300\).aspx)

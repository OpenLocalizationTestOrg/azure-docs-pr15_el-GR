<properties
   pageTitle="Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (PowerShell) | Microsoft Azure"
   description="Εργασίες του PowerShell για την επαναφορά ενός αποθήκη δεδομένων του SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (PowerShell)

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Πύλη][]
- [PowerShell][]
- [ΥΠΌΛΟΙΠΟ][]

Σε αυτό το άρθρο θα μάθετε πώς μπορείτε να επαναφέρετε μια αποθήκη δεδομένων του SQL Azure με χρήση του PowerShell.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

**Επαληθεύστε τη χωρητικότητα DTU.** Κάθε αποθήκη δεδομένων του SQL φιλοξενείται από SQL server (π.χ., myserver.database.windows.net) που έχει ένα προεπιλεγμένο όριο DTU.  Πριν να επαναφέρετε μια αποθήκη δεδομένων του SQL, επιβεβαιώστε ότι το υπόλοιπο DTU υπάρχουν αρκετά για τη βάση δεδομένων που επαναφέρεται περιλαμβάνει το SQL server. Για να μάθετε πώς μπορείτε να υπολογίσετε DTU απαιτείται ή για να ζητήσετε περισσότερες DTU, ανατρέξτε στο θέμα [αίτηση αλλαγής ορίου DTU][].

### <a name="install-powershell"></a>Εγκατάσταση του PowerShell

Για να χρησιμοποιήσετε Azure PowerShell με αποθήκη δεδομένων του SQL, θα πρέπει να εγκαταστήσετε Azure PowerShell έκδοση 1.0 ή μεγαλύτερη.  Μπορείτε να ελέγξετε την έκδοση, εκτελώντας **Get-λειτουργική μονάδα - ListAvailable-AzureRM όνομα**.  Την πιο πρόσφατη έκδοση μπορεί να εγκατασταθεί από το [Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμας][].  Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση της πιο πρόσφατης έκδοσης, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure][].

## <a name="restore-an-active-or-paused-database"></a>Επαναφορά μιας βάσης δεδομένων ενεργό ή βρίσκεται σε παύση

Για να επαναφέρετε μια βάση δεδομένων από ένα στιγμιότυπο, χρησιμοποιήστε το cmdlet του PowerShell [Επαναφορά AzureRmSqlDatabase][] .

1. Άνοιγμα του Windows PowerShell.
2. Συνδεθείτε στο λογαριασμό σας Azure και παράθεση σε λίστα όλες τις συνδρομές που σχετίζεται με το λογαριασμό σας.
3. Επιλέξτε τη συνδρομή που περιέχει τη βάση δεδομένων για να γίνει επαναφορά.
4. Λίστα με τα σημεία επαναφοράς για τη βάση δεδομένων.
5. Επιλέξτε το σημείο επαναφοράς που θέλετε με το RestorePointCreationDate.
6. Επαναφορά της βάσης δεδομένων για το σημείο επαναφοράς που θέλετε.
7. Βεβαιωθείτε ότι η Επαναφορά βάσης δεδομένων είναι σε σύνδεση.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Μετά την ολοκλήρωση της επαναφοράς, μπορείτε να ρυθμίσετε τη βάση δεδομένων έχει ανακτηθεί από την εξής [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][].


## <a name="restore-a-deleted-database"></a>Επαναφορά διαγραμμένων βάσης δεδομένων

Για να επαναφέρετε μια διαγραμμένη βάση δεδομένων, χρησιμοποιήστε το cmdlet [Επαναφορά AzureRmSqlDatabase][] .

1. Άνοιγμα του Windows PowerShell.
2. Συνδεθείτε στο λογαριασμό σας Azure και παράθεση σε λίστα όλες τις συνδρομές που σχετίζεται με το λογαριασμό σας.
3. Επιλέξτε τη συνδρομή που περιέχει το διαγραμμένο βάσης δεδομένων να γίνει επαναφορά.
4. Μεταβείτε στη συγκεκριμένη βάση δεδομένων Διαγραμμένα.
5. Επαναφορά της διαγραμμένης βάσης δεδομένων.
6. Βεβαιωθείτε ότι η Επαναφορά βάσης δεδομένων είναι σε σύνδεση.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Μετά την ολοκλήρωση της επαναφοράς, μπορείτε να ρυθμίσετε τη βάση δεδομένων έχει ανακτηθεί από την εξής [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][].


## <a name="restore-from-an-azure-geographical-region"></a>Επαναφορά από ένα Azure γεωγραφική περιοχή

Για να ανακτήσετε μια βάση δεδομένων, χρησιμοποιήστε το cmdlet [Επαναφορά AzureRmSqlDatabase][] .

1. Άνοιγμα του Windows PowerShell.
2. Συνδεθείτε στο λογαριασμό σας Azure και παράθεση σε λίστα όλες τις συνδρομές που σχετίζεται με το λογαριασμό σας.
3. Επιλέξτε τη συνδρομή που περιέχει τη βάση δεδομένων για να γίνει επαναφορά.
4. Λήψη της βάσης δεδομένων που θέλετε να ανακτήσετε.
5. Δημιουργία αίτησης ανάκτησης για τη βάση δεδομένων.
6. Επαληθεύστε την κατάσταση της παν Επαναφορά βάσης δεδομένων.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Για να ρυθμίσετε τη βάση δεδομένων μετά την ολοκλήρωση της επαναφοράς, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][]. 


Η βάση δεδομένων που έχει ανακτηθεί θα με δυνατότητα TDE εάν η βάση δεδομένων προέλευσης με δυνατότητα TDE.


## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με τις δυνατότητες επιχειρηματικής συνέχειας της βάσης δεδομένων SQL Azure εκδόσεις, διαβάστε τη [βάση δεδομένων SQL Azure επιχειρήσεις συνέχειας Επισκόπηση][].

<!--Image references-->

<!--Article references-->
[Azure Επισκόπηση συνέχειας επαγγελματικής βάσης δεδομένων SQL]: sql-database-business-continuity.md
[Αίτηση αλλαγής ορίου DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Ρύθμιση παραμέτρων βάσης δεδομένων σας μετά την ανάκτηση]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell]: powershell-install-configure.md
[Επισκόπηση]: ./sql-data-warehouse-restore-database-overview.md
[Πύλη]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ΥΠΌΛΟΙΠΟ]: ./sql-data-warehouse-restore-database-rest-api.md
[Ρύθμιση παραμέτρων βάσης δεδομένων σας μετά την ανάκτηση]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Επαναφορά AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Πρόγραμμα εγκατάστασης πλατφόρμας Microsoft στο Web]: https://aka.ms/webpi-azps

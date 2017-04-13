<properties
    pageTitle="Δημιουργήστε ένα νέο χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιείτε PowerShell για βάση δεδομένων SQL Azure κλίμακα ανάληψης πόρους με τη δημιουργία ενός χώρου συγκέντρωσης μεταβλητού μεγέθους ελαστικότητας βάσης δεδομένων για να διαχειριστείτε πολλές βάσεις δεδομένων."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Δημιουργήστε ένα νέο χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Μάθετε πώς μπορείτε να δημιουργήσετε ένα [χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md) με χρήση των cmdlet του PowerShell. 

Για συνήθεις τους κωδικούς σφάλματος, ανατρέξτε στο θέμα [τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL: σφάλμα σύνδεσης και άλλα θέματα της βάσης δεδομένων](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Ελαστικά χώρους συγκέντρωσης είναι γενικά διαθέσιμο (GA) σε όλες τις περιοχές Azure εκτός από Βόρεια κεντρική ΜΑΣ και Δυτική Ινδίας όπου είναι αυτήν τη στιγμή στην προεπισκόπηση.  Όσο το δυνατόν πιο σύντομα θα παρέχονται GA ελαστικότητας συνόλων σε αυτές τις περιοχές. Επίσης, ελαστικά χώρους συγκέντρωσης δεν υποστηρίζεται επί του παρόντος βάσεων δεδομένων με χρήση [Συναλλαγής στη μνήμη ή ανάλυσης που βρίσκεται στη μνήμη](sql-database-in-memory.md).


Πρέπει να εκτελείται Azure PowerShell 1.0 ή νεότερη έκδοση. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Δημιουργήστε ένα νέο χώρο συγκέντρωσης

Το cmdlet [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) δημιουργεί ένα νέο σύνολο. Οι τιμές για eDTU ανά χώρου συγκέντρωσης, ελάχιστο και μέγιστο Dtus είναι περιορισμένη κατά την τιμή επίπεδο υπηρεσίας (basic, τυπική ή premium). Ανατρέξτε στο θέμα [eDTU και αποθήκευσης τα όρια για ελαστικά σύνολα και ελαστικά βάσεις δεδομένων](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Δημιουργήστε μια νέα βάση δεδομένων ελαστικότητας σε ένα χώρο συγκέντρωσης

Χρησιμοποιήστε το cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) και ορίστε την παράμετρο **ElasticPoolName** στο χώρο συγκέντρωσης προορισμού. Για να μετακινήσετε μια υπάρχουσα βάση δεδομένων σε ένα χώρο συγκέντρωσης, ανατρέξτε στο θέμα [Μετακίνηση μιας βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Δημιουργία χώρου συγκέντρωσης και συμπληρώστε τον με πολλές νέες βάσεις δεδομένων 

Δημιουργία ενός μεγάλου αριθμού των βάσεων δεδομένων σε ένα χώρο συγκέντρωσης μπορεί να χρειαστεί χρόνο κατά την εργασία με την πύλη ή το cmdlet του PowerShell που δημιουργούν μόνο μία βάση δεδομένων κάθε φορά. Για να αυτοματοποιήσετε τη δημιουργία σε ένα νέο σύνολο, ανατρέξτε στο θέμα [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Παράδειγμα: δημιουργία χώρου συγκέντρωσης χρησιμοποιώντας PowerShell 

Αυτή η δέσμη ενεργειών δημιουργεί μια νέα ομάδα Azure πόρων και ένα νέο διακομιστή. Όταν σας ζητηθεί, δώστε ένα όνομα χρήστη διαχειριστή και τον κωδικό πρόσβασης για το νέο διακομιστή (δεν Azure τα διαπιστευτήριά σας).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Επόμενα βήματα

- [Διαχείριση του χώρου συγκέντρωσης](sql-database-elastic-pool-manage-powershell.md)
- [Δημιουργία ελαστικότητας εργασίες](sql-database-elastic-jobs-overview.md) Ελαστικά εργασίες σάς επιτρέπουν να εκτελεστεί δέσμες ενεργειών T SQL σε οποιονδήποτε αριθμό βάσεων δεδομένων στο χώρο συγκέντρωσης.
- [Κλιμάκωση με βάση δεδομένων SQL Azure το θέμα](sql-database-elastic-scale-introduction.md): χρήση εργαλείων ελαστικότητας βάσης δεδομένων με κλίμακα.


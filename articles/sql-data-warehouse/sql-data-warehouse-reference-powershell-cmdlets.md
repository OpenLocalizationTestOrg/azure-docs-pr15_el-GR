<properties
   pageTitle="Cmdlet του PowerShell για αποθήκη δεδομένων του SQL Azure"
   description="Βρείτε το επάνω cmdlet του PowerShell για αποθήκη δεδομένων του SQL Azure συμπεριλαμβανομένου του τρόπου παύση και συνέχιση μιας βάσης δεδομένων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Cmdlet του PowerShell και REST API για αποθήκη δεδομένων του SQL

Μπορείτε να διαχειριστείτε πολλές εργασίες διαχείρισης αποθήκη δεδομένων του SQL με χρήση είτε cmdlet του Azure PowerShell ή REST API.  Ακολουθούν ορισμένα παραδείγματα πώς μπορείτε να χρησιμοποιήσετε τις εντολές του PowerShell για να αυτοματοποιήσετε κοινές εργασίες στο σας αποθήκη δεδομένων του SQL.  Για ορισμένα παραδείγματα ΥΠΌΛΟΙΠΑ καλή, ανατρέξτε στο άρθρο [Διαχείριση κλιμάκωση με το ΥΠΌΛΟΙΠΟ][].

> [AZURE.NOTE]  Για να χρησιμοποιήσετε Azure PowerShell με αποθήκη δεδομένων του SQL, χρειάζεστε Azure PowerShell έκδοση 1.0.3 ή μεγαλύτερη.  Μπορείτε να ελέγξετε την έκδοση, εκτελώντας **Get-λειτουργική μονάδα - ListAvailable-Azure όνομα**.  Την πιο πρόσφατη έκδοση μπορεί να εγκατασταθεί από το [Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμας][].  Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση της πιο πρόσφατης έκδοσης, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Γρήγορα αποτελέσματα με το cmdlet του Azure PowerShell

1. Άνοιγμα του Windows PowerShell. 
2. Στη γραμμή εντολών του PowerShell, εκτελέστε αυτές οι εντολές για να εισέλθετε στη Διαχείριση πόρων Azure και επιλέξτε τη συνδρομή σας.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Παράδειγμα αποθήκη δεδομένων SQL παύση

Καταδείξτε μια βάση δεδομένων με το όνομα "Database02" φιλοξενούνται σε ένα διακομιστή που ονομάζεται "Server01."  Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Μια παραλλαγή, αυτό το παράδειγμα σωλήνες τα ανακτημένα αντικειμένου στο [Αναστολής AzureRmSqlDatabase][].  Ως αποτέλεσμα, έχει διακοπεί η βάση δεδομένων. Η τελική εντολή εμφανίζει τα αποτελέσματα.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Έναρξη παράδειγμα αποθήκη δεδομένων SQL

Συνέχιση λειτουργία μιας βάσης δεδομένων με το όνομα "Database02" φιλοξενούνται σε ένα διακομιστή που ονομάζεται "Server01." Ο διακομιστής είναι που περιέχονται σε μια ομάδα πόρων με το όνομα "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Μια παραλλαγή, αυτό το παράδειγμα ανακτά μια βάση δεδομένων με το όνομα "Database02" από ένα διακομιστή που ονομάζεται "Server01" που περιέχονται σε μια ομάδα πόρων με το όνομα "ResourceGroup1." Το σωλήνες τα ανακτημένα αντικειμένου στο [Βιογραφικό σημείωμα AzureRmSqlDatabase][].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Σημειώστε ότι εάν ο διακομιστής σας είναι foo.database.windows.net, χρησιμοποιήστε τη "foo" ως όνομα_διακομιστή - στο τα cmdlet του PowerShell.

## <a name="frequently-used-powershell-cmdlets"></a>Συχνά cmdlet του PowerShell που χρησιμοποιείται

Αυτά τα cmdlet του PowerShell που χρησιμοποιούνται συχνά με αποθήκη δεδομένων του SQL Azure.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Νέα AzureRmSqlDatabase][]
- [Κατάργηση AzureRmSqlDatabase][]
- [Επαναφορά AzureRmSqlDatabase][] 
- [Βιογραφικό σημείωμα AzureRmSqlDatabase][]
- [Επιλέξτε AzureRmSubscription][]
- [Ορισμός AzureRmSqlDatabase][]
- [Αναστολή AzureRmSqlDatabase][]

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερα παραδείγματα PowerShell, ανατρέξτε στα θέματα:

- [Δημιουργήστε μια αποθήκη δεδομένων του SQL με χρήση του PowerShell][]
- [Επαναφορά βάσης δεδομένων][]

Για μια λίστα με όλες τις εργασίες που μπορεί να γίνει αυτόματα με το PowerShell, ανατρέξτε στο θέμα [Cmdlet βάσης δεδομένων SQL Azure][].  Για μια λίστα των εργασιών που μπορεί να γίνει αυτόματα με τα ΥΠΌΛΟΙΠΑ, δείτε τις [εργασίες για βάσεις δεδομένων SQL Azure][].

<!--Image references-->

<!--Article references-->
[Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure]: ./powershell-install-configure.md
[Δημιουργήστε μια αποθήκη δεδομένων του SQL με χρήση του PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Επαναφορά βάσης δεδομένων]: ./sql-data-warehouse-restore-database-powershell.md
[Διαχείριση κλιμάκωση με το ΥΠΌΛΟΙΠΟ]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Cmdlet για βάση δεδομένων Azure SQL]: https://msdn.microsoft.com/library/mt574084.aspx
[Λειτουργίες για βάσεις δεδομένων Azure SQL]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Νέα AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Κατάργηση AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Επαναφορά AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Βιογραφικό σημείωμα AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Επιλέξτε AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Ορισμός AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Αναστολή AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Πρόγραμμα εγκατάστασης πλατφόρμας Microsoft στο Web]: https://aka.ms/webpi-azps

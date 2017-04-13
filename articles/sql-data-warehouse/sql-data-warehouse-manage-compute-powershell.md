<properties
   pageTitle="Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (PowerShell) | Microsoft Azure"
   description="Εργασίες του PowerShell για τη Διαχείριση υπολογιστική ισχύ. Κλίμακα υπολογίσετε τους πόρους, προσαρμόζοντας DWUs. Ή, παύση και συνέχιση υπολογισμού πόρους για να αποθηκεύσετε κόστους."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (PowerShell)

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-data-warehouse-manage-compute-overview.md)
- [Πύλη](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ΥΠΌΛΟΙΠΟ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Κλίμακα επιδόσεων, αλλάζοντας την κλίμακα ανάληψη υπολογιστική πόρους και μνήμης για να καλύψει τις μεταβαλλόμενες απαιτήσεις του φόρτου εργασίας σας. Αποθήκευση κόστους από τους πόρους κλίμακας πίσω κατά τη διάρκεια μη κορύφωσης ώρες ή παύση υπολογισμού εντελώς. 

Αυτή η συλλογή εργασίες χρησιμοποιεί το Azure πύλη για να:

- Κλίμακα υπολογισμού
- Παύση υπολογισμού
- Βιογραφικό σημείωμα υπολογισμού

Για να μάθετε σχετικά με αυτό, ανατρέξτε στο θέμα [Διαχείριση τον υπολογισμό Επισκόπηση][].


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

### <a name="install-the-latest-version-of-azure-powershell"></a>Εγκαταστήστε την πιο πρόσφατη έκδοση του Azure PowerShell

> [AZURE.NOTE]  Για να χρησιμοποιήσετε Azure PowerShell με αποθήκη δεδομένων του SQL, χρειάζεστε Azure PowerShell έκδοση 1.0.3 ή μεγαλύτερη.  Για να επιβεβαιώσετε την τρέχουσα έκδοση, εκτελέστε την εντολή **Get-λειτουργική μονάδα - ListAvailable-Azure όνομα**. Μπορείτε να εγκαταστήσετε την πιο πρόσφατη έκδοση από το [Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμας][].  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Γρήγορα αποτελέσματα με το cmdlet του Azure PowerShell

Για να ξεκινήσετε:

1. Ανοίξτε το Azure PowerShell. 
2. Στη γραμμή εντολών του PowerShell, εκτελέστε αυτές οι εντολές για να εισέλθετε στη Διαχείριση πόρων Azure και επιλέξτε τη συνδρομή σας.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Κλίμακα power υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Για να αλλάξετε το DWUs, χρησιμοποιήστε το cmdlet [Set-AzureRmSqlDatabase][] PowerShell. Το παρακάτω παράδειγμα ορίζει το στόχο επιπέδου υπηρεσίας DW1000 για τη βάση δεδομένων MySQLDW που φιλοξενείται σε διακομιστή MyServer. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Παύση υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Για να διακόψετε προσωρινά μια βάση δεδομένων, χρησιμοποιήστε το cmdlet [Αναστολής AzureRmSqlDatabase][] . Το παρακάτω παράδειγμα διακόπτει προσωρινά μια βάση δεδομένων με το όνομα Database02 φιλοξενούνται σε ένα διακομιστή που ονομάζεται Server01. Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα ResourceGroup1. 

> [AZURE.NOTE] Σημειώστε ότι εάν ο διακομιστής σας είναι foo.database.windows.net, χρησιμοποιήστε τη "foo" ως όνομα_διακομιστή - στο τα cmdlet του PowerShell.

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Μια παραλλαγή, αυτό το παράδειγμα επόμενη ανακτά τη βάση δεδομένων στο αντικείμενο $database. Το σωλήνες, στη συνέχεια, να [Αναστολής AzureRmSqlDatabase][]το αντικείμενο. Τα αποτελέσματα είναι αποθηκευμένα στο resultDatabase το αντικείμενο. Η τελική εντολή εμφανίζει τα αποτελέσματα.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Βιογραφικό σημείωμα υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Για να ξεκινήσετε μια βάση δεδομένων, χρησιμοποιήστε το cmdlet [AzureRmSqlDatabase βιογραφικό σημείωμα][] . Το παρακάτω παράδειγμα ξεκινά μια βάση δεδομένων με το όνομα Database02 φιλοξενούνται σε ένα διακομιστή που ονομάζεται Server01. Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Μια παραλλαγή, αυτό το παράδειγμα επόμενη ανακτά τη βάση δεδομένων στο αντικείμενο $database. Στη συνέχεια, σωλήνες να [Βιογραφικού σημειώματος AzureRmSqlDatabase][] το αντικείμενο και αποθηκεύει τα αποτελέσματα σε $resultDatabase. Η τελική εντολή εμφανίζει τα αποτελέσματα.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Επόμενα βήματα

Για άλλες εργασίες διαχείρισης, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Επισκόπηση της διαχείρισης]: ./sql-data-warehouse-overview-manage.md
[Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure]: ./powershell-install-configure.md
[Διαχείριση Επισκόπηση υπολογισμού]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Βιογραφικό σημείωμα AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Αναστολή AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Ορισμός AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Πρόγραμμα εγκατάστασης πλατφόρμας Microsoft στο Web]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/

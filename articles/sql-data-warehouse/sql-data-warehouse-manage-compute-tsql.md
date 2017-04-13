<properties
   pageTitle="Διαχείριση power υπολογισμού στο Azure SQL δεδομένων αποθήκης (REST) | Microsoft Azure"
   description="Εργασίες Transact-SQL (T-SQL) για να κλιμάκωσης επιδόσεων, προσαρμόζοντας DWUs. Αποθήκευση κόστους, αλλάζοντας την κλίμακα πίσω κατά τη διάρκεια μη κορύφωσης φορές."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Διαχείριση ενέργειας υπολογισμού σε αποθήκη δεδομένων SQL Azure (T-SQL)

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-data-warehouse-manage-compute-overview.md)
- [Πύλη](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ΥΠΌΛΟΙΠΟ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Κλίμακα επιδόσεων, αλλάζοντας την κλίμακα ανάληψη υπολογιστική πόρους και μνήμης για να καλύψει τις μεταβαλλόμενες απαιτήσεις του φόρτου εργασίας σας. Αποθήκευση κόστους από τους πόρους κλίμακας πίσω κατά τη διάρκεια μη κορύφωσης ώρες ή παύση υπολογισμού εντελώς. 

Αυτή η συλλογή εργασίες χρησιμοποιεί T-SQL για να:

- Ρυθμίσεις DWU τρέχουσας προβολής
- Αλλαγή υπολογισμού πόρους, προσαρμόζοντας DWUs

Παύση ή συνέχιση μιας βάσης δεδομένων, επιλέξτε μία από τις άλλες επιλογές πλατφόρμα στο επάνω μέρος αυτού του άρθρου.

Για να μάθετε σχετικά με αυτό, ανατρέξτε στο θέμα [Διαχείριση υπολογιστική ισχύ Επισκόπηση][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Ρυθμίσεις DWU τρέχουσας προβολής

Για να προβάλετε τις τρέχουσες ρυθμίσεις DWU για τις βάσεις δεδομένων:

1. Ανοίξτε την Εξερεύνηση αντικειμένου SQL Server στο Visual Studio 2015.
2. Σύνδεση με την κύρια βάση δεδομένων που σχετίζονται με τη λογική διακομιστή βάσης δεδομένων SQL.
2. Επιλέξτε από την προβολή δυναμική διαχείριση sys.database_service_objectives. Ακολουθεί ένα παράδειγμα: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Κλίμακα υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Για να αλλάξετε το DWUs:


1. Σύνδεση με την κύρια βάση δεδομένων που σχετίζονται με τις λογικές διακομιστή βάσης δεδομένων SQL.
2. Χρησιμοποιήστε την πρόταση TSQL [ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ][] . Το παρακάτω παράδειγμα ορίζει το στόχο επιπέδου υπηρεσίας DW1000 για τη βάση δεδομένων MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Επόμενα βήματα

Για άλλες εργασίες διαχείρισης, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Επισκόπηση της διαχείρισης]: ./sql-data-warehouse-overview-manage.md
[Διαχείριση Επισκόπηση power υπολογισμού]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ΤΡΟΠΟΠΟΊΗΣΗ ΒΆΣΗΣ ΔΕΔΟΜΈΝΩΝ]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

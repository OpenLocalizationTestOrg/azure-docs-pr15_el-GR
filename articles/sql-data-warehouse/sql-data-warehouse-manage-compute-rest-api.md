<properties
   pageTitle="Διαχείριση power υπολογισμού στο Azure SQL δεδομένων αποθήκης (REST) | Microsoft Azure"
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Διαχείριση power υπολογισμού στο Azure SQL δεδομένων αποθήκης (REST)

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

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Κλίμακα power υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Για να αλλάξετε το DWUs, χρησιμοποιήστε το REST API [Δημιουργία ή ενημέρωση βάσης δεδομένων][] . Το παρακάτω παράδειγμα ορίζει το στόχο επιπέδου υπηρεσίας DW1000 για τη βάση δεδομένων MySQLDW που φιλοξενείται σε διακομιστή MyServer. Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Παύση υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Για να διακόψετε προσωρινά μια βάση δεδομένων, χρησιμοποιήστε το REST API [Παύση βάσης δεδομένων][] . Το παρακάτω παράδειγμα διακόπτει προσωρινά μια βάση δεδομένων με το όνομα Database02 φιλοξενούνται σε ένα διακομιστή που ονομάζεται Server01. Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Βιογραφικό σημείωμα υπολογισμού

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Για να ξεκινήσετε μια βάση δεδομένων, χρησιμοποιήστε το REST API [Βιογραφικού σημειώματος βάσης δεδομένων][] . Το παρακάτω παράδειγμα ξεκινά μια βάση δεδομένων με το όνομα Database02 φιλοξενούνται σε ένα διακομιστή που ονομάζεται Server01. Ο διακομιστής είναι σε μια ομάδα Azure πόρων με το όνομα ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Επόμενα βήματα

Για άλλες εργασίες διαχείρισης, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση της διαχείρισης]: ./sql-data-warehouse-overview-manage.md
[Διαχείριση Επισκόπηση υπολογισμού]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Παύση βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Βιογραφικό σημείωμα βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Δημιουργία ή ενημέρωση βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

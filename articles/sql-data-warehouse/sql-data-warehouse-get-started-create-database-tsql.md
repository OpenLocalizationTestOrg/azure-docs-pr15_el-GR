<properties
   pageTitle="Δημιουργήστε μια αποθήκη δεδομένων του SQL με TSQL | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια αποθήκη δεδομένων του SQL Azure με TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Δημιουργία βάσης δεδομένων SQL αποθήκη δεδομένων με τη χρήση Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Αυτό το άρθρο σας δείχνει πώς μπορείτε να δημιουργήσετε μια αποθήκη δεδομένων του SQL χρησιμοποιώντας T-SQL.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ξεκινήσετε, πρέπει: 

- **Λογαριασμός Azure**: επισκεφθείτε [Azure δωρεάν δοκιμαστική έκδοση][] ή [Πιστώσεων Azure MSDN][] για να δημιουργήσετε ένα λογαριασμό.
- **Azure SQL server**: ανατρέξτε στο θέμα [Δημιουργία μιας λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure][] ή [Δημιουργία μια λογική διακομιστή βάσης δεδομένων SQL Azure με το PowerShell][] για περισσότερες λεπτομέρειες.
- **Ομάδα πόρων**: είτε χρησιμοποιεί την ίδια ομάδα πόρων με το Azure SQL server ή δείτε [πώς μπορείτε να δημιουργήσετε μια ομάδα πόρων][].
- **Περιβάλλον για την εκτέλεση T-SQL**: μπορείτε να χρησιμοποιήσετε το [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]ή [SSMS][] να εκτελέσει T-SQL.

> [AZURE.NOTE] Δημιουργία μιας αποθήκη δεδομένων του SQL μπορεί να οδηγήσουν σε μια νέα υπηρεσία χρεώσιμων.  Δείτε [τις πληροφορίες τιμολόγησης αποθήκη δεδομένων του SQL][] για περισσότερες λεπτομέρειες σχετικά με τις πληροφορίες τιμολόγησης.

## <a name="create-a-database-with-visual-studio"></a>Δημιουργία βάσης δεδομένων με το Visual Studio

Εάν είστε νέος χρήστης του Visual Studio, ανατρέξτε στο άρθρο [(Visual Studio) αποθήκη δεδομένων του SQL Azure ερωτήματος][].  Για να ξεκινήσετε, ανοίξτε την Εξερεύνηση αντικειμένου SQL Server στο Visual Studio και συνδεθείτε με το διακομιστή που θα φιλοξενήσει τη βάση δεδομένων SQL αποθήκη δεδομένων.  Αφού συνδεθεί, μπορείτε να δημιουργήσετε μια αποθήκη δεδομένων του SQL, εκτελέστε την παρακάτω εντολή SQL σε σχέση με την **κύρια** βάση δεδομένων.  Αυτή η εντολή δημιουργεί τη βάση δεδομένων MySqlDwDb με μια υπηρεσία στόχο DW400 και να επιτρέψετε τη βάση δεδομένων θα αυξηθεί στο μέγιστο μέγεθος 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Δημιουργία βάσης δεδομένων με sqlcmd

Εναλλακτικά, μπορείτε να εκτελέσετε την ίδια εντολή με sqlcmd, εκτελώντας τα ακόλουθα σε μια γραμμή εντολών.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Η προεπιλεγμένη ταξινόμηση όταν δεν έχει καθοριστεί είναι ΣΥΡΡΑΦΉ SQL_Latin1_General_CP1_CI_AS.  Το `MAXSIZE` μπορεί να είναι μεταξύ 250 GB και 240 TB.  Το `SERVICE_OBJECTIVE` μπορεί να είναι μεταξύ DW100 και DW2000 [DWU][].  Για μια λίστα με όλες τις έγκυρες τιμές, ανατρέξτε στην τεκμηρίωση MSDN για [ΔΗΜΙΟΥΡΓΊΑ βάσης ΔΕΔΟΜΈΝΩΝ][].  Τόσο η MAXSIZE και SERVICE_OBJECTIVE μπορεί να αλλάξει με μια εντολή T SQL [ΤΡΟΠΟΠΟΊΗΣΗ βάσης ΔΕΔΟΜΈΝΩΝ][] .  Η συρραφή της βάσης δεδομένων δεν μπορεί να αλλάξει μετά τη δημιουργία της.   Είστε προσεκτικοί πρέπει να χρησιμοποιείται κατά την αλλαγή του SERVICE_OBJECTIVE με την αλλαγή DWU προκαλεί την επανεκκίνηση των υπηρεσιών, οποίες ακυρώνει όλα τα ερωτήματα στην πτήσεων.  Αλλαγή MAXSIZE επανεκκίνηση υπηρεσίες όπως είναι απλώς μια λειτουργία απλό μετα-δεδομένων.

## <a name="next-steps"></a>Επόμενα βήματα

Αφού ολοκληρωθεί η σας αποθήκη δεδομένων του SQL προμήθεια μπορείτε να [φορτώσετε το δείγμα δεδομένων][] ή να δείτε πώς μπορείτε να [αναπτύξετε][], [Φόρτωση][]ή [μετεγκατάσταση][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Αποθήκη δεδομένων του ερωτήματος Azure SQL (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[μετεγκατάσταση]: sql-data-warehouse-overview-migrate.md
[ανάπτυξη]: sql-data-warehouse-overview-develop.md
[φόρτωση]: sql-data-warehouse-overview-load.md
[φόρτωση του δείγματος δεδομένων]: sql-data-warehouse-load-sample-databases.md
[Δημιουργήστε μια λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη του Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Δημιουργήστε μια λογική διακομιστή βάσης δεδομένων SQL Azure με το PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[πώς μπορείτε να δημιουργήσετε μια ομάδα πόρων]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[SQLCMD]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[ΔΗΜΙΟΥΡΓΊΑ ΒΆΣΗΣ ΔΕΔΟΜΈΝΩΝ]: https://msdn.microsoft.com/library/mt204021.aspx
[ΤΡΟΠΟΠΟΊΗΣΗ ΒΆΣΗΣ ΔΕΔΟΜΈΝΩΝ]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[Τις τιμές αποθήκη δεδομένων του SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure δωρεάν δοκιμαστική έκδοση]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure πιστώσεων]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

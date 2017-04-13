<properties
   pageTitle="Δημιουργία αποθήκη δεδομένων του SQL με χρήση του PowerShell | Microsoft Azure"
   description="Δημιουργία αποθήκη δεδομένων του SQL με χρήση του PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Δημιουργία αποθήκη δεδομένων του SQL με χρήση του PowerShell

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να δημιουργήσετε μια αποθήκη δεδομένων του SQL με χρήση του PowerShell.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ξεκινήσετε, πρέπει:

- **Λογαριασμός Azure**: επισκεφθείτε [Azure δωρεάν δοκιμαστική έκδοση][] ή [Πιστώσεων Azure MSDN][] για να δημιουργήσετε ένα λογαριασμό.
- **Azure SQL server**: ανατρέξτε στο θέμα [Δημιουργία μιας λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure][] ή [Δημιουργία μια λογική διακομιστή βάσης δεδομένων SQL Azure με το PowerShell][] για περισσότερες λεπτομέρειες.
- **Ομάδα πόρων**: είτε χρησιμοποιεί την ίδια ομάδα πόρων με το Azure SQL server ή δείτε [πώς μπορείτε να δημιουργήσετε μια ομάδα πόρων][].
- **PowerShell έκδοση 1.0.3 ή μεγαλύτερη**: μπορείτε να ελέγξετε την έκδοση, εκτελώντας **Get-λειτουργική μονάδα - ListAvailable-Azure όνομα**.  Την πιο πρόσφατη έκδοση μπορεί να εγκατασταθεί από το [Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμας][].  Για περισσότερες πληροφορίες σχετικά με την εγκατάσταση της πιο πρόσφατης έκδοσης, δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure][].

> [AZURE.NOTE] Δημιουργία μιας αποθήκη δεδομένων του SQL μπορεί να οδηγήσουν σε μια νέα υπηρεσία χρεώσιμων.  Δείτε [τις πληροφορίες τιμολόγησης αποθήκη δεδομένων του SQL][] για περισσότερες λεπτομέρειες σχετικά με τις πληροφορίες τιμολόγησης.

## <a name="create-a-sql-data-warehouse"></a>Δημιουργήστε μια αποθήκη δεδομένων SQL

1. Άνοιγμα του Windows PowerShell.
2. Εκτελέστε αυτό το cmdlet για να σύνδεσης για τη διαχείριση πόρων Azure.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Επιλέξτε τη συνδρομή που θέλετε να χρησιμοποιήσετε για την τρέχουσα περίοδο λειτουργίας.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Δημιουργία βάσης δεδομένων. Αυτό το παράδειγμα δημιουργεί μια βάση δεδομένων με το όνομα "mynewsqldw", με το επίπεδο στόχου υπηρεσίας "DW400", στο διακομιστή με το όνομα "sqldwserver1", που βρίσκεται στην ομάδα πόρων με το όνομα "mywesteuroperesgp1".

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Απαιτούμενες παράμετροι είναι οι εξής:

- **RequestedServiceObjectiveName**: το ποσό της [DWU][] που ζητάτε.  Υποστηριζόμενες τιμές είναι: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 και DW6000.
- **Όνομα βάσης δεδομένων**: το όνομα του αποθήκη δεδομένων SQL που θέλετε να δημιουργήσετε.
- **Όνομα διακομιστή**: το όνομα του διακομιστή που χρησιμοποιείτε για τη δημιουργία (πρέπει να είναι V12).
- **ResourceGroupName**: ομάδα πόρων που χρησιμοποιείτε.  Για να βρείτε διαθέσιμες πόρων ομάδες στη συνδρομή σας, χρησιμοποιήστε Get-AzureResource.
- **Έκδοση**: πρέπει να είναι "DataWarehouse" για να δημιουργήσετε μια αποθήκη δεδομένων του SQL.

Προαιρετικές παράμετροι είναι οι εξής:

- **CollationName**: SQL_Latin1_General_CP1_CI_AS είναι η προεπιλεγμένη ταξινόμηση, εάν δεν έχει καθοριστεί.  Συρραφή δεν μπορεί να αλλάξει σε μια βάση δεδομένων.
- **MaxSizeBytes**: το προεπιλεγμένο μέγιστο μέγεθος μιας βάσης δεδομένων είναι 10 GB.


Για περισσότερες λεπτομέρειες σχετικά με τις επιλογές παραμέτρων, ανατρέξτε στο θέμα [Δημιουργία AzureRmSqlDatabase][] και [Δημιουργία βάσης δεδομένων (αποθήκη δεδομένων SQL Azure)][].

## <a name="next-steps"></a>Επόμενα βήματα

Αφού ολοκληρωθεί η προμήθεια σας αποθήκη δεδομένων του SQL που μπορεί να θέλετε να προσπαθήσετε να [φορτώσετε το δείγμα δεδομένων][] ή να δείτε πώς μπορείτε να [αναπτύξετε][], [Φόρτωση][]ή [μετεγκατάσταση][].

Εάν σας ενδιαφέρει περισσότερες πληροφορίες σχετικά με τον τρόπο διαχείρισης αποθήκη δεδομένων του SQL μέσω προγραμματισμού, δείτε το άρθρο σχετικά με τη χρήση [των cmdlet του PowerShell και REST API του Yammer][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[μετεγκατάσταση]: ./sql-data-warehouse-overview-migrate.md
[ανάπτυξη]: ./sql-data-warehouse-overview-develop.md
[φόρτωση]: ./sql-data-warehouse-load-with-bcp.md
[φόρτωση δείγμα δεδομένων]: ./sql-data-warehouse-load-sample-databases.md
[Cmdlet του PowerShell και REST API του Yammer]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Δημιουργήστε μια λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη του Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Δημιουργήστε μια λογική διακομιστή βάσης δεδομένων SQL Azure με το PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[πώς μπορείτε να δημιουργήσετε μια ομάδα πόρων]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Νέα AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Δημιουργία βάσης δεδομένων (αποθήκη δεδομένων του Azure SQL)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Πρόγραμμα εγκατάστασης πλατφόρμας Microsoft στο Web]: https://aka.ms/webpi-azps
[Τις τιμές αποθήκη δεδομένων του SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure δωρεάν δοκιμαστική έκδοση]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure πιστώσεων]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

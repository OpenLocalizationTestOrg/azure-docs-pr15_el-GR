<properties
   pageTitle="Δημιουργήστε μια αποθήκη δεδομένων του SQL στην πύλη του Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια αποθήκη δεδομένων του SQL Azure στην πύλη του Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Δημιουργήστε μια αποθήκη δεδομένων του Azure SQL

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί την πύλη του Azure για να δημιουργήσετε μια αποθήκη δεδομένων του SQL που περιέχει ένα δείγμα βάσης δεδομένων AdventureWorksDW.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ξεκινήσετε, πρέπει:

- **Λογαριασμός Azure**: επισκεφθείτε [Azure δωρεάν δοκιμαστική έκδοση][] ή [Πιστώσεων Azure MSDN][] για να δημιουργήσετε ένα λογαριασμό.
- **Azure SQL server**: ανατρέξτε στο θέμα [Δημιουργία μιας λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη του Azure][] για περισσότερες λεπτομέρειες.

> [AZURE.NOTE] Δημιουργία μιας αποθήκη δεδομένων του SQL ενδέχεται να έχει ως αποτέλεσμα μια νέα υπηρεσία χρεώσιμων.  Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [τις τιμές αποθήκη δεδομένων του SQL][] .

## <a name="create-a-sql-data-warehouse"></a>Δημιουργήστε μια αποθήκη δεδομένων SQL

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com).

2. Κάντε κλικ στο κουμπί **+ νέο** > **δεδομένων + χώρος αποθήκευσης** > **αποθήκη δεδομένων του SQL**.

    ![Δημιουργία](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. Στο το blade **Αποθήκη δεδομένων του SQL** , συμπληρώστε τις πληροφορίες είναι απαραίτητο, στη συνέχεια, πατήστε το πλήκτρο 'Δημιουργία' για να δημιουργήσετε.

    ![Δημιουργία βάσης δεδομένων](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Διακομιστής**: συνιστάται να επιλέξετε πρώτα το διακομιστή σας.  

    - **Όνομα βάσης δεδομένων**: το όνομα που χρησιμοποιείται για την αναφορά αποθήκη δεδομένων SQL.  Πρέπει να είναι μοναδικό στο διακομιστή.
    
    - **Απόδοση**: συνιστάται να ξεκινούν με 400 [DWUs][DWU]. Μπορείτε να μετακινήσετε το ρυθμιστικό προς τα αριστερά ή δεξιά για να ρυθμίσετε τις επιδόσεις της σας αποθήκη δεδομένων, ή την κλίμακα προς τα επάνω ή προς τα κάτω μετά τη δημιουργία της.  Για να μάθετε περισσότερα σχετικά με το DWUs, ανατρέξτε στο θέμα την τεκμηρίωση στο [κλίμακας](./sql-data-warehouse-manage-compute-overview.md) ή μας [τις τιμές σελίδα][Τιμολόγηση αποθήκη δεδομένων του SQL]. 

    - **Συνδρομή**: Επιλέξτε τη [συνδρομή] στην οποία θα τιμολόγησης αυτό αποθήκη δεδομένων του SQL.

    - **Ομάδα πόρων**: [ομάδες πόρων] [ Resource group] είναι κοντέινερ που έχει σχεδιαστεί για να σας βοηθήσει να διαχειρίζεστε συλλογές Azure πόρων. Μάθετε περισσότερα σχετικά με τις [ομάδες πόρων](../azure-resource-manager/resource-group-overview.md).

    - **Επιλέξτε προέλευση**: κάντε κλικ στην επιλογή **Επιλέξτε προέλευση** > **δείγμα**. Azure συμπληρώνει αυτόματα την επιλογή **Επιλογή δείγματος** με AdventureWorksDW.

> [AZURE.NOTE] Η προεπιλεγμένη ταξινόμηση για μια αποθήκη δεδομένων του SQL είναι SQL_Latin1_General_CP1_CI_AS. Αν μια διαφορετική συρραφή είναι απαραίτητο, [T-SQL][] μπορεί να χρησιμοποιηθεί για τη δημιουργία της βάσης δεδομένων με μια διαφορετική συρραφή.

4. Κάντε κλικ στην επιλογή **Δημιουργία** για να δημιουργήσετε την αποθήκη δεδομένων του SQL.

5. Περιμένετε λίγα λεπτά. Όταν σας αποθήκη δεδομένων είναι έτοιμη, πρέπει να επιστρέφεται στην [πύλη του Azure](https://portal.azure.com). Μπορείτε να βρείτε την αποθήκη δεδομένων του SQL στον πίνακα εργαλείων σας, που αναφέρονται στην περιοχή τις βάσεις δεδομένων SQL ή στην ομάδα πόρων που χρησιμοποιήσατε για τη δημιουργία του. 

    ![Προβολή πύλης](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει μια αποθήκη δεδομένων του SQL, είστε έτοιμοι για [σύνδεση](./sql-data-warehouse-connect-overview.md) και ξεκινήσετε την υποβολή ερωτημάτων.

Για να φορτώσετε δεδομένα σε αποθήκη δεδομένων του SQL, ανατρέξτε στο θέμα η [Φόρτωση Επισκόπηση](./sql-data-warehouse-overview-load.md).

Εάν προσπαθείτε να μετεγκαταστήσετε μια υπάρχουσα βάση δεδομένων σε αποθήκη δεδομένων του SQL, ανατρέξτε στο θέμα η [Επισκόπηση μετεγκατάστασης](./sql-data-warehouse-overview-migrate.md) ή χρησιμοποιήστε [Βοηθητικού προγράμματος μετεγκατάστασης](./sql-data-warehouse-migrate-migration-utility.md).

Κανόνες τείχους προστασίας μπορεί να ρυθμιστεί επίσης μέσω Transact-SQL. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [sp_set_firewall_rule][] και [sp_set_database_firewall_rule][].

Είναι επίσης μια καλή ιδέα να δούμε τις [βέλτιστες πρακτικές][].

<!--Article references-->
[Δημιουργήστε μια λογική διακομιστή βάσης δεδομένων SQL Azure με την πύλη του Azure]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Βέλτιστες πρακτικές]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[συνδρομή]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[Τις τιμές αποθήκη δεδομένων του SQL]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure δωρεάν δοκιμαστική έκδοση]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure πιστώσεων]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F


<properties
   pageTitle="Κρυπτογράφηση διαφανή δεδομένων σε αποθήκη δεδομένων του SQL (T-SQL) | Microsoft Azure"
   description="Κρυπτογράφηση διαφανή δεδομένων (TDE) σε αποθήκη δεδομένων του SQL (T-SQL)"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016"
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="get-started-with-transparent-data-encryption-tde"></a>Γρήγορα αποτελέσματα με διαφανή κρυπτογράφησης δεδομένων (TDE)


> [AZURE.SELECTOR]
- [Επισκόπηση ασφαλείας](sql-data-warehouse-overview-manage-security.md)
- [Έλεγχος ταυτότητας](sql-data-warehouse-authentication.md)
- [Κρυπτογράφηση (πύλη)](sql-data-warehouse-encryption-tde.md)
- [Κρυπτογράφηση (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

## <a name="required-permssions"></a>Απαιτούμενα δικαιώματα

Για να ενεργοποιήσετε διαφανή κρυπτογράφησης δεδομένων (TDE), πρέπει να είστε διαχειριστής ή μέλος του ρόλου dbmanager.

## <a name="enabling-encryption"></a>Ενεργοποίηση της κρυπτογράφησης

Ακολουθήστε τα παρακάτω βήματα για να ενεργοποιήσετε TDE για μια αποθήκη δεδομένων του SQL:

1. Σύνδεση με την *κύρια* βάση δεδομένων στο διακομιστή που φιλοξενεί τη βάση δεδομένων με χρήση μιας σύνδεσης που είναι ο διαχειριστής ή μέλος του ρόλου **dbmanager** της κύριας βάσης δεδομένων
2. Εκτελέστε την ακόλουθη δήλωση για να κρυπτογραφήσετε τη βάση δεδομένων.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

## <a name="disabling-encryption"></a>Απενεργοποίηση της κρυπτογράφησης

Ακολουθήστε τα παρακάτω βήματα για να απενεργοποιήσετε TDE για μια αποθήκη δεδομένων του SQL:

1. Σύνδεση με την *κύρια* βάση δεδομένων με χρήση μιας σύνδεσης που είναι ο διαχειριστής ή μέλος του ρόλου **dbmanager** στην κύρια βάση δεδομένων
2. Εκτελέστε την ακόλουθη δήλωση για να κρυπτογραφήσετε τη βάση δεδομένων.

```sql
ALTER DATABASE [AdventureWorks] SET ENCRYPTION OFF;
```

> [AZURE.NOTE] Πρέπει να είναι δυνατή η συνέχιση μιας παύσης αποθήκη δεδομένων του SQL πριν να κάνετε αλλαγές στις ρυθμίσεις TDE.

## <a name="verifying-encryption"></a>Επαλήθευση κρυπτογράφησης

Για να επαληθεύσετε την κατάσταση κρυπτογράφησης για μια αποθήκη δεδομένων του SQL, ακολουθήστε τα παρακάτω βήματα:

1. Σύνδεση με το *πρωτότυπο* ή παρουσία βάση δεδομένων με χρήση μιας σύνδεσης που είναι ο διαχειριστής ή μέλος του ρόλου **dbmanager** στην κύρια βάση δεδομένων
2. Εκτελέστε την ακόλουθη δήλωση για να κρυπτογραφήσετε τη βάση δεδομένων.

```sql
SELECT
    [name],
    [is_encrypted]
FROM
    sys.databases;
```

Αποτέλεσμα της ```1``` υποδεικνύει κρυπτογραφημένης βάσης δεδομένων, ```0``` υποδεικνύει μια βάση δεδομένων μη κρυπτογραφημένα.

## <a name="encryption-dmvs"></a>Κρυπτογράφηση DMVs  

- [sys.Databases][] 
- [sys.dm_pdw_nodes_database_encryption_keys][]


<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx  
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx  

<!--Image references-->

<!--Link references-->

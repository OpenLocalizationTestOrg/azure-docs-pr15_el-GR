<properties
   pageTitle="Ενεργοποίηση της κρυπτογράφησης διαφανή δεδομένων (TDE) για επέκταση βάση δεδομένων SQL Server σε Azure TSQL | Microsoft Azure"
   description="Ενεργοποίηση της κρυπτογράφησης διαφανή δεδομένων (TDE) για επέκταση βάση δεδομένων SQL Server σε Azure TSQL"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure-transact-sql"></a>Ενεργοποίηση κρυπτογράφηση διαφανή δεδομένων (TDE) για επέκταση βάσης δεδομένων σε Azure (Transact-SQL)
> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Διαφανή κρυπτογράφησης δεδομένων (TDE) συμβάλλει στην προστασία υπό την απειλή κακόβουλης δραστηριότητας, εκτελώντας σε πραγματικό χρόνο κρυπτογράφηση και αποκρυπτογράφηση τη βάση δεδομένων, συσχετισμένη αντίγραφα ασφαλείας και αρχεία καταγραφής συναλλαγών στο υπόλοιπο χωρίς να απαιτείται αλλαγές στην εφαρμογή.

TDE κρυπτογραφεί την αποθήκευση μιας ολόκληρης βάσης δεδομένων με τη χρήση συμμετρική κλειδιού που ονομάζεται του κλειδιού κρυπτογράφησης βάσης δεδομένων. Το κλειδί κρυπτογράφηση βάσης δεδομένων προστατεύεται από ένα πιστοποιητικό διακομιστή ενσωματωμένα. Το πιστοποιητικό διακομιστή ενσωματωμένη είναι μοναδικός για κάθε Azure διακομιστή. Microsoft περιστρέφει αυτόματα αυτά τα πιστοποιητικά τουλάχιστον κάθε 90 ημέρες. Για μια γενική περιγραφή της TDE, ανατρέξτε στο θέμα [Διαφανή κρυπτογράφησης δεδομένων (TDE)].

##<a name="enabling-encryption"></a>Ενεργοποίηση της κρυπτογράφησης

Για να ενεργοποιήσετε TDE για μια Azure μετεγκατάσταση βάσης δεδομένων που αποθηκεύει τα δεδομένα από μια βάση δεδομένων SQL Server με δυνατότητα παραμόρφωση, κάντε τα εξής στοιχεία:

1. Σύνδεση με την *κύρια* βάση δεδομένων στο Azure διακομιστή που φιλοξενεί τη βάση δεδομένων με χρήση μιας σύνδεσης που είναι ο διαχειριστής ή μέλος του ρόλου **dbmanager** στην κύρια βάση δεδομένων
2. Εκτελέστε την ακόλουθη δήλωση για να κρυπτογραφήσετε τη βάση δεδομένων.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION ON;
```

##<a name="disabling-encryption"></a>Απενεργοποίηση της κρυπτογράφησης

Για να απενεργοποιήσετε TDE για μια Azure μετεγκατάσταση βάσης δεδομένων που αποθηκεύει τα δεδομένα από μια βάση δεδομένων SQL Server με δυνατότητα παραμόρφωση, κάντε τα εξής στοιχεία:

1. Σύνδεση με την *κύρια* βάση δεδομένων με χρήση μιας σύνδεσης που είναι ο διαχειριστής ή μέλος του ρόλου **dbmanager** στην κύρια βάση δεδομένων
2. Εκτελέστε την ακόλουθη δήλωση για να κρυπτογραφήσετε τη βάση δεδομένων.

```sql
ALTER DATABASE [database_name] SET ENCRYPTION OFF;
```

##<a name="verifying-encryption"></a>Επαλήθευση κρυπτογράφησης

Για να επαληθεύσετε μετεγκατασταθεί κατάστασης κρυπτογράφηση μιας βάσης δεδομένων Azure που αποθηκεύει τα δεδομένα από μια βάση δεδομένων SQL Server με δυνατότητα παραμόρφωση, κάντε τα εξής στοιχεία:

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


<!--Anchors-->
[Κρυπτογράφηση διαφανή δεδομένων (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->

<!--Link references-->

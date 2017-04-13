<properties
   pageTitle="Ερώτημα αποθήκη δεδομένων SQL Azure (sqlcmd) | Microsoft Azure"
   description="Υποβολή ερωτημάτων αποθήκη δεδομένων του SQL Azure με το βοηθητικό πρόγραμμα γραμμής εντολών sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>

# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Ερώτημα αποθήκη δεδομένων SQL Azure (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure μηχανικής εκμάθησης](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Αυτόν τον Οδηγό χρησιμοποιεί το βοηθητικό πρόγραμμα γραμμής εντολών [sqlcmd][] ερώτημα για μια αποθήκη δεδομένων του SQL Azure.  

## <a name="1-connect"></a>1. σύνδεση

Για να ξεκινήσετε με [sqlcmd][], ανοίξτε τη γραμμή εντολών και πληκτρολογήστε **sqlcmd** ακολουθούμενο από τη συμβολοσειρά σύνδεσης για τη βάση δεδομένων SQL Data Warehouse. Η συμβολοσειρά σύνδεσης απαιτεί τις ακόλουθες παραμέτρους:

+ **Διακομιστή (-S):** Διακομιστής στη φόρμα `<`το όνομα του διακομιστή`>`. database.windows.net
+ **Βάση δεδομένων (-δ):** Όνομα βάσης δεδομένων.
+ **Ενεργοποίηση εισαγωγικά αναγνωριστικά (-να):** Αναγνωριστικά σε εισαγωγικά, πρέπει να είναι ενεργοποιημένη για να συνδεθείτε με μια παρουσία αποθήκη δεδομένων του SQL.

Για να χρησιμοποιήσετε τον έλεγχο ταυτότητας του SQL Server, πρέπει να προσθέσετε τις παραμέτρους username/τον κωδικό πρόσβασης:

+ **Χρήστη (-U):** Διακομιστής χρήστη στη φόρμα `<`χρήστη`>`
+ **Τον κωδικό πρόσβασης (-P):** Ο κωδικός πρόσβασης που σχετίζεται με το χρήστη.

Για παράδειγμα, η συμβολοσειρά σύνδεσης μπορεί να είναι όπως τα εξής:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Για να χρησιμοποιήσετε Azure Active Directory ενσωματωμένο έλεγχο ταυτότητας, πρέπει να προσθέσετε τις παραμέτρους Azure Active Directory:

+ **Ελέγχου ταυτότητας azure Active Directory (-G):** Χρησιμοποιήστε Azure Active Directory για τον έλεγχο ταυτότητας

Για παράδειγμα, η συμβολοσειρά σύνδεσης μπορεί να είναι όπως τα εξής:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Πρέπει να [ενεργοποιήσετε Azure Active Directory τον έλεγχο ταυτότητας](sql-data-warehouse-authentication.md) για τον έλεγχο ταυτότητας με χρήση της υπηρεσίας καταλόγου Active Directory.

## <a name="2-query"></a>2. ερώτημα

Μετά τη σύνδεση, μπορείτε να εκδώσετε οποιοδήποτε υποστηριζόμενο προτάσεις Transact-SQL σε σχέση με την παρουσία.  Σε αυτό το παράδειγμα, υποβάλλονται τα ερωτήματα σε αλληλεπιδραστική κατάσταση λειτουργίας.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Αυτά τα επόμενα παραδείγματα δείχνουν πώς μπορείτε να εκτελέσετε τα ερωτήματά σας σε κατάσταση λειτουργίας δέσμης χρησιμοποιώντας την επιλογή -Q ή σωληνώσεων σας SQL για να sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τις λεπτομέρειες σχετικά με τις επιλογές που είναι διαθέσιμες στο sqlcmd, ανατρέξτε στο θέμα [τεκμηρίωση sqlcmd][sqlcmd] .

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[SQLCMD]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->

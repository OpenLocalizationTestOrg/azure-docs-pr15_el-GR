<properties
   pageTitle="Φόρτωση του δείγματος δεδομένων σε αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Φόρτωση του δείγματος δεδομένων σε αποθήκη δεδομένων του SQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Φόρτωση του δείγματος δεδομένων σε αποθήκη δεδομένων του SQL

Ακολουθήστε αυτά τα απλά βήματα για τη φόρτωση και ερωτήματος Adventure Works δείγμα βάσης δεδομένων. Αυτές οι δέσμες ενεργειών πρώτα να χρησιμοποιήσετε το sqlcmd για να εκτελέσετε SQL που θα δημιουργήσετε πίνακες και προβολές. Αφού έχουν δημιουργηθεί πίνακες, τις δέσμες ενεργειών θα χρησιμοποιήσει bcp για τη φόρτωση των δεδομένων.  Εάν δεν έχετε ήδη sqlcmd και bcp εγκατασταθεί, ακολουθήστε αυτές τις συνδέσεις για να [εγκαταστήσετε bcp][] και να [εγκαταστήσετε το sqlcmd][].

##<a name="load-sample-data"></a>Φόρτωση του δείγματος δεδομένων

1. Κάντε λήψη του αρχείου zip [Adventure Works δείγμα δέσμης ενεργειών για αποθήκη δεδομένων του SQL][] .

2. Εξαγάγετε τα αρχεία από ληφθέντων zip σε έναν κατάλογο στον τοπικό υπολογιστή σας.

3. Επεξεργαστείτε το αρχείο που έχει εξαχθεί aw_create.bat και ορίστε τις παρακάτω μεταβλητές που βρέθηκε στο επάνω μέρος του αρχείου.  Φροντίστε να αφήσετε χωρίς κενό διάστημα μεταξύ του "=" και την παράμετρο.  Ακολουθούν παραδείγματα των πώς μπορεί να δείτε τις αλλαγές που θέλετε.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Από μια cmd εντολών των Windows, εκτελέστε το επεξεργασμένο aw_create.bat.  Βεβαιωθείτε ότι βρίσκεστε στον κατάλογο όπου αποθηκεύσατε το επεξεργασμένο έκδοση του aw_create.bat.
Αυτή η δέσμη ενεργειών θα...
    * Απόθεση Adventure Works πίνακες ή προβολές που υπάρχουν ήδη στη βάση δεδομένων σας
    * Δημιουργήστε την Adventure Works πίνακες και προβολές
    * Φόρτωση κάθε πίνακα Adventure Works με χρήση bcp
    * Επικυρώστε τις μετρήσεις γραμμή για κάθε πίνακα Adventure Works
    * Συλλογή στατιστικών στοιχείων σε κάθε στήλη για κάθε πίνακα Adventure Works


##<a name="query-sample-data"></a>Δείγμα δεδομένων ερωτήματος

Όταν θα έχετε φορτώνεται μερικά δείγματα δεδομένων σε σας αποθήκη δεδομένων του SQL, μπορείτε να εκτελέσετε γρήγορα μερικά ερωτήματα.  Για να εκτελέσετε ένα ερώτημα, συνδεθείτε στη βάση δεδομένων που έχουν δημιουργηθεί πρόσφατα Adventure Works στο DW SQL Azure με χρήση του Visual Studio και SSDT, όπως περιγράφεται στο έγγραφο [ερωτήματος με Visual Studio][] .

Παράδειγμα απλού πρόταση select για να λάβετε όλες τις πληροφορίες των εργαζομένων:

```sql
SELECT * FROM DimEmployee;
```

Παράδειγμα ένα πιο σύνθετο ερώτημα χρησιμοποιώντας δομές, όπως ΟΜΑΔΟΠΟΊΗΣΗ κατά για να δείτε το συνολικό ποσό για όλες τις πωλήσεις κάθε ημέρα:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Παράδειγμα μια, ΕΠΙΛΈΞΤΕ με έναν όρο WHERE για να φιλτράρεται παραγγελίες από πριν από μια συγκεκριμένη ημερομηνία:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Αποθήκη δεδομένων του SQL υποστηρίζει σχεδόν όλες τις δομές T-SQL που υποστηρίζει το SQL Server.  Οι διαφορές τεκμηριώνονται στο την [μετεγκατάσταση κώδικα][] τεκμηρίωση.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε είχατε ευκαιρία να τις δοκιμάσετε ορισμένα ερωτήματα με δείγμα δεδομένων, ανατρέξτε στο θέμα πώς μπορείτε να [αναπτύξετε][], [Φόρτωση][]ή [μετεγκατάσταση][] στο αποθήκη δεδομένων του SQL.

<!--Image references-->

<!--Article references-->
[μετεγκατάσταση]: sql-data-warehouse-overview-migrate.md
[ανάπτυξη]: sql-data-warehouse-overview-develop.md
[φόρτωση]: sql-data-warehouse-overview-load.md
[ερώτημα με Visual Studio]: sql-data-warehouse-query-visual-studio.md
[μετεγκατάσταση κώδικα]: sql-data-warehouse-migrate-code.md
[εγκατάσταση bcp]: sql-data-warehouse-load-with-bcp.md
[εγκατάσταση sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure λειτουργεί δείγματα δεσμών ενεργειών για αποθήκη δεδομένων του SQL]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip

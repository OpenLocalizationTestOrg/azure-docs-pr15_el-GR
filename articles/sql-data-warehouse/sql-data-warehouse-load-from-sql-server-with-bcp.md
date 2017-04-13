<properties
   pageTitle="Φόρτωση δεδομένων από SQL Server σε αποθήκη δεδομένων SQL Azure (bcp) | Microsoft Azure"
   description="Για το μέγεθος: μικρό δεδομένων χρησιμοποιεί bcp για να εξαγάγετε δεδομένα από SQL Server σε επίπεδο αρχείων και να εισαγάγετε τα δεδομένα απευθείας στο αποθήκη δεδομένων του SQL Azure."
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
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Φόρτωση δεδομένων από SQL Server σε αποθήκη δεδομένων SQL Azure (επίπεδο αρχεία)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Για μικρές σύνολα δεδομένων, μπορείτε να χρησιμοποιήσετε το βοηθητικό πρόγραμμα bcp γραμμής εντολών για να εξαγάγετε δεδομένα από το SQL Server και, στη συνέχεια, να το φορτώσετε απευθείας στο αποθήκη δεδομένων του SQL Azure.

Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσετε bcp για να:

- Εξαγωγή πίνακα από από SQL Server με χρήση του bcp εντολής (ή να δημιουργήσετε ένα απλό παράδειγμα αρχείου)
- Εισαγωγή πίνακα από ένα απλό αρχείο στο αποθήκη δεδομένων του SQL.
- Δημιουργήστε στατιστικά στοιχεία φόρτωση των δεδομένων.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να περιηγηθείτε στις αυτό το πρόγραμμα εκμάθησης, πρέπει:

- Μια βάση δεδομένων SQL αποθήκη δεδομένων
- Το βοηθητικό πρόγραμμα γραμμής εντολών bcp εγκατεστημένο
- Το βοηθητικό πρόγραμμα γραμμής εντολών sqlcmd εγκατεστημένο

Μπορείτε να κάνετε λήψη των βοηθητικών προγραμμάτων bcp και sqlcmd από το [Κέντρο λήψης της Microsoft][].

### <a name="data-in-ascii-or-utf-16-format"></a>Δεδομένα σε μορφή ASCII ή UTF-16

Εάν προσπαθείτε αυτό το πρόγραμμα εκμάθησης με τα δικά σας δεδομένα, τα δεδομένα σας πρέπει να χρησιμοποιήσετε την ASCII ή UTF-16 κωδικοποίηση εφόσον bcp δεν υποστηρίζει UTF-8. 

PolyBase υποστηρίζει UTF-8, αλλά ακόμα δεν υποστηρίζει UTF-16. Σημειώστε ότι, εάν θέλετε να συνδυάσετε bcp με PolyBase θα πρέπει να μετατρέψετε τα δεδομένα σε UTF-8 αφού εξάγεται από SQL Server. 


## <a name="1-create-a-destination-table"></a>1. δημιουργήσετε έναν πίνακα προορισμού

Ορισμός ενός πίνακα σε αποθήκη δεδομένων του SQL που θα είναι ο πίνακας προορισμού για τη φόρτωση. Οι στήλες του πίνακα πρέπει να αντιστοιχεί τα δεδομένα σε κάθε γραμμή του αρχείου δεδομένων.

Για να δημιουργήσετε έναν πίνακα, ανοίξτε μια γραμμή εντολών και χρήση sqlcmd.exe για να εκτελέσετε την ακόλουθη εντολή:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Δημιουργία αρχείου προέλευσης δεδομένων

Ανοίξτε το Σημειωματάριο και αντιγράψτε τις ακόλουθες γραμμές δεδομένων σε ένα νέο αρχείο κειμένου και, στη συνέχεια, να αποθηκεύσετε αυτό το αρχείο στον τοπικό προσωρινό κατάλογο, C:\Temp\DimDate2.txt. Αυτά τα δεδομένα είναι σε μορφή ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Προαιρετικό) Για να εξαγάγετε τα δικά σας δεδομένα από μια βάση δεδομένων SQL Server, ανοίξτε μια γραμμή εντολών και εκτελέστε την ακόλουθη εντολή. Αντικαταστήστε το όνομα πίνακα, όνομα διακομιστή, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης με τις δικές σας πληροφορίες.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. η φόρτωση των δεδομένων
Για να φορτώσετε τα δεδομένα, ανοίξτε μια γραμμή εντολών και εκτελέστε την παρακάτω εντολή, αντικαθιστώντας τις τιμές για το όνομα του διακομιστή, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης με τις πληροφορίες σας.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Χρησιμοποιήστε αυτή την εντολή για να επαληθεύσετε τα δεδομένα φορτώθηκε σωστά

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Τα αποτελέσματα θα πρέπει να μοιάζει ως εξής:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## <a name="4-create-statistics"></a>4. Δημιουργία στατιστικά στοιχεία

Αποθήκη δεδομένων του SQL κάνει δεν έχουν ακόμα υποστήριξη αυτόματης δημιουργίας ή αυτόματης ενημέρωσης στατιστικά στοιχεία. Για να λάβετε τις καλύτερες επιδόσεις ερωτημάτων, είναι σημαντικό να δημιουργήσετε στατιστικά στοιχεία σε όλες τις στήλες όλων των πινάκων μετά την πρώτη φόρτωσης ή αφού γίνουν οποιαδήποτε σημαντικές αλλαγές στα δεδομένα. Για μια αναλυτική εξήγηση των στατιστικών στοιχείων, ανατρέξτε στο θέμα [Στατιστικά στοιχεία][]. 

Εκτελέστε την παρακάτω εντολή για να δημιουργήσετε στατιστικά στοιχεία στον πίνακα που μόλις έχει φορτωθεί.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Εξαγωγή δεδομένων από αποθήκη δεδομένων του SQL
Για διασκέδαση, μπορείτε να εξαγάγετε τα δεδομένα που μόλις φορτωθεί πίσω από το αποθήκη δεδομένων του SQL.  Η εντολή για να εξαγάγετε είναι ακριβώς ίδια με την εξαγωγή από SQL Server.

Ωστόσο, υπάρχει διαφορά στα αποτελέσματα. Επειδή τα δεδομένα αποθηκεύονται σε θέσεις κατανέμεται μέσα σε αποθήκη δεδομένων του SQL, κατά την εξαγωγή δεδομένων κάθε κόμβος εγγράφει δεδομένα το αρχείο εξόδου. Η σειρά των δεδομένων στο αρχείο εξόδου είναι πιθανό να είναι διαφορετικά από τη σειρά των δεδομένων στο αρχείο εισαγωγής.

### <a name="export-a-table-and-compare-exported-results"></a>Εξαγωγή ενός πίνακα και συγκρίνετε εξαγόμενα αποτελέσματα

Για να δείτε τα εξαγόμενα δεδομένα, ανοίξτε μια γραμμή εντολών και να εκτελέσετε αυτή την εντολή χρησιμοποιώντας τη δική σας παραμέτρους. Όνομα διακομιστή είναι το όνομα του διακομιστή SQL Azure λογική.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Μπορείτε να επαληθεύσετε τα δεδομένα έχει εξαχθεί σωστά, ανοίγοντας το νέο αρχείο. Τα δεδομένα του αρχείου θα πρέπει να συμφωνεί με το κείμενο που ακολουθεί, αλλά πιθανώς θα ταξινομηθεί με διαφορετική σειρά:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-the-results-of-a-query"></a>Εξαγάγετε τα αποτελέσματα ενός ερωτήματος

Μπορείτε να χρησιμοποιήσετε τη συνάρτηση **queryout** του bcp για να εξαγάγετε τα αποτελέσματα ενός ερωτήματος αντί για εξαγωγή ολόκληρου του πίνακα. 

## <a name="next-steps"></a>Επόμενα βήματα
Για μια επισκόπηση της φόρτωσης, ανατρέξτε στο θέμα [Φόρτωση δεδομένων σε αποθήκη δεδομένων του SQL][].
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL][].
Ανατρέξτε στο θέμα [Επισκόπηση πίνακα][] ή [ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ σύνταξη][] για περισσότερες πληροφορίες σχετικά με τη δημιουργία ενός πίνακα σε αποθήκη δεδομένων του SQL.

<!--Image references-->

<!--Article references-->

[Φόρτωση δεδομένων σε αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-overview-load.md
[Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL]: ./sql-data-warehouse-overview-develop.md
[Επισκόπηση πίνακα]: ./sql-data-warehouse-tables-overview.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ σύνταξη]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Κέντρο λήψης της Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433
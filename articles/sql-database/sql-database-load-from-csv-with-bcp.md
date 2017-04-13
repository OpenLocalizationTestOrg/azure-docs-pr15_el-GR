<properties
   pageTitle="Φόρτωση δεδομένων από αρχείο CSV στο SQL Azure Databaase (bcp) | Microsoft Azure"
   description="Για το μέγεθος: μικρό δεδομένων χρησιμοποιεί bcp για εισαγωγή δεδομένων σε βάση δεδομένων SQL Azure."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Φόρτωση δεδομένων από CSV σε αποθήκη δεδομένων SQL Azure (επίπεδο αρχεία)

Μπορείτε να χρησιμοποιήσετε το βοηθητικό πρόγραμμα bcp γραμμής εντολών για εισαγωγή δεδομένων από ένα αρχείο CSV σε βάση δεδομένων SQL Azure.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

### <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να περιηγηθείτε στις αυτό το πρόγραμμα εκμάθησης, πρέπει:

- Μια βάση δεδομένων SQL Azure λογική διακομιστής και βάση δεδομένων
- Το βοηθητικό πρόγραμμα γραμμής εντολών bcp εγκατεστημένο
- Το βοηθητικό πρόγραμμα γραμμής εντολών sqlcmd εγκατεστημένο

Μπορείτε να κάνετε λήψη των βοηθητικών προγραμμάτων bcp και sqlcmd από το [Κέντρο λήψης της Microsoft][].

### <a name="data-in-ascii-or-utf-16-format"></a>Δεδομένα σε μορφή ASCII ή UTF-16

Εάν προσπαθείτε αυτό το πρόγραμμα εκμάθησης με τα δικά σας δεδομένα, τα δεδομένα σας πρέπει να χρησιμοποιήσετε την ASCII ή UTF-16 κωδικοποίηση εφόσον bcp δεν υποστηρίζει UTF-8. 

## <a name="1-create-a-destination-table"></a>1. δημιουργήσετε έναν πίνακα προορισμού

Ορισμός ενός πίνακα σε βάση δεδομένων SQL ως τον πίνακα προορισμού. Οι στήλες του πίνακα πρέπει να αντιστοιχεί τα δεδομένα σε κάθε γραμμή του αρχείου δεδομένων.

Για να δημιουργήσετε έναν πίνακα, ανοίξτε μια γραμμή εντολών και χρήση sqlcmd.exe για να εκτελέσετε την ακόλουθη εντολή:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Επόμενα βήματα

Για τη μετεγκατάσταση μιας βάσης δεδομένων SQL Server, ανατρέξτε στο θέμα [Μετεγκατάσταση βάσης δεδομένων SQL Server](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Κέντρο λήψης της Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433

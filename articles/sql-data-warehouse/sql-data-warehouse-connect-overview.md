<properties
   pageTitle="Σύνδεση με αποθήκη δεδομένων του Azure SQL | Microsoft Azure"
   description="Πώς μπορείτε να βρείτε το όνομα και η συμβολοσειρά σύνδεσης για το διακομιστή σας αποθήκη δεδομένων SQL Azure"
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
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Σύνδεση με αποθήκη δεδομένων του Azure SQL

Σε αυτό το άρθρο σάς βοηθά να συνδεθείτε σε αποθήκη δεδομένων του SQL για πρώτη φορά.

## <a name="find-your-server-name"></a>Εύρεση του ονόματος διακομιστή

Το πρώτο βήμα για τη σύνδεση σε αποθήκη δεδομένων του SQL είναι να γνωρίζετε πώς μπορείτε να βρείτε το όνομα του διακομιστή.  Για παράδειγμα, το όνομα του διακομιστή στο παράδειγμα που ακολουθεί είναι sample.database.windows.net. Για να βρείτε το όνομα του διακομιστή πλήρως προσδιορισμένη:

1. Μεταβείτε στην [πύλη του Azure][].
2. Κάντε κλικ στην επιλογή σε **βάσεις δεδομένων SQL** 
3. Κάντε κλικ στη βάση δεδομένων που θέλετε να συνδεθείτε.
4. Εντοπίστε το πλήρες όνομα διακομιστή.

    ![Πλήρες όνομα διακομιστή][1]

## <a name="supported-drivers-and-connection-strings"></a>Υποστηριζόμενα προγράμματα οδήγησης και συμβολοσειρών σύνδεσης

Αποθήκη δεδομένων του SQL Azure υποστηρίζει [ADO.NET][], [ODBC][], [PHP][]και [JDBC][]. Κάντε κλικ σε ένα από τα προηγούμενα προγράμματα οδήγησης για να βρείτε την πιο πρόσφατη έκδοση και την τεκμηρίωση. Για την αυτόματη δημιουργία τη συμβολοσειρά σύνδεσης για το πρόγραμμα οδήγησης που χρησιμοποιείτε από την πύλη του Azure, μπορείτε να επιλέξετε την **Εμφάνιση συμβολοσειρές σύνδεσης βάσης δεδομένων** από το προηγούμενο παράδειγμα.  Ακολουθούν επίσης ορισμένα παραδείγματα του τι εμφανίζεται μια συμβολοσειρά σύνδεσης για κάθε πρόγραμμα οδήγησης.

> [AZURE.NOTE] Εξετάστε το ενδεχόμενο ορίζοντας το χρονικό όριο σύνδεσης σε 300 δευτερόλεπτα για να επιτρέψετε τη σύνδεση επιβίωσης σύντομες περίοδοι μη διαθεσιμότητα.

### <a name="adonet-connection-string-example"></a>Παράδειγμα συμβολοσειρά σύνδεσης ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Παράδειγμα συμβολοσειρά σύνδεσης ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Παράδειγμα συμβολοσειρά σύνδεσης PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Παράδειγμα συμβολοσειρά σύνδεσης JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Ρυθμίσεις σύνδεσης

Αποθήκη δεδομένων του SQL τυποποιεί ορισμένες ρυθμίσεις κατά τη διάρκεια της σύνδεσης και δημιουργίας αντικειμένου. Αυτές οι ρυθμίσεις είναι δυνατό να παρακαμφθούν και περιλαμβάνουν:

| Ρύθμιση βάσης δεδομένων       | Τιμή                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | ΕΝΕΡΓΟΠΟΊΗΣΗ                           |
| [QUOTED_IDENTIFIERS][] | ΕΝΕΡΓΟΠΟΊΗΣΗ                           |
| [ΜΟΡΦΉ_ΗΜΕΡΟΜΗΝΊΑΣ][]         | MDY                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Επόμενα βήματα

Για να συνδεθείτε και υποβολή ερωτημάτων με το Visual Studio, ανατρέξτε στο θέμα [ερωτήματος με το Visual Studio][]. Για να μάθετε περισσότερα σχετικά με τις επιλογές ελέγχου ταυτότητας, ανατρέξτε στο θέμα [Έλεγχος ταυτότητας αποθήκη δεδομένων SQL Azure][].

<!--Articles-->
[Ερώτημα με το Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Έλεγχος ταυτότητας αποθήκη δεδομένων Azure SQL]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[ΜΟΡΦΉ_ΗΜΕΡΟΜΗΝΊΑΣ]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Πύλη του Azure]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png



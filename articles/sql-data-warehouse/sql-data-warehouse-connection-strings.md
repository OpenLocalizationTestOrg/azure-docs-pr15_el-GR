<properties
   pageTitle="Προγράμματα οδήγησης για αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβολοσειρές σύνδεσης και τα προγράμματα οδήγησης SQL αποθήκη δεδομένων"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="sonyama;barbkess"/>


# <a name="drivers-for-azure-sql-data-warehouse"></a>Προγράμματα οδήγησης για αποθήκη δεδομένων του Azure SQL

Μπορείτε να συνδεθείτε σε αποθήκη δεδομένων του SQL με διάφορες πρωτόκολλα διαφορετική εφαρμογή, όπως [ADO.NET][], [ODBC][], [PHP][] και [JDBC][]. Ακολουθούν ορισμένα παραδείγματα συμβολοσειρών συνδέσεις για κάθε πρωτόκολλο.  Μπορείτε επίσης να χρησιμοποιήσετε την πύλη του Azure για να δημιουργήσετε τη συμβολοσειρά σύνδεσης.  Για να δημιουργήσετε τη συμβολοσειρά σύνδεσης με την πύλη Azure, μεταβείτε σας blade βάσης δεδομένων, στην περιοχή *Essentials* κάντε κλικ στο στοιχείο *Εμφάνιση συμβολοσειρές σύνδεσης βάσης δεδομένων*.

## <a name="sample-adonet-connection-string"></a>Συμβολοσειρά σύνδεσης ADO.NET δείγματος

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

## <a name="sample-odbc-connection-string"></a>Δείγμα συμβολοσειρά σύνδεσης ODBC

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

## <a name="sample-php-connection-string"></a>Συμβολοσειρά σύνδεσης PHP δείγματος

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

## <a name="sample-jdbc-connection-string"></a>Συμβολοσειρά σύνδεσης JDBC δείγματος

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

> [AZURE.NOTE] Εξετάστε το ενδεχόμενο ορίζοντας το χρονικό όριο σύνδεσης σε 300 δευτερόλεπτα για να επιτρέπεται η σύνδεση επιβίωσης σύντομες περίοδοι μη διαθεσιμότητα.

## <a name="next-steps"></a>Επόμενα βήματα

Για να ξεκινήσετε την υποβολή ερωτημάτων σας αποθήκη δεδομένων με το Visual Studio και άλλες εφαρμογές, ανατρέξτε στο θέμα [ερωτήματος με το Visual Studio][].

<!--Image references-->

<!--Azure.com references-->
 [Ερώτημα με το Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
 
<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx

<!--Other references-->

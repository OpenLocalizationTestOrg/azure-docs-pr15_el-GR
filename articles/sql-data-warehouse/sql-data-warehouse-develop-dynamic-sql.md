<properties
   pageTitle="Δυναμική SQL σε αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για τη χρήση δυναμικής SQL σε αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Δυναμική SQL σε αποθήκη δεδομένων του SQL
Κατά την ανάπτυξη κώδικα της εφαρμογής για αποθήκη δεδομένων του SQL ίσως χρειαστεί να χρησιμοποιήσετε δυναμικής sql για να κάνουν ευέλικτων, γενικής χρήσης και λειτουργική λύσεις. Αποθήκη δεδομένων του SQL δεν υποστηρίζει τους τύπους δεδομένων blob αυτήν τη στιγμή. Αυτό μπορεί να περιορίσετε το μέγεθος των σας συμβολοσειρές ως τύποι αντικειμένων blob περιλαμβάνονται οι τύποι varchar(max) και nvarchar(max). Εάν έχετε χρησιμοποιήσει αυτούς τους τύπους σε κώδικα της εφαρμογής σας κατά τη δημιουργία πολύ μεγάλες συμβολοσειρές, θα πρέπει να διασπάσετε τον κώδικα σε μπλοκ και να χρησιμοποιήσετε τη δήλωση ΕΚΤΈΛΕΣΗΣ αντί για αυτό.

Ένα απλό παράδειγμα:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Αν η συμβολοσειρά είναι σύντομο μπορείτε να χρησιμοποιήσετε [sp_executesql][] κανονικά.

> [AZURE.NOTE] Δηλώσεις που εκτελούνται ως δυναμικό SQL θα εξακολουθήσουν να υπόκεινται σε όλους τους κανόνες επικύρωσης TSQL.

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->

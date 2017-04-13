<properties
   pageTitle="Σχεδίαση αποφάσεις και κωδικοποίησης τεχνικές για την ανάπτυξη αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Ανάπτυξη έννοιες, αποφάσεις σχεδιασμού, συστάσεις και κωδικοποίησης τεχνικές για την αποθήκη δεδομένων του SQL."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Αποφάσεις σχεδιασμού και κωδικοποίησης τεχνικές για την αποθήκη δεδομένων του SQL

Ρίξτε μια ματιά σε αυτά τα άρθρα ανάπτυξης για να κατανοήσετε καλύτερα αποφάσεις κλειδιού σχεδιασμού, συστάσεις και κωδικοποίησης τεχνικές για αποθήκη δεδομένων του SQL.

## <a name="key-design-decisions"></a>Αποφάσεις σχεδιασμού κλειδιού
Τα ακόλουθα άρθρα επισήμανση ορισμένες από τις βασικές έννοιες και σχεδίαση αποφάσεις που θα πρέπει να κατανοήσετε για την ανάπτυξη της αποθήκης σας κατανεμημένων δεδομένων με χρήση αποθήκη δεδομένων του SQL:

- [συνδέσεις][]
- [ταυτόχρονης εκτέλεσης][]
- [συναλλαγές][]
- [τα σχήματα που ορίζονται από το χρήστη][]
- [πίνακας διανομής][]
- [ευρετήρια πινάκων][]
- [διαμερίσματα του πίνακα][]
- [CTAS][]
- [Στατιστικά στοιχεία][]

## <a name="development-recommendations-and-coding-techniques"></a>Κωδικοποίηση τεχνικές και συστάσεις ανάπτυξης
Αυτά τα άρθρα επισημαίνουν συγκεκριμένες τεχνικές κωδικοποίηση, συμβουλές και συστάσεις για την ανάπτυξη σας αποθήκη δεδομένων του SQL:

- [αποθηκευμένες διαδικασίες][]
- [ετικέτες][]
- [προβολές][]
- [προσωρινό πίνακες][]
- [δυναμική SQL][]
- [Επανάληψη][]
- [Ομαδοποίηση κατά επιλογές][]
- [μεταβλητό ανάθεσης][]

## <a name="next-steps"></a>Επόμενα βήματα
Αφού σας έχουν έως τα άρθρα ανάπτυξης Ρίξτε μια ματιά μέσω της σελίδας [αναφοράς Transact-SQL][] για περισσότερες λεπτομέρειες σχετικά με τη σύνταξη υποστηριζόμενες για αποθήκη δεδομένων του SQL.

<!--Image references-->

<!--Article references-->
[ταυτόχρονης εκτέλεσης]: ./sql-data-warehouse-develop-concurrency.md
[συνδέσεις]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[δυναμική SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Ομαδοποίηση κατά επιλογές]: ./sql-data-warehouse-develop-group-by-options.md
[ετικέτες]: ./sql-data-warehouse-develop-label.md
[Επανάληψη]: ./sql-data-warehouse-develop-loops.md
[Στατιστικά στοιχεία]: ./sql-data-warehouse-tables-statistics.md
[αποθηκευμένες διαδικασίες]: ./sql-data-warehouse-develop-stored-procedures.md
[πίνακας διανομής]: ./sql-data-warehouse-tables-distribute.md
[ευρετήρια πινάκων]: ./sql-data-warehouse-tables-index.md
[διαμερίσματα του πίνακα]: ./sql-data-warehouse-tables-partition.md
[προσωρινό πίνακες]: ./sql-data-warehouse-tables-temporary.md
[συναλλαγές]: ./sql-data-warehouse-develop-transactions.md
[τα σχήματα που ορίζονται από το χρήστη]: ./sql-data-warehouse-develop-user-defined-schemas.md
[μεταβλητό ανάθεσης]: ./sql-data-warehouse-develop-variable-assignment.md
[προβολές]: ./sql-data-warehouse-develop-views.md
[Αναφορά Transact-SQL]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->

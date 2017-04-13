<properties
   pageTitle="Τα σχήματα που ορίζονται από το χρήστη στο αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για τη χρήση σχημάτων Transact-SQL στο αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
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

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Τα σχήματα που ορίζονται από το χρήστη στο αποθήκη δεδομένων του SQL

Χώρους αποθήκευσης δεδομένων παραδοσιακή χρήση συχνά ξεχωριστές βάσεις δεδομένων για τη δημιουργία ορίων εφαρμογών που βασίζονται σε φόρτο εργασίας, τομέα ή ασφάλειας. Για παράδειγμα, μια παραδοσιακή αποθήκη δεδομένων του SQL Server ενδέχεται να περιλαμβάνουν μια βάση δεδομένων δοκιμής, μια βάση δεδομένων αποθήκης δεδομένων και ορισμένες βάσεις δεδομένων mart δεδομένων. Σε αυτό τοπολογία κάθε βάση δεδομένων λειτουργεί ως φόρτο εργασίας και όριο ασφαλείας της αρχιτεκτονικής.

Αντίθετα, αποθήκη δεδομένων του SQL εκτελείται το φόρτο εργασίας αποθήκης ολόκληρο δεδομένων μέσα σε μια βάση δεδομένων. Σύνδεσμοι δεν επιτρέπεται διασταύρωσης βάσης δεδομένων. Επομένως, αποθήκη δεδομένων του SQL αναμένει όλους τους πίνακες που χρησιμοποιούνται από την αποθήκη για να αποθηκευτούν σε μια βάση δεδομένων.

> [AZURE.NOTE] SQL Data Warehouse δεν υποστηρίζει ερωτήματα διασταύρωσης βάσης δεδομένων οποιοδήποτε είδος. Ως εκ τούτου, υλοποιήσεις αποθήκη δεδομένων που αξιοποιήσετε αυτό το μοτίβο θα πρέπει να αναθεωρηθούν.

## <a name="recommendations"></a>Συστάσεις

Αυτές είναι συστάσεις για την ενοποίηση φόρτους εργασίας, ασφάλεια, τομέα και λειτουργική όρια χρησιμοποιώντας σχήματα που ορίζονται από το χρήστη

1. Χρησιμοποιήστε μία βάση δεδομένων αποθήκη δεδομένων του SQL για την εκτέλεση του φόρτου εργασίας αποθήκης ολόκληρο δεδομένων
2. Συνολική εικόνα υπάρχον περιβάλλον αποθήκη δεδομένων για να χρησιμοποιήσετε μία βάση δεδομένων SQL αποθήκη δεδομένων
3. Αξιοποίηση **σχήματα που ορίζονται από το χρήστη** για την παροχή το όριο υλοποιηθεί προηγουμένως χρήση βάσεων δεδομένων.

Εάν τα σχήματα που ορίζονται από το χρήστη δεν έχουν χρησιμοποιηθεί προηγουμένως, στη συνέχεια, έχετε καθαρό σημείο εκκίνησης. Απλώς να χρησιμοποιήσουμε το παλιό όνομα βάσης δεδομένων ως βάση για τις διατάξεις που ορίζονται από το χρήστη στη βάση δεδομένων SQL αποθήκη δεδομένων.

Εάν έχουν ήδη χρησιμοποιηθεί σχήματα, στη συνέχεια, έχετε μερικές επιλογές:

1. Καταργήστε τα ονόματα παλαιού τύπου σχήματος και ξεκινήστε από την αρχή
2. Διατήρηση τα ονόματα σχήματος παλαιού τύπου με προ-σε εκκρεμότητα το όνομα παλαιού τύπου σχήματος στο όνομα του πίνακα
3. Διατηρεί τα ονόματα σχήματος παλαιού τύπου με την εφαρμογή προβολές πάνω από τον πίνακα σε ένα σχήμα επιπλέον για να δημιουργήσετε ξανά το παλιό δομής σχήματος.

> [AZURE.NOTE] Στην πρώτη επιθεώρηση επιλογή 3 μπορεί να φαίνεται η επιλογή πιο ελκυστικό. Ωστόσο, το διάβολο είναι στις λεπτομέρειες. Προβολές είναι μόνο για ανάγνωση στο αποθήκη δεδομένων του SQL. Οποιαδήποτε τροποποίηση δεδομένων ή πίνακα θα πρέπει να εκτελεστούν σε σχέση με το βασικό πίνακα. Επιλογή 3 παρουσιάζει επίσης ένα επίπεδο προβολών στο σύστημα. Ενδέχεται να θέλετε να δώσετε αυτό ορισμένες επιπλέον σκέψης Εάν χρησιμοποιείτε προβολές στην αρχιτεκτονική ήδη.


### <a name="examples"></a>Παραδείγματα:

Υλοποίηση οι διατάξεις που ορίζονται από το χρήστη που βασίζονται σε ονόματα βάσης δεδομένων

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Τα διατηρήσουν ονόματα παλαιού τύπου σχημάτων από σε προ-εκκρεμότητα στο όνομα του πίνακα. Χρήση σχημάτων για το όριο φόρτο εργασίας.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Διατήρηση ονόματα παλαιού τύπου σχημάτων με χρήση προβολών

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Οποιαδήποτε αλλαγή στο σχήμα στρατηγικής χρειάζεται μια αναθεώρηση του μοντέλου ασφαλείας για τη βάση δεδομένων. Σε πολλές περιπτώσεις που ίσως να απλοποιήσετε το μοντέλο ασφαλείας με την εκχώρηση δικαιωμάτων στο επίπεδο της διάταξης. Εάν απαιτούνται δικαιώματα πιο λεπτομερές, στη συνέχεια, μπορείτε να χρησιμοποιήσετε τους ρόλους βάσης δεδομένων.

## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

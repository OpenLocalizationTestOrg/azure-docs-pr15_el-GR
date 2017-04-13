<properties
   pageTitle="Εκχώρηση μεταβλητές σε αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για την εκχώρηση Transact-SQL μεταβλητές σε αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Εκχώρηση μεταβλητές σε αποθήκη δεδομένων του SQL
Μεταβλητές σε αποθήκη δεδομένων του SQL έχουν οριστεί με χρήση του `DECLARE` πρόταση ή το `SET` πρόταση.

Όλα τα παρακάτω είναι απόλυτα έγκυρη τρόποι για να ορίσετε μια τιμή μεταβλητής:

## <a name="setting-variables-with-declare"></a>Ρύθμιση μεταβλητές με DECLARE

Προετοιμασία μεταβλητές με DECLARE είναι μόνο ένας από τους πιο ευέλικτη τρόπους για να ορίσετε μια τιμή μεταβλητής αποθήκη δεδομένων του SQL.

```sql
DECLARE @v  int = 0
;
```

Μπορείτε επίσης να χρησιμοποιήσετε DECLARE για να ορίσετε μεταβλητή περισσότερες από μία κάθε φορά. Δεν μπορείτε να χρησιμοποιήσετε `SELECT` ή `UPDATE` να το κάνετε αυτό:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Δεν μπορείτε να προετοιμασία και να χρησιμοποιήσετε μια μεταβλητή στην ίδια πρόταση DECLARE. Για την απεικόνιση του σημείου το παρακάτω παράδειγμα **δεν** επιτρέπεται ως @p1 είναι τόσο προετοιμασία και χρησιμοποιείται στην ίδια πρόταση DECLARE. Αυτό θα έχει ως αποτέλεσμα ένα σφάλμα.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Ορισμός τιμών με ΣΎΝΟΛΟ
Ορισμός είναι μια συνηθισμένη μέθοδος για τη ρύθμιση μια μεμονωμένη μεταβλητή.

Όλα τα παρακάτω παραδείγματα είναι έγκυρη τρόποι για τη ρύθμιση μιας μεταβλητής με ΣΎΝΟΛΟ:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Μπορείτε να ορίσετε μόνο μία μεταβλητή ταυτόχρονα με το ΣΎΝΟΛΟ. Ωστόσο, όπως φαίνεται παραπάνω σύνθετης τελεστές είναι επιτρεπτή.

## <a name="limitations"></a>Περιορισμοί
Δεν μπορείτε να χρησιμοποιήσετε ΕΠΙΛΟΓΉΣ ή ΕΝΗΜΈΡΩΣΗ για ανάθεση μεταβλητών.


## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

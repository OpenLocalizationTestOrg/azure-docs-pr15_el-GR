<properties
   pageTitle="Χρήση ετικετών στα ερωτήματα instrument σε αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για τη χρήση ετικετών στα ερωτήματα instrument στο αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Χρήση ετικετών στα ερωτήματα instrument στο αποθήκη δεδομένων του SQL
Αποθήκη δεδομένων του SQL υποστηρίζει μια έννοια που ονομάζεται ετικέτες ερωτήματος. Πριν να μεταβαίνουν σε οποιοδήποτε βάθος ας δούμε ένα παράδειγμα ενός:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Αυτής της τελευταίας γραμμής ετικέτες τη συμβολοσειρά 'Ετικέτα μου ερωτήματος' στο ερώτημα. Αυτό είναι ιδιαίτερα χρήσιμα, όπως η ετικέτα είναι ερωτήματος-δυνατότητα μέσω του DMVs. Αυτό μας παρέχει ένα μηχανισμό για να εντοπίσετε το πρόβλημα ερωτήματα και επίσης για να προσδιορίσετε την πρόοδο μέσω μιας εκτέλεσης ETL.

Μια καλή κανόνες ονοματοθεσίας βοηθά πραγματικά εδώ. Για παράδειγμα, κάτι όπως ' ΈΡΓΟ: ΔΙΑΔΙΚΑΣΊΑ: ΔΉΛΩΣΗ: ΣΧΌΛΙΟ ' θα σας βοηθήσουν να προσδιορίζει το ερώτημα στο μεταξύ όλον τον κώδικα στο στοιχείο ελέγχου προέλευσης.

Για να πραγματοποιήσετε αναζήτηση κατά ετικέτα, μπορείτε να χρησιμοποιήσετε το ακόλουθο ερώτημα που χρησιμοποιεί τη Διαχείριση δυναμικών προβολών:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Είναι απαραίτητο ότι μπορείτε να αναδιπλώσετε αγκύλες ή διπλά εισαγωγικά γύρω από την ετικέτα word κατά την υποβολή ερωτήματος. Ετικέτα είναι δεσμευμένη λέξη και να προκληθεί ένα σφάλμα εάν έχει μη οριοθετημένο.


## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης][].

<!--Image references-->

<!--Article references-->
[Επισκόπηση ανάπτυξης]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

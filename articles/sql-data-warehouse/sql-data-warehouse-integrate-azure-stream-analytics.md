<properties
   pageTitle="Χρήση του Azure ροή ανάλυσης με αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Συμβουλές για τη χρήση Azure ροή ανάλυσης με αποθήκη δεδομένων του SQL Azure για την ανάπτυξη λύσεων."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Χρήση του Azure ροή ανάλυσης με αποθήκη δεδομένων του SQL

Azure ροή ανάλυσης είναι μια πλήρως διαχειριζόμενη υπηρεσία παρέχοντας επεξεργασία χαμηλής αδράνειας, ιδιαίτερα διαθέσιμες, με σύνθετη συμβάντος μέσω ροής δεδομένων στο cloud. Μπορείτε να μάθετε τα βασικά στοιχεία διαβάζοντας [Εισαγωγή στην ανάλυση ροή Azure][]. Στη συνέχεια, μπορείτε να μάθετε πώς μπορείτε να δημιουργήσετε μια λύση για να ολοκληρωμένες με ροή ανάλυση, ακολουθώντας το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης][] .

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε τη βάση δεδομένων SQL Azure Data Warehouse ως ενός δέκτη εξόδου για τις εργασίες ατμού ανάλυσης.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πρώτα, εκτελέστε τα ακόλουθα βήματα στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης][] .  

1. Δημιουργήστε ένα συμβάν διανομέα εισαγωγής
2. Ρύθμιση παραμέτρων και την έναρξη της εφαρμογής δημιουργίας συμβάντος
3. Παροχή μιας ανάλυσης ροής εργασίας
4. Καθορίστε εισαγωγής έργου και ερωτήματος

Στη συνέχεια, δημιουργήστε μια βάση δεδομένων της αποθήκη δεδομένων του SQL Azure

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Καθορισμός εργασία εξόδου: αποθήκη δεδομένων του SQL Azure βάσης δεδομένων

### <a name="step-1"></a>Βήμα 1

Στην εργασία σας ανάλυση ροή κάντε κλικ στην επιλογή **ΈΞΟΔΟΣ** από το επάνω μέρος της σελίδας και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη ΕΞΌΔΟΥ**.

### <a name="step-2"></a>Βήμα 2

Επιλέξτε βάση δεδομένων SQL και κάντε κλικ στο κουμπί Επόμενο.

![][add-output]

### <a name="step-3"></a>Βήμα 3
Εισαγάγετε τις παρακάτω τιμές στην επόμενη σελίδα:

- *Ψευδώνυμο εξόδου*: Πληκτρολογήστε ένα φιλικό όνομα για αυτό το αποτέλεσμα εργασίας.
- *Συνδρομή*:
    - Εάν τη βάση δεδομένων SQL Data Warehouse είναι στην ίδια συνδρομή ως η εργασία ροής αναλυτικών στοιχείων, επιλέξτε χρήση της βάσης δεδομένων SQL από τρέχουσα συνδρομή σας.
    - Εάν η βάση δεδομένων βρίσκεται σε μια διαφορετική συνδρομή, επιλέξτε χρήση της βάσης δεδομένων SQL από άλλη συνδρομή.
- *Βάση δεδομένων*: Καθορίστε το όνομα της βάσης δεδομένων προορισμού.
- *Το όνομα του διακομιστή*: Καθορίστε το όνομα του διακομιστή για τη βάση δεδομένων που καθορίσατε. Μπορείτε να χρησιμοποιήσετε στην πύλη κλασική Azure για να βρείτε.

![][server-name]

- *Όνομα χρήστη*: Καθορίστε το όνομα χρήστη του λογαριασμού που έχει δικαιώματα εγγραφής για τη βάση δεδομένων.
- *Κωδικός πρόσβασης*: Εισαγάγετε τον κωδικό πρόσβασης για τον συγκεκριμένο λογαριασμό χρήστη.
- *Πίνακας*: Καθορίστε το όνομα του πίνακα προορισμού στη βάση δεδομένων.

![][add-database]

### <a name="step-4"></a>Βήμα 4

Κάντε κλικ στο κουμπί ελέγχου για να προσθέσετε αυτό το αποτέλεσμα εργασία και για να επαληθεύσετε ότι ανάλυση ροή μπορεί να συνδεθεί με επιτυχία στη βάση δεδομένων.

![][test-connection]

Όταν η σύνδεση με τη βάση δεδομένων με επιτυχία, θα δείτε μια ειδοποίηση στο κάτω μέρος της πύλης. Μπορείτε να επιλέξετε δοκιμή σύνδεσης στο κάτω μέρος για να ελέγξετε τη σύνδεση με τη βάση δεδομένων.

## <a name="next-steps"></a>Επόμενα βήματα

Για μια επισκόπηση της ενοποίησης, ανατρέξτε στο θέμα [Επισκόπηση ενοποίησης αποθήκη δεδομένων του SQL][].

Για περισσότερες συμβουλές ανάπτυξης, ανατρέξτε στο θέμα [Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Εισαγωγή στην ανάλυση Azure ροής]: ../stream-analytics/stream-analytics-introduction.md
[Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης]: ../stream-analytics/stream-analytics-get-started.md
[Επισκόπηση ανάπτυξης αποθήκη δεδομένων του SQL]:  ./sql-data-warehouse-overview-develop.md
[Επισκόπηση ενοποίησης αποθήκη δεδομένων του SQL]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/

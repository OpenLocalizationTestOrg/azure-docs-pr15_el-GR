<properties
   pageTitle="Εγκατάσταση του Visual Studio και SSDT για αποθήκη δεδομένων του SQL | Microsoft Azure"
   description="Εγκατάσταση του Visual Studio και εργαλεία ανάπτυξης του SQL Server (SSDT) για την αποθήκη δεδομένων του Azure SQL"
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
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Εγκατάσταση του Visual Studio 2015 και SSDT για αποθήκη δεδομένων του SQL

Για να αναπτύξετε εφαρμογές για αποθήκη δεδομένων του SQL, συνιστάται να χρησιμοποιείτε Visual Studio 2015 με την πιο πρόσφατη έκδοση του SQL Server δεδομένων Εργαλεία (SSDT).  5 ενημερωμένη έκδοση 2013 Visual Studio με SSDT υποστηρίζεται επίσης για συμβατότητα με προηγούμενες εκδόσεις.  

Χρήση του Visual Studio με SSDT θα σας επιτρέψει να χρησιμοποιήσετε Εξερεύνηση αντικείμενο του SQL Server για να οπτικά Εξερεύνηση πινάκων, προβολών, αποθηκευμένων διαδικασιών και πολλά περισσότερα αντικείμενα στο σας αποθήκη δεδομένων του SQL, καθώς και εκτέλεση ερωτημάτων.

> [AZURE.NOTE] SQL Data Warehouse δεν υποστηρίζει ακόμα έργα βάσης δεδομένων του Visual Studio.  Αυτή η δυνατότητα θα προστεθούν σε μια μελλοντική έκδοση.

## <a name="step-1-install-visual-studio-2015"></a>Βήμα 1: Εγκατάσταση του Visual Studio 2015

Ακολουθήστε αυτές τις συνδέσεις για να κάνετε λήψη και εγκατάσταση του Visual Studio 2015. Εάν έχετε ήδη Visual Studio 2013 ή του 2015 εγκατεστημένο, μπορείτε να μεταβείτε στο βήμα 2, εγκατάσταση SSDT.

1. [Κάντε λήψη του Visual Studio 2015][].
2. Ακολουθήστε τον οδηγό [Την εγκατάσταση του Visual Studio][] στο MSDN και επιλέξτε τις προεπιλεγμένες ρυθμίσεις παραμέτρων.

## <a name="step-2-install-ssdt"></a>Βήμα 2: Εγκαταστήστε SSDT

Για να εγκαταστήσετε το SSDT για το Visual Studio απλώς Ελέγξτε για μια ενημερωμένη έκδοση SSDT από μέσα σε Visual Studio, ακολουθώντας τα παρακάτω βήματα.

1. Στο Visual Studio, κάντε κλικ στην επιλογή **Εργαλεία** / **επεκτάσεις και ενημερώσεις...**  /  **Ενημερώσεις**
2. Επιλέξτε **Ενημερώσεις προϊόντος** και, στη συνέχεια, πραγματοποιήστε αναζήτηση για **tooling ενημέρωση του Microsoft SQL Server για τη βάση δεδομένων**

Εάν δεν βρεθεί μια ενημέρωση, στη συνέχεια, θα πρέπει να έχετε την πιο πρόσφατη έκδοση εγκατεστημένο.  Για να επιβεβαιώσετε SSDT είναι εγκατεστημένο, κάντε κλικ στη **Βοήθεια** / **Σχετικά με το Microsoft Visual Studio** και αναζητήστε το SQL Server Data Tools στη λίστα.  Η πιο πρόσφατη έκδοση του SSDT είναι 14.0.60525.0.  Εάν η επιλογή για την εγκατάσταση δεν είναι διαθέσιμη στο Visual Studio, Εναλλακτικά μπορείτε να επισκεφθείτε τη σελίδα [Λήψης SSDT][] για να κάνετε λήψη και εγκατάσταση SSDT με μη αυτόματο τρόπο.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε την πιο πρόσφατη έκδοση του SSDT, είστε έτοιμοι να [συνδεθείτε][] για να σας αποθήκη δεδομένων του SQL.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[σύνδεση]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Κάντε λήψη του Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Κατά την εγκατάσταση του Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[Λήψη SSDT]: https://msdn.microsoft.com/library/mt204009.aspx

<properties
    pageTitle="Επισκόπηση: Εργαλεία διαχείρισης για βάση δεδομένων SQL | Microsoft Azure"
    description="Συγκρίνει εργαλεία και επιλογές για τη διαχείριση της βάσης δεδομένων SQL Azure"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="sstein"/>

# <a name="overview-management-tools-for-sql-database"></a>Επισκόπηση: Εργαλεία διαχείρισης για βάση δεδομένων SQL

Αυτό το θέμα εξετάζει και τη συγκρίνει εργαλεία και επιλογές για τη Διαχείριση βάσεων δεδομένων Azure SQL, ώστε να μπορείτε να επιλέξετε το κατάλληλο εργαλείο για την εργασία, την επιχείρησή σας και έχετε. Επιλέγοντας το κατάλληλο εργαλείο εξαρτάται από το πόσες βάσεις δεδομένων που διαχείριση, την εργασία και πόσο συχνά εκτελείται μια εργασία.

## <a name="azure-portal"></a>Πύλη του Azure

Στην [πύλη του Azure](https://portal.azure.com) είναι μια εφαρμογή που βασίζεται στο web όπου μπορείτε να δημιουργήσετε, ενημέρωση, και διαγραφή βάσεων δεδομένων και λογική διακομιστές και παρακολούθηση της δραστηριότητας βάσης δεδομένων. Αυτό το εργαλείο είναι ιδανική Εάν μόλις ξεκινάτε με Azure, τη Διαχείριση μερικές βάσεις δεδομένων, ή πρέπει να κάνετε κάτι γρήγορα.

Για περισσότερες πληροφορίες σχετικά με την πύλη, ανατρέξτε στο θέμα [Διαχείριση βάσεων δεδομένων SQL με την πύλη Azure](sql-database-manage-portal.md).

## <a name="sql-server-management-studio-and-sql-server-data-tools-in-visual-studio"></a>SQL Server Management Studio και εργαλεία δεδομένων του SQL Server στο Visual Studio

SQL Server Management Studio (SSMS) και εργαλεία δεδομένων SQL Server (SSDT) είναι εργαλεία προγράμματος-πελάτη που εκτελούνται στον υπολογιστή σας για τη διαχείριση και ανάπτυξη βάση δεδομένων σας στο cloud. Εάν είστε εξοικειωμένοι με το Visual Studio ή άλλα ενοποιημένα περιβάλλοντα ανάπτυξης (IDE), [δοκιμάστε να χρησιμοποιήσετε SSDT στο Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)προγραμματιστή της εφαρμογής. Πολλοί διαχειριστές βάσης δεδομένων είναι εξοικειωμένοι με SSMS, τα οποία μπορούν να χρησιμοποιηθούν με βάσεις δεδομένων Azure SQL. [Λήψη πιο πρόσφατη έκδοση του SSMS](https://msdn.microsoft.com/library/mt238290) και να χρησιμοποιείτε πάντα την πιο πρόσφατη έκδοση όταν εργάζεστε με βάση δεδομένων SQL Azure. Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση τις βάσεις δεδομένων SQL Azure με SSMS, ανατρέξτε στο θέμα [Διαχείριση βάσεων δεδομένων SQL χρησιμοποιώντας SSMS](sql-database-manage-azure-ssms.md).

> [AZURE.IMPORTANT] Να χρησιμοποιείται πάντα την πιο πρόσφατη έκδοση του [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290) και [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) για να παραμείνετε συγχρονισμένοι με ενημερώσεις για το Microsoft Azure και βάση δεδομένων SQL.


## <a name="powershell"></a>PowerShell

Μπορείτε να χρησιμοποιήσετε PowerShell για τη Διαχείριση βάσεων δεδομένων και τα σύνολα ελαστικότητας βάσης δεδομένων και για την αυτοματοποίηση αναπτύξεις Azure πόρων. Η Microsoft συνιστά αυτό το εργαλείο για τη διαχείριση μεγάλου αριθμού βάσεις δεδομένων και αυτοματοποίηση ανάπτυξης και αλλαγές πόρων σε ένα περιβάλλον παραγωγής.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση βάσης δεδομένων SQL με το PowerShell](sql-database-manage-powershell.md)

## <a name="elastic-database-tools"></a>Ελαστικά Εργαλεία βάσης δεδομένων
Χρησιμοποιήστε τα εργαλεία ελαστικότητας βάσης δεδομένων για να εκτελέσετε ενέργειες όπως 

* Εκτέλεση μιας δέσμης ενεργειών T-SQL σε σχέση με ένα σύνολο βάσεις δεδομένων χρησιμοποιώντας μια [ελαστική εργασία](sql-database-elastic-jobs-overview.md)
* Μετακίνηση πολλών μισθωτών μοντέλο βάσεων δεδομένων σε ένα μοντέλο μίας μισθωτή με το [εργαλείο διαίρεσης συγχώνευσης](sql-database-elastic-scale-overview-split-and-merge.md)
* Διαχείριση βάσεων δεδομένων σε ένα μοντέλο μίας μισθωτή ή μοντέλου πολλών μισθωτή με τη [βιβλιοθήκη προγράμματος-πελάτη ελαστικότητας κλίμακα](sql-database-elastic-database-client-library.md).
 

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Azure από διαχειριστή πόρων](https://azure.microsoft.com/features/resource-manager/)
- [Αυτοματοποίηση Azure](https://azure.microsoft.com/documentation/services/automation/)
- [Azure χρονοδιαγράμματος](https://azure.microsoft.com/documentation/services/scheduler/)
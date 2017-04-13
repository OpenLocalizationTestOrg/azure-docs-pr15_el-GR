<properties
   pageTitle="Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (REST API) | Microsoft Azure"
   description="Εργασίες REST API για την επαναφορά ενός αποθήκη δεδομένων του SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (REST API)

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Πύλη][]
- [PowerShell][]
- [ΥΠΌΛΟΙΠΟ][]

Σε αυτό το άρθρο θα μάθετε πώς μπορείτε να επαναφέρετε μια αποθήκη δεδομένων του SQL Azure χρησιμοποιώντας το REST API.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

**Επαληθεύστε τη χωρητικότητα DTU.** Κάθε αποθήκη δεδομένων του SQL φιλοξενείται από SQL server (π.χ., myserver.database.windows.net) που έχει ένα προεπιλεγμένο όριο DTU.  Πριν να επαναφέρετε μια αποθήκη δεδομένων του SQL, επιβεβαιώστε ότι το υπόλοιπο DTU υπάρχουν αρκετά για τη βάση δεδομένων που επαναφέρεται περιλαμβάνει το SQL server. Για να μάθετε πώς μπορείτε να υπολογίσετε DTU απαιτείται ή για να ζητήσετε περισσότερες DTU, ανατρέξτε στο θέμα [αίτηση αλλαγής ορίου DTU][].

## <a name="restore-an-active-or-paused-database"></a>Επαναφορά μιας βάσης δεδομένων ενεργό ή βρίσκεται σε παύση

Για να επαναφέρετε μια βάση δεδομένων:

1. Λήψη της λίστας των σημείων επαναφοράς βάσης δεδομένων χρησιμοποιώντας τη λειτουργία Get σημεία επαναφοράς βάσης δεδομένων.
2. Ξεκινήστε την επαναφορά χρησιμοποιώντας τη λειτουργία [αίτηση επαναφορά Δημιουργία βάσης δεδομένων][] .
3. Παρακολουθείτε την κατάσταση της επαναφοράς σας με τη χρήση της [κατάστασης λειτουργίας βάσης δεδομένων][] λειτουργίας.

>[AZURE.NOTE] Μετά την ολοκλήρωση της επαναφοράς, μπορείτε να ρυθμίσετε τη βάση δεδομένων έχει ανακτηθεί από την εξής [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][].

## <a name="restore-a-deleted-database"></a>Επαναφορά διαγραμμένων βάσης δεδομένων

Για να επαναφέρετε μια διαγραμμένη βάση δεδομένων:

1.  Λίστα όλων των δυνατότητα επαναφοράς διαγραμμένων στις βάσεις δεδομένων με τη χρήση της λειτουργίας [λίστα δυνατότητα επαναφοράς απορρίφθηκαν βάσεις δεδομένων][] .
2.  Λάβετε τις λεπτομέρειες για το διαγραμμένο βάσης δεδομένων που θέλετε να επαναφέρετε, χρησιμοποιώντας τη λειτουργία [γρήγορα δυνατότητα επαναφοράς Απόρριψη βάσης δεδομένων][] .
3.  Ξεκινήστε την επαναφορά χρησιμοποιώντας τη λειτουργία [αίτηση επαναφορά Δημιουργία βάσης δεδομένων][] .
4.  Παρακολούθηση της κατάστασης των σας, επαναφέρετε με τη χρήση της [κατάστασης λειτουργίας βάσης δεδομένων][] λειτουργίας.

>[AZURE.NOTE] Για να ρυθμίσετε τη βάση δεδομένων μετά την ολοκλήρωση της επαναφοράς, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][]. 


## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με τις δυνατότητες επιχειρηματικής συνέχειας της βάσης δεδομένων SQL Azure εκδόσεις, διαβάστε τη [βάση δεδομένων SQL Azure επιχειρήσεις συνέχειας Επισκόπηση][].

<!--Image references-->

<!--Article references-->
[Azure Επισκόπηση συνέχειας επαγγελματικής βάσης δεδομένων SQL]: ./sql-database-business-continuity.md
[Αίτηση αλλαγής ορίου DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Ρύθμιση παραμέτρων βάσης δεδομένων σας μετά την ανάκτηση]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Επισκόπηση]: ./sql-data-warehouse-restore-database-overview.md
[Πύλη]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ΥΠΌΛΟΙΠΟ]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Δημιουργία αίτησης Επαναφορά βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Κατάσταση λειτουργίας της βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Λήψη δυνατότητα επαναφοράς Απόρριψη βάσης δεδομένων]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Λίστα δυνατότητα επαναφοράς αποτεθεί βάσεις δεδομένων]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps

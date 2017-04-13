<properties
   pageTitle="Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (πύλη) | Microsoft Azure"
   description="Azure πύλης εργασίες για την επαναφορά ενός αποθήκη δεδομένων του SQL Azure."
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

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Επαναφορά ενός αποθήκη δεδομένων του Azure SQL (πύλη)

> [AZURE.SELECTOR]
- [Επισκόπηση][]
- [Πύλη][]
- [PowerShell][]
- [ΥΠΌΛΟΙΠΟ][]

Σε αυτό το άρθρο θα μάθετε πώς μπορείτε να επαναφέρετε μια αποθήκη δεδομένων του SQL Azure με την πύλη Azure.

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

**Επαληθεύστε τη χωρητικότητα DTU.** Κάθε αποθήκη δεδομένων του SQL φιλοξενείται από SQL server (π.χ., myserver.database.windows.net) που έχει ένα προεπιλεγμένο όριο DTU.  Πριν να επαναφέρετε μια αποθήκη δεδομένων του SQL, επιβεβαιώστε ότι το υπόλοιπο DTU υπάρχουν αρκετά για τη βάση δεδομένων που επαναφέρεται περιλαμβάνει το SQL server. Για να μάθετε πώς μπορείτε να υπολογίσετε DTU απαιτείται ή για να ζητήσετε περισσότερες DTU, ανατρέξτε στο θέμα [αίτηση αλλαγής ορίου DTU][].


## <a name="restore-an-active-or-paused-database"></a>Επαναφορά μιας βάσης δεδομένων ενεργό ή βρίσκεται σε παύση

Για να επαναφέρετε μια βάση δεδομένων:

1. Συνδεθείτε [πύλη του Azure][]
2. Στην αριστερή πλευρά της οθόνης, επιλέξτε **Αναζήτηση** και, στη συνέχεια, επιλέξτε **διακομιστές SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Μεταβείτε στο διακομιστή σας και επιλέξτε την
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Βρείτε την αποθήκη δεδομένων του SQL που θέλετε να επαναφέρετε από και επιλέξτε την
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Στο επάνω μέρος του blade αποθήκη δεδομένων, κάντε κλικ στην επιλογή **Επαναφορά**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Καθορίστε ένα νέο **όνομα βάσης δεδομένων**
7. Επιλέξτε την πιο πρόσφατη **Σημείο επαναφοράς**
    1. Βεβαιωθείτε ότι επιλέγετε την πιο πρόσφατη σημείο επαναφοράς.  Επειδή το σημεία επαναφοράς εμφανίζονται στο UTC, μερικές φορές η προεπιλεγμένη επιλογή εμφανίζεται δεν είναι η πιο πρόσφατη σημείο επαναφοράς.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Κάντε κλικ στο κουμπί **OK**
9. Η διαδικασία επαναφοράς βάσης δεδομένων θα ξεκινήσει και να παρακολουθήσετε χρήση **ΕΙΔΟΠΟΙΉΣΕΩΝ**

>[AZURE.NOTE] Μετά την ολοκλήρωση της επαναφοράς, μπορείτε να ρυθμίσετε τη βάση δεδομένων έχει ανακτηθεί από την εξής [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][].


## <a name="restore-a-deleted-database"></a>Επαναφορά διαγραμμένων βάσης δεδομένων

Για να επαναφέρετε μια διαγραμμένη βάση δεδομένων:

1. Συνδεθείτε στην [πύλη του Azure][]
2. Στην αριστερή πλευρά της οθόνης, επιλέξτε **Αναζήτηση** και, στη συνέχεια, επιλέξτε **διακομιστές SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Μεταβείτε στο διακομιστή σας και επιλέξτε την
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Κάντε κύλιση προς τα κάτω στην ενότητα λειτουργίες σχετικά με το διακομιστή blade
5. Κάντε κλικ στο πλακίδιο **Διαγραφεί βάσεις δεδομένων**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Επιλέξτε που θέλετε να επαναφέρετε τη διαγραμμένη βάση δεδομένων
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Καθορίστε ένα νέο **όνομα βάσης δεδομένων**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Κάντε κλικ στο κουμπί **OK**
9. Η διαδικασία επαναφοράς βάσης δεδομένων θα ξεκινήσει και να παρακολουθήσετε χρήση **ΕΙΔΟΠΟΙΉΣΕΩΝ**

>[AZURE.NOTE] Για να ρυθμίσετε τη βάση δεδομένων μετά την ολοκλήρωση της επαναφοράς, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων μετά την ανάκτηση][]. 

## <a name="next-steps"></a>Επόμενα βήματα
Για να μάθετε περισσότερα σχετικά με τις δυνατότητες επιχειρηματικής συνέχειας των εκδόσεων βάση δεδομένων SQL Azure, διαβάστε την [Επισκόπηση συνέχειας επαγγελματικής βάσης δεδομένων SQL Azure][].

<!--Image references-->

<!--Article references-->
[Azure Επισκόπηση συνέχειας επαγγελματικής βάσης δεδομένων SQL]: ./sql-database-business-continuity.md
[Επισκόπηση]: ./sql-data-warehouse-restore-database-overview.md
[Πύλη]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[ΥΠΌΛΟΙΠΟ]: ./sql-data-warehouse-restore-database-rest-api.md
[Ρύθμιση παραμέτρων βάσης δεδομένων σας μετά την ανάκτηση]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Αίτηση αλλαγής ορίου DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Πύλη του Azure]: https://portal.azure.com/

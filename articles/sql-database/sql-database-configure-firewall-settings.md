<properties
    pageTitle="Ρύθμιση παραμέτρων ενός κανόνα τείχους προστασίας του επιπέδου διακομιστή βάσης δεδομένων SQL | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του τείχους προστασίας για διευθύνσεις IP που πρόσβαση Azure SQL server."
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="rickbyh;carlrab"/>


# <a name="configure-an-azure-sql-database-server-level-firewall-rule-using-the-azure-portal"></a>Ρύθμιση παραμέτρων ενός κανόνα τείχους προστασίας του επιπέδου διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-firewall-configure.md)
- [Πύλη του Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)

Azure SQL server χρησιμοποιεί κανόνες τείχους προστασίας για να επιτρέψετε σε συνδέσεις με τους διακομιστές σας και των βάσεων δεδομένων. Μπορείτε να ορίσετε ρυθμίσεις επιπέδου διακομιστή και βάσης δεδομένων σε επίπεδο τείχους προστασίας για το υπόδειγμα ή μια βάση δεδομένων χρήστη στο Azure SQL server λογική διακομιστή σας να επιτρέψετε την πρόσβαση στη βάση δεδομένων. Αυτό το θέμα περιγράφει κανόνες τείχους προστασίας επιπέδου διακομιστή.

> [AZURE.IMPORTANT] Για να επιτρέψετε τις εφαρμογές από το Azure για να συνδεθείτε με το διακομιστή Azure SQL, πρέπει να ενεργοποιηθεί Azure συνδέσεις. Για να κατανοήσετε τον τρόπο το τείχος προστασίας κανόνων εργασίας, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους ένα τείχος προστασίας Azure SQL server \- Επισκόπηση](sql-database-firewall-configure.md). Εάν πραγματοποιείτε συνδέσεις μέσα στο όριο Azure cloud, ίσως χρειαστεί να ανοίξετε ορισμένα επιπλέον θύρες TCP. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το **V12 της βάση δεδομένων του SQL: εκτός και στο εσωτερικό** ενότητα [θύρες πέρα από 1433 για διαίρεσης 4,5 ADO.NET και V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md)

**Σύσταση:** Χρησιμοποιήστε κανόνες τείχους προστασίας επιπέδου διακομιστή για τους διαχειριστές και όταν έχετε πολλές βάσεις δεδομένων που έχουν τις ίδιες απαιτήσεις πρόσβασης και δεν θέλετε να ξοδέψετε χρόνο μεμονωμένα ρύθμιση παραμέτρων κάθε βάσης δεδομένων. Microsoft συνιστά τη χρήση κανόνες τείχους προστασίας επίπεδο βάσης δεδομένων κάθε φορά που είναι δυνατόν, για τη βελτίωση της ασφάλειας και να κάνετε πιο φορητό τη βάση δεδομένων.

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Διαχείριση υπάρχουσας κανόνες τείχους προστασίας επιπέδου διακομιστή μέσω της πύλης Azure

Επαναλάβετε τα βήματα για να διαχειριστείτε τους κανόνες τείχους προστασίας επιπέδου διακομιστή.

- Για να προσθέσετε τον τρέχοντα υπολογιστή, κάντε κλικ στην επιλογή Προσθήκη IP υπολογιστή-πελάτη.
- Για να προσθέσετε επιπλέον διευθύνσεις IP, πληκτρολογήστε το όνομα κανόνα, ξεκινήστε τη διεύθυνση IP και τέλος διεύθυνση IP.
- Για να τροποποιήσετε έναν υπάρχοντα κανόνα, κάντε κλικ σε οποιοδήποτε από τα πεδία στον κανόνα και τροποποίηση.
- Για να διαγράψετε έναν υπάρχοντα κανόνα, τοποθετήστε το δείκτη επάνω από τον κανόνα μέχρι να εμφανιστεί το X στο τέλος της γραμμής. Κάντε κλικ στο X για να καταργήσετε τον κανόνα.

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τις αλλαγές.

## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με ένα άρθρο σχετικά με τη χρήση Transact-SQL για να δημιουργήσετε κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων SQL Azure κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων χρησιμοποιώντας T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Για το πώς να άρθρα σχετικά με τη δημιουργία κανόνες τείχους προστασίας επιπέδου διακομιστή χρησιμοποιώντας άλλες μεθόδους, ανατρέξτε στο θέμα: 

- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με χρήση του PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure χρησιμοποιώντας το REST API](sql-database-configure-firewall-settings-rest.md)

Για ένα πρόγραμμα εκμάθησης σχετικά με τη δημιουργία μιας βάσης δεδομένων, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL σε λεπτά με την πύλη Azure](sql-database-get-started.md).
Για βοήθεια σχετικά με τη σύνδεση σε μια βάση δεδομένων Azure SQL από ανοιχτό αρχείο προέλευσης ή εφαρμογές τρίτων κατασκευαστών, ανατρέξτε στο θέμα [προγράμματος-πελάτη γρήγορης εκκίνησης δείγματα κώδικα με βάση δεδομένων SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Για να κατανοήσετε τον τρόπο για να μεταβείτε σε βάσεις δεδομένων, ανατρέξτε στο θέμα [Διαχείριση ασφαλείας βάσης δεδομένων access και συνδεθείτε](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Ασφάλιση τη βάση δεδομένων](sql-database-security.md)
- [Κέντρο ασφάλειας για το μηχανισμό βάσεων δεδομένων SQL Server και βάση δεδομένων SQL Azure](https://msdn.microsoft.com/library/bb510589)


<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->

 

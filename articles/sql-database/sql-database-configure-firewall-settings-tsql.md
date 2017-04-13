<properties
    pageTitle="Azure κανόνες τείχους προστασίας του επιπέδου βάσης δεδομένων και επιπέδου διακομιστή βάσης δεδομένων SQL χρησιμοποιώντας T-SQL | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του τείχους προστασίας για διευθύνσεις IP που πρόσβαση σε βάσεις δεδομένων Azure SQL."
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
    ms.author="rickbyh"/>


# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Ρύθμιση παραμέτρων κανόνες τείχους προστασίας του επιπέδου βάσης δεδομένων και επιπέδου διακομιστή βάσης δεδομένων SQL Azure χρησιμοποιώντας T-SQL


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-firewall-configure.md)
- [Πύλη του Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Βάση δεδομένων SQL Microsoft Azure χρησιμοποιεί κανόνες τείχους προστασίας για να επιτρέπουν συνδέσεις προς τους διακομιστές σας και των βάσεων δεδομένων. Μπορείτε να ορίσετε ρυθμίσεις επιπέδου διακομιστή και βάσης δεδομένων σε επίπεδο τείχους προστασίας για το υπόδειγμα ή μια βάση δεδομένων χρήστη στο διακομιστή βάσης δεδομένων SQL Azure να επιτρέψετε την πρόσβαση στη βάση δεδομένων.

> [AZURE.IMPORTANT] Για να επιτρέψετε τις εφαρμογές από το Azure για να συνδεθείτε με το διακομιστή βάσης δεδομένων, πρέπει να ενεργοποιηθεί Azure συνδέσεις. Για περισσότερες πληροφορίες σχετικά με την ενεργοποίηση συνδέσεις από το Azure και κανόνες τείχους προστασίας, ανατρέξτε στο θέμα [Τείχος προστασίας βάσης δεδομένων SQL Azure](sql-database-firewall-configure.md). Εάν πραγματοποιείτε συνδέσεις μέσα στο όριο Azure cloud, ίσως χρειαστεί να ανοίξετε ορισμένα επιπλέον θύρες TCP. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το **V12 της βάση δεδομένων του SQL: εκτός και στο εσωτερικό** ενότητα [θύρες πέρα από 1433 για διαίρεσης 4,5 ADO.NET και V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="server-level-firewall-rules"></a>Κανόνες τείχους προστασίας επιπέδου διακομιστή

Μόνο ο κεφαλαίου login επιπέδου διακομιστή ή ο διαχειριστής Azure Active Directory να δημιουργήσετε έναν κανόνα τείχους προστασίας επιπέδου διακομιστή χρησιμοποιώντας Transact-SQL.

1. Εκκίνηση ενός παραθύρου ερωτήματος και να συνδεθείτε με το εικονικό κύρια βάση δεδομένων με χρήση του SQL Server Management Studio.
2. Κανόνες τείχους προστασίας επιπέδου διακομιστή μπορεί να είναι επιλεγμένο, δημιουργήθηκε, ενημέρωση, ή διαγραφεί από μέσα στο παράθυρο του ερωτήματος.
3. Για να δημιουργήσετε ή να ενημερώσετε τους κανόνες τείχους προστασίας επιπέδου διακομιστή, εκτελέστε το `sp_set_firewall_rule` αποθηκευμένη διαδικασία. Το παρακάτω παράδειγμα ενεργοποιεί μια περιοχή διευθύνσεων IP του διακομιστή Contoso.<br/>Ξεκινήστε κάνοντας βλέπουν οι κανόνες που υπάρχουν ήδη.

        SELECT * FROM sys.firewall_rules ORDER BY name;

    Στη συνέχεια, να προσθέσετε έναν κανόνα τείχους προστασίας.

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    Για να διαγράψετε έναν κανόνα τείχους προστασίας του επιπέδου διακομιστή, εκτελέστε τη διαδικασία sp_delete_firewall_rule που είναι αποθηκευμένα. Το παρακάτω παράδειγμα διαγράφει τον κανόνα με το όνομα ContosoFirewallRule.
 
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
 
 Για περισσότερες πληροφορίες σχετικά με αυτές τις αποθηκευμένες διαδικασίες, ανατρέξτε στο θέμα [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) και [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Κανόνες τείχους προστασίας επίπεδο βάσης δεδομένων

Μόνο ένας χρήστης βάσης δεδομένων με το δικαίωμα **ΕΛΈΓΧΟΥ** από τη βάση δεδομένων (όπως ο κάτοχος της βάσης δεδομένων) να δημιουργήσετε έναν κανόνα τείχους προστασίας επίπεδο βάσης δεδομένων.

1. Αφού δημιουργήσετε ένα τείχος προστασίας επιπέδου διακομιστή για τη διεύθυνση IP, ξεκινήστε ένα παράθυρο ερωτήματος μέσω της πύλης κλασική ή μέσω του SQL Server Management Studio.
2. Σύνδεση με τη βάση δεδομένων για την οποία θέλετε να δημιουργήσετε έναν κανόνα τείχους προστασίας επίπεδο βάσης δεδομένων.

    Για να δημιουργήσετε μια νέα ή ενημερώσετε έναν υπάρχοντα κανόνα τείχους προστασίας του επιπέδου βάσης δεδομένων, εκτελεί την `sp_set_database_firewall_rule` αποθηκευμένη διαδικασία. Το παρακάτω παράδειγμα δημιουργεί έναν νέο κανόνα τείχος προστασίας που ονομάζεται ContosoFirewallRule.
 
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
 
    Για να διαγράψετε έναν υπάρχοντα κανόνα τείχους προστασίας του επιπέδου βάσης δεδομένων, εκτελεί την `sp_delete_database_firewall_rule` αποθηκευμένη διαδικασία. Το παρακάτω παράδειγμα διαγράφει τον κανόνα με το όνομα ContosoFirewallRule.
`
   ΕΚΤΈΛΕΣΗ sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Για περισσότερες πληροφορίες σχετικά με αυτές τις αποθηκευμένες διαδικασίες, ανατρέξτε στο θέμα [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) και [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

## <a name="next-steps"></a>Επόμενα βήματα

Για το πώς να άρθρα σχετικά με τη δημιουργία κανόνες τείχους προστασίας επιπέδου διακομιστή χρησιμοποιώντας άλλες μεθόδους, ανατρέξτε στο θέμα: 

- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure](sql-database-configure-firewall-settings.md)
- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με χρήση του PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure χρησιμοποιώντας το REST API](sql-database-configure-firewall-settings-rest.md)

Για ένα πρόγραμμα εκμάθησης σχετικά με τη δημιουργία μιας βάσης δεδομένων, ανατρέξτε στο θέμα [Δημιουργία μιας βάσης δεδομένων SQL σε λεπτά με την πύλη Azure](sql-database-get-started.md).
Για βοήθεια σχετικά με τη σύνδεση σε μια βάση δεδομένων Azure SQL από ανοιχτό αρχείο προέλευσης ή εφαρμογές τρίτων κατασκευαστών, ανατρέξτε στο θέμα [προγράμματος-πελάτη γρήγορης εκκίνησης δείγματα κώδικα με βάση δεδομένων SQL](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Για να κατανοήσετε τον τρόπο για να μεταβείτε σε βάσεις δεδομένων, ανατρέξτε στο θέμα [Διαχείριση ασφαλείας βάσης δεδομένων access και συνδεθείτε](https://msdn.microsoft.com/library/azure/ee336235.aspx).


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Ασφάλιση τη βάση δεδομένων](sql-database-security.md)
- [Κέντρο ασφάλειας για το μηχανισμό βάσεων δεδομένων SQL Server και βάση δεδομένων SQL Azure](https://msdn.microsoft.com/library/bb510589)

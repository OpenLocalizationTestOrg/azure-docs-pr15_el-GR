<properties
    pageTitle="Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με χρήση του PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους του τείχους προστασίας για διευθύνσεις IP που πρόσβαση σε βάσεις δεδομένων Azure SQL."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="sstein"/>


# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με χρήση του PowerShell


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-firewall-configure.md)
- [Πύλη του Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Βάση δεδομένων SQL του Azure χρησιμοποιεί κανόνες τείχους προστασίας για να επιτρέπουν συνδέσεις προς τους διακομιστές σας και των βάσεων δεδομένων. Μπορείτε να ορίσετε ρυθμίσεις επιπέδου διακομιστή και βάσης δεδομένων σε επίπεδο τείχους προστασίας για την κύρια βάση δεδομένων ή μια βάση δεδομένων χρήστη στο διακομιστή βάσης δεδομένων SQL να επιτρέψετε την πρόσβαση στη βάση δεδομένων.

> [AZURE.IMPORTANT] Για να επιτρέψετε τις εφαρμογές από το Azure για να συνδεθείτε με το διακομιστή βάσης δεδομένων, πρέπει να ενεργοποιηθεί Azure συνδέσεις. Για περισσότερες πληροφορίες σχετικά με την ενεργοποίηση συνδέσεις από το Azure και κανόνες τείχους προστασίας, ανατρέξτε στο θέμα [Τείχος προστασίας βάσης δεδομένων SQL Azure](sql-database-firewall-configure.md). Εάν πραγματοποιείτε συνδέσεις μέσα στο όριο Azure cloud, ίσως χρειαστεί να ανοίξετε ορισμένα επιπλέον θύρες TCP. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το "V12 της βάσης δεδομένων SQL: εκτός και στο εσωτερικό" ενότητα [θύρες πέρα από 1433 για διαίρεσης 4,5 ADO.NET και V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Δημιουργία διακομιστή κανόνες τείχους προστασίας

Τείχος προστασίας επιπέδου διακομιστή μπορούν να δημιουργηθούν κανόνες, ενημέρωση και διαγραφεί με χρήση του Azure PowerShell.

Για να δημιουργήσετε έναν νέο κανόνα τείχους προστασίας του επιπέδου διακομιστή, εκτελεί την [δημιουργία-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) cmdlet. Το παρακάτω παράδειγμα ενεργοποιεί μια περιοχή διευθύνσεων IP του διακομιστή Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'       

Για να τροποποιήσετε έναν υπάρχοντα κανόνα τείχους προστασίας του επιπέδου διακομιστή, εκτελέστε το [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx) cmdlet. Το παρακάτω παράδειγμα αλλάζει την περιοχή των αποδεκτή διευθύνσεις IP για τον κανόνα με το όνομα ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Για να διαγράψετε έναν υπάρχοντα κανόνα τείχους προστασίας του επιπέδου διακομιστή, εκτελέστε το [κατάργηση-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx) cmdlet. Το παρακάτω παράδειγμα διαγράφει τον κανόνα με το όνομα ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –RuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Διαχείριση κανόνες τείχους προστασίας με χρήση του PowerShell

Μπορείτε επίσης να χρησιμοποιήσετε το PowerShell για να διαχειριστείτε κανόνες τείχους προστασίας. Για περισσότερες πληροφορίες, ανατρέξτε στα παρακάτω θέματα:

* [Νέα AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx)
* [Κατάργηση-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603588(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603789(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603586(v=azure.300\).aspx)


## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με τον τρόπο χρήσης του Transact-SQL για να δημιουργήσετε κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων SQL Azure κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων χρησιμοποιώντας T-SQL](sql-database-configure-firewall-settings-tsql.md).

Για πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε κανόνες τείχους προστασίας επιπέδου διακομιστή με άλλες μεθόδους, ανατρέξτε στα θέματα:

- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure](sql-database-configure-firewall-settings.md)
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

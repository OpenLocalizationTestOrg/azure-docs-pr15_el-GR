<properties
    pageTitle="Azure κανόνες τείχους προστασίας του επιπέδου διακομιστή βάσης δεδομένων SQL χρησιμοποιώντας το REST API | Microsoft Azure"
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


#  <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure χρησιμοποιώντας το REST API


> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-firewall-configure.md)
- [Πύλη του Azure](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)


Βάση δεδομένων SQL Microsoft Azure χρησιμοποιεί κανόνες τείχους προστασίας για να επιτρέπουν συνδέσεις προς τους διακομιστές σας και των βάσεων δεδομένων. Μπορείτε να ορίσετε ρυθμίσεις επιπέδου διακομιστή και βάσης δεδομένων σε επίπεδο τείχους προστασίας για το υπόδειγμα ή μια βάση δεδομένων χρήστη στο διακομιστή βάσης δεδομένων SQL Azure να επιτρέψετε την πρόσβαση στη βάση δεδομένων.

> [AZURE.IMPORTANT] Για να επιτρέψετε τις εφαρμογές από το Azure για να συνδεθείτε με το διακομιστή βάσης δεδομένων, πρέπει να ενεργοποιηθεί Azure συνδέσεις. Για περισσότερες πληροφορίες σχετικά με την ενεργοποίηση συνδέσεις από το Azure και κανόνες τείχους προστασίας, ανατρέξτε στο θέμα [Τείχος προστασίας βάσης δεδομένων SQL Azure](sql-database-firewall-configure.md). Εάν πραγματοποιείτε συνδέσεις μέσα στο όριο Azure cloud, ίσως χρειαστεί να ανοίξετε ορισμένα επιπλέον θύρες TCP. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το **V12 της βάση δεδομένων του SQL: εκτός και στο εσωτερικό** ενότητα [θύρες πέρα από 1433 για διαίρεσης 4,5 ADO.NET και V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md)


## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Διαχείριση κανόνων τείχους προστασίας του επιπέδου διακομιστή μέσω REST API
1. Διαχείριση κανόνες τείχους προστασίας μέσω REST API πρέπει να υποβληθούν σε έλεγχο ταυτότητας. Για πληροφορίες, ανατρέξτε στο θέμα [Οδηγός για προγραμματιστές να εξουσιοδότησης με το API διαχείρισης πόρων Azure](../resource-manager-api-authentication.md).
2. Κανόνες επιπέδου διακομιστή μπορούν να δημιουργηθούν ενημερωμένη ή τη διαγραφή χρησιμοποιώντας REST API

    Για να δημιουργήσετε ή να ενημερώσετε έναν κανόνα τείχους προστασίας του επιπέδου διακομιστή, η εφαρμογή της μεθόδου ΤΟΠΟΘΈΤΗΣΗ χρησιμοποιώντας τα εξής:
 
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
    
    Σώμα αίτησης

        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 
 

    Για να καταργήσετε έναν υπάρχοντα κανόνα τείχους προστασίας του επιπέδου διακομιστή, εκτελέστε τη μέθοδο DELETE χρησιμοποιώντας τα εξής:
     
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Διαχείριση κανόνων τείχους προστασίας χρησιμοποιώντας το REST API

* [Δημιουργία ή ενημέρωση κανόνα τείχους προστασίας](https://msdn.microsoft.com/library/azure/mt445501.aspx)
* [Διαγραφή κανόνα τείχους προστασίας](https://msdn.microsoft.com/library/azure/mt445502.aspx)
* [Λήψη κανόνα τείχους προστασίας](https://msdn.microsoft.com/library/azure/mt445503.aspx)
* [Λίστα όλοι οι κανόνες τείχους προστασίας](https://msdn.microsoft.com/library/azure/mt604478.aspx)
 
## <a name="next-steps"></a>Επόμενα βήματα

Για πληροφορίες σχετικά με ένα άρθρο σχετικά με τη χρήση Transact-SQL για να δημιουργήσετε κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων βάσης δεδομένων SQL Azure κανόνες τείχους προστασίας του επιπέδου διακομιστή και επίπεδο βάσης δεδομένων χρησιμοποιώντας T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Για το πώς να άρθρα σχετικά με τη δημιουργία κανόνες τείχους προστασίας επιπέδου διακομιστή χρησιμοποιώντας άλλες μεθόδους, ανατρέξτε στο θέμα: 

- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με την πύλη Azure](sql-database-configure-firewall-settings.md)
- [Ρύθμιση παραμέτρων κανόνες τείχους προστασίας επιπέδου διακομιστή βάσης δεδομένων SQL Azure με χρήση του PowerShell](sql-database-configure-firewall-settings-powershell.md)

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

 

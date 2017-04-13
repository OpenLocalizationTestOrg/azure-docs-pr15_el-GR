<properties
    pageTitle="Νέα βάση δεδομένων SQL εγκατάστασης με το PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων SQL με το PowerShell. Κοινές εργασίες ρύθμισης βάσης δεδομένων μπορεί να γίνει διαχείριση μέσω των cmdlet του PowerShell."
    keywords="Δημιουργία νέας βάσης δεδομένων sql, το πρόγραμμα εγκατάστασης βάσης δεδομένων"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Δημιουργήστε μια βάση δεδομένων SQL και να εκτελείτε συνήθεις εργασίες ρύθμισης βάσης δεδομένων με το cmdlet του PowerShell


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Μάθετε πώς μπορείτε να δημιουργήσετε μια βάση δεδομένων SQL με τη χρήση των cmdlet του PowerShell. (Για τη δημιουργία ελαστικότητας βάσεις δεδομένων, ανατρέξτε στο θέμα [Δημιουργία νέου χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Ρύθμιση βάσης δεδομένων: Δημιουργήστε μια ομάδα πόρων διακομιστή και κανόνα τείχους προστασίας

Όταν έχετε πρόσβαση για να εκτελέσετε cmdlet σε σχέση με τη συνδρομή σας επιλεγμένο Azure, το επόμενο βήμα είναι Δημιουργία ομάδας πόρων που περιέχει το διακομιστή όπου θα δημιουργηθεί η βάση δεδομένων. Μπορείτε να επεξεργαστείτε την επόμενη εντολή για να χρησιμοποιήσετε οποιαδήποτε έγκυρη θέση που επιλέγετε. Εκτέλεση **(Get-AzureRmLocation | Θέση αντικειμένου {$_. Υπηρεσίες παροχής - eq "Microsoft.Sql"}). Θέση** για να λάβετε μια λίστα με έγκυρες θέσεις.

Εκτελέστε την παρακάτω εντολή για να δημιουργήσετε μια ομάδα πόρων:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Δημιουργήστε ένα διακομιστή

Βάσεις δεδομένων SQL δημιουργούνται μέσα σε διακομιστές βάσης δεδομένων SQL Azure. Εκτελέστε το [δημιουργία-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) για να δημιουργήσετε ένα διακομιστή. Το όνομα του διακομιστή σας πρέπει να είναι μοναδικό σε όλους τους διακομιστές βάσης δεδομένων SQL Azure. Εάν το όνομα του διακομιστή είναι ήδη ληφθεί, λαμβάνετε ένα σφάλμα. Επίσης αξίζει να αναφερθούν είναι ότι αυτή η εντολή ενδέχεται να χρειαστούν αρκετά λεπτά για να ολοκληρωθεί. Μπορείτε να επεξεργαστείτε την εντολή για να χρησιμοποιήσετε οποιαδήποτε έγκυρη θέση που επιλέγετε, αλλά θα πρέπει να χρησιμοποιήσετε την ίδια θέση που χρησιμοποιήσατε για την ομάδα πόρων που δημιουργήσατε στο προηγούμενο βήμα.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Κατά την εκτέλεση αυτής της εντολής, θα σας ζητηθεί για το όνομα χρήστη και τον κωδικό πρόσβασης. Δεν εισαγάγετε τα διαπιστευτήριά σας Azure. Αντί για αυτό, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης για να δημιουργήσετε με το διαχειριστή του διακομιστή. Η δέσμη ενεργειών στο κάτω μέρος αυτού του άρθρου δείχνει πώς μπορείτε να ορίσετε τα διαπιστευτήρια διακομιστή στον κώδικα.

Εμφανίζονται οι λεπτομέρειες διακομιστή αφού δημιουργήθηκε με επιτυχία στο διακομιστή.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Ρύθμιση παραμέτρων του διακομιστή κανόνα τείχους προστασίας για να επιτρέψετε την πρόσβαση στο διακομιστή

Για να αποκτήσετε πρόσβαση στο διακομιστή, πρέπει να δημιουργήσετε έναν κανόνα τείχους προστασίας. Εκτελέστε το [δημιουργία-AzureRmSqlServerFirewallRule] (εντολή https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx), αντικαθιστώντας τις διευθύνσεις IP έναρξης και λήξης με έγκυρες τιμές για τον υπολογιστή σας.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Εμφανίζονται οι λεπτομέρειες κανόνα τείχους προστασίας μετά την επιτυχημένη δημιουργίας του κανόνα.

Για να επιτρέψετε σε άλλες υπηρεσίες του Azure για να αποκτήσετε πρόσβαση στο διακομιστή, Προσθήκη κανόνα τείχος προστασίας και ρυθμίστε το StartIpAddress και EndIpAddress 0.0.0.0. Αυτός ο κανόνας επιτρέπει Azure κίνησης από οποιαδήποτε Azure συνδρομή για να αποκτήσετε πρόσβαση στο διακομιστή.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τείχος προστασίας βάσης δεδομένων SQL Azure](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Δημιουργία βάσης δεδομένων SQL

Τώρα μπορείτε να κάνετε μια ομάδα πόρων, ένα διακομιστή και έναν κανόνα τείχους προστασίας που έχει ρυθμιστεί ώστε να μπορείτε να αποκτήσετε πρόσβαση στο διακομιστή.

Τα ακόλουθα [δημιουργία-AzureRmSqlDatabase] (η εντολή https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) δημιουργεί μια (κενή) βάση δεδομένων SQL κατά τη σειρά τυπική υπηρεσίας, με επίπεδο επιδόσεων S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Τις λεπτομέρειες της βάσης δεδομένων εμφανίζεται αφού η βάση δεδομένων δημιουργήθηκε με επιτυχία.

## <a name="create-a-sql-database-powershell-script"></a>Δημιουργία βάσης δεδομένων SQL δέσμη ενεργειών του PowerShell

Η ακόλουθη δέσμη ενεργειών PowerShell δημιουργεί μια βάση δεδομένων SQL και όλους τους πόρους του εξαρτώμενα. Αντικατάσταση όλων `{variables}` με τιμές ειδικά για τη συνδρομή σας και τους πόρους (Κατάργηση του **{}** όταν ορίζετε τις τιμές).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Επόμενα βήματα
Αφού δημιουργήσετε μια βάση δεδομένων SQL και εκτέλεση των εργασιών ρύθμισης βασικές βάσης δεδομένων, είστε έτοιμοι για τα εξής:

- [Διαχείριση της βάσης δεδομένων SQL με το PowerShell](sql-database-manage-powershell.md)
- [Σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγματος](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Cmdlet για βάση δεδομένων azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Βάση δεδομένων SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)

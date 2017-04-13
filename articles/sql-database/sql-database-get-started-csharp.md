<properties
    pageTitle="Βάσης δεδομένων SQL δοκιμάστε: Χρήση C# για να δημιουργήσετε μια βάση δεδομένων SQL | Microsoft Azure"
    description="Δοκιμάστε βάση δεδομένων SQL για την ανάπτυξη εφαρμογών SQL και C# και να δημιουργήσετε μια βάση δεδομένων SQL Azure με C# με χρήση της βιβλιοθήκης βάσης δεδομένων SQL για .NET."
    keywords="Δοκιμάστε sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Βάση δεδομένων SQL δοκιμάστε: Χρήση C# για να δημιουργήσετε μια βάση δεδομένων SQL με τη βιβλιοθήκη βάσης δεδομένων SQL για .NET


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Μάθετε πώς μπορείτε να χρησιμοποιήσετε C# για να δημιουργήσετε μια βάση δεδομένων Azure SQL με το [Microsoft Azure SQL βιβλιοθήκης διαχείρισης για το .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε μια βάση δεδομένων μία μόνο με SQL και C#. Για να δημιουργήσετε χώρους συγκέντρωσης ελαστικότητας βάση δεδομένων, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάση δεδομένων](sql-database-elastic-pool-create-csharp.md).

Η βιβλιοθήκη διαχείρισης Azure SQL βάσης δεδομένων για το .NET παρέχει ένα [Διαχειριστή πόρων Azure](../azure-resource-manager/resource-group-overview.md)-βάσει API που αναδιπλώνεται το [REST API βάση δεδομένων διαχείρισης πόρων βάσει SQL](https://msdn.microsoft.com/library/azure/mt163571.aspx).

>[AZURE.NOTE] Πολλές νέες δυνατότητες της βάσης δεδομένων SQL υποστηρίζονται μόνο όταν χρησιμοποιείτε το [μοντέλο ανάπτυξης διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md), έτσι θα πρέπει να χρησιμοποιείτε πάντα τις πιο πρόσφατες **βιβλιοθήκης διαχείρισης βάση δεδομένων SQL Azure για το .NET ([έγγραφα](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Το παλαιότερο [μοντέλο κλασική ανάπτυξης βάσει βιβλιοθήκες](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) υποστηρίζονται για συμβατότητα με προηγούμενες εκδόσεις μόνο, επομένως συνιστάται να χρησιμοποιείτε τις βιβλιοθήκες από διαχειριστή πόρων βάσει νεότερη.

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, χρειάζεστε τα εξής:

- Μια συνδρομή του Azure. Εάν χρειάζεστε μια συνδρομή του Azure απλώς κάντε κλικ στην επιλογή **ΔΩΡΕΆΝ ΛΟΓΑΡΙΑΣΜΌ** στο επάνω μέρος αυτής της σελίδας και, στη συνέχεια, επιστρέψτε στο τέλος αυτού του άρθρου.
- Visual Studio. Για μια δωρεάν αντίγραφο του Visual Studio, δείτε τη σελίδα [Λήψεων του Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

>[AZURE.NOTE] Σε αυτό το άρθρο δημιουργεί μια νέα, κενή βάση δεδομένων SQL. Τροποποιήστε τη μέθοδο *CreateOrUpdateDatabase(...)* στο ακόλουθο δείγμα για να αντιγράψετε βάσεις δεδομένων, κλίμακα βάσεις δεδομένων, δημιουργήστε μια βάση δεδομένων σε ένα χώρο συγκέντρωσης, κ.λπ. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα κλάσεις [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) και [Ιδιότητες βάσης δεδομένων](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) .



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Δημιουργία εφαρμογής κονσόλας και εγκαταστήστε τις απαιτούμενες βιβλιοθήκες

1. Ξεκινήστε το Visual Studio.
2. Κάντε κλικ στο **αρχείο** > **νέα** > **έργου**.
3. Δημιουργία C# **Εφαρμογής κονσόλας** και ονομάστε το: *SqlDbConsoleApp*


Για να δημιουργήσετε μια βάση δεδομένων SQL με C#, φόρτωσης των βιβλιοθηκών απαιτείται διαχείρισης (χρησιμοποιώντας την [Κονσόλα διαχείρισης πακέτου](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Κάντε κλικ στην επιλογή **Εργαλεία** > **διαχείρισης πακέτου NuGet** > **Κονσόλα διαχείρισης πακέτου**.
2. Τύπος `Install-Package Microsoft.Azure.Management.Sql –Pre` για να εγκαταστήσετε την πιο πρόσφατη [Βιβλιοθήκης διαχείρισης του Microsoft Azure SQL](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Τύπος `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` για την εγκατάσταση του [Microsoft Azure διαχείριση πόρων βιβλιοθήκης](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Τύπος `Install-Package Microsoft.Azure.Common.Authentication –Pre` για την εγκατάσταση του [Microsoft Azure κοινές βιβλιοθήκη ελέγχου ταυτότητας](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Τα παραδείγματα σε αυτό το άρθρο Χρησιμοποιήστε μια φόρμα σύγχρονη κάθε αίτησης API και αποκλεισμός μέχρι την ολοκλήρωση της ΥΠΌΛΟΙΠΗΣ κλήση στην υπηρεσία υποκείμενα. Υπάρχουν διαθέσιμες ασύγχρονης μεθόδους.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>Δημιουργήστε μια βάση δεδομένων SQL server, κανόνα τείχους προστασίας και βάση δεδομένων SQL - C# παράδειγμα

Το παρακάτω παράδειγμα δημιουργεί μια ομάδα πόρων διακομιστή, κανόνα τείχους προστασίας και μια βάση δεδομένων SQL. Ανατρέξτε στο θέμα, [Δημιουργία υπηρεσίας κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](#create-a-service-principal-to-access-resources) για να λάβετε το `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` μεταβλητές.

Αντικαταστήστε τα περιεχόμενα του **Program.cs** με τα παρακάτω και ενημερώστε το `{variables}` με τις τιμές του εφαρμογής (δεν περιλαμβάνει το `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Δημιουργία μιας υπηρεσίας κεφαλαίου για πρόσβαση σε πόρους

Η ακόλουθη δέσμη ενεργειών PowerShell δημιουργεί εφαρμογή του Active Directory (AD) και του αρχικού υπηρεσιών που θα πρέπει να ελέγχουν την ταυτότητα μας εφαρμογή C#. Η δέσμη ενεργειών εξόδους τιμών χρειαζόμαστε για το προηγούμενο παράδειγμα C#. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure για να δημιουργήσετε μια υπηρεσία κεφαλαίου για να αποκτήσετε πρόσβαση σε πόρους](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε δοκιμάσει βάσης δεδομένων SQL και να ρυθμίσετε μια βάση δεδομένων με το C#, είστε έτοιμοι για τα ακόλουθα άρθρα:

- [Σύνδεση με βάση δεδομένων SQL με SQL Server Management Studio και να εκτελέσετε ένα ερώτημα T SQL δείγματος](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Βάση δεδομένων SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [Κλάση βάσης δεδομένων](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png

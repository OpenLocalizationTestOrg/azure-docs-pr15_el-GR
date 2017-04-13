<properties
    pageTitle="Διαχείριση .NET SDK για ανάλυση ροή | Microsoft Azure"
    description="Γρήγορα αποτελέσματα με ροή ανάλυση διαχείρισης .NET SDK. Μάθετε πώς μπορείτε να ρυθμίσετε και να εκτελέσετε εργασίες ανάλυσης: δημιουργία ενός έργου, εισόδων, εξόδους και μετασχηματισμοί."
    keywords=".NET SDK, ανάλυση API"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Διαχείριση .NET SDK: Ρύθμιση του και να εκτελέσετε εργασίες αναλυτικών στοιχείων με χρήση του API Azure ροή ανάλυσης για .NET

Μάθετε πώς μπορείτε να ρυθμίσετε μια εκτέλεση ανάλυσης εργασιών με χρήση του API ανάλυσης ροής για .NET χρησιμοποιώντας το .NET SDK διαχείρισης. Ρύθμιση του έργου, να δημιουργήσετε προελεύσεις εισόδου και εξόδου, μετασχηματισμοί και έναρξη και διακοπή εργασιών. Για την ανάλυση εργασίες, μπορείτε να μεταδώσετε με ροή δεδομένων από το χώρο αποθήκευσης αντικειμένων Blob ή από ένα συμβάν διανομέα.

Ανατρέξτε στην [τεκμηρίωση αναφοράς διαχείρισης για το API ανάλυσης ροής για .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure ροή ανάλυσης είναι μια πλήρως διαχειριζόμενη υπηρεσία παρέχοντας Επεξεργασία συμβάντος χαμηλής αδράνειας, ιδιαίτερα διαθέσιμη, μεταβλητού μεγέθους, σύνθετες μέσω ροής δεδομένων στο cloud. Ανάλυση ροή επιτρέπει στους πελάτες για να ρυθμίσετε το ροής εργασιών για την ανάλυση δεδομένων ροών και σας επιτρέπει να καθοδηγήσετε κοντά σε πραγματικό χρόνο ανάλυση.  


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- Εγκατάσταση του Visual Studio 2012 ή 2013.
- Λήψη και εγκατάσταση του [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Δημιουργήστε μια ομάδα πόρων του Azure στη συνδρομή σας. Ακολουθεί ένα δείγμα δέσμης ενεργειών του PowerShell Azure. Για πληροφορίες Azure PowerShell, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Ορίσετε μια προέλευση εισόδου και εξόδου προορισμού για να χρησιμοποιήσετε. Για περαιτέρω οδηγίες ανατρέξτε στο θέμα [Προσθήκη εισόδων](stream-analytics-add-inputs.md) για να ρυθμίσετε ένα δείγμα εισαγωγής και [Προσθήκη εξόδους](stream-analytics-add-outputs.md) για να ρυθμίσετε ένα δείγμα εξόδου.


## <a name="set-up-a-project"></a>Ρύθμιση του έργου

Για να δημιουργήσετε μια εργασία ανάλυσης χρησιμοποιούν το API ανάλυσης ροής για .NET, ρυθμίστε πρώτα το έργο σας.

1. Δημιουργία μιας εφαρμογής κονσόλας Visual Studio C# .NET.
2. Στην κονσόλα διαχείρισης πακέτου, εκτελέστε τις ακόλουθες εντολές για να εγκαταστήσετε τα πακέτα NuGet. Το πρώτο είναι το .NET SDK Azure ροή ανάλυση διαχείρισης. Στο δεύτερο παράδειγμα είναι το πρόγραμμα-πελάτη Azure Active Directory που θα χρησιμοποιηθεί για τον έλεγχο ταυτότητας.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Προσθέστε την παρακάτω ενότητα **appSettings** στο αρχείο App.config:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Αντικατάσταση τιμών για **SubscriptionId** και **ActiveDirectoryTenantId** με τη συνδρομή σας Azure και μισθωτή αναγνωριστικά. Μπορείτε να λάβετε αυτές τις τιμές, εκτελώντας το ακόλουθο cmdlet του PowerShell Azure:

        Get-AzureAccount

5. Προσθέστε τις παρακάτω προτάσεις **χρησιμοποιώντας** το αρχείο προέλευσης (Program.cs) στο έργο:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Προσθέστε μια μέθοδο βοηθητική εφαρμογή του ελέγχου ταυτότητας:

        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(
                        ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                        ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    result = context.AcquireToken(
                        resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                        clientId: ConfigurationManager.AppSettings["AsaClientId"],
                        redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                        promptBehavior: PromptBehavior.Always);
                }
                catch (Exception threadEx)
                {
                    Console.WriteLine(threadEx.Message);
                }
            });

            thread.SetApartmentState(ApartmentState.STA);
            thread.Name = "AcquireTokenThread";
            thread.Start();
            thread.Join();

            if (result != null)
            {
                return result.AccessToken;
            }

            throw new InvalidOperationException("Failed to acquire token");
        }  


## <a name="create-a-stream-analytics-management-client"></a>Δημιουργήστε ένα πρόγραμμα-πελάτη διαχείρισης ροή ανάλυσης

Ένα αντικείμενο **StreamAnalyticsManagementClient** σάς επιτρέπει να διαχειριστείτε την εργασία και τα στοιχεία του έργου, όπως εισαγωγής, εξόδου και μετασχηματισμό.

Προσθέστε τον ακόλουθο κώδικα στην αρχή της μεθόδου **κύριες** :

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Τιμή της μεταβλητής **resourceGroupName** πρέπει να είναι το ίδιο με το όνομα της ομάδας πόρων που δημιουργήσατε ή επιλέξατε στα προαπαιτούμενες βήματα.

Για να αυτοματοποιήσετε τα διαπιστευτήρια αναλογίες παρουσίαση της δημιουργίας θέσεων εργασίας, ανατρέξτε [τον έλεγχο ταυτότητας αρχής υπηρεσίας με τη διαχείριση πόρων Azure](../resource-group-authenticate-service-principal.md).

Οι υπόλοιπες ενότητες σε αυτό το άρθρο προϋποθέτουν ότι αυτός ο κώδικας είναι στην αρχή της μεθόδου **κύριες** .

## <a name="create-a-stream-analytics-job"></a>Δημιουργία μιας εργασίας ροής ανάλυσης

Ο ακόλουθος κώδικας δημιουργεί μια εργασία ροής ανάλυση κάτω από την ομάδα πόρων που έχετε ορίσει. Θα προσθέσετε μια εισαγωγή δεδομένων εξόδου και μετασχηματισμό στο έργο αργότερα.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Δημιουργία προέλευσης εισαγωγής ροή ανάλυσης

Ο ακόλουθος κώδικας δημιουργεί ένα αρχείο προέλευσης εισαγωγής ανάλυση ροή με τον τύπο προέλευσης εισαγωγής blob και σειριοποίησης CSV. Για να δημιουργήσετε μια προέλευση εισόδου συμβάντος διανομέα, χρησιμοποιήστε **EventHubStreamInputDataSource** αντί για **BlobStreamInputDataSource**. Ομοίως, μπορείτε να προσαρμόσετε τον τύπο σειριοποίησης της εισαγωγής προέλευσης.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Προελεύσεις εισόδου, είτε από το χώρο αποθήκευσης αντικειμένων Blob ή ένα συμβάν διανομέα, συνδέονται με μια συγκεκριμένη εργασία. Για να χρησιμοποιήσετε την ίδια προέλευση εισαγωγής διαφορετικά έργα, που πρέπει να καλέσετε τη μέθοδο ξανά και να καθορίσετε ένα άλλο όνομα εργασίας.


## <a name="test-a-stream-analytics-input-source"></a>Έλεγχος αρχείου προέλευσης εισαγωγής ροή ανάλυσης

Η μέθοδος **TestConnection** ελέγχει εάν η εργασία ροής ανάλυσης είναι μπορείτε να συνδεθείτε με το αρχείο προέλευσης εισαγωγής, καθώς και άλλες πτυχές συγκεκριμένους τύπους προέλευσης εισαγωγής. Για παράδειγμα, στο αρχείο προέλευσης blob εισαγωγής που δημιουργήσατε σε ένα προηγούμενο βήμα, η μέθοδος θα ελέγχει ότι το όνομα λογαριασμού χώρου αποθήκευσης και ζεύγος κλειδιών μπορεί να χρησιμοποιηθεί για σύνδεση με το λογαριασμό χώρου αποθήκευσης, καθώς και να ελέγξετε ότι υπάρχει στο καθορισμένο κοντέινερ.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Δημιουργία προορισμός εξόδου ροή ανάλυσης

Δημιουργία μιας εξόδου προορισμού είναι παρόμοια με τη δημιουργία μιας προέλευσης εισαγωγής ροή αναλυτικών στοιχείων. Όπως προελεύσεις εισόδου, προορισμών εξόδου είναι συνδεδεμένη με μια συγκεκριμένη εργασία. Για να χρησιμοποιήσετε τον ίδιο προορισμό εξόδου διαφορετικά έργα, που πρέπει να καλέσετε τη μέθοδο ξανά και να καθορίσετε ένα άλλο όνομα εργασίας.

Ο ακόλουθος κώδικας δημιουργεί μια εξόδου προορισμού (βάση δεδομένων Azure SQL). Μπορείτε να προσαρμόσετε τον τύπο δεδομένων του προορισμού εξόδου ή/και τύπος σειριοποίησης.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Δοκιμή ανάλυσης ροή προορισμός εξόδου

Προορισμός εξόδου ανάλυσης ροή έχει επίσης τη μέθοδο **TestConnection** για σκοπούς δοκιμής συνδέσεις.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Δημιουργήστε ένα μετασχηματισμό ροή ανάλυσης

Ο ακόλουθος κώδικας δημιουργεί ένα μετασχηματισμό ροή αναλυτικών στοιχείων με το ερώτημα "επιλογή * από εισόδου" και καθορίζει για την εκχώρηση μιας μονάδας ροής για την εργασία ροής ανάλυσης. Για περισσότερες πληροφορίες σχετικά με την προσαρμογή ροής μονάδες, ανατρέξτε στο θέμα [ανάλυση ροή Azure κλίμακα εργασίες](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Όπως εισόδου και εξόδου, μια μετασχηματισμοί συνδέεται επίσης με το συγκεκριμένο έργο ροή ανάλυσης που δημιουργήθηκε στην περιοχή.

## <a name="start-a-stream-analytics-job"></a>Έναρξη μιας εργασίας ροής ανάλυσης
Μετά τη δημιουργία μιας εργασίας ροής ανάλυση και input(s), output(s) και μετασχηματισμό, μπορείτε να ξεκινήσετε την εργασία με κλήση της μεθόδου **Έναρξη** .

Το παρακάτω δείγμα κώδικα έναρξης μιας εργασίας ροής ανάλυσης με ώρα έναρξης προσαρμοσμένο εξόδου Ορισμός 12 Δεκεμβρίου 2012, 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Διακοπή μιας ανάλυσης ροής εργασίας
Μπορείτε να διακόψετε μια εργασία που εκτελείται ανάλυση ροή καλώντας τη μέθοδο **Διακοπή** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Διαγραφή μιας ανάλυσης ροής εργασίας
Η μέθοδος **Διαγραφή** θα διαγράψει την εργασία, καθώς και τα υποκείμενα δευτερεύοντες πόροι, συμπεριλαμβανομένων των input(s), output(s) και μετασχηματισμό της εργασίας.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Λήψη υποστήριξης
Για περαιτέρω βοήθεια, δοκιμάστε να μας [φόρουμ Azure ροή ανάλυσης](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Επόμενα βήματα

Έχετε μάθετε τα βασικά στοιχεία της χρήσης ενός .NET SDK για να δημιουργήσετε και να εκτελέσετε εργασίες ανάλυσης. Για περισσότερες πληροφορίες, ανατρέξτε στα παρακάτω:

- [Εισαγωγή στην ανάλυση Azure ροής](stream-analytics-introduction.md)
- [Γρήγορα αποτελέσματα με το Azure ροή ανάλυσης](stream-analytics-get-started.md)
- [Κλίμακα Azure ανάλυση ροής εργασιών](stream-analytics-scale-jobs.md)
- [Azure ροή ανάλυση διαχείρισης .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Αναφορά γλώσσας ερωτήματος ανάλυσης Azure ροής](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure ροή ανάλυση διαχείρισης REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

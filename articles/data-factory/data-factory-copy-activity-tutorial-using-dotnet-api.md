<properties 
    pageTitle="Πρόγραμμα εκμάθησης: Δημιουργία μιας διοχέτευσης με χρήση του .NET API δραστηριότητα αντίγραφο | Microsoft Azure" 
    description="Σε αυτό το πρόγραμμα εκμάθησης, δημιουργείτε μια διοχέτευση Azure εργοστασίου δεδομένων με μια δραστηριότητα αντίγραφο με χρήση του .NET API." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Πρόγραμμα εκμάθησης: Δημιουργία μιας διοχέτευσης με χρήση του .NET API δραστηριότητα αντιγράφου
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Αντιγραφή οδηγού](data-factory-copy-data-wizard-tutorial.md)
- [Πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure προτύπου για τη διαχείριση πόρων](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς να δημιουργήσετε και να παρακολουθείτε μια εργοστασίου Azure δεδομένων με χρήση του API .NET. Της διοχέτευσης στο η προέλευση δεδομένων χρησιμοποιεί μια δραστηριότητα Αντιγραφή για να αντιγράψετε δεδομένα από το χώρο αποθήκευσης Blob του Azure με βάση δεδομένων SQL Azure.

Τη δραστηριότητα αντίγραφο εκτελεί την κυκλοφορία δεδομένων Azure εργοστασίου δεδομένων. Η δραστηριότητα είναι υποστηρίζεται από μια καθολικά διαθέσιμη υπηρεσία που να αντιγράψετε τα δεδομένα μεταξύ των διαφόρων αποθηκεύει δεδομένα με ασφάλεια, αξιοπιστία και με τον τρόπο. Δείτε το άρθρο [Δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) για λεπτομέρειες σχετικά με τη δραστηριότητα αντίγραφο.   

> [AZURE.NOTE] 
> Σε αυτό το άρθρο δεν καλύπτει όλα τα API δεδομένων εργοστασίου .NET. Για λεπτομέρειες σχετικά με την προέλευση δεδομένων .NET SDK, ανατρέξτε στο θέμα [Αναφορά API .NET εργοστασίου δεδομένων](https://msdn.microsoft.com/library/mt415893.aspx) . 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
- Μεταβείτε μέσω του [προγράμματος εκμάθησης επισκόπηση και προαπαιτούμενα](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) για να δείτε μια επισκόπηση του προγράμματος εκμάθησης και ολοκληρώστε τα βήματα **προϋπόθεση** . 
- Visual Studio 2012 ή 2013 ή 2015
- Λήψη και εγκατάσταση του [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure PowerShell. Ακολουθήστε τις οδηγίες στο άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md) για να εγκαταστήσετε το Azure PowerShell στον υπολογιστή σας. Μπορείτε να χρησιμοποιήσετε Azure PowerShell για να δημιουργήσετε μια εφαρμογή του Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory
Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory, να δημιουργήσετε αρχής υπηρεσίας για την εφαρμογή και να εκχωρήσετε σε το ρόλο **Συμβολής εργοστασίου δεδομένων** .  

1. Εκκίνηση **του PowerShell**. 
1. Εκτελέστε την ακόλουθη εντολή και πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.
    
        Login-AzureRmAccount   
2. Εκτελέστε την παρακάτω εντολή για να προβάλετε όλες τις συνδρομές για αυτόν το λογαριασμό.

        Get-AzureRmSubscription 
3. Εκτελέστε την παρακάτω εντολή για να επιλέξετε τη συνδρομή στην οποία θέλετε να εργαστείτε. Αντικατάσταση ** &lt;NameOfAzureSubscription** &gt; με το όνομα της συνδρομής σας στο Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Σημείωση προς τα κάτω **SubscriptionId** και **TenantId** από την έξοδο αυτής της εντολής. 
4. Δημιουργία ομάδας του Azure πόρων με το όνομα **ADFTutorialResourceGroup** εκτελώντας την παρακάτω εντολή στο το PowerShell.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Εάν υπάρχει ήδη στην ομάδα πόρων, καθορίστε εάν θέλετε να ενημερώσετε (Y) ή αφήστε το ως (N). 

    Εάν χρησιμοποιείτε μια ομάδα διαφορετικό πόρο, πρέπει να χρησιμοποιήσετε το όνομα της ομάδας σας πόρων στη θέση ADFTutorialResourceGroup σε αυτό το πρόγραμμα εκμάθησης.
5. Δημιουργία μιας εφαρμογής υπηρεσίας καταλόγου Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Εάν λάβετε το ακόλουθο σφάλμα, καθορίστε μια διαφορετική διεύθυνση URL και εκτελέστε ξανά την εντολή. 

        Another object with the same value for property identifierUris already exists.

6. Δημιουργήστε το AD κεφάλαιο υπηρεσίας. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Προσθήκη υπηρεσίας κεφάλαιο με το ρόλο του **Συνεργάτη εργοστασίου δεδομένων** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Λάβετε το αναγνωριστικό εφαρμογής.

        $azureAdApplication

    Σημείωση προς τα κάτω το Αναγνωριστικό εφαρμογής (**αναγνωριστικά εφαρμογής** από την έξοδο).

Θα πρέπει να έχετε παρακολούθηση τέσσερις τιμές από τα εξής βήματα: 

- Αναγνωριστικό μισθωτή
- Αναγνωριστικό συνδρομής
- Αναγνωριστικό εφαρμογής 
- Τον κωδικό πρόσβασης (που καθορίζεται στην πρώτη εντολή)   

## <a name="walkthrough"></a>Αναλυτικές οδηγίες
1. Χρήση του Visual Studio 2012/2013/2015, η δημιουργία μιας εφαρμογής κονσόλας C# .NET.
    1. Εκκινήστε το **Visual Studio** 2012/2013/2015.
    2. Κάντε κλικ στο **αρχείο**, επιλέξτε **Δημιουργία**και κάντε κλικ στο **έργο**.
    3. Ανάπτυξη **προτύπων**και επιλέξτε **Visual C#**. Σε αυτόν τον οδηγό, μπορείτε να χρησιμοποιήσετε C#, αλλά μπορείτε να χρησιμοποιήσετε οποιαδήποτε γλώσσα .NET.
    4. Επιλέξτε **Εφαρμογή κονσόλας** από τη λίστα των τύπων έργου στη δεξιά πλευρά.
    5. Πληκτρολογήστε **DataFactoryAPITestApp** για το όνομα.
    6. Επιλέξτε **C:\ADFGetStarted** για τη θέση.
    7. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.
2. Κάντε κλικ στην επιλογή **Εργαλεία**, οδηγεί στη **Διαχείριση πακέτου Nuget**και κάντε κλικ στην επιλογή **Κονσόλα διαχείρισης πακέτου**.
3.  Στην **Κονσόλα διαχείρισης πακέτου**, ακολουθήστε τα παρακάτω βήματα: 
    1.  Εκτελέστε την παρακάτω εντολή για να εγκαταστήσετε το πακέτο εργοστασίου δεδομένων:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Εκτελέστε την ακόλουθη εντολή για την εγκατάσταση του πακέτου Azure Active Directory (χρησιμοποιείτε API του Active Directory στον κώδικα):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. Προσθέστε την παρακάτω ενότητα **appSetttings** στο αρχείο **App.config** . Αυτές οι ρυθμίσεις που χρησιμοποιούνται από τη μέθοδο Βοήθειας: **GetAuthorizationHeader**. 

    Αντικατάσταση τιμών για ** &lt;Αναγνωριστικό εφαρμογής&gt;**, ** &lt;τον κωδικό πρόσβασης&gt;**, ** &lt;Αναγνωριστικό συνδρομής&gt;**, και ** &lt;μισθωτή Αναγνωριστικό&gt; ** με τις δικές σας τιμές. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Προσθέστε τις παρακάτω προτάσεις **χρησιμοποιώντας** το αρχείο προέλευσης (Program.cs) στο έργο.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί μια παρουσία της κλάσης **DataPipelineManagementClient** τη μέθοδο **κύριες** . Μπορείτε να χρησιμοποιήσετε αυτό το αντικείμενο για να δημιουργήσετε μια προέλευση δεδομένων, μια υπηρεσία στο συνδεδεμένο, σύνολα δεδομένων εισόδου και εξόδου και μια διαδικασία. Μπορείτε επίσης να χρησιμοποιήσετε αυτό το αντικείμενο για την παρακολούθηση της φέτες από ένα σύνολο δεδομένων κατά το χρόνο εκτέλεσης.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Αντικαταστήστε την τιμή του **resourceGroupName** με το όνομα της ομάδας σας Azure πόρων. 
    > 
    > Ενημερώστε το όνομα του εργοστασίου δεδομένων (**dataFactoryName**) για να είναι μοναδικό. Όνομα του εργοστασίου δεδομένων πρέπει να είναι μοναδικό καθολικά. Ανατρέξτε στο θέμα [Factory δεδομένων - κανόνες ονοματοθεσίας](data-factory-naming-rules.md) για τους κανόνες ονοματοθεσίας για αντικείμενα εργοστασίου δεδομένων. 

7. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί μια **προέλευση δεδομένων** στη μέθοδο **κύριες** .

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί ένα **χώρο αποθήκευσης Azure συνδεδεμένες υπηρεσίας** με τη μέθοδο **κύριες** . 

    > [AZURE.IMPORTANT] Αντικατάσταση **storageaccountname** και **accountkey** με το όνομα και το κλειδί του λογαριασμού σας χώρο αποθήκευσης Azure. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί μια **Azure SQL συνδεδεμένες υπηρεσίας** με τη μέθοδο **κύριες** .
 
    > [AZURE.IMPORTANT] Αντικαταστήστε το **όνομα διακομιστή**, **όνομα βάσης δεδομένων**, **όνομα χρήστη**και **τον κωδικό πρόσβασης** με τα ονόματα των διακομιστή Azure SQL, βάση δεδομένων, χρήστη και τον κωδικό πρόσβασης.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Προσθέστε τον ακόλουθο κώδικα που δημιουργεί **σύνολα δεδομένων εισόδου και εξόδου** στην **κύρια** μέθοδο. 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Προσθέστε τον ακόλουθο κώδικα που **δημιουργεί και ενεργοποιεί μια διαδικασία** για τη μέθοδο **κύριες** . Αυτή η διαδικασία έχει μια **CopyActivity** που τίθεται σε **BlobSource** ως προέλευση και **BlobSink** ως ένα δέκτη.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Προσθέστε τον ακόλουθο κώδικα στην **κύρια** μέθοδο για να λάβετε την κατάσταση του μια φέτα δεδομένων του συνόλου δεδομένων εξόδου. Υπάρχει μόνο φέτα αναμένεται σε αυτό το δείγμα.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Προσθέστε τον ακόλουθο κώδικα για να λάβετε εκτελέσετε λεπτομέρειες για ένα κομμάτι δεδομένων στην **κύρια** μέθοδο.

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Προσθέστε την ακόλουθη μέθοδο Βοήθειας που χρησιμοποιούνται από την **κύρια** μέθοδο για να την κλάση **πρόγραμμα** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. Στην Εξερεύνηση λύσεων, αναπτύξτε το έργο (**DataFactoryAPITestApp**), κάντε δεξί κλικ σε **αναφορές**και κάντε κλικ στην επιλογή **Προσθήκη αναφοράς**. Επιλέξτε το πλαίσιο ελέγχου για συγκρότησης "**System.Configuration**" και κάντε κλικ στο **κουμπί OK**. 
16. Δημιουργία της εφαρμογής κονσόλας. Κάντε κλικ στην επιλογή " **Δημιουργία** " στο μενού και κάντε κλικ στην επιλογή **Δημιουργία λύσης**. 
16. Επιβεβαιώστε ότι υπάρχει τουλάχιστον ένα αρχείο στο κοντέινερ **adftutorial** στο χώρο αποθήκευσης αντικειμένων blob του Azure του. Εάν δεν είναι, δημιουργία αρχείου **Emp.txt** στο σημειωματάριο με το παρακάτω περιεχόμενο και στείλτε το στο κοντέινερ adftutorial.

        John, Doe
        Jane, Doe
     
17. Εκτελέστε το δείγμα κάνοντας κλικ στην επιλογή **Εντοπισμός σφαλμάτων** -> **Έναρξη εντοπισμού** στο μενού. Όταν δείτε το **γρήγορα εκτέλεση λεπτομέρειες ένα κομμάτι δεδομένων**, περιμένετε λίγα λεπτά και πατήστε το πλήκτρο **ENTER**. 
18. Χρησιμοποιήστε την πύλη του Azure για να επιβεβαιώσετε ότι η προέλευση δεδομένων **APITutorialFactory** δημιουργείται με τα ακόλουθα αντικείμενα: 
    - Συνδεδεμένες υπηρεσίας: **LinkedService_AzureStorage** 
    - Σύνολο δεδομένων: **DatasetBlobSource** και **DatasetBlobDestination**.
    - Διαδικασία: **PipelineBlobSample** 
18. Βεβαιωθείτε ότι δημιουργούνται οι καρτέλες δύο υπαλλήλου στον πίνακα "**emp**" στην καθορισμένη βάση δεδομένων Azure SQL.

## <a name="next-steps"></a>Επόμενα βήματα

- Διαβάστε το άρθρο [Δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , που παρέχει λεπτομερείς πληροφορίες σχετικά με τη δραστηριότητα αντίγραφο που χρησιμοποιούνται στην εκμάθηση.
- Για λεπτομέρειες σχετικά με την προέλευση δεδομένων .NET SDK, ανατρέξτε στο θέμα [Αναφορά API .NET εργοστασίου δεδομένων](https://msdn.microsoft.com/library/mt415893.aspx) . Σε αυτό το άρθρο δεν καλύπτει όλα τα API δεδομένων εργοστασίου .NET. 

 

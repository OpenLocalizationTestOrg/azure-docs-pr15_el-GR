<properties 
    pageTitle="Κωδικοποίηση με ροή εργασίας Premium κωδικοποιητή πολυμέσων για προχωρημένους | Microsoft Azure" 
    description="Μάθετε πώς να κωδικοποιήσετε με ροή εργασίας Premium κωδικοποιητή πολυμέσων. Δείγματα κώδικα είναι γραμμένες σε C# και χρησιμοποιήστε το Media Services SDK για .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Κωδικοποίηση με ροή εργασίας Premium κωδικοποιητή πολυμέσων για προχωρημένους

>[AZURE.NOTE] Επεξεργαστής πολυμέσων ροής εργασίας Premium κωδικοποιητή πολυμέσων που περιγράφονται σε αυτό το θέμα δεν είναι διαθέσιμη στην Κίνα.

Για ερωτήσεις encoder premium, ηλεκτρονικού ταχυδρομείου mepd στο Microsoft.com.

##<a name="overview"></a>Επισκόπηση

Ο επεξεργαστής πολυμέσων **Ροής εργασίας Premium κωδικοποιητή πολυμέσων** παρουσίαση του Microsoft Azure Media Services. Αυτό επεξεργαστής προσφορών εκ των προτέρων κωδικοποίηση δυνατότητες για τις ροές εργασίας σε ζήτηση premium. 

Τα παρακάτω θέματα της διάρθρωσης λεπτομερειών που σχετίζονται με **Ροή εργασίας Premium κωδικοποιητή πολυμέσων**: 

- [Μορφές που υποστηρίζονται από τη ροή εργασίας Media Encoder Premium](media-services-premium-workflow-encoder-formats.md) – Discusses το κωδικοποιητών και μορφές αρχείων που υποστηρίζονται από τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων**.

- Στην ενότητα [σύγκριση κωδικοποιητές](media-services-encode-asset.md#compare_encoders) συγκρίνει τις δυνατότητες κωδικοποίησης της **Ροής εργασίας Premium κωδικοποιητή πολυμέσων** και των **Τυπικών κωδικοποιητή πολυμέσων**.

Αυτό το θέμα παρουσιάζει πώς μπορείτε να κωδικοποιήσετε με **Ροή εργασίας Premium κωδικοποιητή πολυμέσων** χρησιμοποιώντας .NET.

Κωδικοποίησης εργασίες για τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων** απαιτούν ένα αρχείο ρύθμισης παραμέτρων ξεχωριστά, που ονομάζεται αρχείο ροής εργασίας. Αυτά τα αρχεία έχουν επέκταση .workflow και δημιουργούνται με χρήση του εργαλείου [Σχεδίασης ροής εργασίας](media-services-workflow-designer.md) .

##<a name="encode"></a>Κωδικοποίηση

Κωδικοποίησης εργασίες για τη **Ροή εργασίας Premium κωδικοποιητή πολυμέσων** απαιτούν ένα αρχείο ρύθμισης παραμέτρων ξεχωριστά, που ονομάζεται αρχείο ροής εργασίας. Αυτά τα αρχεία έχουν επέκταση .workflow και δημιουργούνται με χρήση του εργαλείου [Σχεδίασης ροής εργασίας](media-services-workflow-designer.md) .


Μπορείτε επίσης να λάβετε την προεπιλεγμένη αρχεία της ροής εργασίας [εδώ](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Ο φάκελος περιέχει επίσης την περιγραφή αυτών των αρχείων.

Τα αρχεία ροή εργασίας πρέπει να αποσταλεί στο λογαριασμό σας Media Services ως ενός περιουσιακού στοιχείου και αυτού του περιουσιακού στοιχείου πρέπει να δώσετε στο κωδικοποίησης στην εργασία.

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να κωδικοποιήσετε με **Ροή εργασίας Premium κωδικοποιητή πολυμέσων**. 

Πραγματοποιούνται τα παρακάτω βήματα: 
 
1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο ροής εργασίας. 
2. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο προέλευσης πολυμέσων.
3. Λάβετε τον επεξεργαστή πολυμέσων "Media Encoder Premium ροής εργασίας".
4. Δημιουργήστε μια εργασία και μια εργασία. 

    Στις περισσότερες περιπτώσεις, η συμβολοσειρά ρύθμισης παραμέτρων για την εργασία είναι κενό (όπως στο παράδειγμα που ακολουθεί). Υπάρχουν ορισμένα σενάρια για προχωρημένους (που απαιτείται για να ορίσετε ιδιότητες χρόνου εκτέλεσης δυναμικά) σε αυτήν την περίπτωση που θα παρέχουν μια συμβολοσειρά XML στην εργασία κωδικοποίησης. Παραδείγματα τέτοιων σεναρίων είναι: δημιουργία μια επικάλυψη, παράλληλες ή διαδοχική πολυμέσων συρράψετε, υποτιτλισμός.
5. Προσθέστε δύο στοιχεία εισόδου με την εργασία.
    
    μια. το 1ος – παγίου ροής εργασίας.

    β. 2η – βίντεο περιουσιακού στοιχείου.
    
    **Σημείωση**: παγίου ροή εργασίας πρέπει να προστεθεί στην εργασία πριν από την διαθέσιμων στοιχείων πολυμέσων. Η συμβολοσειρά ρύθμισης παραμέτρων για αυτήν την εργασία, θα πρέπει να είναι κενή. 

6. Υποβάλετε την εργασία κωδικοποίησης.

Ακολουθεί ένα παράδειγμα ολοκλήρωσης. Για πληροφορίες σχετικά με τον τρόπο ρύθμισης .NET υπηρεσίες πολυμέσων στην ανάπτυξη, ανατρέξτε στο θέμα [Ανάπτυξη Media Services με .NET](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            // Field for service context.
            private static CloudMediaContext _context = null;
            private static MediaServicesCredentials _cachedCredentials = null;
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);
    
                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Για ερωτήσεις encoder premium, ηλεκτρονικού ταχυδρομείου mepd στο Microsoft.com.

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

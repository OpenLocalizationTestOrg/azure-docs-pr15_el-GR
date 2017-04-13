<properties 
    pageTitle="Διαχείριση πολυμέσων υπηρεσιών περιουσιακών στοιχείων σε πολλαπλούς λογαριασμούς χώρου αποθήκευσης | Microsoft Azure" 
    description="Αυτό το άρθρο σάς παρέχουν οδηγίες σχετικά με τον τρόπο για να διαχειριστείτε πόρους υπηρεσίες πολυμέσων σε πολλαπλούς λογαριασμούς χώρου αποθήκευσης." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


#<a name="managing-media-services-assets-across-multiple-storage-accounts"></a>Διαχείριση πολυμέσων υπηρεσιών περιουσιακών στοιχείων σε πολλαπλούς λογαριασμούς χώρου αποθήκευσης

Ξεκινώντας με το Microsoft Azure Media Services 2.2, μπορείτε να επισυνάψετε πολλούς λογαριασμούς χώρου αποθήκευσης σε ένα λογαριασμό Media Services. Δυνατότητα για να επισυνάψετε πολλούς λογαριασμούς χώρου αποθήκευσης σε ένα λογαριασμό Media Services παρέχει τα ακόλουθα πλεονεκτήματα:

- Την εξισορρόπηση φόρτου τους πόρους σας σε πολλαπλούς λογαριασμούς χώρου αποθήκευσης.
- Υπηρεσίες πολυμέσων κλίμακας για μεγάλες ποσότητες επεξεργασίας περιεχομένου (όπως τη συγκεκριμένη στιγμή ένα λογαριασμό μόνο χώρο αποθήκευσης έχει μέγιστο όριο 500 TB). 

Αυτό το θέμα παρουσιάζει τον τρόπο για να επισυνάψετε πολλούς λογαριασμούς χώρου αποθήκευσης σε έναν λογαριασμό Media Services με χρήση Azure υπηρεσίας διαχείρισης REST API. Εμφανίζει επίσης πώς μπορείτε να καθορίσετε διαφορετικό χώρο αποθήκευσης λογαριασμούς κατά τη δημιουργία περιουσιακών στοιχείων με χρήση του SDK υπηρεσίες πολυμέσων. 

##<a name="considerations"></a>Ζητήματα

Κατά την επισύναψη πολλούς λογαριασμούς χώρου αποθήκευσης στο λογαριασμό σας Media Services, ισχύουν τα ακόλουθα θέματα:

- Όλοι οι λογαριασμοί χώρου αποθήκευσης που έχουν επισυναφθεί σε ένα λογαριασμό Media Services πρέπει να είναι στο ίδιο κέντρο δεδομένων με το λογαριασμό Media Services.
- Προς το παρόν, όταν ένα λογαριασμό του χώρου αποθήκευσης είναι συνδεδεμένο με το καθορισμένο λογαριασμό Media Services, το δεν είναι δυνατή η αποσύνδεση.
- Ο λογαριασμός πρωτεύοντος χώρου αποθήκευσης είναι αυτό που υποδεικνύεται κατά τη διάρκεια της ώρας δημιουργίας λογαριασμού Media Services. Προς το παρόν, δεν μπορείτε να αλλάξετε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης. 

Άλλα θέματα:

Υπηρεσίες πολυμέσων χρησιμοποιεί την τιμή της ιδιότητας **IAssetFile.Name** κατά τη δημιουργία διευθύνσεις URL για τη ροή περιεχομένου (για παράδειγμα, http://{WAMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Για αυτόν το λόγο, δεν επιτρέπεται η κωδικοποίηση. Η τιμή της ιδιότητας όνομα δεν μπορεί να έχει έναν από τους παρακάτω [χαρακτήρες τοις εκατό κωδικοποίηση-αποκλειστικές](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Επίσης, μπορεί να υπάρξει μόνο μία '.' για την επέκταση ονόματος αρχείου.

##<a name="to-attach-a-storage-account-with-azure-service-management-rest-api"></a>Για να επισυνάψετε ένα λογαριασμό χώρου αποθήκευσης με Azure υπηρεσίας διαχείρισης REST API

Προς το παρόν, ο μόνος τρόπος για να επισυνάψετε πολλούς λογαριασμούς χώρου αποθήκευσης είναι χρησιμοποιώντας [Azure υπηρεσίας διαχείρισης REST API](http://msdn.microsoft.com/library/azure/dn167014.aspx). Το δείγμα κώδικα στο το [τον τρόπο: χρήση πολυμέσων υπηρεσίες διαχείρισης REST API](https://msdn.microsoft.com/library/azure/dn167656.aspx) θέμα ορίζει τη μέθοδο **AttachStorageAccountToMediaServiceAccount** που συνδέεται ένα λογαριασμό του χώρου αποθήκευσης για το συγκεκριμένο λογαριασμό Media Services. Ο κώδικας στο ίδιο θέμα ορίζει τη μέθοδο **ListStorageAccountDetails** που παραθέτει όλους τους λογαριασμούς χώρου αποθήκευσης που συνδέονται με το συγκεκριμένο λογαριασμό Media Services.


##<a name="to-manage-media-services-assets-across-multiple-storage-accounts"></a>Για να διαχειριστείτε πόρους Media Services σε πολλαπλούς λογαριασμούς χώρου αποθήκευσης

Ο ακόλουθος κώδικας χρησιμοποιεί την πιο πρόσφατη SDK υπηρεσίες πολυμέσων για να εκτελέσετε τις ακόλουθες εργασίες:

1. Εμφάνιση όλων των λογαριασμών χώρου αποθήκευσης που σχετίζεται με τον συγκεκριμένο λογαριασμό Media Services.
1. Ανάκτηση του ονόματος του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης.
1. Δημιουργήστε ένα νέο πάγιο στο τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης.
1. Δημιουργία ενός περιουσιακού στοιχείου εξόδου του κωδικοποίησης έργου στο χώρο αποθήκευσης στο συγκεκριμένο λογαριασμό.
    
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace MultipleStorageAccounts
        {
            class Program
            {
                // Location of the media file that you want to encode. 
                private static readonly string _singleInputFilePath =
                    Path.GetFullPath(@"../..\supportFiles\multifile\interview2.wmv");
        
                private static readonly string MediaServicesAccountName = 
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string MediaServicesAccountKey = 
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static CloudMediaContext _context;
                private static MediaServicesCredentials _cachedCredentials = null;
    
                static void Main(string[] args)
                {
    
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    MediaServicesAccountName,
                                    MediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
    
        
                    // Display the storage accounts associated with 
                    // the specified Media Services account:
                    foreach (var sa in _context.StorageAccounts)
                        Console.WriteLine(sa.Name);
        
                    // Retrieve the name of the default storage account.
                    var defaultStorageName = _context.StorageAccounts.Where(s => s.IsDefault == true).FirstOrDefault();
                    Console.WriteLine("Name: {0}", defaultStorageName.Name);
                    Console.WriteLine("IsDefault: {0}", defaultStorageName.IsDefault);
        
                    // Retrieve the name of a storage account that is not the default one.
                    var notDefaultStroageName = _context.StorageAccounts.Where(s => s.IsDefault == false).FirstOrDefault();
                    Console.WriteLine("Name: {0}", notDefaultStroageName.Name);
                    Console.WriteLine("IsDefault: {0}", notDefaultStroageName.IsDefault);
                    
                    // Create the original asset in the default storage account.
                    IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.None, 
                        defaultStorageName.Name, _singleInputFilePath);
                    Console.WriteLine("Created the asset in the {0} storage account", asset.StorageAccountName);
                    
                    // Create an output asset of the encoding job in the other storage account.
                    IAsset outputAsset = CreateEncodingJob(asset, notDefaultStroageName.Name, _singleInputFilePath);
                    if(outputAsset!=null)
                        Console.WriteLine("Created the output asset in the {0} storage account", outputAsset.StorageAccountName);
        
                }
        
                static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string storageName, string singleFilePath)
                {
                    var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                    
                    // If you are creating an asset in the default storage account, you can omit the StorageName parameter.
                    var asset = _context.Assets.Create(assetName, storageName, assetCreationOptions);
        
                    var fileName = Path.GetFileName(singleFilePath);
        
                    var assetFile = asset.AssetFiles.Create(fileName);
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    return asset;
                }
        
                static IAsset CreateEncodingJob(IAsset asset, string storageName, string inputMediaFilePath)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My encoding job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.ProtectedConfiguration);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset", storageName,
                        AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new
                            EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish. 
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                    progressJobTask.Wait();
        
                    // Get an updated job reference.
                    job = GetJob(job.Id);
        
                    // If job state is Error the event handling 
                    // method for job progress should log errors.  Here we check 
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        Console.WriteLine("\nExiting method due to job error.");
                        return null;
                    }
        
                    // Get a reference to the output asset from the job.
                    IAsset outputAsset = job.OutputMediaAssets[0];
        
                    return outputAsset;
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                private static void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("********************");
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine("Please wait while local tasks or downloads complete...");
                            Console.WriteLine("********************");
                            Console.WriteLine();
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
                            Console.WriteLine("An error occurred in {0}", job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
                static IJob GetJob(string jobId)
                {
                    // Use a Linq select query to get an updated 
                    // reference by Id. 
                    var jobInstance =
                        from j in _context.Jobs
                        where j.Id == jobId
                        select j;
                    // Return the job reference as an Ijob. 
                    IJob job = jobInstance.FirstOrDefault();
        
                    return job;
                }
            }
        }
 

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

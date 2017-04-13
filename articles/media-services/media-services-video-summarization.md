<properties
    pageTitle="Χρησιμοποιήστε πολυμέσων Azure μικρογραφίες βίντεο για να δημιουργήσετε μια σύνοψη βίντεο | Microsoft Azure"
    description="Σύνοψη βίντεο μπορεί να σας βοηθήσει να δημιουργήσετε συνόψεις μεγάλη βίντεο επιλέγοντας αυτόματα ενδιαφέρον τμήματα κώδικα από το βίντεο προέλευσης. Αυτό είναι χρήσιμο όταν θέλετε να δώσετε μια γρήγορη επισκόπηση των τι να περιμένει στο μεγάλο βίντεο."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>

#<a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Χρησιμοποιήστε πολυμέσων Azure μικρογραφίες βίντεο για να δημιουργήσετε μια σύνοψη βίντεο
##<a name="overview"></a>Επισκόπηση

Ο επεξεργαστής πολυμέσων **Azure Media βίντεο μικρογραφίες** (πολλών Επεξεργαστών) σάς επιτρέπει να δημιουργήσετε μια σύνοψη των βίντεο που είναι χρήσιμο για τους πελάτες που απλώς θέλετε να δείτε σε προεπισκόπηση μια σύνοψη των μεγάλη βίντεο. Για παράδειγμα, οι πελάτες μπορεί να θέλετε να δείτε μια σύντομη "Σύνοψη βίντεο" όταν τους το δείκτη του ποντικιού επάνω από μια μικρογραφία. Προσαρμόζοντας τις παραμέτρους των **μικρογραφιών Azure Media Video** μέσω μιας προκαθορισμένης ρύθμισης παραμέτρων, μπορείτε να χρησιμοποιήσετε του πολλών Επεξεργαστών ισχυρή στιγμιότυπο ανίχνευσης και συνένωση τεχνολογία για τη δημιουργία algorithmically ένα περιγραφικό subclip.  

Η **Μικρογραφία βίντεο πολυμέσων Azure** πολλών Επεξεργαστών είναι αυτήν τη στιγμή στην προεπισκόπηση.

Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με τη **Μικρογραφία βίντεο πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET

##<a name="video-summary-example"></a>Παράδειγμα βίντεο σύνοψης 

Ακολουθούν ορισμένα παραδείγματα του τι μπορεί να κάνει ο επεξεργαστής πολυμέσων Azure Media Video μικρογραφίες:

###<a name="original-video"></a>Αρχικό βίντεο

[Αρχικό βίντεο](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

###<a name="video-thumbnail-result"></a>Αποτέλεσμα μικρογραφίας του βίντεο

[Αποτέλεσμα μικρογραφίας του βίντεο](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="task-configuration-preset"></a>Ρύθμιση παραμέτρων εργασίας (προεπιλογή)

Κατά τη δημιουργία βίντεο μικρογραφιών εργασίας με **Βίντεο μικρογραφίες πολυμέσων Azure**, πρέπει να καθορίσετε μια προκαθορισμένη ρύθμιση παραμέτρων. Το παραπάνω παράδειγμα μικρογραφίας που δημιουργήθηκε με τις ακόλουθες βασικές ρυθμίσεις JSON:

    {"version":"1.0"}

Προς το παρόν, μπορείτε να αλλάξετε τις ακόλουθες παραμέτρους:

Παραμέτρου|Περιγραφή
---|---
outputAudio|Καθορίζει ή όχι η εικόνα που προκύπτει περιέχει στοιχεία ήχου. <br/>Επιτρέπονται τιμές είναι: True ή False. Η προεπιλογή είναι True.
fadeInFadeOut|Καθορίζει ή όχι μεταβάσεις σβησίματος χρησιμοποιούνται μεταξύ των μικρογραφιών ξεχωριστή κίνησης.  <br/>Επιτρέπονται τιμές είναι: True ή False.  Η προεπιλογή είναι True.
maxMotionThumbnailDurationInSecs|Ακέραιος αριθμός που καθορίζει πόσο χρόνο πρέπει να ολόκληρου του αποτελέσματος βίντεο.  Προεπιλεγμένη εξαρτάται από το αρχικό βίντεο διάρκειας.


Ο παρακάτω πίνακας περιγράφει την προεπιλεγμένη διάρκεια, όταν δεν χρησιμοποιείται **maxMotionThumbnailInSecs** .

||||
---|---|---|---|---
Βίντεο διάρκειας|d < 3 min|3 min < d < 15 λεπτά|15 λεπτά < d < 30 min| 30 min < d
Διάρκεια μικρογραφιών|15 sec (σκηνών 2-3)| 30 δευτερόλεπτα (3-5 σκηνών)|60 sec (σκηνών 5-10)|90 sec (σκηνών 10-15)


Το παρακάτω JSON ορίζει διαθέσιμες παραμέτρους.
    
    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργεί μια εργασία με βίντεο μικρογραφιών εργασίας που βασίζεται σε ένα αρχείο ρύθμισης παραμέτρων που περιέχει το παρακάτω υπόδειγμα json. 
        
        {               
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }

1. Η λήψη των αρχείων εξόδου. 

###<a name="net-code"></a>Κωδικός .NET
     
    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;
    
    namespace VideoSummarization
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
    
            static void Main(string[] args)
            {
    
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    

                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");
    
                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }
    
            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);
    
                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");
    
                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";
    
                var processor = GetLatestMediaProcessorByName(MediaProcessorName);
    
                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);
    
                // Specify the input asset.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
    
                progressJobTask.Wait();
    
                // If job state is Error, the event handling
                // method for job progress should log errors.  Here we check
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                    Console.WriteLine(string.Format("Error: {0}. {1}",
                                                    error.Code,
                                                    error.Message));
                    return null;
                }
    
                return job.OutputMediaAssets[0];
            }
    
            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);
    
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);
    
                return asset;
            }
    
            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }
    
            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));
    
                return processor;
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
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
        }
    }

###<a name="video-thumbnail-output"></a>Έξοδος μικρογραφίας βίντεο

[Έξοδος μικρογραφίας βίντεο](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
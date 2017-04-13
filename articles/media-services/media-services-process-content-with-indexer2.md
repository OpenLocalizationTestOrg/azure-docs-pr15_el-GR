<properties
    pageTitle="Δημιουργία ευρετηρίου αρχεία πολυμέσων με την προεπισκόπηση ευρετηρίου 2 πολυμέσων Azure | Microsoft Azure"
    description="Azure Media ευρετηρίου σας δίνει τη δυνατότητα για να κάνετε περιεχόμενο από τα αρχεία πολυμέσων με δυνατότητα αναζήτησης και για να δημιουργήσετε ένα αντίγραφο πλήρους κειμένου για κλειστές λεζάντες και λέξεις-κλειδιά. Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε την προεπισκόπηση 2 ευρετηρίου πολυμέσων."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Δημιουργία ευρετηρίου αρχεία πολυμέσων με την προεπισκόπηση ευρετηρίου 2 πολυμέσων Azure

##<a name="overview"></a>Επισκόπηση

Ο επεξεργαστής πολυμέσων **Azure Media ευρετηρίου 2 Preview** (πολλών Επεξεργαστών) σάς επιτρέπει να κάνετε αρχεία πολυμέσων και το περιεχόμενο με δυνατότητα αναζήτησης, καθώς και να δημιουργήσετε κλειστές λεζάντες κομμάτια. Σε σύγκριση με την προηγούμενη έκδοση του [Ευρετηρίου πολυμέσων Azure](media-services-index-content.md), **Προεπισκόπηση 2 ευρετηρίου πολυμέσων Azure** εκτελεί δημιουργίας ευρετηρίου για ταχύτερη και προσφέρει ευρύτερης υποστήριξη γλωσσών. Υποστηριζόμενες γλώσσες περιλαμβάνουν Αγγλικά, Ισπανικά, γαλλικά, γερμανικά, ιταλικά, Κινεζικά, πορτογαλικά και τα Αραβικά.

Η **Προεπισκόπηση 2 ευρετηρίου πολυμέσων Azure** πολλών Επεξεργαστών είναι αυτήν τη στιγμή στην προεπισκόπηση.

Αυτό το θέμα περιγράφει τον τρόπο δημιουργίας εργασιών ευρετηρίου με **Azure Media ευρετηρίου 2 Preview**.

>[AZURE.NOTE]Ισχύουν τα ακόλουθα θέματα:
>
>Ευρετήριο 2 δεν υποστηρίζεται στην Κίνα Azure και για δημόσιους οργανισμούς Azure.
>
>Η προεπισκόπηση περιορίζεται στα ~ 10 λεπτά της επεξεργασίας, αλλά είναι δωρεάν σε όλους τους πελάτες.
>
>Κατά τη δημιουργία ευρετηρίου περιεχομένου, βεβαιωθείτε ότι χρησιμοποιείτε αρχεία πολυμέσων που έχουν πολύ Απαλοιφή ομιλίας (χωρίς φόντο μουσικής, θορύβου, εφέ ή hiss μικρόφωνο). Ορισμένα παραδείγματα κατάλληλο περιεχόμενο είναι: καταγραφεί συσκέψεις, διαλέξεις ή παρουσιάσεις. Το παρακάτω περιεχόμενο ενδέχεται να μην είναι κατάλληλη για τη δημιουργία ευρετηρίου: ταινίες, εμφανίζει Τηλεοπτική, όλα με μεικτή ήχου και ηχητικά εφέ, καταγραφεί ακατάλληλα περιεχομένου με θόρυβο (hiss).


Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με την **Προεπισκόπηση 2 ευρετηρίου πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET

##<a name="input-and-output-files"></a>Αρχεία εισόδου και εξόδου

###<a name="input-files"></a>Αρχεία εισόδου

Αρχεία ήχου ή βίντεο

###<a name="output-files"></a>Αρχεία εξόδου

Μια εργασία δημιουργίας ευρετηρίου μπορεί να δημιουργήσει αρχεία κλειστές λεζάντες με τις παρακάτω μορφές:  

- **Σάμι**
- **TTML**
- **WebVTT**

Κλειστό λεζάντα (Κοιν.) αρχεία σε αυτές τις μορφές μπορούν να χρησιμοποιηθούν για να κάνετε προσβάσιμες για άτομα με ειδικές ανάγκες ακοή αρχείων ήχου και βίντεο.

##<a name="task-configuration-preset"></a>Ρύθμιση παραμέτρων εργασίας (προεπιλογή)

Κατά τη δημιουργία μιας εργασίας δημιουργίας ευρετηρίου με την **Προεπισκόπηση 2 ευρετηρίου πολυμέσων Azure**, πρέπει να καθορίσετε μια προκαθορισμένη ρύθμιση παραμέτρων.

Το παρακάτω JSON ορίζει διαθέσιμες παραμέτρους.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Υποστηριζόμενες γλώσσες  

Azure Media ευρετηρίου 2 Preview υποστηρίζει ομιλίας σε κείμενο για τις ακόλουθες γλώσσες (όταν ορίζετε το όνομα γλώσσας στο τις παραμέτρους της εργασίας, χρησιμοποιήστε 4-ψήφιο κωδικό μέσα σε αγκύλες, όπως φαίνεται παρακάτω):

- Αγγλικά [EnUs]
- Ισπανικά [EsEs]
- Κινεζικά [ZhCn]
- Γαλλικά [FrFr]
- Γερμανικά [DeDe]
- Ιταλικά [ItIt]
- Πορτογαλικά [PtBr]
- Αραβικά (αλεξανδρινό) [ArEg]


## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργεί μια εργασία με μια εργασία δημιουργίας ευρετηρίου που βασίζεται σε ένα αρχείο ρύθμισης παραμέτρων που περιέχει το παρακάτω υπόδειγμα json.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Η λήψη των αρχείων εξόδου. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
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
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
<properties
    pageTitle="Πρόσωπο έκθεσης με αναλυτικά στοιχεία πολυμέσων Azure | Microsoft Azure"
    description="Αυτό το θέμα παρουσιάζει τον τρόπο για να προχωρήσετε όψεις με αναλυτικά στοιχεία πολυμέσων Azure σε."
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
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Πρόσωπο έκθεσης με αναλυτικά στοιχεία πολυμέσων Azure

##<a name="overview"></a>Επισκόπηση

**Redactor πολυμέσων Azure** είναι [Αναλυτικών στοιχείων πολυμέσων Azure](media-services-analytics-overview.md) media επεξεργαστή (πολλών Επεξεργαστών) που προσφέρει έκθεσης με πρόσωπο στο cloud. Πρόσωπο έκθεσης σάς επιτρέπει να τροποποιήσετε το βίντεό σας προκειμένου να θάμπωμα όψεις των επιλεγμένων ατόμων. Ενδέχεται να θέλετε να χρησιμοποιήσετε την υπηρεσία έκθεσης πρόσωπο σε δημόσια ασφάλεια και συζητήσεων πολυμέσων σενάρια. Μερικά λεπτά αποσπάσματος που περιέχει πολλές όψεις μπορεί να διαρκέσει ώρες για να προχωρήσετε με μη αυτόματο τρόπο σε, αλλά με αυτήν την υπηρεσία η διαδικασία έκθεσης πρόσωπο θα απαιτούν λίγα απλά βήματα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτό](https://azure.microsoft.com/blog/azure-media-redactor/) το ιστολόγιο.

Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με την **Redactor πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET.

**Redactor πολυμέσων Azure** πολλών Επεξεργαστών είναι αυτήν τη στιγμή στην προεπισκόπηση.

## <a name="face-redaction-modes"></a>Λειτουργίες έκθεσης πρόσωπο

Του προσώπου έκθεσης λειτουργεί με τον εντοπισμό όψεις σε κάθε πλαίσιο του βίντεο και την παρακολούθηση το πρόσωπο αντικείμενο και τα δύο προς τα εμπρός και προς τα πίσω σε χρόνο, έτσι ώστε το ίδιο άτομο μπορεί να είναι δυσδιάκριτα από καθώς και άλλες γωνίες. Η διαδικασία αυτόματης έκθεσης είναι πολύ περίπλοκο και σας δεν πάντα αγροτικά προϊόντα 100% επιθυμητή εξόδου, για αυτόν το λόγο αναλυτικών στοιχείων πολυμέσων παρέχει με δύο τρόποι για να τροποποιήσετε το τελικό αποτέλεσμα.

Εκτός από ένα πλήρως αυτόματη λειτουργία, υπάρχει μια ροή εργασίας δύο κατευθύνσεων που επιτρέπει την επιλογή/de-selection της βρέθηκαν όψεις μέσω μια λίστα αναγνωριστικών. Επίσης, να κάνετε αυθαίρετο ανά πλαισίου προσαρμογές του πολλών Επεξεργαστών χρησιμοποιεί ένα αρχείο μετα-δεδομένων σε μορφή JSON. Αυτή η ροή εργασίας χωρίζεται σε λειτουργίες **ανάλυση** και **Redact** . Μπορείτε να συνδυάσετε τις δύο καταστάσεις λειτουργίας σε μία φορά που θα εκτελείται και τις δύο εργασίες στη μία εργασία; Αυτή η κατάσταση λειτουργίας ονομάζεται **Combined**.

###<a name="combined-mode"></a>Συνολικό λειτουργία

Αυτό θα δημιουργήσουν μιας έκθεσης mp4 αυτόματα χωρίς οποιαδήποτε μη αυτόματης εισαγωγής.

Στάδιο|Όνομα αρχείου|Σημειώσεις
---|---|---
Εισαγωγής περιουσιακών στοιχείων|FOO.Bar|Βίντεο: μορφή WMV, MOV ή MP4
Ρύθμιση παραμέτρων εισαγωγής|Προκαθορισμένες παραμέτρων εργασίας|{'έκδοση':'1.0 ', 'Επιλογές': {'mode': 'συνδυάζονται'}}
Αποτέλεσμα περιουσιακών στοιχείων|foo_redacted.mp4|Βίντεο με θόλωμα εφαρμοστεί

####<a name="input-example"></a>Παράδειγμα εισαγωγής:

[δείτε αυτό το βίντεο](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Παράδειγμα εξόδου:

[δείτε αυτό το βίντεο](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Ανάλυση λειτουργία

Η φάση **ανάλυση** της ροής εργασίας δύο κατευθύνσεων λαμβάνει μια είσοδο βίντεο και δημιουργεί ένα αρχείο JSON πρόσωπο θέσεις και εικόνες jpg κάθε εντοπίστηκε πρόσωπο.

Στάδιο|Όνομα αρχείου|Σημειώσεις
---|---|----
Εισαγωγής περιουσιακών στοιχείων|FOO.Bar|Βίντεο: μορφή WMV, MPV ή MP4
Ρύθμιση παραμέτρων εισαγωγής|Προκαθορισμένες παραμέτρων εργασίας|{'έκδοση':'1.0 ', 'Επιλογές': {'mode': "Ανάλυση"}}
Αποτέλεσμα περιουσιακών στοιχείων|foo_annotations.JSON|Δεδομένα σχολίων θέσεις πρόσωπο σε μορφή JSON. Αυτό μπορεί να υποστεί επεξεργασία από το χρήστη για να τροποποιήσετε το θόλωμα πλαίσια οριοθέτησης. Δείτε παρακάτω δείγμα.
Αποτέλεσμα περιουσιακών στοιχείων|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Περικομμένη jpg κάθε εντοπίζονται πρόσωπο, όπου ο αριθμός υποδεικνύει το αναγνωριστικού ετικέτας από το πρόσωπο

####<a name="output-example"></a>Παράδειγμα εξόδου:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… περικοπή


###<a name="redact-mode"></a>Έκθεση λειτουργία

Η δεύτερη φάση της ροής εργασίας σας μεταφέρει μεγαλύτερο αριθμό των εισόδων που πρέπει να συνδυάζονται σε ένα μεμονωμένο περιουσιακών στοιχείων.

Αυτό περιλαμβάνει μια λίστα αναγνωριστικών σε θάμπωμα, το αρχικό βίντεο και τα σχόλια JSON. Αυτή η κατάσταση λειτουργίας χρησιμοποιεί τα σχόλια για να εφαρμόσετε θόλωμα στην είσοδο βίντεο.

Το αποτέλεσμα από τη φάση ανάλυση δεν περιλαμβάνει το αρχικό βίντεο. Το βίντεο πρέπει να αποσταλεί σε εισαγωγής περιουσιακού στοιχείου για την εργασία λειτουργία Redact και επιλεγεί ως το αρχικό αρχείο.

Στάδιο|Όνομα αρχείου|Σημειώσεις
---|---|---
Εισαγωγής περιουσιακών στοιχείων|FOO.Bar|Βίντεο σε μορφή WMV, MPV ή MP4. Ίδια βίντεο όπως στο βήμα 1.
Εισαγωγής περιουσιακών στοιχείων|foo_annotations.JSON|αρχείο μετα-δεδομένων σχόλια από φάση ένα, με προαιρετικό τροποποιήσεις.
Εισαγωγής περιουσιακών στοιχείων|foo_IDList.txt (προαιρετικά)|Προαιρετικό νέα γραμμή διαχωρισμένες πρόσωπο αναγνωριστικά για να προχωρήσετε σε λίστα. Εάν μείνει κενό, αυτό Θολώνει όλες τις όψεις.
Ρύθμιση παραμέτρων εισαγωγής|Προκαθορισμένες παραμέτρων εργασίας|{'έκδοση':'1.0 ', 'Επιλογές': {'mode': 'έκθεση'}}
Αποτέλεσμα περιουσιακών στοιχείων|foo_redacted.mp4|Βίντεο με θόλωμα εφαρμογή που βασίζεται σε σχόλια

####<a name="example-output"></a>Παράδειγμα εξόδου

Αυτό είναι το αποτέλεσμα από μια IDList με ένα Αναγνωριστικό που επιλέξατε.

[δείτε αυτό το βίντεο](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Περιγραφές των χαρακτηριστικών

Το πολλών Επεξεργαστών έκθεσης παρέχει εντοπισμού θέσης προσώπων υψηλής ακρίβειας και παρακολούθησης που μπορεί να εντοπίσει έως 64 human όψεις σε ένα πλαίσιο του βίντεο. Μετωπικής όψεις παρέχουν τα καλύτερα αποτελέσματα, ενώ πλαϊνές όψεις και μικρές όψεις (μικρότερη ή ίση με 24 x 24 pixel) είναι δύσκολη.

Τις όψεις Εντοπίστηκε και εντοπισμένες επιστρέφονται με συντεταγμένες που υποδεικνύει τη θέση του όψεις, καθώς και ένα πρόσωπο που υποδεικνύει την παρακολούθηση του αριθμού Αναγνωριστικού. Αριθμούς των Αναγνωριστικών πρόσωπο είναι πιθανό να επαναφέρετε υπό συνθήκες, όταν το πρόσωπο μετωπικής χαθεί ή επικαλυπτόμενη στο πλαίσιο, αποτέλεσμα ορισμένα άτομα γρήγορα στους οποίους έχουν ανατεθεί πολλαπλές ταυτότητες.

Για λεπτομερείς εξηγήσεις για τα χαρακτηριστικά, ανατρέξτε στο θέμα [Εντοπισμός πρόσωπο και συναισθήματα με αναλυτικά στοιχεία πολυμέσων Azure](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργήστε μια εργασία με μια εργασία έκθεσης πρόσωπο που βασίζεται σε ένα αρχείο ρύθμισης παραμέτρων που περιέχει το παρακάτω υπόδειγμα json. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Κάντε λήψη των αρχείων JSON εξόδου. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

<properties
    pageTitle="Χρησιμοποιήστε ανάλυση πολυμέσων Azure για να μετατρέψετε κείμενο περιεχομένου σε αρχεία βίντεο σε ψηφιακό κείμενο | Microsoft Azure"
    description="Azure Media ανάλυση OCR (οπτική αναγνώριση χαρακτήρων) σάς επιτρέπει να μετατρέψετε κείμενο περιεχομένου σε αρχεία βίντεο σε ψηφιακό κείμενο με δυνατότητα επεξεργασίας, με δυνατότητα αναζήτησης.  Αυτό σας επιτρέπει να αυτοματοποιήσετε την εξαγωγή χαρακτηριστικό μετα-δεδομένων από το σήμα βίντεο τα πολυμέσα."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Χρησιμοποιήστε ανάλυση πολυμέσων Azure για να μετατρέψετε κείμενο περιεχομένου σε αρχεία βίντεο σε ψηφιακό κείμενο 

##<a name="overview"></a>Επισκόπηση

Εάν θέλετε να εξαγάγετε περιεχόμενο κειμένου από τα αρχεία βίντεο και να δημιουργήσετε ένα ψηφιακό κείμενο με δυνατότητα επεξεργασίας, με δυνατότητα αναζήτησης, πρέπει να χρησιμοποιήσετε OCR αναλυτικών στοιχείων πολυμέσων Azure (οπτική αναγνώριση χαρακτήρων). Αυτόν τον επεξεργαστή πολυμέσων Azure εντοπίζει περιεχόμενο κειμένου σε αρχεία βίντεο και δημιουργεί αρχεία κειμένου για δική σας χρήση. OCR σάς επιτρέπει να αυτοματοποιήσετε την εξαγωγή χαρακτηριστικό μετα-δεδομένων από το σήμα βίντεο τα πολυμέσα.

Όταν χρησιμοποιείται σε συνδυασμό με μια μηχανή αναζήτησης, μπορείτε εύκολα index τα πολυμέσα από κείμενο, και να βελτιώσετε τη δυνατότητα εντοπισμού του περιεχομένου σας. Αυτό είναι πολύ χρήσιμο σε ιδιαίτερα που περιέχουν κείμενο το βίντεο, όπως μια εγγραφή βίντεο ή καταγραφής οθόνης από μια παρουσίαση προβολής παρουσίασης. Ο επεξεργαστής Azure OCR πολυμέσων είναι βελτιστοποιημένη για ψηφιακή κειμένου.

Ο επεξεργαστής πολυμέσων **Azure Media OCR** είναι αυτήν τη στιγμή στην προεπισκόπηση.

Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με **OCR πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET. Για πρόσθετες πληροφορίες και παραδείγματα, ανατρέξτε στο θέμα [αυτό το ιστολόγιο](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

##<a name="ocr-input-files"></a>Αρχεία εισόδου OCR

Αρχεία βίντεο. Προς το παρόν, υποστηρίζονται οι παρακάτω μορφές: MP4, MOV και WMV.

##<a name="task-configuration"></a>Ρύθμιση παραμέτρων εργασίας 

Ρύθμιση παραμέτρων εργασίας (προεπιλογή). Κατά τη δημιουργία μιας εργασίας με **OCR πολυμέσων Azure**, πρέπει να καθορίσετε μια ρύθμιση παραμέτρων προκαθορισμένες χρησιμοποιώντας JSON ή XML. 

###<a name="attribute-descriptions"></a>Περιγραφές των χαρακτηριστικών

Όνομα χαρακτηριστικού|Περιγραφή
---|---
Γλώσσα|(προαιρετικό) περιγράφει τη γλώσσα του κειμένου για το οποίο θα γίνει αναζήτηση. Ένα από τα εξής: Αυτόματος εντοπισμός (προεπιλογή), Αραβικά, ChineseSimplified, ChineseTraditional, Τσεχικά δανικά, ολλανδικά, Αγγλικά, φινλανδικά, γαλλικά, γερμανικά, ελληνικά, ουγγρικά, ιταλικά, ιαπωνικά, κορεατικά, νορβηγικά, πολωνικά, πορτογαλικά, Ρουμανικά, ρωσικά, Σερβικά (Κυριλλικά), SerbianLatin, Σλοβακικά, Ισπανικά, σουηδικά, τουρκικά.
TextOrientation|(προαιρετικό) περιγράφει τον προσανατολισμό του κειμένου για το οποίο θα γίνει αναζήτηση.  "Αριστερά" σημαίνει ότι είναι αιχμές επάνω από όλα τα γράμματα προς τα αριστερά.  Το προεπιλεγμένο κείμενο (όπως αυτό που βρίσκονται σε βιβλίο) μπορεί να ονομάζεται "Επάνω" προσανατολισμό.  Ένα από τα εξής: Αυτόματος εντοπισμός (προεπιλογή), επάνω, προς τα δεξιά, προς τα κάτω, προς τα αριστερά.
TimeInterval|(προαιρετικό) περιγράφει το επιτόκιο δειγματοληψία.  Η προεπιλογή είναι κάθε δευτερόλεπτο 1/2.<br/>Μορφή JSON – HH:mm:ss. SSS (προεπιλεγμένη 00:00:00.500)<br/>Μορφή XML – στοιχειώδης διάρκεια W3C XSD (προεπιλογή PT0.5)
DetectRegions|(προαιρετικό) Ένας πίνακας DetectRegion αντικειμένων που καθορίζει τις περιοχές μέσα σε πλαίσιο του βίντεο στο οποίο θέλετε να εντοπίσει κείμενο.<br/>Ένα αντικείμενο DetectRegion πραγματοποιείται από τις παρακάτω τέσσερις ακέραιες τιμές:<br/>Αριστερά-pixel από το αριστερό περιθώριο<br/>Επάνω-pixel από το επάνω περιθώριο<br/>Πλάτος-πλάτους της περιοχής σε pixel<br/>Ύψος-ύψος της περιοχής σε pixel

####<a name="json-preset-example"></a>Παράδειγμα προκαθορισμένων JSON
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Παράδειγμα προκαθορισμένων XML

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>Αρχεία εξόδου OCR

Το αποτέλεσμα του επεξεργαστή πολυμέσων OCR είναι ένα αρχείο JSON.

###<a name="elements-of-the-output-json-file"></a>Στοιχεία του αρχείου JSON εξόδου

Το αποτέλεσμα του βίντεο OCR παρέχει φέρουν κατά διαστήματα ώρα δεδομένα σχετικά με τους χαρακτήρες που βρέθηκαν στο βίντεό σας.  Μπορείτε να χρησιμοποιήσετε χαρακτηριστικά όπως γλώσσα ή προσανατολισμού για ηλεφώνου σε ακριβώς τις λέξεις που σας ενδιαφέρουν την ανάλυση. 

Το αποτέλεσμα περιλαμβάνει τα παρακάτω χαρακτηριστικά:

Στοιχείο|Περιγραφή
---|---
Κλίμακα χρόνου|"Υποδιαιρέσεις" ανά δευτερόλεπτο του βίντεο
Μετατόπιση|μετατόπιση για χρονικές σημάνσεις ώρας. Στην έκδοση 1.0 των API του βίντεο, αυτό θα είναι πάντα 0.
Ταχύτητα καρέ|Καρέ το δευτερόλεπτο του βίντεο
πλάτος|πλάτος του βίντεο σε pixel
ύψος|ύψος του βίντεο σε pixel
Τμήματα|πίνακας με βάσει χρόνου μπλοκ βίντεο στο οποίο υπάρχει κατατμημένη τα μετα-δεδομένα
Έναρξη|ώρα έναρξης της ένα απόσπασμα σε "Υποδιαιρέσεις"
διάρκεια|μήκος του ένα απόσπασμα σε "Υποδιαιρέσεις"
χρονικό διάστημα|διάστημα κάθε συμβάν μέσα στο συγκεκριμένο τμήμα
συμβάντα|Πίνακας που περιέχει τις περιοχές
περιοχή|αντικείμενο που αντιπροσωπεύει εντοπίζονται λέξεις ή φράσεις
γλώσσα|γλώσσα του κειμένου εντοπίζονται μέσα σε μια περιοχή
Προσανατολισμός|τον προσανατολισμό του κειμένου εντοπίζονται μέσα σε μια περιοχή
γραμμές|πίνακας με γραμμές του κειμένου που εντοπίστηκε μέσα σε μια περιοχή
κείμενο|το πραγματικό κείμενο

###<a name="json-output-example"></a>Παράδειγμα JSON εξόδου

Το παρακάτω παράδειγμα εξόδου περιέχει τις γενικές πληροφορίες βίντεο και πολλά τμήματα βίντεο. Σε κάθε απόσπασμα βίντεο, περιέχει κάθε περιοχή που ανιχνεύεται από OCR πολλών Επεξεργαστών με τη γλώσσα και το τον προσανατολισμό του κειμένου. Στην περιοχή περιέχει επίσης κάθε γραμμής του word σε αυτήν την περιοχή με τη γραμμή κειμένου, θέση της γραμμής και κάθε πληροφορίες word (word περιεχόμενο, θέση και εμπιστοσύνης) σε αυτή τη γραμμή. Ακολουθεί ένα παράδειγμα και να τοποθετήσετε ορισμένα ενσωματωμένα σχόλια.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργεί μια εργασία με ένα αρχείο ρύθμισης παραμέτρων/προκαθορισμένα OCR.
1. Η λήψη των αρχείων JSON εξόδου. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

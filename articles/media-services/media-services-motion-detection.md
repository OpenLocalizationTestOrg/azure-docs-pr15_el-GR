<properties
    pageTitle="Εντοπισμός κινήσεις με αναλυτικά στοιχεία πολυμέσων Azure | Microsoft Azure"
    description="Ο επεξεργαστής πολυμέσων Azure Media κίνησης ανίχνευσης (πολλών Επεξεργαστών) σάς επιτρέπει να αποτελεσματικά αναγνώριση ενότητες που σας ενδιαφέρουν μέσα σε ένα βίντεο διαφορετικά μεγάλη και uneventful."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Εντοπισμός κινήσεις με αναλυτικά στοιχεία πολυμέσων Azure

##<a name="overview"></a>Επισκόπηση

Ο επεξεργαστής πολυμέσων **Azure Media κίνησης ανίχνευσης** (πολλών Επεξεργαστών) σάς επιτρέπει να αποτελεσματικά αναγνώριση ενότητες που σας ενδιαφέρουν μέσα σε ένα βίντεο διαφορετικά μεγάλη και uneventful. Εντοπισμός κίνησης μπορεί να χρησιμοποιηθεί σε στατικό κάμερας αποσπάσματος για τον προσδιορισμό ενότητες του βίντεο όπου παρουσιάζεται κίνησης. Δημιουργεί ένα αρχείο JSON που περιέχει ένα μετα-δεδομένων με χρονικές σημάνσεις και του πλαισίου περιοχής όπου παρουσιάστηκε το συμβάν.

Στοχεύουν σε τροφοδοσίες βίντεο ασφαλείας, αυτή η τεχνολογία είναι σε θέση να κατηγοριοποιήσετε κίνησης σε σχετικό συμβάντων και false θετικά όπως σκιές και αλλαγές φωτισμού. Αυτό σας επιτρέπει να δημιουργούν ειδοποιήσεων ασφαλείας από τροφοδοσίες κάμερας χωρίς την ανεπιθύμητη αλληλογραφία με απεριόριστες σημασία συμβάντα, κατά τη διαδικασία μπορείτε να εξαγάγετε λεπτά που σας ενδιαφέρουν από πολύ μεγάλο εποπτείας βίντεο.

Το **Πρόγραμμα ανίχνευσης κίνησης πολυμέσων Azure** πολλών Επεξεργαστών είναι αυτήν τη στιγμή στην προεπισκόπηση.

Αυτό το θέμα παρέχει λεπτομέρειες σχετικά με το **Πρόγραμμα ανίχνευσης κίνησης πολυμέσων Azure** και δείχνει πώς μπορείτε να το χρησιμοποιήσετε με το Media Services SDK για .NET


##<a name="motion-detector-input-files"></a>Αρχεία εισόδου ανίχνευσης κίνησης

Αρχεία βίντεο. Προς το παρόν, υποστηρίζονται οι παρακάτω μορφές: MP4, MOV και WMV.

##<a name="task-configuration-preset"></a>Ρύθμιση παραμέτρων εργασίας (προεπιλογή)

Κατά τη δημιουργία μιας εργασίας με **Ανίχνευσης κίνησης πολυμέσων Azure**, πρέπει να καθορίσετε μια προκαθορισμένη ρύθμιση παραμέτρων. 

###<a name="parameters"></a>Παράμετροι

Μπορείτε να χρησιμοποιήσετε τις ακόλουθες παραμέτρους:

Όνομα|Επιλογές|Περιγραφή|Προεπιλεγμένη
---|---|---|---
sensitivityLevel|Συμβολοσειρά: "Χαμηλή", "μεσαίας", "υψηλής"|Σύνολα αναφέρεται το επίπεδο διαβάθμισης στο ποιες κινήσεις. Προσαρμόστε αυτό για να ρυθμίσετε την τιμή false θετικά.|"μεσαίας"
frameSamplingValue|Θετικό ακέραιο αριθμό|Ορίζει τη συχνότητα στο οποίο εκτελείται αλγόριθμο. κάθε πλαίσιο ισούται με το 1, 2, σημαίνει ότι κάθε 2η πλαισίου, και ούτω καθεξής.|1
detectLightChange|Δυαδική: 'true', 'false'|Ορίζει εάν αναφέρονται light αλλαγές στα αποτελέσματα|'False'
mergeTimeThreshold|Xs-ώρα: Hh:mm:ss<br/>Παράδειγμα: 00:00:03|Καθορίζει του χρονικού διαστήματος μεταξύ κίνησης συμβάντα, όπου θα συνδυαστούν και αναφέρεται ως 1 2 συμβάντα.|00:00:00
detectionZones|Ένας πίνακας με ζώνες ανίχνευσης:<br/>-Εντοπισμός ζώνη είναι ένας πίνακας με 3 ή περισσότερα σημεία<br/>-Σημείο είναι ένα x και y συντεταγμένων από 0 έως 1.|Περιγράφει τη λίστα των πολυγωνικού εντοπισμού ζώνες που θα χρησιμοποιηθεί.<br/>Αποτελέσματα θα αναφερθούν με τις ζώνες ως Αναγνωριστικό, με την πρώτη 'id' που: 0|Μία ζώνη που καλύπτει ολόκληρο το πλαίσιο.

###<a name="json-example"></a>Παράδειγμα JSON

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Αρχεία εξόδου ανίχνευσης κίνησης

Μια εργασία εντοπισμού κίνησης που θα επιστρέψει ένα αρχείο JSON στο παγίου εξόδου που περιγράφει τις ειδοποιήσεις κίνησης και τις κατηγορίες, εντός του βίντεο. Το αρχείο θα περιέχει πληροφορίες σχετικά με το χρόνο και τη διάρκεια της κίνησης που εντοπίζονται στο βίντεο.

Το API ανίχνευσης κίνησης παρέχει δείκτες όταν υπάρχουν αντικείμενα με κίνηση σε σταθερή φόντο βίντεο (π.χ. μια εποπτείας βίντεο). Το πρόγραμμα ανίχνευσης κίνησης γίνει απομνημόνευση για να μειώσετε ειδοποιήσεις false, όπως φωτισμού και αλλαγές σκιά. Τρέχουσα περιορισμοί τους αλγορίθμους που περιλαμβάνουν νυχτερινής όρασης βίντεο, ημι-διαφανή αντικείμενα και μικρά αντικείμενα.

###<a id="output_elements"></a>Στοιχεία του αρχείου JSON εξόδου

>[AZURE.NOTE]Στην πιο πρόσφατη έκδοση, τη μορφή εξόδου JSON έχει αλλάξει και μπορεί να αντιπροσωπεύει μια αλλαγή πρόσφατες για ορισμένους πελάτες.

Ο παρακάτω πίνακας περιγράφει τα στοιχεία από το αρχείο εξόδου JSON.

Στοιχείο|Περιγραφή
---|---
Έκδοση|Αυτό αναφέρεται στην έκδοση του API βίντεο. Η τρέχουσα έκδοση είναι 2.
Κλίμακα χρόνου|"Υποδιαιρέσεις" ανά δευτερόλεπτο του βίντεο.
Μετατόπιση|Η ώρα μετατόπιση για χρονικές σημάνσεις στο "Υποδιαιρέσεις". Στην έκδοση 1.0 των API του βίντεο, αυτό θα είναι πάντα 0. Στο μέλλον υποστηρίζουμε σενάρια, αυτή η τιμή ενδέχεται να αλλάξουν.
Ταχύτητα καρέ|Καρέ το δευτερόλεπτο του βίντεο.
Πλάτος, ύψος|Αναφέρεται σε το πλάτος και το ύψος του βίντεο σε pixel.
Έναρξη|Η σήμανση χρόνου έναρξης στο "Υποδιαιρέσεις".
Διάρκεια|Το μήκος του συμβάντος, στο "Υποδιαιρέσεις".
Χρονικό διάστημα|Το χρονικό διάστημα από κάθε καταχώρηση στο συμβάν, στο "Υποδιαιρέσεις".
Συμβάντα|Κάθε τμήμα συμβάν περιέχει την κίνηση εντοπίζονται μέσα σε συγκεκριμένη χρονική διάρκεια.
Τύπος|Με την τρέχουσα έκδοση, αυτό είναι πάντα "2" για το γενικό κίνησης. Αυτό συμβαίνει ετικέτα βίντεο API την ευελιξία για να κατηγοριοποιήσετε κίνησης σε μελλοντικές εκδόσεις.
RegionID|Όπως περιγράφεται παραπάνω, αυτό θα είναι πάντα 0 σε αυτήν την έκδοση. Η ετικέτα παρέχει API βίντεο την ευελιξία να βρείτε κίνησης σε διάφορες περιοχές σε μελλοντικές εκδόσεις.
Περιοχές|Αναφέρεται στην περιοχή στο βίντεό σας όπου που σας ενδιαφέρουν κίνησης. <br/><br/>-αντιπροσωπεύει το "αναγνωριστικό" στην περιοχή περιοχή – σε αυτήν την έκδοση υπάρχει μόνο ένα, Αναγνωριστικό 0. <br/>-"Τύπος" αντιπροσωπεύει το σχήμα της περιοχής που σας ενδιαφέρουν για κίνησης. Προς το παρόν, υποστηρίζονται "ορθογώνιο" και "πολύγωνο".<br/> Εάν έχετε καθορίσει "ορθογώνιο", στην περιοχή έχει διαστάσεις στο X, Y, πλάτος και ύψος. Τις συντεταγμένες X και Y αντιπροσωπεύουν τις συντεταγμένες XY επάνω αριστερής της περιοχής σε μια κανονικοποιημένη κλίμακα του 0,0 να 1.0. Το πλάτος και ύψος αντιπροσωπεύει το μέγεθος της περιοχής σε μια κανονικοποιημένη κλίμακα του 0,0 να 1.0. Με την τρέχουσα έκδοση, X, Y, πλάτος και ύψος είναι πάντα σταθερά 0, 0 και 1, 1. <br/>Εάν έχετε καθορίσει "πολύγωνο", στην περιοχή έχει διαστάσεις σε στιγμές. <br/>
Τμήματα|Τα μετα-δεδομένα είναι κατατμημένη προς τα επάνω σε διαφορετικά τμήματα που ονομάζεται τμήματα. Κάθε τμήμα περιέχει μια έναρξης, διάρκεια, αριθμό διάστημα και συμβάντα. Ένα τμήμα με συμβάντα σημαίνει ότι δεν υπάρχει κίνησης εντοπίστηκε κατά τη συγκεκριμένη ώρα έναρξης και διάρκεια.
Αγκύλες]|Κάθε αγκύλη αντιπροσωπεύει ένα διάστημα στο συμβάν. Άδειασμα αγκύλες για συγκεκριμένο χρονικό διάστημα σημαίνει ότι δεν υπάρχει κίνησης εντοπίστηκε.
θέσεις|Αυτή η νέα καταχώρηση στην περιοχή συμβάντα παραθέτει στη θέση όπου παρουσιάστηκε την κίνηση. Αυτό είναι πιο συγκεκριμένες από τις ζώνες εντοπισμού.

Ακολουθεί ένα παράδειγμα JSON εξόδου

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Περιορισμοί

- Υποστηριζόμενες μορφές βίντεο εισαγωγής περιλαμβάνουν MP4, MOV και WMV.
- Εντοπισμός κίνησης είναι βελτιστοποιημένη για βίντεο στατικού φόντου. Ο αλγόριθμος εστιάζει στη μείωση ειδοποιήσεις false, όπως αλλαγές φωτισμού και σκιές.
- Ορισμένες κίνησης δεν μπορεί να ανιχνεύονται λόγω τεχνικών δυσκολιών; π.χ. νυχτερινής όρασης βίντεο, ημι-διαφανή αντικείμενα και μικρά αντικείμενα.


## <a name="sample-code"></a>Δείγμα κώδικα

Τα ακόλουθα πρόγραμμα παρουσιάζει πώς μπορείτε να:

1. Δημιουργία ενός περιουσιακού στοιχείου και να αποστείλετε ένα αρχείο πολυμέσων σε περιουσιακού στοιχείου.
1. Δημιουργεί μια εργασία με μια εργασία εντοπισμού κίνησης βίντεο που βασίζεται σε ένα αρχείο ρύθμισης παραμέτρων που περιέχει το παρακάτω υπόδειγμα json. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Η λήψη των αρχείων JSON εξόδου. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services κίνησης ανίχνευσης ιστολογίου](https://azure.microsoft.com/blog/motion-detector-update/)

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

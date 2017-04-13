<properties 
    pageTitle="Κωδικοποίηση ενός περιουσιακού στοιχείου με Media Encoder τυπική χρήση .NET | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET για να κωδικοποιήσετε ενός περιουσιακού στοιχείου με χρήση Strandard κωδικοποιητή πολυμέσων." 
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
    ms.date="09/19/2016"
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Κωδικοποίηση ενός περιουσιακού στοιχείου με Media Encoder τυπική χρήση .NET

Οι εργασίες κωδικοποίησης είναι μία από τις πιο κοινές λειτουργίες επεξεργασίας στις υπηρεσίες πολυμέσων. Μπορείτε να δημιουργήσετε κωδικοποίησης εργασίες για να μετατρέψετε τα αρχεία πολυμέσων από μία κωδικοποίηση σε κάποιον άλλο. Όταν κωδικοποιείτε, μπορείτε να χρησιμοποιήσετε το Media Services ενσωματωμένο Media Encoder. Μπορείτε επίσης να χρησιμοποιήσετε έναν κωδικοποιητή που παρέχεται από ένα συνεργάτη Media Services; κωδικοποιητές τρίτων είναι διαθέσιμες από το Azure Marketplace. 

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET για να κωδικοποιήσετε τους πόρους σας με Media Encoder τυπική (MES). Media Encoder τυπική ρυθμίζεται χρησιμοποιώντας ένα από τα encoder υποδείγματα περιγράφεται [εδώ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Συνιστάται να κωδικοποιείτε πάντα τα αρχεία σας θυγατρική σε μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 Ορισμός και, στη συνέχεια, να μετατρέψετε το σύνολο στην επιθυμητή μορφή χρησιμοποιώντας τη [Δυναμική συσκευασία](media-services-dynamic-packaging-overview.md). Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να λάβετε πρώτα τουλάχιστον μία On demand ροής μονάδας για τη ροή τελικό σημείο από το οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να κλίμακα Media Services](media-services-portal-manage-streaming-endpoints.md).

Εάν το αποτέλεσμα του περιουσιακού στοιχείου είναι κρυπτογραφημένα χώρου αποθήκευσης, πρέπει να ρυθμίσετε περιουσιακών στοιχείων παράδοσης πολιτικής. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων περιουσιακών στοιχείων παράδοσης πολιτικής](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MES παράγει ένα αρχείο εξόδου με όνομα που περιέχει οι πρώτοι 32 χαρακτήρες από το όνομα του αρχείου εισαγωγής. Το όνομα βασίζεται σε αυτόν που καθορίζεται στο προκαθορισμένο αρχείο. Για παράδειγμα, "όνομααρχείου": "_ {Basename} {Index} {επέκταση}". {Basename} αντικαθίσταται από οι πρώτοι 32 χαρακτήρες από το όνομα του αρχείου εισαγωγής.

###<a name="mes-formats"></a>Μορφές MES

[Μορφές και τους κωδικοποιητές](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>Υποδείγματα MES

Media Encoder τυπική ρυθμίζεται χρησιμοποιώντας ένα από τα encoder υποδείγματα περιγράφεται [εδώ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Μετα-δεδομένα εισόδου και εξόδου

Όταν κωδικοποιήσετε ενός περιουσιακού στοιχείου εισαγωγής (ή παγίων) χρησιμοποιώντας MES, θα λάβετε ενός περιουσιακού στοιχείου εξόδου στο την επιτυχή ολοκλήρωση που κωδικοποιείτε εργασίας. Παγίου εξόδου περιέχει βίντεο, ήχο, μικρογραφίες, δηλωτικό, κ.λπ., με βάση τα προκαθορισμένα κωδικοποίησης που χρησιμοποιείτε.

Παγίου εξόδου περιέχει επίσης ένα αρχείο με μετα-δεδομένα σχετικά με το εισαγωγής περιουσιακών στοιχείων. Το όνομα του αρχείου XML μετα-δεδομένων έχει την εξής μορφή: < asset_id > _metadata.xml (για παράδειγμα, 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), όπου < asset_id > είναι η τιμή AssetId του παγίου εισαγωγής. Το σχήμα αυτό εισαγωγής μετα-δεδομένων XML περιγράφεται [εδώ](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Παγίου εξόδου περιέχει επίσης ένα αρχείο με μετα-δεδομένα σχετικά με το αποτέλεσμα του περιουσιακού στοιχείου. Το όνομα του αρχείου XML μετα-δεδομένων έχει την εξής μορφή: < source_file_name > _manifest.xml (για παράδειγμα, BigBuckBunny_manifest.xml). Το σχήμα αυτό μετα-δεδομένων εξόδου που περιγράφεται XML [εδώ](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Εάν θέλετε να εξετάσετε ένα από τα δύο αρχεία μετα-δεδομένων, μπορείτε να δημιουργήσετε ένα προσδιοριστικό συσχετισμών Ασφαλείας και να κάνετε λήψη του αρχείου στον τοπικό σας υπολογιστή. Μπορείτε να βρείτε ένα παράδειγμα σχετικά με τον τρόπο για να δημιουργήσετε ένα προσδιοριστικό συσχετισμών Ασφαλείας και να κάνετε λήψη ενός αρχείου χρησιμοποιώντας τις επεκτάσεις SDK .NET υπηρεσίες πολυμέσων.

##<a name="download-sample"></a>Λήψη δείγματος

Λήψη και να εκτελέσετε ένα δείγμα από [εδώ](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Παράδειγμα

Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί SDK .NET υπηρεσίες πολυμέσων για να εκτελέσετε τις ακόλουθες εργασίες:

- Δημιουργήστε μια εργασία κωδικοποίησης.
- Λάβετε μια αναφορά σε τυπική Media Encoder κωδικοποιητή.
- Καθορίστε για να χρησιμοποιήσετε το "H264 πολλών ρυθμό μετάδοσης bit 720p" προκαθορισμένο. Μπορείτε να δείτε όλες τις προκαθορισμένες ρυθμίσεις [εδώ](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Μπορείτε επίσης να εξετάσετε το σχήμα στο οποίο πρέπει να συμμορφώνονται αυτές τις προκαθορισμένες ρυθμίσεις [δείτε](https://msdn.microsoft.com/library/mt269962.aspx) το θέμα.
- Προσθέστε μία κωδικοποίησης εργασία στο έργο. 
- Καθορίστε το εισαγωγής περιουσιακού στοιχείου για κωδικοποίηση.
- Δημιουργία ενός περιουσιακού στοιχείου εξόδου που θα περιέχουν τα κωδικοποιημένα περιουσιακών στοιχείων.
- Προσθήκη ενός προγράμματος χειρισμού συμβάντων για να ελέγξετε την πρόοδο του έργου.
- Υποβάλετε την εργασία.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης 

[Πώς να δημιουργείτε μικρογραφία χρήση Media Encoder τυπική με .NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Επισκόπηση κωδικοποίηση υπηρεσίες πολυμέσων](media-services-encode-asset.md)

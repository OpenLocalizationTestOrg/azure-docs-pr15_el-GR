<properties 
    pageTitle="Χρήση συσκευασία πολυμέσων Azure για την ολοκλήρωση εργασιών στατική συσκευασία | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει διάφορες εργασίες που έχουν πραγματοποιηθεί με συσκευασία πολυμέσων Azure." 
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
    ms.author="juliako"/>


# <a name="using-azure-media-packager-to-accomplish-static-packaging-tasks"></a>Χρήση συσκευασία πολυμέσων Azure για την ολοκλήρωση εργασιών στατική συσκευασία

>[AZURE.NOTE]Στο τέλος της ζωής ημερομηνίας για το Microsoft Azure Media συσκευασία και μονάδα κρυπτογράφησης του Microsoft Azure Media έχει επεκταθεί σε 1 Μαρτίου 2017. Πριν από την ημερομηνία, η λειτουργίες από αυτούς τους επεξεργαστές θα προστεθεί στο Media Encoder τυπική (MES). Οι πελάτες θα παρέχονται με οδηγίες σχετικά με τον τρόπο για να μετεγκαταστήσετε τις ροές εργασίας να στέλνουν εργασίες σε MES. Δυνατότητες μετατροπής και κρυπτογράφησης μορφή μπορεί να είναι επίσης διαθέσιμα μέσω δυναμικής συσκευασία και δυναμική κρυπτογράφηση.

## <a name="overview"></a>Επισκόπηση

Προκειμένου να κάνουν ψηφιακού βίντεο μέσω του internet θα πρέπει να γίνεται η συμπίεση των πολυμέσων. Αρχεία ψηφιακού βίντεο είναι αρκετά μεγάλα και μπορεί να είναι πολύ μεγάλο για παράδοση μέσω του internet ή για συσκευές τους πελάτες σας για να εμφανίζονται σωστά. Κωδικοποίηση είναι η διαδικασία για τη συμπίεση βίντεο και ήχος, ώστε οι πελάτες σας μπορούν να προβάλουν τα πολυμέσα. Όταν έχει κωδικοποιηθεί βίντεο μπορούν να τεθούν σε διαφορετικό αρχείο κοντέινερ. Η διαδικασία τοποθετώντας κωδικοποιημένο πολυμέσων σε ένα κοντέινερ ονομάζεται συσκευασία. Για παράδειγμα, μπορείτε να μεταφέρετε ένα αρχείο MP4 και να μετατρέψετε σε ομαλή ροή ή HLS περιεχόμενο χρησιμοποιώντας τη συσκευασία πολυμέσων Azure. 

Υπηρεσίες πολυμέσων υποστηρίζει δυναμικές και στατικές συσκευασία. Κατά τη χρήση στατικής συσκευασία πρέπει να δημιουργήσετε ένα αντίγραφο του περιεχομένου σας σε κάθε μορφή που απαιτείται από τους πελάτες σας. Με δυναμική συσκευασία μόνο που χρειάζεστε είναι για τη δημιουργία ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4 ή ομαλή ροή αρχείων. Στη συνέχεια, ανάλογα με την καθορισμένη μορφή στην αίτηση δηλωτικό ή τμήματος, η ροή On απαιτήσεων διακομιστή εξασφαλίζει ότι οι χρήστες σας θα λάβουν τη ροή στο πρωτόκολλο που έχετε επιλέξει. Ως αποτέλεσμα, θα πρέπει να αποθηκεύετε και να πληρώσετε για τα αρχεία σε μορφή μία χώρου αποθήκευσης και υπηρεσία Media Services θα δημιουργήσετε και θα λειτουργήσει την κατάλληλη απάντηση που βασίζεται σε αιτήσεις από ένα πρόγραμμα-πελάτη.

>[AZURE.NOTE]Συνιστάται να χρησιμοποιήσετε [δυναμικές συσκευασία](media-services-dynamic-packaging-overview.md).

Ωστόσο, υπάρχουν ορισμένα σενάρια που απαιτούν στατική συσκευασία: 

- Επικύρωση προσαρμόσιμη ρυθμό μετάδοσης bit MP4s κωδικοποιηθεί με εξωτερική κωδικοποιητές (για παράδειγμα, χρήση άλλου κατασκευαστή κωδικοποιητές).

Μπορείτε επίσης να χρησιμοποιήσετε στατική συσκευασία για να εκτελέσετε τις ακόλουθες εργασίες. Ωστόσο, συνιστάται να χρησιμοποιήσετε δυναμική κρυπτογράφηση.

- Χρήση στατικής κρυπτογράφησης για την προστασία των ομαλές και MPEG ΠΑΎΛΑΣ με PlayReady
- Χρήση στατικής κρυπτογράφησης για την προστασία HLSv3 με AES 128
- Χρήση στατικής κρυπτογράφησης για την προστασία HLSv3 με PlayReady


## <a name="validating-adaptive-bitrate-mp4s-encoded-with-external-encoders"></a>Κατά την επαλήθευση προσαρμόσιμη MP4s ρυθμό μετάδοσης bit κωδικοποιηθεί με εξωτερική κωδικοποιητές

Εάν θέλετε να χρησιμοποιήσετε ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit (πολλούς-ρυθμό μετάδοσης bit) MP4 αρχείων που δεν έχουν κωδικοποιηθεί με κωδικοποιητές Media Services, πρέπει να επαληθεύσετε τα αρχεία σας πριν να γίνει περαιτέρω επεξεργασία. Η συσκευασία υπηρεσίες πολυμέσων μπορούν να επικύρωση ενός περιουσιακού στοιχείου που περιέχει ένα σύνολο αρχείων MP4 και ελέγξτε εάν μπορούν να συσκευαστούν παγίου ομαλή ροή ή HLS. Εάν η επικύρωση αποτύχει, θα ολοκληρώσει την εργασία που επεξεργασία της εργασίας με το μήνυμα σφάλματος. Αρχείο XML που ορίζει το υπόδειγμα για την εργασία επικύρωσης μπορείτε να βρείτε στο θέμα [Προκαθορισμένες εργασίας για συσκευασία πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973635.aspx) .

>[AZURE.NOTE]Χρησιμοποιήστε το Media Encoder τυπική για την παραγωγή ή τη συσκευασία υπηρεσίες πολυμέσων για να επικυρώσει το περιεχόμενό σας για να αποφύγετε προβλήματα κατά το χρόνο εκτέλεσης. Εάν ο διακομιστής ροής On ζήτηση δεν είναι δυνατό να ανάλυση αρχεία προέλευσης κατά το χρόνο εκτέλεσης, θα λάβετε σφάλμα HTTP 1.1 "415 τύπο που δεν υποστηρίζεται πολυμέσων". Επανειλημμένα προκαλεί ο διακομιστής να αποτύχει η ανάλυση αρχεία προέλευσης επηρεάζει την απόδοση του διακομιστή ροής On ζήτηση και ενδέχεται να μειώσετε το εύρος ζώνης που είναι διαθέσιμες σε λειτουργία άλλες αιτήσεις. Azure Media Services προσφέρει μια υπηρεσία σύμβαση (SLA) σχετικά με τις υπηρεσίες On Demand ροή; Ωστόσο, αυτή SLA δεν ισχύουν οι επιλογές εάν ο διακομιστής είναι misused με τον τρόπο που περιγράφεται παραπάνω.

Αυτή η ενότητα δείχνει πώς μπορείτε να επεξεργαστείτε την εργασία επικύρωσης. Εμφανίζει επίσης πώς μπορείτε να δείτε την κατάσταση και το μήνυμα σφάλματος της εργασίας που συμπληρώνει με JobStatus.Error.

Για να επικυρώσετε MP4 αρχείων με συσκευασία υπηρεσίες πολυμέσων, πρέπει να δημιουργήσετε το δικό σας αρχείο δηλωτικό (.ism) και στείλτε το μαζί με τα αρχεία προέλευσης στο λογαριασμό Media Services. Κάτω από ένα δείγμα του αρχείου .ism παράγεται από την τυπική κωδικοποιητή πολυμέσων. Τα ονόματα αρχείων είναι διάκριση πεζών-κεφαλαίων. Επίσης, βεβαιωθείτε ότι το κείμενο στο αρχείο .ism κωδικοποιείται με UTF-8.

    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
    <!-- Tells the server that these input files are MP4s – specific to Dynamic Packaging -->
        <meta name="formats" content="mp4" /> 
      </head>
      <body>
        <switch>
          <video src="BigBuckBunny_1000.mp4" />
          <video src="BigBuckBunny_1500.mp4" />
          <video src="BigBuckBunny_2250.mp4" />
          <video src="BigBuckBunny_3400.mp4" />
          <video src="BigBuckBunny_400.mp4" />
          <video src="BigBuckBunny_650.mp4" />
          <audio src="BigBuckBunny_400.mp4" />
        </switch>
      </body>
    </smil>

Όταν έχετε ολοκληρώσει την προσαρμόσιμη ρυθμό μετάδοσης bit MP4 που μπορείτε να επωφεληθείτε από δυναμικής συσκευασία. Δυναμική συσκευασία σάς επιτρέπει να κάνουν ροών στο καθορισμένο πρωτόκολλο χωρίς περαιτέρω συσκευασία. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [δυναμικής συσκευασία](media-services-dynamic-packaging-overview.md).

Το παρακάτω δείγμα κώδικα χρησιμοποιεί Azure Media Services .NET SDK επεκτάσεις.  Φροντίστε να ενημερώνετε τον κωδικό ώστε να οδηγούν στο φάκελο όπου βρίσκονται σας αρχεία MP4 εισόδου και το αρχείο .ism. Και επίσης για να όπου βρίσκεται το αρχείο MediaPackager_ValidateTask.xml. Αυτό το αρχείο XML έχει οριστεί στο θέμα [Προκαθορισμένες εργασίας για συσκευασία πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973635.aspx) .
    
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Xml.Linq;
    
    namespace MediaServicesStaticPackaging
    {
        class Program
        {
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            // The MultibitrateMP4Files folder should also
            // contain the .ism manifest file.
            private static readonly string _multibitrateMP4s =
                Path.Combine(_mediaFiles, @"MultibitrateMP4Files");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations";
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServicesAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Ingest a set of multibitrate MP4s.
                //
                // Use the SDK extension method to create a new asset by 
                // uploading files from a local directory.
                IAsset multibitrateMP4sAsset = _context.Assets.CreateFromFolder(
                    _multibitrateMP4s,
                    AssetCreationOptions.None,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
                // Use Azure Media Packager to validate the files.
                IAsset validatedMP4s =
                    ValidateMultibitrateMP4s(multibitrateMP4sAsset);
    
                // Publish the asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    validatedMP4s,
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                                     // Get the streaming URLs.
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(validatedMP4s.GetSmoothStreamingUri().ToString());
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(validatedMP4s.GetMpegDashUri().ToString());
                Console.WriteLine("HLS URL:");
                Console.WriteLine(validatedMP4s.GetHlsUri().ToString());
            }
    
            public static IAsset ValidateMultibitrateMP4s(IAsset multibitrateMP4sAsset)
            {
                // Set .ism as a primary file 
                // in a multibitrate MP4 set.
                SetISMFileAsPrimary(multibitrateMP4sAsset);
    
                // Create a new job.
                IJob job = _context.Jobs.Create("MP4 validation and converstion to Smooth Stream job.");
    
                // Read the task configuration data into a string. 
                string configMp4Validation = File.ReadAllText(Path.Combine(
                        _configurationXMLFiles,
                        "MediaPackager_ValidateTask.xml"));
    
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Create a task with the conversion details, using the configuration data. 
                ITask task = job.Tasks.AddNew("Mp4 Validation Task",
                    processor,
                    configMp4Validation,
                    TaskOptions.None);
    
                // Specify the input asset to be validated.
                task.InputAssets.Add(multibitrateMP4sAsset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted). 
                task.OutputAssets.AddNew("Validated output asset",
                        AssetCreationOptions.None);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
              
                // If the validation task fails and job completes with JobState.Error,
                // display the error message and throw an exception.
                if (job.State == JobState.Error)
                {
                    Console.WriteLine("  Job ID: " + job.Id);
                    Console.WriteLine("  Name: " + job.Name);
                    Console.WriteLine("  State: " + job.State);
    
                    foreach (var jobTask in job.Tasks)
                    {
                        Console.WriteLine("  Task Id: " + jobTask.Id);
                        Console.WriteLine("  Name: " + jobTask.Name);
                        Console.WriteLine("  Progress: " + jobTask.Progress);
                        Console.WriteLine("  Configuration: " + jobTask.Configuration);
                        Console.WriteLine("  Running time: " + jobTask.RunningDuration);
                        if (jobTask.ErrorDetails != null)
                        {
                            foreach (var errordetail in jobTask.ErrorDetails)
                            {
    
                                Console.WriteLine("  Error Message:" + errordetail.Message);
                                Console.WriteLine("  Error Code:" + errordetail.Code);
                            }
                        }
                    }
                    throw new Exception("The specified multi-bitrate MP4 set is not valid.");
                }
    
    
                return job.OutputMediaAssets[0];
            }
    
            static void SetISMFileAsPrimary(IAsset asset)
            {
                var ismAssetFiles = asset.AssetFiles.ToList().
                    Where(f => f.Name.EndsWith(".ism", StringComparison.OrdinalIgnoreCase)).ToArray();
    
                // The following code assigns the first .ism file as the primary file in the asset.
                // An asset should have one .ism file.  
                ismAssetFiles.First().IsPrimary = true;
                ismAssetFiles.First().Update();
            }
        }
    }

## <a name="using-static-encryption-to-protect-your-smooth-and-mpeg-dash-with-playready"></a>Χρήση στατικής κρυπτογράφησης για την προστασία σας ομαλές και MPEG ΠΑΎΛΑΣ με PlayReady

Εάν θέλετε να προστατεύσετε το περιεχόμενο με PlayReady, έχετε τη δυνατότητα επιλογής χρησιμοποιώντας [δυναμική κρυπτογράφηση](media-services-protect-with-drm.md) (η επιλογή προτεινόμενο) ή στατικό κρυπτογράφησης (όπως περιγράφεται σε αυτήν την ενότητα).

Το παράδειγμα σε αυτήν την ενότητα κωδικοποιεί θυγατρική αρχείου (σε αυτό MP4 πεζών-κεφαλαίων) στο προσαρμόσιμη ρυθμό μετάδοσης bit MP4 αρχεία. Στη συνέχεια, πακέτων MP4s σε ομαλή ροή και, στη συνέχεια, κρυπτογραφεί ομαλή ροή με PlayReady. Ως αποτέλεσμα, θα μπορείτε να ροή ομαλή ροή ή MPEG ΠΑΎΛΑΣ.

Υπηρεσίες πολυμέσων παρέχει τώρα μια υπηρεσία για την παροχή του Microsoft PlayReady άδειες χρήσης. Το παράδειγμα σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να ρυθμίσετε την υπηρεσία παράδοσης άδειας χρήσης PlayReady υπηρεσίες πολυμέσων (δείτε τη μέθοδο ConfigureLicenseDeliveryService που καθορίζονται από τον παρακάτω κώδικα). Για περισσότερες πληροφορίες σχετικά με την υπηρεσία παράδοσης PlayReady υπηρεσίες πολυμέσων άδειας χρήσης, ανατρέξτε στο θέμα [Χρήση δυναμική κρυπτογράφηση PlayReady και η υπηρεσία παράδοσης άδεια χρήσης](media-services-protect-with-drm.md).

>[AZURE.NOTE]Για να προβάλετε ΠΑΎΛΑΣ MPEG κρυπτογραφημένα με PlayReady, βεβαιωθείτε ότι για να χρησιμοποιήσετε επιλογές CENC, ορίζοντας τις ιδιότητες useSencBox και adjustSubSamples (που περιγράφεται στο θέμα [Εργασία προκαθορισμένα για μονάδα κρυπτογράφησης πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973610.aspx) ) στην τιμή true.  


Βεβαιωθείτε ότι για να ενημερώσετε τον παρακάτω κώδικα ώστε να οδηγούν στο φάκελο όπου βρίσκεται το αρχείο εισαγωγής MP4.

Και επίσης για να όπου βρίσκονται τα αρχεία σας MediaPackager_MP4ToSmooth.xml και MediaEncryptor_PlayReadyProtection.xml. MediaPackager_MP4ToSmooth.xml έχει οριστεί στο [Εργασίας προκαθορισμένα για συσκευασία πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973635.aspx) και MediaEncryptor_PlayReadyProtection.xml ορίζεται στο θέμα [Εργασία προκαθορισμένα για μονάδα κρυπτογράφησης πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973610.aspx) . 

Το παράδειγμα ορίζει τη μέθοδο UpdatePlayReadyConfigurationXMLFile που μπορείτε να χρησιμοποιήσετε για να ενημερώσετε δυναμικά το αρχείο MediaEncryptor_PlayReadyProtection.xml. Εάν έχετε διαθέσιμες κλειδιού σπόρο, μπορείτε να χρησιμοποιήσετε τη μέθοδο CommonEncryption.GeneratePlayReadyContentKey για να δημιουργήσετε το κλειδί περιεχομένου με βάση το keySeedValue και το αναγνωριστικό κλειδιού τιμές.

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace PlayReadyStaticEncryptAndKeyDeliverySvc
    {
        class Program
        {
           
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Encoding and encrypting assets //////////////////////
                // Load a single MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset =
                    ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;
    
                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);
    
                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);
    
                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
    
                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);
    
    
                // Encrypt your clear Smooth Streaming to Smooth Streaming with PlayReady.
                IAsset outputAsset = CreateSmoothStreamEncryptedWithPlayReady(clearSmoothStreamAsset);
    
    
                // You can use the http://smf.cloudapp.net/healthmonitor player 
                // to test the smoothStreamURL URL.
                string smoothStreamURL = outputAsset.GetSmoothStreamingUri().ToString();
                Console.WriteLine("Smooth Streaming URL:");
                Console.WriteLine(smoothStreamURL);
    
                // You can use the http://dashif.org/reference/players/javascript/ player 
                // to test the dashURL URL.
                string dashURL = outputAsset.GetMpegDashUri().ToString();
                Console.WriteLine("MPEG DASH URL:");
                Console.WriteLine(dashURL);
            }
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Stream.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains the Smooth Streaming asset.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Encrypts Smooth Stream with PlayReady.
            /// Then creates a Smooth Streaming Url.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateSmoothStreamEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                // Create a job.
                IJob job = _context.Jobs.Create("Encrypt to PlayReady Smooth Streaming.");
    
                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[0];
            }
    
            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
                return key;
            }
    
            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");
    
                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";
    
                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);
    
                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();
    
                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());
    
                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));
    
                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);
    
                doc.Save(xmlFileName);
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
                return asset;
            }
    
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Stream",
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
    
            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: To deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true. 
            /// In this example, MediaEncryptor_PlayReadyProtection.xml contains configuration.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);
    
                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));
    
                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);
    
                playreadyTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);
    
                return playreadyAsset;
            }
    
            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };
    
                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);
    
                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;
    
    
                contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }
    
            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.
    
                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                responseTemplate.LicenseTemplates.Add(licenseTemplate);
    
                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
    
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];
    
                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }
    
                return returnValue;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-aes-128"></a>Χρήση στατικής κρυπτογράφησης για την προστασία HLSv3 με AES 128

Εάν θέλετε να κρυπτογραφήσετε το HLS με AES 128, έχετε τη δυνατότητα επιλογής χρησιμοποιώντας δυναμική κρυπτογράφηση (η επιλογή προτεινόμενο) ή στατικό κρυπτογράφησης (όπως φαίνεται σε αυτήν την ενότητα). Εάν αποφασίσετε να χρησιμοποιήσετε δυναμική κρυπτογράφηση, ανατρέξτε στο θέμα [Χρήση δυναμική κρυπτογράφηση AES 128 και η υπηρεσία παράδοσης αριθμού-κλειδιού](media-services-protect-with-aes128.md).

>[AZURE.NOTE]Για να μετατρέψετε το περιεχόμενό σας σε HLS, πρέπει να πρώτα μετατροπή/κωδικοποιήσετε το περιεχόμενό σας σε ομαλή ροή.
>Επίσης, για το HLS για να λάβετε κρυπτογραφημένα με AES, βεβαιωθείτε ότι για να ορίσετε τις ακόλουθες ιδιότητες στο αρχείο σας MediaPackager_SmoothToHLS.xml: Ορίστε την ιδιότητα κρυπτογράφηση στην τιμή true, ορίστε την τιμή του κλειδιού και την τιμή keyuri ώστε να οδηγούν στο διακομιστή authentication\authorization.
Υπηρεσίες πολυμέσων θα δημιουργήσετε ένα αρχείο κλειδιού και τοποθετήστε το στο κοντέινερ περιουσιακών στοιχείων. Θα πρέπει να αντιγράψετε το /asset-containerguid /*.key αρχείων στο διακομιστή σας (ή δημιουργήστε το δικό σας αρχείο κλειδιού) και, στη συνέχεια, διαγράψτε το *αρχείου .key από το κοντέινερ περιουσιακών στοιχείων.

Το παράδειγμα σε αυτήν την ενότητα κωδικοποιεί θυγατρική αρχείου (σε αυτήν την περίπτωση MP4) στο multibitrate MP4 αρχείων και, στη συνέχεια, πακέτων MP4s σε ομαλή ροή. Το, στη συνέχεια, πακέτων ομαλή ροή σε HTTP Live ροής (HLS) κρυπτογραφημένα με κρυπτογράφηση 128 bit ροής για προχωρημένους πρότυπο κρυπτογράφησης (AES). Βεβαιωθείτε ότι για να ενημερώσετε τον παρακάτω κώδικα ώστε να οδηγούν στο φάκελο όπου βρίσκεται το αρχείο εισαγωγής MP4. Και επίσης για να όπου βρίσκονται τα αρχεία σας MediaPackager_MP4ToSmooth.xml και MediaPackager_SmoothToHLS.xml ρύθμισης παραμέτρων. Μπορείτε να βρείτε τον ορισμό για αυτά τα αρχεία στο θέμα [Εργασία προκαθορισμένα για συσκευασία πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973635.aspx) .
    
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    
    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.
    
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
            
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName = 
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey = 
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName, 
                                _mediaServicesAccountKey);
                // Use the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Encoding and encrypting assets //////////////////////
    
                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create HLS encrypted with AES.
                IAsset HLSEncryptedWithAESAsset = CreateHLSEncryptedWithAES(clearSmoothStreamAsset);
    
                // You can use the following player to test the HLS with AES stream.
                // http://apps.microsoft.com/windows/app/3ivx-hls-player/f79ce7d0-2993-4658-bc4e-83dc182a0614 
                string hlsWithAESURL = HLSEncryptedWithAESAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with AES URL:");
                Console.WriteLine(hlsWithAESURL);
            }
    
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Encrypts an HLS with AES-128.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithAES(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with AES.");
    
                // Add task 1 - Package clear Smooth Streaming to HLS with AES.
                PackageSmoothStreamToHLS(job, clearSmoothStreamAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // The OutputMediaAssets[0] contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[0],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[0];
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
     
                return asset;
            }
    
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s", 
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles, 
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset = 
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming", 
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
            /// <summary>
            /// Converts Smooth Streaming to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Streaming asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Read the configuration data into a string. 
                // For the HLS to get encrypted with AES make sure to set the
                // encrypt configuration property to true.
                //
                // In production, it is recommended to do the following:
                //    Set a Key url for your authn/authz server.
                //    Copy the /asset-containerguid/*.key file to your server (or craft a key file for yourself).
                //    Delete *.key from the asset container.
                //
                string configuration = File.ReadAllText(Path.Combine(_configurationXMLFiles, @"MediaPackager_SmoothToHLS.xml"));
    
                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth Streaming to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);
    
                // Add an output asset to contain the results of the job. 
                IAsset outputAsset = 
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);
    
    
                return outputAsset;
            }
        }
    }

## <a name="using-static-encryption-to-protect-hlsv3-with-playready"></a>Χρήση στατικής κρυπτογράφησης για την προστασία HLSv3 με PlayReady

Εάν θέλετε να προστατεύσετε το περιεχόμενο με PlayReady, έχετε τη δυνατότητα επιλογής χρησιμοποιώντας [δυναμική κρυπτογράφηση](media-services-protect-with-drm.md) (η επιλογή προτεινόμενο) ή στατικό κρυπτογράφησης (όπως περιγράφεται σε αυτήν την ενότητα).

>[AZURE.NOTE] Για να προστατεύσετε το περιεχόμενό σας με χρήση PlayReady πρέπει να πρώτα μετατροπή/κωδικοποιήσετε το περιεχόμενό σας σε μια μορφή ομαλή ροή.

Το παράδειγμα σε αυτήν την ενότητα κωδικοποιεί θυγατρική αρχείου (σε αυτό MP4 πεζών-κεφαλαίων) στο multibitrate MP4 αρχεία. Το, στη συνέχεια, πακέτων MP4s σε ομαλή ροή και κρυπτογραφεί ομαλή ροή με PlayReady. Για να παραγάγετε HTTP Live ροής (HLS) κρυπτογραφημένα με PlayReady, παγίου ομαλή ροή PlayReady πρέπει να συσκευαστούν σε HLS. Αυτό το θέμα παρουσιάζει πώς μπορείτε να εκτελέσετε όλα αυτά τα βήματα.

Υπηρεσίες πολυμέσων παρέχει τώρα μια υπηρεσία για την παροχή του Microsoft PlayReady άδειες χρήσης. Το παράδειγμα σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να ρυθμίσετε την υπηρεσία παράδοσης άδειας χρήσης PlayReady υπηρεσίες πολυμέσων (δείτε τη μέθοδο **ConfigureLicenseDeliveryService** που καθορίζονται από τον παρακάτω κώδικα). 

Βεβαιωθείτε ότι για να ενημερώσετε τον παρακάτω κώδικα ώστε να οδηγούν στο φάκελο όπου βρίσκεται το αρχείο εισαγωγής MP4. Και επίσης για να όπου βρίσκονται τα αρχεία σας MediaPackager_MP4ToSmooth.xml, MediaPackager_SmoothToHLS.xml και MediaEncryptor_PlayReadyProtection.xml. MediaPackager_MP4ToSmooth.xml και MediaPackager_SmoothToHLS.xml ορίζονται σε [Εργασία προκαθορισμένα για συσκευασία πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973635.aspx) και MediaEncryptor_PlayReadyProtection.xml ορίζεται στο θέμα [Εργασία προκαθορισμένα για μονάδα κρυπτογράφησης πολυμέσων Azure](http://msdn.microsoft.com/library/azure/hh973610.aspx) .
    
    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Xml.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace MediaServicesContentProtection
    {
        class Program
        {
            // Paths to support files (within the above base path). You can use 
            // the provided sample media files from the "SupportFiles" folder, or 
            // provide paths to your own media files below to run these samples.
    
            private static readonly string _mediaFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _singleMP4File =
                Path.Combine(_mediaFiles, @"SingleMP4\BigBuckBunny.mp4");
    
            // XML Configruation files path.
            private static readonly string _configurationXMLFiles = @"../..\Configurations\";
    
    
            private static MediaServicesCredentials _cachedCredentials = null;
            private static CloudMediaContext _context = null;
    
            // Media Services account information.
            private static readonly string _mediaServicesAccountName =
                ConfigurationManager.AppSettings["MediaServiceAccountName"];
            private static readonly string _mediaServicesAccountKey =
                ConfigurationManager.AppSettings["MediaServiceAccountKey"];
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                // Load an MP4 file.
                IAsset asset = IngestSingleMP4File(_singleMP4File, AssetCreationOptions.None);
    
                // Encode an MP4 file to a set of multibitrate MP4s.
                // Then, package a set of MP4s to clear Smooth Streaming.
                IAsset clearSmoothStreamAsset = ConvertMP4ToMultibitrateMP4sToSmoothStreaming(asset);
    
                // Create a common encryption content key that is used 
                // a) to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
                //    that is used for encryption.
                // b) to configure the license delivery service and 
                //
                Guid keyId;
                byte[] contentKey;
    
                IContentKey key = CreateCommonEncryptionKey(out keyId, out contentKey);
    
                // The content key authorization policy must be configured by you 
                // and met by the client in order for the PlayReady license
                // to be delivered to the client. 
                // In this example the Media Services PlayReady license delivery service is used.
                ConfigureLicenseDeliveryService(key);
    
                // Get the Media Services PlayReady license delivery URL.
                // This URL will be assigned to the licenseAcquisitionUrl property 
                // of the MediaEncryptor_PlayReadyProtection.xml file.
                Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
    
                // Update the MediaEncryptor_PlayReadyProtection.xml file with the key and URL info.
                UpdatePlayReadyConfigurationXMLFile(keyId, contentKey, acquisitionUrl);
    
                // Create HLS encrypted with PlayReady.
                IAsset playReadyHLSAsset = CreateHLSEncryptedWithPlayReady(clearSmoothStreamAsset);
                //
                string hlsWithPlayReadyURL = playReadyHLSAsset.GetHlsUri().ToString();
                Console.WriteLine("HLS with PlayReady URL:");
                Console.WriteLine(hlsWithPlayReadyURL);
            }
    
            /// <summary>
            /// Creates a job with 2 tasks: 
            /// 1 task - encodes a single MP4 to multibitrate MP4s,
            /// 2 task - packages MP4s to Smooth Streaming.
            /// </summary>
            /// <returns>The output asset.</returns>
            public static IAsset ConvertMP4ToMultibitrateMP4sToSmoothStreaming(IAsset asset)
            {
                // Create a new job.
                IJob job = _context.Jobs.Create("Convert MP4 to Smooth Streaming.");
    
                // Add task 1 - Encode single MP4 into multibitrate MP4s.
                IAsset MP4sAsset = EncodeSingleMP4IntoMultibitrateMP4sTask(job, asset);
                // Add task 2 - Package a multibitrate MP4 set to Clear Smooth Streaming.
                IAsset packagedAsset = PackageMP4ToSmoothStreamingTask(job, MP4sAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Get the output asset that contains Smooth Streaming.
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Create a common encryption content key that is used 
            /// to set the key values in the MediaEncryptor_PlayReadyProtection.xml file
            /// that is used for encryption.
            /// </summary>
            /// <param name="keyId"></param>
            /// <param name="contentKey"></param>
            /// <returns></returns>
            public static IContentKey CreateCommonEncryptionKey(out Guid keyId, out byte[] contentKey)
            {
                keyId = Guid.NewGuid();
                contentKey = GetRandomBuffer(16);
    
                IContentKey key = _context.ContentKeys.Create(
                                        keyId,
                                        contentKey,
                                        "ContentKey",
                                        ContentKeyType.CommonEncryption);
    
                return key;
            }
    
            /// <summary>
            /// Update your configuration .xml file dynamically.
            /// </summary>
            public static void UpdatePlayReadyConfigurationXMLFile(Guid keyId, byte[] keyValue, Uri licenseAcquisitionUrl)
            {
                string xmlFileName = Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml");
    
                XNamespace xmlns = "http://schemas.microsoft.com/iis/media/v4/TM/TaskDefinition#";
    
                // Prepare the encryption task template
                XDocument doc = XDocument.Load(xmlFileName);
    
                var licenseAcquisitionUrlEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "licenseAcquisitionUrl")
                        .FirstOrDefault();
                var contentKeyEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "contentKey")
                        .FirstOrDefault();
                var keyIdEl = doc
                        .Descendants(xmlns + "property")
                        .Where(p => p.Attribute("name").Value == "keyId")
                        .FirstOrDefault();
    
                // Update the "value" property.
                if (licenseAcquisitionUrlEl != null)
                    licenseAcquisitionUrlEl.Attribute("value").SetValue(licenseAcquisitionUrl.ToString());
    
                if (contentKeyEl != null)
                    contentKeyEl.Attribute("value").SetValue(Convert.ToBase64String(keyValue));
    
                if (keyIdEl != null)
                    keyIdEl.Attribute("value").SetValue(keyId);
    
                doc.Save(xmlFileName);
            }
    
            /// <summary>
            // Encrypts clear Smooth Streaming to Smooth Streaming with PlayReady.
            // Then, packages the PlayReady Smooth Streaming to HLS with PlayReady.
            /// </summary>
            /// <param name="clearSmoothAsset">Asset that contains clear Smooth Streaming.</param>
            /// <returns>The output asset.</returns>
            public static IAsset CreateHLSEncryptedWithPlayReady(IAsset clearSmoothStreamAsset)
            {
                IJob job = _context.Jobs.Create("Encrypt to HLS with PlayReady.");
    
                // Add task 1 - Encrypt Smooth Streaming with PlayReady 
                IAsset encryptedSmoothAsset =
                    EncryptSmoothStreamWithPlayReadyTask(job, clearSmoothStreamAsset);
    
                // Add task 2 - Package to HLS with PlayReady.
                PackageSmoothStreamToHLS(job, encryptedSmoothAsset);
    
                // Submit the job and wait until it is completed.
                job.Submit();
                job = job.StartExecutionProgressTask(
                    j =>
                    {
                        Console.WriteLine("Job state: {0}", j.State);
                        Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
                    },
                    CancellationToken.None).Result;
    
                // Since we had two tasks, the OutputMediaAssets[1]
                // contains the desired asset.
                _context.Locators.Create(
                    LocatorType.OnDemandOrigin,
                    job.OutputMediaAssets[1],
                    AccessPermissions.Read,
                    TimeSpan.FromDays(30));
    
                return job.OutputMediaAssets[1];
            }
    
            /// <summary>
            /// Uploads a single file.
            /// </summary>
            /// <param name="fileDir">The location of the files.</param>
            /// <param name="assetCreationOptions">
            ///  You can specify the following encryption options for the AssetCreationOptions.
            ///      None:  no encryption.  
            ///      StorageEncrypted: storage encryption. Encrypts a clear input file 
            ///        before it is uploaded to Azure storage. 
            ///      CommonEncryptionProtected: for Common Encryption Protected (CENC) files. 
            ///        For example, a set of files that are already PlayReady encrypted. 
            ///      EnvelopeEncryptionProtected: for HLS with AES encryption files.
            ///        NOTE: The files must have been encoded and encrypted by Transform Manager. 
            ///     </param>
            /// <returns>Returns an asset that contains a single file.</returns>
            /// </summary>
            /// <returns></returns>
            private static IAsset IngestSingleMP4File(string fileDir, AssetCreationOptions assetCreationOptions)
            {
                // Use the SDK extension method to create a new asset by 
                // uploading a mezzanine file from a local path.
                IAsset asset = _context.Assets.CreateFromFile(
                    fileDir,
                    assetCreationOptions,
                    (af, p) =>
                    {
                        Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
                    });
    
    
                return asset;
    
            }
            /// <summary>
            /// Creates a task to encode to Adaptive Bitrate. 
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncodeSingleMP4IntoMultibitrateMP4sTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Media Encoder Standard.
                IMediaProcessor encoder = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.MediaEncoderStandard);
    
                ITask adpativeBitrateTask = job.Tasks.AddNew("MP4 to Adaptive Bitrate Task",
                   encoder,
                   "H264 Multiple Bitrate 720p",
                   TaskOptions.None);
    
                // Specify the input Asset
                adpativeBitrateTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset abrAsset = adpativeBitrateTask.OutputAssets.AddNew("Multibitrate MP4s",
                                        AssetCreationOptions.None);
    
                return abrAsset;
            }
    
            /// <summary>
            /// Creates a task to convert the MP4 file(s) to a Smooth Streaming asset.
            /// Adds the new task to a job.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset PackageMP4ToSmoothStreamingTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor packager = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Azure Media Packager does not accept string presets, so load xml configuration
                string smoothConfig = File.ReadAllText(Path.Combine(
                            _configurationXMLFiles,
                            "MediaPackager_MP4toSmooth.xml"));
    
                // Create a new Task to convert adaptive bitrate to Smooth Streaming.
                ITask smoothStreamingTask = job.Tasks.AddNew("MP4 to Smooth Task",
                   packager,
                   smoothConfig,
                   TaskOptions.None);
    
                // Specify the input Asset, which is the output Asset from the first task
                smoothStreamingTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is in the clear (unencrypted).
                IAsset smoothOutputAsset =
                    smoothStreamingTask.OutputAssets.AddNew("Clear Smooth Streaming",
                        AssetCreationOptions.None);
    
                return smoothOutputAsset;
            }
    
    
            /// <summary>
            /// Converts Smooth Stream to HLS.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The Smooth Stream asset.</param>
            /// <returns>The asset that was packaged to HLS.</returns>
            private static IAsset PackageSmoothStreamToHLS(IJob job, IAsset smoothStreamAsset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Packager.
                IMediaProcessor processor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaPackager);
    
                // Read the configuration data into a string. 
                //
                string configuration = File.ReadAllText(
                            Path.Combine(_configurationXMLFiles,
                                        @"MediaPackager_SmoothToHLS.xml"));
    
                // Create a task with the encoding details, using a configuration file.
                ITask task = job.Tasks.AddNew("My Smooth to HLS Task",
                   processor,
                   configuration,
                   TaskOptions.ProtectedConfiguration);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(smoothStreamAsset);
    
                // Add an output asset to contain the results of the job. 
                IAsset outputAsset =
                    task.OutputAssets.AddNew("HLS asset", AssetCreationOptions.None);
    
    
                return outputAsset;
            }
    
            /// <summary>
            /// Creates a task to encrypt Smooth Streaming with PlayReady.
            /// Note: Do deliver DASH, make sure to set the useSencBox and adjustSubSamples 
            /// configuration properties to true.
            /// </summary>
            /// <param name="job">The job to which to add the new task.</param>
            /// <param name="asset">The input asset.</param>
            /// <returns>The output asset.</returns>
            private static IAsset EncryptSmoothStreamWithPlayReadyTask(IJob job, IAsset asset)
            {
                // Get the SDK extension method to  get a reference to the Azure Media Encryptor.
                IMediaProcessor playreadyProcessor = _context.MediaProcessors.GetLatestMediaProcessorByName(
                    MediaProcessorNames.WindowsAzureMediaEncryptor);
    
                // Read the configuration XML.
                //
                // Note that the configuration defined in MediaEncryptor_PlayReadyProtection.xml
                // is using keySeedValue. It is recommended that you do this only for testing 
                // and not in production. For more information, see 
                // http://msdn.microsoft.com/library/windowsazure/dn189154.aspx.
                //
                string configPlayReady = File.ReadAllText(Path.Combine(_configurationXMLFiles,
                                            @"MediaEncryptor_PlayReadyProtection.xml"));
    
                ITask playreadyTask = job.Tasks.AddNew("My PlayReady Task",
                   playreadyProcessor,
                   configPlayReady,
                   TaskOptions.ProtectedConfiguration);
    
                playreadyTask.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.CommonEncryptionProtected.
                IAsset playreadyAsset = playreadyTask.OutputAssets.AddNew(
                                                "PlayReady Smooth Streaming",
                                                AssetCreationOptions.CommonEncryptionProtected);
    
    
                return playreadyAsset;
            }
    
    
            /// <summary>
            /// Configures authorization policy for the content key. 
            /// </summary>
            /// <param name="contentKey">The content key.</param>
            static public void ConfigureLicenseDeliveryService(IContentKey contentKey)
            {
                // Create ContentKeyAuthorizationPolicy with Open restrictions 
                // and create authorization policy          
    
                List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                {
                    new ContentKeyAuthorizationPolicyRestriction 
                    { 
                        Name = "Open", 
                        KeyRestrictionType = (int)ContentKeyRestrictionType.Open, 
                        Requirements = null
                    }
                };
    
                // Configure PlayReady license template.
                string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
                IContentKeyAuthorizationPolicyOption policyOption =
                    _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.PlayReadyLicense,
                            restrictions, newLicenseTemplate);
    
                IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                            ContentKeyAuthorizationPolicies.
                            CreateAsync("Deliver Common Content Key with no restrictions").
                            Result;
    
    
                contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
                // Associate the content key authorization policy with the content key.
                contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                contentKey = contentKey.UpdateAsync().Result;
            }
    
            static private string ConfigurePlayReadyLicenseTemplate()
            {
                // The following code configures PlayReady License Template using .NET classes
                // and returns the XML string.
    
                PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
                PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
    
                responseTemplate.LicenseTemplates.Add(licenseTemplate);
    
                return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
            }
            static private byte[] GetRandomBuffer(int length)
            {
                var returnValue = new byte[length];
    
                using (var rng =
                    new System.Security.Cryptography.RNGCryptoServiceProvider())
                {
                    rng.GetBytes(returnValue);
                }
    
                return returnValue;
            }
    
        }
    }

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

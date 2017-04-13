<properties 
    pageTitle="Πώς να εκτελείτε ζωντανή ροή δεδομένων με χρήση των υπηρεσιών Azure Media Services για να δημιουργήσετε πολλούς-ρυθμό μετάδοσης bit ροών με .NET | Microsoft Azure" 
    description="Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί στα βήματα της δημιουργίας ενός καναλιού που λαμβάνει μια ζωντανή ροή μίας-ρυθμό μετάδοσης bit και κωδικοποιεί το ρυθμό μετάδοσης πολλούς-bit ροή χρησιμοποιώντας .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Πώς να εκτελείτε ζωντανή ροή δεδομένων με χρήση των υπηρεσιών Azure Media Services για να δημιουργήσετε πολλούς-ρυθμό μετάδοσης bit ροών με .NET

> [AZURE.SELECTOR]
- [Πύλη](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Επισκόπηση

Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί στα βήματα της δημιουργίας ενός **καναλιού** που λαμβάνει μια ζωντανή ροή μίας-ρυθμό μετάδοσης bit και κωδικοποιεί το ρυθμό μετάδοσης πολλούς-bit ροή.

Για περισσότερες γενικές πληροφορίες που σχετίζονται με κανάλια που έχουν ενεργοποιηθεί για το πραγματικό κωδικοποίηση, ανατρέξτε στο θέμα [Live ροής με χρήση των υπηρεσιών Azure Media Services για να δημιουργήσετε πολλούς-ρυθμό μετάδοσης bit ροών](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Συνηθισμένο σενάριο ζωντανή ροή

Ακολουθήστε τα παρακάτω βήματα περιγράφουν εργασίες που περιλαμβάνονται στη δημιουργία κοινές ζωντανή ροή εφαρμογές.

>[AZURE.NOTE] Προς το παρόν, η max προτεινόμενη διάρκεια ενός συμβάντος live είναι 8 ώρες. Εάν χρειάζεστε για να εκτελέσετε ένα κανάλι για μεγαλύτερα χρονικά διαστήματα, επικοινωνήστε με amslived στο Microsoft.com.

1. Συνδέστε μια βιντεοκάμερα σε έναν υπολογιστή. Εκκίνηση και ρύθμιση παραμέτρων σε μια ζωντανή encoder εσωτερικής εγκατάστασης με δυνατότητα εξόδου μια ροή μία ρυθμό μετάδοσης bit σε ένα από τα ακόλουθα πρωτόκολλα: RTMP, ομαλή ροή ή RTP (MPEG-ΕΕ). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [υποστήριξη RTMP υπηρεσίες πολυμέσων Azure και Live κωδικοποιητές](http://go.microsoft.com/fwlink/?LinkId=532824).

Επίσης μπορεί να ολοκληρωθεί αυτό το βήμα, αφού δημιουργήσετε το κανάλι.

1. Δημιουργία και ξεκινήστε ένα κανάλι.

1. Ανάκτηση του καναλιού ingest διεύθυνση URL.

Η διεύθυνση URL ingest χρησιμοποιείται από το live κωδικοποιητή για να στείλετε τη ροή στο κανάλι.

1. Ανάκτηση της διεύθυνσης URL προεπισκόπηση καναλιού.

Χρησιμοποιήστε αυτήν τη διεύθυνση URL για να επιβεβαιώσετε ότι το κανάλι σωστά λαμβάνει το ζωντανή ροή.

2. Δημιουργία ενός περιουσιακού στοιχείου.
3. Εάν θέλετε για το πάγιο δυναμικά κρυπτογράφησης κατά την αναπαραγωγή, κάντε τα εξής:
1. Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου.
1. Ρύθμιση παραμέτρων πολιτική εξουσιοδότησης του αριθμού-κλειδιού του περιεχομένου.
1. Ρύθμιση πολιτικής παράδοσης περιουσιακών στοιχείων (χρησιμοποιούνται από δυναμική συσκευασία και δυναμική κρυπτογράφηση).
3. Δημιουργήστε ένα πρόγραμμα και καθορίστε για να χρησιμοποιήσετε το περιουσιακών στοιχείων που δημιουργήσατε.
1. Δημοσίευση του παγίου που σχετίζεται με το πρόγραμμα, δημιουργώντας ένα προσδιοριστικό OnDemand.

Βεβαιωθείτε ότι έχετε τουλάχιστον μία ροή δεσμευμένη των μονάδων το ροής τελικό σημείο από το οποίο θέλετε να ροή περιεχομένου.

1. Ξεκινήστε το πρόγραμμα όταν είστε έτοιμοι για να ξεκινήσει η ροή και αρχειοθέτησης.
2. Προαιρετικά, ο κωδικοποιητής live μπορούν να ειδοποιηθεί για να ξεκινήσετε μια διαφήμιση. Η κοινοποίηση εισάγεται στη ροή εξόδου.
1. Διακοπή του προγράμματος όταν θέλετε να διακόψετε τη ροή και αρχειοθέτηση το συμβάν.
1. Διαγραφή του προγράμματος (και προαιρετικά διαγραφή παγίου).

## <a name="what-youll-learn"></a>Τι θα μάθετε

Αυτό το θέμα δείχνει πώς μπορείτε να εκτελεί διαφορετικές λειτουργίες σε κανάλια και προγράμματα που χρησιμοποιούν SDK .NET υπηρεσίες πολυμέσων. Επειδή πολλές λειτουργίες μεγάλη διάρκεια εκτέλεσης API .NET που Διαχείριση χρόνο εκτέλεση λειτουργιών που χρησιμοποιούνται.

Το θέμα δείχνει πώς μπορείτε να κάνετε τα εξής:

1. Δημιουργία και ξεκινήστε ένα κανάλι. Μεγάλη διάρκεια εκτέλεσης APIs χρησιμοποιούνται.
1. Λάβετε τα κανάλια ingest (εισαγωγής) τελικού σημείου. Αυτό το τελικό σημείο θα πρέπει να παρέχεται ο κωδικοποιητής που μπορούν να αποστείλουν μια ζωντανή ροή μία ρυθμό μετάδοσης bit.
1. Λάβετε το τελικό σημείο preview. Αυτό το τελικό σημείο χρησιμοποιείται για να κάνετε προεπισκόπηση του ροής.
1. Δημιουργία ενός περιουσιακού στοιχείου που θα χρησιμοποιηθεί για την αποθήκευση του περιεχομένου. Επίσης, όπως φαίνεται σε αυτό το παράδειγμα, πρέπει να ρυθμιστεί τις πολιτικές παράδοσης περιουσιακών στοιχείων.
1. Δημιουργήστε ένα πρόγραμμα και καθορίστε για να χρησιμοποιήσετε το περιουσιακών στοιχείων που δημιουργήθηκε σε προηγούμενη έκδοση. Ξεκινήστε το πρόγραμμα. Μεγάλη διάρκεια εκτέλεσης APIs χρησιμοποιούνται.
1. Δημιουργήστε ένα προσδιοριστικό του παγίου, ώστε να λαμβάνει δημοσιευτεί το περιεχόμενο και να δυνατό να διοχετεύονται σε υπολογιστές-πελάτες σας.
1. Εμφάνιση και απόκρυψη slates. Έναρξη και διακοπή διαφημίσεις. Μεγάλη διάρκεια εκτέλεσης APIs χρησιμοποιούνται.
1. Εκκαθάριση το κανάλι και όλους τους πόρους που σχετίζονται.


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Απαιτούνται τα εξής για να ολοκληρώσετε το πρόγραμμα εκμάθησης.

- Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure.

Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε ένα δωρεάν λογαριασμό της δοκιμαστικής έκδοσης σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης](/pricing/free-trial/?WT.mc_id=A261C142F). Λαμβάνετε πιστώσεων που μπορούν να χρησιμοποιηθούν για να δοκιμάσετε την πληρωμή Azure υπηρεσίες. Ακόμα και μετά το πιστώσεων χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και να χρησιμοποιήσετε δωρεάν Azure υπηρεσίες και τις δυνατότητες, όπως η δυνατότητα Web Apps στο Azure εφαρμογής υπηρεσίας.
- Ένας λογαριασμός Media Services. Για να δημιουργήσετε ένα λογαριασμό Media Services, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate ή Express) ή νεότερες εκδόσεις.
- Πρέπει να χρησιμοποιήσετε το Media Services .NET SDK έκδοση 3.2.0.0 ή νεότερη έκδοση.
- Κάμερα Web και κωδικοποιητής που μπορούν να αποστείλουν μια ζωντανή ροή μία ρυθμό μετάδοσης bit.

##<a name="considerations"></a>Ζητήματα

- Προς το παρόν, η max προτεινόμενη διάρκεια ενός συμβάντος live είναι 8 ώρες. Εάν χρειάζεστε για να εκτελέσετε ένα κανάλι για μεγαλύτερα χρονικά διαστήματα, επικοινωνήστε με amslived στο Microsoft.com.
- Βεβαιωθείτε ότι έχετε τουλάχιστον μία ροή δεσμευμένη των μονάδων το ροής τελικό σημείο από το οποίο θέλετε να ροή περιεχομένου.

##<a name="download-sample"></a>Λήψη δείγματος

Λήψη και να εκτελέσετε ένα δείγμα από [εδώ](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Ρύθμιση για την ανάπτυξη με το Media Services SDK για .NET

1. Δημιουργία μιας εφαρμογής κονσόλας χρήση του Visual Studio.
1. Προσθέστε το Media Services SDK για .NET στην εφαρμογή σας κονσόλας χρησιμοποιώντας πακέτο NuGet υπηρεσίες πολυμέσων.

##<a name="connect-to-media-services"></a>Σύνδεση με τις υπηρεσίες πολυμέσων
Ως βέλτιστη πρακτική, που πρέπει να χρησιμοποιήσετε ένα αρχείο app.config για να αποθηκεύσετε το κλειδί Media Services και το όνομα του λογαριασμού.

>[AZURE.NOTE]Για να βρείτε το όνομα και το κλειδί τιμές, μεταβείτε στην πύλη του Azure και επιλέξτε το λογαριασμό σας. Εμφανίζεται το παράθυρο διαλόγου ρυθμίσεις στα δεξιά. Στο παράθυρο "Ρυθμίσεις", επιλέξτε κλειδιά. Κάνοντας κλικ στο εικονίδιο δίπλα σε κάθε πλαίσιο κειμένου αντιγράφει την τιμή στο Πρόχειρο του συστήματος.

Προσθέστε την ενότητα appSettings στο αρχείο app.config και ορίστε τις τιμές για τις υπηρεσίες πολυμέσων κλειδί λογαριασμού σας όνομα και το λογαριασμό.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Παράδειγμα κώδικα

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Αναζητάτε κάτι άλλο;

Εάν αυτό το θέμα δεν περιέχουν τι περιμένατε, κάτι που λείπει ή σε κάποιο άλλο τρόπο δεν καλύπτει τις ανάγκες σας, πρέπει να δώσετε μας μαζί σας με χρήση του παρακάτω Disqus νήματος σχολίων.

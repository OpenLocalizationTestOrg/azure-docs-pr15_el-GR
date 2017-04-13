<properties 
    pageTitle="Κωδικοποίηση με τυπική κωδικοποιητή πολυμέσων για προχωρημένους" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να εκτελέσετε για προχωρημένους κωδικοποίηση, προσαρμόζοντας Media Encoder τυπική υποδείγματα εργασίας. Το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε SDK .NET υπηρεσίες πολυμέσων για να δημιουργήσετε ένα κωδικοποίησης εργασιών και έργων. Εμφανίζει επίσης πώς μπορείτε να παρέχετε προσαρμοσμένες προκαθορισμένες ρυθμίσεις κωδικοποίησης στο έργο." 
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
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Κωδικοποίηση με τυπική κωδικοποιητή πολυμέσων για προχωρημένους

##<a name="overview"></a>Επισκόπηση

Αυτό το θέμα δείχνει πώς μπορείτε να εκτελέσετε σύνθετες εργασίες κωδικοποίησης με τυπική κωδικοποιητή πολυμέσων. Το θέμα δείχνει [πώς μπορείτε να χρησιμοποιήσετε .NET για να δημιουργήσετε μια εργασία του κωδικοποίησης και μια εργασία που εκτελεί αυτήν την εργασία](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Εμφανίζει επίσης πώς μπορείτε να παρέχετε προσαρμοσμένες προκαθορισμένες ρυθμίσεις κωδικοποίησης στην εργασία. Για περιγραφή των στοιχείων που χρησιμοποιούνται από τις προκαθορισμένες ρυθμίσεις, ανατρέξτε στο θέμα [αυτού του εγγράφου](https://msdn.microsoft.com/library/mt269962.aspx). 

Τα προσαρμοσμένα υποδείγματα που εκτελούν τις ακόλουθες εργασίες κωδικοποίησης εκδηλώνονται:

- [Δημιουργία μικρογραφίες](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Αποκοπή βίντεο (απόσπασμα)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Δημιουργήστε μια επικάλυψη](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Εισαγάγετε ένα σιωπηρή κομμάτι κατά την εισαγωγή δεδομένων δεν έχει ήχο](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Απενεργοποίηση της αυτόματης απενεργοποίησης συνένωση](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Υποδείγματα μόνο ήχου](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [Συνενώσετε δύο ή περισσότερα αρχεία βίντεο](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Περικοπή βίντεο με τυπική κωδικοποιητή πολυμέσων](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Εισαγάγετε ένα κομμάτι βίντεο κατά την εισαγωγή δεδομένων δεν έχει βίντεο](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Περιστροφή ενός βίντεο](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Κωδικοποίηση με τις υπηρεσίες πολυμέσων .NET SDK

Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί SDK .NET υπηρεσίες πολυμέσων για να εκτελέσετε τις ακόλουθες εργασίες:

- Δημιουργήστε μια εργασία κωδικοποίησης.
- Λάβετε μια αναφορά σε τυπική Media Encoder κωδικοποιητή.
- Φόρτωση του προσαρμοσμένου XML ή προκαθορισμένες JSON. Μπορείτε να αποθηκεύσετε το αρχείο XML ή JSON (για παράδειγμα, [XML](media-services-custom-mes-presets-with-dotnet.md#xml) ή [JSON](media-services-custom-mes-presets-with-dotnet.md#json) σε ένα αρχείο και χρησιμοποιήστε τον παρακάτω κώδικα για να φορτώσετε το αρχείο.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Προσθέστε μια κωδικοποίησης εργασία στο έργο. 
- Καθορίστε το εισαγωγής περιουσιακού στοιχείου για κωδικοποίηση.
- Δημιουργία ενός περιουσιακού στοιχείου εξόδου που περιέχει τα κωδικοποιημένα περιουσιακών στοιχείων.
- Προσθήκη ενός προγράμματος χειρισμού συμβάντων για να ελέγξετε την πρόοδο του έργου.
- Υποβάλετε την εργασία.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
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
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
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
        
            }
        }


##<a name="support-for-relative-sizes"></a>Υποστήριξη για σχετική μεγέθη

Κατά τη δημιουργία μικρογραφίες του, δεν χρειάζεται να πάντα καθορίσετε εξόδου πλάτος και ύψος σε pixel. Μπορείτε να τα καθορίσετε στο ποσοστά, στην περιοχή [1%,..., 100%].

###<a name="json-preset"></a>Προκαθορισμένα JSON 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>Προκαθορισμένα XML
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Δημιουργία μικρογραφίες

Αυτή η ενότητα δείχνει πώς μπορείτε να προσαρμόσετε μια προκαθορισμένη ρύθμιση που δημιουργεί μικρογραφίες. Η προκαθορισμένη ρύθμιση που ορίζονται από το κάτω από το στοιχείο περιέχει πληροφορίες σχετικά με τον τρόπο που θέλετε να κωδικοποιήσετε το αρχείο, καθώς και πληροφορίες που απαιτούνται για τη δημιουργία μικρογραφίες. Μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx) και να προσθέσετε κώδικα που δημιουργεί μικρογραφίες.  

>[AZURE.NOTE]Η ρύθμιση **SceneChangeDetection** στο παρακάτω προκαθορισμένο δυνατότητα μόνο έχει οριστεί σε true, αν κωδικοποίηση σε ένα μεμονωμένο ρυθμό μετάδοσης bit βίντεο. Εάν είναι κωδικοποίηση με πολλούς-ρυθμό μετάδοσης bit βίντεο και **SceneChangeDetection** την τιμή true, ο κωδικοποιητής επιστρέφει ένα σφάλμα.  


Για πληροφορίες σχετικά με το σχήμα, ανατρέξτε [σε αυτό](https://msdn.microsoft.com/library/mt269962.aspx) το θέμα.

Βεβαιωθείτε ότι για να δείτε την ενότητα [ζητήματα](media-services-custom-mes-presets-with-dotnet.md#considerations) .

###<a id="json"></a>Προκαθορισμένα JSON


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>Προκαθορισμένα XML


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Ζητήματα

Ισχύουν τα ακόλουθα θέματα:

- Τη χρήση ρητή χρονικές σημάνσεις για έναρξη/βήμα/περιοχή προϋποθέτει ότι η προέλευση εισόδου είναι πολύ τουλάχιστον 1 λεπτό.
- Png/jpg/BmpImage στοιχεία έχουν Έναρξη, βήμα, και την περιοχή ιδιότητες της συμβολοσειράς – αυτές μπορεί να θεωρηθούν:

    - Πλαίσιο αριθμό, εάν είναι ακέραιοι μη αρνητικός, για παράδειγμα "Έναρξη": "120",
    - Σχετική προέλευσης διάρκεια εάν εκφρασμένη ως ποσοστό επίθημα, για παράδειγμα "εκκίνηση": "15%", ή
    - Χρονική σήμανση εάν εκφρασμένη HH:MM:SS... μορφοποίηση, για παράδειγμα "εκκίνηση": "00: 01:00"

    Μπορείτε να συνδυάσετε και να ταιριάζει επικοινωνήστε με σημειογραφίας κατά τη.
    
    Επιπλέον, Έναρξη υποστηρίζει επίσης μια ειδική μακροεντολή: {βέλτιστη}, που προσπαθεί να προσδιορίσετε το πρώτο καρέ "ενδιαφέρον" τη σημείωση περιεχομένου: (βήμα και περιοχή λαμβάνονται υπόψη κατά την έναρξη έχει οριστεί σε {καλύτερα})
    
    - Προεπιλογών: Έναρξη: {βέλτιστη}
- Μορφή εξόδου πρέπει να παρέχεται ρητά για κάθε μορφή εικόνας: Jpg/Png BmpFormat. Κατά την παρουσίαση, MES αντιστοιχίζει JpgVideo να JpgFormat και ούτω καθεξής. OutputFormat παρουσιάζει μια νέα μακροεντολή συγκεκριμένη εικόνα κωδικοποιητή: {Index}, ποια πρέπει να είναι παρουσίαση (μία φορά και μόνο μία φορά) για τις μορφές εξόδου εικόνας.

##<a id="trim_video"></a>Αποκοπή βίντεο (απόσπασμα)

Αυτή η ενότητα ακρόασης σχετικά με την τροποποίηση τα υποδείγματα encoder clip ή αποκοπή βίντεο εισαγωγής όπου τα δεδομένα εισόδου είναι ένα αρχείο λεγόμενες θυγατρική ή σε ζήτηση. Ο κωδικοποιητής μπορεί επίσης να χρησιμοποιηθεί για clip ή αποκοπή ενός περιουσιακού στοιχείου, το οποίο έχει καταγράψει ή αρχειοθετηθούν από μια ζωντανή ροή-τις λεπτομέρειες για αυτό είναι διαθέσιμες σε [αυτό το ιστολόγιο](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Για να αποκόψετε τα βίντεό σας, μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx) και να τροποποιήσετε το στοιχείο **προελεύσεις** (όπως φαίνεται παρακάτω). Η τιμή της ώρας έναρξης πρέπει να ταιριάζει με τις απόλυτες χρονικές σημάνσεις των το βίντεο εισαγωγής. Για παράδειγμα, εάν το πρώτο καρέ του βίντεο εισαγωγής έχει μια χρονική σήμανση της 12:00:10.000, στη συνέχεια, ώρα έναρξης πρέπει να είναι τουλάχιστον 12:00:10.000 και μεγαλύτερη. Στο παρακάτω παράδειγμα, θα σας λαμβάνεται ως δεδομένο ότι το βίντεο εισαγωγής έχει μια χρονική σήμανση έναρξης μηδέν. **Προελεύσεις** πρέπει να τοποθετηθούν στην αρχή της προεπιλογής. 
 
###<a id="json"></a>Προκαθορισμένα JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>Προκαθορισμένα XML
    
Για να αποκόψετε τα βίντεό σας, μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx) και να τροποποιήσετε το στοιχείο **προελεύσεις** (όπως φαίνεται παρακάτω).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Δημιουργήστε μια επικάλυψη

Η τυπική κωδικοποιητή πολυμέσων σάς επιτρέπει να επικάλυψης μια εικόνα σε ένα υπάρχον βίντεο. Προς το παρόν, υποστηρίζονται οι παρακάτω μορφές: png, jpg, gif, και bmp. Η προκαθορισμένη ρύθμιση που ορίζονται από το κάτω από το είναι ένα βασικό παράδειγμα σχετικά με μια επικάλυψη του βίντεο.

Εκτός από τον ορισμό ενός προκαθορισμένου αρχείου, πρέπει επίσης να σας επιτρέπουν να Media Services γνωρίζετε ποιο αρχείο στο περιουσιακού στοιχείου είναι η εικόνα επικάλυψη και το αρχείο που είναι η προέλευση βίντεο στον οποίο θέλετε να γίνει επικάλυψη την εικόνα. Το αρχείο βίντεο πρέπει να είναι το **κύριο** αρχείο. 

Το παραπάνω παράδειγμα .NET καθορίζει δύο συναρτήσεις: **UploadMediaFilesFromFolder** και **EncodeWithOverlay**. Η συνάρτηση UploadMediaFilesFromFolder κάνει αποστολή των αρχείων από ένα φάκελο (για παράδειγμα, BigBuckBunny.mp4 και Image001.png) και ορίζει το αρχείο mp4 να είναι το κύριο αρχείο στην περιουσιακού στοιχείου. Η συνάρτηση **EncodeWithOverlay** χρησιμοποιεί το προσαρμοσμένο αρχείο προκαθορισμένο που μεταβιβάστηκε σε αυτό (για παράδειγμα, το υπόδειγμα που ακολουθεί) για να δημιουργήσετε την εργασία κωδικοποίησης. 

>[AZURE.NOTE]Τρέχουσα περιορισμοί:
>
>Δεν υποστηρίζεται η ρύθμιση αδιαφάνειας επικάλυψης.
>
>Το αρχείο προέλευσης του βίντεο και το αρχείο εικόνας επικάλυψη πρέπει να βρίσκονται σε τον ίδιο πόρο και στο αρχείο βίντεο πρέπει να οριστεί ως το κύριο αρχείο στην αυτού του περιουσιακού στοιχείου.

###<a name="json-preset"></a>Προκαθορισμένα JSON
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>Προκαθορισμένα XML
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Εισαγάγετε ένα σιωπηρή κομμάτι κατά την εισαγωγή δεδομένων δεν έχει ήχο

Από προεπιλογή, εάν στείλετε είσοδο σε ο κωδικοποιητής που περιέχει μόνο βίντεο και ήχου δεν υπάρχει, στη συνέχεια, παγίου εξόδου περιέχει τα αρχεία που περιέχουν δεδομένα μόνο βίντεο. Ορισμένα προγράμματα αναπαραγωγής ενδέχεται να μην μπορείτε να χειριστείτε όπως ροές εξόδου. Μπορείτε να χρησιμοποιήσετε αυτήν τη ρύθμιση για να επιβάλετε ο κωδικοποιητής για να προσθέσετε ένα κομμάτι σιωπηρή το αποτέλεσμα σε αυτό το σενάριο.

Για να επιβάλετε ο κωδικοποιητής για τη δημιουργία ενός περιουσιακού στοιχείου που περιέχει ένα σιωπηρή κομμάτι κατά την εισαγωγή δεδομένων δεν έχει ήχο, καθορίστε την τιμή "InsertSilenceIfNoAudio".

Μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx), και να πραγματοποιήσετε την τροποποίηση παρακάτω:

###<a name="json-preset"></a>Προκαθορισμένα JSON

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>Προκαθορισμένα XML

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Απενεργοποίηση της αυτόματης απενεργοποίησης συνένωση

Οι πελάτες δεν χρειάζεται να κάνετε τίποτα εάν θέλουν τη συνένωση στα περιεχόμενα συνενωμένες αυτόματα καταργήστε την. Όταν η αυτόματη καταργήστε την συνένωση είναι ενεργοποιημένη (προεπιλογή) το MES κάνει το Αυτόματος εντοπισμός συνενωμένες πλαισίων και μόνο interlaces καταργήστε την καρέ έχει επισημανθεί ώστε να πεπλεγμένη.

Μπορείτε να απενεργοποιήσετε την αυτόματη κατάργηση συνένωση. Αυτή η επιλογή δεν συνιστάται.

###<a name="json-preset"></a>Προκαθορισμένα JSON
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>Προκαθορισμένα XML
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Υποδείγματα μόνο ήχου

Αυτή η ενότητα παρουσιάζει δύο υποδείγματα MES μόνο ήχου: AAC ήχου και AAC καλή ποιότητα ήχου.

###<a name="aac-audio"></a>AAC ήχου 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC καλή ποιότητα ήχου

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>Συνενώσετε δύο ή περισσότερα αρχεία βίντεο

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να δημιουργήσετε μια προκαθορισμένη ρύθμιση για να συνενώσετε δύο ή περισσότερα αρχεία βίντεο. Το πιο συνηθισμένο σενάριο είναι όταν θέλετε να προσθέσετε μια κεφαλίδα ή ρυμουλκούμενο στο κύριο βίντεο. Η προβλεπόμενη χρήση είναι όταν τα αρχεία βίντεο που υφίσταται επεξεργασία μαζί, κοινή χρήση Ιδιότητες (ανάλυση της οθόνης, ρυθμός καρέ, πλήθος κομμάτι ήχου, κ.λπ.). Θα πρέπει να αναλαμβάνουν δεν για να συνδυάσετε βίντεο διαφορετικό πλαίσιο χρεώσεων ή με διαφορετικό αριθμό κομμάτια ήχου.

###<a name="requirements-and-considerations"></a>Απαιτήσεις και τα θέματα

- Εισαγωγής βίντεο θα πρέπει να έχετε μόνο μία παρακολούθηση ήχου.
- Εισαγωγή βίντεο, πρέπει να έχει τον ίδιο συντελεστή πλαισίου.
- Πρέπει να αποστείλετε τα βίντεό σας σε ξεχωριστά στοιχεία και να ορίσετε τα βίντεο ως το πρωτεύον αρχείο σε κάθε περιουσιακών στοιχείων.
- Πρέπει να γνωρίζετε τη διάρκεια του βίντεό σας.
- Στα παρακάτω παραδείγματα προκαθορισμένο προϋποθέτει ότι όλα τα βίντεο εισαγωγής Έναρξη με χρονική σήμανση μηδέν. Πρέπει να τροποποιήσετε τις τιμές ώρας έναρξης, εάν τα βίντεο έχουν διαφορετική αρχική χρονική σήμανση, όπως συνήθως συμβαίνει με ζωντανή αρχειοθηκών.
- Τα προκαθορισμένα JSON κάνει ρητές αναφορές στις τιμές AssetID των περιουσιακών στοιχείων εισαγωγής.
- Το δείγμα κώδικα προϋποθέτει ότι η προκαθορισμένη ρύθμιση JSON έχει αποθηκευτεί σε ένα τοπικό αρχείο, όπως "C:\supportFiles\preset.json". Επίσης, θεωρείται ότι έχουν δημιουργηθεί δύο στοιχεία με την αποστολή δύο αρχεία βίντεο και ότι γνωρίζετε τις τιμές assetid που προκύπτει.
- Το τμήμα κώδικα και JSON προκαθορισμένα παρουσιάζει ένα παράδειγμα συνένωση δύο αρχεία βίντεο. Μπορείτε να τον επεκτείνετε σε περισσότερες από δύο βίντεο από:

    1. Κλήση εργασίας. InputAssets.Add() TAB, για να προσθέσετε περισσότερα βίντεο, με τη σειρά.
    2. Πραγματοποίηση αντιστοιχεί επεξεργάζεται στο στοιχείο "Προελεύσεις" στο το JSON, προσθέτοντας περισσότερες εγγραφές, με την ίδια σειρά. 


###<a name="net-code"></a>Κωδικός .NET

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>Προκαθορισμένα JSON

Ενημέρωση προσαρμοσμένου προκαθορισμένες με αναγνωριστικά των περιουσιακών στοιχείων που θέλετε να συνενώσετε και με το τμήμα κατάλληλη ώρα για κάθε βίντεο.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Περικοπή βίντεο με τυπική κωδικοποιητή πολυμέσων

Ανατρέξτε στο θέμα [Περικοπή βίντεο με τυπική κωδικοποιητή πολυμέσων](media-services-crop-video.md) .

##<a id="no_video"></a>Εισαγάγετε ένα κομμάτι βίντεο κατά την εισαγωγή δεδομένων δεν έχει βίντεο

Από προεπιλογή, εάν στείλετε είσοδο σε ο κωδικοποιητής που περιέχει μόνο ήχου και βίντεο, στη συνέχεια, παγίου εξόδου περιέχει τα αρχεία που περιέχουν δεδομένα μόνο ήχου. Ορισμένα προγράμματα αναπαραγωγής, συμπεριλαμβανομένου Azure Media Player (ανατρέξτε στο θέμα [αυτό](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) ενδέχεται να μην μπορείτε να χειριστείτε όπως ροές. Μπορείτε να χρησιμοποιήσετε αυτήν τη ρύθμιση για να επιβάλετε ο κωδικοποιητής για να προσθέσετε μια μονόχρωμες παρακολούθηση βίντεο για το αποτέλεσμα σε αυτό το σενάριο. 

>[AZURE.NOTE]Να επιβάλετε ο κωδικοποιητής για να εισαγάγετε ένα κομμάτι βίντεο εξόδου αυξάνει το μέγεθος του αποτελέσματος περιουσιακών στοιχείων, και συνεπώς το κόστος που προκύπτει για την εργασία κωδικοποίησης. Θα πρέπει να εκτελέσετε δοκιμές για να επαληθεύσετε ότι αυτή η αύξηση προκύπτει έχει μόνο μέτρια επίδραση στις μηνιαίων χρεώσεων.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Εισαγωγή βίντεο στο μόνο τη χαμηλότερη ρυθμό μετάδοσης bit

Ας υποθέσουμε ότι χρησιμοποιείτε ένα πολλών ρυθμό μετάδοσης bit κωδικοποίηση προκαθορισμένες όπως ["H264 πολλών ρυθμό μετάδοσης bit 720p"](https://msdn.microsoft.com/library/mt269960.aspx) για να κωδικοποιήσετε ολόκληρο εισαγωγής κατάλογό σας για τη ροή, που περιλαμβάνει ένα συνδυασμό και των αρχείων βίντεο και αρχείων μόνο ήχου. Σε αυτό το σενάριο, όταν τα δεδομένα εισόδου δεν έχει βίντεο, μπορεί να θέλετε να επιβάλετε ο κωδικοποιητής για να εισαγάγετε μια μονόχρωμες κομμάτι βίντεο στο απλώς τη χαμηλότερη ρυθμό μετάδοσης bit, σε σύγκριση με την εισαγωγή βίντεο σε κάθε ρυθμό μετάδοσης bit εξόδου. Για να επιτύχετε αυτό, πρέπει να ορίσετε τη σημαία "InsertBlackIfNoVideoBottomLayerOnly".

Μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx), και να πραγματοποιήσετε την τροποποίηση παρακάτω:

#### <a name="json-preset"></a>Προκαθορισμένα JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Προκαθορισμένα XML

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Εισαγωγή βίντεο καθόλου εξόδου bitrates

Ας υποθέσουμε ότι χρησιμοποιείτε ένα πολλών ρυθμό μετάδοσης bit κωδικοποίηση προκαθορισμένες όπως ["H264 πολλών ρυθμό μετάδοσης bit 720p](https://msdn.microsoft.com/library/mt269960.aspx) για την κωδικοποίηση ολόκληρο εισαγωγής κατάλογό σας για τη ροή, που περιλαμβάνει ένα συνδυασμό και των αρχείων βίντεο και αρχείων μόνο ήχου. Σε αυτό το σενάριο, όταν τα δεδομένα εισόδου δεν έχει βίντεο, μπορεί να θέλετε να επιβάλετε ο κωδικοποιητής για να εισαγάγετε μια μονόχρωμες κομμάτι βίντεο σε όλα τα bitrates εξόδου. Αυτό εξασφαλίζει ότι το αποτέλεσμα περιουσιακά στοιχεία είναι όλα ομοιογενές σε σχέση με αριθμό κομμάτια ήχου και βίντεο κομμάτια. Για να επιτύχετε αυτό, πρέπει να ορίσετε τη σημαία "InsertBlackIfNoVideo".

Μπορείτε να κάνετε οποιοδήποτε από τα MES υποδείγματα τεκμηρίωση [εδώ](https://msdn.microsoft.com/library/mt269960.aspx), και να πραγματοποιήσετε την τροποποίηση παρακάτω:

#### <a name="json-preset"></a>Προκαθορισμένα JSON

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>Προκαθορισμένα XML
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Περιστροφή ενός βίντεο

Η [Τυπική κωδικοποιητή πολυμέσων](media-services-dotnet-encode-with-media-encoder-standard.md) υποστηρίζει περιστροφή από τις γωνίες του 0/90/180/270. Η προεπιλεγμένη συμπεριφορά είναι "Αυτόματη", όπου προσπαθεί να εντοπίσει τα μετα-δεδομένα περιστροφής στο εισερχόμενες αρχείο βίντεο και αποζημιώσει για αυτήν. Συμπεριλάβετε το ακόλουθο στοιχείο **προελεύσεις** σε ένα από τα υποδείγματα καθορισμένο [εδώ](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>Προκαθορισμένα JSON

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>Προκαθορισμένα XML

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Δείτε επίσης [αυτό](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) το θέμα για περισσότερες πληροφορίες σε με ποιον τρόπο ο κωδικοποιητής ερμηνεύει τις ρυθμίσεις πλάτους και ύψους στο υπόδειγμα, όταν ενεργοποιείται αποζημίωση περιστροφής.

Μπορείτε να χρησιμοποιήσετε την τιμή "0" για να υποδείξετε σε ο κωδικοποιητής για να αγνοήσετε περιστροφής μετα-δεδομένα, εάν υπάρχει, το βίντεο εισαγωγής. 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης 

[Επισκόπηση κωδικοποίησης υπηρεσίες πολυμέσων](media-services-encode-asset.md)

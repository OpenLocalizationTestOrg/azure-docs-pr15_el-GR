<properties 
    pageTitle="Πώς να δημιουργείτε μικρογραφίες, χρησιμοποιώντας την τυπική κωδικοποιητή πολυμέσων με .NET" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET για να κωδικοποιήσετε ενός περιουσιακού στοιχείου και να δημιουργήσετε μικρογραφίες ταυτόχρονα με χρήση Strandard κωδικοποιητή πολυμέσων." 
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
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Πώς να δημιουργείτε μικρογραφίες, χρησιμοποιώντας την τυπική κωδικοποιητή πολυμέσων με .NET

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε SDK .NET υπηρεσίες πολυμέσων για να κωδικοποιήσετε ενός περιουσιακού στοιχείου και να δημιουργήσετε μικρογραφίες, χρησιμοποιώντας Media Encoder Standard. Το θέμα ορίζει τα υποδείγματα μικρογραφιών XML και JSON που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια εργασία που κάνει την κωδικοποίηση και δημιουργεί μικρογραφίες την ίδια στιγμή. [Αυτό](https://msdn.microsoft.com/library/mt269962.aspx) το έγγραφο περιέχει περιγραφές των στοιχείων που χρησιμοποιούνται από αυτές τις προκαθορισμένες ρυθμίσεις.

Βεβαιωθείτε ότι για να δείτε την ενότητα [ζητήματα](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) .

##<a name="example"></a>Παράδειγμα

Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί SDK .NET υπηρεσίες πολυμέσων για να εκτελέσετε τις ακόλουθες εργασίες:

- Δημιουργήστε μια εργασία κωδικοποίησης.
- Λάβετε μια αναφορά σε τυπική Media Encoder κωδικοποιητή.
- Τοποθετήστε το προκαθορισμένο [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) ή [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) που περιέχουν τις κωδικοποίησης προκαθορισμένα καθώς και πληροφορίες που απαιτούνται για τη δημιουργία μικρογραφίες. Μπορείτε να αποθηκεύσετε αυτό [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) ή [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) σε ένα αρχείο και να χρησιμοποιήσετε τον παρακάτω κώδικα για να φορτώσετε το αρχείο.

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText(fileName);  
- Προσθέστε μία κωδικοποίησης εργασία στο έργο. 
- Καθορίστε το εισαγωγής περιουσιακού στοιχείου για κωδικοποίηση.
- Δημιουργία ενός περιουσιακού στοιχείου εξόδου που θα περιέχουν τα κωδικοποιημένα περιουσιακών στοιχείων.
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
        
        namespace EncodeAndGenerateThumbnails
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
        
                    // Encode and generate the thumbnails.
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
                    string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");
                
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

##<a id="json"></a>Μικρογραφία υποδείγματος JSON

Για πληροφορίες σχετικά με το σχήμα, ανατρέξτε [σε αυτό](https://msdn.microsoft.com/library/mt269962.aspx) το θέμα.

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


##<a id="xml"></a>Μικρογραφία υποδείγματος XML

Για πληροφορίες σχετικά με το σχήμα, ανατρέξτε [σε αυτό](https://msdn.microsoft.com/library/mt269962.aspx) το θέμα.
    
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

##<a name="considerations"></a>Ζητήματα

Ισχύουν τα ακόλουθα θέματα:

- Τη χρήση ρητή χρονικές σημάνσεις για έναρξη/βήμα/περιοχή προϋποθέτει ότι η προέλευση εισόδου είναι πολύ τουλάχιστον 1 λεπτό.
- Png/jpg/BmpImage στοιχεία έχουν έναρξης, βήμα και περιοχή συμβολοσειράς χαρακτηριστικά – αυτές μπορεί να θεωρηθούν:

    - Αριθμός πλαισίου εάν είναι ακέραιοι αριθμοί μη αρνητικός, π.χ. "Εκκίνηση": "120",
    - Σε σχέση με τη διάρκεια προέλευσης Εάν εκφρασμένη % επίθημα, π.χ. "Εκκίνηση": "15%", ή
    - Χρονική σήμανση εάν εκφρασμένη HH:MM:SS... μορφή. Π.χ. "Εκκίνηση": "00: 01:00"

    Μπορείτε να συνδυάσετε και να ταιριάζει επικοινωνήστε με σημειογραφίας κατά τη.
    
    Επιπλέον, Έναρξη υποστηρίζει επίσης μια ειδική μακροεντολή: {βέλτιστη}, που προσπαθεί να προσδιορίσετε το πρώτο καρέ "ενδιαφέρον" τη σημείωση περιεχομένου: (βήμα και περιοχή λαμβάνονται υπόψη κατά την έναρξη έχει οριστεί σε {καλύτερα})
    
    - Προεπιλογών: Έναρξη: {βέλτιστη}
- Μορφή εξόδου πρέπει να παρέχεται ρητά για κάθε μορφή εικόνας: Jpg/Png BmpFormat. Κατά την παρουσίαση, MES θα ταιριάζουν με JpgVideo σε JpgFormat και ούτω καθεξής. OutputFormat παρουσιάζει μια νέα μακροεντολή συγκεκριμένη εικόνα κωδικοποιητή: {Index}, ποια πρέπει να είναι παρουσίαση (μία φορά και μόνο μία φορά) για τις μορφές εξόδου εικόνας.


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης 

[Επισκόπηση κωδικοποίησης υπηρεσίες πολυμέσων](media-services-encode-asset.md)

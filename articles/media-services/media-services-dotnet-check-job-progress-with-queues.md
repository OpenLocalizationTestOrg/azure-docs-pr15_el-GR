<properties 
    pageTitle="Χρήση ουρά Azure χώρου αποθήκευσης για την παρακολούθηση ειδοποιήσεων σχετικά με τις υπηρεσίες πολυμέσων εργασίες με το .NET | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε ουρά Azure χώρου αποθήκευσης για την παρακολούθηση της ειδοποιήσεων σχετικά με τις υπηρεσίες πολυμέσων εργασίες. Το δείγμα κώδικα είναι γραμμένο σε C# και χρησιμοποιεί το Media Services SDK για .NET." 
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
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Χρήση ουρά Azure χώρου αποθήκευσης για την παρακολούθηση ειδοποιήσεων σχετικά με τις υπηρεσίες πολυμέσων εργασίες με .NET

Όταν εκτελείτε εργασίες, συχνά απαιτούν ένας τρόπος για να παρακολουθείτε την πρόοδο του έργου. Μπορείτε να ελέγξετε την πρόοδο χρησιμοποιώντας ουρά Azure χώρου αποθήκευσης για την παρακολούθηση ειδοποιήσεων σχετικά με εργασίες Media Services (όπως περιγράφεται σε αυτό το θέμα) ή τον ορισμό ενός προγράμματος χειρισμού συμβάντων StateChanged (όπως περιγράφεται σε [αυτό](media-services-check-job-progress.md) το θέμα.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Χρήση ουρά Azure χώρου αποθήκευσης για την παρακολούθηση ειδοποιήσεων σχετικά με τις υπηρεσίες πολυμέσων εργασίες

Microsoft Azure Media Services έχει τη δυνατότητα για την παράδοση μηνυμάτων ειδοποίησης για την [Αποθήκευση Azure ουράς](../storage-dotnet-how-to-use-queues.md#what-is) κατά την επεξεργασία εργασίες πολυμέσων. Αυτό το θέμα δείχνει πώς μπορείτε να λάβετε αυτά τα μηνύματα ειδοποιήσεων από το χώρο αποθήκευσης ουρά.

Μηνύματα σε ουρά χώρου αποθήκευσης είναι δυνατή η πρόσβαση από οπουδήποτε στον κόσμο. Η ουρά Azure μηνυμάτων αρχιτεκτονική είναι αξιόπιστη και ιδιαίτερα μεταβλητού μεγέθους. Σταθμοσκόπησης αποθήκευσης ουρά συνιστάται σε σχέση με άλλες μεθόδους. 

Ένα συνηθισμένο σενάριο για την ακρόαση ειδοποιήσεις Media Services είναι εάν σχεδιάζετε ένα σύστημα διαχείρισης περιεχομένου που πρέπει να εκτελέσετε κάποια επιπλέον εργασία μετά από μια εργασία κωδικοποίησης ολοκληρώνει (για παράδειγμα, έναυσμα, το επόμενο βήμα σε μια ροή εργασίας ή δημοσίευση περιεχομένου). 

###<a name="considerations"></a>Ζητήματα

Λάβετε υπόψη τα παρακάτω κατά την ανάπτυξη εφαρμογών Media Services που χρησιμοποιούν ουρά Azure χώρου αποθήκευσης.

- Η υπηρεσία ουρές δεν παρέχει εγγύηση της πρώτης-σε-first-out (FIFO) παραγγελθεί παράδοσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ουρές Azure και Azure Service Bus ουρές σύγκριση και Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure ουρές χώρου αποθήκευσης δεν είναι μια υπηρεσία push; πρέπει να ψηφοφορία με ουρά. 
- Μπορείτε να έχετε οποιονδήποτε αριθμό ουρές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [REST API υπηρεσίας ουράς](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure αποθήκευσης ουρές έχει ορισμένους περιορισμούς και συγκεκριμένες απαιτήσεις που περιγράφονται στο ακόλουθο άρθρο: [ουρές Azure και Azure Service Bus ουρές σύγκριση και Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Παράδειγμα κώδικα

Το παράδειγμα κώδικα σε αυτήν την ενότητα κάνει τα εξής:

1. Καθορίζει την κλάση **EncodingJobMessage** που αντιστοιχεί στη μορφή μηνύματος ειδοποίησης. Ο κώδικας deserializes μηνυμάτων που έχουν ληφθεί από την ουρά σε αντικείμενα του τύπου **EncodingJobMessage** .
1. Φορτώνει τις πληροφορίες λογαριασμού υπηρεσίες πολυμέσων και την αποθήκευση από το αρχείο app.config. Χρησιμοποιεί αυτές τις πληροφορίες για να δημιουργήσετε τα αντικείμενα **CloudMediaContext** και **CloudQueue** .
1. Δημιουργεί ουρά που λαμβάνει μηνύματα ειδοποιήσεων σχετικά με την εργασία κωδικοποίησης.
1. Δημιουργεί το τελικό σημείο ειδοποιήσεων που έχει αντιστοιχιστεί σε ουρά.
1. Επισυνάπτει το τελικό σημείο ειδοποίησης για την εργασία και το υποβάλλει τη κωδικοποίησης εργασία. Μπορείτε να έχετε πολλούς ειδοποίηση τελικά σημεία που έχουν επισυναφθεί σε μια εργασία.
1. Σε αυτό το παράδειγμα, θα σας σας ενδιαφέρει μόνο τελικό μέλη για την επεξεργασία εργασίας, ώστε να μας μεταβιβάζουν **NotificationJobState.FinalStatesOnly** για τη μέθοδο **AddNew** . 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Εάν περάσετε NotificationJobState.All, θα πρέπει να περιμένουν για να λάβετε όλες τις ειδοποιήσεις αλλαγής κατάστασης: στην ουρά -> προγραμματισμένη -> Επεξεργασία -> ολοκληρώθηκε. Ωστόσο, όπως σημειώθηκε προηγούμενα, την υπηρεσία ουρές αποθήκευσης Azure δεν εγγυάται ταξινομημένη παράδοσης. Μπορείτε να χρησιμοποιήσετε την ιδιότητα χρονικής σήμανσης (που καθορίζονται από τον τύπο EncodingJobMessage στο παρακάτω παράδειγμα) σε σειρά μηνύματα. Είναι πιθανό ότι λαμβάνετε μηνύματα ειδοποιήσεων διπλότυπων. Χρησιμοποιήστε την ιδιότητα ETag (που καθορίζονται από τον τύπο EncodingJobMessage) για να ελέγξετε αν υπάρχουν διπλότυπα. Επίσης, είναι πιθανό ότι θα παραλειφθούν ορισμένες ειδοποιήσεις αλλαγής κατάστασης. 
1. Χρειάζεται να περιμένει για το έργο για να μεταβείτε σε την ολοκληρωμένη κατάσταση, επιλέγοντας ουρά κάθε 10 δευτερόλεπτα. Διαγράφει τα μηνύματα, αφού έχει γίνει η επεξεργασία.
1. Διαγράφει στην ουρά και το τελικό σημείο ειδοποίησης.

>[AZURE.NOTE]Το συνιστώμενο τρόπο για την παρακολούθηση της κατάστασης μιας εργασίας είναι να ακρόαση μηνυμάτων ειδοποίησης, όπως φαίνεται στο παρακάτω παράδειγμα.
>
>Εναλλακτικά, μπορεί να ελέγξετε στην κατάσταση μιας εργασίας χρησιμοποιώντας την ιδιότητα **IJob.State** .  Ένα μήνυμα ειδοποίησης σχετικά με την ολοκλήρωση μιας εργασίας μπορεί να φτάσουν πριν από την κατάσταση στην **IJob** έχει οριστεί σε **Ολοκληρωμένη**. Η ιδιότητα **IJob.State** απεικονίζει την ακριβή κατάσταση με μια μικρή καθυστέρηση.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

Το παραπάνω παράδειγμα παραχθεί το παρακάτω αποτέλεσμα. Μπορείτε τιμές θα διαφέρουν.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<properties 
    pageTitle="Σταθμοσκόπησης μεγάλη διάρκεια εκτέλεσης λειτουργιών | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να ψηφοφορία με μεγάλη διάρκεια εκτέλεσης λειτουργιών." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Παράδοση ζωντανή ροή με τις υπηρεσίες πολυμέσων Azure

##<a name="overview"></a>Επισκόπηση

Υπηρεσίες πολυμέσων Azure Microsoft προσφέρει API που αποστολή προσκλήσεων σε υπηρεσίες πολυμέσων για να ξεκινήσετε λειτουργίες (για παράδειγμα: δημιουργία, Έναρξη, διακοπή ή διαγραφή καναλιού). Αυτές οι λειτουργίες είναι μεγάλη διάρκεια εκτέλεσης.

Το .NET SDK υπηρεσίες πολυμέσων παρέχει API που στείλτε την πρόσκληση και περιμένετε να ολοκληρωθεί η λειτουργία (εσωτερικά, τα API σταθμοσκόπησης για την πρόοδο της λειτουργίας σε ορισμένα χρονικά διαστήματα). Για παράδειγμα, όταν καλείτε το κανάλι. Start(), η μέθοδος επιστρέφει μετά την εκκίνηση του καναλιού. Μπορείτε επίσης να χρησιμοποιήσετε την έκδοση ασύγχρονης: αναμένει καναλιού. StartAsync() (για πληροφορίες σχετικά με βάσει εργασίας ασύγχρονης μοτίβο, ανατρέξτε στο θέμα [ΠΑΤΉΣΤΕ](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API που στέλνετε μια πρόσκληση σε λειτουργία και, στη συνέχεια, τις ψηφοφορίας για την κατάσταση μέχρι να ολοκληρωθεί η λειτουργία ονομάζονται "σταθμοσκόπησης μεθόδους". Συνιστάται η τις παρακάτω μεθόδους (ειδικά την έκδοση ασύγχρονης) για εμπλουτισμένο πρόγραμμα-πελάτη εφαρμογές ή/και τις υπηρεσίες με κατάσταση.

Υπάρχουν σενάρια όπου μια εφαρμογή δεν είναι δυνατό να περιμένετε για μια αίτηση http μεγάλης διάρκειας και θέλει να ψηφοφορία για την πρόοδο της λειτουργίας με μη αυτόματο τρόπο. Ένα τυπικό παράδειγμα θα είναι ένα πρόγραμμα περιήγησης αλληλεπίδραση με μια υπηρεσία web χωρίς κατάσταση: όταν το πρόγραμμα περιήγησης ζητά να δημιουργήσετε ένα κανάλι, την υπηρεσία web προετοιμάζει μια λειτουργία μεγάλης διάρκειας και επιστρέφει το Αναγνωριστικό λειτουργίας στο πρόγραμμα περιήγησης. Το πρόγραμμα περιήγησης, στη συνέχεια, θα μπορούσε να ζητήστε από την υπηρεσία web για να λάβετε την κατάσταση λειτουργίας με βάση το αναγνωριστικό. Το .NET SDK υπηρεσίες πολυμέσων παρέχει API που είναι χρήσιμες για αυτό το σενάριο. Αυτά τα API ονομάζονται "μη σταθμοσκόπησης μεθόδους".
Το παρακάτω μοτίβο ονομασίας έχουν τις μεθόδους"μη σταθμοσκόπησης": Αποστολή*OperationName*λειτουργία (για παράδειγμα, SendCreateOperation). Αποστολή*OperationName*λειτουργία μέθοδοι επιστρέφουν του αντικειμένου **IOperation** . το αντικείμενο που επιστρέφεται περιέχει πληροφορίες που μπορούν να χρησιμοποιηθούν για την παρακολούθηση της λειτουργίας. Επιστρέφει τις μεθόδους OperationAsync*OperationName*Αποστολή **εργασίας<IOperation>**.

Προς το παρόν, οι παρακάτω κλάσεις υποστηρίζουν μη σταθμοσκόπησης μεθόδους: **καναλιού**, **StreamingEndpoint**και **πρόγραμμα**.

Για να τις ψηφοφορίας για την κατάσταση λειτουργίας, χρησιμοποιήστε τη μέθοδο **GetOperation** στην τάξη **OperationBaseCollection** . Χρησιμοποιήστε τα παρακάτω χρονικά διαστήματα για να ελέγξετε την κατάσταση λειτουργίας: για το **κανάλι** και **StreamingEndpoint** λειτουργίες, χρήση 30 δευτερόλεπτα; για λειτουργίες **προγράμματος** , χρησιμοποιήστε 10 δευτερόλεπτα.


##<a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα καθορίζει κλάσης που ονομάζεται **ChannelOperations**. Αυτός ο ορισμός κλάσης μπορεί να είναι ένα σημείο εκκίνησης για τον ορισμό κλάσης υπηρεσίας web. Για λόγους ευκολίας, τα παρακάτω παραδείγματα χρησιμοποιούν τις μη ασύγχρονης εκδόσεις του μεθόδους.

Το παράδειγμα εμφανίζει επίσης πώς ο υπολογιστής-πελάτης να χρησιμοποιήσετε αυτήν την κλάση.

###<a name="channeloperations-class-definition"></a>Ορισμός κλάσης ChannelOperations

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
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
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Ο κωδικός προγράμματος-πελάτη

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<properties 
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε χώρο αποθήκευσης Azure ουρά με το SDK WebJobs. Δημιουργία και διαγραφή ουρές; Εισαγωγή, Ρίξτε μια ματιά, λήψη και διαγραφή ουρά μηνυμάτων και πολλά άλλα." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός παρέχει C# δείγματα κώδικα που σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε την έκδοση Azure WebJobs SDK 1.x με την υπηρεσία ουράς Azure χώρου αποθήκευσης.

Ο οδηγός προϋποθέτει ότι γνωρίζετε [πώς μπορείτε να δημιουργήσετε ένα έργο WebJob στο Visual Studio με συμβολοσειρές σύνδεσης που οδηγούν στο λογαριασμό σας χώρο αποθήκευσης](websites-dotnet-webjobs-sdk-get-started.md#configure-storage) ή σε [πολλούς λογαριασμούς χώρου αποθήκευσης](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Περισσότερα από τα τμήματα κώδικα Εμφάνιση μόνο των συναρτήσεων, δεν τον κώδικα που δημιουργεί το `JobHost` αντικείμενο όπως αυτό το παράδειγμα:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }
        
Ο οδηγός περιλαμβάνει τα παρακάτω θέματα:

-   [Τον τρόπο ενεργοποίησης μιας συνάρτησης όταν λαμβάνετε ένα μήνυμα Ουράς](#trigger)
    - Συμβολοσειρά ουρά μηνυμάτων
    - POCO ουρά μηνυμάτων
    - Ασύγχρονων συναρτήσεων
    - Το χαρακτηριστικό QueueTrigger λειτουργεί με τύπους
    - Ο αλγόριθμος ανίχνευσης
    - Πολλές παρουσίες
    - Παράλληλη εκτέλεση
    - Λήψη ουρά ή ουρά μετα-δεδομένων μηνύματος
    - Φυσιολογική τερματισμού
-   [Πώς μπορείτε να δημιουργήσετε ένα μήνυμα ουράς κατά την επεξεργασία ενός μηνύματος ουρά](#createqueue)
    - Συμβολοσειρά ουρά μηνυμάτων
    - POCO ουρά μηνυμάτων
    - Δημιουργία πολλών μηνυμάτων ή στο ασύγχρονων συναρτήσεων
    - Το χαρακτηριστικό ουρά λειτουργεί με τύπους
    - Χρήση του WebJobs SDK χαρακτηριστικά στο σώμα μιας συνάρτησης
-   [Πώς να διαβάζετε και να γράφετε αντικείμενα BLOB κατά την επεξεργασία ενός μηνύματος ουρά](#blobs)
    - Συμβολοσειρά ουρά μηνυμάτων
    - POCO ουρά μηνυμάτων
    - Το χαρακτηριστικό Blob λειτουργεί με τύπους
-   [Τον τρόπο χειρισμού των μηνυμάτων αλλοίωσης](#poison)
    - Αυτόματη τον χειρισμό μηνυμάτων
    - Μη αυτόματη τον χειρισμό μηνυμάτων
-   [Πώς μπορείτε να ορίσετε επιλογές ρύθμισης παραμέτρων](#config)
    - Ορισμός SDK συμβολοσειρές σύνδεσης σε κώδικα
    - Ρύθμιση παραμέτρων QueueTrigger
    - Ορισμός τιμών για WebJobs SDK παραμέτρους κατασκευή στον κώδικα
-   [Τον τρόπο ενεργοποίησης μιας συνάρτησης με μη αυτόματο τρόπο](#manual)
-   [Πώς να συντάξετε αρχείων καταγραφής](#logs) 
-   [Πώς να χειριστείτε σφάλματα και να ρυθμίσετε τις παραμέτρους χρονικών ορίων](#errors)
-   [Επόμενα βήματα](#nextsteps)

## <a id="trigger"></a>Τον τρόπο ενεργοποίησης μιας συνάρτησης όταν λαμβάνετε ένα μήνυμα Ουράς

Για να γράψετε μια συνάρτηση που καλεί το SDK WebJobs όταν λαμβάνετε ένα μήνυμα ουρά, χρησιμοποιήστε το `QueueTrigger` χαρακτηριστικό. Η κατασκευή χαρακτηριστικού μεταφέρει παραμέτρου συμβολοσειράς που καθορίζει το όνομα της ουράς προς απάντηση. Μπορείτε επίσης να [ρυθμίσετε το όνομα ουράς δυναμικά](#config).

### <a name="string-queue-messages"></a>Συμβολοσειρά ουρά μηνυμάτων

Στο παρακάτω παράδειγμα, η ουρά περιέχει ένα μήνυμα συμβολοσειρά, επομένως `QueueTrigger` εφαρμόζεται σε μια παράμετρο συμβολοσειράς που ονομάζεται `logMessage` που περιέχει το περιεχόμενο του μηνύματος ουρά. Η συνάρτηση [συντάσσει ένα μήνυμα καταγραφής στον πίνακα εργαλείων](#logs).
 

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Εκτός από `string`, η παράμετρος μπορεί να είναι ένας πίνακας byte, μια `CloudQueueMessage` αντικείμενο ή μια POCO που έχετε καθορίσει.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(απλό παλιό αντικείμενο CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) ουρά μηνυμάτων

Στο παρακάτω παράδειγμα, το μήνυμα ουρά περιέχει JSON για μια `BlobInformation` αντικείμενο που περιλαμβάνει μια `BlobName` την ιδιότητα. Το SDK deserializes αυτόματα το αντικείμενο.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Το SDK χρησιμοποιεί το [πακέτο Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) σειριοποίηση και αποσειριοποίηση μηνύματα. Εάν δημιουργήσετε ουρά μηνυμάτων σε ένα πρόγραμμα που δεν χρησιμοποιεί το SDK WebJobs, μπορείτε να συντάξετε κώδικα όπως το παρακάτω παράδειγμα για να δημιουργήσετε ένα μήνυμα ουρά POCO τη δυνατότητα ανάλυσης στο SDK. 

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Ασύγχρονων συναρτήσεων

Το παρακάτω ασύγχρονης συνάρτηση [συντάσσει ένα αρχείο καταγραφής στον πίνακα εργαλείων](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Συναρτήσεις ασύγχρονης ενδέχεται να χρειαστούν ένα [διακριτικό ακύρωσης](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), όπως φαίνεται στο παρακάτω παράδειγμα που αντιγράφει ένα blob. (Για επεξήγηση της το `queueTrigger` πλαίσιο κράτησης θέσης, ανατρέξτε στην ενότητα [αντικείμενα BLOB](#blobs) .)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Το χαρακτηριστικό QueueTrigger λειτουργεί με τύπους

Μπορείτε να χρησιμοποιήσετε `QueueTrigger` με τους ακόλουθους τύπους:

* `string`
* Ένας τύπος POCO σειριοποιηθεί ως JSON
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Ο αλγόριθμος ανίχνευσης

Το SDK εφαρμόζει έναν τυχαίο εκθετική πίσω απενεργοποίηση αλγόριθμο για να μειώσετε το αποτέλεσμα της αδράνειας-ουρά σταθμοσκόπησης στο κόστος συναλλαγής χώρου αποθήκευσης.  Όταν βρεθεί ένα μήνυμα, το SDK αναμένει δύο δευτερόλεπτα και, στη συνέχεια, έλεγχος για ένα άλλο μήνυμα; Όταν υπάρχει κανένα μήνυμα αναμένει σχετικά με τέσσερα δευτερόλεπτα πριν να δοκιμάσετε ξανά. Αφού οι επόμενες προσπάθειες απέτυχε να λάβετε ένα μήνυμα ουρά, ο χρόνος αναμονής εξακολουθεί να αυξήσετε μέχρι να φτάσει η μέγιστος χρόνος αναμονής, δίνοντας ως προεπιλογή για ένα λεπτό. [Ο χρόνος μέγιστο αναμονής είναι δυνατό να ρυθμιστεί](#config).

### <a id="instances"></a>Πολλές παρουσίες

Εάν η εφαρμογή web της εκτελείται σε πολλές παρουσίες, μια συνεχόμενη WebJob εκτελείται σε κάθε υπολογιστή και κάθε υπολογιστή θα περιμένετε για εναύσματα και προσπαθήστε να εκτελέσετε λειτουργίες. Το έναυσμα ουρά WebJobs SDK αποτρέπει αυτόματα μια συνάρτηση από την επεξεργασία ενός μηνύματος ουρά πολλές φορές. συναρτήσεις δεν χρειάζεται να εγγραφούν σε είναι idempotent. Ωστόσο, εάν θέλετε να διασφαλίσετε την μόνο μία παρουσία του μιας συνάρτησης εκτελείται ακόμη και όταν υπάρχουν πολλές παρουσίες του κεντρικού υπολογιστή web app, μπορείτε να χρησιμοποιήσετε το `Singleton` χαρακτηριστικό. 

### <a id="parallel"></a>Παράλληλη εκτέλεση

Εάν έχετε πολλές συναρτήσεις ακρόαση σε διαφορετικές ουρές, το SDK θα τους καλέσει παράλληλα όταν γίνεται λήψη μηνυμάτων ταυτόχρονα. 

Το ίδιο ισχύει όταν πολλά μηνύματα λαμβάνονται για μία μόνο ουρά. Από προεπιλογή, το SDK λαμβάνει μια δέσμη 16 ουρά μηνυμάτων κάθε φορά και εκτελεί τη συνάρτηση που επεξεργάζεται τους παράλληλα. [Το μέγεθος δέσμης είναι δυνατό να ρυθμιστεί](#config). Όταν ο αριθμός που υποβάλλεται σε επεξεργασία λαμβάνει προς τα κάτω στο μισό του μεγέθους της δέσμης, το SDK λαμβάνει μια άλλη δέσμη και ξεκινά επεξεργασίας αυτά τα μηνύματα. Επομένως, ο μέγιστος αριθμός των ταυτόχρονες μηνυμάτων που υποβάλλεται σε επεξεργασία ανά συνάρτηση είναι μισή ώρα το μέγεθος δέσμης. Αυτό το όριο ισχύει ξεχωριστά για κάθε συνάρτηση που έχει μια `QueueTrigger` χαρακτηριστικό. 

Εάν δεν θέλετε παράλληλη εκτέλεση για τα μηνύματα που λάβατε στο μία ουρά, μπορείτε να ορίσετε το μέγεθος δέσμης σε 1. Δείτε επίσης **περισσότερο έλεγχο στην ουρά επεξεργασίας** στο [Azure WebJobs SDK 1.1.0 RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Λήψη ουρά ή ουρά μετα-δεδομένων μηνύματος

Μπορείτε να λάβετε τις ακόλουθες ιδιότητες μηνύματος, προσθέτοντας τις παραμέτρους της υπογραφής μέθοδο:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (περιέχει το κείμενο του μηνύματος)
* `string`αναγνωριστικό
* `string`popReceipt
* `int`dequeueCount

Εάν θέλετε να εργαστείτε απευθείας με την αποθήκευση Azure API, μπορείτε επίσης να προσθέσετε μια `CloudStorageAccount` παραμέτρου.

Το παρακάτω παράδειγμα εγγράφει όλων των αυτό μετα-δεδομένων σε ένα αρχείο καταγραφής της εφαρμογής ΠΛΗΡΟΦΟΡΊΕΣ. Στο παράδειγμα, logMessage και queueTrigger περιέχει το περιεχόμενο του μηνύματος ουρά.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Ακολουθεί ένα δείγμα αρχείου καταγραφής από το δείγμα κώδικα:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <a id="graceful"></a>Φυσιολογική τερματισμού

Μια συνάρτηση που εκτελείται σε μια συνεχόμενη WebJob μπορεί να αποδεχτεί μια `CancellationToken` παράμετρο που επιτρέπει στο λειτουργικό σύστημα για να ενημερώσετε τη συνάρτηση όταν το WebJob πρόκειται να τερματιστεί. Μπορείτε να χρησιμοποιήσετε αυτήν την ειδοποίηση για να βεβαιωθείτε ότι η συνάρτηση δεν μη αναμενόμενο τερματισμό με τον τρόπο που απομένουν δεδομένα σε μια συνεπή κατάσταση.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για να ελέγξετε για επικείμενη WebJob τερματισμού σε μια συνάρτηση.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Σημείωση:** Ο πίνακας εργαλείων δεν μπορεί να εμφανίζει σωστά την κατάσταση και το αποτέλεσμα των συναρτήσεων που έχουν τερματιστεί.
 
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [WebJobs φυσιολογική τερματισμού](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Πώς μπορείτε να δημιουργήσετε ένα μήνυμα ουράς κατά την επεξεργασία ενός μηνύματος ουρά

Για να γράψετε μια συνάρτηση που δημιουργεί ένα νέο μήνυμα ουρά, χρησιμοποιήστε το `Queue` χαρακτηριστικό. Όπως `QueueTrigger`, που μεταβιβάζουν το όνομα ουρά ως συμβολοσειρά ή μπορείτε να [ορίσετε το όνομα ουράς δυναμικά](#config).

### <a name="string-queue-messages"></a>Συμβολοσειρά ουρά μηνυμάτων

Το παρακάτω δείγμα κώδικα μη ασύγχρονης δημιουργεί ένα νέο μήνυμα ουρά στην ουρά με το όνομα "outputqueue" με το ίδιο περιεχόμενο με το μήνυμα ουρά λάβατε στην ουρά με το όνομα "inputqueue". (Για ασύγχρονη Χρησιμοποιήστε συναρτήσεις `IAsyncCollector<T>` όπως φαίνεται αργότερα σε αυτήν την ενότητα.)


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }
  
### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(απλό παλιό αντικείμενο CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) ουρά μηνυμάτων

Για να δημιουργήσετε ένα μήνυμα ουρά που περιέχει ένα POCO αντί για μια συμβολοσειρά, μεταβιβάζουν τον τύπο POCO ως παράμετρο εξόδου για να το `Queue` κατασκευή χαρακτηριστικού.
 
        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Το SDK τοποθετεί αυτόματα σειριακά να JSON το αντικείμενο. Ένα μήνυμα ουρά δημιουργείται πάντα, ακόμα και αν το αντικείμενο είναι null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Δημιουργία πολλών μηνυμάτων ή στο ασύγχρονων συναρτήσεων

Για να δημιουργήσετε πολλά μηνύματα, να τον τύπο παραμέτρου για την ουρά εξόδου `ICollector<T>` ή `IAsyncCollector<T>`, όπως φαίνεται στο παρακάτω παράδειγμα.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Κάθε μήνυμα ουρά δημιουργείται αμέσως όταν το `Add` μέθοδος καλείται.

### <a name="types-that-the-queue-attribute-works-with"></a>Τύποι που λειτουργεί το χαρακτηριστικό ουρά με

Μπορείτε να χρησιμοποιήσετε το `Queue` χαρακτηριστικό σχετικά με τους παρακάτω τύπους παραμέτρων:

* `out string`(δημιουργεί μήνυμα ουρά εάν η τιμή της παραμέτρου είναι δεν είναι null όταν λήξει η συνάρτηση)
* `out byte[]`(λειτουργεί ως `string`) 
* `out CloudQueueMessage`(λειτουργεί ως `string`) 
* `out POCO`(ένας τύπος μπορεί να σειριοποιηθεί, δημιουργεί ένα μήνυμα με αντικείμενο null εάν η παράμετρος είναι null όταν λήξει η συνάρτηση)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(για τη δημιουργία μηνυμάτων με μη αυτόματο τρόπο χρησιμοποιώντας το API αποθήκευσης Azure απευθείας)

### <a id="ibinder"></a>Χρήση του WebJobs SDK χαρακτηριστικά στο σώμα μιας συνάρτησης

Εάν πρέπει να κάνετε ορισμένες εργασίες στη συνάρτηση σας πριν να χρησιμοποιήσετε ένα χαρακτηριστικό WebJobs SDK όπως `Queue`, `Blob`, ή `Table`, μπορείτε να χρησιμοποιήσετε το `IBinder` περιβάλλοντος εργασίας.

Το παρακάτω παράδειγμα λαμβάνει μήνυμα ουρά εισόδου και δημιουργεί ένα νέο μήνυμα με το ίδιο περιεχόμενο σε μια ουρά εξόδου. Το όνομα ουράς εξόδου έχει οριστεί από τον κωδικό στο σώμα της συνάρτησης.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Το `IBinder` περιβάλλοντος εργασίας μπορούν επίσης να χρησιμοποιηθούν με το `Table` και `Blob` χαρακτηριστικά.

## <a id="blobs"></a>Πώς να διαβάζετε και να γράφετε αντικείμενα BLOB και πινάκων κατά την επεξεργασία ενός μηνύματος ουρά

Το `Blob` και `Table` χαρακτηριστικά σάς επιτρέπουν να διαβάζετε και να γράφετε αντικείμενα BLOB και πίνακες. Τα δείγματα σε αυτήν την ενότητα εφαρμόζονται σε αντικείμενα blob. Για δείγματα κώδικα που δείχνουν τον τρόπο ενεργοποίησης διεργασίες όταν αντικείμενα BLOB έχουν δημιουργηθεί ή έχουν ενημερωθεί, δείτε [πώς μπορείτε να χρησιμοποιήσετε χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)και για τα δείγματα κώδικα που ανάγνωση και εγγραφή πίνακες, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης πινάκων του Azure με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Συμβολοσειρά ουρά μηνυμάτων ενεργοποίηση λειτουργιών blob

Για ένα μήνυμα ουρά που περιέχει μια συμβολοσειρά, `queueTrigger` είναι ένα σύμβολο κράτησης θέσης που μπορείτε να χρησιμοποιήσετε με το `Blob` του χαρακτηριστικού `blobPath` παράμετρο που περιέχει τα περιεχόμενα του μηνύματος. 

Το ακόλουθο παράδειγμα χρησιμοποιεί `Stream` αντικειμένων για ανάγνωση και εγγραφή αντικείμενα blob. Το μήνυμα ουρά είναι το όνομα του ένα blob που βρίσκεται στο κοντέινερ textblobs. Ένα αντίγραφο του το αντικείμενο blob με "-νέα" προσαρτημένο σε το όνομα που έχει δημιουργηθεί στο ίδιο κοντέινερ. 

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Το `Blob` χαρακτηριστικών παίρνει κατασκευή ενός `blobPath` παράμετρος που καθορίζει το όνομα του κοντέινερ και αντικειμένων blob. Για περισσότερες πληροφορίες σχετικά με αυτήν τη θέση αντικειμένου, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), 

Όταν το χαρακτηριστικό decorates ένα `Stream` αντικείμενο, μια άλλη παράμετρος κατασκευή Καθορίζει το `FileAccess` λειτουργία ως ανάγνωση, εγγραφή ή μόνο για ανάγνωση. 

Το ακόλουθο παράδειγμα χρησιμοποιεί μια `CloudBlockBlob` αντικειμένου για να διαγράψετε ένα blob. Το μήνυμα ουρά είναι το όνομα του το αντικείμενο blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(απλό παλιό αντικείμενο CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) ουρά μηνυμάτων

Για μια POCO που έχουν αποθηκευτεί ως JSON στο μήνυμα ουρά, μπορείτε να χρησιμοποιήσετε χαρακτήρες κράτησης θέσης που ιδιοτήτων του αντικειμένου στο όνομα του `Queue` του χαρακτηριστικού `blobPath` παραμέτρου. Μπορείτε επίσης να χρησιμοποιήσετε [τα ονόματα των ιδιοτήτων μετα-δεδομένων ουρά](#queuemetadata) ως σύμβολα κράτησης θέσης. 

Το παρακάτω παράδειγμα αντιγράφει ένα blob σε ένα νέο blob με διαφορετική επέκταση. Το μήνυμα ουρά είναι μια `BlobInformation` αντικείμενο που περιλαμβάνει το `BlobName` και `BlobNameWithoutExtension` ιδιότητες. Τα ονόματα των ιδιοτήτων που χρησιμοποιούνται ως σύμβολα κράτησης θέσης στη διαδρομή blob για την `Blob` χαρακτηριστικά. 
 
        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Το SDK χρησιμοποιεί το [πακέτο Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) σειριοποίηση και αποσειριοποίηση μηνύματα. Εάν δημιουργήσετε ουρά μηνυμάτων σε ένα πρόγραμμα που δεν χρησιμοποιεί το SDK WebJobs, μπορείτε να συντάξετε κώδικα όπως το παρακάτω παράδειγμα για να δημιουργήσετε ένα μήνυμα ουρά POCO τη δυνατότητα ανάλυσης στο SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Εάν πρέπει να κάνετε ορισμένες εργασίες στη συνάρτηση σας πριν από την σύνδεση ένα blob σε ένα αντικείμενο, μπορείτε να χρησιμοποιήσετε το χαρακτηριστικό στο σώμα της συνάρτησης, [όπως φαίνεται παραπάνω για το χαρακτηριστικό ουρά](#ibinder).

### <a id="blobattributetypes"></a>Μπορείτε να χρησιμοποιήσετε το χαρακτηριστικό Blob με τύπους
 
Το `Blob` χαρακτηριστικό μπορεί να χρησιμοποιηθεί με τους ακόλουθους τύπους:

* `Stream`(ανάγνωσης ή εγγραφής, που καθορίζονται, χρησιμοποιώντας την παράμετρο κατασκευή FileAccess)
* `TextReader`
* `TextWriter`
* `string`(για ανάγνωση)
* `out string`(εγγραφή, δημιουργεί ένα blob μόνο εάν η παράμετρος συμβολοσειράς είναι δεν είναι null όταν η συνάρτηση επιστρέφει)
* POCO (για ανάγνωση)
* ανάληψη POCO (εγγραφή, πάντα δημιουργεί ένα blob, δημιουργεί ως αντικείμενο null εάν η παράμετρος POCO είναι null όταν η συνάρτηση επιστρέφει)
* `CloudBlobStream`(εγγραφή)
* `ICloudBlob`(ανάγνωσης ή εγγραφής)
* `CloudBlockBlob`(ανάγνωσης ή εγγραφής) 
* `CloudPageBlob`(ανάγνωσης ή εγγραφής) 

## <a id="poison"></a>Τον τρόπο χειρισμού των μηνυμάτων αλλοίωσης

Μηνύματα των οποίων το περιεχόμενο έχει ως αποτέλεσμα μια συνάρτηση αποτυχία ονομάζονται *μηνυμάτων αλλοίωσης*. Όταν η συνάρτηση αποτυγχάνει, το μήνυμα ουρά δεν διαγράφεται και τελικά είναι επιλέξατε προς τα επάνω και πάλι, προκαλεί τον κύκλο να επαναληφθεί. Το SDK αυτόματα να διακόψετε τον κύκλο μετά από έναν περιορισμένο αριθμό των διαδοχικών προσεγγίσεων ή μπορείτε να το κάνετε με μη αυτόματο τρόπο.

### <a name="automatic-poison-message-handling"></a>Αυτόματη τον χειρισμό μηνυμάτων

Το SDK θα καλέσετε μια συνάρτηση έως και 5 φορές η επεξεργασία ενός μηνύματος ουρά. Εάν το πέμπτο, δοκιμάστε να αποτύχει, το μήνυμα έχει μετακινηθεί σε ουρά αλλοίωσης. [Ο μέγιστος αριθμός των επαναλήψεων είναι δυνατό να ρυθμιστεί](#config). 

Αλλοίωσης ουρά ονομάζεται *{originalqueuename}*-αλλοίωσης. Μπορείτε να συντάξετε μια συνάρτηση για να επεξεργαστείτε μηνύματα από την αλλοίωσης ουρά με καταγραφή τους ή την αποστολή ειδοποίησης που μη αυτόματη προσοχή είναι απαραίτητο. 

Στο παρακάτω παράδειγμα το `CopyBlob` συνάρτηση θα αποτύχει όταν ένα μήνυμα ουρά περιέχει το όνομα του ένα blob που δεν υπάρχει. Όταν συμβεί αυτό, μετακινηθεί το μήνυμα από την ουρά copyblobqueue στην ουρά copyblobqueue δηλητήριο. Το `ProcessPoisonMessage` καταγράφει το μήνυμα αλλοίωσης.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }
        
        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Η παρακάτω εικόνα εμφανίζει το αποτέλεσμα της κονσόλας από αυτές τις συναρτήσεις κατά την επεξεργασία ενός μηνύματος αλλοίωσης.

![Κονσόλα εξόδου για τον χειρισμό μηνυμάτων](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Μη αυτόματη τον χειρισμό μηνυμάτων

Μπορείτε να λάβετε του αριθμού των φορών που έχει συλλεχθεί μηνύματος για επεξεργασία, προσθέτοντας μια `int` παράμετρο που ονομάζεται `dequeueCount` σας συνάρτηση. Στη συνέχεια, μπορείτε να ελέγξετε το πλήθος εξαγωγής από ουρά συνάρτηση κώδικα και να εκτελέσετε τη δική σας αλλοίωσης Διαχείριση μηνυμάτων όταν ο αριθμός υπερβαίνει το όριο, όπως φαίνεται στο παρακάτω παράδειγμα.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Πώς μπορείτε να ορίσετε επιλογές ρύθμισης παραμέτρων

Μπορείτε να χρησιμοποιήσετε το `JobHostConfiguration` τύπο για να ορίσετε τις ακόλουθες επιλογές ρύθμισης παραμέτρων:

* Ορίστε τις συμβολοσειρές σύνδεσης SDK στον κώδικα.
* Ρύθμιση παραμέτρων `QueueTrigger` ρυθμίσεις όπως η μέγιστη απομάκρυνση από την ουρά count.
* Λήψη ουρά ονομάτων από τις ρυθμίσεις παραμέτρων.

### <a id="setconnstr"></a>Ορισμός SDK συμβολοσειρές σύνδεσης σε κώδικα

Ρύθμιση των συμβολοσειρών σύνδεσης SDK στον κώδικα σάς επιτρέπει να χρησιμοποιήσετε τα δικά σας ονόματα συμβολοσειρά σύνδεσης σε αρχεία ρύθμισης παραμέτρων ή μεταβλητές περιβάλλοντος, όπως φαίνεται στο παρακάτω παράδειγμα.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;
        
            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;
        
            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;
        
            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="configqueue"></a>Ρύθμιση παραμέτρων QueueTrigger

Μπορείτε να ρυθμίσετε τις ακόλουθες ρυθμίσεις που ισχύουν για την επεξεργασία ουρά μηνυμάτων:

- Ο μέγιστος αριθμός ουρά μηνυμάτων που έχουν συλλεχθεί του ταυτόχρονα πρέπει να εκτελεστούν παράλληλα (η προεπιλογή είναι 16).
- Ο μέγιστος αριθμός των επαναλήψεων πριν από την αποστολή ενός μηνύματος ουρά σε ουρά αλλοίωσης (η προεπιλογή είναι 5).
- Ο μέγιστος αριθμός περιμένετε χρόνος μέχρι να σταθμοσκόπησης ξανά όταν μια ουρά είναι κενό (η προεπιλογή είναι 1 λεπτό).

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ρυθμίσετε αυτές τις ρυθμίσεις:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="setnamesincode"></a>Ορισμός τιμών για WebJobs SDK παραμέτρους κατασκευή στον κώδικα

Μερικές φορές θέλετε να καθορίσετε ένα όνομα ουράς, ένα όνομα blob ή κοντέινερ ή έναν πίνακα ονομάσετε στο κώδικα αντί να καθορίσετε από τον προγραμματισμό. Για παράδειγμα, μπορεί να θέλετε να καθορίσετε το όνομα ουράς για `QueueTrigger` σε αρχείο ή περιβάλλον μεταβλητή ρύθμισης παραμέτρων. 

Μπορείτε να το κάνετε περνώντας σε μια `NameResolver` αντικείμενο για να το `JobHostConfiguration` τύπου. Συμπεριλάβετε ειδικούς χαρακτήρες κράτησης θέσης περικλείεται σε σύμβολα ποσοστού (%) στο παραμέτρους κατασκευή χαρακτηριστικό WebJobs SDK, και το `NameResolver` κωδικός καθορίζει τις πραγματικές τιμές που θα χρησιμοποιηθεί στη θέση αυτά τα σύμβολα κράτησης θέσης.

Για παράδειγμα, ας υποθέσουμε ότι θέλετε να χρησιμοποιήσετε μια ουρά με το όνομα logqueuetest στο περιβάλλον δοκιμής και ένα επώνυμο logqueueprod παραγωγή. Αντί για ένα όνομα σχεδιασμένου ουράς, που θέλετε να καθορίσετε το όνομα μιας καταχώρησης στον το `appSettings` συλλογή που θα έχει το όνομα πραγματική ουράς. Εάν το `appSettings` αριθμού-κλειδιού είναι logqueue, η συνάρτηση σας μπορεί να μοιάζει με το παρακάτω παράδειγμα.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Σας `NameResolver` τάξης, στη συνέχεια, ενδέχεται να λάβετε το όνομα ουρά από `appSettings` όπως φαίνεται στο ακόλουθο παράδειγμα:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Περάσετε την `NameResolver` τάξης στο για να το `JobHost` αντικείμενο, όπως φαίνεται στο παρακάτω παράδειγμα.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }
 
**Σημείωση:** Ουρά, πίνακα και τα ονόματα αντικειμένων blob επιλύονται κάθε φορά που ονομάζεται μια συνάρτηση, αλλά τα ονόματα κοντέινερ αντικειμένων blob επιλύονται μόνο κατά την εκκίνηση της εφαρμογής. Δεν μπορείτε να αλλάξετε το όνομα του κοντέινερ αντικειμένων blob ενώ εκτελείται η εργασία. 

## <a id="manual"></a>Τον τρόπο ενεργοποίησης μιας συνάρτησης με μη αυτόματο τρόπο

Για να ενεργοποιήσετε μια συνάρτηση με μη αυτόματο τρόπο, χρησιμοποιήστε το `Call` ή `CallAsync` μέθοδος σε το `JobHost` αντικειμένου και του `NoAutomaticTrigger` χαρακτηριστικών για τη συνάρτηση, όπως φαίνεται στο παρακάτω παράδειγμα. 

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }
        
            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger, 
                string value, 
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a id="logs"></a>Πώς να συντάξετε αρχείων καταγραφής

Πίνακας εργαλείων εμφανίζει αρχεία καταγραφής σε δύο μέρη: στη σελίδα για την WebJob και, στη σελίδα για μια συγκεκριμένη κλήση WebJob. 

![Αρχεία καταγραφής στη σελίδα WebJob](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Αρχεία καταγραφής στη σελίδα κλήσης συνάρτησης](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Έξοδος από την κονσόλα μεθόδους που μπορείτε να καλέσετε σε μια συνάρτηση ή σε το `Main()` μέθοδο εμφανίζεται η σελίδα πίνακα εργαλείων για το WebJob και όχι της σελίδας για μια συγκεκριμένη μέθοδο κλήση. Έξοδος από το αντικείμενο TextWriter που λαμβάνετε από μια παράμετρο στην υπογραφή σας μέθοδο εμφανίζεται στη σελίδα πίνακα εργαλείων για μια κλήση μεθόδου.

Κονσόλα εξόδου δεν μπορεί να είναι συνδεδεμένη με μια συγκεκριμένη μέθοδο κλήση επειδή η κονσόλα είναι μονού νήματος, ενώ πολλές συναρτήσεις εργασία μπορεί να εκτελεί την ίδια στιγμή. Λόγο του SDK παρέχει κάθε συνάρτηση κλήσης με το δικό του αντικειμένου writer μοναδικό αρχείο καταγραφής.

Για να γράψετε [αρχεία καταγραφής ανίχνευσης εφαρμογής](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), χρησιμοποιήστε `Console.Out` (δημιουργεί αρχεία καταγραφής έχει επισημανθεί ως ΠΛΗΡΟΦΟΡΊΕΣ) και `Console.Error` (δημιουργεί αρχεία καταγραφής έχει επισημανθεί ως σφάλμα). Εναλλακτική λύση είναι να χρησιμοποιήσετε [ανίχνευσης ή TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), που παρέχει επίπεδα λεπτομερές, προειδοποίηση και κρίσιμη εκτός από τις πληροφορίες και σφάλματος. Αρχεία καταγραφής ανίχνευσης εφαρμογής που εμφανίζονται στα αρχεία καταγραφής εφαρμογών web, Azure πίνακες, ή αντικείμενα BLOB Azure ανάλογα με το πώς μπορείτε να ρυθμίσετε την εφαρμογή Azure web. Όπως ισχύει και για όλα τα δεδομένα εξόδου Console, τα πιο πρόσφατα αρχεία καταγραφής 100 εφαρμογής εμφανίζεται επίσης στη σελίδα πίνακα εργαλείων για το WebJob, όχι τη σελίδα για μια κλήση συνάρτησης. 

Κονσόλα εξόδου εμφανίζεται στον πίνακα εργαλείων μόνο εάν το πρόγραμμα εκτελείται σε ένα WebJob Azure, δεν εάν το πρόγραμμα εκτελείται τοπικά ή σε ορισμένες άλλες περιβάλλον.

Απενεργοποίηση της καταγραφής πίνακα εργαλείων για σενάρια υψηλής απόδοσης. Από προεπιλογή, το SDK εγγράφει αρχεία καταγραφής στο χώρο αποθήκευσης και αυτήν τη δραστηριότητα μπορεί να μειώσει τις επιδόσεις όταν επεξεργάζεστε πολλά μηνύματα. Για να απενεργοποιήσετε την καταγραφή, ορίστε τη συμβολοσειρά σύνδεσης πίνακα εργαλείων στην τιμή null όπως φαίνεται στο παρακάτω παράδειγμα.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

Το παρακάτω παράδειγμα εμφανίζει διάφορους τρόπους για να γράψετε αρχεία καταγραφής:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

Στον πίνακα εργαλείων SDK WebJobs, την έξοδο από το `TextWriter` αντικείμενο εμφανίζεται όταν μεταβείτε στη σελίδα για μια συγκεκριμένη συνάρτηση κλήσης και κάντε κλικ στο **Κουμπί εναλλαγής εξόδου**:

![Κάντε κλικ στην επιλογή σύνδεση κλήσης συνάρτησης](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Αρχεία καταγραφής στη σελίδα κλήσης συνάρτησης](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Στον πίνακα εργαλείων SDK WebJobs, τις πιο πρόσφατες 100 γραμμές της κονσόλας εξόδου εμφάνιση του όταν μεταβείτε στη σελίδα για την WebJob (όχι για της κλήσης συνάρτησης) και κάντε κλικ στο κουμπί **Εναλλαγής εξόδου**.
 
![Κάντε κλικ στο κουμπί εναλλαγής εξόδου](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

Σε μια συνεχόμενη WebJob, αρχεία καταγραφής εφαρμογών εμφανίζονται στην/δεδομένων/εργασίες/συνεχής /*{webjobname}*/job_log.txt στο σύστημα αρχείων εφαρμογής web.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Σε μια Azure αντικειμένων blob εμφάνισης αρχεία καταγραφής από την εφαρμογή ως εξής: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Χαίρετε!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Χαίρετε!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Χαίρετε!,

Και σε έναν πίνακα του Azure το `Console.Out` και `Console.Error` αρχεία καταγραφής μοιάζει κάπως έτσι:

![Πληροφορίες αρχείου καταγραφής σε πίνακα](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Αρχείο καταγραφής σφαλμάτων σε πίνακα](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Εάν θέλετε να συνδέσετε το δικό σας πρόγραμμα καταγραφής, ανατρέξτε στο θέμα [αυτό το παράδειγμα](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Πώς να χειριστείτε σφάλματα και να ρυθμίσετε τις παραμέτρους χρονικών ορίων

Το SDK WebJobs περιλαμβάνει επίσης ένα [χρονικό όριο](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) χαρακτηριστικό που μπορείτε να χρησιμοποιήσετε για να προκαλέσει μια συνάρτηση για να την ακυρώσετε Εάν δεν ολοκληρωθεί μέσα σε ένα καθορισμένο χρονικό διάστημα. Εάν θέλετε να βελτιώσετε μια ειδοποίηση όταν συμβεί πάρα πολλά σφάλματα μέσα σε μια συγκεκριμένη χρονική περίοδο, μπορείτε να χρησιμοποιήσετε το `ErrorTrigger` χαρακτηριστικό. Ακολουθεί ένα [παράδειγμα ErrorTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Επίσης δυναμικά, μπορείτε να απενεργοποιήσετε και να ενεργοποιήσετε λειτουργίες για να ελέγχετε εάν αυτές μπορεί να ενεργοποιηθεί, χρησιμοποιώντας ένα διακόπτη ρύθμισης παραμέτρων που θα μπορούσε να είναι μια ρύθμιση εφαρμογής ή το όνομα της μεταβλητής περιβάλλοντος. Για το δείγμα κώδικα, ανατρέξτε στο θέμα το `Disable` χαρακτηριστικό στο [αποθετήριο δείγματα WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a>Επόμενα βήματα

Αυτός ο οδηγός έχει παράσχει δείγματα κώδικα που δείχνουν τον τρόπο χειρισμού των συνηθισμένα σενάρια για την εργασία με Azure ουρές. Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs, ανατρέξτε στο θέμα [Πόροι προτεινόμενα WebJobs Azure](http://go.microsoft.com/fwlink/?linkid=390226).
 

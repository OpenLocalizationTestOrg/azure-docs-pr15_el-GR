<properties 
    pageTitle="Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs" 
    description="Μάθετε πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs. Ενεργοποιεί μια διαδικασία, όταν εμφανίζεται ένα νέο blob σε ένα κοντέινερ και χειρισμού 'αλλοίωσης αντικείμενα BLOB'." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων blob του Azure με το SDK WebJobs

## <a name="overview"></a>Επισκόπηση

Αυτός ο οδηγός παρέχει C# δείγματα κώδικα που σας δείχνουν πώς μπορείτε να ενεργοποιήσετε μια διαδικασία όταν δημιουργείται ή ενημερώνεται μια αντικειμένων blob του Azure. Τα δείγματα κώδικα Χρησιμοποιήστε [WebJobs SDK](websites-dotnet-webjobs-sdk.md) έκδοση 1.x.

Για δείγματα κώδικα που σας δείχνουν πώς μπορείτε να δημιουργήσετε αντικείμενα blob, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Ο οδηγός προϋποθέτει ότι γνωρίζετε [πώς μπορείτε να δημιουργήσετε ένα έργο WebJob στο Visual Studio με συμβολοσειρές σύνδεσης που οδηγούν στο λογαριασμό σας χώρο αποθήκευσης](websites-dotnet-webjobs-sdk-get-started.md) ή σε [πολλούς λογαριασμούς χώρου αποθήκευσης](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Τον τρόπο ενεργοποίησης μιας συνάρτησης όταν δημιουργείται ή ενημερώνεται ένα blob

Αυτή η ενότητα σας δείχνει πώς μπορείτε να χρησιμοποιήσετε το `BlobTrigger` χαρακτηριστικό. 

> [AZURE.NOTE] Το SDK WebJobs σαρώνει αρχεία καταγραφής για να παρακολουθήσετε για νέα ή τροποποιημένα αντικείμενα blob. Αυτή η διαδικασία δεν είναι σε πραγματικό χρόνο; μια συνάρτηση ενδέχεται να δεν λάβετε ενεργοποιήσει μέχρι μερικά λεπτά ή περισσότερο αφού δημιουργηθεί το αντικείμενο blob. Επιπλέον, βάση [χώρου αποθήκευσης που δημιουργούνται αρχεία καταγραφής στο "βέλτιστη προσπάθειες"](https://msdn.microsoft.com/library/azure/hh343262.aspx) . δεν υπάρχει εγγύηση ότι θα καταγραφεί όλα τα συμβάντα. Υπό ορισμένες συνθήκες, μπορεί να λείπουν αρχεία καταγραφής. Εάν οι περιορισμοί ταχύτητα και αξιοπιστία εναυσμάτων blob δεν είναι αποδεκτές για την εφαρμογή σας, η προτεινόμενη μέθοδος είναι να δημιουργήσετε ένα μήνυμα ουρά όταν δημιουργείτε το αντικείμενο blob, και χρησιμοποιήστε το χαρακτηριστικό [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) αντί για το `BlobTrigger` χαρακτηριστικό για τη συνάρτηση που επεξεργάζεται το αντικείμενο blob.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Σύμβολο κράτησης θέσης για το όνομα blob με επέκταση  

Το παρακάτω δείγμα κώδικα αντιγράφει αντικείμενα BLOB κειμένου που εμφανίζονται στο κοντέινερ *εισαγωγής* στο κοντέινερ *εξόδου* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Η κατασκευή χαρακτηριστικού μεταφέρει παραμέτρου συμβολοσειράς που καθορίζει το όνομα του κοντέινερ και ένα σύμβολο κράτησης θέσης για το όνομα blob. Σε αυτό το παράδειγμα, εάν έχει δημιουργηθεί ένα blob με το όνομα *Blob1.txt* στο κοντέινερ *εισαγωγής* , η συνάρτηση δημιουργεί ένα blob με το όνομα *Blob1.txt* στο κοντέινερ *εξόδου* . 

Μπορείτε να καθορίσετε ένα μοτίβο όνομα με το πλαίσιο κράτησης θέσης ονομάτων αντικειμένων blob, όπως φαίνεται στο ακόλουθο δείγμα κώδικα:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Αυτός ο κωδικός αντιγράφει μόνο αντικείμενα BLOB που έχουν ονόματα που αρχίζουν από "Αρχική-". Για παράδειγμα, *Αρχικό Blob1.txt* στο κοντέινερ *εισαγωγής* αντιγράφεται *Αντίγραφο Blob1.txt* στο κοντέινερ *εξόδου* .

Εάν χρειάζεστε για να καθορίσετε ένα μοτίβο όνομα για τα ονόματα αντικειμένων blob που έχουν τα άγκιστρα στο όνομά της, κάντε διπλό τα άγκιστρα. Για παράδειγμα, εάν θέλετε να βρείτε αντικείμενα blob στο κοντέινερ *εικόνες* που έχουν ονόματα ως εξής:

        {20140101}-soundfile.mp3

Χρησιμοποιήστε αυτήν την επιλογή για το μοτίβο:

        images/{{20140101}}-{name}

Στο παράδειγμα, η τιμή *όνομα* κράτησης θέσης θα ήταν *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Ξεχωριστές blob όνομα και την επέκταση σύμβολα κράτησης θέσης

Το παρακάτω δείγμα κώδικα αλλάζει την επέκταση αρχείου κατά την αντιγραφή αντικειμένων blob που εμφανίζονται στο κοντέινερ *εισαγωγής* στο κοντέινερ *εξόδου* . Ο κώδικας καταγράφει την επέκταση του το αντικείμενο blob της *εισαγωγής* και ορίζει την επέκταση του blob το *αποτέλεσμα* σε *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Τύποι που μπορείτε να συνδέσετε με αντικείμενα BLOB

Μπορείτε να χρησιμοποιήσετε το `BlobTrigger` χαρακτηριστικό σχετικά με τους παρακάτω τύπους:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Άλλοι τύποι αποσειριοποιηθούν από [ICloudBlobStreamBinder](#icbsb) 

Εάν θέλετε να εργαστείτε απευθείας με το λογαριασμό Azure χώρου αποθήκευσης, μπορείτε επίσης να προσθέσετε μια `CloudStorageAccount` παραμέτρου στην υπογραφή μεθόδου.

Για παραδείγματα, δείτε τον [κώδικα σύνδεσης αντικειμένων blob στο αποθετήριο azure-webjobs-sdk στην GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Λήψη περιεχομένου blob κειμένου από τη σύνδεση σε συμβολοσειρά

Εάν είναι αναμένεται αντικείμενα BLOB κειμένου, `BlobTrigger` μπορούν να εφαρμοστούν σε μια `string` παραμέτρου. Το παρακάτω δείγμα κώδικα συνδέει ένα blob κειμένου σε ένα `string` παράμετρο που ονομάζεται `logMessage`. Η συνάρτηση χρησιμοποιεί αυτήν την παράμετρο για να γράψετε των περιεχομένων του blob στον πίνακα εργαλείων WebJobs SDK. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Γρήγορα Σειριοποιημένο blob περιεχομένου με τη χρήση ICloudBlobStreamBinder

Το παρακάτω δείγμα κώδικα χρησιμοποιεί μια κλάση που υλοποιεί `ICloudBlobStreamBinder` για να ενεργοποιήσετε το `BlobTrigger` χαρακτηριστικό για να δεσμεύσετε ένα blob για να το `WebImage` τύπου.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Το `WebImage` βιβλιοδεσία κώδικα παρέχεται σε μια `WebImageBinder` κλάσης που προέρχεται από `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Γρήγορα τη διαδρομή blob για το αντικείμενο blob της ενεργοποίησης

Για να λάβετε το όνομα κοντέινερ και αντικειμένων blob του το αντικείμενο blob που έχει ενεργοποιήσει τη συνάρτηση, συμπεριλάβετε μια `blobTrigger` η παράμετρος στη συνάρτηση υπογραφής της συμβολοσειράς.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Τρόπος χειρισμού αλλοίωσης αντικείμενα BLOB

Όταν μια `BlobTrigger` συνάρτηση αποτύχει, το SDK καλεί το ξανά, σε περίπτωση αποτυχίας προκλήθηκε από ένα σφάλμα μεταβατικές. Εάν το σφάλμα προκαλείται από το περιεχόμενο του το αντικείμενο blob, η συνάρτηση αποτυγχάνει κάθε φορά που προσπαθεί να επεξεργαστεί το αντικείμενο blob. Από προεπιλογή, το SDK κλήσεις μια συνάρτηση έως 5 φορές για μια δεδομένη blob. Εάν το πέμπτο, δοκιμάστε να αποτύχει, το SDK προσθέτει ένα μήνυμα σε μια ουρά με το όνομα *webjobs-blobtrigger-δηλητήριο*.

Ο μέγιστος αριθμός των επαναλήψεων είναι δυνατό να ρυθμιστεί. Η ίδια ρύθμιση [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) χρησιμοποιείται για το χειρισμό αλλοίωσης blob και χειρισμό μηνυμάτων αλλοίωσης ουρά. 

Το μήνυμα ουρά για αλλοίωσης αντικείμενα BLOB είναι ένα αντικείμενο JSON που περιέχει τις ακόλουθες ιδιότητες:

* FunctionId (στη μορφή *{όνομα WebJob}*. Συναρτήσεις. *{Το όνομα συνάρτησης}*, για παράδειγμα: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" ή "PageBlob")
* ContainerName
* BlobName
* ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

Το παρακάτω δείγμα κώδικα, το `CopyBlob` έχει κώδικα που έχει ως αποτέλεσμα την αποτυχία κάθε φορά που ονομάζεται. Μετά το SDK το κλήσεων για τον μέγιστο αριθμό των επαναλήψεων, δημιουργείται ένα μήνυμα στην ουρά αλλοίωσης blob και αυτό το μήνυμα έχει υποβληθεί σε επεξεργασία από το `LogPoisonBlob` συνάρτηση. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

Το SDK deserializes αυτόματα το μήνυμα JSON. Ακολουθεί η `PoisonBlobMessage` κλάσης: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Ο αλγόριθμος ανίχνευσης BLOB

Το SDK WebJobs σαρώνει όλα τα κοντέινερ που καθορίζεται από `BlobTrigger` χαρακτηριστικά κατά την εκκίνηση εφαρμογών. Σε ένα λογαριασμό μεγάλο χώρο αποθήκευσης αυτή η σάρωση μπορεί να διαρκέσει αρκετά, ώστε να μπορεί να είναι λίγο πριν βρεθούν νέα αντικείμενα BLOB και `BlobTrigger` εκτελούνται συναρτήσεις.

Για να εντοπίσετε νέα ή τροποποιημένα αντικείμενα BLOB μετά την έναρξη της εφαρμογής, το SDK διαβάζει περιοδικά από τα αρχεία καταγραφής αποθήκευσης αντικειμένων blob. Τα αρχεία καταγραφής blob είναι σε buffer και μόνο λήψη φυσική εγγραφή κάθε 10 λεπτά ή έτσι, επομένως, μπορεί να υπάρχουν σημαντική καθυστέρηση αφού ένα blob δημιουργείται ή ενημερώνεται πριν από το αντίστοιχο `BlobTrigger` εκτελεί την συνάρτηση. 

Υπάρχει μια εξαίρεση για αντικείμενα BLOB που δημιουργείτε χρησιμοποιώντας το `Blob` χαρακτηριστικό. Όταν το SDK WebJobs δημιουργεί ένα νέο blob, περάσει για το νέο blob αμέσως σε οποιαδήποτε που ταιριάζουν `BlobTrigger` συναρτήσεις. Επομένως, εάν έχετε μια αλυσίδα blob εισροές και εκροές, το SDK μπορεί να επεξεργαστεί τους αποτελεσματικά. Αλλά εάν θέλετε χαμηλής λανθάνων χρόνος εκτελείται σας blob λειτουργίες επεξεργασίας για αντικείμενα BLOB που έχουν δημιουργηθεί ή έχουν ενημερωθεί από άλλα μέσα, συνιστάται να χρησιμοποιείτε `QueueTrigger` όχι `BlobTrigger`.

### <a id="receipts"></a>Αποδεικτικά BLOB

Το SDK WebJobs εξασφαλίζει ότι δεν υπάρχει `BlobTrigger` συνάρτηση λαμβάνει ονομάζεται περισσότερες από μία φορές για το ίδιο αντικείμενο blob νέο ή ενημερωμένο. Αυτό γίνεται, διατηρώντας *αποδεικτικά blob* προκειμένου να καθορίζουν εάν μια δεδομένη blob έκδοση έχει υποστεί επεξεργασία.

Αποδεικτικά BLOB αποθηκεύονται σε ένα κοντέινερ που ονομάζεται *azure-webjobs-hosts* στο λογαριασμό Azure χώρου αποθήκευσης που καθορίζεται από τη συμβολοσειρά σύνδεσης AzureWebJobsStorage. Αποδεικτικό blob περιλαμβάνει τις ακόλουθες πληροφορίες:

* Η συνάρτηση που ονομαζόταν για το αντικείμενο blob ("*{WebJob όνομα}*. Συναρτήσεις. *{Το όνομα συνάρτησης}*", για παράδειγμα:"WebJob1.Functions.CopyBlob")
* Το όνομα του κοντέινερ
* Ο τύπος blob ("BlockBlob" ή "PageBlob")
* Το όνομα blob
* Το ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

Εάν θέλετε να επιβάλετε επεξεργασία ένα blob, μπορείτε να διαγράψετε την παραλαβή blob για το αντικείμενο blob με μη αυτόματο τρόπο από το κοντέινερ *κεντρικούς υπολογιστές webjobs azure* .

## <a id="queues"></a>Σχετικά θέματα που καλύπτονται από το άρθρο ουρές

Για πληροφορίες σχετικά με τον τρόπο χειρισμού των αντικειμένων blob επεξεργασίας που ενεργοποιείται κάνοντας ένα μήνυμα ουρά ή για WebJobs SDK δεν συγκεκριμένα σενάρια για αντικειμένων blob επεξεργασίας, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Σχετικά θέματα που αναφέρονται σε αυτό το άρθρο περιλαμβάνουν τα εξής:

* Ασύγχρονων συναρτήσεων
* Πολλές παρουσίες
* Φυσιολογική τερματισμού
* Χρήση του WebJobs SDK χαρακτηριστικά στο σώμα μιας συνάρτησης
* Ορίστε τις συμβολοσειρές σύνδεσης SDK στον κώδικα.
* Ορισμός τιμών για WebJobs SDK παραμέτρους κατασκευή στον κώδικα
* Ρύθμιση παραμέτρων `MaxDequeueCount` για το χειρισμό αλλοίωσης blob.
* Ενεργοποίηση μιας συνάρτησης με μη αυτόματο τρόπο
* Γράψτε αρχείων καταγραφής

## <a id="nextsteps"></a>Επόμενα βήματα

Αυτός ο οδηγός έχει παράσχει δείγματα κώδικα που δείχνουν τον τρόπο χειρισμού των συνηθισμένα σενάρια για την εργασία με αντικείμενα BLOB Azure. Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs, ανατρέξτε στο θέμα [Πόροι προτεινόμενα WebJobs Azure](http://go.microsoft.com/fwlink/?linkid=390226).
 

<properties
    pageTitle="Γρήγορα αποτελέσματα με το blob χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες (WebJob έργα) | Microsoft Azure"
    description="Μάθετε πώς να γρήγορα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob σε ένα έργο WebJob μετά τη σύνδεση σε ένα χώρο αποθήκευσης Azure μέσω του Visual Studio συνδεδεμένες υπηρεσίες."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Γρήγορα αποτελέσματα με το Azure Blob χώρου αποθήκευσης και του Visual Studio συνδεδεμένες υπηρεσίες (WebJob έργα)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο παρέχει C# δείγματα κώδικα που σας δείχνουν πώς μπορείτε να ενεργοποιήσετε μια διαδικασία όταν δημιουργείται ή ενημερώνεται μια αντικειμένων blob του Azure. Τα δείγματα κώδικα, χρησιμοποιήστε την έκδοση [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) 1.x. Όταν προσθέτετε ένα λογαριασμό του χώρου αποθήκευσης σε ένα έργο WebJob, χρησιμοποιώντας το παράθυρο διαλόγου **Προσθήκη συνδεδεμένες υπηρεσίες** του Visual Studio, το κατάλληλο πακέτο Azure NuGet χώρου αποθήκευσης είναι εγκατεστημένο, τις κατάλληλες αναφορές .NET προστίθενται στο έργο και ενημερώνονται συμβολοσειρές σύνδεσης για το λογαριασμό χώρου αποθήκευσης του αρχείου App.config.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Τον τρόπο ενεργοποίησης μιας συνάρτησης όταν δημιουργείται ή ενημερώνεται ένα blob

Αυτή η ενότητα δείχνει πώς μπορείτε να χρησιμοποιήσετε το χαρακτηριστικό **BlobTrigger** .

 **Σημείωση:** Το SDK WebJobs σαρώνει αρχεία καταγραφής για να παρακολουθήσετε για νέα ή τροποποιημένα αντικείμενα blob. Αυτή η διαδικασία είναι εγγενώς αργή μια συνάρτηση ενδέχεται να δεν λάβετε ενεργοποιήσει μέχρι μερικά λεπτά ή περισσότερο αφού δημιουργηθεί το αντικείμενο blob.  Εάν η εφαρμογή σας πρέπει να επεξεργαστεί αντικείμενα BLOB αμέσως, η προτεινόμενη μέθοδος είναι να δημιουργήσετε ένα μήνυμα ουρά όταν δημιουργείτε το αντικείμενο blob, και χρησιμοποιήστε το χαρακτηριστικό [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) αντί για το χαρακτηριστικό **BlobTrigger** για τη συνάρτηση που επεξεργάζεται το αντικείμενο blob.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Τύποι που μπορείτε να συνδέσετε με αντικείμενα BLOB

Μπορείτε να χρησιμοποιήσετε το χαρακτηριστικό **BlobTrigger** σχετικά με τους παρακάτω τύπους:

* **συμβολοσειρά**
* **TextReader**
* **Ροή**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Άλλοι τύποι αποσειριοποιηθούν από [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Εάν θέλετε να εργαστείτε απευθείας με το λογαριασμό Azure χώρου αποθήκευσης, μπορείτε επίσης να προσθέσετε μια παράμετρο **CloudStorageAccount** στην υπογραφή μεθόδου.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Λήψη περιεχομένου blob κειμένου από τη σύνδεση σε συμβολοσειρά

Εάν είναι αναμένεται αντικείμενα BLOB κειμένου, **BlobTrigger** μπορούν να εφαρμοστούν σε μια παράμετρο **συμβολοσειρά** . Το παρακάτω δείγμα κώδικα συνδέει ένα blob κειμένου με μια παράμετρο **συμβολοσειράς** που ονομάζεται **logMessage**. Η συνάρτηση χρησιμοποιεί αυτήν την παράμετρο για να γράψετε των περιεχομένων του blob στον πίνακα εργαλείων WebJobs SDK.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Γρήγορα Σειριοποιημένο blob περιεχομένου με τη χρήση ICloudBlobStreamBinder

Το παρακάτω δείγμα κώδικα χρησιμοποιεί μια κλάση που υλοποιεί **ICloudBlobStreamBinder** για να ενεργοποιήσετε το χαρακτηριστικό **BlobTrigger** για να δεσμεύσετε ένα blob στον τύπο **WebImage** .

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

Ο κώδικας σύνδεση **WebImage** παρέχεται σε μια κλάση **WebImageBinder** που προέρχεται από **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Τρόπος χειρισμού αλλοίωσης αντικείμενα BLOB

Όταν αποτύχει η συνάρτηση **BlobTrigger** , το SDK καλεί το ξανά, σε περίπτωση αποτυχίας προκλήθηκε από ένα σφάλμα μεταβατικές. Εάν το σφάλμα προκαλείται από το περιεχόμενο του το αντικείμενο blob, η συνάρτηση αποτυγχάνει κάθε φορά που προσπαθεί να επεξεργαστεί το αντικείμενο blob. Από προεπιλογή, το SDK κλήσεις μια συνάρτηση έως 5 φορές για μια δεδομένη blob. Εάν το πέμπτο, δοκιμάστε να αποτύχει, το SDK προσθέτει ένα μήνυμα σε μια ουρά με το όνομα *webjobs-blobtrigger-δηλητήριο*.

Ο μέγιστος αριθμός των επαναλήψεων είναι δυνατό να ρυθμιστεί. Η ίδια ρύθμιση [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) χρησιμοποιείται για το χειρισμό αλλοίωσης blob και χειρισμό μηνυμάτων αλλοίωσης ουρά.

Το μήνυμα ουρά για αλλοίωσης αντικείμενα BLOB είναι ένα αντικείμενο JSON που περιέχει τις ακόλουθες ιδιότητες:

* FunctionId (στη μορφή *{όνομα WebJob}*. Συναρτήσεις. *{Το όνομα συνάρτησης}*, για παράδειγμα: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" ή "PageBlob")
* ContainerName
* BlobName
* ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

Το παρακάτω δείγμα κώδικα, η συνάρτηση **CopyBlob** έχει κώδικα που έχει ως αποτέλεσμα την αποτυχία κάθε φορά που ονομάζεται. Αφού το SDK το απαιτεί ο μέγιστος αριθμός των επαναλήψεων, δημιουργείται ένα μήνυμα στην ουρά αλλοίωσης blob και αυτό το μήνυμα είναι υποβάλλονται σε επεξεργασία από τη συνάρτηση **LogPoisonBlob** .

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

Το SDK deserializes αυτόματα το μήνυμα JSON. Ακολουθεί η κλάση **PoisonBlobMessage** :

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Ο αλγόριθμος ανίχνευσης BLOB

Το SDK WebJobs σαρώνει όλα τα κοντέινερ που καθορίζεται από τα χαρακτηριστικά **BlobTrigger** κατά την εκκίνηση εφαρμογών. Σε ένα λογαριασμό μεγάλο χώρο αποθήκευσης αυτή η σάρωση μπορεί να διαρκέσει αρκετά, ώστε να μπορεί να είναι λίγο πριν βρεθούν νέα αντικείμενα BLOB και συναρτήσεις **BlobTrigger** εκτελούνται.

Για να εντοπίσετε νέα ή τροποποιημένα αντικείμενα BLOB μετά την έναρξη της εφαρμογής, το SDK διαβάζει περιοδικά από τα αρχεία καταγραφής αποθήκευσης αντικειμένων blob. Τα αρχεία καταγραφής blob είναι σε buffer και μόνο λήψη φυσική εγγραφή κάθε 10 λεπτά ή έτσι, ώστε να μπορεί να υπάρχουν σημαντική καθυστέρηση αφού ένα blob δημιουργείται ή ενημερώνεται πριν από την αντίστοιχη συνάρτηση **BlobTrigger** εκτελεί την.

Υπάρχει μια εξαίρεση για αντικείμενα BLOB που δημιουργείτε χρησιμοποιώντας το χαρακτηριστικό **Blob** . Όταν το SDK WebJobs δημιουργεί ένα νέο blob, περάσει για το νέο blob αμέσως τυχόν αντίστοιχες λειτουργίες **BlobTrigger** . Επομένως, εάν έχετε μια αλυσίδα blob εισροές και εκροές, το SDK μπορεί να επεξεργαστεί τους αποτελεσματικά. Αλλά εάν θέλετε χαμηλής λανθάνων χρόνος εκτελείται σας blob επεξεργασίας συναρτήσεις για αντικείμενα BLOB που έχουν δημιουργηθεί ή έχουν ενημερωθεί από άλλα μέσα, συνιστάται να χρησιμοποιείτε **QueueTrigger** αντί για **BlobTrigger**.

### <a name="blob-receipts"></a>Αποδεικτικά BLOB

Το SDK WebJobs διασφαλίζει ότι η συνάρτηση **BlobTrigger** δεν λαμβάνει περισσότερες από μία φορές ονομάζεται για το ίδιο αντικείμενο blob νέο ή ενημερωμένο. Αυτό γίνεται, διατηρώντας *αποδεικτικά blob* προκειμένου να καθορίζουν εάν μια έκδοση που δίνεται blob έχει υποστεί επεξεργασία.

Αποδεικτικά BLOB αποθηκεύονται σε ένα κοντέινερ που ονομάζεται *azure-webjobs-hosts* στο λογαριασμό Azure χώρου αποθήκευσης που καθορίζεται από τη συμβολοσειρά σύνδεσης AzureWebJobsStorage. Αποδεικτικό blob περιλαμβάνει τις ακόλουθες πληροφορίες:

* Η συνάρτηση που ονομαζόταν για το αντικείμενο blob ("*{WebJob όνομα}*. Συναρτήσεις. *{Το όνομα συνάρτησης}*", για παράδειγμα:"WebJob1.Functions.CopyBlob")
* Το όνομα του κοντέινερ
* Ο τύπος blob ("BlockBlob" ή "PageBlob")
* Το όνομα blob
* Το ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

Εάν θέλετε να επιβάλετε επεξεργασία ένα blob, μπορείτε να διαγράψετε την παραλαβή blob για το αντικείμενο blob με μη αυτόματο τρόπο από το κοντέινερ *azure-webjobs-κεντρικών υπολογιστών* .

## <a name="related-topics-covered-by-the-queues-article"></a>Σχετικά θέματα που καλύπτονται από το άρθρο ουρές

Για πληροφορίες σχετικά με τον τρόπο χειρισμού των αντικειμένων blob επεξεργασίας που ενεργοποιείται κάνοντας ένα μήνυμα ουρά ή για WebJobs SDK δεν συγκεκριμένα σενάρια για αντικειμένων blob επεξεργασίας, ανατρέξτε στο θέμα [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης Azure ουρά με το SDK WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Σχετικά θέματα που αναφέρονται σε αυτό το άρθρο περιλαμβάνουν τα εξής:

* Ασύγχρονων συναρτήσεων
* Πολλές παρουσίες
* Φυσιολογική τερματισμού
* Χρήση του WebJobs SDK χαρακτηριστικά στο σώμα μιας συνάρτησης
* Ορίστε τις συμβολοσειρές σύνδεσης SDK στον κώδικα.
* Ορισμός τιμών για WebJobs SDK παραμέτρους κατασκευή στον κώδικα
* Ρύθμιση παραμέτρων **MaxDequeueCount** για το χειρισμό αλλοίωσης blob.
* Ενεργοποίηση μιας συνάρτησης με μη αυτόματο τρόπο
* Γράψτε αρχείων καταγραφής

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο παρέχει δείγματα κώδικα που δείχνουν τον τρόπο χειρισμού των συνηθισμένα σενάρια για την εργασία με αντικείμενα BLOB Azure. Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure WebJobs και το SDK WebJobs, ανατρέξτε στο θέμα [Azure WebJobs τεκμηρίωση πόρους](http://go.microsoft.com/fwlink/?linkid=390226).

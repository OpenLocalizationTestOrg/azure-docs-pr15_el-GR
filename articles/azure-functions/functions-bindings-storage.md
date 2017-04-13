<properties
    pageTitle="Azure συναρτήσεις εναύσματα και συνδέσεις για το χώρο αποθήκευσης Azure | Microsoft Azure"
    description="Κατανόηση πώς μπορείτε να χρησιμοποιήσετε το χώρο αποθήκευσης Azure εναύσματα και συνδέσεις σε συναρτήσεις Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, συμβάν επεξεργασία, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure συναρτήσεις εναύσματα και συνδέσεις για το χώρο αποθήκευσης Azure

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να ρυθμίσετε τις παραμέτρους και κώδικα αποθήκευσης Azure εναύσματα και συνδέσεις σε συναρτήσεις Azure. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure έναυσμα ουρά χώρου αποθήκευσης

#### <a name="functionjson-for-storage-queue-trigger"></a>Function.JSON για έναυσμα ουρά χώρου αποθήκευσης

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για την ουρά ή το μήνυμα ουρά. 
- `queueName`: Το όνομα της ουράς προς απάντηση. Για τους κανόνες ονοματοθεσίας ουρά, ανατρέξτε στο θέμα [ονομασίας ουρές και μετα-δεδομένων](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης. Εάν αφήσετε `connection` κενά, το έναυσμα θα λειτουργεί με την προεπιλεγμένη συμβολοσειρά σύνδεσης χώρου αποθήκευσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsStorage.
- `type`: Πρέπει να οριστεί στην *queueTrigger*.
- `direction`: Πρέπει να οριστούν *σε*. 

Παράδειγμα *function.json* για ένα έναυσμα ουρά χώρου αποθήκευσης:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Έναυσμα ουρά υποστηριζόμενοι τύποι

Το μήνυμα ουρά μπορεί να αποσειριοποιούνται σε οποιονδήποτε από τους παρακάτω τύπους:

* Αντικείμενο (από JSON)
* Συμβολοσειρά
* Ο πίνακας byte 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Μετα-δεδομένα έναυσμα ουρά

Μπορείτε να λάβετε ουρά μετα-δεδομένων στη συνάρτηση σας με τη χρήση αυτά τα ονόματα των μεταβλητών:

* expirationTime
* insertionTime
* nextVisibleTime
* αναγνωριστικό
* popReceipt
* dequeueCount
* queueTrigger (ένας άλλος τρόπος για να ανακτήσετε το κείμενο του μηνύματος ουρά ως συμβολοσειρά)

Το παράδειγμα αυτό κώδικα C# ανακτά και αρχεία καταγραφής ουρά μετα-δεδομένων:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Χειρισμός μηνυμάτων αλλοίωσης ουρά

Μηνύματα των οποίων το περιεχόμενο έχει ως αποτέλεσμα μια συνάρτηση αποτυχία ονομάζονται *μηνυμάτων αλλοίωσης*. Όταν η συνάρτηση αποτυγχάνει, το μήνυμα ουρά δεν διαγράφεται και τελικά είναι επιλέξατε προς τα επάνω και πάλι, προκαλεί τον κύκλο να επαναληφθεί. Το SDK αυτόματα να διακόψετε τον κύκλο μετά από έναν περιορισμένο αριθμό των διαδοχικών προσεγγίσεων ή μπορείτε να το κάνετε με μη αυτόματο τρόπο.

Το SDK θα καλέσετε μια συνάρτηση έως και 5 φορές η επεξεργασία ενός μηνύματος ουρά. Εάν το πέμπτο, δοκιμάστε να αποτύχει, το μήνυμα έχει μετακινηθεί σε ουρά αλλοίωσης.

Αλλοίωσης ουρά ονομάζεται *{originalqueuename}*-αλλοίωσης. Μπορείτε να συντάξετε μια συνάρτηση για να επεξεργαστείτε μηνύματα από την αλλοίωσης ουρά με καταγραφή τους ή την αποστολή ειδοποίησης που μη αυτόματη προσοχή είναι απαραίτητο. 

Εάν θέλετε να χειριστείτε μηνυμάτων αλλοίωσης με μη αυτόματο τρόπο, μπορείτε να λάβετε του αριθμού των φορών που έχει συλλεχθεί μηνύματος για επεξεργασία, επιλέγοντας `dequeueCount`.

## <a id="storagequeueoutput"></a>Σύνδεση εξόδου Azure ουρά χώρου αποθήκευσης

#### <a name="functionjson-for-storage-queue-output-binding"></a>Function.JSON για το χώρο αποθήκευσης ουρά εξόδου σύνδεσης

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για την ουρά ή το μήνυμα ουρά. 
- `queueName`: Το όνομα της ουράς. Για τους κανόνες ονοματοθεσίας ουρά, ανατρέξτε στο θέμα [Ονομασία ουρές και μετα-δεδομένων](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης. Εάν αφήσετε `connection` κενά, το έναυσμα θα λειτουργεί με την προεπιλεγμένη συμβολοσειρά σύνδεσης χώρου αποθήκευσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsStorage.
- `type`: Πρέπει να οριστεί στην *ουρά*.
- `direction`: Πρέπει να οριστεί στην *ανάληψη*. 

Παράδειγμα *function.json* για μια ουρά αποθήκευσης εξόδου βιβλιοδεσία που χρησιμοποιεί ένα έναυσμα ουρά και καταγράφει ένα μήνυμα ουρά:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Τύποι σύνδεσης εξόδου υποστηρίζονται ουράς

Το `queue` σύνδεσης μπορεί να σειριοποίηση τους παρακάτω τύπους σε ένα μήνυμα ουρά:

* Αντικείμενο (`out T` σε C#, δημιουργεί ένα μήνυμα με ένα αντικείμενο null, εάν η παράμετρος είναι null όταν λήξει η συνάρτηση)
* Συμβολοσειρά (`out string` σε C#, δημιουργεί μήνυμα ουρά εάν η τιμή της παραμέτρου είναι δεν είναι null όταν λήξει η συνάρτηση)
* Ο πίνακας byte (`out byte[]` σε C#, λειτουργεί ως συμβολοσειρά) 
* `out CloudQueueMessage`(C#, λειτουργεί ως συμβολοσειρά) 

Στο C# μπορείτε επίσης να συνδέσετε με `ICollector<T>` ή `IAsyncCollector<T>` όπου `T` είναι μόνο ένας από τους τύπους που υποστηρίζονται.

#### <a name="queue-output-binding-code-examples"></a>Παραδείγματα κώδικα σύνδεσης ουρά εξόδου

Το παράδειγμα αυτό κώδικα C# εγγράφει ένα μήνυμα ουράς εξόδου για κάθε μήνυμα ουρά εισόδου.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

Το παράδειγμα αυτό κώδικα C# συντάσσει πολλών μηνυμάτων με τη χρήση `ICollector<T>` (χρήση `IAsyncCollector<T>` σε μια συνάρτηση ασύγχρονης):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure χώρο αποθήκευσης αντικειμένων blob εναυσμάτων

#### <a name="functionjson-for-storage-blob-trigger"></a>Function.JSON για το χώρο αποθήκευσης αντικειμένων blob έναυσμα

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το αντικείμενο blob. 
- `path`: Μια διαδρομή που καθορίζει το κοντέινερ για την παρακολούθηση και, προαιρετικά ένα μοτίβο ονομάτων αντικειμένων blob.
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης. Εάν αφήσετε `connection` κενά, το έναυσμα θα λειτουργεί με την προεπιλεγμένη συμβολοσειρά σύνδεσης χώρου αποθήκευσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsStorage.
- `type`: Πρέπει να οριστεί στην *blobTrigger*.
- `direction`: Πρέπει να οριστούν *σε*.

Παράδειγμα *function.json* για ένα έναυσμα blob χώρου αποθήκευσης που παρακολουθεί αντικείμενα BLOB που προστίθενται στο κοντέινερ δείγματα workitems:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Έναυσμα BLOB υποστηριζόμενοι τύποι

Το αντικείμενο blob μπορεί να αποσειριοποιούνται σε οποιονδήποτε από τους παρακάτω τύπους σε συναρτήσεις κόμβου ή C#:

* Αντικείμενο (από JSON)
* Συμβολοσειρά

Σε συναρτήσεις C# μπορείτε επίσης να συνδέσετε σε οποιονδήποτε από τους παρακάτω τύπους:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Άλλοι τύποι αποσειριοποιηθούν από [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) 

#### <a name="blob-trigger-c-code-example"></a>Έναυσμα BLOB C# παράδειγμα κώδικα

Το παράδειγμα αυτό κώδικα C# καταγράφει τα περιεχόμενα του κάθε blob που θα προστεθεί στο κοντέινερ εποπτευόμενη.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Μοτίβα όνομα εναύσματος BLOB

Μπορείτε να καθορίσετε ένα μοτίβο για όνομα αντικειμένων blob του `path` την ιδιότητα. Για παράδειγμα:

```json
"path": "input/original-{name}",
```

Αυτή η διαδρομή θα βρείτε ένα blob *Αρχικό Blob1.txt* στο κοντέινερ *εισαγωγής* και την τιμή με το όνομα του `name` μεταβλητή στον κώδικα συνάρτηση θα ήταν `Blob1`.

Ένα άλλο παράδειγμα:

```json
"path": "input/{blobname}.{blobextension}",
```

Αυτή η διαδρομή θα επίσης να βρείτε ένα blob με το όνομα *Αρχικό Blob1.txt*και την τιμή του `blobname` και `blobextension` μεταβλητές σε συνάρτηση κώδικα θα ήταν *Αρχικό Blob1* και *txt*.

Μπορείτε να περιορίσετε τους τύπους των αντικειμένων blob που ενεργοποιούν τη συνάρτηση, καθορίζοντας ένα μοτίβο με σταθερή τιμή για την επέκταση αρχείου. Εάν ορίσετε το `path` *να/δειγμάτων / {όνομα} .png*μόνο *.png* αντικείμενα blob στο κοντέινερ *δείγματα* θα ενεργοποιήσει τη συνάρτηση.

Εάν χρειάζεστε για να καθορίσετε ένα μοτίβο όνομα για τα ονόματα αντικειμένων blob που έχουν τα άγκιστρα στο όνομά της, κάντε διπλό τα άγκιστρα. Για παράδειγμα, εάν θέλετε να βρείτε αντικείμενα blob στο κοντέινερ *εικόνες* που έχουν ονόματα ως εξής:

        {20140101}-soundfile.mp3

Χρησιμοποιήστε αυτό για το `path` ιδιότητα:

        images/{{20140101}}-{name}

Στο παράδειγμα, το `name` τιμή μεταβλητής θα ήταν *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Αποδεικτικά BLOB

Το περιβάλλον εκτέλεσης συναρτήσεις Azure διασφαλίζει ότι δεν υπάρχει συνάρτηση έναυσμα blob λαμβάνει περισσότερες από μία φορές ονομάζεται για το ίδιο αντικείμενο blob νέο ή ενημερωμένο. Αυτό γίνεται, διατηρώντας *αποδεικτικά blob* προκειμένου να καθορίζουν εάν μια δεδομένη blob έκδοση έχει υποστεί επεξεργασία.

Αποδεικτικά BLOB αποθηκεύονται σε ένα κοντέινερ που ονομάζεται *azure-webjobs-hosts* στο λογαριασμό Azure χώρου αποθήκευσης που καθορίζεται από τη συμβολοσειρά σύνδεσης AzureWebJobsStorage. Αποδεικτικό blob περιλαμβάνει τις ακόλουθες πληροφορίες:

* Η συνάρτηση που ονομαζόταν για το αντικείμενο blob ("*{όνομα εφαρμογής της συνάρτησης}*. Συναρτήσεις. *{το όνομα συνάρτησης}*", για παράδειγμα:"functionsf74b96f7. Functions.CopyBlob")
* Το όνομα του κοντέινερ
* Ο τύπος blob ("BlockBlob" ή "PageBlob")
* Το όνομα blob
* Το ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

Εάν θέλετε να επιβάλετε επεξεργασία ένα blob, μπορείτε να διαγράψετε την παραλαβή blob για το αντικείμενο blob με μη αυτόματο τρόπο από το κοντέινερ *κεντρικούς υπολογιστές webjobs azure* .

#### <a name="handling-poison-blobs"></a>Χειρισμός αλλοίωσης αντικείμενα BLOB

Όταν αποτύχει η συνάρτηση έναυσμα blob, το SDK καλεί το ξανά, σε περίπτωση αποτυχίας προκλήθηκε από ένα σφάλμα μεταβατικές. Εάν το σφάλμα προκαλείται από το περιεχόμενο του το αντικείμενο blob, η συνάρτηση αποτυγχάνει κάθε φορά που προσπαθεί να επεξεργαστεί το αντικείμενο blob. Από προεπιλογή, το SDK κλήσεις μια συνάρτηση έως 5 φορές για μια δεδομένη blob. Εάν το πέμπτο, δοκιμάστε να αποτύχει, το SDK προσθέτει ένα μήνυμα σε μια ουρά με το όνομα *webjobs-blobtrigger-δηλητήριο*.

Το μήνυμα ουρά για αλλοίωσης αντικείμενα BLOB είναι ένα αντικείμενο JSON που περιέχει τις ακόλουθες ιδιότητες:

* FunctionId (σε μορφή *{όνομα εφαρμογής της συνάρτησης}*. Συναρτήσεις. *{το όνομα συνάρτησης}*)
* BlobType ("BlockBlob" ή "PageBlob")
* ContainerName
* BlobName
* ETag (ένα αναγνωριστικό έκδοση blob, για παράδειγμα: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>BLOB σταθμοσκόπησης για μεγάλο κοντέινερ

Εάν το κοντέινερ αντικειμένων blob που παρακολουθεί το έναυσμα περιέχει περισσότερα από 10.000 αντικείμενα blob, η σάρωση χρόνου εκτέλεσης συναρτήσεις τα αρχεία καταγραφής για να παρακολουθήσετε για νέα ή τροποποιημένα αντικείμενα blob. Αυτή η διαδικασία δεν είναι σε πραγματικό χρόνο; μια συνάρτηση ενδέχεται να δεν λάβετε ενεργοποιήσει μέχρι μερικά λεπτά ή περισσότερο αφού δημιουργηθεί το αντικείμενο blob. Επιπλέον, βάση [χώρου αποθήκευσης που δημιουργούνται αρχεία καταγραφής στο "βέλτιστη προσπάθειες"](https://msdn.microsoft.com/library/azure/hh343262.aspx) . δεν υπάρχει εγγύηση ότι θα καταγραφεί όλα τα συμβάντα. Υπό ορισμένες συνθήκες, μπορεί να λείπουν αρχεία καταγραφής. Εάν οι περιορισμοί ταχύτητα και αξιοπιστία blob εναυσμάτων για μεγάλο κοντέινερ δεν είναι αποδεκτές για την εφαρμογή σας, η προτεινόμενη μέθοδος είναι να δημιουργήσετε ένα μήνυμα ουρά όταν δημιουργείτε το αντικείμενο blob, και χρησιμοποιήστε ένα έναυσμα ουρά αντί για ένα έναυσμα blob να επεξεργαστεί το αντικείμενο blob.
 
## <a id="storageblobbindings"></a>Azure χώρο αποθήκευσης αντικειμένων blob εισόδου και εξόδου συνδέσεις

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>Function.JSON για ένα χώρο αποθήκευσης αντικειμένων blob εισόδου ή εξόδου σύνδεσης

Το αρχείο *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για το αντικείμενο blob. 
- `path`: Μια διαδρομή που καθορίζει το κοντέινερ για την ανάγνωση το αντικείμενο blob από ή το αντικείμενο blob για να γράψετε και, προαιρετικά ένα μοτίβο ονομάτων αντικειμένων blob.
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης. Εάν αφήσετε `connection` κενό, η σύνδεση θα λειτουργεί με την προεπιλεγμένη συμβολοσειρά σύνδεσης χώρου αποθήκευσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsStorage.
- `type`: Πρέπει να οριστούν *blob*.
- `direction`: Ορίστε σε *ή *Σμίκρυνση** . 

Παράδειγμα *function.json* για ένα χώρο αποθήκευσης αντικειμένων blob εισόδου ή εξόδου σύνδεσης, χρησιμοποιώντας ένα έναυσμα ουρά για να αντιγράψετε ένα blob:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>Υποστηριζόμενοι τύποι εισόδου και εξόδου BLOB

Το `blob` σύνδεση να σειριοποίηση ή αποσειριοποίηση τους παρακάτω τύπους σε συναρτήσεις Node.js ή C#:

* Αντικείμενο (`out T` σε C# για blob εξόδου: δημιουργεί ένα αντικείμενο blob ως αντικείμενο null εάν η τιμή της παραμέτρου είναι null όταν λήξει η συνάρτηση)
* Συμβολοσειρά (`out string` σε C# για blob εξόδου: δημιουργεί ένα blob μόνο εάν η παράμετρος συμβολοσειράς είναι δεν είναι null όταν η συνάρτηση επιστρέφει)

Στο C# συναρτήσεων, μπορείτε επίσης να συνδέσετε με τους ακόλουθους τύπους:

* `TextReader`(μόνο είσοδος)
* `TextWriter`(μόνο εξόδου)
* `Stream`
* `CloudBlobStream`(μόνο εξόδου)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Αποτέλεσμα BLOB C# παράδειγμα κώδικα

Το παράδειγμα αυτό κώδικα C# αντιγράφει ένα blob του οποίου το όνομα λήψη σε ένα μήνυμα ουράς.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure χώρος αποθήκευσης πινάκων εισόδου και εξόδου συνδέσεις

#### <a name="functionjson-for-storage-tables"></a>Function.JSON για το χώρο αποθήκευσης πινάκων

Η *function.json* καθορίζει τις ακόλουθες ιδιότητες.

- `name`: Το όνομα της μεταβλητής χρησιμοποιείται σε συνάρτηση κώδικα για τη σύνδεση του πίνακα. 
- `tableName`: Το όνομα του πίνακα.
- `partitionKey`και `rowKey` : χρησιμοποιούνται μαζί για να διαβάσετε μια μοναδική οντότητα σε μια συνάρτηση C# ή κόμβο ή να γράψετε μία οντότητα σε μια συνάρτηση κόμβο.
- `take`: Ο μέγιστος αριθμός γραμμών για να διαβάσετε για πίνακα εισαγωγής σε μια συνάρτηση κόμβο.
- `filter`: Παράσταση φίλτρου OData για πίνακα εισαγωγής σε μια συνάρτηση κόμβο.
- `connection`: Το όνομα της εφαρμογής ρύθμιση που περιέχει μια συμβολοσειρά σύνδεσης του χώρου αποθήκευσης. Εάν αφήσετε `connection` κενό, η σύνδεση θα λειτουργεί με την προεπιλεγμένη συμβολοσειρά σύνδεσης χώρου αποθήκευσης για την εφαρμογή συνάρτησης, που έχει καθοριστεί από τη ρύθμιση της εφαρμογής AzureWebJobsStorage.
- `type`: Πρέπει να οριστεί σε *πίνακα*.
- `direction`: Ορίστε σε *ή *Σμίκρυνση** . 

Το παρακάτω παράδειγμα *function.json* χρησιμοποιεί ένα έναυσμα ουρά για να διαβάσετε μια γραμμή μόνο πίνακα. Το JSON παρέχει μια τιμή κλειδιού σχεδιασμένου διαμερίσματα και καθορίζει ότι το κλειδί γραμμή προέρχεται από το μήνυμα ουρά.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Χώρος αποθήκευσης πινάκων εισόδου και εξόδου υποστηριζόμενοι τύποι

Το `table` σύνδεση να σειριοποίηση ή αποσειριοποίηση αντικειμένων σε συναρτήσεις Node.js ή C#. Τα αντικείμενα θα έχει RowKey και PartitionKey ιδιότητες. 

Στο C# συναρτήσεων, μπορείτε επίσης να συνδέσετε με τους ακόλουθους τύπους:

* `T`όπου `T` υλοποιεί`ITableEntity`
* `IQueryable<T>`(μόνο είσοδος)
* `ICollector<T>`(μόνο εξόδου)
* `IAsyncCollector<T>`(μόνο εξόδου)

#### <a name="storage-tables-binding-scenarios"></a>Χώρος αποθήκευσης πινάκων σενάρια σύνδεσης

Η σύνδεση πίνακα υποστηρίζει τα ακόλουθα σενάρια:

* Διαβάστε μία γραμμή σε μια συνάρτηση C# ή κόμβο.

    Ορισμός `partitionKey` και `rowKey`. Το `filter` και `take` ιδιότητες δεν χρησιμοποιούνται σε αυτό το σενάριο.

* Ανάγνωση πολλές γραμμές σε μια συνάρτηση C#.

    Το περιβάλλον εκτέλεσης συναρτήσεις παρέχει μια `IQueryable<T>` συνδεδεμένο αντικείμενο με τον πίνακα. Τύπος `T` πρέπει να προέρχονται από `TableEntity` ή υλοποίηση `ITableEntity`. Το `partitionKey`, `rowKey`, `filter`, και `take` ιδιότητες δεν χρησιμοποιούνται σε αυτό το σενάριο; Μπορείτε να χρησιμοποιήσετε το `IQueryable` αντικειμένου για να κάνετε οποιαδήποτε φίλτρα απαιτείται. 

* Ανάγνωση πολλές γραμμές σε μια συνάρτηση κόμβο.

    Ορίστε το `filter` και `take` ιδιότητες. Δεν ορίσετε `partitionKey` ή `rowKey`.

* Γράψτε μία ή περισσότερες γραμμές σε μια συνάρτηση C#.

    Το περιβάλλον εκτέλεσης συναρτήσεις παρέχει ένα `ICollector<T>` ή `IAsyncCollector<T>` συνδεδεμένο με τον πίνακα, όπου `T` Καθορίζει το σχήμα των οντοτήτων που θέλετε να προσθέσετε. Συνήθως, πληκτρολογήστε `T` προέρχεται από `TableEntity` ή υλοποιεί `ITableEntity`, αλλά δεν είναι απαραίτητο να. Το `partitionKey`, `rowKey`, `filter`, και `take` ιδιότητες δεν χρησιμοποιούνται σε αυτό το σενάριο.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Παράδειγμα πίνακες χώρου αποθήκευσης: Διαβάστε μια οντότητα ενιαίο πίνακα στο C# ή κόμβου

Το ακόλουθο παράδειγμα κώδικα C# λειτουργεί με το προηγούμενο αρχείο *function.json* φαίνεται παραπάνω για να διαβάσετε μια οντότητα ενιαίο πίνακα. Το μήνυμα ουρά έχει την τιμή του κλειδιού γραμμή και η οντότητα πίνακα είναι για ανάγνωση σε έναν τύπο που καθορίζεται στο αρχείο *run.csx* . Ο τύπος περιλαμβάνει `PartitionKey` και `RowKey` ιδιότητες και δεν προέρχεται από `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Το ακόλουθο παράδειγμα κώδικα F # λειτουργεί επίσης με το προηγούμενο αρχείο *function.json* για να διαβάσετε μια οντότητα ενιαίο πίνακα.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Το ακόλουθο παράδειγμα κώδικα κόμβο λειτουργεί επίσης με το προηγούμενο αρχείο *function.json* για να διαβάσετε μια οντότητα ενιαίο πίνακα.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Παράδειγμα πίνακες χώρου αποθήκευσης: ανάγνωση πολλές οντοτήτων πίνακα στο C# 

Το παρακάτω *function.json* και C# κωδικό οντοτήτων διαβάζει το παράδειγμα για έναν αριθμό-κλειδί διαμερισμάτων που καθορίζεται στο μήνυμα ουρά.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Ο κώδικας C# προσθέτει την αναφορά στο SDK αποθήκευσης Azure έτσι, ώστε ο τύπος οντότητας μπορεί να προέρχεται από `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Παράδειγμα πίνακες χώρου αποθήκευσης: Δημιουργία πίνακα οντοτήτων στο C# 

Το παρακάτω παράδειγμα *function.json* και *run.csx* δείχνει πώς να συντάξετε οντοτήτων πίνακα σε C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Παράδειγμα πίνακες χώρου αποθήκευσης: Δημιουργία πίνακα οντοτήτων στο F#

Το παρακάτω παράδειγμα *function.json* και *run.fsx* δείχνει πώς να συντάξετε οντοτήτων πίνακα στη F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Παράδειγμα πίνακες χώρου αποθήκευσης: δημιουργία μιας οντότητας πίνακα στον κόμβο

Το παρακάτω παράδειγμα *function.json* και *run.csx* δείχνει πώς να συντάξετε μια οντότητα πίνακα στον κόμβο.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 

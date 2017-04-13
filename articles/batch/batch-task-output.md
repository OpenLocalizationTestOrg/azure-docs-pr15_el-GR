<properties
    pageTitle="Έργων και εργασιών εξόδου διατήρησης σε δέσμη Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure χώρου αποθήκευσης, όπως μια διαρκή store δέσμη εργασιών και έργων εξόδου και να ενεργοποιήσετε την προβολή αυτό μόνιμων εξόδου στην πύλη του Azure."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Παραμένει Azure δέσμη έργων και εργασιών εξόδου

Οι εργασίες που εκτελείτε στη δέσμη συνήθως παράγουν εξόδου που πρέπει να αποθηκεύονται και, στη συνέχεια, αργότερα ανακτώνται από άλλες εργασίες της εργασίας, την εφαρμογή-πελάτη που εκτελείται η εργασία ή και τα δύο. Αυτό το αποτέλεσμα μπορεί να είναι αρχεία που έχουν δημιουργηθεί με την επεξεργασία δεδομένων εισαγωγής ή που σχετίζονται με την εκτέλεση εργασιών αρχεία καταγραφής. Σε αυτό το άρθρο παρουσιάζει μια βιβλιοθήκη κλάσης .NET που χρησιμοποιεί μια τεχνική βάσει συμβάσεις διατήρησης αυτών εξόδου εργασίας με το χώρο αποθήκευσης αντικειμένων Blob του Azure, διάθεση ακόμα και μετά τη διαγραφή του χώρους συγκέντρωσης, εργασίες, και τον υπολογισμό κόμβους.

Με την τεχνική σε αυτό το άρθρο, μπορείτε επίσης να προβάλετε το αποτέλεσμα εργασία σε **αρχεία εξόδου αποθηκεύονται** και οι **αποθηκεύονται αρχεία καταγραφής** στην [πύλη του Azure][portal].

![Αρχεία εξόδου αποθηκεύονται και εργαλεία επιλογής Αποθήκευση αρχείων καταγραφής στην πύλη][1]

>[AZURE.NOTE] Τις [Συμβάσεις αρχείων δέσμης Azure] [ nuget_package] βιβλιοθήκη κλάσεων .NET που αναφέρονται σε αυτό το άρθρο είναι αυτήν τη στιγμή στην προεπισκόπηση. Ορισμένες από τις δυνατότητες που περιγράφονται εδώ ενδέχεται να αλλάξουν πριν από την γενικής διαθεσιμότητας.

## <a name="task-output-considerations"></a>Ζητήματα εξόδου εργασιών

Όταν σχεδιάζετε τη λύση σας δέσμη, πρέπει να εξετάσετε αρκετούς παράγοντες που σχετίζονται με εξόδους έργων και εργασιών.

* **Τον υπολογισμό της διάρκειας ζωής κόμβου**: υπολογισμός κόμβοι είναι συχνά μεταβατικές, ειδικά σε με δυνατότητα autoscale χώρους συγκέντρωσης. Το εξόδους από τις εργασίες που εκτελούνται σε έναν κόμβο είναι διαθέσιμες μόνο όταν υπάρχει ο κόμβος και μόνο εντός του χρόνου διατήρησης αρχείων που έχετε ορίσει για την εργασία. Για να εξασφαλίσετε ότι διατηρείται το αποτέλεσμα της εργασίας, τις εργασίες σας, επομένως, πρέπει να αποστείλετε τα αρχεία εξόδου μια διαρκή Store, για παράδειγμα, αποθήκευσης Azure.

* **Χώρος αποθήκευσης εξόδου**: διατήρησης δεδομένων εξόδου εργασιών με το χώρο αποθήκευσης διαρκή, μπορείτε να χρησιμοποιήσετε το [Azure αποθήκευσης SDK](../storage/storage-dotnet-how-to-use-blobs.md) στον κώδικα της εργασίας σας για να αποστείλετε το αποτέλεσμα εργασίας σε ένα κοντέινερ χώρου αποθήκευσης αντικειμένων Blob. Εάν εφαρμόσετε ένα αρχείο συμβάσεις ονομασίας και κοντέινερ, την εφαρμογή-πελάτη ή άλλες εργασίες της εργασίας μπορεί να, στη συνέχεια, εντοπίστε και λήψη αυτό το αποτέλεσμα με βάση τη σύμβαση.

* **Ανάκτηση εξόδου**: μπορείτε να ανακτήσετε εξόδου εργασίας απευθείας από τους κόμβους υπολογισμού του χώρου συγκέντρωσης ή από χώρο αποθήκευσης Azure αν τις εργασίες σας παραμένει τους εξόδου. Για να ανακτήσετε εξόδου μιας εργασίας απευθείας από έναν κόμβο υπολογισμού, χρειάζεστε το όνομα του αρχείου και τη θέση εξόδου στον κόμβο. Αν παραμένει εξόδου σε χώρο αποθήκευσης Azure, ροής εργασιών ή την εφαρμογή-πελάτη πρέπει να έχετε την πλήρη διαδρομή του αρχείου στο χώρο αποθήκευσης Azure για τη λήψη της, χρησιμοποιώντας το SDK Azure χώρου αποθήκευσης.

* **Προβολή εξόδου**: όταν μεταβείτε σε μια εργασία δέσμης στην πύλη του Azure και επιλέξτε **τα αρχεία στον κόμβο**, που παρουσιάζονται με όλα τα αρχεία που σχετίζονται με την εργασία, όχι μόνο τα αρχεία εξόδου που σας ενδιαφέρει. Και πάλι, τα αρχεία σε κόμβους υπολογιστικών είναι διαθέσιμες μόνο όταν υπάρχει ο κόμβος και μόνο εντός του χρόνου διατήρησης αρχείων που έχετε ορίσει για την εργασία. Για να προβάλετε εξόδου εργασιών που έχετε διατηρηθεί με το χώρο αποθήκευσης Azure στην πύλη του ή μια εφαρμογή, όπως την [Εξερεύνηση χώρου αποθήκευσης Azure][storage_explorer], πρέπει να γνωρίζετε τη θέση του και να μεταβείτε απευθείας στο αρχείο.

## <a name="help-for-persisted-output"></a>Βοήθεια για το μόνιμων εξόδου

Για να σας βοηθήσει να περισσότερα εύκολα παραμένει έργων και εργασιών εξόδου, στην ομάδα δέσμης έχει που ορίζονται από το και να υλοποιηθεί ένα σύνολο συμβάσεις ονοματοδοσίας, καθώς και μια βιβλιοθήκη κλάσης .NET, τις [Συμβάσεις αρχείων δέσμης Azure] [ nuget_package] βιβλιοθήκη, που μπορείτε να χρησιμοποιήσετε στις εφαρμογές σας δέσμης. Επιπλέον, Azure είναι η πύλη του υπόψη αυτές τις συμβάσεις ονοματοδοσίας έτσι ώστε να μπορείτε εύκολα να βρείτε τα αρχεία που έχετε αποθηκεύσει χρησιμοποιώντας τη βιβλιοθήκη.

## <a name="using-the-file-conventions-library"></a>Χρήση της βιβλιοθήκης συμβάσεις αρχείου

[Azure συμβάσεις αρχείο δέσμης] [ nuget_package] είναι μια βιβλιοθήκη κλάσης .NET που να χρησιμοποιήσετε τις εφαρμογές σας .NET δέσμη για εύκολα αποθήκευση και ανάκτηση εξόδους εργασίας προς και από το χώρο αποθήκευσης Azure. Προορίζεται για χρήση σε εργασία και προγράμματος-πελάτη κώδικα--στον κώδικα εργασιών για διατήρηση αρχείων και σε κώδικα προγράμματος-πελάτη για τη λίστα και να τις ανακτήσετε. Τις εργασίες σας επίσης να χρησιμοποιήσετε τη βιβλιοθήκη για την ανάκτηση του εξόδους νέα εργασίες, όπως σε ένα σενάριο [εξαρτήσεις εργασιών](batch-task-dependencies.md) .

Η βιβλιοθήκη συμβάσεις αναλαμβάνει εξασφαλίζει ότι χώρους αποθήκευσης και εργασίας εξόδου αρχεία ονομάζονται σύμφωνα με τη σύμβαση και αποστέλλονται στο σωστό μέρος όταν διατηρηθεί με το χώρο αποθήκευσης Azure. Όταν κάνετε ανάκτηση εξόδους, μπορείτε εύκολα να εντοπίσετε το εξόδους για μια δεδομένη έργου ή εργασίας κατά την καταχώρηση ή την ανάκτηση του εξόδους κατά Αναγνωριστικό και σκοπό, αντί να χρειάζεται να γνωρίζετε τα ονόματα αρχείων ή όπου υπάρχει στο χώρο αποθήκευσης.

Για παράδειγμα, μπορείτε να χρησιμοποιήσετε τη βιβλιοθήκη "Εμφάνιση όλων των αρχείων ενδιάμεσου για εργασία 7" ή "Λήψη εμένα η προεπισκόπηση μικρογραφίας για εργασία *mymovie*," χωρίς να χρειάζεται να γνωρίζετε τα ονόματα αρχείων ή τη θέση μέσα στο λογαριασμό σας χώρου αποθήκευσης.

### <a name="get-the-library"></a>Μεταβείτε στη βιβλιοθήκη

Μπορείτε να αποκτήσετε τη βιβλιοθήκη, το οποίο περιέχει νέες κλάσεις και επεκτείνει την [CloudJob] [ net_cloudjob] και [CloudTask] [ net_cloudtask] κλάσεις με νέες μεθόδους, από [NuGet][nuget_package]. Μπορείτε να την προσθέσετε στο έργο σας Visual Studio, χρησιμοποιώντας τη [Διαχείριση πακέτου βιβλιοθήκη NuGet][nuget_manager].

>[AZURE.TIP] Μπορείτε να βρείτε τον [πηγαίο κώδικα] [ github_file_conventions] για τη βιβλιοθήκη συμβάσεις αρχείων δέσμης Azure στην GitHub στο Microsoft Azure SDK για αποθετήριο δεδομένων .NET.

## <a name="requirement-linked-storage-account"></a>Απαίτηση: χώρο αποθήκευσης στο συνδεδεμένο λογαριασμό

Για να αποθηκεύσετε εξόδους με το χώρο αποθήκευσης διαρκή χρήση της βιβλιοθήκης συμβάσεις αρχείο και να τα δείτε στην πύλη του Azure, πρέπει να [σύνδεση αποθήκευσης Azure λογαριασμού](batch-application-packages.md#link-a-storage-account) στο λογαριασμό σας δέσμης. Εάν δεν το έχετε κάνει ήδη, σύνδεση ενός λογαριασμού χώρου αποθήκευσης στο λογαριασμό σας δέσμης, χρησιμοποιώντας την πύλη του Azure:

Blade **δέσμη λογαριασμού** > **Ρυθμίσεις** > **Λογαριασμού χώρου αποθήκευσης** > **Λογαριασμού χώρου αποθήκευσης** (καμία) > Επιλέξτε ένα λογαριασμό του χώρου αποθήκευσης στη συνδρομή σας

Για μια πιο λεπτομερή Γνωρίστε στη σύνδεση ενός λογαριασμού χώρου αποθήκευσης, ανατρέξτε στο θέμα [ανάπτυξη εφαρμογών με το Azure δέσμη πακέτων εφαρμογών](batch-application-packages.md).

## <a name="persist-output"></a>Παραμένει εξόδου

Υπάρχουν δύο κύρια ενέργειες για να εκτελέσετε κατά την αποθήκευση έργων και εργασιών εξόδου με τη βιβλιοθήκη συμβάσεις αρχείου: δημιουργία του κοντέινερ χώρου αποθήκευσης και αποθήκευση εξόδου στο κοντέινερ.

>[AZURE.WARNING] Επειδή όλες τις εκροές έργων και εργασιών είναι αποθηκευμένα στο ίδιο κοντέινερ, [χώρος αποθήκευσης περιορισμού όρια](../storage/storage-performance-checklist.md#blobs) μπορεί να επιβληθούν εάν μεγάλου αριθμού εργασιών που προσπαθούν να παραμένει αρχεία ταυτόχρονα.

### <a name="create-storage-container"></a>Δημιουργία κοντέινερ χώρου αποθήκευσης

Πριν ξεκινήσετε τη μόνιμη εξόδου με το χώρο αποθήκευσης των εργασιών σας, πρέπει να δημιουργήσετε ένα κοντέινερ χώρου αποθήκευσης αντικειμένων blob στο οποίο αυτές θα αποστείλετε το προϊόν τους. Κάντε το εξής καλώντας [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Αυτή η μέθοδος επέκταση λαμβάνει μια [CloudStorageAccount] [ net_cloudstorageaccount] το αντικείμενο ως παράμετρο και δημιουργεί ένα κοντέινερ που ονομάζεται έτσι ώστε τα περιεχόμενά του είναι ανιχνεύσιμα από την πύλη του Azure και τις μεθόδους ανάκτησης που περιγράφονται παρακάτω στο άρθρο.

Συνήθως μπορείτε να τοποθετήσετε αυτόν τον κώδικα στην εφαρμογή υπολογιστή-πελάτη--την εφαρμογή που δημιουργεί το χώρους συγκέντρωσης, εργασίες και εργασίες.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Αποθήκευση εξόδους εργασίας

Τώρα που έχετε προετοιμάσει ένα κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob, εργασίες να αποθηκεύσετε εξόδου στο κοντέινερ, χρησιμοποιώντας το [TaskOutputStorage] [ net_taskoutputstorage] κλάσης βρίσκονται στη βιβλιοθήκη συμβάσεις του αρχείου.

Στον κώδικα της εργασίας, πρώτα να δημιουργήσετε ένα [TaskOutputStorage] [ net_taskoutputstorage] αντικείμενο και, στη συνέχεια, όταν η εργασία έχει ολοκληρωθεί, τις εργασίες, καλέστε την [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] μέθοδο για να αποθηκεύσετε τα αποτελέσματά αποθήκευσης Azure.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Η παράμετρος "εξόδου είδος" κατηγοριοποιεί μόνιμων τα αρχεία. Υπάρχουν τέσσερις προκαθορισμένες [TaskOutputKind] [ net_taskoutputkind] τύποι: "TaskOutput", "TaskPreview", "TaskLog" και "TaskIntermediate." Μπορείτε επίσης να ορίσετε προσαρμοσμένες είδη αν αυτά θα είναι χρήσιμη στη ροή εργασίας σας.

Αυτοί οι τύποι εξόδου σάς επιτρέπουν να καθορίσετε τον τύπο του εξόδους λίστα εργασιών όταν αργότερα ερώτημα δέσμη για το μόνιμων εξόδους από μια συγκεκριμένη εργασία. Με άλλα λόγια, όταν λίστα το εξόδους για μια εργασία, μπορείτε να φιλτράρετε τη λίστα σε έναν από τους τύπους εξόδου. Για παράδειγμα, "να λαμβάνω το αποτέλεσμα *προεπισκόπησης* για εργασία *109*." Περισσότερα στην καταχώρηση και την ανάκτηση εξόδους εμφανίζεται στο [ανάκτηση στοιχείων εξόδου](#retrieve-output) αργότερα στο άρθρο.

>[AZURE.TIP] Το είδος εξόδου αντιστοιχίζει όπου στην πύλη του Azure ένα συγκεκριμένο αρχείο εμφανίζεται επίσης: *TaskOutput*-κατηγοριοποιημένα αρχεία εμφανίζονται σε "Αρχεία εξόδου εργασίας" και *TaskLog* αρχεία εμφανίζονται στα "Αρχεία καταγραφής εργασίας."

### <a name="store-job-outputs"></a>Εξάγει Αποθήκευση εργασίας

Εκτός από την αποθήκευση εξόδους εργασίας, μπορείτε να αποθηκεύσετε το εξόδους που σχετίζεται με ένα σύνολο του έργου. Για παράδειγμα, στην εργασία συγχώνευση μιας εργασίας απόδοσης ταινίας, που μπορεί να εξακολουθούν να εμφανίζονται στην ταινία πλήρως αποδίδονται ως αποτέλεσμα μια εργασία. Όταν ολοκληρωθεί η εργασία σας, την εφαρμογή-πελάτη να λίστα απλώς και να ανακτήσετε την εξόδους για το έργο, και δεν χρειάζεται ερώτημα για τις μεμονωμένες εργασίες.

Αποθήκευση έργου εξόδου καλώντας την [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] μέθοδο, και να καθορίσετε το [JobOutputKind] [ net_joboutputkind] και το όνομα αρχείου:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Όπως με TaskOutputKind για εξόδους εργασίας, μπορείτε να χρησιμοποιήσετε το [JobOutputKind] [ net_joboutputkind] παράμετρο, για να κατηγοριοποιήσετε μια εργασία του διατηρηθεί αρχεία. Αυτή η παράμετρος σας επιτρέπει να νεότερες ερωτήματος για ένα συγκεκριμένο τύπο εξόδου (λίστα). Το JobOutputKind περιλαμβάνει τους τύπους εξόδου και προεπισκόπηση και υποστηρίζει τη δημιουργία προσαρμοσμένων τύπων.

### <a name="store-task-logs"></a>Αποθήκευση αρχείων καταγραφής εργασίας

Εκτός από συνεχίζεται ένα αρχείο με το χώρο αποθήκευσης διαρκή όταν ολοκληρωθεί μια εργασία ή εργασία, μπορεί να είναι απαραίτητο να παραμένει αρχεία που έχουν ενημερωθεί κατά την εκτέλεση μιας εργασίας--αρχεία καταγραφής ή `stdout.txt` και `stderr.txt`, για παράδειγμα. Για το σκοπό αυτό, η βιβλιοθήκη συμβάσεις αρχείων δέσμης Azure παρέχει το [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] μέθοδο. Με [SaveTrackedAsync][net_savetrackedasync], μπορείτε να παρακολούθηση ενημερώσεων σε ένα αρχείο στον κόμβο (στο χρονικό διάστημα που καθορίζετε) και να παραμένει αυτές τις ενημερώσεις για το χώρο αποθήκευσης Azure.

Στο παρακάτω κώδικα, χρησιμοποιούμε [SaveTrackedAsync] [ net_savetrackedasync] για να ενημερώσετε `stdout.txt` στο χώρο αποθήκευσης Azure κάθε 15 δευτερολέπτων κατά την εκτέλεση της εργασίας:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`είναι απλώς ένα σύμβολο κράτησης θέσης για τον κωδικό που κανονικά πρέπει να κάνετε την εργασία σας. Για παράδειγμα, ίσως έχετε κώδικα που κάνει λήψη δεδομένων από το χώρο αποθήκευσης Azure και εκτελεί μετασχηματισμό ή τον υπολογισμό σε αυτό. Το σημαντικό μέρος του τμήματος αυτό είναι που καταδεικνύουν πώς μπορείτε να κάνετε αναδίπλωση όπως κώδικα σε μια `using` μπλοκ για να ενημερώνετε περιοδικά ένα αρχείο με [SaveTrackedAsync][net_savetrackedasync].

Το `Task.Delay` απαιτείται στο τέλος αυτού του `using` μπλοκ για να βεβαιωθείτε ότι ο παράγοντας κόμβου έχει το χρόνο να Εκκαθαρίστε τα περιεχόμενα του τυπική εκτός στο αρχείο stdout.txt στον κόμβο (τον παράγοντα κόμβου είναι ένα πρόγραμμα που εκτελείται σε κάθε κόμβο στο χώρο συγκέντρωσης και παρέχει το περιβάλλον εργασίας εντολών και ελέγχου μεταξύ τον κόμβο και την υπηρεσία δέσμη). Χωρίς αυτήν την καθυστέρηση, είναι δυνατό να χάσετε τα τελευταία μερικά δευτερόλεπτα εξόδου. Αυτή η καθυστέρηση ενδέχεται να μην είναι απαραίτητη για όλα τα αρχεία.

>[AZURE.NOTE] Όταν ενεργοποιείτε αρχείο παρακολούθησης με SaveTrackedAsync, μόνο *τοποθετεί* το αρχείο εντοπισμένες διατηρούνται στο χώρο αποθήκευσης Azure. Χρησιμοποιήστε αυτήν τη μέθοδο μόνο για την παρακολούθηση μη περιστροφή αρχεία καταγραφής ή άλλα αρχεία που θα προσαρτώνται, δηλαδή, δεδομένα προστίθεται στο τέλος του αρχείου μόνο όταν ενημερώνεται.

## <a name="retrieve-output"></a>Ανάκτηση στοιχείων εξόδου

Όταν κάνετε ανάκτηση μόνιμων εξόδου σας χρησιμοποιώντας τη βιβλιοθήκη συμβάσεις αρχείων δέσμης Azure, που κάνετε σε εργασία και εργασία-επίκεντρο τρόπο. Μπορείτε να ζητήσετε την έξοδο για δεδομένο εργασία ή εργασία, χωρίς να χρειάζεται να γνωρίζετε τη διαδρομή στο χώρο αποθήκευσης blob ή ακόμα και το όνομα του αρχείου. Μπορείτε να απλώς πείτε, "Να λαμβάνω τα αρχεία εξόδου για εργασία *109*."

Το παρακάτω τμήμα κώδικα επαναλαμβάνεται τις εργασίες ενός έργου, εκτυπώνεται ορισμένες πληροφορίες σχετικά με τα αρχεία εξόδου για την εργασία και, στη συνέχεια, να κάνει λήψη των αρχείων από χώρο αποθήκευσης.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Εργασίας εξόδους και την πύλη του Azure

Πύλη του Azure εμφανίζει εξόδους εργασίας και αρχεία καταγραφής που διατηρούνται σε ένα συνδεδεμένο λογαριασμό αποθήκευσης Azure χρησιμοποιώντας τις συμβάσεις ονομασίας που βρέθηκε στο το [Αρχείο README συμβάσεις αρχείο δέσμης Azure][github_file_conventions_readme]. Μπορείτε να εφαρμόσετε τον εαυτό σας αυτές τις συμβάσεις σε γλώσσα της επιλογής σας, ή μπορείτε να χρησιμοποιήσετε τη βιβλιοθήκη συμβάσεις αρχείου στις εφαρμογές σας .NET.

### <a name="enable-portal-display"></a>Ενεργοποίηση εμφάνισης πύλης

Για να ενεργοποιήσετε την εμφάνιση των σας εξόδους στην πύλη, πρέπει να πληρούν τις ακόλουθες απαιτήσεις:

 1. [Σύνδεση ενός λογαριασμού αποθήκευσης Azure](#requirement-linked-storage-account) για το λογαριασμό σας δέσμη.
 2. Συμμορφώνεται με τις προκαθορισμένες συνθήκες ονοματοθεσίας για χώρους αποθήκευσης και αρχείων όταν συνεχίζεται εξόδους. Μπορείτε να βρείτε τον ορισμό των αυτές τις συμβάσεις στη βιβλιοθήκη συμβάσεις αρχείο [README][github_file_conventions_readme]. Εάν χρησιμοποιείτε τις [Συμβάσεις αρχείων δέσμης Azure] [ nuget_package] ικανοποιείται βιβλιοθήκη διατήρησης σας εξόδου, αυτή την απαίτηση.

### <a name="view-outputs-in-the-portal"></a>Προβολή εξόδους στην πύλη

Για να προβάλετε αρχεία καταγραφής και εξόδους εργασίας στην πύλη του Azure, μεταβείτε στην εργασία του οποίου εξόδου που σας ενδιαφέρει, στη συνέχεια, κάντε κλικ στην επιλογή **αρχεία εξόδου αποθήκευση** ή **Αποθήκευση αρχείων καταγραφής**. Αυτή η εικόνα εμφανίζει τα **αρχεία εξόδου αποθηκεύονται** για την εργασία με το Αναγνωριστικό "007":

![Blade εξόδους εργασίας στην πύλη του Azure][2]

## <a name="code-sample"></a>Δείγμα κώδικα

Το [PersistOutputs] [ github_persistoutputs] δείγμα έργου είναι ένα από τα [δείγματα κώδικα δέσμης Azure] [ github_samples] σε GitHub. Αυτήν τη λύση Visual Studio 2015 δείχνει πώς να χρησιμοποιήσετε τη βιβλιοθήκη συμβάσεις αρχείων δέσμης Azure διατήρησης εξόδου εργασιών με το χώρο αποθήκευσης διαρκή. Για να εκτελέσετε το δείγμα, ακολουθήστε τα παρακάτω βήματα:

1. Ανοίξτε το έργο στο **Visual Studio 2015**.
2. Προσθέστε δέσμης και αποθήκευσης **διαπιστευτήρια λογαριασμού** **AccountSettings.settings** του Microsoft.Azure.Batch.Samples.Common έργου.
3. **Δημιουργία** (αλλά δεν λειτουργούν) τη λύση. Επαναφορά των πακέτων NuGet Εάν σας ζητηθεί.
4. Χρησιμοποιήστε την πύλη του Azure για να αποστείλετε ένα [πακέτο εφαρμογών](batch-application-packages.md) για το **PersistOutputsTask**. Συμπεριλάβετε το `PersistOutputsTask.exe` και τις εξαρτημένες συγκροτήσεις στο πακέτο .zip, ορίστε το Αναγνωριστικό εφαρμογής για να "PersistOutputsTask" και την έκδοση του πακέτου εφαρμογής για να "1.0".
5. **Έναρξη** έργο (εκτέλεση) το **PersistOutputs** .

## <a name="next-steps"></a>Επόμενα βήματα

### <a name="application-deployment"></a>Ανάπτυξη εφαρμογών

Η δυνατότητα [πακέτων εφαρμογών](batch-application-packages.md) της δέσμης παρέχει έναν εύκολο τρόπο για να αναπτύξετε και τις εφαρμογές που εκτελεί τις εργασίες σας σε υπολογιστική κόμβους έκδοση.

### <a name="installing-applications-and-staging-data"></a>Την εγκατάσταση εφαρμογών και ενδιάμεσου δεδομένων

Ανατρέξτε στο θέμα η [εγκατάσταση των εφαρμογών και ενδιάμεσου σταδίου δεδομένων στη δέσμη τον υπολογισμό κόμβοι] [ forum_post] δημοσίευση στο φόρουμ Azure δέσμη για μια επισκόπηση των διαφόρων μεθόδων προετοιμασία σας κόμβους για την εκτέλεση εργασιών. Αυτήν τη δημοσίευση συνταχθεί από ένα από τα μέλη της ομάδας δέσμη Azure, είναι ένα καλό κύριο βήμα σχετικά με τους διαφορετικούς τρόπους για να μεταφέρετε αρχεία (συμπεριλαμβανομένων των εφαρμογών και δεδομένων εισαγωγής εργασιών) στο σας κόμβους υπολογιστικών, καθώς και ορισμένα ειδικά ζητήματα για κάθε μέθοδο.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Αρχεία εξόδου αποθηκεύονται και εργαλεία επιλογής Αποθήκευση αρχείων καταγραφής στην πύλη"
[2]: ./media/batch-task-output/task-output-02.png "Blade εξόδους εργασίας στην πύλη του Azure"
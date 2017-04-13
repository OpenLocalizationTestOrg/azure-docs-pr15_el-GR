<properties
    pageTitle="Πρόγραμμα εκμάθησης - γρήγορα αποτελέσματα με τη βιβλιοθήκη .NET δέσμη Azure | Microsoft Azure"
    description="Μάθετε τις βασικές έννοιες των δέσμη Azure και τον τρόπο ανάπτυξης για την υπηρεσία δέσμη με ένα σενάριο παράδειγμα."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Γρήγορα αποτελέσματα με τη βιβλιοθήκη Azure δέσμη για .NET

> [AZURE.SELECTOR]
- [.NET](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

Μάθετε τα βασικά στοιχεία της [Δέσμης Azure] [ azure_batch] και του [.NET δέσμη] [ net_api] βιβλιοθήκη σε αυτό το άρθρο καθώς αναλύονται μιας C# δείγμα εφαρμογής βήμα προς βήμα. Θα εξετάσουμε πώς αυτού του δείγματος εφαρμογής αξιοποιεί την υπηρεσία μαζική επεξεργασία μια παράλληλη φόρτο εργασίας στο cloud, καθώς και τον τρόπο αλληλεπίδρασης με το [Χώρο αποθήκευσης Azure](../storage/storage-introduction.md) για το αρχείο ενδιάμεσου σταδίου και ανάκτησης. Θα μάθετε κοινές τεχνικές ροής εργασίας εφαρμογή δέσμης. Μπορείτε επίσης να αποκτήσετε μια βασική κατανόηση τα κύρια στοιχεία της δέσμης, όπως εργασίες, εργασίες, τα σύνολα και τον υπολογισμό κόμβους.

![Μαζική λύση ροής εργασίας (basic)][11]<br/>

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Σε αυτό το άρθρο προϋποθέτει ότι έχετε κάποια εμπειρία C# και Visual Studio. Επίσης, προϋποθέτει ότι είστε μπορούν να πληρούν τις απαιτήσεις δημιουργίας λογαριασμού που έχουν καθοριστεί κάτω από το Azure και τη δέσμη και τις υπηρεσίες αποθήκευσης.

### <a name="accounts"></a>Λογαριασμοί

- **Λογαριασμός Azure**: Εάν δεν έχετε ήδη μια συνδρομή Azure, [Δημιουργήστε έναν δωρεάν λογαριασμό Azure][azure_free_account].
- **Μαζική λογαριασμό**: όταν έχετε μια συνδρομή Azure, [Δημιουργήστε ένα λογαριασμό δέσμη Azure](batch-account-create-portal.md).
- **Το λογαριασμό χώρου αποθήκευσης**: ανατρέξτε στο θέμα [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) στο [Λογαριασμοί Azure σχετικά με το χώρο αποθήκευσης](../storage/storage-create-storage-account.md).

> [AZURE.IMPORTANT] Μαζική υποστηρίζει επί του παρόντος *μόνο* τον τύπο λογαριασμού χώρου αποθήκευσης **γενικής χρήσης** , όπως περιγράφεται στο βήμα #5 [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) στο [Λογαριασμοί Azure σχετικά με το χώρο αποθήκευσης](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

Πρέπει να έχετε **Visual Studio 2015** για να δημιουργήσετε το δείγμα έργου. Μπορείτε να βρείτε δωρεάν και δοκιμαστικές εκδόσεις του Visual Studio στην [Επισκόπηση του Visual Studio 2015 προϊόντα][visual_studio].

### <a name="dotnettutorial-code-sample"></a>Δείγμα κώδικα *DotNetTutorial*

Το [DotNetTutorial] [ github_dotnettutorial] δείγμα είναι ένα από τα πολλά δείγματα κώδικα που βρίσκονται στα [azure-δέσμη-δείγματα] [ github_samples] αποθετήριο δεδομένων σε GitHub. Μπορείτε να κάνετε λήψη του δείγματος, κάνοντας κλικ στο κουμπί **Λήψη ZIP** στην αρχική σελίδα του αποθετηρίου ή κάνοντας κλικ στην επιλογή το [azure-δέσμη-δείγματα-master.zip] [ github_samples_zip] Άμεση λήψη σύνδεσης. Αφού έχετε εξαγάγατε τα περιεχόμενα του αρχείου ZIP, θα βρείτε τη λύση στον παρακάτω φάκελο:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure δέσμη Explorer (προαιρετικά)

Η [Εξερεύνηση δέσμη Azure] [ github_batchexplorer] είναι μια δωρεάν βοηθητικό πρόγραμμα που περιλαμβάνεται στο [azure-δέσμη-δείγματα] [ github_samples] αποθετήριο δεδομένων σε GitHub. Ενώ δεν απαιτείται για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, μπορεί να είναι χρήσιμο κατά την ανάπτυξη και τον εντοπισμό σφαλμάτων σας λύσεις δέσμης.

## <a name="dotnettutorial-sample-project-overview"></a>Επισκόπηση έργου δείγμα DotNetTutorial

Το δείγμα κώδικα *DotNetTutorial* είναι μια λύση Visual Studio 2015 που αποτελείται από δύο έργα: **DotNetTutorial** και **TaskApplication**.

- **DotNetTutorial** είναι η εφαρμογή προγράμματος-πελάτη που αλληλεπιδρά με τη δέσμη και τις υπηρεσίες αποθήκευσης για να εκτελέσετε μια παράλληλη φόρτο εργασίας σε υπολογιστική κόμβους (εικονικές μηχανές). DotNetTutorial εκτελεί τον τοπικό σταθμούς εργασίας.

- **TaskApplication** είναι το πρόγραμμα που εκτελείται σε κόμβους υπολογιστικών στο Azure για να εκτελέσετε την πραγματική εργασία. Στο δείγμα, `TaskApplication.exe` αναλύει το κείμενο σε ένα αρχείο που έχουν ληφθεί από χώρο αποθήκευσης Azure (το αρχείο εισαγωγής). Στη συνέχεια, δημιουργεί ένα αρχείο κειμένου (το αρχείο εξόδου) που περιέχει μια λίστα με τις καλύτερες τρεις λέξεις που εμφανίζονται στο αρχείο εισαγωγής. Αφού που δημιουργεί το αρχείο εξόδου, TaskApplication αποστέλλει το αρχείο με το χώρο αποθήκευσης Azure. Αυτό καθιστά διαθέσιμα για την εφαρμογή-πελάτη για λήψη. TaskApplication εκτελείται παράλληλα σε πολλούς κόμβους υπολογιστικών στην υπηρεσία δέσμης.

Το παρακάτω διάγραμμα παρουσιάζει τα κύρια λειτουργίες που εκτελούνται από την εφαρμογή-πελάτη, *DotNetTutorial*και την εφαρμογή που έχει εκτελεστεί από τις εργασίες, *TaskApplication*. Αυτή η ροή εργασίας βασικές είναι τυπικό πολλές λύσεις υπολογισμού που δημιουργούνται με τη μαζική. Ενώ δεν υποδεικνύει κάθε δυνατότητα που είναι διαθέσιμη στην υπηρεσία δέσμη, σχεδόν κάθε σενάριο δέσμη περιλαμβάνει παρόμοιες διαδικασίες.

![Μαζική παραδείγματος ροής εργασίας][8]<br/>

[**Βήμα 1.**](#step-1-create-storage-containers) Δημιουργία **κοντέινερ** στο χώρο αποθήκευσης Blob του Azure.<br/>
[**Βήμα 2.**](#step-2-upload-task-application-and-data-files) Αποστολή αρχείων εφαρμογής εργασιών και αρχεία εισόδου στο κοντέινερ.<br/>
[**Βήμα 3.**](#step-3-create-batch-pool) Δημιουργήστε μια δέσμη **χώρου συγκέντρωσης**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3α.** Το χώρο συγκέντρωσης **StartTask** στοιχεία λήψης εργασίας δυαδικών αρχείων (TaskApplication) για να τους κόμβους όπως συνδέονται το χώρο συγκέντρωσης.<br/>
[**Βήμα 4.**](#step-4-create-batch-job) Δημιουργήστε μια μαζική **εργασία**.<br/>
[**Βήμα 5.**](#step-5-add-tasks-to-job) Προσθήκη **εργασιών** στο έργο.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5α.** Οι εργασίες προγραμματίζονται να εκτελέσει σε κόμβους.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Κάθε εργασία λήψεις των εισαγωγής δεδομένων από το χώρο αποθήκευσης Azure και, στη συνέχεια, αρχίζει εκτέλεσης.<br/>
[**Βήμα 6.**](#step-6-monitor-tasks) Παρακολούθηση εργασιών.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Κατά την ολοκλήρωση των εργασιών, αποστείλετε τα δεδομένα εξόδου τους με το χώρο αποθήκευσης Azure.<br/>
[**Βήμα 7.**](#step-7-download-task-output) Κάντε λήψη του εξόδου εργασίας από το χώρο αποθήκευσης.

Όπως αναφέρθηκε, εκτελεί αυτά τα ακριβή βήματα δεν κάθε λύση δέσμης και ενδέχεται να περιλαμβάνει πολλά περισσότερα, αλλά το δείγμα εφαρμογής *DotNetTutorial* παρουσιάζει κοινών διαδικασιών που βρίσκονται σε μια λύση δέσμης.

## <a name="build-the-dotnettutorial-sample-project"></a>Δημιουργήστε το έργο δείγμα *DotNetTutorial*

Μπορείτε να εκτελέσετε με επιτυχία το δείγμα, πρέπει να καθορίσετε δέσμης και αποθήκευσης διαπιστευτήρια λογαριασμού στο του έργου *DotNetTutorial* `Program.cs` αρχείου. Εάν δεν το έχετε κάνει ήδη, ανοίξτε τη λύση στο Visual Studio κάνοντας διπλό κλικ το `DotNetTutorial.sln` αρχείο λύσης. Ή να το ανοίξετε από μέσα σε Visual Studio, χρησιμοποιώντας το **Αρχείο > Άνοιγμα > έργου/λύση** μενού.

Άνοιγμα `Program.cs` μέσα στο έργο *DotNetTutorial* . Στη συνέχεια, προσθέστε τα διαπιστευτήριά σας όπως καθορίζεται κοντά στο επάνω μέρος του αρχείου:

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Όπως προαναφέρθηκε, πρέπει να καθορίσετε τη συγκεκριμένη στιγμή τα διαπιστευτήρια για ένα λογαριασμό του χώρου αποθήκευσης **γενικής χρήσης** στο χώρο αποθήκευσης Azure. Οι εφαρμογές σας δέσμη χρησιμοποιούν χώρο αποθήκευσης αντικειμένων blob εντός του λογαριασμού χώρου αποθήκευσης **γενικής χρήσης** . Καθορίζει τα διαπιστευτήρια για ένα λογαριασμό χώρου αποθήκευσης που δημιουργήθηκε, επιλέγοντας τον τύπο του λογαριασμού *χώρος αποθήκευσης αντικειμένων Blob* .

Μπορείτε να βρείτε τη δέσμη και αποθήκευσης λογαριασμού τα διαπιστευτήριά σας εντός του λογαριασμού blade κάθε υπηρεσίας στην [πύλη του Azure][azure_portal]:

![Μαζική διαπιστευτήρια στην πύλη του][9]
![χώρο αποθήκευσης διαπιστευτηρίων στην πύλη][10]<br/>

Τώρα που έχετε ενημερώσει το έργο με τα διαπιστευτήριά σας, κάντε δεξί κλικ της λύσης στο Εξερεύνηση λύσεων και κάντε κλικ στην επιλογή **Δημιουργία λύσης**. Επιβεβαιώστε την επαναφορά του των πακέτων NuGet, εάν σας ζητηθεί.

> [AZURE.TIP] Εάν τα πακέτα NuGet δεν γίνεται αυτόματη επαναφορά ή εάν δείτε τα σφάλματα σχετικά με την αποτυχία για να επαναφέρετε τα πακέτα, βεβαιωθείτε ότι έχετε τη [Διαχείριση πακέτου NuGet] [ nuget_packagemgr] εγκατεστημένο. Στη συνέχεια, ενεργοποιήστε τη λήψη των πακέτων που λείπουν. Ανατρέξτε στο θέμα [Ενεργοποίηση πακέτου επαναφορά κατά τη δημιουργία] [ nuget_restore] για να ενεργοποιήσετε το πακέτο λήψης.

Στις ενότητες που ακολουθούν, θα σας διασπάστε το δείγμα εφαρμογής σε τα βήματα που εκτελεί για να επεξεργαστείτε ένα φόρτο εργασίας στην υπηρεσία δέσμης και συζητήσετε αυτά τα βήματα με λεπτομέρειες. Συνιστάται να αναφέρεται το άνοιγμα λύσης στο Visual Studio κατά την εργασία σας τρόπο στις υπόλοιπες αυτό το άρθρο, επειδή δεν κάθε γραμμή του κώδικα στο δείγμα αναλύεται.

Περιήγηση στο επάνω μέρος του `MainAsync` μέθοδο σε του έργου *DotNetTutorial* `Program.cs` αρχείο για να ξεκινήσετε με το βήμα 1. Κάθε βήμα παρακάτω, στη συνέχεια, περίπου ακολουθεί την πρόοδο της μεθόδου κλήσεις στο `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Βήμα 1: Δημιουργία χώρους αποθήκευσης

![Δημιουργία κοντέινερ στο χώρο αποθήκευσης Azure][1]
<br/>

Μαζική περιλαμβάνει ενσωματωμένη υποστήριξη για αλληλεπίδραση με το χώρο αποθήκευσης Azure. Θα σας δώσει τα αρχεία που απαιτούνται από τις εργασίες που εκτελούνται στο λογαριασμό σας δέσμη κοντέινερ στο λογαριασμό σας στο χώρο αποθήκευσης. Το κοντέινερ παρέχουν επίσης μια θέση για την αποθήκευση των δεδομένων εξόδου που παράγουν τις εργασίες. Το πρώτο πράγμα που κάνει την εφαρμογή-πελάτη *DotNetTutorial* είναι δημιουργία τρεις κοντέινερ στο [Χώρο αποθήκευσης Blob του Azure](../storage/storage-introduction.md):

- **εφαρμογή**: αυτό το κοντέινερ θα αποθηκεύσει την εφαρμογή εκτέλεση με τις εργασίες, καθώς και οποιαδήποτε από τις εξαρτήσεις, όπως βιβλιοθήκες DLL.
- **εισαγωγής**: εργασίες θα κάνετε λήψη των αρχείων δεδομένων για να επαναλάβετε τη διαδικασία από το κοντέινερ *εισαγωγής* .
- **αποτέλεσμα**: όταν εργασίες ολοκλήρωσης Επεξεργασία αρχείου εισαγωγής, αυτά θα αποσταλούν τα αποτελέσματα στο κοντέινερ *εξόδου* .

Για να αλληλεπιδράσετε με ένα λογαριασμό του χώρου αποθήκευσης και δημιουργία κοντέινερ, χρησιμοποιούμε τη [Βιβλιοθήκη προγράμματος-πελάτη του Azure χώρου αποθήκευσης για .NET][net_api_storage]. Μπορούμε να δημιουργήσουμε μια αναφορά με το λογαριασμό με [CloudStorageAccount][net_cloudstorageaccount], και από που να δημιουργήσετε ένα [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Χρησιμοποιούμε τα `blobClient` αναφορά σε ολόκληρη την εφαρμογή και μεταβιβάζουν ως παράμετρο σε διάφορες μεθόδους. Ένα παράδειγμα αυτό είναι στο μπλοκ κώδικα που ακολουθεί αμέσως μετά τα παραπάνω, όπου ονομάζουμε `CreateContainerIfNotExistAsync` για να δημιουργήσετε στην πραγματικότητα τα κοντέινερ.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Αφού δημιουργηθεί το κοντέινερ, η εφαρμογή τώρα να αποστείλετε τα αρχεία που θα χρησιμοποιηθεί από τις εργασίες.

> [AZURE.TIP] [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από το .NET](../storage/storage-dotnet-how-to-use-blobs.md) παρέχει μια επαρκής Επισκόπηση της εργασίας με χώρους αποθήκευσης Azure και αντικείμενα blob. Καθώς αρχίζετε να εργάζεστε με τη μαζική, θα πρέπει να κοντά στην κορυφή της λίστας σας ανάγνωσης.

## <a name="step-2-upload-task-application-and-data-files"></a>Βήμα 2: Αποστολή εργασίας εφαρμογών και αρχείων δεδομένων

![Αποστολή εφαρμογή εργασιών και αρχείων εισαγωγής (δεδομένα) για κοντέινερ][2]
<br/>

Στη λειτουργία αποστολής αρχείου, *DotNetTutorial* ορίζει πρώτα συλλογές της **εφαρμογής** και **εισαγωγής** διαδρομές αρχείων που υπάρχουν στον τοπικό υπολογιστή. Στη συνέχεια, αποστέλλει αυτά τα αρχεία για να τα κοντέινερ που δημιουργήσατε στο προηγούμενο βήμα.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

Υπάρχουν δύο μέθοδοι στο `Program.cs` που εμπλέκονται στη διαδικασία αποστολής:

- `UploadFilesToContainerAsync`: Αυτή η μέθοδος επιστρέφει μια συλλογή από [ResourceFile] [ net_resourcefile] αντικειμένων (αναφέρονται παρακάτω) και εσωτερικά κλήσεις `UploadFileToContainerAsync` για να αποστείλετε κάθε αρχείο που μεταβιβάζεται στην παράμετρο *filePaths* .
- `UploadFileToContainerAsync`: Αυτή είναι η μέθοδος που πραγματικά εκτελεί η αποστολή του αρχείου και δημιουργεί το [ResourceFile] [ net_resourcefile] αντικείμενα. Μετά την αποστολή του αρχείου, λαμβάνει μια υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) για το αρχείο και επιστρέφει ένα αντικείμενο ResourceFile που την αντιπροσωπεύει. Κοινή χρήση υπογραφών περιγράφονται επίσης κάτω από την access.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Μια [ResourceFile] [ net_resourcefile] παρέχει εργασίες στο δέσμη με τη διεύθυνση URL για ένα αρχείο στο χώρο αποθήκευσης Azure που έχουν ληφθεί σε έναν κόμβο υπολογισμού πριν εκτελέσετε αυτήν την εργασία. Το [ResourceFile.BlobSource] [ net_resourcefile_blobsource] ιδιότητα καθορίζει την πλήρη διεύθυνση URL του αρχείου που υπάρχει στο χώρο αποθήκευσης Azure. Η διεύθυνση URL ενδέχεται να περιλαμβάνουν μια υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) που παρέχει ασφαλή πρόσβαση στο αρχείο. Περισσότερους τύπους εργασιών μέσα σε δέσμη .NET περιλαμβάνουν μια ιδιότητα *ResourceFiles* , όπως:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

Το δείγμα εφαρμογής DotNetTutorial δεν χρησιμοποιεί τους τύπους εργασίας JobPreparationTask ή JobReleaseTask, αλλά μπορείτε να διαβάσετε περισσότερα σχετικά με τους στο [εργασία προετοιμασίας και ολοκλήρωση εργασιών στη δέσμη Azure κόμβους τον υπολογισμό](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Κοινόχρηστη πρόσβαση υπογραφή (συσχετισμών Ασφαλείας)

Κοινόχρηστη πρόσβαση υπογραφές είναι συμβολοσειρές που — όταν συμπεριλαμβάνεται ως τμήμα μιας διεύθυνσης URL — Δώστε ασφαλή πρόσβαση σε κοντέινερ και αντικείμενα blob στο χώρο αποθήκευσης Azure. Το DotNetTutorial εφαρμογή χρησιμοποιεί blob και κοινή χρήση κοντέινερ πρόσβαση υπογραφή διευθύνσεις URL και παρουσιάζει πώς μπορείτε να λάβετε αυτές τις συμβολοσειρές υπογραφή κοινόχρηστη πρόσβαση από την υπηρεσία αποθήκευσης.

- **BLOB θέσει σε κοινή χρήση υπογραφών πρόσβασης**: το χώρο συγκέντρωσης StartTask στο DotNetTutorial χρησιμοποιεί υπογραφές access blob θέσει σε κοινή χρήση κατά τη λήψη τα δυαδικά αρχεία εφαρμογών και αρχείων εισαγωγής δεδομένων από το χώρο αποθήκευσης (ανατρέξτε στο βήμα #3 παρακάτω). Το `UploadFileToContainerAsync` μέθοδο σε του DotNetTutorial `Program.cs` περιέχει τον κωδικό που λαμβάνει κάθε blob κοινόχρηστη πρόσβαση υπογραφής. Αυτό γίνεται με κλήση [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Κοντέινερ θέσει σε κοινή χρήση υπογραφών πρόσβασης**: όπως κάθε εργασία ολοκληρώσει την εργασία στον κόμβο του υπολογισμού, αποστέλλει το αρχείο εξόδου στο κοντέινερ *εξόδου* στο χώρο αποθήκευσης Azure. Για να το κάνετε, TaskApplication χρησιμοποιεί μια υπογραφή κοινόχρηστη πρόσβαση κοντέινερ που παρέχει πρόσβαση εγγραφής στο κοντέινερ ως μέρος της διαδρομής κατά την αποστολή του αρχείου. Απόκτηση την υπογραφή κοινόχρηστη πρόσβαση κοντέινερ γίνεται με παρόμοιο τρόπο ως όταν απόκτηση του blob σε κοινή χρήση access υπογραφής. Στο DotNetTutorial, θα παρατηρήσετε ότι το `GetContainerSasUrl` μέθοδο Βοήθειας κλήσεις [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] για να το κάνετε. Θα μπορείτε να διαβάσετε περισσότερα σχετικά με τον τρόπο TaskApplication χρησιμοποιεί το κοντέινερ κοινόχρηστα υπογραφή πρόσβαση στο "βήμα 6: εργασίες παρακολούθησης."

> [AZURE.TIP] Ελέγξτε τη σειρά δύο τμημάτων στην κοινόχρηστη πρόσβαση υπογραφές, [μέρος 1: Κατανόηση του μοντέλου του υπογραφή (συσχετισμών Ασφαλείας) κοινόχρηστη πρόσβαση](../storage/storage-dotnet-shared-access-signature-part-1.md) και [μέρος 2: δημιουργία και χρήση υπογραφής κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) με το χώρο αποθήκευσης αντικειμένων Blob](../storage/storage-dotnet-shared-access-signature-part-2.md), για να μάθετε περισσότερα σχετικά με την παροχή ασφαλή πρόσβαση σε δεδομένα στο λογαριασμό σας στο χώρο αποθήκευσης.

## <a name="step-3-create-batch-pool"></a>Βήμα 3: Δημιουργία δέσμης συγκέντρωσης

![Δημιουργία χώρου συγκέντρωσης δέσμης][3]
<br/>

Μια δέσμη **χώρου συγκέντρωσης** είναι μια συλλογή από κόμβους υπολογιστικών (εικονικές μηχανές) στην οποία δέσμη εκτελεί τις εργασίες ενός έργου.

Αφού το μεταφέρει τα αρχεία εφαρμογών και δεδομένων με το λογαριασμό χώρου αποθήκευσης, *DotNetTutorial* ξεκινά την επικοινωνία με την υπηρεσία δέσμης χρησιμοποιώντας τη βιβλιοθήκη δέσμη .NET. Για να το κάνετε αυτό, μια [BatchClient] [ net_batchclient] δημιουργείται για πρώτη φορά:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Στη συνέχεια, δημιουργείται ένα χώρο συγκέντρωσης κόμβους υπολογιστικών στο λογαριασμό δέσμη με μια κλήση για να `CreatePoolAsync`. `CreatePoolAsync`χρησιμοποιεί το [BatchClient.PoolOperations.CreatePool] [ net_pool_create] μέθοδο για να δημιουργήσετε ένα χώρο συγκέντρωσης στην υπηρεσία δέσμης.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Όταν δημιουργείτε ένα χώρο συγκέντρωσης με [CreatePool][net_pool_create], μπορείτε να καθορίσετε πολλές παραμέτρους όπως τον αριθμό των κόμβους υπολογιστικών, το [μέγεθος του τους κόμβους](../cloud-services/cloud-services-sizes-specs.md)και τους κόμβους λειτουργικό σύστημα. Σε *DotNetTutorial*, μπορούμε να χρησιμοποιήσουμε [CloudServiceConfiguration] [ net_cloudserviceconfiguration] για να καθορίσετε Windows Server 2012 R2 από [Τις υπηρεσίες Cloud](../cloud-services/cloud-services-guestos-update-matrix.md). Ωστόσο, καθορίζοντας μια [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] αντί για αυτό, μπορείτε να δημιουργήσετε χώρους συγκέντρωσης του κόμβους που δημιουργούνται από τις εικόνες Marketplace, που περιλαμβάνει εικόνες των Windows και Linux — ανατρέξτε στο θέμα [Linux παροχή τον υπολογισμό τους κόμβους χώρους συγκέντρωσης Azure δέσμη](batch-linux-nodes.md) για περισσότερες πληροφορίες.

> [AZURE.IMPORTANT] Που θα χρεωθείτε για τους πόρους υπολογισμού σε δέσμη. Για να ελαχιστοποιήσετε το κόστος, μπορείτε να μειώσετε `targetDedicated` σε 1 πριν να εκτελέσετε το δείγμα.

Μαζί με αυτές τις ιδιότητες φυσικό κόμβο, μπορείτε επίσης να καθορίσετε μια [StartTask] [ net_pool_starttask] για το χώρο συγκέντρωσης. Το StartTask εκτελείται σε κάθε κόμβο όπως αυτόν τον κόμβο συνδέει το χώρο συγκέντρωσης και κάθε φορά που γίνεται επανεκκίνηση έναν κόμβο. Το StartTask είναι ιδιαίτερα χρήσιμη για την εγκατάσταση εφαρμογών σε κόμβους υπολογιστικών πριν από την εκτέλεση των εργασιών. Για παράδειγμα, εάν τις εργασίες σας επεξεργάζεται δεδομένα χρησιμοποιώντας Python δέσμες ενεργειών, μπορείτε να χρησιμοποιήσετε μια StartTask εγκατάστασης Python σε κόμβους υπολογιστικών.

Σε αυτήν την εφαρμογή δείγματος, το StartTask αντιγράφει τα αρχεία που να κάνει λήψη από το χώρο αποθήκευσης (που καθορίζονται, χρησιμοποιώντας το [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] ιδιότητα) από τον κατάλογο εργασίας StartTask στον κοινόχρηστο κατάλογο που να αποκτήσετε πρόσβαση σε *όλες* τις εργασίες που εκτελούνται στον κόμβο. Ουσιαστικά, αυτή η ενέργεια αντιγράφει `TaskApplication.exe` τις εξαρτήσεις στον κοινόχρηστο κατάλογο σε κάθε κόμβο όπως ο κόμβος συνδέει το χώρο συγκέντρωσης, έτσι ώστε οι εργασίες που εκτελούνται στον κόμβο μπορούν να έχουν πρόσβαση σε αυτήν.

> [AZURE.TIP] Η δυνατότητα **πακέτων εφαρμογών** της δέσμης Azure παρέχει ένας άλλος τρόπος για την εφαρμογή σας σε κόμβους υπολογιστικών σε ένα χώρο συγκέντρωσης. Για λεπτομέρειες, ανατρέξτε στο θέμα [ανάπτυξη εφαρμογών με το Azure δέσμη πακέτων εφαρμογών](batch-application-packages.md) .

Δυνατότητα σημείωσης στο επάνω τμήμα κώδικα είναι επίσης τη χρήση των δύο μεταβλητές περιβάλλοντος στην ιδιότητα *εντολών* του StartTask: `%AZ_BATCH_TASK_WORKING_DIR%` και `%AZ_BATCH_NODE_SHARED_DIR%`. Κάθε κόμβος μέσα σε ένα χώρο συγκέντρωσης δέσμη ρυθμίζεται αυτόματα με πολλές μεταβλητές περιβάλλοντος που είναι ειδικές για μαζική. Κάθε διεργασίας που εκτελείται από μια εργασία έχει πρόσβαση σε αυτές τις μεταβλητές περιβάλλοντος.

> [AZURE.TIP] Για να μάθετε περισσότερα σχετικά με το περιβάλλον μεταβλητές που είναι διαθέσιμες σε κόμβους υπολογιστικών σε χώρο συγκέντρωσης δέσμης και πληροφορίες σχετικά με την εργασία εργασία σε καταλόγους, ανατρέξτε στις ενότητες [ρυθμίσεων περιβάλλοντος για τις εργασίες](batch-api-basics.md#environment-settings-for-tasks) και [τα αρχεία και τους καταλόγους](batch-api-basics.md#files-and-directories) στην [επισκόπηση δυνατοτήτων δέσμης για τους προγραμματιστές](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Βήμα 4: Δημιουργία μαζική εργασία

![Δημιουργία μαζική εργασία][4]<br/>

Μια μαζική **εργασία** είναι μια συλλογή των εργασιών και συσχετίζεται με ένα χώρο συγκέντρωσης κόμβους υπολογιστικών. Εκτέλεση των εργασιών σε ένα έργο σε κόμβους υπολογιστικών το σχετικό χώρο συγκέντρωσης.

Μπορείτε να χρησιμοποιήσετε μια εργασία, όχι μόνο για την οργάνωση και την παρακολούθηση των εργασιών στο σχετικό φόρτους εργασίας, αλλά και για την επιβολή ορισμένους περιορισμούς--όπως τον μέγιστο χρόνο εκτέλεσης του έργου (και από την επέκταση, τις εργασίες) καθώς και προτεραιότητα έργων σχέση με άλλες εργασίες στο λογαριασμό δέσμης. Σε αυτό το παράδειγμα, ωστόσο, η εργασία σχετίζεται μόνο με το χώρο συγκέντρωσης που δημιουργήθηκε στο βήμα #3. Πρόσθετες ιδιότητες δεν έχουν ρυθμιστεί.

Όλες οι εργασίες δέσμη σχετίζονται με ένα συγκεκριμένο σύνολο. Αυτή η συσχέτιση δηλώνει ποιες εργασίες του έργου θα εκτελεστεί σε κόμβους. Που καθορίζετε, χρησιμοποιώντας το [CloudJob.PoolInformation] [ net_job_poolinfo] ιδιότητα, όπως φαίνεται στην παρακάτω τμήμα κώδικα.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Τώρα που έχει δημιουργηθεί μια εργασία, εργασίες προστίθενται για να εκτελέσετε την εργασία.

## <a name="step-5-add-tasks-to-job"></a>Βήμα 5: Προσθήκη εργασιών έργου

![Προσθήκη εργασιών στο έργο][5]<br/>
*(1) εργασίες προστίθενται στην εργασία, (2) οι εργασίες είναι προγραμματισμένες για να εκτελέσετε σε κόμβους και (3) τις εργασίες, κάντε λήψη των αρχείων δεδομένων για την επεξεργασία*

Μαζική **εργασίες** είναι οι επιμέρους μονάδες της εργασίας που εκτελούνται σε κόμβους υπολογιστικών. Μια εργασία έχει μια γραμμή εντολών και εκτελεί τις δέσμες ενεργειών ή εκτελέσιμα αρχεία που καθορίζετε σε αυτήν τη γραμμή εντολών.

Για να εκτελούν στην πραγματικότητα μια εργασία, πρέπει να προστεθεί εργασίες σε μια εργασία. Κάθε [CloudTask] [ net_task] ρυθμίζονται χρησιμοποιώντας μια ιδιότητα της γραμμής εντολών και [ResourceFiles] [ net_task_resourcefiles] (όπως με το χώρο συγκέντρωσης StartTask) που η εργασία λήψεις στον κόμβο πριν από τη γραμμή εντολών εκτελείται αυτόματα. Στο δείγμα έργο *DotNetTutorial* , κάθε εργασία επεξεργάζεται μόνο ένα αρχείο. Έτσι, η συλλογή ResourceFiles περιέχει ένα στοιχείο.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Όταν πρόσβαση μεταβλητές περιβάλλοντος όπως `%AZ_BATCH_NODE_SHARED_DIR%` ή η εκτέλεση μιας εφαρμογής που δεν βρίσκονται σε στον κόμβο `PATH`, πρέπει να έχει το πρόθεμα γραμμές εντολών εργασιών `cmd /c`. Ρητά θα εκτελέσει το πρόγραμμα μεταγλώττισης εντολών και πείτε τερματισμό μετά την εκτέλεση εντολής σας. Αυτή η απαίτηση δεν είναι απαραίτητη εάν τις εργασίες σας εκτέλεση μιας εφαρμογής από τον κόμβο `PATH` (όπως *robocopy.exe* ή *powershell.exe*) και χρησιμοποιούνται χωρίς μεταβλητές περιβάλλοντος.

Εντός του `foreach` βρόχο στο τμήμα κώδικα παραπάνω, μπορείτε να δείτε ότι έχει συνταχθεί τη γραμμή εντολών για την εργασία ώστε να μεταβιβάζονται τρία ορίσματα γραμμής εντολών για να *TaskApplication.exe*:

1. Το **πρώτο όρισμα** είναι η διαδρομή του αρχείου για επεξεργασία. Αυτή είναι η τοπική διαδρομή προς το αρχείο που υπάρχει στον κόμβο. Όταν το ResourceFile αντικείμενο στο `UploadFileToContainerAsync` δημιουργήθηκε πρώτα άνω, το όνομα του αρχείου που χρησιμοποιήθηκε για αυτήν την ιδιότητα (ως παράμετρο στην κατασκευή ResourceFile). Αυτό υποδεικνύει ότι το αρχείο μπορεί να βρεθεί στον ίδιο κατάλογο ως *TaskApplication.exe*.

2. Το **δεύτερο όρισμα** Καθορίζει ότι οι καλύτερες λέξεις *N* πρέπει να γράφονται με το αρχείο εξόδου. Στο δείγμα, αυτό είναι σχεδιασμένο ώστε να τις τρεις λέξεις επάνω εγγράφονται στο αρχείο εξόδου.

3. Το **τρίτο όρισμα** είναι η υπογραφή κοινόχρηστη πρόσβαση (συσχετισμών Ασφαλείας) που παρέχει πρόσβαση εγγραφής στο κοντέινερ **εξόδου** στο χώρο αποθήκευσης Azure. *TaskApplication.exe* χρησιμοποιεί αυτήν τη διεύθυνση URL υπογραφή κοινόχρηστη πρόσβαση, όταν το στέλνει το αρχείο εξόδου αποθήκευσης Azure. Μπορείτε να βρείτε τον κώδικα για αυτό στο το `UploadFileToContainer` μέθοδο σε του έργου TaskApplication `Program.cs` αρχείο:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Βήμα 6: Παρακολούθηση εργασιών

![Εργασίες παρακολούθησης][6]<br/>
*Την εφαρμογή-πελάτη (1) παρακολουθεί τις εργασίες για την ολοκλήρωση και κατάσταση επιτυχίας και (2) οι εργασίες αποστολή δεδομένων αποτέλεσμα στο χώρο αποθήκευσης Azure*

Όταν εργασίες προστίθενται σε μια εργασία, είναι αυτόματα στην ουρά και έχει προγραμματιστεί για εκτέλεση σε κόμβους υπολογιστικών εντός του χώρου συγκέντρωσης που σχετίζονται με την εργασία. Με βάση τις ρυθμίσεις που καθορίζετε, δέσμη χειρίζεται όλα ουράς εργασιών, τον προγραμματισμό, επανάληψη και άλλα καθήκοντα διαχείρισης εργασιών για εσάς. Υπάρχουν πολλά προσεγγίσεις για την παρακολούθηση εργασιών εκτέλεσης. DotNetTutorial εμφανίζει ένα απλό παράδειγμα που αναφέρει μόνο αποτυχία ολοκλήρωσης και εργασιών ή επιτυχίας μέλη.

Μέσα σε το `MonitorTasks` μέθοδο σε του DotNetTutorial `Program.cs`, υπάρχουν τρία έννοιες δέσμη .NET που δικαιολογούν συζήτησης. Να παρατίθενται κάτω από το στοιχείο με τη σειρά τους της εμφάνισης της:

1. **ODATADetailLevel**: Καθορισμός [ODATADetailLevel] [ net_odatadetaillevel] στη λίστα είναι απαραίτητες για ταυτόχρονα εξασφαλίζετε ότι οι επιδόσεις εφαρμογής δέσμη λειτουργίες (όπως λήψη μιας λίστας εργασιών του έργου). Προσθήκη [ερωτήματος στην υπηρεσία δέσμη Azure αποτελεσματικά](batch-efficient-list-queries.md) στη λίστα ανάγνωσης εάν σκοπεύετε να κάνουν οποιοδήποτε είδος κατάσταση παρακολούθησης μέσα στις εφαρμογές σας δέσμης.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] παρέχει στις εφαρμογές .NET δέσμη βοηθητικά προγράμματα Βοήθειας για την παρακολούθηση εργασιών μέλη. Στο `MonitorTasks`, *DotNetTutorial* περιμένει για όλες τις εργασίες για την επίτευξη [TaskState.Completed] [ net_taskstate] εντός χρονικού ορίου. Στη συνέχεια, τερματίζεται η εργασία.

3. **TerminateJobAsync**: τον τερματισμό μιας εργασίας με [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (ή το αποκλεισμού JobOperations.TerminateJob) επισημαίνει την εργασία ως ολοκληρωμένη. Είναι σημαντικό να το κάνετε εάν η λύση δέσμης χρησιμοποιεί μια [JobReleaseTask][net_jobreltask]. Αυτό είναι ένας ειδικός τύπος της εργασίας, η οποία περιγράφεται στις [εργασίες προετοιμασίας και ολοκλήρωση του έργου](batch-job-prep-release.md).

Το `MonitorTasks` μέθοδο από *DotNetTutorial*του `Program.cs` εμφανίζεται κάτω από:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Βήμα 7: Λήψη εξόδου εργασίας

![Λήψη εξόδου εργασίας από το χώρο αποθήκευσης][7]<br/>

Τώρα που η εργασία έχει ολοκληρωθεί, το αποτέλεσμα από τις εργασίες μπορούν να ληφθούν από χώρο αποθήκευσης Azure. Αυτό γίνεται με μια κλήση σε `DownloadBlobsFromContainerAsync` στο *DotNetTutorial*του `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Την κλήση σε `DownloadBlobsFromContainerAsync` στο το *DotNetTutorial* εφαρμογή Καθορίζει ότι θα πρέπει να ληφθούν τα αρχεία στο σας `%TEMP%` φακέλου. Είστε ευπρόσδεκτοι να τροποποιήσετε αυτήν τη θέση εξόδου.

## <a name="step-8-delete-containers"></a>Βήμα 8: Διαγραφή κοντέινερ

Επειδή που θα χρεωθείτε για τα δεδομένα που βρίσκονται στο χώρο αποθήκευσης Azure, είναι πάντα καλή ιδέα να καταργήσετε οποιαδήποτε αντικείμενα BLOB που δεν χρειάζονται πλέον για τις μαζικές εργασίες. Στο του DotNetTutorial `Program.cs`, αυτό γίνεται με τρεις κλήσεις στη μέθοδο της Βοήθειας `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Τη μέθοδο του εαυτού απλώς λαμβάνει μια αναφορά στο κοντέινερ και, στη συνέχεια, καλεί [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Βήμα 9: Διαγραφή της εργασίας και το χώρο συγκέντρωσης

Στο τελευταίο βήμα, για να διαγράψετε την εργασία και το χώρο συγκέντρωσης που δημιουργήθηκαν από την εφαρμογή DotNetTutorial ζητείται από το χρήστη. Παρόλο που δεν θα χρεωθείτε για εργασίες και τις εργασίες τους, *είναι* χρεωθείτε για κόμβους υπολογιστικών. Επομένως, συνιστάται να που θα εκχωρηθεί κόμβους μόνο σύμφωνα με τις ανάγκες. Διαγραφή που δεν χρησιμοποιούνται χώρους συγκέντρωσης μπορεί να είναι μέρος της διαδικασίας συντήρησης.

Το BatchClient [JobOperations] [ net_joboperations] και [PoolOperations] [ net_pooloperations] και τα δύο έχουν αντίστοιχες μέθοδοι διαγραφής, που ονομάζονται αν ο χρήστης επιβεβαιώσει διαγραφής:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Έχετε υπόψη ότι θα χρεωθείτε για τους πόρους υπολογισμού — χώρους συγκέντρωσης που δεν χρησιμοποιείται η διαγραφή θα ελαχιστοποιήσει κόστος. Επίσης, πρέπει να γνωρίζετε ότι η διαγραφή ενός χώρου συγκέντρωσης διαγράφει όλους τους κόμβους υπολογισμού μέσα σε αυτόν το χώρο συγκέντρωσης και ότι όλα τα δεδομένα τους κόμβους θα μπορούν να ανακτηθούν μετά από τη διαγραφή του χώρου συγκέντρωσης.

## <a name="run-the-dotnettutorial-sample"></a>Εκτελέστε το δείγμα *DotNetTutorial*

Όταν εκτελείτε το δείγμα εφαρμογής, το αποτέλεσμα κονσόλας θα είναι παρόμοιο με το ακόλουθο. Κατά την εκτέλεση, θα αντιμετωπίσετε μια παύση στο `Awaiting task completion, timeout in 00:30:00...` ενώ ξεκινούν κόμβους υπολογιστικών το χώρο συγκέντρωσης. Χρήση του [Azure πύλη] [ azure_portal] για την παρακολούθηση του χώρου συγκέντρωσης, τον υπολογισμό κόμβους, έργων και εργασιών κατά τη διάρκεια και μετά την εκτέλεση. Χρήση του [Azure πύλη] [ azure_portal] ή την [Εξερεύνηση χώρου αποθήκευσης Azure] [ storage_explorers] για να προβάλετε τους πόρους αποθήκευσης (κοντέινερ και αντικείμενα BLOB) που δημιουργούνται από την εφαρμογή.

Τυπικές χρόνος εκτέλεσης είναι **περίπου 5 λεπτά** , όταν εκτελείτε την εφαρμογή με την προεπιλεγμένη ρύθμιση παραμέτρων.

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Επόμενα βήματα

Είστε ευπρόσδεκτοι να κάνετε αλλαγές σε *DotNetTutorial* και *TaskApplication* για να πειραματιστείτε με διαφορετικές υπολογισμού σενάρια. Για παράδειγμα, δοκιμάστε να προσθέσετε μια καθυστέρηση εκτέλεσης για *TaskApplication*, όπως με [Thread.Sleep][net_thread_sleep], για να προσομοιώσετε μεγάλη διάρκεια εκτέλεσης εργασιών και να παρακολουθείτε τους στην πύλη του. Δοκιμάστε να προσθέσετε περισσότερες εργασίες ή να προσαρμόσετε τον αριθμό των κόμβους υπολογιστικών. Προσθήκη λογικής ελέγχου και να επιτρέψετε τη χρήση ενός υπάρχοντος χώρου συγκέντρωσης ταχύτητα χρόνο εκτέλεσης (*υποδείξεων*: ανάληψη ελέγχου `ArticleHelpers.cs` στο το [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] έργο στο [azure-δέσμη-δείγματα][github_samples]).

Τώρα που είστε εξοικειωμένοι με τη βασική ροή εργασίας μιας λύσης δέσμης, είναι ώρα για να ψάξετε στις πρόσθετες δυνατότητες της υπηρεσίας δέσμης.

- Διαβάστε την [επισκόπηση δυνατοτήτων δέσμης για τους προγραμματιστές](batch-api-basics.md), που συνιστάται για όλους τους νέους χρήστες δέσμης.
- Έναρξη στην τα άλλα άρθρα ανάπτυξης δέσμη στην περιοχή **ανάπτυξης αναλυτικά** κατά τη [διαδικασία εκμάθησης δέσμη][batch_learning_path].
- Δείτε μια διαφορετική υλοποίηση της επεξεργασίας το φόρτο εργασίας "επάνω Ν τις λέξεις" με χρήση δέσμης στο το [TopNWords] [ github_topnwords] δείγμα.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Δημιουργία κοντέινερ στο χώρο αποθήκευσης Azure"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Αποστολή εφαρμογή εργασιών και αρχείων εισαγωγής (δεδομένα) για κοντέινερ"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Δημιουργία χώρου συγκέντρωσης δέσμης"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Δημιουργία μαζική εργασία"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Προσθήκη εργασιών στο έργο"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Εργασίες παρακολούθησης"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Λήψη εξόδου εργασίας από το χώρο αποθήκευσης"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Μαζική λύση ροής εργασίας (πλήρες διάγραμμα)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Μαζική διαπιστευτήρια στην πύλη"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Χώρος αποθήκευσης διαπιστευτηρίων στην πύλη"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Μαζική λύση ροής εργασίας (ελάχιστους διάγραμμα)"

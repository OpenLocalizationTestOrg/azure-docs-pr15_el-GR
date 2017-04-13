<properties
    pageTitle="Δημιουργία ευρετηρίου αρχεία πολυμέσων με ευρετήριο πολυμέσων Azure"
    description="Azure Media ευρετηρίου σας δίνει τη δυνατότητα για να κάνετε περιεχόμενο από τα αρχεία πολυμέσων με δυνατότητα αναζήτησης και για να δημιουργήσετε ένα αντίγραφο πλήρους κειμένου για κλειστές λεζάντες και λέξεις-κλειδιά. Αυτό το θέμα εξηγεί τον τρόπο χρήσης του ευρετηρίου πολυμέσων."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Δημιουργία ευρετηρίου αρχεία πολυμέσων με ευρετήριο πολυμέσων Azure


Azure Media ευρετηρίου σας δίνει τη δυνατότητα για να κάνετε περιεχόμενο από τα αρχεία πολυμέσων με δυνατότητα αναζήτησης και για να δημιουργήσετε ένα αντίγραφο πλήρους κειμένου για κλειστές λεζάντες και λέξεις-κλειδιά. Μπορείτε να επεξεργαστείτε ένα πολυμέσων αρχείου ή πολλών αρχείων πολυμέσων σε μια δέσμη.  

>[AZURE.IMPORTANT] Κατά τη δημιουργία ευρετηρίου περιεχομένου, βεβαιωθείτε ότι χρησιμοποιείτε αρχεία πολυμέσων που έχουν πολύ Απαλοιφή ομιλίας (χωρίς φόντο μουσικής, θορύβου, εφέ ή hiss μικρόφωνο). Ορισμένα παραδείγματα κατάλληλο περιεχόμενο είναι: καταγραφεί συσκέψεις, διαλέξεις ή παρουσιάσεις. Το παρακάτω περιεχόμενο ενδέχεται να μην είναι κατάλληλη για τη δημιουργία ευρετηρίου: ταινίες, εμφανίζει Τηλεοπτική, όλα με μεικτή ήχου και ηχητικά εφέ, καταγραφεί ακατάλληλα περιεχομένου με θόρυβο (hiss).


Μια εργασία δημιουργίας ευρετηρίου μπορεί να δημιουργήσει το παρακάτω εξόδους:

- Κλειστή λεζάντα αρχεία με τις παρακάτω μορφές: **ΣΆΜΙ**, **TTML**και **WebVTT**.

    Αρχεία κλειστές λεζάντες περιλαμβάνουν μια ετικέτα που ονομάζεται Recognizability, των βαθμών που είναι μια εργασία δημιουργίας ευρετηρίου με βάση τον τρόπο αναγνωρίσιμη την ομιλία στο βίντεο προέλευσης είναι.  Μπορείτε να χρησιμοποιήσετε την τιμή της Recognizability σε αρχεία εξόδου οθόνης για χρηστικότητα. Μια χαμηλή βαθμολογία σημαίνει περιορισμένα αποτελέσματα δημιουργίας ευρετηρίου λόγω ποιότητα ήχου.
- Αρχείο λέξεων-κλειδιών (XML).
- Δημιουργία ευρετηρίου αρχείο blob (AIB) για χρήση με τον SQL server ήχου.

    Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση AIB αρχείων με ευρετήριο πολυμέσων Azure και SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


Αυτό το θέμα δείχνει πώς μπορείτε να δημιουργήσετε εργασιών ευρετηρίου **ευρετηρίου ενός περιουσιακού στοιχείου** και **ευρετηρίου πολλών αρχείων**.

Για τις πιο πρόσφατες ενημερώσεις ευρετηρίου πολυμέσων Azure, ανατρέξτε στο θέμα [ιστολόγια Media Services](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Χρήση αρχείων ρύθμισης παραμέτρων και δηλωτικό για τη δημιουργία ευρετηρίου εργασίες

Μπορείτε να καθορίσετε περισσότερες λεπτομέρειες για τις εργασίες σας δημιουργίας ευρετηρίου χρησιμοποιώντας μια ρύθμιση παραμέτρων της εργασίας. Για παράδειγμα, μπορείτε να καθορίσετε ποια μετα-δεδομένων για να χρησιμοποιήσετε για το αρχείο πολυμέσων σας. Αυτό μετα-δεδομένων χρησιμοποιούνται από το μηχανισμό γλώσσα για να αναπτύξετε το λεξιλόγιο και σημαντικά βελτιώνει την ακρίβεια της αναγνώρισης ομιλίας.  Επίσης, θα μπορείτε να καθορίσετε τα αρχεία σας επιθυμητό αποτέλεσμα.

Μπορείτε επίσης να επεξεργαστείτε ταυτόχρονα πολλαπλά αρχεία πολυμέσων, χρησιμοποιώντας ένα αρχείο δηλώσεων.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εργασία προκαθορισμένα για Azure Media ευρετηρίου](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Δημιουργία ευρετηρίου ενός περιουσιακού στοιχείου

Την ακόλουθη μέθοδο αποστέλλει ένα αρχείο πολυμέσων ως ενός περιουσιακού στοιχείου και δημιουργεί μια εργασία για να δημιουργήσετε ευρετήριο περιουσιακού στοιχείου.

Λάβετε υπόψη ότι εάν έχει καθοριστεί κανένα αρχείο ρύθμισης παραμέτρων, το αρχείο θα είναι στο ευρετήριο με όλες τις προεπιλεγμένες ρυθμίσεις.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Αρχεία εξόδου

Από προεπιλογή, μια εργασία δημιουργίας ευρετηρίου δημιουργεί τα παρακάτω αρχεία εξόδου. Τα αρχεία θα αποθηκευτούν στο πρώτο παγίου εξόδου.

Όταν υπάρχουν περισσότερα από ένα αρχείο εισαγωγής πολυμέσων, ευρετηρίου θα δημιουργήσει ένα αρχείο δηλώσεων για εξόδους του έργου, που ονομάζεται 'JobResult.txt'. Για κάθε αρχείο πολυμέσων εισόδου, το AIB που προκύπτει, ΣΆΜΙ, TTML, WebVTT και αρχεία λέξεων-κλειδιών, είναι διαδοχικά με αρίθμηση και με το όνομα χρησιμοποιώντας το "ψευδώνυμο".

Όνομα αρχείου | Περιγραφή
----------|------------
__InputFileName.aib__ | Το αρχείο ευρετηρίου blob ήχου. <br /><br /> Αρχείο ήχου δημιουργίας ευρετηρίου Blob (AIB) είναι ένα δυαδικό αρχείο που μπορεί να γίνει αναζήτηση στον Microsoft SQL server με χρήση της αναζήτησης πλήρους κειμένου.  Το αρχείο AIB είναι πιο ισχυρό από τα αρχεία απλό λεζάντα, επειδή περιέχει εναλλακτικές λύσεις για κάθε λέξη, παρέχει μια πολύ πιο πλούσια εμπειρία αναζήτησης. <br/> <br/>Απαιτεί την εγκατάσταση του πρόσθετου ευρετηρίου SQL στον υπολογιστή εκτελείται Microsoft SQL server 2008 ή νεότερη έκδοση. Αναζήτηση του AIB με χρήση του Microsoft SQL server αναζήτηση πλήρους κειμένου παρέχει πιο ακριβή αποτελέσματα αναζήτησης από την αναζήτηση των αρχείων κλειστές λεζάντες που δημιουργούνται με WAMI. Αυτό συμβαίνει επειδή το AIB περιέχει εναλλακτικές λύσεις word ποια παρόμοια ήχο ότι τα αρχεία κλειστές λεζάντες περιέχουν την υψηλότερη εμπιστοσύνης για το word για κάθε τμήμα του ήχου. Εάν η αναζήτηση για εκφωνημένες λέξεις είναι upmost σημασίας, στη συνέχεια, συνιστάται να χρησιμοποιείτε το AIB σε συνδυασμό με το Microsoft SQL Server.<br/><br/> Για να κάνετε λήψη του προσθέτου, κάντε κλικ στην επιλογή <a href="http://aka.ms/indexersql">Πρόσθετο SQL ευρετηρίου πολυμέσων Azure</a>. <br/><br/>Επίσης, είναι πιθανό να χρησιμοποιούν άλλοι μηχανισμοί αναζήτησης όπως Apache Lucene/Solr να συμπεριλάβετε στο ευρετήριο απλώς το βίντεο με βάση τις κλειστές λεζάντες και αρχεία XML λέξεων-κλειδιών, αλλά αυτό θα έχει ως αποτέλεσμα λιγότερο ακριβή αποτελέσματα αναζήτησης.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Κλειστή λεζάντα (Κοιν.) αρχεία στις μορφές ΣΆΜΙ, TTML και WebVTT.<br/><br/>Μπορεί να χρησιμοποιηθεί για να κάνετε προσβάσιμες για άτομα με ειδικές ανάγκες ακοή αρχείων ήχου και βίντεο.<br/><br/>Κλειστή λεζάντα αρχεία περιλαμβάνουν μια ετικέτα που ονομάζεται <b>Recognizability</b> είναι των βαθμών που είναι μια εργασία δημιουργίας ευρετηρίου με βάση τον τρόπο αναγνωρίσιμη της ομιλίας στην προέλευση βίντεο.  Μπορείτε να χρησιμοποιήσετε την τιμή της <b>Recognizability</b> σε αρχεία εξόδου οθόνης για χρηστικότητα. Μια χαμηλή βαθμολογία σημαίνει περιορισμένα αποτελέσματα δημιουργίας ευρετηρίου λόγω ποιότητα ήχου.
__InputFileName.kw.xml<br />InputFileName.info__ |Λέξη-κλειδί και πληροφορίες αρχείων. <br/><br/>Αρχείο λέξη-κλειδί είναι ένα αρχείο XML που περιέχει λέξεις-κλειδιά που έχουν εξαχθεί από το περιεχόμενο ομιλίας, με συχνότητα και offset πληροφορίες. <br/><br/>Αρχείο πληροφοριών είναι ένα αρχείο απλού κειμένου το οποίο περιέχει λεπτομερείς πληροφορίες σχετικά με κάθε όρο αναγνωρίζεται. Στην πρώτη γραμμή είναι ειδική και περιέχει τη βαθμολογία Recognizability. Οι επόμενες κάθε γραμμή είναι μια λίστα διαχωρισμένο με στηλοθέτες τα ακόλουθα στοιχεία: Έναρξη ώρας, ώρα λήξης, word/φράση, εμπιστοσύνης. Οι χρόνοι δίνονται σε δευτερόλεπτα και το εμπιστοσύνης δίνεται ως αριθμό από 0-1. <br/><br/>Παράδειγμα γραμμής: "word 1,20 1.45 0.67" <br/><br/>Αυτά τα αρχεία μπορεί να χρησιμοποιείται για σκοπούς, αριθμό, όπως για την εκτέλεση ανάλυσης ομιλίας, ή να εκτίθεται μηχανές αναζήτησης, όπως το Bing, Google ή το Microsoft SharePoint για να κάνετε τα αρχεία πολυμέσων πιο εύχρηστο ή ακόμα και να χρησιμοποιηθούν για την παροχή πιο συναφή διαφημίσεις.
__JobResult.txt__ |Αποτέλεσμα δηλωτικό, παρουσίαση μόνο κατά τη δημιουργία ευρετηρίου πολλών αρχείων, που περιλαμβάνει τις ακόλουθες πληροφορίες:<br/><br/><table border="1"><tr><th>Αρχείο_εισόδου</th><th>Ψευδώνυμο</th><th>MediaLength</th><th>Σφάλμα</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Εάν δεν όλα τα αρχεία πολυμέσων εισαγωγής έχουν συμπεριληφθεί με επιτυχία, θα αποτύχει η εργασία δημιουργίας ευρετηρίου με κωδικό σφάλματος 4000. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί σφάλματος](#error_codes).

## <a name="index-multiple-files"></a>Δημιουργία ευρετηρίου πολλών αρχείων

Την ακόλουθη μέθοδο κάνει αποστολή πολλών αρχείων πολυμέσων ως ενός περιουσιακού στοιχείου και δημιουργεί μια εργασία για να δημιουργήσετε ευρετήριο όλα αυτά τα αρχεία σε μια δέσμη.

Ένα αρχείο δηλώσεων με την επέκταση .lst είναι που έχουν δημιουργηθεί και αποστολής σε περιουσιακού στοιχείου. Το αρχείο δηλώσεων περιέχει τη λίστα όλων των αρχείων περιουσιακών στοιχείων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εργασία προκαθορισμένα για Azure Media ευρετηρίου](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Εργασία εν μέρει ολοκληρώθηκε με επιτυχία

Εάν δεν όλα τα αρχεία πολυμέσων εισαγωγής έχουν συμπεριληφθεί με επιτυχία, θα αποτύχει η εργασία δημιουργίας ευρετηρίου με κωδικό σφάλματος 4000. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοί σφάλματος](#error_codes).


Δημιουργούνται την ίδια εξόδους (ως ολοκληρώθηκε με επιτυχία εργασίες). Μπορείτε να ανατρέξετε στο αρχείο εξόδου δήλωσης για να μάθετε ποια αρχεία εισόδου είναι απέτυχε, σύμφωνα με τις τιμές της στήλης σφάλματος. Για αρχεία εισόδου που απέτυχε, η AIB που προκύπτει, ΣΆΜΙ, TTML, WebVTT και λέξεων-κλειδιών αρχείων δεν θα δημιουργηθεί.

### <a id="preset"></a>Υπόδειγμα εργασίας για ευρετήριο πολυμέσων Azure

Η επεξεργασία από το ευρετήριο πολυμέσων Azure μπορεί να προσαρμοστεί, παρέχοντας ένα προαιρετικό εργασίας προκαθορισμένες μαζί με την εργασία.  Το παρακάτω περιγράφει τη μορφή των αυτό xml ρύθμισης παραμέτρων.

Όνομα | Απαιτείται | Περιγραφή
----|----|---
__Εισαγωγή δεδομένων__ | FALSE | Αρχεία περιουσιακών στοιχείων που θέλετε να συμπεριλάβετε στο ευρετήριο.</p><p>Azure Media ευρετηρίου υποστηρίζει τις παρακάτω μορφές αρχείων πολυμέσων: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Μπορείτε να καθορίσετε το όνομα του αρχείου (s) στο χαρακτηριστικό **όνομα** ή τη **λίστα** από το στοιχείο **εισαγωγής** (όπως φαίνεται παρακάτω). Εάν δεν καθορίσετε ποιο αρχείο περιουσιακών στοιχείων στο ευρετήριο, επιλέξατε το αρχικό αρχείο. Εάν έχει οριστεί κανένα αρχείο πρωτεύοντος περιουσιακών στοιχείων, το πρώτο αρχείο στο εισαγωγής περιουσιακού στοιχείου είναι στο ευρετήριο.</p><p>Για να καθορίσετε το όνομα του αρχείου περιουσιακών στοιχείων, κάντε:<br />`<input name="TestFile.wmv">`<br /><br />Μπορείτε επίσης να δημιουργήσετε ευρετήριο πολλών περιουσιακών στοιχείων αρχεία ταυτόχρονα (έως και 10 αρχεία). Για να το κάνετε αυτό:<br /><br /><ol class="ordered"><li><p>Δημιουργήστε ένα αρχείο κειμένου (αρχείο δηλώσεων) και να της δώσετε την επέκταση .lst. </p></li><li><p>Προσθέστε μια λίστα με όλα τα ονόματα αρχείων περιουσιακών στοιχείων στο σας εισαγωγής περιουσιακών στοιχείων σε αυτό το αρχείο δήλωσης. </p></li><li><p>Προσθήκη αρχείου thanifest (αποστολή) στο πάγιο.  </p></li><li><p>Καθορίστε το όνομα του αρχείου δήλωσης στο χαρακτηριστικό λίστας την είσοδο.<br />`<input list="input.lst">`</li></ol><br /><br />Σημείωση: Εάν μπορείτε να προσθέσετε περισσότερους από 10 αρχεία στο αρχείο δήλωσης, της δημιουργίας ευρετηρίου εργασίας θα αποτύχει με τον κωδικό σφάλματος 2006.
__μετα-δεδομένων__ | FALSE | Μετα-δεδομένα για τα αρχεία καθορισμένο περιουσιακών στοιχείων που χρησιμοποιούνται για την προσαρμογή λεξιλόγιο.  Είναι χρήσιμο για να προετοιμάσετε ευρετηρίου την αναγνώριση λεξιλόγιο μη τυπικές λέξεις όπως κύριων ονομάτων.<br />`<metadata key="..." value="..."/>` <br /><br />Μπορείτε να δώσετε __τιμές__ για προκαθορισμένα __κλειδιά__. Προς το παρόν υποστηρίζονται τα ακόλουθα κλειδιά:<br /><br />"Τίτλος" και "Περιγραφή" - χρησιμοποιείται για την προσαρμογή λεξιλόγιο για τροποποιήστε τη γλώσσα του μοντέλου για την εργασία σας και να βελτιώσετε την ακρίβεια της αναγνώρισης ομιλίας.  Οι τιμές σπείρεται αναζητήσεις στο Internet για να βρείτε έγγραφα contextually σχετικό κείμενο, χρησιμοποιώντας το περιεχόμενο για να αυξήσετε το εσωτερικό λεξικό για τη διάρκεια της εργασίας σας ευρετηρίου.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__δυνατότητες__ <br /><br /> Προσθήκη στην έκδοση 1.2. Προς το παρόν, το μόνο υποστηριζόμενη δυνατότητα είναι αναγνώριση ομιλίας ("ASR").| FALSE | Η δυνατότητα αναγνώρισης ομιλίας περιλαμβάνει τα ακόλουθα κλειδιά ρυθμίσεις:<table><tr><th><p>Πλήκτρο</p></th>     <th><p>Περιγραφή</p></th><th><p>Παράδειγμα τιμής</p></th></tr><tr><td><p>Γλώσσα</p></td><td><p>Η φυσική γλώσσα να αναγνωρίζονται του αρχείου πολυμέσων.</p></td><td><p>Αγγλικά, Ισπανικά</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>μια λίστα διαχωρισμένο με ελληνικό ερωτηματικό με τις μορφές λεζάντα επιθυμητό αποτέλεσμα (εάν υπάρχουν)</p></td><td><p>ttml, Σάμι, webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Μια σημαία δυαδικής τιμής που καθορίζει ή όχι ενός αρχείου AIB απαιτείται (για χρήση με το SQL Server και τον πελάτη IFilter ευρετήριο).  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Χρήση AIB αρχείων με ευρετήριο πολυμέσων Azure και SQL Server</a>.</p></td><td><p>Τιμή TRUE. FALSE</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Μια σημαία δυαδικής τιμής που καθορίζει αν απαιτείται ένα αρχείο XML λέξεων-κλειδιών.</p></td><td><p>Τιμή TRUE. FALSE. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Μια σημαία δυαδικής τιμής που καθορίζει αν θα επιβάλετε πλήρους λεζάντες (ανεξάρτητα από το επίπεδο εμπιστοσύνης) ή όχι.  </p><p>Προεπιλογή είναι false, σε αυτήν την περίπτωση λέξεις και φράσεις που έχουν ένα μικρότερο από 50% εμπιστοσύνης επίπεδο έχουν παραλειφθεί από το τελικό λεζάντα εξόδους και να αντικατασταθούν από αποσιωπητικά ("...").  Τα αποσιωπητικά είναι χρήσιμες για τον ποιοτικό έλεγχο λεζάντα και έλεγχος.</p></td><td><p>Τιμή TRUE. FALSE. </p></td></tr></table>

### <a id="error_codes"></a>Κωδικοί σφάλματος

Στην περίπτωση σφάλματος, πρέπει να αναφέρετε ευρετηρίου πολυμέσων Azure πίσω έναν από τους ακόλουθους κωδικούς σφάλματος:

Κωδικός | Όνομα | Πιθανές αιτίες
-----|------|------------------
2000 | Μη έγκυρη ρύθμιση παραμέτρων | Μη έγκυρη ρύθμιση παραμέτρων
2001 | Μη έγκυροι εισαγωγής περιουσιακών στοιχείων | Λείπουν εισαγωγής περιουσιακών στοιχείων ή κενό περιουσιακών στοιχείων.
2002 | Μη έγκυροι δηλωτικό | Δήλωση είναι κενό ή δηλωτικό περιέχει μη έγκυρους στοιχεία.
2003 | Απέτυχε η λήψη του αρχείου πολυμέσων | Μη έγκυρη διεύθυνση URL στο αρχείο δήλωσης.
2004 | Πρωτόκολλο που δεν υποστηρίζεται | Πρωτόκολλο της διεύθυνσης URL πολυμέσων δεν υποστηρίζεται.
2005 | Μη υποστηριζόμενο τύπο αρχείου | Τύπος αρχείου εισαγωγής πολυμέσων δεν υποστηρίζεται.
2006 | Πάρα πολλά αρχεία εισόδου | Υπάρχουν περισσότερα από 10 αρχεία στο δηλωτικό εισαγωγής.
3000 | Απέτυχε η αποκωδικοποιείτε του αρχείου πολυμέσων | Κωδικοποιητή πολυμέσων που δεν υποστηρίζεται <br/>ή<br/> Αρχείο κατεστραμμένο πολυμέσων <br/>ή<br/> Καμία ροή ήχου στο εισαγωγής πολυμέσων.
4000 | Μερικώς μαζική δημιουργία ευρετηρίου ολοκληρώθηκε με επιτυχία | Ορισμένα από τα αρχεία πολυμέσων εισόδου είναι απέτυχε να τοποθετηθούν στο ευρετήριο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα <a href="#output_files">αρχεία εξόδου</a>.
άλλες | Εσωτερικά σφάλματα | Επικοινωνήστε με την ομάδα υποστήριξης. indexer@microsoft.com


## <a id="supported_languages"></a>Υποστηριζόμενες γλώσσες

Υποστηρίζονται προς το παρόν, οι γλώσσες στα Αγγλικά και τα Ισπανικά. Για περισσότερες πληροφορίες, ανατρέξτε [στην καταχώρηση ιστολογίου v1.2 κυκλοφορίας](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Χρήση των αρχείων AIB με ευρετήριο πολυμέσων Azure και του SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Δημιουργία ευρετηρίου αρχεία πολυμέσων με την προεπισκόπηση ευρετηρίου 2 πολυμέσων Azure](media-services-process-content-with-indexer2.md)

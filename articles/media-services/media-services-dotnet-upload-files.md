<properties 
    pageTitle="Αποστολή αρχείων σε λογαριασμό Media Services χρησιμοποιώντας .NET | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να κατανοήσετε περιεχομένου πολυμέσων Media Services με τη δημιουργία και αποστολή περιουσιακών στοιχείων." 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>



# <a name="upload-files-into-a-media-services-account-using-net"></a>Αποστολή αρχείων σε λογαριασμό Media Services χρησιμοποιώντας .NET

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [ΥΠΌΛΟΙΠΟ](media-services-rest-upload-files.md)
 - [Πύλη](media-services-portal-upload-files.md)

Υπηρεσίες πολυμέσων που Αποστολή (ή ingest) ψηφιακή τα αρχεία σας στην ενός περιουσιακού στοιχείου. Η οντότητα **περιουσιακών στοιχείων** μπορεί να περιέχει βίντεο, ήχο, εικόνες, μικρογραφιών συλλογές, κείμενο κομμάτια και κλειστές λεζάντες αρχείων (και τα μετα-δεδομένα σχετικά με αυτά τα αρχεία.)  Όταν τα αρχεία έχουν αποσταλεί, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής.

Τα αρχεία του παγίου ονομάζονται **Αρχεία περιουσιακών στοιχείων**. Η παρουσία **AssetFile** και το αρχείο πραγματική πολυμέσων είναι δύο ξεχωριστά αντικείμενα. Η παρουσία AssetFile περιέχει μετα-δεδομένα σχετικά με το αρχείο πολυμέσων, ενώ το αρχείο περιέχει το περιεχόμενο πραγματική πολυμέσων.

>[AZURE.NOTE]Τα παρακάτω ζητήματα εφαρμόζονται κατά την επιλογή ονόματος ενός αρχείου περιουσιακών στοιχείων:
>
>- Υπηρεσίες πολυμέσων χρησιμοποιεί την τιμή της ιδιότητας IAssetFile.Name κατά τη δημιουργία διευθύνσεις URL για τη ροή περιεχομένου (για παράδειγμα, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Για αυτόν το λόγο, δεν επιτρέπεται η κωδικοποίηση. Η τιμή της ιδιότητας **όνομα** δεν μπορεί να έχει έναν από τους παρακάτω [χαρακτήρες τοις εκατό κωδικοποίηση-αποκλειστικές](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Επίσης, μπορεί να υπάρξει μόνο μία '.' για την επέκταση ονόματος αρχείου.
>
>- Το μήκος του ονόματος δεν πρέπει να είναι μεγαλύτερο από 260 χαρακτήρες.

Όταν δημιουργείτε περιουσιακών στοιχείων, μπορείτε να καθορίσετε τις ακόλουθες επιλογές κρυπτογράφησης. 

- **Κανένα** - χωρίς κρυπτογράφηση χρησιμοποιείται. Αυτή είναι η προεπιλεγμένη τιμή. Σημειώστε ότι όταν χρησιμοποιείτε αυτήν την επιλογή το περιεχόμενό σας δεν είναι προστατευμένο κατά τη μεταφορά ή στο υπόλοιπο στο χώρο αποθήκευσης.
Εάν σκοπεύετε να κάνετε μια MP4 χρησιμοποιώντας προοδευτική λήψη, χρησιμοποιήστε αυτήν την επιλογή. 
- **CommonEncryption** - Χρησιμοποιήστε αυτήν την επιλογή εάν στέλνετε περιεχομένου που έχει ήδη κρυπτογραφημένα και προστατεύεται με κοινές κρυπτογράφηση ή PlayReady DRM (για παράδειγμα, ομαλή ροή προστατεύεται με PlayReady DRM).
- **EnvelopeEncrypted** -Χρησιμοποιήστε αυτήν την επιλογή εάν στέλνετε HLS κρυπτογραφημένα με AES. Σημειώστε ότι τα αρχεία πρέπει να έχουν κωδικοποιημένη και κρυπτογραφημένα με μετασχηματισμός Manager.
- **StorageEncrypted** - κρυπτογραφεί Απαλοιφή περιεχομένου σας τοπικά χρησιμοποιώντας κρυπτογράφηση AES-256 bit και, στη συνέχεια, να αποθήκευσης Azure όπου είναι αποθηκευμένο κρυπτογραφημένα αποστολές στο υπόλοιπο. Στοιχεία που προστατεύονται με κρυπτογράφηση χώρου αποθήκευσης είναι αυτόματα χωρίς κρυπτογράφηση τοποθετείται σε ένα σύστημα κρυπτογραφημένο αρχείο πριν από την κωδικοποίηση και προαιρετικά κρυπτογραφημένα εκ νέου πριν από την αποστολή ξανά ως νέο πάγιο εξόδου. Βασική χρήση συμβαίνει κρυπτογράφησης χώρου αποθήκευσης είναι όταν θέλετε να προστατεύσετε τα αρχεία εισαγωγής πολυμέσων υψηλής ποιότητας με ισχυρή κρυπτογράφηση στο υπόλοιπο στο δίσκο.

    Υπηρεσίες πολυμέσων παρέχει χώρο αποθήκευσης στο δίσκο κρυπτογράφηση για τους πόρους σας, δεν επάνω-το-σύρματος όπως διαχείριση ψηφιακών δικαιωμάτων (DRM).

    Εάν σας περιουσιακού στοιχείου είναι κρυπτογραφημένα χώρου αποθήκευσης, πρέπει να ρυθμίσετε περιουσιακών στοιχείων παράδοσης πολιτικής. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων περιουσιακών στοιχείων παράδοσης πολιτικής](media-services-dotnet-configure-asset-delivery-policy.md).

Εάν καθορίσετε για σας περιουσιακού στοιχείου για να είναι κρυπτογραφημένα με μια επιλογή **CommonEncrypted** ή μια επιλογή **EnvelopeEncypted** , θα πρέπει να συσχετίσετε το περιουσιακών στοιχείων με μια **ContentKey**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε ένα ContentKey](media-services-dotnet-create-contentkey.md). 

Εάν καθορίσετε για σας περιουσιακού στοιχείου για να είναι κρυπτογραφημένα με μια επιλογή **StorageEncrypted** , το SDK υπηρεσίες πολυμέσων για .NET θα δημιουργήσει μια **StorateEncrypted** **ContentKey** για σας περιουσιακών στοιχείων.


Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Media Services .NET SDK, καθώς και επεκτάσεις Media Services .NET SDK για την αποστολή αρχείων σε μια περιουσιακών στοιχείων Media Services.

 
## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Στείλτε ένα αρχείο με Media Services .NET SDK 

Το παρακάτω δείγμα κώδικα χρησιμοποιεί .NET SDK για να εκτελέσετε τις ακόλουθες εργασίες: 

- Δημιουργεί μια κενή περιουσιακών στοιχείων.
- Δημιουργεί μια παρουσία AssetFile που θέλετε να συσχετίσετε με την περιουσιακών στοιχείων.
- Δημιουργεί μια παρουσία AccessPolicy που ορίζει τα δικαιώματα και τη διάρκεια της access στο πάγιο.
- Δημιουργεί μια παρουσία προσδιοριστικό που παρέχει πρόσβαση σε περιουσιακού στοιχείου.
- Αποστέλλει ένα αρχείο μία πολυμέσων σε υπηρεσίες πολυμέσων. 

        
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions); 

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Created assetFile {0}", assetFile.Name);

            var policy = _context.AccessPolicies.Create(
                                    assetName,
                                    TimeSpan.FromDays(30),
                                    AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            locator.Delete();
            policy.Delete();

            return inputAsset;
        }

##<a name="upload-multiple-files-with-media-services-net-sdk"></a>Αποστολή πολλών αρχείων με Media Services .NET SDK 

Ο ακόλουθος κώδικας εμφανίζει τον τρόπο δημιουργίας ενός περιουσιακού στοιχείου και αποστολή πολλών αρχείων.

Ο κώδικας κάνει τα εξής:
    
-   Δημιουργεί μια κενή περιουσιακών στοιχείων με τη μέθοδο CreateEmptyAsset ορίζεται στο προηγούμενο βήμα.
    
-   Δημιουργεί μια παρουσία **AccessPolicy** που ορίζει τα δικαιώματα και τη διάρκεια της access στο πάγιο.
    
-   Δημιουργεί μια παρουσία **προσδιοριστικό** που παρέχει πρόσβαση σε περιουσιακού στοιχείου.
    
-   Δημιουργεί μια παρουσία **BlobTransferClient** . Αυτός ο τύπος αντιπροσωπεύει ένα πρόγραμμα-πελάτη που λειτουργεί σε το Azure αντικείμενα blob. Σε αυτό το παράδειγμα, μπορούμε να χρησιμοποιήσουμε το πρόγραμμα-πελάτη για την παρακολούθηση της προόδου αποστολής. 
    
-   Απαριθμεί στα αρχεία στον καθορισμένο κατάλογο και δημιουργεί μια παρουσία **AssetFile** για κάθε αρχείο.
    
-   Κάνει αποστολή των αρχείων σε Media Services χρησιμοποιώντας τη μέθοδο **UploadAsync** . 
    
>[AZURE.NOTE] Χρησιμοποιήστε τη μέθοδο UploadAsync για να βεβαιωθείτε ότι οι δεν αποκλείουν τις κλήσεις και τα αρχεία έχουν αποσταλεί παράλληλα.
    
    
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AccestFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }
    
    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }



Κατά την αποστολή ενός μεγάλου αριθμού περιουσιακών στοιχείων, λάβετε υπόψη τα εξής.

- Δημιουργία ενός νέου αντικειμένου **CloudMediaContext** ανά νήματος. Η κλάση **CloudMediaContext** δεν διαθέτει ασφάλεια νήματος.
 
- Αύξηση NumberOfConcurrentTransfers από την προεπιλεγμένη τιμή του 2 σε υψηλότερη τιμή, όπως το 5. Ορισμός αυτής της ιδιότητας επηρεάζει όλες τις εμφανίσεις της **CloudMediaContext**. 
 
- Διατήρηση ParallelTransferThreadCount στην προεπιλεγμένη τιμή 10.
 
##<a id="ingest_in_bulk"></a>Ingesting παγίων μαζικά, χρησιμοποιώντας Media Services .NET SDK 

Αποστολή αρχείων μεγάλο περιουσιακών στοιχείων μπορεί να είναι συμφόρηση κατά τη δημιουργία περιουσιακού στοιχείου. Ingesting παγίων μαζικές ή "Μαζική Ingesting", περιλαμβάνει την αποσύνδεση δημιουργία περιουσιακού στοιχείου από τη διαδικασία αποστολής. Για να χρησιμοποιήσετε μια μαζική ingesting προσέγγιση, δημιουργήστε μια δήλωση (IngestManifest) που περιγράφει την περιουσιακών στοιχείων και τα συσχετισμένα αρχεία. Στη συνέχεια, χρησιμοποιήστε τη μέθοδο αποστολής της επιλογής σας για να αποστείλετε τα συσχετισμένα αρχεία το δηλωτικό blob κοντέινερ. Microsoft Azure Media Services παρακολουθεί το κοντέινερ αντικειμένων blob που σχετίζεται με το δηλωτικό. Μετά την αποστολή ενός αρχείου στο κοντέινερ αντικειμένων blob, Microsoft Azure Media Services ολοκληρώνεται η δημιουργία περιουσιακού στοιχείου με βάση τη ρύθμιση παραμέτρων του περιουσιακού στοιχείου στο δηλωτικό (IngestManifestAsset).


Για να δημιουργήσετε μια νέα κλήση IngestManifest τη μέθοδο Create που εκτίθεται από τη συλλογή IngestManifests στο το CloudMediaContext. Αυτή η μέθοδος θα δημιουργήσει μια νέα IngestManifest με το όνομα δήλωσης που παρέχετε.

    IIngestManifest manifest = context.IngestManifests.Create(name);

Δημιουργία των περιουσιακών στοιχείων που θα συσχετιστεί με τη μαζική IngestManifest. Ρύθμιση παραμέτρων των επιλογών επιθυμητή κρυπτογράφησης σε περιουσιακού στοιχείου για μαζική ingesting.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Μια IngestManifestAsset συσχετίζει ενός περιουσιακού στοιχείου με μια μαζική IngestManifest για μαζική ingesting. Επίσης, συσχετίζει την AssetFiles που θα κάνει κάθε περιουσιακού στοιχείου προς τα επάνω. Για να δημιουργήσετε ένα IngestManifestAsset, χρησιμοποιήστε τη μέθοδο Create στο περιβάλλον του διακομιστή.

Το παρακάτω παράδειγμα παρουσιάζει προσθέτοντας δύο νέα IngestManifestAssets που συσχετίσετε δύο περιουσιακών στοιχείων προηγουμένως δημιουργήσει για να μαζικής ingest δηλωτικό. Επίσης, κάθε IngestManifestAsset συσχετίζει ένα σύνολο αρχείων που θα αποσταλεί για κάθε πάγιο κατά τη διάρκεια της μαζικής ingesting.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;
    
    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
    
Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εφαρμογή του προγράμματος-πελάτη υψηλής ταχύτητας δυνατότητα αποστολή των αρχείων περιουσιακών στοιχείων στο κοντέινερ χώρου αποθήκευσης αντικειμένων blob URI που παρέχεται από την ιδιότητα **IIngestManifest.BlobStorageUriForUpload** το IngestManifest. Μία υπηρεσία αποστολής αξιοσημείωτες υψηλής ταχύτητας είναι [Aspera On Demand για την εφαρμογή Azure](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Μπορείτε επίσης να συντάξετε κώδικα για να αποστείλετε τα αρχεία περιουσιακών στοιχείων, όπως φαίνεται στο παρακάτω παράδειγμα κώδικα.
    
    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);
    
            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);
    
            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);
    
            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });
    
        copytask.Start();
    }

Τον κωδικό για αποστολή των αρχείων περιουσιακού στοιχείου για το δείγμα που χρησιμοποιούνται σε αυτό το θέμα εμφανίζεται στο παρακάτω παράδειγμα κώδικα.
    
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
    

Μπορείτε να προσδιορίσετε την πρόοδο της τη μαζική ingesting για όλα τα στοιχεία που σχετίζονται με ένα **IngestManifest** , η ιδιότητα στατιστικά στοιχεία του **IngestManifest**σταθμοσκόπησης. Για να ενημερώσετε τις πληροφορίες προόδου, πρέπει να χρησιμοποιήσετε μια νέα **CloudMediaContext** κάθε φορά που ψηφοφορία με την ιδιότητα στατιστικά στοιχεία.

Το παρακάτω παράδειγμα παρουσιάζει σταθμοσκόπησης μια IngestManifest κατά το **αναγνωριστικό**.
    
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();
    
          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);
    
                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }
    
             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
    


##<a name="upload-files-using-net-sdk-extensions"></a>Αποστολή αρχείων με χρήση επεκτάσεις SDK .NET 

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αποστείλετε ένα αρχείο χρησιμοποιώντας .NET SDK επεκτάσεις. Σε αυτήν την περίπτωση χρησιμοποιείται η μέθοδος **CreateFromFile** , αλλά η ασύγχρονης έκδοση είναι επίσης διαθέσιμη (**CreateFromFileAsync**). Η μέθοδος **CreateFromFile** σάς επιτρέπει να καθορίσετε το όνομα του αρχείου, επιλογή κρυπτογράφηση και επιστροφή κλήσης για να αναφέρετε την πρόοδο αποστολή του αρχείου.


    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });
    
        Console.WriteLine("Asset {0} created.", inputAsset.Id);
    
        return inputAsset;
    }

Το παρακάτω παράδειγμα καλεί συνάρτηση UploadFile θα και καθορίζει κρυπτογράφησης χώρου αποθήκευσης με την επιλογή Δημιουργία περιουσιακού στοιχείου.  


    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-step"></a>Επόμενο βήμα

Τώρα που έχετε αποστείλει ενός περιουσιακού στοιχείου σε Media Services, μεταβείτε στο θέμα [Πώς μπορώ να αποκτήσω ένα πρόγραμμα επεξεργασίας πολυμέσων][] .

[Πώς μπορώ να αποκτήσω ένα πρόγραμμα επεξεργασίας πολυμέσων]: media-services-get-media-processor.md
 

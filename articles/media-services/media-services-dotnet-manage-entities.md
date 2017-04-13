
<properties 
    pageTitle="Διαχείριση πόρων και σχετικών οντοτήτων με τις υπηρεσίες πολυμέσων .NET SDK" 
    description="Μάθετε πώς μπορείτε να διαχειρίζεστε περιουσιακών στοιχείων και των σχετικών οντοτήτων με το SDK υπηρεσίες πολυμέσων για .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Διαχείριση πόρων και σχετικών οντοτήτων με τις υπηρεσίες πολυμέσων .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-manage-entities.md)


Αυτό το θέμα δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες διαχείρισης Media Services:

- Λάβετε μια αναφορά περιουσιακών στοιχείων 
- Λάβετε μια αναφορά εργασίας 
- Λίστα με όλα τα περιουσιακά στοιχεία 
- Λίστα εργασιών και περιουσιακών στοιχείων 
- Λίστα με όλες τις πολιτικές πρόσβασης 
- Λίστα Locators όλων
- Απαρίθμησης μεγάλες συλλογές οντοτήτων
- Διαγραφή ενός περιουσιακού στοιχείου 
- Διαγραφή μιας εργασίας 
- Διαγραφή μιας πολιτικής πρόσβασης 

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Ανατρέξτε στο θέμα [Ρύθμιση του περιβάλλοντος του](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Λάβετε μια αναφορά περιουσιακών στοιχείων

Είναι μια συνηθισμένη εργασία για να λάβετε μια αναφορά σε μια υπάρχουσα περιουσιακών στοιχείων στις υπηρεσίες πολυμέσων. Το ακόλουθο παράδειγμα κώδικα παρουσιάζει πώς μπορείτε να κάνετε μια αναφορά περιουσιακών στοιχείων από τη συλλογή περιουσιακών στοιχείων στο διακομιστή αντικείμενο περιβάλλοντος, βάσει ενός περιουσιακού στοιχείου αναγνωριστικό.
Το ακόλουθο παράδειγμα κώδικα χρησιμοποιεί Linq ερωτήματος για να λάβετε μια αναφορά σε ένα υπάρχον αντικείμενο IAsset.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Λάβετε μια αναφορά εργασίας

Όταν εργάζεστε με επεξεργασία εργασιών σε κώδικα Media Services, συχνά πρέπει να λάβετε μια αναφορά σε μια υπάρχουσα εργασία που βασίζονται σε αναγνωριστικό. Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να λάβετε μια αναφορά σε ένα αντικείμενο IJob από τη συλλογή εργασίες.
WarningWarning ενδέχεται να πρέπει να λάβετε μια αναφορά εργασίας κατά την εκκίνηση ενός έργου κωδικοποίησης μεγάλη διάρκεια εκτέλεσης και πρέπει να ελέγξετε την κατάσταση της εργασίας σε ένα νήμα. Στις περιπτώσεις ως εξής, όταν η μέθοδος επιστρέφει από ένα νήμα, πρέπει να ανακτήσετε μια ανανεωμένα αναφορά σε μια εργασία.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Λίστα με όλα τα περιουσιακά στοιχεία

Καθώς εξελίσσεται τον αριθμό των περιουσιακών στοιχείων που έχετε στο χώρο αποθήκευσης, είναι χρήσιμο να παραθέσετε τους πόρους σας. Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να επαναλάβετε μέσα στη συλλογή στοιχεία στο αντικείμενο του περιβάλλοντος διακομιστή. Με κάθε πάγιο, το παράδειγμα κώδικα εγγράφει επίσης ορισμένες από τις τιμές της ιδιότητας στην κονσόλα. Για παράδειγμα, κάθε πάγιο μπορεί να περιέχει πολλά αρχεία πολυμέσων. Το παράδειγμα κώδικα εγγράφει ανάληψη όλα τα αρχεία που σχετίζονται με κάθε περιουσιακών στοιχείων.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Λίστα εργασιών και περιουσιακών στοιχείων

Ένα σημαντικό σχετική εργασία είναι να στοιχείων λίστας με τις σχετικές εργασίας στις υπηρεσίες πολυμέσων. Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να παραθέσετε κάθε αντικείμενο IJob, και, στη συνέχεια, για κάθε εργασία, εμφανίζει ιδιότητες σχετικά με την εργασία, όλα τα σχετικά εργασίες, όλα εισαγωγής περιουσιακών στοιχείων, καθώς και όλα τα στοιχεία εξόδου. Ο κώδικας σε αυτό το παράδειγμα μπορεί να είναι χρήσιμη για πολλές άλλες εργασίες. Για παράδειγμα, εάν θέλετε να παραθέσετε τα στοιχεία εξόδου από μία ή περισσότερες εργασίες κωδικοποίησης που εκτελέσατε προηγουμένως, αυτός ο κωδικός δείχνει πώς μπορείτε να αποκτήσετε πρόσβαση των περιουσιακών στοιχείων εξόδου. Όταν έχετε μια αναφορά σε ενός περιουσιακού στοιχείου εξόδου, μπορείτε, στη συνέχεια, να παραδώσετε το περιεχόμενο σε άλλους χρήστες ή εφαρμογές κατά τη λήψη της ή παρέχοντας διευθύνσεις URL. 

Για περισσότερες πληροφορίες σχετικά με τις επιλογές για την παροχή περιουσιακών στοιχείων, ανατρέξτε στο θέμα [Προβολή στοιχείων με το SDK υπηρεσίες πολυμέσων για .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Λίστα με όλες τις πολιτικές πρόσβασης

Στις υπηρεσίες πολυμέσων, μπορείτε να ορίσετε μια πολιτική πρόσβασης σε ενός περιουσιακού στοιχείου ή των αρχείων. Μια πολιτική πρόσβασης που ορίζει τα δικαιώματα για ένα αρχείο ή ενός περιουσιακού στοιχείου (Τι τύπο πρόσβασης και τη διάρκεια). Στον κώδικα Media Services, μπορείτε να καθορίσετε μια πολιτική πρόσβασης με τη δημιουργία ενός αντικειμένου IAccessPolicy και, στη συνέχεια, συσχετίζοντας την με μια υπάρχουσα περιουσιακών στοιχείων. Στη συνέχεια, μπορείτε να δημιουργήσετε ένα αντικείμενο ILocator, το οποίο σας επιτρέπει να παράσχετε άμεση πρόσβαση στους πόρους στις υπηρεσίες πολυμέσων. Το έργο Visual Studio που συνοδεύει αυτήν τη σειρά τεκμηρίωση περιέχει διάφορα παραδείγματα κώδικα που δείχνουν πώς μπορείτε να δημιουργήσετε και να αντιστοιχίσετε πολιτικές πρόσβασης και locators σε πόρους.

Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να αναφέρετε όλες τις πολιτικές πρόσβασης στο διακομιστή και εμφανίζει τον τύπο των δικαιωμάτων που σχετίζονται με κάθε μία. Ένας άλλος τρόπος χρήσιμη για να προβάλετε πολιτικές πρόσβασης είναι στη λίστα όλα τα αντικείμενα ILocator στο διακομιστή και, στη συνέχεια, για κάθε locator, μπορείτε να εμφανίσετε την πολιτική σχετίζονται πρόσβασης, χρησιμοποιώντας την ιδιότητα AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Λίστα Locators όλων

Ένα προσδιοριστικό είναι μια διεύθυνση URL που παρέχει μια απευθείας διαδρομή για να αποκτήσετε πρόσβαση ενός περιουσιακού στοιχείου, μαζί με τα δικαιώματα για το περιουσιακών στοιχείων, όπως ορίζεται από την πολιτική πρόσβασης σχετίζεται το προσδιοριστικό. Κάθε πάγιο μπορεί να έχει μια συλλογή ILocator αντικειμένων που σχετίζονται με αυτήν την ιδιότητα Locators. Το περιβάλλον του διακομιστή έχει επίσης μια συλλογή Locators που περιέχει όλα locators.

Το ακόλουθο παράδειγμα κώδικα παραθέτει όλες locators στο διακομιστή. Για κάθε προσδιοριστικό, που εμφανίζει το αναγνωριστικό για το σχετικό πολιτική περιουσιακών στοιχείων και την πρόσβαση. Εμφανίζει επίσης τον τύπο των δικαιωμάτων, την ημερομηνία λήξης και την πλήρη διαδρομή του περιουσιακού στοιχείου.

Σημειώστε ότι μια διαδρομή προσδιοριστικό ενός περιουσιακού στοιχείου είναι μόνο μια βασική διεύθυνση URL για τον πόρο. Για να δημιουργήσετε μια απευθείας διαδρομή για μεμονωμένα αρχεία που θα μπορούσε να μεταβείτε ένα χρήστη ή μια εφαρμογή, τον κωδικό πρέπει να προσθέσετε τη διαδρομή του αρχείου συγκεκριμένα τη διαδρομή προσδιοριστικό. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να το κάνετε αυτό, ανατρέξτε στο θέμα [Προβολή στοιχείων με το SDK υπηρεσίες πολυμέσων για .NET](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Απαρίθμησης μεγάλες συλλογές οντοτήτων

Κατά την υποβολή ερωτήματος οντοτήτων, υπάρχει όριο 1000 οντοτήτων επιστρέφονται ταυτόχρονα επειδή δημόσια v2 ΥΠΌΛΟΙΠΑ περιορίζει τα αποτελέσματα ερωτήματος 1000 αποτελέσματα. Πρέπει να χρησιμοποιήσετε παράλειψη και λήψη όταν απαρίθμησης μεγάλες συλλογές οντοτήτων. 
    
Η ακόλουθη συνάρτηση διέρχεται από όλες τις εργασίες στο παρεχόμενο λογαριασμό υπηρεσίες πολυμέσων. Υπηρεσίες πολυμέσων επιστρέφει 1000 εργασίες εργασίες συλλογής. Η συνάρτηση χρησιμοποιεί παράλειψη και λήψη για να βεβαιωθείτε ότι όλα τα έργα θα γίνεται απαρίθμηση (σε περίπτωση που έχετε περισσότερους από 1000 εργασίες στο λογαριασμό σας).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Διαγραφή ενός περιουσιακού στοιχείου

Το παρακάτω παράδειγμα διαγράφει ενός περιουσιακού στοιχείου.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Διαγραφή μιας εργασίας

Για να διαγράψετε μια εργασία, πρέπει να μπορείτε να ελέγξετε την κατάσταση της εργασίας που αναφέρεται στην ιδιότητα μέλους. Εργασίες που έχουν ολοκληρωθεί ή ακυρωθεί μπορούν να διαγραφούν, ενώ οι εργασίες που βρίσκονται σε ορισμένες άλλες καταστάσεις, όπως σε ουρά, προγραμματισμένη ή επεξεργασία, πρέπει να ακυρωθεί πρώτα και, στη συνέχεια, να διαγραφούν.
Το παρακάτω παράδειγμα κώδικα εμφανίζει μια μέθοδο για τη διαγραφή μιας εργασίας, ο έλεγχος καταστάσεις εργασίας και, στη συνέχεια, διαγράφοντας όταν ολοκληρωθεί ή να ακυρωθεί η κατάσταση. Αυτός ο κωδικός εξαρτάται από την προηγούμενη ενότητα σε αυτό το θέμα για γρήγορα μια αναφορά σε μια εργασία: λήψη μιας αναφοράς έργου.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Διαγραφή μιας πολιτικής πρόσβασης

Το ακόλουθο παράδειγμα κώδικα παρουσιάζει πώς μπορείτε να λάβετε μια αναφορά σε μια πολιτική πρόσβασης που βασίζεται σε μια πολιτική Id σας, και στη συνέχεια, να διαγράψετε την πολιτική.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

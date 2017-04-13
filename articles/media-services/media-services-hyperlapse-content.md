<properties
    pageTitle="Αρχεία πολυμέσων Hyperlapse με Hyperlapse πολυμέσων Azure | Microsoft Azure"
    description="Azure Media Hyperlapse δημιουργεί ομαλές παραγραφεί ώρα βίντεο από το πρώτο άτομο ή ενέργεια κάμερας περιεχόμενο. Αυτό το θέμα εξηγεί τον τρόπο χρήσης του ευρετηρίου πολυμέσων."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Αρχεία πολυμέσων Hyperlapse με Hyperlapse πολυμέσων Azure

Azure Media Hyperlapse είναι μια πολυμέσων Επεξεργαστής (πολλών Επεξεργαστών) που δημιουργεί ομαλές παραγραφεί ώρα βίντεο από το πρώτο άτομο ή ενέργεια κάμερας περιεχόμενο.  Ομοειδή βασίζεται στο cloud για [Microsoft Research υπολογιστή Hyperlapse Pro και Hyperlapse Mobile μέσω τηλεφώνου](http://aka.ms/hyperlapse), Microsoft Hyperlapse για υπηρεσιών Azure Media Services χρησιμοποιεί την κλίμακα μαζικών Azure Media Services πολυμέσων επεξεργασίας της πλατφόρμας για να περιορίσετε το μέγεθος και να parallelize οριζόντια μαζική Hyperlapse επεξεργασίας.

>[AZURE.IMPORTANT]Microsoft Hyperlapse έχει σχεδιαστεί για να λειτουργούν καλύτερα πρώτου προσώπου περιεχομένου με κυλιόμενο κάμερα.  Παρόλο που μπορεί να εξακολουθούν να λειτουργούν οι αποσπάσματος εξακολουθούν να είναι κάμερας, τις επιδόσεις και την ποιότητα του επεξεργαστή πολυμέσων Azure Media Hyperlapse δεν μπορεί να εξασφαλιστεί για τους άλλους τύπους περιεχομένου.  Για να μάθετε περισσότερα σχετικά με το Microsoft Hyperlapse για υπηρεσιών Azure Media Services και δείτε ορισμένα βίντεο παράδειγμα, δείτε το [εισαγωγικό ιστολογίου](http://aka.ms/azurehyperlapseblog) από την προεπισκόπηση δημόσια.

Μια Hyperlapse πολυμέσων Azure εργασίας λαμβάνει ως ένα αρχείο MP4, MOV ή WMV περιουσιακών στοιχείων μαζί με ένα αρχείο ρύθμισης παραμέτρων που καθορίζει ποιες καρέ του βίντεο θα πρέπει να είναι παραγραφεί ώρα και τι ταχύτητα εισόδου (π.χ. πρώτη 10.000 πλαισίων στο 2 x).  Το αποτέλεσμα είναι μια σταθεροποιημένη και παραγραφεί χρόνου εκτέλεσης της οθόνης εισόδου.

Για τις πιο πρόσφατες ενημερώσεις Hyperlapse πολυμέσων Azure, ανατρέξτε στο θέμα [ιστολόγια Media Services](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse ενός περιουσιακού στοιχείου

Πρώτα θα πρέπει να αποστείλετε το επιθυμητό αρχείο εισαγωγής των υπηρεσιών Azure Media Services.  Για να μάθετε περισσότερα σχετικά με τις έννοιες που σχετίζονται με την αποστολή και τη Διαχείριση περιεχομένου, ανατρέξτε στο [άρθρο Διαχείριση περιεχομένου](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Ρύθμιση παραμέτρων προκαθορισμένα για Hyperlapse

Μόλις το περιεχόμενό σας στο λογαριασμό σας στο Media Services, θα πρέπει να δημιουργήσετε προκαθορισμένη ρύθμιση παραμέτρων.  Ο παρακάτω πίνακας περιγράφει τα πεδία που καθορίζονται από το χρήστη:

 Πεδίο | Περιγραφή
-------|-------------
StartFrame|Το πλαίσιο κατά την οποία θα πρέπει να ξεκινήσετε την επεξεργασία Hyperlapse της Microsoft.
NumFrames|Ο αριθμός των πλαισίων για την επεξεργασία
Ταχύτητα|Ο συντελεστής με το οποίο θέλετε να επιταχύνετε το βίντεο εισαγωγής.

Ακολουθεί ένα παράδειγμα ενός αρχείου ρύθμισης παραμέτρων είναι συμβατή σε XML και JSON:

**Προκαθορισμένα XML:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**Προκαθορισμένα JSON:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse με το .NET αθροιστικής μέτρησης ενισχύσεων SDK

Την ακόλουθη μέθοδο αποστέλλει ένα αρχείο πολυμέσων ως ενός περιουσιακού στοιχείου και δημιουργεί μια εργασία με τον επεξεργαστή πολυμέσων Azure Media Hyperlapse.

> [AZURE.NOTE] Θα πρέπει να έχετε ήδη ένα CloudMediaContext στο πεδίο με το όνομα "περιβάλλον" για αυτόν τον κωδικό για να εργαστείτε.  Για να μάθετε περισσότερα σχετικά με αυτό, ανατρέξτε στο [άρθρο Διαχείριση περιεχομένου](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Το όρισμα συμβολοσειράς "hyperConfig" αναμένεται να είναι μια ρύθμιση παραμέτρων είναι συμβατή προκαθορισμένες JSON είτε XML, όπως περιγράφεται παραπάνω.

στατική bool RunHyperlapseJob (συμβολοσειρά εισόδου, εξόδου συμβολοσειρά, συμβολοσειρά hyperConfig) {/ / Δημιουργία περιουσιακού στοιχείου με περιουσιακών στοιχείων IAsset αρχείο εισόδου = περιβάλλοντος. Πόρους. CreateAssetAndUploadSingleFile (είσοδος, "Μου Hyperlapse εισόδου δεδομένων", AssetCreationOptions.None);

Πάρτε παρουσίες του Azure Media Hyperlapse πολλών Επεξεργαστών IMediaProcessor πολλών επεξεργαστών = περιβάλλοντος. MediaProcessors. GetLatestMediaProcessorByName ("πολυμέσων Azure Hyperlapse");

Δημιουργία εργασίας με Hyperlapse εργασίας IJob εργασία = περιβάλλοντος. Εργασίες. Δημιουργία (String.Format ("Hyperlapse {0}", είσοδος));

Εάν (String.IsNullOrEmpty(hyperConfig)) {/ / config δεν μπορεί να είναι κενή τιμή επιστροφής false.}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = εργασία. Tasks.AddNew ("Hyperlapse εργασίας", πολλών επεξεργαστών, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse εξόδου", AssetCreationOptions.None);


εργασία. Submit();

Δημιουργία εκτύπωση προόδου και υποβολή ερωτημάτων εργασίες progressPrintTask εργασίας = νέα Task(() = > {

IJob jobQuery = null. Κάντε {var progressContext = περιβάλλοντος, jobQuery = progressContext.Jobs. Όπου (j = > j.Id == εργασία. ID). First(); Console.WriteLine (συμβολοσειρά. Μορφοποίηση ("\t \t {1} {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Εξέλιξη)); Thread.Sleep(10000); } ενώ (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Υποστηριζόμενοι τύποι αρχείων

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Σχετικές συνδέσεις

[Επισκόπηση ανάλυσης υπηρεσίες πολυμέσων Azure](media-services-analytics-overview.md)

[Azure επιδείξεις αναλυτικά στοιχεία πολυμέσων](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

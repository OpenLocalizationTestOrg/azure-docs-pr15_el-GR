<properties 
    pageTitle="Λήψη διαθέσιμων στοιχείων πολυμέσων" 
    description="Μάθετε πληροφορίες για να κάνετε λήψη διαθέσιμων στοιχείων στον υπολογιστή σας. Δείγματα κώδικα είναι γραμμένες σε C# και χρησιμοποιήστε το Media Services SDK για .NET." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Πώς μπορείτε να: παράδοση ενός περιουσιακού στοιχείου μέσω λήψης

Αυτό το θέμα περιγράφει τις επιλογές για την παροχή πόρους πολυμέσων που έχουν αποσταλεί με τις υπηρεσίες πολυμέσων. Μπορείτε να παραδώσετε Media Services περιεχόμενο σε πολλά σενάρια εφαρμογής. Μπορείτε να κάνετε λήψη πόρους πολυμέσων ή να αποκτήσετε πρόσβαση σε αυτά χρησιμοποιώντας ένα προσδιοριστικό. Μπορείτε να στείλετε περιεχομένου πολυμέσων σε μια άλλη εφαρμογή ή σε άλλη υπηρεσία παροχής περιεχομένου. Βελτιωμένες επιδόσεις και κλιμάκωση, μπορείτε επίσης να παραδώσετε περιεχομένου χρησιμοποιώντας ένα δίκτυο παράδοσης περιεχομένου (CDN).

Αυτό το παράδειγμα δείχνει πώς μπορείτε να κάνετε λήψη πόρους πολυμέσων από το Media Services στον τοπικό σας υπολογιστή. Ο κώδικας ερωτήματα τις εργασίες που σχετίζονται με το λογαριασμό Media Services, Αναγνωριστικό εργασίας και αποκτά πρόσβαση η συλλογή **OutputMediaAssets** (το οποίο είναι το σύνολο των έναν ή περισσότερους πόρους πολυμέσων εξόδου που προκύπτει από την εκτέλεση μιας εργασίας). Αυτό το παράδειγμα δείχνει πώς μπορείτε να κάνετε λήψη πόρους πολυμέσων εξόδου από μια εργασία, αλλά μπορείτε να εφαρμόσετε την ίδια προσέγγιση για να κάνετε λήψη άλλων περιουσιακών στοιχείων.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Δείτε επίσης 

[Παράδοση ροή περιεχομένου](media-services-deliver-streaming-content.md)


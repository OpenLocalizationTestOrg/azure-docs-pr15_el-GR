<properties 
    pageTitle="Δημοσίευση περιεχομένου υπηρεσιών Azure Media Services χρησιμοποιώντας .NET" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα προσδιοριστικό που χρησιμοποιείται για να δημιουργήσετε μια ροή διεύθυνση URL. Δείγματα κώδικα είναι γραμμένες σε C# και χρησιμοποιήστε το Media Services SDK για .NET." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="publish-azure-media-services-content-using-net"></a>Δημοσίευση περιεχομένου υπηρεσιών Azure Media Services χρησιμοποιώντας .NET
 
> [AZURE.SELECTOR]
- [ΥΠΌΛΟΙΠΟ](media-services-rest-deliver-streaming-content.md)
- [.NET](media-services-deliver-streaming-content.md)
- [Πύλη](media-services-portal-publish.md)

##<a name="overview"></a>Επισκόπηση

Μπορείτε να μεταδώσετε με ροή μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση κατά τη δημιουργία μιας ροής προσδιοριστικό OnDemand και τη δημιουργία μιας ροής διεύθυνσης URL. Το θέμα [κωδικοποίηση ενός περιουσιακού στοιχείου](media-services-encode-asset.md) δείχνει πώς μπορείτε να κωδικοποιήσετε σε μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση. 

>[AZURE.NOTE]Εάν το περιεχόμενό σας είναι κρυπτογραφημένο, ρυθμίστε τις παραμέτρους πολιτικής παράδοσης περιουσιακών στοιχείων (όπως περιγράφεται σε [αυτό](media-services-dotnet-configure-asset-delivery-policy.md) το θέμα) πριν να δημιουργήσετε ένα προσδιοριστικό. 

Μπορείτε επίσης να χρησιμοποιήσετε μια OnDemand ροή προσδιοριστικό για να δημιουργήσετε διευθύνσεις URL που οδηγούν σε αρχεία MP4 που μπορεί να ληφθεί σταδιακά.  

Αυτό το θέμα δείχνει πώς μπορείτε να δημιουργήσετε μια OnDemand ροή εντοπισμού προκειμένου να δημοσιεύσετε το περιουσιακών στοιχείων και δημιουργήστε μια ομαλές ΠΑΎΛΑΣ MPEG και HLS ροή διευθύνσεις URL. Εμφανίζει επίσης συντόμευσης για τη δημιουργία προοδευτική λήψη διευθύνσεις URL. 
     
##<a name="create-an-ondemand-streaming-locator"></a>Δημιουργήστε μια OnDemand ροή προσδιοριστικό

Για να δημιουργήσετε το OnDemand ροή προσδιοριστικό και λάβετε διευθύνσεις URL πρέπει να κάνετε τα εξής:

   1. Εάν το περιεχόμενο είναι κρυπτογραφημένο, Ορισμός πολιτικής πρόσβασης.
   2. Δημιουργήστε μια OnDemand ροή προσδιοριστικό.
   3. Εάν πρόκειται για τη ροή, η λήψη ροής αρχείο δήλωσης (.ism) στο περιουσιακού στοιχείου. 
        
    Εάν σκοπεύετε να κάνετε λήψη σταδιακά, λάβετε τα ονόματα των αρχείων MP4 στο περιουσιακού στοιχείου.  
   4. Δημιουργία διευθύνσεις URL για το αρχείο δηλώσεων ή τα αρχεία MP4. 
   

###<a name="use-media-services-net-sdk"></a>Χρήση των υπηρεσιών πολυμέσων .NET SDK 

Δημιουργία ροής διευθύνσεις URL 

    private static void BuildStreamingURLs(IAsset asset)
    {
    
        // Create a 30-day readonly access policy. 
        // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();
        
        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }

Εξόδους του κώδικα:
    
    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)
    

>[AZURE.NOTE]Μπορείτε επίσης να κατευθύνετε το περιεχόμενό σας μέσω σύνδεσης SSL. Για να το κάνετε αυτό, βεβαιωθείτε ότι το ροής διευθύνσεις URL που ξεκινούν με HTTPS. 

Δημιουργία προοδευτική λήψη διευθύνσεις URL 

    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);
    
        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));
    
        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();
    
        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));
                
        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }

Εξόδους του κώδικα:
    
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4
    
    . . . 

###<a name="use-media-services-net-sdk-extensions"></a>Χρήση επεκτάσεις SDK .NET υπηρεσίες πολυμέσων

Ο ακόλουθος κώδικας κλήσεις .NET SDK επεκτάσεις μεθόδους που δημιουργείτε ένα προσδιοριστικό και δημιουργία την ομαλή ροή, HLS και MPEG-ΔΙΑΚΕΚΟΜΜΈΝΗ διευθύνσεις URL για την προσαρμόσιμη ροής.

    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));
    
    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();
    
    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης

[Λήψη διαθέσιμων στοιχείων](media-services-deliver-asset-download.md)
[Ρύθμιση παραμέτρων πολιτικής παράδοσης περιουσιακών στοιχείων](media-services-dotnet-configure-asset-delivery-policy.md)

<properties 
    pageTitle="Δημιουργία φίλτρων με τις υπηρεσίες πολυμέσων Azure .NET SDK" 
    description="Αυτό το θέμα περιγράφει τον τρόπο δημιουργίας φίλτρων, ώστε το πρόγραμμα-πελάτη να τα χρησιμοποιήσετε για συγκεκριμένες ενότητες ροής μιας ροής. Υπηρεσίες πολυμέσων δημιουργεί δυναμικής δηλώσεων για να επιτύχετε αυτό επιλεκτική ροής." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Δημιουργία φίλτρων με τις υπηρεσίες πολυμέσων Azure .NET SDK

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-dynamic-manifest.md)

Ξεκινώντας με την έκδοση 2.11, Media Services σάς επιτρέπει να ορίσετε φίλτρα για τους πόρους σας. Αυτά τα φίλτρα είναι κανόνων στην πλευρά του διακομιστή που θα επιτρέπουν στους πελάτες σας να επιλέξετε να κάνετε πράγματα όπως: αναπαραγωγή μόνο ενός τμήματος του βίντεο (αντί για αναπαραγωγή του βίντεο ολόκληρη), ή να καθορίσετε μόνο ένα υποσύνολο των αποδόσεων ήχου και βίντεο που μπορεί να διαχειριστεί συσκευή του πελάτη σας (αντί για όλες τις αποδόσεις που συσχετίζονται με το περιουσιακών στοιχείων). Αυτό το φιλτράρισμα από τους πόρους σας είναι δυνατό μέσω s **Δυναμικής δήλωσης**που έχουν δημιουργηθεί μετά από αίτηση του πελάτη σας για τη ροή βίντεο που βασίζονται σε καθορισμένη φίλτρα.

Για πιο λεπτομερείς πληροφορίες σχετικά με τα φίλτρα και δυναμική δήλωσης, ανατρέξτε στο θέμα [Επισκόπηση δυναμικής δηλώσεων](media-services-dynamic-manifest-overview.md).

Αυτό το θέμα δείχνει πώς μπορείτε να χρησιμοποιήσετε Media Services .NET SDK για δημιουργία, ενημέρωση και διαγραφή φίλτρα. 


Σημείωση Εάν ενημερώνετε ένα φίλτρο, μπορεί να χρειαστούν έως 2 λεπτά για ροή τελικό σημείο για την ανανέωση των κανόνων. Εάν το περιεχόμενο έχει εξυπηρέτησης με χρήση αυτού του φίλτρου (και στο cache σε διακομιστές μεσολάβησης και CDN μνήμης cache), αυτό το φίλτρο ενημέρωση μπορεί να προκαλέσει αποτυχίες προγράμματος αναπαραγωγής. Είναι συνιστάται να απαλείψετε το cache μετά την ενημέρωση του φίλτρου. Εάν αυτή η επιλογή δεν είναι δυνατό, μπορείτε να χρησιμοποιήσετε ένα διαφορετικό φίλτρο. 

##<a name="types-used-to-create-filters"></a>Τύποι που χρησιμοποιούνται για τη δημιουργία φίλτρων

Οι παρακάτω τύποι χρησιμοποιούνται κατά τη δημιουργία φίλτρα: 

- **IStreamingFilter**.  Αυτός ο τύπος είναι με βάση το παρακάτω REST API [φίλτρου](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Αυτός ο τύπος είναι με βάση το παρακάτω REST API [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Αυτός ο τύπος είναι με βάση το παρακάτω REST API [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** και **IFilterTrackPropertyCondition**. Αυτοί οι τύποι βάση την παρακάτω REST API [FilterTrackSelect και FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Δημιουργία/ενημέρωση/ανάγνωση/διαγραφή καθολικά φίλτρα

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET για να δημιουργήσετε, να ενημερώσετε, διαβάστε και να διαγράψετε τα φίλτρα περιουσιακών στοιχείων.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Φίλτρα δημιουργία/ενημέρωση/ανάγνωση/διαγραφή περιουσιακών στοιχείων

Ο ακόλουθος κώδικας δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET για να δημιουργήσετε, να ενημερώσετε, διαβάστε και να διαγράψετε τα φίλτρα περιουσιακών στοιχείων.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Δημιουργία ροής διευθύνσεις URL που χρησιμοποιούν τα φίλτρα

Για πληροφορίες σχετικά με τον τρόπο για να δημοσιεύσετε και να κάνουν τους πόρους σας, ανατρέξτε στο θέμα [Την παράδοση περιεχομένου στην Επισκόπηση πελάτες](media-services-deliver-content-overview.md).


Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να προσθέσετε φίλτρα ροής τις διευθύνσεις URL σας.

**MPEG ΠΑΎΛΑ** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live ροής V4 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live ροής V3 (HLS)**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Ομαλή ροή**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**ΣΚΛΗΡΟΊ ΔΊΣΚΟΙ**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Δείτε επίσης 

[Επισκόπηση δυναμικής δηλώσεων](media-services-dynamic-manifest-overview.md)
 


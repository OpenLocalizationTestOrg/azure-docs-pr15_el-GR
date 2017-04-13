<properties 
    pageTitle="Δημοσίευση περιεχομένου υπηρεσιών Azure Media Services χρησιμοποιώντας ΥΠΌΛΟΙΠΟ" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα προσδιοριστικό που χρησιμοποιείται για να δημιουργήσετε μια ροή διεύθυνση URL. Ο κώδικας χρησιμοποιεί REST API." 
    authors="Juliako" 
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


# <a name="publish-azure-media-services-content-using-rest"></a>Δημοσίευση περιεχομένου υπηρεσιών Azure Media Services χρησιμοποιώντας ΥΠΌΛΟΙΠΟ

> [AZURE.SELECTOR]
- [.NET](media-services-deliver-streaming-content.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-deliver-streaming-content.md)
- [Πύλη](media-services-portal-publish.md)

##<a name="overview"></a>Επισκόπηση


Μπορείτε να μεταδώσετε με ροή μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση κατά τη δημιουργία μιας ροής προσδιοριστικό OnDemand και τη δημιουργία μιας ροής διεύθυνσης URL. Το θέμα [κωδικοποίηση ενός περιουσιακού στοιχείου](media-services-rest-encode-asset.md) δείχνει πώς μπορείτε να κωδικοποιήσετε σε μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 ρύθμιση. Εάν το περιεχόμενό σας είναι κρυπτογραφημένο, ρυθμίστε τις παραμέτρους πολιτικής παράδοσης περιουσιακών στοιχείων (όπως περιγράφεται σε [αυτό](media-services-rest-configure-asset-delivery-policy.md) το θέμα) πριν να δημιουργήσετε ένα προσδιοριστικό. 

Μπορείτε επίσης να χρησιμοποιήσετε μια OnDemand ροή προσδιοριστικό για να δημιουργήσετε διευθύνσεις URL που οδηγούν σε αρχεία MP4 που μπορεί να ληφθεί σταδιακά.  

Αυτό το θέμα δείχνει πώς μπορείτε να δημιουργήσετε μια OnDemand ροή εντοπισμού προκειμένου να δημοσιεύσετε το περιουσιακών στοιχείων και δημιουργήστε μια ομαλές ΠΑΎΛΑΣ MPEG και HLS ροή διευθύνσεις URL. Εμφανίζει επίσης συντόμευσης για τη δημιουργία προοδευτική λήψη διευθύνσεις URL.

Στην [παρακάτω](#types) ενότητα εμφανίζει τους τύπους απαρίθμηση των οποίων οι τιμές που χρησιμοποιούνται στις ΥΠΌΛΟΙΠΕΣ κλήσεις.   
  
##<a name="create-an-ondemand-streaming-locator"></a>Δημιουργήστε μια OnDemand ροή προσδιοριστικό

Για να δημιουργήσετε το OnDemand ροή προσδιοριστικό και λάβετε διευθύνσεις URL πρέπει να κάνετε τα εξής:


   1. Εάν το περιεχόμενο είναι κρυπτογραφημένο, Ορισμός πολιτικής πρόσβασης.
   2. Δημιουργήστε μια OnDemand ροή προσδιοριστικό.
   3. Εάν πρόκειται για τη ροή, η λήψη ροής αρχείο δήλωσης (.ism) στο περιουσιακού στοιχείου. 
        
    Εάν σκοπεύετε να κάνετε λήψη σταδιακά, λάβετε τα ονόματα των αρχείων MP4 στο περιουσιακού στοιχείου. 
   4. Δημιουργία διευθύνσεις URL για το αρχείο δηλώσεων ή τα αρχεία MP4. 
   5. Σημειώστε ότι δεν μπορείτε να δημιουργήσετε μια ροή προσδιοριστικό χρησιμοποιώντας μια AccessPolicy που περιλαμβάνει σύνταξη ή διαγραφή δικαιωμάτων.


###<a name="create-an-access-policy"></a>Δημιουργήστε μια πολιτική πρόσβασης

Αίτηση:
        
    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    Host: media.windows.net
    Content-Length: 68
    
    {"Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}
    
Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 311
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https:/media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3A69c80d98-7830-407f-a9af-e25f4b0d3e5f')
    Server: Microsoft-IIS/8.5
    request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-request-id: a877528a-bdb4-4414-9862-273f8e64f882
    x-ms-client-request-id: 6bcfd511-a561-448d-a022-a319a89ecffa
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:52:09 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AccessPolicies/@Element","Id":"nb:pid:UUID:69c80d98-7830-407f-a9af-e25f4b0d3e5f","Created":"2015-02-18T06:52:09.8862191Z","LastModified":"2015-02-18T06:52:09.8862191Z","Name":"access policy","DurationInMinutes":43200.0,"Permissions":1}

###<a name="create-an-ondemand-streaming-locator"></a>Δημιουργήστε μια OnDemand ροή προσδιοριστικό

Δημιουργήστε το προσδιοριστικό για την καθορισμένη περιουσιακών στοιχείων και περιουσιακών στοιχείων πολιτικής.

Αίτηση:
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstest1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1424263184&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=NWE%2f986Hr5lZTzVGKtC%2ftzHm9n6U%2fxpTFULItxKUGC4%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    Host: media.windows.net
    Content-Length: 181
    
    {"AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872Z","Type":2}

Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 637
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Abe245661-2bbd-4fc6-b14f-9cf9a1492e5e')
    Server: Microsoft-IIS/8.5
    request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-request-id: 5bd5864a-0afd-44c0-a67a-4044a2c9043b
    x-ms-client-request-id: ac159492-9a0c-40c3-aacc-551b1b4c5f62
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 18 Feb 2015 06:58:37 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#Locators/@Element","Id":"nb:lid:UUID:be245661-2bbd-4fc6-b14f-9cf9a1492e5e","ExpirationDateTime":"2015-03-20T06:34:47.267872+00:00","Type":2,"Path":"http://amstest1.streaming.mediaservices.windows.net/be245661-2bbd-4fc6-b14f-9cf9a1492e5e/","BaseUri":"http://amstest1.streaming.mediaservices.windows.net","ContentAccessComponent":"be245661-2bbd-4fc6-b14f-9cf9a1492e5e","AccessPolicyId":"nb:pid:UUID:1480030d-c481-430a-9687-535c6a5cb272","AssetId":"nb:cid:UUID:cc1e445d-1500-80bd-538e-f1e4b71b465e","StartTime":"2015-02-18T06:34:47.267872+00:00","Name":null}

###<a name="build-streaming-urls"></a>Δημιουργία ροής διευθύνσεις URL

Χρησιμοποιήστε την τιμή **διαδρομή** επιστρέφεται μετά τη δημιουργία της το προσδιοριστικό για να δημιουργήσετε το ομαλές HLS και διευθύνσεις URL ΠΑΎΛΑΣ MPEG. 

Ομαλή ροής: **Διαδρομή** + το όνομα αρχείου δηλώσεων + "/ δήλωσης"

παράδειγμα:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest

HLS: **Διαδρομή** + δηλωτικό όνομα αρχείου + "/ manifest(format=m3u8-aapl)"

παράδειγμα:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)


ΠΑΎΛΑ: **Διαδρομή** + το όνομα αρχείου δηλώσεων + "/ manifest(format=mpd-time-csf)"


παράδειγμα:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


###<a name="build-progressive-download-urls"></a>Δημιουργία προοδευτική λήψη διευθύνσεις URL

Χρησιμοποιήστε την τιμή **διαδρομή** επιστρέφεται μετά τη δημιουργία της το προσδιοριστικό για να δημιουργήσετε τη διεύθυνση URL προοδευτική λήψη.   

Διεύθυνση URL: **Διαδρομή** + περιουσιακών στοιχείων mp4 όνομα αρχείου

παράδειγμα:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

##<a id="types"></a>Τύποι απαρίθμησης

    [Flags]
    public enum AccessPermissions
    {
        None = 0,
        Read = 1,
        Write = 2,
        Delete = 4,
        List = 8,
    }

    public enum LocatorType
    {
        None = 0,
        Sas = 1,
        OnDemandOrigin = 2,
    }

##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης

[Ρύθμιση παραμέτρων της πολιτικής παράδοσης περιουσιακών στοιχείων](media-services-rest-configure-asset-delivery-policy.md)

<properties 
    pageTitle="Αποστολή αρχείων σε λογαριασμό Media Services χρησιμοποιώντας ΥΠΌΛΟΙΠΑ | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να κατανοήσετε περιεχομένου πολυμέσων Media Services με τη δημιουργία και αποστολή περιουσιακών στοιχείων." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
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


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Αποστολή αρχείων σε λογαριασμό Media Services χρησιμοποιώντας ΥΠΌΛΟΙΠΟ

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [ΥΠΌΛΟΙΠΟ](media-services-rest-upload-files.md)
 - [Πύλη](media-services-portal-upload-files.md)

Στις υπηρεσίες πολυμέσων, μπορείτε να αποστείλετε τα αρχεία σας ψηφιακού σε ενός περιουσιακού στοιχείου. Η οντότητα [περιουσιακών στοιχείων](https://msdn.microsoft.com/library/azure/hh974277.aspx) μπορεί να περιέχει βίντεο, ήχο, εικόνες, μικρογραφιών συλλογές, κείμενο κομμάτια και κλειστές λεζάντες αρχείων (και τα μετα-δεδομένα σχετικά με αυτά τα αρχεία.)  Όταν τα αρχεία έχουν αποσταλεί στο περιουσιακού στοιχείου, το περιεχόμενό σας αποθηκεύονται με ασφάλεια στο cloud για περαιτέρω επεξεργασία και ροής. 

>[AZURE.NOTE]Τα παρακάτω ζητήματα εφαρμόζονται κατά την επιλογή ονόματος ενός αρχείου περιουσιακών στοιχείων:
>
>- Υπηρεσίες πολυμέσων χρησιμοποιεί την τιμή της ιδιότητας IAssetFile.Name κατά τη δημιουργία διευθύνσεις URL για τη ροή περιεχομένου (για παράδειγμα, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Για αυτόν το λόγο, δεν επιτρέπεται η κωδικοποίηση. Η τιμή της ιδιότητας **όνομα** δεν μπορεί να έχει έναν από τους παρακάτω [χαρακτήρες τοις εκατό κωδικοποίηση-αποκλειστικές](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Επίσης, μπορεί να υπάρξει μόνο μία '.' για την επέκταση ονόματος αρχείου.
>
>- Το μήκος του ονόματος δεν πρέπει να είναι μεγαλύτερο από 260 χαρακτήρες.

Η βασική ροή εργασίας για αποστολή περιουσιακών στοιχείων χωρίζεται σε ενότητες που ακολουθούν:

- Δημιουργία ενός περιουσιακού στοιχείου
- Κρυπτογράφηση ενός περιουσιακού στοιχείου (προαιρετικά)
- Αποστολή ενός αρχείου στο χώρο αποθήκευσης αντικειμένων blob

Αθροιστικής μέτρησης ενισχύσεων σάς επιτρέπει επίσης να αποστείλετε παγίων μαζικά. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτήν](media-services-rest-upload-files.md#upload_in_bulk) την ενότητα.

##<a name="upload-assets"></a>Αποστολή περιουσιακών στοιχείων

###<a name="create-an-asset"></a>Δημιουργία ενός περιουσιακού στοιχείου

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 
 
Ενός περιουσιακού στοιχείου είναι ένα κοντέινερ για πολλούς τύπους ή σύνολα αντικειμένων σε υπηρεσίες πολυμέσων, όπως βίντεο, ήχο, εικόνες, συλλογές μικρογραφιών, κομμάτια κειμένου και αρχεία κλειστές λεζάντες. Στο το REST API, τη δημιουργία ενός περιουσιακού στοιχείου απαιτεί Αποστολή αίτησης ΔΗΜΟΣΊΕΥΣΗ στις υπηρεσίες πολυμέσων και τη διάθεση οποιαδήποτε ιδιότητα πληροφορίες σχετικά με το περιουσιακών στοιχείων στο σώμα της αίτησης.

Μία από τις ιδιότητες που μπορείτε να καθορίσετε κατά τη δημιουργία ενός περιουσιακού στοιχείου είναι **Επιλογές**. **Επιλογές** είναι μια τιμή απαρίθμησης που περιγράφει τις επιλογές κρυπτογράφησης που μπορούν να δημιουργηθούν ενός περιουσιακού στοιχείου με. Μια έγκυρη τιμή είναι μία από τις τιμές από τη λίστα παρακάτω, δεν ένας συνδυασμός τιμών. 

- **Καμία** = **0**: χωρίς κρυπτογράφηση θα χρησιμοποιηθεί. Αυτή είναι η προεπιλεγμένη τιμή. Σημειώστε ότι όταν χρησιμοποιείτε αυτήν την επιλογή το περιεχόμενό σας δεν είναι προστατευμένο κατά τη μεταφορά ή στο υπόλοιπο στο χώρο αποθήκευσης.
    Εάν σκοπεύετε να κάνετε μια MP4 χρησιμοποιώντας προοδευτική λήψη, χρησιμοποιήστε αυτήν την επιλογή. 

- **StorageEncrypted** = **1**: Καθορίστε εάν θέλετε για τα αρχεία σας για να είναι κρυπτογραφημένα με κρυπτογράφηση AES-256 bit για αποστολή και το χώρο αποθήκευσης.

    Εάν σας περιουσιακού στοιχείου είναι κρυπτογραφημένα χώρου αποθήκευσης, πρέπει να ρυθμίσετε περιουσιακών στοιχείων παράδοσης πολιτικής. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων περιουσιακών στοιχείων παράδοσης πολιτικής](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: Καθορίστε εάν στέλνετε αρχεία που προστατεύονται με μια συνηθισμένη μέθοδος κρυπτογράφησης (όπως PlayReady). 

- **EnvelopeEncryptionProtected** = **4**: Καθορίστε εάν στέλνετε HLS κρυπτογραφημένα με τα αρχεία AES. Σημειώστε ότι τα αρχεία πρέπει να έχουν κωδικοποιημένη και κρυπτογραφημένα με μετασχηματισμός Manager.

>[AZURE.NOTE]Εάν πρόκειται να χρησιμοποιήσετε κρυπτογράφηση σας περιουσιακών στοιχείων, πρέπει να δημιουργήσετε ένα **ContentKey** και να το συνδέσετε με το περιουσιακών στοιχείων όπως περιγράφεται στο παρακάτω θέμα:[πώς μπορείτε να δημιουργήσετε ένα ContentKey](media-services-rest-create-contentkey.md). Σημειώστε ότι μετά την αποστολή των αρχείων σε περιουσιακού στοιχείου, πρέπει να ενημερώσετε τις ιδιότητες κρυπτογράφησης στην οντότητα **AssetFile** με τις τιμές που λάβατε κατά την κρυπτογράφηση **περιουσιακών στοιχείων** . Κάνετε χρησιμοποιώντας μια αίτηση HTTP **ΣΥΓΧΏΝΕΥΣΗ** . 


Το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας ενός περιουσιακού στοιχείου.

**Αίτηση HTTP**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Δημιουργήστε μια AssetFile

Η οντότητα [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) αντιπροσωπεύει ένα αρχείο βίντεο ή ήχου που είναι αποθηκευμένο σε ένα κοντέινερ αντικειμένων blob. Ένα αρχείο περιουσιακού στοιχείου είναι πάντα συσχετισμένη με ενός περιουσιακού στοιχείου και ενός περιουσιακού στοιχείου μπορεί να περιέχει ένα ή περισσότερα αρχεία περιουσιακών στοιχείων. Η εργασία Media Encoder υπηρεσίες αποτυγχάνει εάν ένα αντικείμενο αρχείου περιουσιακών στοιχείων δεν έχει συσχετιστεί με ένα ψηφιακό αρχείο σε ένα κοντέινερ αντικειμένων blob.

Σημειώστε ότι η παρουσία **AssetFile** και το αρχείο πραγματική πολυμέσων είναι δύο ξεχωριστά αντικείμενα. Η παρουσία AssetFile περιέχει μετα-δεδομένα σχετικά με το αρχείο πολυμέσων, ενώ το αρχείο περιέχει το περιεχόμενο πραγματική πολυμέσων.

Μετά την αποστολή του αρχείου σας ψηφιακών πολυμέσων σε ένα κοντέινερ αντικειμένων blob, θα μπορείτε να χρησιμοποιήσετε την αίτηση HTTP **ΣΥΓΧΏΝΕΥΣΗ** για να ενημερώσετε το AssetFile με πληροφορίες σχετικά με το αρχείο σας πολυμέσων (όπως φαίνεται παρακάτω στο θέμα). 

**Αίτηση HTTP**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Απόκριση HTTP**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Δημιουργία του AccessPolicy με δικαιώματα εγγραφής. 

Πριν από την αποστολή των αρχείων στο χώρο αποθήκευσης αντικειμένων blob, ορίστε την πρόσβαση πολιτική δικαιώματα για την εγγραφή σε ενός περιουσιακού στοιχείου. Για να το κάνετε, ΔΗΜΟΣΙΕΎΣΤΕ μια αίτηση HTTP στο σύνολο οντότητα AccessPolicies. Ορισμός τιμής DurationInMinutes κατά τη δημιουργία ή λαμβάνετε μηνύματος σφάλματος 500 εσωτερικό διακομιστή πίσω στην απάντηση. Για περισσότερες πληροφορίες σχετικά με AccessPolicies, ανατρέξτε στο θέμα [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια AccessPolicy:
        
**Αίτηση HTTP**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**Αίτηση HTTP**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Λήψη της διεύθυνσης URL αποστολής

Για να λάβετε τη διεύθυνση URL του πραγματικού αποστολής, δημιουργήστε ένα προσδιοριστικό συσχετισμών Ασφαλείας. Locators Ορισμός της ώρας έναρξης και τον τύπο του τελικού σημείου σύνδεσης για προγράμματα-πελάτες που θέλετε να αποκτήσετε πρόσβαση σε αρχεία σε ενός περιουσιακού στοιχείου. Μπορείτε να δημιουργήσετε πολλών οντοτήτων προσδιοριστικό για μια δεδομένη ζεύγος AccessPolicy και περιουσιακών στοιχείων χειρισμού των προσκλήσεων σε διαφορετικό πρόγραμμα-πελάτη και τις ανάγκες. Κάθε μία από αυτές τις Locators χρησιμοποιήσετε η τιμή ώρας έναρξης συν την τιμή DurationInMinutes το AccessPolicy για να καθορίσετε το χρονικό διάστημα που μπορεί να χρησιμοποιηθεί μια διεύθυνση URL. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [προσδιοριστικό](http://msdn.microsoft.com/library/azure/hh974308.aspx).


Μια διεύθυνση URL συσχετισμών Ασφαλείας έχει την παρακάτω μορφή:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Εφαρμογή ορισμένα ζητήματα:

- Δεν μπορείτε να έχετε περισσότερους από πέντε μοναδικό Locators που σχετίζεται με μια δεδομένη περιουσιακών στοιχείων κάθε φορά. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα προσδιοριστικό.
- Εάν χρειαστεί να αποστείλετε τα αρχεία σας αμέσως, θα πρέπει να μπορείτε να ορίσετε την τιμή ώρας έναρξης έως πέντε λεπτά πριν από την τρέχουσα ώρα. Αυτό συμβαίνει επειδή μπορεί να υπάρχουν ρολόι skew μεταξύ του υπολογιστή-πελάτη και τις υπηρεσίες πολυμέσων σας. Επίσης, πρέπει να είναι η τιμή ώρας έναρξης σας με την εξής μορφή ημερομηνίας/ώρας: εεεε-MM-DDTHH:mm:ssZ (για παράδειγμα, "2014-05-23T17:53:50Z").   
- Μπορεί να υπάρχουν σε δευτερόλεπτα 30-40 καθυστέρηση αφού δημιουργηθεί ένα προσδιοριστικό να όταν είναι διαθέσιμο για χρήση. Αυτό το θέμα ισχύει για το URL συσχετισμών Ασφαλείας και Origin Locators.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια διεύθυνση URL Locator συσχετισμών Ασφαλείας, όπως καθορίζεται από την ιδιότητα τύπου στο σώμα της αίτησης ("1" για ένα προσδιοριστικό συσχετισμών Ασφαλείας) και "2" για ένα προσδιοριστικό origin On ζήτηση. Η ιδιότητα **διαδρομή** επιστρέφονται περιέχει τη διεύθυνση URL που πρέπει να χρησιμοποιήσετε για να αποστείλετε το αρχείο σας.
    
**Αίτηση HTTP**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Αποστολή αρχείου σε ένα κοντέινερ χώρου αποθήκευσης αντικειμένων blob
    
Μόλις η AccessPolicy και ορισμός προσδιοριστικό, το ίδιο το αρχείο αποστέλλεται σε ένα κοντέινερ χώρο αποθήκευσης Blob του Azure χρησιμοποιώντας τα Azure αποθήκευσης REST API. Μπορείτε να αποστείλετε στη σελίδα ή να αποκλείσετε αντικείμενα blob. 

>[AZURE.NOTE] Πρέπει να προσθέσετε το όνομα του αρχείου για το αρχείο που θέλετε να αποστείλετε στην τιμή προσδιοριστικό **διαδρομή** λάβει στην προηγούμενη ενότητα. Για παράδειγμα, https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Για περισσότερες πληροφορίες σχετικά με την εργασία με αντικείμενα BLOB Azure χώρου αποθήκευσης, ανατρέξτε στο θέμα [Blob υπηρεσίας REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Ενημέρωση του AssetFile 

Τώρα που έχετε αποστείλει το αρχείο σας, ενημερώστε τις πληροφορίες FileAsset μέγεθος (και άλλα). Για παράδειγμα:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**Απόκριση HTTP**

Εάν επιτυχής, τα ακόλουθα επιστρέφεται: HTTP/1.1 204 χωρίς περιεχόμενο

### <a name="delete-the-locator-and-accesspolicy"></a>Διαγράψτε το προσδιοριστικό και AccessPolicy 

**Αίτηση HTTP**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:

    HTTP/1.1 204 No Content 
    ...

**Αίτηση HTTP**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**Απόκριση HTTP**

Εάν είναι επιτυχής, επιστρέφεται τα εξής:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Αποστολή παγίων μαζικά

###<a name="create-the-ingestmanifest"></a>Δημιουργία του IngestManifest

Το IngestManifest είναι ένα κοντέινερ για ένα σύνολο περιουσιακών στοιχείων, αρχεία περιουσιακών στοιχείων και στατιστικά στοιχεία που μπορούν να χρησιμοποιηθούν για να προσδιορίσετε την πρόοδο της μαζικής ingesting για το σύνολο.


**Αίτηση HTTP**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Δημιουργία περιουσιακών στοιχείων

Πριν από τη δημιουργία του IngestManifestAsset, πρέπει να δημιουργήσετε το περιουσιακών στοιχείων που θα ολοκληρωθεί χρησιμοποιώντας τη μαζική ingesting. Ενός περιουσιακού στοιχείου είναι ένα κοντέινερ για πολλούς τύπους ή σύνολα αντικειμένων σε υπηρεσίες πολυμέσων, όπως βίντεο, ήχο, εικόνες, συλλογές μικρογραφιών, κομμάτια κειμένου και αρχεία κλειστές λεζάντες. Στο το REST API, η δημιουργία ενός περιουσιακού στοιχείου απαιτεί στείλετε μια αίτηση HTTP POST στο Microsoft Azure Media Services και τη διάθεση οποιαδήποτε ιδιότητα πληροφορίες σχετικά με το περιουσιακών στοιχείων στο σώμα της αίτησης. Σε αυτό το παράδειγμα, παγίου δημιουργείται χρησιμοποιώντας την επιλογή StorageEncrption(1) που περιλαμβάνεται στο σώμα της αίτησης.

**Απόκριση HTTP**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Δημιουργία του IngestManifestAssets

IngestManifestAssets αντιπροσωπεύουν στοιχεία μέσα σε ένα IngestManifest που χρησιμοποιούνται με μαζική ingesting. Η σύνδεση στην ουσία παγίου με δηλωτικό. Azure Media Services εσωτερικά παρακολουθεί η αποστολή του αρχείου που βασίζεται σε συλλογή IngestManifestFiles που έχει συσχετιστεί με το IngestManifestAsset. Εφόσον αυτά τα αρχεία έχουν αποσταλεί, παγίου έχει ολοκληρωθεί. Μπορείτε να δημιουργήσετε μια νέα IngestManifestAsset με μια αίτηση HTTP POST. Στο σώμα της αίτησης, συμπεριλάβετε το αναγνωριστικό IngestManifest και το αναγνωριστικό περιουσιακών στοιχείων που το IngestManifestAsset θα πρέπει να συνδέετε για μαζική ingesting.

**Απόκριση HTTP**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Δημιουργία του IngestManifestFiles για κάθε περιουσιακών στοιχείων

Μια IngestManifestFile αντιπροσωπεύει ένα αντικείμενο πραγματική blob βίντεο ή ήχου που θα αποσταλούν ως μέρος της μαζικής ingesting για ενός περιουσιακού στοιχείου. Κρυπτογράφηση που σχετίζονται με ιδιότητες δεν είναι απαραίτητα, εκτός εάν παγίου χρησιμοποιεί μια επιλογή κρυπτογράφησης. Το παράδειγμα που χρησιμοποιείται σε αυτήν την ενότητα δείχνει τη δημιουργία μιας IngestManifestFile που χρησιμοποιεί StorageEncryption του παγίου που έχετε δημιουργήσει ήδη.


**Απόκριση HTTP**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Αποστείλετε τα αρχεία στο χώρο αποθήκευσης αντικειμένων Blob

Μπορείτε να χρησιμοποιήσετε οποιαδήποτε εφαρμογή του προγράμματος-πελάτη υψηλής ταχύτητας δυνατότητα αποστολή των αρχείων περιουσιακών στοιχείων στο κοντέινερ χώρου αποθήκευσης αντικειμένων blob Uri που παρέχεται από την ιδιότητα BlobStorageUriForUpload το IngestManifest. Μία υπηρεσία αποστολής αξιοσημείωτες υψηλής ταχύτητας είναι [Aspera On Demand για την εφαρμογή Azure](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Οθόνη μαζική Ingest προόδου

Μπορείτε να παρακολουθήσετε την πρόοδο της μαζικής ingesting λειτουργίες για μια IngestManifest από σταθμοσκόπησης την ιδιότητα στατιστικά στοιχεία από το IngestManifest. Ότι η ιδιότητα είναι ένα σύνθετο τύπο, [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx). Για να ψηφοφορία με την ιδιότητα στατιστικά στοιχεία, υποβάλετε μια αίτηση HTTP GET διέρχεται το αναγνωριστικό IngestManifest.
 

##<a name="create-contentkeys-used-for-encryption"></a>Δημιουργία ContentKeys που χρησιμοποιούνται για την κρυπτογράφηση

Εάν πρόκειται να χρησιμοποιήσετε κρυπτογράφηση σας περιουσιακών στοιχείων, πρέπει να δημιουργήσετε το ContentKey μπορούν να χρησιμοποιηθούν για την κρυπτογράφηση πριν από τη δημιουργία περιουσιακού στοιχείου αρχείων. Για την κρυπτογράφηση χώρου αποθήκευσης, οι ακόλουθες ιδιότητες θα πρέπει να συμπεριλαμβάνεται στο σώμα της αίτησης.
 
Αίτηση για την ιδιότητα σώμα   | Περιγραφή
---|---
Αναγνωριστικό | Το αναγνωριστικό ContentKey τα οποία θα σας δημιουργούν αναφερόμαστε χρησιμοποιώντας την ακόλουθη μορφή, "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Πρόκειται για τον τύπο περιεχομένου κλειδιού μορφή ακέραιου αριθμού για αυτό το κλειδί περιεχομένου. Θα σας μεταβιβάζουν την τιμή 1 για την κρυπτογράφηση χώρου αποθήκευσης.
EncryptedContentKey | Μπορούμε να δημιουργήσουμε μια νέα περιεχομένου τιμή του κλειδιού που είναι μια τιμή 256-bit (32 byte). Το κλειδί είναι κρυπτογραφημένη χρησιμοποιώντας το πιστοποιητικό X.509 κρυπτογράφησης χώρου αποθήκευσης, το οποίο γίνεται ανάκτηση από το Microsoft Azure Media Services εκτελώντας μια αίτηση HTTP GET για τις GetProtectionKeyId και τις μεθόδους GetProtectionKey.
ProtectionKeyId | Αυτό είναι το αναγνωριστικό κλειδιού προστασίας για το πιστοποιητικό X.509 κρυπτογράφησης χώρου αποθήκευσης που χρησιμοποιήθηκε για την κρυπτογράφηση μας περιεχομένου κλειδιού.
ProtectionKeyType | Αυτός είναι ο τύπος κρυπτογράφησης για το κλειδί προστασίας που χρησιμοποιήθηκε για να κρυπτογραφήσετε το κλειδί περιεχομένου. Αυτή η τιμή είναι StorageEncryption(1) παράδειγμά μας.
Άθροισμα ελέγχου |Το υπολογιζόμενο άθροισμα ελέγχου MD5 για το κλειδί περιεχομένου. Υπολογίζεται, κρυπτογραφώντας το περιεχόμενο αναγνωριστικό με τον αριθμό-κλειδί περιεχομένου. Το παράδειγμα κώδικα παρουσιάζει πώς μπορείτε να υπολογίσετε το άθροισμα ελέγχου.


**Απόκριση HTTP**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Σύνδεση του ContentKey παγίου

Το ContentKey είναι συσχετισμένη με ένα ή περισσότερα στοιχεία με την αποστολή μιας αίτησης HTTP POST. Η ακόλουθη αίτηση είναι ένα παράδειγμα για να συνδέσετε το παράδειγμα ContentKey στο παράδειγμα πάγιο κατά αναγνωριστικό.

**Απόκριση HTTP**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**Απόκριση HTTP**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Επόμενο βήμα

Αναθεώρηση Media Services διαδικασίες εκμάθησης.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 

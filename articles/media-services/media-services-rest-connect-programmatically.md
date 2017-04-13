<properties 
    pageTitle="Σύνδεση στο λογαριασμό υπηρεσίες πολυμέσων χρησιμοποιώντας REST API | Microsoft Azure" 
    description="Αυτό το θέμα παρουσιάζει πώς μπορείτε να συνδεθείτε με τις υπηρεσίες πολυμέσων uisng REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Σύνδεση στο λογαριασμό υπηρεσίες πολυμέσων χρησιμοποιώντας υπηρεσίες πολυμέσων REST API

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-connect-programmatically.md)

Αυτό το θέμα περιγράφει τον τρόπο για να αποκτήσετε μια σύνδεση μέσω προγραμματισμού στο Microsoft Azure Media Services όταν προγραμματισμού με το Media Services REST API.

Κατά την πρόσβαση στο Microsoft Azure Media Services απαιτούνται οι δύο πράγματα: ένα διακριτικό πρόσβασης που παρέχεται από τις υπηρεσίες ελέγχου πρόσβασης Azure (ACS) και τις υπηρεσίες URI πολυμέσων ίδια. Μπορείτε να χρησιμοποιήσετε οποιοδήποτε μέσο θέλετε κατά τη δημιουργία αυτές τις αιτήσεις με την προϋπόθεση ότι μπορείτε να καθορίσετε τις τιμές σωστή κεφαλίδα και να μεταβίβαση στο διακριτικό πρόσβασης σωστά κατά την κλήση σε υπηρεσίες πολυμέσων.

Ακολουθήστε τα παρακάτω βήματα περιγράφουν τα πιο συνηθισμένα ροής εργασίας όταν χρησιμοποιείτε το Media Services REST API για να συνδεθεί με τις υπηρεσίες πολυμέσων:

1. Γρήγορα ένα διακριτικό πρόσβασης 
2. Σύνδεση με τις υπηρεσίες πολυμέσων URI 

    >[AZURE.NOTE] Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI.
Ενδέχεται επίσης να λάβετε μια απόκριση HTTP/1.1 200 που περιέχει την περιγραφή μετα-δεδομένων ODATA API.

3. Δημοσιεύστε τις επόμενες κλήσεις API για τη νέα διεύθυνση URL. 

    Για παράδειγμα, εάν μετά τη δοκιμή για να συνδεθείτε, έχετε στη διάθεσή σας τα εξής:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Θα πρέπει να καταχωρείτε τις επόμενες κλήσεις API https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Διεύθυνση ελέγχου πρόσβασης

Διεύθυνση ελέγχου πρόσβασης Media Services είναι https://wamsprodglobal001acs.accesscontrol.windows.net, εκτός από την περιοχή Βόρειας Κίνα, όπου είναι https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn.

##<a name="getting-an-access-token"></a>Γρήγορα ένα διακριτικό πρόσβασης

Για να αποκτήσετε πρόσβαση σε υπηρεσίες πολυμέσων απευθείας από το REST API, ανακτούν ένα διακριτικό πρόσβασης από ACS και χρησιμοποιήστε το κατά τη διάρκεια κάθε αίτηση HTTP που κάνετε στην υπηρεσία. Αυτό το διακριτικό είναι παρόμοια με άλλα διακριτικά που παρέχεται από ACS βάσει αξιώσεων πρόσβασης που παρέχεται στην κεφαλίδα του μια αίτηση HTTP και χρησιμοποιεί το πρωτόκολλο v2 OAuth. Δεν χρειάζεται τυχόν άλλες προϋποθέσεις πριν συνδεθείτε απευθείας στο Media Services.

Το παρακάτω παράδειγμα εμφανίζει την κεφαλίδα αίτηση HTTP και σώμα που χρησιμοποιείται για την ανάκτηση ενός διακριτικού.

**Κεφαλίδα**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Οργανισμός**:

Πρέπει να αποδείξετε τις τιμές client_id και client_secret στο σώμα της πρόσκλησης σε αυτό; client_id και client_secret αντιστοιχούν τις τιμές όνομα λογαριασμού και AccountKey, αντίστοιχα. Αυτές οι τιμές παρέχονται σε εσάς από τις υπηρεσίες πολυμέσων κατά τη ρύθμιση του λογαριασμού σας. 

Σημειώστε ότι η AccountKey για το λογαριασμό σας Media Services πρέπει να κωδικοποίηση URL (ανατρέξτε στο θέμα [Κωδικοποίηση](http://tools.ietf.org/html/rfc3986#section-2.1) κατά τη χρήση του ως η τιμή client_secret στην πρόσκληση σε διακριτικό πρόσβασης.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Για παράδειγμα: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Το παρακάτω παράδειγμα εμφανίζει την απόκριση HTTP που περιέχει την πρόσβαση διακριτικού στο σώμα απόκρισης.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
Συνιστάται να τις τιμές "access_token" και "expires_in" σε ένα εξωτερικό χώρο αποθήκευσης στο cache. Τα δεδομένα του διακριτικού θα μπορούσε να αργότερα ανακτηθούν από την αποθήκευση και εκ νέου χρησιμοποιείται σε κλήσεις σας Media Services REST API. Αυτό είναι ιδιαίτερα χρήσιμο για σενάρια όπου το διακριτικό μπορεί να είναι κοινόχρηστη με ασφάλεια μεταξύ πολλών διαδικασιών ή υπολογιστές.

Φροντίστε να παρακολουθείτε τις τιμές "expires_in" το διακριτικό πρόσβασης και να ενημερώσετε τις κλήσεις σας REST API με νέα διακριτικά όπως απαιτείται.

###<a name="connecting-to-the-media-services-uri"></a>Σύνδεση με τις υπηρεσίες πολυμέσων URI

Ο ριζικός κατάλογος URI για τις υπηρεσίες πολυμέσων είναι https://media.windows.net/. Αρχικά, θα πρέπει να συνδέεστε σε αυτό URI και, εάν λάβετε ένα 301 redirect πίσω στο απόκρισης, θα πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI. Επιπλέον, μην χρησιμοποιείτε οποιοδήποτε λογικής αυτόματης-redirect/παρακολούθηση στις προσκλήσεις σε. Ρήματα HTTP και οι οργανισμοί αίτηση δεν θα προωθούνται για το νέο URI.

Σημειώστε ότι το ριζικό κατάλογο URI για αποστολή και λήψη αρχείων περιουσιακού στοιχείου είναι https://yourstorageaccount.blob.core.windows.net/ όπου το όνομα του λογαριασμού χώρου αποθήκευσης είναι το ίδιο που χρησιμοποιήσατε κατά τη διάρκεια της ρύθμισης του λογαριασμού σας Media Services.

Το παρακάτω παράδειγμα παρουσιάζει μια αίτηση HTTP στη ρίζα Media Services URI (https://media.windows.net/). Η πρόσκληση σε λαμβάνει μια 301 redirect πίσω στην απάντηση. Οι επόμενες αίτηση χρησιμοποιεί το νέο URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**Αίτηση HTTP**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**Απόκριση HTTP**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**Αίτηση HTTP** (χρησιμοποιώντας το νέο URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**Απόκριση HTTP**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Αφού δημιουργήσετε το νέο URI, που είναι το URI που πρέπει να χρησιμοποιούνται για την επικοινωνία με τις υπηρεσίες πολυμέσων. 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

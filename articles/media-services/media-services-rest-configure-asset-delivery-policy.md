<properties 
    pageTitle="Ρύθμιση παραμέτρων πολιτικών παράδοσης περιουσιακών στοιχείων με χρήση Media Services REST API | Microsoft Azure" 
    description="Αυτό το θέμα δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους διαφορετικές περιουσιακών στοιχείων παράδοσης πολιτικές χρησιμοποιώντας υπηρεσίες πολυμέσων REST API." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Ρύθμιση παραμέτρων πολιτικών παράδοσης περιουσιακών στοιχείων

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Εάν σκοπεύετε να κάνουν δυναμικά κρυπτογραφημένο περιουσιακών στοιχείων, ένα από τα βήματα στη ροή εργασίας παράδοσης περιεχομένου Media Services ρύθμιση των παραμέτρων πολιτικών παράδοσης για πόρους. Η πολιτική παράδοσης περιουσιακών στοιχείων σας ενημερώνει για το Media Services τον τρόπο που θέλετε για το πάγιο να παραδοθεί: σε ποια ροής πρωτόκολλο θα πρέπει να σας περιουσιακών στοιχείων δυναμικά συσκευαστούν (για παράδειγμα, MPEG ΠΑΎΛΑ, HLS, ομαλή ροή ή όλες), εάν θέλετε να κρυπτογραφήσετε δυναμικά σας περιουσιακών στοιχείων ή όχι και τον τρόπο με τον (φακέλου ή κοινές κρυπτογράφησης).

Αυτό το θέμα περιγράφει γιατί και πώς μπορείτε να δημιουργήσετε και να ρυθμίσετε τις πολιτικές παράδοσης περιουσιακών στοιχείων.

>[AZURE.NOTE]Για να μπορέσετε να χρησιμοποιήσετε δυναμικές συσκευασία και δυναμική κρυπτογράφηση, πρέπει να βεβαιωθείτε ότι έχετε τουλάχιστον μία κλίμακα μονάδας (γνωστό και ως ροή μονάδα). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).
>
>Επίσης, σας περιουσιακών στοιχείων πρέπει να περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit.

Μπορείτε να εφαρμόσετε διαφορετικές πολιτικές στο ίδιο πάγιο. Για παράδειγμα, μπορείτε να εφαρμόσετε PlayReady κρυπτογράφησης σε ομαλή ροή και φάκελος AES κρυπτογράφηση να ΠΑΎΛΑΣ MPEG και HLS. Όλα τα πρωτόκολλα που δεν έχουν οριστεί σε μια πολιτική παράδοσης (για παράδειγμα, προσθέτετε μια μεμονωμένη πολιτική που καθορίζει μόνο HLS ως το πρωτόκολλο) θα αποκλειστούν από ροής. Η εξαίρεση σε αυτό είναι όταν έχετε καμία πολιτική παράδοσης περιουσιακών στοιχείων που ορίζονται από το καθόλου. Στη συνέχεια, όλα τα πρωτόκολλα θα επιτρέπεται στο Απαλοιφή.

Εάν θέλετε να κάνουν ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, πρέπει να ρυθμίσετε την πολιτική παράδοσης του παγίου. Πριν σας περιουσιακού στοιχείου είναι δυνατό να διοχετεύονται, ο διακομιστής ροής καταργεί την κρυπτογράφηση χώρου αποθήκευσης και ροές το περιεχόμενό σας χρησιμοποιώντας την πολιτική καθορισμένο παράδοσης. Για παράδειγμα, για να προβάλετε σας περιουσιακών στοιχείων κρυπτογραφημένα με κλειδιού κρυπτογράφησης φακέλου για προχωρημένους πρότυπο κρυπτογράφησης (AES), ορίστε τον τύπο πολιτικής σε **DynamicEnvelopeEncryption**. Για να καταργήσετε την κρυπτογράφηση χώρου αποθήκευσης και ροή περιουσιακού στοιχείου σε Απαλοιφή, ορίστε τον τύπο πολιτικής σε **NoDynamicEncryption**. Ακολουθούν παραδείγματα που δείχνουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους των τύπων πολιτικής.

Ανάλογα με το πώς μπορείτε να ρυθμίσετε την πολιτική παράδοσης περιουσιακών στοιχείων που θα έχει τη δυνατότητα να δυναμικά συσκευασία, δυναμικά κρυπτογράφηση και ροή τα ακόλουθα πρωτόκολλα ροής: ομαλή ροή, HLS, ΠΑΎΛΑΣ MPEG και σκληροί ΔΊΣΚΟΙ ροών.

Η παρακάτω λίστα παρουσιάζει τις μορφές που μπορείτε να χρησιμοποιήσετε για τη ροή ομαλά, HLS, ΠΑΎΛΑ και σκληροί ΔΊΣΚΟΙ.

Ομαλή ροή:

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG ΠΑΎΛΑ

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

ΣΚΛΗΡΟΊ ΔΊΣΚΟΙ

{ροής τελικού σημείου υπηρεσίες πολυμέσων όνομα λογαριασμού name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Για οδηγίες σχετικά με τη δημοσίευση ενός περιουσιακού στοιχείου και να δημιουργήσετε μια ροή διεύθυνση URL, ανατρέξτε στο θέμα [Δημιουργία μιας ροής διεύθυνσης URL](media-services-deliver-streaming-content.md).


##<a name="considerations"></a>Ζητήματα

- Δεν μπορείτε να διαγράψετε ένα AssetDeliveryPolicy που σχετίζεται με ενός περιουσιακού στοιχείου, ενώ υπάρχει ένα προσδιοριστικό OnDemand (ροής) για το συγκεκριμένο περιουσιακών στοιχείων. Η σύσταση είναι να καταργήσετε την πολιτική από παγίου πριν από τη διαγραφή της πολιτικής.
- Μια ροή προσδιοριστικό δεν μπορεί να δημιουργηθεί σε ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, όταν δεν υπάρχει πολιτική παράδοσης περιουσιακών στοιχείων έχει οριστεί.  Εάν παγίου δεν είναι κρυπτογραφημένα χώρου αποθήκευσης, το σύστημα θα σάς επιτρέπουν να δημιουργήσετε ένα προσδιοριστικό και ροή περιουσιακού στοιχείου σε Απαλοιφή χωρίς μια πολιτική παράδοσης περιουσιακών στοιχείων.
- Μπορείτε να έχετε πολλούς περιουσιακών στοιχείων παράδοσης πολιτικών που σχετίζονται με ένα μεμονωμένο περιουσιακών στοιχείων, αλλά μπορείτε να καθορίσετε μόνο ένας τρόπος χειρισμού μιας δεδομένης AssetDeliveryProtocol.  Δηλαδή, εάν προσπαθείτε να συνδέσετε δύο πολιτικές παράδοσης που καθορίζουν το πρωτόκολλο AssetDeliveryProtocol.SmoothStreaming που θα έχει ως αποτέλεσμα σφάλμα επειδή το σύστημα δεν γνωρίζετε ποιο θέλετε να εφαρμόσετε όταν ένας υπολογιστής-πελάτης δημιουργεί μια αίτηση ομαλή ροή.
- Εάν έχετε ενός περιουσιακού στοιχείου με μια υπάρχουσα ροή προσδιοριστικό, δεν μπορείτε να συνδέσετε μια νέα πολιτική στο πάγιο, να αποσύνδεση μιας υπάρχουσας πολιτικής από περιουσιακού στοιχείου ή να ενημερώσετε μια πολιτική παράδοσης που σχετίζεται με το περιουσιακών στοιχείων.  Πρέπει πρώτα να καταργήσετε τη ροή locator, να προσαρμόσετε τις πολιτικές και, στη συνέχεια, να δημιουργήσετε ξανά το προσδιοριστικό ροής.  Μπορείτε να χρησιμοποιήσετε την ίδια locatorId όταν αναδημιουργήσετε το προσδιοριστικό ροής, αλλά θα πρέπει να εξασφαλίσετε που δεν θα προκαλέσει ζητήματα για προγράμματα-πελάτες, επειδή το περιεχόμενο έχει δυνατότητα cache από την προέλευση ή ένα CDN ροής.

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md).


##<a name="clear-asset-delivery-policy"></a>Απαλοιφή περιουσιακών στοιχείων παράδοσης πολιτικής

###<a id="create_asset_delivery_policy"></a>Δημιουργία πολιτικής παράδοσης περιουσιακών στοιχείων
Η παρακάτω αίτηση HTTP δημιουργεί μια πολιτική παράδοσης περιουσιακών στοιχείων που καθορίζει για να εφαρμόσετε δεν δυναμική κρυπτογράφηση και για παράδοση της ροής σε οποιοδήποτε από τα ακόλουθα πρωτόκολλα: πρωτόκολλα MPEG ΠΑΎΛΑ, HLS και ομαλή ροή. 

Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .   


Αίτηση:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Σύνδεση περιουσιακών στοιχείων με την πολιτική παράδοσης περιουσιακών στοιχείων

Η παρακάτω αίτηση HTTP συνδέσεων το καθορισμένο περιουσιακών στοιχείων στην πολιτική παράδοσης περιουσιακού στοιχείου για να.

Αίτηση:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Απόκριση:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>Πολιτική παράδοσης DynamicEnvelopeEncryption περιουσιακών στοιχείων 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Δημιουργία περιεχομένου αριθμό-κλειδί του τύπου EnvelopeEncryption και να το συνδέσετε με το περιουσιακών στοιχείων

Κατά τον καθορισμό DynamicEnvelopeEncryption παράδοσης πολιτική, πρέπει να βεβαιωθείτε ότι για να συνδέσετε το περιουσιακών στοιχείων με έναν αριθμό-κλειδί περιεχομένου του τύπου EnvelopeEncryption. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Δημιουργία έναν αριθμό-κλειδί περιεχομένου](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Λήψη διεύθυνσης URL παράδοσης

Λήψη της διεύθυνσης URL παράδοσης για τη μέθοδο παράδοσης καθορισμένη από το πλήκτρο περιεχομένου που δημιουργήσατε στο προηγούμενο βήμα. Ένα πρόγραμμα-πελάτη χρησιμοποιεί το επιστρεφόμενο διεύθυνσης URL για να ζητήσετε από ένα κλειδί AES ή μια PlayReady αδειών χρήσης για την αναπαραγωγή του προστατευμένου περιεχομένου.

Καθορίστε τον τύπο της διεύθυνσης URL για να μεταβείτε στο σώμα της αίτησης HTTP. Εάν προστατεύετε το περιεχόμενό σας με PlayReady, αίτηση διεύθυνση URL απόκτηση άδειας χρήσης PlayReady υπηρεσίες πολυμέσων, με χρήση 1 για την keyDeliveryType: {"keyDeliveryType": 1}. Εάν προστατεύετε το περιεχόμενό σας με την κρυπτογράφηση φακέλου, αίτηση για μια διεύθυνση URL κλειδιού acquisition, καθορίζοντας 2 για keyDeliveryType: {"keyDeliveryType": 2}.

Αίτηση:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Απόκριση:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Δημιουργία πολιτικής παράδοσης περιουσιακών στοιχείων

Η παρακάτω αίτηση HTTP δημιουργεί το **AssetDeliveryPolicy** που έχει ρυθμιστεί για να εφαρμόσετε το πρωτόκολλο **HLS** δυναμικής φακέλου κρυπτογράφησης (**DynamicEnvelopeEncryption**) (σε αυτό το παράδειγμα, άλλα πρωτόκολλα θα αποκλειστούν από ροής). 


Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .   

Αίτηση:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Σύνδεση περιουσιακών στοιχείων με την πολιτική παράδοσης περιουσιακών στοιχείων

Ανατρέξτε στο θέμα [σύνδεση περιουσιακών στοιχείων με την πολιτική παράδοσης περιουσιακών στοιχείων](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>Πολιτική παράδοσης DynamicCommonEncryption περιουσιακών στοιχείων 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Δημιουργία περιεχομένου αριθμό-κλειδί του τύπου CommonEncryption και να το συνδέσετε με το περιουσιακών στοιχείων

Κατά τον καθορισμό DynamicCommonEncryption παράδοσης πολιτική, πρέπει να βεβαιωθείτε ότι για να συνδέσετε το περιουσιακών στοιχείων με έναν αριθμό-κλειδί περιεχομένου του τύπου CommonEncryption. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα: [Δημιουργία έναν αριθμό-κλειδί περιεχομένου](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Λήψη διεύθυνσης URL παράδοσης

Λήψη της διεύθυνσης URL παράδοσης για τη μέθοδο παράδοσης PlayReady του περιεχομένου αριθμού-κλειδιού δημιουργήσατε στο προηγούμενο βήμα. Ένα πρόγραμμα-πελάτη χρησιμοποιεί το επιστρεφόμενο διεύθυνσης URL για να ζητήσετε PlayReady άδειας χρήσης για την αναπαραγωγή του προστατευμένου περιεχομένου. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Λήψη διεύθυνσης URL παράδοσης](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Δημιουργία πολιτικής παράδοσης περιουσιακών στοιχείων

Η παρακάτω αίτηση HTTP δημιουργεί το **AssetDeliveryPolicy** που έχει ρυθμιστεί για να εφαρμόσετε δυναμική κοινές κρυπτογράφηση (**DynamicCommonEncryption**) για να το πρωτόκολλο **Ομαλή ροή** (σε αυτό το παράδειγμα, άλλα πρωτόκολλα θα αποκλειστούν από ροής). 

Για πληροφορίες σχετικά με ποιες τιμές μπορείτε να καθορίσετε κατά τη δημιουργία ενός AssetDeliveryPolicy, ανατρέξτε στην ενότητα [τύποι χρησιμοποιείται κατά τον καθορισμό AssetDeliveryPolicy](#types) .   


Αίτηση:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Εάν θέλετε να προστατεύσετε το περιεχόμενό σας με χρήση Widevine DRM, ενημερώστε τις τιμές AssetDeliveryConfiguration για να χρησιμοποιήσετε WidevineLicenseAcquisitionUrl (το οποίο περιλαμβάνει την τιμή του 7) και καθορίστε τη διεύθυνση URL της υπηρεσίας παράδοσης άδεια χρήσης. Μπορείτε να χρησιμοποιήσετε την παρακάτω συνεργάτες αθροιστικής μέτρησης ενισχύσεων θα σας βοηθήσουν να κάνουν Widevine άδειες χρήσης: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Για παράδειγμα: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Κατά την κρυπτογράφηση με Widevine, μόνο θα μπορούν να κάνουν χρήση ΠΑΎΛΑΣ. Βεβαιωθείτε ότι για να καθορίσετε ΠΑΎΛΑ (2) στο πρωτόκολλο παράδοσης περιουσιακών στοιχείων.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Σύνδεση περιουσιακών στοιχείων με την πολιτική παράδοσης περιουσιακών στοιχείων

Ανατρέξτε στο θέμα [σύνδεση περιουσιακών στοιχείων με την πολιτική παράδοσης περιουσιακών στοιχείων](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Τύποι που χρησιμοποιούνται κατά τον καθορισμό AssetDeliveryPolicy

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
        }

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>

    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

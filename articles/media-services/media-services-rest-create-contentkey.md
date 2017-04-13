<properties 
    pageTitle="Δημιουργία ContentKeys με ΥΠΌΛΟΙΠΟ | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε περιεχομένου κλειδιά που παρέχουν ασφαλή πρόσβαση σε πόρους." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>


#<a name="create-contentkeys-with-rest"></a>Δημιουργία ContentKeys με ΥΠΌΛΟΙΠΟ


> [AZURE.SELECTOR]
- [ΥΠΌΛΟΙΠΟ](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)


Media Services σάς επιτρέπει να δημιουργήσετε νέα και παράδοση κρυπτογραφημένο περιουσιακών στοιχείων. Μια **ContentKey** παρέχει ασφαλή πρόσβαση σε σας s **περιουσιακών στοιχείων**. 

Όταν δημιουργείτε ένα νέο πάγιο (για παράδειγμα, πριν από την [Αποστολή αρχείων](media-services-rest-upload-files.md)), μπορείτε να καθορίσετε τις ακόλουθες επιλογές κρυπτογράφησης: **StorageEncrypted**, **CommonEncryptionProtected**ή **EnvelopeEncryptionProtected**. 

Όταν παραδώσετε περιουσιακών στοιχείων σε υπολογιστές-πελάτες σας, μπορείτε να [ρυθμίσετε τις παραμέτρους για στοιχεία σε δυναμικό είναι κρυπτογραφημένα](media-services-rest-configure-asset-delivery-policy.md) με ένα από τα παρακάτω δύο κρυπτογραφήσεις: **DynamicEnvelopeEncryption** ή **DynamicCommonEncryption**.

Κρυπτογραφημένο περιουσιακά στοιχεία πρέπει να συσχετίζονται με **ContentKey**s. Σε αυτό το άρθρο περιγράφει τον τρόπο για να δημιουργήσετε έναν αριθμό-κλειδί περιεχομένου.

Οι παρακάτω είναι γενικά βήματα για τη δημιουργία περιεχομένου τα πλήκτρα που θα συσχετίσετε με στοιχεία που θέλετε να είναι κρυπτογραφημένο. 

1. Τυχαία δημιουργούν έναν αριθμό-κλειδί AES 16 byte (για το κοινό και φάκελος κρυπτογράφηση) ή έναν αριθμό-κλειδί AES 32 byte (για το χώρο αποθήκευσης κρυπτογράφηση). 

    Αυτό θα είναι το κλειδί περιεχομένου για το πάγιο, γεγονός που σημαίνει ότι όλα τα αρχεία που σχετίζονται με αυτό περιουσιακών στοιχείων θα πρέπει να χρησιμοποιήσετε το ίδιο κλειδί περιεχομένου κατά τη διάρκεια της αποκρυπτογράφησης. 
2.  Καλέστε τις μεθόδους [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) και [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) για να λάβετε το σωστό πιστοποιητικό X.509 που πρέπει να χρησιμοποιηθούν για την κρυπτογράφηση κλειδιού περιεχομένου.
3.  Κρυπτογράφηση κλειδιού περιεχομένου με το δημόσιο κλειδί από το πιστοποιητικό X.509. 

    Υπηρεσίες πολυμέσων .NET SDK χρησιμοποιεί RSA με OAEP όταν κάνουν την κρυπτογράφηση.  Μπορείτε να δείτε ένα παράδειγμα στη [συνάρτηση EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Δημιουργήστε μια τιμή αθροίσματος ελέγχου (βάσει αλγόριθμο κλειδιού άθροισμα ελέγχου PlayReady AES) υπολογίζεται με χρήση του αναγνωριστικού του κλειδιού και του περιεχομένου κλειδιού. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα "PlayReady AES κλειδί άθροισμα ελέγχου αλγόριθμο" του εγγράφου PlayReady κεφαλίδα αντικείμενο βρίσκεται [εδώ](http://www.microsoft.com/playready/documents/).

    Το παρακάτω παράδειγμα .NET υπολογίζει το άθροισμα ελέγχου με χρήση του τμήματος GUID το αναγνωριστικό του κλειδιού και τον αριθμό-κλειδί Απαλοιφή περιεχομένων.
    
        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            byte[] array = null;
            using (AesCryptoServiceProvider aesCryptoServiceProvider = new AesCryptoServiceProvider())
            {
                aesCryptoServiceProvider.Mode = CipherMode.ECB;
                aesCryptoServiceProvider.Key = contentKey;
                aesCryptoServiceProvider.Padding = PaddingMode.None;
                ICryptoTransform cryptoTransform = aesCryptoServiceProvider.CreateEncryptor();
                array = new byte[16];
                cryptoTransform.TransformBlock(keyId.ToByteArray(), 0, 16, array, 0);
            }
            byte[] array2 = new byte[8];
            Array.Copy(array, array2, 8);
            return Convert.ToBase64String(array2);
        }

5. Δημιουργία του κλειδιού περιεχομένου με το **EncryptedContentKey** (μετατρέπονται σε base64 κωδικοποιημένη συμβολοσειρά), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**και τιμές **αθροίσματος** που έχετε λάβει στα προηγούμενα βήματα.
6. Συσχετισμός η οντότητα **ContentKey** με σας οντότητα **περιουσιακών στοιχείων** μέσω της λειτουργίας $links.

Σημειώστε ότι τα παραδείγματα που δημιουργούν ένα κλειδί AES, κρυπτογράφηση τον αριθμό-κλειδί και τον υπολογισμό του αθροίσματος ελέγχου έχει παραλειφθεί από αυτό το θέμα. Παρέχονται μόνο τα παραδείγματα που δείχνουν πώς μπορείτε να αλληλεπιδράσετε με τις υπηρεσίες πολυμέσων.


>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 

##<a name="retrieve-the-protectionkeyid"></a>Ανάκτηση του ProtectionKeyId 
 

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο ανάκτησης του ProtectionKeyId, μια αποτύπωση πιστοποιητικού, για το πιστοποιητικό πρέπει να χρησιμοποιήσετε κατά την κρυπτογράφηση κλειδιού περιεχομένου. Κάντε αυτό το βήμα για να βεβαιωθείτε ότι έχετε ήδη το κατάλληλο πιστοποιητικό στον υπολογιστή σας.


Αίτηση:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Απόκριση:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

##<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Ανάκτηση του ProtectionKey για το ProtectionKeyId

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ανακτήσετε το πιστοποιητικό X.509 χρησιμοποιώντας το ProtectionKeyId που λάβατε στο προηγούμενο βήμα.

Αίτηση:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Απόκριση:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

##<a name="create-the-contentkey"></a>Δημιουργία του ContentKey 

Αφού ανακτηθούν το πιστοποιητικό X.509 και χρησιμοποιείται το δημόσιο κλειδί για την κρυπτογράφηση κλειδιού περιεχομένου, δημιουργήστε μια οντότητα **ContentKey** και ορίστε τιμές των ιδιοτήτων αντίστοιχα.

Μία από τις τιμές που πρέπει να ορίσετε πότε να δημιουργήσετε το περιεχόμενο αριθμού-κλειδιού είναι ο τύπος. Επιλέξτε μία από τις παρακάτω τιμές.

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }


Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια **ContentKey** με ένα σύνολο **ContentKeyType** κρυπτογράφησης αποθήκευσης ("1") και το **ProtectionKeyType** οριστεί σε "0" για να υποδείξει ότι το κλειδί προστασίας Id είναι την αποτύπωση πιστοποιητικό X.509.  


Αίτηση

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

##<a name="associate-the-contentkey-with-an-asset"></a>Συσχέτιση του ContentKey με ενός περιουσιακού στοιχείου

Αφού δημιουργήσετε το ContentKey, συσχετίσετε με το περιουσιακών στοιχείων με χρήση της λειτουργίας $links, όπως φαίνεται στο ακόλουθο παράδειγμα:
    
Αίτηση:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Απόκριση:

    HTTP/1.1 204 No Content 


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

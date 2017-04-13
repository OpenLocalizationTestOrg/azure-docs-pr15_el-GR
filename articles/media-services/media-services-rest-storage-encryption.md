<properties 
    pageTitle="Η κρυπτογράφηση του περιεχομένου με κρυπτογράφηση χώρου αποθήκευσης με χρήση αθροιστικής μέτρησης ενισχύσεων REST API" 
    description="Μάθετε πώς μπορείτε να κρυπτογραφήσετε το περιεχόμενό σας με χρήση APIs ΥΠΌΛΟΙΠΑ αθροιστικής μέτρησης ενισχύσεων κρυπτογράφηση χώρου αποθήκευσης." 
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


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Η κρυπτογράφηση του περιεχομένου με κρυπτογράφηση χώρου αποθήκευσης με χρήση αθροιστικής μέτρησης ενισχύσεων REST API

Συνιστάται ιδιαίτερα για να κρυπτογραφήσετε το περιεχόμενό σας τοπικά χρησιμοποιεί κρυπτογράφηση AES-256 bit και, στη συνέχεια, στείλτε το στο χώρο αποθήκευσης Azure όπου θα αποθηκευτούν κρυπτογραφημένα στο υπόλοιπο.

Σε αυτό το άρθρο παρέχει μια επισκόπηση της αθροιστικής μέτρησης ενισχύσεων κρυπτογράφησης χώρου αποθήκευσης και σας δείχνει πώς μπορείτε να αποστείλετε το χώρο αποθήκευσης κρυπτογραφημένα περιεχόμενο:

- Δημιουργήστε έναν αριθμό-κλειδί περιεχομένου.
- Δημιουργία ενός περιουσιακού στοιχείου. Ορίστε το AssetCreationOption σε StorageEncryption κατά τη δημιουργία περιουσιακού στοιχείου.

     Κρυπτογραφημένο περιουσιακά στοιχεία πρέπει να συσχετίζονται με πλήκτρα περιεχομένου.
- Σύνδεση του περιεχομένου κλειδιού περιουσιακού στοιχείου.  
- Ορίσετε για την κρυπτογράφηση που σχετίζονται με τις παραμέτρους σε των οντοτήτων AssetFile.
 
>[AZURE.NOTE]Εάν θέλετε να κάνουν ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, πρέπει να ρυθμίσετε την πολιτική παράδοσης του παγίου. Πριν σας περιουσιακού στοιχείου είναι δυνατό να διοχετεύονται, ο διακομιστής ροής καταργεί την κρυπτογράφηση χώρου αποθήκευσης και ροές το περιεχόμενό σας χρησιμοποιώντας την πολιτική καθορισμένο παράδοσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων πολιτικών παράδοσης περιουσιακών στοιχείων](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 

##<a name="storage-encryption-overview"></a>Επισκόπηση της κρυπτογράφησης χώρου αποθήκευσης 

Η κρυπτογράφηση του χώρου αποθήκευσης αθροιστικής μέτρησης ενισχύσεων ισχύει κρυπτογράφηση **AES ΠΡΌΤ** λειτουργία για ολόκληρο το αρχείο.  Η λειτουργία ΠΡΌΤ AES είναι μια κρυπτογράφηση μπλοκ που μπορείτε να κρυπτογραφήσετε μήκος δεδομένων χωρίς να χρειάζεται αναπλήρωση. Αυτό λειτουργεί, κρυπτογραφώντας ένα μπλοκ μετρητή με ο αλγόριθμος AES και, στη συνέχεια, XOR-ση το αποτέλεσμα της AES με τα δεδομένα για την κρυπτογράφηση ή αποκρυπτογράφηση.  Ο αποκλεισμός μετρητή χρησιμοποιούνται έχει συνταχθεί από την αντιγραφή της τιμής από το InitializationVector byte 0 έως 7 της η τιμή του μετρητή και byte 8 έως 15 της η τιμή του μετρητή έχουν οριστεί στην τιμή μηδέν. Το μπλοκ μετρητής 16 byte, χρησιμοποιούνται byte 8 έως 15 (δηλαδή το λιγότερο σημαντικό byte) ως ακέραιος χωρίς πρόσημο απλό 64 bit που είναι αυξάνεται κατά μία για κάθε οι επόμενες μπλοκ δεδομένων υποβάλλονται σε επεξεργασία και διατηρούνται στη σειρά byte δικτύου. Λάβετε υπόψη ότι εάν αυτό ακέραιο φτάσει τη μέγιστη τιμή (0xFFFFFFFFFFFFFFFF) τα βηματικής, στη συνέχεια, επαναφέρει το μετρητή μπλοκ σε μηδέν (byte 8 έως 15) χωρίς να επηρεαστούν τα άλλα 64 bit του μετρητή (δηλαδή byte 0 έως 7).   Για να διατηρήσετε την ασφάλεια του την κρυπτογράφηση AES ΠΡΌΤ λειτουργία, η τιμή InitializationVector για ένα δεδομένο αριθμό-κλειδί αναγνωριστικό για κάθε κλειδί περιεχομένου είναι μοναδικός για κάθε αρχείο και αρχεία πρέπει να είναι μικρότερη από 2 ^ 64 μπλοκ μήκος.  Πρόκειται για να βεβαιωθείτε ότι μια τιμή μετρητή χρησιμοποιείται ποτέ ξανά με δεδομένο αριθμό-κλειδί. Για περισσότερες πληροφορίες σχετικά με τη λειτουργία ΠΡΌΤ, ανατρέξτε στο θέμα [αυτήν τη σελίδα wiki](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (στο άρθρο wiki, ο όρος "Nonce" αντί για "InitializationVector").

Χρήση της **Κρυπτογράφησης χώρου αποθήκευσης** για την κρυπτογράφηση των περιεχόμενό σας Απαλοιφή τοπικά χρησιμοποιεί κρυπτογράφηση AES-256 bit και, στη συνέχεια, στείλτε το στο χώρο αποθήκευσης Azure όπου είναι αποθηκευμένο κρυπτογραφημένα στο υπόλοιπο. Στοιχεία που προστατεύονται με κρυπτογράφηση χώρου αποθήκευσης είναι αυτόματα χωρίς κρυπτογράφηση τοποθετείται σε ένα σύστημα κρυπτογραφημένο αρχείο πριν από την κωδικοποίηση και προαιρετικά κρυπτογραφημένα εκ νέου πριν από την αποστολή ξανά ως νέο πάγιο εξόδου. Βασική χρήση συμβαίνει κρυπτογράφησης χώρου αποθήκευσης είναι όταν θέλετε να προστατεύσετε τα αρχεία εισαγωγής πολυμέσων υψηλής ποιότητας με ισχυρή κρυπτογράφηση στο υπόλοιπο στο δίσκο.

Για να προβάλετε ένα κρυπτογραφημένο περιουσιακών στοιχείων χώρου αποθήκευσης, πρέπει να ρυθμίσετε την πολιτική παράδοσης του παγίου ώστε να Media Services γνωρίζει πώς θέλετε να κάνετε το περιεχόμενό σας. Πριν από την περιουσιακού στοιχείου είναι δυνατό να διοχετεύονται, ο διακομιστής ροής καταργεί την κρυπτογράφηση χώρου αποθήκευσης και ροές το περιεχόμενό σας χρησιμοποιώντας την πολιτική καθορισμένο παράδοσης (για παράδειγμα, AES, κοινά κρυπτογράφηση ή χωρίς κρυπτογράφηση).

##<a name="create-contentkeys-used-for-encryption"></a>Δημιουργία ContentKeys που χρησιμοποιούνται για την κρυπτογράφηση

Κρυπτογραφημένο περιουσιακά στοιχεία πρέπει να συσχετίζονται με κλειδιού κρυπτογράφησης χώρου αποθήκευσης. Πρέπει να δημιουργήσετε το κλειδί περιεχομένου που θα χρησιμοποιηθεί για την κρυπτογράφηση πριν από τη δημιουργία περιουσιακού στοιχείου αρχείων. Αυτή η ενότητα περιγράφει πώς μπορείτε να δημιουργήσετε έναν αριθμό-κλειδί περιεχομένου.

Οι παρακάτω είναι γενικά βήματα για τη δημιουργία περιεχομένου τα πλήκτρα που θα συσχετίσετε με στοιχεία που θέλετε να είναι κρυπτογραφημένο. 

1. Για την κρυπτογράφηση χώρου αποθήκευσης, δημιουργία τυχαία κλειδιού AES 32 byte. 

    Αυτό θα είναι το κλειδί περιεχομένου για το πάγιο, γεγονός που σημαίνει ότι όλα τα αρχεία που σχετίζονται με αυτό περιουσιακών στοιχείων θα πρέπει να χρησιμοποιήσετε το ίδιο κλειδί περιεχομένου κατά τη διάρκεια της αποκρυπτογράφησης. 
2.  Καλέστε τις μεθόδους [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) και [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) για να λάβετε το σωστό πιστοποιητικό X.509 που πρέπει να χρησιμοποιηθούν για την κρυπτογράφηση κλειδιού περιεχομένου.
3.  Κρυπτογράφηση κλειδιού περιεχομένου με το δημόσιο κλειδί από το πιστοποιητικό X.509. 

    Υπηρεσίες πολυμέσων .NET SDK χρησιμοποιεί RSA με OAEP όταν κάνουν την κρυπτογράφηση.  Μπορείτε να δείτε ένα παράδειγμα .NET στη [συνάρτηση EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Δημιουργήστε μια τιμή αθροίσματος ελέγχου που υπολογίζεται με χρήση του αναγνωριστικού του κλειδιού και του περιεχομένου κλειδιού. Το παρακάτω παράδειγμα .NET υπολογίζει το άθροισμα ελέγχου με χρήση του τμήματος GUID το αναγνωριστικό του κλειδιού και τον αριθμό-κλειδί Απαλοιφή περιεχομένων.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Δημιουργία του κλειδιού περιεχομένου με το **EncryptedContentKey** (μετατρέπονται σε base64 κωδικοποιημένη συμβολοσειρά), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**και τιμές **αθροίσματος** που έχετε λάβει στα προηγούμενα βήματα.

    
    Για την κρυπτογράφηση χώρου αποθήκευσης, οι ακόλουθες ιδιότητες θα πρέπει να συμπεριλαμβάνεται στο σώμα της αίτησης.
     
    Αίτηση για την ιδιότητα σώμα   | Περιγραφή
    ---|---
    Αναγνωριστικό | Το αναγνωριστικό ContentKey τα οποία θα σας δημιουργούν αναφερόμαστε χρησιμοποιώντας την ακόλουθη μορφή, "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Πρόκειται για τον τύπο περιεχομένου κλειδιού μορφή ακέραιου αριθμού για αυτό το κλειδί περιεχομένου. Θα σας μεταβιβάζουν την τιμή 1 για την κρυπτογράφηση χώρου αποθήκευσης.
    EncryptedContentKey | Μπορούμε να δημιουργήσουμε μια νέα περιεχομένου τιμή του κλειδιού που είναι μια τιμή 256-bit (32 byte). Το κλειδί είναι κρυπτογραφημένη χρησιμοποιώντας το πιστοποιητικό X.509 κρυπτογράφησης χώρου αποθήκευσης, το οποίο γίνεται ανάκτηση από το Microsoft Azure Media Services εκτελώντας μια αίτηση HTTP GET για τις GetProtectionKeyId και τις μεθόδους GetProtectionKey. Ως παράδειγμα, δείτε τον παρακάτω κώδικα .NET: τη μέθοδο **EncryptSymmetricKeyData** που ορίζονται από [εδώ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Αυτό είναι το αναγνωριστικό κλειδιού προστασίας για το πιστοποιητικό X.509 κρυπτογράφησης χώρου αποθήκευσης που χρησιμοποιήθηκε για την κρυπτογράφηση μας περιεχομένου κλειδιού.
    ProtectionKeyType | Αυτός είναι ο τύπος κρυπτογράφησης για το κλειδί προστασίας που χρησιμοποιήθηκε για να κρυπτογραφήσετε το κλειδί περιεχομένου. Αυτή η τιμή είναι StorageEncryption(1) παράδειγμά μας.
    Άθροισμα ελέγχου |Το υπολογιζόμενο άθροισμα ελέγχου MD5 για το κλειδί περιεχομένου. Υπολογίζεται, κρυπτογραφώντας το περιεχόμενο αναγνωριστικό με τον αριθμό-κλειδί περιεχομένου. Το παράδειγμα κώδικα παρουσιάζει πώς μπορείτε να υπολογίσετε το άθροισμα ελέγχου.
    

###<a name="retrieve-the-protectionkeyid"></a>Ανάκτηση του ProtectionKeyId 
 

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

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>Ανάκτηση του ProtectionKey για το ProtectionKeyId

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

### <a name="create-the-content-key"></a>Δημιουργήστε το κλειδί περιεχομένου 

Αφού ανακτηθούν το πιστοποιητικό X.509 και χρησιμοποιείται το δημόσιο κλειδί για την κρυπτογράφηση κλειδιού περιεχομένου, δημιουργήστε μια οντότητα **ContentKey** και ορίστε τιμές των ιδιοτήτων αντίστοιχα.

Μία από τις τιμές που πρέπει να ορίσετε πότε να δημιουργήσετε το περιεχόμενο αριθμού-κλειδιού είναι ο τύπος. Σε περίπτωση την κρυπτογράφηση χώρου αποθήκευσης, η τιμή είναι "1". 

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

## <a name="create-an-asset"></a>Δημιουργία ενός περιουσιακού στοιχείου

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
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

##<a name="create-an-assetfile"></a>Δημιουργήστε μια AssetFile

Η οντότητα [AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) αντιπροσωπεύει ένα αρχείο βίντεο ή ήχου που είναι αποθηκευμένο σε ένα κοντέινερ αντικειμένων blob. Ένα αρχείο περιουσιακού στοιχείου είναι πάντα συσχετισμένη με ενός περιουσιακού στοιχείου και ενός περιουσιακού στοιχείου μπορεί να περιέχει ένα ή περισσότερα αρχεία περιουσιακών στοιχείων. Η εργασία Media Encoder υπηρεσίες αποτυγχάνει εάν ένα αντικείμενο αρχείου περιουσιακών στοιχείων δεν έχει συσχετιστεί με ένα ψηφιακό αρχείο σε ένα κοντέινερ αντικειμένων blob.

Σημειώστε ότι η παρουσία **AssetFile** και το αρχείο πραγματική πολυμέσων είναι δύο ξεχωριστά αντικείμενα. Η παρουσία AssetFile περιέχει μετα-δεδομένα σχετικά με το αρχείο πολυμέσων, ενώ το αρχείο περιέχει το περιεχόμενο πραγματική πολυμέσων.

Μετά την αποστολή του αρχείου σας ψηφιακών πολυμέσων σε ένα κοντέινερ αντικειμένων blob, θα μπορείτε να χρησιμοποιήσετε την αίτηση HTTP **ΣΥΓΧΏΝΕΥΣΗ** για να ενημερώσετε το AssetFile με πληροφορίες σχετικά με το αρχείο σας πολυμέσων (δεν εμφανίζονται σε αυτό το θέμα). 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }

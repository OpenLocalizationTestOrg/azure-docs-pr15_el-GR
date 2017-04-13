<properties 
    pageTitle="Ρύθμιση παραμέτρων περιεχομένου χρησιμοποιώντας υπηρεσίες πολυμέσων REST API πολιτική εξουσιοδότησης του αριθμού-κλειδιού | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να ρυθμίσετε μια πολιτική εξουσιοδότησης για ένα κλειδί περιεχομένου χρησιμοποιώντας υπηρεσίες πολυμέσων REST API." 
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


#<a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Δυναμική κρυπτογράφηση: ρύθμιση παραμέτρων περιεχομένου πολιτική εξουσιοδότησης αριθμού-κλειδιού
[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]


##<a name="overview"></a>Επισκόπηση

Υπηρεσίες πολυμέσων Azure Microsoft σάς επιτρέπει να κάνουν το περιεχόμενό σας (δυναμικά) κρυπτογραφημένα με Advanced πρότυπο κρυπτογράφησης (AES) (με χρήση κλειδιών κρυπτογράφησης 128 bit) και PlayReady ή Widevine DRM. Υπηρεσίες πολυμέσων παρέχει επίσης μια υπηρεσία για την παροχή πλήκτρα και PlayReady/Widevine αδειών χρήσης για τους χρήστες με δικαίωμα προγράμματα-πελάτες.

Εάν θέλετε για τις υπηρεσίες πολυμέσων για την κρυπτογράφηση ενός περιουσιακού στοιχείου, πρέπει να συσχετίσετε ενός κλειδιού κρυπτογράφησης (**CommonEncryption** ή **EnvelopeEncryption**) με το περιουσιακών στοιχείων (όπως περιγράφεται [εδώ](media-services-rest-create-contentkey.md)) και επίσης ρύθμιση παραμέτρων πολιτικών εξουσιοδότησης για τον αριθμό-κλειδί (όπως περιγράφεται σε αυτό το άρθρο).


Όταν μια ροή ζητείται από ένα πρόγραμμα αναπαραγωγής, Media Services χρησιμοποιεί τον καθορισμένο αριθμό-κλειδί για την κρυπτογράφηση δυναμικά το περιεχόμενό σας χρησιμοποιεί κρυπτογράφηση AES ή PlayReady. Για να αποκρυπτογραφήσετε τη ροή, το πρόγραμμα αναπαραγωγής θα ζητήσει τον αριθμό-κλειδί από την υπηρεσία παράδοσης κλειδιού. Για να αποφασίσετε αν ο χρήστης έχει δικαίωμα για να λάβετε τον αριθμό-κλειδί, την υπηρεσία υπολογίζει τις πολιτικές εξουσιοδότησης που έχετε καθορίσει για τον αριθμό-κλειδί.

Υπηρεσίες πολυμέσων υποστηρίζει πολλές τρόποι για τον έλεγχο ταυτότητας τους χρήστες που κάνουν αιτήσεις κλειδιού. Η πολιτική περιεχομένου εξουσιοδότησης κλειδιού θα μπορούσε να έχει ένα ή περισσότερα περιορισμοί άδειας: **Άνοιγμα** ή **διακριτικού** περιορισμού. Η πολιτική διακριτικού περιορισμένα πρέπει να συνοδεύεται από ένα διακριτικό εκδοθεί από μια ασφαλούς διακριτικού υπηρεσίας (STS). Υπηρεσίες πολυμέσων υποστηρίζει τα διακριτικά σε το **Απλό διακριτικά Web** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) και της μορφής **JSON Web διακριτικού **(JWT).

Υπηρεσίες πολυμέσων δεν παρέχει ασφαλούς διακριτικού υπηρεσίες. Μπορείτε να δημιουργήσετε μια προσαρμοσμένη STS ή να αξιοποιήσετε Microsoft Azure ACS για να τα διακριτικά το ζήτημα. Το STS πρέπει να ρυθμιστεί για τη δημιουργία ενός διακριτικού συνδεδεμένοι με το συγκεκριμένο πρόβλημα και αριθμού-κλειδιού αξιώσεων που καθορίσατε στο στη ρύθμιση παραμέτρων διακριτικού περιορισμού (όπως περιγράφεται σε αυτό το άρθρο). Η υπηρεσία παράδοσης κλειδιού Media Services θα επιστρέψει του κλειδιού κρυπτογράφησης στον υπολογιστή-πελάτη, εάν ισχύει το διακριτικό και το αξιώσεων στο διακριτικό ταιριάζουν με αυτές που έχουν ρυθμιστεί για το κλειδί περιεχομένου.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα

[JWT authenitcation διακριτικού](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Ενοποίηση Azure Media Services OWIN MVC βάσει εφαρμογή με το Azure Active Directory και να περιορίσετε την παράδοση περιεχομένου κλειδιού βάσει αξιώσεων JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Χρήση Azure ACS για να τα διακριτικά το ζήτημα](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Εφαρμογή ορισμένα ζητήματα:

- Για να μπορέσετε να χρησιμοποιήσετε δυναμικές συσκευασία και δυναμική κρυπτογράφηση, πρέπει να βεβαιωθείτε ότι έχουν τουλάχιστον μία ροή δεσμευμένη μονάδα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τρόπος για να κλιμακωθεί υπηρεσίας πολυμέσων](media-services-portal-manage-streaming-endpoints.md).
- Σας περιουσιακών στοιχείων πρέπει να περιέχει ένα σύνολο προσαρμόσιμων ρυθμό μετάδοσης bit MP4s ή αρχεία ομαλή ροή προσαρμόσιμη ρυθμό μετάδοσης bit. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κωδικοποίηση ενός περιουσιακού στοιχείου](media-services-encode-asset.md).
- Αποστολή και να κωδικοποιείτε τους πόρους σας χρησιμοποιώντας την επιλογή **AssetCreationOptions.StorageEncrypted** .
- Εάν σκοπεύετε να έχει πολλές πλήκτρα περιεχομένου που απαιτούν την ίδια ρύθμιση παραμέτρων της πολιτικής, συνιστάται να δημιουργήσετε μια πολιτική μία εξουσιοδότησης και εκ νέου χρήση με πολλά κλειδιά περιεχομένου.
- Η υπηρεσία παράδοσης κλειδί αποθηκεύει προσωρινά ContentKeyAuthorizationPolicy και τα σχετικά αντικείμενα (επιλογές της πολιτικής και τους περιορισμούς) για 15 λεπτά.  Εάν δημιουργείτε μια ContentKeyAuthorizationPolicy και καθορίστε τη χρήση ενός περιορισμού "Διακριτικού", στη συνέχεια, δοκιμάστε ξανά, και, στη συνέχεια, να ενημερώσετε την πολιτική με περιορισμό "Άνοιγμα", θα χρειαστεί περίπου 15 λεπτά πριν από την πολιτική μεταβαίνει στην έκδοση "Άνοιγμα" της πολιτικής.
- Εάν κάνετε προσθήκη ή ενημέρωση πολιτικής παράδοσης του περιουσιακού στοιχείου, πρέπει να διαγράψετε μια υπάρχουσα locator (εάν υπάρχουν) και να δημιουργήσετε ένα νέο προσδιοριστικό.
- Προς το παρόν, δεν μπορείτε να κρυπτογραφήσετε σκληροί ΔΊΣΚΟΙ μορφή ροής ή προοδευτική στοιχεία λήψης.


##<a name="aes-128-dynamic-encryption"></a>Δυναμική κρυπτογράφηση AES 128

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md).


###<a name="open-restriction"></a>Άνοιγμα περιορισμού

Άνοιγμα περιορισμού σημαίνει ότι το σύστημα θα κάνετε τον αριθμό-κλειδί σε οποιονδήποτε κάνει μια αίτηση κλειδιού. Αυτός ο περιορισμός μπορεί να είναι χρήσιμες για σκοπούς δοκιμής.

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική Άνοιγμα εξουσιοδότησης και το προσθέτει στο κλειδί περιεχομένου.

####<a id="ContentKeyAuthorizationPolicies"></a>Δημιουργία ContentKeyAuthorizationPolicies

Αίτηση:
        
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423578086&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lZlyQ2%2bvH73qtJsb42%2fH3xF7r7EvQFR3UXyezuDENFU%3d
    x-ms-version: 2.11
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 36
    
    {"Name":"Open Authorization Policy"}
    
Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 211
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Adb4593da-f4d1-4cc5-a92a-d20eacbabee4')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d732dbfa-54fc-474c-99d6-9b46a006f389
    request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    x-ms-request-id: aabfa731-e884-4bf3-8314-492b04747ac4
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:25:56 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:db4593da-f4d1-4cc5-a92a-d20eacbabee4","Name":"Open Authorization Policy"}

####<a id="ContentKeyAuthorizationPolicyOptions"></a>Δημιουργία ContentKeyAuthorizationPolicyOptions
    
Αίτηση:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580006&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Ref3EsonGF7fUKCwGwGgiMnZitzIzsDOvvMTeVrVVPg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 168
    
    {"Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

Απόκριση:   
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 349
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d225e357-e60e-4f42-add8-9d93aba1409a
    request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    x-ms-request-id: 81bcad37-295b-431f-972f-b23f2e4172c9
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 08:56:40 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:57829b17-1101-4797-919b-f816f4a007b7","Name":"policy","KeyDeliveryType":2,"KeyDeliveryConfiguration":"","Restrictions":[{"Name":"HLS Open Authorization Policy","KeyRestrictionType":0,"Requirements":null}]}

####<a id="LinkContentKeyAuthorizationPoliciesWithOptions"></a>Σύνδεση ContentKeyAuthorizationPolicies με επιλογές

Αίτηση:
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3A0baa438b-8ac2-4c40-a53c-4d4722b78715')/$links/Options HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580006&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Ref3EsonGF7fUKCwGwGgiMnZitzIzsDOvvMTeVrVVPg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 9847f705-f2ca-4e95-a478-8f823dbbaa29
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 154
    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A57829b17-1101-4797-919b-f816f4a007b7')"}

Απόκριση:

    HTTP/1.1 204 No Content

####<a id="AddAuthorizationPolicyToKey"></a>Προσθήκη πολιτικής εξουσιοδότησης στο κλειδί περιεχομένου

Αίτηση:

    PUT https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A2e6d36a7-a17c-4e9a-830d-eca23ad1a6f9') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: e613efff-cb6a-41b4-984a-f4f8fb6e76a4
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 78
    
    {"AuthorizationPolicyId":"nb:ckpid:UUID:c06cebb8-c4f0-4d1a-ba00-3273fb2bc3ad"}

Απόκριση:

    HTTP/1.1 204 No Content

###<a name="token-restriction"></a>Ο περιορισμός διακριτικού

Αυτή η ενότητα περιγράφει πώς μπορείτε να δημιουργήσετε μια πολιτική εξουσιοδότησης κλειδιού περιεχομένου και να συσχετίσετε με τον αριθμό-κλειδί περιεχομένου. Η πολιτική εξουσιοδότησης περιγράφει τι απαιτήσεις άδειας πρέπει να πληρούνται για να προσδιορίσετε εάν ο χρήστης έχει δικαίωμα να λάβετε το κλειδί (για παράδειγμα, κάνει τη λίστα "Επαλήθευση κλειδί" περιέχουν τον αριθμό-κλειδί που υπογράφηκε με το διακριτικό).

Για να ρυθμίσετε την επιλογή διακριτικού περιορισμού, πρέπει να χρησιμοποιήσετε ένα XML για να περιγράψετε το διακριτικό εξουσιοδότησης απαιτήσεις. Στη ρύθμιση παραμέτρων διακριτικού περιορισμού XML πρέπει να συμφωνεί με το παρακάτω σχήμα XML.

####<a id="schema"></a>Σχήμα διακριτικού περιορισμού
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Κατά τη ρύθμιση των παραμέτρων του **διακριτικού** περιορισμένων πολιτική, πρέπει να καθορίσετε τις πρωτεύον** κλειδί επαλήθευσης**, **εκδότη** και **ακροατηρίου** παραμέτρους. Το **επαλήθευσης πρωτεύον κλειδί **περιέχει τον αριθμό-κλειδί που υπογράφηκε με το διακριτικό, **εκδότη** είναι η υπηρεσία ασφαλούς διακριτικών που εκδίδει το διακριτικό. Το **ακροατήριο** (μερικές φορές ονομάζεται **εύρος**) περιγράφει πρόθεση το διακριτικό ή ο πόρος το διακριτικό παράσχει εξουσιοδότηση πρόσβασης. Η υπηρεσία παράδοσης κλειδιού Media Services επαληθεύει ότι αυτές οι τιμές στο διακριτικό ταιριάζουν με τις τιμές στο πρότυπο. 

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική εξουσιοδότησης με ενός διακριτικού περιορισμού. Σε αυτό το παράδειγμα, ο υπολογιστής-πελάτης θα πρέπει να παρουσιάσετε ένα διακριτικό που περιέχει: Είσοδος κλειδί (VerificationKey), μια εκδότη διακριτικού και απαιτείται αξιώσεων.
    
###<a name="create-contentkeyauthorizationpolicies"></a>Δημιουργία ContentKeyAuthorizationPolicies

Δημιουργία "Διακριτικού πολιτική περιορισμού", όπως φαίνεται [εδώ](#ContentKeyAuthorizationPolicies).


###<a name="create-contentkeyauthorizationpolicyoptions"></a>Δημιουργία ContentKeyAuthorizationPolicyOptions

Αίτηση:
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbbef702-e769-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423580720&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=5LsNu%2b0D4eD3UOP3BviTLDkUjaErdUx0ekJ8402xidQ%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1079
    
    {"Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

Απόκριση:   
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1260
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae1ef6145-46e8-4ee6-9756-b1cf96328c23')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 2643d836-bfe7-438e-9ba2-bc6ff28e4a53
    request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    x-ms-request-id: 2310b716-aeaa-421e-913e-3ce2f6f685ca
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:10:37 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e1ef6145-46e8-4ee6-9756-b1cf96328c23","Name":"Token option for HLS","KeyDeliveryType":2,"KeyDeliveryConfiguration":null,"Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>BklyAFiPTQsuJNKriQJBZHYaKM2CkCTDQX2bw9sMYuvEC9sjW0W7GUIBygQL/+POEeUqCYPnmEU2g0o1GW2Oqg==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>E5BUHiN4vBdzUzdP0IWaHFMMU3D1uRZgF16TOhSfwwHGSw+Kbf0XqsHzEIYk11M372viB9vbiacsdcQksA0ftw==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}
    
####<a name="link-contentkeyauthorizationpolicies-with-options"></a>Σύνδεση ContentKeyAuthorizationPolicies με επιλογές

Σύνδεση ContentKeyAuthorizationPolicies με επιλογές όπως φαίνεται [εδώ](#ContentKeyAuthorizationPolicies).

####<a name="add-authorization-policy-to-the-content-key"></a>Προσθήκη πολιτικής εξουσιοδότησης στο κλειδί περιεχομένου

Προσθέστε AuthorizationPolicy το ContentKey όπως φαίνεται [εδώ](#AddAuthorizationPolicyToKey).


##<a name="playready-dynamic-encryption"></a>Δυναμική PlayReady κρυπτογράφησης 

Υπηρεσίες πολυμέσων σάς επιτρέπει να ρυθμίσετε τα δικαιώματα και τους περιορισμούς που θέλετε για το περιβάλλον εκτέλεσης PlayReady DRM για να επιβάλετε όταν ένας χρήστης επιχειρεί να αναπαραγάγετε προστατευμένου περιεχομένου. 

Κατά την προστασία του περιεχομένου με PlayReady, ένα από τα πράγματα που πρέπει να καθορίσετε σε σας πολιτική εξουσιοδότησης είναι μια συμβολοσειρά XML που καθορίζει το [πρότυπο PlayReady άδεια χρήσης](https://msdn.microsoft.com/library/azure/dn783459.aspx). 

###<a name="open-restriction"></a>Άνοιγμα περιορισμού
    
Άνοιγμα περιορισμού σημαίνει ότι το σύστημα θα κάνετε τον αριθμό-κλειδί σε οποιονδήποτε κάνει μια αίτηση κλειδιού. Αυτός ο περιορισμός μπορεί να είναι χρήσιμες για σκοπούς δοκιμής.

Το παρακάτω παράδειγμα δημιουργεί μια πολιτική Άνοιγμα εξουσιοδότησης και το προσθέτει στο κλειδί περιεχομένου.
    
####<a id="ContentKeyAuthorizationPolicies2"></a>Δημιουργία ContentKeyAuthorizationPolicies

Αίτηση:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=bbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 58
    
    {"Name":"Deliver Common Content Key"}
    
Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 233
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicies('nb%3Ackpid%3AUUID%3Acc3c64a8-e2fc-4e09-bf60-ac954251a387')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 9e7fa407-f84e-43aa-8f05-9790b46e279b
    request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    x-ms-request-id: b3d33c1b-a9cb-4120-ac0c-18f64846c147
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:26:00 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicies/@Element","Id":"nb:ckpid:UUID:cc3c64a8-e2fc-4e09-bf60-ac954251a387","Name":"Deliver Common Content Key"}
    

#### <a name="create-contentkeyauthorizationpolicyoptions"></a>Δημιουργία ContentKeyAuthorizationPolicyOptions

Αίτηση:
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423581565&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=JiNSG3w6r2C0nIyfKvTZj1uPJGjuitD%2b0sbfZ%2b2JDZI%3d
    x-ms-version: 2.11
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 593
    
    {"Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}
    
Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 774
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3A1052308c-4df7-4fdb-8d21-4d2141fc2be0')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: f160ad25-b457-4bc6-8197-315604c5e585
    request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    x-ms-request-id: 563f5a42-50a4-4c4a-add8-a833f8364231
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:23:24 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:1052308c-4df7-4fdb-8d21-4d2141fc2be0","Name":"","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Open","KeyRestrictionType":0,"Requirements":null}]}

####<a name="link-contentkeyauthorizationpolicies-with-options"></a>Σύνδεση ContentKeyAuthorizationPolicies με επιλογές

Σύνδεση ContentKeyAuthorizationPolicies με επιλογές όπως φαίνεται [εδώ](#ContentKeyAuthorizationPolicies).

####<a name="add-authorization-policy-to-the-content-key"></a>Προσθήκη πολιτικής εξουσιοδότησης στο κλειδί περιεχομένου

Προσθέστε AuthorizationPolicy το ContentKey όπως φαίνεται [εδώ](#AddAuthorizationPolicyToKey).


###<a name="token-restriction"></a>Ο περιορισμός διακριτικού

Για να ρυθμίσετε την επιλογή διακριτικού περιορισμού, πρέπει να χρησιμοποιήσετε ένα XML για να περιγράψετε το διακριτικό εξουσιοδότησης απαιτήσεις. Στη ρύθμιση παραμέτρων διακριτικού περιορισμού XML πρέπει να συμφωνεί με το σχήμα XML που εμφανίζονται σε [αυτήν](#schema) την ενότητα.
    
####<a name="create-contentkeyauthorizationpolicies"></a>Δημιουργία ContentKeyAuthorizationPolicies
    
Δημιουργία ContentKeyAuthorizationPolicies όπως φαίνεται [εδώ](#ContentKeyAuthorizationPolicies2).

####<a name="create-contentkeyauthorizationpolicyoptions"></a>Δημιουργία ContentKeyAuthorizationPolicyOptions
    
Αίτηση:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 3.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423583561&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=5eZnkOsSv%2fLLEKmS%2bWObBlsNYyee8BQlp%2bUYbjugcJg%3d
    x-ms-version: 2.11
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 1525
    
    {"Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

Απόκριση:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1706
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeyAuthorizationPolicyOptions('nb%3Ackpoid%3AUUID%3Ae42bbeae-de42-4077-90e9-a844f297ef70')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: ab079b0e-2ba9-4cf1-b549-a97bfa6cd2d3
    request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    x-ms-request-id: ccf8a4ba-731e-4124-8192-079592c251cc
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Tue, 10 Feb 2015 09:58:47 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeyAuthorizationPolicyOptions/@Element","Id":"nb:ckpoid:UUID:e42bbeae-de42-4077-90e9-a844f297ef70","Name":"Token option","KeyDeliveryType":1,"KeyDeliveryConfiguration":"<PlayReadyLicenseResponseTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1\"><LicenseTemplates><PlayReadyLicenseTemplate><AllowTestDevices>false</AllowTestDevices><ContentKey i:type=\"ContentEncryptionKeyFromHeader\" /><LicenseType>Nonpersistent</LicenseType><PlayRight /></PlayReadyLicenseTemplate></LicenseTemplates></PlayReadyLicenseResponseTemplate>","Restrictions":[{"Name":"Token Authorization Policy","KeyRestrictionType":1,"Requirements":"<TokenRestrictionTemplate xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1\"><AlternateVerificationKeys><TokenVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>w52OyHVqXT8aaupGxuJ3NGt8M6opHDOtx132p4r6q4hLI6ffnLusgEGie1kedUewVoIe1tqDkVE6xsIV7O91KA==</KeyValue></TokenVerificationKey></AlternateVerificationKeys><Audience>urn:test</Audience><Issuer>http://testacs.com/</Issuer><PrimaryVerificationKey i:type=\"SymmetricVerificationKey\"><KeyValue>dYwLKIEMBljLeY9VM7vWdlhps31Fbt0XXhqP5VyjQa33bJXleBtkzQ6dF5AtwI9gDcdM2dV2TvYNhCilBKjMCg==</KeyValue></PrimaryVerificationKey><RequiredClaims><TokenClaim><ClaimType>urn:microsoft:azure:mediaservices:contentkeyidentifier</ClaimType><ClaimValue i:nil=\"true\" /></TokenClaim></RequiredClaims><TokenType>SWT</TokenType></TokenRestrictionTemplate>"}]}

####<a name="link-contentkeyauthorizationpolicies-with-options"></a>Σύνδεση ContentKeyAuthorizationPolicies με επιλογές

Σύνδεση ContentKeyAuthorizationPolicies με επιλογές όπως φαίνεται [εδώ](#ContentKeyAuthorizationPolicies).

####<a name="add-authorization-policy-to-the-content-key"></a>Προσθήκη πολιτικής εξουσιοδότησης στο κλειδί περιεχομένου

Προσθέστε AuthorizationPolicy το ContentKey όπως φαίνεται [εδώ](#AddAuthorizationPolicyToKey).


##<a id="types"></a>Τύποι που χρησιμοποιούνται κατά τον καθορισμό ContentKeyAuthorizationPolicy

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
        None = 0,
        PlayReadyLicense = 1,
        BaselineHttp = 2,
        Widevine = 3
    }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε ρυθμίσει τις παραμέτρους πολιτικής εξουσιοδότησης περιεχομένου κλειδί, μεταβείτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις παραμέτρους πολιτικής παράδοσης περιουσιακών στοιχείων](media-services-rest-configure-asset-delivery-policy.md) .

 

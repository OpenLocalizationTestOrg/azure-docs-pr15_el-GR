
<properties 
    pageTitle="Διαχείριση Media Services οντοτήτων με REST API | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να διαχειριστείτε τις υπηρεσίες πολυμέσων οντοτήτων με REST API." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>

#<a name="managing-media-services-entities-with-rest-api"></a>Διαχείριση Media Services οντοτήτων με REST API

> [AZURE.SELECTOR]
- [ΥΠΌΛΟΙΠΟ](media-services-rest-manage-entities.md)
- [.NET](media-services-dotnet-manage-entities.md)

Microsoft Azure Media Services είναι μια υπηρεσία που βασίζεται στο ΥΠΌΛΟΙΠΟ, δημιουργείται στο OData v3. Αυτόν το λόγο, μπορείτε να προσθέσετε, να ερωτήματος, ενημέρωση και διαγραφή οντοτήτων με τον ίδιο τρόπο όπως και σε οποιαδήποτε άλλη υπηρεσία OData. Εξαιρέσεις θα ονομάζεται κατά περίπτωση. Για περισσότερες πληροφορίες σχετικά με το OData, ανατρέξτε [στην τεκμηρίωση Open Data Protocol](http://www.odata.org/documentation/).

- Προσθήκη οντοτήτων 
- Υποβολή ερωτημάτων οντοτήτων 
- Απαρίθμησης μεγάλες συλλογές οντοτήτων
- Ενημέρωση οντοτήτων 
- Διαγραφή οντοτήτων 

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 


##<a name="adding-entities"></a>Προσθήκη οντοτήτων

Κάθε οντότητα στο Media Services προστίθεται σε ένα σύνολο οντότητα, όπως στοιχεία, μέσω μιας αίτησης HTTP ΔΗΜΟΣΊΕΥΣΗ.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια AccessPolicy.

    POST https://media.windows.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: media.windows.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

 
##<a name="querying-entities"></a>Υποβολή ερωτημάτων οντοτήτων

Υποβολή ερωτημάτων και καταχώρηση οντοτήτων είναι άμεση και περιλαμβάνει μόνο μια αίτηση HTTP ΓΡΉΓΟΡΑ και προαιρετικά λειτουργίες OData.
Το παρακάτω παράδειγμα ανακτά μια λίστα όλων των οντοτήτων MediaProcessor.

    GET https://media.windows.net/API/MediaProcessors HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

Μπορείτε επίσης να ανακτήσετε μια συγκεκριμένη οντότητα ή όλα τα σύνολα οντότητα που σχετίζεται με μια συγκεκριμένη οντότητα, όπως στα ακόλουθα παραδείγματα:

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336907474&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=OpuY0CeTylqFFcFaP4pKUVGesT4PGx4CP55zDf2zXnc%3d
    Host: media.windows.net

    GET https://media.windows.net/API/JobTemplates('nb:jtid:UUID:e81192f5-576f-b247-b781-70a790c20e7c')/TaskTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336907474&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=OpuY0CeTylqFFcFaP4pKUVGesT4PGx4CP55zDf2zXnc%3d
    Host: media.windows.net

Το παρακάτω παράδειγμα επιστρέφει μόνο την ιδιότητα State όλων των έργων.

    GET https://media.windows.net/API/Jobs?$select=State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

Το παρακάτω παράδειγμα επιστρέφει όλα JobTemplates με το όνομα "SampleTemplate."

    GET https://media.windows.net/API/JobTemplates?$filter=startswith(Name,%20'SampleTemplate') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

>[AZURE.NOTE]Το $, αναπτύξτε τη λειτουργία δεν υποστηρίζεται σε υπηρεσίες πολυμέσων, καθώς και τη μη υποστηριζόμενη LINQ μεθόδους που περιγράφονται στο LINQ ζητήματα (WCF Data Services).

##<a name="enumerating-through-large-collections-of-entities"></a>Απαρίθμησης μεγάλες συλλογές οντοτήτων

Κατά την υποβολή ερωτήματος οντοτήτων, υπάρχει όριο 1000 οντοτήτων επιστρέφονται ταυτόχρονα επειδή δημόσια v2 ΥΠΌΛΟΙΠΑ περιορίζει τα αποτελέσματα ερωτήματος 1000 αποτελέσματα. Χρησιμοποιήστε **παραλείψετε** και **επάνω** για να κάνετε απαρίθμηση στη μεγάλη συλλογή οντοτήτων. 

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε **παραλείψετε** και **επάνω** για να παραλείψετε τα πρώτα 2000 έργα και να λάβετε το επόμενο 1000 εργασίες.  

    GET https://media.windows.net/api/Jobs()?$skip=2000&$top=1000 HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337078831&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=suFkxhvPWxQVMjOYelOJfYEWkyTWJCBc02pF0N7NghI%3d
    Host: media.windows.net

##<a name="updating-entities"></a>Ενημέρωση οντοτήτων

Ανάλογα με τον τύπο οντότητας και στην κατάσταση που βρίσκεται στην, μπορείτε να ενημερώσετε ιδιότητες σε αυτήν την οντότητα μέσω μια ενημερωμένη έκδοση ΚΏΔΙΚΑ, ΤΟΠΟΘΈΤΗΣΗ ή ΣΥΓΧΏΝΕΥΣΗ HTTP προσκλήσεις. Για περισσότερες πληροφορίες σχετικά με αυτές τις λειτουργίες, ανατρέξτε στο θέμα [Ενημέρωση ΚΏΔΙΚΑ/ΤΟΠΟΘΈΤΗΣΗ ΣΥΓΧΏΝΕΥΣΗΣ](https://msdn.microsoft.com/library/dd541276.aspx).

Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να ενημερώσετε την ιδιότητα Name σε μια οντότητα περιουσιακών στοιχείων.

    MERGE https://media.windows.net/API/Assets('nb:cid:UUID:80782407-3f87-4e60-a43e-5e4454232f60') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337083279&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=DMLQXWah4jO0icpfwyws5k%2b1aCDfz9KDGIGao20xk6g%3d
    Host: media.windows.net
    Content-Length: 21
    Expect: 100-continue
    
    {"Name" : "NewName" }

##<a name="deleting-entities"></a>Διαγραφή οντοτήτων

Οντοτήτων μπορούν να διαγραφούν στις υπηρεσίες πολυμέσων, χρησιμοποιώντας μια αίτηση HTTP ΔΙΑΓΡΑΦΉ. Ανάλογα με την οντότητα, η σειρά με την οποία μπορείτε να διαγράψετε οντοτήτων μπορεί να είναι σημαντικές. Για παράδειγμα, οντοτήτων όπως στοιχεία απαιτούν ανάκληση (ή διαγραφή) όλα Locators που αναφέρονται σε συγκεκριμένο συγκεκριμένο περιουσιακών στοιχείων πριν από τη διαγραφή του περιουσιακού στοιχείου.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να διαγράψετε ένα προσδιοριστικό που χρησιμοποιήθηκε για να αποστείλετε ένα αρχείο στο χώρο αποθήκευσης αντικειμένων blob.

    DELETE https://media.windows.net/API/Locators('nb:lid:UUID:76dcc8e8-4230-463d-97b0-ce25c41b5c8d') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: media.windows.net
    Content-Length: 0



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

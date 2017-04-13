<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε ένα πρόγραμμα επεξεργασίας πολυμέσων | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε ένα στοιχείο επεξεργαστής πολυμέσων για να κωδικοποιήσετε, μετατροπή μορφής, κρυπτογράφηση ή αποκρυπτογράφηση περιεχομένου πολυμέσων για υπηρεσιών Azure Media Services." 
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


#<a name="how-to-get-a-media-processor-instance"></a>Πώς μπορείτε να: γρήγορα μια παρουσία επεξεργαστής πολυμέσων


> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-get-media-processor.md)

##<a name="overview"></a>Επισκόπηση

Στις υπηρεσίες πολυμέσων ένα πρόγραμμα επεξεργασίας πολυμέσων είναι ένα στοιχείο που χειρίζεται μια εργασία συγκεκριμένες επεξεργασίας, όπως η κωδικοποίηση, μορφοποιήστε μετατροπής, η κρυπτογράφηση ή αποκρυπτογράφηση περιεχομένου πολυμέσων. Συνήθως δημιουργείτε έναν επεξεργαστή πολυμέσων όταν δημιουργείτε μια εργασία για να κωδικοποιήσετε, κρυπτογράφηση ή να μετατρέψετε τη μορφή του περιεχομένου πολυμέσων.

Ο παρακάτω πίνακας παρέχει το όνομα και περιγραφή για κάθε επεξεργαστή διαθέσιμη πολυμέσων.

Όνομα επεξεργαστής πολυμέσων|Περιγραφή|Περισσότερες πληροφορίες
---|---|---
Τυπική κωδικοποιητή πολυμέσων|Παρέχει τυπικές δυνατότητες για κωδικοποίηση στη ζήτηση. |[Επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων](media-services-encode-asset.md)
Ροή εργασίας Premium κωδικοποιητή πολυμέσων|Σας επιτρέπει να εκτελείτε κωδικοποίησης εργασιών με ροή εργασίας Premium κωδικοποιητή πολυμέσων.|[Επισκόπηση και σύγκριση των Azure σε ζήτηση κωδικοποιητές πολυμέσων](media-services-encode-asset.md)
Ευρετήριο πολυμέσων Azure| Σας επιτρέπει να κάνετε τα αρχεία πολυμέσων και το περιεχόμενο με δυνατότητα αναζήτησης, καθώς και να δημιουργούν κλειστές λεζάντες κομμάτια και λέξεις-κλειδιά.|[Ευρετήριο πολυμέσων Azure](media-services-index-content.md)
Azure Media Hyperlapse (έκδοση preview)|Σας επιτρέπει να εξομαλύνει τη "χτυπήματα" στο βίντεό σας με το βίντεο σταθεροποίησης. Σας επιτρέπει επίσης να επιταχύνετε το περιεχόμενό σας σε ένα clip ΑΝΑΛΩΣΙΜΩΝ.|[Hyperlapse πολυμέσων Azure](media-services-hyperlapse-content.md)
Azure Media Encoder|Η απόσβεση
Αποκρυπτογράφηση χώρου αποθήκευσης| Η απόσβεση|
Συσκευασία πολυμέσων Azure|Η απόσβεση|
Μονάδα κρυπτογράφησης πολυμέσων Azure|Η απόσβεση|

##<a name="get-mediaprocessor"></a>Λήψη MediaProcessor

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 


Η ακόλουθη κλήση ΥΠΌΛΟΙΠΑ δείχνει πώς μπορείτε να λάβετε μια παρουσία επεξεργαστής πολυμέσων με βάση το όνομα (σε αυτήν την περίπτωση, **Media Encoder τυπική**). 



    
Αίτηση:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Απόκριση:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που γνωρίζετε πώς μπορείτε να λάβετε μια παρουσία επεξεργαστής πολυμέσων, μεταβείτε στο θέμα [πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου](media-services-rest-get-started.md) που θα σας δείξει πώς να χρησιμοποιήσετε την τυπική κωδικοποιητή πολυμέσων για να κωδικοποιήσετε ενός περιουσιακού στοιχείου.

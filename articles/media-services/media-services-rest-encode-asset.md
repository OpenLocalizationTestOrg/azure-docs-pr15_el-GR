<properties 
    pageTitle="Πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου με χρήση Media Encoder τυπική | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την τυπική κωδικοποιητή πολυμέσων για την κωδικοποίηση περιεχομένου πολυμέσων σε υπηρεσίες πολυμέσων. Δείγματα κώδικα Χρησιμοποιήστε REST API." 
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


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Πώς μπορείτε να κωδικοποιήσετε ενός περιουσιακού στοιχείου με χρήση Media Encoder τυπική


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [ΥΠΌΛΟΙΠΟ](media-services-rest-encode-asset.md)
- [Πύλη](media-services-portal-encode.md)

##<a name="overview"></a>Επισκόπηση
Προκειμένου να κάνουν ψηφιακού βίντεο μέσω του internet θα πρέπει να γίνεται η συμπίεση των πολυμέσων. Αρχεία ψηφιακού βίντεο είναι αρκετά μεγάλα και μπορεί να είναι πολύ μεγάλο για παράδοση μέσω του internet ή για συσκευές τους πελάτες σας για να εμφανίζονται σωστά. Κωδικοποίηση είναι η διαδικασία για τη συμπίεση βίντεο και ήχος, ώστε οι πελάτες σας μπορούν να προβάλουν τα πολυμέσα.

Οι εργασίες κωδικοποίησης είναι μία από τις πιο κοινές λειτουργίες επεξεργασίας στις υπηρεσίες πολυμέσων. Μπορείτε να δημιουργήσετε κωδικοποίησης εργασίες για να μετατρέψετε τα αρχεία πολυμέσων από μία κωδικοποίηση σε κάποιον άλλο. Όταν κωδικοποιείτε, μπορείτε να χρησιμοποιήσετε το Media Services ενσωματωμένο κωδικοποιητή (Media Encoder τυπική). Μπορείτε επίσης να χρησιμοποιήσετε έναν κωδικοποιητή που παρέχεται από ένα συνεργάτη Media Services; κωδικοποιητές τρίτων είναι διαθέσιμες από το Azure Marketplace. Μπορείτε να καθορίσετε τις λεπτομέρειες της κωδικοποίηση εργασίες με τη χρήση προκαθορισμένων συμβολοσειρές που ορίζονται για τον κωδικοποιητή ή χρησιμοποιώντας αρχεία προκαθορισμένου ρύθμισης παραμέτρων. Για να δείτε τους τύπους των προκαθορισμένες ρυθμίσεις που είναι διαθέσιμες, ανατρέξτε στο θέμα [Προκαθορισμένες ρυθμίσεις εργασιών για τυπική κωδικοποιητή πολυμέσων](http://msdn.microsoft.com/library/mt269960).

Κάθε εργασία μπορεί να έχει μία ή περισσότερες εργασίες ανάλογα με τον τύπο της επεξεργασίας που θέλετε να εκτελέσετε. Έως το REST API, μπορείτε να δημιουργήσετε εργασίες και τις σχετικές εργασίες με έναν από τους δύο τρόπους:

- Εργασίες μπορεί να είναι καθορισμένο ενσωματωμένη μέσω της ιδιότητας περιήγηση εργασίες στην εργασία οντοτήτων, ή
- έως OData μαζική επεξεργασία.


Συνιστάται να κωδικοποιείτε πάντα τα αρχεία σας θυγατρική σε μια προσαρμόσιμη ρυθμό μετάδοσης bit MP4 Ορισμός και, στη συνέχεια, να μετατρέψετε το σύνολο στην επιθυμητή μορφή χρησιμοποιώντας τη [Δυναμική συσκευασία](media-services-dynamic-packaging-overview.md). Για να επωφεληθείτε από δυναμική συσκευασία, πρέπει να λάβετε πρώτα τουλάχιστον μία On demand ροής μονάδας για τη ροή τελικό σημείο από το οποίο σκοπεύετε να παράδοσης το περιεχόμενό σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να κλίμακα Media Services](media-services-portal-manage-streaming-endpoints.md).

Εάν το αποτέλεσμα του περιουσιακού στοιχείου είναι κρυπτογραφημένα χώρου αποθήκευσης, πρέπει να ρυθμίσετε περιουσιακών στοιχείων παράδοσης πολιτικής. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων περιουσιακών στοιχείων παράδοσης πολιτικής](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Πριν να ξεκινήσετε αναφορά επεξεργαστές πολυμέσων, βεβαιωθείτε ότι έχετε το σωστό μέσο επεξεργαστής αναγνωριστικό. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Λήψη επεξεργαστές πολυμέσων](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Δημιουργήστε μια εργασία με μία μόνο εργασία κωδικοποίησης

>[AZURE.NOTE] Όταν εργάζεστε με το Media Services REST API, ισχύουν τα ακόλουθα θέματα:
>
>Κατά την πρόσβαση οντοτήτων στις υπηρεσίες πολυμέσων, πρέπει να ορίσετε συγκεκριμένες κεφαλίδα πεδία και τιμές στις προσκλήσεις HTTP σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ρύθμιση πολυμέσων υπηρεσίες REST API ανάπτυξης](media-services-rest-how-to-use.md).

>Μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md).
>
>Όταν χρησιμοποιώντας JSON και καθορίζοντας για να χρησιμοποιήσετε τη λέξη-κλειδί **__metadata** στην πρόσκληση σε (για παράδειγμα, για να αναφέρεται σε ένα συνδεδεμένο αντικείμενο) πρέπει να ορίσετε την κεφαλίδα **Αποδοχή** σε [μορφή JSON λεπτομερές](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/): Αποδοχή: εφαρμογή/json; odata = λεπτομερές.

Το παρακάτω παράδειγμα δείχνει πώς να δημιουργήσετε και να καταχωρήσετε μια εργασία με μία εργασία ρυθμιστεί για την κωδικοποίηση βίντεο σε μια συγκεκριμένη ανάλυση και την ποιότητα. Όταν κωδικοποίηση με τυπική κωδικοποιητή πολυμέσων, μπορείτε να χρησιμοποιήσετε προκαθορισμένες ρυθμίσεις παραμέτρων εργασίας που καθορίζονται [εδώ](http://msdn.microsoft.com/library/mt269960).

Αίτηση:

Https://media.windows.net/API/Jobs ΔΗΜΟΣΊΕΥΣΗ τύπου περιεχομένου HTTP/1.1: εφαρμογή/json; odata = λεπτομερές αποδοχή: εφαρμογή/json; odata = λεπτομερές DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0-ms-έκδοση x: 2.11 εξουσιοδότησης: φορέα <token value> 
 x-ms-πρόγραμμα-πελάτη--αναγνωριστικό αίτησης: 00000000-0000-0000-0000-000000000000 κεντρικού υπολογιστή: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Απόκριση:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Ορισμός ονόματος του παγίου εξόδου

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να ορίσετε το χαρακτηριστικό assetName:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Ζητήματα

- Ιδιότητες TaskBody πρέπει να χρησιμοποιήσετε το ακριβές XML για να ορίσετε τον αριθμό των εισόδου ή εξόδου περιουσιακών στοιχείων που θα χρησιμοποιηθεί κατά την εργασία. Το θέμα της εργασίας περιέχει ο ορισμός σχήματος XML για το αρχείο XML.
- Στον ορισμό TaskBody, τιμή για κάθε εσωτερική <inputAsset> και <outputAsset> πρέπει να οριστεί ως JobInputAsset(value) ή JobOutputAsset(value).
- Μια εργασία μπορεί να έχει πολλά στοιχεία εξόδου. Μία JobOutputAsset(x) μπορεί να χρησιμοποιηθεί μόνο μία φορά ως το αποτέλεσμα μιας εργασίας σε ένα έργο.
- Μπορείτε να καθορίσετε JobInputAsset ή JobOutputAsset ως ενός περιουσιακού στοιχείου εισαγωγής μιας εργασίας.
- Εργασίες δεν πρέπει να αποτελούν έναν κύκλο.
- Η παράμετρος value που έχετε στείλει JobInputAsset ή JobOutputAsset αντιπροσωπεύει την τιμή ευρετηρίου για ενός περιουσιακού στοιχείου. Τα πραγματικά στοιχεία ορίζονται στις ιδιότητες InputMediaAssets και OutputMediaAssets περιήγησης στον ορισμό οντότητα εργασίας. 
- Επειδή το Media Services είναι ενσωματωμένη σε OData v3, των μεμονωμένων περιουσιακών στοιχείων InputMediaAssets και OutputMediaAssets συλλογές ιδιοτήτων περιήγησης αναφέρονται μέσω ενός "__metadata: uri" ζεύγος ονόματος-τιμής.
- InputMediaAssets αντιστοιχεί σε ένα ή περισσότερα στοιχεία που έχετε δημιουργήσει στις υπηρεσίες πολυμέσων. OutputMediaAssets δημιουργούνται από το σύστημα. Αυτά παραπέμπει σε μια υπάρχουσα περιουσιακών στοιχείων.
- OutputMediaAssets μπορείτε να δώσετε όνομα χρησιμοποιώντας το χαρακτηριστικό assetName. Εάν δεν υπάρχει αυτό το χαρακτηριστικό και, στη συνέχεια, το όνομα του το OutputMediaAsset θα είναι ανεξάρτητα από την τιμή του εσωτερικό κείμενο το <outputAsset> στοιχείο είναι με κατάληξη είτε την τιμή όνομα έργου ή την τιμή αναγνωριστικό εργασίας (στην περίπτωση όπου δεν έχει οριστεί η ιδιότητα Name). Για παράδειγμα, εάν ορίσετε μια τιμή για assetName "Δείγμα", στη συνέχεια, η ιδιότητα OutputMediaAsset όνομα θα οριστεί σε "Δείγμα". Ωστόσο, εάν δεν έχει οριστεί τιμή για assetName, αλλά έχει αλλάξει ορίστε το όνομα της εργασίας για να "NewJob", στη συνέχεια, το όνομα OutputMediaAsset θα ήταν "_NewJob JobOutputAsset (τιμή)". 


##<a name="create-a-job-with-chained-tasks"></a>Δημιουργία μιας εργασίας με τις συνδεδεμένες εργασίες

Σε πολλά σενάρια εφαρμογής, οι προγραμματιστές θέλετε να δημιουργήσετε μια σειρά από επεξεργασία εργασιών. Στις υπηρεσίες πολυμέσων, μπορείτε να δημιουργήσετε μια σειρά από συνδεδεμένες εργασίες. Κάθε εργασία εκτελεί επεξεργασία διαφορετικά βήματα και να χρησιμοποιήσετε διαφορετικό πολυμέσων επεξεργαστές. Οι συνδεδεμένες εργασίες μπορούν να παραδώσετε ενός περιουσιακού στοιχείου από μία εργασία σε κάποιον άλλο, εκτελεί μια γραμμική σειρά εργασιών στο πάγιο. Ωστόσο, τις εργασίες που εκτελούνται σε ένα έργο δεν απαιτείται να είναι σε μια ακολουθία. Όταν δημιουργείτε μια συνδεδεμένη εργασία, των συνδεδεμένων αντικειμένων **ITask** δημιουργούνται στο ίδιο αντικείμενο **IJob** .

>[AZURE.NOTE] Υπάρχει αυτήν τη στιγμή όριο 30 εργασίες ανά εργασία. Εάν πρέπει να ποδηλάτου περισσότερα από 30 εργασίες, δημιουργήστε περισσότερες από μία εργασίας να περιέχει τις εργασίες.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Ζητήματα

Για να ενεργοποιήσετε την αλυσιδωτή εργασίας:

- Μια εργασία, πρέπει να έχετε τουλάχιστον 2 εργασίες
- Πρέπει να υπάρχει τουλάχιστον μία εργασία των οποίων εισόδου είναι εξόδου από μια άλλη εργασία της εργασίας.

## <a name="use-odata-batch-processing"></a>Χρήση OData μαζική επεξεργασία 

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε OData μαζική επεξεργασία για να δημιουργήσετε μια εργασία και εργασίες. Για πληροφορίες σχετικά με την επεξεργασία δέσμης, ανατρέξτε στο θέμα [Επεξεργασία δέσμης Open Data Protocol (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Δημιουργήστε μια εργασία χρησιμοποιώντας μια JobTemplate


Κατά την επεξεργασία πολλών περιουσιακών στοιχείων με χρήση ενός συνόλου κοινών εργασιών, JobTemplates είναι χρήσιμες για να καθορίσετε τις προεπιλεγμένες προκαθορισμένες ρυθμίσεις εργασιών, τη σειρά των εργασιών, και ούτω καθεξής.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε ένα JobTemplate με μια ενσωματωμένη TaskTemplate που ορίζονται από το. Το TaskTemplate χρησιμοποιεί την τυπική κωδικοποιητή πολυμέσων ως το MediaProcessor για να κωδικοποιήσετε το αρχείο περιουσιακών στοιχείων. Ωστόσο, οι άλλες MediaProcessors μπορεί να χρησιμοποιηθεί καθώς και. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Σε αντίθεση με άλλες υπηρεσίες πολυμέσων οντοτήτων, πρέπει να ορίζουν ένα νέο αναγνωριστικό GUID για κάθε TaskTemplate και να τοποθετήσετε στην taskTemplateId και την ιδιότητα αναγνωριστικό στο το σώμα της αίτησης. Το σχήμα περιεχομένου αναγνώρισης πρέπει να ακολουθήσετε το συνδυασμό που περιγράφονται σε αναγνώριση οντοτήτων υπηρεσίες πολυμέσων Azure. Επίσης, δεν είναι δυνατό να ενημερωθούν JobTemplates. Αντί για αυτό, πρέπει να δημιουργήσετε ένα νέο με τις αλλαγές σας ενημερωμένο.
 

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:
    
    HTTP/1.1 201 Created
    
    . . .


Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε μια αναφορά σε ένα αναγνωριστικό JobTemplate εργασία:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Εάν είναι επιτυχής, επιστρέφεται η ακόλουθη απάντηση:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Επόμενα βήματα
Τώρα που γνωρίζετε πώς μπορείτε να δημιουργήσετε μια εργασία για να κωδικοποιήσετε μια assset, μεταβείτε στο θέμα [Πώς να έλεγχος εργασία προόδου με τις υπηρεσίες πολυμέσων](media-services-rest-check-job-progress.md) .


##<a name="see-also"></a>Δείτε επίσης

[Λήψη επεξεργαστές πολυμέσων](media-services-rest-get-media-processor.md)

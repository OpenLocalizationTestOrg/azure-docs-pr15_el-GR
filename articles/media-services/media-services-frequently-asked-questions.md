<properties 
    pageTitle="Συνήθεις ερωτήσεις | Microsoft Azure" 
    description="Συνήθεις ερωτήσεις" 
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


#<a name="frequently-asked-questions"></a>Συνήθεις ερωτήσεις

##<a name="general-ams-faqs"></a>Συνήθεις ερωτήσεις αθροιστικής μέτρησης ενισχύσεων Γενικά

Ε: πώς μπορείτε να κλιμακωθεί δημιουργίας ευρετηρίου;

Α: το δεσμευμένο μονάδες είναι η ίδια για τις εργασίες κωδικοποίηση και δημιουργίας ευρετηρίου. Ακολουθήστε τις οδηγίες σχετικά με [τον τρόπο κλίμακα κωδικοποίηση δεσμευμένη μονάδες](media-services-scale-media-processing-overview.md). **Σημειώστε** ότι η απόδοση ευρετηρίου δεν επηρεάζεται από δεσμευμένη τύπος μονάδας.

Ε: μπορώ να αποστείλει κωδικοποιημένη και δημοσιευτεί ένα βίντεο. Τι θα είναι ο λόγος του βίντεο δεν αναπαράγεται όταν προσπαθώ να ροή αυτό;

Α: μία από τις πιο συνηθισμένες αιτίες είναι δεν έχετε τουλάχιστον ένα δεσμευμένο ροής μονάδας έχει εκχωρηθεί σε το ροής τελικό σημείο από το οποίο προσπαθείτε να αναπαραγωγής.  Ακολουθήστε τις οδηγίες σχετικά με [τον τρόπο κλίμακα ροής δεσμευμένη μονάδες](media-services-portal-scale-streaming-endpoints.md).

Ε: μπορώ να κάνω σύνθεσης σε μια ζωντανή ροή;

Α: σύνθεσης σε ζωντανή ροές αυτήν τη στιγμή δεν εμφανίζεται στον υπηρεσιών Azure Media Services, θα χρειαστεί να προ-σύνθεση στον υπολογιστή σας.

Ε: μπορώ να χρησιμοποιήσω Azure CDN με ροή Live;

Α: Media Services υποστηρίζει την ενσωμάτωση με το Azure CDN (για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να διαχειριστείτε ροής τα τελικά σημεία σε λογαριασμό υπηρεσίες πολυμέσων](media-services-portal-manage-streaming-endpoints.md)).  Μπορείτε να χρησιμοποιήσετε Live ροής με CDN. Azure Media Services παρέχει ομαλή εξόδους ροής, HLS και MPEG-ΠΑΎΛΑΣ. Όλες αυτές τις μορφές Χρησιμοποιήστε HTTP για τη μεταφορά δεδομένων και λήψη οφέλη από HTTP σε cache. Σε ζωντανή ροή πραγματική δεδομένων ήχου/βίντεο χωρίζεται σε τμήματα και αυτό μεμονωμένα τμήματα γρήγορα προσωρινά στο CDN. Μόνο τα δεδομένα πρέπει να είναι ανανεωμένα είναι τα δεδομένα δήλωσης. CDN ανανεώνει περιοδικά δήλωσης δεδομένα.

Ε: κάνει Azure Media services υποστηρίζουν την αποθήκευση εικόνων;

Α: Εάν θέλετε απλώς να αποθηκεύσετε εικόνες JPEG ή PNG, θα πρέπει να διατηρήσετε εκείνες στο χώρο αποθήκευσης αντικειμένων Blob Azure. Δεν υπάρχει κανένα όφελος τοποθέτησή τους στο λογαριασμό σας στο Media Services, εκτός αν θέλετε να διατηρήσετε τους που σχετίζεται με το βίντεο ή ήχου περιουσιακών στοιχείων. Ή εάν που μπορεί να χρειάζεται να χρησιμοποιήσετε τις εικόνες ως επικαλύψεων σε ο κωδικοποιητής βίντεο. Media Encoder τυπική υποστηρίζει επικάλυψη εικόνες επάνω σε βίντεο και ο οποίος τι παραθέτει JPEG και PNG όπως υποστηρίζονται εισαγωγής μορφές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία επικαλύψεων](media-services-custom-mes-presets-with-dotnet.md#overlay).

Ε: πώς μπορώ να αντιγράψω στοιχεία από ένα λογαριασμό Media Services σε μια άλλη.

Α: για να αντιγράψετε στοιχεία από ένα λογαριασμό Media Services σε ένα άλλο χρησιμοποιώντας .NET, χρησιμοποιήστε τη μέθοδο επέκτασης [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) διαθέσιμη στο αποθετήριο [Azure Media Services .NET SDK επεκτάσεις](https://github.com/Azure/azure-sdk-for-media-services-extensions/) . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτού](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) του νήματος φόρουμ.

Ε: Τι είναι οι υποστηριζόμενες χαρακτήρες για την ονομασία αρχείων κατά την εργασία με αθροιστικής μέτρησης ενισχύσεων;

Α: Media Services χρησιμοποιεί την τιμή της ιδιότητας IAssetFile.Name κατά τη δημιουργία διευθύνσεις URL για τη ροή περιεχομένου (για παράδειγμα, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Για αυτόν το λόγο, δεν επιτρέπεται η κωδικοποίηση. Η τιμή της ιδιότητας **όνομα** δεν μπορεί να έχει έναν από τους παρακάτω [χαρακτήρες τοις εκατό κωδικοποίηση-αποκλειστικές](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Επίσης, μπορεί να υπάρξει μόνο μία '.' για την επέκταση ονόματος αρχείου.


Ε: πώς να συνδεθείτε χρησιμοποιώντας το ΥΠΌΛΟΙΠΟ;

Α: μετά τη σύνδεση με επιτυχία στο https://media.windows.net, θα λάβετε μια 301 redirect καθορίζοντας ένα άλλο URI υπηρεσίες πολυμέσων. Πρέπει να κάνετε οι επόμενες κλήσεις για το νέο URI όπως περιγράφεται στο θέμα [σύνδεση με το Media Services χρησιμοποιώντας REST API](media-services-rest-connect-programmatically.md). 


Ε: πώς να να περιστρέψετε ένα βίντεο κατά τη διαδικασία κωδικοποίησης.

Α: το [Media Encoder τυπική](media-services-dotnet-encode-with-media-encoder-standard.md) υποστηρίζει περιστροφής από τις γωνίες των 90/180/270. Η προεπιλεγμένη συμπεριφορά είναι "Αυτόματη", όπου προσπαθεί να εντοπίσει τα μετα-δεδομένα περιστροφής στο αρχείο εισερχόμενες MP4/MOV και αποζημιώσει για αυτήν. Συμπεριλάβετε το ακόλουθο στοιχείο **προελεύσεις** σε ένα από τα json υποδείγματα καθορισμένο [εδώ](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

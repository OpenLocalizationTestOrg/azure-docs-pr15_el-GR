<properties 
    pageTitle="Επισκόπηση πρότυπο άδειας χρήσης Widevine | Microsoft Azure" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση του Widevine άδεια χρήσης του προτύπου που χρησιμοποιείται για τη ρύθμιση παραμέτρων Widevine άδειες χρήσης." 
    authors="juliako" 
    manager="erikre" 
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

#<a name="widevine-license-template-overview"></a>Επισκόπηση πρότυπο Widevine άδειας χρήσης

##<a name="overview"></a>Επισκόπηση

Azure Media Services τώρα σάς επιτρέπει να ρυθμίσετε τις παραμέτρους και να ζητήσετε Widevine άδειες χρήσης. Όταν το πρόγραμμα αναπαραγωγής τελικού χρήστη προσπαθεί να αναπαραγάγετε το περιεχόμενό σας Widevine προστατεύεται, αποστέλλεται μια αίτηση για την υπηρεσία παράδοσης άδειας χρήσης για να αποκτήσετε μια άδεια χρήσης. Εάν η υπηρεσία άδειας χρήσης απορρίψει την αίτηση, θέματα την άδεια χρήσης που αποστέλλονται στον υπολογιστή-πελάτη και μπορούν να χρησιμοποιηθούν για αποκρυπτογράφηση και αναπαραγωγή του περιεχομένου που καθορίζεται.

Αίτηση άδειας χρήσης Widevine μορφοποιείται ως μήνυμα JSON.  

Σημειώστε ότι μπορείτε να επιλέξετε να δημιουργήσετε ένα κενό μήνυμα χωρίς τιμές απλώς "{}" και θα δημιουργηθεί μια άδεια χρήσης του προτύπου με όλες τις προεπιλογές.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>Μήνυμα JSON

Όνομα | Τιμή | Περιγραφή
---|---|---
φορτίο |Κωδικοποίηση Base64 συμβολοσειράς |Η αίτηση άδειας χρήσης που αποστέλλονται από ένα πρόγραμμα-πελάτη. 
content_id | Κωδικοποίηση Base64 συμβολοσειράς|Αναγνωριστικό που χρησιμοποιείται για την παραγωγή KeyId(s) και κλειδιά περιεχομένου για κάθε content_key_specs.track_type.
υπηρεσία παροχής |συμβολοσειρά |Χρησιμοποιείται για την αναζήτηση περιεχομένου πλήκτρα και τις πολιτικές. Απαιτείται.
policy_name | συμβολοσειρά |Όνομα πολιτικής έχει ήδη καταχωρηθεί. Προαιρετικό
allowed_track_types | Απαρίθμηση  | SD_ONLY ή SD_HD. Στοιχεία ελέγχου που περιεχομένου πλήκτρα πρέπει να περιλαμβάνονται στην άδεια χρήσης
content_key_specs | πίνακας με JSON δομές, ανατρέξτε στο θέμα **Προδιαγραφές περιεχομένου κλειδί** παρακάτω | Ένα καλύτερο γίνει στοιχείου ελέγχου σε ποιο περιεχόμενο πλήκτρων για να επιστρέψει. Για λεπτομέρειες, ανατρέξτε στο θέμα περιεχομένου προδιαγραφή πλήκτρο κάτω από.  Μπορεί να καθοριστεί μόνο μία από τις allowed_track_types και content_key_specs. 
use_policy_overrides_exclusively | μια δυαδική τιμή. TRUE ή false | Χρήση πολιτικής χαρακτηριστικά που καθορίζονται από policy_overrides και να παραλείψετε όλες πολιτικής έχουν αποθηκευτεί προηγουμένως.
policy_overrides | JSON δομή, ανατρέξτε στο θέμα **Πολιτική παρακάμπτει** παρακάτω | Ρυθμίσεις πολιτικής για αυτής της άδειας χρήσης.  Σε περίπτωση αυτού του περιουσιακού στοιχείου έχει μια προκαθορισμένη πολιτική, θα χρησιμοποιηθεί αυτές τις καθορισμένες τιμές. 
session_init | JSON δομή, ανατρέξτε στο θέμα **Προετοιμασία της περιόδου λειτουργίας** παρακάτω | Προαιρετικό δεδομένα περνούν σε άδεια χρήσης.
parse_only | μια δυαδική τιμή. TRUE ή false | Ανάλυση της αίτησης άδειας χρήσης, αλλά εκδίδεται χωρίς άδεια χρήσης. Ωστόσο, οι τιμές φόρμας την αίτηση άδειας χρήσης έχουν επιστραφεί στην απάντηση.  

##<a name="content-key-specs"></a>Προδιαγραφές περιεχομένου κλειδί 

Εάν υπάρχει μια ήδη υπάρχουσα πολιτική, δεν χρειάζεται για να καθορίσετε οποιαδήποτε από τις τιμές σε οι προδιαγραφές αριθμό-κλειδί του περιεχομένου.  Η πολιτική προ-υπαρχόντων που σχετίζονται με αυτό το περιεχόμενο θα χρησιμοποιηθεί για να προσδιορίσετε την προστασία εξόδου όπως HDCP και CGMS.  Εάν μια ήδη υπάρχουσα πολιτική δεν έχει καταχωρηθεί με το διακομιστή Widevine άδεια χρήσης, η υπηρεσία παροχής περιεχομένου μπορεί να εισαγάγει τις τιμές στην την αίτηση άδειας χρήσης.   


Κάθε content_key_specs πρέπει να καθοριστεί για όλα τα κομμάτια, ανεξάρτητα από την επιλογή use_policy_overrides_exclusively. 


Όνομα | Τιμή | Περιγραφή
---|---|---
content_key_specs. track_type | συμβολοσειρά | Όνομα τύπου παρακολούθηση. Εάν content_key_specs έχει οριστεί στην αίτηση άδειας χρήσης, βεβαιωθείτε ότι για να καθορίσετε όλους τους τύπους παρακολούθηση ρητά. Αποτυχία δεν θα έχει ως αποτέλεσμα την αποτυχία για αναπαραγωγή τελευταίες 10 δευτερόλεπτα. 
content_key_specs  <br/> security_level | uint32 | Καθορίζει απαιτήσεις ισχύ προγράμματος-πελάτη για την αναπαραγωγή. <br/> 1 - απαιτείται crypto whitebox με βάση το λογισμικό. <br/> 2 - λογισμικό crypto και μια θολού αποκωδικοποιητή απαιτείται. <br/> 3 - τις crypto λειτουργίες και υλικό κλειδιού πρέπει να εκτελούνται μέσα σε ένα περιβάλλον εκτέλεσης αντίγραφα ασφαλείας αξιόπιστων υλικού. <br/> 4 - την κρυπτογράφηση και αποκωδικοποίηση του περιεχομένου, πρέπει να εκτελούνται μέσα σε ένα περιβάλλον εκτέλεσης αντίγραφα ασφαλείας αξιόπιστων υλικού.  <br/> 5 - οι κρυπτογράφησης, Παρουσιάστηκε και όλα χειρισμού πολυμέσων (συμπιέζονται και χωρίς συμπίεση) πρέπει να αντιμετώπισης μέσα σε ένα περιβάλλον εκτέλεσης αντίγραφα ασφαλείας αξιόπιστων υλικού.  
content_key_specs <br/> required_output_protection.hDC | συμβολοσειρά - μία από: HDCP_NONE, HDCP_V1, HDCP_V2 | Υποδεικνύει αν είναι απαιτούν HDCP
content_key_specs <br/>πλήκτρο | Base64 <br/>κωδικοποιημένη συμβολοσειρά|Το κλειδί για να χρησιμοποιήσετε για αυτό το κομμάτι περιεχομένου. Εάν καθοριστεί, το track_type ή key_id απαιτείται.  Αυτή η επιλογή επιτρέπει την υπηρεσία παροχής περιεχομένου για την εισαγωγή του αριθμού-κλειδιού περιεχομένου για αυτό το κομμάτι αντί να εκφράζετε διακομιστή αδειών χρήσης Widevine δημιουργούν ή να αναζητήσει έναν αριθμό-κλειδί.
content_key_specs.key_id| Base64 κωδικοποιημένη συμβολοσειρά δυαδικό, 16 byte | Μοναδικό αναγνωριστικό για τον αριθμό-κλειδί. 


##<a name="policy-overrides"></a>Υπερισχύει η πολιτική 

Όνομα | Τιμή | Περιγραφή
---|---|---
policy_overrides. can_play | μια δυαδική τιμή. TRUE ή false | Υποδεικνύει επιτρέπεται η αναπαραγωγή του περιεχομένου. Η προεπιλογή είναι false.
policy_overrides. can_persist | μια δυαδική τιμή. TRUE ή false |Υποδεικνύει ότι η άδεια χρήσης μπορεί να είναι μόνιμες μόνιμης αποθήκευσης για χρήση χωρίς σύνδεση. Η προεπιλογή είναι false.
policy_overrides. can_renew | δυαδική τιμή true ή false |Υποδεικνύει ότι επιτρέπεται η ανανέωση αυτής της άδειας χρήσης. Εάν είναι true, η διάρκεια της άδειας χρήσης μπορεί να επεκταθεί κατά το συγχρονισμό. Η προεπιλογή είναι false. 
policy_overrides. license_duration_seconds | Int64 | Υποδεικνύει το χρονικό διάστημα για συγκεκριμένες αυτής της άδειας χρήσης. Η τιμή 0 υποδεικνύει ότι δεν υπάρχει όριο για τη διάρκεια. Η προεπιλογή είναι 0. 
policy_overrides. rental_duration_seconds | Int64 | Υποδεικνύει το χρονικό διάστημα, ενώ επιτρέπεται η αναπαραγωγή. Η τιμή 0 υποδεικνύει ότι δεν υπάρχει όριο για τη διάρκεια. Η προεπιλογή είναι 0. 
policy_overrides. playback_duration_seconds | Int64 | Στο παράθυρο προβολής του χρόνου μετά την αναπαραγωγή ξεκινά κατά τη διάρκεια της άδειας χρήσης. Η τιμή 0 υποδεικνύει ότι δεν υπάρχει όριο για τη διάρκεια. Η προεπιλογή είναι 0. 
policy_overrides. renewal_server_url |συμβολοσειρά | Όλες οι αιτήσεις συγχρονισμό (ανανέωση) για αυτής της άδειας χρήσης απευθύνεται στην καθορισμένη διεύθυνση URL. Αυτό το πεδίο χρησιμοποιείται μόνο εάν ισχύει can_renew.
policy_overrides. renewal_delay_seconds |Int64 |Πόσα δευτερόλεπτα μετά license_start_time, πριν από την ανανέωση επιχειρείται πρώτα. Αυτό το πεδίο χρησιμοποιείται μόνο εάν ισχύει can_renew. Η προεπιλογή είναι 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Καθορίζει την καθυστέρηση σε δευτερόλεπτα μεταξύ των αιτήσεων ανανέωσης οι επόμενες άδειας χρήσης, σε περίπτωση αποτυχίας. Αυτό το πεδίο χρησιμοποιείται μόνο εάν ισχύει can_renew. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Το παράθυρο χρόνου, κατά το οποίο επιτρέπεται η αναπαραγωγή για να συνεχίσετε κατά την ανανέωση είναι Επιχειρήθηκε, ακόμη επιτυχής οφείλεται σε προβλήματα παρασκηνίου με το διακομιστή αδειών χρήσης. Η τιμή 0 υποδεικνύει ότι δεν υπάρχει όριο για τη διάρκεια. Αυτό το πεδίο χρησιμοποιείται μόνο εάν ισχύει can_renew.
policy_overrides. renew_with_usage | δυαδική τιμή true ή false |Υποδεικνύει ότι πρέπει να σταλεί την άδεια χρήσης για την ανανέωση όταν ξεκινήσει με χρήση. Αυτό το πεδίο χρησιμοποιείται μόνο εάν ισχύει can_renew. 

##<a name="session-initialization"></a>Προετοιμασία της περιόδου λειτουργίας

Όνομα | Τιμή | Περιγραφή
---|---|---
provider_session_token | Κωδικοποίηση Base64 συμβολοσειράς |Αυτό το διακριτικό περιόδου λειτουργίας μεταβιβάζεται πίσω στο την άδεια χρήσης και θα υπάρχει σε επόμενες ανανέωση.  Δεν θα εξακολουθούν να το διακριτικό περιόδου λειτουργίας πέρα από τις περιόδους λειτουργίας. 
provider_client_token | Κωδικοποίηση Base64 συμβολοσειράς | Το διακριτικό του προγράμματος-πελάτη για να στείλετε ξανά στην απάντηση άδεια χρήσης.  Εάν η αίτηση άδειας χρήσης περιέχει ένα διακριτικό προγράμματος-πελάτη, αυτή η τιμή παραβλέπεται. Το διακριτικό του προγράμματος-πελάτη θα εξακολουθούν να εμφανίζονται πέρα από τις περιόδους λειτουργίας άδεια χρήσης.
override_provider_client_token | μια δυαδική τιμή. TRUE ή false |Εάν false και την αίτηση άδειας χρήσης περιέχει ενός διακριτικού προγράμματος-πελάτη, χρησιμοποιήστε το διακριτικό από την αίτηση, ακόμα και αν έχει καθοριστεί ενός διακριτικού προγράμματος-πελάτη σε αυτήν τη δομή.  Εάν είναι αληθές, χρησιμοποιείτε πάντα το διακριτικό που καθορίστηκε σε αυτήν τη δομή.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Ρύθμιση παραμέτρων των αδειών χρήσης Widevine χρησιμοποιώντας τύπους .NET

Υπηρεσίες πολυμέσων παρέχει API .NET που σας επιτρέπουν να διαμορφώσετε τις άδειες χρήσης Widevine. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Κατηγορίες όπως ορίζεται στο SDK .NET υπηρεσίες πολυμέσων

Ακολουθούν οι ορισμοί από αυτούς τους τύπους.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε .NET APIs για να ρυθμίσετε τις παραμέτρους μιας απλής άδειας χρήσης Widevine.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Δείτε επίσης

[Χρήση PlayReady ή/και Widevine δυναμικής κοινές κρυπτογράφησης](media-services-protect-with-drm.md)

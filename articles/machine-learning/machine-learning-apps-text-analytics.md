<properties
    pageTitle="Μηχανικής εκμάθησης APIs: Ανάλυση κειμένου | Microsoft Azure"
    description="Της Microsoft μηχανικής εκμάθησης κειμένου ανάλυση APIs μπορεί να χρησιμοποιηθεί για την ανάλυση μη δομημένα κείμενο για ανάλυση άποψη, εξαγωγής κλειδιού φράση, εντοπισμού γλώσσας και το θέμα ανίχνευση."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>APIs μηχανικής εκμάθησης: Ανάλυση κειμένου για άποψη, βασικές εξαγωγής φράση, εντοπισμού γλώσσας και το θέμα εντοπισμού

>[AZURE.NOTE] Αυτός ο οδηγός είναι για την έκδοση 1 του API. Για την έκδοση 2, που [**αναφέρονται σε αυτό το έγγραφο**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). Έκδοση 2 είναι τώρα την προτιμώμενη έκδοση του αυτό το API.

## <a name="overview"></a>Επισκόπηση

Το API ανάλυση κειμένου είναι μια οικογένεια κειμένου ανάλυση [υπηρεσίες web](https://datamarket.azure.com/dataset/amla/text-analytics) ενσωματωμένο με το Azure μηχανικής εκμάθησης. Το API μπορεί να χρησιμοποιηθεί για την ανάλυση μη δομημένα κείμενο για εργασίες όπως η ανάλυση άποψη, εξαγωγής κλειδιού φράση, εντοπισμού γλώσσας και το θέμα ανίχνευση. Δεν υπάρχουν δεδομένα εκπαίδευση είναι απαραίτητη για να χρησιμοποιήσετε αυτό το API: απλώς μεταφέρετε τα δεδομένα σας κείμενο. Αυτό το API χρησιμοποιεί για προχωρημένους φυσικής γλώσσας επεξεργασίας τεχνικές για την παράδοση καλύτερα στο εκπαιδευτικό προβλέψεων.

Μπορείτε να δείτε αναλυτικά στοιχεία κειμένου σε δράση μας [επίδειξη τοποθεσίας](https://text-analytics-demo.azurewebsites.net/), όπου θα βρείτε επίσης [δείγματα](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) σχετικά με τον τρόπο για την υλοποίηση της ανάλυσης κειμένου στο C# και Python.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Άποψη ανάλυσης

Το API επιστρέφει μια αριθμητική βαθμολογία μεταξύ 0 και 1. Βαθμολογίες κοντά 1 υποδεικνύουν θετικό άποψη, ενώ βαθμολογίες κοντά 0 υποδεικνύουν αρνητική άποψη. Άποψη βαθμολογία δημιουργείται χρησιμοποιώντας τεχνικές ταξινόμηση. Οι δυνατότητες εισαγωγής με την τάξη περιλαμβάνουν n-^ g, δυνατότητες που δημιουργούνται από τις ετικέτες τμήματος ομιλίας και ενσωματώσεων word. Προς το παρόν, Αγγλικά είναι η γλώσσα που υποστηρίζεται μόνο.
 
## <a name="key-phrase-extraction"></a>Πλήκτρο φράση εξαγωγής

Το API επιστρέφει μια λίστα με τις συμβολοσειρές δηλώνοντας τα βασικά σημεία συζήτησης εισαγωγής κειμένου. Χρησιμοποιούμε τεχνικές από το Microsoft Office εξελιγμένο φυσικής γλώσσας επεξεργασίας Κιτ εργαλείων. Προς το παρόν, Αγγλικά είναι η γλώσσα που υποστηρίζεται μόνο.

## <a name="language-detection"></a>Εντοπισμός γλώσσας

Το API επιστρέφει τη γλώσσα που εντοπίστηκε και αριθμητικές βαθμολογία μεταξύ 0 και 1. Βαθμολογίες κοντά 1 υποδεικνύουν 100% βέβαιοι ότι η γλώσσα προσδιορισμένο είναι true. Ένα σύνολο 120 γλώσσες που υποστηρίζονται.

## <a name="topic-detection"></a>Εντοπισμός θέματος

Αυτό είναι ένα API που μόλις έχει κυκλοφορήσει ποια επιστρέφει επάνω εντοπίστηκε για μια λίστα με τα θέματα που υποβάλλεται εγγραφές κειμένου. Ένα θέμα προσδιορίζεται με βάση μια φράση κλειδιού, που μπορεί να είναι ένα ή περισσότερα που σχετίζονται με λέξεις. Αυτό το API απαιτεί τουλάχιστον 100 κειμένου εγγραφές, πρέπει να υποβάλλονται, αλλά έχει σχεδιαστεί για να εντοπίσετε τα θέματα σε εκατοντάδες σε χιλιάδες εγγραφές. Σημειώστε ότι αυτό το API χρεώσεων 1 συναλλαγή ανά εγγραφή κειμένου που υποβάλλεται. Το API έχει σχεδιαστεί ώστε να λειτουργεί καλά για σύντομο, human χειρόγραφων κείμενο όπως αναθεωρήσεις και τα σχόλια χρήστη.

---

## <a name="api-definition"></a>Ορισμός API

### <a name="headers"></a>Κεφαλίδες

Βεβαιωθείτε ότι συμπεριλαμβάνετε τη σωστή κεφαλίδες στην πρόσκληση σε, που πρέπει να είναι ως εξής:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Μπορείτε να βρείτε το κλειδί λογαριασμού σας από το λογαριασμό σας στην [Αγορά δεδομένων Azure](https://datamarket.azure.com/account/keys). Σημειώστε ότι τη συγκεκριμένη στιγμή μόνο JSON γίνει αποδεκτή για μορφές εισόδου και εξόδου. XML δεν υποστηρίζεται.

---

## <a name="single-response-apis"></a>APIs μία απόκρισης

### <a name="getsentiment"></a>GetSentiment

**ΔΙΕΎΘΥΝΣΗ URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Παράδειγμα αίτηση**

Στην κλήση κάτω από το στοιχείο, θα σας ζητούν άποψη ανάλυσης για τη φράση "Γεια":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Αυτό θα επιστρέψει μια απάντηση ως εξής:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Παράδειγμα αίτηση**

Στην παρακάτω κλήση, θα σας ζητούν τις βασικές φράσεις που βρέθηκε στο κείμενο "Ήταν ένα υπέροχο ξενοδοχείο, για να παραμείνετε στο, με μοναδικό décor και φιλικό προσωπικού":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Αυτό θα επιστρέψει μια απάντηση ως εξής:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Παράδειγμα αίτηση**

Στο παρακάτω την κλήση ΓΡΉΓΟΡΑ, θα σας αίτηση για την άποψη για τις βασικές φράσεις στο κείμενο *Γεια*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Αυτό θα επιστρέψει μια απάντηση ως εξής:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Προαιρετικές παράμετροι**

`NumberOfLanguagesToDetect`είναι μια προαιρετική παράμετρος. Η προεπιλογή είναι 1.

---

## <a name="batch-apis"></a>APIs δέσμης

Η υπηρεσία Analytics κειμένου σάς επιτρέπει να κάνετε άποψη και αριθμού-κλειδιού φράση εξαγωγές σε κατάσταση λειτουργίας δέσμης. Σημειώστε ότι κάθε μία από τις εγγραφές έχει βαθμολογηθεί υπολογίζεται ως μία συναλλαγή. Ως παράδειγμα, εάν ζητήσετε άποψη για 1000 εγγραφές σε μια μεμονωμένη κλήση, 1000 συναλλαγές θα αφαιρούνται.

Σημειώστε ότι τα αναγνωριστικά εισάγονται στο σύστημα είναι τα αναγνωριστικά που επιστράφηκε από το σύστημα. Η υπηρεσία web δεν ελέγχει ότι αυτά τα αναγνωριστικά είναι μοναδικά. Είναι ευθύνη του καλούντος για να επιβεβαιώσετε τη μοναδικότητα. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**ΔΙΕΎΘΥΝΣΗ URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Παράδειγμα αίτηση**

Στην κλήση κάτω από την ΚΑΤΑΧΏΡΗΣΗ, θα σας αίτηση για την sentiments από τις φράσεις "Γεια", "Foo Γεια" και "Μου Γεια" στο σώμα της πρόσκλησης σε:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Σώμα αίτησης:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

Στην παρακάτω απάντηση, μπορείτε να λάβετε τη λίστα των βαθμών που σχετίζεται με το κείμενό σας αναγνωριστικά:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Παράδειγμα αίτηση**

Σε αυτό το παράδειγμα, θα σας αίτηση για τη λίστα των sentiments για τις βασικές φράσεις στο τα ακόλουθα κείμενα: 

* "Ήταν ένα υπέροχο ξενοδοχείο, για να παραμείνετε στο, με μοναδικό décor και φιλική προσωπικό"
* "Ήταν καταπληκτικών Δόμηση διάσκεψης, με πολύ ενδιαφέρον συνομιλίες"
* "Η κίνηση ήταν απαίσια, να που αναλώθηκε 3 ώρες μεταβαίνοντας στο αεροδρόμιο"

Αυτή η αίτηση γίνεται ως κλήση ΔΗΜΟΣΊΕΥΣΗ στο τελικό σημείο:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Σώμα αίτησης:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

Στην παρακάτω απάντηση, μπορείτε να λάβετε τη λίστα των κλειδιών φράσεις που σχετίζεται με το κείμενό σας αναγνωριστικά:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Αγγλικά",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Γαλλικά",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Το θέμα ανίχνευση APIs

Αυτό είναι ένα API που μόλις έχει κυκλοφορήσει ποια επιστρέφει επάνω εντοπίστηκε για μια λίστα με τα θέματα που υποβάλλεται εγγραφές κειμένου. Ένα θέμα προσδιορίζεται με βάση μια φράση κλειδιού, που μπορεί να είναι ένα ή περισσότερα που σχετίζονται με λέξεις. Σημειώστε ότι αυτό το API χρεώσεων 1 συναλλαγή ανά εγγραφή κειμένου που υποβάλλεται.

Αυτό το API απαιτεί τουλάχιστον 100 κειμένου εγγραφές, πρέπει να υποβάλλονται, αλλά έχει σχεδιαστεί για να εντοπίσετε τα θέματα σε εκατοντάδες σε χιλιάδες εγγραφές.


### <a name="topics--submit-job"></a>Θέματα – υποβολή εργασίας

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Παράδειγμα αίτηση**


Στην ΚΑΤΑΧΏΡΗΣΗ κλήση παρακάτω, θα σας ζητούν θέματα για ένα σύνολο 100 άρθρα, όπου εμφανίζονται τα άρθρα εισαγωγής πρώτο και το τελευταίο και οι δύο StopPhrases συμπεριλαμβάνονται.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Σώμα αίτησης:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

Στην παρακάτω απάντηση, μπορείτε να λάβετε το JobId για το έργο που έχει υποβληθεί:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Μια λίστα με μία λέξη ή πολλών φράσεων word, οι οποίες δεν πρέπει να επιστραφεί ως θέματα. Μπορεί να χρησιμοποιηθεί για να φιλτράρεται πολύ γενικό θέματα. Για παράδειγμα, σε ένα σύνολο δεδομένων σχετικά με τις αναθεωρήσεις ξενοδοχείο, "ξενοδοχείο" και "hostel" μπορεί να είναι ορθολογική διακοπή φράσεις.  

### <a name="topics--poll-for-job-results"></a>Θέματα – ψηφοφορίας για εργασία αποτελέσματα

**ΔΙΕΎΘΥΝΣΗ URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Παράδειγμα αίτηση**

Μεταβίβαση το JobId που επιστρέφονται από το βήμα 'Υποβολή εργασίας' για τη λήψη των αποτελεσμάτων. Σας συνιστούμε να ώστε να καλέσετε αυτό το τελικό σημείο κάθε λεπτό μέχρι την κατάσταση = 'Ολοκλήρωσης' στην απάντηση. Θα χρειαστεί περίπου 10 λεπτά για ένα έργο για να ολοκληρωθεί ή περισσότερο για εργασίες με πολλές χιλιάδες εγγραφές.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Ενώ επεξεργάζεται, η απόκριση θα είναι ως εξής:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Το API επιστρέφει αποτέλεσμα σε μορφή JSON με την εξής μορφή:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Οι ιδιότητες για κάθε τμήμα της απόκρισης είναι ως εξής:

**Ιδιότητες TopicInfo**

| Πλήκτρο | Περιγραφή |
|:-----|:----|
| TopicId | Ένα μοναδικό αναγνωριστικό για κάθε θέμα. |
| Βαθμολογία | Πλήθος των εγγραφών που έχουν εκχωρηθεί σε το θέμα. |
| KeyPhrase | Summarizing λέξης ή φράσης για το θέμα. Μπορεί να είναι 1 ή πολλές λέξεις. |

**Ιδιότητες TopicAssignment**

| Πλήκτρο | Περιγραφή |
|:-----|:----|
| Αναγνωριστικό | Αναγνωριστικό για την εγγραφή. Ισούται με το Αναγνωριστικό που περιλαμβάνονται στην είσοδο. |
| TopicId | Το Αναγνωριστικό το θέμα που έχει εκχωρηθεί η εγγραφή. |
| Απόσταση | Εμπιστοσύνης ότι η εγγραφή ανήκει στο θέμα. Απόσταση πιο κοντά σε μηδέν υποδεικνύει εμπιστοσύνης νεότερη έκδοση. |

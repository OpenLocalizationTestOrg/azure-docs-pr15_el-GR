<properties
    pageTitle="Αναβάθμιση σε έκδοση 2 του την ανάλυση κειμένου API | Microsoft Azure"
    description="Azure μηχανικής εκμάθησης ανάλυση κειμένου - αναβάθμιση στην έκδοση 2"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Αναβάθμιση σε έκδοση 2 του την ανάλυση κειμένου API #

Αυτός ο οδηγός θα σας καθοδηγήσει τη διεργασία αναβάθμισης τον κωδικό από χρησιμοποιώντας την [πρώτη έκδοση του API](../machine-learning/machine-learning-apps-text-analytics.md) με τη χρήση του η δεύτερη έκδοση. 

Εάν δεν έχετε χρησιμοποιήσει το API και θέλετε να μάθετε περισσότερα, μπορείτε να **[Μάθετε περισσότερα σχετικά με το API εδώ](//go.microsoft.com/fwlink/?LinkID=759711)** ή την **[Παρακολούθηση του Οδηγού γρήγορης εκκίνησης](//go.microsoft.com/fwlink/?LinkID=760860)**. Για τεχνικές αναφοράς, ανατρέξτε στον **[Ορισμό API](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Μέρος 1. Λάβετε έναν νέο αριθμό-κλειδί ###

Πρώτα, θα πρέπει να λάβετε ένα νέο αριθμό-κλειδί API από την **Πύλη Azure**:

1. Μεταβείτε στην υπηρεσία ανάλυση κειμένου από τη [Συλλογή πληροφοριών Cortana](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). Εδώ, μπορείτε επίσης θα βρείτε συνδέσεις για τα δείγματα τεκμηρίωση και τον κωδικό.

1. Κάντε κλικ στην επιλογή **εγγραφή**. Αυτή η σύνδεση θα σας μεταφέρει στην πύλη του Azure διαχείρισης, όπου μπορείτε να εγγραφείτε για την υπηρεσία.

1. Επιλέξτε ένα πρόγραμμα. Μπορείτε να επιλέξετε το **επίπεδο δωρεάν 5.000 συναλλαγές/μήνα**. Όπως ένα δωρεάν πρόγραμμα, που δεν θα χρεωθεί για τη χρήση της υπηρεσίας. Θα πρέπει να συνδεθείτε στη συνδρομή σας στο Azure. 

1. Αφού εγγραφείτε για ανάλυση κειμένου, θα έχετε ένα **Πλήκτρο API**. Αντιγράψτε αυτό το κλειδί, όπως θα τον χρειαστείτε κατά τη χρήση των υπηρεσιών API.

### <a name="part-2-update-the-headers"></a>Μέρος 2. Ενημέρωση των κεφαλίδων ###

Ενημερώστε τις τιμές που έχουν υποβληθεί κεφαλίδας, όπως φαίνεται παρακάτω. Σημειώστε ότι το κλειδί λογαριασμού κωδικοποιείται δεν είναι πλέον.

**Έκδοση 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Έκδοση 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Μέρος 3. Ενημερώστε τη βασική διεύθυνση URL ###

**Έκδοση 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Έκδοση 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Τμήμα 4α. Ενημερώστε τις μορφές για άποψη, κλειδιού φράσεις και γλώσσες ###

#### <a name="endpoints"></a>Τα τελικά σημεία ####

ΛΉΨΗ τελικά σημεία έχουν τώρα έχει καταργηθεί, ώστε όλα τα δεδομένα εισόδου θα πρέπει να υποβάλλονται ως μια αίτηση POST. Ενημερώστε τα τελικά σημεία με αυτά που φαίνεται παρακάτω.

| |Τελικό σημείο μεμονωμένη έκδοση 1|Τελικό σημείο δέσμη έκδοση 1|Τελικό σημείο έκδοση 2|
|---|---|---|---|
|Ο τύπος κλήσης|ΛΉΨΗ|ΔΗΜΟΣΊΕΥΣΗ|ΔΗΜΟΣΊΕΥΣΗ|
|Άποψη|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Πλήκτρο φράσεις|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Γλώσσες|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Μορφές εισόδου ####

Σημείωση αυτή μόνο τη μορφή ΔΗΜΟΣΊΕΥΣΗ τώρα γίνει αποδεκτή, έτσι θα πρέπει να διαμορφώσετε οποιαδήποτε εισαγωγή που χρησιμοποιήθηκε παλιότερα τα τελικά σημεία μεμονωμένο έγγραφο αντίστοιχα. Δεδομένα εισόδου δεν είναι διάκριση πεζών-κεφαλαίων.

**Έκδοση 1 (δέσμη)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Έκδοση 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Έξοδος από άποψη ####

**Έκδοση 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Έκδοση 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Έξοδος από κλειδιού φράσεις ####

**Έκδοση 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Έκδοση 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Έξοδος από γλώσσες ####


**Έκδοση 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Έκδοση 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Μέρος 4 β. Ενημερώστε τις μορφές για θέματα ###

#### <a name="endpoints"></a>Τα τελικά σημεία ####

| |Τελικό σημείο έκδοση 1 | Τελικό σημείο έκδοση 2|
|---|---|---|
|Υποβολή για το θέμα ανίχνευση (ΔΗΜΟΣΊΕΥΣΗ)|```StartTopicDetection```|```topics```|
|Λήψη αποτελεσμάτων θέμα (ΛΉΨΗ)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Μορφές εισόδου ####

**Έκδοση 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Έκδοση 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Αποτελέσματα υποβολής ####

**Έκδοση 1 (ΔΗΜΟΣΊΕΥΣΗ)**

Στο παρελθόν, όταν ολοκληρωθεί η εργασία, θα εμφανιστεί το παρακάτω αποτέλεσμα JSON, όπου το jobId θα είναι προσαρτημένο σε μια διεύθυνση URL για τη λήψη το αποτέλεσμα.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Έκδοση 2 (ΔΗΜΟΣΊΕΥΣΗ)**

Την απάντηση τώρα περιλαμβάνει μια τιμή κεφαλίδας ως εξής, όπου `operation-location` χρησιμοποιείται ως το τελικό σημείο για να τις ψηφοφορίας για τα αποτελέσματα:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Αποτελέσματα λειτουργίας ####

**Έκδοση 1 (ΛΉΨΗ)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Έκδοση 2 (ΛΉΨΗ)**

Όπως και πριν, **περιοδικά ψηφοφορία με το αποτέλεσμα** (προτεινόμενο περιόδου είναι κάθε λεπτό) μέχρι το αποτέλεσμα επιστρέφεται. 

Όταν έχει ολοκληρωθεί το API θέματα, μια κατάσταση ανάγνωσης `succeeded` θα επιστραφεί. Στη συνέχεια, αυτό θα περιλαμβάνει τα αποτελέσματα εξόδου με τη μορφή που φαίνεται παρακάτω:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Τμήμα 5. Δοκιμάστε το! ###

Τώρα θα πρέπει να έτοιμοι! Ελέγξτε τον κωδικό με ένα μικρό δείγμα για να βεβαιωθείτε ότι μπορείτε να επεξεργαστείτε με επιτυχία τα δεδομένα σας.

<properties
    pageTitle="Σύνδεση στο DocumentDB αναζήτησης Azure χρησιμοποιώντας Οι δεικτοδότες | Microsoft Azure"
    description="Σε αυτό το άρθρο δείχνει πώς μπορείτε να χρησιμοποιήσετε για να το ευρετήριο αναζήτησης Azure με DocumentDB ως προέλευση δεδομένων."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Σύνδεση στο DocumentDB χρησιμοποιώντας Οι δεικτοδότες αναζήτησης Azure

Εάν αναζητάτε για την υλοποίηση εμπειρίες εξαιρετική αναζήτηση στα δεδομένα σας DocumentDB, χρησιμοποιήστε το ευρετήριο αναζήτησης Azure για DocumentDB! Σε αυτό το άρθρο, θα σας δείξουμε πώς μπορείτε να ενοποιήσετε Azure DocumentDB Azure Αναζήτηση χωρίς να χρειάζεται να γράφετε κώδικα για να διατηρήσετε την υποδομή δημιουργίας ευρετηρίου!

Για να ορίσετε αυτό, πρέπει να [ρύθμισης λογαριασμού Azure αναζήτησης](../search/search-create-service-portal.md) (δεν χρειάζεται να κάνετε αναβάθμιση στην τυπική αναζήτηση), και, στη συνέχεια, να καλέσετε το [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) για να δημιουργήσετε μια DocumentDB **προέλευσης δεδομένων** και ένα **ευρετήριο** για τη συγκεκριμένη προέλευση δεδομένων.

Σε σειρά τις αιτήσεις αποστολή για να αλληλεπιδράσετε με τα ΥΠΌΛΟΙΠΑ API, μπορείτε να χρησιμοποιήσετε [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)ή οποιοδήποτε εργαλείο από τις προτιμήσεις σας.


##<a id="Concepts"></a>Azure έννοιες ευρετήριο αναζήτησης

Azure αναζήτησης υποστηρίζει τη δημιουργία και τη διαχείριση των δεδομένων προέλευσης (συμπεριλαμβανομένων των DocumentDB) και οι δεικτοδότες που λειτουργούν σε σχέση με αυτές τις προελεύσεις δεδομένων.

Μια **προέλευση δεδομένων** καθορίζει ποια δεδομένα πρέπει να είναι στο ευρετήριο, διαπιστευτήρια που θα έχουν πρόσβαση σε δεδομένα και πολιτικές για να ενεργοποιήσετε την αναζήτηση Azure σε αποτελεσματικά τον προσδιορισμό των αλλαγών στα δεδομένα (όπως τροποποιηθεί ή διαγραφεί έγγραφα μέσα από τη συλλογή σας). Το αρχείο προέλευσης δεδομένων ορίζεται ως μια ανεξάρτητη πόρων, ώστε να μπορούν να χρησιμοποιηθούν με πολλούς Οι δεικτοδότες.

Ένα **ευρετήριο** περιγράφει τον τρόπο ροής των δεδομένων από την προέλευση δεδομένων σε ένα ευρετήριο αναζήτησης προορισμού. Θα πρέπει να σκοπεύετε να δημιουργήσετε ένα ευρετήριο για συνδυασμό κάθε προορισμού ευρετηρίου και τα δεδομένα προέλευσης. Ενώ μπορείτε να έχετε πολλούς Οι δεικτοδότες γραφής στο ίδιο ευρετήριο, ένα ευρετήριο μόνο να γράψετε σε ένα ευρετήριο. Χρησιμοποιείται ένα ευρετήριο για:

- Εκτελέστε ένα μοναδικό αντίγραφο των δεδομένων για να συμπληρώσετε ένα ευρετήριο.
- Συγχρονισμός ένα ευρετήριο με τις αλλαγές στο αρχείο προέλευσης δεδομένων σε ένα χρονοδιάγραμμα. Το χρονοδιάγραμμα είναι τμήμα του ορισμού ευρετηρίου.
- Κλήση σε ζήτηση ενημερώσεις για ένα ευρετήριο, όπως απαιτείται.

##<a id="CreateDataSource"></a>Βήμα 1: Δημιουργία μιας προέλευσης δεδομένων

Υποβάλει μια αίτηση HTTP ΔΗΜΟΣΊΕΥΣΗ για να δημιουργήσετε μια νέα προέλευση δεδομένων στην υπηρεσία αναζήτησης Azure, συμπεριλαμβανομένων των κεφαλίδων των παρακάτω αίτηση.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Το `api-version` απαιτείται. Οι έγκυρες τιμές περιλαμβάνουν `2015-02-28` ή νεότερη έκδοση. Επισκεφθείτε [εκδόσεις API στο Azure αναζήτησης](../search/search-api-versions.md) για να δείτε όλες τις υποστηριζόμενες εκδόσεις API αναζήτησης.

Το κυρίως κείμενο της αίτησης περιέχει τον ορισμό προέλευσης δεδομένων, η οποία πρέπει να περιλαμβάνουν τα ακόλουθα πεδία:

- **όνομα**: Επιλέξτε οποιοδήποτε όνομα για να αναπαραστήσετε τη βάση δεδομένων DocumentDB.

- **Τύπος**: χρήση `documentdb`.

- τα **διαπιστευτήρια**:

    - **συμβολοσειρά σύνδεσης**: υποχρεωτικό. Καθορίστε τις πληροφορίες σύνδεσης για τη βάση δεδομένων Azure DocumentDB με την εξής μορφή:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **κοντέινερ**:

    - **όνομα**: υποχρεωτικό. Καθορίστε το αναγνωριστικό της συλλογής DocumentDB να τοποθετηθούν στο ευρετήριο.

    - **ερώτημα**: προαιρετική. Μπορείτε να καθορίσετε ένα ερώτημα για να ισοπέδωση ένα αυθαίρετο JSON έγγραφο σε ένα επίπεδο σχήματος που μπορεί να δημιουργήσετε ευρετήριο αναζήτησης Azure.

- **dataChangeDetectionPolicy**: προαιρετική. Ανατρέξτε στο θέμα [δεδομένα αλλάζουν πολιτική εντοπισμός](#DataChangeDetectionPolicy) παρακάτω.

- **dataDeletionDetectionPolicy**: προαιρετική. Ανατρέξτε στο θέμα [πολιτική εντοπισμού διαγραφή δεδομένων](#DataDeletionDetectionPolicy) παρακάτω.

Δείτε παρακάτω για ένα [παράδειγμα σώμα αίτησης](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Καταγραφή τροποποιημένα έγγραφα

Ο σκοπός της μια πολιτική εντοπισμού αλλαγή δεδομένων είναι να αποτελεσματικά τον προσδιορισμό των στοιχείων τροποποιημένα δεδομένα. Προς το παρόν, η μόνη υποστηριζόμενη πολιτική είναι το `High Water Mark` πολιτική χρησιμοποιώντας το `_ts` ιδιότητα τελευταίας τροποποίησης χρονικής σήμανσης που παρέχεται από DocumentDB - η οποία καθορίζεται ως εξής:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Θα πρέπει επίσης να προσθέσετε `_ts` στην προβολή και `WHERE` όρος για το ερώτημά σας. Για παράδειγμα:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Καταγραφή Διαγραμμένα έγγραφα

Όταν οι γραμμές διαγράφονται από τον πίνακα προέλευσης, θα πρέπει να διαγράψετε αυτές τις γραμμές από το ευρετήριο αναζήτησης. Ο σκοπός της μια πολιτική εντοπισμού διαγραφή δεδομένων είναι να αποτελεσματικά τον προσδιορισμό των στοιχείων διαγραμμένα δεδομένα. Προς το παρόν, η μόνη υποστηριζόμενη πολιτική είναι το `Soft Delete` πολιτικής (διαγραφής έχει επισημανθεί με σημαία κάποια λειτουργία), που έχει καθοριστεί ως εξής:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Θα πρέπει να συμπεριλάβετε την ιδιότητα softDeleteColumnName στον όρο SELECT, εάν χρησιμοποιείτε μια προσαρμοσμένη προβολή.

###<a id="CreateDataSourceExample"></a>Παράδειγμα σώματος αίτησης

Το παρακάτω παράδειγμα δημιουργεί μια προέλευση δεδομένων με ένα προσαρμοσμένο ερώτημα και πολιτικής συμβουλές:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Απόκριση

Εάν το αρχείο προέλευσης δεδομένων που δημιουργήθηκε με επιτυχία, θα λάβετε μια απόκριση HTTP 201 δημιουργήθηκε.

##<a id="CreateIndex"></a>Βήμα 2: Δημιουργία ευρετηρίου

Δημιουργία ευρετηρίου αναζήτησης Azure προορισμού, εάν δεν έχετε ήδη. Μπορείτε να το κάνετε από το [Περιβάλλον εργασίας Χρήστη πύλη Azure](../search/search-create-index-portal.md) ή με τη χρήση του [API δημιουργία ευρετηρίου](https://msdn.microsoft.com/library/azure/dn798941.aspx).

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Βεβαιωθείτε ότι το σχήμα του ευρετηρίου σας προορισμού είναι συμβατά με το σχήμα των εγγράφων JSON προέλευσης ή την έξοδο από το προσαρμοσμένο ερώτημα προβολής.

>[AZURE.NOTE] Για τις συλλογές διαμερίσματα, το κλειδί εγγράφου προεπιλογή είναι του DocumentDB `_rid` ιδιότητας, η οποία λαμβάνει μετονομαστεί σε `rid` στο πλαίσιο Αναζήτηση Azure. Επίσης, DocumentDB του `_rid` τιμές περιέχει χαρακτήρες που δεν είναι έγκυρες σε πλήκτρα Azure αναζήτησης. γι ' αυτό, η `_rid` τιμές είναι κωδικοποίηση Base64.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>Εικόνα A: αντιστοίχιση μεταξύ JSON τύπους δεδομένων και τύποι δεδομένων Azure αναζήτησης

| ΤΎΠΟΣ ΔΕΔΟΜΈΝΩΝ JSON|   ΤΎΠΟΙ ΠΕΔΊΟΥ INDEX ΣΥΜΒΑΤΆ ΠΡΟΟΡΙΣΜΟΎ|
|---|---|
|Bool|Edm.Boolean, Edm.String|
|Αριθμοί που μοιάζουν με ακέραιους αριθμούς|Edm.Int32, Edm.Int64, Edm.String|
|Αριθμοί που να μοιάζει με κινούμενο σημεία|Edm.Double, Edm.String|
|Συμβολοσειρά|Edm.String|
|Οι πίνακες στοιχειώδης τύπων π.χ. "a", "b", "c" |Collection(EDM.String)|
|Συμβολοσειρές που μοιάζουν με ημερομηνίες| Edm.DateTimeOffset, Edm.String|
|GeoJSON αντικείμενα π.χ. {"Τύπος": "Σημείο", "συντεταγμένων": [μεγάλα, lat]} | Edm.GeographyPoint |
|Άλλα αντικείμενα JSON|Δ/Υ|

###<a id="CreateIndexExample"></a>Παράδειγμα σώματος αίτησης

Το παρακάτω παράδειγμα δημιουργεί ένα ευρετήριο με ένα πεδίο αναγνωριστικό και μια περιγραφή:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Απόκριση

Εάν το ευρετήριο που δημιουργήθηκε με επιτυχία, θα λάβετε μια απόκριση HTTP 201 δημιουργήθηκε.

##<a id="CreateIndexer"></a>Βήμα 3: Δημιουργία ενός ευρετηρίου

Μπορείτε να δημιουργήσετε ένα νέο ευρετήριο μέσα σε μια υπηρεσία αναζήτησης Azure χρησιμοποιώντας μια αίτηση HTTP POST με τις παρακάτω κεφαλίδες.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Το κυρίως κείμενο της αίτησης περιέχει τον ορισμό ευρετηρίου, η οποία θα πρέπει να περιλαμβάνει τα παρακάτω πεδία:

- **όνομα**: υποχρεωτικό. Το όνομα του το ευρετήριο.

- **dataSourceName**: υποχρεωτικό. Το όνομα του μια υπάρχουσα προέλευση δεδομένων.

- **targetIndexName**: υποχρεωτικό. Το όνομα του ένα υπάρχον ευρετήριο.

- **Χρονοδιάγραμμα**: προαιρετική. Ανατρέξτε στο θέμα [Δημιουργία ευρετηρίου χρονοδιάγραμμα](#IndexingSchedule) παρακάτω.

###<a id="IndexingSchedule"></a>Εκτέλεση Οι δεικτοδότες σε ένα χρονοδιάγραμμα

Ένα ευρετήριο προαιρετικά να καθορίσετε ένα χρονοδιάγραμμα. Εάν υπάρχει ένα χρονοδιάγραμμα, το ευρετήριο θα εκτελεστεί περιοδικά σύμφωνα με το χρονοδιάγραμμα. Χρονοδιάγραμμα περιλαμβάνει τα παρακάτω χαρακτηριστικά:

- **διάστημα**: υποχρεωτικό. Εκτελεί μια τιμή διάρκειας που καθορίζει ένα διάστημα ή την περίοδο για ευρετήριο. Το διάστημα μικρότερος επιτρεπόμενος είναι 5 λεπτά. μεγαλύτερου είναι μία ημέρα. Αυτό πρέπει να μορφοποιηθούν ως μια τιμή "dayTimeDuration" XSD (περιορισμένα υποσύνολο του μια τιμή [διάρκειας ISO 8601](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Το μοτίβο για αυτό είναι: `P(nD)(T(nH)(nM))`. Παραδείγματα: `PT15M` για κάθε 15 λεπτά, `PT2H` για κάθε 2 ώρες.

- **ώρα έναρξης**: υποχρεωτικό. Μια ημερομηνία/ώρα UTC που καθορίζει πότε θα πρέπει να ξεκινήσει το ευρετήριο εκτελείται.

###<a id="CreateIndexerExample"></a>Παράδειγμα σώματος αίτησης

Το ακόλουθο παράδειγμα δημιουργεί ένα ευρετήριο που αντιγράφει δεδομένα από τη συλλογή στα οποία γίνεται αναφορά από το `myDocDbDataSource` αρχείου προέλευσης δεδομένων για να το `mySearchIndex` ευρετήριο σε ένα χρονοδιάγραμμα που ξεκινά την 1η Ιανουαρίου 2015 UTC και εκτελεί ανά ώρα.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Απόκριση

Εάν το ευρετήριο που δημιουργήθηκε με επιτυχία, θα λάβετε μια απόκριση HTTP 201 δημιουργήθηκε.

##<a id="RunIndexer"></a>Βήμα 4: Εκτέλεση ενός ευρετηρίου

Εκτός από την εκτέλεση περιοδικά σε ένα χρονοδιάγραμμα, ένα ευρετήριο μπορεί επίσης να ενεργοποιηθεί στην ζήτηση με την έκδοση την ακόλουθη αίτηση HTTP POST:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Απόκριση

Εάν το ευρετήριο έγινε κλήση με επιτυχία, θα λάβετε μια απόκριση HTTP 202 αποδεκτές.

##<a name="GetIndexerStatus"></a>Βήμα 5: Λήψη κατάσταση ευρετηρίου

Μπορείτε να εκδώσετε μια αίτηση HTTP GET για την ανάκτηση του τρέχουσα κατάσταση και την εκτέλεση ιστορικού ένα ευρετήριο:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Απόκριση

Θα δείτε μια απόκριση HTTP 200 OK επιστρέφονται μαζί με μια απόκριση σώμα που περιέχει πληροφορίες σχετικά με τη γενική κατάσταση εύρυθμης λειτουργίας ευρετηρίου, το τελευταίο Ενεργοποίηση ευρετηρίου, καθώς και το ιστορικό των πρόσφατα κλήσεων ευρετηρίου (εάν υπάρχει).

Η απόκριση θα πρέπει να μοιάζει με το εξής:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Ιστορικό εκτέλεσης περιέχει μέχρι το 50 πιο πρόσφατες ολοκληρωμένη εκτελέσεις, που ταξινομούνται με αντίστροφη χρονολογική σειρά (, ώστε να την πιο πρόσφατη εκτέλεση βρεθεί πρώτο στην απάντηση).

##<a name="NextSteps"></a>Επόμενα βήματα

Συγχαρητήρια! Μάθατε ακριβώς με τον τρόπο ενσωμάτωσης Azure DocumentDB Azure αναζήτησης με χρήση του ευρετηρίου για DocumentDB.

 - Για να μάθετε πώς πιο Azure DocumentDB, ανατρέξτε στη [σελίδα της υπηρεσίας DocumentDB](https://azure.microsoft.com/services/documentdb/).

 - Για να μάθετε περισσότερα σχετικά με την αναζήτηση Azure τρόπος, ανατρέξτε στη [σελίδα της υπηρεσίας αναζήτησης](https://azure.microsoft.com/services/search/).

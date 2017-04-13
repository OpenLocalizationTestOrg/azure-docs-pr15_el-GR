<properties
pageTitle="Δημιουργία ευρετηρίου αντικείμενα BLOB JSON με ευρετήριο αντικειμένων blob του Azure αναζήτησης"
description="Δημιουργία ευρετηρίου αντικείμενα BLOB JSON με ευρετήριο αντικειμένων blob του Azure αναζήτησης"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Δημιουργία ευρετηρίου αντικείμενα BLOB JSON με ευρετήριο αντικειμένων blob του Azure αναζήτησης 

Σε αυτό το άρθρο παρουσιάζει τον τρόπο ρύθμισης παραμέτρων αναζήτησης Azure blob ευρετηρίου για να εξαγάγετε δομημένο περιεχόμενο από αντικείμενα BLOB που περιλαμβάνουν JSON.

## <a name="scenarios"></a>Σενάρια

Από προεπιλογή, το [ευρετήριο αναζήτησης Azure blob](search-howto-indexing-azure-blob-storage.md) αναλύει αντικείμενα BLOB JSON ως ένα μεμονωμένο μπλοκ κειμένου. Συχνά, θέλετε διατήρηση της δομής των εγγράφων σας JSON. Για παράδειγμα, δίνεται στο έγγραφο JSON 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

ενδέχεται να θέλετε να την ανάλυση σε ένα έγγραφο του Azure αναζήτησης με "κείμενο", "datePublished" και "ετικέτες" πεδία.

Εναλλακτικά, όταν σας αντικείμενα BLOB περιέχουν έναν **πίνακα με αντικείμενα JSON**, ίσως θέλετε κάθε στοιχείο του πίνακα για να γίνετε σε χωριστό έγγραφο Azure αναζήτησης. Για παράδειγμα, δίνεται ένα blob με αυτό JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Μπορείτε να συμπληρώσετε το ευρετήριο αναζήτησης Azure με 3 ξεχωριστή έγγραφα, κάθε μία με τα πεδία "αναγνωριστικό" και "κείμενο". 

> [AZURE.IMPORTANT] Αυτή η λειτουργία είναι αυτήν τη στιγμή στην προεπισκόπηση. Είναι διαθέσιμη μόνο σε το REST API χρησιμοποιώντας την έκδοση **2015-02-28-Preview**. Πρέπει να θυμάστε, προεπισκόπηση API προορίζονται για τον έλεγχο και αξιολόγησης και δεν πρέπει να χρησιμοποιούνται σε περιβάλλοντα παραγωγής. 

## <a name="setting-up-json-indexing"></a>Ρύθμιση JSON δημιουργίας ευρετηρίου

Για να δημιουργήσετε ευρετήριο αντικείμενα BLOB JSON, ορίστε το `parsingMode` η τιμή της παραμέτρου για να `json` (για να δημιουργήσετε ευρετήριο κάθε blob ως ένα μεμονωμένο έγγραφο) ή `jsonArray` (Εάν σας αντικείμενα BLOB περιέχει έναν πίνακα JSON): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Εάν είναι απαραίτητο, χρησιμοποιήστε **αντιστοιχίσεις πεδίων** για να επιλέξετε τις ιδιότητες του εγγράφου JSON προέλευσης που χρησιμοποιείται για τη συμπλήρωση του ευρετηρίου αναζήτησης προορισμού.  Αυτή η διαδικασία περιγράφεται αναλυτικά παρακάτω. 

> [AZURE.IMPORTANT] Όταν χρησιμοποιείτε `json` ή `jsonArray` ανάλυση λειτουργία, αναζήτηση Azure προϋποθέτει ότι θα όλων των blob στο αρχείο προέλευσης δεδομένων JSON. Εάν χρειάζεστε για την υποστήριξη συνδυασμό JSON και μη JSON αντικείμενα blob στο ίδιο αρχείο προέλευσης δεδομένων, ενημερώστε μας στην [τοποθεσία του UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Χρήση αντιστοιχίσεων πεδίων για να δημιουργήσετε την αναζήτηση εγγράφων 

Προς το παρόν, Azure αναζήτησης να δημιουργήσετε ευρετήριο αυθαίρετο JSON έγγραφα απευθείας, καθώς υποστηρίζει μόνο προκαταρκτικούς τύπους δεδομένων, συμβολοσειρά πίνακες και GeoJSON σημεία. Ωστόσο, μπορείτε να χρησιμοποιήσετε **αντιστοιχίσεις πεδίων** , επιλέξτε τμήματα του εγγράφου σας JSON και "απομάκρυνση" τους σε ανώτατου επιπέδου πεδία του εγγράφου αναζήτησης. Για να μάθετε περισσότερα σχετικά με τα βασικά στοιχεία του πεδίου αντιστοιχίσεις, ανατρέξτε στο θέμα [αντιστοιχίσεις πεδίων ευρετήριο αναζήτησης Azure φέρουμε κοντά τις διαφορές μεταξύ των προελεύσεων δεδομένων και των ευρετηρίων αναζήτησης](search-indexer-field-mappings.md).

Επιστρέψουν στο έγγραφό μας JSON παράδειγμα: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Ας υποθέσουμε ότι έχετε ένα ευρετήριο αναζήτησης με τα εξής πεδία: `text` του τύπου Edm.String, `date` του τύπου Edm.DateTimeOffset, και `tags` του τύπου Collection(Edm.String). Για να αντιστοιχίσετε σας JSON στο επιθυμητό σχήμα, χρησιμοποιήστε τις παρακάτω αντιστοιχίσεις πεδίο: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Τα ονόματα των πεδίων προέλευσης στην αντιστοιχίσεις καθορίζονται χρησιμοποιώντας τη σημειογραφία [JSON δείκτη](http://tools.ietf.org/html/rfc6901) . Ξεκινήστε με μια κάθετο παραπέμπει στη ρίζα του εγγράφου σας JSON και κατόπιν κάντε Διερεύνηση στα την επιθυμητή ιδιότητα (επίπεδο αυθαίρετο ένθεσης) χρησιμοποιώντας διαδρομή διαχωρισμένο με κάθετο προς τα εμπρός. 

Μπορείτε επίσης να αναφερθείτε σε μεμονωμένες πίνακα στοιχεία, χρησιμοποιώντας έναν δείκτη. Για παράδειγμα, για να επιλέξετε το πρώτο στοιχείο του πίνακα "ετικέτες" από το παραπάνω παράδειγμα, χρησιμοποιήστε μια αντιστοίχιση πεδίων ως εξής:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Εάν ένα όνομα πεδίου προέλευσης σε μια διαδρομή αντιστοίχισης πεδίων αναφέρεται σε μια ιδιότητα που δεν υπάρχουν στο JSON, αυτήν την αντιστοίχιση παραλείπεται χωρίς σφάλμα. Αυτό γίνεται, έτσι ώστε να υποστηρίζουμε έγγραφα με μια διαφορετική διάταξη (που είναι μια υπόθεση κοινής χρήσης). Επειδή δεν υπάρχει καμία επικύρωση, πρέπει να αναλάβουν την για να αποφύγετε τυπογραφικά λάθη σε σας προδιαγραφή αντιστοίχισης πεδίων. 

Εάν τα έγγραφά σας JSON περιέχει μόνο απλό ιδιότητες ανώτατου επιπέδου, ίσως δεν χρειάζεται αντιστοιχίσεις πεδίων σε όλα. Για παράδειγμα, εάν σας JSON μοιάζει κάπως έτσι, το ανώτατου επιπέδου Ιδιότητες "κείμενο", "datePublished" και "ετικέτες" θα απευθείας αντιστοιχίζονται τα αντίστοιχα πεδία στο ευρετήριο αναζήτησης: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Δημιουργία ευρετηρίου ένθετους πίνακες JSON

Τι γίνεται εάν θέλετε να δημιουργήσετε ευρετήριο ενός πίνακα JSON αντικειμένων, αλλά αυτό πίνακα είναι ένθετο κάπου μέσα στο έγγραφο; Μπορείτε να επιλέξετε ποια ιδιότητα περιέχει του πίνακα με χρήση του `documentRoot` ιδιότητα παραμέτρων. Για παράδειγμα, εάν σας αντικείμενα BLOB μοιάζει κάπως έτσι: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

Χρησιμοποιήστε αυτήν τη ρύθμιση παραμέτρων για να δημιουργήσετε ευρετήριο τον πίνακα που περιέχεται στην ιδιότητα "level2": 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Παραδείγματα αίτηση

Τοποθέτηση όλα μαζί, εδώ θα βρείτε τα παραδείγματα ολοκλήρωσης ωφέλιμα φορτία. 

Αρχείο προέλευσης δεδομένων: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Ευρετήριο:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Βοηθήστε μας να βελτιώσουμε το Azure αναζήτησης

Εάν έχετε αιτήσεις για δυνατότητες ή ιδέες για βελτιώσεις, επικοινωνήστε επικοινωνία μας μας [UserVoice τοποθεσίας](https://feedback.azure.com/forums/263029-azure-search/).
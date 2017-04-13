<properties
pageTitle="Δημιουργία ευρετηρίου αντικείμενα BLOB CSV με ευρετήριο αναζήτησης Azure blob | Microsoft Azure"
description="Μάθετε πώς μπορείτε να δημιουργήσετε ευρετήριο αντικείμενα BLOB CSV με αναζήτηση Azure"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Δημιουργία ευρετηρίου αντικείμενα BLOB CSV με ευρετήριο αντικειμένων blob του Azure αναζήτησης 

Από προεπιλογή, αναλύει [ευρετήριο αντικειμένων blob του Azure αναζήτησης](search-howto-indexing-azure-blob-storage.md) με κόμματα αντικείμενα BLOB κειμένου ως ένα μεμονωμένο μπλοκ κειμένου. Ωστόσο, με αντικείμενα BLOB που περιέχει τα δεδομένα CSV, συχνά θέλετε να χειριστείτε κάθε γραμμή στο αντικείμενο blob ως νέο έγγραφο. Για παράδειγμα, δίνεται το ακόλουθο κείμενο οριοθετημένο: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Ίσως θέλετε να την ανάλυση σε 2 εγγράφων, κάθε μία που περιέχει "αναγνωριστικό", "datePublished" και "ετικέτες" πεδία.

Σε αυτό το άρθρο θα μάθετε πώς ανάλυση αντικείμενα BLOB CSV με ευρετήριο blob Azure αναζήτησης. 

> [AZURE.IMPORTANT] Αυτή η λειτουργία είναι αυτήν τη στιγμή στην προεπισκόπηση. Είναι διαθέσιμη μόνο σε το REST API χρησιμοποιώντας την έκδοση **2015-02-28-Preview**. Πρέπει να θυμάστε, προεπισκόπηση API προορίζονται για τον έλεγχο και αξιολόγησης και δεν πρέπει να χρησιμοποιούνται σε περιβάλλοντα παραγωγής. 

## <a name="setting-up-csv-indexing"></a>Για τη ρύθμιση του ευρετηρίου CSV

Για να δημιουργήσετε ευρετήριο αντικείμενα BLOB CSV, να δημιουργήσετε ή να ενημερώσετε ένα ευρετήριο ορισμό με το `delimitedText` ανάλυση λειτουργία:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Για περισσότερες λεπτομέρειες σχετικά με το API ευρετηρίου δημιουργία, ανατρέξτε στο θέμα [Δημιουργία ευρετηρίου](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`υποδεικνύει ότι η πρώτη γραμμή (μη κενές) του κάθε blob περιέχει κεφαλίδες.
Εάν αντικείμενα BLOB δεν περιέχει μια γραμμή κεφαλίδας αρχικό, τις κεφαλίδες θα πρέπει να έχει καθοριστεί στη ρύθμιση παραμέτρων ευρετήριο: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Προς το παρόν, μόνο η κωδικοποίηση UTF-8 υποστηρίζεται. Επίσης, μόνο το κόμμα `','` χαρακτήρα υποστηρίζεται ως οριοθέτης. Εάν χρειάζεστε υποστήριξη για άλλες κωδικοποιήσεις ή οριοθέτες, ενημερώστε μας στην [τοποθεσία του UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Όταν χρησιμοποιείτε το κειμένου οριοθετημένου με ανάλυση λειτουργία, αναζήτηση Azure προϋποθέτει ότι όλα τα αντικείμενα blob στο αρχείο προέλευσης δεδομένων θα είναι CSV. Εάν χρειάζεστε για την υποστήριξη συνδυασμό CSV και μη-CSV αντικείμενα blob στο ίδιο αρχείο προέλευσης δεδομένων, ενημερώστε μας στην [τοποθεσία του UserVoice](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Παραδείγματα αίτηση

Τοποθέτηση όλα μαζί, εδώ θα βρείτε τα παραδείγματα φορτίο ολοκλήρωσης. 

Αρχείο προέλευσης δεδομένων: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Ευρετήριο:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Βοηθήστε μας να βελτιώσουμε το Azure αναζήτησης

Εάν έχετε αιτήσεις για δυνατότητες ή ιδέες για βελτιώσεις, επικοινωνήστε επικοινωνία μας μας [UserVoice τοποθεσίας](https://feedback.azure.com/forums/263029-azure-search/).
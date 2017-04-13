<properties
pageTitle="Αντιστοιχίσεις πεδίων στο Οι δεικτοδότες Azure αναζήτησης"
description="Ρύθμιση παραμέτρων αναζήτησης Azure ευρετηρίου αντιστοιχίσεις πεδίων με λογαριασμού για διαφορές σε ονόματα πεδίων και οι παραστάσεις δεδομένων"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Αντιστοιχίσεις πεδίων στο Οι δεικτοδότες Azure αναζήτησης

Όταν χρησιμοποιείτε Οι δεικτοδότες Azure αναζήτησης, μπορείτε να τον εαυτό σας βρείτε περιστασιακά στις περιπτώσεις όπου τα δεδομένα εισόδου σας δεν απολύτως συμφωνεί με το σχήμα του ευρετηρίου σας προορισμού. Σε αυτές τις περιπτώσεις, μπορείτε να χρησιμοποιήσετε **αντιστοιχίσεις πεδίων** για να μετατρέψετε τα δεδομένα σας σε το επιθυμητό σχήμα. 

Ορισμένες περιπτώσεις, όπου το πεδίο αντιστοιχίσεων χρήσιμες:
 
- Προέλευση δεδομένων σας έχει ένα πεδίο `_id`, αλλά δεν επιτρέπει την αναζήτηση Azure πεδίο τα ονόματα ξεκινούν με χαρακτήρα υπογράμμισης. Πεδίο αντιστοίχισης σάς επιτρέπει να "Μετονομασία" ένα πεδίο. 
- Θέλετε να συμπληρώσετε πολλά πεδία στο ευρετήριο με τα ίδια δεδομένα προέλευσης δεδομένων, για παράδειγμα, επειδή θέλετε να εφαρμόσετε διαφορετικό αναλυτές σε αυτά τα πεδία. Αντιστοιχίσεις πεδίων σάς επιτρέπουν να "διαχωρίζονται" ένα πεδίο προέλευσης δεδομένων.
- Πρέπει να Base64 κωδικοποιείτε ή αποκωδικοποιείτε τα δεδομένα σας. Αντιστοιχίσεις πεδίων υποστηρίζει πολλές **αντιστοίχισης συναρτήσεις**, όπως συναρτήσεις για Base64 κωδικοποίησης και αποκωδικοποίησης.   


> [AZURE.IMPORTANT] Προς το παρόν, η λειτουργικότητα αντιστοιχίσεις πεδίων είναι σε προεπισκόπηση. Είναι διαθέσιμη μόνο σε το REST API χρησιμοποιώντας την έκδοση **2015-02-28-Preview**. Πρέπει να θυμάστε, προεπισκόπηση API προορίζονται για τον έλεγχο και αξιολόγησης και δεν πρέπει να χρησιμοποιούνται σε περιβάλλοντα παραγωγής.

## <a name="setting-up-field-mappings"></a>Ρύθμιση αντιστοιχίσεων πεδίων

Μπορείτε να προσθέσετε αντιστοιχίσεις πεδίων κατά τη δημιουργία ενός νέου ευρετηρίου χρησιμοποιώντας τη [Δημιουργία ευρετηρίου](search-api-indexers-2015-02-28-preview.md#create-indexer) API. Μπορείτε να διαχειριστείτε αντιστοιχίσεις πεδίων σε ένα ευρετήριο δημιουργίας ευρετηρίου χρησιμοποιώντας την [Ενημέρωση ευρετηρίου](search-api-indexers-2015-02-28-preview.md#update-indexer) API. 

Πεδίο αντιστοίχισης αποτελείται από τρία μέρη: 

1. A `sourceFieldName`, που αντιπροσωπεύει ένα πεδίο στο αρχείο προέλευσης δεδομένων. Αυτή η ιδιότητα απαιτείται. 

2. Μια προαιρετική `targetFieldName`, που αντιπροσωπεύει ένα πεδίο στο ευρετήριο αναζήτησης. Εάν παραλειφθεί το όρισμα, χρησιμοποιείται το ίδιο όνομα όπως το αρχείο προέλευσης δεδομένων. 

3. Μια προαιρετική `mappingFunction`, που μπορούν να μετασχηματισμός των δεδομένων σας χρησιμοποιώντας μία από τις πολλές προκαθορισμένες συναρτήσεις. Την πλήρη λίστα των συναρτήσεων είναι [κάτω από το στοιχείο](#mappingFunctions).

Προστίθενται πεδία αντιστοιχίσεων το `fieldMappings` πίνακα για τον ορισμό του ευρετηρίου. 

Για παράδειγμα, ακολουθεί πώς μπορείτε να διευθετηθούν οι διαφορές σε ονόματα πεδίων: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Ένα ευρετήριο μπορεί να έχει πολλές αντιστοιχίσεις πεδίων. Για παράδειγμα, μάθετε πώς μπορείτε να "διαχωρίζονται" ένα πεδίο:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Η αναζήτηση Azure χρησιμοποιεί διάκριση πεζών-κεφαλαίων σύγκρισης για να επιλύσετε τα ονόματα πεδίων και συνάρτηση στη αντιστοιχίσεις πεδίων. Αυτό είναι χρήσιμο (δεν χρειάζεται να του σωστού όλα το περίβλημα), αλλά αυτό σημαίνει ότι το αρχείο προέλευσης δεδομένων ή ευρετήριο δεν μπορεί να περιέχει τα πεδία που διαφέρουν μόνο από πεζών-κεφαλαίων.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Συναρτήσεις αντιστοίχισης πεδίων

Αυτές οι συναρτήσεις υποστηρίζονται αυτήν τη στιγμή: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Εκτελεί κωδικοποίηση Base64 *ασφαλείς διεύθυνση URL* της συμβολοσειράς εισαγωγής. Προϋποθέτει ότι τα δεδομένα εισόδου είναι κωδικοποίηση UTF-8. 

#### <a name="sample-use-case"></a>Δείγμα περίπτωσης χρήσης 

Μόνο ασφαλείς διεύθυνση URL χαρακτήρες μπορούν να εμφανίζονται σε ένα κλειδί αναζήτησης Azure εγγράφου (επειδή οι πελάτες πρέπει να μπορούν να διεύθυνση στο έγγραφο με χρήση του API αναζήτησης, για παράδειγμα). Εάν τα δεδομένα σας περιέχουν μη ασφαλείς διεύθυνση URL χαρακτήρες και θέλετε να χρησιμοποιήσετε για να συμπληρώσετε ένα πεδίο κλειδιού του ευρετηρίου αναζήτησης, χρησιμοποιήστε αυτήν τη συνάρτηση.   

#### <a name="example"></a>Παράδειγμα 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Εκτελεί Base64 αποκωδικοποίηση συμβολοσειρά εισόδου. Η είσοδος λαμβάνεται μια *διεύθυνση URL ασφαλείς* Base64 κωδικοποιημένη συμβολοσειρά. 

#### <a name="sample-use-case"></a>Δείγμα περίπτωσης χρήσης 

Τιμές BLOB προσαρμοσμένου μετα-δεδομένων πρέπει να είναι κωδικοποίηση ASCII. Μπορείτε να χρησιμοποιήσετε την κωδικοποίηση Base64 για να αναπαραστήσετε αυθαίρετο συμβολοσειρές Unicode στο blob προσαρμοσμένου μετα-δεδομένων. Ωστόσο, για να κάνετε αναζήτηση χαρακτηριστικό, μπορείτε να χρησιμοποιήσετε αυτήν τη συνάρτηση για να ενεργοποιήσετε ξανά τα κωδικοποιημένα δεδομένα σε συμβολοσειρές "Κανονική", όταν συμπληρώνετε το ευρετήριο αναζήτησης.  

#### <a name="example"></a>Παράδειγμα 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Διαιρεί ένα πεδίο συμβολοσειράς χρησιμοποιώντας το καθορισμένο οριοθέτη και επιλέγει το διακριτικό στην καθορισμένη θέση στο παράθυρο που προκύπτει διαίρεση.

Για παράδειγμα, εάν τα δεδομένα εισόδου είναι `Jane Doe`, η `delimiter` είναι `" "`(κενό διάστημα) και το `position` είναι 0, το αποτέλεσμα είναι `Jane`; Εάν το `position` είναι 1, το αποτέλεσμα είναι `Doe`. Εάν η θέση που αναφέρεται σε ένα διακριτικό που δεν υπάρχει, θα επιστραφεί ένα σφάλμα.

#### <a name="sample-use-case"></a>Δείγμα περίπτωσης χρήσης 

Το αρχείο προέλευσης δεδομένων περιέχει ένα `PersonName` πεδίο και θέλετε να δημιουργήσετε το ευρετήριο ως δύο ξεχωριστές `FirstName` και `LastName` πεδίων. Μπορείτε να χρησιμοποιήσετε αυτήν τη συνάρτηση για να διαιρέσετε το είσοδο με χρήση του χαρακτήρα κενού διαστήματος ως οριοθέτης.

#### <a name="parameters"></a>Παράμετροι

- `delimiter`: μια συμβολοσειρά που θα χρησιμοποιηθεί ως το διαχωριστικό όταν η διαίρεση συμβολοσειρά εισόδου.
- `position`: μια θέση ως βάση το μηδέν ακέραιο του διακριτικού για να επιλέξετε μετά τη διαίρεση συμβολοσειρά εισόδου.    

#### <a name="example"></a>Παράδειγμα

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Μετατρέπει μια συμβολοσειρά μορφοποίηση ως πίνακα JSON συμβολοσειρών σε μια συμβολοσειρά πίνακα που μπορούν να χρησιμοποιηθούν για τη συμπλήρωση μιας `Collection(Edm.String)` πεδίου στο ευρετήριο. 

Για παράδειγμα, εάν η συμβολοσειρά εισόδου είναι `["red", "white", "blue"]`, στη συνέχεια, το πεδίο προορισμού του τύπου `Collection(Edm.String)` θα συμπληρωθεί με τις τρεις τιμές `red`, `white` και `blue`. Για τιμές εισαγωγής που δεν μπορούν να αναλυθούν ως πίνακες συμβολοσειρά JSON, θα επιστραφεί ένα σφάλμα. 

#### <a name="sample-use-case"></a>Δείγμα περίπτωσης χρήσης

Βάση δεδομένων SQL του Azure δεν έχει τύπο δεδομένων ενσωματωμένη ομαλά αντιστοιχίζει `Collection(Edm.String)` πεδία στο Azure αναζήτησης. Για να συμπληρώσετε πεδία συλλογής συμβολοσειρά, μορφοποιήστε τα δεδομένα προέλευσης ως πίνακας συμβολοσειρά JSON και χρησιμοποιήστε αυτήν τη συνάρτηση. 

#### <a name="example"></a>Παράδειγμα 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Βοηθήστε μας να βελτιώσουμε το Azure αναζήτησης

Εάν έχετε αιτήσεις για δυνατότητες ή ιδέες για βελτιώσεις, επικοινωνήστε επικοινωνία μας μας [UserVoice τοποθεσίας](https://feedback.azure.com/forums/263029-azure-search/).
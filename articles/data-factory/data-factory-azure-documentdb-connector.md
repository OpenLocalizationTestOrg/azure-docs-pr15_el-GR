<properties 
    pageTitle="Μετακίνηση δεδομένων προς/από DocumentDB | Microsoft Azure" 
    description="Μάθετε πώς να μετακινήσω δεδομένων προς/από τη συλλογή Azure DocumentDB χρησιμοποιώντας εργοστασίου δεδομένων Azure" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Μετακίνηση δεδομένων από και προς DocumentDB χρησιμοποιώντας εργοστασίου δεδομένων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων σε Azure DocumentDB από δεδομένα άλλου αποθήκευση και μετακίνηση δεδομένων από DocumentDB σε άλλο χώρο αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται.

Στα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να αντιγράψετε δεδομένα προς και από το Azure DocumentDB και χώρο αποθήκευσης Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** από οποιαδήποτε από τις προελεύσεις σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  

> [AZURE.NOTE] Αντιγραφή δεδομένων από τα δεδομένα IaaS στην-εσωτερικής εγκατάστασης/Azure αποθηκεύει σε Azure DocumentDB και αντίστροφα υποστηρίζονται με την πύλη διαχείρισης δεδομένων έκδοση 2.1 και παραπάνω.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από DocumentDB αντικειμένων Blob του Azure

Το παρακάτω παράδειγμα εμφανίζει:

1. Ένα συνδεδεμένο υπηρεσία του τύπου [DocumentDb](#azure-documentdb-linked-service-properties).
2. Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα στο Azure DocumentDB αντικειμένων Blob του Azure. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα.

**Azure DocumentDB συνδεδεμένα υπηρεσίας:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Χώρο αποθήκευσης Blob του Azure συνδεδεμένα υπηρεσίας:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DB εγγράφου εισαγωγής συνόλου δεδομένων:**

Το δείγμα προϋποθέτει ότι έχετε μια συλλογή με το όνομα **ατόμου** σε μια βάση δεδομένων της Azure DocumentDB.
 
Ρύθμιση "εξωτερική": "true" και καθορίζοντας externalData πληροφορίες για την πολιτική της υπηρεσίας εργοστασίου δεδομένων Azure ότι ο πίνακας είναι εξωτερική την προέλευση δεδομένων και δεν δημιουργήθηκαν με μια δραστηριότητα στο την προέλευση δεδομένων.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Αντικειμένων Blob του Azure εξόδου συνόλου δεδομένων:**

Δεδομένα αντιγράφονται σε ένα νέο blob κάθε ώρα με τη διαδρομή για το αντικείμενο blob αναλογιστείτε με το συγκεκριμένο ημερομηνίας/ώρας με ώρα υποδιαίρεση.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Δείγμα εγγράφου JSON στη συλλογή άτομο σε μια βάση δεδομένων DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB υποστηρίζει κατά την αναζήτηση εγγράφων χρησιμοποιώντας μια SQL όπως σύνταξη μέσω ιεραρχικών JSON έγγραφα. 

Παράδειγμα: ΕΠΙΛΈΞΤΕ Person.Name.Middle Person.PersonId, Person.Name.First ΩΣ όνομα, ως MiddleName, Person.Name.Last AS "Επώνυμο" από το άτομο

Η παρακάτω διαδικασία αντιγράφει δεδομένα από τη συλλογή άτομο στη βάση δεδομένων DocumentDB μια αντικειμένων blob του Azure. Ως μέρος της δραστηριότητας αντίγραφο της εισόδου και εξόδου έχουν καθοριστεί συνόλων δεδομένων.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Δείγμα: Αντιγράψτε δεδομένα από το Azure Blob Azure DocumentDB

Το παρακάτω παράδειγμα εμφανίζει:

1. Ένα συνδεδεμένο υπηρεσία του τύπου [DocumentDb](#azure-documentdb-linked-service-properties).
2. Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) και [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Το δείγμα αντιγράφει δεδομένα από το Azure blob να Azure DocumentDB. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα.

**Χώρο αποθήκευσης Blob του Azure συνδεδεμένα υπηρεσίας:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB συνδεδεμένα υπηρεσίας:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob εισαγωγής συνόλου δεδομένων:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB εξόδου συνόλου δεδομένων:**

Το δείγμα αντιγράφει δεδομένα σε μια συλλογή με το όνομα "Άτομο".

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Η παρακάτω διαδικασία αντιγράφει δεδομένα από αντικειμένων Blob του Azure στη συλλογή DocumentDB το άτομο. Ως μέρος της δραστηριότητας αντίγραφο της εισόδου και εξόδου έχουν καθοριστεί συνόλων δεδομένων. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Εάν είναι η εισαγωγή αντικειμένων blob δείγματος ως 

    1,John,,Doe

Στη συνέχεια, το αποτέλεσμα του JSON στο DocumentDB θα ως:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB είναι μια NoSQL την αποθήκευση εγγράφων JSON, όπου επιτρέπονται ένθετες δομές. Azure εργοστασίου δεδομένων επιτρέπει στον χρήστη να καταδείξετε ιεραρχία μέσω **nestingSeparator**, δηλαδή "." σε αυτό το παράδειγμα. Με το διαχωριστικό, τη δραστηριότητα αντίγραφο θα δημιουργήσει το αντικείμενο "Όνομα" με τρεις θυγατρικά στοιχεία πρώτα, μεσαίο και την τελευταία, ανάλογα με το "Name.First", "Name.Middle" και "Name.Last" στον ορισμό του πίνακα.

## <a name="azure-documentdb-linked-service-properties"></a>Azure ιδιοτήτων DocumentDB συνδεδεμένων υπηρεσίας

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία DocumentDB Azure συνδεδεμένες JSON στοιχεία. 

| **Ιδιότητα** | **Περιγραφή** | **Απαιτείται** |
| -------- | ----------- | --------- |
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **DocumentDb** | Ναι |
| συμβολοσειρά σύνδεσης | Καθορίστε πληροφορίες που απαιτούνται για σύνδεση με βάση δεδομένων Azure DocumentDB. | Ναι |

## <a name="azure-documentdb-dataset-type-properties"></a>Ιδιότητες τύπου Azure DocumentDB συνόλου δεδομένων

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).
 
Στην ενότητα typeProperties είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **DocumentDbCollection** περιλαμβάνει τις ακόλουθες ιδιότητες.

| **Ιδιότητα** | **Περιγραφή** | **Απαιτείται** |
| -------- | ----------- | -------- |
| collectionName | Το όνομα της συλλογής DocumentDB εγγράφου. | Ναι |


Παράδειγμα:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Σχήμα από προέλευση δεδομένων
Για δεδομένα σχήματος χωρίς καταστήματα όπως DocumentDB, την υπηρεσία εργοστασίου δεδομένων παράγει το σχήμα με έναν από τους εξής τρόπους:  

1.  Εάν καθορίσετε τη δομή των δεδομένων, χρησιμοποιώντας την ιδιότητα **δομή** στον ορισμό του συνόλου δεδομένων, η υπηρεσία εργοστασίου δεδομένων τηρεί αυτήν τη δομή ως το σχήμα. Σε αυτήν την περίπτωση, εάν μια γραμμή δεν περιέχει μια τιμή για μια στήλη, τιμή null θα παρέχονται για αυτήν.
2.  Εάν δεν καθορίσετε τη δομή των δεδομένων, χρησιμοποιώντας την ιδιότητα **δομή** στον ορισμό του συνόλου δεδομένων, η υπηρεσία εργοστασίου δεδομένων παράγει το σχήμα, χρησιμοποιώντας την πρώτη γραμμή στα δεδομένα. Σε αυτήν την περίπτωση, εάν η πρώτη γραμμή δεν περιέχει το πλήρες σχήμα, ορισμένες στήλες θα λείπουν από το αποτέλεσμα της λειτουργίας αντιγραφής.

Επομένως, για τις προελεύσεις δεδομένων χωρίς σχήματος, είναι η βέλτιστη πρακτική για να καθορίσετε τη δομή των δεδομένων χρησιμοποιώντας την ιδιότητα **δομή** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure ιδιότητες τύπου δραστηριότητας αντίγραφο DocumentDB

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και πολιτικής είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων.
 
**Σημείωση:** Η δραστηριότητα αντίγραφο λαμβάνει μία μόνο είσοδο και παράγει μόνο ένα αποτέλεσμα.

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας και σε περίπτωση δραστηριότητας αντίγραφο ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Σε περίπτωση δραστηριότητας αντίγραφο όταν προέλευσης είναι τύπου **DocumentDbCollectionSource** τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα **typeProperties** :

| **Ιδιότητα** | **Περιγραφή** | **Επιτρεπόμενη τιμή** | **Απαιτείται** |
| ------------ | --------------- | ------------------ | ------------ |
| ερώτημα | Καθορίστε το ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά που υποστηρίζονται από το DocumentDB ερωτήματος. <br/><br/>Παράδειγμα:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Όχι <br/><br/>Εάν δεν καθορίζονται, την πρόταση SQL που εκτελείται:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Ειδικός χαρακτήρας για να υποδείξει ότι το έγγραφο είναι ένθετο | Οποιονδήποτε χαρακτήρα. <br/><br/>DocumentDB είναι μια NoSQL την αποθήκευση εγγράφων JSON, όπου επιτρέπονται ένθετες δομές. Azure εργοστασίου δεδομένων επιτρέπει στον χρήστη να καταδείξετε ιεραρχία μέσω nestingSeparator, η οποία είναι "." στα παραπάνω παραδείγματα. Με το διαχωριστικό, τη δραστηριότητα αντίγραφο θα δημιουργήσει το αντικείμενο "Όνομα" με τρεις θυγατρικά στοιχεία πρώτα, μεσαίο και την τελευταία, ανάλογα με το "Name.First", "Name.Middle" και "Name.Last" στον ορισμό του πίνακα. | Όχι

**DocumentDbCollectionSink** υποστηρίζει τις ακόλουθες ιδιότητες:

| **Ιδιότητα** | **Περιγραφή** | **Επιτρεπόμενη τιμή** | **Απαιτείται** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Έναν ειδικό χαρακτήρα στο όνομα της στήλης προέλευσης για να υποδείξετε ότι αυτό το έγγραφο ένθετη είναι απαραίτητο. <br/><br/>Για παράδειγμα παραπάνω: `Name.First` στο αποτέλεσμα του πίνακα δημιουργεί την ακόλουθη δομή JSON στο έγγραφο DocumentDB:<br/><br/>"Όνομα": {<br/>  "Πρώτη": "Ιωάννης"<br/>}, | Χαρακτήρας που χρησιμοποιείται για να διαχωρίσετε τα επίπεδα ένθεσης.<br/><br/>Η προεπιλεγμένη τιμή είναι `.` (τελεία). | Χαρακτήρας που χρησιμοποιείται για να διαχωρίσετε τα επίπεδα ένθεσης. <br/><br/>Η προεπιλεγμένη τιμή είναι `.` (τελεία). | Όχι | 
| writeBatchSize | Αριθμός των αιτήσεων παράλληλες DocumentDB υπηρεσία για να δημιουργήσετε έγγραφα.<br/><br/>Μπορείτε να βελτιστοποιήσετε την απόδοση κατά την αντιγραφή δεδομένων προς/από DocumentDB με τη χρήση αυτής της ιδιότητας. Μπορείτε να περιμένετε μια καλύτερη απόδοση αύξηση writeBatchSize επειδή αποστέλλονται περισσότερες παράλληλες προσκλήσεις σε DocumentDB. Ωστόσο θα πρέπει να αποφύγετε περιορισμού που μπορεί να εμφανίσουν το μήνυμα σφάλματος: "Αίτηση επιτόκιο είναι μεγάλο".<br/><br/>Περιορισμού καθορίζεται από έναν αριθμό παραγόντων, όπως το μέγεθος των εγγράφων, αριθμός των όρων στα έγγραφα, ευρετήριο πολιτικής συλλογής προορισμού, κ.λπ. Για λειτουργίες αντιγραφής, μπορείτε να χρησιμοποιήσετε μια καλύτερη συλλογή (π.χ., S3) για να έχετε τις πιο μετάδοσης διαθέσιμη (2.500 αίτησης μονάδες/δευτερόλεπτο). | Ακέραιος αριθμός | Δεν (προεπιλογή: 10000) |
| writeBatchTimeout | Περιμένετε ώρα για τη λειτουργία για να ολοκληρωθεί πριν λήξει το χρονικό όριο. | χρονικό διάστημα<br/><br/> Παράδειγμα: "00: 30:00" (30 λεπτά). | Όχι |
 
## <a name="appendix"></a>Προσάρτημα
1. **Ερώτηση:**  
    κάνει την ενημέρωση υποστήριξη δραστηριότητας αντίγραφο των υπαρχουσών εγγραφών;

    **Απάντηση:**  
    όχι.

2. **Ερώτηση:**  
    πώς γνωρίζει επανάληψη της ένα αντίγραφο σε θέματα DocumentDB ήδη αντιγραφεί εγγραφές;

    **Απάντηση:**  
    εάν εγγραφές έχουν ένα πεδίο "Αναγνωριστικό" και τη λειτουργία αντιγραφής προσπαθεί να εισαγάγετε μια εγγραφή με το ίδιο Αναγνωριστικό, η λειτουργία αντιγραφής παρουσιάσει σφάλμα.  
 
3. **Ερώτηση:** 
    εργοστασίου δεδομένων υποστηρίζει η [περιοχή ή δεδομένα που βασίζονται σε κατακερματισμός διαμερισμάτων](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/); 

    **Απάντηση:** 
    όχι. 
4. **Ερώτηση:** 
    να να καθορίσετε περισσότερες από μία DocumentDB συλλογής για έναν πίνακα;
    
    **Απάντηση:** 
    όχι. Μπορεί να καθοριστεί μόνο μία συλλογή αυτήν τη στιγμή.
     
## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.

<properties 
    pageTitle="Μετακίνηση δεδομένων από Amazon απλό αποθήκευσης υπηρεσία χρησιμοποιώντας δεδομένα εργοστασίου | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μεταφέρετε δεδομένα από το Amazon απλή υπηρεσία αποθήκευσης (S3) χρησιμοποιώντας Azure εργοστασίου δεδομένων." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Μετακίνηση δεδομένων από το Amazon απλό υπηρεσία αποθήκευσης χρησιμοποιώντας εργοστασίου δεδομένων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων για την από το Amazon απλή υπηρεσία αποθήκευσης (S3) σε μια άλλη δεδομένα αποθήκευση. Σε αυτό το άρθρο δημιουργεί στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) που παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων, καθώς και μια λίστα με τις υποστηριζόμενες προέλευσης/δέκτη χώροι αποθήκευσης δεδομένων δραστηριότητας αντίγραφο.  

Προέλευση δεδομένων υποστηρίζει επί του παρόντος μόνο κυλιόμενο τα δεδομένα από το Amazon S3 σε άλλους χώρους αποθήκευσης, αλλά όχι για τη μετακίνηση δεδομένων από άλλες αποθηκεύει δεδομένα στο Amazon S3.

## <a name="required-permissions"></a>Απαιτούμενα δικαιώματα

Για να αντιγράψετε δεδομένα από το Amazon S3, βεβαιωθείτε ότι σας έχουν παραχωρηθεί παρακάτω δικαιώματα:

- **S3:GetObject** και **s3:GetObjectVersion** για λειτουργίες αντικειμένου S3 Amazon
- **S3:ListBucket** και **s3:ListAllMyBuckets** (χρησιμοποιείται στον Οδηγό αντίγραφο μόνο) για το Amazon S3 χρωματισμού λειτουργίες 

Μπορείτε να βρείτε την πλήρη λίστα των permisions Amazon S3 με λεπτομέρειες από [Τον καθορισμό δικαιωμάτων σε μια πολιτική](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από το Amazon S3 είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Το παρακάτω παράδειγμα παρέχει ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Σας δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από το Amazon S3 χώρο αποθήκευσης Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από το Amazon S3 αντικειμένων Blob του Azure
Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια S3 Amazon στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

- Ένα συνδεδεμένο υπηρεσία του τύπου [AwsAccessKey](#linked-service-properties).
- Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AmazonS3](#dataset-type-properties).
- Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [FileSystemSource](#copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από το Amazon S3 μια αντικειμένων blob του Azure κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

**Υπηρεσία S3 Amazon συνδεδεμένες**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
            }
        }
    }

**Υπηρεσία αποθήκευσης συνδεδεμένες του Azure**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Σύνολο δεδομένων εισαγωγής Amazon S3**

Ρύθμιση **"εξωτερική": true** πληροφορεί την υπηρεσία εργοστασίου δεδομένων που του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων. Ορίστε αυτήν την ιδιότητα στην τιμή true σε ένα σύνολο δεδομένων εισόδου που δεν παράγεται από μια δραστηριότητα στη διοχέτευση.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Σωλήνωση με αντιγραφή δραστηριότητα**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **FileSystemSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Ιδιότητες συνδεδεμένων υπηρεσίας

Ο παρακάτω πίνακας παρέχει περιγραφή για συγκεκριμένα στοιχεία JSON για Amazon S3 (**AwsAccessKey**) συνδεδεμένο υπηρεσίας.

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | Το Αναγνωριστικό του αριθμού-κλειδιού μυστικού πρόσβασης. | συμβολοσειρά | Ναι |
| secretAccessKey | Το ίδιο το κλειδί μυστικού πρόσβασης. | Κρυπτογραφημένο μυστικό συμβολοσειράς | Ναι | 


## <a name="dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και πολιτικής είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **AmazonS3** (το οποίο περιλαμβάνει το σύνολο δεδομένων Amazon S3) περιλαμβάνει τις ακόλουθες ιδιότητες

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------- | ------ | 
| bucketName | Το όνομα του Κάδου S3. | Συμβολοσειρά | Ναι |
| πλήκτρο | Το κλειδί αντικειμένου S3. | Συμβολοσειρά | Όχι | 
| πρόθεμα | Πρόθεμα για το κλειδί αντικειμένου S3. Αντικείμενα του οποίου πλήκτρα ξεκινούν με αυτό το πρόθεμα είναι επιλεγμένα. Ισχύει μόνο όταν το κλειδί είναι κενή. | Συμβολοσειρά | Όχι | 
| έκδοση | Η έκδοση του αντικειμένου S3 εάν είναι ενεργοποιημένη η διαχείριση εκδόσεων S3. | Συμβολοσειρά | Όχι |  
| μορφή | Υποστηρίζονται οι παρακάτω τύποι μορφοποίησης: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Ορίστε την ιδιότητα **τύπου** στην περιοχή μορφοποίηση σε μία από αυτές τις τιμές. Ανατρέξτε στο θέμα [Καθορίζοντας TextFormat](#specifying-textformat), [Καθορίζοντας AvroFormat](#specifying-avroformat), [Καθορίζοντας JsonFormat](#specifying-jsonformat), [Καθορίζοντας OrcFormat](#specifying-orcformat)και [Καθορίζοντας ParquetFormat](#specifying-parquetformat) ενότητες για λεπτομέρειες. Εάν θέλετε να αντιγράψετε τα αρχεία ως-είναι μεταξύ βασίζεται στο αρχείο αποθηκεύονται (δυαδικό αντίγραφο), μπορείτε να μεταβείτε στην ενότητα μορφή σε δύο ορισμών συνόλου δεδομένων εισόδου και εξόδου.| Όχι
| συμπίεση | Καθορίστε τον τύπο και το επίπεδο συμπίεσης για τα δεδομένα. Υποστηριζόμενοι τύποι είναι: **GZip**, **Deflate**, και **BZip2** και υποστηριζόμενα επίπεδα είναι: **βέλτιστη** και **πιο γρήγορη**. Προς το παρόν, οι ρυθμίσεις συμπίεσης δεν υποστηρίζονται για δεδομένα σε **AvroFormat** ή **OrcFormat**. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [υποστήριξη συμπίεσης](#compression-support) .  | Όχι |

> [AZURE.NOTE] bucketName + πλήκτρο Καθορίζει τη θέση του αντικειμένου S3 όπου χρωματισμού είναι το κοντέινερ ριζικό S3 αντικείμενα και αριθμού-κλειδιού είναι η πλήρης διαδρομή S3 αντικείμενο.

### <a name="sample-dataset-with-prefix"></a>Δείγμα συνόλου δεδομένων με πρόθεμα

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Δείγμα συνόλου δεδομένων (με την έκδοση)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Δυναμικές διαδρομές για S3

Στο δείγμα, μπορούμε να χρησιμοποιήσουμε σταθερών τιμών για το κλειδί και bucketName ιδιότητες στο του συνόλου δεδομένων Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Μπορείτε να έχετε τον υπολογισμό το κλειδί και bucketName δυναμικά κατά το χρόνο εκτέλεσης με τη χρήση μεταβλητών συστήματος όπως SliceStart εργοστασίου δεδομένων.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Μπορείτε να κάνετε το ίδιο για την ιδιότητα πρόθεμα ένα σύνολο δεδομένων Amazon S3. Ανατρέξτε στο θέμα [συναρτήσεις δεδομένων εργοστασίου και μεταβλητές συστήματος](data-factory-functions-variables.md) για μια λίστα με τις υποστηριζόμενες συναρτήσεις και μεταβλητές. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Αντιγραφή ιδιότητες τύπου δραστηριότητας

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και οι πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα **typeProperties** της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Όταν προέλευσης στη δραστηριότητα αντιγραφή του τύπου **FileSystemSource** (το οποίο περιλαμβάνει το Amazon S3), οι ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα typeProperties:

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- | 
| περιοδικότητας | Καθορίζει εάν για να σταδιακά σε λίστα S3 αντικείμενα κάτω από τον κατάλογο. | true/false | Όχι | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.

## <a name="next-steps"></a>Επόμενα βήματα
Ανατρέξτε στα ακόλουθα άρθρα: 
- [Πρόγραμμα εκμάθησης αντίγραφο δραστηριότητας](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) για οδηγίες βήμα προς βήμα για τη δημιουργία μιας διοχέτευσης με μια δραστηριότητα αντίγραφο. 

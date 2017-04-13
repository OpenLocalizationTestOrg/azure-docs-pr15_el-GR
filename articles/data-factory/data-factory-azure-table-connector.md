<properties 
    pageTitle="Μετακίνηση δεδομένων προς/από πίνακα του Azure | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μετακινήσετε δεδομένων προς/από χώρο αποθήκευσης πινάκων του Azure μέσω Azure εργοστασίου δεδομένων." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Μετακίνηση δεδομένων από και προς πίνακα Azure χρησιμοποιώντας εργοστασίου δεδομένων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων προς/από πίνακα του Azure από/προς μια άλλη χώρου αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση των δεδομένων κίνηση και συνδυασμών αποθήκευσης δεδομένων υποστηρίζονται με αντιγραφή δραστηριότητα.

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένων προς/από χώρο αποθήκευσης πινάκων του Azure είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Τα παρακάτω παραδείγματα παρέχουν ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα προς και από το χώρο αποθήκευσης πινάκων του Azure και βάση δεδομένων αντικειμένων Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** από οποιαδήποτε από τις προελεύσεις σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από πίνακα του Azure αντικειμένων Blob του Azure

Το παρακάτω παράδειγμα εμφανίζει:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (χρησιμοποιείται για πίνακα & blob).
2.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureTable](#azure-table-dataset-type-properties).
3.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  Της [διοχέτευσης](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [AzureTableSource](#azure-table-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Το δείγμα αντιγράφει δεδομένα που ανήκουν σε τα διαμερίσματα προεπιλεγμένη σε έναν πίνακα σε ένα αντικείμενο blob του Azure κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα.

**Azure χώρου αποθήκευσης που συνδέονται υπηρεσίας:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure εργοστασίου δεδομένων υποστηρίζει δύο τύπους αποθήκευσης Azure συνδεδεμένες υπηρεσίες: **AzureStorage** και **AzureStorageSas**. Για το πρώτο, μπορείτε να καθορίσετε τη συμβολοσειρά σύνδεσης που περιλαμβάνει το κλειδί λογαριασμού και για αυτό νεότερη έκδοση, μπορείτε να καθορίσετε το Uri θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας). Ανατρέξτε στην ενότητα [Συνδεδεμένων υπηρεσιών](#linked-services) για λεπτομέρειες.  

**Azure dataset εισαγωγής πίνακα:**

Το δείγμα προϋποθέτει ότι έχετε δημιουργήσει έναν πίνακα "Πίνακας" στον πίνακα Azure.
 
Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Αντικειμένων Blob του Azure εξόδου συνόλου δεδομένων:**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Σωλήνωση με τη δραστηριότητα αντίγραφο:**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **AzureTableSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που καθορίζεται με την ιδιότητα **AzureTableSourceQuery** επιλέγει τα δεδομένα από την προεπιλεγμένη partition κάθε ώρα για να αντιγράψετε.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },              
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]  
        }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Δείγμα: Αντιγράψτε δεδομένα από αντικειμένων Blob του Azure σε πίνακα του Azure

Το παρακάτω παράδειγμα εμφανίζει:

1.  Ένα συνδεδεμένο της υπηρεσίας τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (χρησιμοποιείται για πίνακα & blob)
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureTable](#azure-table-dataset-type-properties). 
4.  Της [διοχέτευσης](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) και [AzureTableSink](#azure-table-copy-activity-type-properties). 


Το δείγμα αντιγράφει χρονολογική σειρά δεδομένων από μια Azure αντικειμένων blob σε μια Azure πίνακα ανά ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα.

**Υπηρεσία Azure αποθήκευσης (για τόσο πίνακα Azure & Blob) συνδεδεμένες:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure εργοστασίου δεδομένων υποστηρίζει δύο τύπους αποθήκευσης Azure συνδεδεμένες υπηρεσίες: **AzureStorage** και **AzureStorageSas**. Για το πρώτο, μπορείτε να καθορίσετε τη συμβολοσειρά σύνδεσης που περιλαμβάνει το κλειδί λογαριασμού και για αυτό νεότερη έκδοση, μπορείτε να καθορίσετε το Uri θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας). Ανατρέξτε στην ενότητα [Συνδεδεμένων υπηρεσιών](#linked-services) για λεπτομέρειες. 

**Azure Blob εισαγωγής συνόλου δεδομένων:**

Δεδομένα είναι επιλέξατε προς τα επάνω από ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Το φάκελο διαδρομή και το όνομα αρχείου για το αντικείμενο blob αξιολογούνται δυναμικά με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνα και ημέρα τμήμα της ώρας έναρξης και το τμήμα ώρας της ώρας έναρξης χρησιμοποιεί το όνομα του αρχείου. "εξωτερική": "true" Ρύθμιση πληροφορεί την υπηρεσία εργοστασίου δεδομένων που του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
          "fileName": "{Hour}.csv",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**Πινάκων του Azure εξόδου συνόλου δεδομένων:**

Το δείγμα αντιγράφει δεδομένα σε έναν πίνακα με το όνομα "Πίνακας" στον πίνακα Azure. Δημιουργία ενός πίνακα του Azure με τον ίδιο αριθμό στηλών με τον αναμενόμενο τρόπο το αρχείο Blob CSV ώστε να περιέχει. Νέες γραμμές προστίθενται στον πίνακα κάθε ώρα. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Σωλήνωση με τη δραστηριότητα αντίγραφο:**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **BlobSource** και **δέκτη** τύπος έχει οριστεί σε **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
              }
            },
            "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },                      
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="linked-services"></a>Συνδεδεμένες υπηρεσίες
Υπάρχουν δύο τύποι συνδεδεμένων υπηρεσιών που μπορείτε να χρησιμοποιήσετε για να συνδέσετε το χώρο αποθήκευσης αντικειμένων blob του Azure με μια εργοστασίου Azure δεδομένων. Είναι: **AzureStorage** συνδεδεμένες υπηρεσία και η υπηρεσία **AzureStorageSas** συνδεδεμένες. Η υπηρεσία αποθήκευσης Azure συνδεδεμένες παρέχει την προέλευση δεδομένων με την καθολική πρόσβαση για το χώρο αποθήκευσης Azure. Ότι, το Azure αποθήκευσης συσχετισμών Ασφαλείας (κοινόχρηστα υπογραφή Access) συνδέονται υπηρεσία παρέχει την προέλευση δεδομένων με την access συνδεδεμένων-περιορισμένων/ώρα για το χώρο αποθήκευσης Azure. Δεν υπάρχουν άλλες διαφορές ανάμεσα σε αυτές τις δύο συνδεδεμένες υπηρεσίες. Επιλέξτε την υπηρεσία συνδεδεμένων που ικανοποιεί τις ανάγκες σας. Οι παρακάτω ενότητες παρέχουν περισσότερες λεπτομέρειες σχετικά με αυτές τις δύο συνδεδεμένες υπηρεσίες.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure ιδιότητες τύπου συνόλου δεδομένων πίνακα

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα typeProperties είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα **typeProperties** για το σύνολο δεδομένων του τύπου **AzureTable** περιλαμβάνει τις ακόλουθες ιδιότητες.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| όνομα πίνακα | Το όνομα του πίνακα στην παρουσία βάσης δεδομένων πίνακα Azure που αναφέρεται συνδεδεμένων υπηρεσιών. | Ναι. Όταν ένα όνομα πίνακα έχει οριστεί χωρίς μια azureTableSourceQuery, αντιγράφονται όλες τις εγγραφές από τον πίνακα στον προορισμό. Εάν καθορίζεται επίσης ένα azureTableSourceQuery, τις εγγραφές από τον πίνακα που ικανοποιεί το ερώτημα αντιγράφονται στον προορισμό. |

### <a name="schema-by-data-factory"></a>Σχήμα από προέλευση δεδομένων
Για δεδομένα σχήματος χωρίς καταστήματα όπως πίνακα Azure, την υπηρεσία εργοστασίου δεδομένων παράγει το σχήμα με έναν από τους εξής τρόπους:

1.  Εάν καθορίσετε τη δομή των δεδομένων, χρησιμοποιώντας την ιδιότητα **δομή** στον ορισμό του συνόλου δεδομένων, η υπηρεσία εργοστασίου δεδομένων τηρεί αυτήν τη δομή ως το σχήμα. Σε αυτήν την περίπτωση, εάν μια γραμμή δεν περιέχει μια τιμή για μια στήλη, τιμή null παρέχεται για αυτήν.
2. Εάν δεν μπορείτε να καθορίσετε τη δομή των δεδομένων, χρησιμοποιώντας την ιδιότητα **δομή** στον ορισμό του συνόλου δεδομένων, εργοστασίου δεδομένων παράγει το σχήμα, χρησιμοποιώντας την πρώτη γραμμή στα δεδομένα. Σε αυτήν την περίπτωση, εάν η πρώτη γραμμή δεν περιέχει το πλήρες σχήμα, ορισμένες στήλες είναι παραβλέψει στο αποτέλεσμα της λειτουργίας αντιγραφής.

Επομένως, για τις προελεύσεις δεδομένων χωρίς σχήματος, είναι η βέλτιστη πρακτική για να καθορίσετε τη δομή των δεδομένων χρησιμοποιώντας την ιδιότητα **δομή** .

## <a name="azure-table-copy-activity-type-properties"></a>Azure ιδιότητες τύπου δραστηριότητας Αντιγραφή πίνακα

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου συνόλων δεδομένων και πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

**AzureTableSource** υποστηρίζει τις ακόλουθες ιδιότητες στην ενότητα typeProperties:

Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος Azure πίνακα. Δείτε παραδείγματα στην επόμενη ενότητα. | Όχι. Όταν ένα όνομα πίνακα έχει οριστεί χωρίς μια azureTableSourceQuery, αντιγράφονται όλες τις εγγραφές από τον πίνακα στον προορισμό. Εάν καθορίζεται επίσης ένα azureTableSourceQuery, τις εγγραφές από τον πίνακα που ικανοποιεί το ερώτημα αντιγράφονται στον προορισμό.
azureTableSourceIgnoreTableNotFound | Υποδεικνύουν αν δεν υπάρχει swallow την εξαίρεση του πίνακα. | TRUE<br/>FALSE | Όχι |

### <a name="azuretablesourcequery-examples"></a>Παραδείγματα azureTableSourceQuery

Εάν η στήλη πίνακα Azure είναι τύπου συμβολοσειράς: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Εάν η στήλη πίνακα Azure είναι τύπου ημερομηνίας και ώρας: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** υποστηρίζει τις ακόλουθες ιδιότητες στην ενότητα typeProperties:


Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Προεπιλεγμένη partition τιμής κλειδιού που μπορούν να χρησιμοποιηθούν από το δέκτη. | Μια τιμή συμβολοσειρά. | Όχι 
azureTablePartitionKeyName | Καθορίστε το όνομα της στήλης του οποίου τις τιμές που χρησιμοποιούνται ως αριθμούς-κλειδιά διαμερισμάτων. Εάν δεν καθορίζονται, AzureTableDefaultPartitionKeyValue χρησιμοποιείται ως το κλειδί διαμερίσματα. | Ένα όνομα στήλης. | Όχι |
azureTableRowKeyName | Καθορίστε το όνομα της στήλης των οποίων οι τιμές της στήλης που χρησιμοποιούνται ως γραμμή κλειδί. Εάν δεν καθορίζονται, χρησιμοποιήστε ένα GUID για κάθε γραμμή. | Ένα όνομα στήλης. | Όχι  
azureTableInsertType | Η κατάσταση για να εισαγάγετε δεδομένα στο Azure πίνακα.<br/><br/>Αυτήν την ιδιότητα ελέγχει αν υπάρχουσες γραμμές στον πίνακα εξόδου με αντίστοιχα πλήκτρα διαμερίσματα και γραμμή έχει τις τιμές τους αντικατασταθεί ή συγχώνευση. <br/><br/>Για να μάθετε σχετικά με τον τρόπο που λειτουργούν αυτές οι ρυθμίσεις (συγχώνευση και αντικατάσταση), ανατρέξτε στο θέμα [Εισαγωγή ή οντότητα συγχώνευση](https://msdn.microsoft.com/library/azure/hh452241.aspx) και [Εισαγωγή ή αντικατάσταση οντότητα](https://msdn.microsoft.com/library/azure/hh452242.aspx) θέματα. <br/><br> Αυτή η ρύθμιση εφαρμόζεται σε επίπεδο γραμμής, δεν το επίπεδο πίνακα, και καμία επιλογή διαγράφει γραμμές στον πίνακα εξόδου που δεν υπάρχουν στο την είσοδο. | Συγχώνευση (προεπιλογή)<br/>Αντικατάσταση | Όχι 
writeBatchSize | Εισάγει δεδομένα στον πίνακα, Azure όταν πατήσετε το writeBatchSize ή writeBatchTimeout. | Ακέραιος αριθμός (αριθμός γραμμών)| Δεν (προεπιλογή: 10000) 
writeBatchTimeout | Εισάγει δεδομένα στον πίνακα, Azure όταν πατήσετε το writeBatchSize ή writeBatchTimeout | χρονικό διάστημα<br/><br/>Παράδειγμα: "00: 20:00" (20 λεπτά) | Δεν (προεπιλογή για προεπιλεγμένο χρονικό όριο του χώρου αποθήκευσης προγράμματος-πελάτη τιμή 90 sec)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Αντιστοιχίστε μια στήλη προέλευσης σε μια στήλη προορισμού, χρησιμοποιώντας το πρόγραμμα μετατροπής ιδιότητα JSON να μπορέσετε να χρησιμοποιήσετε τη στήλη προορισμού ως το azureTablePartitionKeyName.

Στο παρακάτω παράδειγμα, η στήλη προέλευσης DivisionID έχει αντιστοιχιστεί στη στήλη προορισμού: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

Το DivisionID έχει καθοριστεί ως το κλειδί διαμερίσματα. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Αντιστοίχιση τύπων για πινάκων του Azure

Όπως αναφέρθηκε στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , αντιγραφή δραστηριότητας εκτελεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω δύο βήματα προσέγγιση.

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Κατά τη μετακίνηση δεδομένων σε & από πίνακα του Azure, το παρακάτω [αντιστοιχίσεις που ορίζονται από πίνακα του Azure υπηρεσίας](https://msdn.microsoft.com/library/azure/dd179338.aspx) που χρησιμοποιούνται από τους τύπους Azure OData πίνακα σε τύπο .NET και το αντίστροφο. 

| Τύπος δεδομένων OData | Τύπος .NET | Λεπτομέρειες |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | Έναν πίνακα byte έως 64 KB. |
| Edm.Boolean | bool | Μια δυαδική τιμή. |
| Edm.DateTime | Ημερομηνίας και ώρας | Μια τιμή 64-bit εκφρασμένη ως Συντονισμένη παγκόσμια ώρα (UTC). Υποστηριζόμενες περιοχή DateTime αρχίζει από 12:00 τα μεσάνυχτα, Ιανουάριος 1, 1601 μ.χ. (Μ.Χ.), UTC. Στην περιοχή λήγει στις 31 Δεκεμβρίου 9999. |
| Edm.Double | διπλά | Τιμή κινητής υποδιαστολής μια 64-bit. |
| Edm.Guid | GUID | Καθολικά μοναδικό αναγνωριστικό 128 bit. |
| Edm.Int32 | Int32 ή int | Ένας ακέραιος 32-bit. |
| Edm.Int64 | Int64 ή μεγάλη | Ένας ακέραιος 64-bit. |
| Edm.String | Συμβολοσειρά | Μια τιμή κωδικοποίηση UTF-16. Τιμές συμβολοσειρών μπορεί να είναι έως 64 KB. |

### <a name="type-conversion-sample"></a>Δείγμα μετατροπής τύπου

Το παρακάτω παράδειγμα είναι για την αντιγραφή δεδομένων από ένα αντικειμένων Blob του Azure Azure πίνακα με τις μετατροπές τύπου. 

Ας υποθέσουμε ότι το σύνολο δεδομένων Blob είναι σε μορφή CSV και περιέχει τρεις στήλες. Μία από αυτές είναι μια στήλη ημερομηνίας/ώρας με μια μορφή προσαρμοσμένης ημερομηνίας/ώρας χρησιμοποιώντας συντομογραφίες Γαλλικά για την ημέρα της εβδομάδας. 

Ορισμός του συνόλου δεδομένων προέλευσης Blob ως εξής μαζί με ορισμοί τύπων για τις στήλες.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Δεδομένο την αντιστοίχιση τύπου από τον τύπο OData Azure πίνακα σε τύπο .NET, να ορίζετε τον πίνακα σε πίνακα του Azure με το παρακάτω σχήμα. 

**Σχήμα Azure πίνακα:**

Όνομα στήλης | Τύπος
----------- | --------
αναγνωριστικό χρήστη | Edm.Int64
Όνομα | Edm.String 
lastlogindate | Edm.DateTime

Στη συνέχεια, ορίστε το σύνολο δεδομένων πίνακα Azure ως εξής. Δεν χρειάζεται για να καθορίσετε την ενότητα "Δομή" με τις πληροφορίες του τύπου, επειδή οι πληροφορίες τύπου ήδη που καθορίζεται στο χώρο αποθήκευσης της βάσης δεδομένων.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Σε αυτήν την περίπτωση, εργοστασίου δεδομένων αυτόματα τις μετατροπές τύπων συμπεριλαμβανομένου του πεδίου ημερομηνίας/ώρας με τη μορφή προσαρμοσμένης ημερομηνίας/ώρας χρησιμοποιώντας τα κουλτούρας "fr-fr" κατά τη μετακίνηση δεδομένων από Blob Azure πίνακα.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Για να μάθετε σχετικά με τους παράγοντες κλειδιού ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφοροι τρόποι για να βελτιστοποιήσετε, ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md).








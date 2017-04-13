<properties 
    pageTitle="Μετακίνηση δεδομένων προς/από βάση δεδομένων SQL Azure | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μετακινήσετε δεδομένων προς/από βάση δεδομένων SQL Azure χρησιμοποιώντας εργοστασίου δεδομένων Azure." 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Μετακίνηση δεδομένων από βάση δεδομένων SQL Azure χρησιμοποιώντας Azure δεδομένων εργοστασίου και
Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων προς/από βάση δεδομένων SQL Azure από/προς μια άλλη χώρου αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται. 

## <a name="supported-sources-and-sinks"></a>Υποστηριζόμενες προελεύσεις και δέκτες
Ανατρέξτε στον πίνακα [χώροι αποθήκευσης δεδομένων υποστηρίζονται](data-factory-data-movement-activities.md#supported-data-stores-and-formats) για μια λίστα των χώροι αποθήκευσης δεδομένων που υποστηρίζονται ως προελεύσεις ή δέκτες από τη δραστηριότητα αντίγραφο. Μπορείτε να μετακινήσετε δεδομένα από οποιαδήποτε χώρου αποθήκευσης υποστηριζόμενη προέλευση δεδομένων με βάση δεδομένων SQL Azure ή από βάση δεδομένων SQL Azure σε οποιοδήποτε υποστηριζόμενο δέκτη χώρου αποθήκευσης δεδομένων.

## <a name="create-pipeline"></a>Δημιουργία διοχέτευσης
Μπορείτε να δημιουργήσετε μια διαδικασία με μια δραστηριότητα αντίγραφο που ορίζει τη μετακίνηση δεδομένων προς/από μια βάση δεδομένων Azure SQL, χρησιμοποιώντας διαφορετικό εργαλεία/APIs.  

- Αντιγραφή οδηγού
- Πύλη του Azure
- Visual Studio
- Azure PowerShell
- .NET API
- REST API

Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας εκμάθηση](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) για οδηγίες βήμα προς βήμα για τη δημιουργία μιας διοχέτευσης με μια δραστηριότητα αντίγραφο με διαφορετικούς τρόπους.   

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένων προς/από βάση δεδομένων SQL Azure είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Τα παρακάτω παραδείγματα παρέχουν ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα από βάση δεδομένων SQL Azure και χώρο αποθήκευσης Blob του Azure και. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** από οποιαδήποτε από τις προελεύσεις σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Δείγμα: Αντιγραφή δεδομένων από βάση δεδομένων SQL Azure αντικειμένων Blob του Azure

Η ίδια ορίζει τα εξής οντοτήτων εργοστασίου δεδομένων:

1. Ένα συνδεδεμένο υπηρεσία του τύπου [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [SqlSource](#azure-sql-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει χρονολογική σειρά δεδομένων (ανά ώρα, ημερήσια, κ.λπ.) από έναν πίνακα σε βάση δεδομένων Azure SQL ένα blob κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα.  

**Azure SQL συνδεδεμένες υπηρεσίας**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Ανατρέξτε στην ενότητα [Υπηρεσία συνδεδεμένων SQL Azure](#azure-sql-linked-service-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτήν την υπηρεσία συνδεδεμένων. 

**Azure υπηρεσίας συνδεδεμένο χώρο αποθήκευσης αντικειμένων Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Ανατρέξτε στο άρθρο [Αντικειμένων Blob του Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτήν την υπηρεσία συνδεδεμένων. 

**Azure SQL εισαγωγής συνόλου δεδομένων**

Το δείγμα προϋποθέτει που έχετε δημιουργήσει έναν πίνακα "Πίνακας" στο Azure SQL και περιέχει μια στήλη που ονομάζεται "timestampcolumn" για το χρόνο σειράς δεδομένων. 

Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων Azure ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
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

Ανατρέξτε στην ενότητα [Ιδιότητες τύπου συνόλου δεδομένων Azure SQL](#azure-sql-dataset-type-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτόν τον τύπο συνόλου δεδομένων.  

**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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

Ανατρέξτε στην ενότητα [Ιδιότητες τύπου συνόλου δεδομένων αντικειμένων Blob του Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτόν τον τύπο συνόλου δεδομένων.  

**Σωλήνωση με αντιγραφή δραστηριότητα**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **SqlSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που έχουν καθοριστεί για την ιδιότητα **SqlReaderQuery** επιλέγει την τελευταία ώρα για να αντιγράψετε τα δεδομένα.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

Στο παράδειγμα, **sqlReaderQuery** έχει οριστεί για το SqlSource. Η δραστηριότητα αντίγραφο εκτελείται αυτό το ερώτημα σε σχέση με την προέλευση της βάσης δεδομένων SQL Azure για να λάβετε τα δεδομένα. Εναλλακτικά, μπορείτε να καθορίσετε μια αποθηκευμένη διαδικασία, καθορίζοντας τις **sqlReaderStoredProcedureName** και **storedProcedureParameters** (Εάν η αποθηκευμένη διαδικασία λάβει παραμέτρους).

Εάν δεν καθορίσετε sqlReaderQuery ή sqlReaderStoredProcedureName, τις στήλες που καθορίζονται στην ενότητα δομή του συνόλου δεδομένων JSON χρησιμοποιούνται για τη δημιουργία ενός ερωτήματος για να εκτελέσετε σε σχέση με τη βάση δεδομένων SQL Azure. Για παράδειγμα: `select column1, column2 from mytable`. Εάν ο ορισμός συνόλου δεδομένων δεν έχει τη δομή, όλες οι στήλες έχουν επιλεγεί από τον πίνακα. 


Ανατρέξτε στην ενότητα [Sql προέλευσης](#sqlsource) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) για τη λίστα των ιδιοτήτων που υποστηρίζονται από το SqlSource και BlobSink. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Δείγμα: Αντιγράψτε δεδομένα από αντικειμένων Blob του Azure σε βάση δεδομένων SQL Azure

Το δείγμα ορίζει τα εξής οντοτήτων εργοστασίου δεδομένων:  

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) και [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Το δείγμα αντιγράφει χρονολογική σειρά δεδομένων (ανά ώρα, ημερήσια, κ.λπ.) από το Azure αντικειμένων blob σε έναν πίνακα του Azure SQL βάσης δεδομένων κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 


**Azure SQL συνδεδεμένες υπηρεσίας**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Ανατρέξτε στην ενότητα [Υπηρεσία συνδεδεμένων SQL Azure](#azure-sql-linked-service-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτήν την υπηρεσία συνδεδεμένων. 

**Azure υπηρεσίας συνδεδεμένο χώρο αποθήκευσης αντικειμένων Blob**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Ανατρέξτε στο άρθρο [Αντικειμένων Blob του Azure](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτήν την υπηρεσία συνδεδεμένων.

**Azure Blob εισαγωγής συνόλου δεδομένων**

Δεδομένα είναι επιλέξατε προς τα επάνω από ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Το φάκελο διαδρομή και το όνομα αρχείου για το αντικείμενο blob αξιολογούνται δυναμικά με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνα και ημέρα τμήμα της ώρας έναρξης και το τμήμα ώρας της ώρας έναρξης χρησιμοποιεί το όνομα του αρχείου. "εξωτερική": η ρύθμιση "true" πληροφορεί την υπηρεσία εργοστασίου δεδομένων ότι αυτός ο πίνακας είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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

Ανατρέξτε στην ενότητα [Ιδιότητες τύπου συνόλου δεδομένων αντικειμένων Blob του Azure](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτόν τον τύπο συνόλου δεδομένων.

**Azure συνόλου δεδομένων εξόδου SQL**

Το δείγμα αντιγράφει δεδομένα σε έναν πίνακα με το όνομα "Πίνακας" στο Azure SQL. Δημιουργία πίνακα στο SQL Azure με τον ίδιο αριθμό στηλών με τον αναμενόμενο τρόπο το αρχείο Blob CSV ώστε να περιέχει. Νέες γραμμές προστίθενται στον πίνακα κάθε ώρα. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Ανατρέξτε στην ενότητα [Ιδιότητες τύπου συνόλου δεδομένων Azure SQL](#azure-sql-dataset-type-properties) για τη λίστα ιδιοτήτων που υποστηρίζονται από αυτόν τον τύπο συνόλου δεδομένων.

**Σωλήνωση με αντιγραφή δραστηριότητα**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **BlobSource** και **δέκτη** τύπος έχει οριστεί σε **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
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

Ανατρέξτε στην ενότητα [Sql δέκτη](#sqlsink) και [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) για τη λίστα των ιδιοτήτων που υποστηρίζονται από το SqlSink και BlobSource. 


## <a name="azure-sql-linked-service-properties"></a>Ιδιότητες υπηρεσίας Azure SQL συνδεδεμένες
Στα δείγματα, που έχετε χρησιμοποιήσει ένα συνδεδεμένο της υπηρεσίας τύπου **AzureSqlDatabase** για να συνδέσετε μια βάση δεδομένων Azure SQL με μια προέλευση δεδομένων. Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία SQL Azure συνδεδεμένες JSON στοιχεία.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **AzureSqlDatabase** | Ναι |
| συμβολοσειρά σύνδεσης | Καθορίστε πληροφορίες που απαιτούνται για να συνδεθείτε με την παρουσία βάσης δεδομένων SQL Azure για την ιδιότητα συμβολοσειρά σύνδεσης. | Ναι |

> [AZURE.NOTE] Ρύθμιση παραμέτρων του [Τείχους προστασίας βάσης δεδομένων SQL Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) διακομιστή της βάσης δεδομένων για να [επιτρέψετε τις υπηρεσίες Azure για να αποκτήσετε πρόσβαση στο διακομιστή](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Επιπλέον, εάν θέλετε να αντιγράψετε δεδομένα στη βάση δεδομένων SQL Azure από εξωτερική Azure συμπεριλαμβανομένων από προελεύσεις δεδομένων εσωτερικής εγκατάστασης με πύλης εργοστασίου δεδομένων, ρυθμίστε τις παραμέτρους κατάλληλη περιοχή διευθύνσεων IP για τον υπολογιστή που αποστολή δεδομένων σε βάση δεδομένων SQL Azure. 

## <a name="azure-sql-dataset-type-properties"></a>Azure ιδιότητες τύπου συνόλου δεδομένων SQL
Στα δείγματα, που έχετε χρησιμοποιήσει ένα σύνολο δεδομένων του τύπου **AzureSqlTable** για να αντιπροσωπεύει έναν πίνακα σε μια βάση δεδομένων Azure SQL. 

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.). 

Στην ενότητα typeProperties είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα **typeProperties** για το σύνολο δεδομένων του τύπου **AzureSqlTable** περιλαμβάνει τις ακόλουθες ιδιότητες:

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| όνομα πίνακα | Το όνομα του πίνακα στην παρουσία βάσης δεδομένων SQL Azure που αναφέρεται συνδεδεμένων υπηρεσιών. | Ναι |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure ιδιότητες τύπου δραστηριότητας αντίγραφο SQL
Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και πολιτικής είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων.

> [AZURE.NOTE] Η δραστηριότητα αντίγραφο λαμβάνει μία μόνο είσοδο και παράγει μόνο ένα αποτέλεσμα.

Ιδιότητες που είναι διαθέσιμες στην ενότητα **typeProperties** της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες. 

Εάν θέλετε να μετακινήσετε δεδομένα από μια βάση δεδομένων Azure SQL, μπορείτε να ορίσετε τον τύπο προέλευσης στο αντίγραφο δραστηριότητας σε **SqlSource**. Ομοίως, εάν θέλετε να μετακινήσετε δεδομένα σε μια βάση δεδομένων Azure SQL, θα μπορείτε να ορίσετε τον τύπο δέκτη στη δραστηριότητα Αντιγραφή για να **SqlSink**. Αυτή η ενότητα παρέχει μια λίστα των ιδιοτήτων που υποστηρίζονται από το SqlSource και SqlSink. 

### <a name="sqlsource"></a>SqlSource

Στο αντίγραφο δραστηριότητα, όταν το αρχείο προέλευσης είναι τύπου **SqlSource**, τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα **typeProperties** :

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος SQL. Παράδειγμα: `select * from MyTable`.  | Όχι |
| sqlReaderStoredProcedureName | Το όνομα της αποθηκευμένης διαδικασίας που διαβάζει δεδομένα από τον πίνακα προέλευσης. | Το όνομα της αποθηκευμένης διαδικασίας. | Όχι |
| storedProcedureParameters | Παράμετροι για την αποθηκευμένη διαδικασία. | Ζεύγη ονόματος/τιμής. Ονόματα και περίβλημα των παραμέτρων, πρέπει να συμφωνεί με τα ονόματα και οι περίβλημα από τις παραμέτρους αποθηκευμένη διαδικασία. | Όχι |

Εάν το **sqlReaderQuery** έχει οριστεί για το SqlSource, τη δραστηριότητα αντίγραφο εκτελείται αυτό το ερώτημα σε σχέση με την προέλευση της βάσης δεδομένων SQL Azure για να λάβετε τα δεδομένα. Εναλλακτικά, μπορείτε να καθορίσετε μια αποθηκευμένη διαδικασία, καθορίζοντας τις **sqlReaderStoredProcedureName** και **storedProcedureParameters** (Εάν η αποθηκευμένη διαδικασία λάβει παραμέτρους). 

Εάν δεν καθορίσετε sqlReaderQuery ή sqlReaderStoredProcedureName, τις στήλες που καθορίζονται στην ενότητα δομή του συνόλου δεδομένων JSON που χρησιμοποιούνται για τη δημιουργία ενός ερωτήματος (`select column1, column2 from mytable`) για να εκτελέσετε σε σχέση με τη βάση δεδομένων SQL Azure. Εάν ο ορισμός συνόλου δεδομένων δεν έχει τη δομή, όλες οι στήλες έχουν επιλεγεί από τον πίνακα. 

> [AZURE.NOTE] Όταν χρησιμοποιείτε **sqlReaderStoredProcedureName**, εξακολουθείτε να χρειάζεστε για να καθορίσετε μια τιμή για την ιδιότητα **όνομα πίνακα** σε του συνόλου δεδομένων JSON. Υπάρχουν χωρίς επικυρώσεις Παρόλο που εκτελούνται σε σχέση με αυτόν τον πίνακα. 

### <a name="sqlsource-example"></a>Παράδειγμα SqlSource

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Τον ορισμό αποθηκευμένη διαδικασία:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** υποστηρίζει τις ακόλουθες ιδιότητες:

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Περιμένετε ώρα για τη λειτουργία εισαγωγής δέσμη για να ολοκληρωθεί πριν λήξει το χρονικό όριο. | χρονικό διάστημα<br/><br/> Παράδειγμα: "00: 30:00" (30 λεπτά). | Όχι | 
| writeBatchSize | Εισάγει δεδομένα στον πίνακα SQL, όταν το μέγεθος του buffer φτάσει writeBatchSize. | Ακέραιος αριθμός (αριθμός γραμμών)| Δεν (προεπιλογή: 10000)
| sqlWriterCleanupScript | Καθορίστε ένα ερώτημα για αντιγραφή δραστηριότητα να εκτελέσει τέτοια ώστε η εκκαθάριση δεδομένων από ένα συγκεκριμένο κομμάτι. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [επαναληψιμότητας ενότητας](#repeatability-during-copy). | Μια πρόταση ερωτήματος.  | Όχι |
| sliceIdentifierColumnName | Καθορίστε ένα όνομα στήλης για αντιγραφή δραστηριότητας για να συμπληρώσετε με αναγνωριστικό φέτα που δημιουργούνται αυτόματα, το οποίο χρησιμοποιείται για την εκκαθάριση δεδομένων από ένα συγκεκριμένο κομμάτι όταν εκτελέστε ξανά. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [επαναληψιμότητας ενότητας](#repeatability-during-copy). | Όνομα στήλης της στήλης με τον τύπο δεδομένων binary(32). | Όχι |
| sqlWriterStoredProcedureName | Όνομα της αποθηκευμένης διαδικασίας upserts (ενημερώσεις εισάγει) δεδομένα στον πίνακα προορισμού. | Το όνομα της αποθηκευμένης διαδικασίας. | Όχι |
| storedProcedureParameters | Παράμετροι για την αποθηκευμένη διαδικασία. | Ζεύγη ονόματος/τιμής. Ονόματα και περίβλημα των παραμέτρων, πρέπει να συμφωνεί με τα ονόματα και οι περίβλημα από τις παραμέτρους αποθηκευμένη διαδικασία. | Όχι | 
| sqlWriterTableType | Καθορίστε ένα όνομα τύπου πίνακα για να χρησιμοποιηθεί με την αποθηκευμένη διαδικασία. Αντιγραφή δραστηριότητας κάνει τα δεδομένα για τη μετακίνηση διαθέσιμα σε temp πίνακα με αυτόν τον τύπο πίνακα. Αποθηκευμένη διαδικασία κώδικα, στη συνέχεια, να συγχωνεύσετε τα δεδομένα που αντιγράφεται με υπάρχοντα δεδομένα. | Όνομα ενός τύπου πίνακα. | Όχι |

#### <a name="sqlsink-example"></a>Παράδειγμα SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Οι στήλες ταυτότητας στη βάση δεδομένων προορισμού
Αυτή η ενότητα παρέχει ένα παράδειγμα για την αντιγραφή δεδομένων από έναν πίνακα προέλευσης χωρίς μια στήλη σε έναν πίνακα προορισμού με μια στήλη. 

**Πίνακας προέλευσης:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Πίνακας προορισμού:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Παρατηρήστε ότι ο πίνακας προορισμού έχει μια στήλη. 

**Ορισμός JSON συνόλου δεδομένων προέλευσης**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Καθορισμός JSON συνόλου δεδομένων προορισμού**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Παρατηρήστε ότι έχετε διαφορετικό σχήμα ως τον πίνακα προέλευσης και προορισμού (προορισμού έχει μια επιπλέον στήλη με ταυτότητα). Σε αυτό το σενάριο, πρέπει να καθορίσετε την ιδιότητα **δομή** στο τον ορισμό συνόλου δεδομένων προορισμού, η οποία δεν περιλαμβάνει τη στήλη ταυτότητας. 

Στη συνέχεια, μπορείτε να αντιστοιχίσετε στήλες από το σύνολο δεδομένων προέλευσης σε στήλες του συνόλου δεδομένων προορισμού. Ανατρέξτε στην ενότητα [δείγματα αντιστοίχισης στηλών](#column-mapping-samples) για ένα παράδειγμα. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Αντιστοίχιση τύπων για SQL Server & βάση δεδομένων SQL Azure

Όπως αναφέρεται στην τη δραστηριότητα [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) άρθρο αντίγραφο πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω προσέγγιση βήμα 2:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Όταν μετακινείτε δεδομένα σε & από SQL Azure, SQL server, Sybase παρακάτω αντιστοιχίσεις που χρησιμοποιούνται από τον τύπο SQL στον τύπο .NET και το αντίστροφο.

Η αντιστοίχιση είναι ίδια με την αντιστοίχιση τύπου SQL Server δεδομένων για το ADO.NET.

| Τύπος του μηχανισμού βάσεων δεδομένων SQL Server | Τύπος .NET framework |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| δυαδικό | Byte] |
| bit | Δυαδική τιμή |
| CHAR | Συμβολοσειρά, Char] |
| ημερομηνία | Ημερομηνίας και ώρας |
| Ημερομηνίας και ώρας | Ημερομηνίας και ώρας |
| datetime2 | Ημερομηνίας και ώρας |
| Datetimeoffset | DateTimeOffset |
| Δεκαδικών ψηφίων | Δεκαδικών ψηφίων |
| Χαρακτηριστικό FILESTREAM (varbinary(max)) | Byte] |
| Αιώρηση | Δύο |
| εικόνα | Byte] | 
| Int | Int32 | 
| χρήματα | Δεκαδικών ψηφίων |
| nchar | Συμβολοσειρά, Char] |
| ntext | Συμβολοσειρά, Char] |
| αριθμητική | Δεκαδικών ψηφίων |
| nvarchar | Συμβολοσειρά, Char] |
| πραγματικό | Μεμονωμένο |
| ROWVERSION | Byte] |
| smalldatetime | Ημερομηνίας και ώρας |
| smallint | Int16 |
| smallmoney | Δεκαδικών ψηφίων | 
| SQL_VARIANT | Αντικείμενο * |
| κείμενο | Συμβολοσειρά, Char] |
| ώρα | Χρονικό διάστημα |
| χρονική σήμανση | Byte] |
| tinyint | Byte |
| UniqueIdentifier | GUID |
| varbinary |  Byte] |
| varchar | Συμβολοσειρά, [] Char |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε σχετικά με τους παράγοντες κλειδιού ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.
<properties 
    pageTitle="Μετακίνηση δεδομένων από/προς χρήση εργοστασίου δεδομένων Oracle | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μετακινήσετε δεδομένων προς/από βάση δεδομένων της Oracle που είναι εσωτερικής εγκατάστασης με χρήση εργοστασίου δεδομένων Azure." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Μετακίνηση δεδομένων προς/από Oracle εσωτερικής εγκατάστασης με χρήση εργοστασίου δεδομένων Azure 

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε δραστηριότητα αντίγραφο εργοστασίου δεδομένων για τη μετακίνηση δεδομένων προς/από Oracle για από/προς μια άλλη δεδομένων αποθήκευση. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται.

## <a name="installation"></a>Εγκατάσταση 
Για την υπηρεσία Azure εργοστασίου δεδομένων για να μπορέσετε να συνδεθείτε με τη βάση δεδομένων Oracle εσωτερικής εγκατάστασης, πρέπει να εγκαταστήσετε τα ακόλουθα στοιχεία: 

- Η πύλη διαχείρισης δεδομένων στον ίδιο υπολογιστή που φιλοξενεί τη βάση δεδομένων ή σε έναν νέο υπολογιστή για να αποφύγετε ανταγωνισμό για πόρους με τη βάση δεδομένων. Η πύλη διαχείρισης δεδομένων είναι έναν παράγοντα προγράμματος-πελάτη που συνδέεται προελεύσεις δεδομένων εσωτερικής εγκατάστασης με τις υπηρεσίες cloud με τον τρόπο ασφαλή και διαχειριζόμενα. Δείτε το άρθρο [Μετακίνηση δεδομένων μεταξύ της εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για λεπτομέρειες σχετικά με την πύλη διαχείρισης δεδομένων. 
- Υπηρεσία παροχής δεδομένων Oracle για .NET. Αυτό το στοιχείο περιλαμβάνεται στο [Oracle δεδομένων Access στοιχεία για Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Εγκαταστήστε την κατάλληλη έκδοση (32/64 bit) στον κεντρικό υπολογιστή όπου είναι εγκατεστημένη η πύλη. [12.1 .NET υπηρεσία παροχής δεδομένων Oracle](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) να αποκτήσετε πρόσβαση σε βάση δεδομένων Oracle 10 g, έκδοση 2 ή νεότερη έκδοση.

    Εάν επιλέξετε "XCopy εγκατάστασης", ακολουθήστε τα βήματα του readme.htm. Συνιστάται να επιλέξετε το πρόγραμμα εγκατάστασης με το περιβάλλον εργασίας Χρήστη (μη-XCopy μία). 
 
    Μετά την εγκατάσταση της υπηρεσίας παροχής, επανεκκινήστε την υπηρεσία κεντρικού υπολογιστή πύλης διαχείρισης δεδομένων στον υπολογιστή σας χρησιμοποιώντας τις υπηρεσίες βοηθητική εφαρμογή (ή) Διαχείριση παραμέτρων πύλης διαχείρισης δεδομένων.  

> [AZURE.NOTE] Ανατρέξτε στο θέμα [πύλης αντιμετώπιση προβλημάτων](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) για συμβουλές σχετικά με την αντιμετώπιση προβλημάτων σύνδεσης/πύλης θέματα που σχετίζονται με. 

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από/προς μια βάση δεδομένων Oracle σε οποιονδήποτε από τους χώρους αποθήκευσης δεδομένων υποστηρίζονται δέκτη είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Το παρακάτω παράδειγμα παρέχει ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα από/προς μια βάση δεδομένων Oracle προς/από χώρο αποθήκευσης Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από την Oracle αντικειμένων Blob του Azure
Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια βάση δεδομένων Oracle εσωτερικής εγκατάστασης στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) ως προέλευσης και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ως δέκτη.

Το δείγμα αντιγράφει δεδομένα από έναν πίνακα σε μια βάση δεδομένων Oracle εσωτερικής ένα blob ανά ώρα. Για περισσότερες πληροφορίες σχετικά με τις διάφορες ιδιότητες που χρησιμοποιούνται στο δείγμα, ανατρέξτε στο θέμα τεκμηρίωση στις ενότητες ακολουθώντας τα δείγματα.

**Υπηρεσία Oracle συνδεδεμένες:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Χώρο αποθήκευσης Blob του Azure συνδεδεμένα υπηρεσίας:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Σύνολο δεδομένων Oracle εισαγωγής:**

Το δείγμα προϋποθέτει που έχετε δημιουργήσει έναν πίνακα "Πίνακας" στο Oracle και περιέχει μια στήλη που ονομάζεται "timestampcolumn" για το χρόνο σειράς δεδομένων. 

Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Το φάκελο διαδρομή και το όνομα αρχείου για το αντικείμενο blob αξιολογούνται δυναμικά με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.
    
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


**Σωλήνωση με αντιγραφή δραστηριότητα:**

Της διοχέτευσης περιέχει ένα αντίγραφο δραστηριότητα που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελεστεί ανά ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **OracleSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**.  Το ερώτημα SQL που καθορίζεται με την ιδιότητα **oracleReaderQuery** επιλέγει την τελευταία ώρα για να αντιγράψετε τα δεδομένα.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Πρέπει να προσαρμόσετε τη συμβολοσειρά ερωτήματος με βάση τον τρόπο ημερομηνίες έχουν ρυθμιστεί οι παράμετροι στη βάση δεδομένων της Oracle. Εάν δείτε το ακόλουθο μήνυμα σφάλματος: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Ίσως χρειαστεί να αλλάξετε το ερώτημα, όπως φαίνεται στο ακόλουθο δείγμα (με χρήση της συνάρτησης to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Δείγμα: Αντιγράψτε δεδομένα από το Azure Blob Oracle
Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από το χώρο αποθήκευσης αντικειμένων Blob του Azure σε μια βάση δεδομένων Oracle εσωτερικής εγκατάστασης. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** από οποιοδήποτε από τα προελεύσεις αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ως προέλευσης [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) ως δέκτη.

Το δείγμα αντιγράφει δεδομένα από ένα blob σε έναν πίνακα σε μια βάση δεδομένων Oracle εσωτερικής κάθε ώρα. Για περισσότερες πληροφορίες σχετικά με τις διάφορες ιδιότητες που χρησιμοποιούνται στο δείγμα, ανατρέξτε στο θέμα τεκμηρίωση στις ενότητες ακολουθώντας τα δείγματα.

**Υπηρεσία Oracle συνδεδεμένες:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Χώρο αποθήκευσης Blob του Azure συνδεδεμένα υπηρεσίας:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob εισαγωγής συνόλου δεδομένων**

Δεδομένα είναι επιλέξατε προς τα επάνω από ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Το φάκελο διαδρομή και το όνομα αρχείου για το αντικείμενο blob αξιολογούνται δυναμικά με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνα και ημέρα τμήμα της ώρας έναρξης και το τμήμα ώρας της ώρας έναρξης χρησιμοποιεί το όνομα του αρχείου. "εξωτερική": η ρύθμιση "true" πληροφορεί την υπηρεσία εργοστασίου δεδομένων ότι αυτός ο πίνακας είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

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

**Σύνολο δεδομένων εξόδου Oracle:**

Το δείγμα προϋποθέτει ότι έχετε δημιουργήσει έναν πίνακα "Πίνακας" στο Oracle. Δημιουργία πίνακα στο Oracle με τον ίδιο αριθμό στηλών με τον αναμενόμενο τρόπο το αρχείο Blob CSV ώστε να περιέχει. Νέες γραμμές προστίθενται στον πίνακα κάθε ώρα.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Σωλήνωση με αντιγραφή δραστηριότητα:**

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **BlobSource** και τον τύπο **δέκτη** έχει οριστεί σε **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Ιδιότητες υπηρεσίας Oracle συνδεδεμένες

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία Oracle συνδεδεμένες JSON στοιχεία. 

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **OnPremisesOracle** | Ναι
συμβολοσειρά σύνδεσης | Καθορίστε πληροφορίες που απαιτούνται για να συνδεθείτε με την παρουσία βάση δεδομένων της Oracle για την ιδιότητα συμβολοσειρά σύνδεσης. | Ναι 
gatewayName | Όνομα της πύλης που που χρησιμοποιείται για να συνδεθείτε με το διακομιστή Oracle εσωτερικής εγκατάστασης | Ναι

Ανατρέξτε στο θέμα [Ρύθμιση διαπιστευτήρια και την ασφάλεια](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) για λεπτομέρειες σχετικά με τη ρύθμιση διαπιστευτήρια για μια προέλευση δεδομένων Oracle εσωτερικής εγκατάστασης.
## <a name="oracle-dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων Oracle

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (Oracle, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).
 
Στην ενότητα typeProperties είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου OracleTable περιλαμβάνει τις ακόλουθες ιδιότητες:

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
όνομα πίνακα | Το όνομα του πίνακα στη βάση δεδομένων Oracle που αναφέρεται στην υπηρεσία συνδεδεμένων. | Χωρίς (εάν έχει καθοριστεί **oracleReaderQuery** του **OracleSource** )

## <a name="oracle-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο Oracle

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και πολιτικής είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

> [AZURE.NOTE]
Η δραστηριότητα αντίγραφο λαμβάνει μία μόνο είσοδο και παράγει μόνο ένα αποτέλεσμα.

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

### <a name="oraclesource"></a>OracleSource
Στο αντίγραφο δραστηριότητα, όταν το αρχείο προέλευσης είναι τύπου **OracleSource** τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα **typeProperties** :

Ιδιότητα | Περιγραφή |Επιτρεπόμενη τιμή | Απαιτείται
-------- | ----------- | ------------- | --------
oracleReaderQuery | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος SQL. Για παράδειγμα: Επιλέξτε *από πίνακας <br/> <br/>Εάν δεν καθορίζονται, την πρόταση SQL που εκτελείται: Επιλέξτε* από πίνακας | Χωρίς (εάν έχει καθοριστεί **όνομα πίνακα** **στο σύνολο δεδομένων** )

### <a name="oraclesink"></a>OracleSink
**OracleSink** υποστηρίζει τις ακόλουθες ιδιότητες:

Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται
-------- | ----------- | -------------- | --------
writeBatchTimeout | Περιμένετε ώρα για τη λειτουργία εισαγωγής δέσμη για να ολοκληρωθεί πριν λήξει το χρονικό όριο. | χρονικό διάστημα<br/><br/> Παράδειγμα: 00:30:00 (30 λεπτά). | Όχι
writeBatchSize | Εισάγει δεδομένα στον πίνακα SQL, όταν το μέγεθος του buffer φτάσει writeBatchSize.   | Ακέραιος αριθμός (αριθμός γραμμών)| Δεν (προεπιλογή: 10000)  
sqlWriterCleanupScript | Καθορίστε ένα ερώτημα για αντιγραφή δραστηριότητα να εκτελέσει τέτοια ώστε η εκκαθάριση δεδομένων από ένα συγκεκριμένο κομμάτι. | Μια πρόταση ερωτήματος. | Όχι
sliceIdentifierColumnName | Καθορίστε όνομα στήλης για αντιγραφή δραστηριότητας για να συμπληρώσετε με αναγνωριστικό φέτα που δημιουργούνται αυτόματα, το οποίο χρησιμοποιείται για την εκκαθάριση δεδομένων από ένα συγκεκριμένο κομμάτι όταν εκτελέστε ξανά. | Όνομα στήλης της στήλης με τον τύπο δεδομένων binary(32). | Όχι


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Αντιστοίχιση τύπων για Oracle

Όπως αναφέρεται στην τη δραστηριότητα [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) άρθρο αντίγραφο πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω προσέγγιση βήμα 2:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Κατά τη μετακίνηση δεδομένων από Oracle, οι ακόλουθες αντιστοιχίσεις που χρησιμοποιούνται από τον τύπο δεδομένων Oracle στον τύπο .NET και το αντίστροφο.

Τύπος δεδομένων Oracle | Τύπος δεδομένων .NET framework
---------------- | ------------------------
BFILE | Byte]
BLOB | Byte]
ΚΑΤΆ ΈΝΑ ΧΑΡΑΚΤΉΡΑ | Συμβολοσειρά
CLOB | Συμβολοσειρά
ΗΜΕΡΟΜΗΝΊΑ | Ημερομηνίας και ώρας
ΑΙΏΡΗΣΗ | Δεκαδικών ψηφίων
ΑΚΈΡΑΙΟΣ ΑΡΙΘΜΌΣ | Δεκαδικών ψηφίων
ΔΙΆΣΤΗΜΑ ΈΤΟΣ, ΜΉΝΑ. | Int32
ΔΙΆΣΤΗΜΑ ΗΜΈΡΑ ΔΕΥΤΕΡΌΛΕΠΤΑ | Χρονικό διάστημα
ΜΕΓΆΛΗ | Συμβολοσειρά
ΧΡΌΝΟ ΑΝΕΠΕΞΈΡΓΑΣΤΑ | Byte]
NCHAR | Συμβολοσειρά
NCLOB | Συμβολοσειρά
ΑΡΙΘΜΌΣ | Δεκαδικών ψηφίων
NVARCHAR2 | Συμβολοσειρά
ΑΝΕΠΕΞΈΡΓΑΣΤΑ | Byte]
ID ΓΡΑΜΜΉΣ | Συμβολοσειρά
ΧΡΟΝΙΚΉ ΣΉΜΑΝΣΗ | Ημερομηνίας και ώρας
ΣΉΜΑΝΣΗ ΧΡΌΝΟΥ ΜΕ ΤΟΠΙΚΉ ΖΏΝΗ ΏΡΑΣ | Ημερομηνίας και ώρας
ΣΉΜΑΝΣΗ ΧΡΌΝΟΥ ΜΕ ΖΏΝΗΣ ΏΡΑΣ | Ημερομηνίας και ώρας
ΑΚΈΡΑΙΟΣ ΧΩΡΊΣ ΠΡΌΣΗΜΟ | Αριθμός
VARCHAR2 | Συμβολοσειρά
XML | Συμβολοσειρά

## <a name="troubleshooting-tips"></a>Συμβουλές αντιμετώπισης προβλημάτων

**Πρόβλημα:** Δείτε το εξής **μήνυμα σφάλματος**: αντιγραφή δραστηριότητας πληρούνται οι μη έγκυρες παράμετροι: 'UnknownParameterName', λεπτομερές μήνυμα: δεν είναι δυνατή η εύρεση ζητήθηκε .net Framework υπηρεσία παροχής δεδομένων. Αυτό μπορεί να μην έχει εγκατασταθεί".  

**Πιθανές αιτίες:**

1. Δεν έχει εγκατασταθεί το παροχής δεδομένων .NET Framework για Oracle.
2. Η παροχής δεδομένων .NET Framework για Oracle για .NET Framework 2.0 έχει εγκατασταθεί και δεν βρέθηκε στους φακέλους .NET Framework 4.0. 

**Ανάλυση/λύση:**

1. Εάν δεν έχετε εγκαταστήσει την υπηρεσία παροχής .NET για Oracle, [εγκαταστήστε την](http://www.oracle.com/technetwork/topics/dotnet/downloads/) και προσπαθήστε ξανά το σενάριο. 
2. Εάν λάβετε το μήνυμα σφάλματος ακόμα και μετά την εγκατάσταση της υπηρεσίας παροχής, κάντε τα εξής βήματα: 
    1. Άνοιγμα μηχανικής ρύθμισης παραμέτρων του .NET 2.0 από το φάκελο: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Αναζήτηση για την **Υπηρεσία παροχής δεδομένων Oracle για .NET**και που πρέπει να μπορείτε να βρείτε μια καταχώρηση, όπως φαίνεται στο το παρακάτω δείγμα unwn στις ακόλουθες δείγμα under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Αντιγραφή αυτή την καταχώρηση στο αρχείο machine.config στον παρακάτω φάκελο έκδοση 4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config και αλλάξτε την έκδοση για να 4.xxx.x.x.
3.  Εγκατάσταση "< διαδρομή εγκατεστημένες ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" στο καθολικό cache συγκροτήσεων (GAC), εκτελώντας `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.

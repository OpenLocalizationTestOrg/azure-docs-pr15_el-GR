<properties 
    pageTitle="Μετακίνηση δεδομένων από το Amazon Redshift χρησιμοποιώντας δεδομένα εργοστασίου | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μεταφέρετε δεδομένα από το Amazon Redshift χρησιμοποιώντας εργοστασίου δεδομένων Azure." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Μετακίνηση δεδομένων από το Amazon Redshift χρησιμοποιώντας εργοστασίου δεδομένων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων για την από το Amazon Redshift σε ένα άλλο δεδομένα αποθήκευση. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και μια λίστα με τις αποθηκεύει δεδομένα προέλευσης/δέκτη.  

Προέλευση δεδομένων υποστηρίζει επί του παρόντος μόνο κυλιόμενο τα δεδομένα από το Amazon Redshift σε άλλους χώρους αποθήκευσης, αλλά όχι για τη μετακίνηση δεδομένων από άλλες χώροι αποθήκευσης δεδομένων στο Amazon Redshift.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- Εάν θέλετε να μετακινήσετε δεδομένα σε ένα χώρο αποθήκευσης δεδομένων εσωτερικής εγκατάστασης, παραχωρήστε πύλη διαχείρισης δεδομένων (χρήση διεύθυνση IP του υπολογιστή) πρόσβαση σε σύμπλεγμα Amazon Redshift. Για οδηγίες, ανατρέξτε στο θέμα [εξουσιοδότηση για πρόσβαση στο σύμπλεγμα](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Εάν θέλετε να μετακινήσετε δεδομένα σε ένα χώρο αποθήκευσης Azure δεδομένων, ανατρέξτε στο θέμα [Περιοχές διευθύνσεων IP κέντρο δεδομένων Azure](https://www.microsoft.com/download/details.aspx?id=41653) για τις τον υπολογισμό περιοχές διευθύνσεων IP address (συμπεριλαμβανομένων των περιοχών SQL) χρησιμοποιούνται από το Windows Azure κέντρα δεδομένων. 

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από το Amazon Redshift είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Το παρακάτω παράδειγμα παρέχει ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Σας δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από το Amazon Redshift χώρο αποθήκευσης Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από το Amazon Redshift αντικειμένων Blob του Azure
Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια βάση δεδομένων Amazon Redshift χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

- Ένα συνδεδεμένο υπηρεσία του τύπου [AmazonRedshift](#linked-service-properties).
- Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [RelationalTable](#dataset-type-properties).
- Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [RelationalSource](#copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από το αποτέλεσμα του ερωτήματος στο Amazon Redshift ένα blob κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

**Υπηρεσία Redshift Amazon συνδεδεμένες**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Σύνολο δεδομένων εισαγωγής Amazon Redshift**

Ρύθμιση **"εξωτερική": true** πληροφορεί την υπηρεσία εργοστασίου δεδομένων που του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων. Ορίστε αυτήν την ιδιότητα στην τιμή true σε ένα σύνολο δεδομένων εισόδου που δεν παράγεται από μια δραστηριότητα στη διοχέτευση.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
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
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **RelationalSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που έχουν καθοριστεί για την ιδιότητα **ερωτήματος** επιλέγει την τελευταία ώρα για να αντιγράψετε τα δεδομένα.
    
    {
        "name": "CopyAmazonRedshiftToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonRedshiftInputDataset"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Ιδιότητες συνδεδεμένων υπηρεσίας

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία Redshift Amazon συνδεδεμένες JSON στοιχεία.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- | 
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **AmazonRedshift**. | Ναι |
| διακομιστή | IP address ή όνομα κεντρικού υπολογιστή του διακομιστή Amazon Redshift. | Ναι |
| θύρα | Ο αριθμός της θύρας TCP που χρησιμοποιεί ο διακομιστής Amazon Redshift για να ακούσετε για τις συνδέσεις του προγράμματος-πελάτη. | Όχι, η προεπιλεγμένη τιμή: 5439 |
| βάση δεδομένων | Το όνομα της βάσης δεδομένων Amazon Redshift. | Ναι |
| όνομα χρήστη | Το όνομα του χρήστη που έχει πρόσβαση στη βάση δεδομένων.| Ναι |
| κωδικός πρόσβασης | Ο κωδικός πρόσβασης για το λογαριασμό χρήστη.| Ναι |


## <a name="dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και πολιτικής είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **RelationalTable** (το οποίο περιλαμβάνει το σύνολο δεδομένων Amazon Redshift) περιλαμβάνει τις ακόλουθες ιδιότητες

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| όνομα πίνακα | Το όνομα του πίνακα στη βάση δεδομένων Amazon Redshift που αναφέρεται συνδεδεμένων υπηρεσιών. | Χωρίς (εάν έχει καθοριστεί **ερωτήματος** της **RelationalSource** ) | 

## <a name="copy-activity-type-properties"></a>Αντιγραφή ιδιότητες τύπου δραστηριότητας

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και οι πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα **typeProperties** της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Όταν δραστηριότητας αντίγραφο του αρχείου προέλευσης είναι τύπου **RelationalSource** (το οποίο περιλαμβάνει το Amazon Redshift) τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα typeProperties:

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| ερώτημα | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος SQL. Για παράδειγμα: Επιλέξτε * από πίνακας. | Χωρίς (εάν έχει καθοριστεί **όνομα πίνακα** **στο σύνολο δεδομένων** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Αντιστοίχιση τύπων για το Amazon Redshift

Όπως αναφέρθηκε στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , αντιγραφή δραστηριότητας πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω δύο βήματα προσέγγιση:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Κατά τη μετακίνηση δεδομένων Amazon Redshift, οι ακόλουθες αντιστοιχίσεις θα χρησιμοποιηθεί από τύπους Amazon Redshift στους τύπους .NET.

Τύπος Redshift Amazon | .NET, με βάση τον τύπο
-------------------- | ---------------
SMALLINT | Int16
ΑΚΈΡΑΙΟΣ ΑΡΙΘΜΌΣ | Int32
BIGINT | Int64
ΔΕΚΑΔΙΚΏΝ ΨΗΦΊΩΝ | Δεκαδικών ψηφίων
ΠΡΑΓΜΑΤΙΚΌ | Μεμονωμένο
ΔΙΠΛΉΣ ΑΚΡΊΒΕΙΑΣ | Δύο
ΔΥΑΔΙΚΉ ΤΙΜΉ | Συμβολοσειρά
ΚΑΤΆ ΈΝΑ ΧΑΡΑΚΤΉΡΑ | Συμβολοσειρά
VARCHAR | Συμβολοσειρά
ΗΜΕΡΟΜΗΝΊΑ | Ημερομηνίας και ώρας
ΧΡΟΝΙΚΉ ΣΉΜΑΝΣΗ | Ημερομηνίας και ώρας
ΚΕΊΜΕΝΟ | Συμβολοσειρά



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.

## <a name="next-steps"></a>Επόμενα βήματα
Ανατρέξτε στα ακόλουθα άρθρα: 
- [Πρόγραμμα εκμάθησης αντίγραφο δραστηριότητας](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) για οδηγίες βήμα προς βήμα για τη δημιουργία μιας διοχέτευσης με μια δραστηριότητα αντίγραφο. 
<properties 
    pageTitle="Μετακίνηση δεδομένων από προελεύσεις OData | Εργοστασιακές Azure δεδομένων" 
    description="Μάθετε πώς μπορείτε να μεταφέρετε δεδομένα από προελεύσεις OData χρησιμοποιώντας το Azure εργοστασίου δεδομένων." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Μετακίνηση δεδομένων από μια OData προέλευσης χρησιμοποιώντας εργοστασίου δεδομένων Azure
Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για να μεταφέρετε δεδομένα από μια προέλευση OData άλλο χώρο αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται.

> [AZURE.NOTE] Αυτό OData σύνδεσης με την υποστήριξη αντιγραφή δεδομένων από δύο cloud OData και προελεύσεις OData εσωτερικής εγκατάστασης. Για το τελευταίο, πρέπει να εγκαταστήσετε την πύλη διαχείρισης δεδομένων. Δείτε το άρθρο [Μετακίνηση δεδομένων μεταξύ της εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για λεπτομέρειες σχετικά με την πύλη διαχείρισης δεδομένων.

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από μια προέλευση OData είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Τα παρακάτω παραδείγματα παρέχουν ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα από μια προέλευση OData χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από αρχείο προέλευσης OData αντικειμένων Blob του Azure

Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια προέλευση OData χώρο αποθήκευσης Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OData](#odata-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [ODataResource](#odata-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [RelationalSource](#odata-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από την υποβολή ερωτημάτων σε σχέση με μια προέλευση OData σε μια αντικειμένων blob του Azure κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

**OData συνδεδεμένες υπηρεσίας** Αυτό το παράδειγμα χρησιμοποιεί το βασικό έλεγχο ταυτότητας. Ανατρέξτε στην ενότητα [OData συνδεδεμένες υπηρεσίας](#odata-linked-service-properties) για διαφορετικούς τύπους ελέγχου ταυτότητας που μπορείτε να χρησιμοποιήσετε. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**Σύνολο δεδομένων εισόδου OData**

Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

Καθορισμός **διαδρομής** στο dataset ορισμού είναι προαιρετικό. 


**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **RelationalSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που έχουν καθοριστεί για την ιδιότητα **ερωτήματος** επιλέγει τα πιο πρόσφατα δεδομένα (από το νεότερο) από την προέλευση OData.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


Καθορισμός **ερωτήματος** στον ορισμό διοχέτευσης είναι προαιρετικό. Είναι η **διεύθυνση URL** που χρησιμοποιεί η υπηρεσία εργοστασίου δεδομένων για την ανάκτηση δεδομένων: διεύθυνσης URL που καθορίστηκε στην υπηρεσία συνδεδεμένων (απαιτείται) + διαδρομή που καθορίζεται στο σύνολο δεδομένων (προαιρετικό) + ερωτήματος στη διοχέτευση (προαιρετικό). 

## <a name="odata-linked-service-properties"></a>OData συνδεδεμένες Ιδιότητες υπηρεσίας

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία OData συνδεδεμένες JSON στοιχεία.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- | 
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **OData** | Ναι |
| διεύθυνση URL| Διεύθυνση URL της υπηρεσίας OData. | Ναι |
| authenticationType | Τύπος ελέγχου ταυτότητας που χρησιμοποιείται για να συνδεθείτε στην προέλευση OData. <br/><br/> Για το σύννεφο OData, πιθανές τιμές είναι ανώνυμη και βασικές. Για OData εσωτερικής εγκατάστασης, πιθανές τιμές είναι ανώνυμος, Basic και Windows. | Ναι | 
| όνομα χρήστη | Καθορίστε το όνομα χρήστη, εάν χρησιμοποιείτε το βασικό έλεγχο ταυτότητας. | Ναι (μόνο εάν χρησιμοποιείτε το βασικό έλεγχο ταυτότητας) | 
| κωδικός πρόσβασης | Καθορίστε τον κωδικό πρόσβασης για το λογαριασμό χρήστη που έχετε καθορίσει για το όνομα χρήστη. | Ναι (μόνο εάν χρησιμοποιείτε το βασικό έλεγχο ταυτότητας) | 
| gatewayName | Όνομα της πύλης που πρέπει να χρησιμοποιήσετε την υπηρεσία εργοστασίου δεδομένων για να συνδεθείτε με την υπηρεσία OData εσωτερικής εγκατάστασης. Καθορίστε μόνο εάν αντιγράφετε δεδομένα από αρχείο προέλευσης OData εσωτερικής εγκατάστασης. | Όχι |

### <a name="using-basic-authentication"></a>Χρήση βασικού ελέγχου ταυτότητας

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Χρήση ανώνυμος έλεγχος ταυτότητας
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Χρήση του ελέγχου ταυτότητας των Windows πρόσβαση προέλευση OData εσωτερικής εγκατάστασης

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων OData

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **ODataResource** (το οποίο περιλαμβάνει το σύνολο δεδομένων OData) περιλαμβάνει τις ακόλουθες ιδιότητες

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| διαδρομή | Διαδρομή προς τον πόρο OData | Όχι | 

## <a name="odata-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο OData

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και πολιτικής είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Όταν προέλευσης είναι τύπου **RelationalSource** (το οποίο περιλαμβάνει OData) τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα typeProperties:

| Ιδιότητα | Περιγραφή | Παράδειγμα | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| ερώτημα | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | "; $select = όνομα, περιγραφή και $top = 5" | Όχι | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>Αντιστοίχιση τύπων για OData

Όπως αναφέρθηκε στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , αντιγραφή δραστηριότητας πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω προσέγγιση βήμα 2:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Όταν αποθηκεύει μετακίνηση δεδομένων από δεδομένων OData, τύποι δεδομένων του OData αντιστοιχίζονται σε τύπους .NET.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε σχετικά με τους παράγοντες κλειδιού ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.


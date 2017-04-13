<properties 
    pageTitle="Μετακίνηση δεδομένων από Teradata | Εργοστασιακές Azure δεδομένων" 
    description="Μάθετε περισσότερα σχετικά με Teradata σύνδεσης για την υπηρεσία εργοστασίου δεδομένων που σας επιτρέπει να μετακίνηση δεδομένων από βάση δεδομένων Teradata" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Μετακίνηση δεδομένων από χρησιμοποιώντας Azure εργοστασίου δεδομένων Teradata

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων σε από Teradata σε ένα άλλο δεδομένα store. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται.

Προέλευση δεδομένων υποστηρίζει τη σύνδεση με προελεύσεις εσωτερικής Teradata μέσω της πύλης διαχείρισης δεδομένων. Ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για να μάθετε περισσότερα σχετικά με την πύλη διαχείρισης δεδομένων και αναλυτικές οδηγίες σχετικά με τη ρύθμιση του της πύλης. 

> [AZURE.NOTE]
Πύλης απαιτείται ακόμα και αν το Teradata φιλοξενείται σε μια Εικονική IaaS Azure. Μπορείτε να εγκαταστήσετε την πύλη στο την ίδια εικονική Μηχανή IaaS ως ο χώρος αποθήκευσης δεδομένων ή σε μια διαφορετική Εικονική με την προϋπόθεση ότι η πύλη μπορούν να συνδεθούν με τη βάση δεδομένων. 

Προέλευση δεδομένων υποστηρίζει μόνο μετακίνηση δεδομένων από Teradata σε άλλους χώρους αποθήκευσης δεδομένων και όχι από άλλους χώρους αποθήκευσης για Teradata.

## <a name="installation"></a>Εγκατάσταση 

Για την πύλη διαχείρισης δεδομένων για να συνδεθείτε με τη βάση δεδομένων Teradata, πρέπει να εγκαταστήσετε την υπηρεσία [Παροχής δεδομένων .NET για Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) στο ίδιο σύστημα με την πύλη διαχείρισης δεδομένων.

> [AZURE.NOTE] Ανατρέξτε στο θέμα [πύλης αντιμετώπιση προβλημάτων](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) για συμβουλές σχετικά με την αντιμετώπιση προβλημάτων σύνδεσης/πύλης θέματα που σχετίζονται με. 

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από Teradata σε οποιονδήποτε από τους χώρους αποθήκευσης δεδομένων υποστηρίζονται δέκτη είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Το παρακάτω παράδειγμα παρέχει ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα από Teradata στο χώρο αποθήκευσης Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από Teradata αντικειμένων Blob του Azure

Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια βάση δεδομένων Teradata σε ένα χώρο αποθήκευσης Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  Της [διοχέτευσης](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από το αποτέλεσμα του ερωτήματος σε βάση δεδομένων Teradata ένα blob κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

Ως πρώτο βήμα, το πρόγραμμα εγκατάστασης της πύλης διαχείρισης δεδομένων. Οι οδηγίες είναι στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Υπηρεσία συνδεδεμένες Teradata:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Χώρο αποθήκευσης Blob του Azure συνδεδεμένα υπηρεσίας:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Σύνολο δεδομένων Teradata εισαγωγής:**

Το δείγμα προϋποθέτει που έχετε δημιουργήσει έναν πίνακα "Πίνακας" στο Teradata και περιέχει μια στήλη που ονομάζεται "χρονική σήμανση" για το χρόνο σειράς δεδομένων.

Ρύθμιση "εξωτερική": true ενημερώνει για την υπηρεσία εργοστασίου δεδομένων ότι ο πίνακας είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Σωλήνωση με αντιγραφή δραστηριότητα:**

Της διοχέτευσης περιέχει ένα αντίγραφο δραστηριότητα που έχει ρυθμιστεί να χρησιμοποιεί τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελεστεί ανά ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **RelationalSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που έχουν καθοριστεί για την ιδιότητα **ερωτήματος** επιλέγει την τελευταία ώρα για να αντιγράψετε τα δεδομένα.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Ιδιότητες υπηρεσίας συνδεδεμένες Teradata

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία συνδεδεμένες Teradata JSON στοιχεία. 

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **OnPremisesTeradata** | Ναι
διακομιστή | Το όνομα του διακομιστή Teradata. | Ναι
authenticationType | Τύπος ελέγχου ταυτότητας που χρησιμοποιείται για να συνδεθείτε με τη βάση δεδομένων Teradata. Οι πιθανές τιμές είναι: ανώνυμος, Basic και Windows. | Ναι
όνομα χρήστη | Καθορίστε το όνομα χρήστη, εάν χρησιμοποιείτε τον έλεγχο ταυτότητας Basic ή Windows. | Όχι 
κωδικός πρόσβασης | Καθορίστε τον κωδικό πρόσβασης για το λογαριασμό χρήστη που έχετε καθορίσει για το όνομα χρήστη. | Όχι 
gatewayName | Όνομα της πύλης που πρέπει να χρησιμοποιήσετε την υπηρεσία εργοστασίου δεδομένων για να συνδεθείτε με τη βάση δεδομένων Teradata εσωτερικής εγκατάστασης. | Ναι

Ανατρέξτε στο θέμα [Ρύθμιση διαπιστευτήρια και την ασφάλεια](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) για λεπτομέρειες σχετικά με τη ρύθμιση διαπιστευτήρια για μια προέλευση δεδομένων Teradata εσωτερικής εγκατάστασης.

## <a name="teradata-dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων Teradata

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Προς το παρόν, δεν υπάρχουν ιδιότητες τύπος υποστηρίζονται για το σύνολο δεδομένων Teradata. 


## <a name="teradata-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο Teradata

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και οι πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Όταν προέλευσης είναι τύπου **RelationalSource** (το οποίο περιλαμβάνει Teradata) τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα **typeProperties** :

Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται
-------- | ----------- | -------------- | --------
ερώτημα | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος SQL. Για παράδειγμα: Επιλέξτε * από πίνακας. | Ναι

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Αντιστοίχιση τύπων για Teradata

Όπως αναφέρθηκε στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , τη δραστηριότητα αντίγραφο πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω προσέγγιση βήμα 2:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Κατά τη μετακίνηση δεδομένων Teradata, οι ακόλουθες αντιστοιχίσεις που χρησιμοποιούνται από τον τύπο Teradata στον τύπο .NET.

Τύπος βάση δεδομένων Teradata | Τύπος .NET framework
----------------- | ---------------------------
Κατά ένα χαρακτήρα | Συμβολοσειρά
CLOB | Συμβολοσειρά
Γραφικό | Συμβολοσειρά
VarChar | Συμβολοσειρά
VarGraphic | Συμβολοσειρά
BLOB | Byte]
Byte | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Δεκαδικών ψηφίων | Δεκαδικών ψηφίων
Δύο | Δύο
Ακέραιος αριθμός | Int32
Αριθμός | Δύο
SmallInt | Int16
Ημερομηνία | Ημερομηνίας και ώρας
Ώρα | Χρονικό διάστημα
Το χρόνο με ζώνης ώρας | Συμβολοσειρά
Χρονική σήμανση | Ημερομηνίας και ώρας
Σήμανση χρόνου με ζώνης ώρας | DateTimeOffset
Διάστημα ημέρα | Χρονικό διάστημα
Διάστημα ημέρα σε ώρα | Χρονικό διάστημα
Διάστημα ημέρα λεπτό | Χρονικό διάστημα
Διάστημα ημέρα δευτερόλεπτα | Χρονικό διάστημα
Διάστημα ώρα | Χρονικό διάστημα
Διάστημα ώρα, λεπτό | Χρονικό διάστημα
Ώρα διάστημα δευτερόλεπτα | Χρονικό διάστημα
Λεπτό διάστημα | Χρονικό διάστημα
Λεπτό διάστημα δευτερόλεπτα | Χρονικό διάστημα
Διάστημα δεύτερο | Χρονικό διάστημα
Διάστημα έτος | Συμβολοσειρά
Διάστημα έτος, μήνα. | Συμβολοσειρά
Διάστημα μήνα | Συμβολοσειρά
Period(Date) | Συμβολοσειρά
Period(Time) | Συμβολοσειρά
Περίοδος (χρόνος με ζώνη ώρας) | Συμβολοσειρά
Period(timestamp) | Συμβολοσειρά
Περίοδος (χρονικής σήμανσης με ζώνη ώρας) | Συμβολοσειρά
XML | Συμβολοσειρά

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.
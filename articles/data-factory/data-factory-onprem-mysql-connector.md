<properties 
    pageTitle="Μετακίνηση δεδομένων από MySQL | Εργοστασιακές Azure δεδομένων" 
    description="Μάθετε σχετικά με τον τρόπο για τη μετακίνηση δεδομένων από βάση δεδομένων MySQL χρησιμοποιώντας Azure εργοστασίου δεδομένων." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Μετακίνηση δεδομένων από MySQL χρησιμοποιώντας εργοστασίου δεδομένων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για τη μετακίνηση δεδομένων σε από MySQL σε ένα άλλο δεδομένα store. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων με αντιγραφή δραστηριότητας και οι συνδυασμοί χώρου αποθήκευσης δεδομένων που υποστηρίζονται.

Υπηρεσία εργοστασίου δεδομένων υποστηρίζει τη σύνδεση με προελεύσεις MySQL εσωτερικής εγκατάστασης με την πύλη διαχείρισης δεδομένων. Ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για να μάθετε περισσότερα σχετικά με την πύλη διαχείρισης δεδομένων και αναλυτικές οδηγίες σχετικά με τη ρύθμιση του της πύλης. 

> [AZURE.NOTE] Πύλης απαιτείται ακόμα και αν η βάση δεδομένων MySQL φιλοξενείται σε μια εικονική μηχανή Azure IaaS (Εικονική). Μπορείτε να εγκαταστήσετε την πύλη στο το ίδιο Εικονική ως ο χώρος αποθήκευσης δεδομένων ή σε μια διαφορετική Εικονική με την προϋπόθεση ότι η πύλη μπορούν να συνδεθούν με τη βάση δεδομένων.  

Προέλευση δεδομένων υποστηρίζει επί του παρόντος μόνο δεδομένα κυλιόμενο από MySQL σε άλλους χώρους αποθήκευσης, αλλά όχι για τη μετακίνηση δεδομένων από άλλες αποθηκεύει δεδομένα στο MySQL.

## <a name="installation"></a>Εγκατάσταση 
Για την πύλη διαχείρισης δεδομένων για να συνδεθείτε με τη βάση δεδομένων MySQL, πρέπει να εγκαταστήσετε το [MySQL σύνδεσης/καθαρή 6.6.5 για τα Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) στο ίδιο σύστημα με την πύλη διαχείρισης δεδομένων.

> [AZURE.NOTE] Ανατρέξτε στο θέμα [πύλης αντιμετώπιση προβλημάτων](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) για συμβουλές σχετικά με την αντιμετώπιση προβλημάτων σύνδεσης/πύλης θέματα που σχετίζονται με. 

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από μια βάση δεδομένων MySQL σε οποιονδήποτε από τους χώρους αποθήκευσης δεδομένων υποστηρίζονται δέκτη είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Το παρακάτω παράδειγμα παρέχει ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία. Για μια πλήρη ανάλυση με οδηγίες βήμα προς βήμα, ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) . Δεδομένα μπορεί να αντιγραφεί σε οποιοδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από MySQL αντικειμένων Blob του Azure
Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια βάση δεδομένων MySQL εσωτερικής εγκατάστασης σε ένα χώρο αποθήκευσης Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  

> [AZURE.IMPORTANT] Αυτό το δείγμα παρέχει JSON τμήματα κώδικα. Δεν περιλαμβάνει οδηγίες βήμα προς βήμα για τη δημιουργία την προέλευση δεδομένων. Ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για οδηγίες βήμα προς βήμα. 
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από το αποτέλεσμα του ερωτήματος σε βάση δεδομένων MySQL ένα blob ανά ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

Ως πρώτο βήμα, το πρόγραμμα εγκατάστασης της πύλης διαχείρισης δεδομένων. Οι οδηγίες είναι στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**Υπηρεσία MySQL συνδεδεμένες**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**Σύνολο εισαγωγής δεδομένων MySQL**

Το δείγμα προϋποθέτει που έχετε δημιουργήσει έναν πίνακα "Πίνακας" στο MySQL και περιέχει μια στήλη που ονομάζεται "timestampcolumn" για το χρόνο σειράς δεδομένων.

Ρύθμιση "εξωτερική": "true" ενημερώνει για την υπηρεσία εργοστασίου δεδομένων ότι ο πίνακας είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
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



**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyMySqlToBlob",
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
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Ιδιότητες υπηρεσίας MySQL συνδεδεμένες

Ο παρακάτω πίνακας παρέχει περιγραφή για τη συγκεκριμένη υπηρεσία MySQL συνδεδεμένες JSON στοιχεία.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- | 
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **OnPremisesMySql** | Ναι |
| διακομιστή | Το όνομα του διακομιστή MySQL. | Ναι |
| βάση δεδομένων | Το όνομα της βάσης δεδομένων MySQL. | Ναι | 
| σχήμα  | Το όνομα του σχήματος στη βάση δεδομένων. | Όχι | 
| authenticationType | Τύπος ελέγχου ταυτότητας που χρησιμοποιείται για να συνδεθείτε με τη βάση δεδομένων MySQL. Οι πιθανές τιμές είναι: ανώνυμος, Basic και Windows. | Ναι | 
| όνομα χρήστη | Καθορίστε το όνομα χρήστη, εάν χρησιμοποιείτε τον έλεγχο ταυτότητας Basic ή Windows. | Όχι | 
| κωδικός πρόσβασης | Καθορίστε τον κωδικό πρόσβασης για το λογαριασμό χρήστη που έχετε καθορίσει για το όνομα χρήστη. | Όχι | 
| gatewayName | Όνομα της πύλης που πρέπει να χρησιμοποιήσετε την υπηρεσία εργοστασίου δεδομένων για να συνδεθείτε με τη βάση δεδομένων MySQL εσωτερικής εγκατάστασης. | Ναι |

Ανατρέξτε στο θέμα [Ρύθμιση διαπιστευτήρια και την ασφάλεια](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) για λεπτομέρειες σχετικά με τη ρύθμιση διαπιστευτήρια για μια προέλευση δεδομένων MySQL εσωτερικής εγκατάστασης.

## <a name="mysql-dataset-type-properties"></a>Ιδιότητες τύπου συνόλου δεδομένων MySQL

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **RelationalTable** (το οποίο περιλαμβάνει το σύνολο δεδομένων MySQL) περιλαμβάνει τις ακόλουθες ιδιότητες

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- |
| όνομα πίνακα | Το όνομα του πίνακα στην παρουσία βάση δεδομένων MySQL που αναφέρεται συνδεδεμένων υπηρεσιών. | Χωρίς (εάν έχει καθοριστεί **ερωτήματος** της **RelationalSource** ) | 

## <a name="mysql-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο MySQL

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, πίνακες εισόδου και εξόδου, είναι πολιτικές που είναι διαθέσιμα για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα **typeProperties** της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Όταν είναι προέλευση στη δραστηριότητα αντιγραφή του τύπου **RelationalSource** (το οποίο περιλαμβάνει MySQL) τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα typeProperties:

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| ερώτημα | Χρησιμοποιήστε το προσαρμοσμένο ερώτημα για την ανάγνωση δεδομένων. | Συμβολοσειρά ερωτήματος SQL. Για παράδειγμα: Επιλέξτε * από πίνακας. | Χωρίς (εάν έχει καθοριστεί **όνομα πίνακα** **στο σύνολο δεδομένων** ) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Αντιστοίχιση τύπων για MySQL

Όπως αναφέρθηκε στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) , αντιγραφή δραστηριότητας πραγματοποιεί μετατροπές αυτόματων τύπου από τους τύπους προέλευσης αποδοχή τους τύπους με τα παρακάτω δύο βήματα προσέγγιση:

1. Μετατροπή από τους τύπους προέλευσης εγγενής σε τύπο .NET
2. Μετατροπή από τον τύπο .NET σε τύπο εγγενούς δέκτη

Κατά τη μετακίνηση δεδομένων MySQL, οι ακόλουθες αντιστοιχίσεις που χρησιμοποιούνται από τους τύπους MySQL στους τύπους .NET.

| Τύπος δεδομένων MySQL | Τύπος .NET framework |
| ------------------- | ------------------- | 
| υπογραφή bigint | Δεκαδικών ψηφίων |
| bigint | Int64 |
| bit | Δεκαδικών ψηφίων |
| BLOB | Byte] |
| bool |  Δυαδική τιμή | 
| CHAR | Συμβολοσειρά |
| ημερομηνία | Ημερομηνίας και ώρας |
| ημερομηνίας και ώρας | Ημερομηνίας και ώρας |
| δεκαδικών ψηφίων | Δεκαδικών ψηφίων |
| διπλής ακρίβειας | Δύο |
| διπλά | Δύο |
| Απαρίθμηση | Συμβολοσειρά |
| Αιώρηση | Μεμονωμένο |
| Int υπογραφή | Int64 |
| Int | Int32 |
| Ακέραιος χωρίς πρόσημο | Int64 |
| Ακέραιος αριθμός | Int32 | 
| μεγάλη varbinary | Byte] |
| μεγάλη varchar | Συμβολοσειρά |
| longblob | Byte] |
| τύπου LONGTEXT | Συμβολοσειρά | 
| mediumblob |  Byte] | 
| υπογραφή mediumint | Int64 |
| mediumint | Int32 | 
| mediumtext | Συμβολοσειρά |
| αριθμητική | Δεκαδικών ψηφίων |
| πραγματικό |  Δύο |
| Ορισμός | Συμβολοσειρά |
| υπογραφή smallint | Int32 |
| smallint | Int16 |
| κείμενο | Συμβολοσειρά |
| ώρα | Χρονικό διάστημα |
| χρονική σήμανση | Ημερομηνίας και ώρας |
| tinyblob | Byte] |
| υπογραφή tinyint |  Int16 | 
| tinyint | Int16 |
| tinytext | Συμβολοσειρά | 
| varchar | Συμβολοσειρά | 
| έτος | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.


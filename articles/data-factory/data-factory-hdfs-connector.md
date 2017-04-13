<properties 
    pageTitle="Μετακίνηση δεδομένων από εσωτερικής HDFS | Εργοστασιακές Azure δεδομένων" 
    description="Μάθετε πώς μπορείτε να μεταφέρετε δεδομένα από HDFS εσωτερικής εγκατάστασης με χρήση εργοστασίου δεδομένων Azure." 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Μετακίνηση δεδομένων από HDFS εσωτερικής εγκατάστασης με χρήση εργοστασίου δεδομένων Azure
Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του ενός εργοστασίου Azure δεδομένων για να μεταφέρετε δεδομένα από μια HDFS στην εσωτερική εγκατάσταση άλλου χώρου αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) που παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων δραστηριότητας αντίγραφο και οι συνδυασμοί αποθήκευσης δεδομένων που υποστηρίζονται.

Προέλευση δεδομένων υποστηρίζει επί του παρόντος μόνο κυλιόμενο τα δεδομένα από μια HDFS εσωτερικής εγκατάστασης σε άλλους χώρους αποθήκευσης, αλλά όχι για τη μετακίνηση δεδομένων από άλλες χώροι αποθήκευσης δεδομένων σε μια HDFS εσωτερικής εγκατάστασης.


## <a name="enabling-connectivity"></a>Ενεργοποίηση της συνδεσιμότητας
Υπηρεσία εργοστασίου δεδομένων υποστηρίζει τη σύνδεση σε HDFS εσωτερικής εγκατάστασης με χρήση της πύλης διαχείρισης δεδομένων. Ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για να μάθετε περισσότερα σχετικά με την πύλη διαχείρισης δεδομένων και αναλυτικές οδηγίες σχετικά με τη ρύθμιση του της πύλης. Χρήση της πύλης για να συνδεθείτε στο HDFS, ακόμα και εάν φιλοξενείται σε μια Εικονική IaaS Azure. 

Ενώ μπορείτε να εγκαταστήσετε την πύλη στον ίδιο υπολογιστή εσωτερικής εγκατάστασης ή η Εικονική Azure ως το HDFS, συνιστάται να εγκαταστήσετε την πύλη σε ένα ξεχωριστό υπολογιστή/Azure IaaS Εικονική. Αντιμετωπίζετε πύλης σε ξεχωριστό υπολογιστή μειώνει διένεξης πόρων και βελτιώνει τις επιδόσεις. Κατά την εγκατάσταση της πύλης σε νέο υπολογιστή, πρέπει να είναι μπορούν να έχουν πρόσβαση σε υπολογιστή με το HDFS υπολογιστή. 


## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από HDFS εσωτερικής εγκατάστασης είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Τα παρακάτω παραδείγματα παρέχουν ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Εμφανίζονται πώς μπορείτε να αντιγράψετε δεδομένα από μια HDFS εσωτερικής εγκατάστασης στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, μπορούν να αντιγραφούν δεδομένα σε οποιαδήποτε από τα δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από HDFS εσωτερικής εγκατάστασης σε αντικειμένων Blob του Azure

Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από μια HDFS εσωτερικής εγκατάστασης στο χώρο αποθήκευσης Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

1.  Ένα συνδεδεμένο υπηρεσία του τύπου [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [κοινόχρηστο αρχείο που](#hdfs-dataset-type-properties).
4.  Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [FileSystemSource](#hdfs-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από μια HDFS εσωτερικής εγκατάστασης σε ένα αντικειμένων blob του Azure κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

Ως πρώτο βήμα, ρυθμίστε την πύλη διαχείρισης δεδομένων. Τις οδηγίες στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS συνδεδεμένες υπηρεσίας** Σε αυτό το παράδειγμα χρησιμοποιεί τον έλεγχο ταυτότητας των Windows. Ανατρέξτε στην ενότητα [HDFS συνδεδεμένες υπηρεσίας](#hdfs-linked-service-properties) για διαφορετικούς τύπους ελέγχου ταυτότητας που μπορείτε να χρησιμοποιήσετε. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS εισαγωγής συνόλου δεδομένων** Σε αυτό το σύνολο δεδομένων αναφέρεται στο φάκελο HDFS DataTransfer/UnitTest /. Η διαδικασία αντιγράφει όλα τα αρχεία σε αυτόν το φάκελο στον προορισμό. 

Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Η διαδικασία περιέχει δραστηριότητα αντίγραφο που έχει ρυθμιστεί ώστε να χρησιμοποιήσετε αυτά τα σύνολα δεδομένων εισόδου και εξόδου και έχει προγραμματιστεί να εκτελείται κάθε ώρα. Στη διοχέτευση ορισμού JSON, ο τύπος **προέλευσης** έχει οριστεί σε **FileSystemSource** και **δέκτη** τύπος έχει οριστεί σε **BlobSink**. Το ερώτημα SQL που έχουν καθοριστεί για την ιδιότητα **ερωτήματος** επιλέγει την τελευταία ώρα για να αντιγράψετε τα δεδομένα.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Ιδιότητες HDFS συνδεδεμένων υπηρεσίας

Ο παρακάτω πίνακας παρέχει περιγραφή για στοιχεία JSON ειδικά για HDFS συνδεδεμένες υπηρεσίας.

| Ιδιότητα | Περιγραφή | Απαιτείται |
| -------- | ----------- | -------- | 
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν: **Hdfs** | Ναι | 
| Διεύθυνση URL | Διεύθυνση URL για την HDFS | Ναι |
| encryptedCredential | [Δημιουργία AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) αποτέλεσμα τα διαπιστευτήρια της access. | Όχι |
| όνομα χρήστη | Έλεγχος ταυτότητας όνομα χρήστη για Windows. | Ναι (για έλεγχο ταυτότητας των Windows)
| κωδικός πρόσβασης | Κωδικός πρόσβασης για έλεγχο ταυτότητας των Windows. | Ναι (για έλεγχο ταυτότητας των Windows)
| authenticationType | Windows, ή ανώνυμη. | Ναι |
| gatewayName | Όνομα της πύλης που πρέπει να χρησιμοποιήσετε την υπηρεσία εργοστασίου δεδομένων για να συνδεθείτε με το HDFS. | Ναι |   

Ανατρέξτε στο θέμα [Ρύθμιση διαπιστευτήρια και την ασφάλεια](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) για λεπτομέρειες σχετικά με τη ρύθμιση των διαπιστευτηρίων για HDFS εσωτερικής εγκατάστασης.

### <a name="using-anonymous-authentication"></a>Χρήση ανώνυμος έλεγχος ταυτότητας

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Χρήση του ελέγχου ταυτότητας των Windows
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Ιδιότητες τύπου HDFS συνόλου δεδομένων

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό σύνολα δεδομένων, ανατρέξτε στο άρθρο [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md) . Ενότητες όπως δομή, διαθεσιμότητα και την πολιτική από ένα σύνολο δεδομένων JSON είναι παρόμοια για όλους τους τύπους συνόλου δεδομένων (SQL Azure, αντικειμένων blob του Azure, πινάκων του Azure, κ.λπ.).

Στην ενότητα **typeProperties** είναι διαφορετικές για κάθε τύπο του συνόλου δεδομένων και παρέχει πληροφορίες σχετικά με τη θέση των δεδομένων στο χώρο αποθήκευσης δεδομένων. Στην ενότητα typeProperties για το σύνολο δεδομένων του τύπου **κοινόχρηστο αρχείο που** (το οποίο περιλαμβάνει το σύνολο δεδομένων HDFS) περιλαμβάνει τις ακόλουθες ιδιότητες

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
folderPath | Διαδρομή προς το φάκελο. Παράδειγμα:`myfolder`<br/><br/>Χρήση χαρακτήρα διαφυγής ' \ ' για ειδικούς χαρακτήρες της συμβολοσειράς. Για παράδειγμα: folder\subfolder, καθορίστε το φάκελο\\\\υποφάκελο και d:\samplefolder, καθορίστε δ:\\\\ΔείγμαΦακέλου.<br/><br/>Μπορείτε να συνδυάσετε αυτήν την ιδιότητα με **partitionBy** να έχουν διαδρομές φακέλων που βασίζονται σε φέτα Έναρξη/Λήξη ημερομηνία-ώρα. | Ναι
όνομα αρχείου | Καθορίστε το όνομα του αρχείου στο το **folderPath** εάν θέλετε ο πίνακας για να ανατρέξετε σε ένα συγκεκριμένο αρχείο στο φάκελο. Εάν δεν καθορίσετε οποιαδήποτε τιμή για αυτήν την ιδιότητα, τον πίνακα οδηγεί σε όλα τα αρχεία στο φάκελο.<br/><br/>Όταν δεν έχει καθοριστεί όνομα αρχείου για ένα σύνολο δεδομένων εξόδου, το όνομα του αρχείου που έχει δημιουργηθεί πρέπει να είναι τα εξής αυτήν τη μορφή: <br/><br/>Τα δεδομένα. <Guid>.txt (για παράδειγμα:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Όχι
partitionedBy | partitionedBy μπορεί να χρησιμοποιηθεί για να καθορίσετε μια δυναμική folderPath, όνομα αρχείου για χρόνο σειράς δεδομένων. Παράδειγμα: folderPath με παραμέτρους για κάθε ώρα των δεδομένων. | Όχι
fileFilter | Καθορίστε ένα φίλτρο που θα χρησιμοποιηθεί για να επιλέξετε ένα υποσύνολο των αρχείων στο το folderPath αντί για όλα τα αρχεία. <br/><br/>Επιτρέπονται τιμές είναι: `*` (πολλοί χαρακτήρες) και `?` (μεμονωμένος χαρακτήρας).<br/><br/>Παραδείγματα 1:`"fileFilter": "*.log"`<br/>Παράδειγμα 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Σημείωση**: εφαρμόζεται fileFilter για μια εισαγωγής dataset κοινόχρηστο αρχείο που | Όχι
| μορφή | Υποστηρίζονται οι παρακάτω τύποι μορφοποίησης: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**και **ParquetFormat**. Ορίστε την ιδιότητα **τύπου** στην περιοχή μορφοποίηση σε μία από αυτές τις τιμές. Ανατρέξτε στο θέμα [Καθορίζοντας TextFormat](#specifying-textformat), [Καθορίζοντας AvroFormat](#specifying-avroformat), [Καθορίζοντας JsonFormat](#specifying-jsonformat), [Καθορίζοντας OrcFormat](#specifying-orcformat)και [Καθορίζοντας ParquetFormat](#specifying-parquetformat) ενότητες για λεπτομέρειες. Εάν θέλετε να αντιγράψετε τα αρχεία ως-είναι μεταξύ βασίζεται στο αρχείο αποθηκεύονται (δυαδικό αντίγραφο), μπορείτε να μεταβείτε στην ενότητα μορφή σε δύο ορισμών συνόλου δεδομένων εισόδου και εξόδου. | Όχι 
| συμπίεση | Καθορίστε τον τύπο και το επίπεδο συμπίεσης για τα δεδομένα. Υποστηριζόμενοι τύποι είναι: **GZip**, **Deflate**, και **BZip2** και υποστηριζόμενα επίπεδα είναι: **βέλτιστη** και **πιο γρήγορη**. Προς το παρόν, οι ρυθμίσεις συμπίεσης δεν υποστηρίζονται για δεδομένα σε **AvroFormat** ή **OrcFormat**. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [υποστήριξη συμπίεσης](#compression-support) .  | Όχι |



> [AZURE.NOTE] όνομα αρχείου και fileFilter δεν μπορούν να χρησιμοποιηθούν ταυτόχρονα.


### <a name="using-partionedby-property"></a>Χρησιμοποιώντας την ιδιότητα partionedBy

Όπως αναφέρθηκε στην προηγούμενη ενότητα, μπορείτε να καθορίσετε μια δυναμική folderPath, όνομα αρχείου για χρόνο σειρές δεδομένων με partitionedBy. Μπορείτε να το κάνετε με τις μακροεντολές δεδομένων εργοστασίου και η μεταβλητή SliceStart, SliceEnd που υποδεικνύουν τη λογική χρονική περίοδο για μια δεδομένη δεδομένων φέτα. 

Για να μάθετε περισσότερα σχετικά με τα σύνολα δεδομένων σειράς ώρα, τον προγραμματισμό και φέτες, ανατρέξτε στο θέμα άρθρα [Δημιουργία συνόλων δεδομένων](data-factory-create-datasets.md), [Προγραμματισμός & εκτέλεση](data-factory-scheduling-and-execution.md)και [Τη δημιουργία αγωγούς](data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Παράδειγμα 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Σε αυτό το παράδειγμα {φέτα} έχει αντικατασταθεί με την τιμή της μεταβλητής συστήματος εργοστασίου δεδομένων SliceStart στη μορφή (YYYYMMDDHH) που καθορίζεται. Το SliceStart αναφέρεται σε ώρα έναρξης της στη φέτα. Το folderPath είναι διαφορετικό για κάθε φέτα. Για παράδειγμα: wikisampledataout/wikidatagateway/2014100103 ή wikisampledataout/wikidatagateway/2014100104.

#### <a name="sample-2"></a>Παράδειγμα 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Σε αυτό το παράδειγμα, το έτος, μήνας, ημέρα και ώρα της SliceStart εξάγονται σε ξεχωριστές μεταβλητές που χρησιμοποιούνται από τις ιδιότητες folderPath και το όνομα αρχείου.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο HDFS

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και οι πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τους ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

Για αντιγραφή δραστηριότητα, όταν προέλευσης είναι τύπου **FileSystemSource** τις ακόλουθες ιδιότητες είναι διαθέσιμες στην ενότητα typeProperties:

**FileSystemSource** υποστηρίζει τις ακόλουθες ιδιότητες:

| Ιδιότητα | Περιγραφή | Επιτρεπόμενη τιμή | Απαιτείται |
| -------- | ----------- | -------------- | -------- |
| περιοδικότητας | Υποδεικνύει αν τα δεδομένα είναι ανάγνωση σταδιακά από τους φακέλους sub ή μόνο από τον καθορισμένο φάκελο. | TRUE, False (προεπιλογή)| Όχι |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.


<properties 
    pageTitle="Μετακίνηση δεδομένων από το διακομιστή FTP | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να μεταφέρετε δεδομένα από ένα διακομιστή FTP χρησιμοποιώντας εργοστασίου δεδομένων Azure." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2016" 
    ms.author="spelluru"/>

# <a name="move-data-from-an-ftp-server-using-azure-data-factory"></a>Μετακίνηση δεδομένων από ένα διακομιστή FTP χρησιμοποιώντας εργοστασίου δεδομένων Azure
Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων για να μεταφέρετε δεδομένα από ένα διακομιστή FTP μια υποστηριζόμενη δέκτη χώρου αποθήκευσης δεδομένων. Σε αυτό το άρθρο δημιουργεί στο άρθρο [δραστηριότητες κίνηση δεδομένων](data-factory-data-movement-activities.md) που παρουσιάζει μια γενική επισκόπηση κίνηση δεδομένων δραστηριότητα αντιγραφή και τη λίστα των χώροι αποθήκευσης δεδομένων που υποστηρίζονται ως προελεύσεις/δέκτες. 

Προέλευση δεδομένων υποστηρίζει επί του παρόντος μόνο κυλιόμενο τα δεδομένα από ένα διακομιστή FTP σε άλλους χώρους αποθήκευσης, αλλά όχι για τη μετακίνηση δεδομένων από άλλες αποθηκεύει δεδομένα σε ένα διακομιστή FTP. Υποστηρίζει δύο εσωτερικής εγκατάστασης και cloud διακομιστές FTP. 

Εάν θέλετε να μετακινήσετε δεδομένα από ένα διακομιστή **εσωτερικής εγκατάστασης** FTP ένα χώρο αποθήκευσης δεδομένων στο cloud (παράδειγμα: χώρος αποθήκευσης αντικειμένων Blob του Azure), εγκατάσταση και χρήση πύλης διαχείρισης δεδομένων. Η πύλη διαχείρισης δεδομένων είναι έναν παράγοντα προγράμματος-πελάτη που είναι εγκατεστημένη στον υπολογιστή σας στην εσωτερική εγκατάσταση, το οποίο επιτρέπει τις υπηρεσίες cloud για να συνδεθείτε με τον πόρο στην εσωτερική εγκατάσταση. Για λεπτομέρειες σχετικά με την πύλη, ανατρέξτε στο θέμα [Η πύλη διαχείρισης δεδομένων](data-factory-data-management-gateway.md) . Ανατρέξτε στο άρθρο [Μετακίνηση δεδομένων μεταξύ τοποθεσιών εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για οδηγίες βήμα προς βήμα στη ρύθμιση της πύλης και να τη χρησιμοποιείτε. Μπορείτε να χρησιμοποιήσετε την πύλη για να συνδεθείτε σε ένα διακομιστή FTP, ακόμα και εάν ο διακομιστής είναι σε μια εικονική μηχανή Azure IaaS (Εικονική). 

Μπορείτε να εγκαταστήσετε την πύλη στον ίδιο υπολογιστή εσωτερικής εγκατάστασης ή η Εικονική IaaS Azure ως διακομιστή FTP. Ωστόσο, συνιστάται να εγκαταστήσετε την πύλη σε έναν νέο υπολογιστή ή μια ξεχωριστή Εικονική IaaS Azure για να αποφύγετε διένεξης πόρων και για καλύτερες επιδόσεις. Κατά την εγκατάσταση της πύλης σε νέο υπολογιστή, ο υπολογιστής πρέπει να είναι μπορούν να έχουν πρόσβαση στο διακομιστή FTP. 

## <a name="copy-data-wizard"></a>Αντιγραφή δεδομένων οδηγού
Ο ευκολότερος τρόπος για να δημιουργήσετε μια διαδικασία που αντιγράφει δεδομένα από ένα διακομιστή FTP είναι να χρησιμοποιήσετε τον Οδηγό δεδομένων αντιγράφου. Ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: δημιουργία μιας διοχέτευσης χρήση οδηγού αντιγραφής](data-factory-copy-data-wizard-tutorial.md) για γρήγορη αναλυτικές οδηγίες σχετικά με τη δημιουργία μιας διοχέτευσης χρησιμοποιώντας τον Οδηγό δεδομένων αντιγράφου. 

Τα παρακάτω παραδείγματα παρέχουν ορισμούς JSON δείγμα που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διαδικασία χρησιμοποιώντας την [πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). 

## <a name="sample-copy-data-from-ftp-server-to-azure-blob"></a>Δείγμα: Αντιγράψτε δεδομένα από το διακομιστή FTP αντικειμένων blob του Azure

Αυτό το δείγμα δείχνει πώς μπορείτε να αντιγράψετε δεδομένα από ένα διακομιστή FTP στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Ωστόσο, δεδομένων μπορεί να είναι αντίγραφο **απευθείας** σε οποιεσδήποτε από το δέκτες αναφέρεται [εδώ](data-factory-data-movement-activities.md#supported-data-stores) χρησιμοποιώντας τη δραστηριότητα αντίγραφο του Azure εργοστασίου δεδομένων.  
 
Το δείγμα περιλαμβάνει τα παρακάτω οντοτήτων εργοστασίου δεδομένων:

- Ένα συνδεδεμένο υπηρεσία του τύπου [FtpServer](#ftp-linked-service-properties).
- Ένα συνδεδεμένο υπηρεσία του τύπου [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Μια εισαγωγής [σύνολο δεδομένων](data-factory-create-datasets.md) του τύπου [κοινόχρηστο αρχείο που](#fileshare-dataset-type-properties).
- Ένα αποτέλεσμα του [συνόλου δεδομένων](data-factory-create-datasets.md) του τύπου [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Μια [διαδικασία](data-factory-create-pipelines.md) με αντιγραφή δραστηριότητα που χρησιμοποιεί [FileSystemSource](#ftp-copy-activity-type-properties) και [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Το δείγμα αντιγράφει δεδομένα από ένα διακομιστή FTP μια αντικειμένων blob του Azure κάθε ώρα. Οι ιδιότητες JSON που χρησιμοποιούνται σε αυτά τα δείγματα περιγράφονται στις ενότητες ακολουθώντας τα δείγματα. 

**FTP συνδεδεμένες υπηρεσίας** Αυτό το παράδειγμα χρησιμοποιεί το βασικό έλεγχο ταυτότητας με το όνομα χρήστη και τον κωδικό πρόσβασης σε απλό κείμενο. Μπορείτε επίσης να χρησιμοποιήσετε έναν από τους εξής τρόπους: 

- Ανώνυμος έλεγχος ταυτότητας 
- Βασικό έλεγχο ταυτότητας με κρυπτογραφημένο διαπιστευτήρια
- FTP μέσω SSL/TLS (FTPS)

Ανατρέξτε στην ενότητα [FTP συνδεδεμένες υπηρεσίας](#ftp-linked-service-properties) για διαφορετικούς τύπους ελέγχου ταυτότητας που μπορείτε να χρησιμοποιήσετε. 

    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "myftpserver.com",          
            "authenticationType": "Basic",
            "username": "Admin",
            "password": "123456"
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

**Σύνολο δεδομένων εισαγωγής FTP** Σε αυτό το σύνολο δεδομένων αναφέρεται στο φάκελο FTP `mysharedfolder` και αρχείο `test.csv`. Η διαδικασία αντιγράφει το αρχείο στον προορισμό. 

Ρύθμιση "εξωτερική": "true" ενημερώνει την υπηρεσία εργοστασίου δεδομένων ότι του συνόλου δεδομένων είναι εξωτερική την προέλευση δεδομένων και δεν παράγεται από δραστηριότητα την προέλευση δεδομένων.
    
    {
      "name": "FTPFileInput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": "FTPLinkedService",
        "typeProperties": {
          "folderPath": "mysharedfolder",
          "fileName": "test.csv",
          "useBinaryTransfer": true
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }


**Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure**

Είναι η εγγραφή δεδομένων σε ένα νέο blob κάθε ώρα (συχνότητα: ώρα, διάστημα: 1). Η διαδρομή φακέλου για το αντικείμενο blob δυναμικά αξιολογείται με βάση την ώρα έναρξης του στη φέτα που υποβάλλεται σε επεξεργασία. Η διαδρομή του φακέλου χρησιμοποιεί έτος, μήνας, ημέρα και ώρες τμήματα της ώρας έναρξης.

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/ftp/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "pipeline",
        "properties": {
            "activities": [{
                "name": "FTPToBlobCopy",
                "inputs": [{
                    "name": "FtpFileInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource"
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
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "00:05:00"
                }
            }],
            "start": "2016-08-24T18:00:00Z",
            "end": "2016-08-24T19:00:00Z"
        }
    }

## <a name="ftp-linked-service-properties"></a>Ιδιότητες συνδεδεμένων υπηρεσίας FTP

Ο παρακάτω πίνακας παρέχει περιγραφή για στοιχεία JSON ειδικά για FTP συνδεδεμένων υπηρεσιών.

| Ιδιότητα | Περιγραφή | Απαιτείται | Προεπιλεγμένη |
| -------- | ----------- | -------- | ------- | 
| Τύπος | Η ιδιότητα τύπος πρέπει να οριστούν σε FtpServer | Ναι | &nbsp;
| κεντρικός υπολογιστής | Όνομα ή τη διεύθυνση IP του διακομιστή FTP | Ναι | &nbsp;
| authenticationType | Καθορισμός Τύπος ελέγχου ταυτότητας | Ναι | Βασικός, ανώνυμος |
| όνομα χρήστη | Χρήστης που έχει πρόσβαση στο διακομιστή FTP | Όχι | &nbsp;
| κωδικός πρόσβασης | Κωδικός πρόσβασης για το χρήστη (όνομα χρήστη) | Όχι | &nbsp;
| encryptedCredential | Κρυπτογραφημένο διαπιστευτηρίων για να αποκτήσετε πρόσβαση στο διακομιστή FTP | Όχι | &nbsp;
| gatewayName | Όνομα της πύλης διαχείρισης δεδομένων πύλης για να συνδεθείτε σε ένα διακομιστή FTP εσωτερικής εγκατάστασης | Όχι    | &nbsp;
| θύρα | Κατά την οποία ο διακομιστής FTP λαμβάνει θύρα | Όχι | 21 |
| enableSsl | Καθορίστε εάν θα χρησιμοποιήσετε FTP μέσω καναλιού SSL/TLS | Όχι | TRUE | 
| enableServerCertificateValidation | Καθορίστε εάν θέλετε να ενεργοποιήσετε επικύρωσης πιστοποιητικού SSL διακομιστή κατά τη χρήση FTP μέσω καναλιού SSL/TLS | Όχι | TRUE | 

### <a name="using-anonymous-authentication"></a>Χρήση ανώνυμος έλεγχος ταυτότητας

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {     
                "authenticationType": "Anonymous",
                "host": "myftpserver.com"
            }
        }
    }

### <a name="using-username-and-password-in-plain-text-for-basic-authentication"></a>Χρησιμοποιώντας το όνομα χρήστη και τον κωδικό πρόσβασης σε απλό κείμενο για βασικό έλεγχο ταυτότητας
    
    {
        "name": "FTPLinkedService",
        "properties": {
        "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "username": "Admin",
                "password": "123456"
            }
        }
    }


### <a name="using-port-enablessl-enableservercertificatevalidation"></a>Χρήση θύρα, enableSsl, enableServerCertificateValidation

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",  
                "username": "Admin",
                "password": "123456",
                "port": "21",
                "enableSsl": true,
                "enableServerCertificateValidation": true
            }
        }
    }

### <a name="using-encryptedcredential-for-authentication-and-gateway"></a>Με χρήση του encryptedCredential για τον έλεγχο ταυτότητας και πύλης

    {
        "name": "FTPLinkedService",
        "properties": {
            "type": "FtpServer",
            "typeProperties": {
                "host": "myftpserver.com",
                "authenticationType": "Basic",
                "encryptedCredential": "xxxxxxxxxxxxxxxxx",
                "gatewayName": "mygateway"
            }
        }
    }

Ανατρέξτε στο θέμα [Ρύθμιση διαπιστευτήρια και την ασφάλεια](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) για λεπτομέρειες σχετικά με τη ρύθμιση διαπιστευτήρια για μια προέλευση δεδομένων εσωτερικής εγκατάστασης FTP.

[AZURE.INCLUDE [data-factory-file-share-dataset](../../includes/data-factory-file-share-dataset.md)]   
[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="ftp-copy-activity-type-properties"></a>Ιδιότητες τύπου δραστηριότητας αντίγραφο FTP

Για μια πλήρη λίστα των ενοτήτων & ιδιότητες που είναι διαθέσιμες για τον ορισμό δραστηριότητες, ανατρέξτε στο άρθρο [Δημιουργία αγωγούς](data-factory-create-pipelines.md) . Ιδιότητες, όπως όνομα, περιγραφή, εισόδου και εξόδου πίνακες και οι πολιτικές είναι διαθέσιμες για όλους τους τύπους δραστηριοτήτων. 

Ιδιότητες που είναι διαθέσιμες στην ενότητα typeProperties της δραστηριότητας διαφέρει από την άλλη πλευρά με κάθε τύπο δραστηριότητας. Για αντιγραφή δραστηριότητα, τις ιδιότητες τύπος ποικίλλουν ανάλογα με τους τύπους προελεύσεων και δέκτες.

[AZURE.INCLUDE [data-factory-file-system-source](../../includes/data-factory-file-system-source.md)]   

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Απόδοσης και της ρύθμισης  
Ανατρέξτε στο θέμα [Αντιγραφή δραστηριότητας επιδόσεων και τον Οδηγό ρύθμισης](data-factory-copy-activity-performance.md) για να μάθετε περισσότερα σχετικά με τις βασικές παράγοντες ότι η απόδοση επίδραση κίνηση δεδομένων (αντιγραφή δραστηριότητα) στο Azure δεδομένων εργοστασίου και διάφορους τρόπους για να βελτιστοποιήσετε την.

## <a name="next-steps"></a>Επόμενα βήματα
Ανατρέξτε στα ακόλουθα άρθρα: 

- [Πρόγραμμα εκμάθησης αντίγραφο δραστηριότητας](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) για οδηγίες βήμα προς βήμα για τη δημιουργία μιας διοχέτευσης με μια δραστηριότητα αντίγραφο. 

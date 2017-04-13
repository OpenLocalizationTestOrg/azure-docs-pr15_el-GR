<properties 
    pageTitle="Hadoop ροής δραστηριότητας" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα ροή Hadoop σε μια εργοστασίου Azure δεδομένων για την εκτέλεση προγραμμάτων Hadoop ροής σε ένα στην-ζήτηση/η δική σύμπλεγμα HDInsight." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoop ροής δραστηριότητας
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)

Μπορείτε να χρησιμοποιήσετε τη δραστηριότητα HDInsightStreamingActivity κλήση μιας εργασίας ροής Hadoop από μια διοχέτευση Azure εργοστασίου δεδομένων. Το παρακάτω τμήμα κώδικα JSON εμφανίζει τη σύνταξη για τη χρήση του HDInsightStreamingActivity σε ένα αρχείο JSON διοχέτευσης. 

HDInsight ροής δραστηριότητας σε μια προέλευση δεδομένων [διοχέτευσης](data-factory-create-pipelines.md) εκτελεί ροή Hadoop προγράμματα σε [το δικό σας](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή [σε ζήτηση](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) σύμπλεγμα HDInsight που βασίζεται σε Windows/Linux. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες μετασχηματισμό δεδομένων](data-factory-data-transformation-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση του μετασχηματισμού δεδομένων και τις δραστηριότητες υποστηριζόμενες μετασχηματισμό.

## <a name="json-sample"></a>Δείγμα JSON
Το HDInsight σύμπλεγμα συμπληρώνεται αυτόματα με τα προγράμματα παράδειγμα (wc.exe και cat.exe) και δεδομένα (davinci.txt). Από προεπιλογή, το όνομα του κοντέινερ που χρησιμοποιείται από το HDInsight σύμπλεγμα είναι το όνομα του συμπλέγματος ίδια. Για παράδειγμα, εάν το όνομα συμπλέγματος είναι myhdicluster, το όνομα του κοντέινερ αντικειμένων blob που έχει συσχετιστεί θα είναι myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Λάβετε υπόψη τα εξής σημεία:

1. Ορίστε το **linkedServiceName** στο όνομα της συνδεδεμένων υπηρεσίας που δείχνει το HDInsight σύμπλεγμά σας στον οποίο εκτελείται η ροή εργασίας mapreduce.
2. Ορισμός του τύπου της δραστηριότητας για να **HDInsightStreaming**.
3. Για την ιδιότητα **αντιστοίχισης** , καθορίστε το όνομα του προγράμματος αντιστοίχισης εκτελέσιμο. Στο παράδειγμα, cat.exe είναι το εκτελέσιμο πρόγραμμα αντιστοίχισης.
4. Για την ιδιότητα **reducer** , καθορίστε το όνομα του reducer εκτελέσιμο. Στο παράδειγμα, wc.exe είναι το εκτελέσιμο reducer.
5. Η ιδιότητα type **εισαγωγής** , καθορίστε το αρχείο εισόδου (συμπεριλαμβανομένης της θέσης) για το πρόγραμμα αντιστοίχισης. Στο παράδειγμα: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample είναι το κοντέινερ αντικειμένων blob, δεδομένων/παράδειγμα/Gutenberg είναι ο φάκελος και davinci.txt είναι το αντικείμενο blob.
6. Για την ιδιότητα τύπος **εξόδου** , καθορίστε το αρχείο εξόδου (συμπεριλαμβανομένης της θέσης) για το reducer. Το αποτέλεσμα της Hadoop η ροή εργασίας είναι γραμμένο στη θέση που καθορίζεται για αυτήν την ιδιότητα.
7. Στην ενότητα **filePaths** , καθορίστε τις διαδρομές για το πρόγραμμα αντιστοίχισης και reducer τα εκτελέσιμα. Στο παράδειγμα: "adfsample/example/apps/wc.exe", adfsample είναι το κοντέινερ αντικειμένων blob, παράδειγμα/εφαρμογές είναι ο φάκελος και wc.exe είναι το εκτελέσιμο αρχείο.
8. Για την ιδιότητα **fileLinkedService** , καθορίστε την υπηρεσία αποθήκευσης Azure συνδεδεμένες που αντιπροσωπεύει το Azure χώρου αποθήκευσης που περιέχει τα αρχεία που καθορίζονται στην ενότητα filePaths.
9. Για την ιδιότητα **ορίσματα** , καθορίστε τα ορίσματα για τη ροή εργασίας.
10. Η ιδιότητα **getDebugInfo** είναι ένα προαιρετικό στοιχείο. Όταν έχει ρυθμιστεί ώστε να την αποτυχία, γίνεται λήψη των αρχείων καταγραφής μόνο σε περίπτωση αποτυχίας. Όταν έχει οριστεί σε όλους, αρχεία καταγραφής γίνεται λήψη πάντα ανεξάρτητα από την κατάσταση εκτέλεσης.

> [AZURE.NOTE] Όπως φαίνεται στο παράδειγμα, μπορείτε να καθορίσετε ένα σύνολο δεδομένων εξόδου για τη δραστηριότητα ροή Hadoop για την ιδιότητα **εξόδους** . Σε αυτό το σύνολο δεδομένων είναι απλώς μια εικονική συνόλου δεδομένων που απαιτείται για καθοδηγούν το χρονοδιάγραμμα διοχέτευσης. Δεν χρειάζεται να καθορίσετε οποιαδήποτε εισαγωγής σύνολο δεδομένων για τη δραστηριότητα για την ιδιότητα **εισόδων** .  

    
## <a name="example"></a>Παράδειγμα
Της διοχέτευσης σε αυτές τις οδηγίες εκτελείται το πρόγραμμα αντιστοίχιση/μείωση καταμέτρηση λέξεων ροής σε το σύμπλεγμά σας Azure HDInsight. 

### <a name="linked-services"></a>Συνδεδεμένες υπηρεσίες

#### <a name="azure-storage-linked-service"></a>Υπηρεσία αποθήκευσης συνδεδεμένες του Azure
Πρώτα, δημιουργήστε ένα συνδεδεμένο υπηρεσίας για να συνδέσετε το χώρο αποθήκευσης Azure που χρησιμοποιείται από το Azure HDInsight σύμπλεγμα για την προέλευση δεδομένων Azure. Εάν έχετε αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα, μην ξεχάσετε να αντικαταστήσετε το όνομα του λογαριασμού και το κλειδί λογαριασμού με το όνομα και το κλειδί της αποθήκευσης του Azure. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight συνδεδεμένες υπηρεσίας
Στη συνέχεια, μπορείτε να δημιουργήσετε ένα συνδεδεμένο υπηρεσίας για να συνδέσετε το σύμπλεγμά σας Azure HDInsight με την προέλευση δεδομένων Azure. Εάν έχετε αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα, αντικαταστήστε το όνομα συμπλέγματος HDInsight με το όνομα του συμπλέγματος HDInsight και αλλάξτε τιμές όνομα και τον κωδικό πρόσβασης χρήστη. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Σύνολα δεδομένων

#### <a name="output-dataset"></a>Σύνολο δεδομένων εξόδου
Η διαδικασία σε αυτό το παράδειγμα δεν λαμβάνει οποιαδήποτε δεδομένα εισόδου. Μπορείτε να καθορίσετε ένα σύνολο δεδομένων εξόδου για τη δραστηριότητα ροή HDInsight. Σε αυτό το σύνολο δεδομένων είναι απλώς μια εικονική συνόλου δεδομένων που απαιτείται για καθοδηγούν το χρονοδιάγραμμα διοχέτευσης. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Διοχέτευση

Η διαδικασία σε αυτό το παράδειγμα έχει μόνο μία δραστηριότητα που είναι τύπου: **HDInsightStreaming**. 

Το HDInsight σύμπλεγμα συμπληρώνεται αυτόματα με τα προγράμματα παράδειγμα (wc.exe και cat.exe) και δεδομένα (davinci.txt). Από προεπιλογή, το όνομα του κοντέινερ που χρησιμοποιείται από το HDInsight σύμπλεγμα είναι το όνομα του συμπλέγματος ίδια. Για παράδειγμα, εάν το όνομα συμπλέγματος είναι myhdicluster, το όνομα του κοντέινερ αντικειμένων blob που έχει συσχετιστεί θα είναι myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Δείτε επίσης
- [Ομάδα δραστηριότητας](data-factory-hive-activity.md)
- [Γουρούνι δραστηριότητας](data-factory-pig-activity.md)
- [MapReduce δραστηριότητας](data-factory-map-reduce.md)
- [Κλήση τους προγράμματα](data-factory-spark.md)
- [Κλήση R δέσμες ενεργειών](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


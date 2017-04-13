<properties 
    pageTitle="Ενεργοποίηση προγράμματος MapReduce από εργοστασίου Azure δεδομένων" 
    description="Μάθετε πώς να επεξεργάζονται δεδομένα, εκτελώντας MapReduce προγράμματα σε ένα σύμπλεγμα Azure HDInsight από ένα εργοστασίου Azure δεδομένων." 
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Κλήση MapReduce προγραμμάτων από εργοστασίου δεδομένων
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)

Τη δραστηριότητα HDInsight MapReduce σε μια προέλευση δεδομένων [διοχέτευσης](data-factory-create-pipelines.md) εκτελεί προγράμματα MapReduce σε [το δικό σας](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή [σε ζήτηση](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) σύμπλεγμα HDInsight που βασίζεται σε Windows/Linux. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες μετασχηματισμό δεδομένων](data-factory-data-transformation-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση του μετασχηματισμού δεδομένων και τις δραστηριότητες υποστηριζόμενες μετασχηματισμό.

## <a name="introduction"></a>Εισαγωγή 
Μια διαδικασία σε μια προέλευση δεδομένων Azure επεξεργάζεται τα δεδομένα στις υπηρεσίες συνδεδεμένων χώρου αποθήκευσης με χρήση συνδεδεμένων υπολογισμού των υπηρεσιών. Περιέχει μια ακολουθία δραστηριοτήτων όπου κάθε δραστηριότητα εκτελεί μια συγκεκριμένη επεξεργασίας. Σε αυτό το άρθρο περιγράφει τη χρήση της δραστηριότητας MapReduce HDInsight.
 
Ανατρέξτε στο θέμα [γουρούνι](data-factory-pig-activity.md) και [ομάδας](data-factory-hive-activity.md) για λεπτομέρειες σχετικά με την εκτέλεση γουρούνι/Hive δέσμες ενεργειών σε ένα σύμπλεγμα HDInsight που βασίζεται σε Windows/Linux από μια διαδικασία με τη χρήση των δραστηριοτήτων γουρούνι HDInsight και ομάδα. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON για τη δραστηριότητα HDInsight MapReduce 

Στον ορισμό JSON για τη δραστηριότητα HDInsight: 
 
1. Ορισμός του **τύπου** της **δραστηριότητας** με το **HDInsight**.
3. Καθορίστε το όνομα της κατηγορίας για την ιδιότητα **όνομα κλάσης** .
4. Καθορίστε τη διαδρομή προς το αρχείο ΒΆΖΟ, συμπεριλαμβανομένου του ονόματος αρχείου για την ιδιότητα **jarFilePath** .
5. Καθορίστε την υπηρεσία συνδεδεμένων που αναφέρεται σε το χώρο αποθήκευσης αντικειμένων Blob Azure που περιέχει το αρχείο ΒΆΖΟ για την ιδιότητα **jarLinkedService** .   
6. Καθορίστε κάποια ορίσματα για το πρόγραμμα MapReduce στην ενότητα **ορίσματα** . Κατά το χρόνο εκτέλεσης, δείτε μερικά επιπλέον ορίσματα (για παράδειγμα: mapreduce.job.tags) από το πλαίσιο MapReduce. Για να διαφοροποιήσετε σας ορίσματα με τα ορίσματα MapReduce, εξετάστε το ενδεχόμενο χρήσης επιλογή και τιμή ως ορίσματα, όπως φαίνεται στο παρακάτω παράδειγμα (- s,--εισαγωγής,--εξόδου κ.λπ., είναι οι επιλογές αμέσως ακολουθούμενο από τις τιμές τους).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Μπορείτε να χρησιμοποιήσετε τη δραστηριότητα MapReduce HDInsight για να εκτελέσετε οποιοδήποτε αρχείο βάζο MapReduce σε ένα σύμπλεγμα HDInsight. Στον παρακάτω δείγμα JSON ορισμό δίκτυο αγωγών, τη δραστηριότητα HDInsight έχει ρυθμιστεί ώστε να εκτελέσετε ένα αρχείο ΒΆΖΩΝ Mahout.

## <a name="sample-on-github"></a>Δείγμα σε GitHub
Μπορείτε να κάνετε λήψη ενός δείγματος για τη χρήση τη δραστηριότητα MapReduce HDInsight από: [Δείγματα εργοστασίου δεδομένων σε GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Εκτέλεση του προγράμματος καταμέτρηση λέξεων
Τη διαδικασία σε αυτό το παράδειγμα εκτελείται το πρόγραμμα αντιστοίχιση/μείωση καταμέτρηση λέξεων στη το σύμπλεγμά σας Azure HDInsight.   

### <a name="linked-services"></a>Συνδεδεμένες υπηρεσίες
Πρώτα, δημιουργήστε ένα συνδεδεμένο υπηρεσίας για να συνδέσετε το χώρο αποθήκευσης Azure που χρησιμοποιείται από το Azure HDInsight σύμπλεγμα για την προέλευση δεδομένων Azure. Εάν έχετε αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα, μην ξεχάσετε να αντικαταστήσετε **το όνομα του λογαριασμού** και το **κλειδί λογαριασμού** με το όνομα και το κλειδί της αποθήκευσης του Azure. 

#### <a name="azure-storage-linked-service"></a>Υπηρεσία αποθήκευσης συνδεδεμένες του Azure

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
Στη συνέχεια, μπορείτε να δημιουργήσετε ένα συνδεδεμένο υπηρεσίας για να συνδέσετε το σύμπλεγμά σας Azure HDInsight με την προέλευση δεδομένων Azure. Εάν έχετε αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα, αντικαταστήστε **το όνομα συμπλέγματος HDInsight** με το όνομα του συμπλέγματος HDInsight και αλλάξτε τιμές όνομα και τον κωδικό πρόσβασης χρήστη.   

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
Η διαδικασία σε αυτό το παράδειγμα δεν λαμβάνει οποιαδήποτε δεδομένα εισόδου. Μπορείτε να καθορίσετε ένα σύνολο δεδομένων εξόδου για τη δραστηριότητα MapReduce HDInsight. Σε αυτό το σύνολο δεδομένων είναι απλώς μια εικονική συνόλου δεδομένων που απαιτείται για καθοδηγούν το χρονοδιάγραμμα διοχέτευσης.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Διοχέτευση
Η διαδικασία σε αυτό το παράδειγμα έχει μόνο μία δραστηριότητα που είναι τύπου: HDInsightMapReduce. Ορισμένες σημαντικές ιδιότητες στο το JSON είναι οι εξής: 

Ιδιότητα | Σημειώσεις
:-------- | :-----
Τύπος | Ο τύπος πρέπει να οριστεί στην **HDInsightMapReduce**. 
όνομα κλάσης | Το όνομα της κατηγορίας είναι: **wordcount**
jarFilePath | Διαδρομή προς το αρχείο βάζο που περιέχει την κλάση. Εάν έχετε αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα, μην ξεχάσετε να αλλάξετε το όνομα του συμπλέγματος. 
jarLinkedService | Azure υπηρεσία αποθήκευσης στο συνδεδεμένο που περιέχει το αρχείο βάζο. Αυτή η υπηρεσία συνδεδεμένων αναφέρεται το χώρο αποθήκευσης που σχετίζεται με το σύμπλεγμα HDInsight. 
ορίσματα | Το πρόγραμμα wordcount λαμβάνει δύο ορίσματα, ένα εισόδου και το αποτέλεσμα. Το αρχείο εισόδου είναι το αρχείο davinci.txt.
συχνότητα/διάστημα | Οι τιμές για αυτές τις ιδιότητες ταιριάζουν του συνόλου δεδομένων εξόδου. 
linkedServiceName | αναφέρεται στην υπηρεσία HDInsight συνδεδεμένες είχατε δημιουργήσει προηγουμένως.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Εκτέλεση προγραμμάτων τους
Μπορείτε να χρησιμοποιήσετε MapReduce δραστηριότητας για να εκτελέσετε τους προγράμματα σε το σύμπλεγμά σας HDInsight τους. Για λεπτομέρειες, ανατρέξτε στο θέμα [προγράμματα κλήση τους από το Azure εργοστασίου δεδομένων](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Δείτε επίσης
- [Ομάδα δραστηριότητας](data-factory-hive-activity.md)
- [Γουρούνι δραστηριότητας](data-factory-pig-activity.md)
- [Hadoop ροής δραστηριότητας](data-factory-hadoop-streaming-activity.md)
- [Κλήση τους προγράμματα](data-factory-spark.md)
- [Κλήση R δέσμες ενεργειών](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

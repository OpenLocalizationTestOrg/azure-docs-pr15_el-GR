<properties 
    pageTitle="Ομάδα δραστηριότητας" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα Hive σε μια εργοστασίου Azure δεδομένων για την εκτέλεση ερωτημάτων ομάδας σε ένα στην-ζήτηση/η δική σύμπλεγμα HDInsight." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Ομάδα δραστηριότητας
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)

Στην ομάδα HDInsight δραστηριότητα σε μια προέλευση δεδομένων [διοχέτευσης](data-factory-create-pipelines.md) εκτελεί Hive ερωτήματα σε σύμπλεγμα HDInsight που βασίζεται σε Windows/Linux [το δικό σας](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή [σε ζήτηση](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες μετασχηματισμό δεδομένων](data-factory-data-transformation-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση του μετασχηματισμού δεδομένων και τις δραστηριότητες υποστηριζόμενες μετασχηματισμό.

## <a name="syntax"></a>Σύνταξη

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Σύνταξη λεπτομέρειες

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Όνομα | Όνομα της δραστηριότητας | Ναι
Περιγραφή | Κείμενο που περιγράφει τι χρησιμεύει η δραστηριότητα | Όχι
Τύπος | HDinsightHive | Ναι
εισόδων | Η ομάδα δραστηριότητας | Όχι
εξόδους | Εξόδους που δημιουργήθηκαν με τη δραστηριότητα ομάδας | Ναι 
linkedServiceName | Αναφορά στο σύμπλεγμα HDInsight που έχει καταχωρηθεί ως συνδεδεμένο υπηρεσίας στην προέλευση δεδομένων | Ναι 
δέσμη ενεργειών | Καθορίστε τα ενσωματωμένα Hive δέσμης ενεργειών | Όχι
διαδρομή δέσμης ενεργειών | Αποθηκεύστε τη δέσμη ενεργειών ομάδα στο χώρο αποθήκευσης αντικειμένων blob του Azure και δώστε τη διαδρομή προς το αρχείο. Χρησιμοποιήστε την ιδιότητα 'δέσμη ενεργειών' ή 'scriptPath'. Και τα δύο δεν μπορεί να χρησιμοποιηθεί μαζί. Το όνομα του αρχείου είναι διάκριση πεζών-κεφαλαίων. | Όχι 
Ορίζει | Καθορίστε τις παραμέτρους ως ζεύγη κλειδιού/τιμής για αναφορά μέσα στη δέσμη ενεργειών Hive χρησιμοποιώντας 'hiveconf'  | Όχι

## <a name="example"></a>Παράδειγμα

Ας δούμε ένα παράδειγμα παιχνιδιών αρχεία καταγραφής αναλυτικών στοιχείων όπου θέλετε να προσδιορίσετε το χρόνο από τους χρήστες αναπαραγωγή αγώνων ξεκινά από την εταιρεία σας. 

Το παρακάτω αρχείο καταγραφής είναι ένα δείγμα παιχνιδιών, δηλαδή με κόμματα (`,`) διαχωρισμένες και περιέχει τα ακόλουθα πεδία – ProfileID, SessionStart, διάρκεια, SrcIPAddress και GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Το **Hive δέσμη ενεργειών** για την επεξεργασία αυτών των δεδομένων:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Για να εκτελέσετε αυτήν τη δέσμη ενεργειών Hive στη διοχέτευση μια προέλευση δεδομένων, πρέπει να κάνετε τα εξής

1. Δημιουργήστε μια συνδεδεμένων υπηρεσία για να καταχωρήσετε [τη δική σας HDInsight τον υπολογισμό σύμπλεγμα](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή να ρυθμίσετε τις παραμέτρους [σε απαιτήσεων HDInsight, τον υπολογισμό σύμπλεγμα](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Ας καλέστε αυτήν την υπηρεσία συνδεδεμένων "HDInsightLinkedService".
2. Δημιουργία [συνδεδεμένων υπηρεσιών](data-factory-azure-blob-connector.md) για να ρυθμίσετε τη σύνδεση με το χώρο αποθήκευσης αντικειμένων Blob του Azure φιλοξενούν τα δεδομένα. Ας καλέσετε αυτήν την υπηρεσία συνδεδεμένων "StorageLinkedService"
3. Δημιουργία [συνόλων δεδομένων](data-factory-create-datasets.md) που δείχνει προς τα δεδομένα εισόδου και τα δεδομένα εξόδου. Ας κλήσεων του συνόλου δεδομένων εισόδου "HiveSampleIn" και του συνόλου δεδομένων εξόδου "HiveSampleOut"
4. Αντιγραφή του ερωτήματος Hive ως αρχείο στο χώρο αποθήκευσης Blob του Azure έχει ρυθμιστεί στο βήμα #2. Εάν το χώρο αποθήκευσης για τη φιλοξενία των δεδομένων είναι διαφορετικό από αυτό φιλοξενίας αυτού του αρχείου ερωτήματος, δημιουργήστε μια ξεχωριστή υπηρεσία αποθήκευσης Azure συνδεδεμένες και αναφέρεστε σε αυτό σε τη δραστηριότητα. Χρησιμοποιήστε **scriptPath **για να καθορίσετε τη διαδρομή προς το αρχείο ερωτήματος ομάδας και **scriptLinkedService** για να καθορίσετε το Azure χώρου αποθήκευσης που περιέχει το αρχείο δέσμης ενεργειών. 

    > [AZURE.NOTE] Μπορείτε επίσης να παρέχετε τα ενσωματωμένα δέσμης ενεργειών Hive στον ορισμό δραστηριότητας, χρησιμοποιώντας την ιδιότητα **δέσμης ενεργειών** . Προσπαθούμε να δεν συνιστάται αυτή η προσέγγιση ως όλοι οι ειδικοί χαρακτήρες της δέσμης ενεργειών μέσα στις ανάγκες JSON εγγράφου να είναι διαφυγής και μπορεί να προκαλέσει ζητήματα εντοπισμού σφαλμάτων. Η βέλτιστη πρακτική είναι να ακολουθήσετε το βήμα #4.
5.  Δημιουργήστε μια διαδικασία με τη δραστηριότητα HDInsightHive. Η δραστηριότητα διεργασίες/μετασχηματισμούς τα δεδομένα.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Ανάπτυξη της διοχέτευσης. Ανατρέξτε στο θέμα [Δημιουργία αγωγούς](data-factory-create-pipelines.md) άρθρου για λεπτομέρειες. 
7.  Παρακολούθηση της διοχέτευσης χρησιμοποιώντας τα μοντέλα δεδομένων εργοστασίου παρακολούθησης και διαχείρισης. Ανατρέξτε στο θέμα [Παρακολούθηση και διαχείριση δεδομένων εργοστασίου αγωγούς](data-factory-monitor-manage-pipelines.md) άρθρου για λεπτομέρειες. 


## <a name="specifying-parameters-for-a-hive-script"></a>Καθορίζει τις παραμέτρους για μια ομάδα δέσμη ενεργειών  
Σε αυτό το παράδειγμα, αρχεία καταγραφής παιχνιδιών κατάποση καθημερινά στο χώρο αποθήκευσης Blob του Azure και είναι αποθηκευμένες σε ένα φάκελο διαμερίσματα με ημερομηνία και ώρα. Θέλετε να ρυθμίσετε τις παραμέτρους των τη δέσμη ενεργειών της ομάδας και μεταβιβάζουν δυναμικά τη θέση του φακέλου εισαγωγής κατά τη διάρκεια του χρόνου εκτέλεσης και παράγει επίσης το αποτέλεσμα διαμερίσματα με ημερομηνία και ώρα.

Για να χρησιμοποιήσετε με παραμέτρους δέσμης ενεργειών της ομάδας, κάντε τα εξής

- Μπορείτε να ορίσετε τις παραμέτρους με **ορίζει**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- Στη δέσμη ενεργειών Hive, ανατρέξτε στην παράμετρο χρησιμοποιώντας **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Δείτε επίσης
- [Γουρούνι δραστηριότητας](data-factory-pig-activity.md)
- [MapReduce δραστηριότητας](data-factory-map-reduce.md)
- [Hadoop ροής δραστηριότητας](data-factory-hadoop-streaming-activity.md)
- [Κλήση τους προγράμματα](data-factory-spark.md)
- [Κλήση R δέσμες ενεργειών](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)










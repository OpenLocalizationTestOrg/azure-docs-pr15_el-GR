<properties 
    pageTitle="Γουρούνι δραστηριότητας" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα γουρούνι σε μια εργοστασίου Azure δεδομένων για να εκτελούν δέσμες ενεργειών γουρούνι σε ένα στην-ζήτηση/η δική σύμπλεγμα HDInsight." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Γουρούνι δραστηριότητας
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)

Τη δραστηριότητα γουρούνι HDInsight σε μια προέλευση δεδομένων [διοχέτευσης](data-factory-create-pipelines.md) εκτελεί γουρούνι ερωτήματα σε [το δικό σας](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή [σε ζήτηση](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) σύμπλεγμα HDInsight που βασίζεται σε Windows/Linux. Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες μετασχηματισμό δεδομένων](data-factory-data-transformation-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση του μετασχηματισμού δεδομένων και τις δραστηριότητες υποστηριζόμενες μετασχηματισμό.

## <a name="syntax"></a>Σύνταξη

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Σύνταξη λεπτομέρειες

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Όνομα | Όνομα της δραστηριότητας | Ναι
Περιγραφή | Κείμενο που περιγράφει τι χρησιμεύει η δραστηριότητα | Όχι
Τύπος | HDinsightPig | Ναι
εισόδων | Ένα ή περισσότερα εισροές που καταναλώνεται από τη δραστηριότητα γουρούνι | Όχι
εξόδους | Μία ή περισσότερες εξόδους που δημιουργήθηκαν με τη δραστηριότητα γουρούνι | Ναι
linkedServiceName | Αναφορά στο σύμπλεγμα HDInsight που έχει καταχωρηθεί ως συνδεδεμένο υπηρεσίας στην προέλευση δεδομένων | Ναι
δέσμη ενεργειών | Καθορίστε τα ενσωματωμένα γουρούνι δέσμης ενεργειών | Όχι
διαδρομή δέσμης ενεργειών | Αποθηκεύστε τη δέσμη ενεργειών γουρούνι στο χώρο αποθήκευσης αντικειμένων blob του Azure και δώστε τη διαδρομή προς το αρχείο. Χρησιμοποιήστε την ιδιότητα 'δέσμη ενεργειών' ή 'scriptPath'. Και τα δύο δεν μπορεί να χρησιμοποιηθεί μαζί. Το όνομα του αρχείου είναι διάκριση πεζών-κεφαλαίων. | Όχι
Ορίζει | Καθορίστε τις παραμέτρους ως ζεύγη κλειδιού/τιμής για αναφορά μέσα στη δέσμη ενεργειών γουρούνι | Όχι

## <a name="example"></a>Παράδειγμα

Ας δούμε ένα παράδειγμα παιχνιδιών αρχεία καταγραφής αναλυτικών στοιχείων όπου θέλετε να προσδιορίσετε την ώρα που αναλώθηκε από προγράμματα αναπαραγωγής αναπαραγωγή αγώνων ξεκινά από την εταιρεία σας.
 
Το παρακάτω δείγμα του αρχείου καταγραφής παιχνιδιών είναι ένα αρχείο διαχωρισμένο με κόμματα (,). Περιέχει τα ακόλουθα πεδία – ProfileID, SessionStart, διάρκεια, SrcIPAddress και GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Η **δέσμη ενεργειών γουρούνι** για την επεξεργασία αυτών των δεδομένων:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Για να εκτελέσετε αυτήν τη δέσμη ενεργειών γουρούνι στη διοχέτευση μια προέλευση δεδομένων, κάντε τα εξής:

1. Δημιουργήστε μια συνδεδεμένων υπηρεσία για να καταχωρήσετε [τη δική σας HDInsight τον υπολογισμό σύμπλεγμα](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) ή να ρυθμίσετε τις παραμέτρους [σε απαιτήσεων HDInsight, τον υπολογισμό σύμπλεγμα](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Ας καλέστε αυτήν την υπηρεσία συνδεδεμένων **HDInsightLinkedService**.
2.  Δημιουργία [συνδεδεμένων υπηρεσιών](data-factory-azure-blob-connector.md) για να ρυθμίσετε τη σύνδεση με το χώρο αποθήκευσης αντικειμένων Blob του Azure φιλοξενούν τα δεδομένα. Ας καλέστε αυτήν την υπηρεσία συνδεδεμένων **StorageLinkedService**.
3.  Δημιουργία [συνόλων δεδομένων](data-factory-create-datasets.md) που δείχνει προς τα δεδομένα εισόδου και τα δεδομένα εξόδου. Ας κλήσεων του συνόλου δεδομένων εισαγωγής **PigSampleIn** και του συνόλου δεδομένων εξόδου **PigSampleOut**.
4.  Αντιγράψτε το ερώτημα γουρούνι σε ένα αρχείο με το χώρο αποθήκευσης αντικειμένων Blob Azure ρυθμίσατε στο βήμα #2. Εάν το Azure χώρου αποθήκευσης που φιλοξενεί τα δεδομένα είναι διαφορετικό από εκείνον που φιλοξενεί το αρχείο ερωτήματος, δημιουργήστε μια ξεχωριστή υπηρεσία αποθήκευσης Azure συνδεδεμένες. Ανατρέξτε στην υπηρεσία συνδεδεμένο στη ρύθμιση παραμέτρων δραστηριότητας. Χρησιμοποιήστε **scriptPath **για να καθορίσετε τη διαδρομή προς το αρχείο δέσμης ενεργειών γουρούνι και **scriptLinkedService**. 
    
    > [AZURE.NOTE] Μπορείτε επίσης να παρέχετε τα ενσωματωμένα δέσμης ενεργειών γουρούνι στον ορισμό δραστηριότητας, χρησιμοποιώντας την ιδιότητα **δέσμης ενεργειών** . Ωστόσο, θα σας δεν συνιστάται αυτή η προσέγγιση ως όλων των ειδικών χαρακτήρων σε τη δέσμη ενεργειών πρέπει να είναι διαφυγής και μπορεί να προκαλέσει ζητήματα εντοπισμού σφαλμάτων. Η βέλτιστη πρακτική είναι να ακολουθήσετε το βήμα #4.
5. Δημιουργία της διοχέτευσης με τη δραστηριότητα HDInsightPig. Αυτή η δραστηριότητα επεξεργάζεται δεδομένα εισόδου, εκτελώντας γουρούνι δέσμης ενεργειών σε σύμπλεγμα HDInsight.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Ανάπτυξη της διοχέτευσης. Ανατρέξτε στο θέμα [Δημιουργία αγωγούς](data-factory-create-pipelines.md) άρθρου για λεπτομέρειες. 
7. Παρακολούθηση της διοχέτευσης χρησιμοποιώντας τα μοντέλα δεδομένων εργοστασίου παρακολούθησης και διαχείρισης. Ανατρέξτε στο θέμα [Παρακολούθηση και διαχείριση δεδομένων εργοστασίου αγωγούς](data-factory-monitor-manage-pipelines.md) άρθρου για λεπτομέρειες.

## <a name="specifying-parameters-for-a-pig-script"></a>Καθορίζει τις παραμέτρους για μια δέσμη ενεργειών γουρούνι 

Εξετάστε το ακόλουθο παράδειγμα: παιχνιδιών αρχεία καταγραφής είναι κατάποση καθημερινά στο χώρο αποθήκευσης Blob του Azure και αποθηκεύονται σε ένα φάκελο διαμερίσματα βάσει της ημερομηνίας και ώρας. Θέλετε να ρυθμίσετε τις παραμέτρους των τη δέσμη ενεργειών γουρούνι και μεταβιβάζουν δυναμικά τη θέση του φακέλου εισαγωγής κατά τη διάρκεια του χρόνου εκτέλεσης και παράγει επίσης το αποτέλεσμα διαμερίσματα με ημερομηνία και ώρα.
 
Για να χρησιμοποιήσετε με παραμέτρους γουρούνι δέσμης ενεργειών, κάντε τα εξής:

- Μπορείτε να ορίσετε τις παραμέτρους με **ορίζει**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Στη δέσμη ενεργειών γουρούνι, ανατρέξτε στις παραμέτρους χρησιμοποιώντας '**$parameterName**', όπως φαίνεται στο ακόλουθο παράδειγμα:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Δείτε επίσης
- [Ομάδα δραστηριότητας](data-factory-hive-activity.md)
- [MapReduce δραστηριότητας](data-factory-map-reduce.md)
- [Hadoop ροής δραστηριότητας](data-factory-hadoop-streaming-activity.md)
- [Κλήση τους προγράμματα](data-factory-spark.md)
- [Κλήση R δέσμες ενεργειών](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)



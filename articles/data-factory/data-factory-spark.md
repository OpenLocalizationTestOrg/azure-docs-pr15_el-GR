<properties 
    pageTitle="Κλήση τους προγραμμάτων από εργοστασίου δεδομένων Azure" 
    description="Μάθετε πώς μπορείτε να καλέσετε τους προγραμμάτων από ένα εργοστασίου Azure δεδομένων χρησιμοποιώντας τη δραστηριότητα MapReduce." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Κλήση τους προγραμμάτων από εργοστασίου δεδομένων
## <a name="introduction"></a>Εισαγωγή
Μπορείτε να χρησιμοποιήσετε τη δραστηριότητα MapReduce στη διοχέτευση μια προέλευση δεδομένων για την εκτέλεση προγραμμάτων τους στην το σύμπλεγμά σας HDInsight τους. Δείτε το άρθρο [MapReduce δραστηριότητας](data-factory-map-reduce.md) για λεπτομερείς πληροφορίες σχετικά με τη χρήση της δραστηριότητας πριν να διαβάζετε αυτό το άρθρο. 

## <a name="spark-sample-on-github"></a>Δείγμα τους σε GitHub
Το [τους - δείγμα δεδομένων Factory στην GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) δείχνει πώς να χρησιμοποιείτε MapReduce δραστηριότητας για να ενεργοποιήσετε ένα πρόγραμμα τους. Το πρόγραμμα τους αντιγράφει μόνο δεδομένα από ένα κοντέινερ αντικειμένων Blob του Azure σε κάποιον άλλο. 

## <a name="data-factory-entities"></a>Οντοτήτων εργοστασίου δεδομένων
Ο φάκελος **Τους-ADF/src/ADFJsons** περιέχει τα αρχεία για οντοτήτων εργοστασίου δεδομένων (συνδεδεμένες υπηρεσίες, σύνολα δεδομένων, διοχέτευσης).  

Υπάρχουν δύο **συνδεδεμένες υπηρεσίες** σε αυτό το δείγμα: Azure χώρου αποθήκευσης και το Azure HDInsight. Καθορίστε το όνομα Azure χώρου αποθήκευσης και τιμές κλειδιού στο **StorageLinkedService.json** και clusterUri, το όνομα χρήστη και τον κωδικό πρόσβασης στο **HDInsightLinkedService.json**.

Υπάρχουν δύο **συνόλων δεδομένων** σε αυτό το δείγμα: **input.json** και **output.json**. Αυτά τα αρχεία βρίσκονται στο φάκελο **συνόλων δεδομένων** .  Αυτά τα αρχεία αντιπροσωπεύουν εισόδου και εξόδου σύνολα δεδομένων για τη δραστηριότητα MapReduce

Εύρεση δείγματος αγωγούς στο φάκελο **ADFJsons/διοχέτευσης** . Αναθεώρηση μια διαδικασία για να κατανοήσετε τον τρόπο για να ενεργοποιήσετε ένα πρόγραμμα τους, χρησιμοποιώντας τη δραστηριότητα MapReduce. 

Η δραστηριότητα MapReduce έχει ρυθμιστεί για να καλέσετε **com.adf.sparklauncher.jar** στο κοντέινερ **adflibs** στο χώρο αποθήκευσης Azure του (που καθορίζεται στο το StorageLinkedService.json). Τον πηγαίο κώδικα για αυτό το πρόγραμμα είναι στο τους-ADF/src/κύριες/java/com/adf/φακέλου και κλήσεις υποβολή τους και να εκτελέσετε εργασίες τους. 

> [AZURE.IMPORTANT] 
> Διαβάστε μέσω [αρχείο README. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) για τις πιο πρόσφατες και πρόσθετες πληροφορίες πριν από τη χρήση του δείγματος. 
>  
> Χρησιμοποιήστε το δικό σας σύμπλεγμα HDInsight τους με αυτήν την προσέγγιση για να καλέσετε τους προγράμματα που χρησιμοποιούν τη δραστηριότητα MapReduce. Χρησιμοποιώντας ένα σύμπλεγμα HDInsight σε ζήτηση δεν υποστηρίζεται.   


## <a name="see-also"></a>Δείτε επίσης
- [Ομάδα δραστηριότητας](data-factory-hive-activity.md)
- [Γουρούνι δραστηριότητας](data-factory-pig-activity.md)
- [MapReduce δραστηριότητας](data-factory-map-reduce.md)
- [Hadoop ροής δραστηριότητας](data-factory-hadoop-streaming-activity.md)
- [Κλήση R δέσμες ενεργειών](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

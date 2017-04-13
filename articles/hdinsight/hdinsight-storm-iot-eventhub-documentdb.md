<properties
 pageTitle="Επεξεργασία δεδομένων αισθητήρα οχήματος με καταιγίδας Apache στην HDInsight | Microsoft Azure"
 description="Μάθετε πώς να επεξεργάζονται δεδομένα αισθητήρα οχήματος από διανομείς συμβάν χρησιμοποιώντας καταιγίδας Apache στην HDInsight. Προσθήκη μοντέλου δεδομένων από DocumentDB, και να αποθηκεύσετε το αποτέλεσμα με το χώρο αποθήκευσης."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Επεξεργασία δεδομένων αισθητήρα οχήματος από διανομείς συμβάν Azure χρησιμοποιώντας καταιγίδας Apache στην HDInsight

Μάθετε πώς να επεξεργάζονται δεδομένα αισθητήρα οχήματος από διανομείς συμβάν Azure χρησιμοποιώντας καταιγίδας Apache στην HDInsight. Αυτό το παράδειγμα διαβάζει δεδομένα αισθητήρα από διανομείς συμβάν Azure εμπλουτίζει τις δυνατότητες των δεδομένων με την αναφορά δεδομένα αποθηκευμένα σε Azure DocumentDB και τέλος αποθήκευση των δεδομένων στο χώρο αποθήκευσης Azure χρησιμοποιώντας το σύστημα αρχείων Hadoop (HDFS).

![HDInsight και το διάγραμμα αρχιτεκτονική Internet πράγματα (IoT)](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Επισκόπηση

Προσθήκη αισθητήρες οχήματα σάς επιτρέπει να πρόβλεψης εξοπλισμού προβλήματα που βασίζονται σε δεδομένα ιστορικού τάσεων, καθώς και βελτιώσεις μελλοντικές εκδόσεις με βάση την ανάλυση χρήσης μοτίβο. Κατά την επεξεργασία δέσμης παραδοσιακά MapReduce μπορεί να χρησιμοποιηθεί για αυτήν την ανάλυση, πρέπει να μπορούν να γρήγορα και αποτελεσματικά φόρτωση των δεδομένων από όλα τα οχήματα στο Hadoop πριν από την επεξεργασία MapReduce μπορεί να προκύψει. Επιπλέον, ίσως θελήσετε να κάνετε ανάλυσης για την αποτυχία κρίσιμες διαδρομές (θερμοκρασίας μηχανισμός, πέδες, κ.λπ.) σε πραγματικό χρόνο.

Azure διανομείς συμβάν δημιουργούνται να χειρίζεται το μαζικών όγκο δεδομένων που δημιουργούνται με αισθητήρες και καταιγίδας Apache στην HDInsight μπορεί να χρησιμοποιηθεί για τη φόρτωση και επεξεργασίας των δεδομένων πριν από την αποθήκευση του σε HDFS (υποστηρίζεται από χώρο αποθήκευσης Azure) για επιπλέον MapReduce επεξεργασία.

##<a name="solution"></a>Λύση

Δεδομένα τηλεμετρίας για θερμοκρασίας μηχανισμός, θερμοκρασία και την ταχύτητα οχήματος καταγράφονται από αισθητήρες, στη συνέχεια, αποστέλλονται στο συμβάν διανομείς μαζί με το αυτοκινήτου οχήματος αναγνωριστικός αριθμός (Κατάσταση) και μια χρονική σήμανση. Από εκεί, μια τοπολογία καταιγίδας εκτελείται σε ένα καταιγίδας Apache σε σύμπλεγμα HDInsight διαβάζει τα δεδομένα, επεξεργάζεται και αποθηκεύει σε HDFS.

Κατά την επεξεργασία, την Κατάσταση χρησιμοποιείται για να ανακτήσετε πληροφορίες μοντέλο από το Azure DocumentDB. Αυτό προστίθεται στη ροή δεδομένων πριν από την αποθήκευσή τους.

Τα στοιχεία που χρησιμοποιούνται στην τοπολογία καταιγίδας είναι οι εξής:

* **EventHubSpout** - διαβάζει δεδομένα από διανομείς συμβάν Azure

* **TypeConversionBolt** - μετατρέπει τη συμβολοσειρά JSON από διανομείς συμβάντος σε μιας πλειάδας που περιέχει τις μεμονωμένες τιμές δεδομένων για θερμοκρασίας μηχανισμός, θερμοκρασία, ταχύτητα, Κατάσταση και χρονικής σήμανσης

* **DataReferencBolt** - αναζητά το μοντέλο οχήματος από DocumentDB χρησιμοποιώντας την Κατάσταση

* **WasbStoreBolt** - αποθηκεύει τα δεδομένα για να HDFS (χώρος αποθήκευσης Azure)

Ακολουθεί ένα διάγραμμα αυτής της λύσης:

![τοπολογία καταιγίδας](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Αυτή είναι μια απλοποιημένη διαγράμματος και κάθε στοιχείο της λύσης μπορεί να έχει πολλές παρουσίες. Για παράδειγμα, τις πολλές παρουσίες κάθε στοιχείο της τοπολογίας κατανέμονται σε τους κόμβους το καταιγίδας σε σύμπλεγμα HDInsight.

##<a name="implementation"></a>Εφαρμογή

Μια ολοκληρωμένη, αυτοματοποιημένη λύση για αυτό το σενάριο είναι διαθέσιμη ως μέρος του αποθετηρίου [HDInsight-καταιγίδας-παραδείγματα](https://github.com/hdinsight/hdinsight-storm-examples) στην GitHub. Για να χρησιμοποιήσετε αυτό το παράδειγμα, ακολουθήστε τα βήματα στο θέμα το αρχείο README IoTExample [. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες τοπολογίες καταιγίδας παράδειγμα, ανατρέξτε στο θέμα [τοπολογίες παράδειγμα για καταιγίδας στην HDInsight](hdinsight-storm-example-topology.md).
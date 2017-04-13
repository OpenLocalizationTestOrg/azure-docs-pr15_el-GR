<properties
    pageTitle="Εντοπισμός σφαλμάτων και να αναλύσετε τις υπηρεσίες Hadoop με ενδείξεις σωρού | Microsoft Azure"
    description="Αυτόματη συλλογή σωρού ενδείξεων σφαλμάτων για τις υπηρεσίες Hadoop και τοποθετήσετε μέσα σε το λογαριασμό χώρου αποθήκευσης αντικειμένων Blob του Azure για τον εντοπισμό σφαλμάτων και ανάλυση."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Συλλογή σωρού Αποτυπώνει στο χώρο αποθήκευσης αντικειμένων Blob να εντοπισμός σφαλμάτων και να αναλύσετε τις υπηρεσίες Hadoop

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Οι ενδείξεις σωρού περιέχει ένα στιγμιότυπο της μνήμης της εφαρμογής, συμπεριλαμβανομένων των τιμών των μεταβλητών την ώρα που δημιουργήθηκε η ένδειξη. Έτσι ώστε να είναι πολύ χρήσιμη για τη διάγνωση προβλημάτων που παρουσιάζονται κατά το χρόνο εκτέλεσης. Οι ενδείξεις σωρού μπορεί να συγκεντρώσει για τις υπηρεσίες Hadoop αυτόματα και να τοποθετηθεί μέσα στο λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure ενός χρήστη στην περιοχή HDInsightHeapDumps /. 

Η συλλογή ενδείξεων σφαλμάτων σωρού για διάφορες υπηρεσίες πρέπει να είναι ενεργοποιημένη για τις υπηρεσίες σε μεμονωμένα συμπλεγμάτων. Η προεπιλογή για αυτήν τη δυνατότητα, θα πρέπει να απενεργοποιήσετε για ένα σύμπλεγμα. Αυτές τις ενδείξεις σωρού μπορεί να είναι μεγάλο, ώστε να είναι χρήσιμο για την παρακολούθηση του λογαριασμού χώρου αποθήκευσης αντικειμένων Blob όπου που αποθηκεύονται όταν έχει ενεργοποιηθεί η τη συλλογή.

> [AZURE.NOTE] Οι πληροφορίες σε αυτό το άρθρο ισχύει μόνο για HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με βάση Linux HDInsight, ανατρέξτε στο θέμα [Ενεργοποίηση σωρού ενδείξεις Hadoop υπηρεσίες βάσει Linux HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Υπηρεσίες για ενδείξεις σωρού

Μπορείτε να ενεργοποιήσετε την σωρού ενδείξεων σφαλμάτων για τις ακόλουθες υπηρεσίες:

*  **hcatalog** - tempelton
*  **ομάδα** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **νήματα** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Ρύθμιση παραμέτρων στοιχείων που επιτρέπουν οι ενδείξεις σωρού

Για να ενεργοποιήσετε ενδείξεις σωρού για μια υπηρεσία, πρέπει να ορίσετε τα στοιχεία κατάλληλη ρύθμιση παραμέτρων στην ενότητα για αυτήν την υπηρεσία, που έχει καθοριστεί από την **παράμετρο όνομα_υπηρεσίας**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Η τιμή του **παράμετρο όνομα_υπηρεσίας** μπορεί να είναι οποιαδήποτε από τις υπηρεσίες που αναφέρονται παραπάνω: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, ή namenode.

## <a name="enable-using-azure-powershell"></a>Ενεργοποίηση της χρήσης του PowerShell Azure

Για παράδειγμα, για να ενεργοποιήσετε τις ενδείξεις σωρού με χρήση του Azure PowerShell για jobhistoryserver, μπορείτε να κάντε τα εξής:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Ενεργοποίηση χρησιμοποιώντας .NET SDK

Για παράδειγμα, για να ενεργοποιήσετε τις ενδείξεις σωρού χρησιμοποιώντας το Azure HDInsight .NET SDK για jobhistoryserver, μπορείτε να κάνετε τα εξής:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

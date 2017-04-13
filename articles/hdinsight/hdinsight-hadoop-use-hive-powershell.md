<properties
   pageTitle="Χρήση της ομάδας Hadoop με το PowerShell στο HDInsight | Microsoft Azure"
   description="Χρήση του PowerShell για εκτέλεση ερωτημάτων Hive Hadoop σε HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Εκτέλεση ερωτημάτων Hive χρήση του PowerShell

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Αυτό το έγγραφο παρέχει ένα παράδειγμα της χρήσης του Azure PowerShell στην κατάσταση λειτουργίας Azure ομάδα πόρων για να εκτελέσετε ερωτήματα Hive σε Hadoop σε σύμπλεγμα HDInsight.

> [AZURE.NOTE] Αυτό το έγγραφο δεν παρέχει μια λεπτομερή περιγραφή του τι κάνουν τις προτάσεις HiveQL που χρησιμοποιούνται σε παραδείγματα. Για πληροφορίες σχετικά με το HiveQL που χρησιμοποιείται σε αυτό το παράδειγμα, ανατρέξτε στο θέμα [Χρήση Hive με Hadoop σε HDInsight](hdinsight-use-hive.md).


**Προαπαιτούμενα στοιχεία**

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής.

- **Ένα σύμπλεγμα Azure HDInsight (Hadoop σε HDInsight) (Windows βασίζεται ή Linux)**
- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Εκτέλεση ερωτημάτων Hive με χρήση του PowerShell Azure

Azure PowerShell παρέχει *cmdlet του* που σας επιτρέπει να εκτελέσετε από απόσταση Hive ερωτήματα στην HDInsight. Εσωτερικά, αυτή η ενέργεια πραγματοποιείται χρησιμοποιώντας το ΥΠΌΛΟΙΠΟ κλήσεις [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (παλαιότερα γνωστές ως Templeton) εκτελείται στο σύμπλεγμα HDInsight.

Τα ακόλουθα cmdlet που χρησιμοποιούνται κατά την εκτέλεση ερωτημάτων ομάδας σε ένα σύμπλεγμα απομακρυσμένης HDInsight:

* **Προσθήκη AzureRmAccount**: πραγματοποιεί έλεγχο ταυτότητας του Azure PowerShell για τη συνδρομή σας στο Azure

* **Δημιουργία AzureRmHDInsightHiveJobDefinition**: δημιουργεί ένα νέο *ορισμό εργασίας* , χρησιμοποιώντας το καθορισμένο δηλώσεις HiveQL

* **Έναρξη-AzureRmHDInsightJob**: στέλνει τον ορισμό εργασίας με το HDInsight, η εργασία ξεκινά και επιστρέφει ένα αντικείμενο *εργασίας* που μπορούν να χρησιμοποιηθούν για να ελέγξετε την κατάσταση του έργου

* **Αναμονή AzureRmHDInsightJob**: χρησιμοποιεί το αντικείμενο εργασίας για να ελέγξετε την κατάσταση της εργασίας. Αυτό θα περιμένετε μέχρι να ολοκληρωθεί η εργασία ή υπερβεί το χρόνο αναμονής.

* **Get-AzureRmHDInsightJobOutput**: χρησιμοποιείται για την ανάκτηση το αποτέλεσμα του έργου

* **Ενεργοποίηση AzureRmHDInsightHiveJob**: χρησιμοποιείται για την εκτέλεση HiveQL δηλώσεις. Αυτό θα εμποδίσει το ερώτημα ολοκληρωθεί και, στη συνέχεια, επιστρέφει το αποτέλεσμα

* **Χρήση AzureRmHDInsightCluster**: Ορίζει το τρέχον σύμπλεγμα για να χρησιμοποιήσετε για την εντολή **Ενεργοποίηση AzureRmHDInsightHiveJob**

Τα βήματα που ακολουθούν δείχνουν τον τρόπο χρήσης αυτών των cmdlet για να εκτελέσετε μια εργασία σε το σύμπλεγμά σας HDInsight:

1. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας, να αποθηκεύσετε τον παρακάτω κώδικα ως **hivejob.ps1**. Πρέπει να αντικαταστήσετε **CLUSTERNAME** με το όνομα του συμπλέγματος HDInsight.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Ανοίξτε μια νέα γραμμή εντολών **Του PowerShell Azure** . Αλλαγή σε καταλόγους στη θέση του αρχείου **hivejob.ps1** και, στη συνέχεια, χρησιμοποιήστε την παρακάτω εντολή για να εκτελέσετε τη δέσμη ενεργειών:

        .\hivejob.ps1

    Όταν εκτελείται η δέσμη ενεργειών, θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήρια λογαριασμού HTTPS/διαχείρισης για το σύμπλεγμά σας. Μπορεί επίσης να σας ζητηθεί να συνδεθείτε στη συνδρομή σας στο Azure.
    
7. Όταν ολοκληρωθεί η εργασία, πρέπει να επιστρέψει πληροφορίες παρόμοιο με το εξής:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Όπως προαναφέρθηκε, **Ενεργοποίηση ομάδας** μπορεί να χρησιμοποιηθεί για να εκτελέσετε ένα ερώτημα και να περιμένετε απάντηση. Χρησιμοποιήστε τις παρακάτω εντολές και αντικαταστήστε **CLUSTERNAME** με το όνομα του συμπλέγματος:

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Το αποτέλεσμα θα είναι ως εξής:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Για περισσότερο HiveQL ερωτήματα, μπορείτε να χρησιμοποιήσετε το cmdlet του Azure PowerShell **Εδώ συμβολοσειρές** ή τα αρχεία δέσμης ενεργειών HiveQL. Το παρακάτω τμήμα κώδικα δείχνει πώς μπορείτε να χρησιμοποιήσετε το cmdlet **Ενεργοποίηση ομάδας** για να εκτελέσετε ένα αρχείο δέσμης ενεργειών HiveQL. Το αρχείο δέσμης ενεργειών HiveQL πρέπει να αποσταλεί σε wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > Για περισσότερες πληροφορίες σχετικά με **Αυτό το σημείο συμβολοσειρές**, ανατρέξτε στο θέμα <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Χρήση του Windows PowerShell εδώ-συμβολοσειρές</a>.

##<a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Εάν δεν υπάρχουν πληροφορίες επιστρέφεται όταν ολοκληρωθεί η εργασία, ενδέχεται να έχει παρουσιαστεί σφάλμα κατά την επεξεργασία. Για να προβάλετε πληροφορίες σφαλμάτων για αυτήν την εργασία, προσθέστε τα εξής στο τέλος του αρχείου **hivejob.ps1** , αποθηκεύστε το και, στη συνέχεια, εκτελέστε ξανά.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Αυτή η διαδικασία επιστρέφει τις πληροφορίες που είναι γραμμένο σε STDERR στο διακομιστή κατά την εκτέλεση της εργασίας και, αυτό μπορεί να σας βοηθήσει να προσδιορίσετε γιατί αποτυγχάνει η εργασία.

##<a name="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε, Azure PowerShell παρέχει ένας εύκολος τρόπος για την εκτέλεση ερωτημάτων ομάδας σε ένα σύμπλεγμα HDInsight, παρακολουθείτε την κατάσταση της εργασίας και ανάκτησης το αποτέλεσμα.

##<a name="next-steps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με την ομάδα του HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

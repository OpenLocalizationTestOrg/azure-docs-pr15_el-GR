<properties
   pageTitle="Χρήση Hadoop γουρούνι με το PowerShell στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς να υποβάλλουν γουρούνι εργασίες σε ένα σύμπλεγμα Hadoop στη χρήση του Azure PowerShell HDInsight."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Εκτέλεση εργασιών γουρούνι με χρήση του PowerShell

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Αυτό το έγγραφο παρέχει ένα παράδειγμα της χρήσης του PowerShell Azure για την υποβολή γουρούνι εργασιών για να Hadoop σε σύμπλεγμα HDInsight. Γουρούνι σάς επιτρέπει να γράψετε MapReduce εργασίες, χρησιμοποιώντας μια γλώσσα (Λατινικά γουρούνι), η οποία μοντέλα δεδομένων μετασχηματισμοί, αντί για αντιστοίχιση και μείωση συναρτήσεις.

> [AZURE.NOTE] Αυτό το έγγραφο δεν παρέχει μια λεπτομερή περιγραφή του τι κάνουν τις προτάσεις Λατινικά γουρούνι που χρησιμοποιήθηκαν στα παραδείγματα. Για πληροφορίες σχετικά με τα Λατινικά γουρούνι που χρησιμοποιούνται σε αυτό το παράδειγμα, ανατρέξτε στο θέμα [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής.

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Εκτέλεση εργασιών γουρούνι με χρήση του PowerShell

Azure PowerShell παρέχει *cmdlet του* που σας επιτρέπει να εκτελέσετε από απόσταση εργασίες γουρούνι σε HDInsight. Εσωτερικά, αυτή η ενέργεια πραγματοποιείται χρησιμοποιώντας το ΥΠΌΛΟΙΠΟ κλήσεις [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (παλαιότερα γνωστές ως Templeton) εκτελείται στο σύμπλεγμα HDInsight.

Τα ακόλουθα cmdlet που χρησιμοποιούνται όταν γουρούνι εργασίες που εκτελούνται σε έναν απομακρυσμένο σύμπλεγμα HDInsight:

* **Σύνδεση AzureRmAccount**: πραγματοποιεί έλεγχο ταυτότητας Azure του PowerShell για τη συνδρομή σας στο Azure

* **Δημιουργία AzureRmHDInsightPigJobDefinition**: δημιουργεί ένα νέο *ορισμό εργασίας* , χρησιμοποιώντας το καθορισμένο δηλώσεις γουρούνι Λατινικά

* **Έναρξη-AzureRmHDInsightJob**: στέλνει τον ορισμό εργασίας με το HDInsight, η εργασία ξεκινά και επιστρέφει ένα αντικείμενο *εργασίας* που μπορούν να χρησιμοποιηθούν για να ελέγξετε την κατάσταση του έργου

* **Αναμονή AzureRmHDInsightJob**: χρησιμοποιεί το αντικείμενο εργασίας για να ελέγξετε την κατάσταση της εργασίας. Αυτό θα περιμένετε μέχρι να ολοκληρωθεί η εργασία ή το χρόνο αναμονής έχει γίνει υπέρβαση.

* **Get-AzureRmHDInsightJobOutput**: χρησιμοποιείται για την ανάκτηση το αποτέλεσμα του έργου

Ακολουθήστε τα παρακάτω βήματα δείχνουν τον τρόπο χρήσης αυτών των cmdlet για να εκτελέσετε μια εργασία σε το σύμπλεγμά σας HDInsight.

1. Χρησιμοποιώντας ένα πρόγραμμα επεξεργασίας, να αποθηκεύσετε τον παρακάτω κώδικα ως **pigjob.ps1**. Πρέπει να αντικαταστήσετε **CLUSTERNAME** με το όνομα του συμπλέγματος HDInsight.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Ανοίξτε μια νέα γραμμή εντολών του Windows PowerShell. Αλλαγή σε καταλόγους στη θέση του αρχείου **pigjob.ps1** και, στη συνέχεια, χρησιμοποιήστε την παρακάτω εντολή για να εκτελέσετε τη δέσμη ενεργειών:

        .\pigjob.ps1
        
    Πρώτα θα σας ζητηθεί να συνδεθείτε στη συνδρομή σας στο Azure. Στη συνέχεια, θα σας ζητηθεί για το όνομα του λογαριασμού HTTPs/διαχείρισης και τον κωδικό πρόσβασης για το σύμπλεγμα HDInsight.

7. Όταν ολοκληρωθεί η εργασία, πρέπει να επιστρέψει πληροφορίες παρόμοιο με το εξής:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Εάν δεν υπάρχουν πληροφορίες επιστρέφεται όταν ολοκληρωθεί η εργασία, ενδέχεται να έχει παρουσιαστεί σφάλμα κατά την επεξεργασία. Για να προβάλετε πληροφορίες σφαλμάτων για αυτήν την εργασία, προσθέστε την ακόλουθη εντολή στο τέλος του αρχείου **pigjob.ps1** , αποθηκεύστε το και, στη συνέχεια, εκτελέστε ξανά.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Αυτό θα επιστρέψει τις πληροφορίες που έχει συνταχθεί στο STDERR στο διακομιστή κατά την εκτέλεση της εργασίας και, αυτό μπορεί να σας βοηθήσει να προσδιορίσετε γιατί αποτυγχάνει η εργασία.

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε, Azure PowerShell παρέχει έναν εύκολο τρόπο για να εκτελέσετε εργασίες γουρούνι σε ένα σύμπλεγμα HDInsight, παρακολουθείτε την κατάσταση της εργασίας και ανάκτησης το αποτέλεσμα του.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με γουρούνι στο HDInsight:

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

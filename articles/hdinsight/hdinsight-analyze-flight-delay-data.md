<properties
    pageTitle="Ανάλυση δεδομένων καθυστέρηση πτήσεων με Hadoop στο HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε μία δέσμη ενεργειών του Windows PowerShell για να δημιουργήσετε ένα σύμπλεγμα HDInsight, εκτελέσετε μια εργασία Hive, εκτελέσετε μια εργασία Sqoop και διαγραφή του συμπλέγματος."
    services="hdinsight"
    documentationCenter=""
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

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight

Ομάδα παρέχει ένα μέσο εργασίες που εκτελούνται Hadoop MapReduce μέσω μιας γλώσσας δέσμης ενεργειών μοιάζει με SQL που ονομάζεται * [HiveQL][hadoop-hiveql]*, που μπορούν να εφαρμοστούν προς σύνοψη, υποβολή ερωτημάτων και την ανάλυση σε μεγάλους όγκους δεδομένων.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο απαιτεί ένα σύμπλεγμα HDInsight που βασίζεται στα Windows. Για τα βήματα που λειτουργούν με ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Ένα από τα κύρια πλεονεκτήματα της Azure HDInsight είναι το διαχωρισμό του χώρου αποθήκευσης δεδομένων και υπολογισμού. HDInsight χρησιμοποιεί χώρο αποθήκευσης αντικειμένων Blob του Azure για την αποθήκευση δεδομένων. Μια τυπική εργασία περιλαμβάνει τρία τμήματα:

1. **Αποθήκευση δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob του Azure.**  Για παράδειγμα, καιρού, δεδομένα, δεδομένα αισθητήρα, αρχεία καταγραφής web και, σε αυτήν την περίπτωση, με δεδομένα πτήσεων καθυστέρηση αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων Blob του Azure.
2. **Εκτέλεση εργασιών.** Όταν είναι καιρός να επεξεργάζονται τα δεδομένα, μπορείτε να εκτελέσετε μια δέσμη ενεργειών του Windows PowerShell (ή μια εφαρμογή προγράμματος-πελάτη) για να δημιουργήσετε ένα σύμπλεγμα HDInsight, εκτελέσετε εργασίες και να διαγράψετε το σύμπλεγμα. Οι εργασίες αποθηκεύστε τα δεδομένα εξόδου με το χώρο αποθήκευσης αντικειμένων Blob του Azure. Τα δεδομένα εξόδου διατηρείται ακόμα και μετά από τη διαγραφή του συμπλέγματος. Με αυτόν τον τρόπο, πληρώνετε για μόνο τι που έχουν καταναλωθεί.
3. **Ανακτήστε την έξοδο από το χώρο αποθήκευσης αντικειμένων Blob του Azure**, ή σε αυτό το πρόγραμμα εκμάθησης, εξαγάγετε τα δεδομένα σε μια βάση δεδομένων Azure SQL.

Το παρακάτω διάγραμμα παρουσιάζει το σενάριο και τη δομή του αυτό το πρόγραμμα εκμάθησης:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Σημείωση**: οι αριθμοί στο διάγραμμα αντιστοιχούν οι τίτλοι των ενοτήτων. **M** σημαίνουν τα κύρια διαδικασία. **A** σημαίνει για το περιεχόμενο στο προσάρτημα.

Το κύριο τμήμα του προγράμματος εκμάθησης σας δείχνει πώς μπορείτε να χρησιμοποιήσετε μία δέσμη ενεργειών του Windows PowerShell για να εκτελέσετε τις ακόλουθες εργασίες:

- Δημιουργήστε ένα σύμπλεγμα HDInsight.
- Εκτέλεση μιας ομάδας εργασίας στο σύμπλεγμα για να υπολογίσετε το μέσο όρο των καθυστερήσεων στα αεροδρόμια. Το πτήσεων καθυστέρηση τα δεδομένα αποθηκεύονται σε ένα λογαριασμό του χώρου αποθήκευσης αντικειμένων Blob του Azure.
- Εκτελέστε μια εργασία Sqoop για να εξαγάγετε το αποτέλεσμα της ομάδας εργασίας σε μια βάση δεδομένων Azure SQL.
- Διαγράψτε το σύμπλεγμα HDInsight.

Στο το παραρτήματα, μπορείτε να βρείτε τις οδηγίες για αποστολή με δεδομένα πτήσεων καθυστέρηση, μια συμβολοσειρά ερωτήματος ομάδα για τη δημιουργία/αποστολή και την προετοιμασία τη βάση δεδομένων Azure SQL για το έργο Sqoop.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο είναι συγκεκριμένες για συμπλεγμάτων HDInsight που βασίζεται στα Windows. Για τα βήματα που λειτουργούν με ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα ακόλουθα στοιχεία:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Αρχεία που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης**

Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί τις επιδόσεις στην ώρα της δεδομένα πτήσεων αεροπορικής εταιρείας από την [έρευνα και πρωτοποριακή τεχνολογία διαχείρισης, γραφείο των μεταφορών στατιστικά στοιχεία ή RITA] [rita-website]. Έχει αποσταλεί ένα αντίγραφο των δεδομένων σε ένα κοντέινερ χώρου αποθήκευσης αντικειμένων Blob του Azure με τα δικαιώματα πρόσβασης δημόσια Blob. Ένα τμήμα της δέσμης ενεργειών του PowerShell αντιγράφει τα δεδομένα από το κοντέινερ δημόσια blob το προεπιλεγμένο κοντέινερ αντικειμένων blob του συμπλέγματος. Η δέσμη ενεργειών HiveQL αντιγράφεται επίσης την ίδια κοντέινερ αντικειμένων Blob. Εάν θέλετε να μάθετε πώς μπορείτε να πετύχετε/αποστολή των δεδομένων στο δικό σας λογαριασμό χώρου αποθήκευσης, καθώς και πώς μπορείτε να δημιουργήσετε και αποστολή στο αρχείο δέσμης ενεργειών HiveQL, ανατρέξτε στο θέμα [παράρτημα A](#appendix-a) και [B προσάρτημα](#appendix-b).

Ο παρακάτω πίνακας παραθέτει τα αρχεία που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης:

<table border="1">
<tr><th>Αρχεία</th><th>Περιγραφή</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Το αρχείο δέσμης ενεργειών HiveQL που χρησιμοποιούνται από την ομάδα εργασίας. Αυτή η δέσμη ενεργειών έχει αποσταλεί με ένα λογαριασμό του χώρου αποθήκευσης αντικειμένων Blob του Azure με δημόσια πρόσβαση. <a href="#appendix-b">Προσάρτημα Β</a> περιλαμβάνει οδηγίες σχετικά με την προετοιμασία και την αποστολή αυτού του αρχείου στο δικό σας λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Δεδομένα εισόδου για το έργο της ομάδας. Τα δεδομένα σε ένα λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure με δημόσια πρόσβαση έχει αποσταλεί. <a href="#appendix-a">A προσάρτημα</a> περιλαμβάνει οδηγίες σχετικά με τη λήψη των δεδομένων και την αποστολή των δεδομένων στο δικό σας λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Η διαδρομή εξόδου για το έργο της ομάδας. Το προεπιλεγμένο κοντέινερ χρησιμοποιείται για την αποθήκευση των δεδομένων εξόδου.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Ομάδα εργασίας κατάστασης φάκελο το προεπιλεγμένο κοντέινερ.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Δημιουργία συμπλέγματος και να εκτελέσετε εργασίες Hive/Sqoop

Hadoop MapReduce είναι μαζική επεξεργασία. Ο πιο οικονομικός τρόπος για να εκτελέσετε μια εργασία ομάδας είναι να δημιουργήσετε ένα σύμπλεγμα για την εργασία και να διαγράψετε την εργασία, μετά την ολοκλήρωση της εργασίας. Η ακόλουθη δέσμη ενεργειών καλύπτει ολόκληρης της διαδικασίας. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ένα σύμπλεγμα HDInsight και την εκτέλεση εργασιών ομάδας, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight] [ hdinsight-provision] και [Χρήση ομάδας με το HDInsight][hdinsight-use-hive].

**Για να εκτελέσετε τα ερωτήματα Hive μέσω του PowerShell Azure**

1. Δημιουργήστε μια βάση δεδομένων Azure SQL και του πίνακα για το αποτέλεσμα του έργου Sqoop χρησιμοποιώντας τις οδηγίες που εμφανίζονται στο [Παράρτημα C](#appendix-c).
3. Ανοίξτε το Windows PowerShell ISE και εκτελέστε την ακόλουθη δέσμη ενεργειών:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Σύνδεση με τη βάση δεδομένων SQL και δείτε καθυστερήσεις πτήσεων average κατά πόλη στον πίνακα AvgDelays:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Παράρτημα A - Αποστολή με δεδομένα πτήσεων καθυστέρηση με το χώρο αποθήκευσης αντικειμένων Blob του Azure
Αποστολή του αρχείου δεδομένων και τα αρχεία δέσμης ενεργειών HiveQL (ανατρέξτε στο θέμα [Προσάρτημα Β](#appendix-b)) απαιτεί κάποιο σχεδιασμό. Η ιδέα είναι να αποθηκεύσετε τα αρχεία δεδομένων και το αρχείο HiveQL πριν από τη δημιουργία ένα σύμπλεγμα HDInsight και την εκτέλεση της ομάδας εργασίας. Έχετε δύο επιλογές:

- **Χρησιμοποιήστε τον ίδιο λογαριασμό Azure χώρου αποθήκευσης που θα χρησιμοποιηθεί από το HDInsight σύμπλεγμα ως το προεπιλεγμένο σύστημα αρχείων.** Επειδή το σύμπλεγμα HDInsight θα έχει το πλήκτρο πρόσβασης λογαριασμού χώρου αποθήκευσης, δεν χρειάζεται να κάνετε πρόσθετες αλλαγές.
- **Χρησιμοποιήστε διαφορετικό λογαριασμό αποθήκευσης Azure από το HDInsight σύμπλεγμα προεπιλεγμένο αρχείο σύστημα.** Εάν πρόκειται για την περίπτωση, πρέπει να τροποποιήσετε το τμήμα δημιουργίας της δέσμης ενεργειών του Windows PowerShell που βρέθηκαν στο [σύμπλεγμα HDInsight δημιουργία και εκτέλεση εργασιών Hive/Sqoop](#runjob) για να συνδέσετε το λογαριασμό χώρου αποθήκευσης ως έναν επιπλέον λογαριασμό χώρου αποθήκευσης. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight][hdinsight-provision]. Το HDInsight σύμπλεγμα γνωρίζει, στη συνέχεια, το πλήκτρο πρόσβασης για το λογαριασμό χώρου αποθήκευσης.

>[AZURE.NOTE] Η διαδρομή χώρο αποθήκευσης αντικειμένων Blob για το αρχείο δεδομένων είναι δύσκολο στον κώδικα στο αρχείο δέσμης ενεργειών HiveQL. Θα πρέπει να ενημερώσετε αντίστοιχα.

**Για να κάνετε λήψη του με δεδομένα πτήσεων**

1. Αναζήτηση για να την [έρευνα και διαχείριση πρωτοποριακή τεχνολογία, γραφείο του στατιστικά στοιχεία μεταφορών][rita-website].
2. Στη σελίδα, επιλέξτε τις ακόλουθες τιμές:

    <table border="1">
    <tr><th>Όνομα</th><th>Τιμή</th></tr>
    <tr><td>Φίλτρο έτος</td><td>2013 </td></tr>
    <tr><td>Φιλτράρισμα περίοδο</td><td>Ιανουάριος</td></tr>
    <tr><td>Πεδία</td><td>*Έτος*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *προέλευσης*, *OriginCityName*, *OriginState*, *DestAirportID*, *προορισμού*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (καταργήστε όλα τα άλλα πεδία)</td></tr>
    </table>

3. Κάντε κλικ στην επιλογή **λήψη**.
4. Αποσυμπιέστε το αρχείο στο φάκελο **C:\Tutorials\FlightDelay\2013Data** . Κάθε αρχείο είναι ένα αρχείο CSV και είναι μεγέθους 60GB περίπου.
5.  Μετονομάστε το αρχείο στο όνομα του μήνα που περιέχει δεδομένα. Για παράδειγμα, το αρχείο που περιέχει τα δεδομένα Ιανουαρίου ονομάζεται *January.csv*.
6. Επαναλάβετε τα βήματα 2 και 5 για να κάνετε λήψη ενός αρχείου για κάθε έναν από τους δώδεκα μήνες στο 2013. Χρειάζεστε τουλάχιστον ένα αρχείο για να εκτελέσετε το πρόγραμμα εκμάθησης.  

**Για να αποστείλετε τα δεδομένα πτήσεων καθυστέρηση στο χώρο αποθήκευσης αντικειμένων Blob του Azure**

1. Προετοιμασία τις παραμέτρους:

    <table border="1">
    <tr><th>Το όνομα της μεταβλητής</th><th>Σημειώσεις</th></tr>
    <tr><td>$storageAccountName</td><td>Ο λογαριασμός Azure αποθήκευσης όπου θέλετε να αποστείλετε τα δεδομένα.</td></tr>
    <tr><td>$blobContainerName</td><td>Το κοντέινερ αντικειμένων Blob όπου θέλετε να αποστείλετε τα δεδομένα.</td></tr>
    </table>
2. Ανοίξτε το Azure PowerShell ISE.
3. Επικολλήστε την ακόλουθη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών.

Εάν επιλέξετε να χρησιμοποιήσετε μια διαφορετική μέθοδο για την αποστολή των αρχείων, βεβαιωθείτε ότι η διαδρομή του αρχείου είναι τα προγράμματα εκμάθησης/flightdelay/δεδομένα. Η σύνταξη για να αποκτήσετε πρόσβαση στα αρχεία είναι:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Η διαδρομή flightdelay/προγράμματα εκμάθησης/δεδομένων είναι το εικονικός φάκελος που δημιουργήσατε κατά την αποστολή των αρχείων. Βεβαιωθείτε ότι δεν υπάρχουν 12 αρχεία, μία για κάθε μήνα.

>[AZURE.NOTE] Πρέπει να ενημερώσετε το ερώτημα ομάδας για να διαβάσετε από τη νέα θέση.

> Πρέπει είτε να ρυθμίσετε τα δικαιώματα πρόσβασης κοντέινερ για να είναι δημόσια ή να δεσμεύσετε το λογαριασμό χώρου αποθήκευσης στο σύμπλεγμα HDInsight. Διαφορετικά, η συμβολοσειρά ερωτήματος Hive δεν θα μπορείτε να αποκτήσετε πρόσβαση στα αρχεία δεδομένων.

---
##<a id="appendix-b"></a>Προσάρτημα Β - Δημιουργήστε και στείλτε μια δέσμη ενεργειών HiveQL

Χρήση του Azure PowerShell, μπορείτε να εκτελέσετε πολλαπλές δηλώσεις HiveQL μία κάθε φορά ή να συσκευάσετε τη δήλωση HiveQL σε ένα αρχείο δέσμης ενεργειών. Αυτή η ενότητα σας δείχνει πώς μπορείτε να δημιουργήσετε μια δέσμη ενεργειών HiveQL και να στείλετε τη δέσμη ενεργειών με το χώρο αποθήκευσης αντικειμένων Blob του Azure με χρήση του Azure PowerShell. Ομάδα απαιτεί τις δέσμες ενεργειών HiveQL να είναι αποθηκευμένο στο χώρο αποθήκευσης αντικειμένων Blob του Azure.

Η δέσμη ενεργειών HiveQL θα εκτελέσει τις εξής ενέργειες:

1. **Απόθεση του πίνακα delays_raw**, σε περίπτωση ο πίνακας ήδη υπάρχει.
2. Να **δημιουργήσετε τον πίνακα delays_raw εξωτερική ομάδα** που δείχνει προς τη θέση αποθήκευσης αντικειμένων Blob με τα αρχεία καθυστέρηση πτήσεων. Αυτό το ερώτημα Καθορίζει ότι τα πεδία έχουν οριοθετημένο από "," και ότι οι γραμμές τερματίζονται από "\n". Αυτό αποτελεί ένα πρόβλημα όταν τιμές πεδίων περιέχουν κόμματα, επειδή δεν είναι δυνατό να διαφοροποιήσετε Hive μεταξύ ένα κόμμα που είναι οριοθέτη πεδίου και ένα που είναι μέρος μιας τιμής πεδίου (που είναι τα γράμματα σε τιμές πεδίων για ORIGIN\_ΠΌΛΗ\_ΌΝΟΜΑ και Προορισμού\_ΠΌΛΗ\_ΌΝΟΜΑ). Για να το αντιμετωπίσετε, το ερώτημα δημιουργεί TEMP στήλες για τη διατήρηση δεδομένων που είναι λάθος διαίρεση σε στήλες.  
3. **Απόθεση του πίνακα καθυστερήσεις**, σε περίπτωση ο πίνακας ήδη υπάρχει.
4. **Δημιουργία πίνακα καθυστερήσεις**. Είναι χρήσιμο για να καθαρίσετε τα δεδομένα πριν από την περαιτέρω επεξεργασία. Αυτό το ερώτημα δημιουργεί ένα νέο πίνακα, *καθυστερήσεις*, από τον πίνακα delays_raw. Σημειώστε ότι δεν αντιγράφονται τις στήλες TEMP (όπως προαναφέρθηκε) και ότι η συνάρτηση **δευτερεύουσας** χρησιμοποιείται για να καταργήσετε εισαγωγικά από τα δεδομένα.
5. **Υπολογίσετε τις καθυστέρηση average καιρού και τις ομάδες τα αποτελέσματα με το όνομα της πόλης.** Αυτό θα εξόδου επίσης τα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων Blob. Σημειώστε ότι το ερώτημα θα καταργήσει αποστρόφους από τα δεδομένα και θα αποκλείσετε γραμμές όπου η τιμή του **weather_delay** είναι null. Αυτό είναι απαραίτητο, επειδή Sqoop, χρησιμοποιείται αργότερα σε αυτό το πρόγραμμα εκμάθησης, δεν χειριστεί ομαλά αυτές τις τιμές από προεπιλογή.

Για μια πλήρη λίστα των εντολών HiveQL, ανατρέξτε στο θέμα [Hive Data Definition Language][hadoop-hiveql]. Κάθε εντολή HiveQL πρέπει να τερματιστεί με ελληνικό ερωτηματικό.

**Για να δημιουργήσετε ένα αρχείο δέσμης ενεργειών HiveQL**

1. Προετοιμασία τις παραμέτρους:

    <table border="1">
    <tr><th>Το όνομα της μεταβλητής</th><th>Σημειώσεις</th></tr>
    <tr><td>$storageAccountName</td><td>Ο λογαριασμός Azure αποθήκευσης όπου θέλετε να αποστείλετε τη δέσμη ενεργειών HiveQL για.</td></tr>
    <tr><td>$blobContainerName</td><td>Το κοντέινερ αντικειμένων Blob όπου θέλετε να αποστείλετε τη δέσμη ενεργειών HiveQL για.</td></tr>
    </table>
2. Ανοίξτε το Azure PowerShell ISE.

3. Αντιγράψτε και επικολλήστε την ακόλουθη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Εδώ θα βρείτε τις μεταβλητές που χρησιμοποιούνται στη δέσμη ενεργειών:

    - **$hqlLocalFileName** - η δέσμη ενεργειών θα αποθηκεύει τοπικά το αρχείο δέσμης ενεργειών HiveQL πριν από την αποστολή του σε χώρο αποθήκευσης αντικειμένων Blob. Αυτό είναι το όνομα του αρχείου. Η προεπιλεγμένη τιμή είναι <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** - αυτό είναι το HiveQL δέσμης ενεργειών blob όνομα αρχείου χρησιμοποιείται σε το χώρο αποθήκευσης αντικειμένων Blob του Azure. Η προεπιλεγμένη τιμή είναι tutorials/flightdelay/flightdelays.hql. Επειδή το αρχείο θα εγγράφονται απευθείας στο χώρο αποθήκευσης αντικειμένων Blob του Azure, δεν υπάρχει ένα "/" στην αρχή του ονόματος blob. Εάν θέλετε να έχετε πρόσβαση στο αρχείο από το χώρο αποθήκευσης αντικειμένων Blob, θα πρέπει να προσθέσετε ένα "/" στην αρχή του ονόματος αρχείου.
    - **$srcDataFolder** και **$dstDataFolder** - = "flightdelay/προγράμματα εκμάθησης/δεδομένα" = "flightdelay/προγράμματα εκμάθησης/εξόδου"


---
##<a id="appendix-c"></a>Προσάρτημα C - προετοιμάστε μια βάση δεδομένων Azure SQL για το αποτέλεσμα του έργου Sqoop
**Για να προετοιμάσετε τη βάση δεδομένων SQL (συγχώνευση αυτό με τη δέσμη ενεργειών Sqoop)**

1. Προετοιμασία τις παραμέτρους:

    <table border="1">
    <tr><th>Το όνομα της μεταβλητής</th><th>Σημειώσεις</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Το όνομα του διακομιστή βάσης δεδομένων Azure SQL. Εισαγάγετε τίποτα, προκειμένου να δημιουργήσετε ένα νέο διακομιστή.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Το όνομα σύνδεσης για το διακομιστή βάσης δεδομένων Azure SQL. Εάν $sqlDatabaseServerName είναι υπάρχοντος διακομιστή, η σύνδεση και κωδικό πρόσβασης που χρησιμοποιούνται για τον έλεγχο ταυτότητας με το διακομιστή. Διαφορετικά χρησιμοποιούνται για να δημιουργήσετε ένα νέο διακομιστή.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Ο κωδικός πρόσβασης σύνδεσης για το διακομιστή βάσης δεδομένων Azure SQL.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Αυτή η τιμή χρησιμοποιείται μόνο όταν θέλετε να δημιουργήσετε ένα νέο διακομιστή Azure βάσης δεδομένων.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>Χρησιμοποιείται για τη δημιουργία πίνακα AvgDelays για το έργο Sqoop τη βάση δεδομένων SQL. Εάν αφήσετε κενό θα δημιουργήσει μια βάση δεδομένων που ονομάζεται HDISqoop. Το όνομα του πίνακα για το αποτέλεσμα του έργου Sqoop είναι AvgDelays. </td></tr>
    </table>
2. Ανοίξτε το Azure PowerShell ISE.
3. Αντιγράψτε και επικολλήστε την ακόλουθη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Η δέσμη ενεργειών χρησιμοποιεί παραστάσεως· κατάσταση υπηρεσίας μεταφοράς (REST), http://bot.whatismyipaddress.com, για την ανάκτηση εξωτερικών διεύθυνση IP σας. Η διεύθυνση IP χρησιμοποιείται για τη δημιουργία ενός κανόνα τείχους προστασίας για το διακομιστή βάσης δεδομένων SQL.  

    Εδώ θα βρείτε ορισμένες μεταβλητές που χρησιμοποιούνται στη δέσμη ενεργειών:

    - **$ipAddressRestService** - η προεπιλεγμένη τιμή είναι http://bot.whatismyipaddress.com. Είναι μια δημόσια διεύθυνση IP υπηρεσία REST για τη λήψη εξωτερικών διεύθυνση IP σας. Εάν θέλετε, μπορείτε να χρησιμοποιήσετε άλλες υπηρεσίες. Η εξωτερική διεύθυνση IP που ανακτήθηκαν μέσω της υπηρεσίας θα χρησιμοποιηθεί για να δημιουργήσετε έναν κανόνα τείχους προστασίας για το διακομιστή βάσης δεδομένων Azure SQL, έτσι ώστε να μπορείτε να αποκτήσετε πρόσβαση στη βάση δεδομένων από το σταθμούς εργασίας (χρησιμοποιώντας μια δέσμη ενεργειών του Windows PowerShell).
    - **$fireWallRuleName** - αυτό είναι το όνομα του κανόνα τείχους προστασίας για το διακομιστή βάσης δεδομένων Azure SQL. Το προεπιλεγμένο όνομα είναι <u>FlightDelay</u>. Μπορείτε να τη μετονομάσετε εάν θέλετε.
    - **$sqlDatabaseMaxSizeGB** - αυτή η τιμή χρησιμοποιείται μόνο όταν θέλετε να δημιουργήσετε ένα νέο διακομιστή βάσης δεδομένων Azure SQL. Η προεπιλεγμένη τιμή είναι 10GB. 10GB είναι αρκετό για αυτό το πρόγραμμα εκμάθησης.
    - **$sqlDatabaseName** - αυτή η τιμή χρησιμοποιείται μόνο όταν θέλετε να δημιουργήσετε μια νέα βάση δεδομένων Azure SQL. Η προεπιλεγμένη τιμή είναι HDISqoop. Εάν μπορείτε να το μετονομάσετε, πρέπει να ενημερώσετε τη δέσμη ενεργειών του Windows PowerShell Sqoop αντίστοιχα.

4. Πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών.
5. Επικυρώστε δέσμη ενεργειών. Βεβαιωθείτε ότι η δέσμη ενεργειών εκτελέστηκε με επιτυχία.

##<a id="nextsteps"></a>Επόμενα βήματα
Τώρα μπορείτε να κατανοήσετε τον τρόπο για να αποστείλετε ένα αρχείο στο χώρο αποθήκευσης αντικειμένων Blob του Azure, πώς μπορείτε να συμπληρώσετε έναν πίνακα Hive χρησιμοποιώντας τα δεδομένα από το χώρο αποθήκευσης αντικειμένων Blob του Azure, τον τρόπο εκτέλεσης ερωτημάτων Hive και πώς μπορείτε να χρησιμοποιήσετε Sqoop για να εξαγάγετε δεδομένα από HDFS σε μια βάση δεδομένων Azure SQL. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]
* [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
* [Χρήση Oozie με HDInsight][hdinsight-use-oozie]
* [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop]
* [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
* [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png

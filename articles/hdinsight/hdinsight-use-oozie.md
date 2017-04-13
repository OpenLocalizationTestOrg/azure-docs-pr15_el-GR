<properties
    pageTitle="Χρήση Hadoop Oozie σε HDInsight | Microsoft Azure"
    description="Χρησιμοποιήστε Hadoop Oozie HDInsight, μια υπηρεσία μεγάλο δεδομένων. Μάθετε πώς μπορείτε να ορίσετε μια ροή εργασίας Oozie, και να υποβάλετε μια εργασία Oozie."
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
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-in-hdinsight"></a>Χρησιμοποιήστε Oozie με Hadoop για να ορίσετε και να διαχειριστώ μια ροή εργασίας σε HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Apache Oozie για να ορίσετε μια ροή εργασίας και να εκτελέσετε τη ροή εργασίας HDInsight. Για να μάθετε περισσότερα σχετικά με το συντονισμού Oozie, ανατρέξτε στο θέμα [Συντονισμού βάσει χρόνου Oozie Hadoop χρήση με το HDInsight][hdinsight-oozie-coordinator-time]. Για να μάθετε Azure εργοστασίου δεδομένων, ανατρέξτε στο θέμα [Χρήση γουρούνι και ομάδα με δεδομένα εργοστασίου][azure-data-factory-pig-hive].

Apache Oozie είναι ένα σύστημα ροής εργασίας/συντονισμό που διαχειρίζεται το Hadoop εργασίες. Αυτό είναι συνδεδεμένο με την στοίβα Hadoop, καθώς και υποστηρίζει Hadoop εργασίες για Apache MapReduce, γουρούνι Apache, Apache Hive και Apache Sqoop. Μπορεί επίσης να χρησιμοποιηθεί για να προγραμματίζουν εργασίες που σχετίζονται με ένα σύστημα, όπως τα προγράμματα Java ή δεσμών ενεργειών κελύφους.

Η ροή εργασίας θα υλοποιήσετε, ακολουθώντας τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης περιέχει δύο ενέργειες:

![Διάγραμμα ροής εργασίας][img-workflow-diagram]

1. Μια ομάδα ενέργεια εκτελεί μια δέσμη ενεργειών HiveQL για την καταμέτρηση των εμφανίσεων κάθε τύπο επίπεδο καταγραφής σε ένα αρχείο log4j. Κάθε αρχείο log4j αποτελείται από μια γραμμή των πεδίων που περιέχει ένα πεδίο [ΕΠΊΠΕΔΟ ΚΑΤΑΓΡΑΦΉΣ] που εμφανίζει τον τύπο και της σοβαρότητας, για παράδειγμα:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Δέσμη ενεργειών ομάδας είναι παρόμοια με:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Για περισσότερες πληροφορίες σχετικά με την ομάδα, ανατρέξτε στο θέμα [Χρήση Hive με HDInsight][hdinsight-use-hive].

2.  Μια ενέργεια Sqoop εξάγει το αποτέλεσμα του HiveQL σε έναν πίνακα σε μια βάση δεδομένων Azure SQL. Για περισσότερες πληροφορίες σχετικά με το Sqoop, ανατρέξτε στο θέμα [Χρήση Hadoop Sqoop με HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Για υποστηριζόμενες εκδόσεις Oozie στην συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα Hadoop που παρέχεται από το HDInsight;] [hdinsight-versions].

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Μια σταθμούς εργασίας με το Azure PowerShell**. 

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
    
    Για να εκτελέσετε τις δέσμες ενεργειών του Windows PowerShell, πρέπει να εκτελέσετε ως διαχειριστής και ορίστε την πολιτική εκτέλεσης σε *RemoteSigned*. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εκτέλεση του Windows PowerShell δέσμες ενεργειών][powershell-script].

##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Ορισμός Oozie ροής εργασίας και το σχετικό δέσμης ενεργειών HiveQL

Ορισμοί ροές εργασίας Oozie είναι γραμμένες σε hPDL (μια XML διαδικασία Definition Language). Το προεπιλεγμένο όνομα αρχείου ροής εργασίας είναι *workflow.xml*. Ακολουθεί το αρχείο ροής εργασίας που θα χρησιμοποιήσετε σε αυτό το πρόγραμμα εκμάθησης.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Υπάρχουν δύο ενέργειες που ορίζονται στη ροή εργασίας. Η ενέργεια έναρξης να είναι *RunHiveScript*. Εάν η ενέργεια εκτελείται με επιτυχία, η επόμενη ενέργεια είναι *RunSqoopExport*.

Το RunHiveScript έχει πολλές μεταβλητές. Θα μπορείτε να μεταβιβάσετε τις τιμές κατά την υποβολή της εργασίας Oozie από το σταθμούς εργασίας με χρήση του Azure PowerShell.

<table border = "1">
<tr><th>Μεταβλητές ροής εργασιών</th><th>Περιγραφή</th></tr>
<tr><td>${jobTracker}</td><td>Καθορίζει τη διεύθυνση URL του εργαλείου παρακολούθησης εργασία Hadoop. Χρησιμοποιήστε <strong>jobtrackerhost:9010</strong> HDInsight έκδοση 3.0 και 2.1.</td></tr>
<tr><td>${nameNode}</td><td>Καθορίζει τη διεύθυνση URL του κόμβου όνομα Hadoop. Χρησιμοποιήστε την προεπιλεγμένη διεύθυνση αρχείο συστήματος, για παράδειγμα, <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${Όνομα_ουράς}</td><td>Καθορίζει το όνομα ουράς που θα υποβάλλονται την εργασία. Χρησιμοποιήστε την <strong>προεπιλεγμένη</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Ομάδα ενέργεια μεταβλητή</th><th>Περιγραφή</th></tr>
<tr><td>${hiveDataFolder}</td><td>Καθορίζει τον κατάλογο προέλευσης για την εντολή Hive Δημιουργία πίνακα.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Καθορίζει το φάκελο εξόδου για τη δήλωση εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ.</td></tr>
<tr><td>${hiveTableName}</td><td>Καθορίζει το όνομα του πίνακα Hive που αναφέρεται τα αρχεία δεδομένων log4j.</td></tr>
</table>

<table border = "1">
<tr><th>Μεταβλητή Sqoop ενέργειας</th><th>Περιγραφή</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Καθορίζει τη συμβολοσειρά σύνδεσης βάσης δεδομένων Azure SQL.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Καθορίζει τον πίνακα βάσης δεδομένων Azure SQL όπου τα δεδομένα θα εξαχθούν.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Καθορίζει το φάκελο εξόδου για τη δήλωση Hive εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ. Αυτό είναι στον ίδιο φάκελο για την εξαγωγή Sqoop (εξαγωγή-dir).</td></tr>
</table>

Για περισσότερες πληροφορίες σχετικά με τη ροή εργασίας Oozie και χρησιμοποιώντας ενέργειες ροής εργασίας, ανατρέξτε [στην τεκμηρίωση Apache Oozie 4.0] [ apache-oozie-400] (για το HDInsight έκδοση 3.0) ή [τεκμηρίωση Apache Oozie 3.3.2] [ apache-oozie-332] (για το HDInsight έκδοση 2.1).


Η ενέργεια Hive στη ροή εργασίας καλεί ένα αρχείο δέσμης ενεργειών HiveQL. Αυτό το αρχείο δέσμης ενεργειών περιέχει τρεις δηλώσεις HiveQL:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **Πρόταση DROP TABLE το** διαγράφει τον πίνακα Hive log4j, εάν υπάρχει.
2. **Πρόταση CREATE TABLE το** δημιουργεί έναν πίνακα εξωτερική ομάδα log4j που αφορά τη θέση του αρχείου καταγραφής log4j. Ο οριοθέτης πεδίου είναι ",". Ο προεπιλεγμένος οριοθέτης γραμμή είναι "\n". Για να αποφύγετε το αρχείο δεδομένων που αφαιρείται από την αρχική του θέση, εάν θέλετε να εκτελέσετε τη ροή εργασίας Oozie πολλές φορές χρησιμοποιείται ένας πίνακας εξωτερική ομάδα.
3. **Η εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ δήλωση** μετρά τις εμφανίσεις κάθε τύπο επίπεδο καταγραφής από τον πίνακα ομάδα log4j και αποθηκεύει το αποτέλεσμα σε ένα αντικείμενο blob στο χώρο αποθήκευσης Azure.


Υπάρχουν τρεις μεταβλητές που χρησιμοποιούνται στη δέσμη ενεργειών:

- ${hiveTableName}
- ${hiveDataFolder}
- ${hiveOutputFolder}

Το αρχείο ορισμού ροής εργασίας (workflow.xml σε αυτό το πρόγραμμα εκμάθησης) μεταβιβάζει αυτές τις τιμές σε αυτήν τη δέσμη ενεργειών HiveQL κατά το χρόνο εκτέλεσης.

Το αρχείο ροής εργασίας και το αρχείο HiveQL είναι αποθηκευμένες σε ένα κοντέινερ αντικειμένων blob.  Η δέσμη ενεργειών του PowerShell που θα χρησιμοποιήσετε αργότερα σε αυτό το πρόγραμμα εκμάθησης θα αντιγράψετε και τα δύο αρχεία στον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης. 

##<a name="submit-oozie-jobs-using-powershell"></a>Υποβολή Oozie εργασιών με χρήση του PowerShell

Azure PowerShell αυτήν τη στιγμή δεν παρέχει οποιαδήποτε cmdlet του για τον ορισμό Oozie εργασίες. Μπορείτε να χρησιμοποιήσετε το cmdlet **Ενεργοποίηση RestMethod** για να καλέσετε Oozie υπηρεσιών web. Το API των υπηρεσιών web Oozie είναι μια REST API JSON HTTP. Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες web του Oozie API, ανατρέξτε [στην τεκμηρίωση Apache Oozie 4.0] [ apache-oozie-400] (για το HDInsight έκδοση 3.0) ή [τεκμηρίωση Apache Oozie 3.3.2] [ apache-oozie-332] (για το HDInsight έκδοση 2.1).

Η δέσμη ενεργειών PowerShell σε αυτήν την ενότητα εκτελεί τα ακόλουθα βήματα:

1. Σύνδεση με Azure.
2. Δημιουργία ομάδας του Azure πόρων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](../powershell-azure-resource-manager.md).
3. Δημιουργήστε ένα διακομιστή βάσης δεδομένων SQL Azure, μια βάση δεδομένων Azure SQL και τους δύο πίνακες. Αυτά που χρησιμοποιούνται από την ενέργεια Sqoop στη ροή εργασίας.

    Το όνομα του πίνακα είναι *log4jLogCount*.

4. Δημιουργήστε ένα σύμπλεγμα HDInsight που χρησιμοποιείται για την εκτέλεση εργασιών Oozie.

    Για να εξετάσετε το σύμπλεγμα, μπορείτε να χρησιμοποιήσετε το Azure πύλη ή Azure PowerShell.

5. Αντιγράψτε το αρχείο oozie ροής εργασίας και το αρχείο δέσμης ενεργειών HiveQL το προεπιλεγμένο σύστημα αρχείων.

    Και τα δύο αρχεία αποθηκεύονται σε ένα κοντέινερ αντικειμένων Blob δημόσια.
    
    - Αντιγράψτε τη δέσμη ενεργειών HiveQL (useoozie.hql) με το χώρο αποθήκευσης Azure (wasbs:///tutorials/useoozie/useoozie.hql).
    - Αντιγραφή workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
    - Αντιγράψτε το αρχείο δεδομένων (/ example/data/sample.log) για να wasbs:///tutorials/useoozie/data/sample.log.
     
6. Υποβάλετε μια εργασία Oozie.

    Για να εξετάσετε τα αποτελέσματα OOzie εργασία, χρησιμοποιήστε το Visual Studio ή άλλα εργαλεία για να συνδεθείτε με τη βάση δεδομένων SQL Azure.

Ακολουθεί η δέσμη ενεργειών.  Μπορείτε να εκτελέσετε τη δέσμη ενεργειών από το Windows PowerShell ISE. Πρέπει να ρυθμίσετε τις παραμέτρους των μεταβλητών πρώτα 7.

    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure subscription ID>"
    
    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"
    
    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # The default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"
    
    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion
    
    #region - Create SQL database tables
    Write-Host "Creating the log4jlogs table  ..." -ForegroundColor Green
    
    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    #endregion
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
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
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - copy Oozie workflow and HiveQL files
    
    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green
    
    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    
    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force
    
    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml
    
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql
    
    #endregion
    
    #region - copy the sample.log file
    
    Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
    
    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 
    
    #validate the copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log
    
    #endregion
    
    #region - submit Oozie job
    
    $storageUri="wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"
    
    $oozieJobName = $namePrefix + "OozieJob"
    
    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10
    
    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"
    
    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
    
    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>
    
    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>
    
    <property>
        <name>queueName</name>
        <value>default</value>
    </property>
    
    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>
    
    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>
    
    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>
    
    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>
    
    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>
    
    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>
    
    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>
    
    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>
    
    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>
    
    </configuration>
    "@
    
    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."
    
    # create Oozie job
    Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug
    
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."
    
    # start Oozie job
    Write-Host "Starting the Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug
    
    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
    
    Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")
    
    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }
    
    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green
    
    #endregion


**Για να εκτελέσετε ξανά το πρόγραμμα εκμάθησης**

Για να εκτελέσετε ξανά τη ροή εργασίας, πρέπει να διαγράψετε τα εξής:

- Το αρχείο εξόδου Hive δέσμης ενεργειών
- Τα δεδομένα στον πίνακα log4jLogsCount

Ακολουθεί ένα δείγμα δέσμης ενεργειών του PowerShell που μπορείτε να χρησιμοποιήσετε:

    $resourceGroupName = "<AzureResourceGroupName>"
    
    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να ορίσετε μια ροή εργασίας Oozie και πώς μπορείτε να εκτελέσετε μια εργασία Oozie με χρήση του PowerShell. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

- [Χρήση βάσει χρόνου συντονισμού Oozie με HDInsight][hdinsight-oozie-coordinator-time]
- [Γρήγορα αποτελέσματα με το Hadoop με ομάδα στο HDInsight για να αναλύσετε Χρήση ακουστικού κινητές συσκευές][hdinsight-get-started]
- [Χρήση του χώρου αποθήκευσης αντικειμένων Blob του Azure με HDInsight][hdinsight-storage]
- [Διαχείριση HDInsight χρήση του PowerShell][hdinsight-admin-powershell]
- [Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight][hdinsight-upload-data]
- [Χρήση Sqoop με Hadoop σε HDInsight][hdinsight-use-sqoop]
- [Χρήση της ομάδας με Hadoop σε HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με Hadoop σε HDInsight][hdinsight-use-pig]
- [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

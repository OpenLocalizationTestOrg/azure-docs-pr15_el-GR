<properties
    pageTitle="Χρήση Hadoop Sqoop σε HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell από μια σταθμούς εργασίας για να εκτελέσετε Sqoop εισαγωγή και εξαγωγή ανάμεσα σε ένα σύμπλεγμα Hadoop και μια βάση δεδομένων Azure SQL."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="run-sqoop-jobs-using-azure-powershell-for-hadoop-in-hdinsight"></a>Εκτέλεση Sqoop εργασιών με χρήση του Azure PowerShell για Hadoop σε HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell για να εκτελέσετε εργασίες Sqoop στο HDInsight εισαγωγής και εξαγωγής μεταξύ συμπλεγμάτων HDInsight και βάση δεδομένων Azure SQL ή βάση δεδομένων SQL Server.

> [AZURE.NOTE] Τα βήματα σε αυτό το άρθρο μπορούν να χρησιμοποιηθούν με είτε ένα βασίζεται σε Windows ή Linux HDInsight σύμπλεγμα; Ωστόσο, αυτά τα βήματα θα λειτουργεί μόνο από ένα πρόγραμμα-πελάτη των Windows. Για άλλες μεθόδους υποβολής εργασία, κάντε κλικ στον επιλογέα στηλοθετών στο επάνω μέρος του άρθρου.


###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Σύμπλεγμα A Hadoop στο HDInsight**. Ανατρέξτε στο θέμα [Δημιουργία σύμπλεγμα και βάση δεδομένων SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

    
## <a name="run-sqoop-using-powershell"></a>Εκτέλεση Sqoop χρήση του PowerShell

Η ακόλουθη δέσμη ενεργειών PowerShell προ-επεξεργάζεται το αρχείο προέλευσης και εξάγει σε μια βάση δεδομένων Azure SQL:

    $resourceGroupName = "<AzureResourceGroupName>"
    $hdinsightClusterName = "<HDInsightClusterName>"

    $httpUserName = "admin"
    $httpPassword = "<Password>"

    $defaultStorageAccountName = $hdinsightClusterName + "store"
    $defaultBlobContainerName = $hdinsightClusterName


    $sqlDatabaseServerName = $hdinsightClusterName + "dbserver"
    $sqlDatabaseName = $hdinsightClusterName + "db"
    $sqlDatabaseLogin = "sqluser"
    $sqlDatabasePassword = "<Password>"

    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
        
    #region - pre-process the source file
        
    Write-Host "`nPreprocessing the source file ..." -ForegroundColor Green
        
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
        
    # Define the connection string
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $defaultStorageAccountName
    $storageContainer = ($storageAccount |Get-AzureStorageContainer -Name $defaultBlobContainerName).CloudBlobContainer
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
        
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
        
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
        
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
        
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
        
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
        
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
        
    #endregion
        
    #region - export the log file from the cluster to the SQL database
        
    Write-Host "Exporting the log file ..." -ForegroundColor Green

    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
        
    $tableName_log4j = "log4jlogs"
    $exportDir_log4j = "/tutorials/usesqoop/data"
    $sqljdbcdriver = "/user/oozie/share/lib/sqoop/sqljdbc41.jar"
        
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1" `
        -Files $sqljdbcdriver

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
        
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    #endregion

##<a name="limitations"></a>Περιορισμοί

* Μαζική export - βάσει με Linux HDInsight, Sqoop σύνδεσης που χρησιμοποιείται για την εξαγωγή δεδομένων Microsoft SQL Server ή βάση δεδομένων SQL Azure δεν υποστηρίζει τη συγκεκριμένη στιγμή μαζικής εισάγεται.

* Δέσμης - με βάσει Linux HDInsight, όταν χρησιμοποιείτε το `-batch` εναλλαγή κατά την εκτέλεση εισάγει, Sqoop θα εκτελέσει πολλών εισάγει αντί για δέσμης για τις λειτουργίες εισαγωγή.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να χρησιμοποιήσετε Sqoop. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Χρήση Oozie με HDInsight](hdinsight-use-oozie.md): χρήση Sqoop ενεργειών σε μια ροή εργασίας Oozie.
- [Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση HDInsight](hdinsight-analyze-flight-delay-data.md): Χρησιμοποιήστε την ομάδα για την ανάλυση πτήσεων καθυστέρηση δεδομένων και κατόπιν χρησιμοποιήστε Sqoop να εξαγάγετε τα δεδομένα σε μια βάση δεδομένων Azure SQL.
- [Αποστολή δεδομένων με το HDInsight](hdinsight-upload-data.md): βρείτε άλλες μεθόδους για την αποστολή δεδομένων με το χώρο αποθήκευσης αντικειμένων Blob HDInsight/Azure.


[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

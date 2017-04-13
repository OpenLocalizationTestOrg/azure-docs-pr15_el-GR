<properties 
   pageTitle="Διαχείριση Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure | Azure" 
   description="Μάθετε πώς μπορείτε να διαχειριστείτε εργασίες ανάλυσης δεδομένων λίμνης, προελεύσεις δεδομένων, οι χρήστες. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Διαχείριση Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Μάθετε πώς μπορείτε να διαχειριστείτε λογαριασμοί Azure δεδομένων λίμνης ανάλυση, προελεύσεις δεδομένων, οι χρήστες και εργασίες χρησιμοποιώντας το PowerShell Azure. Για να δείτε θέματα διαχείρισης με χρήση άλλων εργαλείων, κάντε κλικ στην επιλογή καρτέλα, επιλέξτε το παραπάνω.

**Προαπαιτούμενα στοιχεία**

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Εγκατάσταση του Azure PowerShell 1.0 ή μεγαλύτερο

Ανατρέξτε στην ενότητα προαπαιτούμενες με [Χρήση του PowerShell Azure με τη διαχείριση πόρων Azure](powershell-azure-resource-manager.md#prerequisites).
    
## <a name="manage-accounts"></a>Διαχείριση λογαριασμών

Πριν από την εκτέλεση ανάλυσης δεδομένων λίμνης εργασίες, πρέπει να έχετε ένα λογαριασμό ανάλυση λίμνης δεδομένων. Σε αντίθεση με Azure HDInsight, δεν πληρώνετε για ένα λογαριασμό ανάλυσης όταν δεν εκτελείται μια εργασία.  Πληρώνετε μόνο για την περίοδο λειτουργίας όταν εκτελείται μια εργασία.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση ανάλυσης λίμνης Azure δεδομένων](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Δημιουργία λογαριασμών

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
    Write-Host "Create a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location
    
    Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
    New-AzureRmDataLakeStoreAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeStoreName `
        -Location $location 
    
    Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
    New-AzureRmDataLakeAnalyticsAccount `
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Μπορείτε επίσης να χρησιμοποιήσετε ένα πρότυπο Azure ομάδα πόρων. Το πρότυπο για τη δημιουργία ενός λογαριασμού δεδομένων λίμνης ανάλυση και το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης εξαρτώμενα είναι στο [παράρτημα A](#appendix-a). Αποθηκεύστε το πρότυπο σε ένα αρχείο με το πρότυπο .json και, στη συνέχεια, χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του PowerShell για να καλέσετε αυτό:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Λίστα λογαριασμού

Οι λογαριασμοί ανάλυση λίμνης δεδομένων λίστας στο την τρέχουσα συνδρομή σας

    Get-AzureRmDataLakeAnalyticsAccount
    
Το αποτέλεσμα:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Λογαριασμοί ανάλυση λίμνης δεδομένων λίστας μέσα σε μια ομάδα συγκεκριμένο πόρο

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Λήψη λεπτομερειών από ένα συγκεκριμένο λογαριασμό ανάλυσης δεδομένων λίμνης

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Δοκιμή ύπαρξη ένα συγκεκριμένο λογαριασμό ανάλυσης δεδομένων λίμνης

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Το cmdlet θα επιστρέψει **True** ή **False**.

###<a name="delete-data-lake-analytics-accounts"></a>Διαγραφή λογαριασμών ανάλυσης δεδομένων λίμνης

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Διαγραφή δεδομένων λίμνης αναλυτικών στοιχείων λογαριασμού δεν διαγράφει το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης εξαρτώμενα. Το παρακάτω παράδειγμα διαγράφει το λογαριασμό ανάλυση λίμνης δεδομένων και τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Διαχείριση προελεύσεων δεδομένων λογαριασμού

Ανάλυση δεδομένων λίμνης υποστηρίζει αυτήν τη στιγμή τις ακόλουθες προελεύσεις δεδομένων:

- [Χώρος αποθήκευσης λίμνης δεδομένων Azure](../data-lake-store/data-lake-store-overview.md)
- [Azure χώρου αποθήκευσης](storage-introduction.md)

Όταν δημιουργείτε ένα λογαριασμό ανάλυσης, πρέπει να καθορίσετε ένα λογαριασμό αποθήκευσης λίμνης Azure δεδομένων για να τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης. Τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης χρησιμοποιείται για την αποθήκευση έργων μετα-δεδομένων και εργασία αρχείων καταγραφής ελέγχου. Αφού δημιουργήσετε ένα λογαριασμό ανάλυση, μπορείτε να προσθέσετε επιπλέον λογαριασμούς χώρος αποθήκευσης δεδομένων λίμνης ή/και το χώρο αποθήκευσης Azure λογαριασμού. 

### <a name="find-the-default-data-lake-store-account"></a>Βρείτε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Προσθήκη επιπλέον λογαριασμούς χώρο αποθήκευσης αντικειμένων Blob του Azure

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>Προσθήκη επιπλέον λογαριασμούς χώρου αποθήκευσης δεδομένων λίμνης

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Λίστα προελεύσεων δεδομένων:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Διαχείριση εργασιών

Πρέπει να έχετε ένα λογαριασμό ανάλυση λίμνης δεδομένων για να δημιουργήσετε μια εργασία.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση ανάλυσης λίμνης δεδομένων λογαριασμών](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Λίστα εργασιών

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Λήψη λεπτομερειών έργου

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Υποβολή εργασιών

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Η προεπιλεγμένη προτεραιότητα μιας εργασίας είναι 1000 και προεπιλεγμένη βαθμού παραλληλισμό για μια εργασία είναι 1.


### <a name="cancel-jobs"></a>Ακύρωση εργασιών

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Διαχείριση των στοιχείων καταλόγου

Τον κατάλογο U-SQL χρησιμοποιείται για τη Δόμηση δεδομένων και κώδικα, ώστε να είναι κοινόχρηστη με δέσμες ενεργειών U-SQL. Τον κατάλογο επιτρέπει την υψηλότερη απόδοση πιθανές με δεδομένα σε Azure λίμνης δεδομένων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση U-SQL στον κατάλογο](data-lake-analytics-use-u-sql-catalog.md).

###<a name="list-catalog-items"></a>Λίστα στοιχείων καταλόγου

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Λήψη λεπτομερειών στοιχείου καταλόγου 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Δοκιμή ύπαρξη του στοιχείου καταλόγου

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Δημιουργία μυστικό καταλόγου
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Τροποποίηση μυστικό καταλόγου
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Διαγραφή μυστικό καταλόγου
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Χρήση των ομάδων διαχείρισης πόρων Azure

Εφαρμογές είναι συνήθως αποτελείται από πολλά στοιχεία, για παράδειγμα μια εφαρμογή web, βάση δεδομένων, διακομιστή βάσης δεδομένων, χώρος αποθήκευσης και υπηρεσίες τρίτου κατασκευαστή. Διαχείριση Azure πόρου (ARM) σάς επιτρέπει να εργαστείτε με τους πόρους στην εφαρμογή σας ως ομάδα, γνωστή ως μια ομάδα πόρων του Azure. Να αναπτύξετε, ενημέρωση, παρακολούθηση ή να διαγράψετε όλους τους πόρους για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία. Μπορείτε να χρησιμοποιήσετε ένα πρότυπο για ανάπτυξη και μπορεί να λειτουργήσει αυτό το πρότυπο για διαφορετικά περιβάλλοντα όπως δοκιμές, ανάπτυξης και παραγωγής. Να ξεκαθαρίσετε χρεώσεις για την εταιρεία σας, προβάλλοντας το κόστος πολλαπλών επιπέδων για ολόκληρη την ομάδα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md). 

Μια υπηρεσία ανάλυσης λίμνης δεδομένων μπορεί να περιλαμβάνει τα παρακάτω στοιχεία:

- Azure δεδομένων λίμνης αναλυτικών στοιχείων λογαριασμού
- Απαιτείται προεπιλεγμένου λογαριασμού αποθήκευσης λίμνης δεδομένων Azure
- Λογαριασμοί λίμνης δεδομένων Azure επιπλέον χώρου αποθήκευσης
- Επιπλέον χώρο αποθήκευσης Azure λογαριασμούς

Μπορείτε να δημιουργήσετε όλα αυτά τα στοιχεία στην περιοχή μία ομάδα ARM ώστε να είναι πιο εύκολο να διαχειριστείτε.

![Λογαριασμό αναλυτικών στοιχείων Azure λίμνης δεδομένα και αποθήκευσης](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Ένα λογαριασμό ανάλυση λίμνης δεδομένων και τους λογαριασμούς εξαρτώμενα αποθήκευσης πρέπει να τοποθετηθεί στο ίδιο κέντρο Azure δεδομένων.
Ομάδα ARM μπορεί ωστόσο να βρίσκεται σε ένα κέντρο διαφορετικά δεδομένα.  

##<a name="see-also"></a>Δείτε επίσης 

- [Επισκόπηση της ανάλυσης λίμνης δεδομένων Microsoft Azure](data-lake-analytics-overview.md)
- [Γρήγορα αποτελέσματα με το ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-get-started-portal.md)
- [Διαχείριση Azure ανάλυση λίμνης δεδομένων με την πύλη Azure](data-lake-analytics-manage-use-portal.md)
- [Παρακολούθηση και αντιμετώπιση προβλημάτων του Azure δεδομένων λίμνης ανάλυσης εργασιών με πύλη Azure](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Παράρτημα A - ARM ανάλυσης δεδομένων λίμνης προτύπου

Το ακόλουθο πρότυπο ARM μπορεί να χρησιμοποιηθεί για να αναπτύξετε ένα λογαριασμό δεδομένων λίμνης ανάλυση και εξαρτώμενα λογαριασμό χώρου αποθήκευσης λίμνης δεδομένων.  Αποθηκεύστε το ως αρχείο json και, στη συνέχεια, χρησιμοποιήστε δέσμη ενεργειών του PowerShell για να καλέσετε το πρότυπο. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](../resource-group-template-deploy.md#deploy-with-powershell) και [σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα](../resource-group-authoring-templates.md).

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }


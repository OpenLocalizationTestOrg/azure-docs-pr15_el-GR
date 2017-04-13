<properties 
   pageTitle="Γρήγορα αποτελέσματα με το Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure | Azure" 
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το PowerShell Azure για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης λίμνης δεδομένων, να δημιουργήσετε μια εργασία ανάλυσης λίμνης δεδομένων χρησιμοποιώντας U-SQL και υποβάλετε την εργασία. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Azure ανάλυση λίμνης δεδομένων με χρήση του PowerShell Azure

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το Azure PowerShell για να δημιουργήσετε λογαριασμούς Azure δεδομένων λίμνης ανάλυση, Ορισμός ανάλυσης δεδομένων λίμνης εργασίες στο [U-SQL](data-lake-analytics-u-sql-get-started.md)και υποβολή εργασιών σε δεδομένα λίμνης αναλυτική λογαριασμούς. Για περισσότερες πληροφορίες σχετικά με την ανάλυση λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-overview.md).

Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να αναπτύξετε ένα έργο που διαβάζει μια καρτέλα διαχωρισμένες αρχείο τιμών (TSV) και τον μετατρέπει σε ένα αρχείο τιμών διαχωρισμένων με κόμματα (CSV). Για να μεταβείτε στο ίδιο πρόγραμμα εκμάθησης με χρήση άλλων εργαλείων υποστηριζόμενες, κάντε κλικ στις καρτέλες στο επάνω μέρος αυτής της ενότητας.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).
- **Μια σταθμούς εργασίας με το Azure PowerShell**. Δείτε [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Δημιουργία λογαριασμού ανάλυσης δεδομένων λίμνης

Πρέπει να έχετε ένα λογαριασμό ανάλυσης δεδομένων λίμνης μπορέσετε να εκτελέσετε τις εργασίες. Για να δημιουργήσετε ένα λογαριασμό ανάλυση λίμνης δεδομένων, πρέπει να καθορίσετε τα εξής:

- **Ομάδα πόρων Azure**: ανάλυση λίμνης δεδομένων A λογαριασμό πρέπει να δημιουργηθεί μέσα σε μια ομάδα πόρων Azure. [Διαχείριση πόρων Azure](../azure-resource-manager/resource-group-overview.md) σάς επιτρέπει να εργαστείτε με τους πόρους στην εφαρμογή σας ως ομάδα. Να αναπτύξετε, ενημέρωση ή να διαγράψετε όλους τους πόρους για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία.  

    Απαρίθμηση των ομάδων πόρων στη συνδρομή σας:
    
        Get-AzureRmResourceGroup
    
    Για να δημιουργήσετε μια νέα ομάδα πόρων:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Το όνομα του λογαριασμού ανάλυσης δεδομένων λίμνης**
- **Θέση**: ένα από τα κέντρα δεδομένων Azure που υποστηρίζει ανάλυση λίμνης δεδομένων.
- **Προεπιλεγμένη δεδομένων λίμνης λογαριασμό**: κάθε λογαριασμό ανάλυση λίμνης δεδομένων διαθέτει έναν προεπιλεγμένο λογαριασμό λίμνης δεδομένων.

    Για να δημιουργήσετε ένα νέο λογαριασμό λίμνης δεδομένων:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Το όνομα του λογαριασμού λίμνης δεδομένων πρέπει να περιέχει μόνο πεζά γράμματα και αριθμούς.



**Για να δημιουργήσετε ένα λογαριασμό ανάλυσης δεδομένων λίμνης**

1. Ανοίξτε το PowerShell ISE από το σταθμούς εργασίας των Windows.
2. Εκτελέστε την ακόλουθη δέσμη ενεργειών:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
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
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Αποστολή δεδομένων στο λίμνης δεδομένων

Σε αυτό το πρόγραμμα εκμάθησης, θα επεξεργαστεί ορισμένα αρχεία καταγραφής από την αναζήτηση.  Το αρχείο καταγραφής αναζήτησης μπορούν να αποθηκευτούν στο χώρο αποθήκευσης δεδομένων λίμνης ή χώρο αποθήκευσης αντικειμένων Blob του Azure. 

Ένα δείγμα αρχείου καταγραφής αναζήτησης έχει αντιγραφεί σε δημόσια κοντέινερ αντικειμένων Blob του Azure. Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών του PowerShell για να κάνετε λήψη του αρχείου για να σας σταθμούς εργασίας και, στη συνέχεια, αποστείλετε το αρχείο για να τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης του λογαριασμού σας ανάλυση λίμνης δεδομένων.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Η ακόλουθη δέσμη ενεργειών PowerShell δείχνει πώς μπορώ να αποκτήσω το προεπιλεγμένο όνομα χώρου αποθήκευσης λίμνης δεδομένων για ένα λογαριασμό ανάλυση λίμνης δεδομένων:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Η πύλη Azure παρέχει ένα περιβάλλον εργασίας χρήστη για να αντιγράψετε τα δείγματα αρχείων δεδομένων του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης λίμνης δεδομένων. Για οδηγίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυση με Azure πύλη](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Ανάλυση δεδομένων λίμνης επίσης να αποκτήσετε πρόσβαση χώρος αποθήκευσης αντικειμένων Blob του Azure.  Για την αποστολή δεδομένων με το χώρο αποθήκευσης αντικειμένων Blob του Azure, ανατρέξτε στο θέμα [Χρήση του PowerShell Azure με το χώρο αποθήκευσης Azure](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Υποβολή δεδομένων λίμνης ανάλυσης εργασιών

Οι εργασίες ανάλυσης λίμνης δεδομένων εγγράφονται στη γλώσσα U-SQL. Για να μάθετε περισσότερα σχετικά με το U-SQL, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το γλώσσα U SQL](data-lake-analytics-u-sql-get-started.md) και [αναφορά γλώσσας U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Για να δημιουργήσετε μια δέσμη ενεργειών εργασία ανάλυσης δεδομένων λίμνης**

- Δημιουργήστε ένα αρχείο κειμένου με την ακόλουθη δέσμη ενεργειών U-SQL και αποθηκεύστε το αρχείο κειμένου για να σας σταθμούς εργασίας:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Αυτή η δέσμη ενεργειών U-SQL διαβάζει το αρχείο προέλευσης δεδομένων με χρήση **Extractors.Tsv()**και, στη συνέχεια, δημιουργεί ένα αρχείο csv χρησιμοποιώντας **Outputters.Csv()**. 
    
    Μην τροποποιείτε τις δύο διαδρομές, εκτός εάν μπορείτε να αντιγράψετε το αρχείο προέλευσης σε διαφορετική θέση.  Ανάλυση λίμνης δεδομένων θα δημιουργήσει το φάκελο εξόδου, εάν δεν υπάρχει.
    
    Είναι ευκολότερο να χρησιμοποιήσετε σχετικές διαδρομές για προεπιλεγμένες αποθηκευμένα τα αρχεία δεδομένων λίμνης λογαριασμών. Μπορείτε επίσης να χρησιμοποιήσετε απόλυτες διαδρομές.  Για παράδειγμα 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Πρέπει να χρησιμοποιήσετε απόλυτες διαδρομές για να αποκτήσετε πρόσβαση σε αρχεία σε συνδεδεμένους λογαριασμούς χώρου αποθήκευσης.  Η σύνταξη για τα αρχεία που είναι αποθηκευμένα σε συνδεδεμένο λογαριασμό Azure χώρου αποθήκευσης είναι:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob κοντέινερ με αντικείμενα BLOB δημόσια ή τα δικαιώματα πρόσβασης δημόσια κοντέινερ δεν υποστηρίζονται αυτήν τη στιγμή.    
    
    
**Για να υποβάλετε την εργασία**

1. Ανοίξτε το PowerShell ISE από το σταθμούς εργασίας των Windows.
2. Εκτελέστε την ακόλουθη δέσμη ενεργειών:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Στη δέσμη ενεργειών, το αρχείο δέσμης ενεργειών U-SQL είναι αποθηκευμένη σε c:\tutorials\data-lake-analytics\copyFile.usql. Ενημερώστε τη διαδρομή του αρχείου ανάλογα.
 
Μετά την ολοκλήρωση της εργασίας, μπορείτε να χρησιμοποιήσετε τα ακόλουθα cmdlet για να εμφανίσετε το αρχείο και κάντε λήψη του αρχείου:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Δείτε επίσης

- Για να δείτε το ίδιο πρόγραμμα εκμάθησης με χρήση άλλων εργαλείων, κάντε κλικ στην επιλογή των δεικτών επιλογής καρτέλα στο επάνω μέρος της σελίδας.
- Για να δείτε ένα πιο σύνθετο ερώτημα, ανατρέξτε στο θέμα [ανάλυση τοποθεσίας Web αρχεία καταγραφής χρησιμοποιώντας Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-analyze-weblogs.md).
- Για να ξεκινήσετε την ανάπτυξη εφαρμογών U-SQL, ανατρέξτε στο θέμα [Ανάπτυξη U-SQL δέσμης ενεργειών με χρήση εργαλεία λίμνης δεδομένων για το Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Για να μάθετε U-SQL, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το δεδομένων λίμνης ανάλυση U-SQL Azure γλώσσας](data-lake-analytics-u-sql-get-started.md).
- Για τις εργασίες διαχείρισης, ανατρέξτε στο θέμα [Διαχείριση Azure δεδομένων λίμνης αναλυτικών στοιχείων χρήσης Azure πύλη](data-lake-analytics-manage-use-portal.md).
- Για να δείτε μια επισκόπηση της ανάλυσης λίμνης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση Azure δεδομένων λίμνης ανάλυσης](data-lake-analytics-overview.md).


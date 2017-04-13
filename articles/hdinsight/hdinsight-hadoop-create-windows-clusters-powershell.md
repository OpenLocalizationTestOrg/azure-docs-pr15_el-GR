<properties
   pageTitle="Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στη χρήση του Azure PowerShell HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε συμπλεγμάτων για Azure HDInsight με χρήση του Azure PowerShell."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στη χρήση του Azure PowerShell HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Μάθετε πώς να δημιουργείτε χρησιμοποιώντας το Azure PowerShell συμπλεγμάτων HDInsight. Azure PowerShell είναι μια λειτουργική μονάδα που παρέχει το cmdlet για τη Διαχείριση Azure με το Windows PowerShell. Για άλλες δημιουργία συμπλέγματος και εργαλεία που διαθέτει κάντε κλικ στην επιλογή καρτέλα, επιλέξτε στο επάνω μέρος αυτής της σελίδας ή [μεθόδους δημιουργίας σύμπλεγμα](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Πριν να ξεκινήσετε τις οδηγίες σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- Azure συνδρομή. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Δημιουργία συμπλεγμάτων
Azure PowerShell είναι ένα ισχυρό περιβάλλον δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να ελέγξετε και να αυτοματοποιήσετε την ανάπτυξη και τη διαχείριση των σας φόρτους εργασίας στο Azure. Αυτή η ενότητα παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα σύμπλεγμα HDInsight με χρήση του Azure PowerShell. Για πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων ενός σταθμούς εργασίας για να εκτελέσετε τα cmdlet του HDInsight Windows PowerShell, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure](../powershell-install-configure.md). Για περισσότερες πληροφορίες σχετικά με τη χρήση του PowerShell Azure με το HDInsight, ανατρέξτε στο θέμα [Διαχείριση του HDInsight χρήση του PowerShell](hdinsight-administer-use-powershell.md). Για τη λίστα με τα cmdlet του Windows PowerShell HDInsight, ανατρέξτε στο θέμα [αναφορά cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Για να δημιουργήσετε ένα σύμπλεγμα HDInsight με χρήση του Azure PowerShell απαιτούνται τις παρακάτω διαδικασίες:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Δημιουργία συμπλεγμάτων χρησιμοποιώντας το πρότυπο ARM

Μπορείτε να χρησιμοποιήσετε Azure PowerShell για να αναπτύξετε ένα πρότυπο ARM, η οποία δημιουργεί ένα σύμπλεγμα HDInsight.  Ανατρέξτε στο θέμα [κλήσης με χρήση του PowerShell Azure πρότυπα](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Προσαρμογή συμπλεγμάτων

- Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων χρησιμοποιώντας εκκίνησης](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Ανατρέξτε στο θέμα [Προσαρμογή των Windows βάσει HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο μάθατε διάφορους τρόπους για να δημιουργήσετε ένα σύμπλεγμα HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Γρήγορα αποτελέσματα με το Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) - μάθετε πώς μπορείτε να αρχίσετε να εργάζεστε με το σύμπλεγμά σας HDInsight
* [Υποβολή Hadoop έργα μέσω προγραμματισμού](hdinsight-submit-hadoop-jobs-programmatically.md) - μάθετε πώς μπορείτε να υποβάλετε μέσω προγραμματισμού εργασιών με το HDInsight
* [Διαχείριση Hadoop συμπλεγμάτων στη χρήση του PowerShell HDInsight](hdinsight-administer-use-powershell.md) - μάθετε πώς μπορείτε να εργαστείτε με HDInsight με χρήση του Azure PowerShell
* [Azure HDInsight SDK τεκμηρίωση]  [ hdinsight-sdk-documentation] -Ανακαλύψτε το HDInsight SDK




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx

<properties
    pageTitle="Δημιουργία συμπλεγμάτων Hadoop, HBase, καταιγίδας ή τους σε Linux στη χρήση του Azure PowerShell HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε Hadoop, HBase, καταιγίδας ή τους συμπλεγμάτων σε Linux για HDInsight με χρήση του Azure PowerShell."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Δημιουργία βάσει Linux συμπλεγμάτων στο HDInsight με χρήση του Azure PowerShell

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure PowerShell είναι ένα ισχυρό περιβάλλον δέσμης ενεργειών που μπορείτε να χρησιμοποιήσετε για να ελέγξετε και να αυτοματοποιήσετε την ανάπτυξη και τη διαχείριση των σας φόρτους εργασίας του στο Microsoft Azure. Αυτό το έγγραφο παρέχει πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε ένα σύμπλεγμα βάσει Linux HDInsight με χρήση του Azure PowerShell. Περιλαμβάνει επίσης ένα παράδειγμα δέσμης ενεργειών.

> [AZURE.NOTE] Azure PowerShell είναι διαθέσιμη μόνο σε υπολογιστές-πελάτες Windows. Εάν χρησιμοποιείτε ένα πρόγραμμα-πελάτη Linux, Unix ή το Mac OS X, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα βάσει Linux HDInsight χρησιμοποιώντας Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) για πληροφορίες σχετικά με τη χρήση του Azure CLI για να δημιουργήσετε ένα σύμπλεγμα.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Πρέπει να έχετε τα ακόλουθα πριν ξεκινήσετε αυτήν τη διαδικασία:

- Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure PowerShell.
    Για περισσότερες πληροφορίες σχετικά με τη χρήση του PowerShell Azure με το HDInsight, ανατρέξτε στο θέμα [Διαχείριση του HDInsight χρήση του PowerShell](hdinsight-administer-use-powershell.md). Για τη λίστα των cmdlet του HDInsight Windows PowerShell, ανατρέξτε στο θέμα [αναφορά cmdlet HDInsight](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Δημιουργία συμπλεγμάτων

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Για να δημιουργήσετε ένα σύμπλεγμα HDInsight με χρήση του Azure PowerShell, πρέπει να ολοκληρώσετε τις παρακάτω διαδικασίες:

- Δημιουργία ομάδας του Azure πόρων
- Δημιουργήστε ένα λογαριασμό αποθήκευσης Azure
- Δημιουργία ενός κοντέινερ αντικειμένων Blob του Azure
- Δημιουργήστε ένα σύμπλεγμα HDInsight

Τις δύο παραμέτρους πιο σημαντικές πρέπει να ρυθμίσετε για να δημιουργήσετε Linux συμπλεγμάτων είναι εκείνα που καθορίζουν τον τύπο λειτουργικό σύστημα και τις λεπτομέρειες SSH χρήστη:

- Βεβαιωθείτε ότι μπορείτε να καθορίσετε την παράμετρο **- OSType** ως **Linux**.
- Για να χρησιμοποιήσετε SSH απομακρυσμένων περιόδων λειτουργίας σε των συμπλεγμάτων, μπορείτε να καθορίσετε τον κωδικό πρόσβασης χρήστη SSH ή το δημόσιο κλειδί SSH. Εάν καθορίσετε τον κωδικό πρόσβασης χρήστη SSH και το δημόσιο κλειδί SSH, τον αριθμό-κλειδί θα αγνοούνται. Εάν θέλετε να χρησιμοποιήσετε το κλειδί SSH για περιόδους λειτουργίας απομακρυσμένης, πρέπει να καθορίσετε ένα κενό SSH τον κωδικό πρόσβασης όταν σας ζητηθεί για έναν. Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε σε ένα από τα ακόλουθα άρθρα:

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Η ακόλουθη δέσμη ενεργειών δείχνει τον τρόπο για να δημιουργήσετε ένα νέο σύμπλεγμα:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Οι τιμές που καθορίζετε για **$clusterCredentials** που χρησιμοποιούνται για να δημιουργήσετε το λογαριασμό χρήστη Hadoop για το σύμπλεγμα. Χρησιμοποιήστε αυτόν το λογαριασμό για να συνδεθείτε με το σύμπλεγμα.

Οι τιμές που καθορίζετε για **$sshCredentials** χρησιμοποιούνται για τη δημιουργία του χρήστη SSH για το σύμπλεγμα. Χρησιμοποιήστε αυτόν το λογαριασμό για να ξεκινήσετε μια απομακρυσμένη περίοδο λειτουργίας SSH στο σύμπλεγμα και να εκτελέσετε εργασίες.

> [AZURE.IMPORTANT] Σε αυτήν τη δέσμη ενεργειών, πρέπει να καθορίσετε τον αριθμό των κόμβους εργασίας που θα είναι στο σύμπλεγμα. Εάν σκοπεύετε να χρησιμοποιήσετε περισσότερους από 32 κόμβοι εργασίας (είτε με τη δημιουργία συμπλέγματος ή την κλίμακα του συμπλέγματος μετά τη δημιουργία της), πρέπει επίσης να καθορίσετε ένα μέγεθος κεφαλής κόμβο με τουλάχιστον 8 πυρήνων και 14 GB RAM.
>
> Για περισσότερες πληροφορίες σχετικά με μεγέθη κόμβο και σχετικές κόστος, ανατρέξτε στο θέμα [HDInsight τις τιμές](https://azure.microsoft.com/pricing/details/hdinsight/).

Μπορεί να χρειαστούν έως και 20 λεπτά για να δημιουργήσετε ένα σύμπλεγμα.

Το παρακάτω παράδειγμα δείχνει τον τρόπο για να προσθέσετε ένα λογαριασμό επιπλέον χώρο αποθήκευσης:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Προσαρμογή συμπλεγμάτων

- Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων χρησιμοποιώντας εκκίνησης](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Ανατρέξτε στο θέμα [Προσαρμογή των Windows βάσει HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Διαγραφή του συμπλέγματος

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει ένα σύμπλεγμα HDInsight με επιτυχία, χρησιμοποιήστε τους παρακάτω πόρους για να μάθετε πώς μπορείτε να εργαστείτε με το σύμπλεγμά σας.

### <a name="hadoop-clusters"></a>Hadoop συμπλεγμάτων

* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση MapReduce με HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase συμπλεγμάτων

* [Γρήγορα αποτελέσματα με το HBase σε HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ανάπτυξη εφαρμογών Java για HBase σε HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Καταιγίδας συμπλεγμάτων

* [Ανάπτυξη τοπολογίες Java για καταιγίδας στην HDInsight](hdinsight-storm-develop-java-topology.md)
* [Χρησιμοποιήστε τα στοιχεία Python στο καταιγίδας στην HDInsight](hdinsight-storm-develop-python-topology.md)
* [Ανάπτυξη και εποπτεία τοπολογίες με καταιγίδας στην HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Τους συμπλεγμάτων

* [Δημιουργήστε μια μεμονωμένη εφαρμογή χρησιμοποιώντας Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Απομακρυσμένη εκτέλεση εργασιών σε ένα σύμπλεγμα τους χρησιμοποιώντας Λίβιος](hdinsight-apache-spark-livy-rest-interface.md)
* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)
* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη τροφίμων στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

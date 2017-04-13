<properties
    pageTitle="Προσαρμογή με χρήση δέσμης ενεργειών συμπλεγμάτων HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να προσαρμόσετε συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Προσαρμογή συμπλεγμάτων HDInsight που βασίζεται στα Windows, με χρήση δέσμης ενεργειών

**Ενέργεια δέσμη ενεργειών** μπορεί να χρησιμοποιηθεί για να καλέσετε [προσαρμοσμένων δεσμών ενεργειών](hdinsight-hadoop-script-actions.md) κατά τη διαδικασία δημιουργίας σύμπλεγμα για την εγκατάσταση πρόσθετου λογισμικού σε ένα σύμπλεγμα.

Οι πληροφορίες σε αυτό το άρθρο αφορά συγκεκριμένα συμπλεγμάτων HDInsight που βασίζεται στα Windows. Για Linux βάσει συμπλεγμάτων, ανατρέξτε στο θέμα [Προσαρμογή Linux βάσει HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md). 

HDInsight συμπλεγμάτων μπορεί να προσαρμοστεί με διάφορους άλλους τρόπους, όπως όπως επιπλέον χώρο αποθήκευσης Azure λογαριασμούς, αλλάζοντας το Hadoop αρχείων ρύθμισης παραμέτρων του (site.xml πυρήνα, hive site.xml, κ.λπ.) ή προσθήκη θέσει σε κοινή χρήση βιβλιοθηκών (π.χ., ομάδα, Oozie) σε κοινές θέσεις στο σύμπλεγμα. Αυτές οι προσαρμογές μπορεί να γίνει μέσω του PowerShell Azure, το Azure HDInsight .NET SDK ή την πύλη Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Ενέργεια δέσμη ενεργειών στη διαδικασία δημιουργίας συμπλέγματος

Δέσμη ενεργειών ενέργεια χρησιμοποιείται μόνο ενώ μια συμπλεγμάτων δημιουργείται. Το παρακάτω διάγραμμα παρουσιάζει όταν εκτελείται κατά τη διαδικασία δημιουργίας δέσμης ενεργειών:

![Προσαρμογή σύμπλεγμα HDInsight και τα στάδια κατά τη δημιουργία συμπλέγματος][img-hdi-cluster-states]

Όταν εκτελείται η δέσμη ενεργειών, το σύμπλεγμα εισάγει το στάδιο **ClusterCustomization** . Σε αυτό το στάδιο, η δέσμη ενεργειών εκτελείται κάτω από το λογαριασμό διαχειριστή συστήματος, παράλληλα σε όλους τους κόμβους καθορισμένο στο σύμπλεγμα, και παρέχει δικαιώματα διαχειριστή πλήρους τους κόμβους.

> [AZURE.NOTE] Επειδή έχετε δικαιώματα διαχειριστή σε κόμβους συμπλέγματος κατά το στάδιο **ClusterCustomization** , μπορείτε να χρησιμοποιήσετε τη δέσμη ενεργειών για την εκτέλεση λειτουργιών όπως διακοπή και εκκίνηση των υπηρεσιών, συμπεριλαμβανομένων των υπηρεσιών που σχετίζονται με το Hadoop. Επομένως, ως μέρος της δέσμης ενεργειών, πρέπει να βεβαιωθείτε ότι οι υπηρεσίες Ambari και άλλες υπηρεσίες που σχετίζονται με το Hadoop είναι εγκατάσταση και λειτουργία πριν ολοκληρωθεί η εκτέλεση του τη δέσμη ενεργειών. Αυτές οι υπηρεσίες απαιτούνται για να διαπιστωθεί με επιτυχία την εύρυθμη λειτουργία και την κατάσταση του συμπλέγματος ενώ δημιουργείται. Εάν αλλάξετε οποιαδήποτε ρύθμιση παραμέτρων στο σύμπλεγμα που επηρεάζει αυτές τις υπηρεσίες, πρέπει να χρησιμοποιήσετε τις συναρτήσεις Βοήθειας που παρέχονται. Για περισσότερες πληροφορίες σχετικά με τις συναρτήσεις βοηθητική εφαρμογή του, ανατρέξτε στο θέμα [Ανάπτυξη δέσμης ενεργειών δέσμες ενεργειών για HDInsight][hdinsight-write-script].

Το αποτέλεσμα και τα αρχεία καταγραφής σφαλμάτων για τη δέσμη ενεργειών βρίσκονται αποθηκευμένοι στο τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης που έχετε καθορίσει για το σύμπλεγμα. Τα αρχεία καταγραφής είναι αποθηκευμένα σε έναν πίνακα με το όνομα **u < \cluster-name-fragment >< \time-stamp > setuplog**. Αυτά είναι συγκεντρωτικών αποτελεσμάτων αρχεία καταγραφής από τη δέσμη ενεργειών που εκτελούνται σε όλους τους κόμβους (κεφαλής κόμβο και κόμβους εργαζόμενου) στο σύμπλεγμα.

Κάθε σύμπλεγμα μπορεί να αποδεχτεί πολλές ενέργειες δέσμης ενεργειών που καλούνται με τη σειρά με την οποία καθορίζονται. Μια δέσμη ενεργειών μπορεί να είναι εκτελέσατε σε τον κόμβο κεφαλής, οι κόμβοι εργασίας ή και τα δύο.

HDInsight παρέχει διάφορες δέσμες ενεργειών για να εγκαταστήσετε τα ακόλουθα στοιχεία σε HDInsight συμπλεγμάτων:

Όνομα | Δέσμη ενεργειών
----- | -----
**Εγκατάσταση τους** | https://hdiconfigactions.blob.Core.Windows.NET/sparkconfigactionv03/spark-Installer-v03.ps1. Ανατρέξτε στο θέμα [εγκατάσταση και χρήση αυξήσετε στην συμπλεγμάτων HDInsight][hdinsight-install-spark].
**Εγκατάσταση R** | https://hdiconfigactions.blob.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Ανατρέξτε στο θέμα [εγκατάσταση και χρήση R σε συμπλεγμάτων HDInsight][hdinsight-install-r].
**Εγκατάσταση Solr** | https://hdiconfigactions.blob.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Ανατρέξτε στο θέμα [εγκατάσταση και χρήση συμπλεγμάτων Solr σε HDInsight](hdinsight-hadoop-solr-install.md).
- **Εγκατάσταση Giraph** | https://hdiconfigactions.blob.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Ανατρέξτε στο θέμα [εγκατάσταση και χρήση συμπλεγμάτων Giraph σε HDInsight](hdinsight-hadoop-giraph-install.md).
| **Προ-φόρτωση ομάδας βιβλιοθηκών** | https://hdiconfigactions.blob.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Ανατρέξτε στο θέμα [Προσθήκη ομάδας βιβλιοθηκών σε HDInsight συμπλεγμάτων](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Οι δέσμες ενεργειών κλήσης με την πύλη Azure

**Από την πύλη του Azure**

1. Ξεκινήστε τη δημιουργία ένα σύμπλεγμα, όπως περιγράφεται στην [συμπλεγμάτων δημιουργία Hadoop στο HDInsight](hdinsight-provision-clusters.md#portal).
2. Στην περιοχή προαιρετικές ρυθμίσεις για το blade **Ενέργειες δέσμης ενεργειών** , κάντε κλικ στην επιλογή **Προσθήκη ενέργειας δέσμη ενεργειών** για την παροχή λεπτομερειών σχετικά με την ενέργεια δέσμη ενεργειών, όπως φαίνεται παρακάτω:

    ![Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα")

    <table border='1'>
        <tr><th>Ιδιότητα</th><th>Τιμή</th></tr>
        <tr><td>Όνομα</td>
            <td>Καθορίστε ένα όνομα για την ενέργεια δέσμης ενεργειών.</td></tr>
        <tr><td>Δέσμη ενεργειών URI</td>
            <td>Καθορίστε το URI στη δέσμη ενεργειών που καλείται για να προσαρμόσετε το σύμπλεγμα. s</td></tr>
        <tr><td>Προϊστάμενος/εργαζόμενου</td>
            <td>Καθορίστε τους κόμβους (**κεφάλι** ή **εργαζόμενου**) στην οποία εκτελείται η δέσμη ενεργειών προσαρμογής. </b>.
        <tr><td>Παράμετροι</td>
            <td>Καθορίστε τις παραμέτρους, εάν απαιτείται από τη δέσμη ενεργειών.</td></tr>
    </table>

    Πατήστε το πλήκτρο ENTER για να προσθέσετε περισσότερες από μία ενέργεια δέσμη ενεργειών για την εγκατάσταση πολλών στοιχείων στο σύμπλεγμα.

3. Κάντε κλικ στην **επιλογή** για να αποθηκεύσετε τη ρύθμιση παραμέτρων δέσμης ενεργειών και να συνεχίσετε με τη δημιουργία συμπλέγματος.

## <a name="call-scripts-using-azure-powershell"></a>Κλήση δέσμης ενεργειών με χρήση του PowerShell Azure

Αυτή η παρακάτω δέσμη ενεργειών PowerShell παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους σε σύμπλεγμα HDInsight που βασίζονται σε Windows.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Για να εγκαταστήσετε άλλο λογισμικό, θα πρέπει να αντικαταστήσετε το αρχείο δέσμης ενεργειών στη δέσμη ενεργειών:


Όταν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια για το σύμπλεγμα. Μπορεί να χρειαστούν αρκετά λεπτά πριν από τη δημιουργία του συμπλέγματος.

## <a name="call-scripts-using-net-sdk"></a>Κλήση δέσμης ενεργειών με χρήση .NET SDK 

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους σε σύμπλεγμα HDInsight που βασίζονται σε Windows. Για να εγκαταστήσετε άλλο λογισμικό, θα πρέπει να αντικαταστήσετε το αρχείο δέσμης ενεργειών στον κώδικα.

**Για να δημιουργήσετε ένα σύμπλεγμα HDInsight με τους** 

1. Δημιουργία μιας εφαρμογής κονσόλας C# στο Visual Studio.
2. Από την Κονσόλα διαχείρισης Nuget πακέτου, εκτελέστε την ακόλουθη εντολή.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Χρησιμοποιήστε τα ακόλουθα χρήση προτάσεων στο αρχείο Program.cs:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Τοποθετήστε τον κώδικα στην τάξη με τα εξής:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Υποστήριξη για λογισμικό ανοιχτού κώδικα που χρησιμοποιούνται σε HDInsight συμπλεγμάτων
Η υπηρεσία Microsoft Azure HDInsight είναι μια ευέλικτη πλατφόρμα που σας επιτρέπει να δημιουργήσετε εφαρμογές μεγάλο δεδομένων στο cloud χρησιμοποιώντας ένα περιβάλλον εμπορικής προσαρμογής των τεχνολογιών ανοιχτού κώδικα μορφοποιημένη γύρω από Hadoop. Microsoft Azure παρέχει ένα γενικό επίπεδο υποστήριξης για άνοιγμα προέλευσης τεχνολογίες, όπως περιγράφεται στην ενότητα **Υποστηρίζει την εμβέλεια** της <a href="http://azure.microsoft.com/support/faq/" target="_blank">τοποθεσίας Web συνήθεις ερωτήσεις για υποστήριξη Azure</a>. Η υπηρεσία HDInsight παρέχει ένα πρόσθετο επίπεδο υποστήριξης για ορισμένα από τα στοιχεία, όπως περιγράφεται παρακάτω.

Υπάρχουν δύο τύποι ανοιχτού κώδικα τα στοιχεία που είναι διαθέσιμες στην υπηρεσία HDInsight:

- **Ενσωματωμένα στοιχεία** - αυτά τα στοιχεία είναι προεγκατεστημένα στα HDInsight συμπλεγμάτων και παρέχουν βασικές λειτουργίες του συμπλέγματος. Για παράδειγμα, ΝΉΜΑΤΑ ResourceManager, τη γλώσσα ερωτήματος Hive (HiveQL) και τη βιβλιοθήκη Mahout ανήκει σε αυτή την κατηγορία. Μια πλήρη λίστα των στοιχείων σύμπλεγμα είναι διαθέσιμη στο [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα Hadoop που παρέχεται από το HDInsight;](hdinsight-component-versioning.md) </a>.
- **Προσαρμοσμένα στοιχεία** - εσείς, ως χρήστης του συμπλέγματος, να εγκαταστήσετε ή χρησιμοποιήστε στο του φόρτου εργασίας οποιοδήποτε στοιχείο διαθέσιμο στην Κοινότητα ή που έχουν δημιουργηθεί από εσάς.

Υποστηρίζονται πλήρως, ενσωματωμένα στοιχεία και υποστήριξη της Microsoft θα σας βοηθήσει να απομονώσετε και να επιλύσετε θέματα που σχετίζονται με αυτά τα στοιχεία.

> [AZURE.WARNING] Στοιχεία που παρέχονται με το σύμπλεγμα HDInsight υποστηρίζονται πλήρως και υποστήριξη της Microsoft θα σας βοηθήσει να απομονώσετε και να επιλύσετε θέματα που σχετίζονται με αυτά τα στοιχεία.
>
> Προσαρμοσμένα στοιχεία λαμβάνουν εμπορικά λογικές υποστήριξη για να σας βοηθήσει να αντιμετωπίσετε περαιτέρω το ζήτημα. Αυτό μπορεί να έχει ως αποτέλεσμα το ζήτημα επίλυση ή που σας ζητά να συμμετάσχουν διαθέσιμα κανάλια για τις τεχνολογίες Άνοιγμα αρχείου προέλευσης όπου βρίσκονται πολλά επίπεδα εξειδίκευση για συγκεκριμένη τεχνολογία. Για παράδειγμα, υπάρχουν πολλές τοποθεσίες Κοινότητας που μπορούν να χρησιμοποιηθούν, όπως: [φόρουμ MSDN για το HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Επίσης, Apache έργα έχουν τοποθεσίες έργου στο [http://apache.org](http://apache.org), για παράδειγμα: [Hadoop](http://hadoop.apache.org/), [τους](http://spark.apache.org/).

Η υπηρεσία HDInsight παρέχει διάφορους τρόπους για να χρησιμοποιήσετε προσαρμοσμένα στοιχεία. Ανεξάρτητα από το πώς ένα στοιχείο χρησιμοποιείται ή εγκατεστημένο στο σύμπλεγμα, εφαρμογή στο ίδιο επίπεδο υποστήριξης. Ακολουθεί μια λίστα με τους πιο συνηθισμένους τρόπους που τα προσαρμοσμένα στοιχεία που μπορεί να χρησιμοποιηθεί σε συμπλεγμάτων HDInsight:

1. Εργασία υποβολής - Hadoop ή άλλους τύπους εργασιών που εκτελεί ή να χρησιμοποιήσετε προσαρμοσμένα στοιχεία μπορούν να υποβληθούν στο σύμπλεγμα.
2. Σύμπλεγμα προσαρμογής - κατά τη δημιουργία συμπλέγματος, μπορείτε να ορίσετε πρόσθετες ρυθμίσεις και προσαρμοσμένα στοιχεία που θα εγκατασταθεί σε τους κόμβους συμπλέγματος.
3. Δείγματα - για δημοφιλείς προσαρμοσμένα στοιχεία, Microsoft και άλλους μπορεί να σας παρέχει δείγματα πώς αυτά τα στοιχεία μπορούν να χρησιμοποιηθούν σε των συμπλεγμάτων HDInsight. Αυτά τα δείγματα παρέχονται χωρίς την υποστήριξη.

## <a name="develop-script-action-scripts"></a>Ανάπτυξη δεσμών ενεργειών δέσμης ενεργειών

Ανατρέξτε στο θέμα [Ανάπτυξη δέσμης ενεργειών δέσμες ενεργειών για HDInsight][hdinsight-write-script].


## <a name="see-also"></a>Δείτε επίσης

- [Δημιουργία συμπλεγμάτων Hadoop στο HDInsight] [ hdinsight-provision-cluster] παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα σύμπλεγμα HDInsight χρησιμοποιώντας άλλες προσαρμοσμένες επιλογές.
- [Ανάπτυξη δεσμών ενεργειών δέσμης ενεργειών για HDInsight][hdinsight-write-script]
- [Εγκατάσταση και χρήση τους σε HDInsight συμπλεγμάτων][hdinsight-install-spark]
- [Εγκατάσταση και χρήση R στον HDInsight συμπλεγμάτων][hdinsight-install-r]
- [Εγκατάσταση και χρήση συμπλεγμάτων Solr σε HDInsight](hdinsight-hadoop-solr-install.md).
- [Εγκατάσταση και χρήση συμπλεγμάτων Giraph σε HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Στάδια κατά τη δημιουργία συμπλέγματος"

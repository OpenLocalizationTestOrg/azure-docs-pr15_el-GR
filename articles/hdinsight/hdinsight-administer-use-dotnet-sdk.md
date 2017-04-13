<properties
    pageTitle="Διαχείριση συμπλεγμάτων Hadoop στο HDInsight με το .NET SDK | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσετε εργασίες διαχείρισης για το συμπλεγμάτων Hadoop στο HDInsight χρησιμοποιώντας HDInsight .NET SDK."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    tags="azure-portal"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-by-using-net-sdk"></a>Διαχείριση συμπλεγμάτων Hadoop στο HDInsight με τη χρήση .NET SDK

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Μάθετε πώς μπορείτε να διαχειριστείτε χρησιμοποιώντας [HDInsight.NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)συμπλεγμάτων HDInsight.


**Προαπαιτούμενα στοιχεία**

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).


##<a name="connect-to-azure-hdinsight"></a>Σύνδεση με το Azure HDInsight

Θα χρειαστείτε τα εξής πακέτα Nuget:

    Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
    Install-Package Microsoft.Azure.Management.ResourceManager -Pre
    Install-Package Microsoft.Azure.Management.HDInsight

Το παρακάτω δείγμα κώδικα δείχνει πώς μπορείτε να συνδεθείτε με Azure πριν μπορείτε να διαχειριστείτε συμπλεγμάτων HDInsight στην περιοχή Azure τη συνδρομή σας.

    using System;
    using Microsoft.Azure;
    using Microsoft.Azure.Management.HDInsight;
    using Microsoft.Azure.Management.HDInsight.Models;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    using Microsoft.Rest.Azure.Authentication;

    namespace HDInsightManagement
    {
        class Program
        {
            private static HDInsightManagementClient _hdiManagementClient;
            // Replace with your AAD tenant ID if necessary
            private const string TenantId = UserTokenProvider.CommonTenantId; 
            private const string SubscriptionId = "<Your Azure Subscription ID>";
            // This is the GUID for the PowerShell client. Used for interactive logins in this example.
            private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";

            static void Main(string[] args)
            {
                // Authenticate and get a token
                var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
                // Flag subscription for HDInsight, if it isn't already.
                EnableHDInsight(authToken);
                // Get an HDInsight management client
                _hdiManagementClient = new HDInsightManagementClient(authToken);

                // insert code here

                System.Console.WriteLine("Press ENTER to continue");
                System.Console.ReadLine();
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
        }
    }

Θα μπορείτε να δείτε ένα μήνυμα προτροπής κατά την εκτέλεση αυτού του προγράμματος.  Εάν δεν θέλετε να εμφανιστεί η γραμμή εντολών, ανατρέξτε στο θέμα [Δημιουργία μη αλληλεπιδραστική ελέγχου ταυτότητας .NET HDInsight εφαρμογές](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

##<a name="create-clusters"></a>Δημιουργία συμπλεγμάτων

Ανατρέξτε στο θέμα [Δημιουργία Linux βάσει συμπλεγμάτων στο HDInsight με το .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)

##<a name="list-clusters"></a>Λίστα συμπλεγμάτων

Το τμήμα κώδικα παρακάτω λίστες συμπλεγμάτων και ορισμένες ιδιότητες:

    var results = _hdiManagementClient.Clusters.List();
    foreach (var name in results.Clusters) {
        Console.WriteLine("Cluster Name: " + name.Name);
        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
        Console.WriteLine("\t Cluster location: " + name.Location);
        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
    }

##<a name="delete-clusters"></a>Διαγραφή συμπλεγμάτων

Χρησιμοποιήστε το τμήμα κώδικα παρακάτω για να διαγράψετε ένα σύμπλεγμα σύγχρονη ή ασύγχρονη: 

    _hdiManagementClient.Clusters.Delete("<Resource Group Name>", "<Cluster Name>");
    _hdiManagementClient.Clusters.DeleteAsync("<Resource Group Name>", "<Cluster Name>");
            
##<a name="scale-clusters"></a>Κλίμακα συμπλεγμάτων
Το σύμπλεγμα κλίμακας δυνατότητα σάς επιτρέπει να αλλάξετε τον αριθμό των κόμβους εργαζόμενου που χρησιμοποιούνται από ένα σύμπλεγμα που εκτελείται στο Azure HDInsight χωρίς να χρειάζεται να δημιουργήσετε ξανά το σύμπλεγμα.

>[AZURE.NOTE] Μόνο συμπλεγμάτων με HDInsight έκδοση 3.1.3 ή υψηλότερη υποστηρίζονται. Εάν είστε βέβαιοι για την έκδοση του το σύμπλεγμά σας, μπορείτε να ελέγξετε τη σελίδα ιδιοτήτων.  Ανατρέξτε στο θέμα [λίστα και εμφάνιση συμπλεγμάτων](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Την επίδραση των αλλαγή του αριθμού των κόμβοι δεδομένων για κάθε τύπο σύμπλεγμα που υποστηρίζονται από το HDInsight:

- Hadoop

    Απρόσκοπτη, μπορείτε να αυξήσετε τον αριθμό των κόμβους εργαζόμενου σε ένα σύμπλεγμα Hadoop που εκτελείται χωρίς να επηρεάζονται τυχόν εργασίες σε εκκρεμότητα ή δεν εκτελείται. Νέες εργασίες μπορούν να υποβληθούν επίσης ενώ η λειτουργία βρίσκεται σε εξέλιξη. Αποτυχίες σε μια λειτουργία κλίμακας αντιμετωπίζονται ομαλά, έτσι ώστε το σύμπλεγμα πάντα θα παραμείνει σε μια λειτουργική κατάσταση.

    Όταν ένα σύμπλεγμα Hadoop γίνεται κλιμάκωση προς τα κάτω, μειώνοντας τον αριθμό των δεδομένων κόμβοι, ορισμένες από τις υπηρεσίες του συμπλέγματος επανεκκίνηση του. Αυτό θα κάνει όλα εκτελείται και εκκρεμείς εργασίες αποτυχία κατά την ολοκλήρωση της λειτουργίας κλίμακας. Ωστόσο, μπορείτε να, υποβάλετε ξανά τις εργασίες μόλις ολοκληρωθεί η λειτουργία.

- HBase

    Απρόσκοπτη, μπορείτε να προσθέσετε ή να καταργήσετε κόμβοι για το σύμπλεγμα HBase ενώ εκτελείται. Οι τοπικοί διακομιστές είναι εξισορρόπηση αυτόματα μετά από μερικά λεπτά από την ολοκλήρωση της λειτουργίας κλίμακας. Ωστόσο, μπορείτε να εξισορροπήσετε το οι τοπικοί διακομιστές επίσης με μη αυτόματο τρόπο, σύνδεση με το headnode συμπλέγματος και εκτελεί τις παρακάτω εντολές από ένα παράθυρο γραμμής εντολών:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

- Καταιγίδας

    Απρόσκοπτη, μπορείτε να προσθέσετε ή να καταργήσετε κόμβους δεδομένων για το σύμπλεγμά σας καταιγίδας ενώ εκτελείται. Αλλά μετά την επιτυχή ολοκλήρωση της λειτουργίας κλίμακας, θα χρειαστεί να νέα εξισορρόπηση της τοπολογίας.

    Εξισορρόπηση μπορεί να πραγματοποιηθεί με δύο τρόπους:

    * Καταιγίδας web περιβάλλοντος εργασίας Χρήστη
    * Εργαλείο περιβάλλον γραμμής εντολών (CLI)

    Ανατρέξτε στην [τεκμηρίωση καταιγίδας Apache](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) για περισσότερες λεπτομέρειες.

    Το web καταιγίδας περιβάλλοντος εργασίας Χρήστη είναι διαθέσιμο στο σύμπλεγμα HDInsight:

    ![νέα εξισορρόπηση κλίμακα καταιγίδας hdinsight](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    Ακολουθεί ένα παράδειγμα πώς μπορείτε να χρησιμοποιήσετε την εντολή CLI για νέα εξισορρόπηση της τοπολογίας καταιγίδας:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Το παρακάτω τμήμα κώδικα δείχνει πώς μπορείτε να αλλάξετε το μέγεθος ενός συμπλέγματος σύγχρονη ή ασύγχρονη:

    _hdiManagementClient.Clusters.Resize("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    _hdiManagementClient.Clusters.ResizeAsync("<Resource Group Name>", "<Cluster Name>", <New Size>);   
    

##<a name="grantrevoke-access"></a>Εκχώρηση/ανάκληση πρόσβασης

HDInsight συμπλεγμάτων έχουν τις ακόλουθες υπηρεσίες web HTTP (όλες αυτές οι υπηρεσίες έχουν RESTful τελικά σημεία):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Από προεπιλογή, οι υπηρεσίες αυτές έχουν εκχωρηθεί για πρόσβαση. Μπορείτε να revoke/εκχώρηση πρόσβαση. Για να ανακαλέσετε:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = false,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);

Για να εκχωρήσετε:

    var httpParams = new HttpSettingsParameters
    {
        HttpUserEnabled = enable,
        HttpUsername = "admin",
        HttpPassword = "*******",
    };
    _hdiManagementClient.Clusters.ConfigureHttpSettings("<Resource Group Name>, <Cluster Name>, httpParams);


>[AZURE.NOTE] Με την εκχώρηση/ανάκληση της access, θα μπορείτε να επαναφέρετε το σύμπλεγμα όνομα χρήστη και κωδικό πρόσβασης.

Αυτό μπορεί να γίνει επίσης μέσω της πύλης. Ανατρέξτε στο θέμα [Διαχείριση του HDInsight, χρησιμοποιώντας την πύλη Azure][hdinsight-admin-portal].

##<a name="update-http-user-credentials"></a>Ενημέρωση διαπιστευτηρίων χρήστη HTTP

Πρόκειται για την ίδια διαδικασία που [Εκχώρηση/ανάκληση HTTP πρόσβασης](#grant/revoke-access). Εάν το σύμπλεγμα έχει παραχωρηθεί πρόσβαση HTTP, μπορείτε πρώτα πρέπει να ανακαλέσετε την.  Και, στη συνέχεια, να εκχωρήσετε το πρόσβαση με διαπιστευτήρια χρήστη νέου HTTP.


##<a name="find-the-default-storage-account"></a>Βρείτε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης

Το παρακάτω τμήμα κώδικα παρουσιάζει πώς μπορείτε να λάβετε το προεπιλεγμένο όνομα λογαριασμού χώρου αποθήκευσης και το κλειδί λογαριασμού αποθήκευσης προεπιλογή για ένα σύμπλεγμα.

    var results = _hdiManagementClient.Clusters.GetClusterConfigurations(<Resource Group Name>, <Cluster Name>, "core-site");
    foreach (var key in results.Configuration.Keys)
    {
        Console.WriteLine(String.Format("{0} => {1}", key, results.Configuration[key]));
    }


##<a name="submit-jobs"></a>Υποβολή εργασιών

**Για να υποβάλετε MapReduce εργασίες**

Ανατρέξτε στο θέμα [Εκτέλεση Hadoop MapReduce δείγματα στο HDInsight](hdinsight-hadoop-run-samples-linux.md).

**Για να υποβάλετε εργασίες ομάδας** 

Ανατρέξτε στο θέμα [Εκτέλεση Hive ερωτημάτων με το .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).

**Για να υποβάλετε γουρούνι εργασίες**

Ανατρέξτε στο θέμα [Εκτέλεση γουρούνι εργασίες χρησιμοποιώντας .NET SDK](hdinsight-hadoop-use-pig-dotnet-sdk.md).

**Για να υποβάλετε Sqoop εργασίες**

Ανατρέξτε στο θέμα [Χρήση Sqoop με HDInsight](hdinsight-hadoop-use-sqoop-dotnet-sdk.md).

**Για να υποβάλετε Oozie εργασίες**

Ανατρέξτε στο θέμα [Χρήση Oozie με Hadoop να ορίσετε και να εκτελέσετε μια ροή εργασιών στο HDInsight](hdinsight-use-oozie-linux-mac.md).

##<a name="upload-data-to-azure-blob-storage"></a>Αποστολή δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob του Azure
Ανατρέξτε στο θέμα [Αποστολή δεδομένων με το HDInsight][hdinsight-upload-data].


## <a name="see-also"></a>Δείτε επίσης
* [Τεκμηρίωση αναφοράς HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)
* [Διαχείριση HDInsight, χρησιμοποιώντας την πύλη του Azure][hdinsight-admin-portal]
* [Διαχείριση HDInsight χρησιμοποιώντας ένα περιβάλλον γραμμής εντολών][hdinsight-admin-cli]
* [Δημιουργία HDInsight συμπλεγμάτων][hdinsight-provision]
* [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
* [Γρήγορα αποτελέσματα με το Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-portal-linux.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md



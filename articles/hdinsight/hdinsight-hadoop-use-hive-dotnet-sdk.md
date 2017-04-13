<properties
    pageTitle="Εκτέλεση ερωτημάτων Hive χρησιμοποιώντας HDInsight .NET SDK | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να υποβάλετε Hadoop εργασίες να Azure HDInsight Hadoop χρησιμοποιώντας HDInsight .NET SDK."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Εκτέλεση ερωτημάτων ομάδα με το HDInsight .NET SDK

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Μάθετε πώς μπορείτε να υποβάλετε Hive ερωτημάτων με το HDInsight .NET SDK.

> [AZURE.NOTE] Τα βήματα σε αυτό το άρθρο πρέπει να εκτελεστούν από ένα πρόγραμμα-πελάτη των Windows. Για πληροφορίες σχετικά με τη χρήση μιας Linux, OS X ή Unix προγράμματος-πελάτη για να εργαστείτε με ομάδα, χρησιμοποιήστε τον επιλογέα καρτέλα εμφανίζεται στο επάνω μέρος του άρθρου.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα A Hadoop στο HDInsight**. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τη χρήση βάσει Linux Hadoop στο HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Υποβολή ερωτημάτων ομάδα με το HDInsight .NET SDK

Το HDInsight .NET SDK παρέχει βιβλιοθήκες προγράμματος-πελάτη .NET, που διευκολύνει την εργασία με HDInsight συμπλεγμάτων από .NET. 

**Για να υποβάλετε εργασίες**

1. Δημιουργία μιας εφαρμογής κονσόλας C# στο Visual Studio.
2. Από την Κονσόλα διαχείρισης Nuget πακέτου, εκτελέστε την ακόλουθη εντολή.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Χρησιμοποιήστε τον ακόλουθο κώδικα:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.


## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο μάθατε διάφορους τρόπους για να δημιουργήσετε ένα σύμπλεγμα HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

* [Γρήγορα αποτελέσματα με το Azure HDInsight][hdinsight-get-started]
* [Δημιουργία συμπλεγμάτων Hadoop στο HDInsight][hdinsight-provision]
* [Διαχείριση συμπλεγμάτων Hadoop στο HDInsight, χρησιμοποιώντας την πύλη του Azure](hdinsight-administer-use-management-portal.md)
* [Αναφορά HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση Sqoop με HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Δημιουργία μη αλληλεπιδραστική ελέγχου ταυτότητας εφαρμογών .NET HDInsight](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md



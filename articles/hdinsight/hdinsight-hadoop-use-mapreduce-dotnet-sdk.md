<properties
    pageTitle="Υποβολή MapReduce εργασιών χρησιμοποιώντας HDInsight .NET SDK | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να υποβάλετε MapReduce εργασίες να Azure HDInsight Hadoop χρησιμοποιώντας HDInsight .NET SDK."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Εκτέλεση MapReduce εργασίες χρησιμοποιώντας HDInsight .NET SDK

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Μάθετε πώς να υποβάλλουν MapReduce εργασίες χρησιμοποιώντας HDInsight .NET SDK. HDInsight συμπλεγμάτων παρέχονται με ένα αρχείο βάζο με ορισμένα δείγματα MapReduce. Το αρχείο βάζο είναι */example/jars/hadoop-mapreduce-examples.jar*.  Ένα από τα δείγματα είναι *wordcount*. Μπορείτε να αναπτύξετε μια εφαρμογή κονσόλας C# για να υποβάλετε μια εργασία wordcount.  Η εργασία διαβάζει το αρχείο */example/data/gutenberg/davinci.txt* και εμφανίζει τα αποτελέσματα */example/data/davinciwordcount*.  Εάν θέλετε να εκτελέσετε ξανά την εφαρμογή, πρέπει να εκκαθαρίσετε το φάκελο εξόδου.

> [AZURE.NOTE] Τα βήματα σε αυτό το άρθρο πρέπει να εκτελεστούν από ένα πρόγραμμα-πελάτη των Windows. Για πληροφορίες σχετικά με τη χρήση μιας Linux, OS X ή Unix προγράμματος-πελάτη για να εργαστείτε με ομάδα, χρησιμοποιήστε τον επιλογέα καρτέλα εμφανίζεται στο επάνω μέρος του άρθρου.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα A Hadoop στο HDInsight**. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τη χρήση βάσει Linux Hadoop στο HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Υποβολή MapReduce εργασίες χρησιμοποιώντας HDInsight .NET SDK

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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- Για τη δημιουργία ενός συμπλέγματος και υποβολή μιας ομάδας εργασίας, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- Για τη δημιουργία συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Δημιουργία Linux βάσει Hadoop συμπλεγμάτων στο HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
- Για τη Διαχείριση συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο HDInsight](hdinsight-administer-use-management-portal.md).
- Για να μάθετε το .NET SDK HDInsight, ανατρέξτε στο θέμα [αναφορά HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).
- Για μη αλληλεπιδραστική ελέγχουν την ταυτότητα Azure, ανατρέξτε στο θέμα [Δημιουργία εφαρμογών .NET HDInsight μη αλληλεπιδραστική ελέγχου ταυτότητας](hdinsight-create-non-interactive-authentication-dotnet-applications.md).





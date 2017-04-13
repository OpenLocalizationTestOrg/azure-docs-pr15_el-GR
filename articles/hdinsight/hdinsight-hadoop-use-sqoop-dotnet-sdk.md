<properties
    pageTitle="Χρήση Hadoop Sqoop σε HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε HDInsight .NET SDK για την εκτέλεση Sqoop εισαγωγή και εξαγωγή ανάμεσα σε ένα σύμπλεγμα Hadoop και μια βάση δεδομένων Azure SQL."
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

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Εκτέλεση Sqoop εργασιών με χρήση του .NET SDK για Hadoop σε HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Μάθετε πώς να χρησιμοποιείτε το HDInsight .NET SDK για να εκτελέσετε εργασίες Sqoop σε HDInsight εισαγωγής και εξαγωγής μεταξύ συμπλεγμάτων HDInsight και βάση δεδομένων Azure SQL ή βάση δεδομένων SQL Server.

> [AZURE.NOTE] Τα βήματα σε αυτό το άρθρο μπορούν να χρησιμοποιηθούν με είτε ένα βασίζεται σε Windows ή Linux HDInsight σύμπλεγμα; Ωστόσο, αυτά τα βήματα θα λειτουργεί μόνο από ένα πρόγραμμα-πελάτη των Windows. Χρησιμοποιήστε στον επιλογέα στηλοθετών στο επάνω μέρος αυτού του άρθρου, για να επιλέξετε άλλες μεθόδους.

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα A Hadoop στο HDInsight**. Ανατρέξτε στο θέμα [Δημιουργία σύμπλεγμα και βάση δεδομένων SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Εκτέλεση Sqoop χρησιμοποιώντας .NET SDK

Το HDInsight .NET SDK παρέχει βιβλιοθήκες προγράμματος-πελάτη .NET, που διευκολύνει την εργασία με HDInsight συμπλεγμάτων από .NET. Σε αυτήν την ενότητα, θα δημιουργήσετε μια εφαρμογή κονσόλας C# για να εξαγάγετε τα hivesampletable στον πίνακα βάσης δεδομένων SQL που δημιουργήσατε νωρίτερα σε αυτό προγράμματα εκμάθησης.

**Για να υποβάλετε μια εργασία Sqoop**

1. Δημιουργία μιας εφαρμογής κονσόλας C# στο Visual Studio.
2. Από την Κονσόλα διαχείρισης του Visual Studio πακέτου, εκτελέστε την ακόλουθη εντολή Nuget για να εισαγάγετε το πακέτο.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. Χρησιμοποιήστε τον ακόλουθο κώδικα στο αρχείο Program.cs:

        using System.Collections.Generic;
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
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Πατήστε το πλήκτρο **F5** για να εκτελέσετε το πρόγραμμα. 

##<a name="limitations"></a>Περιορισμοί

* Μαζική export - βάσει με Linux HDInsight, Sqoop σύνδεσης που χρησιμοποιείται για την εξαγωγή δεδομένων Microsoft SQL Server ή βάση δεδομένων SQL Azure δεν υποστηρίζει τη συγκεκριμένη στιγμή μαζικής εισάγεται.

* Δέσμης - με βάσει Linux HDInsight, όταν χρησιμοποιείτε το `-batch` εναλλαγή κατά την εκτέλεση εισάγει, Sqoop θα εκτελέσει πολλών εισάγει αντί για δέσμης για τις λειτουργίες εισαγωγή.

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μάθει πώς μπορείτε να χρησιμοποιήσετε Sqoop. Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Χρήση Oozie με HDInsight](hdinsight-use-oozie.md): χρήση Sqoop ενεργειών σε μια ροή εργασίας Oozie.
- [Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση HDInsight](hdinsight-analyze-flight-delay-data.md): Χρησιμοποιήστε την ομάδα για την ανάλυση πτήσεων καθυστέρηση δεδομένων και κατόπιν χρησιμοποιήστε Sqoop να εξαγάγετε τα δεδομένα σε μια βάση δεδομένων Azure SQL.
- [Αποστολή δεδομένων με το HDInsight](hdinsight-upload-data.md): βρείτε άλλες μεθόδους για την αποστολή δεδομένων με το χώρο αποθήκευσης αντικειμένων Blob HDInsight/Azure.



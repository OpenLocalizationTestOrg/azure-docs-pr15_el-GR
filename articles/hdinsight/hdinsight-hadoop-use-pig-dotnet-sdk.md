<properties
   pageTitle="Χρήση Hadoop γουρούνι με το .NET στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το .NET SDK για Hadoop για την υποβολή γουρούνι εργασιών για να Hadoop σε HDInsight."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Εκτέλεση γουρούνι εργασίες χρησιμοποιώντας το .NET SDK για Hadoop σε HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Αυτό το έγγραφο παρέχει ένα παράδειγμα της χρήσης το .NET SDK για Hadoop για την υποβολή γουρούνι εργασιών για να Hadoop σε σύμπλεγμα HDInsight.

Το HDInsight .NET SDK παρέχει βιβλιοθήκες προγράμματος-πελάτη .NET που διευκολύνει την εργασία με HDInsight συμπλεγμάτων από .NET. Γουρούνι σάς επιτρέπει να δημιουργήσετε λειτουργίες MapReduce με μοντελοποίηση μιας σειράς δεδομένων μετασχηματισμοί. Θα μάθετε πώς μπορείτε να χρησιμοποιήσετε μια βασική εφαρμογή C# για να υποβάλετε μια εργασία γουρούνι σε ένα σύμπλεγμα HDInsight.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής.

* Ένα σύμπλεγμα Azure HDInsight (Hadoop σε HDInsight) (είτε Windows ή Linux βάσει).
* Visual Studio 2012 ή 2013 ή 2015.

## <a name="create-the-application"></a>Δημιουργία της εφαρμογής

Το HDInsight .NET SDK παρέχει βιβλιοθήκες προγράμματος-πελάτη .NET, που διευκολύνει την εργασία με HDInsight συμπλεγμάτων από .NET. 


1. Ανοίξτε το Visual Studio 2012 ή 2013
2. Από το μενού **αρχείο** , επιλέξτε **Δημιουργία** και, στη συνέχεια, επιλέξτε το **στοιχείο έργο**.
3. Για το νέο έργο, πληκτρολογήστε ή επιλέξτε τις παρακάτω τιμές.

    <table>
    <tr>
    <th>Ιδιότητα</th>
    <th>Τιμή</th>
    </tr>
    <tr>
    <th>Κατηγορία</th>
    <th>Πρότυπα/Visual C# / Windows</th>
    </tr>
    <tr>
    <th>Πρότυπο</th>
    <th>Εφαρμογή κονσόλας</th>
    </tr>
    <tr>
    <th>Όνομα</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε το έργο.
5. Από το μενού **Εργαλεία** , επιλέξτε **Διαχείριση πακέτου βιβλιοθήκης** ή **Διαχείριση πακέτου Nuget**και, στη συνέχεια, επιλέξτε **Κονσόλα διαχείρισης πακέτου**.
6. Εκτελέστε την ακόλουθη εντολή στην κονσόλα για να εγκαταστήσετε τα πακέτα .NET SDK.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Από την Εξερεύνηση λύσεων, κάντε διπλό κλικ **Program.cs** για να το ανοίξετε. Αντικαταστήσετε τον υπάρχοντα κωδικό με τα εξής.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Πατήστε το πλήκτρο **F5** για να ξεκινήσετε την εφαρμογή.
8. Πατήστε το πλήκτρο **ENTER** για να τερματίσετε την εφαρμογή.

## <a name="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε το .NET SDK για Hadoop σάς επιτρέπει να δημιουργήσετε εφαρμογές .NET που υποβολή γουρούνι εργασιών σε ένα σύμπλεγμα HDInsight, και να παρακολουθείτε την κατάσταση της εργασίας.

## <a name="next-steps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με γουρούνι στο HDInsight.

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight.

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md) [προεπισκόπηση-πύλη]: https://portal.azure.com/

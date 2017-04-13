<properties
   pageTitle="Χρήση γουρούνι Hadoop σε HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε γουρούνι με Hadoop σε HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

# <a name="use-pig-with-hadoop-on-hdinsight"></a>Χρήση γουρούνι με Hadoop σε HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Γουρούνι Apache](http://pig.apache.org/) είναι μια πλατφόρμα για τη δημιουργία προγράμματα για Hadoop, χρησιμοποιώντας μια γλώσσα σχετικά με τη διαδικασία γνωστό ως *Λατινικά γουρούνι*. Γουρούνι είναι μια εναλλακτική λύση για Java για τη δημιουργία *MapReduce* λύσεις και περιλαμβάνεται με το Azure HDInsight.

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε γουρούνι με HDInsight.

##<a id="why"></a>Γιατί να χρησιμοποιήσετε γουρούνι;

Μία από τις προκλήσεις για επεξεργασία δεδομένων με χρήση MapReduce στο Hadoop υλοποιεί σας λογική επεξεργασίας, χρησιμοποιώντας μόνο ένα χάρτη και μια συνάρτηση μείωση. Για σύνθετη επεξεργασία, συχνά πρέπει να διασπάσετε επεξεργασίας σε πολλαπλές λειτουργίες MapReduce που σχηματίζουν μαζί, για να επιτύχετε το επιθυμητό αποτέλεσμα.

Αντί να επιβάλετε μπορείτε να χρησιμοποιήσετε μόνο χάρτη και μείωση συναρτήσεις, γουρούνι σάς επιτρέπει να καθορίζετε επεξεργασίας ως μια σειρά από μετασχηματισμοί που τα δεδομένα ρέει μέσω για να προκύψει το επιθυμητό αποτέλεσμα.

Η γλώσσα Λατινικά γουρούνι σάς επιτρέπει να περιγράφουν τη ροή δεδομένων από ανεπεξέργαστα εισόδου, μέσω ενός ή περισσότερων μετασχηματισμοί, για να προκύψει το επιθυμητό αποτέλεσμα. Τα προγράμματα Λατινικά γουρούνι ακολουθήστε αυτό το μοτίβο γενικά:

- **Φόρτωση**: ανάγνωση δεδομένων για να είναι δυνατός ο χειρισμός από το σύστημα αρχείων
- **Μετασχηματισμός**: χειρισμός δεδομένων
- **Ένδειξη ή store**: εξαγωγή δεδομένων στην οθόνη ή να το αποθηκεύσετε για επεξεργασία

Γουρούνι Λατινικά υποστηρίζει επίσης συναρτήσεις που ορίζονται από το χρήστη (UDF), που σας επιτρέπει να καλέσετε εξωτερικά στοιχεία που υλοποίηση της λογικής που είναι δύσκολο να μοντέλο στο Λατινικά γουρούνι.

Για περισσότερες πληροφορίες σχετικά με την γουρούνι Λατινικά, ανατρέξτε στο θέμα [Λατινικά γουρούνι αναφοράς μη αυτόματος 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) και [2 εγχειρίδιο αναφοράς Λατινικά γουρούνι](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Για ένα παράδειγμα της χρήσης UDF με γουρούνι, δείτε τα ακόλουθα έγγραφα:

* [Χρήση DataFu με γουρούνι στο HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu είναι μια συλλογή από χρήσιμες UDF διατηρούνται από Apache

* [Χρήση Python με γουρούνι και ομάδα στο HDInsight](hdinsight-python.md)

* [Χρήση C# με την ομάδα και γουρούνι στο HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

##<a id="data"></a>Σχετικά με το δείγμα δεδομένων

Σε αυτό το παράδειγμα χρησιμοποιεί ένα δείγμα αρχείου *log4j* , η οποία είναι αποθηκευμένη στο **/example/data/sample.log** σας κοντέινερ χώρου αποθήκευσης αντικειμένων blob. Κάθε αρχείο καταγραφής εντός του αρχείου που αποτελείται από μια γραμμή των πεδίων που περιέχει ένα `[LOG LEVEL]` πεδίο για να εμφανίσετε τον τύπο και της σοβαρότητας, για παράδειγμα:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Στο προηγούμενο παράδειγμα, το επίπεδο καταγραφής είναι σφάλμα.

> [AZURE.NOTE] Μπορείτε επίσης να δημιουργήσετε ένα αρχείο log4j με χρήση του εργαλείου [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) καταγραφής και στείλτε το αρχείο σας blob. Για οδηγίες, ανατρέξτε στο θέμα [Αποστολή δεδομένων με το HDInsight](hdinsight-upload-data.md) . Για περισσότερες πληροφορίες σχετικά με τον τρόπο χρήσης των αντικειμένων blob στο χώρο αποθήκευσης Azure με το HDInsight, ανατρέξτε στο θέμα [Χρήση χώρο αποθήκευσης αντικειμένων Blob Azure με HDInsight](hdinsight-hadoop-use-blob-storage.md).

Το δείγμα δεδομένων είναι αποθηκευμένη στο χώρο αποθήκευσης αντικειμένων Blob του Azure, που χρησιμοποιεί HDInsight ως το προεπιλεγμένο σύστημα αρχείων Hadoop συμπλεγμάτων. HDInsight να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα σε αντικείμενα blob, χρησιμοποιώντας το πρόθεμα **wasb** . Για παράδειγμα, για να αποκτήσετε πρόσβαση στο αρχείο sample.log, πρέπει να χρησιμοποιήσετε την παρακάτω σύνταξη:

    wasbs:///example/data/sample.log

Επειδή WASB είναι το προεπιλεγμένο αποθήκευσης για HDInsight, μπορείτε επίσης να αποκτήσετε πρόσβαση στο αρχείο με τη χρήση **/example/data/sample.log** από Λατινικά γουρούνι.

> [AZURE.NOTE] Η σύνταξη της, **wasbs: / / /**, χρησιμοποιείται για να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα στο προεπιλεγμένο κοντέινερ χώρου αποθήκευσης για το σύμπλεγμα HDInsight. Αν έχετε καθορίσει τους λογαριασμούς επιπλέον χώρου αποθήκευσης όταν παρασχεθεί το σύμπλεγμά σας και θέλετε να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα σε αυτούς τους λογαριασμούς, μπορείτε να αποκτήσετε πρόσβαση στα δεδομένα, καθορίζοντας τη κοντέινερ όνομα και αποθήκευσης λογαριασμού διεύθυνση, για παράδειγμα: **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.


##<a id="job"></a>Σχετικά με την εργασία με δείγμα

Η ακόλουθη εργασία Λατινικά γουρούνι φορτώνει το αρχείο **sample.log** από το χώρο αποθήκευσης προεπιλογή για το σύμπλεγμα HDInsight. Στη συνέχεια, εκτελεί μια σειρά από μετασχηματισμοί που έχει ως αποτέλεσμα μια καταμέτρηση του πόσες φορές κάθε επίπεδο καταγραφής που έγιναν στα δεδομένα εισόδου. Τα αποτελέσματα απορρίφθηκαν σε STDOUT.

    LOGS = LOAD 'wasbs:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

Η παρακάτω εικόνα εμφανίζει μια ανάλυση του τι κάνει κάθε μετασχηματισμό, για τα δεδομένα.

![Γραφική αναπαράσταση των μετασχηματισμών][image-hdi-pig-data-transformation]

##<a id="run"></a>Εκτέλεση της εργασίας γουρούνι Λατινικά

HDInsight να εκτελέσετε εργασίες γουρούνι Λατινικά, χρησιμοποιώντας διάφορες μεθόδους. Χρησιμοποιήστε τον παρακάτω πίνακα για να αποφασίσετε ποια μέθοδο είναι κατάλληλη για εσάς και, στη συνέχεια, ακολουθήστε τη σύνδεση για αναλυτικές οδηγίες.

| **Χρησιμοποιήστε αυτήν την επιλογή** εάν θέλετε...                                   | .. .an **αλληλεπιδραστικών** κελύφους | ... Επεξεργασία **δέσμης** | .. .with αυτό το **σύμπλεγμα λειτουργικό σύστημα** | .. .from αυτό το **πρόγραμμα-πελάτη λειτουργικό σύστημα** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ή των Windows        |
| [Καμπύλη](hdinsight-hadoop-use-pig-curl.md)                      |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Linux, Unix, Mac OS X ή των Windows        |
| [SDK .NET για Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows (προς το παρόν)                        |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)  |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows                                  |
| [Σύνδεση απομακρυσμένης επιφάνειας εργασίας](hdinsight-hadoop-use-pig-remote-desktop.md)  |              ✔              |            ✔            | Windows                                   | Windows                                  |


## <a name="running-pig-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Εργασίες που εκτελούνται γουρούνι στην Azure HDInsight με χρήση εσωτερικής εγκατάστασης των υπηρεσιών ενοποίησης του SQL Server

Μπορείτε επίσης να χρησιμοποιήσετε τις υπηρεσίες ενοποίησης SQL Server (SSIS) για να εκτελέσετε μια εργασία γουρούνι. Το πακέτο δυνατοτήτων Azure για SSIS παρέχει τα παρακάτω στοιχεία που λειτουργούν με εργασίες γουρούνι σε HDInsight.


- [Azure HDInsight γουρούνι εργασίας][pigtask]
- [Διαχείριση συνδέσεων Azure συνδρομή][connectionmanager]


Μάθετε περισσότερα σχετικά με το πακέτο δυνατοτήτων Azure για SSIS [εδώ][ssispack].


##<a id="nextsteps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς μπορείτε να χρησιμοποιήσετε γουρούνι με HDInsight, χρησιμοποιήστε τις παρακάτω συνδέσεις για να εξερευνήσετε άλλους τρόπους για να εργαστείτε με Azure HDInsight.

* [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
* [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
* [Χρήση Sqoop με HDInsight](hdinsight-use-sqoop.md)
* [Χρήση Oozie με HDInsight](hdinsight-use-oozie.md)
* [Χρήση MapReduce εργασίες με το HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-pig/hdi.checkmark.png

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: ../powershell-install-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx

[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png

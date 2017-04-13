<properties
    pageTitle="Υποβολή εργασιών τους από μακριά χρησιμοποιώντας Λίβιος | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε Λίβιος με συμπλεγμάτων HDInsight να υποβάλλουν από απόσταση εργασίες τους."
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
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Υποβολή εργασιών τους από απόσταση σε ένα σύμπλεγμα Apache τους στη χρήση Λίβιος HDInsight Linux

Σύμπλεγμα Apache τους σε Azure HDInsight περιλαμβάνει Λίβιος, ένα περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ για την υποβολή εργασίες από απόσταση σε ένα σύμπλεγμα τους. Για πιο αναλυτική τεκμηρίωση, ανατρέξτε στο θέμα [Λίβιος](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Μπορείτε να χρησιμοποιήσετε Λίβιος εκτέλεσης αλληλεπιδραστικών τους άλλου κελύφους ή υποβολή μαζικές εργασίες πρέπει να εκτελεστούν από τους. Σε αυτό το άρθρο ακρόασης σχετικά με τη χρήση Λίβιος για να υποβάλετε μαζικές εργασίες. Η σύνταξη της παρακάτω χρησιμοποιεί καμπύλη για την πραγματοποίηση κλήσεων ΥΠΌΛΟΙΠΟ στο τελικό σημείο Λίβιος.

**Προαπαιτούμενα στοιχεία:**

Πρέπει να έχετε τα εξής:

- Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ένα σύμπλεγμα Apache τους σε HDInsight Linux. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία τους Apache συμπλεγμάτων στο Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Υποβάλετε μια μαζική εργασία στο σύμπλεγμα

Πριν να υποβάλετε μια μαζική εργασία, πρέπει να αποστείλετε την εφαρμογή βάζο του χώρου αποθήκευσης συμπλέγματος που σχετίζεται με το σύμπλεγμα. Μπορείτε να χρησιμοποιήσετε [**AzCopy**](../storage/storage-use-azcopy.md), ένα βοηθητικό πρόγραμμα γραμμής εντολών, για να το κάνετε. Υπάρχουν πολλά άλλα προγράμματα-πελάτες, μπορείτε να χρησιμοποιήσετε για την αποστολή δεδομένων. Μπορείτε να βρείτε περισσότερες πληροφορίες για τους στην [Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Παραδείγματα**:

* Εάν το αρχείο βάζο του χώρου αποθήκευσης συμπλέγματος (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Εάν το που θέλετε να περάσει το όνομα αρχείου βάζο και το όνομα κλάσης ως μέρος του αρχείου εισαγωγής (σε αυτό το παράδειγμα, input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Λήψη πληροφοριών σχετικά με δέσμες εκτελούνται στο σύμπλεγμα

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Παραδείγματα**:

* Εάν θέλετε να ανακτήσετε όλες τις δέσμες εκτελούνται στο σύμπλεγμα:

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Εάν θέλετε να ανακτήσετε μια συγκεκριμένη δέσμη με μια δεδομένη batchId

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Διαγράψτε μια μαζική εργασία

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Παράδειγμα**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Λίβιος και υψηλής διαθεσιμότητας

Λίβιος παρέχει υψηλής διαθεσιμότητας για τους εργασίες που εκτελούνται στο σύμπλεγμα. Ακολουθούν μερικά παραδείγματα.

* Εάν η υπηρεσία Λίβιος μεταβαίνει προς τα κάτω, αφού έχετε υποβάλει μια εργασία από απόσταση σε ένα σύμπλεγμα τους, η εργασία εξακολουθεί να εκτελείται στο παρασκήνιο. Όταν Λίβιος αντίγραφα ασφαλείας, επαναφέρει την κατάσταση του έργου και αναφορές την ξανά.

* Σημειωματάρια Jupyter για HDInsight είναι υποστηρίζεται από Λίβιος στον υπολογιστή στο παρασκήνιο. Εάν ένα σημειωματάριο εκτέλεσης μιας εργασίας τους και λαμβάνει επανεκκίνηση της υπηρεσίας Λίβιος, το Σημειωματάριο θα εξακολουθήσει να εκτελέσετε τα κελιά κώδικα. 

## <a name="show-me-an-example"></a>Παρουσίαση παραδείγματος

Σε αυτήν την ενότητα, εξετάσουμε παραδείγματα σχετικά με τον τρόπο χρήσης Λίβιος για την υποβολή μιας εφαρμογής τους, παρακολούθηση της προόδου της εφαρμογής και, στη συνέχεια, διαγράψτε την εργασία. Η εφαρμογή χρησιμοποιούμε σε αυτό το παράδειγμα είναι αυτό που αναπτύχθηκε στο άρθρο [Δημιουργία μια μεμονωμένη εφαρμογή Scala και να εκτελέσετε στο σύμπλεγμα HDInsight τους](hdinsight-apache-spark-create-standalone-application.md). Τα παρακάτω βήματα θεωρείται ότι τα εξής:

* Έχετε ήδη αντιγράψει επάνω από την εφαρμογή βάζο με το λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.
* Έχετε εγκατεστημένη στον υπολογιστή όπου προσπαθείτε αυτά τα βήματα καμπύλη.

Ακολουθήστε τα παρακάτω βήματα.

1. Επιτρέψτε μας πρώτα βεβαιωθείτε ότι Λίβιος εκτελείται στο σύμπλεγμα. Θα σας να το κάνετε γρήγορα μια λίστα εκτελούνται δέσμες. Εάν αυτή είναι η πρώτη φορά που εκτελείτε μια εργασία χρησιμοποιώντας Λίβιος, αυτό θα πρέπει να επιστρέψει μηδέν.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Θα πρέπει να λάβετε το αποτέλεσμα παρόμοιο με το εξής:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Παρατηρήστε πώς η τελευταία γραμμή στο αποτέλεσμα εμφανίζεται η ένδειξη **σύνολο: 0**, η οποία προτείνει δεν εκτελείται δέσμες.

2. Επιτρέψτε μας τώρα να υποβάλετε μια μαζική εργασία. Το παρακάτω τμήμα κώδικα χρησιμοποιεί ένα αρχείο εισόδου (input.txt) για τη μεταβίβαση το όνομα βάζο και το όνομα της κλάσης ως παράμετροι. Αυτή είναι η προτεινόμενη προσέγγιση εάν εκτελείτε αυτά τα βήματα από έναν υπολογιστή Windows.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Οι παράμετροι του αρχείου **input.txt** ορίζονται ως εξής:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Παρατηρήστε πώς αναφέρει ότι η τελευταία γραμμή του αποτελέσματος **κατάσταση: Έναρξη**. Επίσης εμφανίζεται η ένδειξη, **αναγνωριστικό: 0**. Αυτό είναι το αναγνωριστικό της δέσμης.

3. Τώρα, μπορείτε να ανακτήσετε τα της κατάστασης των αυτήν τη συγκεκριμένη δέσμη χρησιμοποιώντας το αναγνωριστικό της δέσμης.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Το αποτέλεσμα εμφανίζει τώρα **κατάσταση: επιτυχίας**, που προτείνει ότι η εργασία ολοκληρώθηκε με επιτυχία.

4. Εάν θέλετε, μπορείτε τώρα να διαγράψετε τη δέσμη.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Η τελευταία γραμμή του αποτελέσματος δείχνει ότι η δέσμη έχει διαγραφεί με επιτυχία. Εάν διαγράψετε μια εργασία ενώ εκτελείται, αυτό θα ουσιαστικά τερματισμός της εργασίας. Εάν διαγράψετε μια εργασία που έχει ολοκληρωθεί, διαφορετικά, ή με επιτυχία διαγράφει εντελώς πληροφορίες για την εργασία.

## <a name="seealso"></a>Δείτε επίσης


* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight για την ανάλυση δόμησης θερμοκρασίας με τη χρήση δεδομένων HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη τροφίμων στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

* [Ανάλυση καταγραφής τοποθεσία Web χρησιμοποιώντας τους στο HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Δημιουργία και εκτέλεση εφαρμογών

* [Δημιουργήστε μια μεμονωμένη εφαρμογή χρησιμοποιώντας Scala](hdinsight-apache-spark-create-standalone-application.md)

### <a name="tools-and-extensions"></a>Εργαλεία και επεκτάσεις

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για να δημιουργήσετε και να υποβάλετε τους Scala εφαρμογών](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για τον εντοπισμό σφαλμάτων εφαρμογών τους από απόσταση](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Χρήση Zeppelin σημειωματάρια με ένα σύμπλεγμα τους σε HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Διαθέσιμο για Jupyter σημειωματαρίου στο σύμπλεγμα τους για HDInsight πυρήνων](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Χρήση εξωτερικών πακέτων με σημειωματάρια Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Εγκατάσταση Jupyter στον υπολογιστή σας και να συνδεθείτε με ένα σύμπλεγμα HDInsight τους](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Διαχείριση πόρων

* [Διαχείριση πόρων για το σύμπλεγμα Apache τους στο Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Παρακολούθηση και ο εντοπισμός σφαλμάτων εργασίες που εκτελείται σε ένα σύμπλεγμα Apache τους στο HDInsight](hdinsight-apache-spark-job-debugging.md)

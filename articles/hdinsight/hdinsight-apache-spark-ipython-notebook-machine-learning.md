<properties 
    pageTitle="Χρησιμοποιήστε τους Apache για να δημιουργήσετε εφαρμογές μηχανικής εκμάθησης HDInsight | Microsoft Azure" 
    description="Οδηγίες βήμα προς βήμα σχετικά με τη χρήση σημειωματαρίων με Apache τους για να δημιουργήσετε εφαρμογές μηχανικής εκμάθησης" 
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


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Δημιουργία εφαρμογών μηχανικής εκμάθησης για να εκτελέσετε σε συμπλεγμάτων Apache τους σε HDInsight Linux

Μάθετε πώς να δημιουργείτε μια μηχανικής εκμάθησης εφαρμογής χρησιμοποιώντας ένα σύμπλεγμα Apache τους στο HDInsight. Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε το Σημειωματάριο Jupyter διαθέσιμη με το σύμπλεγμα να δημιουργήσετε και να ελέγξετε την εφαρμογή. Η εφαρμογή χρησιμοποιεί το δείγμα δεδομένων HVAC.csv που είναι διαθέσιμες στο όλα συμπλεγμάτων από προεπιλογή.

**Προαπαιτούμενα στοιχεία:**

Πρέπει να έχετε τα εξής:

- Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ένα σύμπλεγμα Apache τους σε HDInsight Linux. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία τους Apache συμπλεγμάτων στο Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Εμφάνιση των δεδομένων

Πριν μας αρχίσετε να δημιουργείτε την εφαρμογή, επιτρέψτε μας να κατανοήσετε τη δομή των δεδομένων και το είδος της ανάλυσης που θα κάνουμε τα δεδομένα. 

Σε αυτό το άρθρο, μπορούμε να χρησιμοποιήσουμε το δείγμα αρχείου δεδομένων **HVAC.csv** που είναι διαθέσιμη στο λογαριασμό αποθήκευσης Azure που που σχετίζεται με το σύμπλεγμα HDInsight. Μέσα σε το λογαριασμό χώρου αποθήκευσης, το αρχείο βρίσκεται στο **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Κάντε λήψη και ανοίξτε το αρχείο CSV για να λάβετε ένα στιγμιότυπο των δεδομένων.  

![Στιγμιότυπο δεδομένων HVAC] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "Στιγμιότυπο των δεδομένων HVAC")

Τα δεδομένα εμφανίζει τη θερμοκρασία προορισμού και η πραγματική θερμοκρασία κτιρίου που έχει εγκατασταθεί συστήματα HVAC. Ας υποθέσουμε ότι η στήλη **συστήματος** αντιπροσωπεύει Αναγνωριστικού του συστήματος και τη στήλη **SystemAge** αντιπροσωπεύει τον αριθμό των ετών που έχει το σύστημα HVAC στη θέση στο δόμησης.

Χρησιμοποιούμε αυτά τα δεδομένα για να πρόβλεψης εάν κτιρίου θα είναι hotter ή colder που βασίζεται σε τη θερμοκρασία προορισμού, δεδομένο ένα Αναγνωριστικό συστήματος και το σύστημα ηλικία.

##<a name="app"></a>Γράψτε μια εφαρμογή εκμάθησης υπολογιστή χρησιμοποιώντας τους MLlib

Σε αυτήν την εφαρμογή χρησιμοποιούμε μια διοχέτευση ML τους για να εκτελέσετε μια ταξινόμηση εγγράφων. Στη διοχέτευση, θα σας διαίρεση του εγγράφου σε λέξεις, να μετατρέψετε τις λέξεις σε αριθμητική δυνατότητα ανύσματος και τέλος Δημιουργήστε ένα μοντέλο πρόβλεψη χρησιμοποιώντας τη δυνατότητα διανύσματα και ετικέτες. Ακολουθήστε τα παρακάτω βήματα για να δημιουργήσετε την εφαρμογή.

1. Από την [Πύλη Azure](https://portal.azure.com/), από την startboard, κάντε κλικ στο πλακίδιο για το σύμπλεγμα τους (εάν καρφιτσωμένα αυτό για να το startboard). Μπορείτε επίσης να μεταβείτε σε το σύμπλεγμά σας στην περιοχή **Αναζήτηση όλων** > **Συμπλεγμάτων HDInsight**.   

2. Από το σύμπλεγμα blade τους, κάντε κλικ στην επιλογή **Πίνακας εργαλείων σύμπλεγμα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Jupyter σημειωματαρίου**. Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια διαχειριστή για το σύμπλεγμα.

    > [AZURE.NOTE] Μπορείτε επίσης μπορεί να φτάσει στο Σημειωματάριο Jupyter για το σύμπλεγμά σας ανοίγοντας την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Δημιουργήστε ένα νέο σημειωματάριο. Κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **PySpark**.

    ![Δημιουργία νέου σημειωματαρίου Jupyter] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Δημιουργία νέου σημειωματαρίου Jupyter")

3. Δημιουργείται ένα νέο σημειωματάριο και ανοίγει με το όνομα Untitled.pynb. Κάντε κλικ στο όνομα του σημειωματαρίου στο επάνω μέρος και πληκτρολογήστε ένα φιλικό όνομα.

    ![Παροχή ένα όνομα για το Σημειωματάριο] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Παροχή ένα όνομα για το Σημειωματάριο")

3. Επειδή έχετε δημιουργήσει ένα σημειωματάριο χρησιμοποιώντας το PySpark πυρήνα, δεν χρειάζεται να δημιουργήσετε οποιαδήποτε περιβάλλοντα ρητά. Τα περιβάλλοντα τους και η ομάδα θα δημιουργηθεί αυτόματα για εσάς όταν εκτελείτε το πρώτο κελί κώδικα. Μπορείτε να ξεκινήσετε με την εισαγωγή τους τύπους που απαιτούνται για αυτό το σενάριο. Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και, στη συνέχεια, πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Μπορείτε τώρα πρέπει να φορτώσετε τα δεδομένα (hvac.csv), ανάλυσης του και χρησιμοποιήστε το για να εκπαιδεύσετε το μοντέλο. Για αυτό, μπορείτε να ορίσετε μια συνάρτηση που ελέγχει εάν η πραγματική θερμοκρασία του κτιρίου είναι μεγαλύτερη από τη θερμοκρασία προορισμού. Εάν η πραγματική θερμοκρασία είναι μεγαλύτερο, δόμησης είναι συντόμευσης, δηλώνεται από την τιμή **1.0**. Εάν η πραγματική θερμοκρασία είναι μικρότερο βαθμό, δόμησης είναι ψυχρές, δηλώνεται από την τιμή **0,0**. 

    Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Ρύθμιση παραμέτρων της διοχέτευσης τους μηχανικής εκμάθησης που αποτελείται από τρεις στάδια: αντικειμένου δημιουργίας διακριτικών, hashingTF και lr. Για περισσότερες πληροφορίες σχετικά με το τι είναι μια διαδικασία και τον τρόπο που λειτουργεί δείτε <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">τους μηχανικής εκμάθησης διοχέτευσης</a>.

    Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Προσαρμογή της διοχέτευσης στο έγγραφο εκπαίδευσης. Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Επαλήθευση του εκπαιδευτικού εγγράφου σε σημείο ελέγχου την πρόοδό σας με την εφαρμογή. Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        training.show()

    Αυτό θα πρέπει να σας δώσει το αποτέλεσμα είναι παρόμοιο με το εξής:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Μετάβαση προς τα πίσω και να επαληθεύσετε το αποτέλεσμα του σε σχέση με τα ανεπεξέργαστα αρχείο CSV. Για παράδειγμα, την πρώτη γραμμή του αρχείου CSV περιλαμβάνει αυτά τα δεδομένα:

    ![Στιγμιότυπο δεδομένων HVAC] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "Στιγμιότυπο των δεδομένων HVAC")

    Παρατηρήστε πώς η πραγματική θερμοκρασία είναι μικρότερος από το προορισμού θερμοκρασίας που υποδεικνύει την ύπαρξη δόμησης είναι ψυχρές. Ως εκ τούτου στο αποτέλεσμα του εκπαιδευτικού, η τιμή για την **ετικέτα** στην πρώτη γραμμή είναι **0,0**, γεγονός που σημαίνει ότι δεν είναι συντόμευσης δόμησης.

8.  Προετοιμασία ενός συνόλου δεδομένων για να εκτελέσετε το μοντέλο εκπαιδευμένο σε σχέση με. Για να το κάνετε αυτό, θα σας θα μεταβιβάζουν σε Αναγνωριστικό συστήματος και ηλικία σύστημα (δηλώνεται ως **SystemInfo** στο αποτέλεσμα εκπαίδευση) και το μοντέλο θα πρόβλεψης εάν δόμησης με αυτό το Αναγνωριστικό του συστήματος και ηλικία σύστημα θα ήταν hotter (δηλώνεται από 1.0) ή ψυχρότερη (δηλώνεται από 0,0).

    Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Τέλος, μπορείτε να κάνετε προβλέψεις στην τα δεδομένα δοκιμής. Επικολλήστε το εξής τμήμα κώδικα σε ένα κενό κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Θα πρέπει να δείτε το αποτέλεσμα παρόμοιο με το εξής:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Από την πρώτη γραμμή στην πρόβλεψη, μπορείτε να δείτε ότι για ένα σύστημα HVAC με το Αναγνωριστικό 20 και το σύστημα ηλικία 25 έτη, δόμησης θα συντόμευσης (**πρόβλεψη = 1.0**). Η πρώτη τιμή για DenseVector (0.49999) αντιστοιχεί την πρόβλεψη 0,0 και τη δεύτερη τιμή (0.5001) που αντιστοιχεί σε την πρόβλεψη 1.0. Στο αποτέλεσμα, παρόλο που η δεύτερη τιμή είναι μόνο οριακά νεότερη έκδοση, εμφανίζει το μοντέλο **πρόβλεψη = 1.0**.

11. Αφού ολοκληρώσετε την εκτέλεση της εφαρμογής, θα πρέπει να τερματισμού στο Σημειωματάριο για να αφήσετε τους πόρους. Για να το κάνετε αυτό, από το μενού **αρχείο** στο Σημειωματάριο, κάντε κλικ στην επιλογή **Κλείσιμο και διακοπή**. Αυτό θα τερματισμού και κλείσιμο του σημειωματαρίου.
           

##<a name="anaconda"></a>Χρήση Anaconda scikit-μάθετε βιβλιοθήκης για μηχανικής εκμάθησης

Συμπλεγμάτων Apache τους σε HDInsight περιλαμβάνουν Anaconda βιβλιοθήκες. Αυτό περιλαμβάνει επίσης την **scikit-μάθετε** βιβλιοθήκη για μηχανικής εκμάθησης. Η βιβλιοθήκη περιλαμβάνει επίσης διάφορα σύνολα δεδομένων που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε εφαρμογές για το δείγμα απευθείας από ένα σημειωματάριο Jupyter. Για παραδείγματα σχετικά με τη χρήση του scikit-μάθετε βιβλιοθήκη, ανατρέξτε στο θέμα [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Δείτε επίσης

* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη της εστίασης στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

* [Ανάλυση καταγραφής τοποθεσία Web χρησιμοποιώντας τους στο HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Δημιουργία και εκτέλεση εφαρμογών

* [Δημιουργήστε μια μεμονωμένη εφαρμογή χρησιμοποιώντας Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Απομακρυσμένη εκτέλεση εργασιών σε ένα σύμπλεγμα τους χρησιμοποιώντας Λίβιος](hdinsight-apache-spark-livy-rest-interface.md)

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


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

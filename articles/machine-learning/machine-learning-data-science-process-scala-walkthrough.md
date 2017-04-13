<properties
    pageTitle="Χρήση Scala και τους σε Azure Science δεδομένων | Microsoft Azure"
    description="Πώς μπορείτε να χρησιμοποιήσετε Scala για επιβλεπόμενες μηχανικής εκμάθησης εργασίες με τους μεταβλητού μεγέθους MLlib και τους ML πακέτων του σε ένα σύμπλεγμα Azure HDInsight τους."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Χρήση Scala και τους σε Azure Science δεδομένων

Αυτό το άρθρο σάς δείχνει πώς μπορείτε να χρησιμοποιήσετε Scala για επιβλεπόμενες μηχανικής εκμάθησης εργασίες με τους μεταβλητού μεγέθους MLlib και τους ML πακέτων του σε ένα σύμπλεγμα Azure HDInsight τους. Αυτό σας καθοδηγεί σε τις εργασίες που αποτελούν τη [διαδικασία Science δεδομένων](http://aka.ms/datascienceprocess): κατάποσης δεδομένων και Εξερεύνηση, απεικόνιση, η δυνατότητα μηχανικής, μοντελοποίηση και κατανάλωση μοντέλο. Τα μοντέλα στο άρθρο περιλαμβάνουν εφοδιαστική και γραμμικής παλινδρόμησης, τυχαία συμπλεγμάτων δομών και δέντρα ενισχύεται διαβάθμιση (GBTs), εκτός από τις δύο συνήθεις εργασίες εκμάθησης επιβλεπόμενες υπολογιστή:

- Πρόβλημα παλινδρόμησης: πρόβλεψη το ποσό συμβουλή ($) για ένα ταξίδι ταξί
- Δυαδικό ταξινόμηση: πρόβλεψη συμβουλή ή χωρίς συμβουλή (1/0) για ένα ταξίδι ταξί

Η διαδικασία μοντελοποίηση απαιτεί εκπαίδευση και αξιολόγησης σε ένα σύνολο δεδομένων δοκιμής και μετρικά σχετικές ακρίβειας. Σε αυτό το άρθρο, μπορείτε να μάθετε πώς μπορείτε να αποθηκεύσετε αυτά τα μοντέλα στο χώρο αποθήκευσης αντικειμένων Blob του Azure και πώς να βαθμολογία και την αξιολόγησή τους πρόβλεψης επιδόσεων. Σε αυτό το άρθρο καλύπτει επίσης τα πιο σύνθετες θέματα σχετικά με τον τρόπο βελτιστοποίησης μοντέλα χρησιμοποιώντας θανόντων σταυρό επικύρωσης και hyper-παράμετρο. Τα δεδομένα που χρησιμοποιούνται είναι ένα δείγμα του 2013 νέα ΥΌΡΚΗ ταξί ταξιδιού και ναύλο συνόλου δεδομένων διαθέσιμη σε GitHub.

[Scala](http://www.scala-lang.org/), μια γλώσσα με βάση την εικονική μηχανή Java, Ενοποιείται έννοιες προσανατολισμένα σε αντικείμενα και λειτουργική γλώσσας. Πρόκειται για μια γλώσσα με που είναι επίσης και προσαρμοσμένη κατανεμημένη επεξεργασία στο cloud και εκτελείται σε συμπλεγμάτων Azure τους.

[Τους](http://spark.apache.org/) είναι ένα πλαίσιο επεξεργασίας παράλληλα ανοιχτού κώδικα που υποστηρίζει στη μνήμη επεξεργασίας για να ενισχύσει την απόδοση των εφαρμογών ανάλυσης μεγάλο δεδομένων. Ο μηχανισμός επεξεργασίας τους είναι σχεδιασμένες για ταχύτητα, ευκολία στη χρήση και εξελιγμένο analytics. Δυνατότητες στη μνήμη κατανέμεται κατά τον υπολογισμό του τους να είναι μια καλή επιλογή για επαναληπτικού αλγόριθμους σε μηχανικής εκμάθησης και graph υπολογισμούς. Το πακέτο [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) παρέχει ένα ενιαίο σύνολο υψηλού επιπέδου API ενσωματωμένη επάνω από δεδομένα πλαίσια που μπορεί να σας βοηθήσει να δημιουργήσετε και με ακρίβεια τις πρακτικές μηχανικής εκμάθησης αγωγούς. [MLlib](http://spark.apache.org/mllib/) είναι βιβλιοθήκη μεταβλητού μεγέθους μηχανικής εκμάθησης του τους, η οποία εμφανίζει δυνατότητες μοντελοποίησης για αυτό κατανεμημένο περιβάλλον.

[Τους HDInsight](../hdinsight/hdinsight-apache-spark-overview.md) είναι η προσφορά Azure που φιλοξενείται από τους Άνοιγμα προέλευσης. Επίσης περιλαμβάνει υποστήριξη για τα σημειωματάρια Jupyter Scala στο σύμπλεγμα τους, και να εκτελέσετε αλληλεπιδραστικών ερωτήματα SQL τους για να μετατρέψετε, να φιλτράρετε και απεικόνιση δεδομένων που είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Τα τμήματα κώδικα Scala σε αυτό το άρθρο που παρέχουν τις λύσεις και να εμφανίσετε το σχετικό σχεδιάσεων για την απεικόνιση των δεδομένων εκτελούνται σε σημειωματάρια Jupyter εγκατεστημένο στον των συμπλεγμάτων τους. Τα βήματα μοντελοποίηση σε αυτά τα θέματα έχουν κώδικα που δείχνει πώς να εκπαιδεύσετε, αξιολόγηση, αποθήκευση και κατανάλωση κάθε τύπο του μοντέλου.

Τα βήματα για τη ρύθμιση και κώδικα σε αυτό το άρθρο είναι για Azure HDInsight 3.4 τους 1.6. Ωστόσο, ο κώδικας σε αυτό το άρθρο και στο [Σημειωματάριο Jupyter Scala](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) είναι γενικές και θα πρέπει να εργαστείτε σε οποιαδήποτε σύμπλεγμα τους. Τα βήματα ρύθμισης και τη Διαχείριση συμπλέγματος μπορεί να είναι λίγο διαφορετική από το τι εμφανίζεται σε αυτό το άρθρο εάν δεν χρησιμοποιείτε το HDInsight τους.

> [AZURE.NOTE] Για ένα θέμα που σας δείχνει πώς μπορείτε να χρησιμοποιήσετε Python και όχι Scala για να ολοκληρώσετε τις εργασίες για μια διαδικασία Science δεδομένων σε ολοκληρωμένες, ανατρέξτε στο θέμα [Φυσικής δεδομένων με χρήση τους σε Azure HDInsight](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

-   Πρέπει να έχετε μια συνδρομή του Azure. Εάν δεν έχετε ήδη ένα, [λάβετε μια δωρεάν δοκιμαστική έκδοση του Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Χρειάζεστε ένα σύμπλεγμα Azure HDInsight 3.4 τους 1.6 για να ολοκληρώσετε τις παρακάτω διαδικασίες. Για να δημιουργήσετε ένα σύμπλεγμα, ανατρέξτε στην ενότητα [Γρήγορα αποτελέσματα: δημιουργία Apache τους σε Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Ορίστε τον τύπο σύμπλεγμα και την έκδοση από το μενού **Επιλέξτε Τύπος σύμπλεγμα** .

![Ρύθμιση παραμέτρων τύπου HDInsight συμπλέγματος](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Για μια περιγραφή των δεδομένων ταξιδιού ταξί νέα ΥΌΡΚΗ και οδηγίες σχετικά με τον τρόπο εκτέλεσης κώδικα από ένα σημειωματάριο Jupyter στο σύμπλεγμα τους, ανατρέξτε στις σχετικές ενότητες στην [Επισκόπηση δεδομένων επιστημών χρησιμοποιώντας τους σε Azure HDInsight](machine-learning-data-science-spark-overview.md).  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Εκτέλεση κώδικα Scala από ένα σημειωματάριο Jupyter στο σύμπλεγμα τους

Μπορείτε να εκκινήσετε ένα σημειωματάριο Jupyter από την πύλη του Azure. Βρείτε το σύμπλεγμα τους στον πίνακα εργαλείων σας και, στη συνέχεια, κάντε κλικ στην επιλογή για να εισαγάγετε τη σελίδα διαχείρισης για το σύμπλεγμά σας. Στη συνέχεια, κάντε κλικ στην επιλογή **Σύμπλεγμα πινάκων εργαλείων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Jupyter Σημειωματάριο** για να ανοίξετε το Σημειωματάριο που σχετίζεται με το σύμπλεγμα τους.

![Πίνακας εργαλείων σύμπλεγμα και Jupyter σημειωματαρίων](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Μπορείτε επίσης να αποκτήσετε πρόσβαση Jupyter σημειωματάρια στο https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Αντικαταστήστε *clustername* με το όνομα του συμπλέγματος. Χρειάζεστε τον κωδικό πρόσβασης για το λογαριασμό διαχειριστή για να αποκτήσετε πρόσβαση στα σημειωματάρια Jupyter.

![Μετάβαση σε σημειωματάρια Jupyter, χρησιμοποιώντας το όνομα του συμπλέγματος](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Επιλέξτε **Scala** για να δείτε έναν κατάλογο με μερικά παραδείγματα προκαθορισμένους τα σημειωματάρια που χρησιμοποιούν το API PySpark. Η Εξερεύνηση μοντελοποίηση και βαθμολογίας χρήσης Scala.ipynb σημειωματαρίου που περιέχει τα δείγματα κώδικα για αυτό οικογένεια θεμάτων τους είναι διαθέσιμη στο [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Μπορείτε να αποστείλετε το Σημειωματάριο απευθείας από το GitHub στο διακομιστή Jupyter σημειωματαρίου σε το σύμπλεγμά σας τους. Στην αρχική σελίδα της Jupyter, κάντε κλικ στο κουμπί **Αποστολή** . Στην Εξερεύνηση αρχείων, επικολλήστε τη διεύθυνση URL (ανεπεξέργαστα περιεχόμενο) GitHub του σημειωματαρίου Scala και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα**. Το Σημειωματάριο Scala είναι διαθέσιμη από την παρακάτω διεύθυνση URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Πρόγραμμα εγκατάστασης: Τους προκαθορισμένα και Hive περιβάλλοντα magics τους και τους βιβλιοθήκες

### <a name="preset-spark-and-hive-contexts"></a>Προκαθορισμένες τους και Hive περιβάλλοντα

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Το πυρήνων τους που παρέχονται με σημειωματάρια Jupyter διαθέτουν προκαθορισμένες περιβάλλοντα. Δεν χρειάζεται να ορίσετε ρητά την τους ή ανάπτυξη περιβάλλοντα Hive προτού ξεκινήσετε να εργάζεστε με την εφαρμογή. Το προκαθορισμένο περιβάλλοντα είναι οι εξής:

- `sc`για SparkContext
- `sqlContext`για HiveContext


### <a name="spark-magics"></a>Magics τους

Το πυρήνα τους παρέχει ορισμένα προκαθορισμένα "magics", που είναι ειδικές εντολές που μπορείτε να καλέσετε με `%%`. Δύο από αυτές τις εντολές που χρησιμοποιούνται στο τα ακόλουθα δείγματα κώδικα.

- `%%local`Καθορίζει ότι ο κώδικας σε επακόλουθες γραμμές θα εκτελεστεί τοπικά. Ο κωδικός πρέπει να είναι έγκυρο Scala κώδικα.
- `%%sql -o <variable name>`εκτελεί ένα ερώτημα Hive προς `sqlContext`. Εάν το `-o` που του μεταβιβάστηκε η παράμετρος, το αποτέλεσμα του ερωτήματος είναι μόνιμα σε το `%%local` Scala περιβάλλοντος ως πλαισίου δεδομένων τους.

Για περισσότερες πληροφορίες σχετικά με το πυρήνων για σημειωματάρια Jupyter και τις προκαθορισμένες "magics" που καλείτε με `%%` (για παράδειγμα, `%%local`), ανατρέξτε στο θέμα [πυρήνων που είναι διαθέσιμες για τα σημειωματάρια Jupyter με συμπλεγμάτων Linux τους HDInsight σε HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Εισαγωγή βιβλιοθήκες

Εισαγάγετε το τους, MLlib και άλλων βιβλιοθηκών θα χρειαστεί, χρησιμοποιώντας τον παρακάτω κώδικα.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Κατάποσης δεδομένων

Το πρώτο βήμα της διεργασίας Science δεδομένων είναι να ingest τα δεδομένα που θέλετε να αναλύσετε. Μπορείτε να μεταφέρετε τα δεδομένα από εξωτερικές προελεύσεις ή συστήματα το οποίο βρίσκεται στην το περιβάλλον Εξερεύνηση και μοντελοποίηση δεδομένων. Σε αυτό το άρθρο, τα δεδομένα που ingest είναι ένα συνδεδεμένο δείγμα 0,1% του ταξί ταξιδιού και ναύλο αρχείου (αποθηκεύονται ως αρχείο .tsv). Το περιβάλλον Εξερεύνηση και μοντελοποίηση δεδομένων είναι τους. Αυτή η ενότητα περιέχει τον κωδικό για να ολοκληρώσετε την παρακάτω σειρά εργασιών:

1. Ορισμός καταλόγου διαδρομές για δεδομένα και μοντέλο χώρου αποθήκευσης.
2. Διαβάστε στο σύνολο δεδομένων εισόδου (αποθηκεύονται ως αρχείο .tsv).
3. Ορίσετε μια διάταξη για τα δεδομένα και να εκκαθαρίσετε τα δεδομένα.
4. Δημιουργήστε ένα πλαίσιο καθαρισμός δεδομένων και το cache στη μνήμη.
5. Καταχωρήστε τα δεδομένα ως προσωρινός πίνακας σε SQLContext.
6. Υποβολή ερωτήματος στον πίνακα και εισαγωγή τα αποτελέσματα σε ένα πλαίσιο δεδομένων.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Ορισμός καταλόγου διαδρομές για θέσεις αποθήκευσης στο χώρο αποθήκευσης αντικειμένων Blob του Azure

Τους να διαβάζετε και να γράφετε με το χώρο αποθήκευσης αντικειμένων Blob του Azure. Μπορείτε να χρησιμοποιήσετε τους για να επεξεργαστείτε οποιοδήποτε από τα υπάρχοντα δεδομένα σας και, στη συνέχεια, να αποθηκεύσετε ξανά τα αποτελέσματα στο χώρο αποθήκευσης αντικειμένων Blob.

Για να αποθηκεύσετε μοντέλα ή τα αρχεία στο χώρο αποθήκευσης αντικειμένων Blob, πρέπει να ορίσετε σωστά τη διαδρομή. Αναφορά το προεπιλεγμένο κοντέινερ που συνδέονται με το σύμπλεγμα τους, χρησιμοποιώντας μια διαδρομή που αρχίζει με `wasb:///`. Αναφορά άλλες θέσεις χρησιμοποιώντας `wasb://`.

Το παρακάτω δείγμα κώδικα Καθορίζει τη θέση των δεδομένων εισαγωγής για ανάγνωση και τη διαδρομή προς το χώρο αποθήκευσης αντικειμένων Blob που είναι συνδεδεμένος στο σύμπλεγμα τους όπου θα αποθηκευτεί το μοντέλο.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Εισαγωγή δεδομένων, να δημιουργήσετε ένα RDD και ορίζουν ένα πλαίσιο δεδομένα σύμφωνα με το σχήμα

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 8 δευτερόλεπτα.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Υποβολή ερωτήματος στον πίνακα και αποτελέσματα σε ένα πλαίσιο δεδομένων εισαγωγής

Στη συνέχεια, ερωτήματος στον πίνακα για ναύλο επιβατών και συμβουλή δεδομένα. φιλτράρετε τα δεδομένα κατεστραμμένη και απομονωμένα; και να εκτυπώσετε πολλές γραμμές.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Αποτέλεσμα:**

fare_amount|passenger_count|tip_amount|επικαλυπτόμενα
-----------|---------------|----------|------
       13.5|            1.0|       2.9|   1.0
       16.0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Εξερεύνηση δεδομένων και απεικόνισης

Αφού μπορείτε να μεταφέρετε τα δεδομένα στην τους, το επόμενο βήμα της διεργασίας Science δεδομένων είναι να αποκτήσει ένα βαθύτερη Κατανόηση των δεδομένων μέσω Εξερεύνηση και την απεικόνιση. Σε αυτήν την ενότητα, μπορείτε να εξετάσετε τα δεδομένα ταξί χρησιμοποιώντας ερωτήματα SQL. Στη συνέχεια, εισαγάγετε τα αποτελέσματα σε ένα πλαίσιο δεδομένων για να απεικονίσετε τις μεταβλητές προορισμού και πιθανούς δυνατότητες για οπτική επιθεώρηση χρησιμοποιώντας τη δυνατότητα αυτόματης απεικόνιση της Jupyter.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Χρήση του τοπικού και μαγικός SQL για να απεικονίσετε δεδομένα

Από προεπιλογή, το αποτέλεσμα της οποιοδήποτε τμήμα κώδικα που εκτελείτε από ένα σημειωματάριο Jupyter είναι διαθέσιμα στο περιβάλλον της περιόδου λειτουργίας που έχει διατηρηθεί σε τους κόμβους εργασίας. Εάν θέλετε να αποθηκεύσετε ένα ταξίδι τους κόμβους εργασίας για κάθε υπολογισμό και, εάν όλα τα δεδομένα που χρειάζεστε για τον υπολογισμό είναι διαθέσιμη τοπικά στον κόμβο διακομιστή του Jupyter (το οποίο είναι ο κόμβος κεφαλής), μπορείτε να χρησιμοποιήσετε το `%%local` μαγικός για να εκτελέσετε το τμήμα κώδικα στο διακομιστή Jupyter.

- **Μαγικός SQL** (`%%sql`). Το HDInsight τους πυρήνα υποστηρίζει εύκολο ενσωματωμένη σε ερωτήματα HiveQL από SQLContext. Το (`-o VARIABLE_NAME`) όρισμα εξακολουθεί να εμφανίζεται το αποτέλεσμα του ερωτήματος SQL ως πλαισίου δεδομένων Pandas στο διακομιστή Jupyter. Αυτό σημαίνει ότι θα είναι διαθέσιμα σε τοπική λειτουργία.
- `%%local`**μαγικός**. Το `%%local` μαγικός εκτελεί τον κώδικα τοπικά στο διακομιστή Jupyter, δηλαδή το κεφαλής κόμβο του συμπλέγματος HDInsight. Συνήθως, χρησιμοποιείτε `%%local` Μαγικό σε συνδυασμό με το `%%sql` Μαγικό με το `-o` παραμέτρου. Το `-o` παραμέτρου θα παραμένει το αποτέλεσμα του ερωτήματος SQL τοπικά, και, στη συνέχεια, `%%local` μαγικός θα έναυσμα στο επόμενο σύνολο τμήμα κώδικα για να εκτελέσετε τοπικά σε σχέση με το αποτέλεσμα της τα ερωτήματα SQL που έχει διατηρηθεί τοπικά.

### <a name="query-the-data-by-using-sql"></a>Ερώτημα τα δεδομένα, χρησιμοποιώντας SQL
Αυτό το ερώτημα ανακτά τα ταξίδια ταξί ναύλο ποσό, πλήθος επιβατών και ποσό συμβουλή.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Τον παρακάτω κώδικα, η `%%local` μαγικός δημιουργεί ένα πλαίσιο τοπικών δεδομένων, sqlResults. Μπορείτε να χρησιμοποιήσετε sqlResults για να απεικονίσετε με τη χρήση matplotlib.

> [AZURE.TIP] Τοπική μαγικός χρησιμοποιείται πολλές φορές σε αυτό το άρθρο. Εάν το σύνολο δεδομένων σας είναι μεγάλο, επικοινωνήστε δείγματος για να δημιουργήσετε ένα πλαίσιο δεδομένων που μπορεί να χωρέσει στο τοπικής μνήμης.

### <a name="plot-the-data"></a>Σχεδίαση των δεδομένων

Μπορείτε να σχεδιάσετε με χρήση κώδικα Python μετά το πλαίσιο δεδομένων βρίσκεται σε τοπική περιβάλλοντος ως πλαισίου Pandas δεδομένων.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Το πυρήνα τους απεικονίζει αυτόματα το αποτέλεσμα της ερωτήματα SQL (HiveQL) μετά την εκτέλεση του κώδικα. Μπορείτε να επιλέξετε ανάμεσα σε πολλούς τύπους των απεικονίσεων που είναι:
 
- Πίνακας
- Πίτα
- Γραμμή
- Περιοχή
- Γραμμή

Ακολουθεί ο κώδικας για να απεικονίσετε τα δεδομένα:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Αποτέλεσμα:**

![Συμβουλή ποσό ιστογράμματος](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Ποσό συμβουλή από επιβατών μέτρηση](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Συμβουλή ποσό κατά τιμή ναύλο](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Δημιουργία δυνατότητες και Μετασχηματισμός δυνατοτήτων και, στη συνέχεια, προετοιμασία των δεδομένων για εισαγωγή δεδομένων σε μοντελοποίησης συναρτήσεις

Για τις συναρτήσεις βάσει δέντρου μοντελοποίηση από τους ML και MLlib, πρέπει να προετοιμάσετε προορισμού και δυνατότητες, χρησιμοποιώντας μια ποικιλία τεχνικές, όπως binning, η δημιουργία ευρετηρίου, πρόσβασης μίας κωδικοποίηση και vectorization. Εδώ θα βρείτε τις διαδικασίες για να παρακολουθήσετε σε αυτήν την ενότητα:

1. Δημιουργία μια νέα δυνατότητα με **binning** ώρες σε κίνηση Κάδοι ώρα.
2. Εφαρμογή **δημιουργίας ευρετηρίου και ζεστού μία κωδικοποίηση** έλαβε δυνατότητες.
3. **Δείγμα και διαίρεση το σύνολο δεδομένων** σε κλάσματα εκπαίδευση και έλεγχος.
4. **Καθορισμός μεταβλητή εκπαίδευση και δυνατότητες**, και, στη συνέχεια, να δημιουργείτε με ευρετήριο ή πρόσβασης μίας κωδικοποιημένη εκπαίδευση και δοκιμές εισαγωγής με ετικέτες σημείο είναι ανθεκτικά κατανεμημένο σύνολα δεδομένων (RDDs) ή πλαίσια δεδομένων.
5. Αυτόματη **Κατηγοριοποίηση και vectorize δυνατοτήτων και των προορισμών** για να χρησιμοποιήσετε ως εισροές για μοντέλα μηχανικής εκμάθησης.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Δημιουργία μια νέα δυνατότητα με binning ώρες σε κίνηση Κάδοι χρόνου

Αυτόν τον κωδικό σας δείχνει πώς μπορείτε να δημιουργήσετε μια νέα δυνατότητα κατά binning ώρες σε κίνηση ώρα Κάδοι και πώς μπορείτε να το πλαίσιο που προκύπτει δεδομένων στη μνήμη cache. Επανειλημμένα, όπου χρησιμοποιούνται πλαίσια RDDs και δεδομένων σε cache υποψήφιους πελάτες για βελτιωμένη χρόνους εκτέλεσης. Ως εκ τούτου, που θα cache RDDs και πλαίσια δεδομένων σε πολλά στάδια τις παρακάτω διαδικασίες.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Δημιουργία ευρετηρίου και ζεστού μία κωδικοποίηση έλαβε δυνατοτήτων

Η μοντελοποίηση και πρόβλεψης συναρτήσεις MLlib απαιτεί δυνατότητες με έλαβε εισαγωγής δεδομένων με ευρετήριο ή κωδικοποιημένη πριν από τη χρήση. Αυτή η ενότητα σας δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο ή κωδικοποίηση έλαβε δυνατότητες για εισαγωγή δεδομένων σε τις συναρτήσεις μοντελοποίησης.

Πρέπει να δημιουργήσετε ευρετήριο ή να κωδικοποιήσετε μοντέλων σας με διάφορους τρόπους, ανάλογα με το μοντέλο. Για παράδειγμα, μοντέλα εφοδιαστική και γραμμικής παλινδρόμησης απαιτούν πρόσβασης μίας κωδικοποίηση. Για παράδειγμα, μια δυνατότητα με τρεις κατηγορίες μπορούν να αναπτυχθούν σε τρεις στήλες δυνατότητα. Κάθε στήλη θα περιέχει 0 ή 1, ανάλογα με την κατηγορία μια παρακολούθηση. MLlib παρέχει τη συνάρτηση [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) για ένα ζεστού κωδικοποίηση. Αυτό encoder αντιστοιχίζει μια στήλη δεικτών ετικέτα σε μια στήλη με δυαδικό φορέων με πολύ ένα μόνο μία τιμή. Με αυτήν την κωδικοποίηση, αλγορίθμους που περιμένατε αριθμητικών τιμών δυνατότητες, όπως εφοδιαστική παλινδρόμησης, μπορούν να εφαρμοστούν σε έλαβε δυνατότητες.

Εδώ μπορείτε να μετατρέψετε μόνο τέσσερις μεταβλητές για να εμφανίσετε παραδείγματα, η οποία είναι συμβολοσειρές χαρακτήρων. Μπορείτε επίσης να δημιουργήσετε ευρετήριο άλλες μεταβλητές, όπως η ημέρα της εβδομάδας, που αντιπροσωπεύονται από αριθμητικές τιμές, ως έλαβε μεταβλητές.

Για τη δημιουργία ευρετηρίου, χρησιμοποιήστε `StringIndexer()`, και για ένα ζεστού κωδικοποίηση, χρησιμοποιήστε `OneHotEncoder()` συναρτήσεων από MLlib. Ακολουθεί ο κώδικας για να δημιουργήσετε ευρετήριο και να κωδικοποιείτε έλαβε δυνατότητες:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 4 δευτερόλεπτα.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Δείγμα και διαίρεση το σύνολο δεδομένων σε κλάσματα εκπαίδευση και έλεγχος

Αυτός ο κώδικας δημιουργεί μια τυχαία δειγματοληψία των δεδομένων (25%, σε αυτό το παράδειγμα). Παρόλο που δεν είναι απαραίτητο για αυτό το παράδειγμα λόγω του μεγέθους του συνόλου δεδομένων δειγματοληψία, το άρθρο σάς δείχνει πώς μπορείτε να λάβετε δείγματα ώστε να γνωρίζετε πώς μπορείτε να το χρησιμοποιήσετε για τα δικά σας προβλήματα όταν είναι απαραίτητο. Όταν δείγματα είναι μεγάλο, αυτό μπορεί να εξοικονομήσει αρκετό χρόνο ενώ εκπαίδευση μοντέλα. Στη συνέχεια, διαιρέστε το δείγμα σε ένα τμήμα εκπαίδευση (75%, σε αυτό το παράδειγμα) και ένα τμήμα δοκιμών (25%, σε αυτό το παράδειγμα) για να χρησιμοποιήσετε στην ταξινόμηση και μοντελοποίηση παλινδρόμησης.

Προσθέστε έναν τυχαίο αριθμό (μεταξύ 0 και 1) σε κάθε γραμμή (σε μια στήλη "rand") που μπορούν να χρησιμοποιηθούν για να επιλέξετε πτυχών σταυρό επικύρωσης κατά τη διάρκεια της εκπαίδευσης.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 2 δευτερόλεπτα.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Καθορίσετε μεταβλητή εκπαίδευση και δυνατότητες και, στη συνέχεια, δημιουργήστε με ευρετήριο ή πρόσβασης μίας κωδικοποιημένη εκπαίδευση και έλεγχος εισόδου με την ετικέτα πλαίσια RDDs σημείο ή δεδομένων

Αυτή η ενότητα περιέχει κώδικα που δείχνει πώς μπορείτε να index δεδομένα έλαβε κειμένου ως τύπο δεδομένων με ετικέτες σημείο, και να κωδικοποιείτε αυτό, ώστε να μπορείτε να το χρησιμοποιήσετε για την εκπαίδευση και δοκιμή παλινδρόμησης εφοδιαστική MLlib και άλλα μοντέλα κατάταξης. Τα αντικείμενα με ετικέτες σημείο είναι RDDs που έχουν μορφοποιηθεί με τον τρόπο που είναι απαραίτητη ως εισαγωγής δεδομένων από το μεγαλύτερο μέρος μηχανικής εκμάθησης αλγόριθμους στο MLlib. Μια [ετικέτα σημείο](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) είναι ένα τοπικό ανύσματος, είτε πυκνό ή κατακερματισμένο, που σχετίζεται με μια ετικέτα/απάντηση.

Σε αυτόν τον κωδικό, μπορείτε να καθορίσετε τη μεταβλητή προορισμού (εξαρτημένα) και τις δυνατότητες για να χρησιμοποιήσετε για να εκπαιδεύσετε μοντέλα. Στη συνέχεια, δημιουργείτε με ευρετήριο ή πρόσβασης μίας κωδικοποιημένη εκπαίδευση και έλεγχος εισόδου με την ετικέτα πλαίσια RDDs σημείο ή δεδομένων.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 4 δευτερόλεπτα.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Αυτόματη κατηγοριοποίηση και vectorize δυνατοτήτων και των προορισμών που θα χρησιμοποιηθεί ως εισροές για μηχανικής εκμάθησης μοντέλων

Χρησιμοποιήστε τους ML για να κατηγοριοποιήσετε τις δυνατότητες για χρήση σε συναρτήσεις βάσει δέντρου μοντελοποίηση και προορισμού. Ο κώδικας ολοκληρώνει δύο εργασίες:

-   Δημιουργεί μια δυαδική προορισμού για ταξινόμηση αντιστοιχίζοντας μια τιμή μεταξύ του 0 και 1 σε κάθε σημείο δεδομένων μεταξύ 0 και 1, χρησιμοποιώντας μια οριακή τιμή του 0,5.
- Κατηγοριοποιεί αυτόματα δυνατότητες. Εάν ο αριθμός διακριτών αριθμητικές τιμές για οποιαδήποτε δυνατότητα είναι μικρότερο από 32, κατηγοριοποιηθεί αυτήν τη δυνατότητα.

Παρακάτω θα δείτε τον κώδικα για αυτές τις δύο εργασίες.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Δυαδικό ταξινόμηση μοντέλο: πρόβλεψης εάν θα πρέπει να δοθεί μια συμβουλή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε μοντέλα δυαδικό ταξινόμηση πρόβλεψη έστω και θα πρέπει να δοθεί μια συμβουλή τρεις τύπους:

- Ένα **μοντέλο εφοδιαστική παλινδρόμησης** , χρησιμοποιώντας τα ML τους `LogisticRegression()` συνάρτηση
- Ένα **μοντέλο κατάταξης τυχαία δάσος** χρησιμοποιώντας τα ML τους `RandomForestClassifier()` συνάρτηση
- Ένα **διαβαθμίσεις μοντέλο κατάταξης ενίσχυση δέντρου** , χρησιμοποιώντας το MLlib `GradientBoostedTrees()` συνάρτηση

### <a name="create-a-logistic-regression-model"></a>Δημιουργήστε ένα μοντέλο εφοδιαστική παλινδρόμησης

Στη συνέχεια, δημιουργήστε ένα μοντέλο εφοδιαστική παλινδρόμησης, χρησιμοποιώντας τα ML τους `LogisticRegression()` συνάρτηση. Μπορείτε να δημιουργήσετε το μοντέλο δημιουργία κώδικα σε μια σειρά από βήματα:

1. Ορισμός **τρένο του μοντέλου** δεδομένων με μία παράμετρο.
2. **Αξιολόγηση του μοντέλου** σε ένα σύνολο δεδομένων δοκιμής με μετρήσεις.
3. **Αποθηκεύστε το μοντέλο** στο χώρο αποθήκευσης αντικειμένων Blob για μελλοντική κατανάλωση.
4. **Βαθμολογία του μοντέλου** σε σχέση με δεδομένα δοκιμής.
5. **Σχεδιάστε τα αποτελέσματα** με το ακουστικό λειτουργικό καμπύλες χαρακτηριστικό (ROC).

Εδώ είναι ο κωδικός για αυτές τις διαδικασίες:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Φόρτωση, βαθμολογία και αποθηκεύστε τα αποτελέσματα.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Αποτέλεσμα:**

ROC σε δεδομένα δοκιμής = 0.9827381497557599


Χρησιμοποιήστε Python στην τοπική Pandas δεδομένων πλαίσια για τη σχεδίαση της καμπύλης ROC.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


**Αποτέλεσμα:**

![Συμβουλή ή χωρίς καμπύλης ROC συμβουλή](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Δημιουργήστε ένα μοντέλο κατάταξης τυχαία δάσος

Στη συνέχεια, δημιουργήστε ένα μοντέλο κατάταξης τυχαία δάσος χρησιμοποιώντας τα ML τους `RandomForestClassifier()` λειτουργεί και, στη συνέχεια, να αξιολογήσετε το μοντέλο σε δεδομένα δοκιμής.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Αποτέλεσμα:**

ROC σε δεδομένα δοκιμής = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>Δημιουργήστε ένα μοντέλο κατάταξης GBT

Στη συνέχεια, δημιουργήστε ένα μοντέλο κατάταξης GBT με τη χρήση του MLlib `GradientBoostedTrees()` λειτουργεί και, στη συνέχεια, να αξιολογήσετε το μοντέλο σε δεδομένα δοκιμής.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Αποτέλεσμα:**

Περιοχή καμπύλης ROC: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Μοντέλο παλινδρόμησης: πρόβλεψης ποσό συμβουλή

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε δύο τύποι μοντέλων παλινδρόμησης πρόβλεψη το ποσό συμβουλή:

- Ένα **μοντέλο γραμμικής παλινδρόμησης regularized** χρησιμοποιώντας τα ML τους `LinearRegression()` συνάρτηση. Μπορείτε να εξοικονομήσετε το μοντέλο και αξιολόγηση του μοντέλου σε δεδομένα δοκιμής.
- Ένα **μοντέλο παλινδρόμησης ενίσχυση διαβάθμιση δέντρου** , χρησιμοποιώντας τα ML τους `GBTRegressor()` συνάρτηση.


### <a name="create-a-regularized-linear-regression-model"></a>Δημιουργήστε ένα μοντέλο regularized γραμμικής παλινδρόμησης

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 13 δευτερόλεπτα.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Αποτέλεσμα:**

R-sqr σε δεδομένα δοκιμής = 0.5960320470835743


Στη συνέχεια, τα αποτελέσματα δοκιμής ερωτήματος ως πλαισίου δεδομένων και χρησιμοποιήστε AutoVizWidget και matplotlib για την απεικόνιση του.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Ο κώδικας δημιουργεί ένα πλαίσιο τοπικών δεδομένων από το αποτέλεσμα του ερωτήματος και σχεδιάζει τα δεδομένα. Το `%%local` μαγικός δημιουργεί ένα πλαίσιο τοπικών δεδομένων, `sqlResults`, που μπορείτε να χρησιμοποιήσετε για να απεικονίσετε με matplotlib.

>[AZURE.NOTE] Αυτό μαγικός τους χρησιμοποιείται πολλές φορές σε αυτό το άρθρο. Εάν η ποσότητα των δεδομένων είναι μεγάλο, θα πρέπει να δείγματος για να δημιουργήσετε ένα πλαίσιο δεδομένων που μπορεί να χωρέσει στο τοπικής μνήμης.

Δημιουργία σχεδιάσεων χρησιμοποιώντας Python matplotlib.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Αποτέλεσμα:**

![Συμβουλή ποσό: πραγματική έναντι προβλεπόμενες](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>Δημιουργήστε ένα μοντέλο παλινδρόμησης GBT

Δημιουργήστε ένα μοντέλο παλινδρόμησης GBT χρησιμοποιώντας τα ML τους `GBTRegressor()` λειτουργεί και, στη συνέχεια, να αξιολογήσετε το μοντέλο σε δεδομένα δοκιμής.

[Δέντρα ενισχύεται διαβάθμισης](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) είναι σύνολα των δέντρα αποφάσεων. GBTs εκπαίδευση δέντρα αποφάσεων επαναληπτική για την ελαχιστοποίηση της συνάρτησης απώλεια. Μπορείτε να χρησιμοποιήσετε GBTs για παλινδρόμησης και ταξινόμηση. Μπορούν να χειριστείτε έλαβε δυνατότητες, δεν απαιτούν δυνατότητα κλίμακας και να καταγράψετε nonlinearities και τις αλληλεπιδράσεις δυνατότητα. Μπορείτε επίσης να τις χρησιμοποιήσετε σε μια ρύθμιση multiclass ταξινόμηση.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Αποτέλεσμα:**

Δοκιμή R-sqr είναι: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Βοηθητικά προγράμματα εξελιγμένη μοντελοποίηση για βελτιστοποίηση

Σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε υπολογιστή βοηθητικών προγραμμάτων εκμάθησης που χρησιμοποιείτε συχνά προγραμματιστές για βελτιστοποίηση του μοντέλου. Συγκεκριμένα, μπορείτε να βελτιστοποιήσετε μηχανικής εκμάθησης μοντέλων τρεις διαφορετικούς τρόπους με τη χρήση παραμέτρων θανόντων και διασταυρούμενο επικύρωσης:

-   Διαιρέστε τα δεδομένα σε σύνολα τρένο και επικύρωσης, βελτιστοποίηση του μοντέλου, χρησιμοποιώντας θανόντων hyper παραμέτρου σε ένα σύνολο εκπαίδευση και αξιολόγηση σε ένα σύνολο επικύρωσης (γραμμικής παλινδρόμησης)
-   Βελτιστοποίηση του μοντέλου, χρησιμοποιώντας σταυρό επικύρωσης και hyper-παραμέτρου σάρωση με χρήση συνάρτησης CrossValidator του τους ML (δυαδικό ταξινόμηση)
-   Βελτιστοποίηση του μοντέλου με τη χρήση προσαρμοσμένου κώδικα σταυρό επικύρωσης και θανόντων παραμέτρου για να χρησιμοποιήσετε οποιαδήποτε μηχανικής εκμάθησης συνάρτηση και παράμετρος σύνολο (γραμμικής παλινδρόμησης)


**Επικύρωση σταυρό** είναι μια τεχνική που εκτιμάται πόσο καλά θα γενίκευση μοντέλου εκπαίδευση στις γνωστές ένα σύνολο δεδομένων για τις δυνατότητες των συνόλων δεδομένων στην οποία αυτό δεν έχει γίνει εκπαίδευση πρόβλεψης. Η γενική ιδέα πίσω από αυτήν την τεχνική είναι ότι μοντέλου γίνει απομνημόνευση σε ένα σύνολο δεδομένων των γνωστών δεδομένων και, στη συνέχεια, ελέγχεται της ακρίβειας των προβλέψεων του σε σχέση με μια ανεξάρτητη συνόλου δεδομένων. Μια κοινή υλοποίηση είναι να διαιρέσετε ένα σύνολο δεδομένων σε *k*-πτυχών και, στη συνέχεια, εκπαίδευση στο μοντέλο με τρόπο round robin σε όλα εκτός από μία από τις δύο πτυχές.

**Βελτιστοποίηση Hyper-παράμετρος** είναι το πρόβλημα της επιλογής ενός συνόλου hyper-τις παραμέτρους για έναν αλγόριθμο εκμάθησης, συνήθως με το στόχο βελτιστοποίηση μέτρηση της απόδοσης του αλγόριθμου σε ένα σύνολο δεδομένων ανεξάρτητα. Μια hyper-παράμετρος είναι μια τιμή που πρέπει να καθορίσετε εκτός της διαδικασίας εκπαίδευση μοντέλο. Υποθέσεις σχετικά με τις τιμές παραμέτρων hyper μπορεί να επηρεάσει την ευελιξία και την ακρίβεια του μοντέλου. Δέντρα αποφάσεων έχει hyper-παραμέτρους, για παράδειγμα, όπως τον επιθυμητό βάθος και τον αριθμό φύλλων στο δέντρο. Πρέπει να ορίσετε έναν όρο κυρώσεις εφιστεί για έναν υπολογιστή ανύσματος υποστήριξης (SVM).

Κοινές τρόπο για να εκτελέσετε βελτιστοποίηση hyper-η παράμετρος είναι να χρησιμοποιήσετε μια αναζήτηση πλέγμα, που ονομάζεται επίσης μια **παράμετρο εκκαθάριση**. Σε μια αναζήτηση πλέγμα, εκτελείται μια εκτεταμένη αναζήτηση τις τιμές από ένα καθορισμένο υποσύνολο του χώρου της παραμέτρου hyper για έναν αλγόριθμο εκμάθησης. Μπορεί να σας δώσει ένα μετρικό απόδοσης για να ταξινομήσετε τα αποτελέσματα βέλτιστη παράγονται από τον αλγόριθμο αναζήτησης πλέγμα σταυρό επικύρωσης. Εάν χρησιμοποιείτε το θανόντων hyper-η παράμετρος σταυρό επικύρωσης, μπορείτε να αποφύγετε προβλήματα όριο όπως overfitting ενός μοντέλου για την εκπαίδευση δεδομένων. Με αυτόν τον τρόπο, το μοντέλο διατηρεί η δυναμικότητα για να εφαρμόσετε το γενικό σύνολο δεδομένων από το οποίο έχει εξαχθεί τα δεδομένα εκπαίδευσης.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Βελτιστοποίηση ενός μοντέλου γραμμικής παλινδρόμησης με θανόντων hyper παραμέτρου

Στη συνέχεια, διαιρέστε δεδομένων σε σύνολα τρένο και επικύρωσης, χρήση hyper-παραμέτρου σάρωση σε ένα σύνολο εκπαίδευση για να βελτιστοποιήσετε το μοντέλο, και την αξιολόγησή σε ένα σύνολο επικύρωσης (γραμμικής παλινδρόμησης).

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Αποτέλεσμα:**

Δοκιμή R-sqr είναι: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Βελτιστοποίηση του μοντέλου δυαδικό ταξινόμηση με τη χρήση σταυρό επικύρωσης και hyper-παράμετρο θανόντων

Αυτή η ενότητα σας δείχνει πώς να βελτιστοποιήσετε την μοντέλου δυαδικό ταξινόμηση με τη χρήση θανόντων σταυρό επικύρωσης και hyper-παράμετρο. Αυτό χρησιμοποιεί τα ML τους `CrossValidator` συνάρτηση.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 33 δευτερόλεπτα.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Βελτιστοποίηση του μοντέλου γραμμικής παλινδρόμησης με τη χρήση προσαρμοσμένου κώδικα σταυρό επικύρωσης και θανόντων παραμέτρου

Στη συνέχεια, βελτιστοποίηση του μοντέλου με τη χρήση προσαρμοσμένου κώδικα και προσδιορίστε τις παραμέτρους μοντέλο καλύτερα με τη χρήση του κριτηρίου της μεγαλύτερη ακρίβεια. Στη συνέχεια, δημιουργήστε το τελικό μοντέλο, αξιολόγηση του μοντέλου σε δεδομένα δοκιμής και αποθηκεύστε το μοντέλο στο χώρο αποθήκευσης αντικειμένων Blob. Τέλος, φόρτωση στο μοντέλο, βαθμολογία δεδομένα δοκιμής και αξιολόγηση ακρίβειας.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Αποτέλεσμα:**

Ώρα για να εκτελέσετε το κελί: 61 δευτερόλεπτα.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Εκμετάλλευση ενσωματωμένη τους μηχανικής εκμάθησης μοντέλα αυτόματα με Scala

Για μια επισκόπηση των θεμάτων που θα σας καθοδηγήσουν για τις εργασίες που περιλαμβάνουν τη διαδικασία Science δεδομένων στο Azure, ανατρέξτε στο θέμα [Διαδικασία Science δεδομένων ομάδας](http://aka.ms/datascienceprocess).

[Αναλυτικές παρουσιάσεις ομάδας δεδομένων Science διαδικασία](data-science-process-walkthroughs.md) περιγράφει άλλες αναλυτικές παρουσιάσεις σε ολοκληρωμένες που δείχνουν τα βήματα της διαδικασίας ομάδας δεδομένων επιστήμης για συγκεκριμένα σενάρια. Αναλυτικές παρουσιάσεις απεικόνιση επίσης πώς μπορείτε να συνδυάσετε εργαλεία cloud και εσωτερικής εγκατάστασης και υπηρεσίες σε μια ροή εργασίας ή τη διαδικασία για να δημιουργήσετε μια έξυπνη εφαρμογή.

[Ενσωματωμένη τους μηχανικής βαθμολογία να εκμάθησης μοντέλων](machine-learning-data-science-spark-model-consumption.md) δείχνει πώς μπορείτε να χρησιμοποιήσετε κωδικό Scala αυτόματα φόρτωση και βαθμολογία νέων συνόλων δεδομένων με τα μοντέλα μηχανικής εκμάθησης ενσωματωμένη τους και έχουν αποθηκευτεί στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Μπορείτε να ακολουθήσετε τις οδηγίες που παρέχονται και απλώς να την αντικαταστήσετε τον κώδικα Python με κωδικό Scala σε αυτό το άρθρο για αυτοματοποιημένη κατανάλωση.

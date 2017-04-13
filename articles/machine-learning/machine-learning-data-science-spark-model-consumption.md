<properties
    pageTitle="Βαθμολογία ενσωματωμένη τους μηχανικής εκμάθησης μοντέλων | Microsoft Azure"
    description="Μάθετε πώς να βαθμολογία εκμάθησης μοντέλων που έχουν αποθηκευτεί στο χώρο αποθήκευσης αντικειμένων Blob Azure (WASB)."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Βαθμολογία ενσωματωμένη τους μηχανικής εκμάθησης μοντέλων 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Αυτό το θέμα περιγράφει τον τρόπο τοποθέτησης μηχανικής εκμάθησης (ML) τα μοντέλα που έχουν δημιουργηθεί με τη χρήση MLlib τους και είναι αποθηκευμένα στο χώρο αποθήκευσης αντικειμένων Blob Azure (WASB) και πώς μπορείτε να τους βαθμολογία με σύνολα δεδομένων που έχουν αποθηκευτεί επίσης στο WASB. Εμφανίζει τον τρόπο προ-επεξεργασία τα δεδομένα εισόδου, μετασχηματισμός δυνατότητες χρησιμοποιώντας τις συναρτήσεις δημιουργίας ευρετηρίου και κωδικοποίησης στην εργαλειοθήκη του MLlib και πώς μπορείτε να δημιουργήσετε ένα αντικείμενο δεδομένων με ετικέτες σημείο που μπορούν να χρησιμοποιηθούν ως είσοδο για βαθμολογίας με τα μοντέλα ML. Τα μοντέλα που χρησιμοποιούνται για τη βαθμολογία περιλαμβάνουν γραμμικής παλινδρόμησης, εφοδιαστική παλινδρόμησης, τυχαία μοντέλα δάσος και μοντέλα δέντρου ενίσχυση της διαβάθμισης.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

1. Χρειάζεστε ένα λογαριασμό Azure και ένα σύμπλεγμα χρειάζεστε ένα 1.6 HDInsight 3.4 τους HDInsight τους που για να ολοκληρώσετε αυτόν τον οδηγό. Δείτε την [Επισκόπηση των δεδομένων Science χρησιμοποιώντας τους σε Azure HDInsight](machine-learning-data-science-spark-overview.md) για οδηγίες σχετικά με τον τρόπο για να ικανοποιήσετε αυτές τις απαιτήσεις. Αυτό το θέμα περιέχει επίσης μια περιγραφή για τη νέα ΥΌΡΚΗ 2013 ταξί δεδομένα που χρησιμοποιούνται εδώ και οδηγίες σχετικά με τον τρόπο εκτέλεσης κώδικα από ένα σημειωματάριο Jupyter στο σύμπλεγμα τους. Το **pySpark-machine-learning-data-science-spark-model-consumption.ipynb** σημειωματάριο που περιέχει τα δείγματα κώδικα σε αυτό το θέμα είναι διαθέσιμη στο [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Μπορείτε, επίσης, πρέπει να δημιουργήσετε το μηχανικής εκμάθησης μοντέλα που θα έχει βαθμολογηθεί με εδώ από εργασίας έως το θέμα η [Εξερεύνηση δεδομένων και μοντελοποίηση, με τους](machine-learning-data-science-spark-data-exploration-modeling.md) .   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Πρόγραμμα εγκατάστασης: θέσεις αποθήκευσης, βιβλιοθήκες και το προκαθορισμένο περιβάλλον τους

Τους είναι σε θέση να διαβάζετε και να γράφετε για μια χώρο αποθήκευσης αντικειμένων Blob Azure (WASB). Ώστε τα υπάρχοντα δεδομένα σας είναι αποθηκευμένα είναι δυνατή η επεξεργασία χρησιμοποιώντας τους και τα αποτελέσματα που είναι αποθηκευμένα ξανά σε WASB.

Για να αποθηκεύσετε μοντέλα ή αρχεία σε WASB, η διαδρομή πρέπει να έχει καθοριστεί σωστά. Το προεπιλεγμένο κοντέινερ που συνδέονται με το σύμπλεγμα τους μπορούν να χρησιμοποιηθούν χρησιμοποιώντας μια διαδρομή που ξεκινά με: *"wasb / /"*. Το παρακάτω δείγμα κώδικα Καθορίζει τη θέση των δεδομένων για να διαβάσετε και τη διαδρομή για τον κατάλογο αποθήκευσης μοντέλο στην οποία είναι αποθηκευμένο το αποτέλεσμα του μοντέλου. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Ορισμός καταλόγου διαδρομές για θέσεις αποθήκευσης στο WASB

Μοντέλα αποθηκεύονται στο: "wasb: / / / χρήστη/remoteuser/NYCTaxi/μοντέλα". Εάν αυτή η διαδρομή δεν έχει ρυθμιστεί σωστά, μοντέλα δεν φορτώνονται για βαθμολογίας.

Τα αποτελέσματα scored έχουν αποθηκευτεί στο: "wasb: / / / χρήστη/remoteuser/NYCTaxi/ScoredResults". Εάν η διαδρομή φακέλου είναι σωστός, αποτελέσματα δεν αποθηκεύονται σε αυτόν το φάκελο.   


>[AZURE.NOTE] Θέσεις αρχείων διαδρομή μπορεί να αντιγραφή και επικόλληση σε των συμβόλων κράτησης θέσης σε αυτόν τον κωδικό από την έξοδο από το τελευταίο κελί του **machine-learning-data-science-spark-data-exploration-modeling.ipynb** σημειωματαρίου.   


Ακολουθεί ο κώδικας για να ορίσετε διαδρομές καταλόγου: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**ΑΠΟΤΈΛΕΣΜΑ:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Εισαγωγή βιβλιοθήκες

Ορισμός περιβάλλοντος τους και να εισαγάγετε απαραίτητες βιβλιοθήκες με τον ακόλουθο κώδικα

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Προκαθορισμένες περιβάλλον τους και PySpark magics

Το πυρήνων PySpark που παρέχονται με σημειωματάρια Jupyter έχει ένα προκαθορισμένο περιβάλλον. Επομένως, δεν χρειάζεται να ορίσετε το τους ή αναπτύσσετε περιβάλλοντα Hive ρητά πριν να ξεκινήσετε να εργάζεστε με την εφαρμογή. Αυτά είναι διαθέσιμα για εσάς από προεπιλογή. Τα περιβάλλοντα αυτά είναι τα εξής:

- SC - για τους 
- sqlContext - για ομάδα

Το πυρήνα PySpark παρέχει ορισμένα προκαθορισμένα "magics", που είναι ειδικές εντολές που μπορείτε να καλέσετε με %%. Υπάρχουν δύο αυτές οι εντολές που χρησιμοποιούνται σε αυτά τα δείγματα κώδικα.

- **%% τοπικό** Καθορίσει ότι ο κώδικας σε επακόλουθες γραμμές εκτελείται τοπικά. Κωδικός πρέπει να είναι έγκυρο Python κώδικα.
- **%% sql -o<variable name>** 
- Εκτελεί ένα ερώτημα Hive προς το sqlContext. Εάν η παράμετρος -o μεταβιβάζεται, το αποτέλεσμα του ερωτήματος είναι μόνιμα σε το %% τοπικό περιβάλλον Python ως μια dataframe Pandas.
 

Για περισσότερες πληροφορίες σχετικά με το πυρήνων για σημειωματάρια Jupyter και το προκαθορισμένο "magics" που παρέχουν, ανατρέξτε στο θέμα [πυρήνων που είναι διαθέσιμες για τα σημειωματάρια Jupyter με συμπλεγμάτων Linux τους HDInsight σε HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Ingest δεδομένων και να δημιουργήσετε ένα πλαίσιο καθαρισμός δεδομένων

Αυτή η ενότητα περιέχει τον κωδικό για μια σειρά εργασιών που απαιτούνται για να ingest τα δεδομένα που θα έχει βαθμολογηθεί. Ανάγνωση σε ένα συνδεδεμένο δείγμα 0,1% του ταξί ταξιδιού και ναύλο αρχείου (αποθηκεύονται ως αρχείο .tsv), μορφοποίηση των δεδομένων και, στη συνέχεια, δημιουργεί ένα πλαίσιο clean δεδομένων.

Τα αρχεία ταξί ταξιδιού και ναύλο που έχουν συνδεθεί με βάση τη διαδικασία που παρέχεται στην το: [της διαδικασίας Science ομάδας δεδομένων σε δράση: χρήση συμπλεγμάτων HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md) το θέμα.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 46.37 δευτερόλεπτα


## <a name="prepare-data-for-scoring-in-spark"></a>Προετοιμασία των δεδομένων για τη βαθμολογία στην τους 

Αυτή η ενότητα δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο, κωδικοποίηση και κλιμάκωση έλαβε δυνατότητες για να τα προετοιμάσετε για χρήση σε αλγορίθμους MLlib ελέγχεται εκμάθησης για την ταξινόμηση και παλινδρόμησης.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Δυνατότητα μετασχηματισμό: ευρετήριο και να κωδικοποιείτε έλαβε δυνατότητες για εισαγωγή δεδομένων σε μοντέλα για βαθμολογίας 

Αυτή η ενότητα δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο κατηγοριών δεδομένων με τη χρήση μιας `StringIndexer` και να κωδικοποιείτε δυνατότητες με `OneHotEncoder` εισαγωγής σε των μοντέλων.

Το [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) κωδικοποιεί μια στήλη συμβολοσειρά ετικετών σε μια στήλη δεικτών ετικέτας. Οι δείκτες είναι σύμφωνα με την ετικέτα συχνότητα. 

Το [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) αντιστοιχίζει μια στήλη δεικτών ετικέτα σε μια στήλη με δυαδικό φορέων, με πολύ ένα μόνο μία τιμή. Αυτήν την κωδικοποίηση επιτρέπει αλγορίθμους που αναμένετε συνεχής τιμών δυνατότητες, όπως εφοδιαστική παλινδρόμησης, για να εφαρμοστεί σε έλαβε δυνατότητες.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 5.37 δευτερόλεπτα


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Δημιουργία RDD αντικείμενα με δυνατότητα πίνακες για εισαγωγή δεδομένων σε μοντέλα

Αυτή η ενότητα περιέχει κώδικα που δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο δεδομένα έλαβε κειμένου ως αντικειμένου RDD και πρόσβασης μίας κωδικοποίηση, ώστε να μπορεί να χρησιμοποιηθεί για την εκπαίδευση και δοκιμή παλινδρόμησης εφοδιαστική MLlib και τα μοντέλα βάσει δέντρου. Το ευρετήριο τα δεδομένα αποθηκεύονται σε αντικείμενα [Είναι ανθεκτικά κατανεμημένο σύνολο δεδομένων (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) . Αυτά είναι τα βασικά αφαίρεσης στο τους. Ένα αντικείμενο RDD αντιπροσωπεύει μια συλλογή αμετάβλητες, διαμερίσματα των στοιχείων που μπορεί να λειτουργήσει σε παράλληλα με τους.

Περιέχει επίσης κώδικα που δείχνει πώς μπορείτε να περιορίσετε το μέγεθος των δεδομένων με το `StandardScalar` που παρέχεται από MLlib για χρήση σε γραμμική παλινδρόμηση με Stochastic διαβάθμισης καταγωγής (SGD), μια δημοφιλή αλγόριθμο για την εκπαίδευση ένα ευρύ φάσμα μηχανικής εκμάθησης μοντέλων. Το [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) χρησιμοποιείται για να κλιμακωθεί τις δυνατότητες, τη διακύμανση μονάδα. Η δυνατότητα κλίμακας, γνωστή και ως κανονικοποίηση δεδομένων, εξασφαλίζει ότι δυνατότητες με ευρέως εκταμιευθέντες τιμές έχουν δεν δεδομένη πλεονάζουσα αξιολογήσετε στη συνάρτηση στόχου. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 11.72 δευτερόλεπτα


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Βαθμολογία με το μοντέλο παλινδρόμησης εφοδιαστική και αποθήκευση εξόδου σε αντικειμένων blob

Ο κώδικας σε αυτήν την ενότητα εμφανίζει τον τρόπο για να φορτώσετε ένα εφοδιαστική μοντέλο παλινδρόμησης που έχει αποθηκευτεί στο χώρο αποθήκευσης αντικειμένων blob του Azure και χρησιμοποιήστε το για να πρόβλεψης ή όχι μια συμβουλή καταβάλλεται σε ταξίδι ταξί, το βαθμολογία με μετρικά βασική ταξινόμηση, και, στη συνέχεια, αποθήκευση και σχεδιάστε τα αποτελέσματα με το χώρο αποθήκευσης αντικειμένων blob. Τα αποτελέσματα scored αποθηκεύονται σε αντικείμενα RDD. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 19.22 δευτερόλεπτα


## <a name="score-a-linear-regression-model"></a>Βαθμολογία μοντέλου γραμμικής παλινδρόμησης

Χρησιμοποιήσαμε [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) να εκπαιδεύσετε μοντέλου γραμμικής παλινδρόμησης με Stochastic διαβάθμισης καταγωγής (SGD) για βελτιστοποίηση πρόβλεψη το ποσό που καταβλήθηκε συμβουλή. 

Ο κώδικας σε αυτήν την ενότητα δείχνει τον τρόπο Φόρτωση μοντέλου γραμμικής παλινδρόμησης από χώρο αποθήκευσης αντικειμένων blob του Azure, βαθμολογία χρήση μεταβλητών υπό κλίμακα και, στη συνέχεια, αποθηκεύστε τα αποτελέσματα πάλι το αντικείμενο blob.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 16.63 δευτερόλεπτα


## <a name="score-classification-and-regression-random-forest-models"></a>Βαθμολογία ταξινόμηση και παλινδρόμησης τυχαία μοντέλα δάσος

Ο κώδικας σε αυτήν την ενότητα παρουσιάζει τον τρόπο τοποθέτησης την αποθηκευμένη ταξινόμηση και παλινδρόμησης τυχαία μοντέλα δάσος αποθηκεύονται στο χώρο αποθήκευσης αντικειμένων blob του Azure, βαθμολογία τους απόδοσης με τυπική τάξης και παλινδρόμησης μετρήσεις και, στη συνέχεια, να αποθηκεύσετε τα αποτελέσματα ξανά στο χώρο αποθήκευσης αντικειμένων blob.

[Τυχαία συμπλεγμάτων δομών](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) είναι σύνολα των δέντρα αποφάσεων.  Συνδυάζουν πολλά δέντρα αποφάσεων για να μειώσετε τον κίνδυνο overfitting. Τυχαίες συμπλεγμάτων δομών μπορεί να διαχειριστεί έλαβε δυνατότητες, επεκτείνετε για τη ρύθμιση multiclass ταξινόμηση, δεν απαιτούν δυνατότητα κλίμακας και μπορούν να καταγράφουν μη γραμμικές αποκλίσεις και των δυνατοτήτων αλληλεπιδράσεις. Τυχαίες συμπλεγμάτων δομών είναι ένα από τα πιο επιτυχημένη μηχανικής εκμάθησης μοντέλων για ταξινόμηση και παλινδρόμησης.

[spark.mllib](http://spark.apache.org/mllib/) υποστηρίζει τυχαία συμπλεγμάτων δομών για δυαδικό και multiclass ταξινόμηση και για παλινδρόμησης, χρησιμοποιώντας τις δυνατότητες του τόσο συνεχής και κατηγοριών. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 31.07 δευτερόλεπτα


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Ταξινόμηση και παλινδρόμησης διαβάθμισης ενίσχυση δέντρου μοντέλα βαθμολογία

Ο κώδικας σε αυτήν την ενότητα εμφανίζει πώς να φορτώσετε ταξινόμηση και παλινδρόμησης διαβάθμισης ενίσχυση δέντρου μοντέλα από χώρο αποθήκευσης αντικειμένων blob του Azure, βαθμολογία τους απόδοσης με τυπική τάξης και μετρήσεις παλινδρόμησης και, στη συνέχεια, να αποθηκεύσετε τα αποτελέσματα ξανά στο χώρο αποθήκευσης αντικειμένων blob. 

**spark.mllib** υποστηρίζει GBTs για δυαδικό ταξινόμηση και για παλινδρόμησης, χρησιμοποιώντας τις δυνατότητες του τόσο συνεχής και κατηγοριών. 

[Δέντρα ενίσχυση της διαβάθμισης](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) είναι σύνολα των δέντρα αποφάσεων. GBTs εκπαίδευση δέντρα αποφάσεων επαναληπτική για την ελαχιστοποίηση της συνάρτησης απώλεια. GBTs μπορεί να διαχειριστεί έλαβε δυνατότητες δεν απαιτούν δυνατότητα κλίμακας και μπορούν να καταγράφουν μη γραμμικές αποκλίσεις και των δυνατοτήτων αλληλεπιδράσεις. Μπορεί επίσης να χρησιμοποιηθεί σε μια ρύθμιση multiclass ταξινόμηση.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 14.6 δευτερόλεπτα


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Εκκαθάριση αντικειμένων από τη μνήμη και εκτύπωση θέσεις αρχείων έχει βαθμολογηθεί

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**ΑΠΟΤΈΛΕΣΜΑ:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Εκμετάλλευση μοντέλα τους μέσω περιβάλλον web

Τους παρέχει ένα μηχανισμό για απομακρυσμένα υποβολή μαζικές εργασίες ή αλληλεπιδραστικών ερωτήματα μέσω ενός περιβάλλοντος εργασίας ΥΠΌΛΟΙΠΑ με ένα στοιχείο που ονομάζεται Λίβιος. Λίβιος είναι ενεργοποιημένη από προεπιλογή σε το σύμπλεγμά σας HDInsight τους. Για περισσότερες πληροφορίες σχετικά με Λίβιος ανατρέξτε στο θέμα: [υποβολή τους εργασίες από απόσταση χρησιμοποιώντας Λίβιος](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Μπορείτε να χρησιμοποιήσετε Λίβιος για απομακρυσμένα υποβάλετε μια εργασία που μαζική βαθμών που είναι ένα αρχείο που είναι αποθηκευμένο σε μια αντικειμένων blob του Azure και, στη συνέχεια, καταγράφει τα αποτελέσματα σε μια άλλη blob. Για να το κάνετε αυτό, μπορείτε να αποστείλετε τη δέσμη ενεργειών Python από  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) για να το blob του συμπλέγματος τους. Μπορείτε να χρησιμοποιήσετε ένα εργαλείο όπως το **Microsoft Azure αποθήκευσης Explorer** ή **AzCopy** για να αντιγράψετε τη δέσμη ενεργειών για το σύμπλεγμα blob. Σε περίπτωση μας θα σας που έχουν αποσταλεί στη δέσμη ενεργειών για να ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Τα πλήκτρα πρόσβασης που πρέπει να είναι βρέθηκε στην πύλη για το λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα τους. 


Μόλις αποσταλεί σε αυτήν τη θέση, αυτή η δέσμη ενεργειών εκτελείται μέσα στο σύμπλεγμα τους σε ένα περιβάλλον κατανέμεται. Το φορτώνει το μοντέλο και να εκτελέσετε προβλέψεων σε αρχεία εισόδου με βάση το μοντέλο.  

Μπορείτε να ενεργοποιήσετε αυτήν τη δέσμη ενεργειών από απόσταση, κάνοντας μια απλή αίτηση HTTPS/ΥΠΌΛΟΙΠΟ στην Λίβιος.  Ακολουθεί μια καμπύλη εντολή για να δημιουργήσετε μια αίτηση HTTP για να ενεργοποιήσετε τη δέσμη ενεργειών Python από απόσταση. Αντικαταστήστε CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME με τις κατάλληλες τιμές για το σύμπλεγμά σας τους.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Μπορείτε να χρησιμοποιήσετε οποιαδήποτε γλώσσα στο απομακρυσμένο σύστημα να καλέσει την εργασία τους μέσω Λίβιος, κάνοντας μια απλή κλήση HTTPS με βασικό έλεγχο ταυτότητας.   


>[AZURE.NOTE] Θα ήταν χρήσιμο να χρησιμοποιήσετε τη βιβλιοθήκη αιτήσεις Python κατά την πραγματοποίηση αυτής της κλήσης HTTP, αλλά αυτό δεν είναι εγκατεστημένο από προεπιλογή σε συναρτήσεις Azure. Έτσι παλαιότερων HTTP βιβλιοθήκες που χρησιμοποιούνται αντί για αυτό.   


Αυτός είναι ο κωδικός Python για την κλήση HTTP:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Μπορείτε επίσης να προσθέσετε αυτόν τον κωδικό Python [Azure συναρτήσεις](https://azure.microsoft.com/documentation/services/functions/) για να ενεργοποιήσετε μια υποβολή εργασία τους ότι βαθμών που είναι ένα blob που βασίζονται σε διάφορα συμβάντα όπως ένα χρονόμετρο, δημιουργία ή ενημέρωση ένα blob. 

Εάν προτιμάτε μια εμπειρία δωρεάν προγράμματος-πελάτη κώδικα, χρησιμοποιήστε τις [Εφαρμογές λογικής Azure](https://azure.microsoft.com/documentation/services/app-service/logic/) για να καλέσετε τη δέσμη τους βαθμολογίας ορίζοντας μια ενέργεια HTTP στην η **Λογική εφαρμογές σχεδίασης** και τη ρύθμιση παραμέτρων της. 

- Από το Azure πύλη, δημιουργήστε μια νέα εφαρμογή λογικής, επιλέγοντας **+ Δημιουργία** -> **Web + Mobile** -> **Λογική εφαρμογής**. 
- Για να εμφανίσετε το **Λογικής εφαρμογές σχεδίασης**, πληκτρολογήστε το όνομα της εφαρμογής λογικής και εφαρμογή υπηρεσίας πρόγραμμα.
- Επιλέξτε μια ενέργεια HTTP και εισαγάγετε τις παραμέτρους φαίνεται στην παρακάτω εικόνα:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Τι ακολουθεί; 

**Επικύρωση σταυρό και hyperparameter σάρωση**: ανατρέξτε στην ενότητα [Advanced Εξερεύνηση δεδομένων και μοντελοποίηση, με τους](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) σε πώς μπορεί να είναι τα μοντέλα εκπαίδευση χρησιμοποιώντας θανόντων σταυρό επικύρωσης και hyper-παράμετρο.

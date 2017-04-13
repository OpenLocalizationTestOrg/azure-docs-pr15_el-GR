<properties
    pageTitle="Εξερεύνηση δεδομένων και μοντελοποίηση, με τους | Microsoft Azure"
    description="Σάς δείχνουν τις δυνατότητες Εξερεύνηση και μοντελοποίηση δεδομένων του Κιτ εργαλείων MLlib τους."
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

# <a name="data-exploration-and-modeling-with-spark"></a>Εξερεύνηση δεδομένων και μοντελοποίηση, με τους

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Αυτόν τον Οδηγό χρησιμοποιεί HDInsight τους για να το κάνετε Εξερεύνηση δεδομένων και δυαδικό ταξινόμηση και παλινδρόμησης μοντελοποίησης εργασιών σε ένα δείγμα από τη νέα ΥΌΡΚΗ ταξί ταξιδιού και ναύλο 2013 συνόλου δεδομένων.  Σάς καθοδηγεί στα βήματα της [Διαδικασίας Science δεδομένων](http://aka.ms/datascienceprocess), για να ολοκληρωμένες, χρησιμοποιώντας μια τους HDInsight συμπλέγματος για επεξεργασία και αντικείμενα blob του Azure για να αποθηκεύσετε τα δεδομένα και τα μοντέλα. Η διαδικασία εξετάζει και απεικονίζει δεδομένα που έχουν μεταφερθεί από ένα αντικειμένων Blob του Azure χώρου αποθήκευσης και, στη συνέχεια, προετοιμάζει τα δεδομένα για να δημιουργήσετε μοντέλα πρόβλεψης. Αυτά τα μοντέλα είναι Δόμηση χρήση του Κιτ εργαλείων MLlib τους για να το κάνετε δυαδικό ταξινόμηση και παλινδρόμησης μοντελοποίηση εργασίες.

- Η εργασία **δυαδικό ταξινόμησης** είναι η πρόβλεψη ή όχι η πληρωμή μια συμβουλή για το ταξίδι σας. 
- Η εργασία **παλινδρόμησης** είναι η πρόβλεψη το ποσό του άκρου του που βασίζονται σε άλλες δυνατότητες συμβουλή. 

Τα μοντέλα χρησιμοποιούμε περιλαμβάνουν εφοδιαστική και γραμμικής παλινδρόμησης, τυχαία συμπλεγμάτων δομών και διαβάθμισης δέντρα ενισχύεται:

- [Γραμμική παλινδρόμηση με SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) είναι ένα μοντέλο γραμμικής παλινδρόμησης που χρησιμοποιεί μια μέθοδο Stochastic διαβάθμισης καταγωγής (SGD) και για βελτιστοποίηση και τη δυνατότητα κλιμάκωσης πρόβλεψη τα ποσά συμβουλή που καταβλήθηκε. 
- [Εφοδιαστική παλινδρόμησης με LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) ή "logit" παλινδρόμησης, είναι ένα μοντέλο παλινδρόμησης που μπορούν να χρησιμοποιηθούν όταν η μεταβλητή εξαρτώνται από είναι κατηγοριών για να κάνετε ταξινόμηση δεδομένων. LBFGS είναι ένας αλγόριθμος βελτιστοποίησης ημιπερίοδο Παπαδοπούλου που προσεγγίζει αλγόριθμο Broyden – Fletcher – Goldfarb – Shanno (BFGS) χρησιμοποιώντας ένα περιορισμένο χρονικό μνήμη του υπολογιστή και που χρησιμοποιείται ευρέως στο μηχανικής εκμάθησης.
- [Τυχαία συμπλεγμάτων δομών](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) είναι σύνολα των δέντρα αποφάσεων.  Συνδυάζουν πολλά δέντρα αποφάσεων για να μειώσετε τον κίνδυνο overfitting. Τυχαίες συμπλεγμάτων δομών που χρησιμοποιούνται για παλινδρόμησης και ταξινόμηση και μπορεί να διαχειριστεί έλαβε δυνατότητες και μπορεί να επεκταθεί για τη ρύθμιση multiclass ταξινόμηση. Δεν απαιτούν δυνατότητα κλίμακας και μπορούν να καταγράφουν μη γραμμικές αποκλίσεις και των δυνατοτήτων αλληλεπιδράσεις. Τυχαίες συμπλεγμάτων δομών είναι ένα από τα πιο επιτυχημένη μηχανικής εκμάθησης μοντέλων για ταξινόμηση και παλινδρόμησης.
- [Διαβάθμιση ενισχύεται δέντρα](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) είναι σύνολα των δέντρα αποφάσεων. GBTs εκπαίδευση δέντρα αποφάσεων επαναληπτική για την ελαχιστοποίηση της συνάρτησης απώλεια. GBTs χρησιμοποιούνται για παλινδρόμησης και ταξινόμηση και να χειριστείτε έλαβε δυνατότητες δεν απαιτούν δυνατότητα κλίμακας και μπορούν να καταγράφουν μη γραμμικές αποκλίσεις και τις αλληλεπιδράσεις των δυνατοτήτων. Μπορεί επίσης να χρησιμοποιηθεί σε μια ρύθμιση multiclass ταξινόμηση.

Τα βήματα μοντελοποίηση περιέχουν επίσης κώδικα που δείχνει πώς να εκπαιδεύσετε, αξιολόγηση και αποθήκευση κάθε τύπο του μοντέλου. Python έχει χρησιμοποιηθεί για να κώδικα της λύσης και για να εμφανίσετε το σχετικό σχεδιάσεων.   


>[AZURE.NOTE] Παρόλο που το Κιτ εργαλείων MLlib τους έχει σχεδιαστεί για λειτουργία σε μεγάλα σύνολα δεδομένων, ένα σχετικά μικρό δείγμα (με χρήση γραμμών 170K, περίπου 0,1% του αρχικού dataset νέα ΥΌΡΚΗ ~ 30 Mb) χρησιμοποιείται εδώ για τη διευκόλυνσή σας. Η άσκηση δίνονται εδώ εκτελείται αποτελεσματικά (σε 10 λεπτά περίπου) σε ένα σύμπλεγμα HDInsight με 2 κόμβους εργασίας. Τον ίδιο κωδικό, με μικρές τροποποιήσεις, μπορεί να χρησιμοποιηθεί για την επεξεργασία μεγαλύτερα σύνολα δεδομένων, με κατάλληλες τροποποιήσεις για προσωρινή αποθήκευση δεδομένων στη μνήμη και την αλλαγή του μεγέθους συμπλέγματος.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Χρειάζεστε ένα λογαριασμό Azure και ένα σύμπλεγμα χρειάζεστε ένα 1.6 HDInsight 3.4 τους HDInsight τους που για να ολοκληρώσετε αυτόν τον οδηγό. Δείτε την [Επισκόπηση των δεδομένων Science χρησιμοποιώντας τους σε Azure HDInsight](machine-learning-data-science-spark-overview.md) για οδηγίες σχετικά με τον τρόπο για να ικανοποιήσετε αυτές τις απαιτήσεις. Αυτό το θέμα περιέχει επίσης μια περιγραφή για τη νέα ΥΌΡΚΗ 2013 ταξί δεδομένα που χρησιμοποιούνται εδώ και οδηγίες σχετικά με τον τρόπο εκτέλεσης κώδικα από ένα σημειωματάριο Jupyter στο σύμπλεγμα τους. Το **pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** σημειωματάριο που περιέχει τα δείγματα κώδικα σε αυτό το θέμα είναι διαθέσιμη στο [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Πρόγραμμα εγκατάστασης: θέσεις αποθήκευσης, βιβλιοθήκες και το προκαθορισμένο περιβάλλον τους

Τους είναι σε θέση να διαβάζετε και να γράφετε σε Azure χώρο αποθήκευσης αντικειμένων Blob (γνωστό και ως WASB). Ώστε τα υπάρχοντα δεδομένα σας είναι αποθηκευμένα είναι δυνατή η επεξεργασία χρησιμοποιώντας τους και τα αποτελέσματα που είναι αποθηκευμένα ξανά σε WASB.

Για να αποθηκεύσετε μοντέλα ή αρχεία σε WASB, η διαδρομή πρέπει να έχει καθοριστεί σωστά. Το προεπιλεγμένο κοντέινερ που συνδέονται με το σύμπλεγμα τους μπορούν να χρησιμοποιηθούν χρησιμοποιώντας μια διαδρομή που ξεκινά με: "wasb: / / /". Άλλες θέσεις αναφέρονται από "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Ορισμός καταλόγου διαδρομές για θέσεις αποθήκευσης στο WASB

Το παρακάτω δείγμα κώδικα Καθορίζει τη θέση των δεδομένων για να διαβάσετε και τη διαδρομή για τον κατάλογο αποθήκευσης μοντέλο στην οποία είναι αποθηκευμένο το αποτέλεσμα του μοντέλου:


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Εισαγωγή βιβλιοθήκες

Ρύθμιση του απαιτεί επίσης εισαγωγή βιβλιοθήκες είναι απαραίτητο. Ορισμός περιβάλλοντος τους και να εισαγάγετε απαραίτητες βιβλιοθήκες με τον ακόλουθο κώδικα:


    # IMPORT LIBRARIES
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

Το πυρήνων PySpark που παρέχονται με σημειωματάρια Jupyter έχει ένα προκαθορισμένο περιβάλλον. Επομένως, δεν χρειάζεται να ορίσετε το τους ή αναπτύσσετε περιβάλλοντα Hive ρητά πριν να ξεκινήσετε να εργάζεστε με την εφαρμογή. Αυτά τα περιβάλλοντα είναι διαθέσιμες για εσάς από προεπιλογή. Τα περιβάλλοντα αυτά είναι τα εξής:

- SC - για τους 
- sqlContext - για ομάδα

Το πυρήνα PySpark παρέχει ορισμένα προκαθορισμένα "magics", που είναι ειδικές εντολές που μπορείτε να καλέσετε με %%. Υπάρχουν δύο αυτές οι εντολές που χρησιμοποιούνται σε αυτά τα δείγματα κώδικα.

- **%% τοπικό** Καθορίζει ότι ο κώδικας σε επακόλουθες γραμμές που θα εκτελεστεί τοπικά. Κωδικός πρέπει να είναι έγκυρο Python κώδικα.
- **%%sql -o <variable name>** Εκτελεί ένα ερώτημα Hive προς το sqlContext. Εάν η παράμετρος -o μεταβιβάζεται, το αποτέλεσμα του ερωτήματος είναι μόνιμα σε το %% τοπικό περιβάλλον Python ως μια DataFrame Pandas.
 

Για περισσότερες πληροφορίες σχετικά με το πυρήνων για σημειωματάρια Jupyter και το προκαθορισμένο "magics" που παρέχουν, ανατρέξτε στο θέμα [πυρήνων που είναι διαθέσιμες για τα σημειωματάρια Jupyter με συμπλεγμάτων Linux τους HDInsight σε HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>Κατάποσης δεδομένα από δημόσιες blob

Το πρώτο βήμα της διεργασίας science δεδομένων είναι να ingest τα δεδομένα για να αναλυθούν από πηγές όπου είναι βρίσκεται στο περιβάλλον Εξερεύνηση και μοντελοποίηση δεδομένων. Το περιβάλλον είναι τους σε αυτές τις οδηγίες. Αυτή η ενότητα περιέχει τον κωδικό για να ολοκληρώσετε μια σειρά από εργασίες:

- το δείγμα δεδομένων για να διαμορφωθεί ingest
- Διαβάστε στο εισαγωγής dataset (αποθηκεύονται ως αρχείο .tsv)
- μορφοποιήσετε και να εκκαθαρίσετε τα δεδομένα
- Δημιουργία και αντικείμενα (RDDs ή πλαισίων δεδομένων) στη μνήμη cache
- καταχωρήσετε ως έναν πίνακα temp στο περιβάλλον SQL.

Παρακάτω θα δείτε τον κώδικα για την κατάποση δεδομένων.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
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
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 51.72 δευτερόλεπτα


## <a name="data-exploration--visualization"></a>Εξερεύνηση δεδομένων και απεικόνισης

Όταν τα δεδομένα περιλαμβάνει έχουν μεταφερθεί στο τους, το επόμενο βήμα της διεργασίας science δεδομένων είναι να αποκτήσει βαθύτερη Κατανόηση των δεδομένων μέσω Εξερεύνηση και την απεικόνιση. Σε αυτήν την ενότητα, εξετάστε τα δεδομένα ταξί χρησιμοποιώντας ερωτήματα SQL και σχεδιάστε τις μεταβλητές προορισμού και πιθανούς δυνατότητες για οπτική επιθεώρηση. Συγκεκριμένα, σχεδιάστε τη συχνότητα καταμέτρηση επιβατών στο ταξί ταξίδια, της συχνότητας των συμβουλή ποσά και πώς συμβουλές διαφέρουν ανάλογα με την ποσότητα πληρωμής και τον τύπο.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Σχεδιάστε ένα ιστόγραμμα της συχνότητας count επιβατών σε το δείγμα του ταξί ταξίδια

Αυτό κώδικα και οι επόμενες τμήματα χρησιμοποιούν μαγικός SQL ερώτημα για το δείγμα και τοπικού μαγικός σχεδίασης των δεδομένων.

- **Μαγικός SQL (`%%sql`)** Το HDInsight PySpark πυρήνα υποστηρίζει ερωτήματα HiveQL εύκολο ενσωματωμένη σε σχέση με το sqlContext. Το (-o VARIABLE_NAME) όρισμα εξακολουθεί να εμφανίζεται το αποτέλεσμα του ερωτήματος SQL ως μια DataFrame Pandas στο διακομιστή Jupyter. Αυτό σημαίνει ότι είναι διαθέσιμη σε τοπική λειτουργία.
- Το ** `%%local` μαγικός** χρησιμοποιείται για την εκτέλεση κώδικα τοπικά στο διακομιστή Jupyter, που είναι η headnode του συμπλέγματος HDInsight. Συνήθως, χρησιμοποιείτε `%%local` Μαγικό σε συνδυασμό με το `%%sql` Μαγικό με την παράμετρο -o. Η παράμετρος -o θα παραμένει το αποτέλεσμα του ερωτήματος SQL τοπικά και, στη συνέχεια, %% τοπικό μαγικός θα έναυσμα στο επόμενο σύνολο τμήμα κώδικα για να εκτελέσετε τοπικά σε σχέση με το αποτέλεσμα της τα ερωτήματα SQL που έχει διατηρηθεί τοπικά

Το αποτέλεσμα είναι απεικονιστούν αυτόματα μετά την εκτέλεση του κώδικα.

Αυτό το ερώτημα ανακτά τα ταξίδια με επιβατηγά count. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Αυτός ο κώδικας δημιουργεί ένα τοπικό-πλαίσιο δεδομένων από το αποτέλεσμα του ερωτήματος και σχεδιάζει τα δεδομένα. Το `%%local` μαγικός δημιουργεί ένα τοπικό δεδομένων-πλαίσιο, `sqlResults`, που μπορούν να χρησιμοποιηθούν για τη σχεδίαση με matplotlib. 

>[AZURE.NOTE] Αυτό μαγικός PySpark χρησιμοποιείται πολλές φορές σε αυτές τις οδηγίες. Εάν η ποσότητα των δεδομένων είναι μεγάλο, θα πρέπει να δείγματος για να δημιουργήσετε ένα πλαίσιο δεδομένων που μπορεί να χωρέσει στο τοπικής μνήμης.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Εδώ είναι ο κωδικός για να απεικονίσετε τα ταξίδια με καταμετρά επιβατών

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**ΑΠΟΤΈΛΕΣΜΑ:**

![Συχνότητα αποστολής και επιστροφής από επιβατών μέτρηση](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Μπορείτε να επιλέξετε ανάμεσα σε πολλούς διαφορετικούς τύπους από απεικονίσεις (πίνακα, πίτας, γραμμής, περιοχή ή ράβδων), χρησιμοποιώντας τα κουμπιά μενού **Τύπος** στο Σημειωματάριο. Η γραμμή σχεδίασης, εμφανίζεται εδώ.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Σχεδιάστε ένα ιστόγραμμα συμβουλή ποσά και πώς συμβουλή ποσότητα διαφέρει από επιβατών count και ναύλο ποσά.

Χρησιμοποιήστε ένα ερώτημα SQL για το δείγμα δεδομένων.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


Αυτό το κελί κώδικα χρησιμοποιεί το ερώτημα SQL για να δημιουργήσετε τρεις σχεδιάσεων τα δεδομένα.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**ΑΠΟΤΈΛΕΣΜΑ:** 

![Διανομή ποσού συμβουλή](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Συμβουλή ποσό από επιβατών μέτρηση](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Συμβουλή ποσό κατά τιμή ναύλο](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Δυνατότητα μηχανικής, το μετασχηματισμό και δεδομένων προετοιμασίας για μοντελοποίηση
Αυτή η ενότητα περιγράφει και παρέχει τον κωδικό για τις διαδικασίες που χρησιμοποιούνται για την προετοιμασία δεδομένων για χρήση σε ML μοντελοποίησης. Αυτό δείχνει πώς μπορείτε να κάνετε τις ακόλουθες εργασίες:

- Δημιουργία μια νέα δυνατότητα με binning ώρες σε κίνηση Κάδοι χρόνου
- Δημιουργία ευρετηρίου και να κωδικοποιείτε έλαβε δυνατότητες
- Δημιουργία αντικειμένων με ετικέτες σημείο εισαγωγής σε συναρτήσεις ML
- Δημιουργήσετε μια τυχαία δευτερευόντων δειγματοληψία των δεδομένων και τη διαιρέσετε σε εκπαίδευση και έλεγχος σύνολα
- Η δυνατότητα κλιμάκωση
- Cache αντικειμένων στη μνήμη


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Δημιουργία μια νέα δυνατότητα με binning ώρες σε κίνηση Κάδοι χρόνου

Αυτός ο κώδικας εμφανίζει τον τρόπο για να δημιουργήσετε μια νέα δυνατότητα με binning ώρες σε κίνηση Κάδοι ώρα και, στη συνέχεια, στο πλαίσιο που προκύπτει δεδομένων στη μνήμη cache. Όταν είναι ανθεκτικά κατανεμημένο σύνολα δεδομένων (RDDs) και πλαισίων δεδομένων χρησιμοποιούνται επανειλημμένα, σε cache οδηγεί σε βελτιωμένη χρόνους εκτέλεσης. Ως εκ τούτου, θα σας cache RDDs και πλαισίων δεδομένων σε πολλά στάδια της αναλυτικής παρουσίασης. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**ΑΠΟΤΈΛΕΣΜΑ:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Δημιουργία ευρετηρίου και να κωδικοποιείτε έλαβε δυνατότητες για εισαγωγή δεδομένων σε μοντελοποίησης συναρτήσεις

Αυτή η ενότητα δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο ή κωδικοποίηση έλαβε δυνατότητες για εισαγωγή δεδομένων σε τις συναρτήσεις μοντελοποίησης. Η μοντελοποίηση και πρόβλεψης συναρτήσεις MLlib απαιτεί δυνατότητες με έλαβε εισαγωγής δεδομένων με ευρετήριο ή κωδικοποιημένη πριν από τη χρήση. Ανάλογα με το μοντέλο, πρέπει να δημιουργήσετε ευρετήριο ή να κωδικοποιήσετε τους με διάφορους τρόπους:  

- **Βασίζεται στο δέντρο μοντελοποίηση** απαιτεί κατηγορίες να κωδικοποιείται ως αριθμητικές τιμές (για παράδειγμα, μια δυνατότητα με τρεις κατηγορίες μπορεί να είναι κωδικοποιημένη με 0, 1, 2). Παρέχεται από τη συνάρτηση [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) του MLlib. Αυτή η συνάρτηση κωδικοποιεί μια στήλη συμβολοσειρά ετικετών σε μια στήλη δεικτών ετικέτας που είναι σύμφωνα με την ετικέτα συχνότητα. Παρόλο που στο ευρετήριο με αριθμητικές τιμές για είσοδο και χειρισμού δεδομένων, τους αλγορίθμους που βασίζεται στο δέντρο μπορεί να οριστεί για τα χειριστεί σωστά ως κατηγορίες. 

- **Μοντέλα Logistic και γραμμική παλινδρόμηση** απαιτείται πρόσβασης μίας κωδικοποίηση, όπου, για παράδειγμα, μια δυνατότητα με τρεις κατηγορίες μπορούν να αναπτυχθούν σε τρεις στήλες τη δυνατότητα, με κάθε περιέχει 0 ή 1, ανάλογα με την κατηγορία μια παρακολούθηση. MLlib παρέχει [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) συνάρτηση για να κάνετε μία ζεστού κωδικοποίηση. Αυτό encoder αντιστοιχίζει μια στήλη δεικτών ετικέτα σε μια στήλη με δυαδικό φορέων, με πολύ ένα μόνο μία τιμή. Αυτήν την κωδικοποίηση επιτρέπει αλγορίθμους που περιμένατε αριθμητικών τιμών δυνατότητες, όπως εφοδιαστική παλινδρόμησης, για να εφαρμοστεί σε έλαβε δυνατότητες.

Ακολουθεί ο κώδικας για να δημιουργήσετε ευρετήριο και να κωδικοποιείτε έλαβε δυνατότητες:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 1,28 δευτερόλεπτα

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Δημιουργία αντικειμένων με ετικέτες σημείο εισαγωγής σε συναρτήσεις ML

Αυτή η ενότητα περιέχει κώδικα που δείχνει πώς μπορείτε να index δεδομένα έλαβε κειμένου ως τύπο δεδομένων με ετικέτες σημείο και να κωδικοποιείτε το ώστε να μπορεί να χρησιμοποιηθεί για την εκπαίδευση και δοκιμή παλινδρόμησης εφοδιαστική MLlib και άλλα μοντέλα κατάταξης. Τα αντικείμενα με ετικέτες σημείο είναι είναι ανθεκτικά κατανεμημένο σύνολα δεδομένων (RDD) έχει μορφοποιηθεί με τον τρόπο που είναι απαραίτητη ως εισαγωγής δεδομένων από το μεγαλύτερο μέρος ML αλγόριθμους στο MLlib. Μια [ετικέτα σημείο](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) είναι ένα τοπικό ανύσματος, είτε πυκνό ή κατακερματισμένο, που σχετίζεται με μια ετικέτα/απάντηση.  

Αυτή η ενότητα περιέχει κώδικα που δείχνει πώς μπορείτε να δημιουργήσετε ευρετήριο δεδομένα έλαβε κειμένου ως τύπο δεδομένων [με την ετικέτα σημείο](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) και να κωδικοποιείτε το ώστε να μπορεί να χρησιμοποιηθεί για την εκπαίδευση και δοκιμή παλινδρόμησης εφοδιαστική MLlib και άλλα μοντέλα κατάταξης. Τα αντικείμενα με ετικέτες σημείο είναι είναι ανθεκτικά κατανεμημένο σύνολα δεδομένων (RDD) που αποτελείται από μια ετικέτα (προορισμού/απόκρισης μεταβλητή) και τη δυνατότητα άνυσμα. Αυτή η μορφή απαιτείται ως είσοδο από πολλά ML αλγόριθμους στο MLlib.

Παρακάτω θα δείτε τον κώδικα για να δημιουργήσετε ευρετήριο και να κωδικοποιείτε δυνατότητες κειμένου για δυαδικό ταξινόμηση.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Παρακάτω θα δείτε τον κώδικα για την κωδικοποίηση και να δημιουργήσετε ευρετήριο δυνατότητες έλαβε κειμένου για την ανάλυση γραμμικής παλινδρόμησης.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Δημιουργήστε μια δευτερεύουσα τυχαία δειγματοληψία των δεδομένων και τη διαιρέσετε σε εκπαίδευση και έλεγχος σύνολα

Αυτός ο κώδικας δημιουργεί μια τυχαία δειγματοληψία των δεδομένων (25% χρησιμοποιείται εδώ). Παρόλο που δεν είναι απαραίτητη για αυτό το παράδειγμα λόγω του μεγέθους της του συνόλου δεδομένων, θα σας δείχνουν πώς μπορείτε να λάβετε δείγματα εδώ, ώστε να γνωρίζετε πώς μπορείτε να το χρησιμοποιήσετε για τη δική σας πρόβλημα όταν είναι απαραίτητο. Όταν δείγματα είναι μεγάλο, αυτό μπορεί να εξοικονομήσει αρκετό χρόνο κατά την εκπαίδευση μοντέλα. Στη συνέχεια θα σας διαιρέστε το δείγμα σε ένα τμήμα εκπαίδευση (75% εδώ) και δοκιμών τμήμα (25% εδώ) για να χρησιμοποιήσετε στην ταξινόμηση και μοντελοποίηση παλινδρόμησης.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 0,24 δευτερόλεπτα


### <a name="feature-scaling"></a>Η δυνατότητα κλιμάκωση

Η δυνατότητα κλίμακας, γνωστή και ως κανονικοποίηση δεδομένων, εξασφαλίζει ότι δυνατότητες με ευρέως εκταμιευθέντες τιμές έχουν δεν δεδομένη πλεονάζουσα αξιολογήσετε στη συνάρτηση στόχου. Ο κώδικας για κλιμάκωση δυνατότητα χρησιμοποιεί το [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) για να κλιμακωθεί τις δυνατότητες, τη διακύμανση μονάδα. Παρέχεται από MLlib για χρήση σε γραμμική παλινδρόμηση με Stochastic διαβάθμισης καταγωγής (SGD), μια δημοφιλή αλγόριθμο για την εκπαίδευση μια ευρεία ποικιλία άλλων μηχανικής εκμάθησης μοντέλων όπως regularized παλινδρομήσεις ή υποστήριξη ανύσματος μηχανές (SVM).

>[AZURE.NOTE] Εντοπίσαμε αλγόριθμο LinearRegressionWithSGD ευαίσθητες θα εμφανίζονται κλίμακας.

Παρακάτω θα δείτε τον κώδικα σε κλίμακα μεταβλητές για χρήση με regularized γραμμικής SGD αλγόριθμο.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Χρόνος που να εκτελέσει επάνω από το κελί: 13.17 δευτερόλεπτα


### <a name="cache-objects-in-memory"></a>Cache αντικειμένων στη μνήμη

Την ώρα που λαμβάνονται για την εκπαίδευση και έλεγχος των αλγορίθμων ML μπορεί να μειωθεί από την προσωρινή αποθήκευση στο πλαίσιο εισαγωγής δεδομένων αντικείμενα χρησιμοποιούνται για την ταξινόμηση, παλινδρόμησης, και κλιμάκωση δυνατότητες.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:** 

Χρόνος που να εκτελέσει επάνω από το κελί: 0,15 δευτερόλεπτα


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Πρόβλεψης ή όχι η πληρωμή συμβουλή με τα μοντέλα δυαδικό ταξινόμηση

Αυτή η ενότητα δείχνει τον τρόπο χρήσης τρία μοντέλα για την εργασία δυαδικό ταξινόμηση της πρόβλεψης των ή όχι η πληρωμή μια συμβουλή για ταξίδι ταξί. Τα μοντέλα που παρουσιάζονται είναι:

- Regularized εφοδιαστική παλινδρόμησης 
- Μοντέλο τυχαία δάσος
- Δέντρα ενίσχυση της διαβάθμισης

Κάθε μοντέλο Δημιουργία ενότητας κώδικας χωρίζεται σε βήματα: 

1. **Εκπαίδευση μοντέλου** δεδομένων με ένα σύνολο παραμέτρου
2. **Αξιολόγηση μοντέλου** σε ένα σύνολο δεδομένων δοκιμής με μετρικά
3. **Αποθήκευση μοντέλου** σε blob για μελλοντική κατανάλωση

### <a name="classification-using-logistic-regression"></a>Ταξινόμηση με χρήση εφοδιαστική παλινδρόμησης

Ο κώδικας σε αυτήν την ενότητα δείχνει πώς μπορείτε να εκπαιδεύσετε, αξιολόγηση και αποθήκευση ενός μοντέλου εφοδιαστική παλινδρόμησης με [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) που προβλέπει ή όχι μια συμβουλή η πληρωμή ενός ταξιδιού στο τη νέα ΥΌΡΚΗ ταξί ταξιδιού και ναύλο συνόλου δεδομένων.

**Εκπαίδευση του μοντέλου εφοδιαστική παλινδρόμησης χρησιμοποιώντας ΔΚ και hyperparameter θανόντων**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ΑΠΟΤΈΛΕΣΜΑ:** 

Συντελεστές: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Σημείο τομής:-0.0111216486893

Χρόνος που να εκτελέσει επάνω από το κελί: 14.43 δευτερόλεπτα

**Αξιολόγηση του μοντέλου δυαδικό ταξινόμηση με τυπικές μετρήσεις**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ΑΠΟΤΈΛΕΣΜΑ:** 

Περιοχή κάτω από την ΤΙΜΉ = 0.985297691373

Περιοχή ROC = 0.983714670256

Στατιστικά στοιχεία σύνοψης

Ακρίβεια = 0.984304060189

Ανάκληση = 0.984304060189

F1 Βαθμολογία = 0.984304060189

Χρόνος που να εκτελέσει επάνω από το κελί: 57.61 δευτερόλεπτα

**Σχεδίαση καμπύλης ROC.**

Το *predictionAndLabelsDF* έχει καταχωρηθεί ως πίνακα, *tmp_results*, στο προηγούμενο κελί. *tmp_results* μπορεί να χρησιμοποιηθεί για να κάνετε ερωτήματα και αποτελέσματα εξόδου στο πλαίσιο sqlResults δεδομένων για τη σχεδίαση. Παρακάτω θα δείτε τον κώδικα.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Παρακάτω θα δείτε τον κώδικα για να κάνετε προβλέψεις και να σχεδιάσετε την καμπύλη ROC.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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
    

**ΑΠΟΤΈΛΕΣΜΑ:**

![Curve.png ROC εφοδιαστική παλινδρόμησης](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Ταξινόμηση τυχαία δάσος

Ο κώδικας σε αυτήν την ενότητα δείχνει πώς μπορείτε να εκπαιδεύσετε, αξιολόγηση και αποθήκευση ενός μοντέλου τυχαία δάσος που προβλέπει ή όχι μια συμβουλή η πληρωμή ενός ταξιδιού στο τη νέα ΥΌΡΚΗ ταξί ταξιδιού και ναύλο συνόλου δεδομένων.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Περιοχή ROC = 0.985297691373

Χρόνος που να εκτελέσει επάνω από το κελί: 31.09 δευτερόλεπτα


### <a name="gradient-boosting-trees-classification"></a>Ταξινόμηση δέντρα ενίσχυση της διαβάθμισης

Ο κώδικας σε αυτήν την ενότητα δείχνει πώς μπορείτε να εκπαιδεύσετε, υπολογιστεί, και να αποθηκεύσετε διαβάθμισης boosting δέντρα μοντέλο που προβλέπει ή όχι η πληρωμή συμβουλή για ταξίδι στο του ταξιδιού ταξί νέα ΥΌΡΚΗ και ναύλο συνόλου δεδομένων.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ΑΠΟΤΈΛΕΣΜΑ:**

Περιοχή ROC = 0.985297691373

Χρόνος που να εκτελέσει επάνω από το κελί: 19.76 δευτερόλεπτα


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Πρόβλεψη ποσά συμβουλή για ταξίδια ταξί με τα μοντέλα παλινδρόμησης

Αυτή η ενότητα δείχνει τον τρόπο χρήσης τρία μοντέλα για την εργασία παλινδρόμησης της πρόβλεψης το ποσό του άκρου των που καταβλήθηκε για ταξίδι ταξί που βασίζονται σε άλλες δυνατότητες συμβουλή. Τα μοντέλα που παρουσιάζονται είναι:

- Regularized γραμμικής παλινδρόμησης
- Τυχαίες δάσος
- Δέντρα ενίσχυση της διαβάθμισης

Αυτά τα μοντέλα έχουν περιγράφεται στην εισαγωγή. Κάθε μοντέλο Δημιουργία ενότητας κώδικας χωρίζεται σε βήματα: 

1. **Εκπαίδευση μοντέλου** δεδομένων με ένα σύνολο παραμέτρου
2. **Αξιολόγηση μοντέλου** σε ένα σύνολο δεδομένων δοκιμής με μετρικά
3. **Αποθήκευση μοντέλου** σε blob για μελλοντική κατανάλωση

### <a name="linear-regression-with-sgd"></a>Γραμμική παλινδρόμηση με SGD 

Ο κώδικας σε αυτήν την ενότητα εμφανίζει τον τρόπο χρήσης υπό κλίμακα δυνατοτήτων για να εκπαιδεύσετε μια γραμμική παλινδρόμηση που χρησιμοποιεί stochastic διαβάθμισης καταγωγής (SGD) για βελτιστοποίηση και πώς να βαθμολογία, αξιολόγηση και αποθηκεύστε το μοντέλο στο χώρο αποθήκευσης αντικειμένων Blob Azure (WASB).

>[AZURE.TIP] Στην εμπειρία μας, μπορεί να υπάρξουν θέματα με το σύγκλισης μοντέλων LinearRegressionWithSGD και παράμετροι πρέπει να είναι αλλάξει/βελτιστοποιημένη προσεκτικά για τη λήψη ένα έγκυρο μοντέλο. Κλιμάκωσης των μεταβλητών σημαντικά βοηθούν στην σύγκλισης. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

Συντελεστές: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

INTERCEPT: 0.853872718283

RMSE = 1.24190115863

R sqr = 0.608017146081

Χρόνος που να εκτελέσει επάνω από το κελί: 58.42 δευτερόλεπτα


### <a name="random-forest-regression"></a>Τυχαία δάσος παλινδρόμησης

Ο κώδικας σε αυτήν την ενότητα δείχνει πώς μπορείτε να εκπαιδεύσετε, αξιολόγηση και αποθήκευση παλινδρόμηση τυχαία δάσος που προβλέπει ποσό συμβουλή για τα δεδομένα ταξιδιού ταξί νέα ΥΌΡΚΗ.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

RMSE = 0.891209218139

R sqr = 0.759661334921

Χρόνος που να εκτελέσει επάνω από το κελί: 49.21 δευτερόλεπτα


### <a name="gradient-boosting-trees-regression"></a>Παλινδρόμησης δέντρα ενίσχυση της διαβάθμισης

Ο κώδικας σε αυτήν την ενότητα δείχνει πώς μπορείτε να εκπαιδεύσετε, αξιολόγηση και αποθήκευση διαβαθμίσεις boosting δέντρα μοντέλο που προβλέπει ποσό συμβουλή για τα δεδομένα ταξιδιού ταξί νέα ΥΌΡΚΗ.

**Εκπαίδευση και αξιολόγηση**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ΑΠΟΤΈΛΕΣΜΑ:**

RMSE = 0.908473148639

R sqr = 0.753835096681

Χρόνος που να εκτελέσει επάνω από το κελί: 34.52 δευτερόλεπτα

**Σχεδίαση**

*tmp_results* έχει καταχωρηθεί ως ομάδα πίνακα στο προηγούμενο κελί. Αποτελέσματα από τον πίνακα είναι εξόδου στο *sqlResults* δεδομένων-πλαίσιο για τη σχεδίαση. Εδώ είναι ο κωδικός

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Παρακάτω θα δείτε τον κώδικα για να απεικονίσετε αυτά τα δεδομένα με το διακομιστή Jupyter.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**ΑΠΟΤΈΛΕΣΜΑ:**

![Πραγματική-σύγκριση-προβλεπόμενης-συμβουλή-ποσά](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Εκκαθάριση αντικειμένων από τη μνήμη

Χρήση `unpersist()` για να διαγράψετε αντικείμενα προσωρινά στη μνήμη.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Θέσεις αποθήκευσης εγγραφής από τα μοντέλα για κατανάλωση και βαθμολογίας

Για να εκμετάλλευση και βαθμολογία μια ανεξάρτητη συνόλου δεδομένων που περιγράφονται σε το [βαθμολογία και την αξιολόγησή τους ενσωματωμένη μηχανικής εκμάθησης μοντέλων](machine-learning-data-science-spark-model-consumption.md) θέμα, πρέπει να αντιγράψετε και να επικολλήσετε αυτά τα ονόματα αρχείων που περιέχει το αποθηκευμένο μοντέλα δημιουργούνται εδώ στο Σημειωματάριο Jupyter κατανάλωση. Παρακάτω θα δείτε τον κώδικα για να εκτυπώσετε τις διαδρομές σε μοντέλο αρχεία που χρειάζεστε.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**ΕΞΌΔΟΥ**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Τι ακολουθεί;

Τώρα που έχετε δημιουργήσει μοντέλα παλινδρόμησης και ταξινόμηση με τη MlLib τους, είστε έτοιμοι για να μάθετε πώς να βαθμολογία και να αξιολογήσετε αυτές τις μοντέλα. Η Εξερεύνηση δεδομένων για προχωρημένους και μοντελοποίησης Σημειωματάριο dives βαθύτερη σε σταυρό επικύρωσης, hyper-παραμέτρου σάρωση, συμπεριλαμβανομένων και του μοντέλου αξιολόγησης. 

**Του μοντέλου κατανάλωση:** Για να μάθετε πώς να βαθμολογία και να αξιολογήσετε την ταξινόμηση και παλινδρόμησης μοντέλων που έχουν δημιουργηθεί σε αυτό το θέμα, ανατρέξτε στο θέμα [βαθμολογία και την αξιολόγησή τους ενσωματωμένη μηχανικής εκμάθησης μοντέλων](machine-learning-data-science-spark-model-consumption.md).

**Επικύρωση σταυρό και hyperparameter σάρωση**: ανατρέξτε στην ενότητα [Advanced Εξερεύνηση δεδομένων και μοντελοποίηση, με τους](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) σε πώς μπορεί να είναι τα μοντέλα εκπαίδευση χρησιμοποιώντας σταυρό επικύρωσης και hyper-παράμετρο θανόντων




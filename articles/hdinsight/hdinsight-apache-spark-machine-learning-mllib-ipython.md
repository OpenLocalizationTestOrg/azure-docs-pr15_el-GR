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


# <a name="machine-learning-predictive-analysis-on-food-inspection-data-using-mllib-with-apache-spark-cluster-on-hdinsight-linux"></a>Υπολογιστή εκμάθησης: πρόβλεψης ανάλυση σε τρόφιμα επιθεώρησης δεδομένων με τη χρήση MLlib με τους Apache σύμπλεγμα σε HDInsight Linux

> [AZURE.TIP] Αυτό το πρόγραμμα εκμάθησης είναι επίσης διαθέσιμη ως Jupyter σημειωματαρίου σε ένα σύμπλεγμα τους (Linux) που δημιουργείτε στο HDInsight. Η εμπειρία σημειωματαρίου σας επιτρέπει να εκτελέσετε τα τμήματα κώδικα Python από το Σημειωματάριο ίδια. Για να εκτελέσετε το πρόγραμμα εκμάθησης από μέσα σε ένα σημειωματάριο, δημιουργήστε ένα σύμπλεγμα τους, εκκίνηση ενός σημειωματαρίου Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), και, στη συνέχεια, εκτελέστε το Σημειωματάριο **τους μηχανικής εκμάθησης - πρόβλεψης ανάλυση σε τρόφιμα επιθεώρησης δεδομένων χρησιμοποιώντας MLLib.ipynb** κάτω από το φάκελο **Python** .


Σε αυτό το άρθρο παρουσιάζει τον τρόπο χρήσης **MLLib**, του τους ενσωματωμένη μηχανικής εκμάθησης βιβλιοθήκες, για να εκτελέσετε μια απλή ανάλυση πρόβλεψης σε ένα ανοιχτό σύνολο δεδομένων. MLLib είναι μια βιβλιοθήκη τους πυρήνα που παρέχει πολλά βοηθητικά προγράμματα που είναι χρήσιμες για μηχανικής εκμάθησης εργασίες, συμπεριλαμβανομένων των βοηθητικών προγραμμάτων που είναι κατάλληλη για:

* Ταξινόμηση

* Παλινδρόμησης

* Δημιουργία συμπλέγματος

* Μοντελοποίηση θέμα

* Αποδόμησης τιμή στον ενικό (ΦΝΧ) και ανάλυση κεφαλαίου στοιχείου (ΣΕΣΣ)

* Υπόθεση δοκιμή και τον υπολογισμό στατιστικών στοιχείων δείγματος

Σε αυτό το άρθρο παρουσιάζει μια απλή προσέγγιση *κατάταξη* μέσω εφοδιαστική παλινδρόμησης.

## <a name="what-are-classification-and-logistic-regression"></a>Τι είναι η ταξινόμηση και εφοδιαστική παλινδρόμησης;

*Ταξινόμηση*, μια συνηθισμένη μηχανικής εκμάθησης εργασίας, είναι η διαδικασία ταξινόμησης δεδομένα εισόδου σε κατηγορίες. Είναι η δουλειά έναν αλγόριθμο ταξινόμησης για να υπολογίσετε τον τρόπο για να αντιστοιχίσετε "ετικέτες" για να εισαγάγετε δεδομένα που παρέχετε. Για παράδειγμα, μπορεί να πιστεύετε ότι έναν αλγόριθμο εκμάθησης υπολογιστή που αποδέχεται μετοχών πληροφορίες ως είσοδο και διαιρεί το απόθεμα σε δύο κατηγορίες: αποθεμάτων που θα πρέπει να εμπορεύεστε και αποθεμάτων που θα πρέπει να διατηρήσετε.

Εφοδιαστική παλινδρόμησης είναι ο αλγόριθμος που χρησιμοποιείτε για την ταξινόμηση. Του τους εφοδιαστική παλινδρόμησης API είναι χρήσιμη για *δυαδικό ταξινόμησης*ή ταξινόμηση των δεδομένων εισόδου σε μία από τις δύο ομάδες. Για περισσότερες πληροφορίες σχετικά με την εφοδιαστική παλινδρομήσεις, ανατρέξτε στο θέμα [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Εν κατακλείδι, η διαδικασία εφοδιαστική παλινδρόμησης υπολογίζονται μια *συνάρτηση εφοδιαστική* που μπορούν να χρησιμοποιηθούν για την πρόβλεψη την πιθανότητα ότι ένα άνυσμα εισαγωγής ανήκει σε μία ομάδα ή το άλλο.  

## <a name="what-are-we-trying-to-accomplish-in-this-article"></a>Τι θα σας προσπαθείτε να πραγματοποιήσετε σε αυτό το άρθρο;

Θα χρησιμοποιήσετε τους για να εκτελέσετε ορισμένες πρόβλεψης ανάλυσης σε τρόφιμα επιθεώρησης δεδομένων (**Food_Inspections1.csv**) που αποκτήθηκε μέσω της [πύλης δεδομένων πόλη του Σικάγο](https://data.cityofchicago.org/). Σε αυτό το σύνολο δεδομένων περιέχει πληροφορίες σχετικά με τρόφιμα ελέγχους που έχουν εκτελούνται στο Σικάγο, συμπεριλαμβανομένων πληροφοριών σχετικά με κάθε εγκατάσταση τροφίμων που έχει ελεγχθεί, τις παραβιάσεις που βρέθηκαν (εάν υπάρχουν) και τα αποτελέσματα του ελέγχου. Το αρχείο δεδομένων του CSV ήδη είναι διαθέσιμη στο λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα στο **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

Στα παρακάτω βήματα, μπορείτε να αναπτύξετε ένα μοντέλο για να δείτε τι που χρειάζεται για την επιτυχία ή αποτυχία έναν έλεγχο τρόφιμα. 

## <a name="start-building-a-machine-learning-application-using-spark-mllib"></a>Ξεκινήστε τη δημιουργία μιας εφαρμογής εκμάθησης υπολογιστή χρησιμοποιώντας τους MLlib

1. Από την [Πύλη Azure](https://portal.azure.com/), από την startboard, κάντε κλικ στο πλακίδιο για το σύμπλεγμα τους (εάν καρφιτσωμένα αυτό για να το startboard). Μπορείτε επίσης να μεταβείτε σε το σύμπλεγμά σας στην περιοχή **Αναζήτηση όλων** > **Συμπλεγμάτων HDInsight**.   

2. Από το σύμπλεγμα blade τους, κάντε κλικ στην επιλογή **Πίνακας εργαλείων σύμπλεγμα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Jupyter Σημειωματάριο**. Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια διαχειριστή για το σύμπλεγμα.

    > [AZURE.NOTE] Μπορείτε επίσης μπορεί να φτάσει στο Σημειωματάριο Jupyter για το σύμπλεγμά σας ανοίγοντας την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Δημιουργήστε ένα νέο σημειωματάριο. Κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **PySpark**.

    ![Δημιουργία νέου σημειωματαρίου Jupyter] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.createnotebook.png "Δημιουργία νέου σημειωματαρίου Jupyter")

3. Δημιουργείται ένα νέο σημειωματάριο και ανοίγει με το όνομα Untitled.pynb. Κάντε κλικ στο όνομα του σημειωματαρίου στο επάνω μέρος και πληκτρολογήστε ένα φιλικό όνομα.

    ![Παροχή ένα όνομα για το Σημειωματάριο] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/hdispark.note.jupyter.notebook.name.png "Παροχή ένα όνομα για το Σημειωματάριο")

3. Επειδή έχετε δημιουργήσει ένα σημειωματάριο χρησιμοποιώντας το PySpark πυρήνα, δεν χρειάζεται να δημιουργήσετε οποιαδήποτε περιβάλλοντα ρητά. Τα περιβάλλοντα τους και η ομάδα θα δημιουργηθεί αυτόματα για εσάς όταν εκτελείτε το πρώτο κελί κώδικα. Μπορείτε να ξεκινήσετε τη δημιουργία του μηχανικής εκμάθησης εφαρμογής με την εισαγωγή των τύπων που απαιτείται για αυτό το σενάριο. Για να το κάνετε, τοποθετήστε το δρομέα στο κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.


        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Δομή μιας dataframe εισαγωγής

Μπορούμε να χρησιμοποιήσουμε `sqlContext` για να εκτελέσετε μετασχηματισμούς σε δομημένα δεδομένα. Η πρώτη εργασία είναι για να φορτώσετε το δείγμα δεδομένων ((**Food_Inspections1.csv**)) σε έναν SQL τους *dataframe*. 

1. Επειδή τα ανεπεξέργαστα δεδομένα σε μορφή CSV, πρέπει να χρησιμοποιήσετε το περιβάλλον τους για να αποσπάσετε κάθε γραμμή του αρχείου στη μνήμη ως μη δομημένα κείμενο. στη συνέχεια, μπορείτε να χρησιμοποιήσετε του Python CSV βιβλιοθήκη ανάλυση μεμονωμένα κάθε γραμμή. 


        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value
        
        inspections = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)


2. Έχουμε τώρα το αρχείο CSV ως μια RDD. Επιτρέψτε μας ανάκτηση μία γραμμή από το RDD για να κατανοήσετε τη διάταξη των δεδομένων.


        inspections.take(1)


    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of the plumbing section of the Municipal Code of Chicago and Rules and Regulation of the Board of Health. OBSEVERD THE 3 COMPARTMENT SINK BACKING UP INTO THE 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding to protect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED TO REPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN THE REAR CHILDREN AREA,IN THE KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED TO REPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY THE 15MOS AREA. NEED TO BE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY THE EXPOSED HAND SINK IN THE KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: The floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED TO ELEVATE ALL FOOD ITEMS 6INCH OFF THE FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]


3. Το αποτέλεσμα του παραπάνω μας παρέχει μια ιδέα για το σχήμα από το αρχείο εισαγωγής. το αρχείο περιλαμβάνει το όνομα του κάθε εγκατάσταση, τον τύπο της εγκατάστασης, τη διεύθυνση, τα δεδομένα των ελέγχων και τη θέση, μεταξύ άλλων. Ας επιλέξουμε μερικά στηλών που θα είναι χρήσιμες για μας πρόβλεψης ανάλυσης και να ομαδοποιήσετε τα αποτελέσματα ως ένα dataframe, η οποία χρησιμοποιούμε, στη συνέχεια, για να δημιουργήσετε ένα προσωρινό πίνακα.


        schema = StructType([
        StructField("id", IntegerType(), False), 
        StructField("name", StringType(), False), 
        StructField("results", StringType(), False), 
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')

4. Τώρα έχουμε μια *dataframe*, `df` στον οποίο μπορούμε να εκτελέσουμε μας ανάλυσης. Έχουμε επίσης ένα προσωρινό πίνακα κλήση **CountResults**. Θα σας έχετε συμπεριλάβει 4 στήλες που σας ενδιαφέρει στην το dataframe: **αναγνωριστικό**, **όνομα**, **αποτελέσματα**και **παραβιάσεις**. 
    
    Ας ξεκινήσουμε ένα μικρό δείγμα των δεδομένων:

        df.show(5)

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES TO ...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-the-data"></a>Κατανόηση των δεδομένων

1. Ας ξεκινήσουμε για να πάρετε μια ιδέα του τι περιέχει το σύνολο δεδομένων. Για παράδειγμα, τι είναι οι τις διάφορες τιμές στη στήλη **αποτελέσματα** ;


        df.select('results').distinct().show()

    
    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
    
2. Μια γρήγορη απεικόνιση να μας βοηθήσετε λόγο σχετικά με την κατανομή αυτά τα αποτελέσματα. Έχουμε ήδη τα δεδομένα σε έναν πίνακα προσωρινό **CountResults**. Μπορείτε να εκτελέσετε το ακόλουθο ερώτημα SQL σε σχέση με τον πίνακα για να λάβετε μια καλύτερη κατανόηση του τρόπου διανέμονται τα αποτελέσματα.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Το `%%sql` μαγικός ακολουθούμενο από `-o countResultsdf` εξασφαλίζει ότι το αποτέλεσμα του ερωτήματος διατηρηθεί τοπικά στο διακομιστή Jupyter (συνήθως το headnode του συμπλέγματος). Το αποτέλεσμα είναι σταθερές ως μια dataframe [Pandas](http://pandas.pydata.org/) με το καθορισμένο όνομα **countResultsdf**.
    
    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:
    
    ![Αποτέλεσμα του ερωτήματος SQL] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/query.output.png "Αποτέλεσμα του ερωτήματος SQL")

    Για περισσότερες πληροφορίες σχετικά με το `%%sql` μαγικός, καθώς και άλλες magics είναι διαθέσιμη με το PySpark πυρήνα, ανατρέξτε στο θέμα [πυρήνων διαθέσιμη Jupyter σημειωματάρια με συμπλεγμάτων HDInsight τους](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

3. Μπορείτε επίσης να χρησιμοποιήσετε Matplotlib, μια βιβλιοθήκη που χρησιμοποιούνται για τη δημιουργία απεικόνισης δεδομένων, για να δημιουργήσετε μια σχεδίασης. Επειδή η σχεδίαση πρέπει να δημιουργηθεί από το dataframe τοπικά μόνιμων **countResultsdf** , το τμήμα κώδικα πρέπει να ξεκινούν με το `%%local` μαγικός. Αυτό εξασφαλίζει ότι ο κώδικας εκτελείται τοπικά στο διακομιστή Jupyter.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        
        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:

    ![Αποτέλεσμα εξόδου](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_13_1.png)


4. Μπορείτε να δείτε ότι υπάρχουν 5 διακριτές αποτελέσματα που μπορούν να έχουν επιθεώρηση:
    
    * Επιχειρήσεις που δεν βρίσκεται 
    * Αποτυχία
    * Η φάση
    * PSS με συνθήκες, και
    * Έξοδος από το Business 

    Επιτρέψτε μας αναπτύξτε μοντέλο που μπορεί να προβλέψει το αποτέλεσμα ενός ελέγχου τροφίμων, δίνεται τις παραβιάσεις. Επειδή το εφοδιαστική παλινδρόμησης είναι μια μέθοδο δυαδικό ταξινόμηση, έχει νόημα να ομαδοποιήσετε τα δεδομένα σε δύο κατηγορίες: **αποτύχει** και **μεταβιβάζουν**. Μια "μεταβίβαση με συνθήκες" εξακολουθεί να είναι μια φάση, έτσι ώστε όταν θα σας εξάσκηση στο μοντέλο, θα σας θα μπορείτε να τα δύο αποτελέσματα ισοδύναμα. Τα δεδομένα με τα άλλα αποτελέσματα ("Επιχειρήσεις δεν βρέθηκε", "εκτός επιχειρήσεις") δεν είναι χρήσιμη, επομένως θα αφαιρέσουμε τους από το σύνολο εκπαίδευσης. Αυτό πρέπει να πειράζει, επειδή αυτές οι δύο κατηγορίες αποτελούν ένα πολύ μικρό ποσοστό των αποτελεσμάτων οπωσδήποτε.

5. Επιτρέψτε μας προχωρήσετε και να μετατρέψετε το υπάρχον dataframe (`df`) σε μια νέα dataframe όπου κάθε επιθεώρηση αποδίδεται με ένα ζεύγος παραβιάσεις ετικέτας. Σε περίπτωση μας, μια ετικέτα της `0.0` αντιπροσωπεύει μια αποτυχία, μια ετικέτα της `1.0` αντιπροσωπεύει επιτυχίας και μια ετικέτα της `-1.0` αντιπροσωπεύει ορισμένα αποτελέσματα εκτός από αυτές τις δύο. Θα σας θα φιλτράρετε αυτά τα άλλα αποτελέσματα όταν υπολογιστική το νέο πλαίσιο δεδομένων.


        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')


    Ας ανάκτηση μία γραμμή από τα δεδομένα με ετικέτες για να δείτε πώς εμφανίζεται.


        labeledData.take(1)


    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of the food establishment and all parts of the property used in connection with the operation of the establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: The walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF THE FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: The flow of air discharged from kitchen fans shall always be through a duct to a point above the roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT TO DINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT TO OFFICE.")]


## <a name="create-a-logistic-regression-model-from-the-input-dataframe"></a>Δημιουργήστε ένα μοντέλο εφοδιαστική παλινδρόμησης από το dataframe εισαγωγής

Το τελικό εργασία είναι για να μετατρέψετε το με ετικέτες δεδομένων σε μια μορφή που μπορούν να αναλυθούν από εφοδιαστική παλινδρόμηση. Της εισόδου σε έναν αλγόριθμο εφοδιαστική παλινδρόμησης πρέπει να είναι ένα σύνολο *ζεύγη ανύσματος δυνατότητα ετικετών*, όπου "ανύσματος τη δυνατότητα" είναι άνυσμα αριθμών που αντιπροσωπεύει το σημείο εισαγωγής με κάποιον τρόπο. Επομένως, χρειαζόμαστε ένας τρόπος για να μετατρέψετε τη στήλη "παραβιάσεις", η οποία είναι ημι-δομημένα και περιέχει πολλές σχολίων σε ελεύθερου κειμένου, σε έναν πίνακα πραγματικών αριθμών που εύκολα κατανοητός έναν υπολογιστή. 

Ένα τυπικό μηχανικής εκμάθησης προσέγγιση για την επεξεργασία φυσικής γλώσσας είναι να αντιστοιχίσετε κάθε διακριτές λέξη "Ευρετήριο" και, στη συνέχεια, μεταβιβάζουν ανύσματος το μηχανικής εκμάθησης αλγόριθμο τέτοια ώστε η τιμή κάθε ευρετήριο περιέχει τη σχετική συχνότητα αυτής της λέξης της συμβολοσειράς κειμένου. 

MLLib αποτελούν έναν εύκολο τρόπο για να εκτελέσετε αυτήν τη λειτουργία. Αρχικά, θα σας θα "tokenize" κάθε συμβολοσειρά παραβιάσεις για τις μεμονωμένες λέξεις σε κάθε συμβολοσειρά και, στη συνέχεια, θα χρησιμοποιήσουμε ένα `HashingTF` για να μετατρέψετε κάθε σύνολο των κωδικών σε άνυσμα δυνατότητα που μπορούν να περάσουν, στη συνέχεια, στον αλγόριθμο εφοδιαστική παλινδρόμησης για να δημιουργήσετε ένα μοντέλο. Θα μπορούμε να κάνουμε όλα αυτά τα βήματα με τη σειρά, χρησιμοποιώντας μια "διοχέτευση".


    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    
    model = pipeline.fit(labeledData)


## <a name="evaluate-the-model-on-a-separate-test-dataset"></a>Αξιολόγηση του μοντέλου σε ένα σύνολο δεδομένων ξεχωριστή δοκιμής

Μπορούμε να χρησιμοποιήσουμε το μοντέλο που δημιουργήσαμε παλαιότερη έκδοση για να *προβλέψει* ποια τα αποτελέσματα της νέας επιθεωρήσεις θα, με βάση τις παραβιάσεις που έχουν παρατηρούμενης. Θα σας κάνει την εκπαίδευση σε του συνόλου δεδομένων **Food_Inspections1.csv**αυτό το μοντέλο. Επιτρέψτε μας Χρησιμοποιήστε ένα δεύτερο σύνολο δεδομένων, **Food_Inspections2.csv**, για να *αξιολογήσετε* την ισχύ του αυτό το μοντέλο στη νέα δεδομένα. Αυτό το δεύτερο σύνολο δεδομένων (**Food_Inspections2.csv**) πρέπει να έχει ήδη το προεπιλεγμένο κοντέινερ χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.

1. Το παρακάτω τμήμα κώδικα δημιουργεί μια νέα dataframe, **predictionsDf** που περιέχει την πρόβλεψη που δημιουργούνται από το μοντέλο. Το τμήμα κώδικα δημιουργεί επίσης ένα προσωρινό πίνακα **προβλέψεων** με βάση το dataframe.


        testData = sc.textFile('wasbs:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns


    Θα πρέπει να δείτε το αποτέλεσμα όπως το εξής:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
        
        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']

2. Δείτε ένα από τα προβλέψεων. Εκτελέστε αυτό τμημάτων κώδικα:

        predictionsDf.take(1)

    Θα δείτε την πρόβλεψη για την πρώτη καταχώρηση του συνόλου δεδομένων δοκιμής.

3. Το `model.transform()` μέθοδο θα ισχύουν το ίδιο μετασχηματισμό σε οποιαδήποτε νέα δεδομένα με το ίδιο σχήμα και φτάνετε στη μία πρόβλεψη του τρόπου ταξινόμησης των δεδομένων. Μπορούμε να κάνουμε ορισμένες απλές στατιστικά στοιχεία για να πάρετε μια ιδέα του πώς ακριβή μας προβλέψεων έχουν:


        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR 
                                              (prediction = 1 AND (results = 'Pass' OR 
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()
        
        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    Το αποτέλεσμα μοιάζει με τα εξής:
    
        # -----------------
        # THIS IS AN OUTPUT
        # -----------------
    
        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate


    Χρήση εφοδιαστική παλινδρόμησης με τους παρέχει μας μια ακριβή μοντέλο της σχέσης μεταξύ παραβιάσεις περιγραφές στα Αγγλικά και αν μια δεδομένη επιχείρηση θα μεταβιβάζουν ή αποτύχει έναν έλεγχο τροφίμων. 

## <a name="create-a-visual-representation-of-the-prediction"></a>Δημιουργήστε μια οπτική αναπαράσταση των την πρόβλεψη

Θα σας τώρα να σχηματίσετε μια τελική απεικόνιση για να μας βοηθήσετε λόγο σχετικά με τα αποτελέσματα της αυτόν τον έλεγχο. 

1. Ξεκινήσουμε με την εξαγωγή τις διαφορετικές προβλέψεων και τα αποτελέσματα από τον πίνακα προσωρινό **προβλέψεων** που δημιουργήσατε νωρίτερα. Τα ακόλουθα ερωτήματα Διαχωρίστε το αποτέλεσμα ως *true_positive*, *false_positive*, *true_negative*και *false_negative*. Στα παρακάτω ερωτήματα, να κλείσουμε απεικόνιση με τη χρήση `-q` και επίσης να αποθηκεύσετε το αποτέλεσμα (με τη χρήση `-o`) ως dataframes που μπορούν να χρησιμοποιηθούν, στη συνέχεια, με το `%%local` μαγικός. 

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions') 

2. Τέλος, μπορείτε να χρησιμοποιήσετε το παρακάτω τμήμα κώδικα για τη δημιουργία του σχεδίασης με χρήση **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')
    
    Θα πρέπει να βλέπετε το παρακάτω αποτέλεσμα.
    
    ![Πρόβλεψη εξόδου](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/output_26_1.png)


    Αυτό το γράφημα, "θετικό" αποτέλεσμα αναφέρεται η επιθεώρηση αποτυχίας τροφίμων, ενώ αρνητικό αποτέλεσμα αναφέρεται σε που μεταβιβάστηκε επιθεώρηση.

## <a name="shut-down-the-notebook"></a>Τερματίστε το Σημειωματάριο

Αφού ολοκληρώσετε την εκτέλεση της εφαρμογής, θα πρέπει να τερματισμού στο Σημειωματάριο για να αφήσετε τους πόρους. Για να το κάνετε αυτό, από το μενού **αρχείο** στο Σημειωματάριο, κάντε κλικ στην επιλογή **Κλείσιμο και διακοπή**. Αυτό θα τερματισμού και κλείσιμο του σημειωματαρίου.


## <a name="seealso"></a>Δείτε επίσης


* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight για την ανάλυση δόμησης θερμοκρασίας με τη χρήση δεδομένων HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

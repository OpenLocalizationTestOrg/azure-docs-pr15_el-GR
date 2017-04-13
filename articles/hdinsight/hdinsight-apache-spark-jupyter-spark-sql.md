<properties
    pageTitle="Δημιουργήστε ένα σύμπλεγμα τους σε HDInsight Linux και χρησιμοποιήστε τους SQL από Jupyter για την ανάλυση αλληλεπιδραστικών | Microsoft Azure"
    description="Οδηγίες βήμα προς βήμα σχετικά με τον τρόπο για να δημιουργήσετε γρήγορα μια τους Apache συμπλέγματος σε HDInsight και κατόπιν χρησιμοποιήστε τους SQL από τα σημειωματάρια Jupyter για την εκτέλεση αλληλεπιδραστικών ερωτήματα."
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
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="nitinme"/>


# <a name="get-started-create-apache-spark-cluster-on-hdinsight-linux-and-run-interactive-queries-using-spark-sql"></a>Γρήγορα αποτελέσματα: δημιουργία συμπλέγματος Apache τους σε HDInsight Linux και εκτέλεση αλληλεπιδραστικών ερωτημάτων με τους SQL

Μάθετε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα Apache τους σε HDInsight και, στη συνέχεια, χρησιμοποιήστε το Σημειωματάριο [Jupyter](https://jupyter.org) για την εκτέλεση αλληλεπιδραστικών ερωτήματα SQL τους στο σύμπλεγμα τους.

   ![Γρήγορα αποτελέσματα με τη χρήση τους Apache στο HDInsight] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Να ξεκινήσετε να χρησιμοποιείτε τους Apache στο πρόγραμμα εκμάθησης HDInsight. Τα βήματα που περιγράφονται: Δημιουργία λογαριασμού χώρου αποθήκευσης; Δημιουργήστε ένα σύμπλεγμα; εκτέλεση προτάσεις SQL τους")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- **Azure μια συνδρομή**. Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Κέλυφος ασφαλούς A (SSH) προγράμματος-πελάτη**: Linux, Unix και OS X provied συστήματα SSH ενός προγράμματος-πελάτη έως το `ssh` εντολή. Για συστήματα των Windows, συνιστάται να [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Πλήκτρα ασφαλούς κελύφους (SSH) (προαιρετικό)**: μπορείτε να ασφαλίσετε το λογαριασμό SSH που χρησιμοποιήσατε για να συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας έναν κωδικό πρόσβασης ή ένα δημόσιο κλειδί. Χρήση κωδικού πρόσβασης λαμβάνει έχετε γρήγορα αποτελέσματα και θα πρέπει να χρησιμοποιήσετε αυτήν την επιλογή εάν θέλετε να δημιουργήσετε ένα σύμπλεγμα γρήγορα και να εκτελέσετε ορισμένες εργασίες δοκιμής. Χρήση αριθμού-κλειδιού είναι πιο ασφαλή, ωστόσο απαιτεί επιπλέον ρύθμιση. Ενδέχεται να θέλετε να χρησιμοποιήσετε αυτήν την προσέγγιση, όταν δημιουργείτε ένα σύμπλεγμα παραγωγής. Σε αυτό το άρθρο, χρησιμοποιούμε η προσέγγιση τον κωδικό πρόσβασης. Για οδηγίες σχετικά με τον τρόπο δημιουργίας και χρήσης πλήκτρα SSH με το HDInsight, ανατρέξτε στα ακόλουθα άρθρα:

    -  Από Linux υπολογιστή - [Χρήση SSH με βάσει Linux HDInsight (Hadoop) από το Linux, Unix, ή OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Από έναν υπολογιστή Windows - [Χρήση SSH με βάσει Linux HDInsight (Hadoop) από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] Σε αυτό το άρθρο χρησιμοποιεί ένα πρότυπο manager Azure πόρων για να δημιουργήσετε ένα σύμπλεγμα τους που χρησιμοποιεί [Azure χώρο αποθήκευσης αντικειμένων blob ως του χώρου αποθήκευσης συμπλέγματος](hdinsight-hadoop-use-blob-storage.md). Μπορείτε επίσης να δημιουργήσετε ένα σύμπλεγμα τους που χρησιμοποιεί το [Χώρο αποθήκευσης λίμνης Azure δεδομένων](../data-lake-store/data-lake-store-overview.md) ως μια επιπλέον χώρο αποθήκευσης, εκτός από το Azure χώρο αποθήκευσης αντικειμένων blob ως το προεπιλεγμένο αποθήκευσης. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-spark-cluster"></a>Δημιουργήστε τους συμπλέγματος

Σε αυτήν την ενότητα, μπορείτε να δημιουργήσετε ένα σύμπλεγμα έκδοση 3.4 HDInsight (τους έκδοση 1.6.1) χρησιμοποιώντας ένα πρότυπο manager Azure πόρων. Για πληροφορίες σχετικά με τις εκδόσεις HDInsight και τους SLA, ανατρέξτε στο θέμα [Διαχείριση εκδόσεων στοιχείου HDInsight](hdinsight-component-versioning.md). Για άλλες μεθόδους δημιουργίας σύμπλεγμα, ανατρέξτε στο θέμα [Δημιουργία HDInsight συμπλεγμάτων](hdinsight-hadoop-provision-linux-clusters.md).

1. Κάντε κλικ στην παρακάτω εικόνα για να ανοίξετε το πρότυπο στην πύλη του Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Το πρότυπο βρίσκεται σε δημόσια blob κοντέινερ, *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. Από το blade παραμέτρους, εισαγάγετε τα εξής:

    - **ClusterName**: Πληκτρολογήστε ένα όνομα για το σύμπλεγμα Hadoop που θα δημιουργήσετε.
    - **Σύμπλεγμα όνομα σύνδεσης και τον κωδικό πρόσβασης**: το προεπιλεγμένο όνομα σύνδεσης είναι διαχειριστής.
    - **SSH όνομα χρήστη και τον κωδικό πρόσβασης**.
    
    Γράψτε αυτές τις τιμές.  Θα χρειαστείτε τα αργότερα στην εκμάθηση.

    > [AZURE.NOTE] SSH χρησιμοποιείται για απομακρυσμένη πρόσβαση στο σύμπλεγμα HDInsight χρησιμοποιώντας μια γραμμή εντολών. Το όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείτε εδώ χρησιμοποιείται κατά τη σύνδεση στο σύμπλεγμα μέσω SSH. Επίσης, το όνομα χρήστη SSH πρέπει να είναι μοναδικό, όπως δημιουργεί ένα λογαριασμό χρήστη σε όλους τους κόμβους συμπλέγματος HDInsight. Τα παρακάτω είναι ορισμένες από τα ονόματα των λογαριασμών δεσμευμένο για χρήση από τις υπηρεσίες στο σύμπλεγμα και δεν μπορεί να χρησιμοποιηθεί ως το όνομα χρήστη SSH:
    >
    > ριζικό κατάλογο, hdiuser, καταιγίδας, hbase, ubuntu, zookeeper, hdfs, νήματα, mapred, hbase, hive, oozie, falcon, sqoop, διαχείρισης, tez, hcat, hdinsight zookeeper.

    > Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε σε ένα από τα ακόλουθα άρθρα:

    > * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3, κάντε κλικ στο κουμπί **OK** για να αποθηκεύσετε τις παραμέτρους.

4 από την **Ανάπτυξη προσαρμοσμένης** blade, κάντε κλικ στο πλαίσιο αναπτυσσόμενης λίστας **ομάδα πόρων** και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία** για να δημιουργήσετε μια νέα ομάδα πόρων. Η ομάδα πόρων είναι ένα κοντέινερ που ομαδοποιεί το σύμπλεγμα, ο λογαριασμός εξαρτώμενα χώρου αποθήκευσης και άλλων πόρων συνδεδεμένων.

Διαχειριστής, κάντε κλικ στην επιλογή **νομική τους όρους**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

6 κάντε κλικ στην επιλογή **Δημιουργία**. Θα δείτε ένα νέο πλακίδιο με τίτλο Submitting ανάπτυξης για ανάπτυξη προτύπου. Διαρκεί περίπου περίπου 20 λεπτά για να δημιουργήσετε το σύμπλεγμα και βάση δεδομένων SQL.



## <a name="run-spark-sql-queries-using-a-jupyter-notebook"></a>Εκτέλεση ερωτημάτων SQL τους χρήσης ενός σημειωματαρίου Jupyter

Σε αυτήν την ενότητα, μπορείτε να χρησιμοποιήσετε Jupyter σημειωματάριο για να εκτελέσετε ερωτήματα SQL τους σε σχέση με το σύμπλεγμα τους. Τους HDInsight συμπλεγμάτων παρέχουν δύο πυρήνων που μπορείτε να χρησιμοποιήσετε με το Σημειωματάριο Jupyter. Αυτές είναι:

* **PySpark** (για εφαρμογές που έχουν δημιουργηθεί στο Python)
* **Τους** (για εφαρμογές που έχουν δημιουργηθεί στο Scala)

Σε αυτό το άρθρο, θα χρησιμοποιήσετε τον πυρήνα PySpark. Στο άρθρο [πυρήνων διαθέσιμη Jupyter σημειωματάρια με συμπλεγμάτων HDInsight τους](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) , μπορείτε να διαβάσετε με λεπτομέρειες σχετικά με τα οφέλη της χρήσης του πυρήνα PySpark. Ωστόσο, ορισμένες βασικά πλεονεκτήματα της χρήσης του πυρήνα PySpark είναι:

* Δεν χρειάζεται να ορίσετε τα περιβάλλοντα για τους και ομάδα. Αυτά τα ρυθμίζονται αυτόματα για εσάς.
* Μπορείτε να χρησιμοποιήσετε magics κελιού, όπως είναι οι `%%sql`, για να εκτελέσετε απευθείας σας ερωτήματα SQL ή η ομάδα, χωρίς οποιαδήποτε προηγούμενη τμήματα κώδικα.
* Το αποτέλεσμα για τα ερωτήματα SQL ή η ομάδα είναι απεικονιστούν αυτόματα.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Δημιουργία σημειωματαρίου Jupyter με πυρήνα PySpark 

1. Από την [Πύλη Azure](https://portal.azure.com/), από την startboard, κάντε κλικ στο πλακίδιο για το σύμπλεγμα τους (εάν καρφιτσωμένα αυτό για να το startboard). Μπορείτε επίσης να μεταβείτε σε το σύμπλεγμά σας στην περιοχή **Αναζήτηση όλων** > **Συμπλεγμάτων HDInsight**.   

2. Από το σύμπλεγμα blade τους, κάντε κλικ στην επιλογή **Πίνακας εργαλείων σύμπλεγμα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Jupyter Σημειωματάριο**. Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια διαχειριστή για το σύμπλεγμα.

    > [AZURE.NOTE] Μπορείτε επίσης μπορεί να φτάσει στο Σημειωματάριο Jupyter για το σύμπλεγμά σας ανοίγοντας την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Δημιουργήστε ένα νέο σημειωματάριο. Κάντε κλικ στην επιλογή **Δημιουργία**και, στη συνέχεια, κάντε κλικ στην επιλογή **PySpark**.

    ![Δημιουργία νέου σημειωματαρίου Jupyter] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Δημιουργία νέου σημειωματαρίου Jupyter")

3. Δημιουργείται ένα νέο σημειωματάριο και ανοίγει με το όνομα Untitled.pynb. Κάντε κλικ στο όνομα του σημειωματαρίου στο επάνω μέρος και πληκτρολογήστε ένα φιλικό όνομα.

    ![Παροχή ένα όνομα για το Σημειωματάριο] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Παροχή ένα όνομα για το Σημειωματάριο")

4. Επειδή έχετε δημιουργήσει ένα σημειωματάριο χρησιμοποιώντας το PySpark πυρήνα, δεν χρειάζεται να δημιουργήσετε οποιαδήποτε περιβάλλοντα ρητά. Τα περιβάλλοντα τους και η ομάδα θα δημιουργηθεί αυτόματα για εσάς όταν εκτελείτε το πρώτο κελί κώδικα. Μπορείτε να ξεκινήσετε με την εισαγωγή των τύπων που απαιτείται για αυτό το σενάριο. Για να το κάνετε, επικολλήστε το εξής τμήμα κώδικα σε ένα κελί και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Κάθε φορά που εκτελείτε μια εργασία στο Jupyter, τον τίτλο παράθυρο του προγράμματος περιήγησης web θα εμφανιστεί μια κατάσταση **(απασχολημένος)** μαζί με τον τίτλο του σημειωματαρίου. Μπορείτε, επίσης, θα δείτε έναν κύκλο συμπαγούς δίπλα στο κείμενο **PySpark** στην επάνω δεξιά γωνία. Μετά την ολοκλήρωση της εργασίας, αυτό θα αλλάξει σε ένα κενό κύκλο.

     ![Κατάσταση μιας εργασίας Jupyter σημειωματαρίου] (./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Κατάσταση μιας εργασίας Jupyter σημειωματαρίου")

4. Φόρτωση του δείγματος δεδομένων σε ένα προσωρινό πίνακα. Όταν δημιουργείτε ένα σύμπλεγμα τους στο HDInsight, το δείγμα αρχείου δεδομένων, **hvac.csv**, αντιγράφεται με το λογαριασμό συσχετισμένη χώρου αποθήκευσης στην περιοχή **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Σε ένα κενό κελί, επικολλήστε το ακόλουθο παράδειγμα κώδικα και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**. Αυτό το παράδειγμα κώδικα καταχωρεί τα δεδομένα σε ένα προσωρινό πίνακα που ονομάζεται **hvac**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Επειδή χρησιμοποιείτε μια πυρήνα PySpark, τώρα μπορείτε να απευθείας εκτελέσετε ένα ερώτημα SQL σε το προσωρινό πίνακα **hvac** που μόλις δημιουργήσατε, χρησιμοποιώντας το `%%sql` μαγικός. Για περισσότερες πληροφορίες σχετικά με το `%%sql` μαγικός, καθώς και άλλες magics είναι διαθέσιμη με το PySpark πυρήνα, ανατρέξτε στο θέμα [πυρήνων διαθέσιμη Jupyter σημειωματάρια με συμπλεγμάτων HDInsight τους](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Όταν η εργασία ολοκληρώθηκε με επιτυχία, η ακόλουθη Έξοδος σε μορφή πίνακα εμφανίζεται από προεπιλογή.

    ![Αποτέλεσμα πίνακα αποτέλεσμα του ερωτήματος] (./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Αποτέλεσμα πίνακα αποτέλεσμα του ερωτήματος")

    Μπορείτε επίσης να δείτε τα αποτελέσματα σε καθώς και άλλες απεικονίσεις. Για παράδειγμα, ένα γράφημα περιοχής για το ίδιο αποτέλεσμα θα είναι παρόμοιο με το ακόλουθο.

    ![Γράφημα περιοχής με το αποτέλεσμα του ερωτήματος] (./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Γράφημα περιοχής με το αποτέλεσμα του ερωτήματος")


6. Αφού ολοκληρώσετε την εκτέλεση της εφαρμογής, θα πρέπει να τερματισμού στο Σημειωματάριο για να αφήσετε τους πόρους. Για να το κάνετε αυτό, από το μενού **αρχείο** στο Σημειωματάριο, κάντε κλικ στην επιλογή **Κλείσιμο και διακοπή**. Αυτό θα τερματισμού και κλείσιμο του σημειωματαρίου.

##<a name="delete-the-cluster"></a>Διαγραφή του συμπλέγματος

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="see-also"></a>Δείτε επίσης


* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight για την ανάλυση δόμησης θερμοκρασίας με τη χρήση δεδομένων HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη της εστίασης στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

* [Ανάλυση καταγραφής τοποθεσία Web χρησιμοποιώντας τους στο HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Εφαρμογή πληροφορίες για τηλεμετρίας ανάλυση δεδομένων χρησιμοποιώντας τους στο HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

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

* [Παρακολούθηση και εντοπισμού σφαλμάτων εργασίες που εκτελείται σε ένα σύμπλεγμα Apache τους στο HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

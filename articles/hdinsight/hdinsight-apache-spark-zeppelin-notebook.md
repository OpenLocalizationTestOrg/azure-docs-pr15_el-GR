<properties 
    pageTitle="Χρήση Zeppelin σημειωματάρια με σύμπλεγμα τους σε HDInsight Linux | Microsoft Azure" 
    description="Οδηγίες βήμα προς βήμα σχετικά με τη χρήση Zeppelin σημειωματάρια με τους συμπλεγμάτων σε HDInsight Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Χρήση Zeppelin σημειωματάρια με σύμπλεγμα Apache τους σε HDInsight Linux

Τους HDInsight συμπλεγμάτων περιλαμβάνουν Zeppelin τα σημειωματάρια που μπορείτε να χρησιμοποιήσετε για να εκτελέσετε εργασίες τους. Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε το Σημειωματάριο Zeppelin σε ένα σύμπλεγμα HDInsight.


**Προαπαιτούμενα στοιχεία:**

* Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Ένα σύμπλεγμα Apache τους. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία τους Apache συμπλεγμάτων στο Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Εκκίνηση ενός σημειωματαρίου Zeppelin

1. Από το σύμπλεγμα blade τους, κάντε κλικ στην επιλογή **Πίνακας εργαλείων σύμπλεγμα**και, στη συνέχεια, κάντε κλικ στην επιλογή **Zeppelin σημειωματαρίου**. Εάν σας ζητηθεί, πληκτρολογήστε τα διαπιστευτήρια διαχειριστή για το σύμπλεγμα.

    > [AZURE.NOTE] Μπορείτε επίσης μπορεί να φτάσει το Σημειωματάριο Zeppelin για το σύμπλεγμά σας ανοίγοντας την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης. Αντικαταστήστε το __CLUSTERNAME__ με το όνομα του συμπλέγματος:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Δημιουργήστε ένα νέο σημειωματάριο. Από το παράθυρο κεφαλίδα, κάντε κλικ στο **Σημειωματάριο**και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία νέας σημείωσης**.

    ![Δημιουργία νέου σημειωματαρίου Zeppelin] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Δημιουργία νέου σημειωματαρίου Zeppelin")

    Πληκτρολογήστε ένα όνομα για το Σημειωματάριο και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία σημείωσης**.

3. Επίσης, βεβαιωθείτε ότι η κεφαλίδα Σημειωματάριο εμφανίζει ένα συνδεδεμένο την κατάσταση. Δηλώνεται με μια πράσινη κουκκίδα στην επάνω δεξιά γωνία.

    ![Κατάσταση Zeppelin σημειωματαρίου] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Κατάσταση Zeppelin σημειωματαρίου")

4. Φόρτωση του δείγματος δεδομένων σε ένα προσωρινό πίνακα. Όταν δημιουργείτε ένα σύμπλεγμα τους σε HDInsight, το δείγμα αρχείου δεδομένων, **hvac.csv**, αντιγράφεται στο λογαριασμό συσχετισμένη χώρου αποθήκευσης στην περιοχή **\HdiSamples\SensorSampleData\hvac**.

    Στο κενό παραγράφου που δημιουργούνται από προεπιλογή για το νέο σημειωματάριο, επικολλήστε το εξής τμήμα κώδικα.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER** ή κάντε κλικ στο κουμπί " **αναπαραγωγή** " για την παράγραφο για να εκτελέσετε το τμήμα κώδικα. Η κατάσταση στη δεξιά γωνία της παραγράφου πρέπει να προόδου από είστε ΈΤΟΙΜΟΙ, σε ΕΚΚΡΕΜΌΤΗΤΑ, ΕΚΤΕΛΕΊΤΑΙ σε ΟΛΟΚΛΗΡΩΜΈΝΗ. Το αποτέλεσμα εμφανίζεται στο κάτω μέρος της παραγράφου. Το στιγμιότυπο οθόνης έχει τα εξής:

    ![Δημιουργία προσωρινό πίνακα από ανεπεξέργαστα δεδομένα] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Δημιουργία προσωρινό πίνακα από ανεπεξέργαστα δεδομένα")

    Μπορείτε επίσης να δώσετε έναν τίτλο για κάθε παράγραφο. Από τη δεξιά γωνία, κάντε κλικ στο εικονίδιο **Ρυθμίσεις** και, στη συνέχεια, κάντε κλικ στην επιλογή **Εμφάνιση τίτλου**.

5. Τώρα, μπορείτε να εκτελέσετε προτάσεις SQL τους στον πίνακα **hvac** . Επικολλήστε το ακόλουθο ερώτημα νέας παραγράφου. Το ερώτημα ανακτά το Αναγνωριστικό δόμησης και τη διαφορά μεταξύ του προορισμού και του πραγματικού θερμοκρασίες για κάθε κατασκευή σε μια δεδομένη ημερομηνία. Πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    Η πρόταση **% sql** στην αρχή ενημερώνει το Σημειωματάριο για να χρησιμοποιήσετε το πρόγραμμα μεταγλώττισης Scala Λίβιος.

    Το παρακάτω στιγμιότυπο οθόνης εμφανίζει το αποτέλεσμα.

    ![Εκτέλεσης μιας πρότασης SQL τους, χρησιμοποιώντας το Σημειωματάριο] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Εκτέλεσης μιας πρότασης SQL τους, χρησιμοποιώντας το Σημειωματάριο")

     Κάντε κλικ στην επιλογή Εμφάνιση επιλογών (επισημαίνεται με ορθογώνιο) για να κάνετε εναλλαγή μεταξύ διαφορετικές αναπαραστάσεις για το ίδιο αποτέλεσμα. Κάντε κλικ στην επιλογή **Ρυθμίσεις** για να επιλέξετε ποιες consitutes το κλειδί και τιμές στο αποτέλεσμα. Την εικόνα της οθόνης, επάνω από χρησιμοποιεί **buildingID** ως το κλειδί και το μέσο όρο των **temp_diff** ως η τιμή.

    
6. Μπορείτε επίσης να εκτελέσετε προτάσεις SQL τους χρήση μεταβλητών στο ερώτημα. Το επόμενο τμήμα κώδικα δείχνει πώς μπορείτε να ορίσετε μια μεταβλητή, **Temp**, στο ερώτημα με τις πιθανές τιμές που θέλετε να ερωτήματος με. Πρώτη φορά που εκτελείτε το ερώτημα, μια αναπτυσσόμενη λίστα συμπληρώνεται αυτόματα με τις τιμές που καθορίσατε για τη μεταβλητή.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Επικόλληση αυτό τμήματος κώδικα σε μια νέα παράγραφο και πατήστε το **συνδυασμό πλήκτρων SHIFT + ENTER**. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει το αποτέλεσμα.

    ![Εκτέλεσης μιας πρότασης SQL τους, χρησιμοποιώντας το Σημειωματάριο] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Εκτέλεσης μιας πρότασης SQL τους, χρησιμοποιώντας το Σημειωματάριο")

    Οι επόμενες ερωτήματα, μπορείτε να επιλέξετε μια νέα τιμή από την αναπτυσσόμενη λίστα και να εκτελέσετε ξανά το ερώτημα. Κάντε κλικ στην επιλογή **Ρυθμίσεις** για να επιλέξετε ποιες consitutes το κλειδί και τιμές στο αποτέλεσμα. Η καταγραφή της οθόνης παραπάνω χρησιμοποιεί **buildingID** ως το κλειδί, τον μέσο όρο των **temp_diff** ως η τιμή και **targettemp** ως ομάδα.

7. Επανεκκινήστε το πρόγραμμα μεταγλώττισης Λίβιος για έξοδο από την εφαρμογή. Για να το κάνετε αυτό, ανοίξτε τις ρυθμίσεις διερμηνείας κάνοντας κλικ στην επιλογή αποθηκευμένου στο πλαίσιο όνομα χρήστη από στην επάνω δεξιά γωνία και, στη συνέχεια, κάντε κλικ στην επιλογή **μεταφραστή**.

    ![Εκκίνηση διερμηνείας] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive εξόδου")

2. Κάντε κύλιση στις ρυθμίσεις διερμηνείας Λίβιος και, στη συνέχεια, κάντε κλικ στο κουμπί **επανεκκίνηση**.

    ![Επανεκκινήστε το intepreter Λίβιος] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Επανεκκινήστε το intepreter Zeppelin")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Πώς μπορώ να χρησιμοποιήσω εξωτερικών πακέτων με το Σημειωματάριο;

Μπορείτε να ρυθμίσετε το Σημειωματάριο Zeppelin συμπλέγματος Apache τους σε HDInsight (Linux) για να χρησιμοποιήσετε εξωτερικές, συνεισφέρει Κοινότητας πακέτα που δεν περιλαμβάνονται εκτός του-οι έτοιμες στο σύμπλεγμα. Μπορείτε να κάνετε αναζήτηση στο [αποθετήριο δεδομένων Maven](http://search.maven.org/) για την πλήρη λίστα των πακέτων που είναι διαθέσιμες. Μπορείτε επίσης να λάβετε μια λίστα με διαθέσιμα πακέτα από άλλες προελεύσεις. Για παράδειγμα, μια πλήρη λίστα των πακέτων συνεισφέρει Κοινότητας είναι διαθέσιμη στο [Πακέτων τους](http://spark-packages.org/).

Σε αυτό το άρθρο, θα δείτε πώς μπορείτε να χρησιμοποιήσετε το πακέτο [csv τους](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) με το Σημειωματάριο Jupyter.

1. Ανοίξτε τις ρυθμίσεις μεταφραστή. Από στην επάνω δεξιά γωνία, κάντε κλικ στην επιλογή αποθηκευμένου στο πλαίσιο όνομα χρήστη και, στη συνέχεια, κάντε κλικ στην επιλογή **μεταφραστή**.

    ![Εκκίνηση διερμηνείας] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive εξόδου")

2. Κάντε κύλιση στις ρυθμίσεις Λίβιος μεταφραστής και, στη συνέχεια, κάντε κλικ στην επιλογή **Επεξεργασία**.

    ![Αλλαγή των ρυθμίσεων διερμηνείας] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Αλλαγή των ρυθμίσεων διερμηνείας")

3. Προσθήκη νέου αριθμού-κλειδιού, που ονομάζεται **livy.spark.jars.packages** και ορίστε την τιμή με τη μορφή `group:id:version`. Επομένως, εάν θέλετε να χρησιμοποιήσετε το πακέτο [τους csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) , πρέπει να ορίσετε την τιμή του αριθμού-κλειδιού για να `com.databricks:spark-csv_2.10:1.4.0`.

    ![Αλλαγή των ρυθμίσεων διερμηνείας] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Αλλαγή των ρυθμίσεων διερμηνείας")

    Κάντε κλικ στην επιλογή **Αποθήκευση** και, στη συνέχεια, επανεκκινήστε το πρόγραμμα μεταγλώττισης Λίβιος.

4. **Συμβουλή**: Εάν θέλετε να κατανοήσετε τον τρόπο για να παραδίδεται στο την τιμή του αριθμού-κλειδιού πληκτρολογήσατε παραπάνω, δείτε πώς.

    μια. Εντοπίστε το πακέτο στο αποθετήριο Maven. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήσαμε [τους csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    β. Από το χώρο αποθήκευσης, να συγκεντρώσετε τις τιμές για το **αναγνωριστικό ομάδας**, **ArtifactId**και την **έκδοση**.

    ![Χρήση εξωτερικών πακέτων με Jupyter σημειωματαρίου] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Χρήση εξωτερικών πακέτων με Jupyter σημειωματαρίου")

    c. CONCATENATE τις τρεις τιμές, διαχωρισμένα με ερωτηματικό (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Πού αποθηκεύονται τα σημειωματάρια Zeppelin;

Τα σημειωματάρια Zeppelin αποθηκεύονται τα headnodes σύμπλεγμα. Επομένως, εάν διαγράψετε το σύμπλεγμα, τα σημειωματάρια θα διαγραφούν επίσης. Εάν θέλετε να διατηρήσετε τα σημειωματάριά σας για μελλοντική χρήση σε άλλες συμπλεγμάτων, που πρέπει να τις εξαγάγετε αφού ολοκληρώσετε την εκτέλεση των εργασιών. Για να εξαγάγετε ένα σημειωματάριο, κάντε κλικ στο εικονίδιο **εξαγωγής** , όπως φαίνεται στην παρακάτω εικόνα.

![Κάντε λήψη του σημειωματαρίου] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Λήψη του σημειωματάριου")

Αυτό αποθηκεύει το Σημειωματάριο ως αρχείο JSON στη θέση λήψης.

## <a name="livy-session-management"></a>Διαχείριση περιόδων λειτουργίας Λίβιος

Όταν εκτελείτε την πρώτη παράγραφο κώδικα στο σημειωματάριό σας Zeppelin, μιας νέας περιόδου λειτουργίας Λίβιος δημιουργείται στο το σύμπλεγμά σας HDInsight τους. Αυτή η περίοδος λειτουργίας είναι κοινόχρηστα σε όλα τα σημειωματάρια Zeppelin που θα δημιουργήσετε στη συνέχεια. Εάν για κάποιο λόγο η περίοδος λειτουργίας είναι Λίβιος θανατώθηκαν (σύμπλεγμα επανεκκινήστε τον υπολογιστή, κ.λπ.), δεν θα μπορείτε να εκτελέσετε εργασίες από το Σημειωματάριο Zeppelin.

Σε αυτήν την περίπτωση, πρέπει να εκτελέσετε τα παρακάτω βήματα πριν να ξεκινήσετε την εργασίες που εκτελούνται από ένα σημειωματάριο Zeppelin. 

1. Επανεκκινήστε το πρόγραμμα μεταγλώττισης Λίβιος από το Σημειωματάριο Zeppelin. Για να το κάνετε αυτό, ανοίξτε τις ρυθμίσεις διερμηνείας κάνοντας κλικ στην επιλογή αποθηκευμένου στο πλαίσιο όνομα χρήστη από στην επάνω δεξιά γωνία και, στη συνέχεια, κάντε κλικ στην επιλογή **μεταφραστή**.

    ![Εκκίνηση διερμηνείας] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive εξόδου")

2. Κάντε κύλιση στις ρυθμίσεις διερμηνείας Λίβιος και, στη συνέχεια, κάντε κλικ στο κουμπί **επανεκκίνηση**.

    ![Επανεκκινήστε το intepreter Λίβιος] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Επανεκκινήστε το intepreter Zeppelin")

3. Εκτελέστε ένα κελί κώδικα από ένα υπάρχον σημειωματάριο Zeppelin. Αυτό δημιουργεί μια νέα περίοδο λειτουργίας Λίβιος στο σύμπλεγμα HDInsight.

## <a name="seealso"></a>Δείτε επίσης


* [Επισκόπηση: Apache τους σε Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Σενάρια

* [Τους με το BI: Εκτέλεση ανάλυσης αλληλεπιδραστικών δεδομένων με χρήση τους σε HDInsight με εργαλεία Επιχειρηματικής ευφυΐας](hdinsight-apache-spark-use-bi-tools.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight για την ανάλυση δόμησης θερμοκρασίας με τη χρήση δεδομένων HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Τους με μηχανικής εκμάθησης: χρήση τους σε HDInsight πρόβλεψη της εστίασης στα αποτελέσματα ελέγχου](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Τους ροής: Χρήση τους σε HDInsight για τη δημιουργία εφαρμογών σε πραγματικό χρόνο ροής](hdinsight-apache-spark-eventhub-streaming.md)

* [Ανάλυση καταγραφής τοποθεσία Web χρησιμοποιώντας τους στο HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Δημιουργία και εκτέλεση εφαρμογών

* [Δημιουργήστε μια μεμονωμένη εφαρμογή χρησιμοποιώντας Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Απομακρυσμένη εκτέλεση εργασιών σε ένα σύμπλεγμα τους χρησιμοποιώντας Λίβιος](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Εργαλεία και επεκτάσεις

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για να δημιουργήσετε και να υποβάλετε τους Scala εφαρμογών](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Χρησιμοποιήστε HDInsight εργαλεία προσθήκης για IntelliJ ΙΔΈΑ για τον εντοπισμό σφαλμάτων εφαρμογών τους από απόσταση](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Διαθέσιμο για Jupyter σημειωματαρίου στο σύμπλεγμα τους για HDInsight πυρήνων](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Χρήση εξωτερικών πακέτων με σημειωματάρια Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Εγκατάσταση Jupyter στον υπολογιστή σας και να συνδεθείτε με ένα σύμπλεγμα HDInsight τους](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Διαχείριση πόρων

* [Διαχείριση πόρων για το σύμπλεγμα Apache τους στο Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Παρακολούθηση και ο εντοπισμός σφαλμάτων εργασίες που εκτελείται σε ένα σύμπλεγμα Apache τους στο HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 








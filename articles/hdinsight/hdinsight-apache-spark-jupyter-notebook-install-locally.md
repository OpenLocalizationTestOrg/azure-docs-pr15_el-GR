<properties 
    pageTitle="Εγκατάσταση Jupyter Σημειωματάριο στον υπολογιστή σας και συνδέστε τη με ένα σύμπλεγμα τους HDInsight | Microsoft Azure" 
    description="Μάθετε περισσότερα σχετικά με τον τρόπο εγκατάστασης Jupyter σημειωματαρίων τοπικά στον υπολογιστή σας και συνδέστε τη με ένα σύμπλεγμα Apache τους σε Azure HDInsight." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Εγκατάσταση Jupyter Σημειωματάριο στον υπολογιστή σας και να συνδεθείτε με σύμπλεγμα Apache τους σε HDInsight Linux

Σε αυτό το άρθρο θα μάθετε πώς μπορείτε να εγκαταστήσετε το Σημειωματάριο Jupyter, με το προσαρμοσμένο PySpark (για Python) και πυρήνων τους (για Scala) με τους μαγικός και συνδέστε το σημειωματάριο σε ένα σύμπλεγμα HDInsight. Μπορεί να υπάρχει ένας αριθμός λόγοι για να εγκαταστήσετε το Jupyter στον τοπικό σας υπολογιστή και μπορεί να υπάρχουν επίσης ορισμένες προκλήσεις. Για μια λίστα με τους λόγους και προκλήσεις, ανατρέξτε στην ενότητα [Γιατί πρέπει να εγκαταστήσω το Jupyter στον υπολογιστή μου](#why-should-i-install-jupyter-on-my-computer) στο τέλος αυτού του άρθρου.

Υπάρχουν τρία βασικά βήματα που αφορούν την εγκατάσταση του Jupyter και η μαγεία τους στον υπολογιστή σας.

* Εγκατάσταση Jupyter σημειωματαρίου
* Εγκαταστήστε το πυρήνων PySpark και τους με η μαγεία τους
* Ρύθμιση παραμέτρων μαγικός τους για να αποκτήσετε πρόσβαση σε σύμπλεγμα τους σε HDInsight

Για περισσότερες πληροφορίες σχετικά με την προσαρμοσμένη πυρήνων και η μαγεία τους που είναι διαθέσιμες για τα σημειωματάρια Jupyter με σύμπλεγμα HDInsight, ανατρέξτε στο θέμα [πυρήνων που είναι διαθέσιμες για τα σημειωματάρια Jupyter με Linux τους Apache συμπλεγμάτων σε HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Τις προϋποθέσεις που παρατίθενται εδώ δεν είναι για την εγκατάσταση του Jupyter. Πρόκειται για τη σύνδεση στο Σημειωματάριο Jupyter σε ένα σύμπλεγμα HDInsight όταν είναι εγκατεστημένο το Σημειωματάριο.

- Μια συνδρομή του Azure. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Ένα σύμπλεγμα Apache τους σε HDInsight Linux. Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία τους Apache συμπλεγμάτων στο Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Εγκατάσταση Jupyter Σημειωματάριο στον υπολογιστή σας

Πρέπει να εγκαταστήσετε Python μπορέσετε να εγκαταστήσετε το Jupyter σημειωματάρια. Python και Jupyter είναι διαθέσιμα ως μέρος της [κατανομής Ananconda](https://www.continuum.io/downloads). Κατά την εγκατάσταση του Anaconda, μπορείτε να εγκαταστήσετε στην πραγματικότητα μια κατανομή των Python. Μόλις εγκαταστήσετε Anaconda, μπορείτε να προσθέσετε την εγκατάσταση Jupyter με την εκτέλεση μιας εντολής. Αυτή η ενότητα παρέχει τις οδηγίες που πρέπει να ακολουθήσετε.

1. Κάντε λήψη του [προγράμματος εγκατάστασης Anaconda](https://www.continuum.io/downloads) για την πλατφόρμα σας και εκτελέστε το πρόγραμμα εγκατάστασης. Κατά την εκτέλεση του οδηγού ρύθμισης, βεβαιωθείτε ότι έχετε επιλέξει την επιλογή για να προσθέσετε Anaconda τη μεταβλητή PATH.

2. Εκτελέστε την παρακάτω εντολή για να εγκαταστήσετε το Jupyter.

        conda install jupyter

    Για περισσότερες πληροφορίες σχετικά με installting Jupyter, ανατρέξτε στο θέμα [Jupyter κατά την εγκατάσταση χρησιμοποιώντας Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Εγκαταστήστε το πυρήνων και μαγικός τους

Για οδηγίες σχετικά με τον τρόπο εγκατάστασης η μαγεία τους, πυρήνων PySpark και τους, ανατρέξτε στην [τεκμηρίωση sparkmagic](https://github.com/jupyter-incubator/sparkmagic#installation) σε GitHub.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Ρύθμιση παραμέτρων μαγικός τους για να αποκτήσετε πρόσβαση στο σύμπλεγμα HDInsight τους

Σε αυτήν την ενότητα μπορείτε να ρυθμίσετε η μαγεία τους που έχετε εγκαταστήσει προηγουμένως για σύνδεση σε ένα σύμπλεγμα Apache τους το οποίο πρέπει να έχετε ήδη δημιουργήσει στο Azure HDInsight.

1. Οι πληροφορίες ρύθμισης παραμέτρων Jupyter συνήθως αποθηκεύονται στον κεντρικό κατάλογο χρήστες. Για να εντοπίσετε αρχικού καταλόγου σας σε οποιαδήποτε πλατφόρμα λειτουργικού Συστήματος, πληκτρολογήστε τις παρακάτω εντολές.

    Ξεκινήστε το κέλυφος Python. Σε ένα παράθυρο εντολών, πληκτρολογήστε τα εξής:

        python

    Στην το κέλυφος Python, πληκτρολογήστε την παρακάτω εντολή για να μάθετε τον κεντρικό κατάλογο.

        import os
        print(os.path.expanduser('~'))

2. Μεταβείτε στον κεντρικό κατάλογο και να δημιουργήσετε ένα φάκελο που ονομάζεται **.sparkmagic** , εάν δεν υπάρχει ήδη.

3. Μέσα στο φάκελο, δημιουργήστε ένα αρχείο που ονομάζεται **config.json** και προσθέστε το παρακάτω τμήμα κώδικα JSON εσωτερικό του.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Αντικαταστήστε **{USERNAME}**, **{CLUSTERDNSNAME}**και **{BASE64ENCODEDPASSWORD}** , με τις κατάλληλες τιμές. Μπορείτε να χρησιμοποιήσετε έναν αριθμό των υπηρεσιών κοινής ωφέλειας την αγαπημένη γλώσσα προγραμματισμού είτε στο Internet για να δημιουργήσετε έναν κωδικό πρόσβασης κωδικοποίηση base64 για τον κωδικό πρόσβασής σας actualy. Πρέπει να είναι ένα απλό απόκομμα Python για να εκτελέσετε από την γραμμή εντολών:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Έναρξη Jupyter. Χρησιμοποιήστε την ακόλουθη εντολή από τη γραμμή εντολών.

        jupyter notebook

6. Βεβαιωθείτε ότι μπορείτε να συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας το Σημειωματάριο Jupyter και που μπορείτε να χρησιμοποιήσετε το διαθέσιμο μαγικός τους με το πυρήνων. Ακολουθήστε τα παρακάτω βήματα.

    1. Δημιουργήστε ένα νέο σημειωματάριο. Από τη δεξιά γωνία, κάντε κλικ στην επιλογή **Δημιουργία**. Θα πρέπει να βλέπετε το προεπιλεγμένο πυρήνα **Python2** και τα δύο νέα πυρήνων που εγκαθιστάτε, **PySpark** και **τους**.

        ![Δημιουργία νέου σημειωματαρίου Jupyter] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Δημιουργία νέου σημειωματαρίου Jupyter")

    
        Κάντε κλικ στην επιλογή **PySpark**.


    2. Εκτελέστε το ακόλουθο τμήμα κώδικα.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Εάν με επιτυχία, μπορείτε να ανακτήσετε τα δεδομένα εξόδου, ελέγχεται η σύνδεσή σας με το σύμπλεγμα HDInsight.

    >[AZURE.TIP] Εάν θέλετε να ενημερώσετε τις ρυθμίσεις παραμέτρων σημειωματάριο για να συνδεθείτε με ένα διαφορετικό σύμπλεγμα, ενημερώστε το config.json με το νέο σύνολο από τιμές, όπως φαίνεται στο παραπάνω βήμα 3. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Γιατί πρέπει να εγκαταστήσω Jupyter στον υπολογιστή μου;

Μπορεί να υπάρχει ένας αριθμός λόγους γιατί μπορεί να θέλετε να εγκαταστήσετε Jupyter στον υπολογιστή σας και, στη συνέχεια, συνδέστε τη με ένα σύμπλεγμα τους σε HDInsight.

* Παρόλο που τα σημειωματάρια Jupyter ήδη είναι διαθέσιμες στο σύμπλεγμα τους στο Azure HDInsight, κατά την εγκατάσταση Jupyter στον υπολογιστή σας παρέχει που την επιλογή για να δημιουργήσετε τα σημειωματάριά σας τοπικά, να δοκιμάσετε την εφαρμογή σας σε σχέση με ένα σύμπλεγμα εκτελείται και, στη συνέχεια, αποστείλετε τα σημειωματάρια στο σύμπλεγμα. Για να αποστείλετε τα σημειωματάρια στο σύμπλεγμα, μπορείτε να αποστείλετε τα χρησιμοποιώντας το Σημειωματάριο Jupyter που εκτελείται ή το σύμπλεγμα ή να τα αποθηκεύσετε στο φάκελο /HdiNotebooks στο λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα. Για περισσότερες πληροφορίες σχετικά με τον τρόπο αποθήκευσης σημειωματαρίων στο σύμπλεγμα, ανατρέξτε στο θέμα [Πού αποθηκεύονται τα σημειωματάρια Jupyter](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored);
* Με τα σημειωματάρια που είναι διαθέσιμα τοπικά, μπορείτε να συνδεθείτε σε διαφορετικό τους συμπλεγμάτων, ανάλογα με τις απαιτήσεις εφαρμογής σας.
* Μπορείτε να χρησιμοποιήσετε GitHub για να υλοποιήσετε ένα σύστημα ελέγχου προέλευσης και να έχουν τον έλεγχο έκδοση για τα σημειωματάρια. Μπορείτε επίσης να έχετε ένα περιβάλλον συνεργασίας όπου πολλούς χρήστες να εργαστείτε με το ίδιο σημειωματάριο.
* Μπορείτε να εργαστείτε με σημειωματάρια τοπικά χωρίς να χρειάζεται ένα σύμπλεγμα προς τα επάνω. Χρειάζεστε μόνο ένα σύμπλεγμα για να ελέγξετε τα σημειωματάριά σας σε σχέση με, όχι, για να διαχειριστείτε με μη αυτόματο τρόπο τα σημειωματάριά σας ή ένα περιβάλλον ανάπτυξης.
* Ίσως είναι ευκολότερο για να ρυθμίσετε το δικό σας περιβάλλον ανάπτυξης τοπικά από ό, τι να ρυθμίσετε τις παραμέτρους της εγκατάστασης Jupyter στο σύμπλεγμα.  Μπορείτε να επωφεληθείτε από όλο το λογισμικό που έχετε εγκαταστήσει τοπικά χωρίς τη ρύθμιση των παραμέτρων ενός ή περισσότερων απομακρυσμένο συμπλεγμάτων.

>[AZURE.WARNING] Με Jupyter εγκατεστημένο στον τοπικό σας υπολογιστή, πολλούς χρήστες να εκτελέσετε στο ίδιο σημειωματάριο στο ίδιο σύμπλεγμα τους την ίδια στιγμή. Σε αυτή την περίπτωση, δημιουργούνται πολλές περίοδοι λειτουργίας Λίβιος. Εάν αντιμετωπίσετε κάποιο πρόβλημα και για τον εντοπισμό σφαλμάτων που θέλετε, θα είναι μια σύνθετη εργασία για τον εντοπισμό ποιες περιόδου λειτουργίας Λίβιος ανήκει σε ποιο χρήστη.




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

* [Χρήση Zeppelin σημειωματάρια με ένα σύμπλεγμα τους σε HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Διαθέσιμο για Jupyter σημειωματαρίου στο σύμπλεγμα τους για HDInsight πυρήνων](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Χρήση εξωτερικών πακέτων με σημειωματάρια Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Διαχείριση πόρων

* [Διαχείριση πόρων για το σύμπλεγμα Apache τους στο Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Παρακολούθηση και ο εντοπισμός σφαλμάτων εργασίες που εκτελείται σε ένα σύμπλεγμα Apache τους στο HDInsight](hdinsight-apache-spark-job-debugging.md)
<properties
   pageTitle="Χρησιμοποιήστε το κέλυφος Hive στο HDInsight (Hadoop) | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το κέλυφος Hive με ένα σύμπλεγμα βάσει Linux HDInsight. Που θα μάθετε πώς μπορείτε να συνδεθείτε με το σύμπλεγμα HDInsight χρησιμοποιώντας SSh, στη συνέχεια, χρησιμοποιήστε το κέλυφος Hive αλληλεπιδραστικά εκτέλεση ερωτημάτων."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Χρήση της ομάδας με Hadoop σε HDInsight με SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε ασφαλούς κελύφους (SSH) για να συνδεθείτε σε σύμπλεγμα Azure HDInsight Hadoop και, στη συνέχεια, αλληλεπιδραστικά υποβολή ερωτημάτων Hive, χρησιμοποιώντας το περιβάλλον γραμμής εντολών Hive (CLI).

> [AZURE.IMPORTANT] Ενώ η εντολή "Hive" είναι διαθέσιμη στο συμπλεγμάτων βάσει Linux HDInsight, θα πρέπει να χρησιμοποιήσετε Beeline. Beeline είναι ένα νεότερο πρόγραμμα-πελάτη για την εργασία με ομάδα και περιλαμβάνεται με το σύμπλεγμά σας HDInsight. Για περισσότερες πληροφορίες σχετικά με αυτό, ανατρέξτε στο θέμα [Χρήση Hive με Hadoop σε HDInsight με Beeline](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight.

* Ένα πρόγραμμα-πελάτη SSH. Linux, Unix και Mac OS, θα πρέπει να διαθέτουν ένα πρόγραμμα-πελάτη SSH. Χρήστες των Windows πρέπει να κάνετε λήψη ενός προγράμματος-πελάτη, όπως [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Σύνδεση με SSH

Σύνδεση με το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του συμπλέγματος HDInsight, χρησιμοποιώντας την εντολή SSH. Το FQDN θα είναι το όνομα που έχετε δώσει σύμπλεγμα, στη συνέχεια, **. azurehdinsight.net**. Για παράδειγμα, το ακόλουθο θα συνδεθείτε σε ένα σύμπλεγμα με το όνομα **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει έναν αριθμό-κλειδί πιστοποιητικό για έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, ίσως χρειαστεί να καθορίσετε τη θέση του ιδιωτικού κλειδιού σε σύστημα υπολογιστή-πελάτη:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει έναν κωδικό πρόσβασης για τον έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, θα πρέπει να εισαγάγετε τον κωδικό πρόσβασης όταν σας ζητηθεί.

Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, OS X, και Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows υπολογιστές-πελάτες)

Τα Windows δεν παρέχει ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται να χρησιμοποιείτε **PuTTY**, που μπορούν να ληφθούν από [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Χρησιμοποιήστε την εντολή ομάδας

2. Μετά τη σύνδεση, ξεκινήστε το CLI Hive, χρησιμοποιώντας την ακόλουθη εντολή:

        hive

3. Χρησιμοποιώντας το CLI, εισαγάγετε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα με το όνομα **log4jLogs** , χρησιμοποιώντας το δείγμα δεδομένων:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

    * **ΑΠΌΘΕΣΗ ΠΊΝΑΚΑ** - διαγράφει τον πίνακα και το αρχείο δεδομένων, σε περίπτωση που ο πίνακας υπάρχει ήδη.
    * **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ ΕΞΩΤΕΡΙΚΏΝ** - δημιουργεί ένα νέο πίνακα 'εξωτερικό' στην ομάδα. Εξωτερικοί πίνακες αποθηκεύουν μόνο τον ορισμό του πίνακα στην ομάδα. Τα δεδομένα είναι προς τα αριστερά στην αρχική τους θέση.
    * **ΜΟΡΦΟΠΟΊΗΣΗ ΓΡΑΜΜΉΣ** - την ομάδα σας ενημερώνει για τον τρόπο μορφοποίησης των δεδομένων. Σε αυτήν την περίπτωση, τα πεδία σε κάθε αρχείο καταγραφής διαχωρίζονται από ένα διάστημα.
    * **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ TEXTFILE ΘΈΣΗ** - ενημερώνει Hive όπου τα δεδομένα που είναι αποθηκευμένη (τον κατάλογο/δεδομένα του παραδείγματος) και να έχει αποθηκευτεί ως κείμενο.
    * **ΕΠΙΛΈΞΤΕ** - επιλέγει μια μέτρηση όλων των γραμμών όπου στήλη **Τ4** περιέχει την τιμή **[ΣΦΆΛΜΑΤΟΣ]**. Αυτό πρέπει να επιστρέψει μια τιμή **3** καθώς υπάρχουν τρεις γραμμές που περιέχουν αυτήν την τιμή.
    * **INPUT__FILE__NAME ΌΠΩΣ '%.log'** - σας ενημερώνει για την ομάδα που θα σας πρέπει να επιστρέφει μόνο δεδομένα από αρχεία που τελειώνει σε. καταγραφής. Περιορίζει την αναζήτηση στο αρχείο sample.log που περιέχει τα δεδομένα, και επιστρέφει δεδομένα από άλλα παράδειγμα αρχεία δεδομένων που δεν συμφωνεί με το σχήμα ορίσαμε.

    > [AZURE.NOTE] Εξωτερικοί πίνακες πρέπει να χρησιμοποιείται όταν αναμένετε τα υποκείμενα δεδομένα για να ενημερωθούν από μια εξωτερική προέλευση, όπως μια διεργασία αποστολής αυτοματοποιημένη δεδομένων ή με μια άλλη MapReduce λειτουργία, αλλά θέλετε πάντα Hive ερωτημάτων για να χρησιμοποιήσετε τα πιο πρόσφατα δεδομένα.
    >
    > Απόθεση έναν εξωτερικό πίνακα κάνει **διαγράφει τα δεδομένα, μόνο τον ορισμό του πίνακα** .

4. Χρησιμοποιήστε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα 'εσωτερικού' με το όνομα **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

    * **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑΣ IF δεν ΥΠΆΡΧΕΙ** - δημιουργεί έναν πίνακα, εάν δεν υπάρχει ήδη. Εφόσον δεν χρησιμοποιείται το **ΕΞΩΤΕΡΙΚΌ** λέξεων-κλειδιών, αυτή είναι μια εσωτερική πίνακα, το οποίο είναι αποθηκευμένο στην ομάδα αποθήκη δεδομένων και γίνεται εντελώς από ομάδα.
    * **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ ORC** - αποθηκεύει τα δεδομένα σε μορφή βελτιστοποιημένη σε στήλες γραμμής (ORC). Αυτή είναι μια μορφή ιδιαίτερα βελτιστοποιημένη και αποτελεσματική για την αποθήκευση δεδομένων ομάδας.
    * ΑΝΤΙΚΑΤΆΣΤΑΣΗ **Εισαγωγή... ΕΠΙΛΈΞΤΕ** - επιλέγει γραμμές από τον πίνακα **log4jLogs** που περιέχουν **[ΣΦΆΛΜΑΤΟΣ]**και, στη συνέχεια, εισάγει τα δεδομένα στον πίνακα **errorLogs** .

    Για να επαληθεύσετε ότι έχουν αποθηκευτεί μόνο τις γραμμές που περιέχουν **[ΣΦΆΛΜΑΤΟΣ]** σε Τ4 στήλης στον πίνακα **errorLogs** , χρησιμοποιήστε την ακόλουθη πρόταση για να επιστρέψει όλες τις γραμμές από **errorLogs**:

        SELECT * from errorLogs;

    Τρεις γραμμές δεδομένων πρέπει να επιστρέφεται, όλα περιέχει **[ΣΦΆΛΜΑΤΟΣ]** στο Τ4 στήλης.

    > [AZURE.NOTE] Σε αντίθεση με εξωτερική πίνακες, απόθεση ενός πίνακα του εσωτερικού θα διαγράψει καθώς και τα υποκείμενα δεδομένα.

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε, η εντολή ομάδα αποτελούν έναν εύκολο τρόπο για τη διαδραστική εκτέλεση ερωτημάτων ομάδας σε ένα σύμπλεγμα HDInsight, παρακολουθείτε την κατάσταση της εργασίας και ανακτήστε τα δεδομένα εξόδου.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με ομάδα στο HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

Εάν χρησιμοποιείτε Tez με ομάδα, δείτε τα ακόλουθα έγγραφα για τον εντοπισμό σφαλμάτων πληροφορίες:

* [Χρήση του περιβάλλοντος εργασίας Χρήστη του Tez σε HDInsight που βασίζεται σε Windows](hdinsight-debug-tez-ui.md)

* [Χρήση της προβολής Ambari Tez σε βάσει Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png


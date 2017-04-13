<properties
   pageTitle="Χρήση Hadoop Hive και απομακρυσμένης επιφάνειας εργασίας σε HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να συνδεθείτε με σύμπλεγμα Hadoop στο HDInsight, χρησιμοποιώντας σύνδεση απομακρυσμένης επιφάνειας εργασίας και, στη συνέχεια, εκτελέστε τα ερωτήματα Hive, χρησιμοποιώντας το περιβάλλον γραμμής εντολών Hive."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Χρήση της ομάδας με Hadoop σε HDInsight με σύνδεση απομακρυσμένης επιφάνειας εργασίας

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να συνδεθείτε σε ένα σύμπλεγμα HDInsight, χρησιμοποιώντας σύνδεση απομακρυσμένης επιφάνειας εργασίας και, στη συνέχεια, εκτελέστε Hive ερωτήματα χρησιμοποιώντας το Hive περιβάλλον γραμμής εντολών (CLI).

> [AZURE.NOTE] Αυτό το έγγραφο δεν παρέχει μια λεπτομερή περιγραφή του τι κάνουν τις προτάσεις HiveQL που χρησιμοποιούνται σε παραδείγματα. Για πληροφορίες σχετικά με το HiveQL που χρησιμοποιείται σε αυτό το παράδειγμα, ανατρέξτε στο θέμα [Χρήση Hive με Hadoop σε HDInsight](hdinsight-use-hive.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Ένα σύμπλεγμα HDInsight που βασίζεται σε Windows (Hadoop σε HDInsight)

* Πρόγραμμα-πελάτη υπολογιστή που εκτελεί Windows 10, το παράθυρο 8 ή Windows 7

##<a id="connect"></a>Σύνδεση με απομακρυσμένη επιφάνεια εργασίας

Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα HDInsight και, στη συνέχεια, συνδεθείτε, ακολουθώντας τις οδηγίες στην ενότητα [σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hive"></a>Χρησιμοποιήστε την εντολή ομάδας

Όταν έχετε συνδέσει στην επιφάνεια εργασίας για το σύμπλεγμα HDInsight, χρησιμοποιήστε τα παρακάτω βήματα για να εργαστείτε με ομάδα:

1. Από την επιφάνεια εργασίας HDInsight, ξεκινήστε τη **γραμμή εντολών Hadoop**.

2. Πληκτρολογήστε την παρακάτω εντολή για να ξεκινήσετε το CLI Hive:

        %hive_home%\bin\hive

    Όταν έχει ξεκινήσει η CLI, θα δείτε το μήνυμα Hive CLI: `hive>`.

3. Χρησιμοποιώντας το CLI, εισαγάγετε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα με το όνομα **log4jLogs** με δείγμα δεδομένων:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

    * **ΑΠΌΘΕΣΗ ΠΊΝΑΚΑ**: Διαγράφει τον πίνακα και το αρχείο δεδομένων, εάν ο πίνακας υπάρχει ήδη.

    * **ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΉΣ ΠΊΝΑΚΑ**: δημιουργεί ένα νέο πίνακα 'εξωτερικό' στην ομάδα. Εξωτερικοί πίνακες αποθηκεύουν μόνο τον ορισμό του πίνακα, στην ομάδα (τα δεδομένα είναι προς τα αριστερά στην αρχική τους θέση).

        > [AZURE.NOTE] Εξωτερικοί πίνακες πρέπει να χρησιμοποιείται όταν θεωρείτε ότι τα υποκείμενα δεδομένα για να ενημερωθούν από μια εξωτερική προέλευση (όπως μια διεργασία αποστολής αυτοματοποιημένη δεδομένων) ή από μια άλλη λειτουργία MapReduce, αλλά θέλετε πάντα Hive ερωτημάτων για να χρησιμοποιήσετε τα πιο πρόσφατα δεδομένα.
        >
        > Απόθεση έναν εξωτερικό πίνακα κάνει **διαγράφει τα δεδομένα, μόνο τον ορισμό του πίνακα** .

    * **ΜΟΡΦΟΠΟΊΗΣΗ ΓΡΑΜΜΉΣ**: ενημερώνει Hive τον τρόπο μορφοποίησης των δεδομένων. Σε αυτήν την περίπτωση, τα πεδία σε κάθε αρχείο καταγραφής διαχωρίζονται από ένα διάστημα.

    * **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ TEXTFILE ΘΈΣΗ**: ενημερώνει Hive όπου τα δεδομένα που είναι αποθηκευμένα (τον κατάλογο/δεδομένα του παραδείγματος) και που είναι αποθηκευμένο ως κείμενο.

    * **ΕΠΙΛΈΞΤΕ**: επιλέγει μια μέτρηση όλων των γραμμών όπου στήλη **Τ4** περιέχει την τιμή **[ΣΦΆΛΜΑΤΟΣ]**. Αυτό πρέπει να επιστρέψει μια τιμή **3** , επειδή υπάρχουν τρεις γραμμές που περιέχουν αυτήν την τιμή.

    * **INPUT__FILE__NAME ΌΠΩΣ '%.log'** - σας ενημερώνει για την ομάδα που θα σας πρέπει να επιστρέφει μόνο δεδομένα από αρχεία που τελειώνει σε. καταγραφής. Περιορίζει την αναζήτηση στο αρχείο sample.log που περιέχει τα δεδομένα, και επιστρέφει δεδομένα από άλλα παράδειγμα αρχεία δεδομένων που δεν συμφωνεί με το σχήμα ορίσαμε.


4. Χρησιμοποιήστε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα 'εσωτερικού' με το όνομα **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

    * **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑΣ IF δεν ΥΠΆΡΧΕΙ**: δημιουργεί έναν πίνακα, εάν δεν υπάρχει ήδη. Επειδή δεν χρησιμοποιείται η **ΕΞΩΤΕΡΙΚΉ** λέξεων-κλειδιών, αυτή είναι μια εσωτερική πίνακα, το οποίο είναι αποθηκευμένο στην ομάδα αποθήκη δεδομένων και γίνεται εντελώς από ομάδα.

        > [AZURE.NOTE] Σε αντίθεση με **ΕΞΩΤΕΡΙΚΉ** πίνακες, απόθεση ενός πίνακα του εσωτερικού διαγράφει επίσης τα υποκείμενα δεδομένα.

    * **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ ORC**: αποθηκεύει τα δεδομένα σε μορφή στήλης (ORC) βελτιστοποιημένη γραμμή. Αυτή είναι μια μορφή ιδιαίτερα βελτιστοποιημένη και αποτελεσματική για την αποθήκευση δεδομένων ομάδας.

    * ΑΝΤΙΚΑΤΆΣΤΑΣΗ **Εισαγωγή... ΕΠΙΛΈΞΤΕ**: επιλέγει γραμμές από τον πίνακα **log4jLogs** που περιέχουν **[ΣΦΆΛΜΑΤΟΣ]**, στη συνέχεια, εισάγει τα δεδομένα στον πίνακα **errorLogs** .

    Για να επαληθεύσετε ότι μόνο τις γραμμές που περιέχουν **[ΣΦΆΛΜΑΤΟΣ]** στη στήλη Τ4 έχουν αποθηκευτεί στον πίνακα **errorLogs** , χρησιμοποιήστε την ακόλουθη πρόταση για να επιστρέψει όλες τις γραμμές από **errorLogs**:

        SELECT * from errorLogs;

    Τρεις γραμμές δεδομένων πρέπει να επιστρέφεται, όλα περιέχει **[ΣΦΆΛΜΑΤΟΣ]** στο Τ4 στήλης.

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε το στην εντολή ομάδα αποτελούν έναν εύκολο τρόπο για τη διαδραστική εκτέλεση ερωτημάτων ομάδας σε ένα σύμπλεγμα HDInsight, παρακολουθείτε την κατάσταση της εργασίας και ανακτήσετε το αποτέλεσμα.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με την ομάδα του HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

Εάν χρησιμοποιείτε Tez με ομάδα, δείτε τα ακόλουθα έγγραφα για τον εντοπισμό σφαλμάτων πληροφορίες:

* [Χρήση του περιβάλλοντος εργασίας Χρήστη του Tez σε HDInsight που βασίζεται σε Windows](hdinsight-debug-tez-ui.md)

* [Χρήση της προβολής Ambari Tez σε βάσει Linux HDInsight](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


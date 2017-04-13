<properties
    pageTitle="Μάθετε τι είναι η ομάδα και πώς μπορείτε να χρησιμοποιήσετε HiveQL | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με Apache ομάδας και πώς μπορείτε να το χρησιμοποιήσετε με Hadoop σε HDInsight. Επιλέξτε τον τρόπο για να εκτελέσετε την ομάδα εργασίας, και χρησιμοποιήστε HiveQL για να αναλύσετε ένα δείγμα αρχείου log4j Apache."
    keywords="hiveql, τι είναι η ομάδα"
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
    ms.date="09/19/2016"
    ms.author="larryfr"/>

# <a name="use-hive-and-hiveql-with-hadoop-in-hdinsight-to-analyze-a-sample-apache-log4j-file"></a>Χρήση της ομάδας και HiveQL με Hadoop σε HDInsight για να αναλύσετε ένα δείγμα αρχείου log4j Apache

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Σε αυτό το πρόγραμμα εκμάθησης, θα μάθετε πώς να χρησιμοποιείτε Apache Hive στο Hadoop σε HDInsight και επιλέξτε τον τρόπο για να εκτελέσετε την ομάδα εργασίας. Θα μάθετε επίσης σχετικά με HiveQL και πώς μπορείτε να αναλύσετε ένα δείγμα αρχείου log4j Apache.

##<a id="why"></a>Τι είναι η ομάδα και γιατί το χρησιμοποιήσω;
[Apache Hive](http://hive.apache.org/) είναι ένα σύστημα αποθήκη δεδομένων για Hadoop, που σας επιτρέπει σύνοψη δεδομένων, υποβολή ερωτημάτων και ανάλυση των δεδομένων με τη χρήση HiveQL (μια γλώσσα ερωτημάτων παρόμοια με SQL). Ομάδα μπορεί να χρησιμοποιηθεί για να αλληλεπιδραστικά Εξερεύνηση των δεδομένων σας ή για να δημιουργήσετε με δυνατότητα επανάληψης χρήσης μαζική επεξεργασία εργασίες.

Ομάδα σάς επιτρέπει να δομή έργου σε μεγάλο βαθμό μη δομημένα δεδομένα. Αφού ορίσετε τη δομή, μπορείτε να χρησιμοποιήσετε Hive ερώτημα για αυτά τα δεδομένα χωρίς να γνωρίζει Java ή MapReduce. **HiveQL** (τη γλώσσα ερωτήματος Hive) σάς επιτρέπει να γράφετε ερωτημάτων με προτάσεις που είναι παρόμοια με Τ-SQL.

Ομάδα κατανοούν τον τρόπο εργασίας με δομημένα και ημι-δομημένα δεδομένα, όπως αρχεία κειμένου όπου τα πεδία έχουν οριοθετημένο από συγκεκριμένους χαρακτήρες. Ομάδα υποστηρίζει επίσης προσαρμοσμένο **πρόγραμμα σειριοποίησης/deserializers (SerDe)** για σύνθετη ή ακανόνιστο δομημένα δεδομένα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε ένα προσαρμοσμένο SerDe JSON με HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx).

## <a name="user-defined-functions-udf"></a>Οι συναρτήσεις που ορίζονται από το χρήστη (UDF)

Ομάδα μπορεί επίσης να επεκταθεί στις **συναρτήσεις που ορίζονται από το χρήστη (UDF)**. Ένα αρχείο UDF σάς επιτρέπει να εφαρμόζετε λειτουργικότητα ή λογικής που δεν είναι εύκολα μοντελοποιηθεί στον HiveQL. Για ένα παράδειγμα της χρήσης UDF με ομάδα, ανατρέξτε στα παρακάτω:

* [Χρήση ενός χρήστη Java που ορίζονται από το συνάρτηση με ομάδα](hdinsight-hadoop-hive-java-udf.md)

* [Χρήση Python με ομάδα και γουρούνι σε HDInsight](hdinsight-python.md)

* [Χρήση C# με την ομάδα και γουρούνι στο HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Πώς μπορείτε να προσθέσετε ένα προσαρμοσμένο UDF Hive με το HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Προσαρμοσμένη Hive UDF παράδειγμα για τη μετατροπή μορφές ημερομηνίας/ώρας σε ομάδα χρονικής σήμανσης](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="hive-internal-tables-vs-external-tables"></a>Hive εξωτερικοί πίνακες και στο εσωτερικό πίνακες

Υπάρχουν μερικά πράγματα που πρέπει να γνωρίζετε σχετικά με την ομάδα εσωτερικό πίνακα και εξωτερικό πίνακα:

- Η εντολή **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ** δημιουργεί έναν πίνακα του εσωτερικού. Το αρχείο δεδομένων πρέπει να βρίσκεται στο προεπιλεγμένο κοντέινερ.
- Η εντολή **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ** μετακινεί το αρχείο δεδομένων του /hive/αποθήκης/<TableName> φακέλου.
- Η εντολή **ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΏΝ ΠΊΝΑΚΑ** δημιουργεί έναν εξωτερικό πίνακα. Το αρχείο δεδομένων μπορεί να βρίσκεται έξω από το προεπιλεγμένο κοντέινερ.
- Η εντολή **ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌΣ ΠΊΝΑΚΑΣ** δεν μετακινείται στο αρχείο δεδομένων.
- Η εντολή **ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌΣ ΠΊΝΑΚΑΣ** δεν σας επιτρέπει των φακέλων στη ΘΈΣΗ. Αυτή είναι η αιτία γιατί το πρόγραμμα εκμάθησης δημιουργεί ένα αντίγραφο του αρχείου sample.log.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [HDInsight: Hive εσωτερικές και εξωτερικές Εισαγωγή πινάκων][cindygross-hive-tables].


##<a id="data"></a>Σχετικά με το δείγμα δεδομένων, ένα αρχείο log4j Apache

Σε αυτό το παράδειγμα χρησιμοποιεί ένα δείγμα αρχείου *log4j* , η οποία είναι αποθηκευμένη στο **/example/data/sample.log** σας κοντέινερ χώρου αποθήκευσης αντικειμένων blob. Κάθε αρχείο καταγραφής εντός του αρχείου που αποτελείται από μια γραμμή των πεδίων που περιέχει ένα `[LOG LEVEL]` πεδίο για να εμφανίσετε τον τύπο και της σοβαρότητας, για παράδειγμα:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

Στο προηγούμενο παράδειγμα, το επίπεδο καταγραφής είναι σφάλμα.

> [AZURE.NOTE] Μπορείτε επίσης να δημιουργήσετε ένα αρχείο log4j με χρήση του εργαλείου [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) καταγραφή και, στη συνέχεια, αποστείλετε το αρχείο στο κοντέινερ αντικειμένων blob. Για οδηγίες, ανατρέξτε στο θέμα [Αποστολή δεδομένων με το HDInsight](hdinsight-upload-data.md) . Για περισσότερες πληροφορίες σχετικά με τον τρόπο χρήσης χώρος αποθήκευσης αντικειμένων Blob του Azure με το HDInsight, ανατρέξτε στο θέμα [Χρήση χώρο αποθήκευσης αντικειμένων Blob Azure με HDInsight](hdinsight-hadoop-use-blob-storage.md).

Το δείγμα δεδομένων είναι αποθηκευμένη στο χώρο αποθήκευσης αντικειμένων Blob του Azure, που χρησιμοποιείται από το HDInsight ως το προεπιλεγμένο σύστημα αρχείων. HDInsight να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα σε αντικείμενα blob, χρησιμοποιώντας το πρόθεμα **wasb** . Για παράδειγμα, για να αποκτήσετε πρόσβαση στο αρχείο sample.log, πρέπει να χρησιμοποιήσετε την παρακάτω σύνταξη:

    wasbs:///example/data/sample.log

Επειδή το χώρο αποθήκευσης αντικειμένων Blob του Azure είναι το προεπιλεγμένο αποθήκευσης για HDInsight, μπορείτε επίσης να αποκτήσετε πρόσβαση το αρχείο με τη χρήση **/example/data/sample.log** από HiveQL.

> [AZURE.NOTE] Η σύνταξη της, **wasbs: / / /**, χρησιμοποιείται για να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα στο προεπιλεγμένο κοντέινερ χώρου αποθήκευσης για το σύμπλεγμα HDInsight. Αν έχετε καθορίσει τους λογαριασμούς επιπλέον χώρου αποθήκευσης όταν παρασχεθεί το σύμπλεγμά σας και θέλετε να αποκτήσετε πρόσβαση σε αρχεία που είναι αποθηκευμένα σε αυτούς τους λογαριασμούς, μπορείτε να αποκτήσετε πρόσβαση στα δεδομένα, καθορίζοντας τη κοντέινερ όνομα και αποθήκευσης λογαριασμού διεύθυνση, για παράδειγμα, **wasbs://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**.

##<a id="job"></a>Δείγμα εργασία: Project στήλες σε οριοθετημένα δεδομένων

Οι παρακάτω προτάσεις HiveQL θα έργου στήλες σε οριοθετημένου με δεδομένα που είναι αποθηκευμένα στο το **wasbs: / / / / δεδομένα του παραδείγματος** καταλόγου:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Στο προηγούμενο παράδειγμα, τις προτάσεις HiveQL εκτελέστε τις ακόλουθες ενέργειες:

* __Ορισμός hive.execution.engine=tez;__: Ορίζει το μηχανισμό εκτέλεσης για να χρησιμοποιήσετε Tez. Χρήση Tez αντί για MapReduce μπορούν να παρέχουν μια αύξηση στις επιδόσεις ερωτημάτων. Για περισσότερες πληροφορίες σχετικά με Tez, ανατρέξτε στην ενότητα [Χρήση Apache Tez για βελτιωμένη απόδοση](#usetez) .

    > [AZURE.NOTE] Αυτή η πρόταση είναι μόνο απαιτείται όταν χρησιμοποιείτε ένα σύμπλεγμα HDInsight που βασίζεται στα Windows; Tez είναι ο προεπιλεγμένος μηχανισμός εκτέλεσης για βάσει Linux HDInsight.

* **ΑΠΌΘΕΣΗ ΠΊΝΑΚΑ**: Διαγράφει τον πίνακα και το αρχείο δεδομένων, εάν ο πίνακας υπάρχει ήδη.
* **ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΉΣ ΠΊΝΑΚΑ**: δημιουργείται ένας νέος πίνακας **εξωτερικών** στην ομάδα. Εξωτερικοί πίνακες αποθηκεύουν μόνο τον ορισμό του πίνακα στην ομάδα; τα δεδομένα είναι προς τα αριστερά στην αρχική τους θέση και στην αρχική μορφή.
* **ΜΟΡΦΟΠΟΊΗΣΗ ΓΡΑΜΜΉΣ**: ενημερώνει Hive τον τρόπο μορφοποίησης των δεδομένων. Σε αυτήν την περίπτωση, τα πεδία σε κάθε αρχείο καταγραφής διαχωρίζονται από ένα διάστημα.
* **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ TEXTFILE ΘΈΣΗ**: ενημερώνει Hive όπου τα δεδομένα που είναι αποθηκευμένα (τον κατάλογο/δεδομένα του παραδείγματος) και που είναι αποθηκευμένο ως κείμενο. Τα δεδομένα να είναι σε ένα αρχείο ή να εκτείνεται σε πολλαπλά αρχεία μέσα στον κατάλογο.
* **ΕΠΙΛΈΞΤΕ**: επιλέγει μια μέτρηση όλων των γραμμών όπου η στήλη **Τ4** περιέχει την τιμή **[ΣΦΆΛΜΑΤΟΣ]**. Αυτό πρέπει να επιστρέψει μια τιμή **3** , επειδή υπάρχουν τρεις γραμμές που περιέχουν αυτήν την τιμή.
* **INPUT__FILE__NAME ΌΠΩΣ '%.log'** - σας ενημερώνει για την ομάδα που θα σας πρέπει να επιστρέφει μόνο δεδομένα από αρχεία που τελειώνει σε. καταγραφής. Περιορίζει την αναζήτηση στο αρχείο sample.log που περιέχει τα δεδομένα, και επιστρέφει δεδομένα από άλλα παράδειγμα αρχεία δεδομένων που δεν συμφωνεί με το σχήμα ορίσαμε.

> [AZURE.NOTE] Εξωτερικοί πίνακες πρέπει να χρησιμοποιείται όταν θεωρείτε ότι τα υποκείμενα δεδομένα για να ενημερωθούν από μια εξωτερική προέλευση, όπως μια διεργασία αποστολής αυτοματοποιημένο δεδομένων, ή από μια άλλη λειτουργία MapReduce και θέλετε πάντα Hive ερωτημάτων για να χρησιμοποιήσετε τα πιο πρόσφατα δεδομένα.
>
> Απόθεση έναν εξωτερικό πίνακα κάνει **δεν** διαγράφει τα δεδομένα, διαγράφεται μόνο τον ορισμό του πίνακα.

Αφού δημιουργήσετε το εξωτερικό πίνακα, τις παρακάτω προτάσεις που χρησιμοποιούνται για τη δημιουργία ενός πίνακα του **εσωτερικού** .

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

* **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑΣ IF δεν ΥΠΆΡΧΕΙ**: δημιουργεί έναν πίνακα, εάν δεν υπάρχει ήδη. Επειδή δεν χρησιμοποιείται η **ΕΞΩΤΕΡΙΚΉ** λέξεων-κλειδιών, αυτή είναι μια εσωτερική πίνακα, το οποίο είναι αποθηκευμένο στην ομάδα αποθήκη δεδομένων και γίνεται εντελώς από ομάδα.
* **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ ORC**: αποθηκεύει τα δεδομένα σε μορφή βελτιστοποιημένη σε στήλες γραμμής (ORC). Αυτή είναι μια μορφή ιδιαίτερα βελτιστοποιημένη και αποτελεσματική για την αποθήκευση δεδομένων ομάδας.
* ΑΝΤΙΚΑΤΆΣΤΑΣΗ **Εισαγωγή... ΕΠΙΛΈΞΤΕ**: επιλέγει γραμμές από τον πίνακα **log4jLogs** που περιέχει **[σφάλμα]**και, στη συνέχεια, εισάγει τα δεδομένα στον πίνακα **errorLogs** .

> [AZURE.NOTE] Σε αντίθεση με εξωτερική πίνακες, απόθεση ενός πίνακα του εσωτερικού διαγράφει επίσης τα υποκείμενα δεδομένα.

##<a id="usetez"></a>Χρήση Apache Tez για βελτιωμένες επιδόσεις

[Apache Tez](http://tez.apache.org) είναι ένα πλαίσιο που επιτρέπει την εντατική εφαρμογές δεδομένων, όπως η ομάδα, για να εκτελέσετε πιο αποδοτικά με κλίμακα. Στην πιο πρόσφατη έκδοση του HDInsight, Hive υποστηρίζει εκτελείται σε Tez. Tez είναι ενεργοποιημένη από προεπιλογή για συμπλεγμάτων βάσει Linux HDInsight.

> [AZURE.NOTE] Tez είναι προς το παρόν απενεργοποιημένη από προεπιλογή για συμπλεγμάτων HDInsight που βασίζεται σε Windows και πρέπει να είναι ενεργοποιημένη. Για να επωφεληθείτε από Tez, πρέπει να οριστούν την ακόλουθη τιμή για ένα ερώτημα ομάδας:
>
> ```set hive.execution.engine=tez;```
>
>Αυτό μπορούν να υποβληθούν σε βάση ανά ερώτημα τοποθετώντας το στην αρχή του ερωτήματός σας. Μπορείτε επίσης να ορίσετε έτσι ώστε να είναι από προεπιλογή σε ένα σύμπλεγμα, ορίζοντας την τιμή της ρύθμισης παραμέτρων κατά τη δημιουργία του συμπλέγματος. Μπορείτε να βρείτε περισσότερες λεπτομέρειες σε [Προμήθεια συμπλεγμάτων HDInsight](hdinsight-provision-clusters.md).

Το [Hive σε Tez σχεδίαση έγγραφα](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) περιέχουν έναν αριθμό λεπτομέρειες σχετικά με τις επιλογές υλοποίησης και ρύθμισης παραμέτρων.

Για να βοηθήσει τον εντοπισμό σφαλμάτων εργασίες εκτελέσατε χρησιμοποιώντας Tez, HDInsight παρέχει παρακάτω web περιβάλλοντα εργασίας χρήστη που σας επιτρέπουν να προβάλετε τις λεπτομέρειες της Tez εργασίες:

* [Χρήση του περιβάλλοντος εργασίας Χρήστη του Tez σε HDInsight που βασίζεται σε Windows](hdinsight-debug-tez-ui.md)

* [Χρήση της προβολής Ambari Tez σε HDInsight που βασίζεται στο Linux](hdinsight-debug-ambari-tez-view.md)

##<a id="run"></a>Επιλέξτε τον τρόπο για να εκτελέσετε την εργασία HiveQL

HDInsight να εκτελέσετε εργασίες HiveQL χρησιμοποιώντας διάφορες μεθόδους. Χρησιμοποιήστε τον παρακάτω πίνακα για να αποφασίσετε ποια μέθοδο είναι κατάλληλη για εσάς και, στη συνέχεια, ακολουθήστε τη σύνδεση για αναλυτικές οδηγίες.

| **Χρησιμοποιήστε αυτήν την επιλογή** εάν θέλετε...                                                     | .. .an **αλληλεπιδραστικών** κελύφους | ... Επεξεργασία **δέσμης** | .. .with αυτό το **σύμπλεγμα λειτουργικό σύστημα** | .. .from αυτό το **πρόγραμμα-πελάτη λειτουργικό σύστημα** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Προβολή της ομάδας](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | Οποιαδήποτε (πρόγραμμα περιήγησης με βάση) |
| [Εντολή beeline (από μια περίοδο λειτουργίας SSH)](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ή των Windows        |
| [Εντολή ομάδα (από μια περίοδο λειτουργίας SSH)](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X ή των Windows        |
| [Καμπύλη](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Linux, Unix, Mac OS X ή των Windows        |
| [Κονσόλα ερωτήματος](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | Οποιαδήποτε (πρόγραμμα περιήγησης με βάση)                            |
| [Εργαλεία HDInsight για το Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux ή στα Windows                          | Windows                                  |
| [Σύνδεση απομακρυσμένης επιφάνειας εργασίας](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## <a name="running-hive-jobs-on-azure-hdinsight-using-on-premises-sql-server-integration-services"></a>Εκτέλεση εργασιών ομάδας σε Azure HDInsight με χρήση εσωτερικής εγκατάστασης των υπηρεσιών ενοποίησης του SQL Server

Μπορείτε επίσης να χρησιμοποιήσετε τις υπηρεσίες ενοποίησης SQL Server (SSIS) για να εκτελέσετε μια εργασία της ομάδας. Το πακέτο δυνατοτήτων Azure για SSIS παρέχει τα παρακάτω στοιχεία που λειτουργούν με εργασίες ομάδας σε HDInsight.


- [Azure HDInsight ομάδα εργασίας][hivetask]
- [Διαχείριση συνδέσεων Azure συνδρομή][connectionmanager]


Μάθετε περισσότερα σχετικά με το πακέτο δυνατοτήτων Azure για SSIS [εδώ][ssispack].


##<a id="nextsteps"></a>Επόμενα βήματα

Τώρα που μάθατε ομάδα είναι και πώς μπορείτε να το χρησιμοποιήσετε με Hadoop στο HDInsight, χρησιμοποιήστε τις παρακάτω συνδέσεις για να εξερευνήσετε άλλους τρόπους για να εργαστείτε με Azure HDInsight.


- [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
- [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
- [Χρήση Sqoop με HDInsight](hdinsight-use-sqoop.md)
- [Χρήση Oozie με HDInsight](hdinsight-use-oozie.md)
- [Χρήση MapReduce εργασίες με το HDInsight][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-get-started.md

[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

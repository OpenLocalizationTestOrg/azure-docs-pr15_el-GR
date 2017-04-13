<properties
   pageTitle="Χρήση Beeline για εργασία με ομάδα στο HDInsight (Hadoop) | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε SSH για να συνδεθείτε σε ένα σύμπλεγμα Hadoop στο HDInsight, και, στη συνέχεια, αλληλεπιδραστικά υποβολή ερωτημάτων Hive χρησιμοποιώντας Beeline. Beeline είναι ένα βοηθητικό πρόγραμμα για την εργασία με HiveServer2 μέσω JDBC."
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
   ms.date="10/10/2016"
   ms.author="larryfr"/>

#<a name="use-hive-with-hadoop-in-hdinsight-with-beeline"></a>Χρήση της ομάδας με Hadoop σε HDInsight με Beeline

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε ασφαλούς κελύφους (SSH) για να συνδεθείτε σε ένα σύμπλεγμα βάσει Linux HDInsight και, στη συνέχεια, αλληλεπιδραστικά υποβολή ερωτημάτων Hive, χρησιμοποιώντας το εργαλείο γραμμής εντολών [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) .

> [AZURE.NOTE] Beeline χρησιμοποιεί JDBC για να συνδεθείτε με την ομάδα. Για περισσότερες πληροφορίες σχετικά με τη χρήση JDBC με ομάδα, ανατρέξτε στο θέμα [σύνδεση σε ομάδα στο Azure HDInsight χρησιμοποιώντας το πρόγραμμα οδήγησης Hive JDBC](hdinsight-connect-hive-jdbc-driver.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight.

* Ένα πρόγραμμα-πελάτη SSH. Linux, Unix και Mac OS, θα πρέπει να διαθέτουν ένα πρόγραμμα-πελάτη SSH. Χρήστες των Windows πρέπει να κάνετε λήψη ενός προγράμματος-πελάτη, όπως [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Σύνδεση με SSH

Σύνδεση με το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του συμπλέγματος HDInsight, χρησιμοποιώντας την εντολή SSH. Το FQDN θα είναι το όνομα που έχετε δώσει σύμπλεγμα, στη συνέχεια, **. azurehdinsight.net**. Για παράδειγμα, το ακόλουθο θα συνδεθείτε σε ένα σύμπλεγμα με το όνομα **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει έναν αριθμό-κλειδί πιστοποιητικό για έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, ίσως χρειαστεί να καθορίσετε τη θέση του ιδιωτικού κλειδιού σε σύστημα υπολογιστή-πελάτη:

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Εάν έχετε δώσει έναν κωδικό πρόσβασης για τον έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, θα πρέπει να εισαγάγετε τον κωδικό πρόσβασης όταν σας ζητηθεί.

Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, OS X, και Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (Windows υπολογιστές-πελάτες)

Τα Windows δεν παρέχει ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται να χρησιμοποιείτε **PuTTY**, που μπορούν να ληφθούν από [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="beeline"></a>Χρησιμοποιήστε την εντολή Beeline

1. Μόλις συνδεθεί, χρησιμοποιήστε τα ακόλουθα για να ξεκινήσετε Beeline:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Αυτό θα ξεκινήσει ο υπολογιστής-πελάτης Beeline, και να συνδεθείτε με τη διεύθυνση url JDBC. Εδώ, `localhost` χρησιμοποιείται εφόσον HiveServer2 εκτελείται σε δύο κεφαλών κόμβους του συμπλέγματος και θα σας χρησιμοποιείτε Beeline απευθείας σε το πρωτεύον headnode.
    
    Μόλις ολοκληρωθεί η εντολή, θα μεταβείτε σε μια `jdbc:hive2://localhost:10001/>` ερώτηση.

3. Εντολές beeline συνήθως ξεκινούν με μια `!` χαρακτήρας, για παράδειγμα `!help` εμφανίζει πληροφορίες Βοήθειας. Ωστόσο, το `!` συχνά μπορεί να παραλειφθεί. Για παράδειγμα, `help` λειτουργούν επίσης.

    Εάν προβάλλετε Βοήθεια, θα παρατηρήσετε `!sql`, που χρησιμοποιείται για την εκτέλεση HiveQL δηλώσεις. Ωστόσο, HiveQL επομένως χρησιμοποιείται συνήθως ότι μπορείτε να παραλείψετε το προηγούμενο `!sql`. Οι παρακάτω δύο προτάσεις έχετε ακριβώς τα ίδια αποτελέσματα; Εμφάνιση των πινάκων προς το παρόν διαθέσιμες μέσω της ομάδας:
    
        !sql show tables;
        show tables;
    
    Σε ένα νέο σύμπλεγμα, θα πρέπει να παρατίθενται μόνο έναν πίνακα: __hivesampletable__.

4. Χρησιμοποιήστε τα ακόλουθα για να εμφανίσετε το σχήμα για το hivesampletable:

        describe hivesampletable;
        
    Αυτό θα επιστρέψει τις ακόλουθες πληροφορίες:
    
        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Αυτό εμφανίζει τις στήλες του πίνακα. Ενώ θα μπορούσε να μπορούμε να πραγματοποιήσουμε ορισμένα ερωτήματα σε σχέση με αυτά τα δεδομένα, αντί για αυτό Δημιουργήστε ολοκαίνουργιο πίνακα δείχνουν πώς μπορείτε να φορτώσετε δεδομένα σε ομάδα και να εφαρμόσετε ένα σχήμα.
    
5. Εισαγάγετε τις παρακάτω προτάσεις για να δημιουργήσετε έναν νέο πίνακα με το όνομα **log4jLogs** χρησιμοποιώντας το δείγμα δεδομένων που παρέχεται με το σύμπλεγμα HDInsight:

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
    * **INPUT__FILE__NAME ΌΠΩΣ '%.log'** - σας ενημερώνει για την ομάδα που θα σας πρέπει να επιστρέφει μόνο δεδομένα από αρχεία που τελειώνει σε. καταγραφής. Κανονικά, θα έχετε μόνο τα δεδομένα με το ίδιο σχήμα μέσα στον ίδιο φάκελο κατά την υποβολή ερωτημάτων με hive, ωστόσο αυτό το παράδειγμα το αρχείο καταγραφής αποθηκεύεται με άλλες μορφές δεδομένων.

    > [AZURE.NOTE] Εξωτερικοί πίνακες πρέπει να χρησιμοποιείται όταν αναμένετε τα υποκείμενα δεδομένα για να ενημερωθούν από μια εξωτερική προέλευση, όπως μια διεργασία αποστολής αυτοματοποιημένη δεδομένων ή με μια άλλη MapReduce λειτουργία, αλλά θέλετε πάντα Hive ερωτημάτων για να χρησιμοποιήσετε τα πιο πρόσφατα δεδομένα.
    >
    > Απόθεση έναν εξωτερικό πίνακα κάνει **διαγράφει τα δεδομένα, μόνο τον ορισμό του πίνακα** .
    
    Το αποτέλεσμα αυτής της εντολής πρέπει να είναι παρόμοια με τα εξής:
    
        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :
        
        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)
        
        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

4. Για να εξέλθετε από Beeline, χρησιμοποιήστε `!quit`.

##<a id="file"></a>Εκτέλεση ενός αρχείου HiveQL

Beeline μπορεί επίσης να χρησιμοποιηθεί για να εκτελέσετε ένα αρχείο που περιέχει δηλώσεις HiveQL. Χρησιμοποιήστε τα παρακάτω βήματα για να δημιουργήσετε ένα αρχείο και, στη συνέχεια, εκτελέστε χρησιμοποιώντας Beeline.

1. Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε ένα νέο αρχείο με το όνομα __query.hql__:

        nano query.hql
        
2. Όταν ανοίξει το πρόγραμμα επεξεργασίας, χρησιμοποιήστε τα ακόλουθα με τα περιεχόμενα του αρχείου. Αυτό το ερώτημα θα δημιουργήσετε έναν νέο πίνακα 'εσωτερικού' με το όνομα **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Αυτές τις προτάσεις, εκτελέστε τις ακόλουθες ενέργειες:

    * **ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑΣ IF δεν ΥΠΆΡΧΕΙ** - δημιουργεί έναν πίνακα, εάν δεν υπάρχει ήδη. Εφόσον δεν χρησιμοποιείται το **ΕΞΩΤΕΡΙΚΌ** λέξεων-κλειδιών, αυτή είναι μια εσωτερική πίνακα, το οποίο είναι αποθηκευμένο στην ομάδα αποθήκη δεδομένων και γίνεται εντελώς από ομάδα.
    * **ΑΠΟΘΗΚΕΥΜΈΝΑ ΩΣ ORC** - αποθηκεύει τα δεδομένα σε μορφή βελτιστοποιημένη σε στήλες γραμμής (ORC). Αυτή είναι μια μορφή ιδιαίτερα βελτιστοποιημένη και αποτελεσματική για την αποθήκευση δεδομένων ομάδας.
    * ΑΝΤΙΚΑΤΆΣΤΑΣΗ **Εισαγωγή... ΕΠΙΛΈΞΤΕ** - επιλέγει γραμμές από τον πίνακα **log4jLogs** που περιέχουν **[ΣΦΆΛΜΑΤΟΣ]**και, στη συνέχεια, εισάγει τα δεδομένα στον πίνακα **errorLogs** .
    
    > [AZURE.NOTE] Σε αντίθεση με εξωτερική πίνακες, απόθεση ενός πίνακα του εσωτερικού θα διαγράψει καθώς και τα υποκείμενα δεδομένα.
    
3. Για να αποθηκεύσετε το αρχείο, χρησιμοποιήστε το __συνδυασμό πλήκτρων Ctrl__+ ___X__και, στη συνέχεια, πληκτρολογήστε __Y__και τέλος __Enter__.

4. Χρησιμοποιήστε τα ακόλουθα για να εκτελέσετε το αρχείο χρησιμοποιώντας Beeline. Αντικαταστήστε το __όνομα κεντρικού ΥΠΟΛΟΓΙΣΤΉ__ με το όνομα που έχει ληφθεί προηγουμένως για τα κεφαλής κόμβο και __τον κωδικό ΠΡΌΣΒΑΣΗΣ__ με τον κωδικό πρόσβασης για το λογαριασμό διαχειριστή:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i query.hql

    > [AZURE.NOTE] Το `-i` παραμέτρου ξεκινά Beeline, εκτελεί τις προτάσεις στο αρχείο query.hql και παραμένει στο Beeline στο το `jdbc:hive2://localhost:10001/>` ερώτηση. Μπορείτε επίσης να εκτελέσετε ένα αρχείο χρησιμοποιώντας το `-f` παράμετρο, η οποία επιστρέφει στην πάρτι μετά το αρχείο έχει υποστεί επεξεργασία.

5. Για να επαληθεύσετε ότι ο πίνακας **errorLogs** δημιουργήθηκε, χρησιμοποιήστε την ακόλουθη πρόταση για να επιστρέψει όλες τις γραμμές από **errorLogs**:

        SELECT * from errorLogs;

    Τρεις γραμμές δεδομένων πρέπει να επιστραφεί, όλα περιέχει **[ΣΦΆΛΜΑΤΟΣ]** στο Τ4 στήλης:
    
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a name="more-about-beeline-connectivity"></a>Περισσότερες πληροφορίες σχετικά με τη σύνδεση Beeline

Τα βήματα σε αυτό έγγραφο χρήση `localhost` για να συνδεθείτε με HiveServer2 λειτουργεί με το headnode σύμπλεγμα. Ενώ μπορείτε επίσης να χρησιμοποιήσετε το όνομα κεντρικού υπολογιστή ή το πλήρως προσδιορισμένο όνομα τομέα από το headnode αυτά απαιτούν επιπλέον βήματα για τη διαδικασία (βήματα για να βρείτε το όνομα κεντρικού υπολογιστή ή FQDN). Χρήση `localhost` αρκεί κατά τη χρήση Beeline από το headnode.

Εάν έχετε έναν κόμβο άκρο στο σύμπλεγμά σας, με Beeline εγκατασταθεί, θα πρέπει να χρησιμοποιήσετε το όνομα κεντρικού υπολογιστή ή το FQDN του το headnode για να συνδεθείτε.

Εάν έχετε εγκατεστημένο σε ένα πρόγραμμα-πελάτης εκτός του συμπλέγματος Beeline, μπορείτε να συνδεθείτε χρησιμοποιώντας την ακόλουθη εντολή. Αντικαταστήστε __CLUSTERNAME__ με το όνομα του συμπλέγματος HDInsight. Αντικαταστήστε __τον κωδικό ΠΡΌΣΒΑΣΗΣ__ με τον κωδικό πρόσβασης για το λογαριασμό διαχειριστή (σύνδεση HTTP).

    beeline -u 'jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=hive2' -n admin -p PASSWORD

Σημειώστε ότι οι παράμετροι URI είναι διαφορετική από όταν εκτελούνται απευθείας σε μια headnode ή από έναν κόμβο άκρη μέσα στο σύμπλεγμα. Αυτό συμβαίνει επειδή τη σύνδεση στο σύμπλεγμα από το internet χρησιμοποιεί μια δημόσια πύλη, η οποία δρομολογεί την κίνηση μέσω της θύρας 443. Επίσης, πολλές άλλες υπηρεσίες εκτίθενται από την πύλη δημόσια στη θύρα 443, ώστε το URI είναι διαφορετικό από κατά τη σύνδεση απευθείας. Κατά τη σύνδεση από το internet επίσης απαιτείται έλεγχος ταυτότητας για την περίοδο λειτουργίας, παρέχοντας τον κωδικό πρόσβασης.

##<a id="summary"></a><a id="nextsteps"></a>Επόμενα βήματα

Όπως μπορείτε να δείτε την εντολή Beeline αποτελούν έναν εύκολο τρόπο για να εκτελέσετε αλληλεπιδραστικά Hive ερωτήματα σε ένα σύμπλεγμα HDInsight.

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


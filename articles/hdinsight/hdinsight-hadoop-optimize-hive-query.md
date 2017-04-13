<properties
   pageTitle="Βελτιστοποίηση των ερωτημάτων Hive για ταχύτερη εκτέλεση στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να βελτιστοποιήσετε τα ερωτήματά σας ομάδα για Hadoop στο HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Βελτιστοποίηση Hive ερωτήματα για Hadoop σε HDInsight

Από προεπιλογή, Hadoop συμπλεγμάτων δεν είναι βελτιστοποιημένα για τις επιδόσεις. Σε αυτό το άρθρο καλύπτει μερικές από τις πιο συνηθισμένες Hive επιδόσεων βελτιστοποίηση μεθόδους που μπορείτε να εφαρμόσετε στα ερωτήματα μας.

##<a name="scale-out-worker-nodes"></a>Διαβάθμιση κόμβους εργαζόμενου

Αύξηση του αριθμού των κόμβους εργαζόμενου σε ένα σύμπλεγμα να αξιοποιήσετε περισσότερες mappers και σμίκρυνσης πρέπει να εκτελεστούν σε παράλληλα. Υπάρχουν δύο τρόποι για να αυξήσετε κλίμακα ανάληψη στο HDInsight:

- Κατά την προμήθεια, μπορείτε να καθορίσετε τον αριθμό των κόμβοι εργασίας χρησιμοποιώντας την πύλη Azure, Azure PowerShell ή το περιβάλλον γραμμής εντολών πλατφόρμες.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [συμπλεγμάτων παροχή HDInsight](hdinsight-provision-clusters.md). Η παρακάτω οθόνη εμφάνιση ο εργαζόμενος ρύθμιση παραμέτρων κόμβου στην πύλη του Azure:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Κατά το χρόνο εκτέλεσης, μπορείτε να κλιμάκωση επίσης ανάληψη ένα σύμπλεγμα χωρίς να δημιουργείτε ένα. Αυτό εμφανίζεται κάτω από το στοιχείο.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Για περισσότερες λεπτομέρειες σχετικά με τις διαφορετικές εικονικές μηχανές που υποστηρίζονται από το HDInsight, ανατρέξτε στο θέμα [HDInsight τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Ενεργοποίηση Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) είναι ένας μηχανισμός εναλλακτικές εκτέλεσης για το μηχανισμό MapReduce:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez είναι ταχύτερη, επειδή:

- Εκτέλεση κατευθύνεται Graph μη κυκλικό (DAG) ως μία μόνο εργασία στο μηχανισμό MapReduce, το DAG που εκφράζεται απαιτεί κάθε σύνολο mappers πρέπει να ακολουθείται από ένα σύνολο σμίκρυνσης. Αυτό θα κάνει πολλές MapReduce εργασίες να είναι απορρίμματα για κάθε ομάδα ερώτημα. Tez δεν έχει εν λόγω περιορισμών και να επεξεργαστείτε σύνθετες DAG ως μία εργασία, επομένως, την ελαχιστοποίηση επιβάρυνσης εκκίνησης εργασία.
- **Συντάσσει Avoids δεν είναι απαραίτητες** Λόγω πολλές εργασίες που απορρίμματα για το ίδιο ερώτημα Hive στο μηχανισμό MapReduce, το αποτέλεσμα του κάθε εργασίας έχει συνταχθεί σε HDFS για ενδιάμεσου δεδομένων. Επειδή το Tez ελαχιστοποιεί αριθμός εργασιών για κάθε ομάδα ερώτημα είναι σε θέση να αποφύγετε περιττές εγγραφής.
- **Καθυστερήσεις εκκίνησης Minimizes** Tez καλύτερα είναι δυνατό να ελαχιστοποιήσετε καθυστέρηση εκκίνησης, μείωση του αριθμού των mappers που χρειάζεται για να ξεκινήσετε και επίσης τη βελτίωση της βελτιστοποίησης σε όλο το.
- **Χρησιμοποιεί εκ νέου κοντέινερ** Κάθε φορά που πιθανές Tez είναι δυνατό να επαναχρησιμοποιήσετε κοντέινερ για να βεβαιωθείτε ότι έχει μειωθεί λανθάνων χρόνος λόγω εκκίνηση κοντέινερ.
- **Συνεχής βελτιστοποίηση τεχνικές** Παραδοσιακά βελτιστοποίηση έγινε κατά τη φάση μεταγλώττισης. Ωστόσο περισσότερες πληροφορίες σχετικά με τα δεδομένα εισόδου είναι διαθέσιμη που επιτρέπουν την καλύτερη βελτιστοποίηση κατά τη διάρκεια του χρόνου εκτέλεσης. Tez χρησιμοποιεί συνεχής βελτιστοποίηση τεχνικές που σας επιτρέπει να βελτιστοποίηση του σχεδίου περαιτέρω σε η φάση χρόνου εκτέλεσης.

Για περισσότερες λεπτομέρειες σχετικά με αυτά τα θέματα, κάντε κλικ [εδώ](http://hortonworks.com/hadoop/tez/)

Μπορείτε να κάνετε οποιοδήποτε ερώτημα Hive Tez με δυνατότητα τοποθετώντας το ερώτημα με τη ρύθμιση παρακάτω:

    set hive.execution.engine=tez;

Για συμπλεγμάτων HDInsight που βασίζεται στα Windows, πρέπει να ενεργοποιηθεί Tez κατά την προμήθεια. Ακολουθεί ένα δείγμα δέσμης ενεργειών του PowerShell Azure για παροχή ένα σύμπλεγμα Hadoop με Tez με δυνατότητα:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Βάσει Linux συμπλεγμάτων HDInsight έχουν Tez ενεργοποιημένη από προεπιλογή.
    

## <a name="hive-partitioning"></a>Hive διαμερισμάτων

Η λειτουργία εισόδου/εξόδου είναι η κύρια επιδόσεων συμφόρηση εκτέλεσης ερωτημάτων Hive. Οι επιδόσεις μπορούν να βελτιωθούν εάν μπορεί να μειωθεί η ποσότητα των δεδομένων που πρέπει να διαβάσετε. Από προεπιλογή, τα ερωτήματα Hive σάρωση ολόκληρη ομάδα πίνακες. Αυτό είναι ιδανικά για ερωτήματα όπως σαρώνει πίνακα, ωστόσο για τα ερωτήματα που θα πρέπει να σαρώσετε μια μικρή ποσότητα δεδομένων (π.χ. ερωτήματα με φιλτράρισμα), αυτό δημιουργεί περιττές επιβάρυνσης. Ομάδα διαμερισμάτων επιτρέπει Hive ερωτημάτων για να αποκτήσετε πρόσβαση μόνο την απαραίτητες ποσότητα των δεδομένων σε πίνακες ομάδα.

Ομάδα διαμερισμάτων έχει υλοποιηθεί αναδιοργάνωση τα ανεπεξέργαστα δεδομένα σε νέα καταλόγους με κάθε διαμερίσματα αντιμετωπίζετε δικό του καταλόγου - όπου τα διαμερίσματα ορίζεται από το χρήστη. Το παρακάτω διάγραμμα παρουσιάζει διαμερισμάτων Hive πίνακα με βάση τη στήλη *Year*. Δημιουργείται ένα νέο κατάλογο για κάθε έτος.

![Δημιουργία διαμερισμάτων][image-hdi-optimize-hive-partitioning_1]

Ορισμένα ζητήματα διαμερισμού:

- **Κάντε οριζόντια κάτω δεν partition** - διαμερισμάτων στις στήλες με μερικά μόνο τιμές μπορούν να προκαλέσουν πολύ λίγα τα διαμερίσματα. Για παράδειγμα, διαμερισμάτων σε φύλο θα δημιουργήσετε μόνο δύο διαμερίσματα σε δημιουργηθούν (αρσενικά και θηλυκά), έτσι μόνο μείωση του λανθάνων χρόνος με έως ήμισυ.

- **Δεν υπερ-διαμερισμάτων** - στην Αντίθετα, δημιουργώντας ένα διαμερίσματα σε μια στήλη με μια μοναδική τιμή (π.χ. αναγνωριστικό χρήστη) θα προκαλέσει περισσότερα από ένα διαμερίσματα προκαλεί πολύ μεγάλο φόρτο για το σύμπλεγμα namenode καθώς θα πρέπει επίσης να χειρίζεται το μεγάλο όγκο καταλόγων.

- **Καλό είναι να αποφεύγετε δεδομένων skew** - σωστή επιλογή αριθμού-κλειδιού διαμερισμού έτσι ώστε όλα τα διαμερίσματα είναι ακόμα και μέγεθος. Ένα παράδειγμα διαμερισμάτων στην *κατάσταση* μπορεί να προκαλέσει τον αριθμό των εγγραφών στην περιοχή Καλιφόρνια να είναι σχεδόν 30 x του Vermont λόγω της διαφοράς σε πληθυσμού.

Για να δημιουργήσετε έναν πίνακα διαμερισμάτων, χρησιμοποιήστε τον όρο *Διαμερίσματα κατά* :

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Όταν δημιουργηθεί ο πίνακας διαμερίσματα, μπορείτε είτε να δημιουργήσετε διαμερισμάτων στατική ή δυναμική διαμερισμάτων.

- **Στατική διαμερισμάτων** σημαίνει ότι έχετε ήδη sharded δεδομένα σε στους κατάλληλους καταλόγους και μπορείτε να ζητήσετε από τα διαμερίσματα Hive με μη αυτόματο τρόπο βάσει της θέσης του καταλόγου. Αυτό εμφανίζεται στο παρακάτω τμήμα κώδικα.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Δυναμική διαμερισμάτων** σημαίνει ότι θέλετε ομάδα για να δημιουργήσετε τα διαμερίσματα αυτόματα για εσάς. Επειδή Έχουμε ήδη δημιουργήσει διαμερισμού πίνακα από τον πίνακα ενδιάμεσου σταδίου, μόνο πρέπει να κάνετε είναι να εισαγάγετε δεδομένα στον πίνακα διαμερίσματα, όπως φαίνεται παρακάτω:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία διαμερισμάτων πίνακες](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Χρησιμοποιήστε τη μορφή ORCFile

Ομάδα υποστηρίζει διαφορετικές μορφές αρχείων. Για παράδειγμα:

- **Κείμενο**: Αυτή είναι η προεπιλεγμένη μορφή αρχείου και λειτουργεί με περισσότερα σενάρια
- **Avro**: λειτουργεί καλά για διαλειτουργικότητα σενάρια
- **ORC/Parquet**: καταλληλότερο για τις επιδόσεις

Μορφή ORC (βελτιστοποιημένη γραμμή σε στήλες) είναι μια ιδιαίτερα αποτελεσματικό τρόπο για την αποθήκευση δεδομένων ομάδας. Σε σύγκριση με άλλες μορφές, ORC περιλαμβάνει τα παρακάτω πλεονεκτήματα:

- υποστήριξη για σύνθετους τύπους, συμπεριλαμβανομένων των τύπων σύνθετη και ημι-δομημένα και ημερομηνίας/ώρας
- έως 70% συμπίεσης
- ευρετήρια κάθε 10.000 γραμμές που επιτρέπουν παράλειψη γραμμών
- μια σημαντική αναπτυσσόμενη λίστα εκτέλεσης χρόνου εκτέλεσης

Για να ενεργοποιήσετε ORC μορφή, δημιουργήστε πρώτα έναν πίνακα με τον όρο *αποθηκευμένη ως ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Στη συνέχεια, εισάγετε δεδομένα στον πίνακα ORC από τον πίνακα ενδιάμεσου σταδίου. Για παράδειγμα:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Μπορείτε να διαβάσετε περισσότερα από τη μορφή ORC [εδώ](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization επιτρέπει την ομάδα για να επεξεργαστείτε μια δέσμη 1024 γραμμών μαζί αντί για επεξεργασία μία γραμμή τη φορά. Αυτό σημαίνει ότι οι απλές εργασίες γίνονται πιο γρήγορα επειδή λιγότερο εσωτερικός κώδικας θα πρέπει να εκτελέσετε.

Για να ενεργοποιήσετε την vectorization πρόθεμα το ερώτημά σας ομάδα με την εξής ρύθμιση:

    set hive.vectorized.execution.enabled = true;

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Vectorized εκτέλεση του ερωτήματος](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Άλλες μέθοδοι βελτιστοποίησης

Υπάρχουν περισσότερες μεθόδους βελτιστοποίηση που μπορείτε να λάβετε υπόψη σας, για παράδειγμα:

- **Hive bucketing:** μια τεχνική που σας επιτρέπει να συμπλέγματος ή να χωρίσετε μεγάλα σύνολα δεδομένων για βελτιστοποίηση των επιδόσεων του ερωτήματος.
- **Συμμετοχή βελτιστοποίησης:** βελτιστοποίηση της εκτέλεσης του ερωτήματος της ομάδας προγραμματισμού για να βελτιώσετε την αποτελεσματικότητα της σύνδεσμοι και να μειώσετε την ανάγκη για συμβουλές χρήστη. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [συμμετοχή βελτιστοποίηση](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **αύξηση σμίκρυνσης**

##<a id="nextsteps"></a>Επόμενα βήματα
Σε αυτό το άρθρο μάθατε διάφορες κοινές μεθόδους βελτιστοποίηση ερωτήματος ομάδας. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

- [Χρήση της ομάδας Apache στο HDInsight](hdinsight-use-hive.md)
- [Ανάλυση πτήσεων καθυστέρηση δεδομένων με χρήση της ομάδας στο HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Ανάλυση των δεδομένων Twitter με ομάδα στο HDInsight](hdinsight-analyze-twitter-data.md)
- [Ανάλυση των δεδομένων αισθητήρα χρησιμοποιώντας την κονσόλα ερωτήματος Hive στην Hadoop στο HDInsight](hdinsight-hive-analyze-sensor-data.md)
- [Χρήση της ομάδας με το HDInsight για να αναλύσετε αρχεία καταγραφής από τοποθεσίες Web](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png

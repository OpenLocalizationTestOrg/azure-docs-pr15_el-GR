<properties
    pageTitle="Χρήση ροών εργασίας Hadoop Oozie σε HDInsight που βασίζεται στο Linux | Microsoft Azure"
    description="Χρησιμοποιήστε Hadoop Oozie βάσει Linux HDInsight. Μάθετε πώς μπορείτε να ορίσετε μια ροή εργασίας Oozie, και να υποβάλετε μια εργασία Oozie."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>


# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Χρησιμοποιήστε Oozie με Hadoop για να ορίσετε και να εκτελέσετε μια ροή εργασίας με βάση Linux HDInsight

[AZURE.INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Apache Oozie για να ορίσετε μια ροή εργασίας που χρησιμοποιεί η ομάδα και Sqoop και, στη συνέχεια, να εκτελέσετε τη ροή εργασίας σε ένα σύμπλεγμα βάσει Linux HDInsight.

Apache Oozie είναι ένα σύστημα ροής εργασίας/συντονισμό που διαχειρίζεται το Hadoop εργασίες. Αυτό είναι συνδεδεμένο με την στοίβα Hadoop, καθώς και υποστηρίζει Hadoop εργασίες για Apache MapReduce, γουρούνι Apache, Apache Hive και Apache Sqoop. Μπορεί επίσης να χρησιμοποιηθεί για να προγραμματίζουν εργασίες που σχετίζονται με ένα σύστημα, όπως τα προγράμματα Java ή δεσμών ενεργειών κελύφους

> [AZURE.NOTE] Μια άλλη επιλογή για τον ορισμό ροές εργασιών με HDInsight είναι Azure εργοστασίου δεδομένων. Για να μάθετε περισσότερα σχετικά με το Azure εργοστασίου δεδομένων, ανατρέξτε στο θέμα [Χρήση γουρούνι και ομάδα με δεδομένα εργοστασίου][azure-data-factory-pig-hive].

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Συνδρομή του Azure**: ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI**: ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md)
    
    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- **Μια HDInsight σύμπλεγμα**: ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το HDInsight σε Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Βάση δεδομένων του SQL Azure**: αυτό θα δημιουργηθεί χρησιμοποιώντας τα βήματα που περιγράφονται σε αυτό το έγγραφο

##<a name="example-workflow"></a>Παράδειγμα ροής εργασίας

Η ροή εργασίας θα υλοποιήσετε, ακολουθώντας τις οδηγίες σε αυτό το έγγραφο περιέχει δύο ενέργειες. Οι ενέργειες είναι ορισμούς για εργασίες, όπως η εκτέλεση Hive, Sqoop, MapReduce ή άλλη διαδικασία:

![Διάγραμμα ροής εργασίας][img-workflow-diagram]

1. Μια ομάδα ενέργεια εκτελεί μια δέσμη ενεργειών HiveQL για να εξαγάγετε τις εγγραφές από το **hivesampletable** περιλαμβάνεται με το HDInsight. Κάθε γραμμή δεδομένων περιγράφονται επίσκεψη από μια συγκεκριμένη κινητή συσκευή. Η μορφή εγγραφής εμφανίζεται παρόμοιο με το εξής:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Η ομάδα δέσμη ενεργειών που χρησιμοποιούνται σε αυτό το έγγραφο καταμετρά τις συνολικό επισκέψεις για κάθε πλατφόρμα (όπως Android ή iPhone) και αποθηκεύει τις μετρήσεις σε ένα νέο πίνακα ομάδα.

    Για περισσότερες πληροφορίες σχετικά με την ομάδα, ανατρέξτε στο θέμα [Χρήση Hive με HDInsight][hdinsight-use-hive].

2.  Μια ενέργεια Sqoop εξάγει τα περιεχόμενα του νέου πίνακα Hive σε έναν πίνακα σε μια βάση δεδομένων Azure SQL. Για περισσότερες πληροφορίες σχετικά με το Sqoop, ανατρέξτε στο θέμα [Χρήση Hadoop Sqoop με HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Για υποστηριζόμενες εκδόσεις Oozie στην συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα Hadoop που παρέχεται από το HDInsight;] [hdinsight-versions].

##<a name="create-the-working-directory"></a>Δημιουργήστε τον κατάλογο εργασίας

Oozie αναμένει πόρους που απαιτούνται για μια εργασία να είναι αποθηκευμένο στον ίδιο κατάλογο. Αυτό το παράδειγμα χρησιμοποιεί **wasbs: / / / προγράμματα εκμάθησης/useoozie**. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε αυτόν τον κατάλογο, καθώς και τον κατάλογο δεδομένων που θα περιέχει τον νέο πίνακα ομάδα που δημιουργήθηκε από αυτήν τη ροή εργασίας:

    hdfs dfs -mkdir -p /tutorials/useoozie/data

> [AZURE.NOTE] Το `-p` παραμέτρου προκληθεί όλους τους φακέλους στη διαδρομή σε δημιουργηθούν Εάν δεν υπάρχουν ήδη. Τον κατάλογο **δεδομένων** θα χρησιμοποιηθεί για τη διατήρηση δεδομένων που χρησιμοποιούνται από τη δέσμη ενεργειών **useooziewf.hql** .

Επίσης, εκτελέστε την ακόλουθη εντολή, το οποίο εξασφαλίζει ότι Oozie να μίμηση λογαριασμού χρήστη κατά την εκτέλεση εργασίες ομάδας και Sqoop. Αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** με το όνομα χρήστη σας:

    sudo adduser USERNAME users

Εάν λάβετε ένα σφάλμα ότι ο χρήστης είναι ήδη μέλος των χρηστών, μπορείτε απλώς να αγνοήσετε αυτό.

##<a name="add-a-database-driver"></a>Προσθήκη ενός προγράμματος οδήγησης βάσης δεδομένων

Επειδή αυτή η ροή εργασίας χρησιμοποιεί Sqoop για την εξαγωγή δεδομένων σε βάση δεδομένων SQL, πρέπει να δώσετε ένα αντίγραφο του προγράμματος οδήγησης JDBC που χρησιμοποιούνται για να επικοινωνήσουν με βάση δεδομένων SQL. Χρησιμοποιήστε την ακόλουθη εντολή για να το αντιγράψετε τον κατάλογο εργασίας:

    hdfs dfs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/

Εάν η ροή εργασίας χρησιμοποιήσει άλλους πόρους, όπως μια βάζο που περιέχει μια εφαρμογή MapReduce, θα πρέπει να προσθέσετε αυτές τις καθώς και.

##<a name="define-the-hive-query"></a>Ορισμός του ερωτήματος ομάδας

Χρησιμοποιήστε τα παρακάτω βήματα για να δημιουργήσετε μια δέσμη ενεργειών HiveQL που ορίζει ένα ερώτημα, το οποίο θα χρησιμοποιηθεί σε μια ροή εργασίας Oozie αργότερα σε αυτό το έγγραφο.

1. Χρησιμοποιήστε SSH για να συνδεθείτε με το σύμπλεγμα βάσει Linux HDInsight:

    * **Προγράμματα-πελάτες του Linux, Unix ή OS X**: ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, OS X ή Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Προγράμματα-πελάτες των Windows**: ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Χρησιμοποιήστε την παρακάτω εντολή για να δημιουργήσετε ένα νέο αρχείο:

        nano useooziewf.hql

1. Όταν ανοίξει το πρόγραμμα επεξεργασίας νανομετρικής, χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;

    Υπάρχουν δύο μεταβλητές που χρησιμοποιούνται στη δέσμη ενεργειών:

    - **${hiveTableName}**: θα περιέχει το όνομα του πίνακα που θα δημιουργηθεί
    - **${hiveDataFolder}**: θα περιέχει τη θέση για να αποθηκεύσετε τα αρχεία δεδομένων για τον πίνακα

    Το αρχείο ορισμού ροής εργασίας (workflow.xml σε αυτό το πρόγραμμα εκμάθησης) μεταβιβάζει αυτές τις τιμές σε αυτήν τη δέσμη ενεργειών HiveQL κατά το χρόνο εκτέλεσης.

2. Πατήστε το συνδυασμό πλήκτρων Ctrl-X για να εξέλθετε από το πρόγραμμα επεξεργασίας. Όταν σας ζητηθεί, επιλέξτε **Y** για να αποθηκεύσετε το αρχείο και, στη συνέχεια, χρησιμοποιήστε **Enter** για να χρησιμοποιήσετε το όνομα του αρχείου **useooziewf.hql** .

3. Χρησιμοποιήστε τις παρακάτω εντολές για να αντιγράψετε **useooziewf.hql** **wasbs:///tutorials/useoozie/useooziewf.hql**:

        hdfs dfs -copyFromLocal useooziewf.hql /tutorials/useoozie/useooziewf.hql

    Αυτές οι εντολές αποθηκεύσετε το αρχείο **useooziewf.hql** στον λογαριασμό αποθήκευσης Azure που σχετίζονται με αυτό το σύμπλεγμα, που θα διατηρηθούν στο αρχείο, ακόμα και αν το σύμπλεγμα έχει διαγραφεί. Αυτό σας επιτρέπει να εξοικονομήσετε χρήματα διαγράφοντας συμπλεγμάτων όταν δεν είναι σε χρήση, διατηρώντας τις εργασίες και τις ροές εργασίας.

##<a name="define-the-workflow"></a>Ορισμός της ροής εργασίας

Ορισμοί ροές εργασίας Oozie είναι γραμμένες σε hPDL (μια XML διαδικασία Definition Language). Χρησιμοποιήστε τα ακόλουθα βήματα για να ορίσετε τη ροή εργασίας:

1. Χρησιμοποιήστε την ακόλουθη πρόταση για να δημιουργήσετε και να επεξεργαστείτε ένα νέο αρχείο:

        nano workflow.xml

1. Όταν ανοίξει το πρόγραμμα επεξεργασίας νανομετρικής, πληκτρολογήστε τα εξής ως τα περιεχόμενα του αρχείου:

        <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
            <start to = "RunHiveScript"/>
            <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.job.queue.name</name>
                    <value>${queueName}</value>
                </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
            </action>
            <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                <property>
                    <name>mapred.compress.map.output</name>
                    <value>true</value>
                </property>
                </configuration>
                <arg>export</arg>
                <arg>--connect</arg>
                <arg>${sqlDatabaseConnectionString}</arg>
                <arg>--table</arg>
                <arg>${sqlDatabaseTableName}</arg>
                <arg>--export-dir</arg>
                <arg>${hiveDataFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\t"</arg>
                <archive>sqljdbc41.jar</archive>
                </sqoop>
            <ok to="end"/>
            <error to="fail"/>
            </action>
            <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>
            <end name="end"/>
        </workflow-app>

    Υπάρχουν δύο ενέργειες που ορίζονται εντός της ροής εργασίας:

    - **RunHiveScript**: Αυτή η ενέργεια εκκίνησης και εκτελεί **useooziewf.hql** Hive δέσμης ενεργειών

    - **RunSqoopExport**: αυτό εξάγει τα δεδομένα που έχουν δημιουργηθεί από τη δέσμη ενεργειών της ομάδας στη βάση δεδομένων SQL χρησιμοποιώντας Sqoop. Αυτό θα εκτελεστεί μόνο εάν η ενέργεια **RunHiveScript** είναι επιτυχής.

        > [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τη ροή εργασίας Oozie και χρησιμοποιώντας ενέργειες ροής εργασίας, ανατρέξτε [στην τεκμηρίωση Apache Oozie 4.0] [ apache-oozie-400] (για το HDInsight έκδοση 3.0) ή [τεκμηρίωση Apache Oozie 3.3.2] [ apache-oozie-332] (για το HDInsight έκδοση 2.1).

    Σημειώστε ότι η ροή εργασίας έχει περισσότερες από μία εγγραφές, όπως είναι οι `${jobTracker}`, που θα αντικατασταθούν από τιμές που χρησιμοποιείτε στον ορισμό εργασίας παρακάτω σε αυτό το έγγραφο.

    Επίσης, σημειώστε το `<archive>sqljdbc4.jar</arcive>` καταχώρηση στην ενότητα Sqoop. Αυτό καθοδηγεί Oozie για τη διάθεση αυτό το αρχείο για Sqoop κατά την εκτέλεση αυτής της ενέργειας.

2. Χρησιμοποιήστε το συνδυασμό πλήκτρων Ctrl-X, στη συνέχεια, **Y** και **Enter** για να αποθηκεύσετε το αρχείο.

3. Χρησιμοποιήστε την ακόλουθη εντολή για να αντιγράψετε το αρχείο **workflow.xml** **wasbs:///tutorials/useoozie/workflow.xml**:

        hdfs dfs -copyFromLocal workflow.xml /tutorials/useoozie/workflow.xml

##<a name="create-the-database"></a>Δημιουργία της βάσης δεδομένων

Ακολουθήστε τα βήματα στο έγγραφο [Δημιουργία βάσης δεδομένων SQL](../sql-database/sql-database-get-started.md) για να δημιουργήσετε μια νέα βάση δεδομένων. Κατά τη δημιουργία της βάσης δεδομένων, χρησιμοποιήστε __oozietest__ ως το όνομα της βάσης δεδομένων. Επίσης Σημειώστε το όνομα που χρησιμοποιείται για το διακομιστή βάσης δεδομένων, όπως αυτό θα χρειαστούν στην επόμενη ενότητα.

###<a name="create-the-table"></a>Δημιουργία πίνακα

> [AZURE.NOTE] Υπάρχουν πολλοί τρόποι για να συνδεθείτε με βάση δεδομένων SQL για να δημιουργήσετε έναν πίνακα. Τα παρακάτω βήματα χρησιμοποιούν [FreeTDS](http://www.freetds.org/) από το HDInsight σύμπλεγμα.

3. Χρησιμοποιήστε την ακόλουθη εντολή για να εγκαταστήσετε FreeTDS σε σύμπλεγμα HDInsight:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Μία φορά FreeTDS έχει εγκατασταθεί, χρησιμοποιήστε την ακόλουθη εντολή για να συνδεθείτε με το διακομιστή βάσης δεδομένων SQL που δημιουργήσατε προηγουμένως:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest

    Θα λάβετε εξόδου παρόμοια με τα εξής:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

5. Στο το `1>` ερώτηση, πληκτρολογήστε τις ακόλουθες γραμμές:

        CREATE TABLE [dbo].[mobiledata](
        [deviceplatform] [nvarchar](50),
        [count] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
        GO

    Όταν το `GO` καταχώρηση δήλωση, θα αξιολογηθεί τις προηγούμενες καταστάσεις. Αυτό θα δημιουργήσει έναν νέο πίνακα με το όνομα **mobiledata** που θα εγγραφούν από Sqoop.

    Χρησιμοποιήστε τα ακόλουθα για να επαληθεύσετε ότι έχει δημιουργηθεί στον πίνακα:

        SELECT * FROM information_schema.tables
        GO

    Θα πρέπει να βλέπετε εξόδου παρόμοια με τα εξής:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        oozietest       dbo     mobiledata      BASE TABLE

8. Πληκτρολογήστε `exit` στο το `1>` μηνύματος για να εξέλθετε από το βοηθητικό πρόγραμμα tsql.

##<a name="create-the-job-definition"></a>Δημιουργήστε τον ορισμό εργασίας

Ο ορισμός εργασίας περιγράφονται πού μπορείτε να βρείτε το workflow.xml, καθώς και άλλα αρχεία που χρησιμοποιούνται από τη ροή εργασίας (όπως useooziewf.hql). Καθορίζει επίσης τις τιμές για ιδιότητες χρησιμοποιούνται εντός της ροής εργασίας και συσχετισμένα αρχεία.

1. Χρησιμοποιήστε την παρακάτω εντολή για να λάβετε την πλήρη διεύθυνση WASB με το προεπιλεγμένο χώρο αποθήκευσης. Αυτό θα χρησιμοποιηθεί στο αρχείο παραμέτρων σε μια στιγμή:

        sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml

    Αυτό πρέπει να επιστρέψει πληροφορίες παρόμοιο με το εξής:

        <name>fs.defaultFS</name>
        <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>

    Αποθηκεύστε το **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** τιμή, όπως θα χρησιμοποιηθεί στα επόμενα βήματα.

2. Χρησιμοποιήστε την παρακάτω εντολή για να λάβετε το FQDN του το headnode σύμπλεγμα. Αυτό θα χρησιμοποιηθεί για τη διεύθυνση JobTracker για το σύμπλεγμα. Αυτό θα χρησιμοποιηθεί στο αρχείο παραμέτρων σε μια στιγμή:

        hostname -f

    Αυτό θα επιστρέψει πληροφορίες παρόμοιο με το εξής:

        hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net

    Στη θύρα που χρησιμοποιείται για την JobTracker είναι 8050, ώστε να την πλήρη διεύθυνση για να χρησιμοποιήσετε για το JobTracker θα **hn0 CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050**.

1. Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε τη ρύθμιση παραμέτρων Oozie εργασία ορισμού:

        nano job.xml

2. Όταν ανοίξει το πρόγραμμα επεξεργασίας νανομετρικής, χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

          <property>
            <name>nameNode</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net</value>
          </property>

          <property>
            <name>jobTracker</name>
            <value>JOBTRACKERADDRESS</value>
          </property>

          <property>
            <name>queueName</name>
            <value>default</value>
          </property>

          <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
          </property>

          <property>
            <name>hiveScript</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
          </property>

          <property>
            <name>hiveTableName</name>
            <value>mobilecount</value>
          </property>

          <property>
            <name>hiveDataFolder</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
          </property>

          <property>
            <name>sqlDatabaseConnectionString</name>
            <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
          </property>

          <property>
            <name>sqlDatabaseTableName</name>
            <value>mobiledata</value>
          </property>

          <property>
            <name>user.name</name>
            <value>YourName</value>
          </property>

          <property>
            <name>oozie.wf.application.path</name>
            <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
          </property>
        </configuration>

    * Αντικαταστήστε όλες τις εμφανίσεις της **wasbs://mycontainer@mystorageaccount.blob.core.windows.net** με την τιμή που λάβατε νωρίτερα.

    > [AZURE.WARNING] Πρέπει να χρησιμοποιείτε την πλήρη διαδρομή WASB, με το λογαριασμό κοντέινερ και αποθήκευσης ως μέρος της διαδρομής. Χρησιμοποιώντας τη σύντομη μορφή (wasbs: / / /) θα προκαλέσει την ενέργεια RunHiveScript αποτυχία κατά την έναρξη της εργασίας.

    * Αντικαταστήστε **JOBTRACKERADDRESS** με τη διεύθυνση JobTracker/ResourceManager που λάβατε νωρίτερα.

    * Αντικαταστήστε **Το_όνομά_σας** με το όνομα σύνδεσης για το σύμπλεγμα HDInsight.

    * Αντικατάσταση **όνομα_διακομιστή**, **adminLogin**και **adminPassword** με τις πληροφορίες για τη βάση δεδομένων SQL Azure.

    Περισσότερες πληροφορίες σε αυτό το αρχείο χρησιμοποιείται για να συμπληρώσετε τις τιμές που χρησιμοποιούνται στα αρχεία workflow.xml ή ooziewf.hql (όπως ${nameNode}.)

    > [AZURE.NOTE] Η καταχώρηση **oozie.wf.application.path** ορίζει πού μπορείτε να βρείτε το αρχείο workflow.xml, που περιέχει τη ροή εργασίας που εκτελέστηκε, αυτή η εργασία.

2. Χρησιμοποιήστε το συνδυασμό πλήκτρων Ctrl-X, στη συνέχεια, **Y** και **Enter** για να αποθηκεύσετε το αρχείο.

##<a name="submit-and-manage-the-job"></a>Υποβολή και να διαχειριστείτε την εργασία

Τα παρακάτω βήματα χρησιμοποιούν την εντολή Oozie να υποβάλετε και να διαχειριστείτε τις ροές εργασίας Oozie στο σύμπλεγμα. Η εντολή Oozie είναι ένα φιλικό περιβάλλον εργασίας μέσω του [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [AZURE.IMPORTANT] Όταν χρησιμοποιείτε την εντολή Oozie, πρέπει να χρησιμοποιήσετε το FQDN για το headnode HDInsight. Σε αυτό το FQDN μόνο είναι προσβάσιμη από το σύμπλεγμα, ή εάν το σύμπλεγμα είναι σε ένα δίκτυο εικονικού Azure, από άλλους υπολογιστές στο ίδιο δίκτυο.

1. Χρησιμοποιήστε τα ακόλουθα για να λάβετε τη διεύθυνση URL για την υπηρεσία Oozie:

        sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml

    Αυτό θα επιστρέψει μια τιμή παρόμοιο με το εξής:

        <name>oozie.base.url</name>
        <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>

    Το τμήμα **http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie** είναι η διεύθυνση URL για χρήση με την εντολή Oozie.

2. Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε μια μεταβλητή περιβάλλοντος για τη διεύθυνση URL, ώστε να μην χρειάζεται να την πληκτρολογήσετε για κάθε εντολή:

        export OOZIE_URL=http://HOSTNAMEt:11000/oozie

    Αντικατάσταση της διεύθυνσης URL με το που λάβατε νωρίτερα.

3. Χρησιμοποιήστε τα ακόλουθα για να υποβάλετε την εργασία:

        oozie job -config job.xml -submit

    Αυτό φορτώνει πληροφορίες για την εργασία από **job.xml** και υποβάλλει στην Oozie, αλλά δεν εκτελείται.

    Μόλις ολοκληρωθεί η εντολή, πρέπει να επιστρέψει το Αναγνωριστικό της εργασίας. Για παράδειγμα, `0000005-150622124850154-oozie-oozi-W`. Αυτό θα χρησιμοποιηθεί για τη διαχείριση της εργασίας.

4. Προβολή της κατάστασης της εργασίας, χρησιμοποιώντας την ακόλουθη εντολή. Πληκτρολογήστε το Αναγνωριστικό εργασίας που επιστρέφονται από την προηγούμενη εντολή:

        oozie job -info <JOBID>

    Αυτό θα επιστρέψει πληροφορίες παρόμοιο με το ακόλουθο.

        Job ID : 0000005-150622124850154-oozie-oozi-W
        ------------------------------------------------------------------------------------------------------------------------------------
        Workflow Name : useooziewf
        App Path      : wasbs:///tutorials/useoozie
        Status        : PREP
        Run           : 0
        User          : USERNAME
        Group         : -
        Created       : 2015-06-22 15:06 GMT
        Started       : -
        Last Modified : 2015-06-22 15:06 GMT
        Ended         : -
        CoordAction ID: -
        ------------------------------------------------------------------------------------------------------------------------------------

    Αυτή η εργασία έχει μια κατάσταση `PREP`, που δείχνει ότι έχει υποβληθεί, αλλά δεν έχει αρχίσει ακόμα.

4. Χρησιμοποιήστε τα ακόλουθα για να ξεκινήσει η εργασία:

        oozie job -start JOBID

    Εάν μπορείτε να ελέγξετε την κατάσταση μετά από αυτή την εντολή, θα είναι σε κατάσταση λειτουργίας και θα επιστραφούν πληροφορίες για τις ενέργειες στο έργο.

5. Μόλις η εργασία ολοκληρωθεί με επιτυχία, μπορείτε να επαληθεύσετε ότι τα δεδομένα ήταν που δημιουργούνται και εξάγονται στον πίνακα βάσης δεδομένων SQL, χρησιμοποιώντας τις παρακάτω εντολές:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest

    Κατά την `1>` ερώτηση, πληκτρολογήστε τα εξής:

        SELECT * FROM mobiledata
        GO

    Πρέπει να λάβετε πληροφορίες παρόμοιο με το εξής:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Για περισσότερες πληροφορίες σχετικά με την εντολή Oozie, ανατρέξτε στο θέμα [Oozie εργαλείο γραμμής εντολών](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

##<a name="oozie-rest-api"></a>Oozie REST API

Το REST API Oozie σάς επιτρέπουν να δημιουργήσετε τα δικά σας εργαλεία που λειτουργούν με το Oozie. Οι παρακάτω είναι HDInsight συγκεκριμένες πληροφορίες σχετικά με τη χρήση του REST API Oozie:

* **URI**: το REST API είναι προσβάσιμα από έξω από το σύμπλεγμα στο`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Έλεγχος ταυτότητας**: απαιτείται έλεγχος ταυτότητας για το API χρησιμοποιώντας το λογαριασμό συμπλέγματος HTTP (διαχειριστές), και τον κωδικό πρόσβασης. Για παράδειγμα:

        curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions

Για περισσότερες πληροφορίες σχετικά με τη χρήση του REST API Oozie, ανατρέξτε στο θέμα [Oozie API των υπηρεσιών Web](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

##<a name="oozie-web-ui"></a>Oozie Web περιβάλλοντος εργασίας Χρήστη

Το περιβάλλον εργασίας Χρήστη Web Oozie παρέχει μια προβολή που βασίζεται στο web σε της κατάστασης των εργασιών Oozie στο σύμπλεγμα. Σας επιτρέπει να προβάλετε κατάστασης εργασίας, ο ορισμός εργασίας, ρύθμιση παραμέτρων, ένα γράφημα από τις ενέργειες το έργο και αρχεία καταγραφής για το έργο. Μπορείτε επίσης να προβάλετε λεπτομέρειες για ενέργειες μέσα σε μια εργασία.

Για να αποκτήσετε πρόσβαση στο περιβάλλον εργασίας Χρήστη του Oozie Web, χρησιμοποιήστε τα παρακάτω βήματα:

1. Δημιουργήστε μια διοχέτευση SSH στο σύμπλεγμα HDInsight. Για πληροφορίες σχετικά με τον τρόπο για να το κάνετε αυτό, ανατρέξτε στο θέμα [Χρήση SSH διοχέτευση για πρόσβαση στο web Ambari περιβάλλοντος εργασίας Χρήστη, ResourceManager, JobHistory, NameNode, Oozie, και άλλη τοποθεσία web του περιβάλλοντος εργασίας Χρήστη](hdinsight-linux-ambari-ssh-tunnel.md).

2. Μόλις δημιουργηθεί μια διοχέτευση, ανοίξτε το web Ambari περιβάλλοντος εργασίας Χρήστη στο πρόγραμμα περιήγησης web. Το URI για την τοποθεσία Ambari είναι **https://CLUSTERNAME.azurehdinsight.net**. Αντικαταστήστε **CLUSTERNAME** με το όνομα του συμπλέγματος βάσει Linux HDInsight.

3. Από την αριστερή πλευρά της σελίδας, επιλέξτε **Oozie**, στη συνέχεια, **Γρήγορες συνδέσεις**και τέλος **Oozie Web UI**.

    ![εικόνα μενού](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Εμφανίζει εργασίες ροής εργασίας που εκτελούνται από το Web UI Oozie προεπιλογή. Για να δείτε όλες τις εργασίες ροής εργασίας, επιλέξτε **Όλες τις εργασίες**.

    ![Όλες οι εργασίες εμφανίζονται](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Επιλέξτε μια εργασία για να δείτε περισσότερες πληροφορίες σχετικά με την εργασία.

    ![Κατάσταση εργασίας](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Από την καρτέλα πληροφορίες έργου, μπορείτε να δείτε πληροφορίες για την εργασία βασικές, καθώς και τις μεμονωμένες ενέργειες στο έργο. Χρησιμοποιώντας τις καρτέλες στο επάνω μέρος μπορείτε να δείτε την πρόσβαση ορισμό εργασίας, ρύθμιση παραμέτρων εργασία, το αρχείο καταγραφής της εργασίας ή να προβάλετε μια κατευθύνεται μη κυκλικό Graph (DAG) της εργασίας.

    * **Αρχείο καταγραφής εργασίας**: Επιλέξτε το κουμπί **GetLogs** για να βρείτε όλα τα αρχεία καταγραφής για την εργασία, ή να χρησιμοποιήσετε το πεδίο **Εισαγάγετε φίλτρο αναζήτησης** για το φιλτράρισμα αρχείων καταγραφής

        ![Αρχείο καταγραφής εργασίας](./media/hdinsight-use-oozie-linux-mac/joblog.png)

    * **JobDAG**: DAG το θέμα παρέχει μια επισκόπηση γραφικών από τις διαδρομές δεδομένων που λαμβάνονται μέσω της ροής εργασίας

        ![Εργασία DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Επιλέγοντας μία από τις ενέργειες από την καρτέλα **Πληροφορίες εργασίας** θα εμφανιστεί πληροφορίες για την ενέργεια. Για παράδειγμα, επιλέξτε την ενέργεια **RunHiveScript** .

    ![Κατάσταση ενέργειας](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Μπορείτε να δείτε λεπτομέρειες για την ενέργεια, όπως μια σύνδεση προς τη **Διεύθυνση URL κονσόλας**, το οποίο μπορεί να χρησιμοποιηθεί για την προβολή πληροφοριών JobTracker για το έργο.

##<a name="scheduling-jobs"></a>Προγραμματισμός εργασιών

Το συντονισμό σάς επιτρέπει να καθορίσετε μια Έναρξη, λήξη και τη συχνότητα εμφάνισης για τις εργασίες, ώστε να μπορεί να προγραμματιστεί για ορισμένες φορές.

Για να καθορίσετε ένα χρονοδιάγραμμα για τη ροή εργασίας, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Χρησιμοποιήστε τα ακόλουθα για να δημιουργήσετε ένα νέο αρχείο με το όνομα **coordinator.xml**:

        nano coordinator.xml

    Χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
          <action>
            <workflow>
              <app-path>${workflowPath}</app-path>
            </workflow>
          </action>
        </coordinator-app>

    Σημειώστε ότι αυτό χρησιμοποιεί `${...}` μεταβλητές που θα αντικατασταθούν από τιμές στον ορισμό εργασίας. Οι μεταβλητές είναι:

    * **${coordFrequency}**: χρόνου μεταξύ παρουσίες του έργου
    * **${coordStart}**: ώρα έναρξης της εργασίας
    * **${coordEnd}**: την ώρα λήξης του έργου
    * **${coordTimezone}**: συντονισμού εργασίες που βρίσκονται σε μια σταθερή ζώνη ώρας με καμία θερινή ώρα (συνήθως αναπαριστώνται με τη χρήση UTC). Αυτή η ζώνη ώρας αναφέρεται ως είναι το "Oozie επεξεργασίας ζώνης ώρας"
    * **${wfPath}**: τη διαδρομή προς το workflow.xml

2. Χρησιμοποιήστε το συνδυασμό πλήκτρων Ctrl-X, στη συνέχεια, **Y** και **Enter** για να αποθηκεύσετε το αρχείο.

3. Χρησιμοποιήστε τα ακόλουθα για να το αντιγράψετε τον κατάλογο εργασίας για αυτό το έργο:

        hadoop fs -copyFromLocal coordinator.xml /tutorials/useoozie/coordinator.xml

4. Χρησιμοποιήστε τα ακόλουθα για να τροποποιήσετε το αρχείο **job.xml** :

        nano job.xml

    Κάντε τις ακόλουθες αλλαγές:

    * Αλλαγή `<name>oozie.wf.application.path</name>` να `<name>oozie.coord.application.path</name>`. Αυτό καθοδηγεί Oozie για να εκτελέσετε το αρχείο συντονισμού αντί για το αρχείο ροής εργασίας

    * Προσθέστε τα ακόλουθα, τα οποία θα είναι σύνολα μια μεταβλητή χρησιμοποιείται σε το coordinator.xml ώστε να οδηγεί στη θέση του το workflow.xml:

            <property>
              <name>workflowPath</name>
              <value>wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
            </property>

        Αντικαταστήστε τις τιμές για **mycontainer** και **mystorageaccount** με τις τιμές που χρησιμοποιούνται σε άλλες εγγραφές στο αρχείο job.xml.

    * Προσθέστε τα ακόλουθα, τα οποία ορίζουν Έναρξη, λήξη και τη συχνότητα για να χρησιμοποιήσετε για το αρχείο coordinator.xml:

            <property>
              <name>coordStart</name>
              <value>2015-06-25T12:00Z</value>
            </property>

            <property>
              <name>coordEnd</name>
              <value>2015-06-27T12:00Z</value>
            </property>

            <property>
              <name>coordFrequency</name>
              <value>1440</value>
            </property>

            <property>
              <name>coordTimezone</name>
              <value>UTC</value>
            </property>

        Αυτά τα ορίσετε την ώρα έναρξης για 12:00 μ.μ. 25ο Ιουνίου 2015, την ώρα λήξης για να 27th Ιουνίου 2015, και το χρονικό διάστημα για την εκτέλεση αυτής της εργασίας για ημερήσια (το όρισμα συχνότητα είναι σε λεπτά, επομένως 24 ώρες x 60 λεπτά = 1440 λεπτά.) Τέλος, τη ζώνη ώρας έχει οριστεί σε UTC.

5. Χρησιμοποιήστε το συνδυασμό πλήκτρων Ctrl-X, στη συνέχεια, **Y** και **Enter** για να αποθηκεύσετε το αρχείο.

6. Για να εκτελέσετε την εργασία, χρησιμοποιήστε την ακόλουθη εντολή:

        oozie job -config job.xml -run

    Αυτό θα υποβολή και να ξεκινήσει η εργασία.

7. Εάν επισκεφτείτε το περιβάλλον εργασίας Χρήστη Web Oozie και επιλέξτε την καρτέλα **Συντονισμού εργασίες** , θα πρέπει να πληροφορίες παρόμοιο με το εξής:

    ![καρτέλα συντονισμού εργασιών](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Σημειώστε την καταχώρηση **Επόμενη επέλευσης** ; Αυτό είναι όταν θα εκτελείται η επόμενη εργασία.

8. Παρόμοια με την προηγούμενη εργασία ροής εργασίας, επιλέγοντας την εγγραφή έργου στο περιβάλλον εργασίας Χρήστη web θα εμφανίζουν πληροφορίες για το έργο:

    ![Πληροφορίες έργου συντονισμού](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    Σημειώστε ότι αυτή εμφανίζει μόνο επιτυχής εκτελείται της εργασίας, όχι τα μεμονωμένα ενεργειών μέσα σε προγραμματισμένη ροής εργασίας. Για να δείτε που, επιλέξτε μία από τις καταχωρήσεις **ενέργεια** . Αυτό θα εμφανίσει πληροφορίες παρόμοιες με που ανακτήθηκαν για την προηγούμενη εργασία ροής εργασίας.

    ![Κατάσταση ενέργειας](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

##<a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων

Κατά την αντιμετώπιση προβλημάτων με τις εργασίες Oozie, το περιβάλλον εργασίας Χρήστη Oozie είναι πολύ χρήσιμο όπως σάς επιτρέπει να βλέπετε εύκολα και τα δύο αρχεία καταγραφής Oozie, καθώς και συνδέσεις στα αρχεία καταγραφής JobTracker για MapReduce εργασίες όπως η ομάδα ερωτημάτων. Σε γενικές γραμμές, θα πρέπει να το μοτίβο για την αντιμετώπιση προβλημάτων:

1. Δείτε την εργασία στο περιβάλλον εργασίας Χρήστη Web Oozie.

2. Εάν υπάρχει ένα σφάλμα ή αποτυχίας για μια συγκεκριμένη ενέργεια, επιλέξτε την ενέργεια για να δείτε εάν το πεδίο **Μήνυμα σφάλματος** παρέχει περισσότερες πληροφορίες σχετικά με την αποτυχία.

3. Εάν είναι διαθέσιμη, χρησιμοποιήστε τη διεύθυνση URL από την ενέργεια για να εμφανίσετε περισσότερες λεπτομέρειες (όπως JobTracker αρχεία καταγραφής,) για την ενέργεια.

Οι παρακάτω είναι συγκεκριμένα σφάλματα που μπορεί να αντιμετωπίσετε και τον τρόπο για να τα επιλύσετε.

###<a name="ja009-cannot-initialize-cluster"></a>JA009: Δεν είναι δυνατή προετοιμασία συμπλέγματος

**Συμπτώματα**: την κατάσταση της εργασίας θα αλλάξει σε **σε ΑΝΑΣΤΟΛΉ**. Λεπτομέρειες για το έργο θα εμφανίσει την κατάσταση RunHiveScript ως **START_MANUAL**. Επιλογή της ενέργειας θα αποκαλύψει το ακόλουθο μήνυμα σφάλματος:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**Αιτία**: Η WASB διευθύνσεις που χρησιμοποιούνται στο αρχείο **job.xml** δεν περιέχουν το κοντέινερ χώρου αποθήκευσης ή όνομα λογαριασμού χώρου αποθήκευσης. Πρέπει να είναι η μορφή διεύθυνσης WASB `wasbs://containername@storageaccountname.blob.core.windows.net`.

**Επίλυση**: Αλλάξτε τις διευθύνσεις WASB που χρησιμοποιούνται από την εργασία.

###<a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie δεν επιτρέπεται η μίμηση &lt;ΧΡΉΣΤΗ >

**Συμπτώματα**: την κατάσταση της εργασίας θα αλλάξει σε **σε ΑΝΑΣΤΟΛΉ**. Λεπτομέρειες για το έργο θα εμφανίσει την κατάσταση RunHiveScript ως **START_MANUAL**. Επιλογή της ενέργειας θα αποκαλύψει το ακόλουθο μήνυμα σφάλματος:

    JA002: User: oozie is not allowed to impersonate <USER>

**Αιτία**: τρέχουσες ρυθμίσεις δικαιωμάτων χωρίς να επιτρέπετε Oozie μίμηση τον συγκεκριμένο λογαριασμό χρήστη.

**Ανάλυση**: Oozie επιτρέπεται να μίμηση χρηστών στην ομάδα **χρηστών** . Χρησιμοποιήστε το `groups USERNAME` για να δείτε τις ομάδες στις οποίες ο λογαριασμός χρήστη είναι μέλος. Εάν ο χρήστης δεν είναι μέλος της ομάδας " **χρήστες** ", χρησιμοποιήστε την παρακάτω εντολή για να προσθέσετε το χρήστη στην ομάδα:

    sudo adduser USERNAME users

> [AZURE.NOTE] Ενδέχεται να χρειαστούν αρκετά λεπτά πριν να HDInsight αναγνωρίζει ότι ο χρήστης έχει προστεθεί στην ομάδα.

###<a name="launcher-error-sqoop"></a>Εκκίνηση ΣΦΆΛΜΑΤΟΣ (Sqoop)

**Συμπτώματα**: την κατάσταση της εργασίας θα αλλάξει σε **KILLED**. Λεπτομέρειες για το έργο θα εμφανίσει την κατάσταση RunSqoopExport ως **σφάλμα**. Επιλογή της ενέργειας θα αποκαλύψει το ακόλουθο μήνυμα σφάλματος:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**Αιτία**: Sqoop δεν είναι δυνατή η φόρτωση του προγράμματος οδήγησης βάσης δεδομένων που απαιτείται για την πρόσβαση στη βάση δεδομένων.

**Ανάλυση**: όταν χρησιμοποιείτε Sqoop από μια εργασία Oozie, θα πρέπει να συμπεριλάβετε το πρόγραμμα οδήγησης βάσης δεδομένων με τους άλλους πόρους (όπως το workflow.xml,) χρησιμοποιείται από την εργασία.

Πρέπει επίσης να γίνει αναφορά στην αρχειοθήκη που περιέχει το πρόγραμμα οδήγησης βάσης δεδομένων από το `<sqoop>...</sqoop>` ενότητας από το workflow.xml.

Για παράδειγμα, για την εργασία σε αυτό το έγγραφο, θα μπορείτε να χρησιμοποιήσετε τα παρακάτω βήματα:

1. Αντιγράψτε το αρχείο sqljdbc4.1.jar στον κατάλογο /tutorials/useoozie:

         hadoop fs -copyFromLocal /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar

2. Τροποποίηση του workflow.xml για να προσθέσετε τις ακόλουθες ενέργειες σε μια νέα γραμμή παραπάνω `</sqoop>`:

        <archive>sqljdbc41.jar</archive>

##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να ορίσετε μια ροή εργασίας Oozie και πώς μπορείτε να εκτελέσετε μια εργασία Oozie. Για να μάθετε περισσότερα σχετικά με την εργασία με το HDInsight, ανατρέξτε στα ακόλουθα άρθρα:

- [Χρήση βάσει χρόνου συντονισμού Oozie με HDInsight][hdinsight-oozie-coordinator-time]
- [Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight][hdinsight-upload-data]
- [Χρήση Sqoop με Hadoop σε HDInsight][hdinsight-use-sqoop]
- [Χρήση της ομάδας με Hadoop σε HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με Hadoop σε HDInsight][hdinsight-use-pig]
- [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-mapreduce]


[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]: storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

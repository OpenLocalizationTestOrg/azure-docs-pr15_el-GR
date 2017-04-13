<properties
    pageTitle="Χρήση βάσει χρόνου συντονισμού Hadoop Oozie σε HDInsight | Microsoft Azure"
    description="Χρησιμοποιήστε βάσει χρόνου συντονισμού Hadoop Oozie HDInsight, μια υπηρεσία μεγάλο δεδομένων. Μάθετε πώς μπορείτε να ορίσετε Oozie ροές εργασίας και συντονιστές, και να υποβάλλουν εργασίες."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jgao"/>


# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Χρήση βάσει χρόνου συντονισμού Oozie με Hadoop στο HDInsight να ορίσετε ροές εργασίας και να συντονίσετε εργασίες

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να ορίσετε ροές εργασίας και συντονιστές και πώς μπορείτε να ενεργοποιήσετε τις εργασίες συντονισμού, βάσει χρόνου. Είναι χρήσιμο να ακολουθήσετε [Χρήση Oozie με HDInsight] [ hdinsight-use-oozie] πριν Διαβάστε αυτό το άρθρο. Εκτός από την Oozie, μπορείτε επίσης να προγραμματίσετε εργασίες χρησιμοποιώντας Azure εργοστασίου δεδομένων. Για να μάθετε Azure εργοστασίου δεδομένων, ανατρέξτε στο θέμα [Χρήση γουρούνι και ομάδα με προέλευση δεδομένων](../data-factory/data-factory-data-transformation-activities.md).

> [AZURE.NOTE] Σε αυτό το άρθρο απαιτεί ένα σύμπλεγμα HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με τη χρήση Oozie, συμπεριλαμβανομένων των εργασιών βάσει χρόνου, σε ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [Χρήση Oozie με Hadoop να ορίσετε και να διαχειριστώ μια ροή εργασίας σε βάσει Linux HDInsight](hdinsight-use-oozie-linux-mac.md)

##<a name="what-is-oozie"></a>Τι είναι το Oozie

Apache Oozie είναι ένα σύστημα ροής εργασίας/συντονισμό που διαχειρίζεται το Hadoop εργασίες. Αυτό είναι συνδεδεμένο με την στοίβα Hadoop, καθώς και υποστηρίζει Hadoop εργασίες για Apache MapReduce, γουρούνι Apache, Apache Hive και Apache Sqoop. Μπορεί επίσης να χρησιμοποιηθεί για να προγραμματίζουν εργασίες που σχετίζονται με ένα σύστημα, όπως τα προγράμματα Java ή δεσμών ενεργειών κελύφους.

Η παρακάτω εικόνα δείχνει τη ροή εργασίας θα υλοποιήσετε:

![Διάγραμμα ροής εργασίας][img-workflow-diagram]

Η ροή εργασίας περιέχει δύο ενέργειες:

1. Μια ομάδα ενέργεια εκτελεί μια δέσμη ενεργειών HiveQL για την καταμέτρηση των εμφανίσεων κάθε τύπο επίπεδο καταγραφής σε ένα αρχείο καταγραφής log4j. Κάθε αρχείο καταγραφής log4j αποτελείται από μια γραμμή των πεδίων που περιέχει ένα πεδίο [ΕΠΊΠΕΔΟ ΚΑΤΑΓΡΑΦΉΣ] για να εμφανίσετε τον τύπο και της σοβαρότητας, για παράδειγμα:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    Δέσμη ενεργειών ομάδας είναι παρόμοια με:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Για περισσότερες πληροφορίες σχετικά με την ομάδα, ανατρέξτε στο θέμα [Χρήση Hive με HDInsight][hdinsight-use-hive].

2.  Μια ενέργεια Sqoop εξάγει το αποτέλεσμα ενέργειας HiveQL σε έναν πίνακα σε μια βάση δεδομένων Azure SQL. Για περισσότερες πληροφορίες σχετικά με το Sqoop, ανατρέξτε στο θέμα [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop].

> [AZURE.NOTE] Για υποστηριζόμενες εκδόσεις Oozie στην συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα που παρέχεται από το HDInsight;] [hdinsight-versions].


##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τα εξής:

- **Μια σταθμούς εργασίας με το Azure PowerShell**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Σύμπλεγμα του HDInsight**. Για πληροφορίες σχετικά με τη δημιουργία ένα σύμπλεγμα HDInsight, ανατρέξτε στο θέμα [Δημιουργία HDInsight συμπλεγμάτων][hdinsight-provision], ή [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]. Θα χρειαστείτε τα εξής δεδομένα για να μεταβείτε στο πρόγραμμα εκμάθησης:

    <table border = "1">
    <tr><th>Ιδιότητα συμπλέγματος</th><th>Το όνομα της μεταβλητής του Windows PowerShell</th><th>Τιμή</th><th>Περιγραφή</th></tr>
    <tr><td>Όνομα HDInsight συμπλέγματος</td><td>$clusterName</td><td></td><td>Το HDInsight σύμπλεγμα στην οποία θα εκτελέσετε αυτό το πρόγραμμα εκμάθησης.</td></tr>
    <tr><td>Όνομα χρήστη HDInsight συμπλέγματος</td><td>$clusterUsername</td><td></td><td>Το όνομα χρήστη σύμπλεγμα HDInsight. </td></tr>
    <tr><td>Σύμπλεγμα HDInsight τον κωδικό πρόσβασης χρήστη </td><td>$clusterPassword</td><td></td><td>Κωδικός πρόσβασης χρήστη σύμπλεγμα HDInsight.</td></tr>
    <tr><td>Όνομα λογαριασμού Azure χώρου αποθήκευσης</td><td>$storageAccountName</td><td></td><td>Ένας λογαριασμός αποθήκευσης Azure διαθέσιμη στο σύμπλεγμα HDInsight. Για αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης που καθορίσατε κατά τη διαδικασία παροχή σύμπλεγμα.</td></tr>
    <tr><td>Το όνομα του κοντέινερ αντικειμένων Blob Azure</td><td>$containerName</td><td></td><td>Για αυτό το παράδειγμα, χρησιμοποιήστε το κοντέινερ χώρου αποθήκευσης αντικειμένων Blob του Azure που χρησιμοποιείται για το προεπιλεγμένο σύστημα αρχείων σύμπλεγμα HDInsight. Από προεπιλογή, που έχει το ίδιο όνομα με το σύμπλεγμα HDInsight.</td></tr>
    </table>

- **Βάση δεδομένων του SQL Azure**. Πρέπει να ρυθμίσετε έναν κανόνα τείχους προστασίας για το διακομιστή βάσης δεδομένων SQL για να επιτρέψετε την πρόσβαση από το σταθμούς εργασίας. Για οδηγίες σχετικά με τη δημιουργία μιας SQL Azure βάσης δεδομένων και τη ρύθμιση των παραμέτρων του τείχους προστασίας, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με βάση δεδομένων Azure SQL][sqldatabase-get-started]. Σε αυτό το άρθρο παρέχει μια δέσμη ενεργειών του Windows PowerShell για τη δημιουργία πίνακα βάσης δεδομένων Azure SQL που χρειάζεστε για αυτό το πρόγραμμα εκμάθησης.

    <table border = "1">
    <tr><th>Ιδιότητα βάσης δεδομένων SQL</th><th>Το όνομα της μεταβλητής του Windows PowerShell</th><th>Τιμή</th><th>Περιγραφή</th></tr>
    <tr><td>Όνομα διακομιστή βάσης δεδομένων SQL</td><td>$sqlDatabaseServer</td><td></td><td>Το διακομιστή βάσης δεδομένων SQL στον οποίο Sqoop θα εξαγωγή δεδομένων. </td></tr>
    <tr><td>Όνομα σύνδεσης βάσης δεδομένων SQL</td><td>$sqlDatabaseLogin</td><td></td><td>Όνομα σύνδεσης βάσης δεδομένων SQL.</td></tr>
    <tr><td>Κωδικό πρόσβασης βάσης δεδομένων SQL</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Κωδικό πρόσβασης βάσης δεδομένων SQL.</td></tr>
    <tr><td>Όνομα βάσης δεδομένων SQL</td><td>$sqlDatabaseName</td><td></td><td>Η βάση δεδομένων Azure SQL στην οποία Sqoop θα εξαγωγή δεδομένων. </td></tr>
    </table>

    > [AZURE.NOTE] Από προεπιλογή μια βάση δεδομένων Azure SQL επιτρέπουν συνδέσεις από το Azure υπηρεσίες, όπως Azure HDInsight. Εάν αυτή η ρύθμιση τείχος προστασίας είναι απενεργοποιημένη, πρέπει να το ενεργοποιήσετε από την πύλη του Azure. Για οδηγίες σχετικά με τη δημιουργία μιας βάσης δεδομένων SQL και τη ρύθμιση των παραμέτρων κανόνες τείχους προστασίας, ανατρέξτε στο θέμα [Δημιουργία και ρύθμιση παραμέτρων της βάσης δεδομένων SQL][sqldatabase-get-started].


> [AZURE.NOTE] Συμπλήρωση των τιμών σε πίνακες. Θα είναι χρήσιμο για διέρχονται από αυτό το πρόγραμμα εκμάθησης.


##<a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Ορισμός Oozie ροής εργασίας και το σχετικό δέσμης ενεργειών HiveQL

Ορισμοί ροές εργασίας Oozie είναι γραμμένες σε hPDL (μια γλώσσα XML διαδικασία ορισμού). Το προεπιλεγμένο όνομα αρχείου ροής εργασίας είναι *workflow.xml*.  Θα αποθηκεύσετε το αρχείο ροής εργασίας τοπικά και, στη συνέχεια, να το αναπτύξετε στο σύμπλεγμα HDInsight με χρήση του Azure PowerShell παρακάτω σε αυτό το πρόγραμμα εκμάθησης.

Η ενέργεια Hive στη ροή εργασίας καλεί ένα αρχείο δέσμης ενεργειών HiveQL. Αυτό το αρχείο δέσμης ενεργειών περιέχει τρεις δηλώσεις HiveQL:

1. **Πρόταση DROP TABLE το** διαγράφει τον πίνακα Hive log4j, εάν υπάρχει.
2. **Πρόταση CREATE TABLE το** δημιουργεί log4j Hive εξωτερικών πίνακα, που υποδεικνύει τη θέση του αρχείου καταγραφής log4j;
3.  **Τη θέση του αρχείου καταγραφής log4j**. Ο οριοθέτης πεδίου είναι ",". Ο προεπιλεγμένος οριοθέτης γραμμή είναι "\n". Για να αποφύγετε το αρχείο δεδομένων που αφαιρείται από την αρχική του θέση, σε περίπτωση που θέλετε να εκτελέσετε τη ροή εργασίας Oozie πολλές φορές χρησιμοποιείται ένας πίνακας εξωτερική ομάδα.
3. **Η εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ δήλωση** μετρά τις εμφανίσεις κάθε τύπο επίπεδο καταγραφής από τον πίνακα ομάδα log4j και αποθηκεύει το αποτέλεσμα σε μια θέση αποθήκευσης αντικειμένων Blob του Azure.

**Σημείωση**: είναι ένα γνωστό θέμα διαδρομή της ομάδας. Θα αντιμετωπίσετε αυτό το πρόβλημα κατά την υποβολή μιας εργασίας Oozie. Μπορείτε να βρείτε τις οδηγίες για τη διόρθωση του ζητήματος στο TechNet Wiki: [HDInsight Hive σφάλμα: δεν είναι δυνατή η μετονομασία][technetwiki-hive-error].

**Για να ορίσετε το αρχείο δέσμης ενεργειών HiveQL ώστε να καλείται από τη ροή εργασίας**

1. Δημιουργήστε ένα αρχείο κειμένου με το παρακάτω περιεχόμενο:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Υπάρχουν τρεις μεταβλητές που χρησιμοποιούνται στη δέσμη ενεργειών:

    - ${hiveTableName}
    - ${hiveDataFolder}
    - ${hiveOutputFolder}

    Το αρχείο ορισμού ροής εργασίας (workflow.xml σε αυτό το πρόγραμμα εκμάθησης) θα μεταβιβάζουν αυτές τις τιμές για αυτήν τη δέσμη ενεργειών HiveQL κατά το χρόνο εκτέλεσης.

2. Αποθηκεύστε το αρχείο ως **C:\Tutorials\UseOozie\useooziewf.hql** χρησιμοποιώντας κωδικοποίηση ANSI (ASCII). (Χρησιμοποιήστε το Σημειωματάριο, εάν το πρόγραμμα επεξεργασίας κειμένου δεν παρέχει αυτήν την επιλογή.) Αυτό το αρχείο δέσμης ενεργειών πρόκειται να αναπτυχθούν στο σύμπλεγμα HDInsight αργότερα στην εκμάθηση.



**Για να ορίσετε μια ροή εργασίας**

1. Δημιουργήστε ένα αρχείο κειμένου με το παρακάτω περιεχόμενο:

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
                    <param>hiveOutputFolder=${hiveOutputFolder}</param>
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
                <arg>${hiveOutputFolder}</arg>
                <arg>-m</arg>
                <arg>1</arg>
                <arg>--input-fields-terminated-by</arg>
                <arg>"\001"</arg>
                </sqoop>
                <ok to="end"/>
                <error to="fail"/>
            </action>

            <kill name="fail">
                <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
            </kill>

           <end name="end"/>
        </workflow-app>

    Υπάρχουν δύο ενέργειες που ορίζονται στη ροή εργασίας. Η ενέργεια έναρξης να είναι *RunHiveScript*. Εάν η ενέργεια εκτελείται *OK*, η επόμενη ενέργεια είναι *RunSqoopExport*.

    Το RunHiveScript έχει πολλές μεταβλητές. Θα μπορείτε να μεταβιβάσετε τις τιμές κατά την υποβολή της εργασίας Oozie από το σταθμούς εργασίας με χρήση του Azure PowerShell.

    <table border = "1">
    <tr><th>Μεταβλητές ροής εργασιών</th><th>Περιγραφή</th></tr>
    <tr><td>${jobTracker}</td><td>Καθορίστε τη διεύθυνση URL του εργαλείου παρακολούθησης εργασία Hadoop. Χρησιμοποιήστε <strong>jobtrackerhost:9010</strong> HDInsight σύμπλεγμα έκδοση 3.0 και 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Καθορίστε τη διεύθυνση URL του κόμβου όνομα Hadoop. Χρησιμοποιήστε το προεπιλεγμένο αρχείο συστήματος wasbs: / / διεύθυνση, για παράδειγμα, <i>wasbs: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${Όνομα_ουράς}</td><td>Καθορίζει το όνομα ουράς που θα υποβάλλονται την εργασία. Χρησιμοποιήστε την <strong>προεπιλεγμένη</strong>.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Ομάδα ενέργεια μεταβλητή</th><th>Περιγραφή</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Ο κατάλογος προέλευσης για την εντολή Hive Δημιουργία πίνακα.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Ο φάκελος εξόδου για τη δήλωση εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ.</td></tr>
    <tr><td>${hiveTableName}</td><td>Το όνομα του πίνακα Hive που αναφέρεται τα αρχεία δεδομένων log4j.</td></tr>
    </table>


    <table border = "1">
    <tr><th>Μεταβλητή ενέργεια Sqoop</th><th>Περιγραφή</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Συμβολοσειρά σύνδεσης βάσης δεδομένων SQL.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>Ο πίνακας βάσης δεδομένων Azure SQL στο σημείο όπου θα γίνει εξαγωγή των δεδομένων.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Ο φάκελος εξόδου για τη δήλωση Hive εισαγωγή ΑΝΤΙΚΑΤΆΣΤΑΣΗ. Αυτό είναι στον ίδιο φάκελο για την εξαγωγή Sqoop (εξαγωγή-dir).</td></tr>
    </table>

    Για περισσότερες πληροφορίες σχετικά με τη ροή εργασίας Oozie και χρησιμοποιώντας τις ενέργειες ροής εργασίας, ανατρέξτε [στην τεκμηρίωση Apache Oozie 4.0] [ apache-oozie-400] (για το HDInsight σύμπλεγμα έκδοση 3.0) ή [τεκμηρίωση Apache Oozie 3.3.2] [ apache-oozie-332] (για το HDInsight σύμπλεγμα έκδοση 2.1).

2. Αποθηκεύστε το αρχείο ως **C:\Tutorials\UseOozie\workflow.xml** χρησιμοποιώντας κωδικοποίηση ANSI (ASCII). (Χρησιμοποιήστε το Σημειωματάριο, εάν το πρόγραμμα επεξεργασίας κειμένου δεν παρέχει αυτήν την επιλογή.)

**Για να ορίσετε συντονισμού**

1. Δημιουργήστε ένα αρχείο κειμένου με το παρακάτω περιεχόμενο:

        <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
           <action>
              <workflow>
                 <app-path>${wfPath}</app-path>
              </workflow>
           </action>
        </coordinator-app>

    Υπάρχουν πέντε μεταβλητές που χρησιμοποιούνται στο αρχείο ορισμού:

  	| Μεταβλητή          | Περιγραφή |
  	| ------------------|------------ |
  	| ${coordFrequency} | Παύση χρόνου εργασίας. Συχνότητα πάντα εκφράζεται σε λεπτά. |
  	| ${coordStart}     | Ώρα έναρξης του έργου. |
  	| ${coordEnd}       | Ώρα λήξης του έργου. |
  	| ${coordTimezone}  | Oozie επεξεργάζεται συντονισμού εργασιών σε μια σταθερή ζώνη ώρας με καμία θερινή ώρα (συνήθως αναπαριστώνται με τη χρήση UTC). Αυτή η ζώνη ώρας αναφέρεται ως είναι το "Επεξεργασία Oozie ζώνης ώρας." |
  	| ${wfPath}         | Η διαδρομή για το workflow.xml.  Εάν το όνομα του αρχείου ροής εργασίας δεν είναι το προεπιλεγμένο όνομα αρχείου (workflow.xml), πρέπει να καθορίσετε το. |

2. Αποθηκεύστε το αρχείο ως **C:\Tutorials\UseOozie\coordinator.xml** χρησιμοποιώντας την κωδικοποίηση ANSI (ASCII). (Χρησιμοποιήστε το Σημειωματάριο, εάν το πρόγραμμα επεξεργασίας κειμένου δεν παρέχει αυτήν την επιλογή.)

##<a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Αναπτύξτε το έργο Oozie και να προετοιμάσετε το πρόγραμμα εκμάθησης

Θα μπορείτε να εκτελέσετε μια δέσμη ενεργειών του Azure PowerShell για να εκτελέσετε τις ακόλουθες ενέργειες:

- Αντιγράψτε τη δέσμη ενεργειών HiveQL (useoozie.hql) με το χώρο αποθήκευσης αντικειμένων Blob του Azure, wasbs:///tutorials/useoozie/useoozie.hql.
- Αντιγραφή workflow.xml wasbs:///tutorials/useoozie/workflow.xml.
- Αντιγραφή coordinator.xml wasbs:///tutorials/useoozie/coordinator.xml.
- Αντιγράψτε το αρχείο δεδομένων (/ example/data/sample.log) για να wasbs:///tutorials/useoozie/data/sample.log.
- Δημιουργήστε έναν πίνακα βάσης δεδομένων Azure SQL για την αποθήκευση Sqoop εξαγωγή δεδομένων. Το όνομα του πίνακα είναι *log4jLogCount*.

**Κατανόηση των HDInsight χώρου αποθήκευσης**

HDInsight χρησιμοποιεί χώρο αποθήκευσης αντικειμένων Blob του Azure για την αποθήκευση δεδομένων. wasbs: / / του συστήματος αρχείων κατανεμημένο Hadoop (HDFS) στο χώρο αποθήκευσης αντικειμένων Blob του Azure την υλοποίηση της Microsoft. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [χώρο αποθήκευσης αντικειμένων Blob του Azure χρήση με το HDInsight][hdinsight-storage].

Κατά την προμήθεια ένα σύμπλεγμα HDInsight, ένα λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure και ένα συγκεκριμένο κοντέινερ από αυτόν το λογαριασμό έχει οριστεί ως το προεπιλεγμένο σύστημα αρχείων, όπως στο HDFS. Εκτός από αυτόν το λογαριασμό χώρου αποθήκευσης, μπορείτε να προσθέσετε επιπλέον χώρο αποθήκευσης λογαριασμούς από την ίδια συνδρομή Azure ή από διάφορες συνδρομές του Azure κατά τη διάρκεια της διαδικασίας προετοιμασίας. Για οδηγίες σχετικά με την προσθήκη λογαριασμών επιπλέον χώρο αποθήκευσης, ανατρέξτε στο θέμα [συμπλεγμάτων παροχή HDInsight][hdinsight-provision]. Για να απλοποιήσετε τη δέσμη ενεργειών Azure PowerShell που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης, όλα τα αρχεία αποθηκεύονται στο κοντέινερ συστήματος προεπιλεγμένο αρχείο βρίσκεται στο */tutorials/useoozie*. Από προεπιλογή, αυτό το κοντέινερ έχει το ίδιο όνομα με το όνομα του συμπλέγματος HDInsight.
Η σύνταξη είναι:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [AZURE.NOTE] Μόνο τα *wasb: / /* σύνταξη υποστηρίζεται στο σύμπλεγμα HDInsight έκδοση 3.0. Το παλαιότερο *asv: / /* σύνταξη υποστηρίζεται στο HDInsight 2.1 και 1,6 συμπλεγμάτων, αλλά δεν υποστηρίζεται στο συμπλεγμάτων HDInsight 3.0.

> [AZURE.NOTE] Το wasb: / / διαδρομή είναι μια εικονική διαδρομή. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα [χώρο αποθήκευσης αντικειμένων Blob του Azure χρήση με το HDInsight][hdinsight-storage].

Ένα αρχείο που είναι αποθηκευμένο στο προεπιλεγμένο αρχείο συστήματος κοντέινερ είναι προσβάσιμα από το HDInsight, χρησιμοποιώντας οποιαδήποτε των το παρακάτω URI (χρησιμοποιώ workflow.xml ως παράδειγμα):

    wasbs://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasbs:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Εάν θέλετε να έχετε πρόσβαση στο αρχείο απευθείας από το λογαριασμό χώρου αποθήκευσης, το όνομα blob για το αρχείο είναι:

    tutorials/useoozie/workflow.xml

**Κατανόηση των πινάκων εσωτερικές και εξωτερικές ομάδας**

Υπάρχουν μερικά πράγματα που πρέπει να γνωρίζετε σχετικά με τους πίνακες εσωτερικές και εξωτερικές ομάδα:

- Η εντολή ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ δημιουργεί ένας εσωτερικός πίνακας, γνωστό και ως διαχειριζόμενη πίνακα. Το αρχείο δεδομένων πρέπει να βρίσκεται στο προεπιλεγμένο κοντέινερ.
- Η εντολή ΔΗΜΙΟΥΡΓΊΑ ΠΊΝΑΚΑ μετακινεί το αρχείο δεδομένων του /hive/αποθήκης/<TableName> φακέλου στο προεπιλεγμένο κοντέινερ.
- Η εντολή ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΏΝ ΠΊΝΑΚΑ δημιουργεί έναν εξωτερικό πίνακα. Το αρχείο δεδομένων μπορεί να βρίσκεται έξω από το προεπιλεγμένο κοντέινερ.
- Η εντολή ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌΣ ΠΊΝΑΚΑΣ δεν μετακινείται στο αρχείο δεδομένων.
- Η εντολή ΔΗΜΙΟΥΡΓΊΑ ΕΞΩΤΕΡΙΚΌΣ ΠΊΝΑΚΑΣ δεν σας επιτρέπει των υποφακέλων κάτω από το φάκελο που καθορίζεται στον όρο του ΘΈΣΗ. Αυτή είναι η αιτία γιατί το πρόγραμμα εκμάθησης δημιουργεί ένα αντίγραφο του αρχείου sample.log.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [HDInsight: Hive εσωτερικές και εξωτερικές Εισαγωγή πινάκων][cindygross-hive-tables].

**Για να προετοιμάσετε το πρόγραμμα εκμάθησης**

1. Ανοίξτε το Windows PowerShell ISE (στην οθόνη έναρξης των Windows 8, πληκτρολογήστε **PowerShell_ISE**και, στη συνέχεια, κάντε κλικ στην επιλογή **Windows PowerShell ISE**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Έναρξη του Windows PowerShell στα Windows 8 και Windows][powershell-start]).
2. Στο κάτω τμήμα του παραθύρου, εκτελέστε την ακόλουθη εντολή για να συνδεθείτε με τη συνδρομή σας στο Azure:

        Add-AzureAccount

    Θα σας ζητηθεί να εισαγάγετε τα διαπιστευτήριά σας λογαριασμός Azure. Προσθήκη μιας σύνδεσης συνδρομή αυτή η μέθοδος λήγει το χρονικό όριο και μετά από 12 ώρες, θα πρέπει να εκτελέσετε το cmdlet ξανά.

    > [AZURE.NOTE] Εάν έχετε πολλές συνδρομές Azure και την προεπιλεγμένη συνδρομή σας δεν είναι αυτό που θέλετε να χρησιμοποιήσετε, χρησιμοποιήστε το cmdlet <strong>AzureSubscription επιλογή</strong> για να επιλέξετε μια συνδρομή.

3. Αντιγράψτε την ακόλουθη δέσμη ενεργειών στο παράθυρο δέσμης ενεργειών και, στη συνέχεια, ορίστε τις μεταβλητές πρώτα έξι:

        # WASB variables
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<BlobStorageContainerName>"

        # SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"  
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  
        $sqlDatabaseTableName = "log4jLogsCount"

        # Oozie files for the tutorial
        $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
        $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
        $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

        # WASB folder for storing the Oozie tutorial files.
        $destFolder = "tutorials/useoozie"  # Do NOT use the long path here


    Για περισσότερες περιγραφές των μεταβλητών, ανατρέξτε στην ενότητα [προϋποθέσεις](#prerequisites) σε αυτό το πρόγραμμα εκμάθησης.

3. Προσάρτηση τα εξής στη δέσμη ενεργειών στο τμήμα παραθύρου δέσμης ενεργειών:

        # Create a storage context object
        $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        function uploadOozieFiles()
        {
            Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
            Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
            Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
            Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
        }

        function prepareHiveDataFile()
        {
            Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
            Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
        }

        function prepareSQLDatabase()
        {
            # SQL query string for creating log4jLogsCount table
            $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [Level] [nvarchar](10) NOT NULL,
                    [Total] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                [Level] ASC
                )
                )"

            #Create the log4jLogsCount table
            Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
            $conn = New-Object System.Data.SqlClient.SqlConnection
            $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
            $conn.open()
            $cmd = New-Object System.Data.SqlClient.SqlCommand
            $cmd.connection = $conn
            $cmd.commandtext = $cmdCreateLog4jCountTable
            $cmd.executenonquery()

            $conn.close()
        }

        # upload workflow.xml, coordinator.xml, and ooziewf.hql
        uploadOozieFiles;

        # make a copy of example/data/sample.log to example/data/log4j/sample.log
        prepareHiveDataFile;

        # create log4jlogsCount table on SQL database
        prepareSQLDatabase;

4. Κάντε κλικ στην επιλογή **Εκτέλεση της δέσμης ενεργειών** ή πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών. Το αποτέλεσμα θα είναι παρόμοιο με:

    ![Προετοιμασία προγραμμάτων εκμάθησης εξόδου][img-preparation-output]

##<a name="run-the-oozie-project"></a>Εκτελέστε το έργο Oozie

Azure PowerShell αυτήν τη στιγμή δεν παρέχει οποιαδήποτε cmdlet του για τον ορισμό Oozie εργασίες. Μπορείτε να χρησιμοποιήσετε το cmdlet **Ενεργοποίηση RestMethod** για να καλέσετε Oozie υπηρεσιών web. Το API των υπηρεσιών web Oozie είναι μια REST API JSON HTTP. Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες web του Oozie API, ανατρέξτε [στην τεκμηρίωση Apache Oozie 4.0] [ apache-oozie-400] (για το HDInsight σύμπλεγμα έκδοση 3.0) ή [τεκμηρίωση Apache Oozie 3.3.2] [ apache-oozie-332] (για το HDInsight σύμπλεγμα έκδοση 2.1).

**Για να υποβάλετε μια εργασία Oozie**

1. Ανοίξτε το Windows PowerShell ISE (στην οθόνη έναρξης των Windows 8, πληκτρολογήστε **PowerShell_ISE**και, στη συνέχεια, κάντε κλικ στην επιλογή **Windows PowerShell ISE**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Έναρξη του Windows PowerShell στα Windows 8 και Windows][powershell-start]).

3. Αντιγράψτε την ακόλουθη δέσμη ενεργειών στο παράθυρο δέσμης ενεργειών και, στη συνέχεια, ορίστε τις μεταβλητές πρώτα δεκατέσσερα (Ωστόσο, παραλείψτε **$storageUri**).

        #HDInsight cluster variables
        $clusterName = "<HDInsightClusterName>"
        $clusterUsername = "<HDInsightClusterUsername>"
        $clusterPassword = "<HDInsightClusterUserPassword>"

        #Azure Blob storage (WASB) variables
        $storageAccountName = "<StorageAccountName>"
        $storageContainerName = "<BlobContainerName>"
        $storageUri="wasbs://$storageContainerName@$storageAccountName.blob.core.windows.net"

        #Azure SQL database variables
        $sqlDatabaseServer = "<SQLDatabaseServerName>"
        $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
        $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
        $sqlDatabaseName = "<SQLDatabaseName>"  

        #Oozie WF/coordinator variables
        $coordStart = "2014-03-21T13:45Z"
        $coordEnd = "2014-03-21T13:45Z"
        $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
        $coordTimezone = "UTC"  #UTC/GMT

        $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
        $waitTimeBetweenOozieJobStatusCheck=10

        #Hive action variables
        $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
        $hiveTableName = "log4jlogs"
        $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
        $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

        #Sqoop action variables
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
        $sqlDatabaseTableName = "log4jLogsCount"

        $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    Για περισσότερες περιγραφές των μεταβλητών, ανατρέξτε στην ενότητα [προϋποθέσεις](#prerequisites) σε αυτό το πρόγραμμα εκμάθησης.

    $coordstart και $coordend είναι η ροή εργασίας ώρα έναρξης και λήξης. Για να βρείτε την ώρα UTC/Γκρίνουιτς, αναζήτηση "ώρα utc" στην τοποθεσία bing.com. Το $coordFrequency είναι πόσο συχνά σε λεπτά που θέλετε να εκτελέσετε τη ροή εργασίας.

3. Προσάρτηση τα εξής στη δέσμη ενεργειών. Αυτό το τμήμα ορίζει το φορτίο Oozie:

        #OoziePayload used for Oozie web service submission
        $OoziePayload =  @"
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>

           <property>
               <name>nameNode</name>
               <value>$storageUrI</value>
           </property>

           <property>
               <name>jobTracker</name>
               <value>jobtrackerhost:9010</value>
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
               <name>oozie.coord.application.path</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>wfPath</name>
               <value>$oozieWFPath</value>
           </property>

           <property>
               <name>coordStart</name>
               <value>$coordStart</value>
           </property>

           <property>
               <name>coordEnd</name>
               <value>$coordEnd</value>
           </property>

           <property>
               <name>coordFrequency</name>
               <value>$coordFrequency</value>
           </property>

           <property>
               <name>coordTimezone</name>
               <value>$coordTimezone</value>
           </property>

           <property>
               <name>hiveScript</name>
               <value>$hiveScript</value>
           </property>

           <property>
               <name>hiveTableName</name>
               <value>$hiveTableName</value>
           </property>

           <property>
               <name>hiveDataFolder</name>
               <value>$hiveDataFolder</value>
           </property>

           <property>
               <name>hiveOutputFolder</name>
               <value>$hiveOutputFolder</value>
           </property>

           <property>
               <name>sqlDatabaseConnectionString</name>
               <value>&quot;$sqlDatabaseConnectionString&quot;</value>
           </property>

           <property>
               <name>sqlDatabaseTableName</name>
               <value>$SQLDatabaseTableName</value>
           </property>

           <property>
               <name>user.name</name>
               <value>admin</value>
           </property>

        </configuration>
        "@

    >[AZURE.NOTE] Η κύρια διαφορά σε σύγκριση με το αρχείο φορτίου υποβολής ροής εργασίας είναι η μεταβλητή **oozie.coord.application.path**. Κατά την υποβολή μιας εργασίας ροής εργασίας, μπορείτε χρησιμοποιήσετε **oozie.wf.application.path** αντί για αυτό.

4. Προσάρτηση τα εξής στη δέσμη ενεργειών. Αυτό το τμήμα ελέγχει την κατάσταση της υπηρεσίας web Oozie:

        function checkOozieServerStatus()
        {
            Write-Host "Checking Oozie server status..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieServerSatus = $jsonResponse[0].("systemMode")
            Write-Host "Oozie server status is $oozieServerSatus..."

            if($oozieServerSatus -notmatch "NORMAL")
            {
                Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check the server status and re-run the job."
                exit 1
            }
        }

5. Προσάρτηση τα εξής στη δέσμη ενεργειών. Αυτό το τμήμα δημιουργεί μια εργασία Oozie:

        function createOozieJob()
        {
            # create Oozie job
            Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
            Write-Host "`n--------`n$OoziePayload`n--------"
            $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $oozieJobId = $jsonResponse[0].("id")
            Write-Host "Oozie job id is $oozieJobId..."

            return $oozieJobId
        }

    > [AZURE.NOTE] Κατά την υποβολή μιας εργασίας ροής εργασίας, πρέπει να κάνετε κάποια άλλη υπηρεσία web κλήση για να ξεκινήσει η εργασία, αφού δημιουργηθεί η εργασία. Σε αυτήν την περίπτωση, η εργασία συντονισμού ενεργοποιείται με βάση το χρόνο. Η εργασία θα ξεκινήσει αυτόματα.

6. Προσάρτηση τα εξής στη δέσμη ενεργειών. Αυτό το τμήμα ελέγχει την κατάσταση της εργασίας Oozie:

        function checkOozieJobStatus($oozieJobId)
        {
            # get job status
            Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

            Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
            $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")

            while($JobStatus -notmatch "SUCCEEDED|KILLED")
            {
                Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
                Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
                $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
                $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
                $JobStatus = $jsonResponse[0].("status")
            }

            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
            if($JobStatus -notmatch "SUCCEEDED")
            {
                Write-Host "Check logs at http://headnode0:9014/cluster for detais."
                exit -1
            }
        }

7. (Προαιρετικό) Προσάρτηση τα εξής στη δέσμη ενεργειών.

        function listOozieJobs()
        {
            Write-Host "Listing Oozie jobs..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

            write-host "Job ID                                   App Name        Status      Started                         Ended"
            write-host "----------------------------------------------------------------------------------------------------------------------------------"
            foreach($job in $response.workflows)
            {
                Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
            }
        }

        function ShowOozieJobLog($oozieJobId)
        {
            Write-Host "Showing Oozie job info..." -ForegroundColor Green
            $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
            write-host $response
        }

        function killOozieJob($oozieJobId)
        {
            Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
            $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
            $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
        }

7. Προσάρτηση τα εξής στη δέσμη ενεργειών:

        checkOozieServerStatus
        # listOozieJobs
        $oozieJobId = createOozieJob($oozieJobId)
        checkOozieJobStatus($oozieJobId)
        # ShowOozieJobLog($oozieJobId)
        # killOozieJob($oozieJobId)

    Καταργήστε τα σύμβολα #, εάν θέλετε να εκτελέσετε τις πρόσθετες λειτουργίες.

7. Εάν το σύμπλεγμά σας HDinsight είναι έκδοση 2.1, αντικαταστήστε "https://$clusterName.azurehdinsight.net:443/oozie/v2/" με "https://$clusterName.azurehdinsight.net:443/oozie/v1/". Έκδοση σύμπλεγμα HDInsight 2.1 δεν δεν υποστηρίζει την έκδοση 2 τις υπηρεσίες web.

7. Κάντε κλικ στην επιλογή **Εκτέλεση της δέσμης ενεργειών** ή πατήστε το πλήκτρο **F5** για να εκτελέσετε τη δέσμη ενεργειών. Το αποτέλεσμα θα είναι παρόμοιο με:

    ![Πρόγραμμα εκμάθησης εκτέλεση έξοδος ροής εργασίας][img-runworkflow-output]

8. Σύνδεση με τη βάση δεδομένων SQL για να δείτε τα εξαγόμενα δεδομένα.

**Για να ελέγξετε το αρχείο καταγραφής σφαλμάτων εργασία**

Για την αντιμετώπιση προβλημάτων σε μια ροή εργασίας, μπορείτε να βρείτε το αρχείο καταγραφής Oozie στο C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log από το headnode σύμπλεγμα. Για πληροφορίες σχετικά με RDP, ανατρέξτε στο θέμα [Διαχείριση HDInsight συμπλεγμάτων με την πύλη Azure][hdinsight-admin-portal].

**Για να εκτελέσετε ξανά το πρόγραμμα εκμάθησης**

Για να επαναλάβετε την εκτέλεση της ροής εργασίας, πρέπει να εκτελέσετε τις ακόλουθες εργασίες:

- Διαγράψτε το αρχείο εξόδου δέσμης ενεργειών ομάδας.
- Διαγράψτε τα δεδομένα στον πίνακα log4jLogsCount.

Ακολουθεί ένα δείγμα δέσμης ενεργειών του Windows PowerShell που μπορείτε να χρησιμοποιήσετε:

    $storageAccountName = "<AzureStorageAccountName>"
    $containerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete the Hive script output file ..." -ForegroundColor Green
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

    Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()


##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το πρόγραμμα εκμάθησης, μάθατε πώς μπορείτε να ορίσετε μια ροή εργασίας Oozie και μια Oozie συντονισμού και πώς μπορείτε να εκτελέσετε μια εργασία συντονισμού Oozie με χρήση του Azure PowerShell. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

- [Γρήγορα αποτελέσματα με το HDInsight][hdinsight-get-started]
- [Χρήση του χώρου αποθήκευσης αντικειμένων Blob του Azure με HDInsight][hdinsight-storage]
- [Διαχείριση HDInsight με χρήση του Azure PowerShell][hdinsight-admin-powershell]
- [Αποστολή δεδομένων σε HDInsight][hdinsight-upload-data]
- [Χρήση Sqoop με HDInsight][hdinsight-use-sqoop]
- [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
- [Ανάπτυξη προγραμμάτων Java MapReduce για HDInsight][hdinsight-develop-java-mapreduce]



[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563


[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: ../storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: ../powershell-install-configure.md
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png  

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

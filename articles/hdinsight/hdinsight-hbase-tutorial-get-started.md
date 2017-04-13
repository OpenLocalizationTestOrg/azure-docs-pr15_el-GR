<properties
    pageTitle="Πρόγραμμα εκμάθησης HBase: γρήγορα αποτελέσματα με το HBase στο Hadoop | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης HBase για να ξεκινήσετε να χρησιμοποιείτε Apache HBase με Hadoop σε HDInsight. Δημιουργία πινάκων από το κέλυφος HBase και τους ερωτήματος με χρήση της ομάδας."
    keywords="hbase Apache, hbase, hbase κέλυφος, hbase πρόγραμμα εκμάθησης"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>Πρόγραμμα εκμάθησης HBase: γρήγορα αποτελέσματα με το Apache HBase με Hadoop στο HDInsight που βασίζεται σε Windows

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Μάθετε πώς μπορείτε να δημιουργήσετε συμπλεγμάτων HBase στο HDInsight, δημιουργία HBase πινάκων και ερώτημα των πινάκων, χρησιμοποιώντας Apache Hive. Για γενικές πληροφορίες HBase, ανατρέξτε στο θέμα [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview].

Οι πληροφορίες σε αυτό το έγγραφο είναι συγκεκριμένη για συμπλεγμάτων HDInsight που βασίζεται στα Windows. Για πληροφορίες σχετικά με συμπλεγμάτων που βασίζεται στα Windows, χρησιμοποιήστε στον επιλογέα στηλοθετών στο επάνω μέρος της σελίδας για να αλλάξετε.

> [AZURE.NOTE] HBase (έκδοση 0.98.0) στην HDInsight που βασίζεται στα Windows είναι διαθέσιμη μόνο για χρήση με το HDInsight 3.1 συμπλεγμάτων (βάσει Apache Hadoop και ΝΉΜΑΤΑ 2.4.0). Για πληροφορίες έκδοσης, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα Hadoop που παρέχεται από το HDInsight;][hdinsight-versions]

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης HBase, πρέπει να έχετε τα εξής:

- **A Microsoft Azure συνδρομής**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Μια σταθμούς εργασίας** με το Visual Studio 2013 ή μεγαλύτερη: για οδηγίες, ανατρέξτε στο θέμα [Εγκατάσταση του Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Δημιουργία HBase συμπλέγματος

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Για να δημιουργήσετε ένα σύμπλεγμα HBase χρησιμοποιώντας την πύλη του Azure**

1. Είσοδος στην [πύλη του Azure][azure-management-portal].
2. Κάντε κλικ στην επιλογή **Δημιουργία** ή **+** επάνω αριστερά γωνία και, στη συνέχεια, κάντε κλικ στην επιλογή **δεδομένα + ανάλυση**, **HDInsight**.
3. Εισαγάγετε τις παρακάτω τιμές:

    - **Όνομα συμπλέγματος** - πληκτρολογήστε ένα όνομα για τον προσδιορισμό αυτό το σύμπλεγμα.
    - **Τύπος σύμπλεγμα** - επιλέξτε **HBase**.
    - **Σύμπλεγμα λειτουργικό σύστημα** - επιλογή **Windows**.  Για τη δημιουργία βάσει Linux HBase συμπλέγματος, ανατρέξτε στο θέμα [HBase πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με τη χρήση Apache HBase με Hadoop στο HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Έκδοση** - επιλέξτε μια έκδοση HBase.
    - **Συνδρομή** - επιλέξτε Azure τη συνδρομή σας στο χρησιμοποιούνται για τη δημιουργία αυτού του συμπλέγματος.
    - **Ομάδα πόρων** - Δημιουργία νέας ομάδας πόρων Azure ή επιλέξτε ένα υπάρχον. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Επισκόπηση διαχείρισης πόρων Azure](azure-resource-manager/resource-group-overview.md)
    - **Διαπιστευτήρια** - για το σύμπλεγμα με βάση τα Windows, μπορείτε να δημιουργήσετε ένα σύμπλεγμα χρήστη (χρήστη a.k.a HTTP, χρήστη υπηρεσίας web HTTP) και ένα χρήστη απομακρυσμένης επιφάνειας εργασίας. Κάντε κλικ στην επιλογή **Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας** για να προσθέσετε τα διαπιστευτήρια του χρήστη απομακρυσμένης επιφάνειας εργασίας. Στην επόμενη ενότητα απαιτεί RDP.
    - **Προέλευση δεδομένων** - Δημιουργήστε έναν νέο λογαριασμό Azure χώρου αποθήκευσης ή επιλέξτε έναν υπάρχοντα λογαριασμό Azure χώρου αποθήκευσης που θα χρησιμοποιηθεί ως το προεπιλεγμένο σύστημα αρχείων για το σύμπλεγμα. Η προεπιλεγμένη θέση αποθήκευσης λογαριασμού Καθορίζει τη θέση της θέσης σύμπλεγμα. Τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης και το σύμπλεγμα πρέπει να εντοπίσετε από κοινού στο ίδιο κέντρο δεδομένων.
    - **Επίπεδα τιμολόγησης κόμβου** - επιλέξτε τον αριθμό των διακομιστών περιοχής για το σύμπλεγμα HBase

        > [AZURE.WARNING] Για τη διαθεσιμότητα των υπηρεσιών HBase υψηλή, πρέπει να δημιουργήσετε ένα σύμπλεγμα που περιέχει τουλάχιστον **τρεις** κόμβους. Αυτό εξασφαλίζει ότι, εάν έναν κόμβο μεταβαίνει προς τα κάτω, οι περιοχές δεδομένα HBase είναι διαθέσιμα σε άλλους κόμβους.

        > Εάν μαθαίνετε HBase, πάντα επιλέξτε 1 για το μέγεθος του συμπλέγματος και διαγράψτε το σύμπλεγμα μετά από κάθε χρήση για να μειώσετε το κόστος.

    - **Προαιρετική ρύθμιση παραμέτρων** - ρύθμιση παραμέτρων Azure εικονικού δικτύου, ρύθμιση παραμέτρων δέσμης ενεργειών και προσθήκη λογαριασμών επιπλέον χώρο αποθήκευσης.

4. Κάντε κλικ στην επιλογή **Δημιουργία**.

>[AZURE.NOTE] Μετά τη διαγραφή ένα σύμπλεγμα HBase, μπορείτε να δημιουργήσετε μια άλλη HBase σύμπλεγμα, χρησιμοποιώντας το ίδιο προεπιλεγμένου λογαριασμού χώρου αποθήκευσης και το προεπιλεγμένο κοντέινερ αντικειμένων blob. Νέο σύμπλεγμα θα σηκώστε το HBase πίνακες που δημιουργήσατε στο αρχικό σύμπλεγμα. Για να αποφύγετε ασυνέπειες, συνιστάται να απενεργοποιήσετε τους πίνακες HBase πριν να διαγράψετε το σύμπλεγμα.

## <a name="create-tables-and-insert-data"></a>Δημιουργία πινάκων και εισαγωγή δεδομένων

Προς το παρόν, υπάρχουν δύο τρόπο για να αποκτήσετε πρόσβαση HBase. Αυτή η ενότητα περιγράφει με το κέλυφος HBase. Στην επόμενη ενότητα καλύπτει χρησιμοποιώντας το .NET SDK.

Για τα περισσότερα άτομα, τα δεδομένα εμφανίζονται σε μορφή πίνακα:

![hdinsight hbase δεδομένα σε μορφή πίνακα][img-hbase-sample-data-tabular]

Στο HBase που είναι μια εφαρμογή της BigTable, των ίδιων δεδομένων έχει την παρακάτω:

![hdinsight hbase bigtable δεδομένων][img-hbase-sample-data-bigtable]

Θα μπορεί να είναι προτιμότερο, αφού ολοκληρώσετε την επόμενη διαδικασία.  

**Για να χρησιμοποιήσετε το κέλυφος HBase**

1. Χρησιμοποιήστε RDP για να συνδεθείτε με το σύμπλεγμα HBase στο HDInsight. Για τις RDP οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων σε με την πύλη Azure HDInsight][hdinsight-manage-portal].
2. Κατά την περίοδο λειτουργίας σας RDP, κάντε κλικ στη συντόμευση **γραμμής εντολών Hadoop** που βρίσκεται στην επιφάνεια εργασίας.
3. Ανοίξτε το κέλυφος HBase:

        cd %HBASE_HOME%\bin
        hbase shell

4. Δημιουργήστε μια HBase με δύο οικογένειες στήλης:

        create 'Contacts', 'Personal', 'Office'
        list
5. Εισαγωγή ορισμένων δεδομένων:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase κελύφους][img-hbase-shell]

6. Λήψη μία γραμμή

        get 'Contacts', '1000'

    Θα δείτε τα ίδια αποτελέσματα με τη χρήση της εντολής σάρωσης, επειδή υπάρχει μόνο μία γραμμή.

    Για περισσότερες πληροφορίες σχετικά με τη διάταξη πίνακα Hbase, ανατρέξτε στο άρθρο [Εισαγωγή στη Σχεδίαση σχήματος HBase][hbase-schema]. Για περισσότερες εντολές HBase, ανατρέξτε στο θέμα [Οδηγός αναφοράς Apache HBase][hbase-quick-start].


6. Έξοδος από το κέλυφος

        exit

**Για τη μαζική φόρτωση δεδομένων σε πίνακα HBase επαφών**

HBase περιλαμβάνει διάφορες μεθόδους γίνεται φόρτωση των δεδομένων σε πίνακες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μαζική τη φόρτωση](http://hbase.apache.org/book.html#arch.bulk.load).


Ένα δείγμα αρχείου δεδομένων έχει αποσταλεί σε ένα κοντέινερ δημόσια blob, wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Το περιεχόμενο του αρχείου δεδομένων είναι:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Μπορείτε να δημιουργήσετε ένα αρχείο κειμένου και να αποστείλετε το αρχείο στο δικό σας λογαριασμό χώρου αποθήκευσης, εάν θέλετε. Για τις οδηγίες, ανατρέξτε στο θέμα [Αποστολή δεδομένων για τις εργασίες Hadoop στο HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Αυτή η διαδικασία χρησιμοποιεί τον πίνακα HBase επαφών που δημιουργήσατε στην τελευταία διαδικασία.

1. Κατά την περίοδο λειτουργίας σας RDP, κάντε κλικ στη συντόμευση **γραμμής εντολών Hadoop** που βρίσκεται στην επιφάνεια εργασίας.
2. Αλλαγή καταλόγου:

        cd %HBASE_HOME%\bin

3. Εκτελέστε την παρακάτω εντολή για να μετατρέψετε το αρχείο δεδομένων του για να StoreFiles και αποθήκευσης σε μια σχετική διαδρομή που καθορίζεται από Dimporttsv.bulk.output:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Εκτελέστε την παρακάτω εντολή για να αποστείλετε τα δεδομένα από /example/data/storeDataFileOutput στον πίνακα HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Μπορείτε να ανοίξετε το κέλυφος HBase και χρησιμοποιήστε την εντολή σάρωσης για να εμφανίσετε το περιεχόμενο του πίνακα.



## <a name="use-hive-to-query-hbase-tables"></a>Χρήση της ομάδας για πίνακες HBase ερωτήματος

Μπορείτε να υποβάλετε ερώτημα δεδομένα αποθηκευμένα σε HBase με χρήση της ομάδας. Αυτή η ενότητα δημιουργεί έναν πίνακα Hive που αντιστοιχεί στον πίνακα HBase και χρησιμοποιεί το ερώτημα για τα δεδομένα στον πίνακά σας HBase.

**Για να ανοίξετε τον πίνακα εργαλείων συμπλέγματος**

1. Αναζήτηση για να **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Εισαγάγετε το Hadoop όνομα χρήστη και κωδικό πρόσβασης. **Διαχειριστής** είναι το προεπιλεγμένο όνομα χρήστη και ο κωδικός πρόσβασης που πληκτρολογήσατε κατά τη διαδικασία δημιουργίας. Ανοίγει μια νέα καρτέλα του προγράμματος περιήγησης.
6. Κάντε κλικ στην επιλογή **Επεξεργασία ομάδας** στο επάνω μέρος της σελίδας. Πρόγραμμα επεξεργασίας Hive μοιάζει κάπως έτσι:

    ![Πίνακας εργαλείων σύμπλεγμα HDInsight.][img-hdinsight-hbase-hive-editor]

**Για να εκτελέσετε Hive ερωτημάτων**

1. Εισαγάγετε την ακόλουθη δέσμη ενεργειών HiveQL στο Hive επεξεργασίας και κάντε κλικ στην επιλογή **Υποβολή** για να δημιουργήσετε έναν πίνακα Hive που αντιστοιχεί στον πίνακα HBase. Βεβαιωθείτε ότι έχετε δημιουργήσει το δείγμα πίνακα που αναφέρονται παραπάνω σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιώντας το κέλυφος HBase πριν να εκτελέσετε αυτήν τη δήλωση.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Περιμένετε έως ότου τις ενημερώσεις **κατάστασης** για να **ολοκληρωθεί**.

2. Εισαγάγετε την ακόλουθη δέσμη ενεργειών HiveQL στο Hive επεξεργασίας και, στη συνέχεια, κάντε κλικ στην επιλογή **Υποβολή**. Το ερώτημα Hive ερωτήματα τα δεδομένα στον πίνακα HBase:

        SELECT count(*) FROM hbasecontacts;

4. Για να ανακτήσετε τα αποτελέσματα του ερωτήματος ομάδας, κάντε κλικ στη σύνδεση **Προβολή λεπτομερειών** στο παράθυρο της **Περιόδου λειτουργίας εργασία** όταν ολοκληρωθεί η εκτέλεση του την εργασία. Θα υπάρχει μόνο ένα αρχείο εξόδου εργασία επειδή τοποθετείτε μία εγγραφή στον πίνακα HBase.




**Για να αναζητήσετε το αρχείο εξόδου**

1. Στην κονσόλα ερωτήματος, κάντε κλικ στην επιλογή **Αρχείο προγράμματος περιήγησης**.
2. Κάντε κλικ στην επιλογή ο λογαριασμός Azure χώρου αποθήκευσης που χρησιμοποιείται ως το προεπιλεγμένο σύστημα αρχείων για το σύμπλεγμα HBase.
3. Κάντε κλικ στο όνομα του συμπλέγματος HBase. Το προεπιλεγμένο αποθήκευσης Azure λογαριασμό κοντέινερ χρησιμοποιεί το όνομα του συμπλέγματος.
4. Κάντε κλικ στην επιλογή **χρήστη**και, στη συνέχεια, κάντε κλικ στην επιλογή **Διαχείριση**. (Αυτό είναι το όνομα χρήστη Hadoop.)
6. Επιλέξτε το όνομα της εργασίας με την ώρα **Τελευταίας τροποποίησης** που ταιριάζει με το χρόνο όταν εκτελέσατε το ερώτημα ΕΠΙΛΈΞΤΕ την ομάδα.
4. Κάντε κλικ στην επιλογή **stdout**. Αποθηκεύστε το αρχείο και ανοίξτε το αρχείο με το Σημειωματάριο. Θα υπάρξει ένα αρχείο εξόδου.

    ![HDInsight HBase ομάδα επεξεργασίας αρχείο προγράμματος περιήγησης][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Χρήση της βιβλιοθήκης του προγράμματος-πελάτη .NET HBase REST API

Πρέπει να κάνετε λήψη της βιβλιοθήκης του προγράμματος-πελάτη HBase REST API για .NET από GitHub και δημιουργήστε το έργο, ώστε να μπορείτε να χρησιμοποιήσετε το .NET SDK HBase. Η ακόλουθη διαδικασία περιλαμβάνει τις οδηγίες για αυτήν την εργασία.

1. Δημιουργία νέας εφαρμογής C# Visual Studio κονσόλα των Windows για υπολογιστή.
2. Ανοίξτε την Κονσόλα διαχείρισης πακέτου NuGet κάνοντας κλικ στην επιλογή **Εργαλεία** > **Διαχείρισης πακέτου NuGet** > **Κονσόλα διαχείρισης πακέτου**.
3. Εκτελέστε την ακόλουθη εντολή NuGet στην κονσόλα:

        Install-Package Microsoft.HBase.Client

5. Προσθέστε τις παρακάτω προτάσεις **Χρήση** στο επάνω μέρος του αρχείου:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Αντικατάσταση της συνάρτησης **κύριες** με τα εξής:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Ορίστε τις πρώτες τρεις μεταβλητές στη συνάρτηση **κύριες** .
8. Πατήστε το πλήκτρο **F5** για να εκτελέσετε την εφαρμογή.

## <a name="check-cluster-status"></a>Έλεγχος κατάστασης συμπλέγματος

HBase στο HDInsight διατίθεται με ένα περιβάλλον εργασίας Χρήστη Web για την παρακολούθηση συμπλεγμάτων. Χρήση του περιβάλλοντος εργασίας Χρήστη του Web, μπορείτε να ζητήσετε στατιστικά στοιχεία ή πληροφορίες σχετικά με τις περιοχές.

Για να ανοίξετε το περιβάλλον εργασίας Χρήστη Web, που πρέπει να RDP σε σύμπλεγμα, και, στη συνέχεια, κάντε κλικ στη συντόμευση HMaster πληροφορίες περιβάλλοντος εργασίας Χρήστη Web στην επιφάνεια εργασίας σας ή χρησιμοποιήστε την παρακάτω διεύθυνση URL στο πρόγραμμα περιήγησης web:

    http://zookeeper[0-2]:60010/master-status

Σε ένα σύμπλεγμα υψηλής διαθεσιμότητας, θα βρείτε μια σύνδεση για τον τρέχοντα ενεργό HBase κύρια κόμβο που φιλοξενεί το Web UI.

##<a name="delete-the-cluster"></a>Διαγραφή του συμπλέγματος
Για να αποφύγετε ασυνέπειες, συνιστάται να απενεργοποιήσετε τους πίνακες HBase πριν να διαγράψετε το σύμπλεγμα.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Τι ακολουθεί;
Σε αυτό HBase το πρόγραμμα εκμάθησης για το HDInsight, μάθατε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα HBase και πώς μπορείτε να δημιουργήσετε πίνακες και να προβάλετε τα δεδομένα σε αυτούς τους πίνακες από το κέλυφος HBase. Μπορείτε επίσης να μάθατε πώς να χρησιμοποιήσετε ένα ερώτημα Hive σε δεδομένα σε πίνακες HBase και πώς μπορείτε να χρησιμοποιήσετε τα HBase C# REST API για να δημιουργήσετε έναν πίνακα HBase και να ανακτήσετε δεδομένα από τον πίνακα.

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview].
HBase είναι Apache, Άνοιγμα-προέλευση, NoSQL βάση δεδομένων δημιουργήθηκε στο Hadoop που παρέχει τυχαία πρόσβαση και ισχυρό συνέπειας για μεγάλες ποσότητες δεδομένων μη δομημένα και semistructured.
- [Δημιουργία συμπλεγμάτων HBase Azure εικονικού δικτύου][hdinsight-hbase-provision-vnet].
Με ενσωμάτωση του εικονικού δικτύου, συμπλεγμάτων HBase μπορεί να αναπτυχθεί το ίδιο εικονικό δίκτυο με τις εφαρμογές σας, έτσι ώστε οι εφαρμογές μπορούν να επικοινωνούν με HBase απευθείας.
- [Ρύθμιση παραμέτρων HBase αναπαραγωγή σε HDInsight](hdinsight-hbase-geo-replication.md). Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους HBase αναπαραγωγής σε δύο Azure κέντρα δεδομένων.
- [Ανάλυση άποψη Twitter, με HBase σε HDInsight][hbase-twitter-sentiment].
Μάθετε πώς μπορείτε να κάνετε σε πραγματικό χρόνο [ανάλυση άποψη](http://en.wikipedia.org/wiki/Sentiment_analysis) μεγάλο δεδομένων με τη χρήση HBase σε ένα σύμπλεγμα Hadoop στο HDInsight.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png

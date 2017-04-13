<properties
    pageTitle="Πρόγραμμα εκμάθησης HBase: γρήγορα αποτελέσματα με το συμπλεγμάτων βάσει Linux HBase στο Hadoop | Microsoft Azure"
    description="Παρακολουθήστε αυτό το πρόγραμμα εκμάθησης HBase για να ξεκινήσετε να χρησιμοποιείτε Apache HBase με Hadoop στο HDInsight. Δημιουργία πινάκων από το κέλυφος HBase και τους ερωτήματος με χρήση της ομάδας."
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>Πρόγραμμα εκμάθησης HBase: γρήγορα αποτελέσματα με το Apache HBase με βάσει Linux Hadoop στο HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Μάθετε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα HBase στο HDInsight, να δημιουργήσετε πίνακες HBase και υποβολή ερωτημάτων σε πίνακες με τη χρήση της ομάδας. Για γενικές πληροφορίες HBase, ανατρέξτε στο θέμα [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview].

Οι πληροφορίες σε αυτό το έγγραφο είναι συγκεκριμένη για συμπλεγμάτων βάσει Linux HDInsight. Για πληροφορίες σχετικά με συμπλεγμάτων που βασίζεται στα Windows, χρησιμοποιήστε στον επιλογέα στηλοθετών στο επάνω μέρος της σελίδας για να αλλάξετε.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτό το πρόγραμμα εκμάθησης HBase, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Ασφαλούς Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Δημιουργία HBase συμπλέγματος

Η παρακάτω διαδικασία χρησιμοποιεί ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε ένα σύμπλεγμα βάσει Linux HBase έκδοση 3.4 και τα εξαρτώμενα προεπιλεγμένο αποθήκευσης Azure λογαριασμό. Για να κατανοήσετε τις παραμέτρους που χρησιμοποιούνται στο τη διαδικασία και άλλες μεθόδους δημιουργίας σύμπλεγμα, ανατρέξτε στο θέμα [Δημιουργία Linux βάσει Hadoop συμπλεγμάτων στο HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Κάντε κλικ στην παρακάτω εικόνα για να ανοίξετε το πρότυπο στην πύλη του Azure. Το πρότυπο βρίσκεται σε ένα κοντέινερ δημόσια blob. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Από την **Ανάπτυξη προσαρμοσμένης** blade, εισαγάγετε τα εξής:

    - **Συνδρομή**: Επιλέξτε τη συνδρομή Azure σας που θα χρησιμοποιηθεί για τη δημιουργία του συμπλέγματος.
    - **Ομάδα πόρων**: Δημιουργήστε μια νέα ομάδα διαχείρισης πόρων Azure ή χρησιμοποιήστε ένα υπάρχον.
    - **Θέση**: Καθορίστε τη θέση της ομάδας πόρων. 
    - **ClusterName**: Πληκτρολογήστε ένα όνομα για το σύμπλεγμα HBase που θα δημιουργήσετε.
    - **Σύμπλεγμα όνομα σύνδεσης και τον κωδικό πρόσβασης**: το προεπιλεγμένο όνομα σύνδεσης είναι **διαχειριστής**.
    - **SSH όνομα χρήστη και τον κωδικό πρόσβασης**: **sshuser**είναι το προεπιλεγμένο όνομα χρήστη.  Μπορείτε να τη μετονομάσετε.
     
    Οι υπόλοιπες παράμετροι είναι προαιρετικό.  
    
    Κάθε σύμπλεγμα έχει μια εξάρτηση λογαριασμού χώρου αποθήκευσης αντικειμένων Blob του Azure. Μετά τη διαγραφή ένα σύμπλεγμα, διατηρεί τα δεδομένα στο λογαριασμό χώρου αποθήκευσης. Το σύμπλεγμα προεπιλεγμένο όνομα λογαριασμού χώρου αποθήκευσης είναι το όνομα συμπλέγματος με "store" προσαρτημένο. Αυτό είναι κωδικοποιημένη στην ενότητα μεταβλητές προτύπου.
        
3. Επιλέξτε **να συμφωνούν με τους όρους και προϋποθέσεις που αναφέρονται παραπάνω**και, στη συνέχεια, κάντε κλικ στην επιλογή **αγορά**. Χρειάζονται περίπου 20 λεπτά για να δημιουργήσετε ένα σύμπλεγμα.


>[AZURE.NOTE] Μετά τη διαγραφή ένα σύμπλεγμα HBase, μπορείτε να δημιουργήσετε μια άλλη HBase σύμπλεγμα, χρησιμοποιώντας το ίδιο προεπιλεγμένο κοντέινερ αντικειμένων blob. Νέο σύμπλεγμα θα σηκώστε το HBase πίνακες που δημιουργήσατε στο αρχικό σύμπλεγμα. Για να αποφύγετε ασυνέπειες, συνιστάται να απενεργοποιήσετε τους πίνακες HBase πριν να διαγράψετε το σύμπλεγμα.

## <a name="create-tables-and-insert-data"></a>Δημιουργία πινάκων και εισαγωγή δεδομένων

Μπορείτε να χρησιμοποιήσετε SSH για να συνδεθείτε συμπλεγμάτων HBase και, στη συνέχεια, χρησιμοποιήστε κελύφους HBase για να δημιουργήσετε πίνακες HBase, εισαγάγετε δεδομένα και τα δεδομένα του ερωτήματος. Για πληροφορίες σχετικά με τη χρήση SSH, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix, ή OS X](hdinsight-hadoop-linux-use-ssh-unix.md) και [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Για τα περισσότερα άτομα, τα δεδομένα εμφανίζονται σε μορφή πίνακα:

![HDInsight HBase δεδομένα σε μορφή πίνακα][img-hbase-sample-data-tabular]

Στο HBase που είναι μια εφαρμογή της BigTable, των ίδιων δεδομένων έχει την παρακάτω:

![HDInsight HBase bigtable δεδομένων][img-hbase-sample-data-bigtable]

Θα μπορεί να είναι προτιμότερο, αφού ολοκληρώσετε την επόμενη διαδικασία.  


**Για να χρησιμοποιήσετε το κέλυφος HBase**

1. Από SSH, εκτελέστε την ακόλουθη εντολή:

        hbase shell

4. Δημιουργήστε μια HBase με οικογένειες δύο στηλών:

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

    Για περισσότερες πληροφορίες σχετικά με τη διάταξη πίνακα HBase, ανατρέξτε στο άρθρο [Εισαγωγή στη Σχεδίαση σχήματος HBase][hbase-schema]. Για περισσότερες εντολές HBase, ανατρέξτε στο θέμα [Οδηγός αναφοράς Apache HBase][hbase-quick-start].

6. Έξοδος από το κέλυφος

        exit



**Για τη μαζική φόρτωση δεδομένων σε πίνακα HBase επαφών**

HBase περιλαμβάνει διάφορες μεθόδους γίνεται φόρτωση των δεδομένων σε πίνακες.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μαζική τη φόρτωση](http://hbase.apache.org/book.html#arch.bulk.load).


Ένα δείγμα αρχείου δεδομένων έχει αποσταλεί σε ένα κοντέινερ δημόσια blob, *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Το περιεχόμενο του αρχείου δεδομένων είναι:

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

1. Από SSH, εκτελέστε την ακόλουθη εντολή για να μετατρέψετε το αρχείο δεδομένων του για να StoreFiles και αποθήκευσης σε μια σχετική διαδρομή που καθορίζεται από Dimporttsv.bulk.output:.  Εάν είστε στο κέλυφος HBase, χρησιμοποιήστε την εντολή Έξοδος για να εξέλθετε από.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Εκτελέστε την παρακάτω εντολή για να αποστείλετε τα δεδομένα από /example/data/storeDataFileOutput στον πίνακα HBase:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Μπορείτε να ανοίξετε το κέλυφος HBase και χρησιμοποιήστε την εντολή σάρωσης για να εμφανίσετε το περιεχόμενο του πίνακα.



## <a name="use-hive-to-query-hbase"></a>Χρήση ομάδας σε ερώτημα HBase

Μπορείτε να υποβάλετε ερώτημα δεδομένων σε πίνακες HBase με χρήση της ομάδας. Αυτή η ενότητα δημιουργεί έναν πίνακα Hive που αντιστοιχεί στον πίνακα HBase και χρησιμοποιεί το ερώτημα για τα δεδομένα στον πίνακά σας HBase.

1. Ανοίξτε το **PuTTY**και συνδεθείτε με το σύμπλεγμα.  Δείτε τις οδηγίες στην προηγούμενη διαδικασία.
2. Ανοίξτε το κέλυφος ομάδας.

       hive
3. Εκτελέστε την ακόλουθη δέσμη ενεργειών HiveQL για να δημιουργήσετε έναν πίνακα Hive που αντιστοιχεί στον πίνακα HBase. Βεβαιωθείτε ότι έχετε δημιουργήσει το δείγμα πίνακα που αναφέρονται παραπάνω σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιώντας το κέλυφος HBase πριν να εκτελέσετε αυτήν τη δήλωση.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Εκτελέστε την ακόλουθη δέσμη ενεργειών HiveQL ερώτημα για τα δεδομένα στον πίνακα HBase:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Χρήση HBase ΥΠΌΛΟΙΠΟ APIs με καμπύλη

> [AZURE.NOTE] Όταν χρησιμοποιείτε καμπύλη ή οποιοδήποτε άλλο ΥΠΌΛΟΙΠΟ επικοινωνίας με WebHCat, πρέπει να τον έλεγχο ταυτότητας των αιτήσεων, παρέχοντας το όνομα χρήστη και τον κωδικό πρόσβασης για τη Διαχείριση συμπλέγματος HDInsight. Μπορείτε, επίσης, πρέπει να χρησιμοποιήσετε το όνομα του συμπλέγματος ως μέρος της το ενιαίο αναγνωριστικό πόρου (URI) χρησιμοποιείται για την αποστολή των αιτήσεων στο διακομιστή.
>
> Για τις εντολές σε αυτήν την ενότητα, αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** με το χρήστη για τον έλεγχο ταυτότητας στο σύμπλεγμα και αντικαταστήστε **τον κωδικό ΠΡΌΣΒΑΣΗΣ** με τον κωδικό πρόσβασης για το λογαριασμό χρήστη. Αντικαταστήστε **CLUSTERNAME** με το όνομα του συμπλέγματος.
>
> Το REST API προστατεύεται μέσω [βασικό έλεγχο ταυτότητας](http://en.wikipedia.org/wiki/Basic_access_authentication). Θα πρέπει να κάνετε πάντα αιτήσεις με τη χρήση ασφαλούς HTTP (HTTPS) για να εξασφαλίσετε ότι τα διαπιστευτήριά σας αποστέλλονται με ασφάλεια στο διακομιστή.

1. Από τη γραμμή εντολών, χρησιμοποιήστε την ακόλουθη εντολή για να επιβεβαιώσετε ότι μπορείτε να συνδεθείτε με το σύμπλεγμά σας HDInsight:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Θα πρέπει να λαμβάνετε μια απάντηση παρόμοια με τα εξής:

        {"status":"ok","version":"v1"}

    Οι παράμετροι χρησιμοποιούνται σε αυτήν την εντολή είναι ως εξής:

    * **-u** - το όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείται για τον έλεγχο ταυτότητας της αίτησης.
    * **-G** - υποδεικνύει ότι αυτό είναι ένα αίτημα GET.

2. Χρησιμοποιήστε την ακόλουθη εντολή για μια λίστα των υπαρχόντων πινάκων HBase:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε έναν νέο πίνακα HBase με δύο οικογένειες στήλης:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Το σχήμα παρέχονται σε μορφή JSon.

4. Χρησιμοποιήστε την ακόλουθη εντολή για να εισαγάγετε ορισμένα δεδομένα:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Πρέπει να base64 κωδικοποιείτε τις τιμές που καθορίζονται στα το διακόπτη -d.  Στο το exmaple:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: προσωπικά στοιχεία: όνομα
    - Sm9obiBEb2xl: Dole John

    [False-γραμμής-κλειδί](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) σάς επιτρέπει να εισαγάγετε πολλές (μαζικής) τιμή.

5. Χρησιμοποιήστε την ακόλουθη εντολή για να λάβετε μια γραμμή:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Για περισσότερες πληροφορίες σχετικά με τα υπόλοιπα HBase, ανατρέξτε στο θέμα [Οδηγός αναφοράς HBase Apache](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Έλεγχος κατάστασης συμπλέγματος

HBase στο HDInsight διατίθεται με ένα περιβάλλον εργασίας Χρήστη Web για την παρακολούθηση συμπλεγμάτων. Χρήση του περιβάλλοντος εργασίας Χρήστη του Web, μπορείτε να ζητήσετε στατιστικά στοιχεία ή πληροφορίες σχετικά με τις περιοχές.

SSH μπορεί επίσης να χρησιμοποιηθεί για τη διοχέτευση τοπικές αιτήσεις, όπως αιτήσεων web, στο σύμπλεγμα HDInsight. Η αίτηση θα δρομολογούνται, στη συνέχεια, ο πόρος που ζητήθηκε σαν να είχατε δημιουργήθηκε στον κόμβο κεφαλής συμπλέγματος HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Για να δημιουργήσετε μια SSH διοχέτευση περιόδου λειτουργίας**

1. Άνοιγμα **PuTTY**.  
2. Εάν παρέχεται ένα κλειδί SSH όταν δημιουργήσατε το λογαριασμό χρήστη κατά τη διαδικασία δημιουργίας, πρέπει να εκτελέσετε το ακόλουθο βήμα για να επιλέξετε το ιδιωτικό κλειδί για να χρησιμοποιήσετε κατά τον έλεγχο ταυτότητας στο σύμπλεγμα:

    Στην **κατηγορία**, αναπτύξτε **σύνδεση**ανάπτυξη **SSH**και επιλέξτε **Auth**. Τέλος, κάντε κλικ στην επιλογή **Αναζήτηση** και επιλέξτε το αρχείο .ppk που περιέχει το ιδιωτικό κλειδί.

3. Στην **κατηγορία**, κάντε κλικ στην επιλογή **περιόδου λειτουργίας**.
4. Από τις βασικές επιλογές για την οθόνη σας PuTTY περίοδο λειτουργίας, πληκτρολογήστε τις παρακάτω τιμές:

    - **Όνομα κεντρικού υπολογιστή**: η διεύθυνση SSH του πεδίου HDInsight διακομιστή στο όνομα κεντρικού υπολογιστή (ή τη διεύθυνση IP). Η διεύθυνση SSH είναι το όνομα του συμπλέγματος, στη συνέχεια, **-ssh.azurehdinsight.net**. Για παράδειγμα, *mycluster ssh.azurehdinsight.net*.
    - **Θύρα**: 22. Τα ssh θύρα το πρωτεύον headnode είναι 22.  
5. Στην ενότητα " **κατηγορία** " στα αριστερά του παραθύρου διαλόγου, αναπτύξτε **τη σύνδεση**, αναπτύξτε **SSH**και, στη συνέχεια, κάντε κλικ στην επιλογή **σήραγγες**.
6. Δώστε τις ακόλουθες πληροφορίες σχετικά με τις επιλογές έλεγχος SSH θύρα προώθησης φόρμας:

    - **Θύρα προέλευσης** - στη θύρα του υπολογιστή-πελάτη που θέλετε να προωθήσετε. Για παράδειγμα, 9876.
    - **Δυναμική** - ενεργοποιεί τη δρομολόγηση δυναμικής διακομιστή μεσολάβησης SOCKS.
7. Κάντε κλικ στο κουμπί **Προσθήκη** για να προσθέσετε τις ρυθμίσεις.
8. Κάντε κλικ στην επιλογή **Άνοιγμα** στο κάτω μέρος του παραθύρου διαλόγου για να ανοίξετε μια σύνδεση SSH.
9. Όταν σας ζητηθεί, συνδεθείτε στο διακομιστή με τη χρήση λογαριασμού SSH. Θα δημιουργήσετε μια περίοδο λειτουργίας SSH και ενεργοποίηση της διοχέτευσης.

**Για να βρείτε το FQDN του το zookeepers χρησιμοποιώντας Ambari**

1. Αναζήτηση για να https://<ClusterName>.azurehdinsight.net/.
2. Εισαγάγετε τα διαπιστευτήρια λογαριασμού χρήστη σας σύμπλεγμα δύο φορές.
3. Από το αριστερό μενού, κάντε κλικ στην επιλογή **zookeeper**.
4. Κάντε κλικ σε μία από τις τρεις συνδέσεις **ZooKeeper διακομιστή** από τη λίστα σύνοψης.
5. Αντιγραφή **όνομα κεντρικού υπολογιστή**. Για παράδειγμα, zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Για να ρυθμίσετε ένα πρόγραμμα-πελάτη (Firefox) και ελέγξτε την κατάσταση συμπλέγματος**

1. Ανοίξτε το Firefox.
2. Κάντε κλικ στο κουμπί **Άνοιγμα μενού** .
3. Κάντε κλικ στο κουμπί **Επιλογές**.
4. Κάντε κλικ στην επιλογή **για προχωρημένους**, κάντε κλικ στην επιλογή **δικτύου**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρυθμίσεις**.
5. Επιλέξτε **Ρύθμιση παραμέτρων διακομιστή μεσολάβησης μη αυτόματη**.
6. Εισαγάγετε τις παρακάτω τιμές:

    - **Socks κεντρικού υπολογιστή**: τοπικού κεντρικού υπολογιστή
    - **Θύρα**: Χρησιμοποιήστε την ίδια θύρα που ρυθμίσατε στο το Putty SSH διοχέτευσης.  Για παράδειγμα, 9876.
    - **SOCKS v5**: (επιλεγμένο)
    - **Απομακρυσμένο DNS**: (επιλεγμένο)
7. Κάντε κλικ στο **κουμπί OK** για να αποθηκεύσετε τις αλλαγές.
8. Αναζήτηση για να http://&lt;το FQDN του ένα ZooKeeper >: υποδείγματος/60010-κατάστασης.

Σε ένα σύμπλεγμα υψηλής διαθεσιμότητας, θα βρείτε μια σύνδεση για τον τρέχοντα ενεργό HBase κύρια κόμβο που φιλοξενεί το Web UI.

##<a name="delete-the-cluster"></a>Διαγραφή του συμπλέγματος

Για να αποφύγετε ασυνέπειες, συνιστάται να απενεργοποιήσετε τους πίνακες HBase πριν να διαγράψετε το σύμπλεγμα.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό HBase το πρόγραμμα εκμάθησης για το HDInsight, μάθατε πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα HBase και πώς μπορείτε να δημιουργήσετε πίνακες και να προβάλετε τα δεδομένα σε αυτούς τους πίνακες από το κέλυφος HBase. Μάθατε επίσης πώς μπορείτε να χρησιμοποιήσετε ένα ερώτημα Hive σε δεδομένα σε πίνακες HBase και πώς μπορείτε να χρησιμοποιήσετε τα HBase C# REST API για να δημιουργήσετε έναν πίνακα HBase και να ανακτήσετε δεδομένα από τον πίνακα.

Για περισσότερες πληροφορίες, ανατρέξτε στα θέματα:

- [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview]: HBase είναι Apache, Άνοιγμα-προέλευση, NoSQL βάση δεδομένων δημιουργήθηκε στο Hadoop που παρέχει τυχαία πρόσβαση και ισχυρό συνέπειας για μεγάλες ποσότητες δεδομένων μη δομημένα και semistructured.


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
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png

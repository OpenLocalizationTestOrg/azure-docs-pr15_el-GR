<properties
    pageTitle="Εκτελέστε μια εργασία Hadoop χρησιμοποιώντας DocumentDB και HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσετε μια απλή εργασία Hive, γουρούνι και MapReduce με DocumentDB και Azure HDInsight."
    services="documentdb"
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"
    documentationCenter=""/>


<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="denlee"/>

#<a name="DocumentDB-HDInsight"></a>Εκτελέστε μια εργασία Hadoop χρησιμοποιώντας DocumentDB και HDInsight

Αυτό το πρόγραμμα εκμάθησης σας δείχνει πώς μπορείτε να εκτελέσετε [Apache Hive][apache-hive], [Γουρούνι Apache][apache-pig], και [Apache Hadoop] [ apache-hadoop] MapReduce έργα στο Azure HDInsight με του DocumentDB Hadoop σύνδεσης. Γραμμή σύνδεσης του DocumentDB Hadoop επιτρέπει DocumentDB για εκτέλεση ενεργειών ως προέλευση και δέκτη για ομάδα, γουρούνι και MapReduce εργασίες. Αυτό το πρόγραμμα εκμάθησης θα χρησιμοποιήσει DocumentDB ως δεδομένα προέλευσης και προορισμού για τις εργασίες Hadoop.

Αφού ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, θα έχετε τη δυνατότητα να απαντούν στα παρακάτω ερωτήματα:

- Πώς μπορώ να φορτώσετε δεδομένα από DocumentDB χρησιμοποιώντας μια ομάδα, γουρούνι ή MapReduce εργασία;
- Πώς μπορώ να αποθηκεύσετε δεδομένα στο DocumentDB χρησιμοποιώντας μια ομάδα, γουρούνι ή MapReduce εργασία;

Συνιστάται να γρήγορα αποτελέσματα, παρακολουθώντας βίντεο που ακολουθεί, όπου μπορούμε να εκτελέσουμε μέσω ομάδας εργασίας με χρήση του DocumentDB και HDInsight.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

Στη συνέχεια, επιστρέψτε σε αυτό το άρθρο, όπου θα λάβετε τις πλήρεις πληροφορίες σχετικά με πώς μπορείτε να εκτελέσετε εργασίες ανάλυσης με τα δεδομένα σας DocumentDB.

> [AZURE.TIP] Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε εκ των προτέρων εμπειρία με χρήση Apache Hadoop, ομάδα ή/και γουρούνι. Εάν είστε νέος χρήστης του Apache Hadoop, ομάδας και γουρούνι, συνιστάται να επισκέπτονται την [τεκμηρίωση Apache Hadoop][apache-hadoop-doc]. Αυτό το πρόγραμμα εκμάθησης επίσης προϋποθέτει ότι έχετε εκ των προτέρων εμπειρία με DocumentDB και έχετε λογαριασμό DocumentDB. Εάν είστε νέος χρήστης DocumentDB ή δεν έχετε ένα λογαριασμό DocumentDB, κάντε ανάληψη ελέγχου μας [Γρήγορα αποτελέσματα] [ getting-started] σελίδας.

Δεν έχετε χρόνο για να ολοκληρωθεί το πρόγραμμα εκμάθησης και απλώς θέλετε να λάβετε τα πλήρη δείγματα δεσμών ενεργειών του PowerShell για την ομάδα, γουρούνι και MapReduce; Δεν ένα πρόβλημα, να τις αποκτήσω [εδώ][documentdb-hdinsight-samples]. Η λήψη περιέχει επίσης τα αρχεία hql, γουρούνι και java για αυτά τα δείγματα.

## <a name="NewestVersion"></a>Νεότερη έκδοση

<table border='1'>
    <tr><th>Έκδοση Hadoop σύνδεσης</th>
        <td>1.2.0</td></tr>
    <tr><th>Uri δέσμης ενεργειών</th>
        <td>https://portalcontent.blob.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Ημερομηνία τροποποίησης</th>
        <td>04/26/2016</td></tr>
    <tr><th>Υποστηριζόμενες HDInsight εκδόσεις</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Αρχείο καταγραφής αλλαγών</th>
        <td>Ενημερωμένο DocumentDB Java SDK για 1.6.0</br>
            Υποστήριξη που προστέθηκε για διαμερίσματα συλλογές ως προέλευση και δέκτη</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Προαπαιτούμενα στοιχεία
Πριν να ακολουθήσετε τις οδηγίες σε αυτό το πρόγραμμα εκμάθησης, βεβαιωθείτε ότι έχετε τα εξής:

- Ένα λογαριασμό DocumentDB, μια βάση δεδομένων και μια συλλογή με έγγραφα μέσα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το DocumentDB][getting-started]. Εισαγάγετε το δείγμα δεδομένων στο λογαριασμό σας DocumentDB με το [εργαλείο εισαγωγής DocumentDB][documentdb-import-data].
- Μεταγωγή. Διαβάζει και εγγραφές από το HDInsight θα συνυπολογίζεται και τις μονάδες που του έχει εκχωρηθεί αίτηση για τις συλλογές. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Provisioned μετάδοσης, μονάδες αίτηση, και τις λειτουργίες της βάσης δεδομένων][documentdb-manage-throughput].
- Δυνατότητα για μια επιπλέον αποθηκευμένη διαδικασία μέσα σε κάθε εξόδου συλλογής. Οι αποθηκευμένες διαδικασίες που χρησιμοποιούνται για τη μεταφορά εγγράφων που προκύπτει. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [συλλογές και προμήθεια του φακέλου μετάδοσης][documentdb-manage-document-storage].
- Δυναμικότητα για τα έγγραφα που προκύπτουν από τις εργασίες ομάδας, γουρούνι ή MapReduce. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Διαχείριση DocumentDB χωρητικότητας και της απόδοσης][documentdb-manage-collections].
- [*Προαιρετικό*] Δυναμικότητα για μια επιπλέον συλλογή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αποθήκευση εγγράφων Provisioned και ευρετήριο επιβάρυνσης][documentdb-manage-document-storage].

> [AZURE.WARNING] Για να αποφύγετε τη δημιουργία μιας νέας συλλογής κατά τη διάρκεια οποιασδήποτε από τις εργασίες, μπορείτε να είτε εκτυπώσετε τα αποτελέσματα για να stdout, αποθηκεύστε το αποτέλεσμα του κοντέινερ WASB, ή καθορίστε μια ήδη υπάρχουσα συλλογή. Στην περίπτωση που καθορίζει μια υπάρχουσα συλλογή, τα νέα έγγραφα θα δημιουργηθεί μέσα στη συλλογή και ήδη υπάρχοντα έγγραφα θα επηρεαστεί, εάν υπάρχει μια διένεξη στο *αναγνωριστικών*. **Η γραμμή σύνδεσης θα αντικαταστήσει αυτόματα υπάρχοντα έγγραφα με το αναγνωριστικό διενέξεων**. Μπορείτε να απενεργοποιήσετε αυτήν τη δυνατότητα, ορίζοντας την επιλογή upsert σε false. Εάν upsert είναι false και μια διένεξη παρουσιάζεται, θα αποτύχει η εργασία Hadoop; Δημιουργία αναφορών σφάλμα διένεξης αναγνωριστικό.


## <a name="ProvisionHDInsight"></a>Βήμα 1: Δημιουργήστε ένα νέο σύμπλεγμα HDInsight
Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί δέσμης ενεργειών από την πύλη του Azure για να προσαρμόσετε το σύμπλεγμά σας HDInsight. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε την πύλη του Azure για να δημιουργήσετε το σύμπλεγμά σας HDInsight. Για οδηγίες σχετικά με τη χρήση των cmdlet του PowerShell ή το .NET SDK HDInsight, ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών] [ hdinsight-custom-provision] το άρθρο.

1. Είσοδος στην [πύλη του Azure][azure-portal].

2. Κάντε κλικ στο κουμπί **+ νέα** στο επάνω μέρος το αριστερό παράθυρο περιήγησης, αναζητήστε **HDInsight** στη γραμμή επάνω αναζήτησης το νέο blade.

3. **HDInsight** δημοσιεύεται από τη **Microsoft** θα εμφανίζεται στο επάνω μέρος των αποτελεσμάτων. Κάντε κλικ σε αυτό και, στη συνέχεια, κάντε κλικ στην επιλογή **Δημιουργία**.

4. Στο νέο σύμπλεγμα HDInsight Δημιουργήστε blade, πληκτρολογήστε το **Όνομα του συμπλέγματος** και επιλέξτε τη **συνδρομή** που θέλετε να παρέχετε αυτόν τον πόρο στην περιοχή.

    <table border='1'>
        <tr><td>Όνομα συμπλέγματος</td><td>Το όνομα του συμπλέγματος.<br/>
   Όνομα DNS, πρέπει να ξεκινήσετε και τελειώνει με χαρακτήρα αλφαριθμητικοί και ενδέχεται να περιέχουν παύλες.<br/>
   Το πεδίο πρέπει να είναι μια συμβολοσειρά που αποτελείται από 3 έως 63 χαρακτήρες.</td></tr>
        <tr><td>Όνομα της συνδρομής</td>
            <td>Εάν έχετε περισσότερες από μία συνδρομές Azure, επιλέξτε τη συνδρομή στην οποία θα φιλοξενήσει το σύμπλεγμά σας HDInsight. </td></tr>
    </table>

5. Κάντε κλικ στην **Επιλογή τύπου για το σύμπλεγμα** και ρυθμίστε τις ακόλουθες ιδιότητες για τις καθορισμένες τιμές.

    <table border='1'>
        <tr><td>Τύπος συμπλέγματος</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Επίπεδο συμπλέγματος</td><td><strong>Τυπική</strong></td></tr>
        <tr><td>Λειτουργικό σύστημα</td><td><strong>Windows</strong></td></tr>
        <tr><td>Έκδοση</td><td>πιο πρόσφατη έκδοση</td></tr>
    </table>

    Στη συνέχεια, κάντε κλικ στην **ΕΠΙΛΟΓΉ**.

    ![Παροχή Hadoop HDInsight λεπτομέρειες αρχικό συμπλέγματος][image-customprovision-page1]

6. Κάντε κλικ στο τα **διαπιστευτήρια** για να ορίσετε σύνδεσης και τα διαπιστευτήρια απομακρυσμένη πρόσβαση. Επιλέξτε το **όνομα χρήστη σύνδεσης σύμπλεγμα** και **κωδικό πρόσβασης στο σύμπλεγμα**.

    Εάν θέλετε να απομακρυσμένο σε το σύμπλεγμά σας, επιλέξτε *Ναι* στο κάτω μέρος του blade και δώστε ένα όνομα χρήστη και τον κωδικό πρόσβασης.

7. Κάντε κλικ στο **Αρχείο προέλευσης δεδομένων** για να ορίσετε την κύρια θέση για πρόσβαση σε δεδομένα. Επιλέξτε τη **Μέθοδο επιλογής** και καθορίστε έναν ήδη υπάρχοντα λογαριασμό χώρου αποθήκευσης ή δημιουργήστε ένα νέο.

8. Στην ίδια την blade, καθορίστε ένα **Προεπιλεγμένο κοντέινερ** και μια **θέση**. Και, κάντε κλικ στην **ΕΠΙΛΟΓΉ**.

    > [AZURE.NOTE] Επιλέξτε μια θέση σε περιοχή το λογαριασμό σας DocumentDB για καλύτερες επιδόσεις

8. Κάντε κλικ στο για να επιλέξετε τον αριθμό και τον τύπο της κόμβους **Τιμολόγηση** . Μπορείτε να διατηρήσετε την προεπιλεγμένη ρύθμιση παραμέτρων και να κλιμάκωση τον αριθμό των κόμβους εργαζόμενου αργότερα.

9. Κάντε κλικ στην επιλογή **προαιρετικών παραμέτρων**, στη συνέχεια, **Ενέργειες δέσμης ενεργειών** σε Blade την προαιρετική ρύθμιση παραμέτρων.

    Στις ενέργειες δέσμης ενεργειών, πληκτρολογήστε τις ακόλουθες πληροφορίες για να προσαρμόσετε το σύμπλεγμά σας HDInsight.

    <table border='1'>
        <tr><th>Ιδιότητα</th><th>Τιμή</th></tr>
        <tr><td>Όνομα</td>
            <td>Καθορίστε ένα όνομα για την ενέργεια δέσμης ενεργειών.</td></tr>
        <tr><td>Δέσμη ενεργειών URI</td>
            <td>Καθορίστε το URI στη δέσμη ενεργειών που καλείται για να προσαρμόσετε το σύμπλεγμα.</br></br>
   Πληκτρολογήστε: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</td></tr>
        <tr><td>Κεφαλής</td>
            <td>Επιλέξτε το πλαίσιο ελέγχου για να εκτελέσετε τη δέσμη ενεργειών PowerShell επάνω στον κόμβο προϊστάμενο.</br></br>
            <strong>Επιλέξτε αυτό το πλαίσιο ελέγχου</strong>.</td></tr>
        <tr><td>Εργαζόμενου</td>
            <td>Επιλέξτε το πλαίσιο ελέγχου για να εκτελέσετε τη δέσμη ενεργειών PowerShell επάνω στον κόμβο εργασίας.</br></br>
            <strong>Επιλέξτε αυτό το πλαίσιο ελέγχου</strong>.</td></tr>
        <tr><td>Zookeeper</td>
            <td>Επιλέξτε το πλαίσιο ελέγχου για να εκτελέσετε τη δέσμη ενεργειών PowerShell σε το Zookeeper.</br></br>
            <strong>Δεν είναι απαραίτητο</strong>.
            </td></tr>
        <tr><td>Παράμετροι</td>
            <td>Καθορίστε τις παραμέτρους, εάν απαιτείται από τη δέσμη ενεργειών.</br></br>
            <strong>Παράμετροι δεν είναι απαραίτητο</strong>.</td></tr>
    </table>

10. Δημιουργία είτε μια νέα **Ομάδα πόρων** ή να χρησιμοποιήσετε μια υπάρχουσα ομάδα πόρων κάτω από τη συνδρομή σας στο Azure.

11. Τώρα, επιλέξτε **Pin στον πίνακα εργαλείων** για να παρακολουθείτε την ανάπτυξη και κάντε κλικ στην επιλογή **Δημιουργία**!

## <a name="InstallCmdlets"></a>Βήμα 2: Εγκατάσταση και ρύθμιση παραμέτρων του PowerShell Azure

1. Εγκατάσταση του Azure PowerShell. Μπορείτε να βρείτε οδηγίες [εδώ][powershell-install-configure].

    > [AZURE.NOTE] Εναλλακτικά, απλώς για ομάδα ερωτήματα, μπορείτε να χρησιμοποιήσετε του HDInsight online Hive προγράμματος επεξεργασίας. Για να κάνετε αυτό, εισέλθετε στην [Πύλη του Azure][azure-portal], κάντε κλικ στην επιλογή **HDInsight** στο αριστερό τμήμα του παραθύρου για να προβάλετε μια λίστα με τις συμπλεγμάτων HDInsight. Κάντε κλικ στην επιλογή το σύμπλεγμα που θέλετε να εκτελέσετε ερωτήματα Hive και, στη συνέχεια, κάντε κλικ στην επιλογή **Κονσόλα ερωτήματος**.

2. Ανοίξτε το ενσωματωμένο περιβάλλον δέσμης ενεργειών Azure PowerShell:
    - Σε έναν υπολογιστή που εκτελεί Windows 8 ή Windows Server 2012 ή νεότερη έκδοση, μπορείτε να χρησιμοποιήσετε την ενσωματωμένη αναζήτηση. Από την οθόνη έναρξης, πληκτρολογήστε **powershell ise** και πιέστε το πλήκτρο **Enter**.
    - Σε έναν υπολογιστή που εκτελεί μια έκδοση παλαιότερη από το Windows 8 ή Windows Server 2012, χρησιμοποιήστε το μενού "Έναρξη". Από το μενού Έναρξη, πληκτρολογήστε **γραμμή εντολών** στο πλαίσιο αναζήτησης, στη συνέχεια, στη λίστα των αποτελεσμάτων, κάντε κλικ στην επιλογή **γραμμή εντολών**. Στη γραμμή εντολών, πληκτρολογήστε **powershell_ise** και πιέστε το πλήκτρο **Enter**.

3. Προσθήκη λογαριασμού Azure.
    1. Στο παράθυρο κονσόλας, πληκτρολογήστε **Πρόσθετο AzureAccount** και πιέστε το πλήκτρο **Enter**.
    2. Πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου που σχετίζονται με τη συνδρομή σας στο Azure και κάντε κλικ στην επιλογή **Continue**.
    3. Πληκτρολογήστε τον κωδικό πρόσβασης για τη συνδρομή σας στο Azure.
    4. Κάντε κλικ στην επιλογή **Είσοδος**.

4. Το παρακάτω διάγραμμα προσδιορίζει τα σημαντικά τμήματα του περιβάλλοντος δέσμες ενεργειών του Azure PowerShell.

    ![Διάγραμμα για το Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Βήμα 3: Εκτέλεση ομάδας εργασίας με χρήση του DocumentDB και HDInsight

> [AZURE.IMPORTANT] Όλες τις μεταβλητές που δηλώνεται με < > πρέπει να συμπληρωθεί με χρήση ρυθμίσεων παραμέτρων.

1. Ορίστε τις παρακάτω μεταβλητές στο παράθυρο δέσμη ενεργειών του PowerShell.

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. <p>Ας ξεκινήσουμε δημιουργώντας σας συμβολοσειρά ερωτήματος. Θα σας θα Γράψτε ένα ερώτημα Hive που μεταφέρει χρονικές σημάνσεις δημιουργείται από το σύστημα όλων των εγγράφων (_ts) και μοναδικών αναγνωριστικών (_rid) από μια συλλογή DocumentDB, καταμετρά όλα τα έγγραφα από το λεπτό και, στη συνέχεια, αποθηκεύει, επιστρέψετε τα αποτελέσματα σε μια νέα συλλογή DocumentDB.</p>

    <p>Πρώτα, ας δημιουργήσουμε έναν πίνακα ομάδα από τη συλλογή DocumentDB μας. Προσθέστε το παρακάτω τμήμα κώδικα για τη δέσμη ενεργειών του PowerShell παραθύρου <strong>μετά</strong> το τμήμα κώδικα από #1. Φροντίστε να συμπεριλάβετε την προαιρετική t παράμετρο DocumentDB.query που είναι trim μας εγγράφων για απλώς _ts και _rid.</p>

    > [AZURE.NOTE]**Ονομασία DocumentDB.inputCollections δεν ήταν λάθος.** Ναι, θα σας επιτρέπει την προσθήκη πολλές συλλογές ως εισαγωγή: </br>

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Στη συνέχεια, ας δημιουργήσουμε έναν πίνακα ομάδα για τη συλλογή εξόδου. Οι ιδιότητες εγγράφου εξόδου θα είναι ο μήνας, ημέρα, ώρα, λεπτό και ο συνολικός αριθμός των εμφανίσεων.

    > [AZURE.NOTE]**Ονομασία DocumentDB.outputCollections δεν έχει ακόμη ξανά, λάθος.** Ναι, θα σας επιτρέπει την προσθήκη πολλές συλλογές ως το αποτέλεσμα: </br>
'*DocumentDB.outputCollections*'='*\<DocumentDB εξόδου συλλογής όνομα 1\>*,*\<όνομα συλλογής εξόδου DocumentDB 2\>*' </br> Τα ονόματα συλλογής διαχωρίζονται χωρίς κενά διαστήματα, χρησιμοποιώντας μόνο ένα μεμονωμένο κόμμα. </br></br>
Έγγραφα θα κατανέμεται round-robin σε πολλές συλλογές. Μια δέσμη με τα έγγραφα που θα αποθηκευτούν σε μία συλλογή και, στη συνέχεια, μια δεύτερη δέσμη με τα έγγραφα που θα αποθηκευτούν στο επόμενο συλλογής, και ούτω καθεξής.

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                              "tblproperties ( " +
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Τέλος, ας Αντιστοιχίστε τα έγγραφα, μήνας, ημέρα, ώρα και λεπτά και να εισαγάγετε τα αποτελέσματα ξανά σε το αποτέλεσμα του πίνακα "Hive".

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Προσθέστε το παρακάτω τμήμα κώδικα δέσμης ενεργειών για να δημιουργήσετε έναν ορισμό εργασίας ομάδας από το προηγούμενο ερώτημα.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Μπορείτε επίσης να χρησιμοποιήσετε το αρχείο διακόπτη - για να καθορίσετε ένα αρχείο δέσμης ενεργειών HiveQL σε HDFS.

6. Προσθέστε το παρακάτω τμήμα κώδικα για την αποθήκευση της ώρας έναρξης και υποβολή της ομάδας εργασίας.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Προσθέστε τα εξής για να περιμένετε για την ομάδα να ολοκληρωθεί η εργασία.

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Προσθέστε τα εξής για να εκτυπώσετε την τυπική έξοδο και την ώρα έναρξης και λήξης.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Εκτελέστε** τη νέα δέσμη ενεργειών σας! **Κάντε κλικ** στο κουμπί Εκτέλεση πράσινο.

10. Ελέγξτε τα αποτελέσματα. Πραγματοποιήστε είσοδο στο του [Azure πύλη][azure-portal].
    1. Στο πλαίσιο αριστερό τμήμα, κάντε κλικ στο κουμπί <strong>Αναζήτηση</strong> . </br>
    2. Κάντε κλικ στην επιλογή <strong>όλα τα στοιχεία</strong> στην επάνω δεξιά πλευρά του πίνακα "Αναζήτηση". </br>
    3. Βρείτε και κάντε κλικ στην επιλογή <strong>Λογαριασμοί DocumentDB</strong>. </br>
    4. Στη συνέχεια, βρείτε το <strong>Λογαριασμό DocumentDB</strong>, στη συνέχεια, <strong>DocumentDB βάση δεδομένων</strong> και τη <strong>Συλλογή DocumentDB</strong> που σχετίζεται με τη συλλογή εξόδου που καθορίζεται στο ερώτημά σας ομάδα.</br>
    5. Τέλος, κάντε κλικ στην επιλογή <strong>Εξερεύνηση εγγράφων</strong> κάτω από τα <strong>Εργαλεία για προγραμματιστές</strong>.</br></p>

    Θα δείτε τα αποτελέσματα του ερωτήματός σας ομάδα.

    ![Ομάδα αποτελεσμάτων ερωτήματος][image-hive-query-results]

## <a name="RunPig"></a>Βήμα 4: Εκτέλεση μιας εργασίας γουρούνι με χρήση DocumentDB και HDInsight

> [AZURE.IMPORTANT] Όλες τις μεταβλητές που δηλώνεται με < > πρέπει να συμπληρωθεί με χρήση ρυθμίσεων παραμέτρων.

1. Ορίστε τις παρακάτω μεταβλητές, στο παράθυρο δέσμη ενεργειών του PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Ας ξεκινήσουμε δημιουργώντας σας συμβολοσειρά ερωτήματος. Θα σας θα Γράψτε ένα ερώτημα γουρούνι που μεταφέρει χρονικές σημάνσεις δημιουργείται από το σύστημα όλων των εγγράφων (_ts) και μοναδικών αναγνωριστικών (_rid) από μια συλλογή DocumentDB, καταμετρά όλα τα έγγραφα από το λεπτό και, στη συνέχεια, αποθηκεύει, επιστρέψετε τα αποτελέσματα σε μια νέα συλλογή DocumentDB.</p>
    <p>Πρώτα, φόρτωση έγγραφα από DocumentDB σε HDInsight. Προσθέστε το παρακάτω τμήμα κώδικα για τη δέσμη ενεργειών του PowerShell παραθύρου <strong>μετά</strong> το τμήμα κώδικα από #1. Βεβαιωθείτε ότι για να προσθέσετε ένα ερώτημα DocumentDB την προαιρετική παράμετρος ερωτήματος DocumentDB να αποκόψετε μας εγγράφων για απλώς _ts και _rid.</p>

    > [AZURE.NOTE]Ναι, θα σας επιτρέπει την προσθήκη πολλές συλλογές ως εισαγωγή: </br>
'*\<Συλλογής όνομα εισόδου DocumentDB 1\>*,*\<συλλογής όνομα εισόδου DocumentDB 2\>*'</br> Τα ονόματα συλλογής διαχωρίζονται χωρίς κενά διαστήματα, χρησιμοποιώντας μόνο ένα μεμονωμένο κόμμα. </b>

    Έγγραφα θα κατανέμεται round-robin σε πολλές συλλογές. Μια δέσμη με τα έγγραφα που θα αποθηκευτούν σε μία συλλογή και, στη συνέχεια, μια δεύτερη δέσμη με τα έγγραφα που θα αποθηκευτούν στο επόμενο συλλογής, και ούτω καθεξής.

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Στη συνέχεια, ας Αντιστοιχίστε τα έγγραφα από το μήνα, ημέρα, ώρα, λεπτό και ο συνολικός αριθμός των εμφανίσεων.

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. Τέλος, ας αποθήκευσης των αποτελεσμάτων σε μας νέα συλλογή εξόδου.

    > [AZURE.NOTE]Ναι, θα σας επιτρέπει την προσθήκη πολλές συλλογές ως το αποτέλεσμα: </br>
'\<Όνομα συλλογής DocumentDB εξόδου 1\>,\<όνομα συλλογής εξόδου DocumentDB 2\>'</br> Τα ονόματα συλλογής διαχωρίζονται χωρίς κενά διαστήματα, χρησιμοποιώντας μόνο ένα μεμονωμένο κόμμα.</br>
Έγγραφα θα κατανέμεται round-robin κατά μήκος του πολλές συλλογές. Μια δέσμη με τα έγγραφα που θα αποθηκευτούν σε μία συλλογή και, στη συνέχεια, μια δεύτερη δέσμη με τα έγγραφα που θα αποθηκευτούν στο επόμενο συλλογής, και ούτω καθεξής.

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Προσθέστε το παρακάτω τμήμα κώδικα δέσμης ενεργειών για να δημιουργήσετε έναν ορισμό εργασίας γουρούνι από το προηγούμενο ερώτημα.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Μπορείτε επίσης να χρησιμοποιήσετε το αρχείο διακόπτη - για να καθορίσετε ένα αρχείο δέσμης ενεργειών γουρούνι σε HDFS.

6. Προσθέστε το παρακάτω τμήμα κώδικα για να αποθηκεύσετε την ώρα έναρξης και υποβάλετε την εργασία γουρούνι.

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Προσθέστε τα εξής για να περιμένετε για την ολοκλήρωση της εργασίας γουρούνι.

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Προσθέστε τα εξής για να εκτυπώσετε την τυπική έξοδο και την ώρα έναρξης και λήξης.

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Εκτελέστε** τη νέα δέσμη ενεργειών σας! **Κάντε κλικ** στο κουμπί execute πράσινο.

10. Ελέγξτε τα αποτελέσματα. Πραγματοποιήστε είσοδο στο [Azure πύλη][azure-portal].
    1. Στο πλαίσιο αριστερό τμήμα, κάντε κλικ στο κουμπί <strong>Αναζήτηση</strong> . </br>
    2. Κάντε κλικ στην επιλογή <strong>όλα τα στοιχεία</strong> στην επάνω δεξιά πλευρά του πίνακα "Αναζήτηση". </br>
    3. Βρείτε και κάντε κλικ στην επιλογή <strong>Λογαριασμοί DocumentDB</strong>. </br>
    4. Στη συνέχεια, βρείτε το <strong>Λογαριασμό DocumentDB</strong>, στη συνέχεια, <strong>DocumentDB βάση δεδομένων</strong> και τη <strong>Συλλογή DocumentDB</strong> που σχετίζεται με τη συλλογή εξόδου που καθορίζεται στο ερώτημά σας γουρούνι.</br>
    5. Τέλος, κάντε κλικ στην επιλογή <strong>Εξερεύνηση εγγράφων</strong> κάτω από τα <strong>Εργαλεία για προγραμματιστές</strong>.</br></p>

    Θα δείτε τα αποτελέσματα του ερωτήματος γουρούνι.

    ![Γουρούνι αποτελέσματα ερωτήματος][image-pig-query-results]

## <a name="RunMapReduce"></a>Βήμα 5: Εκτέλεση μια εργασία MapReduce χρησιμοποιώντας DocumentDB και HDInsight

1. Ορίστε τις παρακάτω μεταβλητές στο παράθυρο δέσμη ενεργειών του PowerShell.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name

2. Θα σας θα εκτελέσει μια εργασία MapReduce που καταμετρά τον αριθμό των εμφανίσεων για κάθε ιδιότητα εγγράφου από τη συλλογή DocumentDB. Προσθέστε αυτό δέσμης ενεργειών τμήματος κώδικα **μετά** το τμήμα κώδικα παραπάνω.

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] TallyProperties v01.jar συνοδεύεται από την προσαρμοσμένη εγκατάσταση της γραμμής σύνδεσης Hadoop DocumentDB.

3. Προσθέστε την ακόλουθη εντολή για να υποβάλετε την εργασία MapReduce.

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Εκτός από τον ορισμό MapReduce εργασίας, μπορείτε επίσης να παρέχετε το όνομα συμπλέγματος HDInsight όπου θέλετε να εκτελέσετε την εργασία MapReduce, καθώς και τα διαπιστευτήρια. Η έναρξη-AzureHDInsightJob είναι μια ασύγχρονες κλήση. Για να ελέγξετε την ολοκλήρωση της εργασίας, χρησιμοποιήστε το cmdlet *AzureHDInsightJob αναμονή* .

4. Προσθέστε την ακόλουθη εντολή για να ελέγξετε σφάλματα με την εκτέλεση της εργασίας MapReduce.

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

5. **Εκτελέστε** τη νέα δέσμη ενεργειών σας! **Κάντε κλικ** στο κουμπί Εκτέλεση πράσινο.

6. Ελέγξτε τα αποτελέσματα. Πραγματοποιήστε είσοδο στο [Azure πύλη][azure-portal].
    1. Στο πλαίσιο αριστερό τμήμα, κάντε κλικ στο κουμπί <strong>Αναζήτηση</strong> .
    2. Κάντε κλικ στην επιλογή <strong>όλα τα στοιχεία</strong> στην επάνω δεξιά πλευρά του πίνακα "Αναζήτηση".
    3. Βρείτε και κάντε κλικ στην επιλογή <strong>Λογαριασμοί DocumentDB</strong>.
    4. Στη συνέχεια, βρείτε το <strong>Λογαριασμό DocumentDB</strong>, στη συνέχεια, <strong>DocumentDB βάση δεδομένων</strong> και τη <strong>Συλλογή DocumentDB</strong> που σχετίζεται με τη συλλογή εξόδου που καθορίζεται στην εργασία σας MapReduce.
    5. Τέλος, κάντε κλικ στην επιλογή <strong>Εξερεύνηση εγγράφων</strong> κάτω από τα <strong>Εργαλεία για προγραμματιστές</strong>.

    Θα δείτε τα αποτελέσματα της εργασίας σας MapReduce.

    ![Αποτελέσματα ερωτήματος MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Επόμενα βήματα

Συγχαρητήρια! Μόλις εκτελέσατε πρώτη ομάδα, γουρούνι και MapReduce των εργασιών σας με χρήση του Azure DocumentDB και HDInsight.

Έχουμε Άνοιγμα φορολόγησης που προέρχονται από τη γραμμή σύνδεσης Hadoop. Εάν σας ενδιαφέρει, μπορείτε να συνεισφέρετε στο [GitHub][documentdb-github].

Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

- [Ανάπτυξη μιας εφαρμογής Java με Documentdb][documentdb-java-application]
- [Ανάπτυξη προγραμμάτων Java MapReduce για Hadoop σε HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Γρήγορα αποτελέσματα με το Hadoop με ομάδα στο HDInsight για να αναλύσετε Χρήση ακουστικού κινητές συσκευές][hdinsight-get-started]
- [Χρήση MapReduce με HDInsight][hdinsight-use-mapreduce]
- [Χρήση της ομάδας με το HDInsight][hdinsight-use-hive]
- [Χρήση γουρούνι με HDInsight][hdinsight-use-pig]
- [Προσαρμογή συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md

<properties
    pageTitle="Χρήση απόχρωση με Hadoop σε HDInsight Linux συμπλεγμάτων | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εγκαταστήσετε και να χρησιμοποιήσετε απόχρωση με Hadoop συμπλεγμάτων σε HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Εγκατάσταση και χρήση απόχρωση στον HDInsight Hadoop συμπλεγμάτων

Μάθετε πώς μπορείτε να εγκαταστήσετε απόχρωση σε HDInsight Linux συμπλεγμάτων και να χρησιμοποιήσετε διοχέτευση για να δρομολογήσετε τις αιτήσεις απόχρωση.

## <a name="what-is-hue"></a>Τι είναι η απόχρωση;

Απόχρωση είναι ένα σύνολο των εφαρμογών Web που χρησιμοποιούνται για να αλληλεπιδράσετε με ένα σύμπλεγμα Hadoop. Μπορείτε να χρησιμοποιήσετε απόχρωση για αναζήτηση του χώρου αποθήκευσης που σχετίζεται με ένα σύμπλεγμα Hadoop (WASB, στην περίπτωση HDInsight συμπλεγμάτων), η εκτέλεση των εργασιών ομάδας και δεσμών ενεργειών γουρούνι, κ.λπ. Τα ακόλουθα στοιχεία είναι διαθέσιμες με τις εγκαταστάσεις απόχρωση σε ένα σύμπλεγμα HDInsight Hadoop.

* Πρόγραμμα επεξεργασίας Hive κερί
* Γουρούνι
* Διαχείριση Metastore
* Oozie
* FileBrowser (το οποίο ακρόασης κοντέινερ προεπιλεγμένη WASB)
* Εργασία περιήγησης

> [AZURE.WARNING] Στοιχεία που παρέχονται με το σύμπλεγμα HDInsight υποστηρίζονται πλήρως και υποστήριξη της Microsoft θα σας βοηθήσει να απομονώσετε και να επιλύσετε θέματα που σχετίζονται με αυτά τα στοιχεία.
>
> Προσαρμοσμένα στοιχεία λαμβάνουν εμπορικά λογικές υποστήριξη για να σας βοηθήσει να αντιμετωπίσετε περαιτέρω το ζήτημα. Αυτό μπορεί να έχει ως αποτέλεσμα το ζήτημα επίλυση ή που σας ζητά να συμμετάσχουν διαθέσιμα κανάλια για τις τεχνολογίες Άνοιγμα αρχείου προέλευσης όπου βρίσκονται πολλά επίπεδα εξειδίκευση για συγκεκριμένη τεχνολογία. Για παράδειγμα, υπάρχουν πολλές τοποθεσίες Κοινότητας που μπορούν να χρησιμοποιηθούν, όπως: [φόρουμ MSDN για το HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Επίσης, Apache έργα έχουν τοποθεσίες έργου στο [http://apache.org](http://apache.org), για παράδειγμα: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Εγκατάσταση απόχρωση χρήση δέσμης ενεργειών

Την παρακάτω ενέργεια δέσμη ενεργειών μπορεί να χρησιμοποιηθεί για την εγκατάσταση απόχρωση σε ένα σύμπλεγμα βάσει Linux HDInsight.
https://hdiconfigactions.blob.Core.Windows.NET/linuxhueconfigactionv02/Install-hue-uber-v02.Sh
    
Αυτή η ενότητα παρέχει οδηγίες σχετικά με το πώς μπορείτε να χρησιμοποιήσετε τη δέσμη ενεργειών κατά την προμήθεια του συμπλέγματος με την πύλη Azure. 

> [AZURE.NOTE] Azure PowerShell, την CLI Azure, το HDInsight .NET SDK ή πρότυπα διαχείρισης πόρων Azure επίσης μπορεί να χρησιμοποιηθεί για την εφαρμογή δέσμης ενεργειών. Μπορείτε επίσης να εφαρμόσετε ενέργειες δέσμης ενεργειών για να εκτελείται ήδη συμπλεγμάτων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων ενέργειες δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md).

1. Έναρξη προμήθειας ένα σύμπλεγμα, χρησιμοποιώντας τα βήματα στην [παροχή HDInsight συμπλεγμάτων σε Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), αλλά δεν ολοκληρώθηκε προμήθειας.

    > [AZURE.NOTE] Για να εγκαταστήσετε απόχρωση συμπλεγμάτων HDInsight, το μέγεθος προτεινόμενα headnode είναι τουλάχιστον A4 (8 πυρήνων, 14 GB μνήμης).

2. Στην blade την **Προαιρετική ρύθμιση παραμέτρων** , επιλέξτε **Ενέργειες δέσμης ενεργειών**και δώστε τις πληροφορίες, όπως φαίνεται παρακάτω:

    ![Παροχή παράμετροι ενέργεια δέσμης ενεργειών για απόχρωση] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Παροχή παράμετροι ενέργεια δέσμης ενεργειών για απόχρωση")

    * __ΌΝΟΜΑ__: Πληκτρολογήστε ένα φιλικό όνομα για την ενέργεια δέσμης ενεργειών.
    * __Δέσμη ΕΝΕΡΓΕΙΏΝ URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __ΠΡΟΪΣΤΆΜΕΝΟΣ__: Ενεργοποιήστε αυτή την επιλογή
    * __ΕΡΓΑΖΌΜΕΝΟΥ__: Αφήστε κενό.
    * __ZOOKEEPER__: Αφήστε κενό.
    * __ΠΑΡΆΜΕΤΡΟΙ__: Αφήστε κενό.

3. Στο κάτω από τις **Ενέργειες δέσμης ενεργειών**, χρησιμοποιήστε το κουμπί **επιλογή** για να αποθηκεύσετε τη ρύθμιση παραμέτρων. Τέλος, χρησιμοποιήστε το κουμπί **επιλογή** στο κάτω μέρος του blade **Προαιρετική ρύθμιση παραμέτρων** για να αποθηκεύσετε τις πληροφορίες προαιρετικών παραμέτρων.

4. Συνεχίστε την προμήθεια του συμπλέγματος, όπως περιγράφεται στην [παροχή HDInsight συμπλεγμάτων σε Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Χρήση απόχρωση με HDInsight συμπλεγμάτων

SSH Tunneling είναι ο μόνος τρόπος για να αποκτήσετε πρόσβαση απόχρωση στο σύμπλεγμα όταν εκτελείται. Διοχέτευση μέσω SSH επιτρέπει την κυκλοφορία για να μεταβείτε απευθείας το headnode του συμπλέγματος όπου εκτελείται απόχρωση. Αφού ολοκληρωθεί η προμήθεια του συμπλέγματος, χρησιμοποιήστε τα ακόλουθα βήματα για να χρησιμοποιήσετε απόχρωση σε ένα σύμπλεγμα HDInsight Linux.

1. Χρησιμοποιήστε τις πληροφορίες στο θέμα [Χρήση SSH διοχέτευσης για να αποκτήσετε πρόσβαση Ambari web περιβάλλοντος εργασίας Χρήστη, ResourceManager, JobHistory, NameNode, Oozie, και άλλη τοποθεσία web του περιβάλλοντος εργασίας Χρήστη](hdinsight-linux-ambari-ssh-tunnel.md) για να δημιουργήσετε μια διοχέτευση SSH από το σύστημα του υπολογιστή-πελάτη στο σύμπλεγμα HDInsight και, στη συνέχεια, να ρυθμίσετε το πρόγραμμα περιήγησης Web για να χρησιμοποιήσετε τη διοχέτευση SSH ως ένα διακομιστή μεσολάβησης.

2. Αφού έχετε δημιουργήσει μια διοχέτευση SSH και έχει ρυθμιστεί να διακομιστή μεσολάβησης κυκλοφορία μέσα από το πρόγραμμα περιήγησης, πρέπει να βρείτε το όνομα κεντρικού υπολογιστή του πρωτεύοντος κεφαλής κόμβου. Μπορείτε να το κάνετε με τη σύνδεση στο σύμπλεγμα χρησιμοποιώντας SSH στη θύρα 22. Για παράδειγμα, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` όπου __ΌΝΟΜΑ_ΧΡΉΣΤΗ__ είναι το όνομα χρήστη σας SSH και __CLUSTERNAME__ είναι το όνομα του συμπλέγματος.

    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH, δείτε τα ακόλουθα έγγραφα:

    * [Χρήση SSH με βάσει Linux HDInsight από ένα πρόγραμμα-πελάτη Linux, Unix ή το Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Χρήση SSH με βάσει Linux HDInsight από ένα πρόγραμμα-πελάτη των Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Μόλις συνδεθεί, χρησιμοποιήστε την παρακάτω εντολή για να αποκτήσετε το πλήρως προσδιορισμένο όνομα τομέα από το κύριο headnode:

        hostname -f

    Αυτό θα επιστρέψει ένα όνομα παρόμοιο με το εξής:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Αυτό είναι το όνομα κεντρικού υπολογιστή από το κύριο headnode όπου βρίσκεται η τοποθεσία Web απόχρωση.

2. Χρησιμοποιήστε το πρόγραμμα περιήγησης για να ανοίξετε την πύλη απόχρωση στο http://HOSTNAME:8888. Αντικαταστήστε το όνομα κεντρικού ΥΠΟΛΟΓΙΣΤΉ με το όνομα που έχετε λάβει στο προηγούμενο βήμα.

    > [AZURE.NOTE] Όταν συνδέεστε στο για πρώτη φορά, θα σας ζητηθεί να δημιουργήσετε ένα λογαριασμό για να συνδεθείτε στην πύλη του απόχρωση. Τα διαπιστευτήρια που καθορίζετε εδώ θα περιοριστούν στην πύλη και δεν σχετίζονται με το διαχειριστή ή SSH διαπιστευτήρια χρήστη που καθορίσατε κατά την προμήθεια του συμπλέγματος.

    ![Συνδεθείτε στην πύλη του απόχρωση] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Ορισμός διαπιστευτηρίων για την πύλη απόχρωση")

### <a name="run-a-hive-query"></a>Εκτέλεση ενός ερωτήματος ομάδας

1. Από την πύλη απόχρωση, κάντε κλικ στην επιλογή **Επεξεργασία ερωτήματος**και, στη συνέχεια, κάντε κλικ στην επιλογή " **Hive** " για να ανοίξετε το πρόγραμμα επεξεργασίας της ομάδας.

    ![Χρήση της ομάδας] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Χρήση της ομάδας")

2. Στην καρτέλα **παροχή βοήθειας** , στην περιοχή **βάσης δεδομένων**, θα πρέπει να βλέπετε **hivesampletable**. Αυτό είναι ένα δείγμα πίνακα που συνοδεύει όλες συμπλεγμάτων Hadoop σε HDInsight. Εισαγάγετε ένα δείγμα ερωτήματος στο δεξιό παράθυρο και να δείτε το αποτέλεσμα της καρτέλας **αποτελέσματα** στο παράθυρο παρακάτω, όπως φαίνεται στην την εικόνα της οθόνης.

    ![Εκτέλεση Hive ερωτήματος] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Εκτέλεση Hive ερωτήματος")

    Μπορείτε επίσης να χρησιμοποιήσετε την καρτέλα " **γράφημα** " για να δείτε μια οπτική αναπαράσταση του αποτελέσματος.

### <a name="browse-the-cluster-storage"></a>Αναζήτηση του χώρου αποθήκευσης συμπλέγματος

1. Από την πύλη απόχρωση, κάντε κλικ στην επιλογή **Αρχείο προγράμματος περιήγησης** στην επάνω δεξιά γωνία της γραμμής μενού.

2. Από προεπιλογή, το πρόγραμμα περιήγησης αρχείο ανοίγει στον κατάλογο **/user/myuser** . Κάντε κλικ στην επιλογή η κάθετος προς τα δεξιά πριν από τον κατάλογο χρηστών στη διαδρομή για να μεταβείτε στη ρίζα του κοντέινερ Azure χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.

    ![Χρήση αρχείου περιήγησης] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Χρήση αρχείου περιήγησης")

3. Κάντε δεξί κλικ σε ένα αρχείο ή φάκελο για να δείτε τις διαθέσιμες λειτουργίες. Χρησιμοποιήστε το κουμπί **Αποστολή** στη δεξιά γωνία για να αποστείλετε αρχεία του τρέχοντος καταλόγου. Χρησιμοποιήστε το κουμπί **Δημιουργία** για να δημιουργήσετε νέα αρχεία ή καταλόγους.

> [AZURE.NOTE] Το πρόγραμμα περιήγησης αρχείο απόχρωση να εμφανίσετε μόνο τα περιεχόμενα του κοντέινερ προεπιλεγμένη που σχετίζεται με το σύμπλεγμα HDInsight. Οποιαδήποτε επιπλέον λογαριασμούς/χώρους αποθήκευσης που μπορεί να έχετε που σχετίζονται με το σύμπλεγμα δεν θα είναι προσβάσιμα μέσω του προγράμματος περιήγησης του αρχείου. Ωστόσο, τα πρόσθετα κοντέινερ που σχετίζεται με το σύμπλεγμα θα είναι πάντα δυνατή η πρόσβαση για τις εργασίες ομάδας. Για παράδειγμα, εάν πληκτρολογήσετε την εντολή `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` στο πρόγραμμα επεξεργασίας της ομάδας, μπορείτε να δείτε τα περιεχόμενα του καθώς και πρόσθετες κοντέινερ. Σε αυτήν την εντολή, **newcontainer** δεν είναι το προεπιλεγμένο κοντέινερ που σχετίζεται με ένα σύμπλεγμα.

## <a name="important-considerations"></a>Σημαντικά θέματα

1. Η δέσμη ενεργειών που χρησιμοποιείται για την εγκατάσταση απόχρωση εγκαθιστούν μόνο από το κύριο headnode του συμπλέγματος.

2. Κατά την εγκατάσταση, πολλές υπηρεσίες Hadoop (HDFS, ΝΉΜΑΤΑ, MR2, Oozie) είναι επανεκκίνηση για την ενημέρωση των ρυθμίσεων παραμέτρων. Μετά την ολοκλήρωση της εγκατάστασης απόχρωση τη δέσμη ενεργειών, ενδέχεται να χρειαστεί κάποιος χρόνος για άλλες υπηρεσίες Hadoop για να ξεκινήσετε προς τα επάνω. Αυτό μπορεί να επηρεάσει επιδόσεων του απόχρωση αρχικά. Αφού ξεκινήσετε όλες τις υπηρεσίες, θα είναι πλήρως λειτουργικό απόχρωση.

3.  Απόχρωση δεν κατανοούν Tez εργασίες, που είναι η τρέχουσα προεπιλογή για την ομάδα. Εάν θέλετε να χρησιμοποιήσετε MapReduce ως το μηχανισμό εκτέλεσης της ομάδας, ενημερώστε τη δέσμη ενεργειών για να χρησιμοποιήσετε την παρακάτω εντολή στη δέσμη ενεργειών:

        set hive.execution.engine=mr;

4.  Με Linux συμπλεγμάτων, μπορείτε να έχετε ένα σενάριο όπου των υπηρεσιών σας εκτελούνται σε το πρωτεύον headnode, ενώ η διαχείριση πόρων μπορεί λειτουργεί με τη δευτερεύουσα. Ένα τέτοιο σενάριο ενδέχεται να οδηγήσει σε σφάλματα (απεικονίζεται παρακάτω) κατά τη χρήση απόχρωση για να προβάλετε λεπτομέρειες ΕΚΤΈΛΕΣΗ εργασιών στο σύμπλεγμα. Ωστόσο, μπορείτε να προβάλετε τις λεπτομέρειες της εργασίας, όταν ολοκληρωθεί η εργασία.

    ![Απόχρωση πύλης σφάλματος] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Απόχρωση πύλης σφάλματος")

    Αυτό οφείλεται ένα γνωστό θέμα. Ως λύση, τροποποιήστε Ambari έτσι ώστε να εκτελείται επίσης η ενεργή διαχείριση πόρων σε το πρωτεύον headnode.

5.  Απόχρωση κατανοούν WebHDFS ενώ HDInsight συμπλεγμάτων χρησιμοποιήσετε το χώρο αποθήκευσης Azure χρησιμοποιώντας `wasbs://`. Επομένως, η προσαρμοσμένη δέσμη ενεργειών που χρησιμοποιείται με την ενέργεια δέσμης ενεργειών εγκαθιστά WebWasb, το οποίο είναι συμβατό με WebHDFS υπηρεσίας για τη συνομιλία με WASB. Έτσι, ακόμα και αν την πύλη απόχρωση αναφέρει HDFS σε θέσεις (όπως όταν μετακινείτε το δείκτη του ποντικιού επάνω από το **Πρόγραμμα περιήγησης στο αρχείο**), αυτό θα πρέπει να θεωρηθούν ότι WASB.


## <a name="next-steps"></a>Επόμενα βήματα

- [Εγκατάσταση Giraph στην συμπλεγμάτων HDInsight](hdinsight-hadoop-giraph-install-linux.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε Giraph σε συμπλεγμάτων HDInsight Hadoop. Giraph σάς επιτρέπει να εκτελούν επεξεργασία γραφήματος χρησιμοποιώντας Hadoop, και μπορεί να χρησιμοποιηθεί με το Azure HDInsight.

- [Εγκατάσταση Solr στην συμπλεγμάτων HDInsight](hdinsight-hadoop-solr-install-linux.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε Solr σε συμπλεγμάτων HDInsight Hadoop. Solr σάς επιτρέπει να εκτελέσετε λειτουργίες ισχυρή αναζήτησης σε δεδομένα που έχουν αποθηκευτεί.

- [Εγκατάσταση R σε συμπλεγμάτων HDInsight](hdinsight-hadoop-r-scripts-linux.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε R σε συμπλεγμάτων HDInsight Hadoop. R είναι μια γλώσσα προέλευσης άνοιγμα και περιβάλλον για τον υπολογισμό στατιστικών. Παρέχει εκατοντάδες ενσωματωμένη στατιστικές συναρτήσεις και δικό του γλώσσα προγραμματισμού που συνδυάζει πτυχές προγραμματισμού λειτουργική και προσανατολισμένα σε αντικείμενα. Επίσης, παρέχει εκτεταμένη δυνατότητες γραφικών.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

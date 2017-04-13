<properties
    pageTitle="Εγκατάσταση R σε βάσει Linux HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εγκαταστήσετε και να χρησιμοποιήσετε R για να προσαρμόσετε συμπλεγμάτων Hadoop βάσει Linux."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Εγκατάσταση και χρήση R στον HDInsight Hadoop συμπλεγμάτων

Μπορείτε να εγκαταστήσετε R σε οποιονδήποτε τύπο σύμπλεγμα στο Hadoop σε HDInsight με χρήση **Δέσμης ενεργειών** σύμπλεγμα προσαρμογής. Αυτή η δυνατότητα επιτρέπει επιστημόνων δεδομένα και οι αναλυτές για να χρησιμοποιήσετε R για να αναπτύξετε ισχυρή πλαίσιο προγραμματισμού MapReduce/ΝΉΜΑΤΑ για την επεξεργασία μεγάλες ποσότητες δεδομένων σε συμπλεγμάτων Hadoop που έχουν αναπτυχθεί σε HDInsight.

> [AZURE.IMPORTANT] Η [σειρά premium](https://azure.microsoft.com/pricing/details/hdinsight/) σας δίνει τη δυνατότητα για το HDInsight περιλαμβάνει R διακομιστή ως μέρος του συμπλέγματος HDInsight. Αυτό σας επιτρέπει δέσμες ενεργειών R για να χρησιμοποιήσετε MapReduce και τους για να εκτελέσετε υπολογισμούς κατανέμεται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το διακομιστή R στο HDInsight](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Τι είναι το R;

Το <a href="http://www.r-project.org/" target="_blank">Project R για τον υπολογισμό στατιστικών</a> είναι ένα ανοιχτό γλώσσα προέλευσης και περιβάλλον για τον υπολογισμό στατιστικών. R παρέχει εκατοντάδες ενσωματωμένης στατιστικές συναρτήσεις και δικό του γλώσσα προγραμματισμού που συνδυάζει πτυχές προγραμματισμού λειτουργική και προσανατολισμένα σε αντικείμενα. Επίσης, παρέχει εκτεταμένη δυνατότητες γραφικών. R είναι το περιβάλλον προτιμώμενη προγραμματισμού για πιο επαγγελματική στατιστικών και επιστήμονες σε μια μεγάλη ποικιλία πεδία.

R δέσμες ενεργειών μπορούν να εκτελεστούν σε συμπλεγμάτων Hadoop στο HDInsight που έχουν προσαρμοστεί με χρήση δέσμης ενεργειών κατά τη δημιουργία για να εγκαταστήσετε το περιβάλλον R. R είναι συμβατά με το χώρο αποθήκευσης αντικειμένων Blob Azure (WASB), έτσι ώστε τα δεδομένα που είναι αποθηκευμένα είναι δυνατή η επεξεργασία χρήση R σε HDInsight.

## <a name="what-the-script-does"></a>Τι κάνει η δέσμη ενεργειών

Η ενέργεια δέσμης ενεργειών που χρησιμοποιείται για την εγκατάσταση του R σε το σύμπλεγμά σας HDInsight εγκαθιστά τα ακόλουθα πακέτα Ubuntu, που παρέχουν μια βασική εγκατάσταση R:

* [r-βάσης](http://packages.ubuntu.com/precise/r-base): πακέτο βάσης GNU R
* [r-βάσης-αποκλίσεις](http://packages.ubuntu.com/precise/r-base-dev): πακέτων Βοηθητική είσοδος GNU R

Τα ακόλουθα πακέτα RHadoop εγκαθίστανται, που παρέχουν ενοποίηση με MapReduce και HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): επιτρέπει στους προγραμματιστές R για να χρησιμοποιήσετε Hadoop MapReduce
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): επιτρέπει στους προγραμματιστές R για να χρησιμοποιήσετε HDFS Hadoop (WASB για HDInsight)

Επιπλέον, τα ακόλουθα πακέτα R είναι εγκατεστημένο:

| Πακέτο R | Τι παρέχει |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Χαμηλό επίπεδο R στο περιβάλλον εργασίας Java. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | Ενοποίηση R και C++. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | Σειριοποίηση/αποσειριοποίηση R αντικειμένων JSON |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Συναρτήσεις για πράξης λειτουργίες σε διανύσματα ακέραιο αριθμό. |
| [Σύνοψη](https://cran.r-project.org/web/packages/digest/index.html) | Δημιουργία συνόψεις κρυπτογράφησης κατακερματισμός R αντικειμένων. |
| [λειτουργική](https://cran.r-project.org/web/packages/functional/index.html) | Curry σύνθεση και άλλες συναρτήσεις υψηλότερη σειρά |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Ευελιξία αναδιάρθρωση και συγκέντρωση δεδομένων. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Απλή, ομοιόμορφη προγράμματα εξομοίωσης για κοινές λειτουργίες συμβολοσειράς. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Εργαλεία για διαίρεση, την εφαρμογή και συνδυασμό των δεδομένων. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Εργαλεία για τη μετακίνηση παραθύρου στατιστικά στοιχεία, GIF, Base64, ROC AUC, κ.λπ. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Κατά προσέγγιση συμβολοσειρά που ταιριάζουν και απόσταση συναρτήσεις συμβολοσειράς. |

## <a name="install-r-using-script-actions"></a>Εγκατάσταση R χρήση δέσμης ενεργειών

Την παρακάτω ενέργεια δέσμη ενεργειών χρησιμοποιείται για να εγκαταστήσετε το R σε ένα σύμπλεγμα HDInsight. https://hdiconfigactions.blob.Core.Windows.NET/linuxrconfigactionv01/r-Installer-v01.Sh
    
Αυτή η ενότητα παρέχει οδηγίες σχετικά με το πώς μπορείτε να χρησιμοποιήσετε τη δέσμη ενεργειών όταν δημιουργείτε ένα νέο σύμπλεγμα με την πύλη Azure. 

> [AZURE.NOTE] Azure PowerShell, την CLI Azure, το HDInsight .NET SDK ή πρότυπα διαχείρισης πόρων Azure επίσης μπορεί να χρησιμοποιηθεί για την εφαρμογή δέσμης ενεργειών. Μπορείτε επίσης να εφαρμόσετε ενέργειες δέσμης ενεργειών για να εκτελείται ήδη συμπλεγμάτων. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων ενέργειες δέσμης ενεργειών](hdinsight-hadoop-customize-cluster-linux.md).

1. Έναρξη προμήθειας ένα σύμπλεγμα, χρησιμοποιώντας τα βήματα που περιγράφονται σε [συμπλεγμάτων βάσει Linux παροχή HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal), αλλά δεν ολοκληρώθηκε προμήθειας.

2. Στην blade την **Προαιρετική ρύθμιση παραμέτρων** , επιλέξτε **Ενέργειες δέσμης ενεργειών**και δώστε τις παρακάτω πληροφορίες:

    * __ΌΝΟΜΑ__: Πληκτρολογήστε ένα φιλικό όνομα για την ενέργεια δέσμης ενεργειών.
    * __Δέσμη ΕΝΕΡΓΕΙΏΝ URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __ΠΡΟΪΣΤΆΜΕΝΟΣ__: Ενεργοποιήστε αυτή την επιλογή
    * __ΕΡΓΑΖΌΜΕΝΟΥ__: Ενεργοποιήστε αυτή την επιλογή
    * __ZOOKEEPER__: Χρησιμοποιήστε αυτήν την επιλογή για να εγκαταστήσετε στον κόμβο Zookeeper.
    * __ΠΑΡΆΜΕΤΡΟΙ__: Αφήστε κενό αυτό το πεδίο

3. Στο κάτω από τις **Ενέργειες δέσμης ενεργειών**, χρησιμοποιήστε το κουμπί **επιλογή** για να αποθηκεύσετε τη ρύθμιση παραμέτρων. Τέλος, χρησιμοποιήστε το κουμπί **επιλογή** στο κάτω μέρος του blade **Προαιρετική ρύθμιση παραμέτρων** για να αποθηκεύσετε τις πληροφορίες προαιρετικών παραμέτρων.

4. Συνεχίστε την προμήθεια του συμπλέγματος, όπως περιγράφεται στο [συμπλεγμάτων βάσει Linux παροχή HDInsight](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="run-r-scripts"></a>Εκτέλεση δεσμών ενεργειών R

Αφού ολοκληρωθεί η προμήθεια του συμπλέγματος, χρησιμοποιήστε τα παρακάτω βήματα για να χρησιμοποιήσετε R για να εκτελέσετε μια πράξη MapReduce στο σύμπλεγμα.

1. Συνδεθείτε με το σύμπλεγμα HDInsight χρησιμοποιώντας SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στα παρακάτω:

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix ή λειτουργικό σύστημα OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Από το `username@hn0-CLUSTERNAME:~$` ερώτηση, πληκτρολογήστε την παρακάτω εντολή για να ξεκινήσετε μια περίοδο λειτουργίας με αλληλεπιδραστικών R:

        R

3. Εισαγάγετε το παρακάτω πρόγραμμα R. Αυτό δημιουργεί τους αριθμούς 1 έως και 100 και, στη συνέχεια, πολλαπλασιάζει τους με το 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Στην πρώτη γραμμή καλεί το rmr2 βιβλιοθήκη RHadoop, που χρησιμοποιείται για λειτουργίες MapReduce.

    Η δεύτερη γραμμή δημιουργεί τιμές 1-100, στη συνέχεια, αποθηκεύει τις στο χρησιμοποιώντας σύστημα αρχείων Hadoop `to.dfs`.

    Η τρίτη γραμμή δημιουργεί μια διαδικασία MapReduce χρησιμοποιώντας τη λειτουργικότητα που παρέχεται από rmr2 και αρχίζει επεξεργασίας. Θα πρέπει να βλέπετε περισσότερες από μία γραμμές κύλιση πέρα από τις όπως αρχίζει την επεξεργασία.

4. Στη συνέχεια, χρησιμοποιήστε τα ακόλουθα για να δείτε τη διαδρομή προσωρινό που έχει αποθηκευτεί το αποτέλεσμα του MapReduce για να:

        print(calc())

    Αυτό πρέπει να είναι κάτι παρόμοιο με `/tmp/file5f615d870ad2`. Για να προβάλετε την πραγματική έξοδο, χρησιμοποιήστε τα εξής:

        print(from.dfs(calc))

    Το αποτέλεσμα θα πρέπει να μοιάζει ως εξής:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. Για να έξοδος R, εισαγάγετε τα εξής:

        q()


## <a name="next-steps"></a>Επόμενα βήματα

- [Εγκατάσταση και χρήση συμπλεγμάτων απόχρωση σε HDInsight](hdinsight-hadoop-hue-linux.md). Απόχρωση είναι ένα περιβάλλον εργασίας Χρήστη που σας διευκολύνουν να δημιουργήσετε, να εκτελέσετε και αποθήκευση γουρούνι και ομάδα εργασίες, καθώς και αναζήτηση το προεπιλεγμένο αποθήκευσης για το HDInsight συμπλέγματος web.

- [Εγκατάσταση Giraph στην συμπλεγμάτων HDInsight](hdinsight-hadoop-giraph-install.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε Giraph σε συμπλεγμάτων HDInsight Hadoop. Giraph σάς επιτρέπει να εκτελούν επεξεργασία γραφήματος χρησιμοποιώντας Hadoop, και μπορεί να χρησιμοποιηθεί με το Azure HDInsight.

- [Εγκατάσταση Solr στην συμπλεγμάτων HDInsight](hdinsight-hadoop-solr-install.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε Solr σε συμπλεγμάτων HDInsight Hadoop. Solr σάς επιτρέπει να εκτελέσετε λειτουργίες ισχυρή αναζήτησης σε δεδομένα που έχουν αποθηκευτεί.

- [Εγκατάσταση απόχρωση σε συμπλεγμάτων HDInsight](hdinsight-hadoop-hue-linux.md). Χρησιμοποιήστε σύμπλεγμα προσαρμογής για να εγκαταστήσετε απόχρωση σε συμπλεγμάτων HDInsight Hadoop. Απόχρωση είναι ένα σύνολο των εφαρμογών Web που χρησιμοποιούνται για να αλληλεπιδράσετε με ένα σύμπλεγμα Hadoop.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
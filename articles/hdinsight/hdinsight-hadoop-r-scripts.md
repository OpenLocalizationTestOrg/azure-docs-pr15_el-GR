<properties
    pageTitle="Χρήση R σε HDInsight για να προσαρμόσετε συμπλεγμάτων | Microsoft Azure"
    description="Μάθετε πώς να εγκαταστήσετε R με χρήση δέσμης ενεργειών και χρήση R σε συμπλεγμάτων HDInsight."
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
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Εγκατάσταση και χρήση R στον HDInsight Hadoop συμπλεγμάτων

Μάθετε πώς μπορείτε να προσαρμόσετε τα Windows βάσει σύμπλεγμα HDInsight με R με χρήση δέσμης ενεργειών και τον τρόπο χρήσης R HDInsight συμπλεγμάτων. Η [σειρά premium](https://azure.microsoft.com/pricing/details/hdinsight/) σας δίνει τη δυνατότητα για το HDInsight περιλαμβάνει R διακομιστή ως μέρος του συμπλέγματος HDInsight. Αυτό σας επιτρέπει δέσμες ενεργειών R για να χρησιμοποιήσετε MapReduce και τους για να εκτελέσετε υπολογισμούς κατανέμεται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το διακομιστή R στο HDInsight](hdinsight-hadoop-r-server-get-started.md). Για πληροφορίες σχετικά με τη χρήση R με ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [εγκατάσταση και χρήση R σε συμπλεγμάτων HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
Μπορείτε να εγκαταστήσετε R σε οποιονδήποτε τύπο συμπλέγματος (Hadoop, καταιγίδας, HBase, τους) στο Azure HDInsight με χρήση *Δέσμης ενεργειών*. Ένα δείγμα δέσμης ενεργειών για την εγκατάσταση R σε ένα σύμπλεγμα HDInsight είναι διαθέσιμη από ένα μόνο για ανάγνωση Azure χώρο αποθήκευσης αντικειμένων blob στο [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1). 

**Σχετικά άρθρα**

- [Εγκατάσταση και χρήση R στον συμπλεγμάτων HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md): γενικές πληροφορίες σχετικά με τη δημιουργία HDInsight συμπλεγμάτων
- [Προσαρμογή με χρήση δέσμης ενεργειών σύμπλεγμα HDInsight][hdinsight-cluster-customize]: γενικές πληροφορίες σχετικά με την προσαρμογή συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών
- [Ανάπτυξη δεσμών ενεργειών δέσμης ενεργειών για HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Τι είναι το R;

Το <a href="http://www.r-project.org/" target="_blank">Project R για τον υπολογισμό στατιστικών</a> είναι ένα ανοιχτό γλώσσα προέλευσης και περιβάλλον για τον υπολογισμό στατιστικών. R παρέχει εκατοντάδες ενσωματωμένης στατιστικές συναρτήσεις και δικό του γλώσσα προγραμματισμού που συνδυάζει πτυχές προγραμματισμού λειτουργική και προσανατολισμένα σε αντικείμενα. Επίσης, παρέχει εκτεταμένη δυνατότητες γραφικών. R είναι το περιβάλλον προτιμώμενη προγραμματισμού για πιο επαγγελματική στατιστικών και επιστήμονες σε μια μεγάλη ποικιλία πεδία.

R είναι συμβατά με το χώρο αποθήκευσης αντικειμένων Blob Azure (WASB), έτσι ώστε τα δεδομένα που είναι αποθηκευμένα είναι δυνατή η επεξεργασία χρήση R σε HDInsight.  

## <a name="install-r"></a>Εγκατάσταση R

Ένα [δείγμα δέσμης ενεργειών](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) για την εγκατάσταση R σε ένα σύμπλεγμα HDInsight είναι διαθέσιμη από ένα blob μόνο για ανάγνωση στο χώρο αποθήκευσης Azure. Αυτή η ενότητα παρέχει οδηγίες σχετικά με το πώς μπορείτε να χρησιμοποιήσετε το δείγμα δέσμης ενεργειών κατά τη δημιουργία του συμπλέγματος με την πύλη Azure.

> [AZURE.NOTE] Παρουσιάστηκε το δείγμα δέσμης ενεργειών με το HDInsight σύμπλεγμα έκδοση 3.1. Για περισσότερες πληροφορίες σχετικά με τις εκδόσεις σύμπλεγμα HDInsight, ανατρέξτε στο θέμα [εκδόσεις σύμπλεγμα HDInsight](hdinsight-component-versioning.md).

1. Όταν δημιουργείτε ένα σύμπλεγμα HDInsight από την πύλη, κάντε κλικ στην επιλογή **Προαιρετικό ρύθμισης παραμέτρων**και, στη συνέχεια, κάντε κλικ στην επιλογή **Ενέργειες δέσμης ενεργειών**.
2. Στη σελίδα **Ενέργειες δέσμης ενεργειών** , πληκτρολογήστε τις παρακάτω τιμές:

    ![Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα] (./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα")

    <table border='1'>
        <tr><th>Ιδιότητα</th><th>Τιμή</th></tr>
        <tr><td>Όνομα</td>
            <td>Καθορίστε ένα όνομα για την ενέργεια δέσμη ενεργειών, για παράδειγμα, <b>R εγκατάσταση</b>.</td></tr>
        <tr><td>Δέσμη ενεργειών URI</td>
            <td>Καθορίστε το URI στη δέσμη ενεργειών που καλείται για να προσαρμόσετε το σύμπλεγμα, για παράδειγμα, <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Τύπος κόμβου</td>
            <td>Καθορίστε τους κόμβους στην οποία εκτελείται η δέσμη ενεργειών προσαρμογής. Μπορείτε να επιλέξετε <b>Όλους τους κόμβους</b>, <b>μόνο κόμβους κεφαλή</b>ή <b>κόμβους εργασίας</b> μόνο.
        <tr><td>Παράμετροι</td>
            <td>Καθορίστε τις παραμέτρους, εάν απαιτείται από τη δέσμη ενεργειών. Ωστόσο, η δέσμη ενεργειών για την εγκατάσταση R δεν απαιτεί τις παραμέτρους, ώστε να μπορείτε να αφήσετε αυτό είναι κενό.</td></tr>
    </table>

    Μπορείτε να προσθέσετε περισσότερες από μία ενέργεια δέσμη ενεργειών για την εγκατάσταση πολλών στοιχείων στο σύμπλεγμα. Αφού προσθέσετε τις δέσμες ενεργειών, κάντε κλικ στο σημάδι ελέγχου για να ξεκινήσετε το σύμπλεγμα crating.

Μπορείτε επίσης να χρησιμοποιήσετε τη δέσμη ενεργειών για να εγκαταστήσετε R σε HDInsight με τη χρήση του PowerShell Azure ή το .NET SDK HDInsight. Οδηγίες για αυτές τις διαδικασίες παρέχονται παρακάτω σε αυτό το άρθρο.

## <a name="run-r-scripts"></a>Εκτέλεση δεσμών ενεργειών R
Αυτή η ενότητα περιγράφει πώς μπορείτε να εκτελέσετε μια δέσμη ενεργειών R στο σύμπλεγμα Hadoop με HDInsight.

1. **Δημιουργία μιας σύνδεσης απομακρυσμένης επιφάνειας εργασίας στο σύμπλεγμα**: Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα που δημιουργήσατε με R εγκατεστημένο από την πύλη, και, στη συνέχεια, συνδεθείτε με το σύμπλεγμα. Για οδηγίες, ανατρέξτε στο θέμα [σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#rdp).

2. **Ανοίξτε την κονσόλα R**: το R εγκατάστασης τοποθετεί μια σύνδεση στην κονσόλα R στην επιφάνεια εργασίας του κεφαλής κόμβου. Κάντε κλικ στο για να ανοίξετε την κονσόλα R.

3. **Εκτελέστε τη δέσμη ενεργειών R**: το R δέσμης ενεργειών μπορούν να εκτελεστούν απευθείας από την κονσόλα R επικολλώντας τον, επιλέγοντάς το και πατώντας το πλήκτρο ENTER. Ακολουθεί μια δέσμη ενεργειών απλό παράδειγμα που δημιουργεί τους αριθμούς 1 έως και 100 και, στη συνέχεια, πολλαπλασιάζει τους με το 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

Οι πρώτες δύο γραμμές κλήσεων τις βιβλιοθήκες RHadoop που εγκαθίστανται με R. Η τελική γραμμή εκτυπώνει τα αποτελέσματα στην κονσόλα. Το αποτέλεσμα θα πρέπει να μοιάζει ως εξής:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>Εγκατάσταση με χρήση του Aure PowerShell R

Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Το δείγμα παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους με χρήση του PowerShell Azure. Πρέπει να προσαρμόσετε τη δέσμη ενεργειών για να χρησιμοποιήσετε [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>Εγκατάσταση R χρησιμοποιώντας .NET SDK

Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Το δείγμα παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους χρησιμοποιώντας το .NET SDK. Πρέπει να προσαρμόσετε τη δέσμη ενεργειών για να χρησιμοποιήσετε [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## <a name="see-also"></a>Δείτε επίσης

- [Εγκατάσταση και χρήση R στον συμπλεγμάτων HDinsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md): γενικές πληροφορίες σχετικά με τη δημιουργία HDInsight συμπλεγμάτων
- [Προσαρμογή με χρήση δέσμης ενεργειών σύμπλεγμα HDInsight][hdinsight-cluster-customize]: γενικές πληροφορίες σχετικά με την προσαρμογή συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών
- [Ανάπτυξη δεσμών ενεργειών δέσμης ενεργειών για HDInsight](hdinsight-hadoop-script-actions.md)
- [Εγκατάσταση και χρήση τους σε συμπλεγμάτων HDInsight][hdinsight-install-spark]: δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση τους
- [Εγκατάσταση Giraph σε HDInsight συμπλεγμάτων](hdinsight-hadoop-giraph-install.md): δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση του Giraph
- [Εγκατάσταση Solr σε HDInsight συμπλεγμάτων](hdinsight-hadoop-solr-install-linux.md): δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση του Solr.

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md

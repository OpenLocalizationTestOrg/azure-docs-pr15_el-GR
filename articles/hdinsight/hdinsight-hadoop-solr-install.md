<properties
    pageTitle="Χρησιμοποιήστε την ενέργεια δέσμη ενεργειών για την εγκατάσταση Solr σε σύμπλεγμα Hadoop | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να προσαρμόσετε σύμπλεγμα HDInsight με Solr με χρήση δέσμης ενεργειών."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/05/2016"
    ms.author="nitinme"/>

# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Εγκατάσταση και χρήση Solr στον HDInsight Hadoop συμπλεγμάτων

Μάθετε πώς μπορείτε να προσαρμόσετε σύμπλεγμα HDInsight που βασίζεται σε Windows με Solr με χρήση δέσμης ενεργειών και πώς μπορείτε να χρησιμοποιήσετε Solr για την αναζήτηση δεδομένων. Για πληροφορίες σχετικά με τη χρήση Solr με ένα σύμπλεγμα βάσει Linux, ανατρέξτε στο θέμα [εγκατάσταση και χρήση Solr σε HDinsight Hadoop συμπλεγμάτων (Linux)](hdinsight-hadoop-solr-install-linux.md).
 
Μπορείτε να εγκαταστήσετε Solr σε οποιονδήποτε τύπο συμπλέγματος (Hadoop, καταιγίδας, HBase, τους) στο Azure HDInsight με χρήση *Δέσμης ενεργειών*. Ένα δείγμα δέσμης ενεργειών για την εγκατάσταση Solr σε ένα σύμπλεγμα HDInsight είναι διαθέσιμη από ένα μόνο για ανάγνωση Azure χώρο αποθήκευσης αντικειμένων blob στο [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1). 

Το δείγμα δέσμης ενεργειών λειτουργεί μόνο με HDInsight σύμπλεγμα έκδοση 3.1. Για περισσότερες πληροφορίες σχετικά με εκδόσεις σύμπλεγμα HDInsight, ανατρέξτε στο θέμα [εκδόσεις σύμπλεγμα HDInsight](hdinsight-component-versioning.md).

Το δείγμα δέσμης ενεργειών που χρησιμοποιούνται σε αυτό το θέμα δημιουργεί ένα σύμπλεγμα βασίζεται σε Windows Solr με μια συγκεκριμένη ρύθμιση παραμέτρων. Εάν θέλετε να ρυθμίσετε τις παραμέτρους του συμπλέγματος Solr με διαφορετικές συλλογές, shards, σχήματα, αντίγραφα, κ.λπ., πρέπει να τροποποιείτε τη δέσμη ενεργειών και δυαδικών αρχείων Solr αντίστοιχα.

**Σχετικά άρθρα**

- [Εγκατάσταση και χρήση Solr στον συμπλεγμάτων HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md): γενικές πληροφορίες σχετικά με τη δημιουργία συμπλεγμάτων HDInsight.
- [Προσαρμογή με χρήση δέσμης ενεργειών σύμπλεγμα HDInsight][hdinsight-cluster-customize]: γενικές πληροφορίες σχετικά με την προσαρμογή συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών.
- [Ανάπτυξη δέσμης ενεργειών δέσμες ενεργειών για HDInsight](hdinsight-hadoop-script-actions.md).


## <a name="what-is-solr"></a>Τι είναι το Solr;

<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> είναι μια πλατφόρμα αναζήτησης για μεγάλες επιχειρήσεις που επιτρέπει ισχυρή αναζήτησης πλήρους κειμένου στα δεδομένα. Ενώ Hadoop επιτρέπει την αποθήκευση και τη Διαχείριση τεράστιες ποσότητες δεδομένων, Apache Solr παρέχει τις δυνατότητες αναζήτησης για να ανακτήσετε γρήγορα τα δεδομένα. 

## <a name="install-solr-using-portal"></a>Εγκατάσταση Solr με πύλη

1. Έναρξη δημιουργίας ένα σύμπλεγμα, χρησιμοποιώντας την επιλογή **ΔΗΜΙΟΥΡΓΊΑ ΠΡΟΣΑΡΜΟΣΜΈΝΗΣ** , όπως περιγράφεται στην [συμπλεγμάτων δημιουργία Hadoop στο HDInsight](hdinsight-provision-clusters.md#portal).
2. Στη σελίδα **Ενέργειες δέσμης ενεργειών** του οδηγού, κάντε κλικ στην επιλογή **Προσθήκη ενέργειας δέσμη ενεργειών** για την παροχή λεπτομερειών σχετικά με την ενέργεια δέσμη ενεργειών, όπως φαίνεται παρακάτω:

    ![Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα] (./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα")

    <table border='1'>
        <tr><th>Ιδιότητα</th><th>Τιμή</th></tr>
        <tr><td>Όνομα</td>
            <td>Καθορίστε ένα όνομα για την ενέργεια δέσμης ενεργειών. Για παράδειγμα, <b>Solr εγκατάσταση</b>.</td></tr>
        <tr><td>Δέσμη ενεργειών URI</td>
            <td>Καθορίστε το ενιαίο αναγνωριστικό πόρου (URI) στη δέσμη ενεργειών που καλείται για να προσαρμόσετε το σύμπλεγμα. Για παράδειγμα, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Τύπος κόμβου</td>
            <td>Καθορίστε τους κόμβους στην οποία εκτελείται η δέσμη ενεργειών προσαρμογής. Μπορείτε να επιλέξετε <b>όλους τους κόμβους</b>, <b>μόνο κόμβους κεφαλή</b>ή <b>κόμβους εργασίας μόνο</b>.
        <tr><td>Παράμετροι</td>
            <td>Καθορίστε τις παραμέτρους, εάν απαιτείται από τη δέσμη ενεργειών. Η δέσμη ενεργειών για την εγκατάσταση Solr δεν απαιτεί τις παραμέτρους, ώστε να μπορείτε να αφήσετε αυτό είναι κενό.</td></tr>
    </table>

    Μπορείτε να προσθέσετε περισσότερες από μία ενέργεια δέσμη ενεργειών για την εγκατάσταση πολλών στοιχείων στο σύμπλεγμα. Αφού προσθέσετε τις δέσμες ενεργειών, επιλέξτε το σημάδι επιλογής για να ξεκινήσετε τη δημιουργία του συμπλέγματος.


## <a name="use-solr"></a>Χρήση Solr

Πρέπει να ξεκινήσετε με τη δημιουργία ευρετηρίου Solr με ορισμένα αρχεία δεδομένων. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε Solr για να εκτελέσετε ερωτήματα αναζήτησης στα δεδομένα με ευρετήριο. Ακολουθήστε τα παρακάτω βήματα για να χρησιμοποιήσετε Solr σε ένα σύμπλεγμα HDInsight:

1. **Χρήση απομακρυσμένης επιφάνειας εργασίας Protocol (RDP) σε απομακρυσμένη σε σύμπλεγμα HDInsight με Solr εγκατεστημένο**. Από την πύλη Azure, ενεργοποίηση απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα που δημιουργήσατε με Solr εγκαταστήσει και, στη συνέχεια, remote σε σύμπλεγμα. Για οδηγίες, ανατρέξτε στο θέμα [σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

2. **Ευρετήριο Solr με την αποστολή αρχείων δεδομένων**. Όταν δημιουργήσετε ευρετήριο Solr, μπορείτε να τοποθετήσετε έγγραφα σε αυτό που ίσως χρειαστεί να πραγματοποιήσετε αναζήτηση σε. Για να δημιουργήσετε ευρετήριο Solr, χρησιμοποιήστε RDP σε απομακρυσμένη σε σύμπλεγμα, μεταβείτε στην επιφάνεια εργασίας, ανοίξτε τη γραμμή εντολών Hadoop και μεταβείτε **C:\apps\dist\solr-4.7.2\example\exampledocs**. Εκτελέστε την ακόλουθη εντολή:

        java -jar post.jar solr.xml monitor.xml

    Θα δείτε το εξής αποτέλεσμα στην κονσόλα:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Το βοηθητικό πρόγραμμα post.jar ευρετήρια Solr με δύο δειγμάτων έγγραφα, **solr.xml** και **monitor.xml**. Το βοηθητικό πρόγραμμα post.jar και δείγματα εγγράφων είναι διαθέσιμες με Solr εγκατάσταση.

3. **Χρησιμοποιήστε τον πίνακα εργαλείων Solr για να πραγματοποιήσετε αναζήτηση μέσα στα έγγραφα με ευρετήριο**. Κατά την περίοδο λειτουργίας RDP στο σύμπλεγμα HDInsight, ανοίξτε τον Internet Explorer και εκκίνηση του πίνακα εργαλείων Solr στο **http://headnodehost:8983/solr #/**. Από το αριστερό παράθυρο, από την αναπτυσσόμενη λίστα **Επιλογής πυρήνα** , επιλέξτε **collection1**και μέσα σε, κάντε κλικ στο **ερώτημα**. Ως παράδειγμα, για να επιλέξετε και να επιστρέψετε όλα τα έγγραφα στο Solr, δώστε τις ακόλουθες τιμές:

    * Στο πλαίσιο κειμένου **ερωτήσεις** , πληκτρολογήστε ** \*:**\*. Αυτό θα επιστρέψει όλα τα έγγραφα που έχουν καταχωρηθεί στο ευρετήριο στο Solr. Εάν θέλετε να πραγματοποιήσετε αναζήτηση για μια συγκεκριμένη συμβολοσειρά μέσα στα έγγραφα, μπορείτε να εισαγάγετε η συμβολοσειρά εδώ.
    
    * Στο πλαίσιο κειμένου **wt** , επιλέξτε τη μορφή εξόδου. Η προεπιλογή είναι **json**. Κάντε κλικ στην επιλογή **Εκτέλεση του ερωτήματος**.

    ![Χρήση δέσμης ενεργειών για να προσαρμόσετε ένα σύμπλεγμα] (./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Εκτέλεση ενός ερωτήματος στον πίνακα εργαλείων Solr")
    
    Το αποτέλεσμα εμφανίζει τα δύο έγγραφα που θα σας χρησιμοποιούνται για τη δημιουργία ευρετηρίου Solr. Το αποτέλεσμα μοιάζει με το εξής:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }


4. **Προτείνεται: δημιουργία αντιγράφων ασφαλείας με ευρετήριο δεδομένα από Solr στο χώρο αποθήκευσης αντικειμένων Blob του Azure που σχετίζεται με το σύμπλεγμα HDInsight**. Ως καλή πρακτική, πρέπει να δημιουργήσετε αντίγραφα ασφαλείας των δεδομένων με ευρετήριο από τους κόμβους συμπλέγματος Solr στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Ακολουθήστε τα παρακάτω βήματα για να το κάνετε:

    1. Από την περίοδο λειτουργίας RDP, ανοίξτε τον Internet Explorer και τοποθετήστε το δείκτη στην ακόλουθη διεύθυνση URL:

            http://localhost:8983/solr/replication?command=backup

        Θα πρέπει να δείτε μια απάντηση ως εξής:

            <?xml version="1.0" encoding="UTF-8"?>
            <response>
              <lst name="responseHeader">
                <int name="status">0</int>
                <int name="QTime">9</int>
              </lst>
              <str name="status">OK</str>
            </response>

    2. Στο απομακρυσμένη περίοδο λειτουργίας, μεταβείτε στις {SOLR_HOME}\{συλλογής} \data. Για το σύμπλεγμα που δημιουργήθηκε μέσω του δείγματος δέσμης ενεργειών, αυτό πρέπει να **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Σε αυτήν τη θέση, θα πρέπει να δείτε ένα φάκελο στιγμιότυπου που δημιουργήθηκαν με ένα όνομα που είναι παρόμοια με * *στιγμιότυπο.* χρονική σήμανση***.

    3. Zip στο φάκελο στιγμιότυπο και στείλτε το στο χώρο αποθήκευσης αντικειμένων Blob του Azure. Από τη γραμμή εντολών Hadoop, μεταβείτε στη θέση του φακέλου στιγμιότυπο, χρησιμοποιώντας την ακόλουθη εντολή:

              hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

        Αυτή η εντολή αντιγράφει το στιγμιότυπο /example/data/κάτω από το κοντέινερ μέσα στον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.

## <a name="install-solr-using-aure-powershell"></a>Εγκατάσταση με χρήση του Aure PowerShell Solr

Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell).  Το δείγμα παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους με χρήση του PowerShell Azure. Πρέπει να προσαρμόσετε τη δέσμη ενεργειών για να χρησιμοποιήσετε [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Εγκατάσταση Solr χρησιμοποιώντας .NET SDK

Ανατρέξτε στο θέμα [Προσαρμογή HDInsight συμπλεγμάτων με χρήση δέσμης ενεργειών](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). Το δείγμα παρουσιάζει πώς μπορείτε να εγκαταστήσετε τους χρησιμοποιώντας το .NET SDK. Πρέπει να προσαρμόσετε τη δέσμη ενεργειών για να χρησιμοποιήσετε [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).



## <a name="see-also"></a>Δείτε επίσης

- [Εγκατάσταση και χρήση Solr στον συμπλεγμάτων HDinsight Hadoop (Linux)](hdinsight-hadoop-solr-install-linux.md)
- [Δημιουργία Hadoop συμπλεγμάτων στο HDInsight](hdinsight-provision-clusters.md): γενικές πληροφορίες σχετικά με τη δημιουργία συμπλεγμάτων HDInsight.
- [Προσαρμογή με χρήση δέσμης ενεργειών σύμπλεγμα HDInsight][hdinsight-cluster-customize]: γενικές πληροφορίες σχετικά με την προσαρμογή συμπλεγμάτων HDInsight με χρήση δέσμης ενεργειών.
- [Ανάπτυξη δέσμης ενεργειών δέσμες ενεργειών για HDInsight](hdinsight-hadoop-script-actions.md).
- [Εγκατάσταση και χρήση τους σε συμπλεγμάτων HDInsight][hdinsight-install-spark]: δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση τους.
- [Εγκατάσταση R σε συμπλεγμάτων HDInsight][hdinsight-install-r]: δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση R.
- [Εγκατάσταση Giraph σε HDInsight συμπλεγμάτων](hdinsight-hadoop-giraph-install.md): δείγμα δέσμης ενεργειών σχετικά με την εγκατάσταση του Giraph.


[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md

<properties
    pageTitle="Χρήση αλληλεπιδραστικών ομάδας στο HDInsight | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε αλληλεπιδραστικών Hive (ομάδα σε LLAP) στο HDInsight."
    keywords=""
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
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Χρήση αλληλεπιδραστικών ομάδας στο HDInsight (έκδοση Preview)

Αλληλεπιδραστική Hive (ρομποτική [Ζωντανή Long και διαδικασία]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) είναι ένας νέος HDInsight [σύμπλεγμα τύπου]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Αλληλεπιδραστική ομάδα επιτρέπει σε προσωρινή αποθήκευση στη μνήμη που το κάνει ερωτήματα Hive πολύ πιο αλληλεπιδραστικό και ταχύτερη. Αυτή η νέα δυνατότητα κάνει HDInsight στον κόσμο περισσότερες performant, ευέλικτη, και άνοιγμα λύση μεγάλο δεδομένων στο cloud με στη μνήμη αποθηκεύει προσωρινά (με χρήση της ομάδας και τους) και για προχωρημένους ανάλυση μέσω ενοποίηση με τις υπηρεσίες R. 

Το σύμπλεγμα αλληλεπιδραστικών Hive είναι διαφορετικό από το σύμπλεγμα Hadoop. Περιέχει μόνο την υπηρεσία ομάδας. 

> [AZURE.NOTE] MapReduce, γουρούνι, Sqoop, Oozie και άλλες υπηρεσίες θα καταργηθούν από αυτόν τον τύπο σύμπλεγμα σύντομα.
Η υπηρεσία Hive στο σύμπλεγμα αλληλεπιδραστικών Hive μόνο είναι προσβάσιμα μέσω την προβολή Ambari Hive, Beeline και Hive ODBC. Αυτό δεν είναι δυνατή μέσω κονσόλας Hive, Templeton, Azure CLI και Azure PowerShell. 


 


## <a name="create-an-interactive-hive-cluster"></a>Δημιουργήστε ένα σύμπλεγμα αλληλεπιδραστικών Hive

Αλληλεπιδραστική ομάδα συμπλέγματος υποστηρίζεται μόνο σε βάσει Linux συμπλεγμάτων. Για πληροφορίες σχετικά με τη δημιουργία συμπλεγμάτων HDInsight, ανατρέξτε στο θέμα [Δημιουργία Linux βάσει Hadoop συμπλεγμάτων στο HDInsight](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Εκτέλεση Hive από αλληλεπιδραστικών ομάδας

Υπάρχουν διαφορετικές επιλογές πώς μπορείτε να εκτελείτε ερωτήματα ομάδα:

- Εκτέλεση Hive χρησιμοποιώντας την προβολή Ambari ομάδας

    Για τις πληροφορίες σχετικά με τη χρήση της προβολής Hive, ανατρέξτε στο θέμα [χρήση της προβολής Hive με Hadoop στο HDInsight]( hdinsight-hadoop-use-hive-ambari-view.md).

- Εκτέλεση Hive χρησιμοποιώντας Beeline

    Για τις πληροφορίες σχετικά με τη χρήση Beeline σε HDInsight, ανατρέξτε στο θέμα [Χρήση Hive με Hadoop σε HDInsight με Beeline](hdinsight-hadoop-use-hive-beeline.md).

    Μπορείτε να χρησιμοποιήσετε Beeline από το headnode ή έναν κόμβο κενή άκρο.  Συνιστάται η χρήση Beeline από έναν κόμβο κενή άκρο.  Για πληροφορίες σχετικά με τη δημιουργία ένα σύμπλεγμα HDInsight με μια κενή edgenode, ανατρέξτε στο θέμα [Χρήση κενή άκρο τους κόμβους HDInsight](hdinsight-apps-use-edge-node.md).

- Εκτέλεση Hive χρήση Hive ODBC

    Για τις πληροφορίες σχετικά με τη χρήση Hive ODBC, ανατρέξτε στο θέμα [Σύνδεση να Hadoop με το πρόγραμμα οδήγησης Microsoft Hive ODBC Excel](hdinsight-connect-excel-hive-odbc-driver.md).

**Για να βρείτε τη συμβολοσειρά σύνδεσης JDBC:**

1.  Πραγματοποιήστε είσοδο Ambari χρησιμοποιώντας την παρακάτω διεύθυνση URL: https://<ClusterName>. AzureHDInsight.net.
2.  Κάντε κλικ στην επιλογή **Hive** από το αριστερό μενού.
3.  Κάντε κλικ στο εικονίδιο επισήμανση για να αντιγράψετε τη διεύθυνση URL:

    ![JDBC LLAP αλληλεπιδραστικών Hive HDInsight Hadoop](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Δείτε επίσης
-   [Δημιουργία Linux βάσει Hadoop συμπλεγμάτων στο HDInsight](hdinsight-hadoop-provision-linux-clusters.md): Μάθετε πώς μπορείτε να δημιουργήσετε αλληλεπιδραστικές Hive συμπλεγμάτων στο HDInsight.
-   [Χρήση Hive με Hadoop σε HDInsight με Beeline](hdinsight-hadoop-use-hive-beeline.md): Μάθετε πώς μπορείτε να χρησιμοποιήσετε Beeline για την υποβολή ερωτημάτων ομάδας.
-   [Σύνδεσης του Excel στο Hadoop με το πρόγραμμα οδήγησης Microsoft Hive ODBC](hdinsight-connect-excel-hive-odbc-driver.md): Μάθετε πώς να συνδεθείτε Excel ομάδα.

<properties 
   pageTitle="Χρήση Apache Θήβα και σκίουρου στο HDInsight | Microsoft Azure" 
   description="Μάθετε πώς να χρησιμοποιείτε Apache Θήβα στο HDInsight, καθώς και πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους σκίουρου σε σας σταθμούς εργασίας για να συνδεθείτε με ένα σύμπλεγμα HBase στο HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Χρήση Θήβα Apache με συμπλεγμάτων βάσει Linux HBase στο HDInsight  

Μάθετε πώς να χρησιμοποιείτε [Apache Θήβα](http://phoenix.apache.org/) στο HDInsight, καθώς και τον τρόπο χρήσης του SQLLine. Για περισσότερες πληροφορίες σχετικά με το Θήβα, ανατρέξτε στο θέμα [Θήβα σε 15 λεπτά ή λιγότερο](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Για τη γραμματική Θήβα, ανατρέξτε στο θέμα [Θήβα γραμματικός έλεγχος](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Για τις πληροφορίες έκδοσης Θήβα στα HDInsight, ανατρέξτε στο θέμα [Τι νέο υπάρχει στο τις εκδόσεις σύμπλεγμα Hadoop που παρέχεται από το HDInsight;] [hdinsight-versions].

##<a name="use-sqlline"></a>Χρήση SQLLine
[SQLLine](http://sqlline.sourceforge.net/) είναι ένα βοηθητικό πρόγραμμα γραμμής εντολών για την εκτέλεση SQL. 

###<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Μπορείτε να χρησιμοποιήσετε SQLLine, πρέπει να έχετε τα εξής:

- **Σύμπλεγμα A HBase στο HDInsight**. Για πληροφορίες σχετικά με την παροχή HBase συμπλέγματος, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Apache HBase στο HDInsight][hdinsight-hbase-get-started].
- **Σύνδεση με το σύμπλεγμα HBase μέσω του πρωτοκόλλου απομακρυσμένης επιφάνειας εργασίας**. Για οδηγίες, ανατρέξτε στο θέμα [Διαχείριση Hadoop συμπλεγμάτων στο HDInsight, χρησιμοποιώντας την πύλη κλασική Azure][hdinsight-manage-portal].


Όταν συνδέεστε σε ένα σύμπλεγμα HBase, θα πρέπει να συνδεθείτε σε ένα από τα Zookeepers. Κάθε σύμπλεγμα HDInsight διαθέτει 3 Zookeepers. 

**Για να βρείτε το όνομα κεντρικού υπολογιστή Zookeeper**

1. Ανοίξτε την Ambari κάνοντας αναζήτηση για να **https://<ClusterName>. azurehdinsight.net**.
2. Πληκτρολογήστε το όνομα χρήστη HTTP (σύμπλεγμα) και τον κωδικό πρόσβασης για να συνδεθείτε.
3. Κάντε κλικ στην επιλογή **ZooKeeper** από το αριστερό μενού. Βλέπετε 3 **ZooKeeper διακομιστή** που παρατίθενται.
4. Κάντε κλικ σε μία από το **Διακομιστή ZooKeeper** που αναφέρονται. Στο παράθυρο σύνοψης, βρείτε το **όνομα κεντρικού υπολογιστή**. Είναι παρόμοια με *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Για να χρησιμοποιήσετε SQLLine**

1. Συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας SSH. Για οδηγίες, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, Unix, ή OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ή [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-windows.md) ανάλογα με το λειτουργικό σύστημα του υπολογιστή-πελάτη.

2. Από SSH, εκτελέστε τις ακόλουθες εντολές για να εκτελέσετε SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Εκτελέστε τις ακόλουθες εντολές για να δημιουργήσετε έναν πίνακα HBase και εισαγωγή ορισμένων δεδομένων:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μη αυτόματη SQLLine](http://sqlline.sourceforge.net/#manual) και [Γραμματικός έλεγχος Θήβα](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο μάθατε πώς να χρησιμοποιείτε Apache Θήβα στο HDInsight.  Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα

- [Επισκόπηση HDInsight HBase][hdinsight-hbase-overview]: HBase είναι Apache, Άνοιγμα-προέλευση, NoSQL βάση δεδομένων δημιουργήθηκε στο Hadoop που παρέχει τυχαία πρόσβαση και ισχυρό συνέπειας για μεγάλες ποσότητες δεδομένων μη δομημένα και semistructured.
- [Προμήθεια HBase συμπλεγμάτων σε Azure εικονικού δικτύου][hdinsight-hbase-provision-vnet]: με ενσωμάτωση του εικονικού δικτύου, συμπλεγμάτων HBase μπορεί να αναπτυχθεί το ίδιο εικονικό δίκτυο με τις εφαρμογές σας, έτσι ώστε οι εφαρμογές μπορούν να επικοινωνούν με HBase απευθείας.
- [Ρύθμιση παραμέτρων HBase αναπαραγωγή σε HDInsight](hdinsight-hbase-geo-replication.md): Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους HBase αναπαραγωγής σε δύο Azure κέντρα δεδομένων. 
- [Ανάλυση Twitter άποψη με HBase σε HDInsight][hbase-twitter-sentiment]: Μάθετε πώς μπορείτε να κάνετε σε πραγματικό χρόνο [ανάλυση άποψη](http://en.wikipedia.org/wiki/Sentiment_analysis) μεγάλο δεδομένων με τη χρήση HBase σε ένα σύμπλεγμα Hadoop στο HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 

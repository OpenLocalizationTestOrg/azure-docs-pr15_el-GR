<properties
pageTitle="Χρήση DataFu με γουρούνι σε HDInsight"
description="DataFu είναι μια συλλογή βιβλιοθηκών για χρήση με Hadoop. Μάθετε πώς μπορείτε να χρησιμοποιήσετε DataFu με γουρούνι στην το σύμπλεγμά σας HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Χρήση DataFu με γουρούνι σε HDInsight

DataFu είναι μια συλλογή βιβλιοθηκών Άνοιγμα αρχείου προέλευσης για χρήση με Hadoop. Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε DataFu στην το σύμπλεγμά σας HDInsight και πώς μπορείτε να χρησιμοποιήσετε συναρτήσεις που ορίζονται από το χρήστη DataFu (UDF) με γουρούνι.

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Μια συνδρομή του Azure.

* Ένα σύμπλεγμα Azure HDInsight (Linux ή με βάση τα Windows)

* Βασική εξοικείωση με τη [χρήση των γουρούνι σε HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Εγκατάσταση DataFu σε βάσει Linux HDInsight

> [AZURE.NOTE] DataFu είναι εγκατεστημένη έκδοση βάσει Linux συμπλεγμάτων 3,3 και νεότερη έκδοση και, από συμπλεγμάτων που βασίζεται στα Windows. Δεν είναι εγκατεστημένο στη βάσει Linux συμπλεγμάτων νωρίτερα από 3.3.
>
> Εάν χρησιμοποιείτε μια έκδοση συμπλέγματος που βασίζεται σε Linux 3,3 ή νεότερη έκδοση, ή ένα σύμπλεγμα που βασίζεται στα Windows, μπορείτε να παραλείψετε αυτή την ενότητα.

DataFu να λήψη και εγκατάσταση από το χώρο αποθήκευσης Maven. Χρησιμοποιήστε τα παρακάτω βήματα για να προσθέσετε DataFu το σύμπλεγμά σας HDInsight:

1. Σύνδεση με το σύμπλεγμά σας βάσει Linux HDInsight χρησιμοποιώντας SSH. Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε σε ένα από τα ακόλουθα έγγραφα:

    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, το λειτουργικό σύστημα OS X και Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από το Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Χρησιμοποιήστε την παρακάτω εντολή για να κάνετε λήψη του αρχείου βάζο DataFu χρησιμοποιώντας το βοηθητικό πρόγραμμα wget, ή αντιγράψτε και επικολλήστε τη σύνδεση στο πρόγραμμα περιήγησης για να ξεκινήσετε τη λήψη.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Στη συνέχεια, αποστείλετε το αρχείο στο προεπιλεγμένο αποθήκευσης για το σύμπλεγμα HDInsight. Αυτό το αρχείο γίνεται διαθέσιμο σε όλους τους κόμβους του συμπλέγματος και το αρχείο θα παραμείνει στο χώρο αποθήκευσης, ακόμα και αν μπορείτε να διαγράψετε και να δημιουργήσετε ξανά το σύμπλεγμα.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Το παραπάνω παράδειγμα αποθηκεύει το βάζο στο `wasbs:///example/jars` δεδομένου ότι υπάρχει ήδη αυτόν τον κατάλογο του χώρου αποθήκευσης συμπλέγματος. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε θέση που θέλετε στο χώρο αποθήκευσης σύμπλεγμα HDInsight.

##<a name="use-datafu-with-pig"></a>Χρήση DataFu με γουρούνι

Τα βήματα σε αυτήν την ενότητα προϋποθέτουν ότι είστε εξοικειωμένοι με τη χρήση γουρούνι στο HDInsight και παρέχουν μόνο τις προτάσεις Λατινικά γουρούνι, όχι τα βήματα σχετικά με τον τρόπο χρήσης τους με το σύμπλεγμα. Για περισσότερες πληροφορίες σχετικά με τη χρήση γουρούνι με το HDInsight, ανατρέξτε στο θέμα [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Όταν χρησιμοποιείτε DataFu από γουρούνι σε ένα σύμπλεγμα βάσει Linux HDInsight, πρέπει πρώτα να δηλώσετε το αρχείο βάζο χρησιμοποιώντας την ακόλουθη πρόταση Λατινικά γουρούνι:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu έχει καταχωρηθεί από προεπιλογή στην συμπλεγμάτων HDInsight που βασίζεται στα Windows.

Θα μπορείτε να καθορίσετε ένα ψευδώνυμο για τις συναρτήσεις DataFu συνήθως. Για παράδειγμα:

    DEFINE SHA datafu.pig.hash.SHA();
    
Αυτό ορίζει ένα ψευδώνυμο καθορισμένη `SHA` για το SHA ο κατακερματισμός συνάρτηση. Μπορείτε να χρησιμοποιήσετε αυτό σε λατινικά γουρούνι δέσμη ενεργειών για τη δημιουργία κατακερματισμός για τα δεδομένα εισόδου. Για παράδειγμα, τα ακόλουθα αντικαθιστά τα ονόματα των εισαγόμενων δεδομένων με μια τιμή κατακερματισμός:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Εάν χρησιμοποιείται με τα εξής δεδομένα εισαγωγής:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Θα δημιουργήσει το εξής αποτέλεσμα:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με DataFu ή γουρούνι, ανατρέξτε στα παρακάτω έγγραφα:

* [Οδηγός γουρούνι DataFu Apache](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)

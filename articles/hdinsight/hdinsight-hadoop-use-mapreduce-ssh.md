<properties
   pageTitle="MapReduce και SSH σύνδεσης με το Hadoop στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε SSH για να εκτελέσετε εργασίες MapReduce χρήση Hadoop σε HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Χρήση MapReduce με Hadoop σε HDInsight με SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε ασφαλούς κελύφους (SSH) για να συνδεθείτε σε σύμπλεγμα HDInsight Hadoop και, στη συνέχεια, να υποβάλλουν MapReduce εργασίες, χρησιμοποιώντας εντολές Hadoop.

> [AZURE.NOTE] Εάν είστε ήδη εξοικειωμένοι με τη χρήση των διακομιστών βάσει Linux Hadoop, αλλά είστε εξοικειωμένοι με το HDInsight, ανατρέξτε στο θέμα [συμβουλές βάσει Linux HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Ένα σύμπλεγμα βάσει Linux HDInsight (Hadoop σε HDInsight)

* Ένα πρόγραμμα-πελάτη SSH. Λειτουργικά συστήματα Linux, Unix και Mac θα πρέπει να διαθέτουν ένα πρόγραμμα-πελάτη SSH. Χρήστες των Windows πρέπει να κάνετε λήψη ενός προγράμματος-πελάτη, όπως [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Σύνδεση με SSH

Σύνδεση με το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του συμπλέγματος HDInsight, χρησιμοποιώντας την εντολή SSH. Το FQDN θα είναι το όνομα που σας έδωσε το σύμπλεγμα, ακολουθούμενο από **. azurehdinsight.net**. Για παράδειγμα, το ακόλουθο θα συνδεθείτε σε ένα σύμπλεγμα με το όνομα **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει έναν αριθμό-κλειδί πιστοποιητικό για έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, ίσως χρειαστεί να καθορίσετε τη θέση του ιδιωτικού κλειδιού στο σύστημά σας προγράμματος-πελάτη, για παράδειγμα:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει έναν κωδικό πρόσβασης για τον έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, θα πρέπει να εισαγάγετε τον κωδικό πρόσβασης όταν σας ζητηθεί.

Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με το HDInsight, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από Linux, OS X, και Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>PuTTY (υπολογιστές-πελάτες Windows)

Τα Windows δεν παρέχει ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται να χρησιμοποιείτε **PuTTY**, που μπορούν να ληφθούν από [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με βάσει Linux Hadoop σε HDInsight από τα Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Χρησιμοποιήστε τις εντολές Hadoop

1. Αφού είστε συνδεδεμένοι στο σύμπλεγμα HDInsight, χρησιμοποιήστε την ακόλουθη εντολή **Hadoop** , για να ξεκινήσει μια εργασία MapReduce:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Ξεκινά την κλάση **wordcount** , η οποία περιέχει το αρχείο **hadoop-mapreduce-examples.jar** . Ως είσοδο, που χρησιμοποιεί το έγγραφο **wasbs://example/data/gutenberg/davinci.txt** και εξόδου είναι αποθηκευμένο στο **wasbs: / / / παράδειγμα/δεδομένων/WordCountOutput**.

    > [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με αυτήν την εργασία MapReduce και τα δεδομένα του παραδείγματος, ανατρέξτε στο θέμα [Χρήση MapReduce στο Hadoop σε HDInsight](hdinsight-use-mapreduce.md).

2. Η εργασία εκπέμπει λεπτομέρειες όπως επεξεργάζεται και επιστρέφει πληροφορίες παρόμοιο με το ακόλουθο όταν ολοκληρωθεί η εργασία:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Όταν ολοκληρωθεί η εργασία, χρησιμοποιήστε την παρακάτω εντολή για να παραθέσετε τα αρχεία εξόδου που είναι αποθηκευμένα στο **wasbs://example/data/WordCountOutput**:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    Αυτό πρέπει να εμφανίζει δύο αρχεία, **_SUCCESS** και **τμήμα-r-00000**. Το **τμήμα-r-00000** αρχείο περιέχει την έξοδο για αυτή την εργασία.

    > [AZURE.NOTE] Ορισμένες εργασίες MapReduce μπορεί να διαιρέσετε τα αποτελέσματα σε πολλαπλά αρχεία **τμήμα-r-###** . Εάν Ναι, χρησιμοποιήστε το ### επίθημα για να υποδείξετε τη σειρά των αρχείων.

4. Για να προβάλετε την έξοδο, χρησιμοποιήστε την ακόλουθη εντολή:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Αυτό εμφανίζει μια λίστα με τις λέξεις που περιέχονται στο αρχείο **wasbs://example/data/gutenberg/davinci.txt** και του αριθμού των φορών που παρουσιάστηκε κάθε λέξη. Ακολουθεί ένα παράδειγμα των δεδομένων που θα περιέχονται στο αρχείο:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε, εντολές Hadoop παρέχουν έναν εύκολο τρόπο για να εκτελέσετε εργασίες MapReduce σε ένα σύμπλεγμα HDInsight και, στη συνέχεια, δείτε το αποτέλεσμα του έργου.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με τις εργασίες MapReduce στο HDInsight:

* [Χρήση του MapReduce σε HDInsight Hadoop](hdinsight-use-mapreduce.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

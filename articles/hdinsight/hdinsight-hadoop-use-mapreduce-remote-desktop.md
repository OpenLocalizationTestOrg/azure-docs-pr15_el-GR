<properties
   pageTitle="MapReduce και απομακρυσμένης επιφάνειας εργασίας με Hadoop στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε απομακρυσμένης επιφάνειας εργασίας για να συνδεθείτε με Hadoop σε HDInsight και να εκτελέσετε εργασίες MapReduce."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Χρήση MapReduce σε Hadoop σε HDInsight με σύνδεση απομακρυσμένης επιφάνειας εργασίας

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να συνδεθείτε σε σύμπλεγμα HDInsight Hadoop, χρησιμοποιώντας σύνδεση απομακρυσμένης επιφάνειας εργασίας και, στη συνέχεια, να εκτελέσετε εργασίες MapReduce, χρησιμοποιώντας την εντολή Hadoop.

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Ένα σύμπλεγμα HDInsight που βασίζεται σε Windows (Hadoop σε HDInsight)

* Πρόγραμμα-πελάτη υπολογιστή που εκτελεί Windows 10, Windows 8 ή Windows 7

##<a id="connect"></a>Σύνδεση με απομακρυσμένη επιφάνεια εργασίας

Ενεργοποίηση της απομακρυσμένης επιφάνειας εργασίας για το σύμπλεγμα HDInsight και, στη συνέχεια, συνδεθείτε, ακολουθώντας τις οδηγίες στην ενότητα [σύνδεση με χρήση RDP συμπλεγμάτων HDInsight](hdinsight-administer-use-management-portal.md#rdp).

##<a id="hadoop"></a>Χρησιμοποιήστε την εντολή Hadoop

Όταν είστε συνδεδεμένοι στην επιφάνεια εργασίας για το σύμπλεγμα HDInsight, χρησιμοποιήστε τα παρακάτω βήματα για να εκτελέσετε μια εργασία MapReduce, χρησιμοποιώντας την εντολή Hadoop:

1. Από την επιφάνεια εργασίας HDInsight, ξεκινήστε τη **γραμμή εντολών Hadoop**. Η ενέργεια αυτή ανοίγει μια νέα γραμμή εντολών στο το **c:\apps\dist\hadoop-&lt;αριθμό έκδοσης >** καταλόγου.

    > [AZURE.NOTE] Ο αριθμός έκδοσης αλλάζει καθώς ενημερώνεται Hadoop. Η μεταβλητή περιβάλλοντος **HADOOP_HOME** μπορεί να χρησιμοποιηθεί για να βρείτε τη διαδρομή. Για παράδειγμα, `cd %HADOOP_HOME%` αλλάζει σε καταλόγους στον κατάλογο Hadoop, χωρίς να χρειάζεται να γνωρίζετε τον αριθμό έκδοσης.

2. Για να χρησιμοποιήσετε την εντολή **Hadoop** , για να εκτελέσετε μια εργασία MapReduce παράδειγμα, χρησιμοποιήστε την ακόλουθη εντολή:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Ξεκινά την κλάση **wordcount** , η οποία περιέχει το αρχείο **hadoop-mapreduce-examples.jar** στον τρέχοντα κατάλογο. Ως είσοδο, που χρησιμοποιεί το έγγραφο **wasbs://example/data/gutenberg/davinci.txt** και εξόδου είναι αποθηκευμένο στο: **wasbs: / / / παράδειγμα/δεδομένων/WordCountOutput**.

    > [AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με αυτήν την εργασία MapReduce και τα δεδομένα του παραδείγματος, ανατρέξτε στο θέμα <a href="hdinsight-use-mapreduce.md">Χρήση MapReduce στο HDInsight Hadoop</a>.

2. Η εργασία εκπέμπει λεπτομέρειες που υποβάλλεται σε επεξεργασία και επιστρέφει πληροφορίες παρόμοιο με το ακόλουθο όταν η εργασία έχει ολοκληρωθεί:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Όταν ολοκληρωθεί η εργασία, χρησιμοποιήστε την ακόλουθη εντολή για να παραθέσετε τα αρχεία εξόδου που είναι αποθηκευμένα στο **wasbs://example/data/WordCountOutput**:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    Αυτό πρέπει να εμφανίζει δύο αρχεία, **_SUCCESS** και **τμήμα-r-00000**. Το **τμήμα-r-00000** αρχείο περιέχει την έξοδο για αυτή την εργασία.

    > [AZURE.NOTE] Ορισμένες εργασίες MapReduce μπορεί να διαιρέσετε τα αποτελέσματα σε πολλαπλά αρχεία **τμήμα-r-###** . Εάν Ναι, χρησιμοποιήστε το ### επίθημα για να υποδείξετε τη σειρά των αρχείων.

4. Για να προβάλετε την έξοδο, χρησιμοποιήστε την ακόλουθη εντολή:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Αυτό εμφανίζει μια λίστα με τις λέξεις που περιέχονται στο αρχείο **wasbs://example/data/gutenberg/davinci.txt** , μαζί με τον αριθμό των φορών που παρουσιάστηκε κάθε λέξη. Ακολουθεί ένα παράδειγμα των δεδομένων που θα περιέχονται στο αρχείο:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε την εντολή Hadoop αποτελούν έναν εύκολο τρόπο για να εκτελέσετε εργασίες MapReduce σε ένα σύμπλεγμα HDInsight και, στη συνέχεια, δείτε το αποτέλεσμα του έργου.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με τις εργασίες MapReduce στο HDInsight:

* [Χρήση του MapReduce σε HDInsight Hadoop](hdinsight-use-mapreduce.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

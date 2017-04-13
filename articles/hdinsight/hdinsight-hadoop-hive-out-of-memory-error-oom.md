<properties
    pageTitle="Έξοδος από το σφάλμα μνήμης (ΟΥΜ) - ρυθμίσεις Hive | Microsoft Azure"
    description="Διορθώστε ένα σφάλμα εκτός μνήμης (ΟΥΜ) από ένα ερώτημα Hive στο Hadoop στο HDInsight. Το σενάριο πελάτη είναι ένα ερώτημα σε πολλά μεγάλους πίνακες."
    keywords="Έξοδος από το ρυθμίσεις Hive σφάλμα, ΟΥΜ, μνήμης"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Διόρθωση σφάλμα εκτός μνήμης (ΟΥΜ) με τις ρυθμίσεις μνήμης Hive στο Hadoop στο Azure HDInsight

Ένα από τα κοινά προβλήματα μας πρόσωπο πελάτες είναι εμφανίζεται εκτός μνήμης (ΟΥΜ) σφάλμα όταν με χρήση της ομάδας. Σε αυτό το άρθρο περιγράφει ένα σενάριο πελάτη και τις ρυθμίσεις της ομάδας, συνιστούμε να διορθώσετε το πρόβλημα.

## <a name="scenario-hive-query-across-large-tables"></a>Σενάριο: Hive ερώτημα σε μεγάλους πίνακες

Ένας πελάτης εκτελέσατε το ερώτημα κάτω από το στοιχείο με χρήση της ομάδας.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Ορισμένες nuances αυτού του ερωτήματος:

* Τ1 είναι ένα ψευδώνυμο σε μεγάλο πίνακα, Πίνακας1, η οποία έχει πολλούς τύπους στηλών ΣΥΜΒΟΛΟΣΕΙΡΆ.
* Άλλους πίνακες που δεν είναι μεγάλο, αλλά έχετε μεγάλο αριθμό στηλών.
* Όλοι οι πίνακες θα συμμετάσχουν μεταξύ τους, σε ορισμένες περιπτώσεις με πολλές στήλες με TABLE1 και οι άλλοι χρήστες.

Όταν ο πελάτης εκτελέσατε το ερώτημα χρήση ομάδας σε MapReduce σε έναν κόμβο 24 σύμπλεγμα A3, εκτελέσατε το ερώτημα σε περίπου 26 λεπτά. Ο πελάτης παρατηρήσει τα ακόλουθα μηνύματα προειδοποίησης όταν ήταν η εκτέλεση του ερωτήματος με χρήση της ομάδας στην MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Επειδή το ερώτημα ολοκληρωθεί η εκτέλεση στο περίπου 26 λεπτά, ο πελάτης παραβλέπεται αυτές τις προειδοποιήσεις και αντί για αυτό για να ξεκινήσει η εστίαση σχετικά με τον τρόπο για να βελτιώσετε την εξής περαιτέρω επιδόσεων του ερωτήματος.

Ο πελάτης τη γνώμη [Βελτιστοποίηση Hive ερωτήματα για Hadoop σε HDInsight](hdinsight-hadoop-optimize-hive-query.md)και αποφασίσει να χρησιμοποιήσετε μηχανισμός εκτέλεσης Tez. Μόλις το ίδιο ερώτημα εκτελέστηκε με ενεργοποιημένη τη ρύθμιση Tez το ερώτημα εκτελέσατε για 15 λεπτά και, στη συνέχεια, δημιούργησε το ακόλουθο σφάλμα:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Ο πελάτης αποφασίσει, στη συνέχεια, να χρησιμοποιήσετε ένα μεγαλύτερο Εικονική (δηλαδή D12) υπόψη ένα μεγαλύτερο Εικονική να έχουν περισσότερο χώρο σωρού. Ακόμα και, στη συνέχεια, ο πελάτης συνεχίζεται για να δείτε το σφάλμα. Ο πελάτης φτάσει στην ομάδα HDInsight για βοήθεια σχετικά με τον εντοπισμό σφαλμάτων σε αυτό το ζήτημα.

## <a name="debug-the-out-of-memory-oom-error"></a>Εντοπισμός σφαλμάτων το σφάλμα εκτός μνήμης (ΟΥΜ)

Υποστήριξη και μας ομάδες προγραμματισμού μαζί εντόπισε ένα από τα ζητήματα που προκαλούν το σφάλμα εκτός μνήμης (ΟΥΜ) ήταν ένα [γνωστό θέμα που περιγράφονται σε το JIRA Apache](https://issues.apache.org/jira/browse/HIVE-8306). Από την περιγραφή στο το JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Θα σας επιβεβαιωθεί που **hive.auto.convert.join.noconditionaltask** στην πραγματικότητα έχει οριστεί στην **τιμή true** , εάν ανατρέξετε στην περιοχή ομάδα site.xml αρχείο:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Με βάση την προειδοποίηση και το JIRA, μας υπόθεση ήταν χάρτη συμμετοχή ήταν η αιτία του σφάλματος Java σωρού χώρο ΟΥΜ. Επομένως, θα σας dug βαθύτερη σε αυτό το ζήτημα.

Όπως εξηγείται στην καταχώρηση ιστολογίου [Ρυθμίσεις μνήμης νήματα Hadoop στο HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), όταν Tez εκτέλεσης του μηχανισμού χρησιμοποιείται το χώρο σωρού που χρησιμοποιείται στην πραγματικότητα ανήκει στο κοντέινερ Tez. Δείτε την εικόνα κάτω από το στοιχείο που περιγράφει τη μνήμη κοντέινερ Tez.

![Διάγραμμα μνήμης κοντέινερ Tez: Hive από το σφάλμα μνήμης ΟΥΜ](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Καθώς προτείνει τη δημοσίευση ιστολογίου, οι ακόλουθες ρυθμίσεις δύο μνήμης Ορισμός η μνήμη κοντέινερ για η στοίβα: **hive.tez.container.size** και **hive.tez.java.opts**. Από την εμπειρία, η εξαίρεση ΟΥΜ δεν σημαίνει το μέγεθος του κοντέινερ είναι πολύ μικρό. Αυτό σημαίνει ότι το μέγεθος σωρού Java (hive.tez.java.opts) είναι πολύ μικρό. Επομένως, κάθε φορά που μπορείτε να δείτε ΟΥΜ, μπορείτε να δοκιμάσετε για να αυξήσετε το **hive.tez.java.opts**. Εάν είναι απαραίτητο, ίσως χρειαστεί να αυξήσετε **hive.tez.container.size**. Η ρύθμιση **java.opts** πρέπει να είναι περίπου 80% των **container.size**.

> [AZURE.NOTE]  Η ρύθμιση **hive.tez.java.opts** πρέπει να είναι πάντα μικρότερη από **hive.tez.container.size**.

Επειδή ένα μηχάνημα D12 έχει 28GB μνήμης, αποφασίσει να χρησιμοποιήσετε ένα κοντέινερ μέγεθος των 10GB (10240MB) και εκχωρήστε 80% java.opts. Αυτό έγινε στην κονσόλα Hive χρησιμοποιώντας την παρακάτω ρύθμιση:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Που βασίζονται σε αυτές τις ρυθμίσεις, το ερώτημα εκτελέστηκε με επιτυχία στο στην περιοχή δέκα λεπτά.

## <a name="conclusion-oom-errors-and-container-size"></a>Ολοκλήρωση: Σφάλματα ΟΥΜ και το μέγεθος του κοντέινερ

Εμφανίζεται σφάλμα ΟΥΜ δεν σημαίνει απαραίτητα το μέγεθος του κοντέινερ είναι πολύ μικρό. Αντί για αυτό, θα πρέπει να ρυθμίσετε τις παραμέτρους μνήμη, έτσι ώστε το μέγεθος σωρού αυξάνεται και έχει τουλάχιστον 80% του μεγέθους μνήμης του κοντέινερ.

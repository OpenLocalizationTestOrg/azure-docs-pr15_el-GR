<properties
   pageTitle="Χρήση Hadoop χοίρου με SSH σε ένα σύμπλεγμα HDInsight | Microsoft Azure"
   description="Μάθετε πώς να συνδεθείτε σε ένα σύμπλεγμα με βάση το Linux Hadoop με SSH και στη συνέχεια χρησιμοποιήστε την εντολή χοίρου αλληλεπιδραστική εκτέλεση δηλώσεις Λατινικά χοίρου, ή ως μια μαζική εργασία."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Εκτέλεση εργασιών χοίρου σε ένα σύμπλεγμα που βασίζεται σε Linux με την εντολή χοίρου (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Σε αυτό το έγγραφο θα καθοδηγήσει τη διαδικασία σύνδεσης σε ένα σύμπλεγμα με βάση το Linux HDInsight Azure χρησιμοποιώντας Secure κέλυφος (SSH), στη συνέχεια χρησιμοποιώντας την εντολή χοίρων να λειτουργεί αλληλεπιδραστικά, δηλώσεις Λατινικά χοίρου ή ως μια μαζική εργασία.

Η γλώσσα προγραμματισμού Λατινικά χοίρου σάς επιτρέπει να περιγράφουν μετασχηματισμοί που έχουν εφαρμοστεί στα δεδομένα εισόδου για να παραγάγει το επιθυμητό αποτέλεσμα.

> [AZURE.NOTE] Εάν είστε ήδη εξοικειωμένοι με τη χρήση της με βάση το Linux Hadoop διακομιστές, αλλά έχετε πραγματοποιήσει HDInsight, ανατρέξτε στην ενότητα [Με βάση το Linux συμβουλές HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Προϋποθέσεις

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής.

* Ένα σύμπλεγμα HDInsight με βάση το Linux (Hadoop σε HDInsight).

* Ένα πρόγραμμα-πελάτη SSH. Linux, Unix και Mac OS πρέπει να συνοδεύονται από ένα πρόγραμμα-πελάτη SSH. Οι χρήστες των Windows πρέπει να κάνετε λήψη ενός προγράμματος-πελάτη, όπως [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Συνδεθείτε με SSH

Σύνδεση με το πλήρως αναγνωρισμένο όνομα τομέα (FQDN) του συμπλέγματος HDInsight, χρησιμοποιώντας την εντολή SSH. Το FQDN θα είναι το όνομα που δώσατε στη συνέχεια το σύμπλεγμα, **. azurehdinsight.net**. Για παράδειγμα, το ακόλουθο κείμενο θα συνδεθεί σε ένα σύμπλεγμα που ονομάζεται **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Εάν έχετε δώσει ένα κλειδί πιστοποιητικό για έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το HDInsight σύμπλεγμα, ίσως χρειαστεί να καθορίσετε τη θέση του ιδιωτικού κλειδιού στο σύστημα του υπολογιστή-πελάτη σας.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Εάν έχετε δώσει έναν κωδικό πρόσβασης για έλεγχο ταυτότητας SSH** όταν δημιουργήσατε το σύμπλεγμα HDInsight, θα πρέπει να δώσετε τον κωδικό πρόσβασης όταν σας ζητηθεί.

Για περισσότερες πληροφορίες σχετικά με τη χρήση SSH με HDInsight, ανατρέξτε στο θέμα [Χρήση SSH με Linux με Hadoop σε HDInsight από Linux, OS X, και Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>PuTTY (των Windows σε υπολογιστές-πελάτες)

Τα Windows παρέχουν ένα ενσωματωμένο πρόγραμμα-πελάτη SSH. Συνιστάται η χρήση **PuTTY**, που μπορεί να ληφθεί από το [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Για περισσότερες πληροφορίες σχετικά με τη χρήση PuTTY, ανατρέξτε στο θέμα [Χρήση SSH με Linux με Hadoop σε HDInsight από τα Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Χρησιμοποιήστε την εντολή του χοίρου

2. Αφού συνδεθείτε, ξεκινήστε το περιβάλλον γραμμής εντολών (CLI) του χοίρου, χρησιμοποιώντας την ακόλουθη εντολή.

        pig

    Έπειτα από λίγο, θα πρέπει να δείτε ένα `grunt>` εντολών.

3. Πληκτρολογήστε την παρακάτω πρόταση.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    Αυτή η εντολή φορτώνει τα περιεχόμενα του αρχείου sample.log σε αρχεία ΚΑΤΑΓΡΑΦΉΣ. Μπορείτε να προβάλετε τα περιεχόμενα του αρχείου, χρησιμοποιώντας το ακόλουθο κείμενο.

        DUMP LOGS;

4. Στη συνέχεια, μετατροπή των δεδομένων από την εφαρμογή μια κανονική έκφραση για να εξαγάγετε μόνο το επίπεδο καταγραφής από κάθε εγγραφή, χρησιμοποιώντας τα ακόλουθα.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Μπορείτε να χρησιμοποιήσετε την **ΈΝΔΕΙΞΗ** για να προβάλετε τα δεδομένα μετά από τη μετατροπή. Σε αυτήν την περίπτωση, χρησιμοποιήστε `DUMP LEVELS;`.

5. Συνεχίστε την εφαρμογή μετασχηματισμών, χρησιμοποιώντας τις ακόλουθες δηλώσεις. Χρήση `DUMP` για να προβάλετε τα αποτελέσματα του μετασχηματισμού μετά από κάθε βήμα.

    <table>
    <tr>
    <th>Δήλωση</th><th>Τι κάνει</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = ΕΠΊΠΕΔΑ του ΦΊΛΤΡΟΥ από ΜΌΝΟ του δεν είναι null.</td><td>Καταργεί τις γραμμές που περιέχουν μια τιμή null για το επίπεδο καταγραφής και αποθηκεύει τα αποτελέσματα σε FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS ΟΜΆΔΑ από LOGLEVEL;</td><td>Ομαδοποιεί τις γραμμές από το επίπεδο καταγραφής και αποθηκεύει τα αποτελέσματα σε GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>ΣΥΧΝΌΤΗΤΕΣ = foreach GROUPEDLEVELS Δημιουργία ομάδας ως LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL) κατά την ΚΑΤΑΜΈΤΡΗΣΗ;</td><td>Δημιουργεί ένα νέο σύνολο δεδομένων που περιέχει κάθε μοναδικό αρχείο καταγραφής τιμή επιπέδου και πόσες φορές αυτό συμβαίνει. Αποθηκεύεται σε ΣΥΧΝΌΤΗΤΕΣ.</td>
    </tr>
    <tr>
    <td>ΑΠΟΤΈΛΕΣΜΑ = σειρά ΣΥΧΝΌΤΗΤΕΣ από την ένδειξη desc ΠΛΉΘΟΣ;</td><td>Ταξινομεί τα επίπεδα καταγραφής από απογραφή (φθίνουσα) και την αποθηκεύει σε ΑΠΟΤΈΛΕΣΜΑ.</td>
    </tr>
    </table>

6. Μπορείτε επίσης να αποθηκεύσετε τα αποτελέσματα του μετασχηματισμού, χρησιμοποιώντας το `STORE` δήλωση. Για παράδειγμα, στο ακόλουθο αποθηκεύεται το `RESULT` στον κατάλογο **/example/data/pigout** το προεπιλεγμένο κοντέινερ αποθήκευσης για το σύμπλεγμα.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Τα δεδομένα αποθηκεύονται στον καθορισμένο κατάλογο στα αρχεία που ονομάζονται **nnnnn τμήμα**. Εάν ο κατάλογος υπάρχει ήδη, θα λάβετε ένα σφάλμα.

7. Για να κλείσετε τη γραμμή εντολών grunt, πληκτρολογήστε την παρακάτω πρόταση.

        QUIT;

###<a name="pig-latin-batch-files"></a>Αρχεία δέσμης χοίρου Λατινικά

Μπορείτε επίσης να χρησιμοποιήσετε την εντολή "χοίρος" για να εκτελέσετε Λατινικά χοίρου που περιέχονται σε ένα αρχείο.

3. Μετά την έξοδο από τη γραμμή εντολών grunt, χρησιμοποιήστε την ακόλουθη εντολή για να διοχέτευσης STDIN σε ένα αρχείο που ονομάζεται **pigbatch.pig**. Αυτό το αρχείο θα δημιουργηθεί στον κεντρικό κατάλογο για το λογαριασμό που έχετε συνδεθεί σε για την περίοδο λειτουργίας SSH.

        cat > ~/pigbatch.pig

4. Πληκτρολογήστε ή επικολλήστε τις παρακάτω γραμμές και, στη συνέχεια, χρησιμοποιήστε το συνδυασμό πλήκτρων Ctrl + D όταν ολοκληρωθεί.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Χρησιμοποιήστε τα ακόλουθα για να εκτελέσετε το αρχείο **pigbatch.pig** χρησιμοποιώντας την εντολή χοίρου.

        pig ~/pigbatch.pig

    Όταν ολοκληρωθεί η μαζική εργασία, θα πρέπει να εμφανιστεί το ακόλουθο αποτέλεσμα, που θα πρέπει να είναι το ίδιο όταν χρησιμοποιείται `DUMP RESULT;` στα προηγούμενα βήματα.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Σύνοψη

Όπως μπορείτε να δείτε την εντολή χοίρου σάς επιτρέπει να αλληλεπιδραστικά εκτέλεση λειτουργιών MapReduce χρησιμοποιώντας Λατινικά χοίρου, καθώς και εκτέλεση δηλώσεις που αποθηκεύονται σε ένα αρχείο δέσμης.

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με χοίρου στο HDInsight.

* [Χρησιμοποιήστε χοίρου με Hadoop σε HDInsight](hdinsight-use-pig.md)

Για πληροφορίες σχετικά με άλλους τρόπους, μπορείτε να εργαστείτε με Hadoop σε HDInsight.

* [Χρήση ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

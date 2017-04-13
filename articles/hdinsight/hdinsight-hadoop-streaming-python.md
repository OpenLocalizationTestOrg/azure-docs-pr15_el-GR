<properties
   pageTitle="Ανάπτυξη Python MapReduce έργα με HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να εκτελέσετε εργασίες Python MapReduce στην συμπλεγμάτων βάσει Linux HDInsight."
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

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Ανάπτυξη Python ροής προγράμματα για HDInsight

Hadoop παρέχει ένα API ροής για MapReduce που σας επιτρέπει να γράψετε χάρτη και να μειώσετε συναρτήσεις σε γλώσσα διαφορετική από Java. Σε αυτό το άρθρο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε Python για την εκτέλεση λειτουργιών MapReduce.

> [AZURE.NOTE] Ενώ ο κώδικας Python σε αυτό το έγγραφο μπορεί να χρησιμοποιηθεί με ένα σύμπλεγμα HDInsight που βασίζεται στα Windows, τα βήματα σε αυτό το έγγραφο είναι συγκεκριμένες για βάσει Linux συμπλεγμάτων.

Σε αυτό το άρθρο βασίζεται σε πληροφορίες και παραδείγματα δημοσιεύτηκε από Michael Noll στην [εγγραφή ενός προγράμματος MapReduce Hadoop στο Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight

* Ένα πρόγραμμα επεξεργασίας κειμένου

    > [AZURE.IMPORTANT] Πρόγραμμα επεξεργασίας κειμένου πρέπει να χρησιμοποιήσετε LF ως κατάληξη γραμμής. Εάν χρησιμοποιεί CRLF, αυτό θα προκαλέσει σφάλματα όταν εκτελείται η εργασία MapReduce σε συμπλεγμάτων βάσει Linux HDInsight. Εάν δεν είστε βέβαιοι, χρησιμοποιήστε το προαιρετικό βήμα στην ενότητα [Εκτέλεση MapReduce](#run-mapreduce) για να μετατρέψετε οποιοδήποτε CRLF LF.

* Για πελάτες των Windows, PuTTY και PSCP. Αυτά τα βοηθητικά προγράμματα είναι διαθέσιμες από τη <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Σελίδα λήψης PuTTY</a>.

##<a name="word-count"></a>Καταμέτρηση λέξεων

Για αυτό το παράδειγμα, θα μπορείτε να εφαρμόσετε ένα βασικό των λέξεων, χρησιμοποιώντας ένα πρόγραμμα αντιστοίχισης και reducer. Το πρόγραμμα αντιστοίχισης χωρίζει προτάσεων σε μεμονωμένες λέξεις και το reducer συγκεντρώνει τις λέξεις και απαριθμεί για να προκύψει το αποτέλεσμα.

Το παρακάτω διάγραμμα παρουσιάζει τι συμβαίνει κατά τη διάρκεια του χάρτη και να μειώσετε τις φάσεις.

![Μειώστε την απεικόνιση χάρτη](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Γιατί Python;

Python είναι μια γλώσσα προγραμματισμού γενικής χρήσης, υψηλού επιπέδου που σας επιτρέπει να express έννοιες λιγότερες γραμμές κώδικα από πολλές άλλες γλώσσες. Αυτό περιλαμβάνει έγινε πρόσφατα δημοφιλείς με επιστήμονες δεδομένων ως γλώσσα τη δημιουργία πρωτοτύπων επειδή το ερμηνευθεί φύσης, δυναμική, σύνταξης και πληκτρολόγησης κομψές είναι κατάλληλο για την ανάπτυξη εφαρμογών γρήγορη.

Python έχει εγκατασταθεί σε όλους συμπλεγμάτων HDInsight.

##<a name="streaming-mapreduce"></a>Ροή MapReduce

Hadoop σάς επιτρέπει να καθορίσετε ένα αρχείο που περιέχει το χάρτη και μείωση της λογικής που χρησιμοποιείται από μια εργασία. Οι συγκεκριμένες απαιτήσεις για το χάρτη και να μειώσετε λογικής είναι:

* **Εισαγωγής**: το χάρτη και να μειώσετε τα στοιχεία πρέπει να διαβάσει εισαγωγής δεδομένων από το STDIN.

* **Αποτέλεσμα**: το χάρτη και να μειώσετε τα στοιχεία πρέπει να γράψετε δεδομένα εξόδου στο STDOUT.

* **Μορφοποίηση δεδομένων**: τα δεδομένα που καταναλώθηκε και παραχθεί πρέπει να είναι ένα ζεύγος κλειδιού/τιμής, διαχωρισμένα με χαρακτήρα tab.

Python μπορούν να διαχειρίζονται με ευκολία αυτές τις απαιτήσεις με χρήση της λειτουργικής μονάδας **συστήματος** την ανάγνωση STDIN και χρησιμοποιώντας την **Εκτύπωση** για να εκτυπώσετε σε STDOUT. Η υπόλοιπη εργασία είναι απλώς μορφοποίηση των δεδομένων με μια καρτέλα (`\t`) χαρακτήρα μεταξύ το κλειδί και τιμή.

##<a name="create-the-mapper-and-reducer"></a>Δημιουργία αντιστοίχισης και του reducer

Το πρόγραμμα αντιστοίχισης και reducer είναι αρχεία κειμένου, σε αυτή την περίπτωση **mapper.py** και **reducer.py** (για να κάνετε απαλοιφή που κάνει τι). Μπορείτε να δημιουργήσετε τα χρησιμοποιώντας το πρόγραμμα επεξεργασίας της επιλογής σας.

###<a name="mapperpy"></a>Mapper.PY

Δημιουργήστε ένα νέο αρχείο με το όνομα **mapper.py** και χρησιμοποιήστε τον ακόλουθο κώδικα ως το περιεχόμενο:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Αφιερώστε λίγο χρόνο για να διαβάσετε μέσω του κώδικα, ώστε να μπορείτε να κατανοήσετε τι κάνει.

###<a name="reducerpy"></a>Reducer.PY

Δημιουργήστε ένα νέο αρχείο με το όνομα **reducer.py** και χρησιμοποιήστε τον ακόλουθο κώδικα ως το περιεχόμενο:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Στείλτε τα αρχεία

**Mapper.py** και **reducer.py** πρέπει να είναι στον κεφαλής κόμβο του συμπλέγματος πριν να μπορούμε να εκτελέσουμε τους. Ο ευκολότερος τρόπος για την αποστολή τους είναι να χρησιμοποιήσετε **scp** (**pscp** Εάν χρησιμοποιείτε ένα πρόγραμμα-πελάτη Windows).

Από το πρόγραμμα-πελάτη, στον ίδιο κατάλογο ως **mapper.py** και **reducer.py**, χρησιμοποιήστε την ακόλουθη εντολή. Αντικαταστήστε το **όνομα χρήστη** με ένα χρήστη SSH και **clustername** με το όνομα του συμπλέγματος.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Αυτό αντιγράφει τα αρχεία από το τοπικό σύστημα στον κόμβο κεφαλίδας.

> [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση SSH το λογαριασμό σας, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παράμετρο και τη διαδρομή προς το ιδιωτικό κλειδί, για παράδειγμα, `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Εκτέλεση MapReduce

1. Συνδεθείτε με το σύμπλεγμα χρησιμοποιώντας SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Εάν χρησιμοποιούσατε έναν κωδικό πρόσβασης για την ασφάλιση SSH το λογαριασμό σας, θα σας ζητηθεί για τον κωδικό πρόσβασης. Εάν χρησιμοποιείτε ένα πλήκτρο SSH, ίσως χρειαστεί να χρησιμοποιήσετε το `-i` παράμετρο και τη διαδρομή προς το ιδιωτικό κλειδί, για παράδειγμα, `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Προαιρετικό) Εάν χρησιμοποιήσατε ένα πρόγραμμα επεξεργασίας κειμένου που χρησιμοποιεί CRLF ως γραμμές που τελειώνουν με κατά τη δημιουργία τα αρχεία mapper.py και reducer.py, ή δεν γνωρίζετε τι τέλους της γραμμής πρόγραμμα επεξεργασίας χρησιμοποιεί, χρησιμοποιήστε την παρακάτω εντολές για να μετατρέψετε τις εμφανίσεις του CRLF στο mapper.py και reducer.py LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Χρησιμοποιήστε την παρακάτω εντολή για να ξεκινήσει η εργασία MapReduce.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    Αυτή η εντολή έχει τα εξής τμήματα:

    * **hadoop streaming.jar**: χρησιμοποιείται κατά την εκτέλεση λειτουργιών ροής MapReduce. Το περιβάλλοντα εργασίας Hadoop με το εξωτερικό κώδικα MapReduce παρέχετε.

    * **-αρχεία**: Hadoop ενημερώνει που απαιτούνται για αυτή την εργασία MapReduce τα καθορισμένα αρχεία και θα πρέπει να αντιγραφούν σε όλους τους κόμβους εργαζόμενου.

    * **-πρόγραμμα αντιστοίχισης**: Hadoop, ενημερώνετε το αρχείο που θα χρησιμοποιηθεί ως το πρόγραμμα αντιστοίχισης.

    * **-reducer**: Hadoop, ενημερώνετε το αρχείο που θα χρησιμοποιηθεί ως το reducer.

    * **-εισαγωγής**: το αρχείο εισαγωγής που θα σας θα πρέπει να μετρήσετε λέξεις από.

    * **-εξόδου**: καταλόγου που θα εγγραφούν το αποτέλεσμα.

        > [AZURE.NOTE] Αυτός ο κατάλογος θα δημιουργηθεί από την εργασία.

Πρέπει να δείτε μια ομάδα δηλώσεις **ΠΛΗΡΟΦΟΡΊΕΣ** ως ξεκινήσει η εργασία και, τέλος ανατρέξτε στο θέμα η λειτουργία **χάρτη** και να **μειώσετε** εμφανίζονται ως ποσοστά.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Όταν ολοκληρωθεί, θα λάβετε πληροφορίες κατάστασης σχετικά με την εργασία.

##<a name="view-the-output"></a>Προβάλετε την έξοδο

Όταν ολοκληρωθεί η εργασία, χρησιμοποιήστε την ακόλουθη εντολή για να προβάλετε το αποτέλεσμα:

    hdfs dfs -text /example/wordcountout/part-00000

Αυτό θα πρέπει να εμφανίσετε μια λίστα των λέξεων και πόσες φορές τη λέξη Παρουσιάστηκε. Ακολουθεί ένα δείγμα των δεδομένων εξόδου:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς να χρησιμοποιήσετε τις εργασίες ροής MapRedcue με HDInsight, χρησιμοποιήστε τις παρακάτω συνδέσεις για να εξερευνήσετε άλλους τρόπους για να εργαστείτε με Azure HDInsight.

* [Χρήση της ομάδας με το HDInsight](hdinsight-use-hive.md)
* [Χρήση γουρούνι με HDInsight](hdinsight-use-pig.md)
* [Χρήση MapReduce εργασίες με το HDInsight](hdinsight-use-mapreduce.md)

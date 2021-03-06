<properties
   pageTitle="Χρήση MapReduce και καμπύλη με Hadoop στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να εκτελέσετε από απόσταση εργασίες MapReduce με Hadoop σε HDInsight με καμπύλη."
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

#<a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-curl"></a>Εκτελέστε MapReduce εργασίες με Hadoop στο HDInsight με καμπύλη

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Σε αυτό το έγγραφο, θα μάθετε πώς μπορείτε να χρησιμοποιήσετε καμπύλη για να εκτελέσετε εργασίες MapReduce σε Hadoop σε σύμπλεγμα HDInsight.

Καμπύλη χρησιμοποιείται για να δείχνουν πώς μπορείτε να αλληλεπιδράσετε με HDInsight με τη χρήση ανεπεξέργαστο αιτήσεις HTTP για να εκτελέσετε εργασίες MapReduce. Αυτό λειτουργεί, χρησιμοποιώντας το WebHCat REST API (παλαιότερα γνωστό ως Templeton) που παρέχεται από το σύμπλεγμά σας HDInsight.

> [AZURE.NOTE] Εάν είστε ήδη εξοικειωμένοι με τη χρήση των διακομιστών βάσει Linux Hadoop, αλλά είστε εξοικειωμένοι με το HDInsight, ανατρέξτε στο θέμα [Τι πρέπει να γνωρίζετε σχετικά με βάσει Linux Hadoop σε HDInsight](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Hadoop σε σύμπλεγμα HDInsight (Linux ή βασίζεται στα Windows)

* [Καμπύλη](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a id="curl"></a>Εκτέλεση MapReduce εργασίες χρησιμοποιώντας καμπύλη

> [AZURE.NOTE] Όταν χρησιμοποιείτε καμπύλη ή οποιοδήποτε άλλο ΥΠΌΛΟΙΠΟ επικοινωνίας με WebHCat, πρέπει να τον έλεγχο ταυτότητας των αιτήσεων, παρέχοντας το όνομα χρήστη του διαχειριστή συμπλέγματος HDInsight και τον κωδικό πρόσβασης. Μπορείτε, επίσης, πρέπει να χρησιμοποιήσετε το όνομα του συμπλέγματος ως μέρος του URI που χρησιμοποιείται για να στείλετε τις αιτήσεις στο διακομιστή.
>
> Για τις εντολές σε αυτήν την ενότητα, αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** με το χρήστη για τον έλεγχο ταυτότητας στο σύμπλεγμα και **τον κωδικό ΠΡΌΣΒΑΣΗΣ** με τον κωδικό πρόσβασης για το λογαριασμό χρήστη. Αντικαταστήστε **CLUSTERNAME** με το όνομα του συμπλέγματος.
>
> Το REST API είναι ασφαλής, χρησιμοποιώντας [τον έλεγχο ταυτότητας βασικές πρόσβασης](http://en.wikipedia.org/wiki/Basic_access_authentication). Θα πρέπει να κάνετε πάντα αιτήσεις χρησιμοποιώντας HTTP για να βεβαιωθείτε ότι τα διαπιστευτήριά σας αποστέλλονται με ασφάλεια στο διακομιστή.

1. Από μια γραμμή εντολών, χρησιμοποιήστε την παρακάτω εντολή για να επιβεβαιώσετε ότι μπορείτε να συνδεθείτε με το σύμπλεγμά σας HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Θα πρέπει να λαμβάνετε μια απάντηση παρόμοια με τα εξής:

        {"status":"ok","version":"v1"}

    Οι παράμετροι χρησιμοποιούνται σε αυτήν την εντολή είναι ως εξής:

    * **-u**: υποδεικνύει το όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείται για τον έλεγχο ταυτότητας της αίτησης
    * **-G**: υποδεικνύει ότι αυτό είναι ένα αίτημα GET

    Στην αρχή του URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, είναι το ίδιο για όλες τις αιτήσεις.

2. Για να υποβάλετε μια εργασία MapReduce, χρησιμοποιήστε την ακόλουθη εντολή:

        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d jar=wasbs:///example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=wasbs:///example/data/gutenberg/davinci.txt -d arg=wasbs:///example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar

    Στο τέλος της το URI (/ mapreduce/βάζο) ενημερώνει WebHCat ότι αυτή η αίτηση θα ξεκινήσει μια εργασία MapReduce από μια κλάση σε ένα αρχείο βάζο. Οι παράμετροι χρησιμοποιούνται σε αυτήν την εντολή είναι ως εξής:

    * **-d**: `-G` δεν χρησιμοποιείται, ώστε να την αίτηση προεπιλογών για τη μέθοδο POST. `-d`Καθορίζει τις τιμές των δεδομένων που έχουν αποσταλεί με την αίτηση.

        * **User.Name**: Ο χρήστης που εκτελεί την εντολή
        * **βάζο**: εκτελέσατε τη θέση του αρχείου βάζο που περιέχει κλάση ώστε να είναι
        * **κλάσης**: την κλάση που περιέχει τη λογική MapReduce
        * **όρισμα**: τα ορίσματα περνούν στο έργο MapReduce; σε αυτήν την περίπτωση, το αρχείο εισαγωγής κειμένου και τον κατάλογο που χρησιμοποιούνται για την έξοδο

    Αυτή η εντολή πρέπει να επιστρέψει ένα Αναγνωριστικό εργασίας που μπορούν να χρησιμοποιηθούν για να ελέγξετε την κατάσταση της εργασίας:

        {"id":"job_1415651640909_0026"}

3. Για να ελέγξετε την κατάσταση της εργασίας, χρησιμοποιήστε την ακόλουθη εντολή. Αντικαταστήστε το **JOBID** με την τιμή που επιστρέφεται στο προηγούμενο βήμα. Για παράδειγμα, εάν η τιμή επιστροφής ήταν `{"id":"job_1415651640909_0026"}`, στη συνέχεια, θα ήταν η JOBID `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Εάν η εργασία είναι ολοκληρωμένη, το μέλος θα είναι "ολοκληρώθηκε με ΕΠΙΤΥΧΊΑ".

    > [AZURE.NOTE] Αυτή η αίτηση καμπύλη επιστρέφει ένα έγγραφο JSON με πληροφορίες σχετικά με την εργασία; jq χρησιμοποιείται για να ανακτήσετε μόνο την τιμή κατάσταση.

4. Όταν η κατάσταση της εργασίας έχει αλλάξει **ολοκληρώθηκε με**, μπορείτε να ανακτήσετε τα αποτελέσματα της εργασίας από το χώρο αποθήκευσης αντικειμένων Blob του Azure. Το `statusdir` παράμετρο που διαβιβάζεται με το ερώτημα περιέχει στη θέση του το αρχείο εξόδου. σε αυτήν την περίπτωση, **wasbs: / / / παράδειγμα/καμπύλη**. Αυτή η διεύθυνση αποθηκεύει το αποτέλεσμα του έργου στον κατάλογο **παράδειγμα/καμπύλη** στην το προεπιλεγμένο κοντέινερ χώρου αποθήκευσης που χρησιμοποιείται από το σύμπλεγμά σας HDInsight.

Μπορείτε να λίστα και να κάνετε λήψη αυτών των αρχείων, χρησιμοποιώντας το [Azure CLI](../xplat-cli-install.md). Για παράδειγμα, σε λίστα αρχεία στο **παράδειγμα/καμπύλη**, χρησιμοποιήστε την ακόλουθη εντολή:

    azure storage blob list <container-name> example/curl

Για να κάνετε λήψη ενός αρχείου, χρησιμοποιήστε τα εξής:

    azure storage blob download <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Πρέπει να καθορίσετε το όνομα του λογαριασμού χώρου αποθήκευσης που περιέχει το αντικείμενο blob, χρησιμοποιώντας το `-a` και `-k` παράμετροι ή ρύθμιση του **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΛΟΓΑΡΙΑΣΜΟΎ** και **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ACCESS\_αριθμού-ΚΛΕΙΔΙΟΎ** μεταβλητές περιβάλλοντος. Δείτε [πώς μπορείτε να αποστείλετε δεδομένων με το HDInsight](hdinsight-upload-data.md) για περισσότερες πληροφορίες.

##<a id="summary"></a>Σύνοψη

Όπως φαίνεται σε αυτό το έγγραφο, μπορείτε να χρησιμοποιήσετε ανεπεξέργαστα αίτηση HTTP για να εκτελέσετε, παρακολούθηση και προβολή αποτελεσμάτων του εργασίες ομάδας στο σύμπλεγμά σας HDInsight.

Για περισσότερες πληροφορίες σχετικά με το περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ που χρησιμοποιείται σε αυτό το άρθρο, ανατρέξτε στο άρθρο [Αναφορά WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).

##<a id="nextsteps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με τις εργασίες MapReduce στο HDInsight:

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

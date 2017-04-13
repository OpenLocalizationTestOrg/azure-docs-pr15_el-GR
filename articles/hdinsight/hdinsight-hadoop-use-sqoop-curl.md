<properties
   pageTitle="Χρήση Hadoop Sqoop με καμπύλη στο HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να υποβάλετε απομακρυσμένα Sqoop εργασίες με το HDInsight με καμπύλη."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="jgao"/>

#<a name="run-sqoop-jobs-with-hadoop-in-hdinsight-with-curl"></a>Εκτέλεση εργασιών Sqoop με Hadoop σε HDInsight με καμπύλη

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε καμπύλη για να εκτελέσετε εργασίες Sqoop σε ένα σύμπλεγμα Hadoop στο HDInsight.

Καμπύλη χρησιμοποιείται για να δείχνουν πώς μπορείτε να αλληλεπιδράσετε με HDInsight με τη χρήση ανεπεξέργαστο αιτήσεις HTTP για να εκτελέσετε, παρακολούθηση και ανακτήστε τα αποτελέσματα της Sqoop εργασίες. Αυτό λειτουργεί, χρησιμοποιώντας το WebHCat REST API (παλαιότερα γνωστό ως Templeton) που παρέχεται από το σύμπλεγμά σας HDInsight.

> [AZURE.NOTE] Εάν είστε ήδη εξοικειωμένοι με τη χρήση των διακομιστών βάσει Linux Hadoop, αλλά είστε εξοικειωμένοι με το HDInsight, ανατρέξτε στο θέμα [πληροφορίες σχετικά με τη χρήση HDInsight στην Linux](hdinsight-hadoop-linux-information.md).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, θα χρειαστείτε τα εξής:

* Hadoop σε σύμπλεγμα HDInsight (Linux ή βασίζεται στα Windows)

* [Καμπύλη](http://curl.haxx.se/)

* [jq](http://stedolan.github.io/jq/)

##<a name="submit-sqoop-jobs-by-using-curl"></a>Υποβολή Sqoop εργασιών με τη χρήση καμπύλη

> [AZURE.NOTE] Όταν χρησιμοποιείτε καμπύλη ή οποιοδήποτε άλλο ΥΠΌΛΟΙΠΟ επικοινωνίας με WebHCat, πρέπει να τον έλεγχο ταυτότητας των αιτήσεων, παρέχοντας το όνομα χρήστη και τον κωδικό πρόσβασης για τη Διαχείριση συμπλέγματος HDInsight. Μπορείτε, επίσης, πρέπει να χρησιμοποιήσετε το όνομα του συμπλέγματος ως μέρος της το ενιαίο αναγνωριστικό πόρου (URI) χρησιμοποιείται για την αποστολή των αιτήσεων στο διακομιστή.
>
> Για τις εντολές σε αυτήν την ενότητα, αντικαταστήστε το **όνομα ΧΡΉΣΤΗ** με το χρήστη για τον έλεγχο ταυτότητας στο σύμπλεγμα και αντικαταστήστε **τον κωδικό ΠΡΌΣΒΑΣΗΣ** με τον κωδικό πρόσβασης για το λογαριασμό χρήστη. Αντικαταστήστε **CLUSTERNAME** με το όνομα του συμπλέγματος.
>
> Το REST API προστατεύεται μέσω [βασικό έλεγχο ταυτότητας](http://en.wikipedia.org/wiki/Basic_access_authentication). Θα πρέπει να κάνετε πάντα αιτήσεις με τη χρήση ασφαλούς HTTP (HTTPS) για να εξασφαλίσετε ότι τα διαπιστευτήριά σας αποστέλλονται με ασφάλεια στο διακομιστή.

1. Από τη γραμμή εντολών, χρησιμοποιήστε την ακόλουθη εντολή για να επιβεβαιώσετε ότι μπορείτε να συνδεθείτε με το σύμπλεγμά σας HDInsight:

        curl -u USERNAME:PASSWORD -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status

    Θα πρέπει να λαμβάνετε μια απάντηση παρόμοια με τα εξής:

        {"status":"ok","version":"v1"}

    Οι παράμετροι χρησιμοποιούνται σε αυτήν την εντολή είναι ως εξής:

    * **-u** - το όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείται για τον έλεγχο ταυτότητας της αίτησης.
    * **-G** - υποδεικνύει ότι αυτό είναι ένα αίτημα GET.

    Αρχή της διεύθυνσης URL, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, θα είναι το ίδιο για όλες τις αιτήσεις. Τη διαδρομή, **/status/status**, υποδεικνύει ότι η αίτηση είναι για να επιστρέψετε σε κατάσταση WebHCat (γνωστό και ως Templeton) για το διακομιστή. 

2. Χρησιμοποιήστε τα ακόλουθα για να υποβάλετε μια εργασία sqoop:


        curl -u USERNAME:PASSWORD -d user.name=USERNAME -d command="export --connect jdbc:sqlserver://SQLDATABASESERVERNAME.database.windows.net;user=USERNAME@SQLDATABASESERVERNAME;password=PASSWORD;database=SQLDATABASENAME --table log4jlogs --export-dir /tutorials/usesqoop/data --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasbs:///example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/sqoop

    Οι παράμετροι χρησιμοποιούνται σε αυτήν την εντολή είναι ως εξής:

    * **-d** - εφόσον `-G` δεν χρησιμοποιείται, την αίτηση προεπιλογών για τη μέθοδο POST. `-d`Καθορίζει τις τιμές των δεδομένων που έχουν αποσταλεί με την αίτηση.

        * **user.name** - ο χρήστης που εκτελεί την εντολή.

        * **εντολή** - Sqoop την εντολή για την εκτέλεση.

        * **statusdir** - τον κατάλογο που θα εγγραφούν την κατάσταση για αυτό το έργο.

    Αυτή η εντολή πρέπει να επιστρέφει το Αναγνωριστικό εργασίας που μπορούν να χρησιμοποιηθούν για να ελέγξετε την κατάσταση της εργασίας.

        {"id":"job_1415651640909_0026"}

3. Για να ελέγξετε την κατάσταση της εργασίας, χρησιμοποιήστε την ακόλουθη εντολή. Αντικαταστήστε **JOBID** με την τιμή που επιστρέφεται στο προηγούμενο βήμα. Για παράδειγμα, εάν η τιμή επιστροφής ήταν `{"id":"job_1415651640909_0026"}`, στη συνέχεια, θα ήταν **JOBID** `job_1415651640909_0026`.

        curl -G -u USERNAME:PASSWORD -d user.name=USERNAME https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state

    Εάν η εργασία έχει ολοκληρωθεί, η κατάσταση θα **ολοκληρώθηκε με**.

    > [AZURE.NOTE] Αυτή η αίτηση καμπύλη επιστρέφει ένα έγγραφο σημειογραφίας αντικειμένων JavaScript (JSON) με πληροφορίες σχετικά με την εργασία; jq χρησιμοποιείται για να ανακτήσετε μόνο την τιμή κατάσταση.

4. Όταν η κατάσταση της εργασίας έχει αλλάξει **ολοκληρώθηκε με**, μπορείτε να ανακτήσετε τα αποτελέσματα της εργασίας από το χώρο αποθήκευσης αντικειμένων Blob του Azure. Το `statusdir` παράμετρος που μεταβιβάστηκε με το ερώτημα περιέχει στη θέση του το αρχείο εξόδου. σε αυτήν την περίπτωση, **wasbs: / / / παράδειγμα/καμπύλη**. Αυτή η διεύθυνση αποθηκεύει το αποτέλεσμα του έργου στον κατάλογο **παράδειγμα/καμπύλη** το προεπιλεγμένο κοντέινερ χώρου αποθήκευσης που χρησιμοποιείται από το σύμπλεγμά σας HDInsight.

    Μπορείτε να λίστα και να κάνετε λήψη αυτών των αρχείων, χρησιμοποιώντας το [Azure CLI](../xplat-cli-install.md). Για παράδειγμα, σε λίστα αρχεία στο **παράδειγμα/καμπύλη**, χρησιμοποιήστε την ακόλουθη εντολή:

        azure storage blob list <container-name> example/curl

    Για να κάνετε λήψη ενός αρχείου, χρησιμοποιήστε τα εξής:

        azure storage blob download <container-name> <blob-name> <destination-file>

    > [AZURE.NOTE] Είτε πρέπει να καθορίσετε το όνομα του λογαριασμού χώρου αποθήκευσης που περιέχει το αντικείμενο blob, χρησιμοποιώντας το `-a` και `-k` παράμετροι ή ρύθμιση του **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ΛΟΓΑΡΙΑΣΜΟΎ** και **AZURE\_χώρου ΑΠΟΘΉΚΕΥΣΗΣ\_ACCESS\_αριθμού-ΚΛΕΙΔΙΟΎ** μεταβλητές περιβάλλοντος. Ανατρέξτε στην ενότητα < ένα href = προορισμού "hdinsight-αποστολή-data.md" = "_blank" για περισσότερες πληροφορίες.

##<a name="limitations"></a>Περιορισμοί

* Μαζική export - βάσει με Linux HDInsight, Sqoop σύνδεσης που χρησιμοποιείται για την εξαγωγή δεδομένων Microsoft SQL Server ή βάση δεδομένων SQL Azure δεν υποστηρίζει τη συγκεκριμένη στιγμή μαζικής εισάγεται.

* Δέσμης - με βάσει Linux HDInsight, όταν χρησιμοποιείτε το `-batch` εναλλαγή κατά την εκτέλεση εισάγει, Sqoop θα εκτελέσει πολλών εισάγει αντί για δέσμης για τις λειτουργίες εισαγωγή.

##<a name="summary"></a>Σύνοψη

Όπως φαίνεται σε αυτό το έγγραφο, μπορείτε να χρησιμοποιήσετε μια αίτηση HTTP ανεπεξέργαστα για εκτέλεση, παρακολούθηση και προβολή αποτελεσμάτων του Sqoop εργασίες στην το σύμπλεγμά σας HDInsight.

Για περισσότερες πληροφορίες σχετικά με το περιβάλλον εργασίας ΥΠΌΛΟΙΠΟ που χρησιμοποιούνται σε αυτό το άρθρο, ανατρέξτε στο θέμα <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">Οδηγός Sqoop REST API</a>.

##<a name="next-steps"></a>Επόμενα βήματα

Για γενικές πληροφορίες σχετικά με το HDInsight της ομάδας:

* [Χρήση Sqoop με Hadoop σε HDInsight](hdinsight-use-sqoop.md)

Για πληροφορίες σχετικά με άλλους τρόπους μπορείτε να εργαστείτε με Hadoop σε HDInsight:

* [Χρήση της ομάδας με Hadoop σε HDInsight](hdinsight-use-hive.md)

* [Χρήση γουρούνι με Hadoop σε HDInsight](hdinsight-use-pig.md)

* [Χρήση MapReduce με Hadoop σε HDInsight](hdinsight-use-mapreduce.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx



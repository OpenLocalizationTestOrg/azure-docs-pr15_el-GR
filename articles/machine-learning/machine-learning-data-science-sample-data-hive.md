<properties
    pageTitle="Δείγμα δεδομένων σε πίνακες Azure HDInsight Hive | Microsoft Azure"
    description="Προς τα κάτω δειγματοληψία δεδομένων σε πίνακες Hive Azure HDInsight (Hadopop)"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Δείγμα δεδομένων σε πίνακες Azure HDInsight ομάδας

Σε αυτό το άρθρο θα σας περιγράφουν τον τρόπο δεδομένα που είναι αποθηκευμένα σε πίνακες Azure HDInsight Hive χρήση ερωτημάτων Hive δειγμάτων προς τα κάτω. Ασχοληθούμε με τρεις popularly χρησιμοποιείται δειγματοληψία μεθόδους:

* Ομοιόμορφη τυχαία δειγματοληψία
* Τυχαία δειγματοληψία με ομάδες
* Δειγματοληψία των

**Δείγμα γιατί τα δεδομένα σας;**
Εάν το σύνολο δεδομένων που σχεδιάζετε να αναλύσετε είναι μεγάλο, είναι συνήθως καλή ιδέα να τα δεδομένα για να μειώσετε το μέγεθος μικρότερο αλλά αντιπρόσωπος και πιο εύχρηστη δειγμάτων προς τα κάτω. Αυτό διευκολύνει την κατανόηση των δεδομένων, Εξερεύνηση και η δυνατότητα μηχανικής. Ο ρόλος της διαδικασίας Science δεδομένων ομάδας είναι η ενεργοποίηση γρήγορα τη δημιουργία πρωτοτύπων του λειτουργίες επεξεργασίας δεδομένων και μηχανικής εκμάθησης μοντέλων.

Το **μενού** παρακάτω συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δείγμα δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Αυτή η εργασία δειγματοληψία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Τον τρόπο υποβολής ερωτημάτων ομάδας
Τα ερωτήματα της ομάδας μπορούν να υποβληθούν από την κονσόλα γραμμής εντολών Hadoop στον κεφαλής κόμβο του συμπλέγματος Hadoop. Για να το κάνετε αυτό, συνδεθείτε με το κεφαλής κόμβο του συμπλέγματος Hadoop, ανοίξτε την κονσόλα γραμμής εντολών Hadoop και υποβάλετε τα ερωτήματα Hive από εκεί. Για οδηγίες σχετικά με την υποβολή ερωτημάτων Hive στην κονσόλα γραμμής εντολών Hadoop, δείτε [πώς μπορείτε να υποβάλετε Hive ερωτήματα](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Ομοιόμορφη τυχαία δειγματοληψία
Ομοιόμορφη τυχαία δειγματοληψία σημαίνει ότι κάθε γραμμή του συνόλου δεδομένων έχει μια ίση πιθανότητα που δειγματοληψία. Αυτό μπορεί να υλοποιηθεί, προσθέτοντας ένα πεδίο επιπλέον rand() στο σύνολο δεδομένων στο εσωτερικό ερώτημα "επιλέξτε" και στο εξωτερικός ερώτημα "επιλογή" συνθήκη σε αυτό το πεδίο τυχαία.

Ακολουθεί ένα παράδειγμα ερωτήματος:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

Εδώ, `<sample rate, 0-1>` Καθορίζει την αναλογία των εγγραφών που οι χρήστες θέλουν να δείγμα.

## <a name="group"></a>Τυχαία δειγματοληψία με ομάδες

Όταν δειγματοληψία κατηγοριών δεδομένων, ενδέχεται να θέλετε να συμπεριλάβετε ή να αποκλείσετε όλες τις παρουσίες κάποια συγκεκριμένη τιμή μιας μεταβλητής κατηγοριών. Πρόκειται για τι προορίζεται κατά "δειγματοληψία κατά ομάδα".
Για παράδειγμα, εάν έχετε μια έλαβε μεταβλητή "Κατάσταση", που έχει τις τιμές NY κυλιόμενος μέσος ΌΡΟΣ, CA, NJ, PA, κλπ, που θέλετε να τις εγγραφές του ίδιου μέλους είναι πάντα μαζί, αν είναι δειγματοληψία ή όχι.

Ακολουθεί ένα παράδειγμα ερωτήματος για συγκεκριμένη δείγματα κατά ομάδα:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Δειγματοληψία των

Τυχαία δειγματοληψία είναι των σε σχέση με μια μεταβλητή έλαβε όταν τα δείγματα που λαμβάνονται έχουν τιμές που έλαβε που βρίσκονται σε την αναλογία όπως ο πληθυσμός γονικό στοιχείο από την οποία έχουν ληφθεί τα δείγματα. Χρησιμοποιώντας το ίδιο παράδειγμα ως παραπάνω, ας υποθέσουμε ότι τα δεδομένα σας έχει δευτερεύουσες πληθυσμών από μέλη, πείτε NJ έχει 100 παρατηρήσεις, NY έχει 60 παρατηρήσεις και Αττικη έχει 300 παρατηρήσεις. Εάν καθορίσετε το συντελεστή των δειγματοληψία να είναι 0,5, στη συνέχεια, το δείγμα που λαμβάνονται πρέπει να έχουν περίπου 50, 30 και 150 παρατηρήσεις NJ, NY και Αττικη αντίστοιχα.

Ακολουθεί ένα παράδειγμα ερωτήματος:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Για πληροφορίες σχετικά με την πιο σύνθετες μέθοδοι δειγματοληψία που είναι διαθέσιμες στην ομάδα, ανατρέξτε στο θέμα [LanguageManual δειγματοληψία](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

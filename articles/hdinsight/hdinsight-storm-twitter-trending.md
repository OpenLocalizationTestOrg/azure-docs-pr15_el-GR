<properties
   pageTitle="Twitter τάσης θέματα με καταιγίδας Apache στην HDInsight | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε Trident για να δημιουργήσετε μια τοπολογία καταιγίδας Apache που καθορίζει τάσης θέματα στο Twitter βάση hashtags."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Καθορισμός Twitter τάσης θέματα με καταιγίδας Apache στην HDInsight

Μάθετε πώς μπορείτε να χρησιμοποιήσετε Trident για να δημιουργήσετε μια τοπολογία καταιγίδας που καθορίζει τάσης θέματα (κατακερματισμός ετικέτες) στο Twitter.

Trident είναι μια υψηλού επιπέδου αφαίρεσης που παρέχει εργαλεία όπως το συνδέσμων, συναθροίσεων, ομαδοποίησης, συναρτήσεις και τα φίλτρα. Επιπλέον, Trident προσθέτει στοιχειώδεις τύπους για κάνοντας κατάστασης, τμηματική επεξεργασίας. Το παράδειγμα αυτό παρουσιάζει πώς μπορείτε να δημιουργήσετε μια τοπολογία χρησιμοποιώντας ένα προσαρμοσμένο στομίου, συνάρτηση και πολλές ενσωματωμένες συναρτήσεις που παρέχονται από Trident.

> [AZURE.NOTE] Αυτό το παράδειγμα είναι σε μεγάλο βαθμό με βάση το παράδειγμα [Καταιγίδας Trident](https://github.com/jalonsoramos/trident-storm) από Alonso Χουάν.

##<a name="requirements"></a>Απαιτήσεις

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java και το JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Ένας λογαριασμός Twitter για προγραμματιστές

##<a name="download-the-project"></a>Κάντε λήψη του έργου

Χρησιμοποιήστε τον ακόλουθο κώδικα η κλωνοποίηση τοπικά στο έργο.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Τοπολογία

Η τοπολογία για αυτό το παράδειγμα είναι ως εξής:

![τοπολογία](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] Αυτή είναι μια απλοποιημένη προβολή της τοπολογίας. Πολλές παρουσίες των στοιχείων θα κατανέμεται τους κόμβους του συμπλέγματος.

Ο κώδικας Trident που υλοποιεί της τοπολογίας είναι ως εξής:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Αυτός ο κωδικός κάνει τα εξής:

1. Δημιουργεί μια νέα ροή από το στομίου. Το στομίου ανακτά tweets από Twitter και φιλτράρει τους για συγκεκριμένες λέξεις-κλειδιά (αγάπη, μουσική και καφέ σε αυτό το παράδειγμα).

2. Χρησιμοποιείται HashtagExtractor, μια προσαρμοσμένη συνάρτηση, για να εξαγάγετε κατακερματισμός ετικετών από κάθε tweet. Αυτά είναι αποσταλεί στη ροή.

3. Η ροή είναι ομαδοποιημένα κατά ετικέτα κατακερματισμός και που του μεταβιβάστηκε ενός συλλέκτη. Συνάθροιση αυτό δημιουργεί μια καταμέτρηση του πόσες φορές Παρουσιάστηκε κάθε ετικέτα κατακερματισμός. Αυτά τα δεδομένα είναι μόνιμα στη μνήμη. Τέλος, μια νέα ροή αποστέλλεται που περιέχει την ετικέτα κατακερματισμός και το πλήθος.

4. Επειδή θα σας ενδιαφέρει μόνο τα πιο δημοφιλή tag κατακερματισμός για μια δεδομένη δέσμη tweets, συγκρότησης **FirstN** εφαρμόζεται για να επιστρέψετε μόνο τις 10 κορυφαίες τιμές, με βάση το πεδίο "Καταμέτρηση".

> [AZURE.NOTE] Εκτός από το στομίου και HashtagExtractor, χρησιμοποιούμε ενσωματωμένες λειτουργίες Trident.
>
> Για πληροφορίες σχετικά με τις ενσωματωμένες λειτουργίες, ανατρέξτε στο θέμα <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">πακέτο storm.trident.operation.builtin</a>.
>
> Για υλοποιήσεις Trident φάσεων εκτός από το MemoryMapState, ανατρέξτε στα εξής:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Αναζήτηση ελαστικότητας Trident καταιγίδας</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident redis</a>

###<a name="the-spout"></a>Το στομίου

Το στομίου, **TwitterSpout**, χρησιμοποιεί <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> για να ανακτήσετε tweets από Twitter. Δημιουργείται ένα φίλτρο (αγάπη, μουσική και καφέ σε αυτό το παράδειγμα) και τα εισερχόμενα tweets (κατάστασης) που ταιριάζουν με το φίλτρο είναι αποθηκευμένες σε ένα συνδεδεμένο ουρά αποκλεισμού. (Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">LinkedBlockingQueue κλάσης</a>.) Τέλος, τα στοιχεία είναι τα μηνύματα μεταφέρονται απενεργοποίηση στην ουρά και που εκπέμπει στην τοπολογία.

###<a name="the-hashtagextractor"></a>Το HashtagExtractor

Για να εξαγάγετε κατακερματισμός ετικετών, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> χρησιμοποιείται για την ανάκτηση όλων των ετικετών κατακερματισμός που περιέχονται σε το tweet. Στη συνέχεια, αυτές έχουν αποσταλεί στη ροή.

##<a name="enable-twitter"></a>Ενεργοποίηση Twitter

Χρησιμοποιήστε τα ακόλουθα βήματα για να καταχωρήσετε μια νέα εφαρμογή Twitter και λήψη των καταναλωτών και την πρόσβαση πληροφοριών διακριτικού που απαιτείται για την ανάγνωση από Twitter:

1. Μεταβείτε στο <a href="https://apps.twitter.com" target="_blank">Twitter εφαρμογές</a> και κάντε κλικ στο κουμπί **Δημιουργία νέας εφαρμογής** . Κατά τη συμπλήρωση της φόρμας, αφήστε κενό το πεδίο **Διεύθυνση URL επιστροφής κλήσης** .

2. Όταν δημιουργηθεί η εφαρμογή, κάντε κλικ στην καρτέλα **πλήκτρα και οι κωδικοί πρόσβασης** .

3. Αντιγράψτε τις πληροφορίες **Κλειδί καταναλωτή** και **Μυστικό καταναλωτών** .

4. Στο κάτω μέρος της σελίδας, επιλέξτε **Δημιουργία μου διακριτικό πρόσβασης** , εάν υπάρχει χωρίς διακριτικά. Όταν τα διακριτικά έχουν δημιουργηθεί, αντιγράψτε τις πληροφορίες **Διακριτικό πρόσβασης** και **Μυστικό διακριτικό πρόσβασης** .

5. Στο έργο **TwitterSpoutTopology** προηγουμένως κλωνοποιηθεί, ανοίξτε το αρχείο **resources/twitter4j.properties** , προσθέστε τις πληροφορίες που συλλέγονται στα προηγούμενα βήματα και, στη συνέχεια, αποθηκεύστε το αρχείο.

##<a name="build-the-topology"></a>Δημιουργία της τοπολογίας

Χρησιμοποιήστε τον ακόλουθο κώδικα για να δημιουργήσετε το έργο:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Δοκιμή της τοπολογίας

Χρησιμοποιήστε την ακόλουθη εντολή για να ελέγξετε την τοπολογία τοπικά:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Μετά την εκκίνηση της τοπολογίας, θα πρέπει να βλέπετε πληροφορίες εντοπισμού σφαλμάτων που περιέχει ο κατακερματισμός των ετικετών και απαριθμεί εκπεμπομένου μέσω της τοπολογίας. Το αποτέλεσμα θα πρέπει να είναι παρόμοιο με τα εξής:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δοκιμάσει της τοπολογίας τοπικά, Ανακαλύψτε πώς μπορείτε να αναπτύξετε την τοπολογία: [ανάπτυξη και διαχείριση τοπολογίες καταιγίδας Apache στην HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Μπορεί επίσης να σας ενδιαφέρουν τα παρακάτω θέματα καταιγίδας:

* [Ανάπτυξη τοπολογίες Java για καταιγίδας σε HDInsight με Maven](hdinsight-storm-develop-java-topology.md)

* [Ανάπτυξη C# τοπολογίες για καταιγίδας στη χρήση του Visual Studio HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Για περισσότερα παραδείγματα καταιγίδας για HDinsight:

* [Παράδειγμα τοπολογίες για καταιγίδας στην HDInsight](hdinsight-storm-example-topology.md)

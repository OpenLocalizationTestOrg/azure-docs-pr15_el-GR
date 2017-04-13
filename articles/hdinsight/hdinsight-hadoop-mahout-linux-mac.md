<properties
    pageTitle="Δημιουργία συστάσεις χρησιμοποιώντας Mahout και βάσει Linux HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε το Apache Mahout μηχανικής εκμάθησης βιβλιοθήκης για τη δημιουργία ταινίας συστάσεις με βάσει Linux HDInsight (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Δημιουργία ταινίας συστάσεις, χρησιμοποιώντας το Apache Mahout με βάσει Linux Hadoop στο HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το [Apache Mahout](http://mahout.apache.org) μηχανικής εκμάθησης βιβλιοθήκη με Azure HDInsight για τη δημιουργία ταινίας συστάσεις.

Mahout είναι μια [μηχανικής εκμάθησης] [ ml] βιβλιοθήκη για Apache Hadoop. Mahout περιέχει αλγόριθμους για την επεξεργασία δεδομένων, όπως το φιλτράρισμα, ταξινόμηση, και σύμπλεγμα. Σε αυτό το άρθρο, θα χρησιμοποιήσετε μια μηχανή προτάσεων για τη δημιουργία ταινίας συστάσεις που βασίζονται σε ταινίες είδατε τους φίλους σας.

> [AZURE.NOTE] Τα βήματα σε αυτό το έγγραφο απαιτούν Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight. Για πληροφορίες σχετικά με τη χρήση Mahout με ένα σύμπλεγμα που βασίζεται στα Windows, ανατρέξτε στο θέμα [δημιουργία ταινίας συστάσεις χρησιμοποιώντας Apache Mahout με Hadoop στο HDInsight που βασίζεται σε Windows](hdinsight-mahout.md)

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Hadoop Linux βασίζεται σε σύμπλεγμα HDInsight. Για πληροφορίες σχετικά με τη δημιουργία μία, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με τη χρήση βάσει Linux Hadoop στο HDInsight][getstarted]

##<a name="mahout-versioning"></a>Διαχείριση εκδόσεων mahout

Για περισσότερες πληροφορίες σχετικά με την έκδοση του Mahout περιλαμβάνεται με το σύμπλεγμά σας HDInsight, ανατρέξτε στο θέμα [εκδόσεις HDInsight και Hadoop των στοιχείων](hdinsight-component-versioning.md).

> [AZURE.WARNING] Ενώ είναι δυνατό να αποστείλετε μια διαφορετική έκδοση του Mahout στο σύμπλεγμα HDInsight, μόνο τα στοιχεία που παρέχονται με το σύμπλεγμα HDInsight υποστηρίζονται πλήρως και υποστήριξη της Microsoft θα σας βοηθήσει να απομονώσετε και να επιλύσετε θέματα που σχετίζονται με αυτά τα στοιχεία.
>
> Προσαρμοσμένα στοιχεία λαμβάνουν εμπορικά λογικές υποστήριξη για να σας βοηθήσει να αντιμετωπίσετε περαιτέρω το ζήτημα. Αυτό μπορεί να έχει ως αποτέλεσμα το ζήτημα επίλυση ή που σας ζητά να συμμετάσχουν διαθέσιμα κανάλια για τις τεχνολογίες Άνοιγμα αρχείου προέλευσης όπου βρίσκονται πολλά επίπεδα εξειδίκευση για συγκεκριμένη τεχνολογία. Για παράδειγμα, υπάρχουν πολλές τοποθεσίες Κοινότητας που μπορούν να χρησιμοποιηθούν, όπως: [φόρουμ MSDN για το HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Επίσης, Apache έργα έχουν τοποθεσίες έργου στο [http://apache.org](http://apache.org), για παράδειγμα: [Hadoop](http://hadoop.apache.org/), [τους](http://spark.apache.org/).

##<a name="recommendations"></a>Κατανόηση των συστάσεων

Μία από τις συναρτήσεις που παρέχεται από Mahout είναι ένας μηχανισμός σύσταση. Αυτός ο μηχανισμός αποδέχεται δεδομένα στο πλαίσιο μορφή `userID`, `itemId`, και `prefValue` (τις προτιμήσεις τους χρήστες για το στοιχείο). Mahout, στη συνέχεια, να εκτελέσετε ανάλυση εμφάνιση από κοινού για να προσδιορίσετε: _οι χρήστες που διαθέτουν την προτίμηση για ένα στοιχείο έχουν επίσης μια προτίμηση για αυτά τα άλλα στοιχεία_. Mahout Καθορίζει, στη συνέχεια, οι χρήστες με παρόμοια στοιχείο προτιμήσεις, οι οποίες μπορούν να χρησιμοποιηθούν για να κάνετε συστάσεις.

Ακολουθεί μια πολύ απλό παράδειγμα που χρησιμοποιεί ταινίες:

* __Εμφάνιση από κοινού__: Αλέξανδρος, η Άννα και ο Μπάμπης όλα δηλώσει ότι σας αρέσει _πόλεμοι αστέρι_, _Το αυτοκρατορία απεργίες πίσω_και _επιστροφής από το Jedi_. Mahout Καθορίζει ότι οι χρήστες που όπως επίσης ένα από αυτά τα ταινίες όπως τις άλλες δύο.

* __Εμφάνιση από κοινού__: ο Μπάμπης και η Άννα επίσης δηλώσει ότι σας αρέσει _Η απειλή ανύπαρκτο αντικείμενο_, _επίθεση από τα αντίγραφα_και _Revenge από το Sith_. Mahout Καθορίζει ότι οι χρήστες που τους άρεσε επίσης το προηγούμενο τρεις ταινίες όπως αυτά τα τρία.

* __Ομοιότητα σύσταση__: επειδή Αλέξανδρος αρέσουν τα πρώτα τρία ταινίες, Mahout εξετάζει ταινίες που άλλα άτομα με παρόμοια προτιμήσεις δηλώσει ότι σας αρέσει, αλλά δεν έχει παρακολούθηση Αλέξανδρος (δηλώσει ότι σας αρέσει/χαρακτηρισμός). Σε αυτήν την περίπτωση, Mahout συνιστά _Το απειλή ανύπαρκτο αντικείμενο_, _επίθεση από τα αντίγραφα_και _Revenge από το Sith_.

###<a name="understanding-the-data"></a>Κατανόηση των δεδομένων

Εύκολη, [Έρευνα GroupLens] [ movielens] παρέχει δεδομένα χαρακτηρισμού για ταινίες σε μια μορφή που είναι συμβατή με Mahout. Αυτά τα δεδομένα είναι διαθέσιμη στο το σύμπλεγμά προεπιλεγμένο αποθήκευσης στο `/HdiSamples/HdiSamples/MahoutMovieData`.

Υπάρχουν δύο αρχεία, `moviedb.txt` (πληροφορίες σχετικά με τις ταινίες,) και `user-ratings.txt`. Το αρχείο ratings.txt χρήστη χρησιμοποιείται κατά την ανάλυση, ενώ moviedb.txt χρησιμοποιείται για την παροχή πληροφοριών φιλική προς το χρήστη κειμένου κατά την εμφάνιση των αποτελεσμάτων της ανάλυσης.

Τα δεδομένα που περιέχονται στο χρήστη ratings.txt έχει δομή του `userID`, `movieID`, `userRating`, και `timestamp`, που σας ενημερώνει μας πώς ιδιαίτερα κάθε χρήστης με χαρακτηρισμό μια ταινία. Ακολουθεί ένα παράδειγμα των δεδομένων:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Εκτέλεση ανάλυσης

Χρησιμοποιήστε την ακόλουθη εντολή για να εκτελέσετε την εργασία σύσταση:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Η εργασία ενδέχεται να χρειαστούν αρκετά λεπτά για να ολοκληρώσετε, και ενδέχεται να εκτελέσετε πολλές εργασίες MapReduce.

##<a name="view-the-output"></a>Προβάλετε την έξοδο

1. Μόλις ολοκληρωθεί η εργασία, χρησιμοποιήστε την ακόλουθη εντολή για να δείτε το αποτέλεσμα που δημιουργήθηκε:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Το αποτέλεσμα θα εμφανίζονται ως εξής:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Η πρώτη στήλη είναι η `userID`. Οι τιμές που περιέχονται στο ' [' και ']' είναι `movieId`:`recommendationScore`.

2. Μπορείτε να χρησιμοποιήσετε το αποτέλεσμα, μαζί με το moviedb.txt, για να εμφανίσετε περισσότερες πληροφορίες χρήστη φιλικό. Πρώτα, θα πρέπει να αντιγράψετε τα αρχεία τοπικά χρησιμοποιώντας τις παρακάτω εντολές:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Αυτό θα αντιγράψετε τα δεδομένα εξόδου σε ένα αρχείο με το όνομα του τρέχοντος καταλόγου, μαζί με τα αρχεία δεδομένων ταινίας **recommendations.txt** .

3. Χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε μια νέα δέσμη ενεργειών Python που θα αναζητήσετε ταινίας ονόματα για τα δεδομένα στο αποτέλεσμα συστάσεις:

        nano show_recommendations.py

    Όταν ανοίξει το πρόγραμμα επεξεργασίας, χρησιμοποιήστε τα παρακάτω ανάλογα με τα περιεχόμενα του αρχείου:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Πατήστε το **Συνδυασμό πλήκτρων Ctrl-X**, **Y**και τέλος **Enter** για να αποθηκεύσετε τα δεδομένα.

3. Χρησιμοποιήστε την ακόλουθη εντολή για να κάνετε εκτελέσιμο αρχείο:

        chmod +x show_recommendations.py

4. Εκτελέστε τη δέσμη ενεργειών Python. Η παρακάτω διαδικασία προϋποθέτει που βρίσκονται στον κατάλογο όπου όλα τα αρχεία που έχουν ληφθεί:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Αυτό θα δείτε τις συστάσεις που δημιουργούνται από το χρήστη Αναγνωριστικό 4.

    * Το αρχείο **ratings.txt χρήστη** χρησιμοποιείται για την ανάκτηση ταινίες που ο χρήστης έχει χαρακτηρισμός
    * Το αρχείο **moviedb.txt** χρησιμοποιείται για να ανακτήσετε τα ονόματα των ταινίες
    * Το **recommendations.txt** χρησιμοποιείται για να ανακτήσετε τις συστάσεις ταινίας για αυτόν το χρήστη

    Το αποτέλεσμα από αυτή την εντολή θα είναι παρόμοιο με το εξής:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Διαγραφή προσωρινά δεδομένα

Εργασίες mahout δεν καταργήσει προσωρινά δεδομένα που δημιουργείται κατά την επεξεργασία της εργασίας. Το `--tempDir` παράμετρος καθορίζεται η εργασία παράδειγμα για την απομόνωση τα προσωρινά αρχεία σε μια συγκεκριμένη διαδρομή για εύκολη διαγραφή. Για να καταργήσετε τα προσωρινά αρχεία, χρησιμοποιήστε την ακόλουθη εντολή:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Εάν θέλετε να εκτελέσετε ξανά την εντολή, πρέπει επίσης να διαγράψετε τον κατάλογο αποτελεσμάτων. Χρησιμοποιήστε τα ακόλουθα για να διαγράψετε αυτόν τον κατάλογο:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που μάθατε πώς μπορείτε να χρησιμοποιήσετε Mahout, Ανακαλύψτε άλλοι τρόποι για εργασία με δεδομένα σε HDInsight:

* [Hive με HDInsight](hdinsight-use-hive.md)
* [Γουρούνι με HDInsight](hdinsight-use-pig.md)
* [MapReduce με HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 

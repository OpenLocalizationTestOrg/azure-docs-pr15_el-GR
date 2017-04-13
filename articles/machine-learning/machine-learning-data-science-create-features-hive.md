<properties
    pageTitle="Δημιουργία δυνατοτήτων για τα δεδομένα σε ένα σύμπλεγμα Hadoop χρήση ερωτημάτων Hive | Microsoft Azure"
    description="Παραδείγματα ερωτημάτων Hive που δημιουργούν δυνατότητες σε δεδομένα που είναι αποθηκευμένα σε ένα σύμπλεγμα Azure HDInsight Hadoop."
    services="machine-learning"
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


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Δημιουργία δυνατοτήτων για τα δεδομένα σε ένα σύμπλεγμα Hadoop χρήση ερωτημάτων ομάδας

Αυτό το έγγραφο περιγράφει τον τρόπο δημιουργίας δυνατότητες για δεδομένα που είναι αποθηκευμένα σε ένα σύμπλεγμα Azure HDInsight Hadoop χρήση ερωτημάτων ομάδας. Αυτά τα ερωτήματα Hive Χρησιμοποιήστε ενσωματωμένο Hive που ορίζονται από το συναρτήσεις χρήστη (UDF), τις δέσμες ενεργειών για τις οποίες παρέχονται.

Οι λειτουργίες που απαιτούνται για τη δημιουργία δυνατότητες μπορεί να είναι πολλή μνήμη. Οι επιδόσεις ερωτημάτων ομάδα γίνεται πιο σημαντική σε αυτές τις περιπτώσεις και μπορούν να βελτιωθούν ρυθμίζοντας τις παραμέτρους ορισμένων. Το της ρύθμισης των παραμέτρων αυτών περιγράφεται στην ενότητα τελικό.

Τα ερωτήματα που παρουσιάζονται παραδείγματα είναι ειδικές για τη [Νέα ΥΌΡΚΗ ταξί ταξιδιού δεδομένων](http://chriswhong.com/open-data/foil_nyc_taxi/) σενάρια παρέχονται επίσης στο [αποθετήριο Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Αυτά τα ερωτήματα ήδη σχήμα δεδομένων που καθορίζεται και είστε έτοιμοι να υποβάλλονται για να εκτελέσετε. Στην ενότητα τελικό, περιγράφονται επίσης παράμετροι που οι χρήστες μπορούν να με ακρίβεια, έτσι ώστε να μπορούν να βελτιωθούν οι επιδόσεις ερωτημάτων ομάδας.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Το **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δημιουργίας δυνατοτήτων για τα δεδομένα σε διάφορα περιβάλλοντα. Αυτή η εργασία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε:

* Δημιουργία λογαριασμού Azure χώρου αποθήκευσης. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Προμήθεια ένα προσαρμοσμένο σύμπλεγμα Hadoop με την υπηρεσία HDInsight.  Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Προσαρμογή Azure HDInsight Hadoop συμπλεγμάτων για σύνθετη ανάλυση](machine-learning-data-science-customize-hadoop-cluster.md).
* Τα δεδομένα σε πίνακες ομάδα συμπλεγμάτων Azure HDInsight Hadoop έχει αποσταλεί. Εάν δεν έχει, ακολουθήστε [Δημιουργία και τη φόρτωση δεδομένων σε πίνακες της ομάδας](machine-learning-data-science-move-hive-tables.md) για την αποστολή δεδομένων σε πίνακες Hive πρώτα.
* Με δυνατότητα απομακρυσμένης πρόσβασης στο σύμπλεγμα. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [πρόσβαση κεφαλή κόμβο του Hadoop συμπλέγματος](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Δυνατότητα δημιουργίας

Σε αυτήν την ενότητα, περιγράφονται μερικά παραδείγματα σχετικά με τους τρόπους με την οποία μπορούν να επιχειρήσεις δυνατότητες χρήση ερωτημάτων ομάδας. Αφού δημιουργήσετε πρόσθετες δυνατότητες, να προσθέσετε ως στήλες στον υπάρχοντα πίνακα ή να δημιουργήσετε έναν νέο πίνακα με τις πρόσθετες δυνατότητες και το πρωτεύον κλειδί, το οποίο, στη συνέχεια, μπορεί να συνδεθεί με τον αρχικό πίνακα. Ακολουθούν παραδείγματα παρουσιάζεται:

1. [Συχνότητα με βάση τη δυνατότητα γενιάς](#hive-frequencyfeature)
2. [Κίνδυνοι έλαβε μεταβλητές σε δυαδικό ταξινόμηση](#hive-riskfeature)
3. [Εξαγάγετε δυνατότητες από το πεδίο ημερομηνίας/ώρας](#hive-datefeatures)
4. [Εξαγάγετε δυνατότητες από πεδίο κειμένου](#hive-textfeatures)
5. [Υπολογισμός απόσταση μεταξύ συντεταγμένων GPS](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Συχνότητα με βάση τη δυνατότητα γενιάς

Συχνά είναι χρήσιμο για τον υπολογισμό της συχνότητας των επιπέδων των κατηγοριών μεταβλητή ή της συχνότητας από ορισμένες συνδυασμούς επίπεδα από πολλές μεταβλητές κατηγοριών. Οι χρήστες να χρησιμοποιήσετε την ακόλουθη δέσμη ενεργειών για να υπολογίσετε τα παρακάτω συχνότητας:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Κίνδυνοι έλαβε μεταβλητές σε δυαδικό ταξινόμηση

Σε δυαδικό ταξινόμηση, πρέπει να μετατρέψετε μεταβλητές έλαβε μη αριθμητικό σε αριθμητική δυνατότητες όταν τα μοντέλα που χρησιμοποιείται μόνο να αριθμητική δυνατότητες. Αυτό γίνεται, αντικαθιστώντας κάθε επίπεδο μη αριθμητικό με αριθμητικές κινδύνου. Σε αυτήν την ενότητα, θα σας δείξουμε ορισμένα γενικής χρήσης Hive ερωτήματα που τον υπολογισμό τιμών κινδύνου (πιθανότητα να log) μιας μεταβλητής κατηγοριών.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Σε αυτό το παράδειγμα, μεταβλητές `smooth_param1` και `smooth_param2` έχουν οριστεί ομαλά τις τιμές κινδύνου υπολογισμού από τα δεδομένα. Κίνδυνοι έχουν μιας περιοχής μεταξύ -πληροφορίες και πληροφορίες. Κίνδυνοι > 0 υποδεικνύει ότι είναι μεγαλύτερο του 0,5 την πιθανότητα ότι ο προορισμός είναι ίσο με 1.

Μετά από τον κίνδυνο πίνακα υπολογίζεται, οι χρήστες μπορεί να εκχωρήσει κινδύνου τιμές σε έναν πίνακα με τη συμμετοχή σε με τον πίνακα κινδύνου. Το ερώτημα Ένωσης Hive ήταν που παρέχεται στην προηγούμενη ενότητα.

###<a name="hive-datefeatures"></a>Εξαγάγετε δυνατότητες από πεδία ημερομηνίας και ώρας

Ομάδα συνοδεύεται από ένα σύνολο UDF για την επεξεργασία πεδία ημερομηνίας και ώρας. Στην ομάδα, η προεπιλεγμένη μορφή ημερομηνίας/ώρας είναι ' εεεε-ηη 00:00:00 "('1970-01-01 12:21:32 ' για παράδειγμα). Σε αυτήν την ενότητα, θα σας δείξουμε παραδείγματα που εξάγουν την ημέρα του μήνα, το μήνα από ένα πεδίο ημερομηνίας/ώρας και άλλα παραδείγματα μετατροπή μιας συμβολοσειράς ημερομηνίας/ώρας σε μορφή εκτός από την προεπιλεγμένη μορφή ημερομηνίας/ώρας συμβολοσειρά με την προεπιλεγμένη μορφοποίηση.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Αυτό το ερώτημα Hive θεωρείται ότι το *& #60; πεδίου ημερομηνίας/ώρας >* είναι με την προεπιλεγμένη μορφή ημερομηνίας/ώρας.

Εάν ένα πεδίο ημερομηνίας/ώρας δεν είναι με την προεπιλεγμένη μορφή, πρέπει να μετατρέψετε πρώτα το πεδίο datetime σε Unix χρονικής σήμανσης και, στη συνέχεια, μετατροπή της Unix χρονικής σήμανσης σε μια συμβολοσειρά ημερομηνίας/ώρας που είναι με την προεπιλεγμένη μορφή. Όταν το datetime με την προεπιλεγμένη μορφή, οι χρήστες να εφαρμόσετε το ενσωματωμένο datetime UDF για να εξαγάγετε δυνατότητες.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Σε αυτό το ερώτημα, εάν η *& #60; πεδίου ημερομηνίας/ώρας >* περιλαμβάνει το μοτίβο όπως *26/03/2015 12:04:39*, το *' & #60; μοτίβο του πεδίου ημερομηνίας/ώρας >'* πρέπει να είναι `'MM/dd/yyyy HH:mm:ss'`. Για να το δοκιμάσετε, οι χρήστες μπορούν να εκτελούν

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Το *hivesampletable* σε αυτό το ερώτημα προέρχεται προεγκατεστημένων στην όλα Azure HDInsight Hadoop συμπλεγμάτων από προεπιλογή όταν έχουν παρασχεθεί των συμπλεγμάτων.


###<a name="hive-textfeatures"></a>Εξαγάγετε δυνατότητες από πεδία κειμένου

Όταν ο πίνακας Hive έχει ένα πεδίο κειμένου που περιέχει μια συμβολοσειρά με λέξεις που είναι διαχωρισμένες με κενά διαστήματα, το ακόλουθο ερώτημα εξάγει το μήκος της συμβολοσειράς και τον αριθμό των λέξεων στη συμβολοσειρά.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Υπολογισμός αποστάσεις μεταξύ των συνόλων συντεταγμένων GPS

Το ερώτημα που δίνεται σε αυτήν την ενότητα μπορούν να εφαρμοστούν απευθείας στα δεδομένα ταξιδιού ταξί νέα ΥΌΡΚΗ. Ο σκοπός της αυτό το ερώτημα είναι να δείχνουν πώς μπορείτε να εφαρμόσετε ένα ενσωματωμένο μαθηματικές συναρτήσεις στην ομάδα για τη δημιουργία δυνατότητες.

Τα πεδία που χρησιμοποιούνται σε αυτό το ερώτημα είναι τις συντεταγμένες GPS απάντηση κλήσης από και dropoff θέσεις, με το όνομα *Απάντηση κλήσης από την\_γεωγραφικού*, *Απάντηση κλήσης από την\_γεωγραφικό πλάτος*, *dropoff\_γεωγραφικού*, και *dropoff\_γεωγραφικό πλάτος*. Τα ερωτήματα που υπολογίζουν την άμεση απόσταση μεταξύ τις συντεταγμένες απάντηση κλήσης από και dropoff είναι:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Το μαθηματικές εξισώσεις που υπολογίζουν την απόσταση μεταξύ των δύο συντεταγμένων GPS μπορείτε να βρείτε στην τοποθεσία του <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Τύπου κινητό δέσμες ενεργειών</a> , δημιουργήθηκε από Peter Lapisu. Στην προσωπική Javascript, η συνάρτηση `toRad()` είναι απλώς *lat_or_lon*π/180*, που μετατρέπει μοίρες σε ακτίνια. Εδώ, *lat_or_lon * είναι το γεωγραφικό πλάτος ή το μήκος. Επειδή η ομάδα δεν παρέχει τη συνάρτηση `atan2`, αλλά παρέχει τη συνάρτηση `atan`, το `atan2` συνάρτηση έχει υλοποιηθεί με `atan` συνάρτηση στο παραπάνω ερώτημα ομάδας με τον ορισμό που παρέχεται στην <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Δημιουργία χώρου εργασίας](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Μια πλήρη λίστα των Hive ενσωματωμένου UDF μπορείτε να βρείτε στην ενότητα **Ενσωματωμένες συναρτήσεις** στο <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">wiki Apache Hive</a>).  

## <a name="tuning"></a>Θέματα για προχωρημένους: συντονισμού Hive παραμέτρους βελτίωση ταχύτητας ερωτήματος

Οι προεπιλεγμένες ρυθμίσεις παραμέτρων του συμπλέγματος Hive ενδέχεται να μην είναι κατάλληλο για τα ερωτήματα ομάδας και τα δεδομένα που επεξεργάζονται τα ερωτήματα. Σε αυτήν την ενότητα, αναλύονται ορισμένες παράμετροι που με ακρίβεια οι χρήστες μπορούν να βελτιώσετε την απόδοση Hive ερωτημάτων. Οι χρήστες πρέπει να προσθέσετε την παράμετρο της ρύθμισης ερωτημάτων πριν από τα ερωτήματα επεξεργασίας των δεδομένων.

1. **Κενό διάστημα σωρού Java**: για ερωτήματα που αφορούν τη συμμετοχή σε μεγάλα σύνολα δεδομένων ή επεξεργασία μεγάλων εγγραφών, **επαρκεί ο χώρος σωρού** είναι ένα από τα κοινών σφαλμάτων. Αυτό μπορεί να είναι ρυθμισμένες με τη ρύθμιση παραμέτρων *mapreduce.map.java.opts* και *mapreduce.task.io.sort.mb* στις τιμές που θέλετε. Ακολουθεί ένα παράδειγμα:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Αυτή η παράμετρος εκχωρεί μνήμη 4GB χώρου σωρού Java και επίσης κάνει ταξινόμηση πιο αποτελεσματική εκχωρώντας περισσότερη μνήμη για αυτήν. Είναι καλή ιδέα να αναπαραγάγετε με αυτές τις αναθέσεις, εάν υπάρχουν οποιαδήποτε εργασία αποτυχία σφαλμάτων που σχετίζονται με σωρού χώρο.

2. **Μέγεθος μπλοκ DFS** : Αυτή η παράμετρος ορίζει τη μικρότερη μονάδα δεδομένων που αποθηκεύει το σύστημα αρχείων. Ως παράδειγμα, εάν το μέγεθος του μπλοκ DFS είναι 128MB, στη συνέχεια, δεδομένα του μεγέθους μικρότερο από και προς τα επάνω για να 128MB είναι αποθηκευμένο σε ένα μεμονωμένο μπλοκ, ενώ τα δεδομένα που είναι μεγαλύτερο από 128MB εκχωρείται επιπλέον μπλοκ. Επιλέγοντας ένα μπλοκ πολύ μικρό μέγεθος προκαλεί μεγάλο διαφάνειες στο Hadoop, επειδή ο κόμβος όνομα έχει για την επεξεργασία πολλές περισσότερες αιτήσεις για να βρείτε το σχετικό μπλοκ που αφορούν το αρχείο. Μια προτεινόμενη ρύθμιση όταν στον με gigabyte (ή μεγαλύτερο) τα δεδομένα είναι:

        set dfs.block.size=128m;

3. **Βελτιστοποίηση συμμετοχή σε λειτουργία στην ομάδα** : ενώ κοινών διαδικασιών στο πλαίσιο χάρτη/μείωση συνήθως πραγματοποιείται κατά τη φάση μείωση, ορισμένες φορές, μπορεί να είναι επίτευξη τεράστιο κέρδη κατά τον προγραμματισμό συνδέσμους στη φάση χάρτη (ονομάζεται επίσης "mapjoins"). Για να κατευθύνετε Hive να το κάνετε αυτό, όταν είναι δυνατόν, θα σας μπορεί να ορίσει:

        set hive.auto.convert.join=true;

4. **Καθορίζει τον αριθμό των mappers ομάδα** : ενώ Hadoop επιτρέπει στο χρήστη για να ορίσετε τον αριθμό των σμίκρυνσης, τον αριθμό των mappers είναι συνήθως να μην έχει ρυθμιστεί από το χρήστη. Ένα μυστικό που επιτρέπει σε κάποιο βαθμό του στοιχείου ελέγχου σε αυτόν τον αριθμό που είναι να επιλέξτε μεταβλητές Hadoop, *mapred.min.split.size* και *mapred.max.split.size* ως το μέγεθος του κάθε εργασία χάρτη προσδιορίζεται από:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Συνήθως, η προεπιλεγμένη τιμή της *mapred.min.split.size* είναι 0, του *mapred.max.split.size* είναι **Long.MAX** και του *dfs.block.size* είναι 64 MB. Καθώς μπορούμε να δούμε, δίνεται το μέγεθος των δεδομένων, αυτές οι παράμετροι από "ρύθμιση" της ρύθμισης τους μας επιτρέπει να με ακρίβεια τον αριθμό των mappers που χρησιμοποιείται.

5. Μερικές άλλες περισσότερες **Επιλογές για προχωρημένους** για βελτιστοποίηση των επιδόσεων Hive αναφέρονται παρακάτω. Αυτές σας επιτρέπει να ορίσετε τη μνήμη έχει εκχωρηθεί για να αντιστοιχίσετε και να μειώσετε εργασίες και μπορεί να είναι χρήσιμη για την προσαρμογή επιδόσεων. Λάβετε υπόψη ότι η *mapreduce.reduce.memory.mb* δεν μπορεί να είναι μεγαλύτερη από το μέγεθος φυσικής μνήμης του κάθε εργαζόμενου κόμβο του συμπλέγματος Hadoop.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
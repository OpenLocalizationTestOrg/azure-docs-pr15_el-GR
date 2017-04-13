<properties
    pageTitle="Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση HDInsight Hadoop συμπλεγμάτων σε του συνόλου δεδομένων Criteo 1 TB | Microsoft Azure"
    description="Χρησιμοποιώντας τη διαδικασία Science δεδομένων ομάδας για ένα σενάριο για ολοκληρωμένες χρήση ένα σύμπλεγμα HDInsight Hadoop για τη δημιουργία και ανάπτυξη του μοντέλου με ένα μεγάλο σύνολο δεδομένων διαθέσιμη στο κοινό (1 TB)"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Η διαδικασία Science δεδομένων ομάδας στην πράξη - χρήση του Azure HDInsight Hadoop συμπλεγμάτων σε ένα σύνολο δεδομένων 1 TB

Σε αυτόν τον οδηγό, θα δείξουμε χρησιμοποιώντας τη διαδικασία ομάδας δεδομένων φυσικής σε ένα σενάριο για ολοκληρωμένες με ένα [σύμπλεγμα Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) για την αποθήκευση, εξερευνήστε, η δυνατότητα μηχανικό, και κάτω δείγμα δεδομένων από ένα από τα σύνολα δεδομένων διαθέσιμη στο κοινό [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) . Χρησιμοποιούμε Azure μηχανικής εκμάθησης για να δημιουργήσετε ένα μοντέλο δυαδικό ταξινόμηση σε αυτά τα δεδομένα. Θα σας δείξουμε επίσης πώς μπορείτε να δημοσιεύσετε ένα από αυτά τα μοντέλα ως υπηρεσία Web.

Επίσης, είναι δυνατή η χρήση ενός σημειωματαρίου IPython για την ολοκλήρωση των εργασιών που παρουσιάζονται σε αυτές τις οδηγίες. Οι χρήστες που θέλετε να δοκιμάσετε αυτήν την προσέγγιση πρέπει να συμβουλευτείτε το θέμα [αναλυτικές οδηγίες Criteo χρησιμοποιώντας μια σύνδεση Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Περιγραφή Criteo συνόλου δεδομένων

Το Criteo δεδομένων είναι ένα σύνολο δεδομένων πρόβλεψη κάντε κλικ στην επιλογή που είναι 370GB περίπου TSV gzip συμπίεση αρχείων (~1.3TB χωρίς συμπίεση), που περιλαμβάνει περισσότερες από 4.3 δισεκατομμύριο καρτέλες. Λαμβάνεται από 24 ημέρες της κάντε κλικ στο κουμπί δεδομένα που διατίθενται από [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Για τη διευκόλυνση της επιστήμονες δεδομένων, θα σας έχουν αποσυμπιεσμένο αρχείο διαθέσιμα μας για να πειραματιστείτε με δεδομένα.

Κάθε εγγραφή σε αυτό το σύνολο δεδομένων περιέχει 40 στήλες:

- η πρώτη στήλη είναι μια στήλη "ετικέτα" που υποδεικνύει εάν ένας χρήστης κάνει κλικ σε μια **Προσθήκη** (τιμή 1) ή δεν κάντε κλικ σε μία (τιμή 0)
- Οι αριθμητικές τιμές, στη συνέχεια 13 στήλες και
- τελευταία 26 είναι έλαβε στήλες

Οι στήλες είναι anonymized και χρησιμοποιήστε μια σειρά από αριθμημένα ονόματα: "Στ1" (για τη στήλη ετικέτας) για να ' Col40 "(για την τελευταία στήλη κατηγοριών).            

Εδώ είναι ένα απόσπασμα από τις πρώτες 20 στήλες δύο παρατηρήσεων (γραμμές) από το σύνολο δεδομένων:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Υπάρχουν τιμές που λείπουν σε τα αριθμητικά και έλαβε στήλες σε αυτό το σύνολο δεδομένων. Περιγραφή μια απλή μέθοδο για το χειρισμό τις τιμές που λείπουν. Πρόσθετες λεπτομέρειες για τα δεδομένα είναι εξερευνήσει όταν τις αποθηκεύει σε ομάδα πίνακες.

**Ορισμού:** *Κλικ μετάβασης rate (ΠΡΌΤ):* Αυτό είναι το ποσοστό του κλικ στα δεδομένα. Σε αυτό το dataset Criteo, το ΠΡΌΤ είναι περίπου 3.3% ή 0.033.

## <a name="mltasks"></a>Παραδείγματα εργασιών πρόβλεψης
Δύο προβλήματα πρόβλεψης δείγμα απευθύνονται σε αυτές τις οδηγίες:

1. **Δυαδικό ταξινόμηση**: προβλέπει εάν ένας χρήστης κάνει κλικ σε μια προσθήκη:
    - Κλάση 0: Δεν υπάρχει κλικ
    - Τάξη 1: κάντε κλικ στην επιλογή

2. **Παλινδρόμησης**: προβλέπει την πιθανότητα ένα κλικ ad από τις δυνατότητες του χρήστη.


## <a name="setup"></a>Ορισμός του μια Hadoop του HDInsight σύμπλεγμα για δεδομένα επιστήμης

**Σημείωση:** Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Ρύθμιση του περιβάλλοντος του Azure Science δεδομένων για τη δημιουργία λύσεων πρόβλεψης ανάλυσης με HDInsight συμπλεγμάτων σε τρία βήματα:

1. [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md): αυτόν το λογαριασμό χώρου αποθήκευσης χρησιμοποιείται για την αποθήκευση δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob Azure. Τα δεδομένα που χρησιμοποιούνται σε HDInsight συμπλεγμάτων είναι αποθηκευμένο εδώ.

2. [Προσαρμογή Azure HDInsight Hadoop συμπλεγμάτων για Science δεδομένων](machine-learning-data-science-customize-hadoop-cluster.md): αυτό το βήμα δημιουργεί ένα σύμπλεγμα Azure HDInsight Hadoop με 64-bit Anaconda Python 2.7 εγκατασταθεί σε όλους τους κόμβους. Υπάρχουν δύο σημαντικά βήματα (που περιγράφεται σε αυτό το θέμα) για να ολοκληρώσετε κατά την προσαρμογή του συμπλέγματος HDInsight.

    * Πρέπει να συνδέσετε το λογαριασμό χώρου αποθήκευσης που δημιουργήσατε στο βήμα 1 με το σύμπλεγμά σας HDInsight κατά τη δημιουργία. Αυτόν το λογαριασμό χώρου αποθήκευσης χρησιμοποιείται για την πρόσβαση σε δεδομένα που είναι δυνατή η επεξεργασία μέσα στο σύμπλεγμα.

    * Πρέπει να ενεργοποιήσετε απομακρυσμένη πρόσβαση στον κεφαλής κόμβο του συμπλέγματος, αφού δημιουργηθεί. Να θυμάστε τα διαπιστευτήρια απομακρυσμένης πρόσβασης που καθορίζετε εδώ (διαφορετικές από εκείνες που καθορίζονται για το σύμπλεγμα κατά τη δημιουργία της): πρέπει να ολοκληρώσετε τις παρακάτω διαδικασίες.

3. [Δημιουργία ενός χώρου εργασίας Azure ML](machine-learning-create-workspace.md): αυτή Azure μηχανικής εκμάθησης χώρου εργασίας χρησιμοποιείται για τη δημιουργία μηχανικής εκμάθησης μοντέλων μετά από ένα αρχικό δεδομένων Εξερεύνηση και προς τα κάτω δειγματοληψία στο σύμπλεγμα HDInsight.

## <a name="getdata"></a>Λήψη και χρήση δεδομένων από μια δημόσια προέλευση

Το σύνολο δεδομένων [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) είναι δυνατή η πρόσβαση κάνοντας κλικ στη σύνδεση, αποδεχτείτε τους όρους χρήσης και, παρέχοντας ένα όνομα. Ένα στιγμιότυπο του αυτή μοιάζει εμφανίζεται εδώ:

![Αποδεχτείτε τους όρους Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Κάντε κλικ στο κουμπί **συνέχεια για να λήψης** για να διαβάσετε περισσότερα σχετικά με το σύνολο δεδομένων και τη διαθεσιμότητά της.

Τα δεδομένα βρίσκονται σε μια δημόσια θέση [Azure χώρο αποθήκευσης blob](../storage/storage-dotnet-how-to-use-blobs.md) : wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. Το "wasb" αναφέρεται στη θέση αποθήκευσης αντικειμένων Blob Azure. 

1. Τα δεδομένα σε αυτό το χώρο αποθήκευσης blob δημόσια αποτελείται από τρεις υποφακέλους αποσυμπιεσμένο αρχείο δεδομένων.

    1. Τον υποφάκελο *ανεπεξέργαστα/count/* περιέχει τις πρώτες 21 ημέρες δεδομένων - από ημέρα\_00 ημέρα\_20
    2. Τον υποφάκελο *ανεπεξέργαστα/τρένο/* αποτελείται από μία ημέρα δεδομένων, ημέρα\_21
    3. Τον υποφάκελο *ανεπεξέργαστα/έλεγχο/* αποτελείται από δύο ημέρες δεδομένων, ημέρα\_22 και ημέρα\_23

2. Για τα άτομα που θέλετε να ξεκινήσετε με τα ανεπεξέργαστα gzip δεδομένα, αυτές είναι επίσης διαθέσιμες στο φάκελο κύριο *ανεπεξέργαστο /* ως day_NN.gz, όπου NN μεταβαίνει από 00 έως 23.

Μια εναλλακτική προσέγγιση να αποκτήσει πρόσβαση, να εξερευνήσετε και μοντέλο αυτών των δεδομένων που δεν απαιτεί καμία λήψη του τοπικού επεξηγούνται αργότερα σε αυτές τις οδηγίες όταν δημιουργούμε ομάδα πίνακες.

## <a name="login"></a>Συνδεθείτε με το headnode συμπλέγματος

Για να συνδεθείτε με το headnode του συμπλέγματος, χρησιμοποιήστε την [πύλη του Azure](https://ms.portal.azure.com) για να εντοπίσετε το σύμπλεγμα. Κάντε κλικ στο εικονίδιο ελέφαντα HDInsight στα αριστερά και, στη συνέχεια, κάντε διπλό κλικ στο όνομα του συμπλέγματος. Μεταβείτε στη σελίδα " **Ρύθμιση παραμέτρων** ", κάντε διπλό κλικ στο εικονίδιο ΣΎΝΔΕΣΗ στο κάτω μέρος της σελίδας και εισαγάγετε τα διαπιστευτήριά σας απομακρυσμένης πρόσβασης όταν σας ζητηθεί. Αυτό μεταφέρει το headnode του συμπλέγματος.

Θα δείτε τι μοιάζει ένα τυπικό πρώτο αρχείο καταγραφής στο για να το headnode σύμπλεγμα:

![Συνδεθείτε στο σύμπλεγμα](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Στα αριστερά, θα δούμε "Hadoop γραμμή εντολών", που είναι ισχυρός μηχανισμός που κρύβεται μας για την Εξερεύνηση δεδομένων. Θα δούμε επίσης δύο χρήσιμες URL - "Hadoop νήματα κατάστασης" και "Όνομα κόμβου Hadoop". Η διεύθυνση URL νήματα κατάστασης εμφανίζει την πρόοδο εργασίας και τη διεύθυνση URL κόμβο όνομα παρέχει λεπτομερείς πληροφορίες για τη ρύθμιση παραμέτρων του συμπλέγματος.

Τώρα, θα σας έχουν ρυθμιστεί και είστε έτοιμοι για να ξεκινήσετε το πρώτο τμήμα του το αναλυτικές οδηγίες: Εξερεύνηση δεδομένων με χρήση της ομάδας και τη λειτουργία δεδομένων έτοιμη για Azure μηχανικής εκμάθησης.

## <a name="hive-db-tables"></a>Δημιουργία βάσης δεδομένων της ομάδας και πινάκων

Για να δημιουργήσετε πίνακες Hive για το σύνολο δεδομένων Criteo, ανοίξτε τη ***γραμμή εντολών Hadoop*** στην επιφάνεια εργασίας του κεφαλής κόμβου και εισαγάγετε τον κατάλογο Hive, πληκτρολογώντας την εντολή

    cd %hive_home%\bin

>[AZURE.NOTE] Εκτελέστε όλες οι εντολές της ομάδας σε αυτές τις οδηγίες από τον Κάδο Hive / εντολών του καταλόγου. Αυτό αναλαμβάνει προβλήματα διαδρομή αυτόματα. Χρησιμοποιούμε τους όρους "Hive καταλόγου ερώτηση", "Hive Ανακύκλωσης / εντολών του καταλόγου", και "γραμμή εντολών Hadoop" χωρίς διάκριση.

>[AZURE.NOTE]  Για να εκτελέσετε οποιοδήποτε ερώτημα Hive, μία πάντα να χρησιμοποιήσετε τις παρακάτω εντολές:

        cd %hive_home%\bin
        hive

Μετά την Αντικατάσταση Hive εμφανίζεται με μια "hive >"Είσοδος, απλώς αποκοπή και επικόλληση το ερώτημα για να το εκτελέσει.

Ο ακόλουθος κώδικας δημιουργεί μια βάση δεδομένων "criteo" και, στη συνέχεια, 4 πίνακες:


* έναν *πίνακα για τη δημιουργία καταμετρά* ενσωματωμένο στην ημέρα ημέρες\_00 ημέρα\_20,
* έναν *πίνακα για χρήση ως του συνόλου δεδομένων τρένο* ενσωματωμένο στην ημέρα\_21, και
* δύο *πίνακες για χρήση ως τα σύνολα δεδομένων δοκιμής* ενσωματωμένη ημέρα\_22 και ημέρα\_23 αντίστοιχα.

Θα σας διαιρέστε το σύνολο δεδομένων δοκιμής σε δύο διαφορετικούς πίνακες επειδή μια από τις ημέρες είναι αργία και θέλουμε να προσδιορίσετε εάν το μοντέλο να εντοπίσετε τις διαφορές μεταξύ των διακοπών και μη αργιών από το επιτόκιο κλικ μετάβασης.

Η δέσμη ενεργειών [δείγμα & #95; hive & 95 #, Δημιουργία & #95; criteo & 95 #, 95 #, & βάσης δεδομένων και & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) εμφανίζεται εδώ για τη διευκόλυνσή σας:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

Θα σας Σημειώστε ότι όλες αυτές οι πίνακες είναι εξωτερική όπως θα σας απλώς τοποθετήστε το δείκτη σε θέσεις αποθήκευσης αντικειμένων Blob του Azure (wasb).

**Υπάρχουν δύο τρόποι για την εκτέλεση ερωτήματος ΟΠΟΙΑΔΉΠΟΤΕ Hive που θα σας αναφέρουν τώρα.**

1. **Χρησιμοποιώντας τη γραμμή εντολών Hive σε**: είναι το πρώτο για την έκδοση μια εντολή "hive" και αντιγραφή και επικόλληση ενός ερωτήματος στο το γραμμής εντολών σε ομάδα. Για να το κάνετε αυτό, κάντε:

        cd %hive_home%\bin
        hive

    Τώρα στο γραμμής εντολών σε, αποκοπή και επικόλληση το ερώτημα εκτελείται το.

2. **Αποθήκευση ερωτημάτων σε ένα αρχείο και εκτέλεση της εντολής**: το δεύτερο είναι για να αποθηκεύσετε τα ερωτήματα σε ένα αρχείο .hql ([δείγμα & #95; hive & 95 #, Δημιουργία & #95; criteo & #95; 95 #, & βάσης δεδομένων και & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) και, στη συνέχεια, εισαγάγετε την ακόλουθη εντολή για να εκτελέσετε το ερώτημα:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Επιβεβαίωση για τη δημιουργία βάσης δεδομένων και πίνακα

Στη συνέχεια, επιβεβαιώνουμε τη δημιουργία της βάσης δεδομένων με την ακόλουθη εντολή από τον Κάδο Hive / εντολών του καταλόγου:

        hive -e "show databases;"

Το αποτέλεσμα είναι:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Αυτό επιβεβαιώνει τη δημιουργία νέας βάσης δεδομένων, "criteo".

Για να δείτε ποιοι πίνακες που δημιουργήσαμε, θα σας απλώς εκδώσετε την εντολή από τον Κάδο Hive / εντολών του καταλόγου:

        hive -e "show tables in criteo;"

Στη συνέχεια, εμφανίζεται το εξής αποτέλεσμα:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Εξερεύνηση δεδομένων σε ομάδα

Τώρα θα σας είναι έτοιμοι να κάνετε ορισμένες Εξερεύνηση βασικών δεδομένων στην ομάδα. Θα σας ξεκινήστε από την καταμέτρηση του αριθμού των παραδείγματα το τρένο και να ελέγξετε τους πίνακες δεδομένων.

### <a name="number-of-train-examples"></a>Αριθμός παραδείγματα τρένο

Τα περιεχόμενα του [δείγματος & #95; hive & #95; πλήθος & #95; τρένο & #95; πίνακα & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) εμφανίζονται εδώ:

        SELECT COUNT(*) FROM criteo.criteo_train;

Αυτό παράγει:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Εναλλακτικά, μία μπορεί επίσης να εκδίδει την ακόλουθη εντολή από τον Κάδο Hive / εντολών του καταλόγου:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Αριθμός παραδείγματα δοκιμής σε τα δύο σύνολα δεδομένων δοκιμής

Θα σας τώρα να μετρήσετε τον αριθμό των παραδείγματα τα δύο σύνολα δεδομένων δοκιμής. Τα περιεχόμενα του [δείγμα & #95, hive & #95, πλήθος & #95; criteo & #95; δοκιμής & #95, ημέρα & #95, 22 & #95; πίνακα & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) εδώ:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Αυτό παράγει:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Όπως συνήθως, ενδέχεται να επίσης ονομάζουμε τη δέσμη ενεργειών από τον Κάδο Hive / καταλόγου προτροπή εκδίδει με την εντολή:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Τέλος, θα σας να εξετάσετε το πλήθος των παραδείγματα δοκιμή του συνόλου δεδομένων δοκιμής που βασίζονται σε ημέρα\_23.

Η εντολή για να το κάνετε αυτό είναι παρόμοιο με αυτό που εμφανίζεται ακριβώς (ανατρέξτε στις [δείγμα & #95; hive & #95; πλήθος & #95; criteo & #95; δοκιμής & #95; ημέρα #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Το αποτέλεσμα είναι:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Ετικέτα διανομής στο dataset τρένο

Η κατανομή ετικέτας στο τρένο dataset είναι που σας ενδιαφέρουν. Για να δείτε αυτό, θα σας δείξουμε περιεχόμενα του [δείγματος & #95; hive & #95; criteo & #95; & #95 ετικέτας, διανομής & #95; τρένο & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Αυτό αποδίδει την κατανομή ετικέτα:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Σημειώστε ότι το ποσοστό της θετική ετικέτες είναι περίπου 3.3% (συνεπείς με το αρχικό σύνολο δεδομένων).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Κατανομή ιστόγραμμα ορισμένες αριθμητικές μεταβλητές σε του συνόλου δεδομένων τρένο

Μπορούμε να χρησιμοποιήσουμε εγγενής της ομάδας "Ιστόγραμμα\_αριθμητική" συνάρτηση για να μάθετε πώς φαίνεται η κατανομή των αριθμητικών μεταβλητών. Εδώ θα βρείτε τα περιεχόμενα του [δείγματος & #95; hive & #95, criteo & #95; ιστόγραμμα & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Αυτό παράγει τα εξής:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

ΠΡΟΒΟΛΉ - ΕΓΚΆΡΣΙΩΝ μεγέθυνση συνδυασμό στην ομάδα χρησιμεύει για να δημιουργήσετε ένα αποτέλεσμα μοιάζει με SQL αντί για τη συνήθη λίστα. Σημειώστε ότι σε αυτό τον πίνακα, στην πρώτη στήλη αντιστοιχεί κέντρο του Κάδου και το δεύτερο στη συχνότητα Ανακύκλωσης.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Κατά προσέγγιση εκατοστημόριου ορισμένες αριθμητικές μεταβλητών στο dataset τρένο

Που σας ενδιαφέρουν με αριθμητικές μεταβλητές είναι επίσης τον υπολογισμό της εκατοστημόριου κατά προσέγγιση. Η ομάδα του εγγενούς "εκατοστημόριο\_περίπου" το κάνει αυτό για μας. Τα περιεχόμενα του [δείγμα & #95; hive & #95, criteo & #95, κατά προσέγγιση & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) είναι:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Αυτό παράγει:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Θα σας Σημειώστε ότι η κατανομή των εκατοστημόριου σχετίζεται με τη διανομή ιστόγραμμα οποιαδήποτε αριθμητική μεταβλητή συνήθως.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Βρείτε τον αριθμό των μοναδικών τιμών για ορισμένες έλαβε στήλες στο dataset τρένο

Συνεχίσετε την Εξερεύνηση δεδομένων, θα σας τώρα να βρείτε, για ορισμένα έλαβε στήλες, τον αριθμό των μοναδικών τιμών που λαμβάνουν. Για να το κάνετε αυτό, θα σας δείξουμε περιεχομένων [δείγμα & #95; hive & #95, criteo & #95; μοναδικό & #95, τιμές & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Αυτό παράγει:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Θα σας Σημειώστε ότι Col15 έχει μοναδικές τιμές 19M! Χρήση naïve τεχνικές όπως "πρόσβασης μίας κωδικοποίηση" για την κωδικοποίηση όπως υψηλό διαστάσεων έλαβε μεταβλητές είναι αδύνατη. Συγκεκριμένα, Προσπαθούμε να εξηγήσετε και δείχνουν μια τεχνική ισχυρών, ισχυρό που ονομάζεται [Εκμάθησης με καταμετρά](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) για την αντιμετώπιση των αυτό το πρόβλημα αποτελεσματικά.

Θα σας τερματισμός αυτής της ενότητας δευτερεύουσες, εξετάζοντας τον αριθμό των μοναδικών τιμών για ορισμένες άλλες έλαβε στήλες. Τα περιεχόμενα του [δείγμα & #95; hive & #95, criteo & #95; μοναδικό & #95; τιμές & #95; πολλαπλάσιο & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) είναι:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Αυτό παράγει:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Ξανά, θα δούμε ότι με εξαίρεση Col20, όλες τις άλλες στήλες έχουν πολλές μοναδικές τιμές.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Εμφάνιση από κοινού καταμετρά από ζεύγη έλαβε μεταβλητές σε του συνόλου δεδομένων τρένο

Το πλήθος από κοινού εμφάνισης ζεύγη των κατηγοριών μεταβλητών είναι επίσης που σας ενδιαφέρουν. Αυτό μπορεί να προσδιοριστεί χρησιμοποιώντας τον κώδικα στο [δείγμα & #95; hive & #95, criteo & #95; ζεύγη & #95; έλαβε & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Χρησιμοποιούμε αντίστροφη σειρά τις μετρήσεις με την παρουσία και αναζήτηση στο επάνω μέρος 15 σε αυτήν την περίπτωση. Αυτό σας δίνει μας:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Προς τα κάτω δείγμα τα σύνολα δεδομένων για το Azure μηχανικής εκμάθησης

Έχοντας εξερευνήσει τα σύνολα δεδομένων και επιδεικνύεται πώς μπορεί να κάνουμε αυτόν τον τύπο Εξερεύνηση για οποιεσδήποτε μεταβλητές (συμπεριλαμβανομένων των συνδυασμών), θα σας τώρα προς τα κάτω δείγμα τα σύνολα δεδομένων ώστε να μπορέσουμε να δημιουργήσουμε μοντέλα σε Azure μηχανικής εκμάθησης. Ανάκληση που είναι το πρόβλημα θα σας εστίαση σε: θα σας δώσει ένα σύνολο χαρακτηριστικών παράδειγμα (δυνατότητα τιμές από Στηλ2 - Col40), πρόβλεψης εάν Στ1 είναι 0 (δεν υπάρχει κλικ) ή 1 (κάντε κλικ).

Να προς τα κάτω δείγμα μας τρένο και να ελέγξετε συνόλων δεδομένων με το 1% του αρχικού μεγέθους, χρησιμοποιούμε εγγενούς συνάρτησης RAND() της ομάδας. Η επόμενη δέσμη ενεργειών, [δείγμα & #95; hive & #95; criteo & #95; μείωση μεγέθους & #95; τρένο & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) το κάνει αυτό για το σύνολο δεδομένων τρένο:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Αυτό παράγει:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Το δέσμη ενεργειών [δείγμα & #95; hive & #95; criteo & #95; μείωση μεγέθους & #95; δοκιμής & #95, ημέρα & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) σημαίνει το για τα δεδομένα δοκιμής, ημέρα\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Αυτό παράγει:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Τέλος, η δέσμη ενεργειών [δείγμα & #95; hive & #95; criteo & #95; μείωση μεγέθους & #95; δοκιμής & #95, ημέρα & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) σημαίνει το για τα δεδομένα δοκιμής, ημέρα\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Αυτό παράγει:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Με αυτό, θα σας είναι έτοιμοι να χρησιμοποιήσετε μας κάτω δείγματος τρένο και να ελέγξετε σύνολα δεδομένων για τη δημιουργία μοντέλων στο Azure μηχανικής εκμάθησης.

Υπάρχει ένα ολοκληρωμένο στοιχείο σημαντικές πριν μας προχωρήσω στην εκμάθησης υπολογιστή Azure, τα οποία είναι ανησυχίες στον πίνακα count. Στην επόμενη ενότητα δευτερεύουσα, αναλύονται τα εξής με ορισμένες λεπτομέρειες.

##<a name="count"></a>Μια σύντομη συζήτηση στον πίνακα "count"

Που θα σας είδατε, πολλές μεταβλητές έλαβε διαθέτετε ένα πολύ υψηλή διαστατικότητα. Σε μας οδηγίες, θα σας παρουσίαση μια ισχυρή τεχνική που ονομάζεται [Εκμάθησης με καταμετρά](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) για την κωδικοποίηση αυτές τις μεταβλητές σε μια αποτελεσματική και ισχυρό τρόπο. Περισσότερες πληροφορίες σχετικά με αυτήν την τεχνική είναι σε αυτήν τη σύνδεση που παρέχεται.

**Σημείωση:** Σε αυτές τις οδηγίες, εστίαση σχετικά με τη χρήση πινάκων count για την παραγωγή συμπαγής παραστάσεις υψηλής διαστάσεων έλαβε δυνατότητες. Δεν είναι ο μόνος τρόπος να κωδικοποιήσετε έλαβε δυνατοτήτων. Για περισσότερες πληροφορίες σχετικά με άλλες τεχνικές, ενδιαφερόμενοι χρήστες μπορεί να δείτε [ένα ζεστού-κωδικοποίησης](http://en.wikipedia.org/wiki/One-hot) και [τη δυνατότητα κλειδώματος](http://en.wikipedia.org/wiki/Feature_hashing).

Για να δημιουργήσετε πίνακες καταμέτρηση στα δεδομένα πλήθος, χρησιμοποιούμε τα δεδομένα στην φάκελο ανεπεξέργαστα/καταμέτρηση. Στην ενότητα μοντελοποίηση, θα σας δείξουμε στους χρήστες πώς να δημιουργείτε σε αυτούς τους πίνακες count για έλαβε δυνατότητες από την αρχή, ή εναλλακτικά για να χρησιμοποιήσετε έναν πίνακα προ-δομημένες count για τους explorations. Σε τι ακολουθεί, όταν αναφέρονται σε "Πλήθος προ-δομημένες πίνακες", θα σας σημαίνει τους πίνακες πλήθος που παρέχουμε. Λεπτομερείς οδηγίες σχετικά με τον τρόπο για να αποκτήσετε πρόσβαση σε αυτούς τους πίνακες παρατίθενται στην επόμενη ενότητα.

## <a name="aml"></a>Δημιουργήστε ένα μοντέλο με Azure μηχανικής εκμάθησης

Το μοντέλο διαδικασίας σε Azure μηχανικής εκμάθησης δημιουργίας ακολουθεί τα εξής βήματα:

1. [Μεταφέρετε τα δεδομένα από πίνακες Hive στο Azure μηχανικής εκμάθησης](#step1)
2. [Δημιουργήστε την έρευνα: καθαρίσετε τα δεδομένα, επιλέξτε μαθαίνω και featurize με πίνακες count](#step2)
3. [Εκπαίδευση του μοντέλου](#step3)
4. [Βαθμολογία του μοντέλου σε δεδομένα δοκιμής](#step4)
5. [Αξιολόγηση του μοντέλου](#step5)
6. [Δημοσίευση του μοντέλου ως μια υπηρεσία web να καταναλωθεί](#step6)

Τώρα θα σας είναι έτοιμοι να δημιουργήσετε μοντέλα στο studio Azure μηχανικής εκμάθησης. Μας κάτω δείγματος δεδομένων αποθηκεύεται ως ομάδα πίνακες στο σύμπλεγμα. Χρησιμοποιούμε τη λειτουργική μονάδα Azure μηχανικής εκμάθησης **Εισαγωγή δεδομένων** για να διαβάσετε αυτά τα δεδομένα. Τα διαπιστευτήρια για να αποκτήσετε πρόσβαση στο λογαριασμό χώρου αποθήκευσης αυτού του συμπλέγματος παρέχονται στο αυτό που ακολουθεί.

### <a name="step1"></a>Βήμα 1: Λήψη δεδομένων από ομάδα πίνακες στο Azure μηχανικής εκμάθησης με χρήση της λειτουργικής μονάδας εισαγωγή δεδομένων και επιλέξτε την για μια μηχανικής εκμάθησης έρευνας

Ξεκινήστε επιλέγοντας ένα **+ ΔΗΜΙΟΥΡΓΊΑ** -> **ΈΡΕΥΝΑΣ** -> **Κενό έρευνας**. Στη συνέχεια, από το πλαίσιο **αναζήτησης** στην επάνω αριστερή πλευρά, πραγματοποιήστε αναζήτηση για "Εισαγωγή δεδομένων". Σύρετε και αποθέστε τη λειτουργική μονάδα **Εισαγωγή δεδομένων** με τον καμβά έρευνας (το μεσαίο τμήμα της οθόνης) για να χρησιμοποιήσετε τη λειτουργική μονάδα για πρόσβαση σε δεδομένα.

Αυτή είναι η εμφάνιση την **Εισαγωγή δεδομένων** κατά τη λήψη δεδομένων από τον πίνακα ομάδα:

![Εισαγωγή δεδομένων λαμβάνει τα δεδομένα](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

Για τη λειτουργική μονάδα **Εισαγωγή δεδομένων** , οι τιμές των παραμέτρων που παρέχονται στο γραφικό είναι απλώς παραδείγματα την ταξινόμηση των τιμών πρέπει να παρέχετε. Παρακάτω θα δείτε ορισμένες γενικές οδηγίες σχετικά με τον τρόπο για τη συμπλήρωση της παραμέτρου για τη λειτουργική μονάδα **Εισαγωγή δεδομένων** .

1. Επιλέξτε "Hive ερώτημα" για το **Αρχείο προέλευσης δεδομένων**
2. Στο παράθυρο διαλόγου **Hive ερώτημα βάσης δεδομένων** , μια απλή, ΕΠΙΛΈΞΤΕ * από < σας\_βάσης δεδομένων\_name.your\_πίνακα\_όνομα >-είναι αρκετά.
3. **URI διακομιστή Hcatalog**: Εάν το σύμπλεγμά σας είναι "abc" και, στη συνέχεια, αυτό είναι απλώς: https://abc.azurehdinsight.net
4. **Όνομα λογαριασμού χρήστη Hadoop**: το όνομα χρήστη που έχει επιλέξει τη στιγμή της θέσης σε λειτουργία του συμπλέγματος. (Δεν το όνομα χρήστη απομακρυσμένης πρόσβασης!)
5. **Κωδικός πρόσβασης λογαριασμού χρήστη Hadoop**: Ο κωδικός πρόσβασης για το όνομα χρήστη που έχει επιλέξει τη στιγμή της θέσης σε λειτουργία του συμπλέγματος. (Δεν τον κωδικό απομακρυσμένης πρόσβασης!)
6. **Θέση των δεδομένων εξόδου**: Επιλέξτε "Azure"
7. **Όνομα λογαριασμού Azure χώρου αποθήκευσης**: το λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα
8. **Κλειδί λογαριασμού Azure αποθήκευσης**: το κλειδί του λογαριασμού χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.
9. **Το όνομα του Azure κοντέινερ**: Εάν το όνομα του συμπλέγματος είναι "abc" και, στη συνέχεια, αυτό οφείλεται συνήθως απλώς "ΑΒΓ,".


Μόλις ολοκληρωθεί η **Εισαγωγή δεδομένων** γρήγορα δεδομένα (δείτε την πράσινη υποδιαίρεσης σε τη λειτουργική μονάδα), αποθηκεύστε αυτά τα δεδομένα ως ένα σύνολο δεδομένων (με το όνομα της επιλογής σας). Έτσι μοιάζει:

![Εισαγωγή δεδομένων αποθήκευση δεδομένων](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Κάντε δεξί κλικ στη θύρα εξόδου της λειτουργικής μονάδας **Εισαγωγή δεδομένων** . Αυτό αποκαλύπτει μια επιλογή **Αποθήκευση ως σύνολο δεδομένων** και μια επιλογή **αναπαράσταση** . Η επιλογή **αναπαράσταση** , εάν πατήσατε, εμφανίζει 100 γραμμές των δεδομένων, μαζί με ένα πλαίσιο δεξιά που είναι χρήσιμη για ορισμένες σύνοψης στατιστικά στοιχεία. Για να αποθηκεύσετε τα δεδομένα, απλώς επιλέξτε **Αποθήκευση ως σύνολο δεδομένων** και ακολουθήστε τις οδηγίες.

Για να επιλέξετε το αποθηκευμένο σύνολο δεδομένων για χρήση σε μια έρευνα μηχανικής εκμάθησης, εντοπίστε τα σύνολα δεδομένων χρησιμοποιώντας το πλαίσιο **αναζήτησης** που φαίνεται στην παρακάτω εικόνα. Στη συνέχεια, απλώς πληκτρολογήστε το όνομα που σας έδωσε το σύνολο δεδομένων μερικώς για να έχετε πρόσβαση σε αυτά και σύρετε το κύριο παράθυρο του συνόλου δεδομένων. Απόθεση σε το κύριο παράθυρο την επιλογή για χρήση σε μοντελοποίηση μηχανικής εκμάθησης.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Κάντε τα εξής για το τρένο και τα σύνολα δεδομένων δοκιμής. Επίσης, να θυμάστε να χρησιμοποιήσετε το όνομα της βάσης δεδομένων και ονόματα πινάκων που έχετε δώσει για το σκοπό. Οι τιμές που χρησιμοποιούνται στην εικόνα είναι μόνο για απεικόνιση purposes.* *

### <a name="step2"></a>Βήμα 2: Δημιουργία μιας απλής έρευνας στο Azure μηχανικής εκμάθησης πρόβλεψη κλικ / δεν υπάρχει κλικ

Μας έρευνας Azure ML μοιάζει κάπως έτσι:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

Λοιπόν τώρα εξετάσουμε τα βασικά στοιχεία της έρευνας σε αυτό. Ως υπενθύμιση, πρέπει να σύρετε το αποθηκευμένο τρένο και δοκιμάστε πρώτα τα σύνολα δεδομένων στο μας καμβά έρευνας.

#### <a name="clean-missing-data"></a>Καθαρίσετε τα δεδομένα που λείπουν

Τη λειτουργική μονάδα **Clean δεδομένα λείπουν** κάνει τι υποδηλώνει το όνομά του: το Εκκαθαρίζει δεδομένα που λείπουν με τρόπους που μπορούν να καθοριστεί από το χρήστη. Αναζήτηση σε αυτήν τη λειτουργική μονάδα, θα σας δείτε αυτό:

![Εκκαθάριση δεδομένα που λείπουν](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Εδώ, επιλέγουμε για να αντικαταστήσετε όλες τις τιμές που λείπουν με 0. Υπάρχουν καθώς και άλλες επιλογές που είναι ορατό, εξετάζοντας αναπτυσσόμενη λίστα στη λειτουργική μονάδα.

#### <a name="feature-engineering-on-the-data"></a>Η δυνατότητα μηχανικής στα δεδομένα

Μπορεί να υπάρχει εκατομμύρια μοναδικές τιμές για ορισμένες δυνατότητες έλαβε του μεγάλων συνόλων δεδομένων. Χρήση naïve μεθόδους όπως πρόσβασης μίας κωδικοποίηση για την απεικόνιση αυτές τις δυνατότητες υψηλής διαστάσεων έλαβε είναι εντελώς αδύνατη. Σε αυτές τις οδηγίες, θα σας δείχνουν πώς μπορείτε να χρησιμοποιήσετε δυνατότητες count χρησιμοποιώντας ενσωματωμένο λειτουργικές μονάδες Azure μηχανικής εκμάθησης για τη δημιουργία συμπαγής παραστάσεις αυτών των διαστάσεων υψηλής έλαβε μεταβλητών. Το τελικό αποτέλεσμα είναι μικρότερο μέγεθος μοντέλο, ταχύτερους χρόνους εκπαίδευση και μετρικών απόδοσης που είναι αρκετά συγκρίσιμη με εκείνη χρησιμοποιώντας άλλες τεχνικές.

##### <a name="building-counting-transforms"></a>Καταμέτρηση μετασχηματισμούς δόμησης

Για να δημιουργήσετε πλήθος δυνατοτήτων, χρησιμοποιούμε τη λειτουργική μονάδα **Δόμηση μετρώντας μετασχηματισμός** , το που είναι διαθέσιμες στο Azure μηχανικής εκμάθησης. Η λειτουργική μονάδα μοιάζει κάπως έτσι:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Σημαντική σημείωση** : στο πλαίσιο **Πλήθος στηλών** , θα σας, εισαγάγετε τις στήλες που Ευχόμαστε οι εκτελούν μετρήσεις σε. Συνήθως, αυτές είναι (όπως αναφέρθηκε) υψηλής διαστάσεων έλαβε στήλες. Κατά την έναρξη, αναφέρθηκε ότι το dataset Criteo έχει 26 έλαβε στήλες: από Col15 σε Col40. Εδώ, θα σας μέτρηση όλα τα παραπάνω και δώστε τους δείκτες (από 15 σε 40 διαχωρισμένες με κόμμα, όπως φαίνεται).

Για να χρησιμοποιήσετε τη λειτουργική μονάδα σε κατάσταση λειτουργίας MapReduce (κατάλληλη για μεγάλα σύνολα δεδομένων), χρειαζόμαστε πρόσβαση σε ένα σύμπλεγμα HDInsight Hadoop (αυτόν που χρησιμοποιήθηκε για τη δυνατότητα Εξερεύνηση μπορεί να χρησιμοποιηθεί ξανά για αυτό το σκοπό) και τα διαπιστευτήρια. Τα στοιχεία του προηγούμενου απεικόνιση τι τις συμπληρωμένα τιμές φαίνονται (αντικαταστήστε τις τιμές που παρέχονται για απεικόνιση με τις σχετικές για τη δική σας περίπτωση χρήσης).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Στην παραπάνω εικόνα, θα σας δείξουμε πώς μπορείτε να εισαγάγετε τη θέση εισαγωγής blob. Αυτή η θέση περιέχει τα δεδομένα δεσμευμένες για τη δημιουργία πινάκων καταμέτρηση.


Αφού ολοκληρωθεί η εκτέλεση του αυτήν τη λειτουργική μονάδα, μπορούμε να αποθηκεύσουμε το μετασχηματισμό για αργότερα, κάνοντας δεξί κλικ στη λειτουργική μονάδα και επιλέγοντας την επιλογή **Αποθήκευση ως μετασχηματισμός** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

Στην μας αρχιτεκτονική έρευνας φαίνεται παραπάνω, του συνόλου δεδομένων "ytransform2" αντιστοιχούν ακριβώς μετασχηματισμό αποθηκευμένο count. Για το υπόλοιπο της αυτό έρευνας, υποθέσουμε ότι που χρησιμοποιούνται **Δόμηση μετρώντας μετασχηματισμός** λειτουργικής μονάδας σε ορισμένα δεδομένα για τη δημιουργία καταμετρά του προγράμματος ανάγνωσης και, στη συνέχεια, να χρησιμοποιήσετε αυτές τις μετρήσεις να δημιουργούν πλήθος δυνατοτήτων στις το τρένο και να ελέγξετε συνόλων δεδομένων.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Επιλογή Τι πλήθος δυνατοτήτων για να συμπεριλάβετε ως μέρος του τρένο και δοκιμής συνόλων δεδομένων

Μόλις έχουμε μια καταμέτρηση μετασχηματισμός είστε έτοιμοι, ο χρήστης να επιλέξετε ποιες δυνατότητες να συμπεριλάβετε στο τρένο τους και να ελέγξετε συνόλων δεδομένων με χρήση της λειτουργικής μονάδας **Τροποποίηση παραμέτρων πίνακα Count** . Θα σας δείξουμε απλώς αυτήν τη λειτουργική μονάδα εδώ για λόγους πληρότητας, αλλά στο ενδιαφερόντων απλότητας δεν στην πραγματικότητα χρησιμοποιήσετε στο μας έρευνας.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

Σε αυτήν την περίπτωση, καθώς μπορούν να προβληθούν, θα σας έχετε επιλέξει να χρησιμοποιήσετε μόνο την πιθανότητα καταγραφής και να παραβλέψετε πίσω απενεργοποίηση στήλης. Θα σας, επίσης, να ρυθμίσετε τις παραμέτρους όπως το όριο κάδου απορριμμάτων, πόσες ψευδο-εκ των προτέρων παραδείγματα για να προσθέσετε για εξομάλυνση και, εάν θέλετε να χρησιμοποιήσετε οποιαδήποτε θορύβου Laplacian ή όχι. Όλα αυτά είναι προηγμένες δυνατότητες και είναι να σημειωθεί ότι οι προεπιλεγμένες τιμές είναι ένα καλό σημείο εκκίνησης για τους χρήστες που έχουν προστεθεί σε αυτόν τον τύπο της δυνατότητας γενιάς.

##### <a name="data-transformation-before-generating-the-count-features"></a>Μετασχηματισμό δεδομένων πριν την δημιουργία των δυνατοτήτων count

Τώρα θα σας εστίαση σε ένα σημαντικό σημείο σχετικά με τη μεταμόρφωση μας τρένο και έλεγχος δεδομένων πριν από τη δημιουργία στην πραγματικότητα πλήθος δυνατοτήτων. Σημειώστε ότι υπάρχουν δύο λειτουργικές μονάδες **Εκτέλεση δέσμης ενεργειών R** προτού μας εφαρμογή του μετασχηματισμού καταμέτρηση σε δεδομένα μας.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Εδώ είναι η πρώτη δέσμη ενεργειών R:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


Σε αυτήν τη δέσμη ενεργειών R, θα σας να μετονομάσετε μας στηλών σε ονόματα "Col1" για να "Col40". Αυτό συμβαίνει επειδή το μετασχηματισμό count αναμένει ονόματα αυτής της μορφής.

Στη δεύτερη δέσμη ενεργειών R, θα σας υπόλοιπο την κατανομή μεταξύ θετικές και αρνητικές κλάσεων (κλάδους 1 και 0 αντίστοιχα) με μείωση ανάλυσης αρνητική τάξη. Δείτε εδώ τη δέσμη ενεργειών R δείχνει πώς μπορείτε να το κάνετε αυτό:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Σε αυτό απλή δέσμη ενεργειών R, μπορούμε να χρησιμοποιήσουμε "θέση\_neg\_αναλογία" για να ορίσετε το ποσό του υπολοίπου μεταξύ τους θετικούς και τους αρνητικούς κλάδους. Αυτό είναι σημαντικό να το κάνετε από τη βελτίωση της τάξης δυσαναλογία συνήθως έχει πλεονεκτήματα επιδόσεων για προβλήματα ταξινόμηση πού βρίσκεται η κατανομή κλάσης κλίση (ανάκλησης ότι στην περίπτωσή μας, έχουμε θετικό κλάσης 3.3% και αρνητικές κλάσης 96.7%).

##### <a name="applying-the-count-transformation-on-our-data"></a>Εφαρμογή μετασχηματισμού του πλήθους σε δεδομένα μας

Τέλος, μπορούμε να χρησιμοποιήσουμε τη λειτουργική μονάδα **Εφαρμογή μετασχηματισμού** να εφαρμόσετε οι μετασχηματισμοί καταμέτρηση σε μας τρένο και να ελέγξετε συνόλων δεδομένων. Η ενότητα αυτή λαμβάνει του μετασχηματισμού αποθηκευμένο count ως μία είσοδο και τα σύνολα δεδομένων τρένο ή δοκιμή ως άλλα δεδομένα εισόδου και επιστρέφει τα δεδομένα με δυνατότητες count. Εμφανίζεται εδώ:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Ένα απόσπασμα ποιες δυνατότητες count μοιάζει με

Είναι οδηγιών για να δείτε τι είναι οι δυνατότητες καταμέτρηση στην περίπτωσή μας. Εδώ θα σας δείξουμε απόσπασμα του αυτό:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

Σε αυτό το απόσπασμα, θα σας δείξουμε ότι για τις στήλες που θα σας μέτρηση στην, θα σας γρήγορα τις μετρήσεις και συνδεθείτε πιθανότητα εκτός από τις σχετικές backoffs.

Συνεχίζουμε να τώρα έτοιμοι να δημιουργήσετε ένα μοντέλο Azure μηχανικής εκμάθησης με αυτά τα δεδομένα που έχουν μετατραπεί συνόλων δεδομένων. Στην επόμενη ενότητα, θα σας δείξουμε πώς αυτό μπορεί να γίνει.

#### <a name="azure-machine-learning-model-building"></a>Azure δόμησης μοντέλο μηχανικής εκμάθησης

##### <a name="choice-of-learner"></a>Επιλογή της μαθαίνω

Πρώτα, θα πρέπει να επιλέξετε μια μαθαίνω. Θα χρησιμοποιήσετε δέντρου αποφάσεων δύο κλάσης ενισχύεται ως μας μαθαίνω. Εδώ θα βρείτε τις προεπιλεγμένες επιλογές για αυτό μαθαίνω:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Για την έρευνα, θα επιλέξετε τις προεπιλεγμένες τιμές. Θα σας Σημειώστε ότι οι προεπιλογές είναι συνήθως χαρακτηριστικό και ένας καλός τρόπος για να λάβετε γρήγορα γραμμές βάσης στις επιδόσεις. Μπορείτε να βελτιώσετε στις επιδόσεις με σάρωση παράμετροι Εάν επιλέξετε να όταν έχετε μια γραμμή βάσης.

#### <a name="train-the-model"></a>Εκπαίδευση του μοντέλου

Για την εκπαίδευση, θα σας απλώς να καλέσετε μια λειτουργική μονάδα **Μοντέλο τρένο** . Τα δύο εισόδων σε αυτό είναι το μαθαίνω δέντρου αποφάσεων ενισχύεται δύο κατηγορίας και το σύνολο δεδομένων τρένο. Αυτό εμφανίζεται εδώ:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Βαθμολογία στο μοντέλο

Μόλις έχουμε μοντέλου εκπαιδευμένο, Ζητούμε έτοιμο για τη βαθμολογία στην του συνόλου δεδομένων δοκιμή και να αξιολογήσετε τις επιδόσεις. Αυτό το κάνουμε με χρήση της λειτουργικής μονάδας **Μοντέλο βαθμολογία** φαίνεται στην παρακάτω εικόνα, μαζί με μια λειτουργική μονάδα **Αξιολόγηση μοντέλου** :

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Βήμα 5: Υπολογισμός του μοντέλου

Τέλος, θα σας θα θέλατε να ανάλυση της απόδοσης του μοντέλου. Συνήθως, για δύο προβλήματα (δυαδικό) ταξινόμηση τάξης, μια καλή μέτρηση είναι το AUC. Για την απεικόνιση αυτό, θα σας να συνδέσετε τη λειτουργική μονάδα **Βαθμολογία μοντέλο** σε μια λειτουργική μονάδα **Αξιολόγηση μοντέλου** για αυτό. Κάνοντας κλικ στην επιλογή **αναπαράσταση** σε τη λειτουργική μονάδα **Αξιολόγηση μοντέλο** παράγει ένα γραφικό όπως το εξής:

![Αξιολόγηση μοντέλου BDT λειτουργική μονάδα](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Σε δυαδικό αριθμό (ή δύο κλάσης) προβλήματα με την ταξινόμηση, καλή μέτρηση της ακρίβειας πρόβλεψη είναι η περιοχή στην περιοχή καμπύλης (AUC). Σε αυτό που ακολουθεί, θα σας δείξουμε μας αποτελέσματα χρησιμοποιώντας αυτό το μοντέλο σε μας dataset δοκιμής. Για να λάβετε αυτό, κάντε δεξί κλικ στη θύρα εξόδου της λειτουργικής μονάδας **Αξιολόγηση μοντέλο** και, στη συνέχεια, **αναπαράσταση**.

![Λειτουργική μονάδα αξιολόγηση μοντέλου απεικόνιση](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Βήμα 6: Δημοσίευση στο μοντέλο ως μια υπηρεσία Web
Η δυνατότητα να δημοσιεύσετε ένα μοντέλο Azure μηχανικής εκμάθησης ως υπηρεσίες web με τουλάχιστον fuss είναι ένα πολύτιμο δυνατότητα για την πραγματοποίηση ευρέως διαθέσιμη. Μόλις γίνει αυτό, οποιοσδήποτε να πραγματοποιήσετε κλήσεις στην υπηρεσία web με δεδομένα εισόδου που χρειάζονται προβλέψεων για και, η υπηρεσία web χρησιμοποιεί το μοντέλο για να λάβετε αυτές τις προβλέψεων.

Για να το κάνετε αυτό, θα σας πρώτα αποθηκεύστε μας εκπαιδευμένο μοντέλο ως αντικείμενο εκπαιδευμένο μοντέλο. Αυτό γίνεται κάνοντας δεξί κλικ στη λειτουργική μονάδα **Τρένο μοντέλο** και χρησιμοποιώντας την επιλογή **Αποθήκευση ως εκπαιδευμένο μοντέλο** .

Στη συνέχεια, πρέπει να δημιουργήσετε εισόδου και εξόδου θύρες για την υπηρεσία web μας:

* μια θύρα εισόδου χρειάζεται δεδομένα με την ίδια μορφή με τα δεδομένα που χρειαζόμαστε προβλέψεων για
* μια θύρα εξόδου επιστρέφει τις ετικέτες έχει βαθμολογηθεί και τις σχετικές πιθανότητες.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Επιλέξτε μερικές γραμμές δεδομένων για τη θύρα εισαγωγής

Είναι εύκολο να χρησιμοποιήσετε μια λειτουργική μονάδα **Εφαρμογή μετασχηματισμού SQL** για να επιλέξετε μόνο 10 γραμμές για να λειτουργήσει ως τα δεδομένα θύρα εισόδου. Επιλέξτε μόνο αυτές τις γραμμές δεδομένων για τη θύρα εισόδου με χρήση του ερωτήματος SQL που φαίνεται εδώ:

![Θύρα εισόδου δεδομένων](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Υπηρεσία Web
Τώρα θα σας είναι έτοιμες για να εκτελέσετε ένα μικρό έρευνας που μπορούν να χρησιμοποιηθούν για να δημοσιεύσετε την υπηρεσία web.

#### <a name="generate-input-data-for-webservice"></a>Δημιουργία εισαγωγής δεδομένων για webservice

Ως βήμα zeroth, επειδή ο πίνακας πλήθος είναι μεγάλο, θα σας διαρκέσει μερικές γραμμές δεδομένων δοκιμής και δημιουργία δεδομένα εξόδου από αυτά με τις δυνατότητες count. Αυτό μπορεί να χρησιμοποιηθεί ως τη μορφή δεδομένων εισόδου για μας webservice. Αυτό εμφανίζεται εδώ:

![Δημιουργία BDT εισαγωγής δεδομένων](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Για τη μορφή δεδομένων εισόδου, χρησιμοποιούμε τώρα το ΑΠΟΤΈΛΕΣΜΑ της λειτουργικής μονάδας **Count Featurizer** . Μόλις ολοκληρωθεί η εκτέλεση του αυτό έρευνας, αποθηκεύσετε την έξοδο από τη λειτουργική μονάδα **Featurizer πλήθος** ως ένα σύνολο δεδομένων. Σε αυτό το σύνολο δεδομένων χρησιμοποιείται για τα δεδομένα εισόδου στο το webservice.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Βαθμολογίας έρευνας για webservice δημοσίευσης

Πρώτα, θα σας δείξουμε πώς αυτό θα φαίνεται. Η βασική δομή είναι μια λειτουργική μονάδα **Μοντέλο βαθμολογία** που δέχεται μας εκπαιδευμένο μοντέλο αντικειμένου και μερικές γραμμές δεδομένων εισαγωγής που θα σας δημιούργησε στα προηγούμενα βήματα με χρήση της λειτουργικής μονάδας **Count Featurizer** . Χρησιμοποιούμε "Επιλογή στηλών στο σύνολο δεδομένων" για να έργου τις ετικέτες Scored και τις πιθανότητες βαθμολογία.

![Επιλέξτε τις στήλες στο σύνολο δεδομένων](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Παρατηρήστε πώς μπορούν να χρησιμοποιηθούν τη λειτουργική μονάδα **Επιλογή στηλών στο σύνολο δεδομένων** για 'με φιλτράρισμα' δεδομένων από ένα σύνολο δεδομένων. Θα σας δείξουμε τα περιεχόμενα εδώ:

![Φιλτράρισμα με τις στήλες επιλέξτε στη λειτουργική μονάδα συνόλου δεδομένων](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Για να λάβετε το μπλε εισόδου και εξόδου θύρες, μπορείτε απλώς κάντε κλικ στην επιλογή **Προετοιμασία webservice** στο κάτω δεξιό τμήμα. Εκτέλεση έρευνας σε αυτό σας επιτρέπει επίσης να δημοσιεύσετε την υπηρεσία web: κάντε κλικ στο εικονίδιο **ΔΗΜΟΣΊΕΥΣΗ ΥΠΗΡΕΣΊΑΣ WEB** στην κάτω δεξιά, φαίνεται εδώ:

![Δημοσίευση υπηρεσίας Web](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Μόλις το webservice δημοσιεύεται, θα σας να ανακατευθύνεται σε μια σελίδα που εμφανίζεται, επομένως:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Βλέπουμε δύο συνδέσεις για webservices στην αριστερή πλευρά:

* **Τα/ΑΠΌΚΡΙΣΗ ΑΊΤΗΣΗΣ** εξυπηρέτησης (ή στο) προορίζεται για μία μόνο προβλέψεων και θα σας χρησιμοποιούν σε αυτό το εργαστήριο.
* Η υπηρεσία **ΕΚΤΈΛΕΣΗΣ ΔΈΣΜΗΣ** (BES) χρησιμοποιείται για μαζική προβλέψεων και απαιτεί ότι τα δεδομένα εισόδου που χρησιμοποιούνται για τη δημιουργία προβλέψεων που βρίσκονται στο χώρο αποθήκευσης αντικειμένων Blob Azure.

Κάνοντας κλικ στη σύνδεση **Αίτηση/ΑΠΆΝΤΗΣΗ** μας μεταφέρει σε μια σελίδα που θα σας δώσει μας προ-Κονσερβοποιημένα κωδικός στο C# python και R. Αυτός ο κωδικός μπορεί να χρησιμοποιηθεί εύκολα για την πραγματοποίηση κλήσεων για να το webservice. Σημειώστε ότι το κλειδί API σε αυτήν τη σελίδα πρέπει να χρησιμοποιηθεί για τον έλεγχο ταυτότητας.

Είναι εύκολο να αντιγράψετε αυτόν τον κωδικό python πάνω από ένα νέο κελί στο Σημειωματάριο IPython.

Εδώ θα σας δείξουμε ένα τμήμα του κώδικα python με το σωστό κλειδί API.

![Κωδικός Python](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Σημειώστε ότι θα σας αντικατασταθεί κλειδί API του προεπιλεγμένου με αριθμό-κλειδί API μας webservices. Κάνοντας κλικ στην επιλογή **Εκτέλεση** σε αυτό το κελί σε ένα σημειωματάριο IPython αποδόσεις την ακόλουθη απόκριση:

![Απόκριση IPython](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Θα δούμε ότι για τα δύο παραδείγματα δοκιμής που θα σας ζητηθεί για (στο πλαίσιο JSON της δέσμης ενεργειών python), λαμβάνουμε ξανά απαντήσεις στη φόρμα "Scored ετικετών, πιθανοτήτων Scored". Σημειώστε ότι σε αυτήν την περίπτωση, επιλέγουμε το προεπιλεγμένο τις τιμές που ο κώδικας προ-Κονσερβοποιημένα παρέχει (0 για όλες τις αριθμητικές στήλες και τη συμβολοσειρά "τιμή" για όλες τις στήλες κατηγοριών).

Έτσι ολοκληρώνεται μας αναλυτικές οδηγίες για να ολοκληρωμένες που δείχνει τον τρόπο χειρισμού ευρείας κλίμακας σύνολο δεδομένων με χρήση Azure μηχανικής εκμάθησης. Θα σας αποτελέσματα με μια terabyte δεδομένων συνταχθεί μοντέλου πρόβλεψη και να αναπτυχθεί το ως υπηρεσία web στο cloud.

<properties
    pageTitle="Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση Hadoop συμπλεγμάτων | Microsoft Azure"
    description="Χρησιμοποιώντας τη διαδικασία Science δεδομένων ομάδας για ένα σενάριο για ολοκληρωμένες χρήση ένα σύμπλεγμα HDInsight Hadoop για τη δημιουργία και ανάπτυξη του μοντέλου με μια διαθέσιμη στο κοινό σύνολο δεδομένων."
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση HDInsight Hadoop συμπλεγμάτων

Σε αυτές τις οδηγίες, χρησιμοποιούμε τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](data-science-process-overview.md) σε ένα σενάριο για ολοκληρωμένες χρησιμοποιώντας ένα [σύμπλεγμα Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) , για να αποθηκεύσετε, να εξερευνήσετε και να δυνατοτήτων μηχανικό δεδομένα από τη διαθέσιμη στο κοινό σύνολο δεδομένων [Ταξίδια ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) και προς τα κάτω δείγμα των δεδομένων. Τα μοντέλα των δεδομένων δημιουργούνται με Azure μηχανικής εκμάθησης για το χειρισμό δυαδικό και multiclass ταξινόμηση και παλινδρόμησης πρόβλεψης εργασίες.

Για αναλυτικές οδηγίες που δείχνει τον τρόπο χειρισμού των ένα μεγαλύτερο σύνολο δεδομένων (1 terabyte) για ένα σενάριο παρόμοια με χρήση του HDInsight Hadoop συμπλεγμάτων για επεξεργασία δεδομένων, ανατρέξτε στο θέμα [Διαδικασία Science δεδομένων ομάδας - χρήση του Azure HDInsight Hadoop συμπλεγμάτων σε ένα σύνολο δεδομένων 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md).

Επίσης, είναι δυνατή η χρήση ενός σημειωματαρίου IPython για την ολοκλήρωση των εργασιών που παρουσιάζονται την αναλυτική παρουσίαση χρησιμοποιώντας το σύνολο δεδομένων 1 TB. Οι χρήστες που θέλετε να δοκιμάσετε αυτήν την προσέγγιση πρέπει να συμβουλευτείτε το θέμα [αναλυτικές οδηγίες Criteo χρησιμοποιώντας μια σύνδεση Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Περιγραφή του συνόλου δεδομένων ταξίδια ταξί νέα ΥΌΡΚΗ

Τα δεδομένα του ταξιδιού ταξί νέα ΥΌΡΚΗ είναι 20GB περίπου συμπιεσμένο τιμές διαχωρισμένες με κόμματα (CSV) αρχείων (χωρίς συμπίεση ~ 48GB), που περιλαμβάνει περισσότερες από 173 εκατομμύριο μεμονωμένα ταξίδια και το κομίστρων που καταβλήθηκε για κάθε ταξίδι. Κάθε εγγραφή ταξιδιού περιλαμβάνει τα απάντηση κλήσης από και απόθεσης τη θέση και ώρα, anonymized στοιχείο παραβίασης (του προγράμματος οδήγησης) αριθμό αδειών χρήσης και αριθμό medallion (μοναδικό αναγνωριστικό του ταξί). Τα δεδομένα καλύπτει όλα ταξίδια κατά το έτος 2013 και παρέχεται με τα παρακάτω δύο σύνολα δεδομένων για κάθε μήνα:

1. Τα αρχεία CSV 'trip_data' περιέχουν λεπτομέρειες ταξιδιού, όπως τον αριθμό των προσώπων, απάντηση κλήσης από την και dropoff σημεία, διάρκεια ταξιδιού και μήκους ταξίδι. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Τα αρχεία CSV 'trip_fare' περιέχουν λεπτομέρειες στο ναύλο που καταβάλλεται για κάθε ταξιδιού, όπως τον τύπο πληρωμής, το ποσό ναύλο, πρόσθετης και των φόρων, συμβουλές και διόδια, και το συνολικό ποσό που καταβλήθηκε. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Το μοναδικό κλειδί για να συμμετάσχετε σε ταξίδι\_δεδομένων και ταξίδι\_ναύλο που αποτελείται από τα πεδία: medallion, στοιχείο παραβίασης\_πιστοποιητικό και απάντηση κλήσης από την\_ημερομηνίας/ώρας.

Για να δείτε όλες τις λεπτομέρειες σχετικά με ένα συγκεκριμένο ταξίδι, αρκεί να συμμετάσχετε με τρία πλήκτρα: "medallion", "κλαπεί\_άδεια χρήσης" και "Απάντηση κλήσης από την\_ημερομηνία/ώρα".

Περιγραφή ορισμένες περισσότερες λεπτομέρειες σχετικά με τα δεδομένα όταν αποθηκεύει τους σε πίνακες Hive λίγο.

## <a name="mltasks"></a>Παραδείγματα εργασιών πρόβλεψης
Όταν πλησιάζει δεδομένων, Καθορισμός το είδος των προβλέψεων που θέλετε να κάνετε με βάση την ανάλυσή σας βοηθά διευκρινίσετε τις εργασίες που θα πρέπει να συμπεριλάβετε στη διαδικασία.
Υπάρχουν τρία παραδείγματα της πρόβλεψης προβλήματα που θα σας διεύθυνση σε αυτές τις οδηγίες στο οποίο βασίζεται η διαμόρφωση των οποίων το *Συμβουλή\_ποσό*:

1. **Δυαδικό ταξινόμηση**: πρόβλεψης ή όχι μια συμβουλή πληρώθηκε για ταξίδι, δηλαδή μια *Συμβουλή\_ποσό* που είναι μεγαλύτερη από $0 είναι ένα θετικό παράδειγμα, κατά ένα *Συμβουλή\_ποσό* 0 € αποτελεί ένα παράδειγμα αρνητικός αριθμός.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Ταξινόμηση multiclass**: πρόβλεψη της περιοχής συμβουλή ποσά που καταβλήθηκε για το ταξίδι σας. Διαιρούμε το *Συμβουλή\_ποσό* σε πέντε θέσεων αποθήκευσης ή κατηγορίες:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Εργασία παλινδρόμησης**: πρόβλεψη το ποσό του άκρου του που καταβάλλεται για ένα ταξίδι.  


## <a name="setup"></a>Ρυθμίστε ένα σύμπλεγμα HDInsight Hadoop για προηγμένη ανάλυση

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Μπορείτε να ρυθμίσετε ένα περιβάλλον Azure για προηγμένη ανάλυση που χρησιμοποιεί ένα σύμπλεγμα HDInsight σε τρία βήματα:

1. [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md): αυτόν το λογαριασμό χώρου αποθήκευσης χρησιμοποιείται για την αποθήκευση δεδομένων στο χώρο αποθήκευσης αντικειμένων Blob Azure. Τα δεδομένα που χρησιμοποιούνται σε HDInsight συμπλεγμάτων βρίσκεται επίσης εδώ.

2. [Προσαρμογή Azure HDInsight Hadoop συμπλεγμάτων για τη διαδικασία ανάλυσης για προχωρημένους και τεχνολογία](machine-learning-data-science-customize-hadoop-cluster.md). Αυτό το βήμα δημιουργεί μια HDInsight Hadoop Azure σύμπλεγμα με 2.7 Python Anaconda 64-bit εγκατεστημένα σε όλους τους κόμβους. Υπάρχουν δύο σημαντικά βήματα για να θυμάστε κατά την προσαρμογή το σύμπλεγμά σας HDInsight.

    * Μην ξεχάσετε να συνδέσετε το λογαριασμό χώρου αποθήκευσης που δημιουργήσατε στο βήμα 1 με το σύμπλεγμά σας HDInsight κατά τη δημιουργία του. Για να αποκτήσετε πρόσβαση σε δεδομένα που υποβάλλεται σε επεξεργασία μέσα στο σύμπλεγμα χρησιμοποιείται αυτόν το λογαριασμό χώρου αποθήκευσης.

    * Αφού δημιουργηθεί το σύμπλεγμα, ενεργοποίηση της απομακρυσμένης πρόσβασης στον κεφαλής κόμβο του συμπλέγματος. Μεταβείτε στην καρτέλα **ρύθμισης παραμέτρων** και κάντε κλικ στην επιλογή **Ενεργοποίηση της απομακρυσμένης**. Αυτό το βήμα καθορίζει τα διαπιστευτήρια του χρήστη που χρησιμοποιήθηκε για τη σύνδεση απομακρυσμένης.

3. [Δημιουργία ενός χώρου εργασίας Azure μηχανικής εκμάθησης](machine-learning-create-workspace.md): αυτή Azure μηχανικής εκμάθησης χώρου εργασίας χρησιμοποιείται για να δημιουργήσετε μοντέλα μηχανικής εκμάθησης. Αυτή η εργασία απευθύνεται μετά την ολοκλήρωση μιας Εξερεύνηση αρχικών δεδομένων και προς τα κάτω δειγματοληψία χρησιμοποιώντας το σύμπλεγμα HDInsight.

## <a name="getdata"></a>Λήψη δεδομένων από μια δημόσια προέλευση

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Για να λάβετε το σύνολο δεδομένων [Ταξίδια ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) από τη δημόσια θέση, μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις μεθόδους που περιγράφονται σε [Μετακίνηση δεδομένων από χώρο αποθήκευσης Blob του Azure και](machine-learning-data-science-move-azure-blob.md) να αντιγράψετε τα δεδομένα στον υπολογιστή σας.

Εδώ θα σας περιγράφουν πώς να χρησιμοποιήσετε AzCopy για να μεταφέρετε τα αρχεία που περιέχει τα δεδομένα. Για να κάνετε λήψη και εγκατάσταση AzCopy ακολουθήστε τις οδηγίες στο θέμα [Γρήγορα αποτελέσματα με το βοηθητικό πρόγραμμα γραμμής εντολών AzCopy](../storage/storage-use-azcopy.md).

1. Από το παράθυρο γραμμής εντολών, ζητήματος τις παρακάτω εντολές AzCopy, αντικαθιστώντας *< path_to_data_folder >* με τον επιθυμητό προορισμό:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Όταν ολοκληρωθεί η αντιγραφή, ένα σύνολο 24 αρχεία σε μορφή zip βρίσκονται στο φάκελο δεδομένων επιλέξει. Αποσυμπίεση τα αρχεία που έχουν ληφθεί στον ίδιο κατάλογο στον τοπικό υπολογιστή σας. Σημειώστε το φάκελο όπου βρίσκονται τα αποσυμπιεσμένα αρχεία. Αυτός ο φάκελος θα αναφέρεται ως το *< διαδρομή\_να\_unzipped_data\_αρχεία\> * είναι αυτό που ακολουθεί.


## <a name="upload"></a>Αποστολή των δεδομένων για το προεπιλεγμένο κοντέινερ Azure HDInsight Hadoop συμπλέγματος

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Στις ακόλουθες εντολές AzCopy, αντικαταστήστε τις ακόλουθες παραμέτρους με τις πραγματικές τιμές που καθορίσατε κατά τη δημιουργία του συμπλέγματος Hadoop και unzipping τα αρχεία δεδομένων.

* ***& #60; path_to_data_folder >*** στον κατάλογο (μαζί με τη διαδρομή) στον υπολογιστή σας που περιέχουν τα αρχεία αποσυμπιεσμένο αρχείο δεδομένων  
* ***& #60; όνομα λογαριασμού χώρου αποθήκευσης του συμπλέγματος Hadoop >*** το λογαριασμό χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμά σας HDInsight
* ***& #60; προεπιλεγμένο κοντέινερ συμπλέγματος Hadoop >*** το προεπιλεγμένο κοντέινερ που χρησιμοποιούνται από το σύμπλεγμά σας. Σημειώστε ότι το όνομα του το προεπιλεγμένο κοντέινερ είναι συνήθως το ίδιο όνομα με το σύμπλεγμα ίδια. Για παράδειγμα, εάν το σύμπλεγμα ονομάζεται "abc123.azurehdinsight.net", το προεπιλεγμένο κοντέινερ είναι abc123.
* ***& #60; κλειδί λογαριασμού χώρου αποθήκευσης >*** το κλειδί για το λογαριασμό χώρου αποθήκευσης που χρησιμοποιείται από το σύμπλεγμά σας

Από ένα παράθυρο του Windows PowerShell στον υπολογιστή σας ή μια γραμμή εντολών, εκτελέστε τις ακόλουθες δύο εντολές AzCopy.

Αυτή η εντολή αποστέλλει τα δεδομένα του ταξιδιού στον κατάλογο ***nyctaxitripraw*** το προεπιλεγμένο κοντέινερ του συμπλέγματος Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Αυτή η εντολή αποστέλλει τα δεδομένα ναύλο στον κατάλογο ***nyctaxifareraw*** το προεπιλεγμένο κοντέινερ του συμπλέγματος Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Τα δεδομένα θα πρέπει τώρα στο χώρο αποθήκευσης Blob του Azure και είστε έτοιμοι να καταναλωθεί μέσα στο σύμπλεγμα HDInsight.

## <a name="#download-hql-files"></a>Αρχείο καταγραφής στον κόμβο κεφαλής Hadoop συμπλέγματος και και να προετοιμαστείτε για ανάλυση δεδομένων διερευνητικές

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Για να αποκτήσετε πρόσβαση του κεφαλής κόμβο του συμπλέγματος για ανάλυση δεδομένων διερευνητικές και προς τα κάτω δειγματοληψία των δεδομένων, ακολουθήστε τη διαδικασία που περιγράφεται στην [Access κεφαλή κόμβο του Hadoop συμπλέγματος](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

Σε αυτές τις οδηγίες, χρησιμοποιούμε κυρίως ερωτήματα γραμμένο σε [Hive](https://hive.apache.org/), μια γλώσσα ερωτημάτων μοιάζει με SQL, για να εκτελέσετε explorations προκαταρκτικού δεδομένων. Τα ερωτήματα Hive είναι αποθηκευμένα σε αρχεία .hql. Θα σας, στη συνέχεια, προς τα κάτω δείγματος αυτών των δεδομένων που θα χρησιμοποιηθεί μέσα σε Azure μηχανικής εκμάθησης για τη δημιουργία μοντέλων.

Για να προετοιμάσετε το σύμπλεγμα για την ανάλυση δεδομένων διερευνητικές, γίνεται λήψη των αρχείων .hql που περιέχουν τις σχετικές Hive δέσμες ενεργειών από [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) σε έναν τοπικό κατάλογο (C:\temp) στον κόμβο κεφαλίδας. Για να το κάνετε αυτό, ανοίξτε τη **γραμμή εντολών** μέσα από το κεφαλής κόμβο του συμπλέγματος και θεμάτων τις ακόλουθες δύο εντολές:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Αυτές οι εντολές δύο θα γίνει λήψη όλων των αρχείων .hql απαιτείται σε αυτές τις οδηγίες στον τοπικό κατάλογο ***C:\temp & #92;*** στον κόμβο κεφαλής.

## <a name="#hive-db-tables"></a>Δημιουργία βάσης δεδομένων της ομάδας και πινάκων διαμερίσματα ανά μήνα

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Συνεχίζουμε να τώρα έτοιμοι να δημιουργήσετε πίνακες Hive για το σύνολο δεδομένων ταξί νέα ΥΌΡΚΗ.
Στο κεφαλής κόμβο του συμπλέγματος Hadoop, ανοίξτε τη ***γραμμή εντολών Hadoop*** στην επιφάνεια εργασίας του κεφαλής κόμβου και εισαγάγετε τον κατάλογο Hive, πληκτρολογώντας την εντολή

    cd %hive_home%\bin

>[AZURE.NOTE] **Εκτελέστε όλες οι εντολές της ομάδας σε αυτές τις οδηγίες από τον παραπάνω Κάδο Hive / εντολών του καταλόγου. Αυτό θα εκτελέσει των θεμάτων διαδρομή αυτόματα. Χρησιμοποιούμε τους όρους "Hive καταλόγου ερώτηση", "Hive Ανακύκλωσης / εντολών του καταλόγου", και "γραμμή εντολών Hadoop" χωρίς διάκριση σε αυτές τις οδηγίες.**

Από τη γραμμή εντολών καταλόγου Hive, πληκτρολογήστε την παρακάτω εντολή στο Hadoop γραμμής εντολών του κεφαλής κόμβου για να υποβάλετε το ερώτημα ομάδα για τη δημιουργία βάσης δεδομένων της ομάδας και πίνακες:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Ακολουθεί το περιεχόμενο του ***C:\temp\sample\_hive\_Δημιουργία\_db\_και\_tables.hql*** αρχείο το οποίο δημιουργεί Hive βάσης δεδομένων ***nyctaxidb*** και πινάκων του ***ταξιδιού*** και ***ναύλο***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Αυτή η δέσμη ενεργειών Hive δημιουργεί δύο πίνακες:

* ο πίνακας "ταξιδιού" περιέχει λεπτομέρειες ταξιδιού κάθε βόλτα (λεπτομέρειες του προγράμματος οδήγησης, συλλογής ώρα, απόσταση ταξιδιού και την ώρα)
* ο πίνακας "ναύλο" περιέχει λεπτομέρειες ναύλο (ναύλο ποσό, ποσό συμβουλή, διόδια και πρόσθετες επιβαρύνσεις).

Εάν χρειάζεστε επιπλέον Βοήθεια με αυτές τις διαδικασίες ή θέλετε να ερευνήσετε εναλλακτικές αυτά, ανατρέξτε στην ενότητα [Hive υποβολή ερωτημάτων απευθείας από τη Hadoop γραμμή εντολών ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Φόρτωση δεδομένων σε πίνακες ομάδας από τα διαμερίσματα

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία του **διαχειριστή** .

Το σύνολο δεδομένων ταξί νέα ΥΌΡΚΗ έχει μια φυσική διαμερισμάτων ανά μήνα, που χρησιμοποιούμε για να ενεργοποιήσετε την ταχύτερους χρόνους επεξεργασίας και ερωτήματος. Οι παρακάτω PowerShell εντολές (εκδοθεί από τον κατάλογο Hive χρησιμοποιώντας τη **γραμμή εντολών Hadoop**) φόρτωση δεδομένων με τους πίνακες Hive "ταξιδιού" και "ναύλο" διαμερίσματα ανά μήνα.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

Το *δείγμα\_hive\_φόρτωση\_δεδομένων\_με\_partitions.hql* αρχείο περιέχει τις παρακάτω εντολές **ΦΌΡΤΩΣΗ** .

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Σημειώστε ότι ένα αριθμός ερωτημάτων Hive εδώ χρησιμοποιούμε στη διαδικασία Εξερεύνηση αφορούν θέλετε την απλώς μια μεμονωμένη διαμερίσματα ή την μερικά μόνο τα διαμερίσματα. Αλλά αυτά τα ερωτήματα ήταν δυνατό να εκτελεστεί σε ολόκληρο δεδομένων.

### <a name="#show-db"></a>Εμφάνιση βάσεις δεδομένων του συμπλέγματος HDInsight Hadoop

Για να εμφανίσετε τις βάσεις δεδομένων που έχουν δημιουργηθεί με σύμπλεγμα HDInsight Hadoop μέσα στο παράθυρο γραμμής εντολών Hadoop, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών Hadoop:

    hive -e "show databases;"

### <a name="#show-tables"></a>Εμφάνιση των πινάκων της ομάδας στη βάση δεδομένων nyctaxidb

Για να εμφανίσετε τους πίνακες της βάσης δεδομένων nyctaxidb, εκτελέστε την ακόλουθη εντολή στη γραμμή εντολών Hadoop:

    hive -e "show tables in nyctaxidb;"

Να επιβεβαιωθεί ότι οι πίνακες δημιουργούνται διαμερίσματα με την έκδοση την παρακάτω εντολή:

    hive -e "show partitions nyctaxidb.trip;"

Το αναμενόμενο αποτέλεσμα φαίνεται παρακάτω:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Ομοίως, θα σας να εξασφαλίσετε ότι ο πίνακας ναύλο που έχει διαμερίσματα με την έκδοση την παρακάτω εντολή:

    hive -e "show partitions nyctaxidb.fare;"

Το αναμενόμενο αποτέλεσμα φαίνεται παρακάτω:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Εξερεύνηση δεδομένων και τη δυνατότητα μηχανικής στην ομάδα

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Η Εξερεύνηση δεδομένων και η δυνατότητα μηχανική εργασίες για τα δεδομένα που φορτώνονται στη τους πίνακες Hive μπορεί να γίνει χρήση ερωτημάτων ομάδας. Ακολουθούν παραδείγματα αυτές τις εργασίες που θα σας καθοδηγήσει σε αυτήν την ενότητα:

- Προβάλετε τις 10 πρώτες καρτέλες και στους δύο πίνακες.
- Εξερεύνηση δεδομένων κατανομή ορισμένα πεδία σε διαφορετικές των windows.
- Διερεύνηση ποιότητας δεδομένων των πεδίων γεωγραφικού πλάτους και μήκους.
- Δημιουργία ετικετών δυαδικό και multiclass ταξινόμηση με βάση το **Συμβουλή\_ποσό**.
- Δημιουργία δυνατότητες με τον υπολογισμό τις αποστάσεις απευθείας ταξίδι.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Εξερεύνηση: Προβολή τις 10 πρώτες εγγραφές σε πίνακα ταξιδιού

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Για να δείτε πώς φαίνεται τα δεδομένα, θα σας εξετάστε 10 εγγραφές από κάθε πίνακα. Εκτελέστε τα ακόλουθα δύο ερωτήματα ξεχωριστά από τη γραμμή εντολών καταλόγου Hive στην κονσόλα Hadoop γραμμής εντολών για να ελέγξετε τις εγγραφές.

Για να λάβετε τις 10 πρώτες εγγραφές στον πίνακα "ταξιδιού" από τον πρώτο μήνα:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Για να λάβετε τις καλύτερες 10 εγγραφές στον πίνακα "ναύλο" από τον πρώτο μήνα:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Συχνά είναι χρήσιμο για να αποθηκεύσετε τις εγγραφές σε ένα αρχείο για εύκολη προβολή. Μια μικρή αλλαγή στο παραπάνω ερώτημα επιτυγχάνει αυτό:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Εξερεύνηση: Προβολή του αριθμού εγγραφών σε κάθε ένα από τα διαμερίσματα 12

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Που σας ενδιαφέρουν είναι πώς ο αριθμός των ταξίδια διαφέρει κατά το ημερολογιακό έτος. Ομαδοποίηση κατά μήνα σας επιτρέπει να δείτε πώς φαίνεται αυτή η διανομή ταξίδια.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Αυτό σας δίνει μας το αποτέλεσμα:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Εδώ, η πρώτη στήλη είναι το μήνα και το δεύτερο είναι ο αριθμός των ταξίδια για αυτόν τον μήνα.

Θα σας μπορεί να καταμέτρησης επίσης τον συνολικό αριθμό των εγγραφών στο μας ταξίδι σύνολο δεδομένων με την έκδοση την παρακάτω εντολή στη γραμμή εντολών του καταλόγου της ομάδας.

    hive -e "select count(*) from nyctaxidb.trip;"

Αυτό παράγει:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Χρησιμοποιώντας εντολές παρόμοια με αυτά που εμφανίζονται για το σύνολο δεδομένων ταξιδιού, θα σας να εκδώσετε ερωτήματα Hive από τη γραμμή εντολών Hive καταλόγου για το σύνολο δεδομένων ναύλο για την επικύρωση του αριθμού εγγραφών.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Αυτό σας δίνει μας το αποτέλεσμα:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Σημειώστε ότι το ίδιο ακριβή αριθμό ταξίδια ανά μήνα επιστρέφεται για δύο σύνολα δεδομένων. Αυτό παρέχει την πρώτη επικύρωση ότι τα δεδομένα έχει φορτωθεί σωστά.

Μετρώντας τον συνολικό αριθμό των εγγραφών του συνόλου δεδομένων ναύλο που μπορεί να γίνει χρησιμοποιώντας την εντολή κάτω από τη γραμμή εντολών Hive καταλόγου:

    hive -e "select count(*) from nyctaxidb.fare;"

Αυτό παράγει:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Ο συνολικός αριθμός των εγγραφών και στους δύο πίνακες είναι επίσης το ίδιο. Αυτή η δυνατότητα παρέχει ένα δεύτερο επικύρωσης ότι τα δεδομένα έχει φορτωθεί σωστά.

### <a name="exploration-trip-distribution-by-medallion"></a>Εξερεύνηση: Ταξιδιού κατανομή με medallion

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Αυτό το παράδειγμα προσδιορίζει το medallion (ταξί αριθμοί) με περισσότερα από 100 ταξίδια μέσα σε μια δεδομένη χρονική περίοδο. Τα πλεονεκτήματα ερωτήματος από την access διαμερίσματα πίνακα καθώς αυτό είναι σταθεροποιούνται ανά partition μεταβλητής **μήνα**. Τα αποτελέσματα του ερωτήματος που είναι γραμμένες σε ένα τοπικό αρχείο queryoutput.tsv στο `C:\temp` στον κόμβο κεφαλίδας.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Εδώ είναι το περιεχόμενο του *δείγμα\_hive\_ταξιδιού\_πλήθος\_με\_medallion.hql* αρχείου για τον έλεγχο.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Το medallion του συνόλου δεδομένων ταξί νέα ΥΌΡΚΗ προσδιορίζει ένα μοναδικό θάλαμο. Θα σας να προσδιορίσετε ποια θάλαμοι οδήγησης είναι "απασχολημένος" ζητώντας ποιες κάνει περισσότερα από ένα συγκεκριμένο αριθμό ταξίδια σε μια συγκεκριμένη χρονική περίοδο. Το παρακάτω παράδειγμα προσδιορίζει θάλαμοι οδήγησης που κάνει περισσότερο από εκατό ταξίδια στην πρώτη τρεις μήνες, και αποθηκεύει τα αποτελέσματα του ερωτήματος σε ένα τοπικό αρχείο, C:\temp\queryoutput.tsv.

Εδώ είναι το περιεχόμενο του *δείγμα\_hive\_ταξιδιού\_πλήθος\_με\_medallion.hql* αρχείου για τον έλεγχο.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Από τη γραμμή εντολών καταλόγου Hive, ζητήματος την παρακάτω εντολή:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Εξερεύνηση: Ταξιδιού διανομής, medallion και hack_license

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Όταν Εξερεύνηση ένα σύνολο δεδομένων, θα σας συχνά θέλετε να εξετάσετε το πλήθος των από κοινού-τις περιπτώσεις των ομάδων των τιμών. Αυτή η ενότητα παρέχει ένα παράδειγμα του τρόπου να το κάνετε αυτό για θάλαμοι οδήγησης και τα προγράμματα οδήγησης.

Το *δείγμα\_hive\_ταξιδιού\_πλήθος\_με\_medallion\_license.hql* αρχείο ομαδοποιεί το σύνολο δεδομένων ναύλο σε "medallion" και "hack_license" και επιστρέφει το πλήθος κάθε συνδυασμό. Παρακάτω υπάρχουν τα περιεχόμενά της.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Αυτό το ερώτημα επιστρέφει γαβ και οι συνδυασμοί συγκεκριμένο πρόγραμμα οδήγησης ταξινομημένες κατά φθίνουσα αριθμό ταξίδια.

Από τη γραμμή εντολών καταλόγου Hive, εκτελέστε:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Τα αποτελέσματα του ερωτήματος είναι γραμμένες σε ένα τοπικό αρχείο C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Εξερεύνηση: Αξιολόγησης ποιότητας δεδομένων, επιλέγοντας για εγγραφές δεν είναι έγκυρη γεωγραφικού/γεωγραφικό πλάτος

>[AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Κοινό στόχο της ανάλυσης διερευνητικές δεδομένων είναι να weed θέμα δεν είναι έγκυρη ή εσφαλμένες εγγραφές. Το παράδειγμα σε αυτήν την ενότητα καθορίζει αν το μήκος ή γεωγραφικό πλάτος πεδία περιέχουν μια τιμή άκρο έξω από την περιοχή νέα ΥΌΡΚΗ. Εφόσον είναι πιθανό ότι οι εγγραφές έχουν μια τιμές εσφαλμένους γεωγραφικού-γεωγραφικό πλάτος, θέλουμε να αποκλείσετε τους από τα δεδομένα που θα χρησιμοποιηθεί για μοντελοποίηση.

Εδώ είναι το περιεχόμενο του *δείγμα\_hive\_ποιότητα\_assessment.hql* αρχείου για έλεγχο.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Από τη γραμμή εντολών καταλόγου Hive, εκτελέστε:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Το όρισμα *-S* που περιλαμβάνονται σε αυτήν την εντολή αποκρύπτει εκτύπωσης οθόνης κατάστασης των εργασιών Hive χάρτη/μείωση. Αυτό είναι χρήσιμο, επειδή την κάνει την οθόνη εκτύπωσης από το αποτέλεσμα του ερωτήματος Hive πιο ευανάγνωστο.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Εξερεύνηση: Κατανομές δυαδικό τάξης του ταξιδιού συμβουλές

> [AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Για το πρόβλημα δυαδικό ταξινόμησης που περιγράφονται στην ενότητα [παραδείγματα εργασιών πρόβλεψη](machine-learning-data-science-process-hive-walkthrough.md#mltasks) , είναι χρήσιμο να γνωρίζετε εάν μια συμβουλή δόθηκε ή όχι. Αυτή η κατανομή των συμβουλών είναι δυαδικό:

* Συμβουλή δεδομένο (κλάσης 1, συμβουλή\_ποσό > $0)  
* δεν υπάρχει συμβουλή (κλάση 0, συμβουλή\_ποσό = $0).

Το *δείγμα\_hive\_επικαλυπτόμενα\_frequencies.hql* τα αρχεία που εμφανίζονται κάτω από το κάνει αυτό.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Από τη γραμμή εντολών καταλόγου Hive, εκτελέστε:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Εξερεύνηση: Κλάση κατανομές στη ρύθμιση multiclass

> [AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Για το πρόβλημα multiclass ταξινόμησης που περιγράφονται στην ενότητα [παραδείγματα εργασιών πρόβλεψη](machine-learning-data-science-process-hive-walkthrough.md#mltasks) αυτό το σύνολο δεδομένων επίσης είναι μια φυσική ταξινόμηση όπου θα σας θα θέλατε να προβλέψει το ποσό της τις συμβουλές που δίνεται. Μπορούμε να χρησιμοποιήσουμε κάδους ορισμού περιοχών συμβουλή στο ερώτημα. Για να λάβετε τη διανομή εκπαιδευτικών για τις διάφορες συμβουλή περιοχές, χρησιμοποιούμε τη *δείγμα\_hive\_συμβουλή\_περιοχή\_frequencies.hql* αρχείου. Παρακάτω υπάρχουν τα περιεχόμενά της.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Εκτελέστε την ακόλουθη εντολή από την κονσόλα Hadoop γραμμής εντολών:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Εξερεύνηση: Υπολογίσετε απευθείας απόσταση μεταξύ των δύο θέσεις γεωγραφικού-γεωγραφικό πλάτος

> [AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Χρειάζεται μια μέτρηση της απόστασης απευθείας επιτρέπει μας για να βρείτε την ασυμφωνία μεταξύ αυτής και την απόσταση πραγματική ταξίδι. Θα σας να ενθαρρύνετε αυτή η δυνατότητα τοποθετώντας το δείκτη ότι επιβάτης μπορεί να είναι λιγότερο πιθανό να συμβουλή εάν αυτά να καταλάβετε ότι το πρόγραμμα οδήγησης σκόπιμα έλαβε τους από μια πολύ περισσότερο δρομολόγησης.

Για να δείτε τη σύγκριση μεταξύ πραγματική ταξιδιού απόσταση και την [απόσταση Haversine](http://en.wikipedia.org/wiki/Haversine_formula) μεταξύ δύο σημείων γεωγραφικού-γεωγραφικό πλάτος (την απόσταση "εξαιρετικά κύκλου"), χρησιμοποιούμε τις διαθέσιμες συναρτήσεις τριγωνομετρική μέσα σε ομάδα, επομένως:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Στο παραπάνω ερώτημα, R είναι η ακτίνα της γης σε μίλια και π μετατρέπεται σε ακτίνια. Σημειώστε ότι τα σημεία γεωγραφικού-γεωγραφικό πλάτος "φιλτράρονται" για να καταργήσετε τιμές που είναι μακριά από την περιοχή νέα ΥΌΡΚΗ.

Σε αυτήν την περίπτωση, θα σας να καταγράψετε μας αποτελέσματα σε έναν κατάλογο που ονομάζεται "queryoutputdir". Η ακολουθία των εντολών που φαίνεται παρακάτω δημιουργεί πρώτα αυτόν τον κατάλογο εξόδου, και, στη συνέχεια, εκτελεί την εντολή ομάδα.

Από τη γραμμή εντολών καταλόγου Hive, εκτελέστε:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Τα αποτελέσματα του ερωτήματος που είναι γραμμένες σε 9 Azure αντικείμενα BLOB ***queryoutputdir/000000\_0*** να ***queryoutputdir/000008\_0*** κάτω από το κοντέινερ προεπιλεγμένη του συμπλέγματος Hadoop.

Για να δείτε το μέγεθος του τα μεμονωμένα αντικείμενα blob, μπορούμε να εκτελέσουμε την ακόλουθη εντολή από τη γραμμή εντολών Hive καταλόγου:

    hdfs dfs -ls wasb:///queryoutputdir

Για να δείτε τα περιεχόμενα του αρχείου που δίνεται, πείτε 000000\_0, χρησιμοποιούμε του Hadoop `copyToLocal` , επομένως, εντολή.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`μπορεί να είναι πολύ αργή για μεγάλα αρχεία και δεν συνιστάται για χρήση με αυτά.  

Ένα βασικό πλεονέκτημα της αντιμετωπίζετε αυτά τα δεδομένα που βρίσκονται σε μια αντικειμένων blob του Azure είναι ότι θα σας ενδέχεται να εξερευνήσετε τα δεδομένα μέσα σε Azure μηχανικής εκμάθησης χρησιμοποιώντας την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα.


## <a name="#downsample"></a>Προς τα κάτω δείγμα δεδομένων και δημιουργία μοντέλων στο Azure μηχανικής εκμάθησης

> [AZURE.NOTE] Αυτό είναι συνήθως μια εργασία **Υπεύθυνος δημιουργίας δεδομένων** .

Μετά τη φάση ανάλυσης διερευνητικές δεδομένων, θα σας είναι πλέον έτοιμοι προς τα κάτω δείγμα τα δεδομένα για τη δημιουργία μοντέλων στο Azure μηχανικής εκμάθησης. Σε αυτήν την ενότητα, θα σας δείξουμε πώς μπορείτε να χρησιμοποιήσετε ένα ερώτημα Hive προς τα κάτω δείγμα των δεδομένων, το οποίο είναι πρόσβαση και, στη συνέχεια, από την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα Azure μηχανικής εκμάθησης.

### <a name="down-sampling-the-data"></a>Προς τα κάτω δειγματοληψία των δεδομένων

Υπάρχουν δύο βήματα σε αυτήν τη διαδικασία. Πρώτα θα σας συνδυασμό των πινάκων **nyctaxidb.trip** και **nyctaxidb.fare** σε τρία κλειδιά που είναι παρόντες σε όλες τις εγγραφές: "medallion", "κλαπεί\_άδεια χρήσης", και "Απάντηση κλήσης από την\_ημερομηνία/ώρα". Θα σας, στη συνέχεια, να δημιουργήσετε μια ετικέτα δυαδικό ταξινόμηση **με τμήμα στην άκρη** και μια ετικέτα ταξινόμηση πολλών κλάσης **Συμβουλή\_κλάσης**.

Για να μπορέσετε να χρησιμοποιήσετε το κάτω δείγμα δεδομένων απευθείας από την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα στο Azure μηχανικής εκμάθησης, είναι απαραίτητο να αποθηκεύσετε τα αποτελέσματα του παραπάνω ερωτήματος σε πίνακα του εσωτερικού ομάδας. Σε αυτό που ακολουθεί, Δημιουργία πίνακα του εσωτερικού Hive και συμπληρώσετε τα περιεχόμενά με τους συνδεδεμένους και προς τα κάτω δείγματος δεδομένων.

Το ερώτημα ισχύει τυπικές συναρτήσεις Hive απευθείας, για να δημιουργήσετε την ώρα της ημέρας, εβδομάδας του έτους, ημέρα της εβδομάδας (1 πόδι Δευτέρα και 7 πόδι Κυριακή) από το "Απάντηση κλήσης από την\_ημερομηνία/ώρα" πεδίο και την άμεση απόσταση μεταξύ των θέσεων απάντηση κλήσης από και dropoff. Οι χρήστες μπορούν να αναφέρονται σε [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) για μια πλήρη λίστα των εν λόγω συναρτήσεων.

Το ερώτημα, στη συνέχεια, προς τα κάτω δειγμάτων δεδομένων, έτσι ώστε να χωρούν τα αποτελέσματα του ερωτήματος σε το Studio Azure μηχανικής εκμάθησης. Μόνο περίπου 1% του αρχικού dataset έχει εισαχθεί στο το Studio.

Παρακάτω υπάρχουν τα περιεχόμενα του *δείγμα\_hive\_προετοιμασία\_για\_aml\_full.hql* αρχείου που προετοιμάζει δεδομένων για το μοντέλο που δημιουργείτε στο Azure μηχανικής εκμάθησης.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Για να εκτελέσετε αυτό το ερώτημα, από τη γραμμή εντολών Hive καταλόγου:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Τώρα έχουμε έναν εσωτερικό πίνακα "nyctaxidb.nyctaxi_downsampled_dataset", το οποίο είναι δυνατή η πρόσβαση με την [Εισαγωγή δεδομένων] [ import-data] λειτουργικής μονάδας από το Azure μηχανικής εκμάθησης. Επιπλέον, ενδέχεται να χρησιμοποιήσουμε το σύνολο δεδομένων για τη δημιουργία μοντέλα μηχανικής εκμάθησης.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Χρησιμοποιήστε τη λειτουργική μονάδα εισαγωγή δεδομένων στο Azure μηχανικής εκμάθησης για να αποκτήσετε πρόσβαση στα δεδομένα κάτω δείγματος

Ως τις προϋποθέσεις για την έκδοση Hive ερωτημάτων στα [Δεδομένα εισαγωγής] [ import-data] λειτουργική μονάδα του Azure μηχανικής εκμάθησης, χρειαζόμαστε πρόσβαση σε μια Azure μηχανικής εκμάθησης χώρου εργασίας και πρόσβαση σε τα διαπιστευτήρια του συμπλέγματος και της λογαριασμό συσχετισμένη χώρου αποθήκευσης.

Ορισμένες λεπτομέρειες σχετικά με την [Εισαγωγή δεδομένων] [ import-data] λειτουργικής μονάδας και τις παραμέτρους για να εισαγάγετε:

**URI διακομιστή HCatalog**: Εάν το όνομα του συμπλέγματος είναι abc123 και, στη συνέχεια, αυτό είναι απλώς: https://abc123.azurehdinsight.net

**Όνομα λογαριασμού χρήστη Hadoop** : το όνομα χρήστη που έχει επιλέξει για το σύμπλεγμα (**όχι** το όνομα χρήστη απομακρυσμένης πρόσβασης)

**Ο κωδικός πρόσβασης λογαριασμού ρήστη Hadoop** : το κωδικό πρόσβασης που επιλέξατε για το σύμπλεγμα (**δεν** τον κωδικό απομακρυσμένης πρόσβασης)

**Θέση των δεδομένων εξόδου** : αυτή έχει επιλέξει να Azure.

**Όνομα λογαριασμού Azure χώρου αποθήκευσης** : όνομα του προεπιλεγμένου λογαριασμού χώρου αποθήκευσης που σχετίζεται με το σύμπλεγμα.

**Το όνομα του Azure κοντέινερ** : αυτό είναι το προεπιλεγμένο όνομα κοντέινερ για το σύμπλεγμα και είναι συνήθως το ίδιο με το όνομα του συμπλέγματος. Για ένα σύμπλεγμα που ονομάζεται "abc123", αυτό είναι απλώς abc123.

> [AZURE.IMPORTANT] **Κάθε πίνακα Ευχόμαστε οι ερώτημα χρησιμοποιώντας την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα Azure μάθετε υπολογιστή πρέπει να είναι ένας εσωτερικός πίνακας.** Συμβουλή για τον καθορισμό εάν ένας πίνακας T σε μια βάση δεδομένων D.db είναι ένας εσωτερικός πίνακας είναι ως εξής.

Από τη γραμμή εντολών καταλόγου Hive, εκδώσετε την εντολή:

    hdfs dfs -ls wasb:///D.db/T

Εάν ο πίνακας είναι ένας εσωτερικός πίνακας και το συμπληρώνεται, τα περιεχόμενά πρέπει να εμφανίζεται εδώ. Ένας άλλος τρόπος για να προσδιορίσετε αν ένας πίνακας είναι ένας εσωτερικός πίνακας είναι να χρησιμοποιήσετε την Εξερεύνηση χώρου αποθήκευσης Azure. Χρησιμοποιήστε το για να μεταβείτε στο προεπιλεγμένο όνομα κοντέινερ του συμπλέγματος και, στη συνέχεια, να φιλτράρετε με βάση το όνομα του πίνακα. Εάν ο πίνακας και τα περιεχόμενά εμφανίζεται, αυτό επιβεβαιώνει ότι είναι ένας εσωτερικός πίνακας.

Ακολουθεί ένα στιγμιότυπο του ερωτήματος ομάδας και την [Εισαγωγή δεδομένων] [ import-data] λειτουργικής μονάδας:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Σημειώστε ότι από μας δείγματος δεδομένων βρίσκεται στο προεπιλεγμένο κοντέινερ προς τα κάτω, το ερώτημα που προκύπτει ομάδα από το Azure μηχανικής εκμάθησης είναι πολύ απλή και είναι απλώς μια "ΕΠΙΛΟΓΉ * από nyctaxidb.nyctaxi\_μειωθεί η ανάλυση\_δεδομένων".

Του συνόλου δεδομένων τώρα μπορεί να χρησιμοποιηθεί ως σημείο εκκίνησης για τη δημιουργία μηχανικής εκμάθησης μοντέλων.

### <a name="mlmodel"></a>Δημιουργία μοντέλων στο Azure μηχανικής εκμάθησης

Συνεχίζουμε να τώρα μπορείτε να συνεχίσετε να δόμησης μοντέλο και μοντέλο ανάπτυξης στο [Azure μηχανικής εκμάθησης](https://studio.azureml.net). Τα δεδομένα είναι έτοιμα για μας για να χρησιμοποιήσετε στην εξοικείωση με τα προβλήματα πρόβλεψης που έχουν προσδιοριστεί παραπάνω:

**1. δυαδικό ταξινόμηση**: πρόβλεψη ή όχι μια συμβουλή πληρώθηκε για ταξίδι.

**Μαθαίνω χρησιμοποιούνται:** Δύο κατηγορίας εφοδιαστική παλινδρόμησης

μια. Σε αυτό το πρόβλημα, μας ετικέτας προορισμού (ή κλάσης) είναι "με τμήμα στην άκρη". Το αρχικό σύνολο δεδομένων δειγματοληψία κάτω έχει μερικές στήλες που είναι απώλειες προορισμού για αυτό έρευνας ταξινόμηση. Συγκεκριμένα: συμβουλή\_κλάση, συμβουλή\_ποσό και σύνολο\_ποσό αποκάλυψης πληροφορίες σχετικά με την ετικέτα προορισμού που δεν είναι διαθέσιμη στο δοκιμές ώρα. Θα σας καταργήστε αυτές τις στήλες από την εξέταση χρησιμοποιώντας την [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] λειτουργική μονάδα.

Το παρακάτω στιγμιότυπο εμφανίζει μας έρευνα θα πρόβλεψης ή όχι μια συμβουλή πληρώθηκε για ένα δεδομένο ταξίδι.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

β. Για αυτήν την έρευνα, μας κατανομές ετικέτα προορισμού έχουν περίπου 1:1.

Το παρακάτω στιγμιότυπο εμφανίζει την κατανομή των συμβουλή κλάσης ετικετών για το πρόβλημα δυαδικό ταξινόμηση.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Ως αποτέλεσμα, έχουμε μια AUC του 0.987 όπως φαίνεται στην παρακάτω εικόνα.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass ταξινόμηση**: πρόβλεψη της περιοχής που καταβλήθηκε για το ταξίδι σας, χρησιμοποιώντας τις παλιότερα καθορισμένη κλάσεις ποσά συμβουλή.

**Μαθαίνω χρησιμοποιούνται:** Multiclass εφοδιαστική παλινδρόμησης

μια. Για αυτό το πρόβλημα, μας ετικέτα προορισμού (ή κλάσης) είναι "Συμβουλή\_κλάσης" που μπορεί να διαρκέσει μία από τις πέντε τιμές (0,1,2,3,4). Όπως και στην περίπτωση δυαδικό ταξινόμηση, έχουμε μερικές στήλες που είναι απώλειες προορισμού για αυτήν την έρευνα. Συγκεκριμένα: επικαλυπτόμενα, συμβουλή\_συνολικό ποσό\_ποσό αποκάλυψης πληροφορίες σχετικά με την ετικέτα προορισμού που δεν είναι διαθέσιμη στο δοκιμές ώρα. Θα σας κατάργηση αυτών των στηλών χρησιμοποιώντας την [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] λειτουργική μονάδα.

Το παρακάτω στιγμιότυπο εμφανίζει μας έρευνα θα πρόβλεψης σε ποια θέση αποθήκης μια συμβουλή είναι πιθανό να κυμαίνεται (κλάση 0: συμβουλή = $0, η κατηγορία 1: συμβουλή > $0 και συμβουλή < = $5, κλάσης 2: συμβουλή > $5 και η συμβουλή < = $10, κλάσης 3: συμβουλή > $10 και η συμβουλή < = $20, τάξης 4: συμβουλή > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

Θα σας δείξουμε τώρα μας διανομής κλάσης πραγματική δοκιμής μοιάζει. Θα δούμε ότι ενώ κλάση 0 και 1 κλάσης είναι διαδεδομένο, οι άλλες κλάσεις είναι σπάνια.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

β. Για αυτήν την έρευνα, χρησιμοποιούμε μια μήτρα σύγχυση για να δείτε τις μας ορθότητας πρόβλεψης. Αυτό εμφανίζεται κάτω από το στοιχείο.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Σημειώστε ότι ενώ μας ορθότητας τάξης σε οι κλάσεις διαδεδομένο είναι αρκετά καλή, το μοντέλο δεν κάνει πολύ ικανοποιητικά "εκμάθησης" στην πιο σπάνιες κλάδους.


**3. εργασίας παλινδρόμησης**: πρόβλεψη το ποσό που καταβλήθηκε για ταξίδι συμβουλή.

**Μαθαίνω χρησιμοποιούνται:** Δέντρου αποφάσεων ενισχύεται

μια. Για αυτό το πρόβλημα, μας ετικέτα προορισμού (ή κλάσης) είναι "Συμβουλή\_ποσό". Σε αυτήν την περίπτωση είναι μας απώλειες προορισμού: επικαλυπτόμενα, συμβουλή\_κλάση, σύνολο\_ποσό; όλες αυτές τις μεταβλητές αποκαλύψει πληροφορίες σχετικά με το ποσό συμβουλή που δεν είναι συνήθως διαθέσιμη σε δοκιμές ώρα. Θα σας κατάργηση αυτών των στηλών χρησιμοποιώντας την [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] λειτουργική μονάδα.

Το στιγμιότυπο belows εμφανίζει μας έρευνα θα πρόβλεψης την ποσότητα του τη δεδομένη συμβουλή.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

β. Για προβλήματα παλινδρόμησης, θα σας να μετρήσετε την ένδειξη των βαθμών ακριβείας μας πρόβλεψης, εξετάζοντας το τετράγωνο σφάλμα στο το προβλέψεων, ο συντελεστής προσδιορισμού και παρόμοια. Θα σας δείξουμε αυτές τις παρακάτω.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Βλέπουμε ότι σχετικά με το συντελεστή προσδιορισμού είναι 0.709, πραγματική τους καταγωγή· περίπου 71% διακύμανσης εξηγείται μας συντελεστές μοντέλο.

> [AZURE.IMPORTANT] Για να μάθετε περισσότερα σχετικά με το Azure μηχανικής εκμάθησης και πώς μπορείτε να έχετε πρόσβαση και να το χρησιμοποιήσετε, ανατρέξτε στο [Τι είναι η μηχανικής εκμάθησης;](machine-learning-what-is-machine-learning.md). Ένας πόρος πολύ χρήσιμο για την αναπαραγωγή με μια ομάδα δοκιμές μηχανικής εκμάθησης στην Azure μηχανικής εκμάθησης είναι η [Συλλογή πληροφοριών Cortana](https://gallery.cortanaintelligence.com/). Στη συλλογή καλύπτει μια γκάμα δοκιμών και παρέχει ένα λεπτομερή εισαγωγής στην περιοχή δυνατότητες του Azure μηχανικής εκμάθησης.

## <a name="license-information"></a>Πληροφορίες άδειας χρήσης

Αυτή η αναλυτική παρουσίαση δείγμα και τις συνοδευτικές δέσμες ενεργειών γίνεται κοινή χρήση από τη Microsoft κάτω από την άδεια χρήσης του MIT. Ελέγξτε το αρχείο αρχείο LICENSE.txt στον κατάλογο του δείγματος κώδικα σε GitHub για περισσότερες λεπτομέρειες.

## <a name="references"></a>Αναφορές

• [Ταξίδια ταξί νέα ΥΌΡΚΗ Andrés Monroy λήψη σελίδας](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing του νέα ΥΌΡΚΗ ταξί ταξιδιού δεδομένων από τον Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Ταξί νέα ΥΌΡΚΗ και Limousine προμήθειας έρευνας και στατιστικά στοιχεία](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

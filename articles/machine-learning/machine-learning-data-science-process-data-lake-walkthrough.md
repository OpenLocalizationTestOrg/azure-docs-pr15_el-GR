<properties
    pageTitle="Μεταβλητού μεγέθους Science δεδομένων στο Azure λίμνης δεδομένων: μια αναλυτικές οδηγίες για να ολοκληρωμένες | Microsoft Azure"
    description="Πώς μπορείτε να χρησιμοποιήσετε Azure λίμνης δεδομένων για να κάνετε Εξερεύνηση και δυαδικό ταξινόμηση εργασιών με δεδομένα σε ένα σύνολο δεδομένων."  
    services="machine-learning"
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
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Μεταβλητού μεγέθους Science δεδομένων στο Azure λίμνης δεδομένων: μια αναλυτικές οδηγίες για να ολοκληρωμένες

Αυτόν τον Οδηγό δείχνει πώς να χρησιμοποιείτε Azure λίμνης δεδομένων για να κάνετε Εξερεύνηση δεδομένων και δυαδικό ταξινόμηση εργασιών σε ένα δείγμα του ταξιδιού ταξί νέα ΥΌΡΚΗ και ναύλο dataset πρόβλεψη ή όχι μια συμβουλή αποπληρωμής από μια ναύλο. Σάς καθοδηγεί τα βήματα της [Ομάδας δεδομένα Science διαδικασία](http://aka.ms/datascienceprocess), για να ολοκληρωμένες, από απόκτηση δεδομένων για την εκπαίδευση του μοντέλου και, στη συνέχεια, την ανάπτυξη μιας υπηρεσίας web που δημοσιεύει το μοντέλο.


### <a name="azure-data-lake-analytics"></a>Ανάλυση λίμνης Azure δεδομένων

Το [Microsoft Azure δεδομένων λίμνης](https://azure.microsoft.com/solutions/data-lake/) περιλαμβάνει όλες τις δυνατότητες που απαιτούνται για να διευκολύνετε τους επιστήμονες δεδομένων για την αποθήκευση δεδομένων από οποιοδήποτε μέγεθος, σχήμα και της ταχύτητας και για τη διεξαγωγή επεξεργασίας δεδομένων, προηγμένη ανάλυση και μηχανικής εκμάθησης μοντελοποίηση με υψηλή κλιμάκωση με αποτελεσματικό τρόπο.   Πληρωμή με βάση ανά εργασία, μόνο όταν είναι στην πραγματικότητα επεξεργασία δεδομένων. Ανάλυση λίμνης Azure δεδομένων περιλαμβάνει U-SQL, μια γλώσσα που συνδυάζεται τη φύση δηλωτικό του SQL με τη εκφραστικές δύναμη του C# για την παροχή με κατανεμημένο δυνατότητα ερωτήματος. Το σάς επιτρέπει να επεξεργάζονται μη δομημένα δεδομένα, εφαρμόζοντας σχήματος στην ανάγνωση, εισαγάγετε προσαρμοσμένη λογική και οι συναρτήσεις που ορίζονται από το χρήστη (UDF) και περιλαμβάνει επεκτασιμότητα του για να ενεργοποιήσετε την ρυθμίσετε κοκκώδης έλεγχο τον τρόπο εκτέλεσης με κλίμακα. Για να μάθετε περισσότερα σχετικά με τη σχεδίαση φιλοσοφίας πίσω από το Α-SQL, ανατρέξτε στο θέμα [Δημοσίευση ιστολογίου Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Ανάλυση λίμνης δεδομένων είναι επίσης ένα σημαντικό τμήμα της οικογένειας προγραμμάτων ανάλυση Cortana και λειτουργεί με αποθήκη δεδομένων του SQL Azure, το Power BI και εργοστασίου δεδομένων. Αυτό σας δίνει μια πλατφόρμα προηγμένη ανάλυση και ολοκλήρωση cloud μεγάλο δεδομένων.

Αυτή η αναλυτική παρουσίαση ξεκινά με το που περιγράφει τις προϋποθέσεις και τους πόρους που απαιτούνται για την ολοκλήρωση των εργασιών με δεδομένα λίμνης ανάλυσης που σχηματίζουν διαδικασίας science δεδομένων και πώς μπορείτε να τις εγκαταστήσετε. Στη συνέχεια, αυτό περιγράφει τα βήματα επεξεργασίας δεδομένων χρησιμοποιώντας U-SQL και συμπεράνει, που δείχνει πώς μπορείτε να χρησιμοποιήσετε Python και ομάδα με το Azure μηχανικής εκμάθησης Studio για τη δημιουργία και ανάπτυξη των μοντέλων πρόβλεψης. 


### <a name="u-sql-and-visual-studio"></a>U-SQL και του Visual Studio

Αυτή η αναλυτική παρουσίαση συνιστά τη χρήση του Visual Studio για να επεξεργαστείτε δέσμες ενεργειών U-SQL για την επεξεργασία του συνόλου δεδομένων. Οι δέσμες ενεργειών U-SQL είναι που περιγράφονται εδώ και που παρέχεται σε ξεχωριστό αρχείο. Η διαδικασία περιλαμβάνει ingesting και Εξερεύνηση δειγματοληψία τα δεδομένα. Εμφανίζει επίσης πώς μπορείτε να εκτελέσετε μια εργασία U-SQL δέσμης ενεργειών από την πύλη του Azure. Ομάδα πίνακες δημιουργούνται για τα δεδομένα σε ένα σχετικό σύμπλεγμα HDInsight για τη διευκόλυνση της δόμησης και ανάπτυξης ενός μοντέλου δυαδικό ταξινόμηση στο Azure μηχανικής εκμάθησης Studio.  


### <a name="python"></a>Python

Αυτή η αναλυτική παρουσίαση περιλαμβάνει επίσης μια ενότητα που δείχνει πώς μπορείτε να δημιουργήσετε και να αναπτύξετε μοντέλου πρόβλεψης χρήση Python με Azure μηχανικής εκμάθησης Studio.  Παρέχουμε ένα σημειωματάριο Jupyter με τις δέσμες ενεργειών Python για αυτά τα βήματα σε αυτήν τη διαδικασία. Το Σημειωματάριο περιλαμβάνει κώδικα για ορισμένες επιπλέον δυνατότητα μηχανικής βήματα και μοντέλα κατασκευή όπως multiclass ταξινόμηση και παλινδρόμησης μοντελοποίησης εκτός από το μοντέλο δυαδικό ταξινόμησης που περιγράφονται εδώ. Η εργασία παλινδρόμησης είναι η πρόβλεψη το ποσό του άκρου του που βασίζονται σε άλλες δυνατότητες συμβουλή. 


### <a name="azure-machine-learning"></a>Azure μηχανικής εκμάθησης
Azure μηχανικής εκμάθησης Studio χρησιμοποιείται για τη δημιουργία και ανάπτυξη των μοντέλων πρόβλεψης. Αυτό γίνεται με δύο μεθόδους: πρώτα με Python δέσμες ενεργειών και, στη συνέχεια, ομάδα πίνακες σε ένα σύμπλεγμα HDInsight (Hadoop).


### <a name="scripts"></a>Δέσμες ενεργειών

Μόνο τα κύρια βήματα περιγράφονται σε αυτές τις οδηγίες. Μπορείτε να λάβετε την πλήρη **δέσμη ενεργειών U SQL** και **Jupyter σημειωματαρίου** από [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε αυτά τα θέματα, πρέπει να έχετε τα εξής:

- Μια συνδρομή του Azure. Εάν δεν έχετε ήδη ένα, ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Συνιστάται] Visual Studio 2013 ή του 2015. Εάν δεν έχετε ήδη μία από αυτές τις εκδόσεις που έχουν εγκατασταθεί, μπορείτε να κάνετε λήψη μιας δωρεάν edition Κοινότητας από [εδώ](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Κάντε κλικ στο κουμπί **λήψη 2015 Κοινότητας** κάτω από την ενότητα Visual Studio. 

>[AZURE.NOTE] Αντί για το Visual Studio, μπορείτε επίσης να χρησιμοποιήσετε την πύλη του Azure για την υποβολή ερωτημάτων Azure λίμνης δεδομένων. Παρέχουμε οδηγίες σχετικά με τον τρόπο για να το κάνετε τόσο με το Visual Studio και στην πύλη στην ενότητα με τίτλο **διαδικασία δεδομένα με U-SQL**. 

- Η εγγραφή στο προεπισκόπηση λίμνης Azure δεδομένων

>[AZURE.NOTE] Πρέπει να λάβετε έγκρισης για να χρησιμοποιήσετε Azure δεδομένων λίμνης Store (ADLS) και ανάλυση λίμνης δεδομένων Azure (ADLA) με αυτές τις υπηρεσίες στο preview. Θα σας ζητηθεί να εγγραφούν κατά τη δημιουργία του πρώτου ADLS ή ADLA. Για να sigh προς τα επάνω, κάντε κλικ στην **εγγραφή για να κάνετε προεπισκόπηση**, διαβάστε τη συμφωνία και κάντε κλικ στο κουμπί **OK**. Εδώ, για παράδειγμα, είναι η εγγραφή ADLS σελίδας:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Προετοιμάσετε το περιβάλλον science δεδομένων για λίμνης δεδομένων Azure
Για να προετοιμάσετε το περιβάλλον science δεδομένων για αυτόν τον οδηγό, δημιουργήστε τους ακόλουθους πόρους:

- Χώρος αποθήκευσης λίμνης Azure δεδομένων (ADLS) 
- Ανάλυση λίμνης Azure δεδομένων (ADLA)
- Λογαριασμός Azure χώρο αποθήκευσης αντικειμένων Blob
- Λογαριασμός Azure μηχανικής εκμάθησης Studio
- Εργαλεία λίμνης Azure δεδομένων για το Visual Studio (συνιστάται)

Αυτή η ενότητα παρέχει οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε κάθε έναν από αυτούς τους πόρους. Εάν επιλέξετε να χρησιμοποιήσετε Hive πίνακες με Azure μηχανικής εκμάθησης, αντί να Python, για να δημιουργήσετε ένα μοντέλο, που θα πρέπει επίσης να παρέχετε ένα σύμπλεγμα HDInsight (Hadoop). Αυτή η διαδικασία εναλλακτικές στο περιγράφεται στην κατάλληλη ενότητα παρακάτω.
<br/>
>AZURE. ΣΗΜΕΊΩΣΗ το **Χώρο αποθήκευσης λίμνης Azure δεδομένων** μπορεί να δημιουργηθεί ένα ξεχωριστά ή όταν δημιουργείτε την **Ανάλυση λίμνης Azure δεδομένων** ως το προεπιλεγμένο αποθήκευσης. Αναφέρονται οδηγίες για τη δημιουργία κάθε έναν από αυτούς τους πόρους ξεχωριστά παρακάτω, αλλά το λογαριασμό χώρου αποθήκευσης δεδομένων λίμνης δεν πρέπει να δημιουργηθούν ξεχωριστά.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Δημιουργία ενός χώρου αποθήκευσης λίμνης δεδομένων Azure

Δημιουργήστε μια ADLS από την [πύλη του Azure](http://portal.azure.com). Για λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης με Azure πύλη](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Φροντίστε να ρυθμίσετε την ταυτότητα AAD σύμπλεγμα στην το blade **προέλευση δεδομένων** από την **Προαιρετική ρύθμιση παραμέτρων** blade περιγράφεται εκεί. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Δημιουργήστε ένα λογαριασμό Azure δεδομένων λίμνης ανάλυσης
Δημιουργήστε ένα λογαριασμό ADLA από την [Πύλη Azure](http://portal.azure.com). Για λεπτομέρειες, ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Azure ανάλυση λίμνης δεδομένων με την πύλη Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Δημιουργήστε ένα λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure
Δημιουργήστε ένα λογαριασμό χώρο αποθήκευσης αντικειμένων Blob του Azure από την [Πύλη Azure](http://portal.azure.com). Για λεπτομέρειες, ανατρέξτε στο θέμα Δημιουργία μια ενότητα για το λογαριασμό χώρου αποθήκευσης στο [Λογαριασμοί Azure σχετικά με το χώρο αποθήκευσης](../storage/storage-create-storage-account.md).
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Ρύθμιση λογαριασμού Azure μηχανικής εκμάθησης Studio
Πραγματοποιήστε είσοδο επάνω/στο Azure μηχανικής εκμάθησης Studio από τη σελίδα [Azure μηχανικής εκμάθησης](https://azure.microsoft.com/services/machine-learning/) . Κάντε κλικ στο κουμπί **Ξεκινήστε τώρα** και, στη συνέχεια, επιλέξτε ένα "Ελεύθερο χώρο εργασίας" ή "Τυπική χώρου εργασίας". Μετά από αυτό θα μπορούν να δημιουργούν δοκιμές στο Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Εγκατάσταση εργαλείων λίμνης Azure δεδομένων [συνιστάται]
Εγκαταστήστε εργαλεία λίμνης Azure δεδομένων για την έκδοση του Visual Studio από τα [Εργαλεία λίμνης δεδομένων Azure για το Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Μετά την ολοκλήρωση της εγκατάστασης με επιτυχία, άνοιγμα του Visual Studio. Θα πρέπει να βλέπετε το λίμνης δεδομένων με το tab στο μενού που βρίσκεται επάνω. Azure τους πόρους σας θα εμφανιστούν στο αριστερό τμήμα του παραθύρου κατά την είσοδο στο λογαριασμό σας στο Azure.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Το σύνολο δεδομένων νέα ΥΌΡΚΗ ταξί ταξίδια
Το σύνολο δεδομένων χρησιμοποιήσαμε εδώ είναι μια διαθέσιμη στο κοινό σύνολο δεδομένων--το [σύνολο δεδομένων ταξίδια ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/). Τα δεδομένα του ταξιδιού ταξί νέα ΥΌΡΚΗ αποτελείται από 20GB περίπου συμπιεσμένα αρχεία CSV (χωρίς συμπίεση ~ 48GB), ταξίδια μεμονωμένα υπερβαίνει 173 εκατομμύρια και το κομίστρων εγγραφών που καταβλήθηκε για κάθε ταξίδι. Κάθε εγγραφή ταξιδιού περιλαμβάνει τις θέσεις απάντηση κλήσης από και απόθεσης και ώρες, anonymized στοιχείο παραβίασης (του προγράμματος οδήγησης) αριθμό αδειών χρήσης και τον αριθμό medallion (μοναδικό αναγνωριστικό του ταξί). Τα δεδομένα καλύπτει όλα ταξίδια κατά το έτος 2013 και παρέχεται με τα παρακάτω δύο σύνολα δεδομένων για κάθε μήνα:

 - Η 'trip_data' CSV περιέχει λεπτομέρειες ταξιδιού, όπως τον αριθμό των προσώπων, απάντηση κλήσης από την και dropoff σημεία, διάρκεια ταξιδιού και μήκους ταξίδι. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - Η 'trip_fare' CSV περιέχει στοιχεία στο ναύλο που καταβάλλεται για κάθε ταξιδιού, όπως τον τύπο πληρωμής, το ποσό ναύλο, πρόσθετης και των φόρων, συμβουλές και διόδια, και το συνολικό ποσό που καταβλήθηκε. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Το μοναδικό κλειδί για να συμμετάσχετε σε ταξίδι\_δεδομένων και ταξιδιού\_ναύλο που αποτελείται από τα ακόλουθα τρία πεδία: medallion, στοιχείο παραβίασης\_αδειών χρήσης και απάντηση κλήσης από την\_ημερομηνίας/ώρας. Τα ανεπεξέργαστα αρχεία CSV είναι προσβάσιμα από μια δημόσια Azure χώρο αποθήκευσης αντικειμένων blob. Η δέσμη ενεργειών U-SQL για αυτό το σύνδεσμο είναι στην ενότητα [συμμετοχή ταξιδιού και ναύλο πίνακες](#join) .

## <a name="process-data-with-u-sql"></a>Επεξεργασία δεδομένων με U-SQL

Οι εργασίες επεξεργασίας δεδομένων απεικονίζονται σε αυτήν την ενότητα περιλαμβάνουν ingesting, ο έλεγχος της ποιότητας, Εξερεύνηση και δειγματοληψία τα δεδομένα. Θα σας δείξουμε επίσης πώς μπορείτε να συμμετάσχετε σε πίνακες ταξιδιού και ναύλο. Το τελικό ενότητα σας δείχνει εκτέλεσης μιας εργασίας δέσμης ενεργειών U-SQL από την πύλη του Azure. Εδώ θα βρείτε συνδέσεις για κάθε δευτερεύουσα ενότητα:

- [Δεδομένα κατάποσης: Διαβάστε στα δεδομένα από δημόσιες blob](#ingest)
- [Έλεγχος ποιότητας δεδομένων](#quality)
- [Εξερεύνηση δεδομένων](#explore)
- [Συμμετοχή σε πίνακες ταξιδιού και ναύλο](#join)
- [Δειγματοληψία δεδομένων](#sample)
- [Εκτέλεση εργασιών U-SQL](#run)

Οι δέσμες ενεργειών U-SQL είναι που περιγράφονται εδώ και που παρέχεται σε ξεχωριστό αρχείο. Μπορείτε να λάβετε τις πλήρεις **δέσμες ενεργειών U SQL** από [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Για να εκτελέσετε U-SQL, Άνοιγμα Visual Studio, κάντε κλικ στην επιλογή **Αρχείο--> Δημιουργία--> έργου**, επιλέξτε **Project U-SQL**, όνομα και αποθηκεύστε το σε ένα φάκελο.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Είναι δυνατό να χρησιμοποιήσουν την πύλη Azure να εκτελέσει U-SQL αντί για το Visual Studio. Μπορείτε να μεταβείτε στον πόρο ανάλυση λίμνης Azure δεδομένων στην πύλη και υποβολή ερωτημάτων απευθείας ως εικονογραφημένες στην παρακάτω εικόνα.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Κατάποσης δεδομένων: Ανάγνωση δεδομένα από δημόσιες blob

Τη θέση των δεδομένων στο το αντικειμένων blob του Azure αναφέρεται ως **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** και μπορεί να εξαχθεί με χρήση **Extractors.Csv()**. Αντικαταστήστε το δικό σας όνομα κοντέινερ και το όνομα λογαριασμού χώρου αποθήκευσης στις ακόλουθες δέσμες ενεργειών για container_name@blob_storage_account_name στη διεύθυνση wasb. Επειδή τα ονόματα αρχείων είναι στην ίδια μορφή, μπορούμε να χρησιμοποιήσουμε **ταξιδιού\_data_ {\*\}.csv** ευανάγνωστο σε όλα τα αρχεία 12 ταξίδι. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Επειδή υπάρχουν κεφαλίδες στην πρώτη γραμμή, πρέπει να καταργήσετε τις κεφαλίδες και να αλλάξετε τους τύπους στηλών στο κατάλληλο από αυτές. Μπορούμε είτε αποθήκευση λογαριασμού χρησιμοποιώντας την επεξεργασία δεδομένων λίμνης αποθήκευσης δεδομένων Azure μέσω **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ ή στο χώρο αποθήκευσης αντικειμένων Blob του Azure **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Ομοίως θα σας να διαβάσετε σε τα σύνολα δεδομένων ναύλο. Κάντε δεξί κλικ χώρου αποθήκευσης λίμνης Azure δεδομένων, μπορείτε να επιλέξετε για να δείτε τα δεδομένα σας στο **Azure πύλη--> Εξερεύνηση δεδομένων** ή στην **Εξερεύνηση αρχείων** στο Visual Studio. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Έλεγχος ποιότητας δεδομένων

Αφού πίνακες ταξιδιού και ναύλο που έχει διαβαστεί σε, μπορεί να γίνει έλεγχος ποιότητας δεδομένων με τον εξής τρόπο. Τα αρχεία CSV που προκύπτουν μπορεί να είναι εξόδου σε χώρο αποθήκευσης αντικειμένων Blob του Azure ή Azure δεδομένων λίμνης Store. 

Βρείτε τον αριθμό των medallions και μοναδικό αριθμός medallions:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Βρείτε αυτές τις medallions που είχαν λιγότερα από 100 ταξίδια:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Βρείτε αυτές τις εγγραφές δεν είναι έγκυρη όσον αφορά pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Βρείτε τις τιμές που λείπουν για ορισμένες μεταβλητές:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Εξερεύνηση δεδομένων

Μπορούμε να κάνουμε ορισμένα Εξερεύνηση δεδομένων για να κατανοήσετε καλύτερα τα δεδομένα.

Βρείτε την κατανομή των με τμήμα στην άκρη και μη επικαλυπτόμενα ταξίδια:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Βρείτε την κατανομή του ποσού συμβουλή με τιμές αποκοπής: 0,5,10 και 20 ευρώ.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Βρείτε βασικές στατιστικά στοιχεία του ταξιδιού απόσταση:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Βρείτε το εκατοστημόριου της απόστασης αποστολής και επιστροφής:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Συμμετοχή σε πίνακες ταξιδιού και ναύλο

Ταξίδι και ναύλο που μπορούν να συνδεθούν μεταξύ medallion, hack_license και pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Για κάθε επίπεδο count επιβατών, υπολογίστε τον αριθμό των εγγραφών, το ποσό average συμβουλή, διακύμανση του ποσού συμβουλή, ποσοστό του με τμήμα στην άκρη ταξίδια.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Δειγματοληψία δεδομένων

Θα σας τυχαία επιλέξτε πρώτα 0,1% των δεδομένων από τον συνδεδεμένο πίνακα:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Στη συνέχεια, μπορούμε να κάνουμε των δειγματοληψία από δυαδικό μεταβλητής tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Εκτέλεση εργασιών U-SQL

Όταν ολοκληρώσετε την επεξεργασία δέσμης ενεργειών U-SQL, μπορείτε να υποβάλετε τους στο διακομιστή χρησιμοποιώντας το λογαριασμό σας Azure δεδομένων λίμνης ανάλυσης. Κάντε κλικ στην επιλογή **Λίμνης δεδομένων**, **Υποβολή εργασίας**, επιλέξτε το **Λογαριασμό ανάλυση**, επιλέξτε **παραλληλισμό**και κάντε κλικ στο κουμπί " **Υποβολή** ".  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Όταν η εργασία είναι συμμόρφωση με επιτυχία, η κατάσταση της εργασίας σας θα εμφανιστεί στο Visual Studio για την παρακολούθηση. Αφού ολοκληρωθεί η εκτέλεση της εργασίας, να ακόμα και επανάληψη αναπαραγωγής της διαδικασίας εκτέλεσης έργων και να βρείτε τα βήματα συμφόρηση για τη βελτίωση της αποτελεσματικότητας της εργασίας. Μπορείτε επίσης να μεταβείτε στην πύλη Azure για να ελέγξετε την κατάσταση των εργασιών σας U-SQL.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Τώρα μπορείτε να ελέγξετε τα αρχεία εξόδου στο χώρο αποθήκευσης αντικειμένων Blob του Azure ή Azure πύλη. Θα χρησιμοποιήσουμε το δείγμα των δεδομένων για μας μοντελοποίηση στο επόμενο βήμα.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Δημιουργήστε και αναπτύξτε μοντέλα σε Azure μηχανικής εκμάθησης

Θα δείξουμε δύο επιλογές που είναι διαθέσιμες για την έλξη δεδομένων στο Azure μηχανικής εκμάθησης για να δημιουργήσετε και 

- Από την πρώτη επιλογή, που χρησιμοποιούν τα δεδομένα δείγματος που έχει εγγραφεί σε μια αντικειμένων Blob του Azure (σε **δειγματοληψία δεδομένων** παραπάνω βήμα) και χρησιμοποιήστε Python Δημιουργήστε και αναπτύξτε μοντέλα από το Azure μηχανικής εκμάθησης. 
- Στο η δεύτερη επιλογή, ερωτήματος τα δεδομένα στο Azure δεδομένων λίμνης απευθείας με χρήση ενός ερωτήματος ομάδας. Αυτή η επιλογή απαιτεί που Δημιουργήστε ένα νέο σύμπλεγμα HDInsight ή χρησιμοποιήστε ένα υπάρχον σύμπλεγμα HDInsight όπου τους πίνακες Hive σημείο στα δεδομένα ταξί NY στο χώρο αποθήκευσης δεδομένων λίμνης Azure.  Αναλύονται και οι δύο αυτές τις παρακάτω επιλογές. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Επιλογή 1: Χρήση Python για τη δημιουργία και ανάπτυξη υπολογιστή μοντέλα εκμάθησης

Για να δημιουργήσετε και να αναπτύξετε μηχανικής εκμάθησης μοντέλων χρησιμοποιώντας Python, δημιουργήστε ένα σημειωματάριο Jupyter στον τοπικό υπολογιστή σας ή στο Azure μηχανικής εκμάθησης Studio. Το Σημειωματάριο Jupyter που παρέχονται στη [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) περιέχει τον πλήρη κώδικα για την Εξερεύνηση, απεικόνιση δεδομένων, η δυνατότητα μηχανικής, μοντελοποίηση και ανάπτυξη. Σε αυτό το άρθρο, θα σας δείξουμε τα μοντελοποίηση και ανάπτυξη. 

### <a name="import-python-libraries"></a>Εισαγωγή Python βιβλιοθήκες

Για να εκτελέσετε το δείγμα Jupyter Σημειωματάριο ή το Python δέσμη ενεργειών αρχείο, το παρακάτω Python πακέτων είναι απαραίτητες. Εάν χρησιμοποιείτε την υπηρεσία AzureML σημειωματαρίου, αυτά τα πακέτα έχουν προ-εγκατεστημένες.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Διαβάστε τα δεδομένα από blob

- Συμβολοσειρά σύνδεσης   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Διαβάστε στην ως κείμενο

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Προσθέστε τα ονόματα στηλών και διαχωρίστε τις στήλες

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Αλλαγή ορισμένων στηλών σε αριθμητική

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Δημιουργία μηχανικής εκμάθησης μοντέλων

Δείτε εδώ μπορέσουμε να δημιουργήσουμε ένα μοντέλο δυαδικό ταξινόμηση πρόβλεψη εάν ένα ταξίδι είναι επικαλυπτόμενα ή όχι. Στο Σημειωματάριο Jupyter μπορείτε να βρείτε τις άλλες δύο μοντέλων: multiclass κατάταξη, καθώς και τα μοντέλα παλινδρόμησης.

- Πρώτα πρέπει να δημιουργήσετε εικονική μεταβλητές που μπορούν να χρησιμοποιηθούν σε scikit-μάθετε μοντέλα

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Δημιουργία πλαισίου δεδομένων για το μοντελοποίηση

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Εκπαίδευση και έλεγχος 60-40 διαίρεσης

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Εφοδιαστική παλινδρόμησης στο σύνολο εκπαίδευσης

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Βαθμολογία δοκιμών συνόλου δεδομένων

        Y_test_pred = logit_fit.predict(X_test)

- Υπολογισμός μετρικά αξιολόγησης

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Δημιουργήστε το API της υπηρεσίας Web και την κατανάλωση στο Python

Θέλουμε να operationalize το μηχανικής εκμάθησης μοντέλο αφού έχει δημιουργηθεί. Εδώ χρησιμοποιούμε δυαδικό εφοδιαστική μοντέλου ως παράδειγμα. Βεβαιωθείτε ότι το scikit-μάθετε έκδοση στον τοπικό υπολογιστή σας είναι 0.15.1. Δεν χρειάζεται να ανησυχείτε σχετικά με αυτό, εάν χρησιμοποιείτε την υπηρεσία studio Azure ML.

- Βρείτε τα διαπιστευτήριά σας χώρου εργασίας από τις ρυθμίσεις studio Azure ML. Στο Azure μηχανικής εκμάθησης Studio, κάντε κλικ στην επιλογή **Ρυθμίσεις** --> **όνομα** --> **Διακριτικά εξουσιοδότησης**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Δημιουργία της υπηρεσίας Web

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Λάβετε τα διαπιστευτήρια υπηρεσίας web

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Καλέστε το API της υπηρεσίας Web. Πρέπει να περιμένετε 5-10 δευτερόλεπτα μετά το προηγούμενο βήμα.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Επιλογή 2: Δημιουργία και ανάπτυξη των μοντέλων απευθείας στο Azure μηχανικής εκμάθησης

Azure μηχανικής εκμάθησης Studio μπορεί να διαβάζει δεδομένα απευθείας από το χώρο αποθήκευσης λίμνης Azure δεδομένων και, στη συνέχεια, να χρησιμοποιηθεί για να δημιουργήσετε και να αναπτύξετε τα μοντέλα. Αυτή η προσέγγιση χρησιμοποιεί έναν πίνακα Hive που οδηγεί στο χώρο αποθήκευσης λίμνης Azure δεδομένων. Αυτή η ενέργεια απαιτεί ότι η προμήθεια ένα ξεχωριστό σύμπλεγμα Azure HDInsight, στην οποία δημιουργείται στον πίνακα ομάδα. Οι ενότητες που ακολουθούν δείχνουν πώς μπορείτε να το κάνετε αυτό. 

### <a name="create-an-hdinsight-linux-cluster"></a>Δημιουργήστε ένα σύμπλεγμα Linux HDInsight

Δημιουργήστε ένα σύμπλεγμα HDInsight (Linux) από την [πύλη του Azure](http://portal.azure.com). Για λεπτομέρειες, ανατρέξτε στην ενότητα **Δημιουργία ένα σύμπλεγμα HDInsight με την πρόσβαση στο χώρο αποθήκευσης λίμνης δεδομένων Azure** [Δημιουργία ένα σύμπλεγμα HDInsight με το χώρο αποθήκευσης λίμνης δεδομένων](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)χρησιμοποιώντας την πύλη Azure.

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Δημιουργία πίνακα Hive στο HDInsight

Τώρα μπορούμε να δημιουργήσουμε Hive πίνακες που θα χρησιμοποιηθούν στο Azure μηχανικής εκμάθησης Studio στο σύμπλεγμα HDInsight χρησιμοποιώντας τα δεδομένα που είναι αποθηκευμένα στο χώρο αποθήκευσης λίμνης δεδομένων Azure στο προηγούμενο βήμα. Μεταβείτε στο σύμπλεγμα HDInsight που μόλις δημιουργήσατε. Κάντε κλικ στην επιλογή **Ρυθμίσεις** --> **Ιδιότητες** --> **Σύμπλεγμα AAD ταυτότητας** --> **ADLS πρόσβασης**, βεβαιωθείτε ότι ο λογαριασμός σας χώρου αποθήκευσης λίμνης δεδομένων Azure προστίθεται στη λίστα με την ανάγνωση, εγγραφή και εκτέλεση δικαιωμάτων. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Στη συνέχεια, κάντε κλικ στην επιλογή **Πίνακας εργαλείων** δίπλα στο κουμπί **Ρυθμίσεις** και θα εμφανιστεί ένα παράθυρο. Κάντε κλικ στην επιλογή **Προβολή Hive** στην επάνω δεξιά γωνία της σελίδας και που θα βλέπουν το **Πρόγραμμα επεξεργασίας ερωτήματος**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Επικολλήστε τις παρακάτω δέσμες ενεργειών ομάδα για να δημιουργήσετε έναν πίνακα. Τη θέση του αρχείου προέλευσης δεδομένων που βρίσκεται στο χώρο αποθήκευσης λίμνης Azure δεδομένων αναφοράς με αυτόν τον τρόπο: **adl://data_lake_store_name.azuredatalakestore.net:443/όνομα_φακέλου/όνομα_αρχείου**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Όταν ολοκληρωθεί η εκτέλεση του ερωτήματος, θα δείτε τα αποτελέσματα ως εξής:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Δημιουργήστε και αναπτύξτε μοντέλα στο Azure μηχανικής εκμάθησης Studio

Συνεχίζουμε να τώρα είστε έτοιμοι να δημιουργήσετε και να αναπτύξετε ένα μοντέλο που προβλέπει ή όχι η πληρωμή συμβουλή με Azure μηχανικής εκμάθησης. Το δείγμα των δεδομένων είναι έτοιμη για χρήση σε δυαδικό η διαβάθμιση (συμβουλή ή όχι) το πρόβλημα. Τα μοντέλα πρόβλεψης χρησιμοποιώντας multiclass διαβάθμιση (tip_class) και παλινδρόμησης (tip_amount) επίσης μπορεί να δημιουργηθεί και να αναπτυχθεί με Azure μηχανικής εκμάθησης Studio, αλλά εδώ θα σας δείξουμε μόνο τον τρόπο χειρισμού των πεζών και κεφαλαίων γραμμάτων με χρήση του μοντέλου δυαδικό ταξινόμηση.

1. Μεταφέρετε τα δεδομένα στο Azure ML με **Εισαγωγή δεδομένων** λειτουργική μονάδα, διαθέσιμο στην ενότητα **δεδομένα εισόδου και εξόδου** . Για περισσότερες πληροφορίες, ανατρέξτε στη σελίδα αναφοράς [λειτουργικής μονάδας εισαγωγή δεδομένων](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) .
2. Επιλέξτε **Την ομάδα ερωτήματος** ως **προέλευσης δεδομένων** στον πίνακα " **Ιδιότητες** ".
3. Επικολλήστε την ακόλουθη δέσμη ενεργειών Hive στο πρόγραμμα επεξεργασίας **Hive ερώτημα βάσης δεδομένων**

        select * from nyc_stratified_sample;

4. Εισαγάγετε τη σύμπλεγμα URI του HDInsight (αυτό βρίσκεται στην πύλη Azure), τα διαπιστευτήρια Hadoop, θέση δεδομένα εξόδου και όνομα κοντέινερ/κλειδιού/όνομα λογαριασμού Azure χώρου αποθήκευσης.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Παράδειγμα μιας έρευνας δυαδικό ταξινόμηση την ανάγνωση δεδομένων από πίνακα "Hive" εμφανίζεται στην παρακάτω εικόνα.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Αφού δημιουργήσετε την έρευνα, κάντε κλικ στην επιλογή **Ορισμός υπηρεσίας Web** --> **Πρόβλεψης υπηρεσίας Web**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Εκτέλεση δημιουργείται αυτόματα βαθμολογίας έρευνας, όταν ολοκληρωθεί, κάντε κλικ στην επιλογή **Ανάπτυξη υπηρεσίας Web**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Ο πίνακας εργαλείων υπηρεσίας web θα εμφανίζονται λίγο:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Σύνοψη

Κατά την ολοκλήρωση αυτόν τον Οδηγό έχετε δημιουργήσει ένα περιβάλλον science δεδομένων για τη δημιουργία μεταβλητού μεγέθους για ολοκληρωμένες λύσεις στο Azure λίμνης δεδομένων. Αυτό το περιβάλλον που χρησιμοποιήθηκε για την ανάλυση ένα μεγάλο δημόσια σύνολο δεδομένων, μεταφέρετε τα κανονικής βήματα της διαδικασίας Science δεδομένων, από απόκτηση δεδομένων μέσω του μοντέλου εκπαίδευση και, στη συνέχεια, την ανάπτυξη του μοντέλου ως υπηρεσία web. U-SQL που χρησιμοποιήθηκε για την επεξεργασία, εξερευνήστε και ακούστε τα δείγματα δεδομένων. Python και Hive χρησιμοποιήθηκαν με Azure μηχανικής εκμάθησης Studio για τη δημιουργία και ανάπτυξη πρόβλεψης μοντέλα.

## <a name="whats-next"></a>Τι ακολουθεί;

Η διαδρομή εκμάθησης για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](http://aka.ms/datascienceprocess) παρέχει συνδέσεις σε θέματα που περιγράφει κάθε βήμα της διεργασίας προηγμένη ανάλυση. Υπάρχουν μια σειρά από αναλυτικές αναλυτικών στη σελίδα [διαδικασία Science δεδομένων ομάδας αναλυτικές παρουσιάσεις](data-science-process-walkthroughs.md) που επιδεικνύουν πώς μπορείτε να χρησιμοποιείτε τους πόρους και τις υπηρεσίες σε διάφορα σενάρια πρόβλεψης αναλυτικών στοιχείων:

- [Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση αποθήκη δεδομένων του SQL](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση HDInsight Hadoop συμπλεγμάτων](machine-learning-data-science-process-hive-walkthrough.md)
- [Η διαδικασία Science δεδομένων ομάδας: χρήση του SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Επισκόπηση της διαδικασίας Science δεδομένων χρησιμοποιώντας τους στο Azure HDInsight](machine-learning-data-science-spark-overview.md)


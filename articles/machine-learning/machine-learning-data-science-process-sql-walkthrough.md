<properties
    pageTitle="Τη διαδικασία της επιστήμης δεδομένων ομάδας στην ενέργεια: χρησιμοποιώντας τον SQL Server | Microsoft Azure"
    description="Διαδικασία ανάλυσης για προχωρημένους και την τεχνολογία σε ενέργεια"  
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
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Τη διαδικασία της επιστήμης δεδομένων ομάδας στην ενέργεια: χρησιμοποιώντας τον SQL Server

Η εκμάθηση, που αναλυτικής παρουσίασης δημιουργία και ανάπτυξη ένα μοντέλο υπολογιστή εκμάθηση χρησιμοποιώντας SQL Server και ένα dataset διαθέσιμων--το dataset [Trips ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) . Η διαδικασία ακολουθεί μια ροή εργασίας επιστήμης κανονικά δεδομένα: ingest και Εξερευνήστε τα δεδομένα, μηχανικός δυνατότητες για να διευκολυνθεί η εκμάθηση, και στη συνέχεια να δημιουργήσετε και να αναπτύξετε ένα μοντέλο.


## <a name="dataset"></a>Νέα ΥΌΡΚΗ ταξί Trips περιγραφή συνόλου δεδομένων

Τα δεδομένα νέα ΥΌΡΚΗ ταξί ταξίδι είναι περίπου 20GB συμπιεσμένα αρχεία CSV (χωρίς συμπίεση ~ 48GB), που αποτελείται από περισσότερα από 173 εκατομμύρια μεμονωμένα ταξίδια και τα κόμιστρα που καταβάλλεται για κάθε ταξίδι. Κάθε εγγραφή ταξίδι περιλαμβάνει το αγροτικό και απόθεσης θέση και ώρα, anonymized εισβολή (οδήγησης) αριθμό άδειας χρήσης και αριθμό medallion (μοναδικό αναγνωριστικό του ταξί). Τα δεδομένα που καλύπτει όλα τα ταξίδια του έτους 2013 και παρέχεται με τα ακόλουθα δύο σύνολα δεδομένων για κάθε μήνα:

1. Το 'trip_data' CSV περιέχει λεπτομέρειες ταξίδι, όπως ο αριθμός των επιβατών, αγροτικό και σημεία dropoff, ταξίδι διάρκειας και μήκος διαδρομής. Ακολουθούν μερικά δείγματα εγγραφών:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'trip_fare' CSV περιέχει λεπτομέρειες σχετικά με το ναύλο που καταβλήθηκε για κάθε ταξίδι, όπως τον τύπο πληρωμής, ποσό ναύλο, επιβάρυνση και φόροι, συμβουλές και διόδια, και το συνολικό ποσό που καταβλήθηκε. Ακολουθούν μερικά δείγματα εγγραφών:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Το μοναδικό κλειδί για να συμμετάσχετε σε ταξίδι\_δεδομένων και ταξίδι\_ναύλο που αποτελείται από τα πεδία: medallion, εισβολή\_άδειας και αγροτικό\_ημερομηνίας/ώρας.

## <a name="mltasks"></a>Παραδείγματα εργασιών πρόβλεψης

Εμείς θα διατυπώσει τρία προβλήματα πρόβλεψης, με βάση το *Συμβουλή\_ποσό*, ήτοι:

1. Δυαδική ταξινόμηση: πρόβλεψης ή όχι μια συμβουλή που έχει καταβληθεί για ένα ταξίδι, δηλαδή μια *Συμβουλή\_ποσό* που είναι μεγαλύτερη από $0 είναι ένα θετικό παράδειγμα, ενώ ένα *Συμβουλή\_ποσό* $ 0 είναι ένα αρνητικό παράδειγμα.

2. Ταξινόμηση multiclass: για να προβλέψετε την περιοχή του άκρου που καταβάλλεται για το ταξίδι σας. Εμείς διαιρέστε το *Συμβουλή\_ποσό* σε πέντε θέσεις αποθήκης ή των κατηγοριών:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Εργασία παλινδρόμησης: για να προβλέψετε το ποσό του άκρου που καταβάλλεται για ένα ταξίδι.  


## <a name="setup"></a>Ρύθμιση ασφαλείας του Azure περιβάλλον επιστήμη δεδομένων για ανάλυση για προχωρημένους

Όπως μπορείτε να δείτε από τον οδηγό [Σχεδιασμός περιβάλλον σας](machine-learning-data-science-plan-your-environment.md) , υπάρχουν διάφορες επιλογές για να εργαστείτε με το dataset Trips ταξί νέα ΥΌΡΚΗ στο Azure:

- Εργασία με τα δεδομένα των BLOB Azure και στη συνέχεια το μοντέλο στην εκμάθηση μηχανής Azure
- Η φόρτωση των δεδομένων σε μια βάση δεδομένων SQL Server και στη συνέχεια το μοντέλο στην εκμάθηση μηχανής Azure

Σε αυτό το πρόγραμμα εκμάθησης σας θα δείχνουν παράλληλη μαζικής εισαγωγής των δεδομένων σε ένα διακομιστή SQL, Εξερεύνηση δεδομένων, δυνατότητα μηχανικής και κάτω δειγματοληψία χρησιμοποιώντας SQL Server Management Studio καθώς και χρησιμοποιώντας το Σημειωματάριο IPython. [Δείγματα δεσμών ενεργειών](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) και [IPython φορητοί υπολογιστές](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) είναι κοινόχρηστοι στο GitHub. Ένα δείγμα IPython το Σημειωματάριο για να εργαστείτε με τα δεδομένα των BLOB Azure είναι επίσης διαθέσιμη στην ίδια θέση.

Για να ορίσετε το περιβάλλον Azure επιστημονικά δεδομένα σας:

1. [Δημιουργήστε ένα λογαριασμό αποθήκευσης](../storage/storage-create-storage-account.md)

2. [Δημιουργήστε ένα χώρο εργασίας εκμάθηση μηχανής Azure](machine-learning-create-workspace.md)

3. [Παροχή μιας εικονικής μηχανής επιστημονικά δεδομένα](machine-learning-data-science-setup-sql-server-virtual-machine.md), που θα λειτουργήσει ως διακομιστής SQL ενός διακομιστή IPython το Σημειωματάριο.

    > [AZURE.NOTE] Τα δείγματα δεσμών ενεργειών και σημειωματάρια IPython θα γίνει λήψη για την εικονική μηχανή σας επιστημονικά δεδομένα κατά τη διαδικασία εγκατάστασης. Όταν ολοκληρωθεί η δέσμη ενεργειών μετά την εγκατάσταση του VM, τα δείγματα θα είναι στη βιβλιοθήκη εγγράφων σας VM:  
    > - Δέσμες ενεργειών δείγματος:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Σημειωματάρια IPython δείγμα:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > όπου `<user_name>` είναι το όνομα σύνδεσης των Windows σας VM. Θα αναφερόμαστε στους φακέλους του δείγματος ως **Δείγματα δεσμών ενεργειών** και **Σημειωματάρια IPython δείγματος**.


Ανάλογα με το μέγεθος του dataset, θέση προέλευσης δεδομένων και το περιβάλλον του επιλεγμένου προορισμού Azure, αυτό το σενάριο είναι παρόμοια με [σενάριο \#5: μεγάλο σύνολο δεδομένων σε μια τοπικά αρχεία, επιλέξτε SQL Server σε Azure VM](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Η λήψη των δεδομένων από δημόσια προέλευσης

Για να λάβετε το dataset [Νέα ΥΌΡΚΗ ταξί Trips](http://www.andresmh.com/nyctaxitrips/) από τη δημόσια θέση, μπορείτε να χρησιμοποιήσετε οποιαδήποτε από τις μεθόδους που περιγράφονται σε [Μετακίνηση δεδομένων από και προς το χώρο αποθήκευσης αντικειμένων Blob Azure](machine-learning-data-science-move-azure-blob.md) για να αντιγράψετε τα δεδομένα σας νέα εικονική μηχανή.

Για να αντιγράψετε τα δεδομένα χρησιμοποιώντας AzCopy:

1. Συνδεθείτε με την εικονική μηχανή (VM)

2. Δημιουργία νέου καταλόγου στο δίσκο δεδομένων την εικονική Μηχανή (Σημείωση: Μην χρησιμοποιείτε το στον προσωρινό δίσκο που συνοδεύει την εικονική Μηχανή με ένα δίσκο δεδομένων).

3. Σε ένα παράθυρο εντολών, εκτελέστε την ακόλουθη γραμμή εντολών Azcopy, αντικαθιστώντας το < path_to_data_folder > με το φάκελο δεδομένων που δημιουργήσατε στο (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Όταν ολοκληρωθεί η AzCopy, συνολικά 24 συμπιεσμένα αρχεία CSV (12 για το ταξίδι\_δεδομένων και 12 για το ταξίδι\_ναύλο) θα πρέπει να είναι στο φάκελο δεδομένων.

4. Αποσυμπιέστε τα αρχεία που έχουν ληφθεί. Σημειώστε το φάκελο όπου βρίσκονται τα μη συμπιεσμένα αρχεία. Αυτός ο φάκελος θα αναφέρεται ως η < διαδρομή\_σε\_δεδομένων\_αρχεία\>.

## <a name="dbload"></a>Μαζική εισαγωγή δεδομένων σε βάση δεδομένων του SQL Server

Μπορούν να βελτιωθούν οι επιδόσεις κατά τη φόρτωση/μεταφορά μεγάλων ποσοτήτων δεδομένων σε μια βάση δεδομένων SQL και επόμενα ερωτήματα χρησιμοποιώντας _διαμερίσματα πίνακες και προβολές_. Σε αυτήν την ενότητα, εμείς θα ακολουθήσει τις οδηγίες που περιγράφονται στην [Παράλληλη μαζικής εισαγωγής χρησιμοποιώντας SQL διαμέρισμα πίνακες δεδομένων](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) για να δημιουργήσετε μια νέα βάση δεδομένων και η φόρτωση των δεδομένων σε διαμερίσματα πίνακες παράλληλα.

1. Ενώ είστε συνδεδεμένοι με το VM, ξεκινήστε το **SQL Server Management Studio**.

2. Σύνδεση με χρήση ελέγχου ταυτότητας των Windows.

    ![SSMS σύνδεση][12]

3. Εάν δεν έχει ακόμα έχετε αλλάξει τη λειτουργία ελέγχου ταυτότητας του SQL Server και δημιουργείται ένα νέο χρήστη σύνδεσης SQL, ανοίξτε το αρχείο δέσμης ενεργειών με το όνομα **Αλλαγή\_auth.sql** στο φάκελο **Δείγματα δεσμών ενεργειών** . Για να αλλάξετε το προεπιλεγμένο όνομα χρήστη και κωδικό πρόσβασης. Κάντε κλικ στο κουμπί **! Εκτέλεση** στη γραμμή εργαλείων για να εκτελέσετε τη δέσμη ενεργειών.

    ![Εκτέλεση δέσμης ενεργειών][13]

4. Επιβεβαιώστε ή/και αλλαγή των SQL Server προεπιλογή βάσης δεδομένων και καταγραφής φακέλων για να εξασφαλίσετε που βάσεις δεδομένων που δημιουργήθηκαν πρόσφατα θα αποθηκευτούν σε ένα δίσκο δεδομένων. Το είδωλο SQL Server VM που έχει βελτιστοποιηθεί για datawarehousing φορτία είναι ήδη ρυθμισμένο με δίσκους δεδομένων και καταγραφής. Εάν σας VM δεν συμπεριλάβατε ένα δίσκο δεδομένων και προσθήκη νέων εικονικών σκληρών δίσκων κατά τη διαδικασία εγκατάστασης VM, αλλάξτε τους προεπιλεγμένους φακέλους ως εξής:

    - Κάντε δεξιό κλικ στο όνομα του SQL Server στον αριστερό πίνακα και κάντε κλικ στο κουμπί **Ιδιότητες**.

        ![Ιδιότητες διακομιστή SQL][14]

    - Επιλέξτε τις **Ρυθμίσεις της βάσης δεδομένων** από τη λίστα **Επιλέξτε μια σελίδα** προς τα αριστερά.

    - Επιβεβαιώσετε ή/και να αλλάξετε τις **προεπιλεγμένες θέσεις βάσης δεδομένων** σε **Δίσκο δεδομένων** τοποθεσίες της επιλογής σας. Αυτό είναι όπου βρίσκονται νέες βάσεις δεδομένων, αν δημιουργηθεί με τις προεπιλεγμένες ρυθμίσεις του θέση.

        ![Προεπιλεγμένες ρυθμίσεις βάσης δεδομένων SQL][15]  

5. Για να δημιουργήσετε μια νέα βάση δεδομένων και ένα σύνολο filegroups για τη διατήρηση των πινάκων διαμερίσματα, ανοίξτε το δείγμα δέσμης ενεργειών **Δημιουργία\_db\_default.sql**. Η δέσμη ενεργειών θα δημιουργήσει μια νέα βάση δεδομένων με το όνομα **TaxiNYC** και 12 filegroups στην προεπιλεγμένη θέση δεδομένων. Κάθε ομάδα αρχείων θα περιέχει ένα μήνα το ταξίδι\_δεδομένων και ταξίδι\_ναύλο δεδομένων. Τροποποιήστε το όνομα της βάσης δεδομένων, εάν το επιθυμείτε. Κάντε κλικ στο κουμπί **! Εκτέλεση** για να εκτελέσετε τη δέσμη ενεργειών.

6. Στη συνέχεια, δημιουργήστε δύο πίνακες διαμερισμάτων, μία για το ταξίδι σας\_δεδομένων και έναν άλλο για το ταξίδι σας\_ναύλο. Ανοίξτε το δείγμα δέσμης ενεργειών **Δημιουργία\_διαμερίσματα\_table.sql**, που θα:

    - Για να δημιουργήσετε μια συνάρτηση διαμέρισμα για να διαιρέσετε τα δεδομένα κατά μήνα.
    - Για να δημιουργήσετε ένα σχήμα διαμερίσματος για την αντιστοίχιση δεδομένων κάθε μήνα σε διαφορετική ομάδα αρχείων.
    - Δημιουργήστε δύο διαμερίσματα πίνακες που έχει αντιστοιχιστεί στο συνδυασμό διαμερισμάτων: **nyctaxi\_ταξίδι** θα περιέχει το ταξίδι\_δεδομένων και **nyctaxi\_ναύλο που** θα περιέχει το ταξίδι\_ναύλο δεδομένων.

    Κάντε κλικ στο κουμπί **! Εκτέλεση** για να εκτελέσετε τη δέσμη ενεργειών και να δημιουργήσετε διαμερίσματα πίνακες.

7. Στο φάκελο " **Δείγματα δεσμών ενεργειών** ", υπάρχουν δύο δέσμες ενεργειών PowerShell δείγμα που αποδεικνύουν την παράλληλη μαζικά την εισαγωγή δεδομένων σε πίνακες του SQL Server.

    - **bcp\_παράλληλη\_generic.ps1** είναι μια δέσμη ενεργειών γενικής χρήσης παράλληλης μαζική εισαγωγή δεδομένων σε έναν πίνακα. Για να τροποποιήσετε αυτήν τη δέσμη ενεργειών για να ορίσετε τις μεταβλητές εισόδου και προορισμού, όπως υποδεικνύεται στις γραμμές σχολίου στη δέσμη ενεργειών.
    - **bcp\_παράλληλη\_nyctaxi.ps1** είναι μια προ-ρυθμισμένη έκδοση της γενικής χρήσης δέσμης ενεργειών και μπορεί να χρησιμοποιηθεί για να φορτώσετε και τους δύο πίνακες για τα νέα ΥΌΡΚΗ ταξί Trips δεδομένα.  

8. Κάντε δεξιό κλικ το **bcp\_παράλληλη\_nyctaxi.ps1** ονόματος για δέσμες ενεργειών και κάντε κλικ στο κουμπί " **Επεξεργασία** " για να την ανοίξετε σε PowerShell. Ανασκόπηση των προκαθορισμένων μεταβλητών και τροποποίηση σύμφωνα με το όνομα της επιλεγμένης βάσης δεδομένων, φάκελο εισαγωγής δεδομένων, φάκελο καταγραφής προορισμού και διαδρομές προς τα αρχεία μορφής δείγμα **nyctaxi_trip.xml** και **nyctaxi\_fare.xml** (που αναφέρεται στο φάκελο **Δείγματα δεσμών ενεργειών** ).

    ![Μαζική εισαγωγή δεδομένων][16]

    Μπορείτε επίσης να επιλέξετε τη λειτουργία ελέγχου ταυτότητας, η προεπιλογή είναι έλεγχος ταυτότητας των Windows. Κάντε κλικ στο πράσινο βέλος στη γραμμή εργαλείων για να εκτελέσετε. Η δέσμη ενεργειών θα ξεκινήσει 24 τις λειτουργίες εισαγωγής μαζικής παράλληλη 12 για κάθε πίνακα διαμερίσματα. Ενδέχεται να μπορείτε να παρακολουθείτε την πρόοδο της εισαγωγής δεδομένων, ανοίγοντας το προεπιλεγμένο φάκελο δεδομένων του SQL Server, όπως παραπάνω.

9. Η δέσμη ενεργειών PowerShell αναφέρει τις ημερομηνίες έναρξης και λήξης ωρών. Όταν όλα μαζικές εισαγωγές ολοκληρωθεί, αναφέρει την ώρα λήξης. Ελέγξτε το αρχείο καταγραφής προορισμού για να επαληθεύσετε ότι οι μαζικές εισαγωγές ήταν επιτυχημένα, δηλαδή, δεν αναφέρθηκαν σφάλματα στο αρχείο καταγραφής φάκελο προορισμού.

10. Τη βάση δεδομένων σας είναι τώρα έτοιμος για Εξερεύνηση, δυνατότητα μηχανικής και άλλες λειτουργίες όπως επιθυμείτε. Δεδομένου ότι οι πίνακες έχουν χωριστεί σε διαμερίσματα σύμφωνα με το **αγροτικό\_ημερομηνίας/ώρας** πεδίο, ερωτήματα που περιλαμβάνουν **αγροτικό\_ημερομηνίας/ώρας** συνθήκες στον όρο **όπου** θα ωφεληθούν από το σχήμα του διαμερίσματος.

11. Στο **SQL Server Management Studio**, εξερευνήστε την παρεχόμενη δέσμη ενεργειών **δείγμα\_queries.sql**. Για να εκτελέσετε κάποιο από τα ερωτήματα στο δείγμα, επισημάνετε τις γραμμές ερωτήματος και στη συνέχεια κάντε κλικ στο κουμπί **! Εκτέλεση** στη γραμμή εργαλείων.

12. Τα δεδομένα νέα ΥΌΡΚΗ ταξί Trips φορτώνεται σε δύο διαφορετικούς πίνακες. Για να βελτιώσετε τις λειτουργίες συνδέσμου, συνιστάται ιδιαίτερα να δημιουργήσετε ευρετήριο των πινάκων. Το δείγμα δέσμης ενεργειών **Δημιουργία\_διαμερίσματα\_index.sql** δημιουργεί διαμερίσματα ευρετήρια σχετικά με το κλειδί συνδέσμου σύνθετη **medallion, εισβολή\_άδεια χρήσης και αγροτικό\_ημερομηνίας/ώρας**.

## <a name="dbexplore"></a>Εξερεύνηση δεδομένων και τη δυνατότητα μηχανική στον SQL Server

Σε αυτήν την ενότητα, θα πραγματοποιούμε διερεύνησης δεδομένων και τη δυνατότητα δημιουργίας με την εκτέλεση ερωτημάτων SQL απευθείας στο **SQL Server Management Studio** χρησιμοποιώντας τη βάση δεδομένων SQL Server που δημιουργήσατε νωρίτερα. Ένα δείγμα δέσμης ενεργειών με το όνομα **δείγμα\_queries.sql** παρέχεται στο φάκελο **Δείγματα δεσμών ενεργειών** . Τροποποιήστε τη δέσμη ενεργειών για να αλλάξετε το όνομα της βάσης δεδομένων, εάν είναι διαφορετική από την προεπιλογή: **TaxiNYC**.

Σε αυτή την άσκηση, θα:

- Η σύνδεση με τον **SQL Server Management Studio** χρησιμοποιώντας είτε έλεγχος ταυτότητας των Windows ή χρησιμοποιώντας τον έλεγχο ταυτότητας SQL και το όνομα σύνδεσης σε SQL και κωδικό πρόσβασης.
- Εξερευνήστε κατανομή δεδομένων λίγα πεδία σε διάφορα των windows.
- Εξετάστε την ποιότητα των δεδομένων των πεδίων γεωγραφικού πλάτους και μήκους.
- Δημιουργία ετικετών δυαδικό και multiclass ταξινόμηση με βάση το **Συμβουλή\_ποσό**.
- Δημιουργία δυνατοτήτων και compute/σύγκριση αποστάσεων ταξίδι.
- Συνδέστε τους δύο πίνακες και να εξαγάγετε ένα τυχαίο δείγμα που θα χρησιμοποιηθεί για τη δημιουργία μοντέλων.

Όταν είστε έτοιμοι να συνεχίσετε στην εκμάθηση μηχανής Azure, μπορείτε μπορεί:  

1. Αποθηκεύστε το τελικό ερώτημα SQL για να εξαγάγετε και δείγματα δεδομένων και αντιγραφή-επικόλληση το ερώτημα απευθείας σε μια [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα στην εκμάθηση μηχανής Azure, ή
2. Διατήρηση του δείγματος και τεχνολογικών δεδομένων που σκοπεύετε να χρησιμοποιήσετε για μοντέλο που δημιουργείτε σε ένα νέο πίνακα της βάσης δεδομένων και να χρησιμοποιήσετε τον νέο πίνακα με την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα στην εκμάθηση μηχανής Azure.

Σε αυτήν την ενότητα θα μπορούμε να αποθηκεύσουμε το τελικό ερώτημα για να εξαγάγετε και δείγμα των δεδομένων. Η δεύτερη μέθοδος έχει αποδειχθεί στην ενότητα [δεδομένων εξερευνήσεων και δυνατότητα μηχανικής σε φορητό υπολογιστή IPython](#ipnb) .

Για μια γρήγορη επιβεβαίωση του αριθμού των γραμμών και των στηλών στους πίνακες συμπληρώνεται προηγουμένως χρησιμοποιώντας παράλληλη μαζικής εισαγωγής,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>ΑΝΑΖΗΤΗΣΗ: Ταξίδι διανομής από medallion

Αυτό το παράδειγμα προσδιορίζει το medallion (αριθμοί ταξί) με περισσότερα από 100 trips μέσα σε μια δεδομένη χρονική περίοδο. Το ερώτημα θα μπορούσαν να ωφεληθούν από την πρόσβαση διαμερίσματα πίνακα εφόσον αυτό είναι εξαρτηθεί από το συνδυασμό διαμερισμάτων του **αγροτικό\_ημερομηνίας/ώρας**. Το πλήρες σύνολο δεδομένων κατά την υποβολή ερωτήματος θα κάνει επίσης χρήση του πίνακα διαμερίσματα ή/και ανίχνευση στο ευρετήριο.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>ΑΝΑΖΗΤΗΣΗ: Ταξίδι διανομής, medallion και hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Εκτίμησης της ποιότητας δεδομένων: Επαλήθευση εγγραφών με εσφαλμένη γεωγραφικό μήκος ή/και γεωγραφικό πλάτος

Αυτό το παράδειγμα, διερευνάται Εάν κανένα από τα πεδία γεωγραφικό μήκος ή/και γεωγραφικό πλάτος είτε περιέχει μια μη έγκυρη τιμή (μοίρες radian πρέπει να είναι μεταξύ -90 και 90), ή έχετε (0, 0) συντεταγμένες.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>ΑΝΑΖΗΤΗΣΗ: Tipped και δεν με τμήμα στην άκρη Trips διανομής

Αυτό το παράδειγμα, βρίσκει τον αριθμό των trips που είχαν με τμήμα στην άκρη και δεν με τμήμα στην άκρη σε μια δεδομένη χρονική περίοδο (ή στο πλήρες σύνολο δεδομένων εάν καλύπτουν ολόκληρο το έτος). Αυτή η κατανομή αντανακλά τη διανομή δυαδικών ετικέτα θα χρησιμοποιηθεί αργότερα για μοντελοποίηση δυαδική ταξινόμηση.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>ΑΝΑΖΗΤΗΣΗ: Συμβουλή κλάσης/περιοχή διανομής

Αυτό το παράδειγμα υπολογίζει την κατανομή των περιοχών συμβουλή σε μια δεδομένη χρονική περίοδο (ή στο πλήρες σύνολο δεδομένων εάν καλύπτουν ολόκληρο το έτος). Πρόκειται για την κατανομή των κλάσεων ετικέτα που θα χρησιμοποιηθεί αργότερα για μοντελοποίηση multiclass ταξινόμηση.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>ΑΝΑΖΗΤΗΣΗ: Υπολογίσετε και να συγκρίνετε απόσταση ταξιδιού

Αυτό το παράδειγμα μετατρέπει το αγροτικό και απόθεσης γεωγραφικό μήκος και γεωγραφικό πλάτος σε SQL Γεωγραφία σημεία, υπολογίζει την απόσταση του ταξιδιού χρησιμοποιώντας SQL Γεωγραφία σημεία διαφορά και επιστρέφει ένα τυχαίο δείγμα των αποτελεσμάτων για σύγκριση. Το παράδειγμα που περιορίζει τα αποτελέσματα σε έγκυρη συντεταγμένες χρησιμοποιώντας μόνο το ερώτημα αξιολόγηση ποιότητας δεδομένων που αναφέρονται παραπάνω.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Η δυνατότητα μηχανικής σε ερωτήματα SQL

Η ετικέτα γενιάς Γεωγραφία μετατροπής εξερευνήσεων ερωτήματα και μπορεί επίσης να χρησιμοποιηθεί για τη δημιουργία ετικετών/δυνατότητες, καταργώντας το τμήμα απογραφής. Πρόσθετη δυνατότητα μηχανικής SQL παραδείγματα παρέχονται στην ενότητα [δεδομένων εξερευνήσεων και δυνατότητα μηχανικής σε φορητό υπολογιστή IPython](#ipnb) . Είναι πιο αποτελεσματικό να εκτελέσετε τη δυνατότητα δημιουργίας ερωτημάτων σχετικά με το πλήρες σύνολο δεδομένων ή ένα μεγάλο υποσύνολο των χρησιμοποιώντας ερωτήματα SQL, οι οποίες εκτελούνται απευθείας στην παρουσία βάσης δεδομένων SQL Server. Τα ερωτήματα μπορεί να εκτελεστεί στο **SQL Server Management Studio**, IPython το Σημειωματάριο ή οποιοδήποτε εργαλείο/περιβάλλον ανάπτυξης που μπορεί να αποκτήσει πρόσβαση στη βάση δεδομένων τοπικά ή απομακρυσμένα.

#### <a name="preparing-data-for-model-building"></a>Προετοιμασία δεδομένων για τη δημιουργία μοντέλου

Το ακόλουθο ερώτημα σύνδεσμοι η **nyctaxi\_ταξίδι** και **nyctaxi\_ναύλο που** πίνακες, δημιουργεί μια Δυαδική ταξινόμηση ετικέτα **με τμήμα στην άκρη**, μια ετικέτα ταξινόμηση πολλών κλάσης **Συμβουλή\_κλάση**, και εξάγει ένα τυχαίο δείγμα 1% από το πλήρες ενωμένα dataset. Αυτό το ερώτημα μπορεί να αντιγραφεί τότε θα επικολληθούν απευθείας στα [Studio εκμάθηση μηχανής Azure](https://studio.azureml.net) [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα για την κατάποση δεδομένα απευθείας από την παρουσία της βάσης δεδομένων SQL Server σε Azure. Το ερώτημα εξαιρεί τις εγγραφές με λανθασμένο (0, 0) συντεταγμένες.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Εξερεύνηση δεδομένων και τη δυνατότητα μηχανικής στο Σημειωματάριο IPython

Σε αυτήν την ενότητα, θα πραγματοποιούμε διερεύνησης δεδομένων και τη δυνατότητα δημιουργίας χρήση ερωτημάτων Python και SQL με τη βάση δεδομένων SQL Server που δημιουργήσατε νωρίτερα. Παρέχεται ένα σημειωματάριο IPython δείγμα που ονομάζεται **machine-Learning-data-science-process-sql-story.ipynb** στο φάκελο **Σημειωματάρια IPython δείγματος** . Αυτό το σημειωματάριο είναι επίσης διαθέσιμη σε [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Η προτεινόμενη ακολουθία όταν εργάζεστε με μεγάλα δεδομένα είναι τα εξής:

- Ανάγνωση σε ένα μικρό δείγμα των δεδομένων σε ένα πλαίσιο δεδομένων στη μνήμη.
- Εκτελέστε ορισμένες απεικονίσεις και explorations χρησιμοποιώντας τα δεδομένα του δείγματος.
- Πειραματιστείτε με δυνατότητα μηχανικής χρησιμοποιώντας τα δεδομένα του δείγματος.
- Για μεγαλύτερη Εξερεύνηση δεδομένων, χειρισμός δεδομένων και τη δυνατότητα μηχανική, χρησιμοποιήστε Python για να εκδόσουν ερωτήματα SQL απευθείας με τη βάση δεδομένων του SQL Server με την εικονική Μηχανή Azure.
- Αποφασίσετε να χρησιμοποιήσετε για τη δημιουργία μοντέλου εκμάθηση μηχανής Azure το μέγεθος του δείγματος.

Όταν είστε έτοιμοι να συνεχίσετε στην εκμάθηση μηχανής Azure, μπορείτε μπορεί:  

1. Αποθηκεύστε το τελικό ερώτημα SQL για να εξαγάγετε και δείγματα δεδομένων και αντιγραφή-επικόλληση το ερώτημα απευθείας σε μια [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα στην εκμάθηση μηχανής Azure. Αυτή η μέθοδος έχει αποδειχθεί στην ενότητα [Models δόμησης στην εκμάθηση μηχανής Azure](#mlmodel) .    
2. Διατηρούνται τα δεδομένα δείγματος και τεχνολογικών που σκοπεύετε να χρησιμοποιήσετε για το μοντέλο που δημιουργείτε σε ένα νέο πίνακα της βάσης δεδομένων και, στη συνέχεια, χρησιμοποιήστε το νέο πίνακα στα [Δεδομένα εισαγωγής] [ import-data] λειτουργική μονάδα.

Τα παρακάτω είναι μερικά εξερεύνησης δεδομένων, οπτικοποίησης και δυνατότητα μηχανικής παραδείγματα. Για περισσότερα παραδείγματα, ανατρέξτε στην ενότητα του σημειωματαρίου SQL IPython δείγμα στο φάκελο **Σημειωματάρια IPython δείγμα** .

#### <a name="initialize-database-credentials"></a>Προετοιμασία διαπιστευτήρια βάσης δεδομένων

Προετοιμασία τις ρυθμίσεις σύνδεσης βάσης δεδομένων με τις παρακάτω μεταβλητές:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Δημιουργία σύνδεσης βάσης δεδομένων

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Αναφορά αριθμό γραμμών και στηλών σε πίνακα nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Συνολικός αριθμός γραμμών = 173179759  
- Συνολικός αριθμός στηλών = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Ανάγνωση σε δείγμα μικρό δεδομένα από τη βάση δεδομένων του SQL Server

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Χρόνο για να διαβάσετε τον πίνακα "Δείγματα" είναι 6.492000 δευτερόλεπτα  
Ανάκτηση του αριθμού γραμμών και στηλών = (84952, 21)

#### <a name="descriptive-statistics"></a>Περιγραφικά στατιστικά

Τώρα είστε έτοιμοι για να εξερευνήσετε το δείγμα δεδομένων. Μας ξεκινούν με κοιτάζουν Περιγραφικά στατιστικά στοιχεία για το **ταξίδι\_απόσταση** (ή οποιοδήποτε άλλο) πεδία:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Απεικόνιση: Παράδειγμα σχεδίασης πλαισίου

Στη συνέχεια εξετάσουμε το πλαίσιο-απολήξεις για την απόσταση του ταξιδιού για να απεικονίσετε το quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![Σχεδίαση #1][1]

#### <a name="visualization-distribution-plot-example"></a>Απεικόνιση: Παράδειγμα σχεδίασης διανομής

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Σχεδίαση #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Απεικόνιση: Γραμμή και γραμμή παρατηρητηρίων

Σε αυτό το παράδειγμα, η θέση αποθήκης η απόσταση ταξιδιού σε πέντε θέσεις αποθήκης και οπτικοποίηση των binning αποτελεσμάτων.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Εμείς να σχεδιάσετε την παραπάνω κατανομή θέσεων αποθήκης σε μια γραμμή ή γραμμή σχεδίασης ως εξής

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Σχεδίαση #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Σχεδίαση #4][4]

#### <a name="visualization-scatterplot-example"></a>Απεικόνισης: Παράδειγμα Scatterplot

Δείξουμε παρατηρητήριο διασποράς μεταξύ **ταξίδι\_ώρα\_σε\_δευτ /** και **ταξίδι\_απόσταση** για να δείτε εάν υπάρχει οποιαδήποτε συσχέτιση

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Σχεδίαση #6][6]

Ομοίως να ελέγχεται η σχέση μεταξύ **ρυθμός\_κωδικό** και **το ταξίδι\_απόσταση**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Σχεδίαση #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>Υπο-δειγματοληψία των δεδομένων σε SQL

Κατά την προετοιμασία δεδομένων για υπόδειγμα του [Studio εκμάθηση μηχανής Azure](https://studio.azureml.net), μπορείτε να αποφασίζει σχετικά με το **ερώτημα SQL για να χρησιμοποιήσετε απευθείας στη λειτουργική μονάδα εισαγωγής δεδομένων** ή να το διατηρούνται τα τεχνολογικά και δείγματος δεδομένα σε ένα νέο πίνακα, που θα μπορούσε να χρησιμοποιήσει τα [Δεδομένα εισαγωγής] [ import-data] λειτουργική μονάδα με ένα απλό *από *ΕΠΙΛΈΞΤΕ* < σας\_νέα\_πίνακα\_όνομα >**.

Σε αυτήν την ενότητα θα δημιουργήσουμε ένα νέο πίνακα για τη διατήρηση του δείγματος και τεχνολογικών δεδομένων. Ένα παράδειγμα ένα άμεσο ερώτημα SQL για τη δημιουργία μοντέλου παρέχεται στην ενότητα [δεδομένων εξερευνήσεων και δυνατότητα μηχανικής στον SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Δημιουργήστε ένα δείγμα πίνακα και συμπλήρωση με 1% συνδεδεμένους πίνακες. Αποθέστε πρώτα πίνακα εάν υπάρχει.

Σε αυτήν την ενότητα, εμείς να ενώσετε τους πίνακες **nyctaxi\_ταξίδι** και **nyctaxi\_ναύλο που**, εξαγάγετε ένα τυχαίο δείγμα 1% και σταθεροποίηση του δείγματος δεδομένων σε ένα νέο όνομα πίνακα **nyctaxi\_μία\_τοις εκατό**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Εξερεύνηση δεδομένων χρησιμοποιώντας ερωτήματα SQL στο Σημειωματάριο IPython

Σε αυτήν την ενότητα, εξετάσουμε κατανομές δεδομένων χρησιμοποιώντας τα δεδομένα του 1% δειγματοληψίας που παραμένει στο νέο πίνακα δημιουργήσαμε παραπάνω. Σημειώστε ότι παρόμοια explorations μπορούν να εκτελεστούν χρησιμοποιώντας το αρχικό πίνακες, προαιρετικά χρησιμοποιώντας **TABLESAMPLE** για να περιορίσετε την Εξερεύνηση δείγμα ή με τον περιορισμό των αποτελεσμάτων σε μια δεδομένη χρονική περίοδο χρησιμοποιώντας το **αγροτικό\_ημερομηνίας/ώρας** διαμερίσματα, όπως παρουσιάζεται στην ενότητα [δεδομένων εξερευνήσεων και δυνατότητα μηχανικής στον SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>ΑΝΑΖΗΤΗΣΗ: Ημερήσια κατανομή της trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>ΑΝΑΖΗΤΗΣΗ: Ταξίδι κατανομή ανά medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Δυνατότητα δημιουργίας χρήση ερωτημάτων SQL στο Σημειωματάριο IPython

Σε αυτήν την ενότητα σας θα δημιουργήσει νέες ετικέτες και χαρακτηριστικά απευθείας χρήση ερωτημάτων SQL, λειτουργεί στον πίνακα 1% δείγμα δημιουργήσαμε στην προηγούμενη ενότητα.

#### <a name="label-generation-generate-class-labels"></a>Δημιουργία ετικέτας: Δημιουργία κλάσης ετικέτες

Στο παρακάτω παράδειγμα, μας να δημιουργήσει δύο σύνολα ετικετών για μοντελοποίηση:

1. Δυαδικές ετικέτες κατηγορίας **με τμήμα στην άκρη** (πρόβλεψη εάν θα δοθεί μια συμβουλή)
2. Ετικέτες multiclass **Συμβουλή\_κλάση** (η πρόβλεψη συμβουλή αποθήκης ή περιοχή)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Η δυνατότητα μηχανικής: Μέτρηση δυνατότητες για κατηγορική στήλες

Αυτό το παράδειγμα μετατρέπει ένα πεδίο κατηγοριών σε ένα αριθμητικό πεδίο, αντικαθιστώντας κάθε κατηγορία με το πλήθος των εμφανίσεων στα δεδομένα.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Η δυνατότητα μηχανικής: Κάδος δυνατότητες για αριθμητικές στήλες

Αυτό το παράδειγμα μετατρέπει ένα αριθμητικό πεδίο συνεχούς σε περιοχές προκαθορισμένων κατηγοριών, π.χ., μετασχηματισμός αριθμητικό πεδίο σε ένα πεδίο κατηγοριών.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Η δυνατότητα μηχανικής: Εξαγωγή δυνατότητες θέση από δεκαδικό γεωγραφικό πλάτος/γεωγραφικό μήκος

Αυτό το παράδειγμα διασπά τη δεκαδική απεικόνιση ενός γεωγραφικού πλάτους ή/και γεωγραφικό μήκος πεδίου σε πολλά πεδία περιοχής διαφορετικών χρονικών υποδιαιρέσεων, όπως, χώρα, πόλη, πόλη, Αποκλεισμός, κ.λπ. Σημειώστε ότι τα νέα πεδία γεω-χωρικά δεδομένα δεν αντιστοιχίζονται σε πραγματικές θέσεις. Για πληροφορίες σχετικά με την αντιστοίχιση geocode θέσεις, ανατρέξτε στην ενότητα [Υπηρεσίες ΥΠΌΛΟΙΠΟ χάρτες Bing](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Επιβεβαιώστε την τελική του μορφή του πίνακα featurized

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Είμαστε έτοιμοι να προβεί σε μοντέλο δόμησης και μοντέλο ανάπτυξης στην [Εκμάθηση μηχανής Azure](https://studio.azureml.net). Τα δεδομένα είναι έτοιμα για οποιοδήποτε από τα προβλήματα πρόβλεψης που περιγράφεται παραπάνω, ήτοι:

1. Δυαδική ταξινόμηση: να προβλέψετε έστω και μια συμβουλή που έχει καταβληθεί για ένα ταξίδι.

2. Ταξινόμηση multiclass: για να προβλέψετε την περιοχή του άκρου που καταβάλλονται, σύμφωνα με την προκαθορισμένη κλάσεις.

3. Εργασία παλινδρόμησης: για να προβλέψετε το ποσό του άκρου που καταβάλλεται για ένα ταξίδι.  


## <a name="mlmodel"></a>Μοντέλα κτίριο στην εκμάθηση μηχανής Azure

Για να ξεκινήσετε την άσκηση μοντελοποίησης, συνδεθείτε στο χώρο εργασίας σας εκμάθηση μηχανής Azure. Εάν δεν έχετε δημιουργήσει ακόμη μια μηχανή εκπαιδευτικό χώρο εργασίας, ανατρέξτε στην ενότητα [Δημιουργία ενός χώρου εργασίας εκμάθηση μηχανής Azure](machine-learning-create-workspace.md).

1. Για να ξεκινήσετε με την εκμάθηση μηχανής Azure, ανατρέξτε στην ενότητα [Τι είναι Studio εκμάθηση μηχανής Azure;](machine-learning-what-is-ml-studio.md)

2. Συνδεθείτε στο [μηχάνημα Azure εκμάθηση Studio](https://studio.azureml.net).

3. Studio αρχική σελίδα παρέχει πολλές πληροφορίες, βίντεο, υλικό εκμάθησης, συνδέσεις σε λειτουργικές μονάδες αναφοράς και άλλους πόρους. Για περισσότερες πληροφορίες σχετικά με την εκμάθηση μηχανής Azure, συμβουλευτείτε το [Κέντρο εκμάθησης τεκμηρίωση για τη μηχανή Azure](https://azure.microsoft.com/documentation/services/machine-learning/).

Ένα πείραμα τυπική εκπαίδευση αποτελείται από τα εξής:

1. Δημιουργήστε ένα πείραμα **+ ΝΈΟ** .
2. Η λήψη των δεδομένων για την εκμάθηση μηχανής Azure.
3. Η προ-επεξεργασία, μετασχηματισμός και χειρισμό των δεδομένων, ανάλογα με τις ανάγκες.
4. Δημιουργία δυνατότητες ανάλογα με τις ανάγκες.
5. Διαιρέστε τα δεδομένα σε εκπαίδευση/επικύρωση/δοκιμή σύνολα δεδομένων (ή έχετε ξεχωριστό σύνολα δεδομένων για κάθε).
6. Επιλέξτε μία ή περισσότερες αλγόριθμοι εκμάθησης υπολογιστή ανάλογα με το ζήτημα της εκμάθησης για να τα επιλύσετε. Π.χ., δυαδική ταξινόμηση, κατάταξη multiclass, παλινδρόμησης.
7. Ένα ή περισσότερα μοντέλα χρησιμοποιώντας το dataset εκπαίδευσης της αμαξοστοιχίας.
8. Βαθμολογία το dataset επικύρωσης χρησιμοποιώντας το εκπαιδευμένο μοντέλα.
9. Αξιολογήστε τα μοντέλα για να υπολογίσετε τις σχετικές μετρήσεις για το πρόβλημα της εκμάθησης.
10. Κανονικά συντονιστείτε τα μοντέλα και επιλέξτε το καλύτερο μοντέλο ανάπτυξης.

Σε αυτήν την άσκηση, μας ήδη έχετε εξερευνήσει και να κατασκευαστεί τα δεδομένα σε SQL Server και να αποφασίσει σχετικά με το μέγεθος του δείγματος να ingest στην εκμάθηση μηχανής Azure. Για να δημιουργήσετε ένα ή περισσότερα από τα μοντέλα πρόβλεψης μας αποφάσισε:

1. Η λήψη των δεδομένων για την εκμάθηση μηχανής Azure χρησιμοποιώντας τα [Δεδομένα εισαγωγής] [ import-data] λειτουργική μονάδα, διαθέσιμες στην ενότητα **δεδομένων εισόδου και εξόδου** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισαγωγή δεδομένων] [ import-data] σελίδα αναφοράς λειτουργικής μονάδας.

    ![Εκμάθηση μηχανής Azure εισαγωγής δεδομένων][17]

2. Επιλέξτε **Βάση δεδομένων SQL Azure** ως το αρχείο **προέλευσης δεδομένων** στον πίνακα " **Ιδιότητες** ".

3. Εισαγάγετε το όνομα της βάσης δεδομένων DNS στο πεδίο **όνομα διακομιστή βάσης δεδομένων** . Μορφή:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Πληκτρολογήστε το **όνομα της βάσης δεδομένων** στο αντίστοιχο πεδίο.

5. Πληκτρολογήστε το **όνομα χρήστη SQL** στο το **διακομιστή όνομα aqccount χρήστη και τον κωδικό πρόσβασης στο το **διακομιστή χρήστη λογαριασμός κωδικός πρόσβασης **.

6. Ενεργοποιήστε την επιλογή **Αποδοχή κάθε πιστοποιητικό διακομιστή** .

7. Στην περιοχή κειμένου επεξεργασία **ερωτήματος σε βάση δεδομένων** , επικόλληση του ερωτήματος που εξάγει τα απαραίτητα πεδία (μαζί με οποιαδήποτε πεδία υπολογισμού όπως οι ετικέτες) της βάσης δεδομένων και κάτω δειγματοληψία τα δεδομένα για το μέγεθος δείγματος που θέλετε.

Είναι ένα παράδειγμα ενός πειράματος δυαδική ταξινόμηση ανάγνωση δεδομένων απευθείας από τη βάση δεδομένων του SQL Server στην παρακάτω εικόνα. Παρόμοια πειράματα μπορεί να κατασκευαστεί για multiclass ταξινόμησης και ζητήματα παλινδρόμησης.

![Αμαξοστοιχία εκμάθηση μηχανής Azure][10]

> [AZURE.IMPORTANT] Στα δεδομένα μοντελοποίησης εκχύλιση και δειγματοληψίας παραδείγματα ερωτημάτων που παρέχονται στις προηγούμενες ενότητες, **όλες τις ετικέτες των ασκήσεων τρεις μοντελοποίησης περιλαμβάνονται στο ερώτημα**. Είναι ένα σημαντικό βήμα (απαιτείται) σε κάθε μία από τις ασκήσεις μοντελοποίησης για να **εξαιρέσετε** τις περιττές ετικέτες για τα άλλα δύο προβλήματα και οποιαδήποτε άλλα **διαρροών προορισμού**. Για π.χ., όταν χρησιμοποιείτε δυαδική ταξινόμηση, χρησιμοποιήστε την ετικέτα **με τμήμα στην άκρη** και να αποκλείσετε τα πεδία **Συμβουλή\_κλάση**, **tip\_ποσό**, και **σύνολο\_ποσό**. Αυτός είναι διαρροές προορισμού δεδομένου ότι αυτές προϋποθέτουν τη συμβουλή που καταβάλλεται.
>
> Για να αποκλείσετε περιττές στήλες ή/και προορισμού "διαρροές", μπορείτε να χρησιμοποιήσετε την [Επιλογή στηλών στο Dataset] [ select-columns] λειτουργική μονάδα ή τα [Μετα-δεδομένα επεξεργασία][edit-metadata]. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Επιλογή στηλών στο Dataset] [ select-columns] και να [Επεξεργαστείτε μεταδεδομένα] [ edit-metadata] αναφέρονται σε σελίδες.

## <a name="mldeploy"></a>Την ανάπτυξη μοντέλα στην εκμάθηση μηχανής Azure

Όταν το μοντέλο σας είναι έτοιμη, μπορείτε εύκολα να το αναπτύξετε ως υπηρεσία web απευθείας από το πείραμα. Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη υπηρεσιών web Azure υπολογιστή εκμάθησης, ανατρέξτε στο θέμα [ανάπτυξη μιας υπηρεσίας web εκμάθηση μηχανής Azure](machine-learning-publish-a-machine-learning-web-service.md).

Για να αναπτύξετε μια νέα υπηρεσία web, πρέπει να:

1. Δημιουργήστε ένα πείραμα βαθμολογίας.
2. Η ανάπτυξη της υπηρεσίας web.

Για να δημιουργήσετε ένα πείραμα βαθμολογίας από ένα πείραμα εκπαίδευση **ολοκληρώθηκε** , κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ ΒΑΘΜΟΛΟΓΊΑΣ ΠΕΙΡΑΜΑΤΙΣΤΕΊΤΕ** στην κάτω γραμμή ενέργειας.

![Βαθμολογία Azure][18]

Η εκμάθηση μηχανής Azure θα προσπαθήσει να δημιουργήσει ένα πείραμα βαθμολογίας με βάση τα στοιχεία του πειράματος εκπαίδευσης. Συγκεκριμένα, θα:

1. Αποθηκεύσετε το μοντέλο εκπαιδευμένο και καταργήστε τις λειτουργικές μονάδες εκπαίδευσης του μοντέλου.
2. Για να προσδιορίσετε μια λογική **θύρα εισόδου** για την αναπαράσταση του σχήματος αναμενόμενα δεδομένα εισόδου.
3. Προσδιορισμός λογική **θύρα εξόδου που** να αντιπροσωπεύει το σχήμα εξόδου υπηρεσίας web αναμενόμενη.

Όταν δημιουργείται το πείραμα βαθμολογίας, αναθεωρήσουν και να προσαρμόσετε ανάλογα με τις ανάγκες. Μια τυπική ρύθμιση είναι να αντικαταστήσετε το σύνολο δεδομένων εισόδου ή/και ερωτήματα με εκείνο το οποίο δεν περιλαμβάνει πεδία ετικέτας, όπως αυτές δεν θα είναι διαθέσιμες κατά την κλήση της υπηρεσίας. Είναι επίσης μια καλή πρακτική να μειώσετε το μέγεθος του συνόλου δεδομένων εισόδου ή/και ερώτημα σε λίγες εγγραφές, μόλις αρκετή για να υποδείξετε το σχήμα εισόδου. Για τη θύρα εξόδου, είναι συνηθισμένο για να εξαιρέσετε όλα εισόδου πεδία και περιλαμβάνει μόνο τις **Ετικέτες σκορ** και τις **Πιθανότητες που έχετε κερδίσει** στα δεδομένα εξόδου χρησιμοποιώντας την [Επιλογή στηλών στο Dataset] [ select-columns] λειτουργική μονάδα.

Είναι ένα δείγμα βαθμολογίας πείραμα στην παρακάτω εικόνα. Όταν είστε έτοιμοι να αναπτύξετε, κάντε κλικ στο κουμπί **ΔΗΜΟΣΊΕΥΣΗ ΥΠΗΡΕΣΊΑ WEB** στην κάτω γραμμή ενέργειας.

![Δημοσίευση εκμάθηση μηχανής Azure][11]

Σε recap, σε αυτό το πρόγραμμα εκμάθησης της αναλυτικής παρουσίασης, έχετε δημιουργήσει περιβάλλον επιστήμης Azure δεδομένων, με ένα μεγάλο σύνολο δεδομένων δημόσιου τελείως από απόκτηση δεδομένων μοντέλο εκπαίδευση και την ανάπτυξη εκμάθηση μηχανής Azure υπηρεσίας web.

### <a name="license-information"></a>Πληροφορίες άδειας χρήσης

Αυτή η αναλυτική παρουσίαση δείγματος και τα συνοδευτικά δέσμες ενεργειών και IPython notebook(s) είναι κοινόχρηστα από τη Microsoft με την άδεια χρήσης του MIT δεν Ολοκληρώνεται. Ελέγξτε το αρχείο αρχείο LICENSE.txt στον κατάλογο του δείγματος κώδικα σε GitHub για περισσότερες λεπτομέρειες.

### <a name="references"></a>Αναφορές

• [Σελίδα λήψης Trips ταξί νέα ΥΌΡΚΗ Andrés Monroy](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing δεδομένων ταξιδιού ταξί του νέα ΥΌΡΚΗ, ο Χρήστος Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Νέα ΥΌΡΚΗ ταξί και Limousine Επιτροπή έρευνας και στατιστικών στοιχείων](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

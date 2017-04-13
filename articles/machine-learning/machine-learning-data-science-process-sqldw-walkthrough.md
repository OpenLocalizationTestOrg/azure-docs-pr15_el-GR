<properties
    pageTitle="Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση αποθήκη δεδομένων του SQL | Microsoft Azure"
    description="Διαδικασία προηγμένη ανάλυση και τεχνολογία σε δράση"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Η διαδικασία Science δεδομένων ομάδας στην πράξη: χρήση αποθήκη δεδομένων του SQL

Σε αυτό το πρόγραμμα εκμάθησης, θα σας καθοδηγήσουμε σας καθοδηγεί στη δημιουργία και ανάπτυξη ενός μηχανικής εκμάθησης μοντέλο χρησιμοποιώντας αποθήκη δεδομένων του SQL (SQL DW) για μια διαθέσιμη στο κοινό σύνολο δεδομένων--του συνόλου δεδομένων [Ταξίδια ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) . Το μοντέλο δυαδικό ταξινόμηση συνταχθεί προβλέπει ή όχι η πληρωμή μια συμβουλή για ταξίδι και μοντέλα multiclass διαβάθμιση και παλινδρόμησης είναι όπως επίσης εξηγήθηκε που πρόβλεψης την κατανομή για τα ποσά συμβουλή που καταβλήθηκε.

Η διαδικασία ακολουθεί τη ροή εργασίας [Ομάδας δεδομένων Science διαδικασία (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Θα σας δείξουμε πώς μπορείτε να εγκαταστήσετε ένα περιβάλλον science δεδομένων, πώς να φορτώσετε τα δεδομένα στο SQL DW και πώς να χρησιμοποιείτε SQL DW ή ένα σημειωματάριο IPython για να εξερευνήσετε τα δεδομένα και μηχανικό δυνατοτήτων στο μοντέλο. Θα σας δείξουμε, στη συνέχεια, πώς μπορείτε να δημιουργήσετε και να αναπτύξετε ένα μοντέλο με Azure μηχανικής εκμάθησης.


## <a name="dataset"></a>Το σύνολο δεδομένων νέα ΥΌΡΚΗ ταξί ταξίδια

Τα δεδομένα του ταξιδιού ταξί νέα ΥΌΡΚΗ αποτελείται από 20GB περίπου συμπιεσμένα αρχεία CSV (χωρίς συμπίεση ~ 48GB), ταξίδια μεμονωμένα υπερβαίνει 173 εκατομμύρια και το κομίστρων εγγραφών που καταβλήθηκε για κάθε ταξίδι. Κάθε εγγραφή ταξιδιού περιλαμβάνει τις θέσεις απάντηση κλήσης από και απόθεσης και ώρες, anonymized στοιχείο παραβίασης (του προγράμματος οδήγησης) αριθμό αδειών χρήσης και τον αριθμό medallion (μοναδικό αναγνωριστικό του ταξί). Τα δεδομένα καλύπτει όλα ταξίδια κατά το έτος 2013 και παρέχεται με τα παρακάτω δύο σύνολα δεδομένων για κάθε μήνα:

1. Το αρχείο **trip_data.csv** περιέχει λεπτομέρειες ταξιδιού, όπως τον αριθμό των προσώπων, απάντηση κλήσης από την και dropoff σημεία, διάρκεια ταξιδιού και μήκους ταξίδι. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Το αρχείο **trip_fare.csv** περιέχει τις λεπτομέρειες στο ναύλο που καταβάλλεται για κάθε ταξιδιού, όπως τον τύπο πληρωμής, το ποσό ναύλο, πρόσθετης και των φόρων, συμβουλές και διόδια, και το συνολικό ποσό που καταβλήθηκε. Ακολουθούν μερικές δειγμάτων εγγραφές:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Το **μοναδικό κλειδί** που χρησιμοποιείται για να συμμετάσχετε σε ταξίδι\_δεδομένων και ταξιδιού\_ναύλο που αποτελείται από τα ακόλουθα τρία πεδία:

- Medallion,
- κλαπεί\_άδεια χρήσης και
- απάντηση κλήσης από την\_ημερομηνίας/ώρας.

## <a name="mltasks"></a>Διεύθυνση σε τρεις τύπους εργασιών πρόβλεψης

Θα σας διατύπωση τρεις πρόβλεψη προβλήματα με βάση το *Συμβουλή\_ποσό* για την απεικόνιση τρία είδη μοντελοποίησης εργασίες:

1. **Δυαδικό ταξινόμηση**: πρόβλεψη ή όχι μια συμβουλή πληρώθηκε για ταξίδι, δηλαδή μια *Συμβουλή\_ποσό* που είναι μεγαλύτερη από $0 είναι ένα θετικό παράδειγμα, κατά ένα *Συμβουλή\_ποσό* 0 € αποτελεί ένα παράδειγμα αρνητικός αριθμός.

2. **Ταξινόμηση multiclass**: πρόβλεψη την περιοχή των συμβουλών που καταβλήθηκε για το ταξίδι σας. Διαιρούμε το *Συμβουλή\_ποσό* σε πέντε θέσεων αποθήκευσης ή κατηγορίες:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Εργασία παλινδρόμησης**: πρόβλεψη το ποσό του άκρου που καταβάλλεται για ένα ταξίδι.  


## <a name="setup"></a>Ρύθμιση του περιβάλλοντος του science Azure δεδομένων για προηγμένη ανάλυση

Για να ρυθμίσετε το περιβάλλον Azure Science δεδομένων, ακολουθήστε τα παρακάτω βήματα.

**Δημιουργήστε το δικό σας λογαριασμό χώρο αποθήκευσης αντικειμένων blob του Azure**

- Κατά την προμήθεια το δικό σας χώρο αποθήκευσης αντικειμένων blob του Azure, επιλέξτε μια θέση παν για το χώρο αποθήκευσης αντικειμένων blob του Azure στο ή όσο δυνατή **Νότια κεντρικές ΗΠΑ**, που είναι τα νέα ΥΌΡΚΗ ταξί δεδομένα που είναι αποθηκευμένα. Τα δεδομένα θα αντιγραφούν χρησιμοποιώντας AzCopy από το κοντέινερ χώρου αποθήκευσης αντικειμένων blob δημόσια σε κοντέινερ στο δικό σας λογαριασμό χώρου αποθήκευσης. Όσο πιο μικρή είναι ο χώρος αποθήκευσης αντικειμένων blob του Azure Νότια κεντρικής μας, τόσο πιο γρήγορα θα ολοκληρωθεί αυτή η εργασία (βήμα 4).
- Για να δημιουργήσετε το δικό σας λογαριασμό Azure χώρου αποθήκευσης, ακολουθήστε τα βήματα που περιγράφονται στο [Azure σχετικά με τους λογαριασμούς χώρου αποθήκευσης](../storage/storage-create-storage-account.md). Φροντίστε να δημιουργήσετε σημειώσεις στις τιμές για τα ακόλουθα διαπιστευτήρια λογαριασμού χώρου αποθήκευσης, όπως θα χρειαστούν αργότερα σε αυτές τις οδηγίες.

  - **Όνομα λογαριασμού χώρου αποθήκευσης**
  - **Κλειδί λογαριασμού χώρου αποθήκευσης**
  - **Το όνομα του κοντέινερ** (το οποίο θέλετε τα δεδομένα να είναι αποθηκευμένο στο το χώρο αποθήκευσης αντικειμένων blob του Azure)

**Προμήθεια σας παρουσία Azure SQL DW.**
Ακολουθήστε την τεκμηρίωση στη [Δημιουργία ενός αποθήκη δεδομένων του SQL](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) για να παράσχετε μια παρουσία αποθήκη δεδομένων του SQL. Βεβαιωθείτε ότι κάνετε σημειογραφίας τα εξής διαπιστευτήρια αποθήκη δεδομένων του SQL που θα χρησιμοποιηθεί στα επόμενα βήματα.

  - **Το όνομα του διακομιστή**: <server Name>. database.windows.net
  - **Όνομα SQLDW (βάση δεδομένων)**
  - **Όνομα χρήστη**
  - **Κωδικός πρόσβασης**

**Εγκαταστήστε το Visual Studio 2015 και εργαλεία δεδομένων του SQL Server.** Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση του Visual Studio 2015 ή/και SSDT (SQL Server Data Tools) για την αποθήκη δεδομένων του SQL](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Σύνδεση με το DW Azure SQL με το Visual Studio.** Για οδηγίες, ανατρέξτε στο θέμα τα βήματα 1 και 2 στο στοιχείο [σύνδεση σε αποθήκη δεδομένων του SQL Azure με το Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Εκτελέστε το ακόλουθο ερώτημα SQL στη βάση δεδομένων που δημιουργήσατε στο σας αποθήκη δεδομένων του SQL (αντί για το ερώτημα που παρέχονται στο θέμα σύνδεση βήμα 3) για να **δημιουργήσετε ένα πρωτεύον κλειδί**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Δημιουργία ενός χώρου εργασίας Azure μηχανικής εκμάθησης στην περιοχή Azure τη συνδρομή σας.** Για οδηγίες, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου εργασίας Azure μηχανικής εκμάθησης](machine-learning-create-workspace.md).

## <a name="getdata"></a>Φόρτωση των δεδομένων σε αποθήκη δεδομένων του SQL

Ανοίξτε μια κονσόλα εντολών του Windows PowerShell. Εκτελέστε το ακόλουθο PowerShell εντολές για να κάνετε λήψη του παραδείγματος SQL αρχεία δέσμης ενεργειών που θα σας κάνουν κοινή χρήση με εσάς στο Github σε έναν τοπικό κατάλογο που καθορίζετε με την παράμετρο *- DestDir*. Μπορείτε να αλλάξετε την τιμή της παραμέτρου *-DestDir* σε οποιαδήποτε τοπικό κατάλογο. Εάν δεν υπάρχει *- DestDir* , θα δημιουργηθεί από τη δέσμη ενεργειών PowerShell.

>[AZURE.NOTE] Ίσως χρειαστεί να **Εκτέλεση ως διαχειριστής** κατά την εκτέλεση την ακόλουθη δέσμη ενεργειών PowerShell εάν κατάλογό σας *DestDir* χρειάζονται δικαίωμα διαχειριστή για να δημιουργήσετε ή να γράψετε σε αυτήν.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Μετά την επιτυχή εκτέλεση, τον τρέχοντα κατάλογο εργασίας αλλάζει σε *- DestDir*. Θα πρέπει να μπορούν να δουν την οθόνη όπως παρακάτω:

![][19]

Στο σας *-DestDir*, εκτελέστε την ακόλουθη δέσμη ενεργειών PowerShell σε κατάσταση λειτουργίας διαχειριστή:

    ./SQLDW_Data_Import.ps1

Κατά την εκτέλεση της δέσμης ενεργειών του PowerShell για πρώτη φορά, θα σας ζητηθεί να εισαγάγετε τις πληροφορίες από το DW SQL Azure και του λογαριασμού σας χώρο αποθήκευσης αντικειμένων blob του Azure. Όταν ολοκληρωθεί αυτή η δέσμη ενεργειών PowerShell εκτελείται για πρώτη φορά, τα διαπιστευτήρια εισόδου που θα έχουν εγγραφεί στο αρχείο ρύθμισης παραμέτρων SQLDW.conf τον κατάλογο εργασίας παρουσίαση. Η μελλοντική εκτέλεση αυτού του αρχείου δέσμης ενεργειών του PowerShell έχει την επιλογή για να διαβάσετε όλες τις απαραίτητες παράμετροι από αυτό το αρχείο ρύθμισης παραμέτρων. Εάν χρειάζεστε για να αλλάξετε ορισμένες παράμετροι, μπορείτε να επιλέξετε να εισαγάγετε τις παραμέτρους στην οθόνη κατά την ερώτηση, διαγράφοντας αυτό το αρχείο ρύθμισης παραμέτρων και εισαγωγή τις τιμές παραμέτρων όταν σας ζητηθεί ή να αλλάξετε τις τιμές παραμέτρων με την επεξεργασία του αρχείου SQLDW.conf στον κατάλογό σας *- DestDir* .

>[AZURE.NOTE] Για να αποφύγετε διενέξεις όνομα σχήματος με αυτές που υπάρχουν ήδη στο σας DW SQL Azure, κατά την ανάγνωση παράμετροι απευθείας από το αρχείο SQLDW.conf, έναν τυχαίο αριθμό 3-ψήφια προστίθεται στο όνομα του σχήματος από το αρχείο SQLDW.conf ως το προεπιλεγμένο όνομα σχήματος για κάθε εκτέλεση. Η δέσμη ενεργειών PowerShell μπορεί να σας ζητά ένα όνομα σχήματος: το όνομα μπορεί να έχει καθοριστεί κατά ευχέρεια χρήστη.

Αυτό το αρχείο **δέσμης ενεργειών του PowerShell** ολοκληρώνει τις ακόλουθες εργασίες:

- **Πραγματοποιεί λήψη και εγκαθιστά AzCopy**, εάν AzCopy δεν είναι ήδη εγκατεστημένο

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Αντίγραφα δεδομένων στο λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob ιδιωτικό** από τη δημόσια blob με AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Η φόρτωση δεδομένων με τη χρήση Polybase (με την εκτέλεση LoadDataToSQLDW.sql) για να σας DW SQL Azure** από το λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob ιδιωτικό με τις παρακάτω εντολές.

    - Δημιουργήστε ένα σχήμα

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Δημιουργία μιας βάσης δεδομένων εύρος διαπιστευτηρίων

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Δημιουργήστε μια εξωτερική προέλευση δεδομένων για μια Azure χώρο αποθήκευσης αντικειμένων blob

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Δημιουργήστε μια μορφή εξωτερικό αρχείο για ένα αρχείο csv. Τα δεδομένα είναι αρχεία μη συμπιεσμένων και πεδίων διαχωρίζονται με το χαρακτήρα καθέτου.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Δημιουργία εξωτερικής ναύλο και πινάκων αποστολής και επιστροφής για νέα ΥΌΡΚΗ ταξί dataset στο χώρο αποθήκευσης αντικειμένων blob του Azure.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Φόρτωση δεδομένων από εξωτερική πίνακες στο χώρο αποθήκευσης αντικειμένων blob του Azure αποθήκη δεδομένων SQL

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Δημιουργία πίνακα δεδομένων δείγματος (NYCTaxi_Sample) και να εισαγάγετε δεδομένα σε αυτήν από την επιλογή ερωτήματα SQL σε ταξίδι και ναύλο πίνακες. (Ορισμένα βήματα από αυτόν τον οδηγό πρέπει να χρησιμοποιήσετε αυτό το δείγμα πίνακα).

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Τη γεωγραφική θέση της τους λογαριασμούς σας στο χώρο αποθήκευσης επηρεάζει τους χρόνους φόρτωσης.

>[AZURE.NOTE] Ανάλογα με τη γεωγραφική θέση του λογαριασμού σας χώρο αποθήκευσης αντικειμένων blob ιδιωτικό, η διαδικασία αντιγραφής δεδομένων από μια δημόσια blob στο λογαριασμό σας ιδιωτικό αποθήκευσης μπορεί να διαρκέσει περίπου 15 λεπτά, ή ακόμα περισσότερο, και η διαδικασία για τη φόρτωση των δεδομένων από το λογαριασμό χώρου αποθήκευσης για να σας DW SQL Azure μπορεί να διαρκέσει 20 λεπτά ή περισσότερο.  

Θα πρέπει να αποφασίσετε ποιο εάν έχετε διπλότυπα αρχεία προέλευσης και προορισμού.

>[AZURE.NOTE] Εάν αρχεία .csv για αντιγραφή από το χώρο αποθήκευσης blob δημόσια του στο λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob ιδιωτικό υπάρχουν ήδη στο λογαριασμό σας χώρο αποθήκευσης αντικειμένων blob ιδιωτικό, AzCopy θα σας ερωτηθείτε εάν θέλετε να αντικαταστήσετε. Εάν δεν θέλετε να αντικαταστήσετε, εισαγάγετε **n** , όταν σας ζητηθεί. Εάν θέλετε να αντικαταστήσετε **όλες** από αυτά, εισαγωγής **ενός** όταν σας ζητηθεί. Μπορείτε επίσης να εισαγωγής **y** για να αντικαταστήσετε ξεχωριστά αρχεία .csv.

![Σχεδιάστε #21][21]

Μπορείτε να χρησιμοποιήσετε τα δικά σας δεδομένα. Εάν τα δεδομένα σας είναι στον υπολογιστή σας εσωτερική στην εφαρμογή πραγματική ζωή σας, μπορείτε να χρησιμοποιήσετε AzCopy για την αποστολή δεδομένων εσωτερικής εγκατάστασης για το χώρο αποθήκευσης αντικειμένων blob του ιδιωτικού Azure. Μόνο πρέπει να αλλάξετε τη θέση **προέλευσης** , `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, στην εντολή AzCopy από το αρχείο δέσμης ενεργειών του PowerShell στον τοπικό κατάλογο που περιέχει τα δεδομένα σας.


>[AZURE.TIP] Εάν τα δεδομένα σας είναι ήδη στο χώρο αποθήκευσης αντικειμένων blob του ιδιωτικού Azure του στην εφαρμογή πραγματική ζωή σας, μπορείτε να παραλείψετε το βήμα AzCopy στη δέσμη ενεργειών PowerShell και να αποστείλετε απευθείας τα δεδομένα στο Azure SQL DW. Αυτό θα απαιτήσει πρόσθετες αλλαγές της δέσμης ενεργειών για να το προσαρμόσετε τη μορφή των δεδομένων σας.


Αυτή η δέσμη ενεργειών Powershell επίσης συνδέεται στις πληροφορίες Azure SQL DW στη τα αρχεία παράδειγμα Εξερεύνηση δεδομένων SQLDW_Explorations.sql, SQLDW_Explorations.ipynb και SQLDW_Explorations_Scripts.py ώστε να είναι έτοιμοι να επαναληφθεί αμέσως μετά την ολοκλήρωση της δέσμης ενεργειών του PowerShell αυτά τα τρία αρχεία.

Μετά από μια επιτυχημένη εκτέλεση, θα δείτε οθόνης όπως παρακάτω:

![][20]

## <a name="dbexplore"></a>Εξερεύνηση δεδομένων και τη δυνατότητα μηχανική στο αποθήκη δεδομένων του SQL Azure

Σε αυτήν την ενότητα, μπορούμε να πραγματοποιήσουμε Εξερεύνηση δεδομένων και τη δυνατότητα δημιουργίας, εκτελώντας ερωτήματα SQL σε σχέση με DW SQL Azure απευθείας χρησιμοποιώντας **Εργαλεία δεδομένων του Visual Studio**. Μπορείτε να βρείτε όλα τα ερωτήματα SQL που χρησιμοποιείται σε αυτήν την ενότητα στο το δείγμα δέσμης ενεργειών με το όνομα *SQLDW_Explorations.sql*. Αυτό το αρχείο έχει ήδη ληφθεί στον κατάλογο του τοπικού σας από τη δέσμη ενεργειών PowerShell. Μπορείτε επίσης να την ανακτήσετε από [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Αλλά το αρχείο σε Github δεν διαθέτει τις πληροφορίες Azure SQL DW συνδεδεμένα.

Σύνδεση με το DW SQL Azure με χρήση του Visual Studio με το όνομα σύνδεσης SQL DW και τον κωδικό πρόσβασης και ανοίξτε την **Εξερεύνηση αντικειμένου SQL** για να επιβεβαιώσετε τη βάση δεδομένων και τους πίνακες έχουν εισαχθεί. Ανάκτηση του αρχείου *SQLDW_Explorations.sql* .

>[AZURE.NOTE] Για να ανοίξετε ένα πρόγραμμα επεξεργασίας ερωτήματος παράλληλη αποθήκη δεδομένων (PDW), χρησιμοποιήστε την εντολή **Νέο ερώτημα** , ενώ είναι επιλεγμένο το PDW στην **Εξερεύνηση αντικειμένου SQL**. Το τυπικό πρόγραμμα επεξεργασίας ερωτήματος SQL δεν υποστηρίζεται από PDW.

Εδώ θα βρείτε τον τύπο των δεδομένων Εξερεύνηση και η δυνατότητα γενιάς εργασίες που εκτελούνται σε αυτήν την ενότητα:

- Εξερεύνηση δεδομένων κατανομή ορισμένα πεδία σε διαφορετικές των windows.
- Διερεύνηση ποιότητας δεδομένων των πεδίων γεωγραφικού πλάτους και μήκους.
- Δημιουργία ετικετών δυαδικό και multiclass ταξινόμηση με βάση το **Συμβουλή\_ποσό**.
- Δημιουργία δυνατότητες και υπολογισμού/σύγκριση αποστάσεις ταξίδι.
- Συνδέστε τους δύο πίνακες και εξαγωγή τυχαίο δείγμα που θα χρησιμοποιηθεί για να δημιουργήσετε μοντέλα.

### <a name="data-import-verification"></a>Εισαγωγή δεδομένων επαλήθευσης

Αυτά τα ερωτήματα παρέχει μια γρήγορη επαλήθευσης του αριθμού των γραμμών και στηλών στους πίνακες συμπληρωμένο προηγουμένως με χρήση του Polybase παράλληλες μαζικής εισαγωγής,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Εξόδου:** Θα πρέπει να πάρετε 173,179,759 γραμμές και στήλες 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Εξερεύνηση: Ταξιδιού κατανομή με medallion

Σε αυτό το παράδειγμα ερωτήματος προσδιορίζει το medallions (ταξί αριθμοί) που ολοκληρωθεί λιγότερα από 100 ταξίδια μέσα σε μια συγκεκριμένη χρονική περίοδο. Το ερώτημα θα επωφεληθείτε από την access διαμερίσματα πίνακα καθώς αυτό είναι σταθεροποιούνται κατά τον συνδυασμό διαμερίσματα της **Απάντηση κλήσης από την\_datetime**. Υποβολή ερωτημάτων του πλήρους συνόλου δεδομένων θα κάνει επίσης χρήση του πίνακα διαμερίσματα ή/και να δημιουργήσετε ευρετήριο σάρωση.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Εξόδου:** Το ερώτημα θα πρέπει να επιστρέφει έναν πίνακα με γραμμές που καθορίζει το 13,369 medallions (ταξί) και τον αριθμό των ταξιδιού ολοκληρωθεί από τους στο 2013. Η τελευταία στήλη περιέχει την καταμέτρηση του αριθμού των ταξίδια ολοκληρώθηκε.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Εξερεύνηση: Ταξιδιού διανομής, medallion και hack_license

Αυτό το παράδειγμα προσδιορίζει το medallions (ταξί αριθμοί) και hack_license αριθμοί (προγράμματα οδήγησης) που ολοκληρωθεί περισσότερα από 100 ταξίδια μέσα σε μια συγκεκριμένη χρονική περίοδο.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Εξόδου:** Το ερώτημα θα πρέπει να επιστρέφει έναν πίνακα με 13,369 γραμμές που καθορίζει το 13,369 αυτοκίνητο/πρόγραμμα οδήγησης αναγνωριστικά που έχουν ολοκληρωθεί περισσότερες που 100 ταξίδια στο 2013. Η τελευταία στήλη περιέχει την καταμέτρηση του αριθμού των ταξίδια ολοκληρώθηκε.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Εκτίμησης της ποιότητας δεδομένων: επαλήθευση εγγραφές με εσφαλμένες γεωγραφικού ή/και γεωγραφικό πλάτος

Αυτό το παράδειγμα διερευνάται εάν οποιοδήποτε από τα πεδία μήκος ή/και γεωγραφικό πλάτος είτε περιέχει μια μη έγκυρη τιμή (μοίρες radian πρέπει να είναι μεταξύ -90 και 90), ή να έχετε (0, 0) συντεταγμένες.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Εξόδου:** Το ερώτημα επιστρέφει 837,467 ταξίδια που έχουν μη έγκυρα πεδία γεωγραφικού ή/και γεωγραφικό πλάτος.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Εξερεύνηση: Επικαλυπτόμενα σύγκριση με τμήμα στην άκρη δεν ταξίδια διανομής

Αυτό το παράδειγμα εντοπίζει τον αριθμό των ταξίδια που έχουν επικαλυπτόμενα έναντι τον αριθμό που δεν έχουν επικαλυπτόμενα σε μια συγκεκριμένη χρονική περίοδο (ή σε του πλήρους συνόλου δεδομένων εάν που καλύπτει ολόκληρο το έτος ως έχει ρυθμιστεί εδώ). Αυτή η κατανομή απεικονίζει την κατανομή δυαδικό ετικέτας που θα χρησιμοποιηθεί αργότερα για μοντελοποίηση δυαδικό ταξινόμηση.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Εξόδου:** Το ερώτημα θα πρέπει να επιστρέψει με τμήμα στην άκρη της παρακάτω συχνότητας συμβουλή για το έτος 2013: 90,447,622 και δεν επικαλυπτόμενα 82,264,709.

### <a name="exploration-tip-classrange-distribution"></a>Εξερεύνηση: Διανομή εκπαιδευτικών/περιοχή συμβουλή

Αυτό το παράδειγμα υπολογίζει την κατανομή των περιοχών συμβουλή σε μια δεδομένη χρονική περίοδο (ή σε του πλήρους συνόλου δεδομένων εάν που καλύπτεται από το πλήρες έτος). Αυτή είναι η κατανομή των κλάσεων ετικέτας που θα χρησιμοποιηθεί αργότερα για μοντελοποίηση multiclass ταξινόμηση.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Αποτέλεσμα:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Εξερεύνηση: Υπολογισμός και σύγκριση απόσταση ταξιδιού

Αυτό το παράδειγμα μετατρέπει την απάντηση κλήσης από και απόθεσης γεωγραφικό μήκος και γεωγραφικό πλάτος σε SQL Γεωγραφία σημεία, τύπος υπολογίζει την απόσταση ταξίδι με διαφορά σημεία Γεωγραφία SQL και επιστρέφει έναν τυχαίο δείγμα των αποτελεσμάτων για σύγκριση. Το παράδειγμα περιορίζει τα αποτελέσματα σε έγκυρη συντεταγμένες χρησιμοποιώντας μόνο το ερώτημα αξιολόγησης ποιότητας δεδομένων καλύπτεται νωρίτερα.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Η δυνατότητα μηχανικής με συναρτήσεις SQL

Μερικές φορές συναρτήσεις SQL μπορεί να είναι μια αποτελεσματική επιλογή για τη δυνατότητα μηχανικής. Σε αυτές τις οδηγίες, ορίσαμε μια συνάρτηση SQL για να υπολογίσετε την άμεση απόσταση μεταξύ των θέσεων απάντηση κλήσης από και dropoff. Μπορείτε να εκτελέσετε τις ακόλουθες δέσμες ενεργειών SQL σε **Εργαλεία δεδομένων του Visual Studio**.

Ακολουθεί η δέσμη ενεργειών SQL που καθορίζει τη συνάρτηση απόσταση.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Ακολουθεί ένα παράδειγμα για να καλέσετε αυτήν τη συνάρτηση για να δημιουργήσετε δυνατότητες στο ερώτημά σας SQL:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Εξόδου:** Αυτό το ερώτημα δημιουργεί έναν πίνακα (με 2,803,538 γραμμές) με απάντηση κλήσης από την dropoff γεωγραφικού και μήκη και τις αντίστοιχες αποστάσεις απευθείας σε μίλια. Εδώ θα βρείτε τα αποτελέσματα για τις πρώτες 3 γραμμές:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Προετοιμασία των δεδομένων για τη δημιουργία μοντέλου

Το ακόλουθο ερώτημα συνδέσμους του **nyctaxi\_ταξιδιού** και **nyctaxi\_ναύλο** πίνακες, δημιουργεί μια Δυαδική ταξινόμηση ετικέτας **με τμήμα στην άκρη**, μια ετικέτα ταξινόμηση πολλών κλάσης **Συμβουλή\_κλάσης**, και εξάγει ένα δείγμα από το πλήρες σύνολο δεδομένων συνδεδεμένους. Η δειγματοληψία γίνεται από την ανάκτηση ένα υποσύνολο τα ταξίδια βάσει χρόνου συλλογής.  Αυτό το ερώτημα μπορεί να είναι αντιγράψατε και κατόπιν επικολληθεί απευθείας στα [Azure μηχανικής εκμάθησης Studio](https://studio.azureml.net) [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα για κατάποση άμεσων δεδομένων από την παρουσία της βάσης δεδομένων SQL στο Azure. Το ερώτημα εξαίρεση των εγγραφών με λάθος (0, 0) συντεταγμένες.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Όταν είστε έτοιμοι να συνεχίσετε να Azure μηχανικής εκμάθησης, ενδέχεται να είτε:  

1. Αποθηκεύστε το τελικό ερώτημα SQL για την εξαγωγή και ακούστε τα δείγματα δεδομένων και αντιγραφή επικόλληση στο ερώτημα απευθείας σε μια [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα Azure μάθετε υπολογιστή, ή
2. Τα δεδομένα δείγματος και από αποσυμπίληση σκοπεύετε να χρησιμοποιήσετε για δημιουργία ενός νέου πίνακα SQL DW μοντέλο και χρησιμοποιήστε τον νέο πίνακα στο παράθυρο [Εισαγωγή δεδομένων] παραμένει[ import-data] λειτουργική μονάδα Azure μηχανικής εκμάθησης. Η δέσμη ενεργειών PowerShell στο προηγούμενο βήμα περιλαμβάνει το κάνει αυτό για εσάς. Μπορείτε να διαβάσετε απευθείας από αυτόν τον πίνακα στη λειτουργική μονάδα εισαγωγή δεδομένων.


## <a name="ipnb"></a>Εξερεύνηση δεδομένων και τη δυνατότητα μηχανικής στο Σημειωματάριο IPython

Σε αυτήν την ενότητα, θα σας θα εκτελέσει Εξερεύνηση δεδομένων και τη δυνατότητα δημιουργίας χρησιμοποιώντας δύο Python και ερωτήματα SQL σε σχέση με το DW SQL που δημιουργήσατε νωρίτερα. Έχουν ληφθεί από ένα σημειωματάριο IPython δείγμα με το όνομα **SQLDW_Explorations.ipynb** και ένα αρχείο δέσμης ενεργειών Python **SQLDW_Explorations_Scripts.py** τοπικού καταλόγου σας. Είναι επίσης διαθέσιμες στην [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Αυτά τα δύο αρχεία είναι ίδιες σε δέσμες ενεργειών Python. Το αρχείο δέσμης ενεργειών Python παρέχεται σε εσάς, σε περίπτωση που δεν έχετε ένα σημειωματάριο IPython διακομιστή. Αυτά τα δύο δείγματα αρχείων Python έχουν σχεδιαστεί στην περιοχή **Python 2.7**.

Απαιτείται Azure SQL DW στο δείγμα IPython Σημειωματάριο και το αρχείο δέσμης ενεργειών Python γίνεται λήψη των πληροφοριών στον τοπικό υπολογιστή έχει συνδεθεί από τη δέσμη ενεργειών PowerShell προηγουμένως. Έχουν εκτελέσιμο χωρίς καμία τροποποίηση.

Εάν έχετε ήδη ρυθμίσει ένα χώρο εργασίας του AzureML, μπορείτε να αποστείλετε το δείγμα IPython σημειωματάριο με την υπηρεσία AzureML IPython Σημειωματάριο και να ξεκινήσετε με το απευθείας. Ακολουθούν τα βήματα για να στείλετε σε υπηρεσία AzureML IPython Σημειωματάριο:

1. Συνδεθείτε στο χώρο εργασίας σας AzureML, κάντε κλικ στην επιλογή "Studio" στο επάνω μέρος και κάντε κλικ στην επιλογή "ΣΗΜΕΙΩΜΑΤΆΡΙΑ" στην αριστερή πλευρά της σελίδας web.

    ![Σχεδιάστε #22][22]

2. Κάντε κλικ στην επιλογή "ΔΗΜΙΟΥΡΓΊΑ" στην κάτω αριστερή γωνία της ιστοσελίδας και επιλέξτε "Python 2". Στη συνέχεια, δώστε ένα όνομα για το Σημειωματάριο και κάντε κλικ στο σημάδι ελέγχου για να δημιουργήσετε το νέο σημειωματάριο IPython κενό.

    ![Σχεδιάστε #23][23]

3. Κάντε κλικ στο σύμβολο "Jupyter" στην επάνω αριστερή γωνία του νέου σημειωματαρίου IPython.

    ![Σχεδιάστε #24][24]

4. Σύρετε και αποθέστε το δείγμα IPython Σημειωματάριο στη σελίδα **δέντρου** της υπηρεσίας AzureML IPython σημειωματάριό σας και κάντε κλικ στο κουμπί **Αποστολή**. Στη συνέχεια, το δείγμα IPython Σημειωματάριο θα αποσταλεί στην υπηρεσία AzureML IPython σημειωματαρίου.

    ![Σχεδιάστε #25][25]

Για να εκτελέσετε το δείγμα IPython Σημειωματάριο ή το Python δέσμη ενεργειών αρχείο, το παρακάτω Python πακέτων είναι απαραίτητες. Εάν χρησιμοποιείτε την υπηρεσία AzureML IPython σημειωματαρίου, αυτά τα πακέτα έχουν προ-εγκατεστημένες.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Η προτεινόμενη ακολουθία κατά τη δημιουργία αναλυτικής λύσεις για προχωρημένους στην AzureML με μεγάλου όγκου δεδομένων είναι τα εξής:

- Διαβάστε σε ένα μικρό δείγμα των δεδομένων σε ένα πλαίσιο στη μνήμη δεδομένων.
- Εκτέλεση ορισμένες απεικονίσεις και explorations με τη χρήση του δείγματος δεδομένων.
- Πειραματιστείτε με δυνατότητα μηχανικής με τη χρήση του δείγματος δεδομένων.
- Για μεγαλύτερη Εξερεύνηση δεδομένων, χειρισμός δεδομένων και τη δυνατότητα μηχανικής, χρησιμοποιήστε Python για την έκδοση σε ερωτήματα SQL απευθείας από το DW SQL.
- Αποφασίστε το μέγεθος δείγματος για να είναι κατάλληλη για τη δημιουργία μοντέλου Azure μηχανικής εκμάθησης.

Το followings είναι μερικά Εξερεύνηση δεδομένων, απεικόνιση δεδομένων και τη δυνατότητα μηχανική παραδείγματα. Μπορείτε να βρείτε περισσότερες explorations δεδομένων στο δείγμα IPython Σημειωματάριο και το δείγμα αρχείου δέσμης ενεργειών Python.

### <a name="initialize-database-credentials"></a>Προετοιμασία διαπιστευτήρια βάσης δεδομένων

Προετοιμασία τις ρυθμίσεις σύνδεσης βάσης δεδομένων σε τις παρακάτω μεταβλητές:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Δημιουργία σύνδεσης βάσης δεδομένων

Παρακάτω θα δείτε τη συμβολοσειρά σύνδεσης που δημιουργεί τη σύνδεση με τη βάση δεδομένων.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Αναφορά αριθμό γραμμών και στηλών σε πίνακα < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Συνολικό αριθμό των γραμμών = 173179759  
- Συνολικός αριθμός των στηλών = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Αναφορά αριθμό γραμμών και στηλών σε πίνακα < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Συνολικό αριθμό των γραμμών = 173179759  
- Συνολικός αριθμός των στηλών = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Ανάγνωση σε ένα δείγμα μικρές δεδομένων από τη βάση δεδομένων SQL δεδομένων αποθήκης

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Χρόνος για να διαβάσετε το δείγμα πίνακα είναι 14.096495 δευτερόλεπτα.  
Ανάκτηση πλήθους γραμμών και στηλών = (1000, 21).

### <a name="descriptive-statistics"></a>Περιγραφικά στατιστικά στοιχεία

Τώρα είστε έτοιμοι να εξερευνήσετε τα δεδομένα του δείγματος. Θα σας Έναρξη με εξετάζοντας ορισμένες Περιγραφικά στατιστικά στοιχεία για το **ταξιδιού\_απόσταση** (ή οποιαδήποτε άλλα πεδία που επιλέγετε για να καθορίσετε).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Απεικόνιση: Παράδειγμα σχεδίασης πλαισίου

Επόμενο εξετάσουμε το πλαίσιο σχεδίασης για την απόσταση ταξιδιού για την απεικόνιση του quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Σχεδιάστε #1][1]

### <a name="visualization-distribution-plot-example"></a>Απεικόνιση: Παράδειγμα σχεδίασης διανομής

Διάγραμμα που απεικόνιση της κατανομής και ένα ιστόγραμμα όσον αφορά τις αποστάσεις δείγματος ταξίδι.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Σχεδιάστε #2][2]

### <a name="visualization-bar-and-line-plots"></a>Απεικόνιση: Ράβδων και γραμμών σχεδιάσεων

Σε αυτό το παράδειγμα, κλάσης την απόσταση αποστολής και επιστροφής σε πέντε θέσεις αποθήκευσης και απεικόνιση των binning αποτελεσμάτων.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Θα σας για να σχεδιάσετε την παραπάνω κατανομή Ανακύκλωσης σε μια γραμμή ή γραμμή σχεδίασης με:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Σχεδιάστε #3][3]

και

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Σχεδιάστε #4][4]

### <a name="visualization-scatterplot-examples"></a>Απεικόνιση: Παραδείγματα Scatterplot

Θα σας δείξουμε διασποράς σχεδίασης μεταξύ **ταξιδιού\_χρόνου\_στο\_ενοτήτων του** και **ταξιδιού\_απόσταση** για να δείτε εάν υπάρχει τυχόν συσχέτισης

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Σχεδιάστε #6][6]

Ομοίως, θα σας να ελέγξετε τη σχέση μεταξύ **επιτόκιο\_κώδικα** και **ταξιδιού\_απόσταση**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Σχεδιάστε #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Εξερεύνηση δεδομένων στο δείγμα δεδομένων με ερωτήματα SQL IPython σημειωματαρίου

Σε αυτήν την ενότητα, εξετάσουμε κατανομές δεδομένων με τη χρήση του δείγματος δεδομένων που έχει διατηρηθεί στο νέο πίνακα που δημιουργήσαμε παραπάνω. Σημειώστε ότι μπορούν να εκτελεστούν παρόμοια explorations χρησιμοποιώντας το αρχικό πίνακες.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Εξερεύνηση: Αναφορά αριθμό γραμμών και στηλών του δείγματος πίνακα

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Εξερεύνηση: Επικαλυπτόμενα/δεν προβλήματος διανομής

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Εξερεύνηση: Διανομή εκπαιδευτικών συμβουλή

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Εξερεύνηση: Σχεδιάστε την κατανομή συμβουλή από την κλάση

    tip_class_dist['tip_freq'].plot(kind='bar')

![Σχεδιάστε #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Εξερεύνηση: Ημερήσια κατανομή των ταξίδια

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Εξερεύνηση: Ταξιδιού κατανομή ανά medallion

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Εξερεύνηση: Ταξιδιού διανομής από την άδεια χρήσης medallion και στοιχείο παραβίασης

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Εξερεύνηση: Κατανομής χρόνου αποστολής και επιστροφής

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Εξερεύνηση: Κατανομή απόσταση ταξιδιού

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Εξερεύνηση: Κατανομή τύπο πληρωμής

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Επαληθεύστε την τελική του μορφή του πίνακα featurized

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Δημιουργία μοντέλων στο Azure μηχανικής εκμάθησης

Συνεχίζουμε να τώρα είστε έτοιμοι να συνεχίσετε να δόμησης μοντέλο και μοντέλο ανάπτυξης στο [Azure μηχανικής εκμάθησης](https://studio.azureml.net). Τα δεδομένα είναι έτοιμα για χρήση σε κάποιο από τα προβλήματα πρόβλεψης που περιγράφεται παραπάνω, δηλαδή:

1. **Δυαδικό ταξινόμηση**: πρόβλεψη ή όχι μια συμβουλή πληρώθηκε για ταξίδι.

2. **Ταξινόμηση multiclass**: πρόβλεψη της περιοχής που καταβλήθηκε, σύμφωνα με τις κατηγορίες παλιότερα καθορισμένη συμβουλή.

3. **Εργασία παλινδρόμησης**: πρόβλεψη το ποσό του άκρου που καταβάλλεται για ένα ταξίδι.  



Για να ξεκινήσετε την άσκηση μοντελοποίηση, συνδεθείτε στο χώρο εργασίας σας **Azure μηχανικής εκμάθησης** . Εάν δεν έχετε δημιουργήσει ακόμη μια μηχανικής εκμάθησης χώρου εργασίας, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου εργασίας Azure ML](machine-learning-create-workspace.md).

1. Για να ξεκινήσετε με το Azure μηχανικής εκμάθησης, ανατρέξτε στο θέμα [Τι είναι το Azure μηχανικής εκμάθησης Studio;](machine-learning-what-is-ml-studio.md)

2. Συνδεθείτε στο [Azure μηχανικής εκμάθησης Studio](https://studio.azureml.net).

3. Studio αρχική σελίδα παρέχει πληθώρα πληροφορίες, βίντεο, προγράμματα εκμάθησης, συνδέσεις προς την αναφορά λειτουργικές μονάδες και άλλους πόρους. Για περισσότερες πληροφορίες σχετικά με το Azure μηχανικής εκμάθησης, επικοινωνήστε με το [Κέντρο τεκμηρίωση Azure μηχανικής εκμάθησης](https://azure.microsoft.com/documentation/services/machine-learning/).

Ένα τυπικό εκπαίδευση έρευνας αποτελείται από τα παρακάτω βήματα:

1. Δημιουργήστε μια **+ ΔΗΜΙΟΥΡΓΊΑ** έρευνας.
2. Μεταφέρετε τα δεδομένα στο Azure ML.
3. Προ-επεξεργασία, μετασχηματισμός και να χειριστείτε τα δεδομένα, όπως απαιτείται.
4. Δημιουργία δυνατότητες, όπως απαιτείται.
5. Διαιρέστε τα δεδομένα σε εκπαίδευση/επικύρωσης/δοκιμές σύνολα δεδομένων (ή έχετε ξεχωριστή σύνολα δεδομένων για κάθε).
6. Επιλέξτε μία ή περισσότερες μηχανικής εκμάθησης αλγόριθμους ανάλογα με το πρόβλημα εκμάθησης για την επίλυση. Π.χ., δυαδικό ταξινόμηση, multiclass ταξινόμηση, παλινδρόμησης.
7. Εκπαίδευση ένα ή περισσότερα μοντέλα με χρήση του συνόλου δεδομένων εκπαίδευσης.
8. Βαθμολογία του συνόλου επικύρωσης δεδομένων χρησιμοποιώντας το εκπαιδευμένο μοντέλα.
9. Το αποτέλεσμα είναι τα μοντέλα για τον υπολογισμό τις σχετικές μετρήσεις για το πρόβλημα εκμάθησης.
10. Ρυθμίσετε με ακρίβεια τα μοντέλα και επιλέξτε το μοντέλο βέλτιστες για την ανάπτυξη.

Σε αυτήν την άσκηση, θα σας έχουν ήδη εξερευνήσει κατασκευαστεί τα δεδομένα στο αποθήκη δεδομένων του SQL και αποφασίσει σχετικά με το μέγεθος δείγματος ingest σε Azure ML. Ακολουθεί η διαδικασία για να δημιουργήσετε ένα ή περισσότερα από τα μοντέλα πρόβλεψης:

1. Μεταφέρετε τα δεδομένα στο ML Azure χρησιμοποιώντας την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα, διαθέσιμο στην ενότητα **δεδομένα εισόδου και εξόδου** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εισαγωγή δεδομένων] [ import-data] σελίδα αναφοράς λειτουργική μονάδα.

    ![Azure ML εισαγωγή δεδομένων][17]

2. Επιλέξτε **Βάση δεδομένων SQL Azure** ως **προέλευση δεδομένων** στον πίνακα " **Ιδιότητες** ".

3. Πληκτρολογήστε το όνομα της βάσης δεδομένων DNS στο πεδίο **όνομα διακομιστή βάσης δεδομένων** . Μορφή:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Πληκτρολογήστε το **όνομα της βάσης δεδομένων** στο αντίστοιχο πεδίο.

5. Πληκτρολογήστε το *όνομα χρήστη SQL* στο το **όνομα του λογαριασμού χρήστη Server**και του *κωδικού πρόσβασης* στο πλαίσιο του **κωδικού πρόσβασης λογαριασμού χρήστη διακομιστή**.

6. Ελέγξτε την επιλογή **Αποδοχή οποιαδήποτε πιστοποιητικό διακομιστή** .

7. Στην περιοχή κειμένου επεξεργασία **ερωτήματος βάσης δεδομένων** , επικολλήστε το ερώτημα που εξάγει τα απαραίτητα πεδία (συμπεριλαμβανομένων των πεδίων υπολογισμού όπως οι ετικέτες) βάσης δεδομένων και κάτω δειγμάτων τα δεδομένα για το μέγεθος δείγματος που θέλετε.

Ένα παράδειγμα μιας έρευνας δυαδικό ταξινόμηση την ανάγνωση δεδομένων απευθείας από τη βάση δεδομένων SQL Data Warehouse είναι στην παρακάτω εικόνα (θυμηθείτε να αντικατασταθεί η nyctaxi_trip ονόματα πίνακα και nyctaxi_fare από το όνομα του σχήματος και στα ονόματα των πινάκων που χρησιμοποιούνται στο τις αναλυτικές οδηγίες). Παρόμοια δοκιμές μπορεί να κατασκευαστεί για multiclass ταξινόμηση και ζητήματα παλινδρόμησης.

![Azure ML τρένο][10]

> [AZURE.IMPORTANT] Στο παράθυρο μοντελοποίηση δεδομένων εξαγωγής και δειγματοληψία παραδείγματα ερωτημάτων που παρέχονται στις προηγούμενες ενότητες, **όλες τις ετικέτες των ασκήσεων τρεις μοντελοποίηση περιλαμβάνονται στο ερώτημα**. Ένα σημαντικό βήμα (απαιτείται) σε κάθε ένα από τα ασκήσεις μοντελοποίηση είναι για να **αποκλείσετε** τις περιττές ετικέτες για τα άλλα προβλήματα δύο και τυχόν άλλες **είναι προορισμού**. Για παράδειγμα, όταν χρησιμοποιείτε δυαδικό ταξινόμηση, χρησιμοποιήστε την ετικέτα **με τμήμα στην άκρη** και να αποκλείσετε τα πεδία **Συμβουλή\_κλάσης**, **Συμβουλή\_ποσό**, και **συνολικό\_ποσό**. Αυτός είναι απώλειες προορισμού εφόσον αυτά συνεπάγεται τη συμβουλή που καταβλήθηκε.
>
> Για να αποκλείσετε τις στήλες που δεν είναι απαραίτητες ή στόχου απώλειες, ενδέχεται να μπορείτε να χρησιμοποιήσετε την [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] λειτουργική μονάδα ή την [Επεξεργασία μετα-δεδομένων][edit-metadata]. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] και [Επεξεργασία μετα-δεδομένα] [ edit-metadata] αναφορά σελίδες.

## <a name="mldeploy"></a>Ανάπτυξη μοντέλα σε Azure μηχανικής εκμάθησης

Όταν το μοντέλο σας είναι έτοιμη, μπορείτε εύκολα να το αναπτύξετε ως υπηρεσία web απευθείας από την έρευνα. Για περισσότερες πληροφορίες σχετικά με τις υπηρεσίες web του Azure ML για την ανάπτυξη, ανατρέξτε στο θέμα [ανάπτυξη μιας υπηρεσίας web Azure μηχανικής εκμάθησης](machine-learning-publish-a-machine-learning-web-service.md).

Για να αναπτύξετε μια νέα υπηρεσία web, πρέπει να:

1. Δημιουργήστε μια βαθμολογίας έρευνας.
2. Ανάπτυξη της υπηρεσίας web.

Για να δημιουργήσετε μια έρευνα βαθμολογίας από μια **Ολοκληρωμένη** έρευνας εκπαίδευση, κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ ΒΑΘΜΟΛΟΓΊΑΣ ΠΕΙΡΑΜΑΤΙΣΤΕΊΤΕ** στην κάτω γραμμή ενεργειών.

![Azure βαθμολογίας][18]

Azure μηχανικής εκμάθησης θα επιχειρήσει να δημιουργήσετε μια έρευνα βαθμολογίας με βάση τα στοιχεία του εκπαιδευτικού έρευνας. Συγκεκριμένα, θα:

1. Αποθηκεύστε το μοντέλο εκπαιδευμένο και καταργήστε το μοντέλο λειτουργικές μονάδες εκπαίδευσης.
2. Προσδιορίστε μια λογική **θύρα εισόδου** για να αντιπροσωπεύει το σχήμα αναμενόμενα δεδομένα εισόδου.
3. Προσδιορίστε μια λογική **θύρα εξόδου** για να αντιπροσωπεύει το σχήμα εξόδου υπηρεσίας web αναμενόμενο.

Όταν δημιουργείται το βαθμολογίας έρευνας, ελέγξτε τα και να προσαρμόσετε σύμφωνα με τις ανάγκες. Μια τυπική ρύθμιση είναι για να αντικαταστήσετε το σύνολο δεδομένων εισαγωγής ή/και το ερώτημα με ένα που αποκλείει τους πεδία ετικέτας, όπως αυτές δεν θα είναι διαθέσιμη όταν η υπηρεσία ονομάζεται. Είναι επίσης καλή πρακτική να μειώσετε το μέγεθος του το σύνολο δεδομένων εισαγωγής ή/και το ερώτημα μερικά εγγραφών, απλώς αρκετό για να υποδείξετε ότι το σχήμα εισόδου. Για τη θύρα εξόδου, είναι κοινά για να εξαιρέσετε εισαγωγής όλα τα πεδία και να περιλαμβάνει μόνο τις **Έχει βαθμολογηθεί με ετικέτες** και τις **Πιθανότητες έχει βαθμολογηθεί** στο αποτέλεσμα χρησιμοποιώντας την [Επιλογή στηλών στο σύνολο δεδομένων] [ select-columns] λειτουργική μονάδα.

Δείγμα βαθμολογίας έρευνας παρέχεται στην παρακάτω εικόνα. Όταν είστε έτοιμοι για την ανάπτυξη, κάντε κλικ στο κουμπί **ΔΗΜΟΣΊΕΥΣΗ ΥΠΗΡΕΣΊΑΣ WEB** στην κάτω γραμμή ενεργειών.

![Δημοσίευση Azure ML][11]


## <a name="summary"></a>Σύνοψη
Για να recap τι έχουμε κάνει σε αυτό το πρόγραμμα εκμάθησης αναλυτικές οδηγίες, έχετε δημιουργήσει ένα περιβάλλον science Azure δεδομένων, ολοκληρώθηκε με επιτυχία με ένα μεγάλο δημόσια σύνολο δεδομένων, λαμβάνοντας αυτό κατά τη διαδικασία Science δεδομένων ομάδας, πλήρως από απόκτηση δεδομένων για την εκπαίδευση του μοντέλου και, στη συνέχεια, την ανάπτυξη μιας υπηρεσίας web Azure μηχανικής εκμάθησης.

### <a name="license-information"></a>Πληροφορίες άδειας χρήσης

Αυτή η αναλυτική παρουσίαση δείγμα και τη συνοδεύουν δέσμες ενεργειών και IPython notebook(s) είναι κοινόχρηστα από τη Microsoft κάτω από την άδεια χρήσης του MIT. Ελέγξτε το αρχείο αρχείο LICENSE.txt στον κατάλογο του δείγματος κώδικα σε GitHub για περισσότερες λεπτομέρειες.

## <a name="references"></a>Αναφορές

• [Ταξίδια ταξί νέα ΥΌΡΚΗ Andrés Monroy λήψη σελίδας](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing του νέα ΥΌΡΚΗ ταξί ταξιδιού δεδομένων από τον Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Ταξί νέα ΥΌΡΚΗ και Limousine προμήθειας έρευνας και στατιστικά στοιχεία](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

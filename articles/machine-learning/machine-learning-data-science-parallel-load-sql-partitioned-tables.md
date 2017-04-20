<properties 
    pageTitle="Παράλληλη μαζικής εισαγωγής δεδομένων χρησιμοποιώντας πίνακες διαμερισμάτων SQL | Microsoft Azure" 
    description="Παράλληλη μαζικής εισαγωγής δεδομένων χρησιμοποιώντας πίνακες διαμερισμάτων SQL" 
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
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Παράλληλη μαζικής εισαγωγής δεδομένων χρησιμοποιώντας πίνακες διαμερισμάτων SQL

Αυτό το έγγραφο περιγράφει τον τρόπο για να δημιουργήσετε διαμερίσματα πίνακα fast παράλληλη μαζικής εισαγωγής δεδομένων σε μια βάση δεδομένων του SQL Server. Για μεγάλο φόρτωσης/μεταφορά δεδομένων σε μια βάση δεδομένων SQL, η εισαγωγή δεδομένων στο SQL DB και επόμενα ερωτήματα μπορούν να βελτιωθούν με _διαμερίσματα πίνακες και προβολές_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Δημιουργήστε μια νέα βάση δεδομένων και ένα σύνολο filegroups

- [Δημιουργία μιας νέας βάσης δεδομένων](https://technet.microsoft.com/library/ms176061.aspx) (Εάν δεν υπάρχει)
- Προσθήκη filegroups βάσης δεδομένων στη βάση δεδομένων, η οποία διατηρεί τα φυσικά αρχεία διαμερίσματα

  Σημείωση: Αυτό μπορεί να γίνει με τη [ΔΗΜΙΟΥΡΓΊΑ βάσης ΔΕΔΟΜΈΝΩΝ](https://technet.microsoft.com/library/ms176061.aspx) εάν είναι νέο ή μια [Βάση ΤΡΟΠΟΠΟΊΗΣΗ](https://msdn.microsoft.com/library/bb522682.aspx) εάν η βάση δεδομένων υπάρχει ήδη

- Προσθήκη ενός ή περισσότερων αρχείων (όπως απαιτείται) σε κάθε ομάδα αρχείων βάσης δεδομένων

 > [AZURE.NOTE] Καθορίσετε την ομάδα αρχείων προορισμού που να περιέχει δεδομένα για αυτό το διαμέρισμα και τα ονόματα αρχείων της φυσικής βάσης δεδομένων όπου θα αποθηκευτούν τα δεδομένα ομάδα αρχείων.
 
Το ακόλουθο παράδειγμα δημιουργεί μια νέα βάση δεδομένων με τρεις filegroups εκτός από το πρωτεύον και καταγραφής ομάδες, που περιέχει ένα φυσικό αρχείο σε κάθε ένα. Τα αρχεία βάσης δεδομένων δημιουργούνται στον προεπιλεγμένο φάκελο δεδομένων διακομιστή SQL, όπως έχει ρυθμιστεί στην παρουσία SQL Server. Για περισσότερες πληροφορίες σχετικά με τις προεπιλεγμένες θέσεις αρχείων, ανατρέξτε στην ενότητα " [Θέσεις αρχείων για προεπιλεγμένες και ονομάζεται εμφανίσεων του SQL Server](https://msdn.microsoft.com/library/ms143547.aspx)".

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Δημιουργήστε έναν πίνακα διαμερίσματα

Μπορείτε να δημιουργήσετε διαμερίσματα πίνακα σύμφωνα με το σχήμα των δεδομένων, έχει αντιστοιχιστεί σε το filegroups βάση δεδομένων που δημιουργήσατε στο προηγούμενο βήμα. Όταν τα δεδομένα είναι μαζικής εισαγωγής για τα διαμερίσματα πίνακες, εγγραφές θα κατανέμονται μεταξύ της filegroups σύμφωνα με ένα συνδυασμό διαμερισμάτων, όπως περιγράφεται παρακάτω.

**Για να δημιουργήσετε έναν πίνακα διαμερισμάτων, πρέπει να:**

- [Δημιουργία μιας συνάρτησης διαμέρισμα](https://msdn.microsoft.com/library/ms187802.aspx) που καθορίζει το εύρος των τιμών/όρια που θα συμπεριληφθούν σε κάθε μεμονωμένο διαμέρισμα πίνακα, π.χ., για να περιορίσετε τα διαμερίσματα ανά μήνα (ορισμένες\_ημερομηνίας/ώρας\_πεδίο) κατά το έτος 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Δημιουργία ενός συνδυασμού διαμερισμάτων](https://msdn.microsoft.com/library/ms179854.aspx) που αντιστοιχίζει κάθε περιοχή διαμέρισμα στη συνάρτηση διαμέρισμα σε μια φυσική ομάδα αρχείων, π.χ.:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Συμβουλή: Για να επαληθεύσετε τις περιοχές σε ισχύ σε κάθε διαμέρισμα σύμφωνα με τη συνάρτηση/συνδυασμό, πρέπει να εκτελέσετε το εξής ερώτημα:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Δημιουργία πίνακα διαμερίσματα](https://msdn.microsoft.com/library/ms174979.aspx) (s) με το σχήμα των δεδομένων, και καθορίστε το διαμέρισμα συστήματος περιορισμού πεδίο και χρησιμοποιείται για τη δημιουργία διαμερισμάτων του πίνακα, π.χ.:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα [Δημιουργία διαμερίσματα πίνακες και ευρετήρια](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Μαζική εισαγωγή τα δεδομένα για κάθε μεμονωμένο διαμέρισμα πίνακα

- Μπορείτε να χρησιμοποιήσετε BCP, ΜΑΖΙΚΉ εισαγωγή ή άλλες μεθόδους όπως [Οδηγός μετεγκατάστασης διακομιστή SQL](http://sqlazuremw.codeplex.com/). Το παράδειγμα που χρησιμοποιεί τη μέθοδο BCP.

- [Τροποποίηση της βάσης δεδομένων](https://msdn.microsoft.com/library/bb522682.aspx) για να αλλάξετε το συνδυασμό καταγραφή συναλλαγών σε BULK_LOGGED για να ελαχιστοποιήσετε την επιβάρυνση της καταγραφής, π.χ.:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Να επιδιώξουν δεδομένα κατά τη φόρτωση, εκκίνηση των λειτουργιών εισαγωγής μαζικής παράλληλα. Για συμβουλές σχετικά με τη διεκπεραίωση μαζική εισαγωγή μεγάλων δεδομένων σε βάσεις δεδομένων SQL Server, ανατρέξτε στην ενότητα [Φόρτωση 1 TB σε λιγότερο από 1 ώρα](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Η ακόλουθη δέσμη ενεργειών PowerShell είναι ένα παράδειγμα παράλληλης δεδομένων φόρτωση χρησιμοποιώντας BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Δημιουργία ευρετηρίων για τη βελτιστοποίηση των συνδέσμων και την απόδοση του ερωτήματος

- Εάν θα εξάγετε δεδομένα για μοντελοποίηση από πολλαπλούς πίνακες, θα πρέπει να δημιουργήσετε ευρετήρια στα πλήκτρα συνδέσμου για να βελτιώσετε τις επιδόσεις του συνδέσμου.

- [Δημιουργία ευρετηρίων](https://technet.microsoft.com/library/ms188783.aspx) (Συγκεντρωτικό ή μη συγκεντρωτικά) στόχευση την ίδια ομάδα αρχείων για κάθε διαμέρισμα, π.χ. για:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
ή,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Μπορείτε να επιλέξετε να δημιουργήσετε ευρετήρια πριν μαζικής εισαγωγής των δεδομένων. Δημιουργία ευρετηρίου, πριν από την εισαγωγή μαζικής θα επιβραδύνει τα δεδομένα κατά τη φόρτωση.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Διαδικασία ανάλυσης για προχωρημένους και τεχνολογία στο παράδειγμα ενέργειας

Για ένα παράδειγμα τερματικό για την αναλυτική παρουσίαση, χρησιμοποιώντας τη διαδικασία ανάλυσης του Cortana με ένα δημόσιο dataset, ανατρέξτε στην ενότητα [Cortana διαδικασία ανάλυσης στην ενέργεια: χρησιμοποιώντας τον SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 

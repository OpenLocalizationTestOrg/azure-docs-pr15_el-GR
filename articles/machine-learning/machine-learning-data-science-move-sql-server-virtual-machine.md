<properties 
    pageTitle="Μετακίνηση δεδομένων με τον SQL Server σε μια εικονική μηχανή Azure | Azure" 
    description="Μετακίνηση δεδομένων από το επίπεδο αρχείων ή από έναν SQL Server εσωτερικής εγκατάστασης με τον SQL Server Azure Εικονική." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Μετακίνηση δεδομένων με τον SQL Server σε μια εικονική μηχανή Azure

Αυτό το θέμα περιγράφει τις επιλογές για τη μετακίνηση δεδομένων από επίπεδο αρχείων (CSV ή TSV μορφές) ή από μια εσωτερική SQL Server με τον SQL Server σε μια εικονική μηχανή Azure. Αυτές οι εργασίες για τη μετακίνηση δεδομένων στο cloud είναι μέρος της διαδικασίας Science δεδομένων ομάδας.

Για ένα θέμα που περιγράφει τις επιλογές για τη μετακίνηση δεδομένων σε μια βάση δεδομένων SQL Azure για μηχανικής εκμάθησης, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων σε μια βάση δεδομένων SQL Azure για το Azure μηχανικής εκμάθησης](machine-learning-data-science-move-sql-azure.md).

Το **μενού** παρακάτω συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο ingest δεδομένων σε άλλα περιβάλλοντα προορισμού όπου τα δεδομένα μπορεί να αποθηκευτεί και να υποβάλλονται σε επεξεργασία κατά τη διάρκεια της διεργασίας Science δεδομένων ομάδας (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


Ο παρακάτω πίνακας συνοψίζει τις επιλογές για τη μετακίνηση δεδομένων με τον SQL Server σε μια εικονική μηχανή Azure.

<b>ΑΡΧΕΊΟ ΠΡΟΈΛΕΥΣΗΣ</b> |<b>ΠΡΟΟΡΙΣΜΌΣ: SQL Server στο Azure Εικονική</b> |
------------------ |-------------------- |
<b>Επίπεδο αρχείο</b> |1. <a href="#insert-tables-bcp">το βοηθητικό πρόγραμμα γραμμής εντολών μαζική αντιγραφή (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">μαζική εισαγωγή ερωτήματος SQL</a><br> 3. <a href="#sql-builtin-utilities">γραφικών ενσωματωμένα βοηθητικά προγράμματα στον SQL Server</a>
<b>SQL Server εσωτερικής εγκατάστασης</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Ανάπτυξη μια βάση δεδομένων SQL Server σε έναν οδηγό Εικονική Microsoft Azure</a><br> 2. <a href="#export-flat-file">Εξαγωγή σε επίπεδο αρχείο</a><br> 3. <a href="#sql-migration">Οδηγός μετεγκατάστασης της βάσης δεδομένων SQL</a> <br> 4. <a href="#sql-backup">βάση δεδομένων πίσω ασφαλείας και επαναφορά</a><br>

Σημειώστε ότι αυτό το έγγραφο προϋποθέτει ότι εκτελούνται εντολές SQL από SQL Server Management Studio ή από την Εξερεύνηση βάσης δεδομένων Visual Studio.

> [AZURE.TIP] Ως εναλλακτική λύση, μπορείτε να χρησιμοποιήσετε [Azure εργοστασίου δεδομένων](https://azure.microsoft.com/services/data-factory/) για να δημιουργήσετε και να προγραμματίσετε μια διαδικασία που θα μετακίνηση δεδομένων σε μια Εικονική SQL Server Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Αντιγραφή δεδομένων με το Azure εργοστασίου δεδομένων (αντιγραφή δραστηριότητα)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Προαπαιτούμενα στοιχεία
Αυτό το πρόγραμμα εκμάθησης προϋποθέτει ότι έχετε:

* Μια **συνδρομή Azure**. Εάν δεν έχετε μια συνδρομή, μπορείτε να εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).
* Ένας **λογαριασμός Azure χώρου αποθήκευσης**. Θα χρησιμοποιήσετε ένα λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση των δεδομένων σε αυτό το πρόγραμμα εκμάθησης. Εάν δεν έχετε ένα λογαριασμό Azure χώρου αποθήκευσης, ανατρέξτε στο άρθρο [Δημιουργία λογαριασμού χώρου αποθήκευσης](../storage/storage-create-storage-account.md#create-a-storage-account) . Αφού δημιουργήσετε το λογαριασμό χώρου αποθήκευσης, θα πρέπει να αποκτήσει το κλειδί λογαριασμού που χρησιμοποιούνται για την πρόσβαση του χώρου αποθήκευσης. Ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate χώρου αποθήκευσης](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Προμήθεια του φακέλου **SQL Server στο Azure Εικονική μηχανή**. Για οδηγίες, ανατρέξτε στο θέμα [Ρύθμιση μια εικονική μηχανή Azure SQL Server ως διακομιστής IPython σημειωματάριο για προηγμένη ανάλυση](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Εγκατεστημένη και ρυθμισμένη **Azure PowerShell** τοπικά. Για οδηγίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Μετακίνηση από ένα επίπεδο αρχείο προέλευσης δεδομένων με τον SQL Server Εικονική μηχανή Azure

Εάν τα δεδομένα σας είναι σε ένα επίπεδο αρχείο (τακτοποίηση σε μορφή γραμμής/στήλης), μπορεί να μετακινηθεί στο SQL Server Εικονική στην Azure μέσω του τις παρακάτω μεθόδους:

1. [Βοηθητικό πρόγραμμα αντίγραφο της γραμμής εντολών μαζικής (BCP)](#insert-tables-bcp) 
2. [Ερώτημα SQL μαζικής εισαγωγής](#insert-tables-bulkquery)
3. [Γραφική ενσωματωμένα βοηθητικά προγράμματα στον SQL Server (εισαγωγή/εξαγωγή, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Βοηθητικό πρόγραμμα αντίγραφο της γραμμής εντολών μαζικής (BCP)

BCP είναι ένα βοηθητικό πρόγραμμα γραμμής εγκατασταθεί με τον SQL Server και είναι ένας από τους πιο γρήγορος τρόπους για τη μετακίνηση δεδομένων. Λειτουργεί σε όλες τις τρεις παραλλαγές SQL Server (εσωτερικής εγκατάστασης SQL Server, SQL Azure και SQL Server Εικονική στην Azure). 

> [AZURE.NOTE]**Όπου πρέπει να είναι τα δεδομένα μου για BCP;**  
> Ενώ δεν είναι απαραίτητη, αντιμετωπίζετε τα αρχεία που περιέχει τα δεδομένα προέλευσης που βρίσκεται στον ίδιο υπολογιστή με τον SQL server προορισμού επιτρέπει για ταχύτερη μεταφορές (δικτύου ταχύτητας και στο στον τοπικό δίσκο ταχύτητα εισόδου/ΕΞΌΔΟΥ). Μπορείτε να μετακινήσετε τα αρχεία επίπεδο που περιέχει τα δεδομένα στον υπολογιστή όπου SQL Server είναι εγκατεστημένο χρησιμοποιώντας διάφορες αρχείο αντιγραφή εργαλεία όπως το [AZCopy](../storage/storage-use-azcopy.md), [Εξερεύνηση χώρου αποθήκευσης Azure](http://storageexplorer.com/) ή windows αντιγράψτε και επικολλήστε μέσω του πρωτοκόλλου απομακρυσμένης επιφάνειας εργασίας (RDP).

1. Βεβαιωθείτε ότι η βάση δεδομένων και τους πίνακες δημιουργούνται από τη βάση δεδομένων SQL Server προορισμού. Ακολουθεί ένα παράδειγμα του τρόπου για να το κάνετε αυτό με χρήση του `Create Database` και `Create Table` εντολές:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Δημιουργία του αρχείου μορφής που περιγράφει το σχήμα για τον πίνακα με την έκδοση την ακόλουθη εντολή από τη γραμμή εντολών του υπολογιστή όπου είναι εγκατεστημένο το bcp.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Εισαγάγετε τα δεδομένα στη βάση δεδομένων χρησιμοποιώντας την εντολή bcp ως εξής. Αυτό θα πρέπει να λειτουργεί από τη γραμμή εντολών, με την προϋπόθεση ότι το SQL Server είναι εγκατεστημένη στον ίδιο υπολογιστή:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Βελτιστοποίηση BCP εισάγει** Ανατρέξτε στο ακόλουθο άρθρο ['Οδηγίες για τη βελτιστοποίηση της μαζικής εισαγωγής'](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) για να βελτιστοποιήσετε όπως εισάγεται.


### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing εισάγει για πιο γρήγορη κίνηση δεδομένων

Εάν τα δεδομένα που θέλετε να μετακινήσετε είναι μεγάλο, μπορείτε να επιταχύνετε πράγματα εκτελώντας ταυτόχρονα πολλές εντολές BCP παράλληλα σε μια δέσμη ενεργειών PowerShell.

> [AZURE.NOTE]**Μεγάλο δεδομένων κατάποσης** Για να βελτιστοποιήσετε δεδομένων φόρτωση για μεγάλες και πολύ μεγάλων συνόλων δεδομένων, διαμερίσματα πίνακες της βάσης δεδομένων λογική και φυσική χρησιμοποιώντας πολλούς πίνακες filegroups και διαμερίσματα. Για περισσότερες πληροφορίες σχετικά με τη δημιουργία και τη φόρτωση των δεδομένων σε πίνακες διαμερισμάτων, ανατρέξτε στο θέμα [Παράλληλες πίνακες διαμερισμάτων SQL φόρτωση](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Το δείγμα δέσμης ενεργειών PowerShell παρακάτω δείχνουν παράλληλες εισάγεται με χρήση bcp:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Ερώτημα SQL μαζικής εισαγωγής

[Ερώτημα SQL που εισαγάγετε μαζικής](https://msdn.microsoft.com/library/ms188365) μπορούν να χρησιμοποιηθούν για την εισαγωγή δεδομένων στη βάση δεδομένων από αρχεία γραμμής/στήλης με βάση (οι υποστηριζόμενοι τύποι καλύπτονται στα[Δεδομένα προετοιμασία για μαζική εξαγωγή ή εισαγωγή (SQL Server)](https://msdn.microsoft.com/library/ms188609)) το θέμα. 

Εδώ θα βρείτε ορισμένες εντολές δείγμα για μαζική εισαγωγή είναι ως εξής:  

1. Ανάλυση των δεδομένων σας και ορίστε τις επιλογές προσαρμοσμένης πριν από την εισαγωγή για να βεβαιωθείτε ότι η βάση δεδομένων SQL Server προϋποθέτει την ίδια μορφοποίηση για οποιαδήποτε ειδικά πεδία όπως ημερομηνίες. Ακολουθεί ένα παράδειγμα του τρόπου για να ορίσετε τη μορφή ημερομηνίας ως έτος-μήνας-ημέρα (Εάν τα δεδομένα σας περιέχουν την ημερομηνία σε μορφή έτος-μήνας-ημέρα):

        SET DATEFORMAT ymd; 
    
2. Εισαγωγή δεδομένων με χρήση προτάσεις μαζικής εισαγωγής:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Ενσωματωμένη βοηθητικών προγραμμάτων στον SQL Server

Μπορείτε να χρησιμοποιήσετε τις υπηρεσίες ενοποιήσεις SQL Server (SSIS) για να εισαγάγετε δεδομένα στο SQL Server Εικονική στην Azure από ένα απλό αρχείο. SSIS είναι διαθέσιμο σε δύο περιβάλλοντα studio. Για λεπτομέρειες, ανατρέξτε στο θέμα [υπηρεσίες ενοποίησης (SSIS) και Studio περιβάλλοντα](https://technet.microsoft.com/library/ms140028.aspx):

- Για λεπτομέρειες σχετικά με τα Εργαλεία δεδομένων του SQL Server, ανατρέξτε στο θέμα [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Για λεπτομέρειες σχετικά με τον "Οδηγό εισαγωγής/εξαγωγής", ανατρέξτε στο θέμα [Οδηγό SQL Server εισαγωγής και εξαγωγής](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Μετακίνηση δεδομένων από SQL Server εσωτερικής εγκατάστασης με τον SQL Server Azure Εικονική μηχανή

Μπορείτε επίσης να χρησιμοποιήσετε τις παρακάτω στρατηγικές μετεγκατάστασης:

1. [Ανάπτυξη μιας βάσης δεδομένων SQL Server σε έναν οδηγό Εικονική Microsoft Azure](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Εξαγωγή σε επίπεδο αρχείο](#export-flat-file) 
3. [Οδηγός μετεγκατάστασης της βάσης δεδομένων SQL](#sql-migration)
4. [Βάση δεδομένων πίσω ασφαλείας και επαναφορά](#sql-backup)

Περιγραφή κάθε μία από αυτές τις παρακάτω:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Ανάπτυξη μιας βάσης δεδομένων SQL Server σε έναν οδηγό Εικονική Microsoft Azure

Η **Ανάπτυξη μια βάση δεδομένων SQL Server σε έναν οδηγό Microsoft Azure Εικονική** είναι ένας τρόπος απλές και προτεινόμενες για τη μετακίνηση δεδομένων από μια παρουσία του SQL Server εσωτερικής εγκατάστασης με τον SQL Server σε μια Εικονική Azure. Για λεπτομερείς οδηγίες, καθώς και μια συζήτηση με τις άλλες εναλλακτικές επιλογές, ανατρέξτε στο θέμα [Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Εξαγωγή σε επίπεδο αρχείο

Διάφορες μεθόδους μπορεί να χρησιμοποιηθεί για τη μαζική εξαγωγή δεδομένων από μια SQL Server εσωτερικής εγκατάστασης, όπως περιγράφεται στο θέμα [μαζικής εισαγωγής και εξαγωγής των δεδομένων (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) . Αυτό το έγγραφο θα καλύπτει το πρόγραμμα αντίγραφο μαζικής (BCP) ως παράδειγμα. Μετά την εξαγωγή δεδομένων σε ένα επίπεδο αρχείο, μπορεί να εισαχθεί σε ένα άλλο SQL server με χρήση μαζικής εισαγωγής. 

1. Εξαγάγετε τα δεδομένα από τον SQL Server εσωτερικής εγκατάστασης σε ένα αρχείο χρησιμοποιώντας το βοηθητικό πρόγραμμα bcp ως εξής

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Να δημιουργήσετε τη βάση δεδομένων και τον πίνακα στον SQL Server Εικονική του Azure χρησιμοποιώντας το `create database` και `create table` για το σχήμα πίνακα εξαγωγής στο βήμα 1.

3. Δημιουργήστε ένα αρχείο σε μορφή για την περιγραφή του σχήματος πίνακα των δεδομένων που έχει εξαχθεί/εισαχθεί. Λεπτομέρειες του αρχείου μορφής περιγράφονται στο θέμα [Δημιουργία μορφή αρχείου (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Μορφοποίηση δημιουργία αρχείων όταν εκτελείται BCP από τον υπολογιστή SQL Server 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Μορφοποίηση δημιουργία αρχείων όταν εκτελούνται από απόσταση BCP σε σχέση με SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Χρησιμοποιήστε οποιαδήποτε από τις μεθόδους που περιγράφονται στην ενότητα [Μετακίνηση δεδομένων από το αρχείο προέλευσης](#filesource_to_sqlonazurevm) για να μετακινήσετε τα δεδομένα σε επίπεδο αρχείων σε ένα διακομιστή SQL.

### <a name="sql-migration"></a>Οδηγός μετεγκατάστασης της βάσης δεδομένων SQL

[Οδηγός μετεγκατάστασης βάσης δεδομένων SQL Server](http://sqlazuremw.codeplex.com/) παρέχει έναν εύχρηστο τρόπο για τη μετακίνηση δεδομένων μεταξύ των δύο παρουσίες του SQL server. Επιτρέπει στο χρήστη να αντιστοιχίσετε το σχήμα δεδομένων μεταξύ της προέλευσης και προορισμού πίνακες, επιλέξτε τους τύπους στηλών και διάφορες άλλες λειτουργίες. Χρησιμοποιεί μαζική αντιγραφή (BCP) στην περιοχή στο παρασκήνιο. Ένα στιγμιότυπο οθόνης από την οθόνη υποδοχής για τον Οδηγό μετεγκατάστασης βάσης δεδομένων SQL εμφανίζεται παρακάτω.  

![Οδηγός μετεγκατάστασης του SQL Server][2]

### <a name="sql-backup"></a>Βάση δεδομένων πίσω ασφαλείας και επαναφορά

SQL Server υποστηρίζει: 

1. [Βάση δεδομένων πίσω ασφαλείας και επαναφορά της λειτουργικότητας](https://msdn.microsoft.com/library/ms187048.aspx) (και τα δύο σε ένα τοπικό αρχείο ή bacpac εξαγωγή σε blob) και τις [Εφαρμογές επιπέδων δεδομένων](https://msdn.microsoft.com/library/ee210546.aspx) (χρησιμοποιώντας bacpac). 
2. Δυνατότητα για να δημιουργήσετε απευθείας ΣΠΣ SQL Server στην Azure με μια βάση δεδομένων της αντιγραμμένης ή αντιγραφή σε μια υπάρχουσα βάση δεδομένων SQL Azure. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [χρήση του Οδηγού αντιγραφής βάσης δεδομένων](https://msdn.microsoft.com/library/ms188664.aspx). 

Στιγμιότυπο οθόνης της βάσης δεδομένων πίσω ασφαλείας/Επαναφορά επιλογές από SQL Server Management Studio εμφανίζεται παρακάτω.

![Εργαλείο εισαγωγής SQL Server][1]

## <a name="resources"></a>Πόροι

[Μετεγκατάσταση βάσης δεδομένων με τον SQL Server σε μια Εικονική Azure](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server σε εικονικές μηχανές Windows Azure Επισκόπηση](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png

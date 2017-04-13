<properties
    pageTitle="Γρήγορα αποτελέσματα με τις εργασίες ελαστικότητας βάσης δεδομένων"
    description="πώς μπορείτε να χρησιμοποιήσετε τις εργασίες ελαστικότητας βάσης δεδομένων"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="ddove"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="ddove" />

# <a name="getting-started-with-elastic-database-jobs"></a>Γρήγορα αποτελέσματα με τις εργασίες ελαστικότητας βάσης δεδομένων

Ελαστικά βάσης δεδομένων εργασιών (έκδοση preview) για τη βάση δεδομένων SQL Azure σας επιτρέπει να αξιοπιστία εκτελέσει δέσμες ενεργειών T-SQL που εκτείνονται σε πολλές βάσεις δεδομένων κατά την αυτόματη επανάληψη και παρέχει εγγυήσεις ενδεχόμενη ολοκλήρωσης. Για περισσότερες πληροφορίες σχετικά με τη δυνατότητα εργασία ελαστικότητας βάσης δεδομένων, ανατρέξτε στο θέμα στη [σελίδα επισκόπηση δυνατοτήτων](sql-database-elastic-jobs-overview.md).

Αυτό το θέμα επεκτείνει το δείγμα βρέθηκε [γρήγορα](sql-database-elastic-scale-get-started.md)αποτελέσματα με το εργαλεία ελαστικότητας βάσης δεδομένων. Όταν ολοκληρωθεί, θα: Μάθετε πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τις εργασίες που διαχειρίζονται μια ομάδα σχετικών βάσεων δεδομένων. Δεν είναι απαραίτητο να χρησιμοποιήσετε τα εργαλεία ελαστικότητας κλίμακα προκειμένου να επωφεληθείτε από τα πλεονεκτήματα της ελαστικότητας εργασίες.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Κάντε λήψη και εκτελέστε το παράθυρο [Γρήγορα αποτελέσματα με το δείγμα εργαλεία ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Δημιουργία μιας shard Διαχείριση χάρτη χρησιμοποιώντας την εφαρμογή δείγματος

Εδώ θα δημιουργήσετε ένα χάρτη shard manager μαζί με διάφορες shards, ακολουθούμενο από εισαγωγή δεδομένων στο το shards. Εάν έχετε ήδη shards με sharded δεδομένα σε αυτά, μπορείτε να παραλείψετε τα παρακάτω βήματα και να μετακινηθείτε στην επόμενη ενότητα.

1. Δημιουργία και εκτέλεση του δείγματος εφαρμογής **Γρήγορα αποτελέσματα με τα εργαλεία ελαστικότητας βάσης δεδομένων** . Ακολουθήστε τα βήματα μέχρι βήμα 7 της ενότητας, [κάντε λήψη και εκτελέστε την εφαρμογή δείγματος](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools). Στο τέλος της βήμα 7, θα δείτε την ακόλουθη γραμμή εντολών:

    ![γραμμή εντολών][1]

2.  Στο παράθυρο εντολών, πληκτρολογήστε "1" και πατήστε το πλήκτρο **Enter**. Αυτό δημιουργεί το shard manager χάρτη και προσθέτει δύο shards στο διακομιστή. Στη συνέχεια, πληκτρολογήστε "3" και πατήστε το πλήκτρο **Enter**. Επαναλάβετε αυτήν την ενέργεια τέσσερις φορές. Αυτό εισάγει γραμμές δείγμα δεδομένων σε shards σας.

3.  Η [Πύλη Azure](https://portal.azure.com) θα πρέπει να εμφανίζουν τρεις νέες βάσεις δεδομένων στο διακομιστή σας v12:

    ![Visual Studio επιβεβαίωσης][2]

    Σε αυτό το σημείο, θα δημιουργήσουμε μια συλλογή προσαρμοσμένη βάση δεδομένων που απεικονίζει όλες τις βάσεις δεδομένων στο χάρτη shard. Αυτό θα σας επιτρέψει να δημιουργήσετε και να εκτελέσετε μια εργασία που να προσθέσετε έναν νέο πίνακα σε shards.

Εδώ θα συνήθως δημιουργήσουμε ένα χάρτη προορισμού shard, χρησιμοποιώντας το cmdlet **New-AzureSqlJobTarget** . Η βάση δεδομένων shard χάρτη manager πρέπει να οριστεί ως μια βάση δεδομένων προορισμού και, στη συνέχεια, το χάρτη συγκεκριμένες shard καθορίζεται ως προορισμό. Αντί για αυτό, θα απαρίθμηση όλες τις βάσεις δεδομένων στο διακομιστή και προσθέστε τις βάσεις δεδομένων για τη νέα προσαρμοσμένη συλλογή με εξαίρεση τα κύρια βάση δεδομένων.

##<a name="creates-a-custom-collection-and-adds-all-databases-in-the-server-to-the-custom-collection-target-with-the-exception-of-master"></a>Δημιουργεί μια προσαρμοσμένη συλλογή και προσθέτει όλες τις βάσεις δεδομένων στο διακομιστή στον προορισμό προσαρμοσμένη συλλογή με την εξαίρεση υποδείγματος.


    $customCollectionName = "dbs_in_server"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 
    $ResourceGroupName = "ddove_samples"
    $ServerName = "samples"
    $dbsinserver = Get-AzureRMSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    $dbsinserver | %{
    $currentdb = $_.DatabaseName 
    $ErrorActionPreference = "Stop"
    Write-Output ""

    Try
    {
       New-AzureSqlJobTarget -ServerName $ServerName -DatabaseName $currentdb | Write-Output
    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason
                
        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already a database target." 
        }

        else
        {
            throw $_
        }
    
    }

    Try
    {
        if ($currentdb -eq "master")
        {
            Write-Host $currentdb "will not be added custom collection target" $CustomCollectionName "."
        }

        else
        {
            Add-AzureSqlJobChildTarget -CustomCollectionName $CustomCollectionName -ServerName $ServerName -DatabaseName $currentdb
            Write-Host $currentdb "was added to" $CustomCollectionName "."
        }

    }
    Catch 
    {
        $ErrorMessage = $_.Exception.Message
        $ErrorCategory = $_.CategoryInfo.Reason

        if ($ErrorCategory -eq 'UniqueConstraintViolatedException')
        {
             Write-Host $currentdb "is already in the custom collection target" $CustomCollectionName"."
        }

        else
        {
            throw $_
        }
    }
    $ErrorActionPreference = "Continue"
}
    
## <a name="create-a-t-sql-script-for-execution-across-databases"></a>Δημιουργία δέσμης ενεργειών T-SQL για εκτέλεση σε βάσεις δεδομένων

    $scriptName = "NewTable"
    $scriptCommandText = "
    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'Test')
    BEGIN
        CREATE TABLE Test(
            TestId INT PRIMARY KEY IDENTITY,
            InsertionTime DATETIME2
        );
    END
    GO
    INSERT INTO Test(InsertionTime) VALUES (sysutcdatetime());
    GO"

    $script = New-AzureSqlJobContent -ContentName $scriptName -CommandText $scriptCommandText
    Write-Output $script

##<a name="create-the-job-to-execute-a-script-across-the-custom-group-of-databases"></a>Δημιουργία της εργασίας για να εκτελέσετε μια δέσμη ενεργειών σε προσαρμοσμένη ομάδα βάσεων δεδομένων

    $jobName = "create on server dbs"
    $scriptName = "NewTable"
    $customCollectionName = "dbs_in_server"
    $credentialName = "ddove66"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job


##<a name="execute-the-job"></a>Εκτέλεση της εργασίας 

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να εκτελέσετε μια υπάρχουσα εργασία:

Ενημερώστε την ακόλουθη μεταβλητή ώστε να αντικατοπτρίζει το όνομα της εργασίας που θέλετε να εκτελείται:

    $jobName = "create on server dbs"
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName 
    Write-Output $jobExecution
 
## <a name="retrieve-the-state-of-a-single-job-execution"></a>Ανάκτηση της κατάστασης μιας εκτέλεσης μία μόνο εργασία

Χρησιμοποιήστε το cmdlet **Get-AzureSqlJobExecution** ίδια με την παράμετρο **IncludeChildren** για να προβάλετε την κατάσταση των θυγατρικών εκτελέσεις εργασία, δηλαδή το συγκεκριμένο μέλος για κάθε εργασία εκτέλεσης κάθε βάση στοχευμένες από την εργασία.

    $jobExecutionId = "{Job Execution Id}"
    $jobExecutions = Get-AzureSqlJobExecution -JobExecutionId $jobExecutionId -IncludeChildren
    Write-Output $jobExecutions 

## <a name="view-the-state-across-multiple-job-executions"></a>Προβολή της κατάστασης σε πολλές εκτελέσεις εργασία

Το cmdlet **Get-AzureSqlJobExecution** έχει πολλές προαιρετικές παράμετροι που μπορούν να χρησιμοποιηθούν για να εμφανίσετε πολλές εκτελέσεις εργασία, φιλτράρεται μέσα από τις παραμέτρους που παρέχεται. Το παρακάτω παρουσιάζει ορισμένα από τα τους πιθανούς τρόπους για να χρησιμοποιήσετε Get-AzureSqlJobExecution:

Ανάκτηση όλα εκτελέσεις ενεργού επάνω επιπέδου εργασίας:

    Get-AzureSqlJobExecution

Ανάκτηση όλα εκτελέσεις επάνω επιπέδου εργασίας, συμπεριλαμβανομένων των εκτελέσεις Ανενεργή εργασία:

    Get-AzureSqlJobExecution -IncludeInactive

Ανάκτηση όλα εκτελέσεις εργασία θυγατρικό ενός αναγνωριστικού εκτέλεσης παρεχόμενη εργασία, συμπεριλαμβανομένων των εκτελέσεις Ανενεργή εργασία:

    $parentJobExecutionId = "{Job Execution Id}"
    Get-AzureSqlJobExecution -AzureSqlJobExecution -JobExecutionId $parentJobExecutionId –IncludeInactive -IncludeChildren

Ανάκτηση όλα εργασία εκτελέσεις δημιουργήθηκε με ένα χρονοδιάγραμμα / συνδυασμό, συμπεριλαμβανομένων των εργασιών Ανενεργή εργασία:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Get-AzureSqlJobExecution -JobName $jobName -ScheduleName $scheduleName -IncludeInactive

Ανάκτηση όλων των εργασιών στόχευσης καθορισμένο shard χάρτη, συμπεριλαμβανομένων των ανενεργές εργασίες:

    $shardMapServerName = "{Shard Map Server Name}"
    $shardMapDatabaseName = "{Shard Map Database Name}"
    $shardMapName = "{Shard Map Name}"
    $target = Get-AzureSqlJobTarget -ShardMapManagerDatabaseName $shardMapDatabaseName -ShardMapManagerServerName $shardMapServerName -ShardMapName $shardMapName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive

Ανάκτηση όλων των εργασιών στόχευσης μια καθορισμένη προσαρμοσμένη συλλογή, συμπεριλαμβανομένων ανενεργές εργασίες:

    $customCollectionName = "{Custom Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    Get-AzureSqlJobExecution -TargetId $target.TargetId –IncludeInactive
 
Ανάκτηση της λίστας των εκτελέσεις εργασίας εργασία μέσα σε μια συγκεκριμένη εργασία εκτέλεσης:

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Write-Output $jobTaskExecutions 

Ανάκτηση λεπτομέρειες εκτέλεσης της εργασίας έργου:

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να προβάλετε τις λεπτομέρειες της εκτέλεσης εργασιών μια εργασία, η οποία είναι ιδιαίτερα χρήσιμη κατά τον εντοπισμό σφαλμάτων αποτυχίες εκτέλεσης.

    $jobTaskExecutionId = "{Job Task Execution Id}"
    $jobTaskExecution = Get-AzureSqlJobTaskExecution -JobTaskExecutionId $jobTaskExecutionId
    Write-Output $jobTaskExecution

## <a name="retrieve-failures-within-job-task-executions"></a>Ανάκτηση αποτυχίες εντός εκτελέσεις εργασιών έργου

Το αντικείμενο JobTaskExecution περιλαμβάνει μια ιδιότητα για τον κύκλο ζωής των της εργασίας μαζί με μια ιδιότητα μηνύματος. Εάν μια εργασία εκτέλεση εργασιών απέτυχε, θα οριστεί η ιδιότητα κύκλου ζωής *απέτυχε* και θα οριστεί η ιδιότητα μηνύματος για να το μήνυμα εξαίρεσης που προκύπτει και η στοίβα. Εάν μια εργασία δεν ολοκληρώθηκε με επιτυχία, είναι σημαντικό για να προβάλετε τις λεπτομέρειες των εργασιών έργου που δεν ολοκληρώθηκε με επιτυχία για μια συγκεκριμένη εργασία.

    $jobExecutionId = "{Job Execution Id}"
    $jobTaskExecutions = Get-AzureSqlJobTaskExecution -JobExecutionId $jobExecutionId
    Foreach($jobTaskExecution in $jobTaskExecutions) 
        {
        if($jobTaskExecution.Lifecycle -ne 'Succeeded')
            {
            Write-Output $jobTaskExecution
            }
        }

## <a name="waiting-for-a-job-execution-to-complete"></a>Αναμονή για μια εργασία εκτέλεσης για την ολοκλήρωση

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί να περιμένετε για μια εργασία για να ολοκληρωθεί:

    $jobExecutionId = "{Job Execution Id}"
    Wait-AzureSqlJobExecution -JobExecutionId $jobExecutionId 

## <a name="create-a-custom-execution-policy"></a>Δημιουργία προσαρμοσμένου εκτέλεσης πολιτικής

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει τη δημιουργία προσαρμοσμένου εκτέλεσης πολιτικές που μπορούν να εφαρμοστούν κατά την έναρξη εργασιών.
  
Οι πολιτικές εκτέλεσης επιτρέπουν τη συγκεκριμένη στιγμή για τον ορισμό:

* Όνομα: Αναγνωριστικό για την πολιτική εκτέλεσης.
* Χρονικό όριο εκτύπωσης: Συνολικός χρόνος πριν από μια εργασία θα ακυρωθεί από ελαστικότητας εργασίες βάσης δεδομένων.
* Αρχικό χρονικό διάστημα επανάληψης: το χρονικό διάστημα αναμονής πριν από την πρώτη "Επανάληψη".
* Μέγιστο χρονικό διάστημα επανάληψης: Καπέλο διαστημάτων "Επανάληψη" για να χρησιμοποιήσετε.
* Επανάληψη συντελεστή διπλασιασμών διάστημα: Συντελεστή που χρησιμοποιούνται για τον υπολογισμό το επόμενο χρονικό διάστημα μεταξύ των επαναλήψεων.  Ο παρακάτω τύπος χρησιμοποιείται: (αρχικό χρονικό διάστημα επανάληψης) * Math.pow ((συντελεστής διπλασιασμών διάστημα), (αριθμός επαναλήψεων) - 2). 
* Μέγιστος αριθμός προσπαθειών: Ο μέγιστος αριθμός των επαναλήψεων προσπαθεί να εκτελέσετε μέσα σε μια εργασία.

Η προεπιλεγμένη πολιτική εκτέλεσης χρησιμοποιεί τις ακόλουθες τιμές:

* Όνομα: Προεπιλεγμένη πολιτική εκτέλεσης
* Χρονικό όριο εργασίας: 1 εβδομάδα
* Αρχικό χρονικό διάστημα επανάληψης: 100 χιλιοστά του δευτερολέπτου
* Μέγιστο χρονικό διάστημα επανάληψης: 30 λεπτά
* Επανάληψη συντελεστή διάστημα: 2
* Μέγιστος αριθμός προσπαθειών: 2.147.483.647

Δημιουργήστε την πολιτική επιθυμητή εκτέλεσης:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 10
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $executionPolicy = New-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $executionPolicy

### <a name="update-a-custom-execution-policy"></a>Ενημέρωση μιας πολιτικής προσαρμοσμένο εκτέλεσης

Ενημερώστε την πολιτική εκτέλεσης θέλετε να ενημερώσετε:

    $executionPolicyName = "{Execution Policy Name}"
    $initialRetryInterval = New-TimeSpan -Seconds 15
    $jobTimeout = New-TimeSpan -Minutes 30
    $maximumAttempts = 999999
    $maximumRetryInterval = New-TimeSpan -Minutes 1
    $retryIntervalBackoffCoefficient = 1.5
    $updatedExecutionPolicy = Set-AzureSqlJobExecutionPolicy -ExecutionPolicyName $executionPolicyName -InitialRetryInterval $initialRetryInterval -JobTimeout $jobTimeout -MaximumAttempts $maximumAttempts -MaximumRetryInterval $maximumRetryInterval -RetryIntervalBackoffCoefficient $retryIntervalBackoffCoefficient
    Write-Output $updatedExecutionPolicy
 
## <a name="cancel-a-job"></a>Ακύρωση μιας εργασίας

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει αιτήματα ακύρωσης εργασίες.  Εάν ελαστικότητας εργασίες βάσης δεδομένων εντοπίσει μια αίτηση ακύρωσης για μια εργασία που εκτελείται τη συγκεκριμένη στιγμή, θα επιχειρήσει να διακόψετε την εργασία.

Υπάρχουν δύο τρόποι με τους ότι ελαστικότητας εργασίες βάσης δεδομένων μπορεί να εκτελέσει Ακύρωση:

1. Ακύρωση αυτήν τη στιγμή εκτελούνται εργασίες: σε περίπτωση που εντοπιστεί Ακύρωση ενώ εκτελείται τη συγκεκριμένη στιγμή μια εργασία, θα εφαρμοστούν Ακύρωση εντός του εκτελείται πτυχή της εργασίας.  Για παράδειγμα: Εάν υπάρχει μεγάλης διάρκειας ερωτήματος που εκτελούνται τη συγκεκριμένη στιγμή όταν επιχειρείται ακύρωση, θα υπάρξει μιας προσπάθειας για να ακυρώσετε το ερώτημα.
2. Ακύρωση επαναλήψεις εργασίας: Εάν Ακύρωση εντοπιστεί από το στοιχείο ελέγχου νήμα πριν από μια εργασία γίνεται εκκίνηση για εκτέλεση, το στοιχείο ελέγχου νήμα θα αποφύγετε την εκκίνηση της εργασίας και δηλώνουν την αίτηση ως ακυρώθηκε.

Εάν ζητείται Ακύρωση εργασίας για ένα έργο γονικό, την αίτηση ακύρωσης θα εξακολουθήσουν να ισχύουν για τη γονική εργασία και για όλες τις εργασίες θυγατρικό.
 
Για να υποβάλετε μια αίτηση ακύρωσης, χρησιμοποιήστε το cmdlet **Διακοπή AzureSqlJobExecution** και ορίστε την παράμετρο **JobExecutionId** .

    $jobExecutionId = "{Job Execution Id}"
    Stop-AzureSqlJobExecution -JobExecutionId $jobExecutionId

## <a name="delete-a-job-by-name-and-the-jobs-history"></a>Διαγραφή μιας εργασίας από το όνομα και το ιστορικό του έργου

Ελαστικά εργασίες βάσης δεδομένων υποστηρίζει ασύγχρονης διαγραφής των εργασιών. Μια εργασία να επισημανθεί για διαγραφή και το σύστημα θα διαγράψει η εργασία και όλο το ιστορικό εργασία αφού έχετε ολοκληρώσει όλες εκτελέσεις εργασία για την εργασία. Το σύστημα δεν θα ακυρώσει αυτόματα εκτελέσεις ενεργού εργασίας.  

Αντί για αυτό, πρέπει να είναι δυνατή διακοπή AzureSqlJobExecution για να ακυρώσετε εκτελέσεις ενεργού εργασίας.

Για να ενεργοποιήσετε τη διαγραφή εργασία, χρησιμοποιήστε το cmdlet **Κατάργηση AzureSqlJob** και ορίστε την παράμετρο **όνομα_εργασίας** .

    $jobName = "{Job Name}"
    Remove-AzureSqlJob -JobName $jobName
 
## <a name="create-a-custom-database-target"></a>Δημιουργήστε μια προσαρμοσμένη βάση δεδομένων προορισμού
Προορισμών προσαρμοσμένης βάσης δεδομένων μπορεί να οριστεί σε έργα ελαστικότητας βάσης δεδομένων που μπορεί να χρησιμοποιηθεί για εκτέλεση απευθείας είτε για να συμπεριληφθούν μέσα σε μια ομάδα προσαρμοσμένης βάσης δεδομένων. Επειδή **τα σύνολα ελαστικότητας βάσης δεδομένων** δεν ακόμη απευθείας υποστηρίζονται μέσω τα API του PowerShell, μπορείτε απλώς να δημιουργήσετε μια προσαρμοσμένη βάση δεδομένων προορισμού και προορισμού συλλογής προσαρμοσμένη βάση δεδομένων που περιλαμβάνει όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει τις πληροφορίες της βάσης δεδομένων που θέλετε:

    $databaseName = "{Database Name}"
    $databaseServerName = "{Server Name}"
    New-AzureSqlJobDatabaseTarget -DatabaseName $databaseName -ServerName $databaseServerName 

## <a name="create-a-custom-database-collection-target"></a>Δημιουργήστε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής
Ο στόχος συλλογής προσαρμοσμένης βάσης δεδομένων μπορεί να οριστεί για να ενεργοποιήσετε την εκτέλεση σε πολλούς στόχους καθορισμένη βάση δεδομένων. Μετά τη δημιουργία μιας ομάδας βάσης δεδομένων, βάσεις δεδομένων μπορεί να είναι συσχετισμένη με την προσαρμοσμένη συλλογή προορισμού.

Ορίστε τις παρακάτω μεταβλητές ώστε να αντικατοπτρίζει τις παραμέτρους θέλετε προσαρμοσμένο συλλογής προορισμού:

    $customCollectionName = "{Custom Database Collection Name}"
    New-AzureSqlJobTarget -CustomCollectionName $customCollectionName 

### <a name="add-databases-to-a-custom-database-collection-target"></a>Προσθήκη βάσεων δεδομένων σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Βάση δεδομένων των προορισμών μπορούν να συσχετιστούν με προορισμών συλλογής προσαρμοσμένη βάση δεδομένων για να δημιουργήσετε μια ομάδα βάσεων δεδομένων. Κάθε φορά που δημιουργείται ένα έργο που πρόκειται να εφαρμοστεί ο στόχος συλλογής προσαρμοσμένη βάση δεδομένων, θα αναπτυχθούν στόχευσης τις βάσεις δεδομένων που σχετίζεται με την ομάδα τη στιγμή της εκτέλεσης.

Προσθέστε την επιθυμητή βάση δεδομένων σε μια συγκεκριμένη συλλογή προσαρμοσμένο:

    $serverName = "{Database Server Name}"
    $databaseName = "{Database Name}"
    $customCollectionName = "{Custom Database Collection Name}"
    Add-AzureSqlJobChildTarget -CustomCollectionName $customCollectionName -DatabaseName $databaseName -ServerName $databaseServerName 

#### <a name="review-the-databases-within-a-custom-database-collection-target"></a>Εξετάστε τις βάσεις δεδομένων μέσα σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Χρησιμοποιήστε το cmdlet **Get-AzureSqlJobTarget** για την ανάκτηση των θυγατρικών βάσεων δεδομένων μέσα σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής. 
 
    $customCollectionName = "{Custom Database Collection Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $childTargets = Get-AzureSqlJobTarget -ParentTargetId $target.TargetId
    Write-Output $childTargets

### <a name="create-a-job-to-execute-a-script-across-a-custom-database-collection-target"></a>Δημιουργία μιας εργασίας για να εκτελέσετε μια δέσμη ενεργειών σε μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής

Χρησιμοποιήστε το cmdlet **New-AzureSqlJob** για να δημιουργήσετε μια εργασία σε σχέση με μια ομάδα βάσεις δεδομένων που ορίζονται από μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής. Ελαστικά εργασίες βάσης δεδομένων θα αναπτύξετε το έργο σε πολλές εργασίες θυγατρικό κάθε αντιστοιχεί σε μια βάση δεδομένων που σχετίζεται με το στόχο συλλογής προσαρμοσμένη βάση δεδομένων και βεβαιωθείτε ότι η δέσμη ενεργειών έχει εκτελεστεί σε κάθε βάση δεδομένων. Ξανά, είναι σημαντικό ότι οι δέσμες ενεργειών είναι idempotent να είναι ανθεκτικά στις επαναλήψεις.

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $customCollectionName = "{Custom Collection Name}"
    $credentialName = "{Credential Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $credentialName -ContentName $scriptName -TargetId $target.TargetId
    Write-Output $job

## <a name="data-collection-across-databases"></a>Συλλογή δεδομένων σε βάσεις δεδομένων

**Εργασίες ελαστικότητας βάσης δεδομένων** υποστηρίζει την εκτέλεση ενός ερωτήματος σε μια ομάδα βάσεων δεδομένων και στέλνει τα αποτελέσματα σε μια καθορισμένη βάση δεδομένων του πίνακα. Στον πίνακα μπορούν να αναζητηθούν μετά το γεγονός για να δείτε τα αποτελέσματα του ερωτήματος από κάθε βάση δεδομένων. Αυτό παρέχει ένα ασύγχρονης μηχανισμό για την εκτέλεση ενός ερωτήματος σε πολλές βάσεις δεδομένων. Αποτυχία περιπτώσεις όπως μία από τις βάσεις δεδομένων που είναι προσωρινά μη διαθέσιμος αντιμετωπίζονται αυτόματα μέσω επαναλήψεις.

Ο πίνακας προορισμού καθορισμένο θα δημιουργηθεί αυτόματα αν αυτό δεν υπάρχει ακόμα, που ταιριάζουν με το σχήμα του συνόλου αποτελεσμάτων που επιστράφηκε. Εάν μια δέσμη ενεργειών εκτέλεσης επιστρέφει πολλαπλά σύνολα αποτελεσμάτων, εργασίες ελαστικότητας βάσης δεδομένων θα στέλνει μόνο το πρώτο στον πίνακα προορισμού που παρέχεται.

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για την εκτέλεση μιας δέσμης ενεργειών τη συλλογή τα αποτελέσματά του σε έναν καθορισμένο πίνακα. Αυτή η δέσμη ενεργειών προϋποθέτει ότι μια δέσμη ενεργειών T-SQL έχει δημιουργηθεί που εξάγει ένα μονό σύνολο αποτελεσμάτων και έχει δημιουργηθεί μια προσαρμοσμένη βάση δεδομένων προορισμού συλλογής.

Ορίστε τα εξής για να απεικονίσει την επιθυμητή δέσμη ενεργειών διαπιστευτήρια και εκτέλεσης προορισμού:

    $jobName = "{Job Name}"
    $scriptName = "{Script Name}"
    $executionCredentialName = "{Execution Credential Name}"
    $customCollectionName = "{Custom Collection Name}"
    $destinationCredentialName = "{Destination Credential Name}"
    $destinationServerName = "{Destination Server Name}"
    $destinationDatabaseName = "{Destination Database Name}"
    $destinationSchemaName = "{Destination Schema Name}"
    $destinationTableName = "{Destination Table Name}"
    $target = Get-AzureSqlJobTarget -CustomCollectionName $customCollectionName

### <a name="create-and-start-a-job-for-data-collection-scenarios"></a>Δημιουργία και να ξεκινήσει μια εργασία για σενάρια συλλογής δεδομένων
    $job = New-AzureSqlJob -JobName $jobName -CredentialName $executionCredentialName -ContentName $scriptName -ResultSetDestinationServerName $destinationServerName -ResultSetDestinationDatabaseName $destinationDatabaseName -ResultSetDestinationSchemaName $destinationSchemaName -ResultSetDestinationTableName $destinationTableName -ResultSetDestinationCredentialName $destinationCredentialName -TargetId $target.TargetId
    Write-Output $job
    $jobExecution = Start-AzureSqlJobExecution -JobName $jobName
    Write-Output $jobExecution

## <a name="create-a-schedule-for-job-execution-using-a-job-trigger"></a>Δημιουργία χρονοδιαγράμματος για εκτέλεση εργασίας χρησιμοποιώντας ένα έναυσμα εργασίας

Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα χρονοδιάγραμμα επαναλαμβανόμενη εργασία. Αυτή η δέσμη ενεργειών χρησιμοποιεί ένα διάστημα και ένα λεπτό, αλλά δημιουργία AzureSqlJobSchedule υποστηρίζει επίσης - DayInterval, - HourInterval, - MonthInterval, και - WeekInterval παραμέτρους. Μπορούν να δημιουργηθούν με χρονοδιαγράμματα που εκτελούνται μόνο μία φορά με διέρχεται - ενημερώσεις.

Δημιουργία νέου χρονοδιαγράμματος:

    $scheduleName = "Every one minute"
    $minuteInterval = 1
    $startTime = (Get-Date).ToUniversalTime()
    $schedule = New-AzureSqlJobSchedule -MinuteInterval $minuteInterval -ScheduleName $scheduleName -StartTime $startTime 
    Write-Output $schedule

### <a name="create-a-job-trigger-to-have-a-job-executed-on-a-time-schedule"></a>Δημιουργήστε ένα έναυσμα εργασίας για να έχετε μια εργασία που εκτελείται στο χρονοδιάγραμμα

Ένα έναυσμα εργασίας μπορεί να οριστεί να έχουν εκτελεστεί σύμφωνα με το χρονοδιάγραμμα έργου. Η ακόλουθη δέσμη ενεργειών PowerShell μπορεί να χρησιμοποιηθεί για να δημιουργήσετε ένα έναυσμα εργασίας.

Ορίστε τις παρακάτω μεταβλητές για να αντιστοιχούν στις επιθυμητές εργασία και χρονοδιάγραμμα:

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    $jobTrigger = New-AzureSqlJobTrigger -ScheduleName $scheduleName –JobName $jobName
    Write-Output $jobTrigger

### <a name="remove-a-scheduled-association-to-stop-job-from-executing-on-schedule"></a>Κατάργηση μιας προγραμματισμένη συσχέτισης για να διακόψετε την εργασία από την εκτέλεση στο χρονοδιάγραμμα

Για να διακόψετε την επαναλαμβανόμενη εργασία Εκτέλεση εργασίας μέσω ένα έναυσμα εργασία, το έναυσμα εργασίας μπορούν να καταργηθούν. Καταργήστε ένα έναυσμα εργασίας για να διακόψετε μια εργασία από την εκτέλεση σύμφωνα με ένα χρονοδιάγραμμα χρησιμοποιώντας το cmdlet **Κατάργηση AzureSqlJobTrigger** .

    $jobName = "{Job Name}"
    $scheduleName = "{Schedule Name}"
    Remove-AzureSqlJobTrigger -ScheduleName $scheduleName -JobName $jobName





## <a name="import-elastic-database-query-results-to-excel"></a>Εισαγωγή αποτελεσμάτων ερωτήματος ελαστικότητας βάσης δεδομένων στο Excel

 Μπορείτε να εισαγάγετε τα αποτελέσματα από ένα ερώτημα σε ένα αρχείο Excel.

1. Εκκινήστε το Excel 2013.
2.  Περιηγηθείτε στην κορδέλα **δεδομένα** .
3.  Κάντε κλικ στην επιλογή **Από άλλες προελεύσεις** και κάντε κλικ στην επιλογή **Από SQL Server**.

    ![Excel εισαγωγή από άλλες προελεύσεις][5]
4.  Στον **"Οδηγό σύνδεσης δεδομένων"** , πληκτρολογήστε τα διαπιστευτήρια διακομιστή ονομάτων και συνδεθείτε. Στη συνέχεια, κάντε κλικ στο κουμπί **Επόμενο**.
5.  Στο παράθυρο διαλόγου, **Επιλέξτε τη βάση δεδομένων που περιέχει τα δεδομένα που θέλετε**, επιλέξτε τη βάση δεδομένων **ElasticDBQuery** .
6.  Επιλέξτε τον πίνακα **πελάτες** στην προβολή λίστας και κάντε κλικ στο κουμπί **Επόμενο**. Στη συνέχεια, κάντε κλικ στο κουμπί **Τέλος**.
7.  Στη φόρμα **Δεδομένων εισαγωγής** , στην περιοχή **Επιλέξτε πώς θέλετε να προβάλετε τα δεδομένα στο βιβλίο εργασίας σας**, επιλέξτε τον **πίνακα** και κάντε κλικ στο κουμπί **OK**.

Όλες τις γραμμές από τον πίνακα " **πελάτες** ", είναι αποθηκευμένα σε διαφορετικό shards συμπληρώσετε το φύλλο του Excel.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα, μπορείτε να χρησιμοποιήσετε συναρτήσεις δεδομένων του Excel. Χρησιμοποιήστε τη συμβολοσειρά σύνδεσης με το όνομα του διακομιστή, όνομα βάσης δεδομένων και τα διαπιστευτήρια για σύνδεση σας εργαλεία ενοποίησης BI και τα δεδομένα στη βάση δεδομένων του ελαστικότητας ερωτήματος. Βεβαιωθείτε ότι υποστηρίζεται SQL Server ως προέλευση δεδομένων για το εργαλείο. Ανατρέξτε στο ελαστικότητας ερώτημα βάσης δεδομένων και εξωτερικών πίνακες όπως οποιαδήποτε άλλη βάση δεδομένων SQL Server και πίνακες του SQL Server που θα συνδεθεί με το εργαλείο.

### <a name="cost"></a>Κόστος
Δεν υπάρχει χωρίς πρόσθετη χρέωση για τη χρήση της δυνατότητας ερωτήματος ελαστικότητας βάσης δεδομένων. Ωστόσο, προς το παρόν, αυτή η δυνατότητα είναι διαθέσιμη μόνο σε βάσεις δεδομένων premium ως ένα τελικό σημείο, αλλά μπορεί να είναι η shards από οποιοδήποτε επίπεδο υπηρεσιών.

Για πληροφορίες για τις τιμές ανατρέξτε [Λεπτομέρειες τιμολόγησης βάσης δεδομένων SQL](https://azure.microsoft.com/pricing/details/sql-database/).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->

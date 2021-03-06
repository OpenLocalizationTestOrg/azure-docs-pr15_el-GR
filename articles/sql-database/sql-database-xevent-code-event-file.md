<properties 
    pageTitle="Αρχείο συμβάν XEvent κώδικα για βάση δεδομένων SQL | Microsoft Azure" 
    description="Παρέχει PowerShell και Transact-SQL για ένα δείγμα κώδικα σε δύο φάσεις που παρουσιάζει ο προορισμός αρχείων συμβάν σε ένα εκτεταμένο συμβάν στη βάση δεδομένων SQL Azure. Azure χώρου αποθήκευσης είναι αναπόσπαστο μέρος της αυτό το σενάριο." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Συμβάν αρχείου προορισμού κώδικα για εκτεταμένο συμβάντα σε βάση δεδομένων SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Θέλετε ένα δείγμα ολοκλήρωσης κώδικα για έναν ισχυρό τρόπο καταγραφής και αναφορά πληροφοριών για ένα εκτεταμένο συμβάν.


Στο Microsoft SQL Server, [συμβάν αρχείου προορισμού](http://msdn.microsoft.com/library/ff878115.aspx) χρησιμοποιείται για την αποθήκευση εξόδους συμβάν σε ένα αρχείο τοπική μονάδα σκληρού δίσκου. Αλλά αυτά τα αρχεία δεν είναι διαθέσιμες σε βάση δεδομένων SQL Azure. Αντί για αυτό μπορούμε να χρησιμοποιήσουμε την υπηρεσία αποθήκευσης Azure για την υποστήριξη συμβάν αρχείου προορισμού.


Αυτό το θέμα παρουσιάζει ένα δείγμα κώδικα σε δύο φάσεις:


- PowerShell, για να δημιουργήσετε ένα κοντέινερ Azure χώρου αποθήκευσης στο cloud.

- Transact-SQL:
 - Για να εκχωρήσετε το χώρο αποθήκευσης Azure κοντέινερ για ένα συμβάν αρχείου προορισμού.
 - Για να δημιουργήσετε και ξεκινήστε την περίοδο λειτουργίας του συμβάντος και ούτω καθεξής.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία


- Μια λογαριασμός Azure και τη συνδρομή. Μπορείτε να εγγραφείτε για μια [δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/).


- Μπορείτε να δημιουργήσετε έναν πίνακα σε οποιαδήποτε βάση δεδομένων.
 - Προαιρετικά μπορείτε να [δημιουργήσετε μια βάση δεδομένων της επίδειξης **AdventureWorksLT** ](sql-database-get-started.md) σε λεπτά.


- SQL Server Management Studio (ssms.exe), ιδανικά την πιο πρόσφατη μηνιαία ενημέρωση έκδοση. Μπορείτε να λάβετε την πιο πρόσφατη ssms.exe από:
 - Θέμα με τίτλο [Κάντε λήψη του SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Μια απευθείας σύνδεση για τη λήψη.](http://go.microsoft.com/fwlink/?linkid=616025)


- Πρέπει να έχετε εγκαταστήσει [λειτουργικές μονάδες Azure PowerShell](http://go.microsoft.com/?linkid=9811175) .
 - Οι λειτουργικές μονάδες παρέχουν εντολές, όπως - **Δημιουργία AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Φάση 1: Κώδικα του PowerShell για κοντέινερ χώρου αποθήκευσης Azure


Σε αυτό το PowerShell είναι φάση 1 του δείγματος σε δύο φάσεις κώδικα.

Η δέσμη ενεργειών ξεκινά με εντολές για την εκκαθάριση μετά την εκτέλεση μιας προηγούμενης δυνατόν και είναι rerunnable.



1. Επικολλήστε τη δέσμη ενεργειών PowerShell σε ένα πρόγραμμα επεξεργασίας απλού κειμένου, όπως το Notepad.exe και αποθηκεύστε τη δέσμη ενεργειών ως αρχείο με την επέκταση **.ps1**.

2. Εκκίνηση του PowerShell ISE ως διαχειριστής.

3. Στη γραμμή εντολών, πληκτρολογήστε<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>και, στη συνέχεια, πατήστε το πλήκτρο Enter.

4. Στο PowerShell ISE, ανοίξτε το αρχείο **.ps1** . Εκτελέστε τη δέσμη ενεργειών.

5. Η δέσμη ενεργειών ξεκινά πρώτα ένα νέο παράθυρο στο οποίο συνδέεστε στο Azure.
 - Εάν τη δέσμη χωρίς να διακόψετε την περίοδο λειτουργίας σας, έχετε την εύκολη επιλογή της εντολής **AzureAccount Προσθήκη** σχολίων.


![PowerShell ISE, με Azure λειτουργική μονάδα εγκατεστημένες, είστε έτοιμοι να εκτελέσετε τη δέσμη ενεργειών.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Λάβετε υπόψη τις μερικές τιμές με όνομα που εκτυπώνεται η δέσμη ενεργειών PowerShell όταν τελειώνει. Πρέπει να μπορείτε να επεξεργαστείτε αυτές τις τιμές σε τη δέσμη ενεργειών Transact-SQL που ακολουθεί ως φάση 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Φάση 2: Transact-SQL κώδικα που χρησιμοποιεί το κοντέινερ χώρου αποθήκευσης Azure


- Σε αυτό το δείγμα κώδικα φάση 1, εκτελέσατε μια δέσμη ενεργειών του PowerShell για να δημιουργήσετε ένα κοντέινερ χώρου αποθήκευσης Azure.
- Δίπλα στη φάση 2, η ακόλουθη δέσμη ενεργειών Transact-SQL πρέπει να χρησιμοποιήσετε το κοντέινερ.


Η δέσμη ενεργειών ξεκινά με εντολές για την εκκαθάριση μετά την εκτέλεση μιας προηγούμενης δυνατόν και είναι rerunnable.


Η δέσμη ενεργειών PowerShell εκτυπώνονται μερικά ονομαστικών τιμές, όταν το τερματίστηκε. Πρέπει να επεξεργαστείτε τη δέσμη ενεργειών Transact-SQL για να χρησιμοποιήσετε αυτές τις τιμές. Βρείτε **TODO** στη δέσμη ενεργειών Transact-SQL για να εντοπίσετε την Επεξεργασία σημείων.


1. Ανοίξτε το SQL Server Management Studio (ssms.exe).

2. Σύνδεση στη βάση δεδομένων της βάσης δεδομένων SQL Azure.

3. Κάντε κλικ για να ανοίξετε ένα νέο παράθυρο ερωτήματος.

4. Επικολλήστε την ακόλουθη δέσμη ενεργειών Transact-SQL στο παράθυρο ερωτήματος.

5. Βρείτε κάθε **TODO** στη δέσμη ενεργειών και κάντε τις κατάλληλες αλλαγές.

6. Αποθήκευση και, στη συνέχεια, εκτελέστε τη δέσμη ενεργειών.


&nbsp;


> [AZURE.WARNING] Η τιμή του κλειδιού συσχετισμών Ασφαλείας που δημιουργούνται από την προηγούμενη δέσμη ενεργειών PowerShell μπορεί να ξεκινά με ένα '; ' (λατινικό ερωτηματικό). Όταν χρησιμοποιείτε το πλήκτρο συσχετισμών Ασφαλείας στο την ακόλουθη δέσμη ενεργειών T-SQL, πρέπει να *καταργήσετε το διάστιχο '?'*. Διαφορετικά προσπάθειες σας μπορεί να έχει αποκλειστεί από την ασφάλεια.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Εάν ο προορισμός αποτύχει να επισυνάψετε κατά την εκτέλεση, πρέπει να διακόψετε και επανεκκινήστε την περίοδο λειτουργίας συμβάν:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Εξόδου


Όταν ολοκληρωθεί η δέσμη ενεργειών Transact-SQL, επιλέξτε ένα κελί κάτω από την κεφαλίδα της στήλης **event_data_XML** . Μία **<event>** στοιχείο εμφανίζεται που εμφανίζει μία πρόταση UPDATE.

Ακολουθεί ένα **<event>** στοιχείο που δημιουργήθηκε κατά τις δοκιμές:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Η προηγούμενη δέσμη ενεργειών Transact-SQL χρησιμοποιείται η ακόλουθη συνάρτηση συστήματος για την ανάγνωση του event_file:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


Μια επεξήγηση σύνθετων επιλογών για την προβολή των δεδομένων από το εκτεταμένο συμβάντα είναι διαθέσιμο στο:

- [Προβολή δεδομένων προορισμού από το εκτεταμένο συμβάντων για προχωρημένους](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Μετατροπή το δείγμα κώδικα για την εκτέλεση στον SQL Server


Ας υποθέσουμε ότι θέλετε να εκτελέσετε το προηγούμενο παράδειγμα Transact-SQL στο Microsoft SQL Server.


- Για λόγους ευκολίας, θα θέλετε να αντικαταστήσετε εντελώς χρήση του κοντέινερ αποθήκευσης Azure με ένα απλό αρχείο όπως **C:\myeventdata.xel**. Το αρχείο μπορεί να εγγραφούν στον τοπικό σκληρό δίσκο του υπολογιστή που φιλοξενεί SQL Server.


- Δεν θα χρειαστείτε οποιοδήποτε είδος προτάσεις Transact-SQL για **ΔΗΜΙΟΥΡΓΊΑ ΠΡΩΤΕΎΟΝ ΚΛΕΙΔΊ** και **ΔΗΜΙΟΥΡΓΊΑ ΔΙΑΠΙΣΤΕΥΤΗΡΊΩΝ**.


- Στην πρόταση **ΣΥΜΒΆΝ ΔΗΜΙΟΥΡΓΊΑ περιόδου ΛΕΙΤΟΥΡΓΊΑΣ** , την **Προσθήκη ΠΡΟΟΡΙΣΜΟΎ** τον όρο FROM, πρέπει να αντικαταστήσετε την τιμή Http στους οποίους έχουν ανατεθεί που έχουν γίνει στις **filename =** με μια συμβολοσειρά πλήρης διαδρομή όπως **C:\myfile.xel**.
 - Πρέπει να συμμετέχουν, δεν υπάρχει λογαριασμός Azure χώρου αποθήκευσης.


## <a name="more-information"></a>Περισσότερες πληροφορίες


Για περισσότερες πληροφορίες σχετικά με τους λογαριασμούς και τα κοντέινερ στην υπηρεσία αποθήκευσης Azure, ανατρέξτε στα θέματα:

- [Πώς να χρησιμοποιείτε το χώρο αποθήκευσης αντικειμένων Blob από το .NET](../storage/storage-dotnet-how-to-use-blobs.md)
- [Ονομασία και αναφορά σε κοντέινερ, αντικείμενα BLOB και μετα-δεδομένων](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Εργασία με το κοντέινερ ρίζας](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Μάθημα 1: Δημιουργία μια πολιτική αποθηκευμένες πρόσβασης και μια υπογραφή κοινόχρηστη πρόσβαση σε ένα κοντέινερ Azure](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Μάθημα 2: Δημιουργήστε μια πιστοποίηση SQL Server χρησιμοποιώντας μια υπογραφή κοινόχρηστη πρόσβαση](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png


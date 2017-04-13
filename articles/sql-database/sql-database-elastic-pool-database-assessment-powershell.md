<properties
    pageTitle="Δέσμη ενεργειών του PowerShell για τον προσδιορισμό μία βάσεις δεδομένων που είναι κατάλληλη για ένα χώρο συγκέντρωσης | Microsoft Azure"
    description="Ένα χώρο συγκέντρωσης ελαστικότητας βάση δεδομένων είναι μια συλλογή των διαθέσιμων πόρων που χρησιμοποιούνται από κοινού από μια ομάδα ελαστικότητας βάσεων δεδομένων. Αυτό το έγγραφο παρέχει μια δέσμη ενεργειών του Powershell για να σας βοηθήσει αξιολόγηση της καταλληλότητας της χρήσης ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων για μια ομάδα με βάσεις δεδομένων."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/28/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>

# <a name="powershell-script-for-identifying-databases-suitable-for-an-elastic-database-pool"></a>Δέσμη ενεργειών του PowerShell για τον εντοπισμό βάσεις δεδομένων που είναι κατάλληλη για ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων

Το δείγμα δέσμης ενεργειών του PowerShell σε αυτό το άρθρο υπολογίζει τις τιμές συγκεντρωτικών αποτελεσμάτων eDTU για βάσεις δεδομένων χρήστη σε μια βάση δεδομένων SQL server. Η δέσμη ενεργειών συγκεντρώνει δεδομένα ενώ εκτελείται και για ένα τυπικό παραγωγής φόρτο εργασίας, θα πρέπει να εκτελέσετε τη δέσμη ενεργειών για τουλάχιστον μία ημέρα. Ιδανικά, που θέλετε να εκτελέσετε τη δέσμη ενεργειών για χρονικό διάστημα που αντιπροσωπεύει τις βάσεις δεδομένων κανονικό φόρτο εργασίας. Εκτελέστε τη δέσμη ενεργειών αρκετή ώρα με δεδομένα καταγραφής που αντιπροσωπεύει το κανονικό και μεγίστου χρήσης για τις βάσεις δεδομένων. Εκτέλεση της δέσμης ενεργειών εβδομάδα ή ακόμα περισσότερο πιθανώς θα σας δώσει μια πιο ακριβή εκτίμηση.

Αυτή η δέσμη ενεργειών είναι χρήσιμη για την αξιολόγηση των βάσεων δεδομένων σε διακομιστές V11: για μετεγκατάσταση σε διακομιστές v12, όπου υποστηρίζονται χώρους συγκέντρωσης. Σε διακομιστές v12, βάση δεδομένων SQL έχει ενσωματωμένα πληροφοριών που αναλύει ιστορικού χρήση τηλεμετρίας και προτείνει ένα χώρο συγκέντρωσης όταν θα είναι πιο οικονομική. Για πληροφορίες, ανατρέξτε στο θέμα [οθόνη, διαχείριση, και να αλλάξετε το μέγεθος ενός χώρου συγκέντρωσης ελαστικότητας βάση δεδομένων](sql-database-elastic-pool-manage-portal.md)

> [AZURE.IMPORTANT] Αφήστε ανοιχτό το παράθυρο PowerShell κατά την εκτέλεση της δέσμης ενεργειών. Μην κλείσετε το παράθυρο PowerShell μέχρι να εκτελέσετε τη δέσμη ενεργειών για το χρονικό διάστημα που απαιτείται. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία 

Εγκατάσταση του ακόλουθου πριν από την εκτέλεση της δέσμης ενεργειών:

- Η πιο πρόσφατη Azure PowerShell. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).
- Το [πακέτο δυνατοτήτων του SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=42295).

## <a name="script-details"></a>Λεπτομέρειες δέσμης ενεργειών

Μπορείτε να εκτελέσετε τη δέσμη ενεργειών από το τοπικό υπολογιστή σας ή μια Εικονική στο cloud. Όταν εκτελείτε το από τον τοπικό υπολογιστή, που ενδέχεται να υπάρξουν χρεώσεις δεδομένων εξόδου, επειδή η δέσμη ενεργειών πρέπει να κάνετε λήψη δεδομένων από τις βάσεις δεδομένων προορισμού. Η ακόλουθη εικόνα δείχνει εκτίμησης όγκο δεδομένων που βασίζονται σε διάφορες βάσεις δεδομένων προορισμού και τη διάρκεια της εκτέλεση της δέσμης ενεργειών. Για το κόστος μεταφοράς Azure δεδομένων, ανατρέξτε στο [Λεπτομέρειες τιμολόγησης μεταφορά δεδομένων](https://azure.microsoft.com/pricing/details/data-transfers/).
       
 -     Μία βάση δεδομένων ανά ώρα = 38 KB
 -     Μία βάση δεδομένων ανά ημέρα = 900 KB
 -     Μία βάση δεδομένων ανά εβδομάδα = 6 MB
 -     100 βάσεις δεδομένων ανά ημέρα = 90 MB
 -     500 βάσεις δεδομένων ανά εβδομάδα = 3 GB

Η δέσμη ενεργειών δεν μεταγλωττίζεται πληροφορίες για τις ακόλουθες βάσεις δεδομένων:

* Ελαστικά βάσεων δεδομένων (βάσεις δεδομένων ήδη σε ένα σύνολο ελαστικότητας)
* Κύρια βάση δεδομένων του διακομιστή

Εάν θέλετε να εξαιρέσετε πρόσθετες βάσεις δεδομένων από το διακομιστή προορισμού, να αλλάξετε τη δέσμη ενεργειών για να πληρούν τα κριτήριά σας. Από προεπιλογή.

Η δέσμη ενεργειών χρειάζεται μια βάση δεδομένων εξόδου για την αποθήκευση ενδιάμεσου δεδομένων για ανάλυση. Μπορείτε να χρησιμοποιήσετε μια νέα ή υπάρχουσα βάση δεδομένων. Παρόλο που δεν τεχνικά απαιτείται για να εκτελέσετε το εργαλείο, πρέπει να είναι η βάση δεδομένων εξόδου σε διαφορετικό διακομιστή για να αποφύγετε επηρεάζουν το αποτέλεσμα ανάλυσης. Πρέπει να είναι τουλάχιστον το επίπεδο απόδοσης της βάσης δεδομένων εξόδου S0 ή νεότερη έκδοση. Κατά τη συλλογή δεδομένων για πολλές βάσεις δεδομένων σε ένα μεγάλο χρονικό διάστημα, μπορείτε να την αναβάθμιση βάσης δεδομένων εξόδου σε ανώτερο επίπεδο επιδόσεων.

Η δέσμη ενεργειών πρέπει να καταχωρήσετε τα διαπιστευτήρια για να συνδεθείτε με το διακομιστή προορισμού (υποψηφίου ελαστικότητας βάσης δεδομένων χώρου συγκέντρωσης) με ένα πλήρες όνομα διακομιστή, <*όνομα βάσης δεδομένων*>**. database.windows.net**. Η δέσμη ενεργειών δεν υποστηρίζει την ανάλυση περισσότερα από ένα διακομιστή κάθε φορά.

Μετά την υποβολή τιμές για το αρχικό σύνολο των παραμέτρων, θα σας ζητηθεί να συνδεθείτε στο λογαριασμό σας Azure. Πρόκειται για τη σύνδεση με το διακομιστή προορισμού, όχι στο διακομιστή βάσης δεδομένων εξόδου.
    
Εάν αντιμετωπίσετε τις ακόλουθες προειδοποιήσεις κατά την εκτέλεση της δέσμης ενεργειών μπορείτε να αγνοήσετε τους:

- ΠΡΟΕΙΔΟΠΟΊΗΣΗ: Το cmdlet διακόπτη AzureMode έχει καταργηθεί.
- ΠΡΟΕΙΔΟΠΟΊΗΣΗ: Δεν ήταν δυνατή η λήψη πληροφοριών υπηρεσία SQL Server. Απέτυχε η προσπάθεια σύνδεση WMI σε 'Microsoft.Azure.Commands.Sql.dll' με το ακόλουθο σφάλμα: RPC ο διακομιστής δεν είναι διαθέσιμος.

Όταν ολοκληρωθεί η δέσμη ενεργειών, εξέρχεται τον εκτιμώμενο αριθμό eDTUs που χρειάζονται για ένα χώρο συγκέντρωσης ώστε να περιέχει όλες τις βάσεις δεδομένων υποψηφίου στο διακομιστή προορισμού. Αυτό εκτιμώμενη eDTU μπορεί να χρησιμοποιηθεί για τη δημιουργία και τη ρύθμιση των παραμέτρων του χώρου συγκέντρωσης. Όταν δημιουργείται το χώρο συγκέντρωσης και βάσεις δεδομένων μετακινηθεί στο χώρο συγκέντρωσης, παρακολουθείτε στενά το χώρο συγκέντρωσης για σε λίγες ημέρες και να κάνετε προσαρμογές στη ρύθμιση παραμέτρων eDTU χώρου συγκέντρωσης ανάλογα με τις απαιτήσεις. Ανατρέξτε στο θέμα [οθόνη, διαχείριση, και να αλλάξετε το μέγεθος ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-manage-portal.md).


    
```
param (
[Parameter(Mandatory=$true)][string]$AzureSubscriptionName, # Azure Subscription name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$ResourceGroupName, # Resource Group name - can be found on the Azure portal: https://portal.azure.com/
[Parameter(Mandatory=$true)][string]$servername, # full server name like "abcdefg.database.windows.net"
[Parameter(Mandatory=$true)][string]$username, # user name
[Parameter(Mandatory=$true)][string]$serverPassword, # password
[Parameter(Mandatory=$true)][string]$outputServerName, # metrics collection database for analysis. full server name like "zyxwvu.database.windows.net"
[Parameter(Mandatory=$true)][string]$outputdatabaseName,
[Parameter(Mandatory=$true)][string]$outputDBUsername,
[Parameter(Mandatory=$true)][string]$outputDBpassword,
[Parameter(Mandatory=$true)][int]$duration_minutes # How long to run. Recommend to run for the period of time when your typical workload is running. At least 10 mins.
)

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionName $AzureSubscriptionName

$server = Get-AzureRmSqlServer -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName

# Check version/upgrade status of the server
$upgradestatus = Get-AzureRmSqlServerUpgrade -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName
$version = ""
if ([string]::IsNullOrWhiteSpace($server.ServerVersion)) 
{
$version = $upgradestatus.Status
}
else
{
$version = $server.ServerVersion
}

# For Elastic database pool candidates, we exclude master, and any databases that are already in a pool. You may add more databases to the excluded list below as needed
$ListOfDBs = Get-AzureRmSqlDatabase -ServerName $servername.Split('.')[0] -ResourceGroupName $ResourceGroupName | Where-Object {$_.DatabaseName -notin ("master") -and $_.CurrentServiceLevelObjectiveName -notin ("ElasticPool") -and $_.CurrentServiceObjectiveName -notin ("ElasticPool")}

$outputConnectionString = "Data Source=$outputServerName;Integrated Security=false;Initial Catalog=$outputdatabaseName;User Id=$outputDBUsername;Password=$outputDBpassword"
$destinationTableName = "resource_stats_output"

# Create a table in output database for metrics collection
$sql = "
IF  NOT EXISTS (SELECT * FROM sys.objects 
WHERE object_id = OBJECT_ID(N'$($destinationTableName)') AND type in (N'U'))

BEGIN
Create Table $($destinationTableName) (database_name varchar(128), slo varchar(20), end_time datetime, avg_cpu float, avg_io float, avg_log float, db_size float);
Create Clustered Index ci_endtime ON $($destinationTableName) (end_time);
END
TRUNCATE TABLE $($destinationTableName);
"
Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -ConnectionTimeout 120 -QueryTimeout 120 

# waittime (minutes) is interval between data collection queries in the loop below.
$Waittime = 10
$end_Time = [DateTime]::UtcNow
$start_time = $end_time.AddMinutes(-$Waittime)
$finish_time = $end_Time.AddMinutes($duration_minutes)

While ($end_time -lt $finish_time)
{
Write-Host "Collecting metrics..." 
foreach ($db in $ListOfDBs)
{
if ($version -in ("12.0", "Completed")) # for V12 databases 
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'S3' THEN 100
WHEN 'P1' THEN 125
WHEN 'P2' THEN 250
WHEN 'P4' THEN 500
WHEN 'P6' THEN 1000
WHEN 'P11' THEN 1750
WHEN 'P15' THEN 4000
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 FROM sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}
else
{
$sql = "Declare @dbname varchar(128) = '$($db.DatabaseName)';"
$sql += "Declare @SLO varchar(20) = '$($db.CurrentServiceObjectiveName)';"
$sql+= "
Declare @DTU_cap int, @db_size float;
Select @DTU_cap = CASE @SLO 
WHEN 'Basic' THEN 5
WHEN 'S0' THEN 10
WHEN 'S1' THEN 20
WHEN 'S2' THEN 50
WHEN 'P1' THEN 100
WHEN 'P2' THEN 200
WHEN 'P3' THEN 800
ELSE 50 -- assume Web/Business DBs
END
SELECT @db_size = SUM(reserved_page_count) * 8.0/1024/1024 from sys.dm_db_partition_stats
SELECT @dbname as database_name, @SLO as SLO, dateadd(second, round(datediff(second, '2015-01-01', end_time) / 15.0, 0) * 15,'2015-01-01')
as end_time, avg_cpu_percent * (@DTU_cap/100.0) AS avg_cpu, avg_data_io_percent * (@DTU_cap/100.0) AS avg_io, avg_log_write_percent * (@DTU_cap/100.0) AS avg_log, @db_size as db_size FROM sys.dm_db_resource_stats
WHERE end_time > '$($start_time)' and end_time <= '$($end_time)';
" 
}

$result = Invoke-Sqlcmd -ServerInstance $servername -Database $db.DatabaseName -Username $username -Password $serverPassword -Query $sql -ConnectionTimeout 240 -QueryTimeout 3600 
#bulk copy the metrics to output database
$bulkCopy = new-object ("Data.SqlClient.SqlBulkCopy") $outputConnectionString 
$bulkCopy.BulkCopyTimeout = 600
$bulkCopy.DestinationTableName = "$destinationTableName";
$bulkCopy.WriteToServer($result);

}

$start_time = $start_time.AddMinutes($Waittime)
$end_time = $end_time.AddMinutes($Waittime)
Write-Host $start_time
Write-Host $end_time
do {
Start-Sleep 1
   }
until (([DateTime]::UtcNow) -ge $end_time)
}

Write-Host "Analyzing the collected metrics...."
# Analysis query that does aggregation of the resource metrics to calculate pool size.
$sql1 = 'Declare @DTU_Perf_99 as float, @DTU_Storage as float;
WITH group_stats AS
(
SELECT end_time, SUM(db_size) AS avg_group_Storage, SUM(avg_cpu) AS avg_group_cpu, SUM(avg_io) AS avg_group_io,SUM(avg_log) AS avg_group_log
FROM resource_stats_output 
WHERE slo LIKE '

$sql2 = '
GROUP BY end_time
)
-- calculate aggregate storage and DTUs for all DBs in the group
, group_DTU AS
(
SELECT end_time, avg_group_Storage, 
(SELECT Max(v)
   FROM (VALUES (avg_group_cpu), (avg_group_log), (avg_group_io)) AS value(v)) AS avg_group_DTU
FROM group_stats
)
-- Get top 1 percent of the storage and DTU utilization samples.
, top1_percent AS (
SELECT TOP 1 PERCENT avg_group_Storage, avg_group_dtu FROM group_dtu ORDER BY [avg_group_DTU] DESC
)

-- Max and 99th percentile DTU for the given list of databases if converted into an elastic pool. Storage is increased by factor of 1.25 to accommodate for future growth. Currently storage limit of the pool is determined by the amount of DTUs based on 1GB/DTU.
--SELECT MAX(avg_group_Storage)*1.25/1024.0 AS Group_Storage_DTU, MAX(avg_group_dtu) AS Group_Performance_DTU, MIN(avg_group_dtu) AS Group_Performance_DTU_99th_percentile FROM top1_percent;
SELECT @DTU_Storage = MAX(avg_group_Storage)*1.25/1024.0, @DTU_Perf_99 = MIN(avg_group_dtu) FROM top1_percent;
IF @DTU_Storage > @DTU_Perf_99 
SELECT ''Total number of DTUs dominated by storage: '' + convert(varchar(100), @DTU_Storage)
ELSE 
SELECT ''Total number of DTUs dominated by resource consumption: '' + convert(varchar(100), @DTU_Perf_99)'

#check if there are any web/biz edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Shared*")
{
write-host "`nWeb/Business edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any basic edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'Basic%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "Basic*")
{
write-host "`nBasic edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'Basic%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]} 
}

#check if there are any standard edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'S%' AND slo NOT LIKE 'Shared%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "S*")
{
write-host "`nStandard edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'S%' AND slo NOT LIKE 'Shared%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}

#check if there are any premium edition dbs in the collected metrics
$checkslo = "SELECT TOP 1 slo FROM resource_stats_output WHERE slo LIKE 'P%'"
$output = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $checkslo -QueryTimeout 3600 | select -expand slo
if ($output -like "P*")
{
write-host "`nPremium edition:" -BackgroundColor Green -ForegroundColor Black
$sql = $sql1 + "'P%'"  + $sql2
$data = Invoke-Sqlcmd -ServerInstance $outputServerName -Database $outputdatabaseName -Username $outputDBUsername -Password $outputDBpassword -Query $sql -QueryTimeout 3600
$data | %{'{0}' -f $_[0]}
}
```
        


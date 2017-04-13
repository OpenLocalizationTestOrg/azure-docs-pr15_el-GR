<properties
    pageTitle="Χρήση του PowerShell για να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους ενός χώρου εργασίας ανάλυση καταγραφής | Microsoft Azure"
    description="Καταγραφή δεδομένων χρησιμοποιεί αναλυτικών στοιχείων από τους διακομιστές σας στην εσωτερική εγκατάσταση ή υποδομή στο cloud. Μπορείτε να συλλέξετε δεδομένων υπολογιστή από το Azure χώρου αποθήκευσης όταν που δημιουργούνται από Azure Διαγνωστικά."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="powershell"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-powershell"></a>Διαχείριση καταγραφής αναλυτικών στοιχείων χρήσης του PowerShell

Μπορείτε να χρησιμοποιήσετε το [καταγραφής ανάλυσης των cmdlet του PowerShell] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) για να εκτελέσετε διάφορες λειτουργίες στο αρχείο καταγραφής αναλυτικών στοιχείων από τη γραμμή εντολών ή ως μέρος μιας δέσμης ενεργειών.  Οι εργασίες που μπορείτε να εκτελέσετε με το PowerShell παραδείγματα:

+ Δημιουργία χώρου εργασίας
+ Προσθήκη ή κατάργηση μιας λύσης
+ Εισαγωγή και εξαγωγή αποθηκευμένες αναζητήσεις
+ Δημιουργία ομάδας υπολογιστή
+ Ενεργοποίηση της συλλογής των αρχείων καταγραφής των υπηρεσιών IIS από τους υπολογιστές με εγκαταστήσει τον παράγοντα Windows
+ Συλλογή μετρητών επιδόσεων από υπολογιστές Linux και Windows
+ Συλλογή συμβάντα από syslog Linux υπολογιστές 
+ Συλλογή συμβάντα από αρχεία καταγραφής συμβάντων των Windows
+ Συλλογή προσαρμοσμένα αρχεία καταγραφής συμβάντων
+ Προσθήκη τον παράγοντα καταγραφής αναλυτικών στοιχείων σε μια εικονική μηχανή Azure
+ Ρύθμιση παραμέτρων καταγραφής ανάλυσης για ευρετήριο δεδομένα που συλλέγονται χρησιμοποιώντας τα Διαγνωστικά του Azure


Σε αυτό το άρθρο παρέχει δύο δείγματα κώδικα που απεικονίζουν ορισμένες από τις συναρτήσεις που μπορείτε να εκτελέσετε από PowerShell.  Μπορείτε να ανατρέξετε στην το [αναφορά αρχείου καταγραφής ανάλυσης PowerShell cmdlet] (https://msdn.microsoft.com/library/mt188224(v=azure.300\).aspx) για άλλες λειτουργίες.

> [AZURE.NOTE] Ανάλυση καταγραφής ονομαζόταν προηγουμένως λειτουργικές ιδέες, το οποίο είναι ο λόγος που είναι το όνομα που χρησιμοποιείται στο τα cmdlet.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να χρησιμοποιήσετε PowerShell με το χώρο εργασίας του αρχείου καταγραφής ανάλυσης, πρέπει να έχετε:

+ Μια συνδρομή του Azure και 
+ Ανάλυση καταγραφής Azure χώρο εργασίας σας συνδέεται με τη συνδρομή σας στο Azure.

Εάν έχετε δημιουργήσει ένα χώρο εργασίας OMS, αλλά δεν είναι ακόμη συνδεδεμένο το σε μια συνδρομή του Azure μπορείτε να δημιουργήσετε τη σύνδεση:

+ Στην πύλη του Azure
+ Στην πύλη του OMS ή 
+ Χρησιμοποιώντας το cmdlet Get-AzureRmOperationalInsightsLinkTargets και δημιουργία AzureRmOperationalInsightsWorkspace.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Δημιουργία και ρύθμιση παραμέτρων ενός χώρου εργασίας ανάλυσης αρχείου καταγραφής

Το παρακάτω δείγμα δέσμης ενεργειών απεικονίζει τον τρόπο:

1.  Δημιουργία χώρου εργασίας
2.  Λίστα με τις διαθέσιμες λύσεις
3.  Προσθήκη λύσεις στο χώρο εργασίας
4.  Εισαγωγή αποθηκευτεί αναζητήσεις
5.  Εξαγωγή αποθηκευτεί αναζητήσεις
6.  Δημιουργία ομάδας υπολογιστή
7.  Ενεργοποίηση της συλλογής των αρχείων καταγραφής των υπηρεσιών IIS από τους υπολογιστές με εγκαταστήσει τον παράγοντα Windows
8.  Συλλογή μετρητές επιδόσεων λογικού δίσκου από υπολογιστές Linux (% Inodes χρησιμοποιούνται; Ελεύθερα MB; % Χρησιμοποιούνται χώρου. Μεταφορές δίσκου/δευτερόλεπτο; Διαβάζει δίσκου/sec; Εγγραφές δίσκου/δευτερόλεπτο)
9.  Συλλογή syslog συμβάντα από υπολογιστές Linux
10. Συλλογή σφάλματος και προειδοποίησης συμβάντων από το αρχείο καταγραφής συμβάντων εφαρμογής από υπολογιστές με Windows
11. Συλλογή διαθέσιμα MB μνήμης μετρητή επιδόσεων από υπολογιστές με Windows
12. Συλλογή ενός προσαρμοσμένου αρχείου καταγραφής 


```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

#List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Ρύθμιση παραμέτρων καταγραφής ανάλυση για να δημιουργήσετε ευρετήριο Διαγνωστικά του Azure 

Για agentless παρακολούθηση Azure πόρων, τους πόρους που πρέπει να έχετε Azure Διαγνωστικά ενεργοποιημένη και ρύθμιση παραμέτρων για να γράψετε σε ένα λογαριασμό του χώρου αποθήκευσης. Ανάλυση καταγραφής μπορεί να ρυθμιστεί, στη συνέχεια, να συγκεντρώσετε τα αρχεία καταγραφής από το λογαριασμό χώρου αποθήκευσης. Πόροι που πρέπει να κάνετε την προηγούμενη ρύθμιση παραμέτρων περιλαμβάνει τα εξής:

+ Υπηρεσίες cloud κλασική (ρόλοι web και εργασίας)
+ Υπηρεσία ύφασμα συμπλεγμάτων
+ Ομάδες ασφαλείας δικτύου
+ Βασικές χώροι φύλαξης και 
+ Εφαρμογή πυλών

Μπορείτε επίσης να χρησιμοποιήσετε το PowerShell για τη ρύθμιση παραμέτρων ενός χώρου εργασίας καταγραφής αναλυτικών στοιχείων σε μία Azure εγγραφή για τη συλλογή αρχείων καταγραφής από διάφορες συνδρομές του Azure.

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο:

1.  Λίστα τα υπαρχόντων λογαριασμών χώρου αποθήκευσης και τις θέσεις που ανάλυση καταγραφής θα δημιουργήσετε ευρετήριο δεδομένων από
2.  Δημιουργία μιας ρύθμισης παραμέτρων για να διαβάσετε από ένα λογαριασμό του χώρου αποθήκευσης
3.  Ενημερώστε τη ρύθμιση παραμέτρων που έχουν δημιουργηθεί πρόσφατα σε δεδομένα του ευρετηρίου από επιπλέον θέσεις
4.  Διαγράψτε τη ρύθμιση παραμέτρων που έχουν δημιουργηθεί πρόσφατα

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles", "insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

## <a name="next-steps"></a>Επόμενα βήματα

- Για πρόσθετες πληροφορίες σχετικά με τη χρήση του PowerShell για τη ρύθμιση των παραμέτρων του αρχείου καταγραφής ανάλυσης [cmdlet του PowerShell ανάλυση καταγραφής αναθεώρηση](http://msdn.microsoft.com/library/mt188224.aspx) .


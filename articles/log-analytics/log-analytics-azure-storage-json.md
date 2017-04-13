<properties
    pageTitle="Ανάλυση Azure αρχεία καταγραφής διαγνωστικών καταγραφής αναλυτικών στοιχείων χρήσης | Microsoft Azure"
    description="Ανάλυση καταγραφής μπορεί να διαβάσει τα αρχεία καταγραφής από Azure υπηρεσίες που γράψετε Azure αρχεία καταγραφής διαγνωστικών για να αντικειμένων blob χώρου αποθήκευσης σε μορφή JSON."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Ανάλυση Azure αρχεία καταγραφής διαγνωστικών καταγραφής αναλυτικών στοιχείων χρήσης

Αρχείο καταγραφής ανάλυσης να συγκεντρώσετε τα αρχεία καταγραφής για τις ακόλουθες υπηρεσίες Azure που εγγραφή [Azure αρχεία καταγραφής διαγνωστικών](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) στο χώρο αποθήκευσης αντικειμένων blob σε μορφή JSON:

+ Αυτοματοποίηση (έκδοση Preview)
+ Πλήκτρο θάλαμο (έκδοση Preview)
+ Πύλη εφαρμογής (έκδοση Preview)
+ Ομάδα ασφαλείας δικτύου (έκδοση Preview)

Οι παρακάτω ενότητες θα σας καθοδηγήσουν για χρήση του PowerShell για να:

+ Ρύθμιση παραμέτρων καταγραφής ανάλυσης να συγκεντρώσετε τα αρχεία καταγραφής από χώρο αποθήκευσης για κάθε πόρο  
+ Ενεργοποίηση της λύσης καταγραφής ανάλυσης για την υπηρεσία Azure

Πριν από την ανάλυση καταγραφής μπορεί να συλλέξει δεδομένα για αυτούς τους πόρους, πρέπει να ενεργοποιηθεί Azure Διαγνωστικά. Χρησιμοποιήστε το `Set-AzureRmDiagnosticSetting` cmdlet για να ενεργοποιήσετε την καταγραφή.

Ανατρέξτε στα ακόλουθα άρθρα για περισσότερες πληροφορίες σχετικά με τον τρόπο για να ενεργοποιήσετε την καταγραφή διαγνωστικών:

+ [Πλήκτρο θάλαμο](../key-vault/key-vault-logging.md)
+ [Πύλη εφαρμογής](../application-gateway/application-gateway-diagnostics.md)
+ [Ομάδα ασφαλείας δικτύου](../virtual-network/virtual-network-nsg-manage-log.md)

Αυτή η τεκμηρίωση περιλαμβάνει επίσης λεπτομέρειες σε:

+ Αντιμετώπιση προβλημάτων συλλογής δεδομένων
+ Διακοπή της συλλογής δεδομένων

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Ρύθμιση παραμέτρων καταγραφής αναλυτικών στοιχείων για τη συλλογή Azure αρχεία καταγραφής διαγνωστικών

Η συλλογή αρχείων καταγραφής για αυτές τις υπηρεσίες και ενεργοποίηση της λύσης για την απεικόνιση των αρχείων καταγραφής πραγματοποιείται χρησιμοποιώντας δεσμών ενεργειών του PowerShell.

Το παρακάτω παράδειγμα, σας δίνει τη δυνατότητα καταγραφής σε όλους τους πόρους που υποστηρίζονται

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Για ορισμένους πόρους, είναι δυνατό να εκτελέσετε τις παραπάνω παραμέτρους από την πύλη Azure χρησιμοποιώντας τα βήματα που περιγράφονται στα [αρχεία καταγραφής διαγνωστικών Επισκόπηση του Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs).

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Ρύθμιση παραμέτρων καταγραφής αναλυτικών στοιχείων για τη συλλογή Azure αρχεία καταγραφής διαγνωστικών

Παρέχουμε μια λειτουργική μονάδα δέσμης ενεργειών του PowerShell που εξάγει δύο cmdlet για να σας βοηθήσουν με τη ρύθμιση των παραμέτρων καταγραφής αναλυτικών στοιχείων:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`ζητά δεδομένα εισόδου σας και να ορίσετε τις ρυθμίσεις παραμέτρων απλό
2. `Add-AzureDiagnosticsToLogAnalytics`σας μεταφέρει τους πόρους για την παρακολούθηση της ως είσοδο και, στη συνέχεια, ρυθμίζει τις παραμέτρους καταγραφής ανάλυσης  

### <a name="pre-requisites"></a>Προαπαιτούμενα

1. Azure PowerShell με την έκδοση 1.0.8 ή νεότερη έκδοση της τα cmdlet λειτουργικές ιδέες.
  - [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md)
  - Επιβεβαιώστε την έκδοση των cmdlet του:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Καταγραφή διαγνωστικών ρυθμίζεται για τον πόρο Azure που θέλετε να παρακολουθήσετε. Χρήση `Set-AzureRmDiagnosticSetting` ή να αναφέρονται [Αναλυτικών στοιχείων χρήσης καταγραφής για τη συλλογή δεδομένων από λογαριασμούς Azure χώρου αποθήκευσης](log-analytics-azure-storage.md) για το πώς μπορείτε να ενεργοποιήσετε τα Διαγνωστικά.
3. Ένα χώρο εργασίας [Ανάλυσης αρχείου καταγραφής](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS)  
4. Της λειτουργικής μονάδας AzureDiagnosticsAndLogAnalytics PowerShell
  - Λήψη της λειτουργικής μονάδας [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) από συλλογή PowerShell

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Επιλογή 1: Εκτελέσετε τις δέσμες ενεργειών αλληλεπιδραστικών ρύθμισης παραμέτρων

Ανοίξτε το PowerShell και να εκτελέσετε:

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

Είστε εμφανίζεται μια λίστα με τις διαθέσιμες επιλογές και δίνεται μια προτροπή για να κάνετε την επιλογή σας.
Σας ζητείται να ορίσετε τις επιλογές για κάθε ένα από τα εξής:

+ Τύποι πόρων (Azure υπηρεσίες) για τη συλλογή αρχείων καταγραφής από
+ Παρουσίες πόρων για τη συλλογή αρχείων καταγραφής από
+ Αρχείο καταγραφής αναλυτικών στοιχείων χώρου εργασίας για τη συλλογή δεδομένων

Αφού εκτελέσετε αυτήν τη δέσμη ενεργειών, θα πρέπει να βλέπετε τις εγγραφές στο αρχείο καταγραφής ανάλυσης 30 λεπτά μετά νέων διαγνωστικών δεδομένων είναι γραμμένο με το χώρο αποθήκευσης. Εάν οι εγγραφές δεν είναι διαθέσιμες αφού αυτήν τη στιγμή που αναφέρονται στην παρακάτω ενότητα αντιμετώπισης προβλημάτων.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Επιλογή 2: Δημιουργήστε μια λίστα με τους πόρους και αυτά μεταβιβάζουν το cmdlet ρύθμισης παραμέτρων

Μπορείτε να δημιουργήσετε μια λίστα με τους πόρους που έχουν Azure Διαγνωστικά με δυνατότητα και, στη συνέχεια, μεταβιβάζουν τους πόρους για το cmdlet ρύθμισης παραμέτρων.

Μπορείτε να δείτε πρόσθετες πληροφορίες σχετικά με το cmdlet, εκτελώντας `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


Για να βρείτε περισσότερες λεπτομέρειες σε OMS [Cmdlet του PowerShell ανάλυσης αρχείου καταγραφής](https://msdn.microsoft.com/library/mt188224.aspx)

>[AZURE.NOTE] Εάν πόρων και χώρου εργασίας είναι σε διάφορες συνδρομές του Azure, εναλλαγή μεταξύ τους όπως απαιτείται χρήση`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Αφού εκτελέσετε αυτήν τη δέσμη ενεργειών, θα πρέπει να βλέπετε τις εγγραφές στο αρχείο καταγραφής ανάλυσης 30 λεπτά μετά νέων διαγνωστικών δεδομένων είναι γραμμένο με το χώρο αποθήκευσης. Εάν οι εγγραφές δεν είναι διαθέσιμες αφού αυτήν τη στιγμή που αναφέρονται στην παρακάτω ενότητα αντιμετώπισης προβλημάτων.  

>[AZURE.NOTE] Η ρύθμιση παραμέτρων δεν είναι ορατή στην πύλη του Azure. Μπορείτε να επαληθεύσετε ρύθμισης παραμέτρων χρησιμοποιώντας το `Get-AzureRmOperationalInsightsStorageInsight` cmdlet.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Διακοπή καταγραφής αναλυτικών στοιχείων από τη συλλογή Azure αρχεία καταγραφής διαγνωστικών

Για να διαγράψετε τη ρύθμιση παραμέτρων καταγραφής ανάλυσης για έναν πόρο, χρησιμοποιήστε το `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Αντιμετώπιση προβλημάτων ρύθμισης παραμέτρων για το Azure αρχεία καταγραφής διαγνωστικών

*Το ζήτημα*

Εμφανίζεται το ακόλουθο σφάλμα κατά τη ρύθμιση παραμέτρων ενός πόρου που συνδεδεμένοι με το χώρο αποθήκευσης κλασική:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Ανάλυση*

Συνδεθείτε με το API για το μοντέλο κλασική ανάπτυξης με`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Αντιμετώπιση προβλημάτων συλλογής δεδομένων για Azure αρχεία καταγραφής διαγνωστικών

Εάν δεν βλέπετε δεδομένα για τον πόρο Azure στο αρχείο καταγραφής ανάλυσης, μπορείτε να χρησιμοποιήσετε τα παρακάτω βήματα αντιμετώπισης προβλημάτων:

+ Επαλήθευση ρέει προς το λογαριασμό χώρου αποθήκευσης δεδομένων
+ Επιβεβαιώστε τη λύση καταγραφής ανάλυσης για την υπηρεσία είναι ενεργοποιημένη
+ Βεβαιωθείτε ότι ανάλυση καταγραφής έχει ρυθμιστεί για να διαβάσετε από χώρο αποθήκευσης
+ Ενημερώστε το κλειδί λογαριασμού χώρου αποθήκευσης

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Επαλήθευση ροή δεδομένων με το λογαριασμό χώρου αποθήκευσης

Μπορείτε να ελέγξετε εάν ροή δεδομένων με το λογαριασμό χώρου αποθήκευσης με ένα εργαλείο που σας επιτρέπει την περιήγηση Azure χώρου αποθήκευσης (για παράδειγμα Visual Studio) ή με χρήση του PowerShell.

Για να βρείτε το λογαριασμό χώρου αποθήκευσης στον πόρο έχει ρυθμιστεί για να συνδεθείτε για να χρησιμοποιήσετε το ακόλουθο PowerShell:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Το κοντέινερ χώρου αποθήκευσης που χρησιμοποιείται από το Azure Διαγνωστικά έχει ένα όνομα με μια μορφή της:  

`insights-logs-<Category>`

Ανατρέξτε στη [χρήση των cmdlet του χώρου αποθήκευσης για τον έλεγχο ενός κοντέινερ για πρόσφατα δεδομένα](../storage/storage-powershell-guide-full.md) για να μάθετε περισσότερα σχετικά με την προβολή των περιεχομένων του λογαριασμού χώρου αποθήκευσης.

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Επιβεβαιώστε τη λύση καταγραφής ανάλυσης για την υπηρεσία είναι ενεργοποιημένη

Εάν χρησιμοποιείτε το `Add-AzureDiagnosticsToLogAnalyticsUI`, η σωστή λύση καταγραφής ανάλυσης ενεργοποιείται αυτόματα για εσάς.

Για να ελέγξετε εάν έχει ενεργοποιηθεί μια λύση, εκτελέστε το ακόλουθο PowerShell:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Εάν δεν είναι ενεργοποιημένη η λύση, μπορείτε να την ενεργοποιήσετε χρησιμοποιώντας την παρακάτω PowerShell:

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Για να βρείτε το όνομα της λύσης για να ενεργοποιήσετε για κάθε τύπο πόρου, χρησιμοποιήστε το παρακάτω PowerShell (αυτή η μεταβλητή είναι διαθέσιμη, αφού έχετε εισαγάγει τη λειτουργική μονάδα):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Βεβαιωθείτε ότι ανάλυση καταγραφής έχει ρυθμιστεί για να διαβάσετε από χώρο αποθήκευσης

Εάν προσθέσετε επιπλέον πόρους Azure, πρέπει να ενεργοποιήσετε την καταγραφή για τα Διαγνωστικά και ρύθμιση παραμέτρων καταγραφής ανάλυσης για τους.
Για να ελέγξετε ποια λογαριασμούς χώρου αποθήκευσης και πόρων ανάλυση καταγραφής έχει ρυθμιστεί για τη συλλογή αρχείων καταγραφής για, χρησιμοποιήστε το PowerShell παρακάτω:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Η ενημέρωση του κλειδιού χώρου αποθήκευσης

Εάν αλλάξετε το κλειδί για το λογαριασμό χώρου αποθήκευσης, τη ρύθμιση παραμέτρων καταγραφής ανάλυσης πρέπει επίσης να ενημερώνονται με το νέο κλειδί.
Μπορείτε να ενημερώσετε τις ρυθμίσεις παραμέτρων καταγραφής αναλυτικών στοιχείων με έναν νέο αριθμό-κλειδί με χρήση του PowerShell παρακάτω:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Για να βρείτε το όνομα πληροφορίες για το χώρο αποθήκευσης, χρησιμοποιήστε το `Get-AzureRmOperationalInsightsStorageInsight` cmdlet, όπως φαίνεται στην προηγούμενη παραδείγματα.

## <a name="next-steps"></a>Επόμενα βήματα

- [Χώρος αποθήκευσης αντικειμένων blob χρήση των υπηρεσιών IIS και χώρος αποθήκευσης πινάκων για συμβάντα](log-analytics-azure-storage-iis-table.md) για να διαβάσετε τα αρχεία καταγραφής για Azure υπηρεσιών που Διαγνωστικά εγγραφής στο χώρο αποθήκευσης πινάκων ή αρχεία καταγραφής των υπηρεσιών IIS εγγραφεί στο χώρο αποθήκευσης blob.
- [Ενεργοποίηση λύσεων](log-analytics-add-solutions.md) για να παρέχετε πληροφορίες για τα δεδομένα.
- [Χρήση ερωτημάτων αναζήτησης](log-analytics-log-searches.md) για την ανάλυση των δεδομένων.

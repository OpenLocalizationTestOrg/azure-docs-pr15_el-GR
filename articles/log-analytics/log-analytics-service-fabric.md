<properties
    pageTitle="Βελτιστοποιήστε το περιβάλλον σας με τη λύση ύφασμα υπηρεσίας στο αρχείο καταγραφής ανάλυσης | Microsoft Azure"
    description="Μπορείτε να χρησιμοποιήσετε τη λύση ύφασμα υπηρεσία για την αξιολόγηση του κινδύνου και την εύρυθμη λειτουργία των ύφασμα υπηρεσίας εφαρμογές, υπηρεσίες μικρής κλίμακας, κόμβους και συμπλεγμάτων."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>




# <a name="service-fabric-solution-in-log-analytics"></a>Λύση ύφασμα υπηρεσίας στο αρχείο καταγραφής ανάλυσης

> [AZURE.SELECTOR]
- [Διαχείριση πόρων](log-analytics-service-fabric-azure-resource-manager.md)
- [PowerShell](log-analytics-service-fabric.md)

Σε αυτό το άρθρο περιγράφει τον τρόπο χρήσης της λύσης ύφασμα υπηρεσίας στο αρχείο καταγραφής ανάλυσης για να προσδιορίσετε και αντιμετώπιση προβλημάτων σε ύφασμα υπηρεσιών συμπλέγματος, αυξάνοντας ορατότητα σε πώς πραγματοποιούν σας κόμβους ύφασμα υπηρεσίας και πώς χρησιμοποιείτε τις εφαρμογές και υπηρεσίες μικρής κλίμακας.

Η λύση ύφασμα υπηρεσίας χρησιμοποιεί Azure διαγνωστικά δεδομένα από σας ΣΠΣ ύφασμα υπηρεσίας, από τη συλλογή αυτών των δεδομένων από τους πίνακές σας Azure WAD. Ανάλυση καταγραφής διαβάζει, στη συνέχεια, υπηρεσία ύφασμα framework συμβάντα, συμπεριλαμβανομένων των **Αξιόπιστη συμβάντα της υπηρεσίας**, **Συμβάντα παράγοντα**, **Λειτουργικές συμβάντων**και **τα συμβάντα ETW προσαρμοσμένη**. Στον πίνακα εργαλείων λύση ύφασμα υπηρεσίας εμφανίζει αξιοσημείωτες θέματα και σχετικές συμβάντα στο περιβάλλον σας ύφασμα υπηρεσίας.

## <a name="installing-and-configuring-the-solution"></a>Εγκατάσταση και ρύθμιση παραμέτρων της λύσης

Ακολουθήστε αυτά τα τρία εύκολα βήματα για να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους της λύσης:

1. Βεβαιωθείτε ότι το χώρο εργασίας OMS που χρησιμοποιείτε είναι συσχετισμένες με την ίδια συνδρομή Azure που χρησιμοποιήσατε για να δημιουργήσετε όλους τους πόρους συμπλέγματος, συμπεριλαμβανομένων των λογαριασμών χώρου αποθήκευσης. Για πληροφορίες σχετικά με τη δημιουργία ενός χώρου εργασίας OMS, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το αρχείο καταγραφής ανάλυσης](log-analytics-get-started.md) .
2. Ρύθμιση παραμέτρων OMS για να συλλέξετε και να προβάλετε τα αρχεία καταγραφής ύφασμα υπηρεσίας.
3. Ενεργοποίηση της λύσης υφάσματος υπηρεσίας στο χώρο εργασίας σας.

## <a name="configure-oms-to-collect-and-view-service-fabric-logs"></a>Ρύθμιση παραμέτρων OMS για να συλλέξετε και να προβάλετε τα αρχεία καταγραφής ύφασμα υπηρεσίας
Σε αυτήν την ενότητα, θα μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους OMS για να ανακτήσετε αρχεία καταγραφής από την υπηρεσία υφάσματος. Τα αρχεία καταγραφής επιτρέπουν να προβάλετε, την ανάλυση και αντιμετώπιση προβλημάτων σε το σύμπλεγμά σας ή σε τις εφαρμογές και υπηρεσίες που εκτελούνται σε αυτό το σύμπλεγμα, με την πύλη OMS.

>[AZURE.NOTE] Η επέκταση Διαγνωστικά Azure πρέπει να ρυθμιστεί για να αποστείλετε τα αρχεία καταγραφής στο χώρο αποθήκευσης πινάκων που ταιριάζουν με τι θα αναζητήσει OMS. Δείτε [πώς μπορείτε να συλλέξετε αρχεία καταγραφής με Διαγνωστικά του Azure](../service-fabric/service-fabric-diagnostics-how-to-setup-wad.md) για περισσότερες πληροφορίες σχετικά με τον τρόπο για τη συλλογή αρχείων καταγραφής. Τα παραδείγματα ρυθμίσεις παραμέτρων σε αυτό το άρθρο εμφανίζουν ποιες τα ονόματα των πινάκων χώρου αποθήκευσης θα πρέπει. Όταν Διαγνωστικά έχει ρυθμιστεί στο σύμπλεγμα και η αποστολή αρχείων καταγραφής σε ένα λογαριασμό του χώρου αποθήκευσης, το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους OMS για τη συλλογή αυτά τα αρχεία καταγραφής.

Βεβαιωθείτε ότι ενημερώνετε την ενότητα **EtwEventSourceProviderConfiguration** στο αρχείο **template.json** για να προσθέσετε καταχωρήσεις για το νέο EventSources πριν να εφαρμόσετε την ενημέρωση της ρύθμισης παραμέτρων, εκτελώντας **deploy.ps1**. Ο πίνακας για αποστολή είναι ίδια με (ETWEventTable). Προς το παρόν, OMS μπορείτε μόνο να διαβάσετε συμβάντα ETW εφαρμογής από αυτόν τον πίνακα. Ωστόσο, η υποστήριξη για προσαρμοσμένες ETW πίνακες είναι σε ανάπτυξη.

Τα ακόλουθα εργαλεία χρησιμοποιούνται για να εκτελέσετε ορισμένες από τις εργασίες σε αυτήν την ενότητα:

-   Azure PowerShell
-   [Λειτουργίες διαχείρισης οικογένεια](http://www.microsoft.com/oms)

### <a name="configure-an-oms-workspace-to-show-the-cluster-logs"></a>Ρύθμιση παραμέτρων ενός χώρου εργασίας OMS για να εμφανίσετε τα αρχεία καταγραφής συμπλέγματος

Αφού έχετε δημιουργήσει ένα χώρο εργασίας OMS όπως περιγράφεται παραπάνω, το επόμενο βήμα είναι να ρυθμίσετε τις παραμέτρους στο χώρο εργασίας για να αποσπάσετε τα αρχεία καταγραφής από τους πίνακες Azure αποθήκευσης όπου αυτές που αποστέλλονται από το σύμπλεγμα από την επέκταση Διαγνωστικά. Για να το κάνετε αυτό, εκτελέστε την ακόλουθη δέσμη ενεργειών PowerShell:

```
<#
    This script will configure an Operations Management Suite workspace (previously called an Operational Insights workspace) to read Diagnostics from an Azure Storage account.
    It will enable all supported data types (currently Service Fabric Events, ETW Events and IIS Logs).
    It supports Resource Manager storage accounts.
    If you have more than one Azure Subscription, you will be prompted for the subscription to configure.
    If you have more than one OMS workspace you will be prompted for the workspace to configure.
    It will then look through your Service Fabric clusters, and configure your OMS workspace to read Diagnostics from storage accounts that are connected to that cluster and have diagnostics enabled.
#>

try
{
    Get-AzureRMContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Add-AzureRmAccount
}

$validTables = "WADServiceFabric*EventTable", "WADETWEventTable"
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"

            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.SubscriptionName + " (" + $subscription.SubscriptionId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.SubscriptionId
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  

    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found. `n"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
             Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}

function Check-ETWProviderLogging {
     param(
     [string]$id,
     [string]$provider,
     [string]$expectedTable,
     [string]$table
    )       
         Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
         if ( ($table -eq $null) -or ($table -eq ""))  
         {
             Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
         }  
         elseif ( $table -ne $expectedTable )
         {
             Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by OMS")
         }  
         else
         {
             Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
         }
 }

function Check-ServiceFabricScaleSetDiagnostics {
     param(
          [psobject]$scaleSetDiagnostics
   )
     $storageAccountsFound = @()
     Write-Verbose ("Checking " + $scaleSetDiagnostics)
     $sfReliableActorTable = $null
     $sfReliableServiceTable = $null
     $sfOperationalTable = $null

     Write-Debug $scaleSetDiagnostics
     $serviceFabricProviderList = ""
     $etwManifestProviderList = ""

     if ( $scaleSetDiagnostics.xmlCfg )  
      {
             Write-Debug ("Found XMLcfg")
             $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
             Write-Debug $xmlCfg
             $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                 
             $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
             $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
      } elseif ($scaleSetDiagnostics.WadCfg )  
     {
         Write-Debug ("Found WADcfg")
         Write-Debug $scaleSetDiagnostics.WadCfg
         $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
         $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration
     } else
     {
         Write-Error "Unable to parse Azure Diagnostics setting for $id"
             Write-Warning ("$id does not have diagnostics enabled")
     }
     foreach ($provider in $serviceFabricProviderList)  
     {
         Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
         {
             $sfReliableActorTable = $provider.DefaultEvents.eventDestination  
         } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")  
         {  
             $sfReliableServiceTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }
     foreach ($provider in $etwManifestProviderList)
     {
         Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
         if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
         {
             $sfOperationalTable = $provider.DefaultEvents.eventDestination  
         } else  
         {
             Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
         }
     }

     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
     Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
     Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

     Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)
     $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
     return ($storageAccountsFound)
 }

function Select-StorageAccount {
    $allResources = Get-AzureRmResource #pulls in all resources
    $serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"}) #pulls in all service fabric clusters in the resource
    $storageAccountList = @()
    foreach($cluster in $serviceFabricClusters) {
        Write-Host("Checking cluster: " + $cluster.Name)
         $scaleSet = $allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)})

         foreach($set in $scaleSet) {
             $resource = Get-AzureRmResource -ResourceId $set.ResourceId
             $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions

             foreach($ext in $extensions) {
                 if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                     $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
                 }
             }
          }

         $storageAccountsToCheck = $allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)})

         if ($storageAccountsToCheck.Count -eq "0") {
                Write-Error "No storage accounts found"
           }
           else {
                    foreach ($storageAccount in $storageAccountsToCheck) {
                        Write-Host("Checking Storage Account: " + $storageAccount.Name)
                        $insightsName = $storageAccount.Name + $workspace.Name
                        $existingConfig = ""
                        try
                            {
                                $existingConfig = Get-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -ErrorAction Stop
                            }
                        catch [Hyak.Common.CloudException]
                            {
                                # HTTP Not Found is returned if the storage insight doesn't exist
                            }
                        if ($existingConfig) {                         
                                  [array]$Tables = $existingConfig.Tables
                                   foreach($table in $validTables) {
                                         if($Tables -notcontains $table) {
                                               $Tables += $table
                                               $dirty = $true;
                                               Write-Host "Adding Table: $table";
                                         }
                                         else {
                                               Write-Host "$table is already configured.`n";
                                             }
                                      }
                                      # If any of the tables from the table list are not already monitored, then we add them
                                   if($dirty -eq $true) {
                                           Set-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -Tables $Tables
                                           Write-Host "Updating Storage Insight. `n"
                                    }
                                    else {
                                           Write-Host "Storage Insight already updated."
                                  }
                          }
                     else {
                            $key = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.Name)[0].Value
                           New-AzureRmOperationalInsightsStorageInsight -Workspace $workspace -Name $insightsName -StorageAccountResourceId $storageAccount.ResourceId -StorageAccountKey $key -Tables $validTables
                            Write-Host "New Azure Storage Insight Configured. `n"
                           }
                    }
             }
      }
      return
     }

$subscription = Select-Subscription
$subscriptionId = $subscription.SubscriptionId
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
$storageAccount = Select-StorageAccount
```

Αφού έχετε ρυθμίσει τις παραμέτρους του χώρου εργασίας OMS ανάγνωση από το Azure πίνακες στο λογαριασμό σας χώρου αποθήκευσης, συνδεθείτε στην πύλη του Azure, και επιλέξτε το χώρο εργασίας OMS από **Όλους τους πόρους**. Μόλις επιλεγεί, θα πρέπει να μπορείτε να δείτε τον αριθμό του χώρου αποθήκευσης αρχείων καταγραφής συνδεδεμένοι σε αυτόν το χώρο εργασίας OMS λογαριασμού. Επιλέξτε το πλακίδιο **αρχεία καταγραφής λογαριασμού χώρου αποθήκευσης** και να επαληθεύσετε από τη λίστα των αρχείων καταγραφής αποθήκευσης λογαριασμού ότι το λογαριασμό χώρου αποθήκευσης είναι συνδεδεμένος με αυτόν το χώρο εργασίας OMS:

![Αρχεία καταγραφής λογαριασμού χώρου αποθήκευσης](./media/log-analytics-service-fabric/sf1.png)

## <a name="enable-the-service-fabric-solution"></a>Ενεργοποίηση της λύσης ύφασμα υπηρεσίας
Χρησιμοποιήστε την ακόλουθη δέσμη ενεργειών για να προσθέσετε τη λύση του χώρου εργασίας σας OMS. Εκτελεστεί η δέσμη ενεργειών PowerShell, χρησιμοποιώντας τη συνδρομή Azure που είναι συσχετισμένη με το χώρο εργασίας OMS που θέλετε να ενεργοποιήσετε τη λύση ύφασμα υπηρεσίας στο.

```
function Select-Subscription {
      $subscription = ""
      $allSubscriptions = Get-AzureRmSubscription
      switch ($allSubscriptions.Count) {
             0 {Write-Error "No Operations Management Suite workspaces found"}
             1 {return $allSubscriptions}
        default {
            $uiPrompt = "Enter the number corresponding to the Azure subscription you would like to work with.`n"
            $count = 1
            foreach ($subscription in $allSubscriptions) {
                $uiPrompt += "$count. " + $subscription.SubscriptionName + " (" + $subscription.SubscriptionId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $subscription = $allSubscriptions[$answer]
             Write-Host $subscription.SubscriptionId
        }  
    }
    return $subscription
}

function Select-Workspace {
    $workspace = ""
    $allWorkspaces = Get-AzureRmOperationalInsightsWorkspace  
    switch ($allWorkspaces.Count) {
        0 {Write-Error "No Operations Management Suite workspaces found"}
        1 {return $allWorkspaces}
        default {
            $uiPrompt = "Enter the number corresponding to the workspace you want to configure.`n"
            $count = 1
            foreach ($workspace in $allWorkspaces) {
                $uiPrompt += "$count. " + $workspace.Name + " (" + $workspace.CustomerId + ")`n"
                $count++
            }
            $answer = (Read-Host -Prompt $uiPrompt) - 1
            $workspace = $allWorkspaces[$answer]
                           Write-Host $workspace.WorkspaceName
        }  
    }
    return $workspace
}
$subscription = Select-Subscription
$subscriptionId = $subscription.SubscriptionId
$subscription = Select-AzureRmSubscription -SubscriptionId $subscriptionId
$workspace = Select-Workspace
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -IntelligencePackName "ServiceFabric" -Enabled $true
```

Μετά την ενεργοποίηση της λύσης, το πλακίδιο ύφασμα υπηρεσίας προστίθεται στη σελίδα Επισκόπηση OMS σας, με μια προβολή από αξιοσημείωτες θέματα όπως αποτυχίες runAsync και των ακυρώσεων που έχουν προκύψει τις τελευταίες 24 ώρες.

![Πλακίδιο ύφασμα υπηρεσίας](./media/log-analytics-service-fabric/sf2.png)

### <a name="view-service-fabric-events"></a>Προβολή ύφασμα υπηρεσία συμβάντων

Κάντε κλικ στο πλακίδιο **Ύφασμα υπηρεσίας** για να ανοίξετε τον πίνακα εργαλείων ύφασμα υπηρεσίας. Πίνακας εργαλείων περιλαμβάνει τις στήλες στον παρακάτω πίνακα. Κάθε στήλη παραθέτει τα συμβάντα επάνω δέκα βάσει πλήθους με κριτήρια αυτής της στήλης για το καθορισμένο χρονικό διάστημα. Μπορείτε να εκτελέσετε μια αναζήτηση καταγραφής που παρέχει ολόκληρη τη λίστα, κάνοντας κλικ στην επιλογή **δείτε όλες** στο κάτω δεξιό μέρος κάθε στήλη ή κάνοντας κλικ στην κεφαλίδα της στήλης.

| **Υπηρεσία ύφασμα συμβάντος** | **Περιγραφή** |
| --- | --- |
| Αξιοσημείωτες θέματα | Μια προβολή των θεμάτων όπως RunAsyncFailures RunAsynCancellations και λίστες κόμβο. |
| Λειτουργικές συμβάντα | Αξιοσημείωτες λειτουργικές συμβάντα όπως αναβάθμισης εφαρμογής και αναπτύξεις. |
| Αξιόπιστη υπηρεσία συμβάντα | Αξιοσημείωτες αξιόπιστη υπηρεσία συμβάντα όπως ένα Runasyncinvocations. |
| Συμβάντα παράγοντα | Αξιοσημείωτες παράγοντα συμβάντα που δημιουργούνται από τους-των υπηρεσιών σας, όπως εξαιρέσεις που ανακύπτουν από μια μέθοδο παράγοντα, παράγοντα ενεργοποίησης και deactivations και ούτω καθεξής. |
| Συμβάντα εφαρμογής | Όλα προσαρμοσμένα ETW τα συμβάντα που δημιουργούνται από τις εφαρμογές σας. |

![Υπηρεσία ύφασμα πίνακα εργαλείων](./media/log-analytics-service-fabric/sf3.png)

![Υπηρεσία υφάσματος πίνακα εργαλείων](./media/log-analytics-service-fabric/sf4.png)


Ο παρακάτω πίνακας εμφανίζει μεθόδους συλλογής δεδομένων και άλλες λεπτομέρειες σχετικά με το πώς συλλέγονται δεδομένα για ύφασμα υπηρεσίας.

| πλατφόρμα | Άμεση παράγοντα | Παράγοντας SCOM | Azure χώρου αποθήκευσης | SCOM απαιτείται; | SCOM παράγοντας δεδομένων που αποστέλλονται μέσω ομάδας διαχείρισης | συχνότητα συλλογής |
|---|---|---|---|---|---|---|
|Windows|![Όχι](./media/log-analytics-malware/oms-bullet-red.png)|![Όχι](./media/log-analytics-malware/oms-bullet-red.png)| ![Ναι](./media/log-analytics-malware/oms-bullet-green.png)|            ![Όχι](./media/log-analytics-malware/oms-bullet-red.png)|![Όχι](./media/log-analytics-malware/oms-bullet-red.png)|10 λεπτά |


>[AZURE.NOTE] Μπορείτε να αλλάξετε την εμβέλεια από αυτά τα συμβάντα στη λύση ύφασμα υπηρεσίας κάνοντας κλικ στην επιλογή **δεδομένων με βάση τελευταίες 7 ημέρες** στο επάνω μέρος του πίνακα εργαλείων. Μπορείτε επίσης να εμφανίσετε τα συμβάντα που δημιουργούνται από τις τελευταίες 7 ημέρες, 1 ημέρα ή 6 ώρες. Εναλλακτικά, μπορείτε να επιλέξετε **προσαρμοσμένο** για να καθορίσετε μια προσαρμοσμένη περιοχή ημερομηνιών.


## <a name="troubleshoot-your-service-fabric-and-oms-configuration"></a>Αντιμετώπιση προβλημάτων της ρύθμισης παραμέτρων υφάσματος υπηρεσίας και OMS

Εάν χρειάζεστε για να επαληθεύσετε τις παραμέτρους OMS επειδή δεν μπορείτε να προβάλετε τα δεδομένα συμβάντων στο OMS, χρησιμοποιήστε την παρακάτω δέσμη ενεργειών. Διαβάζει ύφασμα υπηρεσίας Διαγνωστικά τις ρυθμίσεις σας, ελέγχων για τα δεδομένα που εγγράφονται στους πίνακες, και επαληθεύει ότι OMS έχει ρυθμιστεί ώστε να διαβάσετε από τους πίνακες.

```
<#
    Verify Service Fabric and OMS configuration
    1. Read Service Fabric diagnostics configuration
    2. Check for data being written into the tables
    3. Verify OMS is configured to read from the tables

    Supported tables:
    WADServiceFabricReliableActorEventTable
    WADServiceFabricReliableServiceEventTable
    WADServiceFabricSystemEventTable
    WADETWEventTable

    Script will write a warning for every misconfiguration detected
    To see items that are correctly configured set $VerbosePreference="Continue"
#>
Param
(
    [Parameter(Mandatory=$true,
    ValueFromPipeline=$true,
    Position=1)]
    [string]$workspaceName
)

$WADtables = @("WADServiceFabricReliableActorEventTable",
               "WADServiceFabricReliableServiceEventTable",
               "WADServiceFabricSystemEventTable",
               "WADETWEventTable"
               )

<#
    Check if OMS Log Analytics is configured to index service fabric events from the specified table
#>

function Check-OMSLogAnalyticsConfiguration {
    param(
    [psobject]$workspace,
    [psobject]$storageAccount,
    [string]$id
    )

    $existingInsights = Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

    if ($existingInsights)
    {
        $currentStorageAccountInsight = $existingInsights.Where({$_.StorageAccountResourceId -eq $storageAccount.ResourceId})

        if ("WADServiceFabric*EventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric actor, service and operational events from " + $storageAccount.Name)
        }
        if ("WADETWEventTable" -in $currentStorageAccountInsight.Tables)
        {
            Write-Verbose ("OMS Log Analytics workspace " + $workspace.Name + " is configured to index service fabric application events from " + $storageAccount.Name)
        } else
        {
            Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + " is not configured to index service fabric application events from " + $storageAccount.Name)
        }
    } else
    {
        Write-Warning ("OMS Log Analytics workspace " + $workspace.Name + "is not configured to read service fabric events from " + $storageAccount.Name)
    }    
}

<#
    Check Azure table storage to confirm there is recent data written by Service Fabric
#>

function Check-TablesForData {
    param(
    [psobject]$storageAccount
    )

    $ctx = (Get-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.ResourceName).Context

    $createdTables = Get-AzureStorageTable -Context $ctx

    $recently = Get-Date -Format s ((Get-Date).AddMinutes(-20).ToUniversalTime())
    $recently = $recently + "Z"

    foreach ($table in $WADtables)
    {
        if ($table -in $createdTables.Name)
        {
            $tbl = Get-AzureStorageTable -Name $table -Context $ctx
            $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery
            $list = New-Object System.Collections.Generic.List[string]
            $list.Add("RowKey")
            $list.Add("ProviderName")
            $list.Add("Timestamp")
            $query.FilterString = "Timestamp gt datetime'$recently'"
            $query.SelectColumns = $list
            $query.TakeCount = 20
            $entities = $tbl.CloudTable.ExecuteQuery($query)
            Write-Debug $entities
            if ($entities.Count -gt 0)
            {
                Write-Verbose ("Data was written to $table in " + $storageAccount.ResourceName + "after $recently")
            } else
            {
                Write-Warning ("No data after $recently is in  $table in " + $storageAccount.ResourceName)
            }
        } else
        {
            Write-Warning ("$table does not exist in storage account " + $storageAccount.ResourceName)
        }
    }
}

<#
    Check if ETW provider is configured to log events to the expected table storage
#>
function Check-ETWProviderLogging {
    param(
    [string]$id,
    [string]$provider,
    [string]$expectedTable,
    [string]$table
    )      
        Write-Debug ("ID: $id Provider: $provider ExpectedTable $expectedTable ActualTable $table")
        if ( ($table -eq $null) -or ($table -eq ""))
        {
            Write-Warning ("$id No configuration found for $provider. Configure Azure diagnostics to write to $expectedTable.")
        }
        elseif ( $table -ne $expectedTable )
        {
            Write-Warning ("$id $provider events are being written to $table instead of WAD$expectedTable. Events will not be collected by OMS")
        }
        else
        {
            Write-Verbose "$id $provider events are being written to WAD$expectedTable (Correct configuration.)"
        }
}

<#
    Check Azure Diagnostics Configuration for a Service Fabric cluster
#>
function Check-ServiceFabricScaleSetDiagnostics {
    param(
    [psobject]$scaleSetDiagnostics
    )

    $storageAccountsFound = @()
    Write-Verbose ("Checking " + $scaleSetDiagnostics)
    $sfReliableActorTable = $null
    $sfReliableServiceTable = $null
    $sfOperationalTable = $null
    Write-Debug $scaleSetDiagnostics
    $serviceFabricProviderList = ""
    $etwManifestProviderList = ""

    if ( $scaleSetDiagnostics.xmlCfg )
    {
        Write-Debug ("Found XMLcfg")
        $xmlCfg = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($scaleSetDiagnostics.xmlCfg))
        Write-Debug $xmlCfg
        $etwProviders = Select-Xml -Content $xmlCfg -XPath "//EtwProviders"                
        $serviceFabricProviderList = $etwProviders.Node.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $etwProviders.Node.EtwManifestProviderConfiguration
    } elseif ($scaleSetDiagnostics.WadCfg )
    {
        Write-Debug ("Found WADcfg")
        Write-Debug $scaleSetDiagnostics.WadCfg
        $serviceFabricProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwEventSourceProviderConfiguration
        $etwManifestProviderList = $scaleSetDiagnostics.WadCfg.DiagnosticMonitorConfiguration.EtwProviders.EtwManifestProviderConfiguration 
    } else
    {
        Write-Error "Unable to parse Azure Diagnostics setting for $id"
        Write-Warning ("$id does not have diagnostics enabled")
    }

    foreach ($provider in $serviceFabricProviderList)
    {
        Write-Debug ("Event Source Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "Microsoft-ServiceFabric-Actors")
        {
            $sfReliableActorTable = $provider.DefaultEvents.eventDestination
        } elseif ($provider.Provider -eq "Microsoft-ServiceFabric-Services")
        {
            $sfReliableServiceTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }
    foreach ($provider in $etwManifestProviderList)
    {
        Write-Debug ("Manifest Provider: " + $provider.Provider + " Destination: " + $provider.DefaultEvents.eventDestination)
        if ($provider.Provider -eq "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8")
        {
            $sfOperationalTable = $provider.DefaultEvents.eventDestination
        } else
        {
            Check-ETWProviderLogging $id $provider.Provider "ETWEventTable" $provider.DefaultEvents.eventDestination
        }
    }

    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Actors" "ServiceFabricReliableActorEventTable" $sfReliableActorTable
    Check-ETWProviderLogging $id "Microsoft-ServiceFabric-Services" "ServiceFabricReliableServiceEventTable" $sfReliableServiceTable
    Check-ETWProviderLogging $id "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8 (System events)" "ServiceFabricSystemEventTable" $sfOperationalTable

    Write-Verbose ("StorageAccount: " + $scaleSetDiagnostics.StorageAccount)

    $storageAccountsFound += ($scaleSetDiagnostics.StorageAccount)
    return ($storageAccountsFound)
}

# This script uses Get-AzureRmVMDiagnosticsExtension and needs a version where -Name is not a required parameter
Import-Module AzureRM.Compute -MinimumVersion 1.2.2

try
{
    Get-AzureRmContext
}
catch [System.Management.Automation.PSInvalidOperationException]
{
    Login-AzureRmAccount
}

$allResources = Get-AzureRmResource

$OMSworkspace = $allResources.Where({($_.ResourceType -eq "Microsoft.OperationalInsights/workspaces") -and ($_.ResourceName -eq $workspaceName)})

if ($OMSworkspace.Name -ne $workspaceName)
{
    Write-Error ("Unable to find OMS Workspace " + $workspaceName)
}

$serviceFabricClusters = $allResources.Where({$_.ResourceType -eq "Microsoft.ServiceFabric/clusters"})
$storageAccountList = @()
foreach($cluster in $serviceFabricClusters) {
    Write-Verbose ("Checking cluster: " + $cluster.Name)
    $scaleSet = ($allResources.Where({($_.ResourceType -eq "Microsoft.Compute/virtualMachineScaleSets") -and ($_.ResourceGroupName -eq $cluster.ResourceGroupName)}))

    foreach($set in $scaleSet) {
        $resource = Get-AzureRmResource -ResourceId $set.ResourceId
        $extensions = $resource.Properties.VirtualMachineProfile.ExtensionProfile.Extensions
        foreach($ext in $extensions) {
            if ($ext.Properties.Publisher -eq "Microsoft.Azure.Diagnostics" -and $ext.Properties.Type -eq "IaaSDiagnostics") {
                $storageAccountList += (Check-ServiceFabricScaleSetDiagnostics $ext.Properties.Settings)
            }
        }
    }
}

$storageAccountList = $storageAccountList | Sort-Object | Get-Unique
$storageAccountsToCheck = ($allResources.Where({($_.ResourceType -eq "Microsoft.Storage/storageAccounts") -and ($_.ResourceName -in $storageAccountList)}))

foreach($storageAccount in $storageAccountsToCheck)
{
    Check-TablesForData $storageAccount
    Check-OMSLogAnalyticsConfiguration $OMSworkspace $storageAccount
}
 ```


## <a name="next-steps"></a>Επόμενα βήματα

- Χρησιμοποιήστε [Αναζητήσεις καταγραφής στο αρχείο καταγραφής ανάλυσης](log-analytics-log-searches.md) για να δείτε λεπτομερή δεδομένα συμβάντων ύφασμα υπηρεσίας.

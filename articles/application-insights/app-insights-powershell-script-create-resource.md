<properties 
    pageTitle="Δέσμη ενεργειών του PowerShell για να δημιουργήσετε μια εφαρμογή ιδέες πόρων" 
    description="Αυτοματοποίηση δημιουργίας ιδέες εφαρμογής πόρων." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>Δέσμη ενεργειών του PowerShell για να δημιουργήσετε μια εφαρμογή ιδέες πόρων

*Εφαρμογή ιδέες είναι σε προεπισκόπηση.*

Όταν θέλετε να παρακολουθείτε μια νέα εφαρμογή - ή μια νέα έκδοση μιας εφαρμογής - με [Ιδέες εφαρμογή του Visual Studio](https://azure.microsoft.com/services/application-insights/), ορίζετε ένα νέο πόρο στο Microsoft Azure. Αυτός ο πόρος είναι όπου τα δεδομένα τηλεμετρίας από την εφαρμογή σας είναι ανάλυση και εμφανίζεται. 

Μπορείτε να αυτοματοποιήσετε τη δημιουργία ενός νέου πόρου με χρήση του PowerShell.

Για παράδειγμα, εάν δημιουργείτε μια εφαρμογή για κινητή συσκευή, είναι πιθανό ότι, ανά πάσα στιγμή, θα υπάρχουν αρκετές δημοσιευμένες εκδόσεις της εφαρμογής χρησιμοποιείται από τους πελάτες σας. Δεν θέλετε να λάβετε τα αποτελέσματα τηλεμετρίας από διαφορετικές εκδόσεις μεικτή προς τα επάνω. Επομένως, μπορείτε να λάβετε τη Δόμηση διαδικασία για να δημιουργήσετε ένα νέο πόρο για κάθε Δόμηση.

## <a name="script-to-create-an-application-insights-resource"></a>Δέσμη ενεργειών για να δημιουργήσετε μια εφαρμογή ιδέες πόρων

Δείτε το θέμα η προδιαγραφές σχετικές cmdlet:

* [Νέα AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Νέα AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*Δέσμη ενεργειών του PowerShell*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Τι να κάνετε με το iKey

Κάθε πόρο προσδιορίζεται από το κλειδί οργάνων (iKey). Το iKey είναι το αποτέλεσμα της δέσμης ενεργειών δημιουργίας πόρων. Το δέσμη ενεργειών δόμησης πρέπει να παρέχετε το iKey στο SDK ιδέες εφαρμογή ενσωματωμένο στην εφαρμογή σας.

Υπάρχουν δύο τρόποι για να διαθέσετε το iKey στο SDK:
  
* Στο [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Ή σε [κώδικα προετοιμασίας](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Δείτε επίσης

* [Δημιουργία εφαρμογής ιδέες και πόρους δοκιμής web από πρότυπα](app-insights-powershell.md)
* [Ρυθμίστε την παρακολούθηση των Azure Διαγνωστικά με το PowerShell](app-insights-powershell-azure-diagnostics.md) 
* [Ορισμός ειδοποιήσεων με χρήση του PowerShell](app-insights-powershell-alerts.md)

 
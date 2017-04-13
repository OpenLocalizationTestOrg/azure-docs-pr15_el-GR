<properties
    pageTitle="Χρήση του PowerShell για να τη ρύθμιση εφαρμογής ιδέες σε μια Azure | Microsoft Azure"
    description="Αυτοματοποίηση τη ρύθμιση των παραμέτρων Διαγνωστικά Azure διοχέτευση για ιδέες εφαρμογής."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>Χρήση του PowerShell για να ρυθμίσετε ιδέες εφαρμογών για μια εφαρμογή Azure web

[Microsoft Azure](https://azure.com) μπορεί να [ρυθμιστεί για την αποστολή Διαγνωστικά του Azure](app-insights-azure-diagnostics.md) για [Ιδέες εφαρμογή του Visual Studio](app-insights-overview.md). Διαγνωστικά σχετίζονται με τις υπηρεσίες Cloud Azure και ΣΠΣ Azure. Συνοδεύουν το τηλεμετρίας που στέλνετε από μέσα στην εφαρμογή χρησιμοποιώντας την εφαρμογή SDK ιδέες. Ως μέρος της αυτοματοποίηση στη διαδικασία δημιουργίας νέων πόρων στο Azure, μπορείτε να ρυθμίσετε Διαγνωστικά χρήση του PowerShell.

## <a name="azure-template"></a>Πρότυπο Azure

Εάν η εφαρμογή web είναι στο Azure και μπορείτε να δημιουργήσετε τους πόρους σας χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure, μπορείτε να ρυθμίσετε εφαρμογή ιδέες, προσθέτοντας αυτό στον κόμβο πόρους:

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-ένα όνομα για τον πόρο ιδέες εφαρμογής
* `myWebAppName`-το αναγνωριστικό της εφαρμογής web


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Ενεργοποίηση της επέκτασης Διαγνωστικά ως μέρος του για την ανάπτυξη μια υπηρεσία στο Cloud

Το `New-AzureDeployment` cmdlet περιλαμβάνει μια παράμετρο `ExtensionConfiguration`, που θα λαμβάνει ένας πίνακας με ρυθμίσεις παραμέτρων Διαγνωστικά. Αυτές μπορούν να δημιουργηθούν με το `New-AzureServiceDiagnosticsExtensionConfig` cmdlet. Για παράδειγμα:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Ενεργοποίηση της επέκτασης Διαγνωστικά σε μια υπάρχουσα υπηρεσία Cloud

Σε μια υπάρχουσα υπηρεσία, χρησιμοποιήστε `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Λήψη τρέχουσα ρύθμιση παραμέτρων επέκταση Διαγνωστικά

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Κατάργηση Διαγνωστικά επέκτασης

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Εάν έχετε ενεργοποιήσει την επέκταση Διαγνωστικά χρησιμοποιεί ένα από τα `Set-AzureServiceDiagnosticsExtension` ή `New-AzureServiceDiagnosticsExtensionConfig` χωρίς την παράμετρο ρόλο, στη συνέχεια, μπορείτε να καταργήσετε την επέκταση χρησιμοποιώντας `Remove-AzureServiceDiagnosticsExtension` χωρίς την παράμετρο ρόλο. Εάν η παράμετρος ρόλο που χρησιμοποιήθηκε κατά την ενεργοποίηση την επέκταση, στη συνέχεια, πρέπει να επίσης να χρησιμοποιηθεί κατά την κατάργηση της επέκτασης.

Για να καταργήσετε την επέκταση Διαγνωστικά από κάθε μεμονωμένα ρόλο:

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Δείτε επίσης

* [Παρακολούθηση Azure Cloud Services εφαρμογές με ιδέες εφαρμογής](app-insights-cloudservices.md)
* [Αποστολή Azure Διαγνωστικά ιδέες εφαρμογής](app-insights-azure-diagnostics.md)
* [Αυτοματοποίηση τη ρύθμιση παραμέτρων ειδοποιήσεων](app-insights-powershell-alerts.md)


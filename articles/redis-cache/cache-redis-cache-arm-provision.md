<properties 
    pageTitle="Παροχή του Redis Cache | Microsoft Azure" 
    description="Χρήση προτύπου για τη διαχείριση πόρων Azure για την ανάπτυξη μιας Azure Redis Cache." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Δημιουργήστε ένα Cache Redis χρησιμοποιώντας ένα πρότυπο

Σε αυτό το θέμα, θα μάθετε πώς μπορείτε να δημιουργήσετε ένα πρότυπο από διαχειριστή πόρων Azure που αναπτύσσει έναν Azure Redis Cache. Το cache μπορεί να χρησιμοποιηθεί με έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης για τη διατήρηση διαγνωστικών δεδομένων. Μπορείτε επίσης μάθετε τον τρόπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι που καθορίζεται όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Προς το παρόν, διαγνωστικών ρυθμίσεις είναι κοινόχρηστες για όλες οι cache στην ίδια περιοχή για μια συνδρομή. Ενημέρωση μία cache στην περιοχή επηρεάζει όλες οι άλλες cache στην περιοχή.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md).

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα [Redis Cache πρότυπο](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Διαχείριση πόρων πρότυπα για τη νέα [σειρά Premium](cache-premium-tier-intro.md) είναι διαθέσιμα. 
>
>-    [Δημιουργήστε ένα Cache Redis Premium με συμπλέγματος](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Δημιουργία Premium Redis Cache με διατήρησης δεδομένων](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Δημιουργήστε Premium Redis Cache με VNet και προαιρετικά συμπλέγματος](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Για να ελέγξετε την πιο πρόσφατη προτύπων, ανατρέξτε στο θέμα [Πρότυπα γρήγορη έναρξη Azure](https://azure.microsoft.com/documentation/templates/) και αναζητήστε το `Redis Cache`.

## <a name="what-you-will-deploy"></a>Τι θα αναπτύξετε

Σε αυτό το πρότυπο θα αναπτύξετε μια Azure Redis Cache που χρησιμοποιεί έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης για διαγνωστικών δεδομένων.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται παράμετροι που περιέχει όλες τις τιμές παραμέτρων.
Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που διαφέρουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που παραμένουν πάντα τα ίδια. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Η θέση της μνήμης Cache Redis. Για καλύτερες επιδόσεις, χρησιμοποιήστε την ίδια θέση ως την εφαρμογή για χρήση με το cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Το όνομα του υπάρχοντος λογαριασμού χώρου αποθήκευσης για να χρησιμοποιήσετε για τα Διαγνωστικά. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Μια τιμή boolean που υποδεικνύει εάν θέλετε να επιτρέψετε την πρόσβαση μέσω θύρες χωρίς SSL.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Μια τιμή που υποδεικνύει εάν έχει ενεργοποιηθεί Διαγνωστικά. Χρήση ή ΑΠΕΝΕΡΓΟΠΟΙΉΣΤΕ.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

### <a name="redis-cache"></a>Redis Cache

Δημιουργεί το Cache Azure Redis.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup



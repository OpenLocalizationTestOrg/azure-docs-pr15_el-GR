<properties 
    pageTitle="Διάταξη Web App με Redis Cache" 
    description="Χρήση προτύπου για τη διαχείριση πόρων Azure για ανάπτυξη εφαρμογής web με Redis Cache." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erickson-doug" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="create-a-web-app-plus-redis-cache-using-a-template"></a>Δημιουργία μιας εφαρμογής Web μαζί με Redis Cache χρησιμοποιώντας ένα πρότυπο

Σε αυτό το θέμα, θα μάθετε πώς μπορείτε να δημιουργήσετε ένα πρότυπο από διαχειριστή πόρων Azure που αναπτύσσει μια εφαρμογή Web Azure με Redis cache. Θα μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md).

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα [Web App με το πρότυπο Redis Cache](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Τι θα αναπτύξετε

Σε αυτό το πρότυπο θα αναπτύξετε:

- Azure Web App
- Azure Redis Cache.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Για να καθορίσετε τις παραμέτρους

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[AZURE.INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Μεταβλητές για τα ονόματα

Αυτό το πρότυπο χρησιμοποιεί μεταβλητές για να δημιουργήσετε ονόματα για τους πόρους. Χρησιμοποιεί τη συνάρτηση [uniqueString](../resource-group-template-functions.md#uniquestring) για να δημιουργήσετε μια τιμή με βάση το αναγνωριστικό ομάδας πόρων.

    "variables": {
      "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
      "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
      "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
    },


## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="redis-cache"></a>Redis Cache

Δημιουργεί το Azure Redis Cache που χρησιμοποιείται με την εφαρμογή web. Το όνομα της μνήμης cache έχει καθοριστεί στη μεταβλητή **cacheName** .

Το πρότυπο δημιουργεί το cache στην ίδια θέση με την ομάδα των πόρων. 

    {
      "name": "[variables('cacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [ ],
      "tags": {
        "displayName": "cache"
      },
      "properties": {
        "sku": {
          "name": "[parameters('cacheSKUName')]",
          "family": "[parameters('cacheSKUFamily')]",
          "capacity": "[parameters('cacheSKUCapacity')]"
        }
      }
    }


### <a name="web-app"></a>Εφαρμογή Web

Δημιουργεί την εφαρμογή web με το όνομα που καθορίζεται στη μεταβλητή **webSiteName** .

Παρατηρήστε ότι η εφαρμογή web έχει ρυθμιστεί με εφαρμογή ρύθμιση των ιδιοτήτων που σας επιτρέπουν να εργαστείτε με το Cache Redis. Αυτή η εφαρμογή ρυθμίσεων δημιουργούνται δυναμικά με βάση τις τιμές που παρέχονται κατά την ανάπτυξη.
        
    {
      "apiVersion": "2015-08-01",
      "name": "[variables('webSiteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "tags": {
        "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
        "displayName": "Website"
      },
      "properties": {
        "name": "[variables('webSiteName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "type": "config",
          "name": "appsettings",
          "dependsOn": [
            "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
            "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
          ],
          "properties": {
            "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup



<properties 
    pageTitle="Δημιουργία εφαρμογής λογικής με τη χρήση προτύπων από διαχειριστή πόρων Azure στο Azure εφαρμογής υπηρεσίας | Microsoft Azure" 
    description="Χρήση ενός προτύπου για τη διαχείριση πόρων Azure για την ανάπτυξη μια κενή εφαρμογή λογική για τον ορισμό ροών εργασίας." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Δημιουργία εφαρμογής λογικής χρησιμοποιώντας ένα πρότυπο

Χρησιμοποιήστε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε μια εφαρμογή της λογικής κενή που μπορούν να χρησιμοποιηθούν για να ορίσετε τις ροές εργασίας. Μπορείτε να ορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε παραμέτρους που καθορίζονται όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Για περισσότερες λεπτομέρειες σχετικά με τις ιδιότητες εφαρμογής λογικής, ανατρέξτε στο θέμα [Λογική εφαρμογής ροής εργασίας διαχείρισης API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Για παραδείγματα του ορισμού της ίδιας, δείτε [ορισμών συντάκτη λογική εφαρμογής](app-service-logic-author-definitions.md). 

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md).

Για το πρότυπο ολοκλήρωσης, ανατρέξτε στο θέμα [εφαρμογή λογικής πρότυπο](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Τι θα αναπτύξετε

Αυτό το πρότυπο, μπορείτε να αναπτύξετε μια εφαρμογή λογικής.

Για να εκτελέσετε την ανάπτυξη αυτόματα, επιλέξτε το παρακάτω κουμπί:  

[![Ανάπτυξη Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

### <a name="logic-app"></a>Λογική εφαρμογής

Δημιουργεί την εφαρμογή λογικής.

Τα πρότυπα που χρησιμοποιεί μια τιμή παραμέτρου για το όνομα εφαρμογής λογικής. Ορίζει τη θέση της εφαρμογής λογικής στο στην ίδια θέση με την ομάδα των πόρων. 

Αυτός ο ορισμός συγκεκριμένης εκτελείται μία φορά κάθε ώρα και κάνει ping στη θέση που καθορίζεται στην παράμετρο **testUri** . 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 

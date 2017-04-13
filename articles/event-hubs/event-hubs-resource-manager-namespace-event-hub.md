<properties
    pageTitle="Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντων και καταναλωτή ομάδα με χρήση ενός προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντων και καταναλωτή ομάδα με το πρότυπο διαχείρισης πόρων Azure"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντων και καταναλωτή ομάδα με χρήση ενός προτύπου για τη διαχείριση πόρων Azure

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί ένα χώρο ονομάτων διανομείς συμβάν με ένα διανομέα συμβάντων και μια ομάδα καταναλωτή. Θα μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις ή προσαρμόστε το ώστε να πληρούνται οι απαιτήσεις σας

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα][].

Για το πρότυπο ολοκλήρωσης, ανατρέξτε στο [συμβάν διανομέα και πρότυπο ομάδα καταναλωτή][] σε GitHub.

>[AZURE.NOTE]
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και αναζήτησης για διανομείς συμβάν.

## <a name="what-will-you-deploy"></a>Τι θα αναπτύξετε;

Με αυτό το πρότυπο θα αναπτύξετε ένα συμβάν διανομείς χώρο ονομάτων με ένα διανομέα συμβάντων και μια ομάδα καταναλωτή.

[Διανομείς συμβάν](../event-hubs/event-hubs-what-is-event-hubs.md) είναι ένα συμβάν υπηρεσία που χρησιμοποιείται για την παροχή εισόδου συμβάντος και τηλεμετρίας σε Azure σε μεγάλους κλίμακα, με χαμηλή λανθάνοντα χρόνο και την υψηλή αξιοπιστία επεξεργασίας.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται `Parameters` που περιέχει όλες τις τιμές παραμέτρων. Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που θα ποικίλλουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που θα παραμείνει πάντα το ίδιο. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί.

Το πρότυπο καθορίζει τις ακόλουθες παραμέτρους.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Το όνομα του χώρου ονομάτων διανομείς συμβάντος για να δημιουργήσετε.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Το όνομα του συμβάντος διανομέα συμβάντων δημιουργηθεί χώρος ονομάτων διανομείς.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Το όνομα της ομάδας καταναλωτή που έχει δημιουργηθεί για την ενότητα συμβάντων.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Η έκδοση API του προτύπου.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

Δημιουργεί ένα χώρο ονομάτων του τύπου **EventHubs**, με ένα διανομέα συμβάντων και μια ομάδα καταναλωτών.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
[Γρήγορη έναρξη Azure προτύπων]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Πρότυπο ομάδα διανομέα και καταναλωτή συμβάντος]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/

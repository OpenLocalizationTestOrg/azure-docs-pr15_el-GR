<properties
    pageTitle="Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσία χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων | Microsoft Azure"
    description="Χρήση προτύπου για τη διαχείριση πόρων Azure για να δημιουργήσετε ένα χώρο ονομάτων Bus υπηρεσίας"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσία χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί ένα πεδίο Bus υπηρεσία ονομάτων τύπου **μηνυμάτων** με μια τυπική/Basic SKU. Το άρθρο καθορίζει επίσης οι παράμετροι που έχουν καθοριστεί για την εκτέλεση της ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα][].

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα του [προτύπου χώρου ονομάτων Bus υπηρεσίας][] GitHub.

>[AZURE.NOTE] Τα παρακάτω πρότυπα διαχείρισης πόρων Azure είναι διαθέσιμο για λήψη και ανάπτυξη. 
>
>-    [Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με μια ομάδα διανομέα συμβάντων και καταναλωτή](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](service-bus-resource-manager-namespace-queue.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομή](service-bus-resource-manager-namespace-topic.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά και εξουσιοδότησης κανόνα](service-bus-resource-manager-namespace-auth-rule.md)
>
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και να αναζητήσετε Bus υπηρεσίας.

## <a name="what-will-you-deploy"></a>Τι θα αναπτύξετε;

Με αυτό το πρότυπο θα αναπτύξετε ένα χώρο ονομάτων υπηρεσίας Bus με μια [Βασική, τυπικό, ή Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται `Parameters` που περιέχει όλες τις τιμές παραμέτρων. Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που θα ποικίλλουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που θα παραμείνει πάντα το ίδιο. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί.

Αυτό το πρότυπο καθορίζει τις ακόλουθες παραμέτρους.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Το όνομα του χώρου ονομάτων Bus υπηρεσίας για να δημιουργήσετε.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

Το όνομα του την υπηρεσία Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) για να δημιουργήσετε.

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Το πρότυπο καθορίζει τις τιμές που επιτρέπονται για αυτήν την παράμετρο (Basic, τυπική ή Premium) και αντιστοιχίζει μια προεπιλεγμένη τιμή (τυπική), εάν δεν καθορίζεται μια τιμή.

Για περισσότερες πληροφορίες σχετικά με τις τιμές υπηρεσία Bus, ανατρέξτε στο θέμα [υπηρεσία Bus τις τιμές και τις χρεώσεις][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Η έκδοση υπηρεσίας Bus API του προτύπου.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

### <a name="service-bus-namespace"></a>Χώρος ονομάτων Bus υπηρεσίας

Δημιουργεί ένα τυπικό χώρο ονομάτων Bus υπηρεσίας του τύπου **μηνυμάτων**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει και αναπτύξει πόρων με χρήση της διαχείρισης πόρων Azure, μάθετε πώς μπορείτε να διαχειριστείτε αυτούς τους πόρους, διαβάζοντας αυτά τα άρθρα:

- [Διαχείριση υπηρεσίας Bus με το PowerShell](service-bus-powershell-how-to-provision.md)
- [Διαχείριση πόρων υπηρεσίας Bus με την Εξερεύνηση Bus υπηρεσίας](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
  [Υπηρεσία Bus προτύπου χώρου ονομάτων]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Γρήγορη έναρξη Azure προτύπων]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Υπηρεσία Bus τις τιμές και τις χρεώσεις]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md

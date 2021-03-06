<properties
    pageTitle="Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομής με χρήση ενός προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομή χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομής με χρήση ενός προτύπου για τη διαχείριση πόρων Azure

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί ένα πεδίο Bus υπηρεσία ονομάτων με ένα θέμα και μια συνδρομή. Θα μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις ή προσαρμόστε το ώστε να πληρούνται οι απαιτήσεις σας

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα][].

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα το πρότυπο [Bus υπηρεσίας χώρο ονομάτων με το θέμα και συνδρομών][] .

>[AZURE.NOTE] Τα παρακάτω πρότυπα διαχείρισης πόρων Azure είναι διαθέσιμο για λήψη και ανάπτυξη.
>
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά και εξουσιοδότησης κανόνα](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](service-bus-resource-manager-namespace-queue.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας](service-bus-resource-manager-namespace.md)
>-    [Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με μια ομάδα διανομέα συμβάντων και καταναλωτή](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και κάντε αναζήτηση για "Bus υπηρεσίας".

## <a name="what-will-you-deploy"></a>Τι θα αναπτύξετε;

Με αυτό το πρότυπο θα αναπτύξετε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομών.

[Θέματα υπηρεσίας Bus και συνδρομές](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) παρέχουν ένα-προς-πολλά φόρμας επικοινωνίας, σε ένα μοτίβο *για δημοσίευση/εγγραφή* .

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται `Parameters` που περιέχει όλες τις τιμές παραμέτρων. Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που θα ποικίλλουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που θα παραμείνει πάντα το ίδιο. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί.

Το πρότυπο καθορίζει τις ακόλουθες παραμέτρους.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Το όνομα του χώρου ονομάτων Bus υπηρεσίας για να δημιουργήσετε.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Το όνομα του θέματος που έχουν δημιουργηθεί με το χώρο ονομάτων Bus υπηρεσίας.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Το όνομα της συνδρομής που έχουν δημιουργηθεί με το χώρο ονομάτων Bus υπηρεσίας.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Η έκδοση υπηρεσίας Bus API του προτύπου.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

Δημιουργεί ένα τυπικό χώρο ονομάτων Bus υπηρεσίας του τύπου **μηνυμάτων**, με το θέμα και συνδρομών.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει και αναπτύξει πόρων με χρήση της διαχείρισης πόρων Azure, μάθετε πώς μπορείτε να διαχειριστείτε αυτούς τους πόρους, προβάλλοντας αυτά τα άρθρα:

- [Διαχείριση υπηρεσίας Bus με το PowerShell](service-bus-powershell-how-to-provision.md)
- [Διαχείριση πόρων υπηρεσίας Bus με την Εξερεύνηση Bus υπηρεσίας](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
  [Γρήγορη έναρξη Azure προτύπων]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Υπηρεσία Bus χώρο ονομάτων με το θέμα και συνδρομών]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/

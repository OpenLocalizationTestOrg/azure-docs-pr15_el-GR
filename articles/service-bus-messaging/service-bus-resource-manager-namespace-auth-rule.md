<properties
    pageTitle="Δημιουργία κανόνα εξουσιοδότησης Bus υπηρεσία χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure | Microsoft Azure"
    description="Δημιουργία κανόνα εξουσιοδότησης Bus υπηρεσίας για το χώρο ονομάτων και ουρά με χρήση προτύπου για τη διαχείριση πόρων Azure"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Δημιουργία κανόνα εξουσιοδότησης Bus υπηρεσίας για το χώρο ονομάτων και ουρά με χρήση ενός προτύπου για τη διαχείριση πόρων Azure

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί έναν [κανόνα εξουσιοδότησης](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) για ένα χώρο ονομάτων Bus υπηρεσίας και ουρά. Θα μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις, ή να προσαρμόσετε ώστε να πληρούν τις απαιτήσεις σας.

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα][].

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα του [προτύπου κανόνων auth Bus υπηρεσίας][] GitHub.

>[AZURE.NOTE] Τα παρακάτω πρότυπα διαχείρισης πόρων Azure είναι διαθέσιμο για λήψη και ανάπτυξη.
>
>-    [Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με μια ομάδα διανομέα συμβάντων και καταναλωτή](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](service-bus-resource-manager-namespace-queue.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομή](service-bus-resource-manager-namespace-topic.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας](service-bus-resource-manager-namespace.md)
>
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και κάντε αναζήτηση για "Bus υπηρεσίας".

## <a name="what-will-you-deploy"></a>Τι θα αναπτύξετε;

Με αυτό το πρότυπο θα αναπτύξετε έναν κανόνα εξουσιοδότησης Bus υπηρεσίας για ένα χώρο ονομάτων και μηνυμάτων οντότητα (στη συγκεκριμένη περίπτωση, μια ουρά).

Αυτό το πρότυπο χρησιμοποιεί [Θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας)](service-bus-sas-overview.md) για τον έλεγχο ταυτότητας. Συσχετίσεις Ασφαλείας επιτρέπει σε εφαρμογές για τον έλεγχο ταυτότητας Bus υπηρεσία χρησιμοποιώντας ένα πλήκτρο πρόσβασης έχει ρυθμιστεί στο χώρο ονομάτων ή την ανταλλαγή μηνυμάτων οντότητα (ουρά ή θέμα) με την οποία σχετίζονται συγκεκριμένα δικαιώματα. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε αυτό το κλειδί για τη δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας που προγράμματα-πελάτες με τη σειρά να χρησιμοποιήσετε για τον έλεγχο ταυτότητας Bus υπηρεσίας.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Το όνομα του κανόνα εξουσιοδότησης για το χώρο ονομάτων.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Το όνομα της ουράς στο χώρο ονομάτων Bus υπηρεσίας.

```
"serviceBusQueueName": {
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

Δημιουργεί ένα τυπικό χώρο ονομάτων Bus υπηρεσίας του τύπου **ανταλλαγή μηνυμάτων**και ένας κανόνας εξουσιοδότησης Bus υπηρεσίας για το χώρο ονομάτων και οντότητα.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει και αναπτύξει πόρων με χρήση της διαχείρισης πόρων Azure, μάθετε πώς μπορείτε να διαχειριστείτε αυτούς τους πόρους, προβάλλοντας αυτά τα άρθρα:

- [Διαχείριση υπηρεσίας Bus με το PowerShell](service-bus-powershell-how-to-provision.md)
- [Διαχείριση πόρων υπηρεσίας Bus με την Εξερεύνηση Bus υπηρεσίας](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Υπηρεσία Bus τον έλεγχο ταυτότητας και εξουσιοδότησης](service-bus-authentication-and-authorization.md)

  [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
  [Γρήγορη έναρξη Azure προτύπων]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Πρότυπο κανόνα auth Bus υπηρεσίας]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/

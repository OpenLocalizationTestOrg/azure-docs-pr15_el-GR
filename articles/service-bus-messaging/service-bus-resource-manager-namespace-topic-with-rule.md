<properties
    pageTitle="Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα, τη συνδρομή, και του κανόνα με χρήση ενός προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα, τη συνδρομή και κανόνα χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα, τη συνδρομή και κανόνα με χρήση ενός προτύπου για τη διαχείριση πόρων Azure

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί ένα πεδίο Bus υπηρεσία ονομάτων με ένα θέμα, τη συνδρομή και κανόνα (Φίλτρο). Μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις ή προσαρμόστε το ώστε να πληρούνται οι απαιτήσεις σας

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα][].

Για περισσότερες πληροφορίες σχετικά με τις πρακτικές και τα μοτίβα σε πόρους Azure συμβάσεις ονοματοδοσίας, ανατρέξτε στο θέμα [Συμβάσεις ονομασίας Azure πόρους][].

Για την πλήρη πρότυπο, ανατρέξτε στο θέμα το πρότυπο [Bus υπηρεσίας χώρο ονομάτων με το θέμα, τη συνδρομή, και τον κανόνα][] .

>[AZURE.NOTE] Τα παρακάτω πρότυπα διαχείρισης πόρων Azure είναι διαθέσιμο για λήψη και ανάπτυξη.
>
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά και εξουσιοδότησης κανόνα](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](service-bus-resource-manager-namespace-queue.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας](service-bus-resource-manager-namespace.md)
>-    [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομή](service-bus-resource-manager-namespace-topic.md)
>
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και να αναζητήσετε Bus υπηρεσίας.

## <a name="what-will-you-deploy"></a>Τι θα αναπτύξετε;

Αυτό το πρότυπο, μπορείτε να αναπτύξετε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα, τη συνδρομή και κανόνα (Φίλτρο).

[Θέματα υπηρεσίας Bus και συνδρομές](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) παρέχουν ένα-προς-πολλά φόρμας επικοινωνίας, σε ένα μοτίβο *για δημοσίευση/εγγραφή* . Όταν χρησιμοποιείτε τα θέματα και οι συνδρομές, στοιχεία μιας εφαρμογής κατανεμημένες δεν επικοινωνούν απευθείας μεταξύ τους, αντί για αυτό τους ανταλλαγή μηνυμάτων μέσω του θέματος που λειτουργεί ως ενδιάμεσο. Μια συνδρομή σε ένα θέμα μοιάζει με μια εικονική ουρά που λαμβάνει αντίγραφα των μηνυμάτων που έχουν αποσταλεί στο θέμα. Ένα φίλτρο στη συνδρομή σας επιτρέπει να καθορίσετε ποια μηνύματα που αποστέλλονται σε ένα θέμα θα πρέπει να εμφανίζεται μέσα σε μια συνδρομή στο συγκεκριμένο θέμα.

## <a name="what-are-rules-filters"></a>Τι είναι οι κανόνες (φίλτρα);

Σε πολλά σενάρια, τα μηνύματα που έχουν συγκεκριμένα χαρακτηριστικά πρέπει να επεξεργαστούν με διαφορετικούς τρόπους. Για να ενεργοποιήσετε αυτήν, μπορείτε να ρυθμίσετε συνδρομές για να βρείτε μηνύματα που έχουν επιθυμητοί ιδιότητες και, στη συνέχεια, να εκτελέσετε ορισμένες τροποποιήσεις σε αυτές τις ιδιότητες. Κατά την υπηρεσία Bus συνδρομές δείτε όλα τα μηνύματα που αποστέλλονται στο θέμα, μπορείτε να αντιγράψετε μόνο ένα υποσύνολο αυτά τα μηνύματα στην ουρά εικονικού συνδρομής. Αυτή η ενέργεια πραγματοποιείται με τη συνδρομή φίλτρα. Για να μάθετε περισσότερα σχετικά με το rules(filters), ανατρέξτε στο θέμα [υπηρεσία Bus ουρές, θέματα, και συνδρομών][].

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, πρέπει να ορίσετε τις παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται `Parameters` που περιέχει όλες τις τιμές παραμέτρων. Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που διαφέρουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που παραμένουν πάντα τα ίδια. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί.

Το πρότυπο καθορίζει τις ακόλουθες παραμέτρους:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

Το όνομα του το rule(filter) που έχουν δημιουργηθεί με το χώρο ονομάτων Bus υπηρεσίας.

```
   "serviceBusRuleName": {
   "type": "string",
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

Δημιουργεί ένα τυπικό χώρο ονομάτων Bus υπηρεσίας του τύπου **μηνυμάτων**, με το θέμα και συνδρομής και οι κανόνες.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δημιουργήσει και αναπτύξει πόρων με χρήση της διαχείρισης πόρων Azure, μάθετε πώς μπορείτε να διαχειριστείτε αυτούς τους πόρους, προβάλλοντας αυτά τα άρθρα:

- [Διαχείριση Bus υπηρεσίας Azure χρησιμοποιώντας αυτοματισμού Azure](service-bus-automation-manage.md)
- [Διαχείριση υπηρεσίας Bus με το PowerShell](service-bus-powershell-how-to-provision.md)
- [Διαχείριση πόρων υπηρεσίας Bus με την Εξερεύνηση Bus υπηρεσίας](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
  [Γρήγορη έναρξη Azure προτύπων]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure πόρους συμβάσεις ονοματοδοσίας]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Υπηρεσία Bus χώρο ονομάτων με το θέμα, τη συνδρομή και κανόνα]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Υπηρεσία Bus ουρές, θέματα και συνδρομών]:service-bus-queues-topics-subscriptions.md
  

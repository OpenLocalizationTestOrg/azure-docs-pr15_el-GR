<properties
    pageTitle="Δημιουργία υπηρεσίας Bus πόρων με τη χρήση προτύπων από διαχειριστή πόρων Azure | Microsoft Azure"
    description="Χρησιμοποιήστε πρότυπα Azure από διαχειριστή πόρων για να αυτοματοποιήσετε τη δημιουργία των πόρων Bus υπηρεσίας"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Δημιουργία υπηρεσίας Bus πόρων με τη χρήση προτύπων από διαχειριστή πόρων Azure

Σε αυτό το άρθρο περιγράφει τον τρόπο δημιουργίας και ανάπτυξης Bus υπηρεσίας και διανομείς συμβάν πόρων με τους πρότυπα διαχείρισης πόρων Azure, PowerShell και η υπηρεσία παροχής υπηρεσίας Bus πόρων.

Azure πρότυπα διαχείρισης πόρων σας βοηθούν να ορίσετε τους πόρους για να αναπτύξετε μια λύση και για να καθορίσετε τις παραμέτρους και μεταβλητές που δίνουν τη δυνατότητα να εισαγάγετε τιμές για διαφορετικά περιβάλλοντα. Το πρότυπο αποτελείται από JSON και παραστάσεις που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε τιμές για την ανάπτυξη. Για λεπτομερείς πληροφορίες σχετικά με τη σύνταξη από διαχειριστή πόρων Azure πρότυπα και συζήτησης της μορφής πρότυπο, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Τα παραδείγματα σε αυτό το άρθρο δείχνουν πώς μπορείτε να χρησιμοποιήσετε για τη διαχείριση πόρων Azure για να δημιουργήσετε ένα χώρο ονομάτων Bus υπηρεσίας και οντότητα ανταλλαγής μηνυμάτων (ουρά). Για άλλα παραδείγματα πρότυπο, επισκεφθείτε τη [συλλογή προτύπων γρήγορη έναρξη Azure][] και κάντε αναζήτηση για "Bus υπηρεσίας".

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Πρότυπα Bus υπηρεσίας και διαχείριση πόρων διανομείς συμβάντος

Αυτά τα πρότυπα Bus υπηρεσίας και διαχείριση πόρων Azure διανομείς συμβάν είναι διαθέσιμα για λήψη και ανάπτυξη. Κάντε κλικ στις παρακάτω συνδέσεις για λεπτομέρειες σχετικά με κάθε μία, με συνδέσεις για τα πρότυπα GitHub: 

- [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας](service-bus-resource-manager-namespace.md)
- [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](service-bus-resource-manager-namespace-queue.md)
- [Δημιουργήστε ένα χώρο ονομάτων Bus υπηρεσίας με το θέμα και συνδρομή](service-bus-resource-manager-namespace-topic.md)
- [Δημιουργήστε ένα χώρο ονομάτων υπηρεσίας Bus με ουρά και εξουσιοδότησης κανόνα](service-bus-resource-manager-namespace-auth-rule.md)
- [Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με μια ομάδα διανομέα συμβάντων και καταναλωτή](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>Ανάπτυξη με το PowerShell

Η παρακάτω διαδικασία περιγράφει τον τρόπο χρήσης του PowerShell για να αναπτύξετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί μια **Τυπική** χώρο ονομάτων υπηρεσίας Bus σειρά, και μια ουρά μέσα σε αυτό το πεδίο ονομάτων. Αυτό το παράδειγμα βασίζεται στο πρότυπο [Δημιουργία ένα χώρο ονομάτων υπηρεσίας Bus με ουρά](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) . Η ροή εργασίας κατά προσέγγιση είναι ως εξής:

1. Εγκατάσταση του PowerShell.
2. Δημιουργία του προτύπου και (προαιρετικά) ένα αρχείο παραμέτρων.
2. Στο PowerShell, συνδεθείτε στο λογαριασμό σας στο Azure.
3. Δημιουργία νέας ομάδας πόρων, εάν δεν υπάρχει.
4. Δοκιμή της ανάπτυξης.
5. Αν θέλετε, ορίστε τη λειτουργία ανάπτυξη.
6. Ανάπτυξη του προτύπου.

Για πλήρεις πληροφορίες σχετικά με την ανάπτυξη προτύπων για τη διαχείριση πόρων Azure, ανατρέξτε στο θέμα [Ανάπτυξη πόρους με τα πρότυπα για τη διαχείριση πόρων Azure][].

### <a name="install-powershell"></a>Εγκατάσταση του PowerShell

Εγκατάσταση του Azure PowerShell, ακολουθώντας τις οδηγίες στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

### <a name="create-a-template"></a>Δημιουργία προτύπου

Δημιουργία διπλότυπου ή να αντιγράψετε το πρότυπο [201-servicebus-δημιουργία-ουρά](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) από GitHub:

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Δημιουργία αρχείου παράμετροι (προαιρετικά)

Για να χρησιμοποιήσετε ένα αρχείο προαιρετικών παραμέτρων, αντιγράψτε το αρχείο [201-servicebus-δημιουργία-ουρά](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) . Αντικαταστήστε την τιμή της `serviceBusNamespaceName` με το όνομα του χώρου ονομάτων Bus υπηρεσία που θέλετε να δημιουργήσετε με αυτήν την ανάπτυξη και αντικαταστήστε την τιμή της `serviceBusQueueName` με το όνομα της ουράς που θέλετε να δημιουργήσετε. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αρχείο παραμέτρων](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Συνδεθείτε στο Azure και ορίστε τη συνδρομή Azure

Από μια γραμμή εντολών του PowerShell, εκτελέστε την ακόλουθη εντολή:

```
Login-AzureRmAccount
```

Θα σας ζητηθεί να συνδεθείτε στο λογαριασμό σας Azure. Μετά τη σύνδεση, εκτελέστε την ακόλουθη εντολή για να προβάλετε όλες τις συνδρομές σας διαθέσιμη.

```
Get-AzureRMSubscription
```

Αυτή η εντολή επιστρέφει μια λίστα με τις διαθέσιμες Azure συνδρομές. Επιλέξτε μια συνδρομή για την τρέχουσα περίοδο λειτουργίας, εκτελώντας την ακόλουθη εντολή. Αντικατάσταση `<YourSubscriptionId>` με το GUID για τη συνδρομή Azure που θέλετε να χρησιμοποιήσετε.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Ορίστε την ομάδα των πόρων

Εάν δεν έχετε μια υπάρχουσα ομάδα πόρων, δημιουργήστε μια νέα ομάδα πόρων με την εντολή **Δημιουργία AzureRmResourceGroup** . Εισαγάγετε το όνομα της ομάδας πόρων και θέση που θέλετε να χρησιμοποιήσετε. Για παράδειγμα:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Εάν είναι επιτυχής, εμφανίζεται μια σύνοψη της νέας ομάδας πόρων.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Δοκιμή της ανάπτυξης

Επικυρώστε την ανάπτυξη, εκτελώντας το `Test-AzureRmResourceGroupDeployment` cmdlet. Κατά τη δοκιμή ανάπτυξης, δώστε τις παραμέτρους ακριβώς όπως θα κάνατε κατά την εκτέλεση της ανάπτυξης.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Δημιουργία ανάπτυξης

Για να δημιουργήσετε νέα ανάπτυξης, εκτελέστε το `New-AzureRmResourceGroupDeployment` command και δώστε τις απαραίτητες παραμέτρους όταν σας ζητηθεί. Οι παράμετροι περιλαμβάνουν ένα όνομα για την ανάπτυξη, το όνομα του σας ομάδα πόρων, και τη διαδρομή ή διεύθυνση URL για το αρχείο προτύπου. Εάν η παράμετρος **λειτουργία** δεν έχει καθοριστεί, χρησιμοποιείται η προεπιλεγμένη τιμή **προσαύξηση** . Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [προσαύξηση και ολοκλήρωση αναπτύξεις](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Η παρακάτω εντολή σάς ζητά τις τρεις παραμέτρους στο παράθυρο του PowerShell:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Για να καθορίσετε ένα αρχείο παραμέτρων αντί για αυτό, χρησιμοποιήστε την ακόλουθη εντολή.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Μπορείτε επίσης να χρησιμοποιήσετε τις παραμέτρους ενσωματωμένη όταν εκτελείτε το cmdlet ανάπτυξης. Η εντολή είναι ως εξής:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Για να εκτελέσετε μια [πλήρη](../resource-group-template-deploy.md#incremental-and-complete-deployments) ανάπτυξη, ορίστε την παράμετρο **λειτουργία** **ολοκληρώθηκε**:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Επαληθεύστε την ανάπτυξη

Αν οι πόροι έχουν αναπτυχθεί με επιτυχία, εμφανίζεται μια σύνοψη της ανάπτυξης στο παράθυρο του PowerShell:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα που έχετε δει τις εντολές για την ανάπτυξη ενός προτύπου για τη διαχείριση πόρων Azure και βασικές ροής εργασίας. Για πιο λεπτομερείς πληροφορίες, επισκεφθείτε τις παρακάτω συνδέσεις:

- [Azure Επισκόπηση της διαχείρισης πόρων][]
- [Ανάπτυξη των πόρων με τα πρότυπα για τη διαχείριση πόρων Azure][]
- [Σύνταξη από κοινού προτύπων](../resource-group-authoring-templates.md)


[Azure Επισκόπηση της διαχείρισης πόρων]: ../resource-group-overview.md
[Ανάπτυξη των πόρων με τα πρότυπα για τη διαχείριση πόρων Azure]: ../resource-group-template-deploy.md
[Azure συλλογή προτύπων γρήγορη έναρξη]: https://azure.microsoft.com/documentation/templates/?term=service+bus
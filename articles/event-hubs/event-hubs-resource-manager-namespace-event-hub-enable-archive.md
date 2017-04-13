<properties
    pageTitle="Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντων και ενεργοποίηση αρχειοθέτηση με χρήση ενός προτύπου για τη διαχείριση πόρων Azure | Microsoft Azure"
    description="Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντος και ενεργοποίηση αρχειοθέτησης χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Δημιουργήστε ένα συμβάν διανομείς χώρο ονομάτων με διανομέα συμβάντων και ενεργοποίηση αρχειοθέτησης χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure που δημιουργεί ένα συμβάν διανομείς χώρο ονομάτων με ένα διανομέα συμβάντων και επιτρέπει την αρχειοθέτηση στο κέντρο σας συμβάν. Μάθετε πώς να καθορίσετε τους πόρους που έχουν αναπτυχθεί και πώς μπορείτε να ορίσετε τις παραμέτρους που είναι καθοριστεί όταν εκτελείται το ανάπτυξης. Μπορείτε να χρησιμοποιήσετε αυτό το πρότυπο για τη δική σας αναπτύξεις ή προσαρμόστε το ώστε να πληρούνται οι απαιτήσεις σας

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία προτύπων, ανατρέξτε στο θέμα [σύνταξη από κοινού από διαχειριστή πόρων Azure πρότυπα][].

Για περισσότερες πληροφορίες σχετικά με τις πρακτικές και τα μοτίβα σε πόρους Azure συμβάσεις ονοματοδοσίας, ανατρέξτε στο θέμα [Συμβάσεις ονομασίας Azure πόρους][].

Για το πρότυπο ολοκλήρωσης, ανατρέξτε στο [συμβάν διανομέα και ενεργοποιήσετε πρότυπο αρχειοθέτησης][] σε GitHub.

>[AZURE.NOTE]
>Για να ελέγξετε την πιο πρόσφατη πρότυπα, επισκεφθείτε τη συλλογή [Προτύπων γρήγορη έναρξη Azure][] και αναζήτησης για διανομείς συμβάν.

## <a name="what-you-deploy"></a>Τι να αναπτύξετε;

Με αυτό το πρότυπο, μπορείτε να αναπτύξετε ένα συμβάν διανομείς χώρο ονομάτων με ένα διανομέα συμβάντος και ενεργοποίηση αρχειοθέτησης.

[Διανομείς συμβάν](../event-hubs/event-hubs-what-is-event-hubs.md) είναι ένα συμβάν processing υπηρεσία που χρησιμοποιείται για την παροχή εισόδου συμβάντος και τηλεμετρίας σε Azure σε μεγάλους κλίμακα, με χαμηλή λανθάνοντα χρόνο και την υψηλή αξιοπιστία. Αρχειοθέτηση διανομείς συμβάν θα σάς επιτρέπουν να παρέχουν αυτόματα η ροή δεδομένων σε σας διανομείς συμβάν με το χώρο αποθήκευσης αντικειμένων Blob του Azure της επιλογής σας μέσα σε μια συγκεκριμένη χρονική στιγμή ή διάστημα μεγέθους της επιλογής σας.

Για να εκτελέσετε την ανάπτυξη αυτόματα, κάντε κλικ στο κουμπί παρακάτω:

[![Ανάπτυξη Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Παράμετροι

Με το Azure διαχείριση πόρων, ορίζετε παραμέτρους για τις τιμές που θέλετε να καθορίσετε πότε έχει αναπτυχθεί το πρότυπο. Το πρότυπο περιλαμβάνει μια ενότητα που ονομάζεται `Parameters` που περιέχει όλες τις τιμές παραμέτρων. Θα πρέπει να μπορείτε να καθορίσετε μια παράμετρο για τις τιμές που διαφέρουν ανάλογα με το έργο που αναπτύσσετε ή με βάση το περιβάλλον αναπτύσσετε σε. Δεν ορίσετε τις παραμέτρους για τιμές που παραμένουν πάντα τα ίδια. Κάθε τιμή παραμέτρου χρησιμοποιείται στο πρότυπο για να ορίσετε τους πόρους που έχουν αναπτυχθεί.

Το πρότυπο καθορίζει τις ακόλουθες παραμέτρους.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Το όνομα του χώρου ονομάτων διανομείς συμβάντων για να δημιουργήσετε.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Το όνομα του συμβάντος διανομέα συμβάντων δημιουργηθεί χώρος ονομάτων διανομείς.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Ο αριθμός των ημερών που θέλετε τα μηνύματα που διατηρούνται στο κέντρο σας συμβάν. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Ο αριθμός των διαμερισμάτων που θέλετε από την ενότητα συμβάντων.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Ενεργοποιήστε την αρχειοθέτηση στο κέντρο σας συμβάν.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Η μορφή κωδικοποίησης καθορίζετε σειριοποίηση τα δεδομένα του συμβάντος.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Το χρονικό διάστημα στο οποίο η αρχειοθέτηση ξεκινά αρχειοθέτηση τα δεδομένα στο χώρο αποθήκευσης αντικειμένων blob του Azure.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Το μέγεθος το χρονικό διάστημα στο οποίο η αρχειοθέτηση ξεκινά αρχειοθέτηση τα δεδομένα στο χώρο αποθήκευσης αντικειμένων blob του Azure.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Το αρχείο αρχειοθέτησης απαιτεί ένα λογαριασμό χώρου αποθήκευσης αναγνωριστικό πόρου, για να ενεργοποιήσετε την αρχειοθέτηση για να την επιθυμητή αποθήκευσης Azure.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Το κοντέινερ αντικειμένων blob όπου θέλετε τα δεδομένα σας συμβάν αρχειοθετηθούν.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Η έκδοση API του προτύπου.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Πόροι για την ανάπτυξη

Δημιουργεί ένα χώρο ονομάτων του τύπου **EventHubs**, με ένα διανομέα συμβάντων και σας δίνει τη δυνατότητα αρχειοθέτησης.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Εντολές για να εκτελέσετε ανάπτυξης

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure]: ../resource-group-authoring-templates.md
[Γρήγορη έναρξη Azure προτύπων]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure πόρους συμβάσεις ονοματοδοσίας]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Συμβάν διανομέα και ενεργοποιήσετε πρότυπο αρχειοθέτησης]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive

<properties
   pageTitle="Συνδεδεμένες πρότυπα με τη διαχείριση πόρων | Microsoft Azure"
   description="Περιγράφει τον τρόπο χρήσης προτύπων συνδεδεμένα σε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε μια λύση λειτουργική πρότυπο. Δείχνει πώς μπορείτε να μεταβιβάζουν τιμές παραμέτρων, καθορίστε ένα αρχείο παραμέτρων και διευθύνσεις URL που έχουν δημιουργηθεί δυναμικά."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Χρήση συνδεδεμένων προτύπων με τη διαχείριση πόρων Azure

Από μέσα σε ένα πρότυπο από διαχειριστή πόρων Azure, μπορείτε να συνδέσετε σε άλλο πρότυπο, το οποίο σας δίνει τη δυνατότητα για την ανάλυση του ανάπτυξη σε ένα σύνολο στοχευμένες, πρότυπα για έναν συγκεκριμένο σκοπό. Όπως με decomposing μια εφαρμογή σε διάφορες κατηγορίες κώδικα, αποδόμησης παρέχει πλεονεκτήματα όσον αφορά δοκιμές επαναχρησιμοποίηση και αναγνωσιμότητας.  

Μπορείτε να περάσετε παραμέτρους από ένα κύριο πρότυπο σε ένα συνδεδεμένο πρότυπο και οι παράμετροι να αντιστοιχίσετε απευθείας σε παραμέτρους ή τις μεταβλητές που εκτίθεται από το πρότυπο κλήσης. Το πρότυπο συνδεδεμένων μπορεί να περάσει επίσης μια μεταβλητή εξόδου στο πρότυπο προέλευσης, ενεργοποιώντας μια ανταλλαγή αμφίδρομη δεδομένων μεταξύ προτύπων.

## <a name="linking-to-a-template"></a>Σύνδεση με ένα πρότυπο

Μπορείτε να δημιουργήσετε μια σύνδεση ανάμεσα σε δύο πρότυπα με την προσθήκη ενός πόρου ανάπτυξη μέσα στο κύριο πρότυπο που οδηγεί στο συνδεδεμένο πρότυπο. Μπορείτε να ορίσετε την ιδιότητα **templateLink** για να το URI του συνδεδεμένου προτύπου. Μπορείτε να παράσχετε τιμές παραμέτρων για το συνδεδεμένο πρότυπο, καθορίζοντας τις τιμές απευθείας στο πρότυπό σας ή με τη σύνδεση σε ένα αρχείο παραμέτρων. Το παρακάτω παράδειγμα χρησιμοποιεί την ιδιότητα **παραμέτρους** για να καθορίσετε μια τιμή παραμέτρου απευθείας.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Η υπηρεσία διαχείρισης πόρων πρέπει να είναι μπορούν να έχουν πρόσβαση στο συνδεδεμένο πρότυπο. Δεν μπορείτε να καθορίσετε ένα τοπικό αρχείο ή ένα αρχείο που είναι διαθέσιμη μόνο σε τοπικού δικτύου σας για το συνδεδεμένο πρότυπο. Μπορείτε μόνο να δώσετε μια τιμή URI που περιλαμβάνει **http** ή **https**. Μία επιλογή είναι να τοποθετήσετε το πρότυπό σας συνδεδεμένο σε ένα λογαριασμό του χώρου αποθήκευσης και να χρησιμοποιήσετε το URI για αυτό το στοιχείο, όπως φαίνεται στο παρακάτω παράδειγμα.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Παρόλο που το συνδεδεμένο πρότυπο πρέπει να είναι διαθέσιμες εξωτερικά, δεν χρειάζεται να είναι γενικά διαθέσιμο στο κοινό. Μπορείτε να προσθέσετε το πρότυπό σας σε ένα λογαριασμό ιδιωτικού χώρου αποθήκευσης που θα είναι διαθέσιμα μόνο ο κάτοχος λογαριασμού χώρου αποθήκευσης. Στη συνέχεια, μπορείτε να δημιουργήσετε ένα διακριτικό υπογραφής (συσχετισμών Ασφαλείας) κοινόχρηστη πρόσβαση για να ενεργοποιήσετε την πρόσβαση κατά την ανάπτυξη. Μπορείτε να προσθέσετε αυτό το διακριτικό συσχετισμών Ασφαλείας για το URI για το συνδεδεμένο πρότυπο. Για τα βήματα για τη ρύθμιση ενός προτύπου σε ένα λογαριασμό του χώρου αποθήκευσης και δημιουργία ενός διακριτικού συσχετισμών Ασφαλείας, ανατρέξτε στο θέμα [Ανάπτυξη πόρους με πρότυπα διαχείρισης πόρων και Azure PowerShell](resource-group-template-deploy.md) ή [Ανάπτυξη πόρους με πρότυπα διαχείρισης πόρων και Azure CLI](resource-group-template-deploy-cli.md). 

Το παρακάτω παράδειγμα εμφανίζει ένα γονικό πρότυπο που συνδέεται με ένα άλλο πρότυπο. Το συνδεδεμένο πρότυπο είναι δυνατή η πρόσβαση με διακριτικό συσχετισμών Ασφαλείας που μεταβιβάζεται στο ως παράμετρο.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Παρόλο που το διακριτικό μεταβιβάζεται στο ως ασφαλή συμβολοσειρά, το URI του συνδεδεμένου προτύπου, όπως το διακριτικό συσχετισμών Ασφαλείας, έχει συνδεθεί σε τις λειτουργίες ανάπτυξης για αυτήν την ομάδα πόρων. Για να περιορίσετε έκθεσης, ορίστε μια ημερομηνία λήξης για το διακριτικό.

## <a name="linking-to-a-parameter-file"></a>Σύνδεση σε ένα αρχείο παραμέτρων

Το επόμενο παράδειγμα χρησιμοποιεί την ιδιότητα **parametersLink** για να συνδέσετε ένα αρχείο παραμέτρων.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Η τιμή URI για το αρχείο συνδεδεμένων παραμέτρου δεν μπορεί να είναι ένα τοπικό αρχείο και πρέπει να περιλαμβάνει **http** ή **https**. Το αρχείο παραμέτρων μπορεί επίσης να περιορίζεται σε πρόσβαση μέσω ενός διακριτικού συσχετισμών Ασφαλείας.

## <a name="using-variables-to-link-templates"></a>Χρήση μεταβλητών για σύνδεση πρότυπα

Το προηγούμενο παράδειγμα εμφάνιζε σχεδιασμένου τιμές διεύθυνσης URL για τις συνδέσεις του προτύπου. Αυτή η προσέγγιση μπορεί να λειτουργήσει για ένα απλό πρότυπο αλλά δεν λειτουργεί σωστά όταν εργάζεστε με ένα μεγάλο σύνολο λειτουργική προτύπων. Αντί για αυτό, μπορείτε να δημιουργήσετε μια στατική μεταβλητή που αποθηκεύει μια βασική διεύθυνση URL για το κύριο πρότυπο και, στη συνέχεια, να δημιουργήσετε δυναμικά διευθύνσεις URL για το συνδεδεμένο πρότυπα από αυτήν τη βασική διεύθυνση URL. Το πλεονέκτημα αυτής της προσέγγισης είναι μπορείτε εύκολα να μετακινήσετε ή να διαχωρίζονται του προτύπου, επειδή χρειάζεται να αλλάξετε τη στατική μεταβλητή στο κύριο πρότυπο. Το κύριο πρότυπο μεταβιβάζει τη σωστή URIs σε όλο το πρότυπο αναλυμένη.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε μια βασική διεύθυνση URL για να δημιουργήσετε δύο διευθύνσεις URL για συνδεδεμένο πρότυπα (**sharedTemplateUrl** και **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Μπορείτε επίσης να χρησιμοποιήσετε [deployment()](resource-group-template-functions.md#deployment) για να λάβετε τη βασική διεύθυνση URL για το τρέχον πρότυπο, και να το χρησιμοποιήσετε για να λάβετε τη διεύθυνση URL για άλλα πρότυπα στην ίδια θέση. Αυτή η προσέγγιση είναι χρήσιμος εάν αλλάζει τη θέση του προτύπου (ίσως οφείλεται σε διαχείριση εκδόσεων) ή εάν θέλετε να αποφύγετε την οριστική κωδικοποίηση URL στο αρχείο του προτύπου. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Σύνδεση υπό όρους με πρότυπα

Μπορείτε να συνδέσετε με διαφορετικά πρότυπα περνώντας σε μια τιμή παραμέτρου που χρησιμοποιείται για να δημιουργήσετε το URI του συνδεδεμένου προτύπου. Αυτή η προσέγγιση είναι ιδανική όταν πρέπει να καθορίσετε κατά την ανάπτυξη, τα οποία συνδέεται το πρότυπο για να χρησιμοποιήσετε. Για παράδειγμα, μπορείτε να καθορίσετε ένα πρότυπο για να χρησιμοποιήσετε για έναν υπάρχοντα λογαριασμό του χώρου αποθήκευσης και άλλο πρότυπο για να χρησιμοποιήσετε για το νέο λογαριασμό του χώρου αποθήκευσης.

Το παρακάτω παράδειγμα εμφανίζει μια παράμετρο για ένα όνομα λογαριασμού χώρου αποθήκευσης και μια παράμετρο για να καθορίσετε αν ο λογαριασμός χώρου αποθήκευσης είναι νέο ή υπάρχον.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Μπορείτε να δημιουργήσετε μια μεταβλητή για το πρότυπο URI που περιλαμβάνει την τιμή της παραμέτρου νέο ή υπάρχον.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Μπορείτε να παράσχετε αυτήν την τιμή μεταβλητής για τον πόρο ανάπτυξης.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Το URI αναλύεται σε ένα πρότυπο με το όνομα **existingStorageAccount.json** ή **newStorageAccount.json**. Δημιουργία προτύπων για αυτές τις URIs.

Το παρακάτω παράδειγμα εμφανίζει το πρότυπο **existingStorageAccount.json** .

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Το επόμενο παράδειγμα δείχνει το πρότυπο **newStorageAccount.json** . Παρατηρήστε ότι όπως το χώρο αποθήκευσης στο υπάρχον πρότυπο λογαριασμού του αντικειμένου λογαριασμού χώρου αποθήκευσης είναι επιστρέφονται στο εξόδους του. Το κύριο πρότυπο λειτουργεί με δύο συνδεδεμένες πρότυπο.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Παράδειγμα ολοκλήρωσης

Το παρακάτω παράδειγμα πρότυπα εμφανίζουν μια απλοποιημένη διάταξη των συνδεδεμένων πρότυπα για την απεικόνιση πολλές από τις έννοιες σε αυτό το άρθρο. Προϋποθέτει τα πρότυπα που έχουν προστεθεί στο ίδιο κοντέινερ σε ένα λογαριασμό του χώρου αποθήκευσης με δημόσια πρόσβαση απενεργοποιημένη. Το πρότυπο συνδεδεμένων μεταβιβάζει μια τιμή στο κύριο πρότυπο στην ενότητα **εξόδους** .

Το αρχείο **parent.json** αποτελείται από:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Το αρχείο **helloworld.json** αποτελείται από:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
Σε PowerShell, μπορείτε να μεταφέρετε ένα διακριτικό για το κοντέινερ και ανάπτυξη των προτύπων με:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Στο Azure CLI, μπορείτε να μεταφέρετε ένα διακριτικό για το κοντέινερ και ανάπτυξη των προτύπων με τον ακόλουθο κώδικα. Προς το παρόν, πρέπει να δώσετε ένα όνομα για την ανάπτυξη κατά τη χρήση ενός προτύπου URI που περιλαμβάνει ένα διακριτικό συσχετισμών Ασφαλείας.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Θα σας ζητηθεί να δώσετε το διακριτικό συσχετισμών Ασφαλείας ως παράμετρο. Πρέπει να εισαγωγή το διακριτικό με **?**.

## <a name="next-steps"></a>Επόμενα βήματα
- Για να μάθετε σχετικά με τη σειρά ανάπτυξης για τους πόρους σας τον ορισμό, ανατρέξτε στο θέμα [Ορισμός εξαρτήσεων στα πρότυπα διαχείρισης πόρων Azure](resource-group-define-dependencies.md)
- Για να μάθετε πώς μπορείτε να ορίσετε έναν πόρο αλλά να δημιουργήσετε πολλές παρουσίες του, ανατρέξτε στο θέμα [Δημιουργία πολλών παρουσιών των πόρων διαχείρισης πόρων στο Azure](resource-group-create-multiple.md)

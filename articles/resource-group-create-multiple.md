<properties
   pageTitle="Ανάπτυξη πολλές παρουσίες των πόρων | Microsoft Azure"
   description="Χρήση λειτουργίας αντιγραφής και τους πίνακες σε ένα πρότυπο από διαχειριστή πόρων Azure για να επαναλάβετε πολλές φορές κατά την ανάπτυξη πόρους."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Δημιουργία πολλών παρουσιών των πόρων διαχείρισης πόρων στο Azure

Αυτό το θέμα δείχνει πώς να επαναλάβετε στο πρότυπό σας από διαχειριστή πόρων Azure για να δημιουργήσετε πολλές παρουσίες ενός πόρου.

## <a name="copy-copyindex-and-length"></a>Αντιγραφή, copyIndex και μήκους

Μέσα στον πόρο για να δημιουργήσετε πολλές φορές, μπορείτε να ορίσετε ένα αντικείμενο **αντίγραφο** που καθορίζει τον αριθμό των επαναλήψεων διαδοχικές προσεγγίσεις. Το αντίγραφο που λαμβάνει την εξής μορφή:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Μπορείτε να αποκτήσετε πρόσβαση την τρέχουσα τιμή διαδοχικών προσεγγίσεων, η συνάρτηση **copyIndex()** , όπως φαίνεται παρακάτω εντός η συνάρτηση concat.

    [concat('examplecopy-', copyIndex())]

Κατά τη δημιουργία πολλούς πόρους από έναν πίνακα τιμών, μπορείτε να χρησιμοποιήσετε τη συνάρτηση **μήκος** για να καθορίσετε το πλήθος. Μπορείτε να δώσετε έναν πίνακα ως η παράμετρος στη συνάρτηση μήκος.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Χρησιμοποιήστε ευρετηρίου τιμή στο πεδίο όνομα

Μπορείτε να χρησιμοποιήσετε το αντίγραφο λειτουργία Δημιουργία πολλών παρουσιών ενός πόρου που έχουν μοναδικά ονόματα με βάση το βηματικής ευρετήριο. Για παράδειγμα, μπορεί να θέλετε να προσθέσετε έναν μοναδικό κωδικό στο τέλος της κάθε όνομα πόρου που έχει αναπτυχθεί. Για να αναπτύξετε τρεις τοποθεσίες web με το όνομα:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

Χρήση του παρακάτω προτύπου:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Τιμή offset ευρετηρίου

Θα παρατηρήσετε στο προηγούμενο παράδειγμα που η τιμή ευρετηρίου εκτείνεται από το μηδέν σε 2. Για να μετατοπίσετε την τιμή ευρετηρίου, τότε μπορείτε να καταχωρήσετε μια τιμή στη συνάρτηση **copyIndex()** , όπως **copyIndex(1)**. Ο αριθμός των διαδοχικών προσεγγίσεων για την εκτέλεση εξακολουθεί να έχει καθοριστεί στο στοιχείο αντιγραφή, αλλά η τιμή του copyIndex μετατόπισης με την καθορισμένη τιμή. Επομένως, χρησιμοποιώντας το ίδιο πρότυπο με το προηγούμενο παράδειγμα, αλλά καθορίζοντας **copyIndex(1)** θα αναπτύξετε τρεις τοποθεσίες web με το όνομα:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Χρησιμοποιήστε τη δυνατότητα αντιγραφής με πίνακα
   
Η λειτουργία αντιγραφής είναι ιδιαίτερα χρήσιμα κατά την εργασία με πίνακες, επειδή μπορείτε να επαναλάβετε σε κάθε στοιχείο του πίνακα. Για να αναπτύξετε τρεις τοποθεσίες web με το όνομα:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Χρήση του παρακάτω προτύπου:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Φυσικά, μπορείτε να ορίσετε το πλήθος αντίγραφο σε μια τιμή εκτός από το μήκος του πίνακα. Για παράδειγμα, που θα μπορούσε να δημιουργήσετε έναν πίνακα με πολλές τιμές, και, στη συνέχεια, στο μια τιμή παραμέτρου που καθορίζει πόσες στοιχεία του πίνακα για την ανάπτυξη. Σε αυτή την περίπτωση, ορίστε το πλήθος αντίγραφο όπως φαίνεται στο πρώτο παράδειγμα. 

## <a name="depending-on-resources-in-a-loop"></a>Ανάλογα με τους πόρους σε επανάληψη

Μπορείτε να καθορίσετε ότι ένας πόρος να αναπτυχθούν μετά από έναν άλλο πόρο, χρησιμοποιώντας το στοιχείο **dependsOn** . Όταν χρειάζεστε για την ανάπτυξη ενός πόρου που εξαρτάται από τη συλλογή των πόρων σε επανάληψη, μπορείτε να χρησιμοποιήσετε παρέχετε το όνομα του βρόχου αντίγραφο στο στοιχείο **dependsOn** . Το παρακάτω παράδειγμα εμφανίζει τον τρόπο ανάπτυξης 3 λογαριασμούς χώρου αποθήκευσης πριν από την ανάπτυξη η εικονική μηχανή. Το πλήρες ορισμού εικονική μηχανή δεν εμφανίζεται. Παρατηρήστε ότι το στοιχείο αντιγραφή έχει οριστεί σε **storagecopy** **όνομα** και το στοιχείο **dependsOn** για τις εικονικές μηχανές επίσης έχει οριστεί σε **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Επανάληψη σε έναν πόρο ένθετων

Δεν μπορείτε να χρησιμοποιήσετε ένα βρόχο αντίγραφο για έναν πόρο ένθετη. Εάν χρειάζεστε για να δημιουργήσετε πολλές παρουσίες ενός πόρου που έχετε καθορίσει συνήθως ως ένθετο μέσα σε άλλον πόρο, που πρέπει να Αντίθετα δημιουργήσετε τον πόρο ως πόρο ανώτατου επιπέδου και ορίζουν τη σχέση με τον πόρο γονικό μέσω των ιδιοτήτων **τύπο** και το **όνομα** .

Για παράδειγμα, ας υποθέσουμε ότι μπορείτε να καθορίσετε ένα σύνολο δεδομένων ως πόρο ένθετο μέσα σε μια προέλευση δεδομένων.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Για να δημιουργήσετε πολλές παρουσίες των συνόλων δεδομένων, θα πρέπει να αλλάξετε το πρότυπό σας, όπως φαίνεται παρακάτω. Πληκτρολογήστε το πλήρως προσδιορισμένο ειδοποίηση και το όνομα περιλαμβάνει το όνομα εργοστασίου δεδομένων.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Δημιουργία πολλών παρουσιών όταν το αντίγραφο δεν λειτουργεί

Μπορείτε να χρησιμοποιήσετε **αντίγραφο** μόνο σε τύπους πόρων και όχι στις ιδιότητες μέσα σε έναν τύπο πόρου. Αυτό μπορεί να δημιουργήσει προβλήματα για εσάς όταν θέλετε να δημιουργήσετε πολλές παρουσίες κάτι που είναι μέρος ενός πόρου. Ένα συνηθισμένο σενάριο είναι να δημιουργήσετε πολλές δίσκων δεδομένων για μια εικονική μηχανή. Δεν μπορείτε να χρησιμοποιήσετε **αντιγράφου** με τα δεδομένα δίσκων επειδή **dataDisks** είναι μια ιδιότητα στον υπολογιστή εικονικές, δεν δικό του τύπος πόρου. Αντί για αυτό, μπορείτε να δημιουργήσετε έναν πίνακα με όσα δίσκων δεδομένων θα χρειάζεστε, και να μεταβιβάσετε τον πραγματικό αριθμό δίσκων δεδομένων για να δημιουργήσετε. Στον ορισμό εικονική μηχανή, μπορείτε να χρησιμοποιήσετε τη συνάρτηση **λήψη** για να λάβετε μόνο τον αριθμό των στοιχείων που θέλετε στην πραγματικότητα από έναν πίνακα.

Ένα παράδειγμα πλήρους αυτό το μοτίβο είναι εμφάνιση στο πρότυπο [δημιουργήσετε μια Εικονική με μια δυναμική επιλογή δίσκων δεδομένων](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) .

Στις σχετικές ενότητες του προτύπου ανάπτυξης εμφανίζονται παρακάτω. Πολλές το πρότυπο έχει καταργηθεί για να επισημάνετε τις ενότητες που εμπλέκονται στη δυναμικά τη δημιουργία ενός αριθμού δίσκων δεδομένων. Παρατηρήστε ότι η παράμετρος **numDataDisks** που σας επιτρέπει να μεταβιβάσετε τον αριθμό των δίσκων για να δημιουργήσετε. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Επόμενα βήματα
- Εάν θέλετε να μάθετε περισσότερα σχετικά με τις ενότητες ενός προτύπου, ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](./resource-group-authoring-templates.md).
- Για όλες τις συναρτήσεις που μπορείτε να χρησιμοποιήσετε σε ένα πρότυπο, ανατρέξτε στο θέμα [Συναρτήσεις προτύπου για τη διαχείριση πόρων Azure](./resource-group-template-functions.md).
- Για να μάθετε πώς μπορείτε να αναπτύξετε το πρότυπό σας, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).

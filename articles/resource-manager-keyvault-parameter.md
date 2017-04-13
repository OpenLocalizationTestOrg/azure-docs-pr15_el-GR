<properties
   pageTitle="Μυστικό θάλαμο πλήκτρο με το πρότυπο διαχείρισης πόρων | Microsoft Azure"
   description="Δείχνει πώς μπορείτε να μεταβιβάσετε έναν μυστικό από ένα πλήκτρο θάλαμο ως παράμετρο, κατά την ανάπτυξη."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Η φάση ασφαλούς τιμών κατά τη διάρκεια της ανάπτυξης

Όταν χρειάζεστε για τη μεταβίβαση ασφαλούς τιμή (όπως έναν κωδικό πρόσβασης) ως παράμετρο, κατά την ανάπτυξη, μπορείτε να αποθηκεύσετε αυτήν την τιμή ως ένα μυστικό σε ένα [Θάλαμο κλειδί Azure](./key-vault/key-vault-whatis.md) και να αναφέρονται την τιμή σε άλλα πρότυπα διαχείρισης πόρων. Συμπεριλάβετε μόνο μια αναφορά σε το μυστικό στο πρότυπό σας, ώστε το μυστικό δεν εκτίθεται ποτέ και δεν χρειάζεται να με μη αυτόματο τρόπο εισαγάγετε την τιμή για το μυστικό κάθε φορά που μπορείτε να αναπτύξετε τους πόρους. Μπορείτε να καθορίσετε ποιοι χρήστες ή υπηρεσία αρχές να αποκτήσετε πρόσβαση σε το μυστικό.  

## <a name="deploy-a-key-vault-and-secret"></a>Ανάπτυξη ενός κλειδιού θάλαμο και μυστικό

Για να δημιουργήσετε κλειδιού θάλαμο που μπορούν να χρησιμοποιηθούν από άλλα πρότυπα για τη διαχείριση πόρων, πρέπει να μπορείτε να ορίσετε την ιδιότητα **enabledForTemplateDeployment** στην **τιμή true**και πρέπει να εκχωρήσετε πρόσβαση ο χρήστης ή η υπηρεσία κεφάλαιο που θα εκτελεστεί ανάπτυξης που αναφέρεται το μυστικό.

Για να μάθετε σχετικά με την ανάπτυξη ενός κλειδιού θάλαμο και μυστικό, ανατρέξτε στο θέμα [σχήμα θάλαμο κλειδί](resource-manager-template-keyvault.md) και [αριθμού-κλειδιού θάλαμο μυστικού σχήματος](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Αναφορά ένα μυστικό με στατική αναγνωριστικό

Μπορείτε να αναφέρετε το μυστικό από μέσα σε ένα αρχείο παραμέτρων που μεταφέρει τιμές στο πρότυπό σας. Μπορείτε να αναφέρετε το μυστικό, μεταφέροντας το αναγνωριστικό πόρου του κλειδιού θάλαμο και το όνομα του το μυστικό. Σε αυτό το παράδειγμα, πρέπει να υπάρχει ήδη το μυστικό κλειδιού θάλαμο και χρησιμοποιείτε μια στατική τιμή για το αναγνωριστικό πόρου.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Ένα αρχείο ολόκληρη η παράμετρος μπορεί να μοιάζει με:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Η παράμετρος που δέχεται το μυστικό πρέπει να είναι ένα **securestring**. Το παρακάτω παράδειγμα εμφανίζει στις σχετικές ενότητες ενός προτύπου που αναπτύσσει το SQL server που απαιτεί έναν κωδικό πρόσβασης διαχειριστή.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Αναφορά ένα μυστικό με δυναμική αναγνωριστικό

Στην προηγούμενη ενότητα εμφάνιζε τρόπος για τη μεταβίβαση ένα αναγνωριστικό στατική πόρων για το μυστικό κλειδιού θάλαμο. Ωστόσο, σε ορισμένα σενάρια, πρέπει να αναφέρονται σε μια μυστικό κλειδιού θάλαμο που διαφέρει ανάλογα με την τρέχουσα ανάπτυξη. Σε αυτή την περίπτωση, δεν μπορείτε να καθορίσετε από τον προγραμματισμό το αναγνωριστικό πόρου στο αρχείο παραμέτρων. Δυστυχώς, δεν μπορείτε δυναμικά να δημιουργήσετε το αναγνωριστικό πόρου στο αρχείο παραμέτρων επειδή δεν επιτρέπονται εκφράσεις προτύπων στο αρχείο παραμέτρων.

Για να υποδείξουν το αναγνωριστικό πόρου για ένα μυστικό κλειδιού θάλαμο, πρέπει να μετακινήσετε τον πόρο που χρειάζεται το μυστικό σε ένα ένθετο πρότυπο. Στο κύριο πρότυπο σας, προσθέτετε ένθετο πρότυπο και εισάγετε μια παράμετρο που περιέχει το αναγνωριστικό που δημιουργήθηκε δυναμικά πόρων.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Επόμενα βήματα

- Για γενικές πληροφορίες σχετικά με τις βασικές χώροι φύλαξης, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](./key-vault/key-vault-get-started.md).
- Για πληροφορίες σχετικά με τη χρήση ενός κλειδιού θάλαμο με μια εικονική μηχανή, ανατρέξτε στο θέμα [ζητήματα ασφαλείας για τη διαχείριση πόρων Azure](best-practices-resource-manager-security.md).
- Για αναφορά κλειδιού απόρρητο ολοκλήρωσης παραδείγματα, δείτε [παραδείγματα θάλαμο αριθμού-κλειδιού](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).


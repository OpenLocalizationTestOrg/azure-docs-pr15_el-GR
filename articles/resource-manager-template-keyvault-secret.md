<properties
   pageTitle="Διαχείριση πόρων προτύπου για ένα μυστικό σε ένα πλήκτρο θάλαμο | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη του κλειδιού θάλαμο απόρρητο μέσω του προτύπου."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Διάταξη κλειδιού θάλαμο μυστικού προτύπου

Δημιουργεί ένα μυστικό που είναι αποθηκευμένο σε ένα πλήκτρο θάλαμο. Αυτός ο τύπος πόρου έχει αναπτυχθεί συχνά ως πόρο θυγατρικό του [κλειδιού θάλαμο](resource-manager-template-keyvault.md).

## <a name="schema-format"></a>Μορφοποίηση σχήματος

Για να δημιουργήσετε ένα πλήκτρο θάλαμο μυστικό, προσθέστε την ακόλουθη διάταξη στο πρότυπό σας. Το μυστικό μπορεί να οριστεί ως είτε ένα θυγατρικό πόρων ενός κλειδιού θάλαμο ή ως ανώτατου επιπέδου πόρου. Μπορείτε να ορίσετε το ως θυγατρικό πόρου, όταν έχει αναπτυχθεί το πλήκτρο θάλαμο στο ίδιο πρότυπο. Θα πρέπει να ορίσετε το μυστικό ως ανώτατου επιπέδου πόρου, όταν το πλήκτρο θάλαμο δεν έχει αναπτυχθεί στο ίδιο πρότυπο ή όταν χρειάζεστε για να δημιουργήσετε πολλές απόρρητο με επανάληψη τον τύπο πόρου. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Τιμή |
| ---- | ---- | 
| Τύπος | Απαρίθμηση<br />Απαιτείται<br />**απόρρητο** (όταν αναπτυχθεί ως πόρο θυγατρικό του κλειδιού θάλαμο) ή<br /> **Microsoft.KeyVault/vaults/secrets** (όταν αναπτυχθεί ως ανώτατου επιπέδου πόρου)<br /><br />Ο τύπος πόρου για να δημιουργήσετε. |
| apiVersion | Απαρίθμηση<br />Απαιτείται<br />**2015-06-01** ή **2014-12-19-preview**<br /><br />Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου. | 
| Όνομα | Συμβολοσειρά<br />Απαιτείται<br />Μία λέξη όταν αναπτυχθεί ως πόρο θυγατρικό ενός κλειδιού θάλαμο ή με τη μορφή **{κλειδί-θάλαμο-name} / {μυστικό-όνομα}** όταν αναπτυχθεί ως ανώτατου επιπέδου πόρου να προστεθούν σε μια υπάρχουσα κλειδιού θάλαμο.<br /><br />Το όνομα του το μυστικό για να δημιουργήσετε. |
| Ιδιότητες | Αντικείμενο<br />Απαιτείται<br />[Ιδιότητες αντικειμένου](#properties)<br /><br />Αντικείμενο που καθορίζει την τιμή του το μυστικό για να δημιουργήσετε. |
| dependsOn | Πίνακα<br />Προαιρετικό<br />Μια λίστα διαχωρισμένες με κόμματα ενός πόρου ονομάτων ή των μοναδικών αναγνωριστικών πόρων.<br /><br />Η συλλογή των πόρων εξαρτάται από αυτήν τη σύνδεση. Εάν το πλήκτρο θάλαμο για το μυστικό έχει αναπτυχθεί στο ίδιο πρότυπο, συμπεριλάβετε το όνομα του του κλειδιού θάλαμο σε αυτό το στοιχείο για να βεβαιωθείτε ότι έχει αναπτυχθεί πρώτα. |

<a id="properties" />
### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Τιμή |
| ---- | ---- | 
| τιμή | Συμβολοσειρά<br />Απαιτείται<br /><br />Το μυστικό τιμή για να αποθηκεύσετε σε του κλειδιού θάλαμο. Όταν που περνά μέσα σε μια τιμή για αυτήν την ιδιότητα, χρησιμοποιήστε μια παράμετρο του τύπου **securestring**.  |

    
## <a name="examples"></a>Παραδείγματα

Το πρώτο παράδειγμα αναπτύσσει έναν μυστικό ως θυγατρικό πόρο από ένα πλήκτρο θάλαμο.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Το δεύτερο παράδειγμα αναπτύσσει το μυστικό ως ανώτατου επιπέδου πόρου που είναι αποθηκευμένο σε μια υπάρχουσα κλειδιού θάλαμο.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Επόμενα βήματα

- Για γενικές πληροφορίες σχετικά με τις βασικές χώροι φύλαξης, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](./key-vault/key-vault-get-started.md).
- Για παράδειγμα αναφορά σε ένα μυστικό κλειδιού θάλαμο κατά την ανάπτυξη προτύπων, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά την ανάπτυξη](resource-manager-keyvault-parameter.md).



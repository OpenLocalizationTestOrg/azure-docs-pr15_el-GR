<properties
   pageTitle="Πρότυπο διαχείρισης πόρων για κλειδιού θάλαμο | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη του κλειδιού χώροι φύλαξης μέσω του προτύπου."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Διάταξη κλειδιού θάλαμο προτύπου

Δημιουργεί ένα πλήκτρο θάλαμο.

## <a name="schema-format"></a>Μορφοποίηση σχήματος

Για να δημιουργήσετε ένα πλήκτρο θάλαμο, προσθέστε την ακόλουθη διάταξη στην ενότητα πόροι του προτύπου σας.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Τιμή |
| ---- | ---- | 
| Τύπος | Απαρίθμηση<br />Απαιτείται<br />**Microsoft.KeyVault/vaults**<br /><br />Ο τύπος πόρου για να δημιουργήσετε. |
| apiVersion | Απαρίθμηση<br />Απαιτείται<br />**2015-06-01** ή **2014-12-19-preview**<br /><br />Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου. | 
| Όνομα | Συμβολοσειρά<br />Απαιτείται<br />Ένα όνομα που είναι μοναδικό σε Azure.<br /><br />Το όνομα του κλειδιού θάλαμο για να δημιουργήσετε. Μπορείτε να χρησιμοποιήσετε τη συνάρτηση [uniqueString](resource-group-template-functions.md#uniquestring) με σας κανόνες ονοματοθεσίας για να δημιουργήσετε ένα μοναδικό όνομα, όπως φαίνεται στο παρακάτω παράδειγμα. |
| θέση | Συμβολοσειρά<br />Απαιτείται<br />Μια έγκυρη περιοχή για κλειδιού χώροι φύλαξης. Για να προσδιορίσετε έγκυρη περιοχές, ανατρέξτε στο θέμα [υποστηρίζονται περιοχές](resource-manager-supported-services.md#supported-regions).<br /><br />Η περιοχή για τη φιλοξενία του κλειδιού θάλαμο. |
| Ιδιότητες | Αντικείμενο<br />Απαιτείται<br />[Ιδιότητες αντικειμένου](#properties)<br /><br />Αντικείμενο που καθορίζει τον τύπο του κλειδιού θάλαμο για να δημιουργήσετε. |
| πόροι | Πίνακα<br />Προαιρετικό<br />Επιτρέπεται τιμές: [πλήκτρο θάλαμο μυστικού πόροι](resource-manager-template-keyvault-secret.md)<br /><br />Θυγατρικό πόροι για το πλήκτρο θάλαμο. |

<a id="properties" />
### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Τιμή |
| ---- | ---- | 
| enabledForDeployment | Δυαδική τιμή<br />Προαιρετικό<br />**True** ή **false**<br /><br />Καθορίζει αν είναι ενεργοποιημένη η θάλαμο για ανάπτυξη εικονικό μηχάνημα ή ύφασμα υπηρεσίας. |
| enabledForTemplateDeployment | Δυαδική τιμή<br />Προαιρετικό<br />**True** ή **false**<br /><br />Καθορίζει αν είναι ενεργοποιημένη η θάλαμο για χρήση σε ανάπτυξη προτύπου για τη διαχείριση πόρων σε. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά τη διάρκεια της ανάπτυξης](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Δυαδική τιμή<br />Προαιρετικό<br />**True** ή **false**<br /><br />Καθορίζει αν είναι ενεργοποιημένη η θάλαμο για κρυπτογράφηση τόμου. |
| tenantId | Συμβολοσειρά<br />Απαιτείται<br />**Καθολικά μοναδικό αναγνωριστικό**<br /><br />Το αναγνωριστικό μισθωτή για τη συνδρομή. Μπορείτε να την ανακτήσετε με το cmdlet του PowerShell [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) ή την εντολή " **Εμφάνιση λογαριασμός azure** Azure CLI". |
| accessPolicies | Πίνακα<br />Απαιτείται<br />[αντικείμενο accessPolicies](#accesspolicies)<br /><br />Ένας πίνακας έως 16 αντικείμενα που καθορίζουν τα δικαιώματα για το χρήστη ή την υπηρεσία κεφάλαιο. |
| SKU | Αντικείμενο<br />Απαιτείται<br />[SKU του αντικειμένου](#sku)<br /><br />Το SKU για το πλήκτρο θάλαμο. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>αντικείμενο properties.accessPolicies

| Όνομα | Τιμή |
| ---- | ---- | 
| tenantId | Συμβολοσειρά<br />Απαιτείται<br />**Καθολικά μοναδικό αναγνωριστικό**<br /><br />Το αναγνωριστικό μισθωτή του μισθωτή του Azure Active Directory που περιέχει το **objectId** σε αυτή την πολιτική πρόσβασης |
| objectId | Συμβολοσειρά<br />Απαιτείται<br />**Καθολικά μοναδικό αναγνωριστικό**<br /><br />Το αναγνωριστικό αντικειμένου του Azure Active Directory χρήστη ή της υπηρεσίας κεφάλαιο που θα έχουν πρόσβαση στο το θάλαμο. Μπορείτε να ανακτήσετε την τιμή από τη [Λήψη AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) ή τα cmdlet [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShell ή τις εντολές Azure CLI **azure ad χρήστη** ή **sp azure ad** . |
| δικαιώματα | Αντικείμενο<br />Απαιτείται<br />[δικαιώματα αντικειμένου](#permissions)<br /><br />Τα δικαιώματα που έχουν εκχωρηθεί σε αυτήν θάλαμο στο αντικείμενο υπηρεσίας καταλόγου Active Directory. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>αντικείμενο properties.accessPolicies.permissions

| Όνομα | Τιμή |
| ---- | ---- | 
| πλήκτρα | Πίνακα<br />Απαιτείται<br />**όλες**, **αντιγράφου ασφαλείας**, **Δημιουργία**, **αποκρυπτογράφηση**, **Διαγραφή**, **κρυπτογράφηση**, **λήψη**, **Εισαγωγή**, **λίστα**, **επαναφέρετε**, **εισόδου**, **unwrapkey**, **Ενημέρωση**, **Επαλήθευση**, **wrapkey**<br /><br />Τα δικαιώματα που έχουν εκχωρηθεί σε κλειδιά σε αυτό θάλαμο σε αυτό το αντικείμενο υπηρεσίας καταλόγου Active Directory. Αυτή η τιμή πρέπει να έχει καθοριστεί ως ένας πίνακας με μία ή περισσότερες τιμές ρητά. |
| απόρρητο | Πίνακα<br />Απαιτείται<br />**όλες**, **Διαγραφή**, **λήψη**, **λίστα**, **Ορισμός**<br /><br />Τα δικαιώματα που έχουν εκχωρηθεί σε απόρρητο στο αυτό θάλαμο σε αυτό το αντικείμενο υπηρεσίας καταλόγου Active Directory. Αυτή η τιμή πρέπει να έχει καθοριστεί ως ένας πίνακας με μία ή περισσότερες τιμές ρητά. |

<a id="sku" />
### <a name="propertiessku-object"></a>αντικείμενο Properties.SKU

| Όνομα | Τιμή |
| ---- | ---- | 
| Όνομα | Απαρίθμηση<br />Απαιτείται<br />**Τυπική**ή **premium** <br /><br />Το επίπεδο υπηρεσίας του KeyVault για να χρησιμοποιήσετε.  Τυπική υποστηρίζει απορρήτου και κλειδιά που προστατεύεται με λογισμικό.  Premium προσθέτει υποστήριξη για προστασία HSM πλήκτρα. |
| οικογένεια | Απαρίθμηση<br />Απαιτείται<br />**A** <br /><br />Η οικογένεια sku για να χρησιμοποιήσετε. |
 
    
## <a name="examples"></a>Παραδείγματα

Το παρακάτω παράδειγμα αναπτύσσει ένα πλήκτρο θάλαμο και μυστικό.

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

## <a name="quickstart-templates"></a>Γρήγορη έναρξη προτύπων

Το ακόλουθο πρότυπο γρήγορη έναρξη ανάπτυξη ενός κλειδιού θάλαμο.

- [Δημιουργία βασικών θάλαμο](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Επόμενα βήματα

- Για γενικές πληροφορίες σχετικά με τις βασικές χώροι φύλαξης, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure κλειδί θάλαμο](./key-vault/key-vault-get-started.md).
- Για παράδειγμα αναφορά σε ένα μυστικό κλειδιού θάλαμο κατά την ανάπτυξη προτύπων, ανατρέξτε στο θέμα [μεταβιβάζουν ασφαλούς τιμές κατά την ανάπτυξη](resource-manager-keyvault-parameter.md).


<properties
   pageTitle="Διαχείριση πόρων προτύπου για εκχώρησης ρόλου | Microsoft Azure"
   description="Εμφανίζει το σχήμα από διαχειριστή πόρων για την ανάπτυξη μια εκχώρηση ρόλων μέσω του προτύπου."
   services="azure-resource-manager"
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Διάταξη προτύπου εκχωρήσεις ρόλων

Αντιστοιχίζει ένα χρήστη, ομάδας ή υπηρεσία κεφάλαιο ένα ρόλο σε ένα συγκεκριμένο εύρος.

## <a name="resource-format"></a>Μορφή πόρων

Για να δημιουργήσετε μια εκχώρηση ρόλων, προσθέστε την ακόλουθη διάταξη στην ενότητα πόροι του προτύπου σας.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Τιμές

Στους πίνακες που ακολουθούν περιγράφουν τις τιμές πρέπει να ορίσετε στο σχήμα.

| Όνομα | Απαιτείται | Περιγραφή |
| ---- | -------- | ----------- |
| Τύπος | Ναι    | Ο τύπος πόρου για να δημιουργήσετε.<br /><br /> Για την ομάδα πόρων:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Για τον πόρο:<br />**{υπηρεσία παροχής-χώρο ονομάτων} / {τύπος πόρου} / υπηρεσίες παροχής/roleAssignments** |
| apiVersion |Ναι | Η έκδοση API για να χρησιμοποιήσετε για τη δημιουργία του πόρου.<br /><br /> Χρησιμοποιήστε **2015-07-01**. | 
| Όνομα | Ναι | Ένα καθολικά μοναδικό αναγνωριστικό για τη νέα εκχώρηση ρόλου. |
| dependsOn | Όχι | Ένας πίνακας διαχωρισμένες με κόμματα ενός πόρου ονομάτων ή των μοναδικών αναγνωριστικών πόρων.<br /><br />Η συλλογή εξαρτάται από αυτήν την ανάθεση ρόλων πόρων. Εάν εκχώρηση ρόλου που στοχεύουν σε έναν πόρο και που έχει αναπτυχθεί πόρων στο ίδιο πρότυπο, συμπεριλάβετε το όνομα πόρου σε αυτό το στοιχείο για να βεβαιωθείτε ότι ο πόρος έχει αναπτυχθεί πρώτα. |
| Ιδιότητες | Ναι | Το αντικείμενο ιδιοτήτων που προσδιορίζει το ορισμού ρόλου κεφάλαιο και εύρος. |

### <a name="properties-object"></a>Ιδιότητες αντικειμένου

| Όνομα | Απαιτείται | Περιγραφή |
| ---- | -------- | ----------- |
| roleDefinitionId | Ναι |  Το αναγνωριστικό του ενός υπάρχοντος ορισμού ρόλου θα χρησιμοποιηθεί σε εκχώρησης ρόλου.<br /><br /> Χρησιμοποιήστε την παρακάτω μορφή:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Ναι | Καθολικά μοναδικό αναγνωριστικό για μια υπάρχουσα κεφάλαιο. Αυτή η τιμή αντιστοιχίζεται με το αναγνωριστικό μέσα στον κατάλογο και μπορεί να οδηγεί σε ένα χρήστη, υπηρεσία κεφάλαιο ή ομάδα ασφαλείας. |
| πεδίο εφαρμογής | Όχι | Το πεδίο στο οποίο ισχύει για αυτήν την ανάθεση ρόλου.<br /><br />Για τις ομάδες πόρων, χρησιμοποιήστε:<br />**/resourceGroups/ /Subscriptions/ {αναγνωριστικό συνδρομής} {--όνομα της ομάδας πόρων}**  <br /><br />Για τους πόρους, χρησιμοποιήστε:<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Πώς μπορείτε να χρησιμοποιήσετε τον πόρο ανάθεσης ρόλων

Προσθέστε μια εκχώρηση ρόλων στο πρότυπό σας όταν πρέπει να προσθέσετε ένα χρήστη, ομάδας ή υπηρεσία κεφαλαίου σε ένα ρόλο κατά την ανάπτυξη. Εκχωρήσεις ρόλων έχουν μεταβιβαστεί από το ανώτερο επίπεδα εμβέλειας, επομένως εάν έχετε ήδη προσθέσει αρχής σε ένα ρόλο στο επίπεδο της συνδρομής, δεν χρειάζεται να αντιστοιχίσετε εκ νέου για την ομάδα πόρων ή τον πόρο.

Υπάρχουν πολλές τιμές αναγνωριστικού πρέπει να παρέχουν όταν εργάζεστε με εκχώρησης ρόλου. Μπορείτε να ανακτήσετε τις τιμές μέσω του PowerShell ή Azure CLI.

### <a name="powershell"></a>PowerShell

Το όνομα της εκχώρησης ρόλου απαιτεί ένα καθολικά μοναδικό αναγνωριστικό. Μπορείτε να δημιουργήσετε ένα νέο αναγνωριστικό για το **όνομα** με:

    $name = [System.Guid]::NewGuid().toString()

Μπορείτε να ανακτήσετε το αναγνωριστικό για ορισμό ρόλου με:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Μπορείτε να ανακτήσετε το αναγνωριστικό για το κεφάλαιο με μία από τις παρακάτω εντολές.

Για μια ομάδα με το όνομα **ελεγκτές**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Για ένα χρήστη με το όνομα **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Για μια υπηρεσία κεφάλαιο με το όνομα **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure CLI

Μπορείτε να ανακτήσετε το αναγνωριστικό για ορισμό ρόλου με:

    azure role show Reader --json | jq .[].Id -r

Μπορείτε να ανακτήσετε το αναγνωριστικό για το κεφάλαιο με μία από τις παρακάτω εντολές.

Για μια ομάδα με το όνομα **ελεγκτές**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Για ένα χρήστη με το όνομα **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Για μια υπηρεσία κεφάλαιο με το όνομα **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Παραδείγματα

Το ακόλουθο πρότυπο λαμβάνει ένα αναγνωριστικό για ένα ρόλο και ένα αναγνωριστικό για ένα χρήστη, ομάδας ή κεφάλαιο υπηρεσίας. Εκχωρεί το ρόλο σε επίπεδο ομάδας πόρων.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Το επόμενο πρότυπο δημιουργεί ένα λογαριασμό του χώρου αποθήκευσης και εκχωρεί ρόλο αναγνώστη με το λογαριασμό χώρου αποθήκευσης. Τα αναγνωριστικά για τις δύο ομάδες και ρόλο αναγνώστη έχει συμπεριληφθεί στο πρότυπο για να απλοποιήσετε ανάπτυξης. Αυτές οι τιμές θα μπορούσε να ανακτηθούν κατά το χρόνο ανάπτυξης μέσω στη δέσμη ενεργειών και που εισήχθησαν στο ως παραμέτρους.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Γρήγορη έναρξη προτύπων

Τα παρακάτω πρότυπα δείχνουν πώς μπορείτε να χρησιμοποιήσετε τον πόρο ανάθεσης ρόλων:

- [Ανάθεση ενσωματωμένη ρόλου σε ομάδα πόρων](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Εκχώρηση ρόλου ενσωματωμένη σε υπάρχουσα Εικονική](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Εκχώρηση ρόλου ενσωματωμένη σε πολλά υπάρχοντα ΣΠΣ](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Επόμενα βήματα

- Για πληροφορίες σχετικά με τη δομή του προτύπου, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
- Για περισσότερες πληροφορίες σχετικά με τον έλεγχο πρόσβασης βάσει ρόλων, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων Active Directory Azure](./active-directory/role-based-access-control-configure.md).

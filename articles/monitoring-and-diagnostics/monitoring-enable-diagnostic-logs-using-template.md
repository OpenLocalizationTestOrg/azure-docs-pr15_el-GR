<properties
    pageTitle="Αυτόματη ενεργοποίηση διαγνωστικών ρυθμίσεων χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων για να δημιουργήσετε διαγνωστικών ρυθμίσεις που θα σας επιτρέψει να ροή σας αρχεία καταγραφής διαγνωστικών με διανομείς συμβάν ή αποθηκεύει σε ένα λογαριασμό του χώρου αποθήκευσης."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Αυτόματη ενεργοποίηση διαγνωστικών ρυθμίσεων κατά τη δημιουργία πόρων χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων
Σε αυτό το άρθρο θα σας δείξουμε πώς μπορείτε να χρησιμοποιήσετε ένα [πρότυπο Azure από διαχειριστή πόρων](../resource-group-authoring-templates.md) για να ρυθμίσετε τις παραμέτρους διαγνωστικών σε έναν πόρο κατά τη δημιουργία. Αυτό σας επιτρέπει να ξεκινά αυτόματα η ροή σας αρχεία καταγραφής διαγνωστικών και μετρήσεις σε συμβάν διανομείς, αρχειοθέτησή τους σε ένα λογαριασμό του χώρου αποθήκευσης ή αποστολή στο αρχείο καταγραφής ανάλυσης όταν δημιουργείται ένας πόρος.

Η μέθοδος για την ενεργοποίηση αρχείων καταγραφής διαγνωστικών χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων εξαρτάται από τον τύπο του πόρου.

- **Μη υπολογισμού** πόρους (για παράδειγμα, τις ομάδες ασφαλείας δικτύου, λογικής εφαρμογών, αυτοματισμού) Χρησιμοποιήστε [διαγνωστικών ρυθμίσεων που περιγράφονται σε αυτό το άρθρο](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Υπολογισμός** Πόροι (WAD/LAD βάσει) Χρησιμοποιήστε το [αρχείο ρύθμισης παραμέτρων WAD/LAD που περιγράφονται σε αυτό το άρθρο](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md).

Σε αυτό το άρθρο θα σας περιγράφουν τον τρόπο ρύθμισης παραμέτρων Διαγνωστικά χρησιμοποιώντας οποιαδήποτε μέθοδο.

Τα βασικά βήματα είναι ως εξής:

1. Δημιουργήστε ένα πρότυπο ως αρχείο JSON που περιγράφει τον τρόπο για να δημιουργήσετε τον πόρο και ενεργοποίηση Διαγνωστικά.
2. [Ανάπτυξη του προτύπου με οποιαδήποτε μέθοδο ανάπτυξης](../resource-group-template-deploy.md).

Παρακάτω δώσετε ένα παράδειγμα του αρχείου προτύπου JSON χρειάζεται να δημιουργήσετε για μη υπολογισμού και τους πόρους υπολογισμού.

## <a name="non-compute-resource-template"></a>Πρότυπο μη υπολογισμού πόρων
Για τους πόρους μη υπολογισμού, θα πρέπει να κάνετε δύο πράγματα:

1. Προσθέστε παραμέτρους σε το blob παραμέτρους για το όνομα του λογαριασμού χώρου αποθήκευσης, υπηρεσία bus κανόνας ID ή/και Αναγνωριστικό χώρου εργασίας OMS καταγραφής ανάλυσης (ενεργοποίηση αρχειοθέτησης των αρχείων καταγραφής διαγνωστικών σε ένα λογαριασμό του χώρου αποθήκευσης, η ροή των αρχείων καταγραφής με διανομείς συμβάν ή/και αποστολή αρχείων καταγραφής στο αρχείο καταγραφής αναλυτικών στοιχείων).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Στον πίνακα πόροι του πόρου για την οποία θέλετε να ενεργοποιήσετε αρχεία καταγραφής διαγνωστικών, προσθέστε έναν πόρο του τύπου `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Το αντικείμενο blob ιδιότητες για τη ρύθμιση διαγνωστικών ακολουθεί [τη μορφή που περιγράφεται σε αυτό το άρθρο](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Ακολουθεί ένα παράδειγμα πλήρους που δημιουργεί μια ομάδα ασφαλείας δικτύου και ενεργοποιεί ροής για διανομείς συμβάν και την αποθήκευση σε ένα λογαριασμό του χώρου αποθήκευσης.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Τον υπολογισμό πρότυπο πόρων
Για να ενεργοποιήσετε τα Διαγνωστικά σε έναν πόρο υπολογισμού, για παράδειγμα ένα σύμπλεγμα εικονικό μηχάνημα ή ύφασμα υπηρεσία, πρέπει να:

1. Προσθέστε την επέκταση Διαγνωστικά Azure στον ορισμό Εικονική πόρων.
2. Καθορίστε ένα λογαριασμό ή/και το συμβάν διανομέα χώρου αποθήκευσης με την παράμετρο.
3. Προσθέστε τα περιεχόμενα του αρχείου WADCfg XML στην ιδιότητα XMLCfg, Απαλλαχτείτε από όλους τους χαρακτήρες XML σωστά.

> [AZURE.WARNING] Αυτό το τελευταίο βήμα μπορεί να είναι περίπλοκη για να λάβετε δεξιά. Για παράδειγμα που διαιρεί το σχήμα ρύθμισης παραμέτρων Διαγνωστικά σε μεταβλητές που είναι διαφυγής και μορφοποιηθεί σωστά, [ανατρέξτε σε αυτό το άρθρο](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) .

Είναι ολόκληρη τη διαδικασία, δείγματα, όπως περιγράφεται [σε αυτό το έγγραφο](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Επόμενα βήματα
- [Διαβάστε περισσότερα σχετικά με τα αρχεία καταγραφής διαγνωστικών Azure](./monitoring-overview-of-diagnostic-logs.md)
- [Ροή Azure αρχεία καταγραφής διαγνωστικών με διανομείς συμβάντος](./monitoring-stream-diagnostic-logs-to-event-hubs.md)



<properties
    pageTitle="Χρησιμοποιήστε πρότυπα για τη διαχείριση πόρων Azure στο θέμα Δημιουργία και ρύθμιση παραμέτρων ενός χώρου εργασίας ανάλυση καταγραφής | Microsoft Azure"
    description="Μπορείτε να χρησιμοποιήσετε πρότυπα διαχείρισης πόρων Azure για τη δημιουργία και ρύθμιση παραμέτρων χώρων εργασίας καταγραφής ανάλυσης."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="json"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="richrund"/>

# <a name="manage-log-analytics-using-azure-resource-manager-templates"></a>Διαχείριση αρχείου καταγραφής ανάλυσης με τη χρήση προτύπων από διαχειριστή πόρων Azure

Μπορείτε να χρησιμοποιήσετε [πρότυπα διαχείρισης πόρων Azure] (... / azure-resource-manager/resource-group-authoring-templates.md) για τη δημιουργία και ρύθμιση παραμέτρων χώρων εργασίας καταγραφής ανάλυσης. Παραδείγματα από τις εργασίες που μπορείτε να εκτελέσετε με τα πρότυπα περιλαμβάνουν τα εξής:

+ Δημιουργία χώρου εργασίας
+ Προσθήκη μιας λύσης
+ Δημιουργία αποθηκευμένες αναζητήσεις
+ Δημιουργία ομάδας υπολογιστή
+ Ενεργοποίηση της συλλογής των αρχείων καταγραφής των υπηρεσιών IIS από τους υπολογιστές με εγκαταστήσει τον παράγοντα Windows
+ Συλλογή μετρητών επιδόσεων από υπολογιστές Linux και Windows
+ Συλλογή συμβάντα από syslog Linux υπολογιστές 
+ Συλλογή συμβάντα από αρχεία καταγραφής συμβάντων των Windows
+ Συλλογή προσαρμοσμένα αρχεία καταγραφής συμβάντων
+ Προσθήκη τον παράγοντα καταγραφής αναλυτικών στοιχείων σε μια εικονική μηχανή Azure
+ Ρύθμιση παραμέτρων καταγραφής ανάλυσης για ευρετήριο δεδομένα που συλλέγονται χρησιμοποιώντας τα Διαγνωστικά του Azure


Σε αυτό το άρθρο παρέχει ένα πρότυπο δείγματα που απεικονίζουν ορισμένες από τις ρυθμίσεις παραμέτρων που μπορείτε να εκτελέσετε από τα πρότυπα.

## <a name="create-and-configure-a-log-analytics-workspace"></a>Δημιουργία και ρύθμιση παραμέτρων ενός χώρου εργασίας ανάλυσης αρχείου καταγραφής

Το παρακάτω δείγμα προτύπου απεικονίζει τον τρόπο:

1.  Δημιουργία χώρου εργασίας
2.  Προσθήκη λύσεις στο χώρο εργασίας
3.  Δημιουργία αποθηκευμένες αναζητήσεις
4.  Δημιουργία ομάδας υπολογιστή
5.  Ενεργοποίηση της συλλογής των αρχείων καταγραφής των υπηρεσιών IIS από τους υπολογιστές με εγκαταστήσει τον παράγοντα Windows
6.  Συλλογή μετρητές επιδόσεων λογικού δίσκου από υπολογιστές Linux (% Inodes χρησιμοποιούνται; Ελεύθερα MB; % Χρησιμοποιούνται χώρου. Μεταφορές δίσκου/δευτερόλεπτο; Διαβάζει δίσκου/sec; Εγγραφές δίσκου/δευτερόλεπτο)
7.  Συλλογή syslog συμβάντα από υπολογιστές Linux
8.  Συλλογή σφάλματος και προειδοποίησης συμβάντων από το αρχείο καταγραφής συμβάντων εφαρμογής από υπολογιστές με Windows
9.  Συλλογή διαθέσιμα MB μνήμης μετρητή επιδόσεων από υπολογιστές με Windows
10. Συλλογή ενός προσαρμοσμένου αρχείου καταγραφής 
11. Συλλογή των υπηρεσιών IIS αρχεία καταγραφής και τα αρχεία καταγραφής συμβάντων των Windows που εγγράφονται από Azure Διαγνωστικά σε ένα λογαριασμό του χώρου αποθήκευσης


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "workspaceName"
      }
    },
    "serviceTier": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "Service Tier: Free, Standard, or Premium"
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "East US",
        "West Europe",
        "Southeast Asia",
        "Australia Southeast"
      ]
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of the storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "The resource group name containing the storage account with Azure diagnostics output"
        }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-11-01-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "Name": "[parameters('serviceTier')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-11-01-preview",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Type:Event Source=ServiceFabricNodeBootstrapAgent | dedup Computer | measure count () by Computer",
            "Version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLog",
          "properties": {
            "customLogName": "sampleCustomLog1",
            "description": "test custom log datasources",
            "inputs": [
              {
                "location": {
                  "fileSystemLocations": {
                    "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                    "linuxFileTypeLogPaths": [ "/var/logs" ]
                  }
                },
                "recordDelimiter": {
                  "regexDelimiter": {
                    "pattern": "\\n",
                    "matchIndex": 0,
                    "matchIndexSpecified": true,
                    "numberedGroup": null
                  }
                }
              }
            ],
            "extractions": [
              {
                "extractionName": "TimeGenerated",
                "extractionType": "DateTime",
                "extractionProperties": {
                  "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                  }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLogCollection",
          "properties": {
            "state": "LinuxLogsEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceOutput": {
      "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-11-01-preview')]",
      "type": "object"
    }
  }
}

```
### <a name="deploying-the-sample-template"></a>Ανάπτυξη το δείγμα προτύπου

Για να αναπτύξετε το δείγμα προτύπου:

1. Αποθηκεύστε το δείγμα συνημμένα σε ένα αρχείο, για παράδειγμα`azuredeploy.json` 
2. Επεξεργασία του προτύπου να έχουν τη ρύθμιση παραμέτρων που θέλετε
3. Χρήση του PowerShell ή τη γραμμή εντολών για την ανάπτυξη του προτύπου

#### <a name="powershell"></a>PowerShell

`New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json`

#### <a name="command-line"></a>Γραμμή εντολών

```
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```


## <a name="example-resource-manager-templates"></a>Διαχείριση πόρων παράδειγμα προτύπων

Στη συλλογή προτύπων Azure γρήγορη έναρξη περιλαμβάνει πολλά πρότυπα για ανάλυση καταγραφής, συμπεριλαμβανομένων των:

+ [Ανάπτυξη μια εικονική μηχανή με Windows με την επέκταση Εικονική ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
+ [Ανάπτυξη μια εικονική μηχανή εκτελείται Linux με την επέκταση Εικονική ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
+ [Επαναφορά τοποθεσίας Azure οθόνη χρησιμοποιώντας έναν υπάρχοντα χώρο εργασίας ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
+ [Παρακολούθηση εφαρμογών Web Azure χρησιμοποιώντας έναν υπάρχοντα χώρο εργασίας ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
+ [Οθόνη SQL Azure χρησιμοποιώντας έναν υπάρχοντα χώρο εργασίας ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/101-sqlazure-oms-monitoring/)
+ [Ανάπτυξη ένα σύμπλεγμα υφάσματος υπηρεσίας και εποπτεία το με έναν υπάρχοντα χώρο εργασίας ανάλυσης αρχείου καταγραφής](https://azure.microsoft.com/documentation/templates/service-fabric-oms/)
+ [Ανάπτυξη ένα σύμπλεγμα ύφασμα υπηρεσίας και τη δημιουργία χώρου εργασίας καταγραφής ανάλυσης για την παρακολούθηση του](https://azure.microsoft.com/documentation/templates/service-fabric-vmss-oms/)


## <a name="next-steps"></a>Επόμενα βήματα

+ [Ανάπτυξη παράγοντες σε ΣΠΣ Azure με τη χρήση προτύπων από διαχειριστή πόρων](log-analytics-azure-vm-extension.md)




<properties
    pageTitle="Πρόγραμμα εκμάθησης: Δημιουργία μιας διοχέτευσης χρησιμοποιώντας το πρότυπο διαχείρισης πόρων | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, δημιουργείτε μια διοχέτευση Azure εργοστασίου δεδομένων με μια δραστηριότητα αντιγραφή με τη χρήση προτύπου για τη διαχείριση πόρων Azure."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Πρόγραμμα εκμάθησης: Δημιουργία μιας διοχέτευσης με αντιγραφή δραστηριότητα χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Αντιγραφή οδηγού](data-factory-copy-data-wizard-tutorial.md)
- [Πύλη του Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure προτύπου για τη διαχείριση πόρων](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Αυτό το πρόγραμμα εκμάθησης θα μάθετε πώς να δημιουργήσετε και να παρακολουθείτε μια εργοστασίου Azure δεδομένων με χρήση ενός προτύπου για τη διαχείριση πόρων Azure. Τη διαδικασία στο η προέλευση δεδομένων αντιγράφει δεδομένα από το χώρο αποθήκευσης Blob του Azure βάση δεδομένων SQL Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
- Μεταβείτε μέσω του [προγράμματος εκμάθησης επισκόπηση και τις προϋποθέσεις](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) και ολοκληρώστε τα βήματα **προϋπόθεση** .
- Ακολουθήστε τις οδηγίες στο άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md) για να εγκαταστήσετε την πιο πρόσφατη έκδοση του Azure PowerShell στον υπολογιστή σας. Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να χρησιμοποιήσετε PowerShell για την ανάπτυξη οντοτήτων εργοστασίου δεδομένων. 
- (προαιρετικό) Ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md) για να μάθετε σχετικά με τα πρότυπα για τη διαχείριση πόρων Azure.


## <a name="in-this-tutorial"></a>Σε αυτό το πρόγραμμα εκμάθησης

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια προέλευση δεδομένων με την εξής οντοτήτων εργοστασίου δεδομένων:

Οντότητα | Περιγραφή  
------ | ----------- 
Υπηρεσία αποθήκευσης συνδεδεμένες του Azure | Συνδέει το λογαριασμό σας Azure χώρου αποθήκευσης με την προέλευση δεδομένων. Azure χώρου αποθήκευσης είναι ο χώρος αποθήκευσης δεδομένων προέλευσης και βάση δεδομένων Azure SQL είναι ο χώρος αποθήκευσης δεδομένων δέκτη για τη δραστηριότητα αντίγραφο στην εκμάθηση. Καθορίζει το λογαριασμό χώρου αποθήκευσης που περιέχει τα δεδομένα εισόδου για τη δραστηριότητα αντίγραφο. 
Azure service συνδεδεμένες της βάσης δεδομένων SQL| Συνδέει τη βάση δεδομένων Azure SQL με την προέλευση δεδομένων. Καθορίζει τη βάση δεδομένων Azure SQL που περιέχει τα δεδομένα εξόδου για τη δραστηριότητα αντίγραφο. 
Azure Blob εισαγωγής συνόλου δεδομένων | Αναφέρεται στην υπηρεσία αποθήκευσης Azure συνδεδεμένες. Η υπηρεσία συνδεδεμένων αναφέρεται σε ένα λογαριασμό αποθήκευσης Azure και του συνόλου δεδομένων αντικειμένων Blob του Azure Καθορίζει το κοντέινερ, φάκελο και όνομα αρχείου στο χώρο αποθήκευσης της που διατηρεί τα δεδομένα εισαγωγής. 
Azure συνόλου δεδομένων εξόδου SQL | Αναφέρεται σε την υπηρεσία SQL Azure συνδεδεμένες. Η υπηρεσία SQL Azure συνδεδεμένες αναφέρεται σε ένα διακομιστή Azure SQL και του συνόλου δεδομένων Azure SQL Καθορίζει το όνομα του πίνακα που περιέχει τα δεδομένα εξόδου. 
Διοχέτευση δεδομένων | Της διοχέτευσης έχει μία δραστηριότητα του τύπου αντιγραφή που λαμβάνει το σύνολο δεδομένων αντικειμένων blob του Azure ως εισαγωγή και του συνόλου δεδομένων Azure SQL ως το αποτέλεσμα. Η δραστηριότητα αντιγραφή αντιγράφει δεδομένα από μια Azure blob σε έναν πίνακα στη βάση δεδομένων Azure SQL.  

Μια προέλευση δεδομένων μπορεί να έχει ένα ή περισσότερα αγωγούς. Μια διαδικασία μπορεί να έχει μία ή περισσότερες δραστηριότητες σε αυτό. Υπάρχουν δύο τύποι δραστηριοτήτων: [δεδομένων κίνηση δραστηριότητες](data-factory-data-movement-activities.md) και οι [δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md). Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια διαδικασία με μία δραστηριότητα (αντιγραφή δραστηριότητα).

![Αντιγραφή αντικειμένων Blob του Azure σε βάση δεδομένων SQL Azure](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

Η παρακάτω ενότητα παρέχει το πρότυπο ολοκληρωθεί από διαχειριστή πόρων για τον ορισμό οντοτήτων εργοστασίου δεδομένων ώστε να μπορείτε γρήγορα να εκτελέσετε μέσω του προγράμματος εκμάθησης και να ελέγξετε το πρότυπο. Για να κατανοήσετε τον τρόπο κάθε οντότητα εργοστασίου δεδομένων είναι οριστούν, ανατρέξτε στην ενότητα [οντοτήτων εργοστασίου δεδομένων στο πρότυπο](#data-factory-entities-in-the-template) .

## <a name="data-factory-json-template"></a>Πρότυπο JSON εργοστασίου δεδομένων
Το πρότυπο ανώτατου επιπέδου για τη διαχείριση πόρων για τον ορισμό ενός εργοστασίου δεδομένων είναι: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Δημιουργήστε ένα αρχείο JSON με το όνομα **ADFCopyTutorialARM.json** στο φάκελο **C:\ADFGetStarted** με το παρακάτω περιεχόμενο:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>Παράμετροι JSON 
Δημιουργία αρχείου JSON με το όνομα **ADFCopyTutorialARM Parameters.json** που περιέχει τις παραμέτρους για το πρότυπο διαχείρισης πόρων Azure. 

> [AZURE.IMPORTANT] Καθορίστε το όνομα και το κλειδί του λογαριασμού σας Azure χώρου αποθήκευσης για τις παραμέτρους του **storageAccountName** και **storageAccountKey** .  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Ενδέχεται να έχετε ξεχωριστή παράμετρο JSON αρχείων για ανάπτυξη, δοκιμή και περιβάλλοντα παραγωγής που μπορείτε να χρησιμοποιήσετε με το ίδιο πρότυπο JSON εργοστασίου δεδομένων. Χρησιμοποιώντας μια δέσμη ενεργειών κελύφους Power, μπορείτε να αυτοματοποιήσετε ανάπτυξη οντοτήτων εργοστασίου δεδομένα σε αυτά τα περιβάλλοντα.  

## <a name="create-data-factory"></a>Δημιουργία εργοστασίου δεδομένων
1. Εκκινήστε το **Azure PowerShell** και εκτελέστε την ακόλουθη εντολή:
    - Εκτέλεση `Login-AzureRmAccount` και πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.  
    - Εκτέλεση `Get-AzureRmSubscription` για να προβάλετε όλες τις συνδρομές για αυτόν το λογαριασμό.
    - Εκτέλεση `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` για να επιλέξετε τη συνδρομή στην οποία θέλετε να εργαστείτε. 
2. Εκτελέστε την ακόλουθη εντολή για την ανάπτυξη οντοτήτων εργοστασίου δεδομένων χρησιμοποιώντας το πρότυπο διαχείρισης πόρων που δημιουργήσατε στο βήμα 1.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Οθόνη διοχέτευσης
1. Συνδεθείτε στην [πύλη του Azure](https://portal.azure.com) χρησιμοποιώντας το λογαριασμό σας Azure.
2. Επιλέξτε **εργοστάσια δεδομένων** από το αριστερό μενού (ή) κάντε κλικ στην επιλογή **περισσότερες υπηρεσίες** και **εργοστάσια δεδομένων** στην κατηγορία **ΠΛΗΡΟΦΟΡΙΏΝ + ΑΝΆΛΥΣΗΣ** .

    ![Μενού εργοστάσια δεδομένα](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Στη σελίδα **εργοστάσια δεδομένων** , αναζήτηση και εύρεση εργοστασίου δεδομένων σας. 

    ![Αναζήτηση για εργοστασίου δεδομένων](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Κάντε κλικ στην επιλογή εργοστασίου Azure δεδομένων σας. Μπορείτε να δείτε την αρχική σελίδα για την προέλευση δεδομένων.

    ![Αρχική σελίδα για την προέλευση δεδομένων](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Κάντε κλικ στο πλακίδιο **διάγραμμα** για να δείτε την προβολή διαγράμματος του εργοστασίου δεδομένων σας.

    ![Προβολή διαγράμματος εργοστασίου δεδομένων](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Στην προβολή διαγράμματος, κάντε διπλό κλικ του συνόλου δεδομένων **SQLOutputDataset**. Βλέπετε αυτήν την κατάσταση των στη φέτα. Όταν ολοκληρωθεί η αντιγραφή, την κατάσταση ορίζετε για να **είστε έτοιμοι**.

    ![Φέτα εξόδου σε έτοιμη κατάσταση](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Όταν στη φέτα βρίσκεται σε κατάσταση **είστε έτοιμοι** , βεβαιωθείτε ότι τα δεδομένα αντιγράφονται στον πίνακα **emp** στη βάση δεδομένων Azure SQL.

Ανατρέξτε στο θέμα [οθόνη συνόλων δεδομένων και διαδικασία](data-factory-monitor-manage-pipelines.md) για οδηγίες σχετικά με τον τρόπο χρήσης του Azure λεπίδες πύλης για την παρακολούθηση των και συνόλων δεδομένων που δημιουργήσατε σε αυτό το πρόγραμμα εκμάθησης.

Μπορείτε επίσης να χρησιμοποιήσετε για την παρακολούθηση της σας αγωγούς δεδομένων οθόνη και διαχείριση εφαρμογών. Ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε αγωγούς εργοστασίου δεδομένων Azure χρήση της εφαρμογής παρακολούθησης](data-factory-monitor-manage-app.md) για λεπτομέρειες σχετικά με τη χρήση της εφαρμογής.


## <a name="data-factory-entities-in-the-template"></a>Οντοτήτων εργοστασίου δεδομένων στο πρότυπο

### <a name="define-data-factory"></a>Ορισμός εργοστασίου δεδομένων
Μπορείτε να ορίσετε μια προέλευση δεδομένων στο πρότυπο διαχείρισης πόρων όπως φαίνεται στο ακόλουθο δείγμα:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Το dataFactoryName ορίζεται ως εξής: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Πρόκειται για μια μοναδική συμβολοσειρά με βάση το αναγνωριστικό ομάδας του πόρου.  

### <a name="defining-data-factory-entities"></a>Ορισμός οντοτήτων εργοστασίου δεδομένων
Το παρακάτω οντοτήτων εργοστασίου δεδομένων ορίζονται στο πρότυπο JSON: 

1. [Υπηρεσία αποθήκευσης συνδεδεμένες του Azure](#azure-storage-linked-service)
2. [Azure SQL συνδεδεμένες υπηρεσίας](#azure-sql-database-linked-service)
3. [Σύνολο δεδομένων αντικειμένων blob του Azure](#azure-blob-dataset)
4. [Σύνολο δεδομένων Azure SQL](#azure-sql-dataset)
5. [Διοχέτευση δεδομένων με μια δραστηριότητα αντιγράφου](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Υπηρεσία αποθήκευσης συνδεδεμένες του Azure
Μπορείτε να καθορίσετε το όνομα και το κλειδί του λογαριασμού σας Azure χώρου αποθήκευσης σε αυτήν την ενότητα. Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για να ορίσετε μια υπηρεσία αποθήκευσης Azure συνδεδεμένες, ανατρέξτε στο θέμα [αποθήκευσης Azure συνδεδεμένες υπηρεσίας](data-factory-azure-blob-connector.md#azure-storage-linked-service) . 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

Η συμβολοσειρά σύνδεσης χρησιμοποιεί τις παραμέτρους storageAccountName και storageAccountKey. Οι τιμές για αυτές τις παραμέτρους που του μεταβιβάστηκε, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων. Τον ορισμό χρησιμοποιεί επίσης μεταβλητές: azureStroageLinkedService και dataFactoryName οριστεί στο πρότυπο. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure service συνδεδεμένες της βάσης δεδομένων SQL
Μπορείτε να καθορίσετε το όνομα του διακομιστή Azure SQL, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης χρήστη σε αυτήν την ενότητα. Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για τον καθορισμό μιας υπηρεσίας SQL Azure συνδεδεμένες, ανατρέξτε στο θέμα [Azure SQL συνδεδεμένες υπηρεσίας](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

Η συμβολοσειρά σύνδεσης χρησιμοποιεί sqlServerName, όνομα βάσης δεδομένων, sqlServerUserName και sqlServerPassword παράμετροι των οποίων οι τιμές μεταβιβάζονται, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων. Τον ορισμό χρησιμοποιεί επίσης τις ακόλουθες μεταβλητές από το πρότυπο: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Σύνολο δεδομένων αντικειμένων blob του Azure
Μπορείτε να καθορίσετε τα ονόματα των αντικειμένων blob κοντέινερ, φάκελο και αρχείο που περιέχει τα δεδομένα εισόδου. Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για να ορίσετε ένα σύνολο αντικειμένων Blob του Azure δεδομένων, ανατρέξτε στο θέμα [αντικειμένων Blob του Azure ιδιότητες συνόλου δεδομένων](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 


    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Σύνολο δεδομένων Azure SQL
Μπορείτε να καθορίσετε το όνομα του πίνακα στη βάση δεδομένων Azure SQL που περιέχει τα δεδομένα που αντιγράψατε από το χώρο αποθήκευσης αντικειμένων Blob του Azure. Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για τον ορισμό ενός συνόλου δεδομένων Azure SQL, ανατρέξτε στο θέμα [Ιδιότητες συνόλου δεδομένων Azure SQL](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Διοχέτευση δεδομένων
Μπορείτε να καθορίσετε μια διαδικασία που αντιγράφει δεδομένα από το σύνολο δεδομένων αντικειμένων blob του Azure για το σύνολο δεδομένων Azure SQL. Ανατρέξτε στο θέμα [Διοχέτευσης JSON](data-factory-create-pipelines.md#pipeline-json) για περιγραφές των JSON στοιχεία που χρησιμοποιούνται για να ορίσετε μια διαδικασία σε αυτό το παράδειγμα. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Εκ νέου χρήση του προτύπου 
Με το πρόγραμμα εκμάθησης, δημιουργήσατε ένα πρότυπο για τον ορισμό οντοτήτων εργοστασίου δεδομένων και ένα πρότυπο για τη μεταφορά τιμές για τις παραμέτρους του. Διοχέτευση αντιγράφει δεδομένα από ένα λογαριασμό αποθήκευσης Azure σε μια βάση δεδομένων Azure SQL που καθορίζεται μέσω παραμέτρους. Για να χρησιμοποιήσετε το ίδιο πρότυπο για να αναπτύξετε οντοτήτων εργοστασίου δεδομένων σε διαφορετικά περιβάλλοντα, δημιουργείτε ένα αρχείο παραμέτρου για κάθε περιβάλλον και χρησιμοποιήσετε κατά την ανάπτυξη στον συγκεκριμένο περιβάλλον.     

Παράδειγμα:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Σημειώστε ότι η πρώτη εντολή χρησιμοποιεί αρχείο παραμέτρων για το περιβάλλον ανάπτυξης, το δεύτερο μία για το περιβάλλον δοκιμής και το τρίτο μία για το περιβάλλον παραγωγής.  

Μπορείτε επίσης να χρησιμοποιήσετε ξανά το πρότυπο για να εκτελέσετε επαναλαμβανόμενες εργασίες. Για παράδειγμα, πρέπει να δημιουργήσετε πολλά εργοστάσια δεδομένων με μία ή περισσότερες αγωγών που υλοποίηση της λογικής ίδια αλλά κάθε εργοστασίου δεδομένων χρησιμοποιεί διαφορετικό Azure χώρου αποθήκευσης και βάση δεδομένων SQL Azure λογαριασμών. Σε αυτό το σενάριο, χρησιμοποιείτε το ίδιο πρότυπο στο ίδιο περιβάλλον (αποκλίσεις, έλεγχος ή παραγωγής) με διαφορετικές παραμέτρου αρχεία για να δημιουργήσετε εργοστάσια δεδομένων.   


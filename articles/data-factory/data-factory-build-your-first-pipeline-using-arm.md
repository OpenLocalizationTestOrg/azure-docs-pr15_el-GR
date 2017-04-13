<properties
    pageTitle="Δημιουργήστε το πρώτο εργοστασιακές δεδομένων (πρότυπο διαχείρισης πόρων) | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια διοχέτευση εργοστασίου δεδομένων Azure δείγμα χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure."
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Πρόγραμμα εκμάθησης: Δημιουργήστε το πρώτο εργοστασίου Azure δεδομένων χρησιμοποιώντας το πρότυπο διαχείρισης πόρων Azure
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-build-your-first-pipeline.md)
- [Πύλη του Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Το πρότυπο διαχείρισης πόρων](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Σε αυτό το άρθρο, μπορείτε να χρησιμοποιήσετε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε το πρώτο εργοστασίου Azure δεδομένων.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
- Διαβάστε το άρθρο [Επισκόπηση πρόγραμμα εκμάθησης](data-factory-build-your-first-pipeline.md) και ολοκληρώστε τα βήματα **προϋπόθεση** .
- Ακολουθήστε τις οδηγίες στο άρθρο [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md) για να εγκαταστήσετε την πιο πρόσφατη έκδοση του Azure PowerShell στον υπολογιστή σας.
- Ανατρέξτε στο θέμα [Σύνταξη από κοινού πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md) για να μάθετε σχετικά με τα πρότυπα για τη διαχείριση πόρων Azure. 

## <a name="in-this-tutorial"></a>Σε αυτό το πρόγραμμα εκμάθησης
Οντότητα | Περιγραφή  
------ | ----------- 
Υπηρεσία αποθήκευσης συνδεδεμένες του Azure | Συνδέει το λογαριασμό σας Azure χώρου αποθήκευσης με την προέλευση δεδομένων. Ο λογαριασμός Azure αποθήκευσης διατηρεί τα δεδομένα εισόδου και εξόδου για τη διαδικασία σε αυτό το δείγμα. 
Υπηρεσία συνδεδεμένο σε ζήτηση HDInsight| Συνδέσεις ένα HDInsight σε ζήτηση συμπλέγματος για την προέλευση δεδομένων. Το σύμπλεγμα δημιουργείται αυτόματα για εσάς την επεξεργασία δεδομένων και διαγράφεται μόλις ολοκληρωθεί η επεξεργασία.
Azure Blob εισαγωγής συνόλου δεδομένων | Αναφέρεται στην υπηρεσία αποθήκευσης Azure συνδεδεμένες. Η υπηρεσία συνδεδεμένων αναφέρεται σε ένα λογαριασμό αποθήκευσης Azure και του συνόλου δεδομένων αντικειμένων Blob του Azure Καθορίζει το κοντέινερ, φάκελο και όνομα αρχείου στο χώρο αποθήκευσης της που διατηρεί τα δεδομένα εισαγωγής. 
Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure | Αναφέρεται στην υπηρεσία αποθήκευσης Azure συνδεδεμένες. Η υπηρεσία συνδεδεμένων αναφέρεται σε ένα λογαριασμό αποθήκευσης Azure και του συνόλου δεδομένων αντικειμένων Blob του Azure Καθορίζει το κοντέινερ, φάκελο και όνομα αρχείου στο χώρο αποθήκευσης που περιέχει τα δεδομένα εξόδου της. 
Διοχέτευση δεδομένων | Η διαδικασία έχει μία δραστηριότητα του τύπου HDInsightHive καταναλώνει του συνόλου δεδομένων εισόδου και παράγει του συνόλου δεδομένων εξόδου.   

Μια προέλευση δεδομένων μπορεί να έχει ένα ή περισσότερα αγωγούς. Μια διαδικασία μπορεί να έχει μία ή περισσότερες δραστηριότητες σε αυτό. Υπάρχουν δύο τύποι δραστηριοτήτων: [δεδομένων κίνηση δραστηριότητες](data-factory-data-movement-activities.md) και οι [δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md). Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια διαδικασία με μία δραστηριότητα (αντιγραφή δραστηριότητα).

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

Δημιουργήστε ένα αρχείο JSON με το όνομα **ADFTutorialARM.json** στο φάκελο **C:\ADFGetStarted** με το παρακάτω περιεχόμενο:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
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
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
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
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Μπορείτε να βρείτε ένα άλλο παράδειγμα του προτύπου για τη διαχείριση πόρων για τη δημιουργία ενός εργοστασίου Azure δεδομένων στην [πρόγραμμα εκμάθησης: δημιουργία ενός διοχέτευσης με χρήση ενός προτύπου για τη διαχείριση πόρων Azure δραστηριότητα αντίγραφο](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>Παράμετροι JSON 
Δημιουργία αρχείου JSON με το όνομα **ADFTutorialARM Parameters.json** που περιέχει τις παραμέτρους για το πρότυπο διαχείρισης πόρων Azure.  

> [AZURE.IMPORTANT] Καθορίστε το όνομα και το κλειδί του λογαριασμού σας Azure χώρου αποθήκευσης για τις παραμέτρους **storageAccountName** και **storageAccountKey** σε αυτό το αρχείο παραμέτρων. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Ενδέχεται να έχετε ξεχωριστή παράμετρο JSON αρχείων για ανάπτυξη, δοκιμή και περιβάλλοντα παραγωγής που μπορείτε να χρησιμοποιήσετε με το ίδιο πρότυπο JSON εργοστασίου δεδομένων. Χρησιμοποιώντας μια δέσμη ενεργειών κελύφους Power, μπορείτε να αυτοματοποιήσετε ανάπτυξη οντοτήτων εργοστασίου δεδομένα σε αυτά τα περιβάλλοντα. 

## <a name="create-data-factory"></a>Δημιουργία εργοστασίου δεδομένων

1. Εκκινήστε το **Azure PowerShell** και εκτελέστε την ακόλουθη εντολή: 
    - Εκτέλεση `Login-AzureRmAccount` και πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.  
    - Εκτέλεση `Get-AzureRmSubscription` για να προβάλετε όλες τις συνδρομές για αυτόν το λογαριασμό.
    - Εκτέλεση `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` για να επιλέξετε τη συνδρομή στην οποία θέλετε να εργαστείτε. Αυτή η συνδρομή πρέπει να είναι το ίδιο με αυτό που χρησιμοποιούνται στην πύλη του Azure.
1. Εκτελέστε την ακόλουθη εντολή για την ανάπτυξη οντοτήτων εργοστασίου δεδομένων χρησιμοποιώντας το πρότυπο διαχείρισης πόρων που δημιουργήσατε στο βήμα 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Οθόνη διοχέτευσης
 
1.  Μετά τη σύνδεση με την [πύλη του Azure](https://portal.azure.com/), κάντε κλικ στην επιλογή **Αναζήτηση** και επιλέξτε **εργοστάσια δεδομένων**.
        ![Αναζήτηση -> εργοστάσια δεδομένων](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Στο το blade **Εργοστάσια δεδομένων** , επιλέξτε την προέλευση δεδομένων (**TutorialFactoryARM**) που δημιουργήσατε.   
2.  Στο το blade **Εργοστασίου δεδομένων** για την προέλευση δεδομένων, κάντε κλικ στην επιλογή **διαγράμματος**.
        ![Πλακίδιο του διαγράμματος](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  Στην **Προβολή "Διάγραμμα"**, μπορείτε να δείτε μια επισκόπηση των αγωγών και των συνόλων δεδομένων που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης.
    
    ![Προβολή διαγράμματος](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Στην προβολή διαγράμματος, κάντε διπλό κλικ του συνόλου δεδομένων **AzureBlobOutput**. Που βλέπετε στη φέτα που υποβάλλεται σε επεξεργασία.

    ![Σύνολο δεδομένων](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Όταν ολοκληρωθεί η επεξεργασία, μπορείτε να δείτε τη φέτα **έτοιμη** κατάσταση. Η δημιουργία ένα σύμπλεγμα HDInsight σε ζήτηση συνήθως χρειάζεται κάποια στιγμή (περίπου 20 λεπτά). Επομένως, περιμένετε τη διαδικασία για να **περιέχει έως και 30 λεπτά** για την επεξεργασία στη φέτα.

    ![Σύνολο δεδομένων](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Όταν στη φέτα βρίσκεται σε κατάσταση **είστε έτοιμοι** , ελέγξτε το φάκελο **partitioneddata** στο κοντέινερ **adfgetstarted** στο χώρο αποθήκευσης αντικειμένων blob του για τα δεδομένα εξόδου.  

Ανατρέξτε στο θέμα [οθόνη συνόλων δεδομένων και διαδικασία](data-factory-monitor-manage-pipelines.md) για οδηγίες σχετικά με τον τρόπο χρήσης του Azure λεπίδες πύλης για την παρακολούθηση των και συνόλων δεδομένων που δημιουργήσατε σε αυτό το πρόγραμμα εκμάθησης.

Μπορείτε επίσης να χρησιμοποιήσετε για την παρακολούθηση της σας αγωγούς δεδομένων οθόνη και διαχείριση εφαρμογών. Ανατρέξτε στο θέμα [οθόνη και να διαχειριστείτε αγωγούς εργοστασίου δεδομένων Azure χρήση της εφαρμογής παρακολούθησης](data-factory-monitor-manage-app.md) για λεπτομέρειες σχετικά με τη χρήση της εφαρμογής. 

> [AZURE.IMPORTANT] Το αρχείο εισαγωγής λαμβάνει διαγραφεί κατά τη φέτα υποβάλλεται σε επεξεργασία με επιτυχία. Επομένως, εάν θέλετε να εκτελέσετε ξανά στη φέτα ή κάντε ξανά το πρόγραμμα εκμάθησης, στείλτε το αρχείο εισόδου (input.log) στο φάκελο inputdata του κοντέινερ adfgetstarted.

## <a name="data-factory-entities-in-the-template"></a>Οντοτήτων εργοστασίου δεδομένων στο πρότυπο
### <a name="define-data-factory"></a>Ορισμός εργοστασίου δεδομένων
Ορισμός ενός εργοστασίου δεδομένων στο πρότυπο της διαχείρισης πόρων, όπως φαίνεται στο ακόλουθο δείγμα:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

Το dataFactoryName ορίζεται ως εξής: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Πρόκειται για μια μοναδική συμβολοσειρά με βάση το αναγνωριστικό ομάδας του πόρου.  

### <a name="defining-data-factory-entities"></a>Ορισμός οντοτήτων εργοστασίου δεδομένων
Το παρακάτω οντοτήτων εργοστασίου δεδομένων ορίζονται στο πρότυπο JSON: 

- [Υπηρεσία αποθήκευσης συνδεδεμένες του Azure](#azure-storage-linked-service)
- [Υπηρεσία συνδεδεμένο σε ζήτηση HDInsight](#hdinsight-on-demand-linked-service)
- [Σύνολο δεδομένων εισαγωγής αντικειμένων blob του Azure](#azure-blob-input-dataset)
- [Σύνολο δεδομένων εξόδου αντικειμένων blob του Azure](#azure-blob-output-dataset)
- [Διοχέτευση δεδομένων με μια δραστηριότητα αντιγράφου](#data-pipeline)

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

Η **συμβολοσειρά σύνδεσης** χρησιμοποιεί τις παραμέτρους storageAccountName και storageAccountKey. Οι τιμές για αυτές τις παραμέτρους που του μεταβιβάστηκε, χρησιμοποιώντας ένα αρχείο ρύθμισης παραμέτρων. Τον ορισμό χρησιμοποιεί επίσης μεταβλητές: azureStroageLinkedService και dataFactoryName οριστεί στο πρότυπο. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>Υπηρεσία συνδεδεμένο σε ζήτηση HDInsight
Δείτε [τον υπολογισμό συνδεδεμένες υπηρεσίες](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) άρθρο για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για τον καθορισμό μιας υπηρεσίας συνδεδεμένο σε ζήτηση HDInsight.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Λάβετε υπόψη τα εξής σημεία: 

- Η προέλευση δεδομένων δημιουργεί ένα σύμπλεγμα HDInsight **βασίζεται σε Windows** με το παραπάνω JSON. Επίσης, μπορείτε να έχετε το Δημιουργήστε ένα σύμπλεγμα HDInsight **βάσει Linux** . Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Μπορείτε να χρησιμοποιήσετε **τη δική σας σύμπλεγμα HDInsight** αντί να χρησιμοποιήσετε ένα σύμπλεγμα HDInsight στη ζήτηση. Για λεπτομέρειες, ανατρέξτε στο θέμα [Υπηρεσία συνδεδεμένων HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Το HDInsight σύμπλεγμα δημιουργεί ένα **προεπιλεγμένο κοντέινερ** το χώρο αποθήκευσης αντικειμένων blob που καθορίσατε στο το JSON (**linkedServiceName**). HDInsight δεν διαγράφει αυτό το κοντέινερ όταν διαγράφεται το σύμπλεγμα. Αυτή η συμπεριφορά οφείλεται στη σχεδίαση. Με την υπηρεσία HDInsight συνδεδεμένο σε ζήτηση, ένα σύμπλεγμα δημιουργείται κάθε φορά που ένα κομμάτι χρειάζεται να γίνεται εκτός αν υπάρχει ένα υπάρχον HDInsight live σύμπλεγμα (**το timeToLive**) και διαγράφονται όταν ολοκληρωθεί η επεξεργασία.

    Κατά την επεξεργασία περισσότερες φέτες, μπορείτε να δείτε πολλά κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του. Εάν δεν χρειάζεστε τους για την αντιμετώπιση προβλημάτων με τις εργασίες, μπορείτε να τα διαγράψετε για να μειώσετε το κόστος χώρου αποθήκευσης. Τα ονόματα από αυτά τα κοντέινερ ακολουθήστε ένα μοτίβο: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Χρήση εργαλείων όπως [Εξερεύνηση χώρου αποθήκευσης](http://storageexplorer.com/) για τη διαγραφή κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του.

Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) .



#### <a name="azure-blob-input-dataset"></a>Σύνολο δεδομένων εισαγωγής αντικειμένων blob του Azure
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
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Αυτός ο ορισμός χρησιμοποιεί τις ακόλουθες παραμέτρους που ορίζονται στο πρότυπο παραμέτρου: blobContainer, inputBlobFolder, και inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Σύνολο δεδομένων εξόδου αντικειμένων Blob του Azure
Μπορείτε να καθορίσετε τα ονόματα των αντικειμένων blob κοντέινερ και ένα φάκελο που περιέχει τα δεδομένα εξόδου. Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιείται για να ορίσετε ένα σύνολο αντικειμένων Blob του Azure δεδομένων, ανατρέξτε στο θέμα [αντικειμένων Blob του Azure ιδιότητες συνόλου δεδομένων](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Αυτός ο ορισμός χρησιμοποιεί τις ακόλουθες παραμέτρους που ορίζονται στο πρότυπο της παραμέτρου: blobContainer και outputBlobFolder. 

#### <a name="data-pipeline"></a>Διοχέτευση δεδομένων
Μπορείτε να καθορίσετε μια διαδικασία που μετασχηματισμού δεδομένων, εκτελώντας Hive δέσμης ενεργειών σε ένα σύμπλεγμα Azure HDInsight στη ζήτηση. Ανατρέξτε στο θέμα [Διοχέτευσης JSON](data-factory-create-pipelines.md#pipeline-json) για περιγραφές των JSON στοιχεία που χρησιμοποιούνται για να ορίσετε μια διαδικασία σε αυτό το παράδειγμα. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Εκ νέου χρήση του προτύπου 
Με το πρόγραμμα εκμάθησης, δημιουργήσατε ένα πρότυπο για τον ορισμό οντοτήτων εργοστασίου δεδομένων και ένα πρότυπο για τη μεταφορά τιμές για τις παραμέτρους του. Για να χρησιμοποιήσετε το ίδιο πρότυπο για να αναπτύξετε οντοτήτων εργοστασίου δεδομένων σε διαφορετικά περιβάλλοντα, δημιουργείτε ένα αρχείο παραμέτρου για κάθε περιβάλλον και χρησιμοποιήσετε κατά την ανάπτυξη στον συγκεκριμένο περιβάλλον.     

Παράδειγμα:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Σημειώστε ότι η πρώτη εντολή χρησιμοποιεί αρχείο παραμέτρων για το περιβάλλον ανάπτυξης, το δεύτερο μία για το περιβάλλον δοκιμής και το τρίτο μία για το περιβάλλον παραγωγής.  

Μπορείτε επίσης να χρησιμοποιήσετε ξανά το πρότυπο για να εκτελέσετε επαναλαμβανόμενες εργασίες. Για παράδειγμα, πρέπει να δημιουργήσετε πολλά εργοστάσια δεδομένων με μία ή περισσότερες αγωγών που υλοποίηση της λογικής ίδια αλλά κάθε εργοστασίου δεδομένων χρησιμοποιεί διαφορετικό Azure χώρου αποθήκευσης και βάση δεδομένων SQL Azure λογαριασμών. Σε αυτό το σενάριο, χρησιμοποιείτε το ίδιο πρότυπο στο ίδιο περιβάλλον (αποκλίσεις, έλεγχος ή παραγωγής) με διαφορετικές παραμέτρου αρχεία για να δημιουργήσετε εργοστάσια δεδομένων. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Διαχείριση πόρων προτύπου για τη δημιουργία μιας πύλης
Ακολουθεί ένα δείγμα προτύπου για τη διαχείριση πόρων για τη δημιουργία μιας πύλης λογική στο πίσω μέρος. Εγκατάσταση μια πύλη σε υπολογιστή εσωτερικής ή Εικονική IaaS Azure και καταχώρηση της πύλης με την υπηρεσία εργοστασίου δεδομένων χρησιμοποιώντας έναν αριθμό-κλειδί. Για λεπτομέρειες, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων μεταξύ της εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Αυτό το πρότυπο δημιουργεί μια εργοστασίου δεδομένων με το όνομα GatewayUsingArmDF με μια πύλη με το όνομα: GatewayUsingARM. 

## <a name="see-also"></a>Δείτε επίσης
| Το θέμα | Περιγραφή |
| :---- | :---- |
| [Δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md) | Σε αυτό το άρθρο παρέχει μια λίστα των δραστηριοτήτων μετασχηματισμό δεδομένων (όπως το HDInsight Hive μετασχηματισμό που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης) υποστηρίζονται από το Azure εργοστασίου δεδομένων. |
| [Προγραμματισμός και εκτέλεσης](data-factory-scheduling-and-execution.md) | Σε αυτό το άρθρο εξηγεί τις προγραμματισμού και εκτέλεσης πτυχές της εργοστασίου δεδομένων Azure μοντέλο εφαρμογών. |
| [Αγωγούς](data-factory-create-pipelines.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε αγωγούς και δραστηριοτήτων στις εργοστασιακές δεδομένων Azure και πώς να τους χρησιμοποιήσετε για να δημιουργήσετε ολοκληρωμένες να βασίζονται σε δεδομένα ροές εργασίας για το σενάριο ή επιχειρήσεις. |
| [Σύνολα δεδομένων](data-factory-create-datasets.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε συνόλων δεδομένων στην προέλευση δεδομένων Azure.
| [Παρακολούθηση και διαχείριση αγωγούς χρήση της εφαρμογής παρακολούθησης](data-factory-monitor-manage-app.md) | Σε αυτό το άρθρο περιγράφει τον τρόπο για την παρακολούθηση, διαχείριση και εντοπισμός σφαλμάτων αγωγούς με την παρακολούθηση & Διαχείριση εφαρμογών. 

  


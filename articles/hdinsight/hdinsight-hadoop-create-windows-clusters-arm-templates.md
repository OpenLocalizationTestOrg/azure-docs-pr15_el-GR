<properties
   pageTitle="Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στο με τη χρήση προτύπων από διαχειριστή πόρων Azure HDInsight | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε συμπλεγμάτων για Azure HDInsight με τη χρήση προτύπων από διαχειριστή πόρων Azure."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Δημιουργία συμπλεγμάτων που βασίζεται σε Windows Hadoop στο με τη χρήση προτύπων από διαχειριστή πόρων Azure HDInsight

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Μάθετε πώς μπορείτε να δημιουργήσετε με τη χρήση προτύπων από διαχειριστή πόρων Azure συμπλεγμάτων HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](../resource-group-template-deploy.md). Για άλλες δημιουργία συμπλέγματος και εργαλεία που διαθέτει κάντε κλικ στην επιλογή καρτέλα, επιλέξτε στο επάνω μέρος αυτής της σελίδας ή [μεθόδους δημιουργίας σύμπλεγμα](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Προαπαιτούμενα στοιχεία:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Πριν να ξεκινήσετε τις οδηγίες σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- [Azure συνδρομής](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- CLI Azure PowerShell ή Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Απαιτήσεις για στοιχείο ελέγχου πρόσβασης

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Πρότυπα διαχείρισης πόρων

Διαχείριση πόρων πρότυπο διευκολύνει για να δημιουργήσετε συμπλεγμάτων HDInsight, τους πόρους εξαρτώμενα (όπως τον προεπιλεγμένο λογαριασμό χώρου αποθήκευσης) και άλλοι πόροι (όπως η βάση δεδομένων SQL Azure για να χρησιμοποιήσετε Apache Sqoop) για την εφαρμογή σας σε μια ενιαία, συντονισμένη λειτουργία. Στο πρότυπο, μπορείτε να ορίσετε τους πόρους που απαιτούνται για την εφαρμογή και καθορίσετε παραμέτρους ανάπτυξης για να εισαγάγετε τιμές για διαφορετικά περιβάλλοντα. Το πρότυπο αποτελείται από JSON και παραστάσεις που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε τιμές για την ανάπτυξη.

Μπορείτε να βρείτε ένα πρότυπο από διαχειριστή πόρων για να δημιουργήσετε ένα σύμπλεγμα HDInsight και για το λογαριασμό χώρου αποθήκευσης Azure εξαρτώμενα στο [Προσάρτημα-A](#appx-a-arm-template). Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας κειμένου για να αποθηκεύσετε το πρότυπο σε ένα αρχείο σε σας σταθμούς εργασίας. Θα μάθετε πώς μπορείτε να καλέσετε το πρότυπο με διάφορα εργαλεία.

Για περισσότερες πληροφορίες σχετικά με το πρότυπο διαχείρισης πόρων, ανατρέξτε στο θέμα

- [Πρότυπα διαχείρισης πόρων Azure συντάκτη](../resource-group-authoring-templates.md)
- [Ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Ανάπτυξη με το PowerShell

Η παρακάτω διαδικασία δημιουργεί ένα σύμπλεγμα HDInsight.

**Για να αναπτύξετε ένα σύμπλεγμα χρησιμοποιώντας το πρότυπο διαχείρισης πόρων**

1. Αποθηκεύστε το αρχείο json στο [παράρτημα A](#appx-a-arm-template) για να σας σταθμούς εργασίας.
2. Ορίστε τις παραμέτρους, εάν είναι απαραίτητο.
3. Εκτελέστε το πρότυπο χρησιμοποιώντας την ακόλουθη δέσμη ενεργειών PowerShell:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    Η δέσμη ενεργειών PowerShell ρυθμίζει μόνο το όνομα του συμπλέγματος και το όνομα του λογαριασμού χώρου αποθήκευσης.  Μπορείτε να ορίσετε άλλες τιμές στο πρότυπο της διαχείρισης πόρων. 
    
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ανάπτυξη με το PowerShell](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Ανάπτυξη με το Azure CLI

Το παρακάτω δείγμα δημιουργεί ένα σύμπλεγμα και το λογαριασμό εξαρτώμενα χώρου αποθήκευσης και κοντέινερ καλώντας ενός προτύπου για τη διαχείριση πόρων:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Ανάπτυξη με REST API

Ανατρέξτε στο θέμα [Ανάπτυξη με το REST API](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Ανάπτυξη με το Visual Studio

Με το Visual Studio, μπορείτε να δημιουργήσετε ένα έργο ομάδα πόρων και να αναπτύξετε για Azure μέσω του περιβάλλοντος εργασίας χρήστη. Επιλέξτε τον τύπο των πόρων για να συμπεριλάβετε στο έργο σας και αυτούς τους πόρους προστίθενται αυτόματα σε πρότυπο διαχείρισης πόρων. Το project παρέχει επίσης μια δέσμη ενεργειών του PowerShell για να αναπτύξετε το πρότυπο.

Για μια εισαγωγή στη χρήση του Visual Studio με ομάδες πόρων, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη ομάδων πόρων Azure μέσω του Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο μάθατε διάφορους τρόπους για να δημιουργήσετε ένα σύμπλεγμα HDInsight. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:


- Για ένα παράδειγμα ανάπτυξης πόρους μέσω της βιβλιοθήκης του προγράμματος-πελάτη .NET, ανατρέξτε στο θέμα [Χρήση πόρους ανάπτυξης των βιβλιοθηκών .NET και ένα πρότυπο](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Για ένα αναλυτικό παράδειγμα για την ανάπτυξη μιας εφαρμογής, ανατρέξτε στο θέμα [προμήθεια και ανάπτυξη microservices προβλέψιμα στο Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Για οδηγίες σχετικά με την ανάπτυξη της λύσης σε διαφορετικά περιβάλλοντα, ανατρέξτε στο θέμα [ανάπτυξη και περιβάλλοντα δοκιμής στο Microsoft Azure](../solution-dev-test-environments.md).
- Για να μάθετε περισσότερα σχετικά με τις ενότητες του προτύπου για τη διαχείριση πόρων Azure, ανατρέξτε στο θέμα [πρότυπα σύνταξη από κοινού](../resource-group-authoring-templates.md).
- Για μια λίστα με τις συναρτήσεις που μπορείτε να χρησιμοποιήσετε σε ένα πρότυπο από διαχειριστή πόρων Azure, ανατρέξτε στο θέμα [συναρτήσεις πρότυπο](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Διαχείριση πόρων Appx A: προτύπου

Το ακόλουθο πρότυπο διαχείρισης πόρων Azure δημιουργεί ένα σύμπλεγμα Hadoop που βασίζεται σε Windows με το λογαριασμό εξαρτώμενα Azure χώρου αποθήκευσης.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }


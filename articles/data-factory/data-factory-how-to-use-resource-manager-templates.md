<properties 
    pageTitle="Χρήση της διαχείρισης πόρων πρότυπα στο εργοστασίου δεδομένων | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε πρότυπα διαχείρισης πόρων Azure για τη δημιουργία οντοτήτων εργοστασίου δεδομένων." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Χρησιμοποιήστε πρότυπα για τη δημιουργία οντοτήτων εργοστασίου δεδομένων Azure

## <a name="overview"></a>Επισκόπηση
Κατά τη χρήση Azure εργοστασίου δεδομένων για τις ανάγκες σας ενοποίησης δεδομένων, ενδέχεται να μπορείτε να βρείτε τον εαυτό σας επαναχρησιμοποίηση το ίδιο μοτίβο σε διαφορετικά περιβάλλοντα ή εφαρμογής στην ίδια εργασία επανειλημμένη μέσα την ίδια λύση. Τα πρότυπα σας βοηθήσουν να υλοποιήσετε και να διαχειριστείτε αυτά τα σενάρια σε έναν εύκολο τρόπο. Πρότυπα του Azure εργοστασίου δεδομένων είναι ιδανική για σενάρια που έχουν σχέση με δυνατότητα επαναχρησιμοποίησης και επανάληψης.
 
Εξετάστε την περίπτωση όπου μια εταιρεία έχει 10 εγκαταστάσεις κατασκευής σε ολόκληρο τον κόσμο. Τα αρχεία καταγραφής από κάθε εργοστασίου είναι αποθηκευμένα σε μια βάση δεδομένων SQL Server ξεχωριστή εσωτερικής εγκατάστασης. Η εταιρεία θέλει να δημιουργήσετε μια αποθήκη δεδομένων μία μόνο στο cloud για ανάλυση ad-hoc. Θέλει να έχουν το ίδιο λογικής αλλά διαφορετικές ρυθμίσεις παραμέτρων για ανάπτυξη, δοκιμή και παραγωγής περιβάλλοντα. 

Σε αυτήν την περίπτωση, μια εργασία πρέπει να επαναληφθεί μέσα στο ίδιο περιβάλλον, αλλά με διαφορετικές τιμές κατά μήκος του εργοστάσια 10 δεδομένων για κάθε εργοστασίου κατασκευής. Στην πραγματικότητα, υπάρχει **επανάληψης** . Templating επιτρέπει την άντληση αυτήν τη γενική ροή (δηλαδή, αγωγών που έχουν το ίδιο δραστηριότητες σε κάθε εργοστασίου δεδομένων), αλλά χρησιμοποιεί μια παράμετρος ξεχωριστό αρχείο για κάθε εργοστασίου κατασκευής.

Επιπλέον, όπως η εταιρεία θέλει να αναπτύξετε αυτά τα εργοστάσια 10 δεδομένων πολλές φορές σε διαφορετικά περιβάλλοντα, πρότυπα μπορούν να χρησιμοποιούν αυτήν τη **δυνατότητα επαναχρησιμοποίησης** , χρησιμοποιώντας παράμετρος ξεχωριστό αρχείων για περιβάλλοντα ανάπτυξης, δοκιμή και παραγωγής.

## <a name="templating-with-azure-resource-manager"></a>Templating με τη Διαχείριση Azure πόρων
[Διαχείριση πόρων Azure πρότυπα](../azure-resource-manager/resource-group-overview.md#template-deployment) είναι ένας εξαιρετικός τρόπος για να επιτύχετε templating του Azure εργοστασίου δεδομένων. Διαχείριση πόρων πρότυπα ορίζουν την υποδομή και τις παραμέτρους του Azure λύση μέσω ενός αρχείου JSON. Επειδή πρότυπα διαχείρισης πόρων Azure λειτουργούν με τις υπηρεσίες του Azure όλα/πιο, αυτό μπορεί να ευρέως χρησιμοποιηθεί για να διαχειριστείτε εύκολα όλους τους πόρους σας Azure περιουσιακών στοιχείων. Ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](../resource-group-authoring-templates.md) για να μάθετε περισσότερα σχετικά με τα πρότυπα για τη διαχείριση πόρων σε γενικές γραμμές. 

## <a name="tutorials"></a>Προγράμματα εκμάθησης
Δείτε τα παρακάτω προγράμματα εκμάθησης για οδηγίες βήμα προς βήμα για τη δημιουργία οντοτήτων εργοστασίου δεδομένων με τη χρήση προτύπων για τη διαχείριση πόρων:

- [Πρόγραμμα εκμάθησης: Δημιουργία μια διαδικασία για να αντιγράψετε δεδομένα με τη χρήση προτύπου για τη διαχείριση πόρων Azure](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Πρόγραμμα εκμάθησης: Δημιουργία μιας διοχέτευσης την επεξεργασία δεδομένων με τη χρήση προτύπου για τη διαχείριση πόρων Azure](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Πρότυπα εργοστασίου δεδομένων στο Github
Δείτε τα παρακάτω πρότυπα Github Azure γρήγορης εκκίνησης: 

- [Δημιουργία μιας προέλευσης δεδομένων για να αντιγράψετε δεδομένα από το χώρο αποθήκευσης Blob του Azure με βάση δεδομένων SQL Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Δημιουργία μιας προέλευσης δεδομένων με τη δραστηριότητα Hive σε σύμπλεγμα Azure HDInsight](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Δημιουργία μιας προέλευσης δεδομένων για να αντιγράψετε δεδομένα από το Salesforce αντικείμενα blob του Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Είστε ευπρόσδεκτοι να κάνετε κοινή χρήση των προτύπων εργοστασίου δεδομένων Azure στο [Azure γρήγορης εκκίνησης](https://azure.microsoft.com/documentation/templates/). Ανατρέξτε στον [Οδηγό συμμετοχής](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) κατά την ανάπτυξη προτύπων που μπορεί να είναι κοινόχρηστο μέσω του αποθετηρίου δεδομένων. 

Οι παρακάτω ενότητες παρέχουν λεπτομέρειες σχετικά με τον ορισμό πόροι εργοστασίου δεδομένων σε ένα πρότυπο από διαχειριστή πόρων. 

## <a name="defining-data-factory-resources-in-templates"></a>Ορισμός πόροι εργοστασίου δεδομένων σε πρότυπα
Το πρότυπο ανώτατου επιπέδου για τον ορισμό ενός εργοστασίου δεδομένων είναι:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Ορισμός εργοστασίου δεδομένων

Ορισμός ενός εργοστασίου δεδομένων στο πρότυπο της διαχείρισης πόρων, όπως φαίνεται στο ακόλουθο δείγμα:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

Το dataFactoryName ορίζεται σε "Μεταβλητές" ως εξής:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Ορισμός συνδεδεμένες υπηρεσίες 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Ανατρέξτε στο θέμα [Υπηρεσία αποθήκευσης στο συνδεδεμένο](data-factory-azure-blob-connector.md#azure-storage-linked-service) ή [Τον υπολογισμό συνδεδεμένες υπηρεσίες](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) για λεπτομέρειες σχετικά με τις ιδιότητες JSON για τη συγκεκριμένη υπηρεσία συνδεδεμένων που θέλετε να αναπτύξετε. Η παράμετρος "dependsOn" Καθορίζει το όνομα του αντίστοιχου εργοστασίου δεδομένων. Παράδειγμα τον ορισμό συνδεδεμένων υπηρεσίας για το χώρο αποθήκευσης Azure εμφανίζεται στον ορισμό JSON παρακάτω:

### <a name="define-datasets"></a>Ορισμός συνόλων δεδομένων

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Ανατρέξτε [χώροι αποθήκευσης δεδομένων που υποστηρίζονται](data-factory-data-movement-activities.md#supported-data-stores-and-formats) για λεπτομέρειες σχετικά με τις ιδιότητες JSON για τον τύπο συγκεκριμένο σύνολο δεδομένων που θέλετε να αναπτύξετε. Σημείωση η παράμετρος "dependsOn" καθορίζει όνομα της αντίστοιχο εργοστασίου δεδομένα και αποθήκευσης συνδεδεμένη υπηρεσίας. Ένα παράδειγμα του καθορισμού τύπου συνόλου δεδομένων χώρο αποθήκευσης αντικειμένων blob του Azure εμφανίζεται στον ορισμό JSON παρακάτω:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Ορισμός αγωγούς

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Αναφέρουν [τον ορισμό αγωγούς](data-factory-create-pipelines.md#pipeline-json) για λεπτομέρειες σχετικά με τις ιδιότητες JSON για τον ορισμό των συγκεκριμένων και δραστηριότητες που θέλετε να αναπτύξετε. Σημείωση η παράμετρος "dependsOn" Καθορίζει το όνομα του εργοστασίου δεδομένων και τις αντίστοιχες συνδεδεμένες υπηρεσίες ή σύνολα δεδομένων. Παράδειγμα ενός δικτύου αγωγών που αντιγράφει δεδομένα από το χώρο αποθήκευσης Blob του Azure σε βάση δεδομένων SQL Azure εμφανίζεται στην το παρακάτω τμήμα κώδικα JSON:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Ρύθμιση προτύπου εργοστασίου δεδομένων
Για βέλτιστες πρακτικές στη ρύθμιση, ανατρέξτε στο θέμα [βέλτιστες πρακτικές για τη δημιουργία προτύπων για τη διαχείριση πόρων Azure](../resource-manager-template-best-practices.md#parameters) το άρθρο. Σε γενικές γραμμές, χρήση παραμέτρου πρέπει να είναι ελαχιστοποιημένο, ειδικά εάν μεταβλητές μπορούν να χρησιμοποιηθούν αντί για αυτό. Δώστε μόνο τις παραμέτρους στα εξής σενάρια:

- Ρυθμίσεις διαφέρουν από το περιβάλλον (παράδειγμα: ανάπτυξη, έλεγχος και παραγωγής)
- Απόρρητο (όπως κωδικούς πρόσβασης)

Εάν πρέπει να αποσπάσετε απόρρητο από [Θάλαμο κλειδί Azure](../key-vault/key-vault-get-started.md) κατά την ανάπτυξη οντοτήτων Azure εργοστασίου δεδομένων με τη χρήση προτύπων, καθορίστε το **μυστικό όνομα** και **κλειδιού θάλαμο** όπως φαίνεται στο ακόλουθο παράδειγμα:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Κατά την εξαγωγή προτύπων για υπάρχοντα δεδομένα εργοστάσια αυτήν τη στιγμή δεν υποστηρίζεται ακόμα, βρίσκεται σε εξέλιξη. 



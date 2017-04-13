<properties
    pageTitle="Δημιουργία του πρώτου εργοστασίου δεδομένων (REST) | Microsoft Azure"
    description="Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να δημιουργήσετε μια διοχέτευση εργοστασίου δεδομένων Azure δείγμα χρησιμοποιώντας δεδομένα εργοστασίου REST API."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Πρόγραμμα εκμάθησης: Δημιουργία εργοστασίου σας πρώτη Azure δεδομένων με χρήση δεδομένων εργοστασίου REST API
> [AZURE.SELECTOR]
- [Επισκόπηση και τις προϋποθέσεις](data-factory-build-your-first-pipeline.md)
- [Πύλη του Azure](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Το πρότυπο διαχείρισης πόρων](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Σε αυτό το άρθρο, μπορείτε να χρησιμοποιήσετε δεδομένα εργοστασίου REST API για να δημιουργήσετε το πρώτο εργοστασίου Azure δεδομένων.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
- Διαβάστε το άρθρο [Επισκόπηση πρόγραμμα εκμάθησης](data-factory-build-your-first-pipeline.md) και ολοκληρώστε τα βήματα **προϋπόθεση** .
- Εγκαταστήστε το [Curl](https://curl.haxx.se/dlwiz/) στον υπολογιστή σας. Μπορείτε να χρησιμοποιήσετε το εργαλείο ΚΑΜΠΎΛΗ με ΥΠΌΛΟΙΠΟ εντολές για να δημιουργήσετε μια προέλευση δεδομένων. 
- Ακολουθήστε τις οδηγίες από [αυτό το άρθρο](../resource-group-create-service-principal-portal.md) για να: 
    1. Δημιουργία μιας εφαρμογής Web με το όνομα **ADFGetStartedApp** στο Azure Active Directory.
    2. Λάβετε **Αναγνωριστικό υπολογιστή-πελάτη** και **μυστικό κλειδί**. 
    3. Λάβετε **Αναγνωριστικό μισθωτή**. 
    4. Αντιστοιχίστε την εφαρμογή **ADFGetStartedApp** με το ρόλο του **Συνεργάτη εργοστασίου δεδομένων** .  
- Εγκατάσταση του [Azure PowerShell](../powershell-install-configure.md).  
- Εκκινήστε το **PowerShell** και εκτελέστε την ακόλουθη εντολή. Κρατήστε ανοιχτή Azure PowerShell μέχρι το τέλος αυτού του προγράμματος εκμάθησης. Εάν κλείσετε και ανοίξετε ξανά, πρέπει να εκτελέσετε ξανά τις εντολές.
    1. Εκτέλεση **Login AzureRmAccount** και πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.  
    2. Εκτελέστε το **Get-AzureRmSubscription** για να προβάλετε όλες τις συνδρομές για αυτόν το λογαριασμό.
    3. Εκτέλεση **Get AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Ορισμός AzureRmContext** για να επιλέξετε τη συνδρομή στην οποία θέλετε να εργαστείτε. Αντικατάσταση **NameOfAzureSubscription** με το όνομα της συνδρομής σας στο Azure. 
3. Δημιουργία ομάδας του Azure πόρων με το όνομα **ADFTutorialResourceGroup** , εκτελώντας την παρακάτω εντολή στο το PowerShell:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Ορισμένα από τα βήματα σε αυτό το πρόγραμμα εκμάθησης λαμβάνεται ως δεδομένο ότι χρησιμοποιείτε την ομάδα των πόρων με το όνομα ADFTutorialResourceGroup. Εάν χρησιμοποιείτε μια ομάδα διαφορετικό πόρο, πρέπει να χρησιμοποιήσετε το όνομα της ομάδας σας πόρων στη θέση ADFTutorialResourceGroup σε αυτό το πρόγραμμα εκμάθησης.

## <a name="create-json-definitions"></a>Δημιουργία ορισμών JSON
Δημιουργία παρακάτω JSON αρχεία στο φάκελο όπου βρίσκεται το curl.exe. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Όνομα πρέπει να είναι καθολικά μοναδικό, ώστε να μπορείτε να πρόθεμα/επίθημα ADFCopyTutorialDF ώστε να είναι ένα μοναδικό όνομα. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Αντικαταστήστε το **όνομα λογαριασμού** και **accountkey** με όνομα και το κλειδί του λογαριασμού σας Azure χώρου αποθήκευσης. Για να μάθετε πώς μπορείτε να λάβετε τον αριθμό-κλειδί πρόσβασης χώρου αποθήκευσης, ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate χώρου αποθήκευσης](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.2",
                "clusterSize": 1,
                "timeToLive": "00:30:00",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }

Ο παρακάτω πίνακας παρέχει περιγραφές για τις ιδιότητες JSON χρησιμοποιείται σε το τμήμα κώδικα:

| Ιδιότητα | Περιγραφή |
| :------- | :---------- |
| Έκδοση | Καθορίζει ότι η δημιουργία έκδοσης της το HDInsight να είναι 3,2. | 
| ClusterSize | Μέγεθος του συμπλέγματος HDInsight. | 
| Το TimeToLive | Καθορίζει ότι το χρόνο αδράνειας για το σύμπλεγμα HDInsight, πριν να διαγραφεί. |
| linkedServiceName | Καθορίζει το λογαριασμό χώρου αποθήκευσης που χρησιμοποιείται για την αποθήκευση των αρχείων καταγραφής που δημιουργούνται από το HDInsight |

Λάβετε υπόψη τα εξής σημεία: 

- Η προέλευση δεδομένων δημιουργεί ένα σύμπλεγμα HDInsight **βασίζεται σε Windows** με το παραπάνω JSON. Επίσης, μπορείτε να έχετε το Δημιουργήστε ένα σύμπλεγμα HDInsight **βάσει Linux** . Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 
- Μπορείτε να χρησιμοποιήσετε **τη δική σας σύμπλεγμα HDInsight** αντί να χρησιμοποιήσετε ένα σύμπλεγμα HDInsight στη ζήτηση. Για λεπτομέρειες, ανατρέξτε στο θέμα [Υπηρεσία συνδεδεμένων HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) .
- Το HDInsight σύμπλεγμα δημιουργεί ένα **προεπιλεγμένο κοντέινερ** το χώρο αποθήκευσης αντικειμένων blob που καθορίσατε στο το JSON (**linkedServiceName**). HDInsight δεν διαγράφει αυτό το κοντέινερ όταν διαγράφεται το σύμπλεγμα. Αυτή η συμπεριφορά οφείλεται στη σχεδίαση. Με την υπηρεσία HDInsight συνδεδεμένο σε ζήτηση, ένα σύμπλεγμα δημιουργείται κάθε φορά που ένα κομμάτι υποβάλλεται σε επεξεργασία, εκτός εάν υπάρχει μια υπάρχουσα HDInsight live σύμπλεγμα (**το timeToLive**) και διαγράφονται όταν ολοκληρωθεί η επεξεργασία.

    Κατά την επεξεργασία περισσότερες φέτες, μπορείτε να δείτε πολλά κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του. Εάν δεν χρειάζεστε τους για την αντιμετώπιση προβλημάτων με τις εργασίες, μπορείτε να τα διαγράψετε για να μειώσετε το κόστος χώρου αποθήκευσης. Τα ονόματα από αυτά τα κοντέινερ ακολουθήστε ένα μοτίβο: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Χρήση εργαλείων όπως [Εξερεύνηση χώρου αποθήκευσης](http://storageexplorer.com/) για τη διαγραφή κοντέινερ στο χώρο αποθήκευσης αντικειμένων blob του Azure του.

Για λεπτομέρειες, ανατρέξτε στο θέμα [On demand HDInsight συνδεδεμένων υπηρεσιών](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . 

### <a name="inputdatasetjson"></a>inputdataset.JSON

    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


Το JSON ορίζει ένα σύνολο δεδομένων με το όνομα **AzureBlobInput**, που αντιπροσωπεύει δεδομένα εισόδου για μια δραστηριότητα στη διοχέτευση. Επιπλέον, καθορίζει ότι τα δεδομένα εισόδου βρίσκεται στο κοντέινερ αντικειμένων blob που ονομάζεται **adfgetstarted** και στο φάκελο που ονομάζεται **inputdata**.

Ο παρακάτω πίνακας παρέχει περιγραφές για τις ιδιότητες JSON χρησιμοποιείται σε το τμήμα κώδικα:

| Ιδιότητα | Περιγραφή |
| :------- | :---------- |
| Τύπος | Η ιδιότητα τύπος έχει οριστεί σε AzureBlob επειδή βρίσκονται στο χώρο αποθήκευσης αντικειμένων blob του Azure δεδομένα. |  
| linkedServiceName | αναφέρεται το StorageLinkedService που δημιουργήσατε νωρίτερα. |
| όνομα αρχείου | Αυτή η ιδιότητα είναι προαιρετικό. Εάν παραλείψετε αυτή την ιδιότητα, όλα τα αρχεία από το folderPath έχουν συλλεχθεί. Σε αυτήν την περίπτωση, γίνεται επεξεργασία μόνο το input.log. |
| Τύπος | Τα αρχεία καταγραφής είναι σε μορφή κειμένου, ώστε να χρησιμοποιήσουμε TextFormat. | 
| Διαχωριστικό_στήλης | στήλες στα αρχεία καταγραφής είναι οριοθετημένο από ένα κόμμα () |
| συχνότητα/διάστημα | συχνότητα ρυθμιστεί μήνα και διάστημα είναι 1, το οποίο σημαίνει ότι τις φέτες του εισόδου είναι διαθέσιμες κάθε μήνα. | 
| εξωτερική | Αυτή η ιδιότητα έχει οριστεί στην τιμή true αν τα δεδομένα εισόδου δεν δημιουργείται από την υπηρεσία εργοστασίου δεδομένων. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
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

Το JSON ορίζει ένα σύνολο δεδομένων με το όνομα **AzureBlobOutput**, που αντιπροσωπεύει τα δεδομένα εξόδου για μια δραστηριότητα στη διοχέτευση. Επιπλέον, καθορίζει ότι τα αποτελέσματα είναι αποθηκευμένα στο κοντέινερ αντικειμένων blob που ονομάζεται **adfgetstarted** και στο φάκελο που ονομάζεται **partitioneddata**. Στην ενότητα **διαθεσιμότητα** Καθορίζει ότι το σύνολο δεδομένων εξόδου παράγεται σε μηνιαία βάση.

### <a name="pipelinejson"></a>pipeline.JSON
> [AZURE.IMPORTANT] Αντικατάσταση **storageaccountname** με το όνομα του λογαριασμού σας Azure χώρου αποθήκευσης. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
                "policy": {
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Month",
                    "interval": 1
                },
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

Με το τμήμα κώδικα JSON, θέλετε να δημιουργήσετε μια διαδικασία που αποτελείται από μια μεμονωμένη δραστηριότητα που χρησιμοποιεί Hive την επεξεργασία δεδομένων σε ένα σύμπλεγμα HDInsight.

Το αρχείο δέσμης ενεργειών ομάδας, **partitionweblogs.hql**, αποθηκεύεται στο ο λογαριασμός Azure χώρου αποθήκευσης (που καθορίζεται από το scriptLinkedService, που ονομάζεται **StorageLinkedService**) και στο φάκελο **δέσμης ενεργειών** σε το κοντέινερ **adfgetstarted**.

Στην ενότητα **ορίζει** καθορίζει ρυθμίσεις χρόνου εκτέλεσης που μεταβιβάζονται στη δέσμη ενεργειών hive ως τιμές παραμέτρων Hive (π.χ. ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Τις ιδιότητες **Έναρξη** και **Λήξη** της διοχέτευσης Καθορίζει την ενεργή περίοδο της διοχέτευσης.

Στη δραστηριότητα JSON, μπορείτε να καθορίσετε ότι η δέσμη ενεργειών Hive εκτελείται σε του υπολογισμού που καθορίζεται από το **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [AZURE.NOTE] Για λεπτομέρειες σχετικά με τις ιδιότητες JSON που χρησιμοποιούνται στο προηγούμενο παράδειγμα, ανατρέξτε στο θέμα [Ανατομία μια διαδικασία](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

## <a name="set-global-variables"></a>Ορισμός καθολικών μεταβλητών

Στο Azure PowerShell, εκτελέστε τις ακόλουθες εντολές μετά την αντικατάσταση των τιμών με το δικό σας:

> [AZURE.IMPORTANT] Για οδηγίες σχετικά με την επίτευξη Αναγνωριστικό υπολογιστή-πελάτη, μυστικό προγράμματος-πελάτη, ανατρέξτε στην ενότητα [τις προϋποθέσεις](#prerequisites) μισθωτή Αναγνωριστικό και αναγνωριστικό συνδρομής.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>Ο έλεγχος ταυτότητας με AAD

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Δημιουργία εργοστασίου δεδομένων

Σε αυτό το βήμα, δημιουργείτε μια εργοστασίου δεδομένων Azure με το όνομα **FirstDataFactoryREST**. Μια προέλευση δεδομένων μπορεί να έχει ένα ή περισσότερα αγωγούς. Μια διαδικασία μπορεί να έχει μία ή περισσότερες δραστηριότητες σε αυτό. Για παράδειγμα, μια δραστηριότητα Αντιγραφή για να αντιγράψετε δεδομένα από ένα αρχείο προέλευσης για ένα χώρο αποθήκευσης δεδομένων προορισμού και δραστηριότητα HDInsight Hive για να εκτελέσετε Hive δέσμη ενεργειών για το μετασχηματισμό δεδομένων. Εκτελέστε τις ακόλουθες εντολές για να δημιουργήσετε την προέλευση δεδομένων: 

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**. 

    Επιβεβαιώστε ότι το όνομα του εργοστασίου δεδομένων που καθορίζετε εδώ (ADFCopyTutorialDF) συμφωνεί με το όνομα που καθορίζεται στο το **datafactory.json**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.

        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν η προέλευση δεδομένων δημιουργήθηκε με επιτυχία, δείτε το JSON για την προέλευση δεδομένων στα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.  

        Write-Host $results

Λάβετε υπόψη τα εξής σημεία:
 
- Το όνομα του Azure εργοστασίου δεδομένων πρέπει να είναι μοναδικό καθολικό. Εάν βλέπετε το σφάλμα στα αποτελέσματα: **όνομα εργοστασίου δεδομένων "FirstDataFactoryREST" δεν είναι διαθέσιμη**, κάντε τα εξής βήματα:  
    1. Αλλάξτε το όνομα (για παράδειγμα, yournameFirstDataFactoryREST) στο αρχείο **datafactory.json** . Ανατρέξτε στο θέμα [Factory δεδομένων - κανόνες ονοματοθεσίας](data-factory-naming-rules.md) για τους κανόνες ονοματοθεσίας για αντικείμενα εργοστασίου δεδομένων.
    2. Στην πρώτη εντολή όπου η μεταβλητή **$cmd** έχει ανατεθεί μια τιμή, αντικαταστήστε το FirstDataFactoryREST με το νέο όνομα και εκτελέστε την εντολή. 
    3. Εκτελέστε τις επόμενες δύο εντολές για να καλέσετε το REST API για να δημιουργήσετε την προέλευση δεδομένων και να εκτυπώσετε τα αποτελέσματα της λειτουργίας. 
- Για να δημιουργήσετε παρουσίες εργοστασίου δεδομένων, πρέπει να είστε διαχειριστής μιας συμβολής/της συνδρομής Azure
- Το όνομα του εργοστασίου δεδομένων μπορεί να έχει εγγραφεί ως όνομα DNS στο μέλλον και συνεπώς γίνονται ορατά στο κοινό.
- Εάν λαμβάνετε το μήνυμα σφάλματος: "**Αυτή η συνδρομή δεν έχει καταχωρηθεί για να χρησιμοποιήσετε το χώρο ονομάτων Microsoft.DataFactory**", κάντε ένα από τα παρακάτω και προσπαθήστε να δημοσιεύσετε ξανά: 

    - Στο Azure PowerShell, εκτελέστε την ακόλουθη εντολή για να καταχωρήσετε την υπηρεσία παροχής δεδομένων εργοστασίου: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Μπορείτε να εκτελέσετε την παρακάτω εντολή για να επιβεβαιώσετε ότι η προέλευση δεδομένων έχει καταχωρηθεί η υπηρεσία παροχής που χρησιμοποιείτε: 
    
            Get-AzureRmResourceProvider
    - Συνδεθείτε χρησιμοποιώντας το Azure συνδρομή στην [πύλη του Azure](https://portal.azure.com) και μεταβείτε σε μια blade εργοστασίου δεδομένων (ή) Δημιουργήστε μια προέλευση δεδομένων στην πύλη του Azure. Αυτή η ενέργεια καταχωρεί αυτόματα την υπηρεσία παροχής για εσάς.

Πριν να δημιουργήσετε μια διαδικασία, πρέπει πρώτα να δημιουργήσετε μερικά οντοτήτων εργοστασίου δεδομένων. Δημιουργείτε πρώτα να σύνδεση δεδομένων αποθηκεύει/τύπος υπολογίζει το χώρο αποθήκευσης δεδομένων, Καθορισμός εισόδου και εξόδου σύνολα δεδομένων για την αναπαράσταση των δεδομένων σε συνδεδεμένα δεδομένα αποθηκεύει συνδεδεμένες υπηρεσίες. 

## <a name="create-linked-services"></a>Δημιουργία συνδεδεμένων υπηρεσιών 
Σε αυτό το βήμα, μπορείτε να συνδέσετε το λογαριασμό σας χώρο αποθήκευσης Azure και ένα σύμπλεγμα Azure HDInsight στη ζήτηση για την προέλευση δεδομένων. Ο λογαριασμός Azure αποθήκευσης διατηρεί τα δεδομένα εισόδου και εξόδου για τη διαδικασία σε αυτό το δείγμα. Για να εκτελέσετε Hive δέσμη ενεργειών που καθορίζονται στη δραστηριότητα από τη διαδικασία σε αυτό το δείγμα χρησιμοποιείται η υπηρεσία HDInsight συνδεδεμένες. 

### <a name="create-azure-storage-linked-service"></a>Δημιουργία χώρου αποθήκευσης Azure συνδεδεμένες υπηρεσίας
Σε αυτό το βήμα, μπορείτε να συνδέσετε το λογαριασμό σας χώρο αποθήκευσης Azure για την προέλευση δεδομένων. Με αυτό το πρόγραμμα εκμάθησης, μπορείτε να χρησιμοποιήσετε τον ίδιο λογαριασμό Azure χώρου αποθήκευσης για την αποθήκευση δεδομένων εισόδου/εξόδου και το αρχείο δέσμης ενεργειών HQL.

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν η υπηρεσία συνδεδεμένων δημιουργήθηκε με επιτυχία, δείτε το JSON για την υπηρεσία συνδεδεμένων στα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Δημιουργία Azure HDInsight συνδεδεμένες υπηρεσίας
Σε αυτό το βήμα, μπορείτε να συνδέσετε ένα σύμπλεγμα HDInsight στην απαιτήσεων για την προέλευση δεδομένων. Το HDInsight σύμπλεγμα αυτόματα δημιουργείται κατά το χρόνο εκτέλεσης και διαγραφεί μετά την ολοκλήρωση επεξεργασίας και αδρανής για το καθορισμένο χρονικό διάστημα. Μπορείτε να χρησιμοποιήσετε τη δική σας σύμπλεγμα HDInsight αντί να χρησιμοποιήσετε ένα σύμπλεγμα HDInsight στη ζήτηση. Για λεπτομέρειες, ανατρέξτε στο θέμα [Τον υπολογισμό συνδεδεμένες υπηρεσίες](data-factory-compute-linked-services.md) .  

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.

        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν η υπηρεσία συνδεδεμένων δημιουργήθηκε με επιτυχία, δείτε το JSON για την υπηρεσία συνδεδεμένων στα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.  

        Write-Host $results

## <a name="create-datasets"></a>Δημιουργία συνόλων δεδομένων
Σε αυτό το βήμα, δημιουργείτε σύνολα δεδομένων για να αντιπροσωπεύει τα δεδομένα εισόδου και εξόδου δεδομένων για την επεξεργασία της ομάδας. Αυτά τα σύνολα δεδομένων που αναφέρονται σε **StorageLinkedService** που δημιουργήσατε νωρίτερα σε αυτό το πρόγραμμα εκμάθησης. Τα σημεία συνδεδεμένων υπηρεσιών σε λογαριασμό αποθήκευσης Azure και συνόλων δεδομένων καθορίσετε κοντέινερ, φάκελο, όνομα αρχείου στο χώρο αποθήκευσης της όπου διατηρείται εισόδου και εξόδου δεδομένων.   

### <a name="create-input-dataset"></a>Δημιουργία συνόλου δεδομένων εισαγωγής
Σε αυτό το βήμα, δημιουργείτε του συνόλου δεδομένων εισόδου για την αναπαράσταση εισαγωγής δεδομένων που είναι αποθηκευμένα στο το χώρο αποθήκευσης αντικειμένων Blob του Azure.

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.

        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν το σύνολο δεδομένων δημιουργήθηκε με επιτυχία, δείτε το JSON για το σύνολο δεδομένων από τα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Δημιουργία συνόλου δεδομένων εξόδου
Σε αυτό το βήμα, δημιουργείτε το σύνολο δεδομένων εξόδου για να απεικονίσετε δεδομένα εξόδου που είναι αποθηκευμένα σε το χώρο αποθήκευσης αντικειμένων Blob του Azure.

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.

        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν το σύνολο δεδομένων δημιουργήθηκε με επιτυχία, δείτε το JSON για το σύνολο δεδομένων από τα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Δημιουργία διοχέτευσης
Σε αυτό το βήμα, μπορείτε να δημιουργήσετε το πρώτο διοχέτευσης με μια δραστηριότητα **HDInsightHive** . Εισαγωγής φέτα είναι διαθέσιμη κάθε μήνα (συχνότητα: μήνα, διάστημα: 1), φέτα εξόδου παράγεται μηνιαία και είναι επίσης να ορίσετε την ιδιότητα χρονοδιαγράμματος για τη δραστηριότητα σε μηνιαία. Πρέπει να συμφωνεί με τις ρυθμίσεις για το σύνολο δεδομένων εξόδου και το Χρονοδιάγραμμα δραστηριότητας. Προς το παρόν, σύνολο δεδομένων εξόδου είναι τι κατευθύνει το χρονοδιάγραμμα, ώστε να πρέπει να δημιουργήσετε ένα σύνολο δεδομένων εξόδου ακόμα και αν τη δραστηριότητα δεν παράγει κανένα αποτέλεσμα. Εάν η δραστηριότητα μην απαιτηθεί οποιαδήποτε εισαγωγή, μπορείτε να παραλείψετε τη δημιουργία του συνόλου δεδομένων εισόδου.  

Επιβεβαιώστε ότι μπορείτε να δείτε το αρχείο **input.log** στο φάκελο **adfgetstarted/inputdata** στο το χώρο αποθήκευσης αντικειμένων blob του Azure και εκτελέστε την ακόλουθη εντολή για την ανάπτυξη της διοχέτευσης. Δεδομένου ότι έχουν οριστεί τις ώρες **έναρξης** και **λήξης** στο παρελθόν και **isPaused** έχει οριστεί σε false, της διοχέτευσης (δραστηριότητα στη διοχέτευση) εκτελείται αμέσως μετά την ανάπτυξη. 

1. Αντιστοιχίστε την εντολή μεταβλητή με το όνομα **cmd**.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Εκτελέστε την εντολή, χρησιμοποιώντας την **Εντολή Ενεργοποίηση**.

        $results = Invoke-Command -scriptblock $cmd;
3. Προβολή των αποτελεσμάτων. Εάν το σύνολο δεδομένων δημιουργήθηκε με επιτυχία, δείτε το JSON για το σύνολο δεδομένων από τα **αποτελέσματα**; Διαφορετικά, μπορείτε να δείτε ένα μήνυμα σφάλματος.  

        Write-Host $results
5. Συγχαρητήρια, έχετε δημιουργήσει με επιτυχία το πρώτο διοχέτευσης με χρήση του PowerShell Azure!

## <a name="monitor-pipeline"></a>Οθόνη διοχέτευσης
Σε αυτό το βήμα, μπορείτε να χρησιμοποιήσετε δεδομένα εργοστασίου REST API για την παρακολούθηση της φέτες που δημιουργήθηκαν με τη διαδικασία.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Η δημιουργία ένα σύμπλεγμα HDInsight σε ζήτηση συνήθως χρειάζεται κάποια στιγμή (περίπου 20 λεπτά). Επομένως, περιμένετε τη διαδικασία για να **περιέχει έως και 30 λεπτά** για την επεξεργασία στη φέτα.  

Εκτελέστε την εντολή ενεργοποίηση και του επόμενου μέχρι να δείτε τη φέτα **έτοιμοι** κράτος **απέτυχε** ή. Όταν στη φέτα βρίσκεται σε κατάσταση είστε έτοιμοι, ελέγξτε το φάκελο **partitioneddata** στο κοντέινερ **adfgetstarted** στο χώρο αποθήκευσης αντικειμένων blob του για τα δεδομένα εξόδου.  Τη δημιουργία ένα σύμπλεγμα HDInsight σε ζήτηση διαρκεί συνήθως κάποιο χρονικό διάστημα.

![δεδομένα εξόδου](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Το αρχείο εισαγωγής λαμβάνει διαγραφεί κατά τη φέτα υποβάλλεται σε επεξεργασία με επιτυχία. Επομένως, εάν θέλετε να εκτελέσετε ξανά στη φέτα ή κάντε ξανά το πρόγραμμα εκμάθησης, στείλτε το αρχείο εισόδου (input.log) στο φάκελο inputdata του κοντέινερ adfgetstarted.

Μπορείτε επίσης να χρησιμοποιήσετε Azure πύλη για την παρακολούθηση φέτες και την αντιμετώπιση τυχόν προβλημάτων. Ανατρέξτε στο θέμα [Παρακολούθηση αγωγούς με Azure πύλη](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) λεπτομέρειες.  

## <a name="summary"></a>Σύνοψη 
Σε αυτό το πρόγραμμα εκμάθησης, δημιουργήσατε ένα εργοστασίου Azure δεδομένων την επεξεργασία δεδομένων, εκτελώντας Hive δέσμης ενεργειών σε ένα σύμπλεγμα hadoop HDInsight. Χρησιμοποιήσατε το πρόγραμμα επεξεργασίας εργοστασίου δεδομένων στην πύλη του Azure για να εκτελέσετε τα παρακάτω βήματα:  

1.  Δημιουργία μιας Azure **εργοστασίου δεδομένων**.
2.  Δημιουργία δύο **συνδεδεμένες υπηρεσίες**:
    1.  Υπηρεσία **Αποθήκευσης azure** συνδεδεμένες για να συνδέσετε το χώρο αποθήκευσης αντικειμένων blob του Azure που περιέχει τα αρχεία εισόδου/εξόδου για την προέλευση δεδομένων.
    2.  **Azure HDInsight** σε ζήτηση συνδεδεμένων υπηρεσία για να συνδέσετε ένα σύμπλεγμα HDInsight Hadoop σε ζήτηση με την προέλευση δεδομένων. Azure εργοστασίου δεδομένων δημιουργεί μια HDInsight Hadoop σύμπλεγμα απλώς-σε-ώρα για την επεξεργασία δεδομένων εισόδου και να παραγάγετε δεδομένα εξόδου. 
3.  Δημιουργία δύο **συνόλων δεδομένων**, που περιγράφουν δεδομένα εισόδου και εξόδου για ομάδα HDInsight δραστηριότητα στη διοχέτευση. 
4.  Δημιουργία μιας **διοχέτευσης** με μια δραστηριότητα **HDInsight Hive** . 

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο, έχετε δημιουργήσει μια διαδικασία με μια δραστηριότητα μετασχηματισμού (HDInsight δραστηριότητα) που εκτελεί μια δέσμη ενεργειών της ομάδας σε ένα σύμπλεγμα Azure HDInsight στη ζήτηση. Για να δείτε πώς μπορείτε να χρησιμοποιήσετε μια δραστηριότητα Αντιγραφή για να αντιγράψετε δεδομένα από ένα αντικειμένων Blob του Azure Azure SQL, ανατρέξτε στο θέμα [πρόγραμμα εκμάθησης: αντιγράψτε δεδομένα από μια αντικειμένων Blob του Azure Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Δείτε επίσης
| Το θέμα | Περιγραφή |
| :---- | :---- |
| [Δεδομένα εργοστασίου REST API αναφοράς](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Ανατρέξτε στην τεκμηρίωση ολοκληρωμένη σχετικά cmdlet του εργοστασίου δεδομένων |
| [Δραστηριότητες μετασχηματισμού δεδομένων](data-factory-data-transformation-activities.md) | Σε αυτό το άρθρο παρέχει μια λίστα των δραστηριοτήτων μετασχηματισμό δεδομένων (όπως το HDInsight Hive μετασχηματισμό που χρησιμοποιείται σε αυτό το πρόγραμμα εκμάθησης) υποστηρίζονται από το Azure εργοστασίου δεδομένων. |
| [Προγραμματισμός και εκτέλεσης](data-factory-scheduling-and-execution.md) | Σε αυτό το άρθρο εξηγεί τις προγραμματισμού και εκτέλεσης πτυχές της εργοστασίου δεδομένων Azure μοντέλο εφαρμογών. |
| [Αγωγούς](data-factory-create-pipelines.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε αγωγούς και δραστηριοτήτων στις εργοστασιακές δεδομένων Azure και πώς να τους χρησιμοποιήσετε για να δημιουργήσετε ολοκληρωμένες να βασίζονται σε δεδομένα ροές εργασίας για το σενάριο ή επιχειρήσεις. |
| [Σύνολα δεδομένων](data-factory-create-datasets.md) | Σε αυτό το άρθρο σάς βοηθά να κατανοήσετε συνόλων δεδομένων στην προέλευση δεδομένων Azure.
| [Παρακολούθηση και διαχείριση αγωγούς χρησιμοποιώντας Azure λεπίδες πύλης](data-factory-monitor-manage-pipelines.md) | Σε αυτό το άρθρο περιγράφει τον τρόπο για την παρακολούθηση, διαχείριση και εντοπισμός σφαλμάτων σας αγωγούς χρησιμοποιώντας Azure λεπίδες πύλης. |
| [Παρακολούθηση και διαχείριση αγωγούς χρήση της εφαρμογής παρακολούθησης](data-factory-monitor-manage-app.md) | Σε αυτό το άρθρο περιγράφει τον τρόπο για την παρακολούθηση, διαχείριση και εντοπισμός σφαλμάτων αγωγούς με την παρακολούθηση & Διαχείριση εφαρμογών. 


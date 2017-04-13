<properties
   pageTitle="Αποστολή μεγάλες ποσότητες δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων χωρίς σύνδεση μεθόδους | Microsoft Azure"
   description="Χρησιμοποιήστε το εργαλείο AdlCopy για να αντιγράψετε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης δεδομένων λίμνης"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Χρήση υπηρεσίας εισαγωγή-εξαγωγή Azure για το αντίγραφο χωρίς σύνδεση δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης

Σε αυτό το άρθρο, θα μάθετε σχετικά με την αντιγραφή τεράστιες σύνολα δεδομένων (> 200GB) σε ένα χώρο αποθήκευσης λίμνης του Azure δεδομένων, χρησιμοποιώντας μεθόδους αντίγραφο εργασίας χωρίς σύνδεση, όπως [Η υπηρεσία εισαγωγής/εξαγωγής Azure](../storage/storage-import-export-service.md). Συγκεκριμένα, το αρχείο που χρησιμοποιείται ως παράδειγμα σε αυτό το άρθρο είναι 339,420,860,416 byte δηλαδή περίπου 319GB στο δίσκο. Ας κλήση 319GB.tsv σε αυτό το αρχείο.

Azure Service εισαγωγή/εξαγωγή σάς επιτρέπει να μεταφέρετε με ασφάλεια μεγάλες ποσότητες δεδομένων με το χώρο αποθήκευσης αντικειμένων blob του Azure μέσω αποστολής μονάδες σκληρού δίσκου, σε ένα κέντρο δεδομένων Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Προτού ξεκινήσετε σε αυτό το άρθρο, πρέπει να έχετε τα εξής:

- **Azure μια συνδρομή**. Ανατρέξτε στο θέμα [λήψη Azure δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/).

- **Λογαριασμός azure χώρου αποθήκευσης**.

- **Λογαριασμός azure δεδομένων λίμνης ανάλυσης (προαιρετικό)** - ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure δεδομένων λίμνης ανάλυσης](../data-lake-analytics/data-lake-analytics-get-started-portal.md) για οδηγίες σχετικά με τον τρόπο για να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης δεδομένων λίμνης.


## <a name="preparing-the-data"></a>Προετοιμασία των δεδομένων

Πριν να χρησιμοποιήσετε την υπηρεσία εισαγωγής/εξαγωγής, θα σας πρέπει να διακόψετε το αρχείο δεδομένων του για να μεταφερθεί **σε αντιγράφων που είναι μικρότερη από 200 GB με** μέγεθος. Αυτό συμβαίνει επειδή το εργαλείο εισαγωγής δεν λειτουργεί με τα αρχεία που είναι μεγαλύτερη από 200GB. Για να συμμορφώνεστε με αυτό, σε αυτό το πρόγραμμα εκμάθησης θα σας, διαιρέστε το αρχείο σε τμήματα των 100GB byte. Μπορείτε να το κάνετε εύκολα με χρήση [Cygwin](https://cygwin.com/install.html). Cygwin υποστηρίζει Linux εντολή η οποία επιτρέπει στους χρήστες να το κάνετε αυτό εύκολα. Για την περίπτωση, μπορούμε να χρησιμοποιήσουμε την ακόλουθη εντολή.

    split -b 100m 319GB.tsv

Η λειτουργία διαίρεσης δημιουργεί αρχεία με τα ονόματα των παρακάτω.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Προετοιμασία δίσκων με δεδομένα

Ακολουθήστε τις οδηγίες στην [Υπηρεσία εισαγωγής/εξαγωγής χρησιμοποιώντας Azure](../storage/storage-import-export-service.md) (κάτω από την ενότητα **Προετοιμασία μονάδες δίσκων σας** ) για να προετοιμάσετε το μονάδες σκληρού δίσκου. Παρακάτω θα δείτε τη συνολική ροή σχετικά με τον τρόπο για να προετοιμάσετε τη μονάδα δίσκου.

1. Προμήθεια σκληρό δίσκο που πληροί τις απαιτήσεις που θα χρησιμοποιηθεί για την υπηρεσία Auzre εισαγωγή/εξαγωγή.

2. Προσδιορίστε ένα λογαριασμό αποθήκευσης Azure όπου τα δεδομένα θα αντιγράψατε μία φορά εάν αποσταλεί με το Κέντρο Azure δεδομένων.

3. Χρησιμοποιήστε το [Εργαλείο εισαγωγής/εξαγωγής Azure](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), ένα βοηθητικό πρόγραμμα γραμμής εντολών. Ακολουθεί ένα δείγμα απόκομμα σχετικά με τη χρήση του εργαλείου.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Ανατρέξτε στο θέμα [Χρήση υπηρεσίας εισαγωγής/εξαγωγής Azure](../storage/storage-import-export-service.md) για περισσότερα τμήματα δείγμα σχετικά με τη χρήση του **Εργαλείου Azure εισαγωγή/εξαγωγή**.

4. Η παραπάνω εντολή δημιουργεί ένα μπλοκ σημειώσεων στην καθορισμένη θέση. Θα χρησιμοποιήσετε αυτό το αρχείο εγγραφών για να δημιουργήσετε μια εργασία εισαγωγής από την [Πύλη κλασική Azure](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Δημιουργήστε μια εργασία εισαγωγής

Τώρα, μπορείτε να δημιουργήσετε μια εργασία εισαγωγής χρησιμοποιώντας τις οδηγίες στην [Υπηρεσία εισαγωγής/εξαγωγής χρησιμοποιώντας Azure](../storage/storage-import-export-service.md) (στην ενότητα **Δημιουργία εργασίας εισαγωγής** ). Για αυτήν την εργασία εισαγωγής, με άλλες λεπτομέρειες, παρέχουν επίσης το αρχείο εγγραφών που δημιουργήσατε κατά την προετοιμασία του μονάδων δίσκου.

## <a name="physically-ship-the-disks"></a>Φυσικά αποστολή των δίσκων

Τώρα, μπορείτε να αποστείλετε φυσικά το δίσκων σε ένα κέντρο δεδομένων Azure όπου τα δεδομένα αντιγράφονται τα αντικείμενα BLOB αποθήκευσης Azure που παρείχατε κατά τη δημιουργία της εργασίας εισαγωγής. Επίσης, κατά τη δημιουργία της εργασίας, εάν έχετε επιλέξει να παρέχετε τις πληροφορίες παρακολούθησης αργότερα, μπορείτε πλέον να μεταβείτε ξανά στην εργασία σας εισαγωγή και ενημέρωση ο αριθμός παρακολούθησης.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Αντιγράψτε δεδομένα από αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης λίμνης δεδομένων Azure

Όταν η κατάσταση της εργασίας εισαγωγής εμφανίζει ολοκληρωμένες, μπορείτε να επαληθεύσετε εάν τα δεδομένα είναι διαθέσιμα στο τα αντικείμενα BLOB αποθήκευσης Azure που είχαν που καθορίζεται. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε διάφορες μεθόδους για να μετακινήσετε αυτά τα δεδομένα από τα αντικείμενα blob του Azure χώρου αποθήκευσης στο χώρο αποθήκευσης λίμνης δεδομένων Azure. Για όλες τις διαθέσιμες επιλογές για την αποστολή δεδομένων, ανατρέξτε στο θέμα [Ingesting δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

Σε αυτήν την ενότητα, σας παρέχουμε των ορισμών JSON που μπορείτε να χρησιμοποιήσετε για να δημιουργήσετε μια διοχέτευση Azure εργοστασίου δεδομένων για την αντιγραφή δεδομένων. Μπορείτε να χρησιμοποιήσετε αυτές τις ορισμών JSON από την [πύλη του Azure](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) ή [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) ή [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Προέλευση συνδεδεμένες υπηρεσίας (Azure χώρο αποθήκευσης αντικειμένων Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Στοχεύστε συνδεδεμένων υπηρεσιών (Azure δεδομένων λίμνης Store)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Εισαγωγή συνόλου δεδομένων
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Σύνολο δεδομένων εξόδου
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Διοχέτευση (αντιγραφή δραστηριότητα)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Για περισσότερες πληροφορίες σχετικά με τη χρήση Azure εργοστασίου δεδομένων για τη μετακίνηση δεδομένων από το Azure χώρο αποθήκευσης Blob και το χώρο αποθήκευσης λίμνης Azure δεδομένων, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων από το Azure χώρο αποθήκευσης αντικειμένων Blob χρησιμοποιώντας εργοστασίου δεδομένων Azure χώρο αποθήκευσης λίμνης δεδομένων Azure](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Την αναδομήσετε τα αρχεία δεδομένων στο χώρο αποθήκευσης λίμνης δεδομένων Azure

Θα σας ξεκινήσει με ένα αρχείο που έχει 319GB στο μέγεθος και είχε το σε αρχεία με μικρότερο μέγεθος, έτσι ώστε να μπορούν να μεταφερθούν με την υπηρεσία Azure εισαγωγή/εξαγωγή. Τώρα που τα δεδομένα βρίσκονται στο χώρο αποθήκευσης λίμνης Azure δεδομένων, μπορούμε reconstrcut το αρχείο στο αρχικό του μέγεθος. Μπορείτε να χρησιμοποιήσετε την παρακάτω cmldts Azure PowerShell για να το κάνετε.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Επόμενα βήματα

- [Διασφάλιση των δεδομένων στο χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-secure-data.md)
- [Χρήση ανάλυσης λίμνης Azure δεδομένων με το χώρο αποθήκευσης δεδομένων λίμνης](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Χρήση Azure HDInsight με το χώρο αποθήκευσης δεδομένων λίμνης](data-lake-store-hdinsight-hadoop-use-portal.md)

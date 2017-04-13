<properties
   pageTitle="Φόρτωση των δεδομένων με εργοστασίου δεδομένων Azure | Microsoft Azure"
   description="Μάθετε πώς να φορτώσετε τα δεδομένα με εργοστασίου δεδομένων Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Φόρτωση των δεδομένων με εργοστασίου Azure δεδομένων 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Προέλευση δεδομένων](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Αυτό το πρόγραμμα εκμάθησης δείχνει πώς μπορείτε να δημιουργήσετε μια διοχέτευση του Azure εργοστασίου δεδομένων για να μεταφέρετε δεδομένα από το Azure χώρο αποθήκευσης Blob αποθήκη δεδομένων του SQL Azure. Με τα παρακάτω βήματα θα πρέπει:

+ Ρυθμίστε το δείγμα δεδομένων σε ένα αντικειμένων Blob του Azure χώρου αποθήκευσης.
+ Σύνδεση πόροι εργοστασίου δεδομένων Azure.
+ Δημιουργήστε μια διαδικασία για να μεταφέρετε δεδομένα από το χώρο αποθήκευσης αντικειμένων blob αποθήκη δεδομένων του SQL.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Για να εξοικειωθείτε με το Azure εργοστασίου δεδομένων, ανατρέξτε στο θέμα [Εισαγωγή στις εργοστασιακές Azure δεδομένων][].

### <a name="create-or-identify-resources"></a>Δημιουργία ή προσδιορίστε τους πόρους

Πριν να ξεκινήσετε αυτό το πρόγραμμα εκμάθησης, πρέπει να έχετε τους παρακάτω πόρους:

   + **Azure χώρο αποθήκευσης Blob**: αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί Azure χώρο αποθήκευσης Blob ως προέλευση δεδομένων για τη διαδικασία Azure εργοστασίου δεδομένων και πρέπει να έχετε μία διαθέσιμη για την αποθήκευση του δείγματος δεδομένων. Εάν δεν έχετε ήδη, μάθετε πώς μπορείτε να [Δημιουργία λογαριασμού χώρου αποθήκευσης][].

   + **Αποθήκη δεδομένων του SQL**: αυτό το πρόγραμμα εκμάθησης μετακινείται τα δεδομένα από το Azure χώρο αποθήκευσης Blob αποθήκη δεδομένων του SQL και, συνεπώς πρέπει να έχετε μια αποθήκη δεδομένων online που φορτώνεται με το δείγμα δεδομένων AdventureWorksDW. Εάν δεν έχετε ήδη μια αποθήκη δεδομένων, μάθετε τον τρόπο [παροχής μία][Create a SQL Data Warehouse]. Εάν έχετε μια αποθήκη δεδομένων, αλλά δεν έχετε την προμήθεια με το δείγμα δεδομένων, μπορείτε να το [φορτώσετε με μη αυτόματο τρόπο][Load sample data into SQL Data Warehouse].

   + **Azure εργοστασίου δεδομένων**: εργοστασίου δεδομένων Azure ολοκληρώνει την πραγματική φόρτωση και πρέπει να έχετε μία που μπορείτε να χρησιμοποιήσετε για τη δημιουργία της διοχέτευσης κίνηση δεδομένων. Εάν δεν έχετε ήδη, μάθετε πώς μπορείτε να δημιουργήσετε ένα στο βήμα 1 του [Γρήγορα αποτελέσματα με το Azure εργοστασίου δεδομένων (πρόγραμμα επεξεργασίας εργοστασίου δεδομένων)][].

   + **AZCopy**: χρειάζεστε AZCopy για να αντιγράψετε το δείγμα δεδομένων από τον τοπικό υπολογιστή-πελάτη σας αντικειμένων Blob του Azure χώρου αποθήκευσης. Για οδηγίες εγκατάστασης, ανατρέξτε στην [τεκμηρίωση AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Βήμα 1: Αντιγραφή δείγμα δεδομένων σε Azure χώρο αποθήκευσης Blob

Όταν έχετε όλα τα τμήματα που είστε έτοιμοι, είστε έτοιμοι για να αντιγράψετε το δείγμα δεδομένων σας αντικειμένων Blob του Azure χώρου αποθήκευσης.

1. [Λήψη δείγματος δεδομένων][]. Αυτά τα δεδομένα προσθέτει μια άλλη τρία έτη με δεδομένα πωλήσεων, το δείγμα δεδομένων AdventureWorksDW.

2. Χρησιμοποιήστε αυτή την εντολή AZCopy για να αντιγράψετε τα τρία έτη των δεδομένων σας αντικειμένων Blob του Azure χώρου αποθήκευσης.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Βήμα 2: Σύνδεση πόρους για να εργοστασίου δεδομένων Azure

Τώρα που τα δεδομένα είναι σε θέση μπορούμε να δημιουργήσουμε της διοχέτευσης Azure εργοστασίου δεδομένων για να μετακινήσετε τα δεδομένα από το χώρο αποθήκευσης αντικειμένων blob του Azure σε αποθήκη δεδομένων του SQL.

Για να ξεκινήσετε, ανοίξτε την [πύλη του Azure][] και επιλέξτε την προέλευση δεδομένων από το αριστερό μενού.

### <a name="step-21-create-linked-service"></a>Βήμα 2.1: Δημιουργία συνδεδεμένου υπηρεσίας

Σύνδεση το λογαριασμό Azure χώρου αποθήκευσης και αποθήκη δεδομένων του SQL για την προέλευση δεδομένων.  

1. Πρώτα, ξεκινήστε τη διαδικασία εγγραφής, κάνοντας κλικ στην ενότητα "Συνδεδεμένες υπηρεσίες" του εργοστασίου δεδομένων σας και, στη συνέχεια, κάντε κλικ στην επιλογή "νέα δεδομένα αποθηκεύουν." Επιλέξτε ένα όνομα για να καταχωρήσετε του azure χώρου αποθήκευσης στην περιοχή, επιλέξτε αποθήκευσης Azure ως τον τύπο σας και, στη συνέχεια, πληκτρολογήστε το όνομα λογαριασμού και το κλειδί λογαριασμού.

2. Για να καταχωρήσετε αποθήκη δεδομένων του SQL μεταβείτε "Συντάκτη και ανάπτυξη" ενότητα, επιλέξτε 'Δημιουργία χώρου αποθήκευσης δεδομένων' και, στη συνέχεια 'Αποθήκη δεδομένων SQL Azure'. Αντιγραφή και επικόλληση σε αυτό το πρότυπο και, στη συνέχεια, συμπληρώστε τις συγκεκριμένες πληροφορίες.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Βήμα 2.2: Ορισμός του συνόλου δεδομένων

Μετά τη δημιουργία των συνδεδεμένων υπηρεσιών, θα πρέπει να ορίσετε τα σύνολα δεδομένων.  Δείτε αυτό σημαίνει ότι τον ορισμό της δομής των δεδομένων που μετακινείται από το χώρο αποθήκευσης με την αποθήκη δεδομένων σας.  Μπορείτε να διαβάσετε περισσότερα σχετικά με τη δημιουργία

1. Ξεκινήστε τη διαδικασία, μεταβαίνοντας στην ενότητα "Συντάκτη και ανάπτυξη" της εργοστασίου δεδομένων σας.

2. Κάντε κλικ στην επιλογή 'Νέο σύνολο δεδομένων' και στη συνέχεια, 'Χώρος αποθήκευσης αντικειμένων Blob του Azure' για να συνδέσετε το χώρο αποθήκευσης με την προέλευση δεδομένων.  Μπορείτε να χρησιμοποιήσετε την παρακάτω δέσμη ενεργειών για να ορίσετε τα δεδομένα σας στο χώρο αποθήκευσης αντικειμένων Blob του Azure:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Τώρα θα σας να ορίσετε το σύνολο δεδομένων για αποθήκη δεδομένων του SQL. Ξεκινήσουμε με τον ίδιο τρόπο, κάνοντας κλικ στην επιλογή 'Νέο σύνολο δεδομένων' και, στη συνέχεια, 'Αποθήκη δεδομένων SQL Azure'.

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Βήμα 3: Δημιουργία και εκτέλεση του διοχέτευσης

Τέλος, θα σας ρύθμιση και εκτέλεση της διοχέτευσης στο Azure εργοστασίου δεδομένων.  Αυτή είναι η λειτουργία που ολοκληρώνει την κυκλοφορία πραγματικά δεδομένα.  Μπορείτε να βρείτε μια πλήρη προβολή από τις εργασίες που μπορείτε να ολοκληρώσετε με αποθήκη δεδομένων του SQL και εργοστασίου δεδομένων Azure [εδώ][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Στην ενότητα "Συντάκτη και ανάπτυξη", κάντε κλικ στην επιλογή 'Περισσότερες εντολές' και, στη συνέχεια, 'Νέα διαδικασία'.  Αφού δημιουργήσετε τη διαδικασία, μπορείτε να χρησιμοποιήσετε την παρακάτω κώδικα για να μεταφέρετε τα δεδομένα σας αποθήκη δεδομένων:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα, ξεκινήστε με προβολή:

- [Διαδρομή εκμάθησης azure εργοστασίου δεδομένων][].
- [Δεδομένων azure SQL αποθήκης γραμμή σύνδεσης][]. Αυτό είναι το θέμα πυρήνα αναφοράς για τη χρήση εργοστασίου δεδομένων Azure με αποθήκη δεδομένων του SQL Azure.


Αυτά τα θέματα παρέχουν λεπτομερείς πληροφορίες σχετικά με την προέλευση δεδομένων Azure. Που συζητούν βάση δεδομένων SQL Azure ή HDInsight, αλλά οι πληροφορίες ισχύουν επίσης για αποθήκη δεδομένων του SQL Azure.

- [Πρόγραμμα εκμάθησης: γρήγορα αποτελέσματα με το Azure δεδομένων εργοστασίου][] Αυτό είναι το πρόγραμμα εκμάθησης πυρήνα για την επεξεργασία δεδομένων με το Azure εργοστασίου δεδομένων. Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να δημιουργήσετε το πρώτο διοχέτευσης που χρησιμοποιεί το HDInsight να μετατρέψετε και να αναλύσετε αρχεία καταγραφής web σε μηνιαία βάση. Σημειώστε ότι υπάρχει δραστηριότητα Αντιγραφή σε αυτό το πρόγραμμα εκμάθησης.
- [Πρόγραμμα εκμάθησης: αντιγράψτε δεδομένα από το Azure χώρο αποθήκευσης Blob σε βάση δεδομένων SQL Azure][]. Σε αυτό το πρόγραμμα εκμάθησης, θα δημιουργήσετε μια διοχέτευση του Azure εργοστασίου δεδομένων για να αντιγράψετε δεδομένα από το Azure χώρο αποθήκευσης Blob με βάση δεδομένων SQL Azure.

<!--Image references-->

<!--Article references-->
[Τεκμηρίωση AZCopy]: ../storage/storage-use-azcopy.md
[Γραμμή σύνδεσης αποθήκη δεδομένων Azure SQL]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Δημιουργία λογαριασμού χώρου αποθήκευσης]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Γρήγορα αποτελέσματα με το Azure εργοστασίου δεδομένων (πρόγραμμα επεξεργασίας εργοστασίου δεδομένων)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Εισαγωγή στις εργοστασιακές Azure δεδομένων]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Πρόγραμμα εκμάθησης: Αντιγράψτε δεδομένα από το Azure χώρο αποθήκευσης Blob σε βάση δεδομένων SQL Azure]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Πρόγραμμα εκμάθησης: Γρήγορα αποτελέσματα με το Azure εργοστασίου δεδομένων]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Διαδρομή εκμάθησης Azure εργοστασίου δεδομένων]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Πύλη του Azure]: https://portal.azure.com
[Λήψη δείγματος δεδομένων]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv

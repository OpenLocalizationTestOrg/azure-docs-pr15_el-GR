### <a name="type-conversion-sample"></a>Δείγμα μετατροπής τύπου
Το παρακάτω παράδειγμα είναι για την αντιγραφή δεδομένων από ένα Blob στο Azure SQL με μετατροπής τύπων.

Ας υποθέσουμε ότι το dataset Blob είναι σε μορφή CSV και περιέχει 3 στήλες. Μία από αυτές είναι μια στήλη ημερομηνίας/ώρας με ένα προσαρμοσμένο datetime μορφή χρησιμοποιώντας Γαλλικά συντομογραφίες για την ημέρα της εβδομάδας.

Θα μπορείτε να ορίσετε το σύνολο δεδομένων προέλευσης Blob ως εξής μαζί με ορισμοί τύπων για τις στήλες.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
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

Δεδομένο το SQL τύπος .NET τύπος αντιστοίχισης παραπάνω πίνακα που θα ορίσετε τον πίνακα Azure SQL με το παρακάτω σχήμα.

| Όνομα στήλης | Τύπος SQL |
| ----------- | -------- |
| αναγνωριστικό χρήστη | bigint |
| Όνομα | κείμενο |
| lastlogindate | ημερομηνίας και ώρας |

Στη συνέχεια θα μπορείτε να ορίσετε το σύνολο δεδομένων Azure SQL ως εξής. Σημείωση: Δεν χρειάζεται για να καθορίσετε την ενότητα "Δομή" με πληροφορίες για τον τύπο, επειδή οι πληροφορίες τύπου ήδη που καθορίζεται στο χώρο αποθήκευσης της βάσης δεδομένων.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Σε αυτήν την περίπτωση εργοστασιακές δεδομένων θα κάνουν αυτόματα τον τύπο μετατροπών, συμπεριλαμβανομένου του πεδίου ημερομηνίας/ώρας με το προσαρμοσμένο datetime μορφή χρησιμοποιώντας την κουλτούρα fr-fr κατά τη μετακίνηση δεδομένων από Blob Azure SQL.



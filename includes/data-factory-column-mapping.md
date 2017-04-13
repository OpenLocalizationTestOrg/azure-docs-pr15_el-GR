## <a name="column-mapping-with-translator-rules"></a>Αντιστοίχιση στηλών με κανόνες μεταφραστή
Αντιστοίχιση στηλών μπορεί να χρησιμοποιηθεί για να καθορίσετε τον τρόπο στηλών που καθορίζονται στη "Δομή" Χάρτης πίνακα προέλευσης σε στήλες που καθορίζεται στη "Δομή" δέκτη πίνακα. Η ιδιότητα **αντιστοίχιση στηλών** είναι διαθέσιμη στην ενότητα **typeProperties** της δραστηριότητας αντίγραφο.

Αντιστοίχιση στηλών υποστηρίζει τα ακόλουθα σενάρια:

- Όλες οι στήλες του πίνακα προέλευσης "Δομή" έχουν αντιστοιχιστεί σε όλες τις στήλες του πίνακα δέκτη "Δομή".
- Ένα υποσύνολο των στηλών στον πίνακα προέλευσης "Δομή" έχουν αντιστοιχιστεί σε όλες τις στήλες του πίνακα δέκτη "Δομή".

Τα παρακάτω είναι συνθήκες σφάλματος και θα έχει ως αποτέλεσμα μια εξαίρεση:

- Λιγότερες στήλες ή περισσότερες στήλες στη "Δομή" δέκτη πίνακα από που καθορίζεται στην αντιστοίχιση.
- Διπλά αντιστοίχιση.
- Αποτέλεσμα του ερωτήματος SQL δεν διαθέτει ένα όνομα στήλης που καθορίζεται στην αντιστοίχιση.

## <a name="column-mapping-samples"></a>Δείγματα αντιστοίχισης στήλης
> [AZURE.NOTE] Στα παρακάτω παραδείγματα είναι για Azure SQL και αντικειμένων Blob του Azure αλλά ισχύουν για οποιαδήποτε χώρου αποθήκευσης δεδομένων που υποστηρίζει ορθογώνια συνόλων δεδομένων. Θα πρέπει να προσαρμόσετε συνόλου δεδομένων και των ορισμών συνδεδεμένων υπηρεσιών σε παραδείγματα κάτω από ώστε να οδηγεί στα δεδομένα στην προέλευση δεδομένων σχετικές. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Δείγμα 1 – στήλη αντιστοίχιση από Azure SQL σε αντικειμένων blob του Azure
Σε αυτό το δείγμα, ο πίνακας εισαγωγής έχει δομή και να οδηγεί σε έναν πίνακα SQL σε μια βάση δεδομένων Azure SQL.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Σε αυτό το δείγμα, ο πίνακας εξόδου έχει δομή και να δείχνει ένα blob στο χώρο αποθήκευσης αντικειμένων blob του Azure.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
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
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Εμφανίζεται κάτω από το JSON για τη δραστηριότητα. Οι στήλες από πηγή που έχουν αντιστοιχιστεί σε στήλες σε δέκτη (**columnMappings**), χρησιμοποιώντας την ιδιότητα **μεταφραστή** .

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Ροή αντιστοίχισης στήλης:**

![Στήλη αντιστοίχισης ροής](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Δείγμα 2 – στήλη αντιστοίχιση με το ερώτημα SQL από Azure SQL να αντικειμένων blob του Azure
Σε αυτό το δείγμα, ένα ερώτημα SQL χρησιμοποιείται για να εξαγάγετε δεδομένα από το Azure SQL αντί απλώς που καθορίζει το όνομα του πίνακα και τα ονόματα των στηλών στην ενότητα "Δομή". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

Σε αυτήν την περίπτωση, τα αποτελέσματα του ερωτήματος έχουν αντιστοιχιστεί πρώτα σε στήλες που καθορίζεται στο "Δομή" του αρχείου προέλευσης. Στη συνέχεια, στις στήλες από προέλευσης "Δομή" έχουν αντιστοιχιστεί στήλες σε δέκτη "Δομή" με κανόνες που καθορίζεται στο columnMappings.  Ας υποθέσουμε ότι το ερώτημα επιστρέφει 5 στήλες, δύο επιπλέον στήλες, στη συνέχεια, αυτές που καθορίζονται στη "Δομή" προέλευση.

**Στήλη αντιστοίχισης ροής**

![Αντιστοίχιση στήλης ροής-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)








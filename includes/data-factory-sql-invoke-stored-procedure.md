## <a name="invoking-stored-procedure-for-sql-sink"></a>Κλήση αποθηκευμένη διαδικασία για δέκτη SQL

Κατά την αντιγραφή δεδομένων στο SQL Server ή σε βάση δεδομένων SQL/SQL Server Azure, ένας χρήστης που καθορίζεται αποθηκευμένη διαδικασία θα μπορούσε να είναι έχει ρυθμιστεί και κλήση με πρόσθετες παραμέτρους. 

Μια αποθηκευμένη διαδικασία μπορούν να χρησιμοποιηθούν κατά την ενσωματωμένη αντίγραφο μηχανισμούς δεν εξυπηρετεί το σκοπό. Αυτό συνήθως είναι δυνατή κατά την επεξεργασία επιπλέον (συγχώνευση στηλών, αναζήτηση πρόσθετων τιμών, εισαγωγής σε πολλούς πίνακες...) πρέπει να γίνουν πριν από την τελική εισαγωγή των δεδομένων προέλευσης στον πίνακα προορισμού. 

Ενδέχεται να μπορείτε να ενεργοποιήσετε μια αποθηκευμένη διαδικασία της επιλογής. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε μια αποθηκευμένη διαδικασία για να κάνετε μια απλή εισαγωγής σε έναν πίνακα στη βάση δεδομένων. 

**Σύνολο δεδομένων εξόδου**

Σε αυτό το παράδειγμα, τύπος έχει οριστεί σε: SqlServerTable. Ρυθμίσετε να AzureSqlTable για χρήση με μια βάση δεδομένων Azure SQL. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Ορισμός της ενότητας SqlSink στο αντίγραφο δραστηριότητας JSON ως εξής. Για να καλέσετε μια αποθηκευμένη διαδικασία κατά την εισαγωγή δεδομένων, απαιτούνται ιδιότητες SqlWriterStoredProcedureName και SqlWriterTableType.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Στη βάση δεδομένων σας, ορίστε την αποθηκευμένη διαδικασία με το ίδιο όνομα ως SqlWriterStoredProcedureName. Χειρίζεται εισαγωγής δεδομένων από το καθορισμένο αρχείο προέλευσης και εισαγωγή στον πίνακα εξόδου. Παρατηρήστε ότι το όνομα της παραμέτρου της αποθηκευμένης διαδικασίας πρέπει να είναι το ίδιο με το όνομα πίνακα ορίζεται σε αρχείο JSON πίνακα.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Στο βάση δεδομένων σας, να καθορίσετε τον τύπο πίνακα με το ίδιο όνομα ως SqlWriterTableType. Παρατηρήστε ότι το σχήμα του τύπου πίνακα πρέπει να είναι ίδιο με το σχήμα που επιστρέφεται από τα δεδομένα εισόδου σας.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Η δυνατότητα αποθηκευμένη διαδικασία αξιοποιεί [Table-Valued παράμετροι](https://msdn.microsoft.com/library/bb675163.aspx).
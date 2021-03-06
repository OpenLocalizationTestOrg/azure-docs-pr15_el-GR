<properties 
    pageTitle="SQL Server αποθηκευμένη διαδικασία δραστηριότητας" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε τη δραστηριότητα αποθηκευμένη διαδικασία του SQL Server για να καλέσετε μια αποθηκευμένη διαδικασία σε μια βάση δεδομένων SQL Azure ή αποθήκη δεδομένων του SQL Azure από μια προέλευση δεδομένων διοχέτευσης." 
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
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server αποθηκευμένη διαδικασία δραστηριότητας
> [AZURE.SELECTOR]
[Ομάδα](data-factory-hive-activity.md)  
[Γουρούνι](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Ροή Hadoop](data-factory-hadoop-streaming-activity.md)
[Μηχανικής εκμάθησης](data-factory-azure-ml-batch-execution-activity.md) 
[Αποθηκευμένη διαδικασία](data-factory-stored-proc-activity.md)
[U-SQL ανάλυσης δεδομένων λίμνης](data-factory-usql-activity.md)
[.NET προσαρμοσμένο](data-factory-use-custom-activities.md)

Μπορείτε να χρησιμοποιήσετε τη δραστηριότητα SQL Server αποθηκευμένη διαδικασία σε μια προέλευση δεδομένων [διοχέτευσης](data-factory-create-pipelines.md) για να καλέσετε μια αποθηκευμένη διαδικασία με έναν από τους παρακάτω χώρους αποθήκευσης δεδομένων: 


- Βάση δεδομένων SQL Azure 
- Αποθήκη δεδομένων του Azure SQL  
- Βάση δεδομένων SQL Server στην εταιρεία σας ή μια Εικονική Azure. Πρέπει να εγκαταστήσετε την πύλη διαχείρισης δεδομένων στον ίδιο υπολογιστή που φιλοξενεί τη βάση δεδομένων ή σε νέο υπολογιστή για να αποφύγετε ανταγωνισμό για πόρους με τη βάση δεδομένων. Η πύλη διαχείρισης δεδομένων είναι ένα λογισμικό που συνδέεται εσωτερικής προελεύσεις/δεδομένων προελεύσεις δεδομένων που hosed στο ΣΠΣ Azure με υπηρεσίες cloud με τον τρόπο ασφαλή και διαχειριζόμενα. Δείτε το άρθρο [Μετακίνηση δεδομένων μεταξύ της εσωτερικής εγκατάστασης και cloud](data-factory-move-data-between-onprem-and-cloud.md) για λεπτομέρειες σχετικά με την πύλη διαχείρισης δεδομένων. 

Σε αυτό το άρθρο δημιουργεί στο το άρθρο [δραστηριότητες μετασχηματισμό δεδομένων](data-factory-data-transformation-activities.md) , το οποίο παρουσιάζει μια γενική επισκόπηση του μετασχηματισμού δεδομένων και τις δραστηριότητες υποστηριζόμενες μετασχηματισμό.

## <a name="walkthrough"></a>Αναλυτικές οδηγίες

### <a name="sample-table-and-stored-procedure"></a>Δείγμα πίνακα και αποθηκευμένη διαδικασία
1. Δημιουργήστε τον παρακάτω **πίνακα** σε χρησιμοποιώντας το SQL Server Management Studio ή οποιοδήποτε άλλο εργαλείο που είστε εξοικειωμένοι με την βάση δεδομένων SQL Azure. Η στήλη datetimestamp είναι η ημερομηνία και ώρα όταν δημιουργείται το αντίστοιχο Αναγνωριστικό. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Αναγνωριστικό είναι το μοναδικό που έχουν προσδιοριστεί και τη στήλη datetimestamp είναι η ημερομηνία και ώρα όταν δημιουργείται το αντίστοιχο Αναγνωριστικό.
    ![Δείγμα δεδομένων](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Αυτό το δείγμα χρησιμοποιεί βάση δεδομένων SQL Azure αλλά λειτουργεί με τον ίδιο τρόπο για την αποθήκη δεδομένων του SQL Azure και βάση δεδομένων SQL Server. 
2. Δημιουργήστε την παρακάτω **αποθηκευμένη διαδικασία** που εισάγει δεδομένα σε για να το **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Όνομα** και **περίβλημα** της παραμέτρου (DateTime σε αυτό το παράδειγμα) πρέπει να συμφωνεί με εκείνη της παραμέτρου που καθορίζεται στη διοχέτευση/δραστηριότητα JSON. Στον ορισμό αποθηκευμένη διαδικασία, βεβαιωθείτε ότι **@** χρησιμοποιείται ως ένα πρόθεμα για την παράμετρο.
    
### <a name="create-a-data-factory"></a>Δημιουργία μιας προέλευσης δεδομένων  
4. Συνδεθείτε στην [πύλη του Azure](https://portal.azure.com/). 
5. Κάντε κλικ στην επιλογή **ΔΗΜΙΟΥΡΓΊΑ** από το αριστερό μενού, κάντε κλικ στην επιλογή **ευφυΐας + ανάλυση**και επιλέξτε **Εργοστασίου δεδομένων**.
    
    ![Νέα προέλευση δεδομένων](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Στο blade τη **νέα προέλευση δεδομένων** , πληκτρολογήστε **SProcDF** για το όνομα. Azure ονομάτων εργοστασίου δεδομένων είναι **καθολικά μοναδικό**. Πρέπει να πρόθεμα το όνομα του εργοστασίου δεδομένων με το όνομά σας, για να ενεργοποιήσετε την επιτυχή δημιουργία του εργοστασίου.

    ![Νέα προέλευση δεδομένων](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Επιλέξτε το **Azure συνδρομής**. 
4.  Για την **Ομάδα πόρων**, κάντε ένα από τα παρακάτω βήματα: 
    1.  Κάντε κλικ στην επιλογή **Δημιουργία νέου** και πληκτρολογήστε ένα όνομα για την ομάδα των πόρων.
    2.  Κάντε κλικ στην επιλογή **Χρήση υπάρχουσας** και επιλέξτε μια υπάρχουσα ομάδα πόρων.  
5.  Επιλέξτε τη **θέση** για την προέλευση δεδομένων.
6.  Επιλέξτε **Καρφίτσωμα στον πίνακα εργαλείων** , έτσι ώστε να βλέπετε την προέλευση δεδομένων στον πίνακα εργαλείων επόμενη φορά που θα συνδεθείτε. 
6.  Κάντε κλικ στην εντολή **Δημιουργία** blade τη **νέα προέλευση δεδομένων** .
6.  Μπορείτε να δείτε την προέλευση δεδομένων που δημιουργούνται στον πίνακα **εργαλείων** της πύλης Azure. Μετά την προέλευση δεδομένων έχει δημιουργηθεί με επιτυχία, μπορείτε να δείτε σελίδας εργοστασίου δεδομένων, η οποία εμφανίζονται τα περιεχόμενα του εργοστασίου δεδομένων.
    ![Αρχική σελίδα εργοστασίου δεδομένων](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Δημιουργία μιας υπηρεσίας συνδεδεμένες SQL Azure  
Αφού δημιουργήσετε την προέλευση δεδομένων, μπορείτε να δημιουργήσετε μια υπηρεσία συνδεδεμένες SQL Azure που συνδέει τη βάση δεδομένων SQL Azure με την προέλευση δεδομένων. Αυτή η βάση δεδομένων περιέχει το sampletable πίνακα και sp_sample αποθηκευμένη διαδικασία.

7.  Κάντε κλικ στην επιλογή **συντάκτη και ανάπτυξη** στην το blade **Εργοστασίου δεδομένων** για **SProcDF** εκκίνηση του προγράμματος επεξεργασίας εργοστασίου δεδομένων.
2.  Κάντε κλικ στην επιλογή **Αποθήκευση νέα δεδομένα** στη γραμμή εντολών και επιλέξτε **Βάση δεδομένων SQL Azure**. Θα πρέπει να δείτε τη δέσμη ενεργειών JSON για τη δημιουργία μιας υπηρεσίας SQL Azure που συνδέονται στο πρόγραμμα επεξεργασίας. 

    ![Νέος χώρος αποθήκευσης δεδομένων](media/data-factory-stored-proc-activity/new-data-store.png)
4. Στη δέσμη ενεργειών JSON, κάντε τις ακόλουθες αλλαγές: 
    1. Αντικατάσταση ** &lt;όνομα_διακομιστή&gt; ** με το όνομα του διακομιστή βάσης δεδομένων SQL Azure.
    2. Αντικατάσταση ** &lt;όνομα βάσης δεδομένων&gt; ** με τη βάση δεδομένων στην οποία έχετε δημιουργήσει τον πίνακα και την αποθηκευμένη διαδικασία.
    3. Αντικατάσταση ** &lt; username@servername ** με το λογαριασμό χρήστη που έχει πρόσβαση στη βάση δεδομένων.
    4. Αντικατάσταση ** &lt;τον κωδικό πρόσβασης&gt; ** με τον κωδικό πρόσβασης για το λογαριασμό χρήστη. 

    ![Νέος χώρος αποθήκευσης δεδομένων](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Στη γραμμή εντολών για την ανάπτυξη της συνδεδεμένων υπηρεσίας, κάντε κλικ στην επιλογή **Ανάπτυξη** . Επιβεβαιώστε ότι μπορείτε να δείτε το AzureSqlLinkedService στην προβολή δέντρου στα αριστερά. 

    ![προβολή δέντρου με την υπηρεσία συνδεδεμένων](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Δημιουργία ενός συνόλου δεδομένων εξόδου
6. Κάντε κλικ στην επιλογή **... Περισσότερες** στη γραμμή εργαλείων, κάντε κλικ στην επιλογή **νέο σύνολο δεδομένων**και κάντε κλικ στην επιλογή **Azure SQL**. Το **νέο σύνολο δεδομένων** στη γραμμή εντολών και επιλέξτε **Azure SQL**.

    ![προβολή δέντρου με την υπηρεσία συνδεδεμένων](media/data-factory-stored-proc-activity/new-dataset.png)
7. Αντιγράψτε και επικολλήστε την ακόλουθη JSON δέσμη ενεργειών στο στο πρόγραμμα επεξεργασίας JSON.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εντολών για την ανάπτυξη του συνόλου δεδομένων. Επιβεβαιώστε ότι μπορείτε να δείτε το σύνολο δεδομένων στην προβολή δέντρου. 

    ![προβολή δέντρου με τις συνδεδεμένες υπηρεσίες](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Δημιουργήστε μια διαδικασία με SqlServerStoredProcedure δραστηριότητας
Τώρα, ας δημιουργήσουμε μια διαδικασία με μια δραστηριότητα SqlServerStoredProcedure.
 
9. Κάντε κλικ στην επιλογή **... Περισσότερες** στην εντολή γραμμή και κάντε κλικ στην επιλογή **νέα διοχέτευσης**. 
9. Αντιγράψτε και επικολλήστε το εξής τμήμα κώδικα JSON. Ορίστε το **storedProcedureName** σε **sp_sample**. Όνομα και περίβλημα της παραμέτρου **ημερομηνίας/ώρας** , πρέπει να συμφωνεί με το όνομα και τη περίβλημα της παραμέτρου στον ορισμό αποθηκευμένη διαδικασία.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Εάν χρειάζεστε περάσετε null για μια παράμετρο, χρησιμοποιήστε την εξής σύνταξη: "παράμετρος1": null (όλα σε πεζά). 
9. Κάντε κλικ στο κουμπί " **Ανάπτυξη** " στη γραμμή εργαλείων για να αναπτύξετε τη διαδικασία.  

### <a name="monitor-the-pipeline"></a>Παρακολούθηση της διοχέτευσης

6. Κάντε κλικ στο **X** για να κλείσετε το πρόγραμμα επεξεργασίας δεδομένων εργοστασίου λεπίδες και για να επιστρέψετε το blade εργοστασίου δεδομένων και κάντε κλικ στο **διάγραμμα**.

    ![πλακίδιο του διαγράμματος](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. Στην **Προβολή "Διάγραμμα"**, μπορείτε να δείτε μια επισκόπηση των αγωγών και των συνόλων δεδομένων που χρησιμοποιούνται σε αυτό το πρόγραμμα εκμάθησης. 

    ![πλακίδιο του διαγράμματος](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Στην προβολή διαγράμματος, κάντε διπλό κλικ το σύνολο δεδομένων **sprocsampleout**. Μπορείτε να δείτε τις φέτες σε κατάσταση είστε έτοιμοι. Πρέπει να υπάρχει πέντε φέτες, επειδή ένα κομμάτι παράγεται για κάθε ώρα μεταξύ της ώρας έναρξης και ώρα λήξης από την JSON.

    ![πλακίδιο του διαγράμματος](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Όταν ένα κομμάτι βρίσκεται σε κατάσταση **είστε έτοιμοι** , εκτελέστε μια * *επιλογή* από sampletable** ερωτήματος σε σχέση με τη βάση δεδομένων Azure SQL για να επαληθεύσετε ότι τα δεδομένα έχει εισαχθεί στο στον πίνακα από την αποθηκευμένη διαδικασία.

    ![Δεδομένα εξόδου](./media/data-factory-stored-proc-activity/output.png)

    Για λεπτομερείς πληροφορίες σχετικά με την παρακολούθηση εργοστασίου δεδομένων Azure αγωγούς, ανατρέξτε στο θέμα [Παρακολούθηση της διοχέτευσης](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] Σε αυτό το παράδειγμα, το SprocActivitySample έχει χωρίς εισόδων. Εάν θέλετε να ποδηλάτου αυτήν τη δραστηριότητα με μια δραστηριότητα πριν (δηλαδή, πριν από την επεξεργασία), το εξόδους της δραστηριότητας νέα μπορούν να χρησιμοποιηθούν ως εισροές σε αυτή τη δραστηριότητα. Σε αυτήν την περίπτωση, αυτή η δραστηριότητα δεν εκτελεί μέχρι να ολοκληρωθεί η νέα δραστηριότητα και τα εξόδους νέα δραστηριότητες είναι διαθέσιμες (στο έτοιμοι κατάστασης). Τα δεδομένα εισόδου δεν μπορούν να χρησιμοποιηθούν απευθείας ως παράμετρο, για να τη δραστηριότητα αποθηκευμένη διαδικασία

## <a name="json-format"></a>Μορφή JSON
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>Ιδιότητες JSON

Ιδιότητα | Περιγραφή | Απαιτείται
-------- | ----------- | --------
Όνομα | Όνομα της δραστηριότητας | Ναι
Περιγραφή | Κείμενο που περιγράφει τι χρησιμεύει η δραστηριότητα | Όχι
Τύπος | SqlServerStoredProcedure | Ναι
εισόδων | Προαιρετικό. Εάν καθορίσετε ένα σύνολο δεδομένων εισόδου, πρέπει να είναι διαθέσιμο (σε 'Έτοιμη' κατάσταση) για τη δραστηριότητα αποθηκευμένη διαδικασία για να εκτελέσετε. Το σύνολο δεδομένων εισαγωγής δεν μπορεί να καταναλωθεί της αποθηκευμένης διαδικασίας ως παράμετρο. Χρησιμοποιείται μόνο για να ελέγξετε την εξάρτηση πριν να ξεκινήσετε τη δραστηριότητα αποθηκευμένη διαδικασία. | Όχι
εξόδους | Πρέπει να καθορίσετε ένα σύνολο δεδομένων εξόδου για μια αποθηκευμένη διαδικασία δραστηριότητα. Σύνολο δεδομένων εξόδου Καθορίζει το **Χρονοδιάγραμμα** για τη δραστηριότητα αποθηκευμένη διαδικασία (ωριαία, εβδομαδιαία, μηνιαία, κ.λπ.). <br/><br/>Πρέπει να χρησιμοποιήσετε μια **συνδεδεμένων υπηρεσιών** που αναφέρεται σε μια βάση δεδομένων SQL Azure ή μια αποθήκη δεδομένων του SQL Azure ή μια βάση δεδομένων SQL Server που στην οποία θέλετε να εκτελέσετε την αποθηκευμένη διαδικασία του συνόλου δεδομένων εξόδου. <br/><br/>Το σύνολο δεδομένων εξόδου μπορεί να χρησιμοποιηθεί ως ένας τρόπος για τη μεταβίβαση το αποτέλεσμα της αποθηκευμένης διαδικασίας για μετέπειτα επεξεργασία από μια άλλη δραστηριότητα ([αλυσιδωτή δραστηριότητες](data-factory-scheduling-and-execution.md#chaining-activities)) στη διοχέτευση. Ωστόσο, εργοστασίου δεδομένων δεν αυτόματα Γράψτε το αποτέλεσμα μιας αποθηκευμένης διαδικασίας σε αυτό το dataset. Πρόκειται για την αποθηκευμένη διαδικασία που γράφει σε έναν πίνακα SQL που δείχνει το σύνολο δεδομένων εξόδου. <br/><br/>Σε ορισμένες περιπτώσεις, του συνόλου δεδομένων εξόδου μπορεί να είναι μια **εικονική σύνολο δεδομένων**, που χρησιμοποιείται μόνο για να καθορίσετε το χρονοδιάγραμμα για την εκτέλεση της δραστηριότητας αποθηκευμένη διαδικασία. | Ναι
storedProcedureName | Καθορίστε το όνομα της αποθηκευμένης διαδικασίας στη βάση δεδομένων Azure SQL ή αποθήκη δεδομένων SQL Azure που αντιπροσωπεύεται από την υπηρεσία συνδεδεμένων που χρησιμοποιεί τον πίνακα εξόδου. | Ναι
storedProcedureParameters | Καθορίστε τιμές για τις παραμέτρους αποθηκευμένη διαδικασία. Εάν χρειάζεστε για τη μεταβίβαση null για μια παράμετρο, χρησιμοποιήστε την εξής σύνταξη: "παράμετρος1": null (όλα πεζά). Δείτε το παρακάτω παράδειγμα, για να μάθετε σχετικά με τη χρήση αυτής της ιδιότητας.| Όχι

## <a name="passing-a-static-value"></a>Διέρχεται μια στατική τιμή 
Τώρα, ας δούμε προσθέτοντας μια άλλη στήλη 'Σενάριο' με το όνομα του πίνακα που περιέχει μια στατική τιμή που ονομάζεται 'Δείγμα εγγράφου'.

![Δείγμα δεδομένων 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Τώρα, περάσει την παράμετρο σενάριο και την τιμή από την αποθηκευμένη διαδικασία της δραστηριότητας. Στην ενότητα typeProperties στο προηγούμενο δείγμα μοιάζει με το παρακάτω τμήμα κώδικα:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }


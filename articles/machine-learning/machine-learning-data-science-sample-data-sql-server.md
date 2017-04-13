<properties 
    pageTitle="Δείγμα δεδομένων στον SQL Server σε Azure | Microsoft Azure" 
    description="Δείγμα δεδομένων στον SQL Server στο Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Δείγμα δεδομένων στον SQL Server στο Azure


Αυτό το έγγραφο εμφανίζει τον τρόπο δείγμα δεδομένων που είναι αποθηκευμένα στον SQL Server σε Azure χρησιμοποιώντας SQL ή της γλώσσας προγραμματισμού Python. Εμφανίζει επίσης τον τρόπο μετακίνησης δείγματος δεδομένων σε Azure μηχανικής εκμάθησης, αποθηκεύοντάς το σε ένα αρχείο, αποστολή του σε μια αντικειμένων blob του Azure και, στη συνέχεια, να το διαβάσετε σε Azure μηχανικής εκμάθησης Studio.

Η δειγματοληψία Python χρησιμοποιεί τη βιβλιοθήκη ODBC [pyodbc](https://code.google.com/p/pyodbc/) για να συνδεθείτε με τον SQL Server Azure και τη βιβλιοθήκη [Pandas](http://pandas.pydata.org/) για να κάνετε τη δειγματοληψία.

>[AZURE.NOTE] Το δείγμα κώδικα SQL σε αυτό το έγγραφο προϋποθέτει ότι τα δεδομένα είναι στο SQL Server στο Azure. Εάν δεν είναι, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων με τον SQL Server στο Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) για οδηγίες σχετικά με τον τρόπο για να μετακινήσετε τα δεδομένα σας με τον SQL Server Azure.

**Δείγμα γιατί τα δεδομένα σας;**
Εάν το σύνολο δεδομένων που σχεδιάζετε να αναλύσετε είναι μεγάλο, είναι συνήθως καλή ιδέα να τα δεδομένα για να μειώσετε το μέγεθος μικρότερο αλλά αντιπρόσωπος και πιο εύχρηστη δειγμάτων προς τα κάτω. Αυτό διευκολύνει την κατανόηση των δεδομένων, Εξερεύνηση και η δυνατότητα μηχανικής. Ο ρόλος του για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) είναι η ενεργοποίηση γρήγορα τη δημιουργία πρωτοτύπων του λειτουργίες επεξεργασίας δεδομένων και μηχανικής εκμάθησης μοντέλων.

Το **μενού** παρακάτω συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δείγμα δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Αυτή η εργασία δειγματοληψία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Χρήση SQL

Αυτή η ενότητα περιγράφει τις διάφορες μεθόδους χρήση SQL για την εκτέλεση απλό τυχαία δειγματοληψία σε σχέση με τα δεδομένα στη βάση δεδομένων. Επιλέξτε μια μέθοδο με βάση το μέγεθος των δεδομένων και τη διανομή του.

Τα παρακάτω δύο στοιχεία δείχνουν πώς μπορείτε να χρησιμοποιήσετε newid στον SQL Server για να εκτελέσετε τη δειγματοληψία. Η μέθοδος που επιλέγετε εξαρτάται από το τυχαίο πώς θέλετε το δείγμα να είναι (pk_id τον παρακάτω κώδικα δείγμα λαμβάνεται ένα πρωτεύον κλειδί αυτόματης δημιουργίας).

1. Λιγότερο αυστηρών τυχαίου δείγματος

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Δείγμα πιο τυχαία 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample να να χρησιμοποιηθεί για δειγματοληψία καθώς και παρουσιάζεται παρακάτω. Αυτό μπορεί να μια καλύτερη προσέγγιση, εάν το μέγεθος δεδομένων είναι μεγάλο (εάν υποθέσουμε ότι δεν είναι συσχετισμένης δεδομένων σε διαφορετικές σελίδες) και για το ερώτημα για να ολοκληρωθεί σε λογικό χρονικό διάστημα.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Μπορείτε να εξερευνήσετε και να δημιουργήσετε δυνατότητες από αυτά τα δεδομένα δείγματος, αποθηκεύοντάς το σε ένα νέο πίνακα


###<a name="sql-aml"></a>Σύνδεση με Azure μηχανικής εκμάθησης

Μπορείτε να χρησιμοποιήσετε απευθείας τα ερωτήματα δείγμα επάνω από το Azure μηχανικής εκμάθησης [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα για τα δεδομένα στη διάρκεια της λειτουργίας δειγμάτων προς τα κάτω και να την σε μια έρευνα Azure μηχανικής εκμάθησης. Ένα στιγμιότυπο οθόνης με χρήση της λειτουργικής μονάδας ανάγνωσης για να διαβάσετε τα δεδομένα του δείγματος είναι φαίνεται παρακάτω:
   
![πρόγραμμα ανάγνωσης sql][1]

##<a name="python"></a>Χρήση του Python γλώσσα προγραμματισμού 

Αυτή η ενότητα παρουσιάζει τη χρήση της [βιβλιοθήκης pyodbc](https://code.google.com/p/pyodbc/) για να δημιουργήσετε μια ODBC σύνδεση με μια βάση δεδομένων SQL server στο Python. Η συμβολοσειρά σύνδεσης βάσης δεδομένων είναι ως εξής: (αντικαταστήστε όνομα διακομιστή, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης με τη ρύθμιση παραμέτρων):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Στη βιβλιοθήκη [Pandas](http://pandas.pydata.org/) του Python παρέχει ένα εμπλουτισμένο σύνολο δομές δεδομένων και εργαλεία ανάλυσης δεδομένων για το χειρισμό δεδομένων για τον προγραμματισμό Python. Ο παρακάτω κώδικας διαβάζει ένα δείγμα 0,1% των δεδομένων από έναν πίνακα σε βάση δεδομένων Azure SQL στη Pandas δεδομένων:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Τώρα μπορείτε να εργαστείτε με τα δεδομένα του δείγματος στο πλαίσιο δεδομένα Pandas. 

###<a name="python-aml"></a>Σύνδεση με Azure μηχανικής εκμάθησης

Μπορείτε να χρησιμοποιήσετε το παρακάτω δείγμα κώδικα για να αποθηκεύσετε τα δεδομένα δειγματοληψία προς τα κάτω σε ένα αρχείο και στείλτε το σε μια αντικειμένων blob του Azure. Ανάγνωση των δεδομένων στο αντικείμενο blob μπορεί να είναι απευθείας σε μια έρευνα Azure μηχανικής εκμάθησης χρησιμοποιώντας την [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα. Τα βήματα είναι τα εξής: 

1. Γράψτε το πλαίσιο pandas δεδομένων σε ένα τοπικό αρχείο

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Αποστολή αρχείου τοπικό αντικειμένων blob του Azure

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Ανάγνωση δεδομένων από αντικειμένων blob του Azure χρησιμοποιώντας Azure μηχανικής εκμάθησης [Εισαγωγή δεδομένων] [ import-data] λειτουργική μονάδα, όπως φαίνεται στην την οθόνη Πάρτε παρακάτω:
 
![πρόγραμμα ανάγνωσης blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Η διαδικασία Science δεδομένων ομάδας στο παράδειγμα ενέργειας

Για ένα παράδειγμα αναλυτικές οδηγίες για να ολοκληρωμένες της διαδικασίας ομάδας δεδομένων Science μια χρησιμοποιώντας μια δημόσια συνόλου δεδομένων, ανατρέξτε στο θέμα [διαδικασία Science δεδομένων ομάδας σε δράση: χρήση διακομιστή SQL](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

<properties
    pageTitle="Δημιουργία δυνατοτήτων για τα δεδομένα στο SQL Server με χρήση SQL και Python | Microsoft Azure"
    description="Διαδικασία δεδομένων από το SQL Azure"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;fashah;garye" />


# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Δημιουργία δυνατοτήτων για τα δεδομένα στο SQL Server με χρήση SQL και Python


Αυτό το έγγραφο δείχνει πώς να δημιουργείτε δυνατότητες για δεδομένα που είναι αποθηκευμένα σε μια Εικονική SQL Server στο Azure που σας βοηθούν αλγόριθμους μάθετε πιο αποδοτικά από τα δεδομένα. Αυτό μπορεί να γίνει με τη χρήση SQL ή χρησιμοποιώντας μια γλώσσα προγραμματισμού όπως Python, οι οποίες εκδηλώνονται εδώ.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Το **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν τον τρόπο δημιουργίας δυνατοτήτων για τα δεδομένα σε διάφορα περιβάλλοντα. Αυτή η εργασία είναι ένα βήμα για τη [Διαδικασία Science δεδομένων ομάδας (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [AZURE.NOTE] Για ένα παράδειγμα πρακτική, μπορείτε να συμβουλευτείτε το [σύνολο δεδομένων ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) και να αναφέρονται σε το IPNB με τίτλο [wrangling νέα ΥΌΡΚΗ δεδομένων με χρήση του σημειωματαρίου IPython και SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) για μια Γνωρίστε για ολοκληρωμένες.


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε:

* Δημιουργία λογαριασμού Azure χώρου αποθήκευσης. Εάν χρειάζεστε οδηγίες, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού αποθήκευσης Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Αποθηκεύονται τα δεδομένα σας στον SQL Server. Εάν δεν έχετε, ανατρέξτε στο θέμα [Μετακίνηση δεδομένων σε μια βάση δεδομένων SQL Azure για το Azure μηχανικής εκμάθησης](machine-learning-data-science-move-sql-azure.md) για οδηγίες σχετικά με τον τρόπο για να μετακινήσετε τα δεδομένα εκεί.


## <a name="sql-featuregen"></a>Δυνατότητα δημιουργίας με SQL

Σε αυτήν την ενότητα, θα σας περιγράφουν τρόπους δημιουργίας δυνατότητες με χρήση SQL:  

1. [Μέτρηση με βάση τη δυνατότητα δημιουργίας](#sql-countfeature)
2. [Binning δυνατότητα γενιάς](#sql-binningfeature)
3. [Σάρωση τις δυνατότητες από μία στήλη](#sql-featurerollout)


> [AZURE.NOTE] Αφού δημιουργήσετε πρόσθετες δυνατότητες, να προσθέσετε ως στήλες στον υπάρχοντα πίνακα ή να δημιουργήσετε έναν νέο πίνακα με τις πρόσθετες δυνατότητες και το πρωτεύον κλειδί, που μπορούν να συνδεθούν με τον αρχικό πίνακα.

### <a name="sql-countfeature"></a>Μέτρηση με βάση τη δυνατότητα δημιουργίας

Αυτό το έγγραφο παρουσιάζει δύο τρόπους δημιουργίας πλήθος δυνατοτήτων. Η πρώτη μέθοδος χρησιμοποιεί άθροιση υπό όρους και τη δεύτερη μέθοδο χρησιμοποιεί τον όρο 'where'. Αυτά, στη συνέχεια, να είναι συνδεδεμένος με τον αρχικό πίνακα (χρησιμοποιώντας στήλες πρωτεύοντος κλειδιού) για να έχετε πλήθος δυνατοτήτων μαζί με τα αρχικά δεδομένα.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Binning δυνατότητα γενιάς

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας binned δυνατότητες με binning (5 θέσεις αποθήκευσης με χρήση) μια αριθμητική στήλη η οποία μπορεί να χρησιμοποιηθεί ως δυνατότητα αντί για αυτό:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Σάρωση τις δυνατότητες από μία στήλη

Σε αυτήν την ενότητα, θα σας δείχνουν τον τρόπο υπολογισμού ανάληψης μόνο μία στήλη σε έναν πίνακα για να δημιουργήσετε πρόσθετες δυνατότητες. Το παράδειγμα προϋποθέτει ότι υπάρχει μια στήλη γεωγραφικό πλάτος ή το μήκος του πίνακα από το οποίο προσπαθείτε να δημιουργήσετε δυνατότητες.

Ακολουθεί μια σύντομη εισαγωγή σε δεδομένα τοποθεσίας γεωγραφικό πλάτος/μήκος (πόρων από stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Αυτό είναι χρήσιμο να κατανοήσετε πριν από την featurizing πεδίο θέση:

- Το σύμβολο σας ενημερώνει μας εάν Ζητούμε north ή Νότος, Ανατολή ή Δυτική της υδρογείου.
- Μια μη μηδενική εκατοντάδες ψηφίο μας ενημερώνει χρησιμοποιούμε τη γεωγραφικού, δεν γεωγραφικό πλάτος!
- Τα δεκάδες ψηφίο παρέχει μια θέση σε περίπου 1.000 χιλιόμετρα. Σας δίνει μας χρήσιμες πληροφορίες σχετικά με το τι Ήπειρο ή Ωκεανού είμαστε σε.
- Το ψηφίο μονάδες (ένα δεκαδικό βαθμός) παρέχει μια θέση προς τα επάνω στο 111 χιλιόμετρα (60 ναυτικό μίλι, περίπου 69 μίλια). Αυτό μπορεί να πείτε μας περίπου τι μεγάλη μέλος ή χώρα Ζητούμε στο.
- Στο πρώτο δεκαδικό αξίζει έως 11.1 χιλιόμετρα: Αυτό μπορεί να διακρίνετε τη θέση του ένα μεγάλο πόλη από γειτονικών μεγάλο πόλης.
- Το δεύτερο δεκαδικό ψηφίο αξίζει έως 1.1 χιλιόμετρα: Αυτό μπορεί να διαχωρίσετε μία χωριό από το επόμενο.
- Το τρίτο δεκαδικό ψηφίο αξίζει έως 110 m: μπορέσει να προσδιορίσει μεγάλο γεωργικών πεδίου ή θεσμικές της.
- Η τέταρτη δεκαδικό ψηφίο αξίζει έως 11 m: μπορέσει να προσδιορίσει ένα κιβώτιο άποψη. Είναι συγκρίσιμη με εκείνη την ακρίβεια τυπικές μιας μη διορθωμένη μονάδας GPS χωρίς παρεμβολές.
- Το πέμπτο δεκαδικό ψηφίο είναι αξίζει έως 1.1 m: διακρίνετε δέντρα μεταξύ τους. Ακρίβεια σε αυτό το επίπεδο με εμπορικές μονάδες GPS μπορείτε να επιτύχετε μόνο με διαφορά διόρθωση.
- Το έκτο δεκαδικό ψηφίο αξίζει έως 0.11 m: μπορείτε να το χρησιμοποιήσετε για τη διάταξη δομές με λεπτομέρειες για τη σχεδίαση τοπίων, δημιουργία δρόμους. Θα πρέπει να υπερβαίνει είναι αρκετά καλή για την παρακολούθηση των κινήσεων του παγετώνες και ποταμούς. Αυτό μπορεί να είναι δυνατό λαμβάνοντας painstaking μέτρα με GPS, όπως differentially διορθωμένο GPS.

Οι πληροφορίες τοποθεσίας μπορεί να μπορεί να featurized ως εξής, διαχωρισμό εκτός περιοχής, θέση και οι πληροφορίες πόλης. Σημειώστε ότι μία φορά επίσης να καλέσετε ένα τελικό σημείο ΥΠΌΛΟΙΠΑ όπως API χάρτες Bing είναι διαθέσιμο στη `https://msdn.microsoft.com/library/ff701710.aspx` για να λάβετε τις πληροφορίες περιοχής/περιφέρεια.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Οι παραπάνω δυνατότητες θέση με βάση μπορεί να χρησιμοποιηθεί περαιτέρω για τη δημιουργία πλήθους πρόσθετες δυνατότητες, όπως περιγράφεται παραπάνω.


> [AZURE.TIP] Μπορείτε να εισαγάγετε μέσω προγραμματισμού των εγγραφών χρησιμοποιώντας τη γλώσσα της επιλογής. Ίσως χρειαστεί να εισαγάγετε τα δεδομένα σε τμήματα για τη βελτίωση της αποδοτικότητας εγγραφής [ανατρέξτε στο θέμα το παράδειγμα του τρόπου να το κάνετε αυτό με χρήση pyodbc εδώ](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
Μια άλλη επιλογή είναι να εισαγάγετε δεδομένα στη βάση δεδομένων με χρήση [βοηθητικού προγράμματος BCP](https://msdn.microsoft.com/library/ms162802.aspx)

### <a name="sql-aml"></a>Σύνδεση με Azure μηχανικής εκμάθησης

Η δυνατότητα που δημιουργήθηκε πρόσφατα μπορεί να προστεθεί ως στήλη σε έναν υπάρχοντα πίνακα ή να είναι αποθηκευμένα σε έναν νέο πίνακα και να συνδεθεί με τον αρχικό πίνακα για μηχανικής εκμάθησης. Δυνατότητες μπορούν να που δημιουργούνται ή να έχετε πρόσβαση σε αυτές εάν ήδη δημιουργήσει, με χρήση της λειτουργικής μονάδας [Εισαγωγή δεδομένων](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) σε Azure ML, όπως φαίνεται παρακάτω:

![οι αναγνώστες azureml](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Χρησιμοποιείτε μια γλώσσα προγραμματισμού όπως Python

Χρήση Python για τη δημιουργία δυνατότητες όταν τα δεδομένα βρίσκονται σε SQL Server είναι παρόμοια με την επεξεργασία δεδομένων σε αντικειμένων blob του Azure χρησιμοποιώντας Python που τεκμηριώνονται σε [διαδικασία Azure Blob δεδομένων σε που δεδομένων science περιβάλλον](machine-learning-data-science-process-data-blob.md). Τα δεδομένα πρέπει να φορτωθεί από τη βάση δεδομένων σε ένα πλαίσιο pandas δεδομένων και, στη συνέχεια, να δυνατή η περαιτέρω επεξεργασία. Έχουμε τεκμηριώσει της διαδικασίας τη σύνδεση με τη βάση δεδομένων και τη φόρτωση των δεδομένων μέσα στο πλαίσιο δεδομένα σε αυτήν την ενότητα.

Την εξής μορφή της συμβολοσειράς σύνδεσης μπορεί να χρησιμοποιηθεί για να συνδεθείτε με μια βάση δεδομένων SQL Server από Python χρησιμοποιώντας pyodbc (όνομα_διακομιστή αντικατάσταση, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης με τις συγκεκριμένες τιμές):

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Στη [βιβλιοθήκη Pandas](http://pandas.pydata.org/) του Python παρέχει ένα εμπλουτισμένο σύνολο δομές δεδομένων και εργαλεία ανάλυσης δεδομένων για το χειρισμό δεδομένων για τον προγραμματισμό Python. Ο παρακάτω κώδικας διαβάζει τα αποτελέσματα που επιστρέφονται από μια βάση δεδομένων SQL Server σε ένα πλαίσιο δεδομένων Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Τώρα μπορείτε να εργαστείτε με το πλαίσιο δεδομένων Pandas όπως καλύπτεται στα θέματα [Δημιουργία δυνατότητες για δεδομένα χώρο αποθήκευσης αντικειμένων blob του Azure χρησιμοποιώντας Panda](machine-learning-data-science-create-features-blob.md).

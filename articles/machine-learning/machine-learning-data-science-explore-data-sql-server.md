<properties 
    pageTitle="Εξερεύνηση δεδομένων στο SQL Server εικονική μηχανή στο Azure | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να εξερευνήσετε τα δεδομένα που είναι αποθηκευμένα σε μια Εικονική SQL Server στο Azure." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Εξερεύνηση δεδομένων στο SQL Server εικονική μηχανή στο Azure


Αυτό το έγγραφο περιγράφει πώς μπορείτε να εξερευνήσετε τα δεδομένα που είναι αποθηκευμένα σε μια Εικονική SQL Server στο Azure. Αυτό μπορεί να γίνει wrangling δεδομένων με χρήση SQL ή χρησιμοποιώντας μια γλώσσα προγραμματισμού όπως Python.

Το παρακάτω **μενού** συνδέσεις σε θέματα τα οποία περιγράφουν πώς μπορείτε να χρησιμοποιήσετε εργαλεία για την Εξερεύνηση των δεδομένων από διάφορα περιβάλλοντα χώρο αποθήκευσης. Αυτή η εργασία είναι ένα βήμα στη διαδικασία ανάλυσης Cortana (ΚΑΠΈΛΟ).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Τα παραδείγματα προτάσεων SQL σε αυτό το έγγραφο λαμβάνεται ως δεδομένο ότι είναι δεδομένων στον SQL Server. Εάν δεν είναι, ανατρέξτε στο χάρτη cloud δεδομένων science διαδικασία για να μάθετε πώς μπορείτε να μεταφέρετε τα δεδομένα σας με τον SQL Server.



## <a name="sql-dataexploration"></a>Εξερεύνηση δεδομένων SQL με δέσμες ενεργειών SQL

Εδώ θα βρείτε μερικά δείγματα SQL δεσμών ενεργειών που μπορούν να χρησιμοποιηθούν για να εξερευνήσετε χώροι αποθήκευσης δεδομένων στον SQL Server.

1. Καταμέτρηση των παρατηρήσεων ανά ημέρα

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Λάβετε τα επίπεδα σε μια στήλη κατηγοριών

    `select  distinct <column_name> from <databasename>`

3. Βρείτε τον αριθμό των επιπέδων σε συνδυασμό των δύο στήλες κατηγοριών 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Λάβετε την κατανομή για αριθμητικές στήλες

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Για ένα παράδειγμα πρακτική, μπορείτε να χρησιμοποιήσετε το [σύνολο δεδομένων ταξί νέα ΥΌΡΚΗ](http://www.andresmh.com/nyctaxitrips/) και να αναφέρονται σε το IPNB με τίτλο [wrangling νέα ΥΌΡΚΗ δεδομένων με χρήση του σημειωματαρίου IPython και SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) για μια Γνωρίστε για ολοκληρωμένες.

##<a name="python"></a>Εξερεύνηση δεδομένων SQL με Python

Χρήση Python για Εξερεύνηση δεδομένων και δημιουργία δυνατότητες όταν τα δεδομένα βρίσκονται στον SQL Server είναι παρόμοια με την επεξεργασία δεδομένων σε αντικειμένων blob του Azure χρησιμοποιώντας Python, όπως περιγράφεται στις [αντικειμένων Blob του Azure διαδικασία δεδομένων στο περιβάλλον σας science δεδομένων](machine-learning-data-science-process-data-blob.md). Τα δεδομένα πρέπει να φορτωθεί από τη βάση δεδομένων σε μια pandas DataFrame και, στη συνέχεια, να δυνατή η περαιτέρω επεξεργασία. Έχουμε τεκμηριώσει της διαδικασίας τη σύνδεση με τη βάση δεδομένων και φόρτωση των δεδομένων σε το DataFrame σε αυτήν την ενότητα.

Την εξής μορφή της συμβολοσειράς σύνδεσης μπορεί να χρησιμοποιηθεί για να συνδεθείτε με μια βάση δεδομένων SQL Server από Python χρησιμοποιώντας pyodbc (αντικατάσταση όνομα διακομιστή, όνομα βάσης δεδομένων, όνομα χρήστη και τον κωδικό πρόσβασης με τις συγκεκριμένες τιμές):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Στη [βιβλιοθήκη Pandas](http://pandas.pydata.org/) του Python παρέχει ένα εμπλουτισμένο σύνολο δομές δεδομένων και εργαλεία ανάλυσης δεδομένων για το χειρισμό δεδομένων για τον προγραμματισμό Python. Ο ακόλουθος κώδικας διαβάζει τα αποτελέσματα που επιστρέφονται από μια βάση δεδομένων SQL Server σε ένα πλαίσιο δεδομένων Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Τώρα μπορείτε να εργαστείτε με το DataFrame Pandas όπως καλύπτεται στο θέμα [διαδικασία Azure Blob δεδομένων στο περιβάλλον σας science δεδομένων](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Διαδικασία ανάλυσης Cortana στο παράδειγμα ενέργειας

Για παράδειγμα αναλυτικές οδηγίες για να ολοκληρωμένες τη διαδικασία ανάλυσης Cortana χρησιμοποιώντας μια δημόσια συνόλου δεδομένων, ανατρέξτε στο θέμα [της διαδικασίας Science ομάδας δεδομένων σε δράση: χρήση του SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 

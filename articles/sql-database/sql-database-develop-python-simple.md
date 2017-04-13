<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL με χρήση Python | Microsoft Azure"
    description="Παρουσιάζει ένα δείγμα κώδικα Python μπορείτε να χρησιμοποιήσετε για να συνδεθείτε με βάση δεδομένων SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-python"></a>Σύνδεση με βάση δεδομένων SQL με χρήση Python


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Αυτό το θέμα δείχνει πώς μπορείτε να συνδεθείτε και υποβολή ερωτήματος σε μια βάση δεδομένων SQL Azure με χρήση Python. Μπορείτε να εκτελέσετε αυτό το δείγμα από το Windows, Ubuntu Linux ή Mac πλατφόρμες.


## <a name="step-1-create-a-sql-database"></a>Βήμα 1: Δημιουργήστε μια βάση δεδομένων SQL

Δείτε τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md) για να μάθετε πώς μπορείτε να δημιουργήσετε ένα δείγμα βάσης δεδομένων.  Είναι σημαντικό να ακολουθήσετε του οδηγού για τη δημιουργία ενός **προτύπου βάσης δεδομένων AdventureWorks**. Τα δείγματα που φαίνεται παρακάτω λειτουργούν μόνο με το **σχήμα AdventureWorks**. Αφού δημιουργήσετε την πραγματοποίηση βάσης δεδομένων ότι ενεργοποιείτε την πρόσβαση στη διεύθυνση IP, δίνοντάς τους κανόνες τείχους προστασίας, όπως περιγράφεται στο τη [σελίδα γρήγορων αποτελεσμάτων](sql-database-get-started.md)

## <a name="step-2-configure-development-environment"></a>Βήμα 2: Ρύθμιση παραμέτρων περιβάλλον ανάπτυξης

### <a name="mac-os"></a>**Mac OS**   
### <a name="install-the-required-modules"></a>Εγκαταστήστε τις απαιτούμενες λειτουργικές μονάδες
Ανοίξτε το terminal και εγκαταστήστε

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    brew install FreeTDS
    sudo -H pip install pymssql=2.1.1

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**

Ανοίξτε το terminal και μεταβείτε σε έναν κατάλογο όπου σκοπεύετε να δημιουργήσετε το python δέσμη ενεργειών. Εισαγάγετε τις παρακάτω εντολές για την εγκατάσταση **FreeTDS** και **pymssql**. pymssql χρησιμοποιεί FreeTDS για να συνδεθείτε με βάσεις δεδομένων SQL.

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    sudo apt-get --assume-yes install python-dev python-pip
    sudo pip install pymssql=2.1.1
    
### <a name="windows"></a>**Windows**

Εγκαταστήστε pymssql από [**εδώ**](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pymssql). 

Βεβαιωθείτε ότι επιλέγετε το σωστό whl αρχείο. Για παράδειγμα: Επιλέξτε εάν χρησιμοποιείτε Python 2.7 σε υπολογιστή 64 bit: pymssql‑2.1.1‑cp27‑none‑win_amd64.whl. Αφού κάνετε λήψη του αρχείου .whl τοποθετήστε το στο φάκελο C:/Python27.

Τώρα μπορείτε να εγκαταστήσετε το πρόγραμμα οδήγησης pymssql χρησιμοποιώντας pip από γραμμή εντολών. CD στο C:/Python27 και εκτελέστε τα εξής
    
    pip install pymssql‑2.1.1‑cp27‑none‑win_amd64.whl

Οδηγίες για να ενεργοποιήσετε το pip χρήση μπορείτε να βρείτε [εδώ](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="step-3-run-sample-code"></a>Βήμα 3: Εκτέλεση δείγμα κώδικα

Δημιουργήστε ένα αρχείο που ονομάζεται **sql_sample.py** και επικολλήστε τον ακόλουθο κώδικα μέσα σε αυτό. Μπορείτε να το εκτελέσετε από τη γραμμή εντολών χρησιμοποιώντας:
    
    python sql_sample.py

### <a name="connect-to-your-sql-database"></a>Σύνδεση με τη βάση δεδομένων SQL

Η συνάρτηση [pymssql.connect](http://pymssql.org/en/latest/ref/pymssql.html) χρησιμοποιείται για τη σύνδεση με βάση δεδομένων SQL.

    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')


### <a name="execute-an-sql-select-statement"></a>Εκτελεί μια πρόταση SQL SELECT

Η συνάρτηση [cursor.execute](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.execute) μπορεί να χρησιμοποιηθεί για να ανακτήσετε ένα σύνολο αποτελεσμάτων από ένα ερώτημα σε μια βάση δεδομένων SQL. Αυτή η συνάρτηση αποδέχεται ουσιαστικά οποιοδήποτε ερώτημα και επιστρέφει ένα σύνολο αποτελεσμάτων που μπορεί να είναι iterated μέσω με τη χρήση του [cursor.fetchone()](http://pymssql.org/en/latest/ref/pymssql.html#pymssql.Cursor.fetchone).


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute('SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;')
    row = cursor.fetchone()
    while row:
        print str(row[0]) + " " + str(row[1]) + " " + str(row[2])   
        row = cursor.fetchone()


### <a name="insert-a-row-pass-parameters-and-retrieve-the-generated-primary-key"></a>Εισαγάγετε μια γραμμή, μεταβιβάζουν τις παραμέτρους και ανάκτησης του πρωτεύοντος κλειδιού που δημιουργήθηκε

Σε βάση δεδομένων SQL την ιδιότητα [ΤΑΥΤΌΤΗΤΑΣ](https://msdn.microsoft.com/library/ms186775.aspx) και το αντικείμενο [ΑΚΟΛΟΥΘΊΑΣ](https://msdn.microsoft.com/library/ff878058.aspx) μπορεί να χρησιμοποιηθεί για την αυτόματη δημιουργία τιμών [πρωτεύοντος κλειδιού](https://msdn.microsoft.com/library/ms179610.aspx) . 


    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express', 'SQLEXPRESS', 0, 0, CURRENT_TIMESTAMP)")
    row = cursor.fetchone()
    while row:
        print "Inserted Product ID : " +str(row[0])
        row = cursor.fetchone()


### <a name="transactions"></a>Συναλλαγές


Αυτό το παράδειγμα κώδικα δείχνει τη χρήση των συναλλαγών στις οποίες μπορείτε:

* Έναρξη μιας συναλλαγής
* Εισαγάγετε μια γραμμή δεδομένων
* Επαναφορά συναλλαγής σας για να αναιρέσετε την εισαγωγή 

Επικολλήστε τον ακόλουθο κώδικα μέσα σε sql_sample.py.
    
    import pymssql
    conn = pymssql.connect(server='yourserver.database.windows.net', user='yourusername@yourserver', password='yourpassword', database='AdventureWorks')
    cursor = conn.cursor()
    cursor.execute("BEGIN TRANSACTION")
    cursor.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, CURRENT_TIMESTAMP)")
    cnxn.rollback()

## <a name="next-steps"></a>Επόμενα βήματα

* Δείτε την [Επισκόπηση ανάπτυξης βάσης δεδομένων SQL](sql-database-develop-overview.md)
* Περισσότερες πληροφορίες σχετικά με το [Πρόγραμμα οδήγησης Python της Microsoft για τον SQL Server](https://msdn.microsoft.com/library/mt652092.aspx)
* Επισκεφθείτε το [Κέντρο για προγραμματιστές Python](/develop/python/).

## <a name="additional-resources"></a>Πρόσθετοι πόροι 

* [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Εξερεύνηση όλες τις [δυνατότητες της βάσης δεδομένων SQL](https://azure.microsoft.com/services/sql-database/)

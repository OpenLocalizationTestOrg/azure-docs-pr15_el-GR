<properties
    pageTitle="Σύνδεση με βάση δεδομένων SQL με ένα ερώτημα C# | Microsoft Azure"
    description="Εγγραφή ενός προγράμματος στο C# για να υποβάλετε ερώτημα και σύνδεση με βάση δεδομένων SQL. Πληροφορίες σχετικά με τις διευθύνσεις IP, συμβολοσειρές σύνδεσης, ασφαλούς σύνδεσης και δωρεάν Visual Studio."
    services="sql-database"
    keywords="c# ερώτημα βάσης δεδομένων, c# ερώτημα, σύνδεση με βάση δεδομένων, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Σύνδεση με μια βάση δεδομένων SQL με το Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Μάθετε πώς μπορείτε να συνδεθείτε με μια βάση δεδομένων Azure SQL στο Visual Studio. 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία


Για να συνδεθείτε με μια βάση δεδομένων SQL χρήση του Visual Studio, χρειάζεστε τα εξής: 


- Μια βάση δεδομένων SQL για σύνδεση σε. Σε αυτό το άρθρο χρησιμοποιεί το δείγμα βάσης δεδομένων **AdventureWorks** . Για να λάβετε το δείγμα βάσης δεδομένων AdventureWorks, ανατρέξτε στο θέμα [Δημιουργία επίδειξης βάσης δεδομένων της](sql-database-get-started.md).


- Visual Studio ενημέρωση 2013 4 (ή νεότερη έκδοση). Η Microsoft παρέχει τώρα Κοινότητα του Visual Studio για *δωρεάν*.
 - [Κάντε λήψη του Visual Studio Κοινότητα](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Περισσότερες επιλογές για δωρεάν Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Ανοίξτε το Visual Studio από την πύλη του Azure


1. Συνδεθείτε [πύλη του Azure](https://portal.azure.com/).

2. Κάντε κλικ στην επιλογή **Περισσότερες υπηρεσίες** > **βάσεις δεδομένων SQL**
3. Ανοίξτε το blade βάσης δεδομένων **AdventureWorks** τον εντοπισμό και κάνοντας κλικ στη βάση δεδομένων *AdventureWorks* .

6. Κάντε κλικ στο κουμπί **Εργαλεία** στο επάνω μέρος του blade βάσης δεδομένων:

    ![Δημιουργία ερωτήματος. Σύνδεση με βάση δεδομένων SQL server: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Κάντε κλικ στην επιλογή **Άνοιγμα στο Visual Studio** (εάν χρειάζεται Visual Studio, κάντε κλικ στη σύνδεση λήψης):

    ![Δημιουργία ερωτήματος. Σύνδεση με βάση δεδομένων SQL server: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio ανοίγει με το παράθυρο **σύνδεση με το διακομιστή** ήδη ορίσει για να συνδεθείτε με το διακομιστή και τη βάση δεδομένων που επιλέξατε στην πύλη.  (Κάντε κλικ στο κουμπί **Επιλογές** για να επιβεβαιώσετε ότι η σύνδεση έχει ρυθμιστεί στη σωστή βάση δεδομένων.) Πληκτρολογήστε τον κωδικό πρόσβασης διαχειριστή του διακομιστή και κάντε κλικ στην επιλογή **σύνδεση**.


    ![Δημιουργία ερωτήματος. Σύνδεση με βάση δεδομένων SQL server: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Εάν δεν έχετε ένα σύνολο κανόνων τείχους προστασίας για χρήση με τη διεύθυνση IP του υπολογιστή σας, μπορείτε να λάβετε ένα εδώ μήνυμα *δεν μπορεί να συνδεθεί* . Για να δημιουργήσετε έναν κανόνα τείχους προστασίας, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ενός κανόνα τείχους προστασίας του επιπέδου διακομιστή βάσης δεδομένων SQL Azure](sql-database-configure-firewall-settings.md).


9. Μετά τη σύνδεση με επιτυχία, ανοίγει το παράθυρο **Εξερεύνηση αντικειμένου SQL Server** με μια σύνδεση με τη βάση δεδομένων.

    ![Δημιουργία ερωτήματος. Σύνδεση με βάση δεδομένων SQL server: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Εκτέλεση ενός ερωτήματος δείγματος

Τώρα που θα σας είστε συνδεδεμένοι στη βάση δεδομένων, ακολουθήστε τα παρακάτω βήματα δείχνουν τον τρόπο για την εκτέλεση ενός απλού ερωτήματος:

2. Κάντε δεξί κλικ στη βάση δεδομένων και, στη συνέχεια, επιλέξτε **Νέο ερώτημα**.

    ![Δημιουργία ερωτήματος. Σύνδεση με βάση δεδομένων SQL server: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Στο παράθυρο "ερώτημα", αντιγράψτε και επικολλήστε τον ακόλουθο κώδικα.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Κάντε κλικ στο κουμπί **Εκτέλεση** για να εκτελέσετε το ερώτημα:

    ![Επιτυχία. Σύνδεση με βάση δεδομένων SQL server: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Επόμενα βήματα

- Άνοιγμα βάσεων δεδομένων SQL στο Visual Studio χρησιμοποιεί SQL Server Data Tools. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Για να συνδεθείτε με μια βάση δεδομένων SQL χρησιμοποιώντας κωδικό, ανατρέξτε στο θέμα [σύνδεση με βάση δεδομένων SQL με χρήση του .NET (C#)](sql-database-develop-dotnet-simple.md).




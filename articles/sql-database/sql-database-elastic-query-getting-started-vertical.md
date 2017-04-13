<properties
    pageTitle="Γρήγορα αποτελέσματα με το σταυρό-ερωτήματα βάσης δεδομένων (κατακόρυφο διαμερισμάτων) | Microsoft Azure"   
    description="Πώς να χρησιμοποιείτε το ερώτημα ελαστικότητας βάσης δεδομένων με κατακόρυφη διαμερίσματα βάσεις δεδομένων"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Γρήγορα αποτελέσματα με το σταυρό-ερωτήματα βάσης δεδομένων (κατακόρυφο διαμερισμάτων) (έκδοση preview)

Ερώτημα ελαστικότητας βάσης δεδομένων (έκδοση preview) για βάση δεδομένων SQL Azure σας επιτρέπει να εκτελέσετε ερωτήματα T SQL που εκτείνονται σε πολλές βάσεις δεδομένων χρησιμοποιώντας ένα σημείο σύνδεσης μόνο. Αυτό το θέμα ισχύει για [κατακόρυφα διαμερίσματα βάσεις δεδομένων](sql-database-elastic-query-vertical-partitioning.md).  

Όταν ολοκληρωθεί, θα: Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους και να χρησιμοποιήσετε μια βάση δεδομένων SQL Azure για να εκτελέσετε ερωτήματα που εκτείνονται σε πολλές σχετικές βάσεις δεδομένων. 

Για περισσότερες πληροφορίες σχετικά με τη δυνατότητα ερωτήματος ελαστικότητας βάσης δεδομένων, ανατρέξτε στο θέμα [Επισκόπηση ερωτήματος βάσης δεδομένων SQL Azure ελαστικότητας βάσης δεδομένων](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Δημιουργήστε τα δείγματα βάσεων δεδομένων

Για να ξεκινήσετε με, πρέπει να δημιουργήσετε δύο βάσεις δεδομένων, **πελάτες** και **παραγγελίες**, είτε στους διακομιστές λογική ίδιες ή σε διαφορετικές.   

Εκτελέστε τα ακόλουθα ερωτήματα στη βάση δεδομένων **παραγγελίες** για να δημιουργήσετε τον πίνακα **OrderInformation** και να εισαγάγετε το δείγμα δεδομένων. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Τώρα, η εκτέλεση παρακάτω ερώτημα στη βάση δεδομένων **πελατών** για να δημιουργήσετε τον πίνακα **CustomerInformation** και να εισαγάγετε το δείγμα δεδομένων. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Δημιουργία αντικειμένων βάσης δεδομένων
### <a name="database-scoped-master-key-and-credentials"></a>Βάση δεδομένων εύρος πρωτεύον κλειδί και τα διαπιστευτήρια

1. Ανοίξτε το SQL Server Management Studio ή Εργαλεία δεδομένων του SQL Server στο Visual Studio.
2. Συνδεθείτε στη βάση δεδομένων παραγγελίες και εκτελεί τις παρακάτω εντολές T-SQL:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    Το "όνομα χρήστη" και "κωδικός πρόσβασης" πρέπει να είναι το όνομα χρήστη και κωδικό πρόσβασης που χρησιμοποιείται για να συνδεθείτε στη βάση δεδομένων πελατών.
    Έλεγχος ταυτότητας με χρήση Azure Active Directory με ελαστικά ερωτήματα δεν υποστηρίζεται αυτήν τη στιγμή.

### <a name="external-data-sources"></a>Εξωτερικές προελεύσεις δεδομένων
Για να δημιουργήσετε μια εξωτερική προέλευση δεδομένων, εκτελέστε την ακόλουθη εντολή στη βάση δεδομένων παραγγελίες: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Εξωτερικοί πίνακες
Δημιουργήστε έναν εξωτερικό πίνακα από τη βάση δεδομένων παραγγελιών, που ταιριάζει με τον ορισμό του πίνακα CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Εκτέλεση ενός ερωτήματος δείγμα ελαστικότητας βάσης δεδομένων T SQL

Αφού έχετε ορίσει εξωτερικής προέλευσης δεδομένων και εξωτερικών τους πίνακές σας μπορείτε τώρα να χρησιμοποιήσετε T-SQL ερώτημα εξωτερικών τους πίνακές σας. Εκτέλεση αυτού του ερωτήματος από τη βάση δεδομένων παραγγελίες: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Κόστος

Προς το παρόν, η δυνατότητα ερωτήματος ελαστικότητας βάσης δεδομένων περιλαμβάνεται σε το κόστος της βάσης δεδομένων SQL Azure.  

Για πληροφορίες για τις τιμές ανατρέξτε [Τιμολόγησης βάσης δεδομένων SQL](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->

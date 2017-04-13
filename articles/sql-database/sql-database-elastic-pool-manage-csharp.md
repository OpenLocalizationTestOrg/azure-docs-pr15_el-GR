<properties
    pageTitle="Παρακολούθηση και διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το C# | Microsoft Azure"
    description="Χρήση C# τεχνικές ανάπτυξης βάσης δεδομένων για τη διαχείριση ενός χώρου συγκέντρωσης βάση δεδομένων SQL Azure ελαστικότητας βάσης δεδομένων."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Παρακολούθηση και διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το C & #x 23. 

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Μάθετε πώς μπορείτε να διαχειριστείτε ένα [χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md) με C & #x 23. 

>[AZURE.NOTE] Πολλές νέες δυνατότητες της βάσης δεδομένων SQL υποστηρίζονται μόνο όταν χρησιμοποιείτε το [μοντέλο ανάπτυξης διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md), έτσι θα πρέπει να χρησιμοποιείτε πάντα τις πιο πρόσφατες **βιβλιοθήκης διαχείρισης βάσης δεδομένων SQL Azure για το .NET ([έγγραφα](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet πακέτου](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Την παλαιότερη [βιβλιοθήκες βασίζεται σε μοντέλο κλασική ανάπτυξης](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) υποστηρίζονται για συμβατότητα με προηγούμενες εκδόσεις μόνο, επομένως συνιστάται να χρησιμοποιείτε τις νεότερες βιβλιοθήκες από διαχειριστή πόρων με βάση.

Για να ολοκληρώσετε τα βήματα σε αυτό το άρθρο, χρειάζεστε τα ακόλουθα στοιχεία:

- Ένα σύνολο ελαστικά (το χώρο συγκέντρωσης που θέλετε να διαχειριστείτε). Για να δημιουργήσετε ένα χώρο συγκέντρωσης, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Για μια δωρεάν αντίγραφο του Visual Studio, δείτε τη σελίδα [Λήψεων του Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Μετακίνηση μιας βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας

Μπορείτε να μετακινήσετε μια μεμονωμένη βάση δεδομένων ή σμίκρυνση ενός χώρου συγκέντρωσης.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Λίστα βάσεις δεδομένων σε ένα σύνολο ελαστικότητας

Για να ανακτήσετε όλες τις βάσεις δεδομένων σε ένα χώρο συγκέντρωσης, καλέστε τη μέθοδο [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) .

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Αλλαγή των ρυθμίσεων απόδοσης του χώρου συγκέντρωσης

Ανακτήστε υπάρχουσες τις ιδιότητες του χώρου συγκέντρωσης. Τροποποιήστε τις τιμές και η εφαρμογή της μεθόδου CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Λανθάνων χρόνος λειτουργιών ελαστικότητας χώρου συγκέντρωσης

- Αλλάζοντας το ελάχιστο eDTUs ανά βάση δεδομένων ή max eDTUs ανά βάση δεδομένων συνήθως ολοκληρώνει μετά από πέντε λεπτά ή λιγότερο.
- Χρόνος για να αλλάξετε το μέγεθος του χώρου συγκέντρωσης (eDTUs) εξαρτάται από το συνολικό μέγεθος όλων των βάσεων δεδομένων στο χώρο συγκέντρωσης. Αλλαγές μέσος όρος 90 λεπτά ή λιγότερο ανά 100 GB. Για παράδειγμα, εάν ο συνολικός χώρος όλων των βάσεων δεδομένων στο χώρο συγκέντρωσης είναι 200 GB, στη συνέχεια, το αναμενόμενο λανθάνων χρόνος για αλλαγή του χώρου συγκέντρωσης eDTU ανά χώρου συγκέντρωσης είναι 3 ώρες ή λιγότερο.




## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL: σφάλμα σύνδεσης και άλλα θέματα της βάσης δεδομένων](sql-database-develop-error-messages.md).
- [Βάση δεδομένων SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [APIs διαχείρισης πόρων Azure](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Δημιουργήστε ένα νέο σύνολο ελαστικότητας βάσης δεδομένων με C#](sql-database-elastic-pool-create-csharp.md)
- [Πότε θα πρέπει να χρησιμοποιείται ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων;](sql-database-elastic-pool-guidance.md)
- Ανατρέξτε στο θέμα [Κλιμάκωση ανάληψη με βάση δεδομένων SQL Azure](sql-database-elastic-scale-introduction.md): Χρησιμοποιήστε εργαλεία ελαστικότητας βάσης δεδομένων για να κλιμάκωσης, μετακίνηση δεδομένων, ερώτημα ή δημιουργήστε συναλλαγές.


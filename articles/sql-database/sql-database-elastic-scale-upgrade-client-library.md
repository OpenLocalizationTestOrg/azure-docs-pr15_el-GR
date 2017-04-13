< ιδιότητες
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Αναβάθμιση εφαρμογής για να χρησιμοποιήσετε την πιο πρόσφατη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη

Διατίθενται νέες εκδόσεις της [βιβλιοθήκης ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-database-client-library.md) έως NuGetand το περιβάλλον διαχείρισης NuGetPackage στο Visual Studio. Αναβαθμίσεις περιέχουν διορθώσεις σφαλμάτων και υποστήριξης για νέες δυνατότητες της βιβλιοθήκης του προγράμματος-πελάτη.

**Για την πιο πρόσφατη έκδοση:** Μεταβείτε στο [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Δημιουργήστε ξανά την εφαρμογή σας με τη νέα βιβλιοθήκη, καθώς και τα αλλάξετε υπάρχοντα Shard χάρτη διαχείριση μετα-δεδομένων του είναι αποθηκευμένα στις βάσεις δεδομένων SQL Azure για την υποστήριξη νέες δυνατότητες.

Εκτελέσετε αυτά τα βήματα με τη σειρά εξασφαλίζει ότι παλιές εκδόσεις της βιβλιοθήκης του προγράμματος-πελάτη δεν υπάρχουν πλέον στο περιβάλλον του τρόπου ενημέρωσης των αντικειμένων μετα-δεδομένων, γεγονός που σημαίνει ότι δεν θα δημιουργηθούν αντικειμένων μετα-δεδομένων παλιά έκδοση μετά την αναβάθμιση.   

## <a name="upgrade-steps"></a>Βήματα αναβάθμισης

**1. αναβάθμιση τις εφαρμογές σας.** Στο Visual Studio, κάντε λήψη και αναφορά την πιο πρόσφατη έκδοση του προγράμματος-πελάτη βιβλιοθήκη σε όλα τα έργα ανάπτυξης που χρησιμοποιούν τη βιβλιοθήκη. στη συνέχεια, εκ νέου δημιουργία και ανάπτυξη. 

 * Στη λύση σας Visual Studio, επιλέξτε **Εργαλεία** --> **Διαχείρισης πακέτου NuGet** -->  **Διαχείριση πακέτων NuGet για τη λύση**. 
 * (Visual Studio 2013) Στο αριστερό τμήμα του παραθύρου, επιλέξτε **ενημερώσεις**και, στη συνέχεια, επιλέξτε το κουμπί **Ενημέρωση** στη συσκευασία του **Azure SQL βάσης δεδομένων ελαστικότητας κλίμακα προγράμματος-πελάτη βιβλιοθήκη** που εμφανίζεται στο παράθυρο.
 * (Visual Studio 2015) Ορίστε το πλαίσιο φίλτρο για να **αναβαθμίσετε διαθέσιμη**. Επιλέξτε το πακέτο για να ενημερώσετε και κάντε κλικ στο κουμπί **Ενημέρωση** .
    
 
 * Δημιουργήστε και αναπτύξτε. 

**2. αναβάθμιση τις δέσμες ενεργειών.** Εάν χρησιμοποιείτε δεσμών ενεργειών του **PowerShell** για τη Διαχείριση shards, [κάντε λήψη της νέας έκδοσης βιβλιοθήκη](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) και αντιγραφή του στο στον κατάλογο από τον οποίο μπορείτε να εκτελέσετε δέσμες ενεργειών. 

**3. αναβάθμιση της υπηρεσίας διαίρεσης συγχώνευσης.** Εάν χρησιμοποιείτε το εργαλείο διαίρεσης συγχώνευσης ελαστικότητας βάσης δεδομένων για να αναδιοργάνωση sharded δεδομένα, [κάντε λήψη και να αναπτύξετε την πιο πρόσφατη έκδοση του εργαλείου](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Λεπτομερείς βήματα αναβάθμισης για την υπηρεσία μπορείτε να βρείτε [εδώ](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. αναβάθμιση τις βάσεις δεδομένων Manager χάρτη Shard**. Αναβάθμιση τα μετα-δεδομένα υποστήριξης σας χάρτες Shard σε βάση δεδομένων SQL Azure.  Υπάρχουν δύο τρόποι που μπορείτε να εκτελέσετε αυτό, με τη χρήση του PowerShell ή C#. Εμφανίζονται δύο επιλογές κάτω από το στοιχείο.

***Επιλογή 1: Αναβάθμισης μετα-δεδομένων με χρήση του PowerShell***

1. Κάντε λήψη του τελευταίου βοηθητικού προγράμματος γραμμής εντολών για NuGet από [εδώ](http://nuget.org/nuget.exe) και να αποθηκεύσετε σε ένα φάκελο. 

2. Ανοίξτε μια γραμμή εντολών, μεταβείτε στον ίδιο φάκελο και εκδώσετε την εντολή:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Μεταβείτε στον υποφάκελο που περιέχει τη νέα έκδοση του προγράμματος-πελάτη DLL που μόλις λάβατε, για παράδειγμα:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Λήψη ο κώδικας αναβάθμισης ελαστικότητας βάση δεδομένων προγράμματος-πελάτη από το [Κέντρο δεσμών ενεργειών](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)και αποθηκεύστε το στον ίδιο φάκελο που περιέχει το DLL.

5. Από αυτόν το φάκελο, εκτελέστε "PowerShell.\upgrade.ps1" από τη γραμμή εντολών και ακολουθήστε τις οδηγίες.
 
***Επιλογή 2: Αναβάθμισης μετα-δεδομένων με χρήση C#***

Εναλλακτικά, μπορείτε να δημιουργήσετε μια εφαρμογή του Visual Studio που ανοίγει το ShardMapManager, επαναλαμβάνεται επάνω από όλα shards και εκτελεί την αναβάθμιση μετα-δεδομένων, καλώντας τις μεθόδους [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) και [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) όπως αυτό το παράδειγμα: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Αυτές οι τεχνικές για αναβαθμίσεις μετα-δεδομένων μπορούν να εφαρμοστούν πολλές φορές χωρίς βλάβη. Για παράδειγμα, εάν μια παλαιότερη έκδοση του προγράμματος-πελάτη δημιουργεί κατά λάθος μια shard μετά την ενημέρωση ήδη, μπορείτε να εκτελέσετε αναβάθμιση ξανά σε όλους shards για να βεβαιωθείτε ότι υπάρχει την πιο πρόσφατη έκδοση μετα-δεδομένων σε όλη την υποδομή σας. 

**Σημείωση:**  Νέες εκδόσεις της βιβλιοθήκης του προγράμματος-πελάτη δημοσιευτεί σε ημερομηνία συνεχίσετε να εργάζεστε με προηγούμενες εκδόσεις της τα μετα-δεδομένα Shard χάρτη Manager Azure SQL DB και το αντίστροφο.   Ωστόσο, για να επωφεληθείτε από ορισμένες από τις νέες δυνατότητες στο πρόγραμμα-πελάτη πιο πρόσφατη, πρέπει να αναβαθμιστεί μετα-δεδομένων.   Σημειώστε ότι οι αναβαθμίσεις μετα-δεδομένων δεν θα επηρεάσει οποιαδήποτε δεδομένα χρήστη ή συγκεκριμένη εφαρμογή δεδομένων, μόνο αντικείμενα δημιουργούνται και χρησιμοποιούνται από τη Διαχείριση Shard χάρτη.  Και εφαρμογές εξακολουθούν να λειτουργούν μέσω της ακολουθίας αναβάθμισης που περιγράφονται παραπάνω. 

## <a name="elastic-database-client-version-history"></a>Ιστορικό εκδόσεων ελαστικότητας βάση δεδομένων προγράμματος-πελάτη 

Για το ιστορικό εκδόσεων, ανατρέξτε στο θέμα [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 
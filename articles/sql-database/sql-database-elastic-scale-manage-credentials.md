<properties 
    pageTitle="Διαχείριση διαπιστευτηρίων στη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη | Microsoft Azure" 
    description="Πώς μπορείτε να ορίσετε το κατάλληλο επίπεδο διαπιστευτηρίων, διαχείρισης μόνο για ανάγνωση, για τις εφαρμογές ελαστικότητας βάσης δεδομένων" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Διαπιστευτήρια που χρησιμοποιούνται για να αποκτήσετε πρόσβαση στη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη

Η [βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) χρησιμοποιεί τρία διαφορετικά είδη διαπιστευτήρια για να αποκτήσετε πρόσβαση στη [Διαχείριση χάρτη shard](sql-database-elastic-scale-shard-map-management.md). Ανάλογα με την ανάγκη, χρησιμοποιήστε τα διαπιστευτήρια με το χαμηλότερο επίπεδο πρόσβασης πιθανές.

* **Διαχείριση διαπιστευτηρίων**: για τη δημιουργία ή χειρισμός Διαχείριση χάρτη shard. (Ανατρέξτε στο θέμα [Γλωσσάρι](sql-database-elastic-scale-glossary.md).) 
* **Διαπιστευτήρια πρόσβασης**: για να αποκτήσετε πρόσβαση σε μια υπάρχουσα Διαχείριση χάρτη shard για τη λήψη πληροφοριών σχετικά με το shards.
* **Τα διαπιστευτήρια σύνδεσης**: για να συνδεθείτε με shards. 

Δείτε επίσης: [Διαχείριση βάσεων δεδομένων και συνδέσεις σε βάση δεδομένων SQL Azure](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Σχετικά με τη Διαχείριση διαπιστευτηρίων

Διαχείριση διαπιστευτηρίων που χρησιμοποιούνται για τη δημιουργία ενός αντικειμένου [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) για τις εφαρμογές που να χειρίζονται shard χάρτες. (Για παράδειγμα, ανατρέξτε στο θέμα [Προσθήκη ενός shard χρησιμοποιώντας εργαλεία ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-add-a-shard.md) και [εξαρτώμενα δρομολόγηση δεδομένων](sql-database-elastic-scale-data-dependent-routing.md)) Ο χρήστης της βιβλιοθήκης του προγράμματος-πελάτη ελαστικότητας κλίμακα δημιουργεί τους χρήστες SQL και τις συνδέσεις SQL και εξασφαλίζει ότι κάθε έχει παραχωρηθεί τα δικαιώματα ανάγνωσης/εγγραφής για τη βάση δεδομένων χάρτη καθολικού shard όλα shard καθώς και βάσεις δεδομένων. Αυτά τα διαπιστευτήρια που χρησιμοποιούνται για να διατηρήσετε το χάρτη καθολικού shard και τις αντιστοιχίσεις τοπικό shard όταν εκτελούνται αλλαγές στο χάρτη shard. Για παράδειγμα, χρησιμοποιήστε τα διαπιστευτήρια διαχείρισης για να δημιουργήσετε το αντικείμενο shard χάρτης διαχείρισης (χρήση [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Η μεταβλητή **smmAdminConnectionString** είναι μια συμβολοσειρά σύνδεσης που περιέχει τα διαπιστευτήρια διαχείρισης. Το Αναγνωριστικό χρήστη και κωδικό πρόσβασης παρέχει πρόσβαση ανάγνωσης/εγγραφής σε βάση δεδομένων χάρτη shard και μεμονωμένες shards. Η συμβολοσειρά σύνδεσης διαχείρισης περιλαμβάνει επίσης το όνομα του διακομιστή και όνομα βάσης δεδομένων για τον προσδιορισμό της βάσης δεδομένων χάρτη καθολικού shard. Ακολουθεί μια συμβολοσειρά σύνδεσης τυπικές για το σκοπό αυτό:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Μην χρησιμοποιείτε τιμές με τη μορφή "username@server"—instead απλώς να χρησιμοποιήσετε την τιμή "όνομα χρήστη".  Αυτό συμβαίνει επειδή τα διαπιστευτήρια πρέπει να εργαστείτε σε τη βάση δεδομένων διαχείρισης χάρτη shard και μεμονωμένες shards, η οποία μπορεί να είναι σε διαφορετικούς διακομιστές.

## <a name="access-credentials"></a>Διαπιστευτήρια πρόσβασης
  
Κατά τη δημιουργία ενός shard manager χάρτη σε μια εφαρμογή που δεν Διαχείριση shard χάρτες, χρησιμοποιήστε τα διαπιστευτήρια που έχουν δικαιώματα μόνο για ανάγνωση στο καθολικό shard χάρτη. Οι πληροφορίες που ανακτήθηκαν από την αντιστοίχιση καθολικού shard στην περιοχή αυτά τα διαπιστευτήρια που χρησιμοποιούνται για [τη δρομολόγηση εξαρτώνται από δεδομένα](sql-database-elastic-scale-data-dependent-routing.md) και για να συμπληρώσετε το cache χάρτη shard του υπολογιστή-πελάτη. Τα διαπιστευτήρια που παρέχονται μέσω το ίδιο μοτίβο κλήσης για να **GetSqlShardMapManager** , όπως φαίνεται παραπάνω: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Σημειώστε τη χρήση του **smmReadOnlyConnectionString** για να απεικονίσει τη χρήση της διαφορετικά διαπιστευτήρια για πρόσβαση σε αυτήν τη εκ μέρους οι χρήστες **δεν είναι διαχειριστές** : αυτά τα διαπιστευτήρια δεν θα πρέπει να παρέχει δικαιώματα εγγραφής στο καθολικό shard χάρτη. 

## <a name="connection-credentials"></a>Τα διαπιστευτήρια σύνδεσης 

Απαιτούνται διαπιστευτήρια επιπλέον όταν χρησιμοποιείτε τη μέθοδο [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) για να αποκτήσετε πρόσβαση σε μια shard που σχετίζεται με έναν αριθμό-κλειδί sharding. Αυτά τα διαπιστευτήρια πρέπει να δώσετε δικαιώματα για πρόσβαση μόνο για ανάγνωση με τους πίνακες χάρτη τοπικό shard που βρίσκονται σε το shard. Αυτό είναι απαραίτητο για την εκτέλεση επικύρωση σύνδεσης για τη δρομολόγηση εξαρτώνται από δεδομένα σε το shard. Σε αυτό το τμήμα κώδικα επιτρέπει πρόσβαση σε δεδομένα στο περιβάλλον της εξαρτώμενα δρομολόγηση δεδομένων: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Σε αυτό το παράδειγμα, **smmUserConnectionString** περιέχει τη συμβολοσειρά σύνδεσης για τα διαπιστευτήρια του χρήστη. Για Azure SQL DB, ακολουθεί μια συμβολοσειρά σύνδεσης τυπικές για τα διαπιστευτήρια χρήστη: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Όπως συμβαίνει με τα διαπιστευτήρια διαχειριστή, όχι τις τιμές με τη μορφή "username@server". Αντί για αυτό, χρησιμοποιήστε το "όνομα χρήστη".  Επίσης, σημειώστε ότι η συμβολοσειρά σύνδεσης δεν περιέχει ένα όνομα διακομιστή και το όνομα της βάσης δεδομένων. Αυτό συμβαίνει επειδή η κλήση **OpenConnectionForKey** θα κατευθύνουν αυτόματα η σύνδεση με το σωστό shard με βάση τον αριθμό-κλειδί. Επομένως, το όνομα της βάσης δεδομένων και το όνομα του διακομιστή δεν παρέχονται. 

## <a name="see-also"></a>Δείτε επίσης
[Διαχείριση βάσεων δεδομένων και συνδέσεις σε βάση δεδομένων SQL Azure](sql-database-manage-logins.md)

[Ασφάλιση της βάσης δεδομένων SQL](sql-database-security.md)

[Γρήγορα αποτελέσματα με τις εργασίες ελαστικότητας βάση δεδομένων](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
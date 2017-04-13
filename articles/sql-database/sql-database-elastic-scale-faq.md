<properties 
    pageTitle="Azure SQL ελαστικότητας κλίμακα συνήθεις Ερωτήσεις | Microsoft Azure" 
    description="Συνήθεις ερωτήσεις σχετικά με την κλίμακα ελαστικότητας βάσης δεδομένων Azure SQL." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Εργαλεία βάσης δεδομένων ελαστικότητας συνήθεις Ερωτήσεις 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Εάν έχω ένα μονό μισθωτή ανά shard και χωρίς κλειδί sharding, πώς μπορώ να συμπληρώσετε το κλειδί sharding για τις πληροφορίες σχήματος;

Το αντικείμενο πληροφορίες σχήματος χρησιμοποιείται μόνο για να διαιρέσετε σενάρια συγχώνευσης. Εάν μια εφαρμογή είναι εγγενώς μίας μισθωτή, στη συνέχεια, δεν απαιτεί το εργαλείο διαίρεσης συγχώνευση και, επομένως, δεν χρειάζεται να συμπληρώσετε το αντικείμενο πληροφορίες σχήματος.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Να έχετε παρασχεθεί μια βάση δεδομένων και έχω ήδη διαχειριστής Shard χάρτη, πώς μπορώ να εγγραφών αυτήν τη νέα βάση δεδομένων ως μια shard;

Ανατρέξτε στο θέμα **[Προσθήκη ενός shard σε μια εφαρμογή χρησιμοποιώντας τη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Πόσο κοστίζει εργαλεία ελαστικότητας βάσης δεδομένων;

Χρήση της βιβλιοθήκης του προγράμματος-πελάτη ελαστικότητας βάσης δεδομένων δεν γίνονται με οποιοδήποτε κόστος. Προσαύξηση κόστους μόνο για τις βάσεις δεδομένων Azure SQL που χρησιμοποιείτε για shards και τη Διαχείριση Shard χάρτη, καθώς και τους ρόλους web/εργαζόμενου που προμήθεια για το εργαλείο συγχώνευση η διαίρεση.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Γιατί είναι τα διαπιστευτήριά μου δεν λειτουργεί όταν προσθέτω μια shard από διαφορετικό διακομιστή;
Μην χρησιμοποιείτε τα διαπιστευτήρια με τη μορφή "χρήστη ID=username@servername”, αντί για αυτό απλώς να χρησιμοποιήσουμε" Αναγνωριστικό χρήστη = όνομα χρήστη ".  Επίσης, βεβαιωθείτε ότι η σύνδεση "όνομα χρήστη" έχει δικαιώματα στην το shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Πρέπει να δημιουργήσετε Διαχείριση χάρτη Shard και συμπληρώσετε shards κάθε φορά που ξεκινάτε το οι εφαρμογές μου;

Όχι-η δημιουργία της διαχείρισης χάρτη Shard (για παράδειγμα, **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) είναι μια μεμονωμένη λειτουργία.  Η εφαρμογή σας πρέπει να χρησιμοποιήσετε την κλήση **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** κατά το χρόνο εκκίνησης εφαρμογών.  Θα πρέπει να μόνο μία όπως κλήση ανά τομέα εφαρμογής.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Να έχετε ερωτήσεις σχετικά με τη χρήση εργαλεία ελαστικότητας βάσης δεδομένων, πώς μπορώ να αποκτήσω τις απαντήσεις; 

Επικοινωνήστε επικοινωνία μας στο [φόρουμ βάση δεδομένων SQL Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Όταν λαμβάνω μια σύνδεση βάσης δεδομένων χρησιμοποιώντας έναν αριθμό-κλειδί sharding, να μπορεί να εξακολουθεί να ερωτήματος δεδομένων για άλλα πλήκτρα sharding στην το ίδιο shard.  Αυτό είναι από τη σχεδίαση;

Τα ελαστικά API κλίμακα σας δώσει μια σύνδεση με τη σωστή βάση δεδομένων για τον αριθμό-κλειδί sharding, αλλά δεν παρέχουν βασικές φιλτράρισμα sharding.  Προσθέσετε **όπου** όρους στο ερώτημά σας για να περιορίσετε το εύρος στο κλειδί παρεχόμενη sharding, εάν είναι απαραίτητο.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Μπορώ να χρησιμοποιήσω μια διαφορετική έκδοση του Azure βάσης δεδομένων για κάθε shard μου συνόλου shard;

Ναι, μια shard είναι μια μεμονωμένη βάση δεδομένων και, επομένως, μία shard μπορεί να είναι μια έκδοση Premium ενώ μια άλλη είναι μια τυπική έκδοση. Επιπλέον, την έκδοση του ένα shard μπορεί να κλίμακα προς τα επάνω ή προς τα κάτω πολλές φορές κατά τη διάρκεια ζωής του shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Κάνει την παροχή εργαλείο συγχώνευσης διαίρεσης (ή να διαγράψετε) μια βάση δεδομένων κατά τη διάρκεια μιας λειτουργίας διαίρεσης ή τη συγχώνευση; 

Όχι. Για λειτουργίες **Διαίρεση** , τη βάση δεδομένων προορισμού πρέπει να υπάρχει με το κατάλληλο σχήμα και έχει εγγραφεί με τη Διαχείριση χάρτη Shard.  Για **τη συγχώνευση** λειτουργίες, να πρέπει να διαγράψετε το shard από το διαχειριστή του χάρτη shard και να διαγράψετε τη βάση δεδομένων.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
<properties 
    pageTitle="Υποβολή ερωτημάτων πολλών shard | Microsoft Azure" 
    description="Εκτέλεση ερωτημάτων σε shards χρήση της βιβλιοθήκης ελαστικότητας βάση δεδομένων προγράμματος-πελάτη." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Υποβολή ερωτημάτων πολλών shard

## <a name="overview"></a>Επισκόπηση

Με τα [Εργαλεία ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-introduction.md), μπορείτε να δημιουργήσετε λύσεις sharded βάσεων δεδομένων. **Υποβολή ερωτημάτων πολλών shard** χρησιμοποιείται για εργασίες όπως η συλλογή/αναφορά δεδομένων που απαιτούν την εκτέλεση ενός ερωτήματος που παραμόρφωση σε διάφορες shards. (Έρχεται σε αντιδιαστολή [δεδομένων εξαρτώνται από τη δρομολόγηση](sql-database-elastic-scale-data-dependent-routing.md), που εκτελεί όλες τις εργασίες σε μια μεμονωμένη shard.) 

## <a name="overview"></a>Επισκόπηση

1. Λάβετε μια [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) ή [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) χρησιμοποιώντας το [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), το [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)ή τη μέθοδο [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) . Ανατρέξτε στο θέμα [**η κατασκευή ενός ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) και να [**λάβετε μια RangeShardMap ή ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Δημιουργήστε ένα αντικείμενο **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** .
2. Δημιουργήστε μια **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Ορίστε την **[ιδιότητα κείμενο-εντολής](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** σε μια εντολή T-SQL.
3. Εκτελέστε την εντολή καλώντας τη **[μέθοδο ExecuteReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Προβάλετε τα αποτελέσματα με την **[κλάση MultiShardDataReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Παράδειγμα

Ο ακόλουθος κώδικας απεικονίζει τη χρήση των πολλών shard υποβολή ερωτημάτων με χρήση μιας δεδομένης **ShardMap** με το όνομα *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Μια βασική διαφορά είναι η κατασκευή shard πολλών συνδέσεων. Το σημείο όπου **SqlConnection** λειτουργεί σε μία βάση δεδομένων, το **MultiShardConnection** λαμβάνει μια ***συλλογή από shards*** ως είσοδο. Συμπληρώστε τη συλλογή του shards από ένα χάρτη shard. Το ερώτημα εκτελείται, στη συνέχεια, στη συλλογή των shards χρησιμοποιώντας **UNION ALL** σημασιολογία να συγκεντρώσετε ένα μεμονωμένο αποτέλεσμα συνολική. Προαιρετικά, μπορείτε να προσθέσετε το όνομα του το shard όπου η γραμμή προέρχεται από στο αποτέλεσμα του χρησιμοποιώντας την ιδιότητα **ExecutionOptions** στην εντολή. 

Σημειώστε την κλήση σε **myShardMap.GetShards()**. Αυτή η μέθοδος ανακτά όλα shards από την αντιστοίχιση shard και παρέχει έναν εύκολο τρόπο για να εκτελέσετε ένα ερώτημα σε όλες τις σχετικές βάσεις δεδομένων. Η συλλογή shards για ένα ερώτημα πολλών shard μπορεί να είναι εκλεπτυσμένη περαιτέρω κατά την εκτέλεση ενός ερωτήματος LINQ επάνω από τη συλλογή επιστρέφονται από την κλήση σε **myShardMap.GetShards()**. Σε συνδυασμό με την πολιτική μερική αποτελεσμάτων, το τρέχον δυνατότητα κατά την υποβολή ερωτημάτων πολλών shard έχει σχεδιαστεί ώστε να λειτουργεί καλά για δεκάδες έως εκατοντάδες shards.

Περιορισμός με την υποβολή ερωτημάτων πολλών shard είναι αυτήν τη στιγμή η έλλειψη επικύρωσης για shards και shardlets που υποβάλλεται ερώτημα. Ενώ δεδομένων εξαρτώνται από τη δρομολόγηση επαληθεύει ότι ένα δεδομένο shard αποτελεί μέρος του χάρτη shard κατά την υποβολή ερωτημάτων, ερωτήματα πολλών shard μην εκτελέσετε αυτόν τον έλεγχο. Αυτό μπορεί να οδηγήσει σε ερωτήματα πολλών shard εκτελείται σε βάσεις δεδομένων που έχουν καταργηθεί από την αντιστοίχιση shard.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Πολλαπλή shard ερωτήματα και εργασίες διαίρεσης συγχώνευσης

Ερωτήματα πολλών shard επαληθεύει εάν shardlets στη βάση δεδομένων του ερωτήματος συμμετέχουν σε συνεχή λειτουργίες διαίρεσης συγχώνευσης. (Ανατρέξτε στο θέμα [Κλιμάκωση με το εργαλείο διαίρεσης συγχώνευσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-overview-split-and-merge.md)). Αυτό μπορεί να οδηγήσει σε ασυνέπειες όπου τις γραμμές από την ίδια shardlet εμφάνιση για πολλές βάσεις δεδομένων στο ίδιο ερώτημα πολλών shard. Προσέχετε αυτούς τους περιορισμούς και εξετάστε το ενδεχόμενο να απορροής λειτουργίες διαίρεσης συγχώνευσης βρίσκεται σε εξέλιξη και τις αλλαγές στο χάρτη shard κατά την εκτέλεση πολλαπλών shard ερωτήματα.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Δείτε επίσης
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** κλάσεις και οι μέθοδοι.


Διαχείριση shards χρησιμοποιώντας τη [βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-database-client-library.md). Περιλαμβάνει ένα χώρο ονομάτων που ονομάζεται [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) που παρέχει τη δυνατότητα να ερωτήματος πολλών shards χρησιμοποιώντας ένα μεμονωμένο ερωτήματος και το αποτέλεσμα. Παρέχει μια κατά την υποβολή ερωτήματος αφαίρεσης επάνω από μια συλλογή από shards. Παρέχει επίσης εκτέλεσης εναλλακτικές πολιτικές, ιδίως μερική αποτελέσματα, για να αντιμετωπίσετε αποτυχίες κατά την υποβολή ερωτήματος σε πολλές shards.  

 
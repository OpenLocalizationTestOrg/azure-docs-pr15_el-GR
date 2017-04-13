<properties 
    pageTitle="Προσθήκη μιας shard χρησιμοποιώντας εργαλεία ελαστικότητας βάσης δεδομένων | Microsoft Azure" 
    description="Ορίστε τον τρόπο χρήσης του ελαστικότητας APIs κλίμακα για να προσθέσετε νέα shards σε μια shard." 
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

# <a name="adding-a-shard-using-elastic-database-tools"></a>Προσθήκη μιας shard χρησιμοποιώντας εργαλεία ελαστικότητας βάσης δεδομένων

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Για να προσθέσετε μια shard για μια νέα περιοχή ή αριθμού-κλειδιού  

Εφαρμογές συχνά πρέπει απλώς να προσθέσετε το νέο shards χειρισμού των δεδομένων που αναμένεται από νέα κλειδιά ή βασικές περιοχές, για χάρτη shard που υπάρχει ήδη. Για παράδειγμα, μια εφαρμογή του sharded κατά Αναγνωριστικό μισθωτή μπορεί να χρειαστεί να προμηθεύσουν ένα νέο shard για ένα νέο μισθωτή ή sharded μηνιαία δεδομένων μπορεί να χρειαστεί μια νέα shard παρασχεθεί πριν από την έναρξη κάθε νέο μήνα. 

Εάν τη νέα περιοχή τιμών κλειδιού δεν είναι ήδη τμήμα μιας υπάρχουσας αντιστοίχισης, είναι πολύ απλό για να προσθέσετε τη νέα shard και να συσχετίσετε το νέο αριθμό-κλειδί ή την περιοχή για συγκεκριμένη shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Παράδειγμα: Προσθήκη μιας shard και της περιοχής σε μια υπάρχουσα αντιστοίχιση shard
Αυτό το δείγμα χρησιμοποιεί το [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), μεθόδους [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) , και δημιουργεί μια παρουσία της κλάσης [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . Το παρακάτω δείγμα, έχουν δημιουργηθεί μια βάση δεδομένων με το όνομα **sample_shard_2** και όλα τα αντικείμενα είναι απαραίτητο σχήμα μέσα σε αυτό για τη διατήρηση περιοχής [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Ως εναλλακτική λύση, μπορείτε να χρησιμοποιήσετε Powershell για να δημιουργήσετε μια νέα Διαχείριση Shard στο χάρτη. Ένα παράδειγμα είναι διαθέσιμη [εδώ](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Για να προσθέσετε μια shard για ένα κενό τμήμα μιας υπάρχουσας περιοχής  

Σε ορισμένες περιπτώσεις, ενδέχεται να έχετε ήδη αντιστοιχιστεί σε περιοχή σε ένα shard και μερικώς συμπληρωμένο το με τα δεδομένα, αλλά τώρα θέλετε επερχόμενες δεδομένων να κατευθύνονται σε μια διαφορετική shard. Για παράδειγμα, μπορείτε shard κατά περιοχή ημέρα και έχουν ήδη εκχωρημένων 50 ημέρες για μια shard, αλλά την ημέρα 24, θέλετε να ξεκινάτε με ένα διαφορετικό shard μελλοντική δεδομένων. Το ελαστικότητας βάσης δεδομένων [διαιρεμένης συγχώνευσης εργαλείο](sql-database-elastic-scale-overview-split-and-merge.md) μπορεί να εκτελέσει αυτήν τη λειτουργία, αλλά εάν κίνηση δεδομένων δεν είναι απαραίτητα (για παράδειγμα, τα δεδομένα για την περιοχή των ημερών [25, 50), π.χ., ημέρα 25 έως 50 για αποκλειστική χρήση, δεν υπάρχει ακόμα) μπορείτε να εκτελέσετε αυτή εντελώς χρησιμοποιώντας τα API διαχείρισης χάρτη Shard απευθείας.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Παράδειγμα: η διαίρεση μιας περιοχής και εκχώρηση στο κενό τμήμα μιας shard που μόλις προστέθηκε

Έχει δημιουργηθεί μια βάση δεδομένων με το όνομα "sample_shard_2" και όλα τα αντικείμενα είναι απαραίτητο σχήμα μέσα σε αυτό.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**ΣΗΜΑΝΤΙΚΟ**: Χρησιμοποιήστε αυτήν την τεχνική, μόνο αν είστε βέβαιοι ότι η περιοχή για την αντιστοίχιση ενημερωμένη είναι κενό.  Τις παραπάνω μεθόδους δεν επιλέξτε δεδομένα για την περιοχή για τη μετακίνηση, ώστε να είναι καλύτερα να συμπεριλάβετε ελέγχων στον κώδικά σας.  Αν υπάρχουν γραμμές της περιοχής για τη μετακίνηση, την κατανομή πραγματικά δεδομένα δεν θα αντιστοιχεί με το χάρτη ενημερωμένων shard. Χρησιμοποιήστε το [εργαλείο διαίρεσης συγχώνευσης](sql-database-elastic-scale-overview-split-and-merge.md) για την εκτέλεση της λειτουργίας αντί για αυτό σε αυτές τις περιπτώσεις.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 

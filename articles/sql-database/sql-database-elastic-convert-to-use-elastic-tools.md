<properties
   pageTitle="Μετεγκατάσταση υπάρχουσες βάσεις δεδομένων με κλίμακα | Microsoft Azure"
   description="Μετατροπή sharded βάσεων δεδομένων για να χρησιμοποιήσετε εργαλεία ελαστικότητας βάσης δεδομένων, δημιουργώντας μια shard manager χάρτη"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Μετεγκατάσταση υπάρχουσες βάσεις δεδομένων με κλίμακα

Διαχειριστείτε εύκολα τις υπάρχουσες κλιμάκωση ανάληψης sharded βάσεις δεδομένων με βάση δεδομένων SQL Azure Εργαλεία βάσης δεδομένων (όπως η [βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-database-client-library.md)). Πρέπει πρώτα να μετατρέψετε ένα υπάρχον σύνολο των βάσεων δεδομένων για να χρησιμοποιήσετε το [shard αντιστοίχιση manager](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Επισκόπηση
Για τη μετεγκατάσταση μιας υπάρχουσας βάσης δεδομένων sharded: 

1. Προετοιμασία τη [βάση δεδομένων διαχείρισης χάρτη shard](sql-database-elastic-scale-shard-map-management.md).
2. Δημιουργία χάρτη shard.
3. Προετοιμασία τα μεμονωμένα shards.  
2. Προσθήκη αντιστοιχίσεων στο χάρτη shard.

Αυτές οι τεχνικές μπορεί να υλοποιηθεί χρησιμοποιώντας τη [βιβλιοθήκη προγράμματος-πελάτη .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)ή των δεσμών ενεργειών του PowerShell βρίσκεται στο [SQL Azure DB - δέσμες ενεργειών εργαλεία ελαστικότητας βάσης δεδομένων](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Δείτε τα παραδείγματα χρησιμοποιούν τις δέσμες ενεργειών PowerShell.

Για περισσότερες πληροφορίες σχετικά με το ShardMapManager, ανατρέξτε στο θέμα [Χάρτης διαχείρισης Shard](sql-database-elastic-scale-shard-map-management.md). Για μια επισκόπηση των εργαλείων ελαστικότητας βάσης δεδομένων, ανατρέξτε στο θέμα [επισκόπηση δυνατοτήτων ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Προετοιμασία της βάσης δεδομένων shard χάρτη manager

Η διαχείριση χάρτη shard είναι μια ειδική βάση δεδομένων που περιέχει τα δεδομένα για τη Διαχείριση βάσεων δεδομένων κλιμάκωση ελέγχου. Μπορείτε να χρησιμοποιήσετε μια υπάρχουσα βάση δεδομένων ή να δημιουργήσετε μια νέα βάση δεδομένων. Σημειώστε ότι μια βάση δεδομένων, ως shard χάρτη manager δεν πρέπει να την ίδια βάση δεδομένων με μια shard. Επίσης, σημειώστε ότι η δέσμη ενεργειών PowerShell δεν δημιουργεί τη βάση δεδομένων για εσάς. 

## <a name="step-1-create-a-shard-map-manager"></a>Βήμα 1: Δημιουργήστε μια shard manager χάρτη

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Για να ανακτήσετε τη Διαχείριση shard χάρτη

Μετά τη δημιουργία, μπορείτε να ανακτήσετε τη Διαχείριση χάρτη shard με αυτό το cmdlet. Αυτό το βήμα είναι απαραίτητο, κάθε φορά που θα πρέπει να χρησιμοποιήσετε το αντικείμενο ShardMapManager.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Βήμα 2: δημιουργία του χάρτη shard

Πρέπει να επιλέξετε τον τύπο του shard αντιστοίχιση για να δημιουργήσετε. Η επιλογή εξαρτάται από την αρχιτεκτονική βάσης δεδομένων: 

1. Μία μισθωτή ανά βάση δεδομένων (για τους όρους, ανατρέξτε στο [Γλωσσάρι](sql-database-elastic-scale-glossary.md).) 
2. Πολλούς μισθωτές ανά βάση δεδομένων (δύο τύποι):
    3. Αντιστοίχιση λίστας
    4. Περιοχή αντιστοίχισης
 

Για ένα μοντέλο μίας μισθωτή, δημιουργήστε μια αντιστοίχιση shard **αντιστοίχιση της λίστας** . Το μοντέλο μίας μισθωτή αντιστοιχίζει μία βάση δεδομένων ανά μισθωτή. Αυτή είναι μια αποτελεσματική μοντέλου για τους προγραμματιστές ΑΔΑ όπως απλοποιεί την διαχείρισης.

![Αντιστοίχιση λίστας][1]

Το μοντέλο πολλών μισθωτών εκχωρεί πολλούς μισθωτές σε μία βάση δεδομένων (και μπορείτε να διανείμετε ομάδες μισθωτές σε πολλές βάσεις δεδομένων). Χρησιμοποιήστε αυτό το μοντέλο όταν αναμένετε κάθε μισθωτή να έχουν ανάγκες μικρές δεδομένων. Σε αυτό το μοντέλο, θα σας να αντιστοιχίσετε μια περιοχή μισθωτές σε μια βάση δεδομένων με χρήση **περιοχή αντιστοίχισης**. 
 

![Περιοχή αντιστοίχισης][2]

Ή μπορείτε να υλοποιήσετε ένα μοντέλο βάσης δεδομένων πολλών μισθωτών χρησιμοποιώντας *αντιστοίχιση της λίστας* για να αντιστοιχίσετε πολλούς μισθωτές σε μία βάση δεδομένων. Για παράδειγμα, Φθίνουσα 1 χρησιμοποιείται για την αποθήκευση πληροφοριών σχετικά με το αναγνωριστικό μισθωτή 1 και 5 και DB2 αποθηκεύει τα δεδομένα για το μισθωτή 7 και μισθωτή 10. 

![Πολλαπλές μισθωτές σε μία μόνο DB][3] 

**Με βάση την επιλογή σας, επιλέξτε μία από τις εξής επιλογές:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Επιλογή 1: Δημιουργία χάρτη shard για μια αντιστοίχιση λίστας
Δημιουργία χάρτη shard χρησιμοποιώντας το αντικείμενο ShardMapManager. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Επιλογή 2: Δημιουργία χάρτη shard για την αντιστοίχιση μιας περιοχής

Σημειώστε ότι για να χρησιμοποιούν αυτήν την αντιστοίχιση μοτίβο, αναγνωριστικό μισθωτή πρέπει να είναι συνεχής περιοχές τιμών και είναι αποδεκτό να έχουν διάκενο στις περιοχές παραλείποντας απλώς στην περιοχή κατά τη δημιουργία των βάσεων δεδομένων.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Επιλογή 3: Λίστα αντιστοιχίσεις σε μία βάση δεδομένων
Ρύθμιση αυτό το μοτίβο απαιτεί επίσης τη δημιουργία ενός χάρτη λίστα όπως φαίνεται στο βήμα 2, η επιλογή 1.

## <a name="step-3-prepare-individual-shards"></a>Βήμα 3: Προετοιμασία μεμονωμένα shards

Προσθέστε κάθε shard (βάση δεδομένων) για τη Διαχείριση shard χάρτη. Προετοιμάζει τις μεμονωμένες βάσεις δεδομένων για την αποθήκευση πληροφοριών για την αντιστοίχιση. Εκτέλεση της μεθόδου σε κάθε shard.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Βήμα 4: Προσθήκη αντιστοιχίσεων

Η προσθήκη της αντιστοιχίσεων εξαρτάται από το είδος των shard χάρτη που δημιουργήσατε. Εάν έχετε δημιουργήσει ένα χάρτη λίστας, μπορείτε να προσθέσετε αντιστοιχίσεις λίστας. Εάν έχετε δημιουργήσει ένα χάρτη περιοχή, μπορείτε να προσθέσετε αντιστοιχίσεις περιοχή.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Επιλογή 1: αντιστοιχίστε τα δεδομένα για την αντιστοίχιση μιας λίστας

Αντιστοίχιση των δεδομένων με την προσθήκη αντιστοίχισης λίστας για κάθε μισθωτή.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Επιλογή 2: αντιστοιχίστε τα δεδομένα για την αντιστοίχιση μιας περιοχής

Προσθέστε τις αντιστοιχίσεις περιοχής για όλα τα μισθωτή τιμές αναγνωριστικού – συσχετίσεων βάσης δεδομένων:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Βήμα 4 επιλογή 3: αντιστοιχίστε τα δεδομένα για πολλούς μισθωτές σε μία βάση δεδομένων

Για κάθε μισθωτή, εκτελέστε την προσθήκη-ListMapping (option 1, η παραπάνω). 


## <a name="checking-the-mappings"></a>Έλεγχος των αντιστοιχίσεων

Πληροφορίες σχετικά με την υπάρχουσα shards και τις αντιστοιχίσεις που σχετίζονται με τους μπορούν να αναζητηθούν με χρήση του παρακάτω εντολές:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Σύνοψη

Αφού ολοκληρώσετε τη ρύθμιση, μπορείτε να αρχίσετε να χρησιμοποιείτε τη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη. Μπορείτε επίσης να χρησιμοποιήσετε [δεδομένα εξαρτώμενα δρομολόγηση](sql-database-elastic-scale-data-dependent-routing.md) και [πολλών shard ερωτήματος](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Επόμενα βήματα


Λήψη των δεσμών ενεργειών του PowerShell από [sripts Εργαλεία βάσης δεδομένων DB ελαστικές SQL Azure](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Τα εργαλεία είναι επίσης στις GitHub: [Azure/ελαστικές-db-εργαλεία](https://github.com/Azure/elastic-db-tools).

Χρησιμοποιήστε το εργαλείο διαίρεσης συγχώνευσης για τη μετακίνηση δεδομένων σε ή από ένα μοντέλο πολλών μισθωτών σε ένα μοντέλο μόνο μισθωτή. Ανατρέξτε στο θέμα [Διαίρεση συγχώνευσης εργαλείο](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Πρόσθετοι πόροι

Για πληροφορίες σχετικά με κοινές μοτίβα αρχιτεκτονική δεδομένων πολλών μισθωτών λογισμικού-ως-a-(ΑΠΑ) βάση δεδομένων εφαρμογών υπηρεσίας, ανατρέξτε στο θέμα [Σχεδίαση μοτίβα για εφαρμογές ΑΔΑ πολλών μισθωτή με βάση δεδομένων SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Ερωτήσεις και αιτήσεις για δυνατότητες

Για ερωτήσεις, λάβετε επικοινωνία μας στο [φόρουμ βάση δεδομένων SQL](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) και για αιτήσεις για δυνατότητες, προσθέστε τους στο [φόρουμ σχολίων βάση δεδομένων SQL](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 

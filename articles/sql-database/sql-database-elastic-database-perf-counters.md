<properties
    pageTitle="Μετρητές επιδόσεων για διαχείριση shard χάρτη"
    description="ShardMapManager κλάσης και δεδομένων εξαρτώμενα δρομολόγησης μετρητές επιδόσεων"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Μετρητές επιδόσεων για διαχείριση shard χάρτη

Μπορείτε να καταγράψετε τις επιδόσεις της [shard χάρτη manager](sql-database-elastic-scale-shard-map-management.md), ιδίως όταν χρησιμοποιείτε [εξαρτώμενα δρομολόγηση δεδομένων](sql-database-elastic-scale-data-dependent-routing.md). Μετρητές δημιουργούνται με μεθόδους της κλάσης Microsoft.Azure.SqlDatabase.ElasticScale.Client.  

Μετρητές που χρησιμοποιούνται για να παρακολουθήσετε την απόδοση των λειτουργιών [δεδομένων εξαρτώμενα δρομολόγηση](sql-database-elastic-scale-data-dependent-routing.md) . Αυτοί οι μετρητές είναι προσβάσιμα στην Εποπτεία επιδόσεων, κάτω από την κατηγορία "Ελαστικότητας Διαχείριση βάσης δεδομένων: Shard".

**Για την πιο πρόσφατη έκδοση:** Μεταβείτε στο [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Ανατρέξτε επίσης στο θέμα [αναβάθμιση μιας εφαρμογής για να χρησιμοποιήσετε την πιο πρόσφατη βιβλιοθήκη ελαστικότητας βάση δεδομένων προγράμματος-πελάτη](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Για να δημιουργήσετε την κατηγορία επιδόσεων και μετρητές, ο χρήστης πρέπει να είναι ένα τμήμα της τοπικής ομάδας **διαχειριστών** στον υπολογιστή που φιλοξενεί την εφαρμογή.  

* Για να δημιουργήσετε μια παρουσία μετρητή επιδόσεων και την ενημέρωση των μετρητών, ο χρήστης πρέπει να είναι μέλος της ομάδας το **διαχειριστές** ή **Χρήστες εποπτείας επιδόσεων** . 

## <a name="create-performance-category-and-counters"></a>Δημιουργία κατηγορίας επιδόσεων και μετρητών 

Για να δημιουργήσετε το μετρητών, καλέστε τη μέθοδο CreatePeformanceCategoryAndCounters της [ShardMapManagmentFactory τάξης](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx). Μόνο ο διαχειριστής μπορεί να εκτελέσει τη μέθοδο: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Μπορείτε επίσης να χρησιμοποιήσετε [αυτό](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) δέσμη ενεργειών του PowerShell για να εκτελέσετε τη μέθοδο. Η μέθοδος δημιουργεί τους παρακάτω μετρητές επιδόσεων:  

* **Προσωρινή αποθήκευση αντιστοιχίσεων**: αριθμός των αντιστοιχίσεων στο cache για το χάρτη shard.
*  **Λειτουργίες DDR/sec**: ρυθμό εξαρτώμενα δρομολόγησης λειτουργιών δεδομένων για το χάρτη shard. Αυτός ο μετρητής είναι ενημερωθεί όταν μια κλήση σε [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) αποτελέσματα σε μια επιτυχημένη σύνδεση με το shard προορισμού. 
*  **Αντιστοίχιση αναζήτησης cache επισκέψεων/δευτερόλεπτο**: ρυθμός των λειτουργιών αναζήτησης επιτυχής cache για αντιστοιχίσεις shard. 
*  **Αντιστοίχιση αναζήτησης cache αποτυχίες/sec**: ρυθμός των λειτουργιών αναζήτησης αποτυχίας cache για αντιστοιχίσεις shard.
*  **Αντιστοιχίσεις που προστέθηκε ή ενημερώθηκε στο cache/sec**: ρυθμός των αντιστοιχίσεων προστίθενται ή ενημερώθηκαν στο cache για το χάρτη shard. 
*  **Αντιστοιχίσεις καταργούνται από το cache/sec**: χρέωση, την οποία αντιστοιχίσεις καταργούνται από το cache για το χάρτη shard. 

Μετρητές επιδόσεων δημιουργούνται για κάθε αντιστοίχιση στο cache shard ανά διεργασία.  


## <a name="notes"></a>Σημειώσεις
Τα παρακάτω συμβάντα ενεργοποιούν τη δημιουργία των μετρητών επιδόσεων:  

* Η προετοιμασία του το [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) με [ανυπομονείτε τη φόρτωση](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), εάν η ShardMapManager περιέχει οποιαδήποτε shard χάρτες. Αυτά περιλαμβάνουν τα [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) και οι μέθοδοι [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) .
* Επιτυχής αναζήτησης χάρτη shard (χρησιμοποιώντας [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) ή [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Επιτυχής δημιουργία shard αντιστοίχιση χρησιμοποιώντας CreateShardMap().

Οι μετρητές επιδόσεων θα ενημερωθεί, όλες οι λειτουργίες cache που εκτελούνται στον χάρτη shard και αντιστοιχίσεις. Επιτυχής κατάργησης του χάρτη shard χρησιμοποιώντας reults () DeleteShardMap διαγραφή της παρουσίας μετρητών επιδόσεων.  

## <a name="best-practices"></a>Βέλτιστες πρακτικές

* Δημιουργία της κατηγορίας επιδόσεων και μετρητές πρέπει να εκτελεστούν μόνο μία φορά πριν από τη δημιουργία του αντικειμένου ShardMapManager. Κάθε εκτέλεση της εντολής CreatePerformanceCategoryAndCounters() καταργεί την προηγούμενη μετρητές (να χάσετε τα δεδομένα που αναφέρονται από όλες τις εμφανίσεις) και δημιουργεί νέα.  

* Παρουσίες μετρητή επιδόσεων δημιουργούνται ανά διεργασία. Τυχόν σφάλμα εφαρμογή ή κατάργηση του χάρτη shard από το cache θα προκαλέσει τη διαγραφή των παρουσιών μετρητές επιδόσεων.  

### <a name="see-also"></a>Δείτε επίσης

[Επισκόπηση δυνατοτήτων ελαστικότητας βάσης δεδομένων](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->


<properties 
    pageTitle="Ρύθμιση παραμέτρων ενεργού παν-αναπαραγωγής για βάση δεδομένων SQL Azure χρησιμοποιώντας PowerShell | Microsoft Azure" 
    description="Ρύθμιση παραμέτρων ενεργού παν-αναπαραγωγής για βάση δεδομένων SQL Azure με χρήση του PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Ρύθμιση παραμέτρων παν αναπαραγωγής για βάση δεδομένων SQL Azure με το PowerShell

> [AZURE.SELECTOR]
- [Επισκόπηση](sql-database-geo-replication-overview.md)
- [Πύλη του Azure](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Σε αυτό το άρθρο δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους του ενεργού παν-αναπαραγωγής για βάση δεδομένων SQL με το PowerShell.

Για να ξεκινήσετε ανακατεύθυνσης χρησιμοποιώντας το PowerShell, ανατρέξτε στο θέμα [Προετοιμασία προγραμματισμένες ή μη προγραμματισμένες ανακατεύθυνσης για βάση δεδομένων SQL Azure με το PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Ενεργό παν αναπαραγωγής (αναγνώσιμα secondaries) είναι τώρα διαθέσιμο για όλες τις βάσεις δεδομένων σε όλα τα επίπεδα εξυπηρέτησης. Στο 2017 Απριλίου θα απόσυρση ο δευτερεύων τύπος μη αναγνώσιμο και υπάρχουσες βάσεις δεδομένων δεν είναι αναγνώσιμο θα αναβαθμιστούν αυτόματα σε ευανάγνωστη secondaries.



Για τη ρύθμιση παραμέτρων ενεργού παν-αναπαραγωγής χρησιμοποιώντας το PowerShell, χρειάζεστε τα εξής:

- Μια συνδρομή του Azure. 
- Μια βάση δεδομένων Azure SQL - την κύρια βάση δεδομένων που θέλετε να αναπαραγάγετε.
- Azure PowerShell 1.0 ή νεότερη έκδοση. Μπορείτε να κάνετε λήψη και να εγκαταστήσετε τις λειτουργικές μονάδες Azure PowerShell, παρακάτω [πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Ρύθμιση παραμέτρων τα διαπιστευτήριά σας και επιλέξτε τη συνδρομή σας

Πρώτα πρέπει να δημιουργήσετε πρόσβαση στο λογαριασμό σας Azure επομένως, ξεκινήστε PowerShell και, στη συνέχεια, εκτελέστε το ακόλουθο cmdlet. Στην οθόνη σύνδεσης, πληκτρολογήστε το ίδιο μήνυμα ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.


    Login-AzureRmAccount

Αφού πραγματοποιήσετε είσοδο με επιτυχία στο θα δείτε ορισμένες πληροφορίες στην οθόνη που περιλαμβάνει το αναγνωριστικό που εισέλθετε με και τις συνδρομές Azure στις οποίες έχετε πρόσβαση.


### <a name="select-your-azure-subscription"></a>Επιλέξτε τη συνδρομή σας στο Azure

Για να επιλέξετε τη συνδρομή χρειάζεστε τη συνδρομή σας αναγνωριστικό. Μπορείτε να αντιγράψετε το αναγνωριστικό εγγραφής από τις πληροφορίες που εμφανίζονται από το προηγούμενο βήμα, ή εάν έχετε πολλές συνδρομές και χρειάζεστε περισσότερες λεπτομέρειες, μπορείτε να εκτελέσετε το cmdlet **Get-AzureRmSubscription** και να αντιγράψετε πληροφορίες για τη συνδρομή που θέλετε από το σύνολο αποτελεσμάτων. Το ακόλουθο cmdlet χρησιμοποιεί το αναγνωριστικό εγγραφής για να ορίσετε την τρέχουσα συνδρομή σας:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Μετά την επιτυχή εκτέλεση **Επιλέξτε AzureRmSubscription** που επιστρέφονται με το PowerShell μηνύματος.


## <a name="add-secondary-database"></a>Προσθήκη δευτερεύουσας βάσης δεδομένων


Τα παρακάτω βήματα Δημιουργήστε μια νέα δευτερεύουσα βάση δεδομένων σε μια συνεργασία παν αναπαραγωγής.  
  
Για να ενεργοποιήσετε ένα δευτερεύον πρέπει να είστε ο κάτοχος της συνδρομής ή ο κάτοχος από κοινού. 

Μπορείτε να χρησιμοποιήσετε το cmdlet **New-AzureRmSqlDatabaseSecondary** για να προσθέσετε μια δευτερεύουσα βάση δεδομένων σε ένα διακομιστή του συνεργάτη σε τοπική βάση δεδομένων στο διακομιστή στον οποίο είστε συνδεδεμένοι (την κύρια βάση δεδομένων). 

Αυτό το cmdlet αντικαθιστά **Έναρξη AzureSqlDatabaseCopy** με την παράμετρο **– IsContinuous** .  Αυτό θα εξόδου ενός αντικειμένου **AzureRmSqlDatabaseSecondary** που μπορεί να χρησιμοποιηθεί από άλλα cmdlet του για να προσδιορίσετε μια σύνδεση συγκεκριμένες αναπαραγωγής. Αυτό το cmdlet θα επιστρέψει όταν δημιουργείται και πλήρως τοποθετούνται στη δευτερεύουσα βάση δεδομένων. Ανάλογα με το μέγεθος της βάσης δεδομένων μπορεί να χρειαστούν από λεπτά σε ώρες.

Η βάση δεδομένων από αναπαραγωγή στο δευτερεύοντα διακομιστή θα έχει το ίδιο όνομα με τη βάση δεδομένων στο κύριο διακομιστή και θα, από προεπιλογή, έχουν το ίδιο επίπεδο υπηρεσίας. Δευτερεύουσα βάσης δεδομένων μπορεί να είναι αναγνώσιμο ή μη αναγνώσιμο και μπορεί να είναι μία βάση δεδομένων ή μια βάση δεδομένων της ελαστικότητας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) και [Επίπεδα υπηρεσίας](sql-database-service-tiers.md).
Μετά τη δευτερεύουσα δημιουργείται και τοποθετούνται, δεδομένων θα ξεκινήσει αναπαραγωγή από την κύρια βάση δεδομένων στη νέα δευτερεύουσα βάση δεδομένων. Τα παρακάτω βήματα περιγράφουν τον τρόπο για την ολοκλήρωση της εργασίας με χρήση του PowerShell για τη δημιουργία μη αναγνώσιμο και είναι ευανάγνωστο secondaries, είτε με μία βάση δεδομένων ή μια βάση δεδομένων της ελαστικότητας.

Εάν η βάση δεδομένων συνεργάτη υπάρχει ήδη (για παράδειγμα - ως αποτέλεσμα τον τερματισμό μια προηγούμενη σχέση παν αναπαραγωγής) η εντολή θα αποτύχει.



### <a name="add-a-non-readable-secondary-single-database"></a>Προσθήκη μιας δευτερεύουσας μη αναγνώσιμο (μία βάση δεδομένων)

Η παρακάτω εντολή δημιουργεί μη αναγνώσιμο δευτερεύον της βάσης δεδομένων "περίπτωση" του διακομιστή "srv2" στην ομάδα πόρων "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Προσθήκη δευτερεύουσας αναγνώσιμο (μία βάση δεδομένων)

Η παρακάτω εντολή δημιουργεί αναγνώσιμο δευτερεύον της βάσης δεδομένων "περίπτωση" του διακομιστή "srv2" στην ομάδα πόρων "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Προσθήκη μιας δευτερεύουσας μη αναγνώσιμο (ελαστικότητας βάση δεδομένων)

Η παρακάτω εντολή δημιουργεί μη αναγνώσιμο δευτερεύον της βάσης δεδομένων "περίπτωση" στο χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με το όνομα "ElasticPool1" του διακομιστή "srv2" στην ομάδα πόρων "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Προσθήκη αναγνώσιμο δευτερεύον (ελαστικότητας βάση δεδομένων)

Η παρακάτω εντολή δημιουργεί αναγνώσιμο δευτερεύον της βάσης δεδομένων "περίπτωση" στο χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με το όνομα "ElasticPool1" του διακομιστή "srv2" στην ομάδα πόρων "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Κατάργηση δευτερεύοντα βάσης δεδομένων

Χρησιμοποιήστε το cmdlet **AzureRmSqlDatabaseSecondary κατάργηση** για να τερματίσετε οριστικά τη συνεργασία αλληλεπίδραση μεταξύ μιας δευτερεύουσας βάσης δεδομένων και τα κύρια. Μετά τη λύση τη σχέση, τη βάση δεδομένων δευτερεύοντα μετατρέπεται σε μια βάση δεδομένων ανάγνωση και εγγραφή. Εάν τη συνδεσιμότητα με δευτερεύοντα βάση δεδομένων είναι κατεστραμμένη η εντολή επιτύχει, αλλά το δευτερεύον θα μετατραπεί σε ανάγνωση και εγγραφή μετά την επαναφορά συνδεσιμότητας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Κατάργηση AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) και [Επίπεδα υπηρεσίας](sql-database-service-tiers.md).

Αυτό το cmdlet αντικαθιστά διακοπή AzureSqlDatabaseCopy για την αναπαραγωγή. 

Η κατάργηση αυτό είναι ισοδύναμο με μια εξαναγκασμένη τερματισμού που καταργεί τη σύνδεση αναπαραγωγής και αφήνει το δευτερεύον πρώην ως μεμονωμένη βάση δεδομένων που δεν είναι πλήρως αναπαραγάγει πριν από την τερματισμού. Όλα τα δεδομένα σύνδεσης θα η εκκαθάριση στην Πρώην κύρια και δευτερεύουσα, πρώην εάν ή όταν είναι διαθέσιμες. Αυτό το cmdlet θα επιστρέψει κατά τη σύνδεση αναπαραγωγή έχει καταργηθεί. 


Προκειμένου να καταργήσετε δευτερεύοντα, οι χρήστες θα πρέπει να έχετε πρόσβαση εγγραφής κύριων και δευτερευουσών βάσεις δεδομένων σύμφωνα με RBAC. Ανατρέξτε στο θέμα Έλεγχος πρόσβασης βάσει ρόλων για λεπτομέρειες.

Το παρακάτω καταργεί σύνδεσης αναπαραγωγής της βάσης δεδομένων με το όνομα "περίπτωση" διακομιστής "srv2" ομάδα πόρων "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Ρύθμιση παραμέτρων αναπαραγωγής παν οθόνη και εύρυθμης λειτουργίας

Παρακολούθηση εργασίες περιλαμβάνουν παρακολούθηση της ρύθμισης παραμέτρων παν αναπαραγωγής και παρακολούθηση εύρυθμης λειτουργίας αναπαραγωγής δεδομένων.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) μπορεί να χρησιμοποιηθεί για την ανάκτηση των πληροφοριών σχετικά με τις συνδέσεις προς τα εμπρός αναπαραγωγής ορατό στην προβολή στον κατάλογο sys.geo_replication_links.

Η ακόλουθη εντολή ανακτά την κατάσταση της σύνδεσης αλληλεπίδραση μεταξύ την κύρια βάση δεδομένων "περίπτωση" και τη δευτερεύουσα στο διακομιστή "srv2" ομάδα πόρων "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με το ενεργό παν-αναπαραγωγής, ανατρέξτε στο θέμα - [Active παν-αναπαραγωγής](sql-database-geo-replication-overview.md)
- Για μια επισκόπηση επιχειρηματικές συνέχειας και σενάρια, ανατρέξτε στο θέμα [Επισκόπηση επιχειρηματικές συνέχειας](sql-database-business-continuity.md)


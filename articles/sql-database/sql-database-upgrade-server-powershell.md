<properties
    pageTitle="Αναβάθμιση σε V12 βάση δεδομένων SQL Azure με χρήση του PowerShell | Microsoft Azure"
    description="Εξηγεί πώς μπορείτε να κάνετε αναβάθμιση σε V12 βάση δεδομένων SQL Azure συμπεριλαμβανομένων πώς να αναβαθμίσετε τις βάσεις δεδομένων Web και επιχειρήσεις και πώς να κάνετε αναβάθμιση σε διακομιστή V11: μετεγκατάσταση τις βάσεις δεδομένων απευθείας σε ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων με χρήση του PowerShell."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="sstein"/>

# <a name="upgrade-to-azure-sql-database-v12-using-powershell"></a>Αναβάθμιση σε V12 βάση δεδομένων SQL Azure με χρήση του PowerShell


> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


V12 βάση δεδομένων SQL είναι την πιο πρόσφατη έκδοση, επομένως συνιστάται να την αναβάθμιση σε V12 βάση δεδομένων SQL.
V12 βάση δεδομένων SQL περιλαμβάνει πολλά [πλεονεκτήματα σε σχέση με την προηγούμενη έκδοση](sql-database-v12-whats-new.md) όπως:

- Αυξημένη συμβατότητα με τον SQL Server.
- Βελτιωμένη premium επιδόσεις και νέα επίπεδα επιδόσεων.
- [Τα σύνολα ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md).

Σε αυτό το άρθρο παρέχει οδηγίες για την αναβάθμιση υπάρχοντες διακομιστές V11: βάση δεδομένων SQL και των βάσεων δεδομένων σε V12 βάση δεδομένων SQL.

Κατά τη διαδικασία αναβάθμισης σε V12, αναβάθμιση βάσεων δεδομένων Web και επιχειρήσεις σε ένα νέο επίπεδο υπηρεσιών, ώστε να περιλαμβάνονται οδηγίες για την αναβάθμιση των βάσεων δεδομένων Web και επιχειρήσεις.

Επιπλέον, μετεγκατάσταση σε ένα [χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md) μπορεί να είναι πιο οικονομική από την αναβάθμιση σε μεμονωμένες επιδόσεις (Τιμολόγηση βαθμίδες) για μία μόνο βάσεις δεδομένων. Τα σύνολα επίσης απλοποίηση της διαχείρισης βάσης δεδομένων επειδή χρειάζεται να διαχειριστείτε τις ρυθμίσεις επιδόσεων για το χώρο συγκέντρωσης και όχι ξεχωριστά τη Διαχείριση τα επίπεδα επιδόσεων μεμονωμένες βάσεις δεδομένων. Εάν έχετε βάσεις δεδομένων σε πολλούς διακομιστές, μπορείτε να τα μετακινήσετε στον ίδιο διακομιστή και λήψη πλεονέκτημα της τοποθέτησης τους σε ένα χώρο συγκέντρωσης.

Μπορείτε να ακολουθήσετε τα βήματα σε αυτό το άρθρο για να μετεγκαταστήσετε εύκολα βάσεις δεδομένων από διακομιστές V11: απευθείας σε χώρους συγκέντρωσης ελαστικότητας βάση δεδομένων.

Σημειώστε ότι τις βάσεις δεδομένων θα παραμείνει σε κατάσταση σύνδεσης και να συνεχίσετε να εργάζεστε σε ολόκληρη η λειτουργία αναβάθμισης. Κατά τη διάρκεια του πραγματικού Μετάβαση στο νέο επίπεδο επιδόσεων προσωρινό απόθεση των συνδέσεων στη βάση δεδομένων μπορεί να συμβεί για πολύ μικρό χρονικό διάστημα που είναι συνήθως περίπου 90 δευτερόλεπτα, αλλά μπορεί να είναι έως και 5 λεπτά. Εάν η εφαρμογή σας έχει [μεταβατικές σφαλμάτων χειρισμό για απολήξεις σύνδεσης](sql-database-connectivity-issues.md) , στη συνέχεια, αρκεί να προστατευτώ από απορρίφθηκαν συνδέσεις στο τέλος της αναβάθμισης.

Αναβάθμιση σε V12 βάση δεδομένων SQL δεν είναι δυνατή η αναίρεση. Μετά την αναβάθμιση στο διακομιστή δεν είναι δυνατό να γίνει επαναφορά V11:.

Μετά την αναβάθμιση σε V12, [συστάσεις επίπεδο υπηρεσίας](sql-database-service-tier-advisor.md) και [συστάσεις ελαστικότητας χώρου συγκέντρωσης](sql-database-elastic-pool-create-portal.md) δεν θα αμέσως είναι διαθέσιμες μέχρι την υπηρεσία έχει το χρόνο να αξιολογήσετε τον φόρτο εργασίας στο νέο διακομιστή. V11: διακομιστή σύσταση ιστορικού δεν ισχύει για τους διακομιστές V12, ώστε να μην διατηρούνται.  

## <a name="prepare-to-upgrade"></a>Προετοιμασία για αναβάθμιση

- **Αναβάθμιση όλες τις βάσεις δεδομένων Web και επιχειρήσεις**: Χρησιμοποιήστε την πύλη ή χρησιμοποιήστε το [PowerShell για να αναβαθμίσετε βάσεων δεδομένων και διακομιστή](sql-database-upgrade-server-powershell.md).
- **Αναθεώρηση και αναστολή παν αναπαραγωγής**: Εάν τη βάση δεδομένων Azure SQL έχει ρυθμιστεί για την αναπαραγωγή παν θα πρέπει να κάνετε ενός εγγράφου την τρέχουσα ρύθμιση παραμέτρων και [Διακοπή παν αναπαραγωγής](sql-database-geo-replication-portal.md#remove-secondary-database). Μόλις ολοκληρωθεί η αναβάθμιση Ρυθμίστε ξανά τη βάση δεδομένων για την αναπαραγωγή παν.
- **Ανοίξτε αυτές τις θύρες, εάν έχετε προγράμματα-πελάτες του σε μια Εικονική Azure**: Εάν το πρόγραμμα-πελάτης συνδέεται με V12 βάση δεδομένων SQL, ενώ εκτελείται το πρόγραμμα-πελάτη σε μια εικονική μηχανή Azure (Εικονική), πρέπει να ανοίξετε περιοχές θύρας 11000 11999 και 14000-14999 σε η Εικονική. Για λεπτομέρειες, ανατρέξτε στο θέμα [θύρες για V12 βάσης δεδομένων SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να αναβαθμίσετε ένα διακομιστή σε V12 με το PowerShell, πρέπει να έχετε την πιο πρόσφατη Azure PowerShell εγκατασταθεί και εκτελείται. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Ρύθμιση παραμέτρων τα διαπιστευτήριά σας και επιλέξτε τη συνδρομή σας

Για να εκτελέσετε τα cmdlet του PowerShell σε σχέση με τη συνδρομή σας στο Azure πρέπει πρώτα να δημιουργήσετε πρόσβαση στο λογαριασμό σας Azure. Εκτελέστε τις εξής και, θα εμφανιστεί μια οθόνη εισόδου, για να εισαγάγετε τα διαπιστευτήριά σας. Χρησιμοποιήστε το ίδιο μήνυμα ηλεκτρονικού ταχυδρομείου και τον κωδικό πρόσβασης που χρησιμοποιείτε για να συνδεθείτε πύλη του Azure.

    Add-AzureRmAccount

Μετά την είσοδο με επιτυχία, θα πρέπει να βλέπετε ορισμένες πληροφορίες στην οθόνη που περιλαμβάνει το αναγνωριστικό που εισέλθετε με και τις συνδρομές Azure στις οποίες έχετε πρόσβαση.

Για να επιλέξετε τη συνδρομή που θέλετε να εργαστούν μαζί σας πρέπει το αναγνωριστικό (**-SubscriptionId**) εγγραφής ή τη συνδρομή όνομα (**-SubscriptionName**). Μπορείτε να το αντιγράψετε από το προηγούμενο βήμα, ή εάν έχετε πολλές συνδρομές, μπορείτε να εκτελέσετε το cmdlet **Get-AzureRmSubscription** και αντιγράψτε τις πληροφορίες για τη συνδρομή που θέλετε από το σύνολο αποτελεσμάτων.

Εκτελέστε το ακόλουθο cmdlet με τις πληροφορίες τη συνδρομή σας για να ορίσετε την τρέχουσα συνδρομή σας:

    Set-AzureRmContext -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Οι ακόλουθες εντολές θα εκτελείται σε σχέση με τη συνδρομή που μόλις επιλέξατε παραπάνω.

## <a name="get-recommendations"></a>Λήψη συστάσεων

Για να λάβετε την πρόταση για την αναβάθμιση του διακομιστή εκτελέστε το ακόλουθο cmdlet:

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1”

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-create-portal.md) και [βάση δεδομένων SQL Azure τις πληροφορίες τιμολόγησης συστάσεις επίπεδο](sql-database-service-tier-advisor.md).



## <a name="start-the-upgrade"></a>Έναρξη της αναβάθμισης

Για να ξεκινήσετε την αναβάθμιση του διακομιστή εκτελέστε το ακόλουθο cmdlet:

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


Κατά την εκτέλεση αυτής της διαδικασίας αναβάθμισης εντολή θα ξεκινήσει. Μπορείτε να προσαρμόσετε το αποτέλεσμα της σύστασης και να παρέχετε το επεξεργασμένο προτάσεων για αυτό το cmdlet.


## <a name="upgrade-a-server"></a>Αναβάθμιση σε διακομιστή


    # Adding the account
    #
    Add-AzureRmAccount

    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION'
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP'
    $ServerName = 'YOUR_SERVER'

    # Selecting the right subscription
    #
    Set-AzureRmContext -SubscriptionName $SubscriptionName

    # Getting the upgrade recommendations
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName

    # Starting the upgrade process
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## <a name="custom-upgrade-mapping"></a>Προσαρμοσμένη αναβάθμιση αντιστοίχισης

Εάν τις συστάσεις δεν είναι κατάλληλη για το διακομιστή και επιχειρήσεις πεζών-κεφαλαίων, στη συνέχεια, μπορείτε να επιλέξετε πώς τις βάσεις δεδομένων αναβαθμίζονται και να τις αντιστοιχίσει είτε μία είτε ελαστικότητας βάσεις δεδομένων.

Παράμετροι ElasticPoolCollection και DatabaseCollection είναι προαιρετικά:

    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties
    $elasticPool.DatabaseDtuMax = 100
    $elasticPool.DatabaseDtuMin = 0
    $elasticPool.Dtu = 800
    $elasticPool.Edition = "Standard"
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”)
    $elasticPool.Name = "elasticpool_1"
    $elasticPool.StorageMb = 800

    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap1.Name = "DBMain1"
    $databaseMap1.TargetEdition = "Standard"
    $databaseMap1.TargetServiceLevelObjective = "S0"

    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties
    $databaseMap2.Name = "DBMain2"
    $databaseMap2.TargetEdition = "Standard"
    $databaseMap2.TargetServiceLevelObjective = "S2"

    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -ServerVersion 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool)



## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Παρακολούθηση βάσεις δεδομένων μετά την αναβάθμιση σε V12 βάση δεδομένων SQL


Μετά την αναβάθμιση, συνιστάται να παρακολουθείτε τη βάση δεδομένων ενεργά για να βεβαιωθείτε ότι εφαρμογές εκτελούνται κατά τις επιθυμητές επιδόσεις και βελτιστοποίηση της χρήσης σύμφωνα με τις ανάγκες.

Επιπλέον, για την παρακολούθηση μεμονωμένων βάσεων δεδομένων, μπορείτε να παρακολουθείτε ελαστικότητας βάσης δεδομένων χώρους συγκέντρωσης [με την πύλη](sql-database-elastic-pool-manage-portal.md) ή με το [PowerShell](sql-database-elastic-pool-manage-powershell.md)


**Δεδομένα κατανάλωση πόρων:** Για τις βάσεις δεδομένων Basic, τυπική και Premium δεδομένα κατανάλωση πόρων διατίθεται μέσω του [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV στη βάση δεδομένων του χρήστη. Αυτό DMV παρέχει κοντά πληροφορίες κατανάλωση πόρων σε πραγματικό χρόνο στο 15 δεύτερη υποδιαίρεση για την προηγούμενη ώρα της λειτουργίας. Την κατανάλωση ποσοστό DTU για ένα χρονικό διάστημα υπολογίζεται ως την κατανάλωση μέγιστο ποσοστό της CPU, εισόδου/ΕΞΌΔΟΥ και καταγραφής διάστασης. Ακολουθεί ένα ερώτημα για να υπολογίσετε το μέσο κατανάλωση ποσοστό DTU πάνω από την τελευταία ώρα:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Πρόσθετες πληροφορίες παρακολούθησης:

- [Βάση δεδομένων SQL azure επιδόσεων καθοδήγηση για μία μόνο βάσεις δεδομένων](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Ζητήματα τιμής και απόδοσης για ένα χώρο συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-guidance.md).
- [Παρακολούθηση βάση δεδομένων SQL Azure χρησιμοποιώντας δυναμική Διαχείριση προβολών](sql-database-monitoring-with-dmvs.md)



**Ειδοποιήσεις:** Ρύθμιση 'Ειδοποιήσεων' στην πύλη του Azure για να σας ειδοποιεί όταν την κατανάλωση DTU για βάση δεδομένων αναβαθμισμένη προσεγγίσεις ορισμένες υψηλού επιπέδου. Ειδοποιήσεις βάσης δεδομένων μπορεί να ρυθμιστεί στην πύλη του Azure για διάφορες μετρικών απόδοσης όπως DTU CPU, εισόδου/ΕΞΌΔΟΥ και καταγραφής. Μεταβείτε στη βάση δεδομένων σας και επιλέξτε **ειδοποίησης κανόνων** σε το blade **Ρυθμίσεις** .

Για παράδειγμα, μπορείτε να ρυθμίσετε μια ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου σε "Ποσοστό DTU" Εάν η τιμή του μέσου ποσοστού DTU υπερβαίνει 75% πάνω από την τελευταία 5 λεπτά. Ανατρέξτε να [λαμβάνετε ειδοποιήσεις](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) για να μάθετε περισσότερα σχετικά με τη ρύθμιση ειδοποιήσεων υπενθύμισης.



## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool-create-portal.md) και να προσθέσετε ορισμένες ή όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης.
- Για να [αλλάξετε το επίπεδο επίπεδο και επιδόσεων υπηρεσίας της βάσης δεδομένων σας](sql-database-scale-up.md).



## <a name="related-information"></a>Σχετικές πληροφορίες

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Έναρξη-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Διακοπή AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)

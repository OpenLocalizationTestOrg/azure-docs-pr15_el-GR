<properties 
    pageTitle="Διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων (PowerShell) | Microsoft Azure" 
    description="Μάθετε τον τρόπο χρήσης του PowerShell για τη διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Παρακολούθηση και διαχείριση ενός χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων με το PowerShell 

> [AZURE.SELECTOR]
- [Πύλη του Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Διαχείριση ενός [χώρου συγκέντρωσης ελαστικότητας βάσης δεδομένων](sql-database-elastic-pool.md) με χρήση των cmdlet του PowerShell. 

Για συνήθεις τους κωδικούς σφάλματος, ανατρέξτε στο θέμα [τους κωδικούς σφάλματος SQL για εφαρμογές προγράμματος-πελάτη βάσης δεδομένων SQL: σφάλμα σύνδεσης και άλλα θέματα της βάσης δεδομένων](sql-database-develop-error-messages.md).

Μπορείτε να βρείτε τιμές για χώρους συγκέντρωσης σε [όρια eDTU και χώρου αποθήκευσης](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

* Azure PowerShell 1.0 ή νεότερη έκδοση. Για λεπτομερείς πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους Azure PowerShell](../powershell-install-configure.md).
* Τα σύνολα ελαστικότητας βάσης δεδομένων είναι διαθέσιμες με τους διακομιστές V12 βάσης δεδομένων SQL μόνο. Εάν έχετε μια V11: βάση δεδομένων SQL server, [χρήση του PowerShell για αναβάθμιση σε V12 και τη δημιουργία ενός χώρου συγκέντρωσης](sql-database-upgrade-server-portal.md) σε ένα βήμα.


## <a name="move-a-database-into-an-elastic-pool"></a>Μετακίνηση μιας βάσης δεδομένων σε ένα χώρο συγκέντρωσης ελαστικότητας

Μπορείτε να μετακινήσετε μια βάση δεδομένων μέσα ή έξω από ένα σύνολο με το [Σύνολο AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx). 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Αλλαγή των ρυθμίσεων απόδοσης του χώρου συγκέντρωσης

Όταν οι επιδόσεις είναι χαμηλές, μπορείτε να αλλάξετε τις ρυθμίσεις του χώρου συγκέντρωσης για να χωρέσει γεωμετρική πρόοδος. Χρησιμοποιήστε το cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Ορίστε την παράμετρο - Dtu το eDTUs ανά χώρου συγκέντρωσης. Ανατρέξτε στο θέμα [όρια eDTU και χώρου αποθήκευσης](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) για τις πιθανές τιμές.  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Λάβετε την κατάσταση του χώρου συγκέντρωσης λειτουργίες

Δημιουργία ενός χώρου συγκέντρωσης μπορεί να διαρκέσει χρόνο. Για να παρακολουθείτε την κατάσταση του χώρου συγκέντρωσης λειτουργίες, συμπεριλαμβανομένης της δημιουργίας και ενημερώσεις, χρησιμοποιήστε το cmdlet [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) .

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Λάβετε την κατάσταση να μεταφέρετε μια βάση δεδομένων της ελαστικότητας προς και από ένα χώρο συγκέντρωσης

Μετακίνηση μιας βάσης δεδομένων μπορεί να διαρκέσει ώρα. Παρακολουθήστε μια κατάσταση μετακίνησης χρησιμοποιώντας το cmdlet [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) .

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>Λήψη δεδομένων χρήσης πόρων για ένα χώρο συγκέντρωσης

Μετρικά που μπορεί να ανακτηθεί ως ποσοστό του ορίου χώρου συγκέντρωσης πόρων:   


| Μετρικό όνομα | Περιγραφή |
| :-- | :-- |
| CPU\_τοις εκατό | Μέσος όρος υπολογίσετε χρήσης σε ποσοστό του ορίου του χώρου συγκέντρωσης. |
| φυσική\_δεδομένων\_ανάγνωση\_τοις εκατό | Μέσος όρος χρήσης εισόδου/εξόδου στο ποσοστού με βάση το όριο του χώρου συγκέντρωσης. |
| αρχείο καταγραφής\_γράψετε\_τοις εκατό | Μέσος όρος σύνταξη χρήσης πόρων σε ποσοστό του ορίου του χώρου συγκέντρωσης. | 
| DTU\_κατανάλωση\_τοις εκατό | Μέσος όρος eDTU χρήσης σε ποσοστό του ορίου eDTU για το χώρο συγκέντρωσης | 
| χώρος αποθήκευσης\_τοις εκατό | Μέσο αξιοποίηση χώρου αποθήκευσης σε ποσοστό του ορίου χώρου αποθήκευσης του χώρου συγκέντρωσης. |  
| όσοι εργάζονται με\_τοις εκατό | Μέγιστο ταυτόχρονες εργαζομένων (αιτήσεις) σε ποσοστό με βάση το όριο του χώρου συγκέντρωσης. |  
| περίοδοι λειτουργίας\_τοις εκατό | Μέγιστο ταυτόχρονες περιόδους λειτουργίας στο ποσοστού με βάση το όριο του χώρου συγκέντρωσης. | 
| eDTU_limit | Τρέχουσα max ελαστικότητας χώρου συγκέντρωσης DTU ρύθμιση για αυτόν το χώρο συγκέντρωσης ελαστικότητας στη διάρκεια αυτού του διαστήματος. |
| χώρος αποθήκευσης\_όριο | Τρέχον όριο χώρου αποθήκευσης max ελαστικότητας χώρου συγκέντρωσης τη ρύθμιση για αυτόν το χώρο συγκέντρωσης ελαστικότητας σε megabyte στη διάρκεια αυτού του διαστήματος. |
| eDTU\_χρησιμοποιούνται | Μέσος όρος eDTUs που χρησιμοποιείται από το χώρο συγκέντρωσης σε αυτό το χρονικό διάστημα. |
| χώρος αποθήκευσης\_χρησιμοποιούνται | Μέσος όρος χώρος αποθήκευσης που χρησιμοποιείται από το χώρο συγκέντρωσης σε αυτό το χρονικό διάστημα σε byte |

**Μετρικά υποδιαίρεση/διατήρησης περιόδων:**

* Θα επιστραφούν δεδομένα στο υποδιαίρεση 5 λεπτά.  
* Διατήρηση δεδομένων είναι 35 ημέρες.  

Σε αυτό το cmdlet και API περιορίζει τον αριθμό των γραμμών που μπορούν να ανακτηθούν σε μια κλήση 1000 σε γραμμές (περίπου 3 ημέρες των δεδομένων σε υποδιαίρεση 5 λεπτά). Αλλά αυτή την εντολή μπορεί να ονομάζεται πολλές φορές με διαφορετικές Έναρξη/Λήξη χρονικά διαστήματα για την ανάκτηση περισσότερων δεδομένων 

Για να ανακτήσετε τα μετρικά:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Λήψη δεδομένων χρήσης πόρων για μια βάση δεδομένων της ελαστικότητας

Αυτά τα API είναι ίδια με την τρέχουσα API (V12) που χρησιμοποιούνται για την παρακολούθηση της χρήσης πόρων μιας βάσης δεδομένων μεμονωμένη, εκτός από τα εξής σημασιολογίας διαφορά. 

Για αυτό το API, εκφράζονται μετρικά ανάκτηση ως ποσοστό του το ανά max eDTUs (ή ισοδύναμο καπέλο για το υποκείμενο μετρικό όπως CPU, κλπ εισόδου/ΕΞΌΔΟΥ) για αυτόν το χώρο συγκέντρωσης. Για παράδειγμα, 50% χρήση από οποιαδήποτε από αυτές τις μετρήσεις υποδεικνύει ότι η κατανάλωση συγκεκριμένο πόρο είναι στο 50% των το ανά όριο καπέλο βάσης δεδομένων για το συγκεκριμένο πόρο στο χώρο συγκέντρωσης γονικό στοιχείο. 

Για να ανακτήσετε τα μετρικά:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Προσθήκη μια ειδοποίηση σε ένα χώρο συγκέντρωσης πόρων

Μπορείτε να προσθέσετε κανόνες ειδοποίησης με πόρους για την αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου ή ειδοποίησης συμβολοσειρές για [τα τελικά σημεία διεύθυνση URL](https://msdn.microsoft.com/library/mt718036.aspx) όταν ο πόρος επισκέψεις ενός ορίου χρήσης που ορίζετε. Χρησιμοποιήστε το cmdlet Προσθήκη AzureRmMetricAlertRule. 

> [AZURE.IMPORTANT]Παρακολούθηση για χώρους συγκέντρωσης ελαστικότητας χρήσης πόρων έχει μια καθυστέρηση τουλάχιστον 20 λεπτών. Ρύθμιση ειδοποιήσεων μικρότερη από 30 λεπτά για χώρους συγκέντρωσης ελαστικά δεν υποστηρίζεται αυτήν τη στιγμή. Ορίστε τις προειδοποιήσεις για ελαστικά χώρους συγκέντρωσης με τελεία (παράμετρο που ονομάζεται "-WindowSize" στο API του PowerShell) μικρότερη από 30 λεπτά ενδέχεται να μην είναι ενεργοποίησε. Βεβαιωθείτε ότι τις ειδοποιήσεις που ορίζετε για χώρους συγκέντρωσης ελαστικότητας Χρησιμοποιήστε μια περίοδο (Μέγεθος_παραθύρου) 30 λεπτά ή περισσότερο.

Αυτό το παράδειγμα προσθέτει μια ειδοποίηση για λήψη ειδοποίησης κατά την κατανάλωση eDTU χώρου συγκέντρωσης μεταβαίνει επάνω από το συγκεκριμένο όριο.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Προσθήκη ειδοποιήσεις για όλες τις βάσεις δεδομένων σε ένα χώρο συγκέντρωσης

Μπορείτε να προσθέσετε κανόνες ειδοποίησης όλα βάση δεδομένων σε ένα σύνολο ελαστικότητας για την αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου ή ειδοποίησης συμβολοσειρών σε [διεύθυνση URL τελικά σημεία](https://msdn.microsoft.com/library/mt718036.aspx) όταν ένας πόρος επισκέψεις ενός ορίου χρήσης ρύθμιση με την ειδοποίηση. 

> [AZURE.IMPORTANT] Παρακολούθηση για χώρους συγκέντρωσης ελαστικότητας χρήσης πόρων έχει μια καθυστέρηση τουλάχιστον 20 λεπτών. Ρύθμιση ειδοποιήσεων μικρότερη από 30 λεπτά για χώρους συγκέντρωσης ελαστικά δεν υποστηρίζεται αυτήν τη στιγμή. Ορίστε τις προειδοποιήσεις για ελαστικά χώρους συγκέντρωσης με τελεία (παράμετρο που ονομάζεται "-WindowSize" στο API του PowerShell) μικρότερη από 30 λεπτά ενδέχεται να μην είναι ενεργοποίησε. Βεβαιωθείτε ότι τις ειδοποιήσεις που ορίζετε για χώρους συγκέντρωσης ελαστικότητας Χρησιμοποιήστε μια περίοδο (Μέγεθος_παραθύρου) 30 λεπτά ή περισσότερο.

Αυτό το παράδειγμα προσθέτει μια ειδοποίηση σε κάθε μία από τις βάσεις δεδομένων σε ένα χώρο συγκέντρωσης για λήψη ειδοποίησης κατά την κατανάλωση DTU της βάσης δεδομένων που μεταβαίνει επάνω από το συγκεκριμένο όριο.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Συλλογή και παρακολουθήστε τα δεδομένα χρήσης πόρων σε πολλούς χώρους συγκέντρωσης σε μια συνδρομή

Όταν έχετε μεγάλο αριθμό βάσεις δεδομένων σε μια συνδρομή, είναι εύκολα για την παρακολούθηση της κάθε ελαστικότητας χώρου συγκέντρωσης ξεχωριστά. Αντί για αυτό, των cmdlet του PowerShell βάσης δεδομένων SQL και ερωτήματα T SQL μπορεί να συνδυαστεί για τη συλλογή δεδομένων χρήσης πόρων από πολλούς χώρους συγκέντρωσης και τις βάσεις δεδομένων για την παρακολούθηση και ανάλυση των "Χρήση πόρων". Μπορείτε να βρείτε μια [εφαρμογή δείγμα](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) όπως ενός συνόλου δεσμών ενεργειών του powershell στο αποθετήριο δείγματα GitHub SQL Server μαζί με την τεκμηρίωση σε τι κάνει και πώς να το χρησιμοποιήσετε.

Για να χρησιμοποιήσετε αυτήν την υλοποίηση δείγμα ακολουθήστε αυτά τα βήματα που αναφέρονται παρακάτω.


1. Λήψη [δεσμών ενεργειών και την τεκμηρίωση](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Τροποποίηση των δεσμών ενεργειών για το περιβάλλον σας. Καθορίστε έναν ή περισσότερους διακομιστές που φιλοξενούνται ελαστικότητας χώρους συγκέντρωσης.
3. Καθορίστε μια βάση δεδομένων τηλεμετρίας πού βρίσκονται τα που έχουν συλλεχθεί μετρικά να αποθηκευτεί. 
4. Προσαρμόστε τη δέσμη ενεργειών για να καθορίσετε τη διάρκεια της εκτέλεσης των δεσμών ενεργειών.

Σε υψηλό επίπεδο, οι δέσμες ενεργειών κάντε τα εξής:

*   Απαριθμεί όλους τους διακομιστές σε μια δεδομένη Azure συνδρομή (ή μια καθορισμένη λίστα διακομιστών).
*   Εκτελεί μια εργασία στο παρασκήνιο για κάθε διακομιστή. Η εργασία εκτελείται σε επανάληψη σε τακτά χρονικά διαστήματα και συλλέγει δεδομένα τηλεμετρίας για όλα τα σύνολα στο διακομιστή. Στη συνέχεια, φορτώνει τα δεδομένα που έχουν συλλεχθεί στη βάση δεδομένων του καθορισμένου τηλεμετρίας.
*   Απαριθμεί μια λίστα των βάσεων δεδομένων σε κάθε σύνολο για τη συλλογή δεδομένων χρήσης πόρων βάσης δεδομένων. Στη συνέχεια, φορτώνει τα δεδομένα που έχουν συλλεχθεί στη βάση δεδομένων τηλεμετρίας.

Μπορείτε να αναλύσετε τα μετρικά που έχουν συλλεχθεί στη βάση δεδομένων τηλεμετρίας για την παρακολούθηση της εύρυθμης λειτουργίας του ελαστικότητας χώρους συγκέντρωσης και των βάσεων δεδομένων σε αυτό. Η δέσμη ενεργειών εγκαθιστά επίσης μια προκαθορισμένη συνάρτηση πίνακα τιμή (TVF) στη βάση δεδομένων τηλεμετρίας για να σας βοηθήσει συγκεντρωτική τιμή τις μετρήσεις για ένα καθορισμένο χρονικό διάστημα παράθυρο. Για παράδειγμα, αποτελέσματα το TVF μπορεί να χρησιμοποιηθεί για να εμφανίσετε "επάνω Ν ελαστικότητας χώρους συγκέντρωσης με τη χρήση μέγιστο eDTU σε ένα παράθυρο δεδομένη χρονική." Προαιρετικά, χρησιμοποιήστε αναλυτικό εργαλεία όπως το Excel ή το Power BI στο ερώτημα και να αναλύσετε τα δεδομένα που έχουν συλλεχθεί.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Παράδειγμα: ανάκτηση μετρικά κατανάλωση πόρων για τις βάσεις δεδομένων και ένα χώρο συγκέντρωσης

Αυτό το παράδειγμα ανακτά την κατανάλωση μετρήσεις για ένα δεδομένο ελαστικότητας χώρο συγκέντρωσης και όλες τις βάσεις δεδομένων του. Δεδομένα που έχουν συλλεχθεί είναι μορφοποιημένο και γραμμένο σε ένα μορφοποιημένο αρχείο .csv. Το αρχείο είναι δυνατή η αναζήτηση με το Excel.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Λανθάνων χρόνος λειτουργιών ελαστικότητας χώρου συγκέντρωσης

- Αλλάζοντας το ελάχιστο eDTUs ανά βάση δεδομένων ή max eDTUs ανά βάση δεδομένων συνήθως ολοκληρώνει σε 5 λεπτά ή λιγότερο.
- Αλλαγή του eDTUs ανά χώρου συγκέντρωσης εξαρτάται από το συνολικό ποσό του χώρου που χρησιμοποιείται από όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης. Αλλαγές μέσος όρος 90 λεπτά ή λιγότερο ανά 100 GB. Για παράδειγμα, εάν ο συνολικός χώρος που χρησιμοποιούνται από όλες τις βάσεις δεδομένων στο χώρο συγκέντρωσης είναι 200 GB, τότε τα αναμενόμενα λανθάνων χρόνος για την αλλαγή του χώρου συγκέντρωσης eDTU ανά χώρου συγκέντρωσης είναι 3 ώρες ή λιγότερο.

## <a name="migrate-from-v11-to-v12-servers"></a>Μετεγκατάσταση από V11: στους διακομιστές V12

Cmdlet του PowerShell είναι διαθέσιμες για να ξεκινήσετε, να διακόψετε ή παρακολούθηση της αναβάθμισης σε V12 βάση δεδομένων SQL Azure από V11: ή οποιαδήποτε άλλη έκδοση προ-V12.

- [Αναβάθμιση σε SQL V12 βάσης δεδομένων με χρήση του PowerShell](sql-database-upgrade-server-powershell.md)

Για την τεκμηρίωση αναφοράς σχετικά με τα παρακάτω cmdlet του PowerShell, ανατρέξτε στα θέματα:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Έναρξη-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Διακοπή AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Το cmdlet διακοπή σημαίνει ακύρωση, δεν παύση. Δεν υπάρχει τρόπος για να συνεχίσετε την αναβάθμιση, εκτός από το ξεκινήσετε ξανά από την αρχή. Το cmdlet διακοπή εκκαθαρίζει και όλους τους πόρους κατάλληλο για ενημερωμένες εκδόσεις.

## <a name="next-steps"></a>Επόμενα βήματα

- [Δημιουργία ελαστικότητας εργασίες](sql-database-elastic-jobs-overview.md) Ελαστικά εργασίες σάς επιτρέπουν να εκτελεστεί δέσμες ενεργειών T SQL σε οποιονδήποτε αριθμό βάσεων δεδομένων στο χώρο συγκέντρωσης.
- Ανατρέξτε στο θέμα [Κλιμάκωση ανάληψη με βάση δεδομένων SQL Azure](sql-database-elastic-scale-introduction.md): Χρησιμοποιήστε εργαλεία ελαστικότητας βάσης δεδομένων για να κλιμάκωσης, μετακίνηση δεδομένων, ερώτημα ή δημιουργήστε συναλλαγές.

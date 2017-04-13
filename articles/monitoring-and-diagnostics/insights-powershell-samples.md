<properties
    pageTitle="Azure δείγματα γρήγορης εκκίνησης του PowerShell οθόνη. | Microsoft Azure"
    description="Χρήση του PowerShell για να αποκτήσετε πρόσβαση σε οθόνη Azure δυνατότητες όπως autoscale, ειδοποιήσεις, webhooks και αναζήτηση αρχείων καταγραφής της δραστηριότητας."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure δείγματα γρήγορης εκκίνησης του PowerShell οθόνη

Σε αυτό το άρθρο εμφανίζεται δείγμα τις εντολές του PowerShell για να έχετε πρόσβαση στις δυνατότητες Azure οθόνη. Azure οθόνη σάς επιτρέπει να τις υπηρεσίες Cloud AutoScale, εικονικές μηχανές και εφαρμογές Web και να αποστολή ειδοποιήσεων υπενθύμισης ή κλήση διευθύνσεις URL web με βάση τις τιμές δεδομένων έχει ρυθμιστεί τηλεμετρίας.

>[AZURE.NOTE] Azure οθόνη είναι το νέο όνομα για το τι ονομαζόταν "Ιδέες Azure" μέχρι 25ο Σεπτεμβρίου 2016. Ωστόσο, τα πεδία ονομάτων και, επομένως, τις παρακάτω εντολές εξακολουθεί να περιέχουν "ιδέες".

## <a name="set-up-powershell"></a>Ρύθμιση του PowerShell
Εάν δεν το έχετε κάνει ήδη, ρύθμιση του PowerShell για να εκτελέσετε στον υπολογιστή σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να εγκατάσταση και ρύθμιση παραμέτρων του PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Παραδείγματα σε αυτό το άρθρο

Τα παραδείγματα στο άρθρο εξηγούν πώς μπορείτε να χρησιμοποιήσετε το cmdlet του Azure οθόνη. Μπορείτε επίσης να δείτε ολόκληρη τη λίστα των cmdlet του PowerShell οθόνη Azure στο [cmdlet του Azure οθόνη (ιδέες)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).


## <a name="sign-in-and-use-subscriptions"></a>Είσοδος και χρήση συνδρομών

Πρώτα, συνδεθείτε με τη συνδρομή σας στο Azure.

```PowerShell
Login-AzureRmAccount
```

Απαιτεί να εισέλθετε. Αφού κάνετε, το λογαριασμό σας, εμφανίζονται TenantId και το προεπιλεγμένο αναγνωριστικό συνδρομής. Όλα τα cmdlet του Azure λειτουργεί στο περιβάλλον της συνδρομής σας στο προεπιλεγμένο. Για να προβάλετε τη λίστα των συνδρομών έχετε πρόσβαση, χρησιμοποιήστε την ακόλουθη εντολή.

```PowerShell
Get-AzureRmSubscription
```

Για να αλλάξετε το περιβάλλον εργασίας σε μια διαφορετική συνδρομή, χρησιμοποιήστε την ακόλουθη εντολή.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Ανάκτηση αρχείων καταγραφής ελέγχου για μια συνδρομή
Χρησιμοποιήστε το `Get-AzureRmLog` cmdlet.  Ακολουθούν μερικά συνηθισμένα παραδείγματα.

Λάβετε εγγραφές από αυτό ημερομηνία/ώρα για την παρουσίαση:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Λάβετε εγγραφές καταγραφής ανάμεσα σε μια περιοχή ημερομηνίας/ώρας:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Λάβετε εγγραφές από μια ομάδα συγκεκριμένο πόρο:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Λήψη εγγραφές από μια υπηρεσία παροχής του συγκεκριμένου πόρου ανάμεσα σε μια περιοχή ημερομηνίας/ώρας:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Λάβετε όλες τις εγγραφές καταγραφής με μια συγκεκριμένη καλούντα:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Την ακόλουθη εντολή ανακτά την τελευταία 1000 συμβάντα από το αρχείο καταγραφής ελέγχου:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`υποστηρίζει πολλές άλλες παραμέτρους. Ανατρέξτε στο θέμα το `Get-AzureRmLog` αναφοράς για περισσότερες πληροφορίες.

>[AZURE.NOTE] `Get-AzureRmLog`παρέχει μόνο 15 ημερών του ιστορικού. Χρήση της παραμέτρου **- MaxEvents** σάς επιτρέπει να ερωτήματος τα τελευταία συμβάντα N, πέρα από 15 ημέρες. Σε παλαιότερες από 15 ημέρες συμβάντα access, χρησιμοποιήστε την REST API ή SDK (C# δείγμα χρησιμοποιώντας το SDK). Εάν δεν έχετε συμπεριλάβει **ώρα έναρξης**, στη συνέχεια, η προεπιλεγμένη τιμή είναι **ώρα λήξης** μείον μία ώρα. Εάν δεν έχετε συμπεριλάβει **ώρα λήξης**, η προεπιλεγμένη τιμή είναι τρέχουσας ώρας. Όλες οι ώρες είναι σε UTC.

## <a name="retrieve-alerts-history"></a>Ανάκτηση ιστορικού ειδοποιήσεων
Για να δείτε όλα τα συμβάντα ειδοποίησης, μπορείτε να υποβάλετε ερώτημα τα αρχεία καταγραφής από διαχειριστή πόρων Azure χρησιμοποιώντας τα παρακάτω παραδείγματα.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Για να προβάλετε το ιστορικό για έναν συγκεκριμένο κανόνα ειδοποίησης, μπορείτε να χρησιμοποιήσετε το `Get-AzureRmAlertHistory` cmdlet, που περνά μέσα σε το Αναγνωριστικό πόρου του ειδοποίησης κανόνα.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Το `Get-AzureRmAlertHistory` cmdlet υποστηρίζει διάφορες παραμέτρους. Περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Ανάκτηση πληροφοριών σχετικά με τους κανόνες ειδοποίησης
Όλες τις παρακάτω εντολές λειτουργούν σε μια ομάδα πόρων με το όνομα "montest".

Δείτε όλες τις ιδιότητες του ειδοποίησης κανόνα:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Ανάκτηση όλες τις ειδοποιήσεις σε μια ομάδα πόρων:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Ανακτήστε όλοι οι κανόνες ειδοποίησης για έναν πόρο προορισμού. Για παράδειγμα, όλοι οι κανόνες ειδοποίησης ρύθμιση σε μια Εικονική.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`υποστηρίζει άλλες παραμέτρους. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Δημιουργία κανόνων προειδοποίησης
Μπορείτε να χρησιμοποιήσετε το `Add-AlertRule` cmdlet για να δημιουργήσετε, να ενημερώσετε ή να απενεργοποιήσετε έναν κανόνα ειδοποίησης.

Μπορείτε να δημιουργήσετε μηνύματα ηλεκτρονικού ταχυδρομείου και webhook ιδιότητες χρησιμοποιώντας `New-AzureRmAlertRuleEmail` και `New-AzureRmAlertRuleWebhook`, αντίστοιχα. Στο το cmdlet ειδοποίησης κανόνα, εκχωρήστε τους ως ενέργειες στην ιδιότητα **Ενέργειες** του κανόνα ειδοποίησης.

Στην επόμενη ενότητα περιέχει ένα δείγμα που δείχνει τον τρόπο δημιουργίας μιας ειδοποίησης κανόνα με διάφορες παραμέτρους.

### <a name="alert-rule-on-a-metric"></a>Ειδοποίηση του κανόνα σε ένα μετρικό
Ο παρακάτω πίνακας περιγράφει τις παραμέτρους και τις τιμές που χρησιμοποιούνται για τη δημιουργία ειδοποίησης χρησιμοποιώντας ένα μετρικό.


|παράμετρος|τιμή|
|---|---|
|Όνομα|  simpletestdiskwrite|
|Θέση της ειδοποίησης κανόνα|   Ανατολικής η.π.α.|
|Ομάδα πόρων| montest|
|TargetResourceId|  /Subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|MetricName της ειδοποίησης που έχει δημιουργηθεί|   Εγγραφές \Disk \PhysicalDisk (_Σύνολο) / δευτερόλεπτο. Ανατρέξτε στο θέμα το `Get-MetricDefinitions` cmdlet κάτω από το στοιχείο σχετικά με τον τρόπο για να ανακτήσετε τα ακριβή μετρικό ονόματα|
|Τελεστής|  GreaterThan|
|Οριακή τιμή (count/sec σε για αυτό το μετρικό)|    1|
|WindowSize (hh:mm:ss μορφή)|  05:00:00|
|Συνάθροιση (στατιστική τιμή από τη μέτρηση, που χρησιμοποιεί μέση μέτρηση, σε αυτήν την περίπτωση)|  Μέσος όρος|
|προσαρμοσμένα μηνύματα ηλεκτρονικού ταχυδρομείου (συμβολοσειρά πίνακας)|'foo@example.com','bar@example.com'|
|Αποστολή μηνύματος ηλεκτρονικού ταχυδρομείου για τους κατόχους, οι συνεργάτες και οι αναγνώστες|    -SendToServiceOwners|

Δημιουργήστε μια ενέργεια ηλεκτρονικού ταχυδρομείου

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Δημιουργήστε μια ενέργεια Webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Δημιουργία ειδοποίησης κανόνα σε τη μέτρηση % CPU σε μια Εικονική κλασική

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Ανάκτηση ειδοποίησης κανόνα

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Το cmdlet ειδοποίησης Προσθήκη ενημερώνει επίσης τον κανόνα, εάν υπάρχει ήδη μια ειδοποίηση κανόνας για τις ιδιότητες που δίνεται. Για να απενεργοποιήσετε έναν κανόνα ειδοποίησης, συμπεριλάβετε την παράμετρο **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Ειδοποίηση στο συμβάν καταγραφής ελέγχου

>[AZURE.NOTE] Αυτή η δυνατότητα είναι ακόμα σε προεπισκόπηση.

Σε αυτό το σενάριο, θα μπορείτε να στείλετε μήνυμα ηλεκτρονικού ταχυδρομείου όταν ξεκινήσει με επιτυχία με μια τοποθεσία Web στη συνδρομή μου στο ομάδα πόρων *abhingrgtest123*.

Ρύθμιση κανόνα ηλεκτρονικού ταχυδρομείου

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Ρύθμιση κανόνα webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Δημιουργία κανόνα με το συμβάν

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Ανάκτηση ειδοποίησης κανόνα

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Η `Add-AlertRule` cmdlet επιτρέπει σε διάφορες άλλες παραμέτρους. Περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προσθήκη AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Λάβετε μια λίστα με τις διαθέσιμες μετρήσεις για ειδοποιήσεις
Μπορείτε να χρησιμοποιήσετε το `Get-AzureRmMetricDefinition` cmdlet για να προβάλετε τη λίστα των όλες τις μετρήσεις για ένα συγκεκριμένο πόρο.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Το παρακάτω παράδειγμα δημιουργεί έναν πίνακα με τη μέτρηση ονόματος και της μονάδας για αυτήν.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Μια πλήρη λίστα των διαθέσιμων επιλογών για `Get-AzureRmMetricDefinition` είναι διαθέσιμη από την [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Δημιουργία και διαχείριση ρυθμίσεων AutoScale
Ένας πόρος, όπως μια εφαρμογή Web, Εικονική, υπηρεσία Cloud ή να ρυθμίσετε κλίμακα Εικονική μπορεί να έχει μόνο μία autoscale ρύθμιση έχει ρυθμιστεί για αυτήν.
Ωστόσο, κάθε ρύθμιση autoscale μπορεί να έχει πολλά προφίλ. Για παράδειγμα, μία για ένα προφίλ βάσει επιδόσεων κλίμακα και ένα δεύτερο για ένα χρονοδιάγραμμα με βάση το προφίλ. Κάθε προφίλ μπορεί να έχει πολλούς κανόνες που έχει ρυθμιστεί σε αυτήν. Για περισσότερες πληροφορίες σχετικά με την Autoscale, δείτε [πώς μπορείτε να Autoscale μια εφαρμογή του](../cloud-services/cloud-services-how-to-scale.md).

Ακολουθούν τα βήματα, θα χρησιμοποιήσουμε:

1. Δημιουργία κανόνων.
2. Δημιουργία προφίλ αντιστοίχιση των κανόνων που δημιουργήσατε προηγουμένως σε των προφίλ.
3. Προαιρετικό: Δημιουργία ειδοποιήσεων για autoscale, ρυθμίζοντας τις παραμέτρους ιδιοτήτων webhook και ηλεκτρονικού ταχυδρομείου.
4. Δημιουργήστε μια ρύθμιση autoscale με ένα όνομα στον πόρο προορισμού αντιστοιχίζοντας το προφίλ και τις ειδοποιήσεις που δημιουργήσατε στα προηγούμενα βήματα.

Τα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να δημιουργήσετε μια ρύθμιση Autoscale για μια κλίμακα Εικονική ρύθμιση για ένα λειτουργικό σύστημα Windows, με βάση, χρησιμοποιώντας τη μέτρηση χρήση CPU.

Πρώτα, δημιουργήστε έναν κανόνα για την κλίμακα ανάληψης, με αύξηση count παρουσία.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Στη συνέχεια, δημιουργήστε έναν κανόνα κλίμακα στο, με μια παρουσία μείωση count.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Στη συνέχεια, δημιουργήστε ένα προφίλ για τους κανόνες.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Δημιουργήστε μια ιδιότητα webhook.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Δημιουργήστε την ιδιότητα ειδοποίησης για τη ρύθμιση autoscale, συμπεριλαμβανομένων των μηνυμάτων ηλεκτρονικού ταχυδρομείου και το webhook που δημιουργήσατε προηγουμένως.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Τέλος, δημιουργήστε τη ρύθμιση autoscale για να προσθέσετε το προφίλ που δημιουργήσατε παραπάνω.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Για περισσότερες πληροφορίες σχετικά με τη Διαχείριση ρυθμίσεων Autoscale, ανατρέξτε στο θέμα [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Ιστορικό Autoscale
Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δείτε πρόσφατες autoscale και συμβάντα ειδοποίησης. Χρησιμοποιήστε την αναζήτηση του αρχείου καταγραφής ελέγχου για να προβάλετε το ιστορικό Autoscale.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Μπορείτε να χρησιμοποιήσετε το `Get-AzureRmAutoScaleHistory` cmdlet για να ανακτήσετε AutoScale ιστορικού.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Προβολή λεπτομερειών για μια ρύθμιση autoscale
Μπορείτε να χρησιμοποιήσετε το `Get-Autoscalesetting` cmdlet για να ανακτήσετε περισσότερες πληροφορίες σχετικά με τη ρύθμιση autoscale.

Το παρακάτω παράδειγμα εμφανίζει λεπτομέρειες σχετικά με όλες τις ρυθμίσεις autoscale στην ομάδα πόρων 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Το παρακάτω παράδειγμα εμφανίζει λεπτομέρειες σχετικά με όλες τις ρυθμίσεις autoscale σε ομάδα πόρων 'myrg1' και συγκεκριμένα τη ρύθμιση autoscale που ονομάζεται 'MyScaleVMSSSetting'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Καταργήστε μια ρύθμιση autoscale
Μπορείτε να χρησιμοποιήσετε το `Remove-Autoscalesetting` cmdlet για να διαγράψετε μια ρύθμιση autoscale.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Διαχείριση προφίλ καταγραφής για αρχεία καταγραφής ελέγχου

Μπορείτε να δημιουργήσετε ένα *αρχείο καταγραφής προφίλ* και να εξαγάγετε δεδομένα από το αρχεία καταγραφής ελέγχου σε ένα λογαριασμό του χώρου αποθήκευσης και μπορείτε να ρυθμίσετε τις παραμέτρους διατήρηση δεδομένων για αυτό. Προαιρετικά, μπορείτε επίσης να κατευθύνετε τα δεδομένα σας διανομέα συμβάν. Σημειώστε ότι αυτή η δυνατότητα είναι αυτήν τη στιγμή σε προεπισκόπηση και μπορείτε να δημιουργήσετε μόνο ένα αρχείο καταγραφής προφίλ ανά συνδρομή. Μπορείτε να χρησιμοποιήσετε τα ακόλουθα cmdlet με την τρέχουσα συνδρομή σας για να δημιουργήσετε και να διαχειριστείτε τα προφίλ καταγραφής. Μπορείτε επίσης να επιλέξετε μια συγκεκριμένη συνδρομή. Παρόλο που PowerShell ως προεπιλογή για την τρέχουσα συνδρομή σας, μπορείτε πάντα να αλλάξετε αυτήν χρησιμοποιώντας `Set-AzureRmContext`. Μπορείτε να ρυθμίσετε αρχεία καταγραφής ελέγχου για να δρομολόγηση δεδομένα σε οποιαδήποτε λογαριασμού χώρου αποθήκευσης ή συμβάν διανομέα μέσα σε αυτήν τη συνδρομή. Δεδομένα γράφονται ως blob αρχεία σε μορφή JSON.

### <a name="get-a-log-profile"></a>Λάβετε ένα προφίλ του αρχείου καταγραφής
Για να κάνετε λήψη του προφίλ σας υπάρχον αρχείο καταγραφής, χρησιμοποιήστε το `Get-AzureRmLogProfile` cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Προσθέστε ένα προφίλ καταγραφής χωρίς διατήρησης δεδομένων

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Κατάργηση ενός προφίλ του αρχείου καταγραφής

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Προσθέστε ένα προφίλ καταγραφής με διατήρηση των δεδομένων

Μπορείτε να καθορίσετε την ιδιότητα **- RetentionInDays** με τον αριθμό των ημερών, ως ένα θετικό ακέραιο αριθμό, όπου τα δεδομένα διατηρούνται.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Προσθήκη αρχείου καταγραφής προφίλ με διατήρησης και EventHub
Εκτός από τη δρομολόγηση των δεδομένων σας με το λογαριασμό χώρου αποθήκευσης, μπορείτε να κατευθύνετε επίσης το με ένα διανομέα συμβάν. Σημειώστε ότι, σε αυτήν την έκδοση preview και το χώρο αποθήκευσης, ρύθμιση παραμέτρων λογαριασμού είναι υποχρεωτική αλλά ρύθμισης παραμέτρων συμβάντων διανομέα είναι προαιρετικό.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Ρύθμιση παραμέτρων αρχείων καταγραφής διαγνωστικών
Πολλές υπηρεσίες Azure παρέχουν πρόσθετες αρχεία καταγραφής και τηλεμετρίας, όπως τις ομάδες ασφαλείας δικτύου Azure, Balancers φόρτωσης λογισμικού, θάλαμο αριθμού-κλειδιού, υπηρεσίες αναζήτησης Azure, και λογική εφαρμογές και μπορεί να ρυθμιστεί για την αποθήκευση δεδομένων στο λογαριασμό σας στο χώρο αποθήκευσης Azure. Αυτή η λειτουργία μπορεί να εκτελεστεί μόνο σε επίπεδο πόρων και το λογαριασμό χώρου αποθήκευσης θα πρέπει να υπάρχουν στην ίδια περιοχή ως τον πόρο προορισμού όπου έχει ρυθμιστεί η ρύθμιση Διαγνωστικά.

### <a name="get-diagnostic-setting"></a>Λήψη ρύθμισης διαγνωστικών

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Απενεργοποιήσετε τη ρύθμιση διαγνωστικών

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Ενεργοποίηση διαγνωστικών ρύθμιση χωρίς διατήρησης

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Ενεργοποίηση διαγνωστικών ρύθμιση με διατήρησης

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Ενεργοποίηση διαγνωστικών ρύθμιση με διατήρησης για το συγκεκριμένο αρχείο καταγραφής κατηγορίας

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

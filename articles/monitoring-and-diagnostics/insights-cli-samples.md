<properties
    pageTitle="Δείγματα γρήγορης εκκίνησης Azure CLI οθόνη. | Microsoft Azure"
    description="Δείγμα CLI εντολές για τις δυνατότητες του Azure οθόνη. Azure οθόνη είναι μια υπηρεσία Microsoft Azure που σας επιτρέπει να στείλετε ειδοποιήσεις, καλέστε διευθύνσεις URL web με βάση τις τιμές δεδομένων έχει ρυθμιστεί τηλεμετρίας, και τις υπηρεσίες Cloud autoScale, εικονικές μηχανές και εφαρμογές Web."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Δείγματα γρήγορης εκκίνησης Azure CLI πλατφόρμες οθόνη

Αυτό το άρθρο σάς δείχνει δείγμα περιβάλλον γραμμής εντολών (CLI) εντολές για να σας βοηθήσει να πρόσβαση στις δυνατότητες Azure οθόνη. Azure οθόνη σάς επιτρέπει να τις υπηρεσίες Cloud AutoScale, εικονικές μηχανές και εφαρμογές Web και να αποστολή ειδοποιήσεων υπενθύμισης ή κλήση διευθύνσεις URL web με βάση τις τιμές δεδομένων έχει ρυθμιστεί τηλεμετρίας.

>[AZURE.NOTE] Azure οθόνη είναι το νέο όνομα για το τι ονομαζόταν "Ιδέες Azure" μέχρι 25ο Σεπτεμβρίου 2016. Ωστόσο, τα πεδία ονομάτων και, επομένως, τις παρακάτω εντολές εξακολουθεί να περιέχουν "ιδέες".


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εάν δεν έχετε εγκαταστήσει ήδη το Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση του Azure CLI](../xplat-cli-install.md). Εάν δεν είστε εξοικειωμένοι με το Azure CLI, μπορείτε να διαβάσετε περισσότερα σχετικά με το στο θέμα [χρήση του Azure CLI για Mac, Linux, και Windows με το διαχειριστή πόρων Azure](../xplat-cli-azure-resource-manager.md).


Στα Windows, εγκαταστήστε npm από την [τοποθεσία Web Node.js](https://nodejs.org/). Μετά την ολοκλήρωση της εγκατάστασης, με χρήση CMD.exe με δικαιώματα εκτέλεση ως διαχειριστής, εκτελέστε τις ακόλουθες ενέργειες από το φάκελο όπου είναι εγκατεστημένο το npm:

```console
npm install azure-cli --global
```

Στη συνέχεια, μεταβείτε σε οποιαδήποτε φάκελο/θέση που θέλετε και πληκτρολογήστε στη γραμμή εντολών:

```console
azure help
```

## <a name="log-in-to-azure"></a>Συνδεθείτε στο Azure

Το πρώτο βήμα είναι να συνδεθείτε στο λογαριασμό σας Azure.

```console
azure login
```

Μετά την εκτέλεση αυτής της εντολής, πρέπει να εισέλθουν μέσω τις οδηγίες στην οθόνη. Μετά από αυτό, μπορείτε να δείτε το λογαριασμό, TenantId και το προεπιλεγμένο αναγνωριστικό συνδρομής. Όλες οι εντολές που λειτουργούν στο περιβάλλον της συνδρομής σας στο προεπιλεγμένο.

Για να εμφανίσετε τις λεπτομέρειες της τρέχουσας συνδρομής σας, χρησιμοποιήστε την ακόλουθη εντολή.

```console
azure account show
```

Για να αλλάξετε μια διαφορετική συνδρομή του περιβάλλοντος εργασίας, χρησιμοποιήστε την ακόλουθη εντολή.

```console
azure account set "subscription ID or subscription name"
```

Για να χρησιμοποιήσετε τις εντολές για τη διαχείριση πόρων Azure και την εποπτεία Azure, πρέπει να είναι σε λειτουργία Azure διαχείριση πόρων.

```console
azure config mode arm
```

Για να προβάλετε μια λίστα με όλες τις υποστηριζόμενες εντολές Azure οθόνη, κάντε τα εξής.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Προβολή αρχείων καταγραφής ελέγχου για μια συνδρομή

Για να προβάλετε μια λίστα των αρχείων καταγραφής ελέγχου, κάντε τα εξής.

```console
azure insights logs list [options]
```

Δοκιμάστε τα εξής για να προβάλετε όλες τις διαθέσιμες επιλογές.

```console
azure insights logs list -help
```

Ακολουθεί ένα παράδειγμα λίστα αρχεία καταγραφής από μια ομάδα πόρων

```console
azure insights logs list --resourceGroup "myrg1"
```

Παράδειγμα λίστας αρχεία καταγραφής από τον καλούντα

```console
azure insights logs list --caller "myname@company.com"
```

Παράδειγμα λίστας αρχεία καταγραφής από τον καλούντα σε έναν τύπο πόρου, μέσα σε μια ημερομηνία έναρξης και λήξης

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Εργασία με τις ειδοποιήσεις
Μπορείτε να χρησιμοποιήσετε τις πληροφορίες στην ενότητα για να εργαστείτε με τις ειδοποιήσεις.

### <a name="get-alert-rules-in-a-resource-group"></a>Λήψη ειδοποίησης κανόνες σε μια ομάδα πόρων

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Δημιουργία κανόνα μετρικό ειδοποίησης

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Δημιουργία ειδοποίησης κανόνα αρχείου καταγραφής

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Δημιουργία ειδοποίησης κανόνα webtest

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Διαγραφή κανόνα ειδοποίησης

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Αρχείο καταγραφής προφίλ
Χρησιμοποιήστε τις πληροφορίες σε αυτήν την ενότητα για να εργαστείτε με τα προφίλ καταγραφής.

### <a name="get-a-log-profile"></a>Λάβετε ένα προφίλ του αρχείου καταγραφής

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Προσθέστε ένα προφίλ καταγραφής χωρίς διατήρησης

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Κατάργηση ενός προφίλ του αρχείου καταγραφής

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Προσθέστε ένα προφίλ καταγραφής με διατήρησης

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Προσθέστε ένα προφίλ καταγραφής με διατήρησης και EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Εργαλεία διαγνωστικών
Χρησιμοποιήστε τις πληροφορίες σε αυτήν την ενότητα για να εργαστείτε με διαγνωστικών ρυθμίσεων.

### <a name="get-a-diagnostic-setting"></a>Λάβετε μια ρύθμιση διαγνωστικών

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Απενεργοποίηση μια ρύθμιση διαγνωστικών

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Ενεργοποίηση μια ρύθμιση διαγνωστικών χωρίς διατήρησης

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Autoscale
Χρησιμοποιήστε τις πληροφορίες σε αυτήν την ενότητα για να εργαστείτε με τις ρυθμίσεις autoscale. Πρέπει να τροποποιήσουν αυτά τα παραδείγματα.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Λήψη autoscale ρυθμίσεις για μια ομάδα πόρων

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Λήψη autoscale ρυθμίσεις με βάση το όνομα σε μια ομάδα πόρων

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Ορισμός ρυθμίσεων auotoscale

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```

<properties
    pageTitle="Διαχείριση Cache Azure Redis με το Azure PowerShell | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να εκτελέσετε εργασίες διαχείρισης για το Cache Redis Azure χρησιμοποιώντας το Azure PowerShell."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Διαχείριση Cache Azure Redis με το Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Αυτό το θέμα δείχνει τρόπος για να εκτελέσετε τα κοινές εργασίες δημιουργήσετε όπως, ενημέρωση, και κλιμάκωση παρουσίες του Azure Redis Cache, πώς μπορείτε να δημιουργήσετε ξανά τα πλήκτρα πρόσβασης και πώς μπορείτε να προβάλετε πληροφορίες σχετικά με το μνήμης cache. Για μια πλήρη λίστα των cmdlet του Azure Redis Cache PowerShell, ανατρέξτε στο θέμα [cmdlet Azure Redis Cache](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][κλασική μοντέλο](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Εάν έχετε ήδη εγκαταστήσει Azure PowerShell, πρέπει να έχετε Azure PowerShell έκδοση 1.0.0 ή νεότερη έκδοση. Μπορείτε να ελέγξετε την έκδοση του Azure PowerShell που έχετε εγκαταστήσει με αυτήν την εντολή στη γραμμή εντολών του PowerShell Azure.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Αρχικά, πρέπει να συνδεθείτε Azure με αυτήν την εντολή.

    Login-AzureRmAccount

Καθορίστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου του Azure το λογαριασμό σας και τον κωδικό πρόσβασης του από το Microsoft Azure εισόδου στο παράθυρο διαλόγου.

Στη συνέχεια, εάν έχετε πολλές συνδρομές Azure, πρέπει να ορίσετε τη συνδρομή σας στο Azure. Για να δείτε μια λίστα με τις συνδρομές σας στο τρέχον, εκτέλεση αυτής της εντολής.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Για να καθορίσετε τη συνδρομή, εκτελέστε την ακόλουθη εντολή. Στο παρακάτω παράδειγμα, είναι το όνομα της συνδρομής `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Μπορείτε να χρησιμοποιήσετε του Windows PowerShell με τη διαχείριση πόρων Azure, χρειάζεστε τα εξής:

- Windows PowerShell, την έκδοση 3.0 ή 4.0. Για να βρείτε την έκδοση του Windows PowerShell, πληκτρολογήστε:`$PSVersionTable` και βεβαιωθείτε ότι η τιμή του `PSVersion` είναι 3.0 ή 4.0. Για να εγκαταστήσετε μια συμβατή έκδοση, ανατρέξτε στο θέμα [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ή [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Για να λάβετε αναλυτικές πληροφορίες Βοήθειας για οποιαδήποτε cmdlet που βλέπετε σε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιήστε το cmdlet Get-Βοήθεια.

    Get-Help <cmdlet-name> -Detailed

Για παράδειγμα, για να λάβετε Βοήθεια για το `New-AzureRmRedisCache` cmdlet, πληκτρολογήστε:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Πώς μπορείτε να συνδεθείτε στο Cloud για δημόσιους οργανισμούς Azure ή Cloud Κίνα Azure

Από προεπιλογή το Azure είναι περιβάλλον `AzureCloud`, που αντιπροσωπεύει την παρουσία του καθολικού Azure cloud. Για να συνδεθείτε με μια διαφορετική εμφάνιση, χρησιμοποιήστε το `Add-AzureRmAccount` εντολής με το `-Environment` ή -`EnvironmentName` διακόπτη γραμμής εντολών με το επιθυμητό περιβάλλον ή περιβάλλον.

Για να δείτε τη λίστα διαθέσιμες περιβάλλοντα, εκτελέστε το `Get-AzureRmEnvironment` cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Για να συνδεθείτε στο cloud για δημόσιους οργανισμούς Azure

Για να συνδεθείτε στο cloud για δημόσιους οργανισμούς Azure, χρησιμοποιήστε μία από τις παρακάτω εντολές.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

ή

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Για να δημιουργήσετε ένα cache στο Cloud για δημόσιους οργανισμούς Azure, χρησιμοποιήστε μία από τις ακόλουθες θέσεις.

-   USGov Βιρτζίνια
-   USGov Iowa

Για περισσότερες πληροφορίες σχετικά με το Cloud για δημόσιους οργανισμούς Azure, ανατρέξτε στο θέμα [Microsoft Azure για δημόσιους οργανισμούς](https://azure.microsoft.com/features/gov/) και [Οδηγός για προγραμματιστές του Microsoft Azure για δημόσιους οργανισμούς](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Για να συνδεθείτε με το Cloud Κίνα Azure

Για να συνδεθείτε στο cloud Κίνα Azure, χρησιμοποιήστε μία από τις παρακάτω εντολές.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

ή

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Για να δημιουργήσετε ένα cache στο Cloud Κίνα Azure, χρησιμοποιήστε μία από τις ακόλουθες θέσεις.

-   Κίνα Ανατολή
-   Βόρεια Κίνα

Για περισσότερες πληροφορίες σχετικά με το Cloud Azure Κίνα, ανατρέξτε στο θέμα [AzureChinaCloud για Azure με διαχείριση από 21Vianet στην Κίνα](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Ιδιότητες που χρησιμοποιούνται για το Azure Redis Cache PowerShell

Ο παρακάτω πίνακας περιέχει ιδιότητες και τις περιγραφές για παραμέτρους που χρησιμοποιούνται συχνά κατά τη δημιουργία και διαχείριση του Azure Redis Cache παρουσίες με χρήση του PowerShell Azure.

| Παράμετρος          | Περιγραφή                                                                                                                                                                                                        | Προεπιλεγμένη  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Όνομα               | Όνομα της μνήμης cache                                                                                                                                                                                                  |          |
| Θέση           | Θέση της μνήμης cache                                                                                                                                                                                              |          |
| ResourceGroupName  | Όνομα πόρου ομάδα στην οποία θέλετε να δημιουργήσετε το cache                                                                                                                                                                   |          |
| Μέγεθος               | Το μέγεθος της μνήμης cache. Οι έγκυρες τιμές είναι: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Ο αριθμός των shards για να δημιουργήσετε κατά τη δημιουργία μιας cache premium με σύμπλεγμα με δυνατότητα. Οι έγκυρες τιμές είναι: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Καθορίζει την SKU του cache. Οι έγκυρες τιμές είναι: βασικές, τυπικό, Premium                                                                                                                                         | Τυπική |
| RedisConfiguration | Καθορίζει ρυθμίσεις παραμέτρων Redis. Για λεπτομέρειες σχετικά με κάθε ρύθμιση, ανατρέξτε στον παρακάτω πίνακα [RedisConfiguration ιδιότητες](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Υποδεικνύει εάν είναι ενεργοποιημένη η θύρα μη SSL.                                                                                                                                                                     | FALSE    |
| MaxMemoryPolicy    | Αυτή η παράμετρος έχει καταργηθεί - Χρησιμοποιήστε RedisConfiguration αντί για αυτό.                                                                                                                                              |          |
| StaticIP           | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει μια μοναδική διεύθυνση IP στο δευτερεύον για το cache. Εάν δεν παρέχεται, θα επιλεγεί για εσάς από το υποδίκτυο.                                                                                                                     |          |
| Υποδίκτυο             | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει το όνομα του υποδικτύου στην οποία θέλετε να αναπτύξετε το cache.                                                                                                                  |          |
| VirtualNetwork     | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει το Αναγνωριστικό πόρου του VNET στην οποία θέλετε να αναπτύξετε το cache.                                                                                                             |          |
| Τύπο κλειδιού            | Καθορίζει ποιο πλήκτρο πρόσβασης για να αναδημιουργήσετε κατά την ανανέωση των πλήκτρων πρόσβασης. Οι έγκυρες τιμές είναι: κύρια, δευτερεύουσα |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>Ιδιότητες RedisConfiguration

| Ιδιότητα                      | Περιγραφή                                                                                                          | Πληροφορίες για την τιμολόγηση βαθμίδες       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| δυνατότητα δημιουργίας αντιγράφων ασφαλείας RDB            | Αν [Redis διατήρησης δεδομένων](cache-how-to-premium-persistence.md) είναι ενεργοποιημένο                                     | Premium μόνο        |
| RDB-χώρος αποθήκευσης--συμβολοσειρά σύνδεσης | Η συμβολοσειρά σύνδεσης με το λογαριασμό χώρου αποθήκευσης για [Redis διατήρησης δεδομένων](cache-how-to-premium-persistence.md)       | Premium μόνο        |
| Δημιουργία αντιγράφων ασφαλείας-συχνότητας RDB          | Τη συχνότητα δημιουργίας αντιγράφων ασφαλείας για [Redis διατήρησης δεδομένων](cache-how-to-premium-persistence.md)                               | Premium μόνο        |
| δεσμευμένες maxmemory            | Ρυθμίζει τις παραμέτρους της [μνήμης που δεσμεύεται](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) για διαδικασίες-cache | Τυπική και Premium |
| maxmemory πολιτικής              | Ρυθμίζει τις παραμέτρους της [πολιτικής eviction](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) για το cache           | Όλα τα επίπεδα τιμολόγησης   |
| Ειδοποιήστε-keyspace-συμβάντα        | Ρυθμίζει τις παραμέτρους [keyspace ειδοποιήσεων](cache-configure.md#keyspace-notifications-advanced-settings)                     | Τυπική και Premium |
| Κατακερματισμός max-ziplist-τις εγγραφές      | Ρυθμίζει τις παραμέτρους [Βελτιστοποίηση μνήμης](http://redis.io/topics/memory-optimization) για τους τύπους δεδομένων μικρές συγκεντρωτικών αποτελεσμάτων          | Τυπική και Premium |
| Κατακερματισμός max-ziplist-τιμή        | Ρυθμίζει τις παραμέτρους [Βελτιστοποίηση μνήμης](http://redis.io/topics/memory-optimization) για τους τύπους δεδομένων μικρές συγκεντρωτικών αποτελεσμάτων          | Τυπική και Premium |
| Ορισμός max-intset-τις εγγραφές        | Ρυθμίζει τις παραμέτρους [Βελτιστοποίηση μνήμης](http://redis.io/topics/memory-optimization) για τους τύπους δεδομένων μικρές συγκεντρωτικών αποτελεσμάτων          | Τυπική και Premium |
| zset max-ziplist-τις εγγραφές      | Ρυθμίζει τις παραμέτρους [Βελτιστοποίηση μνήμης](http://redis.io/topics/memory-optimization) για τους τύπους δεδομένων μικρές συγκεντρωτικών αποτελεσμάτων          | Τυπική και Premium |
| zset max-ziplist-τιμή        | Ρυθμίζει τις παραμέτρους [Βελτιστοποίηση μνήμης](http://redis.io/topics/memory-optimization) για τους τύπους δεδομένων μικρές συγκεντρωτικών αποτελεσμάτων          | Τυπική και Premium |
| βάσεις δεδομένων                     | Ρυθμίζει τον αριθμό των βάσεων δεδομένων. Αυτή η ιδιότητα μπορεί να ρυθμιστεί μόνο κατά τη δημιουργία cache.                          | Τυπική και Premium |

## <a name="to-create-a-redis-cache"></a>Για να δημιουργήσετε ένα Cache Redis

Νέες παρουσίες Azure Redis Cache δημιουργούνται χρησιμοποιώντας το cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

>[AZURE.IMPORTANT] Την πρώτη φορά που δημιουργείτε ένα cache Redis σε μια συνδρομή με την πύλη Azure, καταχωρήσει την πύλη του `Microsoft.Cache` χώρος ονομάτων για αυτήν τη συνδρομή. Εάν προσπαθήσετε να δημιουργήσετε το πρώτο cache Redis σε μια συνδρομή με χρήση του PowerShell, πρέπει πρώτα να δηλώσετε χώρου ονομάτων χρησιμοποιώντας την ακόλουθη εντολή; Διαφορετικά cmdlet του όπως `New-AzureRmRedisCache` και `Get-AzureRmRedisCache` αποτύχει.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `New-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Για να δημιουργήσετε ένα cache με προεπιλεγμένες παραμέτρους, εκτελέστε την ακόλουθη εντολή.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, και `Location` είναι απαιτείται παράμετροι, αλλά τα υπόλοιπα είναι προαιρετικές και οι προεπιλεγμένες τιμές. Εκτέλεση της προηγούμενης εντολής δημιουργεί μια εμφάνιση τυπική SKU Azure Redis Cache με το καθορισμένο όνομα, θέση και ομάδα πόρων, η οποία είναι 1 GB σε μέγεθος με τη θύρα χωρίς SSL απενεργοποιηθεί.

Για να δημιουργήσετε ένα cache premium, καθορίστε το μέγεθος του P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB), ή P4 (53 GB - 530 GB). Για να ενεργοποιήσετε την υπηρεσία συμπλέγματος, καθορίστε ένα πλήθος shard χρησιμοποιώντας το `ShardCount` παραμέτρου. Το παρακάτω παράδειγμα δημιουργεί μια cache premium P1 με 3 shards. Μνήμη cache premium P1 6 GB στο μέγεθος και δεδομένου ότι θα σας που καθορίζονται σε τρεις shards το συνολικό μέγεθος 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Για να καθορίσετε τιμές για το `RedisConfiguration` παράμετρο, περικλείστε τις τιμές μέσα σε `{}` ως κλειδιού/τιμής όπως ζεύγη `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Το ακόλουθο παράδειγμα δημιουργεί μια τυπική cache 1 GB με `allkeys-random` maxmemory πολιτικής keyspace ειδοποιήσεις και έχουν ρυθμιστεί με `KEA`. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Keyspace ειδοποιήσεις (ρυθμίσεις για προχωρημένους)](cache-configure.md#keyspace-notifications-advanced-settings) και [Maxmemory πολιτικής και δεσμευμένες maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Για να ρυθμίσετε τις παραμέτρους του βάσεις δεδομένων κατά τη δημιουργία του cache

Το `databases` ρύθμιση μπορεί να γίνει μόνο κατά τη δημιουργία της cache. Το ακόλουθο παράδειγμα δημιουργεί μια premium P3 cache (26 GB) με 48 βάσεις δεδομένων χρησιμοποιώντας το cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) .

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Για περισσότερες πληροφορίες σχετικά με το `databases` ιδιότητα, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων του διακομιστή προεπιλεγμένη Azure Redis Cache](cache-configure.md#default-redis-server-configuration). Για περισσότερες πληροφορίες σχετικά με τη δημιουργία μιας cache χρησιμοποιώντας το cmdlet [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) , ανατρέξτε στην προηγούμενη ενότητα [για να δημιουργήσετε ένα Cache Redis](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Για να ενημερώσετε ένα cache Redis

Azure παρουσίες Redis Cache ενημερώνονται χρησιμοποιώντας το cmdlet [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) .

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Set-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Το `Set-AzureRmRedisCache` cmdlet μπορούν να χρησιμοποιηθούν για την ενημέρωση ιδιοτήτων όπως `Size`, `Sku`, `EnableNonSslPort`, και το `RedisConfiguration` τιμές. 

Την ακόλουθη εντολή ενημερώνει την πολιτική maxmemory για το Cache Redis με το όνομα myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Για να κλιμακωθεί ένα cache Redis

`Set-AzureRmRedisCache`μπορεί να χρησιμοποιηθεί για να κλιμακωθεί ένα cache Azure Redis παρουσίας όταν το `Size`, `Sku`, ή `ShardCount` είναι η τροποποίηση των ιδιοτήτων. 

>[AZURE.NOTE]Κλιμάκωση ένα cache με χρήση του PowerShell υπόκειται σε το ίδιο όρια και οδηγίες ως κλίμακας ένα cache από την πύλη του Azure. Μπορείτε να κλιμάκωση σε διαφορετικό επίπεδο τιμολόγησης με τους ακόλουθους περιορισμούς.
>
>-  Δεν μπορείτε να προσθέσετε από το ανώτερο επίπεδο τιμολόγησης ένα επίπεδο κάτω τις πληροφορίες τιμολόγησης.
>    -    Δεν είναι δυνατό να κλιμακωθεί από ένα cache **Premium** προς τα κάτω σε ένα **πρότυπο** ή ένα **βασικό** cache.
>    -    Δεν είναι δυνατό να κλιμακωθεί από μια **Τυπική** cache προς τα κάτω σε μια **Βασική** cache.
>-  Μπορείτε να κλιμάκωση από ένα **βασικό** cache για μια **Τυπική** προσωρινή αλλά δεν μπορείτε να αλλάξετε το μέγεθος, την ίδια στιγμή. Εάν χρειάζεστε διαφορετικό μέγεθος, μπορείτε να κάνετε μια διαδοχική λειτουργία κλίμακας στο επιθυμητό μέγεθος.
>-  Δεν είναι δυνατό να κλιμακωθεί από ένα **βασικό** cache απευθείας στο **Premium** cache. Πρέπει να κλίμακα από **βασικές** σε **Τυπική** σε μια λειτουργία κλίμακας και, στη συνέχεια, από **Τυπική** **Premium** σε μια λειτουργία οι επόμενες κλίμακας.
>-  Δεν είναι δυνατό να κλιμακωθεί από ένα μεγαλύτερο μέγεθος προς τα κάτω για να το **C0 (250 MB)** μέγεθος.
>
>Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Πώς να Cache Redis Azure κλίμακα](cache-how-to-scale.md).

Το παρακάτω παράδειγμα εμφανίζει τον τρόπο για να κλιμακωθεί ένα cache με το όνομα `myCache` για μια προσωρινή 2,5 GB. Σημειώστε ότι αυτή η εντολή λειτουργεί για μια βασική ή ένα τυπικό cache.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Μετά από αυτή την εντολή εκδίδεται, επιστρέφεται η τιμή της κατάστασης της μνήμης cache (παρόμοια με την κλήση `Get-AzureRmRedisCache`). Λάβετε υπόψη ότι η `ProvisioningState` είναι `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Όταν ολοκληρωθεί, η λειτουργία κλίμακας του `ProvisioningState` αλλάζει σε `Succeeded`. Εάν πρέπει να κάνετε μια διαδοχική λειτουργία κλίμακας, όπως η αλλαγή από το βασικό στο πρότυπο και, στη συνέχεια, να αλλάξετε το μέγεθος, θα πρέπει να περιμένετε μέχρι την προηγούμενη λειτουργία έχει ολοκληρωθεί ή λαμβάνετε ένα σφάλμα παρόμοιο με το ακόλουθο.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Για πληροφορίες σχετικά με ένα Redis cache

Μπορείτε να ανακτήσετε πληροφορίες σχετικά με ένα cache χρησιμοποιώντας το cmdlet [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) .

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Get-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Για να λάβετε πληροφορίες σχετικά με όλες οι cache στην τρέχουσα εγγραφή, εκτελέστε `Get-AzureRmRedisCache` χωρίς παραμέτρους.

    Get-AzureRmRedisCache

Για να λάβετε πληροφορίες σχετικά με όλες οι cache μιας συγκεκριμένης ομάδας πόρων, εκτελέστε `Get-AzureRmRedisCache` με το `ResourceGroupName` παραμέτρου.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Για να λάβετε πληροφορίες σχετικά με ένα συγκεκριμένο cache, εκτελέστε `Get-AzureRmRedisCache` με το `Name` παραμέτρου που περιέχει το όνομα της μνήμης cache, και το `ResourceGroupName` παραμέτρου με την ομάδα των πόρων που περιέχει αυτό το cache.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Για να ανακτήσετε τα πλήκτρα πρόσβασης για ένα cache Redis

Για να ανακτήσετε τα πλήκτρα πρόσβασης για το cache, μπορείτε να χρησιμοποιήσετε το cmdlet [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) .

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Get-AzureRmRedisCacheKey`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Για να ανακτήσετε τα κλειδιά για το cache, καλέστε την `Get-AzureRmRedisCacheKey` cmdlet και μεταβιβάζουν στο όνομα του cache το όνομα της ομάδας πόρων που περιέχει το cache.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Για να δημιουργήσετε ξανά τα πλήκτρα πρόσβασης για το cache Redis

Για να δημιουργήσετε ξανά το συνδυασμό πλήκτρων πρόσβασης για το cache, μπορείτε να χρησιμοποιήσετε το cmdlet [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) .

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `New-AzureRmRedisCacheKey`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Για να δημιουργήσετε ξανά το πρωτεύον ή δευτερεύον κλειδί για το cache, καλέστε την `New-AzureRmRedisCacheKey` cmdlet και μεταβιβάζουν στο όνομά της, ομάδα πόρων, και καθορίστε είτε `Primary` ή `Secondary` για το `KeyType` παραμέτρου. Στο παρακάτω παράδειγμα, το κλειδί δευτερεύοντα πρόσβασης για ένα cache αναδημιουργείται.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Για να διαγράψετε ένα cache Redis

Για να διαγράψετε ένα cache Redis, χρησιμοποιήστε το cmdlet [Κατάργηση AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) .

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Remove-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Στο παρακάτω παράδειγμα, με το όνομα του cache `myCache` καταργείται.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Για να εισαγάγετε ένα cache Redis

Μπορείτε να εισαγάγετε δεδομένα σε μια παρουσία Azure Redis Cache χρησιμοποιώντας το `Import-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Εισαγωγή/εξαγωγή είναι διαθέσιμη μόνο για [premium επίπεδο](cache-premium-tier-intro.md) μνήμης cache. Για περισσότερες πληροφορίες σχετικά με την εισαγωγή/εξαγωγή, ανατρέξτε στο θέμα [Εισαγωγή και εξαγωγή δεδομένων στο Azure Redis Cache](cache-how-to-import-export-data.md).

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Import-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Η ακόλουθη εντολή εισάγει δεδομένα από το αντικείμενο blob που καθορίζεται από το uri συσχετισμών Ασφαλείας στο Azure Redis Cache.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Για να εξαγάγετε ένα cache Redis

Μπορείτε να εξαγάγετε δεδομένα από μια παρουσία Azure Redis Cache χρησιμοποιώντας το `Export-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Εισαγωγή/εξαγωγή είναι διαθέσιμη μόνο για [premium επίπεδο](cache-premium-tier-intro.md) μνήμης cache. Για περισσότερες πληροφορίες σχετικά με την εισαγωγή/εξαγωγή, ανατρέξτε στο θέμα [Εισαγωγή και εξαγωγή δεδομένων στο Azure Redis Cache](cache-how-to-import-export-data.md).

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Export-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Την ακόλουθη εντολή εξάγει δεδομένα από μια παρουσία Azure Redis Cache σε κοντέινερ που καθορίζεται από το uri συσχετισμών Ασφαλείας.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Να κάνετε επανεκκίνηση ένα cache Redis

Μπορείτε να πραγματοποιήσετε επανεκκίνηση σας χρησιμοποιώντας την παρουσία Azure Redis Cache του `Reset-AzureRmRedisCache` cmdlet.

>[AZURE.IMPORTANT] Επανεκκινήστε τον υπολογιστή είναι διαθέσιμη μόνο για [premium επίπεδο](cache-premium-tier-intro.md) μνήμης cache. Για περισσότερες πληροφορίες σχετικά με την επανεκκίνηση του cache, ανατρέξτε στο θέμα [Διαχείριση Cache - επανεκκίνηση](cache-administration.md#reboot).

Για να δείτε μια λίστα με τις διαθέσιμες παραμέτρους και τις περιγραφές τους για `Reset-AzureRmRedisCache`, εκτελέστε την ακόλουθη εντολή.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Την ακόλουθη εντολή επανεκκίνηση και στους δύο κόμβους της καθορισμένης μνήμης cache.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με τη χρήση του Windows PowerShell με το Azure, ανατρέξτε στους ακόλουθους πόρους:

- [Azure τεκμηρίωση cmdlet Redis Cache στο MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Cmdlet διαχείρισης πόρων του Azure](http://go.microsoft.com/fwlink/?LinkID=394765): Μάθετε πώς μπορείτε να χρησιμοποιήσετε τα cmdlet στη λειτουργική μονάδα AzureResourceManager.
- [Χρήση πόρου ομάδες για να διαχειριστείτε τους πόρους σας Azure](../resource-group-template-deploy-portal.md): Μάθετε πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τις ομάδες πόρων στην πύλη του Azure.
- [Azure ιστολογίου](http://blogs.msdn.com/windowsazure): Μάθετε περισσότερα σχετικά με τις νέες δυνατότητες του Azure.
- [Ιστολόγιο του Windows PowerShell](http://blogs.msdn.com/powershell): Μάθετε περισσότερα σχετικά με τις νέες δυνατότητες του Windows PowerShell.
- [", Δέσμες ενεργειών Guy!" Ιστολόγιο](http://blogs.technet.com/b/heyscriptingguy/): λήψη ρεαλιστικό συμβουλές και τεχνάσματα από την Κοινότητα του Windows PowerShell.

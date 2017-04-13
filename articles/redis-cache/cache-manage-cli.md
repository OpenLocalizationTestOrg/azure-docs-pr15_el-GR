<properties 
    pageTitle="Πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε Cache Redis Azure χρησιμοποιώντας το περιβάλλον γραμμής εντολών Azure (Azure CLI) | Microsoft Azure" 
    description="Μάθετε πώς να εγκαταστήσετε το Azure CLI σε οποιαδήποτε πλατφόρμα, πώς μπορείτε να χρησιμοποιήσετε για να συνδεθείτε στο λογαριασμό σας Azure και πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε ένα cache Redis από το Azure CLI." 
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
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε Cache Redis Azure χρησιμοποιώντας το περιβάλλον γραμμής εντολών Azure (Azure CLI)

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Το Azure CLI είναι ένας εξαιρετικός τρόπος για να διαχειριστείτε την υποδομή Azure σας από οποιαδήποτε πλατφόρμα. Αυτό το άρθρο σάς δείχνει πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε τις παρουσίες Azure Redis Cache χρησιμοποιώντας το CLI Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για να δημιουργήσετε και να διαχειριστείτε παρουσιών Azure Redis Cache χρησιμοποιώντας Azure CLI, πρέπει να ολοκληρώσετε τα παρακάτω βήματα.

-   Πρέπει να έχετε ένα λογαριασμό Azure. Εάν δεν έχετε, μπορείτε να δημιουργήσετε έναν [δωρεάν λογαριασμό](https://azure.microsoft.com/pricing/free-trial/) σε λίγα λεπτά.
-   [Εγκαταστήστε το Azure CLI](../xplat-cli-install.md).
-   Σύνδεση της εγκατάστασης του Azure CLI με έναν προσωπικό λογαριασμό Azure ή με έναν εταιρικό ή σχολικό λογαριασμό Azure και συνδέεστε από το Azure CLI χρησιμοποιώντας το `azure login` εντολή. Για να κατανοήσετε τις διαφορές και να επιλέξετε, ανατρέξτε στο θέμα [σύνδεση σε μια συνδρομή του Azure από το περιβάλλον γραμμής εντολών Azure (Azure CLI)](../xplat-cli-connect.md).
-   Πριν να εκτελέσετε οποιαδήποτε από τις παρακάτω εντολές, εναλλαγή του Azure CLI σε κατάσταση διαχειριστή πόρων, εκτελώντας το `azure config mode arm` εντολή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Ορισμός στη λειτουργία διαχείρισης πόρων Azure](../xplat-cli-azure-resource-manager.md#set-the-azure-resource-manager-mode).

## <a name="redis-cache-properties"></a>Redis Cache ιδιοτήτων

Οι ακόλουθες ιδιότητες χρησιμοποιούνται κατά τη δημιουργία και την ενημέρωση των παρουσιών Redis Cache.

| Ιδιότητα            | Μετάβαση                                  | Περιγραφή                                                                                                                                                                                                                                          |
|---------------------|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Όνομα                | -n,--όνομα                              | Όνομα της μνήμης Cache Redis.                                                                                                                                                                                                                             |
| ομάδα πόρων      | -g,--ομάδα πόρων                    | Το όνομα της ομάδας πόρων.                                                                                                                                                                                                                          |
| θέση            | -l,--θέση                          | Θέση για να δημιουργήσετε cache.                                                                                                                                                                                                                            |
| μέγεθος                | -z,--μέγεθος                              | Μέγεθος του Cache Redis. Έγκυρες τιμές: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]                                                                                                                                                                  |
| SKU                 | -x,--sku                               | Redis SKU. Πρέπει να είναι ένα από: [βασικές, τυπικό, Premium]                                                                                                                                                                                             |
| EnableNonSslPort    | -e,--ενεργοποίηση-μη--θύρα ssl               | Η ιδιότητα EnableNonSslPort του Redis Cache. Προσθήκη σημαίας σε αυτό, εάν θέλετε να ενεργοποιήσετε τη θύρα SSL μη για το cache                                                                                                                                    |
| Redis ρύθμισης παραμέτρων | -c,--redis-ρύθμιση παραμέτρων               | Redis ρύθμισης παραμέτρων. Εισαγάγετε μια συμβολοσειρά JSON μορφοποιημένο ρύθμισης παραμέτρων κλειδιών και τιμών εδώ. Μορφή: "{" ":"";" ":" "}"                                                                                                                                     |
| Redis ρύθμισης παραμέτρων | -f,--αρχείο ρύθμισης παραμέτρων redis          | Redis ρύθμισης παραμέτρων. Πληκτρολογήστε τη διαδρομή του αρχείου που περιέχει αριθμούς-κλειδιά ρύθμισης παραμέτρων και τιμές εδώ. Μορφοποίηση για την καταχώρηση αρχείου: {"": "";"": ""}                                                                                                                |
| Πλήθος shard         | -r,--shard count                       | Αριθμός Shards για να δημιουργήσετε σε μια Cache σύμπλεγμα Premium με σύμπλεγμα.                                                                                                                                                                               |
| Εικονικό δίκτυο     | -v,--εικονικού δικτύου                   | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει το ακριβές ARM Αναγνωριστικό πόρου του εικονικού δικτύου για να αναπτύξετε το cache redis στο. Παράδειγμα μορφή: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Τύπος κλειδιού            | -t,--τύπου κλειδιού                          | Τύπος για ανανέωση του αριθμού-κλειδιού. Έγκυρες τιμές: [κύρια, δευτερεύοντα]                                                                                                                                                                                             |
| StaticIP            | -p,--στατική ip < στατική-ip >             | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει μια μοναδική διεύθυνση IP στο δευτερεύον για το cache. Εάν δεν παρέχεται, θα επιλεγεί για εσάς από το υποδίκτυο.                                                                                                |
| Υποδίκτυο              | t,--υποδίκτυο<subnet>                    | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει το όνομα του υποδικτύου στην οποία θέλετε να αναπτύξετε το cache.                                                                                                                                                    |
| VirtualNetwork      | -v,--εικονικού δικτύου < εικονικού-δικτύου > | Όταν φιλοξενίας το cache σε μια VNET, καθορίζει το ακριβές ARM Αναγνωριστικό πόρου του εικονικού δικτύου για να αναπτύξετε το cache redis στο. Παράδειγμα μορφή: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Συνδρομή        | -s,--συνδρομή                      | Το αναγνωριστικό συνδρομής.                                                                                                                                                                                                                         |

## <a name="see-all-redis-cache-commands"></a>Δείτε όλες τις εντολές Redis Cache

Για να δείτε όλες τις εντολές Redis Cache και τις παραμέτρους, χρησιμοποιήστε το `azure rediscache -h` εντολή.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Δημιουργήστε ένα Cache Redis

Για να δημιουργήσετε ένα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache create -h` εντολή.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Διαγραφή μιας υπάρχουσας Cache Redis

Για να διαγράψετε ένα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache delete -h` εντολή.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Λίστα όλες οι cache Redis μέσα σε σας συνδρομή ή μια ομάδα πόρων

Για να παραθέσετε όλες οι cache Redis μέσα σε σας συνδρομή ή μια ομάδα πόρων, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache list [options]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache list -h` εντολή.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Εμφάνιση των ιδιοτήτων από μια υπάρχουσα Cache Redis

Για να εμφανιστούν οι ιδιότητες του μια υπάρχουσα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache show -h` εντολή.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>
## <a name="change-settings-of-an-existing-redis-cache"></a>Αλλαγή των ρυθμίσεων μιας υπάρχουσας Cache Redis

Για να αλλάξετε τις ρυθμίσεις από μια υπάρχουσα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache set -h` εντολή.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Ανανέωση του αριθμού-κλειδιού ελέγχου ταυτότητας για μια υπάρχουσα Cache Redis

Για να ανανεώσετε το κλειδί ελέγχου ταυτότητας για μια υπάρχουσα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Καθορισμός `Primary` ή `Secondary` για `key-type`.

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache renew-key -h` εντολή.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Λίστα κύριας και δευτερεύουσας πλήκτρα από μια υπάρχουσα Cache Redis

Για να λίστα κύριας και δευτερεύουσας πλήκτρα από μια υπάρχουσα Cache Redis, χρησιμοποιήστε την ακόλουθη εντολή:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Για περισσότερες πληροφορίες σχετικά με αυτή την εντολή, εκτελέστε το `azure rediscache list-keys -h` εντολή.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<properties
    pageTitle="Azure CLI εντολές σε λειτουργία διαχείρισης πόρων | Microsoft Azure"
    description="Εντολές Azure περιβάλλον γραμμής εντολών (CLI) για να διαχειριστείτε πόρους στο μοντέλο ανάπτυξης για τη διαχείριση πόρων"
    services="virtual-machines-linux,virtual-machines-windows,virtual-network,mobile-services,cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="danlep"/>

# <a name="azure-cli-commands-in-resource-manager-mode"></a>Azure CLI εντολές σε λειτουργία διαχείρισης πόρων

Σε αυτό το άρθρο παρέχει σύνταξη και τις επιλογές για τις εντολές Azure περιβάλλον γραμμής εντολών (CLI) που θα χρησιμοποιούσατε συνήθως για να δημιουργήσετε και να διαχειριστείτε Azure πόρους στο μοντέλο ανάπτυξης Azure διαχείριση πόρων. Μπορείτε να αποκτήσετε πρόσβαση αυτές οι εντολές, εκτελώντας το CLI στη λειτουργία διαχείρισης πόρων (arm). Δεν είναι ολοκληρωμένη αναφορά και την έκδοση CLI μπορεί να εμφανίζονται ελαφρώς διαφορετικές εντολές ή παράμετροι. Για μια γενική επισκόπηση του Azure τους πόρους και τις ομάδες πόρων, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md).  

Για να ξεκινήσετε, πρώτη [εγκατάσταση του Azure CLI](../xplat-cli-install.md) και [συνδεθείτε με τη συνδρομή σας στο Azure](../xplat-cli-connect.md) , χρησιμοποιώντας έναν εταιρικό ή σχολικό λογαριασμό ή μια ταυτότητα λογαριασμού Microsoft.

Για την τρέχουσα εντολή σύνταξη και τις επιλογές στη γραμμή εντολών σε κατάσταση λειτουργίας διαχειριστή πόρων, πληκτρολογήστε `azure help` ή, για να εμφανίσετε τη Βοήθεια για μια συγκεκριμένη εντολή, `azure help [command]`. Επίσης, βρείτε CLI παραδείγματα στην τεκμηρίωση για τη δημιουργία και τη διαχείριση συγκεκριμένων υπηρεσιών Azure.

Προαιρετικές παράμετροι εμφανίζονται σε αγκύλες (για παράδειγμα, `[parameter]`). Όλες οι υπόλοιπες παράμετροι απαιτούνται.

Εκτός από την εντολή συγκεκριμένο προαιρετικές παραμέτρους τεκμηριώνονται εδώ, υπάρχουν τρία προαιρετικές παράμετροι που μπορούν να χρησιμοποιηθούν για να εμφανίσετε λεπτομερείς εξόδου όπως επιλογές αίτηση και τους κωδικούς κατάστασης. Το `-v` παράμετρος παρέχει λεπτομερείς πληροφορίες, και το `-vv` παραμέτρου παρέχει ακόμα πιο λεπτομερή λεπτομερή δεδομένα εξόδου. Το `--json` επιλογή εξάγει το αποτέλεσμα σε μορφή ανεπεξέργαστα json.

## <a name="setting-the-resource-manager-mode"></a>Ρύθμιση της λειτουργίας της διαχείρισης πόρων

Χρησιμοποιήστε την παρακάτω εντολή για να ενεργοποιήσετε τις εντολές για τη διαχείριση πόρων CLI Azure κατάστασης.

    azure config mode arm

>[AZURE.NOTE] Το CLI Azure διαχείρισης πόρων και διαχείριση της υπηρεσίας Azure κατάστασης λειτουργίας είναι αμοιβαία αποκλειόμενα. Αυτό σημαίνει ότι, τους πόρους που έχουν δημιουργηθεί με μία λειτουργία δεν είναι δυνατό να γίνεται από την κατάσταση λειτουργίας.


## <a name="azure-account-manage-your-account-information"></a>λογαριασμός Azure: Διαχείριση των πληροφοριών του λογαριασμού σας
Πληροφορίες Azure τη συνδρομή σας χρησιμοποιείται από το εργαλείο για να συνδεθείτε στο λογαριασμό σας.

**Λίστα με τις συνδρομές που έχουν εισαχθεί**

    account list [options]

**Εμφάνιση λεπτομερειών σχετικά με μια συνδρομή**  

    account show [options] [subscriptionNameOrId]

**Ορίστε την τρέχουσα συνδρομή σας**

    account set [options] <subscriptionNameOrId>

**Κατάργηση συνδρομής ή περιβάλλον ή καταργήστε την επιλογή από όλες τις αποθηκευμένες πληροφορίες λογαριασμού και περιβάλλον**  

    account clear [options]

**Εντολές για τη διαχείριση του περιβάλλοντος του λογαριασμού**  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-to-display-active-directory-objects"></a>Azure ad: εντολές για να εμφανίσετε αντικείμενα της υπηρεσίας καταλόγου Active Directory

**Εντολές για την εμφάνιση των εφαρμογών υπηρεσίας καταλόγου active directory**

    ad app create [options]
    ad app delete [options] <object-id>

**Εντολές για να εμφανίσετε ομάδες της υπηρεσίας καταλόγου active directory**

    ad group list [options]
    ad group show [options]

**Εντολές για την παροχή μια κατάσταση υπηρεσίας καταλόγου active directory sub ομάδας ή μέλος**

    ad group member list [options] [objectId]

**Εντολές για να εμφανίσετε αρχές της υπηρεσίας καταλόγου active directory**

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

**Εντολές για να εμφανίσετε τους χρήστες της υπηρεσίας καταλόγου active directory**

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-to-manage-your-availability-sets"></a>Azure availset: εντολές για τη Διαχείριση συνόλων σας διαθεσιμότητα

**Δημιουργεί μια διαθεσιμότητα Ορισμός μέσα σε μια ομάδα πόρων**

    availset create [options] <resource-group> <name> <location> [tags]

**Παραθέτει τα σύνολα διαθεσιμότητα μέσα σε μια ομάδα πόρων**

    availset list [options] <resource-group>

**Λαμβάνει μία διαθεσιμότητα Ορισμός μέσα σε μια ομάδα πόρων**

    availset show [options] <resource-group> <name>

**Διαγράφει ένα διαθεσιμότητα Ορισμός μέσα σε μια ομάδα πόρων**

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-to-manage-your-local-settings"></a>Azure config: εντολές για να διαχειριστείτε τις τοπικές ρυθμίσεις

**Λίστα ρυθμίσεων παραμέτρων Azure CLI**

    config list [options]

**Διαγράψτε μια ρύθμιση παραμέτρων**

    config delete [options] <name>

**Ενημέρωση μιας ρύθμισης παραμέτρων**

    config set <name> <value>

**Ορίζει τη λειτουργία εργασίας Azure CLI είτε `arm` ή`asm`**

    config mode [options] <modename>


## <a name="azure-feature-commands-to-manage-account-features"></a>η δυνατότητα Azure: εντολές για τη Διαχείριση λογαριασμού δυνατότητες

**Λίστα με όλες τις δυνατότητες διαθέσιμες για τη συνδρομή σας**

    feature list [options]

**Εμφανίζει μια δυνατότητα**

    feature show [options] <providerName> <featureName>

**Καταχωρεί μια προεπισκόπηση δυνατότητα από μια υπηρεσία παροχής πόρων**

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-to-manage-your-resource-groups"></a>ομάδα Azure: εντολές για τη διαχείριση των ομάδων πόρων

**Δημιουργεί μια ομάδα πόρων**

    group create [options] <name> <location>

**Ορισμός ετικέτες σε μια ομάδα πόρων**

    group set [options] <name> <tags>

**Διαγράφει μια ομάδα πόρων**

    group delete [options] <name>

**Παραθέτει τις ομάδες πόρων για τη συνδρομή σας**

    group list [options]

**Εμφανίζει μια ομάδα πόρων για τη συνδρομή σας**

    group show [options] <name>

**Εντολές για να διαχειριστείτε αρχεία καταγραφής από την ομάδα πόρων**

    group log show [options] [name]

**Εντολές για τη διαχείριση του ανάπτυξη σε μια ομάδα πόρων**

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

**Εντολές για τη διαχείριση του προτύπου ομάδα πόρων τοπική ή τη συλλογή**

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-to-manage-your-hdinsight-clusters"></a>Azure hdinsight: εντολές για τη διαχείριση του HDInsight συμπλεγμάτων

**Εντολές για να δημιουργήσετε ή να προσθέσετε ένα αρχείο ρύθμισης παραμέτρων του συμπλέγματος**

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

Παράδειγμα: Δημιουργήστε ένα αρχείο ρύθμισης παραμέτρων που περιέχει μια ενέργεια δέσμη ενεργειών για να εκτελέσετε κατά τη δημιουργία ενός συμπλέγματος.

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

**Εντολή για να δημιουργήσετε ένα σύμπλεγμα σε μια ομάδα πόρων**

    hdinsight cluster create [options] <clusterName>

Παράδειγμα: Δημιουργία μιας καταιγίδας σε Linux συμπλέγματος

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

Παράδειγμα: Δημιουργήστε ένα σύμπλεγμα με ενέργεια δέσμης ενεργειών

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

Παράμετρος επιλογές:

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       The name of the resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for the cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url to use for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key to the storage account to use for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in the storage account to use for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for the cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes to use for the cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for the cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for the cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for the cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for the cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In the format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for the external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for the external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for the external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for the external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for the external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for the external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for the external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for the external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    The subscription id
    --tags <tags>                                              Tags to set to the cluster.
    Can be multiple.
    In the format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


**Εντολή για να διαγράψετε ένα σύμπλεγμα**

    hdinsight cluster delete [options] <clusterName>

**Εντολή για την εμφάνιση λεπτομερειών συμπλέγματος**

    hdinsight cluster show [options] <clusterName>

**Εντολή για να εμφανιστούν όλα συμπλεγμάτων (σε μια ομάδα συγκεκριμένο πόρο, εάν παρέχεται)**

    hdinsight cluster list [options]

**Εντολή για να αλλάξετε το μέγεθος ενός συμπλέγματος**

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

**Εντολή για να ενεργοποιήσετε την πρόσβαση μέσω HTTP για ένα σύμπλεγμα**

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

**Εντολή για να απενεργοποιήσετε την πρόσβαση μέσω HTTP για ένα σύμπλεγμα**

    hdinsight cluster disable-http-access [options] <clusterName>

**Εντολή για την ενεργοποίηση της πρόσβασης RDP για ένα σύμπλεγμα**

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

**Εντολή για να απενεργοποιήσετε την πρόσβαση μέσω HTTP για ένα σύμπλεγμα**

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-to-monitoring-insights-events-alert-rules-autoscale-settings-metrics"></a>Azure ιδέες: εντολές που αφορούν την παρακολούθηση ιδέες (συμβάντα, ειδοποίησης κανόνες, ρυθμίσεις autoscale, μετρικά)

**Ανάκτηση αρχείων καταγραφής λειτουργίας για μια συνδρομή, μια correlationId, μια ομάδα πόρων, πόρων ή υπηρεσίας παροχής πόρων**

    insights logs list [options]

## <a name="azure-location-commands-to-get-the-available-locations-for-all-resource-types"></a>Azure θέση: εντολές για να λάβετε τις διαθέσιμες θέσεις για όλους τους τύπους πόρων

**Λίστα με τις διαθέσιμες θέσεις**

    location list [options]

## <a name="azure-network-commands-to-manage-network-resources"></a>Azure δικτύου: εντολές για τη διαχείριση πόρων δικτύου

**Εντολές για τη διαχείριση εικονικών δικτύων**

    network vnet create [options] <resource-group> <name> <location>
Δημιουργεί ένα εικονικό δίκτυο. Στο παρακάτω παράδειγμα μπορούμε να δημιουργήσουμε ένα εικονικό δίκτυο με το όνομα newvnet για myresourcegroup ομάδα πόρων στην περιοχή Δυτική ΗΠΑ.


    azure network vnet create myresourcegroup newvnet "west us"
    info:    Executing command network vnet create
    + Looking up virtual network "newvnet"
    + Creating virtual network "newvnet"
     Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet create command OK


Παράμετρος επιλογές:

    -h, --help                                 output usage information
    -v, --verbose                              use verbose output
    --json                                     use json output
    -g, --resource-group <resource-group>      the name of the resource group
    -n, --name <name>                          the name of the virtual network
    -l, --location <location>                  the location
    -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            the comma separated list of DNS servers IP addresses
    -t, --tags <tags>                          the tags set on this virtual network.
      Can be multiple. In the format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

Ενημερώνει μια ρύθμιση παραμέτρων εικονικού δικτύου μέσα σε μια ομάδα πόρων.

    azure network vnet set myresourcegroup newvnet

    info:    Executing command network vnet set
    + Looking up virtual network "newvnet"
    + Updating virtual network "newvnet"
    + Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet set command OK

Παράμετρος επιλογές:

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      the name of the resource group
       -n, --name <name>                          the name of the virtual network
       -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended to the current list of address prefixes.
        The address prefixes in this list should not overlap between them.
        The address prefixes in this list should not overlap with existing address prefixes in the vnet.

       -d, --dns-servers [dns-servers]            the comma separated list of DNS servers IP addresses.
        This list will be appended to the current list of DNS server IP addresses.

       -t, --tags <tags>                          the tags set on this virtual network.
        Can be multiple. In the format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended to the current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet list [options] <resource-group>

Η εντολή εμφανίζει όλα τα δίκτυα εικονικού σε μια ομάδα πόρων.


    C:\>azure network vnet list myresourcegroup

    info:    Executing command network vnet list
    + Listing virtual networks
        data:    ID
       Name      Location  Address prefixes  DNS servers
    data:    -------------------------------------------------------------------
    ------  --------  --------  ----------------  -----------
    data:    /subscriptions/###############################/resourceGroups/
    wvnet   newvnet   westus    10.0.0.0/8
    info:    network vnet list command OK

Παράμετρος επιλογές:


      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  the name of the resource group
      -s, --subscription <subscription>      the subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
Η εντολή εμφανίζει τις ιδιότητες εικονικού δικτύου σε μια ομάδα πόρων.

    azure network vnet show -g myresourcegroup -n newvnet

    info:    Executing command network vnet show
    + Looking up virtual network "newvnet"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet show command OK
<BR>

    network vnet delete [options] <resource-group> <name>
Η εντολή καταργεί ένα εικονικό δίκτυο.

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

Παράμετρος επιλογές:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier


**Εντολές για τη Διαχείριση εικονικού δικτύου δευτερεύοντα δίκτυα**

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

Προσθέτει άλλη υποδικτύου σε ένα υπάρχον εικονικό δίκτυο.

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up the subnet "subnet"
    + Creating subnet "subnet"
    + Looking up the subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

Παράμετρος επιλογές:

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            the name of the resource group
     -e, --vnet-name <vnet-name>                                      the name of the virtual network
     -n, --name <name>                                                the name of the subnet
     -a, --address-prefix <address-prefix>                            the address prefix
     -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  the network security group name
     -s, --subscription <subscription>                                the subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

Ορίζει ένα συγκεκριμένο εικονικού δικτύου υποδίκτυο μέσα σε μια ομάδα πόρων.


    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

Παραθέτει σε λίστα όλα τα δευτερεύοντα δίκτυα εικονικού δικτύου για ένα συγκεκριμένο εικονικό δίκτυο μέσα σε μια ομάδα πόρων.

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
Εμφανίζει τις ιδιότητες υποδικτύου εικονικού δικτύου

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -e, --vnet-name <vnet-name>            the name of the virtual network
    -n, --name <name>                      the name of the subnet
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
Καταργεί ένα υποδίκτυο από μια υπάρχουσα εικονικού δικτύου.

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up the subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -e, --vnet-name <vnet-name>            the name of the virtual network
    -n, --name <name>                      the subnet name
    -s, --subscription <subscription>      the subscription identifier
    -q, --quiet                            quiet mode, do not ask for delete confirmation

**Εντολές για τη Διαχείριση balancers φόρτωσης**

    network lb create [options] <resource-group> <name> <location>
Δημιουργεί ένα σύνολο εξισορρόπησης φόρτου.

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up the load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -l, --location <location>              the location
    -t, --tags <tags>                      the list of tags.
     Can be multiple. In the format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb list [options] <resource-group>
Παραθέτει τους πόρους εξισορρόπησης φόρτου μέσα σε μια ομάδα πόρων.

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting the load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

Εμφανίζει φόρτωση εξισορρόπησης πληροφορίες από μια συγκεκριμένη εξισορρόπηση φόρτου μέσα σε μια ομάδα πόρων

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

Διαγραφή πόρους εξισορρόπησης φόρτου.

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up the load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Εντολές για τη Διαχείριση καθετήρες από μια μονάδα εξισορρόπησης φόρτου**

    network lb probe create [options] <resource-group> <lb-name> <name>

Δημιουργήστε τη ρύθμιση παραμέτρων δοκιμή του για την κατάσταση εύρυθμης λειτουργίας στο τη μονάδα εξισορρόπησης φόρτου. Έχετε υπόψη σας για να εκτελέσετε αυτήν την εντολή, την εξισορρόπηση φόρτου απαιτεί έναν πόρο frontend ip (ο έλεγχος εντολής "frontend-ip azure δικτύου" για να αντιστοιχίσετε μια διεύθυνση ip για μονάδα εξισορρόπησης φόρτου).

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -p, --protocol <protocol>              the probe protocol
    -o, --port <port>                      the probe port
    -f, --path <path>                      the probe path
    -i, --interval <interval>              the probe interval in seconds
    -c, --count <count>                    the number of probes
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

Ενημερώνει μια υπάρχουσα δοκιμή του εξισορρόπησης φόρτου με νέες τιμές για αυτήν.

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

Επιλογές παραμέτρου

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -e, --new-probe-name <new-probe-name>  the new name of the probe
    -p, --protocol <protocol>              the new value for probe protocol
    -o, --port <port>                      the new value for probe port
    -f, --path <path>                      the new value for probe path
    -i, --interval <interval>              the new value for probe interval in seconds
    -c, --count <count>                    the new value for number of probes
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

Λίστα δοκιμή του ιδιοτήτων για ένα σύνολο εξισορρόπησης φόρτου.

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up the load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
Καταργεί τη δοκιμή του που έχει δημιουργηθεί για τη μονάδα εξισορρόπησης φόρτου.

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up the load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

**Εντολές για τη Διαχείριση ρυθμίσεων ip frontend από μια μονάδα εξισορρόπησης φόρτου**

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
Δημιουργεί μια ρύθμιση IP frontend σε ένα υπάρχον σύνολο εξισορρόπησης φόρτου.

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up the load balancer "mylb"
    + Looking up the subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    /frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:           10.0.1.4
    data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Public IP address:
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip create command OK

<BR>

    network lb frontend-ip set [options] <resource-group> <lb-name> <name>

Ενημερώνει μια υπάρχουσα ρύθμιση παραμέτρων για μια frontend IP. Η παρακάτω εντολή προσθέτει μια δημόσια IP που ονομάζεται mypubip5 σε μια υπάρχουσα φόρτωσης εξισορρόπησης frontend IP με το όνομα myfrontendip.

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up the load balancer "mylb"
    + Looking up the public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:
    data:    Subnet:
    data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip set command OK

Παράμετρος επιλογές:

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              the name of the resource group
    -l, --lb-name <lb-name>                                            the name of the load balancer
    -n, --name <name>                                                  the name of the frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      the private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  the private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  the public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              the public ip name.
    This public ip must exist in the same resource group as the lb.
    Please use public-ip-id if that is not the case.
    -b, --subnet-id <subnet-id>                                        the subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    the subnet name
    -m, --vnet-name <vnet-name>                                        the virtual network name.
    This virtual network must exist in the same resource group as the lb.
    Please use subnet-id if that is not the case.
    -s, --subscription <subscription>                                  the subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

Παραθέτει όλους τους πόρους IP frontend έχει ρυθμιστεί για την εξισορρόπηση φόρτου.

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
Διαγράφει το αντικείμενο IP frontend που έχει συσχετιστεί με μονάδα εξισορρόπησης φόρτου

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up the load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Εντολές για τη Διαχείριση συνόλων διεύθυνση παρασκηνίου από μια μονάδα εξισορρόπησης φόρτου**

    network lb address-pool create [options] <resource-group> <lb-name> <name>

Δημιουργία χώρου συγκέντρωσης διεύθυνση παρασκηνίου για μια μονάδα εξισορρόπησης φόρτου.

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool add [options] <resource-group> <lb-name> <name>

Μια περιοχή χώρου συγκέντρωσης διευθύνσεων παρασκηνίου είναι πώς μια μονάδα εξισορρόπησης φόρτου γνωρίζει τους πόρους για να δρομολογήσετε την εισερχόμενη κίνηση δικτύου από το τελικό σημείο του με χρήση της διαχείρισης πόρων Azure. Αφού δημιουργήσετε και δώστε ένα όνομα στην περιοχή χώρος συγκέντρωσης διευθύνσεων παρασκηνίου (δείτε εντολή "azure δικτύου λίβρες διεύθυνση χώρου συγκέντρωσης Δημιουργία"), πρέπει να προσθέσετε τα τελικά σημεία, οι οποίες καθορίζονται τώρα από έναν πόρο που ονομάζεται "δικτύου διασυνδέσεων".

Για να ρυθμίσετε τις παραμέτρους της περιοχής διευθύνσεων παρασκηνίου, χρειάζεστε τουλάχιστον ένα "διασύνδεση δικτύου" (ανατρέξτε στο θέμα "nic λίβρες azure δικτύου" γραμμή εντολών για περισσότερες λεπτομέρειες).

Στο παρακάτω παράδειγμα, ένα περιβάλλον εργασίας δικτύου που δημιουργήσατε προηγουμένως "nic1" που χρησιμοποιήθηκε για τη δημιουργία της περιοχής παρασκηνίου διεύθυνση χώρου συγκέντρωσης.

    azure network lb address-pool add -g myresourcegroup -l mylb -n mybackendpool -a nic1

    info:    Executing command network lb address-pool add
    + Looking up the load balancer "mylb"
    + Getting network interfaces
    + Updating network interface "nic1"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Name:                      mybackendpool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:     id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/networkInterfaces/nic1/ipConfigurations/NIC-config
    data:    Load balancing rules:
    data:
    info:    network lb address-pool add command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -i, --vm-id <vm-id>                    the virtual machine identifier.
    e.g. "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>,/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>"
    -m, --vm-name <vm-name>                the name of the virtual machine
    -d, --nic-id <nic-id>                  the network interface identifier.
    e.g. ""/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkInterfaces/<nic-name>"
    -a, --nic-name <nic-name>              the name of the network interface
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool remove [options] <resource-group> <lb-name> <name>

Καταργεί ένα περιβάλλον εργασίας δικτύου από το χώρο συγκέντρωσης διεύθυνση IP υπολογιστή στο παρασκήνιο.

    azure network lb address-pool remove -g myresourcegroup -l mylb -n mybackendpool -a nic1

    info:    Executing command network lb address-pool remove
    + Looking up the load balancer "mylb"
    + Getting network interfaces
    + Updating network interface "nic1"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Name:                      mybackendpool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool remove command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -i, --vm-id <vm-id>                    the virtual machine identifier.
    e.g. "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>,/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>"
    -m, --vm-name <vm-name>                the name of the virtual machine
    -d, --nic-id <nic-id>                  the network interface identifier.
    e.g. ""/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkInterfaces/<nic-name>"
    -a, --nic-name <nic-name>              the name of the network interface
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

Λίστα παρασκηνίου χώρου συγκέντρωσης η περιοχή διευθύνσεων IP για το συγκεκριμένο πόρο ομάδας

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up the load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

<BR>
[επιλογές] <-ομάδα πόρων >< λίβρες-όνομα > Διαγραφή σύνολο διευθύνσεων λίβρες δικτύου<name>

Καταργεί τον πόρο περιοχή παρασκηνίου IP χώρου συγκέντρωσης από εξισορρόπηση φόρτου.

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up the load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Εντολές για τη Διαχείριση φόρτωση εξισορρόπησης κανόνων**

    network lb rule create [options] <resource-group> <lb-name> <name>
Δημιουργήστε κανόνες εξισορρόπησης φόρτου.

Μπορείτε να δημιουργήσετε έναν κανόνα εξισορρόπησης φόρτου τη ρύθμιση των παραμέτρων του τελικού σημείου frontend για την εξισορρόπηση φόρτου και στην περιοχή χώρος συγκέντρωσης διευθύνσεων παρασκηνίου για να λάβετε την εισερχόμενη κίνηση δικτύου. Ρυθμίσεις περιλαμβάνουν επίσης τις θύρες για το τελικό frontend IP και θύρες για την περιοχή χώρου συγκέντρωσης διεύθυνση παρασκηνίου.

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε έναν κανόνα εξισορρόπησης φόρτου, το τελικό σημείο frontend ακρόαση για τη θύρα 80 TCP και φόρτωση αποστολή στη θύρα 8080 για την περιοχή παρασκηνίου διεύθυνση χώρου συγκέντρωσης εξισορρόπησης κίνηση του δικτύου.

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
    data:    Name:                      mylbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule create command OK

<BR>

    network lb rule set [options] <resource-group> <lb-name> <name>

Ενημερώνει έναν υπάρχοντα κανόνα εξισορρόπησης φόρτου ρύθμιση σε μια ομάδα συγκεκριμένο πόρο. Στο παρακάτω παράδειγμα, θα σας αλλάξει το όνομα του κανόνα από mylbrule σε mynewlbrule.

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
    data:    Name:                      mynewlbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule set command OK

Παράμετρος επιλογές:

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              the name of the resource group
    -l, --lb-name <lb-name>                            the name of the load balancer
    -n, --name <name>                                  the name of the rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          the rule protocol
    -f, --frontend-port <frontend-port>                the frontend port
    -b, --backend-port <backend-port>                  the backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  the idle timeout in minutes
    -a, --probe-name [probe-name]                      the name of the probe defined in the same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          the name of the frontend ip configuration in the same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of the backend address pool defined in the same load balancer
    -s, --subscription <subscription>                  the subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

Παραθέτει σε λίστα όλα φόρτωση εξισορρόπησης κανόνες που έχει ρυθμιστεί για μια μονάδα εξισορρόπησης φόρτου σε μια ομάδα συγκεκριμένο πόρο.

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

Διαγραφή κανόνα εξισορρόπησης φόρτου.

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up the load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Εντολές για τη Διαχείριση εξισορρόπηση φόρτου NAT κανόνες εισερχομένων**

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
Δημιουργεί ένα κανόνα εισερχομένων NAT για εξισορρόπηση φόρτου.

Στο παρακάτω παράδειγμα που δημιουργήσαμε κανόνα NAT από frontend IP (το οποίο έχει προηγουμένως καθοριστεί χρησιμοποιώντας την εντολή "frontend azure δικτύου-ip") με μια εισερχόμενη θύρα ακρόασης και θύρα εξόδου που χρησιμοποιεί τη μονάδα εξισορρόπησης φόρτου για να στείλετε την κίνηση δικτύου.


    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              80
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule create command OK

Παράμετρος επιλογές:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id <vm-id>                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.This VM must exist in the same resource group as the lb.
    Please use vm-id if that is not the case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
Ενημερώνει έναν υπάρχοντα κανόνα εισερχομένων nat. Στο παρακάτω παράδειγμα, θα σας αλλάζουν τη θύρα εισερχομένων ακρόασης από 80 σε 81.

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              81
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule set command OK

Παράμετρος επιλογές:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id [vm-id]                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.
    This virtual machine must exist in the same resource group as the lb.
    Please use vm-id if that is not the case
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

Παραθέτει όλους τους κανόνες εισερχομένων nat για εξισορρόπηση φόρτου.

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up the load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

Διαγραφή κανόνα NAT για τη μονάδα εξισορρόπησης φόρτου σε μια ομάδα συγκεκριμένο πόρο.

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up the load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Εντολές για τη διαχείριση δημόσιων διευθύνσεων IP**

    network public-ip create [options] <resource-group> <name> <location>
Δημιουργεί έναν πόρο δημόσια ip. Θα δημιουργήσετε τον πόρο δημόσια ip και θα συσχετίσετε με ένα όνομα τομέα.

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up the public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Dynamic
    data:    Idle timeout:         4
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip create command OK


Παράμετρος επιλογές:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -l, --location <location>                    the location
    -d, --domain-name-label <domain-name-label>  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            the subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
Ενημερώνει τις ιδιότητες ενός υπάρχοντος πόρου δημόσια ip. Στο παρακάτω παράδειγμα θα σας αλλάξει στη δημόσια διεύθυνση IP από δυναμική σε στατικό.

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up the public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip set command OK

Παράμετρος επιλογές:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -d, --domain-name-label [domain-name-label]  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            the subscription identifier

<br>
λίστα δημόσια ip δικτύου [επιλογές] < ομάδα πόρων > παραθέτει όλες δημόσια πόρους IP μέσα σε μια ομάδα πόρων.

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting the public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>
Εμφάνιση δικτύου δημόσιας-ip [επιλογές] < ομάδα πόρων ><name>

Εμφανίζει τις ιδιότητες δημόσια ip για έναν πόρο δημόσια ip μέσα σε μια ομάδα πόρων.

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
    data:    Name:                 mytestpublicip
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip show command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -s, --subscription <subscription>      the subscription identifier


    network public-ip delete [options] <resource-group> <name>

Διαγραφή πόρου δημόσια ip.

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up the public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

Παράμετρος επιλογές:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier


**Εντολές για τη διαχείριση διασυνδέσεων δικτύου**

    network nic create [options] <resource-group> <name> <location>
Δημιουργεί έναν πόρο που ονομάζεται διασύνδεση δικτύου (NIC) τα οποία μπορούν να χρησιμοποιηθούν για balancers φόρτωσης ή να συσχετίσετε με ένα εικονικό μηχάνημα.

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up the network interface "testnic1"
    + Looking up the subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up the network interface "testnic1"
    data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
    data:    Name:                   testnic1
    data:    Type:                   Microsoft.Network/networkInterfaces
    data:    Location:               eastus
    data:    Provisioning state:     Succeeded
    data:    IP configurations:
    data:       Name:                         NIC-config
    data:       Provisioning state:           Succeeded
    data:       Private IP address:           10.0.0.5
    data:       Private IP Allocation Method: Dynamic
    data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1

Παράμετρος επιλογές:

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            the name of the resource group
    -n, --name <name>                                                the name of the network interface
    -l, --location <location>                                        the location
    -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  the network security group name.
    This network security group must exist in the same resource group as the nic.
    Please use network-security-group-id if that is not the case.
    -i, --public-ip-id <public-ip-id>                                the public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            the public IP name.
    This public ip must exist in the same resource group as the nic.
    Please use public-ip-id if that is not the case.
    -a, --private-ip-address <private-ip-address>                    the private IP address
    -u, --subnet-id <subnet-id>                                      the subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  the subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        the vnet name under which subnet-name exists
    -t, --tags <tags>                                                the comma seperated list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                the subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

**Εντολές για τη διαχείριση δικτύου ομάδες ασφαλείας**

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

**Εντολές για τη διαχείριση δικτύου κανόνες ομάδας ασφαλείας**

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

**Εντολές για τη Διαχείριση προφίλ διαχείρισης κίνηση**

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

**Εντολές για τη Διαχείριση τελικά σημεία manager κίνηση**

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

**Εντολές για τη Διαχείριση εικονικού δικτύου πυλών**

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-to-manage-resource-provider-registrations"></a>υπηρεσία παροχής Azure: εντολές για τη διαχείριση καταχωρήσεων παροχής πόρων

**Λίστα αυτήν τη στιγμή που έχουν καταχωρηθεί υπηρεσίες παροχής στη Διαχείριση πόρων**

    provider list [options]

**Εμφάνιση λεπτομερειών σχετικά με το χώρο ονομάτων ζητήθηκε υπηρεσία παροχής**

    provider show [options] <namespace>

**Καταχώρηση υπηρεσίας παροχής με τη συνδρομή**

    provider register [options] <namespace>

**Κατάργηση της καταχώρησης της υπηρεσίας παροχής με τη συνδρομή**

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-to-manage-your-resources"></a>Azure πόρων: εντολές για να διαχειριστείτε τους πόρους σας

**Δημιουργεί έναν πόρο σε μια ομάδα πόρων**

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

**Ενημερώνει έναν πόρο σε μια ομάδα πόρων χωρίς πρότυπα ή παραμέτρους**

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

**Παραθέτει τους πόρους**

    resource list [options] [resource-group]

**Λαμβάνει έναν πόρο μέσα σε μια ομάδα πόρων ή συνδρομή**

    resource show [options] <resource-group> <name> <resource-type> <api-version>

**Διαγραφή ενός πόρου σε μια ομάδα πόρων**

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-to-manage-your-azure-roles"></a>Azure ρόλο: εντολές για τη διαχείριση του Azure ρόλων

**Λήψη όλων των ορισμών διαθέσιμο ρόλο**

    role list [options]

**Λήψη ενός ορισμού διαθέσιμο ρόλο**

    role show [options] [name]

**Εντολές για να διαχειριστείτε την εκχώρηση ρόλων**

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-to-manage-your-storage-objects"></a>Azure χώρου αποθήκευσης: εντολές για τη διαχείριση του χώρου αποθήκευσης αντικειμένων

**Εντολές για να διαχειριστείτε τους λογαριασμούς σας στο χώρο αποθήκευσης**

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

**Εντολές για τη διαχείριση των αριθμών-κλειδιών λογαριασμού χώρου αποθήκευσης**

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

**Εντολές για να εμφανίσετε το χώρο αποθήκευσης συμβολοσειρά σύνδεσης**

    storage account connectionstring show [options] <name>

**Εντολές για να διαχειριστείτε τους χώρους αποθήκευσης**

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

**Πρόσβαση σε εντολές για τη Διαχείριση κοινόχρηστων υπογραφών από το κοντέινερ χώρου αποθήκευσης**

    storage container sas create [options] [container] [permissions] [expiry]

**Πρόσβαση σε εντολές για τη Διαχείριση αποθηκευμένες πολιτικές από το κοντέινερ χώρου αποθήκευσης**

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

**Εντολές για τη διαχείριση του χώρου αποθήκευσης αντικειμένων blob**

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

**Εντολές για τη διαχείριση του blob αντιγραφή λειτουργίες**

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

**Πρόσβαση σε εντολές για τη Διαχείριση κοινόχρηστων υπογραφής από το χώρο αποθήκευσης αντικειμένων blob**

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

**Εντολές για τη διαχείριση του χώρου αποθήκευσης κοινόχρηστα στοιχεία αρχείων**

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

**Εντολές για τη διαχείριση των αρχείων σας χώρου αποθήκευσης**

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

**Εντολές για τη διαχείριση του καταλόγου σας χώρο αποθήκευσης αρχείων**

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

**Εντολές για τη διαχείριση του χώρου αποθήκευσης ουρές**

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

**Πρόσβαση σε εντολές για τη Διαχείριση κοινόχρηστων υπογραφές σας ουράς χώρου αποθήκευσης**

    storage queue sas create [options] [queue] [permissions] [expiry]

**Εντολές για τη Διαχείριση αποθηκευμένες πρόσβαση πολιτικές σας ουράς χώρου αποθήκευσης**

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

**Εντολές για τη διαχείριση του χώρου αποθήκευσης ιδιοτήτων καταγραφής**

    storage logging show [options]
    storage logging set [options]

**Εντολές για να διαχειριστείτε τις ιδιότητες μετρικά αποθήκευσης**

    storage metrics show [options]
    storage metrics set [options]

**Εντολές για τη διαχείριση του χώρου αποθήκευσης πινάκων**

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

**Πρόσβαση σε εντολές για τη Διαχείριση κοινόχρηστων υπογραφές του πίνακα χώρου αποθήκευσης**

    storage table sas create [options] [table] [permissions] [expiry]

**Πρόσβαση σε εντολές για τη Διαχείριση αποθηκευμένες πολιτικές του πίνακα χώρου αποθήκευσης**

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-to-manage-your-resource-manager-tag"></a>Azure ετικέτα: εντολές για να διαχειριστείτε την ετικέτα διαχείρισης πόρων

**Προσθέστε μια ετικέτα**

    tag create [options] <name> <value>

**Κατάργηση ενός ολόκληρου ετικέτας ή μια τιμή ετικέτας**

    tag delete [options] <name> <value>

**Παραθέτει τις πληροφορίες ετικέτας**

    tag list [options]

**Λάβετε μια ετικέτα**

    tag show [options] [name]

## <a name="azure-vm-commands-to-manage-your-azure-virtual-machines"></a>εικονική Azure: εντολές για τη διαχείριση του εικονικές μηχανές Windows Azure

**Δημιουργήστε μια εικονική Μηχανή**

    vm create [options] <resource-group> <name> <location> <os-type>

**Δημιουργήστε μια Εικονική με προεπιλεγμένη πόροι**

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password
    
>[AZURE.TIP]Ξεκινώντας με CLI έκδοση 0.10, μπορείτε να παρέχετε ένα σύντομο ψευδώνυμο όπως "UbuntuLTS" ή "Win2012R2Datacenter" ως το `image-urn` για ορισμένες δημοφιλείς εικόνες Marketplace. Εκτέλεση `azure help vm quick-create` για τις επιλογές. Επιπλέον, ξεκινώντας από την έκδοση 0.10, `azure vm quick-create` χρησιμοποιεί χώρο αποθήκευσης premium από προεπιλογή, εάν είναι διαθέσιμη στην επιλεγμένη περιοχή.

**Λίστα τις εικονικές μηχανές μέσα σε ένα λογαριασμό**

    vm list [options]

**Λάβετε ένα εικονικό μηχάνημα μέσα σε μια ομάδα πόρων**

    vm show [options] <resource-group> <name>

**Διαγράψτε ένα εικονικό μηχάνημα μέσα σε μια ομάδα πόρων**

    vm delete [options] <resource-group> <name>

**Τερματισμός μία εικονική μηχανή μέσα σε μια ομάδα πόρων**

    vm stop [options] <resource-group> <name>

**Επανεκκινήστε μία εικονική μηχανή μέσα σε μια ομάδα πόρων**

    vm restart [options] <resource-group> <name>

**Ξεκινήστε μια εικονική μηχανή μέσα σε μια ομάδα πόρων**

    vm start [options] <resource-group> <name>

**Τερματισμός μία εικονική μηχανή μέσα σε μια ομάδα πόρων και κυκλοφορίες τους πόρους υπολογισμού**

    vm deallocate [options] <resource-group> <name>

**Λίστα διαθέσιμες εικονική μηχανή μεγέθη**

    vm sizes [options]

**Καταγραφή η Εικονική ως εικόνα του λειτουργικού Συστήματος ή Εικονική**

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

**Ορίστε την κατάσταση του η Εικονική σε Generalized**

    vm generalize [options] <resource-group> <name>

**Αποκτήστε την παρουσία προβολή την εικονική Μηχανή**

    vm get-instance-view [options] <resource-group> <name>

**Δίνουν τη δυνατότητα να επαναφέρετε τις ρυθμίσεις πρόσβαση απομακρυσμένης επιφάνειας εργασίας ή SSH σε εικονικό μηχάνημα και για να επαναφέρετε τον κωδικό πρόσβασης για το λογαριασμό που έχει διαχειριστή ή sudo αρχή έκδοσης πιστοποιητικών**

    vm reset-access [options] <resource-group> <name>

**Ενημέρωση Εικονική με νέα δεδομένα**

    vm set [options] <resource-group> <name>

**Εντολές για τη διαχείριση δεδομένων δίσκων σας εικονική μηχανή**

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

**Εντολές για τη Διαχείριση Εικονική επεκτάσεις πόρων**

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

**Εντολές για τη διαχείριση του Docker εικονική μηχανή**

    vm docker create [options] <resource-group> <name> <location> <os-type>

**Εντολές για τη Διαχείριση Εικονική εικόνες**

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]

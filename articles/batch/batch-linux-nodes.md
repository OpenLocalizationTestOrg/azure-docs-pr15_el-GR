<properties
    pageTitle="Κόμβοι Linux σε χώρους συγκέντρωσης δέσμη Azure | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επεξεργαστείτε το φόρτο εργασίας παράλληλες υπολογισμού στους χώρους συγκέντρωσης του Linux εικονικές μηχανές σε δέσμη Azure."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Προμήθεια κόμβους υπολογιστικών Linux σε χώρους συγκέντρωσης δέσμη Azure

Μπορείτε να χρησιμοποιήσετε δέσμη Azure να εκτελέσετε φόρτους εργασίας παράλληλες υπολογισμού σε εικονικές μηχανές Linux και Windows. Αυτό το άρθρο περιγράφει πώς μπορείτε να δημιουργήσετε χώρους συγκέντρωσης του κόμβους υπολογιστικών Linux στην υπηρεσία δέσμη με τη χρήση τόσο τη [Δέσμη Python] [ py_batch_package] και [.NET δέσμη] [ api_net] βιβλιοθήκες προγράμματος-πελάτη.

> [AZURE.NOTE] [Πακέτων εφαρμογών](batch-application-packages.md) δεν υποστηρίζονται αυτήν τη στιγμή σε κόμβους υπολογιστικών Linux.

## <a name="virtual-machine-configuration"></a>Ρύθμιση παραμέτρων εικονική μηχανή

Όταν δημιουργείτε ένα χώρο συγκέντρωσης κόμβους υπολογιστικών στη δέσμη, έχετε δύο επιλογές από την οποία για να επιλέξετε το μέγεθος κόμβου και το λειτουργικό σύστημα: ρύθμιση παραμέτρων των υπηρεσιών Cloud και τη ρύθμιση παραμέτρων εικονική μηχανή.

**Ρύθμιση παραμέτρων των υπηρεσιών cloud** παρέχει υπολογιστική κόμβους *μόνο*των Windows. Διαθέσιμες υπολογισμού κόμβο μεγέθη παρατίθενται σε [μεγέθη για τις υπηρεσίες Cloud](../cloud-services/cloud-services-sizes-specs.md)και διαθέσιμων λειτουργικών συστημάτων παρατίθενται στις [εκδόσεις OS επισκέπτη Azure και SDK συμβατότητα μήτρα](../cloud-services/cloud-services-guestos-update-matrix.md). Όταν δημιουργείτε ένα χώρο συγκέντρωσης που περιέχει τις υπηρεσίες Cloud Azure κόμβους, πρέπει να προσδιορίσετε μόνο το μέγεθος του κόμβου και την "OS η οικογένειά," που βρίσκονται στα άρθρα παραπάνω. Για τα σύνολα των Windows τον υπολογισμό κόμβους, τις υπηρεσίες Cloud χρησιμοποιείται πιο συχνά.

**Ρύθμιση παραμέτρων εικονική μηχανή** παρέχει εικόνες Linux και Windows για κόμβους υπολογιστικών. Διαθέσιμες υπολογισμού κόμβο μεγέθη παρατίθενται στον [μεγέθη για εικονικές μηχανές στο Azure](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) και τα [μεγέθη για εικονικές μηχανές στο Azure](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Όταν δημιουργείτε ένα χώρο συγκέντρωσης που περιέχει τους κόμβους εικονική μηχανή ρύθμισης παραμέτρων, πρέπει να καθορίσετε το μέγεθος του τους κόμβους, η αναφορά εικόνα εικονική μηχανή και τον παράγοντα κόμβο δέσμη SKU στον τους κόμβους.

### <a name="virtual-machine-image-reference"></a>Εικονική μηχανή αναφοράς εικόνας

Η υπηρεσία δέσμης χρησιμοποιεί [κλίμακα εικονική μηχανή σύνολα](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) για την παροχή κόμβους υπολογιστικών Linux. Οι εικόνες λειτουργικό σύστημα για αυτές τις εικονικές μηχανές παρέχονται από το [Azure Marketplace][vm_marketplace]. Όταν ρυθμίζετε μια εικονική μηχανή αναφοράς εικόνας, μπορείτε να καθορίσετε τις ιδιότητες μιας εικόνας εικονική μηχανή Marketplace. Όταν δημιουργείτε μια αναφορά εικόνα εικονική μηχανή απαιτούνται οι ακόλουθες ιδιότητες:

| **Ιδιότητες αναφοράς εικόνας** | **Παράδειγμα** |
| ----------------- | ------------------------ |
| Ο Publisher         | Κανονική                |
| Προσφορά             | UbuntuServer             |
| SKU               | 14.04.4-LTS              |
| Έκδοση           | πιο πρόσφατη                   |

> [AZURE.TIP] Μπορείτε να μάθετε περισσότερα σχετικά με αυτές τις ιδιότητες και πώς μπορείτε να λίστα Marketplace εικόνες στην [Περιήγηση και επιλογή Linux εικονική μηχανή εικόνων στο Azure με CLI ή PowerShell](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md). Σημειώστε ότι δεν όλες τις εικόνες Marketplace είναι αυτήν τη στιγμή συμβατό με τη μαζική. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [κόμβο παράγοντας SKU](#node-agent-sku).

### <a name="node-agent-sku"></a>Παράγοντας κόμβο SKU

Ο παράγοντας κόμβο δέσμης είναι ένα πρόγραμμα που εκτελείται σε κάθε κόμβο στο χώρο συγκέντρωσης και παρέχει το περιβάλλον εργασίας εντολών και ελέγχου μεταξύ τον κόμβο και την υπηρεσία δέσμης. Υπάρχουν διαφορετικές υλοποιήσεις του παράγοντα κόμβου, γνωστό ως SKU, για διαφορετικά λειτουργικά συστήματα. Ουσιαστικά, όταν δημιουργείτε μια ρύθμιση παραμέτρων εικονική μηχανή, μπορείτε πρώτα να καθορίσετε την αναφορά εικονική μηχανή εικόνα και, στη συνέχεια, μπορείτε να καθορίσετε τον παράγοντα κόμβο για να εγκαταστήσετε την εικόνα. Συνήθως, κάθε παράγοντα κόμβο SKU είναι συμβατή με πολλές εικόνες εικονική μηχανή. Ακολουθούν μερικά παραδείγματα κόμβο παράγοντας SKU:

* Batch.node.ubuntu 14.04
* Batch.node.centos 7
* Batch.node.Windows amd64

> [AZURE.IMPORTANT] Όχι όλες τις εικόνες εικονική μηχανή που είναι διαθέσιμες στην αγορά είναι συμβατά με το αυτήν τη στιγμή διαθέσιμα παράγοντες κόμβο δέσμης. Πρέπει να χρησιμοποιήσετε το SDK δέσμη για να παραθέσετε τον παράγοντα διαθέσιμη κόμβο SKU και οι εικόνες εικονική μηχανή με τα οποία είναι συμβατά. Δείτε τη [λίστα των εικονική μηχανή εικόνες](#list-of-virtual-machine-images) αργότερα σε αυτό το άρθρο για περισσότερες πληροφορίες.

## <a name="create-a-linux-pool-batch-python"></a>Δημιουργία χώρου συγκέντρωσης Linux: Python δέσμης

Το παρακάτω τμήμα κώδικα παρουσιάζει ένα παράδειγμα του τρόπου χρήσης του [Microsoft Azure δέσμη βιβλιοθήκη προγράμματος-πελάτη για Python] [ py_batch_package] για να δημιουργήσετε ένα χώρο συγκέντρωσης του διακομιστή Ubuntu κόμβους υπολογιστικών. Τεκμηρίωση αναφοράς για τη λειτουργική μονάδα δέσμης Python μπορείτε να βρείτε στο [πακέτο azure.batch] [py_batch_docs] στην ανάγνωση έγγραφα.

Σε αυτό το τμήμα κώδικα δημιουργεί μια [ImageReference] [ py_imagereference] ρητά και καθορίζει κάθε μία από τις ιδιότητές του (publisher, προσφορά, SKU, έκδοση). Στον κώδικα παραγωγής, ωστόσο, συνιστάται να χρησιμοποιήσετε το [list_node_agent_skus] [ py_list_skus] μέθοδο για να προσδιορίσετε και επιλέξτε από τη διαθέσιμη εικόνα κόμβου παράγοντας SKU συνδυασμούς και κατά το χρόνο εκτέλεσης.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Όπως προαναφέρθηκε, σας συνιστούμε αντί να δημιουργήσετε το [ImageReference] [ py_imagereference] ρητά, μπορείτε να χρησιμοποιήσετε το [list_node_agent_skus] [ py_list_skus] μέθοδο για να επιλέξετε δυναμικά από τους συνδυασμούς εικόνα παράγοντας/Marketplace κόμβο υποστηρίζονται αυτήν τη στιγμή. Το παρακάτω τμήμα κώδικα Python εμφανίζει χρήση αυτής της μεθόδου.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Δημιουργία χώρου συγκέντρωσης Linux: δέσμη .NET

Το παρακάτω τμήμα κώδικα παρουσιάζει ένα παράδειγμα του τρόπου χρήσης του [.NET δέσμη] [ nuget_batch_net] βιβλιοθήκη προγράμματος-πελάτη για να δημιουργήσετε ένα χώρο συγκέντρωσης του διακομιστή Ubuntu κόμβους υπολογιστικών. Μπορείτε να βρείτε την [τεκμηρίωση αναφοράς .NET δέσμη] [ api_net] στο MSDN.

Το παρακάτω τμήμα κώδικα χρησιμοποιεί το [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] μέθοδο για να επιλέξετε από τη λίστα των αυτήν τη στιγμή υποστηρίζονται Marketplace εικόνα και κόμβο παράγοντας SKU συνδυασμών. Αυτή η τεχνική είναι επιθυμητό επειδή τη λίστα των συνδυασμών υποστηριζόμενες ενδέχεται να αλλάξουν κατά καιρούς. Πιο συχνά, προστίθενται υποστηριζόμενες συνδυασμών.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Αν και το προηγούμενο τμήμα κώδικα χρησιμοποιεί το [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] μέθοδο για δυναμική λίστα και επιλέξτε από υποστηρίζεται εικόνα και κόμβο παράγοντας SKU τους συνδυασμούς (συνιστάται), μπορείτε επίσης να ρυθμίσετε μια [ImageReference] [ net_imagereference] ρητά:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Λίστα εικόνων εικονική μηχανή

Ο παρακάτω πίνακας παραθέτει τις εικόνες εικονική μηχανή Marketplace που είναι συμβατά με το διαθέσιμο παράγοντες κόμβο δέσμη όταν έγινε η τελευταία ενημέρωση σε αυτό το άρθρο. Είναι σημαντικό να λάβετε υπόψη ότι αυτή η λίστα δεν είναι οριστικά επειδή εικόνες και κόμβο παράγοντες ενδέχεται να είναι προστεθεί ή καταργηθεί οποιαδήποτε στιγμή. Συνιστάται να δέσμη εφαρμογές και τις υπηρεσίες σας πάντα χρησιμοποιείτε [list_node_agent_skus] [ py_list_skus] (Python) και [ListNodeAgentSkus] [ net_list_skus] (δέσμη .NET) για να προσδιορίσετε και επιλέξτε από τις διαθέσιμες επί του παρόντος SKU.

> [AZURE.WARNING] Η παρακάτω λίστα ενδέχεται να αλλάξουν οποιαδήποτε στιγμή. Πάντα με τις μεθόδους **λίστα κόμβο παράγοντας SKU** είναι διαθέσιμες στο τα API δέσμη λίστα και, στη συνέχεια, επιλέξτε από την συμβατά εικονική μηχανή και κόμβο παράγοντας SKU όταν εκτελείτε τις μαζικές εργασίες.

| **Ο Publisher** | **Προσφορά** | **Εικόνα SKU** | **Έκδοση** | **Παράγοντας κόμβο Αναγνωριστικό SKU** |
| ------- | ------- | ------- | ------- | ------- |
| Κανονική | UbuntuServer | 14.04.0-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 14.04.1-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 14.04.2-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 14.04.3-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 14.04.4-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 14.04.5-LTS | πιο πρόσφατη | Batch.node.ubuntu 14.04 |
| Κανονική | UbuntuServer | 16.04.0-LTS | πιο πρόσφατη | Batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | πιο πρόσφατη | Batch.node.debian 8 |
| OpenLogic | CentOS | 7.0 | πιο πρόσφατη | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | πιο πρόσφατη | Batch.node.centos 7 |
| OpenLogic | CentOS HPC | 7.1 | πιο πρόσφατη | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | πιο πρόσφατη | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | πιο πρόσφατη | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 πρέπει | πιο πρόσφατη | Batch.node.opensuse 13.2 πρέπει |
| SUSE | openSUSE δίσεκτο | 42.1 | πιο πρόσφατη | Batch.node.opensuse 42.1 |
| SUSE | SLES HPC | 12 | πιο πρόσφατη | Batch.node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | πιο πρόσφατη | Batch.node.opensuse 42.1 |
| διαφημίσεις της Microsoft | τυπική δεδομένων-science εικονική | τυπική δεδομένων-science εικονική | πιο πρόσφατη | Batch.node.Windows amd64 |
| διαφημίσεις της Microsoft | Linux δεδομένων-science εικονική | linuxdsvm | πιο πρόσφατη | Batch.node.centos 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 | πιο πρόσφατη | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-κέντρο δεδομένων | πιο πρόσφατη | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Κέντρο δεδομένων R2 2012 | πιο πρόσφατη | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows Server-Technical Preview | πιο πρόσφατη | Batch.node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Σύνδεση με κόμβους Linux

Κατά την ανάπτυξη ή κατά την επίλυση προβλημάτων, ίσως σας φανεί είναι απαραίτητο να εισέλθετε στο τους κόμβους του χώρου συγκέντρωσης. Σε αντίθεση με κόμβους υπολογιστικών των Windows, δεν μπορείτε να χρησιμοποιήσετε το πρωτόκολλο απομακρυσμένης επιφάνειας εργασίας (RDP) για να συνδεθείτε με κόμβους Linux. Αντί για αυτό, η υπηρεσία δέσμη επιτρέπει SSH πρόσβασης σε κάθε κόμβο για τη σύνδεση απομακρυσμένης.

Το παρακάτω τμήμα κώδικα Python δημιουργεί ένα χρήστη σε κάθε κόμβο σε ένα χώρο συγκέντρωσης, η οποία απαιτείται για τη σύνδεση απομακρυσμένης. Στη συνέχεια, εκτυπώνει τις πληροφορίες σύνδεσης ασφαλούς κελύφους (SSH) για κάθε κόμβο.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Ακολουθεί ένα δείγμα εξόδου για τον προηγούμενο κώδικα για ένα σύνολο που περιέχει τέσσερις κόμβους Linux:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Σημειώστε ότι αντί για έναν κωδικό πρόσβασης, μπορείτε να καθορίσετε ένα δημόσιο κλειδί SSH κατά τη δημιουργία ενός χρήστη σε έναν κόμβο. Στο Python SDK, αυτό γίνεται με χρήση της παραμέτρου **ssh_public_key** σε [ComputeNodeUser][py_computenodeuser]. Στο .NET, αυτό γίνεται με τη χρήση του [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] την ιδιότητα.

## <a name="pricing"></a>Τις τιμές

Azure δέσμη είναι ενσωματωμένη στην τεχνολογία υπηρεσιών Cloud Azure και εικονικές μηχανές Windows Azure. Την ίδια την υπηρεσία δέσμη προσφέρεται χωρίς κόστος, γεγονός που σημαίνει ότι θα χρεωθείτε μόνο για τους πόρους υπολογισμού που σας λύσεις δέσμη εκμετάλλευση. Όταν επιλέγετε τη **Ρύθμιση παραμέτρων των υπηρεσιών Cloud**, που θα χρεωθεί με βάση [τις τιμές τις υπηρεσίες Cloud] της[ cloud_services_pricing] δομή. Όταν επιλέγετε τη **Ρύθμιση παραμέτρων εικονική μηχανή**, που θα χρεωθεί με βάση την [Τιμολόγηση εικονικές μηχανές] [ vm_pricing] δομή.

## <a name="next-steps"></a>Επόμενα βήματα

### <a name="batch-python-tutorial"></a>Πρόγραμμα εκμάθησης Python δέσμης

Για μια πιο αναλυτικά το πρόγραμμα εκμάθησης σχετικά με τον τρόπο εργασίας με τη μαζική χρησιμοποιώντας Python, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το πρόγραμμα-πελάτη Python δέσμη Azure](batch-python-tutorial.md). Το συνοδευτικό [δείγμα κώδικα] [ github_samples_pyclient] περιλαμβάνει μια συνάρτηση Βοήθειας, `get_vm_config_for_distro`, που εμφανίζει μια άλλη τεχνική για να αποκτήσετε μια εικονική μηχανή ρύθμιση παραμέτρων.

### <a name="batch-python-code-samples"></a>Δείγματα κώδικα Python δέσμης

Δείτε τα άλλα [δείγματα κώδικα Python] [ github_samples_py] [azure-δέσμη-δείγματα] [ github_samples] αποθετήριο στην GitHub για διάφορες δέσμες ενεργειών που σας δείχνουν πώς να εκτελείτε κοινές λειτουργίες δέσμης όπως χώρου συγκέντρωσης, εργασία και δημιουργία εργασιών. Το [αρχείο README] [ github_py_readme] που συνοδεύει το Python δείγματα περιλαμβάνει λεπτομέρειες σχετικά με το πώς μπορείτε να εγκαταστήσετε τα απαιτούμενα πακέτα.

### <a name="batch-forum"></a>Φόρουμ δέσμης

Το [Φόρουμ δέσμη Azure] [ forum] στο MSDN είναι ένα καλό σημείο για να συζητήσετε δέσμης και να υποβάλετε ερωτήσεις σχετικά με την υπηρεσία. Διαβάστε δημοσιεύσεις χρήσιμα "stickied" και δημοσιεύστε τις ερωτήσεις σας καθώς προκύπτουν ενώ δημιουργείτε τις λύσεις δέσμης.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/

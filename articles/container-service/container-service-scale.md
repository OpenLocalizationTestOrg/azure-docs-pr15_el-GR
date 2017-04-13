<properties
   pageTitle="Κλιμάκωση το σύμπλεγμά σας ACS με το Azure CLI | Microsoft Azure"
   description="Μάθετε πώς να κλιμακωθεί Azure κοντέινερ υπηρεσιών συμπλέγματος χρησιμοποιώντας το Azure CLI."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, κοντέινερ, μικρής κλίμακας-υπηρεσίες, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Κλίμακα μιας υπηρεσίας Azure κοντέινερ

Μπορείτε να κλιμάκωση του αριθμού των κόμβους περιλαμβάνει την υπηρεσία κοντέινερ Azure (ACS), χρησιμοποιώντας το εργαλείο Azure CLI. Όταν χρησιμοποιείτε το CLI Azure για να περιορίσετε το μέγεθος, το εργαλείο επαναφέρει ένα νέο αρχείο ρύθμισης παραμέτρων που αντιπροσωπεύει την αλλαγή να εφαρμόζεται στο κοντέινερ.

## <a name="about-the-command"></a>Σχετικά με την εντολή

Το Azure CLI πρέπει να είναι σε λειτουργία Azure από διαχειριστή πόρων για να αλληλεπιδράσετε με Azure κοντέινερ. Μπορείτε να μεταβείτε σε λειτουργία διαχείρισης πόρων καλώντας `azure config mode arm`. Το `acs` εντολή έχει μια εντολή παιδί που ονομάζεται `scale` που κάνει όλες οι λειτουργίες κλίμακα για μια υπηρεσία κοντέινερ. Μπορείτε να λάβετε βοήθεια σχετικά με τις διάφορες παραμέτρους που χρησιμοποιούνται στην εντολή κλίμακα εκτελώντας το `azure acs scale --help`, που εξάγει κάτι παρόμοιο με αυτό:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Χρησιμοποιήστε την εντολή για να κλιμακωθεί

Για να περιορίσετε το μέγεθος μιας υπηρεσίας κοντέινερ, πρέπει πρώτα να γνωρίζετε την **ομάδα πόρων** και το **όνομα του Azure κοντέινερ υπηρεσίας (ACS)**, και επίσης να καθορίσετε το νέο πλήθος παραγόντων. Χρησιμοποιώντας έναν αριθμό μικρότερο ή νεότερη έκδοση, μπορείτε να κλιμάκωση προς τα κάτω ή προς τα επάνω αντίστοιχα.

Εάν θέλετε, μπορείτε να γνωρίζετε τι την τρέχουσα μέτρηση παραγόντων για μια υπηρεσία κοντέινερ πριν από την κλίμακα. Χρησιμοποιήστε το `azure acs show <resource group> <ACS name>` εντολή για να επιστρέψετε το ACS config. Σημείωση το αποτέλεσμα <mark>Count</mark> .

#### <a name="see-current-count"></a>Ανατρέξτε στην ενότητα τρέχουσα μέτρηση

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Κλιμάκωση σε νέο πλήθος

Ως έχει ήδη προφανείς, μπορείτε να κλιμάκωση την υπηρεσία κοντέινερ καλώντας `azure acs scale` και την παροχή την **ομάδα πόρων**, **ACS**και **παράγοντας count**. Όταν κλίμακα υπηρεσίας κοντέινερ, Azure CLI επιστρέφει μια συμβολοσειρά JSON που αντιπροσωπεύει τη νέα ρύθμιση παραμέτρων της υπηρεσίας κοντέινερ, όπως το νέο πλήθος παράγοντας.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Επόμενα βήματα

- [Ανάπτυξη ενός συμπλέγματος](container-service-deployment.md)
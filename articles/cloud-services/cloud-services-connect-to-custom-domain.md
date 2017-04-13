<properties
  pageTitle="Σύνδεση μια υπηρεσία Cloud με έναν προσαρμοσμένο ελεγκτή τομέα | Microsoft Azure"
  description="Μάθετε πώς μπορείτε να συνδεθείτε ρόλους σας web/εργασίας με έναν προσαρμοσμένο τομέα AD με χρήση του PowerShell και επέκταση τομέα AD"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Σύνδεση ρόλους των υπηρεσιών Cloud Azure με ένα προσαρμοσμένο ελεγκτή τομέα AD φιλοξενείται στο Azure

Πρώτα, θα σας θα ρυθμιστεί ένα εικονικό δίκτυο (VNet) στο Azure. Στη συνέχεια, θα προσθέσουμε μια ενεργή ελεγκτή τομέα καταλόγου (φιλοξενούνται σε μια Azure εικονική μηχανή) για να το VNet. Στη συνέχεια, θα προσθέσετε υπάρχοντες ρόλους υπηρεσία cloud το δημιουργηθεί εκ των προτέρων VNet και στη συνέχεια να τα συνδέσετε στον ελεγκτή τομέα.

Πριν ξεκινήσουμε, μερικά πράγματα που πρέπει να έχετε υπόψη:

1.  Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί PowerShell, οπότε βεβαιωθείτε ότι έχετε εγκαταστήσει PowerShell Azure και είστε έτοιμοι να στείλετε. Για να λάβετε βοήθεια σχετικά με τη ρύθμιση του Azure PowerShell, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του PowerShell Azure](../powershell-install-configure.md).

2.  Τις παρουσίες ελεγκτή τομέα AD και ρόλων Web/εργασίας πρέπει να είναι σε το VNet.

Ακολουθήστε αυτός ο οδηγός βήμα προς βήμα και εάν αντιμετωπίσετε προβλήματα, στείλτε μας τα σχόλιά παρακάτω. Κάποιος να επιστρέψετε σε εσάς (Ναι, θα σας ανάγνωση σχολίων).

3. Το δίκτυο στο οποίο γίνεται αναφορά από το cloud υπηρεσίας <mark>πρέπει να είναι</mark> σε **κλασική εικονικού δικτύου**.

## <a name="create-a-virtual-network"></a>Δημιουργήστε ένα εικονικό δίκτυο

Μπορείτε να δημιουργήσετε ένα εικονικό δίκτυο στο Azure χρησιμοποιώντας την πύλη του Azure κλασική ή PowerShell. Για αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε PowerShell. Για να δημιουργήσετε ένα εικονικό δίκτυο με το Azure κλασική πύλη, ανατρέξτε στο θέμα [Δημιουργία εικονικού δικτύου](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Δημιουργήστε μια εικονική μηχανή

Αφού ολοκληρώσετε τη ρύθμιση του το εικονικό δίκτυο, θα πρέπει να δημιουργήσετε ένα ελεγκτή τομέα AD. Για αυτό το πρόγραμμα εκμάθησης, θα σας θα ρυθμίσετε τον ένα ελεγκτή τομέα AD στην εικονική μηχανή μια Azure.

Για να κάνετε αυτό, δημιουργήστε μια εικονική μηχανή μέσω του PowerShell χρησιμοποιώντας τις παρακάτω εντολές:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Προωθήστε την εικονική μηχανή σε ελεγκτή τομέα
Για να ρυθμίσετε την εικονική μηχανή ως ένα ελεγκτή τομέα AD, θα πρέπει να συνδεθείτε στο η Εικονική και ρυθμίστε τις παραμέτρους του.

Για να συνδεθείτε η Εικονική, μπορείτε να λάβετε το αρχείο RDP μέσω του PowerShell, χρησιμοποιήστε τις παρακάτω εντολές.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Αφού έχετε συνδεθεί στο η Εικονική, το πρόγραμμα εγκατάστασης σας εικονική μηχανή ως ένα ελεγκτή τομέα AD ακολουθώντας το Οδηγός βήμα προς βήμα σχετικά με [τον τρόπο ρύθμισης του ελεγκτή τομέα AD των πελατών σας](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Προσθέστε την υπηρεσία Cloud για να το εικονικό δίκτυο

Στη συνέχεια, πρέπει να προσθέσετε την ανάπτυξη υπηρεσία cloud για να το VNet που μόλις δημιουργήσατε. Για να το κάνετε αυτό, τροποποιήστε σας cscfg υπηρεσία cloud, προσθέτοντας στις σχετικές ενότητες για να σας cscfg χρησιμοποιώντας το Visual Studio ή το πρόγραμμα επεξεργασίας της επιλογής σας.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Στη συνέχεια, δημιουργήστε το έργο υπηρεσιών cloud και αναπτύξετε για Azure. Για να λάβετε βοήθεια σχετικά με την ανάπτυξη του πακέτου υπηρεσιών cloud σε Azure, δείτε [πώς μπορείτε να δημιουργία και ανάπτυξη μια υπηρεσία στο Cloud](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Σύνδεση σας ρόλους web/εργαζόμενου με τον τομέα

Μόλις το έργο υπηρεσία cloud έχει αναπτυχθεί σε Azure, συνδέστε τις παρουσίες ρόλο στον προσαρμοσμένο τομέα AD χρησιμοποιώντας την επέκταση τομέα AD. Για να προσθέσετε την επέκταση τομέα AD του υπάρχοντος ανάπτυξη υπηρεσιών cloud και συνδεθείτε με τον προσαρμοσμένο τομέα, εκτελέστε τις ακόλουθες εντολές στο PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Και τώρα.

Σύννεφο υπηρεσιών θα πρέπει τώρα να συμμετέχει σε ελεγκτή του προσαρμοσμένου τομέα σας. Εάν θέλετε να μάθετε περισσότερα σχετικά με τις διάφορες επιλογές που είναι διαθέσιμες για το πώς μπορείτε να ρυθμίσετε τις παραμέτρους επέκταση τομέα AD, χρησιμοποιήστε τη Βοήθεια του PowerShell, όπως φαίνεται παρακάτω.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```

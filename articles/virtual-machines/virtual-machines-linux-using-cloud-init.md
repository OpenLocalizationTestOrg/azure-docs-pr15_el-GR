<properties
    pageTitle="Χρησιμοποιώντας την προετοιμασία cloud για να προσαρμόσετε μια Εικονική Linux κατά τη δημιουργία | Microsoft Azure"
    description="Χρησιμοποιώντας την προετοιμασία cloud για να προσαρμόσετε μια Εικονική Linux κατά τη δημιουργία."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Χρησιμοποιώντας την προετοιμασία cloud για να προσαρμόσετε μια Εικονική Linux κατά τη δημιουργία

Σε αυτό το άρθρο παρουσιάζει πώς μπορείτε να δημιουργήσετε μια δέσμη ενεργειών cloud την προετοιμασία για να ορίσετε το όνομα κεντρικού υπολογιστή, πακέτων εγκατεστημένη την ενημερωμένη έκδοση, και να διαχειριστείτε τους λογαριασμούς χρηστών.  Οι δέσμες ενεργειών cloud την προετοιμασία ονομάζονται κατά τη δημιουργία Εικονική από το Azure CLI.  Το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).

- το [Azure CLI](../xplat-cli-install.md) συνδεθεί με `azure login`.

- στη λειτουργία διαχείρισης πόρων Azure _πρέπει να είναι σε_ Azure CLI `azure config mode arm`.

## <a name="quick-commands"></a>Γρήγορες εντολές

Δημιουργήστε μια δέσμη ενεργειών cloud init.txt που ορίζει το όνομα κεντρικού υπολογιστή, ενημερώνει όλα τα πακέτα και προσθέτει ένα χρήστη sudo Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Δημιουργήστε μια ομάδα πόρων για την εκκίνηση ΣΠΣ σε.

```bash
azure group create myResourceGroup westus
```

Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας την προετοιμασία cloud για να ρυθμίσετε τις παραμέτρους της κατά την εκκίνηση.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση

### <a name="introduction"></a>Εισαγωγή

Κατά την εκκίνηση μιας νέας Εικονική Linux, λαμβάνετε μια τυπική Εικονική Linux χωρίς τίποτε προσαρμοσμένων ή έτοιμη για τις ανάγκες σας. [Προετοιμασία cloud](https://cloudinit.readthedocs.org) είναι ένας τυπικός τρόπος για την εισαγωγή μια δέσμη ενεργειών ή ρύθμισης παραμέτρων ρυθμίσεων στο συγκεκριμένο Εικονική Linux κατά την εκκίνηση για χρήση με την πρώτη φορά.

Στην Azure, υπάρχουν μια τρεις διαφορετικοί τρόποι για να κάνετε αλλαγές σε μια Εικονική Linux κατά τη διάρκεια αναπτύξει ή την εκκίνηση.

- Εισαγωγή δέσμης ενεργειών με χρήση cloud την προετοιμασία.
- Εισαγωγή δέσμης ενεργειών με χρήση του Azure [Επέκταση VMAccess](virtual-machines-linux-using-vmaccess-extension.md).
- Azure προτύπου χρησιμοποιώντας την προετοιμασία cloud.
- Azure προτύπου με χρήση [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Για την εισαγωγή δέσμες ενεργειών οποιαδήποτε στιγμή μετά την εκκίνηση:

- SSH για να εκτελέσετε εντολές απευθείας
- Εισαγωγή δέσμης ενεργειών με χρήση του Azure [Επέκταση VMAccess](virtual-machines-linux-using-vmaccess-extension.md), imperatively ή σε ένα πρότυπο Azure
- Εργαλεία διαχείρισης ρύθμισης παραμέτρων όπως Ansible, Chef, και το επιτραπέζιο και Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Διαθεσιμότητα cloud την προετοιμασία στην Εικονική Azure γρήγορης δημιουργίας ψευδώνυμα εικόνα:

| Ψευδώνυμο     | Ο Publisher | Προσφορά        | SKU         | Έκδοση | Προετοιμασία cloud |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | πιο πρόσφατη  | Όχι         |
| CoreOS    | CoreOS    | CoreOS       | Σταθερή      | πιο πρόσφατη  | Ναι        |
| Debian    | credativ  | Debian       | 8           | πιο πρόσφατη  | Όχι         |
| openSUSE  | SUSE      | openSUSE     | 13.2 πρέπει        | πιο πρόσφατη  | Όχι         |
| RHEL      | Redhat    | RHEL         | 7.2         | πιο πρόσφατη  | Όχι         |
| UbuntuLTS | Κανονική | UbuntuServer | 14.04.4-LTS | πιο πρόσφατη  | Ναι        |

Η Microsoft συνεργάζεται με τους συνεργάτες μας για να λάβετε cloud την προετοιμασία περιλαμβάνονται και να εργάζονται σε τις εικόνες που θα παρέχουν σε Azure.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Προσθήκη μιας δέσμης ενεργειών cloud την προετοιμασία για τη δημιουργία Εικονική με το CLI Azure

Για να ξεκινήσετε μια δέσμη ενεργειών cloud την προετοιμασία κατά τη δημιουργία μια Εικονική στο Azure, καθορίστε το αρχείο cloud την προετοιμασία χρησιμοποιώντας το Azure CLI `--custom-data` εναλλαγή.

Δημιουργήστε μια ομάδα πόρων για την εκκίνηση ΣΠΣ σε.

```bash
azure group create myResourceGroup westus
```

Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας την προετοιμασία cloud για να ρυθμίσετε τις παραμέτρους της κατά την εκκίνηση.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Δημιουργήσετε μια δέσμη ενεργειών cloud την προετοιμασία για να ορίσετε το όνομα κεντρικού υπολογιστή του μια Εικονική Linux

Μία από τις πιο απλός και πιο σημαντικές ρυθμίσεις για οποιαδήποτε Εικονική Linux θα είναι το όνομα κεντρικού υπολογιστή. Θα σας μπορούν εύκολα να ορίσουν αυτό χρησιμοποιώντας την προετοιμασία cloud με αυτήν τη δέσμη ενεργειών.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Παράδειγμα cloud την προετοιμασία δέσμης ενεργειών με το όνομα `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Κατά την αρχική εκκίνηση η Εικονική, αυτήν τη δέσμη ενεργειών cloud την προετοιμασία ορίζει το όνομα κεντρικού υπολογιστή για να `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Σύνδεση και βεβαιωθείτε ότι το όνομα κεντρικού υπολογιστή από το νέο Εικονική.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Δημιουργήσετε μια δέσμη ενεργειών cloud την προετοιμασία για να ενημερώσετε Linux

Ασφάλεια, θέλετε σας Εικονική Ubuntu για να ενημερώσετε κατά την πρώτη εκκίνηση.  Χρήση cloud την προετοιμασία μπορούμε να κάνουμε που με τη δέσμη ενεργειών παρακολούθηση, ανάλογα με την κατανομή Linux που χρησιμοποιείτε.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Παράδειγμα δέσμης ενεργειών cloud την προετοιμασία `cloud_config_apt_upgrade.txt` για την οικογένεια Debian

```bash
#cloud-config
apt_upgrade: true
```

Μετά την εκκίνηση Linux, όλα τα εγκατεστημένα πακέτα ενημερώνονται μέσω `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Σύνδεση και βεβαιωθείτε ότι όλα τα πακέτα ενημερώνονται.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Δημιουργήσετε μια δέσμη ενεργειών cloud την προετοιμασία για να προσθέσετε ένα χρήστη Linux

Μία από τις πρώτες εργασίες σε οποιαδήποτε νέα Εικονική Linux είναι να προσθέσετε ένα χρήστη για τον εαυτό σας ή για να αποφύγετε τη χρήση `root`. SSH πλήκτρα είναι βέλτιστη πρακτική για την ασφάλεια και για χρηστικότητα και τότε αυτά θα προστεθούν τα `~/.ssh/authorized_keys` αρχείο με αυτήν τη δέσμη ενεργειών cloud την προετοιμασία.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Παράδειγμα δέσμης ενεργειών cloud την προετοιμασία `cloud_config_add_users.txt` για Debian οικογένεια προγραμμάτων

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Μετά την εκκίνηση Linux, όλοι οι χρήστες που παρατίθενται δημιουργούνται και προστίθενται στην ομάδα sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Σύνδεση και βεβαιωθείτε ότι ο χρήστης που μόλις δημιουργήθηκε.

```bash
ssh myVM
cat /etc/group
```

Εξόδου

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Επόμενα βήματα

Προετοιμασία cloud γίνεται μία τυπική τρόπος για να τροποποιήσετε το Εικονική Linux κατά την εκκίνηση. Azure έχει επίσης επεκτάσεις Εικονική, οι οποίες σας επιτρέπουν να τροποποιήσετε το LinuxVM κατά την εκκίνηση ή ενώ εκτελείται. Για παράδειγμα, μπορείτε να χρησιμοποιήσετε το Azure VMAccessExtension για την επαναφορά χρηστών ή SSH πληροφορίες ενώ εκτελείται η Εικονική. Με το cloud την προετοιμασία, θα χρειαστεί επανεκκίνηση για να επαναφέρετε τον κωδικό πρόσβασης.

[Σχετικά με τις δυνατότητες και επεκτάσεις εικονική μηχανή](virtual-machines-linux-extensions-features.md)

[Διαχείριση χρηστών, SSH και ελέγχου ή επιδιόρθωση δίσκων σε ΣΠΣ Linux Azure χρησιμοποιώντας την επέκταση VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

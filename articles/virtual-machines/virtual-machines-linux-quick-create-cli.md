<properties
   pageTitle="Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας το CLI | Microsoft Azure"
   description="Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας το CLI."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας το CLI

Αυτό το άρθρο περιγράφει τον τρόπο ανάπτυξης γρήγορα μια εικονική μηχανή Linux (Εικονική) στην Azure χρησιμοποιώντας το `azure vm quick-create` εντολής στο το Azure περιβάλλον γραμμής εντολών (CLI). Το `quick-create` εντολή αναπτύσσει μια Εικονική μέσα σε μια βασική, ασφαλή υποδομή ότι μπορείτε να χρησιμοποιήσετε για να πρωτοτύπου ή να ελέγξετε γρήγορα μια έννοια. Το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).

- το [Azure CLI](../xplat-cli-install.md) συνδεθεί με `azure login`.

- στη λειτουργία διαχείρισης πόρων Azure _πρέπει να είναι σε_ Azure CLI `azure config mode arm`.

Μπορείτε να αναπτύξετε επίσης γρήγορα μια Εικονική Linux χρησιμοποιώντας την [πύλη του Azure](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Γρήγορες εντολές

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να αναπτύξετε μια Εικονική CoreOS και να επισυνάψετε ασφαλούς κελύφους (SSH) αριθμού-κλειδιού (σας ορίσματα μπορεί να είναι διαφορετικές):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση

Η ακόλουθη αναλυτική παρουσίαση έχει μια Εικονική UbuntuLTS που αναπτύσσονται, βήμα προς βήμα, με επεξηγήσεις του τι κάνει κάθε βήμα.

## <a name="vm-quick-create-aliases"></a>Εικονική γρήγορης δημιουργίας ψευδώνυμα

Ένας γρήγορος τρόπος για να επιλέξετε μια κατανομή είναι να χρησιμοποιήσετε τα ψευδώνυμα Azure CLI αντιστοιχιστεί τα πιο συνηθισμένα κατανομές OS. Ο παρακάτω πίνακας παραθέτει τα ψευδώνυμα (από τον Azure CLI έκδοση 0.10). Όλες τις αναπτύξεις που χρησιμοποιούν `quick-create` προεπιλογή για ΣΠΣ που δημιουργούνται αντίγραφα από χώρο αποθήκευσης οπτικοί μονάδα δίσκου (SSD), το οποίο προσφέρει πιο γρήγορα παροχής και υψηλών επιδόσεων πρόσβαση στο δίσκο. (Αυτά τα ψευδώνυμα αντιπροσωπεύουν ένα μικρό τμήμα από τη διαθέσιμη κατανομές στην Azure. Βρείτε περισσότερες εικόνες από το Azure Marketplace κάνοντας [Αναζήτηση για μια εικόνα στο PowerShell](virtual-machines-linux-cli-ps-findimage.md), [στο web](https://azure.microsoft.com/marketplace/virtual-machines/)ή [Αποστολή τη δική σας προσαρμοσμένη εικόνα](virtual-machines-linux-create-upload-generic.md).)

| Ψευδώνυμο     | Ο Publisher | Προσφορά        | SKU         | Έκδοση |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | πιο πρόσφατη  |
| CoreOS    | CoreOS    | CoreOS       | Σταθερή      | πιο πρόσφατη  |
| Debian    | credativ  | Debian       | 8           | πιο πρόσφατη  |
| openSUSE  | SUSE      | openSUSE     | 13.2 πρέπει        | πιο πρόσφατη  |
| RHEL      | Κόκκινο καπέλο    | RHEL         | 7.2         | πιο πρόσφατη  |
| UbuntuLTS | Κανονική | Ubuntu διακομιστή | 14.04.4-LTS | πιο πρόσφατη  |

Τα παρακάτω ενότητες Χρησιμοποιήστε το `UbuntuLTS` ψευδώνυμο για την επιλογή **ImageURN** (`-Q`) για να αναπτύξετε ένα διακομιστή Ubuntu 14.04.4 Αποτελεσμάτων.

Η προηγούμενη `quick-create` παράδειγμα μόνο καλούνται το `-M` σημαία για να προσδιορίσετε το SSH δημόσιο κλειδί για να αποστείλετε κατά την απενεργοποίηση SSH τους κωδικούς πρόσβασης, έτσι θα σας ζητηθεί για τα παρακάτω ορίσματα:

- όνομα ομάδας πόρων (οποιαδήποτε συμβολοσειρά είναι συνήθως λεπτομερές για την πρώτη ομάδα Azure πόρων)
- Εικονική όνομα
- θέση (`westus` ή `westeurope` είναι καλή τις προεπιλεγμένες ρυθμίσεις)
- Linux (για να επιτρέψετε Azure γνωρίζετε ποιο λειτουργικό σύστημα που θέλετε)
- όνομα χρήστη

Το παρακάτω παράδειγμα καθορίζει όλες τις τιμές, έτσι ώστε να απαιτείται καμία περαιτέρω ερώτηση. Εφόσον έχετε μια `~/.ssh/id_rsa.pub` ως ssh rsa δημόσια κλειδιού αρχείο μορφής, που λειτουργεί όπως είναι:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Το αποτέλεσμα θα πρέπει να μοιάζει με το εξής αποτέλεσμα μπλοκ:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Συνδεθείτε με τη νέα εικονική Μηχανή

Συνδεθείτε στο Εικονική σας με τη χρήση στη δημόσια διεύθυνση IP που αναφέρονται στο αποτέλεσμα. Μπορείτε επίσης να χρησιμοποιήσετε το πλήρως προσδιορισμένο όνομα τομέα (FQDN) που βρίσκεται στη λίστα:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Η διαδικασία σύνδεσης πρέπει να μοιάζει με το ακόλουθο μπλοκ εξόδου:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Επόμενα βήματα

Το `azure vm quick-create` εντολή είναι ο τρόπος για να αναπτύξετε μια Εικονική γρήγορα, ώστε να μπορείτε να συνδεθείτε σε ένα πάρτι κέλυφος και ξεκινήστε την εργασία σας. Ωστόσο, χρησιμοποιώντας `vm quick-create` δεν σάς δίνουν εκτεταμένο έλεγχο ούτε να δίνουν τη δυνατότητα να δημιουργήσετε ένα πιο σύνθετο περιβάλλον.  Για να αναπτύξετε μια Εικονική Linux που είναι προσαρμοσμένο για την υποδομή σας, μπορείτε να ακολουθήσετε οποιαδήποτε από αυτά τα άρθρα:

- [Χρησιμοποιήστε ένα πρότυπο από διαχειριστή πόρων Azure για να δημιουργήσετε μια συγκεκριμένη ανάπτυξη](virtual-machines-linux-cli-deploy-templates.md)
- [Δημιουργήστε το δικό σας προσαρμοσμένο περιβάλλον για μια Εικονική Linux απευθείας χρησιμοποιώντας εντολές Azure CLI](virtual-machines-linux-create-cli-complete.md)
- [Δημιουργία SSH ασφαλές Linux Εικονική μηχανή σε Azure με τη χρήση προτύπων](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Μπορείτε επίσης να [Χρησιμοποιήστε το `docker-machine` Azure πρόγραμμα οδήγησης με διάφορες εντολές για να δημιουργήσετε γρήγορα μια Εικονική Linux ως κεντρικός υπολογιστής docker](virtual-machines-linux-docker-machine.md).

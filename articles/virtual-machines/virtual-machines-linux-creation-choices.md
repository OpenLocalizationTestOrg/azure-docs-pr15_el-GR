<properties
    pageTitle="Διαφορετικούς τρόπους για να δημιουργήσετε μια Εικονική Linux | Microsoft Azure"
    description="Μάθετε τους διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή Linux στην Azure, καθώς και για συνδέσεις σε εργαλεία και προγράμματα εκμάθησης για κάθε μέθοδο."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Διαφορετικούς τρόπους για να δημιουργήσετε μια εικονική μηχανή Linux στο Azure

Έχετε την ευελιξία στο Azure για να δημιουργήσετε μια Linux εικονική μηχανή (Εικονική) χρησιμοποιώντας εργαλεία και ροές εργασίας άνετα για εσάς. Σε αυτό το άρθρο συνοψίζει αυτές τις διαφορές και παραδείγματα για τη δημιουργία του ΣΠΣ Linux.


## <a name="azure-cli"></a>Azure CLI 

Το Azure CLI είναι διαθέσιμη σε πλατφόρμες μέσω ενός πακέτου npm, που παρέχονται από distro πακέτων ή Docker κοντέινερ. Μπορείτε να διαβάσετε περισσότερα σχετικά με [τον τρόπο εγκατάστασης και ρύθμισης παραμέτρων του Azure CLI](../xplat-cli-install.md). Τα παρακάτω προγράμματα εκμάθησης παρέχουν παραδείγματα σχετικά με τη χρήση του Azure CLI. Διαβάστε το άρθρο κάθε για περισσότερες λεπτομέρειες σχετικά με τις εντολές γρήγορης εκκίνησης CLI που εμφανίζονται:

- [Δημιουργήστε μια Εικονική Linux από το Azure CLI για προγραμματιστές και έλεγχος](virtual-machines-linux-quick-create-cli.md)
    - Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική CoreOS χρησιμοποιώντας ένα δημόσιο κλειδί με το όνομα `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Δημιουργήστε μια ασφαλή Εικονική Linux χρησιμοποιώντας ένα πρότυπο Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Το παρακάτω παράδειγμα δημιουργεί μια Εικονική χρησιμοποιώντας ένα πρότυπο που είναι αποθηκευμένο στο GitHub:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Δημιουργήστε ένα πλήρες περιβάλλον Linux χρησιμοποιώντας το CLI Azure](virtual-machines-linux-create-cli-complete.md)
    - Περιλαμβάνει τη δημιουργία ενός εξισορρόπηση φόρτου και πολλές ΣΠΣ σε ένα σύνολο διαθεσιμότητα.

- [Προσθέσετε ένα δίσκο σε μια Εικονική Linux](virtual-machines-linux-add-disk.md)
    - Το παρακάτω παράδειγμα προσθέτει ένα δίσκο 5Gb σε μια υπάρχουσα Εικονική με το όνομα `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Πύλη του Azure

Στην [πύλη του Azure](https://portal.azure.com) σάς επιτρέπει να δημιουργήσετε γρήγορα μια Εικονική εφόσον δεν υπάρχει τίποτα, προκειμένου να εγκαταστήσετε στο σύστημά σας. Χρησιμοποιήστε την πύλη του Azure για να δημιουργήσετε την εικονική Μηχανή:

- [Δημιουργήστε μια Εικονική Linux με την πύλη Azure](virtual-machines-linux-quick-create-portal.md) 
- [Επισυνάψτε ένα δίσκο με την πύλη Azure](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Επιλογές εικόνας και το λειτουργικό σύστημα
Όταν δημιουργείτε μια Εικονική, μπορείτε να επιλέξετε μια εικόνα με βάση το λειτουργικό σύστημα που θέλετε να εκτελέσετε. Azure και οι συνεργάτες προσφέρουν πολλές εικόνες, ορισμένες από τις οποίες περιλαμβάνουν εφαρμογές και εργαλεία προ-εγκατεστημένο. Εναλλακτικά, μπορείτε να αποστείλετε μία από τις δικές σας εικόνες (ανατρέξτε [στην παρακάτω ενότητα](#use-your-own-image)).

### <a name="azure-images"></a>Azure εικόνες
Χρησιμοποιήστε το `azure vm image` CLI εντολές για να δείτε τι είναι διαθέσιμο, publisher, έκδοση distro και εκδόσεις.

Λίστα εκδοτών διαθέσιμη ως εξής:

```bash
azure vm image list-publishers --location WestUS
```

Λίστα διαθέσιμα προϊόντα (προσφέρει) για μια δεδομένη publisher ως εξής:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Λίστα διαθέσιμες SKU (distro εκδόσεις) από μια δεδομένη προσφορά ως εξής:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Λίστα όλες τις διαθέσιμες εικόνες για μια δεδομένη κυκλοφορίας ακολουθεί:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Για περισσότερα παραδείγματα στην περιήγηση και η χρήση διαθέσιμες εικόνες, ανατρέξτε στο θέμα [Περιήγηση και επιλογή Azure εικονική μηχανή εικόνων με το Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

Το `azure vm quick-create` και `azure vm create` εντολές έχουν ψευδώνυμα που μπορείτε να χρησιμοποιήσετε για να μεταβείτε γρήγορα σε τα πιο κοινά distros και τις πιο πρόσφατες εκδόσεις. Χρήση ψευδωνύμων είναι συχνά ταχύτερη από τον καθορισμό του publisher, προσφορά, SKU και έκδοσης κάθε φορά που δημιουργείτε μια Εικονική:

| Ψευδώνυμο     | Ο Publisher | Προσφορά        | SKU         | Έκδοση |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | πιο πρόσφατη  |
| CoreOS    | CoreOS    | CoreOS       | Σταθερή      | πιο πρόσφατη  |
| Debian    | credativ  | Debian       | 8           | πιο πρόσφατη  |
| openSUSE  | SUSE      | openSUSE     | 13.2 πρέπει        | πιο πρόσφατη  |
| RHEL      | Redhat    | RHEL         | 7.2         | πιο πρόσφατη  |
| SLES      | SLES      | SLES         | 12-SP1      | πιο πρόσφατη  |
| UbuntuLTS | Κανονική | UbuntuServer | 14.04.4-LTS | πιο πρόσφατη  |

### <a name="use-your-own-image"></a>Χρησιμοποιήστε τη δική σας εικόνα

Εάν απαιτούνται συγκεκριμένες προσαρμογές, μπορείτε να χρησιμοποιήσετε μια εικόνα που βασίζεται σε μια υπάρχουσα Εικονική Azure *καταχωρώντας* που Εικονική. Μπορείτε επίσης να αποστείλετε μια εικόνα που δημιουργήθηκε εσωτερικής εγκατάστασης. Για περισσότερες πληροφορίες σχετικά με την υποστηριζόμενη distros και πώς μπορείτε να χρησιμοποιήσετε τις δικές σας εικόνες, ανατρέξτε στα ακόλουθα άρθρα:

- [Azure θεωρηθεί κατανομές](virtual-machines-linux-endorsed-distros.md)

- [Πληροφορίες για μη θεωρηθεί κατανομές](virtual-machines-linux-create-upload-generic.md)

- [Πώς μπορείτε να καταγράψετε μια εικονική μηχανή Linux ως πρότυπο για τη διαχείριση πόρων](virtual-machines-linux-capture-image.md).
    - Γρήγορης εκκίνησης παράδειγμα εντολές για να καταγράψετε μια υπάρχουσα Εικονική:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Επόμενα βήματα

- Δημιουργήστε μια Εικονική Linux από την [πύλη](virtual-machines-linux-quick-create-portal.md), με το [CLI](virtual-machines-linux-quick-create-cli.md)ή τη χρήση ενός [προτύπου για τη διαχείριση πόρων Azure](virtual-machines-linux-cli-deploy-templates.md).

- Αφού δημιουργήσετε μια Εικονική Linux, [προσθέστε ένα δίσκο δεδομένων](virtual-machines-linux-add-disk.md).

- Γρήγορα βήματα για να [επαναφέρετε έναν κωδικό πρόσβασης ή πλήκτρα SSH και διαχείριση χρηστών](virtual-machines-linux-using-vmaccess-extension.md)

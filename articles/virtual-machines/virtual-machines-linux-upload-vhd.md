<properties
    pageTitle="Δημιουργία και αποστολή ενός προσαρμοσμένου ειδώλου Linux | Microsoft Azure"
    description="Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου (VHD) Azure με μια προσαρμοσμένη εικόνα Linux χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Αποστολή και να δημιουργήσετε μια Εικονική Linux από εικόνα προσαρμοσμένης δίσκου

Αυτό το άρθρο παρουσιάζει τον τρόπο για την αποστολή ενός εικονικού σκληρού δίσκου (VHD) για να Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων και δημιουργία ΣΠΣ Linux από αυτή η εικόνα προσαρμοσμένης. Αυτή η λειτουργία σάς επιτρέπει να εγκατάσταση και ρύθμιση παραμέτρων ενός distro Linux με τις απαιτήσεις σας και, στη συνέχεια, χρησιμοποιήστε αυτό VHD για να δημιουργήσετε γρήγορα Azure εικονικές μηχανές (ΣΠΣ).

## <a name="quick-commands"></a>Γρήγορες εντολές
Εάν πρέπει να εκτελέσετε γρήγορα την εργασία, τις ακόλουθες λεπτομέρειες ενότητα τη βάση εντολές για να αποστείλετε μια Εικονική στο Azure. Πιο λεπτομερείς πληροφορίες περιβάλλοντος για κάθε βήμα μπορείτε να βρείτε και το υπόλοιπο του εγγράφου, [ξεκινώντας εδώ](#requirements).

Βεβαιωθείτε ότι έχετε [Το Azure CLI](../xplat-cli-install.md) πραγματοποιήσει είσοδο και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `mystorageaccount`, και `myimages`.

Πρώτα, δημιουργήστε μια ομάδα πόρων. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUs` θέση:

```bash
azure group create myResourceGroup --location "WestUS"
```

Δημιουργία λογαριασμού χώρου αποθήκευσης για τη διατήρηση εικονικού δίσκων σας. Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Λίστα των πλήκτρων πρόσβασης για το λογαριασμό χώρου αποθήκευσης. Σημειώστε `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Δημιουργία κοντέινερ μέσα στο λογαριασμό σας χώρο αποθήκευσης χρησιμοποιώντας το κλειδί χώρου αποθήκευσης που έχετε λάβει. Το ακόλουθο παράδειγμα δημιουργεί ένα κοντέινερ που ονομάζεται `myimages` χρησιμοποιώντας την τιμή του κλειδιού χώρου αποθήκευσης από `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Τέλος, αποστείλετε το VHD στο κοντέινερ που δημιουργήσατε. Καθορίστε την τοπική διαδρομή για να σας VHD στην περιοχή `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Τώρα, μπορείτε να δημιουργήσετε μια Εικονική από σας έχουν αποσταλεί εικονικό δίσκο [με τη χρήση προτύπου για τη διαχείριση πόρων](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Μπορείτε επίσης να χρησιμοποιήσετε το CLI, καθορίζοντας το URI σας δίσκο (`--image-urn`). Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική με το όνομα `myVM` που χρησιμοποιεί το εικονικό δίσκο έχετε ήδη αποστείλει:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Το λογαριασμό χώρου αποθήκευσης προορισμού πρέπει να είναι ίδια με όπου έχετε αποστείλει εικονικό δίσκο σας για να. Πρέπει επίσης να καθορίσετε ή να σας ζητηθεί από το answer για όλες τις πρόσθετες παραμέτρους που απαιτούνται από το `azure vm create` εντολής όπως εικονικού δικτύου, δημόσια διεύθυνση IP, όνομα χρήστη και SSH πλήκτρα. Μπορείτε να διαβάσετε περισσότερα σχετικά με τις [διαθέσιμες παραμέτρους διαχείρισης πόρων CLI](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Απαιτήσεις
Για να ολοκληρώσετε τα παρακάτω βήματα, πρέπει:

- **Λειτουργικό σύστημα Linux εγκατεστημένο σε ένα αρχείο .vhd** - εγκαταστήσετε μια [κατανομή θεωρηθεί Azure Linux](virtual-machines-linux-endorsed-distros.md) (ή ανατρέξτε στο θέμα [πληροφορίες για μη θεωρηθεί κατανομές](virtual-machines-linux-create-upload-generic.md)) σε ένα εικονικό δίσκο με τη μορφή VHD. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε μια Εικονική και VHD:
    - Εγκατάσταση και ρύθμιση παραμέτρων [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) ή [KVM](http://www.linux-kvm.org/page/RunningKVM), φροντίζοντας να χρησιμοποιήσετε VHD ως μορφή εικόνας. Εάν είναι απαραίτητο, μπορείτε να [μετατρέψετε μια εικόνα](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) χρησιμοποιώντας `qemu-img convert`.
    - Μπορείτε επίσης να χρησιμοποιήσετε το Hyper-V [στα Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) ή [στο Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Στη νεότερη μορφή VHDX δεν υποστηρίζεται στο Azure. Όταν δημιουργείτε μια Εικονική, καθορίστε VHD ως μορφή. Εάν είναι απαραίτητο, μπορείτε να μετατρέψετε δίσκων VHDX με τη χρήση του VHD [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ή το [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) cmdlet του PowerShell. Επιπλέον, Azure δεν υποστηρίζει αποστολή δυναμικής VHD, ώστε να πρέπει να μετατρέψετε αυτές δίσκων στατική VHD πριν από την αποστολή. Μπορείτε να χρησιμοποιήσετε εργαλεία όπως το [Azure VHD βοηθητικά προγράμματα για ΜΕΤΆΒΑΣΗ](https://github.com/Microsoft/azure-vhd-utils-for-go) για να μετατρέψετε δυναμικών δίσκων κατά τη διαδικασία Αποστολή σε Azure.

- ΣΠΣ που δημιουργήθηκε από την προσαρμοσμένη εικόνα σας πρέπει να βρίσκεται στον ίδιο λογαριασμό χώρου αποθήκευσης με την ίδια την εικόνα
    - Δημιουργία λογαριασμού χώρου αποθήκευσης και κοντέινερ για τη διατήρηση τόσο το προσαρμοσμένο εικόνα και που έχουν δημιουργηθεί ΣΠΣ
    - Αφού δημιουργήσετε όλες τις ΣΠΣ, μπορείτε να διαγράψετε την εικόνα σας

Βεβαιωθείτε ότι έχετε [Το Azure CLI](../xplat-cli-install.md) πραγματοποιήσει είσοδο και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `mystorageaccount`, και `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Προετοιμασία εικόνας για να αποσταλεί

Azure υποστηρίζει διάφορες Linux κατανομές (ανατρέξτε στο θέμα [Θεωρηθεί κατανομές](virtual-machines-linux-endorsed-distros.md)). Τα ακόλουθα άρθρα σας καθοδηγήσει πώς να προετοιμάσετε το διάφορες κατανομές Linux που υποστηρίζονται σε Azure:

- **[Διανομή βάσει centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Κόκκινο καπέλο Linux για μεγάλες επιχειρήσεις](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Άλλα - κατανομές μη θεωρηθεί](virtual-machines-linux-create-upload-generic.md)**

Ανατρέξτε επίσης στις **[Σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** για πιο γενικές συμβουλές σχετικά με την προετοιμασία Linux εικόνες για Azure.

> [AZURE.NOTE] Το [Azure πλατφόρμα SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) ισχύει για το ΣΠΣ εκτελείται Linux μόνο όταν μία από την ένδειξη κατανομές χρησιμοποιείται με τις λεπτομέρειες της ρύθμισης παραμέτρων όπως καθορίζονται στην περιοχή 'Υποστηρίζονται εκδόσεων' στη [Linux στην Azure-Endorsed κατανομές](virtual-machines-linux-endorsed-distros.md).


## <a name="create-a-resource-group"></a>Δημιουργήστε μια ομάδα πόρων
Ομάδες πόρων λογικά συνδυάζουν όλους τους πόρους Azure για την υποστήριξη του εικονικές μηχανές, όπως το εικονικό δίκτυο και χώρου αποθήκευσης. Διαβάστε περισσότερα σχετικά με τις [ομάδες Azure πόρου εδώ](../azure-resource-manager/resource-group-overview.md). Πριν από την αποστολή της εικόνας προσαρμοσμένης δίσκου και τη δημιουργία ΣΠΣ, πρέπει πρώτα να δημιουργήσετε μια ομάδα πόρων. 

Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` θέση:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης
ΣΠΣ αποθηκεύονται ως αντικείμενα BLOB σελίδας μέσα σε ένα λογαριασμό του χώρου αποθήκευσης. Διαβάστε περισσότερα σχετικά με το [χώρο αποθήκευσης αντικειμένων blob του Azure εδώ](../storage/storage-introduction.md#blob-storage). Μπορείτε να δημιουργήσετε ένα λογαριασμό χώρου αποθήκευσης για την εικόνα προσαρμοσμένης δίσκου και ΣΠΣ. Οποιαδήποτε ΣΠΣ που δημιουργείτε από την εικόνα σας προσαρμοσμένη δίσκου πρέπει να είναι στον ίδιο λογαριασμό χώρου αποθήκευσης ως αυτήν την εικόνα.

Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount` στην ομάδα πόρων που δημιουργήθηκε:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Πλήκτρα για το λογαριασμό χώρου αποθήκευσης λίστα
Azure δημιουργεί δύο πλήκτρα πρόσβασης 512 bit για κάθε λογαριασμό χώρου αποθήκευσης. Τα πλήκτρα πρόσβασης που χρησιμοποιούνται κατά τον έλεγχο ταυτότητας με το λογαριασμό χώρου αποθήκευσης, όπως για την εκτέλεση λειτουργιών εγγραφής. Διαβάστε περισσότερα σχετικά με [τη διαχείριση της πρόσβασης με το χώρο αποθήκευσης εδώ](../storage/storage-create-storage-account.md#manage-your-storage-account). Μπορείτε να προβάλετε τα πλήκτρα πρόσβασης με το `azure storage account keys list` εντολή.

Προβολή των πλήκτρων πρόσβασης για το λογαριασμό χώρου αποθήκευσης που δημιουργήσατε:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Το αποτέλεσμα είναι παρόμοια με:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Σημειώστε `key1` όπως θα μπορείτε να το χρησιμοποιήσετε για να αλληλεπιδράσετε με το λογαριασμό χώρου αποθήκευσης στα επόμενα βήματα.

## <a name="create-a-storage-container"></a>Δημιουργία κοντέινερ χώρου αποθήκευσης
Με τον ίδιο τρόπο που δημιουργείτε διαφορετικούς καταλόγους για να οργανώσετε λογικά τοπικό σύστημα αρχείων σας, μπορείτε να δημιουργήσετε κοντέινερ μέσα σε ένα λογαριασμό του χώρου αποθήκευσης για να οργανώσετε το εικονικό δίσκο και εικόνες. Ένα λογαριασμό του χώρου αποθήκευσης μπορεί να περιέχουν οποιονδήποτε αριθμό κοντέινερ. 

Το ακόλουθο παράδειγμα δημιουργεί ένα κοντέινερ που ονομάζεται `myimages`, που καθορίζει τον αριθμό-κλειδί πρόσβασης που λαμβάνονται στο προηγούμενο βήμα (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Αποστολή VHD
Τώρα μπορείτε να αποστείλετε την εικόνα σας προσαρμοσμένη δίσκου στην πραγματικότητα. Ως με όλα εικονικών δίσκων που χρησιμοποιούνται από ΣΠΣ, που αποστολή και αποθηκεύστε την εικόνα σας προσαρμοσμένη δίσκου ως blob σελίδας.

Καθορίστε το πλήκτρο πρόσβασης, το κοντέινερ που δημιουργήσατε στο προηγούμενο βήμα και, στη συνέχεια, τη διαδρομή προς την εικόνα προσαρμοσμένης δίσκου στον τοπικό σας υπολογιστή:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Δημιουργία Εικονική από προσαρμοσμένης εικόνας
Όταν δημιουργείτε ΣΠΣ από την εικόνα σας προσαρμοσμένη δίσκου, καθορίστε το URI στην εικόνα δίσκου. Βεβαιωθείτε ότι τις συμφωνίες λογαριασμού χώρου αποθήκευσης προορισμού όπου είναι αποθηκευμένο την εικόνα σας προσαρμοσμένη δίσκου. Μπορείτε να δημιουργήσετε το Εικονική χρησιμοποιώντας το πρότυπο Azure CLI ή JSON για τη διαχείριση πόρων.


### <a name="create-a-vm-using-the-azure-cli"></a>Δημιουργήστε μια Εικονική χρησιμοποιώντας το CLI Azure
Καθορίζετε το `--image-urn` παραμέτρου με το `azure vm create` εντολή για να τοποθετήστε το δείκτη στην εικόνα σας προσαρμοσμένο δίσκου. Βεβαιωθείτε ότι `--storage-account-name` συμφωνεί με το λογαριασμό χώρου αποθήκευσης όπου είναι αποθηκευμένο την εικόνα σας προσαρμοσμένο δίσκου. Δεν χρειάζεται να χρησιμοποιήσετε το ίδιο κοντέινερ ως η εικόνα προσαρμοσμένης δίσκου για την αποθήκευση του ΣΠΣ. Βεβαιωθείτε ότι έχετε δημιουργήσει τυχόν πρόσθετα κοντέινερ με τον ίδιο τρόπο όπως τα προηγούμενα βήματα πριν από την αποστολή τις εικόνες σας προσαρμοσμένη δίσκου.

Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική με το όνομα `myVM` από την εικόνα σας προσαρμοσμένη δίσκο:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Εξακολουθείτε να χρειάζεστε για να καθορίσετε ή να σας ζητηθεί από το answer για όλες τις πρόσθετες παραμέτρους που απαιτούνται από το `azure vm create` εντολής όπως εικονικού δικτύου, δημόσια διεύθυνση IP, όνομα χρήστη και SSH πλήκτρα. Διαβάστε περισσότερα σχετικά με τις [διαθέσιμες παραμέτρους διαχείρισης πόρων CLI](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Δημιουργήστε μια Εικονική χρησιμοποιώντας ένα πρότυπο JSON
Azure πρότυπα διαχείρισης πόρων είναι αρχεία σημειογραφίας αντικειμένων JavaScript (JSON) που καθορίζουν το περιβάλλον που θέλετε να δημιουργήσετε. Τα πρότυπα αναλύονται σε διαφορετικό πόρο παροχής όπως υπολογισμού ή στο δίκτυο. Μπορείτε να χρησιμοποιήσετε τα υπάρχοντα πρότυπα ή να γράψετε τις δικές σας. Διαβάστε περισσότερα σχετικά με τη [χρήση της διαχείρισης πόρων και πρότυπα](../azure-resource-manager/resource-group-overview.md).

Εντός του `Microsoft.Compute/virtualMachines` υπηρεσία παροχής του προτύπου σας, έχετε μια `storageProfile` κόμβο που περιέχει τις λεπτομέρειες της ρύθμισης παραμέτρων για την Εικονική. Είναι το κύριο δύο παραμέτρους για να επεξεργαστείτε το `image` και `vhd` URI που οδηγεί την εικόνα προσαρμοσμένης δίσκου σας και το νέο Εικονική εικονικού δίσκου. Ακολουθεί ένα παράδειγμα του JSON για τη χρήση μιας εικόνας προσαρμοσμένης δίσκου:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Μπορείτε να χρησιμοποιήσετε [αυτό το υπάρχον πρότυπο για να δημιουργήσετε μια Εικονική από μια προσαρμοσμένη εικόνα](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) ή να διαβάσετε σχετικά με [τη δημιουργία τα δικά σας πρότυπα διαχείρισης πόρων Azure](../resource-group-authoring-templates.md). 

Όταν έχετε ρυθμίσει τις παραμέτρους προτύπου, δημιουργείτε σας ΣΠΣ χρησιμοποιώντας το `azure group deployment create` εντολή. Καθορίστε το URI του προτύπου σας JSON με το `--template-uri` παραμέτρου:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Εάν έχετε ένα αρχείο JSON είναι τοπικά αποθηκευμένα στον υπολογιστή σας, μπορείτε να χρησιμοποιήσετε το `--template-file` παράμετρο αντί για αυτό:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Επόμενα βήματα
Αφού έχετε προετοιμασμένοι και το προσαρμοσμένο εικονικού δίσκου που έχουν αποσταλεί, μπορείτε να διαβάσετε περισσότερα σχετικά με τη [χρήση της διαχείρισης πόρων και πρότυπα](../azure-resource-manager/resource-group-overview.md). Μπορείτε επίσης να [προσθέσετε ένα δίσκο δεδομένων](virtual-machines-linux-add-disk.md) για το νέο ΣΠΣ. Εάν έχετε εφαρμογές που εκτελούνται σε σας ΣΠΣ που χρειάζεστε για να αποκτήσετε πρόσβαση, φροντίστε να [ανοίξετε θύρες και τα τελικά σημεία](virtual-machines-linux-nsg-quickstart.md).
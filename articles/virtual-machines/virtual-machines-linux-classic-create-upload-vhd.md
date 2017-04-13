<properties
    pageTitle="Δημιουργία και αποστολή ενός VHD Linux | Microsoft Azure"
    description="Δημιουργία και αποστολή ενός Azure εικονικού σκληρού δίσκου (VHD) με το μοντέλο κλασική ανάπτυξης που περιέχει το λειτουργικό σύστημα Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μπορείτε επίσης να [αποστείλετε μια εικόνα προσαρμοσμένης δίσκου χρησιμοποιώντας τη διαχείριση πόρων Azure](virtual-machines-linux-upload-vhd.md).

Σε αυτό το άρθρο σάς δείχνει πώς μπορείτε να δημιουργήσετε και να στείλετε μια εικονικού σκληρού δίσκου (VHD), ώστε να μπορείτε να το χρησιμοποιήσετε ως τη δική σας εικόνα για να δημιουργήσετε εικονικές μηχανές στο Azure. Μάθετε πώς να προετοιμάσετε το λειτουργικό σύστημα, ώστε να μπορείτε να το χρησιμοποιήσετε για να δημιουργήσετε πολλές εικονικές μηχανές που βασίζονται σε αυτήν την εικόνα. 

>  [AZURE.NOTE] Εάν έχετε λίγα λεπτά, Βοηθήστε μας να βελτιώσουμε την τεκμηρίωση Εικονική Linux Azure παρακολουθώντας αυτό [γρήγορης έρευνας](https://aka.ms/linuxdocsurvey) με την εμπειρία σας. Κάθε απάντηση βοηθούν στη Βοήθεια για να εργαστείτε.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο προϋποθέτει ότι έχετε τα ακόλουθα στοιχεία:

- **Λειτουργικό σύστημα εγκατεστημένο σε ένα αρχείο .vhd Linux** - που έχετε εγκαταστήσει μια [κατανομή θεωρηθεί Azure Linux](virtual-machines-linux-endorsed-distros.md) (ή ανατρέξτε στο θέμα [πληροφορίες για μη θεωρηθεί κατανομές](virtual-machines-linux-create-upload-generic.md)) σε ένα εικονικό δίσκο με τη μορφή VHD. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε μια Εικονική και VHD:
    - Εγκατάσταση και ρύθμιση παραμέτρων [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) ή [KVM](http://www.linux-kvm.org/page/RunningKVM), φροντίζοντας να χρησιμοποιήσετε VHD ως μορφή εικόνας. Εάν είναι απαραίτητο, μπορείτε να [μετατρέψετε μια εικόνα](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) χρησιμοποιώντας `qemu-img convert`.
    - Μπορείτε επίσης να χρησιμοποιήσετε το Hyper-V [στα Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) ή [στο Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Στη νεότερη μορφή VHDX δεν υποστηρίζεται στο Azure. Όταν δημιουργείτε μια Εικονική, καθορίστε VHD ως μορφή. Εάν είναι απαραίτητο, μπορείτε να μετατρέψετε δίσκων VHDX με τη χρήση του VHD [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ή το [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) cmdlet του PowerShell. Επιπλέον, Azure δεν υποστηρίζει αποστολή δυναμικής VHD, ώστε να πρέπει να μετατρέψετε αυτές δίσκων στατική VHD πριν από την αποστολή. Μπορείτε να χρησιμοποιήσετε εργαλεία όπως το [Azure VHD βοηθητικά προγράμματα για ΜΕΤΆΒΑΣΗ](https://github.com/Microsoft/azure-vhd-utils-for-go) για να μετατρέψετε δυναμικών δίσκων κατά τη διαδικασία Αποστολή σε Azure.

- **Περιβάλλον γραμμής εντολών azure** - εγκαταστήστε την πιο πρόσφατη [Azure περιβάλλον γραμμής εντολών](../virtual-machines-command-line-tools.md) για την αποστολή του VHD.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Βήμα 1: Προετοιμασία την εικόνα που θα αποσταλεί

Azure υποστηρίζει διάφορες Linux κατανομές (ανατρέξτε στο θέμα [Θεωρηθεί κατανομές](virtual-machines-linux-endorsed-distros.md)). Τα ακόλουθα άρθρα σας καθοδηγήσει πώς να προετοιμάσετε το διάφορες κατανομές Linux που υποστηρίζονται σε Azure. Αφού ολοκληρώσετε τα βήματα στο θέμα τους παρακάτω οδηγούς, προέρχονται πίσω εδώ όταν έχετε ένα αρχείο VHD που είναι έτοιμο για αποστολή στο Azure:

- **[Διανομή βάσει centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Κόκκινο καπέλο Linux για μεγάλες επιχειρήσεις](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Άλλα - κατανομές μη θεωρηθεί](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Της πλατφόρμας Azure SLA εφαρμόζεται σε εικονικές μηχανές χρησιμοποιεί το λειτουργικό σύστημα Linux μόνο όταν μία από την ένδειξη κατανομές χρησιμοποιείται με τις λεπτομέρειες της ρύθμισης παραμέτρων όπως καθορίζονται στην περιοχή 'Υποστηρίζονται εκδόσεων' στη [Linux στην Azure-Endorsed κατανομές](virtual-machines-linux-endorsed-distros.md). Όλα κατανομές Linux στη συλλογή Azure εικόνα είναι ένδειξη κατανομές με τις απαιτούμενες ρυθμίσεις.

Ανατρέξτε επίσης στις **[Σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** για πιο γενικές συμβουλές σχετικά με την προετοιμασία Linux εικόνες για Azure.


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Βήμα 2: Προετοιμασία της σύνδεσης με Azure

Βεβαιωθείτε ότι χρησιμοποιείτε το Azure CLI στο μοντέλο κλασική ανάπτυξης (`azure config mode asm`), στη συνέχεια, συνδεθείτε λογαριασμό σας:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Βήμα 3: Αποστολή της εικόνας Azure

Χρειάζεστε ένα λογαριασμό χώρου αποθήκευσης για να αποστείλετε το αρχείο VHD. Μπορείτε είτε να επιλέξετε έναν υπάρχοντα λογαριασμό χώρου αποθήκευσης ή [Δημιουργήστε ένα νέο](../storage/storage-create-storage-account.md).

Χρησιμοποιήστε το CLI Azure για να αποστείλετε την εικόνα, χρησιμοποιώντας την ακόλουθη εντολή:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Στο προηγούμενο παράδειγμα:

- **BlobStorageURL** είναι η διεύθυνση URL για το λογαριασμό χώρου αποθήκευσης που σχεδιάζετε να χρησιμοποιήσετε
- **YourImagesFolder** είναι το κοντέινερ μέσα αποθήκευσης αντικειμένων blob όπου θέλετε να αποθηκεύσετε τις εικόνες σας
- **VHDName** είναι η ετικέτα που εμφανίζεται στην πύλη για τον προσδιορισμό του εικονικού σκληρού δίσκου.
- **PathToVHDFile** είναι η πλήρης διαδρομή και το όνομα του αρχείου .vhd στον υπολογιστή σας.

Ακολουθεί ένα παράδειγμα ολοκληρωθεί:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Βήμα 4: Δημιουργήστε μια Εικονική από την εικόνα
Μπορείτε να δημιουργήσετε μια Εικονική χρησιμοποιώντας `azure vm create` με τον ίδιο τρόπο ως μια κανονική Εικονική. Καθορίστε το όνομα που έχετε δώσει την εικόνα σας στο προηγούμενο βήμα. Στο παρακάτω παράδειγμα, μπορούμε να χρησιμοποιήσουμε το όνομα εικόνας **UbuntuLTS** που δίνεται στο προηγούμενο βήμα:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Για να δημιουργήσετε το δικό σας ΣΠΣ, δώστε το δικό σας όνομα χρήστη + κωδικός πρόσβασης, θέση, όνομα DNS και όνομα εικόνας.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αναφορά Azure CLI για το μοντέλο Azure κλασική ανάπτυξης](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload

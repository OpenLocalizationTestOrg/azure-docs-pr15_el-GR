<properties
    pageTitle="Δημιουργία και αποστολή ενός VHD Linux στο Azure"
    description="Μάθετε πώς να δημιουργήσετε και να στείλετε ένα Azure εικονικού σκληρού δίσκου (VHD) που περιέχει ένα λειτουργικό σύστημα Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="szark"/>

# <a name="information-for-non-endorsed-distributions"></a>Πληροφορίες για μη θεωρηθεί κατανομές #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


**ΣΗΜΑΝΤΙΚΟ**: το Azure πλατφόρμα SLA ισχύει για εικονικές μηχανές χρησιμοποιεί το λειτουργικό σύστημα Linux μόνο όταν ένα από τα [θεωρηθεί κατανομές](virtual-machines-linux-endorsed-distros.md) χρησιμοποιείται. Όλες οι κατανομές Linux που παρέχονται στη συλλογή Azure εικόνα είναι ένδειξη κατανομές με τις απαιτούμενες ρυθμίσεις.

- [Linux στην Azure - θεωρηθεί κατανομές](virtual-machines-linux-endorsed-distros.md)
- [Υποστήριξη για τις εικόνες Linux Microsoft Azure](https://support.microsoft.com/kb/2941892)

Όλα κατανομές εκτελείται σε Azure θα πρέπει να πληρούν ορισμένες προϋποθέσεις για να έχετε τη δυνατότητα να εκτελείται σωστά σε της πλατφόρμας.  Σε αυτό το άρθρο με means δεν είναι ολοκληρωμένη, όπως κάθε κατανομής είναι διαφορετικό; και είναι αρκετά πιθανό ότι ακόμα και αν πληρούνται όλα τα παρακάτω κριτήρια θα εξακολουθεί να πρέπει να σημαντικά τροποποιήστε το σύστημα Linux για να εξασφαλίσετε ότι το εκτελείται σωστά στην πλατφόρμα.

Πρόκειται για αυτόν το λόγο που συνιστάται που θα ξεκινήσετε με μία από μας [Linux στην κατανομές θεωρηθεί Azure](virtual-machines-linux-endorsed-distros.md) όταν είναι δυνατό. Τα ακόλουθα άρθρα θα σας καθοδηγήσει πώς να προετοιμάσετε το διάφορες ένδειξη Linux κατανομές που υποστηρίζονται σε Azure:

- **[Διανομή βάσει centOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Κόκκινο καπέλο Linux για μεγάλες επιχειρήσεις](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**

Τα υπόλοιπα σε αυτό το άρθρο εστιάζει στην γενικές οδηγίες για την εκτέλεση του Linux διανομής σε Azure.


## <a name="general-linux-installation-notes"></a>Σημειώσεις εγκατάστασης Linux Γενικά ##

- Η μορφή VHDX δεν υποστηρίζεται στο Azure, μόνο **σταθερής VHD**.  Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το cmdlet vhd μετατροπή. Εάν χρησιμοποιείτε VirtualBox αυτό σημαίνει ότι η επιλογή **σταθερό μέγεθος** αντί για την προεπιλεγμένη εκχωρηθεί δυναμικά κατά τη δημιουργία του δίσκου.

- Κατά την εγκατάσταση του συστήματος Linux είναι *συνιστάται* να χρησιμοποιείτε βασικά διαμερίσματα και όχι LVM (συχνά η προεπιλογή για πολλές εγκαταστάσεις). Αυτό θα αποφύγετε LVM όνομα έρχεται σε διένεξη με κλωνοποιημένο ΣΠΣ, ιδίως εάν ένα δίσκο OS πρέπει ποτέ να επισυναφθεί σε μια άλλη πανομοιότυπες Εικονική για την αντιμετώπιση προβλημάτων. Μπορεί να χρησιμοποιηθεί [LVM](virtual-machines-linux-configure-lvm.md) ή [RAID](virtual-machines-linux-configure-raid.md) δίσκων δεδομένων.

- Απαιτείται υποστήριξη πυρήνα προσαρμογής συστήματα αρχείων UDF. Κατά την πρώτη εκκίνηση στο Azure τη ρύθμιση παραμέτρων παροχής που του μεταβιβάστηκε η Εικονική Linux μέσω μορφή UDF πολυμέσων που συνδέεται με το επισκέπτη. Ο παράγοντας Azure Linux πρέπει να μπορούν να το σύστημα αρχείων UDF ανάγνωση της ρύθμισης παραμέτρων και παροχή η Εικονική ενεργοποίησης.

- Εκδόσεις πυρήνα Linux κάτω από το στοιχείο 2.6.37 δεν υποστηρίζουν NUMA σε Hyper-V με μεγαλύτερα μεγέθη Εικονική. Αυτό το θέμα κυρίως αντίκτυπο παλαιότερων κατανομές χρησιμοποιώντας τη νέα κόκκινο καπέλο 2.6.32 πυρήνα, και έχει καθοριστεί σε RHEL 6.6 (πυρήνα-2.6.32-504). Συστήματα που εκτελούν το προσαρμοσμένο πυρήνων παλιότερα από 2.6.37 ή βάσει RHEL πυρήνων παλαιότερων από 2.6.32-504 πρέπει να ορίσετε την παράμετρο εκκίνησης `numa=off` στον πυρήνα των εντολών στο grub.conf. Για περισσότερες πληροφορίες ανατρέξτε στο θέμα καπέλο κόκκινο [KB 436883](https://access.redhat.com/solutions/436883).

- Χωρίς ρύθμιση ένα διαμερίσματα αντιμετάθεσης στο δίσκο OS. Ο παράγοντας Linux μπορεί να ρυθμιστεί για να δημιουργήσετε ένα αρχείο αντιμετάθεσης στο δίσκο προσωρινών πόρων.  Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.

- Όλα τα VHD πρέπει να έχετε τα μεγέθη που είναι πολλαπλάσιο του 1 MB.


### <a name="installing-linux-without-hyper-v"></a>Κατά την εγκατάσταση Linux χωρίς Hyper-V ###

Σε ορισμένες περιπτώσεις, προγράμματα εγκατάστασης Linux ενδέχεται να περιλαμβάνουν τα προγράμματα οδήγησης για Hyper-V στο στο αρχικό ramdisk (initrd ή initramfs), εκτός αν εντοπίσει ότι εκτελείται ένα περιβάλλον Hyper-V.  Όταν χρησιμοποιείτε ένα σύστημα διαφορετικό virtualization (δηλαδή Virtualbox, KVM, κ.λπ.) για να προετοιμάσετε την εικόνα σας Linux, ίσως χρειαστεί να αναδημιουργήσετε το initrd για να βεβαιωθείτε ότι τουλάχιστον το `hv_vmbus` και `hv_storvsc` λειτουργικές μονάδες πυρήνα είναι διαθέσιμες στην στο αρχικό ramdisk.  Πρόκειται για ένα γνωστό θέμα τουλάχιστον σε συστήματα με βάση την επόμενη κατανομή καπέλο κόκκινο χρώμα.

Ο μηχανισμός φτιάχνετε η εικόνα initrd ή initramfs ενδέχεται να ποικίλλουν ανάλογα με την κατανομή. Συμβουλευτείτε την κατανομή τεκμηρίωση ή υποστήριξης για την κατάλληλη διαδικασία.  Ακολουθεί ένα παράδειγμα για το πώς μπορείτε να δημιουργήσετε ξανά το initrd χρησιμοποιώντας το `mkinitrd` βοηθητικού προγράμματος:

Πρώτα, δημιουργήστε αντίγραφο ασφαλείας την υπάρχουσα εικόνα initrd:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Στη συνέχεια, δημιουργήστε ξανά το initrd με το `hv_vmbus` και `hv_storvsc` λειτουργικές μονάδες πυρήνα:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Αλλαγή μεγέθους VHD ###

Εικόνες VHD Azure πρέπει να έχετε ένα εικονικό μέγεθος στοιχισμένα 1MB.  Συνήθως, VHD που δημιουργήθηκε με το Hyper-V πρέπει ήδη στοιχιστεί σωστά.  Εάν το VHD δεν είναι στοιχισμένο σωστά, στη συνέχεια, ενδέχεται να λαμβάνετε παρόμοιο με το ακόλουθο μήνυμα σφάλματος όταν προσπαθείτε να δημιουργήσετε μια *εικόνα* από το VHD:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Για την αντιμετώπιση αυτό μπορείτε να αλλάξετε το μέγεθος την εικονική Μηχανή χρησιμοποιώντας την Κονσόλα διαχείρισης Hyper-V ή το cmdlet του Powershell [VHD αλλαγής μεγέθους](http://technet.microsoft.com/library/hh848535.aspx) .  Εάν δεν χρησιμοποιείτε σε ένα περιβάλλον των Windows, στη συνέχεια, καλό είναι να χρησιμοποιήσετε qemu img για να μετατρέψετε (εάν χρειάζεται) και να αλλάξετε το μέγεθος του VHD.

> [AZURE.NOTE] Υπάρχει ένα σφάλμα γνωστά σε εκδόσεις qemu img > = 2.2.1 που καταλήγει σε μια εσφαλμένη μορφοποίηση VHD. Το ζήτημα έχει διορθωθεί στο QEMU 2.6. Συνιστάται να χρησιμοποιήσετε qemu-img 2.2.0 ή κάτω, ή να ενημερώσετε να 2.6 ή μεγαλύτερη. Αναφορά: https://bugs.launchpad.net/qemu/+bug/1490611.


 1. Αλλαγή μεγέθους του VHD απευθείας με εργαλεία όπως `qemu-img` ή `vbox-manage` μπορεί να έχει ως αποτέλεσμα ένα VHD αδυναμία εκκίνησης.  Επομένως, συνιστάται να πρώτη μετατροπή VHD σε μια εικόνα ΑΝΕΠΕΞΈΡΓΑΣΤΑ δίσκου.  Εάν η Εικονική εικόνα έχει ήδη δημιουργήσει ως ΑΝΕΠΕΞΈΡΓΑΣΤΟ δίσκου εικόνας (η προεπιλογή για ορισμένες υπερεπόπτες όπως KVM), στη συνέχεια, μπορείτε να παραλείψετε αυτό το βήμα:

        # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

 2. Υπολογίστε το απαιτούμενο μέγεθος της εικόνας δίσκου για να βεβαιωθείτε ότι το μέγεθος του εικονικού ευθυγραμμίζεται με 1MB.  Η ακόλουθη δέσμη ενεργειών κελύφους πάρτι μπορεί να σας βοηθήσει με αυτό.  Η δέσμη ενεργειών χρησιμοποιεί "`qemu-img info`" για να προσδιορίσετε το εικονικό μέγεθος της εικόνας δίσκου και, στη συνέχεια, υπολογίζει το μέγεθος για το επόμενο 1MB:

        rawdisk="MyLinuxVM.raw"
        vhddisk="MyLinuxVM.vhd"

        MB=$((1024*1024))
        size=$(qemu-img info -f raw --output json "$rawdisk" | \
               gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        rounded_size=$((($size/$MB + 1)*$MB))
        echo "Rounded Size = $rounded_size"

 3. Αλλαγή του μεγέθους του ανεπεξέργαστα δίσκου χρησιμοποιώντας $rounded_size όπως στο παραπάνω δέσμης ενεργειών:

        # qemu-img resize MyLinuxVM.raw $rounded_size

 4. Μετατρέψτε το ΑΝΕΠΕΞΈΡΓΑΣΤΟ δίσκο Επιστροφή στην VHD σταθερού πλάτους:

        # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd



## <a name="linux-kernel-requirements"></a>Απαιτήσεις Linux πυρήνα ##

Τα προγράμματα οδήγησης υπηρεσίες ενοποίησης Linux (LIS) για το Hyper-V και Azure παρέχονται απευθείας με την επόμενη πυρήνα Linux. Πολλά κατανομές που περιλαμβάνουν μια πρόσφατη έκδοση πυρήνα Linux (δηλαδή 3.x) θα έχετε ήδη αυτά τα προγράμματα οδήγησης που είναι διαθέσιμες ή παρέχετε με άλλο τρόπο backported εκδόσεις αυτών των προγραμμάτων οδήγησης με τους πυρήνων.  Αυτά τα προγράμματα οδήγησης ενημερώνονται συνεχώς στην επόμενη πυρήνα με νέες ενημερώσεις κώδικα και τις δυνατότητες, έτσι ώστε όταν είναι δυνατό συνιστάται να εκτελέσετε μια [θεωρηθεί διανομής](virtual-machines-linux-endorsed-distros.md) που θα περιλαμβάνει τα παρακάτω επιδιορθώσεις και ενημερώσεις.

Εάν εκτελείτε μια παραλλαγή κόκκινο καπέλο Enterprise Linux εκδόσεις **6.0 6.3**, στη συνέχεια, θα πρέπει να εγκαταστήσετε τα πιο πρόσφατα προγράμματα οδήγησης LIS για το Hyper-V. Μπορείτε να βρείτε τα προγράμματα οδήγησης [σε αυτήν τη θέση](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Το RHEL **6.4 +** (και προϊόντα) τα προγράμματα οδήγησης LIS περιλαμβάνονται ήδη με πυρήνα και επομένως χωρίς επιπλέον εγκατάστασης πακέτων χρειάζονται για να εκτελέσετε αυτά τα συστήματα στην Azure.

Εάν απαιτείται μια προσαρμοσμένη πυρήνα, συνιστάται να χρησιμοποιήσετε μια πιο πρόσφατη έκδοση πυρήνα (δηλαδή **3.8 +**). Για αυτές τις κατανομές ή που διατηρεί τις δικές τους πυρήνα, θα ορισμένες προσπάθειας απαιτείται για την ενημέρωση τακτικά τα προγράμματα οδήγησης LIS από την επόμενη πυρήνα το προσαρμοσμένο πυρήνα.  Ακόμα και εάν διαθέτετε ήδη μια σχετικά πρόσφατη έκδοση πυρήνα, συνιστάται ιδιαίτερα να παρακολουθείτε οποιαδήποτε νέα επιδιορθώσεις στα προγράμματα οδήγησης LIS και ενημέρωση αυτές σύμφωνα με τις ανάγκες. Η θέση των αρχείων προέλευσης του προγράμματος οδήγησης LIS είναι διαθέσιμη στο αρχείο [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) στο δέντρο Linux πυρήνα προέλευση:

    F:  arch/x86/include/asm/mshyperv.h
    F:  arch/x86/include/uapi/asm/hyperv.h
    F:  arch/x86/kernel/cpu/mshyperv.c
    F:  drivers/hid/hid-hyperv.c
    F:  drivers/hv/
    F:  drivers/input/serio/hyperv-keyboard.c
    F:  drivers/net/hyperv/
    F:  drivers/scsi/storvsc_drv.c
    F:  drivers/video/fbdev/hyperv_fb.c
    F:  include/linux/hyperv.h
    F:  tools/hv/

Στο ένα πολύ ελάχιστη, απουσία τις ακόλουθες ενημερωμένες εκδόσεις κώδικα είναι γνωστό προκαλούν προβλήματα στην Azure και επομένως αυτές πρέπει να συμπεριλαμβάνονται στον πυρήνα. Αυτήν τη λίστα με means δεν είναι εκτεταμένη ή ολοκλήρωσης για όλους κατανομές:

- [ata_piix: αναβάλετε δίσκων με τα προγράμματα οδήγησης Hyper-V από προεπιλογή](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
- [storvsc: λογαριασμού για πακέτα μεταφοράς στη διαδρομή ΕΠΑΝΑΦΟΡΆ](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
- [storvsc: αποφύγετε τη χρήση της WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
- [storvsc: Απενεργοποίηση ΓΡΆΨΕΤΕ ΊΔΙΑ για RAID και τα προγράμματα οδήγησης προσαρμογέα εικονικού κεντρικού υπολογιστή](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
- [storvsc: δείκτης NULL διακοπής αναφοράς fix](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
- [storvsc: κλήση buffer αποτυχίες ενδέχεται να οδηγήσει σε σταθεροποίηση εισόδου/εξόδου](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
- [scsi_sysfs: προστασία από δύο εκτέλεσης της __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)


## <a name="the-azure-linux-agent"></a>Ο παράγοντας Linux Azure ##

Ο [Παράγοντας Linux Azure](virtual-machines-linux-agent-user-guide.md) (waagent) απαιτείται για την παροχή σωστά μια εικονική μηχανή Linux στο Azure. Μπορείτε να λάβετε την πιο πρόσφατη έκδοση, το αρχείο θέματα ή να υποβάλετε αιτήσεις ελκυστική στο το [repo GitHub παράγοντας Linux](https://github.com/Azure/WALinuxAgent).

- Ο παράγοντας Linux έχει κυκλοφορήσει κάτω από την άδεια χρήσης Apache 2.0. Πολλά κατανομές ήδη παρέχουν RPM ή deb πακέτων για τον παράγοντα και, επομένως, σε ορισμένες περιπτώσεις αυτό μπορεί να είναι εγκατεστημένο και ενημερώνονται με μικρή προσπάθεια.

- Ο παράγοντας Linux Azure απαιτεί Python v2.6 +.

- Ο παράγοντας απαιτεί επίσης τη λειτουργική μονάδα python pyasn1. Οι περισσότερες κατανομές παρέχουν αυτό ως ξεχωριστό πακέτο που μπορεί να εγκατασταθεί.

- Σε ορισμένες περιπτώσεις τον παράγοντα Linux Azure ενδέχεται να μην είναι συμβατά με NetworkManager. Πολλά από τα πακέτα RPM/Deb που παρέχεται από κατανομές ρύθμιση παραμέτρων NetworkManager ως διένεξη στο πακέτο waagent και, επομένως, θα καταργήσετε την εγκατάσταση του NetworkManager κατά την εγκατάσταση του πακέτου παράγοντας Linux.


## <a name="general-linux-system-requirements"></a>Απαιτήσεις συστήματος Linux Γενικά ##

- Τροποποίηση της γραμμής εκκίνησης πυρήνα στο ΕΚΣΚΑΦΉ ή GRUB2 για να συμπεριλάβετε τις παρακάτω παραμέτρους. Αυτό θα εξασφαλίσει επίσης όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα:

        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300

    Αυτό θα εξασφαλίσει επίσης όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα.

    Εκτός από τα παραπάνω, συνιστάται να *καταργήσετε* τις ακόλουθες παραμέτρους εάν υπάρχουν:

        rhgb quiet crashkernel=auto

    Δεν είναι χρήσιμη σε ένα περιβάλλον cloud όπου θέλουμε όλα τα αρχεία καταγραφής θα αποσταλούν στη σειριακή θύρα γραφικών και ήσυχο εκκίνησης.

    Το `crashkernel` επιλογή ενδέχεται να έχει ρυθμιστεί εάν θέλετε τα αριστερά, αλλά Σημειώστε ότι αυτή η παράμετρος θα μείωση της ποσότητας διαθέσιμη μνήμη σε η Εικονική από 128MB ή μεγαλύτερη, που μπορεί να είναι προβληματικό τα μικρότερα μεγέθη Εικονική.

- Κατά την εγκατάσταση του Azure Linux παράγοντα

    Ο παράγοντας Linux Azure απαιτείται για την παροχή μια εικόνα Linux σε Azure.  Πολλά κατανομές παρέχουν τον παράγοντα ως ένα πακέτο RPM ή Deb (το πακέτο συνήθως ονομάζεται 'WALinuxAgent' ή 'walinuxagent').  Ο παράγοντας μπορεί επίσης να εγκατασταθεί με μη αυτόματο τρόπο, ακολουθώντας τα βήματα στον [Οδηγό παράγοντας Linux](virtual-machines-linux-agent-user-guide.md).

- Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

- Δεν δημιουργήσετε αντιμετάθεσης χώρο στο δίσκο λειτουργικό σύστημα

    Ο παράγοντας Linux Azure αυτόματα να ρυθμίσετε τις παραμέτρους χώρος αντιμετάθεσης χρησιμοποιώντας το δίσκο τοπικός πόρος που συνδέεται με την εικονική Μηχανή μετά την προμήθεια του Azure. Σημειώστε ότι ο δίσκος τοπικός πόρος είναι ένα *προσωρινό* δίσκο και να αδειάζετε όταν η Εικονική καταργείται. Μετά την εγκατάσταση του τον παράγοντα Linux Azure (ανατρέξτε στο προηγούμενο βήμα), τροποποιήστε κατάλληλα τις ακόλουθες παραμέτρους στην /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

- Ως τελικό βήμα, εκτελέστε τις ακόλουθες εντολές για να deprovision η εικονική μηχανή:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

    >[AZURE.NOTE] Στην Virtualbox μπορεί να δείτε το ακόλουθο σφάλμα όταν εκτελείται το ' waagent-επιβάλετε - διαχειριστείτε ': `[Errno 5] Input/output error`. Αυτό το μήνυμα σφάλματος δεν είναι κρίσιμης και μπορεί να αγνοηθεί.

- Μπορείτε, στη συνέχεια, θα χρειαστεί για να τερματίσετε την εικονική μηχανή και αποστολή του VHD Azure.

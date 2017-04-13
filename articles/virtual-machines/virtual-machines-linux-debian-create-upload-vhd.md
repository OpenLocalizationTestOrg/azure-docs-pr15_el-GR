<properties
    pageTitle="Προετοιμασία Debian Linux VHD | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε Debian 7 και 8 αρχεία VHD για ανάπτυξη στο Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="szark"/>



# <a name="prepare-a-debian-vhd-for-azure"></a>Προετοιμασία Debian VHD για Azure

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Αυτή η ενότητα προϋποθέτει ότι έχετε εγκαταστήσει ήδη ένα λειτουργικό σύστημα Debian Linux από ένα αρχείο .iso λήψη από την [τοποθεσία Web του Debian](https://www.debian.org/distrib/) στο εικονικού σκληρού δίσκου. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε αρχεία .vhd. Το Hyper-V είναι μόνο ένα παράδειγμα. Για οδηγίες χρησιμοποιώντας το Hyper-V, ανατρέξτε στο θέμα [εγκατάσταση του ρόλου Hyper-V και ρύθμιση παραμέτρων μια εικονική μηχανή](https://technet.microsoft.com/library/hh846766.aspx).


## <a name="installation-notes"></a>Σημειώσεις εγκατάστασης

- Ανατρέξτε στο θέμα επίσης [Γενικές σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) για περισσότερες συμβουλές σχετικά με την προετοιμασία Linux για Azure.
- Στη νεότερη μορφή VHDX δεν υποστηρίζεται στο Azure. Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το cmdlet **vhd μετατροπή** .
- Κατά την εγκατάσταση του συστήματος Linux συνιστάται να χρησιμοποιήσετε βασικά διαμερίσματα και όχι LVM (συχνά η προεπιλογή για πολλές εγκαταστάσεις). Αυτό θα αποφύγετε LVM όνομα έρχεται σε διένεξη με κλωνοποιημένο ΣΠΣ, ιδίως εάν ένα δίσκο OS πρέπει ποτέ να επισυναφθεί σε μια άλλη Εικονική για την αντιμετώπιση προβλημάτων. [LVM](virtual-machines-linux-configure-lvm.md) ή [RAID](virtual-machines-linux-configure-raid.md) μπορεί να χρησιμοποιηθεί σε δίσκων δεδομένων εάν προτιμώμενη.
- Χωρίς ρύθμιση ένα διαμερίσματα αντιμετάθεσης στο δίσκο OS. Ο παράγοντας Azure Linux μπορεί να ρυθμιστεί για να δημιουργήσετε ένα αρχείο αντιμετάθεσης στο δίσκο προσωρινών πόρων. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.
- Όλα τα VHD πρέπει να έχετε τα μεγέθη που είναι πολλαπλάσιο του 1 MB.


## <a name="use-azure-manage-to-create-debian-vhds"></a>Χρησιμοποιήστε τη Διαχείριση Azure για να δημιουργήσετε Debian VHD

Υπάρχουν εργαλεία για τη δημιουργία Debian VHD για Azure, όπως το [azure-Διαχείριση](https://github.com/credativ/azure-manage) δέσμες ενεργειών από [credativ](http://www.credativ.com/). Αυτή είναι η προτεινόμενη προσέγγιση σε σχέση με τη δημιουργία μιας εικόνας από την αρχή. Για παράδειγμα, για να δημιουργήσετε έναν Debian VHD 8, εκτελέστε τις ακόλουθες εντολές για να κάνετε λήψη azure-Διαχείριση (και εξαρτήσεις) και να εκτελέσετε τη δέσμη ενεργειών azure_build_image:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Προετοιμασία με μη αυτόματο τρόπο Debian VHD

1. Στη Διαχείριση Hyper-V, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε ένα παράθυρο κονσόλας για την εικονική μηχανή.

3. Σχολιάζετε γραμμής για **deb cdrom** στο `/etc/apt/source.list` Εάν ρυθμίσετε το Εικονική σε σχέση με ένα αρχείο ISO.

4. Επεξεργασία του `/etc/default/grub` αρχείου και να τροποποιήσετε την παράμετρο **GRUB_CMDLINE_LINUX** ως εξής, για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure.

        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"

5. Εκ νέου δημιουργία του εκσκαφή και να εκτελέσετε:

        # sudo update-grub

6. Προσθήκη του Debian Azure αποθετήρια /etc/apt/sources.list για Debian 7 ή 8:

    **Debian 7.x "Wheezy"**

        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main


    **Debian 8.x "Jessie"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


7. Εγκαταστήστε το Azure παράγοντας Linux:

        # sudo apt-get update
        # sudo apt-get install waagent

8. Για Debian 7, απαιτείται για την εκτέλεση του πυρήνα βάσει 3.16 από το χώρο αποθήκευσης wheezy backports. Πρώτα, δημιουργήστε ένα αρχείο που ονομάζεται /etc/apt/preferences.d/linux.pref με το παρακάτω περιεχόμενο:

        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500

    Στη συνέχεια, εκτέλεση "sudo κατάλληλη λήψη εγκατάσταση linux-εικόνα-amd64" για να εγκαταστήσετε το νέο πυρήνα.

8. Deprovision η εικονική μηχανή και προετοιμασία του για την παροχή σε Azure και εκτελέστε:

        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout

9. Κάντε κλικ στην **ενέργεια** -> Τερματισμός λειτουργίας προς τα κάτω στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.


## <a name="next-steps"></a>Επόμενα βήματα

Τώρα είστε έτοιμοι να χρησιμοποιήσετε Debian εικονικό σκληρό σας δίσκο για να δημιουργήσετε νέα εικονικές μηχανές στο Azure. Εάν αυτή είναι η πρώτη φορά που που αποστέλλετε το αρχείο .vhd να Azure, δείτε τα βήματα 2 και 3 στη [Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux](virtual-machines-linux-classic-create-upload-vhd.md).

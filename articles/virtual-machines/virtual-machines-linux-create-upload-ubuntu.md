<properties
    pageTitle="Δημιουργία και αποστολή ενός VHD Linux Ubuntu στο Azure"
    description="Μάθετε πώς να δημιουργήσετε και να στείλετε ένα Azure εικονικού σκληρού δίσκου (VHD) που περιέχει ένα λειτουργικό σύστημα Ubuntu Linux."
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
    ms.date="08/24/2016"
    ms.author="szark"/>

# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Προετοιμασία μια εικονική μηχανή Ubuntu για Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Επίσημη Ubuntu cloud εικόνες
Ubuntu δημοσιεύει τώρα επίσημη Azure VHD για λήψη στην [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Εάν χρειάζεστε για να δημιουργήσετε τη δική σας εικόνα Ubuntu εξειδικευμένες για Azure, προτιμάτε ό, χρησιμοποιήστε την παρακάτω διαδικασία μη αυτόματης καλό είναι να ξεκινήσετε με αυτά τα γνωστά εργασία VHD και να προσαρμόσετε σύμφωνα με τις ανάγκες. Πάντα μπορείτε να βρείτε τις πιο πρόσφατες εκδόσεις εικόνα στις ακόλουθες θέσεις:

 - Ubuntu 12.04/ακριβείς: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/precise/release/ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
 - Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Σε αυτό το άρθρο προϋποθέτει ότι έχετε εγκαταστήσει ήδη σε λειτουργικό σύστημα Ubuntu Linux σε εικονικό σκληρό δίσκο. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε αρχεία .vhd, για παράδειγμα, μια λύση virtualization όπως το Hyper-V. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση του ρόλου Hyper-V και ρύθμιση παραμέτρων μια εικονική μηχανή](http://technet.microsoft.com/library/hh846766.aspx).

**Σημειώσεις εγκατάστασης ubuntu**

- Ανατρέξτε στο θέμα επίσης [Γενικές σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) για περισσότερες συμβουλές σχετικά με την προετοιμασία Linux για Azure.
- Η μορφή VHDX δεν υποστηρίζεται στο Azure, μόνο **σταθερής VHD**.  Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το cmdlet vhd μετατροπή.
- Κατά την εγκατάσταση του συστήματος Linux συνιστάται να χρησιμοποιήσετε βασικά διαμερίσματα και όχι LVM (συχνά η προεπιλογή για πολλές εγκαταστάσεις). Αυτό θα αποφύγετε LVM όνομα έρχεται σε διένεξη με κλωνοποιημένο ΣΠΣ, ιδίως εάν ένα δίσκο OS πρέπει ποτέ να επισυναφθεί σε μια άλλη Εικονική για την αντιμετώπιση προβλημάτων. [LVM](virtual-machines-linux-configure-lvm.md) ή [RAID](virtual-machines-linux-configure-raid.md) μπορεί να χρησιμοποιηθεί σε δίσκων δεδομένων εάν προτιμώμενη.
- Χωρίς ρύθμιση ένα διαμερίσματα αντιμετάθεσης στο δίσκο OS. Ο παράγοντας Linux μπορεί να ρυθμιστεί για να δημιουργήσετε ένα αρχείο αντιμετάθεσης στο δίσκο προσωρινών πόρων.  Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.
- Όλα τα VHD πρέπει να έχετε τα μεγέθη που είναι πολλαπλάσιο του 1 MB.


## <a name="manual-steps"></a>Μη αυτόματα βήματα

> [AZURE.NOTE] Πριν να δημιουργήσετε τη δική σας προσαρμοσμένη εικόνα Ubuntu για Azure, εξετάστε το ενδεχόμενο χρήσης τις εικόνες από [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) αντί για αυτό.


1. Στο κεντρικό παράθυρο του Hyper-V Manager, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε το παράθυρο για την εικονική μηχανή.

3.  Αντικαταστήστε την τρέχουσα αποθετήρια στην εικόνα για να χρησιμοποιήσετε Ubuntu του Azure repos. Τα βήματα διαφέρει λίγο ανάλογα με την έκδοση Ubuntu.

    Πριν από την επεξεργασία /etc/apt/sources.list, συνιστάται να δημιουργήσετε ένα αντίγραφο ασφαλείας:

        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:

        # sudo sed -i "s/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g" /etc/apt/sources.list
        # sudo apt-get update

4. Οι εικόνες Ubuntu Azure τώρα ακολουθείτε τον πυρήνα *ενεργοποίησης υλικού* (HWE). Ενημερώστε το λειτουργικό σύστημα για την πιο πρόσφατη πυρήνα, εκτελώντας τις παρακάτω εντολές:

    Ubuntu 12.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    Ubuntu 14.04:

        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot


5. Τροποποιήστε τη γραμμή εκκίνησης πυρήνα για εκσκαφή για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure. Για να το κάνετε αυτό ανοιχτό "/ κ.λπ/προεπιλεγμένη/εκσκαφή" σε ένα πρόγραμμα επεξεργασίας κειμένου, βρείτε τη μεταβλητή που ονομάζεται `GRUB_CMDLINE_LINUX_DEFAULT` (ή να το προσθέσετε εάν είναι απαραίτητο) και επεξεργασία ώστε να περιλαμβάνει τις ακόλουθες παραμέτρους:

        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Αποθηκεύστε και κλείστε αυτό το αρχείο και, στη συνέχεια, εκτελέστε "`sudo update-grub`". Αυτό θα βεβαιωθείτε ότι όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure τεχνικής υποστήριξης με τον εντοπισμό σφαλμάτων θέματα.

6.  Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

7.  Εγκαταστήστε το Azure παράγοντας Linux:

        # sudo apt-get update
        # sudo apt-get install walinuxagent

    Σημειώστε αυτήν την εγκατάσταση του `walinuxagent` πακέτο θα καταργήσει το `NetworkManager` και `NetworkManager-gnome` πακέτων, εάν έχουν εγκατασταθεί.

8.  Εκτελέστε τις ακόλουθες εντολές deprovision η εικονική μηχανή και προετοιμασία για προμήθεια στην Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Κάντε κλικ στο κουμπί **Τερματισμός λειτουργίας -> ενέργεια προς τα κάτω** στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.


## <a name="next-steps"></a>Επόμενα βήματα
Τώρα είστε έτοιμοι να χρησιμοποιήσετε Ubuntu Linux εικονικό σκληρό σας δίσκο για να δημιουργήσετε νέα εικονικές μηχανές στο Azure. Εάν αυτή είναι η πρώτη φορά που που αποστέλλετε το αρχείο .vhd να Azure, δείτε τα βήματα 2 και 3 στη [Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux](virtual-machines-linux-classic-create-upload-vhd.md).

## <a name="references"></a>Αναφορές ##

Πυρήνα ενεργοποίησης (HWE) ubuntu υλικού:

- [http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
- [http://blog.utlemming.org/2015/02/1204-Azure-cloud-images-Now-using-Hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

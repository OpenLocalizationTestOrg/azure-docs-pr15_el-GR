<properties
    pageTitle="Δημιουργία και αποστολή ενός VHD Linux SUSE στο Azure"
    description="Μάθετε πώς να δημιουργήσετε και να στείλετε ένα Azure εικονικού σκληρού δίσκου (VHD) που περιέχει ένα λειτουργικό σύστημα SUSE Linux."
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

# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Προετοιμασία μια εικονική μηχανή SLES ή openSUSE για Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία ##

Σε αυτό το άρθρο προϋποθέτει ότι έχετε εγκαταστήσει ήδη μια SUSE ή openSUSE λειτουργικό σύστημα Linux σε εικονικό σκληρό δίσκο. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε αρχεία .vhd, για παράδειγμα, μια λύση virtualization όπως το Hyper-V. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση του ρόλου Hyper-V και ρύθμιση παραμέτρων μια εικονική μηχανή](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE σημειώσεις εγκατάστασης

- Ανατρέξτε στο θέμα επίσης [Γενικές σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) για περισσότερες συμβουλές σχετικά με την προετοιμασία Linux για Azure.

- Η μορφή VHDX δεν υποστηρίζεται στο Azure, μόνο **σταθερής VHD**.  Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το cmdlet vhd μετατροπή.

- Κατά την εγκατάσταση του συστήματος Linux συνιστάται να χρησιμοποιήσετε βασικά διαμερίσματα και όχι LVM (συχνά η προεπιλογή για πολλές εγκαταστάσεις). Αυτό θα αποφύγετε LVM όνομα έρχεται σε διένεξη με κλωνοποιημένο ΣΠΣ, ιδίως εάν ένα δίσκο OS πρέπει ποτέ να επισυναφθεί σε μια άλλη Εικονική για την αντιμετώπιση προβλημάτων. [LVM](virtual-machines-linux-configure-lvm.md) ή [RAID](virtual-machines-linux-configure-raid.md) μπορεί να χρησιμοποιηθεί σε δίσκων δεδομένων εάν προτιμώμενη.

- Χωρίς ρύθμιση ένα διαμερίσματα αντιμετάθεσης στο δίσκο OS. Ο παράγοντας Linux μπορεί να ρυθμιστεί για να δημιουργήσετε ένα αρχείο αντιμετάθεσης στο δίσκο προσωρινών πόρων.  Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.

- Όλα τα VHD πρέπει να έχετε τα μεγέθη που είναι πολλαπλάσιο του 1 MB.


## <a name="use-suse-studio"></a>Χρησιμοποιήστε το SUSE Studio
[SUSE Studio](http://www.susestudio.com) εύκολα να δημιουργήσετε και να διαχειριστείτε τις εικόνες σας SLES και openSUSE για Azure και Hyper-V. Αυτή είναι η προτεινόμενη προσέγγιση για την προσαρμογή τις δικές σας εικόνες SLES και openSUSE.

Ως εναλλακτική λύση για τη δημιουργία του δικού σας VHD, SUSE δημοσιεύει εικόνες BYOS (μεταφορά η δική συνδρομής) για SLES στο [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).


## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Προετοιμασία SUSE Linux Enterprise Server 11 SP4 ##

1. Στο κεντρικό παράθυρο του Hyper-V Manager, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε το παράθυρο για την εικονική μηχανή.

3. Καταχώρηση σύστημα SUSE Linux Enterprise για να επιτρέψετε τη λήψη και εγκατάσταση πακέτων ενημερώσεων.

4. Ενημέρωση του συστήματος με τις πιο πρόσφατες ενημερωμένες εκδόσεις κώδικα:

        # sudo zypper update

5. Εγκαταστήστε τον παράγοντα Linux Azure από το χώρο αποθήκευσης SLES:

        # sudo zypper install WALinuxAgent

6. Εάν waagent έχει οριστεί σε "σε" μεταβίβαση ελέγχου chkconfig και, εάν όχι, ενεργοποίησή του για αυτόματη εκκίνηση:
               
        # sudo chkconfig waagent on

7. Ελέγξτε αν η υπηρεσία waagent εκτελείται, και εάν όχι, ξεκινήστε το: 

        # sudo service waagent start
                
8. Τροποποίηση της γραμμής εκκίνησης πυρήνα στο εκσκαφή ρύθμιση των παραμέτρων σας για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure. Για να το κάνετε αυτό, ανοίξτε "/ boot/grub/menu.lst" σε ένα πρόγραμμα επεξεργασίας κειμένου και βεβαιωθείτε ότι η προεπιλεγμένη πυρήνα περιλαμβάνει τις ακόλουθες παραμέτρους:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Αυτό θα βεβαιωθείτε ότι όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα.

9. Επιβεβαιώστε ότι /boot/grub/menu.lst και /etc/fstab και τα δύο αναφοράς στο δίσκο χρησιμοποιώντας το UUID (με uuid) αντί για το Αναγνωριστικό δίσκου (με id). 

    Λήψη δίσκου UUID
    
        # ls /dev/disk/by-uuid/

    Εάν /dev/disk/by-id / έχει χρησιμοποιηθεί, ενημέρωση τόσο /boot/grub/menu.lst και/κ.λπ/fstab με την τιμή proper από uuid

    Πριν από την αλλαγή
    
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1

    Μετά την αλλαγή
    
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

10. Τροποποίηση udev κανόνες για να αποφευχθεί η δημιουργία στατικής κανόνες για τις διασυνδέσεις Ethernet. Αυτοί οι κανόνες μπορεί να προκαλέσει προβλήματα όταν κλωνοποίηση μια εικονική μηχανή στο Microsoft Azure ή Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

11. Είναι συνιστάται να επεξεργαστείτε το αρχείο "/ κ.λπ/sysconfig/δικτύου/dhcp" και αλλάξτε το `DHCLIENT_SET_HOSTNAME` παραμέτρου με το εξής:

        DHCLIENT_SET_HOSTNAME="no"

12. Στο "/ κ.λπ/sudoers", σχολιάσετε ή καταργήστε τις ακόλουθες γραμμές, εάν υπάρχουν:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

13. Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

14. Μην δημιουργήσετε αντιμετάθεσης χώρο στο δίσκο λειτουργικό σύστημα.

    Ο παράγοντας Linux Azure αυτόματα να ρυθμίσετε τις παραμέτρους χώρος αντιμετάθεσης χρησιμοποιώντας το δίσκο τοπικός πόρος που συνδέεται με την εικονική Μηχανή μετά την προμήθεια του Azure. Σημειώστε ότι ο δίσκος τοπικός πόρος είναι ένα *προσωρινό* δίσκο και να αδειάζετε όταν η Εικονική καταργείται. Μετά την εγκατάσταση του τον παράγοντα Linux Azure (ανατρέξτε στο προηγούμενο βήμα), τροποποιήστε κατάλληλα τις ακόλουθες παραμέτρους στην /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

15. Εκτελέστε τις ακόλουθες εντολές deprovision η εικονική μηχανή και προετοιμασία για προμήθεια στην Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

16. Κάντε κλικ στο κουμπί **Τερματισμός λειτουργίας -> ενέργεια προς τα κάτω** στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.


----------

## <a name="prepare-opensuse-131"></a>Προετοιμασία openSUSE 13.1 + ##

1. Στο κεντρικό παράθυρο του Hyper-V Manager, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε το παράθυρο για την εικονική μηχανή.

3. Στην το κέλυφος, εκτελέστε την εντολή '`zypper lr`'. Εάν αυτή η εντολή επιστρέφει εξόδου παρόμοια με τα παρακάτω και, στη συνέχεια, το αποθετήρια έχουν ρυθμιστεί με τον αναμενόμενο τρόπο--απαιτούνται χωρίς προσαρμογές (Σημειώστε ότι οι αριθμοί έκδοσης μπορεί να διαφέρουν):

        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes

    Εάν η εντολή επιστρέφει "Χωρίς αποθετήρια που ορίζονται από το...", στη συνέχεια, χρησιμοποιήστε τις παρακάτω εντολές για να προσθέσετε αυτά τα repos:

        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates

    Μπορείτε, στη συνέχεια, να επαληθεύσετε το αποθετήρια έχουν προστεθεί, εκτελέστε την εντολή '`zypper lr`' ξανά. Σε περίπτωση που ένα από τα αποθετήρια σχετικές ενημέρωση δεν είναι ενεργοποιημένη, την ενεργοποιήσετε με την ακόλουθη εντολή:

        # sudo zypper mr -e [NUMBER OF REPOSITORY]


4. Ενημερώστε τον πυρήνα για την πιο πρόσφατη διαθέσιμη έκδοση:

        # sudo zypper up kernel-default

    Ή να ενημερώσετε το σύστημα με όλες τις πιο πρόσφατες ενημερωμένες εκδόσεις κώδικα:

        # sudo zypper update

5.  Εγκαταστήστε το Azure παράγοντας Linux.

        # sudo zypper install WALinuxAgent

6.  Τροποποίηση της γραμμής εκκίνησης πυρήνα στο εκσκαφή ρύθμιση των παραμέτρων σας για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure. Για να το κάνετε αυτό, ανοίξτε "/ boot/grub/menu.lst" σε ένα πρόγραμμα επεξεργασίας κειμένου και βεβαιωθείτε ότι η προεπιλεγμένη πυρήνα περιλαμβάνει τις ακόλουθες παραμέτρους:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300

    Αυτό θα βεβαιωθείτε ότι όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα. Επιπλέον, καταργήστε τις ακόλουθες παραμέτρους από τη γραμμή εκκίνησης πυρήνα, εάν υπάρχουν:

        libata.atapi_enabled=0 reserve=0x1f0,0x8

7.  Είναι συνιστάται να επεξεργαστείτε το αρχείο "/ κ.λπ/sysconfig/δικτύου/dhcp" και αλλάξτε το `DHCLIENT_SET_HOSTNAME` παραμέτρου με το εξής:

        DHCLIENT_SET_HOSTNAME="no"

8.  **Σημαντικό:** Στο "/ κ.λπ/sudoers", σχολιάσετε ή καταργήστε τις ακόλουθες γραμμές, εάν υπάρχουν:

        Defaults targetpw   # ask for the password of the target user i.e. root
        ALL    ALL=(ALL) ALL   # WARNING! Only use this together with 'Defaults targetpw'!

9.  Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

10. Μην δημιουργήσετε αντιμετάθεσης χώρο στο δίσκο λειτουργικό σύστημα.

    Ο παράγοντας Linux Azure αυτόματα να ρυθμίσετε τις παραμέτρους χώρος αντιμετάθεσης χρησιμοποιώντας το δίσκο τοπικός πόρος που συνδέεται με την εικονική Μηχανή μετά την προμήθεια του Azure. Σημειώστε ότι ο δίσκος τοπικός πόρος είναι ένα *προσωρινό* δίσκο και να αδειάζετε όταν η Εικονική καταργείται. Μετά την εγκατάσταση του τον παράγοντα Linux Azure (ανατρέξτε στο προηγούμενο βήμα), τροποποιήστε κατάλληλα τις ακόλουθες παραμέτρους στην /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

11. Εκτελέστε τις ακόλουθες εντολές deprovision η εικονική μηχανή και προετοιμασία για προμήθεια στην Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

12. Βεβαιωθείτε ότι ο παράγοντας Linux Azure εκτελείται κατά την εκκίνηση:

        # sudo systemctl enable waagent.service

13. Κάντε κλικ στο κουμπί **Τερματισμός λειτουργίας -> ενέργεια προς τα κάτω** στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα είστε έτοιμοι να χρησιμοποιήσετε SUSE Linux εικονικό σκληρό σας δίσκο για να δημιουργήσετε νέα εικονικές μηχανές στο Azure. Εάν αυτή είναι η πρώτη φορά που που αποστέλλετε το αρχείο .vhd να Azure, δείτε τα βήματα 2 και 3 στη [Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux](virtual-machines-linux-classic-create-upload-vhd.md).

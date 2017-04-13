<properties
    pageTitle="Δημιουργία και αποστολή ενός VHD Linux CentOS βασίζονται στο Azure"
    description="Μάθετε πώς να δημιουργήσετε και να στείλετε ένα Azure εικονικού σκληρού δίσκου (VHD) που περιέχει ένα λειτουργικό σύστημα που βασίζεται σε CentOS Linux."
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
    ms.date="05/09/2016"
    ms.author="szarkos"/>

# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Προετοιμασία μια εικονική μηχανή βάσει CentOS για Azure

- [Προετοιμασία μια εικονική μηχανή 6.x CentOS για Azure](#centos-6x)
- [Προετοιμασία μια εικονική μηχανή CentOS 7.0 + για Azure](#centos-70)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία ##

Σε αυτό το άρθρο προϋποθέτει ότι έχετε εγκαταστήσει ήδη ένα CentOS (ή παρόμοιο παραγώγου) το λειτουργικό σύστημα Linux σε εικονικό σκληρό δίσκο. Υπάρχουν πολλά εργαλεία για να δημιουργήσετε αρχεία .vhd, για παράδειγμα, μια λύση virtualization όπως το Hyper-V. Για οδηγίες, ανατρέξτε στο θέμα [εγκατάσταση του ρόλου Hyper-V και ρύθμιση παραμέτρων μια εικονική μηχανή](http://technet.microsoft.com/library/hh846766.aspx).


**Σημειώσεις εγκατάστασης centOS**

- Ανατρέξτε στο θέμα επίσης [Γενικές σημειώσεις εγκατάστασης Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes) για περισσότερες συμβουλές σχετικά με την προετοιμασία Linux για Azure.

- Η μορφή VHDX δεν υποστηρίζεται στο Azure, μόνο **σταθερής VHD**.  Μπορείτε να μετατρέψετε το δίσκο VHD μορφή χρησιμοποιώντας το Hyper-V Manager ή το cmdlet vhd μετατροπή.

- Κατά την εγκατάσταση του συστήματος Linux συνιστάται να χρησιμοποιήσετε βασικά διαμερίσματα και όχι LVM (συχνά η προεπιλογή για πολλές εγκαταστάσεις). Αυτό θα αποφύγετε LVM όνομα έρχεται σε διένεξη με κλωνοποιημένο ΣΠΣ, ιδίως εάν ένα δίσκο OS πρέπει ποτέ να επισυναφθεί σε μια άλλη Εικονική για την αντιμετώπιση προβλημάτων.  [LVM](virtual-machines-linux-configure-lvm.md) ή [RAID](virtual-machines-linux-configure-raid.md) μπορεί να χρησιμοποιηθεί σε δίσκων δεδομένων εάν προτιμώμενη.

- NUMA δεν υποστηρίζεται για μεγαλύτερα μεγέθη Εικονική λόγω σφάλματος σε εκδόσεις πυρήνα Linux κάτω από το 2.6.37. Αυτό το ζήτημα επηρεάζει κυρίως κατανομές χρησιμοποιώντας τη νέα κόκκινο καπέλο 2.6.32 πυρήνα. Μη αυτόματη εγκατάσταση των τον παράγοντα Azure Linux (waagent) θα απενεργοποιήσετε αυτόματα NUMA στη ρύθμιση παραμέτρων ΕΚΣΚΑΦΉ για τον πυρήνα Linux. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.

- Χωρίς ρύθμιση ένα διαμερίσματα αντιμετάθεσης στο δίσκο OS. Ο παράγοντας Linux μπορεί να ρυθμιστεί για να δημιουργήσετε ένα αρχείο αντιμετάθεσης στο δίσκο προσωρινών πόρων.  Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με αυτό στα παρακάτω βήματα.

- Όλα τα VHD πρέπει να έχετε τα μεγέθη που είναι πολλαπλάσιο του 1 MB.


## <a name="centos-6x"></a>CentOS 6.x ##

1. Στη Διαχείριση Hyper-V, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε ένα παράθυρο κονσόλας για την εικονική μηχανή.

3. Κατάργηση εγκατάστασης NetworkManager, εκτελώντας την ακόλουθη εντολή:

        # sudo rpm -e --nodeps NetworkManager

    **Σημείωση:** Εάν το πακέτο δεν είναι ήδη εγκατεστημένο, αυτή η εντολή θα αποτύχει με ένα μήνυμα σφάλματος. Αυτό είναι αναμενόμενο.

4.  Δημιουργήστε ένα αρχείο με το όνομα **δικτύου** σε το `/etc/sysconfig/` κατάλογο που περιέχει το ακόλουθο κείμενο:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5.  Δημιουργήστε ένα αρχείο με το όνομα **ifcfg eth0** στο το `/etc/sysconfig/network-scripts/` κατάλογο που περιέχει το ακόλουθο κείμενο:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Τροποποίηση udev κανόνες για να αποφευχθεί η δημιουργία στατικής κανόνες για τις διασυνδέσεις Ethernet. Αυτοί οι κανόνες μπορεί να προκαλέσει προβλήματα όταν κλωνοποίηση μια εικονική μηχανή στο Microsoft Azure ή Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules


7. Βεβαιωθείτε ότι η υπηρεσία δικτύου θα ξεκινήσει κατά την εκκίνηση, εκτελώντας την ακόλουθη εντολή:

        # sudo chkconfig network on


8. **Μόνο centOS 6.3**: εγκαταστήσετε τα προγράμματα οδήγησης για τις υπηρεσίες ενοποίησης Linux (LIS).

    **Σημαντικό: Το βήμα είναι μόνο έγκυρη για CentOS 6.3 και παλαιότερες εκδόσεις.**  Στο CentOS 6.4 + τις υπηρεσίες ενοποίησης Linux είναι *ήδη διαθέσιμα στον τυπικό πυρήνα*.

    - Ακολουθήστε τις οδηγίες εγκατάστασης στην τη [σελίδα λήψης LIS](https://www.microsoft.com/en-us/download/details.aspx?id=46842) και εγκαταστήστε το RPM στην εικόνα σας.  


9. Εγκατάσταση του πακέτου python pyasn1, εκτελώντας την ακόλουθη εντολή:

        # sudo yum install python-pyasn1

10. Εάν θέλετε να χρησιμοποιήσετε το αντικατοπτρίζει OpenLogic που φιλοξενούνται εντός του Azure κέντρα δεδομένων, στη συνέχεια, αντικαταστήστε το αρχείο /etc/yum.repos.d/CentOS-Base.repo με των εξής αποθετηρίων.  Αυτό θα προσθέσετε επίσης το αποθετήριο **[openlogic]** που περιλαμβάνει πακέτων για τον παράγοντα Azure Linux:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    **Σημείωση:** Τα υπόλοιπα αυτός ο οδηγός θα θεωρείται ότι χρησιμοποιείτε τουλάχιστον την repo [openlogic], που θα χρησιμοποιηθεί για να εγκαταστήσετε τον παράγοντα Azure Linux παρακάτω.


11. Προσθέστε την ακόλουθη γραμμή /etc/yum.conf:

        http_caching=packages

    Και **στην CentOS 6.3 μόνο** , προσθέστε την ακόλουθη γραμμή:

        exclude=kernel*

12. Απενεργοποίηση της λειτουργικής μονάδας yum "fastestmirror" με την επεξεργασία του αρχείου "/ etc/yum/pluginconf.d/fastestmirror.conf", και, στην περιοχή `[main]`, πληκτρολογήστε τα εξής:

        set enabled=0

13. Εκτελέστε την παρακάτω εντολή για να καταργήσετε την τρέχουσα μετα-δεδομένων yum:

        # yum clean all

14. **Στην CentOS 6.3 μόνο**, ενημερώστε τον πυρήνα χρησιμοποιώντας την ακόλουθη εντολή:

        # sudo yum --disableexcludes=all install kernel

15. Τροποποίηση της γραμμής εκκίνησης πυρήνα στο εκσκαφή ρύθμιση των παραμέτρων σας για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure. Για να το κάνετε αυτό, ανοίξτε "/ boot/grub/menu.lst" σε ένα πρόγραμμα επεξεργασίας κειμένου και βεβαιωθείτε ότι η προεπιλεγμένη πυρήνα περιλαμβάνει τις ακόλουθες παραμέτρους:

        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off

    Αυτό θα εξασφαλίσει επίσης όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα. Αυτό θα απενεργοποιήσει NUMA λόγω σφάλματος στην έκδοση πυρήνα χρησιμοποιούνται από CentOS 6.

    Εκτός από τα παραπάνω, συνιστάται να *καταργήσετε* τις ακόλουθες παραμέτρους:

        rhgb quiet crashkernel=auto

    Δεν είναι χρήσιμη σε ένα περιβάλλον cloud όπου θέλουμε όλα τα αρχεία καταγραφής θα αποσταλούν στη σειριακή θύρα γραφικών και ήσυχο εκκίνησης.

    Το `crashkernel` επιλογή ενδέχεται να έχει ρυθμιστεί εάν θέλετε τα αριστερά, αλλά Σημειώστε ότι αυτή η παράμετρος θα μείωση της ποσότητας διαθέσιμη μνήμη σε η Εικονική από 128MB ή μεγαλύτερη, που μπορεί να είναι προβληματικό τα μικρότερα μεγέθη Εικονική.


16. Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

17. Εγκαταστήστε τον παράγοντα Linux Azure, εκτελώντας την ακόλουθη εντολή:

        # sudo yum install WALinuxAgent

    Σημειώστε ότι κατά την εγκατάσταση του πακέτου WALinuxAgent θα καταργήσει τα NetworkManager και NetworkManager gnome πακέτων εάν αυτά δεν ήδη καταργήθηκαν όπως περιγράφεται στο βήμα 2.

18. Μην δημιουργήσετε αντιμετάθεσης χώρο στο δίσκο λειτουργικό σύστημα.

    Ο παράγοντας Linux Azure αυτόματα να ρυθμίσετε τις παραμέτρους χώρος αντιμετάθεσης χρησιμοποιώντας το δίσκο τοπικός πόρος που συνδέεται με την εικονική Μηχανή μετά την προμήθεια του Azure. Σημειώστε ότι ο δίσκος τοπικός πόρος είναι ένα *προσωρινό* δίσκο και να αδειάζετε όταν η Εικονική καταργείται. Μετά την εγκατάσταση του τον παράγοντα Linux Azure (ανατρέξτε στο προηγούμενο βήμα), τροποποιήστε κατάλληλα τις ακόλουθες παραμέτρους στην /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

19. Εκτελέστε τις ακόλουθες εντολές deprovision η εικονική μηχανή και προετοιμασία για προμήθεια στην Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

20. Κάντε κλικ στο κουμπί **Τερματισμός λειτουργίας -> ενέργεια προς τα κάτω** στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.


----------


## <a name="centos-70"></a>CentOS 7.0 + ##

**Αλλαγές σε CentOS 7 (και παρόμοια προϊόντα)**

Προετοιμασία μια εικονική μηχανή CentOS 7 για Azure είναι παρόμοια με 6 CentOS, ωστόσο, υπάρχουν πολλές σημαντικές διαφορές αξίζει να αναφερθούν:

 - Το πακέτο NetworkManager δεν είναι πλέον διένεξη με τον παράγοντα Azure Linux. Αυτό το πακέτο είναι εγκατεστημένο από προεπιλογή και συνιστάται να ότι δεν καταργείται.
 - GRUB2 τώρα χρησιμοποιείται ως το προεπιλεγμένο bootloader, ώστε η διαδικασία για την επεξεργασία παράμετροι πυρήνα έχει αλλάξει (δείτε παρακάτω).
 - XFS είναι τώρα το προεπιλεγμένο σύστημα αρχείων. Το σύστημα αρχείων ext4 εξακολουθεί να μπορεί να χρησιμοποιηθεί, εάν θέλετε.


**Βήματα ρύθμισης παραμέτρων**

1. Στη Διαχείριση Hyper-V, επιλέξτε την εικονική μηχανή.

2. Κάντε κλικ στην επιλογή **σύνδεση** για να ανοίξετε ένα παράθυρο κονσόλας για την εικονική μηχανή.

3.  Δημιουργήστε ένα αρχείο με το όνομα **δικτύου** σε το `/etc/sysconfig/` κατάλογο που περιέχει το ακόλουθο κείμενο:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4.  Δημιουργήστε ένα αρχείο με το όνομα **ifcfg eth0** στο το `/etc/sysconfig/network-scripts/` κατάλογο που περιέχει το ακόλουθο κείμενο:

        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6.  Τροποποίηση udev κανόνες για να αποφευχθεί η δημιουργία στατικής κανόνες για τις διασυνδέσεις Ethernet. Αυτοί οι κανόνες μπορεί να προκαλέσει προβλήματα όταν κλωνοποίηση μια εικονική μηχανή στο Microsoft Azure ή Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Βεβαιωθείτε ότι η υπηρεσία δικτύου θα ξεκινήσει κατά την εκκίνηση, εκτελώντας την ακόλουθη εντολή:

        # sudo chkconfig network on

7. Εγκατάσταση του πακέτου python pyasn1, εκτελώντας την ακόλουθη εντολή:

        # sudo yum install python-pyasn1

8. Εάν θέλετε να χρησιμοποιήσετε το αντικατοπτρίζει OpenLogic που φιλοξενούνται εντός του Azure κέντρα δεδομένων, στη συνέχεια, αντικαταστήστε το αρχείο /etc/yum.repos.d/CentOS-Base.repo με των εξής αποθετηρίων.  Αυτό θα προσθέσετε επίσης το αποθετήριο **[openlogic]** που περιλαμβάνει πακέτων για τον παράγοντα Azure Linux:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0

        [base]
        name=CentOS-$releasever - Base
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7



    **Σημείωση:** Τα υπόλοιπα αυτός ο οδηγός θα θεωρείται ότι χρησιμοποιείτε τουλάχιστον την repo [openlogic], που θα χρησιμοποιηθεί για να εγκαταστήσετε τον παράγοντα Azure Linux παρακάτω.

9.  Εκτελέστε την παρακάτω εντολή για να καταργήσετε την τρέχουσα μετα-δεδομένων yum και εγκαταστήστε τις ενημερώσεις:

        # sudo yum clean all
        # sudo yum -y update

10. Τροποποίηση της γραμμής εκκίνησης πυρήνα στο εκσκαφή ρύθμιση των παραμέτρων σας για να συμπεριλάβετε τις παραμέτρους επιπλέον πυρήνα για Azure. Για να το κάνετε αυτό, Άνοιγμα "/ κ.λπ/προεπιλεγμένη/εκσκαφή" σε ένα πρόγραμμα επεξεργασίας κειμένου και επεξεργαστείτε το `GRUB_CMDLINE_LINUX` παράμετρο, για παράδειγμα:

        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"

    Αυτό θα εξασφαλίσει επίσης όλα τα μηνύματα κονσόλας στην οποία αποστέλλονται την πρώτη σειρά θύρα, την οποία μπορούν να σας βοηθήσουν Azure υποστήριξης με τον εντοπισμό σφαλμάτων θέματα. Επίσης απενεργοποιεί τις νέες συμβάσεις ονομασίας CentOS 7 για NIC. Εκτός από τα παραπάνω, συνιστάται να *καταργήσετε* τις ακόλουθες παραμέτρους:

        rhgb quiet crashkernel=auto

    Δεν είναι χρήσιμη σε ένα περιβάλλον cloud όπου θέλουμε όλα τα αρχεία καταγραφής θα αποσταλούν στη σειριακή θύρα γραφικών και ήσυχο εκκίνησης.

    Το `crashkernel` επιλογή ενδέχεται να έχει ρυθμιστεί εάν θέλετε τα αριστερά, αλλά Σημειώστε ότι αυτή η παράμετρος θα μείωση της ποσότητας διαθέσιμη μνήμη σε η Εικονική από 128MB ή μεγαλύτερη, που μπορεί να είναι προβληματικό τα μικρότερα μεγέθη Εικονική.

11. Όταν ολοκληρώσετε την επεξεργασία "/ κ.λπ/προεπιλεγμένη/εκσκαφή" ανά παραπάνω, εκτελέστε την ακόλουθη εντολή για να δημιουργήσετε ξανά τις παραμέτρους εκσκαφή:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

12. Βεβαιωθείτε ότι ο διακομιστής SSH έχει εγκατασταθεί και ρυθμιστεί να ξεκινά κατά την εκκίνηση.  Συνήθως, αυτή είναι η προεπιλεγμένη.

13. **Μόνο εάν δημιουργείτε την εικόνα από VMWare, VirtualBox ή KVM:** Προσθέστε το Hyper-V λειτουργικές μονάδες σε initramfs:

    Επεξεργασία `/etc/dracut.conf`, προσθήκη περιεχομένου:

        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”

    Εκ νέου δημιουργία του initramfs:

        # dracut –f -v

14. Εγκαταστήστε τον παράγοντα Linux Azure, εκτελώντας την ακόλουθη εντολή:

        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent

15. Μην δημιουργήσετε αντιμετάθεσης χώρο στο δίσκο λειτουργικό σύστημα.

    Ο παράγοντας Linux Azure αυτόματα να ρυθμίσετε τις παραμέτρους χώρος αντιμετάθεσης χρησιμοποιώντας το δίσκο τοπικός πόρος που συνδέεται με την εικονική Μηχανή μετά την προμήθεια του Azure. Σημειώστε ότι ο δίσκος τοπικός πόρος είναι ένα *προσωρινό* δίσκο και να αδειάζετε όταν η Εικονική καταργείται. Μετά την εγκατάσταση του τον παράγοντα Linux Azure (ανατρέξτε στο προηγούμενο βήμα), τροποποιήστε κατάλληλα τις ακόλουθες παραμέτρους στην /etc/waagent.conf:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. Εκτελέστε τις ακόλουθες εντολές deprovision η εικονική μηχανή και προετοιμασία για προμήθεια στην Azure:

        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Κάντε κλικ στο κουμπί **Τερματισμός λειτουργίας -> ενέργεια προς τα κάτω** στον Hyper-V Manager. Σας VHD Linux είναι τώρα έτοιμο να αποσταλεί Azure.

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα είστε έτοιμοι να χρησιμοποιήσετε CentOS Linux εικονικό σκληρό σας δίσκο για να δημιουργήσετε νέα εικονικές μηχανές στο Azure. Εάν αυτή είναι η πρώτη φορά που που αποστέλλετε το αρχείο .vhd να Azure, δείτε τα βήματα 2 και 3 στη [Δημιουργία και αποστολή ενός εικονικού σκληρού δίσκου που περιέχει το λειτουργικό σύστημα Linux](virtual-machines-linux-classic-create-upload-vhd.md).

<properties
    pageTitle="Προσθέσετε ένα δίσκο στις Εικονική Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να προσθέσετε ένα δίσκο μόνιμη σας Εικονική Linux"
    keywords="Linux εικονικό υπολογιστή, προσθέστε δίσκο πόρων"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Προσθέσετε ένα δίσκο σε μια Εικονική Linux

Σε αυτό το άρθρο παρουσιάζει τον τρόπο για να επισυνάψετε ένα δίσκο μόνιμη Εικονική σας ώστε να μπορείτε να διατηρήσετε τα δεδομένα σας - ακόμα και αν είναι reprovisioned σας Εικονική λόγω συντήρησης ή την αλλαγή μεγέθους. Για να προσθέσετε ένα δίσκο, χρειάζεστε [Το Azure CLI](../xplat-cli-install.md) έχουν ρυθμιστεί σε λειτουργία διαχείρισης πόρων (`azure config mode arm`).  

## <a name="quick-commands"></a>Γρήγορες εντολές

Στα παρακάτω παραδείγματα εντολή, αντικαταστήστε τις τιμές μεταξύ &lt; και &gt; με τις τιμές από τη δική σας περιβάλλον.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Επισυνάψτε ένα δίσκο

Επισύναψη ενός νέου δίσκου είναι γρήγορη. Τύπος `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` για να δημιουργήσετε και να επισυνάψετε ένα νέο δίσκο GB για Εικονική σας. Εάν δεν προσδιορίσετε ρητά ένα λογαριασμό του χώρου αποθήκευσης, οποιοδήποτε δίσκο, μπορείτε να δημιουργήσετε τοποθετείται στον ίδιο λογαριασμό χώρου αποθήκευσης όπου βρίσκεται το λειτουργικό σύστημα δίσκου.  Αυτό θα μπορούσε να έχει ως εξής:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Εξόδου

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Σύνδεση με την Εικονική Linux για το νέο δίσκο ενεργοποίησης

> [AZURE.NOTE] Αυτό το θέμα συνδέεται με μια Εικονική χρησιμοποιώντας τα ονόματα των χρηστών και κωδικούς πρόσβασης. Για να χρησιμοποιήσετε δημόσια και ιδιωτικά ζεύγη κλειδιών για να επικοινωνήσετε με την Εικονική, δείτε [πώς μπορείτε να χρησιμοποιήσετε SSH με Linux στην Azure](virtual-machines-linux-mac-create-ssh-keys.md). Μπορείτε να τροποποιήσετε τη συνδεσιμότητα **SSH** του ΣΠΣ που δημιουργήθηκε με το `azure vm quick-create` εντολής χρησιμοποιώντας το `azure vm reset-access` εντολή για την επαναφορά πρόσβασης **SSH** εντελώς, προσθήκη ή κατάργηση χρηστών ή προσθήκη δημόσια βασικών αρχείων για την ασφάλιση της access.

Που χρειάζεστε για SSH σε σας Εικονική Azure για τη δημιουργία διαμερισμάτων, μορφοποίηση και ενεργοποίησης του νέου δίσκου, για να χρησιμοποιήσετε το Εικονική Linux. Εάν δεν είστε εξοικειωμένοι με τη σύνδεση με **ssh**, η εντολή σας μεταφέρει στη φόρμα `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`, και μοιάζει με τα εξής:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Εξόδου

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

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

ops@myuniquevmname:~$
```

Τώρα που είστε συνδεδεμένοι σε Εικονική σας, είστε έτοιμοι να επισυνάψετε ένα δίσκο.  Πρώτα, βρείτε το δίσκο, χρησιμοποιώντας `dmesg | grep SCSI` (τη μέθοδο που χρησιμοποιείτε για να εντοπίσετε το νέο δίσκο μπορεί να διαφέρουν). Σε αυτήν την περίπτωση, μοιάζει κάπως:

```bash
dmesg | grep SCSI
```

Εξόδου

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

και στην περίπτωση αυτό το θέμα, την `sdc` δίσκος είναι αυτό που θέλετε. Τώρα δημιουργήσετε διαμερίσματα στο δίσκο με `sudo fdisk /dev/sdc` --με την προϋπόθεση ότι στην περίπτωσή σας ο δίσκος ήταν `sdc`, και να είναι ένα πρωτεύον δίσκο σε διαμερίσματα 1 και αποδεχτείτε τις προεπιλογές άλλων.

```bash
sudo fdisk /dev/sdc
```

Εξόδου

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Δημιουργήστε τα διαμερίσματα, πληκτρολογώντας `p` στη γραμμή εντολών:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Και γράψτε ένα σύστημα αρχείων για τα διαμερίσματα με τη χρήση της εντολής **mkfs** , που καθορίζει τον τύπο του συστήματος αρχείων σας και το όνομα της συσκευής. Σε αυτό το θέμα, χρησιμοποιούμε τη `ext4` και `/dev/sdc1` από επάνω:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Εξόδου

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Τώρα μπορούμε να δημιουργήσουμε έναν κατάλογο ώστε να ενεργοποίησης του συστήματος αρχείων με χρήση `mkdir`:

```bash
sudo mkdir /datadrive
```

Και να ενεργοποιήσετε τη χρήση του καταλόγου `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Ο δίσκος δεδομένων είναι τώρα έτοιμο για χρήση ως `/datadrive`.

```bash
ls
```

Εξόδου

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Για να βεβαιωθείτε ότι έχει ενεργοποιηθεί αυτόματα στη μονάδα δίσκου ξανά μετά την επανεκκίνηση πρέπει να προστεθεί στο αρχείο fstab/κ.λπ /. Επιπλέον, συνιστάται ιδιαίτερα ότι το UUID (παγκοσμίως μοναδικό αναγνωριστικό) χρησιμοποιείται σε/κ.λπ/fstab παραπέμπει στη μονάδα δίσκου και όχι απλώς το όνομα της συσκευής (όπως, `/dev/sdc1`). Εάν το λειτουργικό σύστημα εντοπίσει σφάλμα στο δίσκο κατά την εκκίνηση, χρησιμοποιώντας το UUID αποτρέπει την εσφαλμένη δίσκου να τοποθετηθεί σε μια συγκεκριμένη θέση. Υπόλοιπη δίσκων δεδομένων θα, στη συνέχεια, να αντιστοιχιστούν αυτά τα ίδια αναγνωριστικά συσκευής. Για να βρείτε το UUID με τη νέα μονάδα δίσκου, χρησιμοποιήστε το βοηθητικό πρόγραμμα **blkid** :

```bash
sudo -i blkid
```

Το αποτέλεσμα μοιάζει με το εξής:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Εσφαλμένη επεξεργάζονται το αρχείο **/etc/fstab** θα μπορούσε να έχει ως αποτέλεσμα την αδυναμία εκκίνησης του συστήματος. Εάν βέβαιοι, ανατρέξτε στην τεκμηρίωση της κατανομής για πληροφορίες σχετικά με το πώς μπορείτε να επεξεργαστείτε σωστά αυτό το αρχείο. Συνιστάται επίσης ότι έχει δημιουργήσει ένα αντίγραφο ασφαλείας του αρχείου /etc/fstab πριν από την επεξεργασία.

Στη συνέχεια, ανοίξτε το αρχείο **/etc/fstab** σε έναν επεξεργαστή κειμένου:

```bash
sudo vi /etc/fstab
```

Σε αυτό το παράδειγμα, μπορούμε να χρησιμοποιήσουμε την τιμή UUID για τη νέα συσκευή **/dev/sdc1** που δημιουργήθηκε με τα προηγούμενα βήματα και το Σημείο_μονταρίσματος **/datadrive**. Προσθέστε την ακόλουθη γραμμή στο τέλος του αρχείου **/etc/fstab** :

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Κατάργηση αργότερα ένα δίσκο δεδομένων χωρίς επεξεργασία fstab μπορεί να προκαλέσει την εικονική Μηχανή αποτυχία εκκίνησης. Οι περισσότερες κατανομές παρέχουν είτε το `nofail` ή/και `nobootwait` fstab επιλογές. Αυτές οι επιλογές επιτρέπουν ενός συστήματος για εκκίνηση ακόμα και εάν αποτύχει η ενεργοποίησης κατά την εκκίνηση του δίσκου. Συμβουλευτείτε την τεκμηρίωση του διανομής σας για περισσότερες πληροφορίες σχετικά με αυτές τις παραμέτρους.

>[AZURE.NOTE] Η επιλογή **nofail** εξασφαλίζει ότι η Εικονική ξεκινά ακόμα και αν το σύστημα αρχείων έχει καταστραφεί ή ο δίσκος δεν υπάρχει κατά την εκκίνηση. Χωρίς αυτήν την επιλογή, που μπορεί να αντιμετωπίσετε συμπεριφορά, όπως περιγράφεται στο [Να SSH για Εικονική Linux λόγω FSTAB σφαλμάτων](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>ΑΠΟΚΟΠΉ/UNMAP υποστήριξης για Linux στο Azure
Ορισμένες πυρήνων Linux υποστηρίζει ΑΠΟΚΟΠΉ/UNMAP λειτουργίες για να απορρίψετε που δεν χρησιμοποιούνται μπλοκ στο δίσκο. Αυτό είναι κυρίως χρήσιμη σε τυπική χώρου αποθήκευσης για να ενημερώσετε Azure που διαγραφεί σελίδες δεν είναι πλέον έγκυρες και μπορεί να απορριφθεί. Αυτό μπορεί να εξοικονομήσει κόστος αν δημιουργήσετε μεγάλων αρχείων και, στη συνέχεια, διαγράψτε τα.

Υπάρχουν δύο τρόποι για να ενεργοποιήσετε την ΑΠΟΚΟΠΉ υποστήριξης σε σας Εικονική Linux. Όπως συνήθως, συμβουλευτείτε την κατανομή για την προτεινόμενη προσέγγιση:

- Χρησιμοποιήστε το `discard` ενεργοποίησης επιλογή `/etc/fstab`, για παράδειγμα:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Εναλλακτικά, μπορείτε να εκτελέσετε το `fstrim` εντολή με μη αυτόματο τρόπο από τη γραμμή εντολών ή να προσθέσετε αυτό το crontab για να εκτελέσετε τακτικά:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL/CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Επόμενα βήματα

- Να θυμάστε, ότι το νέο δίσκο δεν είναι διαθέσιμη για την εικονική Μηχανή όταν γίνεται επανεκκίνηση, εκτός εάν γράφετε αυτές τις πληροφορίες στο αρχείο σας [fstab](http://en.wikipedia.org/wiki/Fstab) .
- Για να εξασφαλίσετε την Εικονική Linux έχει ρυθμιστεί σωστά, ελέγξτε τις συστάσεις [Βελτιστοποίηση των επιδόσεων του υπολογιστή Linux](virtual-machines-linux-optimization.md) .
- Αναπτύξτε τη χωρητικότητα αποθήκευσης με την προσθήκη επιπλέον δίσκων και [Ρύθμιση παραμέτρων RAID](virtual-machines-linux-configure-raid.md) για επιπλέον επιδόσεων.

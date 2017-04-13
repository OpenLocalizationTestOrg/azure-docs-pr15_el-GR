<properties
    pageTitle="Βελτιστοποίηση των επιδόσεων MySQL σε VM Linux | Microsoft Azure"
    description="Μάθετε πώς να βελτιστοποιήσετε την MySQL εκτελείται σε ένα Azure εικονική μηχανή (Εικονική) εκτελείται Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="optimizing-mysql-performance-on-azure-linux-vms"></a>Βελτιστοποίηση των επιδόσεων MySQL σε VM Azure Linux

Υπάρχουν πολλοί παράγοντες που επηρεάζουν τις επιδόσεις MySQL σε Azure, και τα δύο στις ρυθμίσεις παραμέτρων επιλογής και λογισμικό εικονικού υλικού. Σε αυτό το άρθρο εστιάζει στην τη βελτιστοποίηση των επιδόσεων μέσω του χώρου αποθήκευσης, το σύστημα και ρυθμίσεις παραμέτρων βάσης δεδομένων.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="utilizing-raid-on-an-azure-virtual-machine"></a>Με χρήση RAID σε μια εικονική μηχανή Azure
Χώρου αποθήκευσης είναι ο παράγοντας κλειδιού που επηρεάζει την απόδοση της βάσης δεδομένων στο cloud περιβάλλοντα.  Σε σύγκριση με έναν μόνο δίσκο, RAID μπορούν να παρέχουν ταχύτερη πρόσβαση μέσω του συγχρονισμού.  Ανατρέξτε στην [Τυπική επίπεδα RAID](http://en.wikipedia.org/wiki/Standard_RAID_levels) για περισσότερες λεπτομέρειες.   

Είσοδος/έξοδος δίσκου μετάδοσης και το χρόνο απόκρισης εισόδου/εξόδου στο Azure μπορούν να σημαντικά βελτιωθούν με RAID. Μας δοκιμές εργαστήριο εμφάνιση είσοδο/έξοδο δίσκου μπορεί να είναι διπλό μετάδοσης και χρόνο απόκρισης εισόδου/εξόδου μπορεί να μειωθεί κατά το ήμισυ κατά μέσο όρο όταν διπλό τον αριθμό των δίσκων RAID (από 2 έως 4, 4 έως 8, κ.λπ.). Για λεπτομέρειες, ανατρέξτε στο θέμα [προσάρτημα Α](#AppendixA) .  

Εκτός από δίσκο εισόδου/εξόδου, επιδόσεων MySQL βελτιώνει όταν μπορείτε να αυξήσετε το επίπεδο RAID.  Για λεπτομέρειες, ανατρέξτε στο θέμα [Προσάρτημα Β](#AppendixB) .  

Μπορεί επίσης να θέλετε να λάβετε υπόψη το μέγεθος του τμήματος. Σε γενικές γραμμές όταν έχετε ένα μεγαλύτερο μέγεθος μπλοκ, θα εμφανιστεί κάτω επιβάρυνσης, ειδικά για μεγάλη εγγραφές. Ωστόσο, όταν το μέγεθος του μπλοκ είναι πολύ μεγάλο, αυτό μπορεί να προσθέσετε επιπλέον επιβάρυνσης και που δεν είναι δυνατό να εκμεταλλευτείτε το RAID. Το τρέχον προεπιλεγμένο μέγεθος είναι 512KB, η οποία είναι αποδείξει ότι είναι βέλτιστη για πιο γενικές περιβάλλοντα παραγωγής. Για λεπτομέρειες, ανατρέξτε στο θέμα [Προσάρτημα C](#AppendixC) .   

Σημειώστε ότι δεν υπάρχουν περιορισμοί πόσες δίσκων που μπορείτε να προσθέσετε για διαφορετικές εικονική μηχανή τύπους. Αυτά τα όρια ορίζονται λεπτομερώς στην [εικονική μηχανή και τα μεγέθη υπηρεσία Cloud για Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Θα χρειαστείτε 4 δίσκων συνημμένα δεδομένα για να παρακολουθήσετε το παράδειγμα RAID σε αυτό το άρθρο, παρόλο που μπορείτε να επιλέξετε να ορίσετε RAID με λιγότερες δίσκων.  

Σε αυτό το άρθρο προϋποθέτει ότι έχετε ήδη δημιουργήσει μια εικονική μηχανή Linux και MYSQL εγκατεστημένη και ρυθμισμένη. Για περισσότερες πληροφορίες σχετικά με γρήγορα αποτελέσματα, ανατρέξτε στο θέμα Πώς να εγκαταστήσετε το MySQL σε Azure.  

###<a name="setting-up-raid-on-azure"></a>Ρύθμιση RAID στο Azure
Τα ακόλουθα βήματα δείχνουν πώς μπορείτε να δημιουργήσετε RAID σε Azure με την πύλη Azure κλασική. Μπορείτε επίσης να ρυθμίσετε RAID δέσμες ενεργειών του Windows PowerShell.
Σε αυτό το παράδειγμα θα σας θα ρυθμίσετε τις παραμέτρους RAID 0 με 4 δίσκων.  

####<a name="step-1-add-a-data-disk-to-your-virtual-machine"></a>Βήμα 1: Προσθήκη ένα δίσκο δεδομένων για να την εικονική μηχανή  

Στη σελίδα εικονικές μηχανές του Azure κλασική πύλη, κάντε κλικ στην επιλογή η εικονική μηχανή στην οποία θέλετε να προσθέσετε ένα δίσκο δεδομένων. Σε αυτό το παράδειγμα, η εικονική μηχανή είναι mysqlnode1.  

![][1]

Στη σελίδα για την εικονική μηχανή, κάντε κλικ στην επιλογή **πίνακα εργαλείων**.  

![][2]


Στη γραμμή εργασιών, κάντε κλικ στην επιλογή **Επισύναψη**.

![][3]

Και, στη συνέχεια, κάντε κλικ στην επιλογή **Επισύναψη κενό δίσκο**.  

![][4]

Για δίσκων δεδομένων, την **Προτίμηση Cache κεντρικού υπολογιστή** πρέπει να οριστεί σε **κανένα**.  

Αυτό θα προσθέσει ένα κενό δίσκο σε την εικονική μηχανή σας. Επαναλάβετε αυτό το βήμα τρεις φορές ακόμη ώστε να έχετε 4 δίσκων δεδομένων για RAID.  

Μπορείτε να δείτε τις πρόσθετες μονάδες η εικονική μηχανή, εξετάζοντας το αρχείο καταγραφής μηνυμάτων πυρήνα. Για παράδειγμα, για να δείτε αυτό Ubuntu, χρησιμοποιήστε την ακόλουθη εντολή:  

    sudo grep SCSI /var/log/dmesg

####<a name="step-2-create-raid-with-the-additional-disks"></a>Βήμα 2: Δημιουργία RAID με πρόσθετη δισκέτα
Ακολουθήστε αυτό το άρθρο για λεπτομερή βήματα για τη ρύθμιση RAID:  

[Ρύθμιση παραμέτρων του λογισμικού RAID σε Linux](virtual-machines-linux-configure-raid.md)

>[AZURE.NOTE] Εάν χρησιμοποιείτε το σύστημα αρχείων XFS, ακολουθήστε τα παρακάτω βήματα, αφού δημιουργήσετε RAID.

Για να εγκαταστήσετε το XFS σε Debian, Ubuntu ή Linux μέντα, χρησιμοποιήστε την ακόλουθη εντολή:  

    apt-get -y install xfsprogs  

Για να εγκαταστήσετε XFS σε Fedora, CentOS ή RHEL, χρησιμοποιήστε την ακόλουθη εντολή:  

    yum -y install xfsprogs  xfsdump


####<a name="step-3-set-up-a-new-storage-path"></a>Βήμα 3: Ρύθμιση του μια νέα διαδρομή χώρου αποθήκευσης
Χρησιμοποιήστε την ακόλουθη εντολή:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

####<a name="step-4-copy-the-original-data-to-the-new-storage-path"></a>Βήμα 4: Αντιγραφή τα αρχικά δεδομένα για τη νέα διαδρομή χώρου αποθήκευσης
Χρησιμοποιήστε την ακόλουθη εντολή:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

####<a name="step-5-modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a>Βήμα 5: Τροποποίηση δικαιωμάτων, ώστε να μπορούν να έχουν πρόσβαση MySQL (ανάγνωση και εγγραφή) στο δίσκο δεδομένων
Χρησιμοποιήστε την ακόλουθη εντολή:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


##<a name="adjust-the-disk-io-scheduling-algorithm"></a>Προσαρμογή αλγόριθμο προγραμματισμού δίσκου εισόδου/εξόδου
Linux υλοποιεί τέσσερις τύπους εισόδου/εξόδου προγραμματισμού αλγόριθμους:  

-   Ο αλγόριθμος NOOP (όχι σε λειτουργία)
-   Προθεσμία αλγόριθμο (προθεσμίας)
-   Πλήρως έκθεσης ουράς αλγόριθμο (CFQ)
-   Περίοδος αλγόριθμο προϋπολογισμού (Anticipatory)  

Μπορείτε να επιλέξετε διαφορετικό εισόδου/εξόδου τους προγραμματιστές στην περιοχή διαφορετικά σενάρια για βελτιστοποίηση των επιδόσεων. Σε ένα περιβάλλον εντελώς τυχαίας πρόσβασης, δεν υπάρχει μεγάλη διαφορά μεταξύ τους αλγορίθμους CFQ και προθεσμία για τις επιδόσεις. Γενικά, συνιστάται να ρυθμίσετε το περιβάλλον βάση δεδομένων MySQL προθεσμίας για σταθερότητα. Εάν υπάρχει πολλές διαδοχικές εισόδου/εξόδου, CFQ ενδέχεται να μειώσετε τις επιδόσεις εισόδου/εξόδου δίσκου.   

Για SSD και άλλου εξοπλισμού, με τη χρήση NOOP ή προθεσμίας να επιτύχετε καλύτερες επιδόσεις από το προεπιλεγμένο χρονοδιάγραμμα.   

Από τον πυρήνα 2,5, ο προεπιλεγμένος αλγόριθμος προγραμματισμού εισόδου/εξόδου είναι προθεσμίας. Ξεκινώντας από τον πυρήνα 2.6.18, CFQ γίνεται ο προεπιλεγμένος αλγόριθμος προγραμματισμού εισόδου/εξόδου.  Μπορείτε να καθορίσετε αυτήν τη ρύθμιση κατά την εκκίνηση πυρήνα ή να τροποποιήσετε δυναμικά αυτήν τη ρύθμιση, όταν εκτελείται το σύστημα.  

Το παρακάτω παράδειγμα παρουσιάζει πώς μπορείτε να ελέγξετε και να ορίσετε το προεπιλεγμένο χρονοδιάγραμμα στον αλγόριθμο NOOP.  

Για την οικογένεια Debian διανομής:

###<a name="step-1view-the-current-io-scheduler"></a>Βήμα 1 προβολή την είσοδο/έξοδο χρονοδιαγράμματος
Χρησιμοποιήστε την ακόλουθη εντολή:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Θα δείτε παρακάτω εξόδου, που υποδηλώνει το τρέχον χρονοδιάγραμμα.  

    noop [deadline] cfq


###<a name="step-2-change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Βήμα 2. Αλλάξτε την τρέχουσα συσκευή (/ ανάπτυξης/sda) της εισόδου/εξόδου προγραμματισμού αλγόριθμο
Χρησιμοποιήστε τις παρακάτω εντολές:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

>[AZURE.NOTE] Ρύθμιση αυτής της πολιτικής για /dev/sda από μόνο του δεν είναι χρήσιμη. Πρέπει να οριστούν σε όλων των δίσκων δεδομένων όπου βρίσκεται η βάση δεδομένων.  

Θα πρέπει να βλέπετε το παρακάτω αποτέλεσμα, που υποδεικνύει ότι αυτό grub.cfg έχει αναδημιουργείται με επιτυχία και ότι το προεπιλεγμένο χρονοδιάγραμμα έχει ενημερωθεί σε NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Για την οικογένεια διανομής Redhat, χρειάζεστε μόνο την ακόλουθη εντολή:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

##<a name="configure-system-file-operations-settings"></a>Ρύθμιση παραμέτρων συστήματος αρχείου λειτουργίες
Μία βέλτιστη πρακτική είναι να απενεργοποιήσετε τη δυνατότητα καταγραφής atime στο σύστημα αρχείων. Atime είναι η ώρα τελευταίας πρόσβασης του αρχείου. Κάθε φορά που γίνεται πρόσβαση σε ένα αρχείο, το σύστημα αρχείων εγγραφών της χρονικής σήμανσης στο αρχείο καταγραφής. Ωστόσο, αυτές οι πληροφορίες χρησιμοποιείται σπάνια. Μπορείτε να το απενεργοποιήσετε εάν δεν χρειάζεστε, έτσι θα μειωθεί συνολική χρόνου πρόσβασης δίσκου.  

Για να απενεργοποιήσετε την καταγραφή atime, πρέπει να τροποποιήσετε το αρχείο συστήματος ρύθμισης παραμέτρων αρχείου /etc/ fstab και προσθέστε την επιλογή **noatime** .  

Για παράδειγμα, επεξεργαστείτε το αρχείο /etc/fstab vim, προσθέτοντας την noatime όπως φαίνεται παρακάτω.  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Στη συνέχεια, ενεργοποιήστε ξανά το σύστημα αρχείων με την ακόλουθη εντολή:  

    mount -o remount /RAID0

Ελέγξτε το αποτέλεσμα που έχουν τροποποιηθεί. Σημειώστε ότι όταν τροποποιείτε το αρχείο δοκιμή, την ώρα πρόσβασης δεν ενημερώνεται.  

Πριν από το παράδειγμα:     

![][5]

Μετά την παράδειγμα:

![][6]

##<a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a>Αυξήστε τον μέγιστο αριθμό του συστήματος χειρίζεται για υψηλή ταυτόχρονης εκτέλεσης
MySQL είναι υψηλή ταυτόχρονης εκτέλεσης βάση δεδομένων. Ο προεπιλεγμένος αριθμός ταυτόχρονες λαβές είναι 1024 για Linux, που δεν είναι πάντα αρκετό. **Χρησιμοποιήστε τα ακόλουθα βήματα για να αυξήσετε το μέγιστο ταυτόχρονες λαβές του συστήματος για την υποστήριξη υψηλής ταυτόχρονης εκτέλεσης του MySQL**.

###<a name="step-1-modify-the-limitsconf-file"></a>Βήμα 1: Τροποποίηση του αρχείου limits.conf
Προσθέστε τις ακόλουθες τέσσερις γραμμές στο αρχείο /etc/security/limits.conf για να αυξήσετε το μέγιστο επιτρεπόμενο ταυτόχρονες λαβές. Σημειώστε ότι ο μέγιστος αριθμός που μπορεί να υποστηρίξει το σύστημα 65536.   

    * Απαλή nofile 65536
    * δύσκολο nofile 65536
    * Απαλή nproc 65536
    * σκληρό nproc 65536

###<a name="step-2-update-the-system-for-the-new-limits"></a>Βήμα 2: Ενημερώστε το σύστημα για τα νέα όρια
Εκτελέστε τις εξής εντολές:  

    ulimit -SHn 65536
    ulimit -SHu 65536

###<a name="step-3-ensure-that-the-limits-are-updated-at-boot-time"></a>Βήμα 3: Βεβαιωθείτε ότι τα όρια είναι ενημερωθεί κατά την εκκίνηση
Τοποθετήστε τις παρακάτω εντολές εκκίνησης στο αρχείο /etc/rc.local, ώστε να θα διαρκέσει εφέ κατά τη διάρκεια κάθε φορά εκκίνησης.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

##<a name="mysql-database-optimization"></a>Βελτιστοποίηση βάση δεδομένων MySQL
Μπορείτε να χρησιμοποιήσετε την ίδια στρατηγική ρύθμισης επιδόσεων για ρύθμιση παραμέτρων MySQL στο Azure ως σε έναν υπολογιστή εσωτερικής εγκατάστασης.  

Το κύριο κανόνες βελτιστοποίηση εισόδου/εξόδου είναι:   

-   Αύξηση του μεγέθους cache.
-   Μειώστε το χρόνο απόκρισης εισόδου/εξόδου.  

Για να βελτιστοποιήσετε ρυθμίσεων διακομιστή MySQL, μπορείτε να ενημερώσετε το αρχείο my.cnf, το οποίο είναι το προεπιλεγμένο αρχείο ρύθμισης παραμέτρων για το διακομιστή και υπολογιστές-πελάτες.  

Τα ακόλουθα στοιχεία ρύθμισης παραμέτρων είναι οι βασικοί παράγοντες που επηρεάζουν τις επιδόσεις MySQL:  

-   **innodb_buffer_pool_size**: το χώρο συγκέντρωσης buffer περιέχει δεδομένα του buffer και το ευρετήριο. Αυτό συνήθως έχει οριστεί σε 70% φυσικής μνήμης.
-   **innodb_log_file_size**: αυτό είναι το μέγεθος του αρχείου καταγραφής Ακύρωση αναίρεσης. Μπορείτε να χρησιμοποιήσετε αρχεία καταγραφής από την ακύρωση αναίρεσης για να εξασφαλιστεί λειτουργίες εγγραφής γρήγορες, αξιόπιστη και ανακτήσιμα μετά από ένα σφάλμα. Αυτό έχει οριστεί σε 512MB, το οποίο θα σας δώσει αρκετό χώρο για την καταγραφή της λειτουργίες εγγραφής.
-   **max_connections**: ορισμένες φορές εφαρμογές μην κλείσετε συνδέσεις σωστά. Μια μεγαλύτερη τιμή θα σας δώσει το διακομιστή περισσότερο χρόνο για να Κάδος Ανακύκλωσης idled συνδέσεις. Το μέγιστο αριθμό συνδέσεων είναι 10000, αλλά η συνιστώμενη μέγιστη τιμή είναι 5000.
-   **Innodb_file_per_table**: Αυτή η ρύθμιση Ενεργοποίηση ή απενεργοποίηση της δυνατότητας του InnoDB να αποθηκεύσετε πίνακες σε ξεχωριστά αρχεία. Ενεργοποίηση της επιλογής εξασφαλίζει ότι διάφορες λειτουργίες για προχωρημένους διαχείρισης μπορούν να εφαρμοστούν αποτελεσματικά. Από επιδόσεων άποψη, να επιταχύνετε τη μετάδοση χώρο πίνακα και βελτιστοποίηση των επιδόσεων διαχείρισης υπολειμμάτων. Επομένως, η προτεινόμενη ρύθμιση για αυτό είναι Ενεργοποιημένη.</br>
    Από το MySQL 5.6, η προεπιλεγμένη ρύθμιση είναι Ενεργοποιημένη. Επομένως, δεν απαιτείται ενέργεια. Για άλλες εκδόσεις του, που είναι παλαιότερες από 5.6, οι προεπιλεγμένες ρυθμίσεις είναι ΑΠΕΝΕΡΓΟΠΟΙΗΜΈΝΗ. Ενεργοποίηση αυτή ON απαιτείται. Και θα πρέπει να εφαρμόσετε πριν από τη φόρτωση των δεδομένων, επειδή επηρεάζονται μόνο πίνακες που μόλις δημιουργήσατε.
-   **innodb_flush_log_at_trx_commit**: η προεπιλεγμένη τιμή είναι 1, με την εμβέλεια με 0 ~ 2. Η προεπιλεγμένη τιμή είναι η πιο κατάλληλη επιλογή για μεμονωμένη MySQL DB. Η ρύθμιση του 2 επιτρέπει την περισσότερες ακεραιότητα δεδομένων και είναι κατάλληλη για υπόδειγμα συμπλέγματος MySQL. Η ρύθμιση 0 επιτρέπει απώλεια δεδομένων, το οποίο μπορεί να επηρεάσει την αξιοπιστία, σε ορισμένες περιπτώσεις με καλύτερες επιδόσεις, και είναι κατάλληλη για εξαρτώμενη συμπλέγματος MySQL.
-   **Innodb_log_buffer_size**: το αρχείο καταγραφής buffer επιτρέπει συναλλαγές για να εκτελέσετε χωρίς να χρειάζεται να κάνετε εκκαθάριση του αρχείου καταγραφής στο δίσκο πριν από την ολοκλήρωση των συναλλαγών. Ωστόσο, εάν υπάρχει μεγάλη δυαδικό αντικείμενο ή πεδίο κειμένου, το cache θα καταναλωθεί πολύ γρήγορα και θα ενεργοποιηθεί είσοδο/έξοδο συχνές δίσκου. Είναι καλύτερα να αυξήσετε το μέγεθος buffer αν Innodb_log_waits κατάσταση μεταβλητή δεν είναι 0.
-   **query_cache_size**: Η καλύτερη επιλογή είναι να την απενεργοποιήσετε από την αρχή. Ορίστε query_cache_size με το 0 (αυτό είναι τώρα η προεπιλεγμένη ρύθμιση στο MySQL 5.6) και να χρησιμοποιήσετε άλλες μεθόδους για να επιταχύνετε ερωτήματα.  

Ανατρέξτε στο θέμα [Παράρτημα D](#AppendixD) για τη σύγκριση απόδοσης μετά από τη βελτιστοποίηση.


##<a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a>Ενεργοποίηση του αρχείου καταγραφής αργή ερωτήματος MySQL για την ανάλυση του συμφόρηση απόδοσης
Το αρχείο καταγραφής MySQL αργή ερωτήματος μπορεί να σας βοηθήσει να αναγνωρίζετε το αργή ερωτήματα για MySQL. Μετά την ενεργοποίηση του αρχείου καταγραφής MySQL καθυστέρηση του ερωτήματος, μπορείτε να χρησιμοποιήσετε εργαλεία MySQL όπως **mysqldumpslow** για τον προσδιορισμό του συμφόρηση απόδοσης.  

Σημειώστε ότι από προεπιλογή αυτή δεν είναι ενεργοποιημένη. Ενεργοποίηση του αρχείου καταγραφής αργή ερωτήματος μπορεί να εκμετάλλευση ορισμένους πόρους CPU. Επομένως, συνιστάται να ενεργοποιείτε αυτό προσωρινά για την αντιμετώπιση προβλημάτων επιδόσεων συνωστισμών.

###<a name="step-1-modify-mycnf-file-by-adding-the-following-lines-to-the-end"></a>Βήμα 1: Τροποποίηση αρχείου my.cnf, προσθέτοντας τις ακόλουθες γραμμές στο τέλος   

    long_query_time = 2
    slow_query_log = 1
    slow_query_log_file = /RAID0/mysql/mysql-slow.log

###<a name="step-2-restart-mysql-server"></a>Βήμα 2: Επανεκκίνηση διακομιστής mysql
    service  mysql  restart

###<a name="step-3-check-whether-the-setting-is-taking-effect-using-the-show-command"></a>Βήμα 3: Ελέγξτε αν η ρύθμιση διαρκεί εφέ χρησιμοποιώντας την εντολή "Εμφάνιση"

![][7]   

![][8]

Σε αυτό το παράδειγμα, μπορείτε να δείτε ότι έχει ενεργοποιηθεί η δυνατότητα καθυστέρηση του ερωτήματος. Στη συνέχεια, μπορείτε να χρησιμοποιήσετε το εργαλείο **mysqldumpslow** για να προσδιορίσετε συνωστισμών επιδόσεων και βελτιστοποίηση των επιδόσεων, όπως η προσθήκη ευρετήρια.





##<a name="appendix"></a>Προσάρτημα

Οι παρακάτω είναι δείγμα δεδομένων δοκιμής επιδόσεων που παράγεται στο περιβάλλον προορισμού εργαστήριο, που παρέχουν γενικές σχετικά με τις τάσεις απόδοσης δεδομένα με διαφορετικό επιδόσεων προσεγγίσεων, ωστόσο μπορεί να διαφέρουν από τα αποτελέσματα στην περιοχή διαφορετικές εκδόσεις περιβάλλον ή προϊόν.

<a name="AppendixA"></a>Προσάρτημα α:  
**Επιδόσεων δίσκου (IOP Προέλευσης) με RAID διαφορετικά επίπεδα**


![][9]

**Δοκιμή εντολές:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

>AZURE. ΣΗΜΕΊΩΣΗ: Το φόρτο εργασίας από αυτόν τον έλεγχο χρησιμοποιεί 64 νήματα, προσπαθεί να φτάσετε το ανώτατο όριο του RAID.

<a name="AppendixB"></a>Προσάρτημα β:  
**Σύγκριση επιδόσεων (μετάδοσης) MySQL με διαφορετικό RAID επίπεδα**   
(Σύστημα αρχείων XFS)


![][10]  
![][11]

**Δοκιμή εντολές:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Σύγκριση επιδόσεων (Συναλλαγής) MySQL με διαφορετικό RAID επίπεδα**  
![][12]

**Δοκιμή εντολές:**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

<a name="AppendixC"></a>Προσάρτημα γ:   
**Σύγκριση επιδόσεων (IOP Προέλευσης) δίσκου μπλοκ διαφορετικά μεγέθη**  
(Σύστημα αρχείων XFS)


![][13]

**Δοκιμή εντολές:**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

Σημείωση το μέγεθος του αρχείου που χρησιμοποιούνται για τη δοκιμή αυτό είναι 30GB και 1GB, αντίστοιχα, με RAID 0(4 disks) XFS πεδίου συστήματος.


<a name="AppendixD"></a>Προσάρτημα δ:  
**Σύγκριση επιδόσεων (μετάδοσης) MySQL πριν και μετά βελτιστοποίησης**  
(Σύστημα αρχείων XFS)


![][14]

**Δοκιμή εντολές:**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**Η ρύθμιση παραμέτρων για το προεπιλεγμένο και optmization είναι ως εξής:**

|Παράμετροι |Προεπιλεγμένη    |optmization
|-----------|-----------|-----------
|**innodb_buffer_pool_size**    |Κανένας   |7G
|**innodb_log_file_size**   |5M |512M
|**max_connections**    |100    |5000
|**innodb_file_per_table**  |0  |1
|**innodb_flush_log_at_trx_commit** |1  |2
|**innodb_log_buffer_size** |8M |128M
|**query_cache_size**   |16M    |0


Και πιο λεπτομερείς παραμέτρους ρυθμίσεων βελτιστοποίηση, ανατρέξτε στις οδηγίες επίσημη mysql.  

[http://dev.MySQL.com/doc/refman/5.6/EN/innodb-Configuration.HTML](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html)  

[http://dev.MySQL.com/doc/refman/5.6/EN/innodb-Parameters.HTML#sysvar_innodb_flush_method](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method)

**Περιβάλλον δοκιμής**  

|Υλικού   |Λεπτομέρειες
|-----------|-------
|CPU    |AMD Opteron(tm) επεξεργαστής 4171 HE / 4 πυρήνων
|Μνήμη |14G
|δίσκο   |10G/δίσκος
|λειτουργικό σύστημα |Ubuntu 14.04.1 Αποτελεσμάτων



[1]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]: ./media/virtual-machines-linux-classic-optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

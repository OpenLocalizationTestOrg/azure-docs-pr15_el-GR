<properties
    pageTitle="Εκτέλεση ένα σύμπλεγμα MariaDB (MySQL) σε Azure"
    description="Δημιουργήστε μια MariaDB + Galera MySQL συμπλέγματος σε εικονικές μηχανές Windows Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="sabbour"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="04/15/2015"
    ms.author="v-ahsab"/>

# <a name="mariadb-mysql-cluster---azure-tutorial"></a>Σύμπλεγμα MariaDB (MySQL) - Azure πρόγραμμα εκμάθησης

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

> [AZURE.NOTE]  Για μεγάλες επιχειρήσεις MariaDB σύμπλεγμα είναι τώρα διαθέσιμη από το Azure Marketplace.  Η νέα προσφορά που θα αναπτύξετε αυτόματα ένα σύμπλεγμα MariaDB Galera στο ARM. Θα πρέπει να χρησιμοποιήσετε τη νέα προσφορά από https://azure.microsoft.com/en-us/marketplace/partners/mariadb/cluster-maxscale/ 

Θα σας δημιουργείτε ένα σύμπλεγμα πολλαπλή [Galera](http://galeracluster.com/products/) [MariaDBs](https://mariadb.org/en/about/), αντικαθιστά έναν ισχυρό μεταβλητού μεγέθους και αξιόπιστη δοχεία MySQL, για να εργαστείτε σε περιβάλλον ιδιαίτερα διαθέσιμη σε εικονικές μηχανές Windows Azure.

## <a name="architecture-overview"></a>Επισκόπηση αρχιτεκτονικής

Αυτό το θέμα εκτελεί τα ακόλουθα βήματα:

1. Δημιουργήστε ένα σύμπλεγμα 3 κόμβου
2. Διαχωρίστε δίσκων δεδομένων από το δίσκο λειτουργικό σύστημα
3. Δημιουργία των δίσκων δεδομένων στο RAID 0/διαγραμμισμένου τη ρύθμιση για να αυξήσετε IOP Προέλευσης
4. Χρησιμοποιήστε το πρόγραμμα εξισορρόπησης φόρτου Azure για εξισορρόπηση του φόρτου για τους κόμβους 3
5. Για να ελαχιστοποιήσετε την επαναλαμβανόμενη εργασία, δημιουργήστε μια Εικονική εικόνα που περιέχει MariaDB + Galera και το χρησιμοποιήσετε για να δημιουργήσετε το άλλο σύμπλεγμα ΣΠΣ.

![Αρχιτεκτονική](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Setup.png)

> [AZURE.NOTE]  Αυτό το θέμα χρησιμοποιεί τα εργαλεία [Azure CLI](../xplat-cli-install.md) , οπότε βεβαιωθείτε ότι για να κάνετε λήψη τους και να τα συνδέσετε στη συνδρομή σας Azure σύμφωνα με τις οδηγίες. Εάν χρειάζεστε μια αναφορά για τις διαθέσιμες εντολές στην το Azure CLI, δείτε αυτήν τη σύνδεση για την [Αναφορά εντολών Azure CLI](../virtual-machines-command-line-tools.md). Επίσης θα χρειαστεί να [δημιουργήσετε ένα κλειδί SSH για τον έλεγχο ταυτότητας] και σημειώστε τη **θέση του αρχείου .pem**.


## <a name="creating-the-template"></a>Δημιουργία προτύπου

### <a name="infrastructure"></a>Υποδομή

1. Δημιουργία μιας ομάδας συσχέτισης για τη διατήρηση μαζί τους πόρους

        azure account affinity-group create mariadbcluster --location "North Europe" --label "MariaDB Cluster"

2. Δημιουργήστε ένα εικονικό δίκτυο

        azure network vnet create --address-space 10.0.0.0 --cidr 8 --subnet-name mariadb --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --affinity-group mariadbcluster mariadbvnet

3. Δημιουργία λογαριασμού χώρου αποθήκευσης για τη φιλοξενία όλων των δίσκων μας. Σημείωση που που δεν πρέπει να τοποθετεί περισσότερους από 40 σε μεγάλο βαθμό χρησιμοποιείται δίσκων στον ίδιο λογαριασμό χώρου αποθήκευσης για να αποφύγετε πάτημα 20.000 ορίου χώρου αποθήκευσης λογαριασμού IOP Προέλευσης. Σε αυτήν την περίπτωση, είμαστε εκτός από αυτόν τον αριθμό, ώστε να θα αποθηκεύονται όλα τα στοιχεία στον ίδιο λογαριασμό για απλότητα

        azure storage account create mariadbstorage --label mariadbstorage --affinity-group mariadbcluster

3. Βρείτε το όνομα της εικόνας CentOS 7 εικονική μηχανή

        azure vm image list | findstr CentOS
Αυτό θα εξόδου κάπως `5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926`. Χρησιμοποιήστε το όνομα στο ακόλουθο βήμα.

4. Δημιουργία του προτύπου Εικονική αντικαθιστώντας **/path/to/key.pem** με τη διαδρομή όπου είναι αποθηκευμένο το κλειδί που δημιουργήθηκε .pem SSH

        azure vm create --virtual-network-name mariadbvnet --subnet-names mariadb --blob-url "http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-os.vhd"  --vm-size Medium --ssh 22 --ssh-cert "/path/to/key.pem" --no-ssh-password mariadbtemplate 5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20140926 azureuser

5. Επισύναψη 4 x 500GB δεδομένων δίσκων η Εικονική για χρήση στη ρύθμιση παραμέτρων RAID

        FOR /L %d IN (1,1,4) DO azure vm disk attach-new mariadbhatemplate 512 http://mariadbstorage.blob.core.windows.net/vhds/mariadbhatemplate-data-%d.vhd

6. SSH στο πρότυπο Εικονική που δημιουργήσατε στο **mariadbhatemplate.cloudapp.net:22** και συνδεθείτε χρησιμοποιώντας το ιδιωτικό κλειδί.

### <a name="software"></a>Λογισμικό

1. Λήψη ρίζας

        sudo su

2. Εγκαταστήστε το RAID υποστήριξης:

     - Εγκατάσταση mdadm

                yum install mdadm

     - Δημιουργία της ρύθμισης παραμέτρων RAID0/γραμμή με το σύστημα αρχείων EXT4

                mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf
                mdadm --detail --scan >> /etc/mdadm.conf
                mkfs -t ext4 /dev/md0

     - Δημιουργία του καταλόγου σημείο ενεργοποίησης

                mkdir /mnt/data

     - Ανάκτηση το UUID της συσκευής που έχουν δημιουργηθεί πρόσφατα RAID

                blkid | grep /dev/md0

     - Επεξεργασία /etc/fstab

                vi /etc/fstab

     - Προσθήκη της συσκευής στο εκεί για να ενεργοποιήσετε την αυτόματη mouting στην αντικαθιστώντας το UUID με την τιμή που προκύπτει από την εντολή **blkid** πριν από την επανεκκίνηση

                UUID=<UUID FROM PREVIOUS>   /mnt/data ext4   defaults,noatime   1 2

     - Η νέα διαμερίσματα ενεργοποίησης

                mount /mnt/data

3. Εγκαταστήστε το MariaDB:

     - Δημιουργήστε το αρχείο MariaDB.repo:

                vi /etc/yum.repos.d/MariaDB.repo

     - Γέμισμα με το παρακάτω περιεχόμενο

                [mariadb]
                name = MariaDB
                baseurl = http://yum.mariadb.org/10.0/centos7-amd64
                gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
                gpgcheck=1

     - Κατάργηση υπάρχοντος επιθεματική και mariadb-βιβλιοθηκών για να αποφύγετε διενέξεις

            yum remove postfix mariadb-libs-*

     - Εγκατάσταση MariaDB με Galera

            yum install MariaDB-Galera-server MariaDB-client galera

4. Μετακίνηση στον κατάλογο δεδομένων MySQL στη συσκευή RAID μπλοκ

     - Αντιγραφή του τρέχοντος καταλόγου MySQL σε νέα θέση και να καταργήσετε το παλιό καταλόγου

            cp -avr /var/lib/mysql /mnt/data  
            rm -rf /var/lib/mysql

     - Ορισμός δικαιωμάτων στο νέο κατάλογο αντίστοιχα

            chown -R mysql:mysql /mnt/data && chmod -R 755 /mnt/data/  

     - Δημιουργία μιας symlink που δείχνει το παλιό καταλόγου στη νέα θέση στην τα διαμερίσματα RAID

            ln -s /mnt/data/mysql /var/lib/mysql

5. Επειδή [SELinux θα παρεμβάλλεται με τις λειτουργίες συμπλέγματος](http://galeracluster.com/documentation-webpages/configuration.html#selinux), είναι απαραίτητο να απενεργοποιήσετε για την τρέχουσα περίοδο λειτουργίας (μέχρι να εμφανιστεί μια συμβατή έκδοση). Επεξεργασία `/etc/selinux/config` για να το απενεργοποιήσετε για μετέπειτα επανεκκίνηση του:

            setenforce 0

       then editing `/etc/selinux/config` to set `SELINUX=permissive`

6. Επικυρώστε MySQL εκτελείται

    - Έναρξη MySQL

            service mysql start

    - Ασφαλούς την εγκατάσταση MySQL, να ορίσετε τον κωδικό πρόσβασης του ριζικού, να καταργήσετε ανώνυμους χρήστες, απενεργοποίηση σύνδεσης απομακρυσμένος ριζικός και την κατάργηση της βάσης δεδομένων δοκιμής

            mysql_secure_installation

    - Δημιουργήστε ένα χρήστη από τη βάση δεδομένων για το σύμπλεγμα πράξεις και προαιρετικά, των εφαρμογών σας

            mysql -u root -p
            GRANT ALL PRIVILEGES ON *.* TO 'cluster'@'%' IDENTIFIED BY 'p@ssw0rd' WITH GRANT OPTION; FLUSH PRIVILEGES;
            exit

   - Διακοπή MySQL

            service mysql stop

7. Δημιουργία πλαισίου κράτησης θέσης παραμέτρων

    - Επεξεργασία της ρύθμισης παραμέτρων MySQL για να δημιουργήσετε ένα σύμβολο κράτησης θέσης για τις ρυθμίσεις σύμπλεγμα. Να μην αντικατασταθεί το **`<Vairables>`** ή ενεργοποίηση τώρα. Που θα προκύψει αφού δημιουργούμε μια Εικονική από αυτό το πρότυπο.

            vi /etc/my.cnf.d/server.cnf

    - Επεξεργασία της ενότητας **[galera]** και καταργήστε την επιλογή από το

    - Επεξεργασία της ενότητας **[mariadb]**

            wsrep_provider=/usr/lib64/galera/libgalera_smm.so
            binlog_format=ROW
            wsrep_sst_method=rsync
            bind-address=0.0.0.0 # When set to 0.0.0.0, the server listens to remote connections
            default_storage_engine=InnoDB
            innodb_autoinc_lock_mode=2

            wsrep_sst_auth=cluster:p@ssw0rd # CHANGE: Username and password you created for the SST cluster MySQL user
            #wsrep_cluster_name='mariadbcluster' # CHANGE: Uncomment and set your desired cluster name
            #wsrep_cluster_address="gcomm://mariadb1,mariadb2,mariadb3" # CHANGE: Uncomment and Add all your servers
            #wsrep_node_address='<ServerIP>' # CHANGE: Uncomment and set IP address of this server
            #wsrep_node_name='<NodeName>' # CHANGE: Uncomment and set the node name of this server

8. Ανοιχτές θύρες απαιτείται το τείχος προστασίας (χρησιμοποιώντας FirewallD σε CentOS 7)

    - MySQL:`firewall-cmd --zone=public --add-port=3306/tcp --permanent`
    - GALERA:`firewall-cmd --zone=public --add-port=4567/tcp --permanent`
    - GALERA ΠΛΗΡΟΦΟΡΊΑΣ:`firewall-cmd --zone=public --add-port=4568/tcp --permanent`
    - RSYNC:`firewall-cmd --zone=public --add-port=4444/tcp --permanent`
    - Επανάληψη φόρτωσης του τείχους προστασίας:`firewall-cmd --reload`

9.  Βελτιστοποίηση του συστήματος για τις επιδόσεις. Ανατρέξτε σε αυτό το άρθρο για περισσότερες λεπτομέρειες [στρατηγικής ρύθμισης επιδόσεων](virtual-machines-linux-classic-optimize-mysql.md)

    - Επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων MySQL ξανά

            vi /etc/my.cnf.d/server.cnf

    - Επεξεργασία της ενότητας **[mariadb]** και προσάρτηση το παρακάτω

    > [AZURE.NOTE] Συνιστάται **innodb\_buffer\_pool_size** είναι 70% σας Εικονική μνήμη. Το έχει οριστεί στο εδώ 2.45GB για το μέσο Εικονική Azure με 3,5 GB RAM.

            innodb_buffer_pool_size = 2508M # The buffer pool contains buffered data and the index. This is usually set to 70% of physical memory.
            innodb_log_file_size = 512M #  Redo logs ensure that write operations are fast, reliable, and recoverable after a crash
            max_connections = 5000 # A larger value will give the server more time to recycle idled connections
            innodb_file_per_table = 1 # Speed up the table space transmission and optimize the debris management performance
            innodb_log_buffer_size = 128M # The log buffer allows transactions to run without having to flush the log to disk before the transactions commit
            innodb_flush_log_at_trx_commit = 2 # The setting of 2 enables the most data integrity and is suitable for Master in MySQL cluster
            query_cache_size = 0

10. Διακοπή MySQL, απενεργοποίηση MySQL υπηρεσίας από την εκτέλεση κατά την εκκίνηση για να αποφύγετε καταστρέψετε το σύμπλεγμα κατά την προσθήκη ενός νέου κόμβου και deprovision υπολογιστή.

        service mysql stop
        chkconfig mysql off
        waagent -deprovision

11. Καταγράψτε τα Εικονική μέσω της πύλης. (Προς το παρόν, [θέμα 1268 # στο το Azure CLI] εργαλεία περιγράφει το γεγονός ότι εικόνες καταγράφονται από τα εργαλεία Azure CLI καταγράψετε δίσκων συνημμένα δεδομένα).

    - Τερματισμός υπολογιστή μέσω της πύλης
    - Κάντε κλικ στην εντολή καταγραφή, καθορίστε το όνομα εικόνας ως **mariadb-galera-εικόνας** και δώστε μια περιγραφή και έλεγχος "Έχουν εκτέλεση waagent".
    ![Καταγραφή η εικονική μηχανή](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture.png)
    ![καταγραφή η εικονική μηχανή](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Capture2.PNG)

## <a name="creating-the-cluster"></a>Δημιουργία του συμπλέγματος

Δημιουργία 3 ΣΠΣ από το πρότυπο που μόλις δημιουργήσατε και, στη συνέχεια, ρύθμιση παραμέτρων και ξεκινήστε το σύμπλεγμα.

1. Δημιουργία του πρώτου Εικονική 7 CentOS από την εικόνα **mariadb-galera-εικόνα** που δημιουργήσατε, εφόσον το όνομα εικονικού δικτύου **mariadbvnet** και υποδικτύου **mariadb**, υπολογιστή μέγεθος **Μεσαίο**που περνά μέσα στην υπηρεσία Cloud δώστε ένα όνομα για να **mariadbha** (ή όποιο όνομα θέλετε να είναι δυνατή η πρόσβαση μέσω mariadbha.cloudapp.net), ρύθμιση του ονόματος αυτού του υπολογιστή να είναι **mariadb1** και το όνομα χρήστη για να **azureuser**και ενεργοποίηση της πρόσβασης SSH και διέρχεται η SSH πιστοποιητικών αρχείο .pem και αντικατάστασης **/path/to/key.pem** με τη διαδρομή όπου έχετε είναι αποθηκευμένο το κλειδί που δημιουργήθηκε .pem SSH.

    > [AZURE.NOTE] Οι παρακάτω εντολές έχουν χωριστεί σε πολλές γραμμές για σαφήνεια, αλλά πρέπει να εισαγάγετε κάθε ως μία γραμμή.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 22
        --vm-name mariadb1
        mariadbha mariadb-galera-image azureuser

2. Δημιουργία 2 περισσότερες εικονικές μηχανές _συνδέοντας_ τους σε αυτήν τη στιγμή που έχουν δημιουργηθεί **mariadbha** υπηρεσία Cloud, αλλάζοντας το **όνομα Εικονική** , καθώς και τη **θύρα SSH** σε μια μοναδική θύρα δεν βρίσκονται σε διένεξη με άλλες ΣΠΣ στην ίδια υπηρεσία Cloud.

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 23
        --vm-name mariadb2
        --connect mariadbha mariadb-galera-image azureuser
καθώς και για MariaDB3

        azure vm create
        --virtual-network-name mariadbvnet
        --subnet-names mariadb
        --availability-set clusteravset
        --vm-size Medium
        --ssh-cert "/path/to/key.pem"
        --no-ssh-password
        --ssh 24
        --vm-name mariadb3
        --connect mariadbha mariadb-galera-image azureuser

3. Θα πρέπει να λάβετε την εσωτερική διεύθυνση IP κάθε του ΣΠΣ 3 για το επόμενο βήμα:

    ![Λήψη της διεύθυνσης IP](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/IP.png)

4. SSH σε του 3 ΣΠΣ και και να επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων σε κάθε

        sudo vi /etc/my.cnf.d/server.cnf

    uncommenting **`wsrep_cluster_name`** και **`wsrep_cluster_address`** , καταργώντας το **#** στην αρχή και στο επικύρωσης είναι στην πραγματικότητα αυτό που θέλετε.
    Επιπλέον, αντικαταστήστε **`<ServerIP>`** στο **`wsrep_node_address`** και **`<NodeName>`** σε **`wsrep_node_name`** με την εικονική Μηχανή IP διεύθυνση και δώστε ένα όνομα αντίστοιχα και καταργήστε τα σχόλια από καθώς και αυτές τις γραμμές.

5. Εκκίνηση στο σύμπλεγμα σε MariaDB1 και αφήστε να γίνει εκτέλεση κατά την εκκίνηση

        sudo service mysql bootstrap
        chkconfig mysql on

6. Έναρξη MySQL σε MariaDB2 και MariaDB3 και αφήστε να γίνει εκτέλεση κατά την εκκίνηση

        sudo service mysql start
        chkconfig mysql on

## <a name="load-balancing-the-cluster"></a>Το σύμπλεγμα εξισορρόπησης φόρτου
Όταν δημιουργήσατε το ομαδοποιημένων ΣΠΣ, τις προσθέσατε σε μια διαθεσιμότητας Ορισμός που ονομάζεται **clusteravset** για να βεβαιωθείτε ότι που τίθενται σε διαφορετική σφαλμάτων και να ενημερώσετε τους τομείς και ότι Azure ποτέ κάνει συντήρησης σε όλους τους υπολογιστές ταυτόχρονα. Αυτή η ρύθμιση παραμέτρων πληροί τις απαιτήσεις υποστήριξη από αυτό Azure Service σύμβαση (SLA).

Τώρα μπορείτε να χρησιμοποιήσετε τη μονάδα εξισορρόπησης φόρτου Azure να εξισορροπήσετε αιτήσεις μεταξύ τους κόμβους 3.

Εκτελέστε την παρακάτω εντολές στον υπολογιστή σας χρησιμοποιώντας το Azure CLI.
Η δομή παράμετροι εντολή είναι:`azure vm endpoint create-multiple <MachineName> <PublicPort>:<VMPort>:<Protocol>:<EnableDirectServerReturn>:<Load Balanced Set Name>:<ProbeProtocol>:<ProbePort>`

    azure vm endpoint create-multiple mariadb1 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb2 3306:3306:tcp:false:MySQL:tcp:3306
    azure vm endpoint create-multiple mariadb3 3306:3306:tcp:false:MySQL:tcp:3306

Τέλος, επειδή η CLI ορίζει το χρονικό διάστημα δοκιμή του μονάδα εξισορρόπησης φόρτου 15 δευτερολέπτων (το οποίο μπορεί να είναι λίγο πολύ χρόνο), αλλάξετε στην πύλη του στην περιοχή **τα τελικά σημεία** για οποιαδήποτε από τα ΣΠΣ

![Επεξεργασία τελικού σημείου](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint.PNG)

στη συνέχεια, κάντε κλικ στην εντολή Ορισμός Reconfigure The Load-Balanced και μεταβείτε Επόμενο

![Νέα ρύθμιση παραμέτρων φόρτωση ισορροπημένες συνόλου](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint2.PNG)

στη συνέχεια, αλλάξτε το χρονικό διάστημα Διερεύνηση σε 5 δευτερόλεπτα και αποθήκευση

![Αλλαγή δοκιμή του διαστήματος](./media/virtual-machines-linux-classic-mariadb-mysql-cluster/Endpoint3.PNG)

## <a name="validating-the-cluster"></a>Επικύρωση του συμπλέγματος

Μπορεί να γίνει τη σκληρή εργασία σας. Το σύμπλεγμα θα πρέπει να είναι τώρα δυνατή η πρόσβαση στο `mariadbha.cloudapp.net:3306` που θα πατήσετε τη μονάδα εξισορρόπησης φόρτου και δρομολόγηση αιτήσεις μεταξύ του 3 ΣΠΣ ομαλά και αποτελεσματικά.

Χρησιμοποιήστε τις αγαπημένες προγράμματος-πελάτη MySQL για να συνδεθείτε ή απλώς σύνδεση από ένα από τα ΣΠΣ για να επαληθεύσετε λειτουργεί αυτό το σύμπλεγμα.

     mysql -u cluster -h mariadbha.cloudapp.net -p

Στη συνέχεια, δημιουργήστε μια νέα βάση δεδομένων και συμπληρώστε τον με ορισμένα δεδομένα

    CREATE DATABASE TestDB;
    USE TestDB;
    CREATE TABLE TestTable (id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, value VARCHAR(255));
    INSERT INTO TestTable (value)  VALUES ('Value1');
    INSERT INTO TestTable (value)  VALUES ('Value2');
    SELECT * FROM TestTable;

Θα έχει ως αποτέλεσμα στον παρακάτω πίνακα

    +----+--------+
  	| id | value  |
    +----+--------+
  	|  1 | Value1 |
  	|  4 | Value2 |
    +----+--------+
    2 rows in set (0.00 sec)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο που δημιουργήσατε ένα 3 κόμβο MariaDB + Galera ιδιαίτερα διαθέσιμη σύμπλεγμα σε εικονικές μηχανές Windows Azure εκτελεί CentOS 7. Το ΣΠΣ είναι εξισορρόπηση με το πρόγραμμα εξισορρόπησης φόρτου Azure.

Ενδέχεται να θέλετε να Ρίξτε μια ματιά [ένας άλλος τρόπος για σύμπλεγμα MySQL σε Linux](virtual-machines-linux-classic-mysql-cluster.md) και τρόπους για να [βελτιστοποιήσετε και δοκιμή επιδόσεων MySQL σε VM Linux Azure](virtual-machines-linux-classic-optimize-mysql.md).

<!--Anchors-->
[Architecture overview]: #architecture-overview
[Creating the template]: #creating-the-template
[Creating the cluster]: #creating-the-cluster
[Load balancing the cluster]: #load-balancing-the-cluster
[Validating the cluster]: #validating-the-cluster
[Next steps]: #next-steps

<!--Image references-->

<!--Link references-->
[Galera]: http://galeracluster.com/products/
[MariaDBs]: https://mariadb.org/en/about/
[Δημιουργία ενός κλειδιού SSH για τον έλεγχο ταυτότητας]:http://www.jeff.wilcox.name/2013/06/secure-linux-vms-with-ssh-certificates/
[θέμα #1268 στα το CLI Azure]:https://github.com/Azure/azure-xplat-cli/issues/1268

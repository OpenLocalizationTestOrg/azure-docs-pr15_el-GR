<properties
    pageTitle="Ρύθμιση του MySQL σε μια Εικονική Linux | Microsoft Azure "
    description="Μάθετε πώς να εγκαταστήσετε το MySQL στοίβας σε μια εικονική μηχανή Linux (Ubuntu ή RedHat οικογένεια OS) στο Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="how-to-install-mysql-on-azure"></a>Πώς να εγκαταστήσετε MySQL σε Azure


Σε αυτό το άρθρο, θα μάθετε πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους MySQL σε ένα Azure εικονική μηχανή εκτελείται Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


##<a name="install-mysql-on-your-virtual-machine"></a>Εγκατάσταση MySQL στον υπολογιστή σας εικονικές

> [AZURE.NOTE] Πρέπει να έχετε ήδη μια εικονική μηχανή Microsoft Azure εκτελείτε Linux για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης. Ανατρέξτε στο θέμα στην [Εικονική Linux Azure πρόγραμμα εκμάθησης](virtual-machines-linux-quick-create-cli.md) για να δημιουργήσετε και να ρυθμίσετε μια Εικονική Linux με `mysqlnode` ως το όνομα Εικονική και `azureuser` ως χρήστης πριν να συνεχίσετε.

Σε αυτήν την περίπτωση, χρησιμοποιήστε θύρα 3306 ως τη θύρα MySQL.  

Σύνδεση με το Linux Εικονική που δημιουργήσατε μέσω putty. Εάν αυτή είναι η πρώτη φορά που θα χρησιμοποιήσετε Εικονική Linux Azure, δείτε πώς μπορείτε να χρησιμοποιήσετε putty σύνδεση με μια Εικονική Linux [εδώ](virtual-machines-linux-mac-create-ssh-keys.md).

Θα χρησιμοποιήσουμε το πακέτο αποθετήριο δεδομένων για να εγκαταστήσετε το MySQL5.6 ως παράδειγμα σε αυτό το άρθρο. Έχει στην πραγματικότητα, MySQL5.6 περισσότερες βελτίωση των επιδόσεων από MySQL5.5.  Περισσότερες πληροφορίες [εδώ](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).


###<a name="how-to-install-mysql56-on-ubuntu"></a>Πώς να εγκαταστήσετε MySQL5.6 σε Ubuntu
Θα χρησιμοποιήσουμε Εικονική Linux με Ubuntu από το Azure εδώ.

- Βήμα 1: Εγκατάσταση MySQL Server 5.6 Μετάβαση στο `root` χρήστη:

            #[azureuser@mysqlnode:~]sudo su -

    Εγκαταστήστε το διακομιστή mysql 5.6:

            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6

    Κατά την εγκατάσταση, θα εμφανιστεί ένα παράθυρο διαλόγου παράθυρο poping προς τα επάνω για να σας ζητήσει να ορίσετε MySQL ριζικό κωδικό πρόσβασης και χρειάζεστε ορίστε τον κωδικό πρόσβασης εδώ.

    ![εικόνα](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p1.png)


    Εισαγάγετε τον κωδικό πρόσβασης για επιβεβαίωση.

    ![εικόνα](./media/virtual-machines-linux-mysql-install/virtual-machines-linux-install-mysql-p2.png)

- Βήμα 2: Σύνδεση MySQL Server

    Όταν ολοκληρώσετε την εγκατάσταση του διακομιστή MySQL, υπηρεσία MySQL θα ξεκινήσει αυτόματα. Μπορείτε να συνδεθείτε MySQL Server με `root` χρήστη.
    Χρησιμοποιήστε την παρακάτω εντολή για να συνδεθείτε και εισαγωγής κωδικού πρόσβασης.

             #[root@mysqlnode ~]# mysql -uroot -p

- Βήμα 3: Διαχείριση της υπηρεσίας MySQL που εκτελείται

    (α) γρήγορα την κατάσταση της υπηρεσίας MySQL

             #[root@mysqlnode ~]# service mysql status

    (β) ξεκινήστε την υπηρεσία MySQL

             #[root@mysqlnode ~]# service mysql start

    (c) Διακοπή υπηρεσίας MySQL

             #[root@mysqlnode ~]# service mysql stop

    (d) επανεκκινήστε την υπηρεσία MySQL

             #[root@mysqlnode ~]# service mysql restart


###<a name="how-to-install-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a>Πώς να εγκαταστήσετε MySQL σε κόκκινο καπέλο OS οικογένεια όπως CentOS, Oracle Linux
Θα χρησιμοποιήσουμε Εικονική Linux με CentOS ή Oracle Linux εδώ.

- Βήμα 1: Προσθήκη το αποθετήριο δεδομένων MySQL Yum Μετάβαση σε `root` χρήστη:

            #[azureuser@mysqlnode:~]sudo su -

    Κάντε λήψη και εγκατάσταση του πακέτου κυκλοφορίας MySQL:

            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm

- Βήμα 2: Επεξεργασία κάτω από το αρχείο για να ενεργοποιήσετε το αποθετήριο δεδομένων MySQL για τη λήψη του πακέτου MySQL5.6.

            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo

    Ενημέρωση κάθε τιμή αυτού του αρχείου σε παρακάτω:

        \# *Enable to use MySQL 5.6*

        [mysql56-community]
        name=MySQL 5.6 Community Server

        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/

        enabled=1

        gpgcheck=1

        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

- Βήμα 3: MySQL εγκατάσταση από το αποθετήριο δεδομένων MySQL MySQL εγκατάσταση:

           #[root@mysqlnode ~]#yum install mysql-community-server

    Πακέτο MySQL RPM και όλα τα σχετικά πακέτα θα εγκατασταθεί.

- Βήμα 4: Διαχείριση της υπηρεσίας MySQL που εκτελείται

    (α) Ελέγξτε την κατάσταση υπηρεσίας ο διακομιστής MySQL:

           #[root@mysqlnode ~]#service mysqld status

    (β) Ελέγξτε αν εκτελείται στον προεπιλεγμένο διακομιστή θύρας του MySQL:

           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306


    (c) ξεκινήστε ο διακομιστής MySQL:

           #[root@mysqlnode ~]#service mysqld start

    (d) σταματήσει ο διακομιστής MySQL:

           #[root@mysqlnode ~]#service mysqld stop

    (e) MySQL σύνολο για να ξεκινήσετε όταν το σύστημα εκκίνησης προς τα επάνω:

           #[root@mysqlnode ~]#chkconfig mysqld on


###<a name="how-to-install-mysql-on-suse-linux"></a>Πώς να εγκαταστήσετε MySQL σε SUSE Linux
Θα χρησιμοποιήσουμε Εικονική Linux με OpenSUSE εδώ.

- Βήμα 1: Λήψη και εγκατάσταση του διακομιστή MySQL

    Μετάβαση στο `root` χρήστη μέσω κάτω από την εντολή:  

           #sudo su -

    Λήψη και εγκατάσταση του πακέτου MySQL:

           #[root@mysqlnode ~]# zypper update

           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql

- Βήμα 2: Διαχείριση της υπηρεσίας MySQL που εκτελείται

    (α) Ελέγξτε την κατάσταση του διακομιστή MySQL:

           #[root@mysqlnode ~]# rcmysql status

    (β) ελέγχου εάν η προεπιλεγμένη θύρα της ο διακομιστής MySQL:

           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306


    (c) ξεκινήστε ο διακομιστής MySQL:

           #[root@mysqlnode ~]# rcmysql start

    (d) σταματήσει ο διακομιστής MySQL:

           #[root@mysqlnode ~]# rcmysql stop

    (e) MySQL σύνολο για να ξεκινήσετε όταν το σύστημα εκκίνησης προς τα επάνω:

           #[root@mysqlnode ~]# insserv mysql

###<a name="next-step"></a>Επόμενο βήμα
Βρείτε περισσότερες πληροφορίες σχετικά με τις MySQL [εδώ](https://www.mysql.com/)και χρήση.

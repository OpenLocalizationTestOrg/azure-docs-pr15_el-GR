<properties
    pageTitle="Ενημέρωση του Azure παράγοντα Linux από GitHub | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να την ενημερωμένη έκδοση παράγοντας Linux Azure για το Εικονική Linux στα Azure στην έκδοση lateset από Github"
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
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Πώς μπορείτε να ενημερώσετε τον παράγοντα Linux Azure σε μια Εικονική στην πιο πρόσφατη έκδοση από GitHub

Για να ενημερώσετε τον [Παράγοντα Linux Azure](https://github.com/Azure/WALinuxAgent) σε μια Εικονική Linux στα Azure, πρέπει να έχετε ήδη:

1. Μια ενσωματωμένη Εικονική Linux στα Azure.
2. Μια σύνδεση που Εικονική Linux χρησιμοποιώντας SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Εάν θα εκτελέσετε αυτήν την εργασία από έναν υπολογιστή Windows, μπορείτε να χρησιμοποιήσετε PuTTY να SSH στον υπολογιστή σας Linux. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να συνδεθείτε στο μια εικονική μηχανή εκτελείται Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Θεωρηθεί Azure distros Linux τοποθετήσατε το πακέτο παράγοντας Linux Azure στο τους αποθετήρια, επομένως Ελέγξτε και εγκαταστήστε την πιο πρόσφατη έκδοση από συγκεκριμένο αποθετήριο Distro πρώτα εάν είναι δυνατό.  

Για Ubuntu, απλώς πληκτρολογήστε:

    #sudo apt-get install walinuxagent

Και σε CentOS, πληκτρολογήστε:

    #sudo yum install waagent


Για Oracle Linux, βεβαιωθείτε ότι το `Addons` αποθετήριο δεδομένων είναι ενεργοποιημένη. Επιλέξτε για να επεξεργαστείτε το αρχείο `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) ή `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), και αλλάξτε τη γραμμή `enabled=0` να `enabled=1` στην περιοχή **[ol6_addons]** ή **[ol7_addons]** σε αυτό το αρχείο.

Στη συνέχεια, για να εγκαταστήσετε την πιο πρόσφατη έκδοση του παράγοντα Linux Azure, πληκτρολογήστε:


    #sudo yum install WALinuxAgent

Εάν δεν μπορείτε να βρείτε το αρχείο φύλαξης πρόσθετο μπορείτε να προσθέσετε αυτές τις γραμμές απλώς στο τέλος του αρχείου σας .repo σύμφωνα με την έκδοση Oracle Linux σας:

Για Oracle Linux 6 εικονικές μηχανές:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Για Oracle Linux 7 εικονικές μηχανές:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Στη συνέχεια, πληκτρολογήστε:

    #sudo yum update WALinuxAgent

Αυτό είναι συνήθως όλες που χρειάζεστε, αλλά εάν για κάποιο λόγο πρέπει να το εγκαταστήσετε από https://github.com απευθείας, χρησιμοποιήστε τα ακόλουθα βήματα.


## <a name="install-wget"></a>Εγκατάσταση wget

Συνδεθείτε στο σας Εικονική χρησιμοποιώντας SSH.

Εγκατάσταση wget (υπάρχουν ορισμένες distros που δεν εγκαταστήσετε από προεπιλογή όπως Redhat, CentOS και Oracle Linux εκδόσεις 6.4 και 6.5), πληκτρολογώντας `#sudo yum install wget` στη γραμμή εντολών.


## <a name="download-the-latest-version"></a>Κάντε λήψη της πιο πρόσφατης έκδοσης

Ανοίξτε [την έκδοση του Azure Linux παράγοντας στο GitHub](https://github.com/Azure/WALinuxAgent/releases) σε μια ιστοσελίδα και μάθετε τον αριθμό πιο πρόσφατη έκδοση. (Μπορείτε να εντοπίσετε την τρέχουσα έκδοση, πληκτρολογώντας `#waagent --version`.)

### <a name="for-version-20x-type"></a>Για την έκδοση 2.0.x, πληκτρολογήστε:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Την ακόλουθη γραμμή χρησιμοποιεί την έκδοση 2.0.14 ως παράδειγμα:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Για την έκδοση 2.1.x ή νεότερη έκδοση, πληκτρολογήστε:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Την ακόλουθη γραμμή χρησιμοποιεί την έκδοση 2.1.0 ως παράδειγμα:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Εγκατάσταση του Azure Linux παράγοντα

### <a name="for-version-20x-use"></a>Για την έκδοση 2.0.x, χρησιμοποιήστε:

 Μετατροπή σε waagent εκτελέσιμο:

    #chmod +x waagent

 Αντιγραφή νέο το εκτελέσιμο αρχείο /usr/sbin /.

  Για περισσότερες Linux, χρησιμοποιήστε:

    #sudo cp waagent /usr/sbin

  Για CoreOS, χρησιμοποιήστε:

    #sudo cp waagent /usr/share/oem/bin/

  Εάν αυτή είναι μια νέα εγκατάσταση από τον παράγοντα Linux Azure, εκτελέστε:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Για την έκδοση 2.1.x, χρησιμοποιήστε:

Ίσως χρειαστεί να εγκαταστήσετε το πακέτο `setuptools` πρώτα--δείτε [εδώ](https://pypi.python.org/pypi/setuptools). Στη συνέχεια, εκτελέστε:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Επανεκκινήστε την υπηρεσία waagent

Για τα περισσότερα linux Distros:

    #sudo service waagent restart

Για Ubuntu, χρησιμοποιήστε:

    #sudo service walinuxagent restart

Για CoreOS, χρησιμοποιήστε:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Επιβεβαιώστε την έκδοση παράγοντας Linux Azure

    #waagent -version

Για CoreOS, την παραπάνω εντολή ενδέχεται να μην λειτουργούν.

Θα δείτε ότι η έκδοση παράγοντα Linux Azure έχει ενημερωθεί στη νέα έκδοση.

Για περισσότερες πληροφορίες σχετικά με τον παράγοντα Linux Azure, ανατρέξτε στο θέμα [Αρχείο README παράγοντας Linux Azure](https://github.com/Azure/WALinuxAgent).

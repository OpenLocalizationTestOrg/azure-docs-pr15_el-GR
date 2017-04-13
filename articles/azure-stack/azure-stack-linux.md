<properties
    pageTitle="Επισκέπτες Linux στη στοίβα Azure | Microsoft Azure"
    description="Μάθετε πώς να δημιουργήσετε βάσει Linux εικονικές μηχανές σε στοίβα Azure."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Ανάπτυξη Linux εικονικές μηχανές σε στοίβα Azure

Μπορείτε να αναπτύξετε Linux εικονικές μηχανές σε το Azure POC στοίβα, προσθέτοντας μια εικόνα που βασίζεται σε Linux σε το Azure Marketplace στοίβας. Διάφοροι προμηθευτές Linux έχετε δώσει εικόνες που μπορούν να προστεθούν σε μια POC στοίβας Azure ή μπορείτε να δημιουργήσετε το δικό σας.

## <a name="download-an-image"></a>Λήψη εικόνας

 1. Κάντε λήψη και εξαγάγετε μια εικόνα Azure συμβατό με στοίβα από τις παρακάτω συνδέσεις ή να προετοιμάσετε το δικό σας:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 Αποτελεσμάτων](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 Αποτελεσμάτων](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Εξαγάγετε την εικόνα VHD εάν είναι απαραίτητο και να [προσθέσετε την εικόνα για την αγορά](azure-stack-add-vm-image.md). Βεβαιωθείτε ότι το `OSType` παράμετρος έχει οριστεί σε `Linux`.
 
 3. Αφού προσθέσετε την εικόνα για να το Marketplace, δημιουργείται ένα στοιχείο Marketplace και μπορείτε να αναπτύξετε μια εικονική μηχανή Linux.
  
## <a name="prepare-your-own-image"></a>Προετοιμασία τη δική σας εικόνα

1. Προετοιμασία τη δική σας εικόνα Linux χρησιμοποιώντας μία από τις παρακάτω οδηγίες:
 - [Διανομή βάσει centOS](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Κόκκινο καπέλο Linux για μεγάλες επιχειρήσεις](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Λήψη και εγκατάσταση του [Azure Linux παράγοντα](https://github.com/Azure/WALinuxAgent/)

    Ο παράγοντας Linux Azure έκδοση 2.1.3 ή υψηλότερη απαιτείται για την παροχή σας Εικονική Linux στη στοίβα Azure. Πολλά από τα κατανομές που αναφέρονται παραπάνω ήδη περιλαμβάνουν αυτήν την έκδοση του παράγοντα ή υψηλότερη ως ένα πακέτο τους αποθετηρίων (συνήθως ονομάζεται `WALinuxAgent` ή `walinuxagent`). Ωστόσο, εάν η έκδοση του πακέτου Azure παράγοντας είναι μικρότερη από 2.1.3 (δηλαδή 2.0.18 ή κάτω), στη συνέχεια, πρέπει να εγκαταστήσετε τον παράγοντα με μη αυτόματο τρόπο. Εγκατεστημένη έκδοση μπορούν να προσδιοριστούν είτε από το όνομα του πακέτου είτε εκτελώντας `/usr/sbin/waagent -version` σε η Εικονική.

    Ακολουθήστε τις παρακάτω οδηγίες για να εγκαταστήσετε με μη αυτόματο τρόπο - τον παράγοντα Azure Linux

 - Πρώτα, κάντε λήψη του τελευταίου agent Azure Linux από [Github](https://github.com/Azure/WALinuxAgent/releases), το παράδειγμα:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Αποσυσκευασία τον παράγοντα Azure:

            # tar -vzxf v2.2.0.tar.gz

 - Εγκατάσταση python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Εγκαταστήστε το Azure παράγοντα:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Συστήματα με Python 2.x και Python 3.x εγκατεστημένο-παράθεση ίσως χρειαστεί να εκτελέσετε την ακόλουθη εντολή:

        # sudo python3 setup.py install --register-service

    Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα τον παράγοντα Linux Azure [README](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Προσθήκη στην εικόνα για να το Marketplace](azure-stack-add-vm-image.md). Βεβαιωθείτε ότι το `OSType` παράμετρος έχει οριστεί σε `Linux`.

4. Αφού προσθέσετε την εικόνα για να το Marketplace, δημιουργείται ένα στοιχείο Marketplace και μπορείτε να αναπτύξετε μια εικονική μηχανή Linux.

## <a name="next-steps"></a>Επόμενα βήματα

[Συνήθεις ερωτήσεις για τη στοίβα Azure](azure-stack-faq.md)
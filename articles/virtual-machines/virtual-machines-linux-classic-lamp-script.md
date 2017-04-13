<properties
    pageTitle="Χρησιμοποιήστε την επέκταση CustomScript σε μια Εικονική Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την επέκταση CustomScript για την ανάπτυξη εφαρμογών σε Linux εικονικές μηχανές στο Azure που δημιουργούνται με χρήση του μοντέλου κλασική ανάπτυξης."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Ανάπτυξη εφαρμογής ΛΆΜΠΑ χρησιμοποιώντας την επέκταση CustomScript Azure για Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Επέκταση της Microsoft Azure CustomScript για Linux παρέχει έναν τρόπο για να προσαρμόσετε τις εικονικές μηχανές (ΣΠΣ), εκτελώντας αυθαίρετο κώδικα γραμμένο σε οποιαδήποτε γλώσσα δέσμης ενεργειών που υποστηρίζονται από το Εικονική (για παράδειγμα, Python και πάρτι). Αυτό παρέχει ευέλικτες τρόπο για την αυτοματοποίηση ανάπτυξη εφαρμογών σε πολλούς υπολογιστές.

Μπορείτε να αναπτύξετε την επέκταση CustomScript χρησιμοποιώντας την πύλη του Azure κλασική, του Windows PowerShell ή το περιβάλλον γραμμής εντολών Azure (Azure CLI).

Σε αυτό το άρθρο θα χρησιμοποιήσουμε το Azure CLI για την ανάπτυξη μιας απλής εφαρμογής ΦΩΤΙΣΜΟΎ για μια Εικονική Ubuntu δημιουργούνται χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Για αυτό το παράδειγμα, πρέπει πρώτα να δημιουργήσετε δύο Azure ΣΠΣ με Ubuntu 14.04 ή νεότερη έκδοση. Το ΣΠΣ ονομάζονται *εικονική δέσμης ενεργειών* και *λάμπα εικονική*. Χρησιμοποιήστε μοναδικά ονόματα κατά τη δημιουργία του ΣΠΣ. Μία χρησιμοποιείται για να εκτελέσετε τις εντολές CLI και μία χρησιμοποιείται για την ανάπτυξη της εφαρμογής ΛΆΜΠΑ.

Επίσης, χρειάζεστε ένα λογαριασμό αποθήκευσης Azure και έναν αριθμό-κλειδί για να αποκτήσετε πρόσβαση σε αυτό (μπορείτε να βρείτε αυτή από την πύλη του Azure κλασική).

Εάν χρειάζεστε βοήθεια για τη δημιουργία ΣΠΣ Linux στην Azure αναφέρονται στο θέμα [Δημιουργία μια εικονική μηχανή εκτελείται Linux](virtual-machines-linux-classic-createportal.md).

Οι εντολές εγκατάσταση προϋποθέτουν Ubuntu, αλλά μπορείτε να προσαρμόσετε την εγκατάσταση για οποιοδήποτε υποστηριζόμενο distro Linux.

Η Εικονική εικονική δέσμης ενεργειών πρέπει να έχει CLI Azure εγκατάστασης, με μια σύνδεση εργασίας με Azure. Για βοήθεια σχετικά με αυτό ανατρέξτε για να [εγκαταστήσετε και να ρυθμίσετε το περιβάλλον γραμμής εντολών Azure](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Αποστολή μιας δέσμης ενεργειών

Θα χρησιμοποιήσουμε την επέκταση CustomScript να εκτελέσετε μια δέσμη ενεργειών σε έναν απομακρυσμένο Εικονική να εγκαταστήσετε στη στοίβα ΦΩΤΙΣΜΟΎ και να δημιουργήσετε μια σελίδα PHP. Για να έχετε πρόσβαση στη δέσμη ενεργειών από οπουδήποτε θα σας θα αποστείλετε το ως ένα αντικειμένων blob του Azure.

### <a name="script-overview"></a>Επισκόπηση δέσμης ενεργειών

Το παράδειγμα δέσμης ενεργειών εγκαθιστά μιας στοίβας ΛΆΜΠΑ να Ubuntu (συμπεριλαμβανομένου του ορισμού σιωπηρή εγκατάσταση του MySQL), συντάσσει ένα απλό αρχείο PHP και ξεκινά Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Αποστολή δέσμης ενεργειών

Αποθηκεύστε τη δέσμη ενεργειών ως αρχείο κειμένου, για παράδειγμα *install_lamp.sh*, και, στη συνέχεια, στείλτε το στο χώρο αποθήκευσης Azure. Μπορείτε να το κάνετε εύκολα με Azure CLI. Το παρακάτω παράδειγμα κάνει αποστολή του αρχείου σε ένα κοντέινερ χώρου αποθήκευσης με το όνομα "δέσμες ενεργειών". Εάν δεν υπάρχει το κοντέινερ θα πρέπει να δημιουργήσετε πρώτα.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Επίσης να δημιουργήσετε ένα αρχείο JSON που περιγράφει τον τρόπο για να κάνετε λήψη της δέσμης ενεργειών από το χώρο αποθήκευσης Azure. Αποθηκεύσετε ως *public_config.json* (αντικαθιστώντας "mystorage" με το όνομα του λογαριασμού σας χώρο αποθήκευσης):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Ανάπτυξη της επέκτασης

Τώρα μπορείτε να χρησιμοποιήσετε την επόμενη εντολή για να αναπτύξετε την επέκταση CustomScript Linux σε το απομακρυσμένο Εικονική χρησιμοποιώντας το Azure CLI.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Της προηγούμενης εντολής πραγματοποιεί λήψη και εκτελεί τη δέσμη ενεργειών *install_lamp.sh* στην η Εικονική που ονομάζεται *λάμπα εικονική*.

Επειδή η εφαρμογή περιλαμβάνει ένα διακομιστή web, να θυμάστε ότι για να ανοίξετε μια θύρα ακρόασης HTTP σε το απομακρυσμένο Εικονική με την επόμενη εντολή.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Παρακολούθηση και αντιμετώπιση προβλημάτων

Μπορείτε να ελέγξετε σε πόσο καλά η προσαρμοσμένη δέσμη ενεργειών εκτελείται, εξετάζοντας το αρχείο καταγραφής σε το απομακρυσμένο Εικονική. SSH να *λάμπα εικονική* και ουρά το αρχείο καταγραφής με την επόμενη εντολή.

    cd /var/log/azure/customscript
    tail -f handler.log

Αφού εκτελέσετε την επέκταση CustomScript, μπορείτε να περιηγηθείτε στη σελίδα PHP που δημιουργήσατε για πληροφορίες. Η σελίδα PHP για το παράδειγμα σε αυτό το άρθρο είναι *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

Μπορείτε να χρησιμοποιήσετε τα ίδια βασικά βήματα για την ανάπτυξη πιο σύνθετες εφαρμογές. Σε αυτό το παράδειγμα η δέσμη ενεργειών εγκατάστασης έχει αποθηκευτεί ως δημόσια blob στο χώρο αποθήκευσης Azure. Μια πιο ασφαλής επιλογή θα ήταν να αποθηκεύσετε τη δέσμη ενεργειών εγκατάσταση ως ασφαλή blob με μια [Ασφαλή πρόσβαση υπογραφή](https://msdn.microsoft.com/library/azure/ee395415.aspx) (συσχετισμών Ασφαλείας).

Πρόσθετοι πόροι για Azure CLI, Linux και την επέκταση CustomScript παρατίθενται Επόμενο.

[Αυτοματοποίηση εργασιών προσαρμογής Εικονική Linux χρήση CustomScript επέκτασης](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Επεκτάσεις Azure Linux (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux και Άνοιγμα-προέλευσης υπολογιστική στο Azure](virtual-machines-linux-opensource-links.md)

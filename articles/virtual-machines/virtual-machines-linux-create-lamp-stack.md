<properties
    pageTitle="Ανάπτυξη ΛΆΜΠΑ σε μια εικονική μηχανή Linux | Microsoft Azure"
    description="Μάθετε πώς να εγκαταστήσετε στη στοίβα ΛΆΜΠΑ σε μια Εικονική Linux"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>Ανάπτυξη στοίβα ΛΆΜΠΑ στο Azure
Σε αυτό το άρθρο θα σας καθοδηγήσει πώς να αναπτύξετε ένα διακομιστή web Apache, MySQL και PHP (στη στοίβα ΛΆΜΠΑ) στην Azure. Θα χρειαστείτε έναν λογαριασμό Azure ([λήψη δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/)) και το [Azure CLI](../xplat-cli-install.md) που είναι [συνδεδεμένο στο λογαριασμό σας Azure](../xplat-cli-connect.md).

Υπάρχουν δύο μέθοδοι για την εγκατάσταση του ΛΆΜΠΑ που αναφέρονται σε αυτό το άρθρο:

## <a name="quick-command-summary"></a>Γρήγορη εντολή σύνοψη

1) Ανάπτυξη ΛΆΜΠΑ στη νέα Εικονική

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) Ανάπτυξη ΛΆΜΠΑ στην υπάρχουσα Εικονική

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Ανάπτυξη ΛΆΜΠΑ στη νέα Εικονική αναλυτικές οδηγίες

Μπορείτε να ξεκινήσετε με τη δημιουργία νέας [ομάδας πόρων](../azure-resource-manager/resource-group-overview.md) που θα περιέχει η Εικονική:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Για να δημιουργήσετε την εικονική Μηχανή ίδια, μπορείτε να χρησιμοποιήσετε ένα ήδη χειρόγραφων πρότυπο διαχείρισης πόρων Azure βρέθηκε [εδώ στην GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Θα πρέπει να δείτε μια απάντηση στην ερώτηση ορισμένες περισσότερες εισόδων:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Τώρα έχετε δημιουργήσει μια Εικονική Linux με ΛΆΜΠΑ ήδη εγκατεστημένα σε αυτόν. Εάν θέλετε, μπορείτε να επαληθεύσετε την εγκατάσταση, μετάβαση προς τα κάτω, [επαλήθευση ΛΆΜΠΑ εγκαταστάθηκε με επιτυχία].

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>Ανάπτυξη ΛΆΜΠΑ στην υπάρχουσα Εικονική αναλυτικές οδηγίες

Εάν χρειάζεστε βοήθεια για τη δημιουργία μια Εικονική Linux κεντρικών [εδώ για να μάθετε πώς μπορείτε να δημιουργήσετε μια Εικονική Linux] (. / virtual-machines-linux-quick-create-cli.md). Στη συνέχεια, θα πρέπει να SSH σε η Εικονική Linux. Εάν χρειάζεστε βοήθεια σχετικά με τη δημιουργία ενός κλειδιού SSH κεντρικών [εδώ για να μάθετε πώς μπορείτε να δημιουργήσετε ένα κλειδί SSH στο Linux/Mac] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Εάν έχετε ήδη ένα κλειδί SSH, μεταβείτε στο μέλλον και SSH σε σας Εικονική Linux με `ssh username@uniqueDNS`.

Τώρα που εργάζεστε εντός του Εικονική Linux, σας θα σας καθοδηγήσουμε έως την εγκατάσταση στη στοίβα ΛΆΜΠΑ σε βάσει Debian κατανομές. Τα ακριβή εντολές ενδέχεται να διαφέρουν για άλλες distros Linux.

#### <a name="installing-on-debianubuntu"></a>Εγκατάσταση σε Debian/Ubuntu

Θα χρειαστείτε τα εξής πακέτα εγκατεστημένο: `apache2`, `mysql-server`, `php5`, και `php5-mysql`. Μπορείτε να εγκαταστήσετε αυτές τις απευθείας πιάνοντας αυτά τα πακέτα ή χρησιμοποιώντας Tasksel. Οδηγίες για τις δύο επιλογές παρατίθενται πιο κάτω.
Πριν από την εγκατάσταση, θα πρέπει να κάνετε λήψη και ενημέρωση λιστών πακέτο.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Μεμονωμένα πακέτα
Χρήση κατάλληλη λήψη:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Χρήση Tasksel
Εναλλακτικά μπορείτε να κάνετε λήψη Tasksel, ένα εργαλείο Debian/Ubuntu που εγκαθιστά πολλά σχετικά πακέτα ως συντονισμένη "εργασία" στο σύστημά σας.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Μετά την εκτέλεση του οποιαδήποτε από τις παραπάνω επιλογές θα σας ζητηθεί να εγκαταστήσετε αυτά τα πακέτα και έναν αριθμό από άλλα εξαρτήσεις. Πατήστε το πλήκτρο 'y' και, στη συνέχεια, 'Enter' για να συνεχίσετε, και ακολουθήστε τυχόν άλλες οδηγίες για να ορίσετε έναν κωδικό πρόσβασης διαχειριστή για MySQL. Αυτό θα εγκαταστήσει το ελάχιστο απαιτείται PHP επεκτάσεις χρειάζεται, για να χρησιμοποιήσετε PHP με MySQL. 

![][1]

Εκτελέστε την παρακάτω εντολή για να δείτε άλλες επεκτάσεις PHP που είναι διαθέσιμα ως πακέτα:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Δημιουργία εγγράφου info.php

Τώρα θα πρέπει να μπορείτε να ελέγξετε ποια έκδοση του Apache MySQL και PHP που έχετε μέσω της γραμμής εντολών, πληκτρολογώντας `apache2 -v`, `mysql -v`, ή `php -v`.

Εάν θέλετε να ελέγξετε περαιτέρω, μπορείτε να δημιουργήσετε μια γρήγορη σελίδα πληροφοριών PHP για την προβολή σε ένα πρόγραμμα περιήγησης. Δημιουργήστε ένα νέο αρχείο με νανομετρικής πρόγραμμα επεξεργασίας κειμένου με αυτήν την εντολή:

    user@ubuntu$ sudo nano /var/www/html/info.php

Το πρόγραμμα επεξεργασίας κειμένου του νανομετρικής GNU, προσθέστε τις ακόλουθες γραμμές:

    <?php
    phpinfo();
    ?>

Στη συνέχεια, αποθηκεύστε και κλείστε το πρόγραμμα επεξεργασίας κειμένου.

Επανεκκινήστε Apache με αυτήν την εντολή, ώστε όλα τα νέα εγκαταστάσεις θα ισχύσουν.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Επαλήθευση ΛΆΜΠΑ εγκαταστάθηκε με επιτυχία

Τώρα μπορείτε να ελέγξετε τη σελίδα πληροφορίες PHP που μόλις δημιουργήσατε στο πρόγραμμα περιήγησής σας μεταβαίνοντας στο http://youruniqueDNS/info.php, θα πρέπει να μοιάζει με αυτήν.

![][2]

Μπορείτε να ελέγξετε την εγκατάσταση του Apache, προβάλλοντας τη σελίδα προεπιλεγμένη Apache2 Ubuntu μεταβαίνοντας στο http://youruniqueDNS/ που. Θα πρέπει να δείτε κάτι παρόμοιο με αυτό.

![][3]

Συγχαρητήρια, έχετε απλώς ρύθμισης μιας στοίβας ΛΆΜΠΑ στην σας Εικονική Azure!

## <a name="next-steps"></a>Επόμενα βήματα

Δείτε την τεκμηρίωση Ubuntu στη στοίβα ΛΆΜΠΑ:

- [https://Help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png

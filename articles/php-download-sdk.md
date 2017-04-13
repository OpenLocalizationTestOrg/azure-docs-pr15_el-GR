<properties
    pageTitle="Κάντε λήψη του Azure SDK για PHP"
    description="Μάθετε πώς μπορείτε να κάνετε λήψη και εγκατάσταση του SDK Azure για PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Κάντε λήψη του Azure SDK για PHP

## <a name="overview"></a>Επισκόπηση

Το SDK Azure για PHP περιλαμβάνει στοιχεία που σας επιτρέπει να αναπτύξετε, ανάπτυξης και διαχείριση εφαρμογών PHP για Azure. Συγκεκριμένα, η SDK Azure για PHP περιλαμβάνει τα εξής:

* **Το PHP βιβλιοθήκες προγράμματος-πελάτη για Azure**. Αυτές οι βιβλιοθήκες κλάση παρέχει ένα περιβάλλον εργασίας για να αποκτήσετε πρόσβαση Azure δυνατότητες, όπως οι υπηρεσίες διαχείρισης δεδομένων και τις υπηρεσίες cloud.  
* **Το Azure περιβάλλον γραμμής εντολών για Mac, Linux, και Windows (Azure CLI)**. Αυτό είναι ένα σύνολο εντολών για την ανάπτυξη και τη διαχείριση των υπηρεσιών Azure, όπως τοποθεσίες Web Azure και εικονικές μηχανές Windows Azure. Η εργασία Azure CLI σε οποιαδήποτε πλατφόρμα, συμπεριλαμβανομένων των Mac, Linux και Windows.
* **Azure PowerShell (μόνο στα Windows)**. Αυτό είναι ένα σύνολο των cmdlet του PowerShell για την ανάπτυξη και τη διαχείριση των υπηρεσιών Azure, όπως τις υπηρεσίες Cloud και εικονικές μηχανές.
* **Το Azure Emulators (μόνο στα Windows)**. Το emulators υπολογισμού και χώρου αποθήκευσης είναι τοπικό emulators των υπηρεσιών cloud και υπηρεσίες διαχείρισης δεδομένων που σας επιτρέπουν να δοκιμάσετε μια εφαρμογή τοπικά. Το Emulators Azure εκτελούνται μόνο στα Windows.

Οι παρακάτω ενότητες περιγράφουν πώς μπορείτε να κάνετε λήψη και εγκαταστήστε τα στοιχεία που περιγράφονται παραπάνω.

Τις οδηγίες σε αυτό το θέμα, ας υποθέσουμε ότι έχετε [PHP] [ install-php] εγκατεστημένο.

> [AZURE.NOTE] Πρέπει να έχετε PHP 5,5 ή νεότερη έκδοση για να χρησιμοποιήσετε τις βιβλιοθήκες PHP προγράμματος-πελάτη για Azure.

##<a name="php-client-libraries-for-azure"></a>Βιβλιοθήκες του προγράμματος-πελάτη PHP για Azure

Οι βιβλιοθήκες PHP προγράμματος-πελάτη για Azure παρέχει ένα περιβάλλον εργασίας για να αποκτήσετε πρόσβαση Azure δυνατότητες, όπως οι υπηρεσίες διαχείρισης δεδομένων και στο cloud services, από οποιοδήποτε λειτουργικό σύστημα. Αυτές οι βιβλιοθήκες μπορεί να εγκατασταθεί μέσω του σύνθεσης.

Για πληροφορίες σχετικά με τη χρήση των βιβλιοθηκών PHP προγράμματος-πελάτη για Azure, δείτε [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία Blob][blob-service], [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία πίνακα] [ table-service] και [πώς μπορείτε να χρησιμοποιήσετε την υπηρεσία ουράς][queue-service].

###<a name="install-via-composer"></a>Εγκατάσταση μέσω σύνθεσης

1. [Εγκατάσταση Git][install-git].


    > [AZURE.NOTE] Στα Windows, θα πρέπει επίσης να προσθέσετε το εκτελέσιμο Git τη μεταβλητή περιβάλλοντος PATH.

2. Δημιουργήστε ένα αρχείο με το όνομα **composer.json** στον ριζικό κατάλογο του έργου σας και προσθέστε τον ακόλουθο κώδικα σε αυτήν:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Λήψη ** [composer.phar] [ composer-phar] ** στη ρίζα του έργου.

4. Ανοίξτε μια γραμμή εντολών και εκτέλεση αυτό στη ρίζα του έργου

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell και Azure Emulators

Azure PowerShell είναι ένα σύνολο των cmdlet του PowerShell για την ανάπτυξη και τη διαχείριση των υπηρεσιών Azure (όπως τις υπηρεσίες Cloud και εικονικές μηχανές). Το Emulators Azure είναι emulators των υπηρεσιών cloud και υπηρεσίες διαχείρισης δεδομένων που σας επιτρέπουν να δοκιμάσετε μια εφαρμογή τοπικά. Αυτά τα στοιχεία που υποστηρίζονται μόνο στα Windows.

Το συνιστώμενο τρόπο για να εγκαταστήσετε το Azure PowerShell και το Azure Emulators είναι να χρησιμοποιήσετε το [Πρόγραμμα εγκατάστασης του Microsoft Web πλατφόρμα][download-wpi]. Σημειώστε ότι μπορείτε επίσης να επιλέξετε να εγκαταστήσετε το άλλα στοιχεία ανάπτυξης, όπως PHP, SQL Server, τα προγράμματα οδήγησης της Microsoft για τον SQL Server για PHP και WebMatrix.

Για πληροφορίες σχετικά με τον τρόπο χρήσης του Azure PowerShell, δείτε [πώς μπορείτε να χρησιμοποιήσετε Azure PowerShell][powershell-tools].

##<a name="azure-cli"></a>Azure CLI

Το Azure CLI είναι ένα σύνολο εντολών για την ανάπτυξη και τη διαχείριση των υπηρεσιών Azure, όπως τοποθεσίες Web Azure και εικονικές μηχανές Windows Azure. Για πληροφορίες σχετικά με την εγκατάσταση του Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση του Azure CLI](xplat-cli-install.md).

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο για προγραμματιστές PHP](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git

<properties
        pageTitle="Επαναφορά κωδικού πρόσβασης Εικονική Linux και SSH αριθμού-κλειδιού από το CLI | Microsoft Azure"
        description="Πώς μπορείτε να χρησιμοποιήσετε την επέκταση VMAccess από το Azure περιβάλλον γραμμής εντολών (CLI) για να επαναφέρετε μια Εικονική Linux τον κωδικό πρόσβασης ή πλήκτρο SSH και επιδιόρθωση της ρύθμισης παραμέτρων SSH έλεγχος συνέπειας δίσκου"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Πώς μπορείτε να επαναφέρετε μια Εικονική Linux τον κωδικό πρόσβασης ή πλήκτρο SSH και επιδιόρθωση της ρύθμισης παραμέτρων SSH έλεγχος συνέπειας δίσκου χρησιμοποιώντας την επέκταση VMAccess


Εάν δεν μπορείτε να συνδεθείτε σε ένα εικονικό μηχάνημα Linux σε Azure εξαιτίας ενός ξεχασμένου κωδικού πρόσβασης, ένα λάθος πλήκτρο ασφαλούς κελύφους (SSH), ή ένα πρόβλημα με τη ρύθμιση παραμέτρων SSH, χρησιμοποιήστε την επέκταση VMAccessForLinux με το Azure CLI για να επαναφέρετε τον κωδικό πρόσβασης ή πλήκτρο SSH, διορθώσετε τη ρύθμιση παραμέτρων SSH και έλεγχος συνέπειας δίσκου. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Με το Azure CLI, χρησιμοποιείτε την εντολή **Ορισμός azure εικονική επέκταση** από το περιβάλλον γραμμής εντολών (πάρτι, Terminal, εντολών) για πρόσβαση σε εντολές. Εκτελέστε **την επέκταση εικονική azure Βοήθειας ρύθμιση** για χρήση λεπτομερείς επέκταση.

Με το Azure CLI, μπορείτε να κάνετε τις ακόλουθες εργασίες:

+ [Επαναφορά του κωδικού πρόσβασης](#pwresetcli)
+ [Επαναφορά του κλειδιού SSH](#sshkeyresetcli)
+ [Επαναφορά του κωδικού πρόσβασης και τον αριθμό-κλειδί SSH](#resetbothcli)
+ [Δημιουργία νέου λογαριασμού χρήστη sudo](#createnewsudocli)
+ [Επαναφέρει τη ρύθμιση παραμέτρων SSH](#sshconfigresetcli)
+ [Διαγραφή χρήστη](#deletecli)
+ [Εμφανίζει την κατάσταση της επέκτασης VMAccess](#statuscli)
+ [Έλεγχος συνέπειας προστέθηκε δίσκων](#checkdisk)
+ [Προστέθηκε δίσκων σας Εικονική Linux επιδιόρθωση](#repairdisk)


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Θα πρέπει να κάνετε τα εξής:

- Θα πρέπει να [εγκαταστήσετε το CLI Azure](../xplat-cli-install.md) και να [συνδεθείτε με τη συνδρομή σας](../xplat-cli-connect.md) για να χρησιμοποιήσετε Azure πόροι που σχετίζονται με το λογαριασμό σας.
- Ορίστε τη σωστή λειτουργία για το μοντέλο κλασική ανάπτυξης, πληκτρολογώντας τα εξής στη γραμμή εντολών:
        
        azure config mode asm
        
- Έχετε έναν νέο κωδικό πρόσβασης ή ένα σύνολο πλήκτρα SSH, εάν θέλετε να επαναφέρετε οποιοδήποτε από τα δύο. Δεν χρειάζεται αυτές εάν θέλετε να επαναφέρετε τις ρυθμίσεις παραμέτρων SSH.


## <a name="pwresetcli"></a>Επαναφορά του κωδικού πρόσβασης

1. Δημιουργήστε ένα αρχείο με το όνομα PrivateConf.json με αυτές τις γραμμές. Αντικαταστήστε τις αγκύλες και τα & #60; σύμβολο κράτησης θέσης & #62. τιμές με τις πληροφορίες σας.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Εκτέλεση αυτής της εντολής, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62..

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Επαναφορά του κλειδιού SSH

1. Δημιουργήστε ένα αρχείο με το όνομα PrivateConf.json με αυτά τα περιεχόμενα. Αντικαταστήστε τις αγκύλες και τα & #60; σύμβολο κράτησης θέσης & #62. τιμές με τις πληροφορίες σας.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Εκτέλεση αυτής της εντολής, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62..

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Επαναφορά του κωδικού πρόσβασης και τον αριθμό-κλειδί SSH

1. Δημιουργήστε ένα αρχείο με το όνομα PrivateConf.json με αυτά τα περιεχόμενα. Αντικαταστήστε τις αγκύλες και τα & #60; σύμβολο κράτησης θέσης & #62. τιμές με τις πληροφορίες σας.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Εκτέλεση αυτής της εντολής, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62..

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Δημιουργία νέου λογαριασμού χρήστη sudo

Εάν ξεχάσατε το όνομα χρήστη, μπορείτε να χρησιμοποιήσετε VMAccess για να δημιουργήσετε ένα νέο με την αρχή sudo. Σε αυτήν την περίπτωση, το υπάρχον όνομα χρήστη και τον κωδικό πρόσβασης δεν θα αλλάξει.

Για να δημιουργήσετε έναν νέο χρήστη sudo με πρόσβαση με κωδικό πρόσβασης, χρησιμοποιήστε τη δέσμη ενεργειών σε [Επαναφορά του κωδικού πρόσβασης](#pwresetcli) και καθορίστε το νέο όνομα χρήστη.

Για να δημιουργήσετε έναν νέο χρήστη sudo με SSH κλειδιού access, χρησιμοποιήστε τη δέσμη ενεργειών σε [Επαναφορά τον αριθμό-κλειδί SSH](#sshkeyresetcli) και καθορίστε το νέο όνομα χρήστη.

Μπορείτε επίσης να χρησιμοποιήσετε την [Επαναφορά του κωδικού πρόσβασης και τον αριθμό-κλειδί SSH](#resetbothcli) για να δημιουργήσετε έναν νέο χρήστη με τον κωδικό πρόσβασης και SSH πλήκτρων πρόσβασης.

## <a name="sshconfigresetcli"></a>Επαναφέρει τη ρύθμιση παραμέτρων SSH

Εάν η ρύθμιση παραμέτρων SSH βρίσκεται σε κατάσταση ανεπιθύμητα, που ενδέχεται να χαθούν επίσης πρόσβαση για την εικονική Μηχανή. Μπορείτε να χρησιμοποιήσετε την επέκταση VMAccess για να επαναφέρετε τις ρυθμίσεις παραμέτρων στην προεπιλεγμένη κατάσταση. Για να το κάνετε αυτό, μόλις πρέπει να ορίσετε το κλειδί "reset_ssh" στην "True". Την επέκταση θα επανεκκίνηση του διακομιστή SSH, ανοίξτε τη θύρα SSH σας Εικονική και επαναφορά της ρύθμισης παραμέτρων SSH στις προεπιλεγμένες τιμές. Ο λογαριασμός χρήστη (όνομα, τον κωδικό πρόσβασης ή πλήκτρα SSH) δεν θα αλλάξει.

> [AZURE.NOTE] Το αρχείο ρύθμισης παραμέτρων SSH που λαμβάνει επαναφορά βρίσκεται στο /etc/ssh/sshd_config.

1. Δημιουργήστε ένα αρχείο με το όνομα PrivateConf.json με αυτό το περιεχόμενο.

        {
        "reset_ssh":"True"
        }

2. Εκτέλεση αυτής της εντολής, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62.. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Διαγραφή χρήστη

Εάν θέλετε να διαγράψετε ένα λογαριασμό χρήστη χωρίς να συνδεθείτε στο για να την εικονική Μηχανή απευθείας, μπορείτε να χρησιμοποιήσετε αυτήν τη δέσμη ενεργειών.

1. Δημιουργήστε ένα αρχείο με το όνομα PrivateConf.json με αυτό το περιεχόμενο, αντικαθιστώντας το όνομα χρήστη για να καταργήσετε για & #60; usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Εκτέλεση αυτής της εντολής, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62.. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Εμφανίζει την κατάσταση της επέκτασης VMAccess

Για να εμφανίσετε την κατάσταση της επέκτασης VMAccess, εκτέλεση αυτής της εντολής.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< a = όνομα 'checkdisk' <</a>έλεγχος συνέπειας προστέθηκε δίσκων

Για να εκτελέσετε fsck στην όλων των δίσκων στον υπολογιστή σας εικονικές Linux, θα πρέπει να κάνετε τα εξής:

1. Δημιουργήστε ένα αρχείο με το όνομα PublicConf.json με αυτό το περιεχόμενο. Επιλέξτε μια τιμή boolean για το αν θέλετε να ελέγξετε δίσκων που συνδέονται με την εικονική μηχανή σας ή δεν απαιτείται δίσκος. 

        {   
        "check_disk": "true"
        }

2. Εκτέλεση αυτής της εντολής για να εκτελέσετε, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62..

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Επιδιόρθωση δίσκων 

Για να επιδιορθώσετε δίσκων που δεν είναι τοποθέτησης ή που έχουν σφάλματα ρύθμισης παραμέτρων ενεργοποίησης, χρησιμοποιήστε την επέκταση VMAccess για να επαναφέρετε τις ρυθμίσεις παραμέτρων ενεργοποίησης στον υπολογιστή σας εικονικές Linux. Αντικαθιστώντας το όνομα του δίσκου σας για & #60; yourdisk & #62..

1. Δημιουργήστε ένα αρχείο με το όνομα PublicConf.json με αυτό το περιεχόμενο. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Εκτέλεση αυτής της εντολής για να εκτελέσετε, αντικαθιστώντας το όνομα του σας εικονική μηχανή για & #60; εικονική όνομα & #62..

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Επόμενα βήματα

* Εάν θέλετε να χρησιμοποιήσετε το cmdlet του Azure PowerShell ή διαχείριση πόρων Azure πρότυπα για να επαναφέρετε τον κωδικό πρόσβασης ή πλήκτρο SSH, τη ρύθμιση παραμέτρων SSH, και έλεγχος συνέπειας δίσκου, ανατρέξτε στην [τεκμηρίωση για την επέκταση VMAccess σε GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Μπορείτε επίσης να χρησιμοποιήσετε την [πύλη του Azure](https://portal.azure.com) για να επαναφέρετε τον κωδικό πρόσβασης ή πλήκτρο SSH από μια Εικονική Linux αναπτυχθεί στο μοντέλο κλασική ανάπτυξης. Δεν μπορείτε να χρησιμοποιήσετε αυτήν τη στιγμή να μην πύλης με αυτήν για μια Εικονική Linux αναπτυχθεί στο μοντέλο ανάπτυξης διαχείρισης πόρων.

* Ανατρέξτε στο θέμα [σχετικά με τις δυνατότητες και επεκτάσεις εικονική μηχανή](virtual-machines-linux-extensions-features.md) για περισσότερες πληροφορίες σχετικά με τη χρήση επεκτάσεις Εικονική Azure εικονικές μηχανές.

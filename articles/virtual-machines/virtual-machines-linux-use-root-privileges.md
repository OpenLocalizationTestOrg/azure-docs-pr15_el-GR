<properties 
    pageTitle="Χρήση του ριζικού δικαιώματα σε εικονικές μηχανές Linux | Microsoft Azure" 
    description="Μάθετε πώς να χρησιμοποιείτε ριζικό δικαιώματα σε μια εικονική μηχανή Linux στο Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Χρήση του ριζικού δικαιώματα σε Linux εικονικές μηχανές στο Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Από προεπιλογή, το `root` χρήστη είναι απενεργοποιημένες σε εικονικές μηχανές Linux στο Azure. Οι χρήστες μπορούν να εκτελούν εντολές με αναβαθμισμένα δικαιώματα, χρησιμοποιώντας το `sudo` εντολή. Ωστόσο, την εμπειρία ενδέχεται να ποικίλλουν ανάλογα με τον τρόπο παρέχεται του συστήματος.

1. **Πλήκτρο SSH και τον κωδικό πρόσβασης ή τον κωδικό πρόσβασης μόνο** - η εικονική μηχανή παρέχεται με είτε ένα πιστοποιητικό (`.CER` αρχείο) ή πλήκτρο SSH, καθώς και έναν κωδικό πρόσβασης, ή απλώς ένα όνομα χρήστη και τον κωδικό πρόσβασης. Σε αυτήν την περίπτωση `sudo` θα γίνεται ερώτηση για τον κωδικό πρόσβασης του χρήστη πριν από την εκτέλεση της εντολής.

2. **Μόνο κλειδί SSH** - η εικονική μηχανή παρέχεται με ένα πιστοποιητικό (`.cer`, `.pem`, ή `.pub` αρχείο) ή πλήκτρο SSH, αλλά χωρίς τον κωδικό πρόσβασης.  Σε αυτήν την περίπτωση `sudo` **δεν θα** ζητείται ο κωδικός πρόσβασης του χρήστη πριν από την εκτέλεση της εντολής.


## <a name="ssh-key-and-password-or-password-only"></a>SSH κλειδί και τον κωδικό πρόσβασης ή τον κωδικό πρόσβασης μόνο

Συνδεθείτε με την εικονική μηχανή Linux με χρήση ελέγχου ταυτότητας για τον κωδικό πρόσβασης ή πλήκτρο SSH και, στη συνέχεια, εκτελέστε χρησιμοποιώντας εντολές `sudo`, για παράδειγμα:

    # sudo <command>
    [sudo] password for azureuser:

Σε αυτήν την περίπτωση ο χρήστης θα του ζητηθεί κωδικός πρόσβασης. Μετά την εισαγωγή του κωδικού πρόσβασης `sudo` θα εκτελεστεί η εντολή με `root` δικαιώματα.

Μπορείτε επίσης να ενεργοποιήσετε passwordless sudo με την επεξεργασία του `/etc/sudoers.d/waagent` αρχείο, για παράδειγμα:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Αυτή η αλλαγή θα επιτρέπεται passwordless sudo από το χρήστη "azureuser".

## <a name="ssh-key-only"></a>SSH βασικές μόνο

Συνδεθείτε με την εικονική μηχανή Linux με χρήση ελέγχου ταυτότητας κλειδιού SSH και, στη συνέχεια, εκτελέστε χρησιμοποιώντας εντολές `sudo`, για παράδειγμα:

    # sudo <command>

Σε αυτήν την περίπτωση θα ο χρήστης **δεν** θα σας ζητηθεί κωδικός πρόσβασης. Μετά την πατώντας το πλήκτρο `<enter>`, `sudo` θα εκτελεστεί η εντολή με `root` δικαιώματα.

 

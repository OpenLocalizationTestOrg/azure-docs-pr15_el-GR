<properties
    pageTitle="Προγραμματισμένη συντήρηση για ΣΠΣ Linux | Microsoft Azure"
    description="Κατανόηση των τι Azure προγραμματισμένη συντήρηση είναι και πώς επηρεάζει το Linux εικονικές μηχανές εκτελούνται στο Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-linux-virtual-machines-in-azure"></a>Προγραμματισμένη συντήρηση για Linux εικονικές μηχανές στο Azure

Κατανοήστε τι Azure προγραμματισμένη συντήρηση είναι και πώς μπορεί να επηρεάσει τη διαθεσιμότητα των σας εικονικές μηχανές Linux. Σε αυτό το άρθρο είναι επίσης διαθέσιμο για [εικονικές μηχανές Windows](virtual-machines-windows-planned-maintenance.md). 

Σε αυτό το άρθρο παρέχει ένα φόντο ως προς τη διαδικασία Azure προγραμματισμένη συντήρηση. Εάν θέλετε για την αντιμετώπιση προβλημάτων για ποιο λόγο το Εικονική επανεκκίνηση, μπορείτε να [διαβάσετε αυτό το ιστολόγιο δημοσίευση που περιγράφει με λεπτομέρεια προβολή Εικονική επανεκκίνηση της αρχεία καταγραφής](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Γιατί Azure εκτελεί προγραμματισμένη συντήρηση

Microsoft Azure εκτελεί περιοδικά ενημερώσεις σε ολόκληρο τον κόσμο για να βελτιώσετε την αξιοπιστία, επιδόσεων και της ασφάλειας της υποδομής του κεντρικού υπολογιστή, το οποίο βασίζεται εικονικές μηχανές. Πολλές από αυτές τις ενημερώσεις που εκτελούνται χωρίς επιπτώσεις στην εικονικές μηχανές ή τις υπηρεσίες Cloud, συμπεριλαμβανομένων των ενημερώσεων διατήρηση μνήμης.

Ωστόσο, ορισμένες ενημερωμένες εκδόσεις απαιτούν επανεκκίνηση για να σας εικονικές μηχανές για να εφαρμόσετε τις απαιτούμενες ενημερώσεις της υποδομής. Τις εικονικές μηχανές τερματίζονται ενώ θα σας κώδικα υποδομής του και, στη συνέχεια, τις εικονικές μηχανές είναι επανεκκίνηση του.

Έχετε υπόψη ότι υπάρχουν δύο τύποι συντήρησης που μπορεί να επηρεάσει τη διαθεσιμότητα των σας εικονικές μηχανές: προγραμματισμένες και μη προγραμματισμένες. Αυτή η σελίδα περιγράφει πώς το Microsoft Azure εκτελεί προγραμματισμένη συντήρηση. Για περισσότερες πληροφορίες σχετικά με τη μη προγραμματισμένη συντήρηση, ανατρέξτε στο θέμα [Κατανόηση προγραμματισμένη έναντι μη προγραμματισμένη συντήρηση](virtual-machines-linux-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

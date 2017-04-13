<properties 
    pageTitle="Αναπτύξτε ξανά Linux εικονικές μηχανές | Microsoft Azure" 
    description="Περιγράφει τον τρόπο για να αναπτύξετε εκ νέου Linux εικονικές μηχανές να συμβάλει στην αντιμετώπιση προβλημάτων σύνδεσης SSH." 
    services="virtual-machines-linux" 
    documentationCenter="virtual-machines" 
    authors="iainfoulds" 
    manager="timlt"
    tags="azure-resource-manager,top-support-issue" 
/>
    

<tags 
    ms.service="virtual-machines-linux" 
    ms.devlang="na" 
    ms.topic="support-article" 
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure" 
    ms.date="09/19/2016" 
    ms.author="iainfou" 
/>

# <a name="redeploy-virtual-machine-to-new-azure-node"></a>Αναπτύξτε ξανά εικονική μηχανή νέο κόμβο Azure

Εάν έχετε αντιμετωπίζουν δυσκολίες αντιμετώπισης προβλημάτων SSH ή εφαρμογή πρόσβαση σε μια εικονική μηχανή Azure (Εικονική), η επανάληψη ανάπτυξης η Εικονική μπορεί να σας βοηθήσουν. Όταν αναπτύξετε εκ νέου μια Εικονική, μετακινείται η Εικονική σε έναν νέο κόμβο εντός της υποδομής Azure και, στη συνέχεια, να το προσφέρει επιστρέψτε στην, διατηρώντας όλες τις επιλογές ρύθμισης παραμέτρων και τους συναφείς πόρους. Αυτό το άρθρο σας δείχνει πώς μπορείτε να αναπτύξετε εκ νέου μια Εικονική χρησιμοποιώντας Azure CLI ή την πύλη του Azure.

> [AZURE.NOTE] Αφού αναπτύξετε εκ νέου μια Εικονική, χάνεται το προσωρινό δίσκο και ενημερώνονται δυναμικές διευθύνσεις IP που σχετίζεται με το περιβάλλον εργασίας εικονικού δικτύου. 


## <a name="using-azure-cli"></a>Χρήση του Azure CLI

Βεβαιωθείτε ότι έχετε την [Πιο πρόσφατη Azure CLI εγκατεστημένο](../xplat-cli-install.md) στον υπολογιστή σας και είστε στη λειτουργία διαχείρισης πόρων (`azure config mode arm`).

Χρησιμοποιήστε την ακόλουθη εντολή Azure CLI να αναπτύξετε εκ νέου την εικονική μηχανή σας:

```bash
azure vm redeploy --resourcegroup <resourcegroup> --vm-name <vmname> 
```

Μπορείτε να δείτε την κατάσταση της αλλαγής Εικονική καθώς μεταβαίνετε κατά τη διαδικασία επανάληψη ανάπτυξης. Το `PowerState` από την εικονική Μηχανή μεταφερθείτε 'Εκτέλεση' στο 'Ενημέρωση', 'Έναρξη' και, τέλος 'Εκτέλεση' όπως μεταβαίνετε κατά τη διαδικασία επανάληψη ανάπτυξης σε έναν νέο κεντρικό υπολογιστή. Έλεγχος της κατάστασης του ΣΠΣ μέσα σε μια ομάδα πόρων με:

```bash
azure vm list -g <resourcegroup>
```


[AZURE.INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]


## <a name="next-steps"></a>Επόμενα βήματα
Αν αντιμετωπίζετε θέματα σύνδεσης με Εικονική σας, μπορείτε να βρείτε συγκεκριμένες βοήθεια στην [Αντιμετώπιση προβλημάτων SSH συνδέσεις](virtual-machines-linux-troubleshoot-ssh-connection.md) ή [λεπτομερείς SSH βήματα αντιμετώπισης προβλημάτων](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md). Εάν δεν μπορείτε να αποκτήσετε πρόσβαση σε μια εφαρμογή που εκτελείται σε Εικονική σας, μπορείτε επίσης να διαβάσετε [Αντιμετώπιση προβλημάτων εφαρμογής](virtual-machines-linux-troubleshoot-app-connection.md).
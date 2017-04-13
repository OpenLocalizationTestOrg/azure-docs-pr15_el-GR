<properties
   pageTitle="Πώς μπορείτε να αλλάξετε το μέγεθος μια Εικονική Linux | Microsoft Azure"
   description="Πώς μπορείτε να κλιμακωθεί προς τα επάνω ή την κλίμακα προς τα κάτω έναν εικονικό υπολογιστή Linux, αλλάζοντας το μέγεθος Εικονική."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Πώς μπορείτε να αλλάξετε το μέγεθος μια Εικονική Linux

## <a name="overview"></a>Επισκόπηση 

Μετά την προμήθεια μια εικονική μηχανή (Εικονική), να κλίμακα η Εικονική προς τα επάνω ή προς τα κάτω, αλλάζοντας το [μέγεθος Εικονική][vm-sizes]. Σε ορισμένες περιπτώσεις, πρέπει να πρώτα να καταργήσετε την εικονική Μηχανή. Αυτό μπορεί να συμβεί εάν το νέο μέγεθος δεν είναι διαθέσιμη στο σύμπλεγμα υλικό που φιλοξενεί την εικονική Μηχανή.

Αυτό το άρθρο περιγράφει τον τρόπο αλλαγής μεγέθους μια Εικονική Linux χρησιμοποιώντας το [Azure CLI][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.


## <a name="resize-a-linux-vm"></a>Αλλαγή μεγέθους μια Εικονική Linux 

Για να αλλάξετε το μέγεθος μια Εικονική, ακολουθήστε τα παρακάτω βήματα.

1. Εκτελέστε την ακόλουθη εντολή CLI. Αυτή η εντολή παραθέτει τα μεγέθη Εικονική που είναι διαθέσιμες στο σύμπλεγμα υλικού όπου φιλοξενείται η Εικονική.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Εάν εμφανίζεται το επιθυμητό μέγεθος, εκτελέστε την ακόλουθη εντολή για να αλλάξετε το μέγεθος του Εικονική.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Η Εικονική θα γίνει επανεκκίνηση κατά τη διάρκεια αυτής της διαδικασίας. Μετά την επανεκκίνηση, θα να αντιστοιχίζονται το υπάρχον λειτουργικό σύστημα και δίσκων δεδομένων. Όλα τα στοιχεία στον προσωρινό δίσκο θα χαθούν.

    Χρησιμοποιήστε το `--enable-boot-diagnostics` την επιλογή σας δίνει τη δυνατότητα [εκκίνησης Διαγνωστικά][boot-diagnostics], για να συνδεθείτε τυχόν σφάλματα που σχετίζονται με εκκίνησης.

3. Διαφορετικά, εάν το μέγεθος που θέλετε δεν εμφανίζεται, εκτελέστε τις ακόλουθες εντολές για να καταργήσετε την εκχώρηση του Εικονική αλλάξετε το μέγεθός του και, στη συνέχεια, επανεκκινήστε την εικονική Μηχανή.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Επίσης κατάργηση εκχώρησης η Εικονική για ενημερωμένες εκδόσεις οποιαδήποτε δυναμικές διευθύνσεις IP που έχουν εκχωρηθεί σε η Εικονική. Το δίσκων λειτουργικό σύστημα και τα δεδομένα δεν επηρεάζονται.
   
## <a name="next-steps"></a>Επόμενα βήματα

Για πρόσθετες κλιμάκωση, εκτελέστε πολλές παρουσίες Εικονική και διαβάθμιση. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [αυτόματης κλιμάκωσης Linux μηχανές σε ένα σύνολο κλίμακα εικονική μηχανή][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md
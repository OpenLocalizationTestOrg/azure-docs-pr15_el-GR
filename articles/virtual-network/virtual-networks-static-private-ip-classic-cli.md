<properties 
   pageTitle="Πώς μπορείτε να ορίσετε μια στατική ιδιωτικό IP σε κλασική λειτουργία ausing το CLI | Microsoft Azure"
   description="Κατανόηση των στατική ιδιωτικό διευθύνσεις IP (πτώσεις) και πώς μπορείτε να διαχειριστείτε τους σε κλασική λειτουργία χρησιμοποιώντας το CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Πώς μπορείτε να ορίσετε μια στατική διεύθυνση IP ιδιωτικό (κλασική) στο Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [διαχειριστείτε μια στατική διεύθυνση IP ιδιωτικό στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-networks-static-private-ip-arm-cli.md).

Οι εντολές Azure CLI δείγμα παρακάτω θεωρείτε ότι ένα απλό περιβάλλον που έχετε ήδη δημιουργήσει. Εάν θέλετε να εκτελέσετε τις εντολές που εμφανίζονται σε αυτό το έγγραφο, δημιουργήστε πρώτα το περιβάλλον δοκιμής περιγράφεται στη [Δημιουργία ενός vnet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Πώς μπορείτε να καθορίσετε μια στατική διεύθυνση IP ιδιωτικά, κατά τη δημιουργία μια εικονική Μηχανή
Για να δημιουργήσετε μια νέα Εικονική με το όνομα *DNS01* σε μια νέα υπηρεσία στο cloud με το όνομα *TestService* με βάση το παραπάνω σενάριο, ακολουθήστε τα παρακάτω βήματα:

1. Εάν δεν έχετε χρησιμοποιήσει ποτέ Azure CLI, ανατρέξτε στο θέμα [εγκατάσταση και ρύθμιση παραμέτρων του Azure CLI](../xplat-cli-install.md) και ακολουθήστε τις οδηγίες μέχρι το σημείο όπου μπορείτε να επιλέξετε το Azure λογαριασμό και τη συνδρομή.
1. Εκτελέστε την εντολή **Δημιουργία azure υπηρεσίας** για να δημιουργήσετε την υπηρεσία cloud.

        azure service create TestService --location uscentral

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. Εκτελέστε την εντολή **azure δημιουργία εικονική** για να δημιουργήσετε την εικονική Μηχανή. Σημειώστε την τιμή για μια στατική διεύθυνση IP ιδιωτικό. Στη λίστα που εμφανίζεται μετά την έξοδο εξηγεί τις παραμέτρους που χρησιμοποιούνται.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Αναμενόμενο αποτέλεσμα:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (ή--θέση)**. Azure περιοχής όπου θα δημιουργηθεί η Εικονική. Για το σενάριο, *centralus*.
    - **-n (ή όνομα--εικονική)**. Όνομα της η Εικονική θα δημιουργηθεί.
    - **-w (ή--όνομα εικονικού δικτύου)**. Όνομα το VNet όπου θα δημιουργηθεί η Εικονική. 
    - **-S (ή--στατική ip)**. Στατική ιδιωτική διεύθυνση IP για την εικονική Μηχανή.
    - **TestService**. Το όνομα της υπηρεσίας cloud όπου θα δημιουργηθεί η Εικονική.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Εικόνα που θα χρησιμοποιηθεί για να δημιουργήσετε την εικονική Μηχανή.
    - **adminuser**. Τοπικού διαχειριστή για το Windows Εικονική.
    - **AdminP@ssw0rd**. Κωδικός πρόσβασης τοπικού διαχειριστή για το Windows Εικονική.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Πώς μπορείτε να ανακτήσετε ιδιωτικό πληροφορίες στατικής διεύθυνσης IP για μια Εικονική
Για να προβάλετε τα ιδιωτικά πληροφορίες στατικής διεύθυνσης IP για την εικονική Μηχανή που δημιουργήθηκαν με τη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή Azure CLI και παρατηρήστε την τιμή για το *Δίκτυο StaticIP*:

    azure vm static-ip show DNS01

Αναμενόμενο αποτέλεσμα:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Πώς να καταργήσετε μια στατική διεύθυνση IP ιδιωτικό από μια εικονική Μηχανή
Για να καταργήσετε τη στατική διεύθυνση IP ιδιωτικό προστεθεί η Εικονική στη δέσμη ενεργειών παραπάνω, εκτελέστε την ακόλουθη εντολή Azure CLI:
    
    azure vm static-ip remove DNS01

Αναμενόμενο αποτέλεσμα:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Πώς να προσθέσετε μια στατική ιδιωτικό IP σε υπάρχουσα Εικονική μηχανή
Για να προσθέσετε μια στατική ιδιωτική διεύθυνση IP για την εικονική Μηχανή που δημιουργήθηκε με τη δέσμη ενεργειών παραπάνω, σφάλμα χρόνου εκτέλεσης ακόλουθο εντολή:

    azure vm static-ip set DNS01 192.168.1.101

Αναμενόμενο αποτέλεσμα:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε σχετικά με τις διευθύνσεις [δεσμευμένη δημόσια διεύθυνση IP](virtual-networks-reserved-public-ip.md) .
- Μάθετε σχετικά με τις διευθύνσεις [επιπέδου παρουσίας δημόσια IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Συμβουλευθείτε τα [APIs ΥΠΌΛΟΙΠΑ δεσμευμένη διεύθυνση IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).

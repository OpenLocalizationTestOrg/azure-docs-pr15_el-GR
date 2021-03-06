<properties
    pageTitle="Καταγράψτε μια εικόνα από μια Εικονική Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να καταγράψετε μια εικόνα από μια βάσει Linux Azure εικονική μηχανή (Εικονική) που δημιουργήθηκαν με το μοντέλο κλασική ανάπτυξης."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Πώς μπορείτε να καταγράψετε μια κλασική Linux εικονική μηχανή ως εικόνα

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](virtual-machines-linux-capture-image.md).

Αυτό το άρθρο παρουσιάζει τον τρόπο για να καταγράψετε μια κλασική Azure εικονική μηχανή εκτελείται Linux ως μια εικόνα για να δημιουργήσετε άλλες εικονικές μηχανές. Αυτή η εικόνα περιλαμβάνει το λειτουργικό σύστημα δίσκου και δίσκων δεδομένων που έχουν επισυναφθεί σε η εικονική μηχανή. Δεν περιλαμβάνει τις παραμέτρους του δικτύου, ώστε να πρέπει να ρυθμίσετε τις παραμέτρους που όταν δημιουργείτε τις άλλες εικονικές μηχανές από την εικόνα.

Azure αποθηκεύει την εικόνα στην περιοχή **εικόνες**, μαζί με τις εικόνες που έχετε αποστείλει. Για περισσότερες πληροφορίες σχετικά με τις εικόνες, ανατρέξτε στο θέμα [Σχετικά με τις εικόνες εικονική μηχανή στο Azure] [].

## <a name="before-you-begin"></a>Πριν ξεκινήσετε

Τα παρακάτω βήματα λαμβάνεται ως δεδομένο ότι έχετε ήδη δημιουργήσει ένα Azure εικονική μηχανή χρησιμοποιώντας το μοντέλο ανάπτυξης κλασική και έχει ρυθμιστεί το λειτουργικό σύστημα, συμπεριλαμβανομένων των επισύναψη οποιαδήποτε δίσκων δεδομένων. Εάν χρειάζεστε για να δημιουργήσετε μια Εικονική, διαβάστε [πώς μπορείτε να δημιουργήσετε μια εικονική μηχανή Linux] [].


## <a name="capture-the-virtual-machine"></a>Καταγραφή η εικονική μηχανή

1. [Σύνδεση με την εικονική μηχανή](virtual-machines-linux-mac-create-ssh-keys.md) χρησιμοποιώντας ένα πρόγραμμα-πελάτη SSH της επιλογής σας.

2. Στο παράθυρο SSH, πληκτρολογήστε την ακόλουθη εντολή. Το αποτέλεσμα από `waagent` μπορεί να διαφέρει λίγο ανάλογα με την έκδοση αυτού του βοηθητικού προγράμματος:

    `sudo waagent -deprovision+user`

    Αυτή η εντολή προσπαθεί να καθαρίσετε το σύστημα και να είναι κατάλληλη για reprovisioning. Αυτή η λειτουργία εκτελεί τις ακόλουθες εργασίες:

    - Καταργεί πλήκτρα SSH κεντρικού υπολογιστή (εάν Provisioning.RegenerateSshHostKeyPair είναι 'y' στο αρχείο παραμέτρων)
    - Ρύθμιση παραμέτρων του διακομιστή ονομάτων στο /etc/resolv.conf καταργεί
    - Καταργεί το `root` κωδικού πρόσβασης χρήστη από/κ.λπ/σκιά (εάν Provisioning.DeleteRootPassword είναι το "α" στο αρχείο παραμέτρων)
    - Καταργεί στο cache χρήσεων DHCP προγράμματος-πελάτη
    - Επαναφέρει όνομα κεντρικού υπολογιστή για να localhost.localdomain
    - Διαγράφει το τελευταίο προμήθεια του φακέλου χρήστη λογαριασμό (που λαμβάνονται από /var/lib/waagent) **και σχετικών δεδομένων**.

    >[AZURE.NOTE] Deprovisioning διαγράφει αρχεία και δεδομένα σε "γενίκευση" η εικόνα. Να εκτελέσετε αυτήν την εντολή μόνο σε εικονικό μηχάνημα που σκοπεύετε να καταγράψετε ως νέο πρότυπο εικόνας. Δεν εγγυάται ότι η εικόνα είναι απενεργοποιημένο όλων των ευαίσθητων πληροφοριών ή είναι κατάλληλη για αναδιανομή σε τρίτους.


3. Πληκτρολογήστε **y** για να συνεχίσετε. Μπορείτε να προσθέσετε το `-force` παράμετρο, για να αποφύγετε αυτό το βήμα επιβεβαίωσης.

4. Πληκτρολογήστε **Έξοδος** , για να κλείσετε το πρόγραμμα-πελάτη SSH.

    >[AZURE.NOTE] Τα υπόλοιπα βήματα θεωρείται ότι έχετε ήδη [εγκαταστήσει το Azure CLI](../xplat-cli-install.md) στον υπολογιστή-πελάτη. Όλα τα ακόλουθα βήματα μπορεί να γίνει επίσης στην [πύλη του Azure κλασική] [].

5. Από τον υπολογιστή-πελάτη, ανοίξτε Azure CLI και να συνδεθείτε στη συνδρομή σας στο Azure. Για λεπτομέρειες, διαβάστε τη [σύνδεση σε μια συνδρομή του Azure από το Azure CLI](../xplat-cli-connect.md).

6. Βεβαιωθείτε ότι βρίσκεστε σε λειτουργία Διαχείριση υπηρεσίας:

    `azure config mode asm`

7. Τερματίστε την εικονική μηχανή που ήδη καταργείται στα προηγούμενα βήματα με:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Μπορείτε να βρείτε τις όλες τις εικονικές μηχανές δημιουργηθεί με τη συνδρομή σας, χρησιμοποιώντας`azure vm list`

8. Όταν η εικονική μηχανή έχει διακοπεί, καταγράψετε την εικόνα με την εντολή:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Πληκτρολογήστε το όνομα της εικόνας που θέλετε στη θέση _ονόματος νέας εικόνας_. Αυτή η εντολή δημιουργεί μια γενική εικόνα OS. Το `-t` δευτερεύουσας διαγράφει το αρχικό εικονική μηχανή.

9.  Η νέα εικόνα είναι τώρα διαθέσιμη στη λίστα των εικόνων που μπορούν να χρησιμοποιηθούν για τη ρύθμιση παραμέτρων οποιαδήποτε νέα εικονικές μηχανές. Μπορείτε να το προβάλετε με την εντολή:

    `azure vm image list`

    Στην [πύλη του Azure κλασική] [], εμφανίζεται στη λίστα **ΕΙΚΌΝΕΣ** .

    ![Επιτυχής καταγραφή εικόνας](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Επόμενα βήματα
Η εικόνα είναι έτοιμος να χρησιμοποιηθεί για τη δημιουργία εικονικές μηχανές. Μπορείτε να χρησιμοποιήσετε την εντολή Azure CLI `azure vm create` και παρέχετε το όνομα της εικόνας που δημιουργήσατε. Για λεπτομέρειες σχετικά με την εντολή, ανατρέξτε στο θέμα [χρήση του Azure CLI με κλασική μοντέλο ανάπτυξης](../virtual-machines-command-line-tools.md) . Εναλλακτικά, μπορείτε να χρησιμοποιήσετε το [Azure κλασική πύλη] [] για να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή χρησιμοποιώντας τη μέθοδο **Από τη συλλογή** και επιλέγοντας την εικόνα που δημιουργήσατε. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή] [] .

**Δείτε επίσης:** [Οδηγός Azure Linux παράγοντα χρήστη](virtual-machines-linux-agent-user-guide.md)

[Azure κλασική πύλη]: http://manage.windowsazure.com
[Πληροφορίες για τις εικόνες εικονική μηχανή στο Azure]: virtual-machines-linux-classic-about-images.md
[Πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εικονική μηχανή]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Πώς μπορείτε να δημιουργήσετε μια εικονική μηχανή Linux]: virtual-machines-linux-classic-create-custom.md

<properties 
    pageTitle="Επιλογή ονομάτων χρήστη για Linux | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να επιλέξετε ονόματα χρηστών για μια εικονική μηχανή Linux στο Azure." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Επιλογή ονομάτων χρήστη για Linux στο Azure#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Κατά την προμήθεια ένα εικονικό μηχάνημα Linux σε Azure πρέπει να καθορίσετε το όνομα του χρήστη μη ριζικό που μπορείτε να χρησιμοποιήσετε αργότερα για να συνδεθείτε με την εικονική Μηχανή. Μπορείτε να επιλέξετε το όνομα του νέου χρήστη ή εάν προμήθεια μέσω Azure κλασική πύλη, μπορείτε να αποδεχθείτε το προεπιλεγμένο όνομα "azureuser".

Στις περισσότερες περιπτώσεις αυτός ο χρήστης δεν υπάρχει στην εικόνα της βάσης και θα δημιουργηθεί κατά τη διάρκεια της διαδικασίας προετοιμασίας. Εάν ο χρήστης βρίσκεται σε τη βασική εικόνα Εικονική, στη συνέχεια, τον παράγοντα Azure Linux απλώς ρυθμίζει τις παραμέτρους του κωδικού πρόσβασης ή/και SSH κλειδί για τον συγκεκριμένο χρήστη βάσει των πληροφοριών που καθορίσατε κατά τη δημιουργία του Εικονική.

**Ωστόσο**, Linux ορίζει ένα σύνολο ονόματα χρηστών που δεν πρέπει να χρησιμοποιούνται. Η διαδικασία προετοιμασίας θα **αποτύχει** , εάν προσπαθήσετε να προμηθεύσουν μια Εικονική Linux χρησιμοποιώντας έναν υπάρχοντα χρήστη συστήματος, η οποία καθορίζεται ως χρήστης με UID 0-99. Ένα τυπικό παράδειγμα είναι το `root` χρήστη, το οποίο έχει UID 0.

 - Δείτε επίσης: [περιοχές βάσης τυπική Linux - Αναγνωριστικό χρήστη](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Ακολουθεί μια λίστα με κοινές ενσωματωμένο σύστημα χρηστών για CentOS και θα πρέπει να αποφύγετε τη χρήση κατά την προμήθεια ένα εικονικό μηχάνημα Linux σε Azure Ubuntu. Αυτή η λίστα είναι απλώς ένα παράδειγμα, ανατρέξτε στην τεκμηρίωση για τη διανομή σας για να βεβαιωθείτε ότι το όνομα χρήστη που επιλέγετε δεν βρίσκεται σε διένεξη με έναν υπάρχοντα χρήστη του συστήματος.


## <a name="centos"></a>CentOS
- abrt
- adm
- ήχου
- Ανακύκλωσης
- CDROM
- cgred
- μονού
- dbus
- εξερχόμενες κλήσεις απομακρυσμένης
- DIP
- δίσκο
- δισκέτα
- FTP
- FTP
- αγώνων
- Gopher
- haldaemon
- Διακοπή
- kmem
- Κλείδωμα
- LP
- Αλληλογραφία
- Άντρας
- μνήμης
- nfsnobody
- κανένας δεν
- NTP
- Τελεστής
- oprofile
- postdrop
- επιθεματική
- qpidd
- ρίζας
- RPC
- rpcuser
- saslauth
- Τερματισμός
- slocate
- sshd
- stapdev
- stapusr
- Συγχρονισμός
- συστήματος
- ταινίας
- δοκιμή
- tcpdump
- TTY
- Οι χρήστες
- utempter
- utmp
- uucp
- vcsa
- βίντεο
- τροχού


## <a name="ubuntu"></a>Ubuntu
- adm
- διαχειριστής
- ήχου
- Δημιουργία αντιγράφων ασφαλείας
- Ανακύκλωσης
- CDROM
- crontab
- μονού
- εξερχόμενες κλήσεις απομακρυσμένης
- DIP
- δίσκο
- φαξ
- δισκέτα
- ασφάλεια
- αγώνων
- gnats
- IRC
- kmem
- Οριζόντιος
- libuuid
- λίστα
- LP
- Αλληλογραφία
- Άντρας
- messagebus
- mlocate
- netdev
- συζητήσεων
- κανένας δεν
- καμία ομάδα
- Τελεστής
- plugdev
- διακομιστής μεσολάβησης
- ρίζας
- SASL
- σκιά
- src
- SSH
- sshd
- διοικητικό προσωπικό
- sudo
- Συγχρονισμός
- συστήματος
- SYSLOG
- ταινίας
- TTY
- Οι χρήστες
- utmp
- uucp
- βίντεο
- φωνής
- whoopsie
- δεδομένα www

 
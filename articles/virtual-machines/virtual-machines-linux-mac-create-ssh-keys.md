<properties
    pageTitle="Δημιουργία SSH πλήκτρα σε Linux και Mac | Microsoft Azure"
    description="Δημιουργία και χρήση SSH πλήκτρα στο Linux και Mac για τη διαχείριση πόρων και μοντέλα κλασική ανάπτυξης Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Δημιουργία πλήκτρα SSH Linux και Mac για ΣΠΣ Linux στο Azure

Με μια κλειδιών SSH μπορείτε να δημιουργήσετε εικονικές μηχανές σε Azure που από προεπιλογή χρήση των πλήκτρων SSH για τον έλεγχο ταυτότητας, καταργώντας την ανάγκη για τους κωδικούς πρόσβασης για να συνδεθείτε.  Κωδικοί πρόσβασης μπορούν να μαντέψει και ανοίξτε σας ΣΠΣ έως αμείλικτης συμμαχίας επίθεση επιβολής προσπαθεί να προβλέψει τον κωδικό πρόσβασής σας. ΣΠΣ που δημιουργήθηκαν με τα πρότυπα Azure ή το `azure-cli` μπορούν να περιλαμβάνουν το δημόσιο κλειδί SSH ως μέρος της ανάπτυξης, καταργώντας μια ρύθμιση παραμέτρων ανάπτυξης δημοσίευση.  Εάν συνδέεστε με μια Εικονική Linux από το Windows, ανατρέξτε στο θέμα [αυτού του εγγράφου.](virtual-machines-linux-ssh-from-windows.md)

Το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).

- έχετε συνδεθεί με [Azure CLI](../xplat-cli-install.md)`azure login`

- στη λειτουργία διαχείρισης πόρων Azure _πρέπει να είναι σε_ Azure CLI`azure config mode arm`

## <a name="quick-commands"></a>Γρήγορες εντολές

Στις ακόλουθες εντολές, αντικαταστήστε τα παραδείγματα με τις δικές σας επιλογές.

SSH κλειδιά είναι από προεπιλογή ενήμεροι το `.ssh` καταλόγου.  

```bash
cd ~/.ssh/
```

Εάν δεν έχετε ένα `~/.ssh` καταλόγου το `ssh-keygen` εντολή θα δημιουργήσει για εσάς με τα κατάλληλα δικαιώματα.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Πληκτρολογήστε το όνομα του αρχείου που έχει αποθηκευτεί σε το `~/.ssh/` καταλόγου:

```bash
id_rsa
```

Εισαγάγετε φράση πρόσβασης για id_rsa:

```bash
correct horse battery staple
```

Δεν υπάρχει πλέον ένα `id_rsa` και `id_rsa.pub` SSH ζεύγος κλειδιών στο το `~/.ssh` καταλόγου.

```bash
ls -al ~/.ssh
```

Προσθήκη του κλειδιού που έχουν δημιουργηθεί πρόσφατα για να `ssh-agent` Linux και Mac (προστίθεται επίσης αλυσίδας κλειδιών OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Αντιγράψτε το δημόσιο κλειδί SSH στο διακομιστή Linux:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Ελέγξτε τη σύνδεση χρήση αριθμών-κλειδιών αντί για έναν κωδικό πρόσβασης:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση

Χρήση SSH δημόσια και ιδιωτικά κλειδιά είναι ο ευκολότερος τρόπος για να συνδεθείτε διακομιστές σας Linux. [Δημόσιο κλειδί κρυπτογράφησης](https://en.wikipedia.org/wiki/Public-key_cryptography) παρέχει μια πολύ πιο ασφαλής τρόπος για να συνδεθείτε με το Linux ή Εικονική BSD στα Azure από τους κωδικούς πρόσβασης, η οποία μπορεί να είναι υποχρεωτική βίαιες ενέργειες πολύ πιο εύκολα. Το δημόσιο κλειδί μπορεί να είναι κοινόχρηστη με οποιονδήποτε; αλλά μόνο εσείς (ή την υποδομή σας τοπικής ασφάλειας) διαθέτουν σας ιδιωτικό κλειδί.  Το ιδιωτικό κλειδί SSH θα πρέπει να έχετε ένα [πολύ ασφαλούς κωδικού πρόσβασης](https://www.xkcd.com/936/) (Προέλευση:[xkcd.com](https://xkcd.com)) για την προστασία του.  Αυτός ο κωδικός πρόσβασης είναι απλώς να αποκτήσετε πρόσβαση σε ιδιωτικό κλειδί SSH και **δεν είναι** ο κωδικός πρόσβασης λογαριασμού χρήστη.  Όταν προσθέτετε έναν κωδικό πρόσβασης για να τον αριθμό-κλειδί SSH, κρυπτογραφεί το ιδιωτικό κλειδί, έτσι ώστε το ιδιωτικό κλειδί είναι άχρηστο χωρίς τον κωδικό πρόσβασης για να την ξεκλειδώσετε.  Εάν ένας εισβολέας απάτη σας ιδιωτικό κλειδί και το κλειδί αυτό δεν είχε έναν κωδικό πρόσβασης, θα ήταν μπορούν να χρησιμοποιήσουν αυτό το ιδιωτικό κλειδί για να συνδεθείτε όλους τους διακομιστές που έχουν το αντίστοιχο δημόσιο κλειδί.  Εάν ένα ιδιωτικό κλειδί είναι το προστατεύεται με κωδικό πρόσβασης δεν μπορούν να χρησιμοποιηθούν με συγκεκριμένο εισβολέας, παρέχοντας ένα επιπλέον επίπεδο ασφάλειας για την υποδομή στην Azure.

Σε αυτό το άρθρο δημιουργεί κλειδιού αρχεία *ssh rsa* μορφοποιημένο, τα οποία συνιστώνται για αναπτύξεις στη Διαχείριση πόρων.  πλήκτρα *SSH rsa* απαιτούνται στην [πύλη](https://portal.azure.com) για αναπτύξεις κλασική και διαχείριση πόρων.

## <a name="create-the-ssh-keys"></a>Δημιουργία των κλειδιών SSH

Azure απαιτεί τουλάχιστον 2048 bit, ssh rsa μορφή δημόσια και ιδιωτικά κλειδιά. Για να δημιουργήσετε τη χρήση αριθμών-κλειδιών `ssh-keygen`, που ζητά από μια σειρά από ερωτήσεις και, στη συνέχεια, συντάσσει ένα ιδιωτικό κλειδί και ένα αντίστοιχο δημόσιο κλειδί. Όταν δημιουργείται μια Εικονική Azure, αντιγράφεται το δημόσιο κλειδί `~/.ssh/authorized_keys`.  Πλήκτρα SSH στο `~/.ssh/authorized_keys` που χρησιμοποιούνται για να αποτελούν πρόκληση για το πρόγραμμα-πελάτη, ώστε να ταιριάζει με το αντίστοιχο ιδιωτικό κλειδί σε μια σύνδεση login SSH.

## <a name="using-ssh-keygen"></a>Χρήση ssh keygen

Αυτή η εντολή δημιουργεί έναν κωδικό πρόσβασης, ασφαλές (κρυπτογραφημένα) SSH κλειδιών χρησιμοποιώντας 2048 bit RSA και αυτό είναι σχόλιο για να προσδιορίσετε εύκολα.  

Έναρξη με αλλαγή σε καταλόγους, έτσι ώστε όλα σας ssh κλειδιά δημιουργούνται με αυτόν τον κατάλογο.

```bash
cd ~/.ssh
```

Εάν δεν έχετε ένα `~/.ssh` καταλόγου το `ssh-keygen` εντολή θα δημιουργήσει για εσάς με τα κατάλληλα δικαιώματα.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Εντολή επεξήγηση_

`ssh-keygen`= το πρόγραμμα που χρησιμοποιήθηκε για τη δημιουργία των κλειδιών

`-t rsa`= τύπος του αριθμού-κλειδιού για να δημιουργήσετε ποιο είναι το [](https://en.wikipedia.org/wiki/RSA_(cryptosystem) μορφή RSA

`-b 2048`= τμήματα του αριθμού-κλειδιού

`-C "myusername@myserver"`= σχολίου προσαρτηθεί στο τέλος του αρχείου δημόσια κλειδιού για να προσδιορίσετε εύκολα.  Κανονικά ένα μήνυμα ηλεκτρονικού ταχυδρομείου που θα χρησιμοποιείται ως το σχόλιο, αλλά μπορείτε να χρησιμοποιήσετε ό, τι λειτουργεί καλύτερα για την υποδομή σας.

### <a name="using-pem-keys"></a>Χρήση των πλήκτρων PEM

Εάν χρησιμοποιείτε το κλασικό ανάπτυξη μοντέλου (κλασική πύλη Azure ή το Azure υπηρεσίας διαχείρισης CLI `asm`), ίσως χρειαστεί να χρησιμοποιήσετε PEM μορφοποιημένο SSH πλήκτρα πρόσβασης σας ΣΠΣ Linux.  Ακολουθεί ο τρόπος για να δημιουργήσετε έναν αριθμό-κλειδί PEM από ένα υπάρχον SSH δημόσιο κλειδί και μια υπάρχουσα x509 πιστοποιητικού.

Για να δημιουργήσετε ένα PEM μορφοποίηση αριθμού-κλειδιού από ένα υπάρχον δημόσιο κλειδί SSH:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Παράδειγμα ssh keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Αποθήκευση αρχείων κλειδιού:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Το όνομα ζεύγος κλειδιών για αυτό το άρθρο.  Αντιμετωπίζετε ένα ζεύγος κλειδιών που ονομάζεται **id_rsa** είναι η προεπιλογή και ορισμένα εργαλεία ίσως αναμένατε το όνομα αρχείο ιδιωτικού κλειδιού **id_rsa** , ώστε να έχετε έναν είναι καλή ιδέα. Ο κατάλογος `~/.ssh/` είναι η προεπιλεγμένη θέση για ζεύγη κλειδιών SSH και το αρχείο ρύθμισης παραμέτρων SSH.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Μια λίστα με τα `~/.ssh` καταλόγου. `ssh-keygen`δημιουργεί το `~/.ssh` καταλόγου αν δεν υπάρχει και ρυθμίζει επίσης τις σωστές λειτουργίες της κατοχής και αρχείων.

Τον κωδικό πρόσβασης κλειδιού:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`αναφέρεται σε έναν κωδικό πρόσβασης ως "μια φράση πρόσβασης."  Είναι *ιδιαίτερα* συνιστάται για να προσθέσετε έναν κωδικό πρόσβασης για να σας ζεύγη κλειδιών. Χωρίς κωδικό πρόσβασης προστασία το ζεύγος κλειδιών, οποιοσδήποτε με το αρχείο ιδιωτικού κλειδιού να το χρησιμοποιήσετε για να συνδεθείτε οποιονδήποτε διακομιστή που έχει το αντίστοιχο δημόσιο κλειδί. Προσθήκη κωδικού πρόσβασης προσφέρει περισσότερες προστασία σε περίπτωση που κάποιος έχει τη δυνατότητα να αποκτήσει πρόσβαση στο αρχείο ιδιωτικό κλειδί, δώσει χρόνο για να αλλάξετε τα πλήκτρα που χρησιμοποιούνται για τον έλεγχο ταυτότητας που.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Χρήση ssh παράγοντας για την αποθήκευση κωδικού πρόσβασης ιδιωτικό κλειδί

Για να αποφύγετε να πληκτρολογείτε τον κωδικό πρόσβασης αρχείο ιδιωτικού κλειδιού με κάθε login SSH, μπορείτε να χρησιμοποιήσετε `ssh-agent` στη μνήμη cache τον κωδικό πρόσβασής σας αρχείο ιδιωτικού κλειδιού. Εάν χρησιμοποιείτε υπολογιστή Mac, η αλυσίδα κλειδιών OSX αποθηκεύει με ασφάλεια τους κωδικούς πρόσβασης ιδιωτικό κλειδί όταν καλείτε `ssh-agent`.

Πρώτα βεβαιωθείτε ότι `ssh-agent` εκτελείται

```bash
eval "$(ssh-agent -s)"
```

Προσθέστε τώρα το ιδιωτικό κλειδί για να `ssh-agent` χρησιμοποιώντας την εντολή `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Το ιδιωτικό τον κωδικό πρόσβασης κλειδιού τώρα είναι αποθηκευμένη στο `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Δημιουργία και ρύθμιση παραμέτρων ενός αρχείου ρύθμισης παραμέτρων SSH

Μια προτεινόμενη βέλτιστη πρακτική να δημιουργήσετε και να ρυθμίσετε τις παραμέτρους μιας `~/.ssh/config` αρχείο για να επιταχύνετε συνδεθείτε πρόσθετα και για τη βελτιστοποίηση τη συμπεριφορά του προγράμματος-πελάτη SSH.

Το παρακάτω παράδειγμα εμφανίζει μια τυπική ρύθμιση παραμέτρων.

### <a name="create-the-file"></a>Δημιουργία του αρχείου

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Επεξεργαστείτε το αρχείο για να προσθέσετε τη νέα ρύθμιση παραμέτρων SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Παράδειγμα `~/.ssh/config` αρχείο:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Αυτό config SSH παρέχει ενότητες για κάθε διακομιστή για να ενεργοποιήσετε την κάθε να έχει το δικό της αποκλειστικό ζεύγος κλειδιών. Οι προεπιλεγμένες ρυθμίσεις (`Host *`) είναι για οποιαδήποτε κεντρικούς υπολογιστές που δεν ταιριάζουν με το συγκεκριμένο κεντρικών υπολογιστών υψηλότερη θέση στο αρχείο παραμέτρων.


### <a name="config-file-explained"></a>Αρχείο ρύθμισης παραμέτρων επεξήγηση

`Host`= το όνομα του κεντρικού υπολογιστή καλείτε με το terminal.  `ssh fedora22`ενημερώνει `SSH` να χρησιμοποιήσετε τις τιμές στο μπλοκ ρυθμίσεις με την ετικέτα `Host fedora22` ΣΗΜΕΊΩΣΗ: Αυτό μπορεί να είναι οποιαδήποτε ετικέτα που είναι λογική για τη χρήση και δεν αντιπροσωπεύει το πραγματικό όνομα κεντρικού υπολογιστή από οποιονδήποτε διακομιστή.

`Hostname 102.160.203.241`= η διεύθυνση IP ή όνομα DNS για το διακομιστή την πρόσβαση.

`User myusername`= ο απομακρυσμένος λογαριασμός χρήστη για χρήση κατά τη σύνδεση στο διακομιστή.

`PubKeyAuthentication yes`= ενημερώνει SSH που θέλετε να χρησιμοποιήσετε ένα κλειδί SSH για να συνδεθείτε.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= η SSH ιδιωτικό κλειδί και το αντίστοιχο δημόσιο κλειδί για να χρησιμοποιήσετε για τον έλεγχο ταυτότητας.


## <a name="ssh-into-linux-without-a-password"></a>SSH σε Linux χωρίς κωδικό πρόσβασης

Τώρα που έχετε ένα ζεύγος κλειδιών SSH και έχει ρυθμιστεί αρχείου ρύθμισης παραμέτρων SSH, είστε σε θέση να συνδεθείτε με το Εικονική Linux γρήγορα και με ασφάλεια. Την πρώτη φορά που θα συνδεθείτε σε ένα διακομιστή χρησιμοποιώντας ένα πλήκτρο SSH τις οδηγίες εντολή που για τη φράση πρόσβασης για το συγκεκριμένο αρχείο κλειδιού.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Εντολή επεξήγηση

Όταν `ssh fedora22` εκτελείται SSH πρώτα εντοπίζει και φορτώνει τις ρυθμίσεις από την `Host fedora22` μπλοκ και, στη συνέχεια, φορτία όλες τις υπόλοιπες ρυθμίσεις από την τελευταία μπλοκ, `Host *`.

## <a name="next-steps"></a>Επόμενα βήματα

Επόμενη up είναι να δημιουργήσετε ΣΠΣ Linux Azure χρησιμοποιώντας το νέο κλειδί δημόσια SSH.  Azure ΣΠΣ που έχουν δημιουργηθεί με ένα δημόσιο κλειδί SSH ως login είναι πιο ασφαλείς συγκριτικά με ΣΠΣ που δημιουργήθηκαν με την προεπιλεγμένη μέθοδο σύνδεσης, κωδικούς πρόσβασης.  Azure ΣΠΣ που δημιουργήθηκε με τη χρήση κλειδιών SSH είναι από προεπιλογή έχουν ρυθμιστεί με τους κωδικούς πρόσβασης απενεργοποιημένη, αποφυγή υποχρεωτική βίαιες ενέργειες κάποιο προσπάθειες.

- [Δημιουργία ενός ασφαλούς Εικονική Linux χρησιμοποιώντας ένα πρότυπο Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Δημιουργία ενός ασφαλούς Εικονική Linux με την πύλη Azure](virtual-machines-linux-quick-create-portal.md)
- [Δημιουργία ενός ασφαλούς Εικονική Linux χρησιμοποιώντας το CLI Azure](virtual-machines-linux-quick-create-cli.md)
<properties
    pageTitle="Απενεργοποίηση SSH τους κωδικούς πρόσβασης σε σας Εικονική Linux με τη ρύθμιση των παραμέτρων SSHD | Microsoft Azure"
    description="Ασφαλής σας Εικονική Linux στην Azure απενεργοποιώντας συνδέσεις τον κωδικό πρόσβασης για SSH."
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Απενεργοποίηση SSH τους κωδικούς πρόσβασης σε σας Εικονική Linux με τη ρύθμιση των παραμέτρων SSHD

Σε αυτό το άρθρο εστιάζει σχετικά με τον τρόπο για να κλειδώσετε της ασφάλειας σύνδεσής σας Εικονική Linux.  Μόλις ανοίξει τη θύρα SSH 22 στην αρχή τα BOT κόσμο που προσπαθείτε να συνδεθείτε με υποθέσεις τους κωδικούς πρόσβασης.  Τι θα κάνουμε σε αυτό το άρθρο είναι να απενεργοποιήσετε τον κωδικό πρόσβασης συνδέσεις μέσω SSH.  Καταργώντας πλήρως τη δυνατότητα να χρησιμοποιήσετε τους κωδικούς πρόσβασης προστατεύει η Εικονική Linux από αυτόν τον τύπο επίθεση επιβολής.  Το επιπλέον δώρο είναι θα σας θα ρυθμίσετε τις παραμέτρους SSHD Linux ώστε να επιτρέπει μόνο συνδέσεις μέσω SSH δημόσια και ιδιωτικά κλειδιά, με άκρο ο πιο ασφαλής τρόπος για να συνδεθείτε στο Linux.  Των πιθανών συνδυασμών της που θα χρειαζόταν να προβλέψει το ιδιωτικό κλειδί είναι πολλούς και, επομένως, αποθαρρύνει τα BOT από bothering ακόμα και να δοκιμάσετε με τα πλήκτρα SSH επίθεση επιβολής.


## <a name="goals"></a>Στόχοι

- Ρύθμιση παραμέτρων SSHD για να απενεργοποιήσετε:
  - Συνδέσεις κωδικού πρόσβασης
  - Σύνδεση χρήστη ρίζας
  - Έλεγχος ταυτότητας πρόκλησης-απόκρισης
- Ρύθμιση παραμέτρων SSHD για να επιτρέψετε σε:
  - μόνο SSH κλειδιού συνδέσεις
- Επανεκκινήστε το SSHD ενώ είστε ακόμα συνδεδεμένοι
- Δοκιμάστε τη νέα ρύθμιση παραμέτρων SSHD

## <a name="introduction"></a>Εισαγωγή

[SSH που ορίζονται από το](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD είναι SSH διακομιστή που εκτελείται σε η Εικονική Linux.  SSH είναι ένα πρόγραμμα-πελάτη που εκτελείται από έναν εξωτερικό περίβλημα στον σας σταθμούς εργασίας MacBook ή Linux.  SSH είναι επίσης το πρωτόκολλο που χρησιμοποιείται για την ασφάλεια και κρυπτογράφηση επικοινωνία μεταξύ του σταθμούς εργασίας και η Εικονική Linux.

Για αυτό το άρθρο είναι πολύ σημαντικό για να διατηρήσετε μία σύνδεση για να σας Εικονική Linux Άνοιγμα για ολόκληρο το ματιά.  Για αυτόν το λόγο θα σας θα ανοίξει δύο σταθμούς και SSH η Εικονική Linux από και τα δύο.  Θα χρησιμοποιήσουμε το πρώτο terminal για να κάνετε τις αλλαγές σε αρχείο ρύθμισης παραμέτρων SSHDs και επανεκκινήστε την υπηρεσία SSHD.  Θα χρησιμοποιήσουμε το δεύτερο terminal για να ελέγξετε αυτές τις αλλαγές όταν γίνει επανεκκίνηση της υπηρεσίας.  Επειδή το απενεργοποιείτε SSH κωδικών πρόσβασης και βασίζεστε αυστηρά στο πλήκτρα SSH, εάν σας SSH κλειδιά δεν είναι σωστή και να κλείσετε τη σύνδεση με την εικονική Μηχανή, η Εικονική θα οριστικά κλειδωμένο και κανένα άτομο δεν θα μπορούν να συνδεθείτε σε αυτήν που απαιτεί να διαγραφούν και εκ νέου.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

- [Δημιουργία πλήκτρα SSH Linux και Mac για ΣΠΣ Linux στο Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Λογαριασμός Azure
  - [εγγραφή στο δωρεάν δοκιμαστικής έκδοσης](https://azure.microsoft.com/pricing/free-trial/)
  - [Πύλη του Azure](http://portal.azure.com)
- Εικονική Linux εκτελείται σε azure
- SSH δημόσια και ιδιωτικά ζεύγος κλειδιών στο`~/.ssh/`
- Δημόσιο κλειδί SSH σε `~/.ssh/authorized_keys` σε η Εικονική Linux
- Sudo δικαιωμάτων στην την εικονική Μηχανή
- Θύρα ανοιχτό 22

## <a name="quick-commands"></a>Γρήγορες εντολές

_Έμπειρος διαχειριστές Linux που θέλετε την έκδοση TLDR απλώς Ξεκινήστε εδώ.  Για οποιονδήποτε άλλο θέλει η αναλυτική εξήγηση και δείτε τα παραλείψετε αυτή την ενότητα._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Λεπτομερή ματιά

Συνδεθείτε στο η Εικονική Linux σε τερματικού 1 (Τ1).  Συνδεθείτε στο η Εικονική Linux στο terminal 2 (Τ2).

Στην Τ2, πρόκειται να επεξεργαστείτε το αρχείο ρύθμισης παραμέτρων SSHD.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Από εδώ θα σας θα επεξεργαστείτε μόνο τις ρυθμίσεις για να απενεργοποιήσετε τους κωδικούς πρόσβασης και να ενεργοποιήσετε SSH κλειδιού συνδέσεις.  Υπάρχουν πολλές ρυθμίσεις σε αυτό το αρχείο που θα πρέπει να αναζητήσετε και αλλαγή για να κάνετε Linux & SSH ως ασφαλή χρειάζεστε.

#### <a name="disable-password-logins"></a>Απενεργοποίηση συνδέσεις κωδικού πρόσβασης

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Ενεργοποίηση ελέγχου ταυτότητας δημόσιο κλειδί

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Απενεργοποίηση σύνδεσης ρίζας

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Απενεργοποίηση του ελέγχου ταυτότητας πρόκλησης-απόκρισης

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Επανεκκινήστε το SSHD

Από το κέλυφος Τ1 επαλήθευση ότι είστε ακόμα συνδεδεμένοι.  Αυτό είναι σημαντικό, ώστε να που δεν λάβετε κλειδωμένο εκτός του Εικονική εάν κλειδιά SSH σας δεν είναι σωστές, επειδή τώρα είναι απενεργοποιημένες τους κωδικούς πρόσβασης.  Εάν οποιαδήποτε ρύθμιση είναι εσφαλμένη σε σας Εικονική Linux που μπορείτε να χρησιμοποιήσετε Τ1 για να διορθώσετε sshd_config όπως θα εξακολουθούν να συνδεθείτε και SSH θα διατηρεί τη σύνδεση ενεργών κατά τη διάρκεια της υπηρεσίας SSHD επανεκκίνηση.

Από Τ2 εκτέλεση:

##### <a name="on-the-debian-family"></a>Για την οικογένεια Debian

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>Για την οικογένεια RedHat

```
username@macbook$ sudo service sshd restart
```

Τώρα είναι απενεργοποιημένες τους κωδικούς πρόσβασης στον σας Εικονική προστατεύοντάς τα από προσπάθειες σύνδεσης επίθεση επιβολής κωδικού πρόσβασης.  Με μόνο SSH πλήκτρα επιτρέπεται θα μπορείτε να συνδεθείτε πιο γρήγορα και πολύ πιο ασφαλή.

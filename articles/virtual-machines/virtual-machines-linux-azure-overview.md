 <properties
   pageTitle="Azure και Linux | Microsoft Azure"
   description="Περιγράφει τον υπολογισμό Azure χώρου αποθήκευσης και δικτύωση υπηρεσίες με Linux εικονικές μηχανές."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure και Linux

Microsoft Azure είναι μια αναπτυσσόμενη συλλογή ενσωματωμένη δημόσια στο cloud όπως ανάλυση, εικονικές μηχανές, βάσεις δεδομένων, κινητές συσκευές, δίκτυο, αποθήκευση, τις υπηρεσίες και web — ιδανική για τη φιλοξενία του λύσεις.  Microsoft Azure παρέχει μια μεταβλητού μεγέθους υπολογιστών πλατφόρμα που σας επιτρέπει να πληρώσετε μόνο για τα στοιχεία που χρησιμοποιείτε, όταν θέλετε το - χωρίς να χρειάζεται να επενδύσετε σε εσωτερικής εγκατάστασης υλικού.  Azure είναι έτοιμη, όταν είστε κλιμάκωσης σας λύσεις και ανάληψη όποιο κλίμακα που απαιτείται για να εξυπηρετήσει τις ανάγκες των πελατών σας.

Εάν είστε εξοικειωμένοι με τις διάφορες δυνατότητες του του Amazon AWS, μπορείτε να εξετάσετε το AWS Azure και στο [έγγραφο ορισμού αντιστοίχισης](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Περιοχές
Πόροι Microsoft Azure κατανέμονται σε πολλές γεωγραφικές περιοχές του κόσμου.  Μια "περιοχή" αντιπροσωπεύει πολλά κέντρα δεδομένων σε μια μεμονωμένη γεωγραφική περιοχή.  Το 1 Ιανουαρίου 2016, περιλαμβάνει: 8 Αμερικής, 2 στην Ευρώπη, 6 στο χώρες Ασίας και Ειρηνικού, 2 στην Ηπειρωτική Κίνα και 3 στην Ινδία.  Εάν θέλετε μια πλήρη λίστα των όλες τις περιοχές Azure, μας διατηρεί μια λίστα από υπάρχον και που μόλις ανακοινώθηκε περιοχές.

- [Περιοχές Azure](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Διαθεσιμότητα
Με τη σειρά για την ανάπτυξη να πληροίτε τις προϋποθέσεις για μας 99.95 συμφωνία επιπέδου εξυπηρέτησης Εικονική, πρέπει να αναπτύξετε δύο ή περισσότερες ΣΠΣ εκτελείται το φόρτο εργασίας μέσα σε μια διαθεσιμότητα ρύθμιση. Αυτό θα εξασφαλίσει σας ΣΠΣ είναι κατανέμεται πολλούς τομείς σφαλμάτων στο μας κέντρα δεδομένων, καθώς και αναπτυχθεί σε κεντρικούς υπολογιστές με windows διαφορετικό συντήρηση. Το πλήρες [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) εξηγεί τη διαθεσιμότητα εγγυημένη του Azure ως σύνολο.

## <a name="azure-virtual-machines--instances"></a>Azure εικονικές μηχανές & παρουσίες
Microsoft Azure υποστηρίζει εκτελεί έναν αριθμό δημοφιλών Linux κατανομές που παρέχεται και διατηρούνται από έναν αριθμό από τους συνεργάτες.  Θα βρείτε κατανομές όπως Enterprise καπέλο κόκκινο, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD και άλλα στο το Azure Marketplace. Ενεργά εργασίας μας με διάφορες Κοινότητες Linux για να προσθέσετε ακόμα περισσότερες μπορεί να είναι στη λίστα [Azure θεωρηθεί Linux Distros](virtual-machines-linux-endorsed-distros.md) .

Εάν δεν υπάρχει αυτήν τη στιγμή στη συλλογή σας προτιμώμενη distro Linux της επιλογής, μπορείτε να "μεταφέρετε το δικό σας Linux" Εικονική με [τη δημιουργία και αποστολή ενός VHD Linux στο Azure](virtual-machines-linux-create-upload-generic.md).

Azure εικονικές μηχανές σας επιτρέπουν να αναπτύξετε ένα ευρύ φάσμα υπολογιστική λύσεις με ευέλικτη τρόπο. Μπορείτε να αναπτύξετε σχεδόν οποιαδήποτε φόρτο εργασίας και οποιαδήποτε γλώσσα σε σχεδόν οποιοδήποτε λειτουργικό σύστημα - Windows, Linux, ή μια προσαρμοσμένη δημιουργήσει κάποια από οποιαδήποτε από την αναπτυσσόμενη λίστα με συνεργάτες. Εξακολουθείτε να μην βλέπετε αυτό που αναζητάτε;  Μην ανησυχείτε - μπορείτε, επίσης, να μεταφέρετε τις δικές σας εικόνες από εσωτερικής εγκατάστασης.

## <a name="vm-sizes"></a>Εικονική μεγέθη
Όταν αναπτύξετε μια Εικονική στα Azure, που πρόκειται να επιλέξτε ένα μέγεθος Εικονική μέσα σε μία από μας σειρά μεγέθη που είναι κατάλληλη για το φόρτο εργασίας. Το μέγεθος επηρεάζει επίσης η δυναμικότητα ενέργειας, μνήμης και χώρου αποθήκευσης επεξεργασίας των η εικονική μηχανή. Χρέωσης με βάση το χρονικό διάστημα, η Εικονική εκτελείται και κατανάλωση τα εκχωρημένων πόρων. Μια πλήρη λίστα των [μεγέθη των εικονικές μηχανές](virtual-machines-linux-sizes.md).

Εδώ θα βρείτε ορισμένες βασικές οδηγίες για την επιλογή μια Εικονική μέγεθος από τη σειρά (A, D, DS, G και GS).

* Μια σειρά ΣΠΣ είναι μας τιμή σε τιμές γενικό ΣΠΣ σενάρια ανάπτυξης/έλεγχο και light φόρτους εργασίας. Αυτές είναι ευρέως διαθέσιμη σε όλες τις περιοχές και να συνδεθείτε και να χρησιμοποιήσετε διαθέσιμη σε εικονικές μηχανές τυπική πόρους.
* Μια σειρά μεγέθη (A8 - A11) είναι ειδική υπολογισμού εντατική ρυθμίσεις παραμέτρων κατάλληλη για εφαρμογές σύμπλεγμα υπολογιστών υψηλών επιδόσεων.
* Σειρά D ΣΠΣ έχουν σχεδιαστεί για να εκτελέσετε εφαρμογές που απαιτούν μεγαλύτερη power υπολογισμού και επιδόσεων προσωρινό δίσκο. D σειρά ΣΠΣ παρέχουν ταχύτερους επεξεργαστές, αναλογία υψηλότερη μνήμης-πυρήνα και μια μονάδα δίσκου οπτικοί (SSD) για τον προσωρινό δίσκο.
* Dv2-σειρά, είναι η πιο πρόσφατη έκδοση του μας σειράς D, δυνατότητες μια πιο ισχυρή CPU. Της CPU Dv2 σειρά είναι περίπου 35% ταχύτερα από το CPU D σειρά. Βασίζεται σε την τελευταία γενιά 2,4 GHz Intel Xeon® E5-2673 v3 Επεξεργαστής (Haskell), και με το 2.0 τεχνολογία Intel των ενίσχυση, να μεταβείτε προς τα επάνω στο 3.2 GHz. Η σειρά Dv2 έχει τις ίδιες ρυθμίσεις παραμέτρων μνήμης και δίσκου ως η σειρά D.
* Σειρά G ΣΠΣ προσφέρουν την περισσότερη μνήμη και να εκτελέσετε σε κεντρικούς υπολογιστές οι οποίοι έχουν οικογένειας επεξεργαστές Intel Xeon E5 V3.

Σημείωση: Σειράς DS και ΣΠΣ GS σειρά έχουν πρόσβαση σε κορυφαίες χώρος αποθήκευσης - μας SSD αντίγραφα υψηλών επιδόσεων, χαμηλής αδράνειας χώρου αποθήκευσης για εντατική φόρτους εργασίας εισόδου/εξόδου. Premium χώρου αποθήκευσης είναι διαθέσιμη σε ορισμένες περιοχές. Για λεπτομέρειες, ανατρέξτε στα θέματα:

- [Χώρος αποθήκευσης Premium: Χώρος αποθήκευσης υψηλών επιδόσεων για φόρτους εργασίας του Azure εικονική μηχανή](../storage/storage-premium-storage.md)

## <a name="automation"></a>Αυτοματοποίηση
Για να επιτύχετε proper τοπικές ρυθμίσεις DevOps, όλα υποδομή πρέπει να είναι κώδικα.  Όταν όλα της υποδομής βρίσκονται στον κώδικα που μπορεί να είναι εύκολα ξανά (Θήβα διακομιστές).  Azure λειτουργεί με όλα τα κύρια αυτοματισμού tooling όπως Ansible Chef, SaltStack και Puppet.  Azure διαθέτει επίσης το δικό του tooling για την αυτοματοποίηση:

- [Πρότυπα του Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Υποστήριξη για [την προετοιμασία cloud](http://cloud-init.io/) Azure παράδοση σε περισσότερες Linux Distros που υποστηρίζουν.  Αυτήν τη στιγμή της Canonical Ubuntu ΣΠΣ αναπτύσσονται με cloud την προετοιμασία ενεργοποιημένη από προεπιλογή.  RedHats RHEL, CentOS και Fedora υποστηρίζουν την προετοιμασία cloud, ωστόσο οι εικόνες Azure διατηρούνται από RedHat δεν έχουν εγκατεστημένη την cloud προετοιμασία.  Για να χρησιμοποιήσετε την προετοιμασία cloud σε μια οικογένεια RedHat λειτουργικό σύστημα, πρέπει να δημιουργήσετε μια προσαρμοσμένη εικόνα με cloud την προετοιμασία εγκατεστημένο.

- [Χρήση του cloud την προετοιμασία σε ΣΠΣ Linux Azure](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Ορίων
Κάθε συνδρομή Azure έχει προεπιλεγμένη ορίων στη θέση που ενδέχεται να επηρεάσουν την ανάπτυξη του μεγάλου αριθμού ΣΠΣ για το έργο σας. Το τρέχον όριο σε βάση ανά συνδρομή είναι 20 ΣΠΣ ανά περιοχή.  Όρια του μπορεί να είναι προκύπτουν από αρχειοθέτηση δελτίου υποστήριξη αίτηση μια αύξηση όριο.  Για περισσότερες λεπτομέρειες σχετικά με τα όρια:

- [Τα όρια Azure συνδρομητική υπηρεσία](../azure-subscription-service-limits.md)


## <a name="partners"></a>Συνεργάτες

Η Microsoft συνεργάζεται στενά με τους συνεργάτες μας για να βεβαιωθείτε ότι οι εικόνες που είναι διαθέσιμες είναι ενημερωθεί και βελτιστοποιημένη για ένα περιβάλλον εκτέλεσης Azure.  Για περισσότερες πληροφορίες σχετικά με τους συνεργάτες μας Ελέγξτε τους παρακάτω marketplace σελίδες.

- [Linux στην κατανομές θεωρηθεί Azure](virtual-machines-linux-endorsed-distros.md)

Redhat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Κανονική - [Azure Marketplace - Ubuntu διακομιστή 16.04 Αποτελεσμάτων](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - 8 Debian "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (σταθερό)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Bitnami βιβλιοθήκη για Azure](https://azure.bitnami.com/)

Δημιουργία νέων - [Azure Marketplace - δημιουργία νέων ελεγκτή Τομέα/OS στο Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - Azure κοντέινερ υπηρεσίας με Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins πλατφόρμα](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Ολοκλήρωση της εγκατάστασης στον Azure
Για να ξεκινήσει να χρησιμοποιεί Azure χρειάζεστε Azure λογαριασμού, το Azure CLI εγκατεστημένη και ένα ζεύγος SSH δημόσια και ιδιωτικά κλειδιά.

## <a name="sign-up-for-an-account"></a>Εγγραφείτε για ένα λογαριασμό
Το πρώτο βήμα χρησιμοποιώντας το Cloud Azure είναι να εγγραφείτε για ένα λογαριασμό Azure.  Μεταβείτε στη σελίδα [Εγγραφής λογαριασμού Azure](https://azure.microsoft.com/pricing/free-trial/) για να ξεκινήσετε.

## <a name="install-the-cli"></a>Εγκαταστήστε το CLI
Με το νέο λογαριασμό Azure, μπορείτε να ξεκινήσετε αμέσως με την πύλη Azure, το οποίο είναι ένα πλαίσιο διαχείρισης που βασίζεται στο web.  Για να διαχειριστείτε το Cloud Azure μέσω γραμμή εντολών, θα εγκαταστήσετε το `azure-cli`.  Εγκαταστήστε το [Azure CLI ](../xplat-cli-install.md)στη σας σταθμούς εργασίας Mac ή Linux.

## <a name="create-an-ssh-key-pair"></a>Δημιουργήστε ένα ζεύγος κλειδιών SSH
Τώρα έχετε Azure λογαριασμού, στην πύλη Azure web και το Azure CLI.  Το επόμενο βήμα είναι να δημιουργήσετε ένα ζεύγος κλειδιών SSH που χρησιμοποιείται για να SSH σε Linux χωρίς να χρησιμοποιήσετε έναν κωδικό πρόσβασης.  [Δημιουργία SSH τα πλήκτρα σε Linux και Mac](virtual-machines-linux-mac-create-ssh-keys.md) για να ενεργοποιήσετε την συνδέσεις χωρίς τον κωδικό πρόσβασης και μεγαλύτερη ασφάλεια.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Γρήγορα αποτελέσματα με το Linux στο Microsoft Azure
Με την εγκατάσταση του Azure λογαριασμού, το Azure CLI εγκατεστημένο και SSH κλειδιά που έχουν δημιουργηθεί τώρα είστε έτοιμοι να ξεκινήσετε να δημιουργείτε μια υποδομή στο Azure Cloud.  Η πρώτη εργασία είναι να δημιουργήσετε δύο ΣΠΣ.

## <a name="create-a-vm-using-the-cli"></a>Δημιουργήστε μια Εικονική χρησιμοποιώντας το CLI
Δημιουργία μια Εικονική Linux χρησιμοποιώντας το CLI είναι ένας γρήγορος τρόπος για να αναπτύξετε μια Εικονική χωρίς να κλείσετε το terminal εργάζεστε σε.  Όλα όσα μπορείτε να καθορίσετε στην πύλη web είναι διαθέσιμη μέσω της γραμμής εντολών σημαίας ή μετάβαση.  

- [Δημιουργήστε μια Εικονική Linux χρησιμοποιώντας το CLI](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Δημιουργήστε μια Εικονική στην πύλη
Δημιουργία μια Εικονική Linux στην πύλη του Azure web είναι ένας τρόπος για να τοποθετήστε το δείκτη του και κάντε κλικ στην επιλογή έως τις διάφορες επιλογές για να μεταβείτε σε μια ανάπτυξη εύκολα.  Αντί για τη χρήση της γραμμής εντολών σημαίες ή διακόπτες, θα μπορείτε να προβάλετε μια διάταξη web όμορφες και διάφορες επιλογές και τις ρυθμίσεις.  Όλα τα στοιχεία που είναι διαθέσιμα μέσω το περιβάλλον γραμμής εντολών είναι επίσης διαθέσιμες στην πύλη.

- [Δημιουργήστε μια Εικονική Linux με την πύλη](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Συνδεθείτε χρησιμοποιώντας SSH χωρίς κωδικό πρόσβασης
Η Εικονική εκτελείται τώρα στον Azure και είστε έτοιμοι να συνδεθείτε στο.  Χρήση κωδικών πρόσβασης για να συνδεθείτε μέσω SSH είναι ασφαλή και χρονοβόρα.  Χρήση των πλήκτρων SSH είναι ο πιο ασφαλής τρόπος και, επίσης, ο πιο γρήγορος τρόπος για να συνδεθείτε.  Όταν δημιουργείτε που Εικονική Linux μέσω της πύλης ή το CLI, έχετε δύο επιλογές ελέγχου ταυτότητας.  Εάν επιλέξετε έναν κωδικό πρόσβασης για SSH, Azure ρυθμίζει τις παραμέτρους του Εικονική για να επιτρέψετε συνδέσεις μέσω κωδικών πρόσβασης.  Εάν επιλέξατε να χρησιμοποιήσετε ένα δημόσιο κλειδί SSH, Azure ρυθμίζει την εικονική Μηχανή ώστε να επιτρέπει μόνο συνδέσεις μέσω πλήκτρα SSH και απενεργοποιεί συνδέσεις τον κωδικό πρόσβασης. Για να ασφαλίσετε σας Εικονική Linux, επιτρέποντας μόνο SSH κλειδιού συνδέσεις, χρησιμοποιήστε την SSH δημόσια κλειδιού επιλογή κατά τη δημιουργία Εικονική στην πύλη ή CLI.

- [Απενεργοποίηση SSH τους κωδικούς πρόσβασης σε σας Εικονική Linux με τη ρύθμιση των παραμέτρων SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Σχετικά στοιχεία Azure

## <a name="storage"></a>Χώρος αποθήκευσης

- [Εισαγωγή στο χώρο αποθήκευσης του Windows Azure](../storage/storage-introduction.md)

- [Προσθέσετε ένα δίσκο σε μια Εικονική Linux χρησιμοποιώντας το cli azure](virtual-machines-linux-add-disk.md)

- [Πώς να επισυνάψετε ένα δίσκο δεδομένων σε μια Εικονική Linux στην πύλη του Azure](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Δικτύωση

- [Επισκόπηση εικονικού δικτύου](../virtual-network/virtual-networks-overview.md)

- [Διευθύνσεις IP στο Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Άνοιγμα θυρών σε μια Εικονική Linux στα Azure](virtual-machines-linux-nsg-quickstart.md)

- [Δημιουργήστε ένα πλήρως προσδιορισμένο όνομα τομέα στην πύλη του Azure](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Κοντέινερ

- [Εικονικές μηχανές και κοντέινερ στο Azure](virtual-machines-linux-containers.md)

- [Εισαγωγή Azure κοντέινερ υπηρεσίας](../container-service/container-service-intro.md)

- [Ανάπτυξη ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα έχετε μια επισκόπηση του Linux στην Azure.  Το επόμενο βήμα είναι να εμβάθυνση και να δημιουργήσετε μερικά ΣΠΣ!

- [Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας την πύλη](virtual-machines-linux-quick-create-portal.md)

- [Δημιουργήστε μια Εικονική Linux στην Azure χρησιμοποιώντας το CLI](virtual-machines-linux-quick-create-cli.md)

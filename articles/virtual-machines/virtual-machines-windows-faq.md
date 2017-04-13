<properties
    pageTitle="Συνήθεις Ερωτήσεις για το Windows ΣΠΣ | Microsoft Azure"
    description="Παρέχει απαντήσεις σε ορισμένες συνήθεις ερωτήσεις σχετικά με εικονικές μηχανές Windows που δημιουργήθηκε με το μοντέλο από διαχειριστή πόρων."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Συνήθεις ερωτήσεις σχετικά με το Windows εικονικές μηχανές 


Σε αυτό το άρθρο διευθύνσεις ορισμένες συνήθεις ερωτήσεις σχετικά με τις εικονικές μηχανές Windows που έχουν δημιουργηθεί με Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων. Για την έκδοση Linux αυτού του θέματος, ανατρέξτε στο θέμα [Συνήθεις ερώτηση σχετικά με το Linux εικονικές μηχανές](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Τι μπορώ να εκτελέσω σε μια Εικονική Azure;

Όλες οι συνδρομητές να εκτελέσετε λογισμικό διακομιστή σε μια εικονική μηχανή Azure. Για πληροφορίες σχετικά με την πολιτική υποστήριξης για την εκτέλεση λογισμικού διακομιστή της Microsoft στο Azure, ανατρέξτε στο θέμα [υποστήριξη για εικονικές μηχανές Windows Azure λογισμικό διακομιστή της Microsoft](https://support.microsoft.com/kb/2721672)

Ορισμένες εκδόσεις των Windows 7 και Windows 8.1 είναι διαθέσιμες σε συνδρομητές όφελος MSDN Azure και προγραμματιστές MSDN και δοκιμή Pay-As-You-Go συνδρομητές, για τις εργασίες ανάπτυξης και δοκιμής. Για λεπτομέρειες, όπως οδηγίες και τους περιορισμούς, ανατρέξτε στο θέμα [εικόνες Windows προγράμματος-πελάτη για τους συνδρομητές του MSDN](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Πόσο χώρο αποθήκευσης μπορώ να χρησιμοποιήσω με μια εικονική μηχανή;

Κάθε δίσκο δεδομένων μπορεί να είναι έως 1 TB. Τον αριθμό των δίσκων δεδομένων που μπορείτε να χρησιμοποιήσετε εξαρτάται από το μέγεθος του η εικονική μηχανή. Για λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-windows-sizes.md).

Ένα λογαριασμό Azure αποθήκευσης παρέχει χώρο αποθήκευσης για το λειτουργικό σύστημα δίσκο και οποιαδήποτε δίσκων δεδομένων. Κάθε δίσκος είναι ένα αρχείο .vhd που έχουν αποθηκευτεί ως ένα αντικείμενο blob σελίδας. Για τις τιμές λεπτομέρειες, ανατρέξτε στο θέμα [Λεπτομέρειες τιμολόγησης χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Πώς μπορώ να αποκτήσω πρόσβαση μου εικονική μηχανή;

Δημιουργήσετε μια σύνδεση απομακρυσμένης χρησιμοποιώντας τη σύνδεση απομακρυσμένης επιφάνειας εργασίας (RDP) για μια Εικονική των Windows. Για οδηγίες, ανατρέξτε στο θέμα [Πώς να συνδεθείτε και να συνδεθείτε με μια Azure εικονική μηχανή με Windows](virtual-machines-windows-connect-logon.md). Έως και δύο ταυτόχρονες συνδέσεις υποστηρίζονται, εκτός εάν ο διακομιστής έχει ρυθμιστεί ως κεντρικός υπολογιστής περιόδου λειτουργίας υπηρεσιών απομακρυσμένης επιφάνειας εργασίας.  


Εάν αντιμετωπίζετε προβλήματα με σύνδεση απομακρυσμένης επιφάνειας εργασίας, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων απομακρυσμένης επιφάνειας εργασίας συνδέσεις για να βασίζεται σε Windows Azure εικονικό μηχάνημα](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Εάν είστε εξοικειωμένοι με το Hyper-V, ενδέχεται να αναζητάτε για παρόμοια με VMConnect εργαλείο. Azure δεν προσφέρει παρόμοιου εργαλείου επειδή κονσόλας πρόσβαση σε μια εικονική μηχανή δεν υποστηρίζονται.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Μπορώ να χρησιμοποιήσω το προσωρινό δίσκο (μονάδα δίσκου δ: από προεπιλογή) για την αποθήκευση δεδομένων;

Μην χρησιμοποιείτε τον προσωρινό δίσκο για την αποθήκευση δεδομένων. Αυτό είναι μόνο προσωρινό χώρο αποθήκευσης, ώστε να μπορείτε να ρισκάρετε να χάσετε τα δεδομένα που είναι δυνατή η ανάκτησή. Απώλεια δεδομένων μπορεί να προκύψει όταν η εικονική μηχανή μετακινείται σε ένα διαφορετικό κεντρικό υπολογιστή. Αλλαγή μεγέθους μια εικονική μηχανή, ενημερώνετε τον κεντρικό υπολογιστή, ή σφάλμα υλικού του κεντρικού υπολογιστή είναι ορισμένες από τους λόγους που μπορεί να μετακινήσετε μια εικονική μηχανή.

Εάν έχετε μια εφαρμογή που πρέπει να χρησιμοποιήσετε το γράμμα δ:, μπορείτε να εκχωρήσετε γράμματα της μονάδας δίσκου, έτσι ώστε να χρησιμοποιεί τον προσωρινό δίσκο χαρακτήρα διάφορο δ:. Για οδηγίες, ανατρέξτε στο θέμα [Αλλαγή το γράμμα της του δίσκου προσωρινό των Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Πώς μπορώ να αλλάξω το γράμμα της τον προσωρινό δίσκο;

Μπορείτε να αλλάξετε το γράμμα, μετακινώντας το αρχείο της σελίδας και εκ νέου εκχώρηση γράμματα της μονάδας δίσκου, αλλά πρέπει να βεβαιωθείτε ότι μπορείτε να κάνετε τα βήματα σε μια συγκεκριμένη σειρά. Για οδηγίες, ανατρέξτε στο θέμα [Αλλαγή το γράμμα της του δίσκου προσωρινό των Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Μπορώ να προσθέσω μια υπάρχουσα Εικονική σε ένα σύνολο διαθεσιμότητα;

Όχι. Εάν θέλετε το Εικονική να αποτελεί τμήμα ενός συνόλου διαθεσιμότητα, πρέπει να δημιουργήσετε την εικονική Μηχανή εντός του συνόλου. Αυτήν τη στιγμή δεν υπάρχει ένας τρόπος για να προσθέσετε μια Εικονική διαθεσιμότητα Ορισμός αφού έχει δημιουργηθεί.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Μπορώ να αποστείλετε μια εικονική μηχανή στο Azure;

Ναι. Για οδηγίες, ανατρέξτε στο θέμα [αποστείλετε μια εικόνα Εικονική των Windows για να Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Να αλλάξω το μέγεθος του δίσκου λειτουργικού Συστήματος;

Ναι. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να αναπτύξετε τη μονάδα δίσκου λειτουργικού Συστήματος των μια εικονική μηχανή σε μια ομάδα πόρων του Azure](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Μπορώ να αντιγράψω ή να κλωνοποίηση μια υπάρχουσα Εικονική Azure;

Ναι. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του Windows εικονικό μηχάνημα στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Γιατί δεν βλέπω κεντρικής Καναδά και περιοχές Ανατολή Καναδά μέσω της διαχείρισης πόρων Azure;

Τα δύο νέες περιοχές της κεντρικής Καναδά και Καναδά Ανατολή δεν καταχωρούνται αυτόματα για τη δημιουργία εικονική μηχανή για υπάρχοντα Azure συνδρομές. Αυτή η καταχώρηση γίνεται αυτόματα όταν μια εικονική μηχανή έχει αναπτυχθεί μέσω της πύλης Azure για οποιαδήποτε άλλη περιοχή χρήση της διαχείρισης πόρων Azure. Αφού μια εικονική μηχανή έχει αναπτυχθεί σε οποιαδήποτε άλλη Azure περιοχή, θα πρέπει να είναι διαθέσιμη για οι επόμενες εικονικές μηχανές στις νέες περιοχές.

## <a name="does-azure-support-linux-vms"></a>Υποστηρίζει Azure ΣΠΣ Linux;

Ναι. Για να δημιουργήσετε γρήγορα μια Εικονική Linux να δοκιμάσετε, ανατρέξτε στο θέμα [Δημιουργία μια Εικονική Linux στην Azure με την πύλη](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Μπορώ να προσθέσω μια NIC για να μου Εικονική αφού δημιουργηθεί;

Όχι. Προσθήκη NIC μπορεί να γίνει μόνο κατά τη δημιουργία.

## <a name="are-there-any-computer-name-requirements"></a>Υπάρχουν τυχόν απαιτήσεις όνομα υπολογιστή;

Ναι. Το όνομα του υπολογιστή μπορεί να είναι έως 15 χαρακτήρες. Για περισσότερες πληροφορίες σχετικά με ονομασία τους πόρους σας, ανατρέξτε στο θέμα [οδηγίες ονομασίας υποδομής](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Ποιες είναι οι απαιτήσεις όνομα χρήστη κατά τη δημιουργία μια Εικονική;

Τα ονόματα των χρηστών, μπορεί να είναι έως 20 χαρακτήρες σε μήκος και δεν μπορεί να τελειώνει με τελεία ("."). 

Δεν επιτρέπονται τα παρακάτω τα ονόματα των χρηστών:

<table>
    <tr>
        <td style="text-align:center">διαχειριστής </td><td style="text-align:center"> διαχειριστής </td><td style="text-align:center"> χρήστη </td><td style="text-align:center"> user1</td>
    </tr>
    <tr>
        <td style="text-align:center">δοκιμή </td><td style="text-align:center"> User2 </td><td style="text-align:center"> Δοκιμή1 </td><td style="text-align:center"> χρήστη3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> μια</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> adm </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">Δημιουργία αντιγράφων ασφαλείας </td><td style="text-align:center"> Κονσόλα </td><td style="text-align:center"> ο Γιώργος κάνει </td><td style="text-align:center"> επισκέπτης</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> κάτοχος </td><td style="text-align:center"> ρίζας </td><td style="text-align:center"> διακομιστή</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> υποστήριξη </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> συστήματος</td>
    </tr>
    <tr>
        <td style="text-align:center">TEST2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> ο χρήστης4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Ποιες είναι οι απαιτήσεις κωδικού πρόσβασης κατά τη δημιουργία μια Εικονική;

Κωδικοί πρόσβασης πρέπει να είναι χαρακτήρες 8-123 και ικανοποιούν 3 από τις ακόλουθες απαιτήσεις 4 πολυπλοκότητα:

- Έχετε κάτω χαρακτήρες
- Έχει επάνω χαρακτήρες
- Έχετε ένα ψηφίο
- Έχετε έναν ειδικό χαρακτήρα (Regex ταιριάζουν [\W_])

Δεν επιτρέπονται οι παρακάτω κωδικοί πρόσβασης:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ $ το word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Ο κωδικός πρόσβασης!</td><td style="text-align:center">Κωδικού πρόσβασης1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>

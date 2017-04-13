<properties
    pageTitle="Συνήθεις Ερωτήσεις για το Linux ΣΠΣ | Microsoft Azure"
    description="Παρέχει απαντήσεις σε ορισμένες συνήθεις ερωτήσεις σχετικά με Linux εικονικές μηχανές που δημιουργήθηκαν με το μοντέλο από διαχειριστή πόρων."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Συνήθεις ερωτήσεις σχετικά με το Linux εικονικές μηχανές

Σε αυτό το άρθρο διευθύνσεις ορισμένες συνήθεις ερωτήσεις σχετικά με το Linux εικονικές μηχανές που έχουν δημιουργηθεί με Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων. Για την έκδοση των Windows από αυτό το θέμα, ανατρέξτε στο θέμα [Συνήθεις ερώτηση σχετικά με το Windows εικονικές μηχανές](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Τι μπορώ να εκτελέσω σε μια Εικονική Azure;

Όλες οι συνδρομητές να εκτελέσετε λογισμικό διακομιστή σε μια εικονική μηχανή Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Linux στην κατανομές Azure-Endorsed](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Πόσο χώρο αποθήκευσης μπορώ να χρησιμοποιήσω με μια εικονική μηχανή;

Κάθε δίσκο δεδομένων μπορεί να είναι έως 1 TB. Τον αριθμό των δίσκων δεδομένων που μπορείτε να χρησιμοποιήσετε εξαρτάται από το μέγεθος του η εικονική μηχανή. Για λεπτομέρειες, ανατρέξτε στο θέμα [μεγέθη για εικονικές μηχανές](virtual-machines-linux-sizes.md).

Ένα λογαριασμό Azure αποθήκευσης παρέχει χώρο αποθήκευσης για το λειτουργικό σύστημα δίσκο και οποιαδήποτε δίσκων δεδομένων. Κάθε δίσκος είναι ένα αρχείο .vhd που έχουν αποθηκευτεί ως ένα αντικείμενο blob σελίδας. Για τις τιμές λεπτομέρειες, ανατρέξτε στο θέμα [Λεπτομέρειες τιμολόγησης χώρου αποθήκευσης](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Πώς μπορώ να αποκτήσω πρόσβαση μου εικονική μηχανή;

Δημιουργήστε μια απομακρυσμένη σύνδεση για να συνδεθείτε με το εικονικό υπολογιστή, με χρήση ασφαλούς κελύφους (SSH). Δείτε τις οδηγίες σχετικά με τον τρόπο σύνδεσης [από το Windows](virtual-machines-linux-ssh-from-windows.md) ή [από το Linux και Mac](virtual-machines-linux-mac-create-ssh-keys.md). Από προεπιλογή, SSH επιτρέπει έως 10 ταυτόχρονες συνδέσεις. Μπορείτε να αυξήσετε αυτόν τον αριθμό με την επεξεργασία του αρχείου ρύθμισης παραμέτρων.


Εάν αντιμετωπίζετε προβλήματα, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων ασφαλούς κελύφους (SSH) συνδέσεις](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Μπορώ να χρησιμοποιήσω το προσωρινό δίσκο (sdb1/ανάπτυξης /) για την αποθήκευση δεδομένων;

Μην χρησιμοποιείτε τον προσωρινό δίσκο (sdb1/ανάπτυξης /) για την αποθήκευση δεδομένων. Είναι μόνο εκεί για προσωρινή αποθήκευση. Υπάρχει κίνδυνος να χάσετε τα δεδομένα που είναι δυνατή η ανάκτησή.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Μπορώ να αντιγράψω ή να κλωνοποίηση μια υπάρχουσα Εικονική Azure;

Ναι. Για οδηγίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε ένα αντίγραφο του μια εικονική μηχανή Linux στο μοντέλο ανάπτυξης διαχείρισης πόρων](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Γιατί δεν βλέπω κεντρικής Καναδά και περιοχές Ανατολή Καναδά μέσω της διαχείρισης πόρων Azure;

Τα δύο νέες περιοχές της κεντρικής Καναδά και Καναδά Ανατολή δεν καταχωρούνται αυτόματα για τη δημιουργία εικονική μηχανή για υπάρχοντα Azure συνδρομές. Αυτή η καταχώρηση γίνεται αυτόματα όταν μια εικονική μηχανή έχει αναπτυχθεί μέσω της πύλης Azure για οποιαδήποτε άλλη περιοχή χρήση της διαχείρισης πόρων Azure. Αφού μια εικονική μηχανή έχει αναπτυχθεί σε οποιαδήποτε άλλη Azure περιοχή, θα πρέπει να είναι διαθέσιμη για οι επόμενες εικονικές μηχανές στις νέες περιοχές.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Μπορώ να προσθέσω μια NIC για να μου Εικονική αφού δημιουργηθεί;

Όχι. Προσθήκη NIC μπορεί να γίνει μόνο κατά τη δημιουργία.


## <a name="are-there-any-computer-name-requirements"></a>Υπάρχουν τυχόν απαιτήσεις όνομα υπολογιστή;

Ναι. Το όνομα του υπολογιστή μπορεί να είναι έως και 64 χαρακτήρες σε μήκος. Για περισσότερες πληροφορίες σχετικά με ονομασία τους πόρους σας, ανατρέξτε στο θέμα [οδηγίες ονομασίας υποδομής](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Ποιες είναι οι απαιτήσεις όνομα χρήστη κατά τη δημιουργία μια Εικονική;

Τα ονόματα των χρηστών πρέπει να είναι 1-64 χαρακτήρες.

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

Κωδικοί πρόσβασης πρέπει να είναι 6-72 χαρακτήρες και ικανοποιούν 3 από τις ακόλουθες απαιτήσεις πολυπλοκότητα 4:

- Έχετε κάτω χαρακτήρες
- Έχει επάνω χαρακτήρες
- Έχετε ένα ψηφίο
- Έχετε έναν ειδικό χαρακτήρα (Regex ταιριάζουν [\W_])

Δεν επιτρέπονται οι παρακάτω κωδικοί πρόσβασης:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ $ το word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Ο κωδικός πρόσβασης!</td>
        <td style="text-align:center">Κωδικού πρόσβασης1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>

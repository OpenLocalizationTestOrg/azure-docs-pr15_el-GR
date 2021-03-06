<properties
    pageTitle="Οδηγίες εικονικές μηχανές Windows | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με το πλήκτρο σχεδίαση και υλοποίηση οδηγίες για την ανάπτυξη εικονικές μηχανές windows στο Azure"
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Εικονικές μηχανές κατευθυντήριες γραμμές

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Σε αυτό το άρθρο εστιάζει στην κατανόηση τα απαραίτητα βήματα προγραμματισμού για τη δημιουργία και διαχείριση εικονικές μηχανές (ΣΠΣ) εντός του περιβάλλοντος του Azure.

## <a name="implementation-guidelines-for-vms"></a>Υλοποίηση οδηγίες για ΣΠΣ
Αποφάσεις:

- Πόσες ΣΠΣ χρειάζεται από διάφορα επίπεδα εφαρμογών και στοιχεία της υποδομής σας;
- Τι πόρους CPU και μνήμης κάθε Εικονική χρειάζεται και ποιες είναι οι απαιτήσεις αποθήκευσης;

Εργασίες:

- Ορισμός το φόρτο εργασίας για την εφαρμογή σας και τους πόρους του ΣΠΣ απαιτείται.
- Στοιχίστε τις απαιτήσεις πόρων για κάθε Εικονική με τον κατάλληλο τύπο μέγεθος και αποθήκευσης Εικονική.
- Ορισμός των ομάδων πόρων για το διαφορετικά επίπεδα και τα στοιχεία της υποδομής σας.
- Ορισμός του Εικονική κανόνες ονοματοθεσίας.
- Δημιουργία του ΣΠΣ χρησιμοποιώντας το PowerShell Azure, web πύλης, ή με τα πρότυπα διαχείρισης πόρων.

## <a name="virtual-machines"></a>Εικονικές μηχανές

Ένα από τα κύρια στοιχεία εντός του περιβάλλοντος του Azure είναι πιθανό ΣΠΣ. Αυτό είναι όπου μπορείτε να εκτελέσετε τις εφαρμογές βάσεων δεδομένων, υπηρεσίες ελέγχου ταυτότητας, κ.λπ.

Είναι σημαντικό να κατανοήσετε τα [διαφορετικά μεγέθη Εικονική](virtual-machines-windows-sizes.md) σωστά το μέγεθος του περιβάλλοντος από μια απόδοσης και κόστους προοπτική. Εάν σας ΣΠΣ δεν διαθέτετε αρκετές πυρήνων CPU ή μνήμη, πάσχει απόδοση της εφαρμογής σας ανεξάρτητα από το πόσο καλά έχει σχεδιαστεί και που αναπτύχθηκε. Αναθεωρήστε το προτεινόμενο φόρτο εργασίας για κάθε σειρά Εικονική ως σημείο εκκίνησης όπως μπορείτε να αποφασίσετε ποια μέγεθος Εικονική για να χρησιμοποιήσετε για κάθε στοιχείο του υποδομής σας. Μπορείτε να [αλλάξετε το μέγεθος του μια Εικονική](https://azure.microsoft.com/blog/resize-virtual-machines/) μετά την ανάπτυξη.

Χώρος αποθήκευσης παίζει κάποιο ρόλο κλειδιού σε Εικονική επιδόσεων. Μπορείτε να χρησιμοποιήσετε τυπική χώρου αποθήκευσης, που χρησιμοποιεί το κανονικό περιστρεφόμενο δίσκων, ή Premium χώρου αποθήκευσης για υψηλή όγκου εργασίας εισόδου/εξόδου και κορυφαίες επιδόσεις, που χρησιμοποιεί SSD δίσκων. Ως με το μέγεθος Εικονική, είναι το κόστος υποδείξεις σχετικά με την επιλογή το μέσο αποθήκευσης. Διαβάστε το [άρθρο οδηγίες υποδομή του χώρου αποθήκευσης](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) για να κατανοήσετε τον τρόπο σχεδίασης κατάλληλο χώρου αποθήκευσης για βέλτιστη απόδοση της ΣΠΣ σας.


## <a name="resource-groups"></a>Ομάδες πόρων
Στοιχεία όπως ΣΠΣ λογικά ομαδοποιούνται για διευκόλυνση της διαχείρισης και συντήρηση με τη χρήση [Azure ομάδες πόρων](../azure-resource-manager/resource-group-overview.md). Χρησιμοποιώντας τις ομάδες πόρων, μπορείτε να δημιουργήσετε, διαχείριση και παρακολούθηση όλους τους πόρους που απαρτίζουν μια δεδομένη εφαρμογή. Μπορείτε επίσης να εφαρμόσετε [πρόσβασης βάσει ρόλων στοιχεία ελέγχου](../active-directory/role-based-access-control-what-is.md) για να εκχωρήσετε πρόσβαση σε άλλους χρήστες με την ομάδα σας να μόνο τους πόρους που χρειάζονται. Χρειαστούν χρόνο για το σχεδιασμό σας ομάδες πόρων και αναθέσεων ρόλο. Υπάρχουν διαφορετικές προσεγγίσεις στην πραγματικότητα σχεδίαση και υλοποίηση ομάδες πόρων, επομένως, φροντίστε να διαβάσετε το [άρθρο οδηγίες ομάδες πόρων](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) για να κατανοήσετε τον καλύτερο τρόπο δημιουργίας του ΣΠΣ.


## <a name="templates"></a>Πρότυπα 
Μπορείτε να δημιουργήσετε πρότυπα, που ορίζονται από δηλωτικό JSON αρχεία, για να δημιουργήσετε το ΣΠΣ. Πρότυπα συνήθως επίσης να δημιουργήσουν το απαιτούμενο χώρο αποθήκευσης, δικτύωση, διασυνδέσεις δικτύου, διευθύνσεις IP, κ.λπ. μαζί με το ΣΠΣ τον εαυτό τους. Χρησιμοποιήστε πρότυπα για να δημιουργήσετε περιβάλλοντα συνεπή, μπορούν να αναπαραχθούν για ανάπτυξη και δοκιμές σκοπούς για την αναπαραγωγή εύκολα περιβάλλοντα παραγωγής και το αντίστροφο. Μπορείτε να διαβάσετε περισσότερα σχετικά με τη [Δημιουργία και τη χρήση προτύπων](../azure-resource-manager/resource-group-overview.md#template-deployment) για να κατανοήσετε πώς μπορείτε να τις χρησιμοποιήσετε για τη δημιουργία και την ανάπτυξη του ΣΠΣ.


## <a name="next-steps"></a>Επόμενα βήματα
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 
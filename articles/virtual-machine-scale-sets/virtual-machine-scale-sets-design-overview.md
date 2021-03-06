<properties
    pageTitle="Σχεδίαση κλίμακα εικονική μηχανή σύνολα για κλίμακα | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τον τρόπο σύνολα σας κλίμακα εικονική μηχανή για την κλίμακα σχεδίασης"
    keywords="Ορίζει κλίμακα εικονική μηχανή Linux εικονικό υπολογιστή," 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="gatneil"/>

# <a name="designing-vm-scale-sets-for-scale"></a>Σχεδίαση κλίμακα Εικονική σύνολα για κλίμακας

Αυτό το θέμα περιγράφει θέματα σχεδίασης για εικονική μηχανή κλίμακα σύνολα. Για πληροφορίες σχετικά με το τι είναι οι εικονική μηχανή κλίμακα σύνολα, ανατρέξτε στην [Επισκόπηση σύνολα κλίμακα εικονική μηχανή](virtual-machine-scale-sets-overview.md).


## <a name="storage"></a>Χώρος αποθήκευσης

Ένα σύνολο κλίμακα χρησιμοποιεί λογαριασμούς χώρου αποθήκευσης για την αποθήκευση των δίσκων λειτουργικό σύστημα του του ΣΠΣ του συνόλου. Συνιστάται να αναλογία 20 ΣΠΣ ανά λογαριασμού χώρου αποθήκευσης ή λιγότερο. Συνιστάται επίσης ότι που εκτείνεται αλφαβήτου τους αρχικούς χαρακτήρες από τα ονόματα λογαριασμού χώρου αποθήκευσης. Με τον τρόπο, ώστε να συμβάλλει που εκτείνεται φόρτωσης διαφορετικό εσωτερικά συστήματα. Για παράδειγμα, στο πρότυπο της παρακάτω, χρησιμοποιούμε τη συνάρτηση προτύπου για τη διαχείριση πόρων uniqueString για τη δημιουργία κατακερματισμών πρόθεμα που θα τοποθετηθούν μπροστά από χώρο αποθήκευσης ονόματα λογαριασμών: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## <a name="overprovisioning"></a>Overprovisioning

Ξεκινώντας με την έκδοση API "2016-03-30", σύνολα κλίμακα Εικονική από προεπιλογή "overprovisioning" ΣΠΣ. Με overprovisioning ενεργοποιημένη, της κλίμακας ρύθμιση στην πραγματικότητα περιστροφές του ΣΠΣ περισσότερες από αυτές που ζητηθούν, στη συνέχεια, διαγράφει το επιπλέον ΣΠΣ που απορρίμματα του τελευταίου. Overprovisioning βελτιώνει παροχής συντελεστές επιτυχίας. Δεν χρέωσης για αυτές τις επιπλέον ΣΠΣ και δεν μετρούν των ορίων.

Ενώ overprovisioning βελτίωση παροχής συντελεστές επιτυχίας, μπορεί να προκαλέσει σύγχυση συμπεριφορά για μια εφαρμογή που δεν έχει σχεδιαστεί για το χειρισμό ΣΠΣ να εξαφανίζονται χωρίς προειδοποίηση. Για να ενεργοποιήσετε overprovisioning απενεργοποιημένη, βεβαιωθείτε ότι έχετε την ακόλουθη συμβολοσειρά στο πρότυπό σας: "overprovision": "false". Μπορείτε να βρείτε περισσότερες λεπτομέρειες στην [τεκμηρίωση Εικονική κλίμακα Ορισμός REST API](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Εάν απενεργοποιήσετε το overprovisioning, μπορείτε να λάβετε δεν βρίσκομαι στον υπολογιστή με το μεγαλύτερο αναλογία ΣΠΣ ανά λογαριασμού χώρου αποθήκευσης, αλλά αυτό δεν συνιστάται ξεκινήστε πάνω από 40.


## <a name="limits"></a>Όρια
Ένα σύνολο κλίμακα ενσωματωμένη σε μια προσαρμοσμένη εικόνα (ένα ενσωματωμένο από εσάς) πρέπει να δημιουργήσετε όλα VHD δίσκου OS μέσα σε ένα λογαριασμό του χώρου αποθήκευσης. Ως αποτέλεσμα, το μέγιστο αριθμό ΣΠΣ σε ένα σύνολο κλίμακα ενσωματωμένη σε μια προσαρμοσμένη εικόνα προτεινόμενα είναι 20. Εάν απενεργοποιήσετε το overprovisioning, μπορείτε να μεταβείτε προς τα επάνω στο 40.

Ένα σύνολο κλίμακα ενσωματωμένη σε μια εικόνα πλατφόρμα περιορίζεται αυτήν τη στιγμή σε 100 ΣΠΣ (συνιστάται να 5 λογαριασμών χώρου αποθήκευσης για αυτήν την κλίμακα).

Για περισσότερες ΣΠΣ από αυτά τα όρια επιτρέψετε, πρέπει να αναπτύξετε πολλά σύνολα κλίμακα, όπως φαίνεται σε [αυτό το πρότυπο](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale).
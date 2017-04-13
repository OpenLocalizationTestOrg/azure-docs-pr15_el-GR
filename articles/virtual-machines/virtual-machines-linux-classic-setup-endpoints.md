<properties
    pageTitle="Ρύθμιση τελικά σημεία σε μια Εικονική κλασική Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τα τελικά σημεία για μια Εικονική Linux στην πύλη του Azure κλασική ώστε να επιτρέπεται η επικοινωνία με μια εικονική μηχανή Linux στο Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Πώς μπορείτε να ρυθμίσετε τα τελικά σημεία σε Linux κλασική εικονικό μηχάνημα στο Azure

Όλα Linux εικονικές μηχανές που δημιουργείτε στο Azure χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης αυτόματα μπορούν να επικοινωνούν με ένα κανάλι ιδιωτικό δίκτυο με άλλες εικονικές μηχανές στην ίδια υπηρεσία cloud ή εικονικού δικτύου. Ωστόσο, οι υπολογιστές στο Internet ή σε άλλα δίκτυα εικονικού απαιτούν τελικά σημεία για να κατευθύνετε την κίνηση δικτύου εισερχομένων για μια εικονική μηχανή. Σε αυτό το άρθρο είναι επίσης διαθέσιμο για [εικονικές μηχανές Windows](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Στο μοντέλο ανάπτυξης **Για τη διαχείριση πόρων** , τα τελικά σημεία έχουν ρυθμιστεί με **Τις ομάδες ασφαλείας δικτύου (NSGs)**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [άνοιγμα θυρών και τα τελικά σημεία] (εικονικού-μηχανές-linux-nsg-quickstart.md).

Όταν δημιουργείτε μια εικονική μηχανή Linux στην πύλη του Azure κλασική, ένα τελικό σημείο για κελύφους που ασφαλούς (SSH) συνήθως δημιουργείται για εσάς αυτόματα. Μπορείτε να ρυθμίσετε επιπλέον τελικά σημεία κατά τη δημιουργία η εικονική μηχανή ή αργότερα, ανάλογα με τις ανάγκες.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Επόμενα βήματα

* Μπορείτε επίσης να δημιουργήσετε ένα τελικό σημείο Εικονική, χρησιμοποιώντας το [περιβάλλον γραμμής εντολών Azure](../virtual-machines-command-line-tools.md). Εκτελέστε την εντολή **Δημιουργία azure εικονική τελικού σημείου** .

* Εάν έχετε δημιουργήσει μια εικονική μηχανή στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, μπορείτε να χρησιμοποιήσετε το Azure CLI στη λειτουργία διαχείρισης πόρων για να [δημιουργήσετε ομάδες ασφαλείας δικτύου](../virtual-network/virtual-networks-create-nsg-arm-cli.md) κίνηση του στοιχείου ελέγχου για να την εικονική Μηχανή.
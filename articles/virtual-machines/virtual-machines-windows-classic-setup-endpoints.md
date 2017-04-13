<properties
    pageTitle="Ρύθμιση τελικά σημεία σε μια Εικονική Windows classic | Microsoft Azure"
    description="Μάθετε πώς να ρυθμίσετε τα τελικά σημεία για μια Εικονική Windows στην πύλη του Azure κλασική ώστε να επιτρέπεται η επικοινωνία με μια εικονική μηχανή Windows στο Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Πώς μπορείτε να ρυθμίσετε τα τελικά σημεία σε κλασική Windows εικονικό μηχάνημα στο Azure


Όλα τα παράθυρα εικονικές μηχανές που δημιουργείτε στο Azure χρησιμοποιώντας το μοντέλο κλασική ανάπτυξης αυτόματα να επικοινωνείτε μέσω ένα ιδιωτικό κανάλι δικτύου με άλλες εικονικές μηχανές στην ίδια υπηρεσία cloud ή εικονικού δικτύου. Ωστόσο, οι υπολογιστές στο Internet ή σε άλλα δίκτυα εικονικού απαιτούν τελικά σημεία για να κατευθύνετε την κίνηση δικτύου εισερχομένων για μια εικονική μηχανή. Σε αυτό το άρθρο είναι επίσης διαθέσιμο για [Linux εικονικές μηχανές](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Στο μοντέλο ανάπτυξης **Για τη διαχείριση πόρων** , τα τελικά σημεία έχουν ρυθμιστεί με **Τις ομάδες ασφαλείας δικτύου (NSGs)**. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [δυνατότητα εξωτερικής πρόσβασης για να σας Εικονική με την πύλη Azure] (virtual-machines-windows-nsg-quickstart-portal.md).

Όταν δημιουργείτε μια εικονική μηχανή Windows στην πύλη του Azure κλασική, κοινά τελικά σημεία όπως αυτές για σύνδεση απομακρυσμένης επιφάνειας εργασίας και απομακρυσμένης πρόσβασης του Windows PowerShell συνήθως δημιουργούνται αυτόματα. Μπορείτε να ρυθμίσετε επιπλέον τελικά σημεία κατά τη δημιουργία η εικονική μηχανή ή αργότερα, ανάλογα με τις ανάγκες.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Επόμενα βήματα

* Για να χρησιμοποιήσετε ένα cmdlet του Azure PowerShell για να ρυθμίσετε ένα τελικό σημείο Εικονική, ανατρέξτε στο θέμα [Προσθήκη AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Για να χρησιμοποιήσετε ένα cmdlet του Azure PowerShell για τη διαχείριση ενός ACL σε ένα τελικό σημείο, ανατρέξτε στο θέμα [λίστες ελέγχου πρόσβασης Διαχείριση (ACL) για τα τελικά σημεία με χρήση του PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Εάν έχετε δημιουργήσει μια εικονική μηχανή στο μοντέλο ανάπτυξης για τη διαχείριση πόρων, μπορείτε να χρησιμοποιήσετε Azure PowerShell για τη [Δημιουργία ομάδων ασφαλείας δικτύου](../virtual-network/virtual-networks-create-nsg-arm-ps.md) κίνηση του στοιχείου ελέγχου για να την εικονική Μηχανή.

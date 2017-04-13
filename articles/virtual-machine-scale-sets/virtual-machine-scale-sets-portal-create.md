<properties
    pageTitle="Δημιουργία ενός συνόλου κλίμακα εικονική μηχανή με την πύλη Azure | Microsoft Azure"
    description="Ανάπτυξη σύνολα κλίμακα με Azure πύλη."
    keywords="σύνολα κλίμακα εικονική μηχανή" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Δημιουργία ενός συνόλου κλίμακα εικονική μηχανή με την πύλη Azure

Αυτό το πρόγραμμα εκμάθησης δείχνει πόσο εύκολο είναι να δημιουργήσετε ένα σύνολο κλίμακα εικονική μηχανή σε λίγα λεπτά, χρησιμοποιώντας την πύλη του Azure. Εάν δεν έχετε μια συνδρομή του Azure, δημιουργήστε ένα [δωρεάν λογαριασμό](https://azure.microsoft.com/free/) πριν να ξεκινήσετε.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Επιλέξτε την εικόνα Εικονική από το marketplace

Από την πύλη, μπορείτε εύκολα να αναπτύξετε μια κλίμακα που έχουν οριστεί με CentOS, CoreOS, Debian, Άνοιγμα Suse, Linux Enterprise καπέλο κόκκινο, SUSE Linux Enterprise Server, Ubuntu Server ή Windows Server εικόνες.

Αρχικά, μεταβείτε στην [πύλη του Azure](https://portal.azure.com) σε πρόγραμμα περιήγησης web. Κάντε κλικ στην επιλογή `New`, αναζητήστε `scale set`, και, στη συνέχεια, επιλέξτε το `Virtual machine scale set` καταχώρησης:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Δημιουργήστε την Linux εικονική μηχανή

Τώρα μπορείτε να χρησιμοποιήσετε τις προεπιλεγμένες ρυθμίσεις και να δημιουργήσετε γρήγορα την εικονική μηχανή.

* Στην το `Basics` blade, πληκτρολογήστε ένα όνομα για το σύνολο κλίμακα. Αυτό το όνομα γίνεται βάσης το FQDN του τη μονάδα εξισορρόπησης φόρτου μπροστά από το σύνολο κλίμακα, οπότε βεβαιωθείτε ότι το όνομα είναι μοναδικό σε όλους Azure.

* Επιλέξτε την επιθυμητή OS πληκτρολογήστε, εισαγάγετε το όνομα χρήστη που θέλετε και επιλέξτε ποια ελέγχου ταυτότητας, πληκτρολογήστε που προτιμάτε. Εάν επιλέξετε έναν κωδικό πρόσβασης, πρέπει να είναι τουλάχιστον 12 χαρακτήρες μεγάλη και πληρούν τρεις από τις τέσσερις παρακάτω απαιτήσεις πολυπλοκότητα: μία πεζούς χαρακτήρες, μία με κεφαλαία γράμματα χαρακτήρα, έναν αριθμό και ένα ειδικό χαρακτήρα. Δείτε περισσότερες πληροφορίες σχετικά με [τις απαιτήσεις όνομα χρήστη και τον κωδικό πρόσβασης](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Εάν επιλέξετε `SSH public key`, είναι ότι μόνο επικόλληση στο το δημόσιο κλειδί και δεν σας ιδιωτικό κλειδί:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Πληκτρολογήστε το όνομα της ομάδας επιθυμητή πόρων και θέση και, στη συνέχεια, κάντε κλικ στην επιλογή `OK`.

* Στην το `Virtual machine scale set service settings` blade: Εισαγάγετε την ετικέτα όνομα τομέα που θέλετε (τη βάση των το FQDN για τη μονάδα εξισορρόπησης φόρτου μπροστά από το σύνολο κλίμακα). Η ετικέτα πρέπει να είναι μοναδικό σε όλους Azure.

* Επιλέξτε σας ειδώλου του δίσκου επιθυμητή λειτουργικού συστήματος, πλήθος παρουσία και το μέγεθος του υπολογιστή.

* Ενεργοποίηση ή απενεργοποίηση autoscale και ρύθμιση παραμέτρων εάν επιτρέπεται:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Στην το `Summary` blade, όταν ολοκληρωθεί η επικύρωση, κάντε κλικ στην επιλογή `OK`.

* Τέλος, στην την `Purchase` blade, κάντε κλικ στην επιλογή `Purchase` για να ξεκινήσετε την κλίμακα Ορισμός ανάπτυξης.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Σύνδεση με μια Εικονική στα το σύνολο κλίμακας

Όταν έχει αναπτυχθεί το σύνολο κλίμακα, μεταβείτε στο το `Inbound NAT Rules` καρτέλα του την εξισορρόπηση φόρτου για το σύνολο κλίμακα:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Μπορείτε να συνδεθείτε σε κάθε Εικονική στα της κλίμακας χρησιμοποιώντας αυτούς τους κανόνες NAT. Για παράδειγμα, για ένα σύνολο κλίμακα των Windows, εάν υπάρχει ένας κανόνας NAT στην εισερχόμενη θύρα 50000, μπορείτε να συνδεθείτε σε αυτόν τον υπολογιστή μέσω RDP στην `<load-balancer-ip-address>:50000`. Για ένα σύνολο κλίμακα Linux, μπορείτε να συνδεθείτε χρησιμοποιώντας την εντολή `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Επόμενα βήματα

Για την τεκμηρίωση σχετικά με τον τρόπο για την ανάπτυξη των συνόλων κλίμακα από το CLI, ανατρέξτε [στην τεκμηρίωση αυτό](./virtual-machine-scale-sets-cli-quick-create.md).

Για την τεκμηρίωση σχετικά με τον τρόπο για την ανάπτυξη των συνόλων κλίμακα από το PowerShell, ανατρέξτε [στην τεκμηρίωση αυτό](./virtual-machine-scale-sets-windows-create.md).

Για την τεκμηρίωση σχετικά με τον τρόπο για την ανάπτυξη των συνόλων κλίμακα από το Visual Studio, ανατρέξτε [στην τεκμηρίωση αυτό](./virtual-machine-scale-sets-vs-create.md).

Για γενικές τεκμηρίωση, ελέγξτε τη [σελίδα επισκόπησης τεκμηρίωση για την κλίμακα σύνολα](./virtual-machine-scale-sets-overview.md).

Για γενικές πληροφορίες, ανατρέξτε στο θέμα στη [σελίδα κύριο προορισμού για σύνολα κλίμακα](https://azure.microsoft.com/services/virtual-machine-scale-sets/).


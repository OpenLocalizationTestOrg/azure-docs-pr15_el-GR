<properties
   pageTitle="Ανοίξτε θύρες σε μια Εικονική με την πύλη Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ανοίξετε μια θύρα / δημιουργία ένα τελικό σημείο για να σας Εικονική Windows χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων στην πύλη του Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Άνοιγμα θυρών σε μια Εικονική στο Azure χρησιμοποιώντας την πύλη του Azure
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Γρήγορες εντολές
Μπορείτε επίσης να [εκτελέσετε αυτά τα βήματα με χρήση του PowerShell Azure](virtual-machines-windows-nsg-quickstart-powershell.md).

Πρώτα, δημιουργήστε την ομάδα ασφαλείας δικτύου. Επιλέξτε έναν πόρο στην πύλη, κάντε κλικ στην επιλογή **Προσθήκη**, στη συνέχεια, αναζητήστε και επιλέξτε 'Δικτύου ομάδα ασφαλείας':

![Προσθήκη ομάδας ασφαλείας δικτύου](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Πληκτρολογήστε ένα όνομα για την ομάδα ασφαλείας σας δίκτυο, επιλέξτε ή δημιουργήστε μια ομάδα πόρων και επιλέξτε μια θέση. Κάντε κλικ στην επιλογή **Δημιουργία** όταν ολοκληρώσετε τη διαδικασία:

![Δημιουργήστε μια ομάδα ασφαλείας δικτύου](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Επιλέξτε τη νέα ομάδα ασφαλείας δικτύου. Επιλέξτε "κανόνες εισερχομένων ασφάλεια" και, στη συνέχεια, κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε έναν κανόνα:

![Προσθήκη κανόνα εισερχομένων](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Δώστε ένα όνομα για τον νέο κανόνα. Θύρα 80 έχει εισαχθεί ήδη από προεπιλογή. Αυτό blade είναι όπου μπορείτε να αλλάξετε την προέλευση, πρωτόκολλο και προορισμού κατά την προσθήκη επιπλέον κανόνες για την ομάδα ασφαλείας δικτύου. Κάντε κλικ στο **κουμπί OK** για να δημιουργήσετε τον κανόνα:

![Δημιουργία κανόνα εισερχομένων](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Το τελικό βήμα είναι να συσχετίσετε την ομάδα ασφαλείας δικτύου με ένα υποδίκτυο ή ένα συγκεκριμένο δίκτυο περιβάλλον εργασίας. Ας συσχετίσετε την ομάδα ασφαλείας δικτύου με ένα δευτερεύον δίκτυο. Επιλέξτε 'Δευτερεύοντα δίκτυα' και, στη συνέχεια, κάντε κλικ στην επιλογή **Συσχέτιση**:

![Συσχετισμός μια ομάδα ασφαλείας δικτύου με ένα υποδίκτυο](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Επιλέξτε το εικονικό δίκτυο και, στη συνέχεια, επιλέξτε το κατάλληλο υποδίκτυο:

![Συσχετισμός μια ομάδα ασφαλείας δίκτυο με το εικονικό δίκτυο](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Τώρα έχετε δημιουργήσει μια ομάδα ασφαλείας δικτύου, δημιουργήσει έναν κανόνα εισερχομένων που επιτρέπει την κυκλοφορία στη θύρα 80 και που έχει συσχετιστεί με ένα δευτερεύον δίκτυο. Οποιαδήποτε ΣΠΣ συνδέεστε σε αυτό το δευτερεύον δίκτυο είναι δυνατή η πρόσβαση στη θύρα 80.


## <a name="more-information-on-network-security-groups"></a>Περισσότερες πληροφορίες σχετικά με τις ομάδες ασφαλείας δικτύου
Η γρήγορη εντολές εδώ σάς επιτρέπουν να εξασκηθείτε στη με κίνηση ρέει προς το Εικονική. Ομάδες ασφαλείας δικτύου παρέχει πολλά εξαιρετικά δυνατότητες και υποδιαίρεσης για τον έλεγχο πρόσβασης σε πόρους σας. Μπορείτε να διαβάσετε περισσότερα σχετικά με [τη δημιουργία μιας ομάδας ασφαλείας δικτύου και κανόνες ACL εδώ](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Μπορείτε να ορίσετε κανόνες ACL και τις ομάδες ασφαλείας δικτύου ως μέρος της διαχείρισης πόρων Azure πρότυπα. Διαβάστε περισσότερα σχετικά με [τη δημιουργία δικτύου ομάδες ασφαλείας με τα πρότυπα](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Εάν πρέπει να χρησιμοποιήσετε θύρα προώθησης για να αντιστοιχίσετε ένα μοναδικό εξωτερική θύρα σε μια εσωτερική θύρα Εικονική σας, χρησιμοποιήστε μια μονάδα εξισορρόπησης φόρτου και κανόνες μετάφρασης διευθύνσεων δικτύου (NAT). Για παράδειγμα, μπορεί να θέλετε να εκθέσετε TCP θύρα 8080 εξωτερικά και έχουν κίνηση κατευθύνεται στη θύρα TCP 80 σε μια Εικονική. Μπορείτε να μάθετε σχετικά με [τη δημιουργία μια μονάδα εξισορρόπησης φόρτου μέσω Internet](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το παράδειγμα, ίσως να δημιουργήσει έναν απλό κανόνα για να επιτρέψετε την κυκλοφορία HTTP. Μπορείτε να βρείτε πληροφορίες σχετικά με τη δημιουργία πιο λεπτομερή περιβάλλοντα στα ακόλουθα άρθρα:

- [Azure Επισκόπηση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md)
- [Τι είναι μια ομάδα ασφαλείας δικτύου (NSG);](../virtual-network/virtual-networks-nsg.md)
- [Azure Επισκόπηση διαχείρισης πόρων για Balancers φόρτωσης](../load-balancer/load-balancer-arm.md)

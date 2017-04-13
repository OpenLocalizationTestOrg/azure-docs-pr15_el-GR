<properties
   pageTitle="Άνοιγμα θύρες και τελικών σημείων μια Εικονική Linux | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να ανοίξετε μια θύρα / δημιουργία ένα τελικό σημείο για να σας Εικονική Linux χρησιμοποιώντας το μοντέλο ανάπτυξης Azure πόρων διαχείρισης και την CLI Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Άνοιγμα θυρών και τελικών σημείων μια Εικονική Linux στα Azure
Ανοίξτε μια θύρα ή να δημιουργήσετε ένα τελικό σημείο, σε μια εικονική μηχανή (Εικονική) στο Azure, δημιουργώντας ένα φίλτρο δικτύου σε υποδίκτυο ή Εικονική διασύνδεση δικτύου. Μπορείτε να τοποθετήσετε τα φίλτρα, οι οποίες ελέγχουν την κυκλοφορία εισερχόμενων και εξερχόμενων, σε μια ομάδα ασφαλείας δικτύου που συνδέονται με τον πόρο που λαμβάνει την κυκλοφορία. Ας χρησιμοποιήσουμε ένα συνηθισμένο παράδειγμα κυκλοφορία web στη θύρα 80.

## <a name="quick-commands"></a>Γρήγορες εντολές
Για να δημιουργήσετε μια ομάδα ασφαλείας δικτύου και κανόνες που χρειάζεστε [Το Azure CLI](../xplat-cli-install.md) εγκαταστήσει και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `myNetworkSecurityGroup`, και `myVnet`.

Δημιουργήστε την ομάδα ασφαλείας δικτύου, πληκτρολογώντας σωστά τα δικά σας ονόματα και τη θέση. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup` στο το `WestUS` θέση:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Προσθήκη κανόνα για να επιτρέψετε την κυκλοφορία HTTP στο διακομιστή σας στο Web (ή προσαρμόστε το δικό σας σενάριο, όπως SSH access ή βάση δεδομένων της συνδεσιμότητας). Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myNetworkSecurityGroupRule` για να επιτρέψετε την κυκλοφορία TCP στη θύρα 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Συσχετισμός την ομάδα ασφαλείας δικτύου με την Εικονική διασύνδεση δικτύου (NIC). Το ακόλουθο παράδειγμα συσχετίζει ένα υπάρχον NIC με το όνομα `myNic` με την ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Εναλλακτικά, μπορείτε να συσχετίσετε την ομάδα ασφαλείας δικτύου με ένα υποδίκτυο εικονικού δικτύου και όχι μόνο στο περιβάλλον εργασίας του δικτύου, σε ένα μεμονωμένο Εικονική. Το ακόλουθο παράδειγμα συσχετίζει ένα υπάρχον υποδικτύου με το όνομα `mySubnet` στο το `myVnet` εικονικού δικτύου με την ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Περισσότερες πληροφορίες σχετικά με τις ομάδες ασφαλείας δικτύου
Η γρήγορη εντολές εδώ σάς επιτρέπουν να εξασκηθείτε στη με κίνηση ρέει προς το Εικονική. Ομάδες ασφαλείας δικτύου παρέχει πολλά εξαιρετικά δυνατότητες και υποδιαίρεσης για τον έλεγχο πρόσβασης σε πόρους σας. Μπορείτε να διαβάσετε περισσότερα σχετικά με [τη δημιουργία μιας ομάδας ασφαλείας δικτύου και κανόνες ACL εδώ](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Μπορείτε να ορίσετε κανόνες ACL και τις ομάδες ασφαλείας δικτύου ως μέρος της διαχείρισης πόρων Azure πρότυπα. Διαβάστε περισσότερα σχετικά με [τη δημιουργία δικτύου ομάδες ασφαλείας με τα πρότυπα](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Εάν πρέπει να χρησιμοποιήσετε θύρα προώθησης για να αντιστοιχίσετε ένα μοναδικό εξωτερική θύρα σε μια εσωτερική θύρα Εικονική σας, χρησιμοποιήστε μια μονάδα εξισορρόπησης φόρτου και κανόνες μετάφρασης διευθύνσεων δικτύου (NAT). Για παράδειγμα, μπορεί να θέλετε να εκθέσετε TCP θύρα 8080 εξωτερικά και έχουν κίνηση κατευθύνεται στη θύρα TCP 80 σε μια Εικονική. Μπορείτε να μάθετε σχετικά με [τη δημιουργία μια μονάδα εξισορρόπησης φόρτου μέσω Internet](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το παράδειγμα, ίσως να δημιουργήσει έναν απλό κανόνα για να επιτρέψετε την κυκλοφορία HTTP. Μπορείτε να βρείτε πληροφορίες σχετικά με τη δημιουργία πιο λεπτομερή περιβάλλοντα στα ακόλουθα άρθρα:

- [Azure Επισκόπηση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md)
- [Τι είναι μια ομάδα ασφαλείας δικτύου (NSG);](../virtual-network/virtual-networks-nsg.md)
- [Azure Επισκόπηση διαχείρισης πόρων για Balancers φόρτωσης](../load-balancer2    /load-balancer-arm.md)
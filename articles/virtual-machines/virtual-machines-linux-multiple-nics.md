<properties
   pageTitle="Δημιουργήστε μια Εικονική Linux με πολλά NIC | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε μια Εικονική Linux με πολλά NIC συνημμένη χρησιμοποιώντας τα πρότυπα Azure CLI ή διαχείριση πόρων."
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
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Δημιουργία μια Εικονική Linux με πολλά NIC
Μπορείτε να δημιουργήσετε μια εικονική μηχανή (Εικονική) στο Azure που έχει πολλές εικονικού διασυνδέσεις δικτύου (NIC) που έχουν επισυναφθεί σε αυτό. Ένα συνηθισμένο σενάριο που θα ήταν να έχει διαφορετικά δευτερεύοντα δίκτυα για προσκηνίου και παρασκηνίου, δικτύου ή σύνδεσης ενός αφοσιωμένη στο να μια λύση παρακολούθησης ή δημιουργίας αντιγράφων ασφαλείας. Σε αυτό το άρθρο παρέχει γρήγορη εντολές για να δημιουργήσετε μια Εικονική με πολλά NIC συνημμένη. Για λεπτομερείς πληροφορίες, συμπεριλαμβανομένου του τρόπου για τη δημιουργία πολλών NIC μέσα σε δέσμες ενεργειών το δικό σας πάρτι, διαβάστε περισσότερα σχετικά με [την ανάπτυξη πολλών NIC ΣΠΣ](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Διαφορετικά [μεγέθη Εικονική](virtual-machines-linux-sizes.md) υποστήριξη διαφορετικό αριθμό NIC, επομένως το μέγεθος σας Εικονική αντίστοιχα.

>[AZURE.WARNING] Πρέπει να επισυνάψετε πολλά NIC όταν μπορείτε να δημιουργήσετε μια Εικονική - δεν μπορείτε να προσθέσετε NIC σε μια υπάρχουσα Εικονική. Μπορείτε να [δημιουργήσετε μια Εικονική με βάση το αρχικό εικονικού δίσκους](virtual-machines-linux-copy-vm.md) και δημιουργήσετε πολλά NIC, όπως να αναπτύξετε την εικονική Μηχανή.

## <a name="quick-commands"></a>Γρήγορες εντολές
Βεβαιωθείτε ότι έχετε το [Azure CLI](../xplat-cli-install.md) πραγματοποιήσει είσοδο και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Τα ονόματα παραμέτρων παράδειγμα περιλαμβάνονται `myResourceGroup`, `mystorageaccount`, και `myVM`.

Πρώτα, δημιουργήστε μια ομάδα πόρων. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` θέση:

```bash
azure group create myResourceGroup -l WestUS
```

Δημιουργία λογαριασμού χώρου αποθήκευσης για τη διατήρηση του ΣΠΣ. Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Δημιουργήστε ένα εικονικό δίκτυο για να σας ΣΠΣ για να συνδεθείτε. Το ακόλουθο παράδειγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα `myVnet` με μια διεύθυνση πρόθεμα `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Δημιουργήστε δύο δευτερευόντων δικτύων εικονικού δικτύου - μία για την κυκλοφορία προσκηνίου και μία για την κίνηση παρασκηνίου. Το ακόλουθο παράδειγμα δημιουργεί δύο δευτερευόντων δικτύων, με το όνομα `mySubnetFrontEnd` και `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Δημιουργήστε δύο NIC, επισύναψη μία NIC στο υποδίκτυο προσκηνίου και μία NIC στο υποδίκτυο παρασκηνίου. Το ακόλουθο παράδειγμα δημιουργεί δύο NIC, με το όνομα `myNic1` και `myNic2`, και επισυνάπτει σας δευτερεύοντα δίκτυα:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Τέλος, δημιουργήστε το Εικονική επισύναψη τα δύο NIC που δημιουργήσατε προηγουμένως. Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική με το όνομα `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Δημιουργία πολλών NIC χρησιμοποιώντας Azure CLI
Εάν έχετε προηγουμένως δημιουργήσει μια Εικονική χρησιμοποιώντας το CLI Azure, πρέπει να είστε εξοικειωμένοι γρήγορα τις εντολές. Η διαδικασία είναι η ίδια για να δημιουργήσετε ένα NIC ή πολλά NIC. Μπορείτε να διαβάσετε περισσότερες λεπτομέρειες σχετικά με [την ανάπτυξη πολλών NIC χρησιμοποιώντας το Azure CLI](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), συμπεριλαμβανομένων των δεσμών ενεργειών της διαδικασίας βρόχου μέσω για να δημιουργήσετε όλα τα NIC.

Το ακόλουθο παράδειγμα δημιουργεί δύο NIC, με το όνομα `myNic1` και `myNic2`, με μία NIC τη σύνδεση σε κάθε υποδίκτυο:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Συνήθως δημιουργείτε επίσης μια [Ομάδα ασφαλείας δικτύου](../virtual-network/virtual-networks-nsg.md) ή [μονάδα εξισορρόπησης φόρτου](../load-balancer/load-balancer-overview.md) για να διαχειρίζεστε και να διανέμετε κίνηση κατά μήκος του ΣΠΣ. Ξανά, οι οι εντολές είναι το ίδιο, όταν εργάζεστε με πολλά NIC. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Η σύνδεση σας NIC με την ομάδα ασφαλείας δικτύου με χρήση `azure network nic set`. Το παρακάτω παράδειγμα συνδέει `myNic1` και `myNic2` με `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Κατά τη δημιουργία του Εικονική, καθορίζετε τώρα πολλών NIC. Προτιμάτε να κάνετε χρησιμοποιώντας `--nic-name` για την παροχή μια μεμονωμένη NIC, αντί για αυτό μπορείτε χρησιμοποιήσετε `--nic-names` και παρέχουν μια λίστα NIC διαχωρισμένες με κόμματα. Πρέπει επίσης να αναλαμβάνουν όταν επιλέγετε το μέγεθος Εικονική. Υπάρχουν περιορισμοί για τον συνολικό αριθμό των NIC που μπορείτε να προσθέσετε μια Εικονική. Διαβάστε περισσότερα σχετικά με τα [μεγέθη Εικονική Linux](virtual-machines-linux-sizes.md). Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να καθορίσετε πολλά NIC και, στη συνέχεια, το μέγεθος του Εικονική που υποστηρίζει τη χρήση πολλών NIC (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Δημιουργία πολλών NIC με τη χρήση προτύπων από διαχειριστή πόρων
Azure πρότυπα διαχείρισης πόρων χρησιμοποιούν δηλωτικό JSON αρχείων για να ορίσετε το περιβάλλον σας. Μπορείτε να διαβάσετε μια [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md). Πρότυπα διαχείρισης πόρων παρέχει κάποιο τρόπο για να δημιουργήσετε πολλές παρουσίες ενός πόρου κατά την ανάπτυξη, όπως τη δημιουργία πολλών NIC. Μπορείτε να χρησιμοποιήσετε *Αντιγραφή* για να καθορίσετε τον αριθμό των εμφανίσεων για να δημιουργήσετε:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Διαβάστε περισσότερα σχετικά με [τη δημιουργία πολλών παρουσιών με χρήση *αντιγραφής*](../resource-group-create-multiple.md). 

Μπορείτε επίσης να χρησιμοποιήσετε ένα `copyIndex()` για να προσαρτήσετε, στη συνέχεια, έναν αριθμό σε ένα όνομα πόρου, το οποίο σας επιτρέπει να δημιουργήσετε `myNic1`, `myNic2`κ.λπ. Ακολουθεί ένα παράδειγμα προσαρτώντας την τιμή ευρετήριο:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Μπορείτε να διαβάσετε μια ολοκληρωμένη παράδειγμα τη δημιουργία [πολλών NIC με τη χρήση προτύπων από διαχειριστή πόρων](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Επόμενα βήματα
Βεβαιωθείτε ότι για να δείτε [τα μεγέθη Εικονική Linux](virtual-machines-linux-sizes.md) όταν προσπαθείτε να δημιουργήσετε μια Εικονική με πολλά NIC. Προσέξτε να τον μέγιστο αριθμό NIC υποστηρίζει κάθε μεγέθους Εικονική. 

Να θυμάστε ότι δεν μπορείτε να προσθέσετε επιπλέον NIC σε μια υπάρχουσα Εικονική, πρέπει να δημιουργήσετε όλα τα NIC κατά την ανάπτυξη του Εικονική. Να χειριστείτε κατά το σχεδιασμό αναπτύξεις σας για να βεβαιωθείτε ότι έχετε όλα τη συνδεσιμότητα δικτύου απαιτείται από την αρχή.
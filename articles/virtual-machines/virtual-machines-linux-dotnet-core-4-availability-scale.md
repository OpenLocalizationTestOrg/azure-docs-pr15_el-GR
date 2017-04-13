<properties
   pageTitle="Διαθεσιμότητα και την κλίμακα σε Azure πρότυπα διαχείρισης πόρων | Microsoft Azure"
   description="Πρόγραμμα εκμάθησης πυρήνα DotNet Azure εικονική μηχανή"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Διαθεσιμότητα και την κλίμακα σε πρότυπα διαχείρισης πόρων Azure

Διαθεσιμότητα και την κλίμακα ανατρέξτε συνεχούς και τη δυνατότητα για να ικανοποιήσετε ζήτηση. Εάν πρέπει να είναι μια εφαρμογή του 99,9% του χρόνου, πρέπει να έχετε μια αρχιτεκτονική που επιτρέπει για πολλούς πόρους ταυτόχρονες υπολογισμού. Για παράδειγμα, αντί να χρειάζεται μια μεμονωμένη τοποθεσία Web, μια ρύθμιση παραμέτρων με υψηλότερο επίπεδο διαθεσιμότητας περιλαμβάνει πολλές παρουσίες της ίδιας τοποθεσίας, με εξισορρόπηση τεχνολογία μπροστά από τους. Σε αυτήν τη ρύθμιση, μία παρουσία της εφαρμογής μπορούν να ληφθούν προς τα κάτω για συντήρηση, ενώ το υπόλοιπο συνεχίσει να λειτουργεί. Κλίμακα από την άλλη πλευρά αναφέρεται σε μια δυνατότητα εφαρμογές για να εξυπηρετήσει ζήτηση. Με φόρτωση ισορροπημένες εφαρμογή, προσθέτοντας ή καταργώντας παρουσίες από το χώρο συγκέντρωσης επιτρέπει σε μια εφαρμογή για να κλιμακωθεί για να ικανοποιήσετε ζήτηση.

Αυτό το έγγραφο λεπτομέρειες για τον τρόπο ανάπτυξης δείγμα κατάστημα μουσικής έχει ρυθμιστεί για διαθεσιμότητα και την κλίμακα. Επισημαίνονται όλων των εξαρτήσεων και ρυθμίσεις παραμέτρων μοναδικών τιμών. Για βέλτιστη εμπειρία, η προ-ανάπτυξη μια παρουσία της λύσης για να σας Azure συνδρομής και λειτουργούν μαζί με το πρότυπο διαχείρισης πόρων Azure. Το πρότυπο ολοκλήρωσης μπορείτε να βρείτε εδώ – [Ανάπτυξης Store μουσικής στο Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Ορισμός διαθεσιμότητας

Μια ρύθμιση διαθεσιμότητα λογικά εκτείνεται σε εικονικές μηχανές Windows Azure σε φυσική hosts και άλλα στοιχεία υποδομής όπως power προμηθειών και φυσική υλικού δικτύου. Διαθεσιμότητα σύνολα να εξασφαλίσετε ότι κατά τη διάρκεια της συντήρησης, αποτυχία συσκευή ή άλλο χρόνου εκτός λειτουργίας, πραγματοποιούνται δεν όλες οι εικονικές μηχανές. Μια ρύθμιση διαθεσιμότητα μπορούν να προστεθούν σε ένα πρότυπο από διαχειριστή πόρων Azure χρησιμοποιώντας Visual Studio Προσθήκη νέου πόρου οδηγό ή εισαγωγή έγκυρη JSON σε ένα πρότυπο.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων- [Ορισμός διαθεσιμότητας](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Μια ρύθμιση διαθεσιμότητα δηλώνεται ως μια ιδιότητα ενός πόρου εικονική μηχανή. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Ρύθμιση διαθεσιμότητα συσχετισμού με εικονική μηχανή](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Διαθεσιμότητα Ορισμός όπως φαίνεται από την πύλη του Azure. Κάθε εικονική μηχανή και λεπτομέρειες σχετικά με τη ρύθμιση παραμέτρων είναι λεπτομερή εδώ.

![Ορισμός διαθεσιμότητας](./media/virtual-machines-linux-dotnet-core/aset.png)

Για λεπτομερείς πληροφορίες σχετικά με τα σύνολα διαθεσιμότητας, ανατρέξτε στο θέμα [Διαχείριση διαθεσιμότητα εικονικές μηχανές](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Εξισορρόπηση φόρτου δικτύου

Ότι ένα σύνολο διαθεσιμότητα παρέχει ανοχή σφαλμάτων εφαρμογών, μια μονάδα εξισορρόπησης φόρτου κάνει πολλές εμφανίσεις της εφαρμογής διαθέσιμα σε μια μεμονωμένη διεύθυνση δικτύου. Πολλές παρουσίες μιας εφαρμογής του μπορούν να φιλοξενηθούν σε πολλές εικονικές μηχανές, κάθε μία συνδεδεμένοι σε μια μονάδα εξισορρόπησης φόρτου. Κατά την πρόσβαση στην εφαρμογή, η εξισορρόπηση φόρτου δρομολογεί την εισερχόμενη αίτηση κατά μήκος τα συνημμένα μέλη. Μια μονάδα εξισορρόπησης φόρτου μπορούν να προστεθούν χρησιμοποιώντας Visual Studio Προσθήκη νέου πόρου οδηγό ή με την εισαγωγή σωστά μορφοποιημένο JSON πόρων στο πρότυπο Azure διαχείριση πόρων.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Εξισορρόπησης φόρτου δικτύου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Επειδή το δείγμα εφαρμογής είναι εκτεθειμένη στο Internet με μια δημόσια διεύθυνση IP, αυτή η διεύθυνση σχετίζεται με τη μονάδα εξισορρόπησης φόρτου. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [συσχετισμού εξισορρόπησης φόρτου δικτύου με δημόσια διεύθυνση IP](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Από την πύλη Azure, η επισκόπηση εξισορρόπησης φόρτου δικτύου δείχνουν τη σχέση με τη δημόσια διεύθυνση IP.

![Εξισορρόπηση φόρτου δικτύου](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Κανόνας εξισορρόπησης φόρτου

Όταν χρησιμοποιείτε μια μονάδα εξισορρόπησης φόρτου, ρυθμίζονται κανόνες που ελέγχουν τον τρόπο κυκλοφορία διανέμεται μεταξύ τους προβλεπόμενο πόρους. Με το δείγμα εφαρμογής κατάστημα μουσικής, η κυκλοφορία φτάνει στη θύρα 80 του στη δημόσια διεύθυνση IP και κατανέμεται σε θύρα 80 της όλες οι εικονικές μηχανές. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Κανόνα εξισορρόπησης φόρτου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Μια προβολή του κανόνα εξισορρόπησης φόρτου δικτύου από την πύλη.

![Κανόνας εξισορρόπησης φόρτου δικτύου](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Δοκιμή του εξισορρόπησης φόρτου

Η μονάδα εξισορρόπησης φόρτου πρέπει επίσης να παρακολουθείτε κάθε εικονική μηχανή ώστε να είναι που σερβιρίστηκε αιτήσεις μόνο στα συστήματα που εκτελούνται. Η παρακολούθηση τίθεται σε ισχύ με σταθερό Διερεύνηση μιας προκαθορισμένες θύρας. Η ανάπτυξη κατάστημα μουσικής έχει ρυθμιστεί για τη διερεύνηση θύρα 80 σε όλες οι περιλαμβάνονται εικονικές μηχανές. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Διερεύνηση εξισορρόπησης φόρτου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Τη διερεύνηση εξισορρόπησης φόρτου φαίνεται από την πύλη του Azure.

![Δοκιμή του εξισορρόπησης φόρτου δικτύου](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>NAT κανόνες εισερχομένων

Όταν χρησιμοποιείτε ένα πρόγραμμα εξισορρόπησης φόρτου, κανόνες πρέπει να τεθούν στη θέση που παρέχουν πρόσβαση ισορροπημένες μη φόρτωση σε κάθε εικονική μηχανή. Για παράδειγμα, κατά τη δημιουργία μιας σύνδεσης SSH με κάθε εικονική μηχανή, αυτήν την κυκλοφορία δεν πρέπει να εξισορρόπησης φόρτου, μάλλον πρέπει να ρυθμιστεί διαδρομής προκαθορισμένων. προκαθορισμένα διαδρομές έχουν ρυθμιστεί με έναν κανόνα εισερχομένων NAT πόρο. Με αυτόν τον πόρο, εισερχόμενη επικοινωνία μπορεί να αντιστοιχιστεί σε μεμονωμένες εικονικές μηχανές. 

Με την εφαρμογή κατάστημα μουσικής, μια θύρα ξεκινώντας από 5000 έχει αντιστοιχιστεί σε κάθε εικονική μηχανή για πρόσβαση SSH θύρα 22. Το `copyindex()` συνάρτηση χρησιμοποιείται για να αυξήσετε την εισερχόμενη θύρα, τέτοια ώστε η δεύτερη εικονική μηχανή λαμβάνει μια εισερχόμενη θύρα της 5001, την τρίτη 5002, και ούτω καθεξής.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Κανόνες εισερχομένων NAT](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Ένα παράδειγμα Εισερχόμενος κανόνας NAT, όπως φαίνεται στην πύλη του Azure. Δημιουργείται ένας κανόνας SSH NAT για κάθε εικονική μηχανή στην ανάπτυξη.

![NAT κανόνα εισερχομένων](./media/virtual-machines-linux-dotnet-core/natrule.png)

Για λεπτομερείς πληροφορίες σχετικά με τη μονάδα εξισορρόπησης φόρτου δικτύου Azure, ανατρέξτε στο θέμα [εξισορρόπηση φόρτου για τις υπηρεσίες υποδομής Azure](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Ανάπτυξη πολλών ΣΠΣ

Τέλος, για μια ρύθμιση διαθεσιμότητα ή μονάδα εξισορρόπησης φόρτου αποτελεσματική λειτουργία, πολλές εικονικές μηχανές απαιτούνται. Πολλές ΣΠΣ μπορούν να αναπτυχθούν με χρήση της συνάρτησης αντίγραφο προτύπου για τη διαχείριση πόρων Azure. Χρήση της συνάρτησης αντίγραφο, δεν χρειάζεται να ορίσετε πεπερασμένο αριθμό εικονικές μηχανές, προτιμάτε να κάνετε αυτήν την τιμή μπορεί να παρέχεται δυναμικά τη στιγμή της ανάπτυξης. Η συνάρτηση αντίγραφο καταναλώνει τον αριθμό των παρουσιών δημιουργηθεί και λαβές για την ανάπτυξη του proper αριθμού εικονικές μηχανές και τους συναφείς πόρους.

Στο πρότυπο δείγμα αποθήκευσης μουσικής, μια παράμετρος έχει οριστεί που τίθεται σε μια μέτρηση παρουσιών. Αυτός ο αριθμός χρησιμοποιείται σε όλο το πρότυπο κατά τη δημιουργία εικονικές μηχανές και σχετικοί πόροι.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Στον πόρο εικονική μηχανή, ο βρόχος αντίγραφο δίνεται ένα όνομα και τον αριθμό των παρουσιών παράμετρος που χρησιμοποιείται για να ελέγξετε τον αριθμό των αντιτύπων που θα προκύψει.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Συνάρτηση αντίγραφο εικονική μηχανή](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Είναι δυνατή η πρόσβαση του τρέχοντος διαδοχικών προσεγγίσεων της συνάρτησης αντίγραφο με το `copyIndex()` συνάρτηση. Η τιμή της συνάρτησης index αντίγραφο μπορεί να χρησιμοποιηθεί για να ονομάσετε εικονικές μηχανές και άλλων πόρων. Για παράδειγμα, αν έχουν αναπτυχθεί δύο παρουσίες του μια εικονική μηχανή, χρειάζονται διαφορετικά ονόματα. Το `copyIndex()` συνάρτηση μπορεί να χρησιμοποιηθεί ως μέρος του ονόματος εικονική μηχανή για να δημιουργήσετε ένα μοναδικό όνομα. Ένα παράδειγμα του `copyindex()` συνάρτησης που χρησιμοποιείται για την ονομασία σκοπούς μπορούν να προβληθούν στον πόρο εικονική μηχανή. Εδώ, το όνομα του υπολογιστή σας είναι μια συνένωση το `vmName` παράμετρο, και το `copyIndex()` συνάρτηση. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Συνάρτηση Index αντίγραφο](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

Το `copyIndex` συνάρτηση χρησιμοποιείται πολλές φορές σε ένα δείγμα προτύπου του χώρου αποθήκευσης μουσικής. Συναρτήσεις με χρήση και πόροι `copyIndex` περιλαμβάνει οτιδήποτε ειδικά για μια μεμονωμένη εμφάνιση η εικονική μηχανή όπως διασύνδεση δικτύου, κανόνες εξισορρόπησης φόρτου, και οποιαδήποτε εξαρτάται από συναρτήσεις. 

Για περισσότερες πληροφορίες σχετικά με τη συνάρτηση αντίγραφο, ανατρέξτε στο θέμα [Δημιουργία πολλών παρουσιών των πόρων στη Διαχείριση Azure πόρων](../resource-group-create-multiple.md).

## <a name="next-step"></a>Επόμενο βήμα

<hr>

[Βήμα 4 - ανάπτυξη εφαρμογών με το Azure πρότυπα διαχείρισης πόρων](./virtual-machines-linux-dotnet-core-5-app-deployment.md)
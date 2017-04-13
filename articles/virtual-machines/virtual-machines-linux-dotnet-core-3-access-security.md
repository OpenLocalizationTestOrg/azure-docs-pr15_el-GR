<properties
   pageTitle="Πρόσβαση και την ασφάλεια Azure πρότυπα διαχείρισης πόρων | Microsoft Azure" 
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

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Πρόσβαση και την ασφάλεια πρότυπα διαχείρισης πόρων Azure

Εφαρμογές που φιλοξενούνται στο Azure πιθανότατα πρέπει να είναι πρόσβαση μέσω του internet ή ένα VPN / Express δρομολόγηση σύνδεσης με το Azure. Με το δείγμα εφαρμογής κατάστημα μουσικής, την τοποθεσία web γίνεται διαθέσιμο στο internet με μια δημόσια διεύθυνση IP. Με την πρόσβαση σε εξέλιξη, πρέπει να διασφαλιστεί συνδέσεις με την εφαρμογή και πρόσβαση στους πόρους εικονική μηχανή τον εαυτό τους. Η ασφάλεια της access αυτό παρέχεται με μια ομάδα ασφαλείας δικτύου. 

Αυτό το έγγραφο "Λεπτομέρειες" Πώς προστατεύεται της εφαρμογής αποθήκευσης μουσικής στο πρότυπο διαχείρισης πόρων Azure δείγμα. Επισημαίνονται όλων των εξαρτήσεων και ρυθμίσεις παραμέτρων μοναδικών τιμών. Για βέλτιστη εμπειρία, η προ-ανάπτυξη μια παρουσία της λύσης για να σας Azure συνδρομής και λειτουργούν μαζί με το πρότυπο διαχείρισης πόρων Azure. Το πρότυπο ολοκλήρωσης μπορείτε να βρείτε εδώ – [Ανάπτυξης Store μουσικής στο Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Δημόσια διεύθυνση IP

Για την παροχή δημόσιας πρόσβασης σε έναν πόρο Azure, μπορεί να χρησιμοποιηθεί πόρου δημόσια διεύθυνση IP. Δημόσια διεύθυνση IP μπορούν να ρυθμιστούν με μια στατική ή δυναμική διεύθυνση IP. Εάν χρησιμοποιείται μια δυναμική διεύθυνση και η εικονική μηχανή διακοπεί και κατάργηση εκχώρησης, οι διευθύνσεις καταργείται. Όταν ο υπολογιστής ξεκινήσει ξανά, μπορεί να οριστεί διαφορετική δημόσια διεύθυνση IP. Για να αποτρέψετε την αλλαγή διεύθυνσης IP, μπορεί να χρησιμοποιηθεί ένα δεσμευμένη διεύθυνση IP. 

Μια δημόσια διεύθυνση IP μπορούν να προστεθούν σε ένα πρότυπο από διαχειριστή πόρων Azure χρησιμοποιώντας Visual Studio Προσθήκη νέου πόρου οδηγό ή με την εισαγωγή έγκυρη JSON σε ένα πρότυπο. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Δημόσια διεύθυνση IP](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Μια δημόσια διεύθυνση IP μπορούν να συσχετιστούν με έναν προσαρμογέα εικονικού δικτύου ή μια μονάδα εξισορρόπησης φόρτου. Σε αυτό το παράδειγμα, επειδή η τοποθεσία Web κατάστημα μουσικής είναι εξισορρόπηση σε πολλές εικονικές μηχανές, τη δημόσια διεύθυνση IP είναι συνδεδεμένη με το πρόγραμμα εξισορρόπησης φόρτου.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [δημόσια διεύθυνση IP συσχετισμού με εξισορρόπηση φόρτου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

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

Στη δημόσια διεύθυνση IP όπως εμφανίζεται από την πύλη του Azure. Παρατηρήστε ότι είναι συσχετισμένη με μια μονάδα εξισορρόπησης φόρτου και όχι μια εικονική μηχανή στη δημόσια διεύθυνση IP. Balancers φόρτωση δικτύου είναι λεπτομερή στο επόμενο έγγραφο αυτής της σειράς.

![Δημόσια διεύθυνση IP](./media/virtual-machines-linux-dotnet-core/pubip.png)

Για περισσότερες πληροφορίες σχετικά με τις δημόσιες διευθύνσεις IP Azure, ανατρέξτε στο θέμα [διευθύνσεις IP στο Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Ομάδα ασφαλείας δικτύου

Αφού έχει δημιουργηθεί πρόσβασης σε πόρους Azure, πρέπει να περιοριστεί πρόσβαση σε αυτήν. Για το Azure εικονικές μηχανές, ασφαλή πρόσβαση πραγματοποιείται χρησιμοποιώντας μια ομάδα ασφαλείας δικτύου. Με το δείγμα εφαρμογής κατάστημα μουσικής, όλα πρόσβαση η εικονική μηχανή είναι περιορισμένη εκτός των μέσω θύρα 80 για πρόσβαση μέσω http, καθώς και τη θύρα 22 για SSH πρόσβαση. Μια ομάδα ασφαλείας δικτύου μπορούν να προστεθούν σε ένα πρότυπο από διαχειριστή πόρων Azure χρησιμοποιώντας Visual Studio Προσθήκη νέου πόρου οδηγό ή με την εισαγωγή έγκυρη JSON σε ένα πρότυπο.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Ομάδα ασφαλείας δικτύου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

Σε αυτό το παράδειγμα, η ομάδα ασφαλείας δικτύου είναι συσχέτιση του αντικειμένου υποδικτύου δηλωθεί στον πόρο εικονικού δικτύου. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [συσχετισμού ομάδα ασφαλείας δικτύου με εικονικού δικτύου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Παρακάτω θα δείτε την ομάδα ασφαλείας δικτύου μοιάζει από την πύλη του Azure. Παρατηρήστε ότι μπορεί να είναι μια NSG συσχετίσετε με ένα περιβάλλον εργασίας υποδικτύου ή / και δικτύου. Σε αυτήν την περίπτωση, η NSG σχετίζεται με ένα δευτερεύον. Σε αυτήν τη ρύθμιση, οι κανόνες εισερχομένων εφαρμογή σε όλες οι εικονικές μηχανές συνδεδεμένοι στο υποδίκτυο.

![Ομάδα ασφαλείας δικτύου](./media/virtual-machines-linux-dotnet-core/nsg.png)

Για λεπτομερείς πληροφορίες σχετικά με τις ομάδες ασφαλείας δικτύου, ανατρέξτε στο θέμα [Τι είναι μια ομάδα ασφαλείας δικτύου]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Επόμενο βήμα

<hr>

[Βήμα 3 - διαθεσιμότητα και την κλίμακα σε Azure πρότυπα διαχείρισης πόρων](./virtual-machines-linux-dotnet-core-4-availability-scale.md)

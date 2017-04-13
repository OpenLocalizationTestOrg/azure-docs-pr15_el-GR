<properties
   pageTitle="Ανάπτυξη τον υπολογισμό πόρους με Azure πρότυπα διαχείρισης πόρων | Microsoft Azure"
   description="Πρόγραμμα εκμάθησης πυρήνα DotNet Azure εικονική μηχανή"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Αρχιτεκτονική της εφαρμογής με τα πρότυπα για τη διαχείριση πόρων Azure

Κατά την ανάπτυξη μιας ανάπτυξης διαχείρισης πόρων Azure, απαιτήσεις υπολογισμού πρέπει να αντιστοιχιστεί σε Azure πόρους και τις υπηρεσίες. Εάν μια εφαρμογή αποτελείται από πολλά τελικά σημεία http, μια βάση δεδομένων και δεδομένων σε cache υπηρεσίας, τους πόρους Azure που φιλοξενούν κάθε από αυτά τα στοιχεία πρέπει να είναι rationalized. Για παράδειγμα, το δείγμα εφαρμογής κατάστημα μουσικής περιλαμβάνει μια εφαρμογή web που φιλοξενείται σε μια εικονική μηχανή και μια βάση δεδομένων SQL, το οποίο βρίσκεται στη βάση δεδομένων Azure SQL. 

Πώς έχουν ρυθμιστεί οι πόροι υπολογισμού κατάστημα μουσικής στο το δείγμα προτύπου για τη διαχείριση πόρων Azure λεπτομέρειες για αυτό το έγγραφο. Επισημαίνονται όλων των εξαρτήσεων και ρυθμίσεις παραμέτρων μοναδικών τιμών. Για βέλτιστη εμπειρία, η προ-ανάπτυξη μια παρουσία της λύσης για να σας Azure συνδρομής και λειτουργούν μαζί με το πρότυπο διαχείρισης πόρων Azure. Το πρότυπο ολοκλήρωσης μπορείτε να βρείτε εδώ – [Μουσικής ανάπτυξης Store στα Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="virtual-machine"></a>Εικονική μηχανή

Η εφαρμογή κατάστημα μουσικής περιλαμβάνει μια εφαρμογή web, όπου οι πελάτες να αναζητήσετε και να αγοράσετε μουσική. Παρόλο που υπάρχουν πολλές υπηρεσίες Azure που μπορεί να φιλοξενήσει εφαρμογές web, για αυτό το παράδειγμα, χρησιμοποιείται μια εικονική μηχανή. Χρησιμοποιώντας το δείγμα προτύπου κατάστημα μουσικής, έχει αναπτυχθεί μια εικονική μηχανή, σε διακομιστή web εγκατάσταση και την τοποθεσία Web του χώρου αποθήκευσης μουσικής εγκατασταθεί και ρυθμιστεί. Για αυτό το άρθρο, μόνο την ανάπτυξη εικονική μηχανή είναι λεπτομερές. Η ρύθμιση των παραμέτρων του διακομιστή web και την εφαρμογή είναι λεπτομερή μια νεότερη έκδοση του άρθρου.

Μια εικονική μηχανή μπορούν να προστεθούν σε ένα πρότυπο χρησιμοποιώντας τον οδηγό του Visual Studio Προσθήκη νέου πόρου ή με την εισαγωγή έγκυρη JSON στο πρότυπο ανάπτυξης. Κατά την ανάπτυξη μια εικονική μηχανή, απαιτούνται επίσης πολλές Σχετικοί πόροι. Εάν χρησιμοποιείτε το Visual Studio για τη δημιουργία του προτύπου, αυτοί οι πόροι δημιουργούνται για εσάς. Εάν με μη αυτόματο τρόπο η κατασκευή το πρότυπο, αυτοί οι πόροι πρέπει να εισαχθεί και να ρυθμίσει τις παραμέτρους.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [JSON εικονική μηχανή](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L285).

```none
{
  {
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
  ........<truncated>  
}
```

Όταν αναπτυχθεί, τις ιδιότητες εικονική μηχανή μπορούν να προβληθούν στην πύλη του Azure.

![Εικονική μηχανή](./media/virtual-machines-windows-dotnet-core/vm-win.png)

## <a name="storage-account"></a>Το λογαριασμό χώρου αποθήκευσης

Λογαριασμοί χώρο αποθήκευσης έχετε πολλές επιλογές αποθήκευσης και τις δυνατότητες. Για το περιβάλλον του Azure εικονικές μηχανές, ένα λογαριασμό του χώρου αποθήκευσης διατηρεί το εικονικό μονάδες σκληρού δίσκου η εικονική μηχανή και δίσκων οποιαδήποτε πρόσθετα δεδομένα. Το δείγμα κατάστημα μουσικής περιλαμβάνει ένα λογαριασμό χώρου αποθήκευσης για τη διατήρηση εικονική μονάδα σκληρού δίσκου του κάθε εικονική μηχανή στην ανάπτυξη. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Λογαριασμού χώρου αποθήκευσης](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L98).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Ένα λογαριασμό του χώρου αποθήκευσης είναι συσχέτιση με μια εικονική μηχανή μέσα στη δήλωση προτύπου για τη διαχείριση πόρων η εικονική μηχανή. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [συσχετισμού εικονική μηχανή και του λογαριασμού χώρου αποθήκευσης](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L321).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Μετά την ανάπτυξη, το λογαριασμό χώρου αποθήκευσης μπορούν να προβληθούν στην πύλη του Azure.

![Το λογαριασμό χώρου αποθήκευσης](./media/virtual-machines-windows-dotnet-core/storacct-win.png)

Κάνοντας κλικ στο κοντέινερ χώρου αποθήκευσης αντικειμένων blob λογαριασμό, μπορείτε να δείτε το αρχείο εικονική μονάδα σκληρού δίσκου για κάθε εικονική μηχανή αναπτυχθεί με το πρότυπο.

![Εικονικό μονάδες σκληρού δίσκου](./media/virtual-machines-windows-dotnet-core/vhd-win.png)

Για περισσότερες πληροφορίες σχετικά με Azure χώρου αποθήκευσης, ανατρέξτε [στην τεκμηρίωση αποθήκευσης Azure](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Εικονικό δίκτυο

Εάν μια εικονική μηχανή απαιτεί εσωτερικού δικτύου όπως τη δυνατότητα επικοινωνίας με άλλα εικονικές μηχανές και Azure πόρους, απαιτείται ένα Azure εικονικού δικτύου.  Ένα εικονικό δίκτυο δεν κάνει η εικονική μηχανή προσβάσιμα μέσω του internet. Συνδεσιμότητα με δημόσια δίκτυα απαιτεί μια δημόσια διεύθυνση IP, η οποία περιγράφεται παρακάτω σε αυτήν τη σειρά.

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON μέσα στο πρότυπο διαχείρισης πόρων – [εικονικού δικτύου και δευτερεύοντα δίκτυα](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L126).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
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
    ]
  }
}
```

Από την πύλη Azure, το εικονικό δίκτυο μοιάζει κάπως έτσι η παρακάτω εικόνα. Παρατηρήστε ότι όλες οι εικονικές μηχανές αναπτυχθεί με το πρότυπο είναι συνδεδεμένος στο δίκτυο εικονικού.

![Εικονικό δίκτυο](./media/virtual-machines-windows-dotnet-core/vnet-win.png)

## <a name="network-interface"></a>Περιβάλλον εργασίας δικτύου

 Περιβάλλον εργασίας δικτύου συνδέεται μια εικονική μηχανή με ένα εικονικό δίκτυο και πιο συγκεκριμένα με ένα δευτερεύον που έχει καθοριστεί στο εικονικό δίκτυο. 
 
 Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Διασύνδεση δικτύου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L156).
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceName'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'RDP-VM', copyIndex())]"
  ],
  "properties": {
    "ipConfigurations": [
      {
        "name": "ipconfig",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[variables('subnetRef')]"
          },
          "loadBalancerBackendAddressPools": [
            {
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Κάθε πόρο εικονική μηχανή περιλαμβάνει ένα προφίλ δικτύου. Περιβάλλον εργασίας του δικτύου είναι συσχετισμένη με την εικονική μηχανή σε αυτό το προφίλ.  

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Εικονική μηχανή προφίλ δικτύου](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L330).


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceName'), copyindex()))]"
    }
  ]
}
```

Από την πύλη Azure, το περιβάλλον εργασίας δικτύου μοιάζει κάπως έτσι η παρακάτω εικόνα. Την εσωτερική διεύθυνση IP και η συσχέτιση εικονική μηχανή μπορούν να προβληθούν στον πόρο περιβάλλοντος εργασίας δικτύου.

![Περιβάλλον εργασίας δικτύου](./media/virtual-machines-windows-dotnet-core/nic-win.png)

Για περισσότερες πληροφορίες σχετικά με Azure εικονικών δικτύων, ανατρέξτε [στην τεκμηρίωση Azure εικονικού δικτύου](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Βάση δεδομένων SQL Azure

Εκτός από μια εικονική μηχανή φιλοξενίας της τοποθεσίας Web κατάστημα μουσικής, μια βάση δεδομένων SQL Azure έχει αναπτυχθεί για τη φιλοξενία της βάσης δεδομένων αποθήκευσης μουσικής. Το πλεονέκτημα της χρήσης βάσης δεδομένων SQL Azure εδώ είναι ότι δεν απαιτείται ένα δεύτερο σύνολο εικονικές μηχανές και κλίμακα και τη διαθεσιμότητα είναι ενσωματωμένη στην υπηρεσία.

Μια βάση δεδομένων Azure SQL μπορούν να προστεθούν χρησιμοποιώντας το Visual Studio Προσθήκη νέου πόρου οδηγού, ή με την εισαγωγή έγκυρη JSON σε ένα πρότυπο. Ο πόρος SQL Server περιλαμβάνει ένα όνομα χρήστη και τον κωδικό πρόσβασης που παρέχεται δικαιώματα διαχειριστή στην παρουσία SQL. Επίσης, προστίθεται ένας πόρος τείχος προστασίας SQL. Από προεπιλογή, οι εφαρμογές που φιλοξενούνται στο Azure είναι μπορεί να συνδεθεί με την παρουσία του SQL. Για να επιτρέψετε σε εξωτερική εφαρμογή όπως SQL Server Management studio για να συνδεθείτε με την παρουσία του SQL, το τείχος προστασίας πρέπει να ρυθμίσετε. Για την επίδειξη κατάστημα μουσικής, η προεπιλεγμένη ρύθμιση παραμέτρων είναι σωστή. 

Ακολουθήστε αυτήν τη σύνδεση για να δείτε το δείγμα JSON εντός του προτύπου για τη διαχείριση πόρων – [Azure SQL DB](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L379).


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicstoresqlName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('adminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicstoresqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

Προβολή του SQL server και MusicStore βάσης δεδομένων, όπως φαίνεται στην πύλη του Azure.

![SQL Server](./media/virtual-machines-windows-dotnet-core/sql-win.png)

Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη της βάσης δεδομένων SQL Azure, ανατρέξτε [στην τεκμηρίωση βάσης δεδομένων SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Επόμενο βήμα

<hr>

[Βήμα 2 - πρόσβαση και την ασφάλεια Azure πρότυπα διαχείρισης πόρων](./virtual-machines-windows-dotnet-core-3-access-security.md)

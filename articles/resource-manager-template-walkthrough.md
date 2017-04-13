<properties
   pageTitle="Αναλυτικές οδηγίες για τη διαχείριση πόρων πρότυπο | Microsoft Azure"
   description="Αναλυτικές οδηγίες βήμα προς βήμα ενός προτύπου διαχείρισης πόρων προμήθεια μια βασική αρχιτεκτονική Azure IaaS."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Αναλυτικές οδηγίες για το πρότυπο διαχείρισης πόρων

Μία από τις ερωτήσεις πρώτης κατά τη δημιουργία ενός προτύπου είναι "Πώς να ξεκινήσετε;". Μία να ξεκινήσετε από ένα κενό πρότυπο, σύμφωνα με τη βασική δομή που περιγράφεται στο [άρθρο σύνταξης προτύπου](resource-group-authoring-templates.md#template-format), και να προσθέσετε τους πόρους και κατάλληλες παραμέτρους και μεταβλητές. Μια καλή εναλλακτική λύση είναι να ξεκινήσετε από τη [συλλογή γρήγορη έναρξη](https://github.com/Azure/azure-quickstart-templates) και αναζητήστε σενάρια παρόμοια με εκείνη που προσπαθείτε να δημιουργήσετε. Μπορείτε να συγχωνεύσετε αρκετά πρότυπα ή να επεξεργαστείτε μια υπάρχουσα να ταιριάζει με το δικό σας συγκεκριμένο σενάριο. 

Ας ρίξουμε μια ματιά σε μια κοινή υποδομή:

* Δύο εικονικές μηχανές που χρησιμοποιούν τον ίδιο λογαριασμό χώρου αποθήκευσης είναι στο ίδιο σύνολο διαθεσιμότητα και στο ίδιο υποδίκτυο εικονικού δικτύου.
* Ένα μεμονωμένο NIC και Εικονική τη διεύθυνση IP για κάθε εικονική μηχανή.
* Μια μονάδα εξισορρόπησης φόρτου με φόρτωση εξισορρόπηση κανόνα στη θύρα 80

![αρχιτεκτονική](./media/resource-group-overview/arm_arch.png)

Αυτό το θέμα που παρουσιάζει τα βήματα για τη δημιουργία ενός προτύπου για τη διαχείριση πόρων για αυτήν την υποδομή. Το τελικό πρότυπο που δημιουργείτε βασίζεται σε ένα πρότυπο γρήγορη έναρξη που ονομάζεται [2 ΣΠΣ σε πρόγραμμα εξισορρόπησης φόρτου και κανόνες εξισορρόπησης φόρτου](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

Ωστόσο, που είναι πολύ για τη δημιουργία όλα ταυτόχρονα, οπότε ας πρώτα να δημιουργήσετε ένα λογαριασμό του χώρου αποθήκευσης και το αναπτύξετε. Αφού που έχουν mastered δημιουργία του λογαριασμού χώρου αποθήκευσης, θα προσθέσετε τους άλλους πόρους και θα αναπτύξετε ξανά το πρότυπο για να ολοκληρώσετε την υποδομή.

>[AZURE.NOTE] Μπορείτε να χρησιμοποιήσετε οποιονδήποτε τύπο προγράμματος επεξεργασίας κατά τη δημιουργία του προτύπου. Visual Studio παρέχει εργαλεία που θα απλοποιήσει ανάπτυξη προτύπου, αλλά δεν χρειάζεται Visual Studio για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης. Για ένα πρόγραμμα εκμάθησης σχετικά με τη χρήση του Visual Studio για να δημιουργήσετε μια ανάπτυξη του Web App και βάση δεδομένων SQL, ανατρέξτε στο θέμα [Δημιουργία και ανάπτυξη ομάδων πόρων Azure μέσω του Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Δημιουργία του προτύπου για τη διαχείριση πόρων

Το πρότυπο είναι ένα αρχείο JSON που ορίζει όλους τους πόρους που θα αναπτύξετε. Επιτρέπει επίσης να ορίσετε παραμέτρους που καθορίζονται κατά την ανάπτυξη, μεταβλητές που συνταχθεί από άλλες τιμές και παραστάσεις και εξόδους από την ανάπτυξη. 

Ας ξεκινήσουμε με το πρότυπο απλούστερος:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Αποθήκευση αυτού του αρχείου ως **azuredeploy.json** (Σημειώστε ότι το πρότυπο μπορεί να έχει οποιοδήποτε όνομα που θέλετε, απλώς να πρέπει να είναι ένα αρχείο json).

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης
Μέσα σε ενότητα **πόροι** , προσθέστε ένα αντικείμενο το οποίο ορίζει το λογαριασμό χώρου αποθήκευσης, όπως φαίνεται παρακάτω. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Ίσως αναρωτιέστε αυτές τις ιδιότητες και τις τιμές από πού προέρχονται τα. Το ιδιότητες **Τύπος**, **όνομα**, **apiVersion**και **θέση** είναι βασικά στοιχεία που είναι διαθέσιμα για όλους τους τύπους πόρων. Μπορείτε να μάθετε σχετικά με τα κοινά στοιχεία σε [πόρους](resource-group-authoring-templates.md#resources). **όνομα** έχει οριστεί σε μια τιμή παραμέτρου που μεταβιβάζουν στο κατά τη διάρκεια της ανάπτυξης και τη **θέση** ως στη θέση που χρησιμοποιούνται από την ομάδα των πόρων. Θα εξετάσουμε πώς μπορείτε να προσδιορίσετε **τον τύπο** και **apiVersion** στις παρακάτω ενότητες.

Στην ενότητα **Ιδιότητες** περιέχει όλες τις ιδιότητες που είναι μοναδικές στο ενός συγκεκριμένου τύπου. Οι τιμές που καθορίζετε σε αυτήν την ενότητα ακριβώς συμφωνεί με τη λειτουργία ΤΟΠΟΘΈΤΗΣΗ στο το REST API για τη δημιουργία αυτού του τύπου πόρου. Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, πρέπει να δώσετε μια **accountType**. Παρατηρήστε στο [REST API για τη δημιουργία ενός λογαριασμού χώρου αποθήκευσης](https://msdn.microsoft.com/library/azure/mt163564.aspx) ότι η ενότητα Ιδιότητες της λειτουργίας REST περιλαμβάνει επίσης μια ιδιότητα **accountType** και τεκμηριώνονται τις επιτρεπόμενες τιμές. Σε αυτό το παράδειγμα, τον τύπο του λογαριασμού που έχει οριστεί σε **Standard_LRS**, αλλά θα μπορούσε να μπορείτε να καθορίσετε κάποια άλλη τιμή ή να επιτρέψετε στους χρήστες να μεταφέρουν του τον τύπο του λογαριασμού ως παράμετρο.

Τώρα ας μεταβείτε ξανά στην ενότητα **παραμέτρους** και δείτε πώς μπορείτε να καθορίσετε το όνομα του λογαριασμού χώρου αποθήκευσης. Μπορείτε να μάθετε περισσότερα σχετικά με τη χρήση των παραμέτρων στο [παραμέτρους](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Εδώ έχετε ορίσει μια παράμετρος τύπου συμβολοσειράς που θα περιέχει το όνομα του λογαριασμού χώρου αποθήκευσης. Η τιμή για αυτήν την παράμετρο θα παρέχονται κατά την ανάπτυξη προτύπου.

## <a name="deploying-the-template"></a>Για την ανάπτυξη του προτύπου
Έχουμε πλήρους προτύπου για τη δημιουργία ενός νέου λογαριασμού χώρου αποθήκευσης. Όπως μπορείτε να ανακαλέσετε, το πρότυπο έχει αποθηκευτεί σε αρχείο **azuredeploy.json** :

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Υπάρχουν αρκετά μερικοί τρόποι για να αναπτύξετε ένα πρότυπο, όπως μπορείτε να δείτε στο [άρθρο ανάπτυξης του πόρου](resource-group-template-deploy.md). Για να αναπτύξετε το πρότυπο με χρήση του PowerShell Azure, χρησιμοποιήστε:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Εναλλακτικά, για να αναπτύξετε το πρότυπο χρησιμοποιώντας Azure CLI, χρησιμοποιήστε:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Τώρα είστε ο κάτοχος του περήφανοι ενός λογαριασμού χώρου αποθήκευσης!

Για να προσθέσετε όλους τους πόρους που απαιτούνται για την ανάπτυξη της αρχιτεκτονικής που περιγράφεται στην έναρξη αυτής της εκμάθησης, θα τα επόμενα βήματα. Θα προσθέσετε αυτούς τους πόρους στο ίδιο πρότυπο που εργαζόσασταν.

## <a name="availability-set"></a>Ορισμός διαθεσιμότητας
Μετά τον ορισμό για το λογαριασμό χώρου αποθήκευσης, προσθέστε ένα ορισμένο availably για τις εικονικές μηχανές. Σε αυτήν την περίπτωση, δεν υπάρχουν πρόσθετες ιδιότητες απαιτείται, ώστε να είναι αρκετά απλό τον ορισμό της. Ανατρέξτε στο θέμα το [REST API για να δημιουργήσετε μια ρύθμιση διαθεσιμότητα](https://msdn.microsoft.com/library/azure/mt163607.aspx) για την ενότητα πλήρους ιδιότητες, σε περίπτωση που θέλετε να ορίσετε την ενημέρωση τομέα count και σφαλμάτων τομέα καταμέτρηση τιμών.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Παρατηρήστε ότι το **όνομα** έχει οριστεί στην τιμή μιας μεταβλητής. Για αυτό το πρότυπο, το όνομα του συνόλου διαθεσιμότητα απαιτείται σε μερικά διαφορετικά σημεία. Μπορείτε να διατηρήσετε πιο εύκολα το πρότυπό σας από τον ορισμό αυτήν την τιμή μόνο μία φορά και η χρήση της σε πολλές θέσεις.

Την τιμή που καθορίζετε για **τον τύπο** περιέχει την υπηρεσία παροχής του πόρου και ο τύπος του πόρου. Για σύνολα διαθεσιμότητα, η υπηρεσία παροχής πόρων είναι **Microsoft.Compute** και ο τύπος πόρου είναι **availabilitySets**. Μπορείτε να λάβετε τη λίστα των διαθέσιμων πόρων υπηρεσίες παροχής, εκτελέστε την παρακάτω εντολή PowerShell:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Ή, εάν χρησιμοποιείτε Azure CLI, μπορείτε να εκτελέσετε την ακόλουθη εντολή:
```
    azure provider list
```
Δεδομένου ότι σε αυτό το θέμα που δημιουργείτε με λογαριασμούς χώρου αποθήκευσης, εικονικές μηχανές και το εικονικό δίκτυο, θα μπορείτε να εργαστείτε με:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Για να δείτε τους τύπους πόρων για μια συγκεκριμένη υπηρεσία παροχής, εκτελέστε την ακόλουθη εντολή PowerShell:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Εναλλακτικά, για Azure CLI, την ακόλουθη εντολή θα επιστρέψει τους διαθέσιμους τύπους σε μορφή JSON και αποθηκεύστε το σε ένα αρχείο.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Θα πρέπει να βλέπετε **availabilitySets** ως έναν από τους τύπους μέσα σε **Microsoft.Compute**. Το πλήρες όνομα του τύπου είναι **Microsoft.Compute/availabilitySets**. Μπορείτε να προσδιορίσετε το όνομα του τύπου πόρων για οποιονδήποτε από τους πόρους στο πρότυπο που.

## <a name="public-ip"></a>Δημόσια IP
Ορισμός μια δημόσια διεύθυνση IP. Ξανά, εξετάστε το [REST API για δημόσιες διευθύνσεις IP](https://msdn.microsoft.com/library/azure/mt163590.aspx) για τις ιδιότητες για να ορίσετε.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Η μέθοδος εκχώρησης έχει οριστεί σε **δυναμικό** , αλλά μπορείτε να την ορίσετε την τιμή που χρειάζεστε ή να τη ρυθμίσετε για να αποδεχτείτε μια τιμή παραμέτρου. Έχετε ενεργοποιήσει τους χρήστες του προτύπου σας για τη μεταβίβαση μια τιμή για την ετικέτα ονόματος τομέα.

Τώρα, ας ρίξουμε μια ματιά πώς μπορείτε να προσδιορίσετε το **apiVersion**. Την τιμή που καθορίζετε απλώς αντιστοιχεί στην έκδοση του το REST API που θέλετε να χρησιμοποιήσετε κατά τη δημιουργία του πόρου. Επομένως, μπορείτε να δείτε την τεκμηρίωση REST API για αυτόν τον τύπο πόρου. Εναλλακτικά, μπορείτε να εκτελέσετε την ακόλουθη εντολή PowerShell για έναν συγκεκριμένο τύπο.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Που επιστρέφει τις παρακάτω τιμές:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Για να δείτε τις εκδόσεις API με Azure CLI, εκτελέστε την ίδια εντολή **Εμφάνιση azure υπηρεσία παροχής** που εμφανίζεται παραπάνω.

Κατά τη δημιουργία ενός νέου προτύπου, επιλέξτε την πιο πρόσφατη έκδοση API.

## <a name="virtual-network-and-subnet"></a>Εικονικού δικτύου και το δευτερεύον
Δημιουργήστε ένα εικονικό δίκτυο με ένα δευτερεύον δίκτυο. Εξετάστε το [REST API για εικονικού δίκτυα](https://msdn.microsoft.com/library/azure/mt163661.aspx) για όλες τις ιδιότητες για να ορίσετε.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Εξισορρόπηση φόρτου
Τώρα, θα μπορείτε να δημιουργήσετε μια εξωτερική μονάδα αντικριστές εξισορρόπησης φόρτου. Επειδή αυτό εξισορρόπηση φόρτου χρησιμοποιεί στη δημόσια διεύθυνση IP, πρέπει να δηλώσετε εξάρτησης στη δημόσια διεύθυνση IP στην ενότητα **dependsOn** . Αυτό σημαίνει ότι τη μονάδα εξισορρόπησης φόρτου δεν αναπτύσσονται μέχρι να ολοκληρωθεί η δημόσια διεύθυνση IP για την ανάπτυξη. Χωρίς τον ορισμό αυτήν την εξάρτηση, θα λάβετε ένα σφάλμα επειδή θα επιχειρήσει να αναπτύξετε τους πόρους παράλληλα διαχείρισης πόρων και θα προσπαθήσει να ορίσετε τη μονάδα εξισορρόπησης φόρτου σε δημόσια διεύθυνση IP που δεν υπάρχει ακόμα. 

Μπορείτε, επίσης, θα δημιουργήσετε ένα χώρο συγκέντρωσης διεύθυνση παρασκηνίου, ορισμένες κανόνες εισερχόμενων NAT στο RDP σε του ΣΠΣ και έναν κανόνα εξισορρόπησης φόρτου με μια δοκιμή του tcp στη θύρα 80 σε αυτόν τον ορισμό πόρων. Ανάληψη ελέγχου του [REST API για εξισορρόπηση φόρτου](https://msdn.microsoft.com/library/azure/mt163574.aspx) για όλες τις ιδιότητες.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Περιβάλλον εργασίας δικτύου
Θα δημιουργήσετε 2 διασυνδέσεις δικτύου, μία για κάθε Εικονική. Αντί να χρειάζεται να συμπεριλάβετε τις διπλότυπες εγγραφές για τις διασυνδέσεις δικτύου, μπορείτε να χρησιμοποιήσετε τη [συνάρτηση copyIndex()](resource-group-create-multiple.md) διαδοχικές προσεγγίσεις μέσω του βρόχου αντίγραφο (αναφέρεται ως nicLoop) και τη δημιουργία του αριθμού διασυνδέσεις δικτύου όπως καθορίζονται στο το `numberOfInstances` μεταβλητές. Το περιβάλλον εργασίας δικτύου εξαρτάται από τη δημιουργία του εικονικού δικτύου και τη μονάδα εξισορρόπησης φόρτου. Για να ρυθμίσετε το χώρο συγκέντρωσης διευθύνσεων εξισορρόπησης φόρτου και τους κανόνες εισερχομένων NAT χρησιμοποιεί το υποδίκτυο ορίζεται στο της δημιουργίας εικονικού δικτύου και το αναγνωριστικό εξισορρόπησης φόρτου.
Εξετάστε το [REST API για διασυνδέσεις δικτύου](https://msdn.microsoft.com/library/azure/mt163668.aspx) για όλες τις ιδιότητες.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Εικονική μηχανή
Θα δημιουργήσετε 2 εικονικές μηχανές, με χρήση συνάρτησης copyIndex(), όπως κάνατε σε δημιουργίας [διασυνδέσεις δικτύου](#network-interface).
Η δημιουργία Εικονική εξαρτάται από το λογαριασμό χώρου αποθήκευσης, δικτύου του περιβάλλοντος εργασίας και διαθεσιμότητας σύνολο. Αυτή η Εικονική θα δημιουργηθεί από μια εικόνα marketplace, όπως καθορίζονται στο το `storageProfile` ιδιότητα - `imageReference` χρησιμοποιείται για να ορίσετε την εικόνα publisher, προσφορά, sku και την έκδοση. Τέλος, διαγνωστικών προφίλ έχει ρυθμιστεί για να ενεργοποιήσετε διαγνωστικών για την εικονική Μηχανή. 

Για να βρείτε τις σχετικές ιδιότητες για μια εικόνα marketplace, ακολουθήστε τα άρθρα [Επιλέξτε Linux εικονική μηχανή εικόνες](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) ή [εικόνες εικονική μηχανή των Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Για εικόνες που δημοσιεύτηκε από **3η προμηθευτές**, θα πρέπει να καθορίσετε μια άλλη ιδιότητα με το όνομα `plan`. Παράδειγμα μπορείτε να βρείτε σε [αυτό το πρότυπο](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) από τη συλλογή γρήγορη έναρξη. 

Έχετε ολοκληρώσει τον καθορισμό των πόρων για το πρότυπό σας.

## <a name="parameters"></a>Παράμετροι

Στην ενότητα παράμετροι, ορίστε τις τιμές που μπορεί να καθοριστεί κατά την ανάπτυξη του προτύπου. Ορισμός μόνο τις παραμέτρους για τις τιμές που πιστεύετε ότι θα πρέπει να ποικίλλουν κατά την ανάπτυξη. Μπορείτε να παράσχετε μια προεπιλεγμένη τιμή για μια παράμετρο που χρησιμοποιείται εάν δεν παρέχεται κατά την ανάπτυξη. Μπορείτε επίσης να ορίσετε τη επιτρεπόμενη τιμή, όπως φαίνεται για την παράμετρο **imageSKU** .

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Μεταβλητές

Στην ενότητα μεταβλητές, μπορείτε να ορίσετε τιμές που χρησιμοποιούνται σε περισσότερα από ένα σημείο στο πρότυπό σας, ή που έχουν δημιουργηθεί από άλλες παραστάσεις ή τις μεταβλητές. Μεταβλητές που χρησιμοποιούνται συχνά για να απλοποιήσετε τη σύνταξη του προτύπου σας.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Έχετε ολοκληρώσει το πρότυπο! Μπορείτε να συγκρίνετε το πρότυπό σας σε σχέση με το πλήρες πρότυπο στη [συλλογή γρήγορη έναρξη](https://github.com/Azure/azure-quickstart-templates) στην περιοχή [2 ΣΠΣ με μονάδα εξισορρόπησης φόρτου και φόρτωση εξισορρόπησης προτύπου κανόνων](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Το πρότυπο μπορεί να είναι λίγο διαφορετική βάση με διαφορετικό αριθμό έκδοσης. 

Μπορείτε να αναπτύξετε ξανά το πρότυπο, χρησιμοποιώντας τις ίδιες εντολές που χρησιμοποιήσατε κατά την ανάπτυξη του λογαριασμού χώρου αποθήκευσης. Δεν χρειάζεται να διαγράψετε το λογαριασμό χώρου αποθήκευσης πριν αναπτύξετε εκ νέου επειδή διαχείριση πόρων θα παραλείψει εκ νέου δημιουργία τους πόρους που υπάρχουν ήδη και δεν έχουν αλλάξει.

## <a name="next-steps"></a>Επόμενα βήματα

- [Azure διαχείριση πόρων πρότυπο Visualizer (ARMViz)](http://armviz.io/#/) είναι ένα εξαιρετικό εργαλείο για την απεικόνιση ARM πρότυπα, όταν θα μπορεί να είναι πολύ μεγάλο για να κατανοήσετε μόνο από την ανάγνωση του αρχείου json.
- Για να μάθετε περισσότερα σχετικά με τη δομή ενός προτύπου, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
- Για να μάθετε περισσότερα σχετικά με την ανάπτυξη ενός προτύπου, ανατρέξτε στο θέμα [ανάπτυξη μιας ομάδας πόρων με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md)

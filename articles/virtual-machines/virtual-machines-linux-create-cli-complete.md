
<properties
   pageTitle="Δημιουργήστε ένα πλήρες περιβάλλον Linux χρησιμοποιώντας το Azure CLI | Microsoft Azure"
   description="Δημιουργία χώρου αποθήκευσης, μια Εικονική Linux, ένα εικονικό δίκτυο και υποδίκτυο, μια μονάδα εξισορρόπησης φόρτου, μια NIC, μια δημόσια IP και μια ομάδα ασφαλείας δικτύου, όλες από το μηδέν προς τα επάνω, χρησιμοποιώντας το Azure CLI."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Δημιουργήστε ένα πλήρες περιβάλλον Linux χρησιμοποιώντας το CLI Azure

Σε αυτό το άρθρο θα σας να δημιουργήσετε ένα απλό δίκτυο με μια μονάδα εξισορρόπησης φόρτου και ένα ζεύγος ΣΠΣ που είναι χρήσιμες για την ανάπτυξη και την απλή υπολογιστική. Θα σας καθοδηγήσουμε μέσα από τη διαδικασία, εντολή command, έως ότου έχετε δύο ΣΠΣ Linux εργάσιμο, ασφαλή στο οποίο μπορείτε να συνδεθείτε από οπουδήποτε στο Internet. Στη συνέχεια, μπορείτε να μετακινήσετε σε για πιο σύνθετες δίκτυα και περιβάλλοντα.

Καθώς προχωράτε, μάθετε σχετικά με την ιεραρχία εξάρτηση που παρέχει το μοντέλο ανάπτυξης διαχείρισης πόρων και σχετικά με τον όγκο του power το παρέχει. Αφού μπορείτε να δείτε πώς είναι ενσωματωμένος στο σύστημα, μπορείτε να εκ νέου δημιουργία το πολύ πιο γρήγορα με τη χρήση [προτύπων από διαχειριστή πόρων Azure](../resource-group-authoring-templates.md). Επίσης, αφού μάθετε τον τρόπο προσαρμογής μαζί τα τμήματα του περιβάλλοντός σας, τη δημιουργία προτύπων για την αυτοματοποίηση τους γίνεται πιο εύκολη.

Το περιβάλλον περιέχει:

- Δύο ΣΠΣ μέσα σε ένα σύνολο διαθεσιμότητα.
- Μια μονάδα εξισορρόπησης φόρτου με έναν κανόνα εξισορρόπησης φόρτου στη θύρα 80.
- Κανόνες ομάδας (NSG) ασφαλείας δικτύου για την προστασία σας Εικονική από ανεπιθύμητα κίνηση.

![Επισκόπηση του βασικού περιβάλλοντος](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Για να δημιουργήσετε αυτό το προσαρμοσμένο περιβάλλον, χρειάζεστε την πιο πρόσφατη [Azure CLI](../xplat-cli-install.md) στη λειτουργία διαχείρισης πόρων (`azure config mode arm`). Χρειάζεστε επίσης ένα εργαλείο ανάλυσης JSON. Σε αυτό το παράδειγμα χρησιμοποιεί [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Γρήγορες εντολές
Εάν πρέπει να εκτελέσετε γρήγορα την εργασία, τις ακόλουθες λεπτομέρειες ενότητα τη βάση εντολές για να αποστείλετε μια Εικονική στο Azure. Πιο λεπτομερείς πληροφορίες και περιβάλλοντος για κάθε βήμα μπορείτε να βρείτε στο υπόλοιπο μέρος του εγγράφου, έναρξης [εδώ](#detailed-walkthrough).

Βεβαιωθείτε ότι έχετε [Το Azure CLI](../xplat-cli-install.md) πραγματοποιήσει είσοδο και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Συμπεριλάβετε τα ονόματα παραμέτρων παράδειγμα `myResourceGroup`, `mystorageaccount`, και `myVM`.

Δημιουργήστε την ομάδα των πόρων. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `westeurope` θέση:

```bash
azure group create -n myResourceGroup -l westeurope
```

Επιβεβαιώστε την ομάδα των πόρων με τη χρήση του προγράμματος ανάλυσης JSON:

```bash
azure group show myResourceGroup --json | jq '.'
```

Δημιουργήστε το λογαριασμό χώρου αποθήκευσης. Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount` (το όνομα του λογαριασμού χώρου αποθήκευσης πρέπει να είναι μοναδικό, επομένως, πληκτρολογήστε το δικό σας μοναδικό όνομα):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Επιβεβαιώστε το λογαριασμό χώρου αποθήκευσης με τη χρήση του προγράμματος ανάλυσης JSON:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Δημιουργήστε το εικονικό δίκτυο. Το ακόλουθο παράδειγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Δημιουργήστε ένα υποδίκτυο. Το ακόλουθο παράδειγμα δημιουργεί ένα δευτερεύον δίκτυο με το όνομα `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Επιβεβαιώστε την εικονικού δικτύου και το δευτερεύον με τη χρήση του προγράμματος ανάλυσης JSON:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Δημιουργία μιας δημόσιας διευθύνσεων IP. Το ακόλουθο παράδειγμα δημιουργεί μια δημόσια IP με το όνομα `myPublicIP` με το όνομα DNS του `mypublicdns` (το όνομα DNS πρέπει να είναι μοναδικό, επομένως, πληκτρολογήστε το δικό σας μοναδικό όνομα):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Δημιουργήστε τη μονάδα εξισορρόπησης φόρτου. Το ακόλουθο παράδειγμα δημιουργεί μια μονάδα εξισορρόπησης φόρτου που ονομάζεται `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Δημιουργία χώρου συγκέντρωσης προσκηνίου IP για την εξισορρόπηση φόρτου, και να συσχετίσετε το δημόσιο διευθύνσεων IP. Το ακόλουθο παράδειγμα δημιουργεί μια προσκηνίου χώρου συγκέντρωσης διευθύνσεων IP με το όνομα `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Δημιουργία χώρου συγκέντρωσης IP παρασκηνίου για τη μονάδα εξισορρόπησης φόρτου. Το ακόλουθο παράδειγμα δημιουργεί ένα χώρο συγκέντρωσης διευθύνσεων IP παρασκηνίου με το όνομα `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Δημιουργία δικτύου εισερχόμενη SSH διεύθυνση μετάφραση (NAT) κανόνων για τη μονάδα εξισορρόπησης φόρτου. Το ακόλουθο παράδειγμα δημιουργεί δύο κανόνες εξισορρόπησης φόρτου, `myLoadBalancerRuleSSH1` και `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Δημιουργία web NAT κανόνες για τη μονάδα εξισορρόπησης φόρτου εισερχομένων. Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα εξισορρόπησης φόρτου που ονομάζεται`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Δημιουργήστε τη διερεύνηση εύρυθμης λειτουργίας εξισορρόπησης φόρτου. Το ακόλουθο παράδειγμα δημιουργεί μια δοκιμή του TCP με το όνομα `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Επιβεβαιώστε την εξισορρόπηση φόρτου, χώρους συγκέντρωσης IP και NAT κανόνες με τη χρήση του προγράμματος ανάλυσης JSON:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Δημιουργία της πρώτης κάρτας περιβάλλοντος εργασίας δικτύου (NIC). Αντικαταστήστε το `#####-###-###` ενότητες με το δικό σας αναγνωριστικό Azure συνδρομής. Αναγνωριστικό σημειώνεται στο αποτέλεσμα της τη συνδρομή σας `jq` κατά την εξέταση των πόρων που δημιουργείτε. Μπορείτε επίσης να προβάλετε το Αναγνωριστικό συνδρομής με `azure account list`. 

Το ακόλουθο παράδειγμα δημιουργεί μια NIC με το όνομα `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Δημιουργήστε το δεύτερο NIC. Το ακόλουθο παράδειγμα δημιουργεί μια NIC με το όνομα `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Επιβεβαιώστε το NIC δύο με τη χρήση του προγράμματος ανάλυσης JSON:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Δημιουργήστε την ομάδα ασφαλείας δικτύου. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Προσθέστε δύο κανόνες εισερχομένων για την ομάδα ασφαλείας δικτύου. Το ακόλουθο παράδειγμα δημιουργεί δύο κανόνες, `myNetworkSecurityGroupRuleSSH` και `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Επαληθεύστε την ομάδα ασφαλείας δικτύου και οι κανόνες εισερχόμενων με τη χρήση του προγράμματος ανάλυσης JSON:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Συνδέσετε την ομάδα ασφαλείας δικτύου με τα δύο NIC:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Δημιουργία συνόλου διαθεσιμότητα. Το ακόλουθο παράδειγμα δημιουργεί μια διαθεσιμότητα Ορισμός καθορισμένων `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Δημιουργήστε το πρώτο Εικονική Linux. Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική με το όνομα `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Δημιουργήστε το δεύτερο Εικονική Linux. Το ακόλουθο παράδειγμα δημιουργεί μια Εικονική με το όνομα `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Χρήση του προγράμματος ανάλυσης JSON για να βεβαιωθείτε ότι όλα τα στοιχεία που έχει δημιουργηθεί:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Εξαγάγετε το νέο περιβάλλον σας σε ένα πρότυπο για να δημιουργήσετε εκ νέου γρήγορα νέες παρουσίες:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Λεπτομερή ανάλυση
Τα λεπτομερή βήματα που ακολουθούν εξηγούν τι κάθε εντολή κάνει καθώς δημιουργείτε ανάληψη το περιβάλλον σας. Αυτά τα θέματα είναι χρήσιμα όταν δημιουργείτε τη δική σας προσαρμοσμένη περιβάλλοντα για ανάπτυξη ή την παραγωγή.

Βεβαιωθείτε ότι έχετε [Το Azure CLI](../xplat-cli-install.md) πραγματοποιήσει είσοδο και χρήση της κατάστασης λειτουργίας διαχείριση πόρων:

```bash
azure config mode arm
```

Στα παρακάτω παραδείγματα, αντικαταστήστε τα ονόματα παραμέτρων παράδειγμα με τις δικές σας τιμές. Συμπεριλάβετε τα ονόματα παραμέτρων παράδειγμα `myResourceGroup`, `mystorageaccount`, και `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Δημιουργήστε ομάδες πόρων και επιλέξτε ανάπτυξη θέσεις

Οι ομάδες Azure πόρων είναι οντοτήτων λογικής ανάπτυξης που περιέχουν πληροφορίες ρύθμισης παραμέτρων και μετα-δεδομένων για να ενεργοποιήσετε τη λογική διαχείριση των πόρων αναπτύξεις. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `westeurope` θέση:

```bash
azure group create --name myResourceGroup --location westeurope
```

Αποτέλεσμα:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Δημιουργία λογαριασμού χώρου αποθήκευσης

Χρειάζεστε λογαριασμούς χώρου αποθήκευσης για δίσκων σας Εικονική και για οποιονδήποτε δίσκων πρόσθετα δεδομένα που θέλετε να προσθέσετε. Μπορείτε να δημιουργήσετε λογαριασμούς χώρου αποθήκευσης σχεδόν αμέσως αφού δημιουργήσετε ομάδες πόρων.

Εδώ χρησιμοποιούμε τα `azure storage account create` εντολή, που περνά μέσα στη θέση του λογαριασμού, την ομάδα των πόρων που ελέγχει, καθώς και τον τύπο του χώρου αποθήκευσης υποστήριξης που θέλετε. Το ακόλουθο παράδειγμα δημιουργεί ένα λογαριασμό χώρου αποθήκευσης με την ονομασία `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Αποτέλεσμα:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Για να εξετάσετε μας ομάδα πόρων, χρησιμοποιώντας το `azure group show` εντολής, ας χρησιμοποιήσουμε το εργαλείο [jq](https://stedolan.github.io/jq/) μαζί με το `--json` επιλογή Azure CLI. (Μπορείτε να χρησιμοποιήσετε **jsawk** ή οποιαδήποτε βιβλιοθήκη γλώσσα που προτιμάτε για την ανάλυση του JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Για να εξερευνήσετε το λογαριασμό χώρου αποθήκευσης, χρησιμοποιώντας το CLI, πρέπει πρώτα να ορίσετε τα πλήκτρα και ονόματα λογαριασμών. Αντικαταστήστε το όνομα του λογαριασμού χώρου αποθήκευσης στο παρακάτω παράδειγμα με όνομα που επιλέγετε:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Στη συνέχεια, μπορείτε να προβάλετε τις πληροφορίες σας χώρο αποθήκευσης εύκολα:

```bash
azure storage container list
```

Αποτέλεσμα:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Δημιουργία εικονικού δικτύου και υποδίκτυο

Στη συνέχεια θα πρέπει να δημιουργήσετε ένα εικονικό δίκτυο που λειτουργεί στο Azure και ένα δευτερεύον δίκτυο στο οποίο μπορείτε να δημιουργήσετε το ΣΠΣ. Το ακόλουθο παράδειγμα δημιουργεί ένα εικονικό δίκτυο με το όνομα `myVnet` με το `192.168.0.0/16` πρόθεμα διεύθυνσης:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Αποτέλεσμα:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Ξανά, ας χρησιμοποιήσουμε την επιλογή--json `azure group show` και **jq** για να δείτε πώς θα σας δημιουργία μας πόρους. Τώρα έχουμε μια `storageAccounts` πόρων και μια `virtualNetworks` πόρων.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Τώρα ας δημιουργήσουμε ένα υποδίκτυο στο το `myVnet` εικονικού δικτύου στο οποίο έχουν αναπτυχθεί του ΣΠΣ. Χρησιμοποιούμε τα `azure network vnet subnet create` εντολής, μαζί με τους πόρους που ήδη έχετε δημιουργήσαμε: το `myResourceGroup` ομάδα πόρων και τα `myVnet` εικονικού δικτύου. Στο παρακάτω παράδειγμα, προσθέτουμε το δευτερεύον δίκτυο με το όνομα `mySubnet` με το πρόθεμα διεύθυνση υποδικτύου `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Αποτέλεσμα:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Επειδή το υποδίκτυο λογικά είναι εντός του εικονικού δικτύου, δείτε τις πληροφορίες υποδικτύου με μια εντολή λίγο διαφορετικά. Η εντολή χρησιμοποιούμε είναι `azure network vnet show`, αλλά θα συνεχίσουμε να εξετάσετε το αποτέλεσμα JSON χρησιμοποιώντας **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Δημιουργήστε μια δημόσια διεύθυνση IP (PIP)

Τώρα ας δημιουργήσουμε στη δημόσια διεύθυνση IP (PIP) που θα σας εκχώρηση σας εξισορρόπηση φόρτου. Μπορείτε να συνδεθείτε με το ΣΠΣ από το Internet, χρησιμοποιώντας το `azure network public-ip create` εντολή. Επειδή η προεπιλεγμένη διεύθυνση είναι δυναμικό, δημιουργούμε μια καθορισμένη καταχώρηση DNS στον τομέα **cloudapp.azure.com** χρησιμοποιώντας το `--domain-name-label` την επιλογή. Το ακόλουθο παράδειγμα δημιουργεί μια δημόσια IP με το όνομα `myPublicIP` με το όνομα DNS του `mypublicdns`. Καθώς το όνομα DNS πρέπει να είναι μοναδικό, πληκτρολογήστε το δικό σας μοναδικό όνομα DNS:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Αποτέλεσμα:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Στη δημόσια διεύθυνση IP είναι επίσης ένας πόρος ανώτατου επιπέδου, ώστε να μπορείτε να δείτε την με `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Αποτέλεσμα:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Μπορείτε να ερευνήσετε περισσότερες λεπτομέρειες πόρων, όπως το πλήρως προσδιορισμένο όνομα τομέα (FQDN) του ο δευτερεύων τομέας, χρησιμοποιώντας την πλήρη `azure network public-ip show` εντολή. Η δημόσια πόρων διεύθυνση IP έχει εκχωρηθεί λογικά, αλλά δεν έχει εκχωρηθεί ακόμη μια συγκεκριμένη διεύθυνση. Για να αποκτήσετε μια διεύθυνση IP, θα χρειαστείτε μια μονάδα εξισορρόπησης φόρτου, το οποίο θα σας δεν έχετε δημιουργήσει ακόμη.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Αποτέλεσμα:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου και χώρους συγκέντρωσης IP
Όταν δημιουργείτε μια μονάδα εξισορρόπησης φόρτου, σας επιτρέπει να διανείμετε κίνηση σε πολλές ΣΠΣ. Παρέχει επίσης πλεονασμού στην εφαρμογή σας, εκτελώντας πολλών ΣΠΣ που απάντηση σε αιτήματα χρήστη σε περίπτωση συντήρησης ή χοντρό φορτία. Το ακόλουθο παράδειγμα δημιουργεί μια μονάδα εξισορρόπησης φόρτου που ονομάζεται `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Αποτέλεσμα:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Μας εξισορρόπηση φόρτου είναι αρκετά κενό, επομένως, ας δημιουργήσουμε ορισμένα σύνολα διευθύνσεων IP. Θέλουμε να δημιουργήσετε δύο σύνολα IP για την εξισορρόπηση φόρτου - μία για το προσκήνιο και μία για παρασκηνίου. Προσκηνίου χώρου συγκέντρωσης IP είναι ορατή στο κοινό. Επίσης, είναι στη θέση στην οποία θα σας να εκχωρήσετε το PIP που δημιουργήσαμε νωρίτερα. Στη συνέχεια, χρησιμοποιούμε το χώρο συγκέντρωσης παρασκηνίου ως μια θέση για το ΣΠΣ για να συνδεθείτε με. Με αυτόν τον τρόπο, η κίνηση να ροής έως τη μονάδα εξισορρόπησης φόρτου για να του ΣΠΣ.

Πρώτα, ας δημιουργήσουμε μας προσκηνίου χώρου συγκέντρωσης διευθύνσεων IP. Το ακόλουθο παράδειγμα δημιουργεί μια προσκηνίου χώρου συγκέντρωσης με το όνομα `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Αποτέλεσμα:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Παρατηρήστε πώς χρησιμοποιήσαμε το `--public-ip-name` αλλαγής για τη μεταβίβαση στο το `myPublicIP` που δημιουργήσαμε νωρίτερα. Εκχώρηση στη δημόσια διεύθυνση IP για την εξισορρόπηση φόρτου σάς επιτρέπει να φτάσετε ΣΠΣ σας μέσω του Internet.

Στη συνέχεια, ας δημιουργήσουμε το δεύτερο σύνολο IP, αυτήν τη φορά για την κίνηση μας παρασκηνίου. Το ακόλουθο παράδειγμα δημιουργεί ένα χώρο συγκέντρωσης παρασκηνίου με το όνομα `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Αποτέλεσμα:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Μπορούμε να δούμε πώς κάνει μας εξισορρόπηση φόρτου, εάν ανατρέξετε με `azure network lb show` και το αποτέλεσμα JSON εξέταση:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Δημιουργία εξισορρόπηση φόρτου NAT κανόνων
Για να λάβετε την κυκλοφορία ρέει μέσω μας εξισορρόπηση φόρτου, πρέπει να δημιουργήσετε διεύθυνση δικτύου κανόνες μετάφραση (NAT) που καθορίζουν είτε εισερχομένων ή εξερχομένων ενέργειες. Μπορείτε να καθορίσετε το πρωτόκολλο για να χρησιμοποιήσετε και έπειτα αντιστοιχίσετε εξωτερικές θύρες στο εσωτερικό θύρες που θέλετε. Για το περιβάλλον, ας δημιουργήσουμε ορισμένοι κανόνες που επιτρέπουν SSH μέσω μας εξισορρόπηση φόρτου για να μας ΣΠΣ. Μπορούμε να εγκαταστήσουμε θύρες TCP 4222 και 4223 για να κατευθύνετε στη θύρα TCP 22 σε μας VM (το οποίο δημιουργούμε αργότερα). Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myLoadBalancerRuleSSH1` για να αντιστοιχίσετε τη θύρα TCP 4222 στη θύρα 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Αποτέλεσμα:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Επαναλάβετε τη διαδικασία για τον δεύτερο κανόνα NAT για SSH. Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myLoadBalancerRuleSSH2` για να αντιστοιχίσετε τη θύρα TCP 4223 στη θύρα 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Ας επίσης προχωρήσετε και δημιουργήστε έναν κανόνα NAT για τη θύρα TCP 80 για την κυκλοφορία web, διάταξη τον κανόνα προς τους χώρους συγκέντρωσης διευθύνσεων IP. Εάν θα σας συνδέσετε τον κανόνα στο χώρο συγκέντρωσης IP, αντί για εγκατάσταση τον κανόνα για να μας ΣΠΣ ξεχωριστά, θα σας να προσθέσετε ή να καταργήσετε ΣΠΣ από το χώρο συγκέντρωσης διευθύνσεων IP. Η μονάδα εξισορρόπησης φόρτου προσαρμόζεται αυτόματα της ροής κυκλοφορίας. Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myLoadBalancerRuleWeb` για να αντιστοιχίσετε τη θύρα TCP 80 στη θύρα 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Αποτέλεσμα:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Δημιουργήστε μια δοκιμή του εύρυθμης λειτουργίας της εξισορρόπησης φόρτου

Μια εύρυθμης λειτουργίας περιοδικά Διερεύνηση ελέγχων του ΣΠΣ που βρίσκονται πίσω από μας εξισορρόπηση φόρτου για να βεβαιωθείτε ότι μπορούν να λειτουργικό και απάντηση σε προσκλήσεις όπως ορίζεται. Εάν όχι, διαγραφούν από τη λειτουργία για να βεβαιωθείτε ότι οι χρήστες δεν γίνεται κατευθυνθεί τους. Μπορείτε να ορίσετε προσαρμοσμένο ελέγχων για τη δοκιμή του εύρυθμης λειτουργίας, μαζί με διαστήματα και τιμές χρονικών ορίων. Για περισσότερες πληροφορίες σχετικά με την εύρυθμη λειτουργία καθετήρες, ανατρέξτε στο θέμα [probes εξισορρόπηση φόρτου](../load-balancer/load-balancer-custom-probe-overview.md). Το ακόλουθο παράδειγμα δημιουργεί μια TCP εύρυθμης λειτουργίας διερευνηθεί επώνυμο `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Αποτέλεσμα:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Εδώ, καθοριστεί ένα χρονικό διάστημα 15 δευτερολέπτων για μας έλεγχοι εύρυθμης λειτουργίας. Θα σας μπορεί να χάσετε έως τέσσερα καθετήρες (ένα λεπτό) πριν από τη μονάδα εξισορρόπησης φόρτου θεωρεί ότι ο κεντρικός υπολογιστής δεν λειτουργεί πλέον.

## <a name="verify-the-load-balancer"></a>Επαληθεύστε τη μονάδα εξισορρόπησης φόρτου
Τώρα ολοκληρώθηκε η ρύθμιση παραμέτρων εξισορρόπησης φόρτου. Ακολουθούν τα βήματα που εκτελέσατε:

1. Πρώτα που δημιουργήσατε μια μονάδα εξισορρόπησης φόρτου.
2. Στη συνέχεια, δημιουργήσατε ένα προσκηνίου χώρο συγκέντρωσης IP και εκχωρηθεί μια δημόσια IP.
3. Έχετε δημιουργήσει ένα χώρο συγκέντρωσης διευθύνσεων IP παρασκηνίου που μπορούν να συνδεθούν ΣΠΣ Επόμενο.
4. Μετά από αυτό, που δημιουργήσατε NAT κανόνες που επιτρέπουν SSH για να του ΣΠΣ για τη διαχείριση, μαζί με έναν κανόνα που επιτρέπει τη θύρα TCP 80 για το web app.
5. Τέλος προσθέσατε μια δοκιμή του εύρυθμης λειτουργίας για να κάνετε περιοδικό έλεγχο του ΣΠΣ. Αυτό δοκιμή εύρυθμης λειτουργίας του εξασφαλίζει ότι οι χρήστες δεν προσπαθούν να αποκτήσουν πρόσβαση σε μια Εικονική που δεν είναι πλέον λειτουργία ή εξυπηρετεί περιεχόμενο.

Ας Διαβάστε πώς σας εξισορρόπηση φόρτου μοιάζει τώρα:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Δημιουργία μιας NIC για να χρησιμοποιήσετε με το Εικονική Linux

NIC διατίθενται μέσω προγραμματισμού επειδή μπορείτε να εφαρμόσετε κανόνες για τη χρήση τους. Μπορείτε επίσης να έχετε περισσότερες από μία. Παρακάτω `azure network nic create` εντολή, συνδέσετε το NIC στο χώρο συγκέντρωσης φόρτωσης παρασκηνίου IP και συσχετίσετε με τον κανόνα NAT για να επιτρέψετε την κυκλοφορία SSH.
 
Αντικαταστήστε το `#####-###-###` ενότητες με το δικό σας αναγνωριστικό Azure συνδρομής. Αναγνωριστικό σημειώνεται στο αποτέλεσμα της τη συνδρομή σας `jq` κατά την εξέταση των πόρων που δημιουργείτε. Μπορείτε επίσης να προβάλετε το Αναγνωριστικό συνδρομής με `azure account list`. 

Το ακόλουθο παράδειγμα δημιουργεί μια NIC με το όνομα `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Αποτέλεσμα:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Μπορείτε να δείτε τις λεπτομέρειες, εξετάζοντας τον πόρο απευθείας. Μπορείτε να εξετάσετε τον πόρο με τη χρήση του `azure network nic show` εντολή:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Αποτέλεσμα:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Τώρα μπορούμε να δημιουργήσουμε το δεύτερο NIC, διάταξη ξανά στο χώρο συγκέντρωσης IP μας παρασκηνίου. Αυτός ο κανόνας NAT η δεύτερη φορά επιτρέπει την κυκλοφορία SSH. Το ακόλουθο παράδειγμα δημιουργεί μια NIC με το όνομα `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Δημιουργήστε μια ομάδα ασφαλείας δικτύου και κανόνων

Τώρα μπορούμε να δημιουργήσουμε μια ομάδα ασφαλείας δικτύου και τους κανόνες εισερχομένων που διέπουν την πρόσβαση στο NIC. Μια ομάδα ασφαλείας δικτύου μπορούν να εφαρμοστούν σε NIC ή υποδικτύου. Μπορείτε να ορίσετε κανόνες για τον έλεγχο της ροής κυκλοφορίας και έξοδος από το ΣΠΣ. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα ασφαλείας δίκτυο με το όνομα `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Ας προσθέσουμε τον κανόνα εισερχομένων για την NSG για να επιτρέψετε εισερχόμενες συνδέσεις στη θύρα 22 (για την υποστήριξη SSH). Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myNetworkSecurityGroupRuleSSH` για να επιτρέψετε τη θύρα TCP 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Τώρα ας προσθέσουμε τον κανόνα εισερχομένων για την NSG για να επιτρέψετε εισερχόμενες συνδέσεις στη θύρα 80 (για να υποστηρίξει κυκλοφορία web). Το ακόλουθο παράδειγμα δημιουργεί έναν κανόνα που ονομάζεται `myNetworkSecurityGroupRuleHTTP` για να επιτρέψετε τη θύρα TCP 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Ο κανόνας εισερχομένων είναι ένα φίλτρο για εισερχόμενες συνδέσεις δικτύου. Σε αυτό το παράδειγμα, θα σας να δεσμεύσετε το NSG στο NIC εικονικού ΣΠΣ, γεγονός που σημαίνει ότι οποιαδήποτε αίτηση στη θύρα 22 περνά στο NIC σε μας Εικονική. Αυτός ο κανόνας εισερχομένων είναι σχετικά με μια σύνδεση δικτύου, και δεν σχετικά με ένα τελικό σημείο, ποια είναι αυτή που θα ήταν σχετικά με σε κλασική αναπτύξεις. Για να ανοίξετε μια θύρα, πρέπει να αφήσετε το `--source-port-range` οριστεί σε '\*' (η προεπιλεγμένη τιμή) για να αποδεχτείτε εισερχόμενες αιτήσεις από **οποιαδήποτε** αίτηση θύρα. Θύρες είναι συνήθως δυναμικές.

## <a name="bind-to-the-nic"></a>Σύνδεση με το NIC

Η σύνδεση του NSG το NIC. Πρέπει να συνδεθείτε μας NIC με την ομάδα ασφαλείας δικτύου. Εκτέλεση εντολών και τα δύο, για να συνδέσετε και τα δύο από τα NIC:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Δημιουργήστε ένα σύνολο διαθεσιμότητα
Διαθεσιμότητα ορίζει Άνοιγμα Βοήθεια σας ΣΠΣ σε τομείς σφαλμάτων και αναβάθμισης τομέων. Ας δημιουργήσουμε μια ρύθμιση για το ΣΠΣ διαθεσιμότητα. Το ακόλουθο παράδειγμα δημιουργεί μια διαθεσιμότητα Ορισμός καθορισμένων `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Σφάλμα τομείς Ορισμός μιας ομαδοποίησης εικονικές μηχανές που έχουν κοινά power προέλευσης και δικτύου διακόπτη. Από προεπιλογή, το εικονικές μηχανές που έχουν ρυθμιστεί το σύνολο διαθεσιμότητα διαχωρίζονται σε έως και τρεις τομείς σφαλμάτων. Η ιδέα είναι ότι ένα ζήτημα υλικού με έναν από αυτούς τους τομείς σφαλμάτων δεν επηρεάζει κάθε Εικονική που εκτελεί την εφαρμογή σας. Azure κατανέμει αυτόματα ΣΠΣ τους τομείς σφαλμάτων κατά την τοποθέτησή τους σε ένα σύνολο διαθεσιμότητα.

Αναβάθμιση τομείς υποδεικνύουν ομάδες εικονικές μηχανές και υποκείμενα φυσικό υλικό που μπορεί να γίνει επανεκκίνηση την ίδια στιγμή. Η σειρά με την οποία είναι επανεκκίνηση αναβάθμισης τομείς ενδέχεται να μην είναι διαδοχική κατά τη διάρκεια της προγραμματισμένης συντήρησης, αλλά μόνο μία αναβάθμιση γίνει επανεκκίνηση κάθε φορά. Ξανά, Azure αυτόματα κατανέμει σας ΣΠΣ αναβάθμισης τομέων κατά την τοποθέτησή τους σε μια τοποθεσία διαθεσιμότητα.

Διαβάστε περισσότερα σχετικά με [τη Διαχείριση τη διαθεσιμότητα των ΣΠΣ](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Δημιουργία του ΣΠΣ Linux

Έχετε δημιουργήσει τους πόρους δικτύου και χώρου αποθήκευσης για την υποστήριξη ΣΠΣ πρόσβαση στο Internet. Τώρα ας τα δημιουργήσετε αυτά ΣΠΣ και να το ασφαλίσετε με έναν αριθμό-κλειδί SSH που δεν έχει έναν κωδικό πρόσβασης. Σε αυτήν την περίπτωση, θα κάνουμε για να δημιουργήσετε μια Εικονική με βάση την πιο πρόσφατη Αποτελεσμάτων Ubuntu. Θα σας εντοπίσετε αυτές τις πληροφορίες εικόνας με τη χρήση `azure vm image list`, όπως περιγράφεται στην [Εύρεση Εικονική Azure εικόνες](virtual-machines-linux-cli-ps-findimage.md).

Θα σας να επιλέξετε μια εικόνα, χρησιμοποιώντας την εντολή `azure vm image list westeurope canonical | grep LTS`. Σε αυτήν την περίπτωση, χρησιμοποιούμε `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Για το τελευταίο πεδίο, θα σας μεταβιβάζουν `latest` ώστε στο μέλλον λαμβάνουμε πάντα την πιο πρόσφατη δομή. (Η συμβολοσειρά χρησιμοποιούμε είναι `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Αυτό το επόμενο βήμα είναι οικεία για οποιοδήποτε άτομο έχει ήδη δημιουργήσει ένα ssh rsa δημόσιο και ιδιωτικό κλειδί σύζευξη σε Linux ή Mac με χρήση **rsa -t ssh keygen-b 2048**. Εάν δεν έχετε οποιαδήποτε ζεύγη κλειδιών πιστοποιητικό σας `~/.ssh` καταλόγου, μπορείτε να τις δημιουργήσετε:

- Αυτόματα, με τη χρήση του `azure vm create --generate-ssh-keys` την επιλογή.
- Με μη αυτόματο τρόπο, χρησιμοποιώντας [τις οδηγίες για να δημιουργήσετε τον εαυτό σας](virtual-machines-linux-mac-create-ssh-keys.md).

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε το `--admin-password` μέθοδο για τον έλεγχο ταυτότητας συνδέσεις σας SSH, αφού δημιουργηθεί η Εικονική. Αυτή η μέθοδος είναι συνήθως λιγότερο ασφαλής.

Μπορούμε να δημιουργήσουμε το Εικονική μεταφέροντας όλους τους πόρους και τις πληροφορίες μαζί με το `azure vm create` εντολή:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Αποτέλεσμα:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Μπορείτε να συνδεθείτε για να σας Εικονική αμέσως με τη χρήση των αριθμών-κλειδιών SSH προεπιλογή. Βεβαιωθείτε ότι μπορείτε να καθορίσετε την κατάλληλη θύρα εφόσον μας που περνά μέσα από τη μονάδα εξισορρόπησης φόρτου. (Για το πρώτο Εικονική, θα σας ρυθμίστε τον κανόνα NAT να προωθήσετε το μήνυμα θύρα 4222 μας Εικονική):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Αποτέλεσμα:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Προχωρήστε και δημιουργήστε το δεύτερο Εικονική με τον ίδιο τρόπο:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Και τώρα μπορείτε να χρησιμοποιήσετε το `azure vm show myResourceGroup myVM1` εντολή για να εξετάσετε τι έχετε δημιουργήσει. Σε αυτό το σημείο, χρησιμοποιείτε το ΣΠΣ Ubuntu πίσω από μια μονάδα εξισορρόπησης φόρτου στο Azure που μπορείτε να πραγματοποιήσετε είσοδο σε μόνο με το ζεύγος κλειδιών SSH (επειδή είναι απενεργοποιημένες οι κωδικοί πρόσβασης). Μπορείτε να εγκαταστήσετε nginx ή httpd, αναπτύξτε μια εφαρμογή web, και δείτε την κίνηση ροής έως τη μονάδα εξισορρόπησης φόρτου στους δύο του ΣΠΣ.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Αποτέλεσμα:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Εξαγωγή του περιβάλλοντος ως προτύπου
Τώρα που έχετε δημιουργήσει εκτός αυτού του περιβάλλοντος, τι θα συμβεί εάν που θέλετε να δημιουργήσετε ένα περιβάλλον ανάπτυξης επιπλέον με τις ίδιες παραμέτρους ή ένα περιβάλλον παραγωγής που ταιριάζουν με αυτό; Διαχείριση πόρων χρησιμοποιεί πρότυπα JSON που ορίζουν όλες τις παραμέτρους για το περιβάλλον σας. Μπορείτε να δημιουργήσετε ολόκληρο περιβάλλοντα με αναφορά σε αυτό το πρότυπο JSON. Μπορείτε να [δημιουργήσετε πρότυπα JSON με μη αυτόματο τρόπο](../resource-group-authoring-templates.md) ή να εξαγάγετε ένα υπάρχον περιβάλλον για τη δημιουργία του προτύπου JSON για εσάς:

```bash
azure group export --name myResourceGroup
```

Αυτή η εντολή δημιουργεί το `myResourceGroup.json` αρχείου σε κατάλογο τρέχουσας εργασίας. Όταν δημιουργείτε ένα περιβάλλον από αυτό το πρότυπο, θα σας ζητηθεί για όλα τα ονόματα πόρων, συμπεριλαμβανομένων των ονομάτων για την εξισορρόπηση φόρτου, διασυνδέσεις δικτύου ή ΣΠΣ. Μπορείτε να συμπληρώσετε αυτά τα ονόματα στο αρχείο σας πρότυπο, προσθέτοντας την `-p` ή `--includeParameterDefaultValue` παράμετρο, για να το `azure group export` εντολή που έχει φαίνεται παραπάνω. Επεξεργασία του προτύπου JSON για να καθορίσετε τα ονόματα των πόρων ή [Δημιουργήστε ένα αρχείο parameters.json](../resource-group-authoring-templates.md#parameters) που καθορίζει τα ονόματα των πόρων.

Για να δημιουργήσετε ένα περιβάλλον από το πρότυπό σας:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Ενδέχεται να θέλετε να διαβάσετε [περισσότερα σχετικά με την ανάπτυξη από τα πρότυπα](../resource-group-template-deploy-cli.md). Μάθετε σχετικά με τον τρόπο βαθμιαία ενημερώσετε περιβάλλοντα, χρησιμοποιήστε το αρχείο παραμέτρων και πρόσβασης σε πρότυπα από μια θέση αποθήκευσης μία.

## <a name="next-steps"></a>Επόμενα βήματα

Τώρα είστε έτοιμοι να ξεκινήσετε να εργάζεστε με πολλά στοιχεία δικτύου και ΣΠΣ. Μπορείτε να χρησιμοποιήσετε αυτό το δείγμα περιβάλλον δημιουργίας την εφαρμογή σας, χρησιμοποιώντας τα βασικά στοιχεία που έχουν εισαχθεί εδώ.

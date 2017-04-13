<properties
   pageTitle="Χρησιμοποιώντας την επέκταση Εικονική Docker Azure | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker να αναπτύσσουν γρήγορα και με ασφάλεια ένα περιβάλλον Docker στο Azure με τη χρήση προτύπων από διαχειριστή πόρων."
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
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Δημιουργία περιβάλλοντος Docker στο Azure χρησιμοποιώντας την επέκταση Εικονική Docker
Docker είναι ένα κοντέινερ δημοφιλείς διαχείρισης και απεικόνισης πλατφόρμα που σας επιτρέπει να εργάζεστε γρήγορα με το κοντέινερ Linux (και καθώς και Windows). Στο Azure, υπάρχουν διάφοροι τρόποι που μπορείτε να αναπτύξετε Docker σύμφωνα με τις ανάγκες σας. Σε αυτό το άρθρο εστιάζει χρησιμοποιώντας την επέκταση Εικονική Docker και πρότυπα διαχείρισης πόρων Azure. 

Για περισσότερες πληροφορίες σχετικά με τις διάφορες μεθόδους ανάπτυξης, συμπεριλαμβανομένης της χρήσης Docker υπολογιστή και των υπηρεσιών Azure κοντέινερ, ανατρέξτε στα ακόλουθα άρθρα:

- Για γρήγορη πρωτοτύπου μια εφαρμογή, μπορείτε να δημιουργήσετε έναν μεμονωμένο κεντρικό υπολογιστή Docker χρησιμοποιώντας [Docker υπολογιστή](./virtual-machines-linux-docker-machine.md).
- Για περιβάλλοντα μεγαλύτερο, πιο σταθερή, μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker Azure, η οποία υποστηρίζει επίσης [Σύνθεση Docker](https://docs.docker.com/compose/overview/) για τη δημιουργία αναπτύξεις συνεπή κοντέινερ. Σε αυτό το άρθρο λεπτομέρειες χρησιμοποιώντας την επέκταση Εικονική Docker Azure.
- Για να δημιουργήσετε ετοιμότητα παραγωγής, με περιβάλλοντα που παρέχουν πρόσθετες εργαλείων διαχείρισης και τον προγραμματισμό, μπορείτε να αναπτύξετε ένα [σύμπλεγμα Docker Swarm σχετικά με τις υπηρεσίες κοντέινερ Azure](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Επισκόπηση επέκταση Azure Εικονική Docker
Η επέκταση Εικονική Docker Azure εγκαθιστά και ρυθμίζει τις παραμέτρους του μονού Docker Docker προγράμματος-πελάτη και σύνθεση Docker στον υπολογιστή σας εικονικές Linux (Εικονική). Χρησιμοποιώντας την επέκταση Εικονική Docker Azure, έχετε περισσότερες ελέγχου και δυνατότητες από απλώς χρησιμοποιώντας Docker υπολογιστή ή τη δημιουργία του κεντρικού υπολογιστή Docker μόνοι σας. Αυτές οι πρόσθετες δυνατότητες, όπως [Docker σύνθεση](https://docs.docker.com/compose/overview/), κάντε την επέκταση Εικονική Docker Azure κατάλληλοι για έναν πιο ισχυρό προγραμματιστής ή περιβάλλοντα παραγωγής.

Azure πρότυπα διαχείρισης πόρων Ορισμός ολόκληρη τη δομή του περιβάλλοντός σας. Πρότυπα σάς επιτρέπουν να δημιουργήσετε και να ρυθμίσετε τους πόρους, όπως τον κεντρικό υπολογιστή Docker ΣΠΣ, χώρος αποθήκευσης, ελέγχου πρόσβασης βάσει ρόλων (RBAC) και διαγνωστικά. Μπορείτε να χρησιμοποιήσετε ξανά αυτά τα πρότυπα για να δημιουργήσετε πρόσθετες αναπτύξεις με συνεπή τρόπο. Για περισσότερες πληροφορίες σχετικά με τη διαχείριση πόρων Azure και προτύπων, ανατρέξτε στο θέμα [Επισκόπηση της διαχείρισης πόρων](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Ανάπτυξη προτύπου με την επέκταση Εικονική Docker Azure
Ας χρησιμοποιήσουμε ένα υπάρχον πρότυπο γρήγορης έναρξης για να δημιουργήσετε μια Εικονική Ubuntu που χρησιμοποιεί την επέκταση Εικονική Docker Azure να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του κεντρικού υπολογιστή Docker. Μπορείτε να προβάλετε το πρότυπο εδώ: [Απλή υλοποίηση της Εικονική μηχανή Ubuntu με Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Χρειάζεστε την [Πιο πρόσφατη CLI Azure](../xplat-cli-install.md) εγκαταστήσει και συνδεθείτε χρησιμοποιώντας τη λειτουργία διαχείρισης πόρων ως εξής:

```
azure config mode arm
```

Ανάπτυξη του προτύπου χρησιμοποιώντας το Azure CLI, που καθορίζει το πρότυπο URI. Το ακόλουθο παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` θέση. Χρησιμοποιήστε το δικό σας όνομα ομάδας πόρων και τη θέση ως εξής:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Απαντήστε τις οδηγίες για να ονομάσετε το λογαριασμό χώρου αποθήκευσης, δώστε ένα όνομα χρήστη και τον κωδικό πρόσβασης και δώστε ένα όνομα DNS. Το αποτέλεσμα θα είναι παρόμοιο με το ακόλουθο παράδειγμα:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Η Azure CLI επιστρέφει στην ερώτηση μετά την μερικά μόνο δευτερόλεπτα, αλλά ο κεντρικός υπολογιστής Docker εξακολουθείτε να δημιουργηθεί και έχει ρυθμιστεί από την επέκταση Εικονική Docker Azure. Χρειάζονται μερικά λεπτά για την ανάπτυξη για να ολοκληρώσετε. Μπορείτε να προβάλετε λεπτομέρειες σχετικά με τη χρήση του κατάσταση Docker κεντρικού υπολογιστή του `azure vm show` εντολή.

Το παρακάτω παράδειγμα ελέγχει την κατάσταση της την εικονική Μηχανή με το όνομα `myDockerVM` (το προεπιλεγμένο όνομα από το πρότυπο - μην αλλάξετε το όνομα αυτό) στην ομάδα πόρων με το όνομα `myResourceGroup`. Πληκτρολογήστε το όνομα της ομάδας πόρων που δημιουργήσατε στο προηγούμενο βήμα:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Το αποτέλεσμα του `azure vm show` εντολή είναι παρόμοιο με το ακόλουθο παράδειγμα:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Κοντά στο επάνω μέρος το αποτέλεσμα, μπορείτε να δείτε το `ProvisioningState` από την εικονική Μηχανή. Όταν αυτή εμφανίζει `Succeeded`, την ανάπτυξη έχει ολοκληρωθεί και μπορείτε να SSH για την εικονική Μηχανή.

Προς το τέλος του αποτελέσματος, `FQDN` εμφανίζει το πλήρως προσδιορισμένο όνομα τομέα σας Docker κεντρικού υπολογιστή. Σε αυτό το FQDN είναι χρησιμοποιούνται για τη SSH τον κεντρικό υπολογιστή Docker στο τα υπόλοιπα βήματα.


## <a name="deploy-your-first-nginx-container"></a>Αναπτύξτε το πρώτο κοντέινερ nginx
Μόλις ανάπτυξης ολοκληρωθεί, SSH για το νέο host Docker από τον τοπικό υπολογιστή. Πληκτρολογήστε το δικό σας όνομα χρήστη και FQDN ως εξής:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Όταν είστε συνδεδεμένοι με τον κεντρικό υπολογιστή του Docker, ας εκτελέστε ένα κοντέινερ nginx:

```
sudo docker run -d -p 80:80 nginx
```

Το αποτέλεσμα είναι παρόμοιο με το ακόλουθο παράδειγμα όπως η εικόνα nginx γίνεται λήψη και να ξεκινήσετε ένα κοντέινερ:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Έλεγχος της κατάστασης των κοντέινερ εκτελείται στην υπηρεσία παροχής φιλοξενίας Docker ως εξής:

```
sudo docker ps
```

Το αποτέλεσμα είναι παρόμοιο με το παρακάτω παράδειγμα, που δείχνει ότι το κοντέινερ nginx εκτελείται και θύρες TCP 80 και 443 και γίνεται προώθηση:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Για να δείτε το κοντέινερ στην πράξη, ανοίξτε ένα πρόγραμμα περιήγησης web και πληκτρολογήστε το όνομα FQDN σας Docker κεντρικού υπολογιστή:

![Εκτελείται ngnix κοντέινερ](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Εικονική Docker επέκταση πρότυπο αναφοράς
Το προηγούμενο παράδειγμα χρησιμοποιεί ένα υπάρχον πρότυπο γρήγορη έναρξη. Μπορείτε επίσης να αναπτύξετε την επέκταση Εικονική Docker Azure με τα δικά σας πρότυπα διαχείρισης πόρων. Για να το κάνετε αυτό, προσθέστε τα ακόλουθα σας πρότυπα για τη διαχείριση πόρων, τον ορισμό του `vmName` του σας Εικονική σωστά:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Μπορείτε να βρείτε πιο λεπτομερείς οδηγίες σχετικά με τη χρήση προτύπων από διαχειριστή πόρων διαβάζοντας [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Επόμενα βήματα
Ενδέχεται να θέλετε να [ρυθμίσετε τις παραμέτρους του μονού Docker θύρα TCP](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), Κατανόηση [Docker ασφαλείας](https://docs.docker.com/engine/security/security/)ή ανάπτυξη κοντέινερ χρησιμοποιώντας [Docker σύνθεση](https://docs.docker.com/compose/overview/). Για περισσότερες πληροφορίες σχετικά με την επέκταση Εικονική Docker Azure ίδια, ανατρέξτε στο θέμα [GitHub έργου](https://github.com/Azure/azure-docker-extension/).

Διαβάστε περισσότερες πληροφορίες σχετικά με τις πρόσθετες επιλογές ανάπτυξης Docker στο Azure:

- [Χρήση Docker υπολογιστή με το πρόγραμμα οδήγησης του Azure](./virtual-machines-linux-docker-machine.md)  
- [Γρήγορα αποτελέσματα με το Docker και σύνθεση να ορίσετε και να εκτελέσετε μια εφαρμογή πολλών κοντέινερ σε μια εικονική μηχανή Azure](virtual-machines-linux-docker-compose-quickstart.md).
- [Ανάπτυξη ένα σύμπλεγμα Azure κοντέινερ υπηρεσίας](../container-service/container-service-deployment.md)

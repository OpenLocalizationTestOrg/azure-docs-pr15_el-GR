<properties
    pageTitle="Δημιουργία Docker hosts στο Azure με μηχανική Docker | Microsoft Azure"
    description="Περιγράφει χρήση Docker υπολογιστή για να δημιουργήσετε docker hosts στο Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Χρήση Docker υπολογιστή με το πρόγραμμα οδήγησης του Azure

[Docker](https://www.docker.com/) είναι μία από τις πιο δημοφιλείς μεθόδους Application virtualization που χρησιμοποιεί Linux κοντέινερ και όχι σε εικονικές μηχανές έτσι ώστε να απομόνωση δεδομένα εφαρμογής και την υπολογιστική σε κοινόχρηστους πόρους. Αυτό το θέμα περιγράφει πότε και πώς μπορείτε να χρησιμοποιήσετε [Τον υπολογιστή Docker](https://docs.docker.com/machine/) (το `docker-machine` εντολή) για να δημιουργήσετε νέα ΣΠΣ Linux στο Azure με δυνατότητα ως κεντρικός υπολογιστής docker για τους χώρους Linux.


## <a name="create-vms-with-docker-machine"></a>Δημιουργία ΣΠΣ με Docker υπολογιστή

Δημιουργία docker ΣΠΣ κεντρικού υπολογιστή στο Azure με το `docker-machine create` εντολή χρησιμοποιώντας το `azure` όρισμα πρόγραμμα οδήγησης για την επιλογή προγράμματος οδήγησης (`-d`) και άλλα ορίσματα. 

Το παρακάτω παράδειγμα βασίζεται στις τις προεπιλεγμένες τιμές, αλλά το ανοίξετε θύρα 80 την εικονική Μηχανή στο Internet για να ελέγξετε με ένα κοντέινερ nginx, κάνει `ops` ο χρήστης σύνδεσης για SSH και κλήσεις τη νέα Εικονική `machine`. 

Τύπος `docker-machine create --driver azure` για να δείτε τις επιλογές και τις προεπιλεγμένες τιμές. Μπορείτε επίσης να διαβάσετε την [τεκμηρίωση του προγράμματος οδήγησης Azure Docker](https://docs.docker.com/machine/drivers/azure/). (Σημειώστε ότι αν έχετε έλεγχο ταυτότητας δύο παραγόντων με δυνατότητα, θα σας ζητηθεί για τον έλεγχο ταυτότητας χρησιμοποιώντας το δεύτερο παράγοντα.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Το αποτέλεσμα θα πρέπει να μοιάζει με αυτό, ανάλογα με το εάν έχετε τον έλεγχο ταυτότητας δύο παραγόντων που έχει ρυθμιστεί στο λογαριασμό σας.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Ρύθμιση παραμέτρων του κελύφους docker

Τώρα, πληκτρολογήστε `docker-machine env <VM name>` για να δείτε τι πρέπει να κάνετε για να ρυθμίσετε τις παραμέτρους του κελύφους. 

```bash
docker-machine env machine
```

Που εκτυπώνει τις πληροφορίες περιβάλλοντος, η οποία μοιάζει κάπως έτσι. Σημειώστε τη διεύθυνση IP έχει εκχωρηθεί, που θα πρέπει να ελέγξετε την εικονική Μηχανή.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Μπορείτε να εκτελέσετε την εντολή προτεινόμενα ρύθμισης παραμέτρων, ή μπορείτε να ορίσετε τις μεταβλητές περιβάλλοντος στον εαυτό σας. 

## <a name="run-a-container"></a>Εκτέλεση ενός κοντέινερ

Τώρα μπορείτε να εκτελέσετε σε διακομιστή web απλό για να ελέγξετε εάν όλα λειτουργούν σωστά. Εδώ θα σας Χρησιμοποιήστε μια εικόνα τυπική nginx, να καθορίσετε ότι αυτό θα πρέπει να κάνουν ακρόαση σε θύρες 80 και ότι εάν η Εικονική επανεκκίνηση του κοντέινερ θα πρέπει να επανεκκινήσετε καθώς και (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Το αποτέλεσμα θα πρέπει να μοιάζει με τα εξής:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Δοκιμή του κοντέινερ

Εξετάστε εκτελείται κοντέινερ χρησιμοποιώντας `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Και ελέγξτε για να δείτε το τρέχον κοντέινερ, πληκτρολογήστε `docker-machine ip <VM name>` για να βρείτε τη διεύθυνση IP (Εάν έχετε ξεχάσει από το `env` εντολή):

![Εκτελείται ngnix κοντέινερ](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Επόμενα βήματα

Εάν σας ενδιαφέρει, μπορείτε να δοκιμάσετε την Azure [Επέκταση Εικονική Docker](virtual-machines-linux-dockerextension.md) για να κάνετε την ίδια λειτουργία χρησιμοποιώντας το CLI Azure ή πρότυπα διαχείρισης πόρων Azure. 

Για περισσότερα παραδείγματα της εργασίας με Docker, ανατρέξτε στο θέμα [εργασία με Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) από τη σύνδεση [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [επίδειξη](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Για περισσότερες γρήγορες εκκινήσεις από την επίδειξη HealthClinic.biz, ανατρέξτε στο θέμα [Γρήγορες εκκινήσεις εργαλεία για προγραμματιστές Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).


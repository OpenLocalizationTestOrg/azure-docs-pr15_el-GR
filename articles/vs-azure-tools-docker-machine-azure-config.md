<properties
   pageTitle="Δημιουργία Docker hosts στο Azure με μηχανική Docker | Microsoft Azure"
   description="Περιγράφει χρήση Docker υπολογιστή για να δημιουργήσετε docker hosts στο Azure."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Δημιουργία Docker Hosts στο Azure με Docker-μηχανικής

Εκτέλεση κοντέινερ [Docker](https://www.docker.com/) απαιτεί μια Εικονική εκτελείται το μονού docker υπηρεσία παροχής φιλοξενίας.
Αυτό το θέμα περιγράφει πώς μπορείτε να χρησιμοποιήσετε την εντολή [docker υπολογιστή](https://docs.docker.com/machine/) για να δημιουργήσετε νέα ΣΠΣ Linux, έχουν ρυθμιστεί με το μονού Docker, εκτελείται στο Azure. 

**Σημείωση:** 
- *Σε αυτό το άρθρο εξαρτάται από την έκδοση υπολογιστή docker 0.7.0 ή μεγαλύτερο*
- *Κοντέινερ Windows θα υποστηρίζεται μέσω υπολογιστή docker στο άμεσο μέλλον*

## <a name="create-vms-with-docker-machine"></a>Δημιουργία ΣΠΣ με Docker υπολογιστή

Δημιουργία docker ΣΠΣ κεντρικού υπολογιστή στο Azure με το `docker-machine create` εντολή χρησιμοποιώντας το `azure` πρόγραμμα οδήγησης. 

Το πρόγραμμα οδήγησης Azure θα χρειαστεί το αναγνωριστικό συνδρομής. Μπορείτε να χρησιμοποιήσετε το [Azure CLI](xplat-cli-install.md) ή την [Πύλη του Azure](https://portal.azure.com) για να ανακτήσετε τη συνδρομή σας στο Azure. 

**Με την πύλη Azure**
- Επιλέξτε συνδρομές από τη σελίδα αριστερό παράθυρο περιήγησης και αντιγραφή σε αναγνωριστικό συνδρομής.

**Χρήση του Azure CLI**
- Τύπος ```azure account list``` και αντιγράψτε το αναγνωριστικό συνδρομής.

Τύπος `docker-machine create --driver azure` για να δείτε τις επιλογές και τις προεπιλεγμένες τιμές.
Μπορείτε επίσης να δείτε την [τεκμηρίωση του προγράμματος οδήγησης Azure Docker](https://docs.docker.com/machine/drivers/azure/) για περισσότερες πληροφορίες. 

Το παρακάτω παράδειγμα βασίζεται στις τις προεπιλεγμένες τιμές, αλλά το ανοίξετε προαιρετικά θύρα 80 σε η Εικονική για πρόσβαση στο internet. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Επιλέξτε μια υπηρεσία παροχής φιλοξενίας docker με docker-μηχανικής
Όταν έχετε μια καταχώρηση σε docker μηχανή για τον κεντρικό υπολογιστή, μπορείτε να ορίσετε το προεπιλεγμένο κεντρικό κατά την εκτέλεση εντολών docker.
##<a name="using-powershell"></a>Χρήση του PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Χρήση πάρτι

```bash
eval $(docker-machine env MyDockerHost)
```

Τώρα μπορείτε να εκτελέσετε εντολές docker σε σχέση με το καθορισμένο κεντρικό υπολογιστή

```
docker ps
docker info
```

## <a name="run-a-container"></a>Εκτέλεση ενός κοντέινερ

Με έναν κεντρικό υπολογιστή που έχει ρυθμιστεί, τώρα μπορείτε να εκτελέσετε σε διακομιστή web απλό για να ελέγξετε αν ο κεντρικός υπολογιστής σας έχει ρυθμιστεί σωστά.
Εδώ θα σας Χρησιμοποιήστε μια εικόνα τυπική nginx, να καθορίσετε ότι αυτό θα πρέπει να κάνουν ακρόαση σε θύρες 80 και ότι, εάν ο κεντρικός υπολογιστής Εικονική επανεκκίνηση του, θα το κοντέινερ επανεκκίνηση καθώς και (`--restart=always`). 

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

Και ελέγξτε για να δείτε το τρέχον κοντέινερ, πληκτρολογήστε `docker-machine ip <VM name>` για να βρείτε τη διεύθυνση IP για την εισαγωγή στο πρόγραμμα περιήγησης:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Εκτελείται ngnix κοντέινερ](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Σύνοψη
Με docker υπολογιστή που μπορεί να παροχή εύκολα hosts docker στο Azure για το σας μεμονωμένα docker επικυρώσεων κεντρικού υπολογιστή.
Για τη φιλοξενία παραγωγής κοντέινερ, ανατρέξτε στο θέμα η [Υπηρεσία κοντέινερ Azure](http://aka.ms/AzureContainerService)

Για να αναπτύξετε εφαρμογές πυρήνα .NET με το Visual Studio, ανατρέξτε στο θέμα [Docker εργαλεία για το Visual Studio](http://aka.ms/DockerToolsForVS)

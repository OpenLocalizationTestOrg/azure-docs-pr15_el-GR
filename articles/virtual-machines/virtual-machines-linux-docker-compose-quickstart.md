<properties
   pageTitle="Docker και σύνθεση σε εικονικό μηχάνημα | Microsoft Azure"
   description="Γρήγορη εισαγωγή στην εργασία με σύνθεση και Docker σε εικονικές μηχανές Linux στο Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Γρήγορα αποτελέσματα με το Docker και σύνθεση για να ορίσετε και να εκτελέσετε μια εφαρμογή πολλών κοντέινερ σε μια εικονική μηχανή Azure

Γρήγορα αποτελέσματα χρησιμοποιώντας Docker και [σύνθεση](http://github.com/docker/compose) να ορίσετε και να εκτελέσετε μια σύνθετη εφαρμογή σε μια εικονική μηχανή Linux στο Azure. Με σύνθεση, μπορείτε να χρησιμοποιήσετε ένα αρχείο απλού κειμένου για να ορίσετε μια εφαρμογή που αποτελείται από πολλά Docker κοντέινερ. Στη συνέχεια, αυξομείωσης του την εφαρμογή σας σε μια μεμονωμένη εντολή που κάνει όλα τα στοιχεία για να αναπτύξετε το καθορισμένο περιβάλλον. 

Ως παράδειγμα, αυτό το άρθρο παρουσιάζει πώς μπορείτε να ρυθμίσετε γρήγορα ένα ιστολόγιο WordPress με μια βάση δεδομένων MariaDB SQL σε μια Εικονική Ubuntu παρασκηνίου. Μπορείτε επίσης να χρησιμοποιήσετε σύνθεση για να ορίσετε πιο σύνθετες εφαρμογές.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Βήμα 1: Ρύθμιση μια Εικονική Linux ως Docker κεντρικός υπολογιστής

Μπορείτε να χρησιμοποιήσετε διάφορες Azure διαδικασίες και διαθέσιμες εικόνες ή πρότυπα διαχείρισης πόρων από το Azure Marketplace για να δημιουργήσετε μια Εικονική Linux και να ρυθμίσετε ως κεντρικός υπολογιστής Docker. Για παράδειγμα, ανατρέξτε στο θέμα [χρησιμοποιώντας την επέκταση Εικονική Docker για να αναπτύξετε το περιβάλλον σας](virtual-machines-linux-dockerextension.md) για να δημιουργήσετε γρήγορα μια Εικονική Ubuntu με την επέκταση Εικονική Docker Azure χρησιμοποιώντας ένα [πρότυπο γρήγορη έναρξη](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Όταν χρησιμοποιείτε την επέκταση Εικονική Docker, Εικονική σας είναι να ρυθμίσει αυτόματα το ως κεντρικός υπολογιστής Docker και σύνθεση είναι ήδη εγκατεστημένο. Το παράδειγμα σε αυτό το άρθρο δείχνει πώς μπορείτε να χρησιμοποιήσετε το [Azure περιβάλλον γραμμής εντολών για Mac, Linux, και Windows](../xplat-cli-install.md) (το Azure CLI) σε κατάσταση λειτουργίας διαχειριστή πόρων για να δημιουργήσετε την εικονική Μηχανή.

Η βασική εντολή από το προηγούμενο έγγραφο δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `West US` θέση και την ανάπτυξη μια Εικονική με την επέκταση Εικονική Docker Azure εγκατεστημένο:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Βήμα 2: Βεβαιωθείτε ότι έχει εγκατασταθεί σύνθεση

Μόλις ολοκληρωθεί η ανάπτυξη, SSH για το νέο Docker στον κεντρικό υπολογιστή χρησιμοποιώντας το DNS όνομα που παρέχονται κατά την ανάπτυξη. Μπορείτε να χρησιμοποιήσετε `azure vm show -g myDockerResourceGroup -n myDockerVM` για να προβάλετε λεπτομέρειες Εικονική σας, συμπεριλαμβανομένου του ονόματος του DNS.

Για να ελέγξετε ότι σύνθεση είναι εγκατεστημένη στον η Εικονική, εκτελέστε την ακόλουθη εντολή:

```bash
docker-compose --version
```

Δείτε έξοδο παρόμοια με `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Εάν χρησιμοποιήσατε μια άλλη μέθοδος για να δημιουργήσετε μια υπηρεσία παροχής φιλοξενίας Docker και πρέπει να εγκαταστήσετε σύνθεση στον εαυτό σας, ανατρέξτε στην [τεκμηρίωση σύνθεση](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Βήμα 3: Δημιουργία ενός αρχείου ρύθμισης παραμέτρων docker compose.yml

Στη συνέχεια δημιουργείτε μια `docker-compose.yml` αρχείο, το οποίο είναι απλώς ένα κείμενο αρχείο ρύθμισης παραμέτρων, για να ορίσετε το κοντέινερ Docker να εκτελέσετε στο η Εικονική. Το αρχείο Καθορίζει την εικόνα που θα εκτελείται σε κάθε κοντέινερ (ή μπορεί να είναι μια Δόμηση από μια Dockerfile), μεταβλητές περιβάλλοντος είναι απαραίτητο και εξαρτήσεις, τις θύρες και οι συνδέσεις μεταξύ του κοντέινερ. Για λεπτομέρειες σχετικά με σύνταξη για το αρχείο yml, ανατρέξτε στο θέμα [αναφορά αρχείου σύνθεση](http://docs.docker.com/compose/yml/).

Δημιουργία του `docker-compose.yml` αρχείου ως εξής:

```bash
touch docker-compose.yml
```

Χρησιμοποιήστε το πρόγραμμα επεξεργασίας κειμένου αγαπημένων για να προσθέσετε ορισμένα δεδομένα στο αρχείο. Το ακόλουθο παράδειγμα χρησιμοποιεί το `vi` επεξεργασίας:

```bash
vi docker-compose.yml
```

Επικολλήστε το παρακάτω παράδειγμα στο αρχείο κειμένου σας. Αυτή η ρύθμιση παραμέτρων χρησιμοποιεί εικόνες από το [Μητρώο DockerHub](https://registry.hub.docker.com/_/wordpress/) για την εγκατάσταση WordPress (το άνοιγμα αρχείου προέλευσης δημιουργία ιστολογίου και το περιεχόμενο σύστημα διαχείρισης) και ένα συνδεδεμένο στοιχείου διακομιστή βάσης δεδομένων MariaDB SQL. Εισαγάγετε τις δικές σας `MYSQL_ROOT_PASSWORD` ως εξής:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Βήμα 4: Έναρξη τα κοντέινερ με σύνθεση

Στον ίδιο κατάλογο ως σας `docker-compose.yml` αρχείο, εκτελέστε την ακόλουθη εντολή (ανάλογα με το περιβάλλον σας, ίσως χρειαστεί να εκτελέσετε `docker-compose` χρησιμοποιώντας `sudo`.):

```bash
docker-compose up -d

```

Αυτή η εντολή ξεκινά το κοντέινερ Docker που καθορίζεται στο `docker-compose.yml`. Χρειάζονται ένα ή δύο για να ολοκληρώσετε αυτό το βήμα λεπτά. Δείτε εξόδου παρόμοιο με το ακόλουθο παράδειγμα:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Φροντίστε να χρησιμοποιήσετε την επιλογή **-δ** σε εκκίνησης ώστε να εκτελείται στο παρασκήνιο το κοντέινερ συνεχώς.

Για να βεβαιωθείτε ότι τα κοντέινερ είναι προς τα επάνω, πληκτρολογήστε `docker-compose ps`. Θα πρέπει να δείτε κάτι παρόμοιο με:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Τώρα μπορείτε να συνδεθείτε με WordPress απευθείας σε η Εικονική στη θύρα 80. Ανοίξτε ένα πρόγραμμα περιήγησης web και πληκτρολογήστε το όνομα DNS σας Εικονική (όπως είναι οι `http://myresourcegroup.westus.cloudapp.azure.com`). Τώρα θα πρέπει να βλέπετε το WordPress οθόνη έναρξης, όπου μπορείτε να ολοκληρώσετε την εγκατάσταση και γρήγορα αποτελέσματα με την εφαρμογή.

![Οθόνη έναρξης WordPress][wordpress_start]


## <a name="next-steps"></a>Επόμενα βήματα

* Μετάβαση στον [Οδηγό χρήσης επέκταση Docker Εικονική](https://github.com/Azure/azure-docker-extension/blob/master/README.md) για περισσότερες επιλογές για τη ρύθμιση παραμέτρων Docker και σύνθεση σε σας Εικονική Docker. Για παράδειγμα, μία επιλογή είναι να τοποθετήσει το αρχείο yml σύνθεση (μετατρέπονται σε JSON) απευθείας στη ρύθμιση παραμέτρων της επέκτασης Εικονική Docker.
* Δείτε τη [σύνθεση τη γραμμή εντολών](http://docs.docker.com/compose/reference/) και [Οδηγός](http://docs.docker.com/compose/) για περισσότερα παραδείγματα δημιουργία και ανάπτυξη εφαρμογών πολλών κοντέινερ.
* Χρήση ενός προτύπου για τη διαχείριση πόρων Azure, είτε το δικό ή μία συνεισφέρει από την [Κοινότητα](https://azure.microsoft.com/documentation/templates/), για να αναπτύξετε μια Εικονική Azure με Docker και μια εφαρμογή με το σύνθεση. Για παράδειγμα, το πρότυπο [Ανάπτυξη WordPress ιστολογίου με Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) χρησιμοποιεί Docker και σύνθεση γρήγορης ανάπτυξης WordPress με έναν υπολογιστή στο παρασκήνιο MySQL σε μια Εικονική Ubuntu.
* Δοκιμάστε ενοποίηση Docker σύνθεση με ένα σύμπλεγμα [Docker Swarm](virtual-machines-linux-docker-swarm.md) . Για σενάρια, ανατρέξτε στο θέμα [Χρήση σύνθεση με Swarm](https://docs.docker.com/compose/swarm/) .

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png

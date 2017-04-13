<properties
    pageTitle="Χρησιμοποιώντας την επέκταση Εικονική Docker για Linux στο Azure"
    description="Περιγράφει Docker και τις επεκτάσεις εικονικές μηχανές Windows Azure και δείχνει πώς μπορείτε να δημιουργήσετε μέσω προγραμματισμού εικονικές μηχανές σε Azure που είναι hosts docker από τη γραμμή εντολών χρησιμοποιώντας το Azure CLI."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Χρησιμοποιώντας την επέκταση Εικονική Docker από το Azure γραμμής εντολών περιβάλλοντος εργασίας (Azure CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Αυτό το θέμα περιγράφει πώς μπορείτε να δημιουργήσετε μια Εικονική με την επέκταση Εικονική Docker από την υπηρεσία διαχείρισης (asm) κατάσταση λειτουργίας στο Azure CLI σε οποιαδήποτε πλατφόρμα. [Docker](https://www.docker.com/) είναι μία από τις πιο δημοφιλείς μεθόδους Application virtualization που χρησιμοποιεί [Linux κοντέινερ](http://en.wikipedia.org/wiki/LXC) και όχι σε εικονικές μηχανές έτσι ώστε να απομόνωση δεδομένων και την υπολογιστική σε κοινόχρηστους πόρους. Μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker και τον [Παράγοντα Linux Azure](virtual-machines-linux-agent-user-guide.md) για να δημιουργήσετε μια Εικονική Docker που φιλοξενεί οποιονδήποτε αριθμό κοντέινερ για τις εφαρμογές στο Azure. Για να δείτε μια συζήτηση υψηλού επιπέδου του κοντέινερ και τα πλεονεκτήματα, ανατρέξτε στο θέμα [Docker υψηλού επιπέδου πίνακα](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker με Azure
Για να χρησιμοποιήσετε την επέκταση Εικονική Docker με Azure, πρέπει να εγκαταστήσετε μια έκδοση του [Azure γραμμής εντολών περιβάλλοντος εργασίας](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) υψηλότερα από 0.8.6 (όπως παρόν την τρέχουσα έκδοση είναι 0.10.0). Μπορείτε να εγκαταστήσετε το Azure CLI σε Mac, Linux και Windows.


Η ολοκλήρωση διαδικασία για τη χρήση Docker σε Azure είναι απλή:

+ Εγκαταστήστε το CLI Azure και τις εξαρτήσεις στον υπολογιστή από την οποία θέλετε να ελέγξετε Azure (στα Windows, αυτό θα είναι μια κατανομή Linux εκτελείται ως μια εικονική μηχανή)
+ Χρησιμοποιήστε τις εντολές Azure CLI Docker για να δημιουργήσετε μια υπηρεσία παροχής φιλοξενίας Εικονική Docker στο Azure
+ Χρησιμοποιήστε τις εντολές της τοπικής Docker για να διαχειριστείτε τους χώρους Docker σε σας Εικονική Docker στο Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Εγκαταστήστε το Azure περιβάλλον γραμμής εντολών (Azure CLI)

Για να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους του Azure CLI, ανατρέξτε στο θέμα [Πώς να εγκαταστήσετε το περιβάλλον γραμμής εντολών του Azure](../xplat-cli-install.md). Για να επιβεβαιώσετε την εγκατάσταση, πληκτρολογήστε `azure` στη γραμμή εντολών και έπειτα από λίγο σύντομα θα πρέπει να βλέπετε το καλλιτεχνικό Azure CLI ASCII, που παραθέτει τις βασικές εντολές που είναι διαθέσιμες σε εσάς. Εάν η εγκατάσταση λειτουργούσε σωστά, θα πρέπει να πληκτρολογήσετε `azure help vm` και δείτε ότι μία από τις εντολές που αναγράφονται είναι "docker".

> [AZURE.NOTE] Docker περιλαμβάνει εργαλεία για Windows, [Docker υπολογιστή](https://docs.docker.com/installation/windows/), και μπορείτε επίσης να χρησιμοποιήσετε για να αυτοματοποιήσετε τη δημιουργία ενός πελάτη docker που μπορείτε να χρησιμοποιήσετε για να εργαστείτε με ΣΠΣ Azure ως docker hosts.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Σύνδεση του Azure CLI για λογαριασμό Azure
Για να χρησιμοποιήσετε το Azure CLI πρέπει να συσχετίσετε τα διαπιστευτήριά σας λογαριασμός Azure με το Azure CLI στην πλατφόρμα σας. Στην ενότητα [πώς μπορείτε να συνδεθείτε με τη συνδρομή σας στο Azure](../xplat-cli-connect.md) εξηγεί πώς μπορείτε να κάνετε λήψη και εισαγάγετε το αρχείο **.publishsettings** ή συσχετίσετε σας CLI Azure με εταιρικό αναγνωριστικό.

> [AZURE.NOTE] Υπάρχουν ορισμένες διαφορές συμπεριφοράς όταν χρησιμοποιείτε ένα ή άλλων μεθόδων ελέγχου ταυτότητας, ώστε να μην παραλείψετε να διαβάσετε το έγγραφο παραπάνω για να κατανοήσετε τη λειτουργικότητα διαφορετικό.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Εγκαταστήστε Docker και χρησιμοποιήστε την επέκταση Εικονική Docker για Azure
Ακολουθήστε τις [οδηγίες εγκατάστασης Docker](https://docs.docker.com/installation/#installation) για να εγκαταστήσετε Docker τοπικά στον υπολογιστή σας.

Για να χρησιμοποιήσετε Docker με μια Azure εικονική μηχανή, η εικόνα Linux που χρησιμοποιείται για την εικονική Μηχανή πρέπει να έχετε εγκαταστήσει τον [Παράγοντα Εικονική Linux Azure](virtual-machines-linux-agent-user-guide.md) . Προς το παρόν, υπάρχουν μόνο δύο τύποι των εικόνων που παρέχουν αυτό:

+ Μια εικόνα Ubuntu από τη συλλογή εικόνων Azure ή

+ Μια προσαρμοσμένη εικόνα Linux που έχετε δημιουργήσει με τον παράγοντα Azure Linux Εικονική εγκατασταθεί και ρυθμιστεί. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε μια προσαρμοσμένη Εικονική Linux με τον παράγοντα Εικονική Azure, ανατρέξτε στο θέμα [Azure Linux Εικονική παράγοντα](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Χρησιμοποιώντας τη συλλογή εικόνα Azure

Από ένα πάρτι ή περιόδου λειτουργίας τερματικού, χρησιμοποιήστε την ακόλουθη εντολή Azure CLI για να εντοπίσετε την πιο πρόσφατη εικόνα Ubuntu στη συλλογή Εικονική για χρήση κατά την πληκτρολόγηση

`azure vm image list | grep Ubuntu-14_04`

και επιλέξτε ένα από τα ονόματα των εικόνων, όπως είναι οι `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, και χρησιμοποιήστε την ακόλουθη εντολή για να δημιουργήσετε μια νέα Εικονική χρησιμοποιώντας αυτήν την εικόνα.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

όπου:

+ * &lt;όνομα εικονική cloudservice&gt; * είναι το όνομα του η Εικονική που θα γίνει ο κεντρικός υπολογιστής κοντέινερ Docker στο Azure

+  * &lt;username&gt; * είναι το όνομα χρήστη του χρήστη ρίζας προεπιλεγμένη από την εικονική Μηχανή

+ * &lt;τον κωδικό πρόσβασης&gt; * είναι ο κωδικός πρόσβασης του λογαριασμού *όνομα χρήστη* που πληροί τα πρότυπα πολυπλοκότητα για Azure

> [AZURE.NOTE] Προς το παρόν, έναν κωδικό πρόσβασης πρέπει να είναι τουλάχιστον 8 χαρακτήρες, να περιέχουν μία πεζούς και μία με κεφαλαία γράμματα χαρακτήρα, έναν αριθμό και έναν ειδικό χαρακτήρα, όπως έναν από τους εξής χαρακτήρες: `!@#$%^&+=`. Όχι, η περίοδος στο τέλος της προηγούμενης πρότασης δεν είναι ένα ειδικό χαρακτήρα.

Εάν η εντολή ολοκληρώθηκε με επιτυχία, θα πρέπει να δείτε κάτι παρόμοιο με το εξής, ανάλογα με τις ακριβείς ορίσματα και τις επιλογές που χρησιμοποιήσατε:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Δημιουργία μια εικονική μηχανή μπορεί να χρειαστούν μερικά λεπτά, αλλά μετά την έχει αποδοθεί (η τιμή κατάσταση είναι `ReadyRole`) ξεκινά το μονού Docker (την υπηρεσία Docker) και μπορείτε να συνδεθείτε με τον κεντρικό υπολογιστή του κοντέινερ Docker.

Για να ελέγξετε την εικονική Μηχανή Docker που δημιουργήσατε στο Azure, πληκτρολογήστε

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

όπου * &lt;εικονική-όνομα--χρησιμοποιήσατε&gt; * είναι το όνομα του υπολογιστή εικονικές που χρησιμοποιήσατε σε της κλήσης σε `azure vm docker create`. Θα πρέπει να μπορείτε να δείτε κάτι παρόμοιο με το εξής, που υποδεικνύει ότι το Εικονική Host Docker είναι προς τα επάνω και εκτελείται στο Azure και αναμονή για τις εντολές. 

Τώρα μπορείτε να δοκιμάσετε να συνδεθείτε χρησιμοποιώντας το πρόγραμμα-πελάτη docker για να λάβετε πληροφορίες (σε ορισμένες ρυθμίσεις προγράμματος-πελάτη Docker, όπως που σε υπολογιστή Mac, ίσως χρειαστεί να χρησιμοποιήσετε `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Για να βεβαιωθείτε ότι είναι όλα εργασία, μπορείτε να εξετάσετε την εικονική Μηχανή για την επέκταση Docker:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Έλεγχος ταυτότητας Εικονική Host docker

Εκτός από τη δημιουργία του Εικονική Docker το `azure vm docker create` εντολή δημιουργεί επίσης αυτόματα τα πιστοποιητικά είναι απαραίτητο για να επιτρέψετε σε υπολογιστή-πελάτη Docker για να συνδεθείτε με τον κεντρικό υπολογιστή του Azure κοντέινερ χρησιμοποιώντας το HTTPS και τα πιστοποιητικά είναι αποθηκευμένα σε υπολογιστές του πελάτη και του κεντρικού υπολογιστή, ανάλογα με την περίπτωση. Στην οι επόμενες προσπάθειες, τα υπάρχοντα πιστοποιητικά είναι ξανά και να θέσει σε κοινή χρήση με τον νέο κεντρικό υπολογιστή.

Από προεπιλογή, τα πιστοποιητικά τοποθετούνται στη `~/.docker`, και Docker θα ρυθμιστεί για να εκτελέσετε στη θύρα **2376**. Εάν θέλετε να χρησιμοποιήσετε μια διαφορετική θύρα ή έναν κατάλογο και, στη συνέχεια, μπορείτε να χρησιμοποιήσετε μία από τις ακόλουθες `azure vm docker create` επιλογές γραμμής εντολών για να ρυθμίσετε τις παραμέτρους του κοντέινερ Docker Εικονική για να χρησιμοποιήσετε μια διαφορετική θύρα ή διαφορετικά πιστοποιητικά για τη σύνδεση σε προγράμματα-πελάτες του κεντρικού υπολογιστή:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Το μονού Docker στον κεντρικό υπολογιστή έχει ρυθμιστεί ώστε να περιμένετε να ακούσετε και τον έλεγχο ταυτότητας του προγράμματος-πελάτη συνδέσεις στην καθορισμένη θύρα χρησιμοποιώντας τα πιστοποιητικά που δημιουργούνται από το `azure vm docker create` εντολή. Στον υπολογιστή-πελάτη πρέπει να έχετε αυτά τα πιστοποιητικά για να αποκτήσετε πρόσβαση στον κεντρικό υπολογιστή Docker.

> [AZURE.NOTE] Ένα κεντρικό υπολογιστή δικτύου που εκτελεί χωρίς αυτά τα πιστοποιητικά να είναι εκτεθειμένο σε όλα τα άτομα που μπορούν να για να συνδεθείτε με τον υπολογιστή. Πριν να τροποποιήσετε την προεπιλεγμένη ρύθμιση παραμέτρων, βεβαιωθείτε ότι έχετε κατανοήσει τους κινδύνους για υπολογιστές και εφαρμογές.

## <a name="next-steps"></a>Επόμενα βήματα

* Είστε έτοιμοι για να μεταβείτε στον [Οδηγό χρήσης Docker] και χρησιμοποιήστε το Εικονική Docker. Για να δημιουργήσετε μια Εικονική με δυνατότητα Docker στην νέα πύλη, δείτε [πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker με την πύλη].

* Επίσης, την επέκταση Εικονική Docker Azure υποστηρίζει Docker σύνθεση, που χρησιμοποιεί ένα αρχείο δηλωτικό YAML για να τραβήξετε μια εφαρμογή μοντελοποιηθεί προγραμματιστής σε όλα τα περιβάλλοντα και να δημιουργήσετε μια συνεπή ανάπτυξη. Ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Docker και σύνθεση να ορίσετε και να εκτελέσετε μια εφαρμογή πολλών κοντέινερ σε μια εικονική μηχανή Azure].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker με την πύλη]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Οδηγός docker]: https://docs.docker.com/userguide/
 
[Γρήγορα αποτελέσματα με το Docker και σύνθεση για να ορίσετε και να εκτελέσετε μια εφαρμογή πολλών κοντέινερ σε μια εικονική μηχανή Azure]:virtual-machines-linux-docker-compose-quickstart.md
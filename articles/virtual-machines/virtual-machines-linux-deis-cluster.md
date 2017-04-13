<properties
   pageTitle="Ανάπτυξη έναν κόμβο 3 Deis σύμπλεγμα | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να δημιουργήσετε έναν κόμβο 3 Deis σύμπλεγμα σε Azure χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Ανάπτυξη έναν κόμβο 3 Deis συμπλέγματος

Σε αυτό το άρθρο σάς καθοδηγεί προμήθεια μιας [Deis](http://deis.io/) σύμπλεγμα σε Azure. Να καλύπτει όλα τα βήματα από τη δημιουργία τα πιστοποιητικά είναι απαραίτητο για την ανάπτυξη και κλίμακας ένα δείγμα εφαρμογής **μεταβείτε** στο σύμπλεγμα που μόλις προμήθεια του φακέλου.

Το παρακάτω διάγραμμα παρουσιάζει την αρχιτεκτονική του εγκατεστημένου συστήματος. Ένας διαχειριστής συστήματος διαχειρίζεται το σύμπλεγμα χρησιμοποιώντας Deis εργαλεία όπως το **deis** και **deisctl**. Συνδέσεις δημιουργούνται μέσω ενός Azure εξισορρόπηση φόρτου, η οποία προωθεί τις συνδέσεις σε μία από το μέλος κόμβοι στο σύμπλεγμα. Τα προγράμματα-πελάτες πρόσβασης αναπτυχθεί εφαρμογές μέσω καθώς και τη μονάδα εξισορρόπησης φόρτου. Σε αυτήν την περίπτωση, την εξισορρόπηση φόρτου προωθεί την κυκλοφορία σε μια Deis δικτυώματος δρομολογητή, τα οποία routs περαιτέρω την κυκλοφορία στο αντίστοιχο Docker κοντέινερ φιλοξενείται στο σύμπλεγμα.

  ![Διάγραμμα αρχιτεκτονική ανεπτυγμένος Desis συμπλέγματος](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Για να εκτελέσετε τα παρακάτω βήματα, θα πρέπει:

 * Μια ενεργή συνδρομή Azure. Εάν δεν έχετε, μπορείτε να λάβετε μια δωρεάν ίχνους στην [azure.com](https://azure.microsoft.com/).
 * Έναν εταιρικό ή σχολικό id για να χρησιμοποιήσετε ομάδες Azure πόρων. Εάν έχετε έναν προσωπικό λογαριασμό και συνδεθείτε με το αναγνωριστικό Microsoft, πρέπει να [δημιουργήσετε ένα αναγνωριστικό εργασίας από το προσωπικό](virtual-machines-windows-create-aad-work-id.md).
 * Είτε--ανάλογα με το πρόγραμμα-πελάτη το λειτουργικό σας σύστημα--το [Azure PowerShell](../powershell-install-configure.md) ή το [Azure CLI για Mac, Linux, και Windows](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL χρησιμοποιείται για να δημιουργήσετε τα πιστοποιητικά είναι απαραίτητο.
 * Ενός πελάτη Git όπως [Git πάρτι](https://git-scm.com/).
 * Για να ελέγξετε το δείγμα εφαρμογής, θα χρειαστεί επίσης ένα διακομιστή DNS. Μπορείτε να χρησιμοποιήσετε οποιαδήποτε διακομιστές DNS ή τις υπηρεσίες που υποστηρίζουν εγγραφές A μπαλαντέρ.
 * Έναν υπολογιστή για να εκτελέσετε Deis εργαλεία προγράμματος-πελάτη. Μπορείτε να χρησιμοποιήσετε ένα τοπικό υπολογιστή ή μια εικονική μηχανή. Μπορείτε να εκτελέσετε αυτά τα εργαλεία σε σχεδόν οποιοδήποτε Linux διανομής, αλλά Ubuntu χρησιμοποιήστε τις παρακάτω οδηγίες.

## <a name="provision-the-cluster"></a>Παροχή του συμπλέγματος

Σε αυτήν την ενότητα, θα χρησιμοποιήσετε ένα πρότυπο [Από διαχειριστή πόρων Azure](../azure-resource-manager/resource-group-overview.md) από το αποθετήριο Άνοιγμα αρχείου προέλευσης [azure-γρήγορη έναρξη-πρότυπα](https://github.com/Azure/azure-quickstart-templates). Πρώτα, θα μπορείτε να αντιγράψετε προς τα κάτω στο πρότυπο. Στη συνέχεια, θα δημιουργήσετε ένα νέο ζεύγος κλειδιού SSH για έλεγχο ταυτότητας. Και, στη συνέχεια, θα μπορείτε να ρυθμίσετε ένα νέο αναγνωριστικό για το σύμπλεγμα που. Και τέλος, θα χρησιμοποιήσετε τη δέσμη ενεργειών κελύφους ή τη δέσμη ενεργειών του PowerShell για την προμήθεια του συμπλέγματος.

1. Δημιουργία διπλότυπου του αποθετηρίου: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Μεταβείτε στο φάκελο προτύπων:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Δημιουργήστε ένα νέο SSH κλειδιού ζεύγος χρησιμοποιώντας ssh keygen:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Δημιουργήστε ένα πιστοποιητικό χρησιμοποιώντας το παραπάνω ιδιωτικό κλειδί:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Μεταβείτε στο [https://discovery.etcd.io/new](https://discovery.etcd.io/new) για να δημιουργήσετε έναν νέο κωδικό σύμπλεγμα, η οποία μοιάζει κάτι όπως:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Κάθε σύμπλεγμα CoreOS πρέπει να έχει ένα μοναδικό διακριτικό από αυτήν τη δωρεάν υπηρεσία. Για περισσότερες λεπτομέρειες, ανατρέξτε [στην τεκμηρίωση CoreOS](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Τροποποιήστε το αρχείο **cloud config.yaml** για να αντικαταστήσετε το υπάρχον διακριτικό **εντοπισμού** με το νέο κωδικό:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Τροποποιήστε το αρχείο **azuredeploy-parameters.json** : Ανοίξτε το πιστοποιητικό που δημιουργήσατε στο βήμα 4 σε έναν επεξεργαστή κειμένου. Αντιγράψτε όλο το κείμενο μεταξύ `----BEGIN CERTIFICATE-----` και `-----END CERTIFICATE-----` σε την παράμετρο **sshKeyData** (θα πρέπει να καταργήσετε όλους τους χαρακτήρες νέας γραμμής).

8. Τροποποιήστε την παράμετρο **newStorageAccountName** . Αυτός είναι ο λογαριασμός χώρου αποθήκευσης για το λειτουργικό σύστημα Εικονική δίσκων. Αυτό το όνομα λογαριασμού πρέπει να έχει καθολικά μοναδικό.

9. Τροποποιήστε την παράμετρο **publicDomainName** . Αυτό θα μετατραπεί σε μέρος του ονόματος DNS που σχετίζονται με τη δημόσια IP εξισορρόπησης φόρτου. Το τελικό FQDN θα έχει τη μορφή του _[τιμή αυτής της παραμέτρου]_. _[περιοχή]_. cloudapp.azure.com. Για παράδειγμα, εάν καθορίσετε το όνομα ως deishbai32 και την ομάδα των πόρων έχει αναπτυχθεί στην περιοχή Δυτική η.π.α., στη συνέχεια, το τελικό FQDN για την εξισορρόπηση φόρτου θα deishbai32.westus.cloudapp.azure.com.

10. Αποθηκεύστε το αρχείο παραμέτρων. Και, στη συνέχεια, που μπορούν να προμηθεύσουν στο σύμπλεγμα χρησιμοποιώντας Azure PowerShell:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  ή Azure CLI:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Μόλις παρασχεθεί την ομάδα των πόρων, μπορείτε να δείτε όλους τους πόρους στην ομάδα στην πύλη Azure κλασική. Όπως φαίνεται στο παρακάτω στιγμιότυπο οθόνης, στην ομάδα πόρων περιέχει ένα εικονικό δίκτυο με τρεις ΣΠΣ, που είναι συνδεδεμένοι με το ίδιο σύνολο διαθεσιμότητα. Η ομάδα περιέχει επίσης μια μονάδα εξισορρόπησης φόρτου, η οποία έχει ένα συσχετισμένο δημόσια IP.

  ![Η ομάδα πόρων προμήθεια του φακέλου στην πύλη Azure κλασική](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Εγκατάσταση του προγράμματος-πελάτη

Χρειάζεστε **deisctl** για να ελέγχετε το Deis σύμπλεγμα. Παρόλο που deisctl εγκαθίσταται αυτόματα σε όλους τους κόμβους συμπλέγματος, είναι καλό να χρησιμοποιήσουν deisctl σε ένα ξεχωριστό διαχείρισης υπολογιστή. Επιπλέον, επειδή όλοι οι κόμβοι έχουν ρυθμιστεί με μόνο ιδιωτικές διευθύνσεις IP, θα χρειαστεί να χρησιμοποιήσετε SSH διοχέτευση μέσω τη μονάδα εξισορρόπησης φόρτου, που περιλαμβάνει μια δημόσια IP, για να συνδεθείτε με το μηχανές κόμβο. Ακολουθούν τα βήματα με τη ρύθμιση του deisctl σε ένα ξεχωριστό Ubuntu φυσική ή εικονική μηχανή.

1. Εγκατάσταση deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Προσθήκη σας ιδιωτικό κλειδί για να ssh παράγοντα:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Ρύθμιση παραμέτρων deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Το πρότυπο καθορίζει κανόνες εισερχόμενων NAT που αντιστοιχούν 2223 να παρουσίας 1, 2224 παρουσία 2 και 2225 παρουσία 3. Αυτό παρέχει πλεονασμού για τη χρήση του εργαλείου deisctl. Μπορείτε να εξετάσετε αυτούς τους κανόνες στην πύλη Azure κλασική:

![NAT κανόνες σχετικά με τη μονάδα εξισορρόπησης φόρτου](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Προς το παρόν το πρότυπο υποστηρίζει μόνο συμπλεγμάτων 3-κόμβο. Αυτό είναι λόγω έναν περιορισμό του προτύπου από διαχειριστή πόρων Azure ορισμό κανόνα NAT, που δεν υποστηρίζει σύνταξη βρόχο.

## <a name="install-and-start-the-deis-platform"></a>Εγκαταστήστε και ξεκινήστε την Deis πλατφόρμα

Τώρα μπορείτε να χρησιμοποιήσετε deisctl να εγκαταστήσετε και να ξεκινήσετε την πλατφόρμα Deis:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Έναρξη της πλατφόρμας χρειάζεται λίγο (έως και 10 λεπτά). Ειδικά, η εκκίνηση της υπηρεσίας Δόμηση μπορεί να διαρκέσει μεγάλο χρονικό διάστημα. Και ορισμένες φορές χρειάζεται προσπαθήσετε μερικές φορές με επιτυχία: Εάν η λειτουργία φαίνεται να μην ανταποκρίνεται, δοκιμάστε να πληκτρολογήσετε `ctrl+c` για να διακόψετε την εκτέλεση της εντολής και προσπαθήστε ξανά.

Μπορείτε να χρησιμοποιήσετε `deisctl list` για να επαληθεύσετε εάν εκτελείτε όλες τις υπηρεσίες:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Συγχαρητήρια! Τώρα έχετε στη διάθεσή σας τρέχον Deis clsuter σε Azure! Στη συνέχεια, ας ανάπτυξη ενός δείγματος Μετάβαση εφαρμογής για να δείτε το σύμπλεγμα στην πράξη.

## <a name="deploy-and-scale-a-hello-world-application"></a>Ανάπτυξη και κλιμάκωση μιας εφαρμογής Γεια

Ακολουθήστε τα παρακάτω βήματα δείχνουν πώς μπορείτε να αναπτύξετε ένα "Γεια" Μετάβαση εφαρμογής για το σύμπλεγμα. Τα βήματα βασίζονται στις [Deis τεκμηρίωση](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Για τη δρομολόγηση πλέγμα για να λειτουργήσει σωστά, θα πρέπει να έχετε μια εγγραφή A μπαλαντέρ για τον τομέα που δείχνει προς τη δημόσια IP από τη μονάδα εξισορρόπησης φόρτου. Το παρακάτω στιγμιότυπο οθόνης εμφανίζει την εγγραφή Α για ένα δείγμα δήλωσης τομέα στο GoDaddy:

    ![Εγγραφή Α Godaddy](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Εγκατάσταση deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Δημιουργήστε ένα νέο κλειδί SSH και, στη συνέχεια, προσθέστε το δημόσιο κλειδί για να GitHub (βεβαίως, μπορείτε επίσης να χρησιμοποιήσετε ξανά τα υπάρχοντα κλειδιά). Για να δημιουργήσετε ένα νέο ζεύγος κλειδιού SSH, χρησιμοποιήστε:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Προσθέστε id_rsa.pub ή το δημόσιο κλειδί της επιλογής σας, για να GitHub. Μπορείτε να το κάνετε αυτό, χρησιμοποιώντας το προσθέσετε SSH κλειδιού κουμπί στην οθόνη ρύθμισης παραμέτρων σας SSH πλήκτρα:

  ![Πλήκτρο Github](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Καταχωρήσετε έναν νέο χρήστη:

        deis register http://deis.[your domain]
<p />
6. Προσθήκη του κλειδιού SSH:

        deis keys:add [path to your SSH public key]
  <p />      
7. Δημιουργία μιας εφαρμογής.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Το πάτημα git θα ενεργοποιήσει Docker εικόνες που θα δημιουργηθεί και να αναπτυχθεί, θα χρειαστούν μερικά λεπτά. Από την εμπειρία μου, μερικές φορές, βήμα 10 (Pushing εικόνας στο αποθετήριο δεδομένων ιδιωτικό) μπορεί να σταματήσει να ανταποκρίνεται. Όταν συμβαίνει αυτό, μπορείτε να διακόψετε τη διαδικασία, να καταργήσετε την εφαρμογή που χρησιμοποιεί ' deis εφαρμογές: καταστροφή – ένα <application name> ` to remove the application and try again. You can use `deis apps:list' για να βρείτε το όνομα της εφαρμογής σας. Εάν όλα λειτουργούν, πρέπει να δείτε κάτι παρόμοιο με το εξής στο τέλος της εντολής εξόδους:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Επιβεβαιώστε εάν λειτουργεί η εφαρμογή:

        curl -S http://[your application name].[your domain]
  Θα πρέπει να δείτε:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Κλίμακα 3 παρουσιών της εφαρμογής:

        deis scale cmd=3
<p />
11. Προαιρετικά, μπορείτε να χρησιμοποιήσετε deis πληροφορίες για να εξετάσετε τις λεπτομέρειες της εφαρμογής σας. Το παρακάτω εξόδους είναι από μου ανάπτυξη εφαρμογών:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Επόμενα βήματα

Σε αυτό το άρθρο περπάτησαν που όλα τα βήματα για την προμήθεια νέου Deis σύμπλεγμα σε Azure χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων Azure. Το πρότυπο υποστηρίζει πλεονασμού σε tooling συνδέσεις, καθώς και για την ανάπτυξη εφαρμογών εξισορρόπησης φόρτου. Το πρότυπο αποφεύγονται επίσης με δημόσια διευθύνσεις IP σε κόμβους μέλος, το οποίο αποθηκεύει πολύτιμο δημόσια IP πόρων και παρέχει ένα πιο ασφαλή περιβάλλον για τις εφαρμογές του κεντρικού υπολογιστή. Για περισσότερες πληροφορίες, ανατρέξτε στα ακόλουθα άρθρα:

[Azure Επισκόπηση της διαχείρισης πόρων] [resource-group-overview]  
[Πώς μπορείτε να χρησιμοποιήσετε το CLI Azure] [azure-command-line-tools]  
[Χρήση του Azure PowerShell με τη Διαχείριση Azure πόρων] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md

<properties
   pageTitle="Γρήγορα αποτελέσματα με τη χρήση docker με swarm στο Azure"
   description="Περιγράφει τον τρόπο για να δημιουργήσετε μια ομάδα ΣΠΣ με την επέκταση Εικονική Docker και χρησιμοποιήστε swarm για να δημιουργήσετε ένα σύμπλεγμα Docker."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Πώς μπορείτε να χρησιμοποιήσετε docker με swarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Αυτό το θέμα δείχνει ένα πολύ απλής τρόπο για να χρησιμοποιήσετε [docker](https://www.docker.com/) με [swarm](https://github.com/docker/swarm) για να δημιουργήσετε ένα σύμπλεγμα Διαχείριση swarm στην Azure. Δημιουργεί τέσσερα εικονικές μηχανές στο Azure, μία για εκτέλεση ενεργειών ως διαχειριστής του swarm και τριών ως μέρος του συμπλέγματος docker κεντρικών υπολογιστών. Όταν ολοκληρώσετε, μπορείτε να χρησιμοποιήσετε swarm για να δείτε το σύμπλεγμα και, στη συνέχεια, να αρχίσετε να χρησιμοποιείτε docker σε αυτό. Επιπλέον, οι κλήσεις Azure CLI σε αυτό το θέμα χρήση της λειτουργίας διαχείρισης (asm) υπηρεσίας. 

> [AZURE.NOTE] Αυτό το θέμα χρησιμοποιεί docker με swarm και το Azure CLI *χωρίς* χρήση **docker υπολογιστή** για να εμφανίσετε πώς τα εργαλεία διαφορετικό συνεργάζονται, αλλά παραμένουν ανεξάρτητες. **docker μηχάνημα** έχει **--swarm** διακόπτες που σας επιτρέπουν να χρησιμοποιήσετε **docker υπολογιστή** για να προσθέσετε απευθείας κόμβους σε ένα swarm. Για παράδειγμα, ανατρέξτε στην τεκμηρίωση [docker υπολογιστή](https://github.com/docker/machine) . Εάν χάσατε **docker υπολογιστή** που εκτελεί έναντι ΣΠΣ Azure, δείτε [πώς μπορείτε να χρησιμοποιήσετε docker-μηχανής με Azure](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Δημιουργία docker hosts με εικονικές μηχανές Windows Azure

Αυτό το θέμα δημιουργεί τέσσερις ΣΠΣ, αλλά μπορείτε να χρησιμοποιήσετε οποιονδήποτε αριθμό που θέλετε. Κλήση τα εξής με τις * &lt;τον κωδικό πρόσβασης&gt; * αντικαθίστανται από τον κωδικό πρόσβασης που έχετε επιλέξει.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Όταν ολοκληρώσετε τη διαδικασία θα πρέπει να μπορείτε να χρησιμοποιήσετε **τη λίστα azure εικονική** για να δείτε το ΣΠΣ Azure:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Κατά την εγκατάσταση swarm στο υπόδειγμα swarm Εικονική

Αυτό το θέμα χρησιμοποιεί το που [μοντέλο κοντέινερ της εγκατάστασης από το docker swarm τεκμηρίωση](https://github.com/docker/swarm#1---docker-image) --αλλά επίσης μπορείτε να SSH στο **υπόδειγμα swarm**. Σε αυτό το μοντέλο, **swarm** γίνεται λήψη ως κοντέινερ docker εκτελεί swarm. Παρακάτω, εκτελέστε αυτό το βήμα *από απόσταση από το φορητό υπολογιστή σας με τη χρήση docker* για να συνδεθείτε στο **υπόδειγμα swarm** Εικονική και ενημερώστε ώστε να χρησιμοποιεί το αναγνωριστικό δημιουργίας εντολή συμπλέγματος, **Δημιουργία swarm**. Το αναγνωριστικό σύμπλεγμα είναι **swarm** ανακαλύψει πώς τα μέλη της ομάδας swarm. (Μπορείτε να επίσης κλωνοποίηση του αποθετηρίου και δημιουργήστε την στον εαυτό σας, το οποίο θα σας δώσει πλήρους ελέγχου και να ενεργοποιήσετε τον εντοπισμό σφαλμάτων.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Τελευταία γραμμή που είναι το αναγνωριστικό σύμπλεγμα; Αντιγράψτε το κάπου επειδή που θα χρησιμοποιήσετε ξανά όταν συμμετέχετε σε τον κόμβο ΣΠΣ στο υπόδειγμα swarm για να δημιουργήσετε το "swarm". Σε αυτό το παράδειγμα, το αναγνωριστικό σύμπλεγμα είναι **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Απλώς να είμαστε σαφείς, χρησιμοποιούμε μας εγκατάστασης του τοπικού docker για να συνδεθείτε με το **υπόδειγμα swarm** Εικονική στο Azure και οδηγία **swarm υποδείγματος** για να κάνετε λήψη, εγκατάσταση και εκτελέστε την εντολή **Δημιουργία** , η οποία επιστρέφει το αναγνωριστικό συμπλέγματος που χρησιμοποιούμε για σκοπούς εντοπισμού αργότερα.
<!-- -->
> Για να επιβεβαιώσετε αυτό, εκτελέστε `docker -H tcp://` * &lt;hostname&gt; * ` images` για να εμφανίσετε τις διαδικασίες κοντέινερ στον υπολογιστή **swarm υπόδειγμα** και σε έναν άλλο κόμβο για σύγκριση (επειδή μας εκτέλεση της προηγούμενης εντολής swarm με το διακόπτη **--Διαχείριση πόρων** , το κοντέινερ καταργήθηκε μετά την ολοκλήρωση, επομένως χρησιμοποιώντας **docker ps - a** δεν επιστρέφουν οτιδήποτε).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Εάν είστε εξοικειωμένοι με **docker**, θα ξέρετε ότι άλλους κόμβους έχουν καταχωρήσεις επειδή έχουν ληφθεί και εκτελέστε ακόμη χωρίς εικόνες.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Συμμετοχή στον κόμβο ΣΠΣ μας σύμπλεγμα docker

Για κάθε κόμβο, λίστα το τελικό σημείο πληροφοριών με το Azure CLI. Παρακάτω κάνουμε για τον κεντρικό υπολογιστή docker **swarm-κόμβου-1** για να λάβετε τον κόμβο docker θύρα.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Χρήση **docker** και το `-H` την επιλογή για να τοποθετήστε το δείκτη του προγράμματος-πελάτη docker σας κόμβο Εικονική συμμετοχή σε αυτόν τον κόμβο για να το swarm που δημιουργείτε, μεταφέροντας το αναγνωριστικό σύμπλεγμα και τη θύρα docker στον κόμβο (η τελευταία με χρήση **διεύθυνσης--**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Που σας αρέσει. Για να επιβεβαιώσετε αυτήν **swarm** εκτελείται σε **swarm-κόμβου-1** , θα σας, πληκτρολογήστε:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Επαναλάβετε για όλους τους κόμβους του συμπλέγματος. Σε περίπτωση μας, κάνουμε για **swarm-κόμβου-2** και **swarm-κόμβου-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Ξεκινήστε τη διαχείριση του swarm συμπλέγματος

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

και, στη συνέχεια, μπορείτε να παραθέσετε ανάληψη σας κόμβους στο σύμπλεγμά σας:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

Μεταβείτε εκτελέσετε ενέργειες σε swarm σας. Για να αναζητήσετε έμπνευση, ανατρέξτε στο θέμα [https://github.com/docker/swarm/](https://github.com/docker/swarm/)ή ίσως ένα [βίντεο](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 

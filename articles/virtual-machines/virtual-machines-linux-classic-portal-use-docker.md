<properties
    pageTitle="Χρήση επέκτασης Εικονική Docker για Linux | Microsoft Azure"
    description="Περιγράφει Docker και τις επεκτάσεις εικονικές μηχανές Windows Azure και πώς μπορείτε να δημιουργήσετε εικονικές μηχανές Windows Azure που είναι docker hosts χρησιμοποιώντας το Azure CLI στο μοντέλο κλασική ανάπτυξης."
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
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Χρησιμοποιώντας την επέκταση Εικονική Docker με την πύλη κλασική του Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) είναι μία από τις πιο δημοφιλείς μεθόδους Application virtualization που χρησιμοποιεί [Linux κοντέινερ](http://en.wikipedia.org/wiki/LXC) και όχι σε εικονικές μηχανές έτσι ώστε να απομόνωση δεδομένων και την υπολογιστική σε κοινόχρηστους πόρους. Μπορείτε να χρησιμοποιήσετε την επέκταση Docker Εικονική διαχειριζόμενο από [Τον παράγοντα Linux Azure] για να δημιουργήσετε μια Εικονική Docker που φιλοξενεί οποιονδήποτε αριθμό κοντέινερ για τις εφαρμογές στο Azure.

> [AZURE.NOTE] Αυτό το θέμα περιγράφει πώς μπορείτε να δημιουργήσετε μια Εικονική Docker από την πύλη του Azure κλασική. Για να δείτε πώς μπορείτε να δημιουργήσετε μια Εικονική Docker στη γραμμή εντολών, δείτε [πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker από το περιβάλλον γραμμής εντολών Azure (Azure CLI)]. Για να δείτε μια συζήτηση υψηλού επιπέδου του κοντέινερ και τα πλεονεκτήματα, ανατρέξτε στο θέμα [Docker υψηλού επιπέδου πίνακα](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Δημιουργήστε μια νέα Εικονική από τη συλλογή εικόνων
Το πρώτο βήμα απαιτεί μια Εικονική Azure από μια εικόνα Linux που υποστηρίζει την επέκταση Εικονική Docker, χρησιμοποιώντας μια εικόνα Ubuntu 14.04 Αποτελεσμάτων από τη συλλογή εικόνων ως παράδειγμα εικόνας διακομιστή και υπολογιστή 14.04 Ubuntu ως ένα πρόγραμμα-πελάτη. Στην πύλη του, επιλέξτε **+ νέο** στην κάτω αριστερή γωνία για να δημιουργήσετε μια νέα παρουσία Εικονική και επιλέξτε μια εικόνα Ubuntu 14.04 Αποτελεσμάτων από τις διαθέσιμες επιλογές ή από την πλήρη συλλογή εικόνων, όπως φαίνεται παρακάτω.

> [AZURE.NOTE] Προς το παρόν, πιο πρόσφατο από Ιούλιος 2014 μόνο Ubuntu 14.04 Αποτελεσμάτων εικόνες υποστηρίζει την επέκταση Εικονική Docker.

![Δημιουργία μιας νέας εικόνας Ubuntu](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Δημιουργία πιστοποιητικών Docker

Αφού δημιουργηθεί η Εικονική, βεβαιωθείτε ότι Docker είναι εγκατεστημένη στον υπολογιστή-πελάτη. (Για λεπτομέρειες, ανατρέξτε στο θέμα [οδηγίες εγκατάστασης Docker](https://docs.docker.com/installation/#installation).)

Δημιουργήστε αρχεία με το πιστοποιητικό και το κλειδί για επικοινωνία Docker σύμφωνα με την [Εκτέλεση Docker με το πρόθεμα https] και τοποθετήστε τα στο το **`~/.docker`** καταλόγου στον υπολογιστή-πελάτη.

> [AZURE.NOTE] Η επέκταση Εικονική Docker στην πύλη αυτήν τη στιγμή απαιτεί διαπιστευτήρια που είναι κωδικοποίηση base64.

Στη γραμμή εντολών, χρησιμοποιήστε **`base64`** ή κάποιο άλλο αγαπημένες κωδικοποίησης εργαλείο για να δημιουργήσετε κωδικοποίηση base64 θέματα. Αυτή η ενέργεια με ένα απλό σύνολο αρχείων πιστοποιητικό και το κλειδί μπορεί να μοιάζει με αυτό:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Προσθέστε την επέκταση Εικονική Docker
Για να προσθέσετε την επέκταση Εικονική Docker, εντοπίστε την παρουσία Εικονική που δημιουργήσατε και κάντε κύλιση προς τα κάτω, **επεκτάσεις** και κάντε κλικ για να εμφανίσετε επεκτάσεις Εικονική, όπως φαίνεται παρακάτω.
> [AZURE.NOTE] Αυτή η λειτουργία υποστηρίζεται στην πύλη προεπισκόπηση μόνο: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Προσθέστε μια επέκταση
Κάντε κλικ στο **+ Προσθήκη** για να εμφανίσετε τις πιθανές επεκτάσεις Εικονική μπορείτε να προσθέσετε αυτό Εικονική.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Επιλέξτε την επέκταση Εικονική Docker
Επιλέξτε την επέκταση Εικονική Docker, η οποία εμφανίζει τα τις Docker περιγραφή και τις σημαντικές συνδέσεις, και, στη συνέχεια, κάντε κλικ στην επιλογή " **Δημιουργία** " στο κάτω μέρος για να ξεκινήσετε τη διαδικασία εγκατάστασης.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Προσθέστε το πιστοποιητικό και το πλήκτρο αρχεία:

Στα πεδία φόρμας, εισαγάγετε τις εκδόσεις κωδικοποίηση base64 πιστοποιητικού CA, το πιστοποιητικό διακομιστή και τον αριθμό-κλειδί διακομιστή, όπως φαίνεται στο παρακάτω γραφικό.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Σημειώστε ότι (όπως στο στην προηγούμενη εικόνα) το 2376 είναι συμπληρώσει από προεπιλογή. Μπορείτε να εισαγάγετε οποιαδήποτε τελικού σημείου εδώ, αλλά θα είναι το επόμενο βήμα για να ανοίξετε το αντίστοιχο τελικό σημείο. Εάν αλλάξετε την προεπιλεγμένη, βεβαιωθείτε ότι για να ανοίξετε το αντίστοιχο τελικό σημείο στο επόμενο βήμα.

## <a name="add-the-docker-communication-endpoint"></a>Προσθέστε το τελικό σημείο επικοινωνίας Docker
Όταν προβάλετε την ομάδα των πόρων που έχετε δημιουργήσει, επιλέξτε την ομάδα ασφαλείας δικτύου που σχετίζονται με την Εικονική και κάντε κλικ στην επιλογή **Κανόνες εισερχομένων ασφαλείας** για να προβάλετε τους κανόνες, όπως φαίνεται εδώ.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Κάντε κλικ στο κουμπί **+ Προσθήκη** για να προσθέσετε έναν άλλο κανόνα και στην προεπιλεγμένη περίπτωση, πληκτρολογήστε ένα όνομα για το τελικό σημείο (σε αυτό το παράδειγμα, **Docker**) και 2376 "προορισμός θύρα περιοχή". Ορίστε την τιμή πρωτόκολλο που εμφανίζει **TCP**και κάντε κλικ στο κουμπί **OK** για να δημιουργήσετε τον κανόνα.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Δοκιμή του προγράμματος-πελάτη Docker και Azure Docker κεντρικού υπολογιστή
Εντοπίστε και αντιγράψτε το όνομα του τομέα σας Εικονική και, στη γραμμή εντολών του υπολογιστή-πελάτη, τύπος `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (όπου *dockerextension* αντικαθίσταται από ο δευτερεύων τομέας για σας Εικονική).

Το αποτέλεσμα θα πρέπει να είναι παρόμοιο με αυτό:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Αφού ολοκληρώσετε τα παραπάνω βήματα, τώρα έχετε ένα πλήρως λειτουργικό host Docker εκτελείται σε μια Εικονική Azure έχει ρυθμιστεί ώστε να συνδεθείτε με απομακρυσμένα από άλλα προγράμματα-πελάτες.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα

Είστε έτοιμοι για να μεταβείτε στον [Οδηγό χρήσης Docker] και χρησιμοποιήστε το Εικονική Docker. Εάν θέλετε να αυτοματοποιήσετε τη δημιουργία Docker κεντρικούς υπολογιστές ΣΠΣ Azure μέσω περιβάλλον γραμμής εντολών, ανατρέξτε στο θέμα [πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker από το περιβάλλον γραμμής εντολών Azure (Azure CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Πώς μπορείτε να χρησιμοποιήσετε την επέκταση Εικονική Docker από το περιβάλλον γραμμής εντολών Azure (Azure CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux παράγοντα]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Εκτέλεση Docker με https]: http://docs.docker.com/articles/https/
[Οδηγός docker]: https://docs.docker.com/userguide/

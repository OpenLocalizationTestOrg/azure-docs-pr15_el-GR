<properties
    pageTitle="Εγκαταστήστε το Azure περιβάλλον γραμμής εντολών | Microsoft Azure"
    description="Εγκαταστήστε το περιβάλλον γραμμής εντολών Azure (CLI) για Mac, Linux και Windows για να ξεκινήσετε τη χρήση των υπηρεσιών Azure"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Εγκαταστήστε το Azure CLI

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Εγκαταστήστε γρήγορα το περιβάλλον γραμμής εντολών Azure (Azure CLI) για να χρησιμοποιήσετε ένα σύνολο εντολών βάσει κελύφους ανοιχτού κώδικα για τη δημιουργία και τη διαχείριση των πόρων στο Microsoft Azure. Έχετε πολλές επιλογές για να εγκαταστήσετε αυτά τα εργαλεία πλατφόρμες στον υπολογιστή σας: 

* **το πακέτο npm** - εκτελέστε npm (η Διαχείριση πακέτου για JavaScript) για να εγκαταστήσετε την πιο πρόσφατη πακέτου Azure CLI σε Linux διανομής ή λειτουργικό σύστημα. Απαιτεί node.js και npm στον υπολογιστή σας.
* **Πρόγραμμα εγκατάστασης** - λήψη εγκατάσταση για εύκολη εγκατάσταση σε Mac ή Windows.
* **Κοντέινερ docker** - ξεκινήσετε να χρησιμοποιείτε την πιο πρόσφατη CLI σε ένα κοντέινερ Docker ετοιμότητα για χρήση. Απαιτεί Docker κεντρικού υπολογιστή στον υπολογιστή σας.
    
Για περισσότερες επιλογές και φόντου, ανατρέξτε στο θέμα το αποθετήριο δεδομένων έργου στο [GitHub](https://github.com/azure/azure-xplat-cli). 

Μόλις το Azure CLI εγκαθίσταται, [Συνδέστε τη με τη συνδρομή σας Azure](xplat-cli-connect.md) και εκτελέστε τις εντολές **azure** από το περιβάλλον γραμμής εντολών (πάρτι, Terminal, γραμμή εντολών, κ.ο.κ.) για να εργαστείτε με τους πόρους σας Azure.



## <a name="option-1-install-an-npm-package"></a>Επιλογή 1: Εγκατάσταση του πακέτου npm

Για να εγκαταστήσετε το CLI από ένα πακέτο npm, βεβαιωθείτε ότι έχετε λάβει και εγκαταστήσει τις [πιο πρόσφατες Node.js και npm](https://nodejs.org/en/download/package-manager/). Στη συνέχεια, εκτελέστε το **npm εγκατάσταση** για να εγκαταστήσετε το πακέτο του azure cli: 

    npm install -g azure-cli

Στην κατανομές Linux, ίσως χρειαστεί να χρησιμοποιήσετε **sudo** για να εκτελέσετε με επιτυχία την εντολή __npm__ , ως εξής:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Εάν πρέπει να εγκαταστήσετε ή να ενημερώσετε Node.js και npm στο Linux διανομής ή το λειτουργικό σύστημα, συνιστάται να εγκαταστήσετε την πιο πρόσφατη έκδοση Node.js Αποτελεσμάτων (4.x). Εάν χρησιμοποιείτε μια παλαιότερη έκδοση, μπορεί να λάβετε σφάλματα εγκατάστασης. 

Εάν προτιμάτε, κάντε λήψη του το πιο πρόσφατο Linux [αρχείο πίσσα] [ linux-installer] για το πακέτο npm τοπικά. Στη συνέχεια, εγκαταστήστε το πακέτο ληφθεί npm ως εξής (σε Linux κατανομές ίσως χρειαστεί να χρησιμοποιήσετε **sudo**):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Επιλογή 2: Χρήση ενός προγράμματος εγκατάστασης

Εάν χρησιμοποιείτε υπολογιστή Mac ή Windows, τα ακόλουθα προγράμματα εγκατάστασης CLI είναι διαθέσιμο για λήψη:

* [Πρόγραμμα εγκατάστασης του Mac OS X][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]Στα Windows, μπορείτε επίσης να κάνετε λήψη του [Προγράμματος εγκατάστασης πλατφόρμας Web](https://go.microsoft.com/?linkid=9828653) για να εγκαταστήσετε το CLI. Αυτό το πρόγραμμα εγκατάστασης σάς δίνει την επιλογή για να εγκαταστήσετε το πρόσθετο SDK Azure και εργαλεία γραμμής εντολών μετά την εγκατάσταση του CLI. 


## <a name="option-3-use-a-docker-container"></a>Επιλογή 3: Χρησιμοποιήστε ένα κοντέινερ Docker

Εάν έχετε ρυθμίσει τον υπολογιστή σας ως μια υπηρεσία παροχής φιλοξενίας [Docker](https://docs.docker.com/engine/understanding-docker/) , μπορείτε να εκτελέσετε την πιο πρόσφατη CLI Azure σε ένα κοντέινερ Docker. Εκτελέστε την ακόλουθη εντολή (στο Linux κατανομές ίσως χρειαστεί να χρησιμοποιήσετε **sudo**):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Εκτέλεση εντολών Azure CLI
Μετά την εγκατάσταση του Azure CLI, εκτελέστε την εντολή **azure** από το περιβάλλον εργασίας χρήστη της γραμμής εντολών (πάρτι, Terminal, γραμμή εντολών, κ.ο.κ.). Για παράδειγμα, για να εκτελέσετε την εντολή Βοήθεια, πληκτρολογήστε τα εξής:

```
azure help
```
> [AZURE.NOTE]Σε ορισμένες κατανομές Linux, ενδέχεται να λάβετε ένα σφάλμα παρόμοιο με `/usr/bin/env: ‘node’: No such file or directory`. Αυτό το σφάλμα προέρχεται από πρόσφατες εγκαταστάσεις του Node.js που εγκαθίσταται στο /usr/bin/nodejs. Για να διορθώσετε αυτό το πρόβλημα, δημιουργήστε μια συμβολική σύνδεση με /usr/bin/node κατά την εκτέλεση αυτής της εντολής:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Για να δείτε την έκδοση του το Azure CLI εγκαταστήσατε, πληκτρολογήστε τα εξής:

```
azure --version
```

Τώρα είστε έτοιμοι! Για να αποκτήσετε πρόσβαση σε όλες τις εντολές CLI για εργασία με το δικό σας πόρους, [συνδεθείτε με τη συνδρομή σας Azure από το Azure CLI](xplat-cli-connect.md).

>[AZURE.NOTE] Όταν χρησιμοποιείτε Azure CLI για πρώτη φορά, εμφανίζεται ένα μήνυμα που σας ρωτά εάν θέλετε να επιτρέψετε στη Microsoft να συλλέγει πληροφορίες χρήσης. Η συμμετοχή είναι προαιρετική. Εάν επιλέξετε να συμμετάσχετε, μπορείτε να διακόψετε την ανά πάσα στιγμή, εκτελώντας `azure telemetry --disable`. Για να ενεργοποιήσετε τη συμμετοχή οποιαδήποτε στιγμή, εκτελέστε `azure telemetry --enable`.


## <a name="update-the-cli"></a>Ενημερώστε το CLI

Η Microsoft δημοσιεύει συχνά ενημερωμένες εκδόσεις από το Azure CLI. Εγκαταστήστε ξανά το CLI χρησιμοποιώντας το πρόγραμμα εγκατάστασης για το λειτουργικό σας σύστημα, ή να εκτελέσετε την πιο πρόσφατη κοντέινερ Docker. Ή, εάν έχετε εγκαταστήσει τις πιο πρόσφατες Node.js και npm εγκατασταθεί, ενημέρωση, πληκτρολογώντας τα εξής (σε Linux κατανομές ίσως χρειαστεί να χρησιμοποιήσετε **sudo**).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Ενεργοποίηση της καρτέλας ολοκλήρωσης

Ολοκλήρωση καρτέλα εντολών CLI υποστηρίζεται για Mac και το Linux.

Για να ενεργοποιήσετε το zsh, εκτελέστε:

```
echo '. <(azure --completion)' >> .zshrc
```

Για να ενεργοποιήσετε το πάρτι, εκτελέστε:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Επόμενα βήματα 

* [Σύνδεση από το CLI στη συνδρομή σας στο Azure](xplat-cli-connect.md) για να δημιουργήσετε και να διαχειριστείτε Azure πόρους.

* Για να μάθετε περισσότερα σχετικά με το Azure CLI, κάντε λήψη του πηγαίου κώδικα, προβλήματα με την αναφορά, ή συνεισφέρουν στο έργο, επισκεφθείτε τα [GitHub αποθετήριο δεδομένων για το Azure CLI](https://github.com/azure/azure-xplat-cli).

* Εάν έχετε ερωτήσεις σχετικά με τη χρήση της Azure CLI ή Azure, επισκεφθείτε τα [Φόρουμ του Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Εάν θέλετε, μπορείτε επίσης να δοκιμάσετε το βάσει Python [Azure CLI 2.0 Preview](https://github.com/azure/azure-cli).

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md

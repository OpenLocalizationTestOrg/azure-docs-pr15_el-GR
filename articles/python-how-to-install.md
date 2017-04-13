<properties
    pageTitle="Εγκατάσταση Python και το SDK - Azure"
    description="Μάθετε πώς να εγκαταστήσετε Python και το SDK για χρήση με το Azure."
    services=""
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="installing-python-and-the-sdk"></a>Κατά την εγκατάσταση Python και το SDK

Python είναι εύκολο να το πρόγραμμα εγκατάστασης των Windows και διατίθεται προ-εγκατεστημένες σε Mac, Linux και [Πάρτι για Windows](https://msdn.microsoft.com/commandline/wsl/about). Αυτός ο οδηγός σάς καθοδηγεί σε εγκατάσταση και τη λειτουργία του υπολογιστή σας έτοιμη για χρήση με το Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Τι περιέχει η Python Azure SDK;

Το SDK Azure για Python περιλαμβάνει στοιχεία που σας επιτρέπει να αναπτύξετε, ανάπτυξης και διαχείριση εφαρμογών Python για Azure. Συγκεκριμένα, η SDK Azure για Python περιλαμβάνει τα εξής:

* **Διαχείριση βιβλιοθηκών**. Αυτές οι βιβλιοθήκες κλάσης παρέχουν ένα περιβάλλον εργασίας Διαχείριση Azure πόροι, όπως λογαριασμοί χώρου αποθήκευσης, εικονικές μηχανές.

* **Βιβλιοθήκες χρόνου εκτέλεσης**. Αυτές οι βιβλιοθήκες κλάσης παρέχουν ένα περιβάλλον εργασίας για να αποκτήσετε πρόσβαση Azure δυνατότητες, όπως η υπηρεσία και αποθήκευσης bus.

## <a name="which-python-and-which-version-to-use"></a>Ποια Python και ποια έκδοση να χρησιμοποιήσετε

Υπάρχουν αρκετές μπορεί να είναι από προγράμματα μετατροπής Python - παραδείγματα:

* CPython - στο μεταφραστή Python τυπική και πιο συχνά χρησιμοποιούμενες
* PyPy - γρήγορη, συμβατή εναλλακτικές υλοποίηση για CPython
* IronPython - μεταφραστή Python που εκτελείται στον .net/CLR
* Jython - μεταφραστή Python που εκτελείται σε η εικονική μηχανή Java

**CPython** v2.7 ή v3.3 + και PyPy 5.4.0 είναι δοκιμή και υποστήριξη για το SDK Azure Python.

## <a name="where-to-get-python"></a>Πού μπορείτε να βρείτε Python;

Υπάρχουν πολλοί τρόποι για να λάβετε CPython:

* Απευθείας από [www.python.org][]
* Από μια φερέγγυα distro όπως [www.continuum.io][], [www.enthought.com][] ή [www.activestate.com][]
* Δημιουργία από αρχείο προέλευσης!

Εάν δεν έχετε μια συγκεκριμένη ανάγκη, συνιστάται να τις δύο πρώτες επιλογές.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Εγκατάσταση του SDK των Windows, Linux και MacOS (μόνο για βιβλιοθήκες του προγράμματος-πελάτη)

Εάν έχετε ήδη εγκαταστήσει Python, μπορείτε να χρησιμοποιήσετε pip για να εγκαταστήσετε μια δέσμη όλες τις βιβλιοθήκες του προγράμματος-πελάτη στο υπάρχον 2.7 Python ή Python 3.3 + περιβάλλον. Αυτό θα γίνει λήψη των πακέτων του από το [Ευρετήριο πακέτου Python][] (PyPI).

Ίσως χρειαστεί δικαιώματα διαχειριστή:

- Linux και MacOS, χρησιμοποιήστε το `sudo` εντολή: `sudo pip install azure-mgmt-compute`.
- Windows: Ανοίξτε το PowerShell/εντολών ως διαχειριστής

Μπορείτε να εγκαταστήσετε ξεχωριστά κάθε βιβλιοθήκη για κάθε υπηρεσία του Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Προεπισκόπηση πακέτων μπορεί να εγκατασταθεί χρησιμοποιώντας το `--pre` σημαία:

```console
   $ pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

Μπορείτε, επίσης, να εγκαταστήσετε ένα σύνολο Azure βιβλιοθήκες σε μια μονή γραμμή χρησιμοποιώντας το `azure` μετα-πακέτο. Εφόσον δεν όλα τα πακέτα σε αυτό το μετα-πακέτο δημοσιεύονται ως σταθερή ακόμη, το `azure` μετα-πακέτου εξακολουθεί να βρίσκεται σε προεπισκόπηση. Ωστόσο, τα πακέτα πυρήνα, από κώδικα ποιότητα/πληρότητα προοπτικές μπορούν να θεωρούνται "Σταθερό" αυτήν τη στιγμή
- Αυτό θα να επίσημα επισημαίνονται ως τέτοια σε συγχρονισμό με άλλες γλώσσες όσο το δυνατόν πιο σύντομα. Θα σας δεν προγραμματισμού στην κύρια περαιτέρω αλλαγές μέχρι τότε.

Εφόσον είναι μια έκδοση προεπισκόπησης, πρέπει να χρησιμοποιήσετε το `--pre` σημαία:

```console
   $ pip install --pre azure
```
   
ή απευθείας

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Λήψη περισσότερων πακέτων

Το [Ευρετήριο πακέτου Python][] (PyPI) περιλαμβάνει μια επιλογή εμπλουτισμένου Python βιβλιοθηκών.  Εάν επιλέξατε να εγκαταστήσετε μια Distro, θα έχετε ήδη περισσότερα από τα bit ενδιαφέρον για διάφορα σενάρια από το web ανάπτυξης για τεχνικές εφαρμογές πληροφορικής.


## <a name="python-tools-for-visual-studio"></a>Εργαλεία Python για το Visual Studio

[Εργαλεία Python για το Visual Studio][] (PTVS) είναι μια δωρεάν OSS Προσθήκη από τη Microsoft, η οποία μετατραπεί και στο σε ένα πλήρως ανεπτυγμένο IDE Python:

![How-σε-εγκατάσταση-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Χρήση PTVS είναι προαιρετικό, αλλά συνιστάται όπως σας προσφέρει υποστήριξη Python και έργου/λύση Web, τον εντοπισμό σφαλμάτων, δημιουργία προφίλ, παραθύρου αλληλεπίδρασης, επεξεργασία προτύπου και Intellisense.

PTVS επίσης καθιστά εύκολη την ανάπτυξη του Microsoft Azure, με την υποστήριξη για ανάπτυξη για [Τις υπηρεσίες Cloud][] και [τοποθεσίες Web που διαθέτετε][].

PTVS λειτουργεί με το υπάρχον Visual Studio 2013 ή με την εγκατάσταση 2015.  Για την τεκμηρίωση, στοιχεία λήψης και συζητήσεις, ανατρέξτε στο θέμα [Εργαλεία Python για το Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure σενάρια για Linux και MacOS

Για το Linux ή MacOS, κύριο Azure σενάρια που υποστηρίζονται:

1. Κατανάλωση υπηρεσιών Azure χρησιμοποιώντας τις βιβλιοθήκες προγράμματος-πελάτη για Python

2. Εκτέλεση της εφαρμογής σας σε μια Εικονική Linux

3. Ανάπτυξη και τη δημοσίευση σε τοποθεσίες Web Azure χρησιμοποιώντας Git

Το πρώτο σενάριο σάς επιτρέπει να συντάκτης εφαρμογές web εμπλουτισμένου που να εκμεταλλευτείτε τις δυνατότητες Azure PaaS όπως [χώρο αποθήκευσης αντικειμένων blob][], [ουρά χώρου αποθήκευσης][], [χώρος αποθήκευσης πινάκων][] κ.λπ., μέσω Pythonic προγράμματα εξομοίωσης για τα API ΥΠΌΛΟΙΠΑ Azure. Αυτά τα έργα τον ίδιο τρόπο στο Windows, Mac και Linux.  Μπορείτε επίσης να χρησιμοποιήσετε αυτές τις βιβλιοθήκες προγράμματος-πελάτη από τον υπολογιστή σας τοπικής ανάπτυξης ή μια Εικονική Linux εκτελείται σε Azure.

Για το σενάριο Εικονική, απλώς ξεκινήστε μια Εικονική Linux της επιλογής σας (Ubuntu, CentOS, Suse) και εκτέλεση/Διαχείριση τι σας αρέσει.  Ως παράδειγμα, μπορείτε να εκτελέσετε σε [][] IPython/Σημειωματάριο στον υπολογιστή σας Mac/Windows/Linux και να κατευθύνετε το πρόγραμμα περιήγησης σε Linux ή Εικονική πολλαπλών διεργασιών των Windows που εκτελείται ο μηχανισμός IPython σε Azure. Ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [IPython σημειωματαρίου στο Azure][] για περισσότερες πληροφορίες.

Για πληροφορίες σχετικά με τον τρόπο ρύθμισης μια Εικονική Linux, ανατρέξτε στο θέμα το πρόγραμμα εκμάθησης [Δημιουργία μια εικονική μηχανή εκτελείται Linux][] .

Χρήση Git ανάπτυξης, μπορείτε να αναπτύξετε μια εφαρμογή web Python και να δημοσιεύσετε σε μια τοποθεσία Web Azure από οποιοδήποτε λειτουργικό σύστημα.  Όταν πατάτε το αποθετήριο δεδομένων για να Azure, δημιουργεί αυτόματα ένα εικονικό περιβάλλον και pip εγκαταστήστε τις απαιτούμενες πακέτων.

Για περισσότερες πληροφορίες σχετικά με την ανάπτυξη και τη δημοσίευση Azure τοποθεσίες Web, ανατρέξτε στο θέμα τα προγράμματα εκμάθησης για [Τη δημιουργία τοποθεσιών Web με Django][], [Τη δημιουργία τοποθεσιών Web με μπουκάλι][]και [Τη δημιουργία τοποθεσιών Web με φιάλη][]. Για περισσότερες γενικές πληροφορίες σχετικά με τη χρήση οποιαδήποτε συμβατή με WSGI framework, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων Python με Azure τοποθεσίες Web][].


## <a name="additional-software-and-resources"></a>Πρόσθετο λογισμικό και τους πόρους:

* [Azure SDK για Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK για Python Github](https://github.com/Azure/azure-sdk-for-python)
* [Επίσημη Azure δείγματα για Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Συνέχεια ανάλυση Python διανομής][]
* [Κατανομή Enthought Python][]
* [Κατανομή ActiveState Python][]
* [SciPy - μιας οικογένειας προγραμμάτων επιστημονική Python βιβλιοθηκών][]
* [NumPy - μια βιβλιοθήκη αριθμητικές τιμές για Python][]
* [Django Project - ένα πλαίσιο/CMS ώριμη web][]
* [IPython - ένα σύνθετο Αντικατάσταση/σημειωματάριο για Python][]
* [IPython σημειωματαρίου στο Azure][]
* [Εργαλεία Python για το Visual Studio στο GitHub][]
* [Κέντρο για προγραμματιστές Python](/develop/python/)

[Συνέχεια ανάλυση Python διανομής]: http://continuum.io
[Κατανομή Enthought Python]: http://www.enthought.com
[Κατανομή ActiveState Python]: http://www.activestate.com
[www.Python.org]: http://www.python.org
[www.continuum.IO]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - μιας οικογένειας προγραμμάτων επιστημονική Python βιβλιοθηκών]: http://www.scipy.org
[NumPy - μια βιβλιοθήκη αριθμητικές τιμές για Python]: http://www.numpy.org
[Django Project - ένα πλαίσιο/CMS ώριμη web]: http://www.djangoproject.com
[IPython - ένα σύνθετο Αντικατάσταση/σημειωματάριο για Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython σημειωματαρίου στο Azure]: virtual-machines-linux-jupyter-notebook.md
[Υπηρεσίες cloud]: cloud-services-python-ptvs.md
[Τοποθεσίες Web]: web-sites-python-ptvs-django-mysql.md
[Εργαλεία Python για το Visual Studio]: http://aka.ms/ptvs
[Εργαλεία Python για το Visual Studio στο GitHub]: https://github.com/microsoft/ptvs
[Python πακέτου ευρετηρίου]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Δημιουργήστε μια εικονική μηχανή εκτελείται Linux]: virtual-machines-linux-quick-create-cli.md
[Δημιουργία τοποθεσιών Web με Django]: web-sites-python-create-deploy-django-app.md
[Δημιουργία τοποθεσιών Web με μπουκάλι]: web-sites-python-create-deploy-bottle-app.md
[Δημιουργία τοποθεσιών Web με φιάλη]: web-sites-python-create-deploy-flask-app.md
[Ρύθμιση παραμέτρων Python με Azure τοποθεσίες Web]: web-sites-python-configure.md
[χώρος αποθήκευσης πινάκων]: storage-python-how-to-use-table-storage.md
[ουρά χώρου αποθήκευσης]: storage-python-how-to-use-queue-storage.md
[χώρος αποθήκευσης αντικειμένων blob]: storage-python-how-to-use-blob-storage.md

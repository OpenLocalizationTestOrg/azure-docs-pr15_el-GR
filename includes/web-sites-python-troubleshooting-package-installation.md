Ορισμένα πακέτα δεν μπορεί να εγκαταστήσει χρησιμοποιώντας pip κατά την εκτέλεση στο Azure.  Απλώς, μπορεί να είναι ότι το πακέτο δεν είναι διαθέσιμη στο ευρετήριο πακέτου Python.  Μπορεί να είναι ότι απαιτείται ένα πρόγραμμα μεταγλώττισης (ένα πρόγραμμα μεταγλώττισης δεν είναι διαθέσιμη στον υπολογιστή που εκτελεί την εφαρμογή web στο Azure εφαρμογής υπηρεσίας).

Σε αυτήν την ενότητα, θα εξετάσουμε τρόπους για να αντιμετωπίσετε αυτό το ζήτημα.

### <a name="request-wheels"></a>Αίτηση λειτουργίας του τροχού

Εάν η εγκατάσταση του πακέτου απαιτεί ένα πρόγραμμα μεταγλώττισης, προσπαθείτε θα πρέπει να επικοινωνήσετε με τον κάτοχο του πακέτου για να ζητήσετε λειτουργίας του τροχού να γίνει διαθέσιμη για το πακέτο.

Με τη διαθεσιμότητα πρόσφατες του [Microsoft Visual C++ μεταγλώττισης για Python 2.7][], τώρα είναι πιο εύκολο να δημιουργείτε πακέτα, τα οποία έχετε τοπικό κώδικα για Python 2.7.

### <a name="build-wheels-requires-windows"></a>Δημιουργία λειτουργίας του τροχού (απαιτεί Windows)

Σημείωση: Όταν χρησιμοποιείτε αυτήν την επιλογή, βεβαιωθείτε ότι για να μεταγλωττίσετε το πακέτο χρησιμοποιώντας ένα περιβάλλον Python που ταιριάζει με την πλατφόρμα/αρχιτεκτονική/έκδοση που χρησιμοποιείται στην εφαρμογή web στο Azure εφαρμογής υπηρεσίας (Windows/32-bit/2.7 ή 3.4).

Εάν το πακέτο δεν έχει εγκατασταθεί επειδή απαιτεί ένα πρόγραμμα μεταγλώττισης, μπορείτε να εγκαταστήσετε το πρόγραμμα μεταγλώττισης στον τοπικό υπολογιστή σας και να δημιουργήσετε έναν τροχό για το πακέτο, το οποίο θα συμπεριλάβει στη συνέχεια στο αποθετήριο σας.

Χρήστες Mac/Linux: Εάν δεν έχετε πρόσβαση σε έναν υπολογιστή Windows, ανατρέξτε στο θέμα [Δημιουργία μια εικονική μηχανή με Windows][] για το πώς μπορείτε να δημιουργήσετε μια Εικονική σε Azure.  Μπορείτε να το χρησιμοποιήσετε για να δημιουργήσετε το λειτουργίας του τροχού, προσθέτετε στο αποθετήριο δεδομένων και να απορρίψετε την εικονική Μηχανή εάν θέλετε. 

Για Python 2.7, μπορείτε να εγκαταστήσετε [Microsoft Visual C++ μεταγλώττισης για Python 2.7][].

Για Python 3.4, μπορείτε να εγκαταστήσετε [Το Microsoft Visual C++ 2010 Express][].

Για να δημιουργήσετε λειτουργίας του τροχού, θα χρειαστεί το πακέτο του τροχού:

    env\scripts\pip install wheel

Θα χρησιμοποιήσετε `pip wheel` για να μεταγλωττίσετε εξάρτησης:

    env\scripts\pip wheel azure==0.8.4

Αυτό δημιουργεί ένα αρχείο .whl στο φάκελο \wheelhouse.  Προσθέστε τα αρχεία φακέλων και του τροχού \wheelhouse χώρου αποθήκευσης στο.

Επεξεργασία του requirements.txt για να προσθέσετε το `--find-links` την επιλογή στο επάνω μέρος. Αυτό σας ενημερώνει pip για να αναζητήσετε μια ακριβή αντιστοιχία στον τοπικό φάκελο πριν μεταβείτε στο ευρετήριο πακέτου python.

    --find-links wheelhouse
    azure==0.8.4

Εάν θέλετε να συμπεριλάβετε όλες τις εξαρτήσεις στο φάκελο \wheelhouse και δεν χρησιμοποιεί το ευρετήριο πακέτου python καθόλου, μπορείτε να επιβάλετε pip για να αγνοήσετε το ευρετήριο πακέτου με την προσθήκη `--no-index` στο επάνω μέρος του requirements.txt.

    --no-index

### <a name="customize-installation"></a>Προσαρμογή εγκατάστασης

Μπορείτε να προσαρμόσετε τη δέσμη ενεργειών ανάπτυξης για να εγκαταστήσετε ένα πακέτο το εικονικό περιβάλλον χρησιμοποιώντας μια εναλλακτική installer, όπως εύκολη\_εγκατάσταση.  Ανατρέξτε στο θέμα deploy.cmd για ένα παράδειγμα το οποίο είναι σχόλια.  Βεβαιωθείτε ότι τα πακέτα δεν είναι στη λίστα στο requirements.txt, για να αποτρέψετε την εγκατάσταση τους pip.

Προσθέστε αυτήν τη δέσμη ενεργειών ανάπτυξης:

    env\scripts\easy_install somepackage

Ενδέχεται επίσης να μπορούν να χρησιμοποιήσουν εύκολη\_εγκατάστασης για εγκατάσταση από ένα πρόγραμμα εγκατάστασης exe (ορισμένες είναι zip συμβατούς, τόσο εύκολη\_εγκατάσταση υποστηρίζει τους).  Προσθήκη το πρόγραμμα εγκατάστασης του αποθετηρίου και εύκολη κλήση\_εγκατάσταση, μεταφέροντας τη διαδρομή για το εκτελέσιμο αρχείο.

Προσθέστε αυτήν τη δέσμη ενεργειών ανάπτυξης:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-the-virtual-environment-in-the-repository-requires-windows"></a>Συμπεριλάβετε το εικονικό περιβάλλον στο αποθετήριο (απαιτεί Windows)

Σημείωση: Όταν χρησιμοποιείτε αυτήν την επιλογή, βεβαιωθείτε ότι χρησιμοποιείτε ένα εικονικό περιβάλλον που ταιριάζει με την πλατφόρμα/αρχιτεκτονική/έκδοση που χρησιμοποιείται στην εφαρμογή web στο Azure εφαρμογής υπηρεσίας (Windows/32-bit/2.7 ή 3.4).

Εάν συμπεριλάβετε το εικονικό περιβάλλον στο αποθετήριο δεδομένων, μπορείτε να αποτρέψετε τη δέσμη ενεργειών ανάπτυξης κάνοντας εικονικό περιβάλλον διαχείρισης στο Azure, δημιουργώντας ένα κενό αρχείο:

    .skipPythonDeployment

Συνιστάται να διαγράψετε το υπάρχον εικονικό περιβάλλον στην εφαρμογή του, ώστε να μην έχουν απομείνει αρχεία από όταν το εικονικό περιβάλλον ήταν διαχειριζόμενων αυτόματα.


[Δημιουργήστε μια εικονική μηχανή με Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Πρόγραμμα μεταγλώττισης Microsoft Visual C++ για Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949

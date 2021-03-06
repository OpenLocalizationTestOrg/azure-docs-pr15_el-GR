Η δέσμη ενεργειών ανάπτυξης θα παραλείπεται η δημιουργία το εικονικό περιβάλλον στη Azure, αν εντοπίσει ότι υπάρχει ήδη ένα συμβατό εικονικό περιβάλλον.  Αυτό μπορεί να επιταχύνει ανάπτυξης σημαντικά.  Πακέτα που είναι ήδη εγκατεστημένα θα παραλειφθούν από pip.

Σε ορισμένες περιπτώσεις, ίσως θέλετε να επιβάλετε διαγραφή συγκεκριμένο εικονικό περιβάλλον.  Θέλετε να το κάνετε αυτό, εάν αποφασίσετε να συμπεριλάβετε ένα εικονικό περιβάλλον ως μέρος του αποθετηρίου σας.  Μπορεί επίσης να θέλετε να το κάνετε αυτό, εάν πρέπει να καταργήσω ορισμένες πακέτων, ή να εξετάσετε τις αλλαγές σε requirements.txt.

Υπάρχουν μερικές επιλογές για να διαχειριστείτε το υπάρχον εικονικό περιβάλλον στους Azure:

### <a name="option-1-use-ftp"></a>Επιλογή 1: Χρήση FTP

Με ένα πρόγραμμα-πελάτη FTP, συνδεθείτε με το διακομιστή και θα έχετε τη δυνατότητα να διαγράψετε το φάκελο φάκελος.  Σημειώστε ότι ορισμένα προγράμματα-πελάτες FTP (όπως τα προγράμματα περιήγησης web) μπορεί να είναι μόνο για ανάγνωση και δεν θα σας επιτρέψει να διαγράψετε τους φακέλους, έτσι θα θέλετε να βεβαιωθείτε ότι χρησιμοποιείτε ένα πρόγραμμα-πελάτη FTP με αυτή τη δυνατότητα.  Το όνομα κεντρικού υπολογιστή FTP και χρήστη εμφανίζονται σε blade της εφαρμογής web σας στην [Πύλη του Azure](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Επιλογή 2: Εναλλαγή runtime

Ακολουθεί μια εναλλακτική λύση που αξιοποιεί το γεγονός ότι η δέσμη ενεργειών ανάπτυξης θα διαγράψτε το φάκελο φάκελος όταν δεν συμφωνεί με την επιθυμητή έκδοση του Python.  Αυτό αποτελεσματική θα διαγράψτε το υπάρχον περιβάλλον, και δημιουργήστε ένα νέο.

1. Μεταβείτε σε μια διαφορετική έκδοση του Python (μέσω runtime.txt ή το blade **Ρυθμίσεις εφαρμογής** στην πύλη του Azure)
1. git push ορισμένες αλλαγές (Παράβλεψη σφάλματα εγκατάστασης pip εάν υπάρχει)
1. Επιστρέψτε στην αρχική έκδοση του Python
1. git push ορισμένες αλλαγές ξανά

### <a name="option-3-customize-deployment-script"></a>Επιλογή 3: Προσαρμογή δέσμη ενεργειών ανάπτυξης

Εάν έχετε προσαρμόσει τη δέσμη ενεργειών ανάπτυξης, μπορείτε να αλλάξετε τον κωδικό στο deploy.cmd για να επιβάλετε το για να διαγράψετε το φάκελο φάκελος.

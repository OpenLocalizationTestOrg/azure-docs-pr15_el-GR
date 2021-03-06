<properties
    pageTitle="Συνήθεις Ερωτήσεις: Azure AD κωδικού πρόσβασης διαχείρισης | Microsoft Azure"
    description="Συνήθεις ερωτήσεις σχετικά με τη Διαχείριση κωδικών πρόσβασης στο Azure AD, συμπεριλαμβανομένων των επαναφοράς κωδικού πρόσβασης, την καταγραφή, αναφορές και αντιγραφής υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="password-management-frequently-asked-questions"></a>Διαχείριση κωδικών πρόσβασης συνήθεις ερωτήσεις

> [AZURE.IMPORTANT] **Είστε εδώ επειδή αντιμετωπίζετε προβλήματα κατά την είσοδο;** Εάν Ναι, [Μάθετε πώς μπορείτε να αλλάξετε και να επαναφέρετε το δικό σας κωδικό πρόσβασης](active-directory-passwords-update-your-own-password.md).

Ακολουθούν ορισμένες συνήθεις ερωτήσεις για όλα τα πράγματα που σχετίζονται με τη Διαχείριση κωδικών πρόσβασης.

Εάν δεν μπορείτε να το βρείτε στον εαυτό σας με μια ερώτηση που δεν γνωρίζετε την απάντηση ή αναζητάτε βοήθεια σχετικά με ένα συγκεκριμένο πρόβλημα που αντικριστές, μπορείτε να διαβάσετε στην κάτω από το στοιχείο για να δείτε εάν θα σας έχετε καλύπτεται το ήδη.  Μην ανησυχείτε εάν δεν θα σας κάνει ήδη! Μην διστάσεις να υποβάλετε οποιαδήποτε ερώτηση έχετε που δεν καλύπτεται εδώ στα [Φόρουμ του Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD) και θα σας θα να επιστρέψετε στην που μόλις μπορούμε.

Αυτές οι συνήθεις Ερωτήσεις χωρίζονται στις ακόλουθες ενότητες:

- [**Ερωτήσεις σχετικά με τη δήλωση επαναφορά κωδικού πρόσβασης**](#password-reset-registration)
- [**Ερωτήσεις σχετικά με την επαναφορά του κωδικού πρόσβασης**](#password-reset)
- [**Ερωτήσεις σχετικά με τις αναφορές διαχείρισης κωδικού πρόσβασης**](#password-management-reports)
- [**Ερωτήσεις σχετικά με τον κωδικό πρόσβασης αντιγραφής**](#password-writeback)

## <a name="password-reset-registration"></a>Καταχώρηση επαναφοράς κωδικού πρόσβασης
 - **Ε: μπορούν να τους χρήστες μου να καταχωρήσετε τα δικά τους δεδομένα επαναφορά κωδικού πρόσβασης;**

 > **A:** Ναι, με την προϋπόθεση ότι είναι ενεργοποιημένη η επαναφορά του κωδικού πρόσβασης και έχετε άδεια χρήσης, μπορούν να μεταβαίνουν στην πύλη του δήλωσης επαναφορά κωδικού πρόσβασης στη http://aka.ms/ssprsetup για να καταχωρήσετε τις πληροφορίες ελέγχου ταυτότητας που θα χρησιμοποιηθεί με την επαναφορά του κωδικού πρόσβασης. Οι χρήστες επίσης να καταχωρήσετε, μεταβαίνοντας στο πλαίσιο πρόσβασης στο http://myapps.microsoft.com, κάνοντας κλικ στην καρτέλα προφίλ και κάνοντας κλικ στην επιλογή αρχείο καταχωρήσεων για την επιλογή επαναφοράς κωδικού πρόσβασης. Μάθετε περισσότερα σχετικά με το πώς μπορείτε να τους χρήστες σας έχει ρυθμιστεί για διαβάζοντας πώς μπορείτε να προετοιμάσετε τους χρήστες που έχουν ρυθμιστεί για επαναφορά του κωδικού πρόσβασης για επαναφορά κωδικού πρόσβασης.

 - **Ε: μπορώ να ορίζουν δεδομένα επαναφορά του κωδικού πρόσβασης εκ μέρους τους χρήστες μου;**

 > **A:** Ναι, μπορείτε να το κάνετε με DirSync ή PowerShell ή μέσω της πύλης [Πύλη διαχείρισης Azure](https://manage.windowsazure.com) ή διαχείρισης του Office. Μάθετε περισσότερα σχετικά με αυτήν τη δυνατότητα στη δημοσίευση ιστολογίου βελτιωμένη προστασίας προσωπικών δεδομένων για το Azure AD MFA και οι αριθμοί τηλεφώνου επαναφορά κωδικού πρόσβασης και με ανάγνωσης μάθετε τον τρόπο χρήσης δεδομένων με κωδικό πρόσβασης επαναφέρετε.

 - **Ε: μπορώ να συγχρονίσω δεδομένων για ερωτήσεις ασφαλείας από εσωτερικής εγκατάστασης;**

 > **A:** Όχι, αυτό δεν είναι δυνατό σήμερα, αλλά θα σας το σκέφτεστε.

 - **Ε: μπορούν οι χρήστες μου καταχώρηση δεδομένων έτσι ώστε οι άλλοι χρήστες δεν είναι δυνατό να δείτε αυτά τα δεδομένα;**

 > **A:** Ναι, όταν οι χρήστες καταχώρηση δεδομένων με την πύλη εγγραφής επαναφορά κωδικού πρόσβασης του στοιχεία αποθηκεύονται σε πεδία ιδιωτικό ελέγχου ταυτότητας που είναι ορατές μόνο από καθολικοί διαχειριστές και ο χρήστης μόνος του. Μάθετε περισσότερα σχετικά με αυτήν τη δυνατότητα στη δημοσίευση ιστολογίου βελτιωμένη προστασίας προσωπικών δεδομένων για το Azure AD MFA και οι αριθμοί τηλεφώνου επαναφορά κωδικού πρόσβασης και με ανάγνωσης μάθετε τον τρόπο χρήσης δεδομένων με κωδικό πρόσβασης επαναφέρετε.

 - **Ε: οι χρήστες μου πρέπει να έχει εγγραφεί μπορέσουν να χρησιμοποιήσουν την επαναφορά του κωδικού πρόσβασης;**

 > **A:** Όχι, αν ορίσετε αρκετές πληροφορίες ελέγχου ταυτότητας για λογαριασμό τους, οι χρήστες δεν θα χρειαστεί να καταχωρήσετε. Επαναφορά του κωδικού πρόσβασης θα λειτουργεί κανονικά εφόσον που έχουν μορφοποιηθεί σωστά δεδομένα που είναι αποθηκευμένα στα κατάλληλα πεδία στον κατάλογο. Μάθετε περισσότερα σχετικά με την με την ανάγνωση μάθετε τον τρόπο χρήσης δεδομένων με την επαναφορά του κωδικού πρόσβασης.

 - **Ε: μπορώ να συγχρονίσετε ή να ορίστε τα πεδία ελέγχου ταυτότητας ηλεκτρονικού ταχυδρομείου ή εναλλακτικός αριθμός τηλεφώνου ελέγχου ταυτότητας, έλεγχος ταυτότητας τηλέφωνο εκ μέρους τους χρήστες μου;**

 > **A:** Δεν αυτήν τη στιγμή, αλλά θα σας σκέφτεστε ενεργοποίηση αυτήν τη δυνατότητα.

 - **Ε: πώς γνωρίζει την πύλη εγγραφής ποιες επιλογές για να εμφανίσετε τους χρήστες μου;**

 > **A:** Πύλη εγγραφής επαναφορά του κωδικού πρόσβασης εμφανίζει μόνο τις επιλογές που έχετε ενεργοποιήσει για τους χρήστες σας κάτω από την ενότητα πολιτικής επαναφορά κωδικού πρόσβασης χρήστη στην καρτέλα ρύθμιση παραμέτρων του καταλόγου σας. Αυτό σημαίνει ότι εάν δεν ενεργοποιήσετε, πείτε ερωτήσεις ασφαλείας, στη συνέχεια, οι χρήστες δεν θα μπορείτε να εγγραφείτε για αυτήν την επιλογή.

 - **Ε: όταν θεωρείται χρήστη έχουν καταχωρηθεί;**

 > **A:** Ένας χρήστης θεωρείται καταχωρηθεί όταν ο συνάδελφός διαθέτει τουλάχιστον N κομμάτια πληροφορίες ελέγχου ταυτότητας που ορίζονται από το, όπου N είναι το αριθμό του απαιτείται έλεγχος ταυτότητας μεθόδους που έχετε ορίσει στην [Πύλη διαχείρισης του Azure](https://manage.windowsazure.com). Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα Προσαρμογή της πολιτικής κωδικού χρήστη επαναφορά.


## <a name="password-reset"></a>Επαναφορά του κωδικού πρόσβασης

 - **Ε: πόσο χρόνο πρέπει να περιμένω για να λάβετε ένα μήνυμα ηλεκτρονικού ταχυδρομείου, SMS ή τηλεφωνική κλήση από επαναφορά του κωδικού πρόσβασης;**

 > **A:** Μήνυμα ηλεκτρονικού ταχυδρομείου, SMS μηνύματα και τηλεφωνικές κλήσεις θα πρέπει να παραδίδεται στο στην περιοχή 1 λεπτό, με την κανονική υπόθεση που 5-20 δευτερόλεπτα. Εάν δεν λάβετε ειδοποίηση σε αυτό το χρονικό πλαίσιο, ελέγξτε το φάκελο ανεπιθύμητης αλληλογραφίας, που ο αριθμός / την επικοινωνία με του ηλεκτρονικού ταχυδρομείου είναι αυτό που περιμένατε και ότι τα δεδομένα ελέγχου ταυτότητας στον κατάλογο έχει μορφοποιηθεί σωστά. Για να μάθετε περισσότερα σχετικά με τη μορφοποίηση αριθμών τηλεφώνου και το ηλεκτρονικό ταχυδρομείο διευθύνσεις για χρήση με την επαναφορά του κωδικού πρόσβασης ανατρέξτε στην ενότητα μάθετε τον τρόπο χρήσης δεδομένων με την επαναφορά του κωδικού πρόσβασης.

 - **Ε: ποιες γλώσσες υποστηρίζονται από επαναφορά του κωδικού πρόσβασης;**

 > **A:** Επαναφορά του κωδικού πρόσβασης περιβάλλοντος εργασίας Χρήστη, μηνύματα SMS και τις κλήσεις φωνής μεταφράζονται σε τις ίδιες 40 γλώσσες που υποστηρίζονται στο Office 365. Αυτές είναι: Αραβικά, βουλγαρικά, απλοποιημένα κινεζικά, Κινεζικά, παραδοσιακά κινεζικά, Κροατικά, Τσεχικά, δανικά, ολλανδικά, Αγγλικά, εσθονικά, φινλανδικά, γαλλικά, γερμανικά, ελληνικά, εβραϊκά, Χίντι, ουγγρικά, ινδονησιακά, ιταλικά, ιαπωνικά, καζαχστανικά, κορεατικά, Λετονικά, Λιθουανικά, μαλαϊκά (Μαλαισίας), νορβηγικά (Bokmål), πολωνικά, πορτογαλικά (Βραζιλίας), πορτογαλικά (Πορτογαλίας), Ρουμανικά, ρωσικά, Σερβικά (Λατινικά), Σλοβακικά, Σλοβενικά, Ισπανικά, σουηδικά, ταϊλανδικά, Τουρκικά, Ουκρανικά, και βιετναμικά.

 - **Ε: ποια μέρη της εμπειρίας επαναφορά του κωδικού πρόσβασης να δημιουργήσει εμπορική προσαρμογή όταν ρυθμίζω την εταιρική εμπορικής προσαρμογής μου καταλόγου του ρυθμίστε τις παραμέτρους της καρτέλας;**

 > **A:** Πύλη επαναφορά του κωδικού πρόσβασης θα εμφανίσει το εταιρικό λογότυπο σας και σας επιτρέπει επίσης να ρυθμίσετε τις παραμέτρους της επαφής σας σύνδεση διαχειριστή ώστε να οδηγούν σε ένα προσαρμοσμένο μήνυμα ηλεκτρονικού ταχυδρομείου ή διεύθυνση URL. Οποιοδήποτε μήνυμα ηλεκτρονικού ταχυδρομείου που αποστέλλονται με την επαναφορά του κωδικού πρόσβασης θα περιλαμβάνει το λογότυπο της εταιρείας σας, χρώματα (σε αυτήν την περίπτωση κόκκινο), όνομα στο σώμα του μηνύματος ηλεκτρονικού ταχυδρομείου, και προσαρμογή του από το όνομα. Δείτε ένα παράδειγμα με όλα τα εμπορική επωνυμία στοιχεία που παρακάτω. Για περισσότερες πληροφορίες, διαβάστε την επαναφορά του κωδικού πρόσβασης προσαρμογή εμφάνισης και της αίσθησης.

  ![][001]

 - **Ε: πώς να μπορούν να Εκπαιδεύστε τους χρήστες μου σχετικά με το τι να κάνετε για να επαναφέρετε τους κωδικούς πρόσβασης;**

 > **A:** Μπορείτε να στείλετε τους χρήστες σας να https://passwordreset.microsoftonline.com απευθείας, ή μπορείτε να ζητήσετε να κάντε κλικ στην εντολή την πρόσβαση δεν είναι δυνατή η σύνδεση λογαριασμός περιλαμβάνεται σε οποιαδήποτε σχολείο ή το Αναγνωριστικό εργασίας στην οθόνη εισόδου. Που μπορεί να μην διστάσεις να δημοσιεύσετε αυτές τις συνδέσεις (ή δημιουργία διεύθυνσης URL ανακατευθύνει τους) σε οποιαδήποτε θέση στην οποία είναι εύκολα προσβάσιμο στους χρήστες σας.

 - **Ε: μπορώ να χρησιμοποιήσω αυτήν τη σελίδα από μια κινητή συσκευή;**

 > **A:** Ναι, η σελίδα αυτή λειτουργεί σε κινητές συσκευές.

 - **Ε: υποστηρίζετε ξεκλείδωμα λογαριασμούς τοπική υπηρεσία καταλόγου active directory όταν οι χρήστες η επαναφορά τους κωδικούς πρόσβασης;**

 > **A:** Ναι, όταν ένας χρήστης επαναφέρει την τον κωδικό πρόσβασης και τον κωδικό πρόσβασης αντιγραφής έχει ανεπτυγμένος με όλες τις εκδόσεις του Azure AD Connect ή εκδόσεις του Azure AD Sync 1.0.0485.0222 ή νεότερη έκδοση, τότε το λογαριασμό του χρήστη θα είναι μη κλειδωμένα αυτόματα όταν αυτόν το χρήστη να επαναφέρει τον κωδικό πρόσβασής του.

 - **Ε: πώς μπορώ να ενσωματώσω απευθείας στο του χρήστη μου υπολογιστή εμπειρία εισόδου επαναφορά κωδικού πρόσβασης;**

 > **A:** Αυτό δεν είναι δυνατό σήμερα. Ωστόσο, εάν απολύτως χρειάζεστε αυτήν τη δυνατότητα και είστε πελάτης Azure AD Premium, μπορείτε να εγκαταστήσετε Διαχείριση ταυτοτήτων Microsoft χωρίς πρόσθετο κόστος και ανάπτυξη της λύσης επαναφορά κωδικού πρόσβασης στην εσωτερική εγκατάσταση βρίσκονται εκεί για να επιλύσετε αυτήν την απαίτηση.

 - **Ε: μπορώ να ορίσω ερωτήσεις διαφορετικό ασφαλείας για διαφορετικές τοπικές ρυθμίσεις;**

 > **A:** Όχι, αυτό δεν είναι δυνατό σήμερα, αλλά θα σας το σκέφτεστε.

 - **Ε: πώς πολλά ερωτήματα που θα σας να ρυθμίσετε για την επιλογή ελέγχου ταυτότητας ερωτήσεις ασφαλείας;**

 > **A:** Μπορείτε να ρυθμίσετε έως και 20 προσαρμοσμένες ασφαλείας ερωτήσεις στην [Πύλη διαχείρισης του Azure](https://manage.windowsazure.com).

 - **Ε: πώς μεγάλη ερωτήσεις ασφαλείας ίσως;**

 > **A:** Ερωτήσεις ασφαλείας ενδέχεται να είναι από 3 έως 200 χαρακτήρες.

 - **Ε: πώς μεγάλο μπορεί να βρείτε απαντήσεις σε ερωτήσεις ασφαλείας είναι;**

 > **A:** Απαντήσεις μπορεί να είναι 3 έως 40 χαρακτήρες.

 - **Ε: υπάρχουν διπλότυπα απαντήσεις σε ερωτήσεις ασφαλείας απορριφθεί;**

 > **A:** Ναι, απόρριψη διπλότυπων απαντήσεις σε ερωτήσεις ασφαλείας.

 - **Ε: Μάιος χρήστη καταχώρηση περισσότερες από μία από την ίδια ερώτηση ασφαλείας;**

 > **A:** Όχι, όταν ένας χρήστης καταχωρεί μια συγκεκριμένη ερώτηση, ο συνάδελφός ενδέχεται να μην καταχωρήσει για αυτήν την ερώτηση δεύτερη φορά.

 - **Ε: είναι δυνατόν να ορίσετε ένα ελάχιστο όριο ερωτήσεις ασφαλείας για καταχώρηση και επαναφορά;**

 > **A:** Ναι, ένα όριο μπορεί να ρυθμιστεί για την καταχώρηση και ένα άλλο για επαναφορά. ερωτήσεις ασφαλείας 3-5 ενδεχομένως να απαιτούνται για την καταχώρηση και 3-5 μπορεί να απαιτείται για επαναφορά.

 - **Ε: Εάν ένας χρήστης έχει καταχωρηθεί περισσότερους χαρακτήρες από τον μέγιστο αριθμό ερωτήσεις που απαιτούνται για την επαναφορά, τον τρόπο ερωτήσεις ασφαλείας επιλογής στη διάρκεια της επαναφοράς;**

 > **A:** Ερωτήσεις ασφαλείας N επιλέγονται τυχαία από τον συνολικό αριθμό των ερωτήσεων ένας χρήστης έχει καταχωρηθεί για, όπου N είναι ο ελάχιστος αριθμός ερωτήσεις που απαιτείται για την επαναφορά του κωδικού πρόσβασης. Για παράδειγμα, εάν ένας χρήστης έχει 5 ερωτήσεις ασφαλείας που έχουν καταχωρηθεί, αλλά μόνο 3 απαιτούνται για να επαναφέρετε, 3 από αυτές τις 5 θα επιλέγεται τυχαία και θα που παρουσιάζονται στο χρήστη τη στιγμή της επαναφοράς. Εάν ο χρήστης λαμβάνει τις απαντήσεις στις ερωτήσεις πρόβλημα, η διαδικασία επιλογής επανεμφανίζεται ώστε να μην hammering ερώτηση.

 - **Ε: που αποτρέψω χρηστών προσπάθεια πολλές φορές σε μια μικρή χρονική περίοδο επαναφορά κωδικού πρόσβασης;**

 > **A:** Ναι, υπάρχουν πολλές ενσωματωμένες δυνατότητες ασφαλείας στο επαναφορά του κωδικού πρόσβασης. Οι χρήστες ενδέχεται να προσπαθήσει μόνο 5 προσπάθειες επαναφορά κωδικού πρόσβασης μέσα σε μία ώρα πριν από την κλειδωμένος 24 ώρες. Οι χρήστες ενδέχεται να προσπαθήσει μόνο για την επικύρωση έναν αριθμό τηλεφώνου 5 φορές μέσα σε μία ώρα πριν από την κλειδωμένος 24 ώρες. Οι χρήστες ενδέχεται να προσπαθήσει μόνο μεθόδου ελέγχου ταυτότητας 5 φορές μέσα σε μία ώρα πριν από την κλειδωμένος 24 ώρες.

 - **Ε: για το χρονικό διάστημα ισχύουν του ηλεκτρονικού ταχυδρομείου και κωδικό πρόσβασης μίας φοράς SMS;**

 > **A:** Η διάρκεια ζωής περίοδο λειτουργίας για επαναφορά του κωδικού πρόσβασης είναι 105 λεπτά. Αυτό σημαίνει ότι, από την αρχή του κωδικού πρόσβασης, επαναφέρετε τη λειτουργία, ο χρήστης έχει 105 λεπτά για να επαναφέρετε τον κωδικό πρόσβασης. Το μήνυμα ηλεκτρονικού ταχυδρομείου και κωδικό πρόσβασης μίας φοράς SMS είναι έγκυρες αφού λήξει η συγκεκριμένη χρονική περίοδο.


## <a name="password-management-reports"></a>Αναφορές διαχείρισης κωδικού πρόσβασης

 - **Ε: πόσος χρόνος χρειάζεται για τα δεδομένα για να εμφανίζονται στην τις αναφορές διαχείρισης κωδικού πρόσβασης;**

 > **A:** Δεδομένα πρέπει να εμφανίζονται στις εκθέσεις διαχείρισης κωδικού πρόσβασης μέσα σε 5-10 λεπτά. Το μερικές φορές μπορεί να καταλάβει σε ώρα να εμφανίζονται.

 - **Ε: πώς μπορώ να φιλτράρω τις αναφορές διαχείρισης κωδικού πρόσβασης;**

 > **A:** Μπορείτε να φιλτράρετε τις αναφορές διαχείρισης κωδικού πρόσβασης, κάνοντας κλικ στο μικρό Μεγεθυντικό φακό ακραίες δεξιά από τις ετικέτες των στηλών, προς το επάνω μέρος της αναφοράς (δείτε το στιγμιότυπο οθόνης). Εάν θέλετε να κάνετε πιο πλούσια φιλτράρισμα, μπορείτε να κάνετε λήψη της αναφοράς του excel και δημιουργήστε έναν Συγκεντρωτικό πίνακα.

  ![][002]

 - **Ε: Τι είναι ο μέγιστος αριθμός γεγονότων είναι αποθηκευμένα στις αναφορές διαχείρισης κωδικού πρόσβασης;**

 > **A:** Έως 1.000 τον κωδικό πρόσβασης τον κωδικό πρόσβασης ή επαναφορά συμβάντα δήλωσης επαναφορά αποθηκεύονται στις αναφορές διαχείρισης κωδικού πρόσβασης.  Εργαζόμαστε για να αναπτύξετε αυτόν τον αριθμό για να συμπεριλάβετε περισσότερες συμβάντα.

 - **Ε: πόση πίσω μεταβείτε τις αναφορές διαχείρισης κωδικού πρόσβασης;**

 > **A:** Οι αναφορές διαχείρισης κωδικού πρόσβασης εμφάνιση των λειτουργιών που πραγματοποιούνται από τις τελευταίες 30 ημέρες. Προς το παρόν Διερεύνηση πώς μπορείτε να ορίσετε τη μεγαλύτερης χρονικής περιόδου. Προς το παρόν, εάν θέλετε να αρχειοθετήσετε αυτά τα δεδομένα, μπορείτε να λήψη περιοδικά τις αναφορές και αποθηκεύσετε τους σε διαφορετική θέση.

 - **Ε: υπάρχει ένα μέγιστο αριθμό γραμμών που μπορούν να εμφανίζονται στις εκθέσεις διαχείρισης κωδικού πρόσβασης;**

 > **A:** Ναι, ενδέχεται να εμφανιστεί έως 1.000 γραμμών σε οποιαδήποτε από τις αναφορές Διαχείριση κωδικών πρόσβασης, εάν που εμφανίζονται στο περιβάλλον εργασίας Χρήστη ή τη λήψη. Διερεύνηση αυτήν τη στιγμή τρόπος για να αυξήσετε αυτό το όριο.

 - **Ε: υπάρχει ένα API για να αποκτήσετε πρόσβαση την επαναφορά του κωδικού πρόσβασης ή την καταγραφή δεδομένα αναφοράς;**

 > **A:** Ναι, ανατρέξτε στο θέμα την ακόλουθη τεκμηρίωση για να μάθετε πώς μπορείτε να αποκτήσετε πρόσβαση την επαναφορά του κωδικού πρόσβασης αναφοράς ροής δεδομένων.  [Μάθετε πώς μπορείτε να αποκτήσετε πρόσβαση κωδικού πρόσβασης επαναφορά αναφοράς συμβάντα μέσω προγραμματισμού](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).

## <a name="password-writeback"></a>Αντιγραφή κωδικού πρόσβασης
 - **Ε: πώς λειτουργεί η αντιγραφής τον κωδικό πρόσβασης στο παρασκήνιο;**

 > **A:** Ανατρέξτε στο θέμα [Πώς κωδικού πρόσβασης αντιγραφής συνεργάζεται](active-directory-passwords-learn-more.md#how-password-writeback-works) για μια αναλυτική εξήγηση των τι συμβαίνει όταν ενεργοποιείτε αντιγραφής τον κωδικό πρόσβασης, καθώς και τον τρόπο ρέει δεδομένων μέσω του συστήματος πίσω στο περιβάλλον εσωτερικής εγκατάστασης. Δείτε λειτουργεί [μοντέλο ασφάλειας αντιγραφής τον κωδικό πρόσβασης](active-directory-passwords-learn-more.md#password-writeback-security-model) στο πώς αντιγραφής τον κωδικό πρόσβασης για να μάθετε πώς θα σας διασφαλίζει ότι η αντιγραφής κωδικού πρόσβασης είναι ιδιαίτερα ασφαλής υπηρεσίας.

 - **Ε: πώς αντιγραφής κωδικού πρόσβασης διαρκέσει η εργασία;  Υπάρχει μια καθυστέρηση συγχρονισμού όπως με το συγχρονισμό κατακερματισμός τον κωδικό πρόσβασης;**

 > **A:** Κωδικός πρόσβασης αντιγραφής είναι άμεσα. Είναι μια συγχρονισμένη διοχέτευσης που λειτουργεί ουσιαστικά με διαφορετικό τρόπο από τον κωδικό πρόσβασης κατακερματισμός συγχρονισμού. Αντιγραφής κωδικού πρόσβασης επιτρέπει στους χρήστες να λάβετε πραγματικού χρόνου σχόλια σχετικά με την επιτυχία της την επαναφορά του κωδικού πρόσβασης ή να αλλάξετε τη λειτουργία. Ο μέσος χρόνος για μια επιτυχημένη αντιγραφής ενός κωδικού πρόσβασης είναι στην περιοχή 500 ms.

 - **Ε: Τι τύπους λογαριασμών αντιγραφής κωδικού πρόσβασης λειτουργεί για;**

 > **A:** Λειτουργεί αντιγραφής τον κωδικό πρόσβασης για ομόσπονδων και συγχρονισμό κωδικού πρόσβασης κατακερματισμός οποίους θα τους χρήστες.

 - **Ε: αντιγραφής τον κωδικό πρόσβασης να επιβάλετε πολιτικές κωδικού πρόσβασης του τομέα μου;**

 > **A:** Ναι, τον κωδικό πρόσβασης αντιγραφής επιβάλλει ηλικία κωδικού πρόσβασης, το ιστορικό, πολυπλοκότητα, φίλτρα και τυχόν άλλες περιορισμού μπορεί να τοποθετείτε στο σημείο τους κωδικούς πρόσβασης στον τοπικό τομέα σας.

 - **Ε: είναι ασφαλής αντιγραφής τον κωδικό πρόσβασης;  Πώς μπορώ να είμαι βέβαιος που δεν θα λάβετε έγινε εισβολή;**

 > **A:** Ναι, αντιγραφής κωδικός πρόσβασης είναι πολύ ασφαλής. Για περισσότερες πληροφορίες σχετικά με τα επίπεδα ασφαλείας, 4 υλοποιηθεί από την υπηρεσία αντιγραφής τον κωδικό πρόσβασης, ανατρέξτε στο θέμα το [μοντέλο ασφάλειας αντιγραφής τον κωδικό πρόσβασης](active-directory-passwords-learn-more.md#password-writeback-security-model) στο works Τρόπος αντιγραφής τον κωδικό πρόσβασης.




## <a name="links-to-password-reset-documentation"></a>Συνδέσεις με κωδικό πρόσβασης επαναφορά τεκμηρίωση
Παρακάτω υπάρχουν συνδέσεις σε όλες τις σελίδες τεκμηρίωση για επαναφορά του κωδικού πρόσβασης Azure AD:

* **Είστε εδώ επειδή αντιμετωπίζετε προβλήματα κατά την είσοδο;** Εάν Ναι, [Μάθετε πώς μπορείτε να αλλάξετε και να επαναφέρετε το δικό σας κωδικό πρόσβασης](active-directory-passwords-update-your-own-password.md).
* [**Πώς λειτουργεί**](active-directory-passwords-how-it-works.md) - μάθετε περισσότερα σχετικά με τα έξι διάφορα στοιχεία της υπηρεσίας και τι σημαίνει κάθε
* [**Γρήγορα αποτελέσματα**](active-directory-passwords-getting-started.md) - μάθετε πώς μπορείτε να επιτρέπει στους χρήστες να επαναφέρετε και να αλλάξετε τους κωδικούς πρόσβασής τους cloud ή εσωτερικής εγκατάστασης
* [**Προσαρμογή**](active-directory-passwords-customize.md) - μάθετε πώς μπορείτε να προσαρμόσετε την εμφάνιση και αίσθηση και τη συμπεριφορά της υπηρεσίας για τις ανάγκες της εταιρείας σας
* [**Βέλτιστες πρακτικές**](active-directory-passwords-best-practices.md) - μάθετε τον τρόπο ανάπτυξης γρήγορη και αποτελεσματική διαχείριση κωδικών πρόσβασης στην εταιρεία σας
* [**Λήψη ιδεών**](active-directory-passwords-get-insights.md) - μάθετε περισσότερα σχετικά με το ενσωματωμένες δυνατότητες αναφοράς
* [**Αντιμετώπιση προβλημάτων**](active-directory-passwords-troubleshoot.md) - μάθετε τον τρόπο αντιμετώπισης προβλημάτων με την υπηρεσία γρήγορα
* [**Μάθετε περισσότερα**](active-directory-passwords-learn-more.md) - Επιλέξτε βάθος των τεχνικών λεπτομερειών πώς λειτουργεί η υπηρεσία


[001]: ./media/active-directory-passwords-faq/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-faq/002.jpg "Image_002.jpg"

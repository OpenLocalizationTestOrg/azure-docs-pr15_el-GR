<properties
   pageTitle="Azure AD Connect: Ιστορικό εκδόσεων κυκλοφορίας | Microsoft Azure"
   description="Αυτό το θέμα παραθέτει όλες τις εκδόσεις του Azure AD Connect και Azure AD Sync"
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Ιστορικό εκδόσεων κυκλοφορίας

Την ομάδα των υπηρεσιών Azure Active Directory ενημερώνεται τακτικά Azure AD Connect με νέες δυνατότητες και λειτουργίες. Δεν είναι όλες οι προσθήκες ισχύουν για όλα τα ακροατήρια.

Σε αυτό το άρθρο έχει σχεδιαστεί για να σας βοηθήσει να παρακολουθείτε τις εκδόσεις που έχουν κυκλοφορήσει και να καταλάβετε εάν πρέπει να ενημερώσετε την τελευταία έκδοση ή όχι.

Αυτή είναι η λίστα των σχετικών θεμάτων:

Το θέμα |  
--------- | --------- |
Βήματα για να κάνετε αναβάθμιση από το Azure AD Connect | Διαφορετικές μεθόδους για να [αναβάθμιση από προηγούμενη έκδοση για τις πιο πρόσφατες](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect κυκλοφορίας.
Απαιτούμενα δικαιώματα | Για τα δικαιώματα που απαιτούνται για να εφαρμόσετε μια ενημερωμένη έκδοση, ανατρέξτε στο θέμα [λογαριασμούς και δικαιώματα](./connect/active-directory-aadconnect-accounts-permissions.md#upgrade)
Λήψη| [Σύνδεση λήψης Azure AD](http://go.microsoft.com/fwlink/?LinkId=615771)

## <a name="112810"></a>1.1.281.0
Κυκλοφορήσει: Αυγούστου 2016

**Σταθερή θέματα:**

- Αλλαγές για να συγχρονίσετε το χρονικό διάστημα δεν πραγματοποιείται μέχρι αφού ολοκληρωθεί η επόμενη περίοδο συγχρονισμού.
- Azure AD Connect οδηγού δεν αποδέχεται Azure AD λογαριασμό του οποίου το όνομα χρήστη που ξεκινά με ένα χαρακτήρα υπογράμμισης (\_).
- Οδηγός Azure AD Connect αποτυγχάνει για τον έλεγχο ταυτότητας λογαριασμού Azure AD που παρέχεται εάν ο κωδικός πρόσβασης λογαριασμού περιέχει πάρα πολλά ειδικούς χαρακτήρες. Μήνυμα σφάλματος "δεν είναι δυνατή η επικύρωση διαπιστευτήρια. Ένα μη αναμενόμενο σφάλμα." επιστρέφεται.
- Κατάργηση της εγκατάστασης του ενδιάμεσου διακομιστή απενεργοποιεί το συγχρονισμό κωδικού πρόσβασης στο μισθωτή Azure AD και προκαλεί συγχρονισμό κωδικού πρόσβασης για να αποτύχει με active server.
- Συγχρονισμό κωδικού πρόσβασης αποτυγχάνει σε ασυνήθιστο περιπτώσεις, όταν υπάρχει χωρίς κατακερματισμός τον κωδικό πρόσβασης που είναι αποθηκευμένα στον χρήστη.
- Όταν είναι ενεργοποιημένη η Azure AD Connect server για τη λειτουργία δημιουργίας σταδίων, δεν είναι προσωρινά απενεργοποιημένη αντιγραφής τον κωδικό πρόσβασης.
- Azure AD Connect οδηγού δεν εμφανίζει το συγχρονισμό πραγματική κωδικού πρόσβασης και τη ρύθμιση παραμέτρων αντιγραφής τον κωδικό πρόσβασης όταν ο διακομιστής είναι σε λειτουργία ενδιάμεσου. Πάντα εμφανίζει τους ως απενεργοποιημένη.
- Αλλαγές στη ρύθμιση παραμέτρων για συγχρονισμό κωδικού πρόσβασης και τον κωδικό πρόσβασης αντιγραφής δεν διατηρούνται κατά Azure AD Connect οδηγού, όταν ο διακομιστής είναι σε λειτουργία ενδιάμεσου.

**Βελτιώσεις:**

- Ενημερωμένη Έναρξη-ADSyncSyncCycle cmdlet για να υποδείξετε αν είναι μπορούν να ξεκινούν με επιτυχία έναν νέο κύκλο συγχρονισμού ή όχι.
- Προστέθηκε διακοπή ADSyncSyncCycle cmdlet για τον τερματισμό του συγχρονισμού κύκλος και λειτουργία που είναι σε εξέλιξη.
- Ενημερωμένο cmdlet διακοπή ADSyncScheduler για τον τερματισμό του συγχρονισμού κύκλος και λειτουργία που είναι σε εξέλιξη.
- Όταν ρυθμίζετε τις παραμέτρους [Επεκτάσεις καταλόγου](active-directory-aadconnectsync-feature-directory-extensions.md) Azure AD Connect οδηγού, τώρα μπορείτε να επιλέξετε χαρακτηριστικό AD του τύπου "Συμβολοσειρά Teletex".

## <a name="111890"></a>1.1.189.0
Κυκλοφορήσει: 2016 Ιουνίου

**Σταθερή θέματα και βελτιώσεις:**

- Azure AD Connect μπορεί να έχει εγκατασταθεί σε ένα διακομιστή συμβατή με FIPS.
    - Για το συγχρονισμό κωδικού πρόσβασης, ανατρέξτε στο θέμα [συγχρονισμό κωδικού πρόσβασης και FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips)
- Σταθερή ένα ζήτημα όπου ένα όνομα NetBIOS δεν ήταν δυνατή η επίλυση προς το FQDN στο του Active Directory Connector.

## <a name="111800"></a>1.1.180.0
Κυκλοφορήσει: 2016 Μάιος

**Νέες δυνατότητες:**

- Προειδοποιεί τους και σας βοηθά να επαλήθευση τομέων, εάν δεν μπορείτε να το κάνετε πριν από την εκτέλεση Azure AD Connect.
- Υποστήριξη που προστέθηκε για [Γερμανία Microsoft Cloud](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
- Υποστήριξη που προστέθηκε για την πιο πρόσφατη υποδομής [cloud για δημόσιους οργανισμούς Microsoft Azure](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) με τις απαιτήσεις νέα διεύθυνση URL.

**Σταθερή θέματα και βελτιώσεις:**

- Η προσθήκη φιλτραρίσματος στο πρόγραμμα επεξεργασίας κανόνα συγχρονισμού ώστε να μπορείτε εύκολα να βρείτε κανόνες συγχρονισμού.
- Βελτιωμένες επιδόσεις κατά τη διαγραφή χώρο γραμμή σύνδεσης.
- Σταθερή μια προβλήματα όταν το ίδιο αντικείμενο τόσο διαγράφηκε και προστίθενται στο ίδιο Εκτέλεση (που ονομάζεται διαγραφή/Προσθήκη).
- Κανόνα απενεργοποίησης συγχρονισμός θα είναι πλέον εκ νέου ενεργοποίηση περιλαμβάνονται αντικείμενα και χαρακτηριστικά σε σχήμα αναβάθμισης "ή" Κατάλογος ανανέωση.

## <a name="111300"></a>1.1.130.0
Κυκλοφορήσει: 2016 Απρίλιος

**Νέες δυνατότητες:**

- Υποστήριξη που προστέθηκε για πολλαπλών τιμών χαρακτηριστικά [Επεκτάσεις καταλόγου](active-directory-aadconnectsync-feature-directory-extensions.md).
- Υποστήριξη που προστέθηκε για περισσότερες παραλλαγές ρύθμισης παραμέτρων για [Αυτόματη αναβάθμιση](active-directory-aadconnect-feature-automatic-upgrade.md) πρέπει να θεωρούνται απαιτήσεις καταλληλότητας για αναβάθμιση.
- Η προσθήκη ορισμένες cmdlet για [προσαρμοσμένο χρονοδιάγραμμα](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Κυκλοφορήσει: 2016 Μάρτιος

**Σταθερή θέματα:**

- Κάνατε ότι Express εγκατάστασης δεν μπορεί να χρησιμοποιηθεί σε Windows Server 2008 (προ-R2) από συγχρονισμό κωδικού πρόσβασης δεν υποστηρίζεται σε αυτό το λειτουργικό σύστημα.
- Αναβάθμιση από DirSync με τη ρύθμιση παραμέτρων ενός προσαρμοσμένου φίλτρου δεν λειτούργησε όπως αναμένεται.
- Κατά την αναβάθμιση σε μια νεότερη έκδοση και δεν υπάρχουν αλλαγές στη ρύθμιση παραμέτρων, δεν πρέπει να προγραμματιστεί πλήρη εισαγωγή/συγχρονισμό.

## <a name="111100"></a>1.1.110.0
Κυκλοφορήσει: Φεβρουαρίου 2016

**Σταθερή θέματα:**

- Αναβάθμιση από προηγούμενες εκδόσεις δεν λειτουργεί, εάν η εγκατάσταση δεν βρίσκεται στον προεπιλεγμένο φάκελο **C:\Program Files** .
- Εάν εγκαταστήσετε και κατάργηση επιλογής **Ξεκινήστε τη διαδικασία συγχρονισμού...** στο τέλος του Οδηγού εγκατάστασης, επανεκτέλεση του Οδηγού εγκατάστασης δεν δίνει τη δυνατότητα το χρονοδιάγραμμα.
- Το χρονοδιάγραμμα δεν λειτουργεί όπως αναμένεται σε διακομιστές όπου η μορφή ημερομηνίας/ώρας δεν είναι en ΗΠΑ. Επίσης αποκλείει `Get-ADSyncScheduler` για να λάβετε σωστές ώρες.
- Εάν έχετε εγκαταστήσει μια παλαιότερη έκδοση του Azure AD Connect με ADFS ως η είσοδος επιλογή και η αναβάθμιση, δεν μπορείτε να εκτελέσετε τον Οδηγό εγκατάστασης ξανά.

## <a name="111050"></a>1.1.105.0
Κυκλοφορήσει: Φεβρουαρίου 2016

**Νέες δυνατότητες:**

- Η δυνατότητα [αυτόματης αναβάθμισης](active-directory-aadconnect-feature-automatic-upgrade.md) για τους πελάτες σας ρυθμίσεις Express.
- Υποστήριξη για τον καθολικό διαχειριστή χρησιμοποιώντας MFA και PIM στον Οδηγό εγκατάστασης.
    - Πρέπει να επιτρέψετε το διακομιστή μεσολάβησης να επιτρέπει επίσης την κυκλοφορία στο https://secure.aadcdn.microsoftonline-p.com Εάν χρησιμοποιείτε το MFA.
    - Πρέπει να προσθέσετε https://secure.aadcdn.microsoftonline-p.com στη λίστα αξιόπιστων τοποθεσιών για MFA για να λειτουργήσει σωστά.
- Να επιτρέπεται η αλλαγή του χρήστη εισόδου μέθοδο μετά την αρχική εγκατάσταση.
- Να επιτρέπεται σε [τομέα και OU φιλτράρισμα](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) τον Οδηγό εγκατάστασης. Αυτό σας επιτρέπει επίσης τη σύνδεση σε συμπλεγμάτων δομών όπου δεν όλους τους τομείς είναι διαθέσιμες.
- [Το χρονοδιάγραμμα](active-directory-aadconnectsync-feature-scheduler.md) είναι ενσωματωμένο στο μηχανισμό συγχρονισμού.

**Δυνατότητες προωθούνται από την προεπισκόπηση GA:**

- [Συσκευή αντιγραφής](active-directory-aadconnect-feature-device-writeback.md).
- [Επεκτάσεις καταλόγου](active-directory-aadconnectsync-feature-directory-extensions.md).

**Νέες δυνατότητες προεπισκόπησης:**

- Η νέα προεπιλογή συγχρονισμός κύκλος διάστημα είναι 30 λεπτά. Χρησιμοποιείται για να 3 ώρες για όλες τις προηγούμενες εκδόσεις. Προσθέτει υποστήριξη για να αλλάξετε τη συμπεριφορά [του χρονοδιαγράμματος](active-directory-aadconnectsync-feature-scheduler.md) .

**Σταθερή θέματα:**

- Στη σελίδα επαλήθευση DNS τομέων δεν αναγνωρίζει πάντα τους τομείς.
- Οδηγίες για τα διαπιστευτήρια διαχειριστή τομέα κατά τη ρύθμιση παραμέτρων ADFS.
- Το εσωτερικής λογαριασμούς AD δεν αναγνωρίζονται από τον Οδηγό εγκατάστασης εάν βρίσκεται σε έναν τομέα με μια διαφορετική δέντρου DNS από τον ριζικό τομέα.

## <a name="1091310"></a>1.0.9131.0
Κυκλοφορήσει: Δεκεμβρίου 2015

**Σταθερή θέματα:**

- Συγχρονισμό κωδικού πρόσβασης ενδέχεται να μην λειτουργούν όταν αλλάζετε τους κωδικούς πρόσβασης στο AD DS, αλλά λειτουργεί όταν θέλετε να ορίσετε τον κωδικό πρόσβασης.
- Όταν έχετε διακομιστή μεσολάβησης, Azure AD του ελέγχου ταυτότητας ενδέχεται να αποτύχει κατά την εγκατάσταση ή κατάργηση αναβάθμισης στη σελίδα Ρύθμιση παραμέτρων.
- Ενημέρωση από μια προηγούμενη έκδοση του Azure AD Connect με ένα πλήρες SQL Server θα αποτύχει εάν δεν είστε σα σε SQL.
- Ενημέρωση από μια προηγούμενη έκδοση του Azure AD Connect με έναν απομακρυσμένο SQL Server, θα εμφανιστεί το σφάλμα "Δεν είναι δυνατή η πρόσβαση στη βάση δεδομένων ADSync SQL".

## <a name="1091250"></a>1.0.9125.0
Κυκλοφορήσει: Νοεμβρίου 2015

**Νέες δυνατότητες:**

- Μπορεί να ρυθμίσει ξανά τις παραμέτρους του ADFS να Azure AD αξιοπιστίας.
- Να ανανεώσετε το σχήμα της υπηρεσίας καταλόγου Active Directory και να αναδημιουργήσετε κανόνες συγχρονισμού.
- Να απενεργοποιήσετε έναν κανόνα συγχρονισμού.
- Να καθορίσετε "AuthoritativeNull" ως μια νέα λεκτικής σταθεράς σε έναν κανόνα συγχρονισμού.

**Νέες δυνατότητες προεπισκόπησης:**

- [Azure AD σύνδεση εύρυθμης λειτουργίας για το συγχρονισμό](active-directory-aadconnect-health-sync.md).
- Υποστήριξη για [τις υπηρεσίες τομέα AD Azure](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) συγχρονισμό κωδικού πρόσβασης.

**Νέες υποστηριζόμενες σενάριο:**

- Υποστηρίζει πολλές εταιρείες με Exchange εσωτερικής εγκατάστασης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [υβριδικές αναπτύξεις με πολλαπλές δομές υπηρεσίας καταλόγου Active Directory](https://technet.microsoft.com/library/jj873754.aspx) .

**Σταθερή θέματα:**

- Θέματα συγχρονισμού κωδικού πρόσβασης:
    - Αντικείμενο μετακινηθεί από εκτός της εμβέλειας στο εύρος δεν θα έχει τον κωδικό πρόσβασης που συγχρονίζονται. Σε αυτό το incudes OU και το φιλτράρισμα χαρακτηριστικό.
    - Επιλέγοντας ένα νέο OU για να συμπεριλάβετε σε συγχρονισμό δεν απαιτεί συγχρονισμό πλήρους τον κωδικό πρόσβασης.
    - Όταν είναι ενεργοποιημένη η απενεργοποίησης χρήστη δεν συγχρονίσετε τον κωδικό πρόσβασης.
    - Η ουρά "Επανάληψη" κωδικός πρόσβασης είναι άπειρο και το προηγούμενο όριο 5.000 αντικειμένων για να είναι απόσυρση έχει καταργηθεί.
    - [Αντιμετώπιση προβλημάτων βελτιωμένο](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization).
- Δεν είναι δυνατή η σύνδεση σε υπηρεσία καταλόγου Active Directory με το Windows Server 2016 δάσος διαλειτουργικού επίπεδο.
- Δεν μπορείτε να αλλάξετε την ομάδα που χρησιμοποιείται για το φιλτράρισμα ομάδας μετά την αρχική εγκατάσταση.
- Δεν είναι πλέον δημιουργεί ένα νέο προφίλ χρήστη στο διακομιστή Azure AD Connect για κάθε χρήστη με τον τρόπο αλλαγής κωδικού πρόσβασης με αντιγραφή κωδικού πρόσβασης με δυνατότητα.
- Δεν μπορείτε να χρησιμοποιήσετε Ακέραιος μεγάλου μήκους σε συγχρονισμό κανόνων εύρους τιμών.
- Το πλαίσιο ελέγχου "συσκευή αντιγραφής" παραμένει απενεργοποιημένη εάν υπάρχουν μη προσβάσιμος ελεγκτές.

## <a name="1086670"></a>1.0.8667.0
Κυκλοφορήσει: Αυγούστου 2015

**Νέες δυνατότητες:**

- Ο Οδηγός εγκατάστασης Azure AD Connect τώρα έχει μεταφραστεί σε όλες τις γλώσσες Windows Server.
- Υποστήριξη που προστέθηκε για το λογαριασμό ξεκλειδώσετε κατά τη χρήση Azure AD Διαχείριση κωδικών πρόσβασης.

**Σταθερή θέματα:**

- Οδηγός εγκατάστασης Azure AD Connect παρουσιάζει σφάλμα εάν κάποιος άλλος χρήστης εξακολουθεί εγκατάστασης και όχι το άτομο που ξεκίνησε πρώτα την εγκατάσταση.
- Εάν μια προηγούμενη κατάργησης της εγκατάστασης του Azure AD Connect αποτυγχάνει για να καταργήσετε την εγκατάσταση του συγχρονισμού Azure AD Connect καθαρή, δεν είναι δυνατό να επαναλάβετε την εγκατάσταση.
- Δεν είναι δυνατή η εγκατάσταση του Azure AD Connect χρησιμοποιώντας εγκατάσταση Express, εάν ο χρήστης δεν είναι σε τον ριζικό τομέα του συμπλέγματος ή εάν χρησιμοποιείται μια μη αγγλική έκδοση της υπηρεσίας καταλόγου Active Directory.
- Εάν δεν μπορεί να επιλυθεί το FQDN του λογαριασμού χρήστη υπηρεσίας καταλόγου Active Directory, εμφανίζεται μια παραπλανητικά μήνυμα σφάλματος "Αποτυχία για να δεσμεύσετε το σχήμα".
- Εάν ο λογαριασμός που χρησιμοποιείται στη γραμμή σύνδεσης Active Directory έχει αλλάξει εκτός του οδηγού, ο οδηγός θα αποτύχει σε επόμενες εκτελείται.
- Azure AD Connect αποτυγχάνει μερικές φορές για να εγκαταστήσετε σε έναν ελεγκτή τομέα.
- Δεν είναι δυνατό να ενεργοποίηση και απενεργοποίηση "Ενδιάμεσου σταδίου λειτουργία", εάν έχουν προστεθεί επέκταση χαρακτηριστικά.
- Κωδικός πρόσβασης αντιγραφής αποτυγχάνει σε ορισμένες ρυθμίσεις παραμέτρων λόγω εσφαλμένο κωδικό πρόσβασης του Active Directory Connector.
- Δεν είναι δυνατό να αναβαθμιστούν DirSync εάν dn χρησιμοποιείται στο χαρακτηριστικό φιλτραρίσματος.
- Η υπερβολική χρήση CPU κατά τη χρήση επαναφορά του κωδικού πρόσβασης.

**Προεπισκόπηση καταργημένες δυνατότητες:**

- Η δυνατότητα προεπισκόπησης προσωρινά καταργήθηκε [αντιγραφής χρήστη](active-directory-aadconnect-feature-preview.md#user-writeback) με βάση τα σχόλια από πελάτες μας preview. Εκ νέου προστίθεται αργότερα όταν θα σας έχετε απαντήσει το παρεχόμενο σχολίων.

## <a name="1086410"></a>1.0.8641.0
Κυκλοφορήσει: 2015 Ιουνίου

**Αρχική έκδοση του Azure AD Connect.**

Τροποποιημένο όνομα από το Azure AD Sync Azure AD Connect.

**Νέες δυνατότητες:**

- [Γρήγορες ρυθμίσεις](./connect/active-directory-aadconnect-get-started-express.md) εγκατάστασης
- Να [ρυθμίσετε τις παραμέτρους ADFS](./connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
- Να [κάνετε αναβάθμιση από DirSync](./connect/active-directory-aadconnect-dirsync-upgrade-get-started.md)
- [Αποτροπή τυχαίες διαγραφές](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
- Παρουσιάστηκε [ενδιάμεσου λειτουργία](active-directory-aadconnectsync-operations.md#staging-mode)

**Νέες δυνατότητες προεπισκόπησης:**

- [Αντιγραφή χρήστη](active-directory-aadconnect-feature-preview.md#user-writeback)
- [Ομάδα αντιγραφής](active-directory-aadconnect-feature-preview.md#group-writeback)
- [Συσκευή αντιγραφής](active-directory-aadconnect-feature-device-writeback.md)
- [Επεκτάσεις καταλόγου](active-directory-aadconnect-feature-preview.md#directory-extensions)


## <a name="104940501"></a>1.0.494.0501
Κυκλοφορήσει: 2015 Μάιος

**Νέα απαίτηση:**

- Azure AD Sync απαιτεί τώρα το .net framework έκδοση 4.5.1 να έχει εγκατασταθεί.

**Σταθερή θέματα:**

- Αντιγραφή κωδικού πρόσβασης από το Azure AD αποτυγχάνει με σφάλμα συνδεσιμότητας servicebus.

## <a name="104910413"></a>1.0.491.0413
Κυκλοφορήσει: 2015 Απρίλιος

**Σταθερή θέματα και βελτιώσεις:**

- Το Active Directory Connector δεν επεξεργάζονται διαγράφει σωστά εάν έχει ενεργοποιηθεί ο Κάδος Ανακύκλωσης και υπάρχουν πολλοί τομείς στο σύμπλεγμα.
- Οι επιδόσεις των λειτουργιών εισαγωγής που έχουν βελτιωθεί για τη σύνδεση Azure Active Directory.
- Όταν μια ομάδα έχει υπερβεί το όριο συμμετοχής (από προεπιλογή, το όριο έχει οριστεί σε 50k αντικείμενα), η ομάδα διαγράφηκε στο Azure Active Directory. Η νέα συμπεριφορά είναι ότι θα παραμείνει στην ομάδα, παρουσιαστεί σφάλμα και θα εξαχθούν οι νέες αλλαγές συμμετοχή ως μέλος.
- Ένα νέο αντικείμενο δεν μπορεί να αποδοθεί εάν μια σταδιακή διαγραφή με το ίδιο DN υπάρχει ήδη στο χώρο γραμμή σύνδεσης.
- Ορισμένα αντικείμενα επισημαίνονται για που συγχρονίζονται κατά τη διάρκεια του συγχρονισμού delta, παρόλο που δεν υπάρχει αλλαγή τοποθετούνται στο αντικείμενο.
- Να επιβάλετε μια συγχρονισμό κωδικού πρόσβασης καταργεί επίσης την προτιμώμενη λίστα ελεγκτή Τομέα.
- CSExportAnalyzer έχει προβλήματα με ορισμένα μέλη αντικείμενα.

**Νέες δυνατότητες:**

- Τώρα να συνδεθείτε σε "ANY" Τύπος αντικειμένου σε MV του συνδέσμου.

## <a name="104850222"></a>1.0.485.0222
Κυκλοφορήσει: Φεβρουάριος 2015

**Βελτιώσεις:**

- Εισαγωγή βελτιωμένη απόδοση.

**Σταθερή θέματα:**

- Το χαρακτηριστικό cloudFiltered που χρησιμοποιείται από το χαρακτηριστικό φιλτράρισμα θα διατηρήσει συγχρονισμό κωδικού πρόσβασης. Φιλτραρισμένες αντικειμένων δεν θα είναι πλέον στην εμβέλεια για συγχρονισμό κωδικού πρόσβασης.
- Στις σπάνιων περιπτώσεις όπου η τοπολογία είχατε πολύ πολλούς ελεγκτές τομέα, συγχρονισμό κωδικού πρόσβασης δεν λειτουργεί.
- "Διακοπή διακομιστή" κατά την εισαγωγή από τη γραμμή σύνδεσης Azure AD μετά τη Διαχείριση συσκευών έχει ενεργοποιηθεί στο Azure AD/Intune.
- Συμμετοχή σε εξωτερικό αρχές ασφαλείας (FSPs) από πολλούς τομείς στο ίδιο δάσος προκαλεί σφάλμα ασαφών.

## <a name="104751202"></a>1.0.475.1202
Κυκλοφορήσει: Δεκέμβριος 2014

**Νέες δυνατότητες:**

- Υποστηρίζεται τώρα για να κάνετε το συγχρονισμό κωδικού πρόσβασης με το φιλτράρισμα με βάση το χαρακτηριστικό. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Συγχρονισμός κωδικού πρόσβασης με το φιλτράρισμα](active-directory-aadconnectsync-configure-filtering.md).
- Το χαρακτηριστικό msDS-ExternalDirectoryObjectID εγγράφεται ξανά AD. Αυτό προσθέτει υποστήριξη για τις εφαρμογές του Office 365 χρησιμοποιώντας OAuth2 για να αποκτήσετε πρόσβαση και τα δύο, Online και γραμματοκιβωτίων σε μια υβριδική ανάπτυξη του Exchange εσωτερικής.

**Σταθερή προβλημάτων αναβάθμισης για:**

- Μια πιο πρόσφατη έκδοση του Βοηθού εισόδου είναι διαθέσιμη στο διακομιστή.
- Μια προσαρμοσμένη διαδρομή εγκατάστασης που χρησιμοποιήθηκε για να εγκαταστήσετε το Azure AD Sync.
- Ένα κριτήριο δεν είναι έγκυρο προσαρμοσμένο σύνδεσμο αποκλείει την αναβάθμιση.

**Άλλες ενημερώσεις κώδικα:**

- Σταθερή τα πρότυπα για το Office Pro Plus.
- Σταθερή ζητήματα κατά την εγκατάσταση που προκαλούνται από τα ονόματα χρηστών που ξεκινούν με μια παύλα.
- Σταθερή να χάσετε τη ρύθμιση sourceAnchor κατά την εκτέλεση του Οδηγού εγκατάστασης δεύτερη φορά.
- Σταθερή ανίχνευση ETW για συγχρονισμό κωδικού πρόσβασης

## <a name="104701023"></a>1.0.470.1023
Κυκλοφορήσει: Οκτώβριος 2014

**Νέες δυνατότητες:**

- Συγχρονισμός κωδικού πρόσβασης από πολλές εσωτερικής AD να Azure AD.
- Μεταφρασμένα εγκατάστασης περιβάλλοντος εργασίας Χρήστη σε όλες τις γλώσσες Windows Server.

**Αναβάθμιση από GA AADSync 1.0**

Εάν έχετε ήδη εγκαταστήσει το Azure AD Sync, υπάρχει ένα επιπλέον βήμα που πρέπει να κάνετε σε περίπτωση που έχετε αλλάξει οποιονδήποτε από τους κανόνες συγχρονισμού της πρώτης. Μετά την αναβάθμιση για να το 1.0.470.1023 αφήστε, ο συγχρονισμός είναι διπλή κανόνων που έχετε τροποποιήσει. Για κάθε κανόνα συγχρονισμού που έχουν τροποποιηθεί, κάντε τα εξής:

- Εντοπίστε τον κανόνα συγχρονισμού που έχετε τροποποιήσει και σημειώστε τις αλλαγές.
- Διαγράψτε τον κανόνα συγχρονισμού.
- Εντοπίστε το νέο κανόνα συγχρονισμού που δημιουργήθηκε από το Azure AD Sync και εφαρμόστε ξανά τις αλλαγές.

**Δικαιώματα για το λογαριασμό AD**

Ο λογαριασμός AD πρέπει να έχει επιπλέον δικαιώματα για να μπορέσετε να διαβάσετε τα κλειδιά κατακερματισμού του κωδικού πρόσβασης από το AD. Τα δικαιώματα για να εκχωρήσετε ονομάζονται "Αναπαραγωγή αλλαγών καταλόγου" και "Αναπαραγωγή καταλόγου τις αλλαγές όλων". Και τα δύο δικαιώματα απαιτούνται για να μπορέσετε να διαβάσετε τα κλειδιά κατακερματισμού του κωδικού πρόσβασης

## <a name="104190911"></a>1.0.419.0911
Κυκλοφορήσει: Σεπτεμβρίου 2014

**Αρχική έκδοση του Azure AD Sync.**

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
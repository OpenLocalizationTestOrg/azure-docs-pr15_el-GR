<properties
    pageTitle="Azure AD Connect λειτουργίες εύρυθμης λειτουργίας."
    description="Σε αυτό το άρθρο περιγράφει πρόσθετες λειτουργίες που μπορούν να εκτελεστούν αφού έχετε αναπτύξει Azure AD σύνδεση υγείας."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect λειτουργίες εύρυθμης λειτουργίας

Το παρακάτω θέμα περιγράφει τις διάφορες λειτουργίες που μπορούν να εκτελεστούν χρησιμοποιώντας Azure AD σύνδεση υγείας.

## <a name="enable-email-notifications"></a>Ενεργοποίηση ειδοποιήσεων ηλεκτρονικού ταχυδρομείου
Μπορείτε να ρυθμίσετε το Azure AD σύνδεση εύρυθμης λειτουργίας υπηρεσίας για την αποστολή ειδοποιήσεων ηλεκτρονικού ταχυδρομείου όταν δημιουργούνται ειδοποιήσεις που υποδεικνύει υποδομή ταυτότητά σας δεν είναι σε καλή κατάσταση. Αυτό θα συμβεί όταν δημιουργείται μια ειδοποίηση, καθώς και όταν έχει σημανθεί ως επιλυθεί. Ακολουθήστε τις παρακάτω οδηγίες για τη ρύθμιση παραμέτρων ειδοποιήσεων ηλεκτρονικού ταχυδρομείου.

![Azure AD Connect εύρυθμης λειτουργίας ηλεκτρονικού ταχυδρομείου ειδοποίησης Ανακαλύψτε](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Ειδοποιήσεις ηλεκτρονικού ταχυδρομείου είναι απενεργοποιημένες από προεπιλογή.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Για να ενεργοποιήσετε την Azure AD σύνδεση εύρυθμης λειτουργίας ειδοποιήσεις ηλεκτρονικού ταχυδρομείου

1. Ανοίξτε το Blade ειδοποιήσεων για την υπηρεσία για την οποία θέλετε να λαμβάνετε ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου.
2. Κάντε κλικ στο κουμπί "Ρυθμίσεις ειδοποιήσεων" από τη γραμμή ενεργειών.
3. Ενεργοποιήστε το διακόπτη ειδοποίηση μέσω ηλεκτρονικού ταχυδρομείου σε Ενεργοποιηση.
4. Επιλέξτε το πλαίσιο ελέγχου για να ρυθμίσετε τις παραμέτρους του καθολικοί διαχειριστές να λαμβάνετε ειδοποιήσεις μέσω ηλεκτρονικού ταχυδρομείου.
5. Εάν θέλετε να λαμβάνετε ειδοποιήσεις μέσω ηλεκτρονικού ταχυδρομείου σε άλλες διευθύνσεις ηλεκτρονικού ταχυδρομείου, μπορείτε να τα καθορίσετε στο πλαίσιο επιπλέον παραλήπτη ηλεκτρονικού ταχυδρομείου. Για να καταργήσετε μια διεύθυνση ηλεκτρονικού ταχυδρομείου από αυτήν τη λίστα, κάντε δεξί κλικ στην εγγραφή και επιλέξτε Διαγραφή.
6. Για να ολοκληρώσετε τις αλλαγές, κάντε κλικ στο "Αποθήκευση". Όλες οι αλλαγές θα χρειαστούν εφέ μόνο μετά την επιλογή "Αποθήκευση".

## <a name="delete-a-server-or-service-instance"></a>Διαγράψτε μια παρουσία διακομιστή ή υπηρεσία

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Διαγραφή ενός διακομιστή από Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας
Σε ορισμένες περιπτώσεις, ίσως θέλετε να καταργήσετε ένα διακομιστή από παρακολουθείται. Ακολουθήστε τις παρακάτω οδηγίες για να καταργήσετε ένα διακομιστή από Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας.

Κατά τη διαγραφή ενός διακομιστή, λάβετε υπόψη τα εξής:

- Αυτή η ενέργεια θα ΔΙΑΚΌΨΕΙ τη συλλογή περαιτέρω δεδομένα από αυτόν το διακομιστή. Αυτός ο διακομιστής θα καταργηθεί από την υπηρεσία παρακολούθησης. Μετά από αυτήν την ενέργεια, δεν θα μπορούν να δουν ειδοποιήσεις για νέα, παρακολούθηση ή χρήση ανάλυσης δεδομένων για αυτόν το διακομιστή.
- Αυτή η ενέργεια δεν θα καταργήσετε την εγκατάσταση ή καταργήστε τον παράγοντα εύρυθμης λειτουργίας από το διακομιστή. Εάν δεν καταργήσετε τον παράγοντα εύρυθμης λειτουργίας πριν από την εκτέλεση αυτού του βήματος, μπορείτε να δείτε τα συμβάντα σφάλματος στο διακομιστή που σχετίζονται με τον παράγοντα εύρυθμης λειτουργίας.
- Αυτή η ενέργεια δεν θα διαγράψει τα δεδομένα που συλλέγονται ήδη από αυτόν το διακομιστή. Αυτά τα δεδομένα θα διαγραφούν σύμφωνα με την πολιτική διατήρησης του Microsoft Azure δεδομένων.
- Αφού εκτελέσετε αυτήν την ενέργεια, εάν θέλετε να ξεκινήσετε την παρακολούθηση στον ίδιο διακομιστή ξανά, θα πρέπει να καταργήσετε την εγκατάσταση και να εγκαταστήσετε ξανά τον παράγοντα εύρυθμης λειτουργίας σε αυτόν το διακομιστή.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Για να διαγράψετε ένα διακομιστή από Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας

Azure AD Connect εύρυθμη λειτουργία AD FS & Azure AD σύνδεση (Συγχρονισμός):

1. Ανοίξτε το Blade διακομιστή από το διακομιστή Blade λίστας, επιλέγοντας το όνομα του διακομιστή για να καταργηθούν.
2. Στην Blade το διακομιστή, κάντε κλικ στο κουμπί "Διαγραφή" από τη γραμμή ενεργειών.
3. Επιβεβαιώστε την ενέργεια για να διαγράψετε το διακομιστή, πληκτρολογώντας το όνομα του διακομιστή στο παράθυρο διαλόγου επιβεβαίωσης.
4. Κάντε κλικ στο κουμπί "Διαγραφή".

Azure AD Connect εύρυθμης λειτουργίας για AD DS:

1. Ανοίξτε τον πίνακα εργαλείων ελεγκτές τομέα.
2. Επιλέξτε τον ελεγκτή τομέα που θέλετε να καταργήσετε.
3. Κάντε κλικ στο κουμπί "Διαγραφή επιλεγμένων" από τη γραμμή ενεργειών.
4. Επιβεβαιώστε την ενέργεια για να διαγράψετε το διακομιστή.
5. Κάντε κλικ στο κουμπί "Διαγραφή".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Διαγραφή μιας παρουσίας υπηρεσίας από το Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας

Σε ορισμένες περιπτώσεις, ίσως θέλετε να καταργήσετε μια παρουσία της υπηρεσίας. Ακολουθήστε τις παρακάτω οδηγίες για να καταργήσετε μια παρουσία της υπηρεσίας από Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας.

Κατά τη διαγραφή μιας παρουσίας υπηρεσίας, λάβετε υπόψη τα εξής:

- Αυτή η ενέργεια θα καταργήσει την τρέχουσα παρουσία της υπηρεσίας από την υπηρεσία παρακολούθησης.
- Αυτή η ενέργεια δεν θα καταργήσετε την εγκατάσταση ή κατάργηση ο παράγοντας εύρυθμης λειτουργίας από οποιονδήποτε από τους διακομιστές που έχουν παρακολουθούνται ως μέρος της εμφάνισης αυτής της υπηρεσίας. Εάν δεν καταργήσετε τον παράγοντα εύρυθμης λειτουργίας πριν από την εκτέλεση αυτού του βήματος, μπορείτε να δείτε τα συμβάντα σφάλματος σε οι διακομιστές που σχετίζονται με τον παράγοντα εύρυθμης λειτουργίας.
- Σύμφωνα με την πολιτική διατήρησης του Microsoft Azure δεδομένων θα διαγραφούν όλα τα δεδομένα από αυτήν την παρουσία υπηρεσίας.
- Αφού εκτελέσετε αυτήν την ενέργεια, εάν θέλετε να ξεκινήσετε την παρακολούθηση της υπηρεσίας, καταργήστε την εγκατάσταση και να εγκαταστήσετε ξανά τον παράγοντα εύρυθμης λειτουργίας σε όλους τους διακομιστές που θα παρακολουθείται. Αφού εκτελέσετε αυτήν την ενέργεια, εάν θέλετε να ξεκινήσετε την παρακολούθηση στον ίδιο διακομιστή ξανά, θα πρέπει να καταργήσετε την εγκατάσταση και να εγκαταστήσετε ξανά τον παράγοντα εύρυθμης λειτουργίας σε αυτόν το διακομιστή.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Για να διαγράψετε μια παρουσία της υπηρεσίας από Azure AD σύνδεση υπηρεσίας εύρυθμης λειτουργίας

1. Ανοίξτε την υπηρεσία Blade από την υπηρεσία Blade λίστας, επιλέγοντας το αναγνωριστικό υπηρεσίας (όνομα συστοιχίας) που θέλετε να καταργήσετε.
2. Στην Blade το διακομιστή, κάντε κλικ στο κουμπί "Διαγραφή" από τη γραμμή ενεργειών.
3. Επιβεβαιώστε ότι το όνομα της υπηρεσίας πληκτρολογώντας τον στο πλαίσιο επιβεβαίωσης. (για παράδειγμα: sts.contoso.com)
4. Κάντε κλικ στο κουμπί "Διαγραφή".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Διαχείριση πρόσβασης με το ρόλο με βάση τον έλεγχο πρόσβασης
### <a name="overview"></a>Επισκόπηση
[Έλεγχος πρόσβασης βάσει ρόλων](role-based-access-control-configure.md) για Azure εύρυθμη λειτουργία της σύνδεση AD παρέχει πρόσβαση υπηρεσία υγείας σύνδεση Azure AD χρήστες ή/και ομάδες εκτός καθολικοί διαχειριστές. Αυτό είναι δυνατό με την εκχώρηση ρόλων στις ομάδες ή\και προβλεπόμενοι χρήστες και παρέχει ένα μηχανισμό για να περιορίσετε οι καθολικοί διαχειριστές εντός του καταλόγου.

#### <a name="roles"></a>Ρόλοι
Υγεία σύνδεση Azure AD υποστηρίζει τους εξής ρόλους ενσωματωμένα.

| Ρόλος | Δικαιώματα |
| ----------- | ---------- |
| Κάτοχος | Οι κάτοχοι είναι δυνατό να ***διαχειριστείτε την πρόσβαση*** (π.χ. εκχώρηση ρόλου σε χρήστη/ομάδας), ***δείτε όλες τις πληροφορίες*** (π.χ., Προβολή προειδοποιήσεων) από την πύλη και να ***αλλάξετε τις ρυθμίσεις*** (π.χ., ειδοποιήσεις ηλεκτρονικού ταχυδρομείου) μέσα σε Azure AD σύνδεση υγείας. <br>Από προεπιλογή, οι καθολικοί διαχειριστές Azure AD έχουν εκχωρηθεί ο ρόλος αυτός και αυτό δεν μπορεί να αλλάξει.  |
|Συμβολής|  Οι συνεργάτες να ***προβάλετε όλες τις πληροφορίες*** (π.χ., Προβολή προειδοποιήσεων) από την πύλη και να ***αλλάξετε τις ρυθμίσεις*** (π.χ., ειδοποιήσεις ηλεκτρονικού ταχυδρομείου) μέσα σε Azure AD σύνδεση υγείας.|
|Πρόγραμμα ανάγνωσης| Οι αναγνώστες να ***προβάλετε όλες τις πληροφορίες*** (π.χ., Προβολή προειδοποιήσεων) από την πύλη εντός Azure AD σύνδεση υγείας.|

Όλα τα άλλα ρόλους (όπως 'Οι διαχειριστές χρηστών Access' ή 'DevTest Labs χρήστες'), ακόμα και αν είναι διαθέσιμη στο της πύλης εμπειρίας, έχει καμία επίδραση για να αποκτήσετε πρόσβαση σε μέσα υγείας σύνδεση Azure AD.

#### <a name="access-scope"></a>Πεδίο εφαρμογής Access

Azure AD Connect υποστηρίζει τη διαχείριση της πρόσβασης σε δύο επίπεδα:

- ***Όλες τις εμφανίσεις της υπηρεσίας***: Αυτή είναι η προτεινόμενη διαδρομή για τους περισσότερους πελάτες και ελέγχει την πρόσβαση για όλες τις εμφανίσεις υπηρεσίας (π.χ., ένα σύμπλεγμα ADFS) σε όλους τους τύπους ρόλο που παρακολουθούνται από την κατάσταση σύνδεσης Azure AD.

- ***Η παρουσία της υπηρεσίας***: σε ορισμένες περιπτώσεις, ίσως χρειαστεί να segregate πρόσβασης που βασίζεται σε ρόλο τύπους ή από μια παρουσία της υπηρεσίας. Σε αυτήν την περίπτωση, μπορείτε να διαχειριστείτε την πρόσβαση στο επίπεδο παρουσία της υπηρεσίας.  

Έχει εκχωρηθεί δικαίωμα εάν ο τελικός χρήστης έχει πρόσβαση σε ένα επίπεδο καταλόγου ή παρουσία της υπηρεσίας.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Πώς μπορείτε να επιτρέψετε την πρόσβαση χρηστών ή ομάδων υγείας σύνδεση Azure AD
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Τα βήματα 1: Επιλέξτε τα κατάλληλα δικαιώματα πρόσβασης εύρος
Για να επιτρέψετε σε ένα χρήστη πρόσβαση σε *όλες τις εμφανίσεις της υπηρεσίας* επίπεδο μέσα σε κατάσταση σύνδεσης Azure AD, ανοίξτε το κύριο blade στο Azure AD σύνδεση υγείας.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Βήμα 2: Προσθήκη χρηστών, ομάδων και εκχώρηση ρόλων
1. Κάντε κλικ στην επιλογή από το τμήμα "Χρήστες" από την ενότητα Ρύθμιση παραμέτρων.<br>
![Azure AD Connect κύριο Blade RBAC εύρυθμης λειτουργίας](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Επιλέξτε "Προσθήκη"
3. Επιλέξτε το "ρόλο" όπως "Κάτοχος"<br>
![Azure AD Connect εύρυθμης λειτουργίας RBAC Προσθήκη χρήστη](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Πληκτρολογήστε το όνομα ή το αναγνωριστικό του χρήστη προορισμού ή της ομάδας. Μπορείτε να επιλέξετε έναν ή περισσότερους χρήστες ή ομάδες την ίδια στιγμή. Κάντε κλικ στο κουμπί "επιλογή".
![Azure AD Connect εύρυθμης λειτουργίας επιλογής RBAC χρήστη](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Επιλέξτε "Ok".<br>

6. Μόλις ολοκληρωθεί η εκχώρηση ρόλων, θα εμφανιστεί στη λίστα το χρήστες ή/και ομάδες.<br>
![Azure AD Connect λίστα χρηστών RBAC εύρυθμης λειτουργίας](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Τα παρακάτω βήματα θα σας επιτρέψει που παρατίθενται χρήστες και πρόσβαση στην ομάδα σύμφωνα με τους ρόλους που του έχουν ανατεθεί.
>[AZURE.NOTE]
- Καθολικοί διαχειριστές έχουν πάντα πλήρη πρόσβαση σε όλες τις λειτουργίες, αλλά ο Καθολικός διαχειριστής λογαριασμών δεν θα εμφανίζεται στην παραπάνω λίστα.
- Η δυνατότητα "Πρόσκληση χρηστών" δεν υποστηρίζεται εντός Azure AD σύνδεση υγείας.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Βήμα 3: Κοινή χρήση της θέσης blade με χρήστες ή ομάδες
1. Μετά την εκχώρηση δικαιωμάτων, ο χρήστης έχει πρόσβαση υγείας σύνδεση Azure AD μεταβαίνοντας στο [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Μία φορά στην το blade, ο χρήστης να καρφιτσώσετε το blade ή διαφορετικά τμήματα στον πίνακα εργαλείων κάνοντας απλώς κλικ στο κουμπί "Καρφίτσωμα στον πίνακα εργαλείων"<br>
![Blade pin Azure AD σύνδεση RBAC εύρυθμης λειτουργίας](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Ένας χρήστης με ρόλο "Αναγνώστη" στους οποίους έχουν ανατεθεί δεν θα μπορείτε να εκτελέσετε τη λειτουργία "Δημιουργία" για να λάβετε την επέκταση υγείας σύνδεση Azure AD από το Azure Marketplace. Αυτός ο χρήστης μπορούν ακόμη να επισκέπτονται για να την blade, μεταβαίνοντας στη σύνδεση παραπάνω. Για μετέπειτα χρήση, ο χρήστης να καρφιτσώσετε το blade στον πίνακα εργαλείων.

### <a name="remove-users-andor-groups"></a>Κατάργηση χρηστών ή/και ομάδες
Μπορείτε να καταργήσετε ένα χρήστη ή μια ομάδα προστεθεί τμήμα Azure AD σύνδεση εύρυθμη λειτουργία των ρόλων με βάση τον έλεγχο πρόσβασης κάνοντας δεξί κλικ και επιλέγοντας κατάργηση.<br>
![Azure AD Connect εύρυθμης λειτουργίας RBAC Κατάργηση χρήστη](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Σχετικές συνδέσεις

* [Azure AD Connect εύρυθμης λειτουργίας](active-directory-aadconnect-health.md)
* [Azure AD Connect εγκατάστασης παράγοντα εύρυθμης λειτουργίας](active-directory-aadconnect-health-agent-install.md)
* [Χρήση του Azure AD σύνδεση εύρυθμης λειτουργίας με AD FS](active-directory-aadconnect-health-adfs.md)
* [Με χρήση του Azure AD σύνδεση υγείας για συγχρονισμό](active-directory-aadconnect-health-sync.md)
* [Χρήση του Azure AD σύνδεση εύρυθμη λειτουργία με το AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect εύρυθμης λειτουργίας συνήθεις Ερωτήσεις](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect ιστορικό εκδόσεων εύρυθμης λειτουργίας](active-directory-aadconnect-health-version-history.md)

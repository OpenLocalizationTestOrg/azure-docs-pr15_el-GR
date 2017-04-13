<properties
    pageTitle="Azure AD Connect: Επόμενα βήματα και πώς μπορείτε να διαχειριστείτε Azure AD Connect | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να επεκτείνετε την προεπιλεγμένη ρύθμιση παραμέτρων εργασιών και λειτουργίας για Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Επόμενα βήματα και πώς μπορείτε να διαχειριστείτε Azure AD Connect
Τα παρακάτω είναι για προχωρημένους λειτουργικές θέματα που σας επιτρέπουν να προσαρμόσετε Azure Active Directory σύνδεση ώστε να ανταποκρίνεται στις ανάγκες της εταιρείας σας και τις απαιτήσεις.  

## <a name="add-additional-sync-administrators"></a>Προσθήκη επιπλέον Συγχρονισμός διαχειριστών
Από προεπιλογή μόνο ο χρήστης που έκανε την εγκατάσταση και την τοπική οι διαχειριστές θα μπορούν να διαχειρίζονται το μηχανισμό εγκατεστημένων συγχρονισμού. Για επιπλέον άτομα να μπορούν να αποκτήσετε πρόσβαση και να διαχειριστείτε το μηχανισμό συγχρονισμού, εντοπίστε την ομάδα με το όνομα ADSyncAdmins στον τοπικό διακομιστή και προσθέστε τις σε αυτήν την ομάδα.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Εκχώρηση αδειών χρήσης σε χρήστες του Azure AD Premium και επιλογές φορητότητας για μεγάλες επιχειρήσεις

Τώρα που οι χρήστες έχουν συγχρονιστεί στο cloud, θα πρέπει να τις εκχωρήσετε μια άδεια χρήσης, ώστε να μπορούν να ξεκινήστε με τις εφαρμογές cloud όπως το Office 365.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Για να αντιστοιχίσετε Azure AD Premium ή για μεγάλες επιχειρήσεις φορητότητα οικογένεια άδεια χρήσης του
--------------------------------------------------------------------------------
1. Πραγματοποιήστε είσοδο στην πύλη του Azure ως διαχειριστής.
2. Στα αριστερά, επιλέξτε την **Υπηρεσία καταλόγου Active Directory**.
3. Στη σελίδα της υπηρεσίας καταλόγου Active Directory, κάντε διπλό κλικ στον κατάλογο που έχει τους χρήστες που θέλετε να ενεργοποιήσετε.
4. Στο επάνω μέρος της σελίδας καταλόγου, επιλέξτε **άδειες χρήσης**.
5. Στη σελίδα άδειες χρήσης, επιλέξτε Active Directory Premium ή την οικογένεια προγραμμάτων για μεγάλες επιχειρήσεις φορητότητα και, στη συνέχεια, κάντε κλικ στο κουμπί **Αντιστοίχιση**.
6. Στο παράθυρο διαλόγου, επιλέξτε τους χρήστες που θέλετε να εκχωρήσετε άδειες χρήσης και, στη συνέχεια, κάντε κλικ στο εικονίδιο σημάδι ελέγχου για να αποθηκεύσετε τις αλλαγές.


## <a name="verifying-the-scheduled-synchronization-task"></a>Επαλήθευση της εργασίας προγραμματισμένου συγχρονισμού
Εάν θέλετε να ελέγξετε την κατάσταση του συγχρονισμού, μπορείτε να το κάνετε αυτό, επιλέγοντας στην πύλη του Azure.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Για να επιβεβαιώσετε την εργασία του προγραμματισμένου συγχρονισμού
--------------------------------------------------------------------------------
1. Πραγματοποιήστε είσοδο στην πύλη του Azure ως διαχειριστής.
2. Στα αριστερά, επιλέξτε την **Υπηρεσία καταλόγου Active Directory**.
3. Στη σελίδα της υπηρεσίας καταλόγου Active Directory, κάντε διπλό κλικ στον κατάλογο που έχει τους χρήστες που θέλετε να ενεργοποιήσετε.
4. Στο επάνω μέρος της σελίδας καταλόγου, επιλέξτε **Ενοποίηση καταλόγου**.
5. Στην περιοχή ενοποίηση με τοπική υπηρεσία καταλόγου active directory σημείωση της ώρας της τελευταίας συγχρονισμού.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Εκκίνηση μιας εργασίας προγραμματισμένου συγχρονισμού
Εάν πρέπει να εκτελέσετε μια εργασία συγχρονισμού, μπορείτε να το κάνετε αυτό, εκτελώντας τα βήματα του οδηγού Azure AD Connect ξανά.  Θα χρειαστεί να καταχωρήσετε τα διαπιστευτήριά σας Azure AD.  Στον οδηγό, επιλέξτε την εργασία **Προσαρμογή επιλογών συγχρονισμού** και κάντε κλικ στο κουμπί Επόμενο μέσω του οδηγού. Στο τέλος, βεβαιωθείτε ότι είναι επιλεγμένο το πλαίσιο **εκκίνηση της διαδικασίας συγχρονισμού μόλις ολοκληρωθεί η αρχική ρύθμιση παραμέτρων** .

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Για περισσότερες πληροφορίες σχετικά με τον συγχρονισμό Azure AD Connect: χρονοδιάγραμμα, ανατρέξτε στο θέμα [Azure AD σύνδεση χρονοδιαγράμματος](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Πρόσθετες εργασίες που είναι διαθέσιμες στο Azure AD Connect
Μετά την αρχική εγκατάσταση του Azure AD Connect, μπορείτε να πάντα ξεκινήσετε τον οδηγό ξανά από τη συντόμευση Azure AD Connect Έναρξη σελίδα ή επιφάνειας εργασίας.  Θα παρατηρήσετε ότι θα μέσω του οδηγού ξανά παρέχει ορισμένες επιλογές νέα μορφή πρόσθετες εργασίες.  

Ο παρακάτω πίνακας παρέχει μια σύνοψη των αυτές τις εργασίες και μια σύντομη περιγραφή σε κάθε μία από αυτές.

![Συμμετοχή σε κανόνα](./media/active-directory-aadconnect-whats-next/addtasks.png)


Επιπλέον εργασιών | Περιγραφή
------------- | ------------- |
Προβολή του επιλεγμένου σεναρίου  |Σας επιτρέπει να προβάλετε την τρέχουσα λύση Azure AD Connect.  Αυτό περιλαμβάνει γενικές ρυθμίσεις, συγχρονισμένη σε καταλόγους, ρυθμίσεις συγχρονισμού, κ.λπ.
Προσαρμογή επιλογών συγχρονισμού | Σας επιτρέπει να αλλάξετε την τρέχουσα ρύθμιση παραμέτρων, συμπεριλαμβανομένης της προσθήκης επιπλέον συμπλεγμάτων δομών της υπηρεσίας καταλόγου Active Directory στη ρύθμιση παραμέτρων ή ενεργοποίηση συγχρονισμού επιλογές όπως χρήστη, ομάδας, συσκευή ή τον κωδικό πρόσβασης write-back.
Ενεργοποίηση της λειτουργίας ενδιάμεσου σταδίου |  Αυτό σας επιτρέπει να πληροφορίες του σταδίου που αργότερα θα συγχρονιστεί αλλά τίποτα, προκειμένου να εξαχθούν στο Azure AD ή υπηρεσία καταλόγου Active Directory.  Αυτό σας επιτρέπει να κάνετε προεπισκόπηση του συγχρονισμών πριν προκύπτουν.

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).
<properties
    pageTitle="Azure AD Connect: Γρήγορα αποτελέσματα να χρησιμοποιώντας τις ρυθμίσεις express | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να κάνετε λήψη, εγκατάσταση και εκτέλεση του Οδηγού εγκατάστασης για το Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Γρήγορα αποτελέσματα με το Azure AD Connect χρησιμοποιώντας τις ρυθμίσεις express
Azure AD Connect **Γρήγορες ρυθμίσεις** χρησιμοποιείται όταν έχετε μια τοπολογία μίας δάσος και [συγχρονισμό κωδικού πρόσβασης](../active-directory-aadconnectsync-implement-password-synchronization.md) για τον έλεγχο ταυτότητας. **Γρήγορες ρυθμίσεις** είναι η προεπιλεγμένη επιλογή και χρησιμοποιείται για το σενάριο ανεπτυγμένος πιο συχνά. Είστε μόνο μερικά σύντομο κλικ δεν βρίσκομαι στον υπολογιστή, για να επεκτείνετε τον κατάλογο εσωτερικής εγκατάστασης στο cloud.

Πριν να ξεκινήσετε την εγκατάσταση του Azure AD Connect, βεβαιωθείτε ότι για να [κάνετε λήψη Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) και ολοκληρώστε τα προ-απαραίτητα βήματα στο [Azure AD Connect: υλικό και τις προϋποθέσεις](../active-directory-aadconnect-prerequisites.md).

Εάν express ρυθμίσεις δεν ταιριάζουν με την τοπολογία σας, ανατρέξτε στο θέμα [σχετικές τεκμηρίωση](#related-documentation) για τα άλλα σενάρια.

## <a name="express-installation-of-azure-ad-connect"></a>Σύντομη εγκατάσταση του Azure AD Connect
Μπορείτε να δείτε αυτά τα βήματα στην πράξη στην ενότητα [βίντεο](#videos) .

1. Πραγματοποιήστε είσοδο ως διαχειριστής τοπικά στο διακομιστή που θέλετε να εγκαταστήσετε το Azure AD Connect σε. Θα πρέπει να κάνετε αυτό στο διακομιστή που θέλετε να είναι ο διακομιστής συγχρονισμού.
2. Εντοπίστε και κάντε διπλό κλικ στο **AzureADConnect.msi**.
3. Στην οθόνη υποδοχής, επιλέξτε το πλαίσιο συμφωνώντας να τους όρους της άδειας χρήσης και κάντε κλικ στην επιλογή **Continue**.  
4. Στην οθόνη ρυθμίσεις Express, κάντε κλικ στην επιλογή **Χρήση ρυθμίσεων express**.  
![Καλώς ορίσατε στο Azure AD σύνδεση](./media/active-directory-aadconnect-get-started-express/express.png)
5. Στη σύνδεση του Azure AD οθόνη, πληκτρολογήστε το όνομα χρήστη και τον κωδικό πρόσβασης του ο Καθολικός διαχειριστής για το Azure AD. Κάντε κλικ στο κουμπί **Επόμενο**.  
![Σύνδεση με Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) εάν λαμβάνετε ένα μήνυμα σφάλματος και έχετε προβλήματα με τη σύνδεση, στη συνέχεια, ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων συνδεσιμότητας](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Στη σύνδεση οθόνη AD DS, εισαγάγετε το όνομα χρήστη και τον κωδικό πρόσβασης για ένα λογαριασμό διαχειριστή εταιρείας. Μπορείτε να εισαγάγετε το τμήμα τομέα με NetBios ή FQDN μορφή, δηλαδή, FABRIKAM\administrator ή fabrikam.com\administrator. Κάντε κλικ στο κουμπί **Επόμενο**.  
![Συνδεθείτε στο AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. Σελίδα [**Azure AD εισόδου ρύθμιση παραμέτρων**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) εμφανίζει μόνο εάν δεν έχετε ολοκληρώσει την [επαλήθευση τομέων](../active-directory-add-domain.md) με τις [προϋποθέσεις](../active-directory-aadconnect-prerequisites.md).
![Χωρίς επαλήθευση τομέων](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Εάν βλέπετε αυτήν τη σελίδα, στη συνέχεια, εξετάστε κάθε τομέα που έχει επισημανθεί **Δεν προστίθεται** και **Δεν έχει επαληθευτεί**. Βεβαιωθείτε ότι έχουν επαληθευτεί στο Azure AD αυτούς τους τομείς που χρησιμοποιείτε. Όταν έχετε επαληθεύσει τους τομείς σας, κάντε κλικ στο σύμβολο ανανέωσης.
8. Στο παράθυρο είστε έτοιμοι να ρυθμίσετε τις παραμέτρους της οθόνης, κάντε κλικ στην επιλογή **εγκατάσταση**.
    - Προαιρετικά στο παράθυρο είστε έτοιμοι για να ρυθμίσετε τις παραμέτρους σελίδας, μπορείτε να καταργήσετε το πλαίσιο ελέγχου **ξεκινήσετε τη διαδικασία συγχρονισμού μόλις ολοκληρωθεί η ρύθμιση παραμέτρων** . Θα πρέπει να μπορείτε να καταργήσετε αυτό το πλαίσιο ελέγχου εάν θέλετε να κάνετε πρόσθετες ρυθμίσεις, όπως [το φιλτράρισμα](../active-directory-aadconnectsync-configure-filtering.md). Εάν καταργήσετε την επιλογή αυτήν την επιλογή, ο οδηγός ρυθμίζει τις παραμέτρους συγχρονισμού αλλά αφήνει το χρονοδιάγραμμα απενεργοποιηθεί. Δεν εκτελείται μέχρι να τον ενεργοποιήσετε με μη αυτόματο τρόπο με [ποιον τρόπο ο Οδηγός εγκατάστασης](../active-directory-aadconnectsync-installation-wizard.md).
    - Εάν έχετε Exchange σας στην εσωτερική εγκατάσταση υπηρεσίας καταλόγου Active Directory, στη συνέχεια, έχετε επίσης μια επιλογή για να ενεργοποιήσετε την [**Ανάπτυξη υβριδική ανάπτυξη του Exchange**](https://technet.microsoft.com/library/jj200581.aspx). Ενεργοποιήστε αυτήν την επιλογή εάν σκοπεύετε να έχουν γραμματοκιβώτια του Exchange τόσο στο cloud και εσωτερικής την ίδια στιγμή.
![Είστε έτοιμοι να ρυθμίσετε τις παραμέτρους Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Όταν ολοκληρωθεί η εγκατάσταση, κάντε κλικ στην επιλογή **Έξοδος**.
10. Μετά την ολοκλήρωση της εγκατάστασης, αποσυνδεθείτε και συνδεθείτε ξανά μπορέσετε να χρησιμοποιήσετε Διαχείριση υπηρεσίας συγχρονισμού ή πρόγραμμα επεξεργασίας κανόνων συγχρονισμού.

## <a name="videos"></a>Βίντεο

Για ένα βίντεο σχετικά με τη χρήση η Γρήγορη εγκατάσταση, ανατρέξτε στα θέματα:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Επόμενα βήματα
Τώρα που έχετε Azure AD Connect εγκατεστημένο μπορείτε να [επαληθεύσετε την εγκατάσταση και την εκχώρηση αδειών χρήσης](../active-directory-aadconnect-whats-next.md).

Μάθετε περισσότερα σχετικά με αυτές τις δυνατότητες, οι οποίες έχουν ενεργοποιημένες με την εγκατάσταση: [Αυτόματη αναβάθμιση](../active-directory-aadconnect-feature-automatic-upgrade.md) [διαγράφει αποτροπή τυχαίες](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)και [Azure AD σύνδεση υγείας](../active-directory-aadconnect-health-sync.md).

Μάθετε περισσότερα σχετικά με αυτά τα κοινά θέματα: [το χρονοδιάγραμμα και τον τρόπο ενεργοποίησης συγχρονισμού](../active-directory-aadconnectsync-feature-scheduler.md).

Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Σχετικές τεκμηρίωση

Το θέμα |  
--------- | ---------
Επισκόπηση Azure AD Connect | [Ενοποίηση του ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](../active-directory-aadconnect.md)
Εγκατάσταση χρησιμοποιώντας προσαρμοσμένες ρυθμίσεις | [Προσαρμοσμένη εγκατάσταση του Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Αναβάθμιση από DirSync | [Αναβάθμιση από το εργαλείο συγχρονισμού Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Οι λογαριασμοί που χρησιμοποιούνται για την εγκατάσταση | [Περισσότερες πληροφορίες σχετικά με τους λογαριασμούς Azure AD Connect και δικαιώματα](active-directory-aadconnect-accounts-permissions.md)

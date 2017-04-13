<properties
    pageTitle="Azure AD Connect συγχρονισμού: Διαχείριση του λογαριασμού υπηρεσίας Azure AD | Microsoft Azure"
    description="Αυτό το θέμα καταγράφει τον τρόπο επαναφοράς του λογαριασμού υπηρεσίας Azure AD."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, πώς μπορείτε να επαναφέρετε τον κωδικό πρόσβασης για το συγχρονισμό Azure AD Connect λογαριασμός υπηρεσίας σύνδεσης"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect συγχρονισμού: Διαχείριση του λογαριασμού υπηρεσίας Azure AD
Ο λογαριασμός υπηρεσίας που χρησιμοποιείται με τη γραμμή σύνδεσης Azure AD είναι πρέπει να είναι δωρεάν υπηρεσία. Εάν θέλετε να επαναφέρετε τα διαπιστευτήριά, στη συνέχεια, αυτό το θέμα ισχύει για εσάς. Για παράδειγμα, εάν ο Καθολικός διαχειριστής έχει κατά λάθος επαναφορά του κωδικού πρόσβασης του λογαριασμού υπηρεσίας με χρήση του PowerShell.

## <a name="reset-the-credentials"></a>Επαναφέρετε τα διαπιστευτήρια
Εάν ο λογαριασμός υπηρεσίας που ορίζονται από το στη γραμμή σύνδεσης Azure AD δεν μπορεί να επικοινωνήσει Azure AD λόγω προβλημάτων ελέγχου ταυτότητας, μπορείτε να επαναφέρετε τον κωδικό πρόσβασης.

1. Συνδεθείτε στο διακομιστή συγχρονισμού Azure AD Connect και ξεκινήστε PowerShell.
2. Εκτέλεση `Add-ADSyncAADServiceAccount`.  
![Addadsyncaadserviceaccount cmdlet του PowerShell](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Δώστε διαπιστευτήρια Azure AD καθολικού διαχειριστή.

Αυτό το cmdlet επαναφέρει τον κωδικό πρόσβασης για το λογαριασμό της υπηρεσίας και ενημερώστε το Azure AD και ο μηχανισμός συγχρονισμού.

## <a name="known-issues-these-steps-can-solve"></a>Γνωστά θέματα που μπορεί να επιλύσει τα παρακάτω βήματα
Αυτή η ενότητα είναι μια λίστα με σφάλματα που εμφανίζονται οι πελάτες που είχαν διορθωθεί από μια επαναφορά του λογαριασμού υπηρεσίας Azure AD διαπιστευτήρια.

-----------
Συμβάν 6900  
Ο διακομιστής αντιμετώπισε ένα μη αναμενόμενο σφάλμα κατά την επεξεργασία μια ειδοποίηση αλλαγής κωδικού πρόσβασης:  
AADSTS70002: Σφάλμα κατά την επαλήθευση διαπιστευτήρια. AADSTS50054: Παλιός κωδικός πρόσβασης χρησιμοποιείται για τον έλεγχο ταυτότητας.

----------
Συμβάν 659  
Σφάλμα κατά την ανάκτηση ρύθμιση παραμέτρων συγχρονισμού της πολιτικής κωδικού πρόσβασης. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Σφάλμα κατά την επαλήθευση διαπιστευτήρια. AADSTS50054: Παλιός κωδικός πρόσβασης χρησιμοποιείται για τον έλεγχο ταυτότητας.

## <a name="next-steps"></a>Επόμενα βήματα

**Επισκόπηση θεμάτων**

- [Azure AD Connect συγχρονισμού: Κατανόηση και προσαρμογή συγχρονισμού](active-directory-aadconnectsync-whatis.md)
- [Ενοποίηση σας ταυτοτήτων εσωτερικής εγκατάστασης με το Azure Active Directory](active-directory-aadconnect.md)

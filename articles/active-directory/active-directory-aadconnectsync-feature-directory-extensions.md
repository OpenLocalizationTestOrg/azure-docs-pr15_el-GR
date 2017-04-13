<properties
   pageTitle="Azure AD Connect συγχρονισμού: επεκτάσεις καταλόγου | Microsoft Azure"
   description="Αυτό το θέμα περιγράφει τη δυνατότητα επεκτάσεις καταλόγου στο Azure AD Connect."
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
   ms.date="08/19/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect συγχρονισμού: επεκτάσεις καταλόγου
Επεκτάσεις καταλόγου σάς επιτρέπει να επεκτείνετε το σχήμα στο Azure AD με το δικό σας χαρακτηριστικά από την υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης. Αυτή η δυνατότητα σάς επιτρέπει να δημιουργήσετε εφαρμογές LOB κατανάλωση χαρακτηριστικά που μπορείτε να συνεχίσετε τη Διαχείριση εσωτερικής εγκατάστασης. Αυτά τα χαρακτηριστικά μπορεί να χρησιμοποιηθεί από [επεκτάσεις καταλόγου Azure AD γράφημα](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) ή το [Microsoft Graph](https://graph.microsoft.io/). Μπορείτε να δείτε τα χαρακτηριστικά που είναι διαθέσιμα χρησιμοποιώντας την [Εξερεύνηση Azure AD Graph](https://graphexplorer.cloudapp.net) και [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) αντίστοιχα.

Προς το παρόν δεν υπάρχει φόρτο εργασίας του Office 365 καταναλώνει αυτά τα χαρακτηριστικά.

Μπορείτε να ρυθμίσετε τα επιπλέον χαρακτηριστικά που θέλετε να συγχρονίσετε με τη διαδρομή προσαρμοσμένες ρυθμίσεις του Οδηγού εγκατάστασης.
![Οδηγός επέκταση σχήματος](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png) τα παρακάτω χαρακτηριστικά, που είναι έγκυρες υποψήφιοι εμφανίζει την εγκατάσταση:

- Τύποι αντικειμένων χρηστών και ομάδων
- Χαρακτηριστικά μίας τιμής: συμβολοσειρά, Boolean, ακέραιος, δυαδικό
- Πολλαπλών τιμών χαρακτηριστικά: συμβολοσειρά, δυαδικό

Η λίστα των χαρακτηριστικών διαβάζεται από το cache δημιουργείται κατά την εγκατάσταση του Azure AD Connect. Εάν έχετε επεκτείνονται το σχήμα της υπηρεσίας καταλόγου Active Directory με πρόσθετα χαρακτηριστικά, το [σχήμα πρέπει να ανανεωθούν](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) πριν από αυτά τα νέα χαρακτηριστικά είναι ορατές.

Ένα αντικείμενο μπορεί να έχει έως 100 χαρακτηριστικά επεκτάσεις καταλόγου. Το μέγιστο μήκος είναι 250 χαρακτήρες. Εάν μια τιμή του χαρακτηριστικού είναι περισσότερο, τα δεκαδικά ψηφία περικόπτονται από το μηχανισμό συγχρονισμού.

Κατά την εγκατάσταση του Azure AD Connect, μια εφαρμογή έχει καταχωρηθεί όπου αυτά τα χαρακτηριστικά είναι διαθέσιμες. Μπορείτε να δείτε αυτήν την εφαρμογή στην πύλη του Azure.  
![Εφαρμογή επέκταση σχήματος](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3.png)

Αυτά τα χαρακτηριστικά είναι πλέον διαθέσιμες μέσω του γραφήματος:  
![Γράφημα](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Τα χαρακτηριστικά είναι το πρόθεμα με επέκταση\_{AppClientId}\_. Το AppClientId έχει την ίδια τιμή για όλα τα χαρακτηριστικά στον κατάλογό σας Azure AD.

## <a name="next-steps"></a>Επόμενα βήματα
Μάθετε περισσότερα σχετικά με τη ρύθμιση παραμέτρων [Azure AD Connect συγχρονισμός](active-directory-aadconnectsync-whatis.md) .

Μάθετε περισσότερα σχετικά με την [ενσωμάτωση του ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).

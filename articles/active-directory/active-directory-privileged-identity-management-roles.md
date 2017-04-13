<properties
   pageTitle="Ρόλοι στο PIM | Microsoft Azure"
   description="Μάθετε τι τους ρόλους που χρησιμοποιούνται για Προνομιούχους ταυτότητες με την επέκταση Azure Προνομιούχους διαχείρισης των ταυτοτήτων."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Ρόλοι διαχείρισης ταυτοτήτων Azure AD δικαιώματα

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Μπορείτε να εκχωρήσετε στους χρήστες στην εταιρεία σας να διαφορετικούς ρόλους διαχειριστή στο Azure AD. Αυτές οι αναθέσεις ρόλων ελέγχετε ποιες εργασίες, όπως η προσθήκη ή κατάργηση χρηστών ή την αλλαγή ρυθμίσεων της υπηρεσίας, οι χρήστες έχουν τη δυνατότητα να αναλάβουν Azure AD, Office 365 και άλλες υπηρεσίες Microsoft Online Services και συνδεδεμένο εφαρμογών.  

Ο Καθολικός διαχειριστής να ενημερώσετε τους χρήστες που έχουν **οριστικά** που έχουν εκχωρηθεί σε ρόλους σε Azure AD, χρήση των cmdlet του PowerShell, όπως `Add-MsolRoleMember` και `Remove-MsolRoleMember`, ή μέσω της πύλης κλασική όπως περιγράφεται στο θέμα [Εκχώρηση ρόλων διαχειριστή στο Azure Active Directory](active-directory-assign-admin-roles.md).

Η Διαχείριση ταυτοτήτων Azure AD δικαιώματα (PIM) διαχειρίζεται πολιτικές για δικαιώματα πρόσβασης για τους χρήστες στο Azure AD. PIM αντιστοιχίζει τους χρήστες σε έναν ή περισσότερους ρόλους στο Azure AD και μπορείτε να αναθέσετε κάποιον να είστε μόνιμα στο ρόλο, ή πληρούν τις απαιτήσεις για το ρόλο. Όταν ένας χρήστης οριστικά έχει αντιστοιχιστεί σε ένα ρόλο ή ενεργοποιεί μια ανάθεση, το κατάλληλο ρόλο, στη συνέχεια, μπορούν να διαχειρίζονται Azure Active Directory, Office 365 και άλλων εφαρμογών με τα δικαιώματα που έχουν εκχωρηθεί σε ρόλους τους.

Υπάρχει διαφορά όσον αφορά την πρόσβαση που δίνεται σε κάποιον με μια μόνιμη έναντι μια ανάθεση κατάλληλο ρόλο. Η μόνη διαφορά είναι ότι ορισμένα άτομα δεν χρειάζεται ότι η πρόσβαση πάντα. Μπορούν πραγματοποιούνται πληρούν τις απαιτήσεις για το ρόλο και να το ενεργοποιήσετε και απενεργοποίηση όποτε χρειάζεται.

## <a name="roles-managed-in-pim"></a>Ρόλοι διαχείρισης στο PIM

Δικαιώματα διαχείρισης των ταυτοτήτων σάς επιτρέπει να εκχωρήσετε στους χρήστες να κοινά τους ρόλους διαχειριστή, συμπεριλαμβανομένων των:


- **Ο Καθολικός διαχειριστής** (γνωστό και ως διαχειριστής εταιρείας) έχει πρόσβαση σε όλες τις δυνατότητες διαχείρισης. Μπορείτε να έχετε περισσότερες από μία καθολικός διαχειριστής στην εταιρεία σας. Το άτομο που εγγράφεται για να αγοράσετε το Office 365 αυτόματα γίνεται ενός καθολικού διαχειριστή.
- **Προνομιούχους ρόλο διαχειριστή** διαχειρίζεται Azure AD PIM και ενημερώνει τις αναθέσεις ρόλων για άλλους χρήστες.  
- **Διαχειριστής χρεώσεων** πραγματοποιεί αγορές, διαχειρίζεται συνδρομές, διαχειρίζεται δελτία υποστήριξης και παρακολουθεί την εύρυθμη λειτουργία των υπηρεσιών.
- **Διαχειριστής κωδικών πρόσβασης** επαναφέρει τον κωδικό πρόσβασης, διαχειρίζεται αιτήσεις εξυπηρέτησης και παρακολουθεί την εύρυθμη λειτουργία των υπηρεσιών. Διαχειριστές κωδικών περιορίζονται σε επαναφορά κωδικών πρόσβασης για τους χρήστες.
- **Διαχειριστής υπηρεσίας** διαχειρίζεται αιτήσεις εξυπηρέτησης και οθόνες εύρυθμη λειτουργία υπηρεσιών.

  > [AZURE.NOTE] Εάν χρησιμοποιείτε το Office 365, στη συνέχεια, πριν να εκχωρήσετε το ρόλο διαχειριστή υπηρεσιών σε ένα χρήστη, πρώτα αναθέσει τα διαχειριστικά δικαιώματα χρήστη σε μια υπηρεσία, όπως το Exchange Online.

- **Διαχειριστής χρηστών** επαναφέρει τον κωδικό πρόσβασης, παρακολουθεί την εύρυθμη λειτουργία των υπηρεσιών και διαχειρίζεται λογαριασμούς χρηστών, ομάδες χρηστών και τις αιτήσεις εξυπηρέτησης. Τα δικαιώματα διαχειριστή διαχείρισης χρηστών δεν είναι δυνατό να διαγράψετε καθολικός διαχειριστής, δημιουργία άλλους ρόλους διαχειριστή ή επαναφορά κωδικών πρόσβασης για χρεώσεων, τους καθολικούς και τους διαχειριστές υπηρεσιών.
- **Διαχειριστής Exchange** έχει δικαιώματα διαχειριστή στο Exchange Online μέσω του κέντρου διαχείρισης του Exchange (EAC) και μπορεί να εκτελέσει σχεδόν οποιαδήποτε εργασία στο Exchange Online.
- **Διαχειριστής του SharePoint** έχει δικαιώματα διαχειριστή στο SharePoint Online μέσω του κέντρου διαχείρισης του SharePoint Online και μπορεί να εκτελέσει σχεδόν οποιαδήποτε εργασία στο SharePoint Online.
- **Διαχειριστής Skype για επιχειρήσεις** έχει δικαιώματα διαχειριστή στο Skype για επιχειρήσεις μέσω του Skype για επιχειρήσεις Κέντρο διαχείρισης και μπορεί να εκτελέσει σχεδόν οποιαδήποτε εργασία στο Skype για επιχειρήσεις Online.

Διαβάστε αυτά τα άρθρα για περισσότερες λεπτομέρειες σχετικά με την [Εκχώρηση ρόλων διαχειριστή στο Azure AD](active-directory-assign-admin-roles.md) και [Εκχώρηση ρόλων διαχειριστή στο Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Από το πρόγραμμα PIM, μπορείτε να [αντιστοιχίσετε αυτούς τους ρόλους σε ένα χρήστη](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , ώστε ο χρήστης να [ενεργοποιήσετε το ρόλο όταν είναι απαραίτητο](active-directory-privileged-identity-management-how-to-activate-role.md).

Εάν θέλετε να δώσετε μια άλλη πρόσβασης χρήστη για τη Διαχείριση στο PIM ίδια, τους ρόλους που PIM απαιτεί να έχει ο χρήστης περιγράφονται περαιτέρω πώς [να δώσετε πρόσβαση PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Ρόλοι διαχειριζόμενων δεν στο PIM

Ρόλους στο Exchange Online ή το SharePoint Online, εκτός από αυτά που αναφέρονται παραπάνω, δεν αντιπροσωπεύονται στο Azure AD και επομένως δεν είναι ορατές σε PIM. Για περισσότερες πληροφορίες σχετικά με την αλλαγή εκχώρησης ρόλου λεπτομερή σε αυτές τις υπηρεσίες του Office 365, ανατρέξτε στο θέμα [δικαιώματα στο Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure συνδρομές και τις ομάδες πόρων δεν παρουσιάζονται επίσης στο Azure AD. Για να διαχειριστείτε Azure συνδρομές, δείτε [πώς μπορείτε να προσθέσετε ή να αλλάξετε τους ρόλους διαχειριστή του Azure](../billing-add-change-azure-subscription-administrator.md) και για περισσότερες πληροφορίες σχετικά με Azure RBAC ανατρέξτε στο θέμα [Έλεγχος πρόσβασης Azure Role-Based](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Ρόλοι χρήστη και είσοδος
Για ορισμένες υπηρεσίες της Microsoft και εφαρμογές, εκχώρηση χρήστη σε ένα ρόλο μπορεί να μην επαρκούν για να ενεργοποιήσετε αυτόν το χρήστη να είστε διαχειριστής.

Πρόσβαση στην πύλη του Azure κλασική απαιτεί ο χρήστης έχει ένα διαχειριστή της υπηρεσίας ή διαχειριστής από κοινού σε μια συνδρομή του Azure ακόμα και εάν ο χρήστης δεν χρειάζεται να διαχειριστείτε τις συνδρομές Azure.  Για παράδειγμα, για να διαχειριστείτε ρυθμίσεις παραμέτρων για το Azure AD στην κλασική πύλη, ένας χρήστης πρέπει να καθολικός διαχειριστής στο Azure AD και ένας διαχειριστής από κοινού τη συνδρομή σε μια συνδρομή του Azure.  Για να μάθετε πώς μπορείτε να προσθέσετε χρήστες στο Azure συνδρομές, δείτε [πώς μπορείτε να προσθέσετε ή να αλλάξετε τους ρόλους διαχειριστή του Azure](../billing-add-change-azure-subscription-administrator.md).

Πρόσβαση σε υπηρεσίες Microsoft Online Services μπορεί να απαιτεί στον χρήστη επίσης να εκχωρηθεί μια άδεια χρήσης πριν να ανοίξετε την υπηρεσία πύλης ή εκτέλεση εργασιών διαχείρισης.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Εκχώρηση άδειας χρήσης σε ένα χρήστη στο Azure AD

1. Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη] (http://manage.windowsazure.com) με ένα λογαριασμό καθολικού διαχειριστή ή ένα λογαριασμό διαχειριστή από κοινού.
2. Επιλογή **Όλων των στοιχείων** από το κύριο μενού.
3. Επιλέξτε τον κατάλογο που θέλετε να εργαστείτε με και που διαθέτει άδειες χρήσης που έχει συσχετιστεί με αυτό.
4. Επιλέξτε **άδειες χρήσης**. Θα εμφανιστεί η λίστα των διαθέσιμων αδειών χρήσης.
5. Επιλέξτε το πρόγραμμα άδειας χρήσης, το οποίο περιέχει τις άδειες χρήσης που θέλετε να διανείμετε.
6. Επιλέξτε **εκχωρήσετε στους χρήστες**.
7. Επιλέξτε το χρήστη που θέλετε να εκχωρήσετε μια άδεια χρήσης.
8. Κάντε κλικ στο κουμπί **Αντιστοίχιση** .  Ο χρήστης μπορεί να πραγματοποιήστε είσοδο Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Επόμενα βήματα
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
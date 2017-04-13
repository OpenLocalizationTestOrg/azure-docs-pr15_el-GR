<properties
    pageTitle="Προσθέστε χρήστες από άλλους καταλόγους ή συνεργάτη εταιρείες στην προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Εξηγεί πώς μπορείτε να προσθέσετε χρήστες ή αλλαγή πληροφοριών χρήστη στο Azure Active Directory, συμπεριλαμβανομένων των εξωτερικών και επισκέπτη χρηστών."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Προσθήκη χρηστών από άλλους καταλόγους ή οι εταιρείες συνεργάτες σε προεπισκόπηση Azure Active Directory

> [AZURE.SELECTOR]
- [Πύλη του Azure](active-directory-users-create-external-azure-portal.md)
- [Azure κλασική πύλη](active-directory-create-users-external.md)

Σε αυτό το άρθρο εξηγεί πώς να προσθέτετε χρήστες από άλλους καταλόγους στην προεπισκόπηση Azure Active Directory (Azure AD) ή από εταιρείες συνεργάτες. [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Για πληροφορίες σχετικά με την προσθήκη νέων χρηστών στην εταιρεία σας και προσθέσετε χρήστες που διαθέτουν λογαριασμούς Microsoft, ανατρέξτε στο θέμα [Προσθήκη νέων χρηστών σε Azure Active Directory](active-directory-users-create-azure-portal.md). Προστέθηκε οι χρήστες δεν έχουν δικαιώματα διαχειριστή από προεπιλογή, αλλά μπορείτε να εκχωρήσετε ρόλους σε αυτούς οποιαδήποτε στιγμή.

## <a name="add-a-user"></a>Προσθήκη χρήστη

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2.  Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρήστες και ομάδες** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

    ![Άνοιγμα Διαχείριση χρηστών](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Στην blade **χρήστες και ομάδες** , επιλέξτε **τους χρήστες**και, στη συνέχεια, επιλέξτε **Προσθήκη**.

    ![Επιλέγοντας την εντολή Προσθήκη](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Στη blade του **χρήστη** , δώστε ένα εμφανιζόμενο όνομα στο πλαίσιο **όνομα** και όνομα εισόδου του χρήστη στο πλαίσιο **όνομα χρήστη**.

5. Αντιγράψτε ή σημειώστε διαφορετικά τον κωδικό πρόσβασης του χρήστη που έχει δημιουργηθεί, ώστε να μπορείτε να παρέχετε το στο χρήστη μετά την ολοκλήρωση αυτής της διαδικασίας.

6. Προαιρετικά, επιλέξτε **το προφίλ** για να προσθέσετε τους χρήστες πρώτα και επωνύμου, έναν τίτλο εργασίας και όνομα ενός τμήματος.
    
    ![Άνοιγμα προφίλ χρήστη](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Επιλέξτε **ομάδες** για να προσθέσετε το χρήστη σε μία ή περισσότερες ομάδες.

        ![Προσθήκη ενός χρήστη σε ομάδες](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Επιλέξτε **εταιρικό ρόλο** για να εκχωρήσετε στο χρήστη σε ένα ρόλο από τη λίστα **ρόλων** . Για περισσότερες πληροφορίες σχετικά με τους ρόλους χρήστη και διαχειριστή, ανατρέξτε στο θέμα [Εκχώρηση ρόλων διαχειριστή στο Azure AD](active-directory-assign-admin-roles.md).

        ![Εκχώρηση χρήστη σε ένα ρόλο](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Επιλέξτε **Δημιουργία**.

8. Διανομή με ασφάλεια τον κωδικό πρόσβασης που δημιουργήθηκε στο νέο χρήστη έτσι, ώστε ο χρήστης να εισέλθετε.

> [AZURE.IMPORTANT] Εάν η εταιρεία σας χρησιμοποιεί περισσότερους από έναν τομείς, θα πρέπει να γνωρίζετε σχετικά με τα ακόλουθα προβλήματα όταν προσθέτετε ένα λογαριασμό χρήστη:
>
> - Για να προσθέσετε λογαριασμούς χρηστών με το ίδιο κύριο όνομα χρήστη (UPN) σε όλους τους τομείς, **πρώτη** προσθέσετε, για παράδειγμα, geoffgrisso@contoso.onmicrosoft.com, **ακολουθούμενο από** geoffgrisso@contoso.com.
> - **Χωρίς** Προσθήκη geoffgrisso@contoso.com πριν από την προσθήκη geoffgrisso@contoso.onmicrosoft.com. Αυτή η σειρά είναι σημαντικό και μπορεί να είναι εύκολα για να αναιρέσετε.

Εάν αλλάξετε τις πληροφορίες για ένα χρήστη του οποίου η ταυτότητα έχει συγχρονιστεί με την υπηρεσία καταλόγου Active Directory εσωτερικής εγκατάστασης, δεν μπορείτε να αλλάξετε τις πληροφορίες χρήστη στην πύλη του Azure κλασική. Για να αλλάξετε τις πληροφορίες χρήστη, χρησιμοποιήστε τα εργαλεία διαχείρισης της υπηρεσίας καταλόγου Active Directory εσωτερικής εγκατάστασης.


## <a name="whats-next"></a>Τι ακολουθεί

- [Προσθήκη χρήστη](active-directory-users-create-azure-portal.md)
- [Επαναφορά κωδικού πρόσβασης ενός χρήστη στην πύλη του νέου Azure](active-directory-users-reset-password-azure-portal.md)
- [Εκχώρηση χρήστη σε ένα ρόλο στο σας Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Αλλαγή των πληροφοριών εργασίας ενός χρήστη](active-directory-users-work-info-azure-portal.md)
- [Διαχείριση προφίλ χρηστών](active-directory-users-profile-azure-portal.md)
- [Διαγραφή χρήστη στο σας Azure AD](active-directory-users-delete-user-azure-portal.md)

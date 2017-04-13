<properties
    pageTitle="Προσθήκη νέων χρηστών σε προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Εξηγεί πώς μπορείτε να προσθέσετε νέους χρήστες ή αλλαγή πληροφοριών χρήστη στην υπηρεσία καταλόγου Azure Active Directory."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Προσθήκη νέων χρηστών σε προεπισκόπηση Azure Active Directory

> [AZURE.SELECTOR]
- [Πύλη του Azure](active-directory-users-create-azure-portal.md)
- [Azure κλασική πύλη](active-directory-create-users.md)

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να προσθέσετε νέους χρήστες στην εταιρεία σας στην προεπισκόπηση Azure Active Direstory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md)

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2.  Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρήστες και ομάδες** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

    ![Άνοιγμα Διαχείριση χρηστών](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Στην blade **χρήστες και ομάδες** , επιλέξτε **όλους τους χρήστες**και, στη συνέχεια, επιλέξτε **Προσθήκη**.

    ![Επιλέγοντας την εντολή Προσθήκη](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Πληκτρολογήστε λεπτομέρειες για το χρήστη, όπως το **όνομα** και το **όνομα χρήστη**. Το τμήμα του ονόματος τομέα από το όνομα χρήστη πρέπει να είναι το αρχικό προεπιλεγμένο όνομα τομέα του ονόματος τομέα "foo.onmicrosoft.com" ή ένα όνομα επαληθευτεί, μη ομόσπονδο τομέα όπως "contoso.com."

5. Αντιγράψτε ή σημειώστε διαφορετικά τον κωδικό πρόσβασης του χρήστη που έχει δημιουργηθεί, ώστε να μπορείτε να παρέχετε το στο χρήστη μετά την ολοκλήρωση αυτής της διαδικασίας.

6. Προαιρετικά, μπορείτε να ανοίξετε και συμπληρώστε τις πληροφορίες στο blade το **προφίλ** , το blade **ομάδες** ή το blade **ρόλο καταλόγου** για το χρήστη. Για περισσότερες πληροφορίες σχετικά με τους ρόλους χρήστη και διαχειριστή, ανατρέξτε στο θέμα [Εκχώρηση ρόλων διαχειριστή στο Azure AD](active-directory-assign-admin-roles.md).

7.  Στην blade του **χρήστη** , επιλέξτε **Δημιουργία**.

8. Διανομή με ασφάλεια τον κωδικό πρόσβασης που δημιουργήθηκε στο νέο χρήστη έτσι, ώστε ο χρήστης να εισέλθετε.

## <a name="whats-next"></a>Τι ακολουθεί

- [Προσθήκη ενός εξωτερικού χρήστη](active-directory-users-create-external-azure-portal.md)
- [Επαναφορά κωδικού πρόσβασης ενός χρήστη στην πύλη του νέου Azure](active-directory-users-reset-password-azure-portal.md)
- [Αλλαγή των πληροφοριών εργασίας ενός χρήστη](active-directory-users-work-info-azure-portal.md)
- [Διαχείριση προφίλ χρηστών](active-directory-users-profile-azure-portal.md)
- [Διαγραφή χρήστη στο σας Azure AD](active-directory-users-delete-user-azure-portal.md)
- [Εκχώρηση χρήστη σε ένα ρόλο στο σας Azure AD](active-directory-users-assign-role-azure-portal.md)

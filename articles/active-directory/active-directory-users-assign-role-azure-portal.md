<properties
    pageTitle="Αντιστοίχιση χρήστη σε ρόλους διαχειριστή σε προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Εξηγεί πώς μπορείτε να αλλάξετε διαχείρισης πληροφοριών χρήστη στην υπηρεσία καταλόγου Azure Active Directory"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Αντιστοίχιση χρήστη σε ρόλους διαχειριστή σε προεπισκόπηση Azure Active Directory

Σε αυτό το άρθρο εξηγεί πώς να εκχωρήσουν ρόλο διαχειριστή σε ένα χρήστη σε προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Για πληροφορίες σχετικά με την προσθήκη τους νέους χρήστες στην εταιρεία σας, ανατρέξτε στο θέμα [Προσθήκη νέων χρηστών σε Azure Active Directory](active-directory-users-create-azure-portal.md). Προστέθηκε οι χρήστες δεν έχουν δικαιώματα διαχειριστή από προεπιλογή, αλλά μπορείτε να εκχωρήσετε ρόλους σε αυτούς οποιαδήποτε στιγμή.

## <a name="assign-a-role-to-a-user"></a>Εκχώρηση ρόλου σε ένα χρήστη

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2.  Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρήστες και ομάδες** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

    ![Άνοιγμα Διαχείριση χρηστών](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Στην blade **χρήστες και ομάδες** , επιλέξτε **όλους τους χρήστες**.

    ![Άνοιγμα του blade όλες οι χρήστες](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Στην blade **χρήστες και ομάδες - όλους τους χρήστες** , επιλέξτε ένα χρήστη από τη λίστα.

5. Στην το blade για τον επιλεγμένο χρήστη, επιλέξτε **το ρόλο καταλόγου**και, στη συνέχεια, να εκχωρήσετε στο χρήστη σε ένα ρόλο από τη λίστα **καταλόγου ρόλο** . Για περισσότερες πληροφορίες σχετικά με τους ρόλους χρήστη και διαχειριστή, ανατρέξτε στο θέμα [Εκχώρηση ρόλων διαχειριστή στο Azure AD](active-directory-assign-admin-roles.md).

      ![Εκχώρηση χρήστη σε ένα ρόλο](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Επιλέξτε **Αποθήκευση**.


## <a name="whats-next"></a>Τι ακολουθεί

- [Προσθήκη χρήστη](active-directory-users-create-azure-portal.md)
- [Επαναφορά κωδικού πρόσβασης ενός χρήστη στην πύλη του νέου Azure](active-directory-users-reset-password-azure-portal.md)
- [Αλλαγή των πληροφοριών εργασίας ενός χρήστη](active-directory-users-work-info-azure-portal.md)
- [Διαχείριση προφίλ χρηστών](active-directory-users-profile-azure-portal.md)
- [Διαγραφή χρήστη στο σας Azure AD](active-directory-users-delete-user-azure-portal.md)

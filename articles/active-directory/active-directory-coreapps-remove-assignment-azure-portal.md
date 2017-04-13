<properties
    pageTitle="Κατάργηση μιας ανάθεσης χρήστη ή μια ομάδα από μια εφαρμογή για μεγάλες επιχειρήσεις σε προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να καταργήσετε την αντιστοίχιση της access από ένα χρήστη ή μια ομάδα από μια εφαρμογή για μεγάλες επιχειρήσεις στο Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Καταργείτε ένα χρήστη ή μια ανάθεση σε ομάδα από μια εφαρμογή για μεγάλες επιχειρήσεις σε προεπισκόπηση Azure Active Directory

Είναι εύκολο να καταργήσετε ένα χρήστη ή μια ομάδα από την αντιστοίχιση πρόσβαση σε μία από τις εφαρμογές της επιχείρησής σας σε προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Πρέπει να έχετε τα κατάλληλα δικαιώματα για τη διαχείριση της εφαρμογής για μεγάλες επιχειρήσεις. Στην τρέχουσα προεπισκόπηση, πρέπει να είστε καθολικός διαχειριστής για τον κατάλογο.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Πώς μπορώ να καταργήσω ένα χρήστη ή μια ανάθεση σε ομάδα;

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2. Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **Azure Active Directory** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

3. Στην το * *Azure Active Directory - *όνομα_καταλόγου* ** blade (δηλαδή, το Azure AD blade για τον κατάλογο διαχειρίζεστε), επιλέξτε **Enterprise εφαρμογές **.

    ![Άνοιγμα εταιρικών εφαρμογών σας](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Στην το blade **εφαρμογές για μεγάλες επιχειρήσεις** , επιλέξτε **όλες τις εφαρμογές**. Θα δείτε μια λίστα με τις εφαρμογές που μπορείτε να διαχειριστείτε.

5. Στην το blade **εταιρικές εφαρμογές - όλες τις εφαρμογές** , επιλέξτε μια εφαρμογή.

6. Στην blade ***όνομα_εφαρμογής*** (δηλαδή, το blade με το όνομα της επιλεγμένης εφαρμογής στον τίτλο), επιλέξτε **χρήστες και ομάδες**.

    ![Επιλογή χρηστών ή ομάδων](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Στην τη ***όνομα_εφαρμογής*** **-χρήστη & ανάθεση σε ομάδα** blade, επιλέξτε μία από περισσότερους χρήστες ή ομάδες και, στη συνέχεια, επιλέξτε την εντολή **Κατάργηση** . Επιβεβαιώστε την απόφασή σας στη γραμμή εντολών.

    ![Επιλέγοντας την εντολή Κατάργηση](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Επόμενα βήματα

- [Δείτε όλες μου ομάδες](active-directory-groups-view-azure-portal.md)
- [Εκχώρηση χρήστη ή ομάδας σε μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-assign-user-azure-portal.md)
- [Απενεργοποιήσετε πρόσθετα εισόδου χρήστη για μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-disable-app-azure-portal.md)
- [Αλλάξτε το όνομα ή το λογότυπο της εφαρμογής για μεγάλες επιχειρήσεις](active-directory-coreapps-change-app-logo-user-azure-portal.md)

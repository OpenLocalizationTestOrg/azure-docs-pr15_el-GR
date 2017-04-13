<properties
    pageTitle="Απενεργοποιήσετε πρόσθετα εισόδου χρήστη για μια εφαρμογή για μεγάλες επιχειρήσεις σε προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να απενεργοποιήσετε μια εφαρμογή για μεγάλες επιχειρήσεις, έτσι ώστε να χωρίς οι χρήστες ενδέχεται να συνδέονται σε αυτήν στο Azure Active Directory"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Απενεργοποιήσετε πρόσθετα εισόδου χρήστη για μια εφαρμογή για μεγάλες επιχειρήσεις σε προεπισκόπηση Azure Active Directory

Είναι εύκολο να απενεργοποιήσετε μια εφαρμογή για μεγάλες επιχειρήσεις, έτσι ώστε κανένας χρήστης μπορεί να εισέλθετε σε αυτό σε προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Πρέπει να έχετε τα κατάλληλα δικαιώματα για τη διαχείριση της εφαρμογής για μεγάλες επιχειρήσεις. Στην τρέχουσα προεπισκόπηση, πρέπει να είστε καθολικός διαχειριστής για τον κατάλογο.

## <a name="how-do-i-disable-user-sign-ins"></a>Πώς μπορώ να απενεργοποιήσω πρόσθετα εισόδου χρήστη;

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2. Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **Azure Active Directory** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

3. Στην το * *Azure Active Directory - *όνομα_καταλόγου* ** blade (δηλαδή, το Azure AD blade για τον κατάλογο διαχειρίζεστε), επιλέξτε **Enterprise εφαρμογές **.

    ![Άνοιγμα εταιρικών εφαρμογών σας](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Στην το blade **εφαρμογές για μεγάλες επιχειρήσεις** , επιλέξτε **όλες τις εφαρμογές**. Μπορείτε να δείτε μια λίστα με τις εφαρμογές που μπορείτε να διαχειριστείτε.

5. Στην το blade **εταιρικές εφαρμογές - όλες τις εφαρμογές** , επιλέξτε μια εφαρμογή.

6. Στην blade ***όνομα_εφαρμογής*** (δηλαδή, το blade με το όνομα της επιλεγμένης εφαρμογής στον τίτλο), επιλέξτε **Ιδιότητες**.

    ![Επιλέγοντας την εντολή όλες τις εφαρμογές](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Στην τη ***όνομα_εφαρμογής*** **-Ιδιότητες** blade, επιλέξτε **όχι** για **ενεργοποιημένες για τους χρήστες για να εισέλθετε;**.

8. Επιλέξτε την εντολή **Αποθήκευση** .

## <a name="next-steps"></a>Επόμενα βήματα

- [Δείτε όλες τις ομάδες μου](active-directory-groups-view-azure-portal.md)
- [Εκχώρηση χρήστη ή ομάδας σε μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-assign-user-azure-portal.md)
- [Κατάργηση μιας ανάθεσης χρήστη ή μια ομάδα από μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Αλλάξτε το όνομα ή το λογότυπο της εφαρμογής για μεγάλες επιχειρήσεις](active-directory-coreapps-change-app-logo-user-azure-portal.md)

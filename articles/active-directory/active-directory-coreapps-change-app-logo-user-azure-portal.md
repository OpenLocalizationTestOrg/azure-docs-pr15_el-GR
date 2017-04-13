<properties
    pageTitle="Αλλάξτε το όνομα ή το λογότυπο της εταιρείας εφαρμογής στην προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να αλλάξετε το όνομα ή το λογότυπο για μια εφαρμογή προσαρμοσμένου εταιρικού στο Azure Active Directory"
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

# <a name="change-the-name-or-logo-of-an-enterprise-app-in-azure-active-directory-preview"></a>Αλλάξτε το όνομα ή το λογότυπο της εταιρείας εφαρμογής στην προεπισκόπηση Azure Active Directory

Είναι εύκολο να αλλάξετε το όνομα ή το λογότυπο για μια εφαρμογή προσαρμοσμένου εταιρικού στην προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Πρέπει να έχετε τα κατάλληλα δικαιώματα για να κάνετε αυτές τις αλλαγές. Στην τρέχουσα προεπισκόπηση, πρέπει να είστε ο δημιουργός της προσαρμοσμένης εφαρμογής.

## <a name="how-do-i-change-an-enterprise-apps-name-or-logo"></a>Πώς μπορώ να αλλάξω το όνομα ή το λογότυπο της εφαρμογής για μεγάλες επιχειρήσεις;

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2. Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **Azure Active Directory** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

3. Στην το * *Azure Active Directory - *όνομα_καταλόγου* ** blade (δηλαδή, το Azure AD blade για τον κατάλογο διαχειρίζεστε), επιλέξτε **Enterprise εφαρμογές **.

    ![Άνοιγμα εταιρικών εφαρμογών σας](./media/active-directory-coreapps-change-app-logo-azure-portal/open-enterprise-apps.png)

4. Στην το blade **εφαρμογές για μεγάλες επιχειρήσεις** , επιλέξτε **όλες τις εφαρμογές**. Θα δείτε μια λίστα με τις εφαρμογές που μπορείτε να διαχειριστείτε.

5. Στην το blade **εταιρικές εφαρμογές - όλες τις εφαρμογές** , επιλέξτε μια εφαρμογή.

6. Στην blade ***όνομα_εφαρμογής*** (δηλαδή, το blade με το όνομα της επιλεγμένης εφαρμογής στον τίτλο), επιλέξτε **Ιδιότητες**.

    ![Επιλέγοντας την εντολή Ιδιότητες](./media/active-directory-coreapps-change-app-logo-azure-portal/select-app.png)

7. Στην τη ***όνομα_εφαρμογής*** **-Ιδιότητες** blade, πραγματοποιήστε αναζήτηση για ένα αρχείο για να χρησιμοποιήσετε ως νέα ένα λογότυπο ή να επεξεργαστείτε το όνομα εφαρμογής, ή και τα δύο.

    ![Αλλαγή της εφαρμογής λογότυπου ή nameproperties εντολής](./media/active-directory-coreapps-change-app-logo-azure-portal/change-logo.png)

8. Επιλέξτε την εντολή **Αποθήκευση** .

## <a name="next-steps"></a>Επόμενα βήματα

- [Δείτε όλες μου ομάδες](active-directory-groups-view-azure-portal.md)
- [Εκχώρηση χρήστη ή ομάδας σε μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-assign-user-azure-portal.md)
- [Κατάργηση μιας ανάθεσης χρήστη ή μια ομάδα από μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Απενεργοποιήσετε πρόσθετα εισόδου χρήστη για μια εφαρμογή για μεγάλες επιχειρήσεις](active-directory-coreapps-disable-app-azure-portal.md)

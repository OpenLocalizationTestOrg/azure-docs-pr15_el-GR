<properties
    pageTitle="Διαχειριστείτε τις ομάδες σας η ομάδα είναι μέλος στην προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Οι ομάδες μπορούν να περιέχουν άλλες ομάδες στο Azure Active Directory. Παρακάτω θα δείτε πώς μπορείτε να διαχειριστείτε αυτά τα μέλη."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Διαχειριστείτε τις ομάδες σας η ομάδα είναι μέλος στην προεπισκόπηση Azure Active Directory

Οι ομάδες μπορούν να περιέχουν άλλες ομάδες στην προεπισκόπηση Azure Active Directory. [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Παρακάτω θα δείτε πώς μπορείτε να διαχειριστείτε αυτά τα μέλη.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Πώς μπορώ να βρω τις ομάδες, η ομάδα μου είναι μέλος;

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2.  Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρήστες και ομάδες** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

  ![Άνοιγμα Διαχείριση χρηστών](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Στην blade **χρήστες και ομάδες** , επιλέξτε **όλες τις ομάδες**.

  ![Άνοιγμα του blade ομάδες](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Στην blade **χρήστες και ομάδες - όλες τις ομάδες** , επιλέξτε μια ομάδα.

5. Στην το * *Group - *όνομα ομάδας* ** blade, επιλέξτε **ομάδα μελών **.

  ![Άνοιγμα του blade μελών ομάδας](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Για να προσθέσετε την ομάδα σας ως μέλος της ομάδας, μια άλλη, στην την blade **ομάδα - μέλη ομάδας** , επιλέξτε την εντολή " **Προσθήκη** ".

7. Επιλέξτε μια ομάδα από την **Επιλογή ομάδας** blade και, στη συνέχεια, επιλέξτε το κουμπί " **επιλογή** " στο κάτω μέρος του blade. Μπορείτε να προσθέσετε την ομάδα σας σε μία μόνο ομάδα κάθε φορά. Πλαίσιο **χρήστη** φιλτράρει την εμφάνιση που βασίζονται σε που ταιριάζουν με την καταχώρησή σας σε οποιοδήποτε τμήμα ενός ονόματος χρήστη ή συσκευή. Δεν υπάρχει χαρακτήρες μπαλαντέρ γίνονται δεκτές στο πλαίσιο.

  ![Προσθέστε μια ιδιότητα μέλους ομάδας](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Για να καταργήσετε την ομάδα σας ως μέλος της ομάδας, μια άλλη, στην την blade **ομάδα - μέλη ομάδας** , επιλέξτε μια ομάδα.

9. Στην blade το ***όνομα ομάδας*** , επιλέξτε την εντολή **Κατάργηση** και επιβεβαιώστε την επιλογή σας στη γραμμή εντολών.

  ![Κατάργηση εντολής συμμετοχή ως μέλος](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Όταν ολοκληρώσετε την αλλαγή των ιδιοτήτων μέλους ομάδας για την ομάδα σας, επιλέξτε **Αποθήκευση**.


## <a name="additional-information"></a>Πρόσθετες πληροφορίες

Τα άρθρα αυτά παρέχουν πρόσθετες πληροφορίες σχετικά με Azure Active Directory.

* [Ανατρέξτε στο θέμα υπάρχουσες ομάδες](active-directory-groups-view-azure-portal.md)
* [Δημιουργήστε μια νέα ομάδα και προσθήκη μελών](active-directory-groups-create-azure-portal.md)
* [Διαχείριση ρυθμίσεων ομάδας](active-directory-groups-settings-azure-portal.md)
* [Διαχείριση μελών μιας ομάδας](active-directory-groups-members-azure-portal.md)
* [Διαχείριση δυναμικών κανόνων για τους χρήστες σε μια ομάδα](active-directory-groups-dynamic-membership-azure-portal.md)

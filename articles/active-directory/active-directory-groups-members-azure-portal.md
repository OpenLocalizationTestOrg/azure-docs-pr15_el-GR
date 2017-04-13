<properties
    pageTitle="Διαχειριστείτε τα μέλη για μια ομάδα στην προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να τους χρήστες και τις συσκευές που είναι μέλη της ομάδας στο Azure Active Directory"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Διαχειριστείτε τα μέλη για μια ομάδα στην προεπισκόπηση Azure Active Directory

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να διαχειριστείτε τα μέλη για μια ομάδα στην προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Πώς να βρείτε τα μέλη και να διαχειριστείτε τους;

1.  Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2.  Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρήστες και ομάδες** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

  ![Άνοιγμα Διαχείριση χρηστών](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Στην blade **χρήστες και ομάδες** , επιλέξτε **όλες τις ομάδες**.

  ![Άνοιγμα του blade ομάδες](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Στην blade **χρήστες και ομάδες - όλες τις ομάδες** , επιλέξτε μια ομάδα.

5. Στην την * *Ομαδοποίηση - *όνομα ομάδας* ** blade, επιλογή **μελών **.

  ![Άνοιγμα του blade μελών](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Για να προσθέσετε μέλη στην ομάδα, το blade **ομάδα - μέλη** , επιλέξτε **Προσθήκη μελών**.

  ![Εντολή "Προσθήκη μελών"](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Στην blade τα **μέλη** , επιλέξτε έναν ή περισσότερους χρήστες ή τις συσκευές για να προσθέσετε στην ομάδα και επιλέξτε το κουμπί " **επιλογή** " στο κάτω μέρος του blade για να τα προσθέσετε στην ομάδα. Πλαίσιο **χρήστη** φιλτράρει την εμφάνιση που βασίζονται σε που ταιριάζουν με την καταχώρησή σας σε οποιοδήποτε τμήμα ενός ονόματος χρήστη ή συσκευή. Δεν υπάρχει χαρακτήρες μπαλαντέρ γίνονται δεκτές στο πλαίσιο.

8. Για να καταργήσετε μέλη από την ομάδα, στην την blade **ομάδα - μέλη** , επιλέξτε ένα μέλος.

9. Στην blade το ***όνομα μέλους*** , επιλέξτε την εντολή **Κατάργηση** και επιβεβαιώστε την επιλογή σας στη γραμμή εντολών.

  ![Κατάργηση εντολής μελών](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Όταν ολοκληρώσετε την αλλαγή των μελών της ομάδας, επιλέξτε **Αποθήκευση**.


## <a name="additional-information"></a>Πρόσθετες πληροφορίες

Τα άρθρα αυτά παρέχουν πρόσθετες πληροφορίες σχετικά με Azure Active Directory.

* [Ανατρέξτε στο θέμα υπάρχουσες ομάδες](active-directory-groups-view-azure-portal.md)
* [Δημιουργήστε μια νέα ομάδα και προσθήκη μελών](active-directory-groups-create-azure-portal.md)
* [Διαχείριση ρυθμίσεων ομάδας](active-directory-groups-settings-azure-portal.md)
* [Διαχείριση μελών μιας ομάδας](active-directory-groups-membership-azure-portal.md)
* [Διαχείριση δυναμικών κανόνων για τους χρήστες σε μια ομάδα](active-directory-groups-dynamic-membership-azure-portal.md)

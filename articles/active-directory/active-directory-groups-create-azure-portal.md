<properties
    pageTitle="Δημιουργήστε μια νέα ομάδα στην προεπισκόπηση Azure Active Directory | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια ομάδα στην υπηρεσία καταλόγου Azure Active Directory και προσθήκη χρηστών (μέλη) στην ομάδα"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Δημιουργήστε μια νέα ομάδα στην προεπισκόπηση Azure Active Directory

> [AZURE.SELECTOR]
- [Πύλη του Azure](active-directory-groups-create-azure-portal.md)
- [Azure κλασική πύλη](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Σε αυτό το άρθρο εξηγεί πώς μπορείτε να δημιουργήσετε και να συμπληρώσετε μια νέα ομάδα στην προεπισκόπηση Azure Active Directory (Azure AD). [Τι περιέχει η προεπισκόπηση;](active-directory-preview-explainer.md) Χρησιμοποιήστε μια ομάδα για την εκτέλεση εργασιών διαχείρισης, όπως η εκχώρηση αδειών χρήσης ή δικαιώματα σε έναν αριθμό από τους χρήστες ή τις συσκευές ταυτόχρονα.

## <a name="how-do-i-create-a-group"></a>Πώς μπορώ να δημιουργήσω μια ομάδα;

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com) με ένα λογαριασμό που είναι καθολικός διαχειριστής για τον κατάλογο.

2. Επιλέξτε **περισσότερες υπηρεσίες**, εισαγάγετε **χρηστών και ομάδων** στο πλαίσιο κειμένου και, στη συνέχεια, πατήστε το **πλήκτρο Enter**.

  ![Διαχείριση χρηστών ανοίγματος](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Στην blade **χρήστες και ομάδες** , επιλέξτε **όλες τις ομάδες**.

  ![Άνοιγμα του blade ομάδες](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Στο blade **χρήστες και ομάδες - όλες τις ομάδες** , επιλέξτε την εντολή **Προσθήκη** .

  ![Επιλέγοντας την εντολή Προσθήκη](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Στην blade την **ομάδα** , προσθέστε ένα όνομα και μια περιγραφή για την ομάδα.

6. Για να επιλέξετε τα μέλη για να προσθέσετε την ομάδα, επιλέξτε **ανάθεση** στο πλαίσιο **Τύπος συμμετοχή ως μέλος** και, στη συνέχεια, επιλέξτε **τα μέλη**. Για περισσότερες πληροφορίες σχετικά με τη διαχείριση των μελών μιας ομάδας δυναμικά, ανατρέξτε στο θέμα [Χρήση χαρακτηριστικά για τη δημιουργία σύνθετων κανόνων για την ιδιότητα μέλους ομάδας](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Επιλογή για να προσθέσετε τα μέλη](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Στην blade τα **μέλη** , επιλέξτε έναν ή περισσότερους χρήστες ή τις συσκευές για να προσθέσετε στην ομάδα και επιλέξτε το κουμπί " **επιλογή** " στο κάτω μέρος του blade για να τα προσθέσετε στην ομάδα. Πλαίσιο **χρήστη** φιλτράρει την εμφάνιση που βασίζονται σε που ταιριάζουν με την καταχώρησή σας σε οποιοδήποτε τμήμα ενός ονόματος χρήστη ή συσκευή. Δεν υπάρχει χαρακτήρες μπαλαντέρ γίνονται δεκτές στο πλαίσιο.

6. Όταν ολοκληρώσετε την προσθήκη μελών σε ομάδα, επιλέξτε **Δημιουργία** στην blade την **ομάδα** .    

  ![Δημιουργία ομάδας επιβεβαίωσης](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Πρόσθετες πληροφορίες

Τα άρθρα αυτά παρέχουν πρόσθετες πληροφορίες σχετικά με Azure Active Directory.

* [Ανατρέξτε στο θέμα υπάρχουσες ομάδες](active-directory-groups-view-azure-portal.md)
* [Διαχείριση ρυθμίσεων ομάδας](active-directory-groups-settings-azure-portal.md)
* [Διαχείριση μελών μιας ομάδας](active-directory-groups-members-azure-portal.md)
* [Διαχείριση μελών μιας ομάδας](active-directory-groups-membership-azure-portal.md)
* [Διαχείριση δυναμικών κανόνων για τους χρήστες σε μια ομάδα](active-directory-groups-dynamic-membership-azure-portal.md)

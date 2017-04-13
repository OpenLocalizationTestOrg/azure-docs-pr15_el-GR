<properties
    pageTitle="Αντιστοίχιση χρηστών σε ένα προσαρμοσμένο τομέα στο Azure Active Directory | Microsoft Azure"
    description="Μάθετε πώς να συμπληρώσετε έναν προσαρμοσμένο τομέα στο Azure Active Directory με τους λογαριασμούς χρηστών."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="assign-users-to-a-custom-domain"></a>Αντιστοίχιση χρηστών σε ένα προσαρμοσμένο τομέα

Αφού προσθέσετε τον προσαρμοσμένο τομέα σας να Azure Active Directory, πρέπει να προσθέσετε τους λογαριασμούς χρηστών για αυτόν τον τομέα, έτσι ώστε να μπορείτε να ξεκινήσετε τον έλεγχο ταυτότητας τους.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Οι χρήστες που συγχρονίζονται στο από έναν κατάλογο στο εταιρικό σας δίκτυο

Εάν έχετε ήδη ορίσει μια σύνδεση μεταξύ του εσωτερικής εγκατάστασης υπηρεσίας καταλόγου Active Directory και Azure Active Directory, συγχρονισμού να συμπληρώσετε τους λογαριασμούς. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να συγχρονίσετε Azure Active Directory με σας στην εσωτερική εγκατάσταση υπηρεσίας καταλόγου Active Directory, ανατρέξτε στο θέμα [ενοποίηση σας ταυτοτήτων εσωτερικής εγκατάστασης με Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Στους χρήστες να προσθέσει και διαχειριζόμενων στο cloud

Για να αλλάξετε τον τομέα για έναν υπάρχοντα λογαριασμό χρήστη:

1.  Στην πύλη του Azure κλασική χρησιμοποιώντας ένα λογαριασμό που είναι καθολικός διαχειριστής ή διαχειριστής χρηστών

2.  Ανοίξτε τον κατάλογο.

3.  Επιλέξτε την καρτέλα **χρήστες** .

4.  Επιλέξτε το χρήστη από τη λίστα.

5.  Αλλάξτε τον τομέα για το χρήστη και, στη συνέχεια, επιλέξτε **Αποθήκευση**.

Αυτό μπορεί να γίνει με τη χρήση [Του PowerShell Microsoft](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) ή το [API του γραφήματος](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Επιλέξτε ένα προσαρμοσμένο τομέα κατά τη δημιουργία νέου χρήστη

1.  Στην πύλη του Azure κλασική χρησιμοποιώντας ένα λογαριασμό που είναι καθολικός διαχειριστής ή διαχειριστής χρηστών

2.  Ανοίξτε τον κατάλογο.

3.  Επιλέξτε την καρτέλα **χρήστες** .

4.  Στη γραμμή εντολών, επιλέξτε **Προσθήκη**.

5.  Όταν προσθέτετε το όνομα χρήστη, επιλέξτε τον προσαρμοσμένο τομέα από τη λίστα τομέων.

6.  Επιλέξτε **Αποθήκευση**.

## <a name="next-steps"></a>Επόμενα βήματα

-   [Χρήση προσαρμοσμένου τομέα ονομάτων για να απλοποιήσετε την εμπειρία εισόδου για τους χρήστες σας](active-directory-add-domain.md)

-   [Διαχείριση προσαρμοσμένα ονόματα τομέα](active-directory-add-manage-domain-names.md)

-   [Μάθετε σχετικά με τα έννοιες διαχείρισης τομέα Azure AD](active-directory-add-domain-concepts.md)

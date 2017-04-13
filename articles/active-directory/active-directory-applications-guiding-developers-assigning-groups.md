<properties
    pageTitle="Azure AD και εφαρμογές: Αντιστοίχιση ομάδων σε μια εφαρμογή | Microsoft Azure"
    description="Πώς μπορείτε να υλοποιήσετε ανάθεση σε ομάδα για τις εφαρμογές του Azure."
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="inhenk"/>

# <a name="azure-ad-and-applications-assigning-groups-to-an-application"></a>Azure AD και εφαρμογές: Αντιστοίχιση ομάδων σε μια εφαρμογή
Πριν μπορέσετε να αντιστοιχίσετε χρήστες και ομάδες σε μια εφαρμογή, πρέπει να απαιτούν εκχώρηση χρήστη. Για να μάθετε πώς μπορείτε να απαιτούν εκχώρηση χρήστη, ανατρέξτε στο άρθρο [Που απαιτεί εκχώρηση χρήστη](active-directory-applications-guiding-developers-requiring-user-assignment.md) .

Σε αυτό το άρθρο προϋποθέτει ότι έχετε δημιουργήσει ήδη ομάδες στην υπηρεσία καταλόγου active directory που χρησιμοποιείτε για αυτήν την εφαρμογή.

## <a name="assigning-groups-to-an-application"></a>Αντιστοίχιση ομάδων σε μια εφαρμογή
1. Συνδεθείτε στην πύλη του Azure με ένα λογαριασμό διαχειριστή.
2. Κάντε κλικ στο στοιχείο **Όλα τα στοιχεία** από το κύριο μενού.
3. Επιλέξτε τον κατάλογο που χρησιμοποιείτε για την εφαρμογή.
4. Κάντε κλικ στην καρτέλα **ΕΦΑΡΜΟΓΈΣ** .
5. Επιλέξτε την εφαρμογή από τη λίστα με τις εφαρμογές που σχετίζονται με αυτόν τον κατάλογο.
6. Κάντε κλικ στην καρτέλα **ΧΡΉΣΤΕΣ και ΟΜΆΔΕΣ** .
7. Φιλτράρετε τη λίστα των ομάδων στην υπηρεσία καταλόγου active directory, χρησιμοποιώντας την αναπτυσσόμενη λίστα " **ομάδες** ".
8. Επιλέξτε την ομάδα.
9. Κάντε κλικ στο κουμπί **ΑΝΤΙΣΤΟΊΧΙΣΗ**.
10. Κάντε κλικ στην επιλογή **Ναι** όταν σας ζητηθεί.

## <a name="next-steps"></a>Επόμενα βήματα
[AZURE.INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]

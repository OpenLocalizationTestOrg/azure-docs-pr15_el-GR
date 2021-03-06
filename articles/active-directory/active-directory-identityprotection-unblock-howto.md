<properties
    pageTitle="Azure Active Directory προστασία ταυτότητας - πώς μπορείτε να καταργήσετε τον αποκλεισμό χρήστες | Microsoft Azure"
    description="Μάθετε πώς να καταργήσετε τον αποκλεισμό τους χρήστες που έχουν αποκλειστεί από μια πολιτική Azure Active Directory ταυτότητας προστασίας."
    services="active-directory"
    keywords="προστασία ταυτότητας καταλόγου Azure active directory, κατάργηση του αποκλεισμού χρήστη"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory προστασία ταυτότητας - πώς μπορείτε να καταργήσετε τον αποκλεισμό χρηστών

Με το Azure Active Directory ταυτότητας προστασία με, μπορείτε να ρυθμίσετε πολιτικές για να αποκλείσετε χρήστες εάν πληρούνται οι όροι έχει ρυθμιστεί. Συνήθως, μια αποκλεισμένων χρήστη επαφές υπηρεσία Βοήθειας για να γίνετε Αναίρεση αποκλεισμού. Το θέμα αυτό εξηγεί τα βήματα που μπορείτε να εκτελέσετε για να καταργήσετε τον αποκλεισμό αποκλεισμένων χρήστη.


## <a name="determine-the-reason-for-blocking"></a>Προσδιορίστε το λόγο για τον αποκλεισμό

Ως πρώτο βήμα για να καταργήσετε τον αποκλεισμό ενός χρήστη, πρέπει να προσδιορίσετε τον τύπο της πολιτικής που έχει αποκλείσει το χρήστη, επειδή τα επόμενα βήματά σας εξαρτώνται από αυτή. Με το Azure Active Directory ταυτότητας προστασία χρήστη μπορούν να αποκλειστούν είτε από μια πολιτική εισόδου κινδύνου ή μια πολιτική κινδύνου χρήστη. 

Μπορείτε να λάβετε τον τύπο της πολιτικής που έχει αποκλείσει χρήστη από την επικεφαλίδα στο παράθυρο διαλόγου που παρουσιάστηκε στο χρήστη κατά την προσπάθεια εισόδου:

|Πολιτική | Παράθυρο διαλόγου χρήστη|
|--- | --- |
|Είσοδος κινδύνου | ![Αποκλεισμένα εισόδου](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Κινδύνου χρήστη | ![Αποκλεισμένα λογαριασμού](./media/active-directory-identityprotection-unblock-howto/104.png) |


Ένας χρήστης που έχει αποκλειστεί από:

- Μια πολιτική εισόδου κινδύνου είναι επίσης γνωστή ως ύποπτο εισόδου
- Μια πολιτική κινδύνου χρήστη είναι επίσης γνωστή ως ένα λογαριασμό στο κινδύνου

 
## <a name="unblocking-suspicious-sign-ins"></a>Κατάργηση αποκλεισμού ύποπτες πρόσθετα εισόδου

Για να καταργήσετε τον αποκλεισμό μια ύποπτη εισόδου, έχετε τις εξής επιλογές:

1. **Είσοδος από ένα οικείο θέση ή τη συσκευή** - κοινές λόγος για αποκλεισμένων ύποπτες πρόσθετα εισόδου είναι εισόδου προσπάθειες από άγνωστα θέσεις ή συσκευές. Οι χρήστες σας γρήγορα να προσδιορίσετε εάν αυτό είναι το λόγο για τον αποκλεισμό κατά την προσπάθεια εισόδου από ένα οικείο θέση ή τη συσκευή.


3. **Εξαίρεση από την πολιτική** - Εάν πιστεύετε ότι η τρέχουσα ρύθμιση παραμέτρων της πολιτικής εισόδου προκαλεί θέματα για συγκεκριμένους χρήστες, μπορείτε να εξαιρέσετε τους χρήστες από αυτό. Ανατρέξτε στο θέμα [Είσοδος κινδύνου πολιτικής](active-directory-identityprotection.md#sign-in-risk-policy) για περισσότερες λεπτομέρειες.
 
4. **Απενεργοποίηση πολιτικής** - Εάν πιστεύετε ότι η ρύθμιση παραμέτρων της πολιτικής σας προκαλεί προβλήματα για όλους τους χρήστες σας, μπορείτε να απενεργοποιήσετε την πολιτική. Ανατρέξτε στο θέμα [Είσοδος κινδύνου πολιτικής](active-directory-identityprotection.md#sign-in-risk-policy) για περισσότερες λεπτομέρειες.


## <a name="unblocking-accounts-at-risk"></a>Κατάργηση αποκλεισμού λογαριασμούς σε κίνδυνο

Για να καταργήσετε τον αποκλεισμό ενός λογαριασμού σε κίνδυνο, έχετε τις εξής επιλογές:

1. **Επαναφορά κωδικού πρόσβασης** - μπορείτε να επαναφέρετε τον κωδικό πρόσβασης του χρήστη. Ανατρέξτε στο θέμα [Επαναφορά του κωδικού πρόσβασης μη αυτόματη ασφαλή](active-directory-identityprotection.md#manual-secure-password-reset) για περισσότερες λεπτομέρειες.

2. **Ματαίωση όλων των εκδηλώσεων κινδύνου** - πολιτικής κινδύνου χρήστη αποκλείει χρήστη εάν συμπληρώθηκε το επίπεδο κινδύνου χρήστης που έχει οριστεί για τον αποκλεισμό πρόσβασης. Μπορείτε να μειώσετε χρήστη του επιπέδου κινδύνου με μη αυτόματο τρόπο κλείνοντας αναφερθεί συμβάντα κινδύνου. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Κλείσιμο συμβάντα κινδύνου με μη αυτόματο τρόπο](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Εξαίρεση από την πολιτική** - Εάν πιστεύετε ότι η τρέχουσα ρύθμιση παραμέτρων της πολιτικής εισόδου προκαλεί θέματα για συγκεκριμένους χρήστες, μπορείτε να εξαιρέσετε τους χρήστες από αυτό. Ανατρέξτε στο θέμα [πολιτική κινδύνου χρήστη](active-directory-identityprotection.md#user-risk-policy) για περισσότερες λεπτομέρειες.
 
4. **Απενεργοποίηση πολιτικής** - Εάν πιστεύετε ότι η ρύθμιση παραμέτρων της πολιτικής σας προκαλεί προβλήματα για όλους τους χρήστες σας, μπορείτε να απενεργοποιήσετε την πολιτική. Ανατρέξτε στο θέμα [πολιτική κινδύνου χρήστη](active-directory-identityprotection.md#user-risk-policy) για περισσότερες λεπτομέρειες.




## <a name="next-steps"></a>Επόμενα βήματα

 Θέλετε να μάθετε περισσότερα σχετικά με την προστασία Azure AD ταυτότητας; Ανατρέξτε στο θέμα [Προστασία Azure Active Directory ταυτότητας](active-directory-identityprotection.md).
 


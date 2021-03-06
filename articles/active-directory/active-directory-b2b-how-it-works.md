<properties
   pageTitle="Προεπισκόπηση συνεργασίας Azure AD B2B: πώς λειτουργεί | Microsoft Azure"
   description="Περιγράφει τον τρόπο Azure Active Directory B2B συνεργασίας υποστηρίζει τις σχέσεις μεταξύ εταιρεία σας, ενεργοποιώντας συνεργάτες επιλεκτικής πρόσβασης εταιρικών εφαρμογών σας"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: πώς λειτουργεί
Συνεργασίας Azure AD B2B βασίζεται σε μια πρόσκληση και να εξαργυρώσετε μοντέλο. Μπορείτε να παρέχετε τις διευθύνσεις ηλεκτρονικού ταχυδρομείου από τα μέρη που θέλετε να εργαστείτε, μαζί με τις εφαρμογές που θέλετε να χρησιμοποιήσετε. Azure AD τους αποστέλλεται μια πρόσκληση ηλεκτρονικού ταχυδρομείου με μια σύνδεση. Ο χρήστης συνεργάτη ακολουθεί τη σύνδεση και είναι σας ζητηθεί να εισέλθετε με τη χρήση τους λογαριασμός Azure AD ή εισόδου για χρήση με ένα νέο Azure AD λογαριασμού.

1. Ο διαχειριστής σας ζητά από τους χρήστες συνεργάτη με την αποστολή [ενός αρχείου .csv δομημένα](active-directory-b2b-references-csv-file-format.md) με την πύλη Azure.
2. Η πύλη στέλνει πρόσκληση μηνύματα ηλεκτρονικού ταχυδρομείου σε αυτούς τους χρήστες του συνεργάτη.
3. Οι χρήστες συνεργάτη κάντε κλικ στη σύνδεση στο μήνυμα ηλεκτρονικού ταχυδρομείου και θα σας ζητηθεί να συνδεθείτε χρησιμοποιώντας τα διαπιστευτήριά τους εργασίας (εάν βρίσκονται ήδη στο Azure AD) ή να εγγραφείτε ως χρήστης συνεργασίας Azure AD B2B.
4. Οι χρήστες συνεργάτη ανακατευθύνονται στην εφαρμογή που τους έχουν προσκληθεί, όπου έχουν άμεση πρόσβαση.

## <a name="directory-operations"></a>Λειτουργίες καταλόγου
Χρήστες του συνεργάτη που υπάρχουν στο σας Azure AD με εξωτερικούς χρήστες. Αυτό σημαίνει ότι ο διαχειριστής σας μπορεί να παρέχετε άδειες χρήσης, να εκχωρήσετε την ιδιότητα μέλους ομάδας, και περαιτέρω εκχώρηση πρόσβασης σε εταιρικές εφαρμογές από το Azure πύλη ή με χρήση του PowerShell Azure ακριβώς όπως για τους χρήστες στην εταιρεία σας.

Ενώ μια επί πληρωμή Azure AD συνδρομής (Basic ή Premium) δεν είναι απαραίτητο να χρησιμοποιήσετε Azure AD B2B, οι μισθωτές που έχουν μια επί πληρωμή συνδρομή Azure AD (Basic ή Premium) λαμβάνουν τα ακόλουθα πρόσθετα πλεονεκτήματα:

 - Διαχειριστές μπορούν να εκχωρήσουν ομάδες εφαρμογές, παρέχοντας για τη διαχείριση της access ο προσκεκλημένος χρήστης απλοποίησης.
 - Διαχειριστής μισθωτή εμπορική προσαρμογή χρησιμοποιείται για την προσθήκη εμπορικής επωνυμίας τα μηνύματα ηλεκτρονικού ταχυδρομείου πρόσκλησης και εμπειρία εξαγοράς, παρέχοντας περισσότερες περιβάλλοντος για να προσκαλέσει χρήστες συνεργάτη.

## <a name="related-articles"></a>Σχετικά άρθρα
 Περιήγηση σε μας άλλα άρθρα Azure AD B2B συνεργασία

 - [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
 - [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
 - [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
 - [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
 - [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

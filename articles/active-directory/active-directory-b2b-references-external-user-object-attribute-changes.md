<properties
   pageTitle="Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών για προεπισκόπηση συνεργασίας Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B υποστηρίζει τις σχέσεις μεταξύ εταιρεία σας, ενεργοποιώντας συνεργάτες επιλεκτικής πρόσβασης εταιρικών εφαρμογών σας"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών

Κάθε χρήστη σε έναν κατάλογο Azure AD αντιπροσωπεύεται από ένα αντικείμενο χρήστη. Το αντικείμενο χρήστη στην Azure AD υφίσταται χαρακτηριστικό αλλαγές στα διάφορα στάδια από τη συνεργασία B2B πρόσκληση-εξαργύρωση ροής. Το αντικείμενο χρήστη που αντιπροσωπεύει το χρήστη συνεργάτη στον κατάλογο έχει χαρακτηριστικά που αλλάζουν στην εξαργύρωση ώρα, όταν ο χρήστης συνεργάτη κάνει κλικ στη σύνδεση στο μήνυμα ηλεκτρονικού ταχυδρομείου πρόσκλησης. Συγκεκριμένα:

- Τα χαρακτηριστικά **SignInName** και **AltSecId** συμπληρώνονται
- Το χαρακτηριστικό **DisplayName** αλλαγών από το κύριο όνομα χρήστη (user_fabrikam.com#EXT#@contoso.com) για το όνομα εισόδου(user@fabrikam.com)

Παρακολούθηση αυτά τα χαρακτηριστικά στο Azure AD μπορεί να σας βοηθήσει να αντιμετωπίσετε χρήστη συνεργάτη έχει εξαργυρώσατε τους πρόσκληση συνεργασίας B2B ή όχι.

## <a name="related-articles"></a>Σχετικά άρθρα
Αναζήτηση μας άλλα άρθρα στο Azure AD B2B συνεργασίας:

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
- [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
- [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
- [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

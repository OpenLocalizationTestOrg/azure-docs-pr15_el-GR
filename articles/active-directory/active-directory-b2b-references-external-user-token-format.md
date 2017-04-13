<properties
   pageTitle="Μορφή διακριτικού εξωτερικών χρηστών για προεπισκόπηση συνεργασίας Azure Active Directory B2B | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: μορφοποίηση διακριτικού εξωτερικών χρηστών

Απαιτήσεις για μια τυπική Azure AD διακριτικό περιγράφεται στο άρθρο [τύποι διεκδίκηση και υποστηρίζονται διακριτικού](active-directory-token-and-claims.md) στην azure.microsoft.com.

Οι δηλώσεις που είναι διαφορετικές για έλεγχο ταυτότητας B2B συνεργασίας εξωτερικό χρήστη είναι οι εξής:<br/>
- **Αναγνωριστικό Αντικειμένου:** το Αναγνωριστικό αντικειμένου από το μισθωτή του πόρου<br/>
- **TID**: μισθωτή Αναγνωριστικό από το μισθωτή του πόρου<br/>
- **Εκδότης**: Αυτός είναι ο μισθωτής πόρων<br/>
- **IDP**: Αυτή είναι η αρχική μισθωτή του χρήστη<br/>
- **AltSecId**: αυτό είναι το Αναγνωριστικό εναλλακτικές ασφαλείας, το οποίο είναι αδιαφανές για εσάς<br/>

## <a name="related-articles"></a>Σχετικά άρθρα
Αναζήτηση μας άλλα άρθρα στο Azure AD B2B συνεργασίας:

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
- [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
- [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

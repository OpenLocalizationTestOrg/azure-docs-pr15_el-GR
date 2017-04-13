<properties
   pageTitle="Τρέχουσα περιορισμοί προεπισκόπηση συνεργασίας του Azure Active Directory B2B | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Προεπισκόπηση συνεργασίας Azure AD B2B: τρέχοντα προεπισκόπηση περιορισμοί

- Έλεγχος ταυτότητας πολλών παραγόντων (MFA) δεν υποστηρίζεται σε εξωτερικούς χρήστες. Για παράδειγμα, εάν Contoso έχει MFA, αλλά οργανισμού συνεργάτη δεν, στη συνέχεια, οι χρήστες του οργανισμού συνεργάτη δεν μπορεί να εκχωρηθεί MFA μέσω συνεργασίας B2B.
- Οι προσκλήσεις είναι διαθέσιμες μόνο μέσω CSV; μεμονωμένες προσκλήσεων και των API access δεν υποστηρίζονται.
- Μόνο Azure AD καθολικοί διαχειριστές μπορούν να αποστείλετε αρχεία .csv.
- Προσκλήσεις για καταναλωτές διευθύνσεις ηλεκτρονικού ταχυδρομείου (όπως το hotmail.com, Gmail.com ή comcast.net) προς το παρόν, δεν υποστηρίζονται.
- Πρόσβαση εξωτερικών χρηστών στην εσωτερική εγκατάσταση εφαρμογές που δεν έχει ελεγχθεί.
- Εξωτερικοί χρήστες δεν αυτόματα διαγράφονται κατά τον πραγματικό χρήστη διαγράφεται και από τους καταλόγου.
- Προσκλήσεις για λίστες διανομής δεν υποστηρίζονται.
- Έως 2.000 εγγραφών μπορούν να αποσταλούν μέσω CSV.

## <a name="related-articles"></a>Σχετικά άρθρα
Αναζήτηση μας άλλα άρθρα στο Azure AD B2B συνεργασίας:

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
- [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
- [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
- [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

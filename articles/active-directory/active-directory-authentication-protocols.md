<properties
   pageTitle="Πρωτόκολλα ελέγχου ταυτότητας καταλόγου Azure Active Directory | Microsoft Azure"
   description="Επισκόπηση τα πρωτόκολλα ελέγχου ταυτότητας που υποστηρίζονται από Azure Active Directory (AD)"
   documentationCenter="dev-center-name"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-authentication-protocols"></a>Πρωτόκολλα ελέγχου ταυτότητας καταλόγου Azure Active Directory

Azure Active Directory (Azure AD) υποστηρίζει πολλές τα χρησιμοποιούνται ευρύτερα πρωτόκολλα ελέγχου ταυτότητας και εξουσιοδότησης. Τα θέματα σε αυτήν την ενότητα περιγράφουν τα υποστηριζόμενα πρωτόκολλα και την εφαρμογή τους στο Azure AD. Τα θέματα περιλαμβάνεται μια αναθεώρηση υποστηριζόμενη διεκδίκηση τύπων, μια εισαγωγή σχετικά με τη χρήση μετα-δεδομένων ομοσπονδίας, λεπτομερή διακριτικό 2.0. και τεκμηρίωση αναφοράς πρωτόκολλο SAML 2.0 και μια ενότητα αντιμετώπισης προβλημάτων.

## <a name="authentication-protocols-articles-and-reference"></a>Έλεγχος ταυτότητας πρωτόκολλα άρθρα και αναφορά

- [Σημαντικές πληροφορίες σχετικά με την είσοδο κλειδί κατάδειξης στο Azure AD](active-directory-signing-key-rollover.md) – μάθετε σχετικά με την είσοδο Azure AD cadence αναστροφής κλειδιού, οι αλλαγές που μπορείτε να κάνετε για να ενημερώσετε αυτόματα τον αριθμό-κλειδί και συζήτησης για το πώς μπορείτε να ενημερώσετε τα πιο συνηθισμένα σενάρια εφαρμογής.


- [Τύποι διεκδίκηση και υποστηρίζονται διακριτικού](active-directory-token-and-claims.md) - μάθετε περισσότερα σχετικά με το αξιώσεων στο τα διακριτικά αυτό το ζήτημα Azure AD.


- [Ομοσπονδία μετα-δεδομένων](https://msdn.microsoft.com/library/azure/dn195592.aspx) - μάθετε πώς μπορείτε να βρείτε και ερμηνεία των εγγράφων μετα-δεδομένων που δημιουργεί Azure AD.


- [2.0 διακριτικό στο Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) - μάθετε περισσότερα σχετικά με την εφαρμογή της 2.0 διακριτικό στο Azure AD.


- [OpenID σύνδεση 1.0](https://msdn.microsoft.com/library/azure/dn645541.aspx) - μάθετε πώς να χρησιμοποιείτε το διακριτικό 2.0, ένα πρωτόκολλο εξουσιοδότησης, για τον έλεγχο ταυτότητας.


- [Αναφορά πρωτόκολλο SAML](https://msdn.microsoft.com/library/azure/dn195591.aspx) - μάθετε περισσότερα σχετικά με τα προφίλ Καθολικής σύνδεσης και μία SAML Sign-out του Azure AD.


- [WS-Ομοσπονδία 1.2](https://msdn.microsoft.com/library/azure/dn903702.aspx) - μάθετε σχετικά με τα WS-Ομοσπονδία 1.2 Azure AD.


- [Αντιμετώπιση προβλημάτων πρωτόκολλα ελέγχου ταυτότητας](https://msdn.microsoft.com/library/azure/dn195584.aspx) - μάθετε πώς μπορείτε να αποφύγετε τα προβλήματα και ερμηνεία και επίλυση σφαλμάτων κατά τη χρήση του Azure AD.



## <a name="see-also"></a>Δείτε επίσης

[Οδηγός καταλόγου Azure Active Directory για προγραμματιστές](active-directory-developers-guide.md)

[Χρήση Azure AD για έλεγχο ταυτότητας](../app-service-web/web-sites-authentication-authorization.md)

[Δείγματα κώδικα της υπηρεσίας καταλόγου Active Directory](active-directory-code-samples.md)

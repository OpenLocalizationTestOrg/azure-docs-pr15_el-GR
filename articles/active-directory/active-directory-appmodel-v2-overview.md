<properties
    pageTitle="Επισκόπηση τελικού σημείου v2.0 | Microsoft Azure"
    description="Εισαγωγή στη δημιουργία εφαρμογών με το λογαριασμό Microsoft και Azure Active Directory εισόδου."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Είσοδος στο λογαριασμό Microsoft & Azure AD χρήστες σε μια μεμονωμένη εφαρμογή

Στο παρελθόν, μια προγραμματιστή της εφαρμογής που θέλετε για την υποστήριξη λογαριασμούς Microsoft και Azure Active Directory έχει απαιτείται για την ενοποίηση με συστήματα δύο ξεχωριστά.  Θα σας τώρα έχετε εισαχθεί μια νέα έκδοση API του ελέγχου ταυτότητας που σας επιτρέπει να εισέλθετε χρήστες με δύο τύπους λογαριασμών με χρήση του συστήματος Azure AD.  Αυτό το σύστημα συγκλίνει ελέγχου ταυτότητας είναι γνωστό ως **το τελικό σημείο v2.0**.  Με το τελικό σημείο v2.0, μία απλή ενσωμάτωση σάς επιτρέπει να φτάσει ένα ακροατήριο που εκτείνεται σε εκατομμύρια χρήστες με προσωπικές και εταιρικό/σχολικό λογαριασμούς.

Εφαρμογές που χρησιμοποιούν το τελικό σημείο v2.0 επίσης να εκμετάλλευση APIs ΥΠΌΛΟΙΠΑ από το [Microsoft Graph](https://graph.microsoft.io) και το [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) χρησιμοποιώντας οποιονδήποτε τύπο λογαριασμού.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Γρήγορα αποτελέσματα
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Επιλέξτε την πλατφόρμα αγαπημένες σας από την παρακάτω λίστα για να δημιουργήσετε μια εφαρμογή με το άνοιγμα αρχείου προέλευσης βιβλιοθήκες & πλαισίων.  Εναλλακτικά, μπορείτε να χρησιμοποιήσετε την τεκμηρίωση πρωτόκολλο OAuth 2.0 & OpenID σύνδεση για αποστολή και παραλαβή μηνυμάτων πρωτόκολλο απευθείας χωρίς τη χρήση μιας βιβλιοθήκης auth.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Τι νέο υπάρχει
Οι πληροφορίες εννοιολογική εδώ θα είναι χρήσιμη σε κατανόηση τι είναι και τι δεν μπορείτε να κάνετε με το τελικό σημείο v2.0.

- Μάθετε περισσότερα σχετικά με τους [τύπους των εφαρμογών που μπορείτε να δημιουργήσετε με το τελικό σημείο v2.0](active-directory-v2-flows.md).
- Κατανόηση του [περιορισμούς, περιορισμούς, και τους περιορισμούς](active-directory-v2-limitations.md) με το τελικό σημείο v2.0.
- Προσθέσαμε πρόσφατα υποστήριξη [διαχείρισης περιορισμένων εύρους](active-directory-v2-scopes.md) και το [OAuth2 προγράμματος-πελάτη εκχώρηση διαπιστευτήρια](active-directory-v2-protocols-oauth-client-creds.md).  Δοκιμάστε τα!

## <a name="reference"></a>Αναφορά
Αυτές οι συνδέσεις θα είναι χρήσιμες για την Εξερεύνηση της πλατφόρμας σε βάθος:

- Δόμηση 2016: [Γρήγορα αποτελέσματα με το Microsoft ταυτότητες: μεγάλες επιχειρήσεις βαθμού είσοδο για τις εφαρμογές του](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Λάβετε βοήθεια σχετικά με υπερχείλισης στοίβας, με χρήση του [καταλόγου azure active](http://stackoverflow.com/questions/tagged/azure-active-directory) ή [adal](http://stackoverflow.com/questions/tagged/adal) ετικέτες.
- [Αναφορά πρωτόκολλο v2.0](active-directory-v2-protocols.md)
- [Αναφορά διακριτικού v2.0](active-directory-v2-tokens.md)
- [Αναφορά βιβλιοθήκη v2.0](active-directory-v2-libraries.md)
- [Εύρος και συγκατάθεση το τελικό σημείο v2.0](active-directory-v2-scopes.md)
- [Πύλη του Microsoft εφαρμογή εγγραφής](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API αναφοράς](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Το Microsoft Graph](https://graph.microsoft.io)
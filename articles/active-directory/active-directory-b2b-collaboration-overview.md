<properties
   pageTitle="Συνεργασίας Azure Active Directory B2B | Microsoft Azure"
   description="Συνεργασίας Azure Active Directory B2B επιτρέπει συνεργάτες για πρόσβαση στο εταιρικό εφαρμογές σας, με κάθε έναν από τους χρήστες που αντιπροσωπεύονται από μια μεμονωμένη Azure AD λογαριασμού"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B συνεργασίας

Συνεργασίας Azure Active Directory (Azure AD) B2B σάς επιτρέπει να ενεργοποιήσετε την πρόσβαση στις εφαρμογές του εταιρικού από συνεργάτη διαχειριζόμενες ταυτότητες. Μπορείτε να δημιουργήσετε σχέσεις μεταξύ εταιρείας, πρόσκληση και δικαιώματα χρήστη εταιρειών συνεργάτη για να αποκτήσετε πρόσβαση τους πόρους σας. Πολυπλοκότητα μειώνεται, επειδή κάθε εταιρεία federates μία φορά με το Azure Active Directory και κάθε χρήστης αντιπροσωπεύεται από ένα μόνο λογαριασμό Azure AD. Ασφάλεια αυξάνεται εάν συνεργάτη σας διαχειρίζεστε τους λογαριασμούς στο Azure AD, επειδή η access έχει ανακληθεί όταν χρήστες συνεργάτη τερματίζονται από τους εταιρείες και ακούσια access μέσω συμμετοχής σε εσωτερικές σε καταλόγους δεν επιτρέπεται. Για συνεργάτες που δεν έχουν ήδη Azure AD, B2B συνεργασίας έχει ένα βελτιωμένο εμπειρία εγγραφής για την παροχή λογαριασμοί Azure AD στους συνεργάτες σας.

-   Συνεργάτη σας, χρησιμοποιήστε τις δικές τους τα διαπιστευτήρια εισόδου, που σας απαλλάσσει από τη διαχείριση ενός καταλόγου εξωτερικών συνεργατών και από την ανάγκη για να καταργήσετε την πρόσβαση όταν οι χρήστες αποχώρηση από την εταιρεία του συνεργάτη.

-   Διαχείριση πρόσβασης στις εφαρμογές σας, ανεξάρτητα από τα κύκλου ζωής του συνεργάτη σας λογαριασμό. Αυτό σημαίνει ότι, για παράδειγμα, ότι μπορείτε να ανακαλέσετε την πρόσβαση χωρίς να χρειάζεται να ζητήσετε από το τμήμα IT του συνεργάτη σας για να κάνετε τίποτα.

## <a name="capabilities"></a>Δυνατότητες

Συνεργασίας B2B απλοποιεί διαχείρισης και βελτιώνει την ασφάλεια πρόσβασης συνεργάτη σε εταιρικούς πόρους, όπως ΑΔΑ εφαρμογές, όπως Office 365, Salesforce, υπηρεσίες Azure και κάθε mobile, στο cloud και εφαρμογή αξιώσεων υπόψη εσωτερικής εγκατάστασης. Συνεργάτες παρέχει τη δυνατότητα συνεργασίας B2B διαχειριστείτε τις δικές τους λογαριασμούς και επιχειρήσεις να εφαρμόσετε πολιτικές ασφάλειας σε πρόσβαση συνεργάτη.

Azure Active Directory B2B συνεργασίας είναι εύκολο να ρυθμίσετε τις παραμέτρους με απλοποιημένη εγγραφής για συνεργάτες κάθε μεγέθους ακόμα και αν δεν διαθέτουν το δικό τους Azure Active Directory μέσω μιας διαδικασίας επαληθευτεί ηλεκτρονικού ταχυδρομείου. Είναι επίσης εύκολο για να διατηρήσετε με χωρίς εξωτερικών καταλόγων ή ανά ρυθμίσεις παραμέτρων ομοσπονδίας συνεργάτη.

## <a name="b2b-collaboration-process"></a>Διαδικασία B2B συνεργασίας

1. Συνεργασίας Azure AD B2B επιτρέπει διαχειριστής εταιρείας για να προσκαλέσετε και εγκρίνετε ένα σύνολο των εξωτερικών χρηστών με την αποστολή ενός αρχείου τιμών διαχωρισμένων με κόμμα (CSV) των γραμμών που δεν περισσότερους από 2000 πύλη συνεργασίας του B2B.

  ![Παράθυρο διαλόγου Αποστολή αρχείου CSV](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Η πύλη θα αποστολή προσκλήσεων μέσω ηλεκτρονικού ταχυδρομείου σε αυτούς τους εξωτερικούς χρήστες.

3. Ο προσκεκλημένος χρήστης θα είτε είσοδος σε έναν υπάρχοντα λογαριασμό εργασίας με τη Microsoft (διαχείρισης από το Azure AD) ή λάβετε ένα νέο λογαριασμό εργασίας στο Azure AD.

4. Αφού εισέλθετε, ο χρήστης θα ανακατευθύνονται στην εφαρμογή που έχει θέσει σε κοινή χρήση με αυτά.

Προσκλήσεις για καταναλωτές διευθύνσεις ηλεκτρονικού ταχυδρομείου (για παράδειγμα, Gmail ή [*comcast.net*](http://comcast.net/)) δεν υποστηρίζονται αυτήν τη στιγμή.

Για περισσότερες πληροφορίες σχετικά με τον τρόπο λειτουργίας συνεργασίας B2B, δείτε [αυτό το βίντεο](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Επόμενα βήματα
Αναζητήστε τα άλλα άρθρα Azure AD B2B συνεργασία.

- [Τι είναι το Azure AD B2B συνεργασίας;](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Πώς λειτουργεί](active-directory-b2b-how-it-works.md)
- [Λεπτομερή ανάλυση](active-directory-b2b-detailed-walkthrough.md)
- [Αναφορά μορφή αρχείου CSV](active-directory-b2b-references-csv-file-format.md)
- [Μορφή διακριτικού εξωτερικών χρηστών](active-directory-b2b-references-external-user-token-format.md)
- [Αλλαγές χαρακτηριστικό αντικειμένου εξωτερικών χρηστών](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Τρέχουσα περιορισμοί preview](active-directory-b2b-current-preview-limitations.md)
- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

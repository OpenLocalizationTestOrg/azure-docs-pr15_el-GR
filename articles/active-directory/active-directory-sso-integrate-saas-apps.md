<properties
    pageTitle="Ενοποίηση Azure Active Directory καθολικής σύνδεσης με εφαρμογές ΑΔΑ |  Microsoft Azure"
    description="Ενεργοποίηση καθολική σύνδεση τον έλεγχο ταυτότητας και χρήστη προμήθεια κεντρική πρόσβαση διαχείρισης εφαρμογών ΑΔΑ στο Azure Active Directory. Μάθετε πώς να ενοποιήσετε το Azure Active Directory ΑΔΑ εφαρμογές."
    services="active-directory"
      keywords="ενοποίηση του Azure AD με εφαρμογές ΑΠΑ"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Ενοποίηση Azure Active Directory καθολικής σύνδεσης με εφαρμογές ΑΠΑ  

> [AZURE.SELECTOR]
- [Πύλη του Azure](active-directory-enterprise-apps-manage-sso.md)
- [Azure κλασική πύλη](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Για να ξεκινήσετε τη ρύθμιση του καθολικής σύνδεσης για μια εφαρμογή που μεταφέρετε στην εταιρεία σας, θα χρησιμοποιήσετε έναν υπάρχοντα κατάλογο στο Azure Active Directory (Azure AD). Μπορείτε να χρησιμοποιήσετε έναν κατάλογο Azure AD που λαμβάνετε μέσω του Microsoft Azure, Office 365 ή το Windows Intune. Εάν έχετε δύο ή περισσότερα από αυτά, ανατρέξτε στο θέμα [Διαχείριση του καταλόγου Azure AD σας](active-directory-administer.md) για να αποφασίσετε ποια να χρησιμοποιήσετε.

## <a name="authentication"></a>Έλεγχος ταυτότητας

Για εφαρμογές που υποστηρίζουν το SAML 2.0, WS-ομοσπονδίας, ή σύνδεση OpenID πρωτόκολλα Azure Active Directory χρησιμοποιεί πιστοποιητικά για να δημιουργήσετε σχέσεις αξιοπιστίας με υπογραφή. Για περισσότερες πληροφορίες σχετικά με αυτό, ανατρέξτε στο θέμα [Διαχείριση πιστοποιητικών για εξωτερική καθολικής σύνδεσης](active-directory-sso-certs.md).

Για τις εφαρμογές που υποστηρίζουν μόνο HTML βασίζεται σε φόρμες εισόδου, Azure Active Directory, χρησιμοποιεί "διαφύλαξης κωδικού πρόσβασης" για να δημιουργήσετε σχέσεις αξιοπιστίας. Αυτή η δυνατότητα επιτρέπει στους χρήστες στην εταιρεία σας να συνδεθείτε αυτόματα μια εφαρμογή ΑΠΑ από Azure AD με χρήση των πληροφοριών λογαριασμού χρήστη από την εφαρμογή ΑΔΑ. Azure AD συλλέγει και με ασφάλεια αποθηκεύει τις πληροφορίες λογαριασμού χρήστη και τον σχετικό κωδικό πρόσβασης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [με κωδικό πρόσβασης καθολικής σύνδεσης](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Εξουσιοδότηση

Ένας λογαριασμός προμήθεια του φακέλου επιτρέπει στο χρήστη να επιτρέπεται να χρησιμοποιήσετε μια εφαρμογή, αφού έχετε έλεγχο ταυτότητας μέσω καθολικής σύνδεσης. Προμήθεια του χρήστη μπορεί να γίνει με μη αυτόματο τρόπο ή, σε ορισμένες περιπτώσεις, μπορείτε να προσθέσετε και να καταργήσετε πληροφορίες χρήστη από την εφαρμογή ΑΔΑ με βάση τις αλλαγές που κάνατε στο Azure Active Directory. Για περισσότερες πληροφορίες σχετικά με τη χρήση υπάρχουσας σύνδεσης Azure AD για παροχή αυτοματοποιημένη, ανατρέξτε στο θέμα [χρήστη αυτόματης προμήθεια και καταργήστε την προμήθεια για εφαρμογές ΑΔΑ](active-directory-saas-app-provisioning.md).

Διαφορετικά, να με μη αυτόματο τρόπο προσθήκη πληροφορίες χρήστη σε μια εφαρμογή, είτε να χρησιμοποιήσετε άλλες λύσεις παροχής που είναι διαθέσιμες στην αγορά.

## <a name="access"></a>Πρόσβαση

Azure AD παρέχει διάφορους τρόπους προσαρμόσιμες για την ανάπτυξη εφαρμογών για τελικούς χρήστες στον οργανισμό σας. Που δεν είναι κλειδωμένα σε οποιαδήποτε συγκεκριμένη ανάπτυξη ή access λύση. Μπορείτε να χρησιμοποιήσετε [τη λύση που ταιριάζει καλύτερα στις ανάγκες σας](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Επιπλέον ζητήματα για εφαρμογές ήδη σε χρήση

Ρύθμιση στην καθολική σύνδεση για μια εφαρμογή που χρησιμοποιεί ήδη η εταιρεία σας είναι μια διαδικασία διαφορετική από τη διαδικασία για τη δημιουργία νέων λογαριασμών για μια νέα εφαρμογή. Υπάρχουν μερικά προκαταρκτικού βήματα, συμπεριλαμβανομένων των: αντιστοίχιση ταυτότητες χρήστη της εφαρμογής για να τις ταυτότητες Azure AD και κατανόηση πώς οι χρήστες θα είναι η εμπειρία καταγραφή στο σε μια εφαρμογή, αφού είναι ενσωματωμένο.

> [AZURE.NOTE] Για να ρυθμίσετε SSO για μια υπάρχουσα εφαρμογή, πρέπει να έχετε δικαιώματα καθολικού διαχειριστή σε δύο Azure AD και την εφαρμογή ΑΔΑ.

### <a name="mapping-user-accounts"></a>Αντιστοίχιση λογαριασμών χρηστών

Ταυτότητα χρήστη συνήθως έχει ένα μοναδικό αναγνωριστικό που θα μπορούσε να είναι μια διεύθυνση ηλεκτρονικού ταχυδρομείου ή το κύριο όνομα χρήστη (UPN). Θα χρειαστεί να σύνδεσης (χάρτη) ταυτότητα της εφαρμογής του κάθε χρήστη για τις αντίστοιχες Azure AD ταυτότητα. Υπάρχουν δύο τρόποι για να γίνει αυτό, ανάλογα με τον τρόπο την απαίτηση σας εφαρμογής ελέγχου ταυτότητας.

Για περισσότερες πληροφορίες σχετικά με την αντιστοίχιση ταυτότητες εφαρμογής με Azure AD ταυτότητες, ανατρέξτε στο θέμα [Προσαρμογή αξιώσεων εκδοθεί στο διακριτικό SAML](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) και [Προσαρμογή αντιστοιχίσεις χαρακτηριστικών για παροχή](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Κατανόηση της καταγραφής του χρήστη στην εμπειρία

Όταν ενσωματώνετε SSO για μια εφαρμογή που χρησιμοποιείται ήδη, είναι σημαντικό να γνωρίζετε ότι θα επηρεαστεί η εμπειρία χρήστη. Για όλες τις εφαρμογές, οι χρήστες θα αρχίσετε να χρησιμοποιείτε τα διαπιστευτήριά τους Azure AD για να πραγματοποιήσετε είσοδο. Μπορεί επίσης να είναι ότι πρέπει να χρησιμοποιήσουν μια διαφορετική πύλη για να αποκτήσετε πρόσβαση τις εφαρμογές.

Μπορεί να γίνει SSO για ορισμένες εφαρμογές στη εισόδου της εφαρμογής στο περιβάλλον εργασίας, αλλά για άλλες εφαρμογές, ο χρήστης θα πρέπει να ακολουθήσετε μια κεντρική πύλη όπως ([Οι εφαρμογές μου](http://myapps.microsoft.com) ή το [Office365](http://portal.office.com/myapps)) για να πραγματοποιήσετε είσοδο. Μάθετε περισσότερα σχετικά με τους διαφορετικούς τύπους SSO και τους εμπειρίες χρήστη σε [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory](active-directory-appssoaccess-whatis.md).

Μια άλλη χρήσιμος πόρος είναι *συγκατάθεση Suppressing χρήστη* στο άρθρο [Guiding προγραμματιστές](active-directory-applications-guiding-developers-for-lob-applications.md) .

## <a name="next-steps"></a>Επόμενα βήματα


Για εφαρμογές ΑΔΑ που μπορείτε να βρείτε στη συλλογή εφαρμογών, Azure AD παρέχει πολλά [προγράμματα εκμάθησης σχετικά με τον τρόπο για να ενσωματώσετε ΑΔΑ εφαρμογές](active-directory-saas-tutorial-list.md).

Εάν η εφαρμογή δεν υπάρχει στη συλλογή εφαρμογών, μπορείτε να [το προσθέσετε στη συλλογή Azure AD εφαρμογή ως μια προσαρμοσμένη εφαρμογή](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Υπάρχει πολύ περισσότερες λεπτομέρειες σχετικά με όλα αυτά τα ζητήματα στη βιβλιοθήκη Azure.com, ξεκινώντας με [Τι είναι η εφαρμογή access και καθολικής σύνδεσης με το Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Δείτε επίσης

- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)

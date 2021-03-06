<properties
    pageTitle="Κοινή χρήση λογαριασμούς που χρησιμοποιούν το Azure AD |  Microsoft Azure"
    description="Περιγράφει τον τρόπο Azure Active Directory επιτρέπει εταιρείες για την ασφαλή κοινή χρήση λογαριασμών για εφαρμογές εσωτερικής εγκατάστασης και τις υπηρεσίες cloud καταναλωτών."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Κοινή χρήση τους λογαριασμούς με Azure AD

## <a name="overview"></a>Επισκόπηση
Μερικές φορές οι οργανισμοί πρέπει να χρησιμοποιήσετε ένα μοναδικό όνομα χρήστη και τον κωδικό πρόσβασης για πολλά άτομα. Αυτό συμβαίνει συνήθως σε δύο περιπτώσεις:

- Κατά την πρόσβαση σε εφαρμογές που απαιτούν ένα μοναδικό σύνδεσης και τον κωδικό πρόσβασης για κάθε χρήστη, εάν εσωτερικής εγκατάστασης εφαρμογών ή καταναλωτή στο cloud services (π.χ. εταιρικό μέσα κοινωνικής δικτύωσης λογαριασμοί).
- Κατά τη δημιουργία περιβάλλοντα πολλών χρηστών. Ενδέχεται να έχετε ένα λογαριασμό μόνο, τοπικό που έχει αυξημένα δικαιώματα και μπορεί να χρησιμοποιηθεί για τη βασική διαμόρφωση, διαχείριση και αποκατάσταση δραστηριότητες (π.χ., το λογαριασμό τοπικό "Καθολικός διαχειριστής" για το Office 365) ή ο ριζικός λογαριασμός στο Salesforce.

Στο παρελθόν, θα είναι κοινόχρηστη αυτούς τους λογαριασμούς με τη διανομή τα διαπιστευτήρια (όνομα χρήστη/κωδικός πρόσβασης) για να το σωστό άτομα ή την αποθήκευσή τους σε μια κοινόχρηστη θέση όπου πολλαπλοί αξιόπιστων παράγοντες να αποκτήσετε πρόσβαση σε αυτά.

Το παραδοσιακό μοντέλο κοινής χρήσης περιλαμβάνει πολλές μειονεκτήματα:

- Ενεργοποίηση της πρόσβασης σε νέες εφαρμογές απαιτεί να διανείμετε διαπιστευτήρια για όλους τους χρήστες που χρειάζονται πρόσβαση.
- Κάθε κοινόχρηστη εφαρμογή μπορεί να απαιτεί το δικό της σύνολο μοναδικό κοινόχρηστο διαπιστευτήρια, απαιτώντας από τους χρήστες να θυμάστε πολλά σύνολα διαπιστευτήρια. Όταν οι χρήστες έχουν να θυμάστε πολλά διαπιστευτήρια, αυξάνει τον κίνδυνο ότι θα γίνεται επαναφορά επικίνδυνη πρακτικές. (π.χ. γράφετε προς τα κάτω τους κωδικούς πρόσβασης).
- Δεν είναι δυνατό να γνωρίζετε ποιος έχει πρόσβαση σε μια εφαρμογή του.
- Δεν είναι δυνατό να γνωρίζετε ποιος έχει *πρόσβαση σε* μια εφαρμογή του.
- Όταν πρέπει να καταργήσετε την πρόσβαση σε μια εφαρμογή, πρέπει να ενημερώσετε τα διαπιστευτήρια και να τα διανείμετε εκ νέου για όλους τους χρήστες που χρειάζονται πρόσβαση σε αυτήν την εφαρμογή.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory λογαριασμό κοινής χρήσης

Azure AD παρέχει μια νέα προσέγγιση για χρήση κοινόχρηστου λογαριασμών που αποκλείει αυτά τα μειονεκτήματα.

Ο διαχειριστής Azure AD ρυθμίζει ποιες εφαρμογές να αποκτήσετε πρόσβαση σε ένα χρήστη, χρησιμοποιώντας τον πίνακα της Access και επιλέξτε τον τύπο του καθολική σύνδεση καλύτερα κατάλληλος για αυτήν την εφαρμογή. Έναν από αυτούς τους τύπους, *με βάση τον κωδικό πρόσβασης καθολικής σύνδεσης στο*, σας επιτρέπει να Azure AD λειτουργεί ως μια είδους "broker" κατά τη διαδικασία εισόδου για αυτήν την εφαρμογή.

Οι χρήστες συνδεθούν σε μία φορά με τον εταιρικό λογαριασμό. Πρόκειται για τον ίδιο λογαριασμό που χρησιμοποιείτε τακτικά για να αποκτήσετε πρόσβαση σε υπολογιστή ή του ηλεκτρονικού ταχυδρομείου τους. Μπορούν να ανακαλύψετε και να έχετε πρόσβαση μόνο αυτές τις εφαρμογές που τους έχουν ανατεθεί. Με κοινόχρηστα λογαριασμούς, αυτή η λίστα με τις εφαρμογές του μπορεί να περιλαμβάνει οποιονδήποτε αριθμό κοινόχρηστο διαπιστευτήρια. Ο τελικός χρήστης δεν χρειάζεται να θυμάστε ή καταγράψτε τους διάφορους λογαριασμούς που ενδεχομένως να χρησιμοποιούν.

Κοινόχρηστο λογαριασμούς όχι μόνο αύξηση εποπτεία, καθώς και τη βελτίωση χρηστικότητας, τους επίσης να βελτιώσουν την ασφάλεια. Οι χρήστες με δικαιώματα για να χρησιμοποιήσετε τα διαπιστευτήρια δεν βλέπετε τον κοινόχρηστο κωδικό πρόσβασης, αλλά προτιμάτε να λάβω δικαιώματα για να χρησιμοποιήσετε τον κωδικό πρόσβασης ως μέρος μιας ροής orchestrated ελέγχου ταυτότητας. Περαιτέρω, με ορισμένες εφαρμογές SSO τον κωδικό πρόσβασης, έχετε την επιλογή να έχουν Azure AD περιοδικά κατάδειξης (ενημέρωση) τον κωδικό πρόσβασης χρησιμοποιώντας μεγάλη και πολύπλοκη τους κωδικούς πρόσβασης, αυξάνοντας την ασφάλεια λογαριασμού. Ο διαχειριστής μπορεί να εύκολα εκχωρήσετε ή να ανακαλέσετε την πρόσβαση σε μια εφαρμογή του και επίσης να γνωρίζετε ποιος έχει πρόσβαση στο λογαριασμό και το ποιος ανοίξει στο παρελθόν.

Azure AD υποστηρίζει κοινόχρηστο λογαριασμούς για κάθε Enterprise φορητότητα οικογένεια προγραμμάτων (EMS), Premium ή βασικές χρήστες με άδεια χρήσης, σε όλους τους τύπους του κωδικού πρόσβασης μόνο είσοδος σε εφαρμογές. Μπορείτε να κάνετε κοινή χρήση τους λογαριασμούς για τους χιλιάδες προ-ενσωματωμένες εφαρμογές στη συλλογή εφαρμογών και να προσθέσετε τη δική σας εφαρμογή τον έλεγχο ταυτότητας κωδικού πρόσβασης με [προσαρμοσμένες εφαρμογές SSO](active-directory-sso-integrate-saas-apps.md).

Azure AD δυνατότητες οι οποίες ενεργοποιήσετε την κοινή χρήση λογαριασμού περιλαμβάνουν τα εξής:

- [Κωδικός πρόσβασης καθολικής σύνδεσης](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Τον κωδικό πρόσβασης του καθολική σύνδεση παράγοντα
- [Ανάθεση σε ομάδα](active-directory-accessmanagement-self-service-group-management.md)
- Εφαρμογές του προσαρμοσμένου κωδικού πρόσβασης
- [Πίνακας εργαλείων/αναφορές χρήσης εφαρμογής](active-directory-passwords-get-insights.md)
- Πύλες πρόσβασης τελικού χρήστη
- [Εφαρμογή διακομιστή μεσολάβησης](active-directory-application-proxy-get-started.md)
- [Αγορά του καταλόγου Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Κοινή χρήση ενός λογαριασμού
Για να χρησιμοποιήσετε Azure AD για την κοινή χρήση ενός λογαριασμού θα πρέπει να:

- Προσθέστε ένα εφαρμογής [Συλλογή εφαρμογών](https://azure.microsoft.com/marketplace/active-directory/) ή μια [προσαρμοσμένη εφαρμογή](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Ρύθμιση παραμέτρων της εφαρμογής για τον κωδικό πρόσβασης μόνο καθολικής σύνδεσης (SSO)
- Χρήση [ομάδας βάσει ανάθεσης](active-directory-accessmanagement-group-saasapps.md) και ενεργοποιήστε την επιλογή για να εισαγάγετε ένα κοινόχρηστο διαπιστευτηρίων
- Προαιρετικό: σε ορισμένες εφαρμογές, όπως το Facebook, στο Twitter ή LinkedIn, μπορείτε να ενεργοποιήσετε την επιλογή για την [αναδιάταξη Azure AD αυτοματοποιημένη κωδικού πρόσβασης](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Μπορείτε επίσης να κάνετε κοινόχρηστο λογαριασμό σας πιο ασφαλή με έλεγχο ταυτότητας πολλών παραγόντων (MFA) (μάθετε περισσότερα σχετικά με την [ασφάλεια των εφαρμογών με το Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) και μπορείτε να αναθέσετε τη δυνατότητα να διαχειρίζεστε ποιος έχει πρόσβαση στην εφαρμογή με [Λειτουργία χρήστη Azure AD](active-directory-accessmanagement-self-service-group-management.md) ομάδα διαχείρισης.

## <a name="related-articles"></a>Σχετικά άρθρα

- [Το άρθρο ευρετηρίου για Διαχείριση εφαρμογών υπηρεσίας καταλόγου Azure Active Directory](active-directory-apps-index.md)
- [Προστασία εφαρμογών με πρόσβαση υπό όρους](active-directory-conditional-access.md)
- [Διαχείριση ομάδας από το χρήστη/SSAA](active-directory-accessmanagement-self-service-group-management.md)

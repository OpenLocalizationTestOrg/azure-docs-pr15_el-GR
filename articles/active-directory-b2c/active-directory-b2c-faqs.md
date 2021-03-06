<properties
    pageTitle="B2C καταλόγου Azure Active Directory: Συνήθεις ερωτήσεις | Microsoft Azure"
    description="Συνήθεις ερωτήσεις σχετικά με το Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>B2C καταλόγου Azure Active Directory: συνήθεις ερωτήσεις

Αυτή η σελίδα παρέχει απαντήσεις σε συνήθεις ερωτήσεις σχετικά με την B2C Azure Active Directory (Azure AD). Διατηρήστε τον έλεγχο ξανά για ενημερώσεις.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Μπορώ να χρησιμοποιήσω Azure AD B2C δυνατότητες στο μισθωτή μου Azure AD υπαρχόντων, βάσει υπαλλήλων;

Αυτήν τη στιγμή Azure AD B2C δυνατότητες δεν είναι δυνατό να ενεργοποιηθεί στο μισθωτή του υπάρχοντος Azure AD. Συνιστάται να δημιουργήσετε ένα νέο μισθωτή για να χρησιμοποιήσετε Azure AD B2C δυνατότητες για τη Διαχείριση οι χρήστες του.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Μπορώ να χρησιμοποιήσω Azure AD B2C για την παροχή κοινωνικής login (Facebook και Google +) στο Office 365;

Azure AD B2C δεν μπορούν να χρησιμοποιηθούν με το Microsoft Office 365. Σε γενικές γραμμές, δεν μπορεί να χρησιμοποιηθεί για την παροχή ελέγχου ταυτότητας για τις εφαρμογές ΑΔΑ (Office 365, Salesforce, εργάσιμης ημέρας, κ.λπ.). Παρέχει διαχείριση ταυτότητας και την πρόσβαση μόνο για καταναλωτές-τοποθεσία web και εφαρμογές για κινητές συσκευές, και δεν ισχύει για σενάρια υπαλλήλου ή συνεργατών.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Τι είναι οι τοπικοί λογαριασμοί στο Azure AD B2C; Πώς είναι διαφορετικό από το εταιρικό ή σχολικό λογαριασμούς στο Azure AD;

Σε ένα μισθωτή του Azure AD, κάθε χρήστης στο μισθωτή (εκτός από τους χρήστες με υπαρχόντων λογαριασμών Microsoft) συνδέεται με μια διεύθυνση ηλεκτρονικού ταχυδρομείου της φόρμας `<xyz>@<tenant domain>`, όπου `<tenant domain>` είναι μόνο ένας από τους τομείς επαληθευτεί στο μισθωτή ή τα αρχικά `<...>.onmicrosoft.com` τομέα. Αυτός ο τύπος λογαριασμού είναι ένα εταιρικό ή σχολικό λογαριασμό.

Σε ένα μισθωτή του Azure AD B2C, οι περισσότερες εφαρμογές θέλετε ο χρήστης να συνδεθείτε με οποιαδήποτε διεύθυνση ηλεκτρονικού ταχυδρομείου αυθαίρετο (για παράδειγμα, joe@comcast.net, bob@gmail.com, sarah@contoso.com, ή jim@live.com). Αυτός ο τύπος του λογαριασμού είναι ένα τοπικό λογαριασμό. Σήμερα, επίσης υποστηρίζουμε ονόματα χρηστών αυθαίρετο (μόνο απλό συμβολοσειρές) ως Τοπικοί λογαριασμοί (για παράδειγμα, Αλέξανδρος, ο Μπάμπης, Χριστίνα ή jim). Μπορείτε να επιλέξετε μία από αυτές τις δύο τύπους λογαριασμού τοπικού στην υπηρεσία Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Ποιες υπηρεσίες παροχής ταυτότητας κοινωνικών που υποστηρίζουν τώρα; Ποιες σκοπεύετε να υποστηρίζει στο μέλλον;

Αυτήν τη στιγμή υποστηρίζουμε Facebook, Google +, το LinkedIn και Amazon. Θα προσθέσουμε υποστήριξη για άλλες υπηρεσίες παροχής δημοφιλείς κοινωνικών ταυτότητας βάσει απαιτήσεων πελατών.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Να η ρύθμιση παραμέτρων του εύρους για τη συγκέντρωση περισσότερες πληροφορίες σχετικά με τους καταναλωτές από διάφορες υπηρεσίες παροχής ταυτότητας κοινωνικών;

Όχι, αλλά αυτή η δυνατότητα είναι ενεργοποιημένη μας χάρτης. Τα πεδία προεπιλεγμένη χρησιμοποιούνται για μας υποστηριζόμενες σύνολο από υπηρεσίες παροχής ταυτότητας κοινωνικών είναι οι εξής:

- Facebook: μήνυμα ηλεκτρονικού ταχυδρομείου
- Google +: μήνυμα ηλεκτρονικού ταχυδρομείου
- Λογαριασμός Microsoft: openid προφίλ ηλεκτρονικού ταχυδρομείου
- Amazon: προφίλ
- LinkedIn: r_emailaddress, r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Η εφαρμογή μου πρέπει να εκτελεστεί σε Azure για την εργασία με Azure AD B2C;

Όχι, μπορείτε να φιλοξενήσετε την εφαρμογή σας σε οποιοδήποτε σημείο (στο cloud ή εσωτερικής εγκατάστασης). Το μόνο που χρειάζεται για να αλληλεπιδράσετε με Azure AD B2C είναι τη δυνατότητα να στέλνετε και να λαμβάνετε προσκλήσεις HTTP σε δημόσια προσβάσιμη τελικά σημεία.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Να έχετε πολλούς μισθωτές του Azure AD B2C. Πώς μπορώ να διαχειριστώ τους στην πύλη του Azure;

Κάθε μισθωτή Azure AD B2C διαθέτει δικό του blade δυνατότητες B2C στην πύλη του Azure. Ανατρέξτε στο θέμα [Azure AD B2C: καταχώρηση εφαρμογής σας](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) για να μάθετε πώς μπορείτε να μεταβείτε σε ένα συγκεκριμένο μισθωτή B2C δυνατότητες blade στην πύλη του Azure. Εναλλαγή μεταξύ Azure AD B2C καταλόγους στην πύλη του Azure δεν θα διατηρήσει τις δυνατότητες B2C blade ανοίξει σε περισσότερα προγράμματα περιήγησης.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Πώς μπορώ να προσαρμόσω επαλήθευσης μηνύματα ηλεκτρονικού ταχυδρομείου (το περιεχόμενο και την "από:" το πεδίο) αποστέλλονται από Azure AD B2C;

Μπορείτε να χρησιμοποιήσετε τη [δυνατότητα εμπορικής προσαρμογής εταιρείας](../active-directory/active-directory-add-company-branding.md) για να προσαρμόσετε το περιεχόμενο των μηνυμάτων ηλεκτρονικού ταχυδρομείου επαλήθευσης. Συγκεκριμένα, μπορείτε να προσαρμόσετε αυτά τα δύο στοιχεία από το μήνυμα ηλεκτρονικού ταχυδρομείου:

- **Πανό λογότυπο**: εμφανίζεται στο κάτω δεξιό τμήμα.
- **Χρώμα φόντου**: εμφανίζεται στο επάνω μέρος.

    ![Στιγμιότυπο οθόνης από ένα μήνυμα ηλεκτρονικού ταχυδρομείου προσαρμοσμένων επαλήθευσης](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Η υπογραφή ηλεκτρονικού ταχυδρομείου περιέχει το όνομα του μισθωτή B2C που παρείχατε κατά τη δημιουργία πρώτα στο μισθωτή B2C. Μπορείτε να αλλάξετε το όνομα, χρησιμοποιώντας τις παρακάτω οδηγίες:

- Πραγματοποιήστε είσοδο στο [Azure κλασική πύλη](https://manage.windowsazure.com/) ως ο διαχειριστής της συνδρομής.
- Περιήγηση στο μισθωτή σας B2C.
- Κάντε κλικ στην καρτέλα **Ρύθμιση παραμέτρων** .
- Αλλάξτε το πεδίο **όνομα** στην ενότητα **Ιδιότητες καταλόγου** .
- Κάντε κλικ στην επιλογή " **Αποθήκευση** " στο κάτω μέρος της σελίδας.

Προς το παρόν δεν υπάρχει τρόπος για να αλλάξετε το "από:" πεδίο σε μήνυμα ηλεκτρονικού ταχυδρομείου. Εάν σας ενδιαφέρει σε αυτήν τη δυνατότητα και σε πλήρως Προσαρμογή στο κυρίως σώμα του μηνύματος ηλεκτρονικού ταχυδρομείου επαλήθευσης, ψηφίσετε για τη δυνατότητα στην [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails).

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Πώς μπορώ να μετεγκαταστήσω μου υπάρχοντα ονόματα χρηστών, κωδικούς πρόσβασης και προφίλ από τη βάση δεδομένων για να Azure AD B2C;

Μπορείτε να χρησιμοποιήσετε το API Azure AD γράφημα για να γράψετε το εργαλείο μετεγκατάστασης. Ανατρέξτε στο θέμα το [δείγμα Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md) για λεπτομέρειες. Θα παράσχουμε διάφορες επιλογές μετεγκατάστασης και εργαλεία εκτός του-οι έτοιμες στο μέλλον.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Ποια πολιτική κωδικών πρόσβασης χρησιμοποιείται για τοπικούς λογαριασμούς στο Azure AD B2C;

Η πολιτική κωδικών πρόσβασης Azure AD B2C για τοπικούς λογαριασμούς είναι με βάση την πολιτική για Azure AD. Azure AD B2C της εγγραφής, εγγραφής ή εισόδου και τον κωδικό πρόσβασης επαναφορά πολιτικές χρησιμοποιεί την ισχύ "ισχυρό" κωδικό πρόσβασης και δεν λήγει οποιαδήποτε τους κωδικούς πρόσβασης. Διαβάστε την [πολιτική κωδικών πρόσβασης Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) για περισσότερες λεπτομέρειες.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Μπορώ να χρησιμοποιήσω Azure AD Connect για τη μετεγκατάσταση ταυτότητες καταναλωτή που είναι αποθηκευμένα σε μου στην εσωτερική εγκατάσταση υπηρεσίας καταλόγου Active Directory για να Azure AD B2C;

Όχι, Azure AD Connect δεν έχει σχεδιαστεί για λειτουργία με το Azure AD B2C. Θα παράσχουμε διάφορες επιλογές μετεγκατάστασης και εργαλεία εκτός του-οι έτοιμες στο μέλλον.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Λειτουργεί Azure AD B2C με τα συστήματα όπως το Microsoft Dynamics CRM;

Δεν αυτήν τη στιγμή. Ενοποίηση αυτά τα συστήματα βρίσκεται στη μας χάρτης.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Η Azure AD B2C λειτουργούν με το SharePoint εσωτερικής 2016 ή προηγούμενες εκδόσεις;

Δεν αυτήν τη στιγμή. Azure AD B2C δεν διαθέτει υποστήριξη για τα διακριτικά SAML 1.1 που πύλες και τις εφαρμογές ηλεκτρονικού εμπορίου ενσωματωμένος στην ανάγκη εσωτερικής εγκατάστασης του SharePoint. Σημειώστε ότι Azure AD B2C δεν προορίζεται για το SharePoint εξωτερικής κοινής χρήσης συνεργάτη σενάριο; ανατρέξτε στο θέμα [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) αντί για αυτό.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Θα πρέπει να χρησιμοποιήσω Azure AD B2C ή B2B για τη Διαχείριση εξωτερικών ταυτότητες;

Διαβάστε αυτό το άρθρο σχετικά με τις [εξωτερικές ταυτότητες](../active-directory/active-directory-b2b-compare-external-identities.md) για να μάθετε περισσότερα σχετικά με την εφαρμογή τις κατάλληλες δυνατότητες για να τα σενάρια εξωτερικών την ταυτότητά σας.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Τι αναφοράς και έλεγχος δυνατότητες παρέχει Azure AD B2C; Είναι τα ίδια με αυτά Azure AD Premium;

Όχι, Azure AD B2C δεν υποστηρίζει το ίδιο σύνολο αναφορών με Azure AD Premium. Azure AD B2C θα αφήσετε βασικές αναφοράς και έλεγχος APIs σύντομα.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Να μπορεί να μεταφραστεί το περιβάλλον εργασίας Χρήστη των σελίδων που σερβιρίστηκε από Azure AD B2C; Ποιες γλώσσες υποστηρίζονται;

Προς το παρόν, Azure AD B2C είναι βελτιστοποιημένη για Αγγλικά μόνο. Στόχος μας είναι να αναπτύξετε δυνατότητες μετάφρασης το συντομότερο δυνατό.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Μπορώ να χρησιμοποιήσω το δικό μου διευθύνσεις URL στις μου σελίδες εγγραφής και εισόδου που είναι λειτουργούσαν από Azure AD B2C; Για παράδειγμα, μπορώ να αλλάξω τη διεύθυνση URL από login.microsoftonline.com να login.contoso.com;

Δεν αυτήν τη στιγμή. Αυτή η δυνατότητα είναι σε μας χάρτης. Επίσης, σημειώστε ότι επαλήθευση του τομέα σας, στην καρτέλα **Domains** του μισθωτή σας στην πύλη του Azure κλασική θα το κάνετε αυτό.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Πώς μπορώ να διαγράψω μισθωτή Azure AD B2C μου;

Ακολουθήστε τα παρακάτω βήματα για να διαγράψετε το Azure AD B2C μισθωτή:

- Ακολουθήστε τα παρακάτω βήματα για να [μεταβείτε σε το blade δυνατότητες B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) στην πύλη του Azure.
- Μεταβείτε στο τις **εφαρμογές**, τις **υπηρεσίες παροχής ταυτότητας** και **όλες τις πολιτικές** λεπίδες και διαγράψτε όλες τις καταχωρήσεις από κάθε μία από αυτές.
- Πραγματοποιήστε είσοδο στην [πύλη Azure κλασική](https://manage.windowsazure.com/) ως ο διαχειριστής της συνδρομής. (Αυτή είναι η ίδια εργασία ή σχολικό λογαριασμό ή τον ίδιο λογαριασμό Microsoft που χρησιμοποιήσατε για να εγγραφείτε για Azure.)
- Μεταβείτε στο θέμα επέκταση της υπηρεσίας καταλόγου Active Directory στα αριστερά και κάντε κλικ στο μισθωτή σας B2C.
- Κάντε κλικ στην καρτέλα **χρήστες** .
- Επιλέξτε κάθε χρήστη στην ενεργοποίηση (εξαιρέσετε στο χρήστη που έχετε εισέλθει ως, π.χ., η συνδρομή διαχειριστής). Κάντε κλικ στην επιλογή " **Διαγραφή** " στο κάτω μέρος της σελίδας και κάντε κλικ στην επιλογή **Ναι** όταν σας ζητηθεί.
- Κάντε κλικ στην καρτέλα **εφαρμογές** .
- Επιλέξτε **εφαρμογές που ανήκει η εταιρεία μου** στο πεδίο **Εμφάνιση** αναπτυσσόμενη λίστα και κάντε κλικ στο σημάδι ελέγχου.
- Θα δείτε μια εφαρμογή που ονομάζεται **b2c επεκτάσεις εφαρμογής** που παρατίθενται παρακάτω. Κάντε κλικ στην επιλογή " **Διαγραφή** " στο κάτω μέρος της σελίδας και κάντε κλικ στην επιλογή **Ναι** όταν σας ζητηθεί.
- Μεταβείτε στο θέμα επέκταση της υπηρεσίας καταλόγου Active Directory ξανά και επιλέξτε το μισθωτή B2C.
- Κάντε κλικ στην επιλογή " **Διαγραφή** " στο κάτω μέρος της σελίδας. Ακολουθήστε τις οδηγίες στην οθόνη για να ολοκληρώσετε τη διαδικασία.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Μπορώ να λάβω Azure AD B2C ως μέρος της οικογένειας προγραμμάτων για μεγάλες επιχειρήσεις φορητότητα;

Όχι, Azure AD B2C είναι μια pay-as-you-go Azure υπηρεσίας και δεν είναι μέρος της οικογένειας προγραμμάτων φορητότητας για μεγάλες επιχειρήσεις.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Πώς μπορώ να αναφέρω προβλήματα με Azure AD B2C;

Ανατρέξτε στο θέμα [αρχείο υποστηρίζουν αιτήσεις για Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="more-information"></a>Περισσότερες πληροφορίες

Επίσης μπορεί να θέλετε να δείτε τις τρέχουσες [περιορισμούς υπηρεσίας, περιορισμούς, και τους περιορισμούς](active-directory-b2c-limitations.md).

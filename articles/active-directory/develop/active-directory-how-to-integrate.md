<properties
   pageTitle="Πώς μπορείτε να ενοποιήσετε με το Azure Active Directory | Microsoft Azure"
   description="Οδηγός πλεονεκτήματα και τους πόρους για την ενοποίηση με το Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
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

# <a name="integrating-with-azure-active-directory"></a>Ενοποίηση με το Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory παρέχει εταιρείες με διαχείρισης των ταυτοτήτων επίπεδου μεγάλης εταιρείας για εφαρμογές cloud.  Ενοποίηση του Azure AD παρέχει ένα βελτιωμένο εμπειρία εισόδου τους χρήστες σας και σας βοηθά να συμφωνούν με την πολιτική IT την εφαρμογή σας.

## <a name="how-to-integrate"></a>Πώς μπορείτε να ενοποιήσετε

Υπάρχουν πολλοί τρόποι για την εφαρμογή σας για να ενσωματώσετε με Azure AD.  Εκμεταλλευτείτε όσα ή ως λίγα από αυτά τα σενάρια, όπως είναι κατάλληλη για την εφαρμογή σας.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Υποστήριξη Azure AD ως ένας τρόπος για να συνδεθείτε σε εφαρμογή σας

**Μειώστε είσοδος τριβής και μείωση του κόστους υποστήριξης.** Με τη χρήση Azure AD να πραγματοποιήσουν είσοδο στην εφαρμογή σας, οι χρήστες δεν θα έχουν ένα περισσότερα ονόματα και τον κωδικό πρόσβασης για να θυμάστε.  Προγραμματιστής, θα έχετε ένα μικρότερο κωδικό πρόσβασης για την αποθήκευση και την προστασία.  Δεν χρειάζεται να χειριστείτε επαναφέρει ξεχασμένος κωδικός πρόσβασης μπορεί να είναι μια σημαντική εξοικονόμησης από μόνο του.  Azure AD προσφέρει είσοδος για μερικές από τον κόσμο πιο δημοφιλή cloud εφαρμογές, συμπεριλαμβανομένου του Office 365 και το Windows Azure.  Με εκατοντάδες εκατομμύρια χρήστες από εκατομμύρια εταιρείες, το πιθανότερο είναι χρήστη σας έχει ήδη πραγματοποιήσει είσοδο στο Azure AD.  Μάθετε περισσότερα σχετικά με την [Προσθήκη υποστήριξη Azure AD είσοδος](../active-directory-authentication-scenarios.md).

**Απλοποιήστε εισόδου προς τα επάνω για την εφαρμογή σας.**  Κατά την εγγραφή για την εφαρμογή σας, Azure AD να στείλετε σημαντικές πληροφορίες σχετικά με ένα χρήστη, ώστε να μπορείτε να προ-συμπληρώσετε την εγγραφή φόρμας ή να αποκλείσετε το πλήρως.  Οι χρήστες μπορούν να εγγραφείτε για την εφαρμογή σας χρησιμοποιώντας το λογαριασμό Azure AD μέσω εμπειρία συγκατάθεση γνωρίζει παρόμοια με εκείνα που βρέθηκαν στο μέσα κοινωνικής δικτύωσης και εφαρμογές για κινητές συσκευές.  Οποιοσδήποτε χρήστης μπορεί να εγγραφείτε και να συνδεθείτε σε μια εφαρμογή που είναι συνδεδεμένο με το Azure AD χωρίς να απαιτείται συμμετοχή IT.  Μάθετε περισσότερα σχετικά με την [είσοδο ασφαλείας της εφαρμογής για είσοδο με λογαριασμό Azure AD](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Αναζήτηση για τους χρήστες, Διαχείριση προμήθεια του χρήστη και τον έλεγχο της πρόσβασης στην εφαρμογή σας

**Αναζήτηση για τους χρήστες στον κατάλογο.**  Χρήση του API του γραφήματος για να βοηθήσετε τους χρήστες να πραγματοποιήσετε αναζήτηση και αναζητήστε άλλα άτομα στον οργανισμό τους όταν προσκαλείτε άλλους ή εκχώρηση πρόσβασης, αντί να απαιτείται να πληκτρολογήσετε ηλεκτρονικού ταχυδρομείου διευθύνσεις.  Οι χρήστες μπορούν να μεταβείτε χρησιμοποιώντας γνωστές διεύθυνση βιβλίο στυλ περιβάλλον, συμπεριλαμβανομένης και της προβολής των λεπτομερειών της ιεραρχίας του οργανισμού.  Μάθετε περισσότερα σχετικά με το [API του γραφήματος](../active-directory-graph-api.md).

**Εκ νέου χρήση ομάδων της υπηρεσίας καταλόγου Active Directory και οι λίστες διανομής που ήδη τη διαχείριση των πελατών σας.**  Azure AD περιλαμβάνει τις ομάδες που των πελατών σας ήδη χρησιμοποιείτε για διανομή ηλεκτρονικού ταχυδρομείου και τη διαχείριση της πρόσβασης.  Χρήση του API του Graph, το χρησιμοποιήσετε αυτές τις ομάδες αντί για απαίτηση των πελατών σας για να δημιουργήσετε και να διαχειριστείτε ένα ξεχωριστό σύνολο των ομάδων στην εφαρμογή σας.  Πληροφορίες ομάδας μπορούν να σταλούν επίσης στην εφαρμογή σας στο εισόδου διακριτικά.  Μάθετε περισσότερα σχετικά με το [API του γραφήματος](../active-directory-graph-api.md).

**Χρησιμοποιήστε το Azure AD για να ελέγχετε ποιος έχει πρόσβαση στην εφαρμογή σας.**  Διαχειριστές και τους κατόχους εφαρμογή στο Azure AD να εκχωρήσετε πρόσβαση σε εφαρμογές σε συγκεκριμένους χρήστες και ομάδες.  Χρήση του API του γραφήματος, μπορείτε να διαβάσετε αυτήν τη λίστα και να το χρησιμοποιήσετε για να ελέγξετε προμήθεια και καταργήστε την προμήθεια των πόρων και αποκτήστε πρόσβαση στην εφαρμογή σας.

**Έλεγχος πρόσβασης βάσει χρήση Azure AD για ρόλους.**  Διαχειριστές και τους κατόχους εφαρμογή μπορεί να εκχωρήσει χρήστες και ομάδες για να τους ρόλους που έχετε καθορίσει όταν καταχωρείτε την εφαρμογή σας σε Azure AD.  Πληροφορίες ρόλου αποστέλλεται στην εφαρμογή σας στο εισόδου διακριτικά και μπορείτε επίσης να διαβάσετε με χρήση του API του γραφήματος.  Μάθετε περισσότερα σχετικά με τη [Χρήση Azure AD για άδεια](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Πρόσβαση σε προφίλ χρήστη, ημερολόγιο, μήνυμα ηλεκτρονικού ταχυδρομείου, επαφών, αρχεία και άλλα

**Azure AD είναι ο διακομιστής εξουσιοδότησης για το Office 365 και άλλες υπηρεσίες Microsoft business.**  Εάν υποστηρίζει Azure AD για είσοδο στην εφαρμογή σας ή να υποστήριξη σύνδεση λογαριασμών χρήστη τρέχουσα με λογαριασμούς χρήστη Azure AD που χρησιμοποιούν το διακριτικό 2.0, μπορείτε να ζητήσετε πρόσβαση ανάγνωσης και εγγραφής για προφίλ χρήστη, ημερολόγιο, μήνυμα ηλεκτρονικού ταχυδρομείου, επαφών, αρχεία και άλλες πληροφορίες.  Απρόσκοπτα, μπορείτε να γράψετε συμβάντα στο ημερολόγιο του χρήστη, και να διαβάσετε ή να γράψετε αρχεία για να τους στο OneDrive.  Μάθετε περισσότερα σχετικά με [την πρόσβαση τα API Office 365](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Προωθήστε την εφαρμογή σας στην Azure και στο Office 365 Marketplaces

**Προωθήστε την εφαρμογή με το εκατομμύρια εταιρείες που χρησιμοποιούν ήδη Azure AD.**  Οι χρήστες που αναζήτηση και αναζητήστε αυτές τις marketplaces χρησιμοποιούν ήδη μία ή περισσότερες υπηρεσίες cloud, καθιστώντας τις προσδιορισμένο πελάτες υπηρεσία cloud.  Μάθετε περισσότερα σχετικά με την προώθηση της εφαρμογής σας από [Το Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/).

**Όταν οι χρήστες εγγραφείτε για την εφαρμογή σας, θα εμφανιστεί το Azure AD access πίνακα και εκκίνηση εφαρμογών του Office 365.**  Οι χρήστες θα μπορούν να επιστρέψετε γρήγορα και εύκολα αργότερα, η εφαρμογή σας τη βελτίωση της δέσμευσης χρήστη.  Μάθετε περισσότερα σχετικά με το [Azure AD πρόσβαση στον πίνακα](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Ασφαλής επικοινωνία υπηρεσία συσκευής και υπηρεσία εξυπηρέτησης

**Χρησιμοποιείτε το Azure AD για Διαχείριση ταυτοτήτων υπηρεσίες και τις συσκευές μειώνει τον κώδικα που χρειάζεστε για να γράψετε και επιτρέπει στους επαγγελματίες IT για να διαχειριστείτε την πρόσβαση.**  Υπηρεσίες και οι συσκευές να λάβετε τα διακριτικά από το Azure AD χρησιμοποιώντας διακριτικό και να χρησιμοποιήσετε αυτά τα διακριτικά για πρόσβαση στο web APIs.  Χρήση του Azure AD μπορείτε να αποφύγετε σύνταξη κώδικα σύνθετα τον έλεγχο ταυτότητας.  Επειδή οι ταυτότητες των υπηρεσιών και των συσκευών που είναι αποθηκευμένα σε Azure AD, ΠΛΗΡΟΦΟΡΙΚΉΣ μπορούν να διαχειρίζονται πλήκτρα και ανάκλησης σε ένα σημείο αντί να χρειάζεται να το κάνετε αυτό ξεχωριστά στην εφαρμογή σας.

## <a name="benefits-of-integration"></a>Πλεονεκτήματα της ενοποίησης

Ενοποίηση με το Azure AD συνοδεύεται από πλεονεκτήματα που δεν απαιτούν να γράφετε επιπλέον κώδικα.

### <a name="integration-with-enterprise-identity-management"></a>Ενοποίηση με διαχείρισης των ταυτοτήτων για μεγάλες επιχειρήσεις

**Βοήθεια εφαρμογής σας συμμόρφωση με τις πολιτικές IT.**  Εταιρείες ενοποίηση τους συστημάτων διαχείρισης ταυτοτήτων για μεγάλες επιχειρήσεις με το Azure AD, έτσι ώστε όταν ένα άτομο αποχωρεί από μια εταιρεία, αυτά θα χαθούν αυτόματα πρόσβαση στην εφαρμογή σας χωρίς να ΠΛΗΡΟΦΟΡΙΚΉΣ να χρειάζεται να εκτελέσετε επιπλέον βήματα.  ΠΛΗΡΟΦΟΡΙΚΉΣ μπορούν να διαχειρίζονται την εφαρμογή σας πρόσβασης και καθορίζουν ποιες πολιτικές πρόσβασης απαιτούνται - για έλεγχο ταυτότητας πολλών παραγόντων παράδειγμα - μειώνοντας την ανάγκη για να γράψετε κώδικα για συμμόρφωση με σύνθετα εταιρικών πολιτικών.  Azure AD παρέχει στους διαχειριστές με ένα αρχείο καταγραφής ελέγχου λεπτομερείς του ποιος πραγματοποιήσει είσοδο στην εφαρμογή σας ώστε να μπορούν να παρακολουθείτε τη χρήση.

**Azure AD επεκτείνει υπηρεσίας καταλόγου Active Directory στο cloud, έτσι ώστε να ενοποιήσετε το την εφαρμογή σας με το AD.**  Πολλές εταιρείες όλο τον κόσμο χρήση υπηρεσίας καταλόγου Active Directory ως τους κεφαλαίου εισόδου και το σύστημα διαχείρισης ταυτοτήτων και απαιτούν εφαρμογές τους για να εργαστείτε με AD.  Ενοποίηση με το Azure AD ενσωματώνει την εφαρμογή σας με την υπηρεσία καταλόγου Active Directory.

### <a name="advanced-security-features"></a>Δυνατότητες ασφαλείας για προχωρημένους

**Έλεγχος ταυτότητας πολλών παραγόντων.**  Azure AD παρέχει εγγενούς έλεγχο ταυτότητας πολλών παραγόντων.  Διαχειριστές IT μπορεί να απαιτούν έλεγχο ταυτότητας πολλών παραγόντων για πρόσβαση σε εφαρμογή σας, ώστε να δεν χρειάζεται να κώδικα αυτήν την υποστήριξη στον εαυτό σας.  Μάθετε περισσότερα σχετικά με [Τον έλεγχο ταυτότητας πολλών παραγόντων](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Ύπαρξη είσοδος εντοπισμού.**  Azure AD επεξεργάζεται πρόσθετα περισσότερα από ένα δισεκατομμύριο εισόδου ημέρα, κατά τη χρήση του υπολογιστή εκμάθησης αλγόριθμους για τον εντοπισμό ύποπτες δραστηριότητες και να ειδοποιήσετε τους διαχειριστές IT πιθανών προβλημάτων.  Με την υποστήριξη Azure AD-είσοδος, η εφαρμογή σας λαμβάνει το όφελος των αυτήν την προστασία. Μάθετε περισσότερα σχετικά με την [Προβολή Azure Active Directory πρόσβασης σε αναφορά](../active-directory-view-access-usage-reports.md).

**Υπό όρους πρόσβασης.**  Εκτός από τον έλεγχο ταυτότητας πολλών παραγόντων, οι διαχειριστές μπορούν να απαιτούν συγκεκριμένες συνθήκες να πληρούνται πριν από τους χρήστες να είσοδος στην εφαρμογή σας.  Συνθήκες που μπορεί να οριστεί περιλαμβάνει την περιοχή διευθύνσεων IP των συσκευών-πελατών, συμμετοχής σε καθορισμένες ομάδες και την κατάσταση της συσκευής που χρησιμοποιείται για την πρόσβαση.  Μάθετε περισσότερα σχετικά με την [πρόσβαση υπό όρους Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Εύκολη ανάπτυξη

**Τυπικά πρωτόκολλα κλάδο.**  Η Microsoft δεσμεύεται για την υποστήριξη βιομηχανικά πρότυπα.  Azure AD υποστηρίζει τα πρωτόκολλα ελέγχου ταυτότητας SAML 2.0, 1.0 σύνδεση OpenID, διακριτικό 2.0 και 1.2 WS ομοσπονδίας.  Το API Graph είναι συμβατή 4.0 OData.  Εάν ήδη την εφαρμογή σας υποστηρίζει τα πρωτόκολλα SAML 2.0 ή 1.0 σύνδεση OpenID για εξωτερική είσοδος, προσθέτοντας υποστήριξη για Azure AD μπορεί να είναι απλή.  Μάθετε περισσότερα για [τα πρωτόκολλα ελέγχου ταυτότητας Azure AD υποστηρίζονται](../active-directory-authentication-protocols.md).

**Άνοιγμα αρχείου προέλευσης βιβλιοθήκες.**  Η Microsoft παρέχει βιβλιοθήκες υποστηρίζεται πλήρως Άνοιγμα αρχείου προέλευσης για δημοφιλείς γλώσσες και πλατφόρμες ανάπτυξη ταχύτητας.  Ο κωδικός προέλευσης έχει άδεια χρήσης στην περιοχή Apache 2.0 και που είναι δωρεάν να διαχωρίζονται και να συνεισφέρουν πίσω για τα έργα.  Μάθετε περισσότερα σχετικά με τις [βιβλιοθήκες Azure AD ελέγχου ταυτότητας](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Πληροφορίες παρουσίας σε όλο τον κόσμο και υψηλή διαθεσιμότητα

**Azure AD έχει αναπτυχθεί σε κέντρα δεδομένων όλο τον κόσμο και γίνεται και παρακολουθούνται όλο το 24ωρο.**  Azure AD είναι το σύστημα διαχείρισης ταυτοτήτων για το Microsoft Azure και το Office 365 και έχει αναπτυχθεί σε 28 κέντρα δεδομένων όλο τον κόσμο.  Κατάλογος δεδομένων είναι εγγυημένη να αναπαραχθεί σε τουλάχιστον τρεις κέντρα δεδομένων.  Καθολικός φόρτωσης balancers βεβαιωθείτε στους χρήστες πρόσβαση πλησιέστερη αντίγραφο του Azure AD που περιέχει τα δεδομένα τους και εκ νέου δρομολογεί αυτόματα αιτήσεων σε άλλα κέντρα δεδομένων σε περίπτωση που εντοπιστεί κάποιο πρόβλημα.

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη σύνταξη κώδικα](../active-directory-developers-guide.md#getting-started).

[Σύμβολο χρηστών χρησιμοποιώντας Azure AD](../active-directory-authentication-scenarios.md)
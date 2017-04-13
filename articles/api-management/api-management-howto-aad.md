<properties 
    pageTitle="Πώς μπορείτε να εγκρίνετε χρησιμοποιώντας Azure Active Directory του Azure API διαχείρισης λογαριασμών για προγραμματιστές" 
    description="Μάθετε πώς μπορείτε να εγκρίνετε οι χρήστες που χρησιμοποιούν Azure Active Directory API διαχείρισης." 
    services="api-management" 
    documentationCenter="API Management" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-authorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a>Πώς μπορείτε να εγκρίνετε χρησιμοποιώντας Azure Active Directory του Azure API διαχείρισης λογαριασμών για προγραμματιστές


## <a name="overview"></a>Επισκόπηση
Αυτός ο οδηγός δείχνει πώς μπορείτε να ενεργοποιήσετε την πρόσβαση στην πύλη για προγραμματιστές για όλους τους χρήστες σε μία ή περισσότερες Azure Active καταλόγους. Αυτός ο οδηγός σας δείχνει πώς μπορείτε να διαχειριστείτε ομάδες χρηστών Azure Active Directory με την προσθήκη εξωτερικών ομάδες που περιέχουν τους χρήστες του Azure Active Directory.

>Για να ολοκληρώσετε τα βήματα σε αυτόν τον οδηγό πρέπει πρώτα να έχετε ένα Azure Active Directory στο οποίο θέλετε να δημιουργήσετε μια εφαρμογή.

## <a name="how-to-authorize-developer-accounts-using-azure-active-directory"></a>Πώς μπορείτε να εγκρίνετε λογαριασμούς για προγραμματιστές που χρησιμοποιούν το Azure Active Directory

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη του Publisher][api-management-management-console]

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

Κάντε κλικ στην επιλογή " **ασφάλεια** " από το μενού **API διαχείρισης** στην αριστερή πλευρά και κάντε κλικ στην **Εξωτερική ταυτότητες**.

![Εξωτερική ταυτότητες][api-management-security-external-identities]

Κάντε κλικ στην επιλογή **καταλόγου Azure Active Directory**. Κάντε μια σημείωση για τη **Διεύθυνση URL ανακατεύθυνσης** και αλλάξετε πάνω από το Azure Active Directory στην πύλη κλασική Azure.

![Εξωτερική ταυτότητες][api-management-security-aad-new]

Κάντε κλικ στο κουμπί **Προσθήκη** για να δημιουργήσετε μια νέα εφαρμογή Azure Active Directory και επιλέξτε **Προσθήκη εφαρμογής ανάπτυξη την εταιρεία μου**.

![Προσθήκη νέας εφαρμογής Azure Active Directory][api-management-new-aad-application-menu]

Πληκτρολογήστε ένα όνομα για την εφαρμογή, επιλέξτε **την εφαρμογή Web ή/και το API Web**και κάντε κλικ στο κουμπί Επόμενο.

![Νέα εφαρμογή Azure Active Directory][api-management-new-aad-application-1]

Για **είσοδο στη διεύθυνση URL**, πληκτρολογήστε τη διεύθυνση σύνδεσης URL της πύλης για προγραμματιστές. Σε αυτό το παράδειγμα, το **σύμβολο στη διεύθυνση URL** είναι `https://aad03.portal.current.int-azure-api.net/signin`. 

Για τη διεύθυνση **URL Αναγνωριστικό εφαρμογής**, πληκτρολογήστε τον προεπιλεγμένο τομέα ή ένα προσαρμοσμένο τομέα για το Azure Active Directory και προσάρτηση μια μοναδική συμβολοσειρά σε αυτήν. Σε αυτό το παράδειγμα τον προεπιλεγμένο τομέα της **https://contoso5api.onmicrosoft.com** χρησιμοποιείται με το επίθημα του **/api** που καθορίζεται.

![Νέες ιδιότητες εφαρμογής Azure Active Directory][api-management-new-aad-application-2]

Κάντε κλικ στο κουμπί ελέγχου για να αποθηκεύσετε και να δημιουργήσετε τη νέα εφαρμογή και μεταβείτε στην καρτέλα **Ρύθμιση παραμέτρων** για τη ρύθμιση παραμέτρων της νέας εφαρμογής.

![Νέα εφαρμογή Azure Active Directory που δημιουργήσατε][api-management-new-aad-app-created]

Εάν πολλών Azure Active καταλόγων πρόκειται να χρησιμοποιηθεί για αυτήν την εφαρμογή, κάντε κλικ στο κουμπί **Ναι** για **εφαρμογή είναι πολλών μισθωτή**. Η προεπιλογή είναι " **όχι**".

![Η εφαρμογή είναι πολλών μισθωτή][api-management-aad-app-multi-tenant]

Αντιγράψτε τη **Διεύθυνση URL ανακατεύθυνσης** από την ενότητα **Azure Active Directory** της καρτέλας **Εξωτερικών ταυτότητες** στην πύλη του publisher και επικολλήστε το στο πλαίσιο κειμένου **Διεύθυνση URL απάντηση** . 

![Διεύθυνση URL απάντηση][api-management-aad-reply-url]

Κάντε κύλιση προς τα κάτω στην καρτέλα "Ρύθμιση παραμέτρων", επιλέξτε **Δικαιώματα εφαρμογής** αναπτυσσόμενο μενού και επιλέξτε **Ανάγνωση καταλόγου δεδομένων**.

![Δικαιώματα εφαρμογής][api-management-aad-app-permissions]

Επιλέξτε **Δικαιώματα πληρεξουσίου** αναπτυσσόμενο μενού και επιλέξτε **Ενεργοποίηση της καθολικής σύνδεσης και διαβάστε προφίλ χρηστών**.

![Ανάθεση δικαιωμάτων][api-management-aad-delegated-permissions]

>Για περισσότερες πληροφορίες σχετικά με την εφαρμογή και την ανάθεση δικαιωμάτων, ανατρέξτε στο θέμα [πρόσβαση σε το API του γραφήματος][].

Αντιγράψτε το **Αναγνωριστικό υπολογιστή-πελάτη** στο Πρόχειρο.

![Αναγνωριστικό υπολογιστή-πελάτη][api-management-aad-app-client-id]

Μεταβείτε ξανά στην πύλη του publisher και επικολλήστε το **Αναγνωριστικό υπολογιστή-πελάτη** που αντιγράψατε από τη ρύθμιση παραμέτρων εφαρμογής Azure Active Directory.

![Αναγνωριστικό υπολογιστή-πελάτη][api-management-client-id]

Επιστρέψτε στο τη ρύθμιση παραμέτρων της υπηρεσίας καταλόγου Azure Active Directory, και κάντε κλικ στην επιλογή το **Επιλέξτε διάρκεια** μετακινηθείτε προς τα κάτω στην ενότητα **πλήκτρα** και καθορίστε ένα χρονικό διάστημα. Σε αυτό το παράδειγμα χρησιμοποιείται **ενός έτους** .

![Πλήκτρο][api-management-aad-key-before-save]

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τη ρύθμιση παραμέτρων και να εμφανίσετε τον αριθμό-κλειδί. Αντιγράψτε τον αριθμό-κλειδί στο Πρόχειρο.

>Σημειώστε αυτού του κλειδιού. Αφού κλείσετε το παράθυρο Ρύθμιση παραμέτρων Azure Active Directory, τον αριθμό-κλειδί δεν μπορεί να εμφανιστεί ξανά.

![Πλήκτρο][api-management-aad-key-after-save]

Μεταβείτε ξανά στην πύλη του publisher και επικολλήστε τον αριθμό-κλειδί στο πλαίσιο κειμένου **Μυστικό προγράμματος-πελάτη** .

![Μυστικό προγράμματος-πελάτη][api-management-client-secret]

**Επιτρέπεται μισθωτές** καθορίζει τους καταλόγους που έχουν πρόσβαση σε τα API της παρουσίας υπηρεσίας API διαχείρισης. Καθορίστε τους τομείς των παρουσιών Azure Active Directory στο οποίο θέλετε να εκχωρήσετε πρόσβαση. Μπορείτε να διαχωρίσετε πολλούς τομείς με χαρακτήρες νέας γραμμής, κενά διαστήματα ή κόμματα.

![Οι επιτρεπόμενοι μισθωτές][api-management-client-allowed-tenants]

Στην ενότητα **Επιτρέπεται μισθωτές** , μπορεί να καθοριστεί πολλούς τομείς. Πριν από κάθε χρήστης μπορεί να συνδεθεί από διαφορετικό τομέα από τον αρχικό τομέα όπου έχει καταχωρηθεί η εφαρμογή, ο Καθολικός διαχειριστής του διαφορετικό τομέα πρέπει να εκχωρήσετε δικαιώματα για την εφαρμογή για πρόσβαση σε δεδομένα καταλόγου. Για να εκχωρήσετε δικαίωμα, ο Καθολικός διαχειριστής πρέπει να συνδεθείτε με την εφαρμογή και κάντε κλικ στην επιλογή **Αποδοχή**. Στο παρακάτω παράδειγμα `miaoaad.onmicrosoft.com` έχει προστεθεί για να **Επιτρέπεται μισθωτές** και καθολικός διαχειριστής από αυτόν τον τομέα καταγραφή στο για πρώτη φορά.

![Δικαιώματα][api-management-permissions-form]

>Εάν μη καθολικός διαχειριστής προσπαθεί να συνδεθεί πριν να έχουν εκχωρηθεί δικαιώματα από το καθολικό διαχειριστή, η προσπάθεια πρόσβασης αποτυγχάνει και εμφανίζεται η οθόνη ένα σφάλμα.

Όταν έχει οριστεί η επιθυμητή ρύθμιση παραμέτρων, κάντε κλικ στην επιλογή **Αποθήκευση**.

![Αποθήκευση][api-management-client-allowed-tenants-save]

Όταν οι αλλαγές αποθηκεύονται, τους χρήστες στην καθορισμένη Azure Active Directory μπορεί να συνδεθείτε με την πύλη για προγραμματιστές, ακολουθώντας τα βήματα στο θέμα [συνδεθείτε πύλη για προγραμματιστές χρησιμοποιώντας ένα λογαριασμό Azure Active Directory][].

## <a name="how-to-add-an-external-azure-active-directory-group"></a>Πώς μπορείτε να προσθέσετε μια εξωτερική ομάδα Azure Active Directory

Μετά την ενεργοποίηση της πρόσβασης για τους χρήστες σε μια Azure Active Directory, μπορείτε να προσθέσετε ομάδες Azure Active Directory στο API διαχείρισης για να διαχειριστείτε πιο εύκολα η συσχέτιση τους προγραμματιστές στην ομάδα με τα προϊόντα που θέλετε.

> Για να ρυθμίσετε μια εξωτερική ομάδα Azure Active Directory, το Azure Active Directory πρέπει πρώτα να ρυθμίσετε στην καρτέλα ταυτότητες, ακολουθώντας τη διαδικασία στην προηγούμενη ενότητα. 

Από την καρτέλα **ορατότητα** του προϊόντος για το οποίο θέλετε να εκχωρήσετε πρόσβαση στην ομάδα προστίθενται εξωτερικών ομάδες Azure Active Directory. Κάντε κλικ στην επιλογή **προϊόντα**και, στη συνέχεια, κάντε κλικ στο όνομα του προϊόντος επιθυμητή.

![Ρύθμιση παραμέτρων του προϊόντος][api-management-configure-product]

Μεταβείτε στην καρτέλα **ορατότητα** και κάντε κλικ στην επιλογή **Προσθήκη ομάδων από το Azure Active Directory**.

![Προσθήκη ομάδων][api-management-add-groups]

Επιλέξτε το **Azure Active Directory μισθωτή** από την αναπτυσσόμενη λίστα και, στη συνέχεια, πληκτρολογήστε το όνομα της ομάδας στην επιθυμητή στις **ομάδες** να προστεθούν πλαίσιο κειμένου.

![Επιλέξτε την ομάδα][api-management-select-group]

Αυτό το όνομα ομάδας μπορείτε να βρείτε στη λίστα **ομάδες** για το Azure Active Directory, όπως φαίνεται στο παρακάτω παράδειγμα.

![Λίστα ομάδων καταλόγου Azure Active Directory][api-management-aad-groups-list]

Κάντε κλικ στο κουμπί **Προσθήκη** για να επικυρώσει το όνομα της ομάδας και προσθέστε την ομάδα. Σε αυτό το παράδειγμα τους **Προγραμματιστές 5 Contoso** προστίθεται εξωτερική ομάδα. 

![Ομάδα που προστέθηκε][api-management-aad-group-added]

Κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τη νέα επιλογή ομάδα.

Όταν μια ομάδα Azure Azure Active Directory έχει ρυθμιστεί από ένα προϊόν, τότε είναι διαθέσιμη να γίνει μεταβίβαση ελέγχου στην καρτέλα **ορατότητα** για τα άλλα προϊόντα της παρουσίας υπηρεσίας API διαχείρισης.

Να αναθεωρείτε και να διαμορφώσετε τις ιδιότητες για εξωτερική ομάδες αφού ότι έχουν προστεθεί, κάντε κλικ στο όνομα της ομάδας από την καρτέλα **ομάδες** .

![Διαχείριση ομάδων][api-management-groups]

Από εδώ, μπορείτε να επεξεργαστείτε το **όνομα** και την **Περιγραφή** της ομάδας.

![Επεξεργασία ομάδας][api-management-edit-group]

Χρήστες από το ρυθμισμένο Azure Active Directory μπορεί να συνδεθείτε με την πύλη για προγραμματιστές και προβολή και εγγραφή σε ομάδες για τα οποία έχουν ορατότητα, ακολουθώντας τις οδηγίες στην επόμενη ενότητα.

## <a name="how-to-log-in-to-the-developer-portal-using-an-azure-active-directory-account"></a>Πώς μπορείτε να συνδεθείτε στην πύλη του προγραμματιστή χρησιμοποιώντας ένα λογαριασμό Azure Active Directory

Για να συνδεθείτε στην πύλη του προγραμματιστή χρησιμοποιώντας ένα λογαριασμό Azure Active Directory που έχει ρυθμιστεί στις προηγούμενες ενότητες, ανοίξτε ένα νέο παράθυρο του προγράμματος περιήγησης, χρησιμοποιώντας το **σύμβολο στη διεύθυνση URL** από τη ρύθμιση παραμέτρων της εφαρμογής υπηρεσίας καταλόγου Active Directory και κάντε κλικ στην επιλογή **Azure Active Directory**.

![Πύλη για προγραμματιστές][api-management-dev-portal-signin]

Εισαγάγετε τα διαπιστευτήρια ενός από τους χρήστες στο σας Azure Active Directory και κάντε κλικ στην επιλογή **Είσοδος**.

![Είσοδος][api-management-aad-signin]

Ενδέχεται να εμφανιστεί με μια φόρμα εγγραφής εάν απαιτείται τυχόν πρόσθετες πληροφορίες. Συμπληρώστε τη φόρμα καταχώρησης και κάντε κλικ στην επιλογή **εγγραφή**.

![Καταχώρηση][api-management-complete-registration]

Χρήστη σας τώρα έχουν συνδεθεί με την πύλη για προγραμματιστές για την παρουσία της υπηρεσίας διαχείρισης API.

![Η εγγραφή ολοκληρώθηκε][api-management-registration-complete]



[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Πρόσβαση σε γράφημα API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

[Συνδεθείτε πύλη για προγραμματιστές χρησιμοποιώντας ένα λογαριασμό Azure Active Directory]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account


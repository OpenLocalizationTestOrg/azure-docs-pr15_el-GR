<properties 
    pageTitle="Πώς μπορείτε να εγκρίνετε λογαριασμούς για προγραμματιστές που χρησιμοποιούν το διακριτικό 2.0 Azure API διαχείρισης" 
    description="Μάθετε πώς μπορείτε να εγκρίνετε οι χρήστες που χρησιμοποιούν 2.0 διακριτικό API διαχείρισης." 
    services="api-management" 
    documentationCenter="" 
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

# <a name="how-to-authorize-developer-accounts-using-oauth-20-in-azure-api-management"></a>Πώς μπορείτε να εγκρίνετε λογαριασμούς για προγραμματιστές που χρησιμοποιούν το διακριτικό 2.0 Azure API διαχείρισης

Πολλές διασυνδέσεις API υποστηρίζει το [διακριτικό 2.0](http://oauth.net/2/) για την ασφάλεια του API και βεβαιωθείτε ότι μόνο έγκυροι χρήστες έχουν πρόσβαση και μπορούν να έχουν μόνο πρόσβαση πόρους στο οποίο είστε έχει δικαίωμα. Προκειμένου να χρησιμοποιήσετε αλληλεπιδραστικών κονσόλα για προγραμματιστές του Azure API διαχείρισης με όπως API, την υπηρεσία σάς επιτρέπει να ρυθμίσετε την παρουσία της υπηρεσίας για να εργαστείτε με το διακριτικό 2.0 με δυνατότητα API.

## <a name="prerequisites"> </a>Τις προϋποθέσεις

Αυτός ο οδηγός σας δείχνει πώς μπορείτε να ρυθμίσετε τις παραμέτρους σας παρουσία της υπηρεσίας διαχείρισης API για να χρησιμοποιήσετε εξουσιοδότησης διακριτικό 2.0 για λογαριασμούς για προγραμματιστές, αλλά δεν σας δείξουν πώς μπορείτε να ρυθμίσετε τις παραμέτρους μιας υπηρεσίας παροχής διακριτικό 2.0. Η ρύθμιση παραμέτρων για κάθε υπηρεσία παροχής διακριτικό 2.0 είναι διαφορετικό, παρόλο που τα βήματα είναι παρόμοια και απαιτείται τμήματα της πληροφορίες που χρησιμοποιούνται σε ρύθμιση παραμέτρων OAuth 2.0 την παρουσία της υπηρεσίας διαχείρισης API είναι τα ίδια. Αυτό το θέμα εμφανίζει παραδείγματα που χρησιμοποιούν το Azure Active Directory ως μια υπηρεσία παροχής διακριτικό 2.0.

>[AZURE.NOTE] Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση OAuth 2.0, χρησιμοποιώντας την υπηρεσία καταλόγου Azure Active Directory, ανατρέξτε στο θέμα το δείγμα [WebApp-GraphAPI-DotNet][] .

## <a name="step1"> </a>Ρύθμισης παραμέτρων ενός διακομιστή εξουσιοδότησης 2.0 διακριτικό API διαχείρισης

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη του Publisher][api-management-management-console]

>[AZURE.NOTE] Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

Κάντε κλικ στην επιλογή " **ασφάλεια** " από το μενού **API διαχείρισης** στα αριστερά, κάντε κλικ **OAuth 2.0**, και, στη συνέχεια, κάντε κλικ στην επιλογή **Προσθήκη άδειας διακομιστή**.

![Διακριτικό 2.0][api-management-oauth2]

Μετά την κάνοντας κλικ στην επιλογή **Προσθήκη άδειας διακομιστή**, εμφανίζεται η νέα φόρμα εξουσιοδότησης στο διακομιστή.

![Νέο διακομιστή][api-management-oauth2-server-1]

Πληκτρολογήστε ένα όνομα και μια προαιρετική περιγραφή στα πεδία **όνομα** και μια **Περιγραφή** . 

>[AZURE.NOTE] Αυτά τα πεδία που χρησιμοποιούνται για να προσδιορίσετε το διακριτικό 2.0 διακομιστή εξουσιοδότησης εντός της τρέχουσας παρουσίας της υπηρεσίας API διαχείρισης και τις τιμές τους δεν προέρχονται από το διακομιστή OAuth 2.0.

Πληκτρολογήστε τη **διεύθυνση URL της σελίδας καταχώρηση προγράμματος-πελάτη**. Αυτή η σελίδα είναι όπου οι χρήστες μπορούν να δημιουργούν και να διαχειριστείτε τους λογαριασμούς, και ποικίλλει ανάλογα με την υπηρεσία παροχής 2.0 διακριτικό που χρησιμοποιείται. Η **διεύθυνση URL της σελίδας δήλωσης πελάτη** σημείων στη σελίδα που μπορούν να χρησιμοποιούν οι χρήστες για τη δημιουργία και ρύθμιση παραμέτρων τις δικές τους λογαριασμούς για το διακριτικό 2.0 υπηρεσίες παροχής που υποστηρίζουν διαχείριση λογαριασμών χρήστη. Ορισμένες εταιρείες δεν να ρυθμίσετε ή να χρησιμοποιήσετε αυτή η λειτουργία, ακόμα και εάν η υπηρεσία παροχής OAuth 2.0 υποστηρίζει αυτή. Εάν δεν έχει παροχής 2.0 διακριτικό χρήστη διαχείρισης λογαριασμών έχει ρυθμιστεί, καταχωρήστε μια διεύθυνση URL κράτησης θέσης εδώ όπως τη διεύθυνση URL της εταιρείας σας ή μια διεύθυνση URL όπως `https://placeholder.contoso.com`.

Στην επόμενη ενότητα της φόρμας περιέχει τις ρυθμίσεις **κωδικό εξουσιοδότησης εκχώρηση τύπους**, **διεύθυνση URL τελικού σημείου εξουσιοδότησης**και **μέθοδο αίτησης εξουσιοδότησης** .

![Νέο διακομιστή][api-management-oauth2-server-2]

Καθορίστε το **κωδικό εξουσιοδότησης εκχώρηση τύπους** , επιλέγοντας τους τύπους που θέλετε. **Ο κωδικός εξουσιοδότησης** καθορίζεται από προεπιλογή.

Πληκτρολογήστε τη **διεύθυνση URL τελικού σημείου εξουσιοδότησης**. Για το Azure Active Directory, αυτή η διεύθυνση URL θα είναι παρόμοια με την παρακάτω διεύθυνση URL, όπου `<client_id>` αντικαθίσταται με το αναγνωριστικό πελάτη που προσδιορίζει την εφαρμογή με το διακομιστή OAuth 2.0.

    https://login.windows.net/<client_id>/oauth2/authorize

Η **μέθοδος αίτησης εξουσιοδότησης** καθορίζει πώς η αίτηση εξουσιοδότησης αποστέλλεται στο διακομιστή διακριτικό 2.0. Από προεπιλογή επιλέγεται η **ΛΉΨΗ** .

Στην επόμενη ενότητα είναι όπου έχουν καθοριστεί η **διεύθυνση URL τελικού σημείου διακριτικό**, **μεθόδους ελέγχου ταυτότητας προγράμματος-πελάτη**, **διακριτικό πρόσβασης αποστολή μέθοδο**και **προεπιλεγμένο εύρος** .

![Νέο διακομιστή][api-management-oauth2-server-3]

Διακομιστής Azure Active Directory διακριτικό 2.0, το **διακριτικό διεύθυνση URL τελικού σημείου** θα έχει την εξής μορφή, όπου `<APPID>` έχει τη μορφή `yourapp.onmicrosoft.com`.

    https://login.windows.net/<APPID>/oauth2/token

Η προεπιλεγμένη ρύθμιση για **μεθόδους ελέγχου ταυτότητας προγράμματος-πελάτη** είναι **βασικές**και **διακριτικό πρόσβασης αποστολή μέθοδος** είναι η **κεφαλίδα ελέγχου ταυτότητας**. Αυτές οι τιμές έχουν ρυθμιστεί σε αυτήν την ενότητα της φόρμας, μαζί με το **προεπιλεγμένο εύρος**.

Η ενότητα **διαπιστευτήρια προγράμματος-πελάτη** περιέχει το **Αναγνωριστικό υπολογιστή-πελάτη** και το **μυστικό προγράμματος-πελάτη**, που προκύπτουν κατά τη διαδικασία δημιουργίας και ρύθμισης παραμέτρων του διακομιστή σας διακριτικό 2.0. Αφού έχουν καθοριστεί το **Αναγνωριστικό υπολογιστή-πελάτη** και **μυστικό προγράμματος-πελάτη** , δημιουργείται το **redirect_uri** για τον **κωδικό εξουσιοδότησης** . Σε αυτό το URI χρησιμοποιείται για να ρυθμίσετε τις παραμέτρους της διεύθυνσης URL απάντηση στη ρύθμιση παραμέτρων του διακομιστή σας διακριτικό 2.0.

![Νέο διακομιστή][api-management-oauth2-server-4]

Εάν **κωδικό εξουσιοδότησης εκχώρηση τύποι** έχει οριστεί **ο κωδικός πρόσβασης κατόχου πόρων**, την ενότητα **διαπιστευτήρια τον κωδικό πρόσβασης κατόχου πόρου** χρησιμοποιείται για να καθορίσετε αυτά τα διαπιστευτήρια; Διαφορετικά μπορείτε να αφήσετε το κενό.

![Νέο διακομιστή][api-management-oauth2-server-5]

Μόλις ολοκληρωθεί η φόρμα, κάντε κλικ στο κουμπί **Αποθήκευση** για να αποθηκεύσετε τη ρύθμιση παραμέτρων του διακομιστή εξουσιοδότησης API διαχείρισης διακριτικό 2.0. Αφού αποθηκεύσετε τη ρύθμιση παραμέτρων του διακομιστή, μπορείτε να ρυθμίσετε API για να χρησιμοποιήσετε αυτήν τη ρύθμιση, όπως φαίνεται στην επόμενη ενότητα.

## <a name="step2"> </a>Ρύθμιση παραμέτρων API για να χρησιμοποιήσετε εξουσιοδότηση χρήστη διακριτικό 2.0

Κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά, κάντε κλικ στο όνομα του API θέλετε, κάντε κλικ στην επιλογή **ασφάλεια**και, στη συνέχεια, επιλέξτε το πλαίσιο ελέγχου για το **διακριτικό 2.0**.

![Εξουσιοδότηση χρήστη][api-management-user-authorization]

Επιλέξτε το επιθυμητό **εξουσιοδότησης διακομιστή** από την αναπτυσσόμενη λίστα και κάντε κλικ στην επιλογή **Αποθήκευση**.

![Εξουσιοδότηση χρήστη][api-management-user-authorization-save]

## <a name="step3"> </a>Δοκιμή άδειας χρήστη διακριτικό 2.0 στην πύλη για προγραμματιστές

Αφού έχετε ρυθμίσει τις παραμέτρους του διακομιστή εξουσιοδότησης διακριτικό 2.0 και ρύθμιση των παραμέτρων σας API για να χρησιμοποιήσετε αυτόν το διακομιστή, μπορείτε να το ελέγξετε, μεταβαίνοντας στην πύλη για προγραμματιστές και κλήση API.  Κάντε κλικ στην επιλογή **Προγραμματιστής πύλη** στο μενού επάνω δεξιά.

![Πύλη για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** στο επάνω μενού και επιλέξτε **API Αντήχηση**.

![API αντήχηση][api-management-apis-echo-api]

>[AZURE.NOTE] Εάν έχετε μόνο μία API ρύθμιση παραμέτρων ή ορατό στο λογαριασμό σας, στη συνέχεια, κάνοντας κλικ στην επιλογή APIs μεταβαίνετε απευθείας τις λειτουργίες για το συγκεκριμένο API.

Επιλέξτε τη λειτουργία **ΛΉΨΗ πόρων** , κάντε κλικ στην επιλογή **Άνοιγμα κονσόλας**, και κατόπιν επιλέξτε **εξουσιοδότηση κώδικα** από την αναπτυσσόμενη λίστα.

![Άνοιγμα κονσόλας][api-management-open-console]

Όταν είναι επιλεγμένο **κωδικό εξουσιοδότησης** , εμφανίζεται ένα αναδυόμενο παράθυρο με τη φόρμα εισόδου της υπηρεσίας παροχής OAuth 2.0. Σε αυτό το παράδειγμα στη φόρμα εισόδου παρέχεται από το Azure Active Directory.

>[AZURE.NOTE] Εάν έχετε απενεργοποιήσει αναδυόμενα παράθυρα που θα σας ζητηθεί να ενεργοποιήσετε τους από το πρόγραμμα περιήγησης. Αφού ενεργοποιήσετε τους, επιλέξτε **κωδικό εξουσιοδότησης** ξανά και στη φόρμα εισόδου θα εμφανίζονται.

![Είσοδος][api-management-oauth2-signin]

Όταν έχετε πραγματοποιήσει είσοδο, τις **κεφαλίδες αίτησης** συμπληρώνονται με ένα `Authorization : Bearer` κεφαλίδα που επιτρέπει την αίτηση.

![Ζητήστε κωδικό κεφαλίδας][api-management-request-header-token]

Σε αυτό το σημείο μπορείτε να ρυθμίσετε τις παραμέτρους τις επιθυμητές τιμές για τις υπόλοιπες παραμέτρους και υποβολή της αίτησης. 

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με τη χρήση OAuth 2.0 και API διαχείρισης, ανατρέξτε στο θέμα βίντεο που ακολουθεί και συνοδεύουν [το άρθρο](api-management-howto-protect-backend-with-aad.md).

> [AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

[api-management-management-console]: ./media/api-management-howto-oauth2/api-management-management-console.png
[api-management-oauth2]: ./media/api-management-howto-oauth2/api-management-oauth2.png
[api-management-user-authorization]: ./media/api-management-howto-oauth2/api-management-user-authorization.png
[api-management-user-authorization-save]: ./media/api-management-howto-oauth2/api-management-user-authorization-save.png
[api-management-oauth2-signin]: ./media/api-management-howto-oauth2/api-management-oauth2-signin.png
[api-management-request-header-token]: ./media/api-management-howto-oauth2/api-management-request-header-token.png
[api-management-developer-portal-menu]: ./media/api-management-howto-oauth2/api-management-developer-portal-menu.png
[api-management-open-console]: ./media/api-management-howto-oauth2/api-management-open-console.png
[api-management-oauth2-server-1]: ./media/api-management-howto-oauth2/api-management-oauth2-server-1.png
[api-management-oauth2-server-2]: ./media/api-management-howto-oauth2/api-management-oauth2-server-2.png
[api-management-oauth2-server-3]: ./media/api-management-howto-oauth2/api-management-oauth2-server-3.png
[api-management-oauth2-server-4]: ./media/api-management-howto-oauth2/api-management-oauth2-server-4.png
[api-management-oauth2-server-5]: ./media/api-management-howto-oauth2/api-management-oauth2-server-5.png
[api-management-apis-echo-api]: ./media/api-management-howto-oauth2/api-management-apis-echo-api.png


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

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API to use OAuth 2.0 user authorization]: #step2
[Test the OAuth 2.0 user authorization in the Developer Portal]: #step3
[Next steps]: #next-steps

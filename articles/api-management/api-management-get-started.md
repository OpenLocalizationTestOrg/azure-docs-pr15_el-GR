<properties
    pageTitle="Διαχείριση του πρώτου API Azure API διαχείρισης | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να δημιουργήσετε APIs, να προσθέσετε λειτουργίες και γρήγορα αποτελέσματα με το API διαχείρισης."
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
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="manage-your-first-api-in-azure-api-management"></a>Διαχείριση του πρώτου API Azure API διαχείρισης

## <a name="overview"> </a>Επισκόπηση

Αυτός ο οδηγός θα μάθετε πώς να ξεκινήσετε χρησιμοποιώντας Azure API διαχείρισης γρήγορα και να κάνετε την πρώτη κλήση API.

## <a name="concepts"> </a>Τι είναι η διαχείριση API Azure;

Μπορείτε να χρησιμοποιήσετε Azure API διαχείρισης για να τραβήξετε οποιαδήποτε παρασκηνίου και εκκίνηση πλήρως ανεπτυγμένο API του προγράμματος που βασίζεται σε αυτό.

Συνηθισμένα σενάρια περιλαμβάνουν τα εξής:

* **Κινητές συσκευές υποδομή ασφαλή** από πυλών πρόσβαση με κλειδιά API, αποτροπή DOS επιθέσεις με χρήση του περιορισμού ή τη χρήση πολιτικών ασφαλείας για προχωρημένους όπως JWT διακριτικού επικύρωσης.
* **Ενεργοποίηση ISV συνεργάτη οικοσυστημάτων** , σας δίνει τη δυνατότητα προσθήκης λογαριασμών fast συνεργάτη μέσω της πύλης προγραμματιστής και τη δημιουργία μιας πρόσοψη API να ξεχωρίζουμε από εσωτερικές υλοποίησης που δεν είναι ώριμα για κατανάλωση συνεργάτη.
* **Εκτελεί μια εσωτερική πρόγραμμα API** από μια κεντρική θέση για τον οργανισμό σας δίνει τη δυνατότητα για την επικοινωνία σχετικά με τη διαθεσιμότητα και τις πιο πρόσφατες αλλαγές σε API, πυλών πρόσβασης βάσει εταιρικοί λογαριασμοί, όλα με βάση ασφαλούς καναλιού μεταξύ της πύλης API και του υπολογιστή στο παρασκήνιο.


Το σύστημα αποτελείται από τα ακόλουθα στοιχεία:

* Η **πύλη API** είναι το τελικό σημείο που:
  * Αποδέχεται API καλεί και δρομολογεί το παρασκήνιο.
  * Επαληθεύει κλειδιά API, τα διακριτικά JWT, πιστοποιητικά και άλλα διαπιστευτήρια.
  * Επιβάλλει ορίων χρήσης και χαρακτηρισμός όρια.
  * Μετατρέπει το API στη διάρκεια της λειτουργίας χωρίς τροποποιήσεις κώδικα.
  * Αποθηκεύει προσωρινά τις απαντήσεις παρασκηνίου όπου ρυθμίσετε.
  * Αρχεία καταγραφής κλήσεων μετα-δεδομένων για σκοπούς ανάλυσης.

* Η **πύλη του publisher** είναι το περιβάλλον εργασίας διαχείρισης όπου ρυθμίζετε το πρόγραμμα API. Χρησιμοποιήστε το για να:
    * Ορισμός ή εισαγωγή API του σχήματος.
    * Το πακέτο APIs σε προϊόντα.
    * Ρύθμιση πολιτικών όπως ορίων ή μετασχηματισμοί για τα API.
    * Λήψη ιδεών από ανάλυσης.
    * Διαχείριση χρηστών.

* Η **πύλη για προγραμματιστές** λειτουργεί ως το κύριο web παρουσίας για τους προγραμματιστές, όπου να:
    * Ανάγνωση API τεκμηρίωση.
    * Δοκιμάστε ένα API μέσω της αλληλεπιδραστικών κονσόλας.
    * Δημιουργήστε ένα λογαριασμό και να εγγραφείτε για να λάβετε κλειδιά API.
    * Πρόσβαση αναλυτικών στοιχείων χρήσης δικό τους.


## <a name="create-service-instance"> </a>Δημιουργία παρουσίας API διαχείρισης

>[AZURE.NOTE] Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρειάζεστε ένα λογαριασμό Azure. Εάν δεν έχετε ένα λογαριασμό, μπορείτε να δημιουργήσετε έναν δωρεάν λογαριασμό σε λίγα λεπτά. Για λεπτομέρειες, ανατρέξτε στο θέμα [Azure δωρεάν δοκιμαστικής έκδοσης][].

Εργασία με το API διαχείρισης το πρώτο βήμα είναι να δημιουργήσετε μια παρουσία της υπηρεσίας. Πραγματοποιήστε είσοδο στην [Πύλη κλασική Azure][] και κάντε κλικ στην επιλογή **Δημιουργία**, **Εφαρμογή υπηρεσιών**, **API διαχείρισης**, **Δημιουργία**.

![Νέα παρουσία API διαχείρισης][api-management-create-instance-menu]

Για **διεύθυνση URL**, καθορίστε ένα όνομα μοναδικές υποτομέα για να χρησιμοποιήσετε για τη διεύθυνση URL της υπηρεσίας.

Επιλέξτε την επιθυμητή **συνδρομής** και **περιοχής** για την παρουσία της υπηρεσίας. Αφού κάνετε τις επιλογές σας, κάντε κλικ στο κουμπί **Επόμενο** .

![Νέα υπηρεσία API διαχείρισης][api-management-create-instance-step1]

Πληκτρολογήστε **Contoso Ltd.** για το **Όνομα της εταιρείας**, και πληκτρολογήστε τη διεύθυνση ηλεκτρονικού ταχυδρομείου στο πεδίο **E-Mail διαχειριστή** .

>[AZURE.NOTE] Χρησιμοποιείται αυτή η διεύθυνση ηλεκτρονικού ταχυδρομείου για τις ειδοποιήσεις από το σύστημα διαχείρισης API. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να ρυθμίσετε τις ειδοποιήσεις και τα πρότυπα ηλεκτρονικού ταχυδρομείου του Azure API διαχείρισης][].

![Νέα υπηρεσία API διαχείρισης][api-management-create-instance-step2]

Παρουσίες υπηρεσίας API διαχείρισης είναι διαθέσιμα σε τρεις σειρές: προγραμματιστή, τυπική και Premium. Από προεπιλογή, οι νέες παρουσίες υπηρεσίας API διαχείρισης δημιουργούνται στο επίπεδο προγραμματιστής. Για να επιλέξετε το επίπεδο τυπική ή Premium, επιλέξτε το πλαίσιο ελέγχου **ρυθμίσεις για προχωρημένους** και επιλέξτε την επιθυμητή σειρά στην παρακάτω οθόνη.

>[AZURE.NOTE] Το επίπεδο για προγραμματιστές είναι για ανάπτυξη, δοκιμή και ενδεικτικών προγραμμάτων API όπου υψηλή διαθεσιμότητα δεν είναι ένα πρόβλημα. Στο τις τυπικές και Premium σειρές, να κλίμακα σας καταμέτρηση δεσμευμένη μονάδας για να χειριστούν περισσότερη κυκλοφορία. Τις τυπικές και Premium βαθμίδες παρέχουν την υπηρεσία διαχείρισης API με τα περισσότερα ισχύος επεξεργασίας και επιδόσεων. Μπορείτε να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης, χρησιμοποιώντας οποιαδήποτε σειρά. Για περισσότερες πληροφορίες σχετικά με το API διαχείρισης βαθμίδες, ανατρέξτε στο θέμα [Διαχείριση API τις τιμές][].

Επιλέξτε το πλαίσιο ελέγχου για να δημιουργήσετε την παρουσία της υπηρεσίας.

![Νέα υπηρεσία API διαχείρισης][api-management-instance-created]

Όταν δημιουργηθεί η παρουσία της υπηρεσίας, το επόμενο βήμα είναι να δημιουργήσετε ή να εισαγάγετε ένα API.

## <a name="create-api"> </a>Εισαγωγή API

API αποτελείται από ένα σύνολο των λειτουργιών που μπορούν να ενεργοποιηθούν από μια εφαρμογή προγράμματος-πελάτη. Λειτουργίες API είναι μέσω διακομιστή μεσολάβησης σε υπάρχουσες υπηρεσίες web.

Μπορούν να δημιουργηθούν APIs (και λειτουργίες μπορούν να προστεθούν) με μη αυτόματο τρόπο, ή να μπορούν να εισαχθούν. Σε αυτό το πρόγραμμα εκμάθησης, θα εισαγάγουμε το API για μια υπηρεσία δείγμα Αριθμομηχανής web που παρέχεται από τη Microsoft και φιλοξενείται σε Azure.

>[AZURE.NOTE] Για οδηγίες σχετικά με τη δημιουργία API και μη αυτόματη προσθήκη λειτουργίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε APIs](api-management-howto-create-apis.md) και [πώς μπορείτε να προσθέσετε εργασίες για μια API](api-management-howto-add-operations.md).

APIs ρυθμίζονται από την πύλη publisher, το οποίο έχετε πρόσβαση μέσω της πύλης κλασική Azure. Για την επίτευξη πύλη του publisher, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API.

![Πύλη του Publisher][api-management-management-console]

Για να εισαγάγετε την Αριθμομηχανή API, κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Εισαγωγή API**.

![Κουμπί "Εισαγωγή API"][api-management-import-api]

Ακολουθήστε τα παρακάτω βήματα για να ρυθμίσετε την Αριθμομηχανή API:

1. Κάντε κλικ στην επιλογή **Από διεύθυνση URL**, πληκτρολογήστε **http://calcapi.cloudapp.net/calcapi.json** στο πλαίσιο κειμένου **διεύθυνση URL εγγράφου προδιαγραφή** και κάντε κλικ στο κουμπί επιλογής **Swagger** .
2. Πληκτρολογήστε **Υπολογισμός** στο πλαίσιο **διεύθυνση URL του API Web επίθημα** κειμένου.
3. Κάντε κλικ στο πλαίσιο **προϊόντα (προαιρετικό)** και επιλέξτε **Starter**.
4. Κάντε κλικ στο κουμπί **Αποθήκευση** για να εισαγάγετε το API.

![Προσθήκη νέου API][api-management-import-new-api]

>[AZURE.NOTE] **API διαχείρισης** υποστηρίζει επί του παρόντος έκδοση 1.2 και 2.0 Swagger εγγράφου για εισαγωγή. Βεβαιωθείτε ότι, παρόλο που [προδιαγραφή Swagger 2.0](http://swagger.io/specification) δηλώνει ότι `host`, `basePath`, και `schemes` ιδιότητες είναι προαιρετική, το έγγραφό σας Swagger 2.0 **πρέπει να** περιέχει αυτές τις ιδιότητες; διαφορετικά το δεν θα εισαχθούν. 

Μόλις εισαχθούν τα API, τη σελίδα σύνοψης για το API εμφανίζεται στην πύλη του publisher.

![Σύνοψη API][api-management-imported-api-summary]

Στην ενότητα API έχει περισσότερες από μία σελίδες. Στην καρτέλα **Σύνοψη** Εμφανίζει βασικές μετρικά και πληροφορίες σχετικά με το API. Στην καρτέλα [Ρυθμίσεις](api-management-howto-create-apis.md#configure-api-settings) χρησιμοποιείται για την προβολή και επεξεργασία της ρύθμισης παραμέτρων για το API. Στην καρτέλα [Λειτουργίες](api-management-howto-add-operations.md) χρησιμοποιείται για τη διαχείριση του API λειτουργίες. Στην καρτέλα **ασφάλεια** μπορεί να χρησιμοποιηθεί για τη ρύθμιση παραμέτρων ελέγχου ταυτότητας πύλης για το διακομιστή παρασκηνίου χρησιμοποιώντας το βασικό έλεγχο ταυτότητας ή [αμοιβαία πιστοποιητικό ελέγχου ταυτότητας](api-management-howto-mutual-certificates.md)και να ρυθμίσετε τις παραμέτρους [εξουσιοδότηση χρήστη χρησιμοποιώντας το διακριτικό 2.0](api-management-howto-oauth2.md).  Καρτέλα " **θέματα** " χρησιμοποιείται για την προβολή των θεμάτων που αναφέρονται από τους προγραμματιστές που χρησιμοποιούν το APIs. Στην καρτέλα **προϊόντα** χρησιμοποιείται για τη ρύθμιση παραμέτρων των προϊόντων που περιέχουν αυτό το API.

Από προεπιλογή, κάθε παρουσία API διαχείρισης συνοδεύεται από δύο δείγματα προϊόντων:

-   **Starter**
-   **Απεριόριστος χώρος**

Σε αυτό το πρόγραμμα εκμάθησης, το βασικό API Αριθμομηχανής προστέθηκε με το προϊόν Starter, όταν το API έγινε εισαγωγή.

Για να κάνετε κλήσεις API, οι προγραμματιστές πρέπει να πρώτα να εγγραφείτε σε ένα προϊόν που παρέχει πρόσβαση σε αυτό. Οι προγραμματιστές να εγγραφείτε σε προϊόντα στην πύλη για προγραμματιστές ή οι διαχειριστές μπορούν να εγγραφούν στους προγραμματιστές να προϊόντα στην πύλη του publisher. Είστε διαχειριστής δεδομένου ότι δημιουργήσατε την παρουσία API διαχείρισης στα προηγούμενα βήματα στο το πρόγραμμα εκμάθησης, ώστε να έχετε ήδη εγγραφεί για κάθε προϊόν από προεπιλογή.

## <a name="call-operation"> </a>Κλήση μιας λειτουργίας από την πύλη για προγραμματιστές

Λειτουργίες μπορεί να ονομάζεται απευθείας από την πύλη για προγραμματιστές, η οποία παρέχει έναν εύχρηστο τρόπο για να προβάλετε και να ελέγξετε τις λειτουργίες του API. Σε αυτό το βήμα προγραμμάτων εκμάθησης, θα ονομάσετε το βασικό API Αριθμομηχανής λειτουργία **πρόσθετο δύο ακέραιους αριθμούς** . Κάντε κλικ στην επιλογή **πύλη για προγραμματιστές** από το μενού στην επάνω δεξιά γωνία της την πύλη του publisher.

![Πύλη για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** από το επάνω μενού και, στη συνέχεια, κάντε κλικ στην επιλογή **Βασική Αριθμομηχανή** για να δείτε τις διαθέσιμες λειτουργίες.

![Πύλη για προγραμματιστές][api-management-developer-portal-calc-api]

Σημείωση το δείγμα περιγραφές και τις παραμέτρους που έχουν εισαχθεί μαζί με το API και διαδικασιών, παρέχοντας τεκμηρίωση για τους προγραμματιστές που θα χρησιμοποιήσετε αυτήν τη λειτουργία. Αυτές οι περιγραφές μπορούν επίσης να προστεθούν όταν λειτουργίες προστίθενται με μη αυτόματο τρόπο.

Για να καλέσετε τη λειτουργία **πρόσθετο δύο ακέραιους αριθμούς** , κάντε κλικ στην επιλογή **Δοκιμή**.

![Δοκιμάστε το][api-management-developer-portal-calc-api-console]

Μπορείτε να εισαγάγετε ορισμένες τιμές για τις παραμέτρους ή διατηρήστε τις προεπιλεγμένες ρυθμίσεις και, στη συνέχεια, κάντε κλικ στην επιλογή **Αποστολή**.

![HTTP Get][api-management-invoke-get]

Αφού μια λειτουργία ενεργοποιείται, την πύλη για προγραμματιστές εμφανίζει την **κατάσταση απόκρισης**, τις **κεφαλίδες απόκρισης**και οποιοδήποτε **περιεχόμενο απόκρισης**.

![Απόκριση][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Προβολή ανάλυσης

Για να προβάλετε αναλυτικά στοιχεία για βασική Αριθμομηχανή, μεταβείτε στην πύλη του publisher, επιλέγοντας **Διαχείριση** από το μενού στο επάνω μέρος δεξιά από την πύλη για προγραμματιστές.

![Διαχείριση][api-management-manage-menu]

Η προεπιλεγμένη προβολή για την πύλη του publisher είναι του **πίνακα εργαλείων**, που παρέχει μια επισκόπηση της παρουσίας σας API διαχείρισης.

![Πίνακας εργαλείων][api-management-dashboard]

Τοποθετήστε το δείκτη του ποντικιού πάνω από το γράφημα για **Βασική Αριθμομηχανή** για να δείτε τις συγκεκριμένες μετρήσεις για τη χρήση του API για μια δεδομένη χρονική περίοδο.

>[AZURE.NOTE] Εάν δεν βλέπετε όλες τις γραμμές στο γράφημά σας, μεταβείτε ξανά στην πύλη του προγραμματιστή και ορισμένες κλήσεις σε το API, περιμένετε λίγα λεπτά και, στη συνέχεια, επιστρέψτε στον πίνακα εργαλείων.

Κάντε κλικ στην επιλογή **Προβολή λεπτομερειών** για να προβάλετε τη σελίδα σύνοψης για το API, όπως μια μεγαλύτερη έκδοση την εμφανιζόμενη μετρικών.

![Ανάλυση][api-management-mouse-over]

![Σύνοψη][api-management-api-summary-metrics]

Για λεπτομερείς μετρικά και αναφορές, κάντε κλικ στην επιλογή **αναλυτικών στοιχείων** από το **API διαχείρισης** μενού στα αριστερά.

![Επισκόπηση][api-management-analytics-overview]

Στην ενότητα **αναλυτικών στοιχείων** περιλαμβάνει τα παρακάτω τέσσερις καρτέλες:

-   **Με μια ματιά** παρέχει συνολική μετρικά εύρυθμης λειτουργίας και χρήσης, καθώς και το επάνω τους προγραμματιστές δημοφιλή προϊόντα, επάνω APIs λειτουργίες, και επάνω.
-   **Χρήση** παρέχει μια αναλυτικά ματιά στην κλήσεις API και το εύρος ζώνης, όπως μια γεωγραφική αναπαράσταση.
-   **Εύρυθμη λειτουργία** εστιάζει σε κατάσταση κωδικών, cache συντελεστές επιτυχίας, χρόνους απόκρισης και API και υπηρεσίας χρόνους απόκρισης.
-   **Δραστηριότητα** παρέχει αναφορές που Διερεύνηση σχετικά με τη συγκεκριμένη δραστηριότητα προγραμματιστή, προϊόντος, API και λειτουργία.

## <a name="next-steps"> </a>Επόμενα βήματα

- Μάθετε πώς μπορείτε να [προστασία σας API με τα όρια](api-management-howto-product-with-rules.md).

[Azure δωρεάν δοκιμαστική έκδοση]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Πώς μπορείτε να ρυθμίσετε τις παραμέτρους ειδοποιήσεις και τα πρότυπα ηλεκτρονικού ταχυδρομείου Azure API διαχείρισης]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Τις τιμές για το API διαχείρισης]: http://azure.microsoft.com/pricing/details/api-management/

[Azure κλασική πύλη]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png

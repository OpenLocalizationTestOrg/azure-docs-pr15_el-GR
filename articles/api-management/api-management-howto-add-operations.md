<properties 
    pageTitle="Πώς μπορείτε να προσθέσετε εργασίες για μια API διαχείρισης API Azure | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να προσθέσετε λειτουργίες API Azure API διαχείρισης." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Πώς μπορείτε να προσθέσετε εργασίες για μια API διαχείρισης API Azure

Μπορεί να χρησιμοποιηθεί ένα API API διαχείρισης, πρέπει να προστεθεί λειτουργίες. Αυτός ο οδηγός δείχνει πώς μπορείτε να προσθέσετε και να ρυθμίσετε τις παραμέτρους διαφορετικούς τύπους λειτουργιών για ένα API API διαχείρισης.

## <a name="add-operation"> </a>Προσθέστε μια πράξη

Λειτουργίες προστίθενται και έχει ρυθμιστεί ώστε να API στην πύλη του publisher. Για να αποκτήσετε πρόσβαση στην πύλη του publisher, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API.

![Πύλη του Publisher][api-management-management-console]

>Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

Επιλέξτε το επιθυμητό API στην πύλη του publisher και, στη συνέχεια, επιλέξτε την καρτέλα **Λειτουργίες** . 

![Λειτουργίες][api-management-operations]

Κάντε κλικ στην επιλογή **Προσθήκη λειτουργίας** για να προσθέσετε μια νέα λειτουργία. Θα εμφανιστεί η **νέα λειτουργία** και θα είναι επιλεγμένη στην καρτέλα **υπογραφή** από προεπιλογή.

![Η λειτουργία προσθήκης][api-management-add-operation]

Καθορίστε το **Ρηματικές HTTP** , επιλέγοντας από την αναπτυσσόμενη λίστα.

![Μέθοδος HTTP][api-management-http-method]

<a name="url-template"></a>

Ορισμός του προτύπου διεύθυνση URL, πληκτρολογώντας μια διεύθυνση URL τμήματος που αποτελείται από ένα ή περισσότερα τμήματα διαδρομή διεύθυνσης URL και παραμέτρους κανέναν ή περισσότερους συμβολοσειράς ερωτήματος. Το πρότυπο διεύθυνση URL, προσαρτημένο σε τη βασική διεύθυνση URL του API, προσδιορίζει μία λειτουργία HTTP. Μπορεί να περιέχει ένα ή περισσότερα με το όνομα μεταβλητής τμημάτων που αναγνωρίζονται από τα άγκιστρα. Αυτά τα τμήματα που μεταβλητής ονομάζονται παραμέτρους προτύπου και έχουν εκχωρηθεί δυναμικά τιμές που έχουν εξαχθεί από τη διεύθυνση URL της αίτησης όταν η αίτηση υποβάλλεται σε επεξεργασία από την πλατφόρμα API διαχείρισης.

![Διεύθυνση URL προτύπου][api-management-url-template]

<a name="rewrite-url-template"></a>

Εάν θέλετε, καθορίστε το **πρότυπο επανεγγραφή διεύθυνσης URL**. Αυτό σας επιτρέπει να χρησιμοποιήσετε το τυπικό πρότυπο διεύθυνση URL για την επεξεργασία εισερχόμενες προσκλήσεις σε προσκηνίου, κατά την κλήση του παρασκηνίου μέσω μιας διεύθυνσης URL που έχει μετατραπεί σύμφωνα με το πρότυπο αναδιατύπωση. Παράμετροι προτύπου από το πρότυπο διεύθυνση URL πρέπει να χρησιμοποιούνται στο πρότυπο αναδιατύπωση. Το παρακάτω παράδειγμα εμφανίζει τον τύπο περιεχομένου πώς κωδικοποιηθεί ως τμήμα διαδρομής στην υπηρεσία web από το προηγούμενο παράδειγμα μπορεί να παρέχεται ως παραμέτρου ερωτήματος στο API δημοσιευτεί μέσω της πλατφόρμας API διαχείρισης χρησιμοποιώντας τα πρότυπα διεύθυνση URL.

![Διεύθυνση URL προτύπου αναδιατύπωση][api-management-url-template-rewrite]

Οι καλούντες για τη λειτουργία θα χρησιμοποιήσει τη μορφή `/customers?customerid=ALFKI` και αυτό θα αντιστοιχιστούν `/Customers('ALFKI')` όταν ενεργοποιείται η υπηρεσία παρασκηνίου.


**Εμφανιζόμενο** όνομα και μια **Περιγραφή** Δώστε μια περιγραφή της λειτουργίας και χρησιμοποιούνται για να παράσχετε τεκμηρίωση για τους προγραμματιστές χρησιμοποιώντας αυτό το API στην πύλη για προγραμματιστές.

![Περιγραφή][api-management-description]

Η περιγραφή λειτουργίας μπορεί να οριστεί ως απλού κειμένου ή HTML στο πλαίσιο κειμένου **Περιγραφή** .

## <a name="operation-caching"> </a>Λειτουργίας σε cache

Απάντηση σε cache μειώνει λανθάνων χρόνος θεωρούνται από το API των καταναλωτών, μειώνει κατανάλωση εύρους ζώνης και μειώσεων των χρεώσεων η φόρτωση στο HTTP web υπηρεσίας εφαρμογής το API. 

Για να εύκολα και γρήγορα ενεργοποίηση του cache για τη λειτουργία, επιλέξτε την καρτέλα **προσωρινή αποθήκευση** και επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση** .

![Προσωρινή αποθήκευση][api-management-caching-tab]

**Διάρκεια** Καθορίζει τη χρονική περίοδο κατά την οποία η απόκριση λειτουργία παραμένει στο cache. Η προεπιλεγμένη τιμή είναι 3.600 δευτερόλεπτα ή 1 ώρα.

Πλήκτρα cache που χρησιμοποιούνται για να διαφοροποιήσετε μεταξύ των απαντήσεων, ώστε να την απάντηση που αντιστοιχεί σε κάθε αριθμό-κλειδί διαφορετική cache θα λάβετε τη δική του ξεχωριστή τιμή στο cache. Προαιρετικά, εισαγάγετε συγκεκριμένο ερώτημα παραμέτρων συμβολοσειράς ή/και κεφαλίδες HTTP που θα χρησιμοποιηθεί σε υπολογιστική cache βασικές τιμές στα πλαίσια κειμένου **ανάλογα με τις παραμέτρους συμβολοσειράς ερωτήματος** και **ανάλογα με την κεφαλίδες** αντίστοιχα. Όταν καμία αίτηση καθορισμένο, πλήρους διεύθυνσης URL και τις παρακάτω τιμές κεφαλίδας HTTP χρησιμοποιούνται στο cache Δημιουργία κλειδιών: **Αποδοχή** και **Αποδοχή-σύνολο χαρακτήρων**.

>Για περισσότερες πληροφορίες σχετικά με την αποθήκευση σε cache και σε cache πολιτικές, δείτε [πώς μπορείτε να αποτελέσματα λειτουργίας cache Azure API διαχείρισης][].


## <a name="request-parameters"> </a>Παράμετροι αίτησης

Παράμετροι λειτουργίας πραγματοποιείται στην καρτέλα παράμετροι. Παράμετροι που καθορίζεται στο **Πρότυπο διεύθυνση URL** στην καρτέλα **υπογραφή** προστίθενται αυτόματα και μπορεί να αλλάξει μόνο με την επεξεργασία του προτύπου διεύθυνση URL. Πρόσθετες παράμετροι μπορεί να εισαχθεί με μη αυτόματο τρόπο.

Για να προσθέσετε μια νέα παραμέτρου ερωτήματος, κάντε κλικ στην επιλογή **Προσθήκη παραμέτρου ερωτήματος** και καταχωρήστε τις ακόλουθες πληροφορίες:

-   **Όνομα** - όνομα παραμέτρου.
-   **Περιγραφή** - μια σύντομη περιγραφή της παραμέτρου (προαιρετικό).
-   **Τύπος** - τύπος παραμέτρου, επιλεγμένο στο αναπτυσσόμενο μενού προς τα κάτω.
-   **Τιμές** - τιμές που μπορεί να αντιστοιχιστεί σε αυτήν την παράμετρο. Μία από τις τιμές μπορούν να επισημανθούν ως προεπιλογή (προαιρετικό).
-   **Απαιτείται** - κάνετε υποχρεωτική την παράμετρο, επιλέγοντας το πλαίσιο ελέγχου. 

![Παράμετροι αίτησης][api-management-request-parameters]

## <a name="request-body"> </a>Σώμα αίτησης

Εάν η λειτουργία επιτρέπει (π.χ., ΘΈΣΗ, ΔΗΜΟΣΊΕΥΣΗ) και απαιτεί έναν οργανισμό που μπορεί να σας παρέχει ένα παράδειγμα της με όλες τις μορφές υποστηριζόμενες αναπαράσταση (π.χ. json, XML). 

>Σώμα της αίτησης χρησιμοποιείται για τεκμηρίωση μόνο και δεν είναι επικύρωση.

Για να εισαγάγετε μια πρόσκληση σε σώμα, μεταβείτε στην καρτέλα **σώμα** .

Κάντε κλικ στην επιλογή **Προσθήκη αναπαράσταση**, αρχίστε να πληκτρολογείτε το όνομα του τύπου περιεχομένου που θέλετε (π.χ. εφαρμογή/json), επιλέξτε το από την αναπτυσσόμενη λίστα και επικολλήστε το παράδειγμα σώματος επιθυμητή αίτηση στην επιλεγμένη μορφή στο πλαίσιο κειμένου. 

![Σώμα αίτησης][api-management-request-body]

Σε επιπλέον σε παραστάσεις, μπορείτε επίσης να καθορίσετε μια προαιρετική περιγραφή στο πλαίσιο κειμένου **Περιγραφή** .

## <a name="responses"> </a>Απαντήσεων

Είναι καλό να παρέχουν παραδείγματα απαντήσεων για όλους τους κωδικούς κατάστασης που μπορεί να δημιουργήσουν τη λειτουργία. Κάθε κωδικός κατάστασης μπορεί να έχει περισσότερες από μία απάντηση σώμα το παράδειγμα, μία για κάθε έναν από τους τύπους περιεχομένου που υποστηρίζονται. 

Για να προσθέσετε μια απάντηση, κάντε κλικ στην επιλογή **Προσθήκη** και αρχίστε να πληκτρολογείτε τον κωδικό επιθυμητή κατάστασης. Σε αυτό το παράδειγμα είναι ο κωδικός κατάστασης **200 OK**. Όταν ο κώδικας εμφανίζεται στο αναπτυσσόμενο μενού, επιλέξτε το και ο κωδικός απόκρισης δημιουργείται και προστίθεται σε λειτουργία σας.

![Κωδικός απόκρισης][api-management-response-code]

Κάντε κλικ στην επιλογή **Προσθήκη αναπαράσταση**, αρχίστε να πληκτρολογείτε το όνομα του τύπου περιεχομένου που θέλετε (π.χ. εφαρμογή/json) και, στη συνέχεια, επιλέξτε την στο αναπτυσσόμενο μενού προς τα κάτω.

![Σώμα τύπου περιεχομένου][api-management-response-body-content-type]

Επικολλήστε το παράδειγμα σώματος απόκρισης στην επιλεγμένη μορφή στο πλαίσιο κειμένου. 

![Σώμα απόκρισης][api-management-response-body]

Εάν θέλετε, προσθέστε μια προαιρετική περιγραφή στο πλαίσιο κειμένου **Περιγραφή** .

Όταν η λειτουργία έχει ρυθμιστεί, κάντε κλικ στην επιλογή **Αποθήκευση**.


## <a name="next-steps"> </a>Επόμενα βήματα

Αφού τις λειτουργίες προστίθενται σε ένα API, το επόμενο βήμα είναι να συσχετίσετε το API με ένα προϊόν και να δημοσιεύσετε έτσι ώστε να τους προγραμματιστές να καλέσετε τις λειτουργίες.

-   [Πώς μπορείτε να δημιουργήσετε και να δημοσιεύσετε ένα προϊόν][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Πώς μπορείτε να δημιουργήσετε και να δημοσιεύσετε ένα προϊόν]: api-management-howto-add-products.md
[Πώς να αποτελέσματα λειτουργίας cache Azure API διαχείρισης]: api-management-howto-cache.md
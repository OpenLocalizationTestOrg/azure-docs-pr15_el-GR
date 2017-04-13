<properties 
    pageTitle="Βασικές έννοιες API διαχείρισης" 
    description="Μάθετε περισσότερα σχετικά με APIs, προϊόντα, ρόλοι, ομάδες και άλλες βασικές έννοιες API διαχείρισης." 
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

# <a name="how-to-import-the-definition-of-an-api-with-operations-in-azure-api-management"></a>Πώς μπορείτε να εισαγάγετε τον ορισμό των API με τις λειτουργίες του Azure API διαχείρισης

API διαχείρισης, μπορούν να δημιουργηθούν νέα API και τις λειτουργίες που προστίθενται με μη αυτόματο τρόπο ή το API μπορούν να εισαχθούν μαζί με τις λειτουργίες σε ένα βήμα.

API και τις λειτουργίες μπορούν να εισαχθούν χρησιμοποιώντας τις παρακάτω μορφές.

-   WADL
-   Swagger

Αυτός ο οδηγός εμφανίζει πώς να δημιουργήσετε μια νέα API και εισαγάγετε τις εργασίες του σε ένα βήμα. Για πληροφορίες σχετικά με μη αυτόματο τρόπο τη δημιουργία API και προσθέτοντας λειτουργίες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε APIs][] και [πώς μπορείτε να προσθέσετε εργασίες για μια API][].

## <a name="import-api"> </a>Εισαγωγή API

API έχουν δημιουργηθεί και τη ρύθμιση των παραμέτρων στην πύλη του publisher. Για να αποκτήσετε πρόσβαση στην πύλη του publisher, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

![Πύλη του Publisher][api-management-management-console]

Κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **Εισαγωγή API**.

![Εισαγωγή API][api-management-import-apis]

Το παράθυρο **Εισαγωγής API** έχει τρεις καρτέλες που αντιστοιχούν στα τους τρεις τρόπους για την παροχή της προδιαγραφής API.

-   **Από το Πρόχειρο** σάς επιτρέπει να επικολλήσετε την προδιαγραφή API στο πλαίσιο κειμένου που έχει οριστεί ως.
-   **Από το αρχείο** σας επιτρέπει να αναζητήσετε και να επιλέξετε το αρχείο που περιέχει την προδιαγραφή API.
-   **Από το URL** σάς επιτρέπει να παρέχετε τη διεύθυνση URL για την προδιαγραφή για το API.

![Μορφή εισαγωγής API][api-management-import-api-clipboard]

Μετά την παροχή της προδιαγραφής API, χρησιμοποιήστε τα κουμπιά επιλογής στη δεξιά πλευρά για να υποδείξετε τη μορφή προδιαγραφής. Υποστηρίζονται οι παρακάτω μορφές.

-   WADL
-   Swagger

Στη συνέχεια, πληκτρολογήστε μια **διεύθυνση URL API Web επίθημα**. Αυτό είναι προσαρτημένο σε τη βασική διεύθυνση URL της υπηρεσίας διαχείρισης API. Η βάση διεύθυνση URL είναι κοινά για όλα τα API που φιλοξενούνται σε κάθε παρουσία μιας υπηρεσίας API διαχείρισης. API διαχείρισης ξεχωρίζει APIs από τους επίθημα και, επομένως, πρέπει να είναι μοναδικός για κάθε API σε μια συγκεκριμένη παρουσία της υπηρεσίας διαχείρισης API το επίθημα.

Αφού έχουν καταχωρηθεί όλες τις τιμές, κάντε κλικ στο κουμπί **Αποθήκευση** για να δημιουργήσετε το API και τις σχετικές εργασίες. 

>[AZURE.NOTE] Για ένα πρόγραμμα εκμάθησης της εισαγωγής βασική Αριθμομηχανή API σε μορφή Swagger, ανατρέξτε στο θέμα [Διαχείριση του πρώτου API Azure API διαχείρισης](api-management-get-started.md).

## <a name="export-api"></a> Εξαγωγή API

Εκτός από την εισαγωγή νέα API, μπορείτε να εξαγάγετε οι ορισμοί των API σας από την πύλη του publisher. Για να το κάνετε αυτό, κάντε κλικ στην επιλογή **Εξαγωγή API** από την **καρτέλα "Σύνοψη"** από το **API**.

![API εξαγωγής][api-management-export-api]

APIs μπορούν να εξαχθούν με τη χρήση WADL ή Swagger. Επιλέξτε την επιθυμητή μορφή, κάντε κλικ στην επιλογή **Αποθήκευση**και επιλέξτε τη θέση στην οποία θέλετε να αποθηκεύσετε το αρχείο.

![Μορφή API εξαγωγής][api-management-export-api-format]

## <a name="next-steps"> </a>Επόμενα βήματα

Όταν δημιουργείται ένα API και τις λειτουργίες που εισάγονται, μπορείτε να ελέγξετε και να ρυθμίσετε τις παραμέτρους τυχόν πρόσθετες ρυθμίσεις, προσθέστε το API για ένα προϊόν και δημοσιεύσετε έτσι ώστε να είναι διαθέσιμη για τους προγραμματιστές. Για περισσότερες πληροφορίες, ανατρέξτε στους παρακάτω οδηγούς.

-   [Πώς μπορείτε να ρυθμίσετε τις παραμέτρους API][]
-   [Πώς μπορείτε να δημιουργήσετε και να δημοσιεύσετε ένα προϊόν][]




[api-management-management-console]: ./media/api-management-howto-import-api/api-management-management-console.png
[api-management-import-apis]: ./media/api-management-howto-import-api/api-management-api-import-apis.png
[api-management-import-api-clipboard]: ./media/api-management-howto-import-api/api-management-import-api-wizard.png
[api-management-export-api]: ./media/api-management-howto-import-api/api-management-export-api.png
[api-management-export-api-format]: ./media/api-management-howto-import-api/api-management-export-api-format.png

[Import an API]: #import-api
[Export an API]: #export-api
[Configure API settings]: #configure-api-settings
[Next steps]: #next-steps

[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md
[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance

[Πώς μπορείτε να προσθέσετε εργασίες για μια API]: api-management-howto-add-operations.md
[Πώς μπορείτε να δημιουργήσετε και να δημοσιεύσετε ένα προϊόν]: api-management-howto-add-products.md
[Πώς μπορείτε να δημιουργήσετε APIs]: api-management-howto-create-apis.md
[Πώς μπορείτε να ρυθμίσετε τις παραμέτρους API]: api-management-howto-create-apis.md#configure-api-settings

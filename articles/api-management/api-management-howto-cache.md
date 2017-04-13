<properties
    pageTitle="Προσθήκη σε cache για βελτίωση της απόδοσης Azure API διαχείρισης | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να βελτιώσετε την λανθάνων χρόνος κατανάλωση εύρους ζώνης και φόρτωση υπηρεσίας web για το API διαχείρισης υπηρεσίας κλήσεις."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Προσθήκη σε cache για βελτίωση της απόδοσης Azure API διαχείρισης

Λειτουργίες API διαχείρισης μπορεί να ρυθμιστεί για προσωρινή αποθήκευση απόκρισης. Απάντηση σε cache να σημαντικά μείωση λανθάνων χρόνος API, κατανάλωση εύρους ζώνης, και web φόρτωσης υπηρεσίας για δεδομένα τα οποία δεν αλλάζουν συχνά.

Αυτός ο οδηγός δείχνει πώς μπορείτε να προσθέσετε απόκρισης σε cache για το API και ρύθμιση παραμέτρων πολιτικών για το δείγμα λειτουργίες API Αντήχηση. Στη συνέχεια, μπορείτε να καλέσετε τη λειτουργία από την πύλη για προγραμματιστές του για να επαληθεύσετε σε cache στην πράξη.

>[AZURE.NOTE] Για πληροφορίες σχετικά με την προσωρινή αποθήκευση στοιχείων με χρήση παραστάσεων πολιτικής αριθμού-κλειδιού, ανατρέξτε στο θέμα [Προσαρμογή εγγραφής στο cache του Azure API διαχείρισης](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

Πριν να ακολουθήσετε τα βήματα αυτού του οδηγού, πρέπει να έχετε μια παρουσία της υπηρεσίας διαχείρισης API με API και ρύθμιση των παραμέτρων ενός προϊόντος. Εάν δεν έχετε δημιουργήσει ακόμη μια παρουσία της υπηρεσίας διαχείρισης API, ανατρέξτε στο θέμα [Δημιουργία μια παρουσία της υπηρεσίας διαχείρισης API][] στην εκμάθηση [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][] .

## <a name="configure-caching"> </a>Ρύθμιση παραμέτρων μιας λειτουργίας για προσωρινή αποθήκευση

Σε αυτό το βήμα, θα το εξετάσετε τις ρυθμίσεις σε cache της λειτουργίας **ΛΉΨΗ πόρων (cached)** του δείγματος API Αντήχηση.

>[AZURE.NOTE] Κάθε παρουσία της υπηρεσίας διαχείρισης API διατίθεται προδιαμορφωμένο με API Αντήχηση που μπορούν να χρησιμοποιηθούν για να πειραματιστείτε με και ενημερωθείτε σχετικά με το API διαχείρισης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Azure API διαχείρισης][].

Για να ξεκινήσετε, κάντε κλικ στην επιλογή **Διαχείριση** στην πύλη του Azure κλασική της υπηρεσίας διαχείρισης API. Ενέργεια αυτή σας μεταφέρει στην πύλη του publisher API διαχείρισης.

![Πύλη του Publisher][api-management-management-console]

Κάντε κλικ στην επιλογή **APIs** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, κάντε κλικ στην επιλογή **API Αντήχηση**.

![API αντήχηση][api-management-echo-api]

Κάντε κλικ στην καρτέλα **εργασίες** και, στη συνέχεια, κάντε κλικ στη λειτουργία **ΛΉΨΗ πόρων (cached)** από τη λίστα **λειτουργιών** .

![Λειτουργίες API αντήχηση][api-management-echo-api-operations]

Κάντε κλικ στην καρτέλα **προσωρινή αποθήκευση** για να προβάλετε τις ρυθμίσεις σε cache για αυτήν τη λειτουργία.

![Καρτέλα σε cache][api-management-caching-tab]

Για να ενεργοποιήσετε την προσωρινή αποθήκευση για μια εργασία, επιλέξτε το πλαίσιο ελέγχου **Ενεργοποίηση** . Σε αυτό το παράδειγμα, η προσωρινή αποθήκευση είναι ενεργοποιημένη.

Κάθε ανταπόκρισης λειτουργίας έχει ρυθμιστεί, με βάση τις τιμές στα πεδία **ανάλογα με τις παραμέτρους συμβολοσειράς ερωτήματος** και **ανάλογα με την κεφαλίδες** . Εάν θέλετε να προσωρινή αποθήκευση πολλών απαντήσεων που βασίζεται σε παραμέτρους συμβολοσειράς ερωτήματος ή τις κεφαλίδες, μπορείτε να ρυθμίσετε τους σε αυτά τα δύο πεδία.

**Διάρκεια** Καθορίζει το χρονικό διάστημα λήξης των αποκρίσεων στο cache. Σε αυτό το παράδειγμα, το χρονικό διάστημα είναι **3600** δευτερόλεπτα, που είναι ισοδύναμη με μία ώρα.

Χρησιμοποιώντας τις ρυθμίσεις σε cache σε αυτό το παράδειγμα, την πρώτη αίτηση για τη λειτουργία **ΛΉΨΗ πόρων (cached)** επιστρέφει μια απάντηση από την υπηρεσία υποστήριξης. Αυτή η απόκριση θα αποθηκευτούν προσωρινά, με τον καθορισμένο κεφαλίδες και παραμέτρους συμβολοσειράς ερωτήματος. Οι επόμενες κλήσεις για τη λειτουργία, με αντίστοιχες παραμέτρους, θα έχετε την απάντηση στο cache επιστρέφονται, μέχρι να έχει λήξει το χρονικό διάστημα διάρκεια cache.

## <a name="caching-policies"> </a>Αναθεωρήσετε τις πολιτικές σε cache

Σε αυτό το βήμα, μπορείτε να αναθεωρήσετε τις ρυθμίσεις σε cache για τη λειτουργία **ΛΉΨΗ πόρων (cached)** του δείγματος API Αντήχηση.

Όταν έχουν ρυθμιστεί σε cache ρυθμίσεις για μια εργασία στην καρτέλα **προσωρινή αποθήκευση** , προστίθεται σε cache πολιτικές για τη λειτουργία. Οι πολιτικές αυτές είναι δυνατό να προβάλετε και να υποστεί επεξεργασία στο πρόγραμμα επεξεργασίας πολιτικής.

Κάντε κλικ στην επιλογή **πολιτικών** από το **API διαχείρισης** μενού στα αριστερά και, στη συνέχεια, επιλέξτε **API Αντήχηση / GET πόρων (cached)** από την αναπτυσσόμενη λίστα **λειτουργία** .

![Η λειτουργία εύρος πολιτικής][api-management-operation-dropdown]

Εμφανίζει τις πολιτικές για αυτήν τη λειτουργία στο πρόγραμμα επεξεργασίας πολιτικής.

![Πρόγραμμα επεξεργασίας πολιτικής διαχείρισης API][api-management-policy-editor]

Ο ορισμός πολιτικής για αυτήν τη λειτουργία περιλαμβάνει τις πολιτικές που ορίζουν τη ρύθμιση παραμέτρων σε cache που έχουν αναθεωρηθεί χρησιμοποιώντας την καρτέλα **προσωρινή αποθήκευση** στο προηγούμενο βήμα.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Αλλαγές που έγιναν στο πολιτικές σε cache του προγράμματος επεξεργασίας πολιτικής αντικατοπτρίζονται στην καρτέλα **προσωρινή αποθήκευση** μια λειτουργία και το αντίστροφο.

## <a name="test-operation"> </a>Κλήση μιας λειτουργίας και να ελέγξετε την προσωρινή αποθήκευση

Για να δείτε την προσωρινή αποθήκευση σε δράση, θα σας να καλέσετε τη λειτουργία από την πύλη για προγραμματιστές. Κάντε κλικ στην επιλογή **Προγραμματιστής πύλη** στο μενού επάνω δεξιά.

![Πύλη για προγραμματιστές][api-management-developer-portal-menu]

Κάντε κλικ στην επιλογή **APIs** στο μενού επάνω και, στη συνέχεια, επιλέξτε **API Αντήχηση**.

![API αντήχηση][api-management-apis-echo-api]

>Εάν έχετε μόνο μία API ρύθμιση παραμέτρων ή ορατό στο λογαριασμό σας, στη συνέχεια, κάνοντας κλικ στην επιλογή APIs μεταβαίνετε απευθείας τις λειτουργίες για το συγκεκριμένο API.

Επιλέξτε τη λειτουργία **ΛΉΨΗ πόρων (cached)** και, στη συνέχεια, κάντε κλικ στην επιλογή **Άνοιγμα κονσόλας**.

![Άνοιγμα κονσόλας][api-management-open-console]

Κονσόλα σάς επιτρέπει να καλέσετε λειτουργίες απευθείας από την πύλη για προγραμματιστές.

![Κονσόλα][api-management-console]

Διατηρήστε τις προεπιλεγμένες τιμές για **παράμετρος1** και **παράμετρος2**.

Επιλέξτε τον επιθυμητό αριθμό-κλειδί από την αναπτυσσόμενη λίστα **συνδρομή-κλειδί** . Εάν ο λογαριασμός σας έχει μόνο μία συνδρομή, που θα είναι ήδη επιλεγμένο.

Πληκτρολογήστε **sampleheader:value1** στο πλαίσιο κειμένου **αίτηση κεφαλίδες** .

Κάντε κλικ στην επιλογή **HTTP Get** και σημειώστε τις κεφαλίδες απόκρισης.

Πληκτρολογήστε **sampleheader:value2** στο πλαίσιο κειμένου **αίτηση για κεφαλίδες** και, στη συνέχεια, κάντε κλικ στην επιλογή **HTTP Get**.

Σημειώστε ότι η τιμή του **sampleheader** εξακολουθεί να είναι **τιμή1** στην απάντηση. Δοκιμάστε ορισμένες διαφορετικές τιμές και Σημειώστε ότι επιστρέφεται στο cache απάντηση από την πρώτη κλήση.

Εισαγάγετε **25** στο πεδίο **παράμετρος2** και, στη συνέχεια, κάντε κλικ στην επιλογή **HTTP Get**.

Σημειώστε ότι η τιμή του **sampleheader** στην απάντηση είναι τώρα **τιμή2**. Επειδή τα αποτελέσματα της λειτουργίας θα σχετίζονται με συμβολοσειρά ερωτήματος, την προηγούμενη απάντηση στο cache δεν επιστράφηκαν.

## <a name="next-steps"> </a>Επόμενα βήματα

-   Για περισσότερες πληροφορίες σχετικά με τις πολιτικές σε cache, ανατρέξτε στο θέμα [σε cache πολιτικές][] στην [αναφορά πολιτικής API διαχείρισης][].
-   Για πληροφορίες σχετικά με την προσωρινή αποθήκευση στοιχείων με χρήση παραστάσεων πολιτικής αριθμού-κλειδιού, ανατρέξτε στο θέμα [Προσαρμογή εγγραφής στο cache του Azure API διαχείρισης](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Γρήγορα αποτελέσματα με το Azure API διαχείρισης]: api-management-get-started.md

[Αναφορά πολιτικής διαχείρισης API]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Προσωρινή αποθήκευση πολιτικές]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Δημιουργήστε μια παρουσία της υπηρεσίας διαχείρισης API]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps

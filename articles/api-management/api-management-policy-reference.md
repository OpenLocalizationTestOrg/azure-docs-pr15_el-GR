<properties 
    pageTitle="Αναφορά πολιτικής διαχείρισης API του Azure" 
    description="Μάθετε περισσότερα σχετικά με τις πολιτικές που είναι διαθέσιμες για να ρυθμίσετε τις παραμέτρους API διαχείρισης." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Αναφορά πολιτικής διαχείρισης API του Azure

Αυτή η ενότητα παρέχει ένα ευρετήριο για τις πολιτικές στην [αναφορά πολιτικής API διαχείρισης][]. Για πληροφορίες σχετικά με την προσθήκη και ρύθμιση παραμέτρων πολιτικών, ανατρέξτε στο θέμα [πολιτικές API διαχείρισης][].

Πολιτική παραστάσεις μπορεί να χρησιμοποιηθεί ως χαρακτηριστικό τιμές ή τιμές κειμένου σε οποιαδήποτε από τις πολιτικές διαχείρισης API, εκτός εάν η πολιτική καθορίζει διαφορετικά. Ορισμένες πολιτικές όπως το [στοιχείο ελέγχου ροής][] και [Ρυθμίστε τη μεταβλητή][] πολιτικές βασίζονται σε παραστάσεις πολιτικής. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πολιτικές για προχωρημένους][] και [παραστάσεις πολιτικής][]

## <a name="policy-reference-index"></a>Πολιτική αναφοράς ευρετηρίου

-   [Πολιτικές ο περιορισμός πρόσβασης][]
    -   [Κεφαλίδα ελέγχου HTTP][] - επιβάλλει ύπαρξη ή/και τιμή κεφαλίδας HTTP.
    -   [Ρυθμός κλήσεων όριο από τη συνδρομή][] - χρήση API αποτρέπει φθάσει με περιορισμό Ρυθμός κλήσεων, με βάση ανά συνδρομή.
    -   [Ρυθμός κλήσεων όριο με αριθμό-κλειδί](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - χρήση API αποτρέπει φθάσει με περιορισμό Ρυθμός κλήσεων, με βάση ανά κλειδί.
    -   [Περιορισμός καλούντος διευθύνσεις IP][] - φίλτρα (επιτρέπει/αποτρέπει την) κλήσεις από συγκεκριμένες διευθύνσεις IP ή/και περιοχές διευθύνσεων.
    -   [Ορισμός ορίου χρήσης από τη συνδρομή][] - σάς επιτρέπει να επιβάλετε μια ανανεωθεί ή τη διάρκεια κλήσης ένταση ή/και το εύρος ζώνης ορίου, με βάση ανά συνδρομή.
    -   [Ορισμός ορίου χρήσης με αριθμό-κλειδί](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - σάς επιτρέπει να επιβάλετε μια ανανεωθεί ή τη διάρκεια κλήσης ένταση ή/και το εύρος ζώνης ορίου, με βάση ανά κλειδί.
    -   [Επικυρώστε JWT][] - επιβάλλει ύπαρξης και της ισχύος από ένα JWT που έχουν εξαχθεί από μια καθορισμένη κεφαλίδα HTTP ή ένα καθορισμένο ερώτημα παραμέτρων.
-   [Πολιτικές για προχωρημένους][]
    -   [Στοιχείο ελέγχου ροής][] - ισχύει υπό όρους δηλώσεις πολιτικής με βάση τα αποτελέσματα της αξιολόγησης του [παραστάσεις][]δυαδικών τιμών.
    -   [Προώθηση πρόσκλησης σε][] - προωθεί την αίτηση για την υπηρεσία υποστήριξης.
    -   [Αρχείο καταγραφής συμβάντων διανομέα][] - στέλνει μηνύματα στην καθορισμένη μορφή σε έναν προορισμό μηνύματος που ορίζονται από μια οντότητα [καταγραφής](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) .
    -   [Επανάληψη](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - επαναλήψεις εκτέλεσης των προτάσεων εσωκλείεται πολιτικής, εάν και μέχρι η συνθήκη. Εκτέλεση θα επαναλάβετε στο τα καθορισμένα χρονικά διαστήματα και έως και το καθορισμένο αριθμός επανάληψης.
    -   [Επιστροφή απόκριση](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - ματαίωση διοχέτευσης εκτέλεσης και επιστρέφει την καθορισμένη απάντηση απευθείας με τον καλούντα.
    -   [Αποστολή πρόσκλησης σε μονόδρομη](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - αποστέλλει μια αίτηση στην καθορισμένη διεύθυνση URL χωρίς αναμονή για απάντηση.
    -   [Αποστολή πρόσκλησης σε](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - αποστέλλει μια αίτηση στην καθορισμένη διεύθυνση URL.
    -   [Μέθοδος αίτησης set](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - σάς επιτρέπει να αλλάξετε τη μέθοδο HTTP για μια αίτηση.
    -   [Ορισμός κατάστασης](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - αλλάζει ο κωδικός κατάστασης HTTP στην καθορισμένη τιμή.
    -   [Ορισμός μεταβλητής][] - παραμένει μια τιμή σε μια καθορισμένη [περιβάλλοντος][] μεταβλητή για μελλοντική πρόσβαση.
    -   [Ανίχνευση](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - προσθέτει μια συμβολοσειρά σε το αποτέλεσμα του [API επιθεώρηση](../api-management/api-management-howto-api-inspector.md) .
    -   [Περιμένετε](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - αναμένει εσωκλείεται αίτηση αποστολής, τιμή λήψη από το cache ή πολιτικών του ελέγχου ροής για να ολοκληρωθεί πριν να συνεχίσετε.
-   [Πολιτικές ελέγχου ταυτότητας][]
    -   [Έλεγχος ταυτότητας με Basic][] - έλεγχος ταυτότητας με μια υπηρεσία παρασκηνίου χρησιμοποιώντας βασικό έλεγχο ταυτότητας.
    -   [Έλεγχος ταυτότητας με πιστοποιητικό προγράμματος-πελάτη][] - έλεγχος ταυτότητας με μια υπηρεσία παρασκηνίου χρησιμοποιώντας πιστοποιητικά προγράμματος-πελάτη.
-   [Προσωρινή αποθήκευση πολιτικές][] 
    -   [Λήψη από το cache][] - εκτελέστε cache αναζητήσετε και να επιστρέψετε μια έγκυρη απάντηση στο cache όταν είναι διαθέσιμες.
    -   [Αποθήκευση σε cache][] - απόκρισης μνήμης cache σύμφωνα με τη ρύθμιση παραμέτρων cache καθορισμένο στοιχείο ελέγχου.
    -   [Λήψη της τιμής από το cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - ανάκτηση ενός στοιχείου στο cache με αριθμό-κλειδί.
    -   [Τιμή χώρο αποθήκευσης στη μνήμη cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Store ένα στοιχείο στο cache με αριθμό-κλειδί.
    -   [Κατάργηση τιμή από το cache](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - κατάργηση ένα στοιχείο στο cache με αριθμό-κλειδί.
-   [Cross πολιτικές τομέα][] 
    -   [Να επιτρέπονται οι κλήσεις μεταξύ τομέων][] - κάνει το API προσβάσιμο από το Adobe Flash και Microsoft Silverlight πελάτες με το πρόγραμμα περιήγησης.
    -   [CORS][] - προσθέτει σταυρό origin πόρο κοινής χρήσης υποστήριξης (CORS) σε μια λειτουργία ή API για να επιτρέψετε τις κλήσεις μεταξύ τομέων από προγράμματα-πελάτες που βασίζονται σε πρόγραμμα περιήγησης.
    -   [JSONP][] - προσθέτει JSON με την υποστήριξη αναπλήρωσης (JSONP) σε μια εργασία ή μια API για να επιτρέψετε τις κλήσεις μεταξύ τομέων από προγράμματα-πελάτες με το πρόγραμμα περιήγησης JavaScript.
-   [Πολιτικές μετασχηματισμό][] 
    -   [Μετατροπή JSON σε XML][] - απόκριση ή αίτηση μετατρέπει το σώμα από JSON σε XML.
    -   [Μετατροπή XML σε JSON][] - απόκριση ή αίτηση μετατρέπει το σώμα από XML σε JSON.
    -   [Εύρεση και αντικατάσταση συμβολοσειρά στο σώμα][] - εντοπίζει μια δευτερεύουσα αίτησης ή απόκρισης και αντικαθιστά με μια διαφορετική δευτερεύουσα.
    -   [Διευθύνσεις URL μάσκας στο περιεχόμενο][] - κάνει επανεγγραφή (μάσκες) συνδέσεις στην απάντηση σώμα έτσι ώστε να οδηγούν στην ισοδύναμη σύνδεση μέσω της πύλης.
    -   [Ρύθμιση υπηρεσίας υποστήριξης][] - αλλάζει την υπηρεσία υποστήριξης για μια εισερχόμενη αίτηση.
    -   [Ορισμός σώμα][] - ορίζει το σώμα του μηνύματος για αιτήσεις εισερχόμενων και εξερχόμενων.
    -   [Κεφαλίδα Ορισμός HTTP][] - αναθέτει μια τιμή σε μια υπάρχουσα απόκριση ή/και κεφαλίδα αίτησης ή προσθέτει μια νέα επικεφαλίδα απάντηση ή/και αίτηση.
    -   [Ορισμός παραμέτρου συμβολοσειράς ερωτήματος][] - προσθέτει, τιμή αντικαθιστά ή διαγράφει αίτηση παραμέτρου συμβολοσειράς ερωτήματος.
    -   [Επανεγγραφή διεύθυνσης URL][] - μετατρέπει μια πρόσκληση σε διεύθυνση URL από τη δημόσια φόρμα στη φόρμα αναμένεται από την υπηρεσία web.

## <a name="next-steps"></a>Επόμενα βήματα

Για περισσότερες πληροφορίες σχετικά με την πολιτική παραστάσεων, ανατρέξτε στο θέμα βίντεο που ακολουθεί.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Πολιτικές ο περιορισμός πρόσβασης]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Έλεγχος κεφαλίδα HTTP]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Ρυθμός κλήσεων όριο από τη συνδρομή]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Περιορισμός καλούντος διευθύνσεις IP]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Ορισμός ορίου χρήσης από τη συνδρομή]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Επικύρωση JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Πολιτικές για προχωρημένους]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Στοιχείο ελέγχου ροής]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Ορισμός μεταβλητής]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[παραστάσεις]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[περιβάλλον]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Προώθηση πρόσκλησης σε]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Αρχείο καταγραφής συμβάντων διανομέα]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Πολιτικές ελέγχου ταυτότητας]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Ο έλεγχος ταυτότητας με Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Ο έλεγχος ταυτότητας με πιστοποιητικό προγράμματος-πελάτη]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Προσωρινή αποθήκευση πολιτικές]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Λήψη από το cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Αποθήκευση σε cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross πολιτικές τομέα]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Επιτρέπει κλήσεις μεταξύ τομέων]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Πολιτικές μετασχηματισμό]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Μετατροπή JSON σε XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Μετατροπή XML σε JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Εύρεση και αντικατάσταση συμβολοσειρά στο κυρίως κείμενο]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Διευθύνσεις URL μάσκας στο περιεχόμενο]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Ορισμός παρασκηνίου υπηρεσίας]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Ορισμός σώμα]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Κεφαλίδα HTTP σύνολο]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Ορισμός παραμέτρου συμβολοσειράς ερωτήματος]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Επανεγγραφή διεύθυνσης URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Πολιτικές διαχείρισης API]: api-management-howto-policies.md
[Αναφορά πολιτικής διαχείρισης API]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Πολιτική παραστάσεις]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 

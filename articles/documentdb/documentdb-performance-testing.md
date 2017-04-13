<properties 
    pageTitle="DocumentDB κλίμακα και έλεγχος απόδοσης | Microsoft Azure" 
    description="Μάθετε πώς να εκτελείτε κλίμακα και επιδόσεων δοκιμών με Azure DocumentDB"
    keywords="δοκιμή επιδόσεων"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Επιδόσεις και την κλίμακα δοκιμών με Azure DocumentDB
Επιδόσεις και την κλίμακα δοκιμές είναι ένα πλήκτρο βήμα στην ανάπτυξη εφαρμογών. Για πολλές εφαρμογές, το επίπεδο βάσης δεδομένων έχει σοβαρές συνέπειες στη συνολική απόδοση και κλιμάκωση και, επομένως, είναι κρίσιμης στοιχείο δοκιμές επιδόσεων. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) είναι κατασκευασμένο επί τούτου για ελαστικά κλίμακα και προβλέψιμα απόδοση και, επομένως, εξαιρετικός για εφαρμογές που χρειάζεστε ένα επίπεδο υψηλών επιδόσεων βάσης δεδομένων. 

Σε αυτό το άρθρο είναι μια αναφορά για τους προγραμματιστές εφαρμογής οικογενειών προγραμμάτων δοκιμών επιδόσεων για τους φόρτους εργασίας DocumentDB ή την αξιολόγηση DocumentDB για σενάρια υψηλών επιδόσεων εφαρμογών. Εστιάζει κυρίως σε δοκιμή απομόνωσης απόδοσης της βάσης δεδομένων, αλλά περιλαμβάνει επίσης βέλτιστες πρακτικές για την παραγωγή εφαρμογές.

Μετά την ανάγνωση αυτό το άρθρο, θα μπορούν να απαντούν στα παρακάτω ερωτήματα:   

- Πού μπορώ να βρω μια εφαρμογή προγράμματος-πελάτη .NET δείγμα για σκοπούς δοκιμής επιδόσεων του Azure DocumentDB; 
- Πώς μπορώ να επιτύχετε επίπεδα υψηλής απόδοσης με Azure DocumentDB από εφαρμογής του προγράμματος-πελάτη;

Για να ξεκινήσετε με κώδικα, κάντε λήψη του έργου από το [Δείγμα δοκιμές DocumentDB επιδόσεων](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [AZURE.NOTE] Είναι ο στόχος αυτής της εφαρμογής για μια επίδειξη βέλτιστες πρακτικές για την εξαγωγή καλύτερες επιδόσεις από DocumentDB με ένα μικρό αριθμό των υπολογιστών-πελατών. Αυτό δεν είχε γίνει για μια επίδειξη η δυναμικότητα κορυφής από την υπηρεσία, η οποία μπορεί να κλιμακωθεί limitlessly.

Εάν αναζητάτε επιλογές ρύθμισης παραμέτρων πλευρά του προγράμματος-πελάτη για βελτίωση της απόδοσης DocumentDB, ανατρέξτε στο θέμα [συμβουλές επιδόσεων DocumentDB](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Εκτελέστε τις επιδόσεις δοκιμή εφαρμογής
Είναι ο πιο γρήγορος τρόπος για να ξεκινήσετε να μεταγλωττίσετε και να εκτελέσετε το δείγμα .NET παρακάτω, όπως περιγράφεται στα παρακάτω βήματα. Μπορείτε επίσης να ελέγξετε τον πηγαίο κώδικα και να υλοποιήσετε παρόμοια ρυθμίσεις παραμέτρων για τις δικές σας εφαρμογές προγράμματος-πελάτη.

**Βήμα 1:** Κάντε λήψη του έργου από το [Δείγμα δοκιμές επιδόσεων DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)ή διαχωρίζονται του αποθετηρίου Github.

**Βήμα 2:** Τροποποιήστε τις ρυθμίσεις για EndpointUrl, AuthorizationKey, CollectionThroughput και DocumentTemplate (προαιρετικό) στο App.config.

> [AZURE.NOTE] Πριν από την προμήθεια συλλογές με υψηλή μετάδοσης, ανατρέξτε στη [Σελίδα τις τιμές](https://azure.microsoft.com/pricing/details/documentdb/) για να εκτιμήσετε το κόστος ανά συλλογή. Χώρος αποθήκευσης χρεώσεις DocumentDB και απόδοση ανεξάρτητα σε ωριαία βάση, ώστε να μπορείτε να αποθηκεύσετε κόστους, διαγράφοντας ή να μειώσετε την ταχύτητα των συλλογών σας DocumentDB μετά τη δοκιμή.

**Βήμα 3:** Μεταγλώττιση και εκτελέστε την εφαρμογή κονσόλας από τη γραμμή εντολών. Θα πρέπει να βλέπετε εξόδου όπως τα εξής:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Βήμα 4 (εάν χρειάζεται):** Η ταχύτητα μεταγωγής αναφερθεί (RU/s) από το εργαλείο θα πρέπει να είναι η ίδια ή υψηλότερα από την προμήθεια του φακέλου μετάδοσης της συλλογής. Εάν όχι, αυξάνοντας το DegreeOfParallelism σε μικρά διαστήματα μπορεί να σας βοηθήσουν να λάβετε το όριο. Εάν plateaus τη μετάδοση από την εφαρμογή υπολογιστή-πελάτη, εάν πραγματοποιηθεί εκκίνηση πολλών παρουσιών της εφαρμογής σε υπολογιστές ίδιες ή σε διαφορετικές θα σας βοηθήσει να φτάσετε το όριο προμήθεια του φακέλου σε διαφορετικές παρουσίες του. Εάν χρειάζεστε βοήθεια σχετικά με αυτό το βήμα, πληκτρολογήστε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να askdocdb@microsoft.com ή συμπληρώσετε ένα δελτίο υποστήριξης.

Μόλις η εφαρμογή εκτελείται, μπορείτε να δοκιμάσετε διαφορετικές [πολιτικές δημιουργίας ευρετηρίου](documentdb-indexing-policies.md) και [επίπεδα συνέπειας](documentdb-consistency-levels.md) για να κατανοήσετε τις επιπτώσεις στην απόδοση και λανθάνοντος χρόνου. Επίσης, μπορείτε να ελέγξετε τον πηγαίο κώδικα και να υλοποιήσετε παρόμοια ρυθμίσεις παραμέτρων για να τις δικές σας δοκιμής οικογένειες προγραμμάτων ή παραγωγής εφαρμογές.

## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτό το άρθρο θα σας βλέπατε πώς μπορείτε να εκτελέσετε τις επιδόσεις και την κλίμακα δοκιμών με χρήση εφαρμογής κονσόλας .NET DocumentDB. Ανατρέξτε τις παρακάτω συνδέσεις για επιπλέον πληροφορίες σχετικά με την εργασία με DocumentDB.

* [Δείγμα δοκιμής επιδόσεων DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Επιλογές ρύθμισης παραμέτρων προγράμματος-πελάτη για βελτίωση της απόδοσης DocumentDB](documentdb-performance-tips.md)
* [Πλευρά του διακομιστή διαμερισμάτων στο DocumentDB](documentdb-partition-data.md)
* [Συλλογές DocumentDB και επιδόσεων](documentdb-performance-levels.md)
* [Τεκμηρίωση DocumentDB .NET SDK στο MSDN](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [Δείγματα DocumentDB .NET](https://github.com/Azure/azure-documentdb-net)
* [Ιστολόγιο DocumentDB σε συμβουλές επιδόσεων](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)

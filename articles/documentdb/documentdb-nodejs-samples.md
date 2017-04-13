<properties
    pageTitle="Παραδείγματα NoSQL Node.js για DocumentDB | Microsoft Azure"
    description="Βρείτε NoSQL Node.js παραδείγματα στην github για συνήθεις εργασίες στο DocumentDB, συμπεριλαμβανομένων των λειτουργίες CRUD για έγγραφα JSON σε βάσεις δεδομένων NoSQL."
    keywords="Παραδείγματα node.js"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>Παραδείγματα DocumentDB Node.js

> [AZURE.SELECTOR]
- [Παραδείγματα .NET](documentdb-dotnet-samples.md)
- [Παραδείγματα node.js](documentdb-nodejs-samples.md)
- [Παραδείγματα Python](documentdb-python-samples.md)
- [Συλλογή δείγματα κώδικα Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Δείγμα λύσεις που εκτελούν λειτουργίες CRUD και άλλες κοινές λειτουργίες σε πόρους Azure DocumentDB περιλαμβάνονται στο αποθετήριο GitHub [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) . Σε αυτό το άρθρο παρέχει:

- Συνδέσεις για τις εργασίες σε κάθε ένα από τα αρχεία Node.js παράδειγμα του έργου.
- Συνδέσεις προς το σχετικό API αναφοράς περιεχόμενο.

**Προαπαιτούμενα στοιχεία**

1. Χρειάζεστε ένα λογαριασμό Azure για να χρησιμοποιήσετε αυτά τα παραδείγματα Node.js:
    - Μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](https://azure.microsoft.com/pricing/free-trial/): λάβετε πιστώσεων μπορείτε να χρησιμοποιήσετε για να δοκιμάσετε την πληρωμή Azure υπηρεσιών και ακόμα και αν χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και χρήση δωρεάν Azure υπηρεσίες, όπως τοποθεσίες Web που διαθέτετε. Η πιστωτική κάρτα σας θα χρεωθεί ποτέ, εκτός εάν αλλάξετε τις ρυθμίσεις σας και ζητήστε του να χρεωθεί ρητά.
   - Μπορείτε να [ενεργοποιήσετε το Visual Studio συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): τη συνδρομή σας Visual Studio παρέχει πιστώσεων κάθε μήνα που μπορείτε να χρησιμοποιήσετε για τις υπηρεσίες του Azure επί πληρωμή.
2. Χρειάζεστε επίσης το [Node.js SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Κάθε δείγμα είναι αυτόνομο, το ίδιο ρυθμίζει και εκκαθαρίζει μετά την ίδια. Ως εκ τούτου, τα δείγματα θεμάτων πολλών κλήσεων στο [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Κάθε φορά, αυτό γίνεται η συνδρομή σας θα χρεωθείτε για 1 ώρα χρήσης ανά το επίπεδο απόδοσης της συλλογής που δημιουργούνται.

## <a name="database-examples"></a>Παραδείγματα βάσης δεδομένων

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) του έργου [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργία βάσης δεδομένων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Ερώτημα ένα λογαριασμό για μια βάση δεδομένων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Διαβάστε μια βάση δεδομένων μέσω του αναγνωριστικού](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Λίστα βάσεις δεδομένων για ένα λογαριασμό](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Διαγραφή βάσης δεδομένων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Παραδείγματα συλλογής

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) του έργου [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργία συλλογής](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Διαβάστε μια λίστα με όλες τις συλλογές σε μια βάση δεδομένων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Λάβετε μια συλλογή από _self:](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Λάβετε μια συλλογή κατά αναγνωριστικό](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Λήψη επίπεδο επιδόσεων μιας συλλογής](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Αλλαγή επιπέδων επιδόσεων μιας συλλογής](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Διαγραφή συλλογής](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Παραδείγματα εγγράφου

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) του έργου [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργία εγγράφων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Διαβάστε την τροφοδοσία για μια συλλογή εγγράφων](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Ανάγνωση εγγράφου κατά Αναγνωριστικό](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Ανάγνωση εγγράφων μόνο εάν το έγγραφο έχει αλλάξει](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Ερώτημα για έγγραφα](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Αντικατάσταση ενός εγγράφου](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Αντικατάσταση εγγράφου με υπό όρους, επιλέξτε ETag](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Διαγραφή εγγράφου](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Παραδείγματα δημιουργίας ευρετηρίου

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) του έργου [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
Δημιουργία συλλογής με προεπιλεγμένη δημιουργίας ευρετηρίου | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Ευρετηρίου με μη αυτόματο τρόπο ένα συγκεκριμένο έγγραφο](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: 'include'](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Αποκλεισμός με μη αυτόματο τρόπο ένα συγκεκριμένο έγγραφο από το ευρετήριο](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Χρήση τεμπέλη δημιουργίας ευρετηρίου για μαζικής εισαγωγής ή να διαβάσουν έντονη συλλογές](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Συμπεριλάβετε συγκεκριμένες διαδρομές ενός εγγράφου στο ευρετήριο](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Εξαίρεση ορισμένων διαδρομές από τη δημιουργία ευρετηρίου](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Να επιτρέπεται η σάρωση στο μια διαδρομή συμβολοσειρά στη διάρκεια μιας περιοχής](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Δημιουργία ευρετηρίου περιοχής σε μια διαδρομή συμβολοσειράς](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Δημιουργία συλλογής με indexPolicy προεπιλεγμένη και, στη συνέχεια, ενημερώστε αυτό online](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία ευρετηρίου, ανατρέξτε στο θέμα [DocumentDB πολιτικές δημιουργίας ευρετηρίου](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Παραδείγματα προγραμματισμού πλευρά του διακομιστή

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) του έργου [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργήστε μια αποθηκευμένη διαδικασία](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Εκτέλεση μιας αποθηκευμένης διαδικασίας](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Για περισσότερες πληροφορίες σχετικά με την πλευρά του διακομιστή προγραμματισμού, ανατρέξτε στο θέμα [προγραμματισμού διακομιστή DocumentDB: αποθηκευμένες διαδικασίες, εναύσματα βάσης δεδομένων και UDF](documentdb-programming.md).

## <a name="partitioning-examples"></a>Παραδείγματα διαμερισμού

Το αρχείο [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) του έργου [διαμέριση](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Χρησιμοποιήστε ένα HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία διαμερισμάτων δεδομένων σε DocumentDB, ανατρέξτε στο θέμα [διαμερίσματα και την κλίμακα των δεδομένων σε DocumentDB](documentdb-partition-data.md).

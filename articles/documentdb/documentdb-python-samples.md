<properties 
    pageTitle="Παραδείγματα NoSQL Python για DocumentDB | Microsoft Azure" 
    description="Βρείτε NoSQL Python παραδείγματα στην github για συνήθεις εργασίες στο DocumentDB, συμπεριλαμβανομένων των λειτουργίες CRUD για έγγραφα JSON σε βάσεις δεδομένων NoSQL." 
    keywords="Παραδείγματα Python"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Παραδείγματα DocumentDB Python

> [AZURE.SELECTOR]
- [Παραδείγματα .NET](documentdb-dotnet-samples.md)
- [Παραδείγματα node.js](documentdb-nodejs-samples.md)
- [Παραδείγματα Python](documentdb-python-samples.md)
- [Συλλογή δείγματα κώδικα Azure](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Δείγμα λύσεις που εκτελούν λειτουργίες CRUD και άλλες κοινές λειτουργίες σε πόρους Azure DocumentDB περιλαμβάνονται στο αποθετήριο GitHub [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) . Σε αυτό το άρθρο παρέχει:

- Συνδέσεις για τις εργασίες σε κάθε ένα από τα αρχεία Python παράδειγμα του έργου. 
- Συνδέσεις προς το σχετικό API αναφοράς περιεχόμενο.

**Προαπαιτούμενα στοιχεία**

1. Χρειάζεστε ένα λογαριασμό Azure για να χρησιμοποιήσετε αυτά τα παραδείγματα Python:
    - Μπορείτε να [ανοίξετε ένα λογαριασμό Azure δωρεάν](https://azure.microsoft.com/pricing/free-trial/): λάβετε πιστώσεων μπορείτε να χρησιμοποιήσετε για να δοκιμάσετε την πληρωμή Azure υπηρεσιών και ακόμα και αν χρησιμοποιούνται προς τα επάνω, μπορείτε να διατηρήσετε το λογαριασμό και χρήση δωρεάν Azure υπηρεσίες, όπως τοποθεσίες Web που διαθέτετε. Η πιστωτική κάρτα σας θα χρεωθεί ποτέ, εκτός εάν αλλάξετε τις ρυθμίσεις σας και ζητήστε του να χρεωθεί ρητά.
   - Μπορείτε να [ενεργοποιήσετε το Visual Studio συνδρομητών πλεονεκτήματα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): τη συνδρομή σας Visual Studio παρέχει πιστώσεων κάθε μήνα που μπορείτε να χρησιμοποιήσετε για τις υπηρεσίες του Azure επί πληρωμή.
2. Χρειάζεστε επίσης το [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Κάθε δείγμα είναι αυτόνομο, το ίδιο ρυθμίζει και εκκαθαρίζει μετά την ίδια. Ως εκ τούτου, τα δείγματα θεμάτων πολλές κλήσεις [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Κάθε φορά, αυτό γίνεται η συνδρομή σας θα χρεωθείτε για 1 ώρα χρήσης ανά το επίπεδο απόδοσης της συλλογής που δημιουργούνται. 

## <a name="database-examples"></a>Παραδείγματα βάσης δεδομένων

Το αρχείο [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) του έργου [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργία βάσης δεδομένων](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Ερώτημα ένα λογαριασμό για μια βάση δεδομένων](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Διαβάστε μια βάση δεδομένων μέσω του αναγνωριστικού](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Λίστα βάσεις δεδομένων για ένα λογαριασμό](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Διαγραφή βάσης δεδομένων](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Παραδείγματα συλλογής 

Το αρχείο [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) του έργου [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) δείχνει πώς μπορείτε να εκτελέσετε τις ακόλουθες εργασίες.

Εργασία | Αναφορά API
--- | ---
[Δημιουργία συλλογής](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Διαβάστε μια λίστα με όλες τις συλλογές σε μια βάση δεδομένων](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Λάβετε μια συλλογή κατά αναγνωριστικό](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Λήψη επίπεδο επιδόσεων μιας συλλογής](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Αλλαγή επιπέδων επιδόσεων μιας συλλογής](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Διαγραφή συλλογής](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)

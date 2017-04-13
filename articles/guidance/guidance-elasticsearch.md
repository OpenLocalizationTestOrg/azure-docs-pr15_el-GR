
<properties
   pageTitle="Elasticsearch Azure οδηγίες | Microsoft Azure"
   description="Elasticsearch Azure οδηγίες."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch Azure οδηγίες 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch είναι μια μηχανή αναζήτησης ιδιαίτερα μεταβλητού μεγέθους ανοιχτού κώδικα και βάση δεδομένων. Είναι χρήσιμο για περιπτώσεις που απαιτούν γρήγορη ανάλυση και εντοπισμού των πληροφοριών που θα διατηρούνται στα μεγάλα σύνολα δεδομένων. Αυτές οι οδηγίες εξετάζει ορισμένες βασικές πτυχές πρέπει να λάβετε υπόψη κατά τη σχεδίαση μιας σύμπλεγμα Elasticsearch, με εστίαση σχετικά με τους τρόπους για να βελτιστοποιήσετε και δοκιμή της ρύθμισης παραμέτρων.

> [AZURE.NOTE]Αυτές οι οδηγίες προϋποθέτει ορισμένες βασικές εξοικείωση με Elasticsearch. Για πιο λεπτομερείς πληροφορίες, επισκεφθείτε την [τοποθεσία Web Elasticsearch](https://www.elastic.co/products/elasticsearch). 

- **[Εκτέλεση Elasticsearch στην Azure][]** παρέχει μια σύντομη εισαγωγή στις η γενική δομή των Elasticsearch και περιγράφει πώς μπορείτε να υλοποιήσετε ένα σύμπλεγμα Elasticsearch χρησιμοποιώντας Azure. 
- **[Απόδοση κατάποσης δεδομένων ρύθμισης για Elasticsearch σε Azure][]** περιγράφει την ανάπτυξη του ένα σύμπλεγμα Elasticsearch και παρέχει εις βάθος ανάλυση από τις διάφορες επιλογές ρύθμισης παραμέτρων πρέπει να λάβετε υπόψη όταν αναμένετε μεγάλη ταχύτητα κατάποσης δεδομένων.
- **[Συγκέντρωση δεδομένων ρύθμισης και επιδόσεις ερωτημάτων για Elasticsearch σε Azure][]** παρέχει μια εις βάθος ανάλυση από τις επιλογές για να λάβετε υπόψη όταν αποφασίζετε για τον τρόπο για να βελτιστοποιήσετε το σύστημά σας για τις επιδόσεις ερωτημάτων και αναζήτησης.
- **[Ρύθμιση παραμέτρων ανθεκτικότητα και αποκατάστασης σε Elasticsearch σε Azure][]** περιγράφει ορισμένες σημαντικές δυνατότητες του ένα σύμπλεγμα Elasticsearch που μπορούν να σας βοηθήσουν να ελαχιστοποιήσετε τις πιθανότητες απώλειας δεδομένων και ώρες αποκατάστασης εκτεταμένο δεδομένων.
- **[Δημιουργία σε περιβάλλον δοκιμής επιδόσεων για Elasticsearch σε Azure][]** περιγράφει πώς μπορείτε να ρυθμίσετε ένα περιβάλλον για τον έλεγχο του κατάποσης δεδομένων επιδόσεων και φόρτους εργασίας του ερωτήματος σε ένα σύμπλεγμα Elasticsearch. 
- **[Εφαρμογή ενός JMeter δοκιμή πρόγραμμα για Elasticsearch][]** συνοψίζει εκτελείται δοκιμές επιδόσεων υλοποιηθεί με τη χρήση σχεδίων δοκιμής JMeter μαζί με τον κώδικα Java ενσωματώνονται δοκιμή JUnit για την εκτέλεση εργασιών, όπως η αποστολή δεδομένων σε σύμπλεγμα Elasticsearch.
- **[Ανάπτυξη ένα δείγμα JMeter JUnit για σκοπούς δοκιμής επιδόσεων Elasticsearch][]** περιγράφει πώς μπορείτε να δημιουργήσετε και να χρησιμοποιήσετε ένα δείγμα JUnit που μπορούν να δημιουργούν και αποστολή δεδομένων σε ένα σύμπλεγμα Elasticsearch. Αυτή η δυνατότητα παρέχει μια εξαιρετικά ευέλικτη προσέγγιση για τη φόρτωση δοκιμών που μπορούν να δημιουργούν μεγάλες ποσότητες δεδομένων δοκιμής χωρίς ανάλογα με τα αρχεία εξωτερικών δεδομένων. 
- **[Εκτέλεση δοκιμών ανοχή Elasticsearch αυτοματοποιημένη][]** συνοψίζει τον τρόπο για να εκτελέσετε τις δοκιμές ανοχή που χρησιμοποιούνται σε αυτήν τη σειρά. 
- **[Εκτελεί την αυτοματοποιημένη δοκιμές επιδόσεων Elasticsearch][]** συνοψίζει τον τρόπο για να εκτελέσετε τις δοκιμές επιδόσεων που χρησιμοποιούνται σε αυτήν τη σειρά.


[Εκτέλεση Elasticsearch σε Azure]: guidance-elasticsearch-running-on-azure.md
[Ρύθμιση επιδόσεων κατάποσης δεδομένων για Elasticsearch σε Azure]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[Δημιουργία μιας επιδόσεις περιβάλλοντος δοκιμής για Elasticsearch σε Azure]: guidance-elasticsearch-creating-performance-testing-environment.md
[Εφαρμογή ένα σχέδιο δοκιμών JMeter για Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[Ανάπτυξη ένα δείγμα JMeter JUnit για σκοπούς δοκιμής Elasticsearch επιδόσεων]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[Ρύθμιση δεδομένων συνάθροισης και επιδόσεις ερωτημάτων για Elasticsearch στο Azure]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[Ρύθμιση παραμέτρων του ανθεκτικότητα και αποκατάστασης σε Elasticsearch στο Azure]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[Εκτέλεση δοκιμών ανοχή αυτοματοποιημένη Elasticsearch]: guidance-elasticsearch-running-automated-resilience-tests.md
[Εκτέλεση δοκιμών επιδόσεων αυτοματοποιημένη Elasticsearch]: guidance-elasticsearch-running-automated-performance-tests.md

<properties
    pageTitle="Azure τεκμηρίωση για δημόσιους οργανισμούς | Microsoft Azure"
    description="Αυτό παρέχει ένα όσον αφορά τα των δυνατοτήτων και καθοδήγηση στην ανάπτυξη εφαρμογών για δημόσιους οργανισμούς Azure"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Βάσεις δεδομένων Azure δημόσιους οργανισμούς

##  <a name="sql-database"></a>Βάση δεδομένων SQL

Ανατρέξτε στο<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Κέντρο ασφάλειας της Microsoft για το μηχανισμό βάσεων δεδομένων SQL</a> και [Δημόσια τεκμηρίωση βάσης δεδομένων SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/) για πρόσθετες οδηγίες για τη ρύθμιση παραμέτρων ορατότητα μετα-δεδομένων και βέλτιστες πρακτικές προστασίας.

### <a name="variations"></a>Παραλλαγές

Βάση δεδομένων SQL του V12 είναι γενικά διαθέσιμο σε δημόσιους οργανισμούς Azure.

Η διεύθυνση για τους διακομιστές Azure SQL σε δημόσιους οργανισμούς Azure είναι διαφορετική:

Τύπος υπηρεσίας|Δημόσια Azure|Azure δημόσιους οργανισμούς
---|---|---
Βάση δεδομένων SQL|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Ζητήματα

Οι ακόλουθες πληροφορίες προσδιορίζει το όριο για δημόσιους οργανισμούς Azure για Azure SQL:

| Οργανωμένη/δεδομένα που ελέγχονται από επιτρέπεται | Οργανωμένη/ελέγχονται δεδομένων δεν επιτρέπεται |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Όλα τα δεδομένα αποθηκεύονται και υποβάλλονται σε επεξεργασία στο Microsoft Azure SQL μπορεί να περιέχει ρυθμίζεται για δημόσιους οργανισμούς Azure δεδομένων. Πρέπει να χρησιμοποιήσετε εργαλεία βάσης δεδομένων για τη μεταφορά δεδομένων ρυθμίζεται για δημόσιους οργανισμούς Azure δεδομένων. | Μετα-δεδομένα Azure SQL δεν επιτρέπεται να περιέχει ελέγχονται εξαγωγή δεδομένων. Αυτό μετα-δεδομένων περιλαμβάνει όλα τα δεδομένα ρύθμισης παραμέτρων που εισάγονται κατά τη δημιουργία και τη διατήρηση του προϊόντος χώρου αποθήκευσης.  Δεν καταχωρείτε δεδομένα οργανωμένη/ελέγχονται στα ακόλουθα πεδία: όνομα, όνομα της συνδρομής, ομάδες πόρων, το όνομα του διακομιστή, σύνδεση σε διακομιστή διαχείρισης, ονόματα ανάπτυξης, ονόματα πόρων, ετικέτες πόρων της βάσης δεδομένων

## <a name="azure-redis-cache"></a>Azure Redis Cache

Για λεπτομέρειες σχετικά με αυτήν την υπηρεσία και πώς μπορείτε να χρησιμοποιήσετε, ανατρέξτε [στην τεκμηρίωση δημόσια Azure Redis Cache](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Παραλλαγές

Οι διευθύνσεις URL για την πρόσβαση και διαχείριση Cache Redis Azure σε δημόσιους οργανισμούς Azure διαφέρουν:

Τύπος υπηρεσίας|Δημόσια Azure|Azure δημόσιους οργανισμούς
---|---|---
Τελικό σημείο cache|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Πύλη του Azure|https://Portal.Azure.com|https://Portal.Azure.US

>[AZURE.NOTE] Όλες τις δέσμες ενεργειών και κώδικα πρέπει να λογαριασμού για το κατάλληλο τελικά σημεία και περιβάλλοντα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [πώς μπορείτε να συνδεθείτε στο Cloud για δημόσιους οργανισμούς Azure](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Ζητήματα

Οι ακόλουθες πληροφορίες προσδιορίζει το όριο για δημόσιους οργανισμούς Azure για το Azure Redis Cache:

| Οργανωμένη/δεδομένα που ελέγχονται από επιτρέπεται | Οργανωμένη/ελέγχονται δεδομένων δεν επιτρέπεται |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Όλα τα δεδομένα αποθηκεύονται και υποβάλλονται σε επεξεργασία στο Azure Redis Cache μπορούν να περιέχουν ρυθμίζεται για δημόσιους οργανισμούς Azure δεδομένων. | Azure μετα-δεδομένων Redis Cache δεν επιτρέπεται να περιέχει ελέγχονται εξαγωγή δεδομένων. Δεν καταχωρείτε δεδομένα οργανωμένη/ελέγχονται στα ακόλουθα πεδία: Cache ονόματος, VNET, όνομα της συνδρομής, ομάδες πόρων, ετικέτες πόρων, Redis ιδιότητες.  

##  <a name="next-steps"></a>Επόμενα βήματα

Για εγγραφή συμπληρωματικές πληροφορίες και ενημερώσεις του <a href="https://blogs.msdn.microsoft.com/azuregov/">ιστολόγιο του Microsoft Azure για δημόσιους οργανισμούς.</a>
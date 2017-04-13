<properties 
    pageTitle="API DocumentDB Python & SDK | Microsoft Azure" 
    description="Μάθετε όλες οι πληροφορίες για το Python API και SDK συμπεριλαμβανομένων κυκλοφορίας ημερομηνίες, συνταξιοδότηση ημερομηνίες και τις αλλαγές που έγιναν μεταξύ κάθε έκδοση του DocumentDB Python SDK." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API DocumentDB και SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ΥΠΌΛΟΙΠΟ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>API DocumentDB Python και SDK

<table>
<tr><td>**Κάντε λήψη του SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**Τεκμηρίωση API**</td><td>[Τεκμηρίωση αναφοράς Python API](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**Οδηγίες εγκατάστασης του SDK**</td><td>[Οδηγίες εγκατάστασης Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Συμβάλλει στην SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Γρήγορα αποτελέσματα**</td><td>[Γρήγορα αποτελέσματα με το SDK Python](documentdb-python-application.md)</td></tr>
<tr><td>**Τρέχουσα υποστηριζόμενη πλατφόρμα**</td><td>[Python 2.7](https://www.python.org/downloads/) και [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Σημειώσεις έκδοσης

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Υποστήριξη που προστέθηκε για Python 3.5.
- Υποστήριξη που προστέθηκε για τη συγκέντρωση συνδέσεων με χρήση της λειτουργικής μονάδας αιτήσεις.
- Υποστήριξη που προστέθηκε για την περίοδο λειτουργίας συνέπειας.
- Υποστήριξη που προστέθηκε για ΕΠΆΝΩ/ORDERBY ερωτημάτων για συλλογές διαμερίσματα.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Υποστήριξη πολιτικής προστέθηκε "Επανάληψη" για αιτήσεις επιτάχυνσης. (Επιτάχυνσης αιτήσεις λάβει μια αίτηση επιτόκιο πολύ μεγάλο εξαίρεση, κωδικός σφάλματος 429.) Από προεπιλογή, DocumentDB επαναλήψεις εννέα ώρες για κάθε αίτηση όταν συναντήσει κωδικός σφάλματος 429, honoring την ώρα retryAfter στην κεφαλίδα απόκρισης. Φορά διάστημα σταθερό "Επανάληψη" μπορούν να οριστούν ως μέρος της ιδιότητας RetryOptions στο αντικείμενο ConnectionPolicy εάν θέλετε να παραβλέψετε την ώρα retryAfter που επιστρέφεται από διακομιστή μεταξύ των επαναλήψεων του. DocumentDB τώρα αναμονή για έως 30 δευτερόλεπτα για κάθε αίτηση που είναι να επιβραδύνει (ανεξάρτητα από το πλήθος των επαναλήψεων) και επιστρέφει την απάντηση με κωδικό σφάλματος 429. Αυτήν τη στιγμή μπορεί επίσης να αντικαταστήσει στην ιδιότητα RetryOptions σε αντικείμενο ConnectionPolicy.

- DocumentDB επιστρέφει τώρα x-ms-επιτάχυνσης--των επαναλήψεων και x-ms-throttle-retry-wait-time-ms, όπως τις κεφαλίδες απόκρισης σε κάθε αίτηση για να καταδείξετε η επιτάχυνση επανάληψης count και την ώρα cummulative την πρόσκληση σε αναμονή μεταξύ των επαναλήψεων του.

- Καταργούνται RetryPolicy τάξη και την αντίστοιχη ιδιότητα (retry_policy) που εμφανίζονται στην κλάση document_client και αντί για αυτό Παρουσιάστηκε κλάση RetryOptions εκθέσετε της ιδιότητας RetryOptions κλάση ConnectionPolicy που μπορούν να χρησιμοποιηθούν για να παρακάμψετε κάποιες από τις προεπιλεγμένες επιλογές "Επανάληψη".

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Η προσθήκη την υποστήριξη για λογαριασμούς πολλών περιοχή της βάσης δεδομένων.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Η προσθήκη την υποστήριξη για τη δυνατότητα χρόνο για να Live(TTL) για τα έγγραφα.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Διορθώσεις σφαλμάτων που σχετίζονται με το διακομιστή πλευρά διαμερισμάτων για να επιτρέψετε σε ειδικούς χαρακτήρες στη διαδρομή partitionkey.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Εφαρμόζεται [διαμερίσματα συλλογές](documentdb-partition-data.md) και [επιδόσεων που ορίζονται από το χρήστη](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Προσθήκη κατακερματισμός & περιοχή διαμερισμάτων επίλυσης για να σας βοηθήσει με τις εφαρμογές sharding σε περισσότερα από ένα διαμερίσματα.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Υλοποίηση Upsert. Νέα UpsertXXX μέθοδοι προστεθεί υποστηρίζει τη δυνατότητα Upsert.
- Αναγνωριστικό υλοποίηση σύμφωνα με τη δρομολόγηση. Δεν υπάρχει δημόσια αλλαγών API, όλες οι αλλαγές εσωτερικό.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Ευρετήριο σε Γεωχωρικά υποστηρίζει.
- Επαληθεύει την ιδιότητα αναγνωριστικό για όλους τους πόρους. Ταυτότητες για τους πόρους που δεν είναι δυνατό να περιέχουν ?, /, #, \, χαρακτήρες ή τελειώνει με κενό διάστημα.
- Προσθέτει νέα κεφαλίδα "ευρετήριο μετασχηματισμό προόδου" ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Εφαρμόζει V2 πολιτικής δημιουργίας ευρετηρίου.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Υποστηρίζει σύνδεσης διακομιστή μεσολάβησης.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Αποδέσμευση & συνταξιοδότηση ημερομηνιών
Η Microsoft θα παρέχει ειδοποίηση τουλάχιστον **12 μηνών** πριν από την ακύρωση μιας SDK προκειμένου να ομαλά τη μετάβαση σε μια έκδοση νεότερη/υποστηρίζονται.

Νέες δυνατότητες και λειτουργίες και βελτιστοποιήσεις προστίθενται μόνο στο τρέχον SDK, όπως αυτό είναι να κάνετε πάντα αναβάθμιση στην πιο πρόσφατη SDK έκδοση ως πρώιμη όσο το δυνατόν. 

Οποιαδήποτε αίτηση για DocumentDB χρησιμοποιώντας μια ακύρωσης SDK θα απορρίπτονται από την υπηρεσία.

> [AZURE.WARNING]
Όλες οι εκδόσεις του SDK DocumentDB Azure για Python πριν από την έκδοση **1.0.0** θα είναι απόσυρση σε **29 Φεβρουαρίου 2016**. 

<br/>

| Έκδοση | Ημερομηνία δημοσίευσης | Απόσυρση ημερομηνίας 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 Σεπτεμβρίου 2016 |---
| [1.9.0](#1.9.0) | 07 Ιουλίου 2016 |---
| [1.8.0](#1.8.0) | 14 Ιουνίου 2016 |---
| [1.7.0](#1.7.0) | 26 Απριλίου 2016 |---
| [1.6.1](#1.6.1) | 08 Απριλίου 2016 |---
| [1.6.0](#1.6.0) | 29 Μαρτίου 2016 |---
| [1.5.0](#1.5.0) | 03 Ιανουαρίου 2016 |---
| [1.4.2](#1.4.2) | 06 Οκτωβρίου 2015 |---
| [1.4.1](#1.4.1) | 06 Οκτωβρίου 2015 |---
| [1.2.0](#1.2.0) | 06 Αυγούστου 2015 |---
| [1.1.0](#1.1.0) | 09 ΙΟΥΛΙΟΥ 2015 |---
| [1.0.1](#1.0.1) | 25 Μάιος 2015 |---
| [1.0.0](#1.0.0) | 07 Απριλίου 2015 |---
| 0.9.4-prelease | 14 Ιανουαρίου 2015 | 29 Φεβρουαρίου 2016
| 0.9.3-prelease | 09 Δεκέμβριο του 2014 | 29 Φεβρουαρίου 2016
| 0.9.2-prelease | 25 Νοεμβρίου 2014 | 29 Φεβρουαρίου 2016
| 0.9.1-prelease | 23 Σεπτεμβρίου 2014 | 29 Φεβρουαρίου 2016
| 0.9.0-prelease | 21 Αυγούστου 2014 | 29 Φεβρουαρίου 2016

## <a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Δείτε επίσης

Για να μάθετε περισσότερα σχετικά με το DocumentDB, ανατρέξτε στο θέμα σελίδα της υπηρεσίας [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 

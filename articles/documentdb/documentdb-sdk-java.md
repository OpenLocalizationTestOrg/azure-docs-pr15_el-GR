
<properties
    pageTitle="API DocumentDB Java & SDK | Microsoft Azure"
    description="Μάθετε όλες οι πληροφορίες για το Java API και SDK συμπεριλαμβανομένων κυκλοφορίας ημερομηνίες, συνταξιοδότηση ημερομηνιών και τις αλλαγές που έγιναν μεταξύ κάθε έκδοση του DocumentDB Java SDK."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API DocumentDB και SDK

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ΥΠΌΛΟΙΠΟ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>API DocumentDB Java και SDK

<table>
<tr><td>**Λήψη SDK**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**Τεκμηρίωση API**</td><td>[Τεκμηρίωση αναφοράς Java API](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Συμβάλλει στην SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Γρήγορα αποτελέσματα**</td><td>[Γρήγορα αποτελέσματα με το SDK Java](documentdb-java-application.md)</td></tr>
<tr><td>**Τρέχουσα υποστηριζόμενες χρόνου εκτέλεσης**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Σημειώσεις έκδοσης

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Υποστήριξη που προστέθηκε για το επίπεδο συνέπειας BoundedStaleness.
  - Υποστήριξη που προστέθηκε για άμεση συνδεσιμότητα για λειτουργίες CRUD για τις συλλογές διαμερίσματα.
  - Σταθερή ένα σφάλμα κατά την υποβολή ερωτημάτων βάσης δεδομένων με SQL.
  - Σταθερή ένα σφάλμα στο cache περιόδου λειτουργίας όπου μπορεί να έχει ρυθμιστεί σωστά διακριτικό περιόδου λειτουργίας.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Υποστήριξη που προστέθηκε για διασταύρωσης partition παράλληλες ερωτήματα.
  - Υποστήριξη που προστέθηκε για ΕΠΆΝΩ/ORDER BY ερωτημάτων για συλλογές διαμερίσματα.
  - Υποστήριξη που προστέθηκε για ισχυρό συνέπειας.
  - Υποστήριξη που προστέθηκε για αιτήσεις βάσει ονόματος κατά τη χρήση απευθείας σύνδεση.
  - Για να κάνετε ActivityId παραμείνετε συνεπείς σε όλες τις επαναλήψεις αίτηση διορθωθεί.
  - Σταθερή ένα σφαλμάτων που σχετίζονται με το cache περίοδο λειτουργίας όταν να δημιουργείτε μια συλλογή με το ίδιο όνομα.
  - Προστέθηκε πολύγωνου και τύποι δεδομένων LineString ενώ ο καθορισμός συλλογής ευρετηρίου πολιτικής για χωριστή παρουσίαση παν και διαχείριση χώρου ερωτήματα.
  - Σταθερή ζητήματα με το έγγραφο Java για Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Σταθερή ένα σφάλμα στο PartitionKeyDefinitionMap στο cache μόνο διαμερίσματα συλλογές και όχι να κάνετε επιπλέον λήψη διαμερισμάτων βασικών αιτήσεις.
  - Σταθερή ένα σφάλμα στην ξανά όταν παρέχεται μιας τιμής κλειδιού εσφαλμένες διαμερίσματα.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Η προσθήκη την υποστήριξη για λογαριασμούς πολλών περιοχή της βάσης δεδομένων.
  - Υποστήριξη που προστέθηκε για αυτόματη επανάληψη στην επιτάχυνσης αιτήσεις με επιλογές για να προσαρμόσετε την max προσπάθειες και την ώρα αναμονής max "Επανάληψη".  Ανατρέξτε στο θέμα RetryOptions και ConnectionPolicy.getRetryOptions().
  - Που έχουν καταργηθεί IPartitionResolver βάσει προσαρμοσμένου κώδικα διαμερισμού. Χρησιμοποιήστε διαμερίσματα συλλογές για μεγαλύτερη χώρου αποθήκευσης και απόδοση.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Υποστήριξη πολιτικής προστέθηκε "Επανάληψη" για περιορισμού.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Προστέθηκε ώρα για την υποστήριξη του live (TTL) για τα έγγραφα.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Εφαρμόζεται [διαμερίσματα συλλογές](documentdb-partition-data.md) και [επιδόσεων που ορίζονται από το χρήστη](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Σταθερή ένα σφάλμα στο HashPartitionResolver για τη δημιουργία κατακερματισμός τιμές σε μικρά endian ώστε να είναι συνεπείς με άλλες SDK.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Προσθήκη κατακερματισμός & περιοχή διαμερισμάτων επίλυσης για να σας βοηθήσει με τις εφαρμογές sharding σε περισσότερα από ένα διαμερίσματα.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Υλοποίηση Upsert. Νέα upsertXXX μέθοδοι προστεθεί υποστηρίζει τη δυνατότητα Upsert.
- Αναγνωριστικό υλοποίηση σύμφωνα με τη δρομολόγηση. Δεν υπάρχει δημόσια αλλαγών API, όλες οι αλλαγές εσωτερικό.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Παράλειψη για να μεταφέρετε αριθμό έκδοσης σε στοίχιση με άλλες SDK κυκλοφορίας

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Υποστηρίζει σε Γεωχωρικά ευρετηρίου
- Επαληθεύει την ιδιότητα αναγνωριστικό για όλους τους πόρους. Ταυτότητες για τους πόρους που δεν είναι δυνατό να περιέχουν ?, /, #, \, χαρακτήρες ή τελειώνει με κενό διάστημα.
- Προσθέτει νέα κεφαλίδα "ευρετήριο μετασχηματισμό προόδου" ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Υλοποιεί V2 πολιτικής δημιουργίας ευρετηρίου

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Αποδέσμευση & συνταξιοδότηση ημερομηνιών
Η Microsoft θα παρέχει ειδοποίηση τουλάχιστον **12 μηνών** πριν από την ακύρωση μιας SDK προκειμένου να ομαλά τη μετάβαση σε μια έκδοση νεότερη/υποστηρίζονται.

Νέες δυνατότητες και λειτουργίες και βελτιστοποιήσεις προστίθενται μόνο στο τρέχον SDK, όπως αυτό είναι να κάνετε πάντα αναβάθμιση στην πιο πρόσφατη SDK έκδοση ως πρώιμη όσο το δυνατόν.

Οποιαδήποτε αίτηση για DocumentDB χρησιμοποιώντας μια ακύρωσης SDK θα απορρίπτονται από την υπηρεσία.

> [AZURE.WARNING]
Όλες οι εκδόσεις του Azure DocumentDB SDK για Java πριν από την έκδοση **1.0.0** θα είναι απόσυρση σε **29 Φεβρουαρίου 2016**.

<br/>

| Έκδοση | Ημερομηνία δημοσίευσης | Απόσυρση ημερομηνίας
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 Οκτωβρίου 2016 |---
| [1.9.0](#1.9.0) | 03 Οκτωβρίου 2016 |---
| [1.8.1](#1.8.1) | 30 Ιουνίου 2016 |---
| [1.8.0](#1.8.0) | 14 Ιουνίου 2016 |---
| [1.7.1](#1.7.1) | 30 Απριλίου 2016 |---
| [1.7.0](#1.7.0) | 27 Απριλίου 2016 |---
| [1.6.0](#1.6.0) | 29 Μαρτίου 2016 |---
| [1.5.1](#1.5.1) | 31 Δεκεμβρίου 2015 |---
| [1.5.0](#1.5.0) | 04 Δεκεμβρίου 2015 |---
| [1.4.0](#1.4.0) | 05 Οκτωβρίου 2015 |---
| [1.3.0](#1.3.0) | 05 Οκτωβρίου 2015 |---
| [1.2.0](#1.2.0) | 05 Αυγούστου 2015 |---
| [1.1.0](#1.1.0) | 09 ΙΟΥΛΙΟΥ 2015 |---
| [1.0.1](#1.0.1) | 12 Μάιος 2015 |---
| [1.0.0](#1.0.0) | 07 Απριλίου 2015 |---
| 0.9.5-prelease | 09 Μαρ 2015 | 29 Φεβρουαρίου 2016
| 0.9.4-prelease | 17 Φεβρουαρίου 2015 | 29 Φεβρουαρίου 2016
| 0.9.3-prelease | 13 Ιανουαρίου 2015 | 29 Φεβρουαρίου 2016
| 0.9.2-prelease | 19 Δεκέμβριο του 2014 | 29 Φεβρουαρίου 2016
| 0.9.1-prelease | 19 Δεκέμβριο του 2014 | 29 Φεβρουαρίου 2016
| 0.9.0-prelease | 10 Δεκέμβριο του 2014 | 29 Φεβρουαρίου 2016

## <a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Δείτε επίσης

Για να μάθετε περισσότερα σχετικά με το DocumentDB, ανατρέξτε στο θέμα σελίδα της υπηρεσίας [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .

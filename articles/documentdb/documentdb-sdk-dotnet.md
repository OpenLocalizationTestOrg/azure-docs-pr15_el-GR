<properties 
    pageTitle="DocumentDB .NET API & SDK | Microsoft Azure" 
    description="Μάθετε όλες οι πληροφορίες για το .NET API και SDK συμπεριλαμβανομένων κυκλοφορίας ημερομηνίες, συνταξιοδότηση ημερομηνίες και τις αλλαγές που έγιναν μεταξύ κάθε έκδοση του DocumentDB .NET SDK." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>API DocumentDB και SDK 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [ΥΠΌΛΟΙΠΟ](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API και SDK

<table>
<tr><td>**Λήψη SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**Τεκμηρίωση API**</td><td>[Τεκμηρίωση αναφοράς .NET API](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Δείγματα**</td><td>[Δείγματα κώδικα .NET](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Γρήγορα αποτελέσματα**</td><td>[Γρήγορα αποτελέσματα με το .NET SDK DocumentDB](documentdb-get-started.md)</td></tr>
<tr><td>**Πρόγραμμα εκμάθησης εφαρμογής Web**</td><td>[Ανάπτυξη εφαρμογών Web με DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Τρέχουσα υποστηριζόμενες framework**</td><td>[Microsoft .NET Framework διαίρεσης 4,5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Σημειώσεις έκδοσης

> [AZURE.IMPORTANT] Ξεκινώντας με την έκδοση έκδοση 1.9.2, ενδέχεται να λάβετε System.NotSupportedException κατά την υποβολή ερωτήματος διαμερίσματα συλλογών. Για να αποφύγετε αυτό το σφάλμα, βεβαιωθείτε ότι το διεργασία κεντρικού υπολογιστή είναι 64-bit. Για το εκτελέσιμο έργα, αυτό μπορεί να γίνει, καταργώντας την επιλογή "Προτίμηση 32-bit" στο παράθυρο διαλόγου Ιδιότητες έργου, στην καρτέλα "Δημιουργία".

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Προστέθηκε απευθείας σύνδεση με την υποστήριξη για τις συλλογές διαμερίσματα.
  - Βελτιωμένη απόδοση για το επίπεδο συνέπειας δεσμεύεται Staleness.
  - Προστέθηκε πολύγωνου και τύποι δεδομένων LineString ενώ ο καθορισμός συλλογής ευρετηρίου πολιτικής για χωριστή παρουσίαση παν και διαχείριση χώρου ερωτήματα.
  - Υποστήριξη που προστέθηκε LINQ για StringEnumConverter, IsoDateTimeConverter και UnixDateTimeConverter κατά τη μετάφραση κατηγορήματα.
  - Διάφορες διορθώσεις σφαλμάτων SDK.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Σταθερή ζητήματος που προκάλεσαν το παρακάτω NotFoundException: Η περίοδος λειτουργίας ανάγνωσης δεν είναι διαθέσιμη για το διακριτικό περιόδου λειτουργίας εισαγωγής. Αυτή η εξαίρεση Παρουσιάστηκε σε ορισμένες περιπτώσεις, κατά την υποβολή ερωτήματος για την περιοχή ανάγνωση ενός λογαριασμού κατανεμημένο παν.
  - Εμφανίζεται η ιδιότητα ResponseStream στην τάξη ResourceResponse, που επιτρέπει την άμεση πρόσβαση στη ροή υποκείμενα από μια απάντηση.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Η τροποποίηση των κλάσεων ResourceResponse, FeedResponse, StoredProcedureResponse και MediaResponse για την υλοποίηση το αντίστοιχο δημόσια περιβάλλον εργασίας ώστε να μπορεί να είναι η mocked τους για δοκιμή βάσει ανάπτυξης (TDD).
  - Διορθωθεί το πρόβλημα που προκάλεσαν κεφαλίδας βασικών ακατάλληλη partition όταν χρησιμοποιείτε ένα προσαρμοσμένο αντικείμενο JsonSerializerSettings για σειριοποίησης δεδομένων.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Σταθερή ένα θέμα που προκάλεσαν μεγάλης διάρκειας ερωτήματα αποτυχία με το σφάλμα: το διακριτικό εξουσιοδότησης δεν είναι έγκυρη προς το παρόν.
  - Διορθωθεί το πρόβλημα που καταργηθεί το αρχικό SqlParameterCollection από διασταύρωσης partition επάνω/κατά ερωτήματα.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Υποστήριξη που προστέθηκε για παράλληλες ερωτημάτων για συλλογές διαμερίσματα.
  - Υποστήριξη που προστέθηκε για διασταύρωσης partition ORDER BY και ΕΠΆΝΩ ερωτημάτων για διαμερίσματα συλλογές.
  - Σταθερή τις αναφορές που λείπουν σε DocumentDB.Spatial.Sql.dll και Microsoft.Azure.Documents.ServiceInterop.dll που απαιτούνται κατά την αναφορά DocumentDB έργου με μια αναφορά στο πακέτο DocumentDB Nuget.
  - Σταθερή τη δυνατότητα να χρησιμοποιήσετε τις παραμέτρους των διαφορετικών τύπων όταν χρησιμοποιείτε συναρτήσεις που ορίζονται από το χρήστη σε LINQ. 
  - Σταθερή ένα σφάλμα για καθολικά από αναπαραγωγή λογαριασμούς όπου Upsert κλήσεις, να οδηγηθήκατε για να διαβάσετε θέσεις αντί για θέσεις εγγραφής.
  - Προστέθηκε μεθόδους για το περιβάλλον εργασίας IDocumentClient που έχουν που λείπουν: 
      - Μέθοδος UpsertAttachmentAsync που τίθεται σε mediaStream και επιλογές ως παραμέτρους
      - Μέθοδος CreateAttachmentAsync που λαμβάνει επιλογές ως παράμετρο
      - Μέθοδος CreateOfferQuery που τίθεται σε querySpec ως παράμετρο.
  - Αποσφραγισμένων κλάσεις δημόσια που εκτίθενται στο περιβάλλον εργασίας IDocumentClient.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Η προσθήκη την υποστήριξη για λογαριασμούς πολλών περιοχή της βάσης δεδομένων.
  - Υποστήριξη που προστέθηκε για "Επανάληψη" για αιτήσεις επιτάχυνσης.  Χρήστη να προσαρμόσετε τον αριθμό των επαναλήψεων και το χρόνο max αναμονής, ρυθμίζοντας τις παραμέτρους της ιδιότητας ConnectionPolicy.RetryOptions.
  - Προστίθεται ένα νέο περιβάλλον εργασίας IDocumentClient που καθορίζει τις υπογραφές όλων των ιδιοτήτων DocumenClient και μεθόδους.  Ως μέρος αυτής της αλλαγής, αλλάζουν επίσης μέθοδοι επέκτασης που δημιουργούν IQueryable και IOrderedQueryable μεθόδους στην τάξη DocumentClient ίδια.
  - Η προσθήκη επιλογή ρύθμισης παραμέτρων για να ορίσετε το ServicePoint.ConnectionLimit για μια δεδομένη τελικού σημείου DocumentDB Uri.  Χρησιμοποιήστε ConnectionPolicy.MaxConnectionLimit για να αλλάξετε την προεπιλεγμένη τιμή, η οποία έχει οριστεί σε 50.
  - IPartitionResolver που έχουν καταργηθεί και την εφαρμογή.  Υποστήριξη για IPartitionResolver τώρα έχει καταργηθεί. Συνιστάται να χρησιμοποιήσετε διαμερίσματα συλλογές για μεγαλύτερη χώρου αποθήκευσης και απόδοση.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Προστίθεται μια υπερφόρτωσης σε Uri με βάση τη μέθοδο ExecuteStoredProcedureAsync που τίθεται σε RequestOptions ως παράμετρο.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Προστέθηκε ώρα για την υποστήριξη του live (TTL) για τα έγγραφα.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[σημείο 1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Σταθερή ένα σφάλμα στη συσκευασία Nuget του .NET SDK για συσκευασία ως μέρος μιας λύσης υπηρεσία Cloud Azure.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Εφαρμόζεται [διαμερίσματα συλλογές](documentdb-partition-data.md) και [επιδόσεων που ορίζονται από το χρήστη](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Σταθερό]** Υποβολή ερωτημάτων DocumentDB παρουσιάζει τελικού σημείου: ' System.Net.Http.HttpRequestException: σφάλμα κατά την αντιγραφή περιεχομένου σε μια ροή.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Ανεπτυγμένο LINQ υποστηρίζει συμπεριλαμβανομένων νέα τελεστές για παραστάσεις σελιδοποίηση, υπό όρους και περιοχής σύγκρισης.
    - Λήψη τελεστή για να ενεργοποιήσετε την ΕΠΙΛΟΓΉ ΕΠΆΝΩ συμπεριφορά σε LINQ
    - Τελεστής CompareTo για να ενεργοποιήσετε την συγκρίσεις συμβολοσειρών περιοχής
    - Μορφοποίηση υπό όρους (?) και τη συνένωση τελεστές (?)
  - **[Σταθερό]** ArgumentOutOfRangeException κατά τον συνδυασμό προβολής μοντέλο με το πού σε linq ερωτήματος.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Σταθερό]** Εάν η επιλογή δεν είναι η τελευταία παράσταση η υπηρεσία παροχής LINQ θεωρείται ότι δεν υπάρχει προβολής και παραχθεί ΕΠΙΛΈΞΤΕ * λάθος.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Εφαρμόζεται Upsert, μεθόδους προσθέσει UpsertXXXAsync
 - Βελτιώσεις επιδόσεων για όλες τις αιτήσεις
 - Υπηρεσία παροχής LINQ υποστήριξη για μορφοποίηση υπό όρους, τη συνένωση και οι μέθοδοι CompareTo για τις συμβολοσειρές
 - **[Σταθερό]** Υπηρεσία παροχής LINQ--> περιέχει υλοποίηση μεθόδου στη λίστα για να δημιουργήσει το ίδιο SQL ως IEnumerable και πίνακα
 - **[Σταθερό]** BackoffRetryUtility χρησιμοποιεί ξανά το ίδιο HttpRequestMessage αντί για τη δημιουργία μιας νέας σε "Επανάληψη"
 - **[Παλιά]** --> UriFactory.CreateCollection πρέπει τώρα να χρησιμοποιείτε UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Σταθερό]** Θέματα τοπικής προσαρμογής κατά τη χρήση μη en κουλτούρας πληροφορίες όπως nl-NL κ.λπ. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Αναγνωριστικό με βάση τη δρομολόγηση
    - Νέα UriFactory Βοηθό να σας βοηθήσουν με δημιουργία ανακοινώσεων Αναγνωριστικό βάσει συνδέσεις πόρων
    - Νέα τύποι στην DocumentClient να τραβήξετε στο URI
  - Προστέθηκε IsValid() και IsValidDetailed() στο LINQ για σε γεωχωρικά
  - Βελτιωμένη υποστήριξη LINQ υπηρεσία παροχής
    - Περικοπή Atan **μαθηματικών** - Abs, Acos, Asin, Ceiling, Cos Exp, Floor, αρχείο καταγραφής, Log10, Pow, Round, εισόδου, Sin, η συνάρτηση Sqrt, Tan,
    - **Συμβολοσειρά** - Concat, περιέχει, EndsWith, IndexOf, πλήθος, ToLower, TrimStart, αντικατάσταση, αναστρέψετε, TrimEnd, StartsWith, δευτερεύουσα συμβολοσειρά, ToUpper
    - Περιέχει **πίνακα** - Concat, Count
    - Τελεστή **IN**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Υποστήριξη που προστέθηκε για την τροποποίηση πολιτικές δημιουργίας ευρετηρίου
    - Νέα μέθοδος ReplaceDocumentCollectionAsync στο DocumentClient
    - Νέα ιδιότητα IndexTransformationProgress στο ResourceResponse<T> για παρακολούθηση της προόδου ποσοστού των αλλαγών πολιτικής ευρετηρίου
    - Τώρα είναι μεταβλητά DocumentCollection.IndexingPolicy
  - Υποστήριξη που προστέθηκε για χώρου δημιουργίας ευρετηρίου και ερωτήματος
    - Νέο χώρο ονομάτων Microsoft.Azure.Documents.Spatial για τη σειριοποίηση/αποσειριοποίηση χώρου τύποι όπως σημείο και πολυγώνου
    - Νέα κλάση SpatialIndex για τη δημιουργία ευρετηρίου GeoJSON δεδομένα αποθηκευμένα σε DocumentDB
  - **[Σταθερά]** : ερώτημα εσφαλμένες SQL που δημιουργούνται από την παράσταση linq [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Εξάρτηση από Newtonsoft.Json v5.0.7 
- Αλλαγές για την υποστήριξη Order By
  - Υποστήριξη παροχής LINQ για OrderBy() ή OrderByDescending()
  - IndexingPolicy για την υποστήριξη Order By 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Υποστήριξη για τη δημιουργία διαμερισμάτων δεδομένων, χρησιμοποιώντας το νέο κλάσεις HashPartitionResolver και RangePartitionResolver και το IPartitionResolver
- DataContract σειριοποίησης
- Υποστήριξη GUID στην υπηρεσία παροχής LINQ
- Υποστήριξη UDF στο LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
Παρουσιάστηκε μια αλλαγή του ονόματος του πακέτου NuGet ανάμεσα σε προεπισκόπηση και GA. Θα σας μετακινείται από **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Προεπισκόπηση SDK [παλιά]

## <a name="release--retirement-dates"></a>Αποδέσμευση & συνταξιοδότηση ημερομηνιών
Η Microsoft θα παρέχει ειδοποίηση τουλάχιστον **12 μηνών** πριν από την ακύρωση μιας SDK προκειμένου να ομαλά τη μετάβαση σε μια έκδοση νεότερη/υποστηρίζονται.

Νέες δυνατότητες και λειτουργίες και βελτιστοποιήσεις προστίθενται μόνο στο τρέχον SDK, όπως συνιστάται να κάνετε πάντα αναβάθμιση στην πιο πρόσφατη SDK έκδοση ως πρώιμη όσο το δυνατόν. 

Οποιαδήποτε αίτηση για DocumentDB χρησιμοποιώντας μια ακύρωσης SDK θα απορρίπτονται από την υπηρεσία.

> [AZURE.WARNING]
Όλες οι εκδόσεις του SDK DocumentDB Azure για το .NET πριν από την έκδοση **1.0.0** θα είναι απόσυρση σε **29 Φεβρουαρίου 2016**. 
 
<br/>
 
| Έκδοση | Ημερομηνία δημοσίευσης | Απόσυρση ημερομηνίας 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 Σεπτεμβρίου 2016 |---
| [1.9.5](#1.9.5) | 01 Σεπτεμβρίου 2016 |---
| [1.9.4](#1.9.4) | 24 Αυγούστου 2016 |---
| [1.9.3](#1.9.3) | 15 Αυγούστου 2016 |---
| [1.9.2](#1.9.2) | 23 Ιουλίου 2016 |---
| 1.9.1 | Καταργηθεί |---
| 1.9.0 | Καταργηθεί |---
| [1.8.0](#1.8.0) | 14 Ιουνίου 2016 |---
| [1.7.1](#1.7.1) | 06 Μαΐου 2016 |---
| [1.7.0](#1.7.0) | 26 Απριλίου 2016 |---
| [σημείο 1.6.3](#1.6.3) | 08 Απριλίου 2016 |---
| [1.6.2](#1.6.2) | 29 Μαρτίου 2016 |---
| [1.5.3](#1.5.3) | 19 Φεβρουαρίου 2016 |---
| [1.5.2](#1.5.2) | 14 Δεκεμβρίου 2015 |---
| [1.5.1](#1.5.1) | 23 Νοεμβρίου 2015 |---
| [1.5.0](#1.5.0) | 05 Οκτωβρίου 2015 |---
| [1.4.1](#1.4.1) | 25 Αυγούστου 2015 |---
| [1.4.0](#1.4.0) | 13 Αυγούστου 2015 |---
| [1.3.0](#1.3.0) | 05 Αυγούστου 2015 |---
| [1.2.0](#1.2.0) | 06 ΙΟΥΛΙΟΥ 2015 |---
| [1.1.0](#1.1.0) | 30 Απριλίου 2015 |---
| [1.0.0](#1.0.0) | 08 Απριλίου 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12 Μαρτίου 2015 | 29 Φεβρουαρίου 2016
| [0.9.2-prelease](#0.9.x-preview) | Ιανουαρίου 2015 | 29 Φεβρουαρίου 2016
| [.9.1-prelease](#0.9.x-preview) | 13 Οκτωβρίου 2014 | 29 Φεβρουαρίου 2016
| [0.9.0-prelease](#0.9.x-preview) | 21 Αυγούστου 2014 | 29 Φεβρουαρίου 2016

## <a name="faq"></a>ΣΥΝΉΘΕΙΣ ΕΡΩΤΉΣΕΙΣ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Δείτε επίσης

Για να μάθετε περισσότερα σχετικά με το DocumentDB, ανατρέξτε στο θέμα σελίδα της υπηρεσίας [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) . 

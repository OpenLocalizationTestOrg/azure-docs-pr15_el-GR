<properties 
    pageTitle="Ερωτήσεις DocumentDB βάσης δεδομένων - συνήθεις ερωτήσεις | Microsoft Azure" 
    description="Λάβετε απαντήσεις σε συνήθεις ερωτήσεις σχετικά με το Azure DocumentDB μια υπηρεσία NoSQL εγγράφου βάσης δεδομένων για JSON. Answer βάσης δεδομένων ερωτήσεις σχετικά με τη χωρητικότητα, επιδόσεις και κλιμάκωση." 
    keywords="Ερωτήματα βάσης δεδομένων, συνήθεις ερωτήσεις, documentdb, azure, Microsoft azure"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Συνήθεις ερωτήσεις σχετικά με DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Βάση δεδομένων ερωτήσεις σχετικά με τα βασικά Microsoft Azure DocumentDB

### <a name="what-is-microsoft-azure-documentdb"></a>Τι είναι το Microsoft Azure DocumentDB; 
Microsoft Azure DocumentDB είναι μια εκπληκτική γρήγορη, πλανήτη κλίμακας NoSQL εγγράφου βάσης δεδομένων-ως-a-υπηρεσία που προσφέρει εμπλουτισμένες υποβολή ερωτημάτων στα δεδομένα σχήματος χωρίς, σάς βοηθά να κάνουν με δυνατότητα ρύθμισης παραμέτρων και αξιόπιστες επιδόσεις, και επιτρέπει ταχεία ανάπτυξη, πάντα μέσα από μια διαχειριζόμενη πλατφόρμα ασφαλείας από το power και επίτευξη του Microsoft Azure. DocumentDB είναι η σωστή λύση για κινητές συσκευές, παιχνιδιών web και εφαρμογές IoT όταν είναι προβλέψιμα μετάδοσης, υψηλή διαθεσιμότητα, χαμηλή λανθάνων χρόνος και μοντέλου δεδομένων χωρίς σχήματος βασικές απαιτήσεις. DocumentDB παρέχει ευελιξία σχήματος και εμπλουτισμένων ευρετηρίου μέσω του μοντέλου δεδομένων με JSON εγγενούς, ενώ περιλαμβάνει πολλά έγγραφα υποστήριξη συναλλαγών με ενσωματωμένη JavaScript.  
  
Για περισσότερες ερωτήσεις βάσης δεδομένων, απαντήσεις και οδηγίες σχετικά με την ανάπτυξη και τη χρήση αυτής της υπηρεσίας, ανατρέξτε στη [σελίδα τεκμηρίωση DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Τι είδους βάση δεδομένων είναι DocumentDB;
DocumentDB είναι NoSQL εγγράφου προσανατολισμό βάσης δεδομένων που αποθηκεύει δεδομένα σε μορφή JSON.  DocumentDB υποστηρίζει δομές ένθετων, προσωπική contained δεδομένων που μπορούν να αναζητηθούν μέσω ενός εμπλουτισμένου DocumentDB [γραμματικής ερωτήματος SQL](documentdb-sql-query.md). DocumentDB παρέχει υψηλές επιδόσεις συναλλαγών επεξεργασίας του διακομιστή JavaScript μέσω [αποθηκευμένες διαδικασίες, εναύσματα, και οι συναρτήσεις που ορίζονται από το χρήστη](documentdb-programming.md). Η βάση δεδομένων υποστηρίζει επίσης επίπεδα tunable συνέπειας προγραμματιστής με σχετικές [επιδόσεων](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Βάσεις δεδομένων DocumentDB έχουν πίνακες, όπως μια σχεσιακή βάση δεδομένων (RDBMS);
Όχι, DocumentDB αποθηκεύει δεδομένα σε συλλογές εγγράφων JSON.  Για πληροφορίες σχετικά με τους πόρους DocumentDB, ανατρέξτε στο θέμα [μοντέλο DocumentDB πόρων και τα θέματα](documentdb-resources.md). Για περισσότερες πληροφορίες σχετικά με το πώς NoSQL λύσεις όπως DocumentDB διαφέρουν από σχεσιακές λύσεις, ανατρέξτε στο θέμα [σύγκριση NoSQL SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>DocumentDB βάσεις δεδομένων υποστηρίζουν σχήμα χωρίς δεδομένα;
Ναι, DocumentDB επιτρέπει στις εφαρμογές για να αποθηκεύσετε αυθαίρετο JSON έγγραφα χωρίς ορισμό σχήματος ή συμβουλές. Δεδομένα είναι αμέσως διαθέσιμα για το ερώτημα μέσω του περιβάλλοντος εργασίας του ερωτήματος DocumentDB SQL.   

### <a name="does-documentdb-support-acid-transactions"></a>Υποστηρίζει DocumentDB ΟΞΈΟΣ συναλλαγές;
Ναι, DocumentDB υποστηρίζει συναλλαγές άλλα έγγραφα εκφρασμένη JavaScript αποθηκευμένες διαδικασίες και εναύσματα. Συναλλαγές στοχεύουν σε μια μεμονωμένη partition μέσα σε κάθε συλλογή και εκτελεστεί με ΟΞΈΟΣ σημασιολογία με όλα ή τίποτα απομονωμένες από άλλες ταυτόχρονα εκτέλεση αιτήσεις κώδικα και χρήστη.  Εάν εξαιρέσεις είναι δημιουργήθηκε μέσω της εκτέλεσης πλευρά του διακομιστή κώδικα JavaScript εφαρμογής, ολόκληρη η συναλλαγή επανέρχεται. Για περισσότερες πληροφορίες σχετικά με τις συναλλαγές, ανατρέξτε στο θέμα [πρόγραμμα συναλλαγές της βάσης δεδομένων](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Τι είναι η τυπική χρήση περιπτώσεις για DocumentDB;  
DocumentDB είναι μια καλή επιλογή για νέα παιχνιδιών κινητές συσκευές, web, και είναι σημαντικό να IoT εφαρμογές όπου Αυτόματη κλίμακα, προβλέψιμα επιδόσεις, γρήγορη σειρά των χρόνων απόκρισης χιλιοστά του δευτερολέπτου, καθώς και τη δυνατότητα να ερωτήματος επάνω από το σχήμα χωρίς δεδομένα. DocumentDB είναι για ταχεία ανάπτυξη και την υποστήριξη του συνεχής διαδοχικών προσεγγίσεων μοντέλων δεδομένων εφαρμογής. Εφαρμογές που διαχειρίζεστε περιεχόμενο που δημιουργούνται από το χρήστη και τα δεδομένα είναι [συνήθεις περιπτώσεις χρήσης για DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Πώς DocumentDB προσφέρουν προβλέψιμα επιδόσεων;
Μια [πρόσκληση σε μονάδα](documentdb-request-units.md) είναι το μέτρο της μετάδοση σε DocumentDB. 1 RU αντιστοιχεί με την ταχύτητα μεταγωγής του ΛΉΨΗΣ ενός εγγράφου 1KB. Κάθε λειτουργία σε DocumentDB, συμπεριλαμβανομένων των διαβάζει, εγγραφών, ερωτήματα SQL και εκτελέσεις αποθηκευμένη διαδικασία έχει ντετερμινιστική RU τιμή με βάση τη μετάδοση που απαιτείται για την ολοκλήρωση της λειτουργίας. Αντί να σκέφτεστε CPU, εισόδου/ΕΞΌΔΟΥ και μνήμης και πώς κάθε ένα από αυτά επηρεάζουν την ταχύτητα εφαρμογής, μπορείτε να θεωρήσετε όσον αφορά μία μέτρηση RU.

Κάθε συλλογή DocumentDB μπορεί να είναι δεσμευμένο με προμήθεια του φακέλου μεταγωγή όσον αφορά RUs ταχύτητα μεταγωγής ανά δευτερόλεπτο. Για εφαρμογές οποιαδήποτε κλίμακας, μπορείτε να σημείων αναφοράς μεμονωμένα αιτήματα για να μετρήσετε τους RU τιμές και παροχή συλλογές να χειρίζεται το σύνολο των μονάδων αίτηση σε όλες τις αιτήσεις. Μπορείτε να περιορίσετε το μέγεθος ή να κλιμακωθεί προς τα κάτω τη συλλογή μετάδοσης με τις ανάγκες σας evolve εφαρμογής. Για περισσότερες πληροφορίες σχετικά με τις μονάδες αίτηση και για βοήθεια σχετικά με τον καθορισμό τη συλλογή πρέπει, ελέγξτε Διαβάστε [Διαχείριση επιδόσεων και χωρητικότητας](documentdb-manage.md) και προσπαθήστε την [Αριθμομηχανή μετάδοσης](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Είναι συμβατό με DocumentDB HIPAA;
Ναι, DocumentDB είναι συμβατά με HIPAA. Απαιτήσεις για τη χρήση, κοινοποίηση, δημιουργεί HIPAA και την προστασία των πληροφοριών μεμονωμένα αναγνωρίσιμα εύρυθμης λειτουργίας. Για περισσότερες πληροφορίες, ανατρέξτε στο [Κέντρο αξιοπιστίας της Microsoft](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-the-storage-limits-of-documentdb"></a>Τι είναι τα όρια χώρου αποθήκευσης της DocumentDB; 
Υπάρχει όριο θεωρητική στο συνολικό ποσό των δεδομένων που μπορούν να αποθηκεύουν μια συλλογή στην DocumentDB. Εάν θέλετε να αποθηκεύσετε πάνω από 250 GB δεδομένων μέσα σε μια ενιαία συλλογή, επικοινωνήστε [Επικοινωνήστε με την υποστήριξη](documentdb-increase-limits.md) για να έχετε το όριο του λογαριασμού σας αυξάνεται. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Τι είναι τα όρια μετάδοσης της DocumentDB; 
Υπάρχει όριο θεωρητική για το σύνολο της μετάδοσης που μπορεί να υποστηρίξει μια συλλογή στην DocumentDB, εάν το φόρτο εργασίας μπορούν να διανεμηθούν περίπου ομοιόμορφα μεταξύ των αρκετά μεγάλο αριθμό αριθμούς-κλειδιά διαμερισμάτων. Εάν θέλετε να υπερβεί 250.000 αίτηση μονάδες/δευτερόλεπτο ανά συλλογή ή το λογαριασμό, επικοινωνήστε [Επικοινωνήστε με την υποστήριξη](documentdb-increase-limits.md) για να έχετε το όριο του λογαριασμού σας αυξάνεται. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Πόσο κοστίζει το Microsoft Azure DocumentDB;
Ανατρέξτε στη σελίδα [λεπτομερειών τιμολόγησης DocumentDB](https://azure.microsoft.com/pricing/details/documentdb/) για λεπτομέρειες. Χρεώσεις χρήσης DocumentDB καθορίζονται από τον αριθμό των συλλογών χρησιμοποιείται, τον αριθμό των ωρών που έχουν online, τις συλλογές και την αποθήκευση αναλωμένες και την προμήθεια του φακέλου μετάδοσης για κάθε συλλογή. 

### <a name="is-there-a-free-account-available"></a>Υπάρχει ένα δωρεάν λογαριασμό διαθέσιμη;
Εάν είστε νέος χρήστης του Azure, μπορείτε να εγγραφείτε για έναν [δωρεάν λογαριασμό της Azure](https://azure.microsoft.com/free/), ο οποίος σάς παρέχει 30 ημέρες και $200 για να δοκιμάσετε όλων των υπηρεσιών Azure. Εναλλακτικά, εάν έχετε μια συνδρομή στο Visual Studio, που είναι επιλέξιμα από [150 $ στο δωρεάν Azure πιστώσεων ανά μήνα](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) για να χρησιμοποιήσετε οποιαδήποτε υπηρεσία Azure.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Πώς μπορώ να λάβω επιπλέον βοήθεια σχετικά με DocumentDB;
Εάν χρειάζεστε βοήθεια με οποιοδήποτε, επικοινωνία μας σε [Υπερχείλιση στοίβας](http://stackoverflow.com/questions/tagged/azure-documentdb), τα [Φόρουμ για προγραμματιστές MSDN DocumentDB Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), ή προγραμματισμός μια [συνομιλία 1:1 με την ομάδα των υπηρεσιών μηχανικής DocumentDB](http://www.askdocdb.com/). Για να παραμείνετε ενήμεροι για τις πιο πρόσφατες ειδήσεις DocumentDB και τις δυνατότητες, ακολουθήστε μας στο [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Ρύθμιση του Microsoft Azure DocumentDB

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Πώς μπορώ να εγγραφής για το Microsoft Azure DocumentDB;
Microsoft Azure DocumentDB είναι διαθέσιμη στην [Πύλη του Azure][azure-portal].  Πρώτα που πρέπει να εγγραφείτε για μια συνδρομή στο Microsoft Azure.  Αφού εγγραφείτε για μια συνδρομή στο Microsoft Azure, μπορείτε να προσθέσετε ένα λογαριασμό DocumentDB στη συνδρομή σας στο Azure. Για οδηγίες σχετικά με την προσθήκη ενός λογαριασμού DocumentDB, ανατρέξτε στο θέμα [Δημιουργία λογαριασμού DocumentDB βάσης δεδομένων](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Τι είναι ένα πρωτεύον κλειδί;
Ένα πρωτεύον κλειδί είναι ένα διακριτικού ασφαλείας για να αποκτήσετε πρόσβαση σε όλους τους πόρους για ένα λογαριασμό. Άτομα με τον αριθμό-κλειδί έχει πρόσβαση ανάγνωσης και εγγραφής για να το όλους τους πόρους στο λογαριασμό βάσης δεδομένων. Να είστε προσεκτικοί κατά την κατανομή πρωτεύοντα κλειδιά. Το πρωτεύον κλειδί κύρια και δευτερεύουσα πρωτεύον κλειδί είναι διαθέσιμες στο τα **πλήκτρα **blade της [Πύλης Azure][azure-portal]. Για περισσότερες πληροφορίες σχετικά με τα κλειδιά, ανατρέξτε στο θέμα [Προβολή "," Αντιγραφή "και" πλήκτρα πρόσβασης regenerate](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Πώς μπορώ να δημιουργήσω μια βάση δεδομένων;
Μπορείτε να δημιουργήσετε βάσεις δεδομένων με την [Πύλη Azure]() , όπως περιγράφεται στο θέμα [Δημιουργία μιας βάσης δεδομένων DocumentDB](documentdb-create-database.md), ένα από τα [DocumentDB SDK](documentdb-sdk-dotnet.md), ή μέσω τα [ΥΠΌΛΟΙΠΑ APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).  

### <a name="what-is-a-collection"></a>Τι είναι μια συλλογή;
Μια συλλογή είναι ένα κοντέινερ JSON εγγράφων και τη συσχετισμένη λογική εφαρμογής JavaScript. Μια συλλογή είναι χρεώσιμων οντότητα, όπου το [κόστος](documentdb-performance-levels.md) προσδιορίζεται από τη μετάδοση και storaged χρησιμοποιούνται. Συλλογές μπορεί να εκτείνεται σε έναν ή περισσότερους διαμερίσματα/διακομιστές και μπορεί να περιορίσετε το μέγεθος για το χειρισμό όγκους σχεδόν απεριόριστο χώρο αποθήκευσης ή μετάδοσης.

Συλλογές είναι επίσης των χρεώσεων οντοτήτων για DocumentDB. Κάθε συλλογή χρεώθηκε ανά ώρα με βάση την προμήθεια του φακέλου μετάδοσης και το χώρο αποθήκευσης που χρησιμοποιείται. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [DocumentDB τις πληροφορίες τιμολόγησης](https://azure.microsoft.com/pricing/details/documentdb/).  

### <a name="how-do-i-set-up-users-and-permissions"></a>Πώς μπορώ να ρυθμίσω χρήστες και δικαιώματα;
Μπορείτε να δημιουργήσετε χρήστες και δικαιώματα χρησιμοποιώντας ένα από τα [DocumentDB SDK](documentdb-sdk-dotnet.md) ή μέσω τα [ΥΠΌΛΟΙΠΑ APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Βάση δεδομένων ερωτήσεις σχετικά με την ανάπτυξη σε σχέση με το Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Τρόπος για να το κάνετε να ξεκινήσω ανάπτυξη σε σχέση με DocumentDB;
[SDK](documentdb-sdk-dotnet.md) είναι διαθέσιμες για το .NET, Python, Node.js, JavaScript και Java.  Οι προγραμματιστές μπορούν επίσης να αξιοποιήσετε τα [RESTful HTTP APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) για να αλληλεπιδράσετε με πόρους DocumentDB από μια ποικιλία πλατφόρμες και γλώσσες. 

Δείγματα για το DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)και [Python](documentdb-python-samples.md) SDK είναι διαθέσιμες στην GitHub.

### <a name="does-documentdb-support-sql"></a>Υποστηρίζει DocumentDB SQL;
Τη γλώσσα ερωτήματος DocumentDB SQL είναι ένα βελτιωμένο υποσύνολο τις λειτουργίες ερωτήματος που υποστηρίζονται από το SQL. Τη γλώσσα ερωτήματος DocumentDB SQL παρέχει εμπλουτισμένου ιεραρχική και σχεσιακούς τελεστές και επεκτασιμότητα μέσω του χρήστη βάσει JavaScript που ορίζονται από το συναρτήσεις (UDF). Γραμματικός έλεγχος JSON επιτρέπει μοντελοποίησης JSON εγγράφων ως δέντρα με ετικέτες ως τους κόμβους δέντρου, που χρησιμοποιείται από τόσο τις τεχνικές αυτόματης δημιουργίας ευρετηρίου DocumentDB καθώς και η διάλεκτος ερωτήματος SQL του DocumentDB.  Για λεπτομέρειες σχετικά με τον τρόπο για να χρησιμοποιήσετε τη σύνταξη SQL, ανατρέξτε στο θέμα το [Ερώτημα DocumentDB] [ query] το άρθρο.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Ποιοι είναι οι τύποι δεδομένων που υποστηρίζονται από το DocumentDB;
Το προκαταρκτικούς τύπους δεδομένων που υποστηρίζονται σε DocumentDB είναι ίδια όπως και JSON. JSON έχει ένα σύστημα απλού τύπου που αποτελείται από συμβολοσειρές, οι αριθμοί (IEEE754 διπλής ακρίβειας) και δυαδικές τιμές - τιμή true, false και τιμές null.  Πιο σύνθετες τύπους δεδομένων, όπως το DateTime, Guid, Int64 και γεωμετρική μπορεί να απεικονιστεί τόσο στο JSON και DocumentDB μέσω της δημιουργίας ένθετα αντικείμενα χρησιμοποιώντας τον τελεστή {} και πίνακες χρησιμοποιώντας τον τελεστή []. 

### <a name="how-does-documentdb-provide-concurrency"></a>Πώς παρέχει DocumentDB ταυτόχρονης εκτέλεσης;
DocumentDB υποστηρίζει βέλτιστου ελέγχου (OCC) μέσω HTTP οντότητα ετικέτες ή etags. Κάθε πόρο DocumentDB έχει μια etag και το etag έχει ρυθμιστεί στο διακομιστή κάθε φορά που ένα έγγραφο έχει ενημερωθεί. Στην κεφαλίδα etag και την τρέχουσα τιμή περιλαμβάνονται σε όλα τα μηνύματα απάντησης. Etags μπορεί να χρησιμοποιηθεί με την κεφαλίδα If-Match για να επιτρέψετε στο διακομιστή για να αποφασίσετε εάν θα πρέπει να ενημερωθούν έναν πόρο. Η τιμή If-Match είναι η τιμή etag πρέπει να ελεγχθεί. Εάν η τιμή etag συμφωνεί με την τιμή etag διακομιστή, ο πόρος θα ενημερωθεί. Εάν το etag δεν είναι πλέον τρέχουσα, ο διακομιστής απορρίπτει τη λειτουργία με μια "HTTP 412 προϋπόθεση αποτυχία" κωδικός απόκρισης. Το πρόγραμμα-πελάτη, στη συνέχεια, θα πρέπει να επαναλάβει τη λήψη του πόρου για να αποκτήσετε την τρέχουσα τιμή etag για τον πόρο. Επιπλέον, μπορεί να χρησιμοποιηθεί etags με κεφαλίδα If-None-Match για να προσδιορίσετε εάν είναι απαραίτητη η εκ νέου λήψη ενός πόρου. 

Για να χρησιμοποιήσετε βέλτιστου .NET, χρησιμοποιήστε την κλάση [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) . Για ένα δείγμα .NET, ανατρέξτε στο θέμα [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) στο δείγμα DocumentManagement σε github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Πώς μπορώ να εκτελέσω συναλλαγές στο DocumentDB;
DocumentDB υποστηρίζει ενοποιημένη γλώσσα συναλλαγές μέσω JavaScript αποθηκευμένες διαδικασίες και εναύσματα. Όλες οι λειτουργίες βάσης δεδομένων μέσα σε δέσμες ενεργειών εκτελούνται στην περιοχή απομόνωσης στιγμιότυπου που έχει ρυθμιστεί για τη συλλογή εάν είναι μια συλλογή μίας διαμερισμάτων ή έγγραφα με την ίδια τιμή κλειδιού partition μέσα σε μια συλλογή, εάν έχει διαμερίσματα της συλλογής. Ένα στιγμιότυπο των εκδόσεων εγγράφου (ETags) έχει ληφθεί κατά την έναρξη της κίνησης και δεσμευμένου μόνο εάν η δέσμη ενεργειών ολοκληρωθεί με επιτυχία. Εάν το JavaScript παρουσιάσει σφάλμα, η συναλλαγή ακυρώνεται. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πλευρά του διακομιστή DocumentDB προγραμματισμού](documentdb-programming.md) .

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Πώς μπορώ να μαζικής εισαγωγή εγγράφων σε DocumentDB; 
Υπάρχουν τρεις τρόποι για τη μαζική εισαγωγή εγγράφων σε DocumentDB:

- Τα δεδομένα εργαλείου μετεγκατάστασης, όπως περιγράφεται στην [Εισαγωγή δεδομένων σε DocumentDB](documentdb-import-data.md).
- Εξερεύνηση εγγράφων στην πύλη του Azure, όπως περιγράφεται στο [μαζική προσθήκη εγγράφων με την Εξερεύνηση των εγγράφων](documentdb-view-json-document-explorer.md#BulkAdd).
- Αποθηκευμένες διαδικασίες, όπως περιγράφεται στην [πλευρά του διακομιστή DocumentDB προγραμματισμού](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB υποστηρίζει προσωρινή αποθήκευση του πόρου σύνδεση;
Ναι, επειδή DocumentDB είναι μια υπηρεσία RESTful, συνδέσεις πόρων είναι αμετάβλητες και μπορούν να είναι στο cache. Προγράμματα-πελάτες του DocumentDB να καθορίσετε μια κεφαλίδα "If-None-Match" για διαβάζει σε σχέση με οποιαδήποτε πόρων όπως το έγγραφο ή τη συλλογή και ενημέρωση τους τοπική αντιγράφει μόνο όταν έχει αλλάξει την έκδοση διακομιστή. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Είναι διαθέσιμη μια τοπική παρουσία της DocumentDB;
Αυτήν τη στιγμή δεν είναι διαθέσιμη μια τοπική παρουσία της DocumentDB. Μπορείτε να παρακολουθείτε την κατάσταση μιας τοπικής προσομοίωσης και ψηφοφορία για αυτό στο [φόρουμ σχολίων](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance).


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 

<properties 
    pageTitle="Προγραμματισμός DocumentDB: αποθηκευμένες διαδικασίες, εναύσματα βάσης δεδομένων και UDF | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε DocumentDB για να γράψετε αποθηκευμένες διαδικασίες, εναύσματα βάσης δεδομένων και οι συναρτήσεις που ορίζονται από το χρήστη (UDF) στο JavaScript. Λήψη συμβουλών programing βάσης δεδομένων και πολλά άλλα." 
    keywords="Βάση δεδομένων εναύσματα, αποθηκευμένη διαδικασία, αποθηκευμένη διαδικασία, πρόγραμμα βάσης δεδομένων, sproc, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Προγραμματισμός διακομιστή DocumentDB: αποθηκευμένες διαδικασίες, εναύσματα βάσης δεδομένων και UDF

Μάθετε πώς ολοκλήρωμα της γλώσσας του Azure DocumentDB, συναλλαγών εκτέλεσης της JavaScript επιτρέπει στους προγραμματιστές σύνταξη εγγενώς **αποθηκευμένες διαδικασίες**, **εναύσματα** και **Οι συναρτήσεις που ορίζονται από το χρήστη** στο JavaScript. Αυτό σας επιτρέπει να γράψετε λογική εφαρμογής πρόγραμμα βάσης δεδομένων που μπορούν να αποσταλούν και να εκτελεστεί απευθείας σε τα διαμερίσματα χώρου αποθήκευσης της βάσης δεδομένων 

Συνιστάται να γρήγορα αποτελέσματα, παρακολουθώντας βίντεο που ακολουθεί, όπου Liu Ανδρέας παρέχει μια σύντομη εισαγωγή στις μοντέλο προγραμματισμού του DocumentDB πλευρά του διακομιστή βάσης δεδομένων. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Στη συνέχεια, επιστρέψετε σε αυτό το άρθρο, όπου θα μάθετε τις απαντήσεις στις ακόλουθες ερωτήσεις:  

- Πώς μπορώ να συντάξετε ένα μια αποθηκευμένη διαδικασία, έναυσμα ή UDF χρησιμοποιώντας JavaScript;
- Πώς εγγυάται DocumentDB ΟΞΎ;
- Πώς λειτουργούν συναλλαγές στο DocumentDB;
- Τι είναι οι προ-ενεργοποιεί και μετά ενεργοποιεί και πώς να συντάξω ένα;
- Πώς να καταχωρήσετε και να εκτελεί μια αποθηκευμένη διαδικασία, έναυσμα ή UDF σε RESTful, με τη χρήση HTTP;
- Τι είναι διαθέσιμες για τη δημιουργία και εκτέλεση DocumentDB SDK αποθηκευμένων διαδικασιών, εναύσματα και UDF;

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Εισαγωγή στην αποθηκευμένη διαδικασία και προγραμματισμό UDF

Αυτή η προσέγγιση του *"JavaScript ως σύγχρονο ημέρα T-SQL"* ελευθερώνει προγραμματιστές εφαρμογών από το τις πολυπλοκότητες τύπος συστήματος διαφορές και τεχνολογιών σχεσιακό αντικείμενο αντιστοίχισης. Έχει επίσης έναν αριθμό από εγγενή πλεονεκτήματα που μπορεί να χρησιμοποιηθεί για τη Δημιουργία εμπλουτισμένων εφαρμογών:  

-   **Σχετικά με τη διαδικασία λογικής:** JavaScript ως γλώσσα προγραμματισμού υψηλού επιπέδου παρέχει ένα εμπλουτισμένου και οικείο περιβάλλον εργασίας για να εκφράσετε επιχειρηματικής λογικής. Μπορείτε να εκτελέσετε σύνθετες ακολουθίες λειτουργιών πιο κοντά στα δεδομένα.

-   **Μεμονωμένες συναλλαγές:** DocumentDB εγγυάται ότι η εκτέλεση λειτουργιών βάσης δεδομένων μέσα σε μια μεμονωμένη αποθηκευμένη διαδικασία ή έναυσμα είναι ατομικής. Αυτό σας επιτρέπει να μια εφαρμογή συνδυασμός σχετικές εργασίες σε μια μεμονωμένη δέσμη ώστε να είτε όλες τις ολοκληρωθεί με επιτυχία ή καμία από αυτές δεν ολοκληρωθεί με επιτυχία. 

-   **Επιδόσεων:** Το γεγονός ότι JSON εγγενώς έχει αντιστοιχιστεί στο σύστημα τύπο γλώσσας Javascript και είναι επίσης η βασική μονάδα χώρου αποθήκευσης στο DocumentDB επιτρέπει για έναν αριθμό βελτιστοποιήσεις όπως τεμπέλη επέλευσης JSON εγγράφων στο χώρο συγκέντρωσης buffer και καθιστώντας τις διαθέσιμες σε απαιτήσεων για την εκτέλεση κώδικα. Υπάρχουν περισσότερα πλεονεκτήματα επιδόσεων που σχετίζονται με αποστολής επιχειρηματικής λογικής στη βάση δεδομένων:
    -   Δέσμης – τους προγραμματιστές να ομαδοποιήσετε λειτουργίες όπως εισάγει και υποβολή τους μαζικά. Λανθάνων χρόνος κίνηση του δικτύου κόστους και το χώρο αποθήκευσης για να δημιουργήσετε ξεχωριστές συναλλαγές μειώνονται σημαντικά. 
    -   Προ-μεταγλώττισης – DocumentDB precompiles αποθηκευμένες διαδικασίες, εναύσματα και συναρτήσεις που ορίζονται από το χρήστη (UDF) για να αποφύγετε την JavaScript μεταγλώττισης κόστος για κάθε κλήση. Το επιβάρυνσης της δόμησης τον κωδικό byte για τη λογική σχετικά με τη διαδικασία αποσβέννυται μια ελάχιστη τιμή.
    -   Ακολουθίας – πολλές λειτουργίες χρειάζεστε μια πλευρά-εφέ ("έναυσμα") που ενδεχομένως να περιλαμβάνει την κάνοντας ένα ή πολλά λειτουργίες δευτερεύων χώρος αποθήκευσης. Εκτός από ατομικότητα, αυτό είναι περισσότερες performant κατά τη μετακίνηση με το διακομιστή. 
-   **Συμπύκνωση:** Αποθηκευμένες διαδικασίες μπορεί να χρησιμοποιηθεί για την ομαδοποίηση εταιρικής λογικής σε ένα σημείο. Αυτό περιλαμβάνει δύο πλεονεκτήματα:
    -   Προσθέτει ένα επίπεδο αφαίρεσης επάνω από τα ανεπεξέργαστα δεδομένα, βοηθώντας έτσι αρχιτέκτονες δεδομένων να εξελίσσονται εφαρμογές τους, ανεξάρτητα από τα δεδομένα. Αυτό είναι ιδιαίτερα χρήσιμο όταν τα δεδομένα είναι σχήμα χωρίς, λόγω του brittle υποθέσεις που ίσως χρειαστεί να ψήνονται σε μια εφαρμογή, εάν πρέπει να ασχοληθείτε με τα δεδομένα απευθείας.  
    -   Αυτό αφαίρεσης σας επιτρέπει να διατηρήσει ασφαλείς τις δεδομένων τους, απλοποίηση της πρόσβασης από τις δέσμες ενεργειών επιχειρήσεις.  

Τη δημιουργία και εκτέλεση των εναυσμάτων βάσης δεδομένων, αποθηκευμένη διαδικασία και τελεστών προσαρμοσμένο ερώτημα υποστηρίζεται έως το [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)και [προγράμματος-πελάτη SDK](documentdb-sdk-dotnet.md) σε πολλές πλατφόρμες συμπεριλαμβάνοντας .NET, Node.js και JavaScript.

**Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί το [SDK Node.js με υποσχέσεις ερωτήσεις](http://azure.github.io/azure-documentdb-node-q/) ** για την απεικόνιση σύνταξη και η χρήση της αποθηκευμένες διαδικασίες, εναύσματα και UDF.   

## <a name="stored-procedures"></a>Αποθηκευμένες διαδικασίες

### <a name="example-write-a-simple-stored-procedure"></a>Παράδειγμα: Γράψτε μια απλή αποθηκευμένη διαδικασία 
Ας ξεκινήσουμε με μια απλή αποθηκευμένη διαδικασία που επιστρέφει μια απόκριση "Γεια".

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Αποθηκευμένες διαδικασίες καταχωρούνται ανά συλλογή και μπορεί να λειτουργήσει σε οποιοδήποτε έγγραφο και το συνημμένο που είναι παρόντες σε αυτήν τη συλλογή. Το παρακάτω τμήμα κώδικα δείχνει πώς να καταχωρήσετε την helloWorld αποθηκευμένη διαδικασία με μια συλλογή. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Μετά την αποθηκευμένη διαδικασία είναι καταχωρημένος, να το εκτελέσει σε σχέση με τη συλλογή, και να διαβάσετε τα αποτελέσματα πίσω στο πρόγραμμα-πελάτη. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Το αντικείμενο περιβάλλοντος παρέχει πρόσβαση σε όλες τις εργασίες που μπορούν να εκτελεστούν σε DocumentDB χώρου αποθήκευσης, καθώς και πρόσβαση στα αντικείμενα αίτησης και απόκρισης. Σε αυτήν την περίπτωση, χρησιμοποιήσαμε το αντικείμενο απόκρισης για να ορίσετε στο κυρίως σώμα του την απάντηση που έχει σταλεί με τον πελάτη. Για περισσότερες λεπτομέρειες, ανατρέξτε στο [διακομιστή DocumentDB JavaScript τεκμηρίωση SDK](http://azure.github.io/azure-documentdb-js-server/).  

Επιτρέψτε μας ανάπτυξη σε αυτό το παράδειγμα και να προσθέσετε περισσότερα βάσης δεδομένων λειτουργίες που σχετίζονται με την αποθηκευμένη διαδικασία. Αποθηκευμένες διαδικασίες μπορεί να δημιουργία, ενημέρωση, ανάγνωση, ερωτήματος και διαγραφή εγγράφων και συνημμένα μέσα στη συλλογή.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Παράδειγμα: Γράψτε μια αποθηκευμένη διαδικασία για να δημιουργήσετε ένα έγγραφο 
Το επόμενο τμήμα κώδικα δείχνει πώς μπορείτε να χρησιμοποιήσετε το αντικείμενο περιβάλλοντος για να αλληλεπιδράσετε με DocumentDB πόρους.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Αυτή η αποθηκευμένη διαδικασία λαμβάνει ως εισαγωγής documentToCreate, στο κυρίως σώμα του εγγράφου που θα δημιουργηθεί στην τρέχουσα συλλογή. Όλες οι λειτουργίες είναι ασύγχρονης και εξαρτώνται από επιστροφές κλήσης συνάρτησης JavaScript. Η λειτουργία επιστροφής κλήσης έχει δύο παραμέτρους, μία για το αντικείμενο σφάλματος στην περίπτωση που η λειτουργία αποτυγχάνει και μία για το αντικείμενο που δημιουργήθηκε. Μέσα την επιστροφή κλήσης, οι χρήστες μπορούν να είτε χειρίστηκε την εξαίρεση ή εμφανίσουν ένα μήνυμα σφάλματος. Σε περίπτωση που δεν παρέχεται επιστροφή κλήσης και υπάρχει ένα σφάλμα, το περιβάλλον εκτέλεσης DocumentDB παρουσιάσει σφάλμα.   

Στο παραπάνω παράδειγμα, η επιστροφή κλήσης παρουσιάσει σφάλμα εάν η λειτουργία απέτυχε. Διαφορετικά, ορίζει το αναγνωριστικό του εγγράφου που δημιουργήσατε ως σώμα της απόκρισης στο πρόγραμμα-πελάτη. Παρακάτω θα δείτε πώς αυτό αποθηκευμένη διαδικασία εκτελείται με παραμέτρους εισόδου.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Σημειώστε ότι είναι δυνατή η τροποποίηση αυτό αποθηκευμένη διαδικασία για να τραβήξετε ένας πίνακας οργάνων εγγράφου ως είσοδο και να τα δημιουργήσετε από την ίδια εκτέλεση αποθηκευμένη διαδικασία αντί για πολλές αιτήσεις δικτύου για να δημιουργήσετε κάθε μία από αυτές ξεχωριστά. Αυτό μπορεί να χρησιμοποιηθεί για την υλοποίηση ένα πρόγραμμα αποτελεσματική μαζικής εισαγωγής για DocumentDB (που περιγράφεται παρακάτω σε αυτό το πρόγραμμα εκμάθησης).   

Το παράδειγμα περιγράφεται επιδεικνύεται πώς μπορείτε να χρησιμοποιήσετε τις αποθηκευμένες διαδικασίες. Θα ασχοληθούμε με εναύσματα και οι συναρτήσεις που ορίζονται από το χρήστη (UDF) αργότερα στην εκμάθηση.

## <a name="database-program-transactions"></a>Πρόγραμμα συναλλαγές της βάσης δεδομένων
Συναλλαγών σε μια τυπική βάση δεδομένων μπορεί να οριστεί ως ακολουθία λειτουργίες που εκτελούνται ως ενιαία λογική της εργασίας. Κάθε συναλλαγή παρέχει **εγγυήσεις ΟΞΈΟΣ**. ΟΞΎ είναι ένα γνωστό ακρωνύμιο που αντιπροσωπεύει τέσσερις ιδιότητες - ατομικότητα, συνέπεια, απομόνωση και διάρκεια ζωής.  

Συνοπτικά, ατομικότητα εξασφαλίζει ότι όλες τις εργασίες που έχουν γίνει μέσα σε μια συναλλαγή αντιμετωπίζεται ως ενιαία όπου είτε δεσμευτεί όλα ή κανένα. Συνέπειας διασφαλίζει ότι η τα δεδομένα είναι πάντα σε καλή κατάσταση εσωτερικό μήκος συναλλαγές. Απομόνωσης εγγυάται ότι δεν δύο συναλλαγές παρεμβολές μεταξύ – Γενικά, περισσότερες εμπορικές συστήματα παρέχει πολλά επίπεδα απομόνωσης που μπορούν να χρησιμοποιηθούν με βάση τις ανάγκες της εφαρμογής. Διάρκεια ζωής εξασφαλίζει ότι κάθε αλλαγή που έχει ολοκληρωθεί στη βάση δεδομένων θα είναι πάντα παρουσίαση.   

Στο DocumentDB, JavaScript φιλοξενείται στον ίδιο χώρο μνήμης με τη βάση δεδομένων. Ως εκ τούτου, οι αιτήσεις υποβάλλονται εντός αποθηκευμένες διαδικασίες και εναύσματα εκτέλεση στην ίδια εμβέλεια μιας περιόδου λειτουργίας βάσης δεδομένων. Αυτή η δυνατότητα επιτρέπει DocumentDB για να εξασφαλίσετε ΟΞΈΟΣ για όλες τις εργασίες που αποτελούν μέρος της ένα αποθηκευμένη διαδικασία/έναυσμα. Εξετάστε τον ακόλουθο ορισμό αποθηκευμένη διαδικασία:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Αυτή η αποθηκευμένη διαδικασία χρησιμοποιεί συναλλαγές μέσα σε μια εφαρμογή παιχνιδιών σε στοιχεία του εμπορίου μεταξύ των δύο παίκτες με μία λειτουργία. Αποθηκευμένη διαδικασία προσπαθεί να διαβάσει δύο έγγραφα κάθε αντιστοιχεί τα αναγνωριστικά προγράμματος αναπαραγωγής που εισήχθησαν στο ως όρισμα. Εάν βρεθούν και τα δύο έγγραφα player, στη συνέχεια, την αποθηκευμένη διαδικασία ενημερώνει τα έγγραφα ανταλλαγή τα στοιχεία τους. Εάν οποιαδήποτε παρουσιάζονται σφάλματα κατά μήκος του τρόπου, παρουσιάζει μια εξαίρεση JavaScript που ρητά ματαίωση συναλλαγής.

Εάν η συλλογή την αποθηκευμένη διαδικασία είναι καταχωρημένος έναντι είναι μια συλλογή μίας διαμερισμάτων και, στη συνέχεια, η συναλλαγή είναι έχει ρυθμιστεί για όλα τα docuemnts μέσα στη συλλογή. Εάν η συλλογή είναι διαμερίσματα, αποθηκευμένες διαδικασίες εκτελούνται στην εμβέλεια συναλλαγής ενός κλειδιού μόνο διαμερίσματα. Κάθε αποθηκευμένη διαδικασία εκτέλεσης, στη συνέχεια, πρέπει να περιλαμβάνει μια τιμή κλειδιού partition αντιστοιχεί στο πεδίο που πρέπει να εκτελέσετε τη συναλλαγή στην περιοχή. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Δημιουργία διαμερισμάτων DocumentDB](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Ολοκλήρωση και επαναφοράς
Συναλλαγές βάθος και εγγενώς να ενσωματωθούν σε μοντέλο προγραμματισμού του DocumentDB JavaScript. Μέσα σε μια συνάρτηση JavaScript, όλες οι λειτουργίες περιστρέφονται αυτόματα στην περιοχή μία συναλλαγή. Εάν το JavaScript ολοκληρωθεί χωρίς κάθε εξαίρεση, οι λειτουργίες στη βάση δεδομένων είναι δεσμευμένη. Στην πραγματικότητα, οι καταστάσεις "ΣΥΝΑΛΛΑΓΉ ΞΕΚΙΝΉΣΕΤΕ" και "ΟΛΟΚΛΉΡΩΣΗ ΣΥΝΑΛΛΑΓΉ" σε σχεσιακές βάσεις δεδομένων είναι έμμεσα στο DocumentDB.  
 
Εάν υπάρχει κάθε εξαίρεση που αντιγράφεται από τη δέσμη ενεργειών, χρόνου εκτέλεσης του DocumentDB JavaScript θα επαναφέρει τη ολόκληρη συναλλαγή. Όπως φαίνεται στο προηγούμενο παράδειγμα, εμφάνιση εξαίρεσης είναι ουσιαστικά ισοδύναμο με "ΕΠΑΝΑΦΟΡΆ ΣΥΝΑΛΛΑΓΉ" DocumentDB.
 
### <a name="data-consistency"></a>Συνέπεια δεδομένων
Αποθηκευμένες διαδικασίες και εναύσματα εκτελούνται πάντα στο πρωτεύον αντίγραφο της συλλογής DocumentDB. Αυτό εξασφαλίζει ότι διαβάζει από μέσα σε αποθηκευμένες διαδικασίες προσφορά ισχυρό συνέπειας. Τα ερωτήματα που χρησιμοποιούν συναρτήσεις που ορίζονται από το χρήστη μπορεί να εκτελεστεί σε των κύριων ή οποιοδήποτε δευτερεύον ρεπλίκα, αλλά θα σας βεβαιωθείτε να πληροί το επίπεδο ζητήθηκε συνέπειας, επιλέγοντας την κατάλληλη ρεπλίκα.

## <a name="bounded-execution"></a>Όρια εκτέλεσης
Πρέπει να ολοκληρώσετε όλες οι λειτουργίες DocumentDB μέσα στο διακομιστή που καθορίζεται αίτηση διάρκεια χρονικού ορίου. Αυτός ο περιορισμός ισχύει επίσης για λειτουργίες JavaScript (αποθηκευμένες διαδικασίες, εναύσματα και συναρτήσεων που ορίζονται από το χρήστη). Εάν δεν ολοκληρωθεί μια λειτουργία με την προθεσμία, η συναλλαγή ακυρώνεται. Λειτουργίες JavaScript πρέπει να ολοκληρωθεί εντός του χρονικού ορίου ή να υλοποιήσετε ένα μοντέλο συνέχισης που βασίζεται σε δέσμη/βιογραφικού σημειώματος εκτέλεσης.  

Για να απλοποιήσετε ανάπτυξη αποθηκευμένες διαδικασίες και εναύσματα να χειρίζεται τις προθεσμίες, όλες τις συναρτήσεις κάτω από το αντικείμενο συλλογής (για δημιουργία, ανάγνωση, αντικατάσταση και η διαγραφή των εγγράφων και συνημμένα) επιστρέφουν μια τιμή Boolean που υποδεικνύει εάν θα ολοκληρωθεί αυτή η λειτουργία. Εάν αυτή η τιμή είναι false, είναι ένδειξη ότι πρόκειται να λήξει η προθεσμία και ότι η διαδικασία πρέπει να αναδιπλώνεται εκτέλεσης προς τα επάνω.  Λειτουργίες σε ουρά πριν από την πρώτη λειτουργία δεν έγινε αποδεκτή χώρου αποθήκευσης είναι εγγυημένη για να ολοκληρωθεί εάν την αποθηκευμένη διαδικασία ολοκληρώνεται σε χρόνο και όχι σε ουρά οποιαδήποτε περισσότερες αιτήσεις.  

Λειτουργίες JavaScript είναι επίσης δεσμεύεται για την κατανάλωση πόρων. DocumentDB διατηρεί μετάδοσης ανά συλλογή με βάση την προμήθεια του φακέλου μέγεθος ενός λογαριασμού βάσης δεδομένων. Μετάδοσης εκφράζεται σε μια κανονικοποιημένη μονάδα CPU, μνήμης και κατανάλωση εισόδου/ΕΞΌΔΟΥ που ονομάζεται αίτηση μονάδες ή RUs. Λειτουργίες JavaScript ενδεχομένως να χρησιμοποιήσετε του μεγάλου αριθμού RUs μέσα σε μια σύντομη ώρα και μπορεί να λάβετε επιτόκιο-περιορισμένη εάν η συλλογή όριο. Επίσης μπορεί να είναι σε καραντίνα πόρων εντατική αποθηκευμένες διαδικασίες για να βεβαιωθείτε ότι διαθεσιμότητα λειτουργιών στοιχειώδεις βάσης δεδομένων.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Παράδειγμα: Μαζική εισαγωγή δεδομένων σε ένα πρόγραμμα βάσης δεδομένων
Ακολουθεί ένα παράδειγμα μιας αποθηκευμένης διαδικασίας που έχει συνταχθεί σε μαζικής εισαγωγής εγγράφων σε μια συλλογή. Σημειώστε τον τρόπο την αποθηκευμένη διαδικασία χειρισμού όρια εκτέλεσης, επιλέγοντας τη δυαδική τιμή τιμή επιστροφής από createDocument, και, στη συνέχεια, να χρησιμοποιεί το πλήθος των εγγράφων που έχουν εισαχθεί σε κάθε κλήση της αποθηκευμένης διαδικασίας να παρακολουθείτε και να συνεχίσετε την πρόοδο σε δέσμες.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Εναύσματα βάσης δεδομένων
### <a name="database-pre-triggers"></a>Προ-εναύσματα βάσης δεδομένων
DocumentDB παρέχει εναύσματα που έχουν εκτελεστεί ή ενεργοποιηθεί από μια λειτουργία σε ένα έγγραφο. Για παράδειγμα, μπορείτε να καθορίσετε ένα προ-έναυσμα όταν δημιουργείτε ένα έγγραφο – αυτό το προ-έναυσμα θα εκτελείται πριν από τη δημιουργία του εγγράφου. Ακολουθεί ένα παράδειγμα του πώς προ-εναύσματα μπορούν να χρησιμοποιηθούν για την επικύρωση των ιδιοτήτων ενός εγγράφου που έχει δημιουργηθεί:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Και το αντίστοιχο κώδικα εγγραφής πλευρά του προγράμματος-πελάτη Node.js για το έναυσμα:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Προ-εναύσματα δεν μπορεί να έχει οποιεσδήποτε παραμέτρους εισόδου. Το αντικείμενο request μπορεί να χρησιμοποιηθεί για να χειρίζεστε το μήνυμα αίτησης που σχετίζεται με τη λειτουργία. Εδώ, το προ-έναυσμα εκτελείται με τη δημιουργία ενός εγγράφου και το σώμα του μηνύματος αίτησης περιέχει το έγγραφο να δημιουργηθούν σε μορφή JSON.   

Όταν έχετε καταχωρήσει εναύσματα, οι χρήστες μπορούν να καθορίσετε τις λειτουργίες που μπορεί να εκτελεστεί με. Αυτό το έναυσμα δημιουργήθηκε με TriggerOperation.Create, γεγονός που σημαίνει ότι τα ακόλουθα δεν επιτρέπεται.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Εναύσματα μετά τη βάση δεδομένων
Μετά τη εναύσματα, όπως προ-εναύσματα, συσχετίζονται με μια εργασία σε ένα έγγραφο και δεν λαμβάνουν οποιεσδήποτε παραμέτρους εισόδου. Μπορούν εκτελεστεί **μετά** την ολοκλήρωση της λειτουργίας και έχετε πρόσβαση στο μήνυμα απάντησης που αποστέλλονται στον υπολογιστή-πελάτη.   

Το παρακάτω παράδειγμα εμφανίζει μετά τη εναύσματα σε δράση:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Το έναυσμα μπορεί να καταχωρηθεί, όπως φαίνεται στο ακόλουθο δείγμα.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Αυτό το έναυσμα ερωτήματα για το έγγραφο μετα-δεδομένων και ενημερώνει με λεπτομέρειες σχετικά με το έγγραφο που μόλις δημιουργήθηκε.  

Ένα σημείο που είναι σημαντικό να θυμάστε είναι το **συναλλαγών** εκτέλεσης εναυσμάτων στο DocumentDB. Αυτό το έναυσμα μετά τη εκτελείται ως μέρος της ίδιας πράξης με τη δημιουργία του αρχικού εγγράφου. Επομένως, εάν θα σας να εμφανίσουν εξαίρεση από το μετά τη έναυσμα (Καλώς Εάν δεν είναι δυνατή η ενημέρωση του εγγράφου μετα-δεδομένα), ολόκληρη η συναλλαγή θα αποτύχει και να επανέλθει. Δεν υπάρχει έγγραφο θα δημιουργηθεί και θα επιστρέψει μια εξαίρεση.  

##<a id="udf"></a>Συναρτήσεις που ορίζονται από το χρήστη
Συναρτήσεις που ορίζονται από το χρήστη (UDF) που χρησιμοποιούνται για να επεκτείνετε τη γραμματική γλώσσας ερωτήματος DocumentDB SQL και υλοποίηση της λογικής προσαρμοσμένων επιχειρηματικών. Μόνο μπορεί να ονομάζεται από μέσα σε ερωτήματα. Αυτές δεν έχουν πρόσβαση στο αντικείμενο περιβάλλοντος και έχουν σχεδιαστεί ώστε να χρησιμοποιηθεί ως μόνο για υπολογισμού JavaScript. Επομένως, UDF μπορούν να εκτελεστούν σε δευτερεύοντα αντίγραφα της υπηρεσίας DocumentDB.  
 
Το παρακάτω παράδειγμα δημιουργεί μια UDF για τον υπολογισμό του φόρου εισοδήματος βάσει χρεώσεις για διάφορα έσοδα αγκύλες και στη συνέχεια, να το χρησιμοποιεί μέσα σε ένα ερώτημα για να βρείτε όλα τα άτομα που καταβάλλονται περισσότερα από 20.000 δολάρια φόρων.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


Στη συνέχεια μπορεί να χρησιμοποιηθεί το UDF σε ερωτήματα όπως στο ακόλουθο δείγμα:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>Ερώτημα ενοποιημένη γλώσσα JavaScript API
Εκτός από την έκδοση τα ερωτήματα που χρησιμοποιούν σύνταξη SQL του DocumentDB, στο διακομιστή SDK σάς επιτρέπει να εκτελέσετε βελτιστοποιημένη ερωτημάτων με το περιβάλλον fluent JavaScript χωρίς καμία γνώση του SQL. Το ερώτημα JavaScript API σάς επιτρέπει να μέσω προγραμματισμού δημιουργία ερωτημάτων με τη διέλευση κατηγορήματος συναρτήσεις σε συνάρτηση chainable κλήσεις, με μια σύνταξη οικεία για ενσωματωμένα πρόσθετα πίνακα και τις δημοφιλείς βιβλιοθήκες JavaScript όπως lodash του ECMAScript5. Τα ερωτήματα που αναλύονται από το χρόνο εκτέλεσης JavaScript για να εκτελεστεί αποτελεσματικά με χρήση του DocumentDB δείκτες.

> [AZURE.NOTE]`__` (διπλό-χαρακτήρας υπογράμμισης) είναι ένα ψευδώνυμο στο `getContext().getCollection()`.
> <br/>
> Με άλλα λόγια, μπορείτε να χρησιμοποιήσετε `__` ή `getContext().getCollection()` για να αποκτήσετε πρόσβαση του ερωτήματος JavaScript API.

Υποστηριζόμενες συναρτήσεις περιλαμβάνουν τα εξής:
<ul>
<li>
<b>chain().... τιμή ([επιστροφής κλήσης] [, επιλογές])</b>
<ul>
<li>
Ξεκινά μια κλήση συνδεδεμένες που πρέπει να τελειώνει με value().
</li>
</ul>
</li>
<li>
<b>φίλτρο (predicateFunction [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Φιλτράρει τα δεδομένα εισόδου χρησιμοποιώντας μια συνάρτηση κατηγορήματος η οποία επιστρέφει την τιμή true/false, για να φιλτράρετε μεταβίβασης/ανάληψης ελέγχου εισαγωγής έγγραφα στο σύνολο που προκύπτει. Αυτό συμπεριφέρεται παρόμοια με έναν όρο WHERE σε SQL.
</li>
</ul>
</li>
<li>
<b>χάρτης (transformationFunction [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Εφαρμογή μιας προβολής που δίνεται μια συνάρτηση μετασχηματισμό, η οποία αντιστοιχίζει κάθε στοιχείο εισαγωγής σε ένα αντικείμενο JavaScript ή μια τιμή. Αυτή η συνάρτηση συμπεριφέρεται παρόμοια με έναν όρο SELECT στο SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Αυτή είναι μια συντόμευση για το χάρτη που εξάγει την τιμή της ιδιότητας μόνο από κάθε στοιχείο εισαγωγής.
</li>
</ul>
</li>
<li>
<b>ισοπέδωση ([isShallow] [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Συνδυάζει και ισοπεδώνει πίνακες από κάθε στοιχείο εισαγωγής να μόνο έναν πίνακα. Αυτό συμπεριφέρεται παρόμοια με SelectMany στο LINQ.
</li>
</ul>
</li>
<li>
<b>μενού ταξινόμησης κατά ([κατηγόρημα] [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Δημιουργία ενός νέου συνόλου εγγράφων κατά την ταξινόμηση των εγγράφων στη ροή εισαγωγής εγγράφου σε αύξουσα σειρά χρησιμοποιώντας τη δεδομένη κατηγόρημα. Αυτό συμπεριφέρεται παρόμοια με έναν όρο ORDER BY στο SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([κατηγόρημα] [, επιλογές] [, επιστροφής κλήσης])</b>
<ul>
<li>
Αγροτικά προϊόντα ενός νέου συνόλου εγγράφων με την ταξινόμηση των εγγράφων στη ροή εισαγωγής εγγράφου σε φθίνουσα σειρά χρησιμοποιώντας τη δεδομένη κατηγόρημα. Αυτό συμπεριφέρεται παρόμοια με έναν όρο ORDER BY x DESC σε SQL.
</li>
</ul>
</li>
</ul>


Όταν περιλαμβάνεται εντός συναρτήσεις κατηγόρημα ή/και το κουμπί επιλογής, τις παρακάτω δομές JavaScript αυτόματα λήψη βελτιστοποιημένη για να εκτελέσετε απευθείας σε DocumentDB δείκτες:

* Απλή τελεστές: =-* / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Λεκτικές σταθερές, συμπεριλαμβανομένου του αντικειμένου ακριβές: {}
* VAR, επιστροφής

Τις παρακάτω δομές JavaScript δεν να βελτιστοποιηθεί για τους δείκτες DocumentDB:

* Έλεγχος ροής (π.χ. Εάν, για, ενώ)
* Συνάρτηση κλήσεις

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα μας [JSDocs πλευρά του διακομιστή](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Παράδειγμα: Γράψτε μια αποθηκευμένη διαδικασία χρησιμοποιώντας το ερώτημα JavaScript API

Το παρακάτω δείγμα κώδικα είναι ένα παράδειγμα του πώς μπορεί να χρησιμοποιηθεί το API ερωτήματος JavaScript στο περιβάλλον μιας αποθηκευμένης διαδικασίας. Αποθηκευμένη διαδικασία εισάγει ένα έγγραφο, με μια παράμετρο εισόδου, και ενημερώνει ένα μετα-δεδομένα εγγράφων, χρησιμοποιώντας το `__.filter()` μέθοδο, με minSize, maxSize και totalSize βασισμένου ιδιότητα size του εγγράφου εισαγωγής.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL για να σκονάκι Javascript
Ο παρακάτω πίνακας παρουσιάζει διάφορα ερωτήματα SQL και το αντίστοιχο ερωτήματα JavaScript.

Όπως με τα ερωτήματα SQL, εγγράφων ιδιότητα πλήκτρα (π.χ. `doc.id`) διάκριση πεζών-κεφαλαίων.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>Ερώτημα JavaScript API</th>
<th>Λεπτομέρειες</th>
</tr>
<tr>
<td>
<pre>
ΕΠΙΛΈΞΤΕ * από έγγραφα
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {επιστροφής doc;});
</pre>
</td>
<td>Αποτελέσματα σε όλα τα έγγραφα (σελιδοποίηση με διακριτικό συνέχισης) ως είναι.</td>
</tr>
<tr>
<td>
<pre>
ΕΠΙΛΈΞΤΕ docs.id, docs.message ΩΣ μήνυμα λάθους, docs.actions από έγγραφα
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {επιστροφής {αναγνωριστικό: doc.id, το μήνυμα: doc.message, ενέργειες: doc.actions};});
</pre>
</td>
<td>Προβάλλει το αναγνωριστικό μηνύματος (δημιουργήθηκε ψευδώνυμο στο μήνυμα λάθους) και ενέργεια από όλα τα έγγραφα.</td>
</tr>
<tr>
<td>
<pre>
ΕΠΙΛΈΞΤΕ * από έγγραφα docs.id="X998_Y998 ΘΈΣΗ"
</pre>
</td>
<td>
<pre>
__.Filter(Function(doc) {επιστροφής doc.id === "X998_Y998"});
</pre>
</td>
<td>Ερωτήματα για έγγραφα με το κατηγόρημα: αναγνωριστικό = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
ΕΠΙΛΈΞΤΕ * από έγγραφα όπου ARRAY_CONTAINS(docs. Ετικέτες, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {επιστροφής x.Tags & & x.Tags.indexOf(123) > -1;});
</pre>
</td>
<td>Τα ερωτήματα για τα έγγραφα που έχετε μια ιδιότητα ετικέτες και οι ετικέτες είναι ένας πίνακας που περιέχει την τιμή 123.</td>
</tr>
<tr>
<td>
<pre>
ΕΠΙΛΈΞΤΕ docs.id, docs.message ΩΣ έγγραφα από το μήνυμα docs.id="X998_Y998 ΘΈΣΗ"
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {επιστροφής doc.id === "X998_Y998"}) .map(function(doc) {επιστροφής {αναγνωριστικό: doc.id, το μήνυμα: doc.message};}) .value();
</pre>
</td>
<td>Ερωτήματα για έγγραφα με κατηγορήματος, αναγνωριστικό = "X998_Y998" και έργα, στη συνέχεια, το αναγνωριστικό και το μήνυμα (με ψευδώνυμο για το μήνυμα).</td>
</tr>
<tr>
<td>
<pre>
Προσθήκη ετικετών σε έγγραφα από ετικέτα ΕΠΙΛΈΞΤΕ ΤΙΜΉ ΣΥΜΜΕΤΟΧΉ σε έγγραφα. Ετικέτες ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.chain() .filter(function(doc) {επιστροφής έγγραφο. Ετικέτες & & Array.isArray (doc. Ετικέτες); {επιστροφής doc._ts;}}) .sortBy(function(doc)) .pluck("Tags") .flatten() .value()
</pre>
</td>
<td>Φίλτρα για έγγραφα που έχουν μια ιδιότητα πίνακα, ετικετών, και ταξινομεί τα έγγραφα που προκύπτει από την ιδιότητα _ts χρονικής σήμανσης συστήματος, και, στη συνέχεια, έργα + ισοπεδώνει ετικέτες έναν πίνακα.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Υποστήριξη χρόνου εκτέλεσης
[DocumentDB JavaScript για διακομιστή SDK](http://azure.github.io/azure-documentdb-js-server/) παρέχει υποστήριξη για τις δυνατότητες των τη βασική δυνατότητες γλωσσών JavaScript ως τυποποιημένη από [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Ασφάλεια
Αποθηκευμένες διαδικασίες της JavaScript και εναύσματα είναι με φίλτρο, έτσι ώστε τα εφέ μία δέσμη ενεργειών δεν προκαλέσει απώλεια στην άλλη χωρίς να μεταβείτε μέσω της απομόνωσης συναλλαγής στιγμιότυπου στο επίπεδο της βάσης δεδομένων. Τα περιβάλλοντα χρόνου εκτέλεσης είναι συγκεντρωμένες αλλά εκκαθάριση του περιβάλλοντος μετά από κάθε εκτέλεση. Ως εκ τούτου είναι εγγυημένη ασφαλή από τα ανεπιθύμητα εφέ πλευρά μεταξύ τους.

### <a name="pre-compilation"></a>Προ-μεταγλώττισης
Αποθηκευμένες διαδικασίες, εναύσματα και UDF είναι μη ρητά προ-μεταγλωττισμένη στη μορφή κώδικα byte για να αποφύγετε μεταγλώττισης κόστος κατά τη διάρκεια του κάθε κλήσης δέσμης ενεργειών. Αυτό εξασφαλίζει κλήσεων των αποθηκευμένων διαδικασιών είναι γρήγορη και έχουν μια χαμηλή αποτύπωμα.

## <a name="client-sdk-support"></a>Υποστήριξη προγράμματος-πελάτη SDK
Εκτός από το πρόγραμμα-πελάτη [Node.js](documentdb-sdk-node.md) , DocumentDB υποστηρίζει [.NET](documentdb-sdk-dotnet.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/)και [Python SDK](documentdb-sdk-python.md). Αποθηκευμένες διαδικασίες, εναύσματα και UDF μπορούν να δημιουργηθούν και να εκτελεστεί χρησιμοποιώντας οποιαδήποτε από αυτές τις SDK καθώς και. Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να δημιουργήσετε και να εκτελέσετε μια αποθηκευμένη διαδικασία χρησιμοποιώντας το πρόγραμμα-πελάτη .NET. Παρατηρήστε πώς οι τύποι .NET είναι περνά στο την αποθηκευμένη διαδικασία ως JSON και ανάγνωση.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Αυτό το δείγμα δείχνει πώς μπορείτε να χρησιμοποιήσετε το [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) για να δημιουργήσετε ένα προ- έναυσμα και να δημιουργήσετε ένα έγγραφο με το έναυσμα με δυνατότητα. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Και το παρακάτω παράδειγμα εμφανίζει τον τρόπο δημιουργίας μιας συνάρτησης που ορίζονται από το χρήστη (UDF) και να το χρησιμοποιήσετε σε ένα [ερώτημα DocumentDB SQL](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API
Όλες οι λειτουργίες DocumentDB μπορεί να εκτελεστεί σε RESTful. Αποθηκευμένες διαδικασίες, εναύσματα και συναρτήσεις που ορίζονται από το χρήστη να έχει εγγραφεί κάτω από μια συλλογή με τη χρήση HTTP POST. Ακολουθεί ένα παράδειγμα του τρόπου για να καταχωρήσετε μια αποθηκευμένη διαδικασία:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Αποθηκευμένη διαδικασία έχει καταχωρηθεί με την εκτέλεση μιας αίτησης POST σε σχέση με το URI Demand dbs/testdb/colls/testColl/sprocs με το κυρίως κείμενο που περιέχει την αποθηκευμένη διαδικασία για να δημιουργήσετε. Εναύσματα και UDF μπορεί να καταχωρηθεί παρόμοια με την έκδοση μια ΔΗΜΟΣΊΕΥΣΗ σε σχέση με /triggers και /udfs αντίστοιχα.
Αυτό αποθηκευμένη διαδικασία μπορούν στη συνέχεια να εκτελεστεί με την έκδοση μια αίτηση POST σε σχέση με τη σύνδεση πόρων:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Εδώ, την είσοδο στην αποθηκευμένη διαδικασία μεταβιβάζεται στο σώμα της αίτησης. Σημειώστε ότι τα δεδομένα εισόδου μεταβιβάζεται ως ένας πίνακας JSON παραμέτρους εισόδου. Την αποθηκευμένη διαδικασία λαμβάνει την πρώτη είσοδο ως ένα έγγραφο που είναι οργανισμός απόκρισης. Η απόκριση λαμβάνουμε είναι ως εξής:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Εναύσματα, σε αντίθεση με αποθηκευμένες διαδικασίες, δεν είναι δυνατό να εκτελεστεί απευθείας. Αντί για αυτό εκτελούνται ως μέρος μιας λειτουργίας σε ένα έγγραφο. Θα σας να καθορίσετε τα εναύσματα για να εκτελέσετε με μια αίτηση χρησιμοποιώντας κεφαλίδες HTTP. Τα παρακάτω είναι αίτηση για να δημιουργήσετε ένα έγγραφο.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Δείτε το προ-έναυσμα για να εκτελεστεί με την πρόσκληση σε καθορίζεται στην κεφαλίδα x-ms-documentdb-pre-trigger-include. Αντίστοιχα, οποιαδήποτε μετά τη εναύσματα δίνεται στην κεφαλίδα x-ms-documentdb-post-trigger-include. Λάβετε υπόψη ότι παλαιοί και νέοι εναύσματα μπορεί να καθοριστεί για μια δεδομένη αίτηση.

## <a name="sample-code"></a>Δείγμα κώδικα

Μπορείτε να βρείτε περισσότερα παραδείγματα κώδικα διακομιστή (όπως [μαζικής διαγραφής](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)και [Ενημέρωση](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) σε μας [Github αποθετήριο](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Θέλετε να κάνετε κοινή χρήση του εκπληκτική αποθηκευμένη διαδικασία; Στείλτε μας αίτησης Ελκυστική! 

## <a name="next-steps"></a>Επόμενα βήματα

Όταν έχετε μία ή περισσότερες αποθηκευμένες διαδικασίες, εναύσματα και συναρτήσεις που ορίζονται από το χρήστη που δημιουργήσατε, μπορείτε να τις φορτώσετε και προβολή στην πύλη του Azure χρησιμοποιώντας την Εξερεύνηση των δεσμών ενεργειών. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Προβολή αποθηκευμένες διαδικασίες, εναύσματα, και συναρτήσεις που ορίζονται από το χρήστη, χρησιμοποιώντας την Εξερεύνηση δέσμης ενεργειών DocumentDB](documentdb-view-scripts.md).

Μπορείτε επίσης να βρείτε τις παρακάτω αναφορές και τους πόρους χρήσιμες στη διαδρομή σας για να μάθετε περισσότερα για τον προγραμματισμό DocumentDB διακομιστή:

- [Azure DocumentDB SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON τύπος συστήματος](http://www.json.org/js.html) 
- [Επεκτασιμότητα του ασφαλούς και φορητό βάσης δεδομένων](http://dl.acm.org/citation.cfm?id=276339) 
- [Υπηρεσία προσανατολισμένος αρχιτεκτονική βάσης δεδομένων](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Διατηρεί το περιβάλλον εκτέλεσης .NET στον Microsoft SQL server](http://dl.acm.org/citation.cfm?id=1007669)

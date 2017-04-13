<properties
   pageTitle="Ανάπτυξη με πολλές περιοχές στο DocumentDB | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αποκτήσετε πρόσβαση δεδομένων σε πολλές περιοχές από DocumentDB Azure, μια πλήρως διαχειριζόμενη υπηρεσία NoSQL βάσεων δεδομένων."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>Ανάπτυξη με λογαριασμούς DocumentDB πολλών περιοχών

> [AZURE.NOTE] Καθολικό κατανομή των βάσεων δεδομένων DocumentDB είναι γενικά διαθέσιμο και ενεργοποιηθεί αυτόματα για οποιονδήποτε λογαριασμό DocumentDB που έχουν δημιουργηθεί πρόσφατα. Προσπαθούμε να ενεργοποιήσετε την καθολική διανομή σε όλους τους λογαριασμούς υπάρχουσα, αλλά στο μεταξύ, εάν θέλετε το καθολικό κατανομή με δυνατότητα στο λογαριασμό σας, επικοινωνήστε [Επικοινωνήστε με την υποστήριξη](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) και θα σας θα ενεργοποιήσετε για εσάς τώρα.

Προκειμένου να εκμεταλλευτείτε [καθολικού διανομής](documentdb-distribute-data-globally.md), εφαρμογές προγράμματος-πελάτη να καθορίσετε τη λίστα ταξινομημένη προτίμηση των περιοχών που θα χρησιμοποιηθεί για την εκτέλεση λειτουργιών εγγράφου. Αυτό μπορεί να γίνει με τη ρύθμιση της πολιτικής σύνδεσης. Με βάση τη ρύθμιση παραμέτρων λογαριασμού Azure DocumentDB, τρέχουσα διαθεσιμότητά της τοπικές ρυθμίσεις και τη λίστα προτίμηση που καθορίζονται, το τελικό σημείο πιο βέλτιστη επιλέγονται από το SDK για να εκτελέσετε εγγραφής και λειτουργίες ανάγνωσης. 

Αυτή η λίστα προτίμηση ορίζεται κατά την προετοιμασία μια σύνδεση χρησιμοποιώντας το πρόγραμμα-πελάτη DocumentDB SDK. Το SDK αποδεχτείτε μια προαιρετική παράμετρος "PreferredLocations" που είναι μια ταξινομημένη λίστα Azure περιοχές.

Το SDK θα στείλει αυτόματα όλες οι εγγραφές με την τρέχουσα εγγραφή περιοχής. 

Στην πρώτη διαθέσιμη περιοχή στη λίστα PreferredLocations θα αποσταλούν όλες διαβάζει. Εάν η αίτηση αποτύχει, ο υπολογιστής-πελάτης θα αποτύχει προς τα κάτω στη λίστα στην επόμενη περιοχή, και ούτω καθεξής. 

Το πρόγραμμα-πελάτη SDK μόνο θα επιχειρήσει να διαβάζετε από τις περιοχές καθορίζονται στο PreferredLocations. Επομένως, για παράδειγμα, εάν ο λογαριασμός βάσης δεδομένων είναι διαθέσιμη σε τρεις περιοχές, αλλά ο υπολογιστής-πελάτης μόνο καθορίζει δύο από τις περιοχές-εγγραφής για PreferredLocations, στη συνέχεια, δεν διαβάζει θα είναι που σερβιρίστηκε εκτός της περιοχής εγγραφής, ακόμα και στην περίπτωση ανακατεύθυνσης.

Η εφαρμογή μπορεί να επαληθεύσετε το τελικό σημείο της τρέχουσας εγγραφής και διαβάστε επιλέξει από το SDK, επιλέγοντας δύο ιδιότητες, WriteEndpoint και ReadEndpoint, που είναι διαθέσιμες στην έκδοση SDK 1.8 και επάνω από το τελικό σημείο. 

Εάν δεν έχει οριστεί η ιδιότητα PreferredLocations, θα είναι εξυπηρετήσει όλες τις αιτήσεις από την περιοχή της τρέχουσας εγγραφής. 


## <a name="net-sdk"></a>.NET SDK
Το SDK μπορεί να χρησιμοποιηθεί χωρίς αλλαγές κώδικα. Σε αυτήν την περίπτωση, το SDK αυτόματα κατευθύνει δύο διαβάζει και εγγράφει της τρέχουσας περιοχής εγγραφής. 

Στην έκδοση 1,8 και νεότερη έκδοση του .NET SDK, η παράμετρος ConnectionPolicy για την κατασκευή DocumentClient έχει μια ιδιότητα που ονομάζεται Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations. Αυτή η ιδιότητα είναι τύπου συλλογής `<string>` και θα πρέπει να περιέχει μια λίστα με ονόματα περιοχής. Οι τιμές συμβολοσειράς μορφοποιούνται ανά στήλη όνομα περιοχής σε [Περιοχές Azure]  [ regions] σελίδα, χωρίς κενά διαστήματα πριν ή μετά την πρώτη και τον τελευταίο χαρακτήρα αντίστοιχα.

Η τρέχουσα εγγραφή και διαβάστε τελικά σημεία θα είναι διαθέσιμες στο DocumentClient.WriteEndpoint και DocumentClient.ReadEndpoint αντίστοιχα.

> [AZURE.NOTE] Οι διευθύνσεις URL για τα τελικά σημεία δεν πρέπει να θεωρείται ως μεγάλης διάρκειας σταθερές. Η υπηρεσία μπορεί να ενημερώσει αυτές σε οποιοδήποτε σημείο. Το SDK χειρίζεται αυτόματα αυτή η αλλαγή.

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS, JavaScript και Python SDK
Το SDK μπορεί να χρησιμοποιηθεί χωρίς αλλαγές κώδικα. Σε αυτήν την περίπτωση, το SDK αυτόματα θα άμεσες τόσο ανάγνωσης και εγγραφής με την τρέχουσα εγγραφή περιοχής. 

Στην έκδοση 1,8 και νεότερες εκδόσεις του κάθε SDK, η παράμετρος ConnectionPolicy για την κατασκευή DocumentClient μια νέα ιδιότητα που ονομάζεται DocumentClient.ConnectionPolicy.PreferredLocations. Αυτή είναι η παράμετρος είναι ένας πίνακας συμβολοσειρών που τίθεται σε μια λίστα με ονόματα περιοχής. Τα ονόματα έχουν μορφοποιηθεί ανά στήλη όνομα περιοχής στις [Περιοχές Azure]  [ regions] σελίδας. Μπορείτε επίσης να χρησιμοποιήσετε τις προκαθορισμένες σταθερές στο αντικείμενο ευκολία AzureDocuments.Regions

Η τρέχουσα εγγραφή και διαβάστε τελικά σημεία θα είναι διαθέσιμες στο DocumentClient.getWriteEndpoint και DocumentClient.getReadEndpoint αντίστοιχα.

> [AZURE.NOTE] Οι διευθύνσεις URL για τα τελικά σημεία δεν πρέπει να θεωρείται ως μεγάλης διάρκειας σταθερές. Η υπηρεσία μπορεί να ενημερώσει αυτές σε οποιοδήποτε σημείο. Το SDK θα χειρίζεται αυτόματα αυτή η αλλαγή.

Ακολουθεί ένα παράδειγμα κώδικα για NodeJS/Javascript. Python και Java θα ακολουθούν το ίδιο μοτίβο.

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>ΥΠΌΛΟΙΠΟ 
Όταν ένα λογαριασμό της βάσης δεδομένων έχει κάνει διαθέσιμες σε πολλές περιοχές, προγράμματα-πελάτες να υποβάλετε ερώτημα τη διαθεσιμότητά της, εκτελώντας ένα αίτημα GET στο παρακάτω URI.

    https://{databaseaccount}.documents.azure.com/

Η υπηρεσία θα επιστρέψει μια λίστα με τις περιοχές και τις αντίστοιχες DocumentDB τελικού σημείου URIs για τα αντίγραφα. Τρέχουσα περιοχή εγγραφής υποδεικνύεται στην απάντηση. Το πρόγραμμα-πελάτη, στη συνέχεια, να επιλέξετε το κατάλληλο τελικό σημείο για περαιτέρω αιτήσεις REST API ως εξής.

Παράδειγμα απόκρισης

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   Πρέπει να μεταβείτε αιτήσεις όλα ΤΟΠΟΘΈΤΗΣΗ, ΔΗΜΟΣΊΕΥΣΗ και η ΔΙΑΓΡΑΦΉ του καθορισμένου εγγραφής URI
-   Λαμβάνει όλα και άλλες αιτήσεις μόνο για ανάγνωση (για παράδειγμα ερωτήματα) μπορεί να οδηγεί σε οποιοδήποτε τελικό σημείο της επιλογής του υπολογιστή-πελάτη

Η εγγραφή θα αποτύχει προσκλήσεις σε περιοχές μόνο για ανάγνωση με κωδικό σφάλματος HTTP 403 ("απαγορεύεται").

Εάν η περιοχή εγγραφής αλλάζει μετά τη φάση αρχικό εντοπισμού του υπολογιστή-πελάτη, οι επόμενες εγγραφές στην προηγούμενη περιοχή εγγραφής θα αποτύχει με κωδικό σφάλματος HTTP 403 ("απαγορεύεται"). Το πρόγραμμα-πελάτη, στη συνέχεια, θα πρέπει να ΛΆΒΕΤΕ τη λίστα των περιοχών ξανά για να μεταβείτε στην περιοχή ενημερωμένη εγγραφή.

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τη διανομή δεδομένων καθολικά με DocumentDB στα ακόλουθα άρθρα:

- [Διανομή δεδομένων καθολικά με DocumentDB](documentdb-distribute-data-globally.md)
- [Επίπεδα συνέπειας](documentdb-consistency-levels.md)
- [Πώς λειτουργεί η απόδοση με πολλές περιοχές](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [Προσθέστε τις περιοχές με την πύλη Azure](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 

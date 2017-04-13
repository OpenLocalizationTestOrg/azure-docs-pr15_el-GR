<properties
    pageTitle="Γρήγορα αποτελέσματα με το Azure αναζήτησης στο NodeJS | Microsoft Azure | Υπηρεσία αναζήτησης φιλοξενούμενη cloud"
    description="Δημιουργία μιας εφαρμογής αναζήτησης σε μια υπηρεσία αναζήτησης φιλοξενούμενη cloud στην Azure χρησιμοποιώντας NodeJS ως γλώσσα προγραμματισμού σας καθοδηγήσει."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Γρήγορα αποτελέσματα με το Azure αναζήτησης στο NodeJS
> [AZURE.SELECTOR]
- [Πύλη](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Μάθετε πώς μπορείτε να δημιουργήσετε μια προσαρμοσμένη εφαρμογή αναζήτησης NodeJS που χρησιμοποιεί Azure αναζήτησης για την εμπειρία αναζήτησης. Αυτό το πρόγραμμα εκμάθησης χρησιμοποιεί το [Azure αναζήτησης υπηρεσίας REST API](https://msdn.microsoft.com/library/dn798935.aspx) για να δημιουργήσετε τα αντικείμενα και τις εργασίες που χρησιμοποιούνται σε αυτήν την άσκηση.

Χρησιμοποιήσαμε [NodeJS](https://nodejs.org) και NPM, [Sublime 3 κειμένου](http://www.sublimetext.com/3)και του Windows PowerShell στα Windows 8.1 για να αναπτύξετε και να ελέγξετε αυτόν τον κωδικό.

Για να εκτελέσετε αυτό το δείγμα, πρέπει να έχετε μια υπηρεσία αναζήτησης Azure, η οποία μπορείτε να εγγραφείτε στην [Πύλη του Azure](https://portal.azure.com). Για οδηγίες βήμα προς βήμα, ανατρέξτε στο θέμα [Δημιουργία μιας υπηρεσίας αναζήτησης Azure στην πύλη](search-create-service-portal.md) .

## <a name="about-the-data"></a>Σχετικά με τα δεδομένα

Αυτού του δείγματος εφαρμογής χρησιμοποιεί τα δεδομένα από το [Ηνωμένες Πολιτείες γεωλογικές υπηρεσιών (αυτό ΓΊΝΕΤΑΙ)](http://geonames.usgs.gov/domestic/download_data.htm), φιλτραρισμένο στη την κατάσταση της Ροντ Νησί για να μειώσετε το μέγεθος του συνόλου δεδομένων. Θα χρησιμοποιήσουμε αυτά τα δεδομένα για τη δημιουργία μιας εφαρμογής αναζήτησης που επιστρέφει γνωστών σημείων κτίρια όπως νοσοκομεία και σχολεία, καθώς και γεωλογικές δυνατότητες όπως ροές λίμνες και summits.

Σε αυτήν την εφαρμογή, το πρόγραμμα **DataIndexer** δημιουργεί και φορτώνει το ευρετήριο χρησιμοποιώντας ένα [ευρετήριο](https://msdn.microsoft.com/library/azure/dn798918.aspx) δομή, ανακτώντας τα φιλτραρισμένα dataset ΓΊΝΕΤΑΙ από μια δημόσια βάση δεδομένων SQL Azure. Διαπιστευτήρια και σύνδεση πληροφοριών στο αρχείο προέλευσης δεδομένων με σύνδεση παρέχεται στον κώδικα πρόγραμμα. Ρύθμιση των παραμέτρων δεν περαιτέρω είναι απαραίτητο.

> [AZURE.NOTE] Θα σας να εφαρμοστεί ένα φίλτρο σε αυτό το dataset για να παραμείνετε στην περιοχή του ορίου 10.000 εγγράφων τη δωρεάν σειρά τις πληροφορίες τιμολόγησης. Εάν χρησιμοποιείτε το τυπικό επίπεδο, αυτό το όριο δεν ισχύει. Για λεπτομέρειες σχετικά με τη δυνατότητα για κάθε επίπεδο τιμολόγησης, ανατρέξτε στο θέμα [όρια υπηρεσίας αναζήτησης](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Βρείτε το όνομα της υπηρεσίας και το api-κλειδί της υπηρεσίας αναζήτησης Azure

Αφού δημιουργήσετε την υπηρεσία, επιστρέψτε στην πύλη του για να λάβετε τη διεύθυνση URL ή `api-key`. Συνδέσεις με την υπηρεσία αναζήτησης απαιτούν να έχετε τόσο τη διεύθυνση URL και ένα `api-key` για τον έλεγχο ταυτότητας την κλήση.

1. Είσοδος στην [πύλη του Azure](https://portal.azure.com).
2. Στη γραμμή μεταπήδηση, κάντε κλικ στην **υπηρεσία αναζήτησης** σε λίστα όλων των υπηρεσιών Azure αναζήτησης παρέχεται για τη συνδρομή σας.
3. Επιλέξτε την υπηρεσία που θέλετε να χρησιμοποιήσετε.
4. Στον πίνακα εργαλείων των υπηρεσιών, θα δείτε τα πλακίδια για βασικές πληροφορίες, καθώς και το εικονίδιο κλειδιού για να αποκτήσετε πρόσβαση τα πλήκτρα διαχείρισης.

    ![][3]

5. Αντιγράψτε τη διεύθυνση URL της υπηρεσίας, ένας αριθμός-κλειδί διαχείρισης και έναν αριθμό-κλειδί ερωτήματος. Θα χρειαστείτε τα τρία αργότερα, κατά την προσθήκη τους στο αρχείο config.js.

## <a name="download-the-sample-files"></a>Λήψη τα δείγματα αρχείων

Χρησιμοποιήστε είτε μία από τις παρακάτω μεθόδους για να κάνετε λήψη του δείγματος.

1. Μεταβείτε στο [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Κάντε κλικ στην επιλογή **Λήψη ZIP**, αποθηκεύστε το αρχείο .zip και, στη συνέχεια, να εξαγάγετε όλα τα αρχεία που περιέχει.

Όλες οι επόμενες αρχείο τροποποιήσεις και εκτέλεση δηλώσεις θα γίνει σε σχέση με τα αρχεία σε αυτόν το φάκελο.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Ενημερώστε το config.js. με τη διεύθυνση URL της υπηρεσίας αναζήτησης και το πλήκτρο api

Χρήση της διεύθυνσης URL και api-κλειδί που αντιγράψατε προηγουμένως, καθορίστε τη διεύθυνση URL, διαχείρισης-κλειδί και κλειδί ερωτήματος στο αρχείο ρύθμισης παραμέτρων.

Πλήκτρα διαχείρισης εκχώρηση πλήρη έλεγχο λειτουργίες υπηρεσίας, όπως τη δημιουργία ή διαγραφή ευρετηρίου και τη φόρτωση εγγράφων. Αντίθετα, ερωτήματος πλήκτρα προορίζονται για λειτουργίες μόνο για ανάγνωση, συνήθως χρησιμοποιούνται από εφαρμογές προγράμματος-πελάτη που συνδέονται με Azure αναζήτησης.

Σε αυτό το δείγμα, θα σας περιλαμβάνει το κλειδί ερωτήματος για να σας βοηθήσει ενίσχυση της η βέλτιστη πρακτική χρήσης το κλειδί ερωτήματος σε εφαρμογές προγράμματος-πελάτη.

Το παρακάτω στιγμιότυπο οθόνης εμφανίζει **config.js** ανοιχτό σε ένα κείμενο επεξεργασίας, με τις σχετικές εγγραφές οριοθετήθηκαν ώστε να μπορείτε να δείτε πού μπορείτε να ενημερώσετε το αρχείο με τις τιμές που είναι έγκυροι για την υπηρεσία αναζήτησης.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Κεντρικός υπολογιστής ένα περιβάλλον χρόνου εκτέλεσης για το δείγμα

Το δείγμα απαιτεί ένα διακομιστή HTTP, που μπορείτε να εγκαταστήσετε καθολικά χρησιμοποιώντας npm.

Χρησιμοποιήστε ένα παράθυρο του PowerShell για τις παρακάτω εντολές.

1. Μεταβείτε στο φάκελο που περιέχει το αρχείο **package.json** .
2. Τύπος `npm install`.
2. Τύπος `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Δημιουργήστε το ευρετήριο και εκτέλεσης της εφαρμογής

1. Τύπος `npm run indexDocuments`.
2. Τύπος `npm run build`.
3. Τύπος `npm run start_server`.
4. Κατευθύνετε το πρόγραμμα περιήγησης στο`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>Αναζήτηση σε αυτό ΓΊΝΕΤΑΙ δεδομένα

Το σύνολο δεδομένων ΓΊΝΕΤΑΙ περιλαμβάνει τις εγγραφές που έχουν σχέση για να την κατάσταση της Ροντ Νησί. Εάν κάνετε κλικ στο κουμπί **αναζήτησης** σε ένα πλαίσιο αναζήτησης κενή, θα λάβετε τις καλύτερες εγγραφές 50, ποιο είναι το προεπιλεγμένο.

Εισαγάγετε έναν όρο αναζήτησης θα σας δώσει το μηχανισμό αναζήτησης κάτι για να μεταβείτε στο. Δοκιμάστε να εισαγάγετε ένα όνομα τοπικές ρυθμίσεις. "Roger Χατζή" ήταν ο πρώτος ρυθμιστής της Νησί Ροντ. Πολλά πάρκα, κτίρια και σχολεία ονομάζονται μετά από αυτόν.

![][9]

Μπορείτε επίσης να δοκιμάσετε οποιονδήποτε από τους εξής όρους:

- Pawtucket
- Pembroke
- χήνα + κέιπ


## <a name="next-steps"></a>Επόμενα βήματα

Αυτό είναι το πρώτο πρόγραμμα εκμάθησης Azure αναζήτησης με βάση NodeJS και του συνόλου δεδομένων ΓΊΝΕΤΑΙ. Με τον καιρό, θα επεκτείνουμε αυτό το πρόγραμμα εκμάθησης για την επίδειξη δυνατότητες επιπλέον αναζήτησης που μπορεί να θέλετε να χρησιμοποιήσετε στο προσαρμοσμένων λύσεων.

Εάν έχετε ήδη κάποιες φόντου στο πλαίσιο Αναζήτηση Azure, μπορείτε να χρησιμοποιήσετε αυτό το δείγμα ως μια springboard για δοκιμή suggesters (ερωτήματα πρόβλεψης ή αυτόματης καταχώρησης), φίλτρα και περιήγηση με συνθήκη. Μπορείτε επίσης να βελτιώσετε κατά τη σελίδα αποτελεσμάτων αναζήτησης, προσθέτοντας μετρήσεις και δέσμης για έγγραφα, έτσι ώστε οι χρήστες να ξεφυλλίσετε τα αποτελέσματα.

Νέος χρήστης του Azure αναζήτησης; Συνιστάται να προσπάθεια άλλα προγράμματα εκμάθησης για να αναπτύξετε την κατανόηση του τι μπορείτε να δημιουργήσετε. Επισκεφθείτε μας [σελίδας τεκμηρίωση](https://azure.microsoft.com/documentation/services/search/) για περισσότερους πόρους. Μπορείτε επίσης να προβάλετε τις συνδέσεις σε μας [λίστα προγραμμάτων εκμάθησης και βίντεο](search-video-demo-tutorial-list.md) για να αποκτήσετε πρόσβαση σε περισσότερες πληροφορίες.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
<properties
    pageTitle="Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή Mobile Azure (Cordova) | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε την εφαρμογή Mobile εφαρμογής υπηρεσίας στο cache και συγχρονισμός δεδομένων χωρίς σύνδεση στην εφαρμογή σας Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Ενεργοποίηση του συγχρονισμού χωρίς σύνδεση για την εφαρμογή για κινητές συσκευές Cordova

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Αυτό το πρόγραμμα εκμάθησης παρουσιάζει τη δυνατότητα συγχρονισμού χωρίς σύνδεση του Azure Mobile Apps για Cordova. Συγχρονισμού χωρίς σύνδεση επιτρέπει στους τελικούς χρήστες να αλληλεπιδράσετε με μια εφαρμογή για κινητές συσκευές&mdash;προβολή, προσθήκη ή τροποποίηση δεδομένων&mdash;ακόμα και όταν δεν υπάρχει σύνδεση δικτύου. Οι αλλαγές αποθηκεύονται σε μια τοπική βάση δεδομένων. Όταν η συσκευή είναι πάλι σε σύνδεση, αυτές οι αλλαγές συγχρονίζονται με την υπηρεσία απομακρυσμένης.

Αυτό το πρόγραμμα εκμάθησης είναι με βάση τη λύση γρήγορη έναρξη Cordova για εφαρμογές του Mobile που δημιουργείτε όταν ολοκληρωθεί το πρόγραμμα εκμάθησης [Apache Cordova Γρήγορη εκκίνηση]. Σε αυτό το πρόγραμμα εκμάθησης, θα μπορείτε να ενημερώσετε τη λύση γρήγορης έναρξης για να προσθέσετε χωρίς σύνδεση για τις δυνατότητες του Azure εφαρμογές του Mobile. Επίσης, θα σας θα επισημάνετε τον κώδικα ειδικά για εργασία χωρίς σύνδεση στην εφαρμογή.

Για να μάθετε περισσότερα σχετικά με τη δυνατότητα συγχρονισμού χωρίς σύνδεση, ανατρέξτε στο θέμα [Συγχρονισμού χωρίς σύνδεση δεδομένων σε εφαρμογές του Mobile Azure]. Για λεπτομέρειες σχετικά με χρήση API, ανατρέξτε στο αρχείο [README] στην προσθήκη.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Προσθήκη της λύσης γρήγορη Έναρξη συγχρονισμού χωρίς σύνδεση

Ο κώδικας συγχρονισμού χωρίς σύνδεση πρέπει να προστεθεί στην εφαρμογή. Συγχρονισμού χωρίς σύνδεση απαιτεί την προσθήκη cordova-sqlite-χώρου αποθήκευσης, που λαμβάνει προστίθεται αυτόματα σε εφαρμογή κατά την προσθήκη εφαρμογές του Mobile Azure περιλαμβάνεται στο έργο. Γρήγορη έναρξη έργου περιλαμβάνει και τα δύο από αυτές τις προσθήκες.

1. Στην Εξερεύνηση λύσεων του Visual Studio, ανοίξτε index.js και να αντικαταστήσετε τον παρακάτω κώδικα

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    με αυτόν τον κώδικα:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Στη συνέχεια, αντικαταστήστε τον ακόλουθο κώδικα:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    με αυτόν τον κώδικα:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Το προηγούμενο προσθήκες κώδικα προετοιμασία του τοπικού χώρου αποθήκευσης και τον ορισμό τοπικό πίνακα που ταιριάζει με τις τιμές της στήλης που χρησιμοποιείται στο σας Azure πίσω τέλος. (Δεν χρειάζεται να συμπεριλάβετε όλες οι τιμές στήλης σε αυτόν τον κωδικό.)

    Μπορείτε να λάβετε μια αναφορά σε περιβάλλον συγχρονισμού καλώντας **getSyncContext**. Το περιβάλλον συγχρονισμού σάς βοηθά να διατηρήσει τις σχέσεις πινάκων, παρακολούθηση και Ώθηση αλλαγές σε μια εφαρμογή προγράμματος-πελάτη έχει τροποποιηθεί όταν καλείται **push** όλους τους πίνακες.

3. Ενημερώστε τη διεύθυνση URL της εφαρμογής για να σας διεύθυνση URL της εφαρμογής εφαρμογή Mobile.

4. Στη συνέχεια, αντικαταστήστε αυτόν τον κώδικα:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    με αυτόν τον κώδικα:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Το παραπάνω κώδικα προετοιμάζει το περιβάλλον συγχρονισμού και, στη συνέχεια, κλήσεις getSyncTable (αντί για getTable) για να λάβετε μια αναφορά στον τοπικό πίνακα.

    Αυτό χρησιμοποιεί κώδικα της τοπικής βάσης δεδομένων για όλες δημιουργία, ανάγνωση, ενημέρωση και διαγραφή λειτουργίες πίνακα (CRUD).

    Αυτό το δείγμα εκτελεί απλές σφάλμα κατά το χειρισμό σε διενέξεις συγχρονισμού. Μια πραγματική εφαρμογή θα χειρίζεται τα διάφορα σφάλματα όπως συνθήκες δικτύου διενέξεων διακομιστή και άλλα άτομα. Για παραδείγματα κώδικα, ανατρέξτε στο θέμα το [δείγμα συγχρονισμού χωρίς σύνδεση].

5. Στη συνέχεια, προσθέστε αυτήν τη συνάρτηση για να εκτελέσετε την πραγματική συγχρονισμού.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Μπορείτε να αποφασίσετε πότε να προωθήσετε αλλαγές σε εφαρμογή Mobile υπόβαθρο καλώντας **push** του αντικειμένου **syncContext** που χρησιμοποιείται από το πρόγραμμα-πελάτη. Για παράδειγμα, μπορείτε να προσθέσετε μια κλήση για να **syncBackend** σε ένα πρόγραμμα χειρισμού συμβάντων κουμπί στην εφαρμογή όπως το νέο κουμπί Συγχρονισμός ή μπορείτε να προσθέσετε στην κλήση της συνάρτησης **addItemHandler** για να συγχρονίσετε κάθε φορά που προστίθεται ένα νέο στοιχείο.

##<a name="offline-sync-considerations"></a>Ζητήματα συγχρονισμού χωρίς σύνδεση

Στο δείγμα, η μέθοδος **push** της **syncContext** ονομάζεται μόνο κατά την εκκίνηση εφαρμογών στη συνάρτηση επιστροφής κλήσης για τη σύνδεση.  Σε μια εφαρμογή ρεαλιστικό, μπορείτε επίσης να κάνετε αυτή η λειτουργία συγχρονισμού ενεργοποίησε με μη αυτόματο τρόπο ή όταν αλλάζει την κατάσταση του δικτύου.

Όταν μια ελκυστική έχει εκτελεστεί σε έναν πίνακα που έχει εκκρεμείς τοπικό ενημερώσεις παρακολουθούνται με το περιβάλλον, αυτή η λειτουργία ελκυστική θα ενεργοποιήσει αυτόματα μια προηγούμενη push περιβάλλοντος. Κατά την ανανέωση, προσθήκη και την ολοκλήρωση των στοιχείων σε αυτό το δείγμα, μπορείτε να παραλείψετε την κλήση ρητή **push** , επειδή μπορεί να είναι πλεονάζοντα (ελέγχουν πρώτα το [αρχείο README] για την τρέχουσα κατάσταση δυνατότητα).

Στον κώδικα που παρέχεται, υποβάλλεται ερώτημα όλες τις εγγραφές στον πίνακα απομακρυσμένης todoItem, αλλά είναι επίσης πιθανό να φιλτράρετε εγγραφές, μεταφέροντας ένα αναγνωριστικό ερωτήματος και το ερώτημα για να **push**. Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα *Αυξάνονται συγχρονισμού* στην [Δεδομένων συγχρονισμού χωρίς σύνδεση σε εφαρμογές του Mobile Azure].

## <a name="optional-disable-authentication"></a>(Προαιρετικό) Απενεργοποίηση του ελέγχου ταυτότητας

Εάν δεν έχετε ήδη ρυθμίσει τον έλεγχο ταυτότητας και δεν θέλετε να ρυθμίσετε τον έλεγχο ταυτότητας πριν δοκιμών συγχρονισμού χωρίς σύνδεση, σχολιάσετε τη συνάρτηση επιστροφής κλήσης για τη σύνδεση, αλλά αφήσετε τον κώδικα μέσα σε uncommented η λειτουργία επιστροφής κλήσης.

Ο κώδικας θα πρέπει να είναι κάπως έτσι μετά σχολιασμό τις γραμμές σύνδεσης.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Τώρα, η εφαρμογή θα συγχρονίζονται με το Azure παρασκηνίου κατά την εκτέλεση της εφαρμογής αντί όταν συνδεθείτε.

## <a name="run-the-client-app"></a>Εκτελέστε την εφαρμογή υπολογιστή-πελάτη

Με εργασία χωρίς σύνδεση τώρα τη δυνατότητα συγχρονισμού, τώρα μπορείτε να εκτελέσετε την εφαρμογή-πελάτη τουλάχιστον μία φορά σε κάθε πλατφόρμα για να συμπληρώσετε τη βάση δεδομένων του τοπικού χώρου αποθήκευσης. Αργότερα, θα προσομοιώνουν ένα σενάριο χωρίς σύνδεση και τροποποίηση των δεδομένων του τοπικού χώρου αποθήκευσης, ενώ η εφαρμογή είναι χωρίς σύνδεση.

## <a name="optional-test-the-sync-behavior"></a>(Προαιρετικό) Ελέγξτε τη συμπεριφορά συγχρονισμού

Σε αυτήν την ενότητα, θα μπορείτε να τροποποιήσετε το έργο προγράμματος-πελάτη για να προσομοιώσετε ένα σενάριο χωρίς σύνδεση, χρησιμοποιώντας μια διεύθυνση URL δεν είναι έγκυρη εφαρμογή για τον υπολογιστή στο παρασκήνιο. Όταν προσθέτετε ή αλλάζετε στοιχεία δεδομένων, αυτές οι αλλαγές θα που θα διατηρούνται στα του τοπικού χώρου αποθήκευσης, αλλά δεν συγχρονίζονται στο χώρο αποθήκευσης δεδομένων παρασκηνίου μέχρι η σύνδεση έχει αποκατασταθεί.

1. Στην Εξερεύνηση λύσεων, ανοίξτε το αρχείο έργου index.js και αλλάξτε τη διεύθυνση URL της εφαρμογής ώστε να οδηγούν σε μη έγκυρη διεύθυνση URL, όπως τα εξής:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Στο index.html, ενημερώστε την υπηρεσία παροχής Κρυπτογράφησης `<meta>` στοιχείο με την ίδια διεύθυνση URL δεν είναι έγκυρη.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Δημιουργία και εκτελέστε την εφαρμογή υπολογιστή-πελάτη και παρατηρήστε ότι έχει συνδεθεί μια εξαίρεση στην κονσόλα, όταν η εφαρμογή προσπαθεί να συγχρονίσετε με υπόβαθρο μετά τη σύνδεση. Τα νέα στοιχεία, μπορείτε να προσθέσετε θα υπάρχει μόνο σε του τοπικού χώρου αποθήκευσης, μέχρι να είναι δυνατή η προώθηση τους στον φορητό υπολογιστή στο παρασκήνιο. Η εφαρμογή προγράμματος-πελάτη συμπεριφέρεται σαν να είναι συνδεδεμένη παρασκηνίου, υποστήριξης όλα δημιουργία, ανάγνωση, ενημέρωση, λειτουργίες διαγραφής (CRUD).

4. Κλείστε την εφαρμογή και κάντε επανεκκίνηση, για να βεβαιωθείτε ότι τα νέα στοιχεία που δημιουργήσατε διατηρούνται στον τοπικό χώρο αποθήκευσης.

5. (Προαιρετικό) Χρησιμοποιήστε το Visual Studio για να προβάλετε τον πίνακα βάσης δεδομένων SQL Azure για να δείτε ότι τα δεδομένα στη βάση δεδομένων παρασκηνίου δεν έχει αλλάξει.

    Στο Visual Studio, ανοίξτε την **Εξερεύνηση διακομιστή**. Περιήγηση στη βάση δεδομένων σας στο **Azure**->**Βάσεις δεδομένων SQL**. Κάντε δεξί κλικ σε βάση δεδομένων σας και επιλέξτε **Άνοιγμα στην Εξερεύνηση αντικειμένου SQL Server**. Τώρα μπορείτε να περιηγηθείτε σε πίνακα της βάσης δεδομένων SQL και τα περιεχόμενά της.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Προαιρετικό) Δοκιμάστε την επανασύνδεση για να σας φορητό υπολογιστή στο παρασκήνιο

Σε αυτήν την ενότητα που θα συνδεθείτε ξανά την εφαρμογή για κινητές συσκευές υπόβαθρο, που αναπαριστά την εφαρμογή σύντομα επιστροφή σε κατάσταση σύνδεσης. Όταν συνδεθείτε, θα συγχρονίζονται δεδομένων σας κινητής παρασκηνίου.

1. Ανοίξτε ξανά index.js και διορθώστε τη διεύθυνση URL της εφαρμογής ώστε να δείχνει προς τη σωστή διεύθυνση URL.

2. Ανοίξτε ξανά index.html και να διορθώσετε τη διεύθυνση URL της εφαρμογής στο η υπηρεσία παροχής Κρυπτογράφησης `<meta>` στοιχείο.

3. Εκ νέου δημιουργία και εκτέλεση της εφαρμογής υπολογιστή-πελάτη. Η εφαρμογή προσπαθεί να συγχρονίσετε με την εφαρμογή για κινητές συσκευές παρασκηνίου μετά τη σύνδεση. Βεβαιωθείτε ότι έχουν συνδεθεί χωρίς εξαιρέσεις στην κονσόλα εντοπισμού σφαλμάτων.

4. (Προαιρετικό) Προβάλετε τα ενημερωμένα δεδομένα χρησιμοποιώντας Εξερεύνηση αντικειμένου SQL Server ή ένα εργαλείο ΥΠΌΛΟΙΠΑ όπως Fiddler. Παρατηρήστε τα δεδομένα έχει συγχρονιστεί μεταξύ της βάσης δεδομένων παρασκηνίου και του τοπικού χώρου αποθήκευσης.

    Παρατηρήστε τα δεδομένα έχει συγχρονιστεί μεταξύ της βάσης δεδομένων και του τοπικού χώρου αποθήκευσης και περιέχει τα στοιχεία που προσθέσατε κατά την εφαρμογή σας έχει αποσυνδεθεί.

## <a name="additional-resources"></a>Πρόσθετοι πόροι

* [Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]

* [Εξώφυλλο στο cloud: συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές] \(Σημείωση: το βίντεο σχετικά με τις υπηρεσίες Mobile, αλλά συγχρονισμού χωρίς σύνδεση λειτουργεί με παρόμοιο τρόπο στις εφαρμογές του Mobile Azure\)

* [Εργαλεία του Visual Studio για Apache Cordova]

## <a name="next-steps"></a>Επόμενα βήματα

* Δείτε πιο σύνθετες δυνατότητες συγχρονισμού χωρίς σύνδεση, όπως η επίλυση διενέξεων στο [δείγμα συγχρονισμού χωρίς σύνδεση]
* Εξετάστε το συγχρονισμού χωρίς σύνδεση API αναφορά στο το [αρχείο README]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova γρήγορης εκκίνησης]: app-service-mobile-cordova-get-started.md
[ΑΡΧΕΊΟ README]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[δείγμα συγχρονισμού χωρίς σύνδεση]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Συγχρονισμός δεδομένων χωρίς σύνδεση στο Azure εφαρμογές για κινητές συσκευές]: app-service-mobile-offline-data-sync.md
[Εξώφυλλο του cloud: Συγχρονισμού χωρίς σύνδεση στις υπηρεσίες του Azure κινητές συσκευές]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Εργαλεία του Visual Studio για Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

<properties
    pageTitle="Προσθήκη ελέγχου ταυτότητας σε Cordova Apache με εφαρμογές του Mobile | Azure εφαρμογής υπηρεσίας"
    description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εφαρμογές του Mobile για τον έλεγχο ταυτότητας τους χρήστες της εφαρμογής Apache Cordova στις διάφορες υπηρεσίες παροχής ταυτότητας, συμπεριλαμβανομένων των Google, Facebook, Twitter και Microsoft Azure εφαρμογής υπηρεσίας."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Προσθήκη ελέγχου ταυτότητας για την εφαρμογή σας Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Σύνοψη

Σε αυτό το πρόγραμμα εκμάθησης, μπορείτε να προσθέσετε τον έλεγχο ταυτότητας στο έργο γρήγορη έναρξη todolist σε Cordova Apache χρησιμοποιώντας μια υπηρεσία παροχής ταυτότητας υποστηρίζονται. Αυτό το πρόγραμμα εκμάθησης είναι με βάση το [Γρήγορα αποτελέσματα με εφαρμογές του Mobile] πρόγραμμα εκμάθησης, που πρέπει να ολοκληρώσετε πρώτα.

##<a name="register"></a>Καταχώρηση της εφαρμογής για τον έλεγχο ταυτότητας και ρύθμιση παραμέτρων της εφαρμογής υπηρεσίας

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Παρακολουθήστε ένα βίντεο που δείχνει παρόμοια βήματα](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Περιορισμός δικαιωμάτων σε χρήστες με έλεγχο ταυτότητας

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Τώρα, μπορείτε να επαληθεύσετε ότι έχει απενεργοποιηθεί ανώνυμης πρόσβασης για τον υπολογιστή στο παρασκήνιο. Στο Visual Studio, ανοίξτε το έργο που δημιουργήσατε όταν ολοκληρωθεί το πρόγραμμα εκμάθησης [Γρήγορα αποτελέσματα με εφαρμογές του Mobile], στη συνέχεια, εκτελέσετε την εφαρμογή στο του **Google Android προσομοίωσης** και βεβαιωθείτε ότι ένα μη αναμενόμενο σφάλμα σύνδεσης εμφανίζεται αφού γίνει εκκίνηση της εφαρμογής.

Στη συνέχεια, θα μπορείτε να ενημερώσετε την εφαρμογή για τον έλεγχο ταυτότητας χρηστών πριν από την αίτηση πόρων από την εφαρμογή Mobile παρασκηνίου.

##<a name="add-authentication"></a>Προσθήκη ελέγχου ταυτότητας στην εφαρμογή

1. Ανοίξτε το έργο σας στο **Visual Studio**και, στη συνέχεια, ανοίξτε το `www/index.html` αρχείου για επεξεργασία.

2. Εντοπίστε το `Content-Security-Policy` μετα-ετικέτα στην ενότητα κεφαλίδας.  Θα πρέπει να προσθέσετε τον κεντρικό υπολογιστή OAuth στη λίστα των επιτρεπόμενων προελεύσεων.

  	| Υπηρεσία παροχής               | Όνομα της υπηρεσίας παροχής SDK | Διακριτικό κεντρικού υπολογιστή                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Καταλόγου Azure Active Directory | aad               | https://Login.Windows.NET   |
  	| Facebook               | facebook          | https://www.facebook.com    |
  	| Google                 | Google            | https://Accounts.Google.com |
  	| Microsoft              | διαθέσιμος microsoftaccount  | https://Login.Live.com      |
  	| Twitter                | Twitter           | https://API.Twitter.com     |

    Ένα παράδειγμα πολιτική ασφαλείας περιεχομένου (υλοποιηθεί για Azure Active Directory) είναι ως εξής:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Θα πρέπει να αντικαταστήσετε `https://login.windows.net` με τον κεντρικό υπολογιστή OAuth από τον παραπάνω πίνακα.  Ανατρέξτε στην [τεκμηρίωση πολιτική ασφαλείας περιεχομένου] για περισσότερες πληροφορίες σχετικά με αυτό μετα-ετικέτα.

    Σημειώστε ότι ορισμένες υπηρεσίες παροχής ελέγχου ταυτότητας δεν απαιτεί πολιτική ασφαλείας περιεχόμενο αλλάζει όταν χρησιμοποιείται σε περίπτωση κινητές συσκευές.  Για παράδειγμα, δεν υπάρχει περιεχόμενο πολιτική ασφαλείας απαιτούνται αλλαγές κατά τη χρήση του ελέγχου ταυτότητας Google σε μια συσκευή Android.

3. Άνοιγμα του `www/js/index.js` αρχείο για επεξεργασία, εντοπίστε το `onDeviceReady()` μέθοδο, και στην περιοχή της δημιουργίας προγράμματος-πελάτη κώδικα, προσθέστε τα εξής:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Σημειώστε ότι αυτός ο κωδικός αντικαταστήσετε τον υπάρχοντα κωδικό που δημιουργεί την αναφορά πίνακα και ανανεώνει το περιβάλλον εργασίας Χρήστη.

    Η μέθοδος login() ξεκινά τον έλεγχο ταυτότητας με την υπηρεσία παροχής. Η μέθοδος login() είναι μια συνάρτηση ασύγχρονης που επιστρέφει μια Υπόσχεση JavaScript.  Το υπόλοιπο της προετοιμασίας τοποθετείται εντός της απόκρισης Υπόσχεση να σας, έτσι ώστε να μην εκτελεστεί έως ότου ολοκληρωθεί η μέθοδος login().

4. Τον κώδικα που μόλις προσθέσατε, αντικαταστήστε `SDK_Provider_Name` με το όνομα της υπηρεσίας παροχής σύνδεσης. Για παράδειγμα, για Azure Active Directory, χρησιμοποιήστε `client.login('aad')`.

4. Εκτελέστε το έργο σας.  Όταν ολοκληρωθεί η προετοιμασία του έργου, η εφαρμογή σας θα εμφανιστούν στη σελίδα πρόσβασης OAuth για την υπηρεσία παροχής ελέγχου ταυτότητας επιλεγμένο.

##<a name="next-steps"></a>Επόμενα βήματα

* Μάθετε περισσότερα [Σχετικά με τον έλεγχο ταυτότητας] με Azure εφαρμογής υπηρεσίας.
* Συνεχίστε το πρόγραμμα εκμάθησης, προσθέτοντας [Τις ειδοποιήσεις Push] σας εφαρμογή Apache Cordova.

Μάθετε πώς μπορείτε να χρησιμοποιήσετε το SDK.

* [Apache Cordova SDK]
* [Διακομιστής ASP.NET SDK]
* [Node.js διακομιστή SDK]

<!-- URLs. -->
[Γρήγορα αποτελέσματα με εφαρμογές του Mobile]: app-service-mobile-cordova-get-started.md
[Τεκμηρίωση πολιτική ασφαλείας περιεχομένου]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Ειδοποιήσεις Push]: app-service-mobile-cordova-get-started-push.md
[Σχετικά με τον έλεγχο ταυτότητας]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[Διακομιστής ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js διακομιστή SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

<properties
    pageTitle="Azure AD Cordova γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή Cordova που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Ενοποίηση του Azure AD με μια Apache Cordova εφαρμογής

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova δίνει τη δυνατότητα για να αναπτύξετε εφαρμογές HTML5/JavaScript, οι οποίες είναι δυνατό να εκτελεστεί σε κινητές συσκευές ως πλήρως ανεπτυγμένο εγγενείς εφαρμογές.
Με το Azure AD, μπορείτε να προσθέσετε δυνατότητες ελέγχου ταυτότητας βαθμολογίας για μεγάλες επιχειρήσεις στις εφαρμογές του Cordova. Ευχαριστήριο μια προσθήκη Cordova αναδίπλωση Azure AD εγγενής SDK στο iOS, Android, του Windows Store και Windows Phone, μπορείτε να βελτιώσετε την εφαρμογή με το υποστηρίζει εισόδου σε με τους λογαριασμούς των χρηστών σας AD, να αποκτήσετε ξανά πρόσβαση στο Office 365 και API του Azure και ακόμα και προστασία κλήσεων για το δικό σας προσαρμοσμένο API Web.

Σε αυτό το πρόγραμμα εκμάθησης θα χρησιμοποιήσουμε την προσθήκη Apache Cordova για Active Directory ελέγχου ταυτότητας βιβλιοθήκης (ADAL) για να βελτιώσετε μια απλή εφαρμογή με τις παρακάτω δυνατότητες της:

-   Με λίγα γραμμές κώδικα, τον έλεγχο ταυτότητας ενός χρήστη AD και η λήψη διακριτικού για την κλήση του API Azure AD Graph.
-   Χρησιμοποιήστε αυτό το διακριτικό για να καλέσετε το API Graph για ερωτήματος αυτόν τον κατάλογο και να εμφανίσετε τα αποτελέσματα  
-   Αξιοποίηση του ADAL διακριτικού cache για την ελαχιστοποίηση τα μηνύματα ελέγχου ταυτότητας για το χρήστη.

Για να το κάνετε αυτό, θα πρέπει να:

2. Καταχωρήστε μια εφαρμογή του Azure AD
2. Προσθήκη κώδικα για την εφαρμογή σας για να τα διακριτικά αίτηση
3. Προσθήκη κώδικα για να χρησιμοποιήσετε το διακριτικό για την υποβολή ερωτημάτων το API Graph και να εμφανίσετε αποτελέσματα.
4. Δημιουργήστε το έργο ανάπτυξης Cordova με όλες τις πλατφόρμες που θέλετε να στοχεύσετε και την προσθήκη Cordova ADAL και δοκιμάστε τη λύση σε emulators.

## <a name="0--prerequisites"></a>*0. προαπαιτούμενα στοιχεία*

Για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης θα πρέπει:

- Ένα μισθωτή του Azure AD όπου έχετε ένα λογαριασμό με δικαιώματα ανάπτυξης εφαρμογής
- Ένα περιβάλλον ανάπτυξης που έχει ρυθμιστεί για χρήση Apache Cordova  

Εάν και τα δύο έχουν ήδη οριστεί, προχωρήστε απευθείας στο βήμα 1.

Εάν δεν έχετε ένα μισθωτή του Azure AD, μπορείτε να βρείτε [οδηγίες σχετικά με τον τρόπο για να αποκτήσετε εδώ](active-directory-howto-tenant.md).

Εάν δεν έχετε Cordova Apache ρύθμιση στον υπολογιστή σας, εγκαταστήστε το εξής:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (μπορεί να εγκατασταθεί εύκολα μέσω διαχείρισης πακέτου NPM: `npm install -g cordova`)

Σημειώστε ότι αυτές θα πρέπει να λειτουργεί τόσο στον Υπολογιστή όσο και σε Mac.

Κάθε πλατφόρμα προορισμού έχει διαφορετικές προϋποθέσεις.

- Για να δημιουργήσετε και να εκτελέσετε έκδοση εφαρμογής τηλέφωνο ή Tablet/Υπολογιστή με Windows
    - [Visual Studio 2013 για Windows με ενημερωμένη έκδοση 2 ή νεότερες εκδόσεις](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express ή κάποια άλλη έκδοση).
- Για να δημιουργήσετε και να εκτελέσετε για iOS
    -   Xcode 5.x ή μεγαλύτερη. Κάντε λήψη του http://developer.apple.com/downloads ή το [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12)
    -   [ios sim](https://www.npmjs.org/package/ios-sim) – σάς επιτρέπει να εκκίνηση εφαρμογών του iOS σε του iOS Simulator από τη γραμμή εντολών (μπορεί να εγκατασταθεί εύκολα μέσω τερματικού: `npm install -g ios-sim`)

- Για τη δημιουργία και εκτέλεση της εφαρμογής για Android
    - Εγκατάσταση [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) ή νεότερη έκδοση. Βεβαιωθείτε ότι `JAVA_HOME` (περιβάλλον μεταβλητής) έχει οριστεί σωστά σύμφωνα με JDK διαδρομή εγκατάστασης (για παράδειγμα C:\Program Files\Java\jdk1.7.0_75).
    - Εγκατάσταση του [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) και προσθήκη `<android-sdk-location>\tools` θέση (για παράδειγμα, C:\tools\Android\android-sdk\tools) για να σας `PATH` μεταβλητή περιβάλλοντος.
    - Άνοιγμα της διαχείρισης SDK Android (για παράδειγμα, μέσω terminal: `android`) και εγκατάσταση
    - *Android 5.0.1 (API 21)* πλατφόρμα SDK
    - *Εργαλεία δημιουργίας android SDK* έκδοση 19.1.0 ή νεότερη έκδοση
    - *Android υποστήριξη αποθετήριο δεδομένων* (Πρόσθετα)

  Android sdk δεν παρέχει οποιαδήποτε προεπιλεγμένη παρουσία προσομοίωσης. Δημιουργήστε ένα νέο, εκτελώντας `android avd` από το terminal και, στη συνέχεια, επιλέγοντας *Δημιουργία...* , εάν θέλετε να εκτελέσετε εφαρμογή Android στο προσομοίωσης. Προτεινόμενη *Επίπεδο Api* είναι 19 ή νεότερη έκδοση, ανατρέξτε στο άρθρο [AVD Manager] (http://developer.android.com/tools/help/avd-manager.html) για περισσότερες πληροφορίες σχετικά με τις επιλογές Android προσομοίωσης και δημιουργίας.


## <a name="1--register-an-application-with-azure-ad"></a>*1. καταχώρηση μιας εφαρμογής με το Azure AD*

Σημείωση: αυτό __το βήμα είναι προαιρετικό__. Το πρόγραμμα εκμάθησης που παρέχονται προ-προμήθεια του φακέλου τιμές που θα σας επιτρέψει να δείτε το δείγμα στην πράξη χωρίς κάνοντας οποιαδήποτε προμήθεια στο δικό του μισθωτή. Ωστόσο, συνιστάται που εκτελέσετε αυτό το βήμα και εξοικειωθείτε με τη διαδικασία, όπως θα είναι απαιτείται όταν θα δημιουργήσετε τις δικές σας εφαρμογές.

Azure AD μόνο θα χορηγεί διακριτικά γνωστά εφαρμογές. Μπορείτε να χρησιμοποιήσετε Azure AD από την εφαρμογή, πρέπει να δημιουργήσετε μια καταχώρηση για αυτό στο μισθωτή σας.  Για να καταχωρήσετε μια νέα εφαρμογή στο μισθωτή σας,

-   Πραγματοποιήστε είσοδο στο την [πύλη διαχείρισης Azure](https://manage.windowsazure.com)
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε το μισθωτή όπου θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και να δημιουργήσετε μια νέα **Εφαρμογή εγγενούς προγράμματος-πελάτη** (παρά το γεγονός ότι οι εφαρμογές Cordova είναι HTML με βάση, δημιουργούμε εφαρμογή εγγενές πρόγραμμα-πελάτη εδώ επομένως `Native Client Application` πρέπει να έχετε επιλέξει, διαφορετικά, δεν θα λειτουργεί η εφαρμογή).
    -   Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    -   Η **Ανακατεύθυνση URI** είναι το URI που χρησιμοποιείται για να επιστρέψετε διακριτικά στην εφαρμογή σας. Πληκτρολογήστε `http://MyDirectorySearcherApp`.

Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες: μπορείτε να το βρείτε στην καρτέλα **Ρύθμιση παραμέτρων** της εφαρμογής που μόλις δημιουργήθηκε.

Για να εκτελέσετε `DirSearchClient Sample`, εκχωρήσει το δικαίωμα που έχουν δημιουργηθεί πρόσφατα εφαρμογή ερώτημα για το _API Azure AD γραφήματος_:
-   Στην καρτέλα **Ρύθμιση παραμέτρων** , εντοπίστε την ενότητα "Δικαιώματα σε άλλες εφαρμογές".  Για την εφαρμογή "Azure Active Directory", προσθέστε τα δικαιώματα **πρόσβασης στον κατάλογο ως ο χρήστης πραγματοποιήσει στο** στην περιοχή **Δικαιώματα με ανάθεση**.  Αυτό θα ενεργοποιήσει την εφαρμογή σας ερώτημα για το API Graph για τους χρήστες.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. κλωνοποίηση του αποθετηρίου εφαρμογή δείγματος που απαιτούνται για το πρόγραμμα εκμάθησης*

Από το κέλυφος ή τη γραμμή εντολών, πληκτρολογήστε την ακόλουθη εντολή:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Δημιουργία της εφαρμογής Cordova*

Υπάρχουν πολλοί τρόποι δημιουργίας Cordova εφαρμογές. Σε αυτό το πρόγραμμα εκμάθησης, θα χρησιμοποιήσουμε το περιβάλλον γραμμής εντολών Cordova (CLI).
Από το κέλυφος ή τη γραμμή εντολών, πληκτρολογήστε την ακόλουθη εντολή:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Που θα δημιουργία της δομής των φακέλων και ικριώματος για το έργο Cordova, αντιγράφοντας το περιεχόμενο του έργου starter στον υποφάκελο www.
Μετακίνηση στο νέο φάκελο DirSearchClient.

    cd .\DirSearchClient

Προσθήκη της προσθήκης whitelist, είναι απαραίτητο για την ενεργοποίηση του API Graph.

     cordova plugin add cordova-plugin-whitelist

Στη συνέχεια, προσθέσετε όλες τις πλατφόρμες που θέλετε για την υποστήριξη. Για να έχετε ένα δείγμα εργασία, θα πρέπει να εκτελεί τουλάχιστον μία από τις παρακάτω εντολές. Σημειώστε ότι δεν θα μπορείτε να προσομοιώσετε iOS στα Windows, ή στα Windows/Windows Phone σε Mac.

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Τέλος, μπορείτε να προσθέσετε το ADAL για προσθήκη Cordova στο έργο σας.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. Προσθήκη κώδικα για τον έλεγχο ταυτότητας χρηστών και αποκτήστε τα διακριτικά από AAD*

Η εφαρμογή που αναπτύσσετε σε αυτό το πρόγραμμα εκμάθησης παρέχει μια δυνατότητα αναζήτησης καταλόγου τα χωρίς λειτουργικό σύστημα, όπου ο τελικός χρήστης μπορεί να πληκτρολογήσετε το ψευδώνυμο από οποιονδήποτε χρήστη στον κατάλογο και να οπτικοποιήσετε ορισμένες βασικές χαρακτηριστικά.  Το έργο starter περιέχει τον ορισμό του περιβάλλοντος εργασίας χρήστη βασικές της εφαρμογής (σε www/index.html) και το ικρίωμα που καλώδια συμβάν βασική εφαρμογή κύκλοι, συνδέσεις περιβάλλοντος εργασίας χρήστη και τα αποτελέσματα λογικής εμφάνισης (στο www/js/index.js). Το μόνο αριστερό εξωτερικό για εσάς είναι να προσθέσετε τη λογική εφαρμογής ταυτότητας εργασίες.

Το πρώτο πράγμα που πρέπει να κάνετε είναι να Παρουσιάστε στον κώδικα τις τιμές πρωτόκολλο που χρησιμοποιούνται από AAD για τον προσδιορισμό της εφαρμογής σας και τους πόρους που προορισμού. Αυτές οι τιμές θα χρησιμοποιηθεί για να δημιουργήσετε τις αιτήσεις διακριτικού αργότερα. Εισαγάγετε το τμήμα κώδικα κάτω από το στοιχείο στην κορυφή του αρχείου index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Το `redirectUri` και `clientId` τιμές πρέπει να συμφωνούν με τις τιμές που περιγράφει την εφαρμογή σας στο AAD. Μπορείτε να βρείτε αυτές από την καρτέλα ρύθμιση παραμέτρων στην πύλη του Azure, όπως περιγράφεται στο βήμα 1 νωρίτερα σε αυτό το πρόγραμμα εκμάθησης.
Σημείωση: Εάν έχετε επιλέξει για τη δήλωση δεν μια νέα εφαρμογή στο δικό του μισθωτή, μπορείτε απλώς να το επικολλήσετε τις προκαθορισμένες ρυθμίσεις παραμέτρων παραπάνω τιμές που είναι - που θα σας επιτρέψει να δείτε το τρέχον δείγμα, αν και πρέπει πάντα να δημιουργείτε τη δική σας καταχώρηση για τις εφαρμογές που προορίζονταν για παραγωγής.

Στη συνέχεια, θα χρειαστεί να προσθέσετε τον κώδικα πραγματική αίτηση διακριτικού. Εισαγάγετε το παρακάτω τμήμα κώδικα μεταξύ του `search `και `renderdata `ορισμών.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Ας εξετάσουμε που λειτουργούν με διακοπή προς τα κάτω στο τα δύο κύρια μέρη.
Αυτό το δείγμα έχει σχεδιαστεί για να εργαστείτε με οποιαδήποτε μισθωτή, σε αντίθεση με είναι συνδεδεμένη με ένα συγκεκριμένο. Χρησιμοποιεί το "/ κοινές" τελικό σημείο, το οποίο επιτρέπει στο χρήστη να εισαγάγετε οποιονδήποτε λογαριασμό κατά τον έλεγχο ταυτότητας και κατευθύνει την αίτηση στο μισθωτή ανήκει.
Αυτό το πρώτο τμήμα της μεθόδου επιθεωρεί το cache ADAL για να δείτε εάν υπάρχει ήδη ένα αποθηκευμένο διακριτικό - και εάν υπάρχει, που χρησιμοποιεί το μισθωτές προήλθε για εκ νέου την προετοιμασία ADAL. Αυτό είναι απαραίτητο για να αποφύγετε την επιπλέον ερωτήσεις, όπως τη χρήση του "/ κοινές" πάντα έχει ως αποτέλεσμα ερώτησης προς το χρήστη για να εισαγάγετε ένα νέο λογαριασμό.
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Το δεύτερο τμήμα της μεθόδου εκτελεί την αίτηση proper tokewn.
Το `acquireTokenSilentAsync` μέθοδο σας ζητά να ADAL για να επιστρέψετε ενός διακριτικού για τον συγκεκριμένο πόρο χωρίς εμφάνιση οποιαδήποτε UX. Που μπορεί να συμβεί εάν το cache έχει ήδη ένα διακριτικό κατάλληλο πρόσβασης αποθηκευμένο, ή εάν υπάρχει ένα διακριτικό ανανέωση που μπορούν να χρησιμοποιηθούν για να λάβετε έναν νέο κωδικό πρόσβασης, χωρίς να shwoing οποιοδήποτε μήνυμα.
Εάν που επιχειρείτε αποτύχει, θα σας επιστρέφουν `acquireTokenAsync` -ευδιάκριτη που θα γίνεται ερώτηση στο χρήστη για τον έλεγχο ταυτότητας.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Τώρα που έχουμε το διακριτικό, να τέλος καλέσει το API Graph και να εκτελέσετε το ερώτημα αναζήτησης θέλουμε. Εισαγάγετε το παρακάτω τμήμα κώδικα ακριβώς κάτω από το `authenticate` ορισμού.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Τα αρχεία σημείο έναρξης που παρέχονται μια barebone UX για την εισαγωγή ψευδώνυμο χρήστη σε ένα πλαίσιο κειμένου. Αυτή η μέθοδος χρησιμοποιεί αυτήν την τιμή για να δημιουργήσετε ένα ερώτημα, να συνδυάσετε το με το διακριτικό πρόσβασης, στείλτε το στο γράφημα και ανάλυση των αποτελεσμάτων. Η μέθοδος renderData, υπάρχει ήδη στο αρχείο σημείο έναρξης, αναλαμβάνει για την απεικόνιση των αποτελεσμάτων.

## <a name="4-run"></a>*4. εκτέλεση*
Εφαρμογή σας είναι έτοιμοι τέλος για να εκτελέσετε! Λειτουργικό αυτό είναι πολύ απλή: όταν ξεκινά η εφαρμογή, πληκτρολογήστε στο πλαίσιο κειμένου το ψευδώνυμο του χρήστη που θέλετε να αναζητήσετε - και, στη συνέχεια, κάντε κλικ στο κουμπί. Θα σας ζητηθεί για έλεγχο ταυτότητας. Κατά την επιτυχή έλεγχο ταυτότητας και επιτυχημένη αναζήτηση, θα εμφανιστούν τα χαρακτηριστικά του έχει γίνει αναζήτηση χρήστη. Οι επόμενες εκτελείται θα εκτελέσει αναζήτηση χωρίς εμφάνιση οποιαδήποτε ερώτηση, ευχαριστήριο την παρουσία στο cache του αποκτήσει προηγουμένως διακριτικού.
Τα συγκεκριμένα βήματα για την εκτέλεση της εφαρμογής διαφέρουν ανάλογα με την πλατφόρμα.

####<a name="windows-10"></a>Windows 10:

   Tablet/Υπολογιστή:`cordova run windows --archs=x64 -- --appx=uap`

   Κινητό (απαιτεί Windows10 κινητή συσκευή συνδεδεμένο με τον Υπολογιστή):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Σημείωση__: κατά την πρώτη εκτέλεση μπορεί να σας ζητηθεί να εισέλθετε για μια άδεια χρήσης για προγραμματιστές. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [άδειας χρήσης για προγραμματιστές](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Tablet/Υπολογιστή με Windows 8.1:

   `cordova run windows`

   __Σημείωση__: κατά την πρώτη εκτέλεση μπορεί να σας ζητηθεί να εισέλθετε για μια άδεια χρήσης για προγραμματιστές. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [Προγραμματιστής άδεια χρήσης](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Για να εκτελέσετε στη συνδεδεμένη συσκευή:`cordova run windows --device -- --phone`

   Για να εκτελέσετε στην προεπιλεγμένη προσομοίωσης:`cordova emulate windows -- --phone`

   Χρήση `cordova run windows --list -- --phone` για να δείτε όλων των προορισμών διαθέσιμες και `cordova run windows --target=<target_name> -- --phone` για την εκτέλεση εφαρμογής σε συγκεκριμένες συσκευή ή προσομοίωσης (για παράδειγμα, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Για να εκτελέσετε στη συνδεδεμένη συσκευή:`cordova run android --device`

   Για να εκτελέσετε στην προεπιλεγμένη προσομοίωσης:`cordova emulate android`

   __Σημείωση__: Βεβαιωθείτε ότι έχετε δημιουργήσει παρουσία προσομοίωσης χρησιμοποιώντας τη *Διαχείριση AVD* καθώς αυτό είναι εμφάνιζε στην ενότητα *προϋποθέσεις* .

   Χρήση `cordova run android --list` για να δείτε όλων των προορισμών διαθέσιμες και `cordova run android --target=<target_name>` για την εκτέλεση εφαρμογής σε συγκεκριμένες συσκευή ή προσομοίωσης (για παράδειγμα, `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Για να εκτελέσετε στη συνδεδεμένη συσκευή:`cordova run ios --device`

   Για να εκτελέσετε στην προεπιλεγμένη προσομοίωσης:`cordova emulate ios`

   __Σημείωση__: Βεβαιωθείτε ότι έχετε `ios-sim` πακέτο εγκατασταθεί ώστε να εκτελείται σε προσομοίωσης. Ανατρέξτε στην ενότητα για *τις προϋποθέσεις* για περισσότερες λεπτομέρειες.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Χρήση `cordova run --help` για να δείτε πρόσθετες δημιουργία και εκτέλεση επιλογές.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Τώρα, μπορείτε να μετακινήσετε για πιο σύνθετες σενάρια (ΟΚ και πιο ενδιαφέρον).  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Ασφαλούς Web Node.js API με Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]

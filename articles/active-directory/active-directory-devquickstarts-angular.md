<properties
    pageTitle="Azure AD AngularJS γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή γωνιωδών JS μεμονωμένης σελίδας που ενοποιείται με το Azure AD για είσοδο στο και καλεί Azure AD προστατευμένο APIs χρησιμοποιώντας OAuth."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="securing-angularjs-single-page-apps-with-azure-ad"></a>Ασφάλιση AngularJS μίας σελίδας εφαρμογών με το Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD καθιστά απλή και άμεση για να προσθέσετε το σύμβολο στο στοιχείο Έξοδος και να διασφαλίζουν τις κλήσεις OAuth API στις εφαρμογές σας μία σελίδα.  Σας επιτρέπει να ελέγχουν την ταυτότητα χρηστών με τους λογαριασμούς υπηρεσίας καταλόγου Active Directory και κατανάλωση οποιαδήποτε τοποθεσία web API που προστατεύονται από Azure AD, όπως τα API Office 365 ή το API Azure εφαρμογή σας.

Για εφαρμογές javascript που εκτελούνται σε ένα πρόγραμμα περιήγησης, Azure AD παρέχει τη βιβλιοθήκη ελέγχου ταυτότητας Active Directory ή adal.js.  Σκοπός της Adal.js κατά τη διάρκεια ζωής της είναι ώστε να μπορείτε εύκολα για την εφαρμογή σας για να λάβετε διακριτικά πρόσβασης.  Για μια επίδειξη απλώς πόσο εύκολο είναι, εδώ θα σας θα δημιουργήσετε μια εφαρμογή AngularJS λίστα εκκρεμών εργασιών που:

- Στην εφαρμογή χρησιμοποιώντας Azure AD ως την υπηρεσία παροχής ταυτότητας, τοποθετείται ο χρήστης.
- Εμφανίζει ορισμένες πληροφορίες σχετικά με το χρήστη.
- Κλήσεις με ασφάλεια της εφαρμογής για να κάνετε λίστα API χρησιμοποιώντας φορέα διακριτικά από AAD.
- Τοποθετείται το χρήστη από την εφαρμογή.

Για να δημιουργήσετε την εφαρμογή ολοκλήρωσης εργασίας, θα πρέπει να:

2. Καταχωρήστε την εφαρμογή του Azure AD.
3. ADAL εγκατάσταση και ρύθμιση παραμέτρων του SPA.
5. Χρησιμοποιήστε ADAL για την ασφάλιση σελίδες του SPA.

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD με την οποία μπορείτε να δημιουργήσετε χρήστες και να καταχωρήσετε μια εφαρμογή του.  Εάν δεν έχετε ήδη ένα μισθωτή, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. καταχώρηση της εφαρμογής DirectorySearcher*
Για να ενεργοποιήσετε την εφαρμογή σας για να ελέγχουν την ταυτότητα χρηστών και να λάβετε τα διακριτικά, θα πρέπει πρώτα να καταχωρήσετε στο μισθωτή σας Azure AD:

-   Πραγματοποιήστε είσοδο στο την [πύλη διαχείρισης Azure](https://manage.windowsazure.com)
-   Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**
-   Επιλέξτε έναν μισθωτή στην οποία θέλετε να καταχωρήσετε την εφαρμογή.
-   Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή **Προσθήκη** στο το σχέδιο κάτω.
-   Ακολουθήστε τις οδηγίες και δημιουργήστε μια νέα **εφαρμογή Web ή/και WebAPI**.
    -   Το **όνομα** της εφαρμογής θα περιγράφουν την εφαρμογή σας στους τελικούς χρήστες.
    -   **Ανακατεύθυνση Uri** είναι θέση στην οποία AAD θα επιστρέψει διακριτικά.  Η προεπιλεγμένη θέση για αυτό το δείγμα είναι`https://localhost:44326/`
-   Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα μοναδικό **Αναγνωριστικό υπολογιστή-πελάτη**.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα **Ρύθμιση παραμέτρων** .
- Adal.js χρησιμοποιεί το διακριτικό έμμεσα ροής για να επικοινωνήσετε με το Azure AD.  Πρέπει να ενεργοποιήσετε την μη ρητή ροής για την εφαρμογή σας με:
    - Κάντε λήψη της εφαρμογής δήλωσης κάνοντας κλικ στην επιλογή **Διαχείριση δήλωσης**.
    - Ανοίξτε το δηλωτικό και εντοπίστε το `oauth2AllowImplicitFlow` την ιδιότητα. Ορίστε την τιμή σε `true`.
    - Αποθήκευση & Αποστολή τη δήλωση της εφαρμογής κάνοντας ξανά κλικ στην επιλογή **Διαχείριση δήλωσης** .

## <a name="2-install-adal--configure-the-spa"></a>*2. ADAL εγκατάσταση και ρύθμιση παραμέτρων του SPA*
Τώρα που έχετε μια εφαρμογή στο Azure AD, μπορείτε να εγκαταστήσετε adal.js και να γράψετε τον κωδικό που σχετίζονται με την ταυτότητα.

-   Ξεκινήστε προσθέτοντας adal.js στο έργο TodoSPA χρησιμοποιώντας την Κονσόλα διαχείρισης πακέτου:
  - Λήψη [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) και για να προσθέσετε το `App/Scripts/` καταλόγου έργου.
  - Λήψη [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) και για να προσθέσετε το `App/Scripts/` καταλόγου έργου.
  - Φόρτωση κάθε δέσμη ενεργειών πριν από τη λήξη της το `</body>` στο `index.html`:

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   Για το SPA παρασκηνίου για να κάνετε λίστα API για να αποδεχτείτε τα διακριτικά από το πρόγραμμα περιήγησης, υπόβαθρο χρειάζεται πληροφορίες σχετικά με την καταχώρηση εφαρμογής. Στο έργο TodoSPA, ανοίξτε `web.config`.  Αντικαταστήστε τις τιμές των στοιχείων στο το `<appSettings>` ενότητα ώστε να αντικατοπτρίζει τις τιμές που εισαγάγει στην πύλη του Azure.  Ο κώδικας θα αναφοράς αυτές τις τιμές κάθε φορά που χρησιμοποιεί ADAL.
    -   Η `ida:Tenant` είναι ο τομέας του μισθωτή του Azure AD, π.χ. contoso.onmicrosoft.com
    -   Το `ida:Audience` πρέπει να είναι το **Αναγνωριστικό υπολογιστή-πελάτη** της εφαρμογής σας που αντιγράψατε από την πύλη.

## <a name="3--use-adal-to-secure-pages-in-the-spa"></a>*3. Χρησιμοποιήστε ADAL για την ασφάλιση σελίδων σε το SPA*
Adal.js έχει δημιουργηθεί ενσωμάτωση σε AngularJS δρομολόγηση και http υπηρεσίες παροχής, η οποία σας επιτρέπει να προστατεύσετε μεμονωμένα προβολές στο SPA σας.

- Στο `App/Scripts/app.js`, μεταφορά στη λειτουργική μονάδα adal.js:

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- Μπορείτε τώρα να προετοιμάσετε το `adalProvider` με τις τιμές παραμέτρων την εγγραφή σας εφαρμογής, επίσης στο `App/Scripts/app.js`:

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- Τώρα για την ασφάλιση του `TodoList` προβολή στην εφαρμογή, μόνο μία γραμμή του κώδικα είναι απαραίτητο - `requireADLogin`.

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

Τώρα έχετε μια εφαρμογή ασφαλούς μίας σελίδας με τη δυνατότητα να εισέλθετε χρηστών και πρόβλημα φορέα διακριτικού προστατευμένο αιτήσεις για το API παρασκηνίου.  Όταν ένας χρήστης κάνει κλικ το `TodoList` σύνδεση, το adal.js θα αυτόματη ανακατεύθυνση σε Azure AD για είσοδο στην εάν είναι απαραίτητο.  Επιπλέον, adal.js θα επισυνάψει αυτόματα μια access_token σε οποιαδήποτε ajax προσκλήσεις που αποστέλλονται σε παρασκηνίου της εφαρμογής.  Τα παραπάνω είναι τα κενά ελάχιστα απαραίτητα για να δημιουργήσετε μια SPA με adal.js - αλλά υπάρχουν πολλές άλλες δυνατότητες που είναι χρήσιμη σε ιαματικές πηγές:

- Για να ρητά θέμα είσοδος και έξοδος αιτήσεις μπορείτε να ορίσετε συναρτήσεις σε σας ελεγκτές που κλήση adal.js.  In `App/Scripts/homeCtrl.js`:

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- Μπορεί επίσης να θέλετε να παρουσιάσετε πληροφορίες χρήστη στο περιβάλλον εργασίας Χρήστη της εφαρμογής.  Η υπηρεσία adal έχει ήδη προστεθεί για να το `userDataCtrl` ελεγκτή, ώστε να μπορείτε να αποκτήσετε πρόσβαση το `userInfo` αντικειμένου στη συσχετισμένη προβολή, `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- Υπάρχουν επίσης πολλά σενάρια στο οποίο θέλετε να γνωρίζετε εάν ο χρήστης είναι συνδεδεμένος ή όχι.  Μπορείτε επίσης να χρησιμοποιήσετε το `userInfo` αντικειμένου για τη συγκέντρωση αυτές τις πληροφορίες.  Για παράδειγμα, στο `index.html` μπορείτε να εμφανίσετε το "Σύνδεση" ή "Αποσύνδεσης" κουμπί που βασίζεται στην κατάσταση ελέγχου ταυτότητας:

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

Συγχαρητήρια! Το Azure AD ενσωματωμένη εφαρμογή μία μόνο σελίδα είναι τώρα ολοκληρωμένο.  Το μπορούν να ελέγχουν την ταυτότητα χρηστών, με ασφάλεια κλήσεων του παρασκηνίου χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.  Εκτελέστε το για να κάνετε λίστα SPA και συνδεθείτε με έναν από αυτούς τους χρήστες.  Προσθέστε εργασίες στους χρήστες να λίστας, πραγματοποιήστε έξοδο και συνδεθείτε ξανά.

Adal.js διευκολύνει να ενσωματώσετε όλες αυτές τις συνήθεις δυνατότητες ταυτότητας στην εφαρμογή σας.  Το αναλαμβάνει όλες τις εργασίες dirty για εσάς: Διαχείριση cache, η υποστήριξη πρωτοκόλλου διακριτικό, παρουσίαση στο χρήστη με μια σύνδεση περιβάλλοντος εργασίας Χρήστη, η ανανέωση έχει λήξει διακριτικά και πολλά άλλα.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) παρέχεται [εδώ](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).  Τώρα, μπορείτε να μετακινήσετε επιπλέον σενάρια.  Εάν θέλετε, μπορείτε να δοκιμάσετε:

[Κλήση ενός API Web CORS από ένα SPA >>](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]

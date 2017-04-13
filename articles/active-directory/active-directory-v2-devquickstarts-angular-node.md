<properties
    pageTitle="Azure AD v2.0 AngularJS γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή γωνιωδών JS μεμονωμένης σελίδας που πραγματοποιεί είσοδο εταιρικό ή σχολικό λογαριασμοί και οι χρήστες με δύο προσωπικοί λογαριασμοί Microsoft."
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


# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a>Προσθήκη εισόδου σε μια εφαρμογή μεμονωμένη σελίδα AngularJS - NodeJS

Σε αυτό το άρθρο θα σας θα προσθέσετε εισέλθετε με λογαριασμούς Microsoft που υποστηρίζεται για μια εφαρμογή AngularJS χρησιμοποιώντας το τελικό σημείο v2.0 Azure Active Directory. το τελικό σημείο v2.0 σάς επιτρέπουν να εκτελέσετε μια μεμονωμένη ενοποίησης στην εφαρμογή και τον έλεγχο ταυτότητας χρηστών με προσωπικές και εταιρικό/σχολικό λογαριασμούς.

Αυτό το δείγμα είναι μια απλή λίστα εκκρεμών εργασιών μεμονωμένη σελίδα εφαρμογή που αποθηκεύει εργασίες σε έναν υπολογιστή στο παρασκήνιο REST API, γραμμένο σε NodeJS και ασφαλές χρησιμοποιώντας τα διακριτικά φορέα OAuth από το Azure AD.  Η εφαρμογή AngularJS θα χρησιμοποιήσει μας βιβλιοθήκη ελέγχου ταυτότητας JavaScript [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) Άνοιγμα αρχείου προέλευσης για να χειριστείτε τα ολόκληρο διαδικασία εισόδου σε και απόκτηση διακριτικά για την κλήση το REST API.  Το ίδιο μοτίβο μπορούν να εφαρμοστούν για τον έλεγχο ταυτότητας προς άλλες REST API, όπως το [Microsoft Graph](https://graph.microsoft.com) ή τα API διαχείρισης πόρων Azure.

> [AZURE.NOTE]
    Δεν όλα τα σενάρια Azure Active Directory και δυνατότητες που υποστηρίζονται από το τελικό σημείο v2.0.  Για να καθορίσετε εάν θα πρέπει να χρησιμοποιήσετε το τελικό σημείο v2.0, διαβάστε σχετικά με [τους περιορισμούς v2.0](active-directory-v2-limitations.md).

## <a name="download"></a>Λήψη

Για να ξεκινήσετε, θα πρέπει να κάνετε λήψη και εγκατάσταση του [node.js](https://nodejs.org).  Στη συνέχεια, μπορείτε να αντιγράψετε ή να [κάνετε λήψη](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) μιας εφαρμογής σκελετό:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

Η εφαρμογή σκελετό περιλαμβάνει όλα τα κώδικα στερεότυπο κείμενο για μια απλή εφαρμογή AngularJS, αλλά δεν διαθέτει όλα τα τμήματα που σχετίζονται με την ταυτότητα.  Εάν δεν θέλετε να παρακολουθήσετε κατά μήκος, μπορείτε να κλωνοποίηση αντί για αυτό ή [λήψη](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) ολοκληρωμένο δείγμα.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a>Καταχώρηση εφαρμογής

Πρώτα, δημιουργήστε μια εφαρμογή στην [Πύλη καταχώρηση εφαρμογής](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)ή ακολουθήστε αυτά τα [λεπτομερή βήματα](active-directory-v2-app-registration.md).  Βεβαιωθείτε ότι:

- Προσθέστε την πλατφόρμα για την εφαρμογή σας στο **Web** .
- Εισαγάγετε το σωστό **Ανακατεύθυνση URI**. Η προεπιλογή για αυτό το δείγμα είναι `http://localhost:8080`.
- Αφήστε το πλαίσιο ελέγχου **Να επιτρέπεται έμμεσα ροής** με δυνατότητα. 

Αντιγραφή προς τα κάτω το **Αναγνωριστικό εφαρμογής** που έχει αντιστοιχιστεί σε εφαρμογή σας, θα χρειαστεί το λίγο. 

## <a name="install-adaljs"></a>Εγκατάσταση adal.js
Για να ξεκινήσετε, μεταβείτε στο έργο που έχουν ληφθεί και εγκατάσταση του adal.js.  Εάν έχετε εγκαταστήσει [bower](http://bower.io/) , απλώς μπορείτε να εκτελέσετε αυτή την εντολή.  Για οποιαδήποτε έκδοση διαφορές εξάρτηση, απλώς επιλέξτε τη νεότερη έκδοση.
```
bower install adal-angular#experimental
```

Εναλλακτικά, μπορείτε να με μη αυτόματο τρόπο κάνετε λήψη [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) και [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Προσθήκη και τα δύο αρχεία για να το `app/lib/adal-angular-experimental/dist` καταλόγου.

Τώρα, ανοίξτε το έργο στο πρόγραμμα επεξεργασίας κειμένου Αγαπημένα και φόρτωση adal.js στο τέλος της στο σώμα σελίδας:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a>Ρυθμίστε το REST API

Ενώ θα σας ρυθμίζετε πράγματα, σας επιτρέπει να λάβετε εργάζεστε REST API παρασκηνίου.  Σε μια γραμμή εντολών, πρέπει να εγκαταστήσετε τα απαραίτητα πακέτα, εκτελώντας (βεβαιωθείτε ότι βρίσκεστε στον κατάλογο ανώτατου επιπέδου του έργου):

```
npm install
```

Ανοίξτε το τώρα `config.js` και να αντικαταστήσετε το `audience` τιμή:

```js
exports.creds = {
     
     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',
     
     ...
}
```

Το REST API θα χρησιμοποιήσετε αυτή την τιμή για να επικυρώσει τα διακριτικά που λαμβάνει από την εφαρμογή γωνιωδών σε αιτήσεις AJAX.  Σημειώστε ότι αυτό το απλό REST API αποθηκεύει δεδομένα στη μνήμη - ώστε κάθε φορά για να διακόψετε το διακομιστή, θα χάσετε όλες τις εργασίες που δημιουργήσατε προηγουμένως.

Που είναι πάντα θα κάνουμε Διαθέστε συζητάτε πώς λειτουργεί το REST API.  Μην διστάσεις να ψάξετε στον κώδικα, αλλά εάν θέλετε να μάθετε περισσότερα σχετικά με την ασφάλεια web APIs με Azure AD, ανατρέξτε [σε αυτό το άρθρο](active-directory-v2-devquickstarts-node-api.md). 

## <a name="sign-users-in"></a>Σύμβολο χρηστών στο
Χρόνος για να γράψετε ορισμένες κώδικα ταυτότητα.  Ενδέχεται να έχετε ήδη διαπιστώσατε adal.js που περιέχει μια υπηρεσία παροχής AngularJS, το οποίο αναπαράγεται όμορφα με γωνιωδών δρομολόγησης μηχανισμούς.  Ξεκινήστε προσθέτοντας τη λειτουργική μονάδα adal για την εφαρμογή:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Μπορείτε τώρα να προετοιμάσετε το `adalProvider` με το Αναγνωριστικό εφαρμογής:

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

Εξαιρετική, τώρα adal.js διαθέτει όλες τις πληροφορίες που χρειάζονται για την ασφάλεια της εφαρμογής σας και εισέλθετε χρήστες.  Για να επιβάλετε είσοδος για μια συγκεκριμένη διαδρομή στην εφαρμογή, μόνο που χρειάζεται να είναι μία γραμμή του κώδικα:

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

Τώρα όταν ένας χρήστης κάνει κλικ το `TodoList` σύνδεση, το adal.js θα αυτόματη ανακατεύθυνση σε Azure AD για είσοδο εάν είναι απαραίτητο.  Μπορείτε να στείλετε επίσης ρητά αιτήσεις εισόδου και sign-out καλώντας adal.js σε ελεγκτές σας:

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## <a name="display-user-info"></a>Εμφάνιση πληροφοριών χρήστη
Τώρα που ο χρήστης είναι συνδεδεμένος, πιθανώς θα χρειαστεί για να αποκτήσετε πρόσβαση σε δεδομένα ελέγχου ταυτότητας συνδεδεμένοι στο του χρήστη στην εφαρμογή σας.  Adal.js εκθέτει αυτές τις πληροφορίες στο το `userInfo` αντικειμένου.  Για να αποκτήσετε πρόσβαση σε αυτό το αντικείμενο σε μια προβολή, προσθέστε πρώτα adal.js στο πεδίο ριζικό κατάλογο του αντίστοιχου ελεγκτή:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Στη συνέχεια, μπορείτε να αντιμετωπίσετε απευθείας το `userInfo` αντικείμενο στην προβολή σας: 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

Μπορείτε επίσης να χρησιμοποιήσετε το `userInfo` αντικειμένου για να καθορίσετε εάν ο χρήστης είναι συνδεδεμένος ή όχι.

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a>Καλέστε το REST API
Τέλος, είναι καιρός να λάβει ορισμένες διακριτικά και καλέστε το REST API για τη δημιουργία, ανάγνωση, ενημέρωση και διαγραφή εργασιών.  Καλά προβλέψει ποια;  Δεν χρειάζεται να κάνετε *τίποτα*.  Adal.js θα εκτελέσει αυτόματα γρήγορα, προσωρινή αποθήκευση και ανανέωση διακριτικά.  Αυτό θα επίσης να χειριστείτε επισύναψη αυτά τα διακριτικά σε εξερχόμενες αιτήσεις AJAX που μπορείτε να στείλετε το REST API.  

Πώς ακριβώς λειτουργεί αυτή η δυνατότητα; Είναι όλα ευχαριστήριο η μαγεία της [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), που επιτρέπει την adal.js για τη μετατροπή των εξερχόμενων και τα εισερχόμενα μηνύματα http.  Επιπλέον, adal.js προϋποθέτει ότι τυχόν αιτήσεις αποστολή στον ίδιο τομέα ως παράθυρο πρέπει να χρησιμοποιήσετε τα διακριτικά που προορίζονται για το ίδιο Αναγνωριστικό εφαρμογής με την εφαρμογή AngularJS.  Αυτός είναι ο λόγος χρησιμοποιήσαμε το ίδιο Αναγνωριστικό εφαρμογής στο τόσο την εφαρμογή γωνιωδών και το REST API NodeJS.  Φυσικά, μπορείτε να παρακάμψετε αυτήν τη συμπεριφορά και να υποδείξετε adal.js για να λάβετε τα διακριτικά για άλλες REST API, εάν είναι απαραίτητο - αλλά για αυτό το απλό σενάριο θα κάνετε τις προεπιλογές.

Ακολουθεί ένα απόκομμα που εμφανίζει πόσο εύκολο είναι να στείλετε προσκλήσεις με διακριτικά φορέα από το Azure AD:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Συγχαρητήρια!  Εφαρμογή σας Azure AD ενσωματωμένη μεμονωμένη σελίδα είναι τώρα ολοκληρωμένο.  Προχωρήστε, λαμβάνουν φιόγκο.  Το μπορούν να ελέγχουν την ταυτότητα χρηστών, με ασφάλεια κλήσεων του παρασκηνίου REST API με χρήση της σύνδεσης OpenID και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Έξοδος από το πλαίσιο, υποστηρίζει οποιοσδήποτε χρήστης με έναν προσωπικό λογαριασμό Microsoft ή έναν εταιρικό/σχολικό λογαριασμό από το Azure AD.  Δοκιμάστε την εφαρμογή, εκτελώντας:

```
node server.js
```

Σε ένα πρόγραμμα περιήγησης, μεταβείτε στο `http://localhost:8080`.  Πραγματοποιήστε είσοδο με έναν προσωπικό λογαριασμό Microsoft ή έναν εταιρικό/σχολικό λογαριασμό.  Προσθήκη εργασιών στη λίστα εκκρεμών εργασιών του χρήστη και αποσύνδεση.  Προσπαθήστε να συνδεθείτε με τον άλλο τύπο λογαριασμού. Εάν χρειάζεστε ένα μισθωτή του Azure AD για τη δημιουργία χρηστών εταιρικό/σχολικό, [Μάθετε πώς μπορείτε να αποκτήσετε εδώ](active-directory-howto-tenant.md) (είναι δωρεάν).

Για να συνεχίσετε να μάθετε σχετικά με το το τελικό σημείο v2.0, επιστρέψτε στο μας [Οδηγό για προγραμματιστές v2.0](active-directory-appmodel-v2-overview.md).  Για επιπλέον πόρους, ανατρέξτε στο θέμα:

- [Azure-δείγματα GitHub >>](https://github.com/Azure-Samples)
- [Azure AD σε στοίβα υπερχείλιση >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD τεκμηρίωση στην [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Λήψη ενημερώσεων ασφαλείας για τα προϊόντα μας

Συνιστάται να λαμβάνετε ειδοποιήσεις όταν προκύπτουν περιστατικά ασφαλείας, αν επισκεφθείτε την τοποθεσία [αυτήν τη σελίδα](https://technet.microsoft.com/security/dd252948) και εγγραφή συμβουλευτική ειδοποιήσεων ασφαλείας.
